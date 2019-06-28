---
title: 'Öğretici: Azure Digital Twins ile alan izleme | Microsoft Docs'
description: Uzamsal kaynaklarınızı sağlamak ve Bu öğreticide adımları kullanarak Azure dijital çiftleri ile çalışma koşullarına izleme hakkında bilgi edinin.
services: digital-twins
author: dsk-2015
ms.custom: seodec18
ms.service: digital-twins
ms.topic: tutorial
ms.date: 06/26/2019
ms.author: dkshir
ms.openlocfilehash: 738e78ce06d98960c87414948e045cc4abe37d6b
ms.sourcegitcommit: aa66898338a8f8c2eb7c952a8629e6d5c99d1468
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67462198"
---
# <a name="tutorial-provision-your-building-and-monitor-working-conditions-with-azure-digital-twins-preview"></a>Öğretici: Yapı ve izleyici ile Azure dijital İkizlerini Önizleme koşulları çalışma sağlayın

Bu öğreticide, Azure dijital İkizlerini önizlemesi alanlarınıza istenen sıcaklık koşullardan ve konfor düzeyini izlemek için nasıl kullanılacağı gösterilmektedir. Çalıştırdıktan sonra [, örnek yapı yapılandırma](tutorial-facilities-setup.md), sağlama, yapı ve Bu öğreticide adımları kullanarak özel işlevler sensör verileriniz üzerinde çalıştırın.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * İzleme koşullarını tanımlayın.
> * Bir kullanıcı tanımlı işlev (UDF) oluşturun.
> * Sensör verilerini benzetimini yapar.
> * Bir kullanıcı tanımlı işlev sonuçlarını alın.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide, sahibi olduğunuzu varsayar [Azure dijital İkizlerini kurulumunuzu tamamlandı](tutorial-facilities-setup.md). Devam etmeden önce aşağıdakilere sahip olduğunuzdan emin olun:

- Bir [Azure hesabı](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Çalışan bir Digital Twins örneği. 
- Çalışma makinenize indirilmiş ve ayıklanmış [Digital Twins C# örnekleri](https://github.com/Azure-Samples/digital-twins-samples-csharp). 
- [.NET core SDK'sı sürüm 2.1.403 veya üzeri](https://www.microsoft.com/net/download) geliştirme makinenizde derlemek ve örnek çalıştırmak için. Çalıştırma `dotnet --version` doğru sürümünün yüklü olduğunu doğrulayın. 
- Örnek kodu incelemek için [Visual Studio Code](https://code.visualstudio.com/). 

## <a name="define-conditions-to-monitor"></a>İzleme koşullarını tanımlama

Bir dizi adlı cihaz veya algılayıcı verileri, izlemek için belirli koşullar tanımlayabilirsiniz *matchers*. Daha sonra çağrılan işlevlerin tanımlayabilirsiniz *kullanıcı tanımlı işlevleri*. Kullanıcı tanımlı işlevleri matchers tarafından belirtilen koşullar meydana geldiğinde, boşluk ve cihazlar, söz konusu veriler üzerinde özel mantığı yürütün. Daha fazla bilgi için okuma [veri işleme ve kullanıcı tanımlı işlevleri](concepts-user-defined-functions.md). 

Gelen **doluluk-quickstart** örnek proje, dosyayı açma **src\actions\provisionSample.yaml** Visual Studio code'da. **matchers** türü ile başlayan bölümü bulun. Her girişin altında bu tür bir Eşleştiricisi belirtilen oluşturur **adı**. Eşleştiricisi algılayıcı türündeki izleyecektir **dataTypeValue**. Adlı alanın nasıl ilişkili olduğunu fark *odak odası A1*, sahip olduğu bir **cihazları** birkaç algılayıcıdan içeren düğüm. Bu sensörlerden birini izleyeceği Eşleştiricisi sağlamak için emin olun, **dataTypeValue** algılayıcının eşleşen **dataType**. 

Aşağıdaki Eşleştiricisi mevcut matchers altına ekleyin. Anahtarları hizalanır ve boşluklar, sekmeler olarak değiştirilmez emin olun. Bu satırlar da içinde mevcut *provisionSample.yaml* dosyası olarak geçersiz kılınan satır. Bunları kaldırarak açıklamasını `#` önünde her satırın karakter.

```yaml
      - name: Matcher Temperature
        dataTypeValue: Temperature
```

Bu Eşleştiricisi eklediğiniz SAMPLE_SENSOR_TEMPERATURE algılayıcı izleyeceği [ilk öğreticide](tutorial-facilities-setup.md). 

<a id="udf"></a>

## <a name="create-a-user-defined-function"></a>Kullanıcı tanımlı işlev oluşturma

Kullanıcı tanımlı işlevler, sensör verileriniz işlenmesini özelleştirmek için kullanabilirsiniz. Azure dijital İkizlerini örneğinizin içinde matchers tarafından açıklandığı gibi belirli koşullar meydana geldiğinde çalıştırılabilen özel JavaScript kodu oldukları. Matchers ve izlemek istediğiniz her bir algılayıcı için kullanıcı tanımlı işlevler oluşturabilirsiniz. Daha fazla bilgi için okuma [veri işleme ve kullanıcı tanımlı işlevleri](concepts-user-defined-functions.md). 

Türü ile başlayan bir bölümde örnek provisionSample.yaml dosyasında arayın **userdefinedfunctions**. Bu bölüm olan bir kullanıcı tanımlı işlev sağlayan bir verilen **adı**. Altında matchers listesi bu UDF gerçekleştirildiği **matcherNames**. UDF için kendi JavaScript dosyanızı **script** bölümünde sağlayabilirsiniz.

Ayrıca **roleassignments** adlı bölüme de dikkat edin. Bu alan Yönetici rolü için kullanıcı tanımlı işlevi atar. Bu rol herhangi sağlanan alanları gelen olayları erişmesine izin verir. 

1. UDF ekleyerek veya aşağıdaki satırda uncommenting sıcaklık Eşleştiricisi içerecek şekilde yapılandırmak `matcherNames` provisionSample.yaml dosyasının düğümü:

    ```yaml
            - Matcher Temperature
    ```

1. Dosyayı açmak **src\actions\userDefinedFunctions\availability.js** düzenleyicinizde. Bu, başvurulan dosya **betik** provisionSample.yaml öğesidir. Bu dosyada bulunan kullanıcı tanımlı işlev koşulları için hiçbir hareket odada algılanan ve tasarruf edilen karbon dioksit düzeyleri 1.000 ppm arar. 

   İzleyici sıcaklık ve diğer koşulları JavaScript dosyasını değiştirin. Koşulları için hiçbir hareket odada algılanan, tasarruf edilen karbon dioksit düzeyler 1.000 ppm verilmiştir ve sıcaklık Fahrenhayt 78 olduğu için kod aşağıdaki satırları ekleyin.

   > [!NOTE]
   > Bu bölüm, dosyayı değiştirir *src\actions\userDefinedFunctions\availability.js* için bir kullanıcı tanımlı işlev yazmak ayrıntılı bir şekilde öğrenin. Ancak, dosyanın doğrudan kullanmayı da tercih edebilirsiniz [src\actions\userDefinedFunctions\availabilityForTutorial.js](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/userDefinedFunctions/availabilityForTutorial.js) kurulumunuzu içinde. Bu dosya, öğretici için gerekli olan tüm değişikliklere sahiptir. Bunun yerine bu dosyayı kullanırsanız, doğru dosya adı kullandığınızdan emin olun **betik** anahtarını [src\actions\provisionSample.yaml](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/provisionSample.yaml).

    a. Dosyanın en üstünde, `// Add your sensor type here` açıklamasının altına sıcaklık için şu satırları ekleyin:

    ```JavaScript
        var temperatureType = "Temperature";
        var temperatureThreshold = 78;
    ```

    b. Aşağıdaki satırları ekleyin, tanımlar ve sonra gelen deyim `var motionSensor`, açıklama aşağıda `// Add your sensor variable here`:

     ```JavaScript
        var temperatureSensor = otherSensors.find(function(element) {
            return element.DataType === temperatureType;
        });
    ```

    c. Aşağıdaki satırı ekleyin, tanımlar ve sonra gelen deyim `var carbonDioxideValue`, açıklama aşağıda `// Add your sensor latest value here`:

    ```JavaScript
        var temperatureValue = getFloatValue(temperatureSensor.Value().Value);
    ```

    d. Şu kod satırlarını `// Modify this line to monitor your sensor value` açıklamasının altından kaldırın:

     ```JavaScript
        if(carbonDioxideValue === null || motionValue === null) {
            sendNotification(telemetry.SensorId, "Sensor", "Error: Carbon dioxide or motion are null, returning");
            return;
        }
    ```

    Bunları şu satırlarla değiştirin:

    ```JavaScript
        if(carbonDioxideValue === null || motionValue === null || temperatureValue === null){
            sendNotification(telemetry.SensorId, "Sensor", "Error: Carbon dioxide, motion, or temperature are null, returning");
            return;
        }
    ```

    e. Şu kod satırlarını `// Modify these lines as per your sensor` açıklamasının altından kaldırın:

    ```JavaScript
        var availableFresh = "Room is available and air is fresh";
        var noAvailableOrFresh = "Room is not available or air quality is poor";
    ```

    Bunları şu satırlarla değiştirin:

    ```JavaScript
        var alert = "Room with fresh air and comfortable temperature is available.";
        var noAlert = "Either room is occupied, or working conditions are not right.";
    ```

    f. Şu *if-else* kod bloğunu `// Modify this code block for your sensor` açıklamasının altından kaldırın:

    ```JavaScript
        // If carbonDioxide less than threshold and no presence in the room => log, notify and set parent space computed value
        if(carbonDioxideValue < carbonDioxideThreshold && !presence) {
            log(`${availableFresh}. Carbon Dioxide: ${carbonDioxideValue}. Presence: ${presence}.`);
            setSpaceValue(parentSpace.Id, spaceAvailFresh, availableFresh);

            // Set up custom notification for air quality
            parentSpace.Notify(JSON.stringify(availableFresh));
        }
        else {
            log(`${noAvailableOrFresh}. Carbon Dioxide: ${carbonDioxideValue}. Presence: ${presence}.`);
            setSpaceValue(parentSpace.Id, spaceAvailFresh, noAvailableOrFresh);

            // Set up custom notification for air quality
            parentSpace.Notify(JSON.stringify(noAvailableOrFresh));
        }
    ```

    Sonrasında şu *if-else* bloğuyla değiştirin:

    ```JavaScript
        // If sensor values are within range and room is available
        if(carbonDioxideValue < carbonDioxideThreshold && temperatureValue < temperatureThreshold && !presence) {
            log(`${alert}. Carbon Dioxide: ${carbonDioxideValue}. Temperature: ${temperatureValue}. Presence: ${presence}.`);

            // log, notify and set parent space computed value
            setSpaceValue(parentSpace.Id, spaceAvailFresh, alert);

            // Set up notification for this alert
            parentSpace.Notify(JSON.stringify(alert));
        }
        else {
            log(`${noAlert}. Carbon Dioxide: ${carbonDioxideValue}. Temperature: ${temperatureValue}. Presence: ${presence}.`);

            // log, notify and set parent space computed value
            setSpaceValue(parentSpace.Id, spaceAvailFresh, noAlert);
        }
    ```

    Değiştirilen UDF, bir odanın kullanılabilir durumda olması, karbondioksit ve sıcaklık sınırlarının kabul edilen sınırlar içinde olması durumunu izler. Bu koşul yerine getirildiğinde `parentSpace.Notify(JSON.stringify(alert));` deyimiyle bir bildirim oluşturur. Sağlanan koşuldan bağımsız olarak izlenen alanın değerini ayarlayacak ve aşağıdaki iletiyi görüntüleyecektir.

    g. Dosyayı kaydedin.

1. Bir komut penceresi açın ve klasöre gidin **doluluk quickstart\src**. Uzamsal zeka grafiği ve kullanıcı tanımlı işlevi sağlamak için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    dotnet run ProvisionSample
    ```

   > [!IMPORTANT]
   > Dijital İkizlerini yönetim API'nize, yetkisiz erişimi önlemek için **doluluk-quickstart** uygulama Azure hesabı kimlik bilgilerinizle oturum açmanız gerekir. Her çalıştırdığınızda oturum açmak gerekmeyebilir için kısa bir süre için bilgilerinizi kaydeder. Bu programı ilk kez çalıştırır ve kaydedilen kimlik bilgilerinizi, daha sonra sona erdiğinde, uygulama oturum açma sayfasına yönlendirir ve bu sayfada girmek için bir oturum özgü kodu sağlar. Azure hesabınızda oturum açmak için yönergeleri izleyin.

1. Hesabınızı doğrulandıktan sonra uygulama provisionSample.yaml yapılandırılan bir örnek uzamsal grafik oluşturma başlar. Sağlama tamamlandığında kadar bekleyin. Birkaç dakika sürer. Bundan sonra komut penceresinde iletileri gözlemleyin ve uzamsal grafınızı nasıl oluşturulduğunu dikkat edin. Nasıl bir IOT hub'ı uygulama kök düğümünde oluşturur dikkat edin veya `Venue`.

1. Komut penceresinde çıktısını değerini kopyalayın `ConnectionString`altında `Devices` panonuza bir bölüm. Sonraki bölümde cihaz bağlantı benzetimini yapmak için bu değeri gerekir.

    ![Sağlama örneği](./media/tutorial-facilities-udf/run-provision-sample.png)

> [!TIP]
> Benzer bir hata iletisi alırsanız "g/ç işlemi bir iş parçacığı çıkış veya bir uygulama isteği nedeniyle ortasında sağlama iptal edildi", komutu yeniden çalıştırmayı deneyin. HTTP istemcisi bir ağ sorunu zaman aşımına uğradı alıyorsa bu durum gerçekleşebilir.

<a id="simulate"></a>

## <a name="simulate-sensor-data"></a>Sensör verilerinin simülasyonunu yapma

Bu bölümde, adlı proje kullanacağınız *cihaz bağlantısı* örnekteki. Hareket sıcaklık ve tasarruf edilen karbon dioksit saptamak için sensör verilerini benzetimini yapmak. Bu proje, sensörler için rastgele değerler oluşturur ve bunları cihaz bağlantı dizesini kullanarak IoT hub'a gönderir.

1. Azure dijital İkizlerini örneğe ayrı komut penceresinde, Git ve sonra **cihaz bağlantısı** klasör.

1. Projenizin bağımlılıklarının doğru olduğundan emin olmak için şu komutu çalıştırın:

    ```cmd/sh
    dotnet restore
    ```

1. Açık [appsettings.json](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/device-connectivity/appsettings.json) Düzenleyicisi'nde dosya ve aşağıdaki değerleri düzenleyin:

   a. **DeviceConnectionString**: Değeri atamak `ConnectionString` çıktı penceresinde önceki bölümde. Simülatör ile IOT hub'ı düzgün şekilde bağlanabilmesi için bu dize tamamen tırnak içine kopyalayın.

   b. **HardwareId** içinde **algılayıcılar** dizisi: Azure dijital İkizlerini Örneğinize sağlanan sensörlerden alınan olayları benzetiminin yapıldığı için donanım Kimliğini ve bu dosyadaki algılayıcı adlarını eşleşmelidir `sensors` provisionSample.yaml dosyasının düğümü.

      Sıcaklık algılayıcı için yeni bir giriş ekleyin. **Algılayıcılar** appsettings.json düğümünde, aşağıdaki gibi görünmelidir:

      ```JSON
      "Sensors": [{
        "DataType": "Motion",
        "HardwareId": "SAMPLE_SENSOR_MOTION"
      },{
        "DataType": "CarbonDioxide",
        "HardwareId": "SAMPLE_SENSOR_CARBONDIOXIDE"
      },{
        "DataType": "Temperature",
        "HardwareId": "SAMPLE_SENSOR_TEMPERATURE"
      }]
      ```

1. Sıcaklık, hareket ve karbondioksit için cihaz olayı simülasyonunu başlatmak üzere şu komutu çalıştırın:

    ```cmd/sh
    dotnet run
    ```

   > [!NOTE]
   > Benzetim örneği doğrudan dijital İkizlerini örneğinizle iletişim kurmaz olduğundan, sizden kimlik doğrulaması yapmanızı gerektirmez.

## <a name="get-results-of-the-user-defined-function"></a>Kullanıcı tanımlı işlevin sonuçlarını Al

Örneğiniz cihaz ve sensör verilerini her aldığında kullanıcı tanımlı işlev çalışır. Bu bölümde, kullanıcı tanımlı işlev sonuçlarını almak için Azure dijital İkizlerini örneğinizin sorgular. Oda uzaktan canlıyken ve sıcaklık doğru kullanılabilir olduğunda, neredeyse gerçek zamanlı olarak görürsünüz. 

1. Örnek veya yeni bir komut penceresi sağlamak ve Git için kullanılan komut penceresi açın **doluluk quickstart\src** örnek yeniden klasörü.

1. Aşağıdaki komutu çalıştırın ve istendiğinde oturum açın:

    ```cmd/sh
    dotnet run GetAvailableAndFreshSpaces
    ```

Çıkış penceresinde nasıl kullanıcı tanımlı işlevi çalıştıran ve cihaz benzetimi olaylardan durdurur gösterir. 

   ![UDF için çıkış](./media/tutorial-facilities-udf/udf-running.png)

İzlenen koşul karşılanıyorsa, gördüğümüz gibi kullanıcı tanımlı işlev ilgili ileti alanıyla değerini ayarlar [önceki](#udf). `GetAvailableAndFreshSpaces` İşlevi yazdırır konsolda bir ileti.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu noktada Azure dijital İkizlerini keşfetmeye durdurmak istiyorsanız, bu öğreticide oluşturulan kaynakları silmek çekinmeyin:

1. Sol menüden [Azure portalında](https://portal.azure.com)seçin **tüm kaynakları**dijital İkizlerini kaynak grubunuzu seçin ve seçin **Sil**.

    > [!TIP]
    > Dijital İkizlerini örneğinizin silme sorun olduysa, bir hizmet güncelleştirmesi düzeltme alındı. Örneğiniz silme yeniden deneyin.

2. Gerekirse, iş makinenizde örnek uygulamaları silin.

## <a name="next-steps"></a>Sonraki adımlar

Sağlanan, boşluk ve özel bildirimleri tetiklemek için bir çerçeve oluşturan göre aşağıdaki öğreticilerde birini gidebilirsiniz:

> [!div class="nextstepaction"]
> [Öğretici: Logic Apps kullanarak, Azure dijital İkizlerini boşlukları bildirimleri alma](tutorial-facilities-events.md)

> [!div class="nextstepaction"]
> [Öğretici: Time Series Insights'ı kullanarak Azure dijital İkizlerini alanlarınıza olayları çözümleyin](tutorial-facilities-analyze.md)
