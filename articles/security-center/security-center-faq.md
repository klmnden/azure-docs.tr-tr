---
title: Azure Güvenlik Merkezi, sık sorulan sorular (SSS) | Microsoft Docs
description: Bu SSS, Azure Güvenlik Merkezi hakkında sorular yanıtlanmaktadır.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: be2ab6d5-72a8-411f-878e-98dac21bc5cb
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2018
ms.author: rkarlin
ms.openlocfilehash: 382f85c268b2e21a780756057f4bf78c41c791c2
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39283512"
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

**Standart katman** algılama özellikleri dahil olmak üzere, tehdit zekası, davranışsal analiz, anomali algılama, güvenlik olayları ve tehdit attribution raporları Gelişmiş tehdit ekler. Standart katman ilk 60 gün boyunca ücretsizdir. İlk 60 günden sonraki hizmeti kullanmaya devam etmeyi tercih ederseniz, biz otomatik olarak ücret alınmaya başlatın.  Yükseltmek için seçin [fiyatlandırma katmanı](https://docs.microsoft.com/azure/security-center/security-center-pricing) güvenlik ilkesinde.

## <a name="permissions"></a>İzinler
Azure Güvenlik Merkezi kullanan [rol tabanlı erişim denetimi (RBAC)](../role-based-access-control/role-assignments-portal.md), sağlayan [yerleşik roller](../role-based-access-control/built-in-roles.md) kullanıcıları, grupları ve Azure Hizmetleri için atanabilir.

Güvenlik Merkezi, güvenlik sorunlarını ve güvenlik açıklarını tanımlamak amacıyla kaynaklarınızın yapılandırmasını değerlendirir. Güvenlik Merkezi'nde, yalnızca bir kaynağa sahip, katkıda bulunan veya okuyucu rolü bir kaynağın ait olduğu kaynak grubu veya abonelik atandığında ilgili bilgiler görürsünüz.

Bkz: [Azure Güvenlik Merkezi'nde izinler](security-center-permissions.md) rolleri ve izin verilen eylemleri Güvenlik Merkezi hakkında daha fazla bilgi edinmek için.

## <a name="data-collection"></a>Veri toplama
Güvenlik Merkezi, Azure sanal makineleri (VM'ler) ve Azure harici bilgisayarları güvenlik açıklarını ve tehditleri izlemek için veri toplar. Veriler, makineden güvenlikle ilgili çeşitli yapılandırmaları ve olay günlüklerini okuyup verileri analiz için çalışma alanınıza kopyalayan Microsoft Monitoring Agent kullanılarak toplanır.

### <a name="how-do-i-disable-data-collection"></a>Veri toplama nasıl devre dışı bırakabilirim?
Otomatik sağlama varsayılan olarak kapalıdır. Otomatik kaynaklardan herhangi bir zamanda bu güvenlik ilkesi ayarı devre dışı bırakarak sağlama devre dışı bırakabilirsiniz. Otomatik sağlama güvenlik uyarıları ve sistem güncelleştirmeleri, işletim sistemi güvenlik açıkları ve uç nokta koruma hakkında öneriler almak için kesinlikle önerilir.

Veri toplamayı devre dışı bırakmak için [Azure portalında oturum açın](https://portal.azure.com)seçin **Gözat**seçin **Güvenlik Merkezi**seçip **seçin, ilke**. Otomatik sağlamayı hangi abonelik için devre dışı bırakmak istediğinizi belirtin. Bir aboneliği seçtiğinizde **güvenlik ilkesi - veri toplama** açılır. Altında **otomatik sağlama**seçin **kapalı**.

### <a name="how-do-i-enable-data-collection"></a>Veri toplama hizmetini nasıl etkinleştirebilirim?
Azure aboneliğinizi güvenlik ilkesinde veri toplamayı etkinleştirebilirsiniz. Veri toplamayı etkinleştirmek için. [Azure portalında oturum açın](https://portal.azure.com)seçin **Gözat**seçin **Güvenlik Merkezi**seçip **Güvenlik İlkesi**. Otomatik sağlamayı etkinleştirmek istediğiniz aboneliği seçin. Bir aboneliği seçtiğinizde **güvenlik ilkesi - veri toplama** açılır. Altında **otomatik sağlama**seçin **üzerinde**.

### <a name="what-happens-when-data-collection-is-enabled"></a>Veri toplama etkinleştirilirse ne olur?
Otomatik sağlama etkinleştirildiğinde Güvenlik Merkezi Microsoft Monitoring Agent'ı tüm Azure Vm'lere ve oluşturulan tüm yeni vm'lere desteklenen hazırlar. Otomatik sağlama önemle tavsiye edilir ancak el ile aracı yüklemelerini da kullanılabilir. [Microsoft Monitoring Agent uzantısını nasıl yükleyeceğiniz öğrenin](../log-analytics/log-analytics-quick-collect-azurevm.md#enable-the-log-analytics-vm-extension). 

İşlem oluşturma olayı 4688 Aracısı etkinleştirir ve *CommandLine* olay 4688 içindeki alan. VM üzerinde oluşturulan yeni süreçler olay günlüğü tarafından kaydedilir ve Güvenlik Merkezi'nin algılama hizmetleri tarafından izlenen. Her yeni bir işlem için kayıtlı ayrıntıları hakkında bilgi için bkz. [4688 açıklama alanlarına](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4688#fields). Aracı ayrıca VM üzerinde oluşturulan 4688 olayları toplar ve bunları Search'te depolar.

Aracı ayrıca için veri toplamayı etkinleştirir [Uyarlamalı uygulama denetimleri](security-center-adaptive-application.md), Güvenlik Merkezi, tüm uygulamalara izin vermek üzere Denetim modunda yerel bir AppLocker ilkesini yapılandırır. Bu, daha sonra toplanan ve Güvenlik Merkezi tarafından kullanılabilir olaylar oluşturmak AppLocker neden olur. Bu ilke, üzerinde zaten var. yapılandırılmış bir AppLocker İlkesi tüm makinelerde yapılandırılmaz dikkat edin önemlidir. 

Güvenlik Merkezi, VM'de kuşkulu bir etkinlik algıladığında, müşteri tarafından e-posta, bildirilir [güvenlik bilgilerini](security-center-provide-security-contact-details.md) sağlanmadı. Bir uyarı da Güvenlik Merkezi'nin güvenlik uyarıları panosunda görünür.



### <a name="does-the-monitoring-agent-impact-the-performance-of-my-servers"></a>İzleme Aracısı Sunucularım performansı etkiler mi?
Aracı, sistem kaynaklarının nominal bir miktarını kullanıyor ve performans üzerinde çok az bir etkiye sahip değildir. Performans etkisi ve aracı ve uzantısı hakkında daha fazla bilgi için bkz. [planlama ve işlemler Kılavuzu](security-center-planning-and-operations-guide.md#data-collection-and-storage).

### <a name="where-is-my-data-stored"></a>Verilerim nerede depolanır?
Bu Aracıdan toplanan veriler, aboneliğinizle ilişkili mevcut bir Log Analytics çalışma alanı veya yeni bir çalışma alanı içinde depolanır. Daha fazla bilgi için [veri güvenliği](security-center-data-security.md).

## <a name="using-azure-security-center"></a>Azure Güvenlik Merkezi'ni kullanma
### <a name="what-is-a-security-policy"></a>Güvenlik İlkesi nedir?
Güvenlik İlkesi belirtilen Abonelikteki kaynaklar için önerilen denetim kümesini tanımlar. Azure Güvenlik Merkezi'nde şirketinizin güvenlik gereksinimlerine veya uygulamaların türüne ya da her Abonelikteki verilerin duyarlılığına göre Azure Abonelikleriniz için ilkeler tanımlarsınız.

Azure Güvenlik Merkezi sürücü güvenlik önerilerini ve izlemeyi etkinleştirilen güvenlik ilkeleri. Güvenlik ilkeleri hakkında daha fazla bilgi edinmek için [güvenlik durumunu, Azure Güvenlik Merkezi'nde izleme](security-center-monitoring.md).

### <a name="who-can-modify-a-security-policy"></a>Güvenlik İlkesi değiştirebilecekleri?
Güvenlik ilkesini değiştirmek için bir güvenlik yöneticisi veya sahibi veya katkıda bulunanı o aboneliğin olması gerekir.

Güvenlik İlkesi yapılandırma konusunda bilgi için bkz: [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](security-center-policies.md).

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

### <a name="whats-the-difference-between-threats-detected-and-alerted-on-by-microsoft-security-response-center-versus-azure-security-center"></a>Microsoft Security Response Center ve Azure Güvenlik Merkezi tarafından uyarı ve algılanan tehditlere arasındaki fark nedir?
Microsoft Güvenlik Yanıt Merkezi (MSRC), select güvenlik Azure ağ ve altyapı izleme gerçekleştirir ve tehdit zekasını ve kötüye şikayetlerinin Üçüncü taraflardan alır. MSRC müşteri verilerini bir yasadışı veya yetkisiz bir tarafın eriştiğini veya müşterinin Azure kullanımına şartlarını kabul edilebilir kullanım için uyumlu değil, uyumlu hale geldiğinde, güvenlik olay manager müşteri bildirir. Bildirim, genellikle güvenlik ilgili kişi belirtilmezse, Azure Güvenlik Merkezi veya Azure aboneliği sahibi belirtilen güvenlik kişilere bir e-posta göndererek gerçekleşir.

Güvenlik Merkezi sürekli olarak müşterinin Azure ortamına izler ve analytics çok çeşitli kötü amaçlı etkinlik otomatik olarak algılamak için geçerli bir Azure hizmetidir. Bu algılamaların amacı, Güvenlik Merkezi panosunda güvenlik uyarıları olarak takip edilir.

### <a name="which-azure-resources-are-monitored-by-azure-security-center"></a>Hangi Azure kaynakları, Azure Güvenlik Merkezi tarafından izlenir?
Azure Güvenlik Merkezi, aşağıdaki Azure kaynakları izler:

* Sanal makineleri (VM'ler) (dahil olmak üzere [Cloud Services](../cloud-services/cloud-services-choose-me.md))
* Azure Sanal Ağları
* Azure SQL Hizmeti
* Azure Storage hesabı
* Azure Web Apps (içinde [App Service ortamı](../app-service/environment/intro.md))
* Web uygulaması güvenlik duvarı vm'lerde ve App Service ortamı gibi Azure aboneliğinizle tümleşik iş ortağı çözümleri

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

Güvenlik Merkezi, genellikle her saat yeni verileri tarar. Gecikme süresi yukarıdaki burada son tarama değil veya bir tarama işlemi başarısız oldu kötü bir durum senaryosu değerlerdir.

### <a name="why-do-i-get-the-message-vm-agent-is-missing"></a>"VM Aracısı eksik?" iletiyi neden alıyorum
Veri toplama özelliğini etkinleştirmeyi Vm'lerinde VM aracısı yüklü olmalıdır. VM Aracısı, Azure Marketi’nden dağıtılan VM’ler için varsayılan olarak yüklüdür. Diğer Vm'lere VM Aracısı yükleme hakkında daha fazla bilgi için blog gönderisine bakın [VM aracısı ve uzantıları](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/).
