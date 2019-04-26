---
title: Azure sanal makineler ile kullanılan azure güvenlik özellikleri | Microsoft Docs
description: Bu makalede, Azure sanal makineler ile kullanılabilir Azure güvenlik özelliklerini çekirdek genel bir bakış sağlar.
services: security
documentationcenter: na
author: TerryLanfear
manager: barbkess
editor: TomSh
ms.assetid: 467b2c83-0352-4e9d-9788-c77fb400fe54
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/30/2018
ms.author: terrylan
ms.openlocfilehash: c0a4a8ae270c8d8f6f3c2e86db9deed4e14f668e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60444257"
---
# <a name="azure-virtual-machines-security-overview"></a>Azure sanal makineleri güvenliğine genel bakış

Azure sanal makineler, çok çeşitli bilgi işlem çözümlerini Çevik bir şekilde dağıtmak için kullanabilirsiniz. Hizmeti Microsoft Windows, Linux, Microsoft SQL Server, Oracle, IBM, SAP ve Azure BizTalk Services'ı destekler. Bu nedenle, tüm iş yüklerini ve dilleri neredeyse tüm işletim sistemlerinde dağıtabilirsiniz.

Bir Azure sanal makinesi, sanal makineyi çalıştıran fiziksel donanımı satın almanıza ve muhafaza etmenize gerek kalmadan size sanallaştırma esnekliği sunar. Derleme ve verilerinizin korunduğundan ve yüksek oranda güvenli veri merkezlerini güvende olmasını güvence ile uygulamalarınızı dağıtın.

Azure sayesinde, Gelişmiş Güvenlik uyumlu çözümler oluşturabilirsiniz:

* Sanal makinelerinizi virüslerden ve kötü amaçlı yazılımlardan koruyun.
* Hassas verilerinizi şifreleyin.
* Ağ trafiğinin güvenliğini sağlayın.
* Belirlemek ve tehditleri algılar.
* Uyumluluk gereksinimlerini karşılayın.

Bu makalenin hedefi sanal makinelerle kullanılabilmesi için Azure güvenlik özellikleri çekirdek genel bir bakış sağlamaktır. Daha fazla bilgi için makalelerin bağlantıları her özelliğin ayrıntılarını verir.  

## <a name="antimalware"></a>Kötü Amaçlı Yazılımdan Koruma

Azure ile Microsoft, Symantec, Trend Micro ve Kaspersky gibi güvenlik hizmeti satıcısı kötü amaçlı yazılımdan koruma yazılımından kullanabilirsiniz. Bu yazılımı, sanal makinelerinizi kötü amaçlı dosyalardan, reklam yazılımlarından ve diğer tehditlerden korunmasına yardımcı olur.

Azure Cloud Services ve sanal makineler için Microsoft Antimalware belirlenmesi ve virüslerin, casus yazılımların ve diğer kötü amaçlı yazılım kaldırılmasına yardımcı olan gerçek zamanlı koruma bir özelliktir.  Azure için Microsoft Antimalware bilinen kötü amaçlı veya istenmeyen yazılım kendini yüklemeye veya Azure sistemlerinize çalışmayı denediğinde, yapılandırılabilir uyarılar sağlar.

Azure için Microsoft Antimalware, uygulamalar ve Kiracı ortamları için bir tek aracı çözümüdür. İnsan müdahalesi olmadan arka planda çalıştırmak için tasarlanmıştır. İle ya da temel güvenli varsayılan olarak, uygulama iş yüklerinin gereksinimlerini temel veya Gelişmiş kötü amaçlı yazılımdan koruma izleme de dahil olmak üzere, özel bir yapılandırma koruması dağıtabilirsiniz.

Aşağıdaki temel özellikleri dağıtma ve Azure için Microsoft Antimalware etkinleştirmek, kullanılabilir:

* **Gerçek zamanlı koruma**: Bulut Hizmetleri ve sanal makinelere algılanmasına ve kötü amaçlı yazılım çalıştırma etkinliği izler.
* **Zamanlanmış tarama**: Düzenli olarak etkin olarak çalışan programlar da dahil olmak üzere kötü amaçlı yazılımları algılamak için hedeflenen tarama yapar.
* **Kötü amaçlı yazılım düzeltme**: Otomatik olarak algılanan kötü amaçlı yazılım gibi silme veya kötü amaçlı dosyaları karantinaya ve kötü amaçlı kayıt defteri girdilerini temizleme eylemi alır.
* **İmza güncelleştirme**: Otomatik olarak koruma üzerinde önceden belirlenmiş bir sıklık güncel olduğundan emin olmak için en son (virüs tanımları) koruması imzaları yükler.
* **Kötü amaçlı yazılımdan koruma Altyapısı güncelleştirmeleri**: Microsoft Azure altyapısı Antimalware otomatik olarak güncelleştirir.
* **Kötü amaçlı yazılımdan koruma platformu güncelleştirmeleri**: Microsoft Azure platformu Antimalware otomatik olarak güncelleştirir.
* **Etkin koruma**: Algılanan Tehditler ve şüpheli kaynakları hakkında hızlı yanıt vermek, telemetri meta verileri Azure'a bildirir. Gerçek zamanlı zaman uyumlu imzası teslim aracılığıyla Microsoft etkin koruma sistemi (MAPS) sağlar.
* **Örnekleri raporlama**: Ve Microsoft hizmet iyileştirmek ve sorun giderme etkinleştirmek amacıyla Antimalware için Azure hizmet örnekleri raporlar sağlar.
* **Dışlamalar**: Koruma ve performans ve diğer nedenlerle için tarama dışında tutmak uygulama ve hizmet yöneticileri belirli dosyaları, işlemler ve sürücüleri yapılandırmanıza olanak sağlar.
* **Kötü amaçlı yazılımdan koruma olay toplama**: Kötü amaçlı yazılımdan koruma hizmet durumu, şüpheli etkinlikleri ve işletim sistemi olay günlüğünde gerçekleştirilen düzeltme eylemleri kaydeder ve bunları Azure depolama hesabınızdaki toplar.

Sanal makinelerinizi korumaya yardımcı olmak için kötü amaçlı yazılımdan koruma yazılımı hakkında daha fazla bilgi edinin:

* [Azure bulut Hizmetleri ve sanal makineler için Microsoft kötü amaçlı yazılımdan koruma](azure-security-antimalware.md)
* [Azure Sanal Makinelerinde Kötü Amaçlı Yazılıma Karşı Koruma Çözümleri Dağıtma](https://azure.microsoft.com/blog/deploying-antimalware-solutions-on-azure-virtual-machines/)
* [Yükleme ve bir Windows VM'de hizmet olarak Trend Micro Deep Security yapılandırın](../virtual-machines/windows/classic/install-trend.md)
* [Yükleme ve bir Windows VM'de Symantec Endpoint Protection'ı yapılandırma](../virtual-machines/windows/classic/install-symantec.md)
* [Azure Market'te güvenlik çözümleri](https://azure.microsoft.com/marketplace/?term=security)

Daha güçlü koruma için kullanmayı [Windows Defender Gelişmiş tehdit koruması](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/windows-defender-advanced-threat-protection). Windows Defender ATP ile şunları elde edersiniz:

* [Saldırı yüzeyini azaltma](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/overview-attack-surface-reduction)  
* [Sonraki nesil koruma](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/windows-defender-antivirus-in-windows-10)  
* [Endpoint protection'ı ve yanıt](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/overview-endpoint-detection-response)
* [Otomatik araştırma ve düzeltme](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/automated-investigations-windows-defender-advanced-threat-protection)
* [Güvenli puanı](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/overview-secure-score-windows-defender-advanced-threat-protection)
* [Gelişmiş avcılık](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/overview-hunting-windows-defender-advanced-threat-protection)
* [Yönetim ve API'ler](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/management-apis)
* [Microsoft tehdit koruması](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/threat-protection-integration)

Daha fazla bilgi edinin: 

* [WDATP ile çalışmaya başlama](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/get-started)  
* [WDATP özelliklerine genel bakış](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/overview)  

## <a name="hardware-security-module"></a>Donanım güvenlik modülü

Anahtar güvenlik artar, şifreleme ve kimlik korumaları geliştirebilirsiniz. Azure Key Vault'ta depolayarak bunları kritik gizli dizileri ve anahtarları ve Yönetimi basitleştirebilir. 

Key Vault, anahtarlarınızı FIPS 140-2 2. Düzey standartlarıyla sertifikalanmış olan donanım güvenlik modüllerinde (HSM'ler) depolama seçeneği sunar. SQL Server şifreleme anahtarları için yedekleme veya [saydam veri şifrelemesi](https://msdn.microsoft.com/library/bb934049.aspx) tüm anahtar Kasası'nda tüm anahtarlar veya parolalar uygulamalarınızdan depolanabilir. İçin bu korumalı öğelere izni veya erişimi aracılığıyla yönetilir [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/).

Daha fazla bilgi edinin:

* [Azure anahtar kasası nedir?](../key-vault/key-vault-overview.md)
* [Azure Key Vault blogu](https://blogs.technet.microsoft.com/kv/)

## <a name="virtual-machine-disk-encryption"></a>Sanal makine disk şifrelemesi

Azure Disk şifrelemesi, Windows ve Linux sanal makine disklerini şifrelemek için yeni bir özelliktir. Azure Disk şifrelemesi, endüstri standardı kullanır [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) Windows özelliğidir ve [dm-crypt](https://en.wikipedia.org/wiki/Dm-crypt) Linux işletim sistemi ve veri diskleri için birim şifrelemesi sağlamak için bir özelliğidir.

Disk şifreleme anahtarlarını ve gizli anahtar kasası aboneliğinizi yönetmek ve denetlemek yardımcı olması için Azure Key Vault ile tümleşik bir çözüm. Bu, sanal makine disklerini tüm veriler Azure depolama alanındaki bekleyen şifrelenmesini sağlar.

Daha fazla bilgi edinin:

* [Iaas VM'ler için Azure Disk şifrelemesi](../security/azure-security-disk-encryption-overview.md)
* [Hızlı Başlangıç: Azure PowerShell ile bir Windows Iaas VM'LERİNİ şifreleme](../security/quick-encrypt-vm-powershell.md)

## <a name="virtual-machine-backup"></a>Sanal makine yedekleme

Azure yedekleme, uygulama verilerinizi sıfır sermaye yatırımı ve en az işletim maliyetiyle korumaya yardımcı olan ölçeklenebilir bir çözümdür. Uygulama hataları verilerinizi bozabilir ve insan hataları, uygulamalarınızda hatalar oluşturabilir. Windows ve Linux çalıştıran sanal makinelerinizi Azure Backup ile korunmuş olur.

Daha fazla bilgi edinin:

* [Azure Backup nedir?](../backup/backup-introduction-to-azure-backup.md)
* [Azure Backup hizmeti hakkında SSS](../backup/backup-azure-backup-faq.md)

## <a name="azure-site-recovery"></a>Azure Site Recovery

Kuruluşunuzun BCDR stratejisinin önemli bir bölümü kurumsal iş yüklerini korumak nasıl tespit edilmesi ve Planlı çalışan uygulamalar ve plansız kesintiler oluştuğunda. Azure Site Recovery, çoğaltma, yük devretme ve kurtarma, iş yükleri ve uygulamalar, birincil çökmesi durumunda ikincil konum kullanılabilir olacak şekilde düzenleyin yardımcı olur.

Site Recovery:

* **BCDR stratejinize basitleştirir**: Site Recovery, çoğaltma, yük devretme ve kurtarma, birden çok iş yükleri ve uygulamalar tek bir konumdan işlemek kolaylaştırır. Site Recovery çoğaltma ve yük devretme işlemlerini düzenler ancak değil, uygulama verilerinize müdahale veya hakkında tüm bilgiler toplamaz.
* **Esnek çoğaltma sağlar**: Site Recovery kullanarak Hyper-V sanal makinelerini, VMware sanal makinelerini ve Windows/Linux fiziksel sunucularında çalışan iş yüklerini çoğaltabilirsiniz.
* **Yük devretme ve kurtarma destekler**: Site Recovery, üretim ortamlarını etkilemeden olağanüstü durum kurtarma ayrıntılarını desteklemek için yük devretme testleri sunar. Ayrıca, beklenen kesintilere yönelik olarak sıfır veri kaybı sunan planlanan yük devretmeler veya beklenmeyen olağanüstü durumlar için minimum düzeyde veri kaybıyla sonuçlanan (çoğaltma sıklığına bağlı olarak) planlanmamış yük devretmeler çalıştırabilirsiniz. Yük devretmenin ardından birincil sitelerinizde başarısız olabilir. Yük devretme işlemini özelleştirebilmeniz ve çok katmanlı uygulamaları kurtarabilmeniz için Site Recovery, betikleri ve Azure otomasyonu çalışma kitaplarını içeren kurtarma planları sunar.
* **İkincil veri merkezleri ortadan**: Bir ikincil şirket içi siteye veya azure'a çoğaltabilirsiniz. Azure olağanüstü durum kurtarma için hedef olarak kullanarak, maliyet ve ikincil bir site kullanmanın karmaşıklığını ortadan kaldırır. Çoğaltılan veriler Azure Depolama'da saklanır.
* **Mevcut BCDR teknolojileri ile tümleşen**: Site Recovery, diğer uygulama BCDR özellikleriyle iş ortakları. Örneğin, kurumsal iş yüklerinin SWL Server arka ucunu korumak için Site RECOVERY'yi kullanabilirsiniz. Bu SQL Server her zaman yük devretme kullanılabilirlik grupları yönetmek şirket için yerel destek içerir.

Daha fazla bilgi edinin:

* [Azure Site Recovery nedir?](../site-recovery/site-recovery-overview.md)
* [Azure Site Recovery nasıl çalışır?](../site-recovery/site-recovery-components.md)
* [Hangi iş yüklerini Azure Site Recovery tarafından korunuyor mu?](../site-recovery/site-recovery-workload.md)

## <a name="virtual-networking"></a>Sanal ağ

Sanal makinelerin ağ bağlantısı gerekir. Bu gereksinimi desteklemek için Azure sanal makineler, Azure sanal ağına bağlı olması gerekir. 

Bir Azure sanal ağı, fiziksel Azure ağ dokusu üzerine oluşturulmuş mantıksal bir yapıdır. Her mantıksal Azure sanal ağ, diğer tüm Azure sanal ağlardan yalıtılır. Bu yalıtım, dağıtımlarınızı ağ trafiğini diğer Microsoft Azure müşterileri için erişilebilir değil Sigortası yardımcı olur.

Daha fazla bilgi edinin:

* [Azure ağ güvenliğine genel bakış](security-network-overview.md)
* [Sanal Ağ’a genel bakış](../virtual-network/virtual-networks-overview.md)
* [Ağ özellikleri ve kuruluş senaryolarına yönelik iş ortaklıkları](https://azure.microsoft.com/blog/networking-enterprise/)

## <a name="security-policy-management-and-reporting"></a>Güvenlik İlkesi Yönetimi ve Raporlama

Azure Güvenlik Merkezi, tehditleri önleyin, algılayın ve yardımcı olur. Güvenlik Merkezi verir artırılmış görünürlük ve üzerinde Azure kaynaklarınızın güvenliğini denetleyebilirsiniz. Bu, Azure aboneliklerinizde tümleşik güvenlik izleme ve ilke yönetimi sağlar. Aksi takdirde gözden kaçan geçebilir ve güvenlik çözümlerinin geniş ekosistemiyle çalışan tehditleri algılamanıza yardımcı olur.

Güvenlik Merkezi iyileştirmek ve sanal makinelerinizin güvenlik izlemenize yardımcı olur:

* Sağlama [güvenlik önerilerini](../security-center/security-center-recommendations.md) sanal makineleri için. Örnek öneriler şunlardır: sistem güncelleştirmelerini uygulayın, ACL'ler uç noktalarını yapılandırma, kötü amaçlı yazılımdan koruma etkinleştirme, ağ güvenlik gruplarını etkinleştirme ve disk şifrelemesi uygulayın.
* Sanal makinelerinizin durumunu izleme.

Daha fazla bilgi edinin:

* [Azure Güvenlik Merkezi'ne Giriş](../security-center/security-center-intro.md)
* [Azure Güvenlik Merkezi hakkında sık sorulan sorular](../security-center/security-center-faq.md)
* [Azure Güvenlik Merkezi planlama ve işlemler](../security-center/security-center-planning-and-operations-guide.md)

## <a name="compliance"></a>Uyumluluk

Azure sanal makineleri FISMA, FedRAMP, HIPAA, PCI DSS düzey 1 ve diğer önemli uyumluluk programları için sertifikalı. Bu sertifika için kendi Azure uygulamalarınızın uyumluluk gereksinimlerini karşılamak ve işletmenize, çok çeşitli yurtiçi ve uluslararası Yasal gereksinimlere yönelik olarak daha kolay hale getirir.

Daha fazla bilgi edinin:

* [Microsoft Güven Merkezi: Uyumluluk](https://www.microsoft.com/en-us/trustcenter/compliance)
* [Güvenilir bulut: Microsoft Azure güvenlik, gizlilik ve uyumluluk](https://download.microsoft.com/download/1/6/0/160216AA-8445-480B-B60F-5C8EC8067FCA/WindowsAzure-SecurityPrivacyCompliance.pdf)

## <a name="confidential-computing"></a>Gizli bilgi işlem

Gizli bilgi işlem sanal makine güvenliği teknik bir parçası değildir, ancak sanal makine güvenlik konusunda "işlem" güvenlik üst düzey bir konu aittir. Gizli bilgi işlem, "işlem" güvenlik kategoride aittir. 

Gizli bilgi işlem sağlar veriler "clear" olduğunda verimli işleme, veri için gerekli olan güvenilir bir yürütme ortamı içinde korunduğundan emin https://en.wikipedia.org/wiki/Trusted_execution_environment (TEE - kuşatma olarak da bilinir), bir örneğini aşağıdaki şekilde gösterilmiştir. .  

TEEs görünüm verileri veya içinde bir hata ayıklayıcısı olsa da işlem dışarıdan, yolu bulunduğundan emin olun. Hatta yalnızca yetkili kod verilere erişmesine izin verildiğinden emin olun. Kod değiştirilmiş ya da değiştirilmiş, işlemleri reddedilir ve ortam devre dışı. TEE içindeki kod yürütmeyi boyunca bu korumalar zorlar. 

Daha fazla bilgi edinin:

* [Azure gizli bilgi işlem ile tanışın](https://azure.microsoft.com/blog/introducing-azure-confidential-computing/)  
* [Azure gizli bilgi işlem](https://azure.microsoft.com/blog/azure-confidential-computing/)  

