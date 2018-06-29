---
title: Azure olanakları, şirket içi ve fiziksel güvenlik | Microsoft Docs
description: Bu makalede, fiziksel altyapı, güvenlik ve uyumluluk teklifleri dahil olmak üzere Azure veri merkezleri anlatılmaktadır.
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
ms.openlocfilehash: a6a9b1d6e12dabb09cde684c34481b4b3442c1b8
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37102658"
---
# <a name="azure-facilities-premises-and-physical-security"></a>Azure olanakları, şirket içi ve fiziksel güvenlik
Microsoft'un bulut den oluşur bir [genel olarak dağıtılmış veri merkezi altyapı](https://azure.microsoft.com/global-infrastructure/) güvenli tesis dünya çapında Çevrimiçi Hizmetler binlerce destekleme ve 100'den fazla yüksek oranda kapsayıcı.

Altyapı veri residency koruma ve müşteriler için kapsamlı uyumluluk ve dayanıklılık seçenekleri sunan tüm dünyada kullanıcılara yakın uygulama getirmek için tasarlanmıştır. Azure 52 bölgeler dünya çapında sahiptir ve 140 ülkelerde de kullanılabilir.

Bir bölge, büyük ve esnek bir ağ üzerinden birbirine veri merkezleri kümesidir. Ağ içerik dağıtımı, Yük Dengeleme, artıklık ve şifreleme varsayılan olarak içerir. Tüm bulut sağlayıcıları arasında en fazla küresel bölgeye sahip olan Azure, müşterilere uygulamalarını gerekli konumlara dağıtma esnekliği sunar.

Azure bölgeleri coğrafyalar halinde düzenlenir. Her Azure coğrafyası, veri yerleşimi, bağımsızlık, uyumluluk ve dayanıklılık gereksinimlerinin ilgili coğrafi bölge içinde karşılanmasını sağlar.

Coğrafyalar, özel veri yerleşikliği ve uyumluluk gereksinimleri olan müşterilerin verilerini ve uygulamalarını yakında tutmasına imkan tanır. Farklı coğrafyalara tam bölge hatası adanmış yüksek kapasiteli ağ altyapısı için kendi bağlantısı üzerinden dayanacak şekilde hataya.

Kullanılabilirlik Alanları, bir Azure bölgesi içinde fiziksel olarak birbirinden ayrı konumlardır. Her Kullanılabilirlik Alanı, bağımsız güç, soğutma ve ağ bağlantısı ile donatılmış bir veya daha fazla veri merkezinden oluşur. Kullanılabilirlik Alanları, müşterilerin yüksek kullanılabilirlik ve düşük gecikme süreli çoğaltma ile görev açısından kritik uygulamalar çalıştırmasına olanak tanır.

Aşağıdaki şekil, nasıl Azure genel altyapı bölge ve kullanılabilirlik bölgeleri yüksek kullanılabilirlik, olağanüstü durum kurtarma ve yedekleme için aynı veri residency sınırları içinde çiftleri gösterir.

![Veri residency sınır][1]

Veri merkezleri büyük coğrafi dağıtılmış ayak Microsoft ağ gecikmesini azaltmak ve coğrafi olarak yedekli yedekleme ve yük devretme için izin vermek için müşteriler yakın olmasını sağlar.

## <a name="physical-security"></a>Fiziksel güvenlik
Microsoft tasarlarken, oluşturur ve veri merkezleri kesinlikle Müşteri verilerinin depolandığı alanlara fiziksel erişimi denetleyen bir şekilde çalışır. Microsoft müşteri verileri korumaya önemini anlar ve verilerinizi içeren veri merkezleri güvenli yardımcı olmak için çalışıyoruz. Microsoft'un tasarlamayı, derlemeyi ve Azure destek fiziksel olanaklar işletim için ayrılan tüm bir bölme sahip. Bu takım durumu resim fiziksel güvenlik bakımıyla yatırım.

Microsoft, veri ve veri merkezi kaynaklarını fiziksel erişimini yetkisiz kullanıcıların riskini azaltmak için fiziksel güvenlik için katmanlı yaklaşımın alır. Microsoft tarafından yönetilen veri merkezlerine sahip kapsamlı koruma katmanları: erişim yapı 's çevre, yapı içinde ve datacenter kat tesis 's çevre onay. Fiziksel güvenlik katmanları şunlardır:

- Erişim isteği ve onay – veri merkezi ulaşan önce erişim istemeniz gerekir. Bir siteyi ziyaret ettiğinizde, uyumluluk veya denetim amacıyla gibi geçerli iş gerekçe isteniyor. Tüm istekleri Microsoft çalışanlar tarafından erişmeniz için temelinde onaylanır. Erişmeniz için temel veri merkezlerinde tam en az bir görevi tamamlamak için gereken kişiler sayısı tutmaya yardımcı olur. İzin verildiğinde, bir kişinin yalnızca onaylı iş gerekçesinin üzerinde tabanlı veri merkezi ayrık alanına erişebilir. İzinler zaman bir belirli bir süre için sınırlıdır ve izin verilen süre sonunda süresi dolacak.

- Bir veri merkezinde geldiğinde tesis'ın çevre -, iyi tanımlanmış erişim noktası üzerinden gitmesi gerekir. Genellikle, çelik ve somut oluşan uzun dilimleri çevre her inç kapsar. Kameralar geçici veri merkezleri ile bunların videolar 7/24 ve yılın 365 gün izleme güvenlik ekibine vardır.

- Yapı giriş - datacenter girişinin ayrıntılı eğitim ve arka plan denetimleri yapılması profesyonel güvenlik görevlileri ile çalıştığı. Ayrıca veri merkezinin 7/24 ve yılda 365 gün içinde kameralar videoları izlerken bu güvenlik görevlileri datacenter da düzenli olarak patrol.

- Yapı içinde - yapı girdikten sonra veri merkezi taşıma devam etmek için Biyometri ile iki öğeli kimlik doğrulaması geçmesi gerekir. Kimliğinizi doğrulanırsa, erişimi onaylanmasını datacenter bölümünü girebilirsiniz. Yalnızca onaylanan zaman süresince var. kalabilir.

- Veri Merkezi kat – girmek için onaylanmış yalnızca kat izin verilir. Tam gövde metal algılama filtreleme geçirmek için gerekli değildir. Girme veya datacenter bizim bilginiz dışında bırakmak yetkisiz veri riskini azaltmak için yalnızca onaylı cihazlar datacenter kat kendi şekilde yapabilirsiniz. Ayrıca, video kamera İzleyici ön ve arka her sunucunun raf. Veri Merkezi kat çıktığınızda tam gövde metal algılama filtreleme yinelenir. Veri merkezi olarak bırakmak için ek güvenlik tara geçirmek isteniyor.

Ziyaretçilerin rozetleri ayrılma herhangi bir Microsoft tesisine öğesinden sonra surrender için gereklidir.

## <a name="physical-security-reviews"></a>Fiziksel güvenlik değerlendirmeleri
Fiziksel güvenlik değerlendirmeleri olanaklarının veri merkezleri adresi Microsoft Azure güvenlik gereksinimlerini düzgün şekilde sağlamak için düzenli olarak gerçekleştirilir. Veri Merkezi barındırma sağlayıcısı personel Microsoft Azure hizmet yönetimi sağlamaz. Personel Azure sistemlere oturum açma erişimi veya kafesleri bulunur ve Azure düzenleme yer fiziksel erişimi yok.

## <a name="data-bearing-devices"></a>Veri şifrelemeyle aygıtları
Microsoft, en iyi yöntem yordamları ve wiping bir çözüm kullanır [NIST 800-88 uyumlu](https://csrc.nist.gov/publications/detail/sp/800-88/archive/2006-09-01). Silinemez sabit sürücüler için yok eder ve kurtarma bilgilerinin imkansız işleyen bir yok etme işleminde kullanılır. Yok etme işlemi disintegrate, parçalara ayır, pulverize olabilir veya incinerate. Elden uygun çeşit varlık türüne göre belirlenir. Yok etme kayıtları korunur.  

## <a name="equipment-disposal"></a>Donanımı elden
Microsoft Azure bu ilkeye müşteriler adına uygular. Sistemin son yaşam sonra Microsoft işletimsel personeli ayrıntılı veri işleme ve müşteri verilerini içeren donanım güvenilmeyen taraflara kullanılamayacağı olduğunu güvence altına almak için donanımı elden yordamları izleyin. Güvenli silme bir yaklaşım destekleyen sürücüleri için (sabit sürücü bellenimini) izler. Silinemez sabit sürücüler için yok eder ve kurtarma bilgilerinin imkansız işleyen bir yok etme işleminde kullanılır. Yok etme işlemi disintegrate, parçalara ayır, pulverize olabilir veya incinerate. Elden uygun çeşit varlık türüne göre belirlenir. Yok etme kayıtları korunur. Tüm Microsoft Azure hizmetlerini onaylanan medya depolama ve elden Yönetim Hizmetleri'ni kullanın.

## <a name="compliance"></a>Uyumluluk
Azure altyapı tasarlanmış ve geniş bir ISO 27001, HIPAA, FedRAMP, SOC 1 ve SOC 2 gibi uluslararası ve sektöre özel uyumluluk standartlarını karşılamak üzere yönetilir. Ülkeye özel standartları Ayrıca, Avustralya IRAP ve İngiltere G-bulut Singapur MTCS çeşitli karşılıyor. İngiliz Standartları Enstitüsü gibi üçüncü taraflarca yapılan sıkı denetimler, Azure’ın bu standartların zorunlu kıldığı katı güvenlik denetimlerine olan uyumluluğunu doğrulamıştır.

Bkz: [uyumluluk teklifleri](https://www.microsoft.com/trustcenter/compliance/complianceofferings) uyumluluk standartlarını tam listesi için Azure tarafından bağlı için.

## <a name="next-steps"></a>Sonraki adımlar
Microsoft Azure altyapı güvenli hale getirmek için yaptığı hakkında daha fazla bilgi için bkz:

- [Azure altyapı kullanılabilirliği](azure-infrastructure-availability.md)
- [Azure Information sistem bileşenleri ve sınırlar](azure-infrastructure-components.md)
- [Azure ağ mimarisi](azure-infrastructure-network.md)
- [Azure üretim ağı](azure-production-network.md)
- [Microsoft Azure SQL veritabanı güvenlik özellikleri](azure-infrastructure-sql.md)
- [Azure üretim işlemleri ve Yönetimi](azure-infrastructure-operations.md)
- [Azure altyapı izleme](azure-infrastructure-monitoring.md)
- [Azure altyapı bütünlüğü](azure-infrastructure-integrity.md)
- [Azure müşteri verileri koruma](azure-protection-of-customer-data.md)

<!--Image references-->
[1]: ./media/azure-physical-security/data-residency-boundary.png
