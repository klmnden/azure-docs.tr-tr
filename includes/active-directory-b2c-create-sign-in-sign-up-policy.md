---
author: PatAltimore
ms.service: active-directory-b2c
ms.topic: include
ms.date: 11/03/2016
ms.author: patricka
ms.openlocfilehash: 3485cc96ad1d9ab8f89facf33687a7ab2be43b1e
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50133733"
---
[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

Ayarların ilkeler bölümünde **Oturum açma veya kaydolma ilkeleri**’ni seçip **+ Ekle**’ye tıklayın.

![Oturum açma veya kaydolma ilkeleri’ni seçin ve Ekle düğmesine tıklayın.](media/active-directory-b2c-create-sign-in-sign-up-policy/add-b2c-signup-signin-policy.png)

Uygulamanızın başvuracağı ilke **Adını** girin. Örneğin, `SiUpIn` girin.

**Kimlik sağlayıcıları**’nı seçin ve **E-posta ile kaydolma**’yı işaretleyin. İsteğe bağlı olarak, zaten yapılandırılmışsa sosyal kimlik sağlayıcıları öğesini de seçebilirsiniz. **Tamam** düğmesine tıklayın.

![Kimlik sağlayıcısı olarak E-posta ile kaydolma’yı seçin ve Tamam düğmesine tıklayın.](media/active-directory-b2c-create-sign-in-sign-up-policy/add-b2c-signup-signin-identity-providers.png)

**Kaydolma öznitelikleri**’ni seçin. Tüketiciden kayıt sırasında toplamak istediğiniz öznitelikleri seçin. Örneğin, **Ülke/Bölge**, **Görünen Ad** ve **Posta Kodu**’nu işaretleyin. **Tamam** düğmesine tıklayın.

![Bazı öznitelikleri seçin ve Tamam düğmesine tıklayın.](media/active-directory-b2c-create-sign-in-sign-up-policy/add-b2c-signup-signin-sign-up-attributes.png)

**Uygulama talepleri**’ni seçin. Başarılı bir kayıt veya oturum açma deneyiminden sonra uygulamanıza geri gönderilen yetkilendirme belirteçlerinde döndürülmesini istediğiniz talepleri seçin. Örneğin, **Görünen Ad**, **Kimlik Sağlayıcısı**, **Posta Kodu**, **Kullanıcı yeni** ve **Kullanıcının Nesne Kimliği**’ni işaretleyin.

![Bazı uygulama taleplerini seçin ve Tamam düğmesine tıklayın.](media/active-directory-b2c-create-sign-in-sign-up-policy/add-b2c-signup-signin-application-claims.png)

İlkeyi eklemek için **Oluştur**’a tıklayın. İlke **B2C_1_SiUpIn** olarak listelenir. Ada **B2C_1_** öneki eklenir.

**B2C_1_SiUpIn** adını seçerek ilkeyi açın. Tabloda belirtilen ayarlarını doğrulayın, ardından **Şimdi Çalıştır**’a tıklayın.

![İlkeyi seçin ve çalıştırın.](media/active-directory-b2c-create-sign-in-sign-up-policy/run-b2c-signup-signin-policy.png)

| Ayar      | Değer  |
| ------------ | ------ |
| **Uygulamalar** | Contoso B2C uygulaması |
| **Yanıt URL'sini seçin** | `https://localhost:44316/` |

Yeni bir tarayıcı sekmesi açılır. Buradan, oturum açma veya kaydolma için yapılandırılan tüketici deneyimini doğrulayabilirsiniz.

> [!NOTE]
> İlke oluşturma ve güncelleştirmelerinin etkili olması bir dakika kadar alabilir.
>