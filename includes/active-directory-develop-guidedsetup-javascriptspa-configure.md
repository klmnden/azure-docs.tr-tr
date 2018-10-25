---
title: include dosyası
description: include dosyası
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: nacanuma
ms.custom: include file
ms.openlocfilehash: b3d46e10facdef26b36c910a5c23b40a415a2894
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "49988451"
---
## <a name="register-your-application"></a>Uygulamanızı kaydetme

Bir uygulamayı kaydetme birden çok yolu vardır. Gereksinimlerinize en uygun seçeneği belirleyin:
* [Hızlı mod - uygulamayı yapılandırmak için SPA Hızlı Başlangıç'ı kullanma](#option-1-register-your-application-express-mode)
* [Gelişmiş mod - el ile uygulama ayarlarını yapılandırma](#option-2-register-your-application-advanced-mode)

### <a name="option-1-register-your-application-express-mode"></a>1. seçenek: Register uygulamanızı (hızlı mod)

1. Oturum [(Önizleme) Azure portalı uygulaması kayıt](https://portal.azure.com/?Microsoft_AAD_RegisteredApps=true#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/JavascriptSpaQuickstartPage/sourceType/docs) bir uygulamayı kaydetme.
1. Üzerinde **bir uygulamayı kaydetme** sayfasında, uygulamanız için bir ad girin.
1. Altında **desteklenen hesap türleri**seçin **herhangi bir kuruluş dizinini ve kişisel Microsoft hesapları hesaplarında**.
1. İşiniz bittiğinde seçin **kaydetme**.
1. İndirmek ve yeni uygulamanız için tek bir tıklamayla otomatik olarak yapılandırmak için hızlı başlangıç yönergeleri izleyin.

### <a name="option-2-register-your-application-advanced-mode"></a>2. seçenek: Register uygulamanızı (Gelişmiş mod)

1. Oturum [Azure portalında](https://portal.azure.com/) bir uygulamayı kaydetme.
1. Hesabınız birden fazla kiracıya erişmenizi sağlar, seçin, hesabınızdaki sağ üst köşe ve portal oturumunuzu ayarlamak istediğiniz Azure AD ile Kiracı.
1. Sol gezinti bölmesinde seçin **Azure Active Directory** hizmet ve ardından **uygulama kayıtları (Önizleme) > Yeni kayıt**.
1. Zaman **bir uygulamayı kaydetme** sayfası görüntülenirse, uygulamanız için bir ad girin.
1. Altında **desteklenen hesap türleri**seçin **herhangi bir kuruluş dizinini ve kişisel Microsoft hesapları hesaplarında**.
1. Altında **yeniden yönlendirme URI'si** bölümünden **Web** platform ve uygulama URL'sine değerine göre web sunucunuzda kümesi. Ayarlayın ve Visual Studio ve düğüm yeniden yönlendirme URL'sini almak hakkında yönergeler için aşağıdaki bölümlere bakın.
1. İşiniz bittiğinde seçin **kaydetme**.
1. Uygulamasında **genel bakış** sayfa, Not **uygulama (istemci) kimliği** değeri.
1. Bu Hızlı Başlangıç [örtük izin akışı](../articles/active-directory/develop/v2-oauth2-implicit-grant-flow.md) etkinleştirilecek. Kayıtlı uygulama sol gezinti bölmesinde seçin **kimlik doğrulaması**.
1. İçinde **Gelişmiş ayarlar**altında **örtük vermeyi**, her ikisini de etkinleştirmek **kimlik belirteçlerini** ve **erişim belirteçlerini** onay kutularını. Bu uygulama kullanıcılarının oturumunu ve bir API'yi çağırmak sonun kimliği ve erişim belirteçler gereklidir.
1. **Kaydet**’i seçin.

> #### <a name="setting-the-redirect-url-for-node"></a>Düğümü yeniden yönlendirme URL'sini ayarlama
> Node.js için web sunucusu bağlantı noktası ayarlayabilirsiniz *server.js* dosya. Bu öğretici için başvuru 30662 bağlantı noktasını kullanır ancak kullanılabilir herhangi bir bağlantı kullanabilirsiniz. Uygulama kayıt bilgileri bir yeniden yönlendirme URL'sini ayarlamak için aşağıdaki yönergeleri izleyin:<br/>
> - Dönmek *uygulama kaydı* ayarlayıp `http://localhost:30662/` olarak bir `Redirect URL`, veya `http://localhost:[port]/` özel bir TCP bağlantı noktası kullanıyorsanız (burada *[bağlantı noktası]* özel TCP bağlantı noktası numarası).

<p/>

> #### <a name="visual-studio-instructions-for-obtaining-the-redirect-url"></a>Yeniden yönlendirme URL'sini almak için visual Studio yönergeleri
> Yeniden yönlendirme URL'sini almak için aşağıdaki adımları izleyin:
> 1. İçinde **Çözüm Gezgini**, projeyi seçin ve bakmak **özellikleri** penceresi. Görmüyorsanız, bir **özellikleri** penceresinde, tuşuna **F4**.
> 2. Değeri Şuradan Kopyala: **URL** Pano için:<br/> ![Proje Özellikleri](media/active-directory-develop-guidedsetup-javascriptspa-configure/vs-project-properties-screenshot.png)<br />
> 3. Dönmek *uygulama kaydı* ve değer olarak ayarlanmış bir **tekrar yönlendirme URL'sini**.

#### <a name="configure-your-javascript-spa"></a>JavaScript SPA'ya yapılandırın

1. İçinde `index.html` dosyası proje Kurulum sırasında oluşturulur, uygulama kayıt bilgilerini ekleyin. Üst içinde aşağıdaki kodu ekleyin `<script></script>` gövdesinde etiketleri, `index.html` dosyası:

    ```javascript
    var applicationConfig = {
        clientID: "[Enter the application Id here]",
        graphScopes: ["user.read"],
        graphEndpoint: "https://graph.microsoft.com/v1.0/me"
    };
    ```

<ol start="2">
<li>
Değiştirin <code>Enter the application Id here</code> yalnızca kayıtlı uygulama Kimliğine sahip.
</li>
</ol>
