---
author: PatAltimore
ms.service: active-directory-b2c
ms.topic: include
origin.date: 11/30/2018
ms.date: 04/04/2019
ms.author: v-junlch
ms.openlocfilehash: 0d9f0a24d84bd18bdf1fac84c744cc34a7d89ab3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60456048"
---
Uygulamanızda profili düzenlemeyi etkinleştirmek istiyorsanız, kullandığınız bir **profil düzenleme** kullanıcı akışı. Bu kullanıcı akışını müşteriler profil düzenleme ve başarıyla tamamlandığında uygulamanın alacağı belirteçlerin içeriğini sırasında karşılaşacağı deneyimleri açıklanmaktadır.

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

Altında **Yönet**seçin **kullanıcı akışları** tıklatıp +**yeni kullanıcı akışı**.

![Yeni kullanıcı akışı seçin](./media/active-directory-b2c-create-profile-editing-policy/add-b2c-new-user-flow.png)

Üzerinde **önerilen** sekmesinde **profil düzenleme**.

Kullanıcı akışı girin **adı** uygulamanızın başvuru. Örneğin, `SiPe` girin.

Altında **kimlik sağlayıcıları**, kontrol **yerel hesapla oturum aç**. İsteğe bağlı olarak, zaten yapılandırılmışsa sosyal kimlik sağlayıcıları öğesini de seçebilirsiniz.

![Kimlik sağlayıcısı olarak Yerel Hesapla Oturum Aç’ı seçin ve Tamam düğmesine tıklayın](./media/active-directory-b2c-create-profile-editing-policy/add-b2c-profile-editing-identity-providers.png)

Altında **kullanıcı öznitelikleri**, tıklayın **daha fazla Göster**. İçinde **toplama özniteliği** sütun profilinde tüketici görüntüleyebilir ve düzenleyebilirsiniz öznitelikleri seçin. Örneğin, **Ülke/Bölge**, **Görünen Ad** ve **Posta Kodu**’nu işaretleyin.

İçinde **dönüş talep** sütun, başarılı bir profil düzenleme deneyimi sonra uygulamanıza geri gönderilen yetkilendirme belirteçlerinde döndürülmesini istediğiniz talepleri seçin. Örneğin, **Görünen Ad** ve **Posta Kodu**’nu seçin.

**Tamam** düğmesine tıklayın.

![Bazı uygulama taleplerini seçin ve Tamam düğmesine tıklayın.](./media/active-directory-b2c-create-profile-editing-policy/add-b2c-user-attributes.png)

Tıklayın **Oluştur** kullanıcı akışı eklemek için. Kullanıcı akışı olarak listelenip listelenmediğini **B2C_1_SiPe**. Ada **B2C_1_** öneki eklenir.

Seçin **kullanıcı akışı çalıştırma**. Tabloda belirtilen ayarlarını doğrulayın, ardından tıklayın **kullanıcı akışı çalıştırma**.

![Kullanıcı akışı seçin ve çalıştırın](./media/active-directory-b2c-create-profile-editing-policy/add-b2c-profile-editing-run-user-flow.png)

| Ayar      | Değer  |
| ------------ | ------ |
| **Uygulama** | Contoso B2C uygulaması |
| **Yanıt URL'si** | `https://localhost:44316/` |

Yeni bir tarayıcı sekmesi açılır. Buradan, profil düzenleme için yapılandırılan tüketici deneyimini doğrulayabilirsiniz.

> [!NOTE]
> Bu bir dakika kullanıcı akışı oluşturma ve güncelleştirmelerinin etkili olması için kapladığı.
>

