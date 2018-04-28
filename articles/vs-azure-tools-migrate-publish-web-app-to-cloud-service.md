---
title: Geçiş ve bir Azure bulut hizmeti Visual Studio'dan bir Web uygulaması yayımlama | Microsoft Docs
description: Geçirmek ve web uygulamanıza bir Azure bulut hizmeti Visual Studio kullanarak yayımlama hakkında bilgi edinin
services: visual-studio-online
author: ghogen
manager: douge
ms.assetid: 9394adfd-a645-4664-9354-dd5df08e8c91
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: conceptual
ms.date: 11/10/2017
ms.author: ghogen
ms.openlocfilehash: aa09cd06a5ccea3f18459efb701aeaa8d9e59639
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="how-to-migrate-and-publish-a-web-application-to-an-azure-cloud-service-from-visual-studio"></a>Nasıl yapılır: geçirmek ve bir Azure bulut hizmeti Visual Studio'dan bir Web uygulaması yayımlama

Barındırma hizmetleri ve ölçeklendirme Özelliği Azure yararlanacak şekilde geçirmek ve web uygulamanızı Azure bulut hizmeti dağıtmak isteyebilirsiniz. Sadece küçük değişiklikler gereklidir. Bu makale, yalnızca bulut hizmetlerine dağıtma kapsamaktadır; Uygulama hizmeti için bkz: [Azure App Service'te bir web uygulaması dağıtma](app-service/app-service-deploy-local-git.md).

> [!Important]
> Bu geçiş, yalnızca belirli ASP.NET, Silverlight, WCF ve WCF iş akışı projeleri için desteklenir. ASP.NET Core projeleri için desteklenmiyor. Bkz: [proje şablonları desteklenen](#supported-project-templates).

## <a name="migrate-a-project-to-cloud-services"></a>Bulut Hizmetleri için bir proje geçirme

1. Web uygulaması projesine sağ tıklatın ve **Dönüştür > Microsoft Azure bulut hizmeti projesine dönüştürme**. (Bir web rolü projesi çözümde zaten varsa bu komutu görünmez unutmayın.)
1. Visual Studio gerekli web rolü içeren çözümdeki bulut hizmeti projesi oluşturur. Uygulama projenizi ile hem soneki bu projenin adı aynı olan `.Azure`.
1. Visual Studio ayrıca ayarlar **kopya yerel** özelliğinin MVC 2, MVC 3, MVC 4 ve Silverlight iş uygulamaları için gerekli olan tüm derlemeler için true. Bu özellik bu derlemeler dağıtımı için kullanılan hizmet paketi ekler.

   > [!Important]
   > Diğer derlemeler veya bu web uygulaması için gerekli olan dosyalar varsa, bu dosyalar için özellikleri el ile ayarlamanız gerekir. Bu özellikleri ayarlama hakkında daha fazla bilgi için bkz: [dosyaları içerir hizmet paketinde](#include-files-in-the-service-package).

### <a name="errors-and-warnings"></a>Hatalar ve uyarılar

Herhangi bir uyarı veya oluşan hataları gibi eksik derlemeleri Azure'a dağıtmadan önce düzeltmek için sorunları gösterir.

Uygulamanızı çalıştırın işlem öykünücüsü kullanarak yerel olarak veya Azure'a yayımlama, hatayla görebilirsiniz: "Belirtilen yol, dosya adı veya her ikisini birden çok uzun." Bu hata, tam Azure proje adının uzunluğu 146 karakteri aşıyor gösterir. Sorunu gidermek için çözümünüzün daha kısa yola sahip başka bir klasöre taşıyın.

Tüm uyarıları hata ele hakkında daha fazla bilgi için bkz: [bir Azure bulut hizmeti projesi ile Visual Studio'yu yapılandırma](vs-azure-tools-configuring-an-azure-project.md).

### <a name="test-the-migration-locally"></a>Geçiş yerel olarak test

1. Visual Studio'da **Çözüm Gezgini**, eklenen bulut hizmeti projesine sağ tıklatın ve **başlangıç projesi olarak ayarla**.
1. Seçin **hata ayıklama > hata ayıklamayı Başlat** Azure hata ayıklama ortamı başlatmak için (F5). Bu ortamın özellikle öykünmesi çeşitli Azure hizmetleri sağlar.

### <a name="use-an-azure-sql-database-for-your-application"></a>Uygulamanız için bir Azure SQL veritabanını kullan

Bir şirket içi SQL Server veritabanı kullanan web uygulamanız için bir bağlantı dizesi varsa, veritabanınızı Azure SQL veritabanına geçirilmesini ve bağlantı dizesini güncellemeniz gerekir. Bu işlem hakkında yönergeler için aşağıdaki konulara bakın:

- [Bulutta SQL veritabanı için SQL Server veritabanı geçirme](sql-database/sql-database-cloud-migrate.md)
- [.NET (C#) bağlanma ve sorgulama için Visual Studio ve Azure SQL veritabanı ile kullanmayı](sql-database/sql-database-connect-query-dotnet-visual-studio.md).

## <a name="publish-the-application-to-azure-cloud-service"></a>Azure bulut hizmeti uygulamayı yayımlama

1. Gerekli bulut hizmeti ve depolama hesaplarını Azure aboneliğinizde açıklandığı gibi oluşturmak [yayımlamak veya Visual Studio'dan Azure bir uygulamayı dağıtmak hazırlama](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md).
1. Visual Studio'da Uygulama projesine sağ tıklatın ve **Microsoft Azure Yayımla...**  (Bu, "Yayımla..." komutunu farklıdır.).
1. İçinde **Azure uygulamasını Yayımla** görüntülenir, Azure aboneliğinizle hesabı kullanarak oturum açın ve seçin **sonraki >**.
1. İçinde **ayarlar > Genel ayarları** sekmesinde, hedef bulut hizmetini seçin **bulut hizmeti** seçilen ortamınıza ve yapılandırmalarınıza birlikte açılır listesi. 
1. İçinde **ayarlar > Gelişmiş ayarları**, kullanın ve sonra seçmek için depolama hesabı seçin **sonraki >**.
1. İçinde **tanılama**, Application Insights'a bilgileri göndermek isteyip istemediğinizi seçin.
1. Seçin **sonraki >** özeti görüntülemek için ardından **Yayımla** dağıtımı başlatmak için.
1. Visual Studio burada ilerleme durumunu izleyebilirsiniz bir etkinlik günlüğü penceresi açar:

    ![VST_AzureActivityLog](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC744149.png)

1. (İsteğe bağlı) Dağıtım işlemini iptal etmek için etkinlik günlüğünde satır öğeyi sağ tıklatın ve seçin **iptal edin ve kaldırma**. Bu komut, dağıtım işlemi durdurur ve Azure'dan dağıtım ortamı siler. Not: Bu dağıtıldıktan sonra bu dağıtım ortamı kaldırmak için kullanmanız gerekir [Azure portal](https://portal.azure.com).
1. (İsteğe bağlı) Rolü örneklerinizi başlattıktan sonra Visual Studio otomatik olarak dağıtım ortamında gösterir **Sunucu Gezgini > bulut Hizmetleri** düğümü. Buradan, tek tek rol örneklerinin durumunu görüntüleyebilirsiniz.
1. Dağıtımdan sonra uygulamaya erişmek için dağıtımınızın durumunu zaman yanındaki oka seçin **tamamlandı** görünür **Azure etkinlik günlüğü** URL ile birlikte. Azure'dan belirli türdeki bir web uygulaması başlangıç hakkında ayrıntılı bilgi için aşağıdaki tabloya bakın.

## <a name="using-the-compute-emulator-and-starting-application-in-azure"></a>İşlem öykünücüsü kullanarak ve Azure'da uygulama başlatma

Tüm uygulama türleri için Visual Studio hata ayıklayıcısı seçerek bağlı bir tarayıcıda başlatılabilir **hata ayıklama > hata ayıklamayı Başlat** (F5). Bir ASP.NET boş Web uygulaması projesi ile ilk eklemelisiniz bir `.aspx` sayfasında uygulamanızda ve web projeniz için başlangıç sayfası olarak ayarla.

Aşağıdaki tabloda Azure'da uygulama başlatma hakkında ayrıntılar verilmiştir:

   | Web uygulaması türü | Azure'da çalışan |
   | --- | --- | --- |
   | ASP.NET Web uygulaması<br/>(MVC 2, MVC 3, MVC 4 dahil) | URL'de seçin **dağıtım** sekmesinde **Azure etkinlik günlüğü**. |
   | ASP.NET boş Web uygulaması | Varsayılan varsa `.aspx` sayfasında uygulamanızda, URL'de seçin **dağıtım** sekmesinde **Azure etkinlik günlüğü**. Farklı bir sayfaya gitmek için bir tarayıcıda bir URL aşağıdaki biçimde girin: `<deployment_url>/<page_name>.aspx` |
   | Silverlight uygulaması<br/>Silverlight iş uygulaması<br/>Silverlight gezinti uygulaması | Şu URL biçimi kullanarak, uygulamanız için belirli bir sayfaya gidin: `<deployment_url>/<page_name>.aspx` |
    WCF hizmet uygulaması<br/>WCF iş akışı hizmeti uygulaması | Ayarlama `.svc` WCF Hizmeti projeniz için başlangıç sayfası olarak dosya. Ardından gidin `<deployment_url>/<service_file>.svc` |
   | ASP.NET dinamik varlıklar<br/>ASP.NET dinamik veri LINQ-SQL | Bağlantı dizesi bir sonraki bölümde açıklandığı gibi güncelleştirin. Ardından gidin `<deployment_url>/<page_name>.aspx`. LINQ-SQL, Azure SQL veritabanını kullanmanız gerekir. |

## <a name="update-a-connection-string-for-aspnet-dynamic-entities"></a>ASP.NET dinamik varlıklar için bir bağlantı dizesi güncelleştir

1. ASP.NET dinamik varlıklar web uygulaması için bir SQL Azure veritabanı, daha önce (#use-an-azuresql-database-for-your-application) açıklandığı gibi oluşturun.
1. Tabloları ve Azure portalından bu veritabanı için gereksinim duyduğunuz alanları ekleyin.
1. Bağlantı dizesinde belirtin `web.config` dosya aşağıdaki biçimde ve dosyayı kaydedin:

    ```xml
    <addname="tempdbEntities"connectionString="metadata=res://*/Model1.csdl|res://*/Model1.ssdl|res://*/Model1.msl;provider=System.Data.SqlClient;provider connection string=&quot;data source=<server name>\SQLEXPRESS;initial catalog=<database name>;integrated security=True;multipleactiveresultsets=True;App=EntityFramework&quot;"providerName="System.Data.EntityClient"/>
    ```

    Güncelleştirme *connectionString* SQL Azure veritabanı ADO.NET bağlantı dizesiyle gibi değer:

    ```xml
    XMLCopy<addname="tempdbEntities"connectionString="metadata=res://*/Model1.csdl|res://*/Model1.ssdl|res://*/Model1.msl;provider=System.Data.SqlClient;provider connection string=&quot;Server=tcp:<SQL Azure server name>.database.windows.net,1433;Database=<database name>;User ID=<user name>;Password=<password>;Trusted_Connection=False;Encrypt=True;multipleactiveresultsets=True;App=EntityFramework&quot;"providerName="System.Data.EntityClient"/>
    ```

## <a name="supported-project-templates"></a>Desteklenen proje şablonları

Geçiş ve bulut hizmetlerine yayımlanan uygulamaları aşağıdaki tabloda şablonlarından birini kullanmanız gerekir. ASP.NET Core desteklenmiyor.

| Şablon grubu | Proje şablonu |
| --- | --- |
| Web | ASP.NET Web uygulaması (.NET Framework) |
| Web | ASP.NET MVC 2 Web uygulaması |
| Web | ASP.NET MVC 3 Web uygulaması |
| Web | ASP.NET MVC4 Web uygulaması |
| Web | ASP.NET boş Web uygulaması (veya Site) |
| Web | ASP.NET MVC 2 boş Web uygulaması |
| Web | ASP.NET dinamik veri varlıkları Web uygulaması |
| Web | ASP.NET dinamik veri LINQ-SQL Web uygulaması |
| Silverlight | Silverlight uygulaması |
| Silverlight | Silverlight iş uygulaması |
| Silverlight | Silverlight gezinti uygulaması |
| WCF | WCF hizmet uygulaması |
| WCF | WCF iş akışı hizmeti uygulaması |
| İş akışı | WCF iş akışı hizmeti uygulaması |

## <a name="next-steps"></a>Sonraki adımlar

- [Yayımlamak veya Visual Studio'dan Azure bir uygulamayı dağıtmak hazırlama](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md)
- [Adlı kimlik doğrulama kimlik bilgilerini ayarlama](vs-azure-tools-setting-up-named-authentication-credentials.md).
