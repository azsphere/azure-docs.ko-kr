---
title: PowerShell을 사용 하 여 Apache Hadoop 클러스터 관리-Azure HDInsight
description: Azure PowerShell을 사용하여 HDInsight의 Apache Hadoop 클러스터에 대한 관리 작업을 수행하는 방법에 대해 알아봅니다.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 12/09/2019
ms.openlocfilehash: e37571b0078b4966aab9f505ddf88c2edb353197
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75435635"
---
# <a name="manage-apache-hadoop-clusters-in-hdinsight-by-using-azure-powershell"></a>Azure PowerShell을 사용하여 HDInsight의 Apache Hadoop 클러스터 관리

[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Azure PowerShell은 Azure에서 작업의 배포와 관리를 제어 및 자동화하기 위해 사용할 수 있습니다. 이 문서에서는 Azure PowerShell Az module을 사용 하 여 Azure HDInsight에서 [Apache Hadoop](https://hadoop.apache.org/) 클러스터를 관리 하는 방법에 대해 알아봅니다. HDInsight PowerShell cmdlet의 목록은 [Az. hdinsight 참조](https://docs.microsoft.com/powershell/module/az.hdinsight)를 참조 하세요.

Azure 구독이 아직 없는 경우 시작하기 전에 [무료 계정](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)을 만듭니다.

## <a name="prerequisites"></a>필수 조건

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

PowerShell [Az 모듈](https://docs.microsoft.com/powershell/azure/overview)이 설치되었습니다.

## <a name="create-clusters"></a>클러스터 만들기

[Azure PowerShell을 사용하여 HDInsight에서 Linux 기반 클러스터 만들기](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)

## <a name="list-clusters"></a>클러스터 나열

현재 구독의 클러스터를 모두 나열하려면 다음 명령을 사용합니다.

```powershell
Get-AzHDInsightCluster
```

## <a name="show-cluster"></a>클러스터 표시

현재 구독의 특정 클러스터 세부 정보를 표시하려면 다음 명령을 사용합니다.

```powershell
Get-AzHDInsightCluster -ClusterName <Cluster Name>
```

## <a name="delete-clusters"></a>클러스터 삭제

클러스터를 삭제하려면 다음 명령을 사용합니다.

```powershell
Remove-AzHDInsightCluster -ClusterName <Cluster Name>
```

또한 클러스터를 포함하는 리소스 그룹을 제거하여 클러스터를 삭제할 수도 있습니다. 리소스 그룹을 삭제하면 기본 스토리지 계정을 포함한 그룹 내 모든 리소스가 삭제됩니다.

```powershell
Remove-AzResourceGroup -Name <Resource Group Name>
```

## <a name="scale-clusters"></a>클러스터 크기 조정

클러스터 크기 조정 기능을 사용하여 클러스터를 다시 생성하지 않고 Azure HDInsight에서 실행되는 클러스터에서 사용되는 작업자 노드 수를 변경합니다. Azure PowerShell을 사용하여 Hadoop 클러스터 크기를 변경하려면 클라이언트 컴퓨터에서 다음 명령을 실행합니다.

```powershell
Set-AzHDInsightClusterSize -ClusterName <Cluster Name> -TargetInstanceCount <NewSize>
```

 클러스터 크기 조정에 대 한 자세한 내용은 [HDInsight 클러스터 크기 조정](./hdinsight-scaling-best-practices.md)을 참조 하세요.

## <a name="grantrevoke-access"></a>액세스 권한 부여/해지

HDInsight 클러스터에는 다음과 같은 HTTP 웹 서비스가 있습니다(이러한 모든 서비스에 RESTful 엔드포인트가 있음).

* ODBC
* JDBC
* Ambari
* Oozie
* Templeton

이러한 서비스에는 기본적으로 액세스 권한이 부여됩니다. 액세스 권한을 해지/부여할 수 있습니다. 해지하려면:

```powershell
Revoke-AzHDInsightHttpServicesAccess -ClusterName <Cluster Name>
```

권한하려면:

```powershell
$clusterName = "<HDInsight Cluster Name>"

# Credential option 1
$hadoopUserName = "admin"
$hadoopUserPassword = '<Enter the Password>'
$hadoopUserPW = ConvertTo-SecureString -String $hadoopUserPassword -AsPlainText -Force
$credential = New-Object System.Management.Automation.PSCredential($hadoopUserName,$hadoopUserPW)

# Credential option 2
#$credential = Get-Credential -Message "Enter the HTTP username and password:" -UserName "admin"

Grant-AzHDInsightHttpServicesAccess -ClusterName $clusterName -HttpCredential $credential
```

> [!NOTE]  
> 액세스 권한을 부여/해지하여 클러스터 사용자 이름 및 암호를 다시 설정합니다.

액세스 권한의 부여/해지는 포털을 통해서도 수행할 수 있습니다. [Azure Portal를 사용 하 여 HDInsight에서 Apache Hadoop 클러스터 관리](hdinsight-administer-use-portal-linux.md)를 참조 하세요.

## <a name="update-http-user-credentials"></a>HTTP 사용자 자격 증명 업데이트

Grant/revoke HTTP 액세스와 동일한 절차입니다. 클러스터에 HTTP 액세스 권한이 부여된 경우 먼저 해당 권한을 해지해야 합니다.  그런 다음 새 HTTP 사용자 자격 증명을 사용하여 액세스 권한을 부여합니다.

## <a name="find-the-default-storage-account"></a>기본 스토리지 계정 찾기

다음 PowerShell 스크립트에서는 클러스터에 대한 기본 스토리지 계정 이름 및 관련 정보를 가져오는 방법을 보여 줍니다.

```powershell
#Connect-AzAccount
$clusterName = "<HDInsight Cluster Name>"

$clusterInfo = Get-AzHDInsightCluster -ClusterName $clusterName
$storageInfo = $clusterInfo.DefaultStorageAccount.split('.')
$defaultStoreageType = $storageInfo[1]
$defaultStorageName = $storageInfo[0]

echo "Default Storage account name: $defaultStorageName"
echo "Default Storage account type: $defaultStoreageType"

if ($defaultStoreageType -eq "blob")
{
    $defaultBlobContainerName = $cluster.DefaultStorageContainer
    $defaultStorageAccountKey = (Get-AzStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey

    echo "Default Blob container name: $defaultBlobContainerName"
    echo "Default Storage account key: $defaultStorageAccountKey"
}
```

## <a name="find-the-resource-group"></a>리소스 그룹 찾기

Resource Manager 모드에서 각 HDInsight 클러스터는 Azure 리소스 그룹에 속합니다.  리소스 그룹을 찾으려면:

```powershell
$clusterName = "<HDInsight Cluster Name>"

$cluster = Get-AzHDInsightCluster -ClusterName $clusterName
$resourceGroupName = $cluster.ResourceGroup
```

## <a name="submit-jobs"></a>작업 제출

**MapReduce 작업을 제출하려면**

[HDInsight에 포함된 MapReduce 예제 실행](hadoop/apache-hadoop-run-samples-linux.md)을 참조하세요.

**Apache Hive 작업을 제출하려면**

[PowerShell을 사용하여 Apache Hive 쿼리 실행](hadoop/apache-hadoop-use-hive-powershell.md)을 참조하세요.

**Apache Sqoop 작업을 제출하려면**

[HDInsight에서 Apache Sqoop 사용](hadoop/hdinsight-use-sqoop.md)을 참조하세요.

**Apache Oozie 작업을 제출하려면**

[Apache Hadoop과 함께 Apache Oozie를 사용하여 HDInsight에서 워크플로 정의 및 실행](hdinsight-use-oozie-linux-mac.md)을 참조하세요.

## <a name="upload-data-to-azure-blob-storage"></a>개발자와 관리자는 응용 프로그램을 빌드하거나 관리하는 데 필요한 유틸리티를 모든 가상 머신이나 역할 인스턴스에 다 설치하지 않고, 모든 구성 요소에서 사용할 수 있는 File Storage 공유에 저장할 수 있습니다.

[HDInsight에 데이터 업로드를](hdinsight-upload-data.md)참조 하세요.

## <a name="see-also"></a>참고 항목

* [Az cmdlet](https://docs.microsoft.com/powershell/module/az.hdinsight/?view=azps-3.1.0#hdinsight)
* [Azure Portal을 사용하여 HDInsight의 Apache Hadoop 클러스터 관리](hdinsight-administer-use-portal-linux.md)
* [명령줄 인터페이스를 사용 하 여 HDInsight 관리](hdinsight-administer-use-command-line.md)
* [HDInsight 클러스터 만들기](hdinsight-hadoop-provision-linux-clusters.md)
* [프로그래밍 방식으로 Apache Hadoop 작업 제출](hadoop/submit-apache-hadoop-jobs-programmatically.md)
* [Azure HDInsight 시작](hadoop/apache-hadoop-linux-tutorial-get-started.md)
