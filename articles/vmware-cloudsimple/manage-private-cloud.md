---
title: Azure VMware 솔루션 (AVS) 사설 클라우드 관리
description: AVS 사설 클라우드 리소스 및 활동을 관리 하는 데 사용할 수 있는 기능에 대해 설명 합니다.
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/10/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 47bf2251f71204b99245c1a9d55ef87157c41dd8
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77014830"
---
# <a name="manage-avs-private-cloud-resources-and-activities"></a>AVS 사설 클라우드 리소스 및 활동 관리

Avs 사설 클라우드는 AVS 포털에서 관리 됩니다. Avs 포털에서 상태, 사용 가능한 리소스, AVS 사설 클라우드의 작업 및 기타 설정을 확인 합니다.

## <a name="sign-in-to-azure"></a>Azure에 로그인

[https://portal.azure.com](https://portal.azure.com)에서 Azure Portal에 로그인합니다.

## <a name="access-the-avs-portal"></a>AVS 포털 액세스

[AVS 포털](access-cloudsimple-portal.md)에 액세스 합니다.

## <a name="view-the-list-of-avs-private-clouds"></a>AVS 사설 클라우드 목록 보기

**리소스** 페이지의 **avs 사설 클라우드** 탭은 구독의 모든 avs 사설 클라우드를 나열 합니다. 정보에는 이름, vSphere 클러스터의 수, 위치, AVS 사설 클라우드의 현재 상태 및 리소스 정보가 포함 됩니다.

![AVS 사설 클라우드 페이지](media/manage-private-cloud.png)

추가 정보 및 작업을 위해 AVS 사설 클라우드를 선택 합니다.

## <a name="avs-private-cloud-summary"></a>AVS 사설 클라우드 요약

선택한 AVS 사설 클라우드의 포괄적인 요약을 확인 합니다. 요약 페이지에는 AVS 사설 클라우드에 배포 된 DNS 서버가 포함 됩니다. 온-프레미스 DNS 서버에서 AVS 사설 클라우드 DNS 서버로의 DNS 전달 기능을 설정할 수 있습니다. DNS 전달에 대 한 자세한 내용은 [온-프레미스에서 AVS 사설 클라우드 vCenter에 대 한 이름 확인을 위한 Dns 구성](https://docs.azure.cloudsimple.com/on-premises-dns-setup/)을 참조 하세요.

![AVS 사설 클라우드 요약](media/private-cloud-summary.png)

### <a name="available-actions"></a>사용 가능한 작업

* [VSphere 클라이언트를 시작](https://docs.azure.cloudsimple.com/vsphere-access/)합니다. 이 AVS 사설 클라우드의 vCenter에 액세스 합니다.
* [노드를 구매](create-nodes.md)합니다. 이 AVS 사설 클라우드에 노드를 추가 합니다.
* 를 [확장](expand-private-cloud.md)합니다. 이 AVS 사설 클라우드에 노드를 추가 합니다.
* **새로 고침**. 이 페이지의 정보를 업데이트 합니다.
* **삭제**. 언제 든 지 AVS 사설 클라우드를 삭제할 수 있습니다. **삭제 하기 전에 모든 시스템 및 데이터를 백업 했는지 확인 합니다.** AVS 사설 클라우드를 삭제 하면 모든 Vm, vCenter 구성 및 데이터가 삭제 됩니다. 선택한 AVS 사설 클라우드의 요약 섹션에서 **삭제** 를 클릭 합니다. 삭제 후 모든 AVS 사설 클라우드 데이터는 안전 하 고 호환 되는 지우기 프로세스에서 지워집니다.
* [VSphere 대 한 권한을 변경](escalate-private-cloud-privileges.md)합니다. 이 AVS 사설 클라우드에 대 한 권한을 에스컬레이션 합니다.

## <a name="avs-private-cloud-vlanssubnets"></a>AVS 사설 클라우드 VLAN/서브넷

선택한 AVS 사설 클라우드에 대해 정의 된 Vlan/서브넷 목록을 봅니다. 이 목록에는 AVS 사설 클라우드를 만들 때 만든 관리 Vlan/서브넷이 포함 됩니다.

![AVS 사설 클라우드-Vlan/서브넷](media/private-cloud-vlans-subnets.png) 

### <a name="available-actions"></a>사용 가능한 작업

* [Vlan/서브넷을 추가](https://docs.azure.cloudsimple.com/create-vlan-subnet/)합니다. 이 AVS 사설 클라우드에 VLAN/하위 집합을 추가 합니다.

다음 작업에 대해 VLAN/서브넷을 선택 하십시오.
* [방화벽 테이블을 연결](https://docs.azure.cloudsimple.com/firewall/)합니다. 이 AVS 사설 클라우드에 방화벽 테이블을 연결 합니다.
* **편집**
* **삭제** (사용자 정의 Vlan/서브넷만)

## <a name="avs-private-cloud-activity"></a>AVS 사설 클라우드 작업

선택한 AVS 사설 클라우드에 대 한 다음 정보를 확인 합니다. 활동 정보는 선택한 AVS 사설 클라우드의 모든 활동에 대 한 필터링 된 목록입니다. 이 페이지에는 최대 25 개의 최근 활동이 표시 됩니다.

* 최근 경고
* 최근 이벤트
* 최근 작업
* 최근 감사

![AVS 사설 클라우드-작업](media/private-cloud-activity.png)

## <a name="cloud-racks"></a>클라우드 랙

클라우드 랙은 AVS 사설 클라우드의 빌딩 블록입니다. 각 랙은 용량 단위를 제공 합니다. Avs는 AVS 사설 클라우드를 만들거나 확장할 때 선택 사항에 따라 클라우드 랙을 자동으로 구성 합니다. 각각에 할당 된 AVS 사설 클라우드를 포함 하 여 클라우드 랙의 전체 목록을 봅니다.

![AVS 사설 클라우드-클라우드 랙](media/private-cloud-cloudracks.png)

## <a name="vsphere-management-network"></a>vSphere 관리 네트워크

현재 AVS 사설 클라우드에 구성 된 VMware 관리 리소스 및 가상 컴퓨터의 목록입니다. 정보에는 소프트웨어 버전, FQDN (정규화 된 도메인 이름) 및 리소스의 IP 주소가 포함 됩니다.

![AVS 사설 클라우드-vSphere 관리 네트워크](media/private-cloud-vsphere-management-network.png)

## <a name="next-steps"></a>다음 단계

* [Azure에서 VMware Vm 사용](quickstart-create-vmware-virtual-machine.md)
* [AVS 사설 클라우드에](cloudsimple-private-cloud.md) 대 한 자세한 정보