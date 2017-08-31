--- 
title: "Azure Otomasyonu Kullanmaya Başlama | Microsoft Docs"
description: "Bu makale, Azure Market tekliflerini eklemeyle ilgili tasarım ve uygulama ayrıntılarını gözden geçirerek Azure Otomasyonu hizmetine genel bakış sağlar."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/18/2017
ms.author: magoedte
ms.translationtype: HT
ms.sourcegitcommit: 847eb792064bd0ee7d50163f35cd2e0368324203
ms.openlocfilehash: 54f137b26bf1c8f966e8ef110dcf3d25abf7ac5b
ms.contentlocale: tr-tr
ms.lasthandoff: 08/19/2017

---

# <a name="getting-started-with-azure-automation"></a>Azure Otomasyonu’nu Kullanmaya Başlama

Bu başlangıç kılavuzunda Azure Otomasyonu’nun dağıtımıyla ilgili temel kavramlar açıklanmaktadır. Azure’da Otomasyon’u kullanmaya yeni başladıysanız veya System Center Orchestrator gibi otomasyon iş akışı yazılımlarıyla ilgili deneyiminiz varsa bu kılavuz, Otomasyon’u nasıl hazırlayacağınızı ve kullanmaya başlayacağınızı anlamanıza yardımcı olur.  Ardından işlem otomasyonu ihtiyaçlarınızı karşılamak amacıyla runbook’lar geliştirmeye başlamak için hazır olacaksınız. 


## <a name="automation-architecture-overview"></a>Otomasyon mimarisine genel bakış

![Azure Automation’a genel bakış](media/automation-offering-get-started/automation-infradiagram-networkcomms.png)

Azure Otomasyonu, runbook’lar ile işlemleri otomatik hale getirmek ve Azure, diğer bulut hizmetleri ya da şirket içinde İstenen Durum Yapılandırması (DSC) kullanarak Windows ve Linux sistemlerinde yapılan değişiklikleri yönetmek üzere ölçeklenebilir, güvenilir ve çok kiracılı bir ortam sağlayan hizmet olarak yazılım (SaaS) uygulamasıdır. Otomasyon hesabınızda bulunan runbook, varlık ve Farklı Çalıştır hesapları gibi varlıklar, aboneliğinizdeki ve diğer aboneliklerdeki başka Otomasyon hesaplarından yalıtılır.  

Azure'da çalıştırdığınız runbook'lar, Azure hizmet olarak platform (PaaS) sanal makinelerinde barındırılan Otomasyon korumalı alanı üzerinde yürütülür.  Otomasyon korumalı alanları, runbook yürütme işleminin tüm yönleri için (modüller, depolama, bellek, ağ iletişimi, iş akışları vb.) kiracı yalıtımı sağlar. Bu rol, hizmet tarafından yönetilir ve denetlemek için Azure veya Azure Otomasyonu hesabınızdan bu role erişilemez.         

Yerel veri merkezinizde veya diğer bulut hizmetlerinde bulunan kaynakların dağıtım ve yönetimini otomatikleştirmek için, Otomasyon hesabı oluşturduktan sonra [Karma Runbook Çalışanı (HRW)](automation-hybrid-runbook-worker.md) rolünü çalıştıracak bir veya daha fazla makine belirleyebilirsiniz.  Her HRW, Log Analytics çalışma alanıyla bağlantısı olan bir Microsoft Yönetim Aracısı ve bir Otomasyon hesabı gerektirir.  Log Analytics, yüklemenin önyüklemesini yapmak, Microsoft Yönetim Aracısını korumak ve HRW işlevselliğini izlemek için kullanılır.  Runbook’ların teslim edilmesi ve çalıştırma yönergeleri, Azure Otomasyonu tarafından gerçekleştirilir.

Runbook’larını için yüksek kullanılabilirlik sağlamak, runbook işlerinin yük dengelemesini yapmak ve bazı durumlarda runbook’ları belirli iş yükleri veya ortamlar için ayırmak üzere birden fazla HRW dağıtabilirsiniz.  HRW üzerindeki Microsoft Monitoring Agent, TCP bağlantı noktası 443 üzerinden Otomasyon hizmeti ile iletişim başlatır. Gelen veriler için güvenlik duvarı gereksinimi yoktur.  Ortamdaki bir HRW üzerinde çalışan runbook’unuzun ortamdaki diğer makineler veya hizmetler üzerinde yönetim görevleri gerçekleştirmesini istiyorsanız, bunun için runbook’un başka bağlantı noktalarına erişmesi gerekebilir.  BT güvenlik ilkeleriniz ağınızdaki bilgisayarların İnternet’e bağlanmasına izin vermiyorsa, HRW’nin Otomasyon hesabınızdaki iş durumu bilgilerini toplayan ve yapılandırma bilgilerini alan proxy’si olarak davranan [OMS Ağ Geçidi](../log-analytics/log-analytics-oms-gateway.md) makalesini gözden geçirin.

Bir HRW üzerinde çalışan runbook’lar, bilgisayardaki yerel Sistem hesabı bağlamında çalışır; bu bağlam, yerel Windows makinesinde yönetim eylemleri gerçekleştirirken önerilen güvenlik bağlamıdır. Runbook’un yerel makine dışındaki kaynaklarda görevler çalıştırmasını istiyorsanız, Otomasyon hesabında runbook’tan erişebileceğiniz ve dış kaynakla kimlik doğrulaması yapmak için kullanabileceğiniz güvenli kimlik bilgisi varlıkları tanımlamanız gerekebilir. Runbook’unuzda [Kimlik Bilgisi](automation-credentials.md), [Sertifika](automation-certificates.md) ve [Bağlantı](automation-connections.md) varlıklarını, kimlik doğrulaması yapabilmek için kimlik bilgilerini belirtmenize olanak tanıyan cmdlet’lerle birlikte kullanabilirsiniz.

Azure Otomasyonu'nda depolanan DSC yapılandırmaları, Azure sanal makinelerine doğrudan uygulanabilir. Diğer fiziksel ve sanal makineler, yapılandırmaları Azure Automation DSC çekme sunucusundan isteyebilir.  Şirket içi fiziksel veya sanal Windows ve Linux sistemlerinizin yapılandırmalarını yönetmek için, Automation DSC çekme sunucusunu destekleyen herhangi bir altyapı dağıtmanız gerekmez; yalnızca TCP bağlantı noktası 443 üzerinden OMS hizmetiyle iletişim kurarak Automation DSC tarafından yönetilecek her sistemden giden İnternet erişimi gereklidir.   

## <a name="prerequisites"></a>Ön koşullar

### <a name="automation-dsc"></a>Automation DSC
Azure Automation DSC çeşitli makineleri yönetmek için kullanılabilir:

* Windows veya Linux çalıştıran Azure sanal makineleri (klasik)
* Windows veya Linux çalıştıran Azure sanal makineleri
* Windows veya Linux çalıştıran Amazon Web Services (AWS) sanal makineleri
* Şirket içinde veya Azure ya da AWS dışındaki bir bulutta bulunan fiziksel/sanal Windows bilgisayarları
* Şirket içinde veya Azure ya da AWS dışındaki bir bulutta bulunan fiziksel/sanal Linux bilgisayarları

Windows için PowerShell DSC aracısının Azure Otomasyonu ile iletişim kurabilmesi için en son WMF 5 sürümü yüklü olmalıdır. Linux’un Azure Otomasyonu ile iletişim kurabilmesi için [Linux için PowerShell DSC aracısının](https://www.microsoft.com/en-us/download/details.aspx?id=49150) en son sürümü yüklü olmalıdır.

### <a name="hybrid-runbook-worker"></a>Karma Runbook Çalışanı  
Karma runbook işleri çalıştırmak üzere bir bilgisayar belirlerken, bu bilgisayarın aşağıdakilere sahip olması gerekir:

* Windows Server 2012 veya üzeri
* Windows PowerShell 4.0 veya üzeri.  Daha fazla güvenilirlik için bilgisayara Windows PowerShell 5.0 yüklenmesi önerilir. Yeni sürümü [Microsoft Yükleme Merkezi](https://www.microsoft.com/download/details.aspx?id=50395)'nden indirebilirsiniz
* En az iki çekirdek
* En az 4 GB RAM

### <a name="permissions-required-to-create-automation-account"></a>Otomasyon hesabı oluşturmak için gereken izinler
Otomasyon hesabını oluşturmak veya güncelleştirmek isterseniz bu konuyu tamamlamak için gereken aşağıdaki özel ayrıcalıklara ve izinlere sahip olmanız gerekir.   
 
* Bir Otomasyon hesabı oluşturmak için AD kullanıcı hesabınızın, [Azure Otomasyonu’nda rol tabanlı erişim denetimi](automation-role-based-access-control.md) makalesinde açıklandığı gibi Microsoft.Automation kaynaklarındaki Sahip rolüne eşdeğer izinlere sahip bir role eklenmesi gerekir.  
* Azure AD kiracınızdaki yönetici olmayan kullanıcılar, Uygulama kayıtları ayarı [Evet](../azure-resource-manager/resource-group-create-service-principal-portal.md#check-azure-subscription-permissions) olarak ayarlıysa **AD uygulamalarını kaydedebilir**.  Uygulama kayıtları ayarı **Hayır** olarak ayarlanırsa bu işlemi gerçekleştiren kullanıcının, Azure AD’de genel yönetici olması gerekir. 

Aboneliğin genel yönetici/ortak yönetici rolüne eklenmeden önce aboneliğin Active Directory örneğine üye değilseniz Active Directory’ye konuk olarak eklenirsiniz. Bu durumda, “Oluşturma izniniz yok…” iletisini alırsınız. uyarısını **Otomasyon Hesabı Ekle** dikey penceresinde görürsünüz. İlk olarak genel yönetici/ortak yönetici rolüne eklenen kullanıcılar aboneliğin Active Directory örneğinden kaldırılabilir ve tekrar eklenerek Active Directory’de tam bir Kullanıcı haline getirilebilir. Bu durumu doğrulamak için Azure portalındaki **Azure Active Directory** bölmesinde **Kullanıcılar ve gruplar**’ı, **Tüm kullanıcılar**’ı seçin ve belirli bir kullanıcıyı seçtikten sonra **Profil**’i seçin. Kullanıcı profili altındaki **Kullanıcı türü** özniteliğinin **Konuk** olmaması gerekir.

## <a name="authentication-planning"></a>Kimlik doğrulaması planlama
Azure Otomasyonu, Azure’daki şirket içindeki kaynaklara karşı ve diğer bulut sağlayıcılarıyla görevleri otomatikleştirmenizi sağlar.  Runbook'un gerekli işlemlerini gerçekleştirebilmesi için, abonelikte gereken en düşük haklara sahip kaynaklara güvenli erişim izinlerinin olması gerekir.  

### <a name="what-is-an-automation-account"></a>Otomasyon Hesabı nedir? 
Azure Otomasyonu’nda Azure cmdlet’lerini kullanarak kaynaklara karşı gerçekleştirdiğiniz tüm otomasyon görevleri, Azure Active Directory kuruluş kimliği kimlik bilgilerine dayalı kimlik doğrulaması kullanılarak Azure’da doğrulanır.  Otomasyon hesabı, Azure kaynaklarını yapılandırmak ve kullanmak üzere portalda oturum açmak için kullandığınız hesaptan farklıdır.  Hesaba dahil olan otomasyon kaynakları şunlardır:

* **Sertifikalar** - Runbook’tan veya DSC yapılandırmasından kimlik doğrulaması için kullanılan bir sertifika içerir. Bunları siz de ekleyebilirsiniz.
* **Bağlantılar** - Runbook’tan veya DSC yapılandırmasından dış hizmete veya uygulamaya bağlanmak için gereken kimlik doğrulaması ve yapılandırma bilgilerini içerir.
* **Kimlik bilgileri** - Runbook’tan veya DSC yapılandırmasından kimlik doğrulamak için gereken kullanıcı adı ve parola gibi güvenlik kimlik bilgilerini içeren PSCredential nesnesidir.
* **Tümleştirme modülleri** - Runbook’larda ve DSC yapılandırmalarında cmdlet’lerin kullanılabilmesi için Azure Otomasyonu hesabına dahil edilen PowerShell modülleridir.
* **Zamanlamalar** - Bir runbook’u yineleme sıklıkları dahil belirtilen zamanda başlatan veya durduran zamanlamaları içerir.
* **Değişkenler** -Runbook veya DSC yapılandırmasından kullanılabilen değerleri içerir.
* **DSC Yapılandırmaları** - Windows veya Linux bilgisayarlarda bir işletim sistemi özelliğinin ya da ayarının nasıl yapılandırılacağını veya işletim sistemine bir uygulamanın nasıl yükleneceğini açıklayan PowerShell betikleridir.  
* **Runbook’lar** - Windows PowerShell’i temel alarak Azure Otomasyonu’nda bazı otomatik işlemleri gerçekleştiren görevler gruplarıdır.    

Her Otomasyon hesabı için Otomasyon kaynakları tek bir Azure bölgesiyle ilişkilendirilir, ancak Otomasyon hesapları aboneliğinizdeki tüm kaynakları yönetebilir. Kaynakların belirli bir bölgede yalıtılmasını gerektiren ilkeleriniz varsa, farklı bölgelerde Otomasyon hesapları oluşturun.

> [!NOTE]
> Azure portalında oluşturulan Automation hesapları ve içerdikleri kaynaklara Klasik Azure portalında erişilemez. Bu hesapları veya kaynaklarını Windows PowerShell’le yönetmek istiyorsanız, Azure Resource Manager modüllerini kullanmanız gerekir.
> 

Azure portalında bir Otomasyon hesabı oluşturduğunuzda otomatik olarak iki kimlik doğrulama varlığı oluşturursunuz:

* Bir Farklı Çalıştır hesabı. Bu hesap, Azure Active Directory'de (Azure AD) bir hizmet sorumlusu ve bir sertifika oluşturur. Ayrıca, runbook kullanarak Resource Manager kaynaklarını yöneten Katkıda Bulunan rol tabanlı erişim denetimi (RBAC) iznini atar.
* Klasik Farklı Çalıştır hesabı. Bu hesap, runbook kullanarak klasik kaynakları yönetmek için kullanılan bir yönetim sertifikasını karşıya yükler.

Rol tabanlı erişim denetimi, Azure AD kullanıcı hesabı ve Farklı Çalıştır hesabına izin verilen eylemleri vermek, ve bu hizmet sorumlusunun kimliğini doğrulamak için Azure Resource Manager ile kullanılabilir.  Otomasyon izinlerinin yönetilmesi için modelinizin geliştirilmesine yardımcı olma hakkında daha fazla bilgi için [Azure Otomasyonu’nda rol tabanlı erişim denetimi](automation-role-based-access-control.md) makalesini okuyun.  

#### <a name="authentication-methods"></a>Kimlik doğrulama yöntemleri
Aşağıdaki tabloda, Azure Otomasyonu tarafından desteklenen her ortamla ilgili farklı kimlik doğrulaması yöntemleri özetlenmiştir.

| Yöntem | Ortam 
| --- | --- | 
| Azure Farklı Çalıştır ve Klasik Farklı Çalıştır hesabı |Azure Resource Manager ve Azure klasik dağıtımı |  
| Azure AD Kullanıcı hesabı |Azure Resource Manager ve Azure klasik dağıtımı |  
| Windows kimlik doğrulaması |Yerel veri merkezi veya Karma Runbook Çalışanı kullanan diğer bulut sağlayıcısı |  
| AWS kimlik bilgileri |Amazon Web Hizmetleri |  

**Nasıl yapılır\Kimlik doğrulaması ve güvenlik** bölümü altında, ilgili ortamlar için var olan veya ayırdığınız yeni bir hesapla kimlik doğrulamasını yapılandırmaya yönelik genel bakış ve uygulama adımları verilmektedir.  Azure Farklı Çalıştır ve Klasik Farklı Çalıştır hesabı için, [Otomasyon Farklı Çalıştır hesabını güncelleştirme](automation-create-runas-account.md) konu başlığında, başlangıçta Farklı Çalıştır veya Klasik Farklı Çalıştır hesabıyla yapılandırılmamışsa mevcut Otomasyon hesabınızı portaldan veya PowerShell kullanarak Farklı Çalıştır hesaplarıyla güncelleştirme işlemi açıklanmaktadır. Kuruluş sertifika yetkiliniz (CA) tarafından verilen bir sertifikayla Farklı Çalıştır ve Klasik Farklı Çalıştır hesabı oluşturmak istiyorsanız bu yapılandırmayı kullanarak hesapları oluşturma hakkında bilgi edinmek için bu makaleyi gözden geçirin.     
 
## <a name="network-planning"></a>Ağ planlama
Karma Runbook Çalışanınızın Microsoft Operations Management Suite’e (OMS) bağlanması ve kaydolması için aşağıda belirtilen bağlantı noktası numarası ve URL’lere erişiminin olması gerekir.  Bunlar dışında, OMS’ye bağlanmak için [Microsoft İzleme Aracısının gerektirdiği bağlantı noktaları ve URL’ler](../log-analytics/log-analytics-windows-agents.md#network) mevcuttur. Aracı ile OMS hizmeti arasındaki iletişim için bir ara sunucu kullanıyorsanız uygun kaynakların erişilebilir olduğundan emin olmanız gerekir. İnternet'e erişimi kısıtlamak için güvenlik duvarı kullanıyorsanız erişime izin vermek için güvenlik duvarınızı yapılandırmanız gerekir.

Aşağıdaki bilgiler, Karma Runbook Çalışanının Otomasyon ile iletişim kurması için gereken bağlantı noktası ve URL’leri listeler.

* Bağlantı noktası: Giden İnternet erişimi için yalnızca TCP 443 gereklidir
* Genel URL: *.azure-automation.net

Belirli bir bölge için tanımlanmış bir Otomasyon hesabınız varsa ve bu bölgesel veri merkezi ile iletişimi kısıtlamak istiyorsanız, aşağıdaki tabloda her bölgeye yönelik DNS kaydı verilmiştir.

| **Bölge** | **DNS Kaydı** |
| --- | --- |
| Orta Güney ABD |scus-jobruntimedata-prod-su1.azure-automation.net |
| Doğu ABD 2 |eus2-jobruntimedata-prod-su1.azure-automation.net |
| Batı Orta ABD | wcus-jobruntimedata-prod-su1.azure-automation.net |
| Batı Avrupa |we-jobruntimedata-prod-su1.azure-automation.net |
| Kuzey Avrupa |ne-jobruntimedata-prod-su1.azure-automation.net |
| Orta Kanada |cc-jobruntimedata-prod-su1.azure-automation.net |
| Güneydoğu Asya |sea-jobruntimedata-prod-su1.azure-automation.net |
| Orta Hindistan |cid-jobruntimedata-prod-su1.azure-automation.net |
| Japonya Doğu |jpe-jobruntimedata-prod-su1.azure-automation.net |
| Avustralya Güneydoğu |ase-jobruntimedata-prod-su1.azure-automation.net |
| Birleşik Krallık Güney | uks-jobruntimedata-prod-su1.azure-automation.net |
| ABD Devleti Virginia | usge-jobruntimedata-prod-su1.azure-automation.us |

Adların yerine IP adreslerinin bir listesi için Microsoft Yükleme Merkezi’nden [Azure Veri Merkezi IP adresi](https://www.microsoft.com/download/details.aspx?id=41653) xml dosyasını indirip gözden geçirin. 

> [!NOTE]
> Bu dosya, Microsoft Azure Veri Merkezlerinde kullanılan IP adresi aralıklarını (İşlem, SQL ve Depolama aralıkları dahil olmak üzere) içerir. O anda dağıtılmış aralıkları ve IP adreslerinde gelecekte yapılacak değişiklikleri yansıtan güncelleştirilmiş bir dosya haftalık olarak yayınlanır. Dosyada görünen yeni aralıklar en az bir hafta boyunca veri merkezlerinde kullanılmaz. Lütfen her hafta yeni xml dosyasını indirin ve Azure’da çalışan hizmetleri doğru şekilde tanımlamak üzere sitenizde gerekli değişiklikleri yapın. Express Route kullanıcıları bu dosyanın, her ayın ilk haftasında Azure alanındaki BGP tanıtımını güncelleştirmek için kullanıldığını fark edebilir. 
> 

## <a name="creating-an-automation-account"></a>Otomasyon hesabı oluşturma

Azure portalında bir Otomasyon hesabı oluşturmak için farklı yöntemler vardır.  Aşağıdaki tabloda her dağıtım deneyiminin türü ve aralarındaki farklılıklar verilmiştir.  

|Yöntem | Açıklama |
|-------|-------------|
| Market’ten Otomasyon ve Denetim seçme | Aynı kaynak grubunda ve bölgede birbirine bağlı bir Otomasyon hesabı ve OMS çalışma alanı oluşturan bir teklif.  OMS tümleştirmesi, zaman içinde runbook iş durumunu ve iş akışlarını izlemek, analiz etmek ve sorunların iletilmesini veya araştırılmasını sağlayan gelişmiş özelliklerinden yararlanmak için Log Analytics’in kullanılmasına da imkan tanır. Teklif, varsayılan olarak etkin olan Değişiklik İzleme ve Güncelleştirme Yönetimi çözümlerini de dağıtır. |
| Market’ten Otomasyon seçme | OMS çalışma alanına bağlı olmayan yeni veya mevcut bir kaynak grubunda Otomasyon hesabı oluşturur ve Otomasyon ve Denetim teklifindeki çözümleri içermez. Bu temel yapılandırma sizi Otomasyon ile tanıştırır ve runbook yazma, DSC yapılandırmaları ve hizmetin özelliklerini kullanma hakkında bilgi edinmenize yardımcı olabilir. |
| Seçili Yönetim çözümleri | **[Güncelleştirme Yönetimi](../operations-management-suite/oms-solution-update-management.md)**, **[Mesai saatleri dışında VM’leri başlatma/durdurma](automation-solution-vm-management.md)** veya **[Değişiklik İzleme](../log-analytics/log-analytics-change-tracking.md)** gibi bir çözüm seçerseniz, mevcut bir Otomasyon ve OMS çalışma alanını seçmeniz istenir ya da çözümün aboneliğinizde dağıtılması için gerekiyorsa her ikisini de oluşturma seçeneği sunulur. |

Bu konu başlığı, Otomasyon ve Denetim teklifi eklenerek bir Otomasyon hesabı ve OMS çalışma alanı oluşturma işleminde size yol gösterir.  Teste yönelik tek başına Otomasyon hesabı oluşturmak veya hizmetin önizlemesini görmek için, aşağıdaki [Tek başına Otomasyon hesabı oluşturma](automation-create-standalone-account.md) makalesini gözden geçirin.  

### <a name="create-automation-account-integrated-with-oms"></a>OMS ile tümleştirilmiş Otomasyon hesabı oluşturma
Otomasyon eklemek için önerilen yöntem, Market’ten Otomasyon ve Denetim teklifinin seçilmesidir.  Bu işlem hem bir Otomasyon hesabı oluşturur hem de teklifle birlikte sunulan yönetim çözümlerini yükleme seçeneğiyle birlikte OMS çalışma alanı ile tümleştirme sağlar.  

1. Azure portalında Abonelik Yöneticileri rolünün üyesi ve aboneliğin ortak yöneticisi olan bir hesapla oturum açın.

2. **Yeni**’ye tıklayın.<br><br> ![Azure portalında Yeni seçeneğini belirleyin](media/automation-offering-get-started/automation-portal-martketplacestart.png)<br>  

3. **Otomasyon** araması yapın ve sonra arama sonuçlarından **Otomasyon ve Denetim*** öğesini seçin.<br><br> ![Market’te Otomasyon ve Denetim araması yapıp seçin](media/automation-offering-get-started/automation-portal-martketplace-select-automationandcontrol.png).<br>   

4. Teklifin açıklamasını okuduktan sonra **Oluştur**’a tıklayın.  

5. **Otomasyon ve Denetim** ayarları dikey penceresinde **OMS Çalışma Alanı**’nı seçin.  **OMS Çalışma Alanları** dikey penceresinde, Otomasyon hesabının bulunduğu Azure aboneliğine bağlı olan bir OMS çalışma alanı seçin ya da yeni bir OMS çalışma alanı oluşturun.  OMS çalışma alanınız yoksa **Yeni Çalışma Alanı Oluştur**’u seçip **OMS Çalışma Alanı** dikey penceresinde aşağıdakileri yapın: 
   - Yeni **OMS Çalışma Alanı** için bir ad belirtin.
   - Varsayılan seçili abonelik uygun değilse açılan listeden bağlanacak bir **Abonelik** seçin.
   - **Kaynak Grubu** için bir kaynak grubu oluşturabilir veya mevcut bir kaynak grubunu seçebilirsiniz.  
   - Bir **Konum** seçin.  Şu anda yalnızca **Avustralya Güneydoğu**, **Doğu ABD**, **Güneydoğu Asya**, **Batı Orta ABD** ve **Batı Avrupa** konumları kullanılabilir.
   - Bir **Fiyatlandırma katmanı** seçin.  Çözüm iki katmanda sunulur: ücretsiz ve Düğüm Başına (OMS) katmanı.  Ücretsiz katmanında günlük toplanan veri miktarı, elde tutma süresi ve runbook işi çalışma zamanı dakika sayısına ilişkin sınırlar vardır.  Düğüm Başına (OMS) katmanında günlük toplanan veri miktarı için bir sınır yoktur.  
   - **Otomasyonu Hesabı**’nı seçin.  Yeni bir OMS çalışma alanı oluşturuyorsanız Azure aboneliğiniz, kaynak grubunuz ve bölgeniz dahil olmak üzere belirtilen yeni OMS çalışma alanı ile ilişkilendirilen bir Otomasyon hesabı da oluşturmanız gerekir.  **Otomasyon hesabı oluştur**’u seçin ve **Otomasyon Hesabı** dikey penceresinde aşağıdaki bilgileri girin: 
  - **Ad** alanına Otomasyon hesabının adını girin.

    Tüm diğer seçenekler, seçili OMS çalışma alanına dayalı olarak otomatik doldurulur ve bu seçenekler değiştirilemez.  Teklif için varsayılan kimlik doğrulama yöntemi, bir Azure Farklı Çalıştır hesabıdır.  **Tamam**’a tıkladığınızda yapılandırma seçenekleri doğrulanır ve Otomasyon hesabı oluşturulur.  Bu işlemin ilerleme durumunu menüdeki **Bildirimler**’in altından izleyebilirsiniz. 

    Aksi takdirde, mevcut bir Otomasyon Farklı Çalıştır hesabını seçebilirsiniz.  Seçtiğiniz hesap önceden başka bir OMS çalışma alanına bağlı olamaz, böyle olması durumunda dikey pencerede bir bildirim iletisi gösterilir.  Önceden bağlıysa, farklı bir Otomasyon Farklı Çalıştır hesabı seçmeniz veya bir hesap oluşturmanız gerekir.

    Gerekli bilgileri doldurduktan sonra **Oluştur**’a tıklayın.  Bilgiler doğrulanır ve Otomasyon Hesabı ile Farklı Çalıştır hesapları oluşturulur.  Otomatik olarak **OMS çalışma alanı** dikey penceresine geri dönersiniz.  

6. **OMS Çalışma Alanı** dikey penceresinde gerekli bilgileri girdikten sonra **Oluştur**’a tıklayın.  Bilgilerin doğrulanıp çalışma alanının oluşturulması sırasında işlemin ilerleme durumunu menüdeki **Bildirimler**’in altından izleyebilirsiniz.  **Çözüm Ekle** dikey penceresine geri dönersiniz.  

7. **Otomasyon ve Denetim** ayarları dikey penceresinde, önceden seçilmiş önerilen çözümleri yüklemek istediğinizi onaylayın. Herhangi bir seçimi kaldırırsanız, daha sonra tek tek yükleyebilirsiniz.  

8. Otomasyon ve OMS çalışma alanı ekleme işlemine devam etmek için **Oluştur**’a tıklayın. Tüm ayarlar doğrulanır ve sonra teklifin aboneliğinize dağıtılması denenir.  Bu işlemin tamamlanması birkaç saniye alabilir ve ilerleme durumunu menüdeki **Bildirimler**’in altından izleyebilirsiniz. 

Teklif eklendikten sonra runbook oluşturmaya başlayabilir, etkinleştirdiğiniz yönetim çözümleriyle çalışabilir, [Karma Runbook çalışanı](automation-hybrid-runbook-worker.md) rolü dağıtabilir veya bulut ya da şirket içi ortamlarınızdaki kaynaklar tarafından oluşturulan verileri toplamak üzere [Log Analytics](https://docs.microsoft.com/azure/log-analytics) ile çalışabilirsiniz.   

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Otomasyonu Farklı Çalıştır hesabı kimlik doğrulama testi](automation-verify-runas-authentication.md) bölümünü gözden geçirerek, yeni Otomasyon hesabınızın Azure kaynaklarıyla kimlik doğrulaması yapıp yapamadığını onaylayabilirsiniz.
* Runbook'lar oluşturmaya başlamak için yazmaya başlamadan önce desteklenen [Otomasyon runbook türlerini](automation-runbook-types.md) ve ilgili noktaları gözden geçirin.



