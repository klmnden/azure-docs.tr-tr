---
title: Bir Azure barındırılan API PowerApps ve Microsoft akış için dışa aktarma | Microsoft Docs
description: Uygulama hizmeti PowerApps ve Microsoft Flow barındırılan bir API kullanıma sunmak nasıl genel bakış
services: app-service
documentationcenter: ''
author: ggailey777
manager: cfowler
editor: ''
ms.assetid: ''
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 12/15/2017
ms.author: glenga
ms.reviewer: sunayv
ms.openlocfilehash: ef3fe5002a28c66478a10909a7e9556449cd9712
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35234776"
---
# <a name="exporting-an-azure-hosted-api-to-powerapps-and-microsoft-flow"></a>Bir Azure barındırılan API PowerApps ve Microsoft akış için dışa aktarma

[PowerApps](https://powerapps.microsoft.com/guided-learning/learning-introducing-powerapps/) oluşturma ve verilerinize bağlanın ve çalışma platformlarda özel iş uygulamaları kullanmak için bir hizmettir. [Microsoft Flow](https://flow.microsoft.com/guided-learning/learning-introducing-flow/) iş akışları ve sık kullanılan uygulamalar ve hizmetler arasındaki iş süreçlerini otomatikleştirmek daha kolay hale gelir. PowerApps ve Microsoft Flow Office 365, Dynamics 365, Salesforce ve daha fazlası gibi veri kaynaklarına yerleşik bağlayıcıları çeşitli ile gelir. Bazı durumlarda, uygulama ve akış oluşturucular ayrıca veri kaynakları ve kuruluşu tarafından oluşturulan API bağlanmak istediğiniz.

Benzer şekilde, kendi API daha geniş bir kuruluştaki kullanıma sunmak istediğiniz geliştiricilerin kendi API uygulaması ve akış oluşturucuları için sunabilirsiniz. Bu konu ile oluşturulan bir API dışarı aktarma gösterir [Azure işlevleri](../azure-functions/functions-overview.md) veya [Azure App Service](../app-service/app-service-web-overview.md). Dışarı aktarılan API hale bir *özel bağlayıcı*, kullanılan PowerApps ve Microsoft Flow yalnızca yerleşik bir bağlayıcısı gibi.

## <a name="create-and-export-an-api-definition"></a>Oluşturma ve bir API tanımı dışarı aktarma
Bir API dışarı aktarmadan önce OpenAPI tanımı kullanılarak API açıklamak gerekir (önceki adıyla bilinen bir [Swagger](http://swagger.io/) dosyası). Bu tanım, bir API’de hangi işlemlerin kullanılabildiğinin yanı sıra API için istek ve yanıt verilerinin nasıl yapılandırılması gerektiğiyle ilgili bilgileri içerir. PowerApps ve Microsoft Flow herhangi bir OpenAPI 2.0 tanımının özel bağlayıcılarının oluşturabilirsiniz. Azure işlevleri ve Azure uygulama hizmeti oluşturmak, barındırma ve OpenAPI tanımları yönetmek için yerleşik destek vardır. Daha fazla bilgi için bkz: [bir RESTful API'sini Azure App Service CORS ile ana bilgisayar](../app-service/app-service-web-tutorial-rest-api.md).

> [!NOTE]
> PowerApps ve akış Microsoft UI özel bağlayıcılar OpenAPI tanımı kullanmadan da oluşturabilirsiniz. Daha fazla bilgi için bkz: [kaydedin ve kullanım özel bir bağlayıcı (PowerApps)](https://powerapps.microsoft.com/tutorials/register-custom-api/) ve [kaydedin ve kullanım özel bir bağlayıcı (Microsoft Flow)](https://flow.microsoft.com/documentation/register-custom-api/).

API tanımı dışarı aktarmak için aşağıdaki adımları izleyin:

1. İçinde [Azure portal](https://portal.azure.com), Azure işlevleri veya başka bir uygulama hizmeti uygulaması gidin.

    Azure işlevleri kullanıyorsanız, işlevi uygulamanızı seçin, **Platform özellikleri**ve ardından **API tanımı**.

    ![Azure işlevleri API tanımı](media/app-service-export-api-to-powerapps-and-flow/api-definition-function.png)

    Azure uygulama hizmeti kullanıyorsanız, seçin **API tanımı** ayarları listesinden.

    ![Uygulama hizmeti API tanımı](media/app-service-export-api-to-powerapps-and-flow/api-definition-app.png)

2. **Vermek için PowerApps + Microsoft Flow** düğmesi kullanılabilir (değilse, önce bir OpenAPI tanımı oluşturmanız gerekir). Dışarı aktarma işlemini başlatmak için bu düğmeye tıklayın.

    ![PowerApps + Microsoft Flow dışarı aktarma düğmesi](media/app-service-export-api-to-powerapps-and-flow/export-apps-flow.png)

3. Seçin **verme modu**:

    **Express** özel bir bağlayıcı Azure portalındaki oluşturmanızı sağlar. PowerApps ya da Microsoft Flow imzalı ve hedef ortamda bağlayıcılar oluşturma iznine sahip olmasını gerektirir. Bu, bu iki gereksinimlerin karşılanması durumunda önerilen yaklaşımdır. Bu mod kullanıyorsanız, izleyin [express dışa](#express) aşağıdaki yönergeleri.

    **El ile** , API tanımı verme sonra PowerApps ya da Microsoft Flow portalları kullanarak içe aktarma sağlar. Bu önerilen Azure kullanıcı ve Bağlayıcıyı oluşturmak için izni olan bir kullanıcı kişiler farklı olduğunda veya bağlayıcı başka bir Azure kiracısı'nda oluşturulması gerekiyorsa yaklaşımdır. Bu mod kullanıyorsanız, izleyin [el ile dışarı](#manual) aşağıdaki yönergeleri.

    ![Verme modu](media/app-service-export-api-to-powerapps-and-flow/export-mode.png)

> [!NOTE]
> Özel Bağlayıcısı'nı kullanan bir *kopya* PowerApps ve Microsoft Flow hemen uygulama ve onun API tanımı değişiklik olursa anlayamazsınız şekilde API tanımı. Değişiklik yaparsanız, yeni sürümü için dışa aktarma adımları yineleyin.

<a name="express"></a>
## <a name="use-express-export"></a>Express verme kullanın

Dışa aktarma tamamlamak için **Express** modu, şu adımları izleyin:

1. Vermek istediğiniz PowerApps veya Microsoft Flow Kiracı kapattığınızdan emin olun. 

2. Tabloda belirtildiği gibi ayarları kullanın.

    |Ayar|Açıklama|
    |--------|------------|
    |**Ortam**|Özel bağlayıcı için kaydedilmesi ortamını seçin. Daha fazla bilgi için bkz: [ortamlarına genel bakış](https://powerapps.microsoft.com/tutorials/environments-overview/).|
    |**Özel API adı**|Hangi PowerApps ve Microsoft Flow oluşturucular kendi bağlayıcı listesinde görürsünüz, bir ad girin.|
    |**Güvenlik Yapılandırması hazırlama**|Gerekirse, kullanıcıların API'nizi erişim vermek için gereken güvenlik yapılandırma ayrıntıları sağlar. Bu örnek bir API anahtarı gösterir. Daha fazla bilgi için bkz: [kimlik doğrulama türünü belirtin](#auth) aşağıda.|
 
    ![PowerApps ve Microsoft Flow Express dışarı aktarma](media/app-service-export-api-to-powerapps-and-flow/export-express.png)

3. **Tamam**’a tıklayın. Özel Bağlayıcısı'nı şimdi yerleşik ve belirttiğiniz ortama eklenir.

Kullanım örnekleri için **Express** Azure işlevleri, moduyla bkz [PowerApps bir işlevi çağırmak](functions-powerapps-scenario.md) ve [Microsoft Flow bir işlevi çağırmak](functions-flow-scenario.md).

<a name="manual"></a>
## <a name="use-manual-export"></a>El ile dışarı aktarma kullanın

Dışa aktarma tamamlamak için **el ile** modu, şu adımları izleyin:

1. Tıklatın **karşıdan** ve dosyayı kaydedin veya Kopyala düğmesini tıklatın ve URL kaydedin. İçeri aktarma sırasında yükleme dosyası veya URL'yi kullanır.
 
    ![PowerApps ve Microsoft Flow el ile dışarı aktarma](media/app-service-export-api-to-powerapps-and-flow/export-manual.png)
 
2. API tanımı tüm güvenlik tanımlarını içeriyorsa, bunlar #2. adımda çağrılır. İçeri aktarma sırasında PowerApps ve Microsoft Flow bunlar algılar ve güvenlik bilgilerini ister. Sonraki bölümde her tanımını ilgili kimlik bilgilerini toplayın. Daha fazla bilgi için bkz: [kimlik doğrulama türünü belirtin](#auth) aşağıda.

    ![Güvenlik el ile dışarı aktarma](media/app-service-export-api-to-powerapps-and-flow/export-manual-security.png)

    Bu örnek OpenAPI tanımında eklenmiştir API anahtar güvenlik tanımını gösterir.

API tanımı dışa aktardıysanız, PowerApps ve Microsoft Flow özel bir bağlayıcı oluşturmak üzere alın. Özel bağlayıcıları yalnızca bir kez tanımını içeri aktarmak gereken şekilde iki hizmetleri arasında paylaşılır.

API tanımı PowerApps ve Microsoft Flow aktarmak için aşağıdaki adımları izleyin:

1. [powerapps.com](https://web.powerapps.com) veya [flow.microsoft.com](https://flow.microsoft.com) adresine gidin.

2. Sağ üst köşesinde, dişli simgesine tıklayın ve ardından tıklayın **özel Bağlayıcılar**.

   ![Hizmetteki dişli simgesi](media/app-service-export-api-to-powerapps-and-flow/icon-gear.png)

3. ' I tıklatın **özel bağlayıcı oluşturma**, ardından **OpenAPI tanımını içeri aktar**.

   ![Özel bağlayıcı oluşturma](media/app-service-export-api-to-powerapps-and-flow/flow-apps-create-connector.png)

4. Özel bağlayıcı için bir ad girin sonra dışarı aktardığınız OpenAPI tanımı gidin ve tıklatın **devam**.

   ![OpenAPI tanımı karşıya yükle](media/app-service-export-api-to-powerapps-and-flow/flow-apps-upload-definition.png)

4. Üzerinde **genel** sekmesinde, OpenAPI tanımından gelen bilgileri gözden geçirin.

5. Üzerinde **güvenlik** sekmesinde, istendiğinde, kimlik doğrulama ayrıntıları sağlamak için kimlik doğrulama türü için uygun değerleri girin. Tıklatın **devam**.

    ![Güvenlik sekmesi](media/app-service-export-api-to-powerapps-and-flow/tab-security.png)

    Bu örnek, API anahtar kimlik doğrulaması için gerekli alanlar gösterir. Alanları kimlik doğrulama türüne bağlı olarak farklılık gösterir.

6. Üzerinde **tanımları** sekmesinde OpenAPI dosyasında tanımlanan tüm işlemleri otomatik olarak doldurulur. Tüm gerekli işlemleri tanımlanmışsa, sonraki adıma gidebilirsiniz. Değilse, ekleme ve burada işlemleri değiştirme.

    ![Tanımları sekmesi](media/app-service-export-api-to-powerapps-and-flow/tab-definitions.png)

    Bu örnek adlı bir işlem olan `CalculateCosts`. Meta veriler gibi **açıklama**, tüm OpenAPI dosyasından gelir.

7. Tıklatın **Bağlayıcısı oluşturma** sayfanın üst kısmındaki.

Şimdi, PowerApps ve Microsoft Flow özel Connector bağlanabilir. Bağlayıcılar PowerApps ve Microsoft Flow portallarında oluşturma hakkında daha fazla bilgi için bkz: [özel Bağlayıcınızı (PowerApps) kaydetmek](https://powerapps.microsoft.com/tutorials/register-custom-api/#register-your-custom-connector) ve [özel Bağlayıcınızı (Microsoft Flow) kaydetmek](https://flow.microsoft.com/documentation/register-custom-api/#register-your-custom-connector).

<a name="auth"></a>
## <a name="specify-authentication-type"></a>Kimlik doğrulaması türünü belirtme

PowerApps ve Microsoft Flow özel bağlayıcılar için kimlik doğrulaması sağlamak kimlik sağlayıcıları koleksiyonu destekler. API'nizi kimlik doğrulaması gerektiriyorsa, olarak yakalanan emin olun bir _güvenlik tanımı_ OpenAPI belgenizin aşağıdaki örneğe benzer:

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
Dışa aktarma sırasında PowerApps ve Microsoft Flow kullanıcıların kimliklerini doğrulamak izin yapılandırma değerlerini sağlayın.

Bu bölümde desteklenen kimlik doğrulama türlerini kapsar **Express** modu: API anahtarı, Azure Active Directory ve genel OAuth 2.0. PowerApps ve Microsoft Flow da temel kimlik doğrulaması ve OAuth 2.0 Dropbox, Facebook ve SalesForce gibi belirli hizmetleri için destek.

### <a name="api-key"></a>API anahtarı
Bir API anahtarı kullanırken Bağlayıcınızı kullanıcılarının bir bağlantı oluşturduğunuzda anahtarı sağlamanız istenir. Hangi anahtar gerekli anlamalarına yardımcı olmak için bir API anahtar adı belirtin. Önceki örnekte ad kullanırız `API Key (contact meganb@contoso.com)` kişiler API anahtarı hakkında bilgi almak nereye göndereceğimizi. Azure işlevleri için anahtar genellikle işlev uygulaması içinde çeşitli işlevleri kapsayan ana bilgisayar anahtarları biridir.

### <a name="azure-active-directory-azure-ad"></a>Azure Active Directory (Azure AD)
Azure AD kullanırken iki Azure AD uygulama kayıtlar gerekir: API için ve bir özel Bağlayıcısı için:

- Kayıt için API yapılandırmak için kullanın [App Service kimlik doğrulama/yetkilendirme](../app-service/app-service-mobile-how-to-configure-active-directory-authentication.md) özelliği.

- Bağlayıcı kaydı yapılandırmak için adımları [Azure AD uygulaması ekleme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#adding-an-application). Kayıt, API ve yanıt URL'sini erişimi vermiş olması `https://msmanaged-na.consent.azure-apim.net/redirect`. 

Daha fazla bilgi için Azure AD kaydı örnekler için bkz: [PowerApps](https://powerapps.microsoft.com/tutorials/customapi-azure-resource-manager-tutorial/) ve [Microsoft Flow](https://flow.microsoft.com/documentation/customapi-azure-resource-manager-tutorial/). Bu örnekler, Azure Resource Manager API kullanmak; adımları izlerseniz, API değiştirin.

Aşağıdaki yapılandırma değerlerini gereklidir:
- **İstemci kimliği** -Azure AD kaydını bağlayıcının istemci kimliği
- **İstemci parolası** -Azure AD kaydını bağlayıcının istemci parolası
- **Oturum açma URL'si** -Azure AD için temel URL. Azure'da, bu genellikle olur `https://login.windows.net`.
- **Kiracı kimliği** -oturum açma için kullanılacak Kiracı kimliği. Bu "ortak" olmalıdır veya bağlayıcı oluşturulur Kiracı kimliği.
- **Kaynak URL** -API'nizi Azure AD kaydını kaynak URL'sini

> [!IMPORTANT]
> Başka birinin API tanımı PowerApps ve Microsoft Flow el ile akışının bir parçası içeri aktaracak, bunları istemci Kimliğini ve istemci parolası, sağlamanız gereken *bağlayıcı kaydı*, API'nizi kaynak URL'sini yanı sıra. Bu gizli anahtarları güvenli bir şekilde yönetildiğinden emin olun. **API güvenlik kimlik bilgileri paylaşmaz.**

### <a name="generic-oauth-20"></a>Generic OAuth 2.0
Genel OAuth 2.0 kullanırken, tüm OAuth 2.0 sağlayıcı ile tümleştirebilirsiniz. Bu, yerel olarak desteklenmeyen özel sağlayıcıları ile çalışmanıza olanak sağlar.

Aşağıdaki yapılandırma değerlerini gereklidir:
- **İstemci kimliği** -OAuth 2.0 istemci kimliği
- **İstemci parolası** -OAuth 2.0 istemci parolası
- **Yetkilendirme URL'si** -OAuth 2.0 yetkilendirme URL'si
- **Simge URL** -OAuth 2.0 belirteç URL'si
- **URL yenileme** -OAuth 2.0 yenileme URL'si


