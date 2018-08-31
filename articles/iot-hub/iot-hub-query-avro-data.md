---
title: Azure Data Lake Analytics'i kullanarak avro veri sorgulama | Microsoft Docs
description: Cihaz telemetrisi Blob depolama alanına ve Blob depolama alanına yazılır Avro biçimi verileri sorgulamak, ileti gövdesi özelliklerini kullanın.
author: ash2017
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: asrastog
ms.openlocfilehash: a17df39c55b5c02c83e3f0b74a91d7109ddb4d3d
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2018
ms.locfileid: "43188953"
---
# <a name="query-avro-data-by-using-azure-data-lake-analytics"></a>Azure Data Lake Analytics'i kullanarak avro verileri Sorgulama

Bu makalede, Azure Hizmetleri için Azure IOT Hub'ından iletiler verimli bir şekilde yönlendirmek için Avro verileri sorgulamak anlatılmaktadır. Blog gönderisinde duyurduk gibi [Azure IOT Hub ileti yönlendirme: artık ileti gövdesinde yönlendirme ile], özellikleri veya ileti yönlendirme IOT hub'ı destekler. Daha fazla bilgi için [İleti gövdeleri üzerinde yönlendirme][Routing on message bodies]. 

Azure IOT Hub iletilerini Azure Blob depolama alanına yönlendirirken, sınama olmuştur, IOT Hub, hem bir ileti gövdesi özelliğinden hem de bir ileti özelliği Avro biçiminde içerik yazar. IOT hub'ı yalnızca Avro verileri biçiminde Blob Depolama veri yazmada destekler ve bu biçim için diğer tüm uç noktaları kullanılmaz. Daha fazla bilgi için [Azure depolama kapsayıcıları kullanırken][When using Azure storage containers]. Avro biçimi veri ve ileti korunması için mükemmel olmakla birlikte, bu verileri sorgulamak için kullanılacak kolay değildir. Buna karşılık, JSON veya CSV biçiminde veri sorgulama için çok daha kolaydır.

İlişkisel olmayan büyük veri gereksinimleri ve biçimler adresi ve bu zorluğun üstesinden gelmek için çok büyük veri modellerini dönüştürme hem veri ölçekleme için kullanabilirsiniz. Bir desen, "pay sorgu" odak noktası, bu makalede Azure Data Lake Analytics olur. Hadoop veya diğer çözümleri kolayca sorgu yürütebilirsiniz olsa da, Data Lake Analytics "sorgu ödeme" Bu yaklaşım için genellikle daha uygundur. 

U-SQL'de Avro için bir "ayıklayıcısı" yoktur. Daha fazla bilgi için [U-SQL Avro örneği].

## <a name="query-and-export-avro-data-to-a-csv-file"></a>Sorgulamak ve Avro veri bir CSV dosyasına aktar
Bu bölümde, Avro verileri sorgulamak ve diğer depolara veya veri depolarında veriyi kolayca 'er olsa da Azure Blob Depolama alanında bir CSV dosyasına aktarmak.

1. İletileri seçmek için ileti gövdesinde bir özelliğini kullanarak bir Azure Blob Depolama uç noktasına rota verileri için Azure IOT hub'ı ayarladınız ayarlayın.

    !["Özel uç noktaları" bölümünde][img-query-avro-data-1a]

    ![Yollar komutu][img-query-avro-data-1b]

2. Cihazınızı kodlama, içerik türü ve gerekli verileri özelliklerini ya da ileti gövdesi, ürün belgelerinde belirtildiği gibi olduğundan emin olun. Burada gösterildiği gibi bu öznitelikler Device Explorer içinde görüntülediğinizde, bunların doğru şekilde ayarlandığını doğrulayabilirsiniz.

    ![Olay hub'ı veri bölmesi][img-query-avro-data-2]

3. Azure Data Lake Store örneği ve bir Data Lake Analytics örneği ayarlayın. Azure IOT hub'ı Data Lake Store örneğine yönlendirilmez, ancak bir Data Lake Analytics örneği gerektirir.

    ![Data Lake Store ve Data Lake Analytics örnekleri][img-query-avro-data-3]

4. Data Lake Analytics, Azure Blob depolama alanına ek bir deposu, veri için Azure IOT hub'ı yönlendiren aynı Blob Depolama olarak yapılandırın.

    !["Veri kaynakları" bölmesi][img-query-avro-data-4]
 
5. Bölümünde açıklandığı gibi [U-SQL Avro örneği], dört DLL dosyaları gerekir. Bu dosyalar, Data Lake Store Örneğinizde bir konuma yükleyin.

    ![Dört karşıya yüklenen DLL dosyaları][img-query-avro-data-5] 

6. Visual Studio'da bir U-SQL projesi oluşturun.
 
    ![U-SQL projesi oluşturmak][img-query-avro-data-6]

7. Aşağıdaki komut dosyası içeriğini yeni oluşturulan dosyaya yapıştırın. Üç vurgulanan bölüm değiştirme: Data Lake Analytics hesabınızı ve ilişkili DLL dosya yolları depolama hesabınız için doğru yol.
    
    ![Değiştirilecek üç bölüm][img-query-avro-data-7a]

    Basit bir CSV dosyası çıkışı gerçek U-SQL betiği:
    
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

    Data Lake Analytics sürdüğünü beş dakika içinde 10 analitik birime sınırlıydı ve işlenen 177 dosyası aşağıdaki betiği çalıştırın. Sonuç, aşağıdaki görüntüde gösterilen CSV dosyası çıktıda gösterilir:
    
    ![CSV dosyası için çıkış sonuçları][img-query-avro-data-7b]

    ![CSV dosyasına dönüştürülmüş çıktı][img-query-avro-data-7c]

    JSON Ayrıştır 8. adımına devam edin.
    
8. Çoğu IOT iletileri JSON dosya biçimindedir. Aşağıdaki satırı ekleyerek, WHERE yan tümceler eklemek ve yalnızca gerekli verileri çıktı olanak sağlayan bir JSON dosyasına ileti ayrıştırabilirsiniz.

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

    Her öğe için bir sütun çıktıyı görüntüler `SELECT` komutu. 
    
    ![Çıkış sütunu her öğe için gösterme][img-query-avro-data-8]

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, Azure Hizmetleri için Azure IOT Hub'ından iletiler verimli bir şekilde yönlendirmek için Avro verileri Sorgulama öğrendiniz.

IOT hub'ı kullanan tam uçtan uca çözümler örnekleri için bkz: [Azure IOT Uzaktan izleme çözüm Hızlandırıcısını][lnk-iot-sa-land].

IOT Hub ile çözümleri geliştirme hakkında daha fazla bilgi için bkz. [IOT Hub Geliştirici Kılavuzu].

IOT Hub'ında ileti yönlendirme hakkında daha fazla bilgi için bkz: [göndermek ve IOT Hub ile ileti alma][lnk-devguide-messaging].

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
[Azure IOT Hub ileti yönlendirme: artık ileti gövdesinde yönlendirme ile]: https://azure.microsoft.com/blog/iot-hub-message-routing-now-with-routing-on-message-body/

[Routing on message bodies]: iot-hub-devguide-query-language.md#routing-on-message-bodies
[When using Azure storage containers]:iot-hub-devguide-endpoints.md#when-using-azure-storage-containers

[U-SQL Avro örneği]:https://github.com/Azure/usql/tree/master/Examples/AvroExamples

[lnk-iot-sa-land]: ../iot-accelerators/index.yml
[IOT Hub Geliştirici Kılavuzu]: iot-hub-devguide.md
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
