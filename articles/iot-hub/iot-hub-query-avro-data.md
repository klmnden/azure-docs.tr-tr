---
title: Azure Data Lake Analytics kullanarak Avro verileri Sorgulama | Microsoft Docs
description: Blob depolamaya ve BLOB depolamaya yazılan Avro biçimi veri sorgulama için rota cihaz telemetrisi ileti gövdesi özelliklerini kullanın.
services: iot-hub
documentationcenter: ''
author: ksaye
manager: obloch
ms.service: iot-hub
ms.topic: article
ms.date: 05/29/2018
ms.author: Kevin.Saye
ms.openlocfilehash: 98a30155c73a937042b4bea6568543fb5152d748
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34728082"
---
# <a name="query-avro-data-using-azure-data-lake-analytics"></a>Azure Data Lake Analytics kullanarak Avro verileri Sorgulama

Bu makale Azure IOT Hub iletilerden Azure hizmetlerine verimli bir şekilde yönlendirme sorgu Avro verileri nasıl hakkındadır. Blog yayını duyurusuna aşağıdaki —[Azure IOT hub'ı ileti yönlendirme: ileti gövdesinde yönlendirme ile artık], özellikleri veya ileti gövdesi yönlendirme IOT hub'ı destekler. Ayrıca bkz. [İleti gövdeleri yönlendirme][Routing on message bodies]. 

Azure IOT Hub BLOB depolamaya iletileri yönlendirirken challenge, bırakıldı, IOT hub'ı hem ileti gövdesi, hem de ileti özellikleri Avro biçiminde içeriği yazar. IOT Hub, yalnızca BLOB depolamaya Avro veri biçiminde veri yazma destekler ve bu biçim için diğer tüm uç noktaları kullanılmaz unutmayın. Bkz: [Azure Storage kapsayıcıları kullanırken][When using Azure storage containers]. Avro biçimi veri/iletisi korunması için harika olsa da, veri sorgulama için zordur. Buna karşılık, JSON veya CSV biçiminde veri sorgulama için çok daha kolaydır.

Bunu çözmek için çok büyük veri desenleri dönüştürme hem veri ölçekleme için adres ilişkisel olmayan büyük veri gereksinimlerini ve biçimleri için kullanabilirsiniz. "Sorgu başına ödeme" deseni desenleri Azure Data Lake Analytics (ADLA) biridir. Bu, bu makalenin odak noktasıdır. Hadoop veya diğer çözümleri kolayca sorgu yürütebilir ancak ADLA genellikle daha iyi "sorgu ödeme" Bu yaklaşım için uygundur. U-SQL Avro için bir "ayıklayıcısı" dir. Bkz: [U-SQL Avro örneği].

## <a name="query-and-export-avro-data-to-a-csv-file"></a>Sorgulamak ve Avro verileri bir CSV dosyasına dışarı aktarma
Verileri diğer depoları veya veri depolarında kolayca yerleştirin ancak bölüm Avro verileri sorgulamak ve Azure Blob Depolama, CSV dosyasına dışarı aktarma anlatılmaktadır.

1. İletileri seçmek için ileti gövdesinde bir özelliğini kullanarak bir Azure Blob Storage uç nokta için rota verileri için Azure IOT Hub ayarlayın.

    ![Adım 1a ekran yakalama][img-query-avro-data-1a]

    ![Adım 1b ekran yakalama][img-query-avro-data-1b]

2. Cihazınızı kodlama, içerik türü ve gerekli tüm verileri özellikleri ya da ileti gövdesi olarak başvurulan ürün belgelerinde olduğundan emin olun. (Aşağıya bakın) cihaz Explorer'da görüntülendiğinde bu öznitelikler ayarlandığını doğrulayabilirsiniz.

    ![2. adım ekran yakalama][img-query-avro-data-2]

3. Bir Azure veri Gölü deposu (ADLS) ve Azure Data Lake Analytics örneği ayarlayın. Azure IOT hub'ı bir Azure Data Lake Store yönlendirmez olmakla birlikte, bir ADLA gerektirir.

    ![3. adım ekran yakalama][img-query-avro-data-3]

4. ADLA içinde Azure Blob Depolama verileri için Azure IOT Hub yönlendirir Blob Storage ek bir deposu olarak yapılandırın.

    ![4. adım ekran yakalama][img-query-avro-data-4]
 
5. ' Da anlatıldığı gibi [U-SQL Avro örneği], gereken 4 DLL'leri vardır.  Bu dosyalar, ADLS bir konumda karşıya yükleyin.

    ![5. adım ekran yakalama][img-query-avro-data-5] 

6. Visual Studio'da U-SQL projesi oluşturma
 
    ![Adım 6 için ekran yakalama][img-query-avro-data-6]

7. Aşağıdaki komut dosyası içeriği Kopyala ve yeni oluşturulan dosyaya yapıştırın. 3 vurgulanan bölümler değiştirin: ADLA hesabınızı, ilişkili DLL'ler yolları ve depolama hesabınız için doğru yolu.
    
    ![Adım 7a ekran yakalama][img-query-avro-data-7a]

    CSV için basit çıktı gerçek U-SQL betiği:
    
    ```sql
        DROP ASSEMBLY IF EXISTS [Avro];
        CREATE ASSEMBLY [Avro] FROM @"/Assemblies/Avro/Avro.dll";
        DROP ASSEMBLY IF EXISTS [Microsoft.Analytics.Samples.Formats];
        CREATE ASSEMBLY [Microsoft.Analytics.Samples.Formats] FROM @"/Assemblies/Avro/Microsoft.Analytics.Samples.Formats.dll";
        DROP ASSEMBLY IF EXISTS [Newtonsoft.Json];
        CREATE ASSEMBLY [Newtonsoft.Json] FROM @"/Assemblies/Avro/Newtonsoft.Json.dll";
        DROP ASSEMBLY IF EXISTS [log4net];
        CREATE ASSEMBLY [log4net] FROM @"/Assemblies/Avro/log4net.dll";

        REFERENCE ASSEMBLY [Newtonsoft.Json];
        REFERENCE ASSEMBLY [log4net];
        REFERENCE ASSEMBLY [Avro];
        REFERENCE ASSEMBLY [Microsoft.Analytics.Samples.Formats];

        // Blob container storage account filenames, with any path
        DECLARE @input_file string = @"wasb://hottubrawdata@kevinsayazstorage/kevinsayIoT/{*}/{*}/{*}/{*}/{*}/{*}";
        DECLARE @output_file string = @"/output/output.csv";

        @rs =
        EXTRACT
        EnqueuedTimeUtc string,
        Body byte[]
        FROM @input_file

        USING new Microsoft.Analytics.Samples.Formats.ApacheAvro.AvroExtractor(@"
        {
        ""type"":""record"",
        ""name"":""Message"",
        ""namespace"":""Microsoft.Azure.Devices"",
        ""fields"":[{
        ""name"":""EnqueuedTimeUtc"",
        ""type"":""string""
        },
        {
        ""name"":""Properties"",
        ""type"":{
        ""type"":""map"",
        ""values"":""string""
        }
        },
        {
        ""name"":""SystemProperties"",
        ""type"":{
        ""type"":""map"",
        ""values"":""string""
        }
        },
        {
        ""name"":""Body"",
        ""type"":[""null"",""bytes""]
        }
        ]
        }");

        @cnt =
        SELECT EnqueuedTimeUtc AS time, Encoding.UTF8.GetString(Body) AS jsonmessage
        FROM @rs;

        OUTPUT @cnt TO @output_file USING Outputters.Text(); 
    ```    

    Aşağıda gösterilen komut dosyası çalıştırarak, ADLA 5 dakika sürdü 10 analitik birimlerine sınırlı ve işlenen 177 dosyası, bir CSV dosyası çıkışı özetleme.
    
    ![Adım 7b ekran yakalama][img-query-avro-data-7b]

    Çıktı görüntüleme, Avro içeriği bir CSV dosyasına dönüştürülen görebilirsiniz. JSON ayrıştırma istiyorsanız 8. adımına devam edin.
    
    ![Adım 7c için ekran yakalama][img-query-avro-data-7c]

    
8. Çoğu IOT iletileri JSON biçimindedir.  WHERE yan tümceleri ekleyin ve yalnızca gerekli verileri çıktı aşağıdaki satırları ekleme, ileti JSON içinde ayrıştıramıyor.

    ```sql
       @jsonify = SELECT Microsoft.Analytics.Samples.Formats.Json.JsonFunctions.JsonTuple(Encoding.UTF8.GetString(Body)) AS message FROM @rs;
    
        /*
        @cnt =
            SELECT EnqueuedTimeUtc AS time, Encoding.UTF8.GetString(Body) AS jsonmessage
            FROM @rs;
        
        OUTPUT @cnt TO @output_file USING Outputters.Text();
        */
        
        @cnt =
            SELECT message["message"] AS iotmessage,
                   message["event"] AS msgevent,
                   message["object"] AS msgobject,
                   message["status"] AS msgstatus,
                   message["host"] AS msghost
            FROM @jsonify;
            
        OUTPUT @cnt TO @output_file USING Outputters.Text();
    ```

9. Çıktı görüntüleme, select komutu her öğe için sütunları şimdi bakın. 
    
    ![8. adım ekran yakalama][img-query-avro-data-8]

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, Azure IOT Hub iletilerden Azure hizmetlerine verimli bir şekilde yönlendirme Avro verileri sorgulamak öğrendiniz.

IOT hub'ı kullanan tam uçtan uca çözümler örnekleri görmek için bkz: [Azure IOT Uzaktan izleme Çözüm Hızlandırıcısı][lnk-iot-sa-land].

IOT Hub ile çözümleri geliştirme hakkında daha fazla bilgi için bkz: [IOT Hub Geliştirici Kılavuzu].

IOT Hub içinde ileti yönlendirme hakkında daha fazla bilgi için bkz: [IOT Hub ile iletileri almasına ve göndermesine][lnk-devguide-messaging].

<!-- Images -->
[img-query-avro-data-1a]: ./media/iot-hub-query-avro-data/query-avro-data-1a.png
[img-query-avro-data-1b]: ./media/iot-hub-query-avro-data/query-avro-data-1b.png
[img-query-avro-data-2]: ./media/iot-hub-query-avro-data/query-avro-data-2.png
[img-query-avro-data-3]: ./media/iot-hub-query-avro-data/query-avro-data-3.png
[img-query-avro-data-4]: ./media/iot-hub-query-avro-data/query-avro-data-4.png
[img-query-avro-data-5]: ./media/iot-hub-query-avro-data/query-avro-data-5.png
[img-query-avro-data-6]: ./media/iot-hub-query-avro-data/query-avro-data-6.png
[img-query-avro-data-7a]: ./media/iot-hub-query-avro-data/query-avro-data-7a.png
[img-query-avro-data-7b]: ./media/iot-hub-query-avro-data/query-avro-data-7b.png
[img-query-avro-data-7c]: ./media/iot-hub-query-avro-data/query-avro-data-7c.png
[img-query-avro-data-8]: ./media/iot-hub-query-avro-data/query-avro-data-8.png

<!-- Links -->
[Azure IOT hub'ı ileti yönlendirme: ileti gövdesinde yönlendirme ile artık]: https://azure.microsoft.com/blog/iot-hub-message-routing-now-with-routing-on-message-body/

[Routing on message bodies]: iot-hub-devguide-query-language.md#routing-on-message-bodies
[When using Azure storage containers]:iot-hub-devguide-endpoints.md#when-using-azure-storage-containers

[U-SQL Avro örneği]:https://github.com/Azure/usql/tree/master/Examples/AvroExamples

[lnk-iot-sa-land]: ../iot-accelerators/index.md
[IOT Hub Geliştirici Kılavuzu]: iot-hub-devguide.md
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
