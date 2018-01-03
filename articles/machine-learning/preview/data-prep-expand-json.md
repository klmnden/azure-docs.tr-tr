---
title: "JSON Azure Machine Learning çalışma ekranı kullanarak dönüştürme genişletin"
description: "Başvuru belge 'Genişletin JSON' dönüştürme için"
services: machine-learning
author: ranvijaykumar
ms.author: ranku
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc, reference
ms.topic: article
ms.date: 09/14/2017
ms.openlocfilehash: 614f4422aa987fc32dcce62826bb2477473fdc32
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="expand-json-transformation"></a>JSON dönüştürme genişletin
**Genişletin JSON** dönüştürme birden çok sütuna geçerli JSON metni içeren varolan bir sütunla genişletmek kullanıcıların sağlar.

## <a name="how-to-perform-this-transformation"></a>Bu dönüştürme gerçekleştirme

Bu dönüştürme gerçekleştirmek için aşağıdaki adımları izleyin:
1. JSON metni içeren kaynak sütunu seçin.
2. Seçin **genişletin JSON** gelen **dönüştüren** menüsü. Ya da seçin ve kaynak sütun başlığına sağ tıklayın **genişletin JSON** ve bağlam menüsünden. 
3. **Tamam**’a tıklayın. 
 
Yeni sütunlar yanındaki kaynak sütunu eklenir. Bu sütun JSON metninde hiyerarşisinin sonraki düzeyinden özellikleri içerir. Özellik anahtarı varsa, karşılık gelen sütunun adı oluşturmak için kullanılır. İç içe özellikler, bu kullanıcının JSON metni olarak korunur tekrarlayarak genişletebilir veya gerektiğinde atın.

## <a name="examples"></a>Örnekler

Kaynak sütunu *müşteri* iki sütuna Genişletilmiş *Customer.Name* ve *Customer.Phone*.

| Müşteri                                                | Customer.Name   | Customer.Phone |
|---------------------------------------------------------|-----------------|----------------|
| {"Name": "Carrie Dodson" "Phone",: "123-4567-890"}   | Carrie Dodson   | 123-4567-890   |
| {"Name": "Leonard Robledo" "Phone",: "456-7890-123"} | Leonard Robledo | 456-7890-123   |

