---
title: 외부 트래픽 리디렉션을 포함하는 애플리케이션 게이트웨이 만들기 - Azure CLI | Microsoft Docs
description: Azure CLI를 사용하여 내부 웹 트래픽을 해당 풀로 리디렉션하는 애플리케이션 게이트웨이를 만드는 방법을 알아봅니다.
services: application-gateway
author: vhorne
manager: jpconnock
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/24/2018
ms.author: victorh
ms.openlocfilehash: 1ddbc84004622c2a5fa9dc08d4396e1f300474f2
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/05/2019
ms.locfileid: "55728387"
---
# <a name="create-an-application-gateway-with-external-redirection-using-the-azure-cli"></a>Azure CLI를 사용하여 외부 리디렉션을 포함하는 애플리케이션 게이트웨이 만들기

Azure CLI를 사용하여 [애플리케이션 게이트웨이](overview.md)를 만들 때 [웹 트래픽 리디렉션](multiple-site-overview.md)을 구성할 수 있습니다. 이 자습서에서는 애플리케이션 게이트웨이에 도착하는 웹 트래픽을 외부 사이트에 리디렉션하는 수신기 및 규칙을 구성합니다.

이 문서에서는 다음 방법을 설명합니다.

> [!div class="checklist"]
> * 네트워크 설정
> * 수신기 및 리디렉션 규칙 만들기
> * 애플리케이션 게이트웨이 만들기

Azure 구독이 아직 없는 경우 시작하기 전에 [체험 계정](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)을 만듭니다.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI를 로컬로 설치하여 사용하도록 선택한 경우 이 빠른 시작에서 Azure CLI 버전 2.0.4 이상을 실행해야 합니다. 버전을 확인하려면 `az --version`을 실행합니다. 설치 또는 업그레이드해야 하는 경우 [Azure CLI 설치](/cli/azure/install-azure-cli)를 참조하세요.

## <a name="create-a-resource-group"></a>리소스 그룹 만들기

리소스 그룹은 Azure 리소스가 배포 및 관리되는 논리적 컨테이너입니다. [az group create](/cli/azure/group)를 사용하여 리소스 그룹을 만듭니다.

다음 예제에서는 *eastus* 위치에 *myResourceGroupAG*라는 리소스 그룹을 만듭니다.

```azurecli-interactive 
az group create --name myResourceGroupAG --location eastus
```

## <a name="create-network-resources"></a>네트워크 리소스 만들기 

[az network vnet create](/cli/azure/network/vnet)를 사용하여 *myVNet*이라는 가상 네트워크와 *myAGSubnet*이라는 서브넷을 만듭니다. [az network public-ip create](/cli/azure/network/public-ip)를 사용하여 *myAGPublicIPAddress*라는 IP 주소를 만듭니다. 이러한 리소스는 애플리케이션 게이트웨이 및 연결된 리소스에 대한 네트워크 연결을 제공하는 데 사용됩니다.

```azurecli-interactive
az network vnet create \
  --name myVNet \
  --resource-group myResourceGroupAG \
  --location eastus \
  --address-prefix 10.0.0.0/16 \
  --subnet-name myAGSubnet \
  --subnet-prefix 10.0.1.0/24
az network public-ip create \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress
```

## <a name="create-an-application-gateway"></a>애플리케이션 게이트웨이 만들기

[az network application-gateway create](/cli/azure/network/application-gateway)를 사용하여 *myAppGateway*라는 애플리케이션 게이트웨이를 만들 수 있습니다. Azure CLI를 사용하여 애플리케이션 게이트웨이를 만들 때 용량, sku, HTTP 설정 등의 구성 정보를 지정합니다. 애플리케이션 게이트웨이는 앞에서 만든 *myAGSubnet* 및 *myPublicIPAddress*에 할당됩니다. 

```azurecli-interactive
az network application-gateway create \
  --name myAppGateway \
  --location eastus \
  --resource-group myResourceGroupAG \
  --vnet-name myVNet \
  --subnet myAGsubnet \
  --capacity 2 \
  --sku Standard_Medium \
  --http-settings-cookie-based-affinity Disabled \
  --frontend-port 8080 \
  --http-settings-port 80 \
  --http-settings-protocol Http \
  --public-ip-address myAGPublicIPAddress
```

애플리케이션 게이트웨이가 생성될 때까지 몇 분 정도 걸릴 수 있습니다. 애플리케이션 게이트웨이가 생성되면 다음과 같은 새 기능을 볼 수 있습니다.

- *appGatewayBackendPool* - 애플리케이션 게이트웨이에 백 엔드 주소 풀이 하나 이상 있어야 합니다.
- *appGatewayBackendHttpSettings* - 포트 80 및 HTTP 프로토콜을 통신에 사용하도록 지정합니다.
- *appGatewayHttpListener* - *appGatewayBackendPool*에 연결되는 기본 수신기입니다.
- *appGatewayFrontendIP* - *myAGPublicIPAddress*를 *appGatewayHttpListener*에 할당합니다.
- *rule1* - *appGatewayHttpListener*에 연결되는 기본 라우팅 규칙입니다.

### <a name="add-the-redirection-configuration"></a>리디렉션 구성 추가

[az network application-gateway redirect-config create](/cli/azure/network/application-gateway/redirect-config)를 사용하여 *www.consoto.org*에서 애플리케이션 게이트웨이의 *www.contoso.com* 수신기로 트래픽을 전송하는 리디렉션 구성을 추가합니다.

```azurecli-interactive
az network application-gateway redirect-config create \
  --name myredirect \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --type Temporary \
  --target-url "https://bing.com"
```

### <a name="add-a-listener-and-routing-rule"></a>수신기 및 라우팅 규칙 추가

애플리케이션 게이트웨이에서 트래픽을 적절히 라우팅하려면 수신기가 필요합니다. [az network application-gateway frontend-port create](/cli/azure/network/application-gateway)를 사용하여 만든 프런트 엔드 포트와 함께 [az network application-gateway http-listener create](/cli/azure/network/application-gateway)를 사용하여 수신기를 만듭니다. 수신기가 들어오는 트래픽을 보낼 위치를 알려면 규칙이 필요합니다. [az network application-gateway rule create](/cli/azure/network/application-gateway)를 사용하여 *redirectRule*이라는 기본 규칙을 만듭니다.

```azurecli-interactive
az network application-gateway frontend-port create \
  --port 80 \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --name redirectPort
az network application-gateway http-listener create \
  --name redirectListener \
  --frontend-ip appGatewayFrontendIP \
  --frontend-port redirectPort \
  --resource-group myResourceGroupAG \
  --gateway-name myAppGateway
az network application-gateway rule create \
  --gateway-name myAppGateway \
  --name redirectRule \
  --resource-group myResourceGroupAG \
  --http-listener redirectListener \
  --rule-type Basic \
  --redirect-config myredirect
```

## <a name="test-the-application-gateway"></a>애플리케이션 게이트웨이 테스트

애플리케이션 게이트웨이의 공용 IP 주소를 가져오려면 [az network public-ip show](/cli/azure/network/public-ip)를 사용합니다. 공용 IP 주소를 복사하여 브라우저의 주소 표시줄에 붙여넣습니다.

브라우저에 *bing.com*이 표시되어야 합니다.

## <a name="next-steps"></a>다음 단계

이 자습서에서는 다음 방법에 대해 알아보았습니다.

> * 네트워크 설정
> * 수신기 및 리디렉션 규칙 만들기
> * 애플리케이션 게이트웨이 만들기
