---
title: 자습서 - Yeoman을 사용하여 Azure에서 Terraform 기본 템플릿 만들기
description: Yeoman을 사용하여 Azure에서 Terraform 기본 템플릿을 만드는 방법을 알아봅니다.
ms.topic: tutorial
ms.date: 10/26/2019
ms.openlocfilehash: ba81d0ee797fd879fdadc3a6b25ca8f310383f61
ms.sourcegitcommit: 28688c6ec606ddb7ae97f4d0ac0ec8e0cd622889
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/18/2019
ms.locfileid: "74159157"
---
# <a name="tutorial-create-a-terraform-base-template-in-azure-using-yeoman"></a>자습서: Yeoman을 사용하여 Azure에 Terraform 기본 템플릿 만들기

이 자습서에서는 [Terraform](/azure/terraform/) 및 [Yeoman](https://yeoman.io/)의 조합을 사용하는 방법을 알아봅니다. Terraform은 Azure에서 인프라를 만들기 위한 도구입니다. Yeoman을 사용하면 Terraform 모듈을 손쉽게 만들 수 있습니다.

이 문서에서는 다음 작업을 수행하는 방법을 알아봅니다.
> [!div class="checklist"]
> * Yeoman 모듈 생성기를 사용하여 기본 Terraform 템플릿을 만듭니다.
> * 두 가지 다른 방법으로 Terraform 템플릿을 테스트합니다.
> * Docker 파일을 사용하여 Terraform 모듈을 실행합니다.
> * Azure Cloud Shell에서 기본적으로 Terraform 모듈을 실행합니다.

## <a name="prerequisites"></a>필수 조건

- **Azure 구독**: Azure 구독이 아직 없는 경우 시작하기 전에 [무료 계정](https://azure.microsoft.com/free/) 을 만듭니다.
- **Visual Studio Code**: 플랫폼에 [Visual Studio Code를 다운로드](https://code.visualstudio.com/download)합니다.
- **Terraform**: Yeoman에서 만든 모듈을 실행하기 위해 [Terraform을 설치](/azure/virtual-machines/linux/terraform-install-configure )합니다.
- **Docker**: Yeoman 생성기에서 만든 모듈을 실행하기 위해 [Docker를 설치](https://www.docker.com/get-started)합니다.
- **Go 프로그래밍 언어**: Yeoman에서 생성된 테스트 사례는 Go 언어를 사용하므로 [Go를 설치](https://golang.org/)합니다.

>[!NOTE]
>이 자습서에 나온 프로시저 대부분은 명령줄 인터페이스를 포함합니다. 설명된 단계는 모든 운영 체제 및 명령줄 도구에 적용됩니다. 예제에서는 로컬 환경에 PowerShell을 사용하고, 클라우드 셸 환경에는 Git Bash를 사용하도록 선택했습니다.

## <a name="prepare-your-environment"></a>환경 준비

### <a name="install-nodejs"></a>Node.js 설치

Cloud Shell에서 Terraform을 사용하려면 [Node.js 6.0 이상을 설치](https://nodejs.org/en/download/)해야 합니다.

>[!NOTE]
>Node.js가 설치되어 있는지 확인하려면 터미널 창을 열고 `node --version`을 입력합니다.

### <a name="install-yeoman"></a>Yeoman 설치

다음 명령 실행:

```bash
npm install -g yo
```

![Yeoman 설치](media/terraform-vscode-module-generator/ymg-npm-install-yo.png)

### <a name="install-the-yeoman-template-for-terraform-module"></a>Terraform 모듈에 대한 Yeoman 템플릿 설치

다음 명령 실행:

```bash
npm install -g generator-az-terra-module
```

![generator-az-terra-module 설치](media/terraform-vscode-module-generator/ymg-pm-install-generator-module.png)

Yeoman이 설치되어 있는지 확인하려면 다음 명령을 실행합니다.

```bash
yo --version
```

### <a name="create-a-directory-for-the-yeoman-generated-module"></a>Yeoman에서 생성된 모듈에 대한 디렉터리 만들기

Yeoman 템플릿은 현재 디렉터리에서 파일을 생성합니다. 이러한 이유로 디렉터리를 만들어야 합니다.

이 빈 디렉터리는 $GOPATH/src 아래에 위치해야 합니다. 이 경로에 대한 자세한 내용은 [GOPATH 설정](https://github.com/golang/go/wiki/SettingGOPATH) 문서를 참조하세요.

1. 새 디렉터리를 만들 부모 디렉터리로 이동합니다.

1. 다음 명령에서 자리 표시자를 바꿔서 실행합니다. 이 예제의 경우 `GeneratorDocSample`의 디렉터리 이름이 사용됩니다.

    ```bash
    mkdir <new-directory-name>
    ```

    ![mkdir](media/terraform-vscode-module-generator/ymg-mkdir-GeneratorDocSample.png)

1. 새 디렉터리로 이동합니다.

    ```bash
    cd <new-directory-name>
    ```

    ![새 디렉터리로 이동](media/terraform-vscode-module-generator/ymg-cd-GeneratorDocSample.png)

## <a name="create-a-base-module-template"></a>기본 모듈 템플릿 만들기

1. 다음 명령 실행:

    ```bash
    yo az-terra-module
    ```

1. 화면 지침을 따라 다음 정보를 제공합니다.

    - **Terraform 모듈 프로젝트 이름** - `doc-sample-module`의 값이 예제에 사용됩니다.

        ![프로젝트 이름](media/terraform-vscode-module-generator/ymg-project-name.png)       


    - **Docker 이미지 파일을 포함하시겠습니까?** - `y`를 입력합니다. `n`을 선택하면 생성된 모듈 코드가 기본 모드에서만 실행되도록 지원합니다.

        ![Docker 이미지 파일을 포함하시겠습니까?](media/terraform-vscode-module-generator/ymg-include-docker-image-file.png) 

1. 디렉터리 콘텐츠를 나열하여 생성된 결과 파일을 확인합니다.

    ```bash
    ls
    ```

    ![만든 파일 나열](media/terraform-vscode-module-generator/ymg-ls-GeneratorDocSample-files.png)

## <a name="review-the-generated-module-code"></a>생성된 모듈 코드 검토

1. Visual Studio Code 시작

1. 메뉴 모음에서 **파일 > 폴더 열기**를 선택하고 만든 폴더를 선택합니다.

    ![Visual Studio Code](media/terraform-vscode-module-generator/ymg-open-in-vscode.png)

다음 파일은 Yeoman 모듈 생성기에서 생성되었습니다. 이러한 파일 및 해당 사용량에 대한 자세한 내용은 [Terraform 모듈에서 Terratest](https://mseng.visualstudio.com/VSJava/_git/Terraform?path=%2FTerratest%20Introduction.md&version=GBmaster)를 참조하세요.

- `main.tf` - `random-shuffle`이라는 모듈을 정의합니다. 입력은 `string_list`입니다. 출력은 순열의 개수입니다.
- `variables.tf` - 모듈에서 사용되는 입력 및 출력 변수를 정의합니다.
- `outputs.tf` - 모듈에서 출력하는 항목을 정의합니다. 여기에서는 기본 제공 Terraform 모듈인 `random_shuffle`에서 반환되는 값입니다.
- `Rakefile` - 빌드 단계를 정의합니다. 이러한 단계는 다음과 같습니다.
    - `build` - main.tf 파일에 있는 서식의 유효성을 검사합니다.
    - `unit` - 생성된 모듈 구조에는 단위 테스트에 대한 코드가 포함되지 않습니다. 단위 테스트 시나리오를 지정하려는 경우 여기에서 해당 코드를 추가합니다.
    - `e2e` - 모듈의 엔드투엔드 테스트를 실행합니다.
- `test`
    - 테스트 사례는 Go로 작성되었습니다.
    - 테스트의 모든 코드는 엔드투엔드 테스트입니다.
    - 엔드투엔드 테스트는 `fixture`에서 정의된 모든 항목의 프로비저닝을 시도합니다. `template_output.go` 파일의 결과는 미리 정의된 예상된 값과 비교됩니다.
    - `Gopkg.lock` 및 `Gopkg.toml`: 종속성을 정의합니다. 

## <a name="test-your-new-terraform-module-using-a-docker-file"></a>Docker 파일을 사용하여 새 Terraform 모듈 테스트

이 섹션에서는 Docker 파일을 사용하여 Terraform 모듈을 테스트하는 방법을 보여줍니다.

>[!NOTE]
>이 예제에서는 모듈을 Azure가 아닌 로컬로 실행합니다.

### <a name="confirm-docker-is-installed-and-running"></a>Docker가 설치되고 실행되는지 확인

명령 프롬프트에서 `docker version`을 입력합니다.

![Docker 버전](media/terraform-vscode-module-generator/ymg-docker-version.png)

결과 출력은 Docker가 설치되어 있는지 확인합니다.

해당 Docker가 실제로 실행 중인지 확인하려면 `docker info`를 입력합니다.

![Docker 정보](media/terraform-vscode-module-generator/ymg-docker-info.png)

### <a name="set-up-a-docker-container"></a>Docker 컨테이너 설정

1. 명령 프롬프트에서 다음을 입력합니다.

    `docker build --build-arg BUILD_ARM_SUBSCRIPTION_ID= --build-arg BUILD_ARM_CLIENT_ID= --build-arg BUILD_ARM_CLIENT_SECRET= --build-arg BUILD_ARM_TENANT_ID= -t terra-mod-example .`.

    **성공적으로 빌드됨** 메시지가 표시됩니다.

    ![성공적인 빌드를 표시하는 메시지](media/terraform-vscode-module-generator/ymg-successfully-built.png)

1. 명령 프롬프트에서 `docker image ls`를 입력하여 나열된 `terra-mod-example`에서 생성된 모듈을 표시합니다.

    ![새 모듈을 포함하는 목록](media/terraform-vscode-module-generator/ymg-repository-results.png)

1. `docker run -it terra-mod-example /bin/sh`을 입력합니다. `docker run` 명령을 실행하면 Docker 환경으로 들어갑니다. 이때 `ls` 명령을 사용하여 파일을 검색할 수 있습니다.

    ![Docker의 파일 목록](media/terraform-vscode-module-generator/ymg-list-docker-file.png)

### <a name="build-the-module"></a>모듈 빌드

1. 다음 명령 실행:

    ```bash
    bundle install
    ```

1. 다음 명령 실행:

    ```bash
    rake build
    ```

    ![Rake 빌드](media/terraform-vscode-module-generator/ymg-rake-build.png)

### <a name="run-the-end-to-end-test"></a>엔드투엔드 테스트 실행

1. 다음 명령 실행:

    ```bash
    rake e2e
    ```

1. 잠시 후에 **통과** 메시지가 표시됩니다.

    ![통과](media/terraform-vscode-module-generator/ymg-pass.png)

1. `exit`를 입력하여 테스트를 완료하고 Docker 환경을 종료합니다.

## <a name="use-yeoman-generator-to-create-and-test-a-module-in-cloud-shell"></a>Yeoman 생성기를 사용하여 Cloud Shell에서 모듈 만들기 및 테스트

이 섹션에서는 Yeoman 생성기를 사용하여 Cloud Shell에서 모듈을 만들고 테스트합니다. Docker 파일을 사용하는 대신 Cloud Shell을 사용하면 프로세스가 크게 간소화됩니다. Cloud Shell를 사용하면 다음 제품이 모두 미리 설치되어 있습니다.

- Node.js
- Yeoman
- Terraform

### <a name="start-a-cloud-shell-session"></a>Cloud Shell 세션 시작

1. [Azure Portal](https://portal.azure.com/), [shell.azure.com](https://shell.azure.com), 또는 [Azure 모바일 앱](https://azure.microsoft.com/features/azure-portal/mobile-app/)을 통해 Azure Cloud Shell 세션을 시작합니다.

1. **Azure Cloud Shell 시작** 페이지가 열립니다. **Bash(Linux)** 를 선택합니다.

    ![Azure Cloud Shell 시작](media/terraform-vscode-module-generator/ymg-welcome-to-azure-cloud-shell.png)

1. 아직 Azure Storage 계정을 설정하지 않은 경우 다음 화면이 나타납니다. **스토리지 만들기**를 선택합니다.

    ![탑재된 스토리지가 없음](media/terraform-vscode-module-generator/ymg-you-have-no-storage-mounted.png)

1. Azure Cloud Shell은 사용자가 이전에 선택한 셸에서 시작되며, 방금 만든 클라우드 드라이브에 대한 정보를 표시합니다.

    ![클라우드 드라이브가 만들어졌습니다.](media/terraform-vscode-module-generator/ymg-your-cloud-drive-has-been-created-in.png)

### <a name="prepare-a-directory-to-hold-your-terraform-module"></a>Terraform 모듈을 보관할 디렉터리 준비

1. 이 시점에서 Cloud Shell은 이미 환경 변수에서 GOPATH를 구성했습니다. 경로를 확인하려면 `go env`를 입력합니다.

1. 아직 없는 경우 $GOPATH 디렉터리를 만듭니다. `mkdir ~/go`을 입력합니다.

1. $GOPATH 디렉터리 내에 디렉터리를 만듭니다. 이 디렉터리는 이 예제에서 만든 다른 프로젝트 디렉터리를 저장하는 데 사용됩니다. 

    ```bash
    mkdir ~/go/src
    ```

1. 자리 표시자를 바꾼 Terraform 모듈을 저장할 디렉터리를 만듭니다. 이 예제의 경우 `my-module-name`의 디렉터리 이름이 사용됩니다.

    ```bash
    mkdir ~/go/src/<your-module-name>
    ```

1. 모듈 디렉터리로 이동합니다. 

    ```bash
    cd ~/go/src/<your-module-name>
    ```

### <a name="create-and-test-your-terraform-module"></a>Terraform 모듈 만들기 및 테스트

1. 다음 명령을 실행하고 지침을 따릅니다. Docker 파일을 만들 것인지를 묻는 메시지가 표시되면 `N`을 입력합니다.

    ```bash
    yo az-terra-module
    ```

1. 다음 명령을 실행하여 종속성을 설치합니다.

    ```bash
    bundle install
    ```

1. 다음 명령을 실행하여 모듈을 빌드합니다.

    ```bash
    rake build
    ```

    ![Rake 빌드](media/terraform-vscode-module-generator/ymg-rake-build.png)

1. 다음 명령을 실행하여 텍스트를 실행합니다.

    ```bash
    rake e2e
    ```

    ![테스트 통과 결과](media/terraform-vscode-module-generator/ymg-pass.png)

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [Azure Terraform Visual Studio Code 확장을 설치하고 사용합니다.](/azure/terraform/terraform-vscode-extension)