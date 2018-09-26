---
title: Azure IOT paketi özel bir kural oluşturun | Microsoft Docs
description: Nasıl bir IOT paketi önceden yapılandırılmış çözümde özel bir kural oluşturun.
services: ''
suite: iot-suite
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: 562799dc-06ea-4cdd-b822-80d1f70d2f09
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: dobett
ms.openlocfilehash: 9bf2a13035de141766fd935966ce18459dccdaab
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47092637"
---
# <a name="create-a-custom-rule-in-the-remote-monitoring-preconfigured-solution"></a>Uzaktan izleme çözümünde özel kural oluşturma

## <a name="introduction"></a>Giriş

Önceden yapılandırılmış çözümler, yapılandırabileceğiniz [bir telemetri tetikleyeceğinizi kuralları, bir cihaz belirli bir eşiğe ulaştığında için değer][lnk-builtin-rule]. [Uzaktan izleme ile dinamik telemetri kullanma önceden yapılandırılmış çözüm] [ lnk-dynamic-telemetry] özel telemetri değerleri gibi nasıl ekleyebileceğinizi açıklar *ExternalTemperature* çözümünüze. Bu makalede, çözümünüzdeki dinamik telemetri türleri için özel kural oluşturma işlemini gösterir.

Bu öğreticide basit bir Node.js sanal cihaz için önceden yapılandırılmış çözüm arka ucu göndermek için dinamik telemetri oluşturmak için kullanılır. Özel kuralları eklersiniz **RemoteMonitoring** Visual Studio çözümü ve özelleştirilmiş bu arka uç Azure aboneliğinize dağıtın.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Etkin bir Azure aboneliği. Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü][lnk_free_trial].
* [Node.js] [ lnk-node] sürüm 0.12.x sürümü veya daha sonra bir sanal cihaz oluşturma.
* Visual Studio 2015 veya Visual Studio 2017 önceden yapılandırılmış çözüm arka değiştirmek için yeni kurallarınızı sonlandırın.

[!INCLUDE [iot-suite-v1-provision-remote-monitoring](../../includes/iot-suite-v1-provision-remote-monitoring.md)]

Dağıtımınız için seçtiğiniz çözüm adına not edin. Bu öğreticide daha sonra bu çözüm adı ihtiyacınız var.

[!INCLUDE [iot-suite-v1-send-external-temperature](../../includes/iot-suite-v1-send-external-temperature.md)]

Gönderme olduğunu doğruladıktan Node.js konsol uygulaması durdurabilirsiniz **ExternalTemperature** önceden yapılandırılmış çözümü için telemetri. Çözüme özel kural ekledikten sonra bu Node.js konsol uygulaması yeniden çalıştırmak için konsol penceresini açık tutun.

## <a name="rule-storage-locations"></a>Kural depolama konumları

Kurallarla ilgili bilgiler iki konumda kalıcı hale getirilir:

* **DeviceRulesNormalizedTable** tablo – bu tablo, çözüm portalı tarafından tanımlanan kuralların normalleştirilmiş bir başvuru depolar. Çözüm portalında cihaz kuralları görüntülediğinde, kural tanımları için bu tabloyu sorgular.
* **DeviceRules** blob – bu blob, tüm kayıtlı cihazlara ve Azure Stream Analytics işleri için Giriş bir başvuru olarak tanımlanan tanımlanan tüm kuralların depolar.
 
Mevcut bir kuralı güncelleştirmek ya da çözüm portalında yeni bir kural tanımlamak tablo ve blob değişiklikleri yansıtacak şekilde güncelleştirilir. Portalı'nda görüntülenen kural tanımı tablo depolama alanından gelir ve Stream Analytics işleri tarafından başvurulan kural tanımı blobundan gelir. 

## <a name="update-the-remotemonitoring-visual-studio-solution"></a>Güncelleştirme RemoteMonitoring Visual Studio çözümü

Aşağıdaki adımlarda kullanan yeni bir kural eklemek için RemoteMonitoring Visual Studio çözümünü değiştirmek gösterilmektedir **ExternalTemperature** sanal CİHAZDAN gönderilen telemetri:

1. Zaten yapmadıysanız, kopyalama **azure-IOT-remote-monitoring** depoya aşağıdaki Git komutunu kullanarak yerel makinenizde uygun bir konum:

    ```
    git clone https://github.com/Azure/azure-iot-remote-monitoring.git
    ```

2. Visual Studio'da RemoteMonitoring.sln dosyanın yerel kopyasını açın **azure-IOT-remote-monitoring** depo.

3. Infrastructure\Models\DeviceRuleBlobEntity.cs dosyasını açın ve eklemek bir **ExternalTemperature** özelliğini aşağıdaki gibi:

    ```csharp
    public double? Temperature { get; set; }
    public double? Humidity { get; set; }
    public double? ExternalTemperature { get; set; }
    ```

4. Aynı dosyada ekleyin bir **ExternalTemperatureRuleOutput** özelliğini aşağıdaki gibi:

    ```csharp
    public string TemperatureRuleOutput { get; set; }
    public string HumidityRuleOutput { get; set; }
    public string ExternalTemperatureRuleOutput { get; set; }
    ```

5. Infrastructure\Models\DeviceRuleDataFields.cs dosyasını açın ve aşağıdakini ekleyin **ExternalTemperature** varolan sonra özellik **nem** özelliği:

    ```csharp
    public static string ExternalTemperature
    {
        get { return "ExternalTemperature"; }
    }
    ```

6. Aynı dosyada, güncelleştirme **_availableDataFields** içerecek şekilde yöntemi **ExternalTemperature** gibi:

    ```csharp
    private static List<string> _availableDataFields = new List<string>
    {                    
        Temperature, Humidity, ExternalTemperature
    };
    ```

7. Infrastructure\Repository\DeviceRulesRepository.cs dosya açıp değiştirdiğinizde **BuildBlobEntityListFromTableRows** yöntemini aşağıdaki şekilde:

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

Artık Azure aboneliğinize da güncelleştirilen çözümü dağıtabilirsiniz.

1. Yükseltilmiş bir komut istemi açın ve yerel kopyanızı azure-IOT-remote-monitoring deposunun kök dizinine gidin.

2. Güncelleştirilmiş çözümünüzü dağıtmak için değiştirerek aşağıdaki komutu çalıştırarak **{dağıtım adı}** önceden yapılandırılmış çözümü dağıtımınızı daha önce not ettiğiniz adı:

    ```
    build.cmd cloud release {deployment name}
    ```

## <a name="update-the-stream-analytics-job"></a>Stream Analytics işini güncelleştirme

Dağıtım tamamlandığında, yeni kural tanılarını kullanmak için Stream Analytics işi güncelleştirebilirsiniz.

1. Azure portalında, önceden yapılandırılmış çözüm kaynakları içeren kaynak grubuna gidin. Bu kaynak grubu dağıtımı sırasında çözüm için belirttiğiniz aynı ada sahip.

2. {Dağıtım adı} için gidin-kuralları Stream Analytics işi. 

3. Tıklayın **Durdur** çalışmasını Stream Analytics işi durdurulamadı. (Sorgu düzenlemeden önce Durdur akış işinin tamamlanmasını beklemeniz gerekir).

4. Tıklayın **sorgu**. Eklemek için bu sorguyu düzenleme **seçin** bildirimi **ExternalTemperature**. Aşağıdaki örnek, tam bir sorgu yeni gösterir **seçin** deyimi:

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

5. Tıklayın **Kaydet** güncelleştirilmiş kural sorguyu değiştirmek için.

6. Tıklayın **Başlat** yeniden çalıştırmayı Stream Analytics işi başlatılamadı.

## <a name="add-your-new-rule-in-the-dashboard"></a>Panoda, yeni bir kural ekleyin

Artık ekleyebilirsiniz **ExternalTemperature** çözüm panosundaki bir cihaza kuralı.

1. Çözüm Portalı'na gidin.

2. Gidin **cihazları** paneli.

3. Gönderen, oluşturduğunuz özel cihazı Bul **ExternalTemperature** telemetri ve **cihaz ayrıntıları** panelinde, tıklayın **Kuralı Ekle**.

4. Seçin **ExternalTemperature** içinde **veri alanı**.

5. Ayarlama **eşiği** 56 için. Ardından **kaydetmek ve görüntülemek kuralları**.

6. Uyarı geçmişini görüntülemek için panoya geri dönün.

7. Konsol penceresinde, açık bıraktığınız göndermeye başlamak için bir Node.js konsol uygulaması başlangıç **ExternalTemperature** telemetri verileri.

8. Dikkat **Alarm geçmişi** tablo yeni uyarılar yeni kuralı tetiklendiğinde gösterir.
 
## <a name="additional-information"></a>Ek bilgiler

İşleç değiştirme **>** daha karmaşıktır ve Bu öğreticide özetlenen adımları dışına gider. İstediğiniz ne olursa olsun işlecini kullanmak için Stream Analytics işi değiştirebilirsiniz, ancak çözüm portalındaki işleç yansıtan daha karmaşık bir görevdir. 

## <a name="next-steps"></a>Sonraki adımlar
Özel kurallar oluşturmak nasıl gördüğünüze göre önceden yapılandırılmış çözümler hakkında daha fazla bilgi edinebilirsiniz:

- [Mantıksal uygulama, Azure IOT paketi uzaktan önceden yapılandırılmış izleme çözümüne bağlama][lnk-logic-app]
- [Cihaz bilgi meta verilerini Uzaktan izleme çözümüne][lnk-devinfo].

[lnk-devinfo]: iot-suite-v1-remote-monitoring-device-info.md

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-node]: http://nodejs.org
[lnk-builtin-rule]: iot-suite-v1-getstarted-preconfigured-solutions.md#view-alarms
[lnk-dynamic-telemetry]: iot-suite-v1-dynamic-telemetry.md
[lnk-logic-app]: iot-suite-v1-logic-apps-tutorial.md