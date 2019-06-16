---
title: Şirket içinde erişim API'leri ile Azure AD uygulama ara sunucusu
description: Azure Active Directory Uygulama proxy'si yerel uygulamalar API'leri güvenli bir şekilde erişmesini sağlar ve şirket içi ana iş mantığı ya da bulut Vm'leri.
services: active-directory
author: jeevanbisht
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: celested
ms.reviewer: japere
ms.openlocfilehash: c2b99525e3d0a61c02dc502fcd0927ea65993e5b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67108581"
---
# <a name="secure-access-to-on-premises-apis-with-azure-ad-application-proxy"></a>Azure AD uygulama ara sunucusu ile şirket içi API'lere güvenli erişim

İş mantığı şirket içinde çalışan API'leri olabilir veya sanal makineleri bulutta barındırılan. Yerel Android, iOS, Mac veya Windows uygulamaları, verileri kullanma veya kullanıcı etkileşimi sağlamak için API uç noktaları ile etkileşim kurmak gerekir. Azure AD uygulama proxy'si ve [Azure Active Directory kimlik doğrulama kitaplıkları (ADAL)](/azure/active-directory/develop/active-directory-authentication-libraries) let yerel uygulamalarınızı şirket içi Apı'lerinizi güvenli bir şekilde erişim. Azure Active Directory Uygulama proxy'si, güvenlik duvarı bağlantı noktalarını açma ve kimlik doğrulama ve yetkilendirme uygulama katmanında denetleme daha hızlı ve daha güvenli bir çözümdür. 

Bu makalede, yerel uygulamaların erişebileceği bir web API'si hizmetini barındıran bir Azure AD uygulama proxy'si çözüm kurma gösterilmektedir. 

## <a name="overview"></a>Genel Bakış

Aşağıdaki diyagramda, şirket içi API'ler yayımlamayı geleneksel bir yolunu gösterir. Bu yaklaşım, 80 ve 443 gelen bağlantı noktalarını açmayı gerektirir.

![Geleneksel API erişimi](./media/application-proxy-secure-api-access/overview-publish-api-open-ports.png)

Aşağıdaki diyagramda, herhangi bir gelen bağlantı noktaları açmadan API'leri güvenle yayınlamamızı Azure AD uygulama proxy'si nasıl kullanabileceğinizi gösterir:

![Azure AD uygulama ara Sunucusu API erişimi](./media/application-proxy-secure-api-access/overview-publish-api-app-proxy.png)

API erişimi için ortak bir uç nokta olarak çalışan ve kimlik doğrulama ve yetkilendirme sağlayan, Azure AD uygulama proxy'si çözümünün omurga oluşturur. Geniş bir platformlar dizisinden API'leri kullanarak erişebilirsiniz [ADAL](/azure/active-directory/develop/active-directory-authentication-libraries) kitaplıkları. 

Azure AD üzerinde oluşturulan Azure AD uygulama proxy'si kimlik doğrulama ve yetkilendirme olduğundan, uygulama proxy'si aracılığıyla yayımlandığından API'leri yalnızca güvenilen cihazların erişebildiğinden emin olmak için Azure AD koşullu erişim kullanın. Azure AD'ye katılım'ı veya Azure AD karma katılmış Masaüstü ve Intune ile yönetilen cihazlar için kullanın. Azure çok faktörlü kimlik doğrulaması gibi Azure Active Directory Premium özelliklerden ve makine öğrenimi destekli güvenliğini de alabilir [Azure kimlik koruması](/azure/active-directory/active-directory-identityprotection).

## <a name="prerequisites"></a>Önkoşullar

Bu anlatımı için gerekir:

- Bir hesap oluşturabilir ve uygulamaları kaydetme ile bir Azure dizininin yönetici erişimi
- Örnek web API'si ve yerel istemci uygulamaları [https://github.com/jeevanbisht/API-NativeApp-ADAL-SampleApp](https://github.com/jeevanbisht/API-NativeApp-ADAL-SampleApp) 

## <a name="publish-the-api-through-application-proxy"></a>API uygulama proxy'si aracılığıyla uygulama yayımlama

Uygulama proxy'si aracılığıyla intranetinize dışında bir API'yi yayımlamak için yayımlama web uygulamaları için aynı deseni izleyin. Daha fazla bilgi için [Öğreticisi: Azure Active Directory Uygulama proxy'si aracılığıyla uzaktan erişim için şirket içi uygulama ekleme](application-proxy-add-on-premises-application.md).

Uygulama proxy'si aracılığıyla SecretAPI web API'si yayımlama için:

1. Derleme ve yerel bilgisayarınıza veya intranet üzerinde bir ASP.NET web uygulaması olarak örnek SecretAPI proje yayımlayın. Web uygulamasını yerel olarak erişebildiğinden emin olun. 
   
1. İçinde [Azure portalında](https://portal.azure.com)seçin **Azure Active Directory** sol gezinti bölmesinde. Ardından **genel bakış** sayfasında **kurumsal uygulamalar**.
   
1. Üst kısmındaki **kurumsal uygulamalar - tüm uygulamalar** sayfasında **yeni uygulama**.
   
1. Üzerinde **uygulama ekleme** sayfasındaki **kendi uygulamanızı ekleyin**seçin **şirket içi uygulama**. 
   
1. Bir uygulama ara sunucusu bağlayıcısı yoksa, yüklemeniz istenir. Seçin **uygulama ara sunucusu Bağlayıcısı indirme** bağlayıcı karşıdan yüklenip kurulacak. 
   
1. Şirket uygulama ara sunucusu Bağlayıcısı'nı yükledikten sonra **kendi şirket içi uygulamanızı ekleme** sayfası:
   
   1. Girin *SecretAPI* yanındaki **adı**.
      
   1. Intranet içindeki API'si yanındaki erişmek için URL'yi girin **iç Url**. 
      
   1. Emin **ön kimlik doğrulama** ayarlanır **Azure Active Directory**. 
      
   1. Seçin **Ekle** üst sayfasının ve oluşturulacak uygulamayı tamamlanmasını bekleyin.
   
   ![API uygulaması ekleme](./media/application-proxy-secure-api-access/3-add-api-app.png)
   
1. Üzerinde **kurumsal uygulamalar - tüm uygulamalar** sayfasında **SecretAPI** uygulama. 
   
1. Üzerinde **SecretAPI - genel bakış** sayfasında **özellikleri** sol gezinti bölmesinde.
   
1. API'leri son kullanıcılar için kullanılabilir olmasını istemediğiniz **MyApps** panelinde, olacak şekilde ayarlamanız **kullanıcılara görünür** için **Hayır** kısmındaki **özellikleri**sayfasında ve ardından **Kaydet**.
   
   ![Kullanıcılara gösterilmez](./media/application-proxy-secure-api-access/5-not-visible-to-users.png)
   
Web API'nizi Azure AD uygulama proxy'si aracılığıyla yayımladık. Şimdi uygulamayı erişebilen kullanıcılar ekleyin. 

1. Üzerinde **SecretAPI - genel bakış** sayfasında **kullanıcılar ve gruplar** sol gezinti bölmesinde.
   
1. Üzerinde **kullanıcılar ve gruplar** sayfasında **Kullanıcı Ekle**.  
   
1. Üzerinde **ataması ekleme** sayfasında **kullanıcılar ve gruplar**. 
   
1. Üzerinde **kullanıcılar ve gruplar** sayfasında aramak ve en az kendiniz de dahil olmak üzere bu uygulamaya erişebilecek kullanıcıları seçin. Tüm kullanıcılar'ı seçtikten sonra seçin **seçin**. 
   
   ![Seçme ve kullanıcı atama](./media/application-proxy-secure-api-access/7-select-admin-user.png)
   
1. Yeniden **atama Ekle** sayfasında **atama**. 

> [!NOTE]
> Tümleşik Windows kimlik doğrulaması kullanan API'leri gerektirebilecek [ek adımlar](/azure/active-directory/manage-apps/application-proxy-configure-single-sign-on-with-kcd).

## <a name="register-the-native-app-and-grant-access-to-the-api"></a>Yerel bir uygulamayı kaydetme ve API'sine erişim verin

Belirli bir platform veya cihaz üzerinde kullanmak için geliştirilmiş programları yerel uygulamalardır. Yerel uygulamanızı bağlanabilir ve bir API'ye erişmek için önce Azure AD'ye kaydetmeniz gerekir. Aşağıdaki adımlar, yerel bir uygulamayı kaydetme ve Web uygulama proxy'si aracılığıyla yayımlandığından API'sine erişim verin gösterilmektedir.

AppProxyNativeAppSample yerel uygulamayı kaydetmek için:

1. Azure Active Directory'de **genel bakış** sayfasında **uygulama kayıtları**ve en üstündeki **uygulama kayıtları** bölmesinde **yeni kayıt** .
   
1. Üzerinde **bir uygulamayı kaydetme** sayfası:
   
   1. Altında **adı**, girin *AppProxyNativeAppSample*. 
      
   1. Altında **desteklenen hesap türleri**seçin **herhangi bir kuruluş dizinini ve kişisel Microsoft hesapları hesaplarında**. 
      
   1. Altında **tekrar yönlendirme URL'sini**, açılır listesine tıklayıp **genel istemci (Mobil ve Masaüstü)** yazıp enter *https:\//appproxynativeapp*. 
      
   1. Seçin **kaydetme**, uygulama başarıyla kaydedilecek bekleyin. 
      
      ![Yeni uygulama kaydı](./media/application-proxy-secure-api-access/8-create-reg-ga.png)
   
Şimdi, Azure Active Directory'de AppProxyNativeAppSample uygulama kaydettiğinize göre. Yerel uygulama erişimlerini SecretAPI web API'si için:

1. Azure Active Directory'de **genel bakış** > **uygulama kayıtları** sayfasında **AppProxyNativeAppSample** uygulama. 
   
1. Üzerinde **AppProxyNativeAppSample** sayfasında **API izinleri** sol gezinti bölmesinde. 
   
1. Üzerinde **API izinleri** sayfasında **bir izin eklemek**.
   
1. İlk **istek API izinleri** sayfasında **Kuruluşum kullandığı API'leri** sekmesine ve ardından aramak ve seçmek **SecretAPI**. 
   
1. Sonraki **istek API izinleri** yanındaki onay kutusunu işaretleyin, sayfa **user_impersonation**ve ardından **izinleri eklemek**. 
   
    ![Bir API seçin](./media/application-proxy-secure-api-access/10-secretapi-added.png)
   
1. Yeniden **API izinleri** seçebileceğiniz sayfasında **Contoso için yönetici onayı vermek** ayrı ayrı uygulamaya onay verme diğer kullanıcıların önlemek için. 

## <a name="configure-the-native-app-code"></a>Yerel uygulama kodu yapılandırma

Son adım, yerel uygulama yapılandırmaktır. Alınan aşağıdaki kod *Form1.cs* NativeClient örnek uygulama dosyasında isteyen API çağrısı için bir belirteç almak ve uygulama başlığına taşıyıcı eklemek ADAL kitaplığı neden olur. 
   
   ```csharp
       AuthenticationResult result = null;
       HttpClient httpClient = new HttpClient();
       authContext = new AuthenticationContext(authority);
       result = await authContext.AcquireTokenAsync(todoListResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Auto));
       
       // Append the token as bearer in the request header.
       httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
       
       // Call the API.
       HttpResponseMessage response = await httpClient.GetAsync(todoListBaseAddress + "/api/values/4");
   
       // MessageBox.Show(response.RequestMessage.ToString());
       string s = await response.Content.ReadAsStringAsync();
       MessageBox.Show(s);
   ```
   
Azure Active Directory'ye bağlamanın ve API uygulama proxy'si çağırmak için yerel uygulama yapılandırmak için yer tutucu değerleri güncelleştirme *App.config* NativeClient örnek uygulamanın Azure AD'den değerlerle dosyası: 

- Yapıştırma **dizin (Kiracı) kimliği** içinde `<add key="ida:Tenant" value="" />` alan. Bulabilir ve bu değeri (GUID) kopyalama **genel bakış** uygulamalarınızın ya da sayfanın. 
  
- AppProxyNativeAppSample yapıştırın **uygulama (istemci) kimliği** içinde `<add key="ida:ClientId" value="" />` alan. Bulabilir ve bu değeri (GUID) AppProxyNativeAppSample kopyalayın **genel bakış** sayfası.
  
- AppProxyNativeAppSample yapıştırın **yeniden yönlendirme URI'si** içinde `<add key="ida:RedirectUri" value="" />` alan. Bulabilir ve bu değeri (URI) AppProxyNativeAppSample kopyalayın **kimlik doğrulaması** sayfası. 
  
- SecretAPI yapıştırın **uygulama kimliği URI'si** içinde `<add key="todo:TodoListResourceId" value="" />` alan. Bulabilir ve bu değeri (URI) SecretAPI kopyalayın **bir API'yi kullanıma sunmak** sayfası.
  
- SecretAPI yapıştırın **giriş sayfası URL'si** içinde `<add key="todo:TodoListBaseAddress" value="" />` alan. Bulabilir ve bu değeri (URL) SecretAPI kopyalayın **markalama** sayfası.

Parametreleri yapılandırdıktan sonra derleyin ve yerel uygulamasında çalıştırın. Seçtiğinizde, **oturum** düğmesi uygulamanın oturum açmanızı sağlar ve SecretAPI için bağlı olan başarıyla doğrulamak için bir başarılı ekranı görüntüler.

![Başarılı](./media/application-proxy-secure-api-access/success.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: Azure Active Directory Uygulama proxy'si aracılığıyla uzaktan erişim için şirket içi uygulama ekleme](application-proxy-add-on-premises-application.md)
- [Hızlı Başlangıç: Web API'leri erişmek için bir istemci uygulaması yapılandırma](../develop/quickstart-configure-app-access-web-apis.md)
- [Proxy uygulamaları ile etkileşim kurmak yerel istemci uygulamaları etkinleştirme](application-proxy-configure-native-client-application.md)
