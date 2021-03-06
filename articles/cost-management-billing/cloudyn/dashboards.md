---
title: Azure의 Cloudyn 대시보드에서 주요 메트릭 보기 | Microsoft Docs
description: 이 문서에서는 Cloudyn의 대시보드를 사용하여 주요 메트릭을 보는 방법을 설명합니다.
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 01/24/2020
ms.topic: conceptual
ms.service: cost-management-billing
ms.reviewer: vitavor
ms.custom: seodec18
ms.openlocfilehash: 78af8f2541eb0b28d75be89612d158c889261adc
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/31/2020
ms.locfileid: "76770123"
---
# <a name="view-key-cost-metrics-with-dashboards"></a>대시보드를 사용하여 주요 비용 메트릭 보기

Cloudyn의 대시보드에서는 개요 수준의 보고서를 확인할 수 있습니다. 대시보드를 사용하면 주요 비용 메트릭을 단일 보기에서 확인할 수 있습니다. 또한 비즈니스 추세 하이라이트도 제공되므로, 중요한 비즈니스 의사 결정을 내리는 데 활용할 수 있습니다.

이러한 차이는 엔터티 액세스 수준에 의해 결정됩니다.

- 재무 담당자
- 애플리케이션 또는 프로젝트 소유자
- DevOps 엔지니어
- 경영진

대시보드는 여러 위젯으로 구성되며, 각 위젯은 기본적으로 보고서 썸네일입니다. 위젯을 클릭하면 보고서가 열립니다. 보고서를 사용자 지정하면 보고서가 내 보고서에 저장되고 대시보드에 추가됩니다.

대시보드 버전은 MSP(Management), Enterprise 및 Premium Cloudyn 사용자에 따라 다릅니다. 차이는 엔터티 액세스 레벨에 의해 결정됩니다. 액세스 수준에 대한 자세한 내용은 [엔터티 액세스 수준](tutorial-user-access.md#entity-access-levels)을 참조하세요.

대시보드 가용성은 대시보드를 볼 때 사용되는 클라우드 서비스 공급자 계정의 유형에 따라 다릅니다. 사용 가능한 정보 및 Cloudyn에서 수집하는 정보의 유형에 따라 대시보드의 보고서 내용이 달라집니다. 예를 들어, AWS 계정이 없는 경우 S3 Tracker 대시보드가 표시되지 않습니다. 마찬가지로, Azure Resource Manager에서 Cloudyn에 액세스할 수 있도록 설정하지 않는 경우 Optimizer(최적화 프로그램) 대시보드 위젯에 Azure 특정 정보가 표시되지 않습니다.

미리 만들어진 여러 대시보드 중 선택하여 사용하거나, 사용자 지정 보고서를 갖춘 자체 대시보드를 직접 만들 수도 있습니다. Cloudyn 보고서에 익숙하지 않은 경우 [Cloudyn 보고서 사용](use-reports.md)을 참조하세요.

## <a name="create-a-custom-dashboard"></a>사용자 지정 대시보드 만들기

사용자 지정 대시보드를 빠르게 시작하려면 기존의 대시보드를 복제하여 해당 속성을 활용하면 됩니다. 그런 다음, 필요에 맞게 수정할 수 있습니다. 복사할 대시보드에서 **다른 이름으로 저장**을 클릭합니다. 사용자 지정 대시보드만 복제할 수 있으며, Cloudyn에 포함된 대시보드는 복제할 수 없습니다.

사용자 지정 대시보드를 만들려면 다음을 수행합니다.

1. 홈페이지에서 **새로 추가 +** 를 클릭합니다. 내 대시보드 페이지가 표시됩니다.  
    ![새 보고서를 추가한 내 대시보드 페이지](./media/dashboards/my-dashboard.png)
2. **새 보고서 추가**를 클릭합니다. [보고서 추가] 상자가 표시됩니다.
3. 대시보드 위젯에 추가할 보고서를 선택합니다. 위젯이 대시보드에 추가됩니다.
4. 대시보드가 완료될 때까지 이전 단계를 반복합니다.
5. 대시보드의 이름을 변경하려면 대시보드 홈페이지에서 대시보드 이름을 클릭하고 새 이름을 입력합니다.

## <a name="modify-a-custom-dashboard"></a>사용자 지정 대시보드의 수정

사용자 지정 대시보드를 만들 때와 마찬가지로, Cloudyn에 포함된 대시보드는 수정할 수 없습니다. 사용자 지정 대시보드 보고서를 수정하려면 다음을 수행합니다.

1. 대시보드에서 수정할 보고서를 찾아 **편집**을 클릭합니다. 보고서가 표시됩니다.
2. 원하는 대로 보고서를 변경하고 **저장**을 클릭합니다. 보고서가 업데이트되고 변경 사항이 표시됩니다.

## <a name="share-a-custom-dashboard"></a>사용자 지정 대시보드의 공유

사용자 지정 대시보드를 _Public_(공용) 또는 _My Entity_(내 엔터티)에서 다른 사용자와 공유할 수 있습니다. Public(공용)으로 공유하면 모든 사용자가 대시보드를 볼 수 있고, My Entity(내 엔터티)에 공유하면 현재 엔터티에 대한 액세스 권한이 있는 사용자만 대시보드를 볼 수 있습니다. 사용자 지정 대시보드를 Public(공용)에 공유하는 단계와 My Entity(내 엔터티)에 공유하는 단계는 비슷합니다.

사용자 지정 대시보드를 Public(공용)에 공유하려면 다음을 수행합니다.

1. 대시보드에서 **대시보드 설정**을 클릭합니다. [대시보드 설정] 상자가 표시됩니다.  
    ![사용자 지정 대시보드에 대한 대시보드 설정](./media/dashboards/dashboard-options.png)
2. [대시보드 설정] 상자에서 화살표 기호를 클릭한 다음, **Public**(공용)을 클릭합니다. [Public Dashboard]\(공용 대시보드) 확인 대화 상자가 표시됩니다.
3. **예**를 클릭합니다. 이제 대시보드를 다른 사람이 볼 수 있습니다.

## <a name="delete-a-custom-dashboard-report"></a>사용자 지정 대시보드 보고서의 삭제

대시보드에서 사용자 지정 보고서 구성 요소를 삭제할 수 있습니다. 대시보드에서 보고서를 삭제해도 보고서 목록에서 보고서가 삭제되지 않습니다. 대신, 보고서를 삭제하면 대시보드에서만 제거됩니다.

대시보드 구성 요소를 삭제하려면 대시보드 구성 요소에서 **삭제**를 클릭합니다. **삭제**를 클릭하면 해당하는 대시보드 구성 요소가 즉시 삭제됩니다.

## <a name="share-a-dashboard-enterprise"></a>대시보드 공유(엔터프라이즈)

사용자 지정 대시보드를 조직의 모든 사용자 또는 현재 엔터티의 사용자들과 공유할 수 있습니다. 대시보드를 공유하면 다른 사용자들이 개요 수준의 KPI를 신속하게 확인할 수 있습니다. 대시보드를 공유하면 대시보드가 자동으로 모든 Cloudyn 엔터티/고객에 복제됩니다. 공유 대시보드를 변경하는 경우, 변경 사항이 자동으로 업데이트됩니다.

하위 엔터티를 포함한 모든 사용자와 대시보드를 공유하려면 다음을 수행합니다.

1. 대시보드 홈페이지에서 **편집**을 클릭합니다.
2. **공유**를 클릭한 다음, **Public**(공용)을 선택합니다.
3. [Global Public Dashboard]\(글로벌 공용 대시보드) 확인 상자가 표시됩니다.
4. **예**를 클릭하여 대시보드를 글로벌 공용 대시보드로 설정합니다.

현재 엔터티의 모든 사용자와 대시보드를 공유하려면 다음을 수행합니다.

1. 대시보드 홈페이지에서 **편집**을 클릭합니다.
2. **공유**를 클릭한 다음, **My Entity**(내 엔터티)를 선택합니다.
3. **예**를 클릭하여 대시보드를 공용 대시보드로 설정합니다.

## <a name="duplicate-a-custom-dashboard"></a>사용자 지정 대시보드의 복제

새 대시보드를 만들 때 기존 대시보드의 속성을 그대로 사용할 수 있습니다. 대시보드를 복제하여 새 대시보드를 만들 수 있습니다.

이때 사용자 지정 대시보드만 복제할 수 있으며, 표준 대시보드는 복제할 수 없습니다.

사용자 지정 대시보드를 복제하려면 다음을 수행합니다.

1. 복제할 대시보드에서 **다른 이름으로 저장**을 클릭합니다. 이름과 번호가 같은 새 대시보드가 열립니다.
2. 복제한 대시보드의 이름을 바꾸고 원하는 대로 수정합니다.

-또는-

1. [대시보드 설정]에서 복제할 대시보드의 행에서 **다른 이름으로 저장**을 클릭합니다.
2. 복제한 대시보드가 열립니다.
3. 대시보드의 이름을 바꾸고 원하는 대로 수정합니다.

## <a name="set-a-default-dashboard"></a>기본 대시보드 설정

원하는 대시보드를 선택하여 기본값으로 설정할 수 있습니다. 기본으로 설정한 대시보드는 대시보드 탭 목록에서 맨 왼쪽 탭으로 표시됩니다. 기본 대시보드는 Cloudyn 포털을 열 때 표시됩니다.

- 기본으로 설정할 대시보드 탭을 클릭한 다음, 오른쪽에서 **기본값**을 클릭합니다.

-또는-

1. **대시보드 설정**을 클릭하여 사용 가능한 대시보드 목록을 표시하고 기본으로 설정할 대시보드를 선택합니다.  
    ![기본 대시보드에 대한 대시보드 옵션](./media/dashboards/dashboard-options.png)
2. 대시보드의 행에서 **기본값**을 클릭합니다. [기본 대시보드] 확인 상자가 표시됩니다.
3. **예**를 클릭합니다. 대시보드가 기본값으로 설정됩니다.

## <a name="management-dashboard"></a>관리 대시보드
Management 대시보드(MSP 사용자용 MSP 대시보드)에서는 주요 보고서 유형의 하이라이트를 확인할 수 있습니다.  
![다양한 보고서를 표시하는 관리 대시보드](./media/dashboards/management-dash.png)

### <a name="cost-entity-summary-enterprise-only"></a>비용 엔터티 요약(엔터프라이즈 전용)
이 위젯은 엔터티와 계정의 수를 포함하여 관리되는 비용 엔터티를 요약합니다.
- 위젯을 클릭하면 엔터프라이즈 세부 정보 보고서가 열립니다.

### <a name="cost-over-time"></a>시간에 따른 비용
이 위젯은 비용 추세를 파악하는 데 유용합니다. 지난 30일의 추세를 기반으로 마지막 날의 비용을 보여 줍니다.
- 위젯을 클릭하면 시간에 따른 실제 비용 보고서가 열리며 추가 세부 정보를 보고 필터링할 수 있습니다.

### <a name="asset-controller"></a>자산 컨트롤러
이 위젯은 전날부터 실행 중인 인스턴스 수를 지난 30일의 사용 추세 위에 보여 줍니다.
- 위젯을 클릭하면 자산 컨트롤러 대시보드가 열립니다.

### <a name="unused-ri-detector"></a>미사용 RI 감지기
이 위젯은 Amazon EC2 미사용 예약 수를 보여 줍니다.
- 위젯을 클릭하면 현재 사용되지 않은 예약 보고서가 열리고 여기서 수정 가능한 미사용 예약을 볼 수 있습니다.

### <a name="cost-by-service"></a>서비스별 비용
이 위젯은 지난 30일 동안 서비스별로 상각된 비용을 보여 줍니다. 원형 차트 위로 마우스를 가져가면 서비스별 비용이 표시됩니다.
- 위젯을 클릭하면 실제 비용 분석 보고서가 열립니다.

### <a name="potential-savings"></a>절약 가능 금액
이 위젯은 Amazon EC2 및 Amazon RDS에 대한 인스턴스 유형 가격 권장 사항을 보여 줍니다.
- 위젯을 클릭하면 절감 분석 보고서가 열리고 절감 가능성이 있는 인스턴스 유형별로 비용이 나열됩니다.

### <a name="compute-instances---daily-trend"></a>컴퓨팅 인스턴스 - 일별 추세
이 위젯은 지난 30일 동안 유형별 활성 인스턴스를 보여 줍니다.
- 위젯을 클릭하면 시간에 따른 인스턴스 보고서가 열리고 여기서 지난 30일 동안 실행 중인 모든 인스턴스의 분석을 볼 수 있습니다.

### <a name="storage-by-department"></a>부서별 스토리지 공간
이 위젯은 부서별로 사용되는 스토리지 서비스를 보여 줍니다. 원형 차트 위로 마우스를 가져가면 부서별 스토리지 사용량이 표시됩니다.
- 위젯을 클릭하면 S3 Tracker 대시보드가 열립니다.

## <a name="cost-controller-dashboard"></a>비용 컨트롤러 대시보드
비용 컨트롤러 대시보드에서는 사전 설정된 비용 할당을 보여 줍니다.  
![다양한 보고서를 표시하는 비용 컨트롤러 대시보드](./media/dashboards/cost-controller-dashboard.png)

### <a name="cost-over-time"></a>시간에 따른 비용
이 위젯은 비용 추세를 파악하는 데 유용합니다. 지난 30일의 추세를 기반으로 마지막 날의 비용을 보여 줍니다.
- 위젯을 클릭하면 시간에 따른 실제 비용 보고서가 열리며 추가 세부 정보를 보고 필터링할 수 있습니다.

### <a name="monthly-cost-trends"></a>월 비용 추세
이 위젯은 월의 시작 이후에 상각 지출 예상액 및 실제 지출액을 보여 줍니다.
- 위젯을 클릭하면 현재 월 예상 비용 보고서가 열리고 월간 누계 비용 요약을 보여 줍니다.

이 보고서는 월의 시작 이후에 발생한 비용, 이전 월의 비용, 현재 월 예상 비용을 보여 줍니다. 현재 월 예상 비용은 누계 월별 비용 및 예상 비용을 더해서 계산합니다. 예상치는 지난 30일 동안 모니터링한 비용을 기준으로 계산됩니다.

### <a name="12-month-planner"></a>12개월 플래너
이 위젯은 다음 12개월의 예상 비용 및 절약 가능 금액을 보여 줍니다.
- 위젯을 클릭하면 연간 예상 비용 보고서가 열립니다.

### <a name="cost-by-service"></a>서비스별 비용
이 위젯은 지난 30일 동안 서비스별로 상각된 비용을 보여 줍니다.
- 원형 차트 위로 마우스를 가져가면 서비스별 비용이 표시됩니다.
- 위젯을 클릭하면 실제 비용 분석 보고서가 열립니다.

### <a name="cost-by-account"></a>계정별 비용
이 위젯은 지난 30일 동안 계정별로 상각된 비용을 보여 줍니다.
- 원형 차트 위로 마우스를 가져가면 계정별 비용이 표시됩니다.
- 위젯을 클릭하면 실제 비용 분석 보고서가 열립니다.

### <a name="cost-trend-by-day"></a>일별 비용 추세
이 위젯은 지난 30일의 지출을 보여 줍니다.
- 막대 그래프 위로 마우스를 가져가면 일별 비용이 표시됩니다.
- 위젯을 클릭하면 시간에 따른 실제 비용 보고서가 열립니다.

### <a name="cost-trend-by-month---last-6-months"></a>월별 비용 추세 - 지난 6개월

이 위젯은 지난 6개월의 지출을 보여 줍니다.
- 막대 그래프 위로 마우스를 가져가면 월별 비용이 표시됩니다.
- 위젯을 클릭하면 시간에 따른 실제 비용 보고서가 열립니다.

## <a name="asset-controller-dashboard"></a>자산 컨트롤러 대시보드

이 대시보드에서는 실행 중인 인스턴스 수, 사용 가능한 디스크와 사용 중인 디스크, 인스턴스 유형 분포, 스토리지 정보 등을 보여 줍니다.  
![다양한 보고서를 표시하는 자산 컨트롤러 대시보드](./media/dashboards/asset-controller-dashboard.png)

### <a name="compute-instances"></a>컴퓨팅 인스턴스
이 위젯은 지난 30일의 사용량 추세를 기반으로 실행 중인 인스턴스 수를 보여 줍니다.
- 위젯을 클릭하면 시간에 따른 인스턴스 보고서가 열립니다.

### <a name="disks"></a>디스크
이 위젯은 사용 중이며 사용 가능한 디스크의 총 개수와 볼륨을 보여 줍니다.
- 위젯을 클릭하면 활성 디스크 보고서가 열립니다.

### <a name="instance-type-distribution"></a>인스턴스 유형 분포
이 위젯은 인스턴스 유형을 원형 차트로 보여 줍니다.
- 위젯을 클릭하면 인스턴스 분포 보고서가 열리고 선택한 집계별로 활성 인스턴스를 분석한 결과가 표시됩니다.

### <a name="compute-instances---daily-trend"></a>컴퓨팅 인스턴스 - 일별 추세
이 위젯은 지난 30일 동안의 일별 컴퓨팅 인스턴스(즉석, 예약, 주문형)를 보여 줍니다.
- 그래프 위로 마우스를 가져가면 유형별 컴퓨팅 인스턴스 수가 일별로 표시됩니다.
- 위젯을 클릭하면 시간에 따른 인스턴스 보고서가 열립니다.

### <a name="all-buckets-s3"></a>모든 버킷(S3)
이 위젯은 총 S3 스토리지 공간 및 스토리지된 개체 수를 보여 줍니다.
- 위젯을 클릭하면 S3 Tracker 대시보드가 열립니다. 이 대시보드를 사용하면 현재 스토리지 사용량 및 추세를 찾고 분석하고 표시할 수 있습니다.

### <a name="sql-db-instances-rds"></a>SQL DB 인스턴스(RDS)
이 위젯은 지난 30일의 추세를 기반으로 실행 중인 Amazon RDS 인스턴스 수를 보여 줍니다.
- 위젯을 클릭하면 시간에 따른 RDS 인스턴스 보고서가 열립니다.

## <a name="optimizer-dashboard"></a>Optimizer(최적화 프로그램) 대시보드
이 대시보드에서는 다운사이징 권장 사항, 사용되지 않는 리소스, 절약 가능 금액을 보여 줍니다.  
![다양한 보고서를 표시하는 최적화 대시보드](./media/dashboards/optimizer-dashboard.png)

### <a name="ri-calculator"></a>RI 계산기
이 위젯은 RI 구매 권장 사항을 제시하고 연간 절약 가능 금액을 보여 줍니다.
- 위젯을 클릭하면 예약 인스턴스 계산기가 열리고 여기서 주문형 플랜과 예약 플랜을 언제 사용할지 결정할 수 있습니다.

### <a name="sizing"></a>크기 조정
이 위젯은 구현하는 경우, 크기 조정 권장 사항 및 절약 가능 금액을 보여 줍니다.
- 위젯을 클릭하면 EC2 비용 효과적인 크기 조정 권장 사항 보고서가 열립니다.

### <a name="unused-ri-detector"></a>미사용 RI 감지기
이 위젯은 Amazon EC2 미사용 예약 수를 보여 줍니다.
- 위젯을 클릭하면 현재 사용되지 않은 예약 보고서가 열리고 여기서 수정 가능한 미사용 예약을 볼 수 있습니다.

###  <a name="available-disks"></a>사용 가능한 디스크
이 위젯은 배포에서 연결되지 않은 디스크의 수를 보여 줍니다.
- 위젯을 클릭하면 연결되지 않은 디스크 보고서가 열립니다.

### <a name="rds-ri-calculator"></a>RDS RI 계산기
이 위젯은 권장하는 Amazon RDS 인스턴스 예약 수 및 가능한 절감액을 보여 줍니다.
- 위젯을 클릭하면 RDS RI 구매 권장 사항 보고서가 열리고 여기서 주문형 인스턴스 대신 예약 인스턴스를 사용하도록 하는 Cloudyn 권장 사항을 볼 수 있습니다.

### <a name="rds-sizing"></a>RDS 크기 조정
이 위젯은 크기 조정 권장 사항 및 절약 가능 금액을 보여 줍니다.
- 위젯을 클릭하면 RDS 크기 조정 권장 사항 보고서가 열리고 자세한 Amazon RDS 크기 조정 권장 사항이 표시됩니다.

최적화 권장 사항은 지난달에 모니터링한 사용량 및 성능 데이터를 기반으로 합니다.

## <a name="s3-tracker-dashboard"></a>S3 Tracker 대시보드
S3 Tracker 대시보드를 사용하면 현재 스토리지 사용량 및 추세를 찾고 분석하고 표시할 수 있습니다.  
![다양한 보고서를 표시하는 S3 Tracker 대시보드](./media/dashboards/s3-tracker-dashboard.png)

### <a name="all-buckets"></a>모든 버킷
이 위젯은 모든 버킷의 전체 크기(GB 단위)와 버킷에 있는 총 개체 수를 보여 줍니다.
- 위젯을 클릭하면 S3 크기 분포 보고서가 열립니다. 이 보고서를 활용하면 버킷, 최상위 폴더, 스토리지 클래스 및 버전 관리 상태를 기준으로 S3 크기를 분석할 수 있습니다.

### <a name="bucket-properties"></a>버킷 속성
이 위젯은 총 스토리지 버킷 수를 보여 줍니다.
- 위젯을 클릭하면 S3 버킷 속성 보고서를 볼 수 있습니다.

### <a name="scan-status"></a>검색 상태
이 위젯은 마지막 S3 검색이 완료된 시기 및 다음 검색이 시작되는 시기를 보여 줍니다.
- 위젯을 클릭하면 S3 검색 상태 보고서가 열립니다.

### <a name="storage-by-bucket"></a>버킷별 스토리지 공간
이 위젯은 각 버킷 스토리지 클래스가 사용하는 공간을 백분율로 보여 줍니다.
- 위젯을 클릭하면 S3 크기 분포 보고서가 열립니다. 이 보고서를 활용하면 버킷, 최상위 폴더, 스토리지 클래스 및 버전 관리 상태를 기준으로 S3 크기를 분석할 수 있습니다.

### <a name="number-of-objects-by-bucket"></a>버킷별 개체 수
이 위젯은 버킷별 개체 수를 실제 수와 백분율로 보여 줍니다. 버킷 위로 마우스를 가져가면 총 개체 수가 표시됩니다.
- 위젯을 클릭하면 S3 크기 분포 보고서(검색 기반)가 열립니다.

## <a name="cloud-comparison-dashboard"></a>클라우드 비교 대시보드
클라우드 비교 대시보드를 사용하면 가격, CPU 종류 및 RAM 크기를 기준으로 여러 클라우드 공급자의 비용을 비교할 수 있습니다.  
![다양한 보고서를 표시하는 클라우드 비교 대시보드](./media/dashboards/cloud-comparison-dashboard.png)

### <a name="ec2-cost-in-azure-by-instance-type"></a>인스턴스 유형별 Azure의 EC2 비용
이 위젯은 지난 30일의 주문형 요금 사용 추세를 보여 줍니다. 현재 Amazon EC2 비용과 Azure의 잠재 비용을 비교합니다.
- 막대 위로 마우스를 가져가면 인스턴스 유형별로 비용을 비교할 수 있습니다.
- 위젯을 클릭하면 배포 포팅 - 비용 분석 보고서가 열립니다.

### <a name="ec2-cost-in-azure"></a>Azure의 EC2 비용
이 위젯은 현재 Amazon EC2 비용을 보여주고 이를 Azure와 비교합니다. 비교는 지난 30일 동안 주문형 요금 사용 추세를 기반으로 합니다.
- 위젯을 클릭하면 배포 포팅 - 비용 분석 보고서가 열립니다.

### <a name="ec2azure-instance-type-mapping"></a>EC2/Azure 인스턴스 유형 매핑
이 위젯은 Amazon EC2와 Azure 간에 최적인 탄력적 컴퓨팅 단위 매핑을 보여 줍니다.
- 위젯을 클릭하면 인스턴스 유형 매핑 보고서가 열립니다.

## <a name="next-steps"></a>다음 단계
- 보고서에 대해 자세히 알아보려면 [Cloudyn 보고서 사용](use-reports.md) 문서를 참조하세요.
