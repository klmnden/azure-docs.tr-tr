---
title: Azure sanal makineler ile kullanılan azure güvenlik özellikleri | Microsoft Docs
description: Bu makalede Azure sanal makinelerle kullanılabilir Azure güvenlik özellikleri çekirdek genel bir bakış sağlar.
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: TomSh
ms.assetid: 467b2c83-0352-4e9d-9788-c77fb400fe54
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: terrylan
ms.openlocfilehash: 7d31a434d4ead6dd8f8a13a08d368389904b5b4a
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="azure-virtual-machines-security-overview"></a>Azure sanal makineleri güvenliğine genel bakış
Azure sanal makineler, çok çeşitli bilgi işlem çözümleri Çevik bir şekilde dağıtmak için kullanabilirsiniz. Microsoft Windows, Linux, Microsoft SQL Server, Oracle, IBM, SAP ve Azure BizTalk Services hizmeti destekler. Bu nedenle herhangi bir iş yükünü ve neredeyse tüm işletim sisteminde herhangi bir dil dağıtabilirsiniz.

Bir Azure sanal makinesi, sanal makineyi çalıştıran fiziksel donanımı satın almanıza ve muhafaza etmenize gerek kalmadan size sanallaştırma esnekliği sunar. Derleme ve verilerinizi korumalı ve yüksek güvenlikli veri merkezlerinde güvenli olduğunu güvence uygulamalarınızla dağıtın.

Azure ile gelişmiş güvenlik, uyumlu çözümler oluşturabilir:

* Sanal makinelerinizi virüs ve kötü amaçlı yazılımlardan koruyun.
* Hassas verileri şifreleyin.
* Güvenli ağ trafiği.
* Belirleyin ve tehditleri algılayabilir.
* Uyumluluk gereksinimlerini karşılar.

Bu makalede amacı, sanal makinelerle kullanılabilir Azure güvenlik özellikleri çekirdek genel bir bakış sağlamaktır. Daha fazla bilgi edinmek için makalelerinin bağlantıları her bir özelliğin ayrıntılarını verir.  

## <a name="antimalware"></a>Kötü Amaçlı Yazılımdan Koruma
Azure ile Microsoft, Symantec, eğilim mikro ve Kaspersky gibi güvenlik satıcılardan kötü amaçlı yazılımdan koruma yazılımını kullanabilirsiniz. Bu yazılım, kötü amaçlı dosyalar, reklam ve diğer tehditlerden sanal makinelerinizi korunmasına yardımcı olur.

Azure Cloud Services ve sanal makineleri için Microsoft Antimalware tanımlamak ve virüsler, casus yazılım ve diğer kötü amaçlı yazılım kaldırma yardımcı olan gerçek zamanlı koruma bir özelliktir.  Azure için Microsoft Antimalware kötü amaçlı veya istenmeyen yazılım kendini yüklemeye veya Azure sistemleriniz üzerinde çalışmaya girişiminde bilinen yapılandırılabilir uyarır sağlar.

Azure için Microsoft Antimalware, uygulamaları ve Kiracı ortamları için bir tek Aracısı çözümüdür. İnsan aracılığı olmadan arka planda için tasarlanmıştır. Her iki temel güvenli stl'deki varsayılan ile uygulama iş yüklerinin gereksinimlerini temel veya Gelişmiş kötü amaçlı yazılımdan koruma izleme de dahil olmak üzere özel yapılandırma koruması dağıtabilirsiniz.

Aşağıdaki temel özellikleri, dağıtmak ve Microsoft Antimalware Azure etkinleştirmek seçtiğinizde kullanılabilir:

* **Gerçek zamanlı koruma**: Bulut Hizmetleri ve sanal makinelerin Algıla ve kötü amaçlı yazılım yürütme engelleme etkinliğini izler.
* **Zamanlanmış tarama**: etkin olarak çalışan programlar dahil olmak üzere kötü amaçlı yazılım, algılamak için hedeflenen tarama düzenli olarak gerçekleştirir.
* **Kötü amaçlı yazılım düzeltme**: otomatik olarak algılanan kötü amaçlı yazılım silme veya kötü amaçlı dosyaları karantinaya alma ve kötü amaçlı kayıt defteri girdilerini temizleme gibi eylemi alır.
* **İmza güncelleştirme**: koruma önceden belirlenen bir frekansında güncel olduğundan emin olmak için en son koruma imzaları (virüs tanımları) otomatik olarak yükler.
* **Kötü amaçlı yazılımdan koruma Altyapısı güncelleştirmeleri**: Azure altyapısı için Microsoft Antimalware otomatik olarak güncelleştirir.
* **Kötü amaçlı yazılımdan koruma platformu güncelleştirmeleri**: Microsoft Antimalware Azure platformu için otomatik olarak güncelleştirir.
* **Etkin bir koruma**: raporları telemetri hakkındaki meta verileri Azure algılanan tehditlere ve şüpheli kaynaklara hızlı yanıt emin olun. Gerçek zamanlı zaman uyumlu imza teslim aracılığıyla Microsoft etkin koruma sistem (MAPS) sağlar.
* **Örnekleri raporlama**: sağlar ve Microsoft hizmet iyileştirmek ve sorun giderme etkinleştirmek amacıyla Antimalware için Azure hizmeti örnekleri raporlar.
* **Dışlamalar**: koruma ve performans ve diğer nedenler için tarama dışında tutmak için sürücü ve uygulama ve hizmet yöneticileri işlemleri, belirli dosyaları yapılandırma sağlar.
* **Kötü amaçlı yazılımdan koruma olay toplama**: kötü amaçlı yazılımdan koruma hizmeti durumu, kuşkulu etkinlikleri ve işletim sistemi olay günlüğünde gerçekleştirilecek düzeltme eylemleri kaydeder ve Azure depolama hesabınızdaki toplar.

Sanal makinelerinizi korunmasına yardımcı olmak için kötü amaçlı yazılımdan koruma yazılımı hakkında daha fazla bilgi edinin:

* [Azure bulut Hizmetleri ve sanal makineleri için Microsoft kötü amaçlı yazılımdan koruma](azure-security-antimalware.md)
* [Azure Sanal Makinelerinde Kötü Amaçlı Yazılıma Karşı Koruma Çözümleri Dağıtma](https://azure.microsoft.com/blog/deploying-antimalware-solutions-on-azure-virtual-machines/)
* [Yükleme ve bir Windows VM bir hizmet olarak eğilimi mikro derin güvenliği yapılandırma](../virtual-machines/windows/classic/install-trend.md)
* [Yükleme ve Symantec Endpoint Protection bir Windows VM yapılandırma](../virtual-machines/windows/classic/install-symantec.md)
* [Azure Marketi'nde güvenlik çözümleri](https://azure.microsoft.com/marketplace/?term=security)

## <a name="hardware-security-module"></a>Donanım güvenlik modülü
Anahtar güvenlik geliştirme şifreleme ve kimlik doğrulama korumaları geliştirebilirsiniz. Azure anahtar kasası depolayarak güvenlik kritik parolaları ve anahtarlar ve Yönetimi basitleştirebilir. 

Key Vault, anahtarlarınızı FIPS 140-2 2. Düzey standartlarıyla sertifikalanmış olan donanım güvenlik modüllerinde (HSM'ler) depolama seçeneği sunar. SQL Server şifreleme anahtarları için yedekleme veya [saydam veri şifreleme](https://msdn.microsoft.com/library/bb934049.aspx) herhangi bir anahtar veya gizli, uygulamalardan ile anahtar kasasına tüm depolanabilir. İzinleri ve korumalı bu öğelere erişimi üzerinden yönetilir [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/).

Daha fazla bilgi edinin:

* [Azure anahtar kasası nedir?](../key-vault/key-vault-whatis.md)
* [Azure anahtar kasası ile çalışmaya başlama](../key-vault/key-vault-get-started.md)
* [Azure anahtar kasası blogu](https://blogs.technet.microsoft.com/kv/)

## <a name="virtual-machine-disk-encryption"></a>Sanal makine disk şifrelemesi
Azure Disk şifrelemesi, Windows ve Linux sanal makine disklerinizi şifrelemek için yeni bir özelliktir. Azure Disk şifrelemesi kullanılır endüstri standardı [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) Windows özelliğidir ve [dm-crypt](https://en.wikipedia.org/wiki/Dm-crypt) Linux işletim sistemi ve veri diskleri için birim şifrelemesi sağlamak için özelliğidir.

Çözüm denetlemek ve disk şifreleme anahtarları ve gizli anahtar kasası aboneliğinizde yönetmenize yardımcı olmak için Azure anahtar kasası ile tümleşiktir. Sanal makine disklerini tüm verileri Azure Storage REST şifrelenmesini sağlar.

Daha fazla bilgi edinin:

* [Windows ve Linux Iaas VM'ler için Azure Disk şifrelemesi](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)
* [Linux ve Windows sanal makineler için Azure Disk şifrelemesi](https://blogs.msdn.microsoft.com/azuresecurity/2015/11/16/azure-disk-encryption-for-linux-and-windows-virtual-machines-public-preview-now-available/)
* [Bir sanal makine şifrele](../security-center/security-center-disk-encryption.md)

## <a name="virtual-machine-backup"></a>Sanal makine yedekleme
Azure yedekleme, sıfır sermaye yatırımı ve en az bir işletim maliyetlerini uygulama verilerinizin korunmasına yardımcı olan ölçeklenebilir bir çözümdür. Uygulama hataları verilerinizi bozabilir ve insan hataları, uygulamalarınızda hatalar oluşturabilir. Azure Backup ile Windows ve Linux çalıştıran sanal makinelerinizi korunur.

Daha fazla bilgi edinin:

* [Azure Backup nedir?](../backup/backup-introduction-to-azure-backup.md)
* [Azure yedekleme öğrenme yolu](https://azure.microsoft.com/documentation/learning-paths/backup/)
* [Azure Backup hizmeti ile ilgili SSS](../backup/backup-azure-backup-faq.md)

## <a name="azure-site-recovery"></a>Azure Site Recovery
Kuruluşunuzun BCDR stratejisinin önemli bir parçası kurumsal iş yüklerini korumak nasıl açık ve Planlı çalışan uygulamalar ve planlanmayan kesintiler oluşur. Azure Site Recovery, böylece birincil konumunuz çökmesi durumunda ikincil konum kullanılabilir çoğaltma, yük devretme ve kurtarma iş yüklerini ve uygulamaları düzenlemek yardımcı olur.

Site Recovery:

* **BCDR stratejinize basitleştirir**: Site Recovery çoğaltma, yük devretme ve tek bir konumdan birden çok iş yükünün ve uygulamanın kurtarılması işlemek kolaylaştırır. Site Recovery çoğaltma ve yük devretme düzenler ancak olmayan uygulama verilerinizi müdahale veya hakkında bilgiler sağlayın.
* **Esnek çoğaltma sağlar**: Site Recovery kullanarak Hyper-V sanal makineleri, VMware sanal makineleri ve Windows/Linux fiziksel sunucularında çalışan iş yüklerini çoğaltabilirsiniz.
* **Yük devretme ve kurtarma destekler**: Site Recovery, üretim ortamlarını etkilemeden olağanüstü durum kurtarma ayrıntılarını desteklemek için yük devretme testlerini sağlar. Ayrıca, beklenen kesintilere yönelik olarak sıfır veri kaybı sunan planlanan yük devretmeler veya beklenmeyen olağanüstü durumlar için minimum düzeyde veri kaybıyla sonuçlanan (çoğaltma sıklığına bağlı olarak) planlanmamış yük devretmeler çalıştırabilirsiniz. Yük devretme sonrasında birincil siteleriniz başarısız olabilir. Yük devretme işlemini özelleştirebilmeniz ve çok katmanlı uygulamaları kurtarabilmeniz için Site Recovery, betikleri ve Azure otomasyonu çalışma kitaplarını içeren kurtarma planları sunar.
* **İkincil veri merkezleri ortadan**: bir ikincil şirket içi siteye veya azure'a çoğaltabilirsiniz. Azure için olağanüstü durum kurtarma hedefi olarak kullanarak, maliyet ve bir ikincil site Bakımı karmaşıklığını ortadan kaldırır. Çoğaltılan veriler Azure depolama alanında depolanır.
* **Var olan BCDR teknolojileri ile tümleşir**: Site Recovery iş ortaklarına, diğer uygulama BCDR özellikleriyle. Örneğin, SQL Server arka uç kurumsal iş yüklerinin korunmasına yardımcı olmak için Site Recovery kullanabilirsiniz. Bu SQL Server her zaman kullanılabilirlik grupları için yük devretme yönetmek şirket için yerleşik destek içerir.

Daha fazla bilgi edinin:

* [Azure Site Recovery nedir?](../site-recovery/site-recovery-overview.md)
* [Azure Site Recovery nasıl çalışır?](../site-recovery/site-recovery-components.md)
* [Hangi iş yüklerini Azure Site Recovery tarafından korunan?](../site-recovery/site-recovery-workload.md)

## <a name="virtual-networking"></a>Sanal ağ
Sanal makinelerin ağ bağlantısı gerekir. Bu gereksinimi desteklemek için Azure sanal makineleri bir Azure sanal ağına bağlı olması gerektirir. 

Bir Azure sanal ağı en üstünde fiziksel Azure ağ doku yerleşik mantıksal bir yapıdır. Her mantıksal Azure sanal tüm diğer Azure sanal ağlardan yalıtılmış bir ağdır. Bu yalıtım dağıtımlarınızı ağ trafiğini diğer Microsoft Azure müşterilerine erişilebilir değil güvence altına yardımcı olur.

Daha fazla bilgi edinin:

* [Azure ağ güvenliğine genel bakış](security-network-overview.md)
* [Sanal Ağ’a genel bakış](../virtual-network/virtual-networks-overview.md)
* [Ağ özellikleri ve kuruluş senaryolarına yönelik ortaklıkları](https://azure.microsoft.com/blog/networking-enterprise/)

## <a name="security-policy-management-and-reporting"></a>Güvenlik İlkesi Yönetimi ve Raporlama
Azure Güvenlik Merkezi, engellemenize, algılamanıza ve tehditlerine karşı yanıt yardımcı olur. Güvenlik Merkezi verir görünürlük artan ve Azure kaynaklarınızın güvenlik üzerinden, denetim. Aboneliklerinizde tümleşik güvenlik izleme ve ilke yönetimi, Azure sağlar. Kaçabilecek ve güvenlik çözümlerinin geniş ekosistemiyle ile çalışır tehditleri algılayabilir yardımcı olur.

Güvenlik Merkezi iyileştirmek ve sanal makineler tarafından güvenlik izlemenize yardımcı olur:

* Sağlama [güvenlik önerileri](../security-center/security-center-recommendations.md) sanal makineler için. Örnek önerileri içerir: Sistem güncelleştirmeleri uygulamak, ACL'ler uç noktalarını yapılandırma, kötü amaçlı yazılımdan koruma etkinleştirmek, ağ güvenlik gruplarını etkinleştirmek ve disk şifrelemesi uygulamak.
* Sanal makinelerinizi durumunu izleme.

Daha fazla bilgi edinin:

* [Azure Güvenlik Merkezi'ne Giriş](../security-center/security-center-intro.md)
* [Azure Güvenlik Merkezi sık sorulan sorular](../security-center/security-center-faq.md)
* [Azure Güvenlik Merkezi planlama ve işlemler](../security-center/security-center-planning-and-operations-guide.md)

## <a name="compliance"></a>Uyumluluk
Azure sanal makineleri FISMA, FedRAMP, HIPAA, PCI DSS düzey 1 ve diğer anahtar uyumluluk programları için sertifikalıdır. Bu sertifika, uyumluluk gereksinimlerini karşılamak üzere kendi Azure uygulamalarını ve çok çeşitli yurtiçi ve uluslararası yönetmelik gereksinimlerine yönelik olarak işinizi kolaylaştırır.

Daha fazla bilgi edinin:

* [Microsoft Güven Merkezi: uyumluluk](https://www.microsoft.com/TrustCenter/Compliance/default.aspx)
* [Güvenilir bulut: Microsoft Azure güvenlik, gizlilik ve uyumluluk](http://download.microsoft.com/download/1/6/0/160216AA-8445-480B-B60F-5C8EC8067FCA/WindowsAzure-SecurityPrivacyCompliance.pdf)
