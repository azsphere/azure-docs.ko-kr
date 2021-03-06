---
title: IoT 플러그 앤 플레이 미리 보기 샘플 디바이스 코드를 IoT Hub에 연결(Windows) | Microsoft Docs
description: Windows에서 IoT Hub에 연결하는 IoT 플러그 앤 플레이 미리 보기 샘플 디바이스 코드를 빌드하고 실행합니다. Azure IoT 탐색기 도구를 사용하여 디바이스에서 허브로 전송된 정보를 봅니다.
author: ChrisGMsft
ms.author: chrisgre
ms.date: 12/26/2019
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: 1c8b6a5d133d8838c2c7a23f8881092affa424dc
ms.sourcegitcommit: ce4a99b493f8cf2d2fd4e29d9ba92f5f942a754c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/28/2019
ms.locfileid: "75531214"
---
# <a name="quickstart-connect-a-sample-iot-plug-and-play-preview-device-application-running-on-windows-to-iot-hub-c-windows"></a>빠른 시작: Windows에서 실행되는 샘플 IoT 플러그 앤 플레이 미리 보기 디바이스 애플리케이션을 IoT Hub에 연결(C Windows)

[!INCLUDE [iot-pnp-quickstarts-2-selector.md](../../includes/iot-pnp-quickstarts-2-selector.md)]

이 빠른 시작에서는 샘플 IoT 플러그 앤 플레이 디바이스 애플리케이션을 빌드하고, IoT Hub에 연결하고, Azure IoT 탐색기 도구를 사용하여 디바이스에서 허브로 전송된 정보를 확인하는 방법을 보여줍니다. 샘플 애플리케이션은 C로 작성되었으며 Azure IoT Hub Device C SDK에 포함되어 있습니다. 솔루션 개발자는 디바이스 코드를 볼 필요 없이 Azure IoT 탐색기 도구를 사용하여 IoT 플러그 앤 플레이 디바이스의 기능을 이해할 수 있습니다.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>사전 요구 사항

이 빠른 시작을 완료하려면 로컬 머신에 다음 소프트웨어를 설치해야 합니다.

* [Visual Studio(Community, Professional 또는 Enterprise)](https://visualstudio.microsoft.com/downloads/) - Visual Studio를 설치할 때 **NuGet 패키지 관리자** 구성 요소와 **C++를 사용한 데스크톱 개발** 워크로드를 포함해야 합니다.
* [Git](https://git-scm.com/download/)
* [CMake](https://cmake.org/download/)

### <a name="install-the-azure-iot-explorer"></a>Azure IoT 탐색기 설치

최신 업데이트에 대한 "자산"에서 .msi 파일을 선택하여 도구의 [리포지토리](https://github.com/Azure/azure-iot-explorer/releases) 페이지에서 최신 버전의 **Azure IoT 탐색기**를 다운로드하고 설치합니다.

[!INCLUDE [iot-pnp-prepare-iot-hub.md](../../includes/iot-pnp-prepare-iot-hub.md)]

다음 명령을 실행하여 허브의 _IoT 허브 연결 문자열_을 가져옵니다(나중에 사용하기 위해 참고).

```azurecli-interactive
az iot hub show-connection-string --hub-name <YourIoTHubName> --output table
```

## <a name="prepare-the-development-environment"></a>개발 환경 준비

이 빠른 시작에서는 Azure IoT Hub Device C SDK를 복제하고 빌드하는 데 사용할 수 있는 개발 환경을 준비합니다.

선택한 디렉터리에서 명령 프롬프트를 엽니다. 다음 명령을 실행하여 [Azure IoT C SDK 및 라이브러리](https://github.com/Azure/azure-iot-sdk-c) GitHub 리포지토리를 이 위치에 복제합니다.

```cmd/sh
git clone https://github.com/Azure/azure-iot-sdk-c --recursive -b public-preview
```

이 작업을 완료하는 데 몇 분 정도가 걸립니다.

## <a name="build-the-code"></a>코드 빌드

디바이스 SDK를 사용하여 포함된 샘플 코드를 빌드합니다. 빌드하는 애플리케이션은 IoT Hub에 연결하는 디바이스를 시뮬레이션합니다. 이 애플리케이션은 원격 분석 데이터 및 속성을 보내고 명령을 수신합니다.

1. 디바이스 SDK 루트 폴더에 `cmake` 하위 디렉터리를 만들고 해당 폴더로 이동합니다.

    ```cmd\sh
    cd <root folder>\azure-iot-sdk-c
    mkdir cmake
    cd cmake
    ```

1. 다음 명령을 실행하여 디바이스 SDK 및 생성된 코드 스텁을 빌드합니다.

    ```cmd\sh
    cmake ..
    cmake --build . -- /m /p:Configuration=Release
    ```

    > [!NOTE]
    > cmake에서 C++ 컴파일러를 찾을 수 없는 경우 이전 명령을 실행할 때 빌드 오류가 발생합니다. 이 경우에는 [Visual Studio 명령 프롬프트](https://docs.microsoft.com/dotnet/framework/tools/developer-command-prompt-for-vs)에서 이 명령을 실행합니다.

## <a name="run-the-device-sample"></a>디바이스 샘플 실행

SDK에서 샘플 애플리케이션을 실행하여 IoT Hub에 원격 분석을 전송하는 IoT 플러그 앤 플레이 디바이스를 시뮬레이트합니다. 샘플 애플리케이션을 실행하려면 다음 명령을 사용하여 _디바이스 연결 문자열_을 매개 변수로 전달합니다.

```cmd\sh
cd digitaltwin_client\samples\digitaltwin_sample_device\Release
copy ..\EnvironmentalSensor.interface.json .
digitaltwin_sample_device.exe "<YourDeviceConnectionString>"
```

이제 디바이스가 명령 및 속성 업데이트를 받을 준비가 되었으며, 허브로 원격 분석 데이터를 보내기 시작했습니다. 다음 단계를 완료하는 동안 샘플을 계속 실행하세요.

## <a name="use-the-azure-iot-explorer-to-validate-the-code"></a>Azure IoT 탐색기를 사용하여 코드의 유효성을 검사합니다.

[!INCLUDE [iot-pnp-iot-explorer-1.md](../../includes/iot-pnp-iot-explorer-1.md)]

4. 도구가 디바이스의 인터페이스 모델 정의를 읽을 수 있도록 **설정**을 선택합니다. [설정] 메뉴의 [플러그 앤 플레이 구성]에 **연결된 디바이스**가 이미 표시되어 있을 수도 있습니다. 표시되지 있지 않으면 **+ 모듈 정의 원본 새로 만들기**를 선택한 다음, **연결된 디바이스**를 선택하여 추가합니다.

1. **디바이스** 개요 페이지로 돌아가서 이전에 만든 디바이스 ID를 찾습니다. 디바이스 애플리케이션이 명령 프롬프트에서 계속 실행 중인 상태에서 Azure IoT 탐색기에 디바이스의 **연결 상태**가 _연결됨_으로 보고되고 있는지 확인합니다(그렇지 않은 경우 연결될 때까지 **새로 고침**을 누릅니다). 디바이스를 선택하여 자세한 내용을 봅니다.

1. ID가 **urn:YOUR_COMPANY_NAME_HERE:EnvironmentalSensor:1**인 인터페이스를 확장하여 인터페이스 및 IoT 플러그 앤 플레이 기본 형식인 속성, 명령 및 원격 분석을 표시합니다.

[!INCLUDE [iot-pnp-iot-explorer-2.md](../../includes/iot-pnp-iot-explorer-2.md)]

[!INCLUDE [iot-pnp-clean-resources.md](../../includes/iot-pnp-clean-resources.md)]

## <a name="next-steps"></a>다음 단계

이 빠른 시작에서는 IoT 플러그 앤 플레이 디바이스를 IoT Hub에 연결하는 방법을 알아보았습니다. IoT 플러그 앤 플레이 디바이스와 상호 작용하는 솔루션을 빌드하는 방법에 대한 자세한 내용은 다음 문서를 참조하세요.

> [!div class="nextstepaction"]
> [방법: IoT 플러그 앤 플레이 미리 보기 디바이스에 연결하고 상호 작용](howto-develop-solution.md)
