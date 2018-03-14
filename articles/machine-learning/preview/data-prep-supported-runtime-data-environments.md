---
title: "Azure Machine Learning veriler hazırlıkları yürütme ve veri ortamlar birleşimlerini desteklenen | Microsoft Docs"
description: "Bu belgede, Azure Machine Learning veriler hazırlıkları farklı çalışma zamanları ve veri kaynaklarını desteklenen birleşimleri tam bir listesi sağlanmaktadır"
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 02/01/2018
ms.openlocfilehash: bdd1c51c915787d9e9522f6691ae0ff06d546484
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2018
---
# <a name="supported-matrix-for-this-release"></a>Bu sürüm için desteklenen Matrisi 
Ne zaman Azure Machine Learning veri kaynakları veya Azure Machine Learning veri ya da bir Pandas alma hazırlıklar, kullanarak verileri kodunuzu yükler veya Spark dataframe, deneme aşağıdaki birleşimlerini işlem ortamlarını ve veri konumlar desteklenir:

|     |Yerel dosyaları  |Azure Blob depolama  |SQL Server veritabanı ***  |
|---------|---------|---------|---------|---------|
|Local Python    |     Desteklenen    |Desteklenmiyor         | Desteklenmiyor        |         |
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
