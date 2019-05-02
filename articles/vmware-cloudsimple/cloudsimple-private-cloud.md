---
title: Özel VMware çözümüyle CloudSimple - Azure bulutlarında
description: CloudSimple özel Bulutlar ve kavramları hakkında bilgi edinin.
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: dc07b4eea553e6cb3d9b522826e860ddbfbc1513
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2019
ms.locfileid: "64577053"
---
# <a name="cloudsimple-private-cloud-overview"></a>CloudSimple özel buluta genel bakış

CloudSimple dönüştürür ve genel bulutlara VMware iş yüklerini dakikalar içinde genişletir. CloudSimple hizmetini kullanarak VMware Azure çıplak metal altyapısı üzerinde yerel olarak dağıtabilirsiniz. Dağıtımınız, Azure konumlarında yaşar ve Azure bulut geri kalanı ile tamamen tümleştirilir.

* Tam VMware operasyonel devamlılığı CloudSimple çözümüdür. Bu çözüm, genel bulut avantajlarını sağlar:
  * Esneklik
  * Yenilik
  * Verimlilik
* CloudSimple ile toplam sahiplik maliyetinizi düşürür bir bulut tüketim modelinden yararlanır. Ayrıca, isteğe bağlı sağlama, ödeme olarak-,-büyütme ve kapasite iyileştirme de sunar.
* CloudSimple tamamen uyumludur:
  * Mevcut araçları
  * Beceriler
  * İşlemler
* Bu uyumluluk ilkeleriniz kesintiye uğratmadan iş yükleri, Azure bulutunda yönetme olanağı sağlar:
  * Ağ
  * Güvenlik  
  * Veri koruma  
  * Denetim
* Altyapı ve gerekli tüm ağ ve Yönetim Hizmetleri CloudSimple yönetir. CloudSimple hizmet odaklanmak takımınızın sağlar:
  * İş değeri
  * Uygulama sağlama
  * İş sürekliliği
  * Destek
  * İlke zorlama

## <a name="private-cloud-environment-overview"></a>Özel bulut ortamına genel bakış

Bir özel buluta bu ortamları gibi yalıtılmış bir VMware yığınınızın şöyledir:

* ESXi ana bilgisayarları
* vCenter
* vSAN
* NSX

Özel Bulutlar, kendi Yönetim etki alanındaki bir vCenter sunucusu tarafından yönetilir.

Yığın çalışır:

* Ayrılmış düğümler
* Yalıtılmış çıplak metal donanım

Kullanıcılar yerel VMware araçları yığınından kullanır:

* vCenter
* NSX Yöneticisi

Azure konumları adanmış düğümler dağıtabilirsiniz. Daha sonra bunları CloudSimple ve Azure ile yönetebilirsiniz. Özel bulut bir veya daha fazla vSphere kümelerindeki oluşur ve her küme 3-16 düğüm içerir.

Düğümleri satın kullanarak bir özel bulut oluşturabilirsiniz:

* Kullandıkça Öde düğümleri
* Ayrılmış, adanmış düğümler

Özel bulut, şirket içi ortamınız ve aşağıdaki bağlantıları kullanarak Azure ağı bağlanabilirsiniz:

* Güvenlik
* Özel VPN
* Azure ExpressRoute

Özel bulut ortamı, bir tek hata noktası bulunmamasına ortadan kaldırmak için tasarlanmıştır:

* ESXi kümeleri vSphere yüksek kullanılabilirlik ile yapılandırılmış ve dayanıklılık için en az bir yedek düğüm için boyutlandırılır.
* vsan'ı birincil yedek depolama sağlar. vsan'ı tek bir hataya karşı koruma sağlamak için en az üç düğüm gerektirir. Vsan'ı, daha büyük kümeleri için daha yüksek bir dayanıklılık sağlamak üzere yapılandırabilirsiniz.
* Depolama arızasına karşı korumak için RAID 10 depolama ilkesiyle vCenter ve PSC NSX Manager Vm'leri yapılandırabilirsiniz. Ardından, vSphere HA düğümü ve ağ hatalarına karşı tarafından korumalı olup olmadıklarını.

## <a name="scenarios-for-deploying-a-private-cloud"></a>Özel bir buluta dağıtmak için senaryolar

* **Veri merkezinin kullanımdan kaldırma veya taşıma**

  * Ek kapasite sınırları mevcut veri Merkezinize ulaşın veya donanım Yenile alın.
  * Gereken kapasite buluta ekleyin ve donanım yenilemeleri yönetme sıkıntılarına otomatik çözüm sağlamaya ortadan kaldırın.
  * Risk ve buluta geçiş, zaman alıcı dönüştürmeleri veya rearchitecture karşılaştırıldığında maliyetini azaltın.
  * Buluta geçiş hızlandırmak için alışık olduğunuz VMware araçları ve becerileri kullanın. Bulutta Azure Hizmetleri, kendi belirlediğiniz hızda uygulamalarınızı modernize etme kullanın.

* **İsteğe bağlı olarak genişletin**

  * Yeni geliştirme ortamları veya dönemsel kapasite artışları gibi beklenmeyen ihtiyaçlarınızı karşılayacak şekilde buluta genişletin.
  * İsteğe bağlı olarak yeni kapasite oluşturun ve yalnızca ihtiyacınız olduğu sürece saklayın.
  * Ön yatırımınızı azaltın, sağlama hızını artırın ve hem şirket içinde ve bulutta aynı mimarisi ve ilkeleri ile karmaşıklığı azaltın.

* **Olağanüstü durum kurtarma ve Azure bulut ortamında sanal masaüstleri**

  * Uzaktan erişim verileri, uygulamalar ve masaüstü bilgisayarlar Azure bulutunda oluşturun. Yüksek bant genişlikli bağlantıları olan karşıya yükleme / indirme veri kaynaklı olayların Hızlı Kurtarma. Yanıt hızlı, düşük gecikmeli ağlar verin, kullanıcıların bir masaüstü uygulamasından beklediğiniz zaman.

  * Tüm ilkeler ve CloudSimple portalı ve alışık olduğunuz VMware Araçları'nı kullanarak bulutta ağ çoğaltın. Bu çoğaltma, çaba ve oluşturma ve yönetme DR ve VDI uygulamaları riskini azaltır.

* **Yüksek performanslı uygulamaları ve veritabanları**

  * Hiper yakınsanmış mimarisi CloudSimple tarafından sağlanan En zorlu iş yüklerini çalıştırın.
  * Oracle, Microsoft SQL server, ara yazılım sistemleri ve yüksek performanslı no-SQL veritabanı'nı çalıştırın.

  * Yüksek hızlı 25 GB/sn ağ bağlantıları ile kendi veri merkezi olarak bulut deneyimi. Yüksek hızlı bağlantıları etkinleştirmek, şirket içinde azure'da VMware kapsayan hibrit uygulamalar çalıştırmak ve performanstan taviz olmadan Azure çok özel iş yükleri.

* **Gerçek Hibrit**

  * DevOps, VMware ve Azure Hizmetleri genelinde birleştirin.
  * VMware yönetimi için Azure Hizmetleri ve tüm yüklerinize uygulanabilir çözümleri iyileştirin.
  * Genel bulut Hizmetleri, veri merkezinizi genişletin veya uygulamalarınızı yeniden oluşturma zorunda kalmadan erişir.
  * Kimlik, erişim denetimi ilkeleri, günlüğe kaydetme ve izleme için azure'da VMware uygulamaları tek bir merkezden yönetin.

## <a name="limits"></a>Limits

Aşağıdaki tablo, bir özel bulut kaynaklarında düğümü sınırları gösterir.

| Kaynak | Sınır |
|----------|-------|
| Özel bir bulut oluşturmak için düğüm sayısı alt sınırı | 3 |
| En fazla özel bulut üzerinde bir kümedeki düğüm sayısını | 16 |
| En fazla özel bulutta düğüm sayısı | 64 |
| Yeni kümede düğüm sayısı alt sınırı | 3 |

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi edinmek için nasıl [özel bulut oluşturma](https://docs.azure.cloudsimple.com/create-private-cloud/)
* Bilgi edinmek için nasıl [bir özel bulut ortamı yapılandırma](quickstart-create-private-cloud.md)