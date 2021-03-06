---
title: Azure Automation에 업데이트 및 변경 내용 추적 솔루션 등록
description: Azure Automation에 업데이트 및 변경 내용 추적 솔루션을 등록하는 방법을 알아봅니다.
services: automation
ms.topic: tutorial
ms.date: 05/10/2018
ms.custom: mvc
ms.openlocfilehash: d0024b8c43e76e3dd26b4b73c4ae0e09890b3b46
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75421837"
---
# <a name="onboard-update-and-change-tracking-solutions-to-azure-automation"></a>Azure Automation에 업데이트 및 변경 내용 추적 솔루션 등록

이 자습서에서는 다음과 같이 VM에 대한 업데이트, 변경 내용 추적 및 인벤토리 솔루션을 Azure Automation에 자동으로 등록하는 방법을 알아봅니다.

> [!div class="checklist"]
> * Azure VM 등록
> * 솔루션 사용
> * 모듈 설치 및 업데이트
> * 온보딩 Runbook 가져오기
> * Runbook을 시작합니다.

## <a name="prerequisites"></a>사전 요구 사항

이 자습서를 완료하려면 다음 항목이 필요합니다.

* 동작합니다. 구독이 아직 없는 경우 [MSDN 구독자 혜택을 활성화](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)하거나 [무료 계정](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)을 등록할 수 있습니다.
* 컴퓨터를 관리하기 위한 [Automation 계정](automation-offering-get-started.md)
* 등록할 [가상 머신](../virtual-machines/windows/quick-create-portal.md)

## <a name="onboard-an-azure-vm"></a>Azure VM 등록

머신을 등록하는 여러 가지 방법이 있으며, [가상 머신](automation-onboard-solutions-from-vm.md), [여러 머신 검색](automation-onboard-solutions-from-browse.md), [Automation 계정](automation-onboard-solutions-from-automation-account.md) 또는 runbook에서 솔루션을 등록할 수 있습니다. 이 자습서에서는 Runbook을 통해 업데이트 관리를 사용하도록 설정하는 과정을 안내합니다. Azure Virtual Machines를 규모에 맞게 등록하려면 변경 내용 추적 또는 업데이트 관리 솔루션으로 기존 VM을 등록해야 합니다. 이 단계에서는 가상 머신을 업데이트 관리 및 변경 내용 추적에 등록합니다.

### <a name="enable-change-tracking-and-inventory"></a>변경 내용 추적 및 인벤토리 사용

변경 내용 추적 및 인벤토리 솔루션은 가상 머신에서 [변경 내용 추적](automation-vm-change-tracking.md) 및 [인벤토리](automation-vm-inventory.md)의 기능을 제공합니다. 이 단계에서는 가상 머신에 솔루션을 사용할 수 있습니다.

1. 왼쪽 메뉴에서 **Automation 계정**을 선택한 다음 목록에서 자동화 계정을 선택합니다.
1. **구성 관리** 아래에서 **인벤토리**를 선택합니다.
1. 기존 Log Analytics 작업 영역을 선택하거나 작업 영역을 새로 만듭니다. **사용** 단추를 클릭합니다.

![업데이트 솔루션 등록](media/automation-onboard-solutions/inventory-onboard.png)

변경 내용 추적 및 인벤토리 솔루션 등록 알림이 완료되면 **구성 관리** 아래에서 **업데이트 관리**를 클릭합니다.

### <a name="enable-update-management"></a>업데이트 관리 사용

업데이트 관리 솔루션을 사용하면 Azure Windows VM에 대한 업데이트 및 패치를 관리할 수 있습니다. 사용 가능한 업데이트의 상태를 평가하고, 필수 업데이트의 설치를 예약하고, 배포 결과를 검토하여 업데이트가 VM에 성공적으로 적용되었는지 확인할 수 있습니다. 이 단계에서는 VM에 솔루션을 사용할 수 있습니다.

1. Automation 계정의 **업데이트 관리** 아래에서 **업데이트 관리**를 선택합니다.
1. 선택한 로그 분석 작업 영역은 이전 단계에서 사용된 동일한 작업 영역입니다. **사용**을 클릭하여 업데이트 관리 솔루션을 등록합니다.

![업데이트 솔루션 등록](media/automation-onboard-solutions/update-onboard.png)

업데이트 관리 솔루션을 설치하는 동안 파란색 배너가 표시됩니다. 솔루션을 사용할 때 **구성 관리** 아래에서 **변경 내용 추적**을 선택하여 다음 단계로 이동합니다.

### <a name="select-azure-vm-to-be-managed"></a>Azure VM을 관리하도록 선택

솔루션을 사용했으므로 해당 솔루션에 등록할 Azure VM을 추가할 수 있습니다.

1. Automation 계정의 **변경 내용 추적** 페이지에서 **+ Azure VM 추가**를 선택하여 가상 머신을 추가합니다.

1. 목록에서 VM을 선택하고 **사용**을 선택합니다. 이 작업을 사용하면 가상 머신에서 변경 내용 추적 및 인벤토리 솔루션을 사용할 수 있습니다.

   ![VM에 업데이트 솔루션 사용](media/automation-onboard-solutions/enable-change-tracking.png)

1. VM 등록 알림이 완료되면 Automation 계정의 **업데이트 관리** 아래에서 **업데이트 관리**를 선택합니다.

1. **+ Azure VM 추가**를 선택하여 가상 머신을 추가합니다.

1. 목록에서 VM을 선택하고 **사용**을 선택합니다. 이러한 작업을 사용하면 가상 머신에서 업데이트 관리 솔루션을 사용할 수 있습니다.

   ![VM에 업데이트 솔루션 사용](media/automation-onboard-solutions/enable-update.png)

> [!NOTE]
> 다른 솔루션이 완료되기까지 기다리지 않고 다음 솔루션을 사용하도록 설정하면 다음과 같은 메시지가 나타납니다. *이 가상 머신 또는 다른 가상 머신에서 다른 솔루션 설치가 진행 중입니다. 해당 설치가 사용 단추 사용을 완료하면 이 가상 머신에 솔루션을 설치하도록 요청할 수 있습니다.*

## <a name="install-and-update-modules"></a>모듈 설치 및 업데이트

솔루션 등록을 자동화하려면 최신 Azure 모듈로 업데이트하고 `AzureRM.OperationalInsights`를 가져와야 합니다.

## <a name="update-azure-modules"></a>Azure 모듈 업데이트

Automation 계정의 **공유 리소스** 아래에서 **모듈**을 선택합니다. **Azure 모듈 업데이트**를 선택하여 Azure 모듈을 최신 버전으로 업데이트합니다. 모든 기존 Azure 모듈을 최신 버전으로 업데이트하라는 메시지에서 **예**를 선택합니다.

![모듈 업데이트](media/automation-onboard-solutions/update-modules.png)

### <a name="install-azurermoperationalinsights-module"></a>AzureRM.OperationalInsights 모듈 설치

**모듈** 페이지에서 **찾아보기 갤러리**를 선택하여 모듈 갤러리를 엽니다. AzureRM.OperationalInsights를 검색하고 Automation 계정으로 이 모듈을 가져옵니다.

![OperationalInsights 모듈 가져오기](media/automation-onboard-solutions/import-operational-insights-module.png)

## <a name="import-the-onboarding-runbook"></a>온보딩 Runbook 가져오기

1. Automation 계정의 **프로세스 자동화** 아래에서 **Runbook**을 선택합니다.
1. **갤러리 찾아보기**를 선택합니다.
1. **업데이트 및 변경 내용 추적**을 검색하고, Runbook을 클릭하고, **원본 보기** 페이지에서 **가져오기**를 선택합니다. **확인**을 선택하여 Runbook을 Automation 계정으로 가져옵니다.

   ![온보딩 Runbook 가져오기](media/automation-onboard-solutions/import-from-gallery.png)

1. **Runbook** 페이지에서 **편집**을 선택한 후 **게시**를 선택합니다. **Runbook 게시** 대화 상자에서 **예**를 선택하여 Runbook을 게시합니다.

## <a name="start-the-runbook"></a>Runbook을 시작합니다.

이 runbook을 시작하려면 변경 내용 추적 또는 업데이트 솔루션을 Azure VM에 등록했어야 합니다. 매개 변수로 솔루션이 등록된 기존 가상 머신 및 리소스 그룹이 필요합니다.

1. Enable-MultipleSolution Runbook을 엽니다.

   ![다중 솔루션 Runbook](media/automation-onboard-solutions/runbook-overview.png)

1. 시작 단추를 클릭하고 매개 변수에 대해 다음 값을 입력합니다.

   * **VMNAME** - 빈 상태로 둡니다. 업데이트 또는 변경 내용 추적 솔루션에 등록할 기존 VM의 이름입니다. 이 값을 비워 두면 리소스 그룹에 있는 모든 VM이 등록됩니다.
   * **VMRESOURCEGROUP** - 등록할 VM에 대한 리소스 그룹의 이름입니다.
   * **SUBSCRIPTIONID** - 비워 둡니다. 등록할 새 VM의 구독 ID입니다. 이를 비워 두면 작업 영역의 구독이 사용됩니다. 다른 구독 ID를 지정하는 경우 이 Automation 계정에 대한 실행 계정을 이 구독에 대한 참가자로도 추가해야 합니다.
   * **ALREADYONBOARDEDVM** - 업데이트 또는 변경 내용 추적 솔루션에 수동으로 등록한 VM의 이름입니다.
   * **ALREADYONBOARDEDVMRESOURCEGROUP** - VM이 구성원인 리소스 그룹의 이름입니다.
   * **SOLUTIONTYPE** - **업데이트** 또는 **변경 내용 추적**을 입력합니다.

   ![Enable-MultipleSolution Runbook 매개 변수](media/automation-onboard-solutions/runbook-parameters.png)

1. **확인**을 선택하여 Runbook 작업을 시작합니다.
1. runbook 작업 페이지에서 진행률 및 오류를 모니터링합니다.

## <a name="clean-up-resources"></a>리소스 정리

업데이트 관리에서 VM을 제거하려면:

* Log Analytics 작업 영역에서, 범위 구성 `MicrosoftDefaultScopeConfig-Updates`에 대한 저장된 검색에서 VM을 제거합니다. 저장된 검색은 작업 영역의 **일반**에서 찾을 수 있습니다.
* [Microsoft Monitoring Agent](../azure-monitor/learn/quick-collect-windows-computer.md#clean-up-resources) 또는 [Linux용 Log Analytics 에이전트](../azure-monitor/learn/quick-collect-linux-computer.md#clean-up-resources)를 제거합니다.

## <a name="next-steps"></a>다음 단계

이 자습서에서는 다음 작업 방법을 알아보았습니다.

> [!div class="checklist"]
> * Azure Virtual Machine을 수동으로 등록합니다.
> * 필요한 Azure 모듈을 설치 및 업데이트합니다.
> * Azure VM을 등록하는 Runbook을 가져옵니다.
> * Azure VM을 자동으로 등록하는 Runbook을 시작합니다.

Runbook을 예약하는 방법에 대해 더 자세한 내용을 알아보려면 이 링크를 따라가세요.

> [!div class="nextstepaction"]
> [Runbook 예약](automation-schedules.md)
