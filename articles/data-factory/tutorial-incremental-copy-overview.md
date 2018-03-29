---
title: Azure Data Factory kullanarak verileri artımlı olarak kopyalama | Microsoft Docs
description: Bu öğreticiler verilerin, kaynak veri deposundan hedef veri deposuna nasıl artımlı olarak kopyalanacağını gösterir. İlki bir tablodan veri kopyalar.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/22/2018
ms.author: shlo
ms.openlocfilehash: 7265e20bf89cc9dbc1c44e568e779f2d13f87685
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="incrementally-load-data-from-a-source-data-store-to-a-destination-data-store"></a>Bir kaynak veri deposundan hedef veri deposuna artımlı olarak veri yükleme

İlk veriler yüklendikten sonra verileri artımlı olarak (veya delta) yükleme senaryosu, veri tümleştirme çözümlerinde sıkça kullanılır. Bu bölümdeki öğreticiler, Azure Data Factory sürüm 2 kullanarak artımlı veri yüklemenin farklı yollarını gösterir.

## <a name="delta-data-loading-by-using-a-watermark"></a>Filigran kullanarak delta veri yükleme
Bu durumda, kaynak veritabanında bir filigran tanımlayın. Filigran, en son güncelleştirilen zaman damgası veya artan bir anahtarı olan bir sütundur. Delta yükleme çözümü, eski bir filigran ile yeni bir filigran arasında değiştirilen verileri yükler. Bu yaklaşıma yönelik iş akışı şu diyagramda gösterilmiştir: 

![Filigran kullanmaya yönelik iş akışı](media/tutorial-incremental-copy-overview/workflow-using-watermark.png)

Adım adım yönergeler için şu öğreticilere bakın: 

- [Azure SQL Veritabanı’ndaki bir tablodan Azure Blob depolama Alanı’na artımlı olarak veri kopyalama](tutorial-incremental-copy-powershell.md)
- [Şirket içi SQL Server’daki birden fazla tablodan Azure SQL Veritabanı’na artımlı olarak veri kopyalama](tutorial-incremental-copy-multiple-tables-powershell.md)


## <a name="delta-data-loading-by-using-the-change-tracking-technology"></a>Değişiklik İzleme teknolojisini kullanarak delta veri yükleme
Değişiklik İzleme teknolojisi, SQL Server ve Azure SQL Veritabanı’nda bulunan, uygulamalar için verimli bir değişiklik izleme mekanizması sağlayan basit bir çözümdür. Bir uygulamanın eklenen, güncelleştirilen veya silinen verileri kolayca tanımlamasına olanak sağlar. 

Bu yaklaşıma yönelik iş akışı şu diyagramda gösterilmiştir:

![Değişiklik İzleme kullanmaya yönelik iş akışı](media/tutorial-incremental-copy-overview/workflow-using-change-tracking.png)

Adım adım yönergeler için şu öğreticilere bakın: <br/>
[Değişiklik İzleme teknolojisini kullanarak Azure SQL Veritabanı’ndan Azure Blob depolama alanına artımlı olarak veri kopyalama](tutorial-incremental-copy-change-tracking-feature-powershell.md)


## <a name="next-steps"></a>Sonraki adımlar
Şu öğreticiye ilerleyin: 

> [!div class="nextstepaction"]
>[Azure SQL Veritabanı’ndaki bir tablodan Azure Blob depolama Alanı’na artımlı olarak veri kopyalama](tutorial-incremental-copy-powershell.md)
