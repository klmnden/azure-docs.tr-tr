---
author: PatAltimore
ms.service: active-directory-b2c
ms.topic: include
ms.date: 11/03/2016
ms.author: patricka
ms.openlocfilehash: 19e7c919345c0f56b274737840f8150f7d710501
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50133731"
---
Yalnızca oturum açma, uygulamanızın üzerinde etkinleştirmek istiyorsanız, kullandığınız bir **oturum** ilkesi. Bu ilke, müşterilerin oturum açma ve uygulamanın alacağı belirteçlerin içeriğini sırasında başarılı oturum açma işlemleri üzerinde karşılaşacağı deneyimleri açıklar.

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]
Tıklayın **oturum açma ilkeleri**.

Dikey pencerenin en üstündeki **+Add (+Ekle)** seçeneğine tıklayın.

**Ad**, uygulamanız tarafından kullanılan oturum açma ilke adını belirler. Örneğin, **SiIn** adını girin.

**Kimlik sağlayıcıları**’na tıklayın ve **Yerel Hesapla Oturum Aç**’ı seçin. İsteğe bağlı olarak, zaten yapılandırılmışsa sosyal kimlik sağlayıcıları öğesini de seçebilirsiniz. **Tamam** düğmesine tıklayın.

**Uygulama talepleri**’ne tıklayın. Burada, başarılı bir oturum açma deneyiminden sonra uygulamanıza geri gönderilen belirteçlerde döndürülmesini istediğiniz talepleri seçin. Örneğin, **Görünen Ad**, **Kimlik Sağlayıcısı**, **Posta Kodu** ve **Kullanıcının Nesne Kimliği**’ni işaretleyin. **Tamam** düğmesine tıklayın.

**Oluştur**’a tıklayın. Yeni oluşturduğunuz ilkenin **Oturum açma ilkeleri** dikey penceresinde **B2C_1_SiIn** olarak görüneceğini unutmayın (**B2C\_1\_**  parçası otomatik olarak eklenir).

**B2C_1_SiIn** adına tıklayarak ilkeyi açın.

**Uygulamalar** açılır menüsünde **Contoso B2C uygulamasını** ve **Yanıt URL'si / Yeniden Yönlendirme URI'si** altında `https://localhost:44321/` seçeneğini belirleyin.

**Şimdi çalıştır**’a tıklayın. Yeni bir tarayıcı sekmesi açılır. Buradan, uygulamanız için oturum açmaya yönelik tüketici deneyimi üzerinde işlem yapabilirsiniz.

> [!NOTE]
> İlke oluşturma ve güncelleştirmelerinin etkili olması bir dakika kadar alabilir.
>