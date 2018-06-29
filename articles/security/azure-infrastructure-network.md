---
title: Azure ağ mimarisi
description: Bu makalede, Microsoft Azure altyapı ağı genel bir açıklamasını sağlar.
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
ms.openlocfilehash: 67781f196b445c9330e0dcb1fc7d8b0a1a53cbc0
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37102625"
---
# <a name="azure-network-architecture"></a>Azure ağ mimarisi
Azure ağı mimarisi endüstri standart dağıtım/Core/erişim modeliyle, farklı donanım katmanları değiştirilmiş bir sürümünü izler. Katmanları içerir:

- Çekirdek (Datacenter yönlendirici)
- Dağıtım (erişim yönlendiriciler ve L2 toplama) - L2 geçiş L3 yönlendirme dağıtım katman ayırır
- Erişim (L2 ana anahtarlar)

Ağ mimarisi Katman 2 anahtarları, diğer trafik toplayarak bir katman ve artıklık eklemenizi 2 döngüler Katman İki düzeyde vardır. Bu, daha esnek bir VLAN ayak izini gibi fayda sağlar ve bağlantı noktası ölçeklendirme artırır. Mimari L2 ve her ağ farklı katmanlar donanım kullanılmasına izin verir ve diğer Katmanlar etkilemesini bir katmandaki hata en aza indirir L3 ayrı tutar. Kaynak bağlantısı L3 altyapısına gibi paylaşımı santrallerine kullanımını sağlar.

## <a name="network-configuration"></a>Ağ yapılandırması
Bir veri merkezinde bulunan bir Microsoft Azure küme ağ mimarisini aşağıdaki aygıtından oluşur:

- Yönlendirici (Datacenter, erişim yönlendiricisi ve sınır yaprak yönlendiricileri)
- Anahtarları (toplama ve raf anahtarları üstündeki)
- Digi CMs
- PDU veya Nucleons

Aşağıdaki şekilde bir küme içinde Azure'nın ağ mimarisinin üst düzey bir genel bakış sağlar. Azure iki ayrı mimarileri sahiptir. Yeni bölgeler ve sanal müşterileri Zamanlayıcının 10 (Q10) mimarisi erişilen ancak bazı var olan Azure müşterileri ve paylaşılan hizmetler varsayılan LAN mimarisi (DLA) bulunur. Etkin pasif erişim yönlendiriciler geleneksel ağaç tasarımıyla DLA mimaridir ve güvenlik ACL erişim yönlendiricilerine uygulanır. Kuantum 10 mimarisi, ACL'ler yönlendiricilerde ancak yazılım yük dengeleyici (SLB) aracılığıyla yönlendirme aşağıda uygulanmaz veya VLAN'ları yazılım tanımlı olduğu yönlendiriciler clos/kafes tasarımdır.

![Azure ağı][1]

### <a name="quantum-10-devices"></a>Kuantum 10 cihazları
Kuantum 10 tasarım Katman 3 anahtarlama yayılan bir CLOS/kafes tasarımda birden çok aygıt üzerinden yürütür. Büyük özellik ve mevcut ağ altyapınızda ölçeklendirmek için daha yüksek bir beceri Q10 tasarım avantajları şunlardır. Tasarım sınır yaprak yönlendiricileri, Sırt anahtarlar ve üstünde, raf trafiği hataya dayanıklılık için izin verme birden çok yol boyunca kümeye geçirmek için yönlendiriciler kullanır. Ağ adresi çevirisi işlenir gibi yazılım Yük Dengelemesi yerine aracılığıyla donanım aygıtlarında güvenlik hizmetleri.

### <a name="access-routers"></a>Erişim yönlendiricileri
Dağıtım/erişim L3 yönlendiricileri (ARs) için dağıtım ve erişim katmanları birincil yönlendirme işlevi gerçekleştirir. Bu cihazlar çifti olarak dağıtılan ve varsayılan ağ geçidi alt ağlar için olan. Her AR çifti kapasite bağlı olarak, birden çok L2 toplama anahtar çiftleri destekleyebilir. En fazla hata etki alanlarının yanı sıra aygıt kapasitesini bağlıdır. Bir Genel sayı göre beklenen kapasite AR çifti başına üç L2 toplama anahtar çifti olacaktır.

### <a name="l2-aggregation-switches"></a>L2 Toplama anahtarları  
Bu aygıtların L2 trafiği için bir toplama noktasına işlevini görür. Bunlar L2 doku için dağıtım katmandır ve büyük miktarda trafik işleyebilir. Bu aygıtların trafiği, 802.1Q toplama çünkü işlevselliği gereklidir ve bağlantı noktası toplama ve 10GE gibi yüksek bant genişliği teknolojileri kullanılır.

### <a name="l2-host-switches"></a>L2 Konak anahtarları
Ana bilgisayarlar bu anahtarları doğrudan bağlanın. Bunlar, rafa takılan anahtarları veya kasa dağıtımları olabilir. Bir VLAN ataması yerel bir VLAN olarak (etiketlenmemiş) Ethernet çerçeveleme normal olarak, VLAN değerlendirmesini standart 802.1Q sağlar. Normal koşullar altında yerel VLAN çerçeve aktarılan ve üzerinde bir 802.1Q etiketlenmemiş alınan santral bağlantı noktası. Bu özellik geçiş 802.1Q ve olmayan ile uyumluluk için tasarlanmıştır-802.1Q özellikli cihazların. Bu mimaride, yalnızca ağ altyapısı native VLAN kullanır.

Bu mimari, mümkün olduğunda AR aygıtları her santral için benzersiz bir yerel VLAN ve L2Aggregation santrallerine için L2Aggregation sahip olmasını sağlar yerel VLAN seçimi için bir standart belirtir. L2Aggregation L2Host anahtar santrallerine için varsayılan olmayan yerel bir VLAN vardır.

### <a name="link-aggregation-8023ad"></a>Bağlantı toplama (802.3ad)
Bağlantı toplama (GECİKME) birlikte gruplanır ve tek bir mantıksal bağlantı kabul birden çok bağımsız bağlantılarına izin verir. Bağlantı noktası kanalı arabirimleri belirlemek için kullanılan sayısını daha kolay hata ayıklama faaliyete geçirmek için ağın geri kalanı aynı numarası bir bağlantı noktası kanalı her iki uçta kullanacak standartlaştırılmış gerekiyor. Bu, sonraki kullanılabilir sayı tanımlamak için kullanılacak bağlantı noktası kanalı hangi sonuna belirlemek için standart bir gerektirir.

L2Host anahtara L2Agg için belirtilen sayılar L2Agg tarafında kullanılan bağlantı noktası kanalı numaralarıdır. Dizi numarası L2Host tarafında daha sınırlı olduğundan, standart numaraları 1 ve 2 L2Host tarafında "a" yan ve "b" için sırasıyla giden bağlantı noktası-kanal için başvurmak için kullanmaktır.

### <a name="vlans"></a>VLAN'ları
Ağ mimarisi VLAN'lar grubu sunucuları için tek bir yayın etki alanına birlikte kullanır. VLAN numaraları uygun için 802.1Q VLAN'ları desteklediğini standartları numaralı 1 – 4094.

### <a name="customer-vlans"></a>Müşteri VLAN'ları
Müşterilerin sahip çeşitli VLAN uygulama seçenekleri kendi çözümünü ayırma ve mimari ihtiyaçlarını karşılamak üzere Azure Portal dağıtabilirsiniz. Bu çözümler ile sanal makineleri dağıtılır. Müşteri başvurusunu mimarisi örnekler ziyaret [Azure başvuru mimarileri](https://docs.microsoft.com/azure/architecture/reference-architectures/).

### <a name="edge-architecture"></a>Edge mimarisi
Azure veri merkezlerinde yüksek oranda yedekli ve iyi sağlanan ağ altyapısında oluşturulur. Azure veri merkezleri içinde ağlar "gerek plus bir" uygulanır (N + 1) artıklık mimariler veya iyi olur. Tam Yük devretme Özellikler içinde ve veri merkezleri arasında ağ ve hizmet kullanılabilirliğini sağlamak için yardımcı olur. Veri merkezleri nedenle üzerinde 1200 Internet servis potansiyel kenar kapasite 2.000 Gbps aşan sağlayan sağlayıcıları genel olarak noktalarında birden çok eşleme özelliklerini bağlamak ayrılmış, yüksek bant genişlikli ağ bağlantı hatları tarafından harici olarak sunulur ağ üzerinden.

Microsoft Azure ağı kenarı ve erişim katmanında filtreleme yönlendiriciler Azure'a bağlanmak için yetkisiz girişimleri önlemek için paket düzeyinde iyi yerleşik güvenlik sağlar. Bunlar, paketlerin gerçek içeriği beklenen biçimde verileri içeren ve beklenen istemci/sunucu iletişimi düzenine uygun olmasını sağlamak için yardımcı olur. Azure aşağıdaki ağ arasında ayrım yapma ve erişim denetimi bileşenleri oluşan bir katmanlı mimarisi uygular:

- Yönlendiriciler kenar - Internet'ten uygulama ortamı tutma. Sınır yönlendiricileri, koruma sızma koruma sağlar ve erişim denetim listeleri (ACL'ler) kullanarak erişimi sınırlamak için tasarlanmıştır.
- Dağıtım (erişimi) yönlendiricileri - erişim yönlendiricileri tasarlanmış IP adresleri yalnızca Microsoft onaylı izin vermek için ACL'leri kullanarak yanıltma ve kurulan bağlantılar sağlar.

### <a name="a10-ddos-mitigation-architecture"></a>A10 DDOS azaltma mimarisi
Hizmet reddi saldırılarını devam etmek için Microsoft online services güvenilirliğini gerçek bir tehdit sunmak. Saldırıları daha hedeflenen hale gibi daha karmaşık ve daha coğrafi olarak dağıtılmış hale Microsoft'un hizmetleri tanımlamak ve en aza indirmek için etkili mekanizmalar özelliği olarak bu saldırıların yüksek bir öncelik etkisidir.

Aşağıdaki ayrıntıları A10 DDOS azaltma sistemi bir ağ mimarisi açısından nasıl uygulandığı açıklanmaktadır.  
Microsoft Azure A10 ağ aygıtlarına otomatik algılama ve azaltma sağlayan DCR (Datacenter yönlendirici) kullanır. A10 çözüm, Azure Net akışı paketleri örnek ve bir saldırının olup olmadığını belirlemek için ağ izlemeden yararlanır. Saldırı algılandığında, A10 aygıtları saldırıları azaltmak için scrubbers kullanılır. Azaltma sonra daha fazla temiz trafiğinin Azure veri merkezine doğrudan DCR izin verilir. A10 çözüm Azure ağ altyapısı korumak için kullanılır.

A10 çözümde DDoS korumalar içerir:

- UDP IPv4 ve IPv6 koruma bölgesini doldurmak
- ICMP IPv4 ve IPv6 koruma bölgesini doldurmak
- TCP IPv4 ve IPv6 koruma bölgesini doldurmak
- IPv4 ve IPv6 için TCP Eşitlemeye saldırı koruma
- Parçalanma saldırısı

> [!NOTE]
> DDoS Koruması varsayılan olarak tüm Azure müşterilerine sağlanmıştır.
>
>

## <a name="network-connection-rules"></a>Ağ bağlantı kuralları
Azure Microsoft Azure'a bağlanmak için yetkisiz girişimleri önlemek için paket düzeyinde güvenlik sağlayan sınır yönlendiricileri, ağ üzerinde dağıtır. Bunlar, paketlerin gerçek içeriği beklenen biçimde verileri içeren ve beklenen istemci/sunucu iletişimi düzenine uygun emin olun.

Sınır yönlendiricileri, Internet'ten uygulama ortamı tutma. Sınır yönlendiricileri, koruma sızma koruma sağlar ve erişim denetim listeleri (ACL'ler) kullanarak erişimi sınırlamak için tasarlanmıştır. Bu sınır yönlendiricileri, bir katmanlı ACL yaklaşım sınır yönlendiricileri transit ve yönlendiriciler erişmek için izin verilen sınırı ağ protokolleri kullanılarak yapılandırılır.

Ağ aygıtlarına erişim ve kenar konumlarda konumlandırılmış ve giriş ve/veya çıkış filtreleri nereye uygulanacağını sınır noktaları olarak davranacak.

## <a name="next-steps"></a>Sonraki adımlar
Microsoft Azure altyapı güvenli hale getirmek için yaptığı hakkında daha fazla bilgi için bkz:

- [Azure olanakları, şirket içi ve fiziksel güvenlik](azure-physical-security.md)
- [Azure altyapı kullanılabilirliği](azure-infrastructure-availability.md)
- [Azure Information sistem bileşenleri ve sınırlar](azure-infrastructure-components.md)
- [Azure üretim ağı](azure-production-network.md)
- [Microsoft Azure SQL veritabanı güvenlik özellikleri](azure-infrastructure-sql.md)
- [Azure üretim işlemleri ve Yönetimi](azure-infrastructure-operations.md)
- [Azure altyapı izleme](azure-infrastructure-monitoring.md)
- [Azure altyapı bütünlüğü](azure-infrastructure-integrity.md)
- [Azure müşteri verileri koruma](azure-protection-of-customer-data.md)

<!--Image references-->
[1]: ./media/azure-infrastructure-network/network-arch.png
