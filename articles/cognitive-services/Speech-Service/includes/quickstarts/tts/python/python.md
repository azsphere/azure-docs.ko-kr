---
title: '빠른 시작: 음성 합성, Python - Speech Service'
titleSuffix: Azure Cognitive Services
description: Speech SDK를 사용하여 Python에서 음성을 합성하는 방법 알아보기
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 19/14/2020
ms.author: dapine
ms.openlocfilehash: 7be9037ddfd19d82263b37fe0b824e3c2b3f5078
ms.sourcegitcommit: dbcc4569fde1bebb9df0a3ab6d4d3ff7f806d486
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/15/2020
ms.locfileid: "76037758"
---
## <a name="prerequisites"></a>사전 요구 사항

시작하기 전에 다음을 확인해야 합니다.

> [!div class="checklist"]
> * [Azure Speech 리소스 만들기](../../../../get-started.md)
> * [개발 환경 설정](../../../../quickstarts/setup-platform.md)
> * [빈 샘플 프로젝트 만들기](../../../../quickstarts/create-project.md)

## <a name="support-and-updates"></a>지원 및 업데이트

Speech SDK Python 패키지에 대한 업데이트는 PyPI를 통해 배포되며, [릴리스 정보](~/articles/cognitive-services/Speech-Service/releasenotes.md)에서 발표됩니다.
새 버전을 사용할 수 있는 경우 `pip install --upgrade azure-cognitiveservices-speech` 명령을 사용하여 업데이트할 수 있습니다.
`azure.cognitiveservices.speech.__version__` 변수를 검사하여 현재 설치된 버전을 확인하세요.

문제가 있거나 기능이 누락된 경우 [지원 및 도움말 옵션](~/articles/cognitive-services/Speech-Service/support.md)을 참조하세요.

## <a name="create-a-python-application-that-uses-the-speech-sdk"></a>Speech SDK를 사용하는 Python 애플리케이션 만들기

### <a name="run-the-sample"></a>샘플 실행

이 빠른 시작에서 원본 파일 `quickstart.py`로 [샘플 코드](#sample-code)를 복사하고 IDE 또는 콘솔에서 실행할 수 있습니다.

```sh
python quickstart.py
```

또는 [Speech SDK 샘플 리포지토리](https://aka.ms/csspeech/samples)에서 [Jupyter](https://jupyter.org) Notebook으로 이 빠른 시작 자습서를 다운로드하고 노트북으로 실행할 수 있습니다.

### <a name="sample-code"></a>예제 코드

[!code-python[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/python/text-to-speech/quickstart.py#code)]

### <a name="install-and-use-the-speech-sdk-with-visual-studio-code"></a>Visual Studio Code를 사용하여 Speech SDK 설치 및 사용

1. 컴퓨터에 [Python](https://www.python.org/downloads/)의 64비트 버전(3.5 이상)을 다운로드하고 설치합니다.
1. [Visual Studio Code](https://code.visualstudio.com/Download)를 다운로드하여 설치합니다.
1. Visual Studio Code를 열고 Python 확장을 설치합니다. 메뉴에서 **파일** > **기본 설정** > **확장**을 선택합니다. **Python**을 검색합니다.

   ![Python 확장 설치](~/articles/cognitive-services/Speech-Service/media/sdk/qs-python-vscode-python-extension.png)

1. 프로젝트를 저장할 폴더를 만듭니다. 예를 들어 Windows 탐색기를 사용합니다.
1. Visual Studio Code에서 **파일** 아이콘을 선택합니다. 그런 다음, 사용자가 만든 폴더를 엽니다.

   ![폴더 열기](~/articles/cognitive-services/Speech-Service/media/sdk/qs-python-vscode-python-open-folder.png)

1. 새 파일 아이콘을 선택하여 새 Python 원본 파일 `speechsdk.py`를 만듭니다.

   ![파일 만들기](~/articles/cognitive-services/Speech-Service/media/sdk/qs-python-vscode-python-newfile.png)

1. 새로 생성된 파일에 [Python 코드](#sample-code)를 복사, 붙여넣기 및 저장합니다.
1. Speech Service 구독 정보를 삽입합니다.
1. Python 인터프리터가 이미 선택된 경우 창 아래쪽의 상태 표시줄 왼쪽에 표시됩니다.
   그렇지 않을 경우 사용 가능한 Python 인터프리터 목록을 불러오세요. 명령 팔레트를 열고(Ctrl+Shift+P) **Python: 인터프리터 선택**을 입력합니다. 적절한 항목을 선택합니다.
1. Visual Studio Code 내에서 Speech SDK Python 패키지를 설치할 수 있습니다. 선택한 Python 인터프리터에 아직 설치되지 않은 경우 이렇게 합니다.
   Speech SDK 패키지를 설치하려면 터미널을 엽니다. 명령 팔레트를 다시 불러오고(Ctrl+Shift+P) **터미널: 새 통합 터미널 만들기**를 입력하여 터미널을 엽니다.
   열리는 터미널에서 `python -m pip install azure-cognitiveservices-speech` 명령 또는 시스템에 대한 적절한 명령을 입력합니다.
1. 샘플 코드를 실행하려면 편집기 내의 임의 위치를 마우스 오른쪽 단추로 클릭합니다. **터미널에서 Python 파일 실행**을 선택합니다.
   메시지가 표시되면 텍스트를 입력합니다. 합성된 오디오는 나중에 재생됩니다.

   ![샘플 실행](~/articles/cognitive-services/Speech-Service/media/sdk/qs-python-vscode-python-run-tts.png)

이러한 지침에 따른 문제가 있는 경우 더 광범위한 [Visual Studio Code Python 자습서](https://code.visualstudio.com/docs/python/python-tutorial)를 참조하세요.

## <a name="next-steps"></a>다음 단계

[!INCLUDE [footer](./footer.md)]

## <a name="see-also"></a>참고 항목

- [사용자 지정 음성 만들기](~/articles/cognitive-services/Speech-Service/how-to-custom-voice-create-voice.md)
- [사용자 지정 음성 녹음 샘플](~/articles/cognitive-services/Speech-Service/record-custom-voice-samples.md)
