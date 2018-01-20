---
title: "Azure Automation ile çalışmaya başlama | Microsoft Docs"
description: "Bu makalede Azure Otomasyon hizmetine genel bir bakış sağlar. Onboarding Azure Marketi'nden teklifi için hazırlık tasarım ve uygulama ayrıntıları inceler."
services: automation
documentationcenter: 
author: georgewallace
manager: carmonm
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/31/2017
ms.author: magoedte
ms.openlocfilehash: d6ee5c35ce9866f6106c7b5dbc51599b666c3eb1
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/20/2018
---
# <a name="get-started-with-azure-automation"></a>Azure Otomasyonunu kullanmaya başlayın

Bu makalede Azure Otomasyonu dağıtımıyla ilgili temel kavramlar tanıtılır. Azure Otomasyon için yenidir veya System Center Orchestrator gibi Otomasyon iş akışı yazılım deneyimiyle varsa, hazırlama ve yerleşik Otomasyon öğrenebilirsiniz. Bu makaleyi okuduktan sonra işlemi Otomasyon gerekliliklerini desteklemek için runbook'ları geliştirmeye başlamak hazır olması. 


## <a name="automation-architecture-overview"></a>Otomasyon mimarisine genel bakış

![Azure Automation’a genel bakış](media/automation-offering-get-started/automation-infradiagram-networkcomms.png)

Azure Otomasyonu bir yazılım, runbook'lar işlemleri otomatikleştirmek için kullanabileceğiniz ölçeklenebilir ve güvenilir bir çok kiracılı ortamı sağlayan bir hizmet (SaaS) uygulamasıdır. İstenen durum Yapılandırması'nı (DSC) Azure, diğer bulut hizmetlerini kullanabilir veya bir şirket içi ortamı yapılandırmasını yönetmek üzere Windows ve Linux sistemlere değiştirir. Runbook'lar, varlıklar ve farklı çalıştır hesapları gibi Automation hesabınız varlıklarda aboneliğinizde diğer Otomasyon hesaplarından ve diğer aboneliklerden yalıtılır.  

Azure'da çalışan runbook'ları Automation korumalı üzerinde yürütülür. Sanal bir hizmet (PaaS) sanal makine olarak Azure platformu içinde barındırılır. 

Automation korumalı runbook yürütme, modüller, depolama, bellek, ağ iletişimi ve iş akışları da dahil olmak üzere tüm yönleriyle Kiracı yalıtımı sağlar. Bu rol hizmeti tarafından yönetilir. Erişim veya rol Azure ya da Otomasyon hesabınızı yönetin.         

Bir Otomasyon hesabı oluşturduktan sonra dağıtım ve yönetim yerel veri merkeziniz ya da diğer bulut hizmetlerine kaynakların otomatik hale getirmek için bir veya daha fazla VM çalıştırmak için belirleyebileceğiniz [karma Runbook çalışanı](automation-hybrid-runbook-worker.md) rol. Her karma Runbook çalışanı Microsoft Yönetim aracısı yüklü olmasını ve Automation hesabı gerektirir. Aracıyı Azure günlük analizi çalışma alanına bir bağlantısı olması gerekir. Microsoft Yönetim Aracısı yükleme bootstrap korumak için günlük analizi kullanın ve karma Runbook çalışanı işlevselliğini izlemek. Azure Otomasyonu runbook'ları ve bunları çalıştırmaya karar yönerge teslimini gerçekleştirir.

Birden çok karma Runbook çalışanları dağıtabilirsiniz. Karma Runbook çalışanları runbook işlerini runbook'ları ve Yük Dengelemesi için yüksek kullanılabilirlik sağlamak için kullanın. Bazı durumlarda, belirli iş yükleri veya ortamlar için runbook işleri ayırabilirsiniz. Microsoft Monitoring Agent karma Runbook çalışanı'TCP bağlantı noktası 443 Otomasyon hizmeti ile iletişim başlatır. Karma Runbook çalışanları yok gelen güvenlik duvarı gereksinimleri vardır.  

Ortamınızda diğer makineler veya hizmetler karşı yönetim görevlerini gerçekleştirmek için bir karma Runbook çalışanı üzerinde çalışan bir runbook'u isteyebilirsiniz. Bu senaryoda, diğer bağlantı noktaları erişmek runbook gerekebilir. BT güvenlik ilkeleri, internet'e bağlanmak için ağınızdaki bilgisayarlar izin verme, gözden [OMS ağ geçidi](../log-analytics/log-analytics-oms-gateway.md). Operations Management Suite (OMS) ağ geçidi karma Runbook çalışanı için bir proxy olarak görev yapar. İş durumu toplar ve Otomasyon hesabınızdan yapılandırma bilgilerini alır.

Bir karma Runbook çalışanı üzerinde çalışacak Runbook'lar bilgisayarda yerel sistem hesabı bağlamında çalışır. Yerel Windows makinesinde yönetim işlemlerini gerçekleştirirken bir güvenlik bağlamı öneririz. Yerel makine dışında olan kaynaklara karşı görevleri çalıştırmak için runbook istiyorsanız, Automation hesabında güvenli kimlik bilgisi varlıkları tanımlama gerekebilir. Güvenli kimlik bilgisi varlıkları runbook'tan erişebilir ve bunları ile dış kaynak kimliğini doğrulamak için kullanın. Kullanabileceğiniz [kimlik bilgisi](automation-credentials.md), [sertifika](automation-certificates.md), ve [bağlantı](automation-connections.md) runbook'unuzda varlıklar. Varlıklar, bunları kimlik doğrulaması için kimlik bilgilerini belirtmek için kullanabileceğiniz cmdlet'leriyle kullanın.

Azure Otomasyonu'nda sanal makinelere depolanan DSC yapılandırmaları uygulayabilirsiniz. Diğer fiziksel ve sanal makine yapılandırmaları Otomasyonu DSC istek sunucusundan isteyebilir. Sistemlerinizin şirket içi fiziksel veya sanal Windows ve Linux yapılandırmaları yönetmek için Automation DSC çekme sunucusuna desteklemek için herhangi bir altyapı dağıtmanız gerekmez. Yalnızca Automation DSC kullanarak yönetecek her sisteminden giden internet erişimi gerekir. OMS hizmetine TCP bağlantı noktası 443 üzerinden iletişimi oluşur.   

## <a name="prerequisites"></a>Önkoşullar

### <a name="automation-dsc"></a>Automation DSC
Automation DSC bu makineleri yönetmek için kullanabilirsiniz:

* Azure Windows veya Linux çalıştıran sanal makineler (Klasik).
* Windows veya Linux çalıştıran Azure sanal makineler.
* Windows veya Linux çalıştıran Amazon Web Hizmetleri (AWS) sanal makineler.
* Şirket içi fiziksel ve sanal Windows bilgisayarları veya Azure veya AWS dışındaki bir bulutta.
* Şirket içi fiziksel ve sanal Linux bilgisayarları veya Azure veya AWS dışındaki bir bulutta.

Windows makineler için Windows Management Framework (WMF) 5, en son sürümü yüklenmelidir. Linux makineler, en son sürümünü [Linux için PowerShell DSC Aracısı](https://www.microsoft.com/en-us/download/details.aspx?id=49150) yüklü olması gerekir. PowerShell DSC Aracısı WMF 5 otomasyon ile iletişim kurmak için kullanır. 

### <a name="hybrid-runbook-worker"></a>Karma Runbook Çalışanı  
Karma runbook işlerini çalıştırmak için bir bilgisayar atadığınızda, bilgisayarın aşağıdaki gereksinimleri karşılaması gerekir:

* Windows Server 2012 veya üzeri.
* Windows PowerShell 4.0 veya üzeri. Daha fazla güvenilirlik için Windows PowerShell 5.0 öneririz. Yapabilecekleriniz [yeni sürümü yüklemek](https://www.microsoft.com/download/details.aspx?id=50395) Microsoft Download Center gelen.
* .NET framework 4.6.2 veya sonraki bir sürümü.
* En az iki çekirdek.
* En az 4 GB RAM.

### <a name="permissions-required-to-create-an-automation-account"></a>Bir Otomasyon hesabı oluşturmak için gereken izinler
Oluşturun veya bir Otomasyon hesabı güncelleştirin ve bu makalede açıklanan görevleri tamamlamak için aşağıdaki ayrıcalıkları ve izinleri olması gerekir:   
 
* Bir Otomasyon hesabı oluşturmak için Azure Active Directory (Azure AD) kullanıcı hesabınız için sahibi rolüne eşdeğer izinlere sahip bir rol eklenmeli **Microsoft.Automation** kaynakları. Daha fazla bilgi için bkz: [Azure automation'da rol tabanlı erişim denetimi](automation-role-based-access-control.md).  
* Azure portalında altında **Azure Active Directory** > **Yönet** > **uygulama kayıtlar**, **uygulama kayıtlar**  ayarlanır **Evet**, yönetici olmayan kullanıcıların Azure AD kiracınızda [Active Directory uygulamalarını kaydetmek](../azure-resource-manager/resource-group-create-service-principal-portal.md#check-azure-subscription-permissions). Varsa **uygulama kayıtlar** ayarlanır **Hayır**, bu eylem gerçekleştiren kullanıcıyı Azure AD genel yönetici olması gerekir. 

Aboneliğin genel yönetici/Abonelikteki rolüne eklenmeden önce aboneliğin Active Directory örneğine üyesi değilseniz, Active Directory'ye konuk olarak eklenir. Bu senaryoda, bu iletiyi gördüğünüz **Automation hesabı Ekle** sayfa: "Oluşturmak için izniniz yok." 

Bir kullanıcı genel yönetici/Abonelikteki rolüne eklenen yaparsanız ilk olarak, aboneliğin Active Directory örnekten kaldırın ve ardından bunları Active Directory'de tam kullanıcı rolünü yeniden ekleyin.

Kullanıcı rolleri doğrulamak için:
1. Azure portalında Git **Azure Active Directory** bölmesi.
2. Seçin **kullanıcılar ve gruplar**.
3. Seçin **tüm kullanıcılar**. 
4. Belirli bir kullanıcı seçtikten sonra Seç **profil**. Değeri **kullanıcı türü** özniteliğinin kullanıcının profilini altında olmamalıdır **Konuk**.

## <a name="authentication-planning"></a>Kimlik doğrulamasını planlama
Azure Automation'da Azure, şirket içi ve diğer bulut hizmetlerine kaynaklara karşı görevleri otomatik hale getirebilirsiniz. Bir runbook'un gerekli işlemlerini gerçekleştirebilmesi, güvenli bir şekilde kaynaklara erişmek için izinleri olmalıdır. Abonelikte gereken en düşük haklara sahip olmalıdır.  

### <a name="what-is-an-automation-account"></a>Bir Otomasyon hesabı nedir 
Azure Otomasyonu'nda cmdlet'lerini kullanarak kaynaklara karşı gerçekleştirdiğiniz tüm otomasyon görevleri için Azure, Azure AD kuruluş kimliği kimlik bilgileri tabanlı kimlik doğrulaması kullanarak kimlik doğrulaması.  Bir Otomasyon hesabı, yapılandırma ve Azure kaynaklarını kullanmak için portalda oturum açmak için kullandığınız hesabın ayrıdır. 

Aşağıdaki kaynaklar ile bir Otomasyon hesabı eklenir:

* **Sertifikaları**. Bir runbook veya DSC yapılandırması kimlik doğrulaması için kullanılan bir sertifika var. Sertifikalar da ekleyebilirsiniz.
* **Bağlantıları**. Bir runbook veya DSC yapılandırması bir dış hizmet veya uygulamaya bağlanmak için gerekli olan kimlik doğrulama ve yapılandırma bilgileri içerir.
* **Kimlik bilgileri**. İçeren bir **PSCredential** bir kullanıcı adı ve parola gibi güvenlik kimlik bilgilerine sahip nesne. Bir runbook veya DSC yapılandırması kimlik doğrulaması için kimlik bilgileri gereklidir.
* **Tümleştirme modülleri**. Bir Otomasyon hesabı ile dahil edilen PowerShell modülleri. Runbook'ları ve DSC yapılandırmalarında cmdlet'leri çalıştırmak için PowerShell modülleri kullanın.
* **Zamanlamalar**. Başlatmak veya yineleme sıklığı dahil olmak üzere belirli bir zamanda bir runbook'u durdurmak zamanlamaları içerir.
* **Değişkenleri**. Bir runbook veya DSC yapılandırması kullanılabilir değerler içeriyor.
* **DSC yapılandırmaları**. Bir işletim sistemi özelliği veya ayar nasıl yapılandırılacağı veya bir uygulamanın bir Windows veya Linux bilgisayarda nasıl yükleneceğini açıklayan PowerShell komut dosyaları.  
* **Runbook'ları**. Windows PowerShell tabanlı otomasyon otomatik bir işlem yapan görevler kümesi.    

Her Automation hesabı için Automation kaynakları tek bir Azure bölgesiyle ilişkilendirilir. Ancak, aboneliğinizdeki tüm kaynakları yönetmek için Automation hesaplarını kullanabilirsiniz. Kaynakların belirli bir bölgede yalıtılmasını gerektiren ilkeleriniz varsa, farklı bölgelerde Otomasyon hesapları oluşturun.

Azure portalında bir Otomasyon hesabı oluşturduğunuzda, iki kimlik doğrulama varlık otomatik olarak oluşturulur:

* **Farklı Çalıştır hesabı**. Bu hesap, aşağıdaki görevleri gerçekleştirir:
  - Azure AD içinde bir hizmet sorumlusu oluşturur.
  - Bir sertifika oluşturur.
  - Runbook'ları kullanarak Azure Resource Manager kaynaklarını yönetir Contributor Role-Based erişim denetimi'nı (RBAC) atar.
* **Klasik farklı çalıştır hesabı**. Bu hesap bir yönetim sertifikasını karşıya yükler. Sertifika, runbook'ları kullanarak Klasik kaynakları yönetmek için kullanılır.

RBAC bir Azure AD kullanıcı hesabı ve farklı çalıştır hesabı için izin verilen eylemleri vermek için Resource Manager'ile kullanılabilir. Bu hizmet sorumlusunun kimliğini doğrulamak için RBAC de kullanabilirsiniz. Daha fazla bilgi için ve Automation izinlerinin yönetilmesi için bir model geliştirerek Yardım için bkz: [Azure automation'da rol tabanlı erişim denetimi](automation-role-based-access-control.md).  

#### <a name="authentication-methods"></a>Kimlik doğrulama yöntemleri
Aşağıdaki tabloda Azure Automation'ı destekleyen her ortam için kullandığınız kimlik doğrulama yöntemlerini özetler.

| Yöntem | Ortam 
| --- | --- | 
| Azure Farklı Çalıştır ve Klasik Farklı Çalıştır hesabı |Azure Resource Manager ve Azure klasik dağıtımı |  
| Azure AD Kullanıcı hesabı |Azure Resource Manager ve Azure klasik dağıtımı |  
| Windows kimlik doğrulaması |Yerel veri merkezinde veya karma Runbook çalışanı rolü kullanarak diğer bulut hizmetlerini sağlayıcısı |  
| Amazon Web Hizmetleri kimlik bilgileri |Amazon Web Hizmetleri |  

Aşağıdaki makaleler konusu ortamlara kimlik doğrulamasını yapılandırmak için genel bakış ve uygulama adımlarını sağlar. Makalelerde, var olan kullanarak ve bu ortam için ayrılması yeni bir hesap kullanarak açıklanmaktadır. 
* [Tek başına bir Azure Otomasyonu hesabı oluşturma](automation-create-standalone-account.md)
* [Runbook'ları Azure Klasik dağıtım ve Resource Manager ile kimlik doğrulaması](automation-create-aduser-account.md)
* [Runbook'ları Amazon Web Hizmetleri ile kimlik doğrulaması](automation-config-aws-account.md)
* [Automation farklı çalıştır hesabı güncelleştirilemiyor](automation-create-runas-account.md)

Azure farklı çalıştır ve klasik farklı çalıştır hesapları için [güncelleştirme Automation farklı çalıştır hesabı](automation-create-runas-account.md) Portalı'ndan farklı çalıştır hesaplarıyla var olan Otomasyon hesabınızı güncelleştirmek açıklar. Ayrıca, Otomasyon hesabı ilk olarak bir farklı çalıştır veya Klasik farklı çalıştır hesabıyla yapılandırılmış değildi PowerShell'in nasıl kullanılacağı açıklanmaktadır. Kuruluş sertifika yetkilisi (CA) tarafından verilen bir sertifika kullanarak bir farklı çalıştır hesabı ve klasik farklı çalıştır hesabı oluşturabilirsiniz. Gözden geçirme [güncelleştirme Automation farklı çalıştır hesabı](automation-create-runas-account.md) bu yapılandırmayı kullanarak hesapları oluşturma hakkında bilgi edinmek için.     
 
## <a name="network-planning"></a>Ağınızı planlama
Karma Runbook bağlanıp OMS ile kaydetmek için çalışan, bağlantı noktası numarasını ve bu bölümde açıklanan URL'lere erişimi olmalıdır. Ek olarak budur [bağlantı noktaları ve Microsoft Monitoring Agent için gereken URL'leri](../log-analytics/log-analytics-windows-agent.md) için OMS bağlanmak için. 

OMS hizmeti ile aracı arasındaki iletişim için bir proxy sunucu kullanıyorsanız, uygun kaynaklara erişilebilir olduğundan emin olun. İnternet'e erişimi kısıtlamak için bir güvenlik duvarı kullanıyorsanız, erişimine izin vermek için güvenlik duvarını yapılandırmanız gerekir.

Aşağıdaki bağlantı noktası ve URL'leri otomasyon ile iletişim kurmak karma Runbook çalışanı rolü için gereklidir:

* Bağlantı noktası: Yalnızca TCP 443 giden internet erişimi için gereklidir.
* Genel URL'si: *.azure-automation.net.

Belirli bir bölge için tanımlanmış bir Otomasyon hesabınız varsa, o bölgesel veri merkezine iletişimi kısıtlayabilirsiniz. Aşağıdaki tabloda, her bölge için DNS kaydını sağlar.

| **Bölge** | **DNS kaydı** |
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

Bölge adları yerine bölge IP adresleri listesi için indirme [Azure veri merkezi IP adresi](https://www.microsoft.com/download/details.aspx?id=41653) Microsoft Download Center XML dosyasından. 

> [!NOTE]
> Azure veri merkezi IP adresi XML dosyası Microsoft Azure veri merkezlerinde kullanılan IP adres aralıklarını listeler. İşlem, SQL ve depolama aralıkları dosyasına dahil edilir. 
>
>Güncelleştirilen bir dosya haftalık nakledilir. Dosya şu anda dağıtılmış aralıklarını ve IP aralıklarını yaklaşan değişiklikleri yansıtır. Dosyasında yeni aralıkları, en az bir hafta boyunca veri merkezlerinde kullanılmaz. 
>
> Her hafta yeni bir XML dosyası indirmek için iyi bir fikirdir. Ardından, Azure'da çalışan hizmetleri doğru bir şekilde tanımlamak için sitenizi güncelleştirin. Azure ExpressRoute kullanıcıların bu dosyayı her ayın ilk haftası sınır ağ geçidi Protokolü (BGP) tanıtım Azure alanı güncelleştirmek için kullanılan unutmamalısınız. 
> 

## <a name="creating-an-automation-account"></a>Automation hesabı oluşturma

Aşağıdaki tabloda, Azure portalında bir Otomasyon hesabı oluşturmak için yöntemleri sunar. Aşağıdaki tabloda, her tür dağıtım deneyimi ve bunların arasındaki farklılıklar açıklanmıştır.  

|Yöntem | Açıklama |
|-------|-------------|
| Seçin **otomasyon ve Denetim** Azure markette | Bir Azure Marketi teklifi bağlantılı ve aynı kaynak grubunu ve bölge bir Otomasyon hesabı ve OMS çalışma alanı oluşturur. OMS tümleştirmesi, izlemek ve runbook iş durumu ve iş akışları zamanla çözümlemek için günlük analizi kullanmanın avantajı da içerir. İlerlet ya da sorunları araştırmak için günlük analizi Gelişmiş özellikleri de kullanabilirsiniz. Teklifi dağıtır **değişiklik izleme** ve **güncelleştirme yönetimi** varsayılan olarak etkin olan çözümler. |
| Seçin **Otomasyon** Market'te | Bu yöntem, bir OMS çalışma alanına bağlı olmayan bir yeni veya var olan kaynak grubunda bir Otomasyon hesabı oluşturur. Kullanılabilir tüm çözümlerinden içermeyen **otomasyon ve Denetim** teklifi. Bu yöntem için Otomasyon tanıtan temel bir yapılandırmadır. Runbook'ları ve DSC yapılandırmaları yazma öğrenin ve hizmet özelliklerini kullanma öğrenmenize yardımcı olabilir. |
| Seçin **Yönetim** çözümleri | Seçerseniz bir **Yönetim** çözüm de dahil olmak üzere [güncelleştirme yönetimi](../operations-management-suite/oms-solution-update-management.md), [dışı saatlerde sırasında sanal makineleri Başlat/Durdur](automation-solution-vm-management.md), veya [değişiklik izleme](../log-analytics/log-analytics-change-tracking.md), çözümü bir var olan Otomasyon hesabı ve OMS çalışma alanını seçmenizi ister. Çözümü bir Otomasyon hesabı ve aboneliğinizde dağıtılması için çözüm için gereken OMS çalışma alanı oluşturma seçeneği sunar. |

### <a name="create-an-automation-account-thats-integrated-with-oms"></a>OMS ile tümleşik bir Automation hesabı oluşturma
Yerleşik Otomasyon için seçtiğiniz olan öneririz **otomasyon ve Denetim** Market teklifi. Bu yöntemi kullanarak bir Otomasyon hesabı oluşturur ve bir OMS çalışma alanı ile tümleştirme oluşturur. Bu yöntemi kullandığınızda, ayrıca teklifi ile kullanılabilir yönetim çözümleri yükleme seçeneğiniz vardır.  

[Tek başına bir Otomasyon hesabı oluşturma](automation-create-standalone-account.md) bir Otomasyon hesabı ve OMS çalışma tarafından ekleme oluşturma işleminde size yol göstermektedir **otomasyon ve Denetim** sunar. Tek başına bir test otomasyon hesabı oluşturun veya hizmet Önizleme öğrenebilirsiniz.  

Kullanarak bir Otomasyon hesabı ve OMS çalışma alanı oluşturmak için **otomasyon ve Denetim** Market teklifi:

1. Azure portalında abonelik Yöneticileri rolünün üyesi ve bir Abonelikteki olan bir hesapla oturum açın.
2. Seçin **yeni**.<br><br> ![Azure portalında yeni seçin](media/automation-offering-get-started/automation-portal-martketplacestart.png)<br>  
3. Arama **Otomasyon**. Arama sonuçlarında seçin **otomasyon ve Denetim**.<br><br> ![Arama ve otomasyon ve denetim Azure Marketi'nde seçin](media/automation-offering-get-started/automation-portal-martketplace-select-automationandcontrol.png).<br>   
4. Teklifi için bir açıklama gözden geçirin ve ardından **oluşturma**.  
5. Altında **otomasyon ve Denetim**seçin **OMS çalışma**. Altında **OMS çalışma alanları**, Automation hesabı olan Azure aboneliğine bağlı bir OMS çalışma alanını seçin. Bir OMS çalışma alanı yoksa, seçin **yeni çalışma alanı oluştur**. Altında **OMS çalışma**: 
  1. İçin **OMS çalışma**, yeni bir çalışma alanı için bir ad girin.
  2. İçin **abonelik**, bağlantı sağlamak için bir abonelik seçin. Varsayılan seçim kullanmak istediğiniz aboneliği değilse, aşağı açılan listeden aboneliği seçin.
  3. İçin **kaynak grubu**, bir kaynak grubu oluşturabilir veya varolan bir kaynak grubu seçin.  
  4. İçin **konumu**, bir bölge seçin. Daha fazla bilgi için bkz: [Azure Otomasyonu sağlanmıştır hangi bölgeleri](https://azure.microsoft.com/regions/services/). Çözümleri iki katmanlarda sunulur: ücretsiz ve her düğüm (OMS) katmanı. Ücretsiz katmanı, günlük, saklama dönemi ve runbook iş çalışma zamanı dakika topladığı veri miktarı bir sınırı vardır. Düğüm başına (OMS) katmanı günlük toplanan veri miktarını bir sınır yoktur.  
  5. **Otomasyonu Hesabı**’nı seçin.  Yeni bir OMS çalışma alanı oluşturursanız, yeni OMS çalışma alanıyla ilişkili bir Otomasyon hesabı oluşturmanız gerekir. Azure abonelik, kaynak grubu ve bölge içerir. 
    1. Seçin **Automation hesabı oluşturma**.
    2. Altında **Otomasyon hesabı**, **adı** alanında, Otomasyon hesabının adını girin.
    Diğer tüm seçenekleri seçili OMS çalışma alanını otomatik olarak doldurulur. Bu seçenekler değiştiremezsiniz. 
    3. Teklif için varsayılan kimlik doğrulama yöntemi, bir Azure Farklı Çalıştır hesabıdır. Siz seçtikten sonra **Tamam**, yapılandırma seçenekleri doğrulanır ve Automation hesabı oluşturulur. Menüsünde ilerleme durumunu izlemek için **bildirimleri**. 
    4. Aksi takdirde, mevcut bir Otomasyon Farklı Çalıştır hesabını seçebilirsiniz. Seçtiğiniz hesap zaten başka bir OMS çalışma alanına bağlanamaz. İse, bir uyarı iletisi görüntülenir. Hesap bir OMS çalışma alanına zaten bağlıysa, farklı bir Automation farklı çalıştır hesabı seçin veya oluşturun.
    5. Girin veya gerekli bilgileri seçtikten sonra seçin **oluşturma**. Bilgiler doğrulandı ve Automation hesabı ve farklı çalıştır hesapları oluşturulur. Otomatik olarak döndürülürsünüz **OMS çalışma** bölmesi.  
6. Girin ya da gerekli bilgileri seçtikten sonra **OMS çalışma** bölmesinde, **oluşturma**.  Bilgiler doğrulandı ve çalışma alanı oluşturulur. Menüsünde ilerleme durumunu izlemek için **bildirimleri**. Döndürülürsünüz **Çözüm Ekle** bölmesi.  
7. Altında **otomasyon ve Denetim** ayarlarını onaylayın önerilen seçilmiş çözümleri yüklemek istediğiniz. Varsayılan seçeneklerinden herhangi birini değiştirirseniz, çözümleri daha sonra ayrı olarak yükleyebilirsiniz.  
8. Otomasyonu ekleme ve bir OMS çalışma alanı ile devam etmek için seçin **oluşturma**. Tüm ayarlar doğrulanır ve Azure aboneliğinizi Sunumda dağıtmak dener. Bu işlem birkaç saniye sürebilir. Menüde ilerlemesini izlemek üzere seçmek **bildirimleri**. 

Teklifi edildi olduktan sonra aşağıdaki görevleri yapabilirsiniz:
* Runbook'ları oluşturmaya başlayın.
* Etkinleştirilmiş yönetim çözümleri ile çalışır.
* Dağıtma bir [karma Runbook çalışanı](automation-hybrid-runbook-worker.md) rol.
* İle çalışmaya başlamak [günlük analizi](https://docs.microsoft.com/azure/log-analytics) Bulut veya şirket içi ortamınızdaki kaynakların tarafından oluşturulan verileri toplamak için.   

## <a name="next-steps"></a>Sonraki adımlar
* Yeni Otomasyon hesabınızda Azure kaynaklarına karşı kimlik doğrulaması olduğunu doğrulayın. Daha fazla bilgi için bkz: [Test Azure Automation farklı çalıştır hesabı kimlik doğrulaması](automation-verify-runas-authentication.md).
* Yazma başlamadan önce runbook'ları ve ilgili noktalar oluşturmaya başlamak öğrenin. Daha fazla bilgi için bkz: [Automation runbook türleri](automation-runbook-types.md).


