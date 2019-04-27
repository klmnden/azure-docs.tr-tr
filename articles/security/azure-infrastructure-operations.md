---
title: Azure Üretim Operasyon ve Yönetimi
description: Bu makalede, Yönetim genel bir açıklamasını ve Azure üretim ağı çalışmasını sağlar.
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
ms.openlocfilehash: 0099eb61d97f813f7adca320b47c195fa1aabbdc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60591468"
---
# <a name="azure-production-operations-and-management"></a>Azure Üretim Operasyon ve Yönetimi    
Yönetim ve işlem Azure üretim ağ operasyon ekiplerine Azure ve Azure SQL veritabanı arasında eşgüdümlü bir çaba olduğu. Çeşitli sistem ve uygulama performansı izleme araçları ortamda takımlar kullanın. Ve ağ cihazları, sunucuları, hizmetleri ve uygulama işlemleri izlemek için uygun araçları kullanırlar.

Azure ortamında çalışan hizmetleri güvenli olarak yürütülmesini sağlamak için izleme, günlüğe kaydetme ve raporlama, aşağıdaki eylemler dahil olmak üzere birden çok düzeyi operasyon ekibi uygulayın:

- Öncelikle, Microsoft Monitoring Agent (MMA), yapı denetleyicisi (FC) ve kök işletim sistemi (OS) dahil olmak üzere birçok yerden izleme ve tanılama günlük bilgilerini toplar ve günlük dosyaları için yazar. Aracı, sonunda bir önceden yapılandırılmış Azure depolama hesabına digested bir alt bilgi iter. Buna ek olarak, bağlantısız izleme ve tanılama hizmeti çeşitli izleme ve tanılama günlük verilerini okur ve bilgileri özetler. İzleme ve tanılama hizmeti bilgileri tümleşik bir günlüğe yazar. Azure, Azure sistem izleme uzantısı olan, özel olarak geliştirilmiş Azure güvenlik izleme kullanır. Bileşenlerin izleyebilir, çözümleyebilir ve platform çeşitli noktalarında ilgili güvenlik olayları hakkında rapor var.

- Azure SQL veritabanı Windows Fabric platformu, yönetim, dağıtım, geliştirme ve Azure SQL veritabanı için operasyonel Gözetimi hizmetleri sağlar. Platform, dağıtılmış, çok adımlı Dağıtım Hizmetleri, sistem durumu izleme, otomatik onarımlar ve hizmet sürümü uyumluluk sunar. Bunu aşağıdaki hizmetleri sağlar:

   - Hizmet modelleme özellikleri yüksek uygunluğa sahip geliştirme ortamıyla (veri merkezi kümeleri pahalı ve nadir).
   - Tek tıklamayla dağıtım ve yükseltme iş akışı hizmeti bootstrap ve bakımı için.
   - Kendi kendini iyileştirecek şekilde etkinleştirmek için otomatik onarım iş akışlarıyla raporlama sistem durumu.
   - Gerçek zamanlı izleme, uyarı ve tesis düğümlerde dağıtılmış bir sistemin hata ayıklama.
   - Merkezi koleksiyonunu işletimsel veriler ve ölçümler için Dağıtılmış kök neden analizi ve hizmet Insight.
   - Dağıtım için araç işlemsel değişiklik yönetimi ve izleme.
   - Azure SQL veritabanı Windows Fabric platform ve izleme betikleri sürekli olarak çalıştırın ve gerçek zamanlı olarak izleyin.

Anomalileri meydana gelirse, Azure olay önceliklendirme takım tarafından takip olay yanıt işlemi etkinleştirilir. Olaya yanıt vermek için uygun bir Azure destek personelinin bildirilir. Sorun izleme ve çözümleme belgelenen ve merkezi bir bilet sistemi içinde yönetilir. Gizlilik Sözleşmesi'ni (NDA) ve istek üzerine sistem çalışma süresi ölçümler kullanılabilir.

## <a name="corporate-network-and-multi-factor-access-to-production"></a>Kurumsal ağ ve üretim için çok faktörlü erişim
Azure destek personelinin Kurumsal ağda kullanıcı tabanını içerir. Şirket ağına, iç kurumsal işlevleri destekler ve Azure müşteri desteği için kullanılan dahili uygulamalara erişimi içerir. Şirket ağına, Azure üretim ağınızdan mantıksal ve fiziksel olarak ayrılır. Azure personeli, Azure iş istasyonları ve dizüstü bilgisayarları kullanarak şirket ağına erişmelerine. Tüm kullanıcıların bir kullanıcı adı ve parola, kurumsal ağ kaynaklarına erişmek için de dahil olmak üzere bir Azure Active Directory (Azure AD) hesabı olması gerekir. Kurumsal ağa erişim verilen tüm Microsoft personeli, Yükleniciler ve satıcılar ve Microsoft Bilgi teknolojisi tarafından yönetilen Azure AD hesapları kullanır. Benzersiz kullanıcı tanımlayıcılarını personel Microsoft'ta işe durumlarına göre ayırmak.

İç Azure uygulamaları için erişim, Active Directory Federasyon Hizmetleri (AD FS) ile kimlik doğrulaması aracılığıyla denetlenir. AD FS güvenli belirteç ve kullanıcı talepleri uygulama üzerinden şirket ağına kullanıcı kimlik doğrulaması sağlayan bir Microsoft Bilgi teknolojisi tarafından barındırılan bir hizmettir. AD FS, Microsoft Kurumsal active directory etki alanına göre kullanıcıların kimliklerini doğrulamak, iç Azure uygulamalarını sağlar. Şirket ağına ortamından üretim ağa erişmek için kullanıcı çok faktörlü kimlik doğrulaması kullanarak doğrulaması gerekir.

## <a name="next-steps"></a>Sonraki adımlar
Microsoft Azure altyapısının güvenliğini sağlamak için yaptığı hakkında daha fazla bilgi için bkz:

- [Azure özellikleri, şirket içi ve fiziksel güvenlik](azure-physical-security.md)
- [Azure altyapı kullanılabilirlik](azure-infrastructure-availability.md)
- [Azure Information sistem bileşenleri ve sınırlar](azure-infrastructure-components.md)
- [Azure ağ mimarisi](azure-infrastructure-network.md)
- [Azure üretim ağı](azure-production-network.md)
- [Azure SQL veritabanı güvenlik özellikleri](azure-infrastructure-sql.md)
- [Azure altyapı izleme](azure-infrastructure-monitoring.md)
- [Azure altyapı bütünlüğü](azure-infrastructure-integrity.md)
- [Azure müşteri verilerini koruma](azure-protection-of-customer-data.md)
