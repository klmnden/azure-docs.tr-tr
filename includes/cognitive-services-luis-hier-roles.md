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
ms.openlocfilehash: dd99218765c873120c4117a3ec1712fe0a605e66
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2018
ms.locfileid: "45637593"
---
**Soru**: rolleri ile basit bir varlık ile hiyerarşik bir varlık veya desen kullanmalısınız? 

**Yanıt**:, bağlıdır. Desenler ve örnek konuşma kullanıcının utterance temsil ettikleri, tam olarak ve belirli bir amaç için.  

Konuşma NET bir desenle sahip değilseniz, hiyerarşik varlıkları kullanın. 

|hiyerarşik varlıklar|Varlığın rolleriyle|
|--|--|
|alt varlıklar ile örnek konuşma hedefleri etiketlenmiş gerekir|Örnek konuşma olmalıdır **rolleri hedefleri etiketli olamaz**|
|desenleri kullanabilirsiniz|**gereken** desenleri kullanın|
|gerekebilir **daha fazla** örnek konuşma amacı,|gerekebilir ** amacı, daha az örnek konuşma|