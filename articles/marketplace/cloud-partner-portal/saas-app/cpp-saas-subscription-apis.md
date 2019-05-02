---
title: Azure API'leri ile SaaS satış | Azure Market
description: Bir SaaS teklifi Market API'leri aracılığıyla nasıl oluşturulacağını açıklar.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: reference
ms.date: 09/17/2018
ms.author: pabutler
ms.openlocfilehash: a76fb2989320c64ad85b0f41f17798e2d9c743e1
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64941954"
---
# <a name="saas-sell-through-azure---apis"></a>Azure - API'leri ile SaaS satış

Bu makalede, API'leri ile SaaS teklifi oluşturmak açıklanmaktadır. API'leri, seçili Azure üzerinden satış varsa, SaaS teklifinizle aboneliklere izin vermek için gereklidir.  Bu makalede, iki bölüme ayrılır:

-   Bir SaaS hizmeti ve Azure Market arasında hizmetten hizmete kimlik doğrulaması
-   API yöntemleri ve uç noktaları

Aşağıdaki API, SaaS hizmetinizin Azure ile tümleştirmenize yardımcı olmak için verilmiştir:

-   Çözümleme
-   Abone olun
-   Dönüştür
-   Abonelikten çık

Bu API'leri kullanıldığında ve yeni bir müşteri abonelik akışı aşağıdaki diyagramda gösterilmiştir:

![SaaS API'si akışı sunar.](./media/saas-offer-publish-api-flow.png)


## <a name="service-to-service-authentication-between-saas-service-and-azure-marketplace"></a>Azure Market SaaS hizmeti arasındaki hizmeti kimlik doğrulama hizmeti

Azure, SaaS hizmeti sunan, son kullanıcılar için kimlik doğrulaması tüm kısıtlamalar uygulamaz. Ancak, Azure Market API'leri ile iletişim kurulurken SaaS hizmetine söz konusu olduğunda, kimlik doğrulaması Azure Active Directory (Azure AD) uygulama bağlamında gerçekleştirilir.

Aşağıdaki bölümde, Azure AD uygulaması oluşturmayı açıklar.


### <a name="register-an-azure-ad-application"></a>Azure AD uygulaması kaydedin

Azure AD'nin özelliklerini kullanmak isteyen her uygulama önce bir Azure AD kiracısı olarak kaydedilmelidir. Azure AD'ye nerede, bir kullanıcının kimliği doğrulandıktan sonra yanıtların gönderileceği URL URL gibi uygulamanızın ayrıntılarını vererek kayıt işlemini içerir uygulama ve benzeri tanımlayan URI.

Azure portalını kullanarak yeni bir uygulamayı kaydetmek için aşağıdaki adımları gerçekleştirin:

1. [Azure Portal](https://portal.azure.com/)’da oturum açın.
2. Hesabınız birden fazla Azure AD kiracısına erişim sunuyorsa sağ üst köşeden hesabınıza tıklayın ve portal oturumunuzda kullanmak istediğiniz kiracıyı belirleyin.
3. Sol gezinti bölmesinden **Azure Active Directory** hizmeti ye **uygulama kayıtları**, tıklatıp **yeni uygulama kaydı**.

   ![SaaS AD uygulama kayıtları](./media/saas-offer-app-registration.png)

4. Uygulama Oluştur sayfasında girin\'s kayıt bilgileri:
   - **Ad**: Anlamlı uygulama adı girin
   - **Uygulama türü**: 
     - Bir cihaza yerel olarak yüklenen [istemci uygulamaları](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#client-application) için **Yerel**'i seçin. Bu ayar OAuth ortak [yerel istemcileri](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#native-client) için kullanılır.
     - Seçin **Web uygulaması / API** için [istemci uygulamaları](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#client-application) ve [kaynak/API uygulamaları](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#resource-server) güvenli bir sunucuya yüklenir. Bu ayar, OAuth gizli kullanılır [web istemcileri](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#web-client) ve genel [kullanıcı aracı tabanlı istemciler](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#user-agent-based-client).
     Aynı uygulama gerek bir istemciyi, gerekse kaynağı/API'yi sunabilir.
   - **Oturum açma URL'si**: Web uygulaması/API uygulamaları için uygulamanızın temel URL'si sağlayın. Örneğin, **http:\//localhost:31544** yerel makinenizde çalışan bir web uygulaması URL'si olabilir. Kullanıcılar, bir web istemci uygulamasına oturum açmak için bu URL'yi daha sonra kullanmanız gerekir.
   - **Yeniden yönlendirme URI'si**: Yerel uygulamaları için Azure AD'nin belirteç yanıtlarını döndürmek için kullanılan URI girin. Uygulamanıza özgü bir değer girin, örneğin **http:\//MyFirstAADApp**.

     ![SaaS AD uygulama kayıtları](./media/saas-offer-app-registration-2.png) web uygulamaları veya yerel uygulamalar için belirli örnekler için hızlı başlangıç kullanıma destekli bölmesinin Başlarken bölümünde kullanılabilir ayarlar [Azure AD Geliştirici Kılavuzu](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide).

5. Tamamladığınızda **Oluştur**’a tıklayın. Azure AD uygulamanız ve sizin için benzersiz bir uygulama kimliği atar\'re uygulamanıza geçen\'s ana kayıt sayfası. Web uygulaması ya da yerel uygulama olmasına bağlı olarak uygulamanıza ek özellikler eklemek için değişik seçenekler sunulur.

   **Not:** varsayılan olarak, yeni kaydettiğiniz uygulamayı, uygulamanız için oturum açmak için aynı kiracıda yalnızca kullanıcılar izin verecek şekilde yapılandırılır.

<a name="api-methods-and-endpoints"></a>API yöntemleri ve uç noktaları
-------------------------

Aşağıdaki bölümlerde, API yöntemleri ve bir SaaS teklifi için abonelikleri etkinleştirmek için kullanılabilir uç noktalar açıklanmaktadır.

### <a name="get-a-token-based-on-the-azure-ad-app"></a>Azure AD uygulama tabanlı bir belirteç Al

HTTP yöntemi

`GET`

*İstek URL'si*

**https://login.microsoftonline.com/*{Tenantıd}*/oauth2/belirteç**

*URI parametresi*

|  **Parametre adı**  | **Gerekli**  | **Açıklama**                               |
|  ------------------  | ------------- | --------------------------------------------- |
| Kiracı kimliği             | True          | Kayıtlı uygulama AAD Kiracı kimliği   |
|  |  |  |


*İstek üstbilgisi*

|  **Üst bilgi adı**  | **Gerekli** |  **Açıklama**                                   |
|  --------------   | ------------ |  ------------------------------------------------- |
|  Content-Type     | True         | İstekle ilişkili içerik türü. Varsayılan değer `application/x-www-form-urlencoded` şeklindedir.  |
|  |  |  |


*İstek gövdesi*

| **Özellik adı**   | **Gerekli** |  **Açıklama**                                                          |
| -----------------   | -----------  | ------------------------------------------------------------------------- |
|  Grant_type         | True         | İzin verme türü. Varsayılan değer `client_credentials` şeklindedir.                    |
|  Client_id          | True         |  Azure AD uygulama ile ilişkili istemci/uygulama tanımlayıcısı.                  |
|  client_secret      | True         |  Azure AD uygulama ile ilişkili parola.                               |
|  Kaynak           | True         |  Belirtecin istendiği hedef kaynak. Varsayılan değer `62d94f6c-d599-489b-a797-3e10e42fbe22` şeklindedir. |
|  |  |  |


*Yanıt*

|  **Ad**  | **Tür**       |  **Açıklama**    |
| ---------- | -------------  | ------------------- |
| 200 TAMAM    | TokenResponse  | İstek başarılı   |
|  |  |  |

*TokenResponse*

Örnek yanıt belirteci:

``` json
  {
      "token_type": "Bearer",
      "expires_in": "3600",
      "ext_expires_in": "0",
      "expires_on": "15251…",
      "not_before": "15251…",
      "resource": "62d94f6c-d599-489b-a797-3e10e42fbe22",
      "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6ImlCakwxUmNxemhpeTRmcHhJeGRacW9oTTJZayIsImtpZCI6ImlCakwxUmNxemhpeTRmcHhJeGRacW9oTTJZayJ9…"
  }               
```

### <a name="marketplace-api-endpoint-and-api-version"></a>Market API uç noktası ve API sürümü

Azure Market API'si için uç nokta `https://marketplaceapi.microsoft.com`.

Geçerli API sürümü `api-version=2017-04-15`.


### <a name="resolve-subscription"></a>Abonelik çözümleyin

GÖNDERİSİNİ eyleme çözmek bir kalıcı kaynak kimliği için bir Market belirteç çözmek kullanıcıların uç nokta sağlar  Kaynak Kimliği SAAS abonelik için benzersiz tanımlayıcısıdır. 

Bir kullanıcı, bir ISV Web sitesine yönlendirilir, sorgu parametrelerinde bir belirteç URL'sini içerir. Bu belirteci kullanmasına ve bu sorunu çözmek için bir istekte bulunmak için ISV bekleniyor. Yanıtın benzersiz SAAS abonelik kimliği, adı, Teklif kimliği ve kaynak planlama içerir. Bu belirteci yalnızca bir saat için geçerli değil.

*İstek*

**POST**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/resolve?api-version=2017-04-15**

|  **Parametre adı** |     **Açıklama**                                      |
|  ------------------ |     ---------------------------------------------------- |
|  API sürümü        |  Bu istek için kullanılacak işlem sürümü.   |
|  |  |


*Üst Bilgiler*

| **Üstbilgi anahtarı**     | **Gerekli** | **Açıklama**                                                                                                                                                                                                                  |
|--------------------|--------------|-----------------------------------------------------------|
| x-ms-requestid     | Hayır           | İstemci, tercihen bir GUID istek izleme için benzersiz bir dize değeri. Bu değer sağlanmazsa, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.  |
| x-ms-bağıntı kimliği | Hayır           | İstemci üzerinde işlem için benzersiz bir dize değeri. Bu istemci işlemi tüm olayları sunucu tarafında olaylarıyla ilişkilendirir. Bu değer sağlanmazsa, bir oluşturulur ve yanıt üst bilgilerinde sağlanan. |
| içerik türü       | Evet          | `application/json`                                        |
| Yetkilendirme      | Evet          | JSON web token (JWT) taşıyıcı belirteç.                    |
| x-ms-Pazar-token| Evet| Kullanıcı, Azure SaaS ISV Web sitesine yönlendirilir, URL'deki belirteci sorgu parametresi. **Not:** Bu belirteç, yalnızca 1 saat boyunca geçerlidir. Ayrıca, URL kod çözme tarayıcıdan belirteç değeri kullanmadan önce.|
|  |  |  |
  

*Yanıt gövdesi*

``` json
{
    "id": "",
    "subscriptionName": "",
    "offerId": "",
    "planId": "",
}
```

| **Parametre adı** | **Veri türü** | **Açıklama**                       |
|--------------------|---------------|---------------------------------------|
| id                 | String        | SaaS aboneliğin kimliği.          |
| subscriptionName| String| SaaS hizmetine abone sırasında azure'da kullanıcı tarafından ayarlanan SaaS abonelik adı.|
| OfferId            | String        | Teklif kullanıcıya abone kimliği. |
| Planıd             | String        | Bir kullanıcı abone kimliği planlayın.  |
|  |  |  |


*Yanıt kodları*

| **HTTP durum kodu** | **Hata kodu**     | **Açıklama**                                                                         |
|----------------------|--------------------| --------------------------------------------------------------------------------------- |
| 200                  | `OK`                 | Belirteç başarıyla çözümlendi.                                                            |
| 400                  | `BadRequest`         | Ya da, üst bilgiler eksik veya geçersiz bir API sürümü belirtilen gereklidir. Belirteci hatalı biçimlendirilmiş ya da süresi dolmuş ya da belirteç olduğu için çözümlenemedi (belirteç yalnızca bir kez oluşturulan 1 saat boyunca geçerlidir). |
| 403                  | `Forbidden`          | Çağıranın bu işlemi gerçekleştirmek için yetkili değil.                                 |
| 429                  | `RequestThrottleId`  | Hizmetidir istekleri işlemekle meşgul, daha sonra yeniden deneyin.                                |
| 503                  | `ServiceUnavailable` | Hizmet olduğu aşağı geçici olarak, daha sonra yeniden deneyin.                                        |
|  |  |  |


*Yanıt Üstbilgileri*

| **Üstbilgi anahtarı**     | **Gerekli** | **Açıklama**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Evet          | İstek istemciden alınan kimliği.                                                                   |
| x-ms-bağıntı kimliği | Evet          | Bağıntı kimliği istemci tarafından aksi durumda bu değer iletilmezse server bağıntı kimliğidir.                   |
| x-ms-etkinlik kimliği    | Evet          | Hizmet isteği izlemek için benzersiz bir dize değeri. Bu, tüm Mutabakatları için kullanılır. |
| Yeniden deneme sonrasında        | Hayır           | Bu değer yalnızca bir 429 yanıtı için ayarlanır.                                                                   |
|  |  |  |


### <a name="subscribe"></a>Abone olun

Abone uç noktası bir SaaS hizmetine belirli bir plan için bir abonelik başlatmak ve faturalandırma ticaret sisteminde etkinleştirmesine olanak sağlar.

**PUT**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/*{Subscriptionıd}*?api-version=2017-04-15**

| **Parametre adı**  | **Açıklama**                                       |
|---------------------|-------------------------------------------------------|
| subscriptionId      | Belirteç çözmek API aracılığıyla çözdükten sonra elde edilen saas abonelik benzersiz kimliği.                              |
| API sürümü         | Bu istek için kullanılacak işlem sürümü. |
|  |  |

*Üst Bilgiler*

|  **Üstbilgi anahtarı**        | **Gerekli** |  **Açıklama**                                                  |
| ------------------     | ------------ | --------------------------------------------------------------------------------------- |
| x-ms-requestid         |   Hayır         | İstemci, tercihen bir GUID istek izleme için benzersiz bir dize değeri. Bu sağlanmazsa, bir oluşturulur ve yanıt üst bilgilerinde sağlanan. |
| x-ms-bağıntı kimliği     |   Hayır         | İstemci üzerinde işlem için benzersiz bir dize değeri. Bu değer, tüm istemci işlemi olayları sunucu tarafında olaylarla ilişkilendirmek için kullanılabilir. Bu sağlanmazsa, bir oluşturulur ve yanıt üst bilgilerinde sağlanan. |
| IF-Match/If-None-Match |   Hayır         |   Güçlü Doğrulayıcı ETag değeri.                                                          |
| içerik türü           |   Evet        |    `application/json`                                                                   |
|  Yetkilendirme         |   Evet        |    JSON web token (JWT) taşıyıcı belirteç.                                               |
| x-ms-Pazar-oturum-modu| Hayır | Bir SaaS teklife abone sırasında prova modunu etkinleştirmek için bayrak. Ayarlanırsa, abonelik ücret uygulanmayan durumunda. ISV senaryolarını test etmek için kullanışlıdır. Lütfen kümesine **'prova'**|
|  |  |  |

*Gövde*

``` json
{
    "lanId": "",
}
```

| **Öğe adı** | **Veri türü** | **Açıklama**                      |
|------------------|---------------|--------------------------------------|
| Planıd           | (Gerekli) Dize        | SaaS hizmeti kullanıcı plan kimliği için abone oluyor.  |
|  |  |  |

*Yanıt kodları*

| **HTTP durum kodu** | **Hata kodu**     | **Açıklama**                                                           |
|----------------------|--------------------|---------------------------------------------------------------------------|
| 202                  | `Accepted`           | SaaS abonelik etkinleştirme için belirli bir plan aldı.                   |
| 400                  | `BadRequest`         | Ya da üst bilgileri eksik olan veya JSON gövdesi yanlış biçimlendirilmiş gereklidir. |
| 403                  | `Forbidden`          | Çağıranın bu işlemi gerçekleştirmek için yetkili değil.                   |
| 404                  | `NotFound`           | Aboneliğiniz ile sağlanan kimliği bulunamadı                                  |
| 409                  | `Conflict`           | Abonelik üzerinde başka bir işlem sürüyor.                     |
| 429                  | `RequestThrottleId`  | Hizmetidir istekleri işlemekle meşgul, daha sonra yeniden deneyin.                  |
| 503                  | `ServiceUnavailable` | Hizmet olduğu aşağı geçici olarak, daha sonra yeniden deneyin.                          |
|  |  |  |

'İşlemi-konum' üst bilgisi, istek işlemin durumu için 202 yanıt, iletişim kurun. Kimlik doğrulaması diğer Market API'leri ile aynıdır.

*Yanıt Üstbilgileri*

| **Üstbilgi anahtarı**     | **Gerekli** | **Açıklama**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Evet          | İstek istemciden alınan kimliği.                                                                   |
| x-ms-bağıntı kimliği | Evet          | Bağıntı kimliği istemci tarafından aksi durumda bu değer iletilmezse server bağıntı kimliğidir.                   |
| x-ms-etkinlik kimliği    | Evet          | Hizmet isteği izlemek için benzersiz bir dize değeri. Bu değer, tüm Mutabakatları için kullanılır. |
| Yeniden deneme sonrasında        | Evet          | Zaman aralığı ile hangi istemci durumu kontrol edebilirsiniz.                                                       |
| İşlem konumu | Evet          | İşlem durumunu almak için bir kaynak bağlayın.                                                        |
|  |  |  |

### <a name="change-plan-endpoint"></a>Değişiklik planı uç noktası

Değişiklik uç noktası şu anda abone planlarına dönüştürmek için yeni bir plan izin verir.

**DÜZELTME EKİ**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/*{Subscriptionıd}*?api-version=2017-04-15**

| **Parametre adı**  | **Açıklama**                                       |
|---------------------|-------------------------------------------------------|
| subscriptionId      | Abonelik kimliği, SaaS.                              |
| API sürümü         | Bu istek için kullanılacak işlem sürümü. |
|  |  |

*Üst Bilgiler*

| **Üstbilgi anahtarı**          | **Gerekli** | **Açıklama**                                                                                                                                                                                                                  |
|-------------------------|--------------|---------------------------------------------------------------------------------------------------------------------|
| x-ms-requestid          | Hayır           | İstemciden istek izleme için benzersiz bir dize değeri. Bir GUID önerilir. Bu sağlanmazsa, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.   |
| x-ms-bağıntı kimliği      | Hayır           | İstemci üzerinde işlem için benzersiz bir dize değeri. Bu değer, tüm istemci işlemi olayları sunucu tarafında olaylarla ilişkilendirmek için kullanılabilir. Bu sağlanmazsa, bir oluşturulur ve yanıt üst bilgilerinde sağlanan. |
| IF-Match /If-None-Match | Hayır           | Güçlü Doğrulayıcı ETag değeri.                              |
| içerik türü            | Evet          | `application/json`                                        |
| Yetkilendirme           | Evet          | JSON web token (JWT) taşıyıcı belirteç.                    |
|  |  |  |

*Gövde*

```json
{
    "planId": ""
}
```

|  **Öğe adı** |  **Veri türü**  | **Açıklama**                              |
|  ---------------- | -------------   | --------------------------------------       |
|  Planıd           |  (Gerekli) Dize         | SaaS hizmeti kullanıcı plan kimliği için abone oluyor.          |
|  |  |  |

*Yanıt kodları*

| **HTTP durum kodu** | **Hata kodu**     | **Açıklama**                                                           |
|----------------------|--------------------|---------------------------------------------------------------------------|
| 202                  | `Accepted`           | SaaS abonelik etkinleştirme için belirli bir plan aldı.                   |
| 400                  | `BadRequest`         | Ya da üst bilgileri eksik olan veya JSON gövdesi yanlış biçimlendirilmiş gereklidir. |
| 403                  | `Forbidden`          | Çağıranın bu işlemi gerçekleştirmek için yetkili değil.                   |
| 404                  | `NotFound`           | Aboneliğiniz ile sağlanan kimliği bulunamadı                                  |
| 409                  | `Conflict`           | Abonelik üzerinde başka bir işlem sürüyor.                     |
| 429                  | `RequestThrottleId`  | Hizmetidir istekleri işlemekle meşgul, daha sonra yeniden deneyin.                  |
| 503                  | `ServiceUnavailable` | Hizmet olduğu aşağı geçici olarak, daha sonra yeniden deneyin.                          |
|  |  |  |

*Yanıt Üstbilgileri*

| **Üstbilgi anahtarı**     | **Gerekli** | **Açıklama**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Evet          | İstek istemciden alınan kimliği.                                                                   |
| x-ms-bağıntı kimliği | Evet          | Bağıntı kimliği istemci tarafından aksi durumda bu değer iletilmezse server bağıntı kimliğidir.                   |
| x-ms-etkinlik kimliği    | Evet          | Hizmet isteği izlemek için benzersiz bir dize değeri. Bu değer, tüm Mutabakatları için kullanılır. |
| Yeniden deneme sonrasında        | Evet          | Zaman aralığı ile hangi istemci durumu kontrol edebilirsiniz.                                                       |
| İşlem konumu | Evet          | İşlem durumunu almak için bir kaynak bağlayın.                                                        |
|  |  |  |

### <a name="delete-subscription"></a>Aboneliği silin

Abone uç noktası silme eylemini belirtilen kimliğe sahip bir aboneliği silme açmasına olanak sağlar.

*İstek*

**DELETE**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/*{Subscriptionıd}*?api-version=2017-04-15**

| **Parametre adı**  | **Açıklama**                                       |
|---------------------|-------------------------------------------------------|
| subscriptionId      | Abonelik kimliği, SaaS.                              |
| API sürümü         | Bu istek için kullanılacak işlem sürümü. |
|  |  |

*Üst Bilgiler*

| **Üstbilgi anahtarı**     | **Gerekli** | **Açıklama**                                                                                                                                                                                                                  |
|--------------------|--------------| ----------------------------------------------------------|
| x-ms-requestid     | Hayır           | İstemciden istek izleme için benzersiz bir dize değeri. Bir GUID önerilir. Bu değer sağlanmazsa, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.                                                           |
| x-ms-bağıntı kimliği | Hayır           | İstemci üzerinde işlem için benzersiz bir dize değeri. Bu değer, tüm istemci işlemi olayları sunucu tarafında olaylarla ilişkilendirmek için kullanılabilir. Bu sağlanmazsa, bir oluşturulur ve yanıt üst bilgilerinde sağlanan. |
| Yetkilendirme      | Evet          | JSON web token (JWT) taşıyıcı belirteç.                    |
|  |  |  |

*Yanıt kodları*

| **HTTP durum kodu** | **Hata kodu**     | **Açıklama**                                                           |
|----------------------|--------------------|---------------------------------------------------------------------------|
| 202                  | `Accepted`           | SaaS abonelik etkinleştirme için belirli bir plan aldı.                   |
| 400                  | `BadRequest`         | Ya da üst bilgileri eksik olan veya JSON gövdesi yanlış biçimlendirilmiş gereklidir. |
| 403                  | `Forbidden`          | Çağıranın bu işlemi gerçekleştirmek için yetkili değil.                   |
| 404                  | `NotFound`           | Aboneliğiniz ile sağlanan kimliği bulunamadı                                  |
| 429                  | `RequestThrottleId`  | Hizmet istekleri işlemekle meşgul, lütfen daha sonra yeniden deneyin.                  |
| 503                  | `ServiceUnavailable` | Hizmet geçici olarak kullanılamıyor. Lütfen daha sonra yeniden deneyin.                          |
|  |  |  |

'İşlemi-konum' üst bilgisi, istek işlemin durumu için 202 yanıt, iletişim kurun. Kimlik doğrulaması diğer Market API'leri ile aynıdır.

*Yanıt Üstbilgileri*

| **Üstbilgi anahtarı**     | **Gerekli** | **Açıklama**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Evet          | İstek istemciden alınan kimliği.                                                                   |
| x-ms-bağıntı kimliği | Evet          | Bağıntı kimliği istemci tarafından aksi takdirde bu aktarılırsa server bağıntı kimliğidir.                   |
| x-ms-etkinlik kimliği    | Evet          | Hizmet isteği izlemek için benzersiz bir dize değeri. Bu, tüm Mutabakatları için kullanılır. |
| Yeniden deneme sonrasında        | Evet          | Zaman aralığı ile hangi istemci durumu kontrol edebilirsiniz.                                                       |
| İşlem konumu | Evet          | İşlem durumunu almak için bir kaynak bağlayın.                                                        |
|   |  |  |

### <a name="get-operation-status"></a>İşlem durumunu Al

Bu uç noktayı (abonelik/Aboneliği Kaldır/Değiştir planı) bir tetiklenen zaman uyumsuz işlemin durumunu izlemek kullanıcı sağlar.

*İstek*

**GET**

**https://marketplaceapi.microsoft.com/api/saas/operations/*{Operationıd}*?api-version=2017-04-15**

| **Parametre adı**  | **Açıklama**                                       |
|---------------------|-------------------------------------------------------|
| operationId         | Tetiklenen işlem benzersiz kimliği.                |
| API sürümü         | Bu istek için kullanılacak işlem sürümü. |
|  |  |

*Üst Bilgiler*

| **Üstbilgi anahtarı**     | **Gerekli** | **Açıklama**                                                                                                                                                                                                                  |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Hayır           | İstemciden istek izleme için benzersiz bir dize değeri. Bir GUID önerilir. Bu değer sağlanmazsa, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.   |
| x-ms-bağıntı kimliği | Hayır           | İstemci üzerinde işlem için benzersiz bir dize değeri. Bu değer, tüm istemci işlemi olayları sunucu tarafında olaylarla ilişkilendirmek için kullanılabilir. Bu değer sağlanmazsa, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.  |
| Yetkilendirme      | Evet          | JSON web token (JWT) taşıyıcı belirteç.                    |
|  |  |  | 

*Yanıt gövdesi*

```json
{
    "id": "",
    "status": "",
    "resourceLocation": "",
    "created": "",
    "lastModified": ""
}
```

| **Parametre adı** | **Veri türü** | **Açıklama**                                                                                                                                               |
|--------------------|---------------|-------------------------------------------------------------------------------------------|
| id                 | String        | İşlemin kimliği.                                                                      |
| durum             | Sabit listesi          | İşlem durumu aşağıdakilerden biri: `In Progress`, `Succeeded`, veya `Failed`.          |
| resourceLocation   | String        | Oluşturulan veya değiştirilen aboneliğinize bağlayın. Bu, istemci güncelleştirilen durumu post işlemi almak için yardımcı olur. Bu değer için ayarlanmadı `Unsubscribe` operations. |
| oluşturuldu            | DateTime      | İşlem oluşturma zamanı UTC diliminde saat.                                                           |
| Son değiştirme       | DateTime      | Son güncelleştirme işlemi UTC diliminde saat.                                                      |
|  |  |  |

*Yanıt kodları*

| **HTTP durum kodu** | **Hata kodu**     | **Açıklama**                                                              |
|----------------------|--------------------|------------------------------------------------------------------------------|
| 200                  | `OK`                 | Get isteği başarıyla çözümlendi ve yanıt gövdesi içerir.    |
| 400                  | `BadRequest`         | Ya da, üst bilgiler eksik veya geçersiz bir API sürümü belirtildi gereklidir. |
| 403                  | `Forbidden`          | Çağıranın bu işlemi gerçekleştirmek için yetkili değil.                      |
| 404                  | `NotFound`           | Aboneliğiniz ile sağlanan kimliği bulunamadı                                     |
| 429                  | `RequestThrottleId`  | Hizmetidir istekleri işlemekle meşgul, daha sonra yeniden deneyin.                     |
| 503                  | `ServiceUnavailable` | Hizmet olduğu aşağı geçici olarak, daha sonra yeniden deneyin.                             |
|  |  |  |

*Yanıt Üstbilgileri*

| **Üstbilgi anahtarı**     | **Gerekli** | **Açıklama**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Evet          | İstek istemciden alınan kimliği.                                                                   |
| x-ms-bağıntı kimliği | Evet          | Bağıntı kimliği istemci tarafından aksi takdirde bu aktarılırsa server bağıntı kimliğidir.                   |
| x-ms-etkinlik kimliği    | Evet          | Hizmet isteği izlemek için benzersiz bir dize değeri. Bu, tüm Mutabakatları için kullanılır. |
| Yeniden deneme sonrasında        | Evet          | Zaman aralığı ile hangi istemci durumu kontrol edebilirsiniz.                                                       |
|  |  |  |

### <a name="get-subscription"></a>Aboneliği edinin

Get eylemini abone uç noktası ile belirtilen kaynak tanımlayıcı bir abonelik almak olanak tanır.

*İstek*

**GET**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/*{Subscriptionıd}*?api-version=2017-04-15**

| **Parametre adı**  | **Açıklama**                                       |
|---------------------|-------------------------------------------------------|
| subscriptionId      | Abonelik kimliği, SaaS.                              |
| API sürümü         | Bu istek için kullanılacak işlem sürümü. |
|  |  |

*Üst Bilgiler*

| **Üstbilgi anahtarı**     | **Gerekli** | **Açıklama**                                                                                           |
|--------------------|--------------|-----------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Hayır           | İstemci, tercihen bir GUID istek izleme için benzersiz bir dize değeri. Bu değer sağlanmazsa, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.                                                           |
| x-ms-bağıntı kimliği | Hayır           | İstemci üzerinde işlem için benzersiz bir dize değeri. Bu değer, tüm istemci işlemi olayları sunucu tarafında olaylarla ilişkilendirmek için kullanılabilir. Bu değer sağlanmazsa, bir oluşturulur ve yanıt üst bilgilerinde sağlanan. |
| Yetkilendirme      | Evet          | JSON web token (JWT) taşıyıcı belirteç.                                                                    |
|  |  |  |

*Yanıt gövdesi*

```json
{
    "id": "",
    "saasSubscriptionName": "",
    "offerId": "",
    "planId": "",
    "saasSubscriptionStatus": "",
    "created": "",
    "lastModified": ""
}
```

| **Parametre adı**     | **Veri türü** | **Açıklama**                               |
|------------------------|---------------|-----------------------------------------------|
| id                     | String        | Azure abonelik kaynak kimliği, SaaS.    |
| offerId                | String        | Teklif kullanıcıya abone kimliği.         |
| Planıd                 | String        | Bir kullanıcı abone kimliği planlayın.          |
| saasSubscriptionName   | String        | SaaS abonelik adı.                |
| saasSubscriptionStatus | Sabit listesi          | İşlem durumu.  Aşağıdakilerden biri:  <br/> - `Subscribed`: Abonelik etkin değil.  <br/> - `Pending`: Kullanıcı kaynak oluşturabilirsiniz, ancak ISV tarafından etkinleştirilmez.   <br/> - `Unsubscribed`: Kullanıcı iptal etti.   <br/> - `Suspended`: Kullanıcı aboneliği askıya aldı.   <br/> - `Deactivated`:  Azure abonelik askıya alındı.  |
| oluşturuldu                | DateTime      | Abonelik oluşturma UTC zaman damgası değeri. |
| Son değiştirme           | DateTime      | Abonelik UTC zaman damgası değeri değiştirildi. |
|  |  |  |

*Yanıt kodları*

| **HTTP durum kodu** | **Hata kodu**     | **Açıklama**                                                              |
|----------------------|--------------------|------------------------------------------------------------------------------|
| 200                  | `OK`                 | Get isteği başarıyla çözümlendi ve yanıt gövdesi içerir.    |
| 400                  | `BadRequest`         | Ya da, üst bilgiler eksik veya geçersiz bir API sürümü belirtildi gereklidir. |
| 403                  | `Forbidden`          | Çağıranın bu işlemi gerçekleştirmek için yetkili değil.                      |
| 404                  | `NotFound`           | Aboneliğiniz ile sağlanan kimliği bulunamadı                                     |
| 429                  | `RequestThrottleId`  | Hizmetidir istekleri işlemekle meşgul, daha sonra yeniden deneyin.                     |
| 503                  | `ServiceUnavailable` | Hizmet olduğu aşağı geçici olarak, daha sonra yeniden deneyin.                             |
|  |  |  |

*Yanıt Üstbilgileri*

| **Üstbilgi anahtarı**     | **Gerekli** | **Açıklama**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Evet          | İstek istemciden alınan kimliği.                                                                   |
| x-ms-bağıntı kimliği | Evet          | Bağıntı kimliği istemci tarafından aksi takdirde bu aktarılırsa server bağıntı kimliğidir.                   |
| x-ms-etkinlik kimliği    | Evet          | Hizmet isteği izlemek için benzersiz bir dize değeri. Bu, tüm Mutabakatları için kullanılır. |
| Yeniden deneme sonrasında        | Hayır           | Zaman aralığı ile hangi istemci durumu kontrol edebilirsiniz.                                                       |
| eTag               | Evet          | İşlem durumunu almak için bir kaynak bağlayın.                                                        |
|  |  |  |

### <a name="get-subscriptions"></a>Abonelikleri Al

Abonelik uç noktasında alma işlemi ISV tüm abonelikler için tüm teklifleri almak açmasına olanak sağlar.

*İstek*

**GET**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions?api-version=2017-04-15**

| **Parametre adı**  | **Açıklama**                                       |
|---------------------|-------------------------------------------------------|
| API sürümü         | Bu istek için kullanılacak işlem sürümü. |
|  |  |

*Üst Bilgiler*

| **Üstbilgi anahtarı**     | **Gerekli** | **Açıklama**                                           |
|--------------------|--------------|-----------------------------------------------------------|
| x-ms-requestid     | Hayır           | İstemciden istek izleme için benzersiz bir dize değeri. Bir GUID önerilir. Bu değer sağlanmazsa, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.             |
| x-ms-bağıntı kimliği | Hayır           | İstemci üzerinde işlem için benzersiz bir dize değeri. Bu değer, tüm istemci işlemi olayları sunucu tarafında olaylarla ilişkilendirmek için kullanılabilir. Bu değer sağlanmazsa, bir oluşturulur ve yanıt üst bilgilerinde sağlanan. |
| Yetkilendirme      | Evet          | JSON web token (JWT) taşıyıcı belirteç.                    |
|  |  |  |

*Yanıt gövdesi*

```json
{
    "id": "",
    "saasSubscriptionName": "",
    "offerId": "",
    "planId": "",
    "saasSubscriptionStatus": "",
    "created": "",
    "lastModified": ""
}
```

| **Parametre adı**     | **Veri türü** | **Açıklama**                               |
|------------------------|---------------|-----------------------------------------------|
| id                     | String        | Azure abonelik kaynak kimliği, SaaS.    |
| offerId                | String        | Teklif kullanıcıya abone kimliği.         |
| Planıd                 | String        | Bir kullanıcı abone kimliği planlayın.          |
| saasSubscriptionName   | String        | SaaS abonelik adı.                |
| saasSubscriptionStatus | Sabit listesi          | İşlem durumu.  Aşağıdakilerden biri:  <br/> - `Subscribed`: Abonelik etkin değil.  <br/> - `Pending`: Kullanıcı kaynak oluşturabilirsiniz, ancak ISV tarafından etkinleştirilmez.   <br/> - `Unsubscribed`: Kullanıcı iptal etti.   <br/> - `Suspended`: Kullanıcı aboneliği askıya aldı.   <br/> - `Deactivated`:  Azure abonelik askıya alındı.  |
| oluşturuldu                | DateTime      | Abonelik oluşturma UTC zaman damgası değeri. |
| Son değiştirme           | DateTime      | Abonelik UTC zaman damgası değeri değiştirildi. |
|  |  |  |

*Yanıt kodları*

| **HTTP durum kodu** | **Hata kodu**     | **Açıklama**                                                              |
|----------------------|--------------------|------------------------------------------------------------------------------|
| 200                  | `OK`                 | Get isteği başarıyla çözümlendi ve yanıt gövdesi içerir.    |
| 400                  | `BadRequest`         | Ya da, üst bilgiler eksik veya geçersiz bir API sürümü belirtildi gereklidir. |
| 403                  | `Forbidden`          | Çağıranın bu işlemi gerçekleştirmek için yetkili değil.                      |
| 404                  | `NotFound`           | Aboneliğiniz ile sağlanan kimliği bulunamadı                                     |
| 429                  | `RequestThrottleId`  | Hizmet istekleri işlemekle meşgul, lütfen daha sonra yeniden deneyin.                     |
| 503                  | `ServiceUnavailable` | Hizmet geçici olarak kullanılamıyor. Lütfen daha sonra yeniden deneyin.                             |
|  |  |  |

*Yanıt Üstbilgileri*

| **Üstbilgi anahtarı**     | **Gerekli** | **Açıklama**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Evet          | İstek istemciden alınan kimliği.                                                                   |
| x-ms-bağıntı kimliği | Evet          | Bağıntı kimliği istemci tarafından aksi takdirde bu aktarılırsa server bağıntı kimliğidir.                   |
| x-ms-etkinlik kimliği    | Evet          | Hizmet isteği izlemek için benzersiz bir dize değeri. Bu, tüm Mutabakatları için kullanılır. |
| Yeniden deneme sonrasında        | Hayır           | Zaman aralığı ile hangi istemci durumu kontrol edebilirsiniz.                                                       |
|  |  |  |

### <a name="saas-webhook"></a>SaaS Web kancası

Bir SaaS Web kancası değişiklikleri SaaS hizmeti için proaktif olarak bildirmek için kullanılır. Bu GÖNDERİ API kimliği doğrulanmamış olması beklenir ve Microsoft hizmeti tarafından çağrılır. SaaS hizmeti doğrulamak ve Web kancası bildirim eylemi gerçekleştirmeden önce onları yetkilendirmek için API işlemleri çağırmak için bekleniyor. 

*Gövde*

``` json
  { 
    "id": "be750acb-00aa-4a02-86bc-476cbe66d7fa",
    "activityId": "be750acb-00aa-4a02-86bc-476cbe66d7fa",
    "subscriptionId":"cd9c6a3a-7576-49f2-b27e-1e5136e57f45",
    "offerId": "sampleSaaSOffer", // Provided with "Update" action
    "publisherId": "contoso", 
    "planId": "silver",     // Provided with "Update" action
    "action": "Activate", // Activate/Delete/Suspend/Reinstate/Update
    "timeStamp": "2018-12-01T00:00:00"
  }
```

| **Parametre adı**     | **Veri türü** | **Açıklama**                               |
|------------------------|---------------|-----------------------------------------------|
| id  | String       | Tetiklenen işlem benzersiz kimliği.                |
| activityId   | String        | Hizmet isteği izlemek için benzersiz bir dize değeri. Bu, tüm Mutabakatları için kullanılır.               |
| subscriptionId                     | String        | Azure abonelik kaynak kimliği, SaaS.    |
| offerId                | String        | Teklif kullanıcıya abone kimliği. Yalnızca "Güncelleştir" eylemi ile sağlanır.        |
| publisherId                | String        | Yayımcı kimliği SaaS teklifi         |
| Planıd                 | String        | Bir kullanıcı abone kimliği planlayın. Yalnızca "Güncelleştir" eylemi ile sağlanır.          |
| action                 | String        | Bu bildirim tetikleme eylem. Olası değerler - etkinleştirme, silme, askıda kalma, eski duruma getirme, güncelleştirme          |
| Zaman damgası                 | String        | Bu bildirim tetiklendiğinde UTC zaman damgası değeri.          |
|  |  |  |
