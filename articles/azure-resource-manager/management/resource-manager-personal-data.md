---
title: 개인 데이터
description: Azure Resource Manager 작업과 관련된 개인 데이터를 관리하는 방법을 알아봅니다.
ms.topic: conceptual
ms.date: 05/14/2018
ms.openlocfilehash: 22cfc1b6096980f3d10db404a1c4e02f2de355d2
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75485261"
---
# <a name="manage-personal-data-associated-with-azure-resource-manager"></a>Azure Resource Manager와 관련된 개인 데이터 관리

중요한 정보가 노출되지 않도록 하려면 배포, 자원 그룹 또는 태그에 제공한 개인 정보를 삭제합니다. Azure Resource Manager는 배포, 자원 그룹 또는 태그에 제공한 개인 데이터를 관리할 수 있도록 하는 작업을 제공합니다.

[!INCLUDE [Handle personal data](../../../includes/gdpr-intro-sentence.md)]

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="delete-personal-data-in-deployment-history"></a>배포 기록에서 개인 데이터 삭제

배포의 경우 Resource Manager는 배포 기록에 매개 변수 값과 상태 메시지를 유지합니다. 이러한 값은 실행 기록에서 배포를 삭제할 때까지 유지됩니다. 이 값으로 개인 데이터를 제공했는지 확인하려면 배포를 나열합니다. 개인 데이터를 찾으면 기록에서 배포를 삭제합니다.

기록에 **배포**를 나열하려면 다음을 사용합니다.

* [리소스 그룹별 목록](/rest/api/resources/deployments/listbyresourcegroup)
* [Get-AzResourceGroupDeployment](/powershell/module/az.resources/Get-AzResourceGroupDeployment)
* [az 그룹 배포 목록](/cli/azure/group/deployment#az-group-deployment-list)

기록에서 **배포**를 삭제하려면 다음을 사용합니다.

* [Delete](/rest/api/resources/deployments/delete)
* [Remove-AzResourceGroupDeployment](/powershell/module/az.resources/Remove-AzResourceGroupDeployment)
* [az 그룹 배포 삭제](/cli/azure/group/deployment#az-group-deployment-delete)

## <a name="delete-personal-data-in-resource-group-names"></a>리소스 그룹 이름에서 개인 데이터 삭제

자원 그룹의 이름은 자원 그룹을 삭제할 때까지 지속됩니다. 이름에 개인 데이터를 제공했는지 확인하려면 리소스 그룹 나열합니다. 개인 데이터를 찾으면 새 리소스 그룹으로 [리소스를 이동](move-resource-group-and-subscription.md)하고 이름에 개인 데이터가 있는 리소스 그룹을 삭제합니다.

**리소스 그룹**을 나열하려면 다음을 사용합니다.

* [목록](/rest/api/resources/resourcegroups/list)
* [Get-AzResourceGroup](/powershell/module/az.resources/Get-AzResourceGroup)
* [az 그룹 목록](/cli/azure/group#az-group-list)

**리소스 그룹**을 삭제하려면 다음을 사용홥니다.

* [Delete](/rest/api/resources/resourcegroups/delete)
* [Remove-AzResourceGroup](/powershell/module/az.resources/Remove-AzResourceGroup)
* [az group delete](/cli/azure/group#az-group-delete)

## <a name="delete-personal-data-in-tags"></a>태그에 있는 개인 데이터 삭제

태그 이름 및 값은 태그를 삭제하거나 수정할 때까지 유지됩니다. 태그에 개인 데이터를 제공했는지 확인하려면 태그를 나열합니다. 개인 데이터를 찾으면 태그를 삭제합니다.

**태그**를 나열하려면 다음을 사용합니다.

* [목록](/rest/api/resources/tags/list)
* [Get-AzTag](/powershell/module/az.resources/Get-AzTag)
* [az 태그 목록](/cli/azure/tag#az-tag-list)

**태그**를 삭제하려면 다음을 사용합니다.

* [Delete](/rest/api/resources/tags/delete)
* [Remove-AzTag](/powershell/module/az.resources/Remove-AzTag)
* [az 태그 삭제](/cli/azure/tag#az-tag-delete)

## <a name="next-steps"></a>다음 단계
* Azure Resource Manager에 대한 개요는 [Resource Manager란 무엇입니까?](overview.md)를 참조하세요.