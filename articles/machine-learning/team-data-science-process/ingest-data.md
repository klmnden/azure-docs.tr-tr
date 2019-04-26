---
title: Azure depolama ortamlarına - Team Data Science Process veri yükleme
description: Azure Blob Depolamadan/Depolamaya Veri Taşıma
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/09/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 56c45bf6db79ded64574a44399712951e82c1c3e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60303596"
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
