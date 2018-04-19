---
title: "JSON Azure Machine Learning çalışma ekranı kullanarak dönüştürme genişletin"
description: "Başvuru belge 'Genişletin JSON' dönüştürme için"
services: machine-learning
author: ranvijaykumar
ms.author: ranku
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc, reference
ms.topic: article
ms.date: 09/14/2017
ms.openlocfilehash: 21de94d2d0d3cc12aabcb8e9e8b0eec39b0a2710
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2018
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

