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
ms.openlocfilehash: dbda4b7b6d82e8cf1e89dc78ce82efbac08b9933
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "35651377"
---
# <a name="expand-json-transformation"></a>JSON dönüştürme genişletin
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

