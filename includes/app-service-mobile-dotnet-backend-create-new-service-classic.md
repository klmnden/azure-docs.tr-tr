---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.author: crdun
ms.openlocfilehash: e087a1db008422aeec8fd4e073a7476eebe4d54b
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67189053"
---
1. [Azure portal] oturum açın.
2. Seçin **+ yeni** > **Web + mobil** > **mobil uygulama**ve, Mobile Apps arka ucu için bir ad belirtin.
3. İçin **kaynak grubu**, mevcut bir kaynak grubunu seçin veya (uygulamanızla aynı adı kullanarak) göre yeni bir tane oluşturun. 
4. İçin **App Service planı**, varsayılan plan (içinde [standart katman](https://azure.microsoft.com/pricing/details/app-service/)) seçilir. Aynı zamanda başka bir plan da seçebilir veya [yeni bir tane oluşturabilirsiniz](../articles/app-service/app-service-plan-manage.md#create-an-app-service-plan). 

   App Service planının ayarları belirlemek [konumu, özellikleri, maliyeti ve işlem kaynaklarını](https://azure.microsoft.com/pricing/details/app-service/) uygulamanızla ilişkili. App Service hakkında daha fazla bilgi için planları ve farklı fiyatlandırma yeni bir plan oluşturma katmanı ve tercih ettiğiniz konumda bkz [Azure App Service planlarına ayrıntılı genel bakış](../articles/app-service/overview-hosting-plans.md).
   
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
