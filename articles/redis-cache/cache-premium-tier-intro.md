---
title: Azure Redis önbelleği Premium katmanına giriş | Microsoft Docs
description: Oluşturma ve Redis kalıcılığı yönetmek, kümeleme Redis ve Premium katmanı Azure Redis önbelleği örnekleri için VNET destek öğrenin
services: redis-cache
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
ms.assetid: 30f46f9f-e6ec-4c38-a8cc-f9d4444856e5
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: wesmc
ms.openlocfilehash: 38a43756678a3628040b1b995966eff6dd9fb363
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
ms.locfileid: "27911212"
---
# <a name="introduction-to-the-azure-redis-cache-premium-tier"></a>Azure Redis Önbelleği Premium katmanına giriş
Azure Redis önbelleği verilerinizi Süper hızlı erişim sağlayarak yüksek oranda ölçeklenebilir ve esnek uygulamalar oluşturmanıza yardımcı olan dağıtılmış, yönetilen bir önbelleğidir. 

Yeni Premium katmanı tüm standart katman özellikleri ve daha iyi performans, daha büyük iş yükleri, olağanüstü durum kurtarma, içeri/dışarı aktarma ve Gelişmiş güvenlik gibi daha fazlasını içeren bir kuruluş hazır Katmanı ' dir. Premium önbellek katmanının ek özellikler hakkında daha fazla bilgi için okumaya devam edin.

## <a name="better-performance-compared-to-standard-or-basic-tier"></a>Standart ya da temel katmana göre daha iyi performans
**Performans katmanı standart üzerinden veya temel daha iyi.** Premium katmanındaki önbellekleri daha hızlı bir işlemciye sahip ve temel veya standart katman karşılaştırıldığında daha iyi performans sunan donanımda dağıtılır. Premium katmanı önbellekleri daha yüksek verimlilik ve düşük gecikme vardır. 

**Standart katmanı karşılaştırıldığında Premium'da aynı boyutta önbelleği için işleme yüksektir.** Örneğin, 53 GB'a P4 üretimini (Premium) önbelleğidir C6 150 K karşılaştırıldığında saniyede 250K istekler (standart).

Boyut, işleme ve premium önbelleklere sahip bant genişliği hakkında daha fazla bilgi için bkz: [Azure Redis önbelleği SSS](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)

## <a name="redis-data-persistence"></a>Redis veri kalıcılığı
Premium katmanı, bir Azure depolama hesabındaki önbellek verilerini sürdürülmesi olanak sağlar. Basic/standart önbellekteki tüm verileri yalnızca bellekte depolanır. Altyapının durumunda sorunlar var. olası veri kaybı olabilir. Redis veri kalıcılığını özelliği, veri kaybına karşı dayanıklılığı artırmak için Premium katmanındaki kullanmanızı öneririz. Azure Redis önbelleği seçeneklerinde RDB ve (yakında) AOF sunar [Redis kalıcılığı](http://redis.io/topics/persistence). 

Kalıcılığın yapılandırılması hakkında yönergeler için bkz. [Premium Azure Redis Cache için kalıcılığı yapılandırma](cache-how-to-premium-persistence.md).

## <a name="redis-cluster"></a>Redis kümesi
Önbellekleri 53 GB'den büyük oluşturmak veya birden çok Redis düğümünde parça verileri istediğiniz istiyorsanız, Premium katmanında kullanılabilir olduğu kümeleme Redis kullanabilirsiniz. Her düğüm, yüksek kullanılabilirlik için Azure tarafından yönetilen bir birincil/çoğaltma önbelleği çifti oluşur. 

**Redis kümeleme en fazla ölçek ve verimlilik sağlar.** Kümedeki parça (düğümlerin) sayısını artırmak üretilen işi doğrusal olarak artar. Eg. 10 parça P4 kümesi oluşturun, sonra kullanılabilir verimlilik 250 K'dır, * 10 saniye başına 2,5 milyon istek =. Lütfen bakın [Azure Redis önbelleği SSS](cache-faq.md#what-redis-cache-offering-and-size-should-i-use) boyut, işleme ve premium önbelleklere sahip bant genişliği hakkında daha fazla ayrıntı için.

Kümeleme ile çalışmaya başlamak için bkz: [Premium Azure Redis önbelleği için kümeleri yapılandırma](cache-how-to-premium-clustering.md).

## <a name="enhanced-security-and-isolation"></a>Geliştirilmiş güvenlik ve yalıtım
Temel veya standart katmanında oluşturduğunuzu önbellekleri ortak Internet üzerinden erişilebilir. Önbelleğe erişim erişim anahtarı temel sınırlıdır. Premium katmanı ile daha fazla yalnızca belirtilen ağ istemcileri önbelleğe erişebilir emin olabilirsiniz. Redis Önbelleği'nde dağıtabileceğiniz bir [Azure sanal ağ (VNet)](https://azure.microsoft.com/services/virtual-network/). Redis’e erişimi daha da fazla kısıtlamak için alt ağlar, erişim denetimi, ilkeler gibi VNet’in tüm özelliklerini ve diğer özellikleri kullanabilirsiniz.

Daha fazla bilgi için bkz. [Premium Azure Redis Cache için Sanal Ağ desteğini yapılandırma](cache-how-to-premium-vnet.md).

## <a name="importexport"></a>İçeri/Dışarı Aktarma
İçeri/dışarı aktarma veri Azure Redis önbelleğine alma veya içeri aktarma ve Redis önbelleği veritabanı'nı (RDB) anlık görüntü premium önbellekten bir Azure sayfa blobu verme Azure Redis Önbelleği'nden veri verme olanak tanıyan bir Azure Redis önbelleği veri yönetimi işlemdir Depolama hesabı. Bu, farklı Azure Redis önbelleği örnekleri arasında geçirmek veya önbellek kullanmadan önce verileri ile doldurmak sağlar.

İçeri aktarma, herhangi bir bulut veya ortamını Linux, Windows ya da herhangi bir bulut sağlayıcısına Amazon Web Hizmetleri ve diğerleri gibi çalışan Redis çalıştıran herhangi bir Redis sunucudan Redis uyumlu RDB dosyaları getirmek için kullanılabilir. Veri alma, önceden doldurulmuş haldedir verilerle önbellek oluşturmak için kolay bir yoludur. İçeri aktarma işlemi sırasında Azure Redis önbelleği RDB dosyaları Azure Storage'dan belleğe yükler ve ardından anahtarları önbelleğe ekler.

Dışarı aktarma, Azure Redis uyumlu RDB dosyaları Redis için önbellekte depolanan veriler vermenize olanak sağlar. Verileri bir Azure Redis önbelleği örneğinden diğerine veya başka bir Redis sunucuya taşımak için bu özelliği kullanın. Dışa aktarma işlemi sırasında geçici bir dosya Azure Redis önbelleği sunucu örneğini barındıran VM oluşturulur ve dosya belirtilen depolama hesabına yüklenir. Ya da durumunu başarı veya hata ile dışarı aktarma işlemi tamamlandıktan sonra geçici dosya silindi.

Daha fazla bilgi için bkz: [içine veriler aktarmak ve Azure Redis Önbelleği'nden veri vermek nasıl](cache-how-to-import-export-data.md).

## <a name="reboot"></a>Yeniden başlatma
Premium katmanı, önbellek isteğe bağlı bir veya daha fazla düğümlerinin yeniden başlatılmasını sağlar. Bu, bir hata oluşması durumunda esneklik için uygulamanızı test etmek sağlar. Aşağıdaki düğümler yeniden başlatabilirsiniz.

* Önbelleğinizi ana düğümünün
* Önbelleğinizi ikincil düğümü
* Önbelleğinizi hem ana hem de ikincil düğümlerinin
* Premium önbelleği Kümeleme ile kullanırken, ana, ikincil veya tek tek parça önbelleğinde için her iki düğüm yeniden başlatılabilir

Daha fazla bilgi için bkz: [yeniden](cache-administration.md#reboot) ve [yeniden SSS](cache-administration.md#reboot-faq).

>[!NOTE]
>Yeniden başlatma işlevselliği için tüm Azure Redis önbelleği katmanları şimdi etkinleştirildi.
>
>

## <a name="schedule-updates"></a>Güncelleştirmeleri zamanlama
Zamanlanmış güncelleştirmeler özelliği önbelleğiniz için bir bakım penceresi tanımlamanızı sağlar. Bakım penceresi belirtildiğinde, herhangi bir Redis sunucu güncelleştirme sırasında bu pencereyi yapılır. Pencere bir bakım penceresi belirlemek, istediğiniz günleri seçin ve Bakım belirtmek için her gün saat başlatın. Bakım penceresi saati UTC biçiminde olduğunu unutmayın. 

Daha fazla bilgi için bkz: [zamanlama güncelleştirmeleri](cache-administration.md#schedule-updates) ve [SSS güncelleştirmeleri zamanla](cache-administration.md#schedule-updates-faq).

> [!NOTE]
> Yalnızca sunucu zamanlanmış bakım penceresi sırasında yapılan güncelleştirmeler Redis. Bakım penceresi uygulanmaz Azure güncelleştirmeleri veya VM işletim sisteminde güncelleştirmeler.
> 
> 

## <a name="geo-replication"></a>Coğrafi çoğaltma

**Coğrafi çoğaltma** iki Premium katmanı Azure Redis önbelleği örnekleri bağlama için bir mekanizma sağlar. Bir önbellek birincil bağlantılı önbellek ve diğer ikincil bağlantılı önbelleği olarak atanır. İkincil bağlantılı önbellek salt okunur hale gelir ve birincil önbelleğe yazılan veri ikincil bağlantılı önbelleğine çoğaltılır. Bu işlev, Azure bölgeler arasında bir önbellek çoğaltmak için kullanılabilir.

Daha fazla bilgi için bkz: [Azure Redis önbelleği için coğrafi çoğaltma yapılandırma](cache-how-to-geo-replication.md).


## <a name="to-scale-to-the-premium-tier"></a>Premium katmanı için
Premium katmanı için bir premium katmanı seçmeniz yeterlidir **fiyatlandırma katmanı değişikliği** dikey. Ayrıca PowerShell ve CLI kullanarak premium katmanına önbelleğiniz ölçeklendirebilirsiniz. Adım adım yönergeler için bkz: [ölçek Azure Redis önbelleği nasıl](cache-how-to-scale.md) ve [bir ölçeklendirme işlemi otomatikleştirmek nasıl](cache-how-to-scale.md#how-to-automate-a-scaling-operation).

## <a name="next-steps"></a>Sonraki adımlar
Önbellek oluşturmak ve yeni premium katmanı özellikleri keşfedin.

* [Premium Azure Redis Cache için kalıcılığı yapılandırma](cache-how-to-premium-persistence.md)
* [Premium Azure Redis Cache için Sanal Ağ desteğini yapılandırma](cache-how-to-premium-vnet.md)
* [Premium Azure Redis Cache için kümeleri yapılandırma](cache-how-to-premium-clustering.md)
* [Azure Redis önbelleği içine veri içeri aktar ve dışarı aktarma veri nasıl](cache-how-to-import-export-data.md)
* [Azure Redis önbelleği yönetme](cache-administration.md)

