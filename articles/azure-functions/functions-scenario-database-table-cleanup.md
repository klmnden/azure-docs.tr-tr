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
ms.date: 05/22/2017
ms.author: glenga
ms.openlocfilehash: 024958d8a548313b53fc24ade5805de036a89afb
ms.sourcegitcommit: 8e06d67ea248340a83341f920881092fd2a4163c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49351924"
---
# <a name="use-azure-functions-to-connect-to-an-azure-sql-database"></a>Bir Azure SQL veritabanı'na bağlanmak için Azure işlevleri'ni kullanın
Bu konu Azure SQL veritabanı tablosundaki satırları temizler, zamanlanmış bir iş oluşturmak için Azure işlevleri kullanmayı gösterir. Yeni C# betik işlevi Azure portalında bir önceden tanımlanmış bir zamanlayıcı tetikleyici şablonunu temel alınarak oluşturulur. Bu senaryoyu desteklemek için ayrıca bir veritabanı bağlantı dizesi bir işlev uygulaması uygulama ayarı olarak ayarlamanız gerekir. Bu senaryo, veritabanında bir toplu işlemi kullanır. 

İşlev işlemi bireysel için oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemleri bir Mobile Apps tablodaki, bunun yerine kullanmanız gerektiğini [Mobile Apps bağlamaları](functions-bindings-mobile-apps.md).

> [!IMPORTANT]
> Bu belgedeki örnekler 1.x çalışma zamanı için geçerlidir. 1.x işlev uygulaması oluşturma hakkında bilgi [burada bulunabilir](./functions-versions.md#creating-1x-apps).

## <a name="prerequisites"></a>Önkoşullar

+ Bu konuda, bir Zamanlayıcı ile tetiklenen işlevi kullanır. Bu konu başlığındaki adımları tamamlamak [azure'da bir Zamanlayıcı tarafından tetiklenen bir işlev oluşturma](functions-create-scheduled-function.md) bu işlev, C# sürümünü oluşturmak için.   

+ Bu konuda, bir toplu temizleme işlemi yürüten bir Transact-SQL komutu gösterilir **SalesOrderHeader** AdventureWorksLT örnek veritabanı tablosunda. AdventureWorksLT örnek veritabanını oluşturmak için konu başlığındaki adımları tamamlamak [Azure portalında bir Azure SQL veritabanı oluşturma](../sql-database/sql-database-get-started-portal.md). 

## <a name="get-connection-information"></a>Bağlantı bilgilerini alma

Tamamlandığında, oluşturulan veritabanı için bağlantı dizesini almak gereken [Azure portalında bir Azure SQL veritabanı oluşturma](../sql-database/sql-database-get-started-portal.md).

1. [Azure Portal](https://portal.azure.com/)’da oturum açın.
 
3. Seçin **SQL veritabanları** sol menüdeki ve veritabanınızı seçin **SQL veritabanları** sayfası.

4. Seçin **veritabanı bağlantı dizelerini Göster** ve tam kopyalayın **ADO.NET** bağlantı dizesi. 

    ![ADO.NET bağlantı dizesini kopyalayın.](./media/functions-scenario-database-table-cleanup/adonet-connection-string.png)

## <a name="set-the-connection-string"></a>Bağlantı dizesini ayarlama 

Azure'da işlevlerinizin yürütülmesini bir işlev uygulaması barındırır. Bağlantı dizelerini ve diğer gizli dizileri, işlev uygulaması ayarları içinde saklamak için en iyi bir uygulamadır. Uygulama ayarlarını kullanarak kodunuzu bağlantı dizesiyle yanlışlıkla açıklanması engeller. 

1. Oluşturduğunuz işlev uygulamanıza gidin [azure'da bir Zamanlayıcı tarafından tetiklenen bir işlev oluşturma](functions-create-scheduled-function.md).

2. Seçin **Platform özellikleri** > **uygulama ayarları**.
   
    ![İşlev uygulaması için uygulama ayarları.](./media/functions-scenario-database-table-cleanup/functions-app-service-settings.png)

2. Ekranı aşağı kaydırarak **bağlantı dizelerini** ve tabloda belirtilen ayarları kullanarak bir bağlantı dizesi ekleyin.
   
    ![İşlev uygulaması ayarları için bir bağlantı dizesi ekleyin.](./media/functions-scenario-database-table-cleanup/functions-app-service-settings-connection-strings.png)

    | Ayar       | Önerilen değer | Açıklama             | 
    | ------------ | ------------------ | --------------------- | 
    | **Ad**  |  sqldb_connection  | İşlev kodunuzda depolanan bağlantı dizesini erişmek için kullanılır.    |
    | **Değer** | Kopyalanan dize  | Önceki bölümde kopyaladığınız bağlantı dizesini yapıştırın ve Değiştir `{your_username}` ve `{your_password}` yer tutucuları gerçek değerlerle. |
    | **Tür** | SQL Veritabanı | Varsayılan SQL veritabanı bağlantısını kullanın. |   

3. **Kaydet**’e tıklayın.

Şimdi, SQL veritabanınıza bağlanır C# işlev kodunu ekleyebilirsiniz.

## <a name="update-your-function-code"></a>İşlev kodunuzu güncelleştirin

1. İşlev uygulamanızda Portalı'nda, Zamanlayıcı ile tetiklenen işlevi seçin.
 
3. Mevcut C# betik işlev kodunu üstüne aşağıdaki derleme başvurularını ekleyin:

    ```cs
    #r "System.Configuration"
    #r "System.Data"
    ```
    >[!NOTE]
    >Bu örneklerde kod C# betiği portaldan var. Önceden derlenmiş C# işlevi yerel olarak geliştirirken, bunun yerine bu başvuruları çeviren yerel projenize eklemeniz gerekir.  

3. Aşağıdaki `using` deyimleri işlevi için:
    ```cs
    using System.Configuration;
    using System.Data.SqlClient;
    using System.Threading.Tasks;
    ```

4. Varolan `Run` işlevi aşağıdaki kod ile:
    ```cs
    public static async Task Run(TimerInfo myTimer, TraceWriter log)
    {
        var str = ConfigurationManager.ConnectionStrings["sqldb_connection"].ConnectionString;
        using (SqlConnection conn = new SqlConnection(str))
        {
            conn.Open();
            var text = "UPDATE SalesLT.SalesOrderHeader " + 
                    "SET [Status] = 5  WHERE ShipDate < GetDate();";

            using (SqlCommand cmd = new SqlCommand(text, conn))
            {
                // Execute the command and log the # rows affected.
                var rows = await cmd.ExecuteNonQueryAsync();
                log.Info($"{rows} rows were updated");
            }
        }
    }
    ```

    Bu örnek komut güncelleştirmeleri `Status` sevk tarih sütununa göre. 32 veri satırlarını güncelleştirmeniz.

5. Tıklayın **Kaydet**, watch **günlükleri** windows için sonraki işlev yürütme ve ardından, güncelleştirilen satırların sayısı unutmayın **SalesOrderHeader** tablo.

    ![İşlev günlüklerini görüntüleyin.](./media/functions-scenario-database-table-cleanup/functions-logs.png)

## <a name="next-steps"></a>Sonraki adımlar

Ardından, İşlevler ile Logic Apps diğer hizmetlerle tümleştirme için nasıl kullanılacağını öğrenin.

> [!div class="nextstepaction"] 
> [Logic Apps ile tümleşen bir işlev oluşturma](functions-twitter-email.md)

İşlevler hakkında daha fazla bilgi için aşağıdaki konulara bakın:

* [Azure İşlevleri geliştirici başvurusu](functions-reference.md)  
  İşlevleri kodlamak ve tetikleyicileri ve bağlamaları tanımlamak için programcı başvurusu
* [Azure İşlevlerini test etme](functions-test-a-function.md)  
  İşlevlerinizi test etmek için çeşitli araçları ve teknikleri açıklar.  
