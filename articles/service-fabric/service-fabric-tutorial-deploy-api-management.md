---
title: Azure에서 Service Fabric과 API Management 통합
description: Azure API Management를 빠르게 시작하고 Service Fabric에서 백 엔드 서비스로 트래픽을 라우팅하는 방법을 알아봅니다.
ms.topic: conceptual
ms.date: 07/10/2019
ms.custom: mvc
ms.openlocfilehash: 201d617ce15216ba168bc484f644e165d5ae0e71
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75465349"
---
# <a name="integrate-api-management-with-service-fabric-in-azure"></a>Azure에서 Service Fabric과 API Management 통합

Service Fabric을 사용한 Azure API Management 배포는 고급 시나리오입니다.  API Management는 백 엔드 Service Fabric 서비스에 대한 풍부한 라우팅 규칙 집합을 API를 게시해야 할 경우에 유용합니다. 일반적으로 클라우드 애플리케이션에는 사용자, 디바이스 또는 기타 애플리케이션 수신을 위한 단일 지점을 제공하는 프런트 엔드 게이트웨이가 필요합니다. Service Fabric에서 게이트웨이는 ASP.NET Core 애플리케이션, Event Hubs, IoT Hub 또는 Azure API Management와 같이 트래픽 수신용으로 설계된 상태 비저장 서비스일 수 있습니다.

이 문서에서는 Service Fabric을 사용하여 [Azure API Management](../api-management/api-management-key-concepts.md)를 설정하여 Service Fabric의 백 엔드 서비스로 트래픽을 라우팅하는 방법을 보여줍니다.  작업을 완료한 경우 VNET에 API Management가 배포되고, 백 엔드 상태 비저장 서비스에 트래픽을 전송하도록 API 작업이 구성됩니다. Service Fabric을 사용하는 Azure API Management 시나리오에 대해 자세히 알아보려면 [개요](service-fabric-api-management-overview.md) 문서를 참조하세요.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="availability"></a>가용성

> [!IMPORTANT]
> 이 기능은 필수 가상 네트워크 지원으로 인해 API Management의 **프리미엄** 및 **개발자** 계층에서 사용할 수 있습니다.

## <a name="prerequisites"></a>필수 조건

시작하기 전에

* Azure 구독이 없는 경우 [무료 계정](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)을 만듭니다.
* [Azure Powershell](https://docs.microsoft.com/powershell/azure/install-Az-ps) 또는 [Azure CLI](/cli/azure/install-azure-cli)를 설치합니다.
* 네트워크 보안 그룹에서 보안 [Windows 클러스터](service-fabric-tutorial-create-vnet-and-windows-cluster.md)를 만듭니다.
* Windows 클러스터를 배포하는 경우 Windows 개발 환경을 설정합니다. [Visual Studio 2019](https://www.visualstudio.com), **Azure 개발**, **ASP.NET 및 웹 개발** 및 **.NET Core 플랫폼 간 개발** 워크로드를 설치합니다.  그런 후 [.NET 개발 환경](service-fabric-get-started.md)을 설정합니다.

## <a name="network-topology"></a>네트워크 토폴로지

이제 Azure에 보안 [Windows 클러스터](service-fabric-tutorial-create-vnet-and-windows-cluster.md)가 있으므로, 서브넷의 VNET(가상 네트워크) 및 API Management에 지정된 NSG에 API Management를 배포합니다. 이 문서의 경우 API Management Resource Manager 템플릿이 [Windows 클러스터 자습서](service-fabric-tutorial-create-vnet-and-windows-cluster.md)에서 설정한 VNET, 서브넷 및 NSG의 이름을 사용하도록 미리 구성됐으므로 이 문서에서는 API Management 및 Service Fabric이 동일한 Virtual Network의 서브넷에 있는 경우 Azure에 다음과 같은 토폴로지를 배포합니다.

 ![그림 캡션][sf-apim-topology-overview]

## <a name="sign-in-to-azure-and-select-your-subscription"></a>Azure에 로그인하고 구독 선택

Azure 명령을 실행하기 전에 Azure 계정에 로그인하고 구독을 선택합니다.

```powershell
Connect-AzAccount
Get-AzSubscription
Set-AzContext -SubscriptionId <guid>
```

```azurecli
az login
az account set --subscription <guid>
```

## <a name="deploy-a-service-fabric-back-end-service"></a>Service Fabric 백 엔드 서비스 배포

Service Fabric 백 엔드 서비스로 트래픽을 라우팅하도록 API Management를 구성하기 전에 먼저 요청을 수락하기 위한 실행 중인 서비스가 필요합니다.  

기본 웹 API 프로젝트 템플릿을 사용하여 기본적인 상태 비저장 ASP.NET Core 신뢰할 수 있는 서비스를 만듭니다. 이렇게 하면 서비스의 HTTP 엔드포인트가 Azure API Management를 통해 노출됩니다.

Visual Studio를 관리자 권한으로 시작하고 ASP.NET Core 서비스를 만듭니다.

 1. Visual Studio에서 파일 -> 새 프로젝트를 선택합니다.
 2. 클라우드에서 Service Fabric 애플리케이션 템플릿을 선택하고 이름을 **"ApiApplication"** 으로 지정합니다.
 3. 상태 비저장 ASP.NET Core 서비스 템플릿을 선택하고 프로젝트 이름을 **"WebApiService"** 로 지정합니다.
 4. Web API ASP.NET Core 2.1 프로젝트 템플릿을 선택 합니다.
 5. 프로젝트가 만들어지면 `PackageRoot\ServiceManifest.xml`을 열고 엔드포인트 리소스 구성에서 `Port` 특성을 제거합니다.

    ```xml
    <Resources>
      <Endpoints>
        <Endpoint Protocol="http" Name="ServiceEndpoint" Type="Input" />
      </Endpoints>
    </Resources>
    ```

    포트를 제거 하면 Service Fabric는 클러스터 리소스 관리자 템플릿에서 네트워크 보안 그룹을 통해 연 응용 프로그램 포트 범위에서 동적으로 포트를 지정 하 여 트래픽이 API Management에서 전송 될 수 있도록 합니다.

 6. Web API를 로컬에서 사용할 수 있는지 확인하려면 Visual Studio에서 F5 키를 누릅니다.

    Service Fabric Explorer를 열고 ASP.NET Core 서비스의 특정 인스턴스로 드릴다운하여 서비스가 수신 대기 중인 기준 주소를 확인합니다. 기준 주소에 `/api/values`를 추가하고 브라우저에서 엽니다. 그러면 웹 API 템플릿에서 ValuesController에 대해 Get 메서드가 호출됩니다. 두 개의 문자열을 포함하는 JSON 배열인 템플릿에서 제공하는 기본 응답을 반환합니다.

    ```json
    ["value1", "value2"]`
    ```

    Azure에서 API Management를 통해 노출될 엔드포인트입니다.

 7. 마지막으로, Azure에서 애플리케이션을 클러스터에 배포합니다. Visual Studio에서 애플리케이션 프로젝트를 마우스 오른쪽 단추로 클릭하고 **게시**를 선택합니다. 클러스터 엔드포인트(예: `mycluster.southcentralus.cloudapp.azure.com:19000`)를 제공하여 Azure에서 Service Fabric 클러스터에 애플리케이션을 배포합니다.

`fabric:/ApiApplication/WebApiService`로 명명된 ASP.NET Core 상태 비저장 서비스는 이제 Azure의 Service Fabric 클러스터에서 실행되어야 합니다.

## <a name="download-and-understand-the-resource-manager-templates"></a>리소스 관리자 템플릿 다운로드 및 이해

다음 리소스 관리자 템플릿 및 매개 변수 파일을 다운로드하고 저장합니다.

* [네트워크-apim. json][network-arm]
* [네트워크-apim. parameters. json][network-parameters-arm]
* [apim. json][apim-arm]
* [apim. parameters. json][apim-parameters-arm]

*network-apim.json* 템플릿은 Service Fabric 클러스터가 배포된 가상 네트워크에 새로운 서브넷 및 네트워크 보안 그룹을 배포합니다.

다음 섹션에서는 *apim.json* 템플릿에 의해 정의되는 리소스에 대해 설명합니다. 자세한 내용은 각 섹션 내의 템플릿 참조 설명서 링크를 클릭합니다. *apim.parameters.json* 매개 변수 파일에 정의된 구성 가능한 매개 변수는 이 문서의 뒷부분에서 설정됩니다.

### <a name="microsoftapimanagementservice"></a>Microsoft.ApiManagement/service

[Microsoft.ApiManagement/service](/azure/templates/microsoft.apimanagement/service)에서는 API Management 서비스 인스턴스의 이름, SKU 또는 계층, 리소스 그룹 위치, 게시자 정보, 가상 네트워크 등에 대해 설명합니다.

### <a name="microsoftapimanagementservicecertificates"></a>Microsoft.ApiManagement/service/certificates

[Microsoft.ApiManagement/service/certificates](/azure/templates/microsoft.apimanagement/service/certificates)에서는 API Management 보안을 구성합니다. API Management는 서비스 검색을 위해 클러스터에 대한 액세스 권한이 있는 클라이언트 인증서를 사용하여 Service Fabric 클러스터에 인증해야 합니다. 이 문서에서는 기본적으로 클러스터 액세스에 사용할 수 있는 [Windows 클러스터](service-fabric-tutorial-create-vnet-and-windows-cluster.md#createvaultandcert_anchor)를 만들 때 이전에 지정된 것과 동일한 인증서를 사용합니다.

이 문서에서는 클라이언트 인증 및 클러스터 노드 간 보안에 동일한 인증서를 사용합니다. Service Fabric 클러스터에 액세스하도록 구성되어 있는 경우 별도의 클라이언트 인증서를 사용할 수 있습니다. Service Fabric 클러스터를 만들 때 지정한 클러스터 인증서의 프라이빗 키 파일(.pfx)에 대해 **이름**, **암호** 및 **데이터**(base-64로 인코딩된 문자열)를 제공합니다.

### <a name="microsoftapimanagementservicebackends"></a>Microsoft.ApiManagement/service/backends

[Microsoft.ApiManagement/service/backends](/azure/templates/microsoft.apimanagement/service/backends)에서는 트래픽이 전달되는 백 엔드 서비스에 대해 설명합니다.

Service Fabric 백 엔드의 경우 특정 Service Fabric 서비스가 아니라 Service Fabric 클러스터가 백 엔드입니다. 이렇게 하면 단일 정책에서 클러스터에 있는 둘 이상의 서비스로 라우팅할 수 있습니다. 여기서 **url** 필드는 백 엔드 정책에 지정된 서비스 이름이 없는 경우 기본적으로 모든 요청이 라우팅되는 클러스터 서비스의 정규화된 서비스 이름입니다. 대체 서비스를 포함하지 않으려는 경우 "fabric:/fake/service" 같은 가짜 서비스 이름을 사용할 수 있습니다. **resourceId**는 클러스터 관리 엔드포인트를 지정합니다.  **clientCertificateThumbprint** 및 **serverCertificateThumbprints**는 클러스터로 인증받는 데 사용되는 인증서를 식별합니다.

### <a name="microsoftapimanagementserviceproducts"></a>Microsoft.ApiManagement/service/products

[Microsoft.ApiManagement/service/products](/azure/templates/microsoft.apimanagement/service/products)에서는 제품을 만듭니다. Azure API Management에서 제품은 하나 이상의 API뿐만 아니라 사용 할당량 및 사용 약관을 포함합니다. 제품이 게시되면 개발자는 제품을 구독하고 제품의 API를 사용할 수 있습니다.

제품을 설명하는 **displayName** 및 **description**을 입력합니다. 이 문서에서는 구독이 필요하지만 관리자의 구독 승인은 필요하지 않습니다.  이 제품의 **state**는 "게시됨"이며 구독자에게 표시됩니다.

### <a name="microsoftapimanagementserviceapis"></a>Microsoft.ApiManagement/service/apis

[Microsoft.ApiManagement/service/apis](/azure/templates/microsoft.apimanagement/service/apis)에서는 API를 만듭니다. API Management에서 API는 클라이언트 애플리케이션이 호출할 수 있는 작업 집합을 나타냅니다. 작업이 추가되면 API가 제품에 추가되므로, 이 API를 게시할 수 있습니다. API가 게시되면 개발자가 구독하고 사용할 수 있습니다.

* **displayName**은 API에 대한 어떤 이름도 될 수 있습니다. 이 문서에서는 "Service Fabric 앱"을 사용합니다.
* **name**은 API를 설명하는 고유한 이름(예: "service-fabric-app")을 제공합니다. 개발자 및 게시자 포털에 표시됩니다.
* **serviceUrl**은 API를 구현하는 HTTP 서비스를 참조합니다. API 관리는 이 주소로 요청을 전달합니다. Service Fabric 백 엔드에는 이 URL 값이 사용되지 않습니다. 여기에 임의의 값을 입력할 수 있습니다. 이 문서의 경우 "http:\//servicefabric"와 같습니다.
* **path**는 API Management 서비스의 기본 URL에 추가됩니다. 기본 URL은 API Management 서비스 인스턴스에서 호스트되는 모든 API에 공통으로 사용됩니다. API Management는 접미사를 사용하여 API를 구분하므로, 접미사는 지정된 게시자의 모든 API에 대해 고유해야 합니다.
* **protocols**는 API에 액세스하는 데 사용할 수 있는 프로토콜을 결정합니다. 이 문서에서는 **http** 및 **https**가 나열됩니다.
* **path**는 API에 대한 접미사입니다. 이 문서에서는 "myapp"을 사용합니다.

### <a name="microsoftapimanagementserviceapisoperations"></a>Microsoft.ApiManagement/service/apis/operations

[Microsoft.ApiManagement/service/apis/operations](/azure/templates/microsoft.apimanagement/service/apis/operations) API Management에서 API를 사용하려면 API에 먼저 작업을 추가해야 합니다.  외부 클라이언트는 Service Fabric 클러스터에서 실행되는 ASP.NET Core 상태 비저장 서비스와 통신하기 위해 작업을 사용합니다.

프런트 엔드 API 작업을 추가하려면 다음 값을 채웁니다.

* **displayName** 및 **description**은 작업에 대해 설명합니다. 이 문서에서는 "Values"를 사용합니다.
* **method**는 HTTP 동사를 지정합니다.  이 문서에서는 **GET**을 지정합니다.
* **urlTemplate**은 API의 기준 URL에 추가되며 단일 HTTP 작업을 식별합니다.  이 문서에서는 .NET 백 엔드 서비스를 추가한 경우에는 `/api/values`를, Java 백 엔드 서비스를 추가한 경우에는 `getMessage`를 사용합니다.  기본적으로 여기에 지정된 URL 경로는 백 엔드 Service Fabric 서비스로 전송되는 URL 경로입니다. 여기에 서비스에서 사용하는 것과 동일한 URL 경로(예: "/api/values")를 사용하면 추가 수정 없이 작업이 이루어집니다. 여기에 백 엔드 Service Fabric 서비스에서 사용하는 URL 경로와 다른 URL 경로를 지정할 수도 있습니다. 이 경우 나중에 작업 정책에서도 경로 다시 쓰기를 지정해야 합니다.

### <a name="microsoftapimanagementserviceapispolicies"></a>Microsoft.ApiManagement/service/apis/policies

[Microsoft.ApiManagement/service/apis/policies](/azure/templates/microsoft.apimanagement/service/apis/policies)에서는 모든 항목을 함께 연결하는 백 엔드 정책을 만듭니다. 요청이 라우팅되는 백 엔드 Service Fabric 서비스를 구성하는 위치입니다. 이 정책은 모든 API 작업에 적용할 수 있습니다.  자세한 내용은 [정책 개요](/azure/api-management/api-management-howto-policies)를 참조하세요.

[Service Fabric에 대한 백 엔드 구성](/azure/api-management/api-management-transformation-policies#SetBackendService)은 다음 요청 라우팅 컨트롤을 제공합니다.

* 하드 코드되거나(예: `"fabric:/myapp/myservice"`) HTTP 요청에서 생성된(예: `"fabric:/myapp/users/" + context.Request.MatchedParameters["name"]`) Service Fabric 서비스 인스턴스 이름을 지정하여 서비스 인스턴스 선택
* Service Fabric 분할 체계를 통해 파티션 키를 생성하여 파티션 확인
* 상태 저장 서비스 복제본 선택
* 서비스 위치를 다시 확인하고 요청을 다시 보내는 조건을 지정할 수 있는 다시 확인 시도 조건

**policyContent**는 정책의 Json 이스케이프 XML 콘텐츠입니다.  이 문서에서는 이전에 배포한 .NET 또는 Java 상태 비저장 서비스로 직접 요청을 라우팅하는 백 엔드 정책을 만듭니다. 인바운드 정책 아래에 `set-backend-service` 정책을 추가합니다.  이전에 .NET 백 엔드 서비스를 배포한 경우에는 *sf-service-instance-name* 값을 `fabric:/ApiApplication/WebApiService`로 바꾸고, Java 서비스를 배포한 경우에는 `fabric:/EchoServerApplication/EchoServerService`로 바꿉니다.  *backend-id*는 백 엔드 리소스를 참조하며, 이 경우에는 *apim.json* 템플릿에 정의된 `Microsoft.ApiManagement/service/backends` 리소스입니다. *backend-id*는 또한 API Management API를 사용하여 생성된 다른 백 엔드 리소스를 참조할 수 있습니다. 이 문서에서는 *backend-id*를 *service_fabric_backend_name* 매개 변수 값으로 설정합니다.

```xml
<policies>
  <inbound>
    <base/>
    <set-backend-service
        backend-id="servicefabric"
        sf-service-instance-name="service-name"
        sf-resolve-condition="@(context.LastError?.Reason == "BackendConnectionFailure")" />
  </inbound>
  <backend>
    <base/>
  </backend>
  <outbound>
    <base/>
  </outbound>
</policies>
```

전체 Service Fabric 백 엔드 정책 특성 집합은 [API Management 백 엔드 설명서](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#SetBackendService)를 참조하세요.

## <a name="set-parameters-and-deploy-api-management"></a>매개 변수 설정 및 API Management 배포

사용자 배포의 *cluster.parameters.json*에 다음의 빈 매개 변수를 입력합니다.

|매개 변수|값|
|---|---|
|apimInstanceName|sf-apim|
|apimPublisherEmail|myemail@contosos.com|
|apimSku|개발자|
|serviceFabricCertificateName|sfclustertutorialgroup320171031144217|
|certificatePassword|q6D7nN%6ck@6|
|serviceFabricCertificateThumbprint|C4C1E541AD512B8065280292A8BA6079C3F26F10 |
|serviceFabricCertificate|&lt;base-64로 인코딩된 문자열&gt;|
|url_path|/api/values|
|clusterHttpManagementEndpoint|https://mysfcluster.southcentralus.cloudapp.azure.com:19080|
|inbound_policy|&lt;XML 문자열&gt;|

*certificatePassword* 및 *serviceFabricCertificateThumbprint*는 클러스터를 설정하는 데 사용되는 클러스터 인증서와 일치해야 합니다.

*serviceFabricCertificate*는 다음 스크립트를 사용하여 생성할 수 있는 base-64로 인코딩된 문자열로 나타낸 인증서입니다.

```powershell
$bytes = [System.IO.File]::ReadAllBytes("C:\mycertificates\sfclustertutorialgroup220171109113527.pfx");
$b64 = [System.Convert]::ToBase64String($bytes);
[System.Io.File]::WriteAllText("C:\mycertificates\sfclustertutorialgroup220171109113527.txt", $b64);
```

*inbound_policy*에서 이전에 .NET 백 엔드 서비스를 배포한 경우에는 *sf-service-instance-name* 값을 `fabric:/ApiApplication/WebApiService`로 바꾸고, Java 서비스를 배포한 경우에는 `fabric:/EchoServerApplication/EchoServerService`로 바꿉니다. *backend-id*는 백 엔드 리소스를 참조하며, 이 경우에는 *apim.json* 템플릿에 정의된 `Microsoft.ApiManagement/service/backends` 리소스입니다. *backend-id*는 또한 API Management API를 사용하여 생성된 다른 백 엔드 리소스를 참조할 수 있습니다. 이 문서에서는 *backend-id*를 *service_fabric_backend_name* 매개 변수 값으로 설정합니다.

```xml
<policies>
  <inbound>
    <base/>
    <set-backend-service
        backend-id="servicefabric"
        sf-service-instance-name="service-name"
        sf-resolve-condition="@(context.LastError?.Reason == "BackendConnectionFailure")" />
  </inbound>
  <backend>
    <base/>
  </backend>
  <outbound>
    <base/>
  </outbound>
</policies>
```

다음 스크립트를 사용하여 API Management에 대해 Resource Manager 템플릿 및 매개 변수 파일을 배포합니다.

```powershell
$groupname = "sfclustertutorialgroup"
$clusterloc="southcentralus"
$templatepath="C:\clustertemplates"

New-AzResourceGroupDeployment -ResourceGroupName $groupname -TemplateFile "$templatepath\network-apim.json" -TemplateParameterFile "$templatepath\network-apim.parameters.json" -Verbose

New-AzResourceGroupDeployment -ResourceGroupName $groupname -TemplateFile "$templatepath\apim.json" -TemplateParameterFile "$templatepath\apim.parameters.json" -Verbose
```

```azurecli
ResourceGroupName="sfclustertutorialgroup"
az group deployment create --name ApiMgmtNetworkDeployment --resource-group $ResourceGroupName --template-file network-apim.json --parameters @network-apim.parameters.json

az group deployment create --name ApiMgmtDeployment --resource-group $ResourceGroupName --template-file apim.json --parameters @apim.parameters.json
```

## <a name="test-it"></a>테스트

이제 [Azure Portal](https://portal.azure.com)에서 직접 API Management를 통해 Service Fabric의 백 엔드 서비스에 요청을 전송하는 것을 시도해 볼 수 있습니다.

 1. API Management 서비스에서 **API**를 선택합니다.
 2. 이전 단계에서 만든 **Service Fabric 앱** API에서 **테스트** 탭 및 **값** 작업을 차례로 선택합니다.
 3. **전송** 단추를 클릭하여 백 엔드 서비스로 테스트 요청을 보냅니다.  다음과 유사한 HTTP 응답이 표시됩니다.

    ```http
    HTTP/1.1 200 OK

    Transfer-Encoding: chunked

    Content-Type: application/json; charset=utf-8

    Vary: Origin

    Ocp-Apim-Trace-Location: https://apimgmtstodhwklpry2xgkdj.blob.core.windows.net/apiinspectorcontainer/PWSQOq_FCDjGcaI1rdMn8w2-2?sv=2015-07-08&sr=b&sig=MhQhzk%2FEKzE5odlLXRjyVsgzltWGF8OkNzAKaf0B1P0%3D&se=2018-01-28T01%3A04%3A44Z&sp=r&traceId=9f8f1892121e445ea1ae4d2bc8449ce4

    Date: Sat, 27 Jan 2018 01:04:44 GMT


    ["value1", "value2"]
    ```

## <a name="clean-up-resources"></a>리소스 정리

클러스터는 클러스터 리소스 외에도 다른 Azure 리소스로 이루어져 있습니다. 클러스터 및 클러스터에서 사용하는 모든 리소스를 삭제하는 가장 간단한 방법은 리소스 그룹을 삭제하는 것입니다.

Azure에 로그인하고, 클러스터를 제거하려는 구독 ID를 선택합니다.  [Azure Portal](https://portal.azure.com)에 로그인하여 구독 ID를 찾을 수 있습니다. [AzResourceGroup cmdlet](/en-us/powershell/module/az.resources/remove-azresourcegroup)을 사용 하 여 리소스 그룹 및 모든 클러스터 리소스를 삭제 합니다.

```powershell
$ResourceGroupName = "sfclustertutorialgroup"
Remove-AzResourceGroup -Name $ResourceGroupName -Force
```

```azurecli
ResourceGroupName="sfclustertutorialgroup"
az group delete --name $ResourceGroupName
```

## <a name="next-steps"></a>다음 단계

[API Management](/azure/api-management/import-and-publish)를 사용하는 방법을 자세히 알아봅니다.

[azure-powershell]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/

[apim-arm]:https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/service-integration/apim.json
[apim-parameters-arm]:https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/service-integration/apim.parameters.json

[network-arm]: https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/service-integration/network-apim.json
[network-parameters-arm]: https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/service-integration/network-apim.parameters.json

<!-- pics -->
[sf-apim-topology-overview]: ./media/service-fabric-tutorial-deploy-api-management/sf-apim-topology-overview.png
vice-fabric-scripts-and-templates/blob/master/templates/service-integration/network-apim.parameters.jsonn

<!-- pics -->
[sf-apim-topology-overview]: ./media/service-fabric-tutorial-deploy-api-management/sf-apim-topology-overview.png
