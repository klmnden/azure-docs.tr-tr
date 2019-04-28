---
title: Bir veritabanı temizleme görevini gerçekleştirmek için Azure işlevlerini kullanın. | Microsoft Docs
description: Satırları düzenli aralıklarla temizlemek için Azure SQL veritabanına bağlanan bir görevi zamanlamak için Azure işlevlerini kullanın.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
ms.assetid: 076f5f95-f8d2-42c7-b7fd-6798856ba0bb
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 10/28/2018
ms.author: glenga
ms.openlocfilehash: 4ec2e9b931e6405aca5b4237bc044647af3b8bb3
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62120679"
---
# <a name="use-azure-functions-to-connect-to-an-azure-sql-database"></a>Bir Azure SQL veritabanı'na bağlanmak için Azure işlevleri'ni kullanın

Bu makalede bir Azure SQL veritabanı örneğine bağlanan zamanlanmış bir iş oluşturmak için Azure işlevleri kullanmayı gösterir. İşlev kodu bir veritabanı tablosundaki satırları siler. Yeni C# işlevi, bir Visual Studio 2017'de önceden tanımlı bir zamanlayıcı tetikleyici şablonunu temel alarak oluşturulur. Bu senaryoyu desteklemek için ayrıca bir veritabanı bağlantı dizesi bir işlev uygulaması uygulama ayarı olarak ayarlamanız gerekir. Bu senaryo, veritabanında bir toplu işlemi kullanır. 

Bu çalışma ilk deneyiminizi ise C# İşlevler, kimler [Azure işlevleri C# Geliştirici Başvurusu](functions-dotnet-class-library.md).

## <a name="prerequisites"></a>Önkoşullar

+ Makaledeki adımları tamamlayabilmeniz [Visual Studio kullanarak ilk işlevinizi oluşturma](functions-create-your-first-function-visual-studio.md) sürüm 2.x çalışma zamanını hedefleyen bir yerel işlev uygulaması oluşturmak için. Ayrıca, projenizi azure'da bir işlev uygulaması yayımladığınız gerekir.

+ Bu makalede, bir toplu temizleme işlemi yürüten bir Transact-SQL komut gösterilmektedir **SalesOrderHeader** AdventureWorksLT örnek veritabanı tablosunda. AdventureWorksLT örnek veritabanı oluşturmak için bu makaledeki adımları tamamlamanız [Azure portalında bir Azure SQL veritabanı oluşturma](../sql-database/sql-database-get-started-portal.md).

+ Eklemelisiniz bir [sunucu düzeyinde güvenlik duvarı kuralı](../sql-database/sql-database-get-started-portal-firewall.md) Bu Hızlı Başlangıç için kullanacağınız bilgisayarın genel IP adresi için. Bu kural mümkün erişim yerel bilgisayarınızdan bir SQL veritabanı örneği olması gerekir.  

## <a name="get-connection-information"></a>Bağlantı bilgilerini alma

Tamamlandığında, oluşturulan veritabanı için bağlantı dizesini almak gereken [Azure portalında bir Azure SQL veritabanı oluşturma](../sql-database/sql-database-get-started-portal.md).

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. Seçin **SQL veritabanları** sol menüdeki ve veritabanınızı seçin **SQL veritabanları** sayfası.

1. Seçin **bağlantı dizeleri** altında **ayarları** ve tam kopyalayın **ADO.NET** bağlantı dizesi.

    ![ADO.NET bağlantı dizesini kopyalayın.](./media/functions-scenario-database-table-cleanup/adonet-connection-string.png)

## <a name="set-the-connection-string"></a>Bağlantı dizesini ayarlama

Azure'da işlevlerinizin yürütülmesini bir işlev uygulaması barındırır. En iyi güvenlik uygulaması olarak, bağlantı dizelerini ve diğer gizli dizileri, işlev uygulaması ayarları içinde depolar. Uygulama ayarlarını kullanarak kodunuzu bağlantı dizesiyle yanlışlıkla açıklanması engeller. Doğrudan Visual Studio'dan işlev uygulamanız için uygulama ayarlarına erişebilirsiniz.

Daha önce uygulamanızı Azure'a yayımladığınız gerekir. Bunu zaten bunu yapmadıysanız [işlev uygulamanızı Azure'a yayımlama](functions-develop-vs.md#publish-to-azure).

1. Çözüm Gezgini'nde işlevi uygulama projesine sağ tıklayın ve seçin **Yayımla** > **uygulama ayarlarını yönet...** . Seçin **ekleme ayarı**, **yeni uygulama ayarı adı**, türü `sqldb_connection`seçip **Tamam**.

    ![İşlev uygulaması için uygulama ayarları.](./media/functions-scenario-database-table-cleanup/functions-app-service-add-setting.png)

1. Yeni **sqldb_connection** ayarını önceki bölümünde kopyaladığınız bağlantı dizesini yapıştırın **yerel** değiştirin ve alan `{your_username}` ve `{your_password}` gerçek yer tutucuları değerler. Seçin **yerel bilgisayardan değer Ekle** güncelleştirilmiş değeri içine kopyalamak için **uzak** alan ve ardından **Tamam**.

    ![SQL bağlantı dizesi ayarı ekleyin.](./media/functions-scenario-database-table-cleanup/functions-app-service-settings-connection-string.png)

    Bağlantı dizelerini Azure'da şifrelenmiş olarak depolanır (**uzak**). Sızdıran gizli local.settings.json proje dosyası önlemek (**yerel**) kaynak denetiminden gibi bir .gitignore dosyası kullanarak bırakılmalıdır.

## <a name="add-the-sqlclient-package-to-the-project"></a>SqlClient paketini projeye ekleyin.

SqlClient kitaplığını içeren NuGet paketini eklemeniz gerekir. Bu veri erişim kitaplığı, bir SQL veritabanına bağlanmak için gereklidir.

1. Visual Studio 2017'de yerel işlev uygulaması projenizi açın.

1. Çözüm Gezgini'nde işlevi uygulama projesine sağ tıklayın ve seçin **NuGet paketlerini Yönet**.

1. **Gözat** sekmesinde ```System.Data.SqlClient``` öğesini arayın ve bulduğunuzda seçin.

1. İçinde **System.Data.SqlClient** sayfası, select sürüm `4.5.1` ve ardından **yükleme**.

1. Yükleme tamamlandığında değişiklikleri gözden geçirin ve **Önizleme** penceresini kapamak için **Tamam**’a tıklayın.

1. **Lisans Kabulü** penceresi gösterilirse **Kabul Ediyorum**’a tıklayın.

Şimdi, SQL veritabanınıza bağlanır C# işlev kodunu ekleyebilirsiniz.

## <a name="add-a-timer-triggered-function"></a>Zamanlayıcı ile tetiklenen işlev ekleme

1. Çözüm Gezgini'nde işlevi uygulama projesine sağ tıklayın ve seçin **Ekle** > **yeni Azure işlevi**.

1. İle **Azure işlevleri** şablon seçilmedi, aşağıdakine benzer yeni öğe adı `DatabaseCleanup.cs` seçip **Ekle**.

1. İçinde **yeni Azure işlevi** iletişim kutusunda **Zamanlayıcı tetikleyicisi** ardından **Tamam**. Bu iletişim kutusu, Zamanlayıcı ile tetiklenen işlev için bir kod dosyası oluşturur.

1. Yeni kod dosyasını açın ve aşağıdaki using deyimlerini dosyanın üstüne:

    ```cs
    using System.Data.SqlClient;
    using System.Threading.Tasks;
    ```

1. Varolan `Run` işlevi aşağıdaki kod ile:

    ```cs
    [FunctionName("DatabaseCleanup")]
    public static async Task Run([TimerTrigger("*/15 * * * * *")]TimerInfo myTimer, ILogger log)
    {
        // Get the connection string from app settings and use it to create a connection.
        var str = Environment.GetEnvironmentVariable("sqldb_connection");
        using (SqlConnection conn = new SqlConnection(str))
        {
            conn.Open();
            var text = "UPDATE SalesLT.SalesOrderHeader " +
                    "SET [Status] = 5  WHERE ShipDate < GetDate();";

            using (SqlCommand cmd = new SqlCommand(text, conn))
            {
                // Execute the command and log the # rows affected.
                var rows = await cmd.ExecuteNonQueryAsync();
                log.LogInformation($"{rows} rows were updated");
            }
        }
    }
    ```

    Bu işlev her 15 güncelleştirmek için saniyede çalışan `Status` sevk tarih sütununa göre. Zamanlayıcı tetikleyicisi hakkında daha fazla bilgi için bkz: [için Azure işlevleri Zamanlayıcı tetikleyicisi](functions-bindings-timer.md).

1. Tuşuna **F5** işlev uygulamasını başlatmak için. [Azure işlevleri çekirdek Araçları](functions-develop-local.md) Visual Studio yürütme penceresi açılır.

1. Başlangıç sonra 15 saniyede işlevi çalıştırır. Çıkış izleyin ve güncelleştirilmiştir satır sayısını not edin **SalesOrderHeader** tablo.

    ![İşlev günlüklerini görüntüleyin.](./media/functions-scenario-database-table-cleanup/function-execution-results-log.png)

    İlk çalıştırma üzerinde 32 veri satırlarını güncelleştirmeniz gerekir. Böylece daha fazla satır tarafından seçilir SalesOrderHeader tablo verileri değişiklik yapmadığınız sürece hiçbir veri satırı güncelleştirme aşağıdaki çalışmaları `UPDATE` deyimi.

Eğer [bu işlev yayımlama](functions-develop-vs.md#publish-to-azure), değiştirmeyi unutmayın `TimerTrigger` öznitelik daha makul [cron zamanlama](functions-bindings-timer.md#cron-expressions) her 15 saniyede daha.

## <a name="next-steps"></a>Sonraki adımlar

Ardından, nasıl kullanılacağını öğrenin. Diğer hizmetlerle tümleştirme için Logic Apps ile işlevler.

> [!div class="nextstepaction"]
> [Logic Apps ile tümleşen bir işlev oluşturma](functions-twitter-email.md)

İşlevler hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

+ [Azure İşlevleri geliştirici başvurusu](functions-reference.md)  
  İşlevleri kodlamak ve tetikleyicileri ve bağlamaları tanımlamak için programcı başvurusu
+ [Azure İşlevlerini test etme](functions-test-a-function.md)  
  İşlevlerinizi test etmek için çeşitli araçları ve teknikleri açıklar.  
