---
title: 'Azure Databricks: 일반적인 질문 및 도움말'
description: Azure Databricks와 관련된 일반적인 질문에 대한 답변과 문제 해결 정보를 살펴봅니다.
services: azure-databricks
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: azure-databricks
ms.workload: big-data
ms.topic: conceptual
ms.date: 10/25/2018
ms.openlocfilehash: c2cb7a90f0fe57efcd8f4d75aff3b5ee375abd07
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/15/2020
ms.locfileid: "75971506"
---
# <a name="frequently-asked-questions-about-azure-databricks"></a>Azure Databricks에 대해 자주 묻는 질문

이 문서에는 Azure Databricks와 관련될 수 있는 주요 질문이 나열되어 있습니다. 또한 Databricks를 사용하는 동안 발생할 수 있는 몇 가지 일반적인 문제를 나열합니다. 자세한 내용은 [Azure Databricks란?](what-is-azure-databricks.md)을 참조하세요. 

## <a name="can-i-use-azure-key-vault-to-store-keyssecrets-to-be-used-in-azure-databricks"></a>Azure Databricks에 사용되는 키/비밀 정보를 저장하기 위해 Azure Key Vault를 사용할 수 있나요?
예. Azure Databricks에 사용할 키/비밀 정보를 저장하기 위해 Azure Key Vault를 사용할 수 있습니다. 자세한 내용은 [Azure Key Vault 지원 범위](/azure/databricks/security/secrets/secret-scopes)를 참조하세요.


## <a name="can-i-use-azure-virtual-networks-with-databricks"></a>Databricks에서 Azure Virtual Networks를 사용할 수 있나요?
예. Azure Databricks에서 Azure VNET(Virtual Network)을 사용할 수 있습니다. 자세한 내용은 [Azure Virtual Network에서 Azure Databricks 배포](/azure/databricks/administration-guide/cloud-configurations/azure/vnet-inject)를 참조하세요.

## <a name="how-do-i-access-azure-data-lake-storage-from-a-notebook"></a>노트북에서 Azure Data Lake Storage에 액세스할 어떻게 할까요? 있나요? 

다음 단계를 수행하세요.
1. Azure AD(Active Directory)에서 서비스 주체를 프로비전하고 해당 키를 기록합니다.
1. Data Lake Storage에서 서비스 주체에 필요한 사용 권한을 할당 합니다.
1. Data Lake Storage의 파일에 액세스 하려면 노트북에서 서비스 주체 자격 증명을 사용 합니다.

자세한 내용은 [Azure Databricks와 함께 Azure Data Lake Storage 사용](/azure/databricks/data/data-sources/azure/azure-datalake)을 참조 하세요.

## <a name="fix-common-problems"></a>일반적인 문제 해결

Databricks에서 발생하는 몇 가지 문제는 다음과 같습니다.

### <a name="issue-this-subscription-is-not-registered-to-use-the-namespace-microsoftdatabricks"></a>문제: 구독이 ‘Microsoft.Databricks’ 네임스페이스를 사용하도록 등록되어 있지 않습니다.

#### <a name="error-message"></a>오류 메시지

“구독이 ‘Microsoft.Databricks’ 네임스페이스를 사용하도록 등록되어 있지 않습니다. 구독을 등록하는 방법은 https://aka.ms/rps-not-found 를 참조하세요. (코드: MissingSubscriptionRegistration)”

#### <a name="solution"></a>솔루션

1. [Azure 포털](https://portal.azure.com)로 이동합니다.
1. **구독**을 선택하고 사용 중인 구독을 선택한 다음 **리소스 공급자**를 선택합니다. 
1. 리소스 공급자 목록에서 **Microsoft.Databricks**를 찾아 **등록**을 선택합니다. 구독의 참가자 또는 소유자 역할이 할당된 사용자만 리소스 공급자를 등록할 수 있습니다.


### <a name="issue-your-account-email-does-not-have-the-owner-or-contributor-role-on-the-databricks-workspace-resource-in-the-azure-portal"></a>문제: {메일} 계정에는 Azure Portal의 Databricks 작업 영역 리소스에 소유자 또는 참가자 역할이 없습니다.

#### <a name="error-message"></a>오류 메시지

“{메일} 계정에는 Azure Portal의 Databricks 작업 영역 리소스에 소유자 또는 참가자 역할이 없습니다. 테넌트에서 게스트 사용자인 경우에도 이 오류가 발생할 수 있습니다. 관리자에게 문의하여 액세스 권한을 부여받거나 Databricks 작업 영역에서 직접 사용자로 추가합니다.” 

#### <a name="solution"></a>솔루션

다음은 이 문제에 대한 두 가지 해결 방법입니다.

* 테넌트를 초기화하려면 게스트 사용자가 아닌, 테넌트의 일반 사용자로 로그인해야 합니다. 또한 Databricks 작업 영역 리소스에 참가자 역할이 있어야 합니다. Azure Portal에 있는 Databricks 작업 영역 내의 **Access Control(IAM)** 탭에서 사용자 액세스 권한을 부여할 수 있습니다.

* 메일 도메인 이름이 여러 디렉터리에 할당된 경우에도 이 오류가 발생할 수 있습니다. 이 문제를 해결하려면 Databricks 작업 영역을 갖는 구독이 포함된 디렉터리에 새 사용자를 만듭니다.

    a. Azure Portal에서 Azure AD로 이동합니다. **사용자 및 그룹** > **사용자 추가**를 선택합니다.

    b. `@<your_domain>` 메일이 아닌 `@<tenant_name>.onmicrosoft.com` 메일을 사용하여 사용자를 추가합니다. Azure Portal에 있는 Azure AD의 **사용자 지정 도메인**에서 이 옵션을 찾을 수 있습니다.
    
    다. 새 사용자에게 Databricks 작업 영역 리소스에 대해 **참가자** 역할을 할당합니다.
    
    d. Azure Portal에 새 사용자로 로그인하여 Databricks 작업 영역을 찾습니다.
    
    e. 이 사용자로 Databricks 작업 영역을 시작합니다.


### <a name="issue-your-account-email-has-not-been-registered-in-databricks"></a>문제: {메일} 계정이 Databricks에 등록되지 않았습니다. 

#### <a name="solution"></a>솔루션

본인이 작업 영역을 만들지 않았고 사용자로 추가된 경우 작업 영역을 만든 사용자에게 문의합니다. 해당 사용자에게 Azure Databricks 관리 콘솔을 사용하여 추가하게 합니다. 자세한 내용은 [사용자 추가 및 관리](/azure/databricks/administration-guide/users-groups/users)를 참조하세요. 작업 영역을 만든 후 계속해서 이 오류가 발생되면 Azure Portal에서 **작업 영역 초기화**를 다시 선택합니다.

### <a name="issue-cloud-provider-launch-failure-while-setting-up-the-cluster-publicipcountlimitreached"></a>문제: 클러스터 설정 중 클라우드 공급자 시작 실패(PublicIPCountLimitReached)

#### <a name="error-message"></a>오류 메시지

“클라우드 공급자 시작 실패: 클러스터를 설정하는 동안 클라우드 공급자 오류가 발생했습니다. 자세한 내용은 Databricks 가이드를 참조하세요. Azure 오류 코드: PublicIPCountLimitReached. Azure 오류 메시지: 이 지역에서 이 구독에 대해 60개를 초과하는 공용 IP 주소를 만들 수 없습니다.”

#### <a name="solution"></a>솔루션

Databricks 클러스터는 노드당 하나의 공용 IP 주소를 사용합니다. 구독이 이미 해당 공용 IP를 모두 사용한 경우 [할당량 증가를 요청](https://docs.microsoft.com/azure/azure-portal/supportability/resource-manager-core-quotas-request)해야 합니다. **발급 유형**으로 **할당량**을, **할당량 유형**으로 **네트워킹: ARM**을 선택합니다. **세부 정보**에서 공용 IP 주소 할당량 증대를 요청합니다. 예를 들어, 현재 한도가 60이고 100개의 노드 클러스터를 만들려는 경우 160으로 한도 증가를 요청합니다.

### <a name="issue-a-second-type-of-cloud-provider-launch-failure-while-setting-up-the-cluster-missingsubscriptionregistration"></a>문제: 클러스터 설정 중 두 번째 클라우드 공급자 시작 실패 형식(MissingSubscriptionRegistration)

#### <a name="error-message"></a>오류 메시지

“클라우드 공급자 시작 실패: 클러스터를 설정하는 동안 클라우드 공급자 오류가 발생했습니다. 자세한 내용은 Databricks 가이드를 참조하세요.
Azure 오류 코드: MissingSubscriptionRegistration Azure 오류 메시지: 구독이 ‘Microsoft.Compute’ 네임스페이스를 사용하도록 등록되지 않았습니다. 구독을 등록하는 방법은 https://aka.ms/rps-not-found 를 참조하세요.

#### <a name="solution"></a>솔루션

1. [Azure 포털](https://portal.azure.com)로 이동합니다.
1. **구독**을 선택하고 사용 중인 구독을 선택한 다음 **리소스 공급자**를 선택합니다. 
1. 리소스 공급자 목록에서 **Microsoft.Compute**를 찾아 **등록**을 선택합니다. 구독의 참가자 또는 소유자 역할이 할당된 사용자만 리소스 공급자를 등록할 수 있습니다.

자세한 방법은 [리소스 공급자 및 유형](../azure-resource-manager/management/resource-providers-and-types.md)을 참조하세요.

### <a name="issue-azure-databricks-needs-permissions-to-access-resources-in-your-organization-that-only-an-admin-can-grant"></a>문제: Azure Databricks에는 관리자만이 부여할 수 있는 조직의 리소스에 액세스할 수 있는 권한이 필요합니다.

#### <a name="background"></a>배경

Azure Databricks는 Azure Active Directory와 통합됩니다. Azure AD에서 사용자를 지정하여 Azure Databricks 내에서(예: Notebooks 또는 클러스터에서) 사용 권한을 설정합니다. Azure AD에서 사용자의 이름을 나열할 수 있으려면 Azure Databricks에는 해당 정보에 대한 읽기 권한 및 동의가 필요합니다. 동의를 사용할 수 없는 경우 오류가 표시됩니다.

#### <a name="solution"></a>솔루션

전역 관리자 권한으로 Azure Portal에 로그인합니다. Azure Active Directory의 경우 **사용자 설정** 탭으로 이동하고, **사용자가 대신 회사 데이터에 액세스하는 앱에 동의할 수 있습니다**를 **예**로 설정하도록 합니다.

## <a name="next-steps"></a>다음 단계

- [빠른 시작: Azure Databricks 시작](quickstart-create-databricks-workspace-portal.md)
- [Azure Databricks란?](what-is-azure-databricks.md)

