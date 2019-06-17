---
title: Azure üretim ağı
description: Bu makalede, Azure üretim ağı genel bir açıklamasını sağlar.
services: security
documentationcenter: na
author: TerryLanfear
manager: barbkess
editor: TomSh
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2018
ms.author: terrylan
ms.openlocfilehash: afae7cc6390ea4cd8c18c687e9d99400c8da9da4
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60611352"
---
# <a name="the-azure-production-network"></a>Azure üretim ağı
Kendi Azure uygulamaları ve üretim ağı yönetmek iç Azure destek personeli erişen hem de dış müşterilere Azure üretim ağı kullanıcıları içerir. Bu makalede, Azure üretim ağ bağlantı kurma için koruma mekanizmaları ve güvenlik erişim yöntemleri açıklanmaktadır.

## <a name="internet-routing-and-fault-tolerance"></a>Internet yönlendirme ve hataya dayanıklılık
Birden çok birincil ve ikincil DNS sunucusu kümeleri ile birleştirilmiş bir Global yedekli iç ve dış Azure etki alanı adı hizmeti (DNS) altyapısı, hata toleransı sağlar. Aynı anda NetScaler gibi ek Azure ağ güvenlik denetimleri, dağıtılmış hizmet engelleme (DDoS) saldırılarının önlemek ve Azure DNS hizmetleri bütünlüğünü korumak için kullanılır.

Azure DNS sunucuları birden çok veri merkezinde tesis yer alır. Azure DNS uygulama, genel Azure müşteri etki alanı adlarını çözümlemek için ikincil ve birincil DNS sunucularından oluşan bir hiyerarşi içerir. Etki alanı adları genellikle müşterinin hizmet için sanal IP (VIP) adresi saran bir CloudApp.net adresi çözümlenemiyor. Azure için benzersiz, iç ayrılmış IP (DIP) adresine Kiracı çeviri karşılık gelen VIP Microsoft yük Dengeleyiciler için bu VIP sorumlu gerçekleştirilir.

Azure ABD içindeki Azure coğrafi olarak dağıtılmış veri merkezlerinde barındırılan ve sağlam, ölçeklenebilir mimari standartlarına uygulayan durumu resim yönlendirme platformlarda oluşturulmuştur. Arasında önemli özellikleri şunlardır:

- Bir kesinti oluşursa, verimli bağlantı kullanımı ve normal hizmet düşmesine sağlayan çok protokollü etiket anahtarlama MPLS tabanlı trafik mühendislik.
- Ağlar "gerek plus bir" uygulanır (N + 1) yedeklilik mimariler veya daha iyi.
- Harici olarak nedenle özellikler üzerinde 1.200 internet hizmet sağlayıcıları genel olarak noktalarda birden çok eşleme bağlanmak adanmış, yüksek bant genişliğine sahip ağ bağlantı hatları ile veri merkezlerinden sunulur. Bu bağlantı, 2.000 gigabayt aşan / saniye (GBps) edge kapasitesi sağlar.

Microsoft veri merkezleri arasında kendi ağ bağlantı hattına sahip olmadığından, bu öznitelikler, geleneksel üçüncü taraf internet hizmet sağlayıcıları gerek kalmadan yüzde 99,9 + ağ kullanılabilirlik elde etmek Azure teklifi yardımcı olur.

## <a name="connection-to-production-network-and-associated-firewalls"></a>Üretim ağ ve ilişkili güvenlik duvarları bağlantı
Azure ağ internet trafiği akışı ilkesi en yakın bölgesel veri merkezi ABD içinde bulunan Azure üretim ağ trafiğini yönlendirir. Azure üretim veri merkezlerinde tutarlı ağ mimarisi ve donanım korumak için aşağıdaki trafik akışı Açıklama tüm veri merkezlerine tutarlı bir şekilde uygular.

İnternet trafiği için Azure en yakın veri merkezine yönlendirilir sonra erişim yönlendiriciler için bir bağlantı kurulur. Azure düğümleri ve müşteri örneği VM'ler arasındaki trafiği yalıtmak için bu erişim yönlendiriciler hizmet. Erişim ve uç konumlarında ağ altyapısı cihazları, giriş ve çıkış filtreleri nereye uygulanacağını sınır noktalarıdır. Bu yönlendiriciler, istenmeyen ağ trafiğini filtreleme ve gerekirse, trafiği oran sınırları uygulamak için bir katmanlı erişim denetimi listesi (ACL) olarak yapılandırılır. ACL tarafından izin verilen trafik yük dengeleyiciye yönlendirilir. Dağıtım yönlendiricileri, yalnızca Microsoft onaylı IP adreslerine izin ver, sahtekarlığına karşı koruma sağlar ve ACL'ler kullanan TCP bağlantıları kurmak için tasarlanmıştır.

Dış yük dengeleyici cihazları ağ adresi çevirisi (NAT) internet yönlendirilebilir IP'leri için Azure iç IP'ler gerçekleştirmek için erişim yönlendiriciler arkasında bulunur. Cihazları da iç IP ve bağlantı noktaları ve bunlar iç üretim ağ adres alanı gösterme sınırlamak için bir koruma mekanizması davranan geçerli üretim için paketleri yönlendirmek.

Varsayılan olarak, Microsoft oturum açma ve tüm trafiği bundan sonra dahil olmak üzere müşterilerin web tarayıcıları için iletilen tüm trafik için Köprü Metni Aktarım Protokolü güvenli (HTTPS) uygular. Güvenli bir tünel üzerinden akan trafiği için TLS 1.2 kullanımını etkinleştirir. Erişim ve çekirdek yönlendiriciler ACL'lerin trafik kaynağını beklenen değer ile tutarlı olduğundan emin olun.

Geleneksel güvenlik mimarisi için karşılaştırıldığında bu mimaride önemli bir ayrımdır adanmış donanım güvenlik duvarı olmadığından, özelleştirilmiş izinsiz giriş algılama önleme cihazlara veya normal olarak diğer güvenlik Gereçleri yoktur bağlantılar, Azure üretim ortamına yapılmadan önce bekleniyor. Müşteriler, Azure ağında genellikle bu donanım güvenlik duvarı cihazına beklediğiniz; Azure içinde çalışan ancak yok. Özellikle, bu güvenlik özellikleri sağlam, çok katmanlı güvenlik mekanizmaları, güvenlik duvarı özellikleri dahil olmak üzere sağlamak amacıyla Azure ortamı çalıştıran yazılımını oluşturulur. Ayrıca, sınır ve ilişkili genişlemesine yol açan kritik güvenlik cihazların kapsamını Azure çalışan yazılım tarafından yönetildiğinden, önceki resimde gösterildiği gibi yönetip envanter, daha kolay olur.

## <a name="core-security-and-firewall-features"></a>Temel Güvenlik ve Güvenlik Duvarı Özellikleri
Azure, güçlü yazılım güvenliği ve güvenlik duvarı özellikleri genellikle geleneksel bir ortamda çekirdek güvenlik yetkilendirme sınır koruma beklenen güvenlik özellikleri uygulamak için çeşitli düzeylerde uygular.

### <a name="azure-security-features"></a>Azure güvenlik özellikleri
Azure ana bilgisayar tabanlı bir yazılım güvenlik duvarları üretim ağı içinde uygular. Birkaç güvenlik çekirdek ve güvenlik duvarı özellikleri çekirdek Azure ortamı içinde bulunur. Bu güvenlik özellikleri Azure ortamındaki bir derinlemesine savunma stratejisi yansıtır. Azure'da müşteri verilerini aşağıdaki güvenlik duvarları tarafından korunur:

**Hiper yönetici Güvenlik Duvarı (paket filtresi)** : Bu güvenlik duvarı hiper yönetici içine uygulanan ve yapı denetleme (FC) aracısı ile yapılandırılmış. Bu güvenlik duvarı Kiracı VM içinde çalışan yetkisiz erişime karşı korur. Varsayılan olarak, bir VM oluşturulduğunda, tüm trafik engellenir ve ardından FC Aracısı yetkili trafiğe izin verecek şekilde filtrede kurallar ve özel durumları ekler.

Burada programlanan iki kural kategorisi:

- **Makine Yapılandırması veya altyapı kuralları**: Varsayılan olarak, tüm iletişim engellenir. Özel durumlar, dinamik konak Yapılandırma Protokolü (DHCP) iletişimleri ve DNS bilgilerini göndermek ve almak için bir VM sağlayan ve "Genel" internet'e işletim sistemi aktivasyon sunucularına ve FC küme içindeki diğer vm'lere giden trafik yok. Sanal makinelerin listesi, giden izin verilen hedefleri Azure yönlendirici alt ağları ve diğer Microsoft özellikleri içermez, kuralları bir koruma katmanı olarak bunlar için harekete.
- **Rol yapılandırma dosyası kuralları**: Kiracıların hizmet modelini temel alarak gelen ACL'leri tanımlar. Örneğin, bir kiracı, belirli bir VM'nin 80 numaralı bağlantı noktasında web ön ucu varsa, 80 numaralı bağlantı noktasını tüm IP adreslerine açılır. VM çalıştıran bir çalışan rolü varsa, çalışan rolü yalnızca aynı Kiracı içindeki VM için açıldı.

**Yerel ana bilgisayar güvenlik duvarı**: Azure Service Fabric ve Azure depolama, hiper yönetici bulunmayan bir yerel işletim sisteminde çalıştırmak ve bu nedenle, Windows Güvenlik Duvarı Yukarıdaki iki kural kümesiyle yapılandırılmıştır.

**Ana bilgisayar güvenlik duvarı**: Konak güvenlik duvarı hiper yöneticiyi çalıştıran ana bilgisayar bölümü korur. Kurallar, yalnızca FC izin vermek ve atlama kutularının ana bölüme belirli bir bağlantı noktasını kurmak için programlanmıştır. Diğer özel durumlar, DHCP ve DNS yanıtlarına izin vermek üzeresiniz. Azure ana bilgisayar bölümü için güvenlik duvarı kuralları şablonu içeren bir makine yapılandırma dosyasını kullanır. Bir ana bilgisayar güvenlik duvarı özel durumu bileşenlerini barındıracak, kablo sunucu ve belirli protokol/bağlantı noktası aracılığıyla meta veri sunucusu iletişim kurmak Vm'leri veren bulunmaktadır.

**Konuk Güvenlik Duvarı**: Müşteri VM ve depolama kullanan müşteriler tarafından yapılandırılabilir olan Windows Güvenlik Duvarı parçası konuk işletim sistemi.

Azure özelliklerini oluşturulan ek güvenlik özellikleri içerir:

- Dıps IP adresi atanmış olan altyapı bileşenleri. Internet üzerindeki bir saldırgan, Microsoft ulaşmak değil çünkü bu adresler için trafiği ele alamaz. Internet ağ geçidi yönlendirici, üretim ağı girmemeniz, böylece yalnızca iç adreslere gönderilen paketleri filtreleyin. Vıp'leri yönlendirilen trafiği kabul yalnızca yük Dengeleyiciler bileşenlerdir.
- Tüm iç düğümlerinde uygulanan güvenlik duvarları, herhangi bir verilen senaryo için üç birincil güvenlik mimarisi konuları vardır:

   - Güvenlik duvarları, yük dengeleyicinin arkasına yerleştirilen ve paketleri yerden kabul edin. Bu paketler, harici olarak kullanıma yöneliktir ve geleneksel çevre güvenlik duvarında bağlantı noktalarını açma karşılık gelir.
   - Güvenlik duvarları, paketleri yalnızca sınırlı sayıda adresleri kabul edin. Bu göz önünde bulundurarak DDoS saldırılarına karşı savunma ayrıntılı stratejisinin bir parçasıdır. Bu tür bağlantıları şifreli olarak doğrulanır.
   - Güvenlik duvarları, yalnızca select iç düğümlerinden erişilebilir. Bunlar, paketleri yalnızca kaynak IP adresleri, Azure ağında Dıps her biri, numaralandırılmış bir listesini kabul eder. Örneğin, istekleri bu adresler kurumsal ağ üzerindeki bir saldırı yönlendirebilir, ancak paket kaynak adresini bir Azure ağı içinde listelenmiş listesinde olduğu sürece saldırıları engellenebilir.
     - Çevre erişim yönlendiricide nedeniyle, yapılandırılmış statik yollar, Azure ağında bir adrese gönderilen giden paketleri engeller.

## <a name="next-steps"></a>Sonraki adımlar
Microsoft Azure altyapısının güvenliğini sağlamak için yaptığı hakkında daha fazla bilgi için bkz:

- [Azure özellikleri, şirket içi ve fiziksel güvenlik](azure-physical-security.md)
- [Azure altyapı kullanılabilirlik](azure-infrastructure-availability.md)
- [Azure Information sistem bileşenleri ve sınırlar](azure-infrastructure-components.md)
- [Azure ağ mimarisi](azure-infrastructure-network.md)
- [Azure SQL veritabanı güvenlik özellikleri](azure-infrastructure-sql.md)
- [Azure Üretim Operasyon ve Yönetimi](azure-infrastructure-operations.md)
- [Azure altyapı izleme](azure-infrastructure-monitoring.md)
- [Azure altyapı bütünlüğü](azure-infrastructure-integrity.md)
- [Azure müşteri verilerini koruma](azure-protection-of-customer-data.md)
