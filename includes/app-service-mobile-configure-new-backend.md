---
title: include dosyası
description: include dosyası
services: app-service\mobile
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 05/06/2019
ms.author: crdun
ms.custom: include file
ms.openlocfilehash: a7c994f85d90e94d514bb4e4f91a6644ed45432c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66455138"
---
1. Aşağıdaki platformlar için SDK hızlı başlangıçları istemciyi indirin:
    
    [iOS (Objective-C)](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/iOS)  
    [iOS (Swift)](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/iOS-Swift)  
    [Android (Java)](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/android)  
    [Xamarin.iOS](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/xamarin.iOS)  
    [Xamarin.Android](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/xamarin.android)  
    [Xamarin.Forms](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/xamarin.forms)  
    [Cordova](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/cordova)  
    [Windows (C#)](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/windows-uwp-cs)  

    > [!NOTE]
    > İOS projesi kullanıyorsanız indirmeniz gerekir "azuresdk-iOS -\*.zip" den [son GitHub sürüm](https://github.com/Azure/azure-mobile-apps-ios-client/releases/latest). Sıkıştırmasını açın ve eklemek `MicrosoftAzureMobile.framework` projenin kök dosya.
    >

2. Veritabanı bağlantısı eklemek veya mevcut bir bağlantısı gerekir. İlk olarak, bir veri deposu oluşturmak veya mevcut bir belirler.

    - **Yeni veri deposu Oluştur**: Bir veri deposu oluşturmak için kullanacaksanız, şu hızlı başlangıcı kullanın:

        [Hızlı Başlangıç: Azure SQL veritabanı'nda tek veritabanları ile çalışmaya başlama](https://docs.microsoft.com/azure/sql-database/sql-database-single-database-quickstart-guide)

    - **Varolan bir veri kaynağına**: Varolan bir veritabanı bağlantısı kullanmak istiyorsanız aşağıdaki yönergeleri izleyin.

        1. SQL veritabanı bağlantı dizesi biçimi- `Data Source=tcp:{your_SQLServer},{port};Initial Catalog={your_catalogue};User ID={your_username};Password={your_password}`

           **{your_SQLServer}**  Ad sunucusunun bu veritabanınıza ilişkin genel bakış sayfasında bulunabilir ve genellikle "server_name.database.windows.net" biçiminde.
            **{port}**  genellikle 1433.
            **{your_catalogue}**  Veritabanının adı.
            **{your_username}**  Veritabanınıza erişmek için kullanıcı adı.
            **{your_password}**  Veritabanınıza erişmek için parola.

            [SQL bağlantı dizesi biçimi hakkında daha fazla bilgi edinin](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-string-syntax#sqlclient-connection-strings)

        2. Bağlantı dizesi ekleyin, **mobil uygulama** App Service'de yönetebileceğiniz bağlantı dizelerini uygulamanızın kullanarak **yapılandırma** menüsündeki seçeneği.

            Bir bağlantı dizesi eklemek için:

            1. Tıklayarak **uygulama ayarları** sekmesi.

            2. Tıklayarak **[+] yeni bağlantı dizesi**.

            3. Sağlamanız gerekir **adı**, **değer** ve **türü** , bağlantı dizesi.

            4. Tür **adı** olarak `MS_TableConnectionString`

            5. Değer, önce adımda oluşturulmuş bağlantı dizesi olmalıdır.

            6. Bir SQL Azure veritabanına bir bağlantı dizesi ekliyorsanız seçin **SQLAzure** altında **türü**.

3. Azure Mobile Apps SDK'ları için .NET ve Node.js Arka Uçlara sahiptir.

   - **Node.js arka ucu**
    
     Node.js hızlı başlangıç uygulamasını kullanacaksanız, aşağıdaki yönergeleri izleyin.

     1. Azure portalında Git **kolay tablolar**, bu ekranı görürsünüz.
      
        ![Düğüm kolay tablolar](./media/app-service-mobile-configure-new-backend/node-easy-tables.png)

     2. Emin SQL bağlantı dizesi zaten eklenir **yapılandırma** sekmesi. Sonra da kutuyu işaretleyin **bu tüm site içeriğinin üzerine yazacağını kabul ediyorum** tıklatıp **Todoıtem tablosu oluştur** düğmesi.
     
        ![Düğüm kolay tablolar yapılandırma](./media/app-service-mobile-configure-new-backend/node-easy-tables-configuration.png)

     3. İçinde **kolay tablolar**, tıklayın **+ Ekle** düğmesi.
    
        ![Kolay tablolar düğümü Ekle düğmesi](./media/app-service-mobile-configure-new-backend/node-easy-tables-add.png)

     4. Oluşturma bir `TodoItem` anonim erişimi olan tablo.
      
        ![Düğüm kolay tablolar tablo ekleme](./media/app-service-mobile-configure-new-backend/node-easy-tables-table-add.png)

   - **.NET arka ucu**
    
        .NET hızlı başlangıç uygulaması kullanacaksanız, aşağıdaki yönergeleri izleyin.

        1. Azure Mobile Apps .NET sunucu projesinden indirmeniz [azure mobile apps quickstarts depo](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/backend/dotnet/Quickstart).

        2. Visual Studio'da yerel olarak .NET sunucu projesi oluşturun.

        3. Visual Studio'da Çözüm Gezgini'ni açın, sağ `ZUMOAPPNAMEService` proje, tıklayın **Yayımla**, görürsünüz bir `Publish to App Service` penceresi. Mac üzerinde çalışıyorsanız, uygulamayı dağıtmak için diğer yöntemlere göz atın [burada](https://docs.microsoft.com/azure/app-service/deploy-local-git).
        
           ![Visual studio yayımlama](./media/app-service-mobile-configure-new-backend/visual-studio-publish.png)

        4. Seçin **App Service** hedef yayımlama gibi ardından **var olanı Seç**, ardından **Yayımla** pencerenin alt kısmındaki düğmesi.

        5. Visual Studio ile ilk Azure aboneliğinizle oturum gerekecektir. Seçin `Subscription`, `Resource Group`ve ardından uygulamanızın adını seçin. Hazır olduğunuzda tıklayın **Tamam**, bu olan .NET sunucu projesini App Service arka uca yerel olarak dağıtır. Dağıtım tamamlandığında, yönlendirilecek `http://{zumoappname}.azurewebsites.net/` tarayıcıda.
        
           ![Arka uç çalışıyor](./media/app-service-mobile-configure-new-backend/backend-is-up.png)
