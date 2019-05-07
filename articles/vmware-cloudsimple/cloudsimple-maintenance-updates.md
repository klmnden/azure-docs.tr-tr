---
title: Azure VMware çözümüyle CloudSimple - CloudSimple Bakım ve güncelleştirmeler
description: Zamanlanan Bakım ve güncelleştirmeler için CloudSimple hizmet işlemi açıklanır
author: sharaths-cs
ms.author: dikamath
ms.date: 04/30/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 4dde358f10e9ac5054297ff68a0971404c0dc135
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65160253"
---
# <a name="cloudsimple-maintenance-and-updates"></a>CloudSimple Bakım ve güncelleştirmeler

Özel bulut ortamı yok tek hata noktası sağlamak için tasarlanmıştır:

* ESXi kümeler vSphere yüksek kullanılabilirlik ile yapılandırılmıştır. Kümeler, dayanıklılık için en az bir yedek düğüm için boyutlandırılır.
* Yedekli birincil depolama, tek bir hataya karşı koruma sağlamak için en az üç düğüm gerektiren vSAN tarafından sağlanır. vSAN, daha büyük kümeleri için daha yüksek bir dayanıklılık sağlamak üzere yapılandırılabilir.
* vCenter ve PSC NSX Yöneticisi VM'ler depolama arızasına karşı korumak için RAID 10 saklama İlkesi ile yapılandırılır. Vm'leri vSphere ile HA düğümü/ağ hatalarına karşı korunur.
* ESXi konakları gereksiz fanlar ve NIC ' var.
* TOR ve sırt anahtarları HA çiftlerini dayanıklılık sağlamak üzere yapılandırılır.

CloudSimple sürekli çalışma süresi ve kullanılabilirlik için aşağıdaki sanal makineleri izler ve kullanılabilirlik SLA'larını sunar:

* ESXi ana bilgisayarları
* vCenter
* PSC
* NSX Yöneticisi

CloudSimple ayrıca aşağıdaki hataları için sürekli olarak izler:

* Sabit diskler
* Fiziksel NIC bağlantı noktası
* Sunucular
* Fanı
* Güç
* Anahtarlar
* Anahtar bağlantı noktaları

Bir disk veya düğüm başarısız olursa, yeni bir düğüm hemen sistem durumu geri getirmek için etkilenen VMware küme otomatik olarak eklenir.

CloudSimple yedekler, korur ve bu özel Bulutları VMware öğeleri güncelleştirir:

* ESXi
* vCenter Platform Hizmetleri
* Denetleyici
* vSAN
* NSX

## <a name="back-up-and-restore"></a>Yedekleme ve geri yükleme

CloudSimple yedeklemesi şunları içerir:

* VCenter ve PSC DVS her gece artımlı yedeklerini kuralları.
* Kullanım vCenter'ın uygulama katmanındaki bileşenleri yedeklemek için yerel API.
* Otomatik yedekleme önce herhangi bir güncelleştirme veya yükseltme VMware yönetim yazılımı.
* Kaynaktaki verileri Azure'a TLS1.2 şifrelenmiş bir kanal üzerinden aktarmadan önce vcenter veri şifreleme. Verilerin nerede bölgede çoğaltılan Azure blob'a depolanır.

Bir geri yükleme açarak isteyebilir bir [destek isteği](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="maintenance"></a>Bakım

CloudSimple birden fazla planlanmış bakım gerçekleştirir.

### <a name="backendinternal-maintenance"></a>Arka uç/dahili bakım

Bu bakım, genellikle fiziksel varlıklar yeniden yapılandırma ya da yazılım düzeltme eklerini yüklemeyi içerir. Bu normal tüketiminin bakımda varlıkları etkilemez. Her bir fiziksel rafa giderek yedekli NIC ile normal ağ trafiğini ve özel bulut işlemleri etkilenmez. Bir performans etkisi, yalnızca kuruluşunuzun tam yedekli bant genişliği bakım aralığı sırasında kullanılacak bekliyorsa fark edebilirsiniz.

### <a name="cloudsimple-portal-maintenance"></a>CloudSimple portal bakım

Bazı sınırlı kapalı kalma CloudSimple denetim düzlemi veya altyapı güncelleştirildiğinde gereklidir. Şu anda bakım aralıkları ayda bir kez kadar sık olabilir. Sıklığını, zaman içinde reddetme beklenir. CloudSimple portal bakımı için bildirim sağlar ve aralığı olabildiğince kısa tutar. Bir portal bakım aralığı sırasında aşağıdaki hizmetleri herhangi bir etkisi çalışmaya devam eder:

* VMware yönetim düzlemi ve uygulamalar
* vCenter erişim
* Tüm ağ ve depolama
* Tüm Azure trafiğinin

### <a name="vmware-infrastructure-maintenance"></a>VMware altyapı Bakımı

Bazen VMware altyapısı yapılandırmada değişiklik yapmak gereklidir.  Şu anda, bu aralık 1-2 ayda oluşabilir, ancak sıklığı zaman içinde reddetme beklenir. Bu tür bir bakım genellikle normal tüketim CloudSimple Hizmetleri kesintiye uğratmadan yapılabilir. Bir VMware bakım aralığı sırasında aşağıdaki hizmetleri herhangi bir etkisi çalışmaya devam eder:

* VMware yönetim düzlemi ve uygulamalar
* vCenter erişim
* Tüm ağ ve depolama
* Tüm Azure trafiğinin

## <a name="updates-and-upgrades"></a>Güncelleştirmeler ve yükseltmeler

CloudSimple yaşam döngüsü yönetimi ve özel bulutta VMware yazılımının (ESXi ve vCenter, PSC ve NSX) sorumludur.

Yazılım güncelleştirmeleri içerir:

* **Düzeltme ekleri**. Güvenlik düzeltme ekleri veya VMware tarafından yayımlanan hata düzeltmeleri.
* **Güncelleştirmeleri**. Bir VMware yığın bileşenin podverze değiştirin.
* **Yükseltmeler**. Bir VMware yığın bileşenin sürümle değiştirin.

Vmware'den kullanılabilir duruma geldiğinde CloudSimple kritik güvenlik düzeltme eki sınar. SLA'sı CloudSimple bir hafta içinde özel bulut ortamları için güvenlik düzeltme ekini yapar.

Üç aylık bakım güncelleştirmeleri için VMware yazılım bileşenlerini CloudSimple sağlar. VMware yazılım'ın yeni bir ana sürüm kullanılabilir olduğunda CloudSimple yükseltme için uygun bakım penceresi koordine etmek için müşteriler ile çalışır.

## <a name="next-steps"></a>Sonraki adımlar

[Geri Veeam kullanarak iş yükü Vm'leri yedekleme](https://docs.azure.cloudsimple.com/backup-workloads-veeam/).