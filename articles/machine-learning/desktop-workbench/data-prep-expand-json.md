---
title: Azure Machine Learning Workbench'i kullanarak JSON dönüştürme genişletin
description: Başvuru belgesini 'JSON' ı genişletin dönüştürme için
services: machine-learning
author: ranvijaykumar
ms.author: ranku
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: mvc, reference
ms.topic: article
ms.date: 09/14/2017
ROBOTS: NOINDEX
ms.openlocfilehash: 0a5cbca114b220686d656f93edb00a199e3cbeeb
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46989830"
---
# <a name="expand-json-transformation"></a>JSON dönüştürme genişletin

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 


**Genişletin JSON** dönüştürme kullanıcıların birden çok sütuna geçerli JSON metnini içeren varolan bir sütunla genişletmesini sağlar.

## <a name="how-to-perform-this-transformation"></a>Bu dönüşüm gerçekleştirme

Bu dönüştürme işlemini gerçekleştirmek için aşağıdaki adımları izleyin:
1. JSON metnini içeren bir kaynak sütun seçin.
2. Seçin **genişletin JSON** gelen **dönüştüren** menüsü. Ya da seçin ve kaynak sütun başlığına sağ tıklayın **genişletin JSON** bağlam menüsünden. 
3. **Tamam** düğmesine tıklayın. 
 
Yeni sütunlar yanındaki kaynak sütunu eklenir. Bu sütunlar, JSON metnini hiyerarşideki bir sonraki düzeyini özelliklerini içerir. Özellik anahtarı varsa, karşılık gelen sütunun adı oluşturmak için kullanılır. İç içe özellikler, bu kullanıcı JSON metnini korunur çalıştırmalarınızı genişletebilir veya gerektiğinde atın.

## <a name="examples"></a>Örnekler

Kaynak sütunu *müşteri* iki sütuna Genişletilmiş *Customer.Name* ve *Customer.Phone*.

| Müşteri                                                | Customer.Name   | Customer.Phone |
|---------------------------------------------------------|-----------------|----------------|
| {"Name": "Carrie Dodson" "Phone",: "123-4567-890"}   | Carrie Dodson   | 123-4567-890   |
| {"Name": "Leonard Robledo" "Phone",: "456 7890 123"} | Leonard Robledo | 456-7890-123   |

