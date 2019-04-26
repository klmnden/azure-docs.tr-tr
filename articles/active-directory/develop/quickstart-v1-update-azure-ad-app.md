---
title: Uygulamaları Azure Active Directory ile tümleştirme
description: Azure Active Directory (Azure AD) ile bir uygulamayı güncelleştirmeyi öğrenin.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: celested
ms.custom: aaddev
ms.reviewer: lenalepa, sureshja
ms.collection: M365-identity-device-management
ms.openlocfilehash: bee16ed8205453546702946628c98c73b0f34b15
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60299159"
---
# <a name="quickstart-update-an-application-in-azure-active-directory"></a>Hızlı Başlangıç: Azure Active Directory'de bir uygulamayı güncelleştirme

Uygulamalarını Azure Active Directory'ye (Azure AD) kaydetmiş olan kurumsal geliştiriciler ve hizmet olarak yazılım (SaaS) sağlayıcıları, uygulamaları diğer kuruluşlara ve daha fazla kullanıcıya sunmak için web API'leri gibi diğer kaynaklara erişecek şekilde yapılandırmaya ihtiyaç duyabilir.

Bu hızlı başlangıçta uygulamanızı sizin ve diğer kuruluşların gereksinimlerine veya ihtiyaçlarına göre yapılandırmak veya güncelleştirmek için kullanabileceğiniz yöntemleri öğreneceksiniz.

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki adımları tamamladığınızdan emin olun:

* Diğer kullanıcılar veya uygulamalar tarafından kullanılması gereken uygulamaları derleme konusunda önemli olan [Azure AD onay çerçevesi](consent-framework.md) genel bakış makalesini okuyun.
* Uygulamaların kaydedilmiş olduğu bir Azure AD kiracısı kullanın.
  * Henüz kiracınız yoksa, [nasıl alabileceğinizi öğrenin](quickstart-create-new-tenant.md).
  * Kiracınızda kayıtlı uygulama yoksa [Azure AD'den uygulama eklemeyi öğrenin](quickstart-v1-integrate-apps-with-azure-ad.md).

## <a name="configure-a-client-application-to-access-web-apis"></a>Bir istemci uygulamasını web API'lerine erişecek şekilde yapılandırma

Bir web/gizli istemci uygulamasının kimlik doğrulaması gerektiren bir yetkilendirme akışına dahil olabilmesi (ve erişim belirteci alabilmesi) için güvenli kimlik bilgileri kullanması gerekir. Azure portal tarafından desteklenen varsayılan kimlik doğrulaması yöntemi istemci kimliği ve gizli anahtar kullanımıdır. Bu bölümde istemcinizin kimlik bilgileriyle birlikte gizli anahtar sağlamak için gerekli olan yapılandırma adımları anlatılmaktadır.

İstemcinin bir kaynak uygulaması tarafından kullanıma sunulan web API'sine (Microsoft Graph API gibi) erişebilmesi için onay çerçevesi istenen izinlere bağlı olarak istemcinin gerekli izni almasını sağlar. Varsayılan olarak tüm uygulamalar **Azure Active Directory** (Graph API) ve Azure klasik dağıtım modelinden izin seçebilir. [Graph API'si “Oturum açma ve kullanıcı profilini okuma” izni](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes#PermissionScopeDetails) de varsayılan olarak seçilir. İstemciniz Office 365 aboneliğine sahip hesapların bulunduğu bir kiracıya kaydedilirse SharePoint ve Exchange Online web API'lerini ve izinlerini de seçebilirsiniz. İstenen her web API'si için iki izin türü arasından seçim yapabilirsiniz:

- Uygulama izinleri: Web API'sini doğrudan kendisi (kullanıcı içerik yok) olarak erişmek istemci uygulamanız gerekir. Bu izin türü için yönetici onayı gerekir ve bu tür yerel istemci uygulamalarında kullanılamaz.
- Temsilci izinleri: Oturum açmış kullanıcı olarak, ancak seçilen izinle sınırlı erişimi olan web API'sine erişmek istemci uygulamanız gerekir. Yönetici onayı gerektirmediği sürece bu izin türü bir kullanıcı tarafından verilebilir.

  > [!NOTE]
  > Bir uygulamaya temsilcili izin eklenmesi kiracı içindeki kullanıcılara otomatik olarak onay vermez. Yöneticinin tüm kullanıcıların adına onay vermemesi durumunda kullanıcıların yine çalışma zamanında eklenen temsilcili izinler için el ile onay vermesi gerekir.

### <a name="add-application-credentials-or-permissions-to-access-web-apis"></a>Web API'lerine erişmek için uygulama kimlik bilgileri veya izinleri ekleme

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Hesabınız birden fazla Azure AD kiracısına erişim sunuyorsa sağ üst köşeden hesabınızı seçin ve portal oturumunuzu kullanmak istediğiniz Azure AD kiracısına ayarlayın.
3. Sol taraftaki gezinti bölmesinden **Azure Active Directory** hizmetini seçin, **Uygulama kayıtları** öğesini seçin ve ardından yapılandırmak istediğiniz uygulamayı bulun/seçin.

   ![Uygulama kaydını güncelleştirme](./media/quickstart-v1-integrate-apps-with-azure-ad/update-app-registration.png)

4. Uygulamanın **Ayarlar** sayfasının bulunduğu uygulama ana kayıt sayfası açılır. Web uygulamanıza kimlik bilgileri eklemek için:
   1. **Ayarlar** sayfasının **Anahtarlar** bölümünü seçin.
   1. Sertifika eklemek için:
      - **Ortak Anahtarı Karşıya Yükle**'yi seçin.
      - Yüklemek istediğiniz dosyayı seçin. Şu dosya türlerinden biri olmalıdır: .cer, .pem, .crt.
   1. Parola eklemek için:
      - Anahtarınız için bir açıklama ekleyin.
      - Bir süre seçin.
      - **Kaydet**’i seçin. Yapılandırmada yaptığınız değişiklikleri kaydettikten sonra en sağdaki sütunda anahtar değeriniz gösterilir. İstemci uygulamanızın kodunda kullanmak üzere **anahtarı kopyalamayı unutmayın**. Bu sayfadan çıktıktan sonra anahtara erişemezsiniz.

5. İstemcinizden kaynak API'lerine erişim izni eklemek için
   1. **Ayarlar** sayfasında **Gerekli izinler** bölümünü ve ardından **Ekle**'yi seçin.
   1. Seçim yapmak istediğiniz kaynak türünü belirlemek için **Bir API seçin**'e tıklayın.
   1. Kullanabileceğiniz farklı API'lerin yer aldığı listeye göz atın veya arama kutusunu kullanarak dizininizde bulunan ve web API'si sunan kaynak uygulamalarından seçim yapın. İstediğiniz kaynağı seçin ve **Seç**'e tıklayın.
   1. **Erişimi Etkinleştir** sayfasında uygulamanızın API'ye erişmek için kullanacağı uygulama izinlerini ve/veya temsilcili izinleri seçin.
   
   ![Uygulama kaydını güncelleştirme - permissions api](./media/quickstart-v1-integrate-apps-with-azure-ad/update-app-registration-settings-permissions-api.png)

   ![Uygulama kaydını güncelleştirme - permissions perms](./media/quickstart-v1-integrate-apps-with-azure-ad/update-app-registration-settings-permissions-perms.png)

6. İşlemi tamamladığınızda **Erişimi Etkinleştir** sayfasındaki **Seç** düğmesini ve ardından **API erişimi ekle** sayfasındaki **Bitti** düğmesini seçin. Yeniden **Gerekli izinler** sayfası açılır ve yeni kaynak API listesine eklenmiş olur.

## <a name="configuring-a-resource-application-to-expose-web-apis"></a>Kaynak uygulamasını web API'lerini kullanıma sunacak şekilde yapılandırma

Bir web API'si geliştirip erişim [kapsamlarını](developer-glossary.md#scopes) ve [rolleri](developer-glossary.md#roles) kullanıma sunarak istemci uygulamaları tarafından kullanılmasını sağlayabilirsiniz. Doğru şekilde yapılandırılmış olan bir web API'sini kullanıma sunmak için yapılması gereken işlemler Graph API ve Office 365 API'leri gibi diğer Microsoft web API'leri için yapılması gerekenlerle aynıdır. Erişim kapsamları ve roller, uygulamanızın kimlik yapılandırmasını temsil eden bir JSON dosyası olan [uygulama bildiriminde](developer-glossary.md#application-manifest) kullanıma sunulur.

Aşağıdaki bölümde kaynak uygulamasının bildirimini değiştirerek erişim kapsamlarını kullanıma sunma adımları gösterilmektedir.

### <a name="add-access-scopes-to-your-resource-application"></a>Kaynak uygulamanıza erişim kapsamı ekleme

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Hesabınız birden fazla Azure AD kiracısına erişim sunuyorsa sağ üst köşeden hesabınızı seçin ve portal oturumunuzu kullanmak istediğiniz Azure AD kiracısına ayarlayın.
3. Sol taraftaki gezinti bölmesinden **Azure Active Directory > Uygulama kayıtları** seçimini yapın ve ardından yapılandırmak istediğiniz uygulamayı bulun/seçin.

   ![Uygulama kaydını güncelleştirme](./media/quickstart-v1-integrate-apps-with-azure-ad/update-app-registration.png)

4. Uygulamanın **Ayarlar** sayfasının açık olduğu uygulama ana kayıt sayfası görüntülenir. Uygulama kayıt sayfasında **Bildirim**'e tıklayarak **Bildirimi düzenle** sayfasına geçin. Bildirimi portalda **Düzenleme** seçeneği sunana web tabanlı bir bildirim düzenleyici açılır. İsteğe bağlı olarak **İndir**'e tıklayıp düzenlemelerinizi yerel ortamda yaptıktan sonra **Yükle** seçeneğiyle uygulamanıza yeniden uygulayabilirsiniz.
5. Bu örnekte `oauth2Permissions` koleksiyonuna aşağıdaki JSON öğesini ekleyerek kaynak/API öğesinde `Employees.Read.All` adlı yeni bir kapsamı kullanıma sunacağız. Var olan `user_impersonation` kapsamı kayıt sırasında varsayılan olarak sağlanmıştır. `user_impersonation`, bir istemci uygulamasının oturum açmış olan kullanıcının kimliğiyle kaynağa erişme izni istemesini sağlar. Var olan `user_impersonation` kapsam öğesinden sonra virgül eklemeyi ve özellik değerlerini kaynağınıza göre değiştirmeyi unutmayın. 

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
   > `id` Değeri programlı olarak oluşturulmalıdır veya bir GUID kullanarak oluşturma aracı gibi [GUIDgen](https://msdn.microsoft.com/library/ms241442%28v=vs.80%29.aspx). `id`, web API'si tarafından kullanıma sunulan kapsam için benzersiz tanıtıcıyı temsil eder. Bir istemci web API'nize erişmek için gerekli izinlerle uygun şekilde yapılandırıldıktan sonra Azure AD tarafından bir OAuth2.0 erişim belirteci düzenlenir. İstemci web API'sini çağırdığında uygulama kaydında istenen izinlere göre ayarlanan kapsam (scp) talebine sahip olan erişim belirtecini sunar.
   >
   > Gerekirse daha sonra ek kapsamları kullanıma sunabilirsiniz. Web API'niz farklı işlevlerle ilişkilendirilmiş birden fazla kapsamı kullanıma sunabilir. Kaynağınız alınan OAuth 2.0 erişim belirtecindeki kapsam (`scp`) taleplerini değerlendirerek web API'si erişimini çalışma zamanında denetleyebilir.

6. İşlemi tamamladıktan sonra **Kaydet**’e tıklayın. Web API'nizi dizininizdeki diğer uygulamalar tarafından kullanılacak şekilde yapılandırmış oldunuz.

   ![Uygulama kaydını güncelleştirme](./media/quickstart-v1-integrate-apps-with-azure-ad/update-app-registration-manifest.png)

### <a name="verify-the-web-api-is-exposed-to-other-applications-in-your-tenant"></a>Web API'sinin kiracınızdaki diğer uygulamaların kullanımına sunulduğunu doğrulama

1. Azure AD kiracınıza geri dönün, **Uygulama kayıtları**'nı tekrar seçin ve ardından yapılandırmak istediğiniz istemci uygulamasını bulun/seçin.

   ![Uygulama kaydını güncelleştirme](./media/quickstart-v1-integrate-apps-with-azure-ad/update-app-registration.png)

2. [Bir istemci uygulamasını web API'lerine erişecek şekilde yapılandırma](#configure-a-client-application-to-access-web-apis) bölümündeki 5. adımda gerçekleştirdiğiniz işlemleri tekrarlayın. **Bir API seçin** adımına geldiğinizde uygulama adını arama alanına girerek kaynağınızı bulun ve **Seç**'e tıklayın.

3. **Erişimi Etkinleştir** sayfasında istemci izin istekleri için kullanılabilir durumdaki yeni kapsamı görmeniz gerekir.

   ![Yeni izinler gösterilir](./media/quickstart-v1-integrate-apps-with-azure-ad/update-app-registration-settings-permissions-perms-newscopes.png)

### <a name="more-on-the-application-manifest"></a>Uygulama bildirimi hakkında daha fazla bilgi

Uygulama bildirimi, yukarıda anlatılan API erişim kapsamları dahil olmak üzere bir Azure AD uygulamasının kimlik yapılandırmasının tüm özniteliklerini tanımlayan Uygulama varlığının güncelleştirilmesi için bir uygulama bildirimi görevi görür. Uygulama varlığı ve şeması hakkında daha fazla bilgi için bkz. [Graph API Uygulama varlığı belgeleri](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity). İlgili makalede aşağıdakiler dahil olmak üzere API'niz için izin belirtmek için kullanılan Uygulama varlığı üyeleri hakkında ayrıntılı başvuru bilgileri bulunur:  

- [AppRole](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type) varlığı koleksiyonu olan ve bir web API'sinin [uygulama izinlerini](developer-glossary.md#permissions) tanımlamak için kullanılan appRoles üyesi. 
- [OAuth2Permission](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type) varlığı koleksiyonu olan ve bir web API'sinin [temsilcili izinlerini](developer-glossary.md#permissions) tanımlamak için kullanılan oauth2Permissions üyesi.

Uygulama bildirimi kavramları hakkında daha fazla genel bilgi için bkz. [Azure AD uygulama bildirimi](reference-app-manifest.md).

## <a name="accessing-the-azure-ad-graph-and-office-365-via-microsoft-graph-apis"></a>Microsoft Graph API'leriyle Azure AD Graph ve Office 365 erişimi  

Yukarıda da bahsettiğimiz üzere kendi uygulamalarınızın API'lerini kullanıma sunmaya ve erişim sağlamaya ek olarak istemci uygulamanızı Microsoft kaynakları tarafından kullanıma sunulan API'lere erişecek şekilde kaydedebilirsiniz. Portalın kaynak/API listesinde “Microsoft Graph” olarak geçen Microsoft Graph API'si, Azure AD'ye kayıtlı tüm uygulamalar tarafından kullanılabilir. İstemci uygulamanızı Office 365 aboneliğine kaydolmuş hesapların bulunduğu bir kiracıda kaydediyorsanız çeşitli Office 365 kaynakları tarafından kullanıma sunulan kapsamlara da erişebilirsiniz.

Microsoft Graph API tarafından kullanıma sunulan kapsamlarla ilgili ayrıntılı bilgi için [Microsoft Graph izinleri başvurusu](https://docs.microsoft.com/graph/permissions-reference) makalesine bakın.

> [!NOTE]
> Mevcut bir sınırlama nedeniyle yerel istemci uygulamaları yalnızca “Kuruluşunuzun dizinine erişin” iznini kullanarak Azure AD Graph API'sine çağrı gönderebilir. Bu kısıtlama web uygulamaları için geçerli değildir.

## <a name="configuring-multi-tenant-applications"></a>Çok kiracılı uygulamaları yapılandırma

Bir uygulamayı Azure AD'ye kaydederken uygulamaya yalnızca kuruluşunuzdaki kullanıcılar tarafından erişim sağlanmasını isteyebilirsiniz. Alternatif olarak uygulamanıza başka bir kuruluşun kullanıcıları tarafından erişim sağlanmasını da isteyebilirsiniz. Bu iki uygulama türü tek kiracılı ve çok kiracılı uygulama olarak adlandırılır. Bu bölümde tek kiracılı bir uygulamanın yapılandırmasını değiştirerek çok kiracılı bir uygulama haline getirme adımları açıklanmaktadır.

Tek kiracılı ve çok kiracılı uygulama arasındaki farkları göz önünde bulundurmak önemlidir:  

- Tek kiracılı bir uygulama tek bir kuruluşta kullanılmak üzere tasarlanmıştır. Genellikle kurumsal bir geliştirici tarafından yazılmış olan iş kolu (LoB) uygulamasıdır. Tek kiracılı uygulamaya yalnızca hesabı uygulama kaydıyla aynı kiracıda bulunan kullanıcılar erişebilir. Sonuç olarak tek bir dizinde sağlanması gerekir.
- Çok kiracılı bir uygulama birden fazla kuruluşta kullanılmak üzere tasarlanmıştır. Genellikle hizmet olarak yazılım (SaaS) web uygulaması olarak bilinen bu uygulama genellikle bağımsız bir yazılım satıcısı (ISV) tarafından yazılmıştır. Çok kiracılı uygulamaların kullanıcıların erişmesi gereken tüm kiracılarda sağlanması gerekir. Uygulamanın kayıtlı olduğu kiracının dışındaki kiracılarda kayıt için kullanıcı veya yönetici onayı gerekir. Yerel istemci uygulamaları kaynak sahibinin cihazına yüklendiğinden varsayılan olarak çok kiracılı olduğunu unutmayın. Önceki Ayrıntılar için onay çerçeve bölümü üzerinde onay çerçevesine genel bakış.

Bir uygulamayı çok kiracılı hale getirmek için hem uygulama kaydında hem de web uygulamasının kendisinde değişiklik yapılması gerekir. Aşağıdaki bölümde ikisi de açıklanmıştır.

### <a name="changing-the-application-registration-to-support-multi-tenant"></a>Uygulama kaydını çok kiracılı modeli destekleyecek şekilde değiştirme

Kuruluşunuzun dışındaki müşterilerinize veya iş ortaklarınıza sunmak istediğiniz bir uygulama yazıyorsanız Azure portalda uygulama tanımını güncelleştirmeniz gerekir.

> [!IMPORTANT]
> Azure AD, çok kiracılı uygulamaların Uygulama Kimliği URI'sinin genel olarak benzersiz olmasını gerektirir. Uygulama Kimliği URI'si, uygulamanın protokol iletileri içinde tanımlanması için kullanılan yollardan biridir. Tek kiracılı bir uygulamada Uygulama Kimliği URI'sinin kiracı içinde benzersiz olması yeterlidir. Azure AD'nin uygulamayı tüm kiracılar arasında bulabilmesi için çok kiracılı uygulamada bu değerin genel olarak benzersiz olması gerekir.
> Genel olarak benzersiz olma gereksinimi, Uygulama Kimliği URI'sinin Azure AD kiracısının doğrulanmış etki alanı ile eşleşen bir ana bilgisayar adına sahip olması şartıyla sağlanır. Örneğin kiracınızın adı contoso.onmicrosoft.com ise geçerli bir Uygulama Kimliği URI'si https://contoso.onmicrosoft.com/myapp şeklinde olacaktır. Kiracınızda contoso.com etki alanı doğrulanmışsa geçerli Uygulama Kimliği URI'si https://contoso.com/myapp şeklinde olacaktır. Uygulama Kimliği URI'si bu düzene uygun olmadığında uygulamayı çok kiracılı hale getirme işlemi başarısız olur.

Kiracınızın dışındaki kullanıcıların uygulamanıza erişmesini sağlamak için:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Hesabınız birden fazla Azure AD kiracısına erişim sunuyorsa sağ üst köşeden hesabınıza tıklayın ve portal oturumunuzda kullanmak istediğiniz kiracıyı belirleyin.
3. Sol taraftaki gezinti bölmesinden **Azure Active Directory** hizmetine tıklayın, **Uygulama kayıtları**'na tıklayın ve ardından yapılandırmak istediğiniz uygulamayı bulun/tıklayın. Uygulamanın **Ayarlar** sayfasının açık olduğu uygulama ana kayıt sayfası görüntülenir.
4. **Ayarlar** sayfasında **Özellikler**'e tıklayın ve **Çok kiracılı** ayarını **Evet** olarak değiştirin.

Siz gerekli değişiklikleri yaptıktan sonra diğer kuruluşlardaki kullanıcılar ve yöneticiler kullanıcılarına uygulamanızda oturum açma izni verebilir ve bu sayede uygulamanız onların kiracısındaki kaynaklara erişim sağlayabilir.

### <a name="changing-the-application-to-support-multi-tenant"></a>Uygulamayı çok kiracılı modeli destekleyecek şekilde değiştirme

Çok kiracılı uygulama desteği çoğunlukla Azure AD onay çerçevesine bağımlıdır. Onay, başka bir kiracıdaki kullanıcının uygulamaya kullanıcının kiracısındaki kaynaklara erişme izni vermesini sağlayan mekanizmadır. Bu deneyim "kullanıcı onayı" olarak bilinir.

Web uygulamanız şu özellikleri de sunabilir:

- Yöneticiler için "şirketimi kaydet" özelliği. "Yönetici onayı" olarak bilinen bu deneyim bir yöneticinin kuruluşundaki *tüm kullanıcılar* adına onay verebilmesini sağlar. Yönetici onayını yalnızca Genel Yönetici rolüne ait olan bir hesapla kimlik doğrulamasından geçen kullanıcılar verebilir. Diğer kullanıcılar bir hatayla karşılaşacaktır.
- Kullanıcılar için kaydolma deneyimi. Kullanıcıya bir "kaydol" düğmesi sunulması ve bu düğmenin tarayıcıyı Azure AD OAuth2.0 `/authorize` uç noktasına veya OpenID Connect `/userinfo` uç noktasına yönlendirmesi beklenir. Bu uç noktaları, uygulamanın id_token verisini inceleyerek yeni kullanıcı hakkında bilgi edinmesini sağlar. Kaydolma aşaması kullanıcı onayı çerçeve bölümü bakış gösterilene benzer bir onay istemi sunulur.

Çok kiracılı erişim ve oturum açma/kaydolma deneyimlerini desteklemek için gerekli uygulama değişiklikleri hakkında daha fazla bilgi için bkz.:

- [Çok kiracılı uygulama desenini kullanarak istediğiniz bir Azure Active Directory (AD) kullanıcısı ile oturum açma](howto-convert-app-to-be-multi-tenant.md)
- [Çok kiracılı kod örnekleri](https://azure.microsoft.com/documentation/samples/?service=active-directory&term=multi-tenant) listesi.
- [Hızlı Başlangıç: Azure AD'de, oturum açma sayfası markalama şirket ekleme](../fundamentals/customize-branding.md)

## <a name="enabling-oauth-20-implicit-grant-for-single-page-applications"></a>Tek sayfalı uygulamalar için OAuth 2.0 örtük onayını etkinleştirme

Tek sayfalı uygulamalar (SPA) genellikle tarayıcıda çalışan ve iş mantığını gerçekleştirmek için uygulamanın web API'sini çağıran ve yoğun JavaScript kullanan bir ön uca sahiptir. Azure AD'de barındırılan SPA'lar için OAuth 2.0 Örtük Onayını kullanarak kullanıcı kimliğini Azure AD ile doğrulayabilir ve uygulamanın JavaScript istemcisinden arka uç web API'sine güvenli çağrı göndermek için kullanabileceğiniz bir belirteç alabilirsiniz.

Kullanıcı onay verdikten sonra yine bu kimlik doğrulaması protokolünü kullanarak istemci ile uygulamada yapılandırılmış olan diğer web API'si kaynakları arasında güvenli çağrı göndermek için de belirteç alabilirsiniz. Örtük yetkilendirme onayı hakkında daha fazla bilgi almak ve uygulama senaryonuz için doğru seçenek olup olmadığını öğrenmek için bkz. [Azure Active Directory'de OAuth2 örtük onay akışını anlama](v1-oauth2-implicit-grant-flow.md).

OAuth 2.0 Örtük Onay özelliği uygulamalar için varsayılan olarak devre dışı bırakılır. Uygulamanızda OAuth 2.0 Örtük Onay özelliğini etkinleştirmek istiyorsanız [uygulama bildirimindeki](reference-app-manifest.md) `oauth2AllowImplicitFlow` değerini ayarlayabilirsiniz.

### <a name="to-enable-oauth-20-implicit-grant"></a>OAuth 2.0 örtük onayını etkinleştirmek için

> [!NOTE]
> Uygulama bildirimini düzenleme hakkında ayrıntılı bilgi için öncelikle yukarıdaki [Kaynak uygulamasını web API'lerini kullanıma sunacak şekilde yapılandırma](#configuring-a-resource-application-to-expose-web-apis) bölümünü gözden geçirmeyi unutmayın.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Hesabınız birden fazla Azure AD kiracısına erişim sunuyorsa sağ üst köşeden hesabınıza tıklayın ve portal oturumunuzda kullanmak istediğiniz kiracıyı belirleyin.
3. Sol taraftaki gezinti bölmesinden **Azure Active Directory** hizmetine tıklayın, **Uygulama kayıtları**'na tıklayın ve ardından yapılandırmak istediğiniz uygulamayı bulun/tıklayın. Uygulamanın **Ayarlar** sayfasının açık olduğu uygulama ana kayıt sayfası görüntülenir.
4. Uygulama kayıt sayfasında **Bildirim**'e tıklayarak **Bildirimi düzenle** sayfasına geçin. Bildirimi portalda **Düzenleme** seçeneği sunana web tabanlı bir bildirim düzenleyici açılır. "oauth2AllowImplicitFlow" değerini bulun ve "true" olarak ayarlayın. Varsayılan olarak "false" değerine sahiptir.
   
   ```json
   "oauth2AllowImplicitFlow": true,
   ```
5. Güncelleştirilen bildirimi kaydedin. Kaydetme işleminin ardından web API'niz kullanıcıların kimliğini doğrulamak için OAuth 2.0 Örtük Onay özelliğini kullanacak şekilde yapılandırılmış olur.

## <a name="next-steps"></a>Sonraki adımlar

Azure AD v1.0 uç noktasını kullanan uygulamalar için diğer ilgili uygulama yönetimi hızlı başlangıçları hakkında bilgi edinin:
- [Azure AD'ye uygulama ekleme](quickstart-v1-integrate-apps-with-azure-ad.md)
- [Azure AD'den uygulama kaldırma](quickstart-v1-remove-azure-ad-app.md)

Kayıtlı uygulamayı temsil eden iki Azure AD nesnesi ve aralarındaki ilişki hakkında daha fazla bilgi edinmek için bkz. [Uygulama nesneleri ve hizmet sorumlusu nesneleri](app-objects-and-service-principals.md).

Azure Active Directory ile uygulama geliştirirken kullanmanız gereken markalama yönergeleri hakkında daha fazla bilgi edinmek için bkz. [Uygulamalar için markalama yönergeleri](howto-add-branding-in-azure-ad-apps.md).
