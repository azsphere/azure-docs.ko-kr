---
title: Azure Active Directory 포털에서 사용자 대량 삭제 (미리 보기) | Microsoft Docs
description: Azure Active Directory의 Azure 관리 센터에서 대량으로 사용자를 삭제 합니다.
services: active-directory
author: curtand
ms.author: curtand
manager: mtillman
ms.date: 08/15/2019
ms.topic: conceptual
ms.service: active-directory
ms.subservice: users-groups-roles
ms.workload: identity
ms.custom: it-pro
ms.reviewer: jeffsta
ms.collection: M365-identity-device-management
ms.openlocfilehash: d7c47887c12c8bf9be7a0c5b11dfb3f099965cb7
ms.sourcegitcommit: 42748f80351b336b7a5b6335786096da49febf6a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72174369"
---
# <a name="bulk-delete-users-preview-in-azure-active-directory"></a>Azure Active Directory에서 사용자 대량 삭제 (미리 보기)

Azure Active Directory (Azure AD) 포털을 사용 하 여 쉼표로 구분 된 값 (CSV) 파일을 사용 하 여 사용자를 대량으로 삭제 하 여 많은 수의 구성원을 그룹으로 제거할 수 있습니다.

## <a name="to-bulk-delete-users"></a>사용자를 대량으로 삭제 하려면

1. 조직의 사용자 관리자 계정으로 [AZURE AD 조직에 로그인](https://aad.portal.azure.com) 합니다.
1. Azure AD에서 **사용자** > **대량 삭제**를 선택 합니다.
1. **사용자 대량 삭제** 페이지에서 **다운로드** 를 선택 하 여 사용자 속성의 올바른 CSV 파일을 받습니다.

   ![삭제 하려는 사용자를 나열 하는 로컬 CSV 파일을 선택 합니다.](./media/users-bulk-delete/bulk-delete.png)

1. CSV 파일을 열고 삭제할 각 사용자에 대 한 줄을 추가 합니다. 유일 하 게 필요한 값은 **사용자 계정 이름**입니다. 그런 다음 파일을 저장합니다.

   ![CSV 파일에는 삭제할 사용자의 이름 및 Id가 포함 되어 있습니다.](./media/users-bulk-delete/delete-csv-file.png)

1. 사용자의 **대량 삭제 (미리 보기)** 페이지에서 **csv 파일 업로드**아래에 있는 파일을 찾습니다. 파일을 선택 하 고 제출을 클릭 하면 CSV 파일의 유효성 검사가 시작 됩니다.
1. 파일 콘텐츠의 유효성 검사가 완료되면 **파일 업로드 성공**이 표시됩니다. 오류가 있는 경우 해당 오류를 해결해야 작업을 제출할 수 있습니다.
1. 파일이 유효성 검사를 통과 하면 **제출** 을 선택 하 여 사용자를 삭제 하는 Azure 대량 작업을 시작 합니다.
1. 삭제 작업이 완료 되 면 대량 작업이 성공 했다는 알림이 표시 됩니다.

오류가 있는 경우 **대량 작업 결과** 페이지에서 결과 파일을 다운로드 하 고 볼 수 있습니다. 파일에는 각 오류의 이유가 포함 되어 있습니다.

## <a name="check-status"></a>상태 확인

**대량 작업 결과 (미리 보기)** 페이지에서 보류 중인 모든 대량 요청의 상태를 볼 수 있습니다.

   ![대량 작업 결과 페이지에서 업로드 상태 확인](./media/users-bulk-delete/bulk-center.png)

다음으로, 사용자가 삭제 한 사용자가 Azure Portal 또는 PowerShell을 사용 하 여 Azure AD 조직에 있는지 확인할 수 있습니다.

## <a name="verify-deleted-users-in-the-azure-portal"></a>Azure Portal에서 삭제 된 사용자 확인

1. 조직에서 사용자 관리자인 계정으로 Azure Portal에 로그인합니다.
1. 탐색 창에서 **Azure Active Directory**를 선택합니다.
1. **관리**에서 **사용자**를 선택합니다.
1. **표시**아래에서 **모든 사용자** 만을 선택 하 고 삭제 한 사용자가 더 이상 나열 되지 않는지 확인 합니다.

### <a name="verify-deleted-users-with-powershell"></a>PowerShell을 사용 하 여 삭제 된 사용자 확인

다음 명령을 실행합니다.

``` PowerShell
Get-AzureADUser -Filter "UserType eq 'Member'"
```

삭제 한 사용자가 더 이상 나열 되지 않는지 확인 합니다.

## <a name="next-steps"></a>다음 단계

- [사용자 일괄 추가](users-bulk-add.md)
- [사용자 목록 다운로드](users-bulk-download.md)
- [사용자 대량 복원](users-bulk-restore.md)
