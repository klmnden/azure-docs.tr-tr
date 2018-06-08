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
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2017
ms.author: deguhath
ms.openlocfilehash: 0efd754936b67611a747c6c5756de92443a937e4
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34838016"
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

