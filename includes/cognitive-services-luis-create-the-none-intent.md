---
title: include dosyası
description: include dosyası
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: luis
ms.topic: include
ms.custom: include file
ms.date: 12/21/2018
ms.author: diberry
ms.openlocfilehash: 0579b1e21e714e25e197cb5abf46793a38978d06
ms.sourcegitcommit: 7862449050a220133e5316f0030a259b1c6e3004
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/22/2018
ms.locfileid: "53755719"
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