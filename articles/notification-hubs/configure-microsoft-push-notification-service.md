---
title: Azure Notification Hubs에서 Microsoft 푸시 알림 서비스 구성 | Microsoft Docs
description: Azure 알림 허브에 대 한 Microsoft 푸시 알림 서비스 설정을 구성 하는 방법에 대해 알아봅니다.
services: notification-hubs
author: sethmanheim
manager: femila
editor: jwargo
ms.service: notification-hubs
ms.workload: mobile
ms.topic: article
ms.date: 03/25/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 03/25/2019
ms.openlocfilehash: 0d7bf5410e05bb74a215fb7a95c704673b764b93
ms.sourcegitcommit: 7df70220062f1f09738f113f860fad7ab5736e88
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/24/2019
ms.locfileid: "71212481"
---
# <a name="configure-microsoft-push-notification-service-mpns-settings-for-a-notification-hub-in-the-azure-portal"></a>Azure Portal에서 알림 허브에 대 한 MPNS (Microsoft 푸시 알림 서비스) 설정 구성
이 문서에서는 Azure Portal를 사용 하 여 Azure 알림 허브에 대 한 MPNS (Microsoft 푸시 알림 서비스) 설정을 구성 하는 방법을 보여 줍니다. 

## <a name="prerequisites"></a>사전 요구 사항
알림 허브를 아직 만들지 않은 경우 지금 만듭니다. 자세한 내용은 [Azure Portal에 Azure 알림 허브 만들기](create-notification-hub-portal.md)를 참조하세요. 

## <a name="configure-microsoft-push-notification-service-mpns"></a>MPNS (Microsoft 푸시 알림 서비스) 구성

다음 절차에서는 알림 허브에 대 한 MPNS (Microsoft 푸시 알림 서비스) 설정을 구성 하는 단계를 제공 합니다. 

1. Azure Portal의 **알림 허브** 페이지에서 왼쪽 메뉴에 있는 **Windows Phone (MPNS)** 를 선택 합니다.
1. 인증되지 않았거나 인증된 푸시 알림 사용:

   a. 인증되지 않은 푸시 알림을 사용하도록 설정하려면 **인증되지 않은 푸시 사용** > **저장**을 선택합니다.

      ![인증되지 않은 푸시 알림을 사용하도록 설정하는 방법을 보여주는 스크린샷](./media/notification-hubs-windows-phone-get-started/azure-portal-unauth.png)

   b. 인증된 푸시 알림을 사용하도록 설정하려면 다음을 수행합니다.
      * 도구 모음에서 **인증서 업로드**를 선택합니다.
      * 파일 아이콘을 선택한 다음, 인증서 파일을 선택합니다.
      * 인증서에 대한 암호를 입력합니다.
      * **확인**을 선택합니다.
      * **Windows Phone(MPNS)** 페이지에서 **저장**을 선택합니다.

## <a name="next-steps"></a>다음 단계
Azure Notification Hubs 및 MPNS (Microsoft 푸시 알림 서비스)를 사용 하 여 Windows Phone 장치에 알림을 푸시하는 방법에 대 한 단계별 지침이 포함 된 자습서는 [Notification Hubs를 사용 하 여 Windows Phone 앱에 대 한 푸시 알림](notification-hubs-windows-mobile-push-notifications-mpns.md)을 참조 하세요.

