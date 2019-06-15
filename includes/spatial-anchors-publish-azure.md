---
author: ramonarguelles
ms.service: spatial-anchors
ms.topic: include
ms.date: 1/30/2019
ms.author: rgarcia
ms.openlocfilehash: 0dab71b6d169e26a3d7dc208dd09efe1143fbe13
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "67135363"
---
### <a name="open-the-publish-wizard"></a>Yayımlama sihirbazını açın

İçinde **Çözüm Gezgini**, sağ **SharingService** seçin ve proje **Yayımla**.

Yayımlama Sihirbazı'nı başlatır. Seçin **App Service** > **Yayımla** açmak için **App Service Oluştur** iletişim kutusu.

### <a name="sign-in-to-azure"></a>Azure'da oturum açma

İçinde **App Service Oluştur** iletişim kutusunda **Hesap Ekle** ve Azure aboneliğinizde oturum açın. Henüz oturum açmadıysanız, aşağı açılan listeden istediğiniz hesabı seçin.

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

İçinde **barındırma planını Yapılandır** iletişim kutusunda, bu ayarları kullanın:

| Ayar | Önerilen değer | Açıklama |
|-|-|-|
|App Service Planı| MySharingServicePlan | App Service planının adı. |
| Location | Batı ABD | Web uygulamasının barındırıldığı veri merkezi. |
| Boyut | Boş | [Fiyatlandırma katmanı](https://azure.microsoft.com/pricing/details/app-service/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) , barındırma özelliklerini belirler. |

**Tamam**’ı seçin.

### <a name="create-and-publish-the-web-app"></a>Web uygulaması oluşturma ve yayımlama

İçinde **uygulama adı**, benzersiz bir uygulama adı girin (geçerli karakterler `a-z`, `0-9`, ve `-`), veya otomatik olarak oluşturulan adı kabul edin. Web uygulamasının URL'si `https://<app_name>.azurewebsites.net` şeklindedir; burada `<app_name>`, uygulamanızın adıdır.

Azure kaynaklarını oluşturmaya başlamak için **Oluştur**’u seçin.

Sihirbaz tamamlandıktan sonra ASP.NET Core web uygulamasını Azure'da yayımlar ve ardından uygulamayı varsayılan tarayıcınızda açılır.

![Azure’da yayımlanmış ASP.NET web uygulaması](./media/spatial-anchors-azure/web-app-running-live.png)

Bu bölümde kullanılan uygulama adı biçiminde URL ön eki olarak kullanılan `https://<app_name>.azurewebsites.net`. Bunu gerekeceği için bu URL'yi not alın.
