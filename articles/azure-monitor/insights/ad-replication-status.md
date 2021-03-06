---
title: Azure Monitor를 사용하여 Active Directory 복제 상태 모니터링 | Microsoft Docs
description: Active Directory 복제 상태 솔루션 팩은 Active Directory 환경에서 복제 실패가 있는지를 정기적으로 모니터링합니다.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: 1b988972-8e01-4f83-a7f4-87f62778f91d
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 01/24/2018
ms.author: magoedte
ms.openlocfilehash: f7bbde98c6ef35021cc03b2646193d3601ca1cff
ms.sourcegitcommit: ef20235daa0eb98a468576899b590c0bc1a38394
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59425851"
---
# <a name="monitor-active-directory-replication-status-with-azure-monitor"></a>Azure Monitor를 사용하여 Active Directory 복제 상태 모니터링

![AD 복제 상태 기호](./media/ad-replication-status/ad-replication-status-symbol.png)

Active Directory는 엔터프라이즈 IT 환경의 핵심 구성 요소입니다. 고가용성 및 고성능을 보장하기 위해 각 도메인 컨트롤러에 Active Directory 데이터베이스의 자체 복사본이 있습니다. 도메인 컨트롤러는 변경 내용을 엔터프라이즈 전체에 전파하기 위해 서로 복제합니다. 이 복제 프로세스의 오류는 엔터프라이즈에서 다양한 문제를 발생시킬 수 있습니다.

AD 복제 상태 솔루션 팩은 Active Directory 환경에서 복제 실패가 있는지를 정기적으로 모니터링합니다.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand-solution.md)]

## <a name="installing-and-configuring-the-solution"></a>솔루션 설치 및 구성
다음 정보를 사용하여 솔루션을 설치하고 구성합니다.

### <a name="install-agents-on-domain-controllers"></a>도메인 컨트롤러에 에이전트 설치
평가할 도메인의 구성원인 도메인 컨트롤러에 에이전트를 설치해야 합니다. 또는 구성원 서버에 에이전트를 설치하고 Azure Monitor에 AD 복제 데이터를 보내도록 에이전트를 구성해야 합니다. Windows 컴퓨터를 Azure Monitor에 연결하는 방법을 파악하려면 [Azure Monitor에 Windows 컴퓨터 연결](../../azure-monitor/platform/agent-windows.md)을 참조하세요. 도메인 컨트롤러가 Azure Monitor에 연결하려는 기존 System Center Operations Manager 환경에 이미 속해 있는 경우 [Azure Monitor에 Operations Manager 연결](../../azure-monitor/platform/om-agents.md)을 참조하세요.

### <a name="enable-non-domain-controller"></a>비도메인 컨트롤러 사용
Azure Monitor에 도메인 컨트롤러를 직접 연결하지 않으려는 경우 Azure Monitor에 연결된 도메인의 다른 컴퓨터를 사용하여 AD 복제 상태 솔루션 팩에 대한 데이터를 수집하고 데이터를 보내도록 할 수 있습니다.

1. AD 복제 상태 솔루션을 사용하여 컴퓨터가 모니터링하려는 도메인의 구성원인지 확인합니다.
2. 아직 연결되어 있지 않으면 [Windows 컴퓨터를 Azure Monitor에 연결](../../azure-monitor/platform/om-agents.md)하거나 [기존 Operations Manager 환경을 사용하여 Azure Monitor에 연결](../../azure-monitor/platform/om-agents.md)합니다.
3. 해당 컴퓨터에서 다음 레지스트리 키를 설정합니다.<br>키: **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\HealthService\Parameters\Management Groups\<ManagementGroupName>\Solutions\ADReplication**<br>값: **IsTarget**<br>값 데이터: **true**

   > [!NOTE]
   > Microsoft Monitoring Agent 서비스 (HealthService.exe)를 다시 시작 될 때까지 이러한 변경 내용을 적용 하지 않습니다.
   > ### <a name="install-solution"></a>솔루션 설치
   > [모니터링 솔루션 설치](solutions.md#install-a-monitoring-solution)에 설명된 프로세스에 따라 Log Analytics 작업 영역에 **Active Directory 복제 상태** 솔루션을 추가합니다. 추가 구성은 필요 없습니다.


## <a name="ad-replication-status-data-collection-details"></a>AD 복제 상태 데이터 컬렉션 세부 정보
다음 표에서는 데이터 수집 방법 및 AD 복제 상태에 대해 데이터가 수집되는 방식에 대한 기타 세부 정보를 보여 줍니다.

| 플랫폼 | 직접 에이전트 | SCOM 에이전트 | Azure Storage | SCOM 필요? | 관리 그룹을 통해 전송되는 SCOM 에이전트 데이터 | 수집 빈도 |
| --- | --- | --- | --- | --- | --- | --- |
| Windows |&#8226; |&#8226; |  |  |&#8226; |5일마다 |



## <a name="understanding-replication-errors"></a>복제 오류 이해

[!INCLUDE [azure-monitor-solutions-overview-page](../../../includes/azure-monitor-solutions-overview-page.md)]

AD 복제 상태 타일에는 현재 발생한 복제 오류 수가 표시됩니다. **중요 복제 오류**는 Active Directory 포리스트에 대한 [삭제 표식 수명](https://technet.microsoft.com/library/cc784932%28v=ws.10%29.aspx)의 75% 이상인 오류입니다.

![AD 복제 상태 타일](./media/ad-replication-status/oms-ad-replication-tile.png)

타일을 클릭하면 오류에 대한 자세한 정보를 볼 수 있습니다.
![AD 복제 상태 대시보드](./media/ad-replication-status/oms-ad-replication-dash.png)

### <a name="destination-server-status-and-source-server-status"></a>대상 서버 상태 및 원본 서버 상태
이러한 열에서 복제 오류가 발생하는 대상 서버와 원본 서버의 상태를 보여 줍니다. 각 도메인 컨트롤러 이름 다음의 숫자는 해당 도메인 컨트롤러의 복제 오류 수를 나타냅니다.

일부 문제는 원본 서버 관점에서 해결하기 쉽고 일부는 대상 서버 관점에서 해결하기 쉽기 때문에 대상 서버와 원본 서버에 대한 오류가 표시됩니다.

이 예제에서는 많은 대상 서버가 거의 동일한 오류 수를 가진 것을 볼 수 있지만 다른 서버보다 더 많은 오류를 가진 하나의 원본 서버(ADDC35)가 있습니다. 데이터를 해당 복제 파트너에 전송하는 데 실패하는 ADDC35에 문제가 있을 수 있습니다. ADDC35의 문제를 해결하면 대상 서버 영역에 나타나는 많은 오류를 해결할 수 있습니다.

### <a name="replication-error-types"></a>복제 오류 유형
이 영역에서 기업 전체에서 발견된 오류 유형에 대한 정보를 제공합니다. 각 오류에는 오류의 근본 원인을 확인하는 데 도움이 되는 고유한 숫자 코드와 메시지가 있습니다.

맨 왼쪽 도넛을 통해 사용자 환경에서 더 자주 나타나고 덜 자주 나타나는 오류를 이해할 수 있습니다.

여러 도메인 컨트롤러가 동일한 복제 오류를 경험하는 경우를 보여 줄 수 있습니다. 이 경우 한 도메인 컨트롤러에서 솔루션을 검색하거나 식별한 다음 동일한 오류에 영향을 받는 다른 도메인 컨트롤러에 이를 반복할 수 있습니다.

### <a name="tombstone-lifetime"></a>삭제 표시 수명
삭제 표시 수명은 삭제된 개체가 삭제 표시로 표시되는 기간을 결정하고 Active Directory 데이터베이스에 유지됩니다. 삭제된 개체가 삭제 표시 수명을 넘기는 경우 가비지 수집 프로세스가 Active Directory 데이터베이스에서 해당 개체를 자동으로 제거합니다.

최신 버전의 Windows에 대한 기본 삭제 표시 수명은 180일이지만 이전 버전에서는 60일이었으며 Active Directory 관리자가 명시적으로 변경할 수 있습니다.

삭제 표식 수명에 도달하는 중이거나 삭제 표식 수명이 지난 복제 오류가 있는지 알고 있어야 합니다. 두 개의 도메인 컨트롤러가 삭제 표식 수명이 지난 것을 유지하는 복제 오류를 경험하는 경우 기본 복제 오류를 수정하더라도 이러한 두 도메인 컨트롤러 간의 복제가 비활성화됩니다.

삭제 표식 수명 영역에서 비활성화된 복제에 문제가 발생할 수 있는 위치를 확인할 수 있습니다. **100% TSL 이상** 범주의 각 오류는 적어도 포리스트의 삭제 표시 수명 동안 해당 원본 및 대상 서버 간 복제되지 않은 파티션을 나타냅니다.

이 상황에서는 단순히 복제 오류를 수정하는 것으로 충분하지 않습니다. 최소한 복제를 다시 시작하기 전에 느린 개체를 수동으로 식별하도록 조사하고 정리해야 합니다. 도메인 컨트롤러의 역할 해제를 해야 할 수도 있습니다.

삭제 표식 수명이 지난 것을 유지하는 모든 복제 오류를 식별하는 것 외에도 **50-75% TSL** 또는 **75-100% TSL** 범주로 떨어지는 모든 오류에도 주의할 수 있습니다.

이는 명확하게 느리고 일시적이 아닌 오류이므로 이 오류를 해결하도록 사용자의 개입이 필요합니다. 좋은 소식은 아직 삭제 표시 수명에 도달하지 않았다는 것입니다. 삭제 표시 수명에 도달하기 *전에* 이 문제를 신속하게 해결하는 경우 최소한의 수동 개입으로 복제를 다시 시작할 수 있습니다.

앞에서 설명한 대로 AD 복제 상태 솔루션에 대한 대시보드 타일은 사용자 환경에서 *중요* 복제 오류 수를 표시하며 삭제 표시 수명의 75% 이상인 오류로 정의됩니다(TSL의 100%를 넘는 오류 포함). 이 값을 0으로 유지하도록 합니다.

> [!NOTE]
> 모든 삭제 표시 수명 백분율 계산은 Active Directory 포리스트에 대한 실제 삭제 표시 수명을 기준으로 하므로 삭제 표시 수명 값을 사용자 지정했더라도 해당 백분율이 정확하다는 것을 신뢰할 수 있습니다.
>
>

### <a name="ad-replication-status-details"></a>AD 복제 상태 세부 정보
목록 중 하나에 있는 항목을 클릭하면 로그 쿼리를 사용한 추가 세부 정보가 표시됩니다. 결과는 해당 항목과 관련된 오류만 표시하도록 필터링됩니다. 예를 들어 **대상 서버 상태(ADDC02)** 아래에 나열된 첫 번째 도메인 컨트롤러를 클릭하면 대상 서버로 나열된 해당 도메인 컨트롤러의 오류를 표시하도록 쿼리 결과가 필터링됩니다.

![쿼리 결과의 AD 복제 상태 오류](./media/ad-replication-status/oms-ad-replication-search-details.png)

여기에서 추가로 필터링, 로그 쿼리 수정 등을 수행할 수 있습니다. Azure Monitor에서 로그 쿼리를 사용하는 방법에 대한 자세한 내용은 [Azure Monitor의 로그 데이터 분석](../../azure-monitor/log-query/log-query-overview.md)을 참조하세요.

**HelpLink** 필드는 해당 특정 오류에 대한 추가 세부 정보로 TechNet 페이지의 URL을 표시합니다. 이 링크를 브라우저 창에 복사하여 붙여넣고 문제 해결 및 오류 해결에 대한 정보를 볼 수 있습니다.

**내보내기** 를 클릭하여 결과를 Excel로 내보낼 수도 있습니다. 데이터 내보내기를 통해 원하는 어떤 방식으로든 복제 오류 데이터를 시각화할 수 있습니다.

![Excel에서 내보낸 AD 복제 상태 오류](./media/ad-replication-status/oms-ad-replication-export.png)

## <a name="ad-replication-status-faq"></a>AD 복제 상태 FAQ
**Q: AD 복제 상태 데이터 업데이트 되는 빈도?**
A: 정보는 5일마다 업데이트됩니다.

**Q: 이 데이터가 업데이트 되는 빈도 구성 하는 방법이?**
A: 지금은 없습니다.

**Q: 복제 상태를 보려면 내 Log Analytics 작업 영역에 내 도메인 컨트롤러의 모든 추가 해야 합니까?**
A: 아니요, 단일 도메인 컨트롤러만 추가되어야 합니다. Log Analytics 작업 영역에 도메인 컨트롤러가 여러 개 있는 경우 모든 도메인 컨트롤러의 데이터가 Azure Monitor에 전송됩니다.

**Q: 내 Log Analytics 작업 영역에 도메인 컨트롤러를 추가하고 싶지 않습니다. 여전히 AD 복제 상태 솔루션을를 사용할 수 있나요?**

A: 예. 이 기능을 활성화하도록 레지스트리 키의 값을 설정할 수 있습니다. [비도메인 컨트롤러 사용](#enable-non-domain-controller)을 참조하세요.

**Q: 데이터 수집을 수행 하는 프로세스의 이름은 무엇입니까?**
A: AdvisorAssessment.exe

**Q: 어떻게 하기까지 걸리는 시간 데이터를 수집 하려면?**
A: 데이터 수집 시간은 Active Directory 환경의 크기에 따라 달라지지만 일반적으로 약 15분 미만이 소요됩니다.

**Q: 어떤 유형의 데이터를 수집 하나요?**
A: 복제 정보는 LDAP를 통해 수집됩니다.

**Q: 데이터를 수집 하는 경우 구성 하는 방법이?**
A: 지금은 없습니다.

**Q: 데이터를 수집 하려면 어떤 권한이 필요 하나요?**
A: Active Directory에 대한 일반 사용자 권한으로 충분합니다.

## <a name="troubleshoot-data-collection-problems"></a>데이터 수집 문제 해결
데이터를 수집하려면 AD 복제 상태 솔루션 팩에 하나 이상의 도메인 컨트롤러가 Log Analytics 작업 영역에 연결되어 있어야 합니다. 도메인 컨트롤러에 연결할 때까지 **데이터가 여전히 수집되고 있다고** 표시하는 메시지가 표시됩니다.

도메인 컨트롤러 중 하나를 연결하는 데 도움이 필요한 경우 [Azure Monitor에 Windows 컴퓨터 연결](../../azure-monitor/platform/om-agents.md)에서 문서를 볼 수 있습니다. 또는 도메인 컨트롤러가 기존 System Center Operations Manager 환경에 이미 연결된 경우 [Azure Monitor에 System Center Operations Manager 연결](../../azure-monitor/platform/om-agents.md)에서 문서를 볼 수 있습니다.

Azure Monitor 또는 System Center Operations Manager에 도메인 컨트롤러를 직접 연결하지 않으려는 경우 [비도메인 컨트롤러 사용](#enable-non-domain-controller)을 참조하세요.

## <a name="next-steps"></a>다음 단계
* [Azure Monitor의 로그 쿼리](../../azure-monitor/log-query/log-query-overview.md)를 사용하여 자세한 Active Directory 복제 상태 데이터를 봅니다.
