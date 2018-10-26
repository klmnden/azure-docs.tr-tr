---
author: PatAltimore
ms.service: active-directory-b2c
ms.topic: include
ms.date: 11/03/2016
ms.author: patricka
ms.openlocfilehash: 511b05e6cae769a5b39ae81a3e67efd05d374511
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50134280"
---
Yalnızca etkinleştirmek kaydolma uygulama istiyorsanız, kullandığınız bir **kaydolma** ilkesi. Bu ilke, kayıt sırasında müşteriler üzerinden geçerek deneyimleri açıklar ve üzerinde başarılı kaydolma işlemlerinde uygulamanın aldığı belirteçlerin içeriğini.

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

**Kaydolma ilkeleri**’ne tıklayın.

Dikey pencerenin en üstündeki **+Add (+Ekle)** seçeneğine tıklayın.

**Ad**, uygulamanız tarafından kullanılan kaydolma ilke adını belirler. Örneğin, **SiUp** adını girin.

**Kimlik sağlayıcıları**’na tıklayın ve **E-posta ile kaydolma**’yı seçin. İsteğe bağlı olarak, zaten yapılandırılmışsa sosyal kimlik sağlayıcıları öğesini de seçebilirsiniz. **Tamam** düğmesine tıklayın.

**Kaydolma öznitelikleri**’ne tıklayın. Burada, tüketiciden kayıt sırasında toplamak istediğiniz öznitelikleri seçin. Örneğin, **Ülke/Bölge**, **Görünen Ad** ve **Posta Kodu**’nu seçin. **Tamam** düğmesine tıklayın.

**Uygulama talepleri**’ne tıklayın. Burada, başarılı bir kaydolma deneyiminden sonra uygulamanıza geri gönderilen belirteçlerde döndürülmesini istediğiniz talepleri seçin. Örneğin, **Görünen Ad**, **Kimlik Sağlayıcısı**, **Posta Kodu**, **Kullanıcı yeni** ve **Kullanıcının Nesne Kimliği**’ni işaretleyin.

**Oluştur**’a tıklayın. Oluşturduğunuz ilke **Kaydolma ilkeleri** dikey penceresinde **B2C_1_SiUp** olarak görünür (**B2C\_1\_**  parçası otomatik olarak eklenir).

**B2C_1_SiUp** adına tıklayarak ilkeyi açın.

**Uygulamalar** açılır menüsünde **Contoso B2C uygulamasını** ve **Yanıt URL'si / Yeniden Yönlendirme URI'si** altında `https://localhost:44321/` seçeneğini belirleyin.

**Şimdi çalıştır**’a tıklayın. Yeni bir tarayıcı sekmesi açılır. Buradan, uygulamanız için kaydolmaya yönelik tüketici deneyimi üzerinde işlem yapabilirsiniz.

> [!NOTE]
> İlke oluşturma ve güncelleştirmelerinin etkili olması bir dakika kadar alabilir.
>