---
title: Uygulamaları Azure Active Directory'de SCIM'yi kullanma sağlanmasını otomatikleştirmek | Microsoft Docs
description: Azure Active Directory Kullanıcıları ve grupları SCIM Protokolü belirtiminde tanımlanan arabirimi ile bir web hizmeti tarafından fronted uygulama veya kimlik deposuna otomatik olarak sağlayabilirsiniz
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 12/12/2017
ms.author: barbkess
ms.reviewer: asmalser
ms.custom: aaddev;it-pro;seohack1
ms.openlocfilehash: 4247ef1ffd1b8d5c5ec393e3ebff20c3e04e32b3
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2018
ms.locfileid: "44347707"
---
# <a name="using-system-for-cross-domain-identity-management-scim-to-automatically-provision-users-and-groups-from-azure-active-directory-to-applications"></a>Kullanıcılar ve grupların Azure Active Directory'den uygulamalara otomatik olarak sağlamak için sistem etki alanları arası Kimlik Yönetimi (SCIM) kullanma

## <a name="overview"></a>Genel Bakış
Azure Active Directory (Azure AD) otomatik olarak kullanıcı sağlama ve tanımlanan arabirimi ile bir web hizmeti tarafından fronted herhangi bir uygulama veya kimlik deposu için gruplar [etki alanları arası Kimlik Yönetimi (SCIM) 2.0 protokolü için sistemi belirtimi](https://tools.ietf.org/html/draft-ietf-scim-api-19). Azure Active Directory oluşturma, değiştirme isteği gönderebilir veya web hizmetine silme atanan kullanıcı ve grupları. Web hizmeti bu istekleri hedef kimlik deposu işlemler ardından çevirebilir. 

![][0]
*Şekil 1: Bir web hizmeti aracılığıyla bir kimlik deposu için Azure Active Directory'den sağlama*

Bu özellik, Azure AD'de "kendi uygulamanızı getir" özelliği ile birlikte kullanılabilir. Bu özellik, çoklu oturum açma ve otomatik kullanıcı hazırlama SCIM web hizmeti tarafından fronted uygulamalar sağlar.

Azure Active Directory'de SCIM'yi kullanma için iki kullanım örnekleri vardır:

* **Kullanıcılar ve gruplar SCIM'yi destekleyen uygulamalar için sağlama** -2.0 SCIM'yi destekleyen ve bir yapılandırma olmadan Azure AD ile kimlik doğrulaması çalıştığı için OAuth taşıyıcı belirteçlerini kullanmak uygulamalar.
  
* **Uygulamalar için kendi sağlama çözümü oluşturma desteği diğer API tabanlı sağlama** -SCIM olmayan uygulamalar için Azure AD SCIM uç nokta ve uygulamanın desteklediği için herhangi bir API'yi arasında çeviri yapmak için bir SCIM uç noktası oluşturabilirsiniz. Kullanıcı sağlama. SCIM uç nokta geliştirmenize yardımcı olmak için ortak dil altyapısı (CLI) kitaplıkları birlikte SCIM uç noktası sağlayın ve SCIM iletileri çevirme işlemini gösteren kod örnekleri vardır.  

## <a name="provisioning-users-and-groups-to-applications-that-support-scim"></a>Kullanıcılar ve gruplar SCIM'yi destekleyen uygulamalar için hazırlama
Azure AD, otomatik olarak sağlama atanmış kullanıcılar ve grupları kullanan uygulamalar için yapılandırılabilir bir [etki alanları arası Kimlik Yönetimi 2 (SCIM) sistemi](https://tools.ietf.org/html/draft-ietf-scim-api-19) web hizmeti ve kimlik doğrulaması için OAuth taşıyıcı belirteçleri kabul edin. SCIM 2.0 belirtimini içinde uygulamalar, bu gereksinimleri karşılaması gerekir:

* Kullanıcılara ve/veya SCIM'yi Protokolü 3.3 bölümünü göre gruplar oluşturmayı destekler.  
* Kullanıcılara ve/veya SCIM'yi Protokolü 3.5.2 bölümünü göre patch isteklerinde gruplarıyla değiştirme destekler.  
* SCIM Protokolü 3.4.1 bölümünü göre bilinen bir kaynak alma destekler.  
* Kullanıcılara ve/veya SCIM'yi Protokolü 3.4.2 bölümünü göre gruplar sorgulanmasını destekler.  Varsayılan olarak, kullanıcılar tarafından externalID seçmeleri istenir ve grupları displayName tarafından sorgulanır.  
* Kullanıcı kimliği ve SCIM Protokolü 3.4.2 bölümünü göre Yöneticisi tarafından sorgulanmasını destekler.  
* Gruplar ve Kimliğe göre üye SCIM Protokolü 3.4.2 bölümünü göre sorgulanmasını destekler.  
* SCIM protokolünün yetkilendirme 2.1 bölüm uyarınca için OAuth taşıyıcı belirteçleri kabul eder.

Uygulama sağlayıcınıza veya bilgilerinin bu gereksinimleri ile uyumluluk için uygulama sağlayıcının belgelerine başvurun.

### <a name="getting-started"></a>Başlarken
Bu makalede açıklanan SCIM profilini destekleyen uygulamalar, Azure Active Directory Azure AD uygulama galerisinde bulunan "galeri dışı uygulama" özelliğini kullanarak bağlanabilir. Bağlantı kurulduktan sonra Azure AD eşitleme işlemi burada, uygulamanın SCIM uç noktası için atanan kullanıcılar ve gruplar, sorgular ve oluşturuyor veya bunları göre atama ayrıntıları 40 dakikada bir çalışır.

**SCIM'yi destekleyen bir uygulamaya bağlanmak için:**

1. Oturum [Azure portalında](https://portal.azure.com). 
2. Gözat **Azure Active Directory > Kurumsal uygulamalar**seçip **yeni uygulama > tüm > galeri dışı uygulama**.
3. Uygulamanız için bir ad girin ve tıklayın **Ekle** uygulama nesne oluşturmak için simge.
    
  ![][1]
  *Şekil 2: Azure AD uygulama Galerisi*
    
4. Sonuçta elde edilen ekranında seçin **sağlama** sol sütunda sekmesi.
5. İçinde **sağlama modu** menüsünde **otomatik**.
    
  ![][2]
  *Şekil 3: Azure portalında sağlama yapılandırma*
    
6. İçinde **Kiracı URL'si** uygulamanın SCIM uç nokta URL'sini girin. Örnek: https://api.contoso.com/scim/v2/
7. SCIM uç noktanın bir OAuth taşıyıcı belirtecinden bir veren Azure AD dışındaki gerektiriyorsa, gerekli OAuth taşıyıcı belirteci sonra isteğe bağlı kopyalayın **gizli belirteç** alan. Bu alan boş bırakılırsa, Azure AD OAuth taşıyıcı belirteci her isteği ile Azure AD tarafından verilen dahil. Azure AD kimlik sağlayıcısı bu Azure AD'ye doğrulayabilirsiniz olarak kullanan uygulamalar-belirteç.
8. Tıklayın **Test Bağlantısı** düğmesine sahip Azure Active Directory SCIM uç noktaya bağlanmayı deneyin. Denemesi başarısız olursa hata bilgileri görüntülenir.  
9. Uygulama başarılı olmasına bağlanmaya çalışırsa, ardından tıklatın **Kaydet** yönetici kimlik bilgilerini kaydetmek için.
10. İçinde **eşlemeleri** bölümünde, iki seçilebilir öznitelik eşlemelerini kümesi vardır: biri kullanıcı, nesneyi, diğeri için Grup nesneleri. Uygulamanızı Azure Active Directory'den eşitlenen öznitelikler gözden geçirmek için her birini seçin. Seçilen öznitelikler **eşleşen** özellikleri, kullanıcıları ve grupları güncelleştirme işlemleri için uygulamanızda eşleştirmek için kullanılır. Değişiklikleri kaydetmek için Kaydet düğmesini seçin.

    >[!NOTE]
    >İsteğe bağlı olarak, "eşleme grupları" devre dışı bırakarak Grup nesnelerini eşitlemeyi devre dışı bırakın. 

11. Altında **ayarları**, **kapsam** alanı grupları ve hangi kullanıcıların eşitleneceğini tanımlar. "Eşitleme yalnızca atanan kullanıcı ve grupları (önerilen)" seçerek kullanıcıları yalnızca eşitlendiğini ve atanan gruplar **kullanıcılar ve gruplar** sekmesi.
12. Yapılandırma tamamlandıktan sonra değiştirmek **sağlama durumu** için **üzerinde**.
13. Tıklayın **Kaydet** Azure AD sağlama hizmeti başlatılamadı. 
14. Eşitleme yalnızca kullanıcılar ve gruplar (önerilen) atadıysanız, seçtiğinizden emin olun **kullanıcılar ve gruplar** sekmesini ve kullanıcılara ve/veya eşitlemek istediğiniz grupları atayabilirsiniz.

İlk eşitleme başlatıldıktan sonra kullanabileceğiniz **denetim günlükleri** uygulamanızdan sağlama hizmeti tarafından gerçekleştirilen tüm eylemler gösteren ilerlemeyi izleme için sekmesinde. Azure AD günlüklerini sağlama okuma hakkında daha fazla bilgi için bkz. [hesabı otomatik kullanıcı hazırlama raporlama](check-status-user-account-provisioning.md).

>[!NOTE]
>İlk eşitleme hizmeti çalışıyor sürece yaklaşık 40 dakikada oluşan sonraki eşitlemeler uzun sürer. 


## <a name="building-your-own-provisioning-solution-for-any-application"></a>Herhangi bir uygulama için sağlama kendi çözümünüzü oluşturma
Azure Active Directory ile arabirimleri SCIM bir web hizmeti oluşturarak sağlayan bir REST veya SOAP kullanıcı API sağlama herhangi bir uygulama için çoklu oturum açma ve otomatik kullanıcı sağlamayı etkinleştirebilirsiniz.

Şekli aşağıda verilmiştir:

1. Azure AD adlı bir ortak dil altyapı kitaplığı sağlar [Microsoft.SystemForCrossDomainIdentityManagement](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/). Sistem Tümleştiriciler ve geliştiriciler bu kitaplığı oluşturmak ve bir SCIM tabanlı web hizmeti uç noktası için herhangi bir uygulamanın kimlik deposunu Azure AD'ye bağlanabilen dağıtmak için kullanabilirsiniz.
2. Eşlemeleri, web hizmeti, standart kullanıcı şeması uygulamanın gerektirdiği protokolü ve kullanıcı şeması eşlemek için uygulanır.
3. Uç nokta URL'si, Azure ad uygulama galerisinde özel bir uygulama bir parçası olarak kaydedilir.
4. Kullanıcıları ve grupları, Azure AD'de bu uygulama atanır. Atama sırasında bunlar hedef uygulama için eşitlenmesi gereken bir kuyruğun içine yerleştirilir. Sıranın işleme eşitleme işlemi 40 dakikada bir çalışır.

### <a name="code-samples"></a>Kod Örnekleri
Bu işlemi kolaylaştırmak için [kod örnekleri](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master) SCIM web hizmeti uç noktası oluşturun ve otomatik sağlama göstermek sağlanır. Kullanıcılar ve gruplar temsil eden virgülle ayrılmış değerler satırlarla dosya tutan bir sağlayıcısının bir örneği var.  Diğer Amazon Web Services kimlik ve erişim yönetimi hizmetini çalışır bir sağlayıcı tarafından kullanılır.  

**Önkoşullar**

* Visual Studio 2013 veya üzeri
* [.NET için Azure SDK](https://azure.microsoft.com/downloads/)
* SCIM uç noktası olarak kullanılacak ASP.NET framework 4. 5'i destekleyen Windows makinesi. Bu makine bulut üzerinden erişilebilir olması gerekir
* [Bir Azure aboneliği deneme sürümü ya da lisanslı bir Azure AD Premium sürümü ile](https://azure.microsoft.com/services/active-directory/)

### <a name="getting-started"></a>Başlarken
Azure ad sağlama isteklerini kabul edebilen bir SCIM uç noktası uygulamak için en kolay yolu, derleme ve dağıtma sağlanan kullanıcılar bir virgülle ayrılmış değer (CSV) dosyasına çıkarır kodu örneği sağlamaktır.

**Bir örnek SCIM uç noktası oluşturmak için:**

1. Kod örnek paketini [https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master)
2. Paketin sıkıştırmasını açın ve Windows makinenizde C:\AzureAD-BYOA-Provisioning-Samples\ gibi bir konuma yerleştirin.
3. Bu klasörde, Visual Studio'da FileProvisioning\Host\FileProvisioningService.csproj projesi başlatın.
4. Seçin **Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**ve çözüm başvurularını çözümlemek FileProvisioningService projesi için aşağıdaki komutları yürütün:
  ```` 
   Update-Package -Reinstall
  ````
5. FileProvisioningService projeyi derleyin.
6. Windows komut istemi uygulamasını (Yönetici) olarak açmak ve kullanmak **cd** komut için dizini değiştirmek için **\AzureAD-BYOA-Provisioning-Samples\FileProvisioning\Host\bin\Debug**klasör.
7. < IP adresi > IP adresine veya etki alanı adını Windows makinesi ile değiştirerek aşağıdaki komutu çalıştırın:
  ````   
   FileSvc.exe http://<ip-address>:9000 TargetFile.csv
  ````
8. Windows altında **Windows Ayarları > Ağ ve Internet ayarları**seçin **Windows Güvenlik Duvarı > Gelişmiş ayarlar**, oluşturup bir **gelen kuralı** , 9000 numaralı bağlantı noktasına gelen erişim sağlar.
9. Windows makine bir yönlendiricinin arkasında ise, yönlendirici, bağlantı noktası İnternet'e 9000 ve bağlantı noktası 9000 Windows makinesinde arasında ağ erişim dönüşümü gerçekleştirmek için yapılandırılması gerekir. Bu yapılandırma, bulutta Bu uç noktaya erişebilmesi Azure AD için gereklidir.

**Azure AD'de örnek SCIM uç nokta kaydetmek için:**

1. Oturum [Azure portalında](https://portal.azure.com). 
2. Gözat **Azure Active Directory > Kurumsal uygulamalar**seçip **yeni uygulama > tüm > galeri dışı uygulama**.
3. Uygulamanız için bir ad girin ve tıklayın **Ekle** uygulama nesne oluşturmak için simge. Oluşturulan uygulama nesnesi, için sağlama ve uygulama için çoklu oturum açmayı ve yalnızca SCIM uç noktası hedef uygulamayı temsil etmek üzere tasarlanmıştır.
4. Sonuçta elde edilen ekranında seçin **sağlama** sol sütunda sekmesi.
5. İçinde **sağlama modu** menüsünde **otomatik**.
    
  ![][2]
  *Şekil 4: Azure portalında sağlama yapılandırma*
    
6. İçinde **Kiracı URL'si** internet açık URL ve bağlantı noktası SCIM uç noktanızı girin. Giriş aşağıdakine benzer olan http://testmachine.contoso.com:9000 veya internet olduğu < IP adresi > http://<ip-address>:9000/, kullanıma sunulan IP adresi.  
7. SCIM uç noktanın bir OAuth taşıyıcı belirtecinden bir veren Azure AD dışındaki gerektiriyorsa, gerekli OAuth taşıyıcı belirteci sonra isteğe bağlı kopyalayın **gizli belirteç** alan. Bu alan boş bırakılırsa, Azure AD her isteği ile Azure AD tarafından verilen bir OAuth taşıyıcı belirtecini içerir. Azure AD kimlik sağlayıcısı bu Azure AD'ye doğrulayabilirsiniz olarak kullanan uygulamalar-belirteç.
8. Tıklayın **Test Bağlantısı** düğmesine sahip Azure Active Directory SCIM uç noktaya bağlanmayı deneyin. Denemesi başarısız olursa hata bilgileri görüntülenir.  
9. Uygulama başarılı olmasına bağlanmaya çalışırsa, ardından tıklatın **Kaydet** yönetici kimlik bilgilerini kaydetmek için.
10. İçinde **eşlemeleri** bölümünde, iki seçilebilir öznitelik eşlemelerini kümesi vardır: biri kullanıcı, nesneyi, diğeri için Grup nesneleri. Uygulamanızı Azure Active Directory'den eşitlenen öznitelikler gözden geçirmek için her birini seçin. Seçilen öznitelikler **eşleşen** özellikleri, kullanıcıları ve grupları güncelleştirme işlemleri için uygulamanızda eşleştirmek için kullanılır. Değişiklikleri kaydetmek için Kaydet düğmesini seçin.
11. Altında **ayarları**, **kapsam** alanı grupları ve hangi kullanıcıların eşitleneceğini tanımlar. "Eşitleme yalnızca atanan kullanıcı ve grupları (önerilen)" seçerek kullanıcıları yalnızca eşitlendiğini ve atanan gruplar **kullanıcılar ve gruplar** sekmesi.
12. Yapılandırma tamamlandıktan sonra değiştirmek **sağlama durumu** için **üzerinde**.
13. Tıklayın **Kaydet** Azure AD sağlama hizmeti başlatılamadı. 
14. Eşitleme yalnızca kullanıcılar ve gruplar (önerilen) atadıysanız, seçtiğinizden emin olun **kullanıcılar ve gruplar** sekmesini ve kullanıcılara ve/veya eşitlemek istediğiniz grupları atayabilirsiniz.

İlk eşitleme başlatıldıktan sonra kullanabileceğiniz **denetim günlükleri** uygulamanızdan sağlama hizmeti tarafından gerçekleştirilen tüm eylemler gösteren ilerlemeyi izleme için sekmesinde. Azure AD günlüklerini sağlama okuma hakkında daha fazla bilgi için bkz. [hesabı otomatik kullanıcı hazırlama raporlama](check-status-user-account-provisioning.md).

Örnek doğrulama son adım, Windows makinenizde \AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug klasöründeki TargetFile.csv dosya açmaktır. Sağlama işlemi çalıştırıldıktan sonra bu dosya tüm ayrıntılarını atanan ve kullanıcıları ve grupları sağlanan gösterir.

### <a name="development-libraries"></a>Geliştirme kitaplıkları
SCIM Belirtimi'ne kendi web hizmeti geliştirmek için önce aşağıdaki kitaplıklar geliştirme süreci hızlandırmak için Microsoft tarafından sağlanan tanıyın: 

1. Ortak dil altyapısı (CLI) kitaplıkları, C# gibi bu altyapısının temel dilleri ile kullanım için sunulur. Bu kitaplıklar birini [Microsoft.SystemForCrossDomainIdentityManagement.Service](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/), aşağıdaki çizimde gösterilen bir arabirim Microsoft.SystemForCrossDomainIdentityManagement.IProvider, bildirir: A Bu arabirim için genel bir sağlayıcı olarak başvurulabilir bir sınıf kitaplıkları kullanarak Geliştirici uygulayabilir. SCIM belirtimine uyan bir web hizmetini dağıtmak geliştirici kitaplıkları sağlar. Web hizmeti Internet Information Services veya herhangi bir yürütülebilir ortak dil altyapısı derleme içinde ya da barındırılabilir. İstek, geliştirici tarafından bazı kimlik deposu üzerinde çalışılacak programlanmak sağlayıcının yöntemlere yapılan çağrılar veri dönüştürülür.
  
  ![][3]
  
2. [Express route işleyicileri](http://expressjs.com/guide/routing.html) bir node.js web hizmeti çağrıları (SCIM belirtimi tarafından tanımlanan) temsil eden node.js istek nesneleri ayrıştırmak için kullanılabilir yapılır.   

### <a name="building-a-custom-scim-endpoint"></a>Bir özel SCIM uç noktası oluşturma
CLI kitaplıklarını kullanarak, bu kitaplıkları kullanan geliştiriciler, Internet Information Services veya herhangi bir yürütülebilir ortak dil altyapısı derleme içinde hizmetlerini barındırabilirler. İşte adresten yürütülebilir bir derleme içinde bir hizmet barındırma için örnek kod http://localhost:9000: 

    private static void Main(string[] arguments)
    {
    // Microsoft.SystemForCrossDomainIdentityManagement.IMonitor, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IProvider and 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service are all defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service.dll.  

    Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor = 
      new DevelopersMonitor();
    Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider = 
      new DevelopersProvider(arguments[1]);
    Microsoft.SystemForCrossDomainIdentityManagement.Service webService = null;
    try
    {
        webService = new WebService(monitor, provider);
        webService.Start("http://localhost:9000");

        Console.ReadKey(true);
    }
    finally
    {
        if (webService != null)
        {
            webService.Dispose();
            webService = null;
        }
    }
    }

    public class WebService : Microsoft.SystemForCrossDomainIdentityManagement.Service
    {
    private Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor;
    private Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider;

    public WebService(
      Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitoringBehavior, 
      Microsoft.SystemForCrossDomainIdentityManagement.IProvider providerBehavior)
    {
        this.monitor = monitoringBehavior;
        this.provider = providerBehavior;
    }

    public override IMonitor MonitoringBehavior
    {
        get
        {
            return this.monitor;
        }

        set
        {
            this.monitor = value;
        }
    }

    public override IProvider ProviderBehavior
    {
        get
        {
            return this.provider;
        }

        set
        {
            this.provider = value;
        }
    }
    }

Bu hizmet kök sertifika yetkilisi şu adlardan biri olduğu bir HTTP adresi ve sunucu kimlik doğrulama sertifikasında şunlar olmalıdır: 

* CNNIC
* Comodo
* CyberTrust
* DigiCert
* GeoTrust
* GlobalSign
* Go Daddy
* VeriSign
* WoSign

Bir sunucu kimlik doğrulama sertifikası için bir bağlantı noktası ağ Kabuğu yardımcı programını kullanarak Windows konağında bağlanabilir: 

    netsh http add sslcert ipport=0.0.0.0:443 certhash=0000000000003ed9cd0c315bbb6dc1c08da5e6 appid={00112233-4455-6677-8899-AABBCCDDEEFF}  

Rastgele bir genel benzersiz tanımlayıcı olsa da AppID bağımsız değişkeni için sağlanan değer, burada certhash bağımsız değişkeni için sağlanan sertifikanın parmak izini değerdir.  

Internet Information Services hizmetinde barındırmak için bir geliştirici bir CLA kod kitaplığı başlangıç derleme varsayılan ad alanını adlı bir sınıf ile derleme.  Böyle bir sınıfın bir örneği aşağıdadır: 

    public class Startup
    {
    // Microsoft.SystemForCrossDomainIdentityManagement.IWebApplicationStarter, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IMonitor and  
    // Microsoft.SystemForCrossDomainIdentityManagement.Service are all defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service.dll.  

    Microsoft.SystemForCrossDomainIdentityManagement.IWebApplicationStarter starter;

    public Startup()
    {
        Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor = 
          new DevelopersMonitor();
        Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider = 
          new DevelopersProvider();
        this.starter = 
          new Microsoft.SystemForCrossDomainIdentityManagement.WebApplicationStarter(
            provider, 
            monitor);
    }

    public void Configuration(
      Owin.IAppBuilder builder) // Defined in Owin.dll.  
    {
        this.starter.ConfigureApplication(builder);
    }
    }

### <a name="handling-endpoint-authentication"></a>İşleme uç nokta kimlik doğrulaması
Azure Active Directory gelen istekleri bir OAuth 2.0 taşıyıcı belirteci içerir.   İstek alma herhangi bir hizmet olarak Azure Active Directory, Azure Active Directory Graph'i web hizmetine erişim için beklenen Azure Active Directory kiracısı adına veren kimlik doğrulamalıdır.  Belirteci, verenin "ISS" gibi bir ISS talep tanımlanır: "https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/".  Bu örnekte, talep değeri, temel adresini https://sts.windows.net, göreli adres sırada cbb1a5ac-f33b-45fa-9bf5-f37db0fed422 segment veren, Azure Active Directory tanımlar, şirket Azure Active Directory kiracısının benzersiz bir tanımlayıcıdır. belirteç verildiği adına.  Azure Active Directory Graph'i web hizmetine erişmek için belirteç verildiyse 00000002-0000-0000-c000-000000000000, bu hizmet tanımlayıcısı belirtecin aud talep değerine olması gerekir.  

SCIM hizmeti oluşturmak için Microsoft tarafından sağlanan CLA kitaplıkları kullanan geliştiriciler Microsoft.Owin.Security.ActiveDirectory paket, aşağıdaki adımları kullanarak Azure Active Directory isteklerinden kimlik doğrulaması yapabilir: 

1. Sağlayıcı, bu hizmeti başlatıldığında çağrılacak bir yöntem dönüş sağlayarak Microsoft.SystemForCrossDomainIdentityManagement.IProvider.StartupBehavior özelliği uygulayın: 

  ````
    public override Action\<Owin.IAppBuilder, System.Web.Http.HttpConfiguration.HttpConfiguration\> StartupBehavior
    {
      get
      {
        return this.OnServiceStartup;
      }
    }

    private void OnServiceStartup(
      Owin.IAppBuilder applicationBuilder,  // Defined in Owin.dll.  
      System.Web.Http.HttpConfiguration configuration)  // Defined in System.Web.Http.dll.  
    {
    }
  ````

2. Herhangi bir hizmet uç noktaları Azure AD Graph web hizmetine erişim için belirtilen bir kiracı adına Azure Active Directory tarafından verilmiş bir belirteç pul olarak kimlik doğrulaması yapılan tüm istekleri için bu yönteme aşağıdaki kodu ekleyin: 

  ````
    private void OnServiceStartup(
      Owin.IAppBuilder applicationBuilder IAppBuilder applicationBuilder, 
      System.Web.Http.HttpConfiguration HttpConfiguration configuration)
    {
      // IFilter is defined in System.Web.Http.dll.  
      System.Web.Http.Filters.IFilter authorizationFilter = 
        new System.Web.Http.AuthorizeAttribute(); // Defined in System.Web.Http.dll.configuration.Filters.Add(authorizationFilter);

      // SystemIdentityModel.Tokens.TokenValidationParameters is defined in    
      // System.IdentityModel.Token.Jwt.dll.
      SystemIdentityModel.Tokens.TokenValidationParameters tokenValidationParameters =     
        new TokenValidationParameters()
        {
          ValidAudience = "00000002-0000-0000-c000-000000000000"
        };

      // WindowsAzureActiveDirectoryBearerAuthenticationOptions is defined in 
      // Microsoft.Owin.Security.ActiveDirectory.dll
      Microsoft.Owin.Security.ActiveDirectory.
      WindowsAzureActiveDirectoryBearerAuthenticationOptions authenticationOptions =
        new WindowsAzureActiveDirectoryBearerAuthenticationOptions()    {
        TokenValidationParameters = tokenValidationParameters,
        Tenant = "03F9FCBC-EA7B-46C2-8466-F81917F3C15E" // Substitute the appropriate tenant’s 
                                                      // identifier for this one.  
      };

      applicationBuilder.UseWindowsAzureActiveDirectoryBearerAuthentication(authenticationOptions);
    }
  ````


## <a name="user-and-group-schema"></a>Kullanıcı ve Grup şeması
Azure Active Directory kaynakları SCIM web hizmetleri için iki tür sağlayabilirsiniz.  Bu kaynak türleri, kullanıcılar ve gruplar şunlardır.  

Kullanıcı kaynaklarını, "urn: Bu protokolü belirtimi içinde yer alan ietf:params:scim:schemas:extension:enterprise:2.0:User", şema tanımlayıcısı tarafından tanımlanır: http://tools.ietf.org/html/draft-ietf-scim-core-schema.  Varsayılan eşleme "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User" kaynakları özniteliklerini Azure Active Directory'de kullanıcılara özniteliklerinin tabloda 1, aşağıda verilmiştir.  

Grup kaynaklarının şema tanımlayıcısı tarafından tanımlanan http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.  Varsayılan eşleme öznitelikleri için Azure Active Directory'de grupları özniteliklerinin 2, aşağıda gösterildiği tablo http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group kaynakları.  

### <a name="table-1-default-user-attribute-mapping"></a>Tablo 1: Varsayılan kullanıcı öznitelik eşlemesi
| Azure Active Directory kullanıcısı | "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User" |
| --- | --- |
| IsSoftDeleted |etkin |
| displayName |displayName |
| Faks TelephoneNumber |PhoneNumber [türü eq "faks"] .value |
| givenName |name.givenName |
| İş Unvanı |başlık |
| posta |e-postaları [türü eq "İş"] .value |
| mailNickname |externalId |
| yönetici |yönetici |
| Mobil |PhoneNumber [türü eq "mobile"] .value |
| objectId |Kimlik |
| posta kodu |adresler [türü eq "İş"] .postalCode |
| Ara sunucu adresleri |e-postaları [type eq "diğer"]. Değer |
| fiziksel teslim OfficeName |adresler [type eq "diğer"]. Biçimlendirilmiş |
| streetAddress |adresler [türü eq "İş"] .streetAddress |
| Soyadı |name.familyName |
| telefon numarası |PhoneNumber [türü eq "İş"] .value |
| Kullanıcı PrincipalName |Kullanıcı adı |

### <a name="table-2-default-group-attribute-mapping"></a>Tablo 2: Varsayılan grup öznitelik eşlemesi
| Azure Active Directory grubu | http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group |
| --- | --- |
| displayName |externalId |
| posta |e-postaları [türü eq "İş"] .value |
| mailNickname |displayName |
| üyeler |üyeler |
| objectId |Kimlik |
| proxyAddresses |e-postaları [type eq "diğer"]. Değer |

## <a name="user-provisioning-and-de-provisioning"></a>Kullanıcı hazırlama ve sağlamayı
Aşağıdaki çizimde gösterildiği Azure Active Directory kullanıcı başka bir kimlik deposu olarak ömrünü yönetmek için SCIM'yi hizmete gönderdiği iletileri. Diyagram ayrıca nasıl CLI kitaplıkları kullanılarak uygulanan SCIM sağlanan bir hizmeti Microsoft tarafından yapı için tür hizmetlerin bir sağlayıcısının yöntemlere yapılan çağrılar isteklere küçültmesini gösterir.  

![][4]
*Şekil 5: Kullanıcı hazırlama ve sağlamayı dizisi*

1. Azure Active Directory, Azure AD'de kullanıcı mailNickname özniteliğinin değeri ile eşleşen bir externalID öznitelik değeri olan bir kullanıcı için hizmet sorgular. Sorgu, burada görüntülerle jyoung bir kullanıcının Azure Active Directory'de bir mailNickname örneğidir, bu örnek gibi bir Köprü Metni Aktarım Protokolü (HTTP) isteği olarak ifade edilir: 
  ````
    GET https://.../scim/Users?filter=externalId eq jyoung HTTP/1.1
    Authorization: Bearer ...
  ````
  Hizmet SCIM Hizmetleri uygulamak için Microsoft tarafından sağlanan ortak dil altyapısı kitaplıklar kullanılarak oluşturulduysa, isteği hizmet sağlayıcısının sorgu yöntemine bir çağrı çevrilir.  Aşağıda, bu yöntem imzası verilmiştir: 
  ````
    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Protocol.  

    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource[]> Query(
      Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters parameters, 
      string correlationIdentifier);
  ````
  Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters arabirim tanımı aşağıda verilmiştir: 
  ````
    public interface IQueryParameters: 
      Microsoft.SystemForCrossDomainIdentityManagement.IRetrievalParameters
    {
        System.Collections.Generic.IReadOnlyCollection <Microsoft.SystemForCrossDomainIdentityManagement.IFilter> AlternateFilters 
        { get; }
    }

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IRetrievalParameters
    {
      system.Collections.Generic.IReadOnlyCollection<string> ExcludedAttributePaths 
      { get; }
      System.Collections.Generic.IReadOnlyCollection<string> RequestedAttributePaths 
      { get; }
      string SchemaIdentifier 
      { get; }
    }

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IFilter
    {
        Microsoft.SystemForCrossDomainIdentityManagement.IFilter AdditionalFilter 
          { get; set; }
        string AttributePath 
          { get; } 
        Microsoft.SystemForCrossDomainIdentityManagement.ComparisonOperator FilterOperator 
          { get; }
        string ComparisonValue 
          { get; }
    }

    public enum Microsoft.SystemForCrossDomainIdentityManagement.ComparisonOperator
    {
        Equals
    }
  ````
  Aşağıdaki örnekte bir kullanıcı için bir sorgunun externalID özniteliği için belirtilen bir değerle sorgu yöntemine geçirilen bağımsız değişkenlerin değerleri şunlardır: 
  * Parametreler. AlternateFilters.Count: 1
  * Parametreler. AlternateFilters.ElementAt(0). AttributePath: "externalID ="
  * Parametreler. AlternateFilters.ElementAt(0). ComparisonOperator: ComparisonOperator.Equals
  * Parametreler. AlternateFilter.ElementAt(0). ComparisonValue: "jyoung"
  * correlationIdentifier: System.Net.Http.HttpRequestMessage.GetOwinEnvironment["owin. RequestId"] 

2. Web hizmeti bir kullanıcı mailNickname özniteliğinin değeri ile eşleşen bir externalID öznitelik değeri olan bir kullanıcı için bir sorguya yanıt herhangi bir kullanıcı döndürmezse, ardından Azure Active Directory Hizmet birine karşılık gelen bir kullanıcı sağlama istekleri Azure Active Directory'de.  Böyle bir isteğin bir örnek aşağıda verilmiştir: 
  ````
    POST https://.../scim/Users HTTP/1.1
    Authorization: Bearer ...
    Content-type: application/scim+json
    {
      "schemas":
      [
        "urn:ietf:params:scim:schemas:core:2.0:User",
        "urn:ietf:params:scim:schemas:extension:enterprise:2.0User"],
      "externalId":"jyoung",
      "userName":"jyoung",
      "active":true,
      "addresses":null,
      "displayName":"Joy Young",
      "emails": [
        {
          "type":"work",
          "value":"jyoung@Contoso.com",
          "primary":true}],
      "meta": {
        "resourceType":"User"},
       "name":{
        "familyName":"Young",
        "givenName":"Joy"},
      "phoneNumbers":null,
      "preferredLanguage":null,
      "title":null,
      "department":null,
      "manager":null}
  ````
  SCIM Hizmetleri uygulamak için Microsoft tarafından sağlanan ortak dil altyapısı kitaplıkları, hizmet sağlayıcısı oluşturma yöntemine bir çağrı isteği çevirir.  Oluştur yönteminin bu imzaya sahip: 
  ````
    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  

    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource> Create(
      Microsoft.SystemForCrossDomainIdentityManagement.Resource resource, 
      string correlationIdentifier);
  ````
  Bir kullanıcı sağlamak için istekte kaynak bağımsız değişkenin değeri Microsoft.SystemForCrossDomainIdentityManagement örneğidir. Microsoft.SystemForCrossDomainIdentityManagement.Schemas Kitaplığı'nda tanımlanan Core2EnterpriseUser sınıfı.  Kullanıcı sağlama isteği başarılı olursa, yöntemin uygulanmasını Microsoft.SystemForCrossDomainIdentityManagement örneğini döndürmesi beklenir. Yeni sağlanan kullanıcının benzersiz tanımlayıcısı için ayarlanmış tanımlayıcı özelliğinin değeri ile Core2EnterpriseUser sınıfı.  

3. Bilinen bir kimlik deposu bir SCIM fronted mevcut kullanıcıyı güncelleştirmek için Azure Active Directory gibi bir isteği ile hizmetten kullanıcının geçerli durumunu isteyerek çalışır: 
  ````
    GET ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
  ````
  SCIM Hizmetleri uygulamak için Microsoft tarafından sağlanan ortak dil altyapısı kitaplıklar kullanılarak bir hizmette, hizmetin sağlayıcısının alma yöntemine bir çağrı isteği çevrilir.  Al yönteminin imzası şu şekildedir: 
  ````
    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource and 
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters 
    // are defined in Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  
    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource> 
       Retrieve(
         Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters 
           parameters, 
           string correlationIdentifier);

    public interface 
      Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters:   
        IRetrievalParameters
        {
          Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier 
            ResourceIdentifier 
              { get; }
    }
    public interface Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier
    {
        string Identifier 
          { get; set; }
        string Microsoft.SystemForCrossDomainIdentityManagement.SchemaIdentifier 
          { get; set; }
    }
  ````
  Bir kullanıcının geçerli durumunu almak için bir istek örnekte parametreleri bağımsız değişkenin değeri sağlanan nesne özelliklerinin değerleri aşağıdaki gibidir: 
  
  * Tanımlayıcısı: "54D382A4-2050-4C03-94D1-E769F1D15682"
  * SchemaIdentifier: "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User"

4. Azure Active Directory kimlik deposu olarak başvuru özniteliğinin geçerli değeri hizmet tarafından zaten fronted olup olmadığını belirlemek üzere hizmeti sorgular ardından güncelleştirilmesi için bir başvuru özniteliği ise bu özniteliği Azure Active değerle eşleşir Dizin. Kullanıcılar için bu şekilde sorgulanan geçerli değeri yalnızca öznitelik manager özniteliğidir. Belirli kullanıcı nesnesinin manager özniteliği şu anda belirli bir değere sahip olup olmadığını belirlemek için bir isteğin bir örnek aşağıda verilmiştir: 
  ````
    GET ~/scim/Users?filter=id eq 54D382A4-2050-4C03-94D1-E769F1D15682 and manager eq 2819c223-7f76-453a-919d-413861904646&attributes=id HTTP/1.1
    Authorization: Bearer ...
  ````
  Filtre sorgu parametresi değeri olarak sağlanan ifade karşılayan bir kullanıcı nesnesi varsa "ID" öznitelikleri sorgu parametresi değeri olduğunu belirtir ve hizmet yanıt beklenen bir "urn: ietf:params:scim:schemas:core:2.0: Yalnızca o kaynağın "ID" özniteliğinin değerini de dahil olmak üzere kullanıcı"veya"urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"kaynak.  Değerini **kimliği** istek sahibine bilinen öznitelik. Filtre sorgu parametresinin değerine dahil, Filtre ifadesi olsun veya olmasın herhangi bir nesne var. bir gösterge olarak çağıran bir en az bir kaynağın gösteriminin istemek üzere söylemelisiniz amacı gerçekten var.   

  Hizmet SCIM Hizmetleri uygulamak için Microsoft tarafından sağlanan ortak dil altyapısı kitaplıklar kullanılarak oluşturulduysa, isteği hizmet sağlayıcısının sorgu yöntemine bir çağrı çevrilir. Parametre bağımsız değişkenin değeri sağlanan nesne özelliklerini değerini aşağıdaki gibidir: 
  
  * Parametreler. AlternateFilters.Count: 2
  * Parametreler. AlternateFilters.ElementAt(x). AttributePath: "ID"
  * Parametreler. AlternateFilters.ElementAt(x). ComparisonOperator: ComparisonOperator.Equals
  * Parametreler. AlternateFilter.ElementAt(x). ComparisonValue: "54D382A4-2050-4C03-94D1-E769F1D15682"
  * Parametreler. AlternateFilters.ElementAt(y). AttributePath: "Yönetici"
  * Parametreler. AlternateFilters.ElementAt(y). ComparisonOperator: ComparisonOperator.Equals
  * Parametreler. AlternateFilter.ElementAt(y). ComparisonValue: "2819c223-7f76-453a-919d-413861904646"
  * Parametreler. RequestedAttributePaths.ElementAt(0): "ID"
  * Parametreler. SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"

  Burada, dizinin x değeri 0 olabilir ve dizin y değeri 1 ' olabilir veya 1 x değeri olabilir ve y değeri 0, filtre sorgu parametresinin ifadelerin düzene bağlı olarak olabilir.   

5. İsteğinin bir örneği bir Azure Active Directory'den kullanıcıyı güncelleştirmek için SCIM'yi hizmetine şöyledir: 
  ````
    PATCH ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
    Content-type: application/scim+json
    {
      "schemas": 
      [
        "urn:ietf:params:scim:api:messages:2.0:PatchOp"],
      "Operations":
      [
        {
          "op":"Add",
          "path":"manager",
          "value":
            [
              {
                "$ref":"http://.../scim/Users/2819c223-7f76-453a-919d-413861904646",
                "value":"2819c223-7f76-453a-919d-413861904646"}]}]}
  ````
  SCIM Hizmetleri uygulamak için Microsoft ortak dil altyapısı kitaplıkları hizmet sağlayıcısının güncelleştirme yöntemine bir çağrı isteği çevirir. Güncelleştirme metodun imzası şu şekildedir: 
  ````
    // System.Threading.Tasks.Tasks and 
    // System.Collections.Generic.IReadOnlyCollection<T>
    // are defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IPatch, 
    // Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
    // Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation, 
    // Microsoft.SystemForCrossDomainIdentityManagement.OperationName, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IPath and 
    // Microsoft.SystemForCrossDomainIdentityManagement.OperationValue 
    // are all defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 

    System.Threading.Tasks.Task Update(
      Microsoft.SystemForCrossDomainIdentityManagement.IPatch patch, 
      string correlationIdentifier);

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IPatch
    {
    Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase 
      PatchRequest 
        { get; set; }
    Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier 
      ResourceIdentifier 
        { get; set; }        
    }

    public class PatchRequest2: 
      Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase
    {
    public System.Collections.Generic.IReadOnlyCollection
      <Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation> 
        Operations
        { get;}

    public void AddOperation(
      Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation operation);
    }

    public class PatchOperation
    {
    public Microsoft.SystemForCrossDomainIdentityManagement.OperationName 
      Name
      { get; set; }

    public Microsoft.SystemForCrossDomainIdentityManagement.IPath 
      Path
      { get; set; }

    public System.Collections.Generic.IReadOnlyCollection
      <Microsoft.SystemForCrossDomainIdentityManagement.OperationValue> Value
      { get; }

    public void AddValue(
      Microsoft.SystemForCrossDomainIdentityManagement.OperationValue value);
    }

    public enum OperationName
    {
      Add,
      Remove,
      Replace
    }

    public interface IPath
    {
      string AttributePath { get; }
      System.Collections.Generic.IReadOnlyCollection<IFilter> SubAttributes { get; }
      Microsoft.SystemForCrossDomainIdentityManagement.IPath ValuePath { get; }
    }

    public class OperationValue
    {
      public string Reference
      { get; set; }

      public string Value
      { get; set; }
    }
  ````
    Kullanıcıyı güncelleştirmek için bir istek örnekte düzeltme eki bağımsız değişkenin değeri sağlanan nesne bu özelliği değer vardır: 
  
  * ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"
  * ResourceIdentifier.SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"
  * (PatchRequest PatchRequest2 olarak). Operations.Count: 1
  * (PatchRequest PatchRequest2 olarak). Operations.ElementAt(0). OperationName: OperationName.Add
  * (PatchRequest PatchRequest2 olarak). Operations.ElementAt(0). Path.AttributePath: "Yönetici"
  * (PatchRequest PatchRequest2 olarak). Operations.ElementAt(0). Value.Count: 1
  * (PatchRequest PatchRequest2 olarak). Operations.ElementAt(0). Value.ElementAt(0). Başvuru: http://.../scim/Users/2819c223-7f76-453a-919d-413861904646
  * (PatchRequest PatchRequest2 olarak). Operations.ElementAt(0). Value.ElementAt(0). Değer: 2819c223-7f76-453a-919d-413861904646

6. Bir kullanıcı bir SCIM hizmeti tarafından fronted kimlik mağazadan sağlamasını için Azure AD gibi bir istek gönderir: 
  ````
    DELETE ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
  ````
  Hizmet SCIM Hizmetleri uygulamak için Microsoft tarafından sağlanan ortak dil altyapısı kitaplıklar kullanılarak oluşturulduysa, isteği hizmet sağlayıcısının silme yöntemine bir çağrı çevrilir.   Bu yöntem, bu imzaya sahip: 
  ````
    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
    // is defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 
    System.Threading.Tasks.Task Delete(
      Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier  
        resourceIdentifier, 
      string correlationIdentifier);
  ````
  Resourceıdentifier bağımsız değişkenin değeri sağlanan nesne bir isteğin kullanıcı sağlamasını örnekte bu özellik değerleri vardır: 
  
  * ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"
  * ResourceIdentifier.SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"

## <a name="group-provisioning-and-de-provisioning"></a>Grup sağlama ve sağlamayı
Aşağıdaki çizimde gösterildiği Azure AcD bir grubu başka bir kimlik deposu içinde yaşam döngüsü yönetmek için SCIM'yi hizmete gönderdiği iletileri.  Bu iletileri kullanıcılara üç yolla ilgili iletileri farklıdır: 

* Bir grup kaynak şemasını olarak tanımlanan `http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group`.  
* Grupları almak için istekleri isteğine yanıt olarak sağlanan herhangi bir kaynaktan çıkarılacak üyeleri öznitelik olduğunu gidemez.  
* Bir başvuru özniteliği, belirli bir değere sahip olup olmadığını belirlemek üzere istekleri üyeleri özniteliği hakkında isteklerdir.  

![][5]
*Şekil 6: hazırlama ve sağlamayı dizisi grubu*

## <a name="related-articles"></a>İlgili makaleler
* [Azure Active Directory'de Uygulama Yönetimi için Makale Dizini](../active-directory-apps-index.md)
* [Kullanıcı sağlama/sağlamayı kaldırma SaaS uygulamaları için otomatik hale getirin](user-provisioning.md)
* [Kullanıcı sağlama için öznitelik eşlemelerini özelleştirme](customize-application-attributes.md)
* [Öznitelik eşlemeleri için ifadeler yazma](functions-for-customizing-application-data.md)
* [Kullanıcı sağlama için kapsam oluşturma filtresi](define-conditional-rules-for-provisioning-user-accounts.md)
* [Hesap sağlama bildirimleri](user-provisioning.md)
* [SaaS uygulamalarını tümleştirme hakkında öğreticiler listesi](../saas-apps/tutorial-list.md)

<!--Image references-->
[0]: ./media/use-scim-to-provision-users-and-groups/scim-figure-1.png
[1]: ./media/use-scim-to-provision-users-and-groups/scim-figure-2a.png
[2]: ./media/use-scim-to-provision-users-and-groups/scim-figure-2b.png
[3]: ./media/use-scim-to-provision-users-and-groups/scim-figure-3.png
[4]: ./media/use-scim-to-provision-users-and-groups/scim-figure-4.png
[5]: ./media/use-scim-to-provision-users-and-groups/scim-figure-5.png
