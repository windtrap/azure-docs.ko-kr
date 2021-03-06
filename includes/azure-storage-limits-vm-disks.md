---
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 03/18/2019
ms.author: rogarana
ms.openlocfilehash: 2936fd318f08c74675f7e8b382c861f4a28319fc
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58261404"
---
Azure 가상 머신에 데이터 디스크의 수를 연결할 수 있습니다. VM의 데이터 디스크에 대 한 확장성 및 성능 목표에 따라 성능 및 용량 요구 사항을 충족 해야 하는 디스크의 종류와 수를 확인할 수 있습니다.

> [!IMPORTANT]
> 최적의 성능을 얻기 위해 가상 머신에 연결되어 자주 활용되는 디스크의 수를 제한하여 가능한 제한을 방지합니다. 동시에 연결 된 모든 디스크가 자주 활용 되지, 가상 컴퓨터는 많은 수의 디스크를 지원할 수 있습니다.

**Azure에 대 한 관리 디스크:**

다음 표에서 기본 및 최대 제한은 구독 당 지역별 리소스 수

> | 리소스 | 기본 제한  | 최대 제한 |
> | --- | --- | --- |
> | 표준 관리 디스크 | 25,000 | 50,000 |
> | 표준 SSD 관리 디스크 | 25,000 | 50,000 |
> | 프리미엄 관리 디스크 | 25,000 | 50,000 |
> | Standard_LRS 스냅숏 | 25,000 | 50,000 |
> | Standard_ZRS 스냅숏 | 25,000 | 50,000 |
> | 관리 되는 이미지 | 25,000 | 50,000 |

* **표준 저장소 계정의 경우:** 표준 저장소 계정에 최대 총 요청 속도는 20,000 IOPS입니다. 모든 표준 저장소 계정에 가상 머신 디스크의 총 IOPS는이 제한을 초과할 수 없습니다.
  
    요청 속도 제한에 따라 단일 표준 저장소 계정에서 지 원하는 활용된 된 디스크의 수를 대략적으로 계산할 수 있습니다. 예를 들어 기본 계층 VM에서 활용된 된 디스크의 최대 수는 약 66, 디스크당 20000/300 IOPS. 표준 계층 VM에 대 한 활용된 된 디스크의 최대 수는 약 40 디스크당 20000/500 IOPS. 

* **Premium storage 계정:** 프리미엄 저장소 계정에 최대 총 처리량 속도는 50gbps입니다. 모든 VM 디스크의 총 처리량은 이 제한을 초과할 수 없습니다.

