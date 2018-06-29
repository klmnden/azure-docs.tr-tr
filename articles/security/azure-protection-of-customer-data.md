---
title: Azure müşteri verileri koruma
description: Bu makalede, Azure müşteri verilerini nasıl korur giderir.
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
ms.openlocfilehash: 9a3b00e39f78f65b05b7d730447440d481979539
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37102694"
---
# <a name="protection-of-customer-data-in-azure"></a>Azure müşteri verileri koruma   
Varsayılan olarak Microsoft işlemler ve destek personeli tarafından müşteri verilerine erişim reddedildi. Müşteri verilerine erişim verildiğinde liderlik onayı gereklidir ve ardından erişim dikkatle yönetilen ve günlüğe kaydedilen. Erişim denetimi gereksinimlerine aşağıdaki Microsoft Azure güvenlik ilkesi tarafından oluşturulur:

- Varsayılan olarak müşteri verilerine erişimi yok
- Müşteri sanal makineleri hiçbir kullanıcı veya yönetici hesaplarına
- Tam görev için gereken en az ayrıcalık vermek; Denetim ve erişim isteklerini oturum

Microsoft Azure destek personeli benzersiz AD şirket hesapları Microsoft tarafından atanır. Microsoft Azure Microsoft Kurumsal Active anahtar bilgi systems erişimi denetlemek için MSIT tarafından yönetilen Directory kullanır. Çok faktörlü kimlik doğrulaması gereklidir ve erişimi yalnızca güvenli konsolları aracılığıyla sağlanır.

Tüm erişim denemesi izlenir ve temel bir rapor kümesi görüntülenebilir.

## <a name="data-protection"></a>Veri koruma
Azure müşteri ile güçlü veri güvenliği – hem varsayılan hem de müşteri seçenekleri olarak sağlar.

**Veriler arasında ayrım yapma** -Azure olan çok kiracılı bir hizmet birden çok müşteri dağıtımları ve sanal makineler, aynı fiziksel donanım üzerinde depolanan anlamına gelir. Azure mantıksal ayırma her müşterinin verileri diğer verilerinden kurabilmeleri için kullanır. Arasında ayrım yapma, titizlikle başka birinin verilerine erişme müşteriler sağlarken ölçek ve ekonomik çok müşterili Hizmetleri'nin avantajları sağlar.

**Rest sırasında veri koruması** -müşterilerdir sağlayan Azure'de depolanan verilerin kendi standartlarına uygun olarak şifrelenir. Azure çok çeşitli müşteriler kendi gereksinimlerine en uygun çözümü seçme esnekliğine vermiş şifreleme yetenekleri sunar. Azure anahtar kasası, müşterilerin kolayca verilerini şifrelemek için bulut uygulamalar ve hizmetler tarafından kullanılan anahtarların denetimi korumak yardımcı olur. Sanal makineler şifrelenecek müşterilerin Azure Disk şifrelemesi sağlar. Azure depolama hizmeti şifrelemesi, bir müşterinin depolama hesabı yerleştirilen tüm verileri şifrelemek mümkün kılar.

**Aktarım sırasında veri koruması** -müşteriler kendi sanal makineleri ve son kullanıcılar arasındaki trafik için şifrelemeyi etkinleştirebilirsiniz. Azure için veya dış Bileşenler'den Aktarımdaki verileri korur ve verileri dahili olarak, örneğin iki sanal ağ arasında geçiş. Azure arasındaki iletişimi şifrelemek için CESG/NCSC tarafından önerilen olarak 2048 bit RSA/SHA256 şifreleme anahtarları ile endüstri standardı Aktarım Katmanı Güvenliği (TLS) 1.2 veya protokol kullanır:

- Müşteri ve bulut
- dahili olarak Azure sistemleri ve veri merkezleri arasında

**Şifreleme** -veri depolama ve aktarım sırasında şifreleme gizlilik ve veri bütünlüğünü sağlamaya yönelik en iyi uygulama olarak müşteriler tarafından dağıtılabilir. Müşterilerin Internet'ten iletişimleri korumak için SSL kullanmak üzere kendi Azure bulut Hizmetleri'ni yapılandırmak basittir ve hatta kendi Azure arasında Vm'lerde barındırılan.

**Veri artıklığı** -Microsoft sağlar cyberattack ya da bir veri merkezine fiziksel hasar ise veriler korunur. Müşteriler için tercih edebilirsiniz:

- Uyumluluk veya gecikme konular için ülke depolama
- Güvenlik ve olağanüstü durum kurtarma amacıyla ülke depolama

Veri yedekleme için seçilen bir coğrafi bölge içinde çoğaltılan ancak dışında iletilmez. Müşteriler kopyalar ve sayısı ve konumu çoğaltma veri merkezleri sayısı dahil olmak üzere veri çoğaltmak için birden fazla seçeneği vardır.

Depolama hesabınızı oluşturduğunuzda, aşağıdaki çoğaltma seçeneklerinden birini seçmeniz gerekir:

- Yerel olarak yedekli depolama (LRS). Yerel olarak yedekli depolama verilerinizin üç kopyasını tutar. LRS, tek bir bölgedeki tek bir tesis içinde üç kez çoğaltılır. LRS normal donanım arızalarına karşı verilerinizi korur ancak tek bir tesisin arızalanmasına karşı koruyamaz.
- Bölgesel olarak yedekli depolama (ZRS). Bölgesel olarak yedekli depolama verilerinizin üç kopyasını tutar. ZRS, LRS daha fazla dayanıklılık sağlamak üzere iki ila üç tesis üzerinde üç kez çoğaltılır. Çoğaltma, tek bir bölge içinde veya iki bölgede oluşur. ZRS, verilerinizin tek bir bölge içinde dayanıklı olmasını sağlar.
- Coğrafi olarak yedekli depolama (GRS). Coğrafi olarak yedekli depolama, depolama hesabınızı oluşturduğunuzda hesabınız için varsayılan olarak etkinleştirilir. GRS verilerinizin altı kopyasını tutar. GRS ile verileriniz birincil bölge içinde üç kez çoğaltılır. Verilerinizi ayrıca ikincil bir bölgede yüzlerce mil yüksek seviyede dayanıklılık sağlanır birincil bölge çıktığınızda, üç kez çoğaltılır. Birincil bölgede bir arıza olması durumunda Azure Storage ikincil bölgeye yük devredecektir. GRS, verilerinizin iki ayrı bölge içinde dayanıklı olmasını sağlar.

**Veri yok etme** - müşteriler veri sildiğinizde veya bırakın Azure, Microsoft aşağıdaki depolama kaynaklarını yeniden yanı sıra önce fiziksel yok etme yetkisi alınmış donanım üzerine için katı standartları. Microsoft, Müşteri isteği ve sözleşme sonlandırma veri tam silme işlemini yürütür.

## <a name="customer-data-ownership"></a>Müşteri verileri sahipliği
Microsoft değil inceleyebilir, onaylayabilir veya müşteriler için Azure dağıttığınız uygulamaları izleyin. Ayrıca, Microsoft tür veri müşteriler Azure'da depolanacağı seçin bilmez. Microsoft Veri sahipliği Azure'da girilen müşteri bilgilerini üzerinden talep değil.

## <a name="records-management"></a>Kayıt Yönetimi
Azure arka uç veri iç kayıt tutma gereksinimleri oluşturmuştur. Müşteriler, kendi kaydı saklama gereksinimlerini belirlemek için sorumludur. Azure'da depolanan kayıtlar için müşteri verilerini ayıklanması ve Azure dışında içerik için bir müşteri belirtilen Bekletme dönemi korunuyor sorumludur.

Azure müşteri verilerini dışa aktarma ve raporlar ürün denetleme olanağı sağlar. Dışarı aktarmaları süre müşteri tanımlı bir bekletme bilgilerini korumak için yerel olarak kaydedilir.

## <a name="electronic-discovery-e-discovery"></a>Elektronik bulma (e-keşif)
Azure müşterileri, e-keşif gereksinimlerini Azure Hizmetleri ile bunların kullanımda uymak için sorumludur. Bir Azure müşteri müşteri verilerini koruması gerekiyorsa, bunlar dışarı aktarma ve verilerini yerel olarak kaydedin. Ayrıca, müşterilerin Azure müşteri desteği departmanından verilerini dışarı aktarma isteyebilir. Müşteri verilerini dışarı aktarmak izin yanı sıra, Azure kapsamlı günlüğe kaydetme ve izleme dahili olarak yürütür.

## <a name="next-steps"></a>Sonraki adımlar
Microsoft Azure altyapı güvenli hale getirmek için yaptığı hakkında daha fazla bilgi için bkz:

- [Azure olanakları, şirket içi ve fiziksel güvenlik](azure-physical-security.md)
- [Azure altyapı kullanılabilirliği](azure-infrastructure-availability.md)
- [Azure Information sistem bileşenleri ve sınırlar](azure-infrastructure-components.md)
- [Azure ağ mimarisi](azure-infrastructure-network.md)
- [Azure üretim ağı](azure-production-network.md)
- [Microsoft Azure SQL veritabanı güvenlik özellikleri](azure-infrastructure-sql.md)
- [Azure üretim işlemleri ve Yönetimi](azure-infrastructure-operations.md)
- [Azure altyapı izleme](azure-infrastructure-monitoring.md)
- [Azure altyapı bütünlüğü](azure-infrastructure-integrity.md)
