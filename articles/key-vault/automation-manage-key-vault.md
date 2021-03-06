---
title: Azure Automation을 사용하여 Azure Key Vault 관리 - Azure Key Vault | Microsoft Docs
description: Azure Automation 서비스를 사용하여 Azure Key Vault를 관리하는 방법에 대해 알아봅니다.
services: Key-Vault, automation
documentationcenter: ''
author: mgoedtel
manager: jwhit
editor: ''
ms.assetid: 4e780762-19b6-4ca6-b894-ebb44c538f35
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: magoedte
ms.openlocfilehash: ace32968808dfa919e6ca5d5777818d2672249fe
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58224876"
---
# <a name="managing-azure-key-vault-using-azure-automation"></a>Azure Automation을 사용하여 Azure Key Vault 관리

이 가이드에서는 Azure Automation 서비스 및 이 서비스를 사용하여 Azure Key Vault에서 키 및 암호 관리를 간소화하는 방법에 대해 소개합니다.

## <a name="what-is-azure-automation"></a>Azure Automation 정의

[Azure Automation](../automation/automation-intro.md)은 프로세스 자동화 및 원하는 상태 구성을 통해 클라우드 관리를 간소화하기 위한 Azure 서비스입니다. Azure Automation, 수동, 반복되는, 장기 실행 및 오류가 발생하기 쉬운 작업을 사용하면 조직의 안정성, 효율성 및 가치 창출 시간을 개선하도록 자동화할 수 있습니다.

Azure Automation는 요구 사항에 맞게 크기가 조정되는 매우 안정적이며 가용성이 높은 워크플로 실행 엔진을 제공합니다. Azure Automation에서는 작업이 정확히 필요한 시간에 수행되도록 수동, 타사 시스템 또는 예약된 간격에 의해 프로세스를 시작할 수 있습니다.

Azure Automation에서 자동으로 실행되도록 클라우드 관리 작업을 이동하여 작업 오버헤드를 줄이고 IT 및 DevOps 직원들이 비즈니스 가치를 추가하는 작업에 집중할 수 있게 합니다.

## <a name="how-can-azure-automation-help-manage-azure-key-vault"></a>Azure Automation을 통해 Azure Key Vault를 쉽게 관리하려면 어떻게 해야 하나요?

Key Vault는 [AzureRM Key Vault cmdlet](https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) 및 [Azure 클래식 Key Vault cmdlet](https://docs.microsoft.com/powershell/module/servicemanagement/azure)을 사용하여 Azure Automation으로 관리될 수 있습니다. 클래식 Key Vault 관리를 위한 Azure 모듈은 Azure Automation에 자동으로 제공되며, 서비스 내에서 많은 Key Vault 관리 작업을 수행할 수 있도록, [AzureRM-KeyVault 모듈](https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) 을 Azure Automation으로 가져올 수 있습니다. Azure Automation으로 모듈을 가져오는 방법에 대 한 자세한 내용은 참조 하세요 [Azure Automation에서 모듈을 관리](../automation/shared-resources/modules.md) 에서 복잡 한 작업을 자동화 하려면 다른 Azure 서비스용 cmdlet 사용 하 여 Azure Automation에서 이러한 cmdlet도 페어링할 수 있습니다 Azure 서비스와 타사 시스템입니다.

Azure Key Vault cmdlet을 사용하여 다음과 같은 작업을 수행할 수 있습니다. 

* 주요 자격 증명 모음 만들기 및 구성
* 키 만들기 또는 가져오기
* 암호 만들기 또는 업데이트
* 키 특성 업데이트
* 키 또는 암호 가져오기
* 키 또는 암호 삭제

다음은 PowerShell을 사용하여 주요 자격 증명 모음을 관리하는 몇 가지 예제입니다.  

* [Azure Key Vault - 단계별](https://blogs.technet.microsoft.com/kv/2015/06/02/azure-key-vault-step-by-step)
* [Azure Key Vault 설정 및 구성](https://www.simple-talk.com/cloud/platform-as-a-service/setting-up-and-configuring-an-azure-key-vault)

## <a name="next-steps"></a>다음 단계

Azure Automation의 기본 사항과 이를 사용하여 Azure Key Vault를 관리하는 방법을 알아보았으므로, 이제 다음 링크에 따라 Azure Automation에 대해 자세히 알아보세요.

* Azure Automation [시작 자습서](../automation/automation-first-runbook-graphical.md)를 참조하세요.
* [Azure Key Vault PowerShell 스크립트](https://gallery.technet.microsoft.com/scriptcenter/site/search?query=azure%20key%20vault&f%5B0%5D.Value=azure%20key%20vault&f%5B0%5D.Type=SearchText&ac=5)를 참조하세요.

