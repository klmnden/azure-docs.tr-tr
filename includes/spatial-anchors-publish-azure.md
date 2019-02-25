---
author: ramonarguelles
ms.service: spatial-anchors
ms.topic: include
ms.date: 1/30/2019
ms.author: rgarcia
ms.openlocfilehash: 692eb3fcd1187bf64ba5215631679c4cddf45d56
ms.sourcegitcommit: e88188bc015525d5bead239ed562067d3fae9822
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/24/2019
ms.locfileid: "56752165"
---
### <a name="launch-the-publish-wizard"></a>Yayımlama sihirbazını başlatma

İçinde **Çözüm Gezgini**, sağ **SharingService** seçin ve proje **Yayımla**.

Yayımlama sihirbazı otomatik olarak başlatılır. **App Service** > **Yayımla**'yı seçerek **App Service Oluştur** iletişim kutusunu açın.

### <a name="sign-in-to-azure"></a>Azure'da oturum açma

**App Service Oluştur** iletişim kutusunda **Hesap ekle**’ye tıklayın ve Azure aboneliğinizde oturum açın. Zaten oturum açmış durumdaysanız, açılan listeden istediğiniz hesabı seçin.

> [!NOTE]
> Zaten oturum açtıysanız **Oluştur** öğesini henüz seçmeyin.
>

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[!INCLUDE [resource group intro text](resource-group.md)]

**Kaynak Grubu**’nun yanındaki **Yeni** öğesini seçin.

Kaynak grubunuzu **myResourceGroup** olarak adlandırıp **Tamam**’ı seçin.

### <a name="create-an-app-service-plan"></a>App Service planı oluşturma

[!INCLUDE [app-service-plan](app-service-plan.md)]

**Barındırma Planı**'nın yanındaki **Yeni**'yi seçin.

İçinde **barındırma planını Yapılandır** iletişim kutusunda, tablodaki ayarları kullanın.

| Ayar | Önerilen Değer | Açıklama |
|-|-|-|
|App Service Planı| MySharingServicePlan | App Service planının adı. |
| Konum | Batı ABD | Web uygulamasının barındırıldığı veri merkezi. |
| Boyut | Ücretsiz | [Fiyatlandırma katmanı](https://azure.microsoft.com/pricing/details/app-service/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio), barındırma özelliklerini belirler. |

**Tamam**’ı seçin.

### <a name="create-and-publish-the-web-app"></a>Web uygulaması oluşturma ve yayımlama

**Uygulama Adı**’na benzersiz bir ad girin (geçerli karakterler `a-z`, `0-9` ve `-` karakterleridir) veya otomatik olarak oluşturulan benzersiz adı kabul edin. Web uygulamasının URL'si `http://<app_name>.azurewebsites.net` şeklindedir; burada `<app_name>`, uygulamanızın adıdır.

Azure kaynaklarını oluşturmaya başlamak için **Oluştur**’u seçin.

Sihirbaz tamamlandıktan sonra ASP.NET Core web uygulamasını Azure’da yayımlar ve ardından uygulamayı varsayılan tarayıcıda başlatır.

![Azure’da yayımlanmış ASP.NET web uygulaması](./media/spatial-anchors-azure/web-app-running-live.png)

[Oluşturma ve yayımlama adımında](#create-and-publish-the-web-app) belirtilen uygulama adı `http://<app_name>.azurewebsites.net` biçiminde URL ön eki olarak kullanılır. Daha sonra kullanılacağından bu URL'yi not alın.
