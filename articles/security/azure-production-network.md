---
title: Azure üretim ağı
description: Bu makalede, Microsoft Azure üretim ağı genel bir açıklamasını sağlar.
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: TomSh
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2018
ms.author: terrylan
ms.openlocfilehash: 5c0bfae35464e73278a1efd9c04a03123bb9018a
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37102664"
---
# <a name="azure-production-network"></a>Azure üretim ağı
Azure üretim ağ kullanıcılarının kendi Microsoft Azure uygulamalarının yanı sıra üretim ağı yönetmek iç Microsoft Azure destek personeli erişme dış müşterileri içerir. Güvenlik erişim yöntemleri ve koruma mekanizmalarını Azure üretim ağ bağlantıları kurmak için bu makalede ele alınmıştır.

## <a name="internet-routing-and-fault-tolerance"></a>Internet yönlendirme ve hataya dayanıklılık
Genel olarak yedekli bir dahili ve harici Microsoft Azure etki alanı adı hizmeti (WADNS) altyapı birden çok birincil ve ikincil etki alanı adı hizmeti (DNS) sunucu kümeleri ile birlikte sunar ek Microsoft Azure ağ sırasında hataya dayanıklılık için NetScaler kullanılan gibi dağıtılmış hizmet engelleme (DDoS) saldırıları önlemek ve Microsoft Azure DNS hizmetleri bütünlüğünü korumak için güvenlik denetimleri.

WADNS sunucuları birden çok veri merkezi tesis bulunur. WADNS uygulama genel olarak Azure müşteri etki alanı adları çözümlemek için ikincil/birincil DNS sunucuları hiyerarşisini içerir. Etki alanı adları genellikle Müşteri'nin hizmet için sanal IP (VIP) adresi saran bir CloudApp.net adresi çözümlenemiyor. Azure için benzersiz, Kiracı çeviri iç ayrılmış IP'yi (DIP) adresine karşılık gelen VIP Microsoft yük dengeleyici, VIP için sorumlu gerçekleştirilir.

Azure veri merkezlerinde coğrafi olarak dağıtılmış Azure ABD içinde barındırılan ve güçlü ve ölçeklenebilir mimari standartları uygulama durumu resim yönlendirme platformlarda yerleşik olarak bulunur. Önemli özelliklerinden bazıları şunlardır:

- Bir kesinti ise verimli bağlantı kullanımı ve hizmet normal düşmesine sağlama trafiği mühendislik çok protokollü etiket anahtarlama (MPLS) tabanlı
- Ağlar "gerek plus bir" uygulanır (N + 1) artıklık mimariler veya iyi olur.
- Veri merkezleri nedenle özellikler üzerinde 1.200 Internet hizmet sağlayıcıları, / saniye (Gbps) 2.000 gigabayt aşan sağlayan genel olarak birden çok eşleme noktalarında bağlamak ayrılmış, yüksek bant genişlikli ağ bağlantı hatları tarafından harici olarak sunulur Edge kapasitesi.

Microsoft veri merkezleri arasında kendi ağ devreler sahibi olarak bu öznitelikler elde 99.9 + Azure teklifi Yardım % ağ kullanılabilirliğini geleneksel üçüncü taraf Internet hizmet sağlayıcıları gerek kalmadan.

## <a name="connection-to-production-network-and-associated-firewalls"></a>Üretim ağ ve ilişkili güvenlik duvarları için bağlantı
Azure ağ Internet trafiği akışı İlkesi Amerika Birleşik Devletleri içinde en yakın bölgesel veri merkezinde bulunan Azure üretim ağ trafiğini yönlendirir. Tutarlı bir ağ mimarisi ve donanım, Azure üretim veri merkezleri Bakımı trafik akışı açıklama tutarlı bir şekilde tüm veri merkezleri için geçerlidir.

Azure için Internet trafiği için en yakın veri merkezinin yönlendirilir sonra erişim yönlendiricilerine bir bağlantı oluşturulur. Azure düğümleri ve müşteri örneği VM'ler arasındaki trafiği yalıtmak için bu erişim yönlendiriciler hizmet. Ağ altyapısı aygıtlarındaki erişim ve kenar konumlara giriş ve/veya çıkış filtreleri nereye uygulanacağını sınır noktalarıdır. Bu yönlendirici istenmeyen ağ trafiğini filtrelemek ve gerekirse trafiği oran sınırları uygulamak için katmanlı bir ACL yapılandırılır. ACL tarafından izin verilen trafik yük dengeleyicilerini yönlendirilir. Dağıtım yönlendiriciler yalnızca Microsoft onaylı IP adreslerine izin vermek, ACL'leri kullanarak sahtekarlığa karşı koruma ve Yerleşik TCP bağlantıları sağlamak için tasarlanmıştır.

Dış Yük Dengeleme cihazları ağ adresi çevirisi (NAT) için Azure iç IP'ler Internet'ten yönlendirilebilir Ip'lerden gerçekleştirmek için erişim yönlendiriciler arkasında bulunur. Bunlar ayrıca geçerli üretim iç IP ve bağlantı noktaları için paketleri yönlendirmek ve iç üretim ağ adresi alanını gösterme sınırlamak için bir koruma mekanizması davranır.

Varsayılan olarak, Microsoft Müşteri'nin web tarayıcıları dahil olmak üzere, oturum açın ve tüm trafiği için bundan sonra iletilen tüm trafik için Köprü Metni Aktarım Protokolü güvenli (HTTPS) zorlar. TLS 1.2 sürümü kullanımını akışına trafiği için güvenli bir tünel sağlar. Erişim ve çekirdek yönlendiriciler ACL'lerin trafiği kaynağı nelerin beklendiğini ile tutarlı olduğundan emin olun.

Ayrılmış donanım güvenlik duvarı olmadığından, özelleştirilmiş izinsiz giriş algılama/önleme cihazlar ya da önce normalde beklenen diğer güvenlik Gereçleri geleneksel güvenlik mimarisi karşılaştırıldığında bu mimarisinde önemli bir fark olduğu bağlantılar, Azure üretim ortamına yapılır. Müşteriler, bu donanım güvenlik duvarı aygıtları Azure ağında genellikle beklediğiniz; Ancak, bulunmazlar Azure içinde değişiklik. Neredeyse özel olarak, bu güvenlik özellikleri, güvenlik duvarı yetenekleri de dahil olmak üzere güçlü çok katmanlı güvenlik mekanizmaları sağlamak üzere Azure ortamı çalıştıran yazılımına oluşturulur. Ayrıca, sınır ve kritik güvenlik aygıtların ilişkili karmaşıklığını kapsamını yönetmek ve envanteri Azure çalışan yazılım tarafından yönetildiğinden Yukarıdaki çizimde gösterildiği gibi daha kolay olur.

## <a name="core-security-and-firewall-features"></a>Çekirdek güvenlik ve Güvenlik Duvarı Özellikleri
Azure sağlam yazılım güvenliği ve güvenlik duvarı özellikleri genellikle çekirdek güvenlik yetkilendirme sınır korumak için geleneksel bir ortamda beklenen güvenlik özellikleri zorlamak için çeşitli düzeylerde uygular.

### <a name="azure-security-features"></a>Azure güvenlik özellikleri
Azure üretim ağı içinde yazılım ana bilgisayar tabanlı güvenlik duvarları uygular. Birkaç güvenlik çekirdek ve Güvenlik Duvarı özelliklerini Azure ortamı çekirdek içinde bulunur. Bu güvenlik özellikleri savunma stratejisi Azure ortamında yansıtır. Microsoft Azure müşteri verilerinde aşağıdaki güvenlik duvarları tarafından korunur:

**Hiper yönetici Güvenlik Duvarı'nı (paket filtresi)**: Bu güvenlik duvarı hiper yöneticide uygulanan ve FC aracı tarafından yapılandırılır. Bu güvenlik duvarı Kiracı VM içinde çalışan yetkisiz erişime karşı korur. Varsayılan olarak, bir VM oluşturulduğunda, tüm trafik engellenir ve sonra FC Aracısı kuralları/özel durum yetkili trafiğe izin verecek şekilde filtre ekler.

Burada programlanan iki kural kategorisi vardır:

- Yapılandırma veya altyapı kuralları makine: varsayılan olarak, tüm iletişimin engellenir. Gönderme ve Dinamik Ana Bilgisayar Yapılandırma Protokolü (DHCP) iletişimleri, DNS bilgilerini almak, işletim sistemi Etkinleştirme sunucusu ve FC küme içindeki diğer VM'ler için giden "Genel" Internet trafiği göndermek için bir VM izin veren özel durumlar vardır. Sanal makineleri giden listesi izin bu yana hedefleri Microsoft Azure yönlendirici alt ağları ve diğer Microsoft özellikleri içermez, kuralları bir koruma katmanı olarak bunları hareket.
- Rol yapılandırma dosyası: kiracılar hizmet modelini temel alan gelen ACL'ler tanımlar. Örneğin, bir kiracı belirli bir VM üzerinde bağlantı noktası 80 üzerinde bir web ön ucu vardır, ardından bağlantı noktası 80, tüm IP adresleri için açıldı. VM çalıştıran çalışan rolü varsa, daha sonra çalışan rolü yalnızca aynı Kiracı içinde VM açılır.

**Yerel ana bilgisayar güvenlik duvarı**: Microsoft Azure doku ve depolama çalıştıran bir yerel hiçbir hiper yönetici olan işletim sisteminde, ve bu nedenle Windows Güvenlik duvarı kuralları Yukarıdaki iki kümeleriyle yapılandırılır.

**Ana bilgisayar güvenlik duvarı**: ana bilgisayar güvenlik duvarı hiper yönetici çalıştıran ana bölüm korur. Kuralları yalnızca FC izin vermek ve belirli bir bağlantı noktası ana bölüme konuşmaya kutuları atlamak için programlanmış. Diğer özel durumlar ise DHCP ve DNS yanıtlarına izin vermek üzere belirlenmiştir. Azure güvenlik duvarı kuralları ana bilgisayar bölümü şablonun bir makine yapılandırma dosyası kullanır. Ana bilgisayar bileşenleri, kablo sunucu & bağlantı noktalarından belirli Protokolü/meta veri sunucusu iletişim kurmak sanal makineleri veren bir ana bilgisayar güvenlik duvarı özel durumu yok.

**Konuk Güvenlik Duvarı'nı**: Müşteri sanal makineleri ve depolama müşteri tarafından yapılandırılabilir konuk işletim sisteminin Windows Güvenlik Duvarı parça.

Ek güvenlik özellikleri yerleşik-Azure özellikleri:

- Altyapı bileşenlerine ayrılmış Ip'lerden (Dıps) IP adresi atanır. Bunlar Microsoft kaynaklandığından değil çünkü Internet üzerindeki bir saldırgan bu adreslere trafiği adresi olamaz. Internet ağ geçidi yönlendirici, bunlar üretim ağı girmemeniz böylece yalnızca iç adreslere gönderilen paketleri filtreleyin. VIP'ler için yönlendirilmiş trafiğini kabul yalnızca yük dengeleyici bileşenlerdir.
- Tüm iç düğümlerinde uygulanan güvenlik duvarları, belirli bir senaryo için üç birincil güvenlik mimarisi konuları vardır:

   - Bunlar yük dengeleyici (LB) arkasında yerleştirilir ve her yerden paketleri kabul edin. Bunlar dışarıdan kullanıma yöneliktir ve geleneksel çevre güvenlik duvarında açık bağlantı noktalarını karşılık gelir.
   - Yalnızca sınırlı sayıda adresleri paketler kabul edin. Bu hizmet reddi saldırılarına karşı savunma ayrıntılı stratejisinin bir parçasıdır. Bu tür bağlantıları şifreli olarak doğrulanır.
   - Güvenlik duvarları yalnızca select iç düğümlerden erişilebilir, bu durumda yalnızca bir listeden numaralandırılmış Dıps Azure ağında tümü kaynak IP adreslerinin, paketleri kabul. Örneğin, kurumsal ağ üzerindeki bir saldırı bu adresleri isteklerine yönlendirebilir, ancak paketin kaynak adresini bir Azure ağı içinde numaralandırılmış listede değilse bunlar engellenebilir.
   - Çevre erişim yönlendiricide, yapılandırılmış statik yollar nedeniyle Azure ağında bir adresi ele giden paketleri engeller.

## <a name="next-steps"></a>Sonraki adımlar
Microsoft Azure altyapı güvenli hale getirmek için yaptığı hakkında daha fazla bilgi için bkz:

- [Azure olanakları, şirket içi ve fiziksel güvenlik](azure-physical-security.md)
- [Azure altyapı kullanılabilirliği](azure-infrastructure-availability.md)
- [Azure Information sistem bileşenleri ve sınırlar](azure-infrastructure-components.md)
- [Azure ağ mimarisi](azure-infrastructure-network.md)
- [Microsoft Azure SQL veritabanı güvenlik özellikleri](azure-infrastructure-sql.md)
- [Azure üretim işlemleri ve Yönetimi](azure-infrastructure-operations.md)
- [Azure altyapı izleme](azure-infrastructure-monitoring.md)
- [Azure altyapı bütünlüğü](azure-infrastructure-integrity.md)
- [Azure müşteri verileri koruma](azure-protection-of-customer-data.md)
