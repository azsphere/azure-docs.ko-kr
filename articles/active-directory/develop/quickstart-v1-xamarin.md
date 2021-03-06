---
title: Azure AD Xamarin 시작 | Microsoft Docs
description: 로그인을 위해 Azure AD와 통합되고 OAuth를 사용하여 Azure AD로 보호되는 API를 호출하는 Xamarin 애플리케이션을 빌드합니다.
services: active-directory
author: rwike77
manager: CelesteDG
ms.assetid: 198cd2c3-f7c8-4ec2-b59d-dfdea9fe7d95
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 07/17/2019
ms.author: ryanwi
ms.reviewer: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 26eff9456ba3ee1e2ff5ea41a552b4ec90a4fd1f
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/23/2020
ms.locfileid: "76703697"
---
# <a name="quickstart-build-a-xamarin-app-that-integrates-microsoft-sign-in"></a>빠른 시작: Microsoft 로그인을 통합하는 Xamarin 앱 빌드

[Microsoft ID 플랫폼](v2-overview.md)은 Azure AD(Azure Active Directory) 개발자 플랫폼의 발전된 형태입니다. 이 플랫폼을 사용하면 개발자가 모든 Microsoft ID에 로그인하고, Microsoft Graph와 같은 Microsoft API 또는 개발자가 빌드한 API를 호출하기 위한 토큰을 가져오는 애플리케이션을 빌드할 수 있습니다.

[MSAL(Microsoft 인증 라이브러리)](msal-overview.md)을 통해 개발자는 보안 웹 API에 액세스하기 위해 Microsoft ID 플랫폼 엔드포인트에서 토큰을 획득할 수 있습니다. ADAL(Active Directory 인증 라이브러리)은 개발자용 Azure AD(v1.0) 엔드포인트와 통합되며, MSAL은 Microsoft ID 플랫폼(v2.0) 엔드포인트와 통합됩니다.

## <a name="next-steps"></a>다음 단계

새 Xamarin 애플리케이션의 경우 Microsoft ID 플랫폼(v2.0) 및 MSAL을 사용하여 토큰을 획득하고 보안 웹 API에 액세스하는 것이 좋습니다. 시작하려면 [MSAL을 사용하여 Microsoft ID 및 Microsoft Graph를 Xamarin 양식 앱에 통합](https://github.com/azure-samples/active-directory-xamarin-native-v2#integrate-microsoft-identity-and-the-microsoft-graph-into-a-xamarin-forms-app-using-msal)(선택적 단계 제외)을 참조하세요.
