---
title: Örnek verileri Azure blob kapsayıcılar, SQL Server'ı ve tabloları Hive | Microsoft Docs
description: Çeşitli Azure enviromnents içinde depolanan verileri araştırmak nasıl.
services: machine-learning
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: 80a9dfae-e3a6-4cfb-aecc-5701cfc7e39d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2017
ms.author: deguhath
ms.openlocfilehash: 73b0d3992627b7787ee46e61f62717a545c5b9e2
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="heading"></a>Örnek verileri Azure blob kapsayıcılar, SQL Server'ı ve tabloları Hive
Bu belge bağlantılar üç farklı Azure konumlardan birinde depolanan verileri örnek konularını makaleler için:

* **Azure blob kapsayıcı verileri** programlı olarak karşıdan yükleyip örnek Python kodu ile örnekleme örneklenen.
* **SQL Server verilerini** SQL ve Python programlama dili kullanılarak örneklenen. 
* **Tablo verisi hive** Hive sorgularını kullanarak örneklenen.

Aşağıdaki **menü** her bu Azure depolama ortamlarının veri örneği açıklayan konulara ait bağlantılar. 

[!INCLUDE [cap-sample-data-selector](../../../includes/cap-sample-data-selector.md)]

Bir adımda bu örnekleme görevdir [takım veri bilimi işlem (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

**Neden veri örneği?**

Analiz etmek için planlama dataset büyükse, genellikle aşağı örnek için daha küçük, ancak temsili ve daha kolay yönetilebilir bir boyutunu azaltmak için veri için iyi bir fikir değil. Bu, veri anlama, keşfi ve özellik Mühendisliği kolaylaştırır. Cortana Analytics işleminde rolü hızlı prototipi oluşturulurken veri işleme işlevleri ve makine öğrenimi modellerinin oluşturulmasına etkinleştirmektir.

