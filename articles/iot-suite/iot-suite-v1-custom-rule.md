---
title: "Azure IOT paketindeki özel bir kural oluşturun | Microsoft Docs"
description: "IOT paketi önceden yapılandırılmış çözümde özel bir kural oluşturmak nasıl."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 562799dc-06ea-4cdd-b822-80d1f70d2f09
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: dobett
ms.openlocfilehash: 9bf2a13035de141766fd935966ce18459dccdaab
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2017
---
# <a name="create-a-custom-rule-in-the-remote-monitoring-preconfigured-solution"></a>Uzaktan izleme önceden yapılandırılmış çözümde özel bir kural oluşturun

## <a name="introduction"></a>Giriş

Önceden yapılandırılmış çözümler, yapılandırdığınız [bir telemetri zaman tetikleyen kuralların değeri bir cihaz belirli bir eşiğe ulaştığında için][lnk-builtin-rule]. [Uzaktan izleme ile kullanım dinamik telemetri önceden yapılandırılmış çözüm] [ lnk-dynamic-telemetry] özel telemetri değerleri gibi nasıl ekleyebileceğinizi açıklar *ExternalTemperature* çözümünüze. Bu makalede, çözümünüz dinamik telemetri türleri için özel bir kural oluşturulacağını gösterir.

Bu öğretici, önceden yapılandırılmış çözüm arka ucuna göndermek için dinamik telemetri oluşturmak için basit bir Node.js sanal cihaz kullanır. Ardından özel kurallarında ekleyin **RemoteMonitoring** Visual Studio çözümü ve özelleştirilmiş bu arka uç Azure aboneliğinize dağıtın.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Etkin bir Azure aboneliği. Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü][lnk_free_trial].
* [Node.js] [ lnk-node] sürüm 0.12.x sürümü veya daha sonra bir sanal cihaz oluşturmak için.
* Visual Studio 2015 veya Visual Studio 2017 önceden yapılandırılmış çözüm arka değiştirmek için yeni kurallarınızı ile bitmelidir.

[!INCLUDE [iot-suite-v1-provision-remote-monitoring](../../includes/iot-suite-v1-provision-remote-monitoring.md)]

Dağıtımınız için seçtiğiniz çözüm adına not edin. Bu çözüm adı daha sonra Bu öğreticide gerekir.

[!INCLUDE [iot-suite-v1-send-external-temperature](../../includes/iot-suite-v1-send-external-temperature.md)]

Gönderme olduğunu belirlediğinizde Node.js konsol uygulaması durdurabilirsiniz **ExternalTemperature** önceden yapılandırılmış çözümü telemetri. Çözüme özel kural ekledikten sonra bu Node.js konsol uygulaması yeniden çalıştırmak için konsol penceresi açık tutun.

## <a name="rule-storage-locations"></a>Kural depolama konumları

Kurallar hakkında bilgileri iki konumda kalıcıdır:

* **DeviceRulesNormalizedTable** tablo – bu tablo, çözüm portalı tarafından tanımlanan kurallar normalleştirilmiş başvuru depolar. Çözüm portalı cihaz kuralları görüntülediğinde, kural tanımları için bu tabloyu sorgular.
* **DeviceRules** blob – bu blob tüm kayıtlı cihazlar ve Azure akış analizi işi Giriş bir başvuru olarak tanımlanan tanımlanan tüm kuralları depolar.
 
Mevcut bir kuralı güncelleştirmek veya yeni bir kural çözüm Portalı'nda tanımlamak, tablo ve blob değişiklikleri yansıtacak şekilde güncelleştirilir. Portalda görüntülenen kural tanımı tablo Mağazası'ndan gelen ve akış analizi işleri tarafından başvurulan kural tanımı blobundan sunar. 

## <a name="update-the-remotemonitoring-visual-studio-solution"></a>Güncelleştirme RemoteMonitoring Visual Studio çözümü

Aşağıdaki adımlar RemoteMonitoring Visual Studio çözümü kullanan yeni bir kural eklemek için değiştirme gösterir **ExternalTemperature** benzetimli aygıtından gönderilen telemetri:

1. Zaten yapmadıysanız, kopyalama **azure-iot-remote-monitoring** aşağıdaki Git komutu kullanarak, yerel makinenizde uygun bir konuma deposu:

    ```
    git clone https://github.com/Azure/azure-iot-remote-monitoring.git
    ```

2. Visual Studio'da yerel, kopyasını RemoteMonitoring.sln dosyayı açın **azure-iot-remote-monitoring** deposu.

3. Infrastructure\Models\DeviceRuleBlobEntity.cs dosyasını açın ve eklemek bir **ExternalTemperature** özelliğini aşağıdaki gibi:

    ```csharp
    public double? Temperature { get; set; }
    public double? Humidity { get; set; }
    public double? ExternalTemperature { get; set; }
    ```

4. Aynı dosyada eklemek bir **ExternalTemperatureRuleOutput** özelliğini aşağıdaki gibi:

    ```csharp
    public string TemperatureRuleOutput { get; set; }
    public string HumidityRuleOutput { get; set; }
    public string ExternalTemperatureRuleOutput { get; set; }
    ```

5. Infrastructure\Models\DeviceRuleDataFields.cs dosyasını açın ve aşağıdakileri ekleyin **ExternalTemperature** varolan sonra özelliği **nem** özelliği:

    ```csharp
    public static string ExternalTemperature
    {
        get { return "ExternalTemperature"; }
    }
    ```

6. Aynı dosyada güncelleştirme **_availableDataFields** içerecek şekilde yöntemi **ExternalTemperature** gibi:

    ```csharp
    private static List<string> _availableDataFields = new List<string>
    {                    
        Temperature, Humidity, ExternalTemperature
    };
    ```

7. Infrastructure\Repository\DeviceRulesRepository.cs dosyasını açın ve değiştirme **BuildBlobEntityListFromTableRows** yöntemini aşağıdaki şekilde:

    ```csharp
    else if (rule.DataField == DeviceRuleDataFields.Humidity)
    {
        entity.Humidity = rule.Threshold;
        entity.HumidityRuleOutput = rule.RuleOutput;
    }
    else if (rule.DataField == DeviceRuleDataFields.ExternalTemperature)
    {
      entity.ExternalTemperature = rule.Threshold;
      entity.ExternalTemperatureRuleOutput = rule.RuleOutput;
    }
    ```

## <a name="rebuild-and-redeploy-the-solution"></a>Yeniden oluşturun ve çözümü yeniden dağıtın.

Artık Azure aboneliğinize güncellenen çözümü dağıtabilirsiniz.

1. Yükseltilmiş bir komut istemi açın ve azure-iot-remote-monitoring depoyu yerel kopyasını kök dizinine gidin.

2. Güncelleştirilmiş çözümünüzü dağıtmak için aşağıdaki komutu değiştirerek çalıştırın **{dağıtım adı}** önceden yapılandırılmış çözüm dağıtımınız, daha önce not ettiğiniz adı:

    ```
    build.cmd cloud release {deployment name}
    ```

## <a name="update-the-stream-analytics-job"></a>Stream Analytics işi güncelleştir

Dağıtım tamamlandığında, yeni kural tanımları kullanmak için Stream Analytics işi güncelleştirebilirsiniz.

1. Azure portalında, önceden yapılandırılmış çözüm kaynaklarınızı içeren kaynak grubuna gidin. Bu kaynak grubu çözüm için dağıtım sırasında belirtilen aynı ada sahip.

2. {Dağıtım adına} gitmek-kuralları Stream Analytics işi. 

3. Tıklatın **durdurmak** çalışmasını Stream Analytics işi durdurmak için. (Sorgu düzenleyebilmeniz durdurmak iş akışında için beklemeniz gerekir).

4. Tıklatın **sorgu**. Sorguyu içerecek şekilde düzenleyin **seçin** bildirimi **ExternalTemperature**. Aşağıdaki örnek, tam bir sorgu yeni gösterir **seçin** deyimi:

    ```
    WITH AlarmsData AS 
    (
    SELECT
         Stream.IoTHub.ConnectionDeviceId AS DeviceId,
         'Temperature' as ReadingType,
         Stream.Temperature as Reading,
         Ref.Temperature as Threshold,
         Ref.TemperatureRuleOutput as RuleOutput,
         Stream.EventEnqueuedUtcTime AS [Time]
    FROM IoTTelemetryStream Stream
    JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
    WHERE
         Ref.Temperature IS NOT null AND Stream.Temperature > Ref.Temperature
     
    UNION ALL
     
    SELECT
         Stream.IoTHub.ConnectionDeviceId AS DeviceId,
         'Humidity' as ReadingType,
         Stream.Humidity as Reading,
         Ref.Humidity as Threshold,
         Ref.HumidityRuleOutput as RuleOutput,
         Stream.EventEnqueuedUtcTime AS [Time]
    FROM IoTTelemetryStream Stream
    JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
    WHERE
         Ref.Humidity IS NOT null AND Stream.Humidity > Ref.Humidity
     
    UNION ALL
     
    SELECT
         Stream.IoTHub.ConnectionDeviceId AS DeviceId,
         'ExternalTemperature' as ReadingType,
         Stream.ExternalTemperature as Reading,
         Ref.ExternalTemperature as Threshold,
         Ref.ExternalTemperatureRuleOutput as RuleOutput,
         Stream.EventEnqueuedUtcTime AS [Time]
    FROM IoTTelemetryStream Stream
    JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
    WHERE
         Ref.ExternalTemperature IS NOT null AND Stream.ExternalTemperature > Ref.ExternalTemperature
    )
     
    SELECT *
    INTO DeviceRulesMonitoring
    FROM AlarmsData
     
    SELECT *
    INTO DeviceRulesHub
    FROM AlarmsData
    ```

5. Tıklatın **kaydetmek** güncelleştirilmiş kural sorguyu değiştirmek için.

6. Tıklatın **Başlat** yeniden çalıştırmayı Stream Analytics işini başlatmak için.

## <a name="add-your-new-rule-in-the-dashboard"></a>Panosunda, yeni bir kural ekleyin

Artık ekleyebilirsiniz **ExternalTemperature** çözüm panosunda bir aygıta kuralı.

1. Çözüm Portalı'na gidin.

2. Gidin **aygıtları** paneli.

3. Gönderen, oluşturduğunuz özel cihaz bulun **ExternalTemperature** telemetri ve **cihaz ayrıntıları** öğesine tıklayın **Kuralı Ekle**.

4. Seçin **ExternalTemperature** içinde **veri alanı**.

5. Ayarlama **eşik** 56 için. Ardından **kaydedin ve kuralları**.

6. Uyarı geçmişini görüntülemek için panoya geri dönün.

7. Açık sol konsol penceresinde göndermeye başlamak için Node.js konsol uygulaması başlangıç **ExternalTemperature** telemetri verileri.

8. Dikkat **Alarm geçmişi** tablo yeni uyarılar yeni kuralı tetiklendiğinde gösterir.
 
## <a name="additional-information"></a>Ek bilgiler

İşleç değiştirme  **>**  daha karmaşıktır ve Bu öğreticide özetlenen adımları ötesine geçer. İstediğiniz ne olursa olsun işleci kullanmak için Stream Analytics işi değiştirebilirsiniz, ancak bu çözüm portalı işlecinde yansıtma daha karmaşık bir görevdir. 

## <a name="next-steps"></a>Sonraki adımlar
Özel kurallar oluşturmak öğrendiniz, önceden yapılandırılmış çözümleri hakkında daha fazla bilgi edinebilirsiniz:

- [Mantıksal uygulama, Azure IOT paketi uzaktan önceden yapılandırılmış izleme çözümüne bağlama][lnk-logic-app]
- [Uzaktan izleme aygıt bilgileri meta verileri önceden yapılandırılmış çözüm][lnk-devinfo].

[lnk-devinfo]: iot-suite-v1-remote-monitoring-device-info.md

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-node]: http://nodejs.org
[lnk-builtin-rule]: iot-suite-v1-getstarted-preconfigured-solutions.md#view-alarms
[lnk-dynamic-telemetry]: iot-suite-v1-dynamic-telemetry.md
[lnk-logic-app]: iot-suite-v1-logic-apps-tutorial.md