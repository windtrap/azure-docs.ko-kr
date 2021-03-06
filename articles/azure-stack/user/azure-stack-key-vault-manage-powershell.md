---
title: PowerShell을 사용 하 여 Azure Stack에서 Key Vault 관리 | Microsoft Docs
description: PowerShell을 사용 하 여 Azure Stack에서 Key Vault를 관리 하는 방법 알아보기
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 22B62A3B-B5A9-4B8C-81C9-DA461838FAE5
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2019
ms.author: sethm
ms.lastreviewed: 01/16/2019
ms.openlocfilehash: d2324f9538ce8079be5e660a1613c1c093ecc85a
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58484599"
---
# <a name="manage-key-vault-in-azure-stack-using-powershell"></a>PowerShell을 사용 하 여 Azure Stack에서 Key Vault 관리

*적용 대상: Azure Stack 통합 시스템 및 Azure Stack 개발 키트*

PowerShell을 사용 하 여 Azure Stack의 주요 자격 증명 모음을 관리할 수 있습니다. Key Vault PowerShell cmdlet을 사용 하는 방법에 알아봅니다.

* 키 자격 증명 모음을 만듭니다.
* 저장 하 고 암호화 키 및 비밀을 관리 합니다.
* 사용자 또는 자격 증명 모음에서 작업을 호출 응용 프로그램에 권한을 부여 합니다.

>[!NOTE]
>이 문서에 설명 된 Key Vault PowerShell cmdlet은 Azure PowerShell SDK에 제공 됩니다.

## <a name="prerequisites"></a>필수 조건

* Azure Key Vault 서비스를 포함 하는 제품을 구독 해야 합니다.
* [Azure Stack 용 PowerShell 설치](azure-stack-powershell-install.md)합니다.
* [Azure Stack 사용자의 PowerShell 환경을 구성](azure-stack-powershell-configure-user.md)합니다.

## <a name="enable-your-tenant-subscription-for-key-vault-operations"></a>Key Vault 작업에 대 한 테 넌 트 구독을 사용 하도록 설정

Key vault에 대 한 모든 작업을 실행할 수 있습니다, 전에 테 넌 트 구독의 자격 증명 모음 작업에 대해 설정 되어 있는지 확인 해야 합니다. 자격 증명 모음 작업 설정 되어 있는지를 확인 하려면 다음 명령을 실행 합니다.

```powershell  
Get-AzureRmResourceProvider -ProviderNamespace Microsoft.KeyVault | ft -Autosize
```

**출력**

구독의 자격 증명 모음 작업에 사용 하는 경우 "RegistrationState"가 "등록 됨" key vault의 모든 리소스 종류에 대 한 출력을 보여 줍니다.

![키 자격 증명 모음 등록 상태](media/azure-stack-key-vault-manage-powershell/image1.png)

자격 증명 모음 작업을 사용할 경우에 구독에서 Key Vault 서비스를 등록 하려면 다음 명령을 호출 합니다.

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.KeyVault
```

**출력**

등록에 성공한 경우에 다음과 같은 출력이 반환 됩니다.

![등록](media/azure-stack-key-vault-manage-powershell/image2.png) key vault 명령을 호출 하면 "구독 'Microsoft.KeyVault' 네임 스페이스를 사용 하는 등록 되지 않았습니다."와 같은 오류가 발생할 수 있습니다 오류가 발생 하는 경우 이전에 설명한 지침에 따라 주요 자격 증명 모음 리소스 공급자를 설정 했는지 확인 합니다.

## <a name="create-a-key-vault"></a>키 자격 증명 모음 만들기

Key vault를 만들려면 먼저 key vault와 관련 된 리소스의 모든 리소스 그룹에 존재 하는 리소스 그룹을 만듭니다. 다음 명령을 사용 하 여 새 리소스 그룹을 만듭니다.

```powershell
New-AzureRmResourceGroup -Name "VaultRG" -Location local -verbose -Force

```

**출력**

![새 리소스 그룹](media/azure-stack-key-vault-manage-powershell/image3.png)

이제 사용 하 여 합니다 **New-azurermkeyvault** 명령을 이전에 만든 리소스 그룹에 key vault를 만듭니다. 이 명령은 세 가지 필수 매개 변수를 읽습니다: 리소스 그룹 이름, 키 자격 증명 모음 이름 및 지리적 위치입니다.

Key vault를 만들려면 다음 명령을 실행 합니다.

```powershell
New-AzureRmKeyVault -VaultName "Vault01" -ResourceGroupName "VaultRG" -Location local -verbose
```

**출력**

![새로운 Key Vault](media/azure-stack-key-vault-manage-powershell/image4.png)

이 명령의 출력은 사용자가 만든 key vault의 속성을 보여 줍니다. 응용 프로그램에서이 자격 증명이 모음에 액세스할 때 사용 해야 합니다 **자격 증명 모음 URI** 속성, 즉 "https:\//vault01.vault.local.azurestack.external"이 예제의 합니다.

### <a name="active-directory-federation-services-ad-fs-deployment"></a>Active Directory Federation Services (AD FS) 배포

AD FS 배포에서이 경고가 표시 될 수 있습니다. "액세스 정책이 설정 되지 않았습니다. 응용 프로그램 없거나 사용자에 게 액세스 권한이이 자격 증명이 모음을 사용 하 합니다. " 이 문제를 해결 하려면 자격 증명 모음의 액세스 정책을 사용 하 여 설정 합니다 [Set-azurermkeyvaultaccesspolicy](#authorize-an-application-to-use-a-key-or-secret) 명령:

```powershell
# Obtain the security identifier(SID) of the active directory user
$adUser = Get-ADUser -Filter "Name -eq '{Active directory user name}'"
$objectSID = $adUser.SID.Value

# Set the key vault access policy
Set-AzureRmKeyVaultAccessPolicy -VaultName "{key vault name}" -ResourceGroupName "{resource group name}" -ObjectId "{object SID}" -PermissionsToKeys {permissionsToKeys} -PermissionsToSecrets {permissionsToSecrets} -BypassObjectIdValidation
```

## <a name="manage-keys-and-secrets"></a>키 및 비밀 관리

자격 증명 모음을 만든 후 다음 단계를 사용 하 여를 만들고 자격 증명 모음에서 키 및 비밀을 관리 합니다.

### <a name="create-a-key"></a>키 만들기

사용 된 **Add-azurekeyvaultkey** 만들거나 key vault에 소프트웨어 보호 키를 가져오는 명령입니다.

```powershell
Add-AzureKeyVaultKey -VaultName "Vault01" -Name "Key01" -verbose -Destination Software
```

합니다 **대상** 소프트웨어 보호 키 임을 지정 매개 변수를 사용 합니다. 키를 성공적으로 만들어지면 명령 만든된 키의 세부 정보를 출력 합니다.

**출력**

![새 키](media/azure-stack-key-vault-manage-powershell/image5.png)

이제 해당 URI를 사용 하 여 만든된 키를 참조할 수 있습니다. 을 만들거나 기존 키와 같은 이름을 가진 키를 가져올 경우 원래 키를 새 키에 지정한 값으로 업데이트 됩니다. 키의 버전별 URI를 사용 하 여 이전 버전에 액세스할 수 있습니다. 예를 들면 다음과 같습니다.

* 사용 하 여 "https:\//vault10.vault.local.azurestack.external:443/keys/key01" 항상 현재 버전을 가져올 수 있습니다.
* 사용 하 여 "https:\//vault010.vault.local.azurestack.external:443/keys/key01/d0b36ee2e3d14e9f967b8b6b1d38938a"이 특정 버전을 가져올 수 있습니다.

### <a name="get-a-key"></a>키 가져오기

사용 된 **Get-azurekeyvaultkey** 명령 키를 읽고 해당 세부 정보입니다.

```powershell
Get-AzureKeyVaultKey -VaultName "Vault01" -Name "Key01"
```

### <a name="create-a-secret"></a>비밀 만들기

사용 된 **Set-azurekeyvaultsecret** 만들거나 자격 증명 모음에 비밀을 업데이트 하는 명령입니다. 암호는 존재 하지 않는 경우 만들어집니다. 암호의 새 버전이 이미 있는 경우에 생성 됩니다.

```powershell
$secretvalue = ConvertTo-SecureString "User@123" -AsPlainText -Force
Set-AzureKeyVaultSecret -VaultName "Vault01" -Name "Secret01" -SecretValue $secretvalue
```

**출력**

![비밀 만들기](media/azure-stack-key-vault-manage-powershell/image6.png)

### <a name="get-a-secret"></a>암호를 가져오려는 경우

사용 된 **Get-azurekeyvaultsecret** key vault에서 비밀을 읽도록 명령입니다. 이 명령은 모든 반환할 수 있습니다 또는 특정 버전의 비밀입니다.

```powershell
Get-AzureKeyVaultSecret -VaultName "Vault01" -Name "Secret01"
```

키와 비밀을 만든 후 외부 응용 프로그램 사용 권한을 부여할 수 있습니다.

## <a name="authorize-an-application-to-use-a-key-or-secret"></a>키 또는 비밀을 사용 하도록 응용 프로그램 권한 부여

사용 된 **Set-azurermkeyvaultaccesspolicy** 키 또는 key vault의 비밀에 액세스 하는 응용 프로그램 권한을 부여 하는 명령입니다.
다음 예제에서는 자격 증명 모음 이름은 *ContosoKeyVault* 권한을 부여 하려는 응용 프로그램의 클라이언트 ID 있고 *8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed*합니다. 응용 프로그램에 권한을 부여 하려면 다음 명령을 실행 합니다. 선택적으로 지정할 수는 **PermissionsToKeys** 사용자, 응용 프로그램 또는 보안 그룹에 대 한 사용 권한을 설정 하려면 매개 변수입니다.

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToKeys decrypt,sign
```

자격 증명 모음에서 비밀을 읽도록 동일한 응용 프로그램에 권한을 부여 하려는 경우 다음 cmdlet을 실행 합니다.

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300 -PermissionsToKeys Get
```

## <a name="next-steps"></a>다음 단계

* [Key Vault에 저장 된 암호를 사용 하 여 VM 배포](azure-stack-kv-deploy-vm-with-secret.md)
* [Key Vault에 저장 된 인증서를 사용 하 여 VM 배포](azure-stack-kv-push-secret-into-vm.md)
