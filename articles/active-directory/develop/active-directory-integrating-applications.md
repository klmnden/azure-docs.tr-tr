---
title: "Uygulamaları Azure Active Directory ile tümleştirme"
description: "Ekle, Güncelleştir veya Azure Active Directory (Azure AD) bir uygulamayı kaldırmak nasıl."
services: active-directory
documentationcenter: 
author: bryanla
manager: mbaldwin
editor: mbaldwin
ms.assetid: ae637be5-0b71-4b1e-b1fe-b83df3eb4845
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/04/2017
ms.author: bryanla
ms.custom: aaddev
ms.reviewer: luleon
ms.openlocfilehash: 8a5eab88e10b330bf4da88c01d24a11e95277439
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2017
---
# <a name="integrating-applications-with-azure-active-directory"></a>Uygulamaları Azure Active Directory ile tümleştirme
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Kurumsal geliştiriciler ve yazılım olarak-hizmet (SaaS) sağlayıcıları geliştirme ticari bulut hizmetlerini veya Azure Active Directory (güvenli oturum açma ve kimlik doğrulama sağlamak için Azure AD) ile tümleştirilebilir satır iş kolu uygulamaları için kendi Hizmetler. Bir uygulama veya hizmet Azure AD ile tümleştirmek için bir geliştirici önce Azure AD ile uygulama kaydetmeniz gerekir.

Bu makalede ekleme, güncelleştirme veya Azure AD'de bir uygulamanın kayıt kaldırma gösterilmektedir. Azure AD ile tümleşik uygulamalar farklı uygulama türleri hakkında bilgi edinin ve web API'leri gibi diğer kaynaklarına erişmek için uygulamaların nasıl yapılandırılacağı.

Kayıtlı bir uygulama ve bunları arasındaki ilişkiyi temsil eden iki Azure AD nesneleri hakkında daha fazla bilgi için bkz: [uygulama ve hizmet sorumlusu nesneleri](active-directory-application-objects.md); Azure Active Directory ile geliştirme uygulamaları gördüğünüzde kullanmalıdır markalama talimatları hakkında daha fazla bilgi için [tümleşik uygulamalar için markalama yönergeleri](active-directory-branding-guidelines.md).

## <a name="adding-an-application"></a>Bir uygulama ekleme
Azure AD özelliklerini kullanmak istediği herhangi bir uygulama ilk Azure AD kiracısı kayıtlı olması gerekir. Bu kayıt işlemi nerede, bir kullanıcının kimliği doğrulandıktan sonra yanıtları göndermek için URL'yi URL gibi uygulamanızı Azure AD ayrıntılarını vermiş içerir uygulama ve benzeri tanımlayan URI.

### <a name="to-register-a-new-application-using-the-azure-portal"></a>Azure Portalı'nı kullanarak yeni bir uygulama kaydetmek için
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Birden fazla erişim, sağ üst köşedeki hesabınızda tıklayın ve istenen Azure AD portalı oturumunuz ayarlayın, hesap sağlar, Kiracı.
3. Sol gezinti bölmesinde **Azure Active Directory** hizmet, tıklatın **uygulama kayıtlar**, tıklatıp **yeni uygulama kaydı**.

   ![Yeni uygulamayı Kaydet](./media/active-directory-integrating-applications/add-app-registration.png)

4. Zaman **oluşturma** sayfası görüntülenirse, uygulamanızın kayıt bilgileri girin: 

  - **Ad:** anlamlı uygulama adı girin
  - **Uygulama türü:** 
    - "Yerel" seçin [istemci uygulamaları](active-directory-dev-glossary.md#client-application) yüklenen yerel olarak bir cihaz üzerinde. Bu ayar için OAuth ortak kullanılır [yerel istemci](active-directory-dev-glossary.md#native-client).
    - Seçin "Web uygulaması / API" için [istemci uygulamaları](active-directory-dev-glossary.md#client-application) ve [kaynak/API uygulamaları](active-directory-dev-glossary.md#resource-server) güvenli bir sunucu üzerinde yüklü. Bu ayar OAuth gizli kullanılır [web istemcileri](active-directory-dev-glossary.md#web-client) ve ortak [kullanıcı aracısı tabanlı istemciler](active-directory-dev-glossary.md#user-agent-based-client). Aynı uygulama aynı zamanda hem istemci hem de kaynak/API getirebilir.
  - **Oturum açma URL'si:** için "Web uygulaması / API" uygulamaları, uygulamanızı temel URL'sini sağlayın. Örneğin, `http://localhost:31544` yerel makine üzerinde çalışan bir web uygulaması URL'si olabilir. Kullanıcıların web istemci uygulaması için oturum açmak için bu URL'yi kullanırsınız. 
  - **Yeniden yönlendirme URİ'si:** "Yerel" uygulamalar için belirteç yanıtları döndürmek için Azure AD tarafından kullanılan URI sağlayın. Uygulamanız için belirli bir değer girin, örneğin`http://MyFirstAADApp`

   ![Yeni bir uygulama register - oluşturun](./media/active-directory-integrating-applications/add-app-registration-create.png)

   Belirli örnekler web uygulamaları veya yerel uygulamaları için isterseniz, kullanıma bizim [quickstarts](active-directory-developers-guide.md#get-started).

5. Tamamlandığında tıklatarak **oluşturma**. Azure AD benzersiz bir uygulama kimliği uygulamanıza atar ve uygulamanızın ana kayıt sayfasına yönlendirilirsiniz. Uygulamanızı bir web veya yerel uygulama olmasına bağlı olarak, ek yetenekler uygulamanıza eklemek için farklı seçenekleri sunulur. Uygulama kaydınızı (kimlik bilgileri, etkinleştirme oturum açma diğer kiracılardan gelen kullanıcılar için izinler) ek yapılandırma özelliklerini etkinleştirmek için onay ve ayrıntıları genel bir bakış için bir sonraki bölümüne bakın

  > [!NOTE]
  > Varsayılan olarak, yeni kayıtlı uygulama izin verecek şekilde yapılandırılmış **yalnızca** uygulamanıza oturum açmak için aynı Kiracı kullanıcılardan.
  > 
  > 

## <a name="updating-an-application"></a>Bir uygulamayı güncelleştirme
Uygulamanızı Azure AD ile kaydedildikten sonra web API'leri erişim sağlamak için diğer kuruluşların ve daha fazla kullanılabilir duruma güncelleştirilmesi gerekebilir. Bu bölümde, uygulamanız daha fazla yapılandırabileceğiniz çeşitli yolları açıklanmaktadır. İlk olarak biz diğer kullanıcıları veya uygulamalar tarafından kullanılmak üzere ihtiyaç duyan uygulamalar oluştururken anlamak önemlidir izin framework genel bir bakış ile başlatın.

### <a name="overview-of-the-consent-framework"></a>Onay Framework'e Genel Bakış

Azure AD onay framework çok kiracılı web ve çok katmanlı uygulamalar dahil olmak üzere, yerel istemci uygulamaları geliştirmek kolaylaştırır. Bu uygulamalar, uygulama kayıtlı olandan farklı bir Azure AD kiracısı oturum açma ana kadar kullanıcı hesapları tarafından izin. Web API'ları (Azure Active Directory, Intune ve Office 365'te hizmetlerine erişmek için) Microsoft grafik API'si gibi ve web API'leri yanı sıra diğer Microsoft Hizmetleri API'leri erişmek de gerekebilir. Bir kullanıcı veya yönetici dizin verilerine erişme gerektirebilir kendi dizininde kaydedilecek soran bir uygulamaya izin vermiş framework dayanır.

Bir web istemci uygulaması, Office 365'ten Takvim kullanıcı hakkındaki bilgileri okumak gerekirse, örneğin, bu kullanıcı istemci uygulamasına ilk onayı için gereklidir. İzin verilen sonra istemci uygulamasının kullanıcı adına Microsoft Graph API çağrısı ve Takvim bilgileri gerektiği gibi kullanın. [Microsoft Graph API](https://graph.microsoft.io) kullanıcıları ve grupları Azure AD'den ve diğer veri nesneleri daha fazla Microsoft bulut hizmetlerinden yanı sıra (takvimleri ve Exchange, siteler ve SharePoint, OneDrive, OneNote defterlerinden, Planlayıcısı görevlerden, vb. Excel çalışma kitaplarından belgelerden listelerinden iletileri) gibi Office 365'te verilere erişim sağlar. 

Gibi ortak veya gizli istemcileri kullanarak Yetkilendirme kodu verin ve istemci kimlik bilgileri vermenizi izin framework OAuth 2.0 ve onun çeşitli akışları yerleşik olarak bulunur. OAuth 2.0 kullanarak, Azure AD, bir telefon, tablet, sunucu veya bir web uygulaması gibi farklı türlerde istemci uygulamaları oluşturmak ve gerekli kaynakları erişim kazanmak mümkün kılar.

Onay framework OAuth2.0 yetkilendirme verir ile kullanma hakkında daha fazla bilgi için bkz: [OAuth 2.0 ve Azure AD kullanarak web uygulamalarına erişim yetkisi](active-directory-protocols-oauth-code.md) ve[kimlik doğrulama senaryoları için Azure AD](active-directory-authentication-scenarios.md). Office 365 Microsoft Graph aracılığıyla yetkili erişimi alma hakkında daha fazla bilgi için bkz: [Microsoft Graph ile uygulama kimlik doğrulaması](https://graph.microsoft.io/docs/authorization/auth_overview).

#### <a name="example-of-the-consent-experience"></a>Onayı deneyimi örneği

Aşağıdaki adımlar nasıl onayı deneyimi uygulama geliştiricisi ve kullanıcı için çalışır gösterir.

1. Belirli bir kaynak/API'sine erişim izni istemek için gereken bir web istemci uygulaması olduğunu varsayın. Sonraki bölümde bu yapılandırmada yapmak öğreneceksiniz, ancak aslında Azure portal yapılandırması aynı anda izin istekleri bildirmek için kullanılır. Diğer yapılandırma ayarları gibi bunlar uygulamanın Azure AD kaydını bir parçası haline gelir:
   
  ![Diğer uygulamalara izinler](./media/active-directory-integrating-applications/requiredpermissions.png)
    
2. Uygulamanızın izinlerini güncelleştirildi, uygulaması çalıştıran ve yaklaşık ilk kez kullanmak için bir kullanıcı olduğunu göz önünde bulundurun. İlk Azure AD'den ait bir yetkilendirme kodu almak uygulamanın ihtiyacı `/authorize` uç noktası. Yetkilendirme kodu sonra yeni bir erişim edinmeli ve belirteç yenilemek için kullanılabilir.

3. Kullanıcı kimliği doğrulanmış, Azure AD değilse `/authorize` oturum açma için uç nokta ister.
   
  ![Azure ad kullanıcı veya yönetici oturumu açma](./media/active-directory-integrating-applications/usersignin.png)

4. Kullanıcı oturum açtıktan sonra Azure AD kullanıcı bir onay sayfasına gerekip gerekmediğini belirler. Bu belirleme, kullanıcı (veya kuruluşun yönetici) zaten uygulama izin verilip üzerinde temel alır. İzin zaten verilmiş değil, Azure AD kullanıcıdan izin ve çalışması için gerekli olan gerekli izinleri görüntüler. Onay iletişim kutusunda görüntülenen izinler kümesini Azure portalında izinlere temsilci olarak seçilen dosyalardan eşleşmesi.
   
  ![Kullanıcı onayı deneyimi](./media/active-directory-integrating-applications/consent.png)

5. Kullanıcı izin veren sonra bir erişim belirteci alması ve belirteç yenilemek için kullanılan uygulamanız için bir kimlik doğrulama kodu döndürülür. Bu akış hakkında daha fazla bilgi için bkz: [web uygulaması için Azure AD kimlik doğrulama senaryoları API bölümünde web](active-directory-authentication-scenarios.md#web-application-to-web-api).

6. Yönetici olarak, aynı zamanda tüm kullanıcılar adına bir uygulamanın temsilci izinleri kiracınızda sayılırsınız. Yönetici onayı onay iletişim Kiracı her kullanıcı için görüntülenmesini engeller ve uygulamanızı yapılır sayfasındaki [Azure portal](https://portal.azure.com). Gelen **ayarları** sayfasında uygulamanız için **gerekli izinler** ve tıklayın **izinler** düğmesi. 

  ![Açık yönetici onayı için izinler](./media/active-directory-integrating-applications/grantpermissions.png)
    
  > [!NOTE]
  > Açık verme onayı kullanarak **izinler** düğmesini ADAL.js kullanan tek sayfa uygulamaları için (SPA) şu anda gerekli. Erişim belirteci istendiğinde, aksi takdirde uygulama başarısız olur.   

### <a name="configure-a-client-application-to-access-web-apis"></a>Web API'leri erişmek için bir istemci uygulaması yapılandırın
Kimlik doğrulaması gerektiren bir yetkilendirme grant akışı katılmak (ve bir erişim belirteci almak üzere) kullanabilmek için web/gizli bir istemci uygulaması için sırayla güvenli kimlik bilgileri oluşturmanız gerekir. Azure portal tarafından desteklenen varsayılan kimlik doğrulama yöntemidir istemci kimliği + gizli anahtarı. Bu bölüm, gizli anahtar, istemcinin kimlik bilgilerini sağlamak için gerekli yapılandırma adımları kapsar.

Bir istemci bir web API kaynak uygulama (örneğin, Microsoft Graph API) tarafından sunulan erişebilmeniz için önce ek olarak, onay framework istemci alır gereken izin verme sağlar istenen izinlerine göre. Varsayılan olarak, tüm uygulamaları izinleri "Windows Azure Active Directory" (grafik API'si) ve "Windows Azure Hizmet Yönetimi API'si." seçebilirsiniz [Grafik API'si "oturum açma ve okuma kullanıcı profili" izni](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes#PermissionScopeDetails) de varsayılan olarak seçilidir. İstemci Office 365'e abone hesaplarına sahip bir kiracı kaydedilmekte olan, Web API'ları ve SharePoint ve Exchange Online için izinleri seçilebilir. Aralarından seçim yapabileceğiniz [iki tür izin](active-directory-dev-glossary.md#permissions) her web API istenen için:

- Uygulama izinleri: Web API kendisini (kullanıcı içerik yok) olarak doğrudan erişmek istemci uygulamanız gerekir. Bu türdeki izinler yönetici izni gerektirir ve ayrıca yerel istemci uygulamaları için kullanılabilir değil.

- Temsilci izinleri: Oturum açmış kullanıcı, ancak seçilen izinle sınırlı erişimi olan web API erişmek istemci uygulamanız gerekir. İzni yönetici izni gerektirmedikçe izni bu tür bir kullanıcı tarafından verilebilir. 

  > [!NOTE]
  > Azure Klasik Portalı'nda yaptığınız gibi bir temsilci izni uygulamaya ekleme otomatik olarak izin Kiracı içinde kullanıcılara vermez. Kullanıcıların gerekir yine de el ile onayı eklenen izinlere temsilci çalışma zamanında için yönetici tıklatır sürece **izinler** gelen düğmesini **gerekli izinler** bölümü Azure Portalı'nda uygulama sayfası. 

#### <a name="to-add-application-credentials-or-permissions-to-access-web-apis"></a>Uygulama kimlik bilgilerini ya da web API'leri erişim izinleri eklemek için
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Birden fazla erişim, sağ üst köşedeki hesabınızda tıklayın ve istenen Azure AD portalı oturumunuz ayarlayın, hesap sağlar, Kiracı.
3. Sol gezinti bölmesinde **Azure Active Directory** hizmeti,'ı tıklatın **uygulama kayıtlar**, ardından Bul/yapılandırmak istediğiniz uygulama'ı tıklatın.

   ![Bir uygulamanın kayıt güncelleştir](./media/active-directory-integrating-applications/update-app-registration.png)

4. Açılan uygulamanın ana kayıt sayfasına yönlendirilirsiniz **ayarları** uygulama için sayfa. Web uygulamanızın kimlik bilgilerini için gizli bir anahtar eklemek için:
  - Tıklatın **anahtarları** bölümünde **ayarları** sayfası.  
  - Anahtarınız için bir açıklama ekleyin.
  - Bir veya iki yıllık süresini seçin.
  - **Kaydet** düğmesine tıklayın. Yapılandırma değişiklikleri kaydettikten sonra en sağdaki sütun anahtar değeri içerir. **Anahtar kopyaladığınızdan emin olun** istemci uygulama kodunuzda kullanmak için bir kez erişilebilir olmadığı bu sayfadan ayrılmak.

  ![Bir uygulamanın kayıt - anahtarları güncelleştir](./media/active-directory-integrating-applications/update-app-registration-settings-keys.png)

5. Kaynak API'leri istemcinizden erişmek için işlemlerinizi eklemek için
  - Tıklatın **gerekli izinler** bölümünde **ayarları** sayfası. 
  - Tıklatın **Ekle** düğmesi.
  - Tıklatın **bir API seçin** seçim istediğiniz kaynak türünü seçin.
  - Kullanılabilir API'ler listesini gözden geçirmek veya web API'si ortaya kullanılabilir kaynak uygulamalardan dizininizde seçmek için arama kutusunu kullanın. İlgilendiğiniz, ardından tıklatın kaynağa tıklayın **seçin**.
  - İçin geçen **erişimi etkinleştir** sayfası. Uygulama izinlerini seçin ve/veya API erişirken izinlere temsilci uygulamanız gerekir.
   
  ![Bir uygulamanın kayıt - izinlerini güncelleştirme API](./media/active-directory-integrating-applications/update-app-registration-settings-permissions-api.png)

  ![Bir uygulamanın kayıt - izinleri izinleri güncelleştirme](./media/active-directory-integrating-applications/update-app-registration-settings-permissions-perms.png)

6. Tamamlandığında, tıklatın **seçin** düğmesini **erişimi etkinleştir** sayfasında, sonra **Bitti** düğmesini **API Ekle erişim** sayfası. Döndürülürsünüz **gerekli izinleri** sayfası, burada yeni kaynak eklenir API'lerin listesi.

  > [!NOTE]
  > Tıklatarak **Bitti** düğmesini de otomatik olarak izinleri ayarlar, uygulamanız için yapılandırdığınız diğer uygulamalara izinler göre dizininizde.  Uygulamayı bakarak bu uygulama izinlerini görüntüleyebildiği **ayarları** sayfası.
  > 
  > 

### <a name="configuring-a-resource-application-to-expose-web-apis"></a>Web API'leri kullanıma sunmak için bir kaynak uygulamasını yapılandırma

Web API'si geliştirmek ve erişim göstererek istemci uygulamaları için kullanılabilir hale [kapsamları](active-directory-dev-glossary.md#scopes) ve [rolleri](active-directory-dev-glossary.md#roles). Doğru yapılandırılmış web API yalnızca diğer Microsoft web gibi API'leri, grafik API'sini ve Office 365 API'leri dahil olmak üzere kullanılabilir hale getirilir. Erişim kapsamları ve rolleri aracılığıyla sunulur, [uygulama bildirimi](active-directory-dev-glossary.md#application-manifest), uygulamanızın kimlik yapılandırması temsil eden bir JSON dosyası olduğu. 

Aşağıdaki bölümde kaynak uygulama bildirimi değiştirerek erişim kapsamları kullanıma gösterilmiştir.

#### <a name="adding-access-scopes-to-your-resource-application"></a>Erişim kapsamları kaynak uygulamanıza ekleme

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Birden fazla erişim, sağ üst köşedeki hesabınızda tıklayın ve istenen Azure AD portalı oturumunuz ayarlayın, hesap sağlar, Kiracı.

3. Sol gezinti bölmesinde **Azure Active Directory** hizmeti,'ı tıklatın **uygulama kayıtlar**, ardından Bul/yapılandırmak istediğiniz uygulama'ı tıklatın.

   ![Bir uygulamanın kayıt güncelleştir](./media/active-directory-integrating-applications/update-app-registration.png)

4. Açılan uygulamanın ana kayıt sayfasına yönlendirilirsiniz **ayarları** uygulama için sayfa. Geçiş **düzenleme bildirimi** tıklatarak sayfa **bildirim** uygulamanın kayıt sayfasından. Olanak tanıyan web tabanlı bir bildirim Düzenleyicisi açılır **Düzenle** portalındaki bildirimi. İsteğe bağlı olarak, tıklayabilirsiniz **karşıdan** ve yerel olarak düzenleyin, sonra kullanın **karşıya** uygulamanızı yeniden uygulamak için.

5. Bu örnekte, biz adlı yeni bir kapsam açığa çıkarır `Employees.Read.All` bizim kaynak/aşağıdaki JSON öğesine ekleyerek API üzerinde `oauth2Permissions` koleksiyonu. Varolan `user_impersonation` kapsam, kayıt sırasında varsayılan olarak sağlanır. `user_impersonation`oturum açmış kullanıcının kimliğini altında kaynağa erişim izni istemek bir istemci uygulaması sağlar. Varolan sonra virgül eklediğinizden emin olun `user_impersonation` kapsam öğesi ve özellik değerlerini, kaynağın gereksinimlerinize uyacak şekilde değiştirin. 

  ```json
  {
    "adminConsentDescription": "Allow the application to have read-only access to all Employee data.",
    "adminConsentDisplayName": "Read-only access to Employee records",
    "id": "2b351394-d7a7-4a84-841e-08a6a17e4cb8",
    "isEnabled": true,
    "type": "User",
    "userConsentDescription": "Allow the application to have read-only access to your Employee data.",
    "userConsentDisplayName": "Read-only access to your Employee records",
    "value": "Employees.Read.All"
  }
  ```
  > [!NOTE]
  > "İd" değeri bir GUID oluşturma aracı kullanılarak oluşturulması gerektiğini [GUIDgen](https://msdn.microsoft.com/library/ms241442%28v=vs.80%29.aspx) veya program aracılığıyla. Bu web API'si tarafından gösterilen kapsamı için benzersiz bir tanımlayıcı temsil eder. Bir istemci web API erişim izinleriyle birlikte uygun şekilde yapılandırıldıktan sonra Azure AD tarafından bir OAuth2.0 erişim belirteci verilir. İstemci web API çağrıları, bu kapsamı (scp) erişim belirteci gösterdiğinde talep kendi uygulama kaydı istenen izinleri ayarlayın.
  >
  > Ek kapsamlar daha sonra gerektikçe getirebilir. Web API çeşitli farklı işlevler ile ilişkili birden çok kapsam doğurabilir göz önünde bulundurun. Kaynağınız kapsamı değerlendirerek çalışma zamanında web API erişimi denetleyebilirsiniz (`scp`) talepleri alınan OAuth 2.0 erişim belirteci.
  > 

6. Tamamlandığında tıklatarak **kaydetmek**. Artık web API'nizi dizininizde diğer uygulamalar tarafından kullanılmak üzere yapılandırılmıştır.  

  ![Bir uygulamanın kayıt güncelleştir](./media/active-directory-integrating-applications/update-app-registration-manifest.png)

#### <a name="verify-the-web-api-is-exposed-to-other-applications-in-your-tenant"></a>Web API'si, Kiracı diğer uygulamalarda maruz doğrulayın
1. Azure AD'nize Kiracı, tıklatın gidin **uygulama kayıtlar** yeniden sonra Bul/yapılandırmak istediğiniz istemci uygulaması tıklatın.

   ![Bir uygulamanın kayıt güncelleştir](./media/active-directory-integrating-applications/update-app-registration.png)

2. De yaptığınız gibi 5. adımı yineleyin [web API'leri erişmek için bir istemci uygulaması yapılandırın](#configure-a-client-application-to-access-web-apis). Ulaştığınızda **bir API seçin** adım, arama alanında, uygulama adı girerek, kaynak için arama ve tıklatın **seçin**. 

3. Üzerinde **erişimi etkinleştir** sayfası için istemci izni isteklerini kullanılabilir yeni kapsam görmeniz gerekir.

  ![Yeni izinler gösterilir](./media/active-directory-integrating-applications/update-app-registration-settings-permissions-perms-newscopes.png)

#### <a name="more-on-the-application-manifest"></a>Uygulama bildirimi hakkında daha fazla bilgi

Uygulama bildirimini, aslında biz ele alınan API erişim kapsamları dahil olmak üzere Azure AD uygulama kimliği yapılandırma tüm özniteliklerini tanımlayan uygulama varlığı güncelleştirmek için bir mekanizma olarak görev yapar. Uygulama varlığı ve kendi şeması hakkında daha fazla bilgi için bkz: [Graph API uygulaması varlık belgelerine](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity). Makale, API izinlerini belirtmek için kullanılan uygulama varlık üyeleri tam başvuru bilgileri içerir dahil olmak üzere:  

- Bir koleksiyonu appRoles üye, [uygulama rolü](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type) tanımlamak için kullanılan varlıkların [uygulama izinleri](active-directory-dev-glossary.md#permissions) web API'si için. 
- Bir koleksiyonu oauth2Permissions üye, [OAuth2Permission](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type) tanımlamak için kullanılan varlıkların [izinleri atanmış](active-directory-dev-glossary.md#permissions) web API'si için.

Uygulama hakkında daha fazla bilgi için genel kavramlar bildirim, bkz: [Azure Active Directory Uygulama bildirimini anlama](active-directory-application-manifest.md).

### <a name="accessing-the-azure-ad-graph-and-office-365-via-microsoft-graph-apis"></a>Azure AD Graph ve Office 365 Microsoft Graph API aracılığıyla erişme  

Gösterme/API'leri, kendi uygulamalarınızda erişmenin yanı sıra daha önce belirtildiği gibi istemci uygulamanızın Microsoft kaynaklar tarafından kullanıma sunulan API'lere erişim kaydedebilirsiniz. Microsoft Graph API başvurduğu portal'ın kaynak/API listesinde, "Microsoft Graph" olarak Azure AD ile kayıtlı olan tüm uygulamaları için kullanılabilir. Bir Office 365 aboneliği için kaydolup hesaplarını içeren bir kiracı istemci uygulamanızı kaydediyorsanız çeşitli Office 365 kaynaklar tarafından kullanıma sunulan kapsamları da erişebilirsiniz.

Microsoft Graph API'si tarafından kullanıma sunulan kapsamlar üzerinde tam hakkında bilgi için bkz [izin kapsamları | Microsoft Graph API kavramları](https://graph.microsoft.io/docs/authorization/permission_scopes) makalesi.

> [!NOTE]
> Geçerli bir sınırlama nedeniyle "kuruluşunuzun dizinine erişim" izni kullanırsanız, yerel istemci uygulamaları Azure AD Graph API yalnızca çağırabilir. Bu kısıtlama, web uygulamaları için geçerli değil.
> 
> 

### <a name="configuring-multi-tenant-applications"></a>Çok kiracılı uygulamaları yapılandırma

Bir uygulamayı Azure AD'de kaydederken, yalnızca kuruluşunuzdaki kullanıcılar tarafından erişilecek, uygulamanızın isteyebilirsiniz. Alternatif olarak, uygulamanızın dış kuruluşlar bulunan kullanıcılar tarafından erişilebilir olmasını isteyebilirsiniz. Bu iki uygulama türleri tek Kiracı ve çok kiracılı uygulamalar olarak adlandırılır. Bu bölümde bir çok kiracılı uygulama yapmak için bir tek kiracılı uygulama yapılandırmasının nasıl değiştirileceği açıklanmaktadır.

Tek Kiracı ve çok kiracılı bir uygulama arasındaki farklılıkları dikkate almak önemlidir:  

- Tek kiracılı uygulama bir kuruluştaki kullanıma yöneliktir. Genellikle, bir kurumsal geliştirici tarafından yazılan bir satır iş kolu (LoB) uygulaması değil. Tek kiracılı uygulama yalnızca aynı Kiracı uygulama kaydı olarak hesaplarına sahip kullanıcılar tarafından erişilebilir. Sonuç olarak, bu yalnızca bir dizinde sağlanması gerekir.
- Çok kiracılı uygulama birçok kuruluş içinde kullanıma yöneliktir. Başvurulan bir hizmet olarak yazılım (SaaS) web uygulaması olarak, bunu bir bağımsız yazılım satıcısı (ISV) tarafından genellikle yazılır. Çok kiracılı uygulamalara nerede kullanıcıların erişmesi gereken her Kiracı sağlanmalıdır. Uygulama, kayıtlı bir dışında kiracılar için bunları kaydetmek için kullanıcı veya yönetici onayı gereklidir. Kaynak sahibinin cihazda yüklü olan gibi yerel istemci uygulamalar varsayılan olarak çok kiracılı olduğuna dikkat edin. Yukarıdaki bkz [izin Framework'e Genel Bakış](#overview-of-the-consent-framework) izin framework hakkında ayrıntılı bilgi için bölüm.

Bir uygulama çok kiracılı yapmadan hem uygulama kayıt yapılmasını yanı web uygulama kendisini değiştirir. Aşağıdaki bölümlerde her ikisi de kapsar.

#### <a name="changing-the-application-registration-to-support-multi-tenant"></a>Çok kiracılı desteklemek için uygulama kaydı değiştirme

Müşterilerinizi ve iş ortakları, kuruluşunuz dışında için kullanılabilir hale getirmek istediğiniz bir uygulama yazıyorsanız, Azure portalında uygulama tanımı güncelleştirmeniz gerekir.

> [!IMPORTANT]
> Azure AD uygulama kimliği URI'si çok kiracılı uygulamaların genel olarak benzersiz olması gerekir. Uygulama Kimliği URI'si uygulama protokolü iletilerinde tanımlanan yollardan biridir. Tek kiracılı uygulama için Kiracı içinde benzersiz olması uygulama kimliği URI'si için yeterli olur. Azure AD uygulama tüm kiracılar bulabilmek için çok kiracılı uygulama için genel olarak benzersiz olmalıdır. Genel benzersizlik Azure AD kiracısı doğrulanmış bir etki alanı ile eşleşen bir ana bilgisayar adı uygulama kimliği URI'si gerektirerek zorlanır. Örneğin, Kiracı adı contoso.onmicrosoft.com ise geçerli bir uygulama kimliği URI https://contoso.onmicrosoft.com/myapp olur. Doğrulanmış bir etki alanı contoso.com kiracınız varsa, geçerli bir uygulama kimliği URI'sini de https://contoso.com/myapp olacaktır. Çok kiracılı başarısız olarak bir uygulama ayarı, uygulama kimliği URI'si bu deseni izlemenizi değil
> 

Dış kullanıcılar, uygulama erişim vermek için: 

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Birden fazla erişim, sağ üst köşedeki hesabınızda tıklayın ve istenen Azure AD portalı oturumunuz ayarlayın, hesap sağlar, Kiracı.
3. Sol gezinti bölmesinde **Azure Active Directory** hizmeti,'ı tıklatın **uygulama kayıtlar**, ardından Bul/yapılandırmak istediğiniz uygulama'ı tıklatın. Açılan uygulamanın ana kayıt sayfasına yönlendirilirsiniz **ayarları** uygulama için sayfa.
4. Gelen **ayarları** sayfasında, **özellikleri** değiştirip **çoklu kiralanan** geçiş **Evet**.

Değişiklikleri yaptıktan sonra kullanıcılar ve Yöneticiler, diğer kuruluşların uygulamanızı kendi Kiracı tarafından güvenli hale getirilmiş kaynaklara erişmesine izin vererek, kullanıcılar uygulamanıza oturum açmak için hakkı da verebilirsiniz.

#### <a name="changing-the-application-to-support-multi-tenant"></a>Çok kiracılı destekleyecek şekilde değiştirme

Çok kiracılı uygulamalar için destek üzerinde Azure AD onay framework yoğun olarak kullanır. Onay, kullanıcının Kiracı tarafından güvenli hale getirilmiş kaynaklara uygulama erişim vermek için başka bir kiracı, gelen bir kullanıcı izin veren mekanizmasıdır. Bu deneyim "kullanıcı izni." adlandırılır

Web uygulamanız da teklif edebilir:

- Özelliği yöneticilerin "Şirketim imzalamak." İçin "yönetici onayı" adlandırılan bu deneyim, yönetici adına izin vermek için yeteneği verir. *tüm kullanıcılar* kuruluşlarındaki. Yalnızca genel Yönetici rolüne ait bir hesap kimlik doğrulamasını bir kullanıcı yönetici onayı sağlayabilir; diğer bir hata alırsınız.

- Kullanıcılar için kaydolma bir deneyim. Kullanıcı tarayıcı için Azure AD OAuth2.0 yönlendirir bir "kaydolma" düğmesi sağlanır beklenir `/authorize` uç noktası veya bir Openıd Connect `/userinfo` uç noktası. Bu uç noktalar id_token inceleyerek yeni kullanıcı hakkında bilgi almak için uygulama izin verin. Kullanıcı de gösterilene benzer bir onay istemi olarak sunulan kaydolma aşaması aşağıdaki [izin Framework'e Genel Bakış](#overview-of-the-consent-framework) bölümü.

Çoklu kiralanan erişim ve oturum-açma/kaydolma deneyimleri desteklemek için gereken uygulama değişiklikleri hakkında daha fazla bilgi için bkz:

- [Çok kiracılı uygulama desenini kullanarak istediğiniz bir Azure Active Directory (AD) kullanıcısı ile oturum açma](active-directory-devhowto-multi-tenant-overview.md)
- Listesini [çok kiracılı kod örnekleri](https://azure.microsoft.com/documentation/samples/?service=active-directory&term=multi-tenant). 
- [Hızlı Başlangıç: Şirket Azure AD'de, oturum açma sayfasına markası ekleme](../customize-branding.md)

### <a name="enabling-oauth-20-implicit-grant-for-single-page-applications"></a>OAuth etkinleştirme 2.0 örtük tek sayfa uygulamaları için verme

Tek sayfa uygulamanın (SPAs), genellikle, iş mantığını gerçekleştirmek için arka uç uygulama web API'si çağıran tarayıcıda çalışan JavaScript ağır ön uç ile yapılandırılmıştır. Azure AD içinde barındırılan SPAs için Azure AD ile kullanıcının kimliğini doğrulamak ve uygulamanın JavaScript istemci çağrılarından kendi arka uç Web API güvenliğini sağlamak için kullanabileceğiniz bir belirteç elde etmek için OAuth 2.0 örtük verme kullanın. 

Kullanıcı izin verildi sonra bu aynı kimlik doğrulama protokolü çağrıları diğer web ile istemci arasında uygulama için yapılandırılan API kaynakları güvenli hale getirmek için belirteçleri elde etmek için kullanılabilir. Örtük yetkilendirme verme hakkında daha fazla bilgi ve Uygulama senaryonuz için uygun olup olmadığına karar vermenize yardımcı olması için bkz: [örtük OAuth2 anlama izin akışı Azure Active Directory'de](active-directory-dev-understanding-oauth2-implicit-grant.md).

Varsayılan olarak, OAuth 2.0 örtük Grant uygulamalar için devre dışıdır. Ayarlayarak, uygulamanız için OAuth 2.0 örtük verme etkinleştirebilirsiniz `oauth2AllowImplicitFlow` değeri kendi [uygulama bildirimi](active-directory-application-manifest.md).

#### <a name="to-enable-oauth-20-implicit-grant"></a>OAuth 2.0 örtük grant etkinleştirmek için

> [!NOTE]
> Uygulama bildirimi düzenleme hakkında ayrıntılar için önceki bölümde ilk gözden geçirdiğinizden emin [kullanıma sunmak için bir kaynak uygulaması web API'leri](#configuring-a-resource-application-to-expose-web-apis).
>

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Birden fazla erişim, sağ üst köşedeki hesabınızda tıklayın ve istenen Azure AD portalı oturumunuz ayarlayın, hesap sağlar, Kiracı.
3. Sol gezinti bölmesinde **Azure Active Directory** hizmeti,'ı tıklatın **uygulama kayıtlar**, ardından Bul/yapılandırmak istediğiniz uygulama'ı tıklatın. Açılan uygulamanın ana kayıt sayfasına yönlendirilirsiniz **ayarları** uygulama için sayfa.
4. Geçiş **düzenleme bildirimi** tıklatarak sayfa **bildirim** uygulamanın kayıt sayfasından. Olanak tanıyan web tabanlı bir bildirim Düzenleyicisi açılır **Düzenle** portalındaki bildirimi. Bulun ve "oauth2AllowImplicitFlow" değerini "true" olarak ayarlayın Varsayılan olarak, "false" olarak ayarlanır
   
  ```json
  "oauth2AllowImplicitFlow": true,
  ```
5. Güncelleştirilmiş bildirimini kaydedin. Kaydedildikten sonra web API artık kullanıcıların kimlik doğrulaması için OAuth 2.0 örtük verme kullanmak üzere yapılandırılmıştır.

## <a name="removing-an-application"></a>Bir uygulamayı kaldırma
Bu bölümde, bir uygulamanın kayıt Azure AD kiracınıza kaldırmayı açıklar.

### <a name="removing-an-application-authored-by-your-organization"></a>Kuruluşunuz tarafından yazılan bir uygulamayı kaldırma
Kuruluşunuzun kayıtlı olan uygulamaları Göster altında, kiracının ana "uygulama kayıtlar" sayfasında "Uygulamalarım" filtre. Bu olanları Azure portalı üzerinden veya program aracılığıyla PowerShell veya grafik API'si aracılığıyla el ile kayıtlı uygulamalardır. Daha açık belirtmek gerekirse, bunlar hem uygulama ve hizmet asıl nesne tarafından kiracınızda gösterilir. Daha fazla bilgi için bkz: [uygulama ve hizmet sorumlusu nesneleri](active-directory-application-objects.md).

#### <a name="to-remove-a-single-tenant-application-from-your-directory"></a>Tek kiracılı uygulama dizininizden kaldırmak için
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Birden fazla erişim, sağ üst köşedeki hesabınızda tıklayın ve istenen Azure AD portalı oturumunuz ayarlayın, hesap sağlar, Kiracı.
3. Sol gezinti bölmesinde **Azure Active Directory** hizmeti,'ı tıklatın **uygulama kayıtlar**, ardından Bul/yapılandırmak istediğiniz uygulama'ı tıklatın. Açılan uygulamanın ana kayıt sayfasına yönlendirilirsiniz **ayarları** uygulama için sayfa.
4. Uygulamanın ana kayıt sayfasından tıklatın **silmek**.
5. Tıklatın **Evet** onay iletisi.

#### <a name="to-remove-a-multi-tenant-application-from-its-home-directory"></a>Çok kiracılı uygulama giriş dizininden kaldırmak için
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Birden fazla erişim, sağ üst köşedeki hesabınızda tıklayın ve istenen Azure AD portalı oturumunuz ayarlayın, hesap sağlar, Kiracı.
3. Sol gezinti bölmesinde **Azure Active Directory** hizmeti,'ı tıklatın **uygulama kayıtlar**, ardından Bul/yapılandırmak istediğiniz uygulama'ı tıklatın. Açılan uygulamanın ana kayıt sayfasına yönlendirilirsiniz **ayarları** uygulama için sayfa.
4. Gelen **ayarları** sayfasında, **özellikleri** değiştirip **çoklu kiralanan** geçiş **Hayır**, ilk uygulamanızı değiştirmek için tek-Kiracı, ardından **kaydetmek**. Uygulamanın hizmet asıl nesneleri zaten kendisine rıza tüm organizasyonlar kiracılar kalır.
5. Tıklayın **silmek** uygulamanın ana kayıt sayfası düğmesinden.
6. Tıklatın **Evet** onay iletisi.

### <a name="removing-a-multi-tenant-application-authorized-by-another-organization"></a>Başka bir kuruluş tarafından yetkili bir çok kiracılı uygulama kaldırma
Bir alt kümesini Göster "Tüm uygulamaların" filtre altında uygulamaları (hariç "Uygulamalarım" kayıtlar), kiracının ana "Uygulama kayıtlar" sayfasında, çok kiracılı uygulamalardır. Teknik koşulları, bu çok kiracılı uygulamalar başka bir kiracıdan yüklenir ve Kiracı onay işlemi sırasında kaydettirildi. Daha açık belirtmek gerekirse, bunlar yalnızca bir hizmet asıl nesnesinde, Kiracı tarafından karşılık gelen hiçbir uygulama nesnesi ile temsil edilir. Uygulama ve hizmet asıl nesneleri arasındaki farklar hakkında daha fazla bilgi için bkz: [uygulama ve hizmet asıl nesneleri Azure AD'de](active-directory-application-objects.md).

(İzin verilen sonra) dizininiz bir çok kiracılı uygulama erişimi kaldırmak için şirket Yöneticisi kendi hizmet sorumlusu kaldırmanız gerekir. Yönetici genel yönetici erişimine sahip olmalıdır ve Azure portalı üzerinden kaldırabilirsiniz [Azure AD PowerShell cmdlet'leri](http://go.microsoft.com/fwlink/?LinkId=294151) erişimi kaldırmak için.

## <a name="next-steps"></a>Sonraki adımlar
- Kimlik doğrulaması Azure AD'de nasıl çalıştığı hakkında daha fazla bilgi için bkz: [Azure AD için kimlik doğrulama senaryoları](active-directory-authentication-scenarios.md).
- Bkz: [tümleşik uygulamalar için markalama yönergeleri](active-directory-branding-guidelines.md) uygulamanız için görsel kılavuz ipuçları için.
- Bir uygulamanın uygulama ve hizmet sorumlusu nesneleri arasındaki ilişki hakkında daha fazla bilgi için bkz: [uygulama ve hizmet sorumlusu nesneleri](active-directory-application-objects.md).
- Uygulama bildirim yürütür rolü hakkında daha fazla bilgi edinmek için [Azure Active Directory Uygulama bildirimini anlama](active-directory-application-manifest.md)
- Bkz: [Azure AD Geliştirici sözlüğü](active-directory-dev-glossary.md) bazı Azure Active Directory (AD) Geliştirici kavramları tanımları için.
- Ziyaret [Active Directory Geliştirici Kılavuzu](active-directory-developers-guide.md) Geliştirici ilişkili tüm içeriği genel bakış.

