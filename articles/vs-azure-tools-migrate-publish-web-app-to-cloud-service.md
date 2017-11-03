---
title: "Geçiş ve bir Azure bulut hizmeti Visual Studio'dan bir Web uygulaması yayımlama | Microsoft Docs"
description: "Geçirmek ve web uygulamanıza bir Azure bulut hizmeti Visual Studio kullanarak yayımlama hakkında bilgi edinin."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 9394adfd-a645-4664-9354-dd5df08e8c91
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: d5de4f5a7357cf5adde7773867356d47ad447bab
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-migrate-and-publish-a-web-application-to-an-azure-cloud-service-from-visual-studio"></a>Nasıl yapılır: geçirmek ve bir Azure bulut hizmeti Visual Studio'dan bir Web uygulaması yayımlama
Barındırma hizmetleri ve Azure ölçeklenebilirliğini yararlanacak şekilde geçirmek ve web uygulamanıza bir Azure bulut hizmetinde yayımlama isteyebilirsiniz. Bir web uygulaması Azure'da, varolan bir uygulamaya küçük değişiklikler çalıştırabilirsiniz.

> [!NOTE]
> Bu konuda, web siteleri için bulut Hizmetleri dağıtma hakkında ' dir. Web sitelerine dağıtma hakkında daha fazla bilgi için bkz: [Azure App Service'te bir web uygulaması dağıtma](app-service/app-service-deploy-local-git.md).
>
>

Bölüm hem Visual C# ve Visual Basic için desteklenen belirli şablonları listesi için bkz: **desteklenen proje şablonları** bu konuda daha sonra.

İlk web uygulamanız için Azure Visual Studio'dan etkinleştirmeniz gerekir. Dağıtımı için kullanılacak bir Azure projesi ekleyerek varolan web uygulamanızı yayımlamak için temel adımlar aşağıda gösterilmiştir. Bu işlem, çözümünüz için gerekli web rolüne sahip bir Azure projesi ekler. Sahip olduğunuz web projesi türüne bağlı olarak, hizmet paketi dağıtım için ek derlemeler gerektiriyorsa derlemeler için proje özelliklerini de güncellenir.

![Microsoft Azure Web uygulaması yayımlama](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC748917.png)

> [!NOTE]
> **Dönüştür**, **Azure bulut hizmeti projesine dönüştürme** komutu yalnızca çözümünüzdeki web projesi için görüntülenir. Örneğin, komut, çözümünüz içinde Silverlight projesi için kullanılabilir değil.
> Bir hizmet paketi oluşturduğunuzda ya da uygulamanızı Azure'a yayımlama uyarılar veya hatalar oluşabilir. Bu uyarıları ve hataları Azure'a dağıtmadan önce sorunları çözmenize yardımcı olabilir. Örneğin, eksik bir derleme hakkında bir uyarı alabilirsiniz. Tüm uyarıları hata ele hakkında daha fazla bilgi için bkz: [bir Azure bulut hizmeti projesi ile Visual Studio'yu yapılandırma](vs-azure-tools-configuring-an-azure-project.md). Uygulamanızı çalıştırın işlem öykünücüsü kullanarak yerel olarak veya Azure'a yayımlayacaksınız, aşağıdaki hatayı görebilirsiniz **hata listesi** penceresi: **belirtilen yol, dosya adı veya her ikisini birden çok uzun**. Tam Azure proje adı çok uzun olduğundan bu hata oluşur. Tam yolunu da içeren proje adının uzunluğu birden fazla 146 karakterler olamaz. Örneğin, bu Silverlight uygulaması için oluşturulmuş bir Azure projesi için dosya yolu da dahil olmak üzere tam proje adıdır: `c:\users\<user name>\documents\visual studio 2015\Projects\SilverlightApplication4\SilverlightApplication4.Web.Azure.ccproj`. Çözümünüzü tam proje adının uzunluğu azaltmak için daha kısa yola sahip başka bir dizine taşımanız gerekebilir.
>
>

Geçiş ve Azure Visual Studio'dan bir web uygulamasına yayımlamak için aşağıdaki adımları izleyin.

## <a name="enable-a-web-application-for-deployment-to-azure"></a>Bir Web uygulaması Azure dağıtım için etkinleştir
### <a name="to-enable-a-web-application-for-deployment-to-azure"></a>Bir web uygulaması için Azure dağıtımına etkinleştirmek için
1. Web uygulamanızı azure'a etkinleştirmek için çözümünüz içinde bir web projesi için kısayol menüsünü açın ve Azure dağıtım projesi eklemek seçin.

    Aşağıdaki eylemler gerçekleşir:

   * Bir Azure projesi adlı `<name of the web project>.Azure` uygulamanız için çözümü eklenir.
   * Web projesinin bir web rolü bu Azure projesi eklenir.
   * **Kopya yerel** özelliği MVC 2, MVC 3, MVC için gerekli olan tüm derlemeler için 4 ve Silverlight iş uygulamaları true olarak ayarlanmış. Bu dağıtım için kullanılan hizmet paketi için bu derlemeler ekler.

   > [!IMPORTANT]
   > Diğer derlemeler veya bu web uygulaması için gerekli olan dosyalar varsa, bu dosyalar için özellikleri el ile ayarlamanız gerekir. Bu özellikleri ayarlama hakkında daha fazla bilgi için bkz **dosyaları içerir hizmet paketinde** bu makalenin ilerisinde yer.
   >
   > [!NOTE]
   > Web rolü belirli web projesi için bir Azure projesi çözümde zaten varsa **Dönüştür**, **Azure bulut hizmeti projesine dönüştürme** bu web projesi için kısayol menüsünde gösterilmez.
   >
   >

   Birden çok web projeleri, web uygulamanızda varsa ve her web projesi için web rolleri oluşturmak istediğiniz her web projesi için bu yordamdaki adımları gerçekleştirmeniz gerekir. Bu, her bir web rolü için ayrı Azure proje oluşturur. Her web projesi ayrı olarak yayımlanabilir. Alternatif olarak, el ile başka bir web rolü için mevcut bir Azure projesi web uygulamanızda ekleyebilirsiniz. Bunu yapmak için kısayol menüsünü açın **rolleri** Azure projenizdeki klasörü seçin **Ekle**, ardından **çözüm Web rolü projesinde**, bir web rolü olarak eklemek için proje seçin ve ardından **Tamam** düğmesi.

## <a name="use-an-azure-sql-database-for-your-application"></a>Uygulamanız için bir Azure SQL veritabanını kullan
Şirket içinde bir SQL Server veritabanını kullanan web uygulamanız için bir bağlantı dizesi varsa, Azure barındıran bir SQL veritabanı örneğini kullanması için bu bağlantı dizesi yerine değiştirmeniz gerekir.

> [!IMPORTANT]
> Aboneliğiniz, SQL veritabanını kullanmak etkinleştirmeniz gerekir. Aboneliğinizden erişirseniz [Klasik Azure portalı](http://go.microsoft.com/fwlink/?LinkID=213885), aboneliğinizi sağlar hangi hizmetlerin belirleyebilirsiniz. Aşağıdaki yönergeler yayımlanan için geçerli [Klasik Azure portalı](http://go.microsoft.com/fwlink/?LinkID=213885). Kullanıyorsanız [Azure portal](http://portal.microsoft.com), bir sonraki yordamı atlayın.
>
>

### <a name="to-use-a-sql-database-instance-in-your-web-role-for-your-connection-string"></a>Bir SQL veritabanı örneği web rolünüz için bağlantı dizesi kullanmak için
1. SQL veritabanı'nda bir örneğini oluşturmak için [Klasik Azure portalı](http://go.microsoft.com/fwlink/?LinkID=213885), aşağıdaki makalesindeki adımları izleyin: [bir SQL veritabanı sunucusu oluşturmak](http://go.microsoft.com/fwlink/?LinkId=225109).

   > [!NOTE]
   > SQL veritabanı Örneğiniz için güvenlik duvarı kuralları ayarlamak, seçmelisiniz **bu sunucuya erişmek diğer Azure hizmetleriyle izin** onay kutusu.
   >
   >
2. SQL veritabanı için bağlantı dizesi kullanmak için bir örneğini oluşturmak için sonraki bölümde yer alan aşağıdaki makalede adımları izleyin: [bir SQL veritabanı oluşturma](http://go.microsoft.com/fwlink/?LinkId=225110).
3. Bağlantı dizenizi için kullanılacak ADO.NET bağlantı dizesi kopyalamak için aşağıdaki adımları gerçekleştirin [Klasik Azure portalı](http://go.microsoft.com/fwlink/?LinkID=213885).  

   1. Seçin **veritabanı** düğmesine tıklayın ve ardından SQL veritabanını, örneği oluşturmak için kullanılan abonelik düğümünü açın.
   2. SQL veritabanı kullanılabilir örneklerini görüntülemek için seçin **SQL veritabanları** düğümü.
   3. Veritabanının özelliklerini görüntülemek için veritabanını seçin. **Özellikleri** görünümü görüntülenir.

      > [!NOTE]
      > Varsa **özellikleri** görünüm görünmüyor, ayırıcıyı kullanarak açmanız gerekebilir.
      >
      >
   4. Bağlantı dizeleri görüntülenecek Görünüm'ün yanındaki üç nokta (...) düğmesini seçin.

      **Bağlantı dizeleri** iletişim kutusu görüntülenir.
   5. ADO.NET bağlantı dizesi kopyalamak için metni vurgulayın ve Ctrl + C tuşlarını seçin.
   6. İletişim kutusunu kapatmak için tercih **kapatmak** düğmesi.
4. SQL veritabanı, bu örneği kullanmak için web.config dosyasında bağlantı dizesini değiştirmek için web.config dosyasını açın, varolan bağlantı dizesi girişi vurgulayın ve ardından Ctrl + V tuşlarını seçin. SQL veritabanı örneği için ADO.NET bağlantı dizesi mevcut bağlantı dizesi değiştirir.
5. Parametresini eklemeniz gerekir `MultipleActiveResultSets=True` bağlantı dizesi. Bağlantı dizesi şu biçimde olmalıdır:

    ```
    connectionString=”Server=tcp:<database_server>.database.windows.net,1433;Database=<database_name>;User ID=<user_name>@<database_server>;Password=<myPassword>;Trusted_Connection=False;Encrypt=True;MultipleActiveResultSets=True"
    ```
6. (İsteğe bağlı) Doğrudan web.config dosyasında bağlantı dizesini değiştirmek için alternatif bir yöntem web.config dönüşümü dosyaların, hizmet paketi oluşturmak için kullandığınız yapı yapılandırmasına bağlı olarak tek bir bölüm eklemektir. Web.Debug.Config dosya veya Web.Release.Config dosyasını açın. Aşağıdaki bölümde bu dosyaya ekleyin:

    ```
    XMLCopy<connectionStrings><addname="DefaultConnection"connectionString="Server=tcp:<database_server>.database.windows.net,1433;Database=<database_name>;User ID=<user_name>@<database_server>;Password=<myPassword>;Trusted_Connection=False;Encrypt=True;MultipleActiveResultSets=True"xdt:Transform="SetAttributes"xdt:Locator="Match(name)"/></connectionStrings>
    ```
7. Değiştirdiğiniz dosyayı kaydedin ve uygulamanızı yeniden yayımlayın.

### <a name="to-use-an-instance-of-sql-database-by-using-the-azure-classic-portal"></a>Klasik Azure portalı kullanarak SQL veritabanı örneği kullanmak için
1. İçinde [Klasik Azure portalı](http://go.microsoft.com/fwlink/?LinkID=213885), SQL veritabanları düğümünü seçin.

   * Açmak kullanmak istediğiniz SQL veritabanı örneği varsa, seçin.
   * Hiç örneği oluşturmadıysanız, uygun bağlantıyı seçin ve ardından bir örneği oluşturun.
2. Sonra açın veya bir veritabanı örneği oluşturmayı seçin **bağlantı dizeleri** bağlantı.
3. Sayfanın alt kısmındaki güvenlik duvarı ayarlarını yapılandırmak ve varsayılan değerleri kabul edin veya gereksinim duyduğunuz değerleri yapılandırmak için bağlantıyı seçin.
4. ADO.NET bağlantı dizesini kopyalayın, üzerinden şirket içi veritabanı için eski bağlantı dizesi web.config dosyasına yapıştırın ve eklediğinizden emin olun `MultipleActiveResultSets=True`.

## <a name="publish-a-web-application-to-azure"></a>Azure Web uygulaması yayımlama
### <a name="to-publish-a-web-application-to-azure"></a>Bir Azure Web uygulamasına yayımlamak için
1. Uygulamayı yerel geliştirme test etmek için Azure kullanarak ortamı işlem öykünücüsü, web rolü için Azure projesi için kısayol menüsünü açın ve seçin **başlangıç projesi olarak ayarla**. Ardından **hata ayıklama**, **hata ayıklamayı Başlat** (klavye: **F5**).

    **Hata ayıklama Azure ortamı başlatmak** iletişim kutusu açılır ve tarayıcıda uygulamayı başlatır. İşlem öykünücüsü, web uygulamasında her tür nasıl başlatılacağı hakkında belirli Ayrıntılar için bu bölümdeki tablosuna bakın.
2. Uygulamanız için Azure Yayımlama hizmetlerini ayarlamak için bir Microsoft hesabı ve bir Azure aboneliğiniz olmalıdır. Services'i ayarlamak için şu konudaki adımları kullanın: [yayımlamak veya Visual Studio'dan Azure bir uygulamayı dağıtmak hazırlama](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md).
3. Azure web uygulamasına yayımlamak için web projesinin kısayol menüsünü açın ve seçin **Azure Yayımla**.

    **Azure uygulamasını Yayımla** iletişim kutusu açılır ve Visual Studio dağıtım işlemi başlatır. Uygulama yayımlama hakkında daha fazla bilgi için bkz **Visual Studio'dan Azure uygulamasını Yayımla** içinde [yayımlama Azure araçlarını kullanarak bir bulut hizmeti](vs-azure-tools-publishing-a-cloud-service.md).

   > [!NOTE]
   > Azure projesi web uygulamasından yayımlayabilirsiniz. Bunu yapmak için Azure projesi için kısayol menüsünü açın ve seçin **Yayımla**.
   >
   >
4. Dağıtımın ilerleme durumunu görmek için görüntüleyebileceğiniz **Azure etkinlik günlüğü** penceresi. Bu günlük, dağıtım işlemi başlatıldığında otomatik olarak görüntülenir. Aşağıdaki çizimde gösterildiği gibi ayrıntılı bilgileri görüntülemek için etkinlik günlüğünde satır öğeyi genişletebilirsiniz:

    ![VST_AzureActivityLog](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC744149.png)
5. (İsteğe bağlı) Dağıtım işlemi iptal etmek için etkinlik günlüğünde satır öğesi için kısayol menüsünü açın ve seçin **iptal edin ve kaldırma**. Bu dağıtım işlemi durdurur ve Azure'dan dağıtım ortamı siler.

   > [!NOTE]
   > Bunu dağıtıldıktan sonra bu dağıtım ortamı kaldırmak için kullanmanız gerekir [Klasik Azure portalı](http://go.microsoft.com/fwlink/?LinkID=213885).
   >
   >
6. (İsteğe bağlı) Rolü örneklerinizi başlattıktan sonra Visual Studio otomatik olarak dağıtım ortamında gösterir **Azure işlem** düğümünde **Cloud Explorer** veya **Sunucu Gezgini**. Buradan, tek tek rol örneklerinin durumunu görüntüleyebilirsiniz.

    Rol örnekleri aşağıda gösterilmiştir **Sunucu Gezgini** hala başlatılıyor durumda çalışırken:

    ![VST_DeployComputeNode](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC744134.png)
7. Dağıtımdan sonra uygulamaya erişmek için dağıtımınızın durumunu zaman yanındaki oka seçin **tamamlandı** görünür **Azure etkinlik günlüğü**. Bu, Azure içinde web uygulamanız için URL'yi görüntüler. Azure'dan belirli türdeki bir web uygulaması başlangıç hakkında ayrıntılı bilgi için aşağıdaki tabloya bakın.

    Azure'dan belirli web uygulamalarını başlatmak için çalıştırın veya bir web uygulamasını Azure işlem öykünücüsü kullanarak yerel olarak hata ayıklama hakkında ayrıntılar aşağıdaki tabloda listelenmektedir:

   | Web uygulaması türü | İşlem öykünücüsü kullanarak yerel olarak çalıştır/Hata Ayıkla | Azure'da çalışan |
   | --- | --- | --- |
   | ASP.NET Web uygulaması |Menü çubuğunda seçin **hata ayıklama**, **hata ayıklamayı Başlat** (klavye: seçin **F5** anahtar.). |Görüntülenen URL köprü seçin **dağıtım** sekmesinde **Azure etkinlik günlüğü** başlangıç sayfasını tarayıcıda yüklenemiyor. |
   | ASP.NET MVC 2 Web uygulaması |Menü çubuğunda seçin **hata ayıklama**, **hata ayıklamayı Başlat** (klavye: seçin **F5** anahtar.). |Görüntülenen URL köprü seçin **dağıtım** sekmesinde **Azure etkinlik günlüğü** başlangıç sayfasını tarayıcıda yüklenemiyor. |
   | ASP.NET MVC 3 Web uygulaması |Menü çubuğunda seçin **hata ayıklama**, **hata ayıklamayı Başlat** (klavye: seçin **F5** anahtar.). |Görüntülenen URL köprü seçin **dağıtım** sekmesinde **Azure etkinlik günlüğü** başlangıç sayfasını tarayıcıda yüklenemiyor. |
   | ASP.NET MVC 4 Web uygulaması |Menü çubuğunda seçin **hata ayıklama**, **hata ayıklamayı Başlat** (klavye: seçin **F5** anahtar.). |Görüntülenen URL köprü seçin **dağıtım** sekmesinde **Azure etkinlik günlüğü** başlangıç sayfasını tarayıcıda yüklenemiyor. |
   | ASP.NET boş Web uygulaması |Web projeniz için başlangıç sayfası olarak ayarlamak, uygulamanızdaki bir .aspx sayfasında eklemeniz gerekir. Menü çubuğunda seçin **hata ayıklama**, **hata ayıklamayı Başlat** (klavye: seçin **F5** anahtar.). |Uygulamanıza varsayılan bir .aspx sayfası varsa, görüntülenen URL köprü seçin **dağıtım** sekmesinde **Azure etkinlik günlüğü** ve bu sayfayı tarayıcıda yüklenir. Farklı .aspx sayfa varsa, URL'nizi için aşağıdaki biçimi kullanarak bu belirli sayfasına gitmeniz gerekir:`<url for deployment>/<name of page>.aspx` |
   | Silverlight uygulaması |Menü çubuğunda seçin **hata ayıklama**, **hata ayıklamayı Başlat** (klavye: seçin **F5** anahtar.). |URL'niz için aşağıdaki biçimi kullanarak, uygulamanız için belirli bir sayfaya gitmek gerekir:`<url for deployment>/<name of page>.aspx` |
   | Silverlight iş uygulaması |Menü çubuğunda seçin **hata ayıklama**, **hata ayıklamayı Başlat** (klavye: seçin **F5** anahtar.). |URL'niz için aşağıdaki biçimi kullanarak, uygulamanız için belirli bir sayfaya gitmek gerekir:`<url for deployment>/<name of page>.aspx` |
   | Silverlight gezinti uygulaması |Menü çubuğunda seçin **hata ayıklama**, **hata ayıklamayı Başlat** (klavye: seçin **F5** anahtar.). |URL'niz için aşağıdaki biçimi kullanarak, uygulamanız için belirli bir sayfaya gitmek gerekir:`<url for deployment>/<name of page>.aspx` |
   | WCF hizmet uygulaması |WCF Hizmeti projeniz için .svc dosyasındaki başlangıç sayfası olarak ayarlamanız gerekir. Menü çubuğunda seçin **hata ayıklama**, **hata ayıklamayı Başlat** (klavye: seçin **F5** anahtar.). |URL'niz için aşağıdaki biçimi kullanarak, uygulamanız için svc dosyaya gidin gerekir:`<url for deployment>/<name of service file>.svc` |
   | WCF iş akışı hizmeti uygulaması |WCF Hizmeti projeniz için .svc dosyasındaki başlangıç sayfası olarak ayarlamanız gerekir. Menü çubuğunda seçin **hata ayıklama**, **hata ayıklamayı Başlat** (klavye: seçin **F5** anahtar.). |URL'niz için aşağıdaki biçimi kullanarak, uygulamanız için svc dosyaya gidin gerekir:`<url for deployment>/<name of service file>.svc` |
   | ASP.NET dinamik varlıklar |Menü çubuğunda seçin **hata ayıklama**, **hata ayıklamayı Başlat** (klavye: seçin **F5** anahtar.). |Bağlantı dizesini güncellemeniz gerekir (sonraki bölüme bakın). Ayrıca, url için aşağıdaki biçimi kullanarak, uygulamanız için belirli bir sayfaya gitmek gerekir:`<url for deployment>/<name of page>.aspx` |
   | ASP.NET dinamik veri LINQ-SQL |Menü çubuğunda seçin **hata ayıklama**, **hata ayıklamayı Başlat** (klavye: seçin **F5** anahtar.). |Bu yordamdaki adımları izlemelisiniz: uygulamanız için bir SQL Azure veritabanı kullanın (Bu konuda önceki bölüme bakın). Ayrıca, url için aşağıdaki biçimi kullanarak, uygulamanız için belirli bir sayfaya gitmek gerekir:`<url for deployment>/<name of page>.aspx` |

## <a name="update-a-connection-string-for-aspnet-dynamic-entities"></a>ASP.NET dinamik varlıklar için bir bağlantı dizesi güncelleştir
### <a name="to-update-a-connection-string-for-aspnet-dynamic-entities"></a>ASP.NET dinamik varlıklar için bir bağlantı dizesi güncelleştirmek için
1. ASP.NET dinamik varlıklar web uygulaması için kullanılabilir bir SQL Azure veritabanı oluşturmak için yordamdaki adımları **uygulamanız için bir SQL Azure veritabanı kullan** bu konuda daha önce.
2. Tabloları ve bu veritabanından için gereksinim duyduğunuz alanları ekleme [Klasik Azure portalı](http://go.microsoft.com/fwlink/?LinkID=213885).
3. Bu tür bir uygulama için bağlantı dizesi web.config dosyasında aşağıdaki biçime sahiptir:  

    ```
    <addname="tempdbEntities"connectionString="metadata=res://*/Model1.csdl|res://*/Model1.ssdl|res://*/Model1.msl;provider=System.Data.SqlClient;provider connection string=&quot;data source=<server name>\SQLEXPRESS;initial catalog=<database name>;integrated security=True;multipleactiveresultsets=True;App=EntityFramework&quot;"providerName="System.Data.EntityClient"/>
    ```

    Güncelleştirme *connectionString* SQL Azure veritabanı ADO.NET bağlantı dizesiyle gibi değer:

    ```
    XMLCopy<addname="tempdbEntities"connectionString="metadata=res://*/Model1.csdl|res://*/Model1.ssdl|res://*/Model1.msl;provider=System.Data.SqlClient;provider connection string=&quot;Server=tcp:<SQL Azure server name>.database.windows.net,1433;Database=<database name>;User ID=<user name>;Password=<password>;Trusted_Connection=False;Encrypt=True;multipleactiveresultsets=True;App=EntityFramework&quot;"providerName="System.Data.EntityClient"/>
    ```
4. Web.config dosyasında bağlantı dizesine yaptığınız değişiklikleri kaydetmek için menü çubuğunda seçin **dosya**, **web.config kaydetmek**.

## <a name="supported-project-templates"></a>Desteklenen proje şablonları
Bir Azure web uygulamasına yayımlamak için uygulama proje şablonlarından birini C# veya Visual Basic, aşağıdaki tabloda listelenen için kullanmanız gerekir.

| Proje şablonu grubu | Proje şablonu |
| --- | --- |
| Web |ASP.NET Web uygulaması |
| Web |ASP.NET MVC 2 Web uygulaması |
| Web |ASP.NET MVC 3 Web uygulaması |
| Web |ASP.NET MVC4 Web uygulaması |
| Web |ASP.NET boş Web uygulaması |
| Web |ASP.NET MVC 2 boş Web uygulaması |
| Web |ASP.NET dinamik veri varlıkları Web uygulaması |
| Web |ASP.NET dinamik veri LINQ-SQL Web uygulaması |
| Silverlight |Silverlight uygulaması |
| Silverlight |Silverlight iş uygulaması |
| Silverlight |Silverlight gezinti uygulaması |
| WCF |WCF hizmet uygulaması |
| WCF |WCF iş akışı hizmeti uygulaması |
| İş akışı |WCF iş akışı hizmeti uygulaması |

## <a name="next-steps"></a>Sonraki Adımlar
Yayımlama hakkında daha fazla bilgi için bkz: [yayımlamak veya Visual Studio'dan Azure uygulama dağıtmak hazırlama](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md). Ayrıca kullanıma [ayarı yukarı adlı kimlik doğrulama bilgilerini](vs-azure-tools-setting-up-named-authentication-credentials.md).
