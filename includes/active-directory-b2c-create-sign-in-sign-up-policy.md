---
author: PatAltimore
ms.service: active-directory-b2c
ms.topic: include
origin.date: 11/30/2018
ms.date: 04/04/2019
ms.author: v-junlch
ms.openlocfilehash: f23d2b02bc2a23c5333a48a50532c03f3aa6a031
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60455663"
---
[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

Altında **Yönet**seçin **kullanıcı akışları** tıklatıp +**yeni kullanıcı akışı**.

![Yeni kullanıcı akışı seçin](./media/active-directory-b2c-create-sign-in-sign-up-policy/add-b2c-signup-signin-user-flow.png)

Üzerinde **önerilen** sekmesinde **oturum yukarı ve oturum açma**.

![Oturum seçin ve kullanıcı flow'da oturum açın](./media/active-directory-b2c-create-sign-in-sign-up-policy/add-b2c-signup-signin-user-flow-type.png)

Kullanıcı akışı girin **adı** uygulamanızın başvuru. Örneğin, `SiUpIn` girin.

Altında **kimlik sağlayıcıları** ve **e-posta kaydolma**. İsteğe bağlı olarak, zaten yapılandırılmışsa sosyal kimlik sağlayıcıları öğesini de seçebilirsiniz.

Altında **çok faktörlü kimlik doğrulaması**, seçin ya da **etkin** veya **devre dışı bırakılmış**.

![Bir ad girin ve kimlik sağlayıcısı olarak e-posta kaydolma seçin](./media/active-directory-b2c-create-sign-in-sign-up-policy/add-b2c-signup-signin-name-identity-providers.png)

Altında **kullanıcı öznitelikleri ve talepler**seçin **daha fazla Göster** öznitelikleri ve talepler seçim yapabileceğiniz tam listesini görmek için.

İçinde **toplama özniteliği** sütun, tüketiciden kayıt sırasında toplamak istediğiniz öznitelikleri seçin. Örneğin, **Ülke/Bölge**, **Görünen Ad** ve **Posta Kodu**’nu işaretleyin.

İçinde **dönüş talep** sütun, başarılı bir kaydolma veya oturum açma deneyiminden sonra uygulamanıza geri gönderilen yetkilendirme belirteçlerinde döndürülmesini istediğiniz talepleri seçin. Örneğin, **Görünen Ad**, **Kimlik Sağlayıcısı**, **Posta Kodu**, **Kullanıcı yeni** ve **Kullanıcının Nesne Kimliği**’ni işaretleyin.

**Tamam** düğmesine tıklayın.

![Bazı kullanıcı öznitelikleri ve talepler seçin ve Tamam düğmesine tıklayın.](./media/active-directory-b2c-create-sign-in-sign-up-policy/add-b2c-signup-signin-sign-up-all-attributes.png)

Tıklayın **Oluştur** kullanıcı akışı eklemek için. Kullanıcı akışı olarak listelenip listelenmediğini **b2c_1_siupın**. Ada **B2C_1_** öneki eklenir.

Seçin **kullanıcı akışı çalıştırma**. Tabloda belirtilen ayarlarını doğrulayın, ardından tıklayın **kullanıcı akışı çalıştırma**.

![Kullanıcı akışı Çalıştır'ı seçin](./media/active-directory-b2c-create-sign-in-sign-up-policy/run-user-flow-b2c-signup-signin.png)

| Ayar      | Değer  |
| ------------ | ------ |
| **Uygulama** | Contoso B2C uygulaması |
| **Yanıt URL'si** | `https://localhost:44316/` |

Yeni bir tarayıcı sekmesi açılır. Buradan, oturum açma veya kaydolma için yapılandırılan tüketici deneyimini doğrulayabilirsiniz.

> [!NOTE]
> Bu bir dakika kullanıcı akışı oluşturma ve güncelleştirmelerinin etkili olması için kapladığı.
>

