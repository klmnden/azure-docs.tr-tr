---
author: PatAltimore
ms.service: active-directory-b2c
ms.topic: include
origin.date: 11/03/2016
ms.date: 04/04/2019
ms.author: v-junlch
ms.openlocfilehash: 4d5c8373f2a8f02ccfbe51cb970ab08744950e7e
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62128222"
---
[!INCLUDE [active-directory-b2c-portal-add-application](active-directory-b2c-portal-add-application.md)]

Web uygulamanızı kaydetmek için tabloda belirtilen ayarları kullanın.

![Yeni web uygulaması için örnek kayıt ayarları](./media/active-directory-b2c-register-web-app/b2c-new-app-settings.png)

| Ayar      | Örnek değer  | Açıklama                                        |
| ------------ | ------- | -------------------------------------------------- |
| **Ad** | Contoso B2C uygulaması | Uygulamanızı tüketicilere tanımlayacak bir **Ad** girin. | 
| **Web uygulamasını / web API'sini dahil etme** | Evet | Web uygulaması için **Evet**’i seçin. |
| **Örtük akışa izin verme** | Evet | Uygulamanız **OpenID Connect oturum açma bilgilerini** kullanıyorsa [Evet](../articles/active-directory-b2c/active-directory-b2c-reference-oidc.md)’i seçin. |
| **Yanıt URL'si** | `https://localhost:44316` | Yanıt URL'leri, Azure AD B2C'nin, uygulamanız tarafından istenen belirteçleri getirdiği uç noktalardır. [Uygun bir](../articles/active-directory-b2c/active-directory-b2c-app-registration.md#choosing-a-web-app-or-api-reply-url) **Yanıt URL’si** girin. Bu örnekte, uygulamanız yereldir ve bağlantı noktası 44316’yı dinlemektedir. |

Uygulamanızı kaydetmek için **Create (Oluştur)** seçeneğine tıklayın.

Yeni kaydettiğiniz uygulamanız B2C kiracısı için uygulamalar listesinde görüntülenir. Listeden web uygulamanızı seçin. Web uygulamasının özellik bölmesi görüntülenir.

![Web uygulaması özellikleri](./media/active-directory-b2c-register-web-app/b2c-web-app-properties.png)

Genel benzersiz **Uygulama İstemci Kimliğini** not alın. Bu kimliği uygulamanızın kodlarında kullanırsınız.

