---
title: PowerShell을 사용하여 Azure Data Lake Storage Gen1 시작 | Microsoft Docs
description: Azure PowerShell을 사용하여 Azure Data Lake Storage Gen1 계정을 만들고 기본 작업을 수행합니다.
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: twooley
ms.openlocfilehash: 6635b572d11339307083b8aa2aaaba69dc731101
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58885862"
---
# <a name="get-started-with-azure-data-lake-storage-gen1-using-azure-powershell"></a>Azure PowerShell을 사용하여 Azure Data Lake Storage Gen1 시작
> [!div class="op_single_selector"]
> * [포털](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [Azure CLI](data-lake-store-get-started-cli-2.0.md)
>
>

[!INCLUDE [data-lake-storage-gen1-rename-note.md](../../includes/data-lake-storage-gen1-rename-note.md)]

Azure PowerShell을 사용하여 Azure Data Lake Storage Gen1 계정을 만들고 폴더 만들기, 데이터 파일 업로드 및 다운로드, 계정 삭제 등의 기본 작업을 수행하는 방법에 대해 알아봅니다. Data Lake Storage Gen1에 대한 자세한 내용은 [Data Lake Storage Gen1 개요](data-lake-store-overview.md)를 참조하세요.

## <a name="prerequisites"></a>필수 조건

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

* **Azure 구독**. [Azure 평가판](https://azure.microsoft.com/pricing/free-trial/)을 참조하세요.
* **Azure PowerShell 1.0 이상**. [Azure PowerShell 설치 및 구성 방법](/powershell/azure/overview)을 참조하세요.

## <a name="authentication"></a>Authentication
이 문서에서는 Azure 계정 자격 증명을 입력하라는 메시지가 표시되는 Data Lake Storage Gen1에서 더 간단한 인증 방법을 사용합니다. Data Lake Storage Gen1 계정 및 파일 시스템에 대한 액세스 수준은 로그인한 사용자의 액세스 수준에 따라 결정됩니다. 단, Data Lake Storage Gen1을 통해 인증하는 다른 방법인 **최종 사용자 인증** 또는 **서비스간 인증**도 있습니다. 지침 및 인증 방법에 대한 자세한 내용은 [최종 사용자 인증](data-lake-store-end-user-authenticate-using-active-directory.md) 또는 [서비스 간 인증](data-lake-store-authenticate-using-active-directory.md)을 참조하세요.

## <a name="create-a-data-lake-storage-gen1-account"></a>Data Lake Storage Gen1 계정 만들기
1. 바탕 화면에서 새 Windows PowerShell 창을 엽니다. 다음 코드 조각을 입력하여 Azure 계정에 로그인하고, 구독을 설정한 다음, Data Lake Storage Gen1 공급자를 등록합니다. 로그인하라는 메시지가 표시되면 구독 관리자/소유자 중 하나로 로그인해야 합니다.

        # Log in to your Azure account
        Connect-AzAccount

        # List all the subscriptions associated to your account
        Get-AzSubscription

        # Select a subscription
        Set-AzContext -SubscriptionId <subscription ID>

        # Register for Azure Data Lake Storage Gen1
        Register-AzResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"
2. Data Lake Storage Gen1 계정은 Azure 리소스 그룹과 연결됩니다. Azure 리소스 그룹을 만드는 작업부터 시작합니다.

        $resourceGroupName = "<your new resource group name>"
        New-AzResourceGroup -Name $resourceGroupName -Location "East US 2"

    ![Azure 리소스 그룹 만들기](./media/data-lake-store-get-started-powershell/ADL.PS.CreateResourceGroup.png "Azure 리소스 그룹 만들기")
3. Data Lake Storage Gen1 계정을 만듭니다. 지정하는 이름은 소문자와 숫자만 포함해야 합니다.

        $dataLakeStorageGen1Name = "<your new Data Lake Storage Gen1 account name>"
        New-AzDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStorageGen1Name -Location "East US 2"

    ![Data Lake Storage Gen1 계정 만들기](./media/data-lake-store-get-started-powershell/ADL.PS.CreateADLAcc.png "Data Lake Storage Gen1 계정 만들기")
4. 계정이 성공적으로 만들어졌는지 확인합니다.

        Test-AzDataLakeStoreAccount -Name $dataLakeStorageGen1Name

    cmdlet에 대한 출력은 **True**여야 합니다.

## <a name="create-directory-structures-in-your-data-lake-storage-gen1-account"></a>Data Lake Storage Gen1 계정에서 디렉터리 구조 만들기
Data Lake Storage Gen1 계정 아래에 디렉터리를 만들어 데이터를 관리하고 저장할 수 있습니다.

1. 루트 디렉터리를 지정합니다.

        $myrootdir = "/"
2. 지정된 루트 아래 **mynewdirectory** 라는 새 디렉터리를 만듭니다.

        New-AzDataLakeStoreItem -Folder -AccountName $dataLakeStorageGen1Name -Path $myrootdir/mynewdirectory
3. 새 디렉터리가 성공적으로 만들어졌는지 확인합니다.

        Get-AzDataLakeStoreChildItem -AccountName $dataLakeStorageGen1Name -Path $myrootdir

    다음 스크린샷에 표시된 것처럼 출력됩니다.

    ![디렉터리 확인](./media/data-lake-store-get-started-powershell/ADL.PS.Verify.Dir.Creation.png "디렉터리 확인")

## <a name="upload-data-to-your-data-lake-storage-gen1-account"></a>Data Lake Storage Gen1 계정에 데이터 업로드
데이터는 루트 수준에서 직접 Data Lake Storage Gen1에 업로드하거나 계정 내에 만든 디렉터리에 업로드할 수 있습니다. 이 섹션의 코드 조각은 이전 섹션에서 만든 디렉터리(**mynewdirectory**)에 일부 샘플 데이터를 업로드하는 방법을 보여 줍니다.

업로드할 일부 샘플 데이터를 찾는 경우 **Azure 데이터 레이크 Git 리포지토리** 의 [Ambulance Data](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData)폴더에 있을 수 있습니다. 파일을 다운로드하고 컴퓨터의 로컬 디렉터리(예: C:\sampledata\)에 저장합니다.

    Import-AzDataLakeStoreItem -AccountName $dataLakeStorageGen1Name -Path "C:\sampledata\vehicle1_09142014.csv" -Destination $myrootdir\mynewdirectory\vehicle1_09142014.csv


## <a name="rename-download-and-delete-data-from-your-data-lake-storage-gen1-account"></a>Data Lake Storage Gen1 계정에서 데이터 이름 바꾸기, 다운로드 및 삭제
파일의 이름을 바꾸려면 다음 명령을 사용합니다.

    Move-AzDataLakeStoreItem -AccountName $dataLakeStorageGen1Name -Path $myrootdir\mynewdirectory\vehicle1_09142014.csv -Destination $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

파일을 다운로드하려면 다음 명령을 사용합니다.

    Export-AzDataLakeStoreItem -AccountName $dataLakeStorageGen1Name -Path $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv -Destination "C:\sampledata\vehicle1_09142014_Copy.csv"

파일을 삭제하려면 다음 명령을 사용합니다.

    Remove-AzDataLakeStoreItem -AccountName $dataLakeStorageGen1Name -Paths $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

메시지가 표시되면 **Y** 를 입력하여 항목을 삭제합니다. 삭제할 파일이 둘 이상 있는 경우 쉼표로 구분하여 모든 경로를 입력할 수 있습니다.

    Remove-AzDataLakeStoreItem -AccountName $dataLakeStorageGen1Name -Paths $myrootdir\mynewdirectory\vehicle1_09142014.csv, $myrootdir\mynewdirectoryvehicle1_09142014_Copy.csv

## <a name="delete-your-data-lake-storage-gen1-account"></a>Data Lake Storage Gen1 계정 삭제
다음 명령을 사용하여 Data Lake Storage Gen1 계정을 삭제합니다.

    Remove-AzDataLakeStoreAccount -Name $dataLakeStorageGen1Name

메시지가 표시되면 **Y** 를 입력하여 계정을 삭제합니다.

## <a name="next-steps"></a>다음 단계
* [Azure Data Lake Storage Gen1에서 PowerShell을 사용하기 위한 성능 조정 지침](data-lake-store-performance-tuning-powershell.md)
* [빅 데이터 요구 사항에 대 한 사용 하 여 Azure 데이터 레이크 저장소 Gen1](data-lake-store-data-scenarios.md) 
* [데이터 레이크 저장소 Gen1의 데이터 보안 유지](data-lake-store-secure-data.md)
* [Azure Data Lake Analytics를 사용 하 여 Data Lake Storage Gen1를 사용 하 여](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Azure HDInsight를 Data Lake Storage Gen1 사용](data-lake-store-hdinsight-hadoop-use-portal.md)
