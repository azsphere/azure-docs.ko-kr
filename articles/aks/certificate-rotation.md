---
title: Azure Kubernetes 서비스 (AKS)에서 인증서 회전
description: AKS (Azure Kubernetes Service) 클러스터에서 인증서를 회전 하는 방법에 대해 알아봅니다.
services: container-service
author: zr-msft
ms.service: container-service
ms.topic: article
ms.date: 11/15/2019
ms.author: zarhoads
ms.openlocfilehash: 9c2da82034a3742f789c736d8c0410f005f20edb
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75422306"
---
# <a name="rotate-certificates-in-azure-kubernetes-service-aks"></a>Azure Kubernetes 서비스 (AKS)에서 인증서 회전

AKS (Azure Kubernetes Service)에서는 많은 구성 요소를 인증 하기 위해 인증서를 사용 합니다. 보안 또는 정책 상의 이유로 이러한 인증서를 정기적으로 회전 해야 할 수도 있습니다. 예를 들어 90 일 마다 모든 인증서를 회전 하는 정책이 있을 수 있습니다.

이 문서에서는 AKS 클러스터에서 인증서를 회전 하는 방법을 보여 줍니다.

## <a name="before-you-begin"></a>시작하기 전에

이 문서에서는 Azure CLI 버전 2.0.77 이상을 실행 해야 합니다. `az --version`을 실행하여 버전을 찾습니다. 설치 또는 업그레이드해야 하는 경우 [Azure CLI 설치][azure-cli-install]를 참조하세요.

## <a name="aks-certificates-certificate-authorities-and-service-accounts"></a>인증서, 인증 기관 및 서비스 계정 AKS

AKS은 다음 인증서, 인증 기관 및 서비스 계정을 생성 하 고 사용 합니다.

* AKS API 서버는 클러스터 CA 라고 하는 CA (인증 기관)를 만듭니다.
* API 서버에는 API 서버에서 kubelets로 단방향 통신용 인증서를 서명 하는 클러스터 CA가 있습니다.
* 또한 각 kubelet는 kubelet에서 API 서버로 통신 하기 위해 클러스터 CA에 의해 서명 된 CSR (인증서 서명 요청)를 만듭니다.
* Etcd 키 값 저장소에는 etcd에서 API server로 통신 하기 위해 클러스터 CA에서 서명한 인증서가 있습니다.
* Etcd 키 값 저장소는 인증서를 서명 하는 CA를 만들어 AKS 클러스터의 etcd 복제본 간에 데이터 복제를 인증 하 고 권한을 부여 합니다.
* API 집계는 클러스터 CA를 사용 하 여 Azure에 대 한 Open Service Broker와 같은 다른 Api와 통신 하기 위한 인증서를 발급 합니다. API 집계에는 해당 인증서를 발급 하기 위한 자체 CA가 있을 수 있지만 현재는 클러스터 CA를 사용 합니다.
* 각 노드는 클러스터 CA에서 서명 하는 SA (서비스 계정) 토큰을 사용 합니다.
* `kubectl` 클라이언트에는 AKS 클러스터와 통신 하기 위한 인증서가 있습니다.

> [!NOTE]
> 3 월 2019 일 이전에 만든 AKS 클러스터에는 2 년 후에 만료 되는 인증서가 있습니다. 3 월 2019 일 이후에 생성 된 클러스터 또는 해당 인증서가 회전 된 클러스터에는 30 년 후에 만료 되는 인증서가 있습니다. 클러스터가 생성 된 시간을 확인 하려면 `kubectl get nodes`를 사용 하 여 노드 풀의 *기간* 을 확인 합니다.
> 
> 또한 클러스터 인증서의 만료 날짜를 확인할 수 있습니다. 예를 들어 다음 명령은 *myAKSCluster* 클러스터에 대 한 인증서 세부 정보를 표시 합니다.
> ```console
> kubectl config view --raw -o jsonpath='{.clusters[?(@.name == "myAKSCluster")].cluster.certificate-authority-data}' | base64 -d > my-cert.crt
> openssl x509 -in my-cert.crt -text
> ```

## <a name="rotate-your-cluster-certificates"></a>클러스터 인증서 회전

> [!WARNING]
> `az aks rotate-certs`를 사용 하 여 인증서를 회전 하면 AKS 클러스터에 대 한 최대 30 분의 가동 중지 시간이 발생할 수 있습니다.

[Az aks get-help][az-aks-get-credentials] 를 사용 하 여 aks 클러스터에 로그인 합니다. 이 명령은 로컬 컴퓨터에 `kubectl` 클라이언트 인증서를 다운로드 하 여 구성 합니다.

```console
az aks get-credentials -g $RESOURCE_GROUP_NAME -n $CLUSTER_NAME
```

`az aks rotate-certs`를 사용 하 여 클러스터의 모든 인증서, Ca 및 SAs를 회전 합니다.

```console
az aks rotate-certs -g $RESOURCE_GROUP_NAME -n $CLUSTER_NAME
```

> [!IMPORTANT]
> `az aks rotate-certs`를 완료 하는 데 최대 30 분 정도 걸릴 수 있습니다. 완료 하기 전에 명령이 실패 하는 경우 `az aks show`를 사용 하 여 클러스터의 상태가 *인증서 회전*인지 확인 합니다. 클러스터가 실패 상태인 경우 `az aks rotate-certs`를 다시 실행 하 여 인증서를 다시 회전 합니다.

`kubectl` 명령을 실행 하 여 이전 인증서가 더 이상 유효 하지 않은지 확인 합니다. `kubectl`에서 사용 하는 인증서를 업데이트 하지 않았으므로 오류가 표시 됩니다.  예:

```console
$ kubectl get no
Unable to connect to the server: x509: certificate signed by unknown authority (possibly because of "crypto/rsa: verification error" while trying to verify candidate authority certificate "ca")
```

`az aks get-credentials`를 실행 하 여 `kubectl`에서 사용 하는 인증서를 업데이트 합니다.

```console
az aks get-credentials -g $RESOURCE_GROUP_NAME -n $CLUSTER_NAME --overwrite-existing
```

`kubectl` 명령을 실행 하 여 인증서가 업데이트 되었는지 확인 합니다. 그러면이 작업이 성공적으로 수행 됩니다. 예:

```console
kubectl get no
```

## <a name="next-steps"></a>다음 단계

이 문서에서는 클러스터의 인증서, Ca 및 SAs를 자동으로 회전 하는 방법을 살펴보았습니다. AKS 보안 모범 사례에 대 한 자세한 내용은 [Azure Kubernetes 서비스 (AKS)의 클러스터 보안 및 업그레이드에 대 한 모범 사례][aks-best-practices-security-upgrades] 를 참조 하세요.


[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[aks-best-practices-security-upgrades]: operator-best-practices-cluster-security.md
