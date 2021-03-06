---
title: Azurestack의 경우에서 MySQL 어댑터 RP에서 제공 하는 데이터베이스를 사용 하 여 | Microsoft Docs
description: MySQL 어댑터 리소스 공급자를 사용 하 여 프로 비전 하는 MySQL 데이터베이스 만들기 및 관리 하는 방법
services: azure-stack
documentationCenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/12/2019
ms.author: jeffgilb
ms.reviewer: quying
ms.lastreviewed: 10/16/2018
ms.openlocfilehash: 6eaba728b794c0102ec4e28791b218efa28b51b5
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56160767"
---
# <a name="create-mysql-databases"></a>MySQL 데이터베이스 만들기
MySQL 데이터베이스 서비스를 포함 하는 제품을 구독할 Azure Stack 사용자를 만들고 사용자 포털에서 MySQL 데이터베이스를 셀프 서비스를 관리할 수 있습니다.

## <a name="create-a-mysql-database"></a>MySQL 데이터베이스 만들기

1. Azure Stack 사용자 포털에 로그인 합니다.
2. 선택 **+ 리소스 만들기** > **데이터 + 저장소** > **MySQL Database** > **추가**합니다.
3. 아래 **MySQL 데이터베이스 만들기**, 데이터베이스 이름을 입력 하 고 사용자 환경에 대 한 필요에 따라 다른 설정을 구성 합니다.

    ![테스트 MySQL 데이터베이스 만들기](./media/azure-stack-mysql-rp-deploy/mysql-create-db.png)

4. 아래 **Create Database**를 선택 **SKU**합니다. 아래 **MySQL SKU 선택**, 데이터베이스에 대 한 SKU를 선택 합니다.

    ![MySQL SKU를 선택 합니다.](./media/azure-stack-mysql-rp-deploy/mysql-select-sku.png)

    >[!Note]
    >호스팅 서버는 Azure Stack에 추가 되 면 SKU가 할당 됩니다. 호스팅 서버 SKU에서 풀의 데이터베이스가 만들어집니다.

5. 아래 **로그인**를 선택 ***필요한 설정 구성***합니다.
6. 아래 **로그인을 선택**, 기존 로그인을 선택 하거나 선택할 수 있습니다 **+ 새 로그인 만들기** 새 로그인을 설정 하려면.  입력 한 **데이터베이스 로그인** 이름 및 **암호**를 선택한 후 **확인**합니다.

    ![새 데이터베이스 로그인 만들기](./media/azure-stack-mysql-rp-deploy/create-new-login.png)

    >[!NOTE]
    >데이터베이스 로그인 이름의 길이 MySQL 5.7 32 자를 초과할 수 없습니다. 이전 버전에서 16 자를 초과할 수 없습니다.

7. 선택 **만들기** 데이터베이스 설치를 마칩니다.

데이터베이스를 배포한 후 기록해 합니다 **연결 문자열** 아래에서 **Essentials**합니다. MySQL 데이터베이스에 액세스 해야 하는 모든 응용 프로그램에서이 문자열을 사용할 수 있습니다.

![MySQL 데이터베이스에 대 한 연결 문자열을 가져옵니다.](./media/azure-stack-mysql-rp-deploy/mysql-db-created.png)

## <a name="update-the-administrative-password"></a>관리자 암호를 업데이트 합니다.

MySQL 서버 인스턴스에서 변경 하 여 암호를 수정할 수 있습니다.

1. 선택 **관리 리소스** > **MySQL 호스팅 서버**합니다. 호스팅 서버를 선택 합니다.
2. 아래 **설정을**를 선택 **암호**합니다.
3. 아래 **암호**, 새 암호를 입력 한 다음 선택 **저장**합니다.

![관리자 암호를 업데이트 합니다.](./media/azure-stack-mysql-rp-deploy/mysql-update-password.png)

## <a name="next-steps"></a>다음 단계

[MySQL 리소스 공급자 업데이트](azure-stack-mysql-resource-provider-update.md)
