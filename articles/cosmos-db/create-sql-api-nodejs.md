---
title: '빠른 시작: Node.js를 사용하여 Azure Cosmos DB SQL API 계정에서 쿼리'
description: Node.js를 사용하여 Azure Cosmos DB SQL API 계정에 연결하고 데이터를 쿼리하는 앱을 만드는 방법을 설명합니다.
author: deborahc
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: nodejs
ms.topic: quickstart
ms.date: 11/19/2019
ms.author: dech
ms.openlocfilehash: 8df78df27ffb7e8bb8fc88567bd0b3d37be20488
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76719503"
---
# <a name="quickstart-use-nodejs-to-connect-and-query-data-from-azure-cosmos-db-sql-api-account"></a>빠른 시작: Node.js를 사용하여 Azure Cosmos DB SQL API 계정에 연결하고 데이터 쿼리

> [!div class="op_single_selector"]
> * [.NET V3](create-sql-api-dotnet.md)
> * [.NET V4](create-sql-api-dotnet-V4.md)
> * [Java](create-sql-api-java.md)
> * [Node.JS](create-sql-api-nodejs.md)
> * [Python](create-sql-api-python.md)
> * [Xamarin](create-sql-api-xamarin-dotnet.md)

이 빠른 시작에서는 Node.js 앱을 사용하여 Azure Cosmos DB에서 [SQL API](sql-api-introduction.md) 계정에 연결하는 방법을 보여줍니다. 그런 다음, Azure Cosmos DB SQL 쿼리를 사용하여 데이터를 쿼리하고 관리할 수 있습니다. 이 문서에서 빌드하는 Node.js 앱은 [SQL JavaScript SDK](sql-api-sdk-node.md)를 사용합니다. 이 빠른 시작은 [JavaScript SDK](https://www.npmjs.com/package/@azure/cosmos) 버전 2.0을 사용합니다.

## <a name="prerequisites"></a>사전 요구 사항

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)] 
[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* 또한,
    * [Node.js](https://nodejs.org/en/) 버전 v6.0.0 이상
    * [Git](https://git-scm.com/)

## <a name="create-a-database"></a>데이터베이스 만들기 

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-container"></a>컨테이너 추가

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

## <a name="add-sample-data"></a>샘플 데이터 추가

[!INCLUDE [cosmos-db-create-sql-api-add-sample-data](../../includes/cosmos-db-create-sql-api-add-sample-data.md)]

## <a name="query-your-data"></a>데이터 쿼리

[!INCLUDE [cosmos-db-create-sql-api-query-data](../../includes/cosmos-db-create-sql-api-query-data.md)]

## <a name="clone-the-sample-application"></a>샘플 애플리케이션 복제

이제 GitHub에서 Node.js 앱을 복제하고 연결 문자열을 설정한 다음, 실행해 보겠습니다.

1. 명령 프롬프트를 열고, git-samples라는 새 폴더를 만든 다음 명령 프롬프트를 닫습니다.

    ```bash
    md "C:\git-samples"
    ```

2. Git Bash와 같은 Git 터미널 창을 열고, `cd` 명령을 사용하여 샘플 앱을 설치할 새 폴더로 변경합니다.

    ```bash
    cd "C:\git-samples"
    ```

3. 다음 명령을 실행하여 샘플 리포지토리를 복제합니다. 이 명령은 컴퓨터에서 샘플 앱의 복사본을 만듭니다.

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-sql-api-nodejs-getting-started.git
    ```

## <a name="review-the-code"></a>코드 검토

이 단계는 선택 사항입니다. Azure Cosmos 데이터베이스 리소스를 코드로 만드는 방법을 알아보려면 다음 코드 조각을 검토하면 됩니다. 그렇지 않으면 [연결 문자열 업데이트](#update-your-connection-string)로 건너뛸 수 있습니다. 

이전 버전의 JavaScript SDK에 익숙한 경우 '컬렉션' 및 '문서'라는 용어를 자주 들어 보셨을 것입니다. Azure Cosmos DB가 [여러 API 모델](https://docs.microsoft.com/azure/cosmos-db/introduction)을 지원하므로 JavaScript SDK 버전 2.0 이상에서는 컬렉션, 그래프 또는 테이블을 가리키는 일반적인 용어인 '컨테이너'와 컨테이너의 콘텐츠를 설명하는 '항목'이라는 용어를 사용합니다.

다음 코드 조각은 모두 **app.js** 파일에서 가져옵니다.

* `CosmosClient` 개체가 초기화되었습니다.

    ```javascript
    const client = new CosmosClient({ endpoint, key });
    ```

* 새 Azure Cosmos 데이터베이스 만들기

    ```javascript
    const { database } = await client.databases.createIfNotExists({ id: databaseId });
    ```

* 데이터베이스 내에 새 컨테이너(컬렉션)가 만들어집니다.

    ```javascript
    const { container } = await client.database(databaseId).containers.createIfNotExists({ id: containerId });
    ```

* 항목(문서)이 만들어집니다.

    ```javascript
    const { item } = await client.database(databaseId).container(containerId).items.create(itemBody);
    ```

* 패밀리 데이터베이스에서 JSON에 대한 SQL 쿼리가 수행됩니다. 이 쿼리는 "Anderson" 패밀리의 모든 자식을 반환합니다. 

    ```javascript
      const querySpec = {
        query: 'SELECT VALUE r.children FROM root r WHERE r.lastName = @lastName',
        parameters: [
          {
            name: '@lastName',
            value: 'Andersen'
          }
        ]
      }

      const { resources: results } = await client
        .database(databaseId)
        .container(containerId)
        .items.query(querySpec)
        .fetchAll()
      for (var queryResult of results) {
        let resultString = JSON.stringify(queryResult)
        console.log(`\tQuery returned ${resultString}\n`)
      }
    ```    

## <a name="update-your-connection-string"></a>연결 문자열 업데이트

Azure Portal로 돌아가서 Azure Cosmos 계정의 연결 문자열 세부 정보를 가져옵니다. 앱이 데이터베이스에 연결할 수 있도록 연결 문자열을 앱에 복사합니다.

1. [Azure Portal](https://portal.azure.com/)에서 Azure Cosmos 계정의 왼쪽 탐색 영역에 있는 **키**를 클릭한 다음, **읽기-쓰기 키**를 클릭합니다. 다음 단계에서 화면 오른쪽의 복사 단추를 사용하여 URI 및 기본 키를 `config.js` 파일에 복사하게 됩니다.

    ![Azure Portal에서 선택 키 보기 및 복사, 키 블레이드](./media/create-sql-api-dotnet/keys.png)

2. `config.js` 파일을 엽니다. 

3. 포털에서 URI 값을 복사(복사 단추 사용)한 후 이 값을 `config.js`의 엔드포인트 키 값으로 만듭니다. 

    `config.endpoint = "<Your Azure Cosmos account URI>"`

4. 그 다음, 포털에서 사용자의 기본 키 값을 복사한 후 `config.js`의 `config.key` 값으로 만듭니다. 이제 Azure Cosmos DB와 통신하는 데 필요한 모든 정보로 앱이 업데이트되었습니다. 

    `config.key = "<Your Azure Cosmos account key>"`
    
## <a name="run-the-app"></a>앱 실행

1. 터미널에서 `npm install`를 실행하여 필요한 npm 모듈을 설치합니다.

2. 터미널에서 `node app.js`을 실행하여 노드 애플리케이션을 시작합니다.

Data Explorer로 돌아가서 이 새 데이터를 수정하고 작업에 사용할 수 있습니다.

## <a name="review-slas-in-the-azure-portal"></a>Azure Portal에서 SLA 검토

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>리소스 정리

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>다음 단계

이 빠른 시작에서는 Azure Cosmos 계정을 만들고, 데이터 탐색기를 사용하여 컨테이너를 만들고, 앱을 실행하는 방법을 알아보았습니다. 이제 Azure Cosmos 데이터베이스로 추가 데이터를 가져올 수 있습니다. 

> [!div class="nextstepaction"]
> [Azure Cosmos DB로 데이터 가져오기](import-data.md)


