---
title: Protokol arabellekleri cihaz benzetimi - Azure ile kullanma | Microsoft Docs
description: Bu nasıl yapılır kılavuzunda protokol arabellekleri cihaz benzetimi çözüm hızlandırıcıdan gönderilen telemetri seri hale getirmek için nasıl kullanılacağını öğrenin.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.custom: mvc
ms.date: 11/06/2018
ms.author: dobett
ms.openlocfilehash: 74bb2d181533f802e1428eaa8a855f60fb855193
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61447990"
---
# <a name="serialize-telemetry-using-protocol-buffers"></a>Telemetri protokol arabellekleri kullanarak seri hale getirme

Protokol arabellekleri (Protobuf), yapılandırılmış veriler için bir ikili serileştirme biçimi olur. Protobuf kolaylık ve daha küçük ve daha hızlı bir şekilde XML olma atış performans vurgulamak için tasarlanmıştır.

Cihaz benzetimi destekler **proto3** protokolün dil arabelleğe alır.

Protobuf verilerini seri hale getirmek için derlenmiş kod gerektirdiğinden, cihaz benzetimi, özel bir sürümünü oluşturmanız gerekir.

Nasıl, bu kılavuzdaki adımları Yardım-How-to-göstermek için:

1. Geliştirme ortamınızı hazırlama
1. Bir cihaz modelinde Protobuf biçimini kullanarak belirtin
1. Protobuf biçiminiz tanımlayın
1. Protobuf sınıflar oluşturun
1. Yerel olarak test etme

## <a name="prerequisites"></a>Önkoşullar

Bu nasıl yapılır Kılavuzu'ndaki adımları takip etmek için ihtiyacınız vardır:

* Visual Studio Code. İndirebileceğiniz [Mac, Linux ve Windows için Visual Studio Code](https://code.visualstudio.com/download).
* .NET core. İndirebileceğiniz [Mac, Linux ve Windows için .NET Core](https://www.microsoft.com/net/download).
* Postman. İndirebileceğiniz [Mac, windows veya Linux için Postman](https://www.getpostman.com/apps).
* Bir [IOT hub'ı Azure aboneliğinize dağıtılır](../iot-hub/iot-hub-create-through-portal.md). Bu kılavuzdaki adımları tamamlamak için IOT hub'ınızın bağlantı dizesi gerekir. Bağlantı dizesini Azure portalından alabilirsiniz.
* A [Azure aboneliğinize dağıtılır Cosmos DB veritabanı](../cosmos-db/create-sql-api-dotnet.md#create-account) SQL API'si kullanan ve için yapılandırılmış [güçlü tutarlılık](../cosmos-db/manage-account.md). Bu kılavuzdaki adımları tamamlamak için Cosmos DB veritabanı bağlantı dizesi gerekir. Bağlantı dizesini Azure portalından alabilirsiniz.
* Bir [Azure depolama hesabını Azure aboneliğinize dağıtılır](../storage/common/storage-quickstart-create-account.md). Bu kılavuzdaki adımları tamamlamak için depolama hesabının bağlantı dizesi gerekir. Bağlantı dizesini Azure portalından alabilirsiniz.

## <a name="prepare-your-development-environment"></a>Geliştirme ortamınızı hazırlama

Geliştirme ortamınızı hazırlamak üzere aşağıdaki görevleri tamamlayın:

* Cihaz benzetimi mikro Hizmet kaynağı indirin.
* Depolama bağdaştırıcısı mikro Hizmet kaynağı indirin.
* Depolama bağdaştırıcısı mikro hizmet yerel olarak çalıştırın.

Bu makaledeki yönergeler, Windows kullanmakta olduğunuz varsayılır. Başka bir işletim sistemi kullanıyorsanız, bazı dosya yolları ve komutları ortamınıza uyacak şekilde ayarlamanız gerekebilir.

### <a name="download-the-microservices"></a>Mikro hizmetler indirin

İndirip sıkıştırmasını [Uzaktan izleme mikro Hizmetler](https://github.com/Azure/remote-monitoring-services-dotnet/archive/master.zip) yerel makinenizde uygun bir konuma github'dan. Bu depo, bu yöntem için gereken depolama bağdaştırıcısı mikro hizmet içerir.

İndirip sıkıştırmasını [cihaz benzetimi mikro hizmet](https://github.com/Azure/device-simulation-dotnet/archive/master.zip) yerel makinenizde uygun bir konuma github'dan.

### <a name="run-the-storage-adapter-microservice"></a>Depolama bağdaştırıcısı mikro hizmet çalıştırma

Visual Studio Code'da açmak **remote-monitoring-services-dotnet-master\storage-adapter** klasör. Tıklatın **geri** çözümlenmemiş bağımlılıklar düzeltmek için düğmeler.

Açık **.vscode/launch.json** dosya ve Cosmos DB bağlantı dizenizi atama **bilgisayarları\_STORAGEADAPTER\_DOCUMENTDB\_CONNSTRING** ortamı değişken.

> [!NOTE]
> Mikro hizmet makinenizde yerel olarak çalıştırdığınızda, yine de düzgün çalışması için Azure Cosmos DB örneğine gerektirir.

Depolama bağdaştırıcısı mikro hizmet yerel olarak çalıştırmak için tıklayın **hata ayıklama \> hata ayıklamayı Başlat**.

**Terminal** penceresi Visual Studio code'da web hizmetinin sistem durumu denetimi için bir URL dahil olmak üzere çalışan mikro hizmet çıktısını gösterir: <http://127.0.0.1:9022/v1/status>. Bu adrese gidin, durumu olmalıdır "Tamam: Canlı ve iyi".

Aşağıdaki adımları tamamlarken Visual Studio Code'nın bu örneğinde çalışan depolama bağdaştırıcısı mikro hizmet bırakın.

## <a name="define-your-device-model"></a>Cihazınızın modeline tanımlayın

Açık **cihaz-simülasyon-dotnet-master** Visual Studio Code yeni bir örneğini github'dan indirdiğiniz klasörü. Tıklatın **geri** herhangi düzeltmek için düğmeler çözümlenmemiş bağımlılıklar.

Bu Yardım-How-to-Kılavuzu'nda, bir varlık İzleyicisi için yeni bir cihaz modeli oluşturun:

1. Adlı yeni bir cihaz modeli dosya oluşturma **assettracker 01.json** içinde **Services\data\devicemodels** klasör.

1. Cihaz modeli cihazın işlevselliğini tanımlamak **assettracker 01.json** dosya. Protobuf cihaz modeli telemetri bölümünü gerekir:

   * Cihazınız için oluşturduğunuz Protobuf sınıfın adını içerir. Aşağıdaki bölümde bu sınıfı oluşturmayı gösterir.
   * Protobuf ileti biçimi belirtin.

     ```json
     {
     "SchemaVersion": "1.0.0",
     "Id": "assettracker-01",
     "Version": "0.0.1",
     "Name": "Asset Tracker",
     "Description": "An asset tracker with location, temperature, and humidity",
     "Protocol": "AMQP",
     "Simulation": {
       "InitialState": {
         "online": true,
         "latitude": 47.445301,
         "longitude": -122.296307,
         "temperature": 38.0,
         "humidity": 62.0
       },
       "Interval": "00:01:00",
       "Scripts": [
         {
           "Type": "javascript",
           "Path": "assettracker-01-state.js"
         }
       ]
     },
     "Properties": {
       "Type": "AssetTracker",
       "Location": "Field",
       "Latitude": 47.445301,
       "Longitude": -122.296307
     },
     "Telemetry": [
       {
         "Interval": "00:00:10",
         "MessageTemplate": "{\"latitude\":${latitude},\"longitude\":${longitude},\"temperature\":${temperature},\"humidity\":${humidity}}",
         "MessageSchema": {
           "Name": "assettracker-sensors;v1",
           "ClassName": "Microsoft.Azure.IoTSolutions.DeviceSimulation.Services.Models.Protobuf.AssetTracker",
           "Format": "Protobuf",
           "Fields": {
             "latitude": "double",
             "longitude": "double",
             "temperature": "double",
             "humidity": "double"
           }
         }
       }
     ]
     }
     ```

### <a name="create-device-behaviors-script"></a>Cihaz davranışlarını betiği oluşturma

Cihazınızı nasıl davranacağını tanımlayan davranışı betik yazın. Daha fazla bilgi için [Gelişmiş bir sanal cihaz oluşturma](iot-accelerators-device-simulation-advanced-device.md).

## <a name="define-your-protobuf-format"></a>Protobuf biçiminiz tanımlayın

Cihaz modeli ve ileti biçimi belirledikten oluşturabileceğiniz bir **proto** dosya. İçinde **proto** eklediğiniz dosya:

* A `csharp_namespace` eşleşen **ClassName** özelliği, cihaz modeli.
* Seri hale getirmek her veri yapısı için bir ileti.
* Bir ad ve tür iletinin her bir alan için.

1. Adlı yeni bir dosya oluşturun **assettracker.proto** içinde **Services\Models\Protobuf\proto** klasör.

1. Sözdizimi, ad alanı ve ileti şemasında tanımlamak **proto** aşağıdaki gibi:

    ```proto
    syntax = "proto3";

    option csharp_namespace = "Microsoft.Azure.IoTSolutions.DeviceSimulation.Services.Models.Protobuf";

    message AssetTracker {
      double latitude=1;
      double longitude=2;
      double temperature=5;
      double humidity=6;
    }
    ```

`=1`, `=2` İşaretçileri her öğe üzerinde ikili kodlama alanı kullanan benzersiz bir etiket belirtin. 1-15 numaraları büyük sayılar kodlamak için daha az bir bayt gerektirir.

## <a name="generate-the-protobuf-class"></a>Protobuf sınıfı oluşturun

olduğunda bir **proto** dosyası, sonraki adım ise ileti okumak ve yazmak için gerekli sınıfların. Bu adımı tamamlamak için ihtiyacınız **Protoc** Protobuf derleyici.

1. [Protobuf derleyici Github'dan indirin.](https://github.com/protocolbuffers/protobuf/releases/download/v3.4.0/protoc-3.4.0-win32.zip)

1. Derleyicinin, kaynak dizin, hedef dizin ve adını belirterek çalıştırın, **proto** dosya. Örneğin:

    ```cmd
    protoc -I c:\temp\device-simulation-dotnet-master\Services\Models\Protobuf\proto --csharp_out=C:\temp\device-simulation-dotnet-master\Services\Models\Protobuf assettracker.proto
    ```

    Bu komut oluşturur. bir **Assettracker.cs** dosyası **Services\Models\Protobuf** klasör.

## <a name="test-protobuf-locally"></a>Yerel olarak test Protobuf

Bu bölümde, yerel olarak önceki bölümlerinde oluşturduğunuz varlık İzleyicisi cihaz test edin.

### <a name="run-the-device-simulation-microservice"></a>Cihaz benzetimi mikro hizmet çalıştırma

Açık **.vscode/launch.json** dosya ve ata kullanarak:

* IOT Hub bağlantı dizesine **bilgisayarları\_IOTHUB\_CONNSTRING** ortam değişkeni.
* Depolama hesabı bağlantı dizesi için **bilgisayarları\_AZURE\_depolama\_hesabı** ortam değişkeni.
* Cosmos DB bağlantı dizesi **bilgisayarları\_STORAGEADAPTER\_DOCUMENTDB\_CONNSTRING** ortam değişkeni.

Açık **WebService/Properties/launchSettings.json** dosya ve ata kullanarak:

* IOT Hub bağlantı dizesine **bilgisayarları\_IOTHUB\_CONNSTRING** ortam değişkeni.
* Depolama hesabı bağlantı dizesi için **bilgisayarları\_AZURE\_depolama\_hesabı** ortam değişkeni.
* Cosmos DB bağlantı dizesi **bilgisayarları\_STORAGEADAPTER\_DOCUMENTDB\_CONNSTRING** ortam değişkeni.

Açık **WebService\appsettings.ini** dosya ve ayarları aşağıdaki gibi değiştirin:

#### <a name="configure-the-solution-to-include-your-new-device-model-files"></a>Yeni cihaz model dosyalarınızı eklemek için çözüm yapılandırma

Varsayılan olarak, yeni cihaz modeli JSON ve JS dosyaları içinde oluşturulan çözüm kopyalanmaz. Açıkça bunları eklemeniz gerekir.

Bir girdiyi **services\services.csproj** dahil istediğiniz her bir dosya için dosya. Örneğin:

```xml
<None Update="data\devicemodels\assettracker-01.json">
    <CopyToOutputDirectory>Always</CopyToOutputDirectory>
</None>
<None Update="data\devicemodels\scripts\assettracker-01-state.js">
    <CopyToOutputDirectory>Always</CopyToOutputDirectory>
</None>
```

Mikro hizmet yerel olarak çalıştırmak için tıklayın **hata ayıklama \> hata ayıklamayı Başlat**.

**Terminal** penceresi Visual Studio code'da çalışan mikro hizmet çıktısını gösterir.

Sonraki adımları tamamlarken Visual Studio Code'nın bu örneğinde çalışan cihaz benzetimi mikro hizmet bırakın.

### <a name="set-up-a-monitor-for-device-events"></a>Bir izleyici cihaz olayları için ayarlama

Bu bölümde, IOT hub'ınıza bağlı cihazlardan gönderilen telemetri verilerini görüntülemek için bir Olay İzleyicisi ayarlamak için Azure CLI'yı kullanın.

Aşağıdaki betik, IOT hub'ınızın adı olduğunu varsayar **cihaz benzetimi test**.

```azurecli-interactive
# Install the IoT extension if it's not already installed
az extension add --name azure-cli-iot-ext

# Monitor telemetry sent to your hub
az iot hub monitor-events --hub-name device-simulation-test
```

Sanal cihazlar test ederken çalıştıran Olay İzleyici bırakın.

### <a name="create-a-simulation-with-the-asset-tracker-device-type"></a>Varlık İzleyicisi cihaz türü ile bir simülasyon oluşturma

Bu bölümde, varlık İzleyicisi cihaz türünü kullanarak bir simülasyon çalıştırma için cihaz benzetimi mikro hizmet istemek için Postman Aracı'nı kullanın. Postman, REST istekleri göndermek için bir web hizmeti sağlayan bir araçtır.

Postman'ı ayarlamak için:

1. Yerel makinenizde Postman'i açın.

1. Tıklayın **dosya \> alma**. Ardından **dosya seçin**.

1. Seçin **Azure IOT cihaz benzetimi çözüm accelerator.postman\_koleksiyon** ve **Azure IOT cihaz benzetimi çözüm accelerator.postman\_ortam** ve tıklayın **açık**.

1. Genişletin **Azure IOT cihaz benzetimi Çözüm Hızlandırıcısı** gönderebilirsiniz isteklerini görmek için.

1. Tıklayın **yok ortam** seçip **Azure IOT cihaz benzetimi Çözüm Hızlandırıcısı**.

Artık bir koleksiyon ve cihaz benzetimi mikro hizmet ile etkileşim kurmak için kullanabileceğiniz Postman çalışma alanınızda yüklenen ortam vardır.

Yapılandırma ve benzetim çalıştırmak için:

1. Postman koleksiyonuna seçin **varlık İzleyicisi benzetimi oluşturmak** tıklatıp **Gönder**. Bu istek, dört sanal varlık İzleyicisi cihaz türü örneklerini oluşturur.

1. Olay İzleme çıktısı Azure CLI penceresinde, sanal cihazlardan telemetri gösterir.

Benzetimi durdurmak için seçin **benzetimi Durdur** isteği Postman tıklayıp **Gönder**.

### <a name="clean-up-resources"></a>Kaynakları temizleme

İki yerel olarak çalışan mikro hizmetler, Visual Studio kod örneklerindeki durdurabilirsiniz (**hata ayıklama \> hata ayıklamayı Durdur**).

Artık IOT Hub ve Cosmos DB örnekleri gerektiriyorsa, gereksiz ücretlerden kaçınmak için Azure aboneliğinizden silebilirsiniz.

## <a name="iot-hub-support"></a>IOT hub'ı desteği

IOT hub'ı özelliklerinin Protobuf ya da diğer ikili biçimler yerel olarak desteklemez. Örneğin, IOT Hub ileti yükü işleyemiyor olacağından ileti yükünü tabanlı yönlendirme yapamazsınız. Ancak, ileti üst bilgilere göre yönlendirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Protobuf telemetri göndermek için kullanılacak cihaz benzetimi özelleştirmek öğrendiniz artık artık öğrenmek için sonraki adım olan [bulutta özel bir görüntü dağıtmak](iot-accelerators-device-simulation-deploy-image.md).
