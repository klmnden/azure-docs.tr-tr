---
title: "Azure Active Directory kimlik koruması ve Microsoft Graph kullanmaya başlama | Microsoft Docs"
description: "Sorgu Microsoft Graph risk olaylarına ve ilişkili bilgi listesi için Azure Active Directory'den tanıtılmaktadır."
services: active-directory
keywords: "Azure active directory kimlik koruması, risk olay, güvenlik açığı, güvenlik ilkesi, Microsoft Graph"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: fa109ba7-a914-437b-821d-2bd98e681386
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/08/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 568cad4e394dfedddce1bfce66ddf627947d7568
ms.sourcegitcommit: adf6a4c89364394931c1d29e4057a50799c90fc0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/09/2017
---
# <a name="get-started-with-azure-active-directory-identity-protection-and-microsoft-graph"></a>Azure Active Directory kimlik koruması ve Microsoft Graph kullanmaya başlama
Microsoft Graph olan Microsoft unified API uç noktasını ve giriş, [Azure Active Directory kimlik koruması](active-directory-identityprotection.md) API'leri. İlk API **identityRiskEvents**, bir listesi için Microsoft Graph sorgu sayesinde [risk olayları](active-directory-identityprotection-risk-events-types.md) ve bilgi ilişkili. Bu makalede bu API sorgulama başlamanızı sağlar. Bir ayrıntılı giriş, tam belgelere ve Graph Explorer'da erişim için bakın [Microsoft Graph site](https://graph.microsoft.io/).

> [!IMPORTANT]
> Microsoft, Azure AD’yi bu makalede bahsedilen Klasik Azure Portalı yerine Azure portalındaki [Azure AD yönetim merkezini](https://aad.portal.azure.com) kullanarak yönetmenizi öneriyor.

Microsoft Graph kimlik koruması verilere erişmek için üç adım vardır:

1. Bir istemci parolası uygulamayla ekleyin. 
2. Bu gizli anahtar ve birkaç bilgi parçalarını bir kimlik doğrulama belirteci almanıza Microsoft Graph için kimlik doğrulaması yapmak için kullanın. 
3. İstekte API uç noktasına ve kimlik koruma verilerini geri almak için bu belirteci kullanın.

Başlamadan önce ihtiyacınız vardır:

* Azure AD içinde uygulama oluşturmak için yönetici ayrıcalıkları
* Kiracı'nın etki alanı (örneğin, contoso.onmicrosoft.com) adı

## <a name="add-an-application-with-a-client-secret"></a>Bir istemci parolası ile uygulama ekleme
1. [Oturum](https://manage.windowsazure.com) yönetici olarak, Azure Klasik portalı. 
2. Sol gezinti bölmesinde, tıklayın **Active Directory**. 
   
    ![Uygulama oluşturma](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_01.png)
3. Gelen **Directory** listesinde, directory tümleştirmesini etkinleştirmek istediğiniz dizini seçin.
4. Üstteki menüde tıklatın **uygulamaları**.
   
    ![Uygulama oluşturma](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_02.png)
5. Tıklatın **Ekle** sayfanın sonundaki.
   
    ![Uygulama oluşturma](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_03.png)
6. Üzerinde **ne yapmak istiyorsunuz** iletişim kutusunda, tıklatın **kuruluşumun geliştirmekte olduğu bir uygulama Ekle**.
   
    ![Uygulama oluşturma](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_04.png)
7. Üzerinde **bize uygulamanızı anlatın** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
    ![Uygulama oluşturma](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_05.png)
   
    a. İçinde **adı** metin kutusuna, uygulamanız için bir ad yazın (örneğin: AADIP Risk olayı API uygulama).
   
    b. Olarak **türü**seçin **Web uygulaması ve / veya Web API**.
   
    c. **İleri**’ye tıklayın.
8. Üzerinde **uygulama özellikleri** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
    ![Uygulama oluşturma](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_06.png)
   
    a. İçinde **oturum açma URL'si** metin kutusuna, türü `http://localhost`.
   
    b. İçinde **uygulama kimliği URI'si** metin kutusuna, türü `http://localhost`.
   
    c. **Tamamla**’ya tıklayın.

Can şimdi uygulamanızı yapılandırın.

![Uygulama oluşturma](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_07.png)

## <a name="grant-your-application-permission-to-use-the-api"></a>Uygulama API kullanma izni verin
1. Uygulamanızın sayfasında, üst menüsünde tıklatın **yapılandırma**. 
   
    ![Uygulama oluşturma](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_08.png)
2. İçinde **diğer uygulamalara izinler** 'yi tıklatın **uygulama eklemek**.
   
    ![Uygulama oluşturma](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_09.png)
3. Üzerinde **diğer uygulamalara izinler** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
    ![Uygulama oluşturma](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_10.png)
   
    a. Seçin **Microsoft Graph**.
   
    b. **Tamamla**’ya tıklayın.
4. Tıklatın **uygulama izinleri: 0**ve ardından **tüm kimlik risk olay bilgilerini okuma**.
   
    ![Uygulama oluşturma](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_11.png)
5. Sayfanın alt kısmındaki **Kaydet**’e tıklayın.
   
    ![Uygulama oluşturma](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_12.png)

## <a name="get-an-access-key"></a>Bir erişim anahtarı alma
1. Uygulamanızın sayfasında içinde **anahtarları** bölümünde, 1 yıl süre olarak seçin.
   
    ![Uygulama oluşturma](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_13.png)
2. Sayfanın alt kısmındaki **Kaydet**’e tıklayın.
   
    ![Uygulama oluşturma](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_12.png)
3. anahtarları bölümünde, yeni oluşturulan anahtarı değerini kopyalayın ve güvenli bir konuma yapıştırın.
   
    ![Uygulama oluşturma](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_14.png)
   
   > [!NOTE]
   > Bu anahtar kaybederseniz, bu bölüme geri dönün ve yeni bir anahtar oluşturun gerekecektir. Bu anahtarı bir gizli tutma: olan herkes verilerinize erişebilir.
   > 
   > 
4. İçinde **özellikleri** bölümünde, kopyalama **istemci kimliği**ve güvenli bir konuma yapıştırın. 

## <a name="authenticate-to-microsoft-graph-and-query-the-identity-risk-events-api"></a>Microsoft Graph için kimlik doğrulaması ve kimlik Risk olayları API sorgulama
Bu noktada, olmalıdır:

* İstemci kimliği üzerinde kopyalanır
* Yukarıdaki kopyaladığınız anahtar
* Kiracı'nın etki alanı adı

Kimlik doğrulaması için bir post İsteği Gönder `https://login.microsoft.com` gövdesinde aşağıdaki parametrelerle:

* grant_type: "**client_credentials**"
* Kaynak: "**https://graph.microsoft.com**"
* client_id:<your client ID>
* client_secret:<your key>

> [!NOTE]
> İçin değerler sağlaması gereken **client_id** ve **client_secret** parametresi.
> 
> 

Başarılı olursa, bu kimlik doğrulama belirtecini döndürür.  
API'yi çağırmak için bir başlığı aşağıdaki parametreyle oluşturun:

    `Authorization`=”<token_type> <access_token>"


Kimlik doğrulamasını yaparken, belirteç türü ve erişim belirteci döndürülen belirteç bulabilirsiniz.

Bu üst aşağıdaki API URL'si bir istek gönderin:`https://graph.microsoft.com/beta/identityRiskEvents`

Yanıt başarılı olursa, kimlik risk olaylarına ve ilişkili veriler, ayrıştırılmış ve uygun gördüğünüz şekilde ele OData JSON biçiminde koleksiyonudur.

Kimlik doğrulaması ve Powershell kullanarak API'yi çağıran için örnek kod aşağıda verilmiştir.  
Yalnızca istemci Kimliğiniz Ekle anahtarı ve Kiracı etki alanı.

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

## <a name="additional-resources"></a>Ek kaynaklar
* [Azure Active Directory kimlik koruması](active-directory-identityprotection.md)
* [Azure Active Directory kimlik koruması tarafından algılanan risk olayı türleri](active-directory-identityprotection-risk-events-types.md)
* [Microsoft Graph](https://graph.microsoft.io/)
* [Microsoft Graph genel bakış](https://graph.microsoft.io/docs)
* [Azure AD kimlik koruması hizmet kök](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root)

