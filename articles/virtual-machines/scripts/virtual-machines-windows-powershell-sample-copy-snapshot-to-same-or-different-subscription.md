---
title: Azure PowerShell 스크립트 샘플 - 관리 디스크의 스냅숏을 동일한 구독이나 다른 구독으로 복사(이동) | Microsoft Docs
description: Azure PowerShell 스크립트 샘플 - 관리 디스크의 스냅숏을 동일한 구독이나 다른 구독으로 복사(이동)
services: virtual-machines-windows
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/28/2019
ms.author: ramankum
ms.openlocfilehash: 6164a92e19d8657525029bca9a749baadcb49362
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/02/2019
ms.locfileid: "57242890"
---
# <a name="copy-snapshot-of-a-managed-disk-in-same-subscription-or-different-subscription-with-powershell"></a>PowerShell을 사용하여 같은 구독 또는 다른 구독에 관리 디스크의 스냅숏 복사

이 스크립트는 관리 디스크의 스냅숏을 동일한 구독이나 다른 구독으로 복사합니다. 다음 시나리오에서 이 스크립트를 사용하세요.

1. Premium Storage(Premium_LRS)의 스냅숏을 Standard Storage(Standard_LRS 또는 Standard_ZRS)로 마이그레이션하여 비용을 절감합니다.
1. 로컬 중복 저장소(Premium_LRS, Standard_LRS)의 스냅숏을 영역 중복 저장소(Standard_ZRS)로 마이그레이션하여 ZRS 스토리지의 높은 안정성을 누립니다.
1. 더 오래 보존하기 위해 스냅숏을 같은 지역의 다른 구독으로 이동합니다.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az-vm.md](../../../includes/updated-for-az-vm.md)]

## <a name="sample-script"></a>샘플 스크립트

[!code-powershell[main](../../../powershell_scripts/virtual-machine/copy-snapshot-to-same-or-different-subscription/copy-snapshot-to-same-or-different-subscription.ps1 "Copy snapshot")]

## <a name="script-explanation"></a>스크립트 설명

이 스크립트에서는 다음 명령을 사용하여 원본 스냅숏의 ID를 사용하여 대상 구독에 스냅숏을 만듭니다. 테이블에 있는 각 명령은 명령에 해당하는 문서에 연결됩니다.

| 명령 | 메모 |
|---|---|
| [New-AzSnapshotConfig](https://docs.microsoft.com/powershell/module/az.compute/New-AzSnapshotConfig) | 스냅숏 생성에 사용되는 스냅숏 구성을 만듭니다. 부모 스냅숏의 리소스 ID와 부모 스냅숏과 같은 위치를 포함합니다.  |
| [New-AzSnapshot](https://docs.microsoft.com/powershell/module/az.compute/New-AzDisk) | 매개 변수로 전달된 스냅숏 구성, 스냅숏 이름 및 리소스 그룹 이름을 사용하여 스냅숏을 만듭니다. |

## <a name="next-steps"></a>다음 단계

[스냅숏에서 가상 머신 만들기](./virtual-machines-windows-powershell-sample-create-vm-from-snapshot.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

Azure PowerShell 모듈에 대한 자세한 내용은 [Azure PowerShell 설명서](/powershell/azure/overview)를 참조하세요.

추가 가상 머신 PowerShell 스크립트 샘플은 [Azure Windows VM 설명서](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)에서 확인할 수 있습니다.
