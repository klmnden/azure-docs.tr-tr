---
title: Azure'da barındırılan bir API'yi PowerApps ve Microsoft Flow için dışarı aktarma | Microsoft Docs
description: PowerApps ve Microsoft Flow için App Service'te barındırılan bir API'yi kullanıma sunmak nasıl genel bakış
services: app-service
author: ggailey777
manager: jeconnoc
ms.assetid: ''
ms.service: app-service
ms.devlang: multiple
ms.topic: conceptual
ms.date: 12/15/2017
ms.author: glenga
ms.reviewer: sunayv
ms.openlocfilehash: 9f4bbf91b09abeb917fd9f49482881e33bf788ec
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60499639"
---
# <a name="exporting-an-azure-hosted-api-to-powerapps-and-microsoft-flow"></a>Azure'da barındırılan bir API'yi PowerApps ve Microsoft Flow için dışarı aktarma

[PowerApps](https://powerapps.microsoft.com/guided-learning/learning-introducing-powerapps/) oluşturan ve platformlardaki verilerinize bağlanmak ve özel iş kolu uygulamalarını kullanarak yönelik bir hizmettir. [Microsoft Flow](https://flow.microsoft.com/guided-learning/learning-introducing-flow/) iş akışları ve sık kullandığınız uygulamalar ve hizmetler arasında iş süreçlerini otomatik hale getirmek kolay hale getirir. PowerApps ve Microsoft Flow, Office 365, Dynamics 365, Salesforce ve diğer veri kaynakları için çeşitli yerleşik bağlayıcılar ile gelir. Bazı durumlarda, uygulama ve akış oluşturucular ayrıca veri kaynakları ve kuruluş tarafından oluşturulan API'leri bağlanmak istediğiniz.

Benzer şekilde, kullanıcıların API'leri bir kuruluş içinde daha geniş kapsamda kullanıma sunmak istiyorsanız geliştiriciler kendi API kullanılabilir uygulama ve akış oluşturucuları yapabilirsiniz. Bu konu ile oluşturulmuş bir API dışa aktarmayı gösterir [Azure işlevleri](../azure-functions/functions-overview.md) veya [Azure App Service](../app-service/overview.md). Dışarı aktarılan API olur bir *özel bağlayıcı*, kullanılan Microsoft Flow ve PowerApps ile bir yerleşik Bağlayıcısı gibidir.

## <a name="create-and-export-an-api-definition"></a>Oluşturma ve bir API tanımını dışarı aktarma
Bir API dışarı aktarmadan önce bir Openapı tanımı kullanarak API'yi açıklamanız gerekir (eski adıyla bir [Swagger](https://swagger.io/) dosyası). Bu tanım, bir API’de hangi işlemlerin kullanılabildiğinin yanı sıra API için istek ve yanıt verilerinin nasıl yapılandırılması gerektiğiyle ilgili bilgileri içerir. PowerApps ve Microsoft Flow özel bağlayıcılar için herhangi bir Openapı 2.0 tanımı oluşturabilirsiniz. Azure işlevleri ve Azure App Service oluşturma, barındırma ve Openapı tanımlarıyla yönetmek için yerleşik desteği vardır. Daha fazla bilgi için [Azure App Service'te CORS ile RESTful API barındırma](../app-service/app-service-web-tutorial-rest-api.md).

> [!NOTE]
> Bir Openapı tanımı kullanmadan, PowerApps ve Microsoft Flow kullanıcı Arabirimi, özel bağlayıcılar da oluşturabilirsiniz. Daha fazla bilgi için [kaydetme ve kullanma (PowerApps) özel bir bağlayıcı](https://powerapps.microsoft.com/tutorials/register-custom-api/) ve [kaydetme ve kullanma (Microsoft Flow) özel bir bağlayıcı](https://flow.microsoft.com/documentation/register-custom-api/).

API tanımı dışarı aktarmak için aşağıdaki adımları izleyin:

1. İçinde [Azure portalında](https://portal.azure.com), Azure işlevleri veya başka bir App Service uygulaması gidin.

    Azure işlevleri'ni kullanarak, işlev uygulamanızı seçin, **Platform özellikleri**, ardından **API tanımı**.

    ![Azure işlevleri API tanımı](media/app-service-export-api-to-powerapps-and-flow/api-definition-function.png)

    Azure App Service'i kullanarak, seçin **API tanımı** ayarlar listesinde.

    ![App Service API tanımı](media/app-service-export-api-to-powerapps-and-flow/api-definition-app.png)

2. **Dışarı aktarmak için PowerApps + Microsoft Flow** düğmesi kullanılabilir (Aksi durumda, Openapı tanımı oluşturma). Dışarı aktarma işlemine başlamak için bu düğmeye tıklayın.

    ![PowerApps + Microsoft Flow için dışarı aktarma düğmesi](media/app-service-export-api-to-powerapps-and-flow/export-apps-flow.png)

3. Seçin **dışarı aktarma modu**:

    **Express** Azure portalındaki özel Bağlayıcıdan oluşturmanızı sağlar. Bu, PowerApps veya Microsoft Flow ile imzalanmış ve hedef ortamda bağlayıcılar oluşturma iznine sahip olmasını gerektirir. Bu iki gereksinimi karşılanabiliyorsa, önerilen yaklaşımdır. Bu modu kullanırken, izleyin [express dışa aktarmayı kullanma](#express) aşağıdaki yönergeleri.

    **El ile** , API tanımını dışarı aktarın, ardından PowerApps veya Microsoft Flow portalı kullanarak içeri aktarma olanak tanır. Azure kullanıcı ve bağlayıcılar oluşturma izni olan kullanıcı kişiler farklı olduğunda veya bağlayıcı başka bir Azure kiracısı içinde oluşturulması gerekiyorsa, önerilen yaklaşım budur. Bu modu kullanırken, izleyin [el ile dışarı aktarma](#manual) aşağıdaki yönergeleri.

    ![Dışarı aktarma modu](media/app-service-export-api-to-powerapps-and-flow/export-mode.png)

> [!NOTE]
> Özel bağlayıcıyı kullanan bir *kopyalama* PowerApps ve Microsoft Flow hemen uygulama ve API tanımı için değişiklik yapmadan, bürünüldüğünü bilmediği için API tanımı. Değişiklik yaparsanız, yeni sürüm için dışarı aktarma adımları yineleyin.

<a name="express"></a>
## <a name="use-express-export"></a>Express dışa aktarmayı kullanma

Dışa aktarma, tamamlanması **Express** modu, şu adımları izleyin:

1. Vermek istediğiniz PowerApps veya Microsoft Flow kiracıya açtığınızdan emin olun. 

2. Tabloda belirtilen ayarları kullanın.

    |Ayar|Açıklama|
    |--------|------------|
    |**Ortam**|Özel bağlayıcı için kaydedilmesi ortamı seçin. Daha fazla bilgi için [ortamlara genel bakış](https://powerapps.microsoft.com/tutorials/environments-overview/).|
    |**Özel API adı**|Hangi PowerApps ve Microsoft Flow, Oluşturucular, bağlayıcı listede görürsünüz, bir ad girin.|
    |**Güvenlik yapılandırmasını hazırla**|Zorunlu kılınırsa, kullanıcıların API'nizi erişim vermek için gereken güvenlik yapılandırma ayrıntılarını sağlayın. Bu örnek, bir API anahtarı gösterir. Daha fazla bilgi için [kimlik doğrulaması türünü belirtme](#auth) aşağıda.|
 
    ![Dışarı aktarma PowerApps ve Microsoft Flow için express](media/app-service-export-api-to-powerapps-and-flow/export-express.png)

3. **Tamam** düğmesine tıklayın. Özel bağlayıcı yerleşik ve belirttiğiniz ortama eklenir.

Kullanım örnekleri **Express** Azure işlevleri ile Modu'ndan [Powerapps'ten bir işlev çağırma](functions-powerapps-scenario.md) ve [Microsoft Flow bir işlevi çağırmayı](functions-flow-scenario.md).

<a name="manual"></a>
## <a name="use-manual-export"></a>El ile dışarı aktarma

Dışa aktarma, tamamlanması **el ile** modu, şu adımları izleyin:

1. Tıklayın **indirme** ve dosyayı kaydedin veya Kopyala düğmesine tıklayın ve URL'yi kaydedin. İçeri aktarma sırasında yükleme dosyasını veya URL'sini kullanır.
 
    ![PowerApps ve Microsoft Flow için el ile dışarı aktarma](media/app-service-export-api-to-powerapps-and-flow/export-manual.png)
 
2. API tanımınızı herhangi bir güvenlik tanımı içeriyorsa, bunlar #2. adımda çağrılır. Alma sırasında PowerApps ve Microsoft Flow bu algılar ve güvenlik bilgileri ister. Sonraki bölümde kullanmak için her tanım ilgili kimlik bilgilerini toplayın. Daha fazla bilgi için [kimlik doğrulaması türünü belirtme](#auth) aşağıda.

    ![Güvenlik el ile dışarı aktarma](media/app-service-export-api-to-powerapps-and-flow/export-manual-security.png)

    Bu örnek Openapı tanımına eklenmiş API anahtarı güvenlik tanımı gösterilmektedir.

API tanımı dışa aktardıysanız, Microsoft Flow ve PowerApps ile özel bir bağlayıcı oluşturmak için alın. Özel bağlayıcılar, böylece, yalnızca bir kez tanımını içeri aktarmak iki hizmetler arasında paylaşılır.

API tanımı, PowerApps ve Microsoft Flow içeri aktarmak için aşağıdaki adımları izleyin:

1. [powerapps.com](https://web.powerapps.com) veya [flow.microsoft.com](https://flow.microsoft.com) adresine gidin.

2. Sağ üst köşedeki dişli simgesine tıklayın ve ardından tıklayın **özel Bağlayıcılar**.

   ![Hizmetteki dişli simgesi](media/app-service-export-api-to-powerapps-and-flow/icon-gear.png)

3. Tıklayın **özel bağlayıcı oluşturma**, ardından **bir Openapı tanımını içeri aktarma**.

   ![Özel bağlayıcı oluşturma](media/app-service-export-api-to-powerapps-and-flow/flow-apps-create-connector.png)

4. Özel bağlayıcı için bir ad girin ve dışarı aktardığınız Openapı tanımına gidip tıklayın **devam**.

   ![Openapı tanımı karşıya yükleme](media/app-service-export-api-to-powerapps-and-flow/flow-apps-upload-definition.png)

4. Üzerinde **genel** sekmesinde, Openapı tanımından gelen bilgileri gözden geçirin.

5. Üzerinde **güvenlik** sekmesinde, istendiğinde, kimlik doğrulama ayrıntıları sağlamak için kimlik doğrulaması türü için uygun değerleri girin. **Devam**’a tıklayın.

    ![Güvenlik sekmesi](media/app-service-export-api-to-powerapps-and-flow/tab-security.png)

    Bu örnek, API anahtarı kimlik doğrulaması için gerekli alanları gösterir. Alanlar, kimlik doğrulaması türüne bağlı olarak farklılık gösterir.

6. Üzerinde **tanımları** sekmesinde, Openapı dosyanızda tanımlanan tüm işlemleri otomatik olarak doldurulur. Gerekli tüm işlemleriniz tanımlanmışsa, sonraki adıma gidebilirsiniz. Değilse, burada işlemleri ekleyin ve değiştirebilirsiniz.

    ![Tanımlar sekmesi](media/app-service-export-api-to-powerapps-and-flow/tab-definitions.png)

    Bu örnek adlı bir işlem olan `CalculateCosts`. Meta veriler gibi **açıklama**, tüm Openapı dosyasından gelir.

7. Tıklayın **Bağlayıcısı oluşturma** sayfanın üstünde.

Özel bağlayıcıyı PowerApps ve Microsoft Flow şimdi bağlanabilirsiniz. PowerApps ve Microsoft Flow portallarında bağlayıcılar oluşturma hakkında daha fazla bilgi için bkz. [(PowerApps) özel Bağlayıcınızı kaydedin](https://powerapps.microsoft.com/tutorials/register-custom-api/#register-your-custom-connector) ve [(Microsoft Flow) özel Bağlayıcınızı kaydedin](https://flow.microsoft.com/documentation/register-custom-api/#register-your-custom-connector).

<a name="auth"></a>
## <a name="specify-authentication-type"></a>Kimlik doğrulaması türünü belirtme

PowerApps ve Microsoft Flow özel bağlayıcılar için kimlik doğrulaması kimlik sağlayıcıları koleksiyonu destekler. API kimlik doğrulaması gerektiriyorsa, bunu olarak yakalanır sağlamak bir _güvenlik tanımı_ aşağıdaki örnekte olduğu gibi Openapı belgesi içinde:

```json
"securityDefinitions": {
    "AAD": {
    "type": "oauth2",
    "flow": "accessCode",
    "authorizationUrl": "https://login.windows.net/common/oauth2/authorize",
    "scopes": {}
    }
}
``` 
Dışarı aktarma sırasında PowerApps ve Microsoft Flow kullanıcıların kimliğini doğrulamak izin yapılandırma değerlerini sağlayın.

Bu bölümde desteklenen kimlik doğrulama türleri ele alınmaktadır **Express** modu: API anahtarı, Azure Active Directory ve genel OAuth 2.0. PowerApps ve Microsoft Flow da temel kimlik doğrulaması ve OAuth 2.0 Dropbox, Facebook ve SalesForce gibi hizmetler için destek.

### <a name="api-key"></a>API anahtarı
Bir API anahtarı kullanılırken, Bağlayıcınızı kullanıcıları bağlantı oluşturduklarında anahtarı sağlamanız istenir. Hangi anahtar gerekli anlamalarına yardımcı olmak için bir API anahtar adı belirtin. Önceki örnekte, adını kullanıyoruz `API Key (contact meganb@contoso.com)` kişiler API anahtarını hakkında bilgi almak nereye göndereceğimizi. Azure işlevleri için anahtar genellikle birkaç işlevleri içinde işlev uygulamasını kapsayan ana bilgisayar anahtarlarını biridir.

### <a name="azure-active-directory-azure-ad"></a>Azure Active Directory (Azure AD)
Azure AD kullanarak iki Azure AD uygulama kayıtları gerekir: API ve bir özel bağlayıcı için:

- Kayıt için API yapılandırmak için kullanın [App Service kimlik doğrulama/yetkilendirme](../app-service/configure-authentication-provider-aad.md) özelliği.

- Bağlayıcı kaydını yapılandırmak için adımları [bir Azure AD uygulaması ekleme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Kayıt API ve bir yanıt URL'si, erişim vermiş olması gerekir `https://msmanaged-na.consent.azure-apim.net/redirect`. 

Daha fazla bilgi için Azure AD kayıt örnekler için bkz. [PowerApps](https://powerapps.microsoft.com/tutorials/customapi-azure-resource-manager-tutorial/) ve [Microsoft Flow](https://flow.microsoft.com/documentation/customapi-azure-resource-manager-tutorial/). Bu örnekler, Azure Resource Manager API olarak kullanın. adımları izlerseniz, API'nizi değiştirin.

Aşağıdaki yapılandırma değerleri büyük/küçük harf gereklidir:
- **İstemci kimliği** -bağlayıcınızın Azure AD kaydı istemci kimliği
- **İstemci gizli anahtarı** -gizli bağlayıcınızın Azure AD kaydı
- **Oturum açma URL'si** -Azure AD temel URL'si. Azure'da, bu genellikle, `https://login.windows.net`.
- **Kiracı kimliği** -oturum açma için kullanılacak Kiracı kimliği. Bu "ortak" olmalıdır veya bağlayıcı oluşturulduğu kiracısının kimliği.
- **Kaynak URL** -Azure AD kaydı API'niz için kaynak URL'si

> [!IMPORTANT]
> Başka birinin API tanımı PowerApps ve Microsoft Flow el ile akışının bir parçası içeri aktaracak, bunları istemci Kimliğini ve istemci gizli dizisi ile sağlamanız gerekir *bağlayıcı kaydını*, kaynak URL'sini, API'nizi yanı sıra. Bu gizli dizileri güvenli bir şekilde yönetildiğinden emin olun. **API güvenlik kimlik bilgileri paylaşmaz.**

### <a name="generic-oauth-20"></a>Generic OAuth 2.0
Genel OAuth 2.0 kullanarak, herhangi bir OAuth 2.0 sağlayıcı ile tümleştirebilirsiniz. Bu, yerel olarak desteklenmeyen özel sağlayıcıları ile çalışmanıza olanak sağlar.

Aşağıdaki yapılandırma değerleri büyük/küçük harf gereklidir:
- **İstemci kimliği** -OAuth 2.0 istemci kimliği
- **İstemci gizli anahtarı** -OAuth 2.0 istemci gizli anahtarı
- **Yetkilendirme URL'si** -OAuth 2.0 yetkilendirme URL'si
- **Simge URL'si** -OAuth 2.0 belirteç URL'si
- **URL'yi Yenile** -OAuth 2.0 yenileme URL'si


