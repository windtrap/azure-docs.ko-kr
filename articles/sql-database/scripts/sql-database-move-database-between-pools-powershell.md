---
title: PowerShell 예제-Azure SQL Database 이동-탄력적 풀 | Microsoft Docs
description: PowerShell을 사용하여 탄력적 풀 간에 SQL Database를 이동하는 Azure PowerShell 스크립트 샘플
services: sql-database
ms.service: sql-database
ms.subservice: elastic-pools
ms.custom: ''
ms.devlang: PowerShell
ms.topic: sample
author: CarlRabeler
ms.reviewer: ''
ms.author: carlrab
manager: craigg
ms.date: 03/12/2019
ms.openlocfilehash: 9b179bb7021e77acd99b529d6ab3b0d638858dbd
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2019
ms.locfileid: "57894243"
---
# <a name="use-powershell-to-create-elastic-pools-and-move-databases-between-elastic-pools"></a>PowerShell을 사용한 탄력적 풀 만들기 및 탄력적 풀 간에 데이터베이스 이동

이 PowerShell 스크립트 예제는 두 개의 탄력적 풀을 만들고 한 탄력적 풀에서 다른 탄력적 풀로 데이터베이스를 이동한 다음, 탄력적 풀에서 단일 데이터베이스 계산 크기로 데이터베이스를 이동합니다.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]
[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

PowerShell을 로컬로 설치하고 사용하도록 선택하는 경우 이 자습서에는 AZ PowerShell 1.4.0 이상이 필요합니다. 업그레이드해야 하는 경우 [Azure PowerShell 모듈 설치](/powershell/azure/install-az-ps)를 참조하세요. 또한 PowerShell을 로컬로 실행하는 경우 `Connect-AzAccount`를 실행하여 Azure와 연결해야 합니다.

## <a name="sample-script"></a>샘플 스크립트

[!code-powershell-interactive[main](../../../powershell_scripts/sql-database/move-database-between-pools-and-standalone/move-database-between-pools-and-standalone.ps1?highlight=18-19 "Move database between pools")]

## <a name="clean-up-deployment"></a>배포 정리

스크립트 샘플을 실행한 후에 다음 명령을 사용하여 리소스 그룹 및 관련된 모든 리소스를 제거할 수 있습니다.

```powershell
Remove-AzResourceGroup -ResourceGroupName $resourcegroupname
```

## <a name="script-explanation"></a>스크립트 설명

이 스크립트는 다음 명령을 사용합니다. 테이블에 있는 각 명령은 명령에 해당하는 문서에 연결됩니다.

| 명령 | 메모 |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | 모든 리소스가 저장되는 리소스 그룹을 만듭니다. |
| [New-AzSqlServer](/powershell/module/az.sql/new-azsqlserver) | 단일 데이터베이스 또는 탄력적 풀을 호스트하는 SQL Database 서버를 만듭니다. |
| [New-AzSqlElasticPool](/powershell/module/az.sql/new-azsqlelasticpool) | 탄력적 풀을 만듭니다. |
| [New-AzSQLDatabase](/powershell/module/az.sql/new-azsqldatabase) | SQL Database 서버의 데이터베이스를 독립 실행형 데이터베이스 또는 풀링된 데이터베이스로 만듭니다. |
| [Set-AzSqlDatabase](/powershell/module/az.sql/set-azsqldatabase) | 데이터베이스 속성을 업데이트하거나 탄력적 풀 내부, 외부 또는 내외부 상호 간에 데이터베이스를 이동합니다. |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | 모든 중첩 리소스를 포함한 리소스 그룹을 삭제합니다. |
|||

## <a name="next-steps"></a>다음 단계

Azure PowerShell에 대한 자세한 내용은 [Azure PowerShell 설명서](/powershell/azure/overview)를 참조하세요.

추가 SQL Database PowerShell 스크립트 샘플은 [Azure SQL Database PowerShell 스크립트](../sql-database-powershell-samples.md)에 있습니다.
