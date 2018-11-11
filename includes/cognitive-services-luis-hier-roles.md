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
ms.date: 10/23/2018
ms.author: diberry
ms.openlocfilehash: 0ad3b360611a12b95568e9a20f13a57c93b2e603
ms.sourcegitcommit: f0c2758fb8ccfaba76ce0b17833ca019a8a09d46
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2018
ms.locfileid: "51208313"
---
## <a name="roles-versus-hierarchical-entities"></a>Hiyerarşik varlıkları ve rolleri

Rolleri ile basit bir varlık ile hiyerarşik bir varlık veya desen kullanmalısınız? 

Bağlıdır. Desenler ve örnek konuşma kullanıcının utterance temsil ettikleri, tam olarak ve belirli bir amaç için.  

Konuşma NET bir desenle sahip değilseniz, hiyerarşik varlıkları kullanın. 

|hiyerarşik varlıklar|Varlığın rolleriyle|
|--|--|
|Amaç'ın örnek konuşma ve desenleri|Yalnızca kullanılabilir desenleri|
|Alt varlıklar ile hedefi, örnek konuşma etiketli gerekir|Örnek konuşma amacı, içinde rolleri etiketli olamaz|
|Gerekebilir **daha fazla** varlık ayıklanacak hedefi olarak örnek konuşma|İhtiyacınız olursa **daha az** örnek konuşma amacı,|
