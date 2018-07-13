---
title: include dosyası
description: include dosyası
services: active-directory-b2c
author: davidmu1
ms.service: active-directory-b2c
ms.topic: include
ms.date: 04/09/2018
ms.author: davidmu
ms.custom: include file
ms.openlocfilehash: 8363d023e89c77aabc0d123f19264c9a0758a656
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38740484"
---
[!INCLUDE [active-directory-b2c-portal-add-application](active-directory-b2c-portal-add-application.md)]

Mobil veya yerel uygulamanızı kaydetmek için tabloda belirtilen ayarları kullanın.

![Yeni mobil veya yerel uygulama için örnek kayıt ayarları](./media/active-directory-b2c-register-mobile-native-app/b2c-new-mobile-native-app-settings.png)

| Ayar      | Örnek değer  | Açıklama                                        |
| ------------ | ------- | -------------------------------------------------- |
| **Ad** | Contoso B2C uygulaması | Uygulamanızı tüketicilere tanımlayacak bir **Ad** girin. |
| **Yerel istemci** | Evet | Mobil veya yerel bir uygulama için **Evet**’i seçin. |
| **Özel yeniden yönlendirme URI'si** | `com.onmicrosoft.contoso.appname://redirect/path` | Özel bir düzen ile yeniden yönlendirme URI’si girin. [İyi bir yeniden yönlendirme URI’si](../articles/active-directory-b2c/active-directory-b2c-app-registration.md) seçtiğinizden emin olun ve alt çizgi gibi özel karakterler kullanmayın. |

Uygulamanızı kaydetmek için **Create (Oluştur)** seçeneğine tıklayın.

Yeni kaydettiğiniz uygulamanız B2C kiracısı için uygulamalar listesinde görüntülenir. Listeden mobil veya yerel uygulamanızı seçin. Uygulamanın özellik bölmesi görüntülenir.

![Uygulama özellikleri](./media/active-directory-b2c-register-mobile-native-app/b2c-mobile-native-app-properties.png)

Genel benzersiz **Uygulama İstemci Kimliğini** not alın. Bu kimliği uygulamanızın kodlarında kullanırsınız.

Yerel uygulamanız Azure AD B2C tarafından güvence altına alınmış bir web API'sini çağırıyorsa, aşağıdaki adımları gerçekleştirin:
   1. **Anahtarlar** dikey penceresine gidip **Anahtar Oluştur** düğmesine tıklayarak bir uygulama gizli dizisi oluşturun. **Uygulama anahtarı** değerini not edin. Bu değeri, uygulamanızın kodunda uygulama gizli dizisi olarak kullanırsınız.
   2. **API Erişimi**’ne ve **Ekle**’ye tıklayıp web API’si ve kapsamlarınızı (izinler) seçin.

> [!NOTE]
> **Uygulama Gizli Anahtarı** önemli bir güvenlik kimlik bilgisidir ve güvenliği uygun şekilde sağlanmalıdır.
> 
