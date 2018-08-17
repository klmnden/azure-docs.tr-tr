---
title: include dosyası
description: include dosyası
services: app-service\mobile
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 05/25/2018
ms.author: crdun
ms.custom: include file
ms.openlocfilehash: 1d3bfb7bc8a5432392dba3b0c5019902b3e59773
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2018
ms.locfileid: "39514115"
---
1. **Uygulama Hizmetleri** düğmesine tıklayın, Mobile Apps arka ucunuzu seçin, **Hızlı Başlangıç**’ı seçin ve ardından, istemci platformunuzu (iOS, Android, Xamarin, Cordova) seçin.

    ![Mobile Apps Hızlı Başlangıcın vurgulandığı Azure Portal][quickstart]

1. Veritabanı bağlantısı yapılandırılmamışsa aşağıdakileri yaparak bir bağlantı oluşturun:

    ![Mobile Apps ile Azure Portal, veritabanına bağlanma][connect]

    a. Yeni bir SQL veritabanı ve sunucusu oluşturun. Aşağıdaki 3 numaralı adımı tamamlamak için bağlantı dizesi adı alanındaki MS_TableConnectionString varsayılan değerini tutmanız gerekir.

    ![Mobile Apps ile Azure Portal, yeni veritabanı ve sunucu oluşturma][server]

    b. Veri bağlantısı başarıyla oluşturulana kadar bekleyin.

    ![Veri bağlantısının başarıyla oluşturulduğunu gösteren Azure Portal bildirimi][notification]

    c. Veri bağlantısının başarılı olması gerekir.

    ![Azure Portal bildirimi, "Bir veri bağlantınız zaten var"][already-connection]

1. **2. Tablo API'si oluştur**'un altında **Arka uç dili** olarak Node.js seçeneğini belirleyin.

1. Bildirimi kabul edin ve **TodoItem tablosu oluştur**’u seçin.
    Böylece veritabanınızda yeni bir yapılacaklar tablosu oluşturulur.

    >[!IMPORTANT]
    > Mevcut bir arka uç Node.js'ye geçirildiğinde tüm içeriğin üzerine yazılır. Bunun yerine bir .NET arka ucu oluşturmak için bkz. [Mobile Apps için .NET arka uç sunucu SDK’siyle çalışma][instructions].

<!-- Images. -->
[quickstart]: ./media/app-service-mobile-configure-new-backend/quickstart.png
[connect]: ./media/app-service-mobile-configure-new-backend/connect-to-bd.png
[notification]: ./media/app-service-mobile-configure-new-backend/notification-data-connection-create.png
[server]: ./media/app-service-mobile-configure-new-backend/create-new-server.png
[already-connection]: ./media/app-service-mobile-configure-new-backend/already-connection.png

<!-- URLs -->
[instructions]: ../articles/app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#create-app
