---
author: PatAltimore
ms.service: active-directory-b2c
ms.topic: include
origin.date: 11/30/2018
ms.date: 04/04/2019
ms.author: v-junlch
ms.openlocfilehash: 0ab34d6234db9c13ffe82ccd0e8580217085f631
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60456045"
---
Yalnızca oturum açma, uygulamanızın üzerinde etkinleştirmek istiyorsanız, kullandığınız bir **oturum** kullanıcı akışı. Bu kullanıcı akışını müşteriler oturum açma ve uygulamanın alacağı belirteçlerin içeriğini sırasında başarılı oturum açma işlemleri üzerinde karşılaşacağı deneyimleri açıklanmaktadır.

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]
Altında **Yönet**seçin **kullanıcı akışları**.

Tıklayın +**yeni kullanıcı akışı** dikey penceresinin üstünde.

Altında **kullanıcı akış türü seç**seçin **tüm**ve ardından sürümünü seçin **oturum** kullanmak istiyorsunuz.

**Adı** uygulamanız tarafından kullanılan oturum açma kullanıcı Akış adını belirler. Örneğin, **SiIn** adını girin.

Altında **kimlik sağlayıcıları**, bir seçenek belirleyin. Ayrıca, zaten yapılandırılmışsa sosyal kimlik sağlayıcıları seçebilirsiniz. **Tamam** düğmesine tıklayın.

Altında **uygulama taleplerini**, tıklayın **daha fazla Göster**.

İçinde **dönüş talep** sütun, başarılı bir oturum açma deneyiminden sonra uygulamanıza geri gönderilen belirteçlerde döndürülmesini istediğiniz talepleri seçin. Örneğin, **Görünen Ad**, **Kimlik Sağlayıcısı**, **Posta Kodu** ve **Kullanıcının Nesne Kimliği**’ni işaretleyin. **Tamam** düğmesine tıklayın.

**Oluştur**’a tıklayın. Yeni oluşturduğunuz kullanıcı akışı olarak göründüğüne dikkat edin **b2c_1_siın** ( **B2C\_1\_**  parçası otomatik olarak eklenir).

Tıklayın **kullanıcı akışı çalıştırma**.

Seçin **Contoso B2C uygulaması** içinde **uygulama** açılır ve `https://localhost:44321/` içinde **yanıt URL'si** açılır.

Tıklayın **kullanıcı akışı çalıştırma**. Yeni bir tarayıcı sekmesi açılır. Buradan, uygulamanız için oturum açmaya yönelik tüketici deneyimi üzerinde işlem yapabilirsiniz.

> [!NOTE]
> Bu bir dakika kullanıcı akışı oluşturma ve güncelleştirmelerinin etkili olması için kapladığı.
>

