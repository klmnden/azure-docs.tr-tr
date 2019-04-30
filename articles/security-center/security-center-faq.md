---
title: Azure Güvenlik Merkezi, sık sorulan sorular (SSS) | Microsoft Docs
description: Bu SSS, Azure Güvenlik Merkezi hakkında sorular yanıtlanmaktadır.
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: ''
ms.assetid: be2ab6d5-72a8-411f-878e-98dac21bc5cb
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/19/2019
ms.author: monhaber
ms.openlocfilehash: 79faab0dcf2dd4c5592fe0543fa63f2538facf36
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60909629"
---
# <a name="azure-security-center-frequently-asked-questions-faq"></a>Azure Güvenlik Merkezi - Sık sorulan sorular (SSS)
Bu SSS, Azure Güvenlik Merkezi, artırılmış görünürlük ve Microsoft Azure kaynaklarınızın güvenliğini denetim ile tehditleri önleyin, algılayın ve yardımcı olan bir hizmet hakkında sorular yanıtlanmaktadır.

> [!NOTE]
> Haziran 2017'nin ilk günlerinden itibaren Güvenlik Merkezi, veri toplamak ve depolamak için Microsoft Monitoring Agent'ı kullanacak. Daha fazla bilgi için bkz. [Azure Güvenlik Merkezi Platform geçişi](security-center-platform-migration.md). Bu makaledeki bilgiler, Microsoft Monitoring Agent'a geçiş sonrasındaki Güvenlik Merkezi işlevselliğine yöneliktir.
>
>

## <a name="general-questions"></a>Genel sorular
### <a name="what-is-azure-security-center"></a>Azure Güvenlik Merkezi nedir?
Azure Güvenlik Merkezi, artırılmış görünürlük aracılığıyla tehditleri engellemenize, algılamanıza ve yanıtlamanıza, ayrıca Azure kaynaklarınızın güvenliğini denetlemenize yardımcı olur. Aboneliklerinizde tümleşik güvenlik izleme ve ilke yönetimi sağlar, normal koşullarda gözden kaçabilecek tehditleri algılamaya yardımcı olur ve güvenlik çözümlerinin geniş ekosistemiyle çalışır.

### <a name="how-do-i-get-azure-security-center"></a>Azure Güvenlik Merkezi nasıl alabilirim?
Azure Güvenlik Merkezi ile Microsoft Azure aboneliğinizi etkin ve erişilen [Azure portalında](https://azure.microsoft.com/features/azure-portal/). ([Portalında oturum açın](https://portal.azure.com)seçin **Gözat**, kaydırın **Güvenlik Merkezi**).  

## <a name="billing"></a>Faturalandırma
### <a name="how-does-billing-work-for-azure-security-center"></a>Nasıl Azure Güvenlik Merkezi faturalandırılır?
Güvenlik Merkezi iki katmanda sunulur:

**Ücretsiz katmanı** iş ortaklarının güvenlik ürün ve hizmetleriyle Azure kaynakları, temel güvenlik ilkesi, güvenlik önerileri ve tümleştirme güvenlik durumunu görünürlük sağlar.

**Standart katman** algılama özellikleri dahil olmak üzere, tehdit zekası, davranışsal analiz, anomali algılama, güvenlik olayları ve tehdit attribution raporları Gelişmiş tehdit ekler. Bir standart katman ücretsiz deneme süresi başlatabilirsiniz. Yükseltmek için seçin [fiyatlandırma katmanı](https://docs.microsoft.com/azure/security-center/security-center-pricing) güvenlik ilkesinde. Daha fazla bilgi için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/security-center/).

### <a name="how-can-i-track-who-in-my-organization-performed-pricing-tier-changes-in-azure-security-center"></a>Kuruluşumda kimlerin fiyatlandırma katmanı değişiklikleri, Azure Güvenlik Merkezi'nde yapılan nasıl izleyebilir miyim
Bir Azure aboneliği fiyatlandırma katmanını değiştirmek için gerekli izinlere sahip birden çok Yöneticisi olabilir gibi bir kullanıcı fiyatlandırma katmanı değişikliği gerçekleştiren bilmek isteyebilir. Azure etkinlik günlüğü kullanabilirsiniz, kullanılacak. Lütfen diğer yönergeleri görmek [burada](https://techcommunity.microsoft.com/t5/Security-Identity/Tracking-Changes-in-the-Pricing-Tier-for-Azure-Security-Center/td-p/390832)

## <a name="permissions"></a>İzinler
Azure Güvenlik Merkezi, Azure'daki kullanıcılara, gruplara ve hizmetlere atanabilen [yerleşik roller](../role-based-access-control/built-in-roles.md) sağlayan [Rol Tabanlı Erişim Denetimi'ni (RBAC)](../role-based-access-control/role-assignments-portal.md) kullanır.

Güvenlik Merkezi, güvenlik sorunlarını ve güvenlik açıklarını tanımlamak amacıyla kaynaklarınızın yapılandırmasını değerlendirir. Güvenlik Merkezi'nde, yalnızca bir kaynağa sahip, katkıda bulunan veya okuyucu rolü bir kaynağın ait olduğu kaynak grubu veya abonelik atandığında ilgili bilgiler görürsünüz.

Bkz: [Azure Güvenlik Merkezi'nde izinler](security-center-permissions.md) rolleri ve izin verilen eylemleri Güvenlik Merkezi hakkında daha fazla bilgi edinmek için.

## <a name="data-collection-agents-and-workspaces"></a>Veri toplama aracıları ve çalışma alanları
Güvenlik Merkezi, Azure sanal makineleri (VM'ler), sanal makine ölçek kümeleri (VMSS), Iaas kapsayıcılarınızdaki ve Azure olmayan (dahil, şirket içi) bilgisayarlar güvenlik açıklarını ve tehditleri izlemek için veri toplar. Veriler, makineden güvenlikle ilgili çeşitli yapılandırmaları ve olay günlüklerini okuyup verileri analiz için çalışma alanınıza kopyalayan Microsoft Monitoring Agent kullanılarak toplanır.

### <a name="am-i-billed-for-azure-monitor-logs-on-the-workspaces-created-by-security-center"></a>Azure İzleyici açtığında, Güvenlik Merkezi tarafından oluşturulan çalışma alanları için Faturalandırılacak mıyım?
Hayır. Güvenlik Merkezi tarafından oluşturulan çalışma alanları başına düğüm faturalandırma, Azure İzleyici günlük için yapılandırılmış olsa Azure İzleyici günlüklerine ücretleri uygulanmaz. Güvenlik Merkezi her zaman, Güvenlik Merkezi güvenlik ilkesi ve bir çalışma alanına yüklenmiş çözümlere göre faturalandırılır:

- **Ücretsiz katmanı** – Güvenlik Merkezi için varsayılan çalışma alanı 'SecurityCenterFree' çözümü sağlar. Ücretsiz katmanı için faturalandırılmaz.
- **Standart katman** – Güvenlik Merkezi için varsayılan çalışma alanı 'Güvenlik' çözümü sağlar.

Fiyatlandırma hakkında daha fazla bilgi için bkz. [Güvenlik Merkezi fiyatlandırma](https://azure.microsoft.com/pricing/details/security-center/). Fiyatlandırma sayfası, güvenlik verileri depolama ve Haziran 2017'den itibaren eşit olarak bölünmüş faturalandırma değişiklikleri yöneliktir.

> [!NOTE]
> Fiyatlandırma katmanı Güvenlik Merkezi tarafından oluşturulan çalışma alanları, log analytics, Güvenlik Merkezi faturalandırma etkilemez.
>
>

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

### <a name="what-qualifies-a-vm-for-automatic-provisioning-of-the-microsoft-monitoring-agent-installation"></a>Hangi, bir VM'nin Microsoft Monitoring Agent yüklemesi otomatik sağlama için niteliği taşır?
Windows veya Linux Iaas sanal makineleri, uygun:

- Microsoft Monitoring Agent uzantısı VM'de yüklü değil.
- VM'nin çalışır durumda olduğundan.
- Windows veya Linux [Azure sanal makine Aracısı](https://docs.microsoft.com/en-us/azure/virtual-machines/extensions/agent-windows) yüklenir.
- VM, web uygulaması güvenlik duvarı veya yeni nesil güvenlik duvarı gibi bir gereç olarak kullanılmaz.

### <a name="can-i-delete-the-default-workspaces-created-by-security-center"></a>Ben, Güvenlik Merkezi tarafından oluşturulan varsayılan çalışma alanlarını silebilir miyim?
**Varsayılan çalışma alanı siliniyor önerilmez.** Güvenlik Merkezi Vm'lerinizi güvenlik verileri depolamak için varsayılan çalışma alanı kullanır.  Bir çalışma alanını silerseniz, Güvenlik Merkezi bu verileri ve bazı güvenlik önerilerini toplanacak belirleyemez ve uyarılar kullanılamıyor.

Kurtarmak için Microsoft Monitoring Agent'ı silinmiş çalışma alanına bağlı sanal makineleri kaldırın. Güvenlik Merkezi, aracı yeniden yükler ve yeni varsayılan çalışma alanı oluşturur.

### <a name="how-can-i-use-my-existing-log-analytics-workspace"></a>Mevcut bir Log Analytics çalışma Alanım nasıl kullanabilirim?

Güvenlik Merkezi tarafından toplanan verileri depolamak için mevcut bir Log Analytics çalışma alanını seçebilirsiniz. Mevcut bir Log Analytics çalışma alanınızı kullanmak için:

- Çalışma alanı, seçili Azure aboneliğiniz ile ilişkilendirilmesi gerekir.
- En azından, çalışma alanına erişmek için Okuma izinlerine sahip olmalıdır.

Mevcut bir Log Analytics çalışma alanı seçmek için:

1. Altında **güvenlik ilkesi – veri toplama**seçin **başka bir çalışma alanını kullanabilmesini**.

   ![Başka bir çalışma alanı kullan][5]

2. Aşağı açılır menüden, toplanan verileri depolamak için bir çalışma alanı seçin.

   > [!NOTE]
   > Açılır menü, erişimi ve Azure aboneliğinizde olan çalışma alanları gösterilir.
   >
   >

3. **Kaydet**’i seçin.
4. Seçtikten sonra **Kaydet**, izlenen Vm'leri yeniden yapılandırmak istiyorsanız istenir.

   - Seçin **Hayır** yeni çalışma alanı ayarları için isterseniz **yalnızca yeni Vm'lere uygulamak**. Yeni çalışma alanı ayarları, yalnızca yeni aracı yüklemelerini için geçerlidir; Microsoft Monitoring Agent yüklüyse olmayan yeni bulunmuş VM'ler.
   - Seçin **Evet** yeni çalışma alanı ayarları için isterseniz **tüm sanal makinelere uygulanmasını**. Ayrıca, çalışma alanı oluşturulduğunda bir güvenlik Merkezi'ne bağlı her bir VM yeni hedef çalışma alanına bağlanır.

   > [!NOTE]
   > Evet'i seçerseniz, tüm sanal makineler yeni hedef çalışma alanına bağlandı kadar Güvenlik Merkezi tarafından oluşturulan çalışma alanlarını silmemelisiniz. Bir çalışma alanı çok erken silinirse bu işlem başarısız olur.
   >
   >

   - Seçin **iptal** işlemi iptal etme.

### Ne Microsoft Monitoring Agent, sanal makine uzantısı olarak zaten yüklendi?<a name="mmaextensioninstalled"></a>
İzleme Aracısı, uzantı olarak yüklendikten sonra uzantı yapılandırması yalnızca tek bir çalışma alanına raporlama sağlar. Güvenlik Merkezi, kullanıcı çalışma alanları için varolan bağlantılar kılmaz. Güvenlik Merkezi koşuluyla "Güvenlik" veya "SecurityCenterFree" çözüm üzerinde yüklenmiş olan bir VM Güvenlik verileri zaten bağlıysa, bir çalışma alanında depolar. Güvenlik Merkezi, uzantı sürümü, bu işlem en son sürüme yükseltebilirsiniz.

Daha fazla bilgi için [önceden var olan bir aracı yüklemesi durumlarda otomatik sağlama](security-center-enable-data-collection.md#preexisting).


### Ben Microsoft Monitoring Agent ne vardı doğrudan makinede ancak uzantı (doğrudan aracı) olarak değil yüklü mü?<a name="directagentinstalled"></a>
Microsoft Monitoring Agent (olarak değil bir Azure uzantısı) doğrudan VM'de yüklü değilse, Güvenlik Merkezi Microsoft Monitoring Agent uzantısını yükleyecek ve Microsoft Monitoring agent, en son sürüme yükseltebilirsiniz.
Yüklü aracı için zaten yapılandırılmış kendi çalışma alanlarında bildirmeye devam eder ve ayrıca Güvenlik Merkezi'nde yapılandırılmış çalışma alanına rapor eder (birden çok giriş desteklenir).
Bir kullanıcı çalışma (değil Güvenlik Merkezi'nin varsayılan çalışma alanına) yapılandırılmış çalışma alanı ise yüklemeniz gerekir "güvenlik / dayanarak Güvenlik Merkezi, Vm'leri ve Bilgisayarları'ndan olayları işlemeyi başlatmak"SecurityCenterFree"Çözüm için raporlama Çalışma alanı.

Güvenlik Merkezi'ne abonelikleri eklenmedi 2019-03-mevcut bir aracının ne zaman algılanır, 17 önce mevcut makinelerde Microsoft Monitoring Agent uzantısını yüklenmez ve makine etkilenmez. Bu makineler üzerinde aracı yükleme sorunlarını gidermek için "Çözümle makinelerinizde aracı sistem durumu sorunlarını izleme" öneri bu makineler için bkz.

 Daha fazla bilgi için sonraki bölüme bakın [SCOM veya OMS Aracısı VM üzerinde zaten yüklü doğrudan ne olur?](#scomomsinstalled)

### Bir System Center Operations Manager (SCOM) aracısı VM'deki uygulamalarımdan birine zaten yüklü değilse ne olur?<a name="scomomsinstalled"></a>
Güvenlik Merkezi, Microsoft Monitoring Agent uzantısı yan yana için var olan System Center Operations Manager aracısını yükler. Var olan SCOM Aracısı System Center Operations Manager sunucusuna normalde bildirmeye devam eder. System Center Operations Manager aracısı ve Microsoft Monitoring Agent bu verisine sırasında en son sürüme güncelleştirilir ortak çalışma zamanı kitaplıkları paylaşmak unutmayın. System Center Operations Manager 2012 aracı sürümü yüklü değil açarsanız üzerinde sağlama otomatik - unutmayın (yönetilebilirlik özellikleri olabilir kayıp System Center Operations Manager server 2012 sürümü olduğunda).

### <a name="what-is-the-impact-of-removing-these-extensions"></a>Bu Uzantılar'ı kaldırmanın etkisi nedir?
Microsoft Monitoring uzantısı kaldırırsanız, Güvenlik Merkezi sanal makine ve bazı güvenlik önerilerini güvenlik verilerini toplamak mümkün değildir ve uyarılar kullanılamıyor. 24 saat içinde Güvenlik Merkezi, VM uzantısı eksik ve uzantıyı yükler belirler.

### <a name="how-do-i-stop-the-automatic-agent-installation-and-workspace-creation"></a>Otomatik aracı yükleme ve çalışma alanı oluşturma nasıl durdururum?
Otomatik aboneliklerinizde güvenlik ilkesinde sağlamayı kapatın kapatabilirsiniz, ancak bu önerilmemektedir. Otomatik sağlama sınırları Güvenlik Merkezi önerilerini ve Uyarıları kapatma. Otomatik sağlamayı devre dışı bırakmak için:

1. Aboneliğiniz için standart katmana yapılandırıldıysa, bu abonelik için Güvenlik İlkesi'ni açın ve seçin **ücretsiz** katmanı.

   ![Fiyatlandırma katmanı][1]

2. Ardından, kapatma seçerek otomatik sağlamayı devre dışı **kapalı** üzerinde **güvenlik ilkesi – veri toplama** dikey penceresi.
   ![Veri toplama][2]

### <a name="should-i-opt-out-of-the-automatic-agent-installation-and-workspace-creation"></a>Çalışma alanı oluşturma ve otomatik aracı yüklemesi dışında iyileştirilmiş?

> [!NOTE]
> Bölümleri gözden geçirdiğinizden emin olun [iletişimlerini etkileri nelerdir?](#what-are-the-implications-of-opting-out-of-automatic-provisioning) ve [iletişimlerini, önerilen adımlar](#what-are-the-recommended-steps-when-opting-out-of-automatic-provisioning) iyileştirilmiş dışı otomatik sağlamayı seçerseniz.
>
>

Aşağıdakiler sizin için geçerliyse, otomatik sağlama dışında bırakmak isteyebilirsiniz:

- Güvenlik Merkezi tarafından otomatik aracı yüklemesi, tüm abonelik için geçerlidir.  Otomatik Yükleme VM'lerin bir alt kümesine uygulanamıyor. Microsoft İzleme Aracısı ile yüklenmiş kritik VM'lerin varsa, otomatik sağlama dışında iyileştirilmiş.
- Microsoft Monitoring Agent (MMA) uzantısını yükleme kullanarak aracının sürümünü güncelleştirir. Bu doğrudan bir aracı ve bir SCOM aracısı için geçerlidir (ikincisi SCOM ve MMA işleminde güncelleştirilecek ortak çalışma zamanı kitaplıkları - paylaşma). Yüklü SCOM Aracısı 2012 sürümüdür ve yükseltilir, SCOM server 2012 sürümü olduğunda yönetilebilirlik özellikleri kaybolabilir. Yüklü SCOM Aracısı sürümü 2012 ise, otomatik sağlama dışında edilmesiyle dikkate almanız gerekir.
- Özel çalışma alanı aboneliği (merkezi bir çalışma alanı) dış varsa dışı otomatik sağlamayı tercih. El ile Microsoft Monitoring Agent uzantısını yükleyin ve bağlantıyı geçersiz kılma olmadan Güvenlik Merkezi çalışma alanınızı bağlayın.
- Abonelik başına birden çok çalışma alanı oluşturulmasını önlemek istediğiniz ve kendi özel bir çalışma alanı aboneliği içinde varsa, iki seçeneğiniz vardır:

   1. Otomatik sağlama dışında tercih edebilirsiniz. Geçişten sonra varsayılan çalışma alanı ayarları bölümünde anlatıldığı gibi ayarlama [nasıl kullanabilir miyim mevcut Log Analytics çalışma Alanım?](#how-can-i-use-my-existing-log-analytics-workspace)
   2. Veya, geçiş, sanal makinelere yüklenecek Microsoft Monitoring Agent'ı tamamlamak izin verebilir ve Vm'leri oluşturulan çalışma alanına bağlı. Ardından, önceden yüklenmiş aracıları harcamamak için seçim ile varsayılan çalışma alanı ayarını ayarlayarak kendi özel çalışma alanı seçin. Daha fazla bilgi için [nasıl kullanabilir miyim mevcut Log Analytics çalışma Alanım?](#how-can-i-use-my-existing-log-analytics-workspace)

### <a name="what-are-the-implications-of-opting-out-of-automatic-provisioning"></a>Otomatik sağlama dışında edilmesiyle etkileri nelerdir?
Geçiş tamamlandıktan sonra Güvenlik Merkezi sanal makine ve bazı güvenlik önerilerini güvenlik verilerini toplamak mümkün değildir ve uyarılar kullanılamıyor. Microsoft Monitoring Agent çıkarsanız, el ile yüklemeniz gerekir. Bkz: [iletişimlerini, önerilen adımlar](#what-are-the-recommended-steps-when-opting-out-of-automatic-provisioning).

### <a name="what-are-the-recommended-steps-when-opting-out-of-automatic-provisioning"></a>Otomatik sağlama dışında kabul edilirse, önerilen adımlar nelerdir?

Güvenlik Merkezi, Vm'lerinizden güvenlik verilerini toplamak ve öneriler ve uyarılar sağlamak için Microsoft Monitoring Agent uzantısını el ile yüklemeniz gerekir. Bkz: [aracı yüklemesi Windows VM için](../virtual-machines/extensions/oms-windows.md) veya [Linux sanal makinesi için aracı yüklemesi](../virtual-machines/extensions/oms-linux.md) yükleme hakkında yönergeler için.

Aracı mevcut özel çalışma alanına bağlanabilir veya Güvenlik Merkezi çalışma alanı oluşturulur. Özel bir çalışma alanı etkin 'Güvenlik' veya 'SecurityCenterFree' çözümü yoksa, bir çözüm uygulamak gerekir. Uygulamak için özel çalışma alanı ya da abonelik seçin ve bir fiyatlandırma katmanı aracılığıyla uygulama **güvenlik ilkesi – fiyatlandırma katmanı** dikey penceresi.

   ![Fiyatlandırma katmanı][1]

Güvenlik Merkezi seçili fiyatlandırma katmanını temel alan çalışma alanında doğru çözümün olanak sağlar.

### Güvenlik Merkezi tarafından yüklü OMS uzantıları nasıl kaldırabilirim?<a name="remove-oms"></a>
Microsoft Monitoring Agent el ile kaldırabilirsiniz. Güvenlik Merkezi önerilerini ve Uyarıları sınırlar bu önerilmez.

> [!NOTE]
> Veri toplama etkinleştirilirse Güvenlik Merkezi kaldırdıktan sonra aracıyı yeniden yükler.  El ile aracı kaldırmadan önce veri toplamayı devre dışı bırakmak gerekir. Ben otomatik aracı yükleme ve çalışma alanı oluşturma nasıl vermeyi görüyor musunuz? Veri toplamayı devre dışı bırakma ile ilgili yönergeler için.
>
>

Aracıyı el ile kaldırmak için:

1.  Portalda açın **Log Analytics**.
2.  Log Analytics dikey penceresinde, bir çalışma alanı seçin:
3.  İzleme ve seçmek için istemediğiniz her bir VM seçin **Bağlantıyı Kes**.

   ![Aracıyı kaldır][3]

> [!NOTE]
> Bir Linux VM uzantısı olmayan bir OMS Aracısı zaten varsa, uzantıyı kaldırma da aracıyı kaldırır ve müşterinin yeniden yüklemeniz gerekir.
>
>
### <a name="how-do-i-disable-data-collection"></a>Veri toplama nasıl devre dışı bırakabilirim?
Otomatik sağlama varsayılan olarak kapalıdır. Otomatik kaynaklardan herhangi bir zamanda bu güvenlik ilkesi ayarı devre dışı bırakarak sağlama devre dışı bırakabilirsiniz. Otomatik sağlama güvenlik uyarıları ve sistem güncelleştirmeleri, işletim sistemi güvenlik açıkları ve uç nokta koruma hakkında öneriler almak için kesinlikle önerilir.

Veri toplamayı devre dışı bırakmak için [Azure portalında oturum açın](https://portal.azure.com)seçin **Gözat**seçin **Güvenlik Merkezi**seçip **seçin, ilke**. Otomatik sağlamayı hangi abonelik için devre dışı bırakmak istediğinizi belirtin. Bir aboneliği seçtiğinizde **güvenlik ilkesi - veri toplama** açılır. Altında **otomatik sağlama**seçin **kapalı**.

### <a name="how-do-i-enable-data-collection"></a>Veri toplama hizmetini nasıl etkinleştirebilirim?
Azure aboneliğinizi güvenlik ilkesinde veri toplamayı etkinleştirebilirsiniz. Veri toplamayı etkinleştirmek için. [Azure portalında oturum açın](https://portal.azure.com)seçin **Gözat**seçin **Güvenlik Merkezi**seçip **Güvenlik İlkesi**. Otomatik sağlamayı etkinleştirmek istediğiniz aboneliği seçin. Bir aboneliği seçtiğinizde **güvenlik ilkesi - veri toplama** açılır. Altında **otomatik sağlama**seçin **üzerinde**.

### <a name="what-happens-when-data-collection-is-enabled"></a>Veri toplama etkinleştirilirse ne olur?
Otomatik sağlama etkinleştirildiğinde Güvenlik Merkezi Microsoft Monitoring Agent'ı tüm Azure Vm'lere ve oluşturulan tüm yeni vm'lere desteklenen hazırlar. Otomatik sağlama önemle tavsiye edilir ancak el ile aracı yüklemelerini da kullanılabilir. [Microsoft Monitoring Agent uzantısını nasıl yükleyeceğiniz öğrenin](../azure-monitor/learn/quick-collect-azurevm.md#enable-the-log-analytics-vm-extension). 

İşlem oluşturma olayı 4688 Aracısı etkinleştirir ve *CommandLine* olay 4688 içindeki alan. VM üzerinde oluşturulan yeni süreçler olay günlüğü tarafından kaydedilir ve Güvenlik Merkezi'nin algılama hizmetleri tarafından izlenen. Her yeni bir işlem için kayıtlı ayrıntıları hakkında bilgi için bkz. [4688 açıklama alanlarına](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4688#fields). Aracı ayrıca VM üzerinde oluşturulan 4688 olayları toplar ve bunları Search'te depolar.

Aracı ayrıca için veri toplamayı etkinleştirir [Uyarlamalı uygulama denetimleri](security-center-adaptive-application.md), Güvenlik Merkezi, tüm uygulamalara izin vermek üzere Denetim modunda yerel bir AppLocker ilkesini yapılandırır. Bu, daha sonra toplanan ve Güvenlik Merkezi tarafından kullanılabilir olaylar oluşturmak AppLocker neden olur. Bu ilke, üzerinde zaten var. yapılandırılmış bir AppLocker İlkesi tüm makinelerde yapılandırılmaz dikkat edin önemlidir. 

Güvenlik Merkezi, VM'de kuşkulu bir etkinlik algıladığında, müşteri tarafından e-posta, bildirilir [güvenlik bilgilerini](security-center-provide-security-contact-details.md) sağlanmadı. Bir uyarı da Güvenlik Merkezi'nin güvenlik uyarıları panosunda görünür.



### <a name="does-the-monitoring-agent-impact-the-performance-of-my-servers"></a>İzleme Aracısı Sunucularım performansı etkiler mi?
Aracı, sistem kaynaklarının nominal bir miktarını kullanıyor ve performans üzerinde çok az bir etkiye sahip değildir. Performans etkisi ve aracı ve uzantısı hakkında daha fazla bilgi için bkz. [planlama ve işlemler Kılavuzu](security-center-planning-and-operations-guide.md#data-collection-and-storage).

### <a name="where-is-my-data-stored"></a>Verilerim nerede depolanır?
Bu Aracıdan toplanan veriler, aboneliğinizle ilişkili mevcut bir Log Analytics çalışma alanı veya yeni bir çalışma alanı içinde depolanır. Daha fazla bilgi için [veri güvenliği](security-center-data-security.md).

## Azure İzleyici mevcut müşteriler günlüğe kaydeder<a name="existingloganalyticscust"></a>

### <a name="does-security-center-override-any-existing-connections-between-vms-and-workspaces"></a>Güvenlik Merkezi, Vm'leri ve çalışma alanları arasında herhangi bir mevcut bağlantı geçersiz kılmaz?
Güvenlik Merkezi bir VM zaten Microsoft Monitoring Agent yüklüyse Azure uzantı olarak varsa, varolan bir çalışma alanı bağlantıyı geçersiz kılmaz. Bunun yerine, Güvenlik Merkezi, varolan bir çalışma alanını kullanır. "Güvenlik" veya "SecurityCenterFree" Çözüm için çalışma alanı için raporlama yüklü olması koşuluyla, sanal makine korunur. 

Güvenlik Merkezi çözüm veri toplama ekranda seçilen çalışma alanına yüklenmiş zaten mevcut değilse ve çözüm, yalnızca ilgili Vm'lere uygulanır. Bir çözümü eklediğinizde, Log Analytics çalışma alanınıza bağlı tüm Windows ve Linux aracıları için varsayılan olarak otomatik olarak dağıtılır. [Çözüm hedefleme](../operations-management-suite/operations-management-suite-solution-targeting.md) çözümlerinize bir kapsam uygulamanıza imkan sağlar.

Microsoft Monitoring Agent (olarak değil bir Azure uzantısı) doğrudan VM'de yüklü değilse, Güvenlik Merkezi Microsoft Monitoring Agent yüklemez ve güvenlik izleme sınırlıdır.

### <a name="does-security-center-install-solutions-on-my-existing-log-analytics-workspaces-what-are-the-billing-implications"></a>Güvenlik Merkezi, my mevcut Log Analytics çalışma alanları çözümleri yüklüyor mu? Fatura etkileri nelerdir?
Güvenlik Merkezi, Güvenlik Merkezi bir VM zaten oluşturduğunuz bir çalışma alanına bağlı belirlediğinde bu çalışma alanındaki fiyatlandırma katmanınızı göre çözümler sağlar. Çözümleri aracılığıyla yalnızca ilgili Azure Vm'lerine, uygulanan [çözüm hedefleme](../operations-management-suite/operations-management-suite-solution-targeting.md), faturalandırma aynı kalır.

- **Ücretsiz katmanı** – Güvenlik Merkezi, çalışma alanı 'SecurityCenterFree' çözümü yükler. Ücretsiz katmanı için faturalandırılmaz.
- **Standart katman** – Güvenlik Merkezi, çalışma alanı 'Güvenlik' çözümü yükler.

   ![Varsayılan çalışma alanı çözümleri][4]

### <a name="i-already-have-workspaces-in-my-environment-can-i-use-them-to-collect-security-data"></a>Çalışma Alanım ortamda zaten var, bunların güvenlik verilerini toplamak için kullanabilir miyim?
Güvenlik Merkezi, bir VM'nin Microsoft Monitoring Agent yüklüyse Azure uzantı olarak zaten varsa, mevcut bağlı çalışma kullanır. Güvenlik Merkezi çözüm çalışma alanı yüklü zaten mevcut değilse ve çözümü yalnızca ilgili Vm'lere uygulanır [çözüm hedefleme](../operations-management-suite/operations-management-suite-solution-targeting.md).

Güvenlik Merkezi, Microsoft Monitoring Agent, Vm'lerde yüklendiğinde, Güvenlik Merkezi tarafından oluşturulan varsayılan çalışma alanlarını kullanır.

### <a name="i-already-have-security-solution-on-my-workspaces-what-are-the-billing-implications"></a>Zaten bir güvenlik çözümü üzerinde çalışma alanlarım var. Fatura etkileri nelerdir?
Güvenlik ve denetim çözümü, Azure Vm'leri için Güvenlik Merkezi standart katmanı özellikleri etkinleştirmek için kullanılır. Güvenlik Merkezi, güvenlik ve denetim çözümü, bir çalışma alanında zaten yüklüyse, varolan bir çözümü kullanır. Faturalama değişiklik yoktur.

## <a name="using-azure-security-center"></a>Azure Güvenlik Merkezi'ni kullanma
### <a name="what-is-a-security-policy"></a>Güvenlik İlkesi nedir?
Güvenlik İlkesi belirtilen Abonelikteki kaynaklar için önerilen denetim kümesini tanımlar. Azure Güvenlik Merkezi'nde şirketinizin güvenlik gereksinimlerine veya uygulamaların türüne ya da her Abonelikteki verilerin duyarlılığına göre Azure Abonelikleriniz için ilkeler tanımlarsınız.

Azure Güvenlik Merkezi sürücü güvenlik önerilerini ve izlemeyi etkinleştirilen güvenlik ilkeleri. Güvenlik ilkeleri hakkında daha fazla bilgi edinmek için [güvenlik durumunu, Azure Güvenlik Merkezi'nde izleme](security-center-monitoring.md).

### <a name="who-can-modify-a-security-policy"></a>Güvenlik İlkesi değiştirebilecekleri?
Güvenlik ilkesini değiştirmek için bir güvenlik yöneticisi veya sahibi veya katkıda bulunanı o aboneliğin olması gerekir.

Güvenlik İlkesi yapılandırma konusunda bilgi için bkz: [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](tutorial-security-policy.md).

### <a name="what-is-a-security-recommendation"></a>Bir güvenlik önerisi nedir?
Azure Güvenlik Merkezi, Azure kaynaklarınızın güvenlik durumunu analiz eder. Olası güvenlik açıkları tanımlandığında, öneri oluşturulur. Öneriler gerekli denetimi yapılandırma işlemi boyunca size rehberlik. Örnekler şunlardır:

* Kötü amaçlı yazılımdan koruma tanımlamak ve kötü amaçlı yazılımı kaldırmak için sağlama
* [Ağ güvenlik grupları](../virtual-network/security-overview.md) ve sanal makine trafiği kuralları
* Web uygulamalarınızı hedefleyen saldırılara karşı korumaya yardımcı olmak için bir web uygulaması güvenlik duvarı sağlama
* Eksik sistem güncelleştirmelerini dağıtma
* Önerilen taban çizgileriyle eşleşmeyen işletim sistemi yapılandırmalarını ele alma

Güvenlik ilkeleri ile etkinleştirilen önerileri burada gösterilir.

### <a name="how-can-i-see-the-current-security-state-of-my-azure-resources"></a>Azure Kaynaklarımın geçerli güvenlik durumunu nasıl görebilirim?
**Güvenlik merkezine genel bakış** dikey penceresinde, işlem, ağ, depolama ve veri ve uygulamaları tarafından ayrılmış ortamın genel güvenlik duruşunu gösterir. Olası güvenlik açıkları tanımladıysanız her kaynak türünün bir göstergesi gösteren vardır. Her kutucuğa tıklandığında, aboneliğinizdeki kaynaklar envanterini birlikte Güvenlik Merkezi tarafından tanımlanan güvenlik sorunların bir listesini görüntüler.

### <a name="what-triggers-a-security-alert"></a>Hangi güvenlik uyarısı tetikler?
Azure Güvenlik Merkezi otomatik olarak toplar, çözümler ve Azure kaynaklarınızı, ağ ve kötü amaçlı yazılımdan koruma ve güvenlik duvarları gibi iş ortağı çözümlerinden günlük verilerini birleştirir. Tehditler algılandığında bir güvenlik uyarısı oluşturulur. Örneklere şunların algılanması dahildir:

* Bilinen kötü amaçlı IP adresleri ile iletişim kuran tehlikeye girmiş sanal makineler
* Windows Hata Raporlama kullanılarak algılanan Gelişmiş kötü amaçlı yazılım
* Sanal makinelere karşı deneme yanılma saldırıları
* Kötü amaçlı yazılımdan koruma veya Web uygulaması güvenlik duvarları gibi tümleşik iş ortağı güvenlik çözümlerinden güvenlik uyarıları

### Neden puanları değerleri değişimi güvenli? <a name="secure-score-faq"></a>
Şubat 2019'den itibaren Güvenlik Merkezi, önem derecesi daha iyi uyum sağlamak için bazı öneriler, puanı ayarlanır. Bu düzeltme sonucu olabilir değişiklikleri genel güvenli puanı değerleri.  Güvenli puan hakkında daha fazla bilgi için bkz: [puanı hesaplamaya güvenli](security-center-secure-score.md).

### <a name="whats-the-difference-between-threats-detected-and-alerted-on-by-microsoft-security-response-center-versus-azure-security-center"></a>Microsoft Security Response Center ve Azure Güvenlik Merkezi tarafından uyarı ve algılanan tehditlere arasındaki fark nedir?
Microsoft Güvenlik Yanıt Merkezi (MSRC), select güvenlik Azure ağ ve altyapı izleme gerçekleştirir ve tehdit zekasını ve kötüye şikayetlerinin Üçüncü taraflardan alır. MSRC müşteri verilerini bir yasadışı veya yetkisiz bir tarafın eriştiğini veya müşterinin Azure kullanımına şartlarını kabul edilebilir kullanım için uyumlu değil, uyumlu hale geldiğinde, güvenlik olay manager müşteri bildirir. Bildirim, genellikle güvenlik ilgili kişi belirtilmezse, Azure Güvenlik Merkezi veya Azure aboneliği sahibi belirtilen güvenlik kişilere bir e-posta göndererek gerçekleşir.

Güvenlik Merkezi sürekli olarak müşterinin Azure ortamına izler ve analytics çok çeşitli kötü amaçlı etkinlik otomatik olarak algılamak için geçerli bir Azure hizmetidir. Bu algılamaların amacı, Güvenlik Merkezi panosunda güvenlik uyarıları olarak takip edilir.

### <a name="which-azure-resources-are-monitored-by-azure-security-center"></a>Hangi Azure kaynakları, Azure Güvenlik Merkezi tarafından izlenir?
Azure Güvenlik Merkezi, aşağıdaki Azure kaynakları izler:

* Sanal makineleri (VM'ler) (dahil olmak üzere [Cloud Services](../cloud-services/cloud-services-choose-me.md))
* Sanal makine ölçek kümeleri (VMSSs)
* Azure Sanal Ağları
* Azure SQL Hizmeti
* Azure Storage hesabı
* Azure Web Apps (içinde [App Service ortamı](../app-service/environment/intro.md))
* Web uygulaması güvenlik duvarı vm'lerde ve App Service ortamı gibi Azure aboneliğinizle tümleşik iş ortağı çözümleri

Ayrıca, Azure dışı (şirket içi) dahil bilgisayar de Azure Güvenlik Merkezi tarafından izlenebilir (her ikisi de [Windows bilgisayarları](./quick-onboard-windows-computer.md) ve [Linux bilgisayarları](./quick-onboard-linux-computer.md) desteklenir)

## <a name="virtual-machines"></a>Virtual Machines
### <a name="what-types-of-virtual-machines-are-supported"></a>Sanal makinelerin hangi türleri desteklenir?
İzleme ve öneriler her ikisi de kullanılarak oluşturulan sanal makineler için (VM'ler) kullanılabilir [Klasik ve Resource Manager dağıtım modellerinde](../azure-classic-rm.md).

Bkz: [desteklenen platformlar Azure Güvenlik Merkezi'nde](security-center-os-coverage.md) desteklenen platformlar listesi.

### <a name="why-doesnt-azure-security-center-recognize-the-antimalware-solution-running-on-my-azure-vm"></a>Neden Azure Güvenlik Merkezi, Azure VM'deki uygulamalarımdan birine çalışan kötü amaçlı yazılımdan koruma çözümü tanımıyor?
Azure Güvenlik Merkezi, Azure uzantıları yüklü kötü amaçlı yazılımdan koruma görünürlük sahiptir. Örneğin, Güvenlik Merkezi, sağlanan bir görüntüyü veya üzerinde kendi işlemleri (örneğin, yapılandırma yönetim sistemleri) kullanarak sanal makinelerinizde kötü amaçlı yazılımdan koruma yüklediyseniz önceden yüklenmiş kötü amaçlı yazılımdan koruma algılamasını mümkün değil.

### <a name="why-do-i-get-the-message-missing-scan-data-for-my-vm"></a>Neden ileti "Eksik tarama verileri" sanal Makinem için alıyorum?
Bir sanal makine için hiçbir tarama verileri olduğunda bu ileti görünür. Azure Güvenlik Merkezi'nde veri toplama etkinleştirildikten sonra doldurmak tarama verileri için bazı zaman (bir saatten az) alabilir. Tarama verileri ilk popülasyonunu sonra tarama veri yoktur hiç veya hiç son tarama verileri olduğundan, bu iletiyi alabilirsiniz. Durdurulmuş bir sanal makine için tarama doldurmayın. Tarama verileri kısa bir süre önce (uygun olarak 30 günlük varsayılan değer olan Windows aracısına yönelik, bekletme ilkesi) doldurduğu değil, bu iletiyi de görüntülenebilir.

### <a name="how-often-does-security-center-scan-for-operating-system-vulnerabilities-system-updates-and-endpoint-protection-issues"></a>Güvenlik Merkezi, işletim sistemi güvenlik açıkları, sistem güncelleştirmeleri ve endpoint protection sorunları ne sıklıkta tarama?
Güvenlik açıklarına karşı güncelleştirmeler, Güvenlik Merkezi'nde gecikme süresi tarar ve konular:

- İşletim sistemi güvenlik yapılandırmaları – veri 48 saat içinde güncelleştirilir
- Sistem güncelleştirmeleri – veri 24 saat içinde güncelleştirilir
- Uç nokta koruma sorunları – 8 saat içinde verileri güncelleştirildi.

Güvenlik Merkezi genellikle her saat yeni verileri tarar ve buna göre öneriler yeniler. 

### <a name="why-do-i-get-the-message-vm-agent-is-missing"></a>"VM Aracısı eksik?" iletiyi neden alıyorum
Veri toplama özelliğini etkinleştirmeyi Vm'lerinde VM aracısı yüklü olmalıdır. VM Aracısı, Azure Marketi’nden dağıtılan VM’ler için varsayılan olarak yüklüdür. Diğer Vm'lere VM Aracısı yükleme hakkında daha fazla bilgi için blog gönderisine bakın [VM aracısı ve uzantıları](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/).


<!--Image references-->
[1]: ./media/security-center-platform-migration-faq/pricing-tier.png
[2]: ./media/security-center-platform-migration-faq/data-collection.png
[3]: ./media/security-center-platform-migration-faq/remove-the-agent.png
[4]: ./media/security-center-platform-migration-faq/solutions.png
[5]: ./media/security-center-platform-migration-faq/use-another-workspace.png
