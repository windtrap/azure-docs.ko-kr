---
title: 빠른 시작 - Azure Container Instances에 Docker 컨테이너 배포 - Portal
description: 이 빠른 시작에서는 Azure Portal을 사용하여, 격리된 Azure 컨테이너 인스턴스에서 실행하는 컨테이너화된 웹앱을 신속하게 배포합니다.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: quickstart
ms.date: 03/21/2019
ms.author: danlep
ms.custom: seodec18, mvc
ms.openlocfilehash: f4d232d4d6043ede3979db67e5cd35130d931bef
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58369448"
---
# <a name="quickstart-deploy-a-container-instance-in-azure-using-the-azure-portal"></a>빠른 시작: Azure Portal을 사용하여 Azure에서 컨테이너 인스턴스 배포

Azure Container Instances를 사용하여 Azure에서 서버리스 Docker 컨테이너를 간단하고 빠르게 실행합니다. Azure Kubernetes Service와 같은 풀 컨테이너 오케스트레이션 플랫폼이 필요하지 않을 경우 애플리케이션을 요청 시 컨테이너 인스턴스에 배포합니다.

이 빠른 시작에서는 Azure Portal을 사용하여 격리된 Docker 컨테이너를 배포하고 해당 애플리케이션을 정규화된 도메인 이름(FQDN)으로 사용 가능하게 합니다. 몇 가지 설정을 구성하고 컨테이너를 배포한 후 실행 중인 애플리케이션을 찾아볼 수 있습니다.

![Azure Container Instances에 배포되어 브라우저에 표시된 응용 프로그램][aci-portal-07]

## <a name="sign-in-to-azure"></a>Azure에 로그인

 https://portal.azure.com에서 Azure Portal에 로그인합니다.

Azure 구독이 없는 경우 시작하기 전에 [체험 계정][azure-free-account]을 만듭니다.

## <a name="create-a-container-instance"></a>컨테이너 인스턴스 만들기

**리소스 만들기** > **컨테이너** > **Container Instances**를 선택합니다.

![Azure Portal에서 새 컨테이너 인스턴스를 만들기 시작][aci-portal-01]

**컨테이너 이름**, **컨테이너 이미지** 및 **리소스 그룹** 텍스트 상자에 다음 값을 입력합니다. 다른 값은 기본값으로 두고 **확인**을 선택합니다.

* 컨테이너 이름: `mycontainer`
* 컨테이너 이미지: `mcr.microsoft.com/azuredocs/aci-helloworld`
* 리소스 그룹: **새로 만들기** > `myResourceGroup`

![Azure Portal에서 새 컨테이너 인스턴스의 기본 설정 구성][aci-portal-03]

이 빠른 시작의 경우 **공용**의 기본 설정을 그대로 유지하여 공개 Microsoft `aci-helloworld` 이미지를 배포합니다. 이 이미지는 고정 HTML 페이지를 제공하는 Node.js로 작성된 작은 웹앱을 패키징합니다.

**구성** 아래에서 컨테이너의 **DNS 이름 레이블**을 지정합니다. 이름은 컨테이너 인스턴스를 만드는 Azure 지역 내에서 고유해야 합니다. 컨테이너는 `<dns-name-label>.<region>.azurecontainer.io`에서 공개적으로 연결할 수 있습니다. "DNS 이름 레이블을 사용할 수 없습니다"라는 오류 메시지가 표시되면 다른 DNS 이름 레이블을 사용해 보세요.

**구성**에서 다른 설정은 기본값으로 두고 **확인**을 선택하여 구성의 유효성 검사를 실행합니다.

![Azure Portal에서 새 컨테이너 인스턴스 구성][aci-portal-04]

유효성 검사가 완료되면 컨테이너 설정의 요약 정보가 표시됩니다. **확인**을 선택하여 컨테이너 배포 요청을 제출합니다.

![Azure Portal에서 새 컨테이너 인스턴스의 설정 요약][aci-portal-05]

배포가 시작되면 배포가 진행 중임을 알려주는 알림이 표시됩니다. 컨테이너 그룹이 배포되면 다른 알림이 표시됩니다.

![Azure Portal에서 새 컨테이너 인스턴스를 만들기 진행 상황][aci-portal-08]

**리소스 그룹** > **myResourceGroup** > **mycontainer**로 차례로 이동하여 컨테이너 그룹에 대한 개요를 엽니다. 컨테이너 인스턴스의 **FQDN**(정규화된 도메인 이름) 및 **상태**를 기록해 둡니다.

![Azure Portal의 컨테이너 그룹 개요][aci-portal-06]

**상태**가 *실행 중*이면 브라우저에서 컨테이너의 FQDN으로 이동합니다.

![Azure Container Instances를 사용하여 배포된 앱이 브라우저에 표시됨][aci-portal-07]

축하합니다! 몇 가지 설정을 구성하여 공개적으로 액세스 가능한 애플리케이션을 Azure Container Instances에 배포했습니다.

## <a name="view-container-logs"></a>컨테이너 로그 보기

컨테이너 또는 컨테이너가 실행되는 애플리케이션 문제를 해결할 때 컨테이너 인스턴스의 로그를 살펴보면 도움이 됩니다.

컨테이너의 로그를 보려면 **설정** 아래에서 **컨테이너**, **로그**를 차례로 선택합니다. 브라우저에서 애플리케이션을 살펴볼 때 생성된 HTTP GET 요청이 보일 것입니다.

![Azure Portal의 컨테이너 로그][aci-portal-11]

## <a name="clean-up-resources"></a>리소스 정리

컨테이너 작업을 마친 후에는 *mycontainer* 컨테이너 인스턴스에 대한 **개요**를 선택한 다음, **삭제**를 선택합니다.

![Azure Portal에서 컨테이너 인스턴스 삭제][aci-portal-09]

확인 대화 상자가 나타나면 **예**를 선택합니다.

![Azure Portal에서 컨테이너 인스턴스의 삭제 확인][aci-portal-10]

## <a name="next-steps"></a>다음 단계

이 빠른 시작에서는 공용 Microsoft 이미지에서 Azure 컨테이너 인스턴스를 만들었습니다. 컨테이너 이미지를 빌드하고 개인 Azure 컨테이너 레지스트리에서 배포하려면 Azure Container Instances 자습서로 계속 진행하세요.

> [!div class="nextstepaction"]
> [Azure Container Instances 자습서](./container-instances-tutorial-prepare-app.md)

<!-- IMAGES -->
[aci-portal-01]: ./media/container-instances-quickstart-portal/qs-portal-01.png
[aci-portal-03]: ./media/container-instances-quickstart-portal/qs-portal-03.png
[aci-portal-04]: ./media/container-instances-quickstart-portal/qs-portal-04.png
[aci-portal-05]: ./media/container-instances-quickstart-portal/qs-portal-05.png
[aci-portal-06]: ./media/container-instances-quickstart-portal/qs-portal-06.png
[aci-portal-07]: ./media/container-instances-quickstart-portal/qs-portal-07.png
[aci-portal-08]: ./media/container-instances-quickstart-portal/qs-portal-08.png
[aci-portal-09]: ./media/container-instances-quickstart-portal/qs-portal-09.png
[aci-portal-10]: ./media/container-instances-quickstart-portal/qs-portal-10.png
[aci-portal-11]: ./media/container-instances-quickstart-portal/qs-portal-11.png

<!-- LINKS - External -->
[azure-free-account]: https://azure.microsoft.com/free/