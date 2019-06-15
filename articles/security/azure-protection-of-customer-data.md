---
title: Azure'da müşteri verilerini koruma
description: Bu makalede, Azure müşteri verilerini nasıl koruduğu yöneliktir.
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
ms.openlocfilehash: 04163d1fa2a46a2de877702d479f439a5e8711d7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65603139"
---
# <a name="azure-customer-data-protection"></a>Azure müşteri verilerini koruma   
Microsoft Operasyon ve destek personelinin müşteri verilerine erişimi varsayılan olarak reddedilir. Müşteri verilerine erişim verildiğinde liderlik onay gereklidir ve ardından erişim dikkatli bir şekilde yönetilir ve günlüğe kaydedilen. Erişim denetimi gereksinimleri aşağıdaki Azure güvenlik ilkesi tarafından oluşturulur:

- Varsayılan olarak, müşteri verilerine erişim yok.
- Hiçbir kullanıcı veya yönetici hesaplarını müşteri sanal makinelerinde (VM'ler).
- Görevi tamamlamak için gereken en az ayrıcalık vermek; Denetim ve erişim isteklerini günlüğe kaydeder.

Azure destek personelinin benzersiz Kurumsal Active Directory hesapları, Microsoft tarafından atanır. Azure, Microsoft şirket tarafından Microsoft Bilgi Teknolojisi (önemli bilgi sistemlerine erişimi denetlemek için MSIT), yönetilen Directory'i, bağımlıdır. Çok faktörlü kimlik doğrulaması ve güvenli konsolları yalnızca erişim izni verilir.

Tüm erişim denemesi izlenir ve temel bir raporlar kümesi görüntülenebilir.

## <a name="data-protection"></a>Veri koruma
Azure müşterileri ile güçlü veri güvenliği, hem varsayılan hem de müşteri seçenekleri sağlar.

**Veriler arasında ayrım yapma**: Azure, birden çok müşteri dağıtımları anlamına gelir çok kiracılı bir hizmettir ve Vm'leri aynı fiziksel donanımda depolandığı. Azure, her müşteriye ait verileri diğer verilerden ayırmak için mantıksal yalıtım kullanır. Ayırma titizlikle başka birinin verilere erişimi müşteriler önlenirken ölçek ve çok kiracılı hizmetlerinin ekonomik avantajlarını sağlar.

**Bekleyen veri koruma**: Müşteriler, standartlara uygun olarak Azure'da depolanan verilerin şifrelendiğinden emin olmak sizin sorumluluğunuzdadır. Azure, çok çeşitli şifreleme özellikleri, müşterilerin ihtiyaçlarını en iyi karşılayan çözümü seçme esnekliğini sunar. Azure Key Vault, müşterilerin kolayca bulut uygulamaları ve Hizmetleri tarafından verileri şifrelemek için kullanılan anahtarları denetiminizde tutmanıza yardımcı olur. Azure Disk şifrelemesi, müşterilerin Vm'lerini şifrelemesini sağlar. Azure depolama hizmeti şifrelemesi, bir müşterinin depolama hesabına yerleştirilen tüm verileri şifrelemek mümkün kılar.

**Aktarım sırasında veri koruması**: Müşteriler, kendi Vm'leri ve son kullanıcılar arasındaki trafiği şifreleyebilirsiniz. Azure için veya dış Bileşenler'den Aktarımdaki verileri korur ve veri dahili olarak, örneğin iki sanal ağ arasında geçiş. Azure sektör standardı Aktarım Katmanı Güvenliği (TLS) 1.2 veya sonraki Protokolü 2.048 bit RSA/SHA256 şifreleme anahtarlarıyla CESG/NCSC tarafından önerilen şekilde arasındaki iletişimi şifrelemek için kullanır:

- Müşteri ve bulut.
- Dahili olarak Azure sistemleri ve veri merkezleri arasında.

**Şifreleme**: Veri depolama ve aktarım şifreleme, gizlilik ve veri bütünlüğünü sağlamaya yönelik en iyi uygulama olarak müşteriler tarafından dağıtılabilir. Müşterilerin, Azure bulut Hizmetleri, internet'ten ve hatta kendi Azure'da barındırılan sanal makineler arasındaki iletişimi korumak için SSL kullanacak şekilde yapılandırmak açıktır.

**Veri yedekliği**: Microsoft, cyberattack veya bir veri merkezinin fiziksel zarar olması durumunda verilerin korunduğundan emin olun yardımcı olur. Müşterilerin tercih:

- Uyumluluk veya gecikme süresi konuları için depolama alanı içinde-ülke/bölge.
- Güvenlik ve olağanüstü durum kurtarma amacıyla depolama out-,-ülke/out-,-bölge.

Veri yedekleme için seçilen bir coğrafi alanda çoğaltılabilir ancak dışında iletilemedi. Müşteriler, kopyalar ve sayısı ve konumu çoğaltma veri merkezlerinin sayısı dahil olmak üzere, veri çoğaltmak için birden çok seçeneğiniz vardır.

Depolama hesabınızı oluşturduğunuzda şu çoğaltma seçeneklerinden birini seçin:

- **Yerel olarak yedekli depolama (LRS)** : Yerel olarak yedekli depolama verilerinizin üç kopyasını tutar. LRS, tek bir bölgedeki tek bir tesis içinde üç kez çoğaltılır. LRS, tek bir tesis bir hata değil, ancak normal donanım arızalarına karşı verilerinizi korur.
- **Bölgesel olarak yedekli depolama (ZRS)** : Bölgesel olarak yedekli depolama verilerinizin üç kopyasını tutar. ZRS, lrs'ye daha yüksek bir dayanıklılık düzeyi sunabilmek için üç tesis üzerinde üç kez çoğaltılır. Çoğaltma, tek bir bölgede veya iki bölge arasında oluşur. ZRS, verilerinizi tek bir bölge içinde dayanıklı olmasına yardımcı olur.
- **Coğrafi olarak yedekli depolama (GRS)** : Coğrafi olarak yedekli depolama, depolama hesabınızı oluşturduğunuzda hesabınız için varsayılan olarak etkinleştirilir. GRS verilerinizin altı kopyasını tutar. GRS ile verileriniz birincil bölge içinde üç kez çoğaltılır. Verilerinizi ayrıca ikincil bir bölgede yüzlerce mil uzaktaki en yüksek dayanıklılık düzeyini sağlar. birincil bölgede üç kez çoğaltılır. Birincil bölgede bir arıza olması durumunda Azure Storage ikincil bölgeye devreder. GRS verilerinizin iki ayrı bölge içinde dayanıklı olmasını sağlar.

**Veri yok etme**: Microsoft, müşterilerin verileri silmek ya da Azure bırakın fiziksel yok etme yetkisi alınan donanımın yanı sıra kendi yeniden önce depolama kaynaklarını üzerine yazmak için katı standartlar izler. Microsoft, müşteri talebindeki ve sözleşmeyi sonlandırma verilerin tam bir silme işlemi yürütür.

## <a name="customer-data-ownership"></a>Müşteri veri sahipliği
Microsoft olmayan inceleyin, onaylayın veya müşterilerin Azure'a dağıttığınız uygulamaları izleyin. Üstelik, Microsoft ne tür veri müşterilerin Azure'da depolamayı tercih bilmez. Microsoft Veri sahipliği üzerinden Azure'a girilen müşteri bilgileri iddia etmez.

## <a name="records-management"></a>Kayıt Yönetimi
Azure arka uç veri iç kayıt tutma gereksinimlerini oluşturmuştur. Müşteriler, kendi kayıt saklama gereksinimlerini belirlemek için sorumludur. Azure'da depolanan kayıtlar için müşterilerin kendi verilerini ayıklayarak ve içeriklerini Azure dışındaki bir müşteri tarafından belirtilen saklama süresi için koruma yükümlü olursunuz.

Azure, müşterilerin verileri dışarı aktarma ve denetim raporlarını ürünün olanak tanır. Dışarı aktarmaları süre müşteri tanımlı bir bekletme bilgilerini korumak için yerel olarak kaydedilir.

## <a name="electronic-discovery-e-discovery"></a>Elektronik bulma (e-bulma)
Azure müşterilerine e-bulma gereksinimleri Azure hizmetlerinin kullanımları uymakla sorumlu olursunuz. Azure müşterileri kendilerine ait müşteri verileri koruması gerekiyorsa, bunlar dışarı aktarın ve verileri yerel olarak kaydedin. Ayrıca, müşteriler, Azure müşteri desteği bölümüne verilerini dışarı aktarma isteyebilir. Müşteri verilerini dışarı aktarmak izin ek olarak, Azure, kapsamlı günlüğe kaydetme ve izleme dahili olarak yapmaktadır.

## <a name="next-steps"></a>Sonraki adımlar
Microsoft Azure altyapısının güvenliğini sağlamak için yaptığı hakkında daha fazla bilgi için bkz:

- [Azure özellikleri, şirket içi ve fiziksel güvenlik](azure-physical-security.md)
- [Azure altyapı kullanılabilirlik](azure-infrastructure-availability.md)
- [Azure Information sistem bileşenleri ve sınırlar](azure-infrastructure-components.md)
- [Azure ağ mimarisi](azure-infrastructure-network.md)
- [Azure üretim ağı](azure-production-network.md)
- [Azure SQL veritabanı güvenlik özellikleri](azure-infrastructure-sql.md)
- [Azure Üretim Operasyon ve Yönetimi](azure-infrastructure-operations.md)
- [Azure altyapı izleme](azure-infrastructure-monitoring.md)
- [Azure altyapı bütünlüğü](azure-infrastructure-integrity.md)
