---
title: ASE v1 소개
description: App Service Environment v1 기능에 대해 알아봅니다. 이 문서는 레거시 v1 ASE를 사용 하는 고객 에게만 제공 됩니다.
author: stefsch
ms.assetid: 78e6d4f5-da46-4eb5-a632-b5fdc17d2394
ms.topic: article
ms.date: 07/11/2017
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: f655b7793bfbb5bbeddfc2f1f8e7bc973dabeb4c
ms.sourcegitcommit: 48b7a50fc2d19c7382916cb2f591507b1c784ee5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/02/2019
ms.locfileid: "74687369"
---
# <a name="introduction-to-app-service-environment-v1"></a>App Service Environment v1 소개

> [!NOTE]
> 이 문서는 ASE(App Service Environment) v1에 관한 내용입니다.  사용하기가 더 쉽고 더 강력한 인프라에서 실행되는 최신 버전의 App Service Environment가 있습니다. 새 버전에 대한 자세한 내용은 [App Service Environment 소개](intro.md)를 참조하세요.

## <a name="overview"></a>개요

App Service Environment는 Web Apps, Mobile Apps 및 API Apps을 포함 하 여 Azure App Service 앱을 높은 규모의 안전 하 게 실행 하기 위해 완전히 격리 된 전용 환경을 제공 하는 [Azure App Service](../overview.md) [프리미엄][PremiumTier] 서비스 계획 옵션입니다.  

App Service Environment는 다음을 필요로 하는 애플리케이션 작업에 적합합니다.

* 매우 높은 확장성
* 격리 및 보안된 네트워크 액세스

고객은 단일 Azure 지역 내뿐만 아니라 여러 Azure 지역에 걸쳐서 여러 App Service Environment를 만들 수 있습니다.  따라서 App Service Environment는 높은 RPS 작업을 지원하여 상태가 없는 애플리케이션 계층을 수평적으로 확장하는 데 적합합니다.

App Service Environment는 단일 고객의 애플리케이션만을 실행하도록 격리되며 항상 가상 네트워크에 배포됩니다.  고객은 인바운드 및 아웃바운드 애플리케이션 네트워크 트래픽 둘 다에 대해 세밀하게 제어할 수 있고 애플리케이션은 가상 네트워크를 통해 온-프레미스 회사 리소스에 고속 보안 연결을 설정할 수 있습니다.

App Service 환경에서 높은 확장성 및 보안 네트워크 액세스를 사용 하는 방법에 대 한 개요는 App Service 환경에서 [AzureCon 심층][AzureConDeepDive] 살펴보기를 참조 하세요.

여러 App Service Environment를 사용하는 수평 크기 조정에 대한 심층 분석은 [지역 분산 앱 메모리 공간][GeodistributedAppFootprint]을 설정하는 방법에 대한 문서를 참조하세요.

AzureCon 심층 분석에 표시된 보안 아키텍처를 구성하는 방법을 보려면 App Service Environment를 사용하여 [계층화된 보안 아키텍처](app-service-app-service-environment-layered-security.md) 를 구현하는데 대한 문서를 참조하세요.

App Service Environment에서 실행 중인 앱은 웹 애플리케이션 방화벽 (WAF) 등의 업스트림 디바이스에서 제어된 액세스를 가질 수 있습니다.  [App Service Environment에 대한 WAF 구성](app-service-app-service-environment-web-application-firewall.md) 의 문서는 이 시나리오에 대해 설명합니다.

[!INCLUDE [app-service-web-to-api-and-mobile](../../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="dedicated-compute-resources"></a>전용 Compute 리소스

App Service Environment의 모든 컴퓨팅 리소스는 전적으로 단일 구독 전용이며 App Service Environment는 단일 애플리케이션 전용으로 최대 50개 컴퓨팅 리소스로 구성될 수 있습니다.

App Service Environment는 1~3개 작업자 컴퓨팅 리소스 풀뿐만 아니라 프런트 엔드 컴퓨팅 리소스 풀로 구성됩니다.

프런트 엔드 풀에는 App Service Environment 내의 앱 요청에 대한 자동 부하 분산뿐 아니라 SSL 종료를 담당하는 컴퓨팅 리소스가 포함되어 있습니다.

각 작업자 풀에는 하나 이상의 Azure App Service 앱을 포함 하는 [App Service 계획][AppServicePlan]에 할당 된 계산 리소스가 포함 되어 있습니다.  App Service Environment에는 최대 세 가지 작업자 풀을 둘 수 있으므로 작업자 풀마다 다른 컴퓨팅 리소스를 유연하게 선택할 수 있습니다.  

예를 들어 앱 개발 또는 테스트용 App Service 계획에 사용하기 위해 성능이 낮은 컴퓨팅 리소스가 포함된 하나의 작업자 풀을 만들 수 있습니다.  두 번째(또는 세 번째) 작업자 풀은 프로덕션 앱을 실행하는 App Service 계획에 성능이 보다 뛰어난 컴퓨팅 리소스를 사용할 수 있습니다.

프런트 엔드 및 작업자 풀에서 사용할 수 있는 계산 리소스의 양에 대 한 자세한 내용은 [App Service Environment 구성 하는 방법][HowToConfigureanAppServiceEnvironment]을 참조 하세요.  

App Service Environment에서 지원 되는 사용 가능한 계산 리소스 크기에 대 한 자세한 내용은 [App Service 가격][AppServicePricing] 책정 페이지를 참조 하 고 프리미엄 가격 책정 계층에서 App Service 환경에 대 한 사용 가능한 옵션을 검토 하세요.

## <a name="virtual-network-support"></a>Virtual Network 지원

App Service Environment은 Azure Resource Manager 가상 네트워크 **또는** 클래식 배포 모델 가상 네트워크 ([가상 네트워크에 대 한 추가 정보][MoreInfoOnVirtualNetworks] **)에서 만들** 수 있습니다.  App Service Environment는 항상 가상 네트워크, 더 정확히 말하자면 가상 네트워크의 서브넷 내에 존재하므로 가상 네트워크의 보안 기능을 활용하여 인바운드 및 아웃바운드 네트워크 통신을 모두 제어할 수 있습니다.  

App Service Environment는 공개 IP 주소가 있는 인터넷 연결이거나 Azure ILB(내부 부하 분산 장치) 주소만 있는 내부 연결일 수 있습니다.

[네트워크 보안 그룹][NetworkSecurityGroups] 을 사용 하 여 App Service Environment 있는 서브넷에 대 한 인바운드 네트워크 통신을 제한할 수 있습니다.  이 옵션을 통해 웹 애플리케이션 방화벽 및 SaaS 공급자와 같은 업스트림 디바이스 및 서비스 뒤에서 앱을 실행할 수 있습니다.

또한 앱에서는 내부 데이터베이스 및 웹 서비스와 같은 회사 리소스에 자주 액세스해야 합니다.  일반적인 접근 방법은 Azure 가상 네트워크 내에서 이동하는 내부 네트워크 트래픽에만 이러한 엔드포인트를 사용할 수 있도록 하는 것입니다.  App Service Environment 내부 서비스와 동일한 가상 네트워크에 조인 되 면 환경에서 실행 중인 앱은 [사이트 간][SiteToSite] 및 [Azure express][ExpressRoute] 경로 연결을 통해 도달할 수 있는 끝점을 포함 하 여 액세스할 수 있습니다.

App Service 환경이 가상 네트워크와 온-프레미스 네트워크에서 작동 하는 방식에 대 한 자세한 내용은 [네트워크 아키텍처][NetworkArchitectureOverview]에서 [인바운드 트래픽 제어][ControllingInboundTraffic], [백 엔드에 안전 하 게 연결][SecurelyConnectingToBackends]에 대 한 다음 문서를 참조 하세요. 

## <a name="getting-started"></a>시작

App Service 환경을 시작 하려면 [App Service Environment를 만드는 방법][HowToCreateAnAppServiceEnvironment] 을 참조 하세요.

App Service Environment 네트워크 아키텍처에 대 한 개요는 [네트워크 아키텍처 개요][NetworkArchitectureOverview] 문서를 참조 하세요.

Express 경로에 App Service Environment를 사용 하는 방법에 대 한 자세한 내용은 [Express 경로 및 App Service 환경][NetworkConfigDetailsForExpressRoute]에 대 한 다음 문서를 참조 하세요.

[!INCLUDE [app-service-web-try-app-service](../../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[PremiumTier]: https://azure.microsoft.com/pricing/details/app-service/
[MoreInfoOnVirtualNetworks]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePlan]: ../overview-hosting-plans.md
[HowToCreateAnAppServiceEnvironment]: app-service-web-how-to-create-an-app-service-environment.md
[LogicApps]: https://azure.microsoft.com/documentation/articles/app-service-logic-what-are-logic-apps/
[AzureConDeepDive]:  https://azure.microsoft.com/documentation/videos/azurecon-2015-deploying-highly-scalable-and-secure-web-and-mobile-apps/
[GeodistributedAppFootprint]:  app-service-app-service-environment-geo-distributed-scale.md
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[SiteToSite]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-multi-site
[ExpressRoute]: https://azure.microsoft.com/services/expressroute/
[HowToConfigureanAppServiceEnvironment]:  app-service-web-configure-an-app-service-environment.md
[ControllingInboundTraffic]:  app-service-app-service-environment-control-inbound-traffic.md
[SecurelyConnectingToBackends]:  app-service-app-service-environment-securely-connecting-to-backend-resources.md
[NetworkArchitectureOverview]:  app-service-app-service-environment-network-architecture-overview.md
[NetworkConfigDetailsForExpressRoute]:  app-service-app-service-environment-network-configuration-expressroute.md
[AppServicePricing]: https://azure.microsoft.com/pricing/details/app-service/ 

<!-- IMAGES -->