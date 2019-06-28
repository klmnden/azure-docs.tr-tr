---
title: include dosyası
description: include dosyası
services: app-service\mobile
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 06/20/2019
ms.author: crdun
ms.custom: include file
ms.openlocfilehash: 72a69359d412a7560472fbb73ec525ab5d4a4fce
ms.sourcegitcommit: 5cb0b6645bd5dff9c1a4324793df3fdd776225e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67325804"
---
1. [Azure Portal] oturum açın.

2. **Kaynak oluştur**’a tıklayın.

3. Arama kutusuna **Web uygulaması**.
    
4. Sonuç listesinden **Web uygulaması** marketten.

5. Seçin, **abonelik** ve **kaynak grubu** (mevcut bir kaynak grubunu seçin _veya_ (uygulamanızla aynı adı kullanarak) yeni bir tane oluşturun).

6. Benzersiz bir seçin **adı** web uygulamanızın.

7. Varsayılan seçin **Yayımla** olarak seçeneğini **kod**.

8. İçinde **çalışma zamanı yığını**, altında bir sürüm seçmek gereken **ASP.NET** veya **düğüm**. Bir .NET arka ucu oluşturuyorsanız ASP.NET altında bir sürüm seçin. Aksi takdirde göre düğüm uygulama hedefliyorsanız, sürüm birini düğümünü seçin.

9. Sağa seçin **işletim sistemi**, Linux veya Windows. 

10. Seçin **bölge** dağıtılması için bu uygulamanın istediğiniz. 

11. Uygun seçin **App Service planı** ve isabet **gözden geçir ve Oluştur**. 

12. **Kaynak Grubu** altında mevcut bir kaynak grubunu seçin _ya da_ yeni bir tane oluşturun (uygulamanızla aynı adı kullanarak).

13. **Oluştur**’a tıklayın. Devam etmeden önce hizmetin sorunsuz dağıtılması için birkaç dakika bekleyin. Durum güncelleştirmeleri için portal üst bilgisindeki Bildirimler (zil) simgesini izleyin.

14. Dağıtım tamamlandıktan sonra tıklayarak **dağıtım ayrıntıları** kaynak türünde,'a tıklayın ve bölüm **Microsoft.Web/sites**. Bu App Service Web oluşturduğunuz uygulamasına gider. 

15. Tıklayarak **yapılandırma** altındaki dikey penceresinde **ayarları** ve **uygulama ayarları**, tıklayarak **yeni uygulama ayarı** düğmesi.

16. İçinde **Ekle/Düzenle uygulama ayarı** want **adı** olarak **MobileAppsManagement_EXTENSION_VERSION** olarak değer **son** ve Tamam'a tıklıyorum.

Bu yeni App Service Web uygulaması bir mobil uygulama olarak oluşturulan kullanmaya hazırsınız.

<!-- URLs. -->
[Azure portal]: https://portal.azure.com/