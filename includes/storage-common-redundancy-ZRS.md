---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 03/26/2018
ms.author: jeking
ms.custom: include file
ms.openlocfilehash: f6d437e4ad0e05d55c408ad8f9defe5385b52050
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
Bölge olarak yedekli depolama (ZRS) verilerinizi tek bir bölge içinde üç depolama kümeleri arasında zaman uyumlu olarak çoğaltır. Her Depolama kümesi diğerlerinden fiziksel olarak ayrılır ve kendi kullanılabilirlik bölgesinde (AZ) bulunur. Her kullanılabilirlik bölge, ZRS küme, içinde ise ayrı yardımcı programları ve ağ özellikleri otonom.

ZRS hesabında verilerinizi depolamak, mümkün erişimi olması ve bir bölge kullanılamaz hale gelmesi durumunda, verilerinizi yöneten sağlar. ZRS, mükemmel performans ve son derece düşük gecikme sağlar.

Kullanılabilirlik bölgeler hakkında daha fazla bilgi için bkz: [kullanılabilirlik bölgeleri genel bakış](https://docs.microsoft.com/azure/availability-zones/az-overview).

## <a name="zrs-for-high-availability"></a>Yüksek kullanılabilirlik için ZRS 

ZRS bir veri merkezinde bir kesinti veya doğal afet kullanılamaz işler olsa bile, güçlü tutarlılık, güçlü dayanıklılık ve yüksek kullanılabilirlik gerektiren senaryolar için göz önünde bulundurun. ZRS depolama nesneler en az %99.9999999999 için dayanıklılık sunar (12 9'in) belirli bir yılın üstünde.

ZRS şu anda standart, genel amaçlı v2 destekler (GPv2) hesap türleri. ZRS, blok blobları, disk olmayan sayfa BLOB'ları, dosyaları, tablolar ve Kuyruklar için kullanılabilir. 

ZRS şu bölgelerde genel olarak kullanılabilir:

- ABD Doğu 2
- ABD Orta
- Kuzey Avrupa
- Batı Avrupa
- Fransa Orta
- Güneydoğu Asya

Microsoft Azure diğer bölgelerdeki ZRS etkinleştirmek devam eder.

### <a name="what-happens-when-a-zone-becomes-unavailable"></a>Bir bölge kullanılamaz duruma geldiğinde ne olur?

Verilerinizi bir bölge kullanılamaz hale gelirse dayanıklı kalır. Microsoft, geçici hata işleme, yeniden deneme ilkelerini üstel geri alma ile uygulama gibi için uygulamaları izlemek devam önerir. Bir bölge kullanılamadığında Azure DNS repointing gibi ağ güncelleştirmeleri undertakes. Bunlar tamamladınız önce verilerinizi erişiyorsanız bu güncelleştirmeler, uygulamanızın etkileyebilir.

ZRS, verilerinizin nerede birden fazla bölge kalıcı olarak etkilenen bölgesel bir olağanüstü durum karşı koruma sağlamaz. Bunun yerine, ZRS geçici olarak kullanım dışı kalması durumunda, verileriniz için esneklik sunar. Bölgesel afetler karşı koruma için Microsoft önerir kullanarak [coğrafi olarak yedekli depolama (GRS): Azure Storage için çapraz bölge çoğaltma](../articles/storage/common/storage-redundancy-grs.md).

## <a name="zrs-classic-a-legacy-option-for-block-blobs-redundancy"></a>ZRS Klasik: Eski bir seçeneği blok blobları artıklığı
> [!NOTE]
> ZRS Klasik hesaplarını kullanımdan kaldırma ve 31 Mart 2021 üzerinde gerekli geçiş için planlanmıştır. Microsoft, kullanımdan önce ZRS Klasik müşteriler için daha fazla bilgi gönderir. Otomatik geçiş işlemi ZRS Klasik ZRS için gelecekte sağlamak Microsoft planlar.

ZRS Klasik yüklenebilir yalnızca blok bloblar genel amaçlı V1 (GPv1) depolama hesapları. ZRS Klasik zaman uyumsuz olarak veri bir veya iki bölgeleri içindeki veri merkezleri arasında çoğaltılır. Çoğaltma, Microsoft ikincil birime yük devretme işlemini başlatana kadar kullanılamayabilir. ZRS Klasik hesabı, LRS veya GRS’ye iki yönlü olarak dönüştürülemez ve ölçüm veya günlüğe kaydetme özelliklerine sahip değildir.

ZRS Klasik hesapları için veya LRS, GRS veya RA-GRS dönüştürülemez. ZRS Klasik hesapları, ölçümleri veya günlük kaydı da desteklemez.

ZRS bir bölgede genellikle kullanılabilir olduğunda, artık bu bölgede portalından ZRS klasik bir hesap oluşturmak mümkün olmayacaktır ancak biri arasında başka yollarla PowerShell'i, CLI gibi vb. oluşturabilirsiniz.

El ile ya da bir LRS, ZRS Klasik, GRS veya RA-GRS hesabı'ndan ZRS hesap verilerini geçirmek için AzCopy, Azure Storage Gezgini, Azure PowerShell veya Azure CLI kullanın. Kendi geçiş çözümünüzle birlikte Azure Storage istemci kitaplıklarından birini de oluşturabilirsiniz.