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
ms.openlocfilehash: 135ee9f6b833165cd393b9c5ca582e0ee9499e0f
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54057429"
---
## <a name="register-your-application"></a>Uygulamanızı kaydetme

1. Oturum [Azure portalında](https://portal.azure.com/) bir uygulamayı kaydetme.
1. Hesabınız size birden fazla Azure AD kiracısına erişim sunuyorsa sağ üst köşeden hesabınızı seçin ve portal oturumunuzu istediğiniz Azure AD kiracısına ayarlayın.
1. Sol taraftaki gezinti bölmesinde **Azure Active Directory** hizmetini seçin ve ardından **Uygulama kayıtları (Önizleme) > Yeni kayıt** seçeneğini belirleyin.
1. Zaman **bir uygulamayı kaydetme** sayfası görüntülenirse, uygulamanız için bir ad girin.
1. Altında **desteklenen hesap türleri**seçin **herhangi bir kuruluş dizinini ve kişisel Microsoft hesapları hesaplarında**.
1. Altında **yeniden yönlendirme URI'si** bölümünden **Web** platform ve uygulama URL'sine değerine göre web sunucunuzda kümesi. Ayarlayın ve Visual Studio ve düğüm yeniden yönlendirme URL'sini almak hakkında yönergeler için aşağıdaki bölümlere bakın.
1. Bittiğinde **Kaydet**’i seçin.
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
        clientID: "Enter_the_Application_Id_here",
        authority: "https://login.microsoftonline.com/common",
        graphScopes: ["user.read"],
        graphEndpoint: "https://graph.microsoft.com/v1.0/me"
    };
    ```

<ol start="2">
<li>
Değiştirin <code>Enter the application Id here</code> yalnızca kayıtlı uygulama Kimliğine sahip.
</li>
</ol>
