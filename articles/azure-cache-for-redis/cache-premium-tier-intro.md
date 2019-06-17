---
title: Azure önbelleği için Redis Premium katmanına giriş | Microsoft Docs
description: Oluşturma ve yönetme Redis kalıcılığı, Redis Kümeleme ve sanal ağ desteği, Azure Cache Premium katmanı Redis örneği için bilgi edinin
services: cache
documentationcenter: ''
author: yegu-ms
manager: jhubbard
editor: ''
ms.assetid: 30f46f9f-e6ec-4c38-a8cc-f9d4444856e5
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: yegu
ms.openlocfilehash: 6960c21091e0bc01c198e713c0c276984566ac41
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65786074"
---
# <a name="introduction-to-the-azure-cache-for-redis-premium-tier"></a>Azure önbelleği için Redis Premium katmanına giriş
Redis için Azure Cache, verilerinize çok hızlı erişim sağlayarak üst düzeyde ölçeklenebilir ve hızlı yanıt veren uygulamalar geliştirmenize yardımcı olan dağıtılmış, yönetilen bir önbellektir. 

Yeni Premium katmanı, tüm standart katman özellikleri ve daha iyi performans, daha büyük iş yükleri, olağanüstü durum kurtarma, içeri/dışarı aktarma ve Gelişmiş güvenlik gibi ek içeren bir kurumsal kullanıma hazır katman içindir. Premium önbellek katmanı, ek özellikler hakkında daha fazla bilgi edinmek için okumaya devam edin.

## <a name="better-performance-compared-to-standard-or-basic-tier"></a>Standart veya temel katmana kıyasla daha iyi performans
**Performans katmanı standart veya temel daha iyi.** Premium katmanda önbellekler, daha hızlı işlemcilere sahiptir ve temel veya standart katmana kıyasla daha iyi bir performans sunan bir donanım üzerinde dağıtılır. Premium katman Önbelleklerinden daha yüksek aktarım hızı ve düşük gecikme sürelerine sahip. 

**Aktarım hızı aynı boyutta önbelleği için standart katmana göre Premium'da yüksektir.** Örneğin, aktarım hızı bir 53 GB P4 (Premium) önbellektir C6 150 K karşılaştırıldığında saniye başına 250K istek (Standard).

Boyut, işleme ve premium önbelleklere sahip bant genişliği hakkında daha fazla bilgi için bkz. [Azure önbelleği için Redis SSS](cache-faq.md#what-azure-cache-for-redis-offering-and-size-should-i-use)

## <a name="redis-data-persistence"></a>Redis veri kalıcılığı
Premium katman, kalıcı önbellek verileri bir Azure depolama hesabı olanak tanır. Temel/standart önbellekte tüm verileri yalnızca bellekte depolanır. Altyapının olması durumunda olası veri kaybı sorunları var olabilir. Veri kaybına karşı dayanıklılığı artırmak için Premium katmandaki Redis veri dayanıklılığı özelliğinin kullanılmasını öneririz. Azure önbelleği için Redis RDB ve (çok yakında) AOF seçenekleri sunar [Redis kalıcılığı](https://redis.io/topics/persistence). 

Kalıcılığın yapılandırılması hakkında yönergeler için bkz: [Redis için bir Premium Azure Cache için kalıcılığı yapılandırma](cache-how-to-premium-persistence.md).

## <a name="redis-cluster"></a>Redis kümesi
Önbellekler 53 GB'den büyük oluşturabilir veya birden çok Redis düğümü arasında parça verileri istediğiniz istiyorsanız, Redis kümeleme Premium katmanda kullanılabilir kullanabilirsiniz. Her düğüm, yüksek kullanılabilirlik için Azure tarafından yönetilen bir birincil/çoğaltma önbellek çifti oluşur. 

**Redis kümeleme maksimum ölçek ve aktarım hızı sağlar.** Kümedeki parça (düğümler) sayısı arttıkça aktarım hızı doğrusal olarak artar. Örn. 10 parça P4 küme oluşturma sonra kullanılabilir aktarım hızı olan 250 bin * 10 = saniye başına 2,5 milyon istek. Lütfen [Azure önbelleği için Redis SSS](cache-faq.md#what-azure-cache-for-redis-offering-and-size-should-i-use) boyutu, aktarım hızı ve premium önbelleklere sahip bant genişliği hakkında daha fazla ayrıntı için.

Kümeleme ile çalışmaya başlamak için bkz. [Redis için Premium Azure Cache için kümeleri yapılandırma](cache-how-to-premium-clustering.md).

## <a name="enhanced-security-and-isolation"></a>Geliştirilmiş güvenlik ve yalıtım
Temel veya standart katmanında oluşturduğunuzu önbellekler, genel internet'te erişilebilir. Erişim anahtarını temel alan bir önbelleğe erişim sınırlıdır. Premium katmanı ile daha da yalnızca belirtilen bir ağdaki istemcilerin önbelleğe erişebilir emin olabilirsiniz. Azure Cache Redis için dağıtabileceğiniz bir [Azure sanal ağı (VNet)](https://azure.microsoft.com/services/virtual-network/). Redis’e erişimi daha da fazla kısıtlamak için alt ağlar, erişim denetimi, ilkeler gibi VNet’in tüm özelliklerini ve diğer özellikleri kullanabilirsiniz.

Daha fazla bilgi için [Redis için bir Premium Azure Cache için sanal ağ desteğini yapılandırma](cache-how-to-premium-vnet.md).

## <a name="importexport"></a>İçeri/Dışarı Aktarma
İçeri/dışarı aktarma olan Azure önbelleği için Redis içine veri aktarmak veya Azure önbelleği için Redis içeri ve dışarı aktarma bir Azure önbelleği için Redis veritabanı (RDB) anlık görüntü için bir premium önbelleğinden tarafından verileri dışarı aktarma olanak tanıyan Redis veri yönetimi işlemi için bir Azure önbelleği bir bir Azure depolama hesabındaki sayfa blobu. Bu, farklı Azure önbelleği için Redis örneği arasında geçirmek veya önbellek kullanılmadan önce veri ile doldurmak sağlar.

İçeri aktarma, herhangi bir bulut veya ortam, Linux, Windows ya da herhangi bir bulut sağlayıcısı Amazon Web Hizmetleri ve diğerleri gibi çalışan bir Redis dahil olmak üzere çalışan herhangi bir Redis sunucudan Redis uyumlu RDB dosyaları getirmek için kullanılabilir. Verileri içeri aktarma ile önceden doldurulmuş veri önbellek oluşturmak için kolay bir yoludur. İçeri aktarma işlemi sırasında Azure önbelleği için Redis RDB dosyaları Azure Storage'dan belleğine yükler ve ardından anahtarları önbelleğe ekler.

Dışarı aktarma, Redis için Redis uyumlu RDB dosyaları Azure önbelleğinde depolanan verileri dışarı aktarma olanak tanır. Bu özellik, verileri bir Azure önbelleği için Redis örneğinden diğerine veya başka bir Redis sunucusuna taşıma için kullanabilirsiniz. Dışarı aktarma işlemi sırasında Azure önbelleği için Redis sunucusu örneğini barındıran sanal makine geçici bir dosya oluşturulur ve dosyanın belirtilen depolama hesabına yüklenir. Ya da bir durum başarı veya hata ile dışarı aktarma işlemi tamamlandığında, geçici dosya silinir.

Daha fazla bilgi için [verileri içeri aktarma ve verileri Azure önbelleği için Redis dışarı aktarma](cache-how-to-import-export-data.md).

## <a name="reboot"></a>Yeniden başlatma
Premium katmanı önbellek steğe bağlı bir veya daha fazla düğümü yeniden başlatmanızı sağlar. Bu, bir arıza olması durumunda dayanıklılık için uygulamanızı test etmek sağlar. Aşağıdaki düğüm yeniden başlatılabilir.

* Önbelleğinizin ana düğümü
* İkincil düğüm önbelleğinizin
* Birincil ve ikincil düğüm önbelleğinizin
* Premium önbellek Kümeleme ile kullanırken, birincil, ikincil veya ayrı ayrı parçalarda önbellek için her iki düğüm yeniden başlatılabilir

Daha fazla bilgi için [yeniden](cache-administration.md#reboot) ve [yeniden SSS](cache-administration.md#reboot-faq).

>[!NOTE]
>Yeniden başlatma tüm Azure önbelleği için Redis katmanları için şimdi işlevselliği etkinleştirilir.
>
>

## <a name="schedule-updates"></a>Güncelleştirmeleri zamanlama
Zamanlanan güncelleştirmeler özelliği, bir bakım penceresi için önbelleğinizi belirlemek sağlar. Bakım penceresi belirtildiğinde, herhangi bir Redis sunucu güncelleştirme sırasında bu pencereyi yapılır. Pencere bir bakım penceresi atamak istediğiniz günleri seçin ve Bakım belirtmek için her gün saat başlatın. Bakım penceresi saati UTC biçiminde olduğunu unutmayın. 

Daha fazla bilgi için [güncelleştirmeleri zamanla](cache-administration.md#schedule-updates) ve [güncelleştirmeleri SSS zamanlaması](cache-administration.md#schedule-updates-faq).

> [!NOTE]
> Yalnızca sunucu zamanlanmış bakım penceresi sırasında yapılan güncelleştirmeler Redis. Bakım penceresi uygulanamaz Azure güncelleştirmeleri ya da sanal makine işletim sistemi güncelleştirmeleri.
> 
> 

## <a name="geo-replication"></a>Coğrafi çoğaltma

**Coğrafi çoğaltma** iki Premium katmanı Azure Cache Redis örneği için bağlama için bir mekanizma sağlar. Bir önbelleği birincil bağlı önbellek ve diğer bağlı ikincil önbellek olarak atanır. Bağlı ikincil önbellek salt okunur hale gelir ve birincil önbelleğe yazılan veri ikincil bağlı önbellek için çoğaltılır. Bu işlev, Azure bölgeleri arasında bir önbellek çoğaltmak için kullanılabilir.

Daha fazla bilgi için [coğrafi çoğaltma, Azure önbelleği için Redis için yapılandırma](cache-how-to-geo-replication.md).


## <a name="to-scale-to-the-premium-tier"></a>Premium katmanına ölçeklendirme
Premium katmanına ölçeklendirmek için yalnızca premium katmanda birini **fiyatlandırma katmanı değişikliği** dikey penceresi. Ayrıca, PowerShell ve CLI kullanarak premium katmanına önbelleğinizi ölçeklendirebilirsiniz. Adım adım yönergeler için bkz: [ölçek Azure Cache, Redis için nasıl](cache-how-to-scale.md) ve [bir ölçeklendirme işlemi otomatik hale getirmek nasıl](cache-how-to-scale.md#how-to-automate-a-scaling-operation).

## <a name="next-steps"></a>Sonraki adımlar
Bir önbellek oluşturun ve yeni premium katman özelliklerini keşfetme.

* [Redis için bir Premium Azure Cache için kalıcılığı yapılandırma](cache-how-to-premium-persistence.md)
* [Redis için bir Premium Azure Cache için sanal ağ desteğini yapılandırma](cache-how-to-premium-vnet.md)
* [Redis için Premium Azure Cache için kümeleri yapılandırma](cache-how-to-premium-clustering.md)
* [Azure önbelleği için Redis nasıl veri aktarmak ve verileri dışarı aktarma](cache-how-to-import-export-data.md)
* [Azure önbelleği için Redis yönetme](cache-administration.md)

