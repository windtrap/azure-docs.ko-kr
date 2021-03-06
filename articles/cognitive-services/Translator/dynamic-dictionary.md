---
title: 동적 사전 - Translator Text API
titlesuffix: Azure Cognitive Services
description: Translator Text API의 동적 사전 기능을 사용하는 방법입니다.
services: cognitive-services
author: v-pawal
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 02/21/2019
ms.author: v-jansko
ms.openlocfilehash: 0b4362d78ef105c249aafb4c6b203f69754a56c8
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2019
ms.locfileid: "58916600"
---
# <a name="how-to-use-the-dynamic-dictionary-feature-of-the-translator-text-api"></a>Translator Text API의 동적 사전 기능을 사용하는 방법

단어나 구에 적용할 번역을 이미 알고 있는 경우 요청 내에 태그로 제공할 수 있습니다. 동적 사전은 적절한 이름 및 제품 이름과 같은 복합 명사에 사용할 때만 안전합니다.

**구문**

<mstrans:dictionary translation=”구 번역”>구</mstrans:dictionary>

**De-예: en:**

원본 입력: 단어 <mstrans:dictionary translation=\"wordomatic\">단어 또는 구</mstrans:dictionary>는 사전 항목입니다.

대상 출력: Das Wort "wordomatic" ist ein Wörterbucheintrag.

이 기능은 HTML 모드를 사용할 때와 그렇지 않을 때 같은 결과를 가져옵니다.

이 기능은 자주 사용하지 않는 것이 좋습니다. 번역을 사용자 지정하는 보다 적절하고 훨씬 더 나은 방법은 Custom Translator를 사용하는 것입니다. Custom Translator는 컨텍스트 및 통계적 확률을 최대한 활용합니다. 컨텍스트에 따라 사용자 작업 또는 구를 보여 주는 학습 데이터를 보유하고 있거나 이러한 데이터를 만들 수 있으면 훨씬 더 나은 결과를 얻을 수 있습니다. 사용자 지정 변환기에 대한 자세한 내용은 [https://aka.ms/CustomTranslator](https://aka.ms/CustomTranslator)에서 찾을 수 있습니다.
