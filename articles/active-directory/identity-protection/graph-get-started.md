---
title: Microsoft Graph için Azure Active Directory kimlik koruması | Microsoft Docs
description: Risk olayları ve ilişkili Azure Active Directory bilgilerinden bir listesi için Microsoft Graph sorgulamayı öğrenin.
services: active-directory
keywords: Azure active directory kimlik koruması, risk olayı, güvenlik açığı, güvenlik ilkesi, Microsoft Graph
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
ms.assetid: fa109ba7-a914-437b-821d-2bd98e681386
ms.service: active-directory
ms.subservice: identity-protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/25/2019
ms.author: joflore
ms.reviewer: sahandle
ms.custom: seohack1
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3357cfd5e845346534f263c768b5cf6b6a38ea4e
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58793994"
---
# <a name="get-started-with-azure-active-directory-identity-protection-and-microsoft-graph"></a>Microsoft Graph ve Azure Active Directory kimlik koruması ile çalışmaya başlama

Microsoft Graph olan Microsoft unified API uç noktası ve giriş, [Azure Active Directory kimlik koruması](../active-directory-identityprotection.md) API'leri. Riskli kullanıcılar ve oturum açma işlemleri hakkındaki bilgilerin açığa çıkmasına neden üç API vardır. İlk API **identityRiskEvents**, Microsoft Graph listesi için sorgu sağlar [risk olayları](../reports-monitoring/concept-risk-events.md) ve ilişkili bilgileri. İkinci bir API **riskyUsers**, kullanıcıların algılanan risk kimlik koruması hakkında daha fazla bilgi için Microsoft Graph sorguya izin verir. Üçüncü API **Signın**risk durumuyla ilgili belirli özellikleri olan Azure AD oturum açma işlemleri hakkında bilgi için Microsoft Graph sorgu sağlar ayrıntılı ve düzey. Bu makalede, Microsoft Graph için bağlanma ve sorgulama bu API'leri ile çalışmaya başlamanızı sağlar. Bir ayrıntılı giriş, tüm belgeler ve Graph Gezgini erişimi için bkz. [Microsoft Graph site](https://graph.microsoft.io/) veya bu API'leri için belirli başvuru belgeleri:

* [identityRiskEvents API](https://docs.microsoft.com/graph/api/resources/identityriskevent?view=graph-rest-beta)
* [riskyUsers API](https://docs.microsoft.com/graph/api/resources/riskyuser?view=graph-rest-beta)
* [Signın API](https://docs.microsoft.com/graph/api/resources/signin?view=graph-rest-beta)


## <a name="connect-to-microsoft-graph"></a>Microsoft Graph'a bağlanma

Microsoft Graph üzerinden kimlik koruması verilere erişmek için dört adım vardır:

1. Etki alanınızın adını alın.
2. Yeni bir uygulama kaydı oluşturun. 
3. Bir kimlik doğrulama belirteci nereden alacağınızı Microsoft Graph üzerinde kimlik doğrulaması için bu gizli dizi ve bazı diğer bilgilere kullanın. 
4. API uç noktasına yönelik isteklerde ve kimlik koruma verilerini geri almak için bu belirteci kullanın.

Başlamadan önce şunları yapmanız gerekir:

* Azure AD'de uygulama oluşturmak için yönetici ayrıcalıkları
* Kiracınızın etki alanının (örneğin, contoso.onmicrosoft.com)


## <a name="retrieve-your-domain-name"></a>Etki alanı adınızı alma 

1. [Oturum](https://portal.azure.com) yönetici olarak, Azure portalı. 

2. Sol gezinti bölmesinde **Active Directory**. 
   
    ![Uygulama oluşturma](./media/graph-get-started/41.png)


3. İçinde **Yönet** bölümünde **özellikleri**.

    ![Uygulama oluşturma](./media/graph-get-started/42.png)

4. Etki alanı adınızı kopyalayın.


## <a name="create-a-new-app-registration"></a>Yeni bir uygulama kaydı oluşturma

1. Üzerinde **Active Directory** sayfasında **Yönet** bölümünde **uygulama kayıtları**.

    ![Uygulama oluşturma](./media/graph-get-started/42.png)


2. Üstteki menüden **yeni uygulama kaydı**.
   
    ![Uygulama oluşturma](./media/graph-get-started/43.png)

3. Üzerinde **Oluştur** sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Uygulama oluşturma](./media/graph-get-started/44.png)

    a. İçinde **adı** metin kutusu, uygulamanız için bir ad yazın (örneğin: AADIP Risk olayı API uygulaması).
   
    b. Olarak **türü**seçin **Web uygulaması ve / veya Web API**.
   
    c. İçinde **oturum açma URL'si** metin kutusuna `http://localhost`.

    d. **Oluştur**’a tıklayın.

4. Açmak için **ayarları** sayfasında, uygulamalar listesinde, yeni oluşturulan uygulama kayıt'a tıklayın. 

5. Kopyalama **uygulama kimliği**.


## <a name="grant-your-application-permission-to-use-the-api"></a>API'yi kullanmak için uygulama izni verme

1. Üzerinde **ayarları** sayfasında **gerekli izinler**.
   
    ![Uygulama oluşturma](./media/graph-get-started/15.png)

2. Üzerinde **gerekli izinler** sayfasında, üstteki araç çubuğunda tıklatın **Ekle**.
   
    ![Uygulama oluşturma](./media/graph-get-started/16.png)
   
3. Üzerinde **API erişimi Ekle** sayfasında **bir API seçin**.
   
    ![Uygulama oluşturma](./media/graph-get-started/17.png)

4. Üzerinde **bir API seçin** sayfasında **Microsoft Graph**ve ardından **seçin**.
   
    ![Uygulama oluşturma](./media/graph-get-started/18.png)

5. Üzerinde **API erişimi Ekle** sayfasında **izinleri seçin**.
   
    ![Uygulama oluşturma](./media/graph-get-started/19.png)

6. Üzerinde **erişimini etkinleştir** sayfasında **tüm kimlik risk bilgilerini okuma**ve ardından **seçin**.
   
    ![Uygulama oluşturma](./media/graph-get-started/20.png)

7. Üzerinde **API erişimi Ekle** sayfasında **Bitti**.
   
    ![Uygulama oluşturma](./media/graph-get-started/21.png)

8. Üzerinde **gerekli izinler** sayfasında **izinler**ve ardından **Evet**.
   
    ![Uygulama oluşturma](./media/graph-get-started/22.png)



## <a name="get-an-access-key"></a>Bir erişim anahtarı alma

1. Üzerinde **ayarları** sayfasında **anahtarları**.
   
    ![Uygulama oluşturma](./media/graph-get-started/23.png)

2. Üzerinde **anahtarları** sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Uygulama oluşturma](./media/graph-get-started/24.png)

    a. İçinde **anahtar açıklaması** metin kutusuna bir açıklama yazın (örneğin, *AADIP Risk olayı*).
    
    b. Olarak **süresi**seçin **1 yıl**.

    c. **Kaydet**’e tıklayın.
   
    d. Anahtar değerini kopyalayın ve güvenli bir konuma yapıştırın.   
   
   > [!NOTE]
   > Bu anahtar kaybederseniz, bu bölüme geri dönün ve yeni bir anahtar oluşturmanız gerekir. Bu anahtarı gizli tutmak: sahip herkes verilerinize erişebilirsiniz.
   > 
   > 

## <a name="authenticate-to-microsoft-graph-and-query-the-identity-risk-events-api"></a>Microsoft Graph üzerinde kimlik doğrulaması ve kimlik Risk olayları API sorgulama

Bu noktada, aşağıdakiler:

- Kiracınızın etki alanı adı

- İstemci kimliği 

- Anahtarı 


Kimlik doğrulaması için bir post isteği gönderin. `https://login.microsoft.com` gövdesinde şu parametrelerle:

- grant_type değeri: "**client_credentials**"

-  Kaynak: `https://graph.microsoft.com`

- client_id: \<istemci Kimliğiniz\>

- client_secret: \<anahtarınız\>


Başarılı olursa, bu kimlik doğrulama belirteci döndürür.  
API'yi çağırmak için aşağıdaki parametresiyle birlikte bir başlık oluşturun:

    `Authorization`=”<token_type> <access_token>"


Kimlik doğrulaması yapılırken döndürülen belirteç simge türü ve erişim belirteci bulabilirsiniz.

Bu üst bilgi aşağıdaki API URL'sine istek olarak gönder: `https://graph.microsoft.com/beta/identityRiskEvents`

Yanıt başarılı olursa, kimlik risk olayları ve ilişkili veriler ayrıştırılır ve gördüğünüz gibi ele OData JSON biçiminde koleksiyonudur.

Kimlik doğrulama ve PowerShell kullanarak API'yi çağırmak için örnek kod aşağıda verilmiştir.  
Kiracı etki alanı istemci Kimliğinizi ve gizli anahtarı eklemeniz yeterlidir.

    $ClientID       = "<your client ID here>"        # Should be a ~36 hex character string; insert your info here
    $ClientSecret   = "<your client secret here>"    # Should be a ~44 character string; insert your info here
    $tenantdomain   = "<your tenant domain here>"    # For example, contoso.onmicrosoft.com

    $loginURL       = "https://login.microsoft.com"
    $resource       = "https://graph.microsoft.com"

    $body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}
    $oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

    Write-Output $oauth

    if ($oauth.access_token -ne $null) {
        $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}

        $url = "https://graph.microsoft.com/beta/identityRiskEvents"
        Write-Output $url

        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)

        foreach ($event in ($myReport.Content | ConvertFrom-Json).value) {
            Write-Output $event
        }

    } else {
        Write-Host "ERROR: No Access Token"
    } 

## <a name="query-the-apis"></a>Sorgu API'leri

Bu üç API'leri, çok sayıda riskli kullanıcılar ve oturum açma, kuruluşunuzdaki hakkında bilgi almak için çeşitli fırsatlar sağlar. Aşağıda bu API'ler ve ilişkili örnek istekler için bazı ortak kullanım durumları verilmiştir. Yukarıda veya kullanarak örnek kodu kullanarak aşağıdaki sorguları çalıştırabilirsiniz [Graph Gezgini](https://developer.microsoft.com/graph/graph-explorer).

### <a name="get-the-high-risk-and-medium-risk-events-identityriskevents-api"></a>Yüksek riskli ve orta risk olayları (identityRiskEvents API)

Orta ve yüksek riskli olayları, kimlik koruması oturum açma tetikleyicisi veya kullanıcı risk ilkelerini yeteneği olabilir temsil eder. Kullanıcı oturum açma girişimi meşru kimlik sahibi değil Orta veya yüksek olasılığına sahip oldukları olduğundan, bu olayları düzeltme bir öncelik olmalıdır. 

```
GET https://graph.microsoft.com/beta/identityRiskEvents?`$filter=riskLevel eq 'high' or riskLevel eq 'medium'" 
```

### <a name="get-all-of-the-users-who-successfully-passed-an-mfa-challenge-triggered-by-risky-sign-ins-policy-riskyusers-api"></a>Tüm kullanıcılar başarıyla (riskyUsers API) riskli oturum açma ilke tarafından tetiklenen bir MFA sınaması başarılı

Kuruluşunuzu risk tabanlı ilkeler sahip kimlik koruması etkisini anlamak için tüm kullanıcılar başarıyla riskli oturum açma ilke tarafından tetiklenen bir MFA sınaması başarılı sorgulama yapabilirsiniz. Bu bilgiler, hangi anlamanıza yardımcı olabilir kimlik koruması yanlışlıkla risk ve yasal kullanıcıları kullanıcı algılandı AI riskli olduğunu düşündüğünde eylemleri gerçekleştiriliyor.

```
GET https://graph.microsoft.com/beta/riskyUsers?$filter=riskDetail eq 'userPassedMFADrivenByRiskBasedPolicy'
```

### <a name="get-all-the-risky-sign-ins-for-a-specific-user-signin-api"></a>Belirli bir kullanıcı (Signın API) için tüm riskli oturum açma işlemleri Al

Bir kullanıcı tehlikeye girmiş olabilecek düşündüğünüzde, riskli oturum açma işlemlerinin tümünde alarak kendi risk durumu daha iyi anlayabilirsiniz. 

```
https://graph.microsoft.com/beta/identityRiskEvents?`$filter=userID eq '<userID>' and riskState eq 'atRisk'
```




## <a name="next-steps"></a>Sonraki adımlar

Tebrikler, ilk çağrınızı Microsoft Graph için yaptığınız!  
Artık kimlik risk olayları sorgulamak ve gördüğünüz ancak verileri kullanın.


Microsoft Graph ve Graph API'sini kullanarak uygulamalar oluşturma hakkında daha fazla bilgi edinmek için kullanıma [belgeleri](https://docs.microsoft.com/graph/overview) ve çok daha fazlasını [Microsoft Graph site](https://developer.microsoft.com/graph). 


İlgili bilgiler için bkz:

-  [Azure Active Directory kimlik koruması](../active-directory-identityprotection.md)

-  [Azure Active Directory kimlik koruması tarafından algılanan risk olayı türleri](../reports-monitoring/concept-risk-events.md)

- [Microsoft Graph](https://developer.microsoft.com/graph/)

- [Microsoft Graph’a genel bakış](https://developer.microsoft.com/graph/docs)

- [Azure AD kimlik koruması hizmeti kök](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/identityprotection_root)