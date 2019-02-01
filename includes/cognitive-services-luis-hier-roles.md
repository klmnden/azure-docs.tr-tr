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
ms.openlocfilehash: dee31fca841ea309128681637cc41fa0fb1e7aea
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55480296"
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
