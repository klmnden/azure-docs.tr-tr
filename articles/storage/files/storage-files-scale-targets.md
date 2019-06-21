---
title: Azure dosyaları ölçeklenebilirlik ve performans hedefleri | Microsoft Docs
description: Kapasite, istek hızı ve gelen ve giden bant genişliği sınırlarını dahil olmak üzere Azure dosyaları için ölçeklenebilirlik ve performans hedefleri hakkında öğrenin.
services: storage
author: roygara
ms.service: storage
ms.topic: article
ms.date: 5/5/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 5e2f7b52ca3cb8a4d472b353e844bdad3aa1b0c1
ms.sourcegitcommit: 6e6813f8e5fa1f6f4661a640a49dc4c864f8a6cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67150482"
---
# <a name="azure-files-scalability-and-performance-targets"></a>Azure dosyaları ölçeklenebilirlik ve performans hedefleri

[Azure dosyaları](storage-files-introduction.md) tam olarak yönetilen dosya paylaşımları endüstri standardı SMB protokolünü erişilebilen bulutta sunar. Bu makalede, Azure dosyaları ve Azure dosya eşitleme için ölçeklenebilirlik ve performans hedefleri ele alır.

Burada listelenen ölçeklenebilirlik ve performans hedefleri olan çeşitli yüksek teknolojiye sahip hedefler, ancak diğer değişkenleri, dağıtımınızdaki etkilenebilir. Örneğin, bir dosya aktarım hızı da, kullanılabilir ağ bant genişliği ile yalnızca Azure dosyaları hizmeti barındıran sunucular sınırlı olabilir. Azure dosyaları performansını ve ölçeklenebilirliğini gereksinimlerinizi karşılayıp karşılamadığını belirlemek için kullanım şekillerini sınama önerilir. Biz de bu sınırların zaman içinde artmasına uygulanır. Lütfen görüşlerinizi bize bildirin, ya da altında veya üzerinde açıklamalarda başvurmaktan çekinmeyin [Azure dosyaları UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files), hangi sınırları hakkında bize artırmak görmek istediğiniz.

## <a name="azure-storage-account-scale-targets"></a>Azure depolama hesabı ölçek hedefleri

Azure depolama hesabınız bir Azure dosya paylaşımı için üst kaynaktır. Bir depolama hesabı, Azure dosyaları dahil olmak üzere birden çok depolama hizmeti tarafından verileri depolamak için kullanılan Azure depolama havuzunu temsil eder. Veri depolama hesaplarında depolayabilir diğer hizmetleri Azure Blob Depolama, Azure kuyruk depolama ve Azure tablo depolama tabidir. Tüm depolama hizmetlerine verileri bir depolama hesabında depolama aşağıdaki hedeflere Uygula:

[!INCLUDE [azure-storage-limits](../../../includes/azure-storage-limits.md)]

[!INCLUDE [azure-storage-limits-azure-resource-manager](../../../includes/azure-storage-limits-azure-resource-manager.md)]

> [!Important]  
> Genel amaçlı depolama hesabı kullanımı diğer depolama hizmetlerinde, Azure dosya paylaşımlarını depolama hesabınızda etkiler. Azure Blob Depolama ile en fazla depolama hesabı kapasitesi ulaşırsa, en fazla paylaşım boyutunun altında Azure dosya paylaşımınızı olsa bile, örneğin, yeni dosyaları Azure dosya paylaşımınızı oluşturmak mümkün olmayacaktır.

## <a name="azure-files-scale-targets"></a>Azure dosyaları ölçek hedefleri

### <a name="premium-scale-targets"></a>Premium ölçek hedefleri

Premium dosya paylaşımları için dikkate alınması gereken sınırlamalar üç kategoriye ayrılır: depolama hesapları, paylaşımları ve dosyaları.

Örneğin: Tek bir paylaşım 100.000 IOPS değerlerine ulaşabilir ve tek bir dosyayı en fazla 5000 IOPS ölçeklendirme yapabilirsiniz. Bir paylaşımı üç dosyalarında varsa, bu nedenle, örneğin, bu paylaşımdan alabilirsiniz maksimum IOPS 15.000 olur.

### <a name="premium-filestorage-account-limits"></a>Premium dosya deposundan hesabı sınırları

Premium dosya paylaşımları olarak adlandırılan bir özel depolama hesabında sağlanan **dosya (Önizleme) deposundan**. Bu hesap, standart dosya paylaşımları için kullanılan depolama hesabından biraz farklı ölçek hedefi vardır. Depolama hesabı ölçek hedefleri için tablosuna başvuran [Azure depolama hesabı ölçek hedefleri](#azure-storage-account-scale-targets) bölümü.

> [!IMPORTANT]
> Depolama hesabı sınırları, tüm paylaşımlar için geçerlidir. Kadar ölçeklendirme en yüksek dosya deposundan hesapları için yalnızca dosya deposundan hesap başına yalnızca bir paylaşım ise ulaşılabilir eşittir.

### <a name="file-share-and-file-scale-targets"></a>Dosya Paylaşımı ve dosya ölçek hedefleri

> [!NOTE]
> Standart dosya paylaşımları 5 TiB daha büyük önizleme aşamasındadır ve belirli sınırlamaları vardır.
> Bir liste kısıtlamaları ve bu daha büyük bir dosya paylaşımı boyutları Önizleme ekleme için bkz [standart dosya paylaşımları](storage-files-planning.md#standard-file-shares) Planlama Kılavuzu'nun bölümü.

[!INCLUDE [storage-files-scale-targets](../../../includes/storage-files-scale-targets.md)]

[!INCLUDE [storage-files-premium-scale-targets](../../../includes/storage-files-premium-scale-targets.md)]

## <a name="azure-file-sync-scale-targets"></a>Azure dosya eşitleme ölçek hedefleri

Azure dosya eşitleme ile sınırsız kullanım amacı tasarlanmıştır ancak sınırsız kullanım her zaman mümkün değildir. Aşağıdaki tabloda, Microsoft'un test sınırları gösterir ve de hangi hedeflerin sabit limitlerdir gösterir:

[!INCLUDE [storage-sync-files-scale-targets](../../../includes/storage-sync-files-scale-targets.md)]

### <a name="azure-file-sync-performance-metrics"></a>Azure dosya eşitleme performans ölçümleri

Azure dosya eşitleme aracısının Azure dosya paylaşımları için bağlanan bir Windows Server makinesi üzerinde çalıştığından, etkili eşitleme performansı altyapınızdaki bir dizi etkene bağlıdır: Windows Server ve temel alınan disk yapılandırmasını, sunucu ve Azure depolama arasındaki ağ bant genişliği veri kümesinde dosya boyutu, toplam veri kümesi boyutu ve etkinlik. Azure dosya eşitleme dosya düzeyinde çalışır olduğundan, Azure dosya eşitleme tabanlı bir çözüm performans özelliklerini daha iyi ölçülür saniyede işlenen nesne (dosyalar ve dizinler) sayısı.

Azure dosya eşitleme için performans iki aşamada önemlidir:

1. **İlk tek seferlik sağlama**: İlk sağlama üzerinde performansını iyileştirmek için başvurmak [Azure dosya eşitleme ile ekleme](storage-sync-files-deployment-guide.md#onboarding-with-azure-file-sync) için en uygun dağıtım ayrıntıları.
2. **Devam eden eşitleme**: Azure dosya eşitleme verileri başlangıçta Azure dosya paylaşımlarını sağlanmış sonra birden fazla uç noktası uyumlu kalmasını sağlar.

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
| Nesne sayısı | 25 milyon nesneleri |
| Veri kümesi boyutu| ~4.7 TiB |
| Ortalama dosya boyutu | Yaklaşık 200 KiB (en büyük dosya: 100 giB) |
| Aktarım hızı karşıya yükleme | saniye başına 20 nesneleri |
| Namespace indirme aktarım hızı * | Saniyede 400 nesneleri |

\* Yeni bir sunucu uç noktası oluşturulduğunda, Azure dosya eşitleme aracısının dosya içeriği indirmez. İlk tam ad alanı eşitler ve ardından Tetikleyiciler ya da tamamen dosyaları indirmek için geri çağırma arka plan veya Bulut katmanlaması sunucu uç noktasında ayarlayın bulut katmanlama ilkesi etkinleştirilir.

| Devam eden eşitleme  |   |
|-|--|
| Eşitlenen nesne sayısı| 125,000 nesneleri (yaklaşık %1 değişim sıklığı) |
| Veri kümesi boyutu| 50 giB |
| Ortalama dosya boyutu | Yaklaşık 500 KiB |
| Aktarım hızı karşıya yükleme | saniyede 30 nesneleri |
| Tam yükleme verimi * | saniyede 60 nesneleri |

\* İse bulut katmanlama etkin olduğunda, muhtemelen yalnızca bazı veri karşıdan dosya olarak daha iyi performans gözlemleyin. Bunlar herhangi bir uç nokta değiştirildiğinde azure dosya eşitleme yalnızca verilerin önbelleğe alınmış dosyaları indirir. Aracı, tüm katmanlı veya yeni oluşturulan dosyalar için dosya verilerini indirmez ve bunun yerine, tüm sunucu uç noktaları için ad alanı yalnızca eşitleyebilir. Kullanıcı tarafından erişilen gibi aracı katmanlı dosyaların kısmi yüklemeleri de destekler. 

> [!Note]  
> Yukarıdaki sayılar, yaşar performansı değildir. Gerçek performans, bu bölümün başında belirtildiği gibi pek çok unsura bağlıdır.

Dağıtımınız için genel bir kılavuz olarak, bazı noktalar göz önünde tutmalısınız:

- Nesne aktarım hızı derlemekten sunucuda eşitleme grubu sayısı yaklaşık olarak ölçeklendirir. Bir sunucuda birden çok eşitleme gruplarına veri bölme, ağ ve sunucu tarafından da sınırlı daha iyi verim verir.
- Nesne işleme başına ikinci aktarım hızını MIB inversely orantılıdır. Daha küçük dosyalar için ikinci, ancak daha düşük MiB ikinci işleme başına başına işlenen nesnelerin sayısı bakımından daha yüksek iş hacmi karşılaşırsınız. Buna karşılık, daha büyük dosyalar için daha az nesne ikinci, ancak daha yüksek MiB ikinci işleme başına başına işlenen alırsınız. Azure dosyaları ölçeklendirme hedeflerini ikinci işleme başına MiB sınırlıdır.

## <a name="see-also"></a>Ayrıca bkz.

- [Azure Dosyaları dağıtımını planlama](storage-files-planning.md)
- [Bir Azure dosya eşitleme dağıtımı planlama](storage-sync-files-planning.md)
- [Diğer depolama hizmetleri için ölçeklenebilirlik ve performans hedefleri](../common/storage-scalability-targets.md)
