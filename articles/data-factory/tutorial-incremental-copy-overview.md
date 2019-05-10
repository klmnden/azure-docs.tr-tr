---
title: Azure Data Factory kullanarak verileri artımlı olarak kopyalama | Microsoft Docs
description: Bu öğreticiler verilerin, kaynak veri deposundan hedef veri deposuna nasıl artımlı olarak kopyalanacağını gösterir. İlki bir tablodan veri kopyalar.
services: data-factory
documentationcenter: ''
author: dearandyxu
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 01/22/2018
ms.author: yexu
ms.openlocfilehash: 1d8b31e55a2a230385730c924d3e6bcc6072e7ea
ms.sourcegitcommit: 17411cbf03c3fa3602e624e641099196769d718b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2019
ms.locfileid: "65520435"
---
# <a name="incrementally-load-data-from-a-source-data-store-to-a-destination-data-store"></a>Bir kaynak veri deposundan hedef veri deposuna artımlı olarak veri yükleme

İlk veriler yüklendikten sonra verileri artımlı olarak (veya delta) yükleme senaryosu, veri tümleştirme çözümlerinde sıkça kullanılır. Bu bölümdeki öğreticiler, Azure Data Factory kullanarak artımlı veri yüklemenin farklı yollarını gösterir.

## <a name="delta-data-loading-from-database-by-using-a-watermark"></a>Veritabanından bir filigran kullanarak delta veri yükleme
Bu durumda, kaynak veritabanında bir filigran tanımlayın. Filigran, en son güncelleştirilen zaman damgası veya artan bir anahtarı olan bir sütundur. Delta yükleme çözümü, eski bir filigran ile yeni bir filigran arasında değiştirilen verileri yükler. Bu yaklaşıma yönelik iş akışı şu diyagramda gösterilmiştir: 

![Filigran kullanmaya yönelik iş akışı](media/tutorial-incremental-copy-overview/workflow-using-watermark.png)

Adım adım yönergeler için şu öğreticilere bakın: 

- [Azure SQL Veritabanı’ndaki bir tablodan Azure Blob depolama Alanı’na artımlı olarak veri kopyalama](tutorial-incremental-copy-powershell.md)
- [Şirket içi SQL Server’daki birden fazla tablodan Azure SQL Veritabanı’na artımlı olarak veri kopyalama](tutorial-incremental-copy-multiple-tables-powershell.md)

## <a name="delta-data-loading-from-sql-db-by-using-the-change-tracking-technology"></a>SQL DB değişiklik izleme teknolojisini kullanarak delta veri yükleme
Değişiklik İzleme teknolojisi, SQL Server ve Azure SQL Veritabanı’nda bulunan, uygulamalar için verimli bir değişiklik izleme mekanizması sağlayan basit bir çözümdür. Bir uygulamanın eklenen, güncelleştirilen veya silinen verileri kolayca tanımlamasına olanak sağlar. 

Bu yaklaşıma yönelik iş akışı şu diyagramda gösterilmiştir:

![Değişiklik İzleme kullanmaya yönelik iş akışı](media/tutorial-incremental-copy-overview/workflow-using-change-tracking.png)

Adım adım yönergeler için şu öğreticilere bakın: <br/>
[Değişiklik İzleme teknolojisini kullanarak Azure SQL Veritabanı’ndan Azure Blob depolama alanına artımlı olarak veri kopyalama](tutorial-incremental-copy-change-tracking-feature-powershell.md)

## <a name="loading-new-and-changed-files-only-by-using-lastmodifieddate"></a>Yalnızca LastModifiedDate kullanarak yeni ve değiştirilmiş dosyalar yükleniyor
Yalnızca hedef deponun LastModifiedDate'i kullanarak yeni ve değiştirilen dosyaları kopyalayabilirsiniz. ADF kaynak deposundan tüm dosyaları tarama, dosya filtresi tarafından kendi LastModifiedDate uygulamak ve yalnızca son daraltılmasından yeni ve güncelleştirilmiş dosya için hedef depoyu kopyalayın.  Lütfen ADF tarama büyük miktarlarda dosyaları sağlar, ancak yalnızca birkaç dosyaları kopyalama hedefi için hala dosya tarama nedeniyle uzun süreli de zaman alıcı beklediğiniz unutmayın.   

Adım adım yönergeler için şu öğreticilere bakın: <br/>
[Yeni ve değiştirilmiş dosyalar üzerinde LastModifiedDate Azure Blob depolama alanından Azure Blob depolama alanına göre artımlı olarak kopyalama](tutorial-incremental-copy-lastmodified-copy-data-tool.md)

## <a name="loading-new-files-only-by-using-time-partitioned-folder-or-file-name"></a>Yalnızca zaman kullanarak yeni dosyaları yükleme, klasör veya dosya adı bölümlenmiş.
Burada dosya veya klasör zaten dosya veya klasör adı (örneğin, /yyyy/mm/dd/file.csv) bir parçası olarak timeslice bilgilerle bölümlenmiş zaman yeni dosyalar yalnızca kopyalayabilirsiniz. Bu artımlı yükleme yeni dosyalar için çoğu performans yaklaşımdır. 

Adım adım yönergeler için şu öğreticilere bakın: <br/>
[Yeni dosyaları saat bölümlenmiş klasör veya dosya adı Azure Blob depolamadan Azure Blob depolama alanına göre artımlı olarak kopyalama](tutorial-incremental-copy-partitioned-file-name-copy-data-tool.md)

## <a name="next-steps"></a>Sonraki adımlar
Şu öğreticiye ilerleyin: 

> [!div class="nextstepaction"]
>[Azure SQL Veritabanı’ndaki bir tablodan Azure Blob depolama Alanı’na artımlı olarak veri kopyalama](tutorial-incremental-copy-powershell.md)
