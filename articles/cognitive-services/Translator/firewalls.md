---
title: 방화벽 뒤에서 변환 - Translator Text API
titleSuffix: Azure Cognitive Services
description: Azure Cognitive Services Translator Text API는 도메인 이름 또는 IP 필터링을 사용 하 여 방화벽 뒤에서 변환할 수 있습니다.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: swmachan
ms.openlocfilehash: cd7904fedd3ab3f64315cb6f98d99b8fd12254f6
ms.sourcegitcommit: 35715a7df8e476286e3fee954818ae1278cef1fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/08/2019
ms.locfileid: "73837398"
---
# <a name="how-to-translate-behind-ip-firewalls-with-the-translator-text-api"></a>Translator Text API를 사용하여 IP 방화벽 뒤에서 변환하는 방법

Translator Text API는 도메인 이름 또는 IP 필터링을 사용하여 방화벽 뒤에서 변환할 수 있습니다. 사람들이 선호하는 방법은 도메인 이름 필터링입니다. IP로 필터링되는 방화벽 뒤에서 Microsoft Translator를 실행하는 것은 **좋지 않습니다**. 나중에 공지 없이 설치가 중단될 수 있습니다.

## <a name="translator-ip-addresses"></a>Translator IP 주소
Api.cognitive.microsofttranslator.com에 대 한 IP 주소-2019 년 8 월 21 일 Translator Text API:

* **아시아 태평양:** 20.40.125.208, 20.43.88.240, 20.184.58.62, 40.90.139.163, 104.44.89.44
* **유럽:** 40.90.138.4, 40.90.141.99, 51.105.170.64, 52.155.218.251
* **북아메리카:** 40.90.139.36, 40.90.139.2, 40.119.2.134, 52.224.200.129, 52.249.207.163


## <a name="next-steps"></a>다음 단계
> [!div class="nextstepaction"]
> [Translator API 호출의 IP 방화벽 뒤에 변환](reference/v3-0-translate.md)
