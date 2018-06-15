---
title: Azure dosyaları ölçeklenebilirlik ve performans hedefleri | Microsoft Docs
description: Kapasite, istek hızı ve gelen ve giden bant genişliği sınırları dahil olmak üzere Azure dosyaları için ölçeklenebilirlik ve performans hedefleri hakkında bilgi edinin.
services: storage
documentationcenter: na
author: wmgries
manager: aungoo
editor: tamram
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 12/04/2017
ms.author: wgries
ms.openlocfilehash: beb3e5caf8c8dce9b2ea06bbd0a2ea5a4e05a714
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34738083"
---
# <a name="azure-files-scalability-and-performance-targets"></a>Azure dosyaları ölçeklenebilirlik ve performans hedefleri
[Azure dosyaları](storage-files-introduction.md) tam olarak yönetilen dosya paylaşımları, endüstri standardı SMB protokolü erişilebilir bulutta sunar. Bu makalede, Azure dosyaları ve Azure dosya eşitleme (Önizleme) için ölçeklenebilirlik ve performans hedefleri anlatılmaktadır.

Burada listelenen ölçeklenebilirlik ve performans hedefleri Gelişmiş hedeflerine, ancak diğer değişkenleri dağıtımınızdaki etkilenebilir. Örneğin, bir dosya için işleme Ayrıca, kullanılabilir ağ bant genişliği yalnızca Azure dosyaları hizmetini barındıran sunucular sınırlı olabilir. Azure dosyaları performans ve ölçeklenebilirlik gereksinimlerinizi karşılayacak olup olmadığını belirlemek için kullanım şekillerini sınama öneririz. Biz de zaman içinde bu sınırları artırmak için çalışıyoruz. Lütfen görüşlerinizi bize bildirme, ya da altında veya üzerinde açıklamalarında çekinmeyin [Azure dosyaları UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files), hangi sınırları hakkında bize artırmak görmek istediğiniz.

## <a name="azure-storage-account-scale-targets"></a>Azure depolama hesabı ölçek hedefleri
Üst kaynak bir Azure dosya paylaşımı için bir Azure depolama hesabıdır. Bir depolama hesabı Azure dosyaları dahil olmak üzere birden çok depolama hizmetleri tarafından verileri depolamak için kullanılan Azure depolama havuzunu temsil eder. Depolama hesaplarında verilerini depolayan diğer Azure Blob storage, Azure kuyruk depolama ve Azure Table depolama hizmetleridir. Aşağıdaki hedefleri verileri bir depolama hesabına depolama tüm depolama hizmetleri Uygula:

[!INCLUDE [azure-storage-limits](../../../includes/azure-storage-limits.md)]

[!INCLUDE [azure-storage-limits-azure-resource-manager](../../../includes/azure-storage-limits-azure-resource-manager.md)]

> [!Important]  
> Depolama alanı hesabı kullanımı diğer depolama hizmetlerinden, Azure dosya paylaşımları depolama hesabınızdaki etkiler. Azure Blob storage ile en fazla depolama hesabı kapasitesi ulaştıysanız, Azure dosya paylaşımınıza en fazla paylaşım boyutunun altında olsa bile, örneğin, size, Azure Dosya paylaşımındaki yeni dosyalar oluşturamaz olmaz.

## <a name="azure-files-scale-targets"></a>Azure dosyaları ölçek hedefleri
[!INCLUDE [storage-files-scale-targets](../../../includes/storage-files-scale-targets.md)]

## <a name="azure-file-sync-scale-targets"></a>Azure dosya eşitleme ölçek hedefleri
Bu her zaman mümkün değildir ancak Azure dosya eşitleme ile biz sınırsız kullanım için tasarlamak mümkün olduğunca çalıştınız. Aşağıdaki tablo bizim sınırları ve hangi hedefleri gerçekte sabit kısıtlamalardır gösterir:

[!INCLUDE [storage-sync-files-scale-targets](../../../includes/storage-sync-files-scale-targets.md)]

### <a name="azure-file-sync-performance-metrics"></a>Azure dosya eşitleme performans ölçümleri
Azure dosya eşitleme için Azure dosya paylaşımları bağlanan bir Windows Server makinesinde Agent'in olduğundan, bir dizi etkene altyapınızdaki etkili eşitleme performans bağlıdır: Windows Server ve temel disk yapılandırması, ağ bant genişliği Sunucu ve Azure depolama, dosya boyutu, toplam veri kümesi boyutunu ve veri kümesi faaliyete arasında. Azure dosya eşitleme dosya düzeyinde çalışır olduğundan, Azure dosya eşitleme tabanlı bir çözüme performans özelliklerini daha iyi ölçülür saniyede işlenen nesneler (dosyalar ve dizinler) sayısı. 
 
Azure dosya eşitleme için performans iki aşamada önemlidir:
1. **İlk tek seferlik sağlama**: ilk sağlama üzerinde performansı iyileştirmek için başvurmak [Azure dosya eşitleme ile ekleme](storage-sync-files-deployment-guide.md#onboarding-with-azure-file-sync) en iyi dağıtım ayrıntıları için.
2. **Devam eden eşitleme**: verileri Azure dosya paylaşımları başlangıçta sağlanmış sonra Azure dosya eşitleme birden çok uç nokta eşitlenmiş tutar.

Dağıtımınız için her aşamaları planlamanıza yardımcı olması için aşağıdaki sonuçları iç bir sistemde bir yapılandırma ile test sırasında gözlemlenen
| Sistem yapılandırması |  |
|-|-|
| CPU | 64 MIB L3 Önbellek 64 sanal çekirdeğiyle |
| Bellek | 128 GiB |
| Disk | Önbellek pil ile SAS diskleri RAID 10 ile yedeklenir |
| Ağ | 1 GB/sn ağ |
| İş yükü | Genel amaçlı dosya sunucusu|

| İlk tek seferlik sağlama  |  |
|-|-|
| Nesne sayısı | 10 milyon nesneleri | 
| Veri kümesi boyutu| ~ 4 Tıb |
| Ortalama dosya boyutu | ~ 500 KiB (en büyük dosya: 100 Gib'den) |
| Üretilen iş karşıya yükle | saniye başına 15 nesneleri |
| Namespace indirme verimlilik * | saniye başına 350 nesneleri |
 
* Yeni bir sunucu uç noktası oluşturulduğunda, Azure dosya eşitleme Aracısı dosya içeriği indirmez. İlk kez bir tam ad alanını eşitlenir ve dosyalarını tamamen ya da karşıdan yüklemek için geri çağırma Tetikleyicileri arka plan veya katmanlama bulut üzerinde sunucu uç nokta bulut katmanlama ilkesini için etkinleştirilmiş.

| Devam eden eşitleme  |   |
|-|--|
| Eşitlenen nesne sayısı| 125,000 nesneleri (~ %1 dalgalanma) | 
| Veri kümesi boyutu| 50 Gib'den |
| Ortalama dosya boyutu | ~ 500 KiB (en büyük dosya: 100 Gib'den) |
| Üretilen iş karşıya yükle | saniye başına 20 nesneleri |
| Tam yükleme verimlilik * | saniyede 30 nesneleri |
 
Bulut katmanlandırma etkinleştirildiğinde, daha iyi performans verileri karşıdan dosya yalnızca bazıları olarak izlemek büyük olasılıkla. Bunlar herhangi bir uç noktaları üzerinde değiştirildiğinde azure dosya eşitleme yalnızca verileri önbelleğe alınan dosyaları indirir. Katmanlı ya da yeni oluşturulan tüm dosyalara Aracısı dosya verilerini indirmez ve bunun yerine tüm sunucu uç noktaları için ad alanı yalnızca eşitlenir. Kullanıcı tarafından erişilen gibi aracı katmanlı dosyaların kısmi yüklemeleri de destekler. 
 
> [!Note]  
> Yukarıdaki sayı, yaşar performans göstergesidir değil. Bu bölüm başına içinde özetlendiği gibi gerçek performans birden çok etkene bağlıdır.

Dağıtımınız için genel bir kılavuz olarak, birkaç aklınızda bulundurun:
- Nesne verimliliği yaklaşık sunucudaki eşitleme grupların sayısı orantılı olarak ölçeklendirir. Bir sunucuda birden çok eşitleme gruplar halinde veri bölme ayrıca ağ ve sunucu sınırlı daha iyi verimlilik sağlar.
- Nesne verimlilik, ikinci üretilen iş başına MIB inversely orantılıdır. Daha küçük dosyalar için ikinci ancak daha düşük MIB ikinci üretilen iş başına başına işlenen nesnelerin sayısı bakımından daha yüksek verimlilik karşılaşırsınız. Buna karşılık, daha büyük dosyalar için daha az nesne ikinci, ancak daha yüksek MIB ikinci üretilen iş başına başına işlenen alırsınız. İkinci üretilen iş başına MIB Azure dosyaları ölçek hedeflerini sınırlıdır. 

## <a name="see-also"></a>Ayrıca bkz.
- [Azure Dosyaları dağıtımını planlama](storage-files-planning.md)
- [Bir Azure dosya eşitleme dağıtımını planlama](storage-sync-files-planning.md)
- [Diğer depolama hizmetleri için ölçeklenebilirlik ve performans hedefleri](../common/storage-scalability-targets.md)