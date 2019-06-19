---
title: include dosyası
description: include dosyası
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.subservice: luis
ms.topic: include
ms.custom: include file
ms.date: 12/21/2018
ms.author: diberry
ms.openlocfilehash: 355fe134939b26c51d6e03368f782845628a6b96
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188679"
---
İstemci uygulaması, bir utterance anlamlı veya uygulama için uygun değilse bilmesi gerekir. **Hiçbiri** amacı, bir utterance istemci uygulaması tarafından yanıtlanmış olamaz belirlemek üzere her bir uygulama oluşturma sürecinin bir parçası olarak eklenir.

LUIS döndürürse **hiçbiri** hedefi için bir utterance istemci uygulamanızı kullanıcının konuşma son veya Konuşmaya devam etmek için daha fazla yönergeler vermek isteyip istemediğini isteyebilir. 

> [!CAUTION] 
> Değil bırakın **hiçbiri** hedefi boş. 

1. Sol panelden **Intents** (Amaçlar) öğesini seçin.

2. **None** (Yok) amacını seçin. Kullanıcı girebilirsiniz ancak İnsan Kaynakları uygulamanıza ilgili olmayan üç konuşma ekleyin:

    | Örnek konuşmalar|
    |--|
    |Barking dogs are annoying (Havlayan köpekler rahatsız eder)|
    |Order a pizza for me (Bana bir pizza söyle)|
    |Penguins in the ocean (Okyanustaki penguenler)|
