---
title: '자습서: LockPath Keylight와 Azure Active Directory 통합 | Microsoft Docs'
description: Azure Active Directory와 LockPath Keylight 간에 Single Sign-On을 구성하는 방법에 대해 알아봅니다.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 234a32f1-9f56-4650-9e31-7b38ad734b1a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4bc5121f6604fae9a28b52db1bfb308d7cdb968d
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56205089"
---
# <a name="tutorial-azure-active-directory-integration-with-lockpath-keylight"></a>자습서: Azure Active Directory와 LockPath Keylight 통합

이 자습서에서는 Azure AD(Azure Active Directory)와 LockPath Keylight를 통합하는 방법에 대해 알아봅니다.

LockPath Keylight를 Azure AD와 통합하면 다음과 같은 이점이 있습니다.

- LockPath Keylight에 대한 액세스 권한이 있는 사용자를 Azure AD에서 제어할 수 있습니다.
- 사용자가 해당 Azure AD 계정으로 LockPath Keylight에 자동으로 로그온(Single Sign-on)되도록 설정할 수 있습니다.
- 단일 중앙 위치인 Azure Portal에서 계정을 관리할 수 있습니다.

Azure AD와 SaaS 앱 통합에 대한 자세한 내용은 [Azure Active Directory의 애플리케이션 액세스 및 Single Sign-On이란 무엇인가요?](../manage-apps/what-is-single-sign-on.md)를 참조하세요.

## <a name="prerequisites"></a>필수 조건

LockPath Keylight와 Azure AD 통합을 구성하려면 다음 항목이 필요합니다.

- Azure AD 구독
- LockPath Keylight Single Sign-On이 설정된 구독

> [!NOTE]
> 이 자습서의 단계를 테스트하기 위해 프로덕션 환경을 사용하는 것은 바람직하지 않습니다.

이 자습서의 단계를 테스트하려면 다음 권장 사항을 준수해야 합니다.

- 꼭 필요한 경우가 아니면 프로덕션 환경을 사용하지 마세요.
- Azure AD 평가판 환경이 없으면 [여기](https://azure.microsoft.com/pricing/free-trial/)에서 1개월 평가판을 얻을 수 있습니다.

## <a name="scenario-description"></a>시나리오 설명
이 자습서에서는 테스트 환경에서 Azure AD Single Sign-On을 테스트 합니다.  이 자습서에 설명된 시나리오는 다음 두 가지 주요 구성 요소로 이루어져 있습니다.

1. 갤러리에서 LockPath Keylight 추가
1. Azure AD Single Sign-on 구성 및 테스트

## <a name="adding-lockpath-keylight-from-the-gallery"></a>갤러리에서 LockPath Keylight 추가
LockPath Keylight가 Azure AD에 통합되도록 구성하려면 갤러리에서 LockPath Keylight를 관리되는 SaaS 앱 목록에 추가해야 합니다.

**갤러리에서 LockPath Keylight를 추가하려면 다음 단계를 수행합니다.**

1. **[Azure Portal](https://portal.azure.com)** 의 왼쪽 탐색 창에서 **Azure Active Directory** 아이콘을 클릭합니다. 

    ![Active Directory][1]

1. **엔터프라이즈 응용 프로그램**으로 이동합니다. 그런 후 **모든 애플리케이션**으로 이동합니다.

    ![애플리케이션][2]
    
1. 새 애플리케이션을 추가하려면 대화 상자 맨 위 있는 **새 애플리케이션** 단추를 클릭합니다.

    ![애플리케이션][3]

1. 검색 상자에 **LockPath Keylight**를 입력합니다.

    ![Azure AD 테스트 사용자 만들기](./media/keylight-tutorial/tutorial_keylight_search.png)

1. 결과 창에서 **LockPath Keylight**를 선택하고 **추가** 단추를 클릭하여 애플리케이션을 추가합니다.

    ![Azure AD 테스트 사용자 만들기](./media/keylight-tutorial/tutorial_keylight_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Azure AD Single Sign-on 구성 및 테스트
이 섹션에서는 "Britta Simon"이라는 테스트 사용자를 기반으로 LockPath Keylight에서 Azure AD Single Sign-On을 구성하고 테스트합니다.

Single Sign-On이 작동하려면 Azure AD 사용자에 해당하는 LockPath Keylight 사용자가 누구인지 Azure AD에서 알고 있어야 합니다. 즉, Azure AD 사용자와 LockPath Keylight의 관련 사용자 간에 링크 관계가 설정되어야 합니다.

LockPath Keylight에서 Azure AD의 **사용자 이름** 값을 **Username** 값으로 할당하여 링크 관계를 설정합니다.

LockPath Keylight에서 Azure AD Single Sign-On을 구성하고 테스트하려면 다음 구성 요소를 완료해야 합니다.

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - 사용자가 이 기능을 사용할 수 있도록 합니다.
1. **[Azure AD 테스트 사용자 만들기](#creating-an-azure-ad-test-user)** - Britta Simon으로 Azure AD Single Sign-On 테스트하는 데 사용합니다.
1. **[LockPath Keylight 테스트 사용자 만들기](#creating-a-lockpath-keylight-test-user)** - Britta Simon의 Azure AD 표현과 연결된 해당 사용자를 LockPath Keylight에 만듭니다.
1. **[Azure AD 테스트 사용자 할당](#assigning-the-azure-ad-test-user)** - Britta Simon이 Azure AD Single Sign-on을 사용할 수 있도록 합니다.
1. **[Single Sign-On 테스트](#testing-single-sign-on)** - 구성이 작동하는지 확인합니다.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD Single Sign-On 구성

이 섹션에서는 Azure Portal에서 Azure AD Single Sign-On을 사용하도록 설정하고 LockPath Keylight 애플리케이션에서 Single Sign-On을 구성합니다.

**LockPath Keylight에서 Azure AD Single Sign-on을 구성하려면 다음 단계를 수행합니다.**

1. Azure Portal의 **LockPath Keylight** 애플리케이션 통합 페이지에서 **Single Sign-On**을 클릭합니다.

    ![Configure Single Sign-On][4]

1. **Single Sign-On** 대화 상자에서 **모드**를 **SAML 기반 로그온**으로 선택하여 Single Sign-On을 사용하도록 설정합니다.
 
    ![Configure Single Sign-On](./media/keylight-tutorial/tutorial_keylight_samlbase.png)

1. **LockPath Keylight 도메인 및 URL** 섹션에서 다음 단계를 수행합니다.

    ![Configure Single Sign-On](./media/keylight-tutorial/tutorial_keylight_url.png)

    a. **로그온 URL** 텍스트 상자에서 다음 패턴으로 URL을 입력합니다. `https://<company name>.keylightgrc.com/`

    b. **식별자** 텍스트 상자에서 `https://<company name>.keylightgrc.com` 패턴을 사용하여 URL을 입력합니다.

    다. **회신 URL** 텍스트 상자에 다음 패턴으로 URL을 입력합니다.`https://<company name>.keylightgrc.com/Login.aspx`
    
    > [!NOTE] 
    > 이러한 값은 실제 값이 아닙니다. 이러한 값을 실제 식별자, 회신 URL 및 로그온 URL로 업데이트합니다. 이러한 값을 얻으려면 [LockPath Keylight 클라이언트 지원 팀](https://www.lockpath.com/contact/)에 문의하세요. 

1. **SAML 서명 인증서** 섹션에서 **인증서(원시)** 를 클릭한 후 컴퓨터에 인증서 파일을 저장합니다.

    ![Configure Single Sign-On](./media/keylight-tutorial/tutorial_keylight_certificate.png) 

1. **저장** 단추를 클릭합니다.

    ![Configure Single Sign-On](./media/keylight-tutorial/tutorial_general_400.png)
    
1. **LockPath Keylight 구성** 섹션에서 **LockPath Keylight 구성**을 클릭하여 **로그온 구성** 창을 엽니다. **빠른 참조 섹션**에서 **로그아웃 URL 및 SAML Single Sign-On 서비스 URL**을 복사합니다.

    ![Configure Single Sign-On](./media/keylight-tutorial/tutorial_keylight_configure.png) 

1. LockPath Keylight에서 SSO를 사용하려면 다음 단계를 수행합니다.
   
    a. LockPath Keylight 계정에 관리자 권한으로 로그온합니다.
    
    b. 위쪽 메뉴에서 **사람**을 클릭하고 **Keylight 설치**를 선택합니다.
   
    ![Configure Single Sign-On](./media/keylight-tutorial/401.png) 

    다. 왼쪽의 트리 뷰에서 **SAML**을 클릭합니다.
   
    ![Configure Single Sign-On](./media/keylight-tutorial/402.png) 

    d. **SAML 설정** 대화 상자에서 **편집**을 클릭합니다.
   
    ![Configure Single Sign-On](./media/keylight-tutorial/404.png) 

1. **SAML 설정 편집** 대화 상자 페이지에서 다음 단계를 수행합니다.
   
    ![Configure Single Sign-On](./media/keylight-tutorial/405.png) 
   
    a. **SAML 인증**을 **활성**으로 설정합니다.

    b. Azure Portal에서 복사한 **SAML Single Sign-On 서비스 URL** 값을 **ID 공급자 로그인 URL** 텍스트 상자에 붙여넣습니다.

    다. Azure Portal에서 복사한 **Single Sign-Out 서비스 URL** 값을 **ID 공급자 로그아웃 URL** 텍스트 상자에 붙여넣습니다.

    d. **파일 선택**을 클릭하여 다운로드한 LockPath Keylight 인증서를 선택하고 **열기**를 클릭하여 인증서를 업로드합니다.

    e. **SAML 사용자 ID 위치**를 **Subject 문의 NameIdentifier 요소**로 설정합니다.
    
    f. **https://&lt;CompanyName&gt;.keylightgrc.com** 패턴을 사용하여 **Keylight 서비스 공급자**를 입력합니다.
    
    g. **사용자 자동 프로비전**을 **활성**으로 설정합니다.

    h. **자동 프로비전 계정 유형**을 **전체 사용자**로 설정합니다.

    i. **Auto-provision security role**(자동 프로비전 보안 역할)에 **Standard User with SAML**(SAML을 사용하는 표준 사용자)를 선택합니다.
    
    j. **Auto-provision security config**(자동 프로비전 보안 구성)에 **Standard User Configuration**(표준 사용자 구성)을 선택합니다.
     
    k. **Email attribute**(이메일 특성) 텍스트 상자에 `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`를 입력합니다.
    
    l. **First name attribute**(이름 특성) 텍스트 상자에 `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`을 입력합니다.
    
    m. **Last name attribute**(성 특성) 텍스트 상자에 `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`을 입력합니다.
    
    n. **저장**을 클릭합니다.

> [!TIP]
> 이제 앱을 설정하는 동안 [Azure Portal ](https://portal.azure.com) 내에서 이러한 지침의 간결한 버전을 읽을 수 있습니다.  **Active Directory &gt; 엔터프라이즈 애플리케이션** 섹션에서 이 앱을 추가한 후에는 **Single Sign-On** 탭을 클릭하고 맨 아래에 있는 **구성** 섹션을 통해 포함된 설명서에 액세스하면 됩니다. 포함된 설명서 기능에 대한 자세한 내용은 [Azure AD 포함된 설명서]( https://go.microsoft.com/fwlink/?linkid=845985)에서 확인할 수 있습니다.

### <a name="creating-an-azure-ad-test-user"></a>Azure AD 테스트 사용자 만들기
이 섹션의 목적은 Azure Portal에서 Britta Simon이라는 테스트 사용자를 만드는 것입니다.

![Azure AD 사용자 만들기][100]

**Azure AD에서 테스트 사용자를 만들려면 다음 단계를 수행하세요.**

1. **Azure Portal**의 왼쪽 탐색 창에서 **Azure Active Directory** 아이콘을 클릭합니다.

    ![Azure AD 테스트 사용자 만들기](./media/keylight-tutorial/create_aaduser_01.png) 

1. 사용자 목록을 표시하려면 **사용자 및 그룹**으로 이동한 후 **모든 사용자**를 클릭합니다.
    
    ![Azure AD 테스트 사용자 만들기](./media/keylight-tutorial/create_aaduser_02.png) 

1. **사용자** 대화 상자를 열려면 대화 상자 위쪽에서 **추가**를 클릭합니다.
 
    ![Azure AD 테스트 사용자 만들기](./media/keylight-tutorial/create_aaduser_03.png) 

1. **사용자** 대화 상자 페이지에서 다음 단계를 수행합니다.
 
    ![Azure AD 테스트 사용자 만들기](./media/keylight-tutorial/create_aaduser_04.png) 

    a. **이름** 텍스트 상자에 **BrittaSimon**을 입력합니다.

    b. **사용자 이름** 텍스트 상자에 BrittaSimon의 **전자 메일 주소**를 입력합니다.

    다. **암호 표시**를 선택하고 **암호** 값을 적어둡니다.

    d. **만들기**를 클릭합니다.
 
### <a name="creating-a-lockpath-keylight-test-user"></a>LockPath Keylight 테스트 사용자 만들기

이 섹션에서는 LockPath Keylight에서 Britta Simon이라는 사용자를 만듭니다. LockPath Keylight는 기본적으로 사용하도록 설정된 Just-In-Time 프로비전을 지원합니다.

이 섹션에 작업 항목이 없습니다. 사용자가 아직 존재하지 않는 경우 LockPath Keylight에 액세스할 때 새 사용자가 만들어집니다. 

>[!NOTE]
>사용자를 수동으로 생성해야 하는 경우 [LockPath Keylight 클라이언트 지원 팀](https://www.lockpath.com/contact/)에 문의해야 합니다. 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD 테스트 사용자 할당

이 섹션에서는 Britta Simon이 Azure Single Sign-On을 사용할 수 있도록 LockPath Keylight에 대한 액세스 권한을 부여합니다.

![사용자 할당][200] 

**Britta Simon을 LockPath Keylight에 할당하려면 다음 단계를 수행합니다.**

1. Azure Portal에서 애플리케이션 보기를 연 다음 디렉터리 보기로 이동하고 **엔터프라이즈 애플리케이션**으로 이동한 후 **모든 애플리케이션**을 클릭합니다.

    ![사용자 할당][201] 

1. 애플리케이션 목록에서 **LockPath Keylight**를 선택합니다.

    ![Configure Single Sign-On](./media/keylight-tutorial/tutorial_keylight_app.png) 

1. 왼쪽 메뉴에서 **사용자 및 그룹**을 클릭합니다.

    ![사용자 할당][202] 

1. **추가** 단추를 클릭합니다. 그런 후 **할당 추가** 대화 상자에서 **사용자 및 그룹**을 선택합니다.

    ![사용자 할당][203]

1. **사용자 및 그룹** 대화 상자의 사용자 목록에서 **Britta Simon**을 선택합니다.

1. **사용자 및 그룹** 대화 상자에서 **선택** 단추를 클릭합니다.

1. **할당 추가** 대화 상자에서 **할당** 단추를 클릭합니다.
    
### <a name="testing-single-sign-on"></a>Single Sign-On 테스트

이 섹션에서는 액세스 패널을 사용하여 Azure AD Single Sign-On 구성을 테스트합니다.

액세스 패널에서 LockPath Keylight 타일을 클릭하면 LockPath Keylight 애플리케이션에 자동으로 로그온됩니다. 

## <a name="additional-resources"></a>추가 리소스

* [Azure Active Directory와 SaaS Apps를 통합하는 방법에 대한 자습서 목록](tutorial-list.md)
* [Azure Active Directory로 애플리케이션 액세스 및 Single Sign-On을 구현하는 방법](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/keylight-tutorial/tutorial_general_01.png
[2]: ./media/keylight-tutorial/tutorial_general_02.png
[3]: ./media/keylight-tutorial/tutorial_general_03.png
[4]: ./media/keylight-tutorial/tutorial_general_04.png

[100]: ./media/keylight-tutorial/tutorial_general_100.png

[200]: ./media/keylight-tutorial/tutorial_general_200.png
[201]: ./media/keylight-tutorial/tutorial_general_201.png
[202]: ./media/keylight-tutorial/tutorial_general_202.png
[203]: ./media/keylight-tutorial/tutorial_general_203.png

