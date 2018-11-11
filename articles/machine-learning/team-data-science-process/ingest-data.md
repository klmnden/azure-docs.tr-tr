---
title: Analiz için Azure depolama ortamlarına veri yükleme | Microsoft Docs
description: Azure Blob Depolamadan/Depolamaya Veri Taşıma
services: machine-learning,storage
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: b8fbef77-3e80-4911-8e84-23dbf42c9bee
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: deguhath
ms.openlocfilehash: a5db14b99a81c373fbc72f523798e1f3bbdf9285
ms.sourcegitcommit: 96527c150e33a1d630836e72561a5f7d529521b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/09/2018
ms.locfileid: "51344510"
---
# <a name="load-data-into-storage-environments-for-analytics"></a>Analiz için depolama ortamlarına veri yükleme

Team Data Science Process veri veya alınan farklı depolama ortamları işlenen ya da işlemin her aşamasında en uygun şekilde analiz için çeşitli yüklenmiş olmasını gerektirir. Azure Blob Depolama, SQL Azure veritabanı, SQL Server Azure VM, HDInsight (Hadoop) ve Azure Machine Learning işlemi için yaygın olarak kullanılan veri hedefleri içerir. 

Aşağıdaki makaleler, verileri nerede depolanan ve işlenen çeşitli hedef ortamlara verilerin alımı açıklanmaktadır.

* / Veri deposundan [Azure Blob Depolama](move-azure-blob.md)
* İçin [Azure vm'lerde SQL Server](move-sql-server-virtual-machine.md)
* İçin [Azure SQL veritabanı](move-sql-azure.md)
* İçin [Hive tabloları](move-hive-tables.md)
* İçin [SQL bölümlenmiş tabloları](parallel-load-sql-partitioned-tables.md)
* Gelen [şirket içi SQL Server](move-sql-azure-adf.md)

İlk konum yanı sıra teknik ve iş gereksinimlerini biçimlendirin ve hangi verilerin analiz hedeflere ulaşmak için alınan gereken hedef ortamlarında, verilerin boyutunu belirler. Seyrek Tahmine dayalı bir model oluşturmak için gereken görevler çeşitli ulaşmak için birden fazla ortam arasında taşınacak veri gerektiren bir senaryo değildir. Bu görev dizisi, örneğin, bir veri keşfi, ön işleme, temizleme, aşağı örnekleme ve model eğitiminin içerebilir.
