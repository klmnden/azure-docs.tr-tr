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
ms.date: 10/23/2018
ms.author: diberry
ms.openlocfilehash: 3803bc7f1e0da01fbaa8aa11bc6e8fc5c508b5ae
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/12/2019
ms.locfileid: "59528846"
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
