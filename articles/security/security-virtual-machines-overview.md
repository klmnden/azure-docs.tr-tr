---
title: "Azure sanal makineler ile kullanılan azure güvenlik özellikleri | Microsoft Docs"
description: " Azure sanal makinelerle kullanılan çekirdek Azure güvenlik özelliklerine genel bakış. Azure VM'ler satın alın ve VM çalıştıran fiziksel donanımı sürdürmek zorunda kalmadan sanallaştırma esnekliği sağlar. "
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
ms.date: 05/04/2017
ms.author: terrylan
ms.openlocfilehash: f1fb7f876c7dc010c03f01a4f6698ddc18da1100
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-virtual-machines-security-overview"></a>Azure sanal makineleri güvenliğine genel bakış
Azure Sanal Makineler, çok sayıda bilgi işlem çözümünü çevik bir şekilde dağıtmanızı sağlar. Microsoft Windows, Linux, Microsoft SQL Server, Oracle, IBM, SAP ve Azure BizTalk Hizmetleri desteği sayesinde, istediğiniz iş yükünü istediğiniz dilde ve neredeyse tüm işletim sistemlerinde dağıtabilirsiniz.

Bir Azure sanal makinesi, sanal makineyi çalıştıran fiziksel donanımı satın almanıza ve muhafaza etmenize gerek kalmadan size sanallaştırma esnekliği sunar.  Derleme ve verilerinizi korumalı ve yüksek güvenlikli bizim veri merkezlerinde güvenli olduğunu güvence uygulamalarınızla dağıtın.

Azure ile gelişmiş güvenlik, uyumlu çözümler oluşturabilir:

* Sanal makinelerinizi virüslerden ve kötü amaçlı yazılımlardan koruyun
* Hassas verilerinizi şifreleyin
* Ağ trafiğinin güvenliğini sağlayın
* Tehditleri belirleyin ve tespit edin
* Uyumluluk gereksinimlerini karşılayın

Bu makalede amacı, sanal makinelerle kullanılabilir Azure güvenlik özellikleri çekirdek genel bir bakış sağlamaktır. Daha fazla bilgi için her bir özelliğin ayrıntılarını veren makalelerinin bağlantıları da sunuyoruz.  

Bu makalede ele alınacak temel Azure sanal makine güvenlik özellikleri:

* Kötü Amaçlı Yazılımdan Koruma
* Donanım güvenlik modülü
* Sanal makine disk şifrelemesi
* Sanal makine yedekleme
* Azure Site Recovery
* Sanal ağ
* Güvenlik İlkesi Yönetimi ve Raporlama
* Uyumluluk

## <a name="antimalware"></a>Kötü Amaçlı Yazılımdan Koruma
Azure ile Microsoft, Symantec, eğilim mikro ve Kaspersky gibi güvenlik satıcılardan kötü amaçlı yazılımdan koruma yazılımı, sanal makineleriniz kötü amaçlı dosyalar, reklam ve diğer tehditlerden korumak için kullanabilirsiniz. Daha fazla bilgi makalelerini iş ortağı çözümleri bulmak için aşağıda bölümüne bakın.

Azure Cloud Services ve sanal makineleri için Microsoft Antimalware tanımlamak ve virüsler, casus yazılım ve diğer kötü amaçlı yazılım kaldırma yardımcı olan gerçek zamanlı koruma bir özelliktir.  Microsoft Antimalware kötü amaçlı veya istenmeyen yazılım kendini yüklemeye veya Azure sistemleriniz üzerinde çalışmaya girişiminde bilinen yapılandırılabilir uyarır sağlar.

Microsoft Antimalware, uygulamaları ve İnsan aracılığı olmadan arka planda çalışması için tasarlanmış Kiracı ortamları için bir tek Aracısı çözümüdür. Her iki temel güvenli stl'deki varsayılan ile uygulama iş yüklerinin gereksinimlerini temel veya Gelişmiş kötü amaçlı yazılımdan koruma izleme de dahil olmak üzere özel yapılandırma koruması dağıtabilirsiniz.

Aşağıdaki temel özellikleri, dağıtmak ve Microsoft Antimalware etkinleştirmek seçtiğinizde kullanılabilir:

* Gerçek zamanlı koruma - bulut Hizmetleri ve Algıla ve kötü amaçlı yazılım yürütme engellemek için sanal makinelerde monitör etkinliği.
* Zamanlanmış tarama - düzenli olarak gerçekleştirir etkin olarak çalışan programlar dahil olmak üzere kötü amaçlı yazılım, algılamak için hedeflenen tarama.
* Kötü amaçlı yazılım düzeltme - otomatik olarak gerçekleştirilmesi eylem Algılanan kötü amaçlı yazılım silme veya kötü amaçlı dosyaları karantinaya alma ve kötü amaçlı kayıt defteri girdilerini temizleme gibi üzerinde.
* İmza güncelleştirmeleri - otomatik olarak koruma sağlamak için en son koruma imzaları (virüs tanımları) yükler önceden belirlenen bir frekansında güncel.
* Kötü amaçlı yazılımdan koruma Altyapısı güncelleştirmeleri – otomatik olarak Microsoft Antimalware altyapısı güncelleştirir.
* Kötü amaçlı yazılımdan koruma platformu güncelleştirmeleri – otomatik olarak Microsoft Antimalware platform güncelleştirir.
* Etkin bir koruma - Azure telemetri meta verileri hızlı yanıt emin olmak için algılanan tehditlere ve şüpheli kaynaklar hakkında raporlar ve gerçek zamanlı zaman uyumlu imza teslim aracılığıyla Microsoft etkin koruma sistem (MAPS) sağlar.
* Raporlama örnekleri - sağlar ve hizmeti geliştirmek ve sorun giderme etkinleştirmek yardımcı olmak için Microsoft Antimalware hizmet örnekleri raporlar.
* Dışlamalar – koruma ve performans ve diğer nedenler için tarama dışında tutmak için sürücü ve uygulama ve hizmet yöneticileri işlemleri, belirli dosyaları yapılandırma sağlar.
* Kötü amaçlı yazılımdan koruma olay toplama - kötü amaçlı yazılımdan koruma hizmeti durumu, kuşkulu etkinlikleri ve işletim sistemi olay günlüğünde gerçekleştirilecek düzeltme eylemleri kaydeder ve Müşteri'nin Azure Storage hesabınıza toplar.

Daha fazla bilgi edinin: kötü amaçlı yazılımdan koruma yazılımı, sanal makineleri koruma hakkında daha fazla bilgi için bkz:

* [Azure bulut Hizmetleri ve sanal makineleri için Microsoft kötü amaçlı yazılımdan koruma](azure-security-antimalware.md)
* [Azure Sanal Makinelerinde Kötü Amaçlı Yazılıma Karşı Koruma Çözümleri Dağıtma](https://azure.microsoft.com/blog/deploying-antimalware-solutions-on-azure-virtual-machines/)
* [Yükleme ve bir Windows VM bir hizmet olarak eğilimi mikro derin güvenliği yapılandırma](../virtual-machines/windows/classic/install-trend.md)
* [Yükleme ve Symantec Endpoint Protection bir Windows VM yapılandırma](../virtual-machines/windows/classic/install-symantec.md)
* [Azure Marketi'nde güvenlik çözümleri](https://azure.microsoft.com/marketplace/?term=security)

## <a name="hardware-security-module"></a>Donanım güvenlik modülü
Şifreleme ve kimlik doğrulama korumaları anahtar güvenlik geliştirerek geliştirilebilir. Azure anahtar kasası depolayarak güvenlik kritik parolaları ve anahtarlar ve Yönetimi basitleştirebilir. Key Vault, anahtarlarınızı FIPS 140-2 2. Düzey standartlarıyla sertifikalanmış olan donanım güvenlik modüllerinde (HSM'ler) depolama seçeneği sunar. SQL Server şifreleme anahtarları için yedekleme veya [saydam veri şifreleme](https://msdn.microsoft.com/library/bb934049.aspx) herhangi bir anahtar veya gizli, uygulamalardan ile anahtar kasasına tüm depolanabilir. İzinleri ve korumalı bu öğelere erişimi üzerinden yönetilir [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/).

Daha fazla bilgi edinin:

* [Azure anahtar kasası nedir?](../key-vault/key-vault-whatis.md)
* [Azure anahtar kasası ile çalışmaya başlama](../key-vault/key-vault-get-started.md)
* [Azure anahtar kasası blogu](https://blogs.technet.microsoft.com/kv/)

## <a name="virtual-machine-disk-encryption"></a>Sanal makine disk şifrelemesi
Azure Disk şifrelemesi, Windows ve Linux Azure sanal makine diskleriniz şifrelemek olanak sağlayan yeni bir özelliktir. Azure Disk şifrelemesi kullanılır endüstri standardı [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) Windows özelliğidir ve [dm-crypt](https://en.wikipedia.org/wiki/Dm-crypt) Linux işletim sistemi ve veri diskleri için birim şifrelemesi sağlamak için özelliğidir.

Çözüm denetlemek ve disk şifreleme anahtarları ve gizli anahtarları Azure depolama alanınızı bekleyen sanal makine disklerdeki tüm veriler şifrelenir sağlarken anahtar kasası aboneliğinizde yönetmenize yardımcı olmak için Azure anahtar kasası ile tümleşiktir.

Daha fazla bilgi edinin:

* [Windows ve Linux Iaas VM'ler için Azure Disk şifrelemesi](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)
* [Linux ve Windows sanal makineler için Azure Disk şifrelemesi](https://blogs.msdn.microsoft.com/azuresecurity/2015/11/16/azure-disk-encryption-for-linux-and-windows-virtual-machines-public-preview-now-available/)
* [Bir sanal makine şifrele](../security-center/security-center-disk-encryption.md)

## <a name="virtual-machine-backup"></a>Sanal makine yedekleme
Azure Yedekleme, uygulama verilerinizi sıfır sermaye yatırımı ve en az işletim maliyetiyle koruyan ölçeklenebilir bir çözümdür. Uygulama hataları verilerinizi bozabilir ve insan hataları, uygulamalarınızda hatalar oluşturabilir. Azure Backup ile Windows ve Linux çalıştıran sanal makinelerinizi korunur.

Daha fazla bilgi edinin:

* [Azure Backup nedir?](../backup/backup-introduction-to-azure-backup.md)
* [Azure yedekleme öğrenme yolu](https://azure.microsoft.com/documentation/learning-paths/backup/)
* [Azure Backup hizmeti - SSS](../backup/backup-azure-backup-faq.md)

## <a name="azure-site-recovery"></a>Azure Site Recovery
Kuruluşunuzun BCDR stratejisinin önemli bir bölümü, planlı veya planlanmamış kesintiler meydana geldiğinde kurumsal iş yüklerini ve uygulamaları açık ve çalışır durumda tutma yöntemlerini ele alır. Azure Site Recovery birincil konumunuz çökmesi durumunda ikincil konum kullanılabilir olacak şekilde, çoğaltma, yük devretme ve kurtarma iş yüklerini ve uygulamaları düzenlemek yardımcı olur.

Site Recovery:

* **BCDR stratejinize basitleştirir** — Site Recovery çoğaltma, yük devretme ve tek bir konumdan birden çok iş yükünün ve uygulamanın kurtarılması işlemek kolaylaştırır. Site Recovery, çoğaltma ve yük devretme işlemlerini düzenler ancak uygulama verilerinize müdahale etmez veya uygulamanız hakkında bilgiler toplamaz.
* **Esnek çoğaltma sağlar** — Site Recovery kullanarak Hyper-V sanal makineleri, VMware sanal makineleri ve Windows/Linux fiziksel sunucularında çalışan iş yüklerini çoğaltabilir.
* **Yük devretme ve kurtarma destekler** — Site Recovery, üretim ortamlarını etkilemeden olağanüstü durum kurtarma ayrıntılarını desteklemek için yük devretme testlerini sağlar. Ayrıca, beklenen kesintilere yönelik olarak sıfır veri kaybı sunan planlanan yük devretmeler veya beklenmeyen olağanüstü durumlar için minimum düzeyde veri kaybıyla sonuçlanan (çoğaltma sıklığına bağlı olarak) planlanmamış yük devretmeler çalıştırabilirsiniz. Yük devretmenin ardından birincil sitelerinizde yeniden çalışma işlemini gerçekleştirebilirsiniz. Yük devretme işlemini özelleştirebilmeniz ve çok katmanlı uygulamaları kurtarabilmeniz için Site Recovery, betikleri ve Azure otomasyonu çalışma kitaplarını içeren kurtarma planları sunar.
* **İkincil veri merkezine ortadan** — bir ikincil şirket içi siteye veya azure'a çoğaltabilirsiniz. Azure için olağanüstü durum kurtarma hedefi olarak kullanarak, maliyet ve bir ikincil site Bakımı karmaşıklığını ortadan kaldırır. Çoğaltılan veriler Azure depolama alanında depolanır.
* **Var olan BCDR teknolojileri ile tümleşir** — Site Recovery iş ortaklarına, diğer uygulama BCDR özellikleriyle. Örneğin, kurumsal iş yüklerinin SQL Server arka ucunu korumak için Site Recovery kullanabilirsiniz. Bu SQL Server AlwaysOn Kullanılabilirlik grupları için yük devretme yönetmek için yerleşik destek içerir.

Daha fazla bilgi edinin:

* [Azure Site Recovery nedir?](../site-recovery/site-recovery-overview.md)
* [Azure Site Recovery nasıl çalışır?](../site-recovery/site-recovery-components.md)
* [Hangi iş yüklerini Azure Site Recovery tarafından korunan?](../site-recovery/site-recovery-workload.md)

## <a name="virtual-networking"></a>Sanal ağ
Sanal makinelerin ağ bağlantısı gerekir. Bu gereksinimi desteklemek için sanal makinelerin bir Azure sanal ağına bağlı Azure gerektirir. Bir Azure sanal ağ üzerinde fiziksel Azure ağ doku yerleşik mantıksal bir yapıdır. Her mantıksal Azure sanal tüm diğer Azure sanal ağlardan yalıtılmış bir ağdır. Bu yalıtım dağıtımlarınızı ağ trafiğini diğer Microsoft Azure müşterilerine erişilebilir değil güvence altına yardımcı olur.

Daha fazla bilgi edinin:

* [Azure ağ güvenliğine genel bakış](security-network-overview.md)
* [Sanal ağ genel bakış](../virtual-network/virtual-networks-overview.md)
* [Ağ özellikleri ve kuruluş senaryolarına yönelik ortaklıkları](https://azure.microsoft.com/blog/networking-enterprise/)

## <a name="security-policy-management-and-reporting"></a>Güvenlik İlkesi Yönetimi ve Raporlama
Azure Güvenlik Merkezi, engellemenize, algılamanıza ve tehditlerine karşı yanıt yardımcı olur ve artan, görünürlük ve denetim üzerinden, Azure kaynaklarınızın güvenliğini sağlar. Aboneliklerinizde, tümleşik güvenlik izleme ve ilke yönetimi sağlar; normal koşullarda gözden kaçabilecek tehditleri algılamaya yardımcı olur ve güvenlik çözümlerinin geniş ekosistemiyle çalışır.

Azure Güvenlik Merkezi iyileştirmek ve sanal makine güvenliği tarafından izlemenize yardımcı olur:

* Sanal makine sağlama [güvenlik önerileri](../security-center/security-center-recommendations.md) gibi sistem güncelleştirmeleri uygulamak gibi ACL'ler uç noktalarını yapılandırma, kötü amaçlı yazılımdan koruma etkinleştirmek, ağ güvenlik gruplarını etkinleştirmek ve disk şifrelemesi uygulayın.
* Sanal makinelerinizi durumunu izleme

Daha fazla bilgi edinin:

* [Azure Güvenlik Merkezi'ne giriş](../security-center/security-center-intro.md)
* [Azure Güvenlik Merkezi sık sorulan sorular](../security-center/security-center-faq.md)
* [Azure Güvenlik Merkezi planlama ve işlemler](../security-center/security-center-planning-and-operations-guide.md)

## <a name="compliance"></a>Uyumluluk
Azure sanal makineleri FISMA, FedRAMP, HIPAA, PCI DSS düzey 1 ve diğer anahtar uyumluluk programları için sertifikalıdır. Bu sertifika, uyumluluk gereksinimlerini karşılamak üzere kendi Azure uygulamalarını ve çok çeşitli yurtiçi ve uluslararası yönetmelik gereksinimlerine yönelik olarak işinizi kolaylaştırır.

Daha fazla bilgi edinin:

* [Microsoft Güven Merkezi: uyumluluk](https://www.microsoft.com/TrustCenter/Compliance/default.aspx)
* [Güvenilir bulut: Microsoft Azure güvenlik, gizlilik ve uyumluluk](http://download.microsoft.com/download/1/6/0/160216AA-8445-480B-B60F-5C8EC8067FCA/WindowsAzure-SecurityPrivacyCompliance.pdf)
