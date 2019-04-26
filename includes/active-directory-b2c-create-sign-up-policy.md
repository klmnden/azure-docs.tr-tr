---
author: PatAltimore
ms.service: active-directory-b2c
ms.topic: include
origin.date: 11/30/2018
ms.date: 04/04/2019
ms.author: v-junlch
ms.openlocfilehash: 17c0213d63879687e9c6d5f8dca06b9113c44af8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60456909"
---
Yalnızca etkinleştirmek kaydolma uygulama istiyorsanız, kullandığınız bir **kaydolma** kullanıcı akışı. Bu kullanıcı akışını, kayıt sırasında müşteriler üzerinden geçerek deneyimleri açıklar ve üzerinde başarılı kaydolma işlemlerinde uygulamanın aldığı belirteçlerin içeriğini.

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

Altında **Yönet**seçin **kullanıcı akışları**.

Tıklayın +**yeni kullanıcı akışı** dikey penceresinin üstünde.

Altında **kullanıcı akış türü seç**seçin **tüm**ve ardından sürümünü seçin **kaydolun** kullanmak istiyorsunuz.

**Adı** uygulamanız tarafından kullanılan kaydolma userjourney adı belirler. Örneğin, **SiUp** adını girin.

Altında **kimlik sağlayıcıları**seçin **e-posta kaydolma**. İsteğe bağlı olarak, zaten yapılandırılmışsa sosyal kimlik sağlayıcıları öğesini de seçebilirsiniz.

Altında **kullanıcı öznitelikleri ve talepler**, tıklayın **daha fazla Göster**.

İçinde **toplama özniteliği** sütun, tüketiciden kayıt sırasında toplamak istediğiniz öznitelikleri seçin. Örneğin, **Ülke/Bölge**, **Görünen Ad** ve **Posta Kodu**’nu seçin.

İçinde **dönüş talep** sütun, başarılı bir kaydolma deneyiminden sonra uygulamanıza geri gönderilen belirteçlerde döndürülmesini istediğiniz talepleri seçin. Örneğin, **Görünen Ad**, **Kimlik Sağlayıcısı**, **Posta Kodu**, **Kullanıcı yeni** ve **Kullanıcının Nesne Kimliği**’ni işaretleyin.

**Tamam** düğmesine tıklayın.

**Oluştur**’a tıklayın. Oluşturulan kullanıcı akışı olarak görünür **B2C_1_SiUp** ( **B2C\_1\_**  parçası otomatik olarak eklenir).

Tıklayın **kullanıcı akışı çalıştırma**.

Seçin **Contoso B2C uygulaması** içinde **uygulama** açılır ve `https://localhost:44321/` içinde **yanıt URL'si** açılır.

Tıklayın **kullanıcı akışı çalıştırma**. Yeni bir tarayıcı sekmesi açılır. Buradan, uygulamanız için kaydolmaya yönelik tüketici deneyimi üzerinde işlem yapabilirsiniz.

> [!NOTE]
> Bu bir dakika kullanıcı akışı oluşturma ve güncelleştirmelerinin etkili olması için kapladığı.
>

