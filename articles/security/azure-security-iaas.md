---
title: Güvenlik en iyi yöntemler için Iaas iş yüklerini azure'da | Microsoft Docs
description: " Azure Iaas iş yüklerinin geçişini bizim tasarımları yeniden değerlendirmek için fırsatlar sunar "
services: security
documentationcenter: na
author: barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 02c5b7d2-a77f-4e7f-9a1e-40247c57e7e2
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/05/2019
ms.author: barclayn
ms.openlocfilehash: f4b2506781df5572ddaff8dda34bf3edab8987be
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65145212"
---
# <a name="security-best-practices-for-iaas-workloads-in-azure"></a>Azure Iaas iş yükleri için en iyi güvenlik uygulamaları
Bu makale, Vm'leri ve işletim sistemleri için en iyi güvenlik uygulamaları açıklar.

Bir fikrim fikir birliği üzerinde en iyi uygulamaları temel alır ve geçerli Azure platform özellikleriyle çalışma ve özellik kümeleri. Fikirlerini ve teknolojileri zaman içinde değişebileceği için bu makalede, bu değişiklikleri yansıtacak şekilde güncelleştirilir.

Bir hizmet (Iaas) senaryoları olarak çoğu altyapısında [Azure sanal makineleri (VM'ler)](https://docs.microsoft.com/azure/virtual-machines/) bulut kullanan kuruluşlar için ana iş yükü olan bilgi işlem. Bu, yetkisiz değiştirmeye karşı korumalı bir gerçeğidir [karma senaryolar](https://social.technet.microsoft.com/wiki/contents/articles/18120.hybrid-cloud-infrastructure-design-considerations.aspx) kuruluşlar yavaş iş yüklerini buluta taşımak istediğiniz. Böyle senaryolarda izleyin [Iaas için genel güvenlik konuları](https://social.technet.microsoft.com/wiki/contents/articles/3808.security-considerations-for-infrastructure-as-a-service-iaas.aspx)ve en iyi güvenlik uygulamaları, tüm Vm'lere uygulayabilirsiniz.

## <a name="shared-responsibility"></a>Paylaşılan sorumluluk
Güvenlik için sizin sorumluluğunuzdadır, bulut hizmeti türüne bağlıdır. Aşağıdaki grafik, hem Microsoft hem de sizin sorumluluğunuzdadır bakiyesini özetlenmektedir:

![Sorumluluk alanları](./media/azure-security-iaas/sec-cloudstack-new.png)

Güvenlik gereksinimleri bir dizi farklı türde iş yükleri gibi etkenlere bağlı olarak farklılık gösterir. Bu en iyi bir tek başına sistemlerinizin güvenliğini sağlayabilirsiniz. Başka bir şey gibi güvenlik, size uygun seçenekleri belirleyin ve nasıl çözümler birbirine boşlukları doldurarak tamamlayabilir görmek sahip.

## <a name="protect-vms-by-using-authentication-and-access-control"></a>Kimlik doğrulaması ve erişim denetimi kullanarak Vm'leri koruma
Vm'lerinizi koruma ilk adımı, yeni VM'ler ve VM erişimi, yalnızca yetkili kullanıcıların emin olmak için sonuna ayarlayabilirsiniz ' dir.

> [!NOTE]
> Azure'da Linux Vm'lerini güvenliğini artırmak için Azure AD kimlik doğrulamasıyla tümleştirilebilir. Kullanırken [Linux VM'ler için Azure AD kimlik doğrulaması](../virtual-machines/linux/login-using-aad.md), merkezi olarak denetlemek ve verdiği veya erişimini engellediği Vm'lere ilkeleri zorunlu tutmanıza.
>
>

**En iyi yöntem**: VM erişimi denetler.   
**Ayrıntı**: Kullanım [Azure ilkeleri](../azure-policy/azure-policy-introduction.md) , kuruluşunuzdaki kaynaklar için kuralları kurmak ve özelleştirilmiş ilkeler oluşturun. Bu ilkeler gibi kaynaklara uygulamak [kaynak grupları](../azure-resource-manager/resource-group-overview.md). Bir kaynak grubuna ait VM'lerin onun ilkelerini devralır.

Kuruluşunuzun fazla aboneliğiniz varsa, erişim ilkeleri ve bu Aboneliklerdeki uyumluluğunun verimli bir şekilde yönetmek için bir yol gerekebilir. [Azure Yönetim grupları](../azure-resource-manager/management-groups-overview.md) abonelikleri yukarıda kapsam düzeyini sağlamak. Yönetim gruplarına (kapsayıcılar) abonelikleri düzenlemenize ve bu gruplara, idare koşullar geçerlidir. Bir yönetim grubundaki tüm abonelikler otomatik olarak gruba uygulanan koşullar devralır. Yönetim grupları, sahip olabileceğiniz abonelik türüne bakılmaksızın kurumsal düzeyde yönetimi büyük ölçekte sunar.

**En iyi yöntem**: Kurulum ve dağıtım VM'lerin sonuçlarındaki azaltın.   
**Ayrıntı**: Kullanım [Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) dağıtım seçenekleriniz güçlendirin ve anlayın ve ortamınızda Vm'leri stok daha kolay hale getirmek için şablonları.

**En iyi yöntem**: Güvenli ayrıcalıklı erişim.   
**Ayrıntı**: Kullanım bir [en az ayrıcalık yaklaşım](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/plan/security-best-practices/implementing-least-privilege-administrative-models) ve kullanıcıların erişim ve sanal makinelerini ayarlayın sağlamak için Azure yerleşik rolleri:

- [Sanal makine Katılımcısı](../role-based-access-control/built-in-roles.md#virtual-machine-contributor): VM'ler, ancak bağlı sanal ağ veya depolama hesabı değil yönetebilirsiniz.
- [Klasik sanal makine Katılımcısı](../role-based-access-control/built-in-roles.md#classic-virtual-machine-contributor): VM'lerin bağlanacağı sanal ağ veya depolama hesabı değil ancak klasik dağıtım modeli kullanılarak oluşturulan sanal makineleri yönetebilir.
- [Güvenlik Yöneticisi](../role-based-access-control/built-in-roles.md#security-admin): Güvenlik Merkezi'nde yalnızca: Güvenlik ilkelerini görüntüleyin, güvenlik durumlarını görüntülemek, güvenlik ilkeleri, uyarıları görüntüleme ve öneriler Düzenle, uyarıları ve öneriler kapat.
- [DevTest Labs kullanıcısı](../role-based-access-control/built-in-roles.md#devtest-labs-user): Her şeyi görüntüleyebilir ve bağlanmak, Başlat, yeniden başlatın ve Vm'lerini kapatın.

Abonelik yöneticileri ve ortak Yöneticiler bir Abonelikteki tüm VM'lerin Yöneticiler yönetilmelerini, bu ayarı değiştirebilirsiniz. Tüm abonelik yöneticileri ve ortak Yöneticiler makinelerinizi birine oturum açmak için güvenilir olduğundan emin olun.

> [!NOTE]
> Aynı kaynak grubuna VM ile aynı yaşam döngüsünü birleştirmek öneririz. Kaynak grupları kullanarak dağıtmak, izlemek ve maliyetleri kaynaklarınız için fatura alma.
>
>

VM erişimi ve kurulum denetim kuruluşlar kendi genel VM güvenliği artırın.

## <a name="use-multiple-vms-for-better-availability"></a>Birden çok VM için daha iyi kullanılabilirlik kullanın
Sanal makinenizin yüksek kullanılabilirlik sağlamak için gereken Kritik uygulamalar çalıştırıyorsa, birden çok sanal makine kullanmanızı öneririz. Daha iyi kullanılabilirlik için kullanma bir [kullanılabilirlik kümesi](../virtual-machines/windows/manage-availability.md#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy).

Bir kullanılabilirlik kümesi içine yerleştirdiğiniz sanal makine kaynaklarının bir Azure veri merkezinde dağıtılan birbirinden yalıtılmış olmasını sağlamak için Azure'da kullanabileceğiniz bir mantıksal bir gruplandırmasıdır. Azure Vm'leri bir kullanılabilirlik yerleştirdiğiniz birden fazla fiziksel sunucu, işlem raflar, depolama birimi ve ağ anahtarları arasında çalıştırma ayarlamanızı sağlar. Bir donanım veya Azure yazılım hatası oluşursa yalnızca sanal makinelerinizin bir alt etkilenir ve genel uygulama müşterilerinizin kullanımına sunulmaya devam eder. Güvenilir bulut çözümleri oluşturmak istediğinizde kullanılabilirlik kümelerini gerekli özellikleridir.

## <a name="protect-against-malware"></a>Kötü amaçlı yazılımlardan korunun
Belirlenmesi ve virüslerin, casus yazılımların ve diğer kötü amaçlı yazılım kaldırılmasına yardımcı olmak üzere kötü amaçlı yazılımdan korunma yüklemeniz gerekir. Yükleyebileceğiniz [Microsoft Antimalware](azure-security-antimalware.md) veya bir Microsoft iş ortağı uç nokta koruma çözümüne ([Trend Micro](https://help.deepsecurity.trendmicro.com/azure-marketplace-getting-started-with-deep-security.html), [Symantec](https://www.symantec.com/products), [McAfee](https://www.mcafee.com/us/products.aspx), [Windows Defender](https://www.microsoft.com/search/result.aspx?q=Windows+defender+endpoint+protection), ve [System Center Endpoint Protection](https://www.microsoft.com/search/result.aspx?q=System+Center+endpoint+protection)).

Microsoft Antimalware gerçek zamanlı koruma, zamanlanmış tarama, kötü amaçlı yazılım düzeltme, imza güncelleştirmeleri, altyapı güncelleştirmeleri, raporlama örnekleri ve dışlama olay koleksiyonu gibi özellikler içerir. Üretim ortamınızdan ayrı olarak barındırılan ortamlar için bulut Hizmetleri ve sanal makinelerinizin korunmasına yardımcı olmak için bir kötü amaçlı yazılımdan koruma uzantısını kullanabilirsiniz.

Microsoft Antimalware ve iş ortağı çözümleriyle tümleştirilebilir [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/) kolay dağıtım ve yerleşik algılamalar (Uyarılar ve olaylar).

**En iyi yöntem**: Kötü amaçlı yazılımlara karşı korumak için bir kötü amaçlı yazılımdan koruma çözümü yükleyin.   
**Ayrıntı**: [Bir Microsoft iş ortağı çözüm ya da Microsoft Antimalware yükleyin](../security-center/security-center-install-endpoint-protection.md)

**En iyi yöntem**: Kötü amaçlı yazılımdan koruma çözümünüz, koruma durumunu izlemek için Güvenlik Merkezi ile tümleştirin.   
**Ayrıntı**: [Güvenlik Merkezi ile uç nokta koruma sorunları yönetme](../security-center/security-center-partner-integration.md)

## <a name="manage-your-vm-updates"></a>VM güncelleştirmelerinizi yönetme
Tüm şirket içi sanal makineler gibi Azure Vm'leri yönetilen kullanıcı yöneliktir. Azure Windows güncelleştirmeleri gönderdiğinizde değil. VM güncelleştirmelerinizi yönetmeniz gerekmez.

**En iyi yöntem**: Sanal makinelerinizi güncel kalmasını sağlayın.   
**Ayrıntı**: Kullanım [güncelleştirme yönetimi](../automation/automation-update-management.md) yönetmek için Azure automation'da işletim sistemi güncelleştirmeleri, Azure'da dağıtılan Windows ve Linux bilgisayarlar için şirket içi ortamlarda veya diğer bulut çözümü sağlayıcıları. Tüm aracı bilgisayarlardaki kullanılabilir güncelleştirmelerin durumunu hızla değerlendirebilir ve sunucular için gerekli güncelleştirmeleri yükleme işlemini yönetebilirsiniz.

Güncelleştirme yönetimi tarafından yönetilen bilgisayarlar değerlendirme gerçekleştirmek ve güncelleştirme dağıtımları için aşağıdaki yapılandırmaları kullanın:

- Microsoft Monitoring Agent (MMA) Windows veya Linux için
- Linux için PowerShell İstenen Durum Yapılandırması (DSC)
- Otomasyon Karma Runbook Çalışanı
- Microsoft Update veya Windows bilgisayarlar için Windows Server Update Services (WSUS)

Windows Update kullanıyorsanız, otomatik Windows güncelleştirme ayarı etkin bırakın.

**En iyi yöntem**: Dağıtım sırasında oluşturulan görüntüleri en son Windows güncelleştirmelerini turu eklediğinizden emin olun.   
**Ayrıntı**: Denetleyin ve her dağıtımın ilk adım olarak tüm Windows güncelleştirmeleri yükleyin. Bu ölçü, veya kendi kitaplığı gelen görüntüleri dağıtırken uygulamak özellikle önemlidir. Görüntüleri Azure Market'te varsayılan olarak otomatik olarak güncelleştirilir ancak bir genel yayın sonrasında bir gecikme süresi (en fazla birkaç hafta) olabilir.

**En iyi yöntem**: Düzenli olarak yeni bir işletim sistemi sürümünü zorunlu kılmak için sanal makinelerinizin yeniden dağıtın.   
**Ayrıntı**: VM'nizi tanımlayan bir [Azure Resource Manager şablonu](../azure-resource-manager/resource-group-authoring-templates.md) şekilde kolayca yeniden dağıtabilirsiniz. İhtiyacınız olduğunda bir şablon kullanarak düzeltme eki uygulama ve güvenli bir sanal makine sağlar.

**En iyi yöntem**: Hızlı bir şekilde güvenlik güncelleştirmeleri, Vm'lere uygulanır.   
**Ayrıntı**: Azure Güvenlik Merkezi'ni etkinleştirin (ücretsiz katman veya standart katman) için [eksik güvenlik güncelleştirmelerini tanımlamaya ve bunları uygulamak](../security-center/security-center-apply-system-updates.md).

**En iyi yöntem**: En son güvenlik güncelleştirmelerini yükleyin.   
**Ayrıntı**: Müşterilerin Azure'a taşımak ilk iş yüklerinin laboratuvarlar ve dönük sistemleri bazılarıdır. Azure sanal makineleriniz, internet'e erişilmesi gereken uygulamaları veya hizmetleri barındırıyorsanız, düzeltme eki uygulama hakkında dikkatli olun. İşletim sisteminin ötesine geçen eki. İş ortağı uygulamaları Açıklarında güvenlik açıklarını da iyi düzeltme eki yönetimi yerinde olduğunda önlenebilir sorunlara neden olabilir.

**En iyi yöntem**: Dağıtma ve bir yedekleme çözümü test edin.   
**Ayrıntı**: Bir yedekleme başka bir işlem işleme aynı şekilde ele alınması gerekir. Bu, üretim ortamınızın buluta genişletme parçası olan sistemlerinin geçerlidir.

Test ve geliştirme sistemleri, hangi kullanıcıların dayalı olarak, şirket içi ortamlarla deneyimlerini alışık için benzer bir geri yükleme özelliklerini sağlayın yedekleme stratejileri izlemeniz gerekir. Üretim iş yüklerini Azure'a taşındı. mevcut yedekleme çözümleri ile mümkün olduğunda tümleşmelidir. Veya, kullanabileceğiniz [Azure Backup](../backup/backup-azure-vms-first-look-arm.md) yedekleme gereksinimlerinizi çözümüne yardımcı olmak için.

Yazılım güncelleştirme ilkeleri zorunlu olmayan kuruluşlar, bilinen, daha önce sabit güvenlik açıklarından tehditlerine karşı daha sunulur. Sektör yönetmeliklerine uyum sağlamak için şirketler dikkatli olmaları ve iş yüklerini güvenliğini sağlamaya yardımcı olmak için doğru güvenlik denetimlerini kullanarak bulutta konumlandırılmış kanıtlamaları gerekir.

Yazılım güncelleştirme geleneksel bir veri merkezi için en iyi uygulamalar ve Azure Iaas birçok benzerlikler. Azure'da yer alan VM'ler dahil etmek için geçerli yazılım güncelleştirme ilkelerini değerlendirme öneririz.

## <a name="manage-your-vm-security-posture"></a>VM Güvenlik duruşunuzu yönetme
Logic'in artmaktadır. Vm'lerinizi koruma hızla tehditleri algılamak, kaynaklarınıza yetkisiz erişimi engelleme, uyarıları tetikleyin ve hatalı pozitif sonuçları azaltmak bir izleme yeteneği gerektirir.

Güvenlik açısından duruşunu izlemek için [Windows](../security-center/security-center-virtual-machine.md) ve [Linux Vm'leri](../security-center/security-center-linux-virtual-machine.md), kullanın [Azure Güvenlik Merkezi](../security-center/security-center-intro.md). Güvenlik Merkezi'nde, aşağıdaki özelliklerden yararlanarak Vm'lerinizi koruma:

- Önerilen yapılandırma kuralları ile işletim sistemi güvenlik ayarlarını uygulayın.
- Tanımlar ve sistem güvenliği ve eksik kritik güncelleştirmeleri yükleyin.
- Uç nokta kötü amaçlı yazılımdan korunma için öneriler dağıtın.
- Disk şifrelemesi doğrulayın.
- Değerlendirebilir ve güvenlik açıklarını düzeltin.
- Tehditleri algılayın.

Güvenlik Merkezi, tehditleri etkin bir şekilde izleyebilir ve olası tehditleri Güvenlik Uyarıları ' kullanıma sunulur. Bağıntılı tehditleri adlı bir güvenlik olayı, tek bir görünümde toplanır.

Güvenlik Merkezi veri depolar [Azure İzleyicisi](../log-analytics/log-analytics-overview.md). Azure İzleyici günlüklerine uygulamalarınızın ve kaynaklarınızın çalışmasını öngörülerin bir sorgu dili ve analiz altyapısı sağlar. Veri öğesinden toplanan ayrıca [Azure İzleyici](../monitoring-and-diagnostics/monitoring-overview.md), yönetim çözümleri ve bulutta veya şirket içi sanal makinelerde yüklü aracıları. Bu paylaşılan işlevsellik, ortamınızın eksiksiz bir resmini oluşturmanıza yardımcı olur.

Güçlü güvenlik kendi Vm'leri için zorlamaz; kuruluşların, güvenlik denetimleri sağlamasına olası girişimleri yetkisiz kullanıcılar tarafından farkında kalır.

## <a name="monitor-vm-performance"></a>Sanal makine performansını izleme
VM işlem izin verilenden daha fazla kaynak tüketmesine kaynak uygunsuz bir sorun olabilir. Bir VM ile performans sorunlarını, kullanılabilirlik, güvenlik ilkesini ihlal hizmet kesintisi için neden olabilir. Yüksek CPU veya bellek kullanımı, bir hizmet reddi (DoS) saldırısı gösterebilir, çünkü bu IIS veya diğer web sunucularını barındıran VM'ler için özellikle önemlidir. Bir sorun gerçekleşirken öngörülebiliyorsa değil yalnızca VM erişimi izlemek için zorunludur aynı zamanda normal çalışma sırasında ölçülen proaktif olarak performansınız karşı.

Kullanmanızı öneririz [Azure İzleyici](../monitoring-and-diagnostics/monitoring-overview-metrics.md) kaynağınızın durumu görünürlük elde etmek için. Azure İzleyici özellikleri:

- [Kaynak tanılama günlük dosyaları](../azure-monitor/platform/diagnostic-logs-overview.md): VM kaynaklarınızı izler ve performans ve kullanılabilirlik riske atabilirdi olası sorunları tanımlar.
- [Azure tanılama uzantısını](../azure-monitor/platform/diagnostics-extension-overview.md): Windows Vm'lerinde izleme ve tanılama olanakları sunar. Bu özellikler uzantısı bir parçası olarak dahil ederek etkinleştirebilirsiniz [Azure Resource Manager şablonu](../virtual-machines/windows/extensions-diagnostics-template.md).

VM performansı izleme kuruluşlar, bazı değişikliklerin performans desenleri normal veya anormal olup olmadığını belirleyemiyor. Normalden daha fazla kaynak tüketmeye bir VM, bir dış kaynağa veya VM'de çalışan güvenliği aşılmış bir işlem bir saldırı olduğunu gösteriyor olabilir.

## <a name="encrypt-your-virtual-hard-disk-files"></a>Sanal sabit disk dosyaları şifreleyin
Sanal sabit disklerinizin (VHD) önyükleme birimi ve depolama, veri birimlerini korumak için şifreleme anahtarlarını ve gizli bilgilerinizi yanı sıra şifreleme öneririz.

[Azure Disk şifrelemesi](azure-security-disk-encryption-overview.md) , Windows ve Linux Iaas sanal makine disklerinizi şifreleyin yardımcı olur. Azure Disk şifrelemesi, endüstri standardı kullanır [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) Windows özelliğidir ve [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) Linux işletim sistemi ve veri diskleri için birim şifrelemesi sağlamak için bir özelliğidir. İle tümleşik bir çözüm [Azure anahtar kasası](https://azure.microsoft.com/documentation/services/key-vault/) denetlemenize ve disk şifreleme anahtarlarını ve gizli anahtar kasası aboneliğinizi yönetmenize yardımcı olmak için. Çözüm ayrıca sanal makine disklerindeki tüm veriler Azure depolama alanındaki bekleyen şifrelenmesini sağlar.

Azure Disk şifrelemesi kullanılarak için en iyi uygulamalar aşağıda verilmiştir:

**En iyi yöntem**: VM üzerinde şifrelemeyi etkinleştirir.   
**Ayrıntı**: Azure Disk şifrelemesi oluşturur ve şifreleme anahtarları için anahtar kasanıza yazar. Anahtar kasanızı şifreleme anahtarları yönetme, Azure AD kimlik doğrulaması gerektirir. Bu amaç için bir Azure AD uygulaması oluşturun. Kimlik doğrulama amacıyla ya da istemci gizli anahtarı tabanlı kimlik doğrulaması kullanabilirsiniz veya [istemci Azure AD'ye sertifika tabanlı kimlik doğrulaması](../active-directory/active-directory-certificate-based-authentication-get-started.md).

**En iyi yöntem**: Bir ek şifreleme anahtarları için güvenlik katmanı için bir anahtar şifreleme anahtarı (KEK) kullanın. Bir KEK anahtar kasanızı ekleyin.   
**Ayrıntı**: Kullanım [Ekle AzKeyVaultKey](https://docs.microsoft.com/powershell/module/az.keyvault/add-azkeyvaultkey) anahtar şifreleme anahtarı anahtar kasasını oluşturmak için cmdlet'i. Ayrıca, anahtar yönetimi, şirket içi donanım güvenlik modülü (HSM) gelen bir KEK içeri aktarabilirsiniz. Daha fazla bilgi için [Key Vault belgeleri](../key-vault/key-vault-hsm-protected-keys.md). Anahtar şifreleme anahtarı belirtildiğinde, Azure Disk şifrelemesi anahtar Kasası'na yazmadan önce şifreleme parolaları sarmalamak için bu anahtarı kullanır. Bu anahtarın bir emanet kopyasını tutarak bir şirket içi anahtar yönetimi HSM anahtarları yanlışlıkla silinmesine karşı ek koruma sunar.

**En iyi yöntem**: Ele bir [anlık görüntü](../virtual-machines/windows/snapshot-copy-managed-disk.md) ve/veya disk şifrelenir önce yedekleme. Şifreleme sırasında beklenmeyen bir hata meydana gelirse, yedekleme kurtarma seçeneğini sağlar.   
**Ayrıntı**: Şifreleme gerçekleşmeden önce yönetilen disklere sahip VM'ler yedeklemesini gerektirir. Bir yedekleme yapıldıktan sonra kullanabileceğiniz **kümesi AzVMDiskEncryptionExtension** belirterek yönetilen diskleri şifreleme cmdlet'ini *- skipVmBackup* parametresi. Yedekleme ve şifrelenmiş Vm'leri geri yükleme hakkında daha fazla bilgi için bkz. [Azure Backup](../backup/backup-azure-vms-encryption.md) makalesi.

**En iyi yöntem**: Şifreleme parolaları bölgesel sınırlar arası yoksa emin olmak için Azure Disk şifrelemesi anahtar kasası ve VM'lerin aynı bölgede olması gerekir.   
**Ayrıntı**: Oluşturun ve şifrelenmiş VM ile aynı bölgede olan bir anahtar Kasası'nı kullanın.

Azure Disk şifrelemesi uyguladığınızda, aşağıdaki iş gereksinimlerini karşılamak:

- Iaas Vm'leri, Kurumsal güvenlik ve uyumluluk gereksinimlerini karşılamak için endüstri standardı şifreleme teknolojisini kullanılmadıkları güvenlidir.
- Müşteri tarafından denetlenen anahtarları ve ilkeleri kapsamında Iaas Vm'leri başlatın ve bunların kullanımını anahtar kasanıza denetleyebilirsiniz.

## <a name="restrict-direct-internet-connectivity"></a>Doğrudan internet bağlantısı kısıtlama
İzleme ve VM doğrudan internet bağlantısı kısıtlayın. Saldırganlar, sürekli tarama açık yönetim bağlantı noktalarını için genel bulut IP aralıkları ve sık kullanılan parolaları ve yüklenmemiş bilinen güvenlik açıkları gibi "kolay" saldırılar denemeden. Aşağıdaki tabloda, bu saldırılara karşı korumaya yardımcı olmak için en iyi uygulamaları listelenmiştir:

**En iyi yöntem**: Ağ yönlendirmesi ve güvenlik için yanlışlıkla açığa engeller.   
**Ayrıntı**: RBAC, yalnızca merkezi ağ grubu ağ kaynakları izni olduğundan emin olmak için kullanın.

**En iyi yöntem**: Belirleyin ve "tüm" kaynak IP adresinden erişime izin ver sunulan sanal makineleri düzeltme.   
**Ayrıntı**: Azure Güvenlik Merkezi'ni kullanın. Güvenlik Merkezi, herhangi bir ağ güvenlik gruplarınız varsa "tümü" kaynak IP adresinden erişime izin veren bir veya daha fazla gelen kuralları internet'e yönelik uç noktalarla erişimi sınırlama önerir. Güvenlik Merkezi için aşağıdaki gelen kurallarını düzenlemenizi öneririz [erişimi kısıtlama](../security-center/security-center-restrict-access-through-internet-facing-endpoints.md) gerçekten erişmesi gereken kaynak IP adresleri için.

**En iyi yöntem**: Yönetim bağlantı noktalarına (RDP, SSH) kısıtlayın.   
**Ayrıntı**: [Just-in-time (JIT) VM erişimi](../security-center/security-center-just-in-time.md) Vm'lere gerektiğinde bağlanılabilmesi için kolay erişim sağlamanın yanı sıra saldırılara maruz kalma riskinizi azaltır, Azure Vm'lere gelen trafiği kilitlemek için kullanılabilir. JIT etkinleştirildiğinde Güvenlik Merkezi Azure Vm'lere gelen trafiği bir ağ güvenlik grubu kuralı oluşturarak kilitler. Sizin için gelen trafiği kilitlenir VM bağlantı noktalarını seçin. Bu bağlantı noktaları, JIT çözüm tarafından denetlenir.

## <a name="next-steps"></a>Sonraki adımlar
Bkz: [Azure güvenlik en iyi uygulamaları ve desenleri](security-best-practices-and-patterns.md) kullanmak üzere daha fazla güvenlik için en iyi yöntemler, tasarlama, dağıtma ve Azure'ı kullanarak bulut çözümlerinizi yönetme.

Aşağıdaki kaynaklar, Azure güvenliği ve ilgili Microsoft Hizmetleri hakkında daha fazla genel bilgi sağlamak kullanılabilir:
* [Azure güvenlik ekibi blogu](https://blogs.msdn.microsoft.com/azuresecurity/) - Azure güvenliği en güncel bilgi için
* [Microsoft Security Response Center](https://technet.microsoft.com/library/dn440717.aspx) - Azure ile ilgili sorunlar dahil Microsoft güvenlik açıklarının bildirilebileceği veya e-posta aracılığıyla secure@microsoft.com
