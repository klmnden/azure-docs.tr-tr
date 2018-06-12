---
title: Contoso-geçiş altyapı ayarlama | Microsoft Docs
description: Contoso geçiş Azure için Azure altyapısının nasıl ayarlar hakkında bilgi edinin.
services: azure-migrate
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 06/11/2018
ms.author: raynew
ms.openlocfilehash: 8b7f0675c1bbf378d02eb52843caf27a1dce2fb8
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35301210"
---
# <a name="contoso---deploy-a-migration-infrastructure"></a>Contoso - geçiş altyapısı dağıtın

Bu makalede nasıl bir şirket içi yedekleme Contoso ayarlar inceler ve hazırlık geçiş Azure için ve iş karma bir ortamda çalıştırmak için Azure altyapı.

- Contoso için belirli bir örnek mimarisidir.
- Makalede açıklanan tüm öğeleri gerekip gerekmediğini geçiş stratejinizi bağlıdır. Örneğin, yalnızca Azure bulut yerel uygulamalar oluşturuyorsanız, daha az karmaşık ağ yapısı gerekebilir.

Bu belge, nasıl geçirir Contoso adlı kurgusal şirket Microsoft Azure bulut kaynaklarına şirket içi belge makaleleri dizi saniyedir. Seri arka plan bilgileri içerir ve geçiş altyapısını kurma verilmektedir dağıtım senaryoları birtakım geçiş için şirket içi kaynaklar uygunluğunu değerlendirmek ve devre dışı geçişler farklı türlerde çalıştırın. Karmaşık senaryolar büyümesine ve biz diğer makaleler zamanla eklenmesi.

**Makale** | **Ayrıntılar** | **Durum**
--- | --- | ---
[Makale 1: genel bakış](contoso-migration-overview.md) | Contoso'nun geçiş stratejisi, makale serisi ve kullanırız örnek uygulamaları genel bir bakış sağlar. | Kullanılabilir
Makale 2: Azure altyapısının (Bu makalede) dağıtma | Nasıl kendi şirket içi ve Azure altyapı Contoso bu geçiş için hazırlar açıklar. Aynı alt tüm Contoso geçiş senaryoları için kullanılır. | Kullanılabilir
[Makale 3: şirket içi kaynakları değerlendirin](contoso-migration-assessment.md) | Contoso VMware üzerinden çalışan kendi şirket içi iki katmanlı SmartHotel uygulamasının bir değerlendirme nasıl çalıştığı gösterilmektedir. Uygulama VM'ler ile değerlendirmek [Azure geçirmek](migrate-overview.md) hizmet ve uygulama SQL Server veritabanı ile [Azure veritabanı geçiş Yardımcısı](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Kullanılabilir
[Makale 4: Rehost Azure VM'ler ve SQL yönetilen örneği](contoso-migration-rehost-vm-sql-managed-instance.md) | Contoso SmartHotel uygulamayı Azure'a nasıl geçirir gösterir. VM ön uç uygulamasını kullanarak geçirmek [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview), uygulamayı ve veritabanı kullanma [Azure veritabanı geçiş](https://docs.microsoft.com/azure/dms/dms-overview) yönetilen bir SQL örneğine geçirmek için hizmet. | Kullanılabilir
[Makale 5: Azure VM'ler için yeniden barındırma](contoso-migration-rehost-vm.md) | Contoso SmartHotel uygulama yalnızca Site Recovery kullanarak sanal makineleri geçirmek nasıl gösterir.
[Makale 6: Azure VM'ler ve SQL Server kullanılabilirlik gruplarını yeniden barındırma](contoso-migration-rehost-vm-sql-ag.md) | Contoso SmartHotel uygulama nasıl geçirir gösterir. Sanal makineleri uygulama ve veritabanı geçiş hizmeti uygulama veritabanı için SQL Server kullanılabilirlik grubu geçirmek için geçirmek için Site Recovery kullanırlar. | Kullanılabilir
[Makale 7: Azure VM'ler için Linux uygulama yeniden barındırma](contoso-migration-rehost-linux-vm.md) | Contoso Linux osTicket uygulama Azure Vm'leri için nasıl geçirir gösterir. | Kullanılabilir
[Makale 8: Linux uygulama Azure VM'ler ve Azure MySQL sunucusu için yeniden barındırma](contoso-migration-rehost-linux-vm-mysql.md) | Contoso geçirmek için Site Recovery ve MySQL çalışma ekranı kullanarak Linux osTicket uygulama (yedekleme ve geri yükleme) Azure MySQL Server örneğine nasıl geçirir gösterir. | Kullanılabilir

Bu makaledeki tüm altyapı öğeleri Contoso ayarlamak geçiş senaryolarını tamamlamak ihtiyaç duyar. 


## <a name="overview"></a>Genel Bakış

Azure'a geçirebilmeniz için önce Contoso altyapılarını hazırlama önemlidir.  Genellikle, dikkat etmeniz gereken beş geniş alan vardır:

1. **Azure abonelikleri**: ne bunlar Azure satın alma ve Azure platformu ve Hizmetleri ile etkileşim?
2. **Karma kimlik**: nasıl bunlar yönetmek ve denetlemek için şirket içi ve Azure kaynaklarına erişim geçişten sonra? Nasıl bunlar genişletmek veya kimlik yönetimi buluta taşımak?
3. **Olağanüstü durum kurtarma ve esnekliği**: nasıl bunlar olmasını kesintiler ve olağanüstü durumları ortaya çıkarsa, uygulamalar ve altyapı esnek mi?
4. **Ağ**: nasıl, ağ altyapınızın tasarım ve kendi şirket içi veri merkezi ve Azure arasında bağlantı kurmak?
5. **Güvenlik ve idare**: nasıl, karma/Azure dağıtımınızın güvenliğini ve güvenlik ve idare gereksinimleri ile hizalı tutmak?

## <a name="before-you-start"></a>Başlamadan önce

Altyapısının arayan başlamadan önce bu makalede aşağıdakiler ele Azure özellikleri hakkında bazı bilgiler okumak isteyebilirsiniz:

- Açık Lisans Microsoft satıcılar veya Microsoft Partners bilmeniz bulut çözüm sağlayıcıları (CSP'ler) veya Kullandıkça Öde, Kurumsal Anlaşma (Kurumsal Sözleşme) dahil olmak üzere Azure erişim satın almak için kullanılabilir bir dizi seçeneğiniz vardır. Hakkında bilgi edinin [satın alma seçeneği](https://azure.microsoft.com/pricing/purchase-options/), hakkında okuyup [Azure abonelikleri düzenlenir](https://azure.microsoft.com/blog/organizing-subscriptions-and-resource-groups-within-the-enterprise/).
- Azure genel bir bakış elde [kimlik ve erişim yönetimi](https://www.microsoft.com/en-us/trustcenter/security/identity). Özellikle, öğrenin [Azure AD ve genişletme şirket içi AD buluta](https://docs.microsoft.com/azure/active-directory/identity-fundamentals). Hakkında yararlı indirilebilir e-kitap yoktur [kimlik ve erişim yönetimi (IAM) karma bir ortamda](https://azure.microsoft.com/resources/hybrid-cloud-identity/).
- Azure karma bağlantı için seçeneklerle sağlam bir ağ altyapısı sağlar. Bir bakış elde [ağ ve ağ erişim denetimi](https://docs.microsoft.com/azure/security/security-network-overview).
- Giriş almak [Azure güvenlik](https://docs.microsoft.com/azure/security/azure-security), okuyup yönelik bir plan oluşturma hakkında [idare](https://docs.microsoft.com/azure/security/governance-in-azure).


## <a name="on-premises-architecture"></a>Şirket içi mimarisi

Geçerli Contoso şirket içi altyapı gösteren bir diyagram aşağıdadır.

 ![Contoso mimarisi](./media/contoso-migration-infrastructure/contoso-architecture.png)  

- Contoso içinde Şehir, New York, Doğu Amerika Birleşik Devletleri bulunan bir ana veri merkezi vardır.
- Amerika Birleşik Devletleri arasında üç ek yerel dalları sahiptirler.
- Ana veri merkezi bir fiber metro ethernet bağlantısı (500 MB/sn) ile Internet'e bağlı.
- Her dal Internet'e IPSec VPN tünelleri dön ana veri merkezi ile iş sınıfı bağlantıları kullanarak yerel olarak bağlı. Böylece, kalıcı olarak bağlı olması, tüm ağ ve internet bağlantısı en iyi duruma getirir.
- Ana veri merkezi, VMware ile tam olarak sanallaştırılmış. VCenter Server 6.5 tarafından yönetilen iki ESXi 6.5 sanallaştırma ana bilgisayarı sahiptirler.
- Contoso Active Directory kimlik yönetimi ve iç ağdaki DNS sunucuları için kullanır.
- Etki alanı denetleyicileri veri merkezindeki VMware VM'ler üzerinde çalıştırın. Yerel dalları etki alanı denetleyicilerde fiziksel sunucularda çalıştırın.


## <a name="step-1-buy-and-subscribe-to-azure"></a>1. adım: Satın almak ve Azure'a abone olma

Azure satın alma, nasıl abonelikleri mimari ve hizmetlerinizi ve kaynaklarınızı lisans nasıl bağlayacağınızı contoso gerekir.

### <a name="buy-azure"></a>Azure satın alma

Contoso ile giderek bir [Kurumsal Anlaşma (Kurumsal Sözleşme)](https://azure.microsoft.com/pricing/enterprise-agreement/). Bu, bunları esnek faturalama seçenekleri de dahil olmak üzere çok yarar kazanmak için entitling ve fiyatlandırma en iyi duruma getirilmiş Azure için önden parasal taahhütte kapsar.

- Contoso tahmini ne Azure harcamanız kendi yıllık olacaktır. Bunlar anlaşmayı imzalandığında tam ilk yıl için ödeme.
- Contoso tüm kullanıcıların taahhüt üzerinden yıl de ya da bu dolar değeri kaybedersiniz önce kullanması gerekir.
- Herhangi bir nedenle bunlar kendi taahhüt aşan ve daha fazla harcamanız, Microsoft bunları için farkını fatura.
- Taahhüt sonucunda oluşan herhangi bir maliyeti aynı hızları ve bunların sözleşme de olacaktır. Gitmek için hiçbir cezaları vardır.

### <a name="manage-subscriptions"></a>Abonelikleri yönetme

Azure için ödeme sonra Contoso gerekir kendi Aboneliklerini yönetmek nasıl bağlayacağınızı. Bir EA sahiptir ve bu nedenle sınır Azure abonelikleri sayısı bunlar ayarlayabilirsiniz.

- Bir Azure işletme kaydı bir şirket şekli nasıl tanımlar ve Azure hizmetlerini kullanır ve çekirdek idare yapısını tanımlar.
- İlk adım olarak, Contoso belirlenen bir yapı (kendi Kurumsal kayıt için Kurumsal iskele bilinir. Kullandıkları [bu makalede](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-subscription-governance) ve iskele tasarım anlamanız yardımcı olmak için.
- Şimdilik, Contoso kendi Aboneliklerini yönetmek için işlevsel bir yaklaşım kullanmaya karar vermiştir.
    - Kuruluşun Azure bütçe denetimleri tek bir BT departmanı sahip olacaksınız. Bu abonelikleri yalnızca grubuyla olacaktır.
    - Kurumsal diğer grupları Kurumsal kayıt departmanları olarak katılabilirsiniz için bunlar Bu model gelecekte genişletmeniz.
    - BT departmanı içinde Contoso iki Abonelikleri, üretim ve geliştirme yapılandırılmış.
    - Contoso ek abonelikleri gelecekte gerektiriyorsa, access, ilkeleri ve bu abonelik için uyumluluğu yönetmek gerekir. Bunlar sunarak bunun kullanabileceksiniz [Azure Yönetim grupları](https://docs.microsoft.com/azure/azure-resource-manager/management-groups-overview), abonelikleri üstünde ek katmanı olarak.

    ![Kuruluş yapısı](./media/contoso-migration-infrastructure/enterprise-structure.png) 

### <a name="examine-licensing"></a>Lisans inceleyin

Yapılandırılmış aboneliklerle kendi Microsoft Lisans'ı Contoso bakabilirsiniz. Kendi lisans stratejisi, bunlar Azure ve nasıl Azure VM'ler ve hizmetler seçili dağıtılan ve geçirmek istediğiniz kaynakları bağlıdır. 

#### <a name="azure-hybrid-benefit"></a>Azure Hibrit Avantajı

Azure'da VM dağıtırken standart görüntüleri Contoso tarafından kullanılan yazılım dakikadır ücretinin bir lisans içerir. Ancak, Contoso uzun vadeli bir Microsoft Müşteri olmuştur ve EAs tutulan ve lisansları Yazılım Güvencesi (SA) açın. 

Azure karma avantajı, Azure Vm'leri ve SQL Server iş yükleri dönüştürme ya da Windows Server Datacenter ve Standard edition lisansları Yazılım Güvencesi ile kapsamdaki yeniden kaydetmek vererek Contoso geçiş için uygun maliyetli bir yöntem sağlar. Bu, daha düşük bir tabanlı işlem oranı VM'ler ve SQL Server için ödeme Contoso olanak tanır. [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/hybrid-benefit/).


#### <a name="license-mobility"></a>Lisans Taşınabilirliği

SA ile lisans taşınabilirliği Microsoft Toplu Lisans müşterilerine Contoso gibi Azure üzerinde etkin SA ile uygun sunucu uygulamaları dağıtma esnekliği sağlar. Bu, yeni lisans satın gereğini ortadan kaldırır. Hiçbir ilişkili mobility ücretleri ile var olan lisans kolayca Azure'da dağıtılabilir. [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/license-mobility/).

#### <a name="reserve-instances-for-predictable-workloads"></a>Tahmin edilebilir iş yükleri için ayırma örnekleri

Tahmin edilebilir iş yükleri her zaman çalıştıran VM ile birlikte kullanılabilir olması gereken dosyalardır. Örneğin, satır iş kolu uygulamaları SAP ERP sistemi gibi.  Diğer taraftan, öngörülemeyen iş yükleri değişkeni olanla aynıdır. Örneğin üzerinde sırasında yüksek olan VM'ler talep ve yoğun olmayan saatlerde kapalı.

![Ayrılmış örnek](./media/contoso-migration-infrastructure/reserved-instance.png) 

Bunlar büyük süreleri için tutulması gerektiğini bildirin belirli VM örnekleri için ayrılmış örnekler kullanarak karşılığında konsol bir indirim hem öncelikli kapasite elde edebilirsiniz. Kullanarak [Azure ayrılmış örnekler](https://azure.microsoft.com/pricing/reserved-vm-instances/), Azure karma avantajı, % 82 normal Kullandıkça Öde kapalı Contoso kazandırabilir birlikte (Nisan 2018) fiyatlandırma.


## <a name="step-2-manage-hybrid-identity"></a>Adım 2: karma kimlik yönetme

Veren ve kimlik ve erişim yönetimi (IAM) ile Azure kaynaklarına kullanıcı erişimi denetleme birlikte Azure altyapınıza çekme içindeki önemli bir adımdır.  

- Contoso karar yerine kendi şirket içi Active Directory buluta genişletmek Azure yeni ayrı bir sistemde oluşturun.
- Bunlar, bir Azure tabanlı Bunu yapmak için Active Directory'ye oluşturur.
- Contoso sahip değilseniz Office 365 yerinde, yeni bir Azure AD sağlamak ihtiyaç duydukları şekilde.
- Office 365 kullanıcı yönetimi için Azure AD kullanır. Contoso Office 365 kullanıyorsanız, bunlar zaten bir Azure AD tenet sahip ve bunların birincil AD kullanın.
- [Daha fazla bilgi edinin](https://support.office.com/article/understanding-office-365-identity-and-azure-active-directory-06a189e7-5ec6-4af2-94bf-a22ea225a7a9) , Office 365 için Azure AD hakkında ve öğrenin [bir abonelik ekleme](https://docs.microsoft.com/azure/active-directory/active-directory-how-subscriptions-associated-directory) var olan bir Azure ad.

### <a name="create-an-azure-ad"></a>Azure AD oluşturma

Contoso, eklemiştir Azure AD ücretsiz sürüm bir Azure aboneliği ile kullanıyor. Yeni bir AD dizin aşağıdaki şekilde ekleyin:

1. İçinde [Azure portal](http://portal.azure.com/), Contoso Git **kaynak oluşturma** > **kimlik** > **Azure Active Directory**.
2. İçinde **dizin oluştur**, dizin için bir ad, bir ilk etki alanı adı ve bölge içinde Azure AD dizini oluşturulmalıdır belirtin.

    ![Azure AD oluşturma](./media/contoso-migration-infrastructure/azure-ad-create.png) 

    > [!NOTE]
    > Dizin oluştururken, form domainname.onmicrosoft.com bir ilk etki alanı adı içeriyor. Adı değiştirilmiş veya silinmiş. Bunun yerine, kullanıcıların kayıtlı bir etki alanı adı için Azure AD eklemek gerekir.

### <a name="add-the-domain-name"></a>Etki alanı adı ekleyin

Contoso kendi standart etki alanı adını kullanmak için Azure AD ile özel bir ad eklemek gerekir. Bu seçenek hakkında bilgi sahibi kullanıcı adları atamak yöneticilerinin olanak tanır. Örneğin, bir kullanıcının e-posta adresi oturum oturum açıp billg@contoso.com, gerek yerine billg@contosomigration.onmicrosoft.com. 

Özel adı ayarlamak için bunlar dizine ekleme, bir DNS girişi ekleyin ve ardından ad Azure AD'de doğrulayın.

1. İçinde **özel etki alanı adları** > **özel etki alanı Ekle**, etki alanına ekleyin.
2. Azure'da bir DNS girişi kullanmak için kendi etki alanı kayıt şirketinizle kaydetmeniz gerekir. 

    - İçinde **özel etki alanı adları** listesinde adı için DNS bilgilerinin unutmayın. Contoso bir MX giriş kullanma.
    - Bunu yapmak için ad sunucusu erişimi ihtiyaç duyar. Contoso, söz konusu olduğunda bunlar Contoso.com etki alanına oturum açmış ve not ettiğiniz ayrıntıları kullanarak Azure AD tarafından sağlanan DNS girişini için yeni bir MX kayıt oluşturuldu.  
1. Ayrıntılar ad etki alanı için DNS kayıtlarını yayılması sonra'ı tıklatın **doğrula** özel adı denetlemek için.

     ![Azure AD DNS](./media/contoso-migration-infrastructure/azure-ad-dns.png) 

### <a name="set-up-on-premises-and-azure-groups-and-users"></a>Şirket içi ve Azure grupları ve kullanıcıları ayarlama

Kendi Azure AD çalışır durumda olduğundan, Contoso gerek çalışanlara eklemek için Azure AD ile eşitler AD grupları şirket içi. Azure'da kaynak grupları adlarıyla şirket içi grup adlarını kullanmanızı öneririz. Bu, eşitleme amacıyla eşleşmeleri belirlemek kolaylaştırır.

#### <a name="create-resource-groups-in-azure"></a>Kaynak grupları oluşturma

Azure kaynak gruplarını birlikte Azure kaynaklarını toplayın. Bir kaynak grubu kimliği kullanılarak Grup içindeki kaynaklar üzerinde işlem gerçekleştirmek Azure sağlar.

- Birden çok kaynak grupları bir Azure aboneliğine sahip olabilir, ancak bir kaynak grubu yalnızca tek bir abonelik içinde bulunabilir.
- Ayrıca, tek bir kaynak grubu birden fazla kaynak olabilir, ancak bir kaynak yalnızca tek bir gruba ait olabilir.

Aşağıdaki tabloda özetlendiği gibi contoso Azure kaynak gruplarını ayarlayın.

**Kaynak grubu** | **Ayrıntılar**
--- | ---
**ContosoCobRG** | Bu Grup (COB) iş sürekliliği için ilgili tüm kaynakları içerir.  Contoso Azure Site Recovery hizmeti ve Azure Backup hizmeti kullanırken oluşturacak kasalarını içerir.<br/><br/> Ayrıca Azure geçirmek ve veritabanı geçiş hizmetleri dahil olmak üzere, geçiş için kullanılan kaynaklar dahil edilir.
**ContosoDevRG** | Bu grup, geliştirme ve test kaynakları içerir.
**ContosoFailoverRG** | Bu grup bir giriş bölgesi başarısız kaynakları için işlevi görür.
**ContosoNetworkingRG** | Bu grup, tüm ağ kaynaklarını içerir.
**ContosoRG** | Bu grup, üretim uygulamaları ve veritabanları için ilgili kaynakları içerir.

Bunlar gibi kaynak grupları oluşturun:

1. Azure portalında > **kaynak grupları**, bir grubu ekleyin.
2. Her grup için bunlar bir ad, grubun ait olduğu aboneliğin ve bölge belirtin.
3. Kaynak grupları görünür **kaynak grupları** listesi.

    ![Kaynak grupları](./media/contoso-migration-infrastructure/resource-groups.png) 


#### <a name="create-matching-security-groups-on-premises"></a>Eşleşen güvenlik grupları şirket içi oluşturma

1. Kendi şirket içi Active Directory'de güvenlik grupları Azure kaynak gruplarını adları eşleşen adlara sahip Contoso ayarlayın.
 
    ![Şirket içi AD güvenlik grupları](./media/contoso-migration-infrastructure/on-prem-ad.png) 

2. Yönetim amacıyla, bunlar tüm diğer gruplarının eklenecek başka bir grup oluşturur. Bu grup tüm kaynak gruplarına hakları Azure'da sahip olur. Genel yönetici, sınırlı sayıda bu gruba eklenir.

### <a name="synchronize-ad"></a>AD Eşitle

Contoso şirket kaynaklarına erişmek ve bulutta ortak bir kimlik sağlamak istediğinizde. Bunu yapmak için kullanıcılar kendi şirket içi Active Directory Azure AD ile tümleştirin. Bu model ile:

- Kullanıcıların ve kuruluşların şirket içi uygulamalara erişmek ve Office 365 ya da internet üzerindeki diğer sitelere binlerce gibi hizmetleri bulut için tek bir kimliği yararlanabilir.
- Yöneticileri uygulamak için AD grupları yararlanabilir [rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) azure'da.

Tümleştirme, Contoso kullanım kolaylaştırmak için [Azure AD Connect aracını](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect). Bir etki alanı denetleyicisinde Aracı'nı yükleyip yapılandığında yerel eşitler şirket içi AD kimlikleri Azure ad. 

### <a name="download-the-tool"></a>Aracı'nı indirme

1. Azure portalında Azure gider **Azure Active Directory** > **Azure AD Connect**ve eşitleme için kullandıkları sunucusuna aracının en son sürümünü yükler.

    ![İndirme AD Bağlan](./media/contoso-migration-infrastructure/download-ad-connect.png) 

2. Başlatmaları **AzureADConnect.msi** yükleme, kullanma **hızlı ayarları kullan**. Bu en yaygın yükleme ve tek ormanlı bir topolojiye için kimlik doğrulaması için parola karma eşitlemesi ile birlikte kullanılabilir.

    ![AD Connect Sihirbazı](./media/contoso-migration-infrastructure/ad-connect-wiz1.png) 

3. İçinde **Azure ad Connect**, Azure AD'de (form CONTOSO\admin veya contoso.com\admin) bağlanmak için kimlik bilgilerini belirtin.

    ![AD Connect Sihirbazı](./media/contoso-migration-infrastructure/ad-connect-wiz2.png) 

4. İçinde **AD DS'ye Bağlan**, şirket içi kimlik bilgilerini belirtin AD.

     ![AD Connect Sihirbazı](./media/contoso-migration-infrastructure/ad-connect-wiz3.png) 

5. İçinde **yapılandırma için hazır**, bunlar tıklatın **Yapılandırma tamamlandıktan sonra eşitleme işlemini başlatmak** eşitlemeyi hemen başlatmak için. Ardından bunlar yükleyin.


- Contoso Azure doğrudan bir bağlantı vardır. Varsa, şirket içi AD bir proxy'nin arkasında okunur bu [makale](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-troubleshoot-connectivity).
- İlk eşitleme sonrasında AD nesnelerini Azure AD'de görülebilir şirket içi.

    ![Şirket içi Azure AD'de](./media/contoso-migration-infrastructure/on-prem-ad-groups.png) 

- Contoso BT Ekibi temsil edilen her grubundaki onun rolüne göre.

    ![Şirket içi Azure AD üyeleri](./media/contoso-migration-infrastructure/on-prem-ad-group-members.png) 

### <a name="set-up-rbac"></a>RBAC ayarlayın

Azure [rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) Azure için ayrıntılı erişim yönetimi sağlar. RBAC kullanarak, yalnızca kullanıcıların işlerini yapmak için gereksinim duyduğu erişim miktarını verebilirsiniz. Kullanıcılar, gruplar ve kapsam düzeyinde uygulamalara uygun RBAC rolü atayın. Rol atamasının kapsamı, bir abonelik, bir kaynak grubu veya tek bir kaynak olabilir. 

Contoso şimdi atamak rol içi AD'den eşitlenen AD grupları.

1. İçinde **ControlCobRG** kaynak grubu, bunlar tıklatın **erişim denetimi (IAM)** > **Ekle**.
2. İçinde **izni Ekle** > **rol**, seçtikleri **katkıda bulunan**seçip **ContosoCobRG** listesinden AD grup. Grup sonra görünür **seçili üyeleri** listesi. 
3. Diğer kaynak gruplarının aynı izinlere sahip kullanıcılar bu yineleyin (dışında **ContosoAzureAdmins**), kaynak grubu eşleşen AD hesabı katkıda bulunan izinleri ekleyerek.
4. İçin **ContosoAzureAdmins** AD grubuna, bunlar Ata **sahibi** rol.

    ![Şirket içi Azure AD üyeleri](./media/contoso-migration-infrastructure/on-prem-ad-groups.png) 


## <a name="step-3-design-for-resilience-and-disaster"></a>3. adım: esnekliği ve olağanüstü durum için tasarlama

Azure kaynaklarını bölge içinde dağıtılır.
- Bölgeler coğrafyalara düzenlenir ve coğrafi sınırlar içinde veri residency, Egemenlik, uyumluluk ve dayanıklılık gereksinimlerini dikkate alınır.
- Bir bölge veri merkezleri kümesinden oluşur. Bu veri merkezleri içinde gecikme tanımlı bir çevre dağıtılan ve ayrılmış bölgesel düşük gecikme süreli ağ üzerinden bağlı.
- Her Azure bölgesi dayanıklılık için farklı bir bölge ile eşleştirilmiş.
- Hakkında bilgi edinin [Azure bölgeleri](https://azure.microsoft.com/global-infrastructure/regions/)ve anlama [nasıl bölgeler eşleştirilmiş](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).


Contoso karar Doğu (Virginia bulunur) ABD 2 kendi birincil bölge olarak ve kendi ikincil bölge olarak orta ABD gidin. Bunun nedeni birkaç vardır:

- Contoso datacenter New York'taki bulunur ve gecikme süresi en yakın veri merkezine kabul.
- Doğu ABD 2 bölge tüm hizmet ve ürünleri kullanmak için ihtiyaç duydukları sahiptir. Tüm Azure bölgeleri ürünlerin ve hizmetlerin kullanılabilir bakımından aynıdır. Gözden geçirebilirsiniz [bölgeye göre Azure ürünleri](https://azure.microsoft.com/global-infrastructure/services/).
- Orta ABD Doğu ABD 2 için Azure eşleştirilmiş bölgedir.

Karma ortamlarına hakkında düşünmek gibi iş Contoso esnekliği ve bir olağanüstü durum kurtarma stratejiniz bölge tasarımlarını oluşturma göz önüne almanız gerekir. Genel anlamıyla stratejileri aralığından Azure platformunda dağıtılan ve bakım gibi hata etki alanları ve bölgesel esneklik için tam bir etkin-etkin aracılığıyla eşleştirme model hangi bulut Hizmetleri ve veritabanı özellikleri kullanır tek bölge dağıtımı iki bölgede kullanıcılardan.

Contoso karar Orta yol gerçekleştirilecek. Bunlar, uygulamaları ve kaynakları bir birincil bölge içinde dağıtmak ve tam yedekleme tam uygulama olağanüstü durum veya bölge hatası durumunda olarak görev yapması hazır olması ikincil bölge'de, tam bir altyapı tutun.


## <a name="step-4-design-a-network-infrastructure"></a>4. adım: bir ağ altyapısını tasarlama

Yerinde kendi bölge tasarımıyla Contoso ağ stratejisi dikkate alınması gereken hazır. Nasıl kendi şirket içi veri merkezi ve Azure bağlanmak ve birbirleri ile iletişim kurmak ve ağ altyapınızın Azure tasarlamak nasıl dikkat etmeniz gerekir. Özellikle aktarmanız gerekir:

**Karma ağ bağlantısını planlama**: nasıl bunlar şirket içi ve Azure arasında ağlara bağlanma çıkmasını şekil.
**Bir Azure ağı altyapı tasarım**: Bunlar ağlar kendi bölgeleri nasıl dağıtacağınıza karar verin. Nasıl ağlar aynı bölge içinde ve bölgeler arasında iletişim kurar.
**Tasarım ve Azure ağları ayarlayın**: Azure ağları ve alt ağlar, ayarlama ve bunların içinde yer alacağını ne karar verin.

### <a name="plan-hybrid-network-connectivity"></a>Karma ağ bağlantısını planlama

Contoso kabul bir [mimarileri sayısı](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/) karma kendi şirket içi veri merkezi ve Azure arasında ağ için. [Daha fazla bilgi](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/considerations) seçenekleri karşılaştırma hakkında.

Bir anımsatıcı olarak Contoso şirket içi ağ altyapısı şu anda kendi veri merkezini New York'taki oluşur ve yerel ABD Doğu bölümünde dallandırır.  Tüm konumlara bir iş sınıfı internet bağlantınız.  Her dalları sonra datacenter IPSec VPN tüneli aracılığıyla Internet üzerinden bağlanır.

![Contoso ağ](./media/contoso-migration-infrastructure/contoso-networking.png) 

Contoso karma bağlantı uygulamak nasıl karar aşağıda verilmiştir:

1. New York'taki Contoso datacenter ve Doğu ABD 2 ve orta ABD iki Azure bölgelerinde arasında yeni bir siteden siteye VPN bağlantısı ayarlayın.
2. Azure sanal ağlarına bağlı şube office trafik ana Contoso veri merkezi yönlendirilecek. 
3. Kendi Azure dağıtım ölçeklendirme gibi kendi veri merkezi ve Azure bölgeleri arasında bir ExpressRoute bağlantı oluşturmanız. Bu durumda, yalnızca yük devretme amacıyla VPN siteden siteye bağlantı korunması.
    - [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/considerations) arasında VPN ve ExpressRoute karma çözümü seçme hakkında.
    - Doğrulama [ExpressRoute konumları ve Destek](https://docs.microsoft.com/azure/expressroute/expressroute-locations-providers).


**Yalnızca VPN**

![Contoso VPN](./media/contoso-migration-infrastructure/hybrid-vpn.png) 


**VPN ve ExpressRoute**

![Contoso VPN/ExpressRoute](./media/contoso-migration-infrastructure/hybrid-vpn-expressroute.png) 


### <a name="design-the-azure-network-infrastructure"></a>Azure ağ altyapısını tasarlama

Ağları Contoso bunların karma dağıtımı güvenli ve ölçeklenebilir bir şekilde yerinde koyan önemlidir. Bunu yapmak için Contoso uzun vadeli bir yaklaşım benimseyerek ve sanal ağlar (Vnet'ler) esnekliği ve kurumsal hazır olmasını tasarlama. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/virtual-network/virtual-network-vnet-plan-design-arm) sanal ağlar planlama hakkında.

Kendi iki bölgede bağlanmak için Contoso hub hub ağ modeli uygulamak karar:

- Her bölge içinde Contoso bir hub ve bağlı bileşen modeli kullanır.
- Ağlara ve hub'ları bağlamak için Azure ağ eşlemesi, Contoso kullanır.

#### <a name="network-peering"></a>Ağ eşlemesi

Azure sanal ağlar ve hub'ları bağlamak için Ağ eşlemesi sağlar. Genel eşliği sanal ağlar/hub'ları farklı bölgelerdeki arasında bağlantı sağlar. Yerel eşleme sanal ağlar aynı bölgede bağlanır. VNet eşlemesi, çok sayıda avantaj sağlar:

- Eşlenmiş Vnet'ler arasındaki ağ trafiğini özeldir.
- Sanal ağlar arasında trafiği üzerinde Microsoft omurga ağı tutulur. Sanal ağlar arasındaki iletişimde hiçbir genel internet, ağ geçitleri veya şifreleme gerekir.
- Eşleme varsayılan, farklı Vnet'lerdeki kaynaklar arasında düşük gecikmeli, yüksek bant genişlikli bağlantı sağlar.

[Daha fazla bilgi edinin](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) ağ eşlemesi hakkında.

#### <a name="hub-to-hub-across-regions"></a>Hub hub bölgeler arasında

Contoso her bölgede bir hub'ı dağıtır. Bir hub merkezi bir şirket içi ağınıza bağlantı noktası görevi gören Azure sanal ağ (VNet) kullanılıyor. Hub sanal ağlar birbirlerine genel VNet eşlemesi kullanarak bağlanır. Genel VNet eşlemesi sanal ağlar Azure bölgeler arasında bağlanır.

- Her bölge hub'ı başka bir bölgede kendi ortak hub'ına eşlenen.
- Hub kendi bölgede her ağa eşlenen ve tüm ağ kaynaklarına bağlanabilir.

    ![Genel eşliği](./media/contoso-migration-infrastructure/global-peering.png) 

#### <a name="hub-and-spoke-within-a-region"></a>Hub ve bağlı bileşen bir bölge içinde

Her bölge içinde Contoso sanal ağlar farklı amaçlar için bölge hub'dan spoke ağlar olarak dağıtır. Bir bölgedeki sanal ağlar kendi hub'ına ve birbirine bağlamak için eşliği'ni kullanın.

#### <a name="design-the-hub-network"></a>Hub ağını tasarlama

Bunlar hakkında düşünmek zorunda Contoso seçtiniz hub ve bağlı bileşen modeli içinde kendi şirket içi veri merkezini ve Internet trafiğini, yönlendirilir. İşte Contoso nasıl karar Doğu ABD 2 ve orta ABD hub'ları için yönlendirme işlemek:

- Bu paketleri gelen giden ağdan izleyin yol olduğu gibi "ters c", bilinen ağ tasarlama konusunda.
- Ağ mimarisinin iki sınırları, güvenilmeyen ön uç çevre bölge ve arka uç güvenilir bir bölgesi vardır.
- Bir güvenlik duvarı Güvenilen bölgeler erişimini denetleme her bölgede bir ağ bağdaştırıcısı gerekir.
- İnternet'ten:
    - Internet trafiği yükü dengelenmiş genel bir IP adresi ve çevre ağı üzerinde karşılaşır.
    - Bu trafiğe güvenlik duvarı üzerinden ve güvenlik duvarı kuralları tabi yönlendirilir.
    - Ağ erişim denetimleri uygulandıktan sonra trafiğin güvenilir bölgeye uygun konuma iletilir.
    - VNet giden trafiği kullanıcı tanımlı yollar (Udr'ler) kullanarak Internet'e yönlendirilir. Trafiği, güvenlik duvarı üzerinden zorlamalı ve Contoso ilkeleriyle uyumlu sahip denetlenir.
- Contoso veri merkezinden:
    - VPN siteden siteye (ya da ExpressRoute) üzerinden gelen trafiğe Azure VPN ağ geçidi genel IP adresi kadardır.
    - Trafik, güvenlik duvarı üzerinden ve güvenlik duvarı kuralları tabi yönlendirilir.
    - Kuralları uyguladıktan sonra trafiği güvenilen iç bölgesi alt ağdaki bir iç yük dengeleyici (standart SKU) iletilir.
    - Güvenilir alt ağdan giden trafik VPN üzerinden şirket içi veri merkezine güvenlik duvarı ve siteden siteye VPN üzerinden geçmeden önce uygulanan kurallar yoluyla yönlendirilir.



### <a name="design-and-set-up-azure-networks"></a>Tasarım ve Azure ağları ayarlayın

Bir ağ ve yerinde yönlendirme topolojisi, Contoso kendi Azure ağları ve alt ağları ayarlamak hazır.

- Contoso sınıfı özel ağ (0.0.0.0 için 127.255.255.255) Azure'da gerçekleştireceksiniz. Bu çalışır, adres aralıkları arasında herhangi bir çakışma olmayacak emin olabilir şirket içi bu yana geçerli bir sınıf B özel adres alanı 172.160.0/16 sahiptirler.
- Sanal ağlar, birincil ve ikincil bölgelerde dağıtmak için paylaşacağız.
- Önek içeriyor bir adlandırma kuralı kullanacağınız **VNET** ve bölge kısaltması **EUS2** veya **ö**. Bu standart kullanarak, hub ağları adlandırılacağını **VNET HUB EUS2** (Doğu ABD 2), ve **VNET HUB ö** (Orta ABD).
- Contoso sahip bir [IPAM çözüm](https://docs.microsoft.com/windows-server/networking/technologies/ipam/ipam-top), NAT ağ yönlendirme için planlama gerekir böylece


#### <a name="virtual-networks-in-east-us-2"></a>Doğu ABD 2 sanal ağlar

Doğu ABD 2 Contoso kaynaklarını ve Hizmetleri dağıtmak için kullanacağınız birincil bölge ' dir. İşte nasıl Mimarı ağları içindeki oluşturacağız:

- **Hub**: Doğu ABD 2, VNet hub merkezi kendi şirket içi veri merkezine birincil bağlantı noktasıdır.
- **Sanal ağlar**: Doğu ABD 2 bağlı bileşen sanal ağlar, gerekirse iş yükleri ayırmak için kullanılabilir. Hub VNet ek olarak, iki uç Vnet'ler Doğu ABD 2 Contoso olacaktır:
    - **VNET GELİŞTİRME EUS2**. Bu VNet geliştirme ve test takım geliştirme projeleri için tam olarak işlevsel bir ağ olacak sağlar. Bir üretim pilot alanı olarak hareket eder ve üretim altyapı işlevine bağlıdır.
    - **VNET üretim EUS2**: Azure Iaas üretim bileşenleri bu ağda bulunan. 
    -  Her sanal ağ kendi benzersiz adres alanı ile örtüşmez sahip olur. NAT gerek kalmadan yönlendirmeyi yapılandırma düşündüğünüz
- **Alt ağlar**:
    - Her ağdaki her uygulama katmanı için bir alt ağ olacaktır
    - Üretim ağındaki her alt ağ geliştirme VNet içinde eşleşen bir alt ağ gerekir.
    - Buna ek olarak, üretim ağ etki alanı denetleyicileri için bir alt ağ vardır.

Doğu ABD 2 sanal ağlar aşağıdaki tabloda özetlenmiştir.

**Sanal ağ** | **Aralık** | **Eş**
--- | --- | ---
**VNET HUB EUS2** | 10.240.0.0/20 | VNET HUB CUS2, VNET GELİŞTİRME EUS2, VNET ÜRETİM EUS2
**VNET GELİŞTİRME EUS2** | 10.245.16.0/20 | VNET HUB EUS2
**VNET ÜRETİM EUS2** | 10.245.32.0/20 | VNET HUB EUS2, VNET ÜRETİM Ö

![Hub/bağlı birincil bölge içinde](./media/contoso-migration-infrastructure/primary-hub-peer.png) 


#### <a name="subnets-in-the-east-us-2-hub-network-vnet-hub-eus2"></a>Doğu ABD 2 hub alt ağ (VNET-HUB-EUS2)

**Alt ağ/bölge** | **CIDR** | ** Kullanılabilir IP adresleri
--- | --- | ---
**IB UntrustZone** | 10.240.0.0/24 | 251
**IB TrustZone** | 10.240.1.0/24 | 251
**OB UntrustZone** | 10.240.2.0/24 | 251
**OB TrustZone** | 10.240.3.0/24 | 251
**GatewaySubnets** | 10.240.10.0/24 | 251


#### <a name="subnets-in-the-east-us-2-dev-network-vnet-dev-eus2"></a>Doğu ABD 2 geliştirme alt ağ (VNET-DEV-EUS2)

Geliştirme VNet geliştirme ekibi tarafından üretim pilot alanı olarak kullanılır. Üç alt ağı yok.

**Alt ağ** | **CIDR** | **adresleri** | **Alt ağ**
--- | --- | --- | ---
**GELİŞTİRME FE EUS2** | 10.245.16.0/22 | 1019 | Ön uçlar/web katmanı VM'ler
**GELİŞTİRME UYGULAMA EUS2** | 10.245.20.0/22 | 1019 | Uygulama katmanı VM'ler
**GELİŞTİRME DB EUS2** | 10.245.24.0/23 | 507 | Veritabanı VM'ler


#### <a name="subnets-in-the-east-us-2-production-network-vnet-prod-eus2"></a>Alt ağlar Doğu ABD 2 üretim ağındaki (VNET-üretim-EUS2)

Azure Iaas bileşenler üretim ağında bulunur. Her uygulama katmanı kendi alt ağ vardır. Alt ağlar geliştirme ağdaki bir alt etki alanı denetleyicileri için eklenmesi ile eşleştiğinden.

**Alt ağ** | **CIDR** | **adresleri** | **Alt ağ**
--- | --- | --- | ---
**ÜRETİM FE EUS2** | 10.245.32.0/22 | 1019 | Ön uçlar/web katmanı VM'ler
**ÜRETİM UYGULAMA EUS2** | 10.245.36.0/22 | 1019 | Uygulama katmanı VM'ler
**ÜRETİM DB EUS2** | 10.245.40.0/23 | 507 | Veritabanı VM'ler
**ÜRETİM DC EUS2** | 10.245.42.0/23 | 251 | Etki alanı denetleyicisi VM'ler


![Hub ağ mimarisi](./media/contoso-migration-infrastructure/azure-networks-eus2.png)


#### <a name="virtual-networks-in-central-us-secondary-region"></a>Orta ABD (ikincil bölge ') sanal ağlar

Orta ABD Contoso'nun ikincil bölge ' dir. İşte nasıl Mimarı ağları içindeki oluşturacağız:

- **Hub**: Doğu ABD 2, Vnet kendi şirket içi veri merkezini ve Doğu ABD 2 sanal ağlar, gerekirse, iş yüklerini ayırmak için kullanılabilir uç bağlantıyı merkezi noktasıdır hub'ı diğer bağlı bileşen ayrı olarak yönetilen.
- **Sanal ağlar**: Orta ABD iki Vnet gerekir:
    - VNET-ÜRETİM-Ö. Bu VNet VNET-PROD_EUS2 için benzer bir üretim ağı ' dir. 
    - VNET-ASR-Ö. Bu VNet birincil sunucudan ikincil bölge'ye devredildi Azure VM'ler için sanal makineleri şirket içi yük devretme sonrasında oluşturulan konumu olarak ya da bir konum olarak hareket eder. Bu ağ benzer üretim ağlara, ancak tüm etki alanı denetleyicilerinde.
    -  Her bir Vnet'teki bölgede kendi adres alanı hiçbir çakışıyor sahip olur. NAT gerek kalmadan yönlendirmeyi yapılandırma düşündüğünüz
- **Alt ağlar**: alt ağlar söz konusu Doğu ABD 2 benzer bir şekilde tasarlanmış. Özel durum bunların bir alt etki alanı denetleyicileri için gerekmemesidir.

Sanal ağlar Orta ABD aşağıdaki tabloda özetlenmiştir.

**Sanal ağ** | **Aralık** | **Eş**
--- | --- | ---
**VNET HUB Ö** | 10.250.0.0/20 | VNET HUB EUS2, VNET ASR Ö, VNET ÜRETİM Ö
**VNET ASR Ö** | 10.255.16.0/20 | VNET HUB Ö, VNET ÜRETİM Ö
**VNET ÜRETİM Ö** | 10.255.32.0/20 | VNET HUB Ö, VNET ASR Ö, VNET ÜRETİM EUS2  


![Hub/bağlı eşleştirilmiş bölgede](./media/contoso-migration-infrastructure/paired-hub-peer.png)


#### <a name="subnets-in-the--central-us-hub-network-vnet-hub-cus"></a>BİZE merkezi Hub alt ağ (VNET-HUB-ö)

**Alt ağ** | **CIDR** | **Kullanılabilir IP adresleri**
--- | --- | ---
**IB UntrustZone** | 10.250.0.0/24 | 251
**IB TrustZone** | 10.250.1.0/24 | 251
**OB UntrustZone** | 10.250.2.0/24 | 251
**OB TrustZone** | 10.250.3.0/24 | 251
**GatewaySubnet** | 10.250.2.0/24 | 251


#### <a name="subnets-in-the-central-us-production-network-vnet-prod-cus"></a>Alt ağlar merkezi BİZE üretim ağındaki (VNET-üretim-ö)

Birincil Doğu ABD 2 bölgesindeki üretim ağı için paralel olarak ikincil Orta ABD bölgesinde bir üretim ağı yok. 

**Alt ağ** | **CIDR** | **adresleri** | **Alt ağ**
--- | --- | --- | ---
**ÜRETİM FE Ö** | 10.255.32.0/22 | 1019 | Ön uçlar/web katmanı VM'ler
**ÜRETİM UYGULAMA Ö** | 10.255.36.0/22 | 1019 | Uygulama katmanı VM'ler
**ÜRETİM DB Ö** | 10.255.40.0/23 | 507 | Veritabanı VM'ler
**ÜRETİM DC Ö** | 10.255.42.0/24 | 251 | Etki alanı denetleyicisi VM'ler

#### <a name="subnets-in-the-central-us-failoverrecovery-network-in-central-us-vnet-asr-cus"></a>Orta ABD (VNET-ASR-ö) Orta ABD yük devretme/kurtarma ağında alt ağlar

VNET ASR ö ağ bölgeler arasında yük devretme amacıyla kullanılır. Site kurtarma, çoğaltma ve bölgeler arasında Azure Vm'leri üzerinde başarısız için kullanılacak. Contoso datacenter, şirket içi kalır, ancak olağanüstü durum kurtarma için Azure'a yük korunan iş yükleri için Azure ağ olarak da işler.

VNET ASR ö VNET Doğu ABD 2, ancak bir etki alanı denetleyicisi alt gerek kalmadan üretim aynı temel alt ağda değil.

**Alt ağ** | **CIDR** | **adresleri** | **Alt ağ**
--- | --- | --- | ---
**ASR FE Ö** | 10.255.16.0/22 | 1019 | Ön uçlar/web katmanı VM'ler
**ASR UYGULAMA Ö** | 10.255.20.0/22 | 1019 | Uygulama katmanı VM'ler
**ASR DB Ö** | 10.255.24.0/23 | 507 | Veritabanı VM'ler

![Hub ağ mimarisi](./media/contoso-migration-infrastructure/azure-networks-cus.png)

#### <a name="configure-peered-connections"></a>Eşlenen bağlantılarını yapılandırma

Her bölge hub'ı başka bir bölgede hub'ına ve hub bölge içindeki tüm sanal ağlar için eşlenen. Bu hub'ları için iletişim ve tüm sanal ağlar bir bölge içinde görüntülemek için sağlar. Şunlara dikkat edin:

- Eşleme iki taraflı bir bağlantı oluşturur. İlk VNet başlatma eş adresinden ve ikinci VNet üzerinde başka bir.
- Karma bir dağıtımda, eşler arasında geçen trafik Azure ve şirket içi veri merkezi arasında VPN bağlantısı görülen gerekiyor. Bu ayarı etkinleştirmek için eşlenen bağlantılarında ayarlanması gereken bazı özel ayarları vardır.

Tüm bağlantılarından için spoke sanal ağlar hub'ı aracılığıyla şirket içi veri merkezine, Contoso VPN ağ geçitleri iletilen ve çapraz olmak trafiğine izin vermek gerekiyor.

##### <a name="domain-controller"></a>Etki alanı denetleyicisi

VNET üretim EUS2 ağdaki etki alanı denetleyicileri, Contoso EUS2 hub/üretim ağı arasında hem şirket içi VPN bağlantısı üzerinden akışına istemektedir. Bunu yapmak için aşağıdaki izin vermek ihtiyaç duydukları:

1. **İletilen trafiğe izin ver** ve **ağ geçidi transit yapılandırmaları izin** eşlenmiş bağlantısı. Bizim örneğimizde bu VNET-HUB-EUS2 VNET üretim EUS2 bağlantı olur.

    ![Eşleme](./media/contoso-migration-infrastructure/peering1.png)

2. **İletilen trafiğe izin** ve **uzak ağ geçitlerini kullanan** eşlemenin, VNET-üretim-EUS2 VNET HUB EUS2 bağlantı üzerindeki diğer tarafta.

    ![Eşleme](./media/contoso-migration-infrastructure/peering2.png)

3. Bunlar ağa VPN tüneli üzerinden yönlendirmek için yerel trafiğini yönlendiren bir statik yol ayarlayacaksınız şirket içi. Yapılandırma Contoso Azure VPN tüneli sağlayan ağ geçidinde tamamlanacaktır. Contoso, Windows Yönlendirme ve Uzaktan erişim için bunu kullanır.

    ![Eşleme](./media/contoso-migration-infrastructure/peering3.png)

##### <a name="production-networks"></a>Üretim ağları 

Spoked eş ağ spoked eş ağ üzerinden bir hub'ı başka bir bölgede göremezsiniz.

Birbirine görmek için her iki bölgelerdeki Contoso'nun üretim ağları için VNET üretim EUS2 ve NCEKİ üretim ö için doğrudan eşlenen bağlantı oluşturmak ihtiyaç duyar. 

![Eşleme](./media/contoso-migration-infrastructure/peering4.png)

### <a name="set-up-dns"></a>DNS'yi ayarlama

Sanal ağlar kaynaklarında dağıttığınızda, birkaç seçeneğiniz etki alanı adı çözümlemesine yönelik sahip. Azure tarafından sağlanan ad çözümlemesi kullanın veya DNS sunucuları için çözüm sağlar. Kullandığınız ad çözümlemesi türünün nasıl kaynaklarınızı birbirleri ile iletişim kurmak gereksinimlerine göre değişir. Alma [daha fazla bilgi](https://docs.microsoft.com/azure/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances#azure-provided-name-resolution) Azure DNS hizmeti hakkında.

Contoso karar Azure DNS hizmeti karma ortamlarında iyi bir seçenek değildir. Bunun yerine, bunlar, şirket içi DNS sunucularına yararlanan oluşturacağız.

- Bu karma ağ tüm sanal makineleri şirket içi olduğundan ve Azure düzgün çalışması için adlarını çözümleyebilmesi gerekir. Başka bir deyişle, özel DNS ayarları için tüm sanal ağlar uygulanmış olması gerekir.
- Contoso şu anda Contoso veri merkezi ve şube ofislerindeki dağıtılan DC'leri yok. Birincil DNS sunucularından CONTOSODC1(172.16.0.10) ve CONTOSODC2(172.16.0.1) olan
- Sanal ağlar dağıtıldığında, şirket içi etki alanı denetleyicileri ağlarda DNS sunucusu olarak kullanılmak üzere ayarlanacak. 
- Bu, özel DNS VNet üzerinde kullanırken yapılandırmak için Azure'nın özyinelemeli çözümleyiciler IP adresi (örneğin, 168.63.129.16) DNS listesine eklenmesi gerekir.  Bunu yapmak için Contoso her VNet üzerinde DNS sunucusu ayarlarını yapılandırır. Örneğin, VNET HUB EUS2 ağ özel DNS ayarlarını aşağıdaki gibi olur:
    
    ![Özel DNS](./media/contoso-migration-infrastructure/custom-dns.png)

Kendi şirket içi etki alanı denetleyicilerinin yanı sıra Contoso adımıdır dört daha fazla Azure ağlarına, her bölge için iki desteklemek için uygulanacak. İşte ne bunlar Azure'da dağıtacaksınız.

**Bölge** | **DC** | **Sanal ağ** | **Alt ağ** | **IP adresi**
--- | --- | --- | --- | ---
EUS2 | CONTOSODC3 | VNET ÜRETİM EUS2 | ÜRETİM DC EUS2 | 10.245.42.4
EUS2 | CONTOSODC4 | VNET ÜRETİM EUS2 | ÜRETİM DC EUS2 | 10.245.42.5
Ö | CONTOSODC5 | VNET ÜRETİM Ö | ÜRETİM DC Ö | 10.255.42.4
Ö | CONTOSODC6 | VNET ÜRETİM Ö | ÜRETİM DC Ö | 10.255.42.4

Şirket içi etki alanı denetleyicileri dağıttıktan sonra Contoso yeni etki alanı denetleyicileri, DNS sunucusu listesinde dahil etmek için DNS ayarları ya da bölge ağlarda güncellemeniz gerekir.



#### <a name="set-up-domain-controllers-in-azure"></a>Azure etki alanı denetleyicileri ayarlama

Ağ ayarları güncelleştirdikten sonra Contoso etki alanı denetleyicilerini Azure çıkışı derlemek için hazır.

1. Azure portalında bunlar uygun Vnet'e yeni bir Windows Server VM dağıtın.
2. Bunlar, VM için her konumda kullanılabilirlik kümeleri oluşturun. Kullanılabilirlik kümeleri aşağıdakileri yapın:
    - Azure dokusunu Azure bölgesindeki farklı altyapılar içine VM'ler ayıran emin olun. 
    -  Contoso %99,95 SLA Azure VM'ler için uygun olmasını sağlar.  [Daha fazla bilgi edinin](https://docs.microsoftcom/azure/virtual-machines/windows/regions-and-availability#availability-sets).

    ![Kullanılabilirlik grubu](./media/contoso-migration-infrastructure/availability-group.png) 
3. VM dağıtıldıktan sonra VM için ağ arabirimi kalem. Burada, bunlar özel IP belirlenen statik adres ve geçerli bir adres belirtin.

    ![VM NIC](./media/contoso-migration-infrastructure/vm-nic.png)

4. Şimdi, bunlar yeni bir veri diski VM'e ekleyin. Bu disk, Active Directory veritabanını ve sysvol paylaşımının içerir. 
    - Disk boyutu, desteklenen IOPS sayısını belirler.
    - Zaman içindeki disk boyutunu ortam büyüdükçe artırmanız gerekebilir.
    - Sürücü okuma/yazma için ana bilgisayar önbelleğe almayı ayarlanmamalıdır. Active Directory veritabanlarına bu desteklemez.

     ![Active Directory disk](./media/contoso-migration-infrastructure/ad-disk.png)

5. Disk eklendikten sonra bunların VM'ye Uzak Masaüstü üzerinden bağlanın ve Sunucu Yöneticisi'ni açın.
6. Ardından **dosya ve depolama hizmetleri**, sürücü harfi F: verilen veya üstü olduğundan emin olmanın Yeni Birim Sihirbazı çalıştırmak yerel VM üzerinde.

     ![Yeni Birim Sihirbazı](./media/contoso-migration-infrastructure/volume-wizard.png)

7. Sunucu Yöneticisi'nde ekledikleri **Active Directory etki alanı Hizmetleri** rol. Ardından, bunlar VM bir etki alanı denetleyicisi olarak yapılandırın.

      ![Sunucu rolü](./media/contoso-migration-infrastructure/server-role.png)  

9. VM bir DC olarak yapılandırılmış ve yeniden sonra DNS Yöneticisi'ni açın ve Azure DNS Çözümleyicisi iletici olarak yapılandırın.  Bu, Azure DNS'de çözümlenemiyor DNS sorgularını iletmek etki alanı denetleyicisi sağlar.

    ![DNS ileticisi](./media/contoso-migration-infrastructure/dns-forwarder.png)

10. Şimdi, VNet bölge için uygun etki alanı denetleyicisiyle sonra her bir Vnet'teki özel DNS ayarlarını güncelleştirin. Şirket içi DC'leri listede içerirler.

### <a name="set-up-active-directory"></a>Active Directory'yi ayarlama ayarlayın

AD ağı oluşturulmasında kritik bir hizmet olduğundan ve doğru şekilde yapılandırılmalıdır. Contoso Contoso veri merkezi ve EUS2 ve ö bölgeleri için AD siteler oluşturacaksınız.  

1. Bunlar, iki yeni site (AZURE EUS2 ve AZURE ö) veri merkezi site (ContosoDatacenter) ile birlikte oluşturun.
2. Siteler oluşturduktan sonra bunların siteler sanal ağlar ve datacenter eşleştirmek için alt ağlar oluşturun.

    ![DC alt ağlar](./media/contoso-migration-infrastructure/dc-subnets.png)

3. Ardından, her şeyi bağlanmak için iki site bağlantısı oluşturun. Etki alanı denetleyicileri sonra konumlarına taşınması.

    ![DC bağlantılar](./media/contoso-migration-infrastructure/dc-links.png)

4. Her şeyi yapılandırıldıktan sonra Active Directory çoğaltma topolojisi yerdir.
    
    ![DC çoğaltma](./media/contoso-migration-infrastructure/ad-resolution.png)

5. Her şeyi tam etki alanı denetleyicileri ve sitelerin listesini gösterildiğinden, şirket içi Active Directory Yönetim Merkezi'nde.

    ![AD Yönetim Merkezi](./media/contoso-migration-infrastructure/ad-center.png)

## <a name="step-5-plan-for-governance"></a>5. adım: yönetişim için planlama

Azure Hizmetleri ve Azure platformu üzerinde bir dizi idare denetimleri sağlar. [Daha fazla bilgi](https://docs.microsoft.com/azure/security/governance-in-azure) seçenekleri temel bir anlayış için.

Kimliği yapılandırmak ve erişim denetimi gibi Contoso zaten başlamış idare ve güvenlik bazı yönlerini yerleştirdiniz. Genel anlamıyla göz önünde bulundurmanız gereken üç alan vardır:

- **İlke**: Azure ilke uygulanır ve kaynakları Kurumsal gereksinimleri ve SLA ile uyumlu olan kurallar ve etkileri kaynaklarınızı uygular.
- **Kilitler**: Azure verir kilit abonelikler, kaynak grupları ve diğer kaynaklara böylece bunu yetkisi olanlar tarafından yalnızca değiştirilebilir.
- **Etiketler**: kaynakları denetlenen, denetlenen ve etiketleriyle yönetilen. Etiketler meta veri kaynakları veya sahipleri hakkında bilgi sağlayan kaynaklarına ekleyin.

### <a name="set-up-policies"></a>İlkeleri Ayarla

Azure İlkesi Hizmeti yerinde sahip ilke tanımları ile uyumlu olmayan olanlar için tarama kaynaklarınızı değerlendirir. Örneğin, yalnızca sanal makineleri belirli türde izin veren veya belirli bir etikete sahip için kaynaklar gerektirir bir ilkeye sahip. 

Bir ilke tanımı Azure ilkeleri ve ilke atamasını bir ilkenin uygulanmasını kapsam belirtin. Kapsamı bir yönetim grubundan bir kaynak grubuna aralığında değişebilir. [Bilgi](https://docs.microsoft.com/azure/azure-policy/create-manage-policy) oluşturma ve ilkelerini yönetme hakkında.

Contoso istediğiniz ilkelerin birkaç ile çalışmaya başlamak:

- Kaynakları yalnızca EUS2 ve ö bölgelerde dağıtılmasını sağlamak için bir ilke isterler.
- Bunlar için onaylanan SKU'ları yalnızca VM SKU'ları sınırlamak istiyorsunuz. Amacınız, pahalı VM SKU'ları kullanılmayan sağlamaktır.

#### <a name="limit-resources-to-regions"></a>Bölgelere sınırı kaynakları

Contoso kullanmak yerleşik ilke tanımı **konumları izin** kaynak bölgeleri sınırlamak için.

1. Azure portalında tıklatın **tüm hizmetleri**, arayın ve **İlkesi**.
2. Seçin **atamaları** > **atama İlkesi**.
3. İlke listesinde seçin **konumları izin**.
4. Ayarlama **kapsam** adına Azure aboneliği ve izin verilenler iki bölgelerde seçin.

    ![Bölgeler izin ilkesi](./media/contoso-migration-infrastructure/policy-region.png)

5. Varsayılan olarak, ilke ile ayarlanmışsa **reddetme**, birisi olması durumunda anlamına başlatıldığında bir dağıtım değil EUS2 veya ö Abonelikteki, dağıtım başarısız olur. Kişi Contoso abonelik Batı ABD dağıtımı ayarlamak üzere çalışırsa, şunlar olur.

    ![İlke başarısız oldu](./media/contoso-migration-infrastructure/policy-failed.png)

#### <a name="allow-specific-vm-skus"></a>Belirli VM SKU'ları izin ver

Contoso, yerleşik ilke tanımı kullanacağınız **sanal makineleri SKU'ları izin** abonelikte oluşturulabilir VM'ler türünü sınırlamak için. 

![İlke SKU](./media/contoso-migration-infrastructure/policy-sku.png)



#### <a name="check-policy-compliance"></a>İlke uyumluluğu denetle

Hemen ilkeleri yürürlüğe girer ve Contoso uyumluluk için kaynakları kontrol edebilirsiniz. 

1. Azure portalında tıklatın **Uyumluluk** bağlantı.
2. Uyumluluk Pano görüntülenir. Daha fazla ayrıntı için ayrıntıya girebilirsiniz.

    ![İlke uyumluluğu](./media/contoso-migration-infrastructure/policy-compliance.png)


### <a name="set-up-locks"></a>Kilitli ayarlayın

Contoso sistemlerini yönetimi için uzun ITIL framework kullanıyor. Framework'ün en önemli yönlerinden denetim değiştirmektir ve Contoso değişiklik denetimi kendi Azure dağıtımdaki uygulandığından emin olmak istiyor.

Contoso kilitleri aşağıdaki gibi gerçekleştirin geçmeye:

- Herhangi bir üretim veya yük devretme bileşeni ReadOnly kilidi olan bir kaynak grubunda olması gerekir.  Bu, değiştirmek veya üretim öğeleri silmek için kilit kaldırılması gerektiğini anlamına gelir. 
- Olmayan üretim kaynak grupları CanNotDelete kilitleri sahip olur. Kullanıcıların yetkili Bunun anlamı okuma veya bir kaynak değiştirme, ancak bunu silemezsiniz.

[Daha fazla bilgi edinin](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-lock-resources) kilitleri hakkında.

### <a name="set-up-tagging"></a>Etiketleme ayarlama

Eklenen kaynakları izlemek için kaynaklar uygun departmanı, müşteri ve ortam ile ilişkilendirilecek contoso giderek daha çok önemli olacaktır. 

Kaynakları ve sahipleri hakkında bilgi sağlamaya ek olarak, etiketleri Contoso toplama ve Grup kaynakları ve bu verileri geri ödeme amacıyla kullanmak için olanak sağlar.

Contoso Azure varlıklarına işletmelerini için anlamlı bir şekilde görselleştirin gerekir. Örnek, ancak rol veya departmanı için. Kaynakların bir etiketi paylaşması için aynı kaynak grubunda bulunmaları gerekmez unutmayın. Bunu yapmak için Contoso böylece herkes aynı etiketleri kullanarak basit etiket sınıflandırmanızı oluşturur.

**Etiket adı** | **Değer**
--- | ---
Maliyet merkezi | 12345: SAP geçerli bir maliyet Merkezi'nden olması gerekir.
Departmanı | Departmandan (SAP) adı. Eşleşme CostCenter.
ApplicationTeam | Sahibi olan uygulama için Destek ekibine e-posta diğer adı.
Katalogadı | Kaynak destekleyen hizmet Kataloğu başına uygulama veya ShareServices, adıdır.
ServiceManager | E-posta diğer kaynak ITIL Service Manager'ın.
COBPriority | Önceliği BCDR için iş tarafından ayarlanmış. Değerler 1-5.
ZARF | Geliştirme, STG, üretim olası değerler şunlardır. Geliştirme, hazırlama ve üretim temsil eden.

Örneğin: 

 ![Azure etiketleri](./media/contoso-migration-infrastructure/azure-tag.png)

Etiket oluşturduktan sonra Contoso geri dönün ve yeni Azure ilke tanımları ve gerekli etiketleri kullanımını kuruluş genelinde zorunlu kılmak için atamalar oluşturun.


## <a name="step-6-consider-security"></a>6. adım: güvenlik göz önünde bulundurun.

Güvenlik bulutta önemlidir ve Azure çok çeşitli güvenlik araçları ve yetenekleri sağlar. Bu güvenli Azure platformunda güvenli çözümleri oluşturmanıza yardımcı olmak. Okuma [güvenilen bulutta güvenirlik](https://azure.microsoft.com/overview/trusted-cloud/) Azure güvenliği hakkında daha fazla bilgi için.

Var. contoso dikkate alınması gereken birkaç ana yönü

- **Azure Güvenlik Merkezi**: Azure Güvenlik Merkezi, karma bulut iş yüklerinde Birleşik güvenlik yönetimi ve Gelişmiş tehdit koruması sağlar. Güvenlik Merkezi ile, iş yüklerinize güvenlik ilkeleri uygulayabilir, tehditlere maruz kalma riskinizi sınırlayabilir ve saldırıları algılayıp onlara yanıt verebilirsiniz.  [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/security-center/security-center-intro).
- **Ağ güvenlik grupları (Nsg'ler)**: bir NSG olduğundan, güvenlik listesini içeren bir filtre (Güvenlik Duvarı), kuralları uygulandığında, izin verin veya Azure sanal ağlar için bağlı kaynaklar için ağ trafiğini reddedin. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/virtual-network/security-overview).
- **Veri şifreleme**: Azure Disk şifrelemesi, Windows ve Linux Iaas sanal makine disklerinizi şifrelemek yardımcı olan bir özellik değil. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/security/azure-security-encryption-atrest).

### <a name="work-with-the-azure-security-center"></a>Azure Güvenlik Merkezi ile çalışma

Contoso güvenlik yaklaşımı, yeni bir karma Bulut ve özellikle Azure yüklerini, hızlı bir görünüme için arıyor.  Sonuç olarak, Contoso Azure Güvenlik Merkezi uygulamak aşağıdaki özelliklerle başlayarak karar: 

- Merkezi ilke yönetimi
- Sürekli değerlendirme
- Eyleme dönüştürülebilir öneriler 

#### <a name="centralize-policy-management"></a>İlke yönetimini merkezi hale getirin

Merkezi ilke yönetimi ile Contoso, güvenlik gereksinimleriyle uyumsuz güvenlik ilkelerini merkezi olarak tüm ortamları boyunca yöneterek olun. Bunlar basit ve hızlı bir şekilde tüm Azure kaynakları için geçerli bir ilke uygulayabilirsiniz.

![Güvenlik ilkesi](./media/contoso-migration-infrastructure/security-policy.png)

#### <a name="assess-and-action"></a>Değerlendirmek ve eylem

Contoso makineleri, ağları, depolama, veri ve uygulamaları güvenlik izleyicilerinin sürekli güvenlik değerlendirmesi özelliğinden yararlanır; olası güvenlik sorunlarını bulmak için. 

- Güvenlik Merkezi güvenlik durumunu Contoso'nun hesaplama, altyapı ve veri kaynakları ve Azure uygulamaları ve Hizmetleri analiz eder.
- Sürekli değerlendirme veya ağ bağlantı noktalarını kullanıma sunulan güvenlik güncelleştirmeleri eksik olan sistemleri gibi olası güvenlik sorunlarını keşfetmeye Contoso işletim ekibi yardımcı olur. 
- Özellikle Contoso tüm Vm'leri korumalı emin olmak istiyor. Güvenlik Merkezi bu ile VM sistem durumu doğrulama ve yararlanan önce güvenlik açıkları düzeltmek için öncelikli ve uygulanabilir öneriler yaparak yardımcı olur.

![İzleme](./media/contoso-migration-infrastructure/monitoring.png)

### <a name="work-with-nsgs"></a>Nsg'ler ile çalışma

Contoso ağ trafiği için ağ güvenlik gruplarını kullanarak bir sanal ağ kaynaklarında sınırlayabilirsiniz.

- Bir ağ güvenlik grubunda gelen veya giden ağ trafiğine kaynak ya da hedef IP adresi, bağlantı noktası ve protokole göre izin veren veya bu trafiği reddeden güvenlik kurallarından oluşan bir liste bulunur.
- Bir alt ağa uygulandığında kuralları alt ağdaki tüm kaynaklara uygulanır. Ağ arabirimleri ek olarak, bu alt ağda dağıtılan Azure Hizmetleri örneklerini içerir.
- Uygulama güvenlik grupları (ASGs), ağ güvenlik grubu VM'ler için izin verme, bir uygulama yapısının doğal bir uzantısı olarak yapılandırın ve bu gruplara göre ağ güvenlik ilkelerini tanımlama olanak tanır.
    - Uygulama güvenlik grupları, güvenlik ilkeniz açık IP adreslerinin el ile bakım olmadan ölçekte tekrar anlamına gelir. Platform açık IP adreslerinin ve birden fazla kural kümesinin karmaşık süreçlerini üstlenerek iş mantığınıza odaklanmanızı sağlar.
    - Bir uygulama güvenlik grubunu bir güvenlik kuralında kaynak ve hedef olarak belirtebilirsiniz. Güvenlik ilkeniz tanımlandıktan sonra VM'ler oluşturabilir ve bir gruba VM NIC'ler atayın. 


Contoso Nsg'ler ve ASGs bir karışımını gerçekleştireceksiniz. NSG yönetimi konusunda endişe oldukları. Bunlar, Nsg'ler ve bunların işlem personeli için anlamına gelebilir karmaşıklık aşırı kullanımı hakkında endişeleniyoruz.  Bu durum dikkate alınarak, bunlar genel bir kural kullandıkları iki anahtar sorumluları benimseyen:

- Tüm trafiği içine ve dışına tüm alt ağlar (Kuzey-Güney) Hub ağlarda GatewaySubnets dışında bir NSG kuralı tabi olacaktır.
- Herhangi bir güvenlik duvarları veya etki alanı denetleyicisi alt Nsg'ler ve NIC Nsg'ler tarafından korunur.
- Tüm üretim uygulamaları uygulanan ASGs sahip olur.


Contoso bir modelin bu için uygulamalarını nasıl görüneceğine oluşturdu.

![Güvenlik](./media/contoso-migration-infrastructure/asg.png)


ASGs ile ilişkili Nsg'ler bir ağın parçası hedefine paketler yalnızca izin akabilir emin olmak için en az ayrıcalık ile yapılandırılır.

**Eylem** | **Ad** | **Kaynak** | **Hedef** | **Bağlantı Noktası**
--- | --- | --- | --- | --- 
İzin Ver | AllowiInternetToFE | VNET-HUB-EUS1/IB-TrustZone | APP1-FE 80, 443
İzin Ver | AllowWebToApp | APP1-FE | APP1-DB | 1433
İzin Ver | AllowAppToDB | APP1 UYGULAMA | Herhangi biri | Herhangi biri
Reddet | DenyAllInbound | Herhangi biri | Herhangi biri | Herhangi biri

### <a name="encrypt-data"></a>Verileri şifreleme

Azure Disk şifrelemesi denetlemeye yardımcı olmak ve disk şifreleme anahtarları ve gizli bir anahtar kasası Abonelikteki yönetmek için Azure anahtar kasası ile tümleşir. VM disklerdeki tüm veriler Azure depolama alanında REST şifrelenmesini sağlar.  

- Contoso belirli VM'ler şifreleme gerektir karar vermiştir.
- Müşteri, gizli, VM'ler için şifrelemeyle veya PPI veri geçerli olacak.


## <a name="conclusion"></a>Sonuç

Bu makalede, Contoso Azure kendi altyapısını ayarlamak ve ayarlayın veya Azure aboneliği için altyapı İlkesi planlanan, karma tanımlayın, olağanüstü durum kurtarma, ağ, idare ve güvenlik. 

Contoso burada tamamlandı adımların tümünü buluta geçiş için gerekli değildir. Kendi durumda bunlar tüm türleri geçişleri için kullanılabilir bir ağ altyapısını planlama istediğinizi ve güvenli, esnek ve ölçeklendirilebilir. 

Yerinde bu altyapı ile bunların geçmek ve geçiş deneyin hazırsınız demektir.

## <a name="next-steps"></a>Sonraki adımlar

İlk geçiş senaryosu Contoso giderek Azure'da VMware Vm'lerinde çalışan, şirket içi SmartHotel iki katmanlı uygulama geçirmek için. Yönetilen bir Azure SQL örneğine bunlar Azure vm'lerine app sanal makineleri ve uygulama veritabanı geçirmeniz.
