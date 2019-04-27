---
author: PatAltimore
ms.service: active-directory-b2c
ms.topic: include
origin.date: 11/30/2018
ms.date: 04/04/2019
ms.author: v-junlch
ms.openlocfilehash: 78abb190dccd27c5bf70dfe12f978e1118601815
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60456957"
---
Hassas parola sıfırlama uygulamanızı etkinleştirmek için kullandığınız bir **parola sıfırlama** kullanıcı akışı. Kiracı genelinde parola sıfırlama seçeneği Not belirtilen [burada](../articles/active-directory-b2c/active-directory-b2c-reference-sspr.md). Bu kullanıcı akışını müşterilerin parola sıfırlama ve başarıyla tamamlandığında uygulamanın alacağı belirteçlerin içeriğini sırasında karşılaşacağı deneyimleri açıklanmaktadır.

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

Altında **Yönet**seçin **kullanıcı akışları** tıklatıp +**yeni kullanıcı akışı**.

![Yeni kullanıcı akışı seçin](./media/active-directory-b2c-create-password-reset-policy/add-b2c-new-user-flow.png)

Üzerinde **önerilen** sekmesinde **parola sıfırlama**.

Kullanıcı akışı girin **adı** uygulamanızın başvuru. Örneğin, `SSPR` girin.

Altında **kimlik sağlayıcıları**, kontrol **e-posta adresi kullanarak parola Sıfırla**.

![Ad ve kimlik sağlayıcısı olarak e-posta adresi kullanarak parola seçin Sıfırla girin](./media/active-directory-b2c-create-password-reset-policy/add-b2c-password-reset-identity-providers.png)

Altında **uygulama taleplerini**, tıklayın **daha fazla Göster** ve başarılı bir parola sıfırlama deneyiminden sonra uygulamanıza geri gönderilen yetkilendirme belirteçlerinde döndürülmesini istediğiniz talepleri seçin. Örneğin, **Kullanıcının Nesne Kimliği**’ni seçin.

**Tamam** düğmesine tıklayın.

![Bazı uygulama taleplerini seçin ve Tamam düğmesine tıklayın.](./media/active-directory-b2c-create-password-reset-policy/add-b2c-password-reset-application-claims.png)

Tıklayın **Oluştur** kullanıcı akışı eklemek için. Kullanıcı akışı olarak listelenip listelenmediğini **B2C_1_SSPR**. Ada **B2C_1_** öneki eklenir.

Tıklayın **kullanıcı akışı çalıştırma**. Tabloda belirtilen ayarlarını doğrulayın, ardından tıklayın **kullanıcı akışı çalıştırma**.

![Kullanıcı akışı seçin ve çalıştırın](./media/active-directory-b2c-create-password-reset-policy/add-b2c-sspr-run-user-flow.png)

| Ayar      | Değer  |
| ------------ | ------ |
| **Uygulama** | Contoso B2C uygulaması |
| **Yanıt URL'sini seçin** | `https://localhost:44316/` |

Yeni bir tarayıcı sekmesi açılır. Buradan, uygulamanızın parola sıfırlamaya yönelik tüketici deneyimini doğrulayabilirsiniz.

> [!NOTE]
> İlke oluşturma ve güncelleştirmelerinin etkili olması bir dakika kadar alabilir.
>

