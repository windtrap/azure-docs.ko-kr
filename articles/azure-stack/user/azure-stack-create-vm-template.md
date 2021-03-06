---
title: 이 자습서에서는 Azure Stack VM 템플릿을 사용 하 여 만든 | Microsoft Docs
description: ASDK를 사용 하 여 미리 정의 된 템플릿과 GitHub 사용자 지정 템플릿을 사용 하 여 VM을 만드는 방법을 설명 합니다.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 02/21/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 11/13/2018
ms.openlocfilehash: a663a5c45c542ac4bfa19266c73066f8e41ba5d8
ms.sourcegitcommit: a4efc1d7fc4793bbff43b30ebb4275cd5c8fec77
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/21/2019
ms.locfileid: "56648883"
---
# <a name="tutorial-create-a-vm-using-a-community-template"></a>자습서: 커뮤니티 템플릿을 사용 하 여 VM 만들기

Azure Stack 연산자 또는 사용자를 만들 수 있습니다 사용 하 여 가상 머신 (VM) [사용자 지정 GitHub 빠른 시작 템플릿](https://github.com/Azure/AzureStack-QuickStart-Templates) Azure Stack marketplace에서 직접 템플릿을 배포 하는 대신 합니다.

이 자습서에서는 다음 방법에 대해 알아봅니다.

> [!div class="checklist"]
> * Azure Stack 빠른 시작 템플릿을 사용 하 여
> * 사용자 지정 GitHub 템플릿을 사용 하 여 VM 만들기
> * Minikube를 시작 하 고 응용 프로그램 설치

## <a name="azure-stack-quickstart-templates"></a>Azure Stack 빠른 시작 템플릿

Azure Stack 빠른 시작 템플릿 GitHub에 저장 됩니다 [공용 Azure Stack 빠른 시작 템플릿 리포지토리](https://github.com/Azure/AzureStack-QuickStart-Templates)합니다. 이 리포지토리는 Microsoft Azure Stack 개발 키트 (ASDK) 사용 하 여 테스트 된 Azure Resource Manager 배포 템플릿을 포함 합니다. Azure Stack을 평가 하 고 ASDK 환경을 사용 하 여 쉽게 수행할 수 있도록 사용할 수 있습니다.

시간이 지남에 따라 GitHub 사용자 수에 기여 했을 리포지토리 400 개가 넘는 배포 템플릿의 컬렉션을 생성 합니다. 이 리포지토리는 Azure Stack에 다양 한 환경 배포 하는 방법을 더 나은 이해를 시작 점으로 적당 합니다.

>[!IMPORTANT]
> 이러한 템플릿 중 일부에 Microsoft가 아니라 커뮤니티 구성원에서 생성 됩니다. 각 템플릿에 사용권 계약에 따라 Microsoft가 아닌 해당 소유자에 의해 사용이 허가 됩니다. Microsoft는 이러한 템플릿에 대해 책임이 없으며 및 보안, 호환성 또는 성능을 검사 차단 하지 않습니다. 커뮤니티 템플릿 Microsoft 지원 프로그램 또는 서비스에서 지원 되지 않습니다 및 "그대로", 모든 종류의 보증도 없이 제공 됩니다.

Azure Resource Manager 템플릿 GitHub에 참여 하려는 경우 작성 글을 확인 합니다 [AzureStack-빠른 시작-템플릿을](https://github.com/Azure/AzureStack-QuickStart-Templates) 리포지토리. 이 리포지토리와에 기여 하는 방법에 대 한 자세한 내용은 참조는 [readme 파일](https://github.com/Azure/AzureStack-QuickStart-Templates/blob/master/README.md)합니다.

## <a name="create-a-vm-using-a-custom-github-template"></a>사용자 지정 GitHub 템플릿을 사용 하 여 VM 만들기

이 예제에서는 자습서는 [101-vm-linux-minikube](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-vm-linux-minikube) Minikube Kubernetes 클러스터를 관리 하려면를 실행 하는 Azure Stack에 Ubuntu 16.04 가상 컴퓨터를 배포 하려면 Azure Stack 빠른 시작 템플릿이 사용 됩니다.

Minikube는 쉽게 Kubernetes를 로컬로 실행할 수 있는 도구입니다. Minikube 실행 VM 내에서 단일 노드 Kubernetes 클러스터를 Kubernetes 사용해 또는를 사용 하 여 개발할 수 있도록 일상적인 합니다. Linux VM에서 실행 하는 간단 하 고 1 개 노드 Kubernetes 클러스터를 지원 합니다. Minikube는 실행 하는 모든 기능을 갖춘 Kubernetes 클러스터를 가져오려는 가장 빠르고 가장 간단한 방법입니다. 개발자가 개발 하 고 해당 로컬 컴퓨터에서 Kubernetes 기반 응용 프로그램 배포를 테스트할 수 있습니다. 아키텍처 측면에서 Minikube VM 마스터 및 에이전트 노드 구성 요소를 모두 로컬로 실행 합니다.

* 스케줄러 API 서버와 같은 마스터 노드 구성 요소 및 [Server etcd](https://coreos.com/etcd/) 이라는 단일 Linux 프로세스에서 실행 되 **LocalKube**합니다.
* 에이전트 노드 구성 요소는 일반 에이전트 노드에서 실행 됩니다 대로 정확 하 게 docker 컨테이너 내에서 실행 됩니다. 응용 프로그램 배포 관점에서 차이가 없습니다 Minikube, 또는 일반 Kubernetes 클러스터에서 응용 프로그램을 배포할 때.

이 템플릿은 다음 구성 요소를 설치 합니다.

* Ubuntu 16.04 LTS VM
* [Docker-CE](https://download.docker.com/linux/ubuntu)
* [Kubectl](https://storage.googleapis.com/kubernetes-release/release/v1.8.0/bin/linux/amd64/kubectl)
* [Minikube](https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64)
* xFCE4
* xRDP

> [!IMPORTANT]
> Ubuntu VM 이미지 (Ubuntu Server 16.04 LTS를이 예제의) 해야 이미 추가한 Azure Stack marketplace에 이러한 단계를 수행 하기 전에 합니다.

1. 선택 **+ 리소스 만들기**, 한 다음 **사용자 지정**, 한 다음 **템플릿 배포**합니다.

    ![템플릿 만들기](media/azure-stack-create-vm-template/1.PNG)

2. 선택 **템플릿 편집**합니다.

    ![템플릿 편집](media/azure-stack-create-vm-template/2.PNG)

3. 선택 **빠른 시작 템플릿**합니다.

    ![빠른 시작 템플릿](media/azure-stack-create-vm-template/3.PNG)

4. 선택 **101-vm-linux-minikube** 를 사용 하 여 사용 가능한 템플릿에서 합니다 **템플릿을 선택** 드롭다운 목록 및 클릭 **확인**합니다.

    ![템플릿 선택](media/azure-stack-create-vm-template/4.PNG)

5. 템플릿 JSON 수정 하려는 경우이 수행할 수 있습니다. 그렇지 않은 경우 또는 완료 되 면 선택 **저장** 닫으려면 합니다 **템플릿 편집** 대화 합니다.

    ![템플릿 저장](media/azure-stack-create-vm-template/5.PNG)

6. 선택 **매개 변수**를 입력 하거나 필요에 따라 사용 가능한 필드를 수정 하 고 클릭 **확인**합니다. 구독 사용, 만들기 또는 기존 리소스 그룹 이름을 선택 및 클릭을 선택 **만들기** 템플릿 배포를 시작 합니다.

    ![매개 변수](media/azure-stack-create-vm-template/6.PNG)

7. 구독 사용, 만들기 또는 기존 리소스 그룹 이름을 선택 및 클릭을 선택 **만들기** 템플릿 배포를 시작 합니다.

    ![구독 선택](media/azure-stack-create-vm-template/7.PNG)

8. 리소스 그룹 배포를 사용자 지정 템플릿 기반 VM을 만드는 데 몇 분 정도 걸립니다. 알림을 통해 및 리소스 그룹 속성에서 설치 상태를 모니터링할 수 있습니다.

    ![배포](media/azure-stack-create-vm-template/8.PNG)

    >[!NOTE]
    > VM은 배포가 완료 되 면 실행 됩니다.

## <a name="start-minikube-and-install-an-application"></a>Minikube를 시작 하 고 응용 프로그램 설치

Linux VM에 성공적으로 생성 된 했으므로 Minikube를 시작 하 고 응용 프로그램 설치 서명할 수 있습니다.

1. 배포가 완료 되 면 선택 **Connect** Linux VM에 연결 하는 데 수 있는 공용 IP 주소를 볼 수 있습니다.

    ![연결](media/azure-stack-create-vm-template/9.PNG)

2. 관리자 권한 명령 프롬프트에서 실행할 **mstsc.exe** 를 원격 데스크톱 연결을 열고 이전 단계에서 검색 된 Linux VM 공용 IP 주소에 연결 합니다. XRDP에 로그인 하는 메시지가 표시 되 면 VM을 만들 때 지정한 자격 증명을 사용 합니다.

    ![원격](media/azure-stack-create-vm-template/10.PNG)

3. 터미널 에뮬레이터 열고 Minikube를 시작 하려면 다음 명령을 입력 합니다.

    ```shell
    sudo minikube start --vm-driver=none
    sudo minikube addons enable dashboard
    sudo minikube dashboard --url
    ```

    ![실행 명령](media/azure-stack-create-vm-template/11.PNG)

4. 웹 브라우저를 열고 Kubernetes 대시보드 주소를 방문 합니다. 축, 이제 완벽 하 게 작동 Minikube를 사용 하 여 Kubernetes 설치!

    ![대시보드](media/azure-stack-create-vm-template/12.PNG)

5. 샘플 응용 프로그램을 배포 하려면 공식 Kubernetes 설명서 페이지를 방문 하 고 이전 단계에서 만든 이미 "Minikube 클러스터 만들기" 섹션을 건너뜁니다. 섹션으로 이동할 "Node.js 응용 프로그램 만들기"에서 https://kubernetes.io/docs/tutorials/stateless-application/hello-minikube/합니다.

## <a name="next-steps"></a>다음 단계

이 자습서에서는 다음 방법에 대해 알아보았습니다.

> [!div class="checklist"]
> * Azure Stack 빠른 시작 템플릿에 대해 알아보기
> * 사용자 지정 GitHub 템플릿을 사용 하 여 VM 만들기
> * Minikube를 시작 하 고 응용 프로그램 설치
