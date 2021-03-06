---
title: SSL 연결-Azure Database for MySQL
description: SSL 연결을 올바르게 사용하도록 MySQL용 Azure 데이터베이스 및 연결된 애플리케이션을 구성하는 방법에 대한 정보
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 01/13/2020
ms.openlocfilehash: 6c1f52c69104e19ff0a7a7cae255dd7029e1a4b8
ms.sourcegitcommit: b5106424cd7531c7084a4ac6657c4d67a05f7068
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75941786"
---
# <a name="ssl-connectivity-in-azure-database-for-mysql"></a>MySQL용 Azure 데이터베이스의 SSL 연결

MySQL용 Azure 데이터베이스는 SSL(Secure Sockets Layer)을 사용하여 데이터베이스 서버를 클라이언트 애플리케이션에 연결하도록 지원합니다. 데이터베이스 서버와 클라이언트 애플리케이션 간 SSL 연결을 적용하면 서버와 애플리케이션 간 데이터 스트림을 암호화함으로써 “메시지 가로채기(man in the middle)” 공격으로부터 보호할 수 있습니다.

## <a name="default-settings"></a>기본 설정

기본적으로 데이터베이스 서비스는 MySQL에 연결할 때 SSL 연결이 필요하도록 구성되어야 합니다.  가능하면 SSL 옵션을 해제하지 않는 것이 좋습니다.

Azure Portal 및 CLI를 통해 새로운 MySQL용 Azure 데이터베이스 서버를 프로비전할 때 SSL 연결 적용이 기본적으로 활성화됩니다. 

다양한 프로그래밍 언어에 대한 연결 문자열이 Azure Portal에 표시됩니다. 이러한 연결 문자열에는 데이터베이스에 연결하는 데 필요한 SSL 매개 변수가 포함되어 있습니다. Azure Portal에서 VM을 선택합니다. **설정** 제목 아래에서 **연결 문자열**을 선택합니다. SSL 매개 변수는 “ssl=true” 또는 “sslmode=require” 또는 “sslmode=required” 및 다른 변형과 같은 커넥터에 따라 달라집니다.

> [!NOTE]
> 현재 Azure Database for MySQL에 대해 지원 되는 TLS 버전은 TLS 1.0, TLS 1.1, TLS 1.2입니다.

애플리케이션을 개발할 때 SSL 연결을 사용하거나 사용하지 않도록 설정하는 방법을 알아보려면 [SSL 구성 방법](howto-configure-ssl.md)을 참조하세요.

## <a name="next-steps"></a>다음 단계

[MySQL용 Azure 데이터베이스에 대한 연결 라이브러리](concepts-connection-libraries.md)
