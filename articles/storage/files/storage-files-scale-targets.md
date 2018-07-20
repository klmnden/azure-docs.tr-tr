---
title: Azure dosyaları ölçeklenebilirlik ve performans hedefleri | Microsoft Docs
description: Kapasite, istek hızı ve gelen ve giden bant genişliği sınırlarını dahil olmak üzere Azure dosyaları için ölçeklenebilirlik ve performans hedefleri hakkında öğrenin.
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
ms.openlocfilehash: df777e6ebb8ed58fb820b13fb029ddc67775afa7
ms.sourcegitcommit: 727a0d5b3301fe20f20b7de698e5225633191b06
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2018
ms.locfileid: "39146213"
---
# <a name="azure-files-scalability-and-performance-targets"></a>Azure dosyaları ölçeklenebilirlik ve performans hedefleri
[Azure dosyaları](storage-files-introduction.md) tam olarak yönetilen dosya paylaşımları endüstri standardı SMB protokolünü erişilebilen bulutta sunar. Bu makalede, Azure dosyaları ve Azure dosya eşitleme için ölçeklenebilirlik ve performans hedefleri ele alır.

Burada listelenen ölçeklenebilirlik ve performans hedefleri olan çeşitli yüksek teknolojiye sahip hedefler, ancak diğer değişkenleri, dağıtımınızdaki etkilenebilir. Örneğin, bir dosya aktarım hızı da, kullanılabilir ağ bant genişliği ile yalnızca Azure dosyaları hizmeti barındıran sunucular sınırlı olabilir. Azure dosyaları performansını ve ölçeklenebilirliğini gereksinimlerinizi karşılayıp karşılamadığını belirlemek için kullanım şekillerini sınama önerilir. Biz de bu sınırların zaman içinde artmasına uygulanır. Lütfen görüşlerinizi bize bildirin, ya da altında veya üzerinde açıklamalarda başvurmaktan çekinmeyin [Azure dosyaları UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files), hangi sınırları hakkında bize artırmak görmek istediğiniz.

## <a name="azure-storage-account-scale-targets"></a>Azure depolama hesabı ölçek hedefleri
Azure depolama hesabınız bir Azure dosya paylaşımı için üst kaynaktır. Bir depolama hesabı, Azure dosyaları dahil olmak üzere birden çok depolama hizmeti tarafından verileri depolamak için kullanılan Azure depolama havuzunu temsil eder. Veri depolama hesaplarında depolayabilir diğer hizmetleri Azure Blob Depolama, Azure kuyruk depolama ve Azure tablo depolama tabidir. Tüm depolama hizmetlerine verileri bir depolama hesabında depolama aşağıdaki hedeflere Uygula:

[!INCLUDE [azure-storage-limits](../../../includes/azure-storage-limits.md)]

[!INCLUDE [azure-storage-limits-azure-resource-manager](../../../includes/azure-storage-limits-azure-resource-manager.md)]

> [!Important]  
> Depolama hesabı kullanımı diğer depolama hizmetlerinde, Azure dosya paylaşımlarını depolama hesabınızda etkiler. Azure Blob Depolama ile en fazla depolama hesabı kapasitesi ulaşırsa, en fazla paylaşım boyutunun altında Azure dosya paylaşımınızı olsa bile, örneğin, yeni dosyaları Azure dosya paylaşımınızı oluşturmak mümkün olmayacaktır.

## <a name="azure-files-scale-targets"></a>Azure dosyaları ölçek hedefleri
[!INCLUDE [storage-files-scale-targets](../../../includes/storage-files-scale-targets.md)]

## <a name="azure-file-sync-scale-targets"></a>Azure dosya eşitleme ölçek hedefleri
Bu her zaman mümkün değildir, ancak Azure dosya eşitleme ile sınırsız kullanım için tasarlamak mümkün olduğunca çalıştık. Aşağıdaki tabloda test işlemlerimizi sınırları ve hangi hedeflerin gerçekten sabit limitlerdir gösterir:

[!INCLUDE [storage-sync-files-scale-targets](../../../includes/storage-sync-files-scale-targets.md)]

### <a name="azure-file-sync-performance-metrics"></a>Azure dosya eşitleme performans ölçümleri
Azure dosya eşitleme aracısının Azure dosya paylaşımları için bağlanan bir Windows Server makinesi üzerinde çalıştığından, etkili eşitleme performansı altyapınızdaki bir dizi etkene bağlıdır: Windows Server ve temel alınan disk yapılandırması, ağ bant genişliği Sunucu ve Azure depolama, dosya boyutu, toplam veri kümesi boyutu ve bir veri kümesini etkinlik arasında. Azure dosya eşitleme dosya düzeyinde çalışır olduğundan, Azure dosya eşitleme tabanlı bir çözüm performans özelliklerini daha iyi ölçülür saniyede işlenen nesne (dosyalar ve dizinler) sayısı. 
 
Azure dosya eşitleme için performans iki aşamada önemlidir:
1. **İlk tek seferlik sağlama**: ilk sağlama üzerinde performansını iyileştirmek için başvurmak [Azure dosya eşitleme ile ekleme](storage-sync-files-deployment-guide.md#onboarding-with-azure-file-sync) için en uygun dağıtım ayrıntılarını.
2. **Devam eden eşitleme**: veri başlangıçta Azure dosya paylaşımlarını sağlanmış sonra Azure dosya eşitleme birden fazla uç noktası eşitlenmiş tutar.

Her aşamalar için dağıtım planlamanıza yardımcı olması için aşağıdaki sonuçları iç bir sistemde bir yapılandırma ile test sırasında gözetilir
| Sistem yapılandırması |  |
|-|-|
| CPU | 64 MiB L3 önbellek ile 64 sanal çekirdekler |
| Bellek | 128 GiB |
| Disk | Önbellek pil RAID 10 ile SAS diskleri desteklenir |
| Ağ | 1 GB/sn ağ |
| İş yükü | Genel amaçlı dosya sunucusu|

| İlk tek seferlik sağlama  |  |
|-|-|
| Nesne sayısı | 10 milyon nesneleri | 
| Veri kümesi boyutu| ~ 4 TiB |
| Ortalama dosya boyutu | Yaklaşık 500 KiB (en büyük dosyayı: 100 GiB) |
| Aktarım hızı karşıya yükleme | saniyede 15 nesneleri |
| Namespace indirme aktarım hızı * | saniyede 350 nesneleri |
 
* Yeni bir sunucu uç noktası oluşturulduğunda, Azure dosya eşitleme aracısının dosya içeriği indirmez. İlk tam ad alanı eşitler ve ardından Tetikleyiciler ya da tamamen dosyaları indirmek için geri çağırma arka plan veya Bulut katmanlaması sunucu uç noktasında ayarlayın bulut katmanlama ilkesi etkinleştirilir.

| Devam eden eşitleme  |   |
|-|--|
| Eşitlenen nesne sayısı| 125,000 nesneleri (yaklaşık %1 değişim sıklığı) | 
| Veri kümesi boyutu| 50 giB |
| Ortalama dosya boyutu | Yaklaşık 500 KiB |
| Aktarım hızı karşıya yükleme | saniye başına 20 nesneleri |
| Tam yükleme verimi * | saniyede 30 nesneleri |
 
* İse bulut katmanlama etkin olduğunda, muhtemelen yalnızca bazı veri karşıdan dosya olarak daha iyi performans gözlemleyin. Bunlar herhangi bir uç nokta değiştirildiğinde azure dosya eşitleme yalnızca verilerin önbelleğe alınmış dosyaları indirir. Aracı, tüm katmanlı veya yeni oluşturulan dosyalar için dosya verilerini indirmez ve bunun yerine, tüm sunucu uç noktaları için ad alanı yalnızca eşitleyebilir. Kullanıcı tarafından erişilen gibi aracı katmanlı dosyaların kısmi yüklemeleri de destekler. 
 
> [!Note]  
> Yukarıdaki sayılar, yaşar performansı değildir. Gerçek performans, bu bölümün başında belirtildiği gibi pek çok unsura bağlıdır.

Dağıtımınız için genel bir kılavuz olarak, bazı noktalar göz önünde tutmalısınız:
- Nesne aktarım hızı derlemekten sunucuda eşitleme grubu sayısı yaklaşık olarak ölçeklendirir. Bir sunucuda birden çok eşitleme gruplarına veri bölme, ağ ve sunucu tarafından da sınırlı daha iyi verim verir.
- Nesne işleme başına ikinci aktarım hızını MIB inversely orantılıdır. Daha küçük dosyalar için ikinci, ancak daha düşük MiB ikinci işleme başına başına işlenen nesnelerin sayısı bakımından daha yüksek iş hacmi karşılaşırsınız. Buna karşılık, daha büyük dosyalar için daha az nesne ikinci, ancak daha yüksek MiB ikinci işleme başına başına işlenen alırsınız. Azure dosyaları ölçeklendirme hedeflerini ikinci işleme başına MiB sınırlıdır. 

## <a name="see-also"></a>Ayrıca bkz.
- [Azure Dosyaları dağıtımını planlama](storage-files-planning.md)
- [Bir Azure dosya eşitleme dağıtımı planlama](storage-sync-files-planning.md)
- [Diğer depolama hizmetleri için ölçeklenebilirlik ve performans hedefleri](../common/storage-scalability-targets.md)
