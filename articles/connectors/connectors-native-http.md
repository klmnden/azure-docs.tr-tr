---
title: "HTTP üzerinden - Azure Logic Apps ile herhangi bir uç nokta iletişim | Microsoft Docs"
description: "İletişim kurabilir logic apps ile herhangi bir uç nokta HTTP üzerinden oluşturun."
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
tags: connectors
ms.assetid: e11c6b4d-65a5-4d2d-8e13-38150db09c0b
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/15/2016
ms.author: jehollan; LADocs
ms.openlocfilehash: d422a07a27ffa62a673bd2d471ae4fc837251dee
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-http-action"></a>HTTP eylem ile çalışmaya başlama

HTTP eylem ile kuruluşunuz için iş akışları genişletmek ve herhangi bir uç nokta için HTTP üzerinden iletişim kurar.

Şunları yapabilirsiniz:

* Yönettiğiniz bir Web sitesi azaldığında (tetikleyici) etkinleştirme uygulama iş akışları mantığı oluşturun.
* Herhangi bir uç nokta, iş akışlarınızı diğer Hizmetleri içine genişletmek için HTTP üzerinden iletişim kurar.

HTTP eylemi bir mantıksal uygulama kullanmaya başlamak için bkz: [mantıksal uygulama oluşturma](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="use-the-http-trigger"></a>HTTP tetikleyicisini kullanın
Bir tetikleyici bir mantıksal uygulama tanımlı iş akışını başlatmak için kullanılan bir olaydır. [Tetikleyiciler hakkında daha fazla bilgi](connectors-overview.md).

Burada, örnek dizisi mantığı Uygulama Tasarımcısı'nda HTTP tetikleyicisi ayarlama konusunda verilmiştir.

1. HTTP tetikleyicisini mantıksal uygulamanızı ekleyin.
2. Sorgulamak istediğiniz HTTP uç noktası parametrelerini doldurun.
3. Ne sıklıkta yoklanacağını üzerinde yinelenme aralığını değiştirin.

   Mantıksal uygulama artık her denetimi sırasında döndürülen herhangi bir içerik ile ateşlenir.

   ![HTTP tetikleyicisi](./media/connectors-native-http/using-trigger.png)

### <a name="how-the-http-trigger-works"></a>HTTP tetikleyicisini nasıl çalışır?

HTTP tetikleyicisini HTTP uç noktası için bir çağrı yinelenen bir aralıkta gönderir. Varsayılan olarak, 300'den düşük olduğu herhangi bir HTTP yanıt kodu çalıştırmak bir mantıksal uygulama neden olur. Mantıksal uygulama yangın olup olmadığını belirlemek için kod görünümünde mantıksal uygulama düzenleyebilir ve HTTP çağrısından sonra değerlendiren bir koşul ekleyin. Döndürülen durum kodu değerinden büyük veya eşit olduğunda tetiklenen bir HTTP tetikleyicisi bir örneği burada verilmiştir `400`.

```javascript
"Http":
{
    "conditions": [
        {
            "expression": "@greaterOrEquals(triggerOutputs()['statusCode'], 400)"
        }
    ],
    "inputs": {
        "method": "GET",
        "uri": "https://blogs.msdn.microsoft.com/logicapps/",
        "headers": {
            "accept-language": "en"
        }
    },
    "recurrence": {
        "frequency": "Second",
        "interval": 15
    },
    "type": "Http"
}
```

HTTP trigger parametreleri ilgili tam ayrıntılar bulunur [MSDN](https://msdn.microsoft.com/library/azure/mt643939.aspx#HTTP-trigger).

## <a name="use-the-http-action"></a>HTTP eylemi kullanın

Bir eylem, bir mantıksal uygulama içinde tanımlanan iş akışı tarafından gerçekleştirilen bir işlemdir. 
[Eylemler hakkında daha fazla bilgi](connectors-overview.md).

1. Seçin **yeni adım** > **Eylem Ekle**.
3. Eylem arama kutusuna yazın **http** HTTP eylemler listesi.
   
    ![HTTP eylemi seçin](./media/connectors-native-http/using-action-1.png)

4. HTTP çağrısı için gerekli parametreleri ekleyin.
   
    ![HTTP eylemi tamamlamak](./media/connectors-native-http/using-action-2.png)

5. Tasarımcı araç çubuğunda tıklatın **kaydetmek**. Mantıksal uygulamanızı kaydedildi ve aynı anda (etkin) yayımlandı.

## <a name="http-trigger"></a>HTTP tetikleyicisi
Aşağıda, bu bağlayıcıyı destekler tetikleyici için Ayrıntılar verilmiştir. HTTP Bağlayıcısı bir tetikleyici vardır.

| Tetikleyici | Açıklama |
| --- | --- |
| HTTP |Bir HTTP çağrı yapar ve yanıt içeriği döndürür. |

## <a name="http-action"></a>HTTP eylemi
Aşağıda, bu bağlayıcıyı destekler eylemi için Ayrıntılar verilmiştir. HTTP Bağlayıcısı olası eylem vardır.

| Eylem | Açıklama |
| --- | --- |
| HTTP |Bir HTTP çağrı yapar ve yanıt içeriği döndürür. |

## <a name="http-details"></a>HTTP ayrıntıları
Aşağıdaki tablolar, eylem ve eylem kullanımıyla ilişkili karşılık gelen çıkış ayrıntıları için gerekli ve isteğe bağlı giriş alanlarının açıklamaktadır.

#### <a name="http-request"></a>HTTP isteği
HTTP giden isteğinde eylemi için girdi alanlarının verilmiştir.
A * gerekli bir alan olduğu anlamına gelir.

| Görünen ad | Özellik adı | Açıklama |
| --- | --- | --- |
| Yöntemi * |Yöntemi |Kullanılacak HTTP fiili |
| URI * |URI |HTTP isteği için URI |
| Üstbilgileri |Üstbilgileri |HTTP üstbilgisi eklemek için bir JSON nesnesi |
| Gövde |Gövde |HTTP istek gövdesi |
| Kimlik Doğrulaması |Kimlik doğrulaması |İçinde ayrıntıları [kimlik doğrulaması](#authentication) bölümü |

<br>

#### <a name="output-details"></a>Çıkış Ayrıntıları
HTTP yanıtı için çıkış ayrıntıları verilmiştir.

| Özellik adı | Veri türü | Açıklama |
| --- | --- | --- |
| Üstbilgileri |Nesne |Yanıt Üstbilgileri |
| Gövde |Nesne |Yanıt nesnesi |
| Durum kodu |Int |HTTP durum kodu |

## <a name="authentication"></a>Kimlik Doğrulaması
Logic Apps özelliği, farklı türlerde HTTP uç noktaları karşı kimlik doğrulama kullanmanıza olanak sağlar. Bu kimlik doğrulaması ile kullanabileceğiniz **HTTP**,  **[HTTP + Swagger](connectors-native-http-swagger.md)**, ve  **[HTTP Web kancası](connectors-native-webhook.md)**  bağlayıcılar. Aşağıdaki kimlik doğrulama türlerini yapılandırılabilir:

* [Temel kimlik doğrulaması](#basic-authentication)
* [İstemci sertifikası kimlik doğrulaması](#client-certificate-authentication)
* [Azure Active Directory (Azure AD) OAuth kimlik doğrulaması](#azure-active-directory-oauth-authentication)

#### <a name="basic-authentication"></a>Temel kimlik doğrulaması

Aşağıdaki kimlik doğrulama nesnesini temel kimlik doğrulaması için gereklidir.
A * gerekli bir alan olduğu anlamına gelir.

| Özellik adı | Veri türü | Açıklama |
| --- | --- | --- |
| Türü * |type |Kimlik doğrulama türünü (olmalıdır `Basic` temel kimlik doğrulaması için) |
| Kullanıcı adı * |kullanıcı adı |Kimlik doğrulaması için kullanıcı adı |
| Parola * |password |Kimlik doğrulaması için parola |

> [!TIP]
> Kullanım tanımından geri alınamaz bir parola kullanmak istiyorsanız bir `securestring` parametre ve `@parameters()`  
>  [iş akışı tanımı işlevi](http://aka.ms/logicappdocs).

Örneğin:

```javascript
{
    "type": "Basic",
    "username": "user",
    "password": "test"
}
```

#### <a name="client-certificate-authentication"></a>İstemci sertifikası kimlik doğrulaması

Aşağıdaki kimlik doğrulama nesnesi için istemci sertifikası kimlik doğrulaması gereklidir. A * gerekli bir alan olduğu anlamına gelir.

| Özellik adı | Veri türü | Açıklama |
| --- | --- | --- |
| Türü * |type |Kimlik doğrulaması türü (olmalıdır `ClientCertificate` SSL istemci sertifikaları için) |
| PFX * |PFX |Kişisel bilgi değişimi (PFX) dosyası Base64 ile kodlanmış içeriği |
| Parola * |password |PFX dosyası erişim için parola |

> [!TIP]
> Mantıksal uygulama kaydetme sonra tanımında okunamaz bir parametre kullanmak üzere kullanabileceğiniz bir `securestring` parametre ve `@parameters()`  
>  [iş akışı tanımı işlevi](http://aka.ms/logicappdocs).

Örneğin:

```javascript
{
    "type": "ClientCertificate",
    "pfx": "aGVsbG8g...d29ybGQ=",
    "password": "@parameters('myPassword')"
}
```

#### <a name="azure-ad-oauth-authentication"></a>Azure AD OAuth kimlik doğrulaması
Aşağıdaki kimlik doğrulama nesnesini Azure AD OAuth kimlik doğrulaması için gereklidir. A * gerekli bir alan olduğu anlamına gelir.

| Özellik adı | Veri türü | Açıklama |
| --- | --- | --- |
| Türü * |type |Kimlik doğrulaması türü (olmalıdır `ActiveDirectoryOAuth` Azure AD OAuth için) |
| Kiracı * |Kiracı |Azure AD kiracısı için Kiracı tanımlayıcı |
| Hedef kitle * |Hedef kitle |Kaynak Yetkilendirme kullanmak istiyor. Örneğin, `https://management.core.windows.net/` |
| İstemci kimliği * |istemci kimliği |Azure AD uygulaması için istemci tanımlayıcısı |
| Gizli * |Gizli |Belirteç isteme istemci gizli anahtarı |

> [!TIP]
> Kullanabileceğiniz bir `securestring` parametre ve `@parameters()` [iş akışı tanımı işlevi](http://aka.ms/logicappdocs) kaydettikten sonra tanımında okunamaz bir parametre kullanmak için.
> 
> 

Örneğin:

```javascript
{
    "type": "ActiveDirectoryOAuth",
    "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db47",
    "audience": "https://management.core.windows.net/",
    "clientId": "34750e0b-72d1-4e4f-bbbe-664f6d04d411",
    "secret": "hcqgkYc9ebgNLA5c+GDg7xl9ZJMD88TmTJiJBgZ8dFo="
}
```

## <a name="next-steps"></a>Sonraki adımlar
Şimdi, platform deneyin ve [mantıksal uygulama oluşturma](../logic-apps/logic-apps-create-a-logic-app.md). Logic Apps diğer kullanılabilir bağlayıcılar bakarak keşfedebilirsiniz bizim [API'leri listesi](apis-list.md).

