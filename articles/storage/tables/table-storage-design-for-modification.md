---
title: Veri değişikliği için Azure depolama tabloları tasarlama | Microsoft Docs
description: Azure tablo depolama, veri değişikliği için tablolar tasarlayın.
services: storage
author: MarkMcGeeAtAquent
ms.service: storage
ms.topic: article
ms.date: 04/23/2018
ms.author: sngun
ms.subservice: tables
ms.openlocfilehash: e993d169025f9b76c5e813bae31ca6cb2a39ba71
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60325893"
---
# <a name="design-for-data-modification"></a>Veri değişikliği için tasarım
Bu makalede, ekleme, güncelleştirme iyileştirmek için tasarım konuları üzerinde odaklanır ve siler. Bazı durumlarda (Tasarım stillerden yönetme teknikleri olmasına rağmen tasarımlarında ilişkisel veritabanları için yaptığınız gibi veri değişikliği için en iyi duruma getirme tasarımları karşı sorgulama için en iyi duruma getirme tasarımları arasındaki dengeyi değerlendirin gerekir farklı ilişkisel bir veritabanındaki). Bölüm tablosu tasarım desenleri, tablo hizmeti için bazı ayrıntılı tasarım desenleri açıklar ve bazı bu stillerden vurgular. Uygulamada, varlıklar da değiştirmek için birçok tasarım varlıkları sorgulama için en iyi duruma getirilmiş ayrıca iş bulabilirsiniz.  

## <a name="optimize-the-performance-of-insert-update-and-delete-operations"></a>Ekleme, güncelleştirme ve silme işlemlerinin performansını en iyi duruma getirme
Bir varlığı silme ve güncelleştirmek için kullanarak tanımlayamayabilir olmalıdır **PartitionKey** ve **RowKey** değerleri. Tercih ettiğiniz bu açısından **PartitionKey** ve **RowKey** varlıkları değiştirme benzer ölçütleri için varlıklar olarak tanımlamak istediğiniz çünkü noktası sorguları desteklemek için tercih ettiğiniz izlemelisiniz için mümkün olduğunca verimli şekilde. Bulmak için bir varlık bulmaya verimsiz bir bölüm veya tablo taraması kullanmak istemediğiniz **PartitionKey** ve **RowKey** güncelleştirmek veya silmek için gereken değerleri.  

Performans veya, ekleme, en iyi duruma getirme tablo Tasarım desenleri adresi bölümünde aşağıdaki desenleri, güncelleştirme ve silme işlemleri:  

* [Yüksek hacimli düzeni Sil](table-storage-design-patterns.md#high-volume-delete-pattern) -tablo silerek varlıklarını silme; eşzamanlı silinmesi için tüm varlıkları kendi ayrı bir tabloda depolayarak yüksek hacimli varlıkları silinmesini etkinleştirme.  
* [Veri serisi deseni](table-storage-design-patterns.md#data-series-pattern) -Store eksiksiz bir veri serisinde yaptığınız istek sayısını en aza indirmek için tek bir varlık.  
* [Geniş bir varlıklar deseni](table-storage-design-patterns.md#wide-entities-pattern) -mantıksal varlıklarla birden çok 252 özellik depolamak için birden çok fiziksel varlıkları kullanın.  
* [Büyük varlıklar deseni](table-storage-design-patterns.md#large-entities-pattern) -büyük özellik değerlerini depolamak için blob depolama alanını kullanın.  

## <a name="ensure-consistency-in-your-stored-entities"></a>İçinde depolanan varlıklarınızı tutarlılığı
Seçtiğiniz veri değişiklikleri iyileştirmek için anahtarların etkileyen diğer önemli atomik bir işlem kullanarak tutarlılık sağlamak nasıl faktördür. Yalnızca bir EGT varlıklar aynı bölümde depolanır üzerinde çalışması için de kullanabilirsiniz.  

Makale şu desenlerden [tablo Tasarım desenleri](table-storage-design-patterns.md) tutarlılık yönetme adresi:  

* [İçi bölüm ikincil dizin düzeni](table-storage-design-patterns.md#intra-partition-secondary-index-pattern) -Store kullanarak her varlığın birden çok kopyalarını farklı **RowKey** değerlerini (aynı bölüme) etkinleştirmek hızlı ve verimli aramalar ve farklı kullanarak alternatif sıralama düzenleri **RowKey** değerleri.  
* [İkincil dizin arası bölüm düzeni](table-storage-design-patterns.md#inter-partition-secondary-index-pattern) - hızlı etkinleştirmek için ayrı bölümlerde veya farklı tablolarda farklı RowKey değerleri kullanarak her varlığın birden çok kopyasını Store ve verimli aramalar ve alternatif sıralama siparişleri farklı kullanarak**RowKey** değerleri.  
* [Son tutarlılık işlemleri deseni](table-storage-design-patterns.md#eventually-consistent-transactions-pattern) -etkinleştirme sonunda tutarlı bir davranış bölüm sınırları veya depolama sistemi sınırları arasında Azure kuyrukları kullanılarak.
* [Dizin varlıklarını düzeni](table-storage-design-patterns.md#index-entities-pattern) -varlıklar listesi döndüren verimli aramalar etkinleştirmek için dizin varlıklarını korumak.  
* [Normalleştirilmişlikten çıkarma deseni](table-storage-design-patterns.md#denormalization-pattern) -birleştirme ilgili verilerin birlikte tek nokta sorgu ile ihtiyacınız olan tüm verileri almanızı etkinleştirmek için tek bir varlık içinde.  
* [Veri serisi deseni](table-storage-design-patterns.md#data-series-pattern) -Store eksiksiz bir veri serisinde yaptığınız istek sayısını en aza indirmek için tek bir varlık.  

Varlık grubu işlemleri hakkında daha fazla bilgi için konudaki [varlık grubu işlemleri](table-storage-design.md#entity-group-transactions).  

## <a name="ensure-your-design-for-efficient-modifications-facilitates-efficient-queries"></a>Tasarımınızı etkili değişiklikler için verimli sorgular kolaylaştıran emin olun.
Bu durumda, belirli bir senaryoya yönelik olup olmadığını çoğu durumda, her zaman etkili değişiklikler, ancak verimli sorgulama sonuçları için bir tasarım değerlendirmelidir. Bazı makaledeki desenlerinin [tablo Tasarım desenleri](table-storage-design-patterns.md) açıkça sorgulama ve değiştirme varlıklar arasında denge değerlendirin ve her zaman her işlem türü sayısını dikkate almanız.  

Makale şu desenlerden [tablo Tasarım desenleri](table-storage-design-patterns.md) verimli sorgular için tasarlama ve verimli veri değişikliği için tasarlama arasında denge adresi:  

* [Bileşik anahtar deseni](table-storage-design-patterns.md#compound-key-pattern) -kullanım bileşik **RowKey** tek nokta sorgu ile ilgili verileri aramak bir istemci etkinleştirmek için değerler.  
* [Günlük kuyruğu deseni](table-storage-design-patterns.md#log-tail-pattern) -almak *n* varlıkları kullanarak bir bölüm için en son eklenen bir **RowKey** geriye doğru tarih ve saat sipariş sıralar değeri.  
