---
title: Azure Machine Learning veri hazırlıkları yürütme ve veri ortamları birleşimlerini desteklenen | Microsoft Docs
description: Bu belge, desteklenen birleşimleri farklı çalışma zamanlarını ve veri kaynaklarının tam listesi için Azure Machine Learning veri hazırlıkları sağlar.
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: ''
ms.devlang: ''
ms.topic: article
ms.date: 02/01/2018
ROBOTS: NOINDEX
ms.openlocfilehash: 9168ac1d26432ca3eee5a59b63aa0cec3ae72856
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46989065"
---
# <a name="supported-matrix-for-this-release"></a>Bu sürüm için desteklenen Matrisi 

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 


Azure Machine Learning veri kaynakları veya Azure Machine Learning veri alma ya da bir Pandas hazırlıkları, kullanarak kod veri yükler veya Spark dataframe, aşağıdaki birleşimlerini deneme ortamları ve veri konumları işlem olduğunda desteklenir:

|     |Yerel dosyaları  |Azure Blob depolama  |SQL Server veritabanı ***  |
|---------|---------|---------|---------|---------|
|Yerel Python    |     Desteklenen    |Desteklenmiyor         | Desteklenmiyor        |         |
|Docker (Linux VM) Python     |Yalnızca proje dosyalarında desteklenen *         | Desteklenmiyor        |        Desteklenmiyor |         |
|Docker (Linux VM) PySpark     |Yalnızca proje dosyalarında desteklenen *     |Desteklenen         | Destekleniyor**        |         |
|Azure veri bilimi sanal makinesi Python     |Yalnızca proje dosyalarında desteklenen *         |Desteklenmiyor         |Desteklenmiyor         |         |
|Azure veri bilimi sanal makinesi PySPark     | Yalnızca proje dosyalarında desteklenen *        |Desteklenmiyor         |Desteklenmiyor         |         |
|Azure HDInsight PySpark     | Desteklenmiyor        |Desteklenen         |Destekleniyor**         |         |
|Azure HDInsight Python     | Desteklenmiyor        | Desteklenmiyor        | Desteklenmiyor        |         |

Azure Data Lake Store için herhangi bir işlem hedef şu anda desteklenmiyor.

* Yerel dosya yollarını kullanıldığında, projedeki dosyaları işlem ortamına kopyalanır ve burada okuyun. Proje dışındaki dosyalara kopyalanmaz ve yolları işlem ortamı artık çözümlenmez. Kodunuzu yerel bir dosyayı yerel olarak çalıştırılırken kullanabilmesi için veri kaynağı değiştirme kullanmayı düşünün. Bir farklı çalıştır yapılandırması için bir Azure depolama blobu dönersiniz. Büyük verileri yalnızca belirli çalıştırma yapılandırmaları çalışır yönetmek için veri kaynaklarında örnekleme desteği de kullanabilirsiniz.

** Maven JDBC SQL Server sürücüsünü 6.2.1 kullanır. Bu paket (veya uyumlu bir tane) işlem ortamı için spark_dependencies.yml dosyanızdaki ekleneceğini emin olmanız gerekir.

Destekleyen Azure SQL veritabanı veya SQL Server veritabanı işlem ortamından ulaşılabilir sağlanır. 
