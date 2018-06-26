---
title: Azure Data Lake Analytics kullanarak avro veri sorgulama | Microsoft Docs
description: Cihaz telemetrisi Blob depolama alanına yönlendirmek ve Blob depolama alanına yazılır Avro biçimi verileri sorgulamak için ileti gövdesi özelliklerini kullanın.
services: iot-hub
documentationcenter: ''
author: ksaye
manager: obloch
ms.service: iot-hub
ms.topic: article
ms.date: 05/29/2018
ms.author: Kevin.Saye
ms.openlocfilehash: 08aed809184cbb65d632e1fb6f4b9bd25747e349
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36751083"
---
# <a name="query-avro-data-by-using-azure-data-lake-analytics"></a>Azure Data Lake Analytics kullanarak avro verileri Sorgulama

Bu makalede verimli bir şekilde Azure hizmetlerine Azure IOT Hub gelen iletileri yönlendirmek için Avro verileri Sorgulama anlatılmaktadır. Biz blog postasına duyurdu gibi [Azure IOT hub'ı ileti yönlendirme: ileti gövdesinde yönlendirme ile artık], özellikleri veya ileti gövdesi yönlendirme IOT hub'ı destekler. Daha fazla bilgi için bkz: [İleti gövdeleri yönlendirme][Routing on message bodies]. 

Azure IOT hub'ı Azure Blob depolama alanına iletileri yönlendirirken challenge, bırakıldı, IOT hub'ı hem bir ileti gövdesi özelliği, hem de bir ileti özelliği Avro biçiminde içeriği yazar. IOT hub'ı yalnızca Avro veri biçiminde Blob Depolama veri yazmada destekler ve bu biçim için diğer tüm uç noktaları kullanılmaz. Daha fazla bilgi için bkz: [Azure Storage kapsayıcıları kullanırken][When using Azure storage containers]. Avro biçimi veri ve ileti korunması için harika olsa da, sorgu verileri kullanmak için bir sınama var. Buna karşılık, JSON veya CSV biçiminde veri sorgulama için çok daha kolaydır.

Adres ilişkisel olmayan verilerin büyük gereksinimlerini ve biçimleri ve bu sorunu çözmek için birçok büyük veri desenleri dönüştürme hem veri ölçekleme için kullanabilirsiniz. Desenler birini "ödeme sorgu" odak noktası, bu makalede Azure Data Lake Analytics olur. Hadoop veya diğer çözümleri kolayca sorgu yürütebilir rağmen Data Lake Analytics genellikle daha iyi "sorgu ödeme" Bu yaklaşım için uygundur. 

U-SQL Avro için bir "ayıklayıcısı" dir. Daha fazla bilgi için bkz: [U-SQL Avro örneği].

## <a name="query-and-export-avro-data-to-a-csv-file"></a>Sorgulamak ve Avro verileri bir CSV dosyasına dışarı aktarma
Bu bölümde, Avro verileri sorgulamak ve verileri diğer depoları veya veri depolarında kolayca yerleştirin, ancak Azure Blob Depolama alanında bir CSV dosyasına dışarı.

1. İletileri seçmek için ileti gövdesinde bir özelliğini kullanarak bir Azure Blob storage uç rota verileri için Azure IOT Hub ayarlayın.

    !["Özel uç noktalar" bölümü][img-query-avro-data-1a]

    ![Yollar komutu][img-query-avro-data-1b]

2. Cihazınızı kodlama, içerik türü ve gerekli tüm verileri özellikleri ya da ileti gövdesi, başvurulan ürün belgelerinde olarak olduğundan emin olun. Aşağıda gösterildiği gibi bu öznitelikler aygıt Gezgini'nde görüntülediğinizde, bunların doğru ayarlandığını doğrulayabilirsiniz.

    ![Olay hub'ı veri bölmesi][img-query-avro-data-2]

3. Azure Data Lake Store örneği ve bir Data Lake Analytics örneği ayarlayın. Azure IOT hub'ı bir Data Lake Store örneğine yönlendirmez, ancak bir Data Lake Analytics örneği gerektirir.

    ![Data Lake Store ve Data Lake Analytics örnekleri][img-query-avro-data-3]

4. Data Lake Analytics Azure Blob Depolama verileri için Azure IOT Hub yönlendirir aynı Blob storage ek bir deposu olarak yapılandırın.

    !["Veri kaynakları" bölmesi][img-query-avro-data-4]
 
5. ' Da anlatıldığı gibi [U-SQL Avro örneği], dört DLL dosyaları gerekir. Bu dosyalar, Data Lake Store örneğinizi bir konumda karşıya yükleyin.

    ![Dört karşıya yüklenen DLL dosyaları][img-query-avro-data-5] 

6. Visual Studio'da U-SQL projesi oluşturun.
 
    ![U-SQL projesi oluşturma][img-query-avro-data-6]

7. Aşağıdaki komut dosyası içeriğini yeni oluşturulan dosyaya yapıştırın. Üç vurgulanan bölüm değiştirme: Data Lake Analytics hesabınızı, ilişkili DLL dosya yolları ve depolama hesabınız için doğru yolu.
    
    ![Değiştirilecek üç bölüm][img-query-avro-data-7a]

    Bir CSV dosyasına basit çıktı gerçek U-SQL betiği:
    
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

    Data Lake Analytics sürdü beş dakika 10 analitik birimlerine sınırlı ve işlenen 177 dosyası aşağıdaki betiği çalıştırın. Sonuç, aşağıdaki görüntüde gösterilen CSV dosyası çıkışı gösterilir:
    
    ![CSV dosyası çıkışı sonuçları][img-query-avro-data-7b]

    ![CSV dosyasına dönüştürülen çıkış][img-query-avro-data-7c]

    JSON ayrıştırmak için 8. adımına devam edin.
    
8. Çoğu IOT iletileri JSON dosyası biçiminde edilir. Aşağıdaki satırları ekleyerek, WHERE yan tümceleri eklemenizi ve yalnızca gerekli veri çıkışı sağlayan bir JSON dosyasına ileti ayrıştıramıyor.

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

    Her öğe için bir sütun çıktısını görüntüler `SELECT` komutu. 
    
    ![Her öğe için bir sütun gösteren çıktı][img-query-avro-data-8]

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, verimli bir şekilde Azure hizmetlerine Azure IOT Hub gelen iletileri yönlendirmek için Avro verileri sorgulamak öğrendiniz.

IOT hub'ı kullanan tam uçtan uca çözümler örnekleri için bkz: [Azure IOT Uzaktan izleme Çözüm Hızlandırıcısı][lnk-iot-sa-land].

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
