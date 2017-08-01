---
title: "Azure Active Directory B2C: Uygulama kaydı | Microsoft Belgeleri"
description: "Uygulamanızı Azure Active Directory B2C'ye kaydetme"
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: 20e92275-b25d-45dd-9090-181a60c99f69
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 6/13/2017
ms.author: parakhj
ms.translationtype: Human Translation
ms.sourcegitcommit: 6dbb88577733d5ec0dc17acf7243b2ba7b829b38
ms.openlocfilehash: 3499ff57e650c70679dfa018eec5dbe1a6173a33
ms.contentlocale: tr-tr
ms.lasthandoff: 07/04/2017



---
# <a name="azure-active-directory-b2c-register-your-application"></a>Azure Active Directory B2C: Uygulamanızı kaydetme

> [!IMPORTANT]
> Azure portalında Azure AD B2C dikey penceresinden oluşturulan uygulamaların aynı konumdan yönetilmesi gerekir. B2C uygulamalarını PowerShell veya başka bir portal kullanarak düzenlerseniz bu uygulamalar desteklenmez duruma gelir ve Azure AD B2C ile çalışmaz. [Aşağıda](#faulted-apps) daha fazla bilgi bulabilirsiniz.
>

## <a name="prerequisite"></a>Önkoşul

Tüketicinin kaydolmasını ve oturum açmasını kabul eden bir uygulama oluşturmak için öncelikle uygulamayı Azure Active Directory B2C kiracısına kaydetmeniz gerekir. [Azure AD B2C kiracısı oluşturma](active-directory-b2c-get-started.md) makalesinde ana hatlarıyla belirtilen adımları izleyerek kendi kiracınızı edinin. Söz konusu makaledeki tüm adımları izledikten sonra B2C özellikleri dikey penceresi Başlangıç panonuza sabitlenir.

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="navigate-to-the-b2c-features-blade"></a>B2C özellikleri dikey penceresine gitme

B2C özellikleri dikey penceresini Başlangıç panonuza sabitlediyseniz dikey pencereyi, B2C kiracısının Genel Yöneticisi olarak [Azure portalında](https://portal.azure.com/) oturum açar açmaz görürsünüz.

Ayrıca dikey pencereye, [Azure portalındaki](https://portal.azure.com/) **Diğer hizmetler**’e tıklayarak ve ardından sol gezinti bölmesinde **Azure AD B2C** araması yaparak erişebilirsiniz.

> [!IMPORTANT]
> B2C özellikleri dikey penceresine erişebilmek için B2C kiracısının Genel Yöneticisi olmanız gerekir. Başka bir kiracının Genel Yöneticisi veya başka bir kiracıdan gelen bir kullanıcı, dikey pencereye erişemez.  Azure portalının sağ üst köşesindeki kiracı değiştiriciyi kullanarak B2C kiracınıza geçiş yapabilirsiniz.
>
>

## <a name="register-a-web-application"></a>Web uygulaması kaydetme

1. Azure portalındaki B2C özellikleri dikey penceresinde **Applications (Uygulamalar)** seçeneğine tıklayın.
1. Dikey pencerenin en üstündeki **+Add (+Ekle)** seçeneğine tıklayın.
1. Uygulamanızı tüketicilere tanımlayacak bir **Ad** girin. Örneğin, "Contoso B2C uygulaması"na girebilirsiniz.
1. **Web uygulamasını / web API’sini dahil et** anahtarını **Evet**’e getirin.
1. Azure AD B2C'nin, uygulamanız tarafından istenen belirteçleri döndürdüğü uç noktalar olan **Yanıt URL'leri** için [doğru](#limitations) bir değer girin. Örneğin, `https://localhost:44316/` girin.
1. Uygulamanızı kaydetmek için **Create (Oluştur)** seçeneğine tıklayın.
1. Oluşturduğunuz uygulamaya tıklayın ve daha sonra kodunuzda kullanacağınız genel benzersiz **Uygulama İstemci Kimliğini** kopyalayın.
1. Web uygulamanız güvenliği Azure AD B2C tarafından sağlanan bir web API’si de çağıracaksa, şunları yapmanız gerekir:
    1. **Anahtarlar** dikey penceresine gidip **Anahtar Oluştur** düğmesine tıklayarak bir **Uygulama Gizli Dizisi** oluşturun.
    1. **API Erişimi**’ne ve **Ekle**’ye tıklayıp web API’si ve kapsamlarınızı (izinler) seçme.

> [!NOTE]
> **Uygulama Gizli Anahtarı** önemli bir güvenlik kimlik bilgisidir ve güvenliği uygun şekilde sağlanmalıdır.
>

## <a name="register-a-web-api"></a>Web API’si kaydetme

1. Azure portalındaki B2C özellikleri dikey penceresinde **Applications (Uygulamalar)** seçeneğine tıklayın.
1. Dikey pencerenin en üstündeki **+Add (+Ekle)** seçeneğine tıklayın.
1. Uygulamanızı tüketicilere tanımlayacak bir **Ad** girin. Örneğin, "Contoso B2C api" girebilirsiniz.
1. **Web uygulamasını / web API’sini dahil et** anahtarını **Evet**’e getirin.
1. Azure AD B2C'nin, uygulamanız tarafından istenen belirteçleri döndürdüğü uç noktalar olan **Yanıt URL'leri** için [doğru bir](#choosing-a-web-app/api-reply-url) değer girin. Örneğin, `https://localhost:44316/` girin.
1. Bir **Uygulama Kimliği URI'si** girin. Bu değer, web API’niz için kullanılan tanımlayıcıdır. Örneğin, 'notlar' ifadesini girin. Altında tam tanımlayıcı URI’si oluşturulur.
1. Uygulamanızı kaydetmek için **Create (Oluştur)** seçeneğine tıklayın.
1. Oluşturduğunuz uygulamaya tıklayın ve daha sonra kodunuzda kullanacağınız genel benzersiz **Uygulama İstemci Kimliğini** kopyalayın.
1. **Yayımlanan kapsamlar**’a tıklayın. Burada diğer uygulamalara verilebilecek izinleri (kapsamları) tanımlarsınız.
1. Gerektiğinde daha fazla kapsam ekleyin. Varsayılan olarak, "user_impersonation" kapsamı tanımlanır. Bunun yapılması, diğer uygulamaların oturum açmış kullanıcı adına bu api’de oturum açmasına olanak tanır. İsterseniz bu seçeneği kaldırabilirsiniz.
1. **Kaydet** düğmesine tıklayın.

## <a name="register-a-mobilenative-application"></a>Mobil/yerel bir uygulamayı kaydetme

1. Azure portalındaki B2C özellikleri dikey penceresinde **Applications (Uygulamalar)** seçeneğine tıklayın.
1. Dikey pencerenin en üstündeki **+Add (+Ekle)** seçeneğine tıklayın.
1. Uygulamanızı tüketicilere tanımlayacak bir **Ad** girin. Örneğin, "Contoso B2C uygulaması"na girebilirsiniz.
1. **Yerel istemciyi dahil et** anahtarını **Evet**’e getirin.
1. Özel şema ile bir **Yeniden yönlendirme URI’si** girin. Örneğin, com.onmicrosoft.contoso.appname://redirect/path. [İyi bir yeniden yönlendirme URI’si](#choosing-a-native-application-redirect-uri) seçtiğinizden emin olun ve alt çizgi gibi özel karakterler kullanmayın.
1. Uygulamanızı kaydetmek için **Kaydet** seçeneğine tıklayın.
1. Oluşturduğunuz uygulamaya tıklayın ve daha sonra kodunuzda kullanacağınız genel benzersiz **Uygulama İstemci Kimliğini** kopyalayın.
1. Yerel uygulamanız güvenliği Azure AD B2C tarafından sağlanan bir web API’si de çağıracaksa şunları yapmanız gerekir:
    1. **Anahtarlar** dikey penceresine gidip **Anahtar Oluştur** düğmesine tıklayarak bir **Uygulama Gizli Dizisi** oluşturun.
    1. **API Erişimi**’ne ve **Ekle**’ye tıklayıp web API’si ve kapsamlarınızı (izinler) seçme.

> [!NOTE]
> **Uygulama Gizli Anahtarı** önemli bir güvenlik kimlik bilgisidir ve güvenliği uygun şekilde sağlanmalıdır.
>

## <a name="limitations"></a>Sınırlamalar

### <a name="choosing-a-web-appapi-reply-url"></a>Bir web uygulaması/api yanıt URL'si seçme

Şu anda Azure AD B2C’ye kayıtlı uygulamalar sınırlı sayıda yanıt URL'si değeri ile kısıtlıdır. Web uygulamaları ve hizmetlerine yönelik yanıt URL’si, `https` şemasıyla başlamalı ve tüm yanıt URL’si değerleri tek bir DNS etki alanını paylaşmalıdır. Örneğin, şu yanıt URL'lerinden birine sahip bir web uygulamasını kaydedemezsiniz:

`https://login-east.contoso.com`

`https://login-west.contoso.com`

Kayıt sistemi mevcut yanıt URL'sinin tam DNS adını, eklemekte olduğunuz yanıt URL'sinin DNS adı ile karşılaştırır. Aşağıdaki koşullardan biri geçerli olduğunda DNS adı ekleme isteği başarısız olur:

* Yeni yanıt URL'sinin tam DNS adı, mevcut yanıt URL'sinin DNS adı ile eşleşmiyorsa.
* Yeni yanıt URL'sinin tam DNS adı, mevcut yanıt URL'sinin alt etki alanı değilse.

Örneğin, uygulamanın yanıt URL'si şu ise:

`https://login.contoso.com`

Aşağıdaki gibi ekleme yapabilirsiniz:

`https://login.contoso.com/new`

Bu durumda, DNS adı tam olarak eşleşir. Ya da şunu yapabilirsiniz:

`https://new.login.contoso.com`

Bu durumda, login.contoso.com DNS alt etki alanına başvurursunuz. Yanıt URL’leri login-east.contoso.com ve login-west.contoso.com olan bir uygulamanızın olmasını istiyorsanız, bu yanıt URL’lerini şu sırayla eklemeniz gerekir:

`https://contoso.com`

`https://login-east.contoso.com`

`https://login-west.contoso.com`

Sonraki iki yanıt URL’si, ilk yanıt URL'si olan contoso.com’un alt etki alanları olduğu için bunları ekleyebilirsiniz.

### <a name="choosing-a-native-application-redirect-uri"></a>Yerel uygulama yeniden yönlendirme URI'si seçme

Mobil/yerel uygulamalar için bir yeniden yönlendirme URI’si seçerken dikkat edilmesi gereken iki önemli nokta şunlardır:

* **Benzersiz**: Yeniden yönlendirme URI’si şeması her uygulama için benzersiz olmalıdır. Örneğimizde (com.onmicrosoft.contoso.appname://redirect/path), şema olarak com.onmicrosoft.contoso.appname kullanılır. Bu örneği izlemeniz önerilir. İki uygulama aynı şemayı paylaşıyorsa, kullanıcı bir “bir uygulama seçin” iletişim kutusu görür. Kullanıcı yanlış seçim yaparsa, oturum açma başarısız olur.
* **Tam**: Yeniden yönlendirme URI’sinin bir şeması ve yolu olmalıdır. Yol, etki alanından sonra en az bir eğik çizgi içermelidir (örneğin, //contoso/ çalışırken //contoso başarısız olur).

Yeniden yönlendirme URI'sinde alt çizgi gibi özel karakterler olmadığından emin olun.

### <a name="faulted-apps"></a>Hatalı uygulamalar

B2C uygulamaları şu durumlarda DÜZENLENMEMELİDİR:

* [Klasik Azure portalı](https://manage.windowsazure.com/) ve [Uygulama Kayıt Portalı](https://apps.dev.microsoft.com/) gibi diğer uygulama yanıt portallarında.
* Grafik API'si veya PowerShell kullanılarak

B2C uygulamasını yukarıda açıklanan şekilde düzenleyip Azure portalının Azure AD B2C özellikleri dikey penceresinde yeniden düzenlemeye çalışırsanız, uygulama hatalı bir hale gelir ve Azure AD B2C ile artık kullanılamaz. Uygulamayı silip yeniden oluşturmanız gerekir.

Uygulamayı silmek için [Uygulama Kayıt Portalı](https://apps.dev.microsoft.com/)’na gidin ve uygulamayı silin. Uygulamanın görünür olması için uygulamanın sahibi olmanız (ve yalnızca kiracının yöneticisi olmamanız) gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Azure AD B2C'ye kayıtlı bir uygulamaya sahip olduğunuza göre başlamak için [hızlı başlangıç öğreticilerimizden](active-directory-b2c-overview.md#get-started) birini tamamlayabilirsiniz.

