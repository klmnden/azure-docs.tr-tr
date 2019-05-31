---
title: Azure Data Lake Analytics'i kullanarak avro veri sorgulama | Microsoft Docs
description: Cihaz telemetrisi Blob depolama alanına ve Blob depolama alanına yazılır Avro biçimi verileri sorgulamak, ileti gövdesi özelliklerini kullanın.
author: ash2017
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 05/15/2019
ms.author: asrastog
ms.openlocfilehash: 84e1dd77c6e873dc2facb5126bbddf795192b60d
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66257770"
---
# <a name="query-avro-data-by-using-azure-data-lake-analytics"></a>Azure Data Lake Analytics'i kullanarak avro verileri Sorgulama

Bu makalede, Azure Hizmetleri için Azure IOT Hub'ından iletiler verimli bir şekilde yönlendirmek için Avro verileri sorgulamak anlatılmaktadır. [İleti yönlendirme](iot-hub-devguide-messages-d2c.md) , ileti özelliklerini, ileti gövdesi, cihaz ikizi etiketleri ve cihaz ikizi özelliklerini temel alan zengin sorgular kullanarak verileri filtrelemenize izin verir. İleti yönlendirme sorgulanırken yetenekleri hakkında daha fazla bilgi için bkz [ileti yönlendirme sorgusu söz dizimi](iot-hub-devguide-routing-query-syntax.md).

Azure IOT Hub iletilerini Azure Blob depolama alanına yönlendirirken, varsayılan olarak IOT hub'ı içeriği hem bir ileti gövdesi özelliğinden hem de bir ileti özelliği Avro biçiminde yazdığını zor olmuştur. Avro biçimi, diğer uç noktalar için kullanılmaz. Avro biçimi veri ve ileti korunması için mükemmel olmakla birlikte, bu verileri sorgulamak için kullanılacak kolay değildir. Buna karşılık, JSON veya CSV biçiminde veri sorgulama için çok daha kolaydır. IOT hub'ı artık Blob storage'da AVRO yanı sıra JSON veri yazmaktan destekler.

Daha fazla bilgi için [yönlendirme bir uç nokta olarak Azure Blob Depolama kullanarak](iot-hub-devguide-messages-d2c.md#azure-blob-storage).

İlişkisel olmayan büyük veri gereksinimleri ve biçimler adresi ve bu zorluğun üstesinden gelmek için çok büyük veri modellerini dönüştürme hem veri ölçekleme için kullanabilirsiniz. Bir desen, "sorgu ödeme", odak noktası, bu makalede Azure Data Lake Analytics. Hadoop veya diğer çözümleri kolayca sorgu yürütebilirsiniz olsa da, Data Lake Analytics "sorgu ödeme" Bu yaklaşım için genellikle daha uygundur.

U-SQL'de Avro için bir "ayıklayıcısı" yoktur. Daha fazla bilgi için [U-SQL Avro örnek](https://github.com/Azure/usql/tree/master/Examples/AvroExamples).

## <a name="query-and-export-avro-data-to-a-csv-file"></a>Sorgulamak ve Avro veri bir CSV dosyasına aktar

Bu bölümde, Avro verileri sorgulamak ve diğer depolara veya veri depolarında veriyi kolayca 'er olsa da Azure Blob Depolama alanında bir CSV dosyasına aktarmak.

1. İletileri seçmek için ileti gövdesinde bir özelliğini kullanarak bir Azure Blob Depolama uç noktasına rota verileri için Azure IOT hub'ı ayarladınız ayarlayın.

   !["Özel uç noktaları" bölümünde](./media/iot-hub-query-avro-data/query-avro-data-1a.png)

   ![Yönlendirme kuralları](./media/iot-hub-query-avro-data/query-avro-data-1b.png)

   Yollar ve özel uç noktalar ayarları hakkında daha fazla bilgi için bkz. [ileti yönlendirme için IOT hub'ı](iot-hub-create-through-portal.md#message-routing-for-an-iot-hub).

2. Cihazınızı kodlama, içerik türü ve gerekli verileri özelliklerini ya da ileti gövdesi, ürün belgelerinde belirtildiği gibi olduğundan emin olun. Burada gösterildiği gibi bu öznitelikler Device Explorer içinde görüntülediğinizde, bunların doğru şekilde ayarlandığını doğrulayabilirsiniz.

   ![Olay hub'ı veri bölmesi](./media/iot-hub-query-avro-data/query-avro-data-2.png)

3. Azure Data Lake Store örneği ve bir Data Lake Analytics örneği ayarlayın. Azure IOT hub'ı Data Lake Store örneğine yönlendirilmez, ancak bir Data Lake Analytics örneği gerektirir.

   ![Data Lake Store ve Data Lake Analytics örnekleri](./media/iot-hub-query-avro-data/query-avro-data-3.png)

4. Data Lake Analytics, Azure Blob depolama alanına ek bir deposu, veri için Azure IOT hub'ı yönlendiren aynı Blob Depolama olarak yapılandırın.

   !["Veri kaynakları" bölmesi](./media/iot-hub-query-avro-data/query-avro-data-4.png)

5. Bölümünde açıklandığı gibi [U-SQL Avro örnek](https://github.com/Azure/usql/tree/master/Examples/AvroExamples), dört DLL dosyaları gerekir. Bu dosyalar, Data Lake Store Örneğinizde bir konuma yükleyin.

   ![Dört karşıya yüklenen DLL dosyaları](./media/iot-hub-query-avro-data/query-avro-data-5.png)

6. Visual Studio'da bir U-SQL projesi oluşturun.

   !Create a U-SQL project](./media/iot-hub-query-avro-data/query-avro-data-6.png)

7. Aşağıdaki komut dosyası içeriğini yeni oluşturulan dosyaya yapıştırın. Üç vurgulanan bölüm değiştirme: Data Lake Analytics hesabınızı ve ilişkili DLL dosya yolları depolama hesabınız için doğru yol.

   ![Değiştirilecek üç bölüm](./media/iot-hub-query-avro-data/query-avro-data-7a.png)

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
            ""fields"":
           [{
                ""name"":""EnqueuedTimeUtc"",
                ""type"":""string""
            },
            {
                ""name"":""Properties"",
                ""type"":
                {
                    ""type"":""map"",
                    ""values"":""string""
                }
            },
            {
                ""name"":""SystemProperties"",
                ""type"":
                {
                    ""type"":""map"",
                    ""values"":""string""
                }
            },
            {
                ""name"":""Body"",
                ""type"":[""null"",""bytes""]
            }]
        }"
        );

        @cnt =
        SELECT EnqueuedTimeUtc AS time, Encoding.UTF8.GetString(Body) AS jsonmessage
        FROM @rs;

        OUTPUT @cnt TO @output_file USING Outputters.Text(); 
    ```

    Data Lake Analytics sürdüğünü beş dakika içinde 10 analitik birime sınırlıydı ve işlenen 177 dosyası aşağıdaki betiği çalıştırın. Sonuç, aşağıdaki görüntüde gösterilen CSV dosyası çıktıda gösterilir:

    ![CSV dosyası için çıkış sonuçları](./media/iot-hub-query-avro-data/query-avro-data-7b.png)

    ![CSV dosyasına dönüştürülmüş çıktı](./media/iot-hub-query-avro-data/query-avro-data-7c.png)

    JSON Ayrıştır 8. adımına devam edin.

8. Çoğu IOT iletileri JSON dosya biçimindedir. Aşağıdaki satırı ekleyerek, WHERE yan tümceler eklemek ve yalnızca gerekli verileri çıktı olanak sağlayan bir JSON dosyasına ileti ayrıştırabilirsiniz.

    ```sql
       @jsonify =
         SELECT Microsoft.Analytics.Samples.Formats.Json.JsonFunctions.JsonTuple(Encoding.UTF8.GetString(Body))
           AS message FROM @rs;
    
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

    ![Çıkış sütunu her öğe için gösterme](./media/iot-hub-query-avro-data/query-avro-data-8.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure Hizmetleri için Azure IOT Hub'ından iletiler verimli bir şekilde yönlendirmek için Avro verileri Sorgulama öğrendiniz.

IOT hub'ı kullanan tam uçtan uca çözümler örnekleri için bkz: [Azure IOT Çözüm Hızlandırıcısı belgeleri](/azure/iot-accelerators).

IOT Hub ile çözümleri geliştirme hakkında daha fazla bilgi için bkz. [IOT Hub Geliştirici kılavuzunun](iot-hub-devguide.md).

IOT Hub'ında ileti yönlendirme hakkında daha fazla bilgi için bkz: [göndermek ve IOT Hub ile ileti alma](iot-hub-devguide-messaging.md).
