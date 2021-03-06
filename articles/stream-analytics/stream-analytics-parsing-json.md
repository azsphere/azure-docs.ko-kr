---
title: Azure Stream Analytics에서 JSON 및 AVRO 구문 분석
description: 이 문서에서는 배열, JSON, CSV 형식의 데이터와 같은 복합 데이터 형식을 조작하는 방법을 설명합니다.
ms.service: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.topic: conceptual
ms.date: 01/29/2020
ms.openlocfilehash: ac06521df38bdc91ca717d888c73cd541576014d
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/31/2020
ms.locfileid: "76905459"
---
# <a name="parse-json-and-avro-data-in-azure-stream-analytics"></a>Azure Stream Analytics에서 JSON 및 Avro 데이터 구문 분석

CSV, JSON 및 Avro 데이터 형식의 이벤트 처리를 지원 Azure Stream Analytics 합니다. JSON 및 Avro 데이터는 모두 구조화 될 수 있으며 중첩 된 개체 (레코드) 및 배열과 같은 일부 복합 형식을 포함할 수 있습니다. 

>[!NOTE]
>이벤트 허브 캡처에서 만든 AVRO 파일은 *사용자 지정 역직렬 변환기* 기능을 사용 해야 하는 특정 형식을 사용 합니다. 자세한 내용은 [.net 사용자 지정 deserializers를 사용 하 여 모든 형식의 입력 읽기](https://docs.microsoft.com/azure/stream-analytics/custom-deserializer-examples)를 참조 하세요.



## <a name="record-data-types"></a>레코드 데이터 형식
레코드 데이터 형식은 입력 데이터 스트림에서 사용될 때 JSON 및 Avro 배열을 나타내는 데 사용됩니다. 다음 예제에서는 입력 이벤트를 JSON 형식으로 읽는 샘플 센서를 보여 줍니다. 단일 이벤트의 예제는 다음과 같습니다.

```json
{
    "DeviceId" : "12345",
    "Location" :
    {
        "Lat": 47,
        "Long": 122
    },
    "SensorReadings" :
    {
        "Temperature" : 80,
        "Humidity" : 70,
        "CustomSensor01" : 5,
        "CustomSensor02" : 99,
        "SensorMetadata" : 
        {
        "Manufacturer":"ABC",
        "Version":"1.2.45"
        }
    }
}
```

### <a name="access-nested-fields-in-known-schema"></a>알려진 스키마의 중첩 된 필드에 액세스
점 표기법 (.)을 사용 하 여 쿼리에서 직접 중첩 된 필드에 쉽게 액세스할 수 있습니다. 예를 들어이 쿼리는 위의 JSON 데이터에서 Location 속성 아래에 있는 위도 및 경도 좌표를 선택 합니다. 점 표기법은 아래와 같이 여러 수준을 탐색 하는 데 사용할 수 있습니다.

```SQL
SELECT
    DeviceID,
    Location.Lat,
    Location.Long,
    SensorReadings.Temperature,
    SensorReadings.SensorMetadata.Version
FROM input
```

결과는 다음과 같습니다.

|DeviceID|Lat|장기|온도|버전|
|-|-|-|-|-|
|12345|47|122|80|1.2.45|


### <a name="select-all-properties"></a>모든 속성 선택
중첩된 레코드의 모든 속성은 '*' 와일드카드를 사용하여 선택할 수 있습니다. 다음과 같은 예제를 참조하세요.

```SQL
SELECT
    DeviceID,
    Location.*
FROM input
```

결과는 다음과 같습니다.

|DeviceID|Lat|장기|
|-|-|-|
|12345|47|122|


### <a name="access-nested-fields-when-property-name-is-a-variable"></a>속성 이름이 변수일 때 중첩 된 필드 액세스

속성 이름이 변수인 경우 [GetRecordPropertyValue](https://docs.microsoft.com/stream-analytics-query/getrecordpropertyvalue-azure-stream-analytics) 함수를 사용 합니다. 이를 통해 속성 이름을 하드 코딩 하지 않고 동적 쿼리를 작성할 수 있습니다.

예를 들어 각 장치 센서의 임계값을 포함 하는 **참조 데이터** 를 샘플 데이터 스트림으로 조인 해야 한다고 가정 합니다. 이러한 참조 데이터의 코드 조각은 다음과 같습니다.

```json
{
    "DeviceId" : "12345",
    "SensorName" : "Temperature",
    "Value" : 85
},
{
    "DeviceId" : "12345",
    "SensorName" : "Humidity",
    "Value" : 65
}
```

이 문서의 목표는이 문서의 맨 위에 있는 샘플 데이터 집합을 참조 데이터에 조인 하 고 각 센서 측정값에 대 한 이벤트를 임계값 이상으로 출력 하는 것입니다. 즉, 위의 단일 이벤트는 여러 센서가 각 임계값을 초과 하 여 조인 덕분에 여러 출력 이벤트를 생성할 수 있습니다. 조인 없이 비슷한 결과를 얻으려면 아래 섹션을 참조 하세요.

```SQL
SELECT
    input.DeviceID,
    thresholds.SensorName,
    "Alert : Sensor above threshold" AS AlertMessage
FROM input      -- stream input
JOIN thresholds -- reference data input
ON
    input.DeviceId = thresholds.DeviceId
WHERE
    GetRecordPropertyValue(input.SensorReadings, thresholds.SensorName) > thresholds.Value
```

**GetRecordPropertyValue** 은 참조 데이터에서 들어오는 속성 이름과 일치 하는 *SensorReadings*의 속성을 선택 합니다. 그런 다음 *SensorReadings* 에서 연결 된 값이 추출 됩니다.

결과는 다음과 같습니다.

|DeviceID|SensorName|Alertmessage가 잘못|
|-|-|-|
|12345|습도|경고: 임계값 초과 센서|

### <a name="convert-record-fields-into-separate-events"></a>레코드 필드를 개별 이벤트로 변환

레코드 필드를 별도의 이벤트로 변환하려면 [APPLY](https://docs.microsoft.com/stream-analytics-query/apply-azure-stream-analytics) 연산자를 [GetRecordProperties](https://docs.microsoft.com/stream-analytics-query/getrecordproperties-azure-stream-analytics) 함수와 함께 사용합니다.

원본 예제 데이터를 사용 하 여 다음 쿼리를 사용 하 여 속성을 여러 이벤트로 추출할 수 있습니다.

```SQL
SELECT
    event.DeviceID,
    sensorReading.PropertyName,
    sensorReading.PropertyValue
FROM input as event
CROSS APPLY GetRecordProperties(event.SensorReadings) AS sensorReading
```

결과는 다음과 같습니다.

|DeviceID|SensorName|Alertmessage가 잘못|
|-|-|-|
|12345|온도|80|
|12345|습도|70|
|12345|CustomSensor01|5|
|12345|CustomSensor02|99|
|12345|SensorMetadata|[개체 개체]|

를 [와 함께](https://docs.microsoft.com/stream-analytics-query/with-azure-stream-analytics)사용 하 여 해당 이벤트를 다른 대상으로 라우팅할 수 있습니다.

```SQL
WITH Stage0 AS
(
    SELECT
        event.DeviceID,
        sensorReading.PropertyName,
        sensorReading.PropertyValue
    FROM input as event
    CROSS APPLY GetRecordProperties(event.SensorReadings) AS sensorReading
)

SELECT DeviceID, PropertyValue AS Temperature INTO TemperatureOutput FROM Stage0 WHERE PropertyName = 'Temperature'
SELECT DeviceID, PropertyValue AS Humidity INTO HumidityOutput FROM Stage0 WHERE PropertyName = 'Humidity'
```

## <a name="array-data-types"></a>배열 데이터 형식

배열 데이터 형식은 정렬된 값의 컬렉션입니다. 배열 값에 일반적인 몇 가지 연산은 아래에 자세히 나와 있습니다. 이러한 예제에서는 [GetArrayElement](https://docs.microsoft.com/stream-analytics-query/getarrayelement-azure-stream-analytics), [GetArrayElements](https://docs.microsoft.com/stream-analytics-query/getarrayelements-azure-stream-analytics), [GetArrayLength](https://docs.microsoft.com/stream-analytics-query/getarraylength-azure-stream-analytics) 함수 및 [APPLY](https://docs.microsoft.com/stream-analytics-query/apply-azure-stream-analytics) 연산자를 사용합니다.

다음은 단일 이벤트의 예입니다. `CustomSensor03`와 `SensorMetadata`는 모두 **배열**형식입니다.

```json
{
    "DeviceId" : "12345",
    "SensorReadings" :
    {
        "Temperature" : 80,
        "Humidity" : 70,
        "CustomSensor01" : 5,
        "CustomSensor02" : 99,
        "CustomSensor03": [12,-5,0]
     },
    "SensorMetadata":[
        {          
            "smKey":"Manufacturer",
            "smValue":"ABC"                
        },
        {
            "smKey":"Version",
            "smValue":"1.2.45"
        }
    ]
}
```

### <a name="working-with-a-specific-array-element"></a>특정 배열 요소 작업

지정된 인덱스에서 배열 요소를 선택합니다(첫 번째 배열 요소 선택).

```SQL
SELECT
    GetArrayElement(SensorReadings.CustomSensor03, 0) AS firstElement
FROM input
```

결과는 다음과 같습니다.

|firstElement|
|-|
|12|

### <a name="select-array-length"></a>배열 길이 선택

```SQL
SELECT
    GetArrayLength(SensorReadings.CustomSensor03) AS arrayLength
FROM input
```

결과는 다음과 같습니다.

|arrayLength|
|-|
|3|

### <a name="convert-array-elements-into-separate-events"></a>배열 요소를 개별 이벤트로 변환

모든 배열 요소를 개별 이벤트로 선택합니다. [APPLY](https://docs.microsoft.com/stream-analytics-query/apply-azure-stream-analytics) 연산자는 [GetArrayElements](https://docs.microsoft.com/stream-analytics-query/getarrayelements-azure-stream-analytics) 기본 제공 함수와 함께 모든 배열 요소를 개별 이벤트로 추출합니다.

```SQL
SELECT
    DeviceId,
    CustomSensor03Record.ArrayIndex,
    CustomSensor03Record.ArrayValue
FROM input
CROSS APPLY GetArrayElements(SensorReadings.CustomSensor03) AS CustomSensor03Record

```

결과는 다음과 같습니다.

|deviceId|ArrayIndex|ArrayValue|
|-|-|-|
|12345|0|12|
|12345|1|-5|
|12345|2|0|

```SQL
SELECT   
    i.DeviceId, 
    SensorMetadataRecords.ArrayValue.smKey as smKey,
    SensorMetadataRecords.ArrayValue.smValue as smValue
FROM input i
CROSS APPLY GetArrayElements(SensorMetadata) AS SensorMetadataRecords
 ```
 
결과는 다음과 같습니다.

|deviceId|smKey|smValue|
|-|-|-|
|12345|제조업체|ABC|
|12345|버전|1.2.45|

추출 된 필드가 열에 표시 되어야 하는 경우 [조인](https://docs.microsoft.com/stream-analytics-query/join-azure-stream-analytics) 작업 외에 [WITH](https://docs.microsoft.com/stream-analytics-query/with-azure-stream-analytics) 구문을 사용 하 여 데이터 집합을 피벗 할 수 있습니다. 해당 조인은 중복을 방지 하는 [시간 경계](https://docs.microsoft.com/stream-analytics-query/join-azure-stream-analytics#BKMK_DateDiff) 조건이 필요 합니다.

```SQL
WITH DynamicCTE AS (
    SELECT   
        i.DeviceId,
        SensorMetadataRecords.ArrayValue.smKey as smKey,
        SensorMetadataRecords.ArrayValue.smValue as smValue
    FROM input i
    CROSS APPLY GetArrayElements(SensorMetadata) AS SensorMetadataRecords 
)

SELECT
    i.DeviceId,
    i.Location.*,
    V.smValue AS 'smVersion',
    M.smValue AS 'smManufacturer'
FROM input i
LEFT JOIN DynamicCTE V ON V.smKey = 'Version' and V.DeviceId = i.DeviceId AND DATEDIFF(minute,i,V) BETWEEN 0 AND 0 
LEFT JOIN DynamicCTE M ON M.smKey = 'Manufacturer' and M.DeviceId = i.DeviceId AND DATEDIFF(minute,i,M) BETWEEN 0 AND 0
```

결과는 다음과 같습니다.

|deviceId|Lat|장기|smVersion|smManufacturer|
|-|-|-|-|-|
|12345|47|122|1.2.45|ABC|

## <a name="see-also"></a>참고 항목
[Azure Stream Analytics의 데이터 형식](https://docs.microsoft.com/stream-analytics-query/data-types-azure-stream-analytics)
