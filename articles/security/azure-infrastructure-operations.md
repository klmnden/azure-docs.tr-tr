---
title: Azure üretim işlemleri ve Yönetimi
description: Bu makale, Yönetim genel bir açıklamasını ve Azure üretim ağı çalışmasını sağlar.
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
ms.openlocfilehash: dc389f5f5c155555deb860f041b15b0ea49ee416
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37102702"
---
# <a name="azure-production-operations-and-management"></a>Azure üretim işlemleri ve Yönetimi    
Yönetim ve Azure üretim ağı işlemi Azure işletim ekipleri ve Azure SQL veritabanı arasında Eşgüdümlü çaba olur. Çeşitli sistem ve uygulama performans izleme araçları ortamında kullanılır. Ağ aygıtları, sunucuları, hizmetleri ve uygulama işlemleri uygun araçları ile izlenir.

İzleme, günlüğe kaydetme ve raporlama Microsoft Azure ortamında çalışan hizmetler güvenli yürütülmesini sağlamak amacıyla uygulanan birden çok düzeyi aşağıdaki eylemleri de dahil olmak üzere:

- Öncelikle, Microsoft Azure İzleme Aracısı (MA) FC ve işletim sistemi kök dahil olmak üzere birçok yerden izleme ve tanılama günlük bilgilerini toplar ve günlük dosyalarına yazar. Önceden yapılandırılmış bir Azure Storage hesabınıza digested bilgilerin bir alt kümesini sonunda iter. Ayrıca, izleme ve Tanılama Hizmeti (MDS), çeşitli izleme ve tanılama günlük verilerini okur ve bilgileri özetler bağlantısız bir hizmetidir. MDS bilgileri bir tümleşik günlüğe yazar. Azure özel olarak geliştirilmiş Azure güvenlik izleme (ASM), uzantı Azure izleme sistemine olduğu kullanır. İnceleyin, çözümlemek ve Platform çeşitli noktaları güvenlik ilgili olayları hakkında rapor bileşenleri içerir.
- Microsoft Azure SQL veritabanı WinFabric platform yönetimi, dağıtım, geliştirme ve Microsoft Azure SQL veritabanı için işletimsel gözetim hizmetleri sağlar. Dağıtılmış, çok adımlı Dağıtım Hizmetleri, sistem durumu izleme, otomatik onarım ve hizmet sürümü uyumluluk sunar. Aşağıdaki hizmetleri sağlar:

   - Hizmet modelleme yetenekleri yüksek doğruluk geliştirme ortamı (datacenter kümeleri pahalı ve olması).
   - Tek tıklamayla dağıtım ve yükseltme iş akışı hizmeti önyükleme ve bakımı için.
   - Sistem durumu onaran etkinleştirmek için otomatik onarım iş akışlarıyla raporlama.
   - Gerçek zamanlı izleme, uyarma ve dağıtılmış bir sistemde düğümleri arasında tesisler hata ayıklama.
   - İşletimsel veriler ve ölçümler dağıtılmış bir kök merkezi koleksiyonunu çözümleme ve hizmet Insight neden.
   - Dağıtım için tooling işlemsel değişiklik yönetimi ve izleme.
   - Microsoft Azure SQL veritabanı WinFabric platform ve izleme betikleri sürekli çalışacak ve gerçek zamanlı olarak izleyin.

Tüm anormallikleri meydana gelirse, Azure olay önceliklendirme ekibi tarafından izlenen olay yanıtlama süreci etkinleştirilir. Uygun Azure destek personeli olaya yanıt bildirilir. Sorun izleme ve çözümleme belgelenmiş ve merkezi bir bilet sistemi yönetilen. Sistem çalışma süresi ölçümleri olmayan Gizlilik Sözleşmesi'altında (NDA) ve istek üzerine kullanılabilir.

## <a name="corporate-network-and-multi-factor-access-to-production"></a>Şirket ağı ve üretim için çok faktörlü erişim
Temel şirket ağı kullanıcı Microsoft Azure destek personeli içerir. Şirket ağına, iç şirket işlevlerini destekler ve Azure müşteri desteği için kullanılan iç uygulamalara erişim içerir. Şirket ağına Azure üretim ağınızdan mantıksal ve fiziksel olarak ayrılır. Microsoft Azure personel Microsoft Azure iş istasyonları ve dizüstü bilgisayarları kullanarak şirket ağına erişmek. Tüm kullanıcıların bir kullanıcı adı ve parola, kurumsal ağ kaynaklarına erişmek için de dahil olmak üzere Active Directory (AD) hesabınız olmalıdır. CorpNet erişim verilen tüm Microsoft personeli, Yükleniciler, satıcılar ve MSIT tarafından yönetilen AD hesapları kullanır. Benzersiz kullanıcı tanımlayıcıları personel çalışma durumlarını Microsoft'taki göre ayırt etmek.

İç Azure uygulamalara erişimi kimlik doğrulaması Active Directory Federasyon Hizmetleri (ADFS) ile aracılığıyla denetlenir. ADFS güvenli belirteç ve kullanıcı taleplerini uygulama aracılığıyla CorpNet kullanıcıların kimlik doğrulaması sağlayan MSIT tarafından barındırılan bir hizmettir. ADFS Microsoft Kurumsal AD etki alanına göre kullanıcıların kimliklerini doğrulamak iç Microsoft Azure uygulamaları etkinleştirir. CorpNet ortamından üretim ağa erişmek için kullanıcı çok faktörlü kimlik doğrulamasını kullanarak kimlik doğrulaması gerekir.

## <a name="next-steps"></a>Sonraki adımlar
Microsoft Azure altyapı güvenli hale getirmek için yaptığı hakkında daha fazla bilgi için bkz:

- [Azure olanakları, şirket içi ve fiziksel güvenlik](azure-physical-security.md)
- [Azure altyapı kullanılabilirliği](azure-infrastructure-availability.md)
- [Azure Information sistem bileşenleri ve sınırlar](azure-infrastructure-components.md)
- [Azure ağ mimarisi](azure-infrastructure-network.md)
- [Azure üretim ağı](azure-production-network.md)
- [Microsoft Azure SQL veritabanı güvenlik özellikleri](azure-infrastructure-sql.md)
- [Azure altyapı izleme](azure-infrastructure-monitoring.md)
- [Azure altyapı bütünlüğü](azure-infrastructure-integrity.md)
- [Azure müşteri verileri koruma](azure-protection-of-customer-data.md)
