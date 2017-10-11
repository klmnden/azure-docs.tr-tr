---
title: "Azure SQL Data Warehouse'da Dağıtılmış veri nasıl çalışır? | Microsoft Docs"
description: "Yüksek düzeyde paralel işleme (MPP) ve Azure SQL veri ambarı ve paralel veri ambarı tablolarda dağıtma seçeneklerini verileri nasıl dağıtıldığını öğrenin."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: bae494a6-7ac5-4c38-8ca3-ab2696c63a9f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 07/12/2017
ms.author: jrj;barbkess
ms.openlocfilehash: 3c166acb17193caae32d7bad133ec510ff679353
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="distributed-data-and-distributed-tables-for-massively-parallel-processing-mpp"></a>Dağıtılmış veri ve dağıtılmış tablolar için yüksek düzeyde paralel işleme (MPP)
Azure SQL veri ambarı ve paralel veri ambarı, Microsoft'un yüksek düzeyde paralel işleme (MPP) sistemleri olan kullanıcı verilerini nasıl dağıtıldığını öğrenin. Dağıtılmış veri etkili bir şekilde kullanmak için veri ambarınız tasarlama sorgu MPP mimarisi yararları işleme elde size yardımcı olur. Birkaç veritabanı tasarım seçenekleri sorgu performansını iyileştirme üzerinde önemli bir etkisi olabilir.  

> [!NOTE]
> Azure SQL veri ambarı ve paralel veri ambarı aynı yüksek düzeyde paralel işleme (MPP) tasarım kullanır, ancak bazı farklar nedeniyle temel platform sahiptirler. SQL Data Warehouse Azure üzerinde çalışan hizmet (PaaS) olarak platformudur. Paralel veri ambarı üzerinde Analytics Platform System (Windows Server çalıştıran bir şirket içi gereç olan AP'ler) çalışır.
> 
> 

## <a name="what-is-distributed-data"></a>Dağıtılmış veri nedir?
SQL veri ambarı ve paralel veri ambarı, Dağıtılmış veri MPP sistem birden fazla konumda depolanan kullanıcı verileri ifade eder. Konumların her birini bağımsız depolama ve kendi veri bölümünü sorguları çalıştırır işleme birimi olarak çalışır. Dağıtılmış veri sorguları yüksek sorgu performansı elde etmek için paralel olarak çalıştırmak için temel önemdedir.

Veri dağıtmak için veri ambarı kullanıcı tablodaki her satır bir dağıtılmış konuma atar.  Bir karma dağıtım veya hepsini yöntemini tablolarla dağıtabilirsiniz. Dağıtım yöntemi CREATE TABLE deyiminde belirtilir. 

## <a name="hash-distributed-tables"></a>Karma dağıtılmış tabloları
Aşağıdaki diyagram, tam (dağıtılmış olmayan tablo) karma dağıtılmış bir tablo olarak nasıl depolanır gösterir. Belirleyici işlevi her satır için bir dağıtım ait atar. Tablo tanımı sütunlardan biri dağıtım sütunu olarak atanmış. Karma işlevi, her satır için bir dağıtım atamak için dağıtım sütununda değeri kullanır.

Sistemde distinctness, veri eğme ve sorgu türleri gibi bir dağıtım sütun seçimi için başarım düşünceleri yoktur.

![Dağıtılmış tablo](media/sql-data-warehouse-distributed-data/hash-distributed-table.png "dağıtılmış tablo")  

* Her satır için bir dağıtım aittir.  
* Belirleyici karma algoritma her satır için bir dağıtım atar.  
* Dağıtım başına tablo satır sayısı tablolar farklı boyutlarda tarafından gösterildiği gibi değişir.

## <a name="round-robin-distributed-tables"></a>Hepsini dağıtılmış tabloları
Hepsini dağıtılmış tablo satırlarını her satır için bir dağıtım sıralı bir biçimde atayarak dağıtır. Hepsini tabloya veri yüklemek hızlı, ancak sorgu performansı yavaş olabilir.  Genellikle bir hepsini tablo birleştirme süren bir doğru sonucu oluşturmak sorgu etkinleştirmek için satırları reshuffling gerektirir.

## <a name="distributed-storage-locations-are-called-distributions"></a>Dağıtılmış depolama konumları dağıtımları adlandırılır
Dağıtılmış her konum bir dağıtım adı verilir. Bir sorgu paralel olarak çalıştırıldığında, her dağıtım bir SQL sorgusu, veri bölümünü gerçekleştirir. SQL veri ambarı SQL veritabanı sorgusu çalıştırmak için kullanır. Sorguyu çalıştırmak için SQL Server paralel veri ambarı kullanır. Bu hiçbir şey paylaşılmayan Mimari Tasarım genişleme paralel bilgi işlem ulaşmak için temel önemdedir.

### <a name="can-i-view-the-distributions"></a>Dağıtımları görüntüleyebilir miyim?
Her dağıtım bir dağıtım Kimliğine sahip ve SQL veri ambarı ve paralel veri ambarı ilgilidir sistem görünümlerde görülebilir. Dağıtım kimliği, sorgu performansını ve diğer sorunları gidermek için kullanabilirsiniz. Sistem görünümleri listesi için bkz: [MPP sistem görünümü](sql-data-warehouse-reference-tsql-statements.md).

## <a name="difference-between-a-distribution-and-a-compute-node"></a>Bir dağıtım ve işlem düğümü arasındaki fark
Bir dağıtım temel Dağıtılmış veri depolamak ve paralel sorgular için birimidir. Dağıtımları işlem düğümlerinde gruplandırılır. Her işlem düğümünde bir veya daha fazla dağıtımları izler.  

* Analiz platformu sistemi işlem düğümleri merkezi bir donanım mimarisi ve genişleme özellikleri bileşeni olarak kullanır. Her zaman işlem düğümü başına sekiz dağıtımları karma dağıtılmış tablolar için diyagramda gösterildiği gibi kullanır. İşlem düğümü sayısını ve bu nedenle dağıtımları, sayısı Gereci için satın aldığınız işlem düğümleri sayısı tarafından belirlenir. Örneğin, sekiz işlem düğümleri satın aldığınız durumlarda 64 dağıtımları (8 işlem düğümleri x 8 dağıtımları/düğümü) alın. 
* SQL veri ambarı sabit sayıda 60 dağıtımları ve esnek bir işlem düğümü sayısını vardır. İşlem düğümlerini Azure bilgi işlem ve depolama kaynakları ile uygulanır. İşlem düğümü sayısını arka uç hizmeti iş yükü ve veri ambarı için belirttiğiniz bilgi işlem kapasitesi (Dwu) göre değiştirebilirsiniz. İşlem düğümü sayısını değiştiğinde işlem düğümü başına dağıtımların sayısı da değişir. 

### <a name="can-i-view-the-compute-nodes"></a>İşlem düğümleri görüntüleyebilir miyim?
Her işlem düğümü, bir düğüm kimliği vardır ve SQL veri ambarı ve paralel veri ambarı ilgilidir sistem görünümleri görünür olur.  Sistem görünümleri adları sys.pdw_nodes ile başlayan node_id sütununda arayarak işlem düğümünde görebilirsiniz. Sistem görünümleri listesi için bkz: [MPP sistem görünümü](sql-data-warehouse-reference-tsql-statements.md).

## <a name="Replicated"></a>Çoğaltılmış tablolar
Çoğaltılan bir tablo içeren bir tablonun kopyasını her işlem düğümü üzerinde tam olarak depolanır. Tablo çoğaltma JOIN veya toplama önce işlem düğümleri arasında veri aktarmak için gereksinimini ortadan kaldırır. Çoğaltılmış tablolar yalnızca tam tablo her işlem düğümü üzerinde depolamak için gereken ek depolama alanı nedeniyle küçük tablolarla uygulanabilir.  

Aşağıdaki diyagram, her işlem düğümü üzerinde depolanan çoğaltılmış bir tablo gösterir. SQL Data Warehouse için çoğaltılmış tablo her işlem düğümü üzerinde bir dağıtım veritabanı tam olarak kopyalanır. Paralel veri ambarı için işlem düğümüne atanan tüm disklere çoğaltılmış tablo depolanır.  Bu disk strateji, SQL Server dosya grupları kullanılarak uygulanır.  

![Yinelenmiş tablo](media/sql-data-warehouse-distributed-data/replicated-table.png "yinelenmiş tablosu") 

## <a name="next-steps"></a>Sonraki adımlar
Dağıtılmış tabloları etkili bir şekilde kullanmak için bkz: [SQL Data Warehouse tablolarda dağıtma](sql-data-warehouse-tables-distribute.md)  

