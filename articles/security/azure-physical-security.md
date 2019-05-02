---
title: Azure veri merkezleri - Microsoft Azure'nın fiziksel güvenlik | Microsoft Docs
description: Bu makalede, Microsoft Azure Veri merkezlerindeki fiziksel altyapı, güvenlik ve uyumluluk teklifleri de dahil olmak üzere, güvenliğini sağlamak için yaptığı anlatılmaktadır.
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
ms.date: 04/28/2019
ms.author: terrylan
ms.openlocfilehash: d1b95695de809668987ebb6ef6720a3751205171
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64939841"
---
# <a name="azure-facilities-premises-and-physical-security"></a>Azure özellikleri, şirket içi ve fiziksel güvenlik
Bu makalede, Azure altyapısının güvenliğini sağlamak için Microsoft ne yaptığını açıklar.

## <a name="datacenter-infrastructure"></a>Veri Merkezi altyapı
Azure oluşan bir [Global olarak dağıtılmış veri merkezi altyapı](https://azure.microsoft.com/global-infrastructure/), Çevrimiçi Hizmetler binlerce destekleyen ve dünya çapında 100'den fazla yüksek güvenlikli tesisten kapsayıcı.

Altyapı, veri yerleşimini korur ve müşterilere kapsamlı uyumluluk ve dayanıklılık seçenekleri sunarak dünyanın dört bir yanındaki kullanıcılara yaklaştırılması uygulama getirmek için tasarlanmıştır. Azure bölgeleri 52 dünya çapında sahiptir ve 140 ülkede/bölgede kullanılabilir.

Bir bölge çok büyük ve esnek bir ağ birbirine veri merkezleri kümesidir. Ağ, içerik dağıtımı, Yük Dengeleme, yedeklilik ve şifreleme varsayılan olarak içerir. Diğer tüm bulut sağlayıcılarından daha fazla küresel bölgeye sahip Azure, uygulamaları dağıtmak için esnekliği yerden erişilebilmesini sağlar.

Azure bölgeleri coğrafyalar halinde düzenlenir. Her Azure coğrafyası, veri yerleşimi, bağımsızlık, uyumluluk ve dayanıklılık gereksinimlerinin ilgili coğrafi bölge içinde karşılanmasını sağlar.

Coğrafyalar, özel veri yerleşikliği ve uyumluluk gereksinimleri olan müşterilerin verilerini ve uygulamalarını yakında tutmasına imkan tanır. Coğrafyalar, adanmış ve yüksek kapasiteli ağ altyapısı aracılığıyla bölge hatası dayanacak şekilde hataya.

Kullanılabilirlik alanları, bir Azure bölgesinin içinde fiziksel olarak ayrı konumlardır. Her kullanılabilirlik alanı, soğutma ve ağ bağımsız güç ile donatılmış bir veya daha fazla veri merkezlerinden oluşur. Kullanılabilirlik alanları, yüksek kullanılabilirlik ve düşük gecikme süreli çoğaltma ile görev açısından kritik uygulamalar çalıştırmanıza olanak tanır.

Küresel Azure altyapısı bölge ve kullanılabilirlik bölgeleri yüksek kullanılabilirlik, olağanüstü durum kurtarma ve yedekleme için aynı veri yerleşimi sınırları içinde nasıl çiftlerini aşağıdaki şekilde gösterilmiştir.

![Diyagram gösteren veri yerleşikliği sınır][1]

Coğrafi olarak dağıtılmış veri merkezlerinde Microsoft müşterileri, ağda gecikme süresini azaltın ve coğrafi olarak yedekli ve yük devretme için izin vermek için yakın olmasını sağlar.

## <a name="physical-security"></a>Fiziksel güvenlik
Microsoft tasarlar, oluşturur ve veri merkezleri kesinlikle verilerinizin depolandığı alanlara fiziksel erişimi denetleyen bir şekilde çalışır. Microsoft, verilerinizi korumanın önemini anlar ve verilerinizin bulunduğu veri merkezlerine güvenli yardımcı olmaya kararlıdır. Microsoft'ta tasarlama, oluşturma ve Azure'ı destekleyen fiziksel tesisler çalıştırma için ayrılan tüm bir bölme sahibiz. Bu takım, fiziksel güvenlik durumu resim sürdürmeye yatırım.

Microsoft, veri ve veri merkezi kaynaklarını fiziksel erişim elde yetkisiz kullanıcıların riskini azaltmak için fiziksel güvenlik için katmanlı bir yaklaşım alır. Microsoft tarafından yönetilen veri merkezlerinden oluşan kapsamlı bir katman vardır: onay yapı'nın çevre bina içine ve veri merkezi katında tesis'ın çevre erişim. Fiziksel güvenlik katmanları şunlardır:

- **Erişim isteği ve onay.** Önce veri merkezinde gelen erişim isteği göndermelidir. Uyumluluk veya denetim amacıyla gibi ziyaret ettiğiniz için geçerli iş gerekçesi sağlayabilirsiniz isteniyor. Tüm istekleri erişmeniz için temelinde Microsoft çalışanları tarafından onaylanır. Erişmeniz için temel veri merkezlerindeki için en az bir görevi tamamlamak için gereken kişilerin sayısını önler. Microsoft izni verir sonra bireysel erişim yalnızca gerekli veri merkezinin ayrık alana sahip. onaylı iş gerekçelendirme bağlı. İzinleri zaman bir belirli bir süre için sınırlıdır ve sonra süresi dolar.

- **Tesis'ın çevre.** Bir veri merkezinde geldiğinde, iyi tanımlanmış bir erişim noktası üzerinden Git isteniyor. Genellikle, her inç çevre çelik ve somut oluşan uzun sınırlar kapsayabilir. Veri merkezleri ile onların videolarını izleme her zaman bir güvenlik ekibi etrafında kamera vardır.

- **Yapı giriş.** Veri Merkezi giriş zorlu eğitim ve arka plan denetimleri yapılmıştır profesyonel güvenlik sorumluları ile çalıştığı. Bu güvenlik sorumluları düzenli olarak da veri merkezi patrol ve her zaman veri merkezi içinde kameralar videoları izleyin.

- **Yapı içinde.** Yapı girdikten sonra iki öğeli kimlik doğrulama ile veri merkezi taşıma devam etmek için Biyometri geçmesi gerekir. Kimliğinizin doğrulanmış olması durumunda, yalnızca veri merkezi erişimi onayladığınız bölümü girebilirsiniz. Yalnızca onaylanan saat boyunca var kalabilir.

- **Veri Merkezi katın.** Yalnızca girmek için onayından katın izin verilir. Tam gövdesi metal algılama filtreleme geçirmek için gerekli değildir. Yalnızca onaylı cihazlar girerek veya veri merkezi bizim bilginiz dışında bırakmak verilere yetkisiz riskini azaltmak için veri merkezi kat aşamalarından yapabilirsiniz. Buna ek olarak, video kamera İzleyici ön ve arka her sunucunun rafa. Veri Merkezi kat çıktığınızda tam gövdesi metal algılama filtreleme yoluyla yeniden geçmesi gerekir. Veri merkezi olarak bırakmak için bir ek güvenlik tara geçirmek isteniyor.

Microsoft, herhangi bir Microsoft tesisten kalkış üzerine rozetleri surrender ziyaretçilerin gerektirir.

## <a name="physical-security-reviews"></a>Fiziksel güvenlik incelemeleri
Düzenli aralıklarla veri merkezleri, adres Azure güvenlik gereksinimlerini doğru emin olmak için özellikleri, fiziksel güvenlik değerlendirmelerini yürütün. Veri Merkezi barındırma sağlayıcısı personeli, Azure hizmet yönetimi sağlamaz. Personeli, Azure sistemlerine oturum açamıyorum ve fiziksel kafesleri bulunur ve Azure ulaşmamak odası erişiminiz yok.

## <a name="data-bearing-devices"></a>Veri seçtiğiniz cihazlar
Microsoft, en iyi yöntem yordamları ve wiping bir çözüm kullanır [NIST 800-88 uyumlu](https://csrc.nist.gov/publications/detail/sp/800-88/archive/2006-09-01). Silinemez sabit sürücüler için yok eder ve kurtarma bilgilerinin mümkün olmayan işler yıkım işlemini kullanırız. Bu yok etme işlemi disintegrate, içinse, pulverize veya incinerate olabilir. Varlık türüne göre elden çeşit belirleriz. Biz kayıtları yok etme korur.  

## <a name="equipment-disposal"></a>Donanım elden çıkarma
Bir sistemin uç yaşam, Microsoft Operasyon personeli sıkı veri işleme ve verilerinizi içeren donanım güvenilmeyen taraflara kullanılabilir hale getirilmediğinden emin olmak için donanım elden yordamları izleyin. Bunu destekleyen sabit sürücüler için güvenli silme yaklaşımı kullanıyoruz. Silinemez sabit sürücüler, sürücü yok eder ve kurtarma bilgilerinin mümkün olmayan işler yıkım işlemini kullanırız. Bu yok etme işlemi disintegrate, içinse, pulverize veya incinerate olabilir. Varlık türüne göre elden çeşit belirleriz. Biz kayıtları yok etme korur. Tüm Azure Hizmetleri, onaylanan medya depolama ve elden Yönetimi hizmetlerini kullanın.

## <a name="compliance"></a>Uyumluluk
Biz, tasarlayın ve çok sayıda uluslararası ve sektöre özgü uyumluluk standardını da ISO 27001, HIPAA, FedRAMP, SOC 1 ve SOC 2 gibi karşılamak için Azure altyapısını yönetme. Biz de Avustralya IRAP, UK G-Cloud ve Singapur MTCS gibi ülkeye veya bölgeye özel standartları karşılar. İngiliz Standartları Enstitüsü tarafından yapılan bu gibi sıkı üçüncü taraf denetimleri Bu standartlar kıldığı katı güvenlik denetimlerine kıldığı doğrulayın.

Azure için uyar uyumluluk standartlarını tam bir listesi için bkz. [uyumluluk teklifi](https://www.microsoft.com/trustcenter/compliance/complianceofferings).

## <a name="next-steps"></a>Sonraki adımlar
Microsoft Azure altyapısının güvenliğini sağlamak için yaptığı hakkında daha fazla bilgi için bkz:

- [Azure altyapı kullanılabilirlik](azure-infrastructure-availability.md)
- [Azure Information sistem bileşenleri ve sınırlar](azure-infrastructure-components.md)
- [Azure ağ mimarisi](azure-infrastructure-network.md)
- [Azure üretim ağı](azure-production-network.md)
- [Azure SQL veritabanı güvenlik özellikleri](azure-infrastructure-sql.md)
- [Azure Üretim Operasyon ve Yönetimi](azure-infrastructure-operations.md)
- [Azure altyapı izleme](azure-infrastructure-monitoring.md)
- [Azure altyapı bütünlüğü](azure-infrastructure-integrity.md)
- [Azure müşteri verilerini koruma](azure-protection-of-customer-data.md)

<!--Image references-->
[1]: ./media/azure-physical-security/data-residency-boundary.png
