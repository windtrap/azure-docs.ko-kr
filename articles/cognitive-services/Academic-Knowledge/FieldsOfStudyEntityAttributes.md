---
title: Field of Study 엔터티 특성 - Academic Knowledge API
titlesuffix: Azure Cognitive Services
description: Academic Knowledge API의 Field of Study 엔터티와 함께 사용할 수 있는 특성에 대해 알아봅니다.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/31/2017
ms.author: alch
ms.openlocfilehash: e9d6badf76efd03c0520a728af7b3e47b25f200a
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55878955"
---
# <a name="field-of-study-entity"></a>Field of Study 엔터티

<sub> *다음 특성은 Field of Study 엔터티에 한정됩니다. (Ty = '6') </sub>

Name    |설명                            |Type       | 작업
------- | ------------------------------------- | --------- | ----------------------------
Id      |엔터티 ID                              |Int64      |같음
FN      |연구 분야의 정규화된 이름         |문자열     |같음
DFN     |연구 분야 표시 이름            |문자열     |없음
CC      |연구 분야의 총 인용 횟수    |Int32      |없음  
ECC     |연구 분야의 총 예상 인용 횟수|Int32      |없음
FL      |연구 분야 계층 구조의 수준     |Int32      |Equals, <br/>IsBetween
FP.FN   |연구 이름의 부모 필드             |문자열     |같음
FP.FId  |연구 ID의 부모 필드               |Int64      |같음
FC.FN   |연구 이름의 자식 필드              |문자열     |같음
FC.FId  |연구 ID의 자식 필드                |Int64      |같음
