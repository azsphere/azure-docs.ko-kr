---
title: 기존 Service Fabric 클러스터에서 관리 id 지원 구성
description: 기존 Azure Service Fabric 클러스터에서 관리 id 지원을 사용 하도록 설정 하는 방법은 다음과 같습니다.
ms.topic: article
ms.date: 12/09/2019
ms.custom: sfrev
ms.openlocfilehash: cb6e4ab00afd80cba41881e46296f7046a905919
ms.sourcegitcommit: fa6fe765e08aa2e015f2f8dbc2445664d63cc591
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/01/2020
ms.locfileid: "76934956"
---
# <a name="configure-managed-identity-support-in-an-existing-service-fabric-cluster-preview"></a>기존 Service Fabric 클러스터에서 관리 되는 id 지원 구성 (미리 보기)

Service Fabric 응용 프로그램에서 [Azure 리소스에 관리 되는 id](../active-directory/managed-identities-azure-resources/overview.md) 를 사용 하려면 먼저 클러스터에서 *관리 되는 id 토큰 서비스* 를 사용 하도록 설정 합니다. 이 서비스는 관리 되는 id를 사용 하 여 Service Fabric 응용 프로그램을 인증 하 고 사용자 대신 액세스 토큰을 가져오는 일을 담당 합니다. 서비스를 사용 하도록 설정 하면 왼쪽 창에 있는 **시스템** 섹션의 이름 **Fabric:/System/ManagedIdentityTokenService**에서 실행 되는 Service Fabric Explorer에서 해당 서비스를 볼 수 있습니다.

> [!NOTE]
> **관리 되는 Id 토큰 서비스**를 사용 하려면 Service Fabric runtime version 6.5.658.9590 이상이 필요 합니다.  
>
> 클러스터 리소스를 열고 **Essentials** 섹션에서 **Service Fabric 버전** 속성을 확인 하 여 Azure Portal에서 클러스터의 Service Fabric 버전을 찾을 수 있습니다.
>
> 클러스터가 **수동** 업그레이드 모드에 있는 경우 먼저 6.5.658.9590 이상으로 업그레이드 해야 합니다.

## <a name="enable-managed-identity-token-service-in-an-existing-cluster"></a>기존 클러스터에서 *관리 되는 Id 토큰 서비스* 사용

기존 클러스터에서 관리 되는 Id 토큰 서비스를 사용 하도록 설정 하려면 두 가지 변경 사항을 지정 하 여 클러스터 업그레이드를 시작 해야 합니다. (1) 관리 되는 Id 토큰 서비스를 사용 하도록 설정 하 고, (2) 각 노드를 다시 시작 하도록 요청 합니다. 먼저 클러스터 Azure Resource Manager 템플릿에 다음 코드 조각을 추가 합니다.

```json
"fabricSettings": [
    {
        "name": "ManagedIdentityTokenService",
        "parameters": [
            {
                "name": "IsEnabled",
                "value": "true"
            }
        ]
    }
]
```

변경 내용을 적용 하려면 업그레이드가 클러스터를 진행 하면서 각 노드에서 Service Fabric 런타임의 강제 다시 시작을 지정 하도록 업그레이드 정책을 변경 해야 합니다. 이렇게 다시 시작 하면 새로 활성화 된 시스템 서비스가 각 노드에서 시작 되 고 실행 됩니다. 아래 코드 조각에서 `forceRestart`은 필수 설정입니다. 나머지 설정에 대 한 기존 값을 사용 합니다.  

```json
"upgradeDescription": {
    "forceRestart": true,
    "healthCheckRetryTimeout": "00:45:00",
    "healthCheckStableDuration": "00:05:00",
    "healthCheckWaitDuration": "00:05:00",
    "upgradeDomainTimeout": "02:00:00",
    "upgradeReplicaSetCheckTimeout": "1.00:00:00",
    "upgradeTimeout": "12:00:00"
}
```

> [!NOTE]
> 업그레이드가 성공적으로 완료 되 면 후속 업그레이드의 영향을 최소화 하기 위해 `forceRestart` 설정을 롤백해야 합니다. 

## <a name="errors-and-troubleshooting"></a>오류 및 문제 해결

다음 메시지와 함께 배포가 실패 하는 경우 클러스터가 충분 한 Service Fabric 버전에서 실행 되 고 있지 않음을 의미 합니다.

```json
{
    "code": "ParameterNotAllowed",
    "message": "Section 'ManagedIdentityTokenService' and Parameter 'IsEnabled' is not allowed."
}
```

## <a name="next-steps"></a>다음 단계
* [시스템 할당 관리 id를 사용 하 여 Azure Service Fabric 응용 프로그램 배포](./how-to-deploy-service-fabric-application-system-assigned-managed-identity.md)
* [사용자 할당 관리 id를 사용 하 여 Azure Service Fabric 응용 프로그램 배포](./how-to-deploy-service-fabric-application-user-assigned-managed-identity.md)
* [서비스 코드에서 Service Fabric 응용 프로그램의 관리 되는 id 활용](./how-to-managed-identity-service-fabric-app-code.md)
* [Azure Service Fabric 응용 프로그램에 다른 Azure 리소스에 대 한 액세스 권한 부여](./how-to-grant-access-other-resources.md)