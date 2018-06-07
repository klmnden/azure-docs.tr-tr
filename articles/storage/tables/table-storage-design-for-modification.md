---
title: Azure depolama tabloları veri değişikliği için Tasarım | Microsoft Docs
description: Azure table depolama veri değişikliği için tabloları tasarlayın.
services: storage
documentationcenter: na
author: MarkMcGeeAtAquent
manager: kfile
ms.assetid: 8e228b0c-2998-4462-8101-9f16517393ca
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/23/2018
ms.author: sngun
ms.openlocfilehash: 6c008175f01521ce4f96d13e58244dc72d9f6990
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34661060"
---
# <a name="design-for-data-modification"></a>Veri değişikliği için Tasarım
Bu makalede ekler, güncelleştirmeleri, en iyi duruma getirme için tasarım konuları odaklanır ve siler. Bazı durumlarda (Tasarım dengelemeler yönetmek için kullanılan teknikleri ilişkisel bir veritabanında farklı olmasına rağmen) tasarımlarına ilişkisel veritabanları için yaptığınız gibi veri değişikliği için en iyi duruma getirme tasarımlarını karşı sorgulamak için en iyi duruma getirme tasarımları arasındaki dengelemeyi değerlendirmek gerekir. Bölüm [tablo Tasarım desenleri](#table-design-patterns) tablo hizmeti için bazı ayrıntılı tasarım desenleri açıklar ve bazı bu dengelemeler vurgular. Uygulamada, varlıkları iyi değiştirmek için varlıkları sorgulamak için en iyi duruma getirilmiş çoğu tasarımları da iş bulacaksınız.  

## <a name="optimize-the-performance-of-insert-update-and-delete-operations"></a>Ekleme, güncelleştirme ve silme işlemlerinin performansını en iyi duruma getirme
Güncelleştirmek veya bir varlığı silmek için kullanarak tanımlayabilir olmalıdır **PartitionKey** ve **RowKey** değerleri. Bu açısından, tercih ettiğiniz **PartitionKey** ve **RowKey** varlıklar değiştirme benzer ölçütleri varlıkları mümkün olduğunca verimli bir şekilde tanımlamak istediğiniz için noktası sorguları desteklemek için tercih ettiğiniz için izlemeniz gereken için. Verimsiz bir bölüm veya tablo taraması bulmak üzere bir varlık bulmak için kullanmak istediğiniz değil **PartitionKey** ve **RowKey** değerleri güncelleştirmek veya silmek için gerekir.  

Bölümünde aşağıdaki desenleri [tablo Tasarım desenleri](#table-design-patterns) adres iyileştirme performans veya ekleme, güncelleştirme ve silme işlemleri:  

* [Yüksek hacimli silme düzeni](table-storage-design-patterns.md#high-volume-delete-pattern) -kendi ayrı tabloda eşzamanlı silme işlemi için tüm varlıkları depolayarak varlıklar yüksek hacimli silinmesini sağlar; tablo silerek varlıklarını silme.  
* [Veri serisi deseni](table-storage-design-patterns.md#data-series-pattern) -deposu tam veri serisi içindeki yaptığınız istek sayısını en aza indirmek için tek bir varlık.  
* [Geniş varlıklar düzeni](table-storage-design-patterns.md#wide-entities-pattern) -birden çok 252 özellik sahip mantıksal varlık depolamak için birden çok fiziksel varlık kullanın.  
* [Büyük varlıklar düzeni](table-storage-design-patterns.md#large-entities-pattern) -büyük özellik değerlerini depolamak için blob depolama kullanın.  

## <a name="ensure-consistency-in-your-stored-entities"></a>İçinde depolanan varlıklarınızı tutarlılığı
Veri değişiklikleri iyileştirmek için anahtarların tercih ettiğiniz etkileyen diğer anahtar atomik işlemleri kullanarak tutarlılığını sağlamak nasıl faktördür. Yalnızca bir EGT aynı bölümünde depolanan varlıklar üzerinde çalışması için de kullanabilirsiniz.  

Makalede aşağıdaki desenleri [tablo Tasarım desenleri](table-storage-design-patterns.md) tutarlılık yönetme adresi:  

* [İçi bölüm ikincil dizin düzeni](table-storage-design-patterns.md#intra-partition-secondary-index-pattern) -her varlık kullanan birden çok kopyalarını farklı depolama **RowKey** değerlerini (aynı bölüm) etkinleştirmek hızlı ve verimli aramalar ve farklı kullanarak diğer sıralamalar **RowKey** değerleri.  
* [İkincil dizin arası bölüm düzeni](table-storage-design-patterns.md#inter-partition-secondary-index-pattern) - hızlı etkinleştirmek için ayrı bölümlere veya ayrı tablolarda farklı RowKey değerleri kullanarak her bir varlık birden çok kopyasını depolamak ve verimli aramaları ve diğer sıralama siparişleri farklı kullanarak**RowKey** değerleri.  
* [Sonuçta tutarlı işlemleri düzeni](table-storage-design-patterns.md#eventually-consistent-transactions-pattern) -etkinleştirmek sonuçta tutarlı davranış bölüm sınırları veya depolama sistemi sınırları boyunca Azure sıraları kullanarak.
* [Dizin varlıkları düzeni](table-storage-design-patterns.md#index-entities-pattern) -varlıklar listesi Döndür verimli aramalar etkinleştirmek için dizin varlıkları korumak.  
* [Denormalization deseni](table-storage-design-patterns.md#denormalization-pattern) -birleştirme ilgili verileri birlikte tek nokta sorguyla gereken tüm verileri almak üzere etkinleştirmek için tek bir varlık olarak.  
* [Veri serisi deseni](table-storage-design-patterns.md#data-series-pattern) -deposu tam veri serisi içindeki yaptığınız istek sayısını en aza indirmek için tek bir varlık.  

Varlık Grup işlemler hakkında daha fazla bilgi için bkz [varlık Grup hareketleri](table-storage-design.md#entity-group-transactions).  

## <a name="ensure-your-design-for-efficient-modifications-facilitates-efficient-queries"></a>Tasarımınız etkili değişiklikler için verimli sorguları kolaylaştıran emin olun
Çoğu durumda, bunun belirli senaryonuz için geçerli olup olmadığını tasarım etkili değişiklikler, ancak sorgulama verimli sonuçlar için her zaman değerlendirmelisiniz. Makalede desenleri bazıları [tablo Tasarım desenleri](table-storage-design-patterns.md) açıkça sorgulama ve varlıkları değiştirme arasındaki dengelemeler değerlendirin ve her zaman her türden işlem sayısını dikkate almanız.  

Makalede aşağıdaki desenleri [tablo Tasarım desenleri](table-storage-design-patterns.md) adres için verimli sorguları tasarlama ve verimli veri değişikliği için tasarlama arasındaki dengelemeler:  

* [Bileşik anahtar düzeni](table-storage-design-patterns.md#compound-key-pattern) -kullanım bileşik **RowKey** tek nokta sorgu ile ilgili veri aramak bir istemci etkinleştirmek için değerleri.  
* [Günlük tail düzeni](table-storage-design-patterns.md#log-tail-pattern) -almak *n* kullanılarak bir bölüm için en son eklenen varlıklar bir **RowKey** ters tarihi ve saati sipariş sıralar değeri.  
