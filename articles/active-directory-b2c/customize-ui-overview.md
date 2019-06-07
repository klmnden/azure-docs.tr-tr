---
title: Azure Active Directory B2C'de kullanıcı arabirimi özelleştirme hakkında daha fazla | Microsoft Docs
description: Azure Active Directory B2C kullanan uygulamalarınız için kullanıcı arabirimini özelleştirme hakkında bilgi edinin.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 6c9109cf4d6d67d3d8001a9de1d54e24622a9286
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66511186"
---
# <a name="about-user-interface-customization-in-azure-active-directory-b2c"></a>Azure Active Directory B2C hakkında kullanıcı arabirimi özelleştirme

Özelliği, marka ve Azure Active Directory (Azure AD) B2C hizmet veren uygulamalarınız için kullanıcı arabirimini (UI) özelleştirmek, müşteriniz için sorunsuz bir deneyim sağlamak için önemlidir. Bu deneyimler, kaydolma, oturum açma, profil düzenleme ve parola sıfırlama içerir. Bu makalede, uygulamanızın kullanıcı arabirimini özelleştirme yardımcı olacak bilgiler sağlar.

Bu deneyimler geldiğinde gereksinimlerinize bağlı olarak, uygulamanızın kullanıcı arabirimini farklı şekilde özelleştirin. Örneğin:

- Kullanıyorsanız [kullanıcı akışları](active-directory-b2c-reference-policies.md) kaydolma veya oturum açma parolasını sıfırlama veya profil düzenleme deneyimlerini uygulamanızdaki sağlamak için kullandığınız [Azure portalı kullanıcı arabirimini özelleştirme](tutorial-customize-ui.md).
- V2 kullanıcı akışı kullanıyorsanız, kullanabileceğiniz bir [sayfa düzeni şablonu](#page-layout-templates) kullanıcı akışı sayfalarınızın daha fazla özelleştirme olmadan görünümünü değiştirmek için. Örneğin, kullanıcı akışınızı tüm sayfalar için Okyanus mavi veya maskeleme görüntüsü gri bir tema uygulayabilirsiniz.
- Oturum açma yalnızca sağladığınızı, eşlik eden parolasını sıfırlama sayfası ve doğrulama e-postalar, için kullanılan özelleştirme adımların aynısını kullanırsanız bir [Azure AD oturum açma sayfasının](../active-directory/fundamentals/customize-branding.md).
- Müşterilerin kendi profili oturum açmadan önce düzenlemeye çalışırsanız, Azure AD oturum açma sayfasının özelleştirmek için kullanılan aynı adımları kullanarak özelleştirme bir sayfaya yönlendirilirsiniz.
- Kullanıyorsanız [özel ilkeler](active-directory-b2c-overview-custom.md) kaydolma veya oturum açma, parola sıfırlama veya profil düzenleme, uygulamanızda kullanmak [kullanıcı arabirimini özelleştirmek için ilke dosyaları](active-directory-b2c-ui-customization-custom.md).
- Bir müşterinin karar temel alınarak dinamik içerik sağlamanız gerekiyorsa, kullandığınız [değiştirebilirsiniz özel ilkeler, içerik sayfasında](active-directory-b2c-ui-customization-custom-dynamic.md) bağlı bir sorgu dizesi içinde gönderilen bir parametre olarak. Örneğin, web veya mobil uygulama geçirdiğiniz parametre dayalı bir Azure AD B2C kaydolma veya oturum açma sayfasında arka plan resmi değiştirir.
- Azure AD B2C'de JavaScript istemci tarafı kod etkinleştirebilirsiniz [kullanıcı akışları](user-flow-javascript-overview.md) veya [özel ilkeler](page-contract.md).

Azure AD B2C kod müşterinizin tarayıcıda çalışan ve modern bir yaklaşımı adlı kullanır [çıkış noktaları arası kaynak paylaşımı (CORS)](https://www.w3.org/TR/cors/). Çalışma zamanında, bir kullanıcı akışı veya ilkede belirttiğiniz URL'den içerik yüklendi. Farklı sayfaları için farklı URL'ler belirtmeniz. İçerik, URL'den yüklendikten sonra Azure AD B2C'den eklenen ve ardından müşterinize görüntülenen bir HTML parçasını ile birleştirilir.

Başlamadan önce kullanıcı arabirimini özelleştirmek için kendi HTML ve CSS dosyaları kullanırken aşağıdaki yönergeleri gözden geçirin:

- Azure AD B2C sayfalarınıza HTML içeriğini birleştirir. Yoksa, kopyalama ve Azure AD B2C sağlar varsayılan içerik değiştirmeyi deneyin. HTML içeriğinizi sıfırdan oluşturmak ve varsayılan içerik referans olarak kullanmak en iyisidir.
- JavaScript, artık özel içeriğinize eklenebilir.
- Desteklenen bir tarayıcı sürümleri şunlardır: 
    - Internet Explorer 11, 10 ve Microsoft Edge
    - Internet Explorer 9 ve 8 için sınırlı destek
    - Google Chrome 42.0 ve üzeri
    - Mozilla Firefox 38.0 ve üzeri
- Azure AD B2C eklenen HTML tarafından oluşturulan sonrası işlemleri ile engellemesi nedeniyle, form etiketleri, HTML biçiminde eklemediğinizden emin olun.

## <a name="page-layout-templates"></a>Sayfa düzeni şablonları

V2 kullanıcı akışları için varsayılan sayfalarınızı daha iyi bir görünüm sunar ve kendi özelleştirme için iyi bir temel görevi gören bir önceden tasarlanmış şablonu seçebilirsiniz.

Soldaki menüde altında **Özelleştir**seçin **sayfa düzenleri**. Ardından **şablonu (Önizleme)** .

![Bir sayfa Düzen şablonunu seçin](media/customize-ui-overview/template.png)

Listeden bir şablon seçin. Örneğin, **Okyanusu mavi** şablon kullanıcı akışı sayfalarınıza aşağıdaki düzeni uygular:

![Okyanusu mavi şablonu](media/customize-ui-overview/ocean-blue.png)

Bir şablon seçin, seçilen düzeni, kullanıcı akışınızı tüm sayfalara uygulanır ve her sayfanın URI'sini görülebilir **özel sayfa URI'si** alan.

## <a name="where-do-i-store-ui-content"></a>Kullanıcı Arabirimi içeriği nereye depoluyor?

Kullanıcı arabirimini özelleştirmek için kendi HTML ve CSS dosyaları kullanırken, içeriği her yerde ve gibi kullanıcı Arabirimi barındırabilir [Azure Blob Depolama](../storage/blobs/storage-blobs-introduction.md), web sunucuları, CDN, AWS S3 veya dosya paylaşım sistemi. Önemli barındırdığınız CORS'yi ile genel kullanıma açık bir HTTPS uç noktasının içerik noktasıdır. İçeriğinizi belirttiğinizde, mutlak bir URL kullanmanız gerekir.

## <a name="how-do-i-get-started"></a>Nasıl kullanmaya başlayabilirim?

Kullanıcı arabirimini özelleştirmek için aşağıdakileri yapın:

- İyi biçimlendirilmiş HTML ile boş bir içerik oluşturma `<div id="api"></div>` öğesi herhangi bir yerde bulunan `<body>`. Bu öğe işaretleri Azure AD B2C içeriği burada eklenir. Aşağıdaki örnek, en az bir sayfa görüntülenir:

    ```html
    <!DOCTYPE html>
    <html>
      <head>
        <title>!Add your title here!</title>
        <link rel="stylesheet" href="https://mystore1.blob.core.windows.net/b2c/style.css">
      </head>
      <body>
        <h1>My B2C Application</h1>
        <div id="api"></div>   <!-- Leave this element empty because Azure AD B2C will insert content here. -->
      </body>
    </html>
    ```

- İçeriğinizi bir HTTPS uç noktası ana bilgisayar (izin verilen CORS ile). Her ikisini de almak ve CORS yapılandırırken seçenekleri istek yöntemleri etkinleştirilmesi gerekir.
- CSS stil sayfanıza ekleyen Azure AD B2C kullanıcı Arabirimi öğeleri için kullanın. Aşağıdaki örnek, ayrıca kaydolma eklenen HTML öğeleri için ayarları içeren basit bir CSS dosyası gösterir:

    ```css 
    h1 {
      color: blue;
      text-align: center;
    }
    .intro h2 {
      text-align: center; 
    }
    .entry {
      width: 400px ;
      margin-left: auto ;
      margin-right: auto ;
    }
    .divider h2 {
      text-align: center; 
    }
    .create {
      width: 400px ;
      margin-left: auto ;
      margin-right: auto ;
    }
    ```

- Oluşturabilir veya oluşturduğunuz içeriği kullanmak için bir ilkeyi düzenleyebilirsiniz.

Aşağıdaki tabloda Azure AD B2C birleştirir HTML parçaları `<div id="api"></div>` öğesi, içeriğinizi bulunur.

| Eklenen sayfa | HTML açıklaması |
| ------------- | ------------------- |
| Kimlik sağlayıcısı seçim | Kaydolma veya oturum açma sırasında müşteri seçebilirsiniz kimlik sağlayıcıları için düğmelerin listesini içerir. Bu düğmeler, Facebook, Google veya yerel hesaplar (e-posta adresi veya kullanıcı adına göre) gibi sosyal kimlik sağlayıcılarını içerir. |
| Yerel hesap kaydolma | Bir e-posta adresi veya kullanıcı adına göre bir form için yerel hesap kaydolma içerir. Form, metin girişi kutusunu, parola girişi kutusu, radyo düğmesi, tekli seçim açılır kutuları ve çoklu seçim onay kutuları gibi farklı giriş denetimleri içerebilir. |
| Sosyal hesap kaydolma | Hesabınız bir Facebook veya Google gibi sosyal kimlik sağlayıcısı kullanarak kaydolma olduğunda ortaya çıkabilir. Ek bilgi kaydolma formu kullanarak müşteriden toplanması gereken olduğunda kullanılır. |
| Kaydolma veya oturum açma birleşik | Hem kaydolma ve oturum açma, Facebook, Google veya yerel hesaplar gibi sosyal kimlik sağlayıcıları kullanan müşteriler işler. |
| Multi-factor authentication | Müşteriler, kaydolma veya oturum açma sırasında (metin ya da ses kullanarak) kendi telefon numaralarını doğrulayabilirsiniz. |
| Hata | Hata bilgileri için müşteri sağlar. |


## <a name="how-do-i-localize-content"></a>İçeriği nasıl yerelleştiriyor musunuz?

Etkinleştirerek, HTML içeriği yerelleştirmek [dil özelleştirme](active-directory-b2c-reference-language-customization.md) Azure AD B2C kiracınızdaki. Bu özellik çalışabilmelerini Open ID Connect parametresi iletmek için Azure AD B2C `ui-locales` uç noktanıza. İçerik sunucunuzu Bu parametre, dile özgü HTML sayfalarını sağlamak için kullanabilirsiniz.

İçeriği kullanılan yerel ayarları temel alarak farklı yerlerde çekilebilir. CORS özellikli uç noktanızı içinde belirli diller için ana içerik için bir klasör yapısı ayarlayın. Joker karakter değeri {kültür: RFC5646} kullanıyorsanız, doğru olanı ararız. Örneğin, URI gibi görünebilir, özel sayfa `https://contoso.blob.core.windows.net/{Culture:RFC5646}/myHTML/unified.html`. İçeriği çekerek Fransızca sayfasında yükleyebilirsiniz. `https://contoso.blob.core.windows.net/fr/myHTML/unified.html`

## <a name="examples"></a>Örnekler

Özelleştirme örnekler için indirebilir ve bunları gözden [örnek şablon dosyaları](https://github.com/azureadquickstarts/b2c-azureblobstorage-client/archive/master.zip).

## <a name="next-steps"></a>Sonraki adımlar

- Kullanıcı akışları kullanıyorsanız öğreticisiyle kullanıcı Arabirimi özelleştirme başlayabilirsiniz: [Uygulamalarınızı Azure Active Directory B2C, kullanıcı arabirimini özelleştirme](tutorial-customize-ui.md).
- Özel ilkeleri kullanıyorsanız, makale ile kullanıcı arabirimini özelleştirme başlayabilirsiniz: [Azure Active Directory B2C'de bir özel ilke kullanarak uygulamanızın kullanıcı arabirimini özelleştirme](active-directory-b2c-ui-customization-custom.md).

