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
ms.openlocfilehash: 08328b3c872ce9e8a25db6075b00e0cee2fdf4fd
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55480398"
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
