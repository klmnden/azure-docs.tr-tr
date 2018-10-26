---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.author: crdun
ms.openlocfilehash: 30b5ae499d29b8b78b5852074362841ac1ceb49f
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50132869"
---
1. [Azure portal] oturum açın.
2. Seçin **+ yeni** > **Web + mobil** > **mobil uygulama**ve, Mobile Apps arka ucu için bir ad belirtin.
3. İçin **kaynak grubu**, mevcut bir kaynak grubunu seçin veya (uygulamanızla aynı adı kullanarak) göre yeni bir tane oluşturun. 
4. İçin **App Service planı**, varsayılan plan (içinde [standart katman](https://azure.microsoft.com/pricing/details/app-service/)) seçilir. Aynı zamanda başka bir plan da seçebilir veya [yeni bir tane oluşturabilirsiniz](../articles/app-service/app-service-plan-manage.md#create-an-app-service-plan). 

   App Service planının ayarları belirlemek [konumu, özellikleri, maliyeti ve işlem kaynaklarını](https://azure.microsoft.com/pricing/details/app-service/) uygulamanızla ilişkili. App Service hakkında daha fazla bilgi için planları ve farklı fiyatlandırma yeni bir plan oluşturma katmanı ve tercih ettiğiniz konumda bkz [Azure App Service planlarına ayrıntılı genel bakış](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
   
5. **Oluştur**’u seçin. Bu adım, Mobile Apps arka ucu oluşturur. 
6. İçinde **ayarları** yeni Mobile Apps arka ucu, bölmesinde seçin **Hızlı Başlangıç** > istemci uygulaması platformunuz > **bir veritabanına bağlanmak**. 
   
   ![Bir veritabanına bağlanma seçimleri](./media/app-service-mobile-dotnet-backend-create-new-service/dotnet-backend-create-data-connection.png)
7. İçinde **veri bağlantısı ekleme** bölmesinde **SQL veritabanı** > **yeni veritabanı oluştur**. Veritabanı adını girin, bir fiyatlandırma katmanı seçin ve ardından **sunucu**. Bu yeni veritabanını yeniden kullanabilirsiniz. Aynı konumda zaten bir veritabanınız varsa, bunun yerine **Mevcut veritabanlarından birini kullanın**’ı seçebilirsiniz. Bant genişliği maliyetleri ve daha yüksek gecikme nedeniyle farklı bir konumda bir veritabanının kullanılmasını önermeyiz.
   
   ![Bir veritabanını seçme](./media/app-service-mobile-dotnet-backend-create-new-service/dotnet-backend-create-db.png)
8. İçinde **yeni sunucu** bölmesinde bir benzersiz sunucu adını girin **sunucu adı** kutusunda, oturum açma ve parola sağlayın **izin Azure Hizmetleri için erişim sunucusu**, seçin **Tamam**. Bu adım, yeni bir veritabanı oluşturur.
9. Geri **veri bağlantısı ekleme** bölmesinde **bağlantı dizesi**, veritabanınız için oturum açma adı ve parola değerleri girin ve seçin **Tamam**. 

   Veritabanı, devam etmeden önce sorunsuz dağıtılması birkaç dakika bekleyin.

<!-- URLs. -->
[Azure portal]: https://portal.azure.com/
