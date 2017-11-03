---
title: "Bir veritabanı temizleme görevini gerçekleştirmek için Azure işlevleri kullanın | Microsoft Docs"
description: "Azure işlevleri, düzenli aralıklarla satırları temizlemek için Azure SQL veritabanına bağlanan bir görev zamanlamak için kullanın."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
ms.assetid: 076f5f95-f8d2-42c7-b7fd-6798856ba0bb
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/22/2017
ms.author: glenga
ms.openlocfilehash: ac0399867e0cdab1825022c4ed73ce003cc8c7e6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-azure-functions-to-connect-to-an-azure-sql-database"></a>Bir Azure SQL veritabanına bağlanmak için Azure işlevleri kullanın
Bu konu Azure işlevleri, bir Azure SQL veritabanındaki bir tablodaki satırların temizler zamanlanmış bir işi oluşturmak için nasıl kullanılacağını gösterir. Yeni C# işlevi önceden tanımlanmış Zamanlayıcı tetikleyicisi şablonu Azure portalında temel alınarak oluşturulur. Bu senaryoyu desteklemek için de bir işlev uygulaması ayarında olarak bir veritabanı bağlantı dizesi ayarlamanız gerekir. Bu senaryo, bir toplu işlemin veritabanında kullanır. Mobile Apps tablodaki tek tek CRUD işlemlerini işlevinizi sağlamak için kullanmanız [Mobile Apps bağlamaları](functions-bindings-mobile-apps.md).

## <a name="prerequisites"></a>Ön koşullar

+ Bu konuda bir zamanlayıcı tetiklenen işlevini kullanır. Konusundaki adımları tamamlamak [zamanlayıcısı tarafından tetiklenen Azure işlevi oluşturma](functions-create-scheduled-function.md) bu işlev, C# sürümünü oluşturmak için.   

+ Bu konuda bir toplu temizleme işlemi yürüten bir Transact-SQL komutu gösterilir **SalesOrderHeader** AdventureWorksLT örnek veritabanı tablosunda. AdventureWorksLT örnek veritabanı oluşturmak için konusundaki adımları [Azure portalında bir Azure SQL veritabanı oluşturma](../sql-database/sql-database-get-started-portal.md). 

## <a name="get-connection-information"></a>Bağlantı bilgilerini alma

Tamamlandığında, oluşturulan veritabanı için bağlantı dizesini almak gereken [Azure portalında bir Azure SQL veritabanı oluşturma](../sql-database/sql-database-get-started-portal.md).

1. [Azure Portal](https://portal.azure.com/)’da oturum açın.
 
3. Seçin **SQL veritabanları** sol taraftaki menüden ve veritabanınızı seçin **SQL veritabanları** sayfası.

4. Seçin **veritabanı bağlantı dizelerini Göster** ve tam kopyalayın **ADO.NET** bağlantı dizesi.

    ![ADO.NET bağlantı dizesini kopyalayın.](./media/functions-scenario-database-table-cleanup/adonet-connection-string.png)

## <a name="set-the-connection-string"></a>Bağlantı dizesi ayarlama 

Azure'da işlevlerinizin yürütülmesini bir işlev uygulaması barındırır. Bağlantı dizeleri ve diğer parolaları işlevi uygulama ayarlarınızı depolamak için en iyi bir uygulamadır. Uygulama ayarları kullanarak kodunuzu bağlantı dizesiyle yanlışlıkla açıklanması engeller. 

1. Oluşturduğunuz işlevi uygulamanıza gidin [zamanlayıcısı tarafından tetiklenen Azure işlevi oluşturma](functions-create-scheduled-function.md).

2. Seçin **Platform özellikleri** > **uygulama ayarları**.
   
    ![İşlev uygulaması için uygulama ayarları.](./media/functions-scenario-database-table-cleanup/functions-app-service-settings.png)

2. Ekranı aşağı kaydırarak **bağlantı dizeleri** ve tabloda belirtildiği gibi ayarları kullanarak bir bağlantı dizesi ekleyin.
   
    ![Bir bağlantı dizesi işlevi uygulama ayarlarına ekleyin.](./media/functions-scenario-database-table-cleanup/functions-app-service-settings-connection-strings.png)

    | Ayar       | Önerilen değer | Açıklama             | 
    | ------------ | ------------------ | --------------------- | 
    | **Ad**  |  sqldb_connection  | İşlev kodunuzu depolanan bağlantı dizesinde erişmek için kullanılır.    |
    | **Değer** | Kopyalanan dize  | Bağlantı dizesi önceki bölümde kopyalanır. |
    | **Tür** | SQL Veritabanı | Varsayılan SQL veritabanı bağlantısı kullanın. |   

3. **Kaydet** düğmesine tıklayın.

Şimdi, SQL veritabanına bağlanan C# işlev kodu ekleyebilirsiniz.

## <a name="update-your-function-code"></a>İşlev kodunuzu güncelleştirin

1. İşlev uygulamanızda Zamanlayıcı tetiklenen işlevi seçin.
 
3. Varolan işlev kodu en üstte aşağıdaki derleme başvurularını ekleyin:

    ```cs
    #r "System.Configuration"
    #r "System.Data"
    ```

3. Aşağıdakileri ekleyin `using` deyimleri işlevi için:
    ```cs
    using System.Configuration;
    using System.Data.SqlClient;
    using System.Threading.Tasks;
    ```

4. Varolan **çalıştırmak** işlevi aşağıdaki kod ile:
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

    Bu örnek komut güncelleştirir **durum** sütun temel sevk tarih. Veri 32 satırı güncelleştirmeniz gerekir.

5. Tıklatın **kaydetmek**, izleme **günlükleri** sonraki windows işlev yürütme ve ardından güncelleştirilir satır sayısını Not **SalesOrderHeader** tablo.

    ![İşlev günlüklerine bakın.](./media/functions-scenario-database-table-cleanup/functions-logs.png)

## <a name="next-steps"></a>Sonraki adımlar

Ardından, işlevleri Logic Apps ile diğer hizmetleri ile tümleştirme için nasıl kullanılacağını öğrenin.

> [!div class="nextstepaction"] 
> [Logic Apps ile tümleşen bir işlev oluşturun](functions-twitter-email.md)

İşlevler hakkında daha fazla bilgi için aşağıdaki konulara bakın:

* [Azure İşlevleri geliştirici başvurusu](functions-reference.md)  
  İşlevleri kodlamak ve tetikleyicileri ve bağlamaları tanımlamak için programcı başvurusu
* [Azure İşlevlerini test etme](functions-test-a-function.md)  
  İşlevlerinizi test etmek için çeşitli araçları ve teknikleri açıklar.  
