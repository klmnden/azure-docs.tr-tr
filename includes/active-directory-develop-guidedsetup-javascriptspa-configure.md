---
title: include dosyası
description: include dosyası
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: nacanuma
ms.custom: include file
ms.openlocfilehash: 050bae64a62e90bdb74c93948109e255b69d7518
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67133108"
---
## <a name="register-your-application"></a>Uygulamanızı kaydetme

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. Hesabınız birden fazla kiracıya erişmenizi sağlar, sağ üst köşedeki hesabı seçin ve ardından kullanmak istediğiniz Azure AD kiracısı için portal oturumunuzu ayarlayın.
1. Geliştiriciler için Microsoft kimlik platformu Git [uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfası.
1. Zaman **bir uygulamayı kaydetme** sayfası görüntülenirse, uygulamanız için bir ad girin.
1. Altında **desteklenen hesap türleri**seçin **herhangi bir kuruluş dizinini ve kişisel Microsoft hesapları hesaplarında**.
1. Altında **yeniden yönlendirme URI'si** bölümünde aşağı açılan listesinde seçin **Web** platformu ve web sunucunuz üzerinde temel uygulama URL'si değerine ayarlayın. 

   Visual Studio ve Node.js yeniden yönlendirme URL'sini alma ve ayarlama hakkında daha fazla bilgi için sonraki iki bölümde bakın.

1. **Kaydol**’u seçin.
1. Uygulamasında **genel bakış** sayfa, Not **uygulama (istemci) kimliği** daha sonra kullanmak için değer.
1. Bu Hızlı Başlangıç [örtük izin akışı](../articles/active-directory/develop/v2-oauth2-implicit-grant-flow.md) etkinleştirilecek. Kayıtlı uygulama sol bölmesinde seçin **kimlik doğrulaması**.
1. İçinde **Gelişmiş ayarlar**altında **örtük vermeyi**seçin **kimlik belirteçlerini** ve **erişim belirteçlerini** onay kutuları. Bu uygulama kullanıcılarının oturumunu ve bir API'yi çağırması gerekir çünkü kimlik ve erişim belirteçler gereklidir.
1. **Kaydet**’i seçin.

> #### <a name="set-a-redirect-url-for-nodejs"></a>Node.js için bir yeniden yönlendirme URL'sini Ayarla
> Node.js için web sunucusu bağlantı noktası ayarlayabilirsiniz *server.js* dosya. Bu öğretici, başvuru için bağlantı noktası 30662 kullanır ancak kullanılabilir herhangi bir bağlantı kullanabilirsiniz. 
>
> Uygulama kayıt bilgileri bir yeniden yönlendirme URL'sini ayarlamak için geri dönmek **uygulama kaydı** bölmesinde aşağıdakilerden birini yapın:
>
> - Ayarlama *`http://localhost:30662/`* olarak **yeniden yönlendirme URL'si**.
> - Özel bir TCP bağlantı noktası kullanıyorsanız, *`http://localhost:<port>/`* (burada  *\<bağlantı noktası >* özel TCP bağlantı noktası numarası).
>
> #### <a name="set-a-redirect-url-for-visual-studio"></a>Visual Studio için bir yeniden yönlendirme URL'sini Ayarla
> Visual Studio için yeniden yönlendirme URL'sini almak için aşağıdakileri yapın:
> 1. İçinde **Çözüm Gezgini**, projeyi seçin.
>
>    **Özellikleri** penceresi açılır. Açılmazsa, basın **F4**.
>
>    ![JavaScriptSPA Proje Özellikleri penceresi](media/active-directory-develop-guidedsetup-javascriptspa-configure/vs-project-properties-screenshot.png)
>
> 1. Kopyalama **URL** değeri.
 
> 1. Dönmek **uygulama kaydı** bölmesinde, kopyalanan değeri olarak yapıştırın **tekrar yönlendirme URL'sini**.

#### <a name="configure-your-javascript-spa"></a>JavaScript SPA'ya yapılandırın

1. İçinde *index.html* Proje Kurulumu sırasında oluşturduğunuz dosyasını uygulama kayıt bilgilerini ekleyin. Dosya üst kısmındaki içinde `<script></script>` etiketler, aşağıdaki kodu ekleyin:

    ```javascript
    var msalConfig = {
        auth: {
            clientId: "<Enter_the_Application_Id_here>",
            authority: "https://login.microsoftonline.com/<Enter_the_Tenant_info_here>"
        },
        cache: {
            cacheLocation: "localStorage",
            storeAuthStateInCookie: true
        }
    };
    ```

    Konumlar:
    - *\<Enter_the_Application_Id_here >* olduğu **uygulama (istemci) kimliği** , kayıtlı uygulama için.
    - *\<Enter_the_Tenant_info_here >* aşağıdaki seçeneklerden birine ayarlayın:
       - Uygulamanız destekliyorsa *kuruluş bu dizinde hesapları*, bu değeri ile değiştirin **Kiracı kimliği** veya **Kiracı adı** (örneğin,  *contoso.microsoft.com*).
       - Uygulamanız destekliyorsa *herhangi bir kuruluş dizini hesaplarında*, bu değeri ile değiştirin **kuruluşlar**.
       - Uygulamanız destekliyorsa *herhangi bir kuruluş dizinini ve kişisel Microsoft hesapları hesaplarında*, bu değeri ile değiştirin **ortak**. İçin desteği kısıtlamak için *kişisel Microsoft hesapları yalnızca*, bu değeri ile değiştirin **tüketiciler**.
