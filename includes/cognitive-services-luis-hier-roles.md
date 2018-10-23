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
ms.date: 08/02/2018
ms.author: diberry
ms.openlocfilehash: 1d7723f356274bbd18b1ea08e34046da82a1057c
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2018
ms.locfileid: "49646361"
---
**Soru**: rolleri ile basit bir varlık ile hiyerarşik bir varlık veya desen kullanmalısınız? 

**Yanıt**:, bağlıdır. Desenler ve örnek konuşma kullanıcının utterance temsil ettikleri, tam olarak ve belirli bir amaç için.  

Konuşma NET bir desenle sahip değilseniz, hiyerarşik varlıkları kullanın. 

|hiyerarşik varlıklar|Varlığın rolleriyle|
|--|--|
|alt varlıklar ile örnek konuşma hedefleri etiketlenmiş gerekir|Örnek konuşma olmalıdır **rolleri hedefleri etiketli olamaz**|
|desenleri kullanabilirsiniz|**gereken** desenleri kullanın|
|gerekebilir **daha fazla** örnek konuşma amacı,|gerekebilir **daha az** örnek konuşma amacı,|
