---
title: "Azure Machine Learning veriler hazırlıkları yürütme ve veri ortamlar birleşimlerini desteklenen | Microsoft Docs"
description: "Bu belgede, Azure Machine Learning veriler hazırlıkları farklı çalışma zamanları ve veri kaynaklarını desteklenen birleşimleri tam bir listesi sağlanmaktadır"
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 09/15/2017
ms.openlocfilehash: 248cbcfe35db646a8bc71c6f825dcaa8a4661e91
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="supported-matrix-for-this-release"></a>Bu sürüm için desteklenen Matrisi 
Ne zaman Azure Machine Learning veri kaynakları veya Azure Machine Learning veri ya da bir Pandas alma hazırlıklar, kullanarak verileri kodunuzu yükler veya Spark dataframe, deneme aşağıdaki birleşimlerini işlem ortamlarını ve veri konumlar desteklenir:

|     |Yerel dosyaları  |Azure Blob depolama  |SQL Server veritabanı ***  |
|---------|---------|---------|---------|---------|
|Yerel Python    |     Desteklenen    |Desteklenmiyor         | Desteklenmiyor        |         |
|Docker (Linux VM) Python     |Yalnızca proje dosyalarında desteklenen *         | Desteklenmiyor        |        Desteklenmiyor |         |
|Docker (Linux VM) PySpark     |Yalnızca proje dosyalarında desteklenen *     |Desteklenen         | Destekleniyor**        |         |
|Azure veri bilimi sanal makine Python     |Yalnızca proje dosyalarında desteklenen *         |Desteklenmiyor         |Desteklenmiyor         |         |
|Azure veri bilimi sanal makine PySPark     | Yalnızca proje dosyalarında desteklenen *        |Desteklenmiyor         |Desteklenmiyor         |         |
|Azure Hdınsight PySpark     | Desteklenmiyor        |Desteklenen         |Destekleniyor**         |         |
|Azure Hdınsight Python     | Desteklenmiyor        | Desteklenmiyor        | Desteklenmiyor        |         |

Azure Data Lake Store için herhangi bir işlem hedef şu anda desteklenmiyor.

* Yerel dosya yolları kullanıldığında, projedeki dosyaları işlem ortamına kopyalanır ve orada okuyun. Proje dışındaki dosyalara kopyalanmadı ve yolları artık işlem ortamında giderir. Kodunuzu bir yerel dosyayı yerel olarak çalıştırırken kullanabilmesi için veri kaynağını değiştirme kullanmayı düşünün. Farklı Çalıştır yapılandırması için bir Azure Storage blobu sonra geçin. Büyük veri yalnızca çalışma bazı yapılandırmalarda çalışır yönetmek için veri kaynaklarını örnekleme desteği de kullanabilirsiniz.

** Maven JDBC SQL Server sürücüsü 6.2.1 kullanır. Bu paket (veya uyumlu bir) bilgi işlem ortamı için spark_dependencies.yml dosyanızdaki bulunduğundan emin olun gerekir.

Destekleyen Azure SQL Database veya SQL Server veritabanı işlem ortamından ulaşılabilen sağlanan. 
