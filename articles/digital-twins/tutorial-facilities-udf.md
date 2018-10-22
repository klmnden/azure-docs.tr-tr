---
title: Azure Digital Twins ile alan izleme | Microsoft Docs
description: Azure Digital Twins ile uzamsal kaynaklarınızı sağlamayı ve çalışma koşullarını izlemeyi öğrenmek için bu öğreticideki adımları uygulayın.
services: digital-twins
author: dsk-2015
ms.service: digital-twins
ms.topic: tutorial
ms.date: 10/15/2018
ms.author: dkshir
ms.openlocfilehash: 1e5cb18b4e526cd0a0607f5bc93788fcf07430e1
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49364244"
---
# <a name="tutorial-provision-your-building-and-monitor-working-conditions-with-azure-digital-twins"></a>Öğretici: Azure Digital Twins ile binanızda sağlama yapma ve çalışma koşullarını izleme

Bu öğreticide sıcaklık koşullarının ve konfor düzeyinin istediğiniz gibi olup olmadığını belirleme amacıyla alanlarınızı izlemek için Azure Digital Twins'i kullanma adımları gösterilmektedir. [Örnek binanızı yapılandırdıktan](tutorial-facilities-setup.md) sonra bu öğreticideki adımları kullanarak binanızda sağlama yapıp sensör verilerinizle özel işlevler çalıştırabilirsiniz.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * İzleme koşullarını tanımlama
> * Kullanıcı tanımlı işlev oluşturma
> * Sensör verilerinin simülasyonunu yapma
> * Kullanıcı tanımlı işlevin sonuçlarını alma

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide [Azure Digital Twins kurulumunu yapılandırmış olduğunuz](tutorial-facilities-setup.md) varsayılır. Devam etmeden önce aşağıdakilere sahip olduğunuzdan emin olun:
- Bir [Azure hesabı](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- Çalışan bir Digital Twins örneği 
- Çalışma makinenize indirilmiş ve ayıklanmış [Digital Twins C# örnekleri](https://github.com/Azure-Samples/digital-twins-samples-csharp) 
- Örneği derlemek ve çalıştırmak için geliştirme makinenize yüklenmiş [.NET Core SDK sürüm 2.1.403 veya üzeri](https://www.microsoft.com/net/download). Doğru sürümün yüklü olup olmadığını denetlemek için `dotnet --version` komutunu çalıştırın. 
- Örnek kodu incelemek için [Visual Studio Code](https://code.visualstudio.com/). 

## <a name="define-conditions-to-monitor"></a>İzleme koşullarını tanımlama
Cihaz veya sensör verilerini izlemek için belirli koşulları içeren ve **Eşleştiriciler** olarak adlandırılan bir küme tanımlayabilirsiniz. Ardından eşleştiriciler tarafından belirtilen koşullar gerçekleştiğinde alanlarınızdan ve cihazlarınızdan gelen verilerde özel mantık çalıştıran ve *kullanıcı tanımlı işlevler* olarak adlandırılan işlevler tanımlayacaksınız. Daha fazla bilgi için bkz. [Veri İşleme ve Kullanıcı Tanımlı İşlevler](concepts-user-defined-functions.md). 

**_occupancy-quickstart_** örnek projesinin **_src\actions\provisionSample.yaml_** adlı dosyasını Visual Studio Code'da açın. **matchers** türü ile başlayan bölümü bulun. Bu türün altındaki her giriş, belirtilen **Name** değerine sahip olan ve **dataTypeValue** türündeki bir sensörü izleyecek olan bir eşleştirici oluşturur. Birkaç **sensors** içeren **devices** düğümünün bulunduğu *Focus Room A1* adlı alanla ilişkilendirilmiş olduğunu görebilirsiniz. Bu sensörlerden birini izleyecek bir eşleştirici sağlamak için **dataTypeValue** değerinin sensörün **dataType** değeriyle eşleşmesi gerekir. 

Aşağıdaki eşleştiriciyi, var olan eşleştiricilerin altına ekleyin. Anahtarların aynı hizada olduğundan ve boşlukların sekmeyle değiştirilmediğinden emin olun:

```yaml
      - name: Matcher Temperature
        dataTypeValue: Temperature
```

Bu eşleştirici, [birinci öğreticide](tutorial-facilities-setup.md) eklediğiniz *SAMPLE_SENSOR_TEMPERATURE* sensörünü izleyecektir. Bu satırlar *provisionSample.yaml* dosyasında açıklama satırlarıyla sağlanmıştır. Satırların önündeki '#' karakterini silerek bunları açıklama olmaktan çıkarabilirsiniz. 

<a id="udf" />

## <a name="create-a-user-defined-function"></a>Kullanıcı tanımlı işlev oluşturma
Kullanıcı tanımlı işlevler (UDF) sensör verilerinizin işlenme adımlarını özelleştirmenizi sağlar. Bu işlevler, eşleştiriciler tarafından tanımlanan belirli koşullar gerçekleştiğinde Digital Twins örneğinizde çalışan özel JavaScript kodlarıdır. İzlemek istediğiniz her sensör için farklı *eşleştiriciler* ve *kullanıcı tanımlı işlevler* oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Veri işleme ve kullanıcı tanımlı işlevler](concepts-user-defined-functions.md). 

Örnek *provisionSample.yaml* dosyasında **userdefinedfunctions** türüyle başlayan bölümü bulun. Bu bölümde belirli **Name** değerine sahip olan ve **matcherNames** altındaki eşleştirici listesine göre harekete geçen kullanıcı tanımlı bir işlev bulunur. UDF için kendi JavaScript dosyanızı **script** bölümünde sağlayabilirsiniz. Ayrıca **roleassignments** adlı bölüme de dikkat edin. Bu bölüm, sağlanan alanların herhangi birinden gelen olaylara erişmesi için kullanıcı tanımlı işleve *Alan Yöneticisi* rolünü atar. 

1. *provisionSample.yaml* adlı dosyanın `matcherNames` düğümüne aşağıdaki satırı ekleyerek veya var olan satırın açıklamasını kaldırarak UDF'yi sıcaklık eşleştiricisini içerecek şekilde yapılandırın:

    ```yaml
            - Matcher Temperature
    ```

1. **_src\actions\userDefinedFunctions\availability.js_** dosyasını düzenleyicinizde açın. Bu, *provisionSample.yaml* dosyasının **script** öğesinde başvurulan dosyadır. Bu dosyadaki kullanıcı tanımlı işlev, odada herhangi bir hareket algılanmaması ve karbondioksit düzeylerinin 1000 ppm değerinin altına düşmesi durumlarını izler. JavaScript dosyasını, diğer koşullara ek olarak sıcaklık koşullarını da içerecek şekilde değiştirin. Aşağıdaki kod satırlarını ekleyerek odada herhangi bir hareket algılanmaması, karbondioksit düzeylerinin 1000 ppm değerinin altına düşmesi ve sıcaklığın 78 Fahrenhayt seviyesinin altına inmesi durumlarının izlenmesini sağlayın.

   > [!NOTE]
   > Bu bölüm *src\actions\userDefinedFunctions\availability.js* dosyasını değiştirir ve kullanıcı tanımlı işlev yazma yöntemlerinin birini öğrenmenizi sağlar. Ancak isterseniz mevcut düzeninizde [src\actions\userDefinedFunctions\availabilityForTutorial.js](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/userDefinedFunctions/availabilityForTutorial.js) adlı dosyayı da doğrudan kullanabilirsiniz. Bu dosya, öğretici için gerekli olan tüm değişikliklere sahiptir. Bu dosyayı kullanırsanız [src\actions\provisionSample.yaml](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/provisionSample.yaml) dosyasındaki **_script_** anahtarına doğru dosya adını sağladığınızdan emin olun.

    1. Dosyanın en üstünde, `// Add your sensor type here` açıklamasının altına sıcaklık için şu satırları ekleyin:

        ```JavaScript
            var temperatureType = "Temperature";
            var temperatureThreshold = 78;
        ```
   
    1. Şu satırları `var motionSensor` bölümünü tanımlayan ifadenin sonrasına, `// Add your sensor variable here` açıklamasının altına ekleyin:

        ```JavaScript
            var temperatureSensor = otherSensors.find(function(element) {
                return element.DataType === temperatureType;
            });
        ```
    
    1. Şu satırı `var carbonDioxideValue` bölümünü tanımlayan ifadenin sonrasına, `// Add your sensor latest value here` açıklamasının altına ekleyin:

        ```JavaScript
            var temperatureValue = getFloatValue(temperatureSensor.Value().Value);
        ```
    
    1. Şu kod satırlarını `// Modify this line to monitor your sensor value` açıklamasının altından kaldırın: 

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
    
    1. Şu kod satırlarını `// Modify these lines as per your sensor` açıklamasının altından kaldırın:

        ```JavaScript
            var availableFresh = "Room is available and air is fresh";
            var noAvailableOrFresh = "Room is not available or air quality is poor";
        ```

       Bunları şu satırlarla değiştirin:

        ```JavaScript
            var alert = "Room with fresh air and comfortable temperature is available.";
            var noAlert = "Either room is occupied, or working conditions are not right.";
        ```
    
    1. Şu *if-else* kod bloğunu `// Modify this code block for your sensor` açıklamasının altından kaldırın:

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
        
        Değiştirilen UDF, bir odanın kullanılabilir durumda olması, karbondioksit ve sıcaklık sınırlarının kabul edilen sınırlar içinde olması durumunu izler. Bu koşul yerine getirildiğinde `parentSpace.Notify(JSON.stringigy(alert));` deyimiyle bir bildirim oluşturur. Sağlanan koşuldan bağımsız olarak izlenen alanın değerini ayarlayacak ve aşağıdaki iletiyi görüntüleyecektir.
    
    1. Dosyayı kaydedin. 
    
1. Komut penceresini açın ve **_occupancy-quickstart\src_** klasörüne gidin. Uzamsal akıllı grafınızı ve kullanıcı tanımlı işlevinizi sağlamak için aşağıdaki komutu çalıştırın. 

    ```cmd/sh
    dotnet run ProvisionSample
    ```

   > [!IMPORTANT]
   > Digital Twins yönetim API'nize yetkisiz erişimi önleme amacıyla **_occupancy-quickstart_** uygulaması için Azure hesabı kimlik bilgilerinizle oturum açmanız gerekir. Kimlik bilgileriniz kısa bir süre boyunca kaydedilir ve bu sayede uygulamayı her çalıştırdığınızda yeniden oturum açmanız gerekmez. Programı ilk kez çalıştırdığınızda ve kimlik bilgilerinizin süresi dolduktan sonra oturum açma sayfası açılır ve bu sayfaya girmeniz gereken, oturuma özgü bir kod verilir. Azure hesabınızda oturum açmak için yönergeleri izleyin.


1. Hesabınızda kimlik doğrulaması gerçekleştirildikten sonra uygulama *provisionSample.yaml* dosyasında yapılandırılan şekilde örnek bir uzamsal graf oluşturmaya başlar. Sağlama tamamlanana kadar bekleyin. Bu işlem birkaç dakika sürebilir. Tamamlandıktan sonra komut penceresinin altındaki komutları inceleyin ve oluşturulan uzamsal grafınızı inceleyin. `Venue` öğesinin kök düğümünde bir IoT hub'ı oluşturulduğunu göreceksiniz. 

1. Komut penceresindeki çıktıda `Devices` bölümünün altında bulunan `ConnectionString` değerini panonuza kopyalayın. Aşağıdaki bölümde cihaz simülasyonu gerçekleştirmek için bu değere ihtiyacınız olacak.

    ![Örneği Sağlama](./media/tutorial-facilities-udf/run-provision-sample.png)

> [!TIP]
> Sağlama işleminin ortasında "İş parçacığı çıkışı veya uygulama isteği nedeniyle G/Ç işlemi iptal edildi" gibi bir hata iletisiyle karşılaşırsanız lütfen komutu yeniden çalıştırmayı deneyin. Bu durum HTTP istemcisinin bir ağ sorunu nedeniyle zaman aşımına uğraması halinde gerçekleşebilir.

<a id="simulate" />

## <a name="simulate-sensor-data"></a>Sensör verilerinin simülasyonunu yapma
Bu bölümde örnekteki *device-connectivity* adlı projeyi kullanarak hareket, sıcaklık ve karbondioksit algılamak için sensör verilerinin simülasyonunu yapacaksınız. Bu proje, sensörler için rastgele değerler oluşturur ve bunları cihaz bağlantı dizesini kullanarak IoT hub'a gönderir.

1. Ayrı bir komut penceresinde Digital Twins örneğine ve ardından **_device-connectivity_** klasörüne gidin.

1. Projenizin bağımlılıklarının doğru olduğundan emin olmak için şu komutu çalıştırın:

    ```cmd/sh
    dotnet restore
    ```

1. *appSettings.json* dosyasını düzenleyicinizde açın ve şu değerleri düzenleyin:
    1. *DeviceConnectionString*: Bir önceki bölümde çıktı penceresinde görünen `ConnectionString` değerini atayın. Simülatörün IoT hub'a doğru şekilde bağlanması için tırnak işaretlerini dahil etmeden bu dizenin tamamını kopyalayın.

    1. *Sensors* dizisindeki *HardwareId*: Digital Twins örneğinizde sağlanan sensörlerden olay simülasyonu gerçekleştirdiğiniz için bu dosyadaki sensör donanım kimliklerinin ve adlarının *provisionSample.yaml* dosyasının `sensors` düğümündekilerle eşleşmesi gerekir. Sıcaklık sensörü için yeni bir giriş ekleyin. *appSettings.json* dosyasının **Sensors** düğümü aşağıdaki gibi görünmelidir:

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
   > Simülasyon örneği Digital Twins örneğinizle doğrudan iletişim kurmadığından kimlik doğrulamanıza gerek yoktur.

## <a name="get-results-of-user-defined-function"></a>Kullanıcı tanımlı işlevin sonuçlarını alma
Örneğiniz cihaz ve sensör verilerini her aldığında kullanıcı tanımlı işlev çalışır. Bu bölüm Digital Twins örneğinizi sorgulayarak kullanıcı tanımlı işlevin sonuçlarını alır. Odanın uygun, havanın temiz ve sıcaklığın doğru olup olmadığını neredeyse gerçek zamanlı olarak görebilirsiniz. 

1. Örneği sağlamak için kullandığınız komut penceresini veya yeni bir komut penceresi açın ve yeniden örneğin **_occupancy-quickstart\src_** klasörüne gidin. 

1. Aşağıdaki komutu çalıştırın ve istendiğinde oturum açın:

    ```cmd/sh
    dotnet run GetAvailableAndFreshSpaces
    ```

Çıkış penceresinde, kullanıcı tanımlı işlevin nasıl yürütüldüğü ve cihaz simülasyonu verilerini nasıl aldığı gösterilir. 

   ![UDF'yi yürütme](./media/tutorial-facilities-udf/udf-running.png)

Kullanıcı tanımlı işlev, izlenen koşulun karşılanıp karşılanmadığına bağlı olarak alanın değerini [yukarıdaki bölümde](#udf) gördüğümüz iletiyle birlikte belirler ve bu değer `GetAvailableAndFreshSpaces` işlevi tarafından konsola yazdırılır. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu kadar Azure Digital Twins incelemesi sizin için yeterliyse bu öğreticide oluşturulan kaynakları silebilirsiniz:

1. [Azure portalda](http://portal.azure.com) sol taraftaki menüden **Tüm kaynaklar**'a tıklayın, Digital Twins kaynak grubunuzu ve ardından **Sil**'i seçin.
2. Gerekirse çalışma makinenizdeki örnek uygulamaları da silebilirsiniz. 


## <a name="next-steps"></a>Sonraki adımlar

Alanlarınızı sağlayıp özel bildirimleri tetikleyecek bir çerçeve oluşturduğunuza göre aşağıdaki öğreticilerden herhangi biriyle devam edebilirsiniz. 

> [!div class="nextstepaction"]
> [Öğretici: Logic Apps'i kullanarak Azure Digital Twins alanlarınızdan bildirim alma](tutorial-facilities-events.md)

Veya

> [!div class="nextstepaction"]
> [Öğretici: Time Series Insights'ı kullanarak Azure Digital Twins alanlarınızdan gelen olayları görselleştirme ve analiz etme](tutorial-facilities-analyze.md)
