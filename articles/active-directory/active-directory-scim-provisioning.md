---
title: "Etki alanları arası Identity Management otomatik olarak sağlamak için kullanıcıları ve grupları Azure Active Directory'den uygulamalara sistemiyle | Microsoft Docs"
description: "Azure Active Directory Kullanıcıları ve grupları SCIM'yi protokolü belirtimi içinde tanımlı arabirimi ile bir web hizmeti tarafından fronted uygulama ya da kimliği deposuna otomatik olarak sağlayabilirsiniz"
services: active-directory
documentationcenter: 
author: asmalser-msft
manager: mtillman
editor: 
ms.assetid: 4d86f3dc-e2d3-4bde-81a3-4a0e092551c0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/12/2017
ms.author: asmalser
ms.reviewer: asmalser
<<<<<<< HEAD
ms.custom: aaddev;it-pro;oldportal
ms.openlocfilehash: 91978cee88d55c99bcb63c63cdaf01581ae84668
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
=======
ms.custom: aaddev;it-pro
ms.openlocfilehash: 82649b0da67882a0088876798b6f0d79e46051a7
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2017
---
# <a name="using-system-for-cross-domain-identity-management-to-automatically-provision-users-and-groups-from-azure-active-directory-to-applications"></a>Kullanıcıları ve grupları Azure Active Directory'den uygulamalara otomatik olarak sağlamak için etki alanları arası kimlik yönetimi sistemi kullanarak

## <a name="overview"></a>Genel Bakış
Azure Active Directory (Azure AD) otomatik olarak kullanıcı sağlama ve tanımlanan gruplar arabirimi ile bir web hizmeti tarafından fronted uygulama ya da kimliği deposuna [sistemi etki alanları arası Kimlik Yönetimi (SCIM'yi) 2.0 protokolü belirtimi için](https://tools.ietf.org/html/draft-ietf-scim-api-19). Delete kullanıcılar ve gruplar web hizmetine atanan veya Azure Active Directory oluşturmak, değiştirmek için istek gönderemez. Web hizmeti, hedef kimlik deposu işlemlere bu istekleri sonra anlamına gelebilir. 

![][0]
*Şekil 1: Bir web hizmeti aracılığıyla bir kimlik deposu için Azure Active Directory'den sağlama*

Bu özellik birlikte "kendi uygulamanızı getir" özelliğine sahip Azure AD çoklu oturum açma ve otomatik kullanıcı sağlayan veya SCIM'yi web hizmeti tarafından fronted uygulamalar için hazırlama etkinleştirmek için kullanılabilir.

Azure Active Directory'de SCIM'yi kullanma iki kullanım örnekleri şunlardır:

* **Kullanıcılar ve gruplar SCIM'yi destekleyen uygulamalar için sağlama** SCIM'yi 2.0 desteği ve yapılandırma olmadan Azure AD ile kimlik doğrulaması çalışır OAuth taşıyıcı belirteçlerini kullanmak uygulamalar.
* **Diğer API tabanlı sağlama destekleyen uygulamalar için kendi sağlama çözümü derleme** SCIM'yi olmayan uygulamalar için Azure AD SCIM'yi uç noktası ve uygulama destekleyen kullanıcı sağlama için herhangi bir API'yi arasında çevirmek için SCIM'yi uç noktası oluşturabilirsiniz. Bir SCIM'yi uç noktası geliştirmenize yardımcı olması için SCIM'yi uç noktası sağlamak ve SCIM'yi iletileri çevirmek nasıl Göster kod örnekleri birlikte ortak dil altyapısı (CLI) kitaplıkları sunuyoruz.  

## <a name="provisioning-users-and-groups-to-applications-that-support-scim"></a>Kullanıcılar ve gruplar SCIM'yi destekleyen uygulamalar için hazırlama
Azure AD, otomatik olarak sağlama atanan kullanıcılar ve gruplar kullanan uygulamalar için yapılandırılabilir bir [sistemi için etki alanları arası Kimlik Yönetimi 2 (SCIM'yi)](https://tools.ietf.org/html/draft-ietf-scim-api-19) web hizmeti ve kimlik doğrulaması için OAuth taşıyıcı belirteçlerini kabul edin. SCIM'yi 2.0 belirtimi içinde uygulamalar bu gereksinimleri karşılamalıdır:

* Kullanıcılara ve/veya bölüm 3.3 SCIM'yi Protokolü göredir grupları oluşturmayı destekler.  
* Kullanıcılara ve/veya SCIM'yi Protokolü 3.5.2'de bölümünü göredir patch isteklerinde gruplarla değiştirerek destekler.  
* Bilinen kaynak SCIM'yi Protokolü 3.4.1 bölümünü göredir alma destekler.  
* Kullanıcılara ve/veya SCIM'yi Protokolü 3.4.2 bölümünü göredir grupları sorgulama destekler.  Varsayılan olarak, kullanıcılar tarafından externalID seçmeleri istenir ve grupları displayName tarafından sorgulanır.  
* Kullanıcı kimliği ve bölüm 3.4.2 SCIM'yi Protokolü göredir Yöneticisi tarafından sorgulama destekler.  
* Grup Kimliği ve bölüm 3.4.2 SCIM'yi Protokolü göredir üye tarafından sorgulama destekler.  
* Bölüm 2.1 göredir yetkilendirme SCIM'yi protokolü için OAuth taşıyıcı belirteçleri kabul eder.

Uygulama sağlayıcınıza veya bu gereksinimleri ile uyumluluk bilgilerinin uygulama sağlayıcınızın belgelerine başvurun.

### <a name="getting-started"></a>Başlarken
Bu makalede açıklanan SCIM'yi profili destekleyen uygulamalar Azure AD uygulama galerisinde "galeri olmayan uygulama" özelliği kullanılarak Azure Active Directory'ye bağlı. Bağlandıktan sonra Azure AD eşitleme işlemi burada uygulamanın SCIM'yi endpoint atanan kullanıcılar ve gruplar için sorgular ve oluşturur veya göre atama ayrıntıları değiştirir 20 dakikada bir çalışır.

**SCIM'yi destekleyen bir uygulama bağlanmak için:**

1. Oturum [Azure portalı](https://portal.azure.com). 
2. Gözat ** Azure Active Directory > Kurumsal uygulamaları ve select **yeni uygulama > tüm > olmayan galeri uygulama**.
3. Uygulamanız için bir ad girin ve tıklayın **Ekle** uygulama nesne oluşturmak için simge.
    
  ![][1]
  *Şekil 2: Azure AD uygulama galerisinde*
    
4. Sonuçta elde edilen ekranında şunları seçin **sağlama** sol sütunda sekmesi.
5. İçinde **sağlama modu** menüsünde, select **otomatik**.
    
  ![][2]
  *Şekil 3: Azure portalında sağlama yapılandırma*
    
6. İçinde **Kiracı URL** alanında, uygulamanın SCIM'yi uç nokta URL'sini girin. Örnek: https://api.contoso.com/scim/v2/
7. Ardından SCIM'yi uç noktanın bir OAuth taşıyıcı belirtecinden Azure AD dışında bir veren gerektiriyorsa, gerekli OAuth taşıyıcı belirteci isteğe bağlı kopyalamak **gizli belirteci** alan. Bu alan boş bırakılırsa, Azure AD her istek ile Azure AD tarafından verilen bir OAuth taşıyıcı belirteci dahil. Azure AD kimlik sağlayıcısı bu Azure AD doğrulayabilirsiniz olarak kullanan uygulamaları-belirteç.
8. Tıklatın **Bağlantıyı Sına** SCIM'yi bitiş noktasına bağlanmaya Azure Active Directory için düğmesi. Deneme başarısız olursa hata bilgileri görüntülenir.  
9. Uygulama başarılı için bağlanmaya çalışırsa sonra tıklatırsanız **kaydetmek** yönetici kimlik bilgilerini kaydetmek için.
10. İçinde **eşlemeleri** bölümünde, iki seçilebilir öznitelik eşlemelerini kümesi vardır: biri kullanıcı nesneleri ve Grup nesneleri için bir tane. Uygulamanız için Azure Active Directory'den eşitlenen öznitelikler gözden geçirmek için her birini seçin. Seçilen öznitelikler **eşleşen** özellikleri, kullanıcıları ve grupları güncelleştirme işlemleri için uygulamanızda eşleştirmek için kullanılır. Değişiklikleri kaydetmek için Kaydet düğmesini seçin.

    >[!NOTE]
    >İsteğe bağlı olarak, "eşleme grupları" devre dışı bırakarak group nesnelerini eşitlemeyi devre dışı bırakın. 

11. Altında **ayarları**, **kapsam** alanı tanımlar hangi kullanıcı ve grup veya eşitlenir. "Eşitleme yalnızca kullanıcılar ve gruplar (önerilen) atanan" seçerek yalnızca eşitleme kullanıcılar ve gruplar atanan **kullanıcılar ve gruplar** sekmesi.
12. Yapılandırma tamamlandıktan sonra değiştirmek **sağlama durumu** için **üzerinde**.
13. Tıklatın **kaydetmek** hizmet sağlama Azure AD başlatmak için. 
14. Eşitlemeyi yalnızca kullanıcılar ve gruplar (önerilen) atanmışsa, seçtiğinizden emin olun **kullanıcılar ve gruplar** sekmesinde ve kullanıcılara ve/veya eşitlemek istediğiniz grupları atayabilirsiniz.

İlk eşitleme başladıktan sonra kullanabileceğiniz **denetim günlüklerini** uygulamanızı sağlama hizmeti tarafından gerçekleştirilen tüm eylemler gösterir İzleyici ilerleme sekmesine. Günlükleri sağlama Azure AD okuma hakkında daha fazla bilgi için bkz: [otomatik olarak bir kullanıcı hesabı sağlama raporlama](https://docs.microsoft.com/azure/active-directory/active-directory-saas-provisioning-reporting).

>[!NOTE]
>İlk eşitleme gerçekleştirmek yaklaşık 20 dakikada çalıştığı sürece oluşan sonraki eşitlemeler uzun sürer. 


## <a name="building-your-own-provisioning-solution-for-any-application"></a>Herhangi bir uygulama için kendi sağlama çözümü oluşturma
Azure Active Directory ile arabirimleri SCIM'yi web hizmeti oluşturarak, bir REST veya SOAP kullanıcı API hazırlama sağlar neredeyse her uygulama için tek oturum açma ve otomatik kullanıcı sağlamayı etkinleştirebilirsiniz.

İşte nasıl çalışır:

1. Azure AD sağlar adlı ortak dil altyapısı kitaplık [Microsoft.SystemForCrossDomainIdentityManagement](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/). Sistem tümleştiricileri ve geliştiriciler bu kitaplığı oluşturmak ve bir SCIM'yi tabanlı web hizmeti uç noktası için herhangi bir uygulamanın kimlik deposu Azure AD bağlanıp bağlanamayacağını dağıtmak için kullanabilirsiniz.
2. Eşlemeleri kullanıcı şema ve uygulama tarafından istenen Protokolü standartlaştırılmış kullanıcı şemasını eşleştirmek için web hizmeti uygulanır.
3. Uç nokta URL'sini Azure AD uygulama galerisinde özel bir uygulama bir parçası olarak kaydedilir.
4. Kullanıcıları ve grupları, Azure AD'de bu uygulama atanır. Atama sırasında bunlar hedef uygulama için eşitlenecek kuyruğuna yerleştirilir. Sıranın işleme eşitleme işlemi 20 dakikada bir çalışır.

### <a name="code-samples"></a>Kod Örnekleri
Bu işlem daha kolay, bir dizi yapmak için [kod örnekleri](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master) SCIM'yi web hizmeti uç noktası oluşturun ve otomatik sağlamayı göstermek sağlanır. Kullanıcılar ve gruplar temsil eden virgülle ayrılmış değerler satırlarla bir dosyayı korur bir sağlayıcının bir örnek verilmiştir.  Diğer Amazon Web Hizmetleri kimlik ve erişim yönetimi hizmetini çalışır bir sağlayıcı değil.  

**Önkoşullar**

* Visual Studio 2013 veya üzeri
* [.NET için Azure SDK](https://azure.microsoft.com/downloads/)
* SCIM'yi uç noktası olarak kullanılacak ASP.NET framework 4.5 destekleyen Windows makine. Bu makinenin buluttan erişilebilir olması gerekir
* [Bir Azure aboneliği ile Azure AD Premium deneme veya lisanslı bir sürümü](https://azure.microsoft.com/services/active-directory/)
* Amazon AWS örneği kitaplıklarından gerektirir [Visual Studio için AWS Araç Seti](http://docs.aws.amazon.com/AWSToolkitVS/latest/UserGuide/tkv_setup.html). Daha fazla bilgi için örnekle dahil Benioku dosyasına bakın.

### <a name="getting-started"></a>Başlarken
Azure AD'den sağlama isteklerini kabul edebilir bir SCIM'yi uç noktası uygulamak için kolay derleme ve sağlanan kullanıcılar bir virgülle ayrılmış değer (CSV) dosyasına çıkarır kod örneği dağıtma yoludur.

**Bir örnek SCIM'yi uç noktası oluşturmak için:**

1. Kod örnek paketin karşıdan [https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master)
2. Paketin sıkıştırmasını açın ve Windows makinenizdeki C:\AzureAD-BYOA-Provisioning-Samples\ gibi bir konuma yerleştirin.
3. Bu klasörde, Visual Studio FileProvisioningAgent çözümde başlatın.
4. Seçin **Araçlar > Kitaplık Paket Yöneticisi > Paket Yöneticisi Konsolu**ve çözüm başvuruları çözümlemek FileProvisioningAgent projesi için aşağıdaki komutları çalıştırın:
  ```` 
   Install-Package Microsoft.SystemForCrossDomainIdentityManagement
   Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
   Install-Package Microsoft.Owin.Diagnostics
   Install-Package Microsoft.Owin.Host.SystemWeb
  ````
5. FileProvisioningAgent projesi oluşturun.
6. Windows komut istemi uygulamada (Yönetici) olarak başlatın ve kullanmak **cd** dizine değiştirmek için komutu, **\AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug** klasör.
7. < IP adresi > IP adresine veya etki alanı adıyla Windows makinenin değiştirerek aşağıdaki komutu çalıştırın:
  ````   
   FileAgnt.exe http://<ip-address>:9000 TargetFile.csv
  ````
8. Altında Windows'ta **Windows Ayarları > Ağ ve Internet ayarlarını**seçin **Windows Güvenlik Duvarı > Gelişmiş ayarları**ve oluşturma bir **gelen kuralı** 9000 numaralı bağlantı noktasına gelen erişim sağlar.
9. Windows makine yönlendiricisi arkasında ise, yönlendirici, bağlantı noktası Internet'e açık 9000 ve bağlantı noktası 9000 Windows makinede arasındaki ağ erişim çevirisi gerçekleştirecek şekilde yapılandırılması gerekir. Bu bulutta Bu uç noktasına erişmek Azure AD için gereklidir.

**Azure AD'de örnek SCIM'yi uç noktasını kaydetmek için:**

1. Oturum [Azure portalı](https://portal.azure.com). 
2. Gözat ** Azure Active Directory > Kurumsal uygulamaları ve select **yeni uygulama > tüm > olmayan galeri uygulama**.
3. Uygulamanız için bir ad girin ve tıklayın **Ekle** uygulama nesne oluşturmak için simge. Oluşturulan uygulama nesnesi olmaları için sağlama ve çoklu oturum açma için ve yalnızca SCIM'yi uç uygulama hedef uygulama temsil etmek üzere tasarlanmıştır.
4. Sonuçta elde edilen ekranında şunları seçin **sağlama** sol sütunda sekmesi.
5. İçinde **sağlama modu** menüsünde, select **otomatik**.
    
  ![][2]
  *Şekil 4: Azure portalında sağlama yapılandırma*
    
6. İçinde **Kiracı URL** alanında, Internet kullanıma sunulan URL ve bağlantı noktası SCIM'yi uç noktanızı girin. Http://testmachine.contoso.com:9000 veya http://<ip-address>:9000/, burada < IP adresi >, internet IP kullanıma sunulan gibi bu bir şey olacaktır adresi.  
7. Ardından SCIM'yi uç noktanın bir OAuth taşıyıcı belirtecinden Azure AD dışında bir veren gerektiriyorsa, gerekli OAuth taşıyıcı belirteci isteğe bağlı kopyalamak **gizli belirteci** alan. Bu alan boş bırakılırsa, Azure AD her istek ile Azure AD tarafından verilen bir OAuth taşıyıcı belirteci dahil edilir. Azure AD kimlik sağlayıcısı bu Azure AD doğrulayabilirsiniz olarak kullanan uygulamaları-belirteç.
8. Tıklatın **Bağlantıyı Sına** SCIM'yi bitiş noktasına bağlanmaya Azure Active Directory için düğmesi. Deneme başarısız olursa hata bilgileri görüntülenir.  
9. Uygulama başarılı için bağlanmaya çalışırsa sonra tıklatırsanız **kaydetmek** yönetici kimlik bilgilerini kaydetmek için.
10. İçinde **eşlemeleri** bölümünde, iki seçilebilir öznitelik eşlemelerini kümesi vardır: biri kullanıcı nesneleri ve Grup nesneleri için bir tane. Uygulamanız için Azure Active Directory'den eşitlenen öznitelikler gözden geçirmek için her birini seçin. Seçilen öznitelikler **eşleşen** özellikleri, kullanıcıları ve grupları güncelleştirme işlemleri için uygulamanızda eşleştirmek için kullanılır. Değişiklikleri kaydetmek için Kaydet düğmesini seçin.
11. Altında **ayarları**, **kapsam** alanı tanımlar hangi kullanıcı ve grup veya eşitlenir. "Eşitleme yalnızca kullanıcılar ve gruplar (önerilen) atanan" seçerek yalnızca eşitleme kullanıcılar ve gruplar atanan **kullanıcılar ve gruplar** sekmesi.
12. Yapılandırma tamamlandıktan sonra değiştirmek **sağlama durumu** için **üzerinde**.
13. Tıklatın **kaydetmek** hizmet sağlama Azure AD başlatmak için. 
14. Eşitlemeyi yalnızca kullanıcılar ve gruplar (önerilen) atanmışsa, seçtiğinizden emin olun **kullanıcılar ve gruplar** sekmesinde ve kullanıcılara ve/veya eşitlemek istediğiniz grupları atayabilirsiniz.

İlk eşitleme başladıktan sonra kullanabileceğiniz **denetim günlüklerini** uygulamanızı sağlama hizmeti tarafından gerçekleştirilen tüm eylemler gösterir İzleyici ilerleme sekmesine. Günlükleri sağlama Azure AD okuma hakkında daha fazla bilgi için bkz: [otomatik olarak bir kullanıcı hesabı sağlama raporlama](https://docs.microsoft.com/azure/active-directory/active-directory-saas-provisioning-reporting).

Windows makinenizi \AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug klasöründe TargetFile.csv dosyayı açmak için örnek doğrulama son adım var. Sağlama işlemini çalıştırdıktan sonra bu dosyayı tüm ayrıntılarını atanan ve kullanıcılar ve gruplar sağlanan gösterir.

### <a name="development-libraries"></a>Geliştirme kitaplıkları
SCIM'yi belirtimine uygun kendi web hizmeti geliştirmek için öncelikle geliştirme sürecinin artırmanıza yardımcı olmak üzere Microsoft tarafından sağlanan aşağıdaki kitaplıkları tanıyın: 

1. Ortak dil altyapısı (CLI) kitaplıklar gibi C#, altyapı göre dilleri ile kullanıma sunulur. Bu kitaplıklar birini [Microsoft.SystemForCrossDomainIdentityManagement.Service](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/), aşağıdaki çizimde gösterilen bir arabirim Microsoft.SystemForCrossDomainIdentityManagement.IProvider, bildirir: kitaplıkları kullanarak bir geliştirici bu arabirim için genel olarak, bir sağlayıcısı olarak adlandırılabilir sınıfıyla uygulamak. Kitaplıklar için SCIM'yi belirtimi uyumlu bir web hizmeti dağıtmak Geliştirici etkinleştirin. Web hizmeti ya da Internet Information Services veya yürütülebilir bir ortak dil altyapısı derlemesi içinde barındırılabilir. İstek, bazı kimlik deposu üzerinde çalışması için geliştirici tarafından programlanmış sağlayıcının yöntem çağrıları veri dönüştürülür.
  
  ![][3]
  
2. [Rota işleyicileri express](http://expressjs.com/guide/routing.html) bir node.js web hizmeti çağrıları (SCIM'yi belirtimi tarafından tanımlandığı gibi) temsil eden bir node.js isteği nesneleri ayrıştırma için kullanılabilir yapılır.   

### <a name="building-a-custom-scim-endpoint"></a>Bir özel SCIM'yi uç noktası oluşturma
CLI kitaplıkları kullanarak, bu kitaplığı kullanan geliştiriciler kendi Hizmetleri veya Internet Information Services yürütülebilir ortak dil altyapısı derlemeler içinde barındırabilir. Http://localhost:9000 adresindeki yürütülebilir bütünleştirilmiş kod içinde bir hizmet barındırma için örnek kod aşağıda verilmiştir: 

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

Bu hizmet kök sertifika yetkilisi aşağıdakilerden biri olduğu bir HTTP adresi ve bir sunucu kimlik doğrulama sertifikası olması gerekir: 

* CNNIC
* Comodo
* CyberTrust
* DigiCert
* GeoTrust
* GlobalSign
* Daddy gidin
* VeriSign
* WoSign

Bir sunucu kimlik doğrulama sertifikası ağ Kabuğu yardımcı programını kullanarak bir Windows ana bilgisayarda bir bağlantı noktasına bağlı olabilir: 

    netsh http add sslcert ipport=0.0.0.0:443 certhash=0000000000003ed9cd0c315bbb6dc1c08da5e6 appid={00112233-4455-6677-8899-AABBCCDDEEFF}  

Burada, rastgele bir genel benzersiz tanımlayıcı olsa da AppID bağımsız değişkeni için sağlanan değer certhash bağımsız değişkeni için sağlanan değer sertifikanın parmak izini ' dir.  

Internet Information Services hizmetinde barındırmak için bir geliştirici bir CLA kod kitaplığı başlangıç derleme varsayılan ad alanı adlı bir sınıf ile derleme.  Böyle bir sınıfın bir örneği aşağıda verilmiştir: 

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
      Owin.IAppBuilder builder) // Defined in in Owin.dll.  
    {
        this.starter.ConfigureApplication(builder);
    }
    }

### <a name="handling-endpoint-authentication"></a>İşleme uç noktası kimlik doğrulaması
Azure Active Directory gelen istekleri bir OAuth 2.0 taşıyıcı belirteci içerir.   İsteği alma herhangi bir hizmet olarak Azure Active Directory Azure Active Directory Graph web hizmetine erişim için beklenen Azure Active Directory Kiracı adına veren kimlik doğrulaması.  Belirteç veren "ISS" gibi bir ISS talep tanımlanır: "https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/".  Bu örnekte, talep değeri, taban adresini https://sts.windows.net, göreli adres segment cbb1a5ac-f33b-45fa-9bf5-f37db0fed422, hangi adına belirteç verildiği Azure Active Directory Kiracı benzersiz bir tanımlayıcı olsa da bu Azure Active Directory, veren tanımlar.  Azure Active Directory Graph web hizmetine erişmek için belirteç verilmişse, 00000002-0000-0000-c000-000000000000, bu hizmeti tanıtıcısı belirtecin aud talebin değeri olmalıdır.  

Geliştiriciler SCIM'yi hizmet oluşturmak için Microsoft tarafından sağlanan CLA kitaplıkları kullanarak aşağıdaki adımları izleyerek Microsoft.Owin.Security.ActiveDirectory paketi kullanılarak Azure Active Directory'ye gelen istekleri doğrulayabilir: 

1. Sağlayıcı, bu hizmeti başlatıldığında çağrılacak döndürme sağlayarak Microsoft.SystemForCrossDomainIdentityManagement.IProvider.StartupBehavior özelliği uygulayın: 

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

2. Bu yöntem herhangi bir hizmetin uç noktaları erişim Azure AD grafik web hizmeti için belirtilen bir kiracı adına Azure Active Directory tarafından verilmiş bir belirteç pul olarak kimlik doğrulaması için herhangi bir istek olması için aşağıdaki kodu ekleyin: 

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
Azure Active Directory iki tür kaynak SCIM'yi web hizmetlerine sağlayabilirsiniz.  Bu kaynakların kullanıcılar ve gruplar türleridir.  

Kullanıcı kaynakları şema tanımlayıcısı, bu protokolü belirtimi içinde bulunan urn: ietf:params:scim:schemas:extension:enterprise:2.0:User tanımlanır: http://tools.ietf.org/html/draft-ietf-scim-core-schema.  Azure Active Directory'de kullanıcıları urn: ietf:params:scim:schemas:extension:enterprise:2.0:User kaynakları özniteliklerinin özniteliklerinin varsayılan eşleme Tablo 1, aşağıda verilmiştir.  

Grup kaynaklarının şema tanımlayıcısı tarafından tanımlanan http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.  Azure Active Directory'deki öznitelikleri için http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group kaynak grupları özniteliklerinin varsayılan eşleme Tablo 2, aşağıda gösterilmektedir.  

### <a name="table-1-default-user-attribute-mapping"></a>Tablo 1: Varsayılan kullanıcı özniteliği eşlemesi
| Azure Active Directory kullanıcısı | urn: ietf:params:scim:schemas:extension:enterprise:2.0:User |
| --- | --- |
| IsSoftDeleted |etkin |
| Görünen adı |Görünen adı |
| Faks TelephoneNumber |PhoneNumber [türü eq "faks"] .value |
| givenName |name.givenName |
| İş Unvanı |Başlık |
| Posta |e-postaları [türü eq "İş"] .value |
| mailNickname |externalID |
| Yöneticisi |Yöneticisi |
| Mobil |PhoneNumber [türü eq "mobile"] .value |
| objectID |id |
| posta kodu |adresler [türü eq "İş"] .postalCode |
| Proxy adresleri |e-postaları [eq "diğer" yazın]. Değer |
| fiziksel teslim OfficeName |adresler [eq "diğer" yazın]. Biçimlendirilmiş |
| StreetAddress |adresler [türü eq "İş"] .streetAddress |
| Soyadı |name.familyName |
| telefon numarası |PhoneNumber [türü eq "İş"] .value |
| Kullanıcı PrincipalName |Kullanıcı adı |

### <a name="table-2-default-group-attribute-mapping"></a>Tablo 2: Varsayılan grup özellik eşlemesi
| Azure Active Directory grubu | http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group |
| --- | --- |
| Görünen adı |externalID |
| Posta |e-postaları [türü eq "İş"] .value |
| mailNickname |Görünen adı |
| üyeler |üyeler |
| objectID |id |
| proxyAddresses |e-postaları [eq "diğer" yazın]. Değer |

## <a name="user-provisioning-and-de-provisioning"></a>Kullanıcı hazırlama ve sağlamayı kaldırma özelliklerini
Aşağıdaki çizimde gösterildiği Azure Active Directory kullanıcı başka bir kimlik deposu olarak ömrünü yönetmek için SCIM'yi Hizmeti'ne gönderilen iletileri. Diyagram ayrıca nasıl CLI kitaplıkları kullanılarak uygulanan SCIM'yi sağlanan bir hizmeti Microsoft tarafından yapı için bir sağlayıcı yöntem çağrıları içine bu istekleri gibi hizmetleri Çevir gösterir.  

![][4]
*Şekil 5: Kullanıcı sağlama ve sağlamayı kaldırma özelliklerini sırası*

1. Azure AD'de kullanıcı mailNickname öznitelik değerini eşleşen bir externalID öznitelik değeri olan bir kullanıcı için hizmet Azure Active Directory'yi sorgular. Sorgu; burada görüntülerle jyoung mailNickname bir kullanıcının Azure Active Directory'de örneğidir Bu örnekte gibi bir Köprü Metni Aktarım Protokolü (HTTP) istek olarak ifade edilir: 
  ````
    GET https://.../scim/Users?filter=externalId eq jyoung HTTP/1.1
    Authorization: Bearer ...
  ````
  Hizmet SCIM'yi Hizmetleri uygulamak için Microsoft tarafından sağlanan ortak dil altyapısı kitaplıkları kullanılarak oluşturuldu, istek sorgu yöntemi hizmet sağlayıcı için bir çağrı çevrilir.  Bu yöntem imzası şöyledir: 
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
  Bir kullanıcı için bir sorgu externalID özniteliği için belirtilen bir değerle aşağıdaki örnekte sorgu yönteme geçirilen bağımsız değişkenlerin değerleri şunlardır: 
  * parametreleri. AlternateFilters.Count: 1
  * parametreleri. AlternateFilters.ElementAt(0). AttributePath: "externalID"
  * parametreleri. AlternateFilters.ElementAt(0). Karşılaştırmaİşleci: ComparisonOperator.Equals
  * parametreleri. AlternateFilter.ElementAt(0). ComparisonValue: "jyoung"
  * correlationIdentifier: System.Net.Http.HttpRequestMessage.GetOwinEnvironment["owin. RequestId"] 

2. Web hizmeti, bir kullanıcının mailNickname öznitelik değeri ile eşleşen bir externalID öznitelik değeri olan bir kullanıcı için bir sorguya yanıt herhangi bir kullanıcının döndürmezse, Azure Active Directory hizmeti bir Azure Active Directory'de karşılık gelen bir kullanıcı sağlamak ister.  Böyle bir istek bir örneği burada verilmiştir: 
  ````
    POST https://.../scim/Users HTTP/1.1
    Authorization: Bearer ...
    Content-type: application/json
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
  SCIM'yi Hizmetleri uygulamak için Microsoft tarafından sağlanan ortak dil altyapısı kitaplıkları bu isteği oluşturma yöntemi hizmet sağlayıcı için bir çağrı çevirir.  Create yönteminin bu imzaya sahip: 
  ````
    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  

    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource> Create(
      Microsoft.SystemForCrossDomainIdentityManagement.Resource resource, 
      string correlationIdentifier);
  ````
  Bir kullanıcı sağlamak için istekte kaynak bağımsız değişkeninin değeri Microsoft.SystemForCrossDomainIdentityManagement örneğidir. Microsoft.SystemForCrossDomainIdentityManagement.Schemas Kitaplığı'nda tanımlanan Core2EnterpriseUser sınıfı.  Kullanıcı sağlama isteği başarılı olursa, yöntemin kullanımı Microsoft.SystemForCrossDomainIdentityManagement örneği döndürmesi beklenir. Yeni sağlanan kullanıcının benzersiz tanımlayıcıya ayarlamak tanımlayıcı özelliğinin değeri ile Core2EnterpriseUser sınıfı.  

3. Bir kullanıcı tarafından bir SCIM'yi fronted kimlik deposunda bilinmektedir güncelleştirmek için Azure Active Directory gibi bir istekle hizmetinden kullanıcı geçerli durumunu isteyerek çalışır: 
  ````
    GET ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
  ````
  SCIM'yi Hizmetleri uygulamak için Microsoft tarafından sağlanan ortak dil altyapısı kitaplıkları kullanılarak oluşturulmuş bir hizmeti, hizmet sağlayıcısının alma yöntemine bir çağrı isteği çevrilir.  Alma yöntemi imzası şöyledir: 
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
  Kullanıcının geçerli durumunu almak için bir istek örnekte parametreleri bağımsız değişkenin değeri sağlanan nesne özelliklerinin değerleri aşağıdaki gibidir: 
  
  * Tanımlayıcı: "54D382A4-2050-4C03-94D1-E769F1D15682"
  * SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"

4. Azure Active Directory kimlik deposu olarak başvuru özniteliği geçerli değeri hizmeti tarafından zaten fronted olup olmadığını belirlemek üzere hizmetini sorgular ardından güncelleştirilmesi için bir başvuru özniteliği ise Azure Active Directory'de bu özniteliğin değeri ile eşleşir. Kullanıcılar için bu şekilde sorgulanan geçerli değeri yalnızca öznitelik Yöneticisi özniteliğidir. Belirli bir kullanıcı nesnesinin Yöneticisi özniteliği şu anda belirli bir değere sahip olup olmadığını belirlemek için bir istek bir örneği burada verilmiştir: 
  ````
    GET ~/scim/Users?filter=id eq 54D382A4-2050-4C03-94D1-E769F1D15682 and manager eq 2819c223-7f76-453a-919d-413861904646&attributes=id HTTP/1.1
    Authorization: Bearer ...
  ````
  Değer öznitelikleri sorgu parametresinin kimliği güveninin, bir kullanıcı nesnesi filtre sorgu parametresi değeri olarak sağlanan ifade karşılayan varsa, ardından hizmeti yalnızca o kaynağın Kimliği özniteliğinin değeri de dahil olmak üzere urn: ietf:params:scim:schemas:core:2.0:User veya urn: ietf:params:scim:schemas:extension:enterprise:2.0:User sahip bir kaynak, yanıt vermesi bekleniyor.  Değeri **kimliği** istek sahibine bilinen öznitelik. Filtre sorgu parametresi değeri içinde bulunur; Filtre ifadesinin olup olmadığına böyle bir nesne var olan bir göstergesi olarak çağıran bir kaynak en az bir gösterimini istemek üzere söylemelisiniz amacı gerçekte var.   

  Hizmet SCIM'yi Hizmetleri uygulamak için Microsoft tarafından sağlanan ortak dil altyapısı kitaplıkları kullanılarak oluşturuldu, istek sorgu yöntemi hizmet sağlayıcı için bir çağrı çevrilir. Parametre bağımsız değişkenin değeri sağlanan nesne özelliklerini değeri aşağıdaki gibidir: 
  
  * parametreleri. AlternateFilters.Count: 2
  * parametreleri. AlternateFilters.ElementAt(x). AttributePath: "id"
  * parametreleri. AlternateFilters.ElementAt(x). Karşılaştırmaİşleci: ComparisonOperator.Equals
  * parametreleri. AlternateFilter.ElementAt(x). ComparisonValue: "54D382A4-2050-4C03-94D1-E769F1D15682"
  * parametreleri. AlternateFilters.ElementAt(y). AttributePath: "Yönetici"
  * parametreleri. AlternateFilters.ElementAt(y). Karşılaştırmaİşleci: ComparisonOperator.Equals
  * parametreleri. AlternateFilter.ElementAt(y). ComparisonValue: "2819c223-7f76-453a-919d-413861904646"
  * parametreleri. RequestedAttributePaths.ElementAt(0): "id"
  * parametreleri. SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"

  Burada, dizinin x değeri 0 olabilir ve dizin y değeri 1, olabilir ya da x değerini 1 olabilir ve y değeri filtre sorgu parametresi ifadelerin sırasını bağlı olarak 0 olabilir.   

5. Bir isteğin bir kullanıcıyı güncelleştirmek için SCIM'yi hizmetine örnek Azure Active Directory'den verilmiştir: 
  ````
    PATCH ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
    Content-type: application/json
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
  Microsoft ortak dil altyapısı kitaplıkları SCIM'yi Hizmetleri uygulamak için hizmet sağlayıcısı'nın güncelleştirme yöntemine bir çağrı isteği çevirir. Update yöntemi imzası şöyledir: 
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
    Kullanıcıyı güncelleştirmek için bir istek örnekte, bu özellik değerlerini düzeltme eki bağımsız değişkenin değeri sağlanan nesne vardır: 
  
  * ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"
  * ResourceIdentifier.SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"
  * (PatchRequest PatchRequest2 olarak). Operations.Count: 1
  * (PatchRequest PatchRequest2 olarak). Operations.ElementAt(0). OperationName: OperationName.Add
  * (PatchRequest PatchRequest2 olarak). Operations.ElementAt(0). Path.AttributePath: "Yönetici"
  * (PatchRequest PatchRequest2 olarak). Operations.ElementAt(0). Value.Count: 1
  * (PatchRequest PatchRequest2 olarak). Operations.ElementAt(0). Value.ElementAt(0). Başvuru: http://.../scim/Users/2819c223-7f76-453a-919d-413861904646
  * (PatchRequest PatchRequest2 olarak). Operations.ElementAt(0). Value.ElementAt(0). Değer: 2819c223-7f76-453a-919d-413861904646

6. Bir kullanıcı bir SCIM'yi hizmeti tarafından fronted kimlik mağazadan sağlanmasını için Azure AD gibi bir isteği gönderir: 
  ````
    DELETE ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
  ````
  Hizmet SCIM'yi Hizmetleri uygulamak için Microsoft tarafından sağlanan ortak dil altyapısı kitaplıkları kullanılarak oluşturuldu, istek hizmet sağlayıcısının Delete yöntemine bir çağrı çevrilir.   Bu yöntem, bu imza sahiptir: 
  ````
    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
    // is defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 
    System.Threading.Tasks.Task Delete(
      Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier  
        resourceIdentifier, 
      string correlationIdentifier);
  ````
  Bu özellik değerlerini resourceIdentifier bağımsız değişkenin değeri sağlanan nesne kullanıcı sağlanmasını isteği örnekte vardır: 
  
  * ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"
  * ResourceIdentifier.SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"

## <a name="group-provisioning-and-de-provisioning"></a>Grup sağlama ve sağlamayı kaldırma özelliklerini
Aşağıdaki çizimde gösterildiği Azure AcD bir grubu başka bir kimlik deposu içinde yaşam döngüsü yönetmek için SCIM'yi Hizmeti'ne gönderilen iletileri.  Bu iletiler, kullanıcılara üç yolla ilgili iletiler farklıdır: 

* Bir grup kaynağına şema http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group tanımlanır.  
* Grupları almak için yapılan isteklerinin üyeleri öznitelik isteğine yanıt olarak sağlanan herhangi bir kaynağa dışında tutulması olduğunu iznine.  
* Bir başvuru özniteliği belirli bir değere sahip olup olmadığını belirlemek üzere istekleri üyeleri özniteliği hakkında isteklerdir.  

![][5]
*Şekil 6: Grup sağlama ve sağlamayı kaldırma özelliklerini sırası*

## <a name="related-articles"></a>İlgili makaleler
* [Azure Active Directory'de Uygulama Yönetimi için Makale Dizini](active-directory-apps-index.md)
* [Kullanıcı sağlama/sağlamayı SaaS uygulamaları için otomatik hale getirme](active-directory-saas-app-provisioning.md)
* [Kullanıcı sağlama öznitelik eşlemelerini özelleştirme](active-directory-saas-customizing-attribute-mappings.md)
* [Özellik eşlemeleri için ifade yazma](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [Kapsam belirleme filtreleri kullanıcı sağlama](active-directory-saas-scoping-filters.md)
* [Hesap sağlama bildirimleri](active-directory-saas-account-provisioning-notifications.md)
* [SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)

<!--Image references-->
[0]: ./media/active-directory-scim-provisioning/scim-figure-1.PNG
[1]: ./media/active-directory-scim-provisioning/scim-figure-2a.PNG
[2]: ./media/active-directory-scim-provisioning/scim-figure-2b.PNG
[3]: ./media/active-directory-scim-provisioning/scim-figure-3.PNG
[4]: ./media/active-directory-scim-provisioning/scim-figure-4.PNG
[5]: ./media/active-directory-scim-provisioning/scim-figure-5.PNG
