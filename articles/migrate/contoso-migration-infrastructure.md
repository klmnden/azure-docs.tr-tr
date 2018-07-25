---
title: Contoso-geçiş altyapısını kurma | Microsoft Docs
description: Azure'a geçiş için Azure altyapısının nasıl Contoso ayarlar hakkında bilgi edinin.
services: azure-migrate
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/12/2018
ms.author: raynew
ms.openlocfilehash: 6f535d98ea9e1312e4d5f197d121c8d12c109449
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39002288"
---
# <a name="contoso---deploy-a-migration-infrastructure"></a>Contoso - geçiş altyapısı dağıtma

Bu makalede, nasıl bir şirket içi Contoso ayarlar inceler ve hazırlık Azure'a geçiş için ve iş hibrit bir ortamda çalıştırmak için Azure altyapı.

- Contoso için belirli bir örnek mimaridir.
- Makalede açıklanan tüm öğeleri gerekip gerekmediğini geçiş stratejinizi bağlıdır. Örneğin, yalnızca azure'daki bulutta yerel uygulamaları oluşturuyorsanız, daha az karmaşık bir ağ yapısı gerekebilir.

Bu belge, belge nasıl geçirir Contoso adlı kurgusal şirketin Microsoft Azure bulut kaynaklarına şirket makaleler serisinin saniyedir. Serinin bilgileri içerir ve bir geçiş altyapısını kurma nasıl çalışılacağını dağıtım senaryoları geçiş için şirket içi kaynaklara uygunluğunu değerlendirmek ve devre dışı geçişleri farklı türde çalıştırın. Senaryoları, karmaşık hale gelmesi ve diğer makaleler zamanla ekleyeceğiz.

**Makale** | **Ayrıntılar** | **Durum**
--- | --- | ---
[Makale 1: genel bakış](contoso-migration-overview.md) | Contoso'nun geçiş stratejisi, makale dizisini ve kullandığımız örnek uygulamaları genel bir bakış sağlar. | Kullanılabilir
2. makale: bir Azure altyapısını dağıtma | Açıklayan nasıl kendi şirket içi ve Azure altyapı Contoso bu geçiş için hazırlar. Altyapıyı, tüm geçiş makaleleri için kullanılır. | Bu makalede.
[3. makale: şirket içi kaynaklarınızı Azure'a geçiş için değerlendirme](contoso-migration-assessment.md)  | Contoso değerlendirme Wmware'de çalışan bir şirket içi iki katmanlı SmartHotel uygulamanın nasıl çalıştığını gösterir. Contoso uygulaması Vm'lerle değerlendirir [Azure geçişi](migrate-overview.md) hizmet ve uygulama SQL Server veritabanıyla [veritabanı geçiş Yardımcısı'nı](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Kullanılabilir
[4. makale: Azure sanal makineler ve yönetilen bir SQL örneği üzerinde bir uygulamayı barındırma](contoso-migration-rehost-vm-sql-managed-instance.md) | Contoso lift-and-shift ile taşıma geçiş için Azure SmartHotel şirket içi uygulama için nasıl çalıştığını gösterir. Contoso VM ön uç uygulamasını kullanarak geçirir [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)ve SQL yönetilen örneği, uygulama veritabanını kullanarak [Azure veritabanı geçiş hizmeti](https://docs.microsoft.com/azure/dms/dms-overview). | Kullanılabilir
[Makale 5: bir uygulamayı Azure vm'lerinde yeniden barındırma](contoso-migration-rehost-vm.md) | Nasıl Contoso geçirme SmartHotel uygulama sanal makinelerini Site Recovery hizmetini kullanarak Azure Vm'leri için gösterir. | Kullanılabilir
[Makale 6: Azure sanal makineleri ve Always On kullanılabilirlik grubu SQL Server üzerinde bir uygulamayı barındırma](contoso-migration-rehost-vm-sql-ag.md) | Contoso SmartHotel uygulamayı nasıl geçirdiğini gösterir. Contoso, uygulama sanal makinelerini ve veritabanı geçiş hizmeti uygulama veritabanı AlwaysOn Kullanılabilirlik grubu tarafından korunan bir SQL Server kümesine geçirmek için geçirmek için Site Recovery kullanır. | Kullanılabilir
[Makale 7: Azure sanal makineler'de Linux uygulaması barındırma](contoso-migration-rehost-linux-vm.md) | Nasıl Contoso Linux osTicket uygulamayı lift-and-shift ile taşıma geçişini Azure Vm'leri için Site RECOVERY'yi kullanarak yaptığını gösterir. | Kullanılabilir
[Makale 8: Azure sanal makineler ve Azure MySQL üzerinde bir Linux uygulaması barındırma](contoso-migration-rehost-linux-vm-mysql.md) | Contoso Linux osTicket uygulaması için Azure Site RECOVERY'yi kullanarak VM'lerin nasıl geçirdiğini gösterir ve uygulama veritabanı, MySQL Workbench kullanarak Azure MySQL Server örneğine geçirir. | Kullanılabilir
[Makale 9: bir uygulamayı Azure Web Apps ve Azure SQL veritabanında yeniden düzenleme](contoso-migration-refactor-web-app-sql.md) | Nasıl Contoso SmartHotel uygulamayı bir Azure Web uygulamasına geçirir ve uygulama veritabanının Azure SQL Server örneğine geçirir gösterir | Kullanılabilir
[Makale 10: Azure Web Apps ve Azure MySQL üzerinde bir Linux uygulaması yeniden düzenleyin.](contoso-migration-refactor-linux-app-service-mysql.md) | Linux osTicket uygulaması Contoso birden çok sitede, GitHub ile sürekli teslim için tümleşik Azure Web Apps'e nasıl geçirdiğini gösterir. Bunlar, Azure MySQL örneğine uygulama veritabanına geçirin. | Kullanılabilir
[Makale 11: TFS VSTS üzerinde yeniden düzenleyin.](contoso-migration-tfs-vsts.md) | Geçiş yaparak Contoso şirket içi Team Foundation Server (TFS) dağıtımının nasıl geçirdiğini gösterir. Bunun için Visual Studio Team Services (VSTS) azure'da. | Kullanılabilir
[Makale 12: bir uygulamayı Azure kapsayıcıları ve Azure SQL veritabanı yeniden oluşturma](contoso-migration-rearchitect-container-sql.md) | Contoso geçirir ve Azure SmartHotel uygulamasının rearchitects nasıl gösterir. Bunlar, bir Windows kapsayıcısı ve bir Azure SQL veritabanı'nda uygulama veritabanı uygulama web katmanla yeniden oluşturma. | Kullanılabilir
[Makale 13: uygulamanızı Azure'a yeniden oluşturun.](contoso-migration-rebuild.md) | Contoso Azure özellikleri ve Hizmetleri, uygulama hizmetleri, Azure Kubernetes, Azure işlevleri, Bilişsel hizmetler ve Cosmos DB dahil olmak üzere çeşitli kullanarak SmartHotel uygulamasının nasıl yeniden gösterir. | Kullanılabilir

Bu makaledeki tüm altyapı öğeleri Contoso ayarlar, tüm geçiş senaryolarını tamamlamak gerekir. 


## <a name="overview"></a>Genel Bakış

Bunlar Azure'a geçirmeden önce Contoso Azure altyapısının hazırlar önemlidir.  Genellikle, dikkat etmeniz gereken ihtiyaç duydukları beş geniş alan vardır:

**1. adım: Azure abonelikleri**: nasıl bunlar Azure satın alma ve Azure platformu ve Hizmetleri ile etkileşim?  
**2. adım: Karma kimlik**: nasıl bunlar yönetecek ve geçişten sonra şirket içi ve Azure kaynaklarına erişimi denetler? Nasıl bunlar genişletmek veya kimlik yönetimini buluta taşıyın?  
**3. adım: Olağanüstü durum kurtarma ve dayanıklılık**: nasıl bunlar emin olmanızı kesintiler ve olağanüstü durumlar oluşursa kendi uygulamalarınızı ve altyapınızı dayanıklı olduğunu?  
**4. adım: Ağ**: Bunlar, ağ altyapısını tasarlayın ve bunları nasıl kendi şirket içi veri merkeziniz ile Azure arasında bağlantı kurmak?  
**5. adım: Güvenlik**: nasıl, Güvenli Karma/Azure dağıtımı?  
**6. adım: İdare**: nasıl bunlar olmanızı güvenlik ve idare gereksinimleri ile hizalanan dağıtım?

## <a name="before-you-start"></a>Başlamadan önce

Altyapısını bakarak başlamadan önce bu makalede ele Azure özellikleri hakkında bazı bilgiler okumak isteyebilirsiniz:

- Açık lisanslama satıcıları Microsoft veya Microsoft Partners bilmeniz bulut çözüm sağlayıcıları (CSP'ler) veya Kullandıkça Öde, Kurumsal Anlaşma (EA), dahil olmak üzere Azure erişim, satın aldığınız için kullanılabilir birçok seçenek vardır. Hakkında bilgi edinin [satın alma seçenekleri](https://azure.microsoft.com/pricing/purchase-options/)ve nasıl çalıştıracağınızı okuyun [Azure abonelikleri düzenlenir](https://azure.microsoft.com/blog/organizing-subscriptions-and-resource-groups-within-the-enterprise/).
- Azure genel bakış [kimlik ve erişim yönetimi](https://www.microsoft.com/en-us/trustcenter/security/identity). Özellikle, hakkında bilgi edinin [Azure AD ve şirket içi genişletme AD buluta](https://docs.microsoft.com/azure/active-directory/identity-fundamentals). Hakkında yararlı indirilebilir e-kitap yoktur [kimlik ve erişim yönetimi (IAM) karma bir ortamda](https://azure.microsoft.com/resources/hybrid-cloud-identity/).
- Azure, karma bağlantı seçenekleriyle güçlü bir ağ altyapısı sağlar. Genel Bakış [ağ ve ağ erişim denetimi](https://docs.microsoft.com/azure/security/security-network-overview).
- Giriş yapın [Azure güvenlik](https://docs.microsoft.com/azure/security/azure-security)ve için bir plan oluşturma hakkında bilgi [idare](https://docs.microsoft.com/azure/security/governance-in-azure).


## <a name="on-premises-architecture"></a>Şirket içi mimarisi

İşte geçerli Contoso şirket içi altyapı gösteren diyagram.

 ![Contoso mimarisi](./media/contoso-migration-infrastructure/contoso-architecture.png)  

- Contoso, şehir, New York'ta olarak Doğu ABD'de bulunan bir ana veri merkezinde sahiptir.
- Amerika Birleşik Devletleri arasında üç ek yerel dalları sahiptirler.
- Ana veri merkezinin fiber metro ethernet bağlantı (500 MB/sn) ile İnternet'e bağlı.
- Her dal, yerel olarak IPSec VPN tünelleri ana merkezine iş sınıfı bağlantıları kullanarak İnternet'e bağlı. Böylece, kalıcı olarak bağlanması, tüm ağ ve internet bağlantısı en iyi duruma getirir.
- Ana veri merkezinin VMware ile tam olarak sanallaştırılır. VCenter Server 6.5 tarafından yönetilen iki ESXi 6.5 sanallaştırma konaklarını sahiptirler.
- Contoso, kimlik yönetimi ve iç ağdaki DNS sunucuları için Active Directory kullanır.
- Etki alanı denetleyicileri veri merkezindeki VMware VM'ler üzerinde çalıştırın. Yerel dalları etki alanı denetleyicilerde fiziksel sunucularda çalıştırın.


## <a name="step-1-buy-and-subscribe-to-azure"></a>1. adım: Satın almak ve Azure'a abone olma

Azure satın alma, nasıl abonelikleri Mimar ve hizmetlerinizi ve kaynaklarınızı lisans nasıl bağlayacağınızı contoso gerekir.

### <a name="buy-azure"></a>Azure satın alma

Contoso çalışmaya bir [Kurumsal Anlaşma (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/). Bu harika avantajlar, esnek faturalandırma seçenekleri dahil olmak üzere kazanmak için bunları entitling ve fiyatlandırma için iyileştirilmiş Azure için önden parasal taahhütte kapsar.

- Contoso ne tahmini Azure harcamalarınızı, yıllık olacaktır. Bunlar sözleşmesi imzalandığında tam ilk yıl boyunca Ücretli.
- Contoso yıldır üzerinden ya da bunlar bu ABD Doları değeri kaybedeceksiniz önce tüm taahhütleri kullanmanız gerekir.
- Herhangi bir nedenle bunlar taahhütte uzun ve daha fazla harcama, Microsoft bunların fark için fatura.
- Taahhüt sonucunda herhangi bir maliyet aynı ücretleri ve bunların Sözleşme'de olacaktır. Giden hiçbir yaptırımlara vardır.

### <a name="manage-subscriptions"></a>Abonelikleri yönetme

Azure için ödeme sonra Contoso Azure Aboneliklerini yönetmek nasıl gerekir. Bir EA sahiptirler ve dolayısıyla Azure abonelik sayısı sınırı bunlar ayarlayabilirsiniz.

- Bir Azure Kurumsal kayıt şirket şekli nasıl tanımlar ve Azure hizmetlerini kullanır ve bir çekirdek idare yapısını tanımlar.
- İlk adım, Contoso (kendi Kurumsal kayıt için bir kurumsal iskelesi bilinir. bir yapı belirledi. Bunlar [bu makalede](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-subscription-governance) anlamak ve bir yapı iskelesi tasarlamanıza yardımcı olmak için.
- Şimdilik, Contoso Aboneliklerini yönetmek için bir işlevsel yaklaşım kullanmaya karar verdi.
    - Kuruluşun içinde Azure bütçe denetleyen tek bir BT departmanı sahip olacaksınız. Bu tek Grup aboneliklerine sahip olacaktır.
    - Diğer Kurumsal gruplar olarak bölümlerde Kurumsal kayıt katılabilir, bu model gelecekte genişletmeniz.
    - BT departmanı iki Abonelikleri, üretim ve geliştirme Contoso yapısal.
    - Contoso ek abonelikler gelecekte gerektiriyorsa, erişim, ilkeleri ve bu Aboneliklerdeki için uyumluluğu yönetmek gerekir. Sunarak bunu mümkün olacaktır [Azure Yönetim grupları](https://docs.microsoft.com/azure/azure-resource-manager/management-groups-overview), abonelik üzerinde bir ek katmanı olarak.

    ![Kuruluş yapısı](./media/contoso-migration-infrastructure/enterprise-structure.png) 

### <a name="examine-licensing"></a>Lisanslama inceleyin

Yapılandırılmış abonelikler sayesinde, Microsoft Lisanslama Contoso bakabilirsiniz. Bunların lisans stratejisi, Azure ve nasıl Azure Vm'lerini ve hizmetlerini seçili dağıtılan ve geçirmek istediğiniz kaynakları bağlıdır. 

#### <a name="azure-hybrid-benefit"></a>Azure Hibrit Avantajı

Azure'da sanal makineler dağıtırken, standart görüntüleri Contoso kullanılan yazılım dakikasına göre ücret bir lisans içerir. Ancak, Contoso uzun süreli bir Microsoft Müşteri olmuştur ve EAs tutulması ve lisansları Yazılım Güvencesi (SA) açın. 

Azure hibrit avantajı, Azure Vm'leri ve SQL Server iş yüklerini dönüştürme veya Yazılım Güvencesi kapsamındaki Windows Server Datacenter ve Standard edition lisansları yeniden kaydetmek vererek Contoso geçiş için uygun maliyetli bir yöntem sunar. Bu durum, VM'ler ve SQL Server için daha düşük bir temel işlem ücretini ödemek Contoso olanak sağlar. [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/hybrid-benefit/).


#### <a name="license-mobility"></a>Lisans Taşınabilirliği

SA aracılığıyla lisans taşınabilirliği Contoso gibi Microsoft Toplu Lisans müşterilerine Azure üzerinde etkin SA ile uygun sunucu uygulamalarını dağıtma esnekliği verir. Bu, yeni lisans satın almanıza gerek ortadan kaldırır. Hiçbir bununla bağlantılı hareketlilik ücreti ile var olan lisansları Azure'da kolayca dağıtılabilir. [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/license-mobility/).

#### <a name="reserve-instances-for-predictable-workloads"></a>Öngörülebilir iş yükleriniz için ayrılmış örnekler

Öngörülebilir iş yükleriniz her zaman çalışan Vm'leri ile kullanılabilir olması gereken renklerdir. Örneğin, satır iş kolu uygulamaları gibi bir SAP ERP sistemi.  Öte yandan, öngörülemeyen iş yükleri değişkeni olanla aynıdır. Örneğin yüksek sırasında bulunan VM'ler, talep ve yoğun olmayan zamanlarda kapalı.

![Ayrılmış örnek](./media/contoso-migration-infrastructure/reserved-instance.png) 

Alışık oldukları süreyi büyük sürelerde saklanması gerekir belirli VM örnekleri için ayrılmış örnekler kullanarak lisanslarınıza konsolu bir indirim hem öncelikli kapasite elde edebilirsiniz. Kullanarak [Azure ayrılmış örnekleri](https://azure.microsoft.com/pricing/reserved-vm-instances/)birlikte Azure hibrit teklifi'nden Contoso kazandırabilir %82 varan normal Kullandıkça Öde fiyatlandırması (Nisan 2018).


## <a name="step-2-manage-hybrid-identity"></a>2. adım: karma Kimlik Yönetimi

Ayırabilir ve Azure kaynaklarıyla kimlik ve erişim yönetimi (IAM) kullanıcı erişimini denetleme birlikte Azure altyapınızın çekmeden içinde önemli bir adımdır.  

- Contoso yerine kendi şirket içi Active Directory erişimini bulutla genişletmenin azure'da yeni bir ayrı sistemi yapı karar verir.
- Bir Azure tabanlı Bunu yapmak için Active Directory oluştururlar.
- Yeni bir Azure AD sağlamak ihtiyaç duydukları böylece Contoso yerine, Office 365 sahip değil.
- Office 365 kullanıcı yönetimi için Azure AD kullanır. Contoso Office 365 kullanıyorsanız, bunlar zaten bir Azure AD uyarlamanız sahip ve bunların birincil AD kullanan.
- [Daha fazla bilgi edinin](https://support.office.com/article/understanding-office-365-identity-and-azure-active-directory-06a189e7-5ec6-4af2-94bf-a22ea225a7a9) Office 365, Azure AD hakkında ve bilgi [abonelik ekleme](https://docs.microsoft.com/azure/active-directory/active-directory-how-subscriptions-associated-directory) mevcut bir Azure AD için.

### <a name="create-an-azure-ad"></a>Azure AD oluşturma

Contoso, bir Azure aboneliği ile eklenmiştir. Bu Azure AD ücretsiz sürümü kullanıyor. Yeni bir AD dizini aşağıdaki şekilde ekleyin:

1. İçinde [Azure portalında](http://portal.azure.com/), Contoso gider **kaynak Oluştur** > **kimlik** > **Azure Active Directory**.
2. İçinde **dizin oluştur**, dizin için bir ad, bir ilk etki alanı adı ve hangi Azure AD dizini oluşturulmalıdır bölge belirtin.

    ![Azure AD oluşturma](./media/contoso-migration-infrastructure/azure-ad-create.png) 

    > [!NOTE]
    > Dizin oluşturduklarında form domainname.onmicrosoft.com içinde bir ilk etki alanı adı vardır. Adı değiştirilmiş veya silinmiş. Bunun yerine, kendi kayıtlı etki alanı adını Azure AD'ye eklemeniz gerekir.

### <a name="add-the-domain-name"></a>Etki alanı adı ekleme

Standart bir etki alanı adı kullanmak için Azure AD'ye özel bir ad eklemek Contoso gerekir. Bu seçenek, yöneticilerin tanıdık kullanıcı adları atama olanak sağlar. Örneğin, bir kullanıcının e-posta adresiyle oturum oturum billg@contoso.com, gerek yerine billg@contosomigration.onmicrosoft.com. 

Özel adı ayarlamak için bunlar dizine eklemek, bir DNS girişi ekleyin ve sonra Azure AD'de adını doğrulayın.

1. İçinde **özel etki alanı adları** > **özel etki alanı Ekle**, bunlar etki alanına ekleyin.
2. Azure'da bir DNS girişi kullanmak, kendi etki alanı kayıt şirketi ile kaydetmeniz gerekir. 

    - İçinde **özel etki alanı adları** listesinde, bunlar adı için DNS bilgilerini not edin. Contoso bir MX giriş kullanıyor.
    - Bunlar, bunu yapmak için ad sunucusu erişiminin olması gerekir. Contoso, söz konusu olduğunda, Contoso.com etki alanına oturum ve Not ayrıntıları kullanarak Azure AD tarafından sağlanan DNS girişini için yeni bir MX kayıt oluşturdunuz.  
1. Ayrıntıları adı etki alanı için DNS kayıtlarının yayılması sonra'ı tıklatın **doğrulama** özel ad denetlemek için.

     ![Azure AD DNS](./media/contoso-migration-infrastructure/azure-ad-dns.png) 

### <a name="set-up-on-premises-and-azure-groups-and-users"></a>Şirket içi ve Azure grupları ve kullanıcıları ayarlama

Kendi Azure AD çalışır duruma geldikten sonra çalışanlara eklemek için Contoso gereksinimlerini şirket içi AD grupları, Azure AD ile eşitler. Azure'daki kaynak grupları adları aynı şirket içi grup adlarını kullanmanızı öneririz. Bu, eşitleme amacıyla eşleşmeleri tanımlamak kolaylaştırır.

#### <a name="create-resource-groups-in-azure"></a>Azure'da kaynak grupları oluşturma

Azure kaynak grupları, Azure kaynaklarını birbirine toplayın. Bir kaynak grubu kimliği kullanarak grup içindeki kaynakları üzerinde işlem gerçekleştirmek Azure sağlar.

- Birden fazla kaynak grubu bir Azure aboneliğine sahip olabilir, ancak bir kaynak grubu yalnızca tek bir abonelik içinde bulunabilir.
- Ayrıca, birden çok kaynağı tek bir kaynak grubu olabilir, ancak bir kaynak yalnızca tek bir gruba ait olabilir.

Contoso Azure kaynak gruplarını aşağıdaki tabloda özetlendiği gibi ayarlar.

**Kaynak grubu** | **Ayrıntılar**
--- | ---
**ContosoCobRG** | Bu Grup (COB) iş sürekliliği için ilgili tüm kaynakları içerir.  Contoso Azure Site Recovery hizmeti ve Azure Backup hizmeti kullanırken oluşturduğunuz kasa içerir.<br/><br/> Ayrıca Azure geçişi ve veritabanı geçiş hizmetleri dahil olmak üzere geçiş işlemi için kullanılan kaynakları dahil edilir.
**ContosoDevRG** | Bu grup, geliştirme ve test kaynakları içerir.
**ContosoFailoverRG** | Bu grup, kaynaklarınız üzerinden işlemi başarısız oldu bir giriş bölgesi işlevi görür.
**ContosoNetworkingRG** | Bu grup, tüm ağ kaynakları içerir.
**ContosoRG** | Bu grup, üretim uygulamaları ve veritabanları için ilgili kaynakları içerir.

Bunlar gibi kaynak grupları oluşturun:

1. Azure portalında > **kaynak grupları**, bunlar bir grup ekleyin.
2. Her grup için bir ad, grubun ait olduğu aboneliğin ve bölgeyi belirtin.
3. Kaynak grupları görünür **kaynak grupları** listesi.

    ![Kaynak grupları](./media/contoso-migration-infrastructure/resource-groups.png) 


#### <a name="create-matching-security-groups-on-premises"></a>Eşleşen güvenlik grupları şirket içi oluşturma

1. Kendi şirket içi Active Directory'de güvenlik grupları ile Azure kaynak gruplarının adlarını eşleşen adlara Contoso ayarlar.
 
    ![Şirket içi AD güvenlik grupları](./media/contoso-migration-infrastructure/on-prem-ad.png) 

2. Yönetim amaçları için tüm gruplara eklenir, ek bir grubun oluştururlar. Bu grup, Azure'da tüm kaynak grupları hakkına sahip olursunuz. Sınırlı sayıda genel yöneticileri bu gruba eklenir.

### <a name="synchronize-ad"></a>AD'yi eşitleme

Contoso, bulutta ve şirket kaynaklarına erişmek için ortak bir kimlik sağlamak istiyor. Bunu yapmak için kendi şirket içi Active Directory Azure AD ile tümleştirin. Bu model ile:

- Kullanıcılar ve kuruluşlar şirket içi uygulamalara ve bulut Hizmetleri Office 365 veya internet'teki başka siteler binlerce gibi tek bir kimlik yararlanabilirsiniz.
- Yöneticiler uygulamak için AD'de grupları yararlanabilir [rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) azure'da.

Tümleştirme kolaylaştırmak için Contoso kullanan [Azure AD Connect aracını](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect). Aracı bir etki alanı denetleyicisine yüklediğinizde ve sonra yerel eşitler. şirket içi AD kimlikleri için Azure AD. 

### <a name="download-the-tool"></a>Aracı'nı indirme

1. Azure portalında Azure gider **Azure Active Directory** > **Azure AD Connect**ve eşitleme için kullandıkları sunucuyu aracın en son sürümünü indirir.

    ![AD Connect indirin](./media/contoso-migration-infrastructure/download-ad-connect.png) 

2. Başlatmaları **AzureADConnect.msi** yükleme kullanarak **hızlı ayarları kullan**. Bu, en yaygın yükleme ve tek ormanlı bir topolojiye için kimlik doğrulaması için parola karması eşitlemeyi birlikte kullanılabilir.

    ![AD Connect Sihirbazı](./media/contoso-migration-infrastructure/ad-connect-wiz1.png) 

3. İçinde **Azure ad Connect**, bunların Azure AD'de (form CONTOSO\admin veya contoso.com\admin) bağlanmak için kimlik bilgilerini belirtin.

    ![AD Connect Sihirbazı](./media/contoso-migration-infrastructure/ad-connect-wiz2.png) 

4. İçinde **AD DS'ye Bağlan**, bunlar şirket içi kimlik bilgilerini belirtmenizi AD.

     ![AD Connect Sihirbazı](./media/contoso-migration-infrastructure/ad-connect-wiz3.png) 

5. İçinde **yapılandırma için hazır**, simgeye **Yapılandırma tamamlandığında eşitleme işlemini başlatmak** eşitleme hemen başlatmak için. Ardından bunlar yükleyin.

Şunlara dikkat edin:
- Contoso Azure doğrudan bir bağlantı vardır. Şirket içi AD, bir proxy'nin arkasındayken, okuma bu [makale](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-troubleshoot-connectivity).
- İlk eşitleme sonrasında, Azure AD'de AD nesnelerini görülebilir şirket içi.

    ![Şirket içi Azure AD](./media/contoso-migration-infrastructure/on-prem-ad-groups.png) 

- Contoso BT ekibi kendi rolüne göre her grupta temsil edilir.

    ![Şirket içinde Azure AD üyeleri](./media/contoso-migration-infrastructure/on-prem-ad-group-members.png) 

### <a name="set-up-rbac"></a>RBAC ayarlayın

Azure [rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) Azure için ayrıntılı erişim yönetimi sağlar. RBAC kullanarak, yalnızca kullanıcıların işlerini yapmak için gereksinim duyduğu erişim miktarını verebilirsiniz. Kullanıcılar, gruplara ve kapsam düzeyinde uygulamalara uygun RBAC rolü atayın. Rol atamasının kapsamı, bir abonelik, kaynak grubu veya tek bir kaynak olabilir. 

Contoso, şirket içi ad'nizden eşitlenmiş AD grupları artık rollerini atar.

1. İçinde **ControlCobRG** kaynak grubu, bunlar tıklayın **erişim denetimi (IAM)** > **Ekle**.
2. İçinde **izni Ekle** > **rol**, seçtikleri **katkıda bulunan**seçip **ContosoCobRG** listeden AD grubu. Grup içinde görünür ardından **seçili üyeleri** listesi. 
3. Diğer kaynak gruplarının aynı izinlere sahip kullanıcılar bu yineleyin (dışında **ContosoAzureAdmins**), katkıda bulunan izinleri kaynak grubuyla eşleşen bir AD hesabı ekleyerek.
4. İçin **ContosoAzureAdmins** AD grubu, bunlar atama **sahibi** rol.

    ![Şirket içinde Azure AD üyeleri](./media/contoso-migration-infrastructure/on-prem-ad-groups.png) 


## <a name="step-3-design-for-resilience-and-disaster"></a>3. adım: esneklik ve olağanüstü durum için tasarlama

Azure kaynakları, bölgeleri içinde dağıtılır.
- Bölgeleri coğrafyalar halinde düzenlenir ve coğrafi sınırlar içinde veri yerleşikliği, özerkliği, uyumluluk ve dayanıklılık gereksinimleri dikkate alınır.
- Bir bölge, veri merkezlerinden oluşan bir dizi oluşur. Bu veri merkezleri gecikme süresine göre tanımlanmış bir çember içinde dağıtılmış ve adanmış bir bölgesel düşük gecikmeli ağla birbirine bağlanmış.
- Her Azure bölgesi, dayanıklılık için başka bir bölgeyle eşleştirilir.
- Hakkında bilgi edinin [Azure bölgeleri](https://azure.microsoft.com/global-infrastructure/regions/)ve anlamak [nasıl eşleştirilmiş bölgeleri](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).


Contoso, kendi ikincil bölgeye olarak orta ABD ve Doğu (Virginia içinde bulunur) ABD 2, birincil bölge olarak giderek karar vermiştir. Birkaç Bunun nedeni vardır:

- Contoso veri merkezi New York'ta bulunur ve bunlar gecikme süresi en yakın veri merkezine dikkate.
- Doğu ABD 2 bölgesinde, tüm hizmet ve ürünleri kullanmaya ihtiyaç duydukları sahiptir. Tüm Azure bölgeleri ürünlerini ve hizmetlerini kullanılabilir açısından aynıdır. Gözden geçirebilirsiniz [bölgelere göre Azure ürünleri](https://azure.microsoft.com/global-infrastructure/services/).
- Orta ABD Doğu ABD 2 için Azure eşleştirilmiş bölgedir.

Kendi hibrit ortamı hakkında düşündüklerini gibi Contoso bölgesi tasarımlarına esnekliği ve bir olağanüstü durum kurtarma stratejisi oluşturmaya nasıl göz önünde bulundurun gerekir. Genel anlamıyla stratejileri aralığı hata etki alanları ve bölgesel dayanıklılık için tam bir aktif-aktif aracılığıyla eşleştirme hangi bulut Hizmetleri ve veritabanı modeli özellikleri, dağıtılan ve hizmet, Azure platformunda kullanan bir tek bölgeli dağıtım Kullanıcıların iki bölgeleri.

Contoso Orta yol almak karar vermiştir. Bunlar, uygulamalar ve birincil bölgedeki kaynakları dağıtmanıza ve tam yedekleme tam uygulama olağanüstü durum veya bölge hata durumunda olarak davranmak hazır olması ikincil bölgede tam bir altyapı tutun.


## <a name="step-4-design-a-network-infrastructure"></a>4. adım: bir ağ altyapısını tasarlama

Bölge tasarımlarına ile yerinde, bir ağ stratejisi kullanmayı Contoso hazırdır. Bunların nasıl kendi şirket içi veri merkeziniz ile Azure bağlanmak ve birbirleri ile iletişim kurmak ve azure'da ağ altyapınızı tasarlama konusunda dikkat etmeniz gerekir. Özellikle için gerekir:

**Hibrit ağ bağlantısını planlama**: nasıl ağları şirket içi ve Azure arasında bağlantı sağlamak ilerlediklerini şekil.
**Azure ağ altyapısını tasarlayın**: Bunlar, bölgeler ağları nasıl dağıtacağınıza karar verin. Nasıl ağları, aynı bölge içinde ve bölgeler arasında iletişim kurar.
**Tasarım ve Azure ağları ayarlama**: Azure ağları ve alt ağlar ayarlayın ve bunları ne alacağını karar verin.

### <a name="plan-hybrid-network-connectivity"></a>Hibrit ağ bağlantısını planlama

Contoso kabul bir [mimarileri sayısı](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/) karma kendi şirket içi veri merkeziniz ile Azure arasında ağ için. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/considerations) seçeneklerini karşılaştırma hakkında.

Bir anımsatıcı olarak Contoso şirket içi ağ altyapısı şu anda, veri merkezi New York'ta oluşur ve Doğu ABD bölümünde yerel dal.  Tüm Konumlar internet iş sınıfı bağlantısı olmalıdır.  Her dal sonra veri merkezi bir IPSec VPN tüneli aracılığıyla Internet üzerinden bağlı.

![Contoso ağ](./media/contoso-migration-infrastructure/contoso-networking.png) 

Karma bağlantı uygulamak nasıl Contoso verdi. aşağıda verilmiştir:

1. Contoso veri merkezi New York'ta ve Doğu ABD 2 ve orta ABD iki Azure bölgesi arasında yeni bir siteden siteye VPN bağlantısı ayarlayın.
2. Azure sanal ağlarına bağlı dal office trafik, ana Contoso veri merkezi yönlendirilecek. 
3. Bunlar, Azure dağıtımınızın ölçeğini gibi kendi veri merkeziniz ile Azure bölgeleri arasında bir ExpressRoute bağlantısı oluşturmak. Bu durumda, yalnızca yük devretme amacıyla VPN siteden siteye bağlantı korumanız.
    - [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/considerations) arasında bir VPN ve ExpressRoute hibrit çözüm seçme hakkında.
    - Doğrulama [ExpressRoute konumları ve Destek](https://docs.microsoft.com/azure/expressroute/expressroute-locations-providers).


**Yalnızca VPN**

![Contoso VPN](./media/contoso-migration-infrastructure/hybrid-vpn.png) 


**VPN ve ExpressRoute**

![Contoso VPN/ExpressRoute](./media/contoso-migration-infrastructure/hybrid-vpn-expressroute.png) 


### <a name="design-the-azure-network-infrastructure"></a>Azure ağ altyapısını tasarlama

Contoso, karma dağıtım güvenli ve ölçeklenebilir bir şekilde yerinde ağları koyar önemlidir. Contoso, bunu yapmak için bir uzun vadeli yaklaşımı ve sanal ağlar (Vnet'ler) esneklik ve kurumsal kullanıma hazır olacak şekilde tasarlama. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/virtual-network/virtual-network-vnet-plan-design-arm) sanal ağları planlama hakkında daha fazla.

Kendi iki bölgeleri bağlanmak için hub hub ağ modeli uygulamak Contoso verdi:

- Her bölge içinde Contoso hub-and-spoke modelini kullanır.
- Ağlara ve hub'ları bağlamak için Azure ağ eşlemesi, Contoso kullanır.

#### <a name="network-peering"></a>Ağ eşlemesi

Azure, sanal ağlar ve hub'ları bağlamak için Ağ eşlemesi sağlar. Genel eşleme, farklı bölgelerdeki sanal ağlar/hubs arasında bağlantılar sağlar. Yerel eşleme, aynı bölgedeki sanal ağlar bağlanır. VNet eşlemesi, çok sayıda avantaj sağlar:

- Eşlenmiş Vnet'ler arasında ağ trafiği gizlidir.
- Sanal ağlar arasındaki trafik Microsoft omurga ağı üzerinde tutulur. Hiçbir genel internet, ağ geçidi veya şifreleme Vnet'ler arasındaki iletişime gereklidir.
- Eşleme varsayılan, farklı vnet'lerdeki kaynaklar arasında düşük gecikme süreli, yüksek bant genişlikli bağlantı sağlar.

[Daha fazla bilgi edinin](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) ağ eşlemesi hakkında.

#### <a name="hub-to-hub-across-regions"></a>Hub hub bölgeler

Contoso, her bölgede bir hub'ı dağıtır. Bir hub'ı, merkezi bir şirket içi ağınıza bağlantı noktası gören azure'daki bir sanal ağın (VNet) olan. Hub sanal ağlar, genel sanal ağ eşleme kullanarak bağlanır. Küresel VNet eşlemesi Vnet'ler Azure bölgeleri arasında bağlanır.

- Her bölgede hub'ı, başka bir bölgede iş ortağı hub'ına eşlenmiş.
- Hub'ı, alt bölgedeki her ağa eşlenen ve tüm ağ kaynaklarına bağlanabilir.

    ![Genel eşleme](./media/contoso-migration-infrastructure/global-peering.png) 

#### <a name="hub-and-spoke-within-a-region"></a>Hub ve bağlı bileşen bir bölge içinde

Her bölge içinde Contoso sanal ağlar farklı amaçlar için bileşen ağlarını bölge hub'ından olarak dağıtır. Bir bölge içindeki sanal ağlar, hub'ına ve birbirine bağlamak için eşlemesi'ni kullanın.

#### <a name="design-the-hub-network"></a>Hub'ı ağ tasarımı

Bunlar hakkında düşünmek zorunda Contoso seçtiği merkez ve ışınsal modelini içinde kendi veri merkezinden şirket içi ve İnternet'ten gelen trafiği, yönlendirilir. Contoso Doğu ABD 2 ve orta ABD hub'ları için yönlendirmeyi işler nasıl karar verdiği aşağıda verilmiştir:

- Bu paketleri gelen giden ağ bağlantısını izleyin yol olduğundan "ters c", bilinen bir ağ tasarlama konusunda.
- Ağ mimarisinin iki sınır, güvenilmeyen bir ön uç çevre bölge ve arka uç güvenilir bölgeye sahiptir.
- Bir güvenlik duvarı erişimi denetlemek için güvenilen her bölgede bir ağ bağdaştırıcısı gerekir.
- İnternet'ten:
    - Internet trafiği bir yük dengeli genel IP adresi çevre ağında ulaşırsınız.
    - Bu trafiğe güvenlik duvarı kuralları tabidir ve güvenlik duvarı üzerinden yönlendirilir.
    - Ağ erişim denetimleri kullanılmaya başladıktan sonra trafik güvenilir bölge içindeki uygun konumuna iletilir.
    - Sanal ağdan giden trafiği kullanıcı tanımlı yollar (Udr) kullanarak internet'e yönlendirilmez. Trafik, güvenlik duvarı üzerinden zorlamalı ve Contoso ilkelerine uygun olarak inceledi.
- Contoso veri merkezlerinden:
    - VPN siteden siteye (veya ExpressRoute) üzerinden gelen trafiği Azure VPN ağ geçidinin genel IP adresini ziyaret sayısı.
    - Trafik, güvenlik duvarı kuralları tabidir, güvenlik duvarı üzerinden yönlendirilir.
    - Kural uygulandıktan sonra trafik güvenilen iç bölgesi alt ağ üzerindeki iç yük dengeleyici (standart SKU) iletilir.
    - Güvenilir alt ağdan giden trafiği VPN üzerinden şirket içi veri merkezine, siteden siteye VPN üzerinden geçmeden önce uygulanan kuralları ve güvenlik duvarı üzerinden yönlendirilir.



### <a name="design-and-set-up-azure-networks"></a>Tasarım ve Azure ağları ayarlama

Bir ağ ve yerinde yönlendirme topolojisi ile Contoso kendi Azure ağları ve alt ağlar ayarlamak hazırdır.

- Contoso sınıfı özel ağ (0.0.0.0 için 127.255.255.255) Azure'da uygular. Bu çalışır, adres aralıkları arasında herhangi bir çakışma olmayacak emin olabilir şirket beri geçerli bir sınıf B özel adres alanı 172.160.0/16 sahiptirler.
- Bunlar, birincil ve ikincil bölgelerdeki sanal ağlar dağıtacağız.
- Bunlar ön ekini içeren bir adlandırma kuralı kullanacaksınız **VNET** ve bölge kısaltması **EUS2** veya **cu**. Bu standart'ı kullanarak, hub ağları adlandırılacağını **VNET HUB EUS2** (Doğu ABD 2), ve **VNET-HUB-cu** (Orta ABD).
- Contoso yoksa bir [IPAM çözüm](https://docs.microsoft.com/windows-server/networking/technologies/ipam/ipam-top), NAT ağ yönlendirme için planlamak ihtiyaç duydukları


#### <a name="virtual-networks-in-east-us-2"></a>Doğu ABD 2, sanal ağlar

Doğu ABD 2, Contoso kaynaklarını ve Hizmetleri dağıtmak için kullanacağı birincil bölgedir. Nasıl içindeki Mimarı ağlara bu. aşağıda verilmiştir:

- **Hub**: Doğu ABD 2, VNet hub merkezi kendi şirket içi veri merkezine birincil bağlantı noktasıdır.
- **Sanal ağlar**: uç Vnet'ler Doğu ABD 2, gerekirse iş yüklerini yalıtmak için kullanılabilir. Yanı sıra Hub sanal ağ, iki uç Vnet'ler Doğu ABD 2, Contoso olacaktır:
    - **VNET GELİŞTİRME EUS2**. Bu sanal ağ geliştirme ve test takımı geliştirme projeleri için tam olarak işlevsel bir ağ olacak sağlar. Bir üretim pilot alanı olarak görev yapacak ve üretim altyapı işlevine güvenirsiniz.
    - **VNET-PROD-EUS2**: Azure Iaas üretim bileşenleri bu ağda oluşturulur. 
    -  Her sanal ağ kendi benzersiz adres alanı ile bir çakışma sahip olur. NAT gerek kalmadan yönlendirmeyi yapılandırmak istediğiniz
- **Alt ağlar**:
    - Her uygulama katmanı için her bir ağdaki bir alt ağ olacaktır
    - Üretim ağındaki her alt ağ, geliştirme VNet içinde eşleşen bir alt ağ gerekir.
    - Ayrıca, üretim ağı, etki alanı denetleyicileri için bir alt ağ sahiptir.

Sanal ağlar Doğu ABD 2 aşağıdaki tabloda özetlenmiştir.

**Sanal ağ** | **Aralığı** | **Eş**
--- | --- | ---
**VNET-HUB-EUS2** | 10.240.0.0/20 | VNET-HUB-CUS2, VNET GELİŞTİRME EUS2, VNET-PROD-EUS2
**VNET GELİŞTİRME EUS2** | 10.245.16.0/20 | VNET-HUB-EUS2
**VNET-PROD-EUS2** | 10.245.32.0/20 | VNET-HUB-EUS2, VNET-PROD-CU

![Birincil bölgede merkez/uç](./media/contoso-migration-infrastructure/primary-hub-peer.png) 


#### <a name="subnets-in-the-east-us-2-hub-network-vnet-hub-eus2"></a>Alt ağlar Doğu ABD 2 Hub ağdaki (VNET-HUB-EUS2)

**Alt ağ/bölge** | **CIDR** | ** Kullanılabilir IP adresleri
--- | --- | ---
**IB UntrustZone** | 10.240.0.0/24 | 251
**IB TrustZone** | 10.240.1.0/24 | 251
**OB UntrustZone** | 10.240.2.0/24 | 251
**OB TrustZone** | 10.240.3.0/24 | 251
**GatewaySubnets** | 10.240.10.0/24 | 251


#### <a name="subnets-in-the-east-us-2-dev-network-vnet-dev-eus2"></a>Alt ağlar Doğu ABD 2 geliştirme ağdaki (VNET-DEV-EUS2)

Geliştirme VNet geliştirme ekibi tarafından bir üretim pilot alanı olarak kullanılır. Üç alt ağlar var.

**Alt ağ** | **CIDR** | **Adresleri** | **Alt ağ**
--- | --- | --- | ---
**GELİŞTİRME FE EUS2** | 10.245.16.0/22 | 1019 | Ön uçlar/web katmanı VM'ler
**GELİŞTİRME UYGULAMA EUS2** | 10.245.20.0/22 | 1019 | Uygulama katmanı Vm'leri
**GELİŞTİRME DB EUS2** | 10.245.24.0/23 | 507 | Veritabanı VM'ler


#### <a name="subnets-in-the-east-us-2-production-network-vnet-prod-eus2"></a>Alt ağlar Doğu ABD 2 üretim ağdaki (VNET-PROD-EUS2)

Azure Iaas bileşenlerini üretim ağı içinde yer alır. Her uygulama katmanı kendi alt ağına sahiptir. Alt ağlar, ek etki alanı denetleyicileri için bir alt ağ olarak geliştirme ağ içindeki alanlarla eşleşmesi.

**Alt ağ** | **CIDR** | **Adresleri** | **Alt ağ**
--- | --- | --- | ---
**ÜRÜN FE EUS2** | 10.245.32.0/22 | 1019 | Ön uçlar/web katmanı VM'ler
**ÜRÜN UYGULAMA EUS2** | 10.245.36.0/22 | 1019 | Uygulama katmanı Vm'leri
**ÜRÜN DB EUS2** | 10.245.40.0/23 | 507 | Veritabanı VM'ler
**ÜRÜN DC EUS2** | 10.245.42.0/23 | 251 | Etki alanı denetleyicisi Vm'leri


![Hub'ı ağ mimarisi](./media/contoso-migration-infrastructure/azure-networks-eus2.png)


#### <a name="virtual-networks-in-central-us-secondary-region"></a>Orta ABD (ikincil bölgeye) sanal ağlar

Orta ABD, Contoso'nun ikincil bölge ' dir. Nasıl içindeki Mimarı ağlara bu. aşağıda verilmiştir:

- **Hub**: Doğu ABD 2, Vnet orta noktası kendi şirket içi veri merkezi ve sanal ağlar Doğu ABD 2, gerekli olursa, iş yüklerini yalıtmak için kullanılabilir uç bağlantısı olan bir hub'ı diğer uçlardan ayrı olarak yönetilir.
- **Sanal ağlar**: Orta ABD içindeki iki sanal ağ sahip olur:
    - VNET-PROD-CU. Bu sanal ağ için VNET-PROD_EUS2 benzer bir üretim ağı var. 
    - VNET-ASR-CU. Bu sanal ağ, Azure Vm'leri, birincil düğümden ikincil bölgeye yük devredildi için bir konum, sanal makineleri şirket içi yük devretme sonrasında oluşturulur veya bir konum olarak görür. Bu ağ benzer üretim ağlara, ancak tüm etki alanı denetleyicilerinde bu.
    -  Her sanal ağ bölgesinde kendi adres alanı ile bir çakışma sahip olur. NAT gerek kalmadan yönlendirmeyi yapılandırmak istediğiniz
- **Alt ağlar**: alt ağlar da bu Doğu ABD 2 için benzer bir şekilde tasarlanmış. Bunlar etki alanı denetleyicileri için bir alt ağ gerekmez istisnadır.

Orta ABD içindeki sanal ağlar aşağıdaki tabloda özetlenmiştir.

**Sanal ağ** | **Aralığı** | **Eş**
--- | --- | ---
**VNET-HUB-CU** | 10.250.0.0/20 | VNET-HUB-EUS2, VNET ASR CU, VNET-PROD-CU
**VNET ASR CU** | 10.255.16.0/20 | VNET-HUB-CU, VNET-PROD-CU
**VNET-PROD-CU** | 10.255.32.0/20 | VNET-HUB-CU, VNET ASR CU, VNET-PROD-EUS2  


![Merkez/uç eşleştirilmiş bölgede](./media/contoso-migration-infrastructure/paired-hub-peer.png)


#### <a name="subnets-in-the--central-us-hub-network-vnet-hub-cus"></a>Alt ağlar Orta ABD Hub ağdaki (VNET-HUB-CUS)

**Alt ağ** | **CIDR** | **Kullanılabilir IP adresleri**
--- | --- | ---
**IB UntrustZone** | 10.250.0.0/24 | 251
**IB TrustZone** | 10.250.1.0/24 | 251
**OB UntrustZone** | 10.250.2.0/24 | 251
**OB TrustZone** | 10.250.3.0/24 | 251
**GatewaySubnet** | 10.250.2.0/24 | 251


#### <a name="subnets-in-the-central-us-production-network-vnet-prod-cus"></a>Alt ağlar Orta ABD üretim ağdaki (VNET-PROD-CUS)

Birincil Doğu ABD 2 bölgelerindeki üretim ağı için paralel olarak ikincil Orta ABD bölgesinde bir üretim ağı yok. 

**Alt ağ** | **CIDR** | **Adresleri** | **Alt ağ**
--- | --- | --- | ---
**ÜRÜN FE CU** | 10.255.32.0/22 | 1019 | Ön uçlar/web katmanı VM'ler
**ÜRÜN UYGULAMA CU** | 10.255.36.0/22 | 1019 | Uygulama katmanı Vm'leri
**ÜRÜN DB CU** | 10.255.40.0/23 | 507 | Veritabanı VM'ler
**ÜRÜN DC CU** | 10.255.42.0/24 | 251 | Etki alanı denetleyicisi Vm'leri

#### <a name="subnets-in-the-central-us-failoverrecovery-network-in-central-us-vnet-asr-cus"></a>Orta ABD (VNET-ASR-CUS) Orta ABD yük devretme/kurtarma ağdaki alt ağlar

VNET ASR cu ağ bölgeler arasında yük devretme amacıyla kullanılır. Site Recovery çoğaltıp bölgeleri arasında Azure Vm'leri üzerinde yük devretme için kullanılacak. Bir Contoso veri merkezi için şirket içinde kalır, ancak olağanüstü durum kurtarma için Azure'a yük korunan iş yükleri için Azure ağ olarak da işler.

VNET ASR cu VNET Doğu ABD 2, ancak etki alanı denetleyicisi olan bir alt ağ gerek kalmadan üretim aynı temel alt ağda ' dir.

**Alt ağ** | **CIDR** | **Adresleri** | **Alt ağ**
--- | --- | --- | ---
**ASR FE CU** | 10.255.16.0/22 | 1019 | Ön uçlar/web katmanı VM'ler
**ASR UYGULAMA CU** | 10.255.20.0/22 | 1019 | Uygulama katmanı Vm'leri
**ASR DB CU** | 10.255.24.0/23 | 507 | Veritabanı VM'ler

![Hub'ı ağ mimarisi](./media/contoso-migration-infrastructure/azure-networks-cus.png)

#### <a name="configure-peered-connections"></a>Eşlenen bağlantıları yapılandırma

Her bölgede hub hub bölge içindeki tüm sanal ağları ve başka bir bölgede hub'ına eşlenmesi. Bu hub'ları için iletişim kurmak ve tüm sanal ağları bir bölge içinde görüntülemek için sağlar. Şunlara dikkat edin:

- Eşleme, iki taraflı bir bağlantı oluşturur. İlk VNet üzerinde başlatma eşten bir ve ikinci VNet üzerinde başka bir.
- Karma bir dağıtımda, eşler arasında geçen trafiği şirket içi veri merkeziniz ile Azure arasında VPN bağlantısı görülen gerekir. Bunu etkinleştirmek için eşlenen bağlantılarda ayarlanması gereken bazı özel ayarları vardır.

Tüm bağlantılara için uç sanal ağları hub'ı aracılığıyla şirket içi veri merkezi, Contoso VPN ağ geçitleri iletilen ve çapraz olarak trafiğe izin gerekir.

##### <a name="domain-controller"></a>Etki alanı denetleyicisi

VNET-PROD-EUS2 ağdaki etki alanı denetleyicilerinin, Contoso EUS2 hub/üretim ağı arasında hem şirket içi VPN bağlantısı üzerinden akışına istiyor. Bunu yapmak için aşağıdaki izin gerekir:

1. **İletilen trafiğe izin ver** ve **ağ geçidi geçişi yapılandırmaları izin** eşlenmiş bağlantı. Bizim örneğimizde bunu VNET-HUB-EUS2 VNET PROD EUS2 bağlantısı olacaktır.

    ![Eşleme](./media/contoso-migration-infrastructure/peering1.png)

2. **İletilen trafiğe izin verecek** ve **uzak ağ geçitlerini kullan** diğer tarafında, VNET-PROD-EUS2 VNET HUB EUS2 bağlantı üzerinde eşlemesi.

    ![Eşleme](./media/contoso-migration-infrastructure/peering2.png)

3. Bunlar sanal ağ VPN tüneli üzerinden yönlendirmek için yerel trafiği yönlendiren bir statik rota ayarlayacaksınız şirket içi. Yapılandırma Contoso Azure'a VPN tüneli sağlayan ağ geçidinde tamamlanacaktır. Contoso, Windows Yönlendirme ve Uzaktan erişim için bunu kullanır.

    ![Eşleme](./media/contoso-migration-infrastructure/peering3.png)

##### <a name="production-networks"></a>Üretim ağları 

Hub'ı aracılığıyla başka bir bölgede spoked eş ağ spoked eş ağ göremez.

Birbirine görmek için her iki bölgede de Contoso'nun üretim ağları için bunlar VNET PROD EUS2 ve HAVALANDIRMA PROD cu için doğrudan eşlenmiş bir bağlantı oluşturmanız gerekir. 

![Eşleme](./media/contoso-migration-infrastructure/peering4.png)

### <a name="set-up-dns"></a>DNS ayarlamalısınız

Sanal ağlarda bulunan kaynaklar dağıtırken, birkaç etki alanı adı çözümlemesi için seçenek vardır. Azure tarafından sağlanan ad çözümlemesini kullanmasına veya DNS sunucuları için çözüm sağlar. Kullandığınız ad çözümlemesi türünü nasıl kaynaklarınızı birbirleri ile iletişim kurmak gereken üzerinde bağlıdır. Alma [daha fazla bilgi](https://docs.microsoft.com/azure/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances#azure-provided-name-resolution) Azure DNS hizmeti hakkında.

Contoso karar Azure DNS hizmeti, karma ortamda iyi bir seçim değildir. Bunun yerine, şirket içi DNS sunucularından yararlanmak için ilerlediklerini.

- Bu bir hibrit ağ tüm VM'ler şirket içinde olduğundan ve Azure düzgün çalışması için adları çözümleyebilmesi gerekir. Başka bir deyişle, özel DNS ayarları için tüm Vnet'lerin uygulanmalıdır.
- Contoso şu anda Contoso veri merkezi ve şube ofislerindeki dağıtılan Dc'lerine sahiptir. Birincil DNS sunucularından CONTOSODC1(172.16.0.10) ve CONTOSODC2(172.16.0.1) olan
- Sanal ağlar dağıtıldığında, şirket içi etki alanı denetleyicileri ağlarında DNS sunucusu olarak kullanılmak üzere ayarlanır. 
- Bu, özel DNS VNet üzerinde kullanırken yapılandırmak için Azure'nın yinelemeli Çözümleyicileri IP adresi (168.63.129.16 gibi) DNS listesine eklenmesi gerekir.  Bunu yapmak için her VNet üzerinde Contoso DNS sunucusu ayarlarını yapılandırır. Örneğin, bir VNET HUB EUS2 ağda özel DNS ayarlarını şu şekilde olacaktır:
    
    ![Özel DNS](./media/contoso-migration-infrastructure/custom-dns.png)

Şirket içi etki alanı denetleyicilerini yanı sıra, Contoso seçeceğiz dört daha fazla Azure ağlarının, her bölge için iki destekleyecek şekilde uygulamak için. İşte bunların Azure'da ne dağıtacaksınız.

**Bölge** | **DC** | **Sanal ağ** | **Alt ağ** | **IP adresi**
--- | --- | --- | --- | ---
EUS2 | CONTOSODC3 | VNET-PROD-EUS2 | ÜRÜN DC EUS2 | 10.245.42.4
EUS2 | CONTOSODC4 | VNET-PROD-EUS2 | ÜRÜN DC EUS2 | 10.245.42.5
CU | CONTOSODC5 | VNET-PROD-CU | ÜRÜN DC CU | 10.255.42.4
CU | CONTOSODC6 | VNET-PROD-CU | ÜRÜN DC CU | 10.255.42.4

Şirket içi etki alanı denetleyicileri dağıttıktan sonra Contoso yeni etki alanı denetleyicileri, DNS sunucusu listesinde içerecek şekilde ağlardaki iki bölgesindeki DNS ayarlarının güncelleştirilmesi gerekiyor.



#### <a name="set-up-domain-controllers-in-azure"></a>Azure'daki etki alanı denetleyicileri ayarlama

Ağ ayarlarını güncelleştirdikten sonra Contoso etki alanı denetleyicilerini azure'da oluşturmak hazır olur.

1. Azure portalında bunlar uygun sanal ağa yeni bir Windows Server sanal makine dağıtın.
2. Bunlar, sanal makine için her bir konumdaki kullanılabilirlik kümeleri oluşturun. Kullanılabilirlik kümeleri, aşağıdakileri yapın:
    - Azure dokusunu Azure bölgesindeki farklı altyapıları ile Vm'leri ayıran emin olun. 
    -  Contoso, azure'da sanal makineler için % 99,95 oranında SLA'sı için uygun olmasını sağlar.  [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets).

    ![Kullanılabilirlik grubu](./media/contoso-migration-infrastructure/availability-group.png) 
3. VM dağıtıldıktan sonra sanal makine için ağ arabirimi kalem. Burada, bunlar özel IP'si ayarlanan adresi statik olarak ve geçerli bir adres belirtin.

    ![VM NIC](./media/contoso-migration-infrastructure/vm-nic.png)

4. Şimdi, yeni bir veri diski VM'e ekleyin. Bu disk, Active Directory veritabanını ve sysvol paylaşımının içerir. 
    - Disk boyutu destekliyorsa IOPS sayısını belirler.
    - Zaman içindeki disk boyutu, ortam büyüdükçe artırmanız gerekebilir.
    - Sürücü okuma/yazma için konak önbelleği ayarlanmamalıdır. Active Directory veritabanları bu desteklemez.

     ![Active Directory disk](./media/contoso-migration-infrastructure/ad-disk.png)

5. Disk eklendikten sonra bunlar VM'ye Uzak Masaüstü üzerinden bağlanmak ve Sunucu Yöneticisi'ni açın.
6. Ardından **dosya ve depolama hizmetleri**, sürücü harfi F: verilen veya yukarıda olduğundan emin olmanın Yeni Birim Sihirbazı çalıştırdığı yerel sanal makine üzerinde.

     ![Yeni Birim Sihirbazı](./media/contoso-migration-infrastructure/volume-wizard.png)

7. Sunucu Yöneticisi'nde ekledikleri **Active Directory Domain Services** rol. Daha sonra VM'yi bir etki alanı denetleyicisi olarak yapılandırın.

      ![Sunucu rolü](./media/contoso-migration-infrastructure/server-role.png)  

9. Sanal DC olarak yapılandırılır ve yeniden sonra DNS Yöneticisi'ni açın ve Azure DNS Çözümleyicisi iletici olarak yapılandırın.  Bu, DNS sorgularının Azure DNS'ye çözümlenemiyor iletmek etki alanı denetleyicisi sağlar.

    ![DNS ileticisi](./media/contoso-migration-infrastructure/dns-forwarder.png)

10. Şimdi, VNet bölgesi için uygun etki alanı denetleyicisiyle sonra her sanal ağ özel DNS ayarlarını güncelleştirin. Şirket içi DC'leri listesinde içerirler.

### <a name="set-up-active-directory"></a>Active Directory'yi ayarlama

AD, bir kritik ağ hizmetidir ve doğru şekilde yapılandırılması gerekir. Contoso AD siteleri Contoso veri merkezi ve EUS2 ve cu bölgeleri için oluşturacaksınız.  

1. Bunlar, iki yeni site (EUS2 AZURE ve AZURE CUS) veri merkezinde sitenin (ContosoDatacenter) birlikte oluşturur.
2. Siteleri oluşturduktan sonra siteleri sanal ağlar ve veri merkezi eşleştirmek için alt ağlar oluşturun.

    ![DC alt ağlar](./media/contoso-migration-infrastructure/dc-subnets.png)

3. Ardından, her şeyi bağlanmak için iki site bağlantı oluşturun. Etki alanı denetleyicileri sonra konumlarına taşınmasını gerektirir.

    ![DC bağlantıları](./media/contoso-migration-infrastructure/dc-links.png)

4. Her şeyi yapılandırdıktan sonra Active Directory çoğaltma topolojisi yerdir.
    
    ![DC çoğaltma](./media/contoso-migration-infrastructure/ad-resolution.png)

5. Her şeyi tam bir listesi etki alanı denetleyicilerini ve sitelerini gösterilir şirket içi Active Directory Yönetim Merkezi'nde.

    ![AD Yönetim Merkezi](./media/contoso-migration-infrastructure/ad-center.png)

## <a name="step-5-plan-for-governance"></a>5. adım: Yönetim için planlama

Azure Hizmetleri ve Azure platformu üzerinde bir dizi idare denetimleri sağlar. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/security/governance-in-azure) seçenekleri hakkında bilgi için.

Yapılandırma kimlik ve erişim denetimi gibi yönetim ve güvenlik bazı yönlerini bir yere koymak Contoso zaten başladı. Genel anlamıyla bunlar göz önünde bulundurmanız gereken üç alan vardır:

- **İlke**: Azure'da İlkesi uygular ve kaynakları Kurumsal gereksinimleri ve SLA'lar ile uyumlu kalmasını sağlayın, kaynaklarınız üzerinden kuralları ve etkileri zorlar.
- **Kilitler**: Azure sayesinde kilit abonelikler, kaynak grupları ve diğer kaynaklar, böylece bunu yapmak için yetkiye sahip olanları tarafından yalnızca değiştirilebilir.
- **Etiketleri**: kaynak denetimli, denetlenmiş ve etiketleri ile yönetilen. Sahipleri veya kaynaklar hakkında bilgi sağlayan kaynaklarına meta veri etiketleri ekleyin.

### <a name="set-up-policies"></a>İlkeleri ayarlama

Azure İlkesi hizmetini prosedürleriniz var ilke tanımlarıyla uyumsuz olanlar için tarama kaynaklarınızı değerlendirir. Örneğin, yalnızca belirli türlerdeki Vm'leri sağlar veya belirli bir etikete sahip kaynakları gerektiren bir ilke olabilir. 

Azure ilkeleri bir ilke tanımı ve ilke ataması, bir ilke uygulanması gereken kapsam belirtin. Kapsam, bir kaynak grubu için bir yönetim grubundan değişebilir. [Bilgi](https://docs.microsoft.com/azure/azure-policy/create-manage-policy) ilke oluşturma ve yönetme hakkında.

Contoso istediğiniz ilkelerin birkaç ile kullanmaya başlamak:

- Kaynakları yalnızca EUS2 ve cu bölgelerinde dağıtılabilir emin olmak için bir ilke isterler.
- Bunlar yalnızca onaylanan SKU'lara VM SKU'ları sınırlamak istiyorsunuz. Amaç, pahalı VM SKU'ları kullanılmayan sağlamaktır.

#### <a name="limit-resources-to-regions"></a>Bölgelere sınırı kaynakları

Contoso kullanan yerleşik ilke tanımı **izin verilen Konumlar** kaynak bölgede sınırlamak için.

1. Azure portalında **tüm hizmetleri**, araması **ilke**.
2. Seçin **atamaları** > **atama İlkesi**.
3. İlke listesinde **izin verilen Konumlar**.
4. Ayarlama **kapsam** adına Azure aboneliği ve izin verilen listesinde iki bölgeleri seçin.

    ![Bölgeleri izin ilkesi](./media/contoso-migration-infrastructure/policy-region.png)

5. Varsayılan olarak, ilke ile ayarlanmışsa **Reddet**, biri olması durumunda anlamına başlatıldığında bir dağıtım EUS2 veya cu olmayan aboneliği, dağıtım başarısız olur. Batı ABD bölgesinde bir dağıtımı ayarlamak üzere birisi Contoso aboneliği çalışırsa, şunlar olur.

    ![İlke başarısız oldu](./media/contoso-migration-infrastructure/policy-failed.png)

#### <a name="allow-specific-vm-skus"></a>Belirli VM SKU'ları izin ver

Contoso, yerleşik ilke tanımı kullanacağı **sanal makinelerin SKU'ları** abonelikte oluşturulabilecek Vm'leri türlerini sınırlamak için. 

![İlke SKU](./media/contoso-migration-infrastructure/policy-sku.png)



#### <a name="check-policy-compliance"></a>İlke uyumluluğunu Denetle

İlkeleri hemen yürürlüğe girer ve Contoso uyumluluk için kaynakları denetleyebilirsiniz. 

1. Azure portalında **Uyumluluk** bağlantı.
2. Uyumluluk panosu açılır. Daha fazla ayrıntı için detaya gidebilirsiniz.

    ![İlke uyumluluğu](./media/contoso-migration-infrastructure/policy-compliance.png)


### <a name="set-up-locks"></a>Kilitli ayarlayın

Contoso sistemlerini yönetimi için uzun ITIL framework kullanıyor. Framework'ün en önemli yönlerinden denetim değiştirmektir ve Contoso denetimini Değiştir kendi Azure dağıtımında uygulandığından emin olmak ister.

Contoso, kilitler gibi uygulamak için geçiyor:

- Herhangi bir üretim veya yük devretme bileşeni salt okunur kilidi olan bir kaynak grubunda olmalıdır.  Bu, değiştirme veya üretim öğelerini silmek için kilidin kaldırılması gerektiğini anlamına gelir. 
- Üretim dışı kaynak grupları CanNotDelete kilitleri sahip olur. Yetkili kullanıcıların anlamına gelir okuyun veya bir kaynağı değiştirme, ancak bunu silemezsiniz.

[Daha fazla bilgi edinin](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-lock-resources) kilitleri hakkında.

### <a name="set-up-tagging"></a>Etiketleme ayarlama

Eklenen kaynakları izlemek için kaynakları uygun departmanı, müşteri ve ortam ile ilişkilendirmek üzere Contoso için giderek daha çok önemli olacaktır. 

Bilgi kaynakları ve sahipleri hakkında sağlamaya ek olarak, etiketleri Contoso toplama ve Grup kaynaklarına ve bu verileri geri ödeme amacıyla kullanılmak üzere etkinleştirir.

Contoso kendi Azure varlıkları, iş için anlamlı bir şekilde görselleştirmeniz gerektiğinde. Örnek, ancak rol veya departman için. Kaynakların bir etiketi paylaşması için aynı kaynak grubunda bulunmaları gerekmez unutmayın. Bunu yapmak için Contoso böylece herkesin aynı etiketleri kullanarak basit etiket sınıflandırmanızı oluşturacaksınız.

**Etiket adı** | **Değer**
--- | ---
Maliyet merkezi | 12345: SAP geçerli maliyet merkezi olması gerekir.
Departmanı | İş birimi (SAP'den) adı. CostCenter eşleşir.
ApplicationTeam | Uygulama için destek sahibi olan takım e-posta diğer adı.
Katalog adı | Kaynağın desteklediği hizmet Kataloğu başına ShareServices, ve uygulama adıdır.
ServiceManager | E-posta diğer adı için kaynak ITIL Service Manager'ın.
COBPriority | Öncelikli iş BCDR için ayarlayın. Değerler 1-5.
ENV | Olası değerler DEV, STG, ürün var. Geliştirme, hazırlama ve üretim temsil eden.

Örneğin: 

 ![Azure etiketleri](./media/contoso-migration-infrastructure/azure-tag.png)

Etiket oluşturduktan sonra Contoso geri dönün ve yeni Azure ilke tanımları ve atamaları kuruluş genelinde kullanılmasını gerekli etiketleri uygulamak oluşturun.


## <a name="step-6-consider-security"></a>6. adım: güvenlik göz önünde bulundurun.

Bulutta güvenlik önemlidir ve Azure, çok sayıda güvenlik araçları ve özellikleri sağlar. Bu güvenli bir Azure platformu üzerinde güvenli çözümler oluşturmanıza yardımcı olur. Okuma [güvenilir bulutta emin ellerde](https://azure.microsoft.com/overview/trusted-cloud/) Azure güvenliği hakkında daha fazla bilgi edinmek için.

Orada contoso dikkate alınması gereken birkaç yönü:

- **Azure Güvenlik Merkezi**: Azure Güvenlik Merkezi, hibrit bulut iş yüklerinde Birleşik güvenlik yönetimi ve Gelişmiş tehdit koruması sağlar. Güvenlik Merkezi ile, iş yüklerinize güvenlik ilkeleri uygulayabilir, tehditlere maruz kalma riskinizi sınırlayabilir ve saldırıları algılayıp onlara yanıt verebilirsiniz.  [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/security-center/security-center-intro).
- **Ağ güvenlik grupları (Nsg'ler)**: bir NSG olduğundan, güvenlik listesini içeren bir filtre (Güvenlik Duvarı) kuralları, uygulandığında, izin vermek veya Azure sanal ağlarına bağlı kaynaklara ağ trafiğini engelle. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/virtual-network/security-overview).
- **Veri şifreleme**: Azure Disk şifrelemesi, Windows ve Linux Iaas sanal makine disklerinizi şifreleyin yardımcı olan bir özellik olan. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/security/azure-security-encryption-atrest).

### <a name="work-with-the-azure-security-center"></a>Azure Güvenlik Merkezi ile çalışma

Contoso için yeni hibrit Bulut ve özel Azure iş yüklerini güvenlik durumunu hızlıca görüntülemenizi arıyor.  Sonuç olarak, Contoso Azure Güvenlik Merkezi'ni uygulamak aşağıdaki özelliklerle başlangıç verdi: 

- Merkezi ilke yönetimi
- Sürekli değerlendirme
- Eyleme dönüştürülebilir öneriler 

#### <a name="centralize-policy-management"></a>İlke yönetimini merkezden gerçekleştirin

Merkezi ilke yönetimi ile tüm ortamları boyunca güvenlik ilkelerini merkezi olarak yöneterek güvenlik gereksinimleriyle uyumluluk Contoso sağlayacaktır. Bunlar basit ve hızlı tüm Azure kaynakları için geçerli bir ilke uygulayabilir.

![Güvenlik ilkesi](./media/contoso-migration-infrastructure/security-policy.png)

#### <a name="assess-and-action"></a>Değerlendirmek ve eylem

Contoso makineler, ağlar, depolama, veri ve uygulama güvenliğini izleyen sürekli güvenlik değerlendirmesi özelliğinden yararlanır; olası güvenlik sorunlarını keşfetmek için. 

- Güvenlik Merkezi, Contoso'nun işlem, altyapı ve veri kaynakları ve Azure uygulamaları ve Hizmetleri güvenlik durumunu analiz eder.
- Sürekli değerlendirme veya ağ bağlantı noktalarını kullanıma sunulan güvenlik güncelleştirmeleri eksik olan sistemler gibi olası güvenlik sorunlarını keşfetmek için Contoso operasyon ekibinin'yardımcı olur. 
- Özellikle Contoso tüm Vm'leri korumalı emin olmak ister. Güvenlik Merkezi bu ile VM sistem durumu doğrulama ve güvenlik açıklarını kötüye önce düzeltmek için önceliği belirlenmiş ve eyleme dönüştürülebilir öneriler yapmak yardımcı olur.

![İzleme](./media/contoso-migration-infrastructure/monitoring.png)

### <a name="work-with-nsgs"></a>Nsg'ler ile çalışma

Contoso, ağ trafiğini ağ güvenlik gruplarını kullanarak bir sanal ağ içindeki kaynaklarla sınırlayabilirsiniz.

- Bir ağ güvenlik grubunda gelen veya giden ağ trafiğine kaynak ya da hedef IP adresi, bağlantı noktası ve protokole göre izin veren veya bu trafiği reddeden güvenlik kurallarından oluşan bir liste bulunur.
- Bir alt ağa uygulanan kuralları alt ağdaki tüm kaynaklara uygulanır. Ağ arabirimlerine ek olarak, bu alt ağda dağıtılan Azure hizmetlerinin örnekleri içerir.
- Uygulama güvenlik grupları (Asg'ler) ağ güvenliğini Grup Vm'lere sağlayan bir uygulama yapısının doğal bir uzantısı olarak yapılandırmanıza ve ağ güvenlik ilkelerini bu gruplara göre tanımlamanızı sağlar.
    - Uygulama güvenlik grupları, güvenlik ilkeniz uygun ölçekte, açık IP adreslerinin bakımını el ile olmadan yeniden kullanabileceğiniz anlamına gelir. Platform açık IP adreslerinin ve birden fazla kural kümesinin karmaşık süreçlerini üstlenerek iş mantığınıza odaklanmanızı sağlar.
    - Bir uygulama güvenlik grubunu bir güvenlik kuralında kaynak ve hedef olarak belirtebilirsiniz. Güvenlik ilkeniz tanımlandıktan sonra sanal makineler oluşturmak ve VM NIC bir gruba atayın. 


Contoso bir karışımını Nsg'leri ve Asg'leri uygular. Bunlar, NSG yönetimi konusunda endişe. Bunlar, Nsg'ler, operasyon personeli için gelebilir karmaşıklığı ve aşırı kullanımı hakkında endişeleniyoruz.  Bunu aklınızda, bunlar genel bir kural kullandıkları iki anahtar ilkeleri benimseyen:

- İçine ve dışına tüm alt ağlar (Kuzey-Güney), tüm trafik, Hub ağlardaki GatewaySubnets dışında bir NSG kuralı tabi olacaktır.
- Herhangi bir güvenlik duvarı veya etki alanı denetleyicisi Nsg'leri alt ağ ve NIC Nsg'ler tarafından korunur.
- Tüm üretim uygulamaları, uygulanan Asg'ler sahip olur.


Contoso, bu müşterilerin uygulamaları için nasıl görüneceğini bir modeli sunmuştur.

![Güvenlik](./media/contoso-migration-infrastructure/asg.png)


Asg'ler ile ilişkili Nsg'ler paketler yalnızca izin verilen bir ağın parçası hedefine akış emin olmak için en az ayrıcalık ile yapılandırılır.

**Eylem** | **Ad** | **Kaynak** | **Hedef** | **Bağlantı Noktası**
--- | --- | --- | --- | --- 
İzin Ver | AllowiInternetToFE | VNET-HUB-EUS1/IB-TrustZone | APP1-FE 80, 443
İzin Ver | AllowWebToApp | APP1-FE | APP1-DB | 1433
İzin Ver | AllowAppToDB | APP1 UYGULAMA | Herhangi biri | Herhangi biri
Reddet | DenyAllInbound | Herhangi biri | Herhangi biri | Herhangi biri

### <a name="encrypt-data"></a>Verileri şifreleme

Azure Disk şifrelemesi, Denetim yardımcı olmak ve disk şifreleme anahtarlarını ve gizli bir key vault aboneliği yönetmek için Azure Key Vault ile tümleştirilir. Bu, sanal makine disklerindeki tüm veriler Azure depolama alanındaki bekleyen şifrelenmesini sağlar.  

- Contoso, belirli sanal makineler şifrelemesi belirledi.
- Müşteri, gizli, vm'lere şifrelemeyle veya PPI veri geçerli olacak.


## <a name="conclusion"></a>Sonuç

Bu makalede, bir Azure altyapısı ve Azure aboneliğinin İlkesi ayarlama Contoso karma tanımlayın, olağanüstü durum kurtarma, ağ, yönetim ve güvenlik. 

Burada, Contoso tamamlanan adımların tümü, buluta geçiş için gerekli değildir. Kendi durumda, bunlar tüm türleri geçişleri için kullanılabilir bir ağ altyapısı planlama istiyordu ve güvenli, dayanıklı ve ölçeklenebilir. 

Bu altyapı yerinde bunlar ilerleyelim ve geçiş işlemini deneyin hazırsınız demektir.

## <a name="next-steps"></a>Sonraki adımlar

Contoso kolaylaştıracağını ilk geçiş senaryosu [Azure'a geçiş için şirket içi SmartHotel iki katmanlı uygulama değerlendirmek](contoso-migration-assessment.md). 
