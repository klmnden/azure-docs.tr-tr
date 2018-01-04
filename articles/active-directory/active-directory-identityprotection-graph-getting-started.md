---
title: "Azure Active Directory kimlik koruması ve Microsoft Graph kullanmaya başlama | Microsoft Docs"
description: "Azure Active Directory'den ilgili bilgileri ve risk olaylarını listesi için Microsoft Graph sorgu öğrenin."
services: active-directory
keywords: "Azure active directory kimlik koruması, risk olay, güvenlik açığı, güvenlik ilkesi, Microsoft Graph"
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: fa109ba7-a914-437b-821d-2bd98e681386
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: fafad74f46baaf56a8220dab05028781b2f2258e
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="get-started-with-azure-active-directory-identity-protection-and-microsoft-graph"></a>Azure Active Directory kimlik koruması ve Microsoft Graph kullanmaya başlama
Microsoft Graph olan Microsoft unified API uç noktasını ve giriş, [Azure Active Directory kimlik koruması](active-directory-identityprotection.md) API'leri. İlk API **identityRiskEvents**, bir listesi için Microsoft Graph sorgu sayesinde [risk olayları](active-directory-identityprotection-risk-events-types.md) ve bilgi ilişkili. Bu makalede bu API sorgulama başlamanızı sağlar. Bir ayrıntılı giriş, tam belgelere ve Graph Explorer'da erişim için bakın [Microsoft Graph site](https://graph.microsoft.io/).


Microsoft Graph kimlik koruması verilere erişmek için dört adım vardır:

1. Etki alanınızın adını alır.
2. Yeni bir uygulama kaydı oluşturun. 
2. Bu gizli anahtar ve birkaç bilgi parçalarını bir kimlik doğrulama belirteci almanıza Microsoft Graph için kimlik doğrulaması yapmak için kullanın. 
3. İstekte API uç noktasına ve kimlik koruma verilerini geri almak için bu belirteci kullanın.

Başlamadan önce ihtiyacınız vardır:

* Azure AD içinde uygulama oluşturmak için yönetici ayrıcalıkları
* Kiracı'nın etki alanı (örneğin, contoso.onmicrosoft.com) adı


## <a name="retrieve-your-domain-name"></a>Etki alanı adınızı alma 

1. [Oturum](https://portal.azure.com) , Azure Portal'da yönetici olarak. 

2. Sol gezinti bölmesinde tıklatın **Active Directory**. 
   
    ![Uygulama oluşturma](./media/active-directory-identityprotection-graph-getting-started/41.png)


3. İçinde **Yönet** 'yi tıklatın **özellikleri**.

    ![Uygulama oluşturma](./media/active-directory-identityprotection-graph-getting-started/42.png)

4. Etki alanı adınızı kopyalayın.


## <a name="create-a-new-app-registration"></a>Yeni bir uygulama kaydı oluşturma

1. Üzerinde **Active Directory** sayfasında **Yönet** 'yi tıklatın **uygulama kayıtlar**.

    ![Uygulama oluşturma](./media/active-directory-identityprotection-graph-getting-started/42.png)


2. Üstteki menüde tıklatın **yeni uygulama kaydı**.
   
    ![Uygulama oluşturma](./media/active-directory-identityprotection-graph-getting-started/43.png)

3. Üzerinde **oluşturma** sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Uygulama oluşturma](./media/active-directory-identityprotection-graph-getting-started/44.png)

    a. İçinde **adı** metin kutusuna, uygulamanız için bir ad yazın (örneğin: AADIP Risk olayı API uygulama).
   
    b. Olarak **türü**seçin **Web uygulaması ve / veya Web API**.
   
    c. İçinde **oturum açma URL'si** metin kutusuna, türü `http://localhost`.

    d. **Oluştur**'a tıklayın.

4. Açmak için **ayarları** uygulamalar listesinde sayfaya, yeni oluşturulan uygulama kayıt'a tıklayın. 

5. Kopya **uygulama kimliği**.


## <a name="grant-your-application-permission-to-use-the-api"></a>Uygulama API kullanma izni verin

1. Üzerinde **ayarları** sayfasında, **gerekli izinleri**.
   
    ![Uygulama oluşturma](./media/active-directory-identityprotection-graph-getting-started/15.png)

2. Üzerinde **gerekli izinleri** üstteki araç çubuğunda sayfasını tıklatın **Ekle**.
   
    ![Uygulama oluşturma](./media/active-directory-identityprotection-graph-getting-started/16.png)
   
3. Üzerinde **API Ekle erişim** sayfasında, **bir API seçin**.
   
    ![Uygulama oluşturma](./media/active-directory-identityprotection-graph-getting-started/17.png)

4. Üzerinde **bir API seçin** sayfasında **Microsoft Graph**ve ardından **seçin**.
   
    ![Uygulama oluşturma](./media/active-directory-identityprotection-graph-getting-started/18.png)

5. Üzerinde **API Ekle erişim** sayfasında, **seçin izinleri**.
   
    ![Uygulama oluşturma](./media/active-directory-identityprotection-graph-getting-started/19.png)

6. Üzerinde **erişimi etkinleştir** sayfasında, **tüm kimlik risk bilgilerini okuma**ve ardından **seçin**.
   
    ![Uygulama oluşturma](./media/active-directory-identityprotection-graph-getting-started/20.png)

7. Üzerinde **API Ekle erişim** sayfasında, **Bitti**.
   
    ![Uygulama oluşturma](./media/active-directory-identityprotection-graph-getting-started/21.png)

8. Üzerinde **gerekli izinler** sayfasında, **izinler**ve ardından **Evet**.
   
    ![Uygulama oluşturma](./media/active-directory-identityprotection-graph-getting-started/22.png)



## <a name="get-an-access-key"></a>Bir erişim anahtarı alma

1. Üzerinde **ayarları** sayfasında, **anahtarları**.
   
    ![Uygulama oluşturma](./media/active-directory-identityprotection-graph-getting-started/23.png)

2. Üzerinde **anahtarları** sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Uygulama oluşturma](./media/active-directory-identityprotection-graph-getting-started/24.png)

    a. İçinde **anahtar açıklama** metin kutusuna, bir açıklama yazın (örneğin, *AADIP Risk olayı*).
    
    b. Olarak **süresi**seçin **1 yıl**.

    c. **Kaydet** düğmesine tıklayın.
   
    d. Anahtar değerini kopyalayın ve güvenli bir konuma yapıştırın.   
   
   > [!NOTE]
   > Bu anahtar kaybederseniz, bu bölüme geri dönün ve yeni bir anahtar oluşturun gerekecektir. Bu anahtarı bir gizli tutma: olan herkes verilerinize erişebilir.
   > 
   > 

## <a name="authenticate-to-microsoft-graph-and-query-the-identity-risk-events-api"></a>Microsoft Graph için kimlik doğrulaması ve kimlik Risk olayları API sorgulama
Bu noktada, olmalıdır:

- Kiracı'nın etki alanı adı

- İstemci kimliği 

- Anahtar 


Kimlik doğrulaması için bir post İsteği Gönder `https://login.microsoft.com` gövdesinde aşağıdaki parametrelerle:

- grant_type: "**client_credentials**"

-  Kaynak: "**https://graph.microsoft.com**"

- client_id: \<, istemci kimliği\>

- client_secret: \<anahtarınızı\>


Başarılı olursa, bu kimlik doğrulama belirtecini döndürür.  
API'yi çağırmak için bir başlığı aşağıdaki parametreyle oluşturun:

    `Authorization`=”<token_type> <access_token>"


Kimlik doğrulamasını yaparken, belirteç türü ve erişim belirteci döndürülen belirteç bulabilirsiniz.

Bu üst aşağıdaki API URL'si bir istek gönderin:`https://graph.microsoft.com/beta/identityRiskEvents`

Yanıt başarılı olursa, kimlik risk olaylarına ve ilişkili veriler, ayrıştırılmış ve uygun gördüğünüz şekilde ele OData JSON biçiminde koleksiyonudur.

Kimlik doğrulaması ve PowerShell kullanarak API'yi çağıran için örnek kod aşağıda verilmiştir.  
İstemci kimliği, gizli anahtar ve Kiracı etki alanı eklemeniz yeterlidir.

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


## <a name="next-steps"></a>Sonraki adımlar

Tebrikler, Microsoft Graph, ilk çağrıda yaptığınız.  
Şimdi kimlik risk olaylarını sorgular ve uygun gördüğünüz ancak verileri kullanın.

Microsoft Graph ve grafik API'sini kullanarak uygulamaların nasıl yapılandırıldığı hakkında daha fazla bilgi için kullanıma [belgelerine](https://graph.microsoft.io/docs) ve çok daha fazlası üzerinde [Microsoft Graph site](https://graph.microsoft.io/). Ayrıca, yer işareti emin [Azure AD Identity Protection API](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root) tüm grafiğinde kullanılabilir kimlik koruma API'leri listeler sayfası. API aracılığıyla kimlik koruması çalışmak için yeni yollar eklediğimiz gibi ilgili sayfada görürsünüz.

İlgili bilgi için bkz.:

-  [Azure Active Directory kimlik koruması](active-directory-identityprotection.md)

-  [Azure Active Directory kimlik koruması tarafından algılanan risk olayı türleri](active-directory-identityprotection-risk-events-types.md)

- [Microsoft Graph](https://graph.microsoft.io/)

- [Microsoft Graph genel bakış](https://graph.microsoft.io/docs)

- [Azure AD kimlik koruması hizmet kök](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root)

