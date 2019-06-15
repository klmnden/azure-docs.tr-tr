---
title: Azure ağ mimarisi
description: Bu makalede, Microsoft Azure altyapı ağı genel bir açıklamasını sağlar.
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
ms.date: 02/20/2019
ms.author: terrylan
ms.openlocfilehash: 48a7e52d4284e5c2db1d77d24d91fd4701aad8d7
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60587156"
---
# <a name="azure-network-architecture"></a>Azure ağ mimarisi
Azure ağ mimarisi sektör standart çekirdek/dağıtım/erişim modeliyle, farklı donanım katmanları değiştirilmiş bir sürümünü izler. Katmanlar şunlardır:

- Çekirdek (veri merkezi yönlendiriciler)
- Dağıtım (erişim yönlendiriciler ve L2 toplama). Dağıtım katman L2 geçiş yapmasını L3 yönlendirme ayırır.
- (L2 konak anahtarlar) erişim

Ağ mimarisi, iki düzeyde bir katman 2 anahtarları vardır. Bir katman bir katmandan diğerine trafiği toplar. İkinci Katman yedeklilik birleştirmek için döngüde kalır. Mimari, daha esnek bir VLAN ayak izini sağlar ve bağlantı noktası ölçeklendirmeyi artırır. Mimari L2 ve ayrı katmanlar ağdaki her donanım kullanımına izin verir ve diğer Katmanlar etkilemesini bir katmandaki hata en aza indirir L3 ayrı tutar. Kaynak paylaşımı gibi altyapı L3 bağlantısını için trunks kullanılmasını sağlar.

## <a name="network-configuration"></a>Ağ yapılandırması
Aşağıdaki cihazlar bir veri merkezinde Azure kümesine ağ mimarisini oluşur:

- Yönlendirici (veri merkezi, erişim yönlendirici ve kenarlık yaprak yönlendirici)
- Anahtarları (toplama ve üst raf anahtarlar)
- Digi CMs
- Güç Dağıtım birimleri

Azure, iki ayrı mimarileri sahiptir. Yeni bölgeler ve sanal müşterileri Kuantum 10 (Q10) mimarisinde ise bazı mevcut Azure müşterilerinin ve paylaşılan hizmetler varsayılan LAN mimarisi (DLA) bulunur. Aktif/Pasif erişim yönlendiriciler ve erişim yönlendiricilerine uygulanan güvenlik erişim denetim listeleri (ACL) ile bir geleneksel ağaç tasarım DLA mimaridir. Kuantum 10 yönlendiriciler, Kapat/kafes tasarımını burada ACL'leri yönlendiriciler uygulanmaz mimaridir. Bunun yerine, yazılım Yük Dengelemesi (SLB) aracılığıyla yönlendirme altında ACL uygulanır veya VLAN'ları yazılım tanımlı.

Aşağıdaki diyagramda Azure kümesine ağ mimarisini üst düzey bir genel bakış sağlar:

![Azure Ağ Diyagramı][1]

### <a name="quantum-10-devices"></a>Kuantum 10 cihazları
Kuantum 10 tasarım Katman 3 geçiş yayılan bir Clos/ağ tasarımı birden çok cihazda üzerinden yapmaktadır. Daha büyük özellik ve mevcut ağ altyapınızda ölçeklendirmek için daha yüksek bir beceri Q10 tasarım avantajları içerir. Tasarım sınır yaprak yönlendiricileri, Sırt anahtarlar ve üst raf yönlendiriciler arasında birden çok yol, hataya dayanıklılık için izin kümelerine trafiği geçirmek için kullanır. Yazılım Yük Dengeleme, donanım cihazları yerine güvenlik hizmetleri ağ adresi çevirisi gibi işler.

### <a name="access-routers"></a>Erişim yönlendiricileri
Dağıtım/erişim L3 yönlendiricileri (ARs) için dağıtım ve erişim katmanları birincil yönlendirme işlevini gerçekleştirir. Bu cihazların bir çift olarak dağıtılır ve varsayılan ağ geçidi alt ağları olan. Birden çok L2 toplama anahtar çifti, kapasite bağlı olarak her AR çifti destekler. En fazla hata etki alanları yanı sıra cihaz kapasitesini bağlıdır. AR çifti başına üç L2 toplama anahtar çifti olan tipik bir sayıdır.

### <a name="l2-aggregation-switches"></a>L2 toplama anahtarları  
Bu cihazlar L2 trafiği için bir toplama noktasına görür. Bunlar L2 doku için dağıtım katman ve büyük miktarda trafik işleyebilir. Bu cihazlar trafiğini toplama olduğundan, 802.1Q gerektirdikleri işlevsellik ve bağlantı noktası toplama ve 10GE gibi yüksek bant genişlikli teknolojilerin.

### <a name="l2-host-switches"></a>L2 konak anahtarları
Konaklar, bu anahtarlara doğrudan bağlanın. Bunlar, rafa monte edilen anahtarlar veya kasa dağıtımları olabilir. Bu VLAN normal (etiketlenmemiş) Ethernet çerçeve olarak davranılması için bir VLAN atamasını olarak yerel bir VLAN 802.1Q standart sağlar. Normal koşullar altında yerel VLAN karelerden aktarılan ve üzerinde bir 802.1Q etiketlenmemiş alınan santral bağlantı noktası. Bu özellik, 802.1Q ve uyumlu olmayan geçiş için tasarlanmıştır-802.1Q özellikli cihazlarda. Bu mimaride, yerel VLAN yalnızca ağ altyapısını kullanır.

Bu mimari, standart bir yerel VLAN seçimi belirtir. Standart sağlar, mümkün olduğunda, AR cihazların her santral için benzersiz, yerel bir VLAN ve L2Aggregation L2Aggregation trunks için gereken. Varsayılan olmayan yerel VLAN L2Aggregation L2Host anahtar trunks için var.

### <a name="link-aggregation-8023ad"></a>(802.3ad) bağlantı toplama
Bağlantı toplama birlikte ve tek bir mantıksal bağlantı kabul birden fazla bireysel bağlantılara izin verir. İşletimsel hata ayıklamayı kolaylaştırmak için bağlantı noktası kanalı arabirimleri belirlemek için kullanılan numarasını standartlaştırılmış. Ağın geri kalanı aynı sayıda bağlantı noktasına kanal her iki ucunda kullanır.

L2Agg L2Host geçiş için belirtilen sayılar, L2Agg tarafında kullanılan bağlantı noktası kanalı sayılardır. Dizi numarası L2Host tarafında daha sınırlı olduğundan, standart L2Host tarafında sayı 1 ve 2 kullanmaktır. Bu bağlantı noktası kanalı giderek "a" yan ve "b" yan sırasıyla bakın.

### <a name="vlans"></a>VLAN'ları
Ağ mimarisi VLAN'lar sunucuları gruplandırmanızı birlikte tek bir yayın etki alanına kullanır. VLAN numaraları destekleyen numaralı 1 – 4094 VLAN 802.1Q için standart, uygun.

### <a name="customer-vlans"></a>Müşteri VLAN'ları
Sahip olduğunuz çeşitli VLAN uygulama seçenekleri çözümünüzü ayrımı ve mimari ihtiyaçlarını karşılamak için Azure Portalı aracılığıyla dağıtabilirsiniz. Bu çözümleri aracılığıyla sanal makineler dağıtın. Müşteri başvuru mimarisi örnek için bkz: [Azure başvuru mimarileri](https://docs.microsoft.com/azure/architecture/reference-architectures/).

### <a name="edge-architecture"></a>Edge mimarisi
Azure veri merkezleri, yüksek düzeyde yedekli ve iyi sağlanan ağ altyapıları üzerinde oluşturulur. Microsoft Azure veri merkezleri "gerek artı bir" Ağ uygular (N + 1) yedeklilik mimariler veya daha iyi. Tam Yük devretme özellikleri içinde ve veri merkezleri arasında ağ ve hizmet kullanılabilirliği sağlamaya yardımcı olur. Harici olarak veri merkezleri tarafından ayrılmış olan, yüksek bant genişliğine sahip ağ devreler sunulur. Bu bağlantı hatları özellikleri üzerinde 1200 internet hizmet sağlayıcıları genel olarak noktalarda birden çok eşleme ile nedenle bağlanın. Bu, ağ üzerinden aşan 2.000 Gbps olası edge kapasitesi sağlar.

Filtreleme yönlendiricileri Azure ağının edge ve erişim katmanında, paket düzeyinde tanınmış güvenliği sağlar ve Azure'a bağlanmak için yetkisiz girişimleri engellemeye yardımcı olur. Yönlendiriciler, paketlerin gerçek içeriği beklenen biçimde veri içeriyor ve beklenen istemci/sunucu iletişimini düzenine uygun emin olmak için yardımcı olur. Azure aşağıdaki ağ ayırma ve erişim denetimi bileşenleri oluşan bir katmanlı mimari uygular:

- **Uç yönlendiricileri.** Bu, internet'ten uygulama ortamı ayırabilirsiniz. Uç yönlendiricileri sızma virüsten koruma sağlar ve ACL'leri kullanarak erişimi sınırlamak üzere tasarlanmıştır.
- **Dağıtım (erişimi) yönlendiricileri.** Bu IP adresleri yalnızca Microsoft onaylı izin, sahtekarlığına karşı koruma sağlar ve ACL'leri kullanarak bağlantı kurabilir.

### <a name="ddos-mitigation"></a>DDOS riskini azaltma
Dağıtılmış Hizmet engelleme (DDoS) saldırılarının devam gerçek bir tehdit güvenilirlik Çevrimiçi Hizmetleri sunmak. Bu saldırıların etkisini yüksek öncelik taşır, saldırıları daha hedeflenen ve karmaşık hale gelir ve daha fazla farklı coğrafi olarak tanımlayan ve en aza Microsoft hizmetleri sağlar.

[Azure DDoS koruması standart](../virtual-network/ddos-protection-overview.md) DDoS saldırılarına karşı koruma sağlar. Bkz: [Azure DDoS koruması: En iyi uygulamalar ve başvuru mimarilerimize](azure-ddos-best-practices.md) daha fazla bilgi için.

> [!NOTE]
> Microsoft DDoS Koruması varsayılan olarak tüm Azure müşterileri için sağlar.
>
>

## <a name="network-connection-rules"></a>Ağ bağlantı kuralları
Alt ağda Azure Azure'a bağlanmak için yetkisiz girişimleri önlemek için paket düzeyinde güvenlik sağlayın kenar yönlendiricilerine dağıtır. Uç yönlendiricileri, paketlerin gerçek içeriği beklenen biçimde veriler içeriyor ve beklenen istemci/sunucu iletişimini düzenine uygun emin olun.

İnternet'ten uygulama ortamı uç yönlendiricileri ayırabilirsiniz. Bu yönlendiriciler, sızma virüsten koruma sağlar ve ACL'leri kullanarak erişimi sınırlamak üzere tasarlanmıştır. Microsoft uç yönlendiricileri geçiş ve yönlendiriciler erişmek için izin verilen sınırı ağ protokolleri için katmanlı bir ACL yaklaşımı kullanarak uç yönlendiricileri yapılandırır.

Microsoft ağ aygıtlarını girişi veya çıkışı filtreleri nereye uygulanacağını sınır noktaları olarak davranacak şekilde, erişim ve edge konumlara yerleştirir.

## <a name="next-steps"></a>Sonraki adımlar
Microsoft Azure altyapısının güvenliğini sağlamak için yaptığı hakkında daha fazla bilgi için bkz:

- [Azure özellikleri, şirket içi ve fiziksel güvenlik](azure-physical-security.md)
- [Azure altyapı kullanılabilirlik](azure-infrastructure-availability.md)
- [Azure Information sistem bileşenleri ve sınırlar](azure-infrastructure-components.md)
- [Azure üretim ağı](azure-production-network.md)
- [Azure SQL veritabanı güvenlik özellikleri](azure-infrastructure-sql.md)
- [Azure Üretim Operasyon ve Yönetimi](azure-infrastructure-operations.md)
- [Azure altyapı izleme](azure-infrastructure-monitoring.md)
- [Azure altyapı bütünlüğü](azure-infrastructure-integrity.md)
- [Azure müşteri verilerini koruma](azure-protection-of-customer-data.md)

<!--Image references-->
[1]: ./media/azure-infrastructure-network/network-arch.png
