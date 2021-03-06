---
title: 그룹 또는 액세스 검토-Azure Active Directory의에서 응용 프로그램에 대 한 액세스 검토 결과 검색 합니다. | Microsoft Docs
description: 그룹 구성원 또는 Azure Active Directory 액세스 검토에 대 한 응용 프로그램 액세스에 대 한 액세스 검토 결과 검색 하는 방법에 알아봅니다.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 06/21/2018
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: eae4bafb3eefcee2785c784030d7be8dde66988e
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2019
ms.locfileid: "58578715"
---
# <a name="retrieve-access-review-results-for-groups-or-applications-in-azure-ad-access-reviews"></a>액세스 검토를 검색 결과 그룹 또는 Azure AD에서 응용 프로그램에 대 한 액세스 검토

관리자는 Azure AD(Azure Active Directory)를 사용하여 애플리케이션에 할당된 그룹 구성원 또는 사용자에 대한 [액세스 검토를 만들 수](create-access-review.md) 있습니다.  에 있는 사용자를 **전역 관리자**, **사용자 관리자**, **보안 관리자** 하거나 **보안 읽기 권한자** 역할 수도 있습니다 액세스 검토의 결과 읽습니다.  에 이러한 역할 중 하나에 사용자를 할당 하려면 권한 있는 역할 관리자 사용자 역할을 활성화할 수 있도록 Azure AD PIM를 사용 하거나 전역 관리자 수 영구적 [역할에 사용자 할당](../fundamentals/active-directory-users-assign-role-azure-portal.md)합니다.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="locating-an-access-review"></a>액세스 검토 찾기

어떤 프로그램이 액세스 검토를 포함하고 있는지 아는 경우 액세스 검토 페이지로 이동하고, **프로그램**을 선택하고 액세스 검토 컨트롤이 포함된 프로그램을 선택합니다.  그런 다음, **컨트롤**을 선택하고 액세스 검토 컨트롤을 선택합니다. 프로그램에 많은 컨트롤이 있는 경우 특정 유형의 컨트롤에 대해 필터링하고 해당 상태별로 정렬할 수 있습니다. 또한 액세스 검토 컨트롤의 이름이나 만든 소유자의 표시 이름으로 검색할 수도 있습니다. 

어떤 프로그램이 액세스 검토를 포함하는지 모르는 경우 액세스 검토 페이지로 이동하고 **컨트롤**을 선택합니다.  특정 유형의 컨트롤에 대해 필터링하고 상태별로 정렬하고, 액세스 검토 컨트롤의 이름 또는 해당 컨트롤을 만든 소유자의 표시 이름을 기준으로 검색할 수 있습니다. 

## <a name="retrieving-the-results-for-a-one-time-access-review"></a>일회성 액세스 검토에 대한 결과 검색

검토 되풀이 형식이 일회성인 경우 활성 액세스 검토 진행률 및 완료된 액세스 검토 결과는 **결과** 섹션에서 얻을 수 있습니다.  해당 사용자의 액세스를 보기 위해 검토 중인 액세스 권한이 있는 사용자의 사용자 계정 이름이나 표시 이름을 입력할 수 있습니다.  완료된 액세스 검토의 모든 결과 검색하려면 **다운로드** 단추를 클릭합니다.

## <a name="retrieving-the-results-for-multiple-instances-of-a-recurring-access-review"></a>되풀이 액세스 검토의 여러 인스턴스에 대한 결과 검색

되풀이되는 활성 액세스 검토 진행률을 보려면 **결과** 섹션을 클릭합니다.  검토 중인 액세스 권한이 있는 사용자의 사용자 계정 이름이나 표시 이름을 입력할 수 있습니다.

되풀이되는 액세스 검토의 완료된 인스턴스의 결과를 보려면 **기록 검토**을 선택한 다음, 인스턴스의 시작 및 종료 날짜를 기준으로 완료된 액세스 검토 인스턴스 목록에서 특정 인스턴스를 선택합니다.   이 인스턴스의 결과는 **결과** 섹션에서 얻을 수 있습니다.  해당 사용자의 액세스를 보기 위해 검토 중인 액세스 권한이 있는 사용자의 사용자 계정 이름이나 표시 이름을 입력할 수 있습니다.  되풀이 액세스 검토의 완료된 인스턴스의 모든 결과 검색하려면 **다운로드** 단추를 클릭합니다.


## <a name="removing-users-from-an-access-review"></a>액세스 검토에서 사용자 제거

기본적으로 삭제된 사용자는 30일 동안 Azure AD에서 삭제된 채로 남아 있지만 이 기간 동안 관리자는 필요한 경우 삭제된 사용자를 복원할 수 있습니다.  30일 후 해당 사용자가 영구적으로 삭제됩니다.  또한 Azure Active Directory 포털을 사용하여 전역 관리자는 해당 기간이 만료되기 전에 명시적으로 [최근에 삭제된 사용자를 영구적으로 삭제](../fundamentals/active-directory-users-restore.md)할 수 있습니다.  사용자가 영구적으로 삭제됐으므로 추후 해당 사용자에 대한 데이터가 활성 액세스 검토에서 제거됩니다.  삭제된 사용자에 대한 감사 정보는 감사 로그에 남아 있습니다.

## <a name="next-steps"></a>다음 단계

- [Azure AD 액세스 검토를 사용하여 사용자 액세스 관리](manage-user-access-with-access-reviews.md)
- [Azure AD 액세스 검토를 사용하여 게스트 액세스 관리](manage-guest-access-with-access-reviews.md)
- [Azure AD 액세스 검토용 프로그램 및 컨트롤 관리](manage-programs-controls.md)
- [그룹 또는 응용 프로그램의 액세스 검토 만들기](create-access-review.md)
- [Azure AD 관리 역할에서 사용자 액세스 검토 만들기](../privileged-identity-management/pim-how-to-start-security-review.md)


