---
title: İşleç en iyi uygulamalar - Azure Kubernetes Hizmetleri (AKS) depolama
description: Depolama, veri şifreleme ve yedekleri Azure Kubernetes Service (AKS) için küme işleci en iyi uygulamaları öğrenin
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: conceptual
ms.date: 12/04/2018
ms.author: iainfou
ms.openlocfilehash: 7476747de31819907cf144e5a6b33cb29e1f866f
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58496183"
---
# <a name="best-practices-for-storage-and-backups-in-azure-kubernetes-service-aks"></a>Depolama ve yedekleme Azure Kubernetes Service (AKS) için en iyi uygulamalar

Oluşturma ve Azure Kubernetes Service (AKS) kümelerini yönetme gibi uygulamalarınızı genellikle depolama gerekir. Performans gereksinimlerini anlamak ve uygulamalara uygun depolama sağlayabilmesi için pod'ların yöntemlere erişmek önemlidir. AKS düğüm boyutunu bu depolama seçenekleri etkileyebilir. Yedekleme ve geri yükleme işlemi için bağlı depolama test yollarını planlamanız gerekir.

Bu en iyi yöntemler makalesi küme operatörleri için hazırlanan depolama konuları ele alınmaktadır. Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Hangi tür depolama vardır
> * AKS düğümleri depolama performansı için doğru boyutu nasıl
> * Dinamik ve statik hacimlerdeki sağlama arasındaki farklar
> * Yedekleme ve veri birimlerinizi güvenli şekilde

## <a name="choose-the-appropriate-storage-type"></a>Uygun depolama türü seçin

**En iyi uygulama kılavuzunu** -doğru depolama seçmek için uygulamanızın gereksinimlerini anlayın. Yüksek performanslı, SSD destekli depolamanın üretim iş yükleri için kullanın. Plan için birden çok eş zamanlı bağlantı ihtiyacı olduğunda ağ tabanlı depolama.

Uygulamalar genellikle farklı türler ve depolama saniyeye kadar düşürmeyi başardık gerektirir. Uygulamalarınızı tek pod'ların bağlayan veya birden çok podunuz arasında paylaşılan depolama gerekir mi? Depolama için yalnızca okuma erişimi veri veya büyük miktarlarda yapısal veriyi yazma için mi? Bu depolama, depolama kullanmak için en uygun türünü belirleme.

Aşağıdaki tabloda kullanılabilir depolama alanı türleri ve yeteneklerini özetler:

| Kullanım örneği | Birim eklentisi | Okuma/yazma kez | Salt okunur çok | Okuma/yazma birçok |
|----------|---------------|-----------------|----------------|-----------------|
| Paylaşılan yapılandırma       | Azure Dosyaları   | Evet | Evet | Evet |
| Yapılandırılmış uygulama verileri        | Azure Diskleri   | Evet | Hayır  | Hayır  |
| Uygulama verilerini, salt okunur paylaşımlar | [Dysk (Önizleme)][dysk] | Evet | Evet | Hayır  |
| Yapılandırılmamış veriler, dosya sistemi işlemleri | [BlobFuse (Önizleme)][blobfuse] | Evet | Evet | Evet |

İki birincil tür aks'deki birimler için sağlanan depolama, Azure diskleri veya Azure dosyaları tarafından desteklenir. Her iki tür depolama güvenliğini artırmak için bekleyen verileri şifreler. varsayılan olarak Azure depolama hizmeti şifrelemesi (SSE) kullanın. Diskler, şu anda Azure Disk şifrelemesi kullanılarak AKS düğümü düzeyinde şifrelenemez.

Azure dosyaları şu anda performans standart katmanda kullanılabilir. Azure diskleri, standart ve Premium performans katmanı mevcuttur:

- *Premium* diskler yüksek performanslı katı hal diskleri (SSD'ler) tarafından desteklenir. Premium diskler, tüm üretim iş yükleri için önerilir.
- *Standart* diskler (HDD'ler) normal dönen disklerle desteklenir ve arşiv veya seyrek erişilen veriler için uygundur.

Uygulama Performans gereksinimlerini anlamak ve erişim desenlerini uygun depolama katmanını seçin. Yönetilen disklerin boyut ve performans katmanları hakkında daha fazla bilgi için bkz: [Azure yönetilen disklere genel bakış][managed-disks]

### <a name="create-and-use-storage-classes-to-define-application-needs"></a>Oluşturma ve uygulama gereksinimlerini tanımlamak için depolama sınıfları kullanma

Kubernetes kullanarak depolama türünü tanımlanan *depolama sınıfları*. Depolama sınıfı ardından pod veya dağıtım belirtiminde başvuruluyor. Bu tanımları uygun depolama oluşturma ve pod'ların bağlamak için birlikte çalışır. Daha fazla bilgi için [depolama sınıfları aks'deki][aks-concepts-storage-classes].

## <a name="size-the-nodes-for-storage-needs"></a>Depolama gereksinimlerini düğümlerin boyutu

**En iyi uygulama kılavuzunu** -her düğüm boyutu en fazla sayıda diske destekler. Farklı bir düğüme boyutları, yerel depolama ve ağ bant genişliği miktarı da sağlar. Uygulama ihtiyaçlarınıza uygun boyutta düğümlerinin dağıtmayı planlayın.

AKS düğümlerini Azure Vm'leri olarak çalıştırın. Farklı türleri ve VM boyutları kullanılabilir. Her VM boyutunun CPU ve bellek gibi çekirdek kaynakları farklı bir miktarda sağlar. Bu VM boyutları en fazla eklenebilecek diskler var. Depolama performansı, ayrıca VM boyutları için en fazla yerel ve ekli disk IOPS (saniyede giriş/çıkış işlemi) arasında değişir.

Uygulamalarınızı Azure disk depolama çözümü olarak gerektiriyorsa, planlama ve uygun bir düğüm VM boyutu seçin. VM boyutu seçme, CPU ve bellek miktarı yalnızca Faktör değildir. Depolama kapasitesini de önemlidir. Örneğin, her ikisi de *Standard_B2ms* ve *Standard_DS2_v2* VM boyutları benzer bir CPU ve bellek kaynaklarının miktarını içerir. Aşağıdaki tabloda gösterildiği gibi olası depolama performanslarını farklıdır:

| Düğüm türü ve boyutu | Sanal işlemci | Bellek (GiB) | Maksimum veri diskleri | Maksimum önbelleğe alınmamış disk IOPS | Maksimum önbelleğe alınmamış aktarım hızı (MB/sn) |
|--------------------|------|--------------|----------------|------------------------|--------------------------------|
| Standard_B2ms      | 2    | 8            | 4              | 1,920                  | 22.5                           |
| Standard_DS2_v2    | 2    | 7            | 8              | 6,400                  | 96                             |

Burada, *Standard_DS2_v2* çift ekli disklerin sayısı ve üç ila dört kat miktarını IOPS ve disk aktarım hızı sağlar. Yalnızca temel işlem kaynakları, baktığı ve maliyetleri karşılaştırıldığında, tercih edebilirsiniz *Standard_B2ms* VM boyutunu ve düşük depolama performansı ve sınırlamalar vardır. Depolama kapasite ve performans gereksinimlerini anlamak için uygulama geliştirme ekibinizle birlikte çalışın. AKS düğümlerinin karşıladığında veya performans gereksinimlerine uygun VM boyutunu seçin. VM boyutu gerektiği şekilde ayarlamak için düzenli olarak temel uygulamaları.

Kullanılabilir VM boyutları hakkında daha fazla bilgi için bkz. [azure'da Linux sanal makine boyutları][vm-sizes].

## <a name="dynamically-provision-volumes"></a>Dinamik olarak sağlama birimleri

**En iyi uygulama kılavuzunu** - yönetim yükünü ve ölçeklendirme, yoksa statik olarak oluşturun ve kalıcı birim atamak izin verir. Dinamik sağlama kullanın. Depolama sınıflarınızdaki pod'ların silindikten sonra gereksiz depolama maliyetlerini en aza indirmek için geri kazanmaya uygun ilkeyi tanımlayın.

Pod'ları için depolama ekleme gerektiğinde, kalıcı birimler kullanın. Bu kalıcı birimler, el ile veya dinamik olarak oluşturulabilir. Kalıcı birimlerin el ile oluşturulması, yönetim yükünü ekler ve ölçeklendirme yeteneğinizi kısıtlar. Depolama yönetimini basitleştirmelerine ve uygulamalarınızı büyütün ve gerektiğinde ölçeği izin vermek için sağlama dinamik kalıcı hacim kullanın.

![Azure Kubernetes Hizmetleri (AKS) kümesini Taleplerde kalıcı hacim](media/concepts-storage/persistent-volume-claims.png)

Kalıcı hacim talep (PVC), dinamik olarak gerektiğinde depolama oluşturmanızı sağlar. Pod'ların istekleri gibi temel Azure diskleri oluşturulur. Pod tanımında oluşturulması ve tasarlanmış bağlama yoluna bağlı bir ses isteği

Dinamik olarak oluşturabilen ve birimler kullanmak nasıl kavramlar için bkz. [kalıcı birimleri talep][aks-concepts-storage-pvcs].

Bu birimleri iş başında görmek için nasıl dinamik olarak oluşturabilen ve kalıcı bir birimi ile kullanmak bkz. [Azure diskleri] [ dynamic-disks] veya [Azure dosyaları][dynamic-files].

Depolama sınıfı tanımlarınızı bir parçası olarak, uygun ayarlanmış *reclaimPolicy*. Bu reclaimPolicy pod silinir ve kalıcı hacim artık gerekli olmayabilir, temel alınan Azure depolama kaynağı davranışını denetler. Temel alınan depolama kaynağı silinmiş veya gelecekteki bir pod ile kullanılmak üzere saklanır. ReclaimPolicy ayarlayabilirsiniz *korumak* veya *Sil*. Uygulama ihtiyaçlarınızı anlayabilmemiz ve kullanılan faturalandırılır ve beklemediğiniz kullanılan depolama miktarını en aza indirmek için korunan depolama için normal denetimlerini uygular.

Depolama sınıfı seçenekleri hakkında daha fazla bilgi için bkz. [depolamayı geri alma ilkeleri][reclaim-policy].

## <a name="secure-and-back-up-your-data"></a>Güvenli ve verilerinizi yedekleme

**En iyi uygulama kılavuzunu** - geri Velero veya Azure Site Recovery gibi depolama türü için uygun bir araç kullanarak verilerinizi. Bütünlük ve güvenliğini, bu yedekleri doğrulayın.

Uygulamalarınızı depolamak ve kullanmak veri diskleri veya dosyaları kalıcı, düzenli yedeklemeler veya verilerin anlık görüntüsünü almak gerekir. Azure diskleri yerleşik anlık görüntü teknolojileri kullanabilirsiniz. Anlık görüntü işlemi gerçekleştirmeden önce diske temizleme Yazar uygulamalarınız için bir kancasını gerekebilir. [Velero] [ velero] ek küme kaynaklarını ve yapılandırmalarıyla birlikte kalıcı birimler yedekleyebilirsiniz. Çalıştıramıyorsanız [durumu uygulamasını kaldırmak][remove-state]kalıcı birimlerdeki verileri yedeklemek ve geri yükleme işlemleri, veri bütünlüğü ve gerekli işlemleri doğrulamak için düzenli olarak test edin.

Veri yedekleri ve anlık görüntü önce verilerinizi Sessiz mod için gerekiyorsa farklı yaklaşımların kısıtlamaları anlayın. Veri yedekleri, uygulama ortamınız Küme dağıtımı geri mutlaka izin vermeyin. Bu senaryolar hakkında daha fazla bilgi için bkz. [aks'deki iş sürekliliği ve olağanüstü durum kurtarma için en iyi yöntemler][best-practices-multi-region].

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede aks'deki en iyi depolamaya odaklı. Kubernetes'te depolama temelleri hakkında daha fazla bilgi için bkz. [AKS uygulamalar için depolama kavramları][aks-concepts-storage].

<!-- LINKS - External -->
[velero]: https://github.com/heptio/velero
[dysk]: https://github.com/Azure/kubernetes-volume-drivers/tree/master/flexvolume/dysk
[blobfuse]: https://github.com/Azure/azure-storage-fuse

<!-- LINKS - Internal -->
[aks-concepts-storage]: concepts-storage.md
[vm-sizes]: ../virtual-machines/linux/sizes.md
[dynamic-disks]: azure-disks-dynamic-pv.md
[dynamic-files]: azure-files-dynamic-pv.md
[reclaim-policy]: concepts-storage.md#storage-classes
[aks-concepts-storage-pvcs]: concepts-storage.md#persistent-volume-claims
[aks-concepts-storage-classes]: concepts-storage.md#storage-classes
[managed-disks]: ../virtual-machines/linux/managed-disks-overview.md
[best-practices-multi-region]: operator-best-practices-multi-region.md
[remove-state]: operator-best-practices-multi-region.md#remove-service-state-from-inside-containers
