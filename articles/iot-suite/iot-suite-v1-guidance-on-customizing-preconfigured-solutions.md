---
title: Önceden yapılandırılmış çözümleri özelleştirme | Microsoft Docs
description: Azure IOT paketi önceden yapılandırılmış çözümleri özelleştirme konusunda rehberlik sağlar.
services: ''
suite: iot-suite
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: 4653ae53-4110-4a10-bd6c-7dc034c293a8
ms.service: iot-suite
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: corywink
ms.openlocfilehash: cb5955111cb3954f71f11602042b5153ccee3473
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47107049"
---
# <a name="customize-a-preconfigured-solution"></a>Önceden yapılandırılmış bir çözümü özelleştirme

Azure IOT paketi ile sağlanan önceden yapılandırılmış çözümler, uçtan uca çözüm sunmak için birlikte çalışan grubu içindeki hizmetler gösterilmektedir. Bu başlangıç noktasından genişletebilir ve belirli senaryolar için çözümü özelleştirmek için çeşitli yerlerde vardır. Aşağıdaki bölümlerde, bu ortak özelleştirme noktaları açıklanmaktadır.

## <a name="find-the-source-code"></a>Kaynak kodu bulma

Önceden yapılandırılmış çözümler için kaynak kodu, aşağıdaki depoları github'da kullanılabilir:

* Uzaktan izleme: [https://www.github.com/Azure/azure-iot-remote-monitoring](https://github.com/Azure/azure-iot-remote-monitoring)
* Tahmine dayalı Bakım: [https://github.com/Azure/azure-iot-predictive-maintenance](https://github.com/Azure/azure-iot-predictive-maintenance)
* Bağlı Fabrika: [https://github.com/Azure/azure-iot-connected-factory](https://github.com/Azure/azure-iot-connected-factory)

Önceden yapılandırılmış çözümler için kaynak kodu, desenler ve uygulamalar Azure IOT paketi kullanarak bir IOT çözümü uçtan uca işlevselliğini uygulamak için kullanılan göstermek için sağlanmıştır. GitHub depoları çözümleri oluşturma ve dağıtma hakkında daha fazla bilgi bulabilirsiniz.

## <a name="change-the-preconfigured-rules"></a>Önceden yapılandırılmış bir kural değiştirme

Uzaktan izleme çözümü üç içerir [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) cihaz bilgilerini, telemetri ve çözümünde kural mantığı işlemek için işler.

Üç stream analytics işleri ve kendi sözdizimi, ayrıntılı olarak açıklanmıştır [Uzaktan izleme önceden yapılandırılmış çözüm Kılavuzu](iot-suite-v1-remote-monitoring-sample-walkthrough.md). 

Doğrudan mantıksal değiştirmek için bu işleri düzenleyin ya da kendi senaryonuza özgü mantığı ekleyin. Stream Analytics işleri aşağıda bulabilirsiniz:

1. Git [Azure portalında](https://portal.azure.com).
2. IOT çözümünüzü aynı ada sahip bir kaynak grubuna gidin. 
3. Değiştirmek istediğiniz Azure Stream Analytics işi seçin. 
4. Seçerek işini durdurma **Durdur** komutları kümesi. 
5. Giriş, sorgu ve çıkışları düzenleyin.
   
    Sorgu için basit bir değişiklikle değiştirmektir **kuralları** kullanmak için iş bir **"<"** yerine bir **">"**. Çözüm portalı görüntülenmeye **">"** ne zaman, bir kural düzenleyemezsiniz, ancak temel alınan iş değişiklik nedeniyle davranışını nasıl çevrilmiş dikkat edin.
6. İşi başlatma

> [!NOTE]
> Uzaktan izleme Panosu, işleri değiştirme Pano başarısız olmasına neden olabilir, böylece belirli veri bağlıdır.

## <a name="add-your-own-rules"></a>Kendi kurallarınızı Ekle

Önceden yapılandırılmış Azure Stream Analytics işleri değiştirmenin yanı sıra, yeni işleri ekleyin veya mevcut işleri için yeni bir sorgu eklemek için Azure portalını kullanabilirsiniz.

## <a name="customize-devices"></a>Cihazları özelleştirme

En yaygın genişletme etkinliklerden birini senaryonuz için belirli cihazlar ile çalışmaktadır. Cihazlar ile çalışmak için birkaç yöntem vardır. Bir sanal cihaz senaryonuzla eşleşecek şekilde değiştirmeyi veya kullanarak bu yöntemleri dahil [IOT cihaz SDK'sı] [ IoT Device SDK] çözüme fiziksel Cihazınızı bağlamak için.

Cihaz ekleme için adım adım yönergeler için bkz. [IOT paketinin bağlanan cihazları](iot-suite-v1-connecting-devices.md) makale ve [uzaktan C SDK'sı örneği izleme](https://github.com/Azure/azure-iot-sdk-c/tree/master/serializer/samples/remote_monitoring). Bu örnek, Uzaktan izleme önceden yapılandırılmış çözümü ile çalışacak şekilde tasarlanmıştır.

### <a name="create-your-own-simulated-device"></a>Kendi sanal cihaz oluşturma

Dahil [Uzaktan izleme çözümünün kaynak kodu](https://github.com/Azure/azure-iot-remote-monitoring), .NET simülatör olduğu. Bu simülatörü çözümün bir parçası sağlanan olur ve farklı meta verileri, telemetri, göndermek ve farklı komutlar ve yöntemlerle yanıt vermek için değiştirebilirsiniz.

Önceden yapılandırılmış Uzaktan izleme önceden yapılandırılmış çözümü simülatörde sıcaklık ve nem telemetrisi yayan harika bir cihazın benzetimini yapar. Simülatörde değiştirebileceğiniz [Simulator.WebJob](https://github.com/Azure/azure-iot-remote-monitoring/tree/master/Simulator/Simulator.WebJob) proje GitHub deponun çatalını zaman.

### <a name="available-locations-for-simulated-devices"></a>Sanal cihazlar için kullanılabilir konumları

Varsayılan konumlar kümesidir Seattle/Redmond, Washington, Amerika Birleşik Devletleri. Bu konumlarda değiştirebilirsiniz [SampleDeviceFactory.cs][lnk-sample-device-factory].

### <a name="add-a-desired-property-update-handler-to-the-simulator"></a>İstenen özellik güncelleştirme işleyicisi için simülatör'ü Ekle

Çözüm portalında istenen bir özellik için bir cihaz için bir değer ayarlayabilirsiniz. Bu cihaz, istenen özellik değerini aldığında sorumluluğu özelliği işlemek için cihazın değişiklik isteği olur. İstenen bir özelliği aracılığıyla bir özellik değeri değişiklik desteği eklemek için bir işleyici eklemek için simülatör gerekir.

Simülatör işleyicilerini içeren **SetPointTemp** ve **Telemetryınterval** değerleri çözüm portalında istenen ayarlayarak güncelleştirebileceğiniz özellikleri.

Aşağıdaki örnek, işleyici için gösterir **SetPointTemp** özelliğinde istenen **CoolerDevice** sınıfı:

```csharp
protected async Task OnSetPointTempUpdate(object value)
{
    var telemetry = _telemetryController as ITelemetryWithSetPointTemperature;
    telemetry.SetPointTemperature = Convert.ToDouble(value);

    await SetReportedPropertyAsync(SetPointTempPropertyName, telemetry.SetPointTemperature);
}
```

Bu yöntem telemetri noktası sıcaklık güncelleştirir ve değişikliği geri IOT Hub'ına bildirilen bir özelliği ayarlayarak bildirir.

Önceki örnekte düzenini izleyerek kendi özellikleri için kendi işleyicilerinizi ekleyebilirsiniz.

Ayrıca istenen özellik işleyiciye aşağıdaki örnekte gösterildiği gibi bağlamalısınız **CoolerDevice** Oluşturucusu:

```csharp
_desiredPropertyUpdateHandlers.Add(SetPointTempPropertyName, OnSetPointTempUpdate);
```

Unutmayın **SetPointTempPropertyName** olan "Config.SetPointTemp" tanımlanmış bir sabit.

### <a name="add-support-for-a-new-method-to-the-simulator"></a>Simülatör için yeni bir yöntem için destek eklendi

Yeni bir desteği eklemek için simülatör özelleştirebilirsiniz [yöntemi (doğrudan yöntem)][lnk-direct-methods]. Gereken iki temel adım vardır:

- Simülatör yönteminin ayrıntıları ile önceden yapılandırılmış çözümde IOT hub'ı bildirmeniz gerekir.
- Simülatör ondan çağırdığınızda, yöntem çağrısının işlemek için kod içermelidir **cihaz ayrıntıları** paneli Çözüm Gezgini'nde ya da bir proje.

Uzaktan izleme önceden yapılandırılmış çözümün kullandığı *bildirilen özellikler* desteklenen yöntemlerden ayrıntılarını IOT hub'ına göndermek için. Çözüm arka ucu, yöntem çağrılarını geçmişini yanı sıra her bir cihaz tarafından desteklenen tüm yöntemlerin listesini tutar. Bu cihaz bilgilerini görüntüleyebilir ve çözüm portalındaki yöntemleri çağırır.

Bir cihaz bir yöntem destekler, cihaz ayrıntıları yöntemi eklemeniz gerekir IOT hub'ı bildirmek için **SupportedMethods** bildirilen özellikler düğümünde:

```json
"SupportedMethods": {
  "<method signature>": "<method description>",
  "<method signature>": "<method description>"
}
```

Yöntem imzası aşağıdaki biçime sahiptir: `<method name>--<parameter #0 name>-<parameter #1 type>-...-<parameter #n name>-<parameter #n type>`. Örneğin, belirtmek için **Initiatefirmwareupdate** yöntemi adlı bir dize parametresi bekliyor **Fwpackageurı**, aşağıdaki yöntem imzasını kullanın:

```json
InitiateFirmwareUpate--FwPackageURI-string: "description of method"
```

Desteklenen parametrelerin bir listesi için bkz. **CommandTypes** altyapı projesinde sınıfı.

Yöntem imzası bir yöntemi silmek için kümesine `null` bildirilen özellikleri.

> [!NOTE]
> Çözüm arka ucu aldığında, yalnızca desteklenen yöntemleri hakkında bilgi güncelleştirmeleri bir *cihaz bilgilerini* CİHAZDAN ileti.

Aşağıdaki kod örneğini **SampleDeviceFactory** ortak proje sınıfta bir yöntem listesine eklemek nasıl gösterir **SupportedMethods** cihaz tarafından gönderilen bildirilen özellikleri:

```csharp
device.Commands.Add(new Command(
    "InitiateFirmwareUpdate",
    DeliveryType.Method,
    "Updates device Firmware. Use parameter 'FwPackageUri' to specifiy the URI of the firmware file, e.g. https://iotrmassets.blob.core.windows.net/firmwares/FW20.bin",
    new[] { new Parameter("FwPackageUri", "string") }
));
```

Bu kod parçacığı ayrıntılarını ekler **Initiatefirmwareupdate** çözüm portalı ve gereken yöntemini parametrelerin ayrıntılarını görüntülemek üzere metin dahil olmak üzere yöntemi.

Simülatör, IOT hub'ına simülatör başlatıldığında desteklenen yöntemlerin listesi dahil olmak üzere bildirilen özellikleri gönderir.

Her bir yöntemin desteklediği benzetici kodu için bir işleyici ekleyin. Mevcut işleyiciler gördüğünüz **CoolerDevice** Simulator.WebJob projedeki sınıf. Aşağıdaki örnek, işleyici için gösterir **Initiatefirmwareupdate** yöntemi:

```csharp
public async Task<MethodResponse> OnInitiateFirmwareUpdate(MethodRequest methodRequest, object userContext)
{
    if (_deviceManagementTask != null && !_deviceManagementTask.IsCompleted)
    {
        return await Task.FromResult(BuildMethodRespose(new
        {
            Message = "Device is busy"
        }, 409));
    }

    try
    {
        var operation = new FirmwareUpdate(methodRequest);
        _deviceManagementTask = operation.Run(Transport).ContinueWith(async task =>
        {
            // after firmware completed, we reset telemetry
            var telemetry = _telemetryController as ITelemetryWithTemperatureMeanValue;
            if (telemetry != null)
            {
                telemetry.TemperatureMeanValue = 34.5;
            }

            await UpdateReportedTemperatureMeanValue();
        });

        return await Task.FromResult(BuildMethodRespose(new
        {
            Message = "FirmwareUpdate accepted",
            Uri = operation.Uri
        }));
    }
    catch (Exception ex)
    {
        return await Task.FromResult(BuildMethodRespose(new
        {
            Message = ex.Message
        }, 400));
    }
}
```

Yöntem işleyici adları ile başlamalıdır `On` ardından yöntemin adı. **MethodRequest** parametre içeren çözüm arka ucu ile yöntemi çağırmayı geçirilen herhangi bir parametre. Döndürülen değer türünde olmalıdır **görev&lt;MethodResponse&gt;**. **BuildMethodResponse** yardımcı yöntemi, dönüş değeri oluşturmanıza yardımcı olur.

Yöntemin işleyicisi, şunları yapabilirsiniz:

- Zaman uyumsuz bir görev başlatın.
- İstenen özellikleri almak *cihaz ikizi* IOT hub'ında.
- Tek bildirilen özellik kullanarak bir güncelleştirme **SetReportedPropertyAsync** yönteminde **CoolerDevice** sınıfı.
- Oluşturarak birden çok rapor edilen özellikleri güncelleştirmek bir **TwinCollection** örneği ve arama **Transport.UpdateReportedPropertiesAsync** yöntemi.

Üretici yazılımı güncelleştirme sabitlerini, aşağıdaki adımları gerçekleştirir:

- Cihazın üretici yazılımı güncelleştirme isteğini kabul etmek üzere denetler.
- Zaman uyumsuz olarak üretici yazılımı güncelleştirme işlemi başlatır ve işlem tamamlandığında telemetri sıfırlar.
- Hemen cihaz tarafından istek kabul edildi belirtmek için "Kabul FirmwareUpdate" iletisi döndürür.

### <a name="build-and-use-your-own-physical-device"></a>Derleme ve (fiziksel) Cihazınızı kullanma

[Azure IOT SDK'ları](https://github.com/Azure/azure-iot-sdks) IOT çözümleriyle çok sayıda cihaz türleri (diller ve işletim sistemleri) bağlamak için kitaplıkları sağlar.

## <a name="modify-dashboard-limits"></a>Pano sınırlarını değiştirme

### <a name="number-of-devices-displayed-in-dashboard-dropdown"></a>Pano açılan menüsünde görüntülenen cihaz sayısı

Varsayılan 200'dür. Bu numarayı değiştirebilirsiniz [DashboardController.cs][lnk-dashboard-controller].

### <a name="number-of-pins-to-display-in-bing-map-control"></a>Sayısını Bing harita denetimi görüntüleme

Varsayılan 200'dür. Bu numarayı değiştirebilirsiniz [TelemetryApiController.cs][lnk-telemetry-api-controller-01].

### <a name="time-period-of-telemetry-graph"></a>Telemetri grafiğin zaman aralığı

Varsayılan değer 10 dakikadır. Bu değeri değiştirebilirsiniz [TelmetryApiController.cs][lnk-telemetry-api-controller-02].

## <a name="feedback"></a>Geri Bildirim

Bir özelleştirme sahip değilsiniz, görmek bu belgedeki kapsanan istersiniz? Özellik önerileri ekleme [User Voice](https://feedback.azure.com/forums/321918-azure-iot), ya da bu makalede yorum. 

## <a name="next-steps"></a>Sonraki adımlar

Önceden yapılandırılmış çözümleri özelleştirme seçenekleri hakkında daha fazla bilgi için bkz:

* [Mantıksal uygulama, Azure IOT paketi uzaktan önceden yapılandırılmış izleme çözümüne bağlama][lnk-logicapp]
* [Uzaktan izleme önceden yapılandırılmış çözümüyle dinamik telemetri kullanma][lnk-dynamic]
* [Cihaz önceden yapılandırılmış Uzaktan izleme çözümünü bilgi meta veriler][lnk-devinfo]
* [Bağlı Fabrika çözümü OPC UA sunucularınızdan verilerin biçimini özelleştirme][lnk-cf-customize]

[lnk-logicapp]: iot-suite-v1-logic-apps-tutorial.md
[lnk-dynamic]: iot-suite-v1-dynamic-telemetry.md
[lnk-devinfo]: iot-suite-v1-remote-monitoring-device-info.md

[IoT Device SDK]: https://azure.microsoft.com/documentation/articles/iot-hub-sdks-summary/
[lnk-permissions]: iot-suite-v1-permissions.md
[lnk-dashboard-controller]: https://github.com/Azure/azure-iot-remote-monitoring/blob/3fd43b8a9f7e0f2774d73f3569439063705cebe4/DeviceAdministration/Web/Controllers/DashboardController.cs#L27
[lnk-telemetry-api-controller-01]: https://github.com/Azure/azure-iot-remote-monitoring/blob/3fd43b8a9f7e0f2774d73f3569439063705cebe4/DeviceAdministration/Web/WebApiControllers/TelemetryApiController.cs#L27
[lnk-telemetry-api-controller-02]: https://github.com/Azure/azure-iot-remote-monitoring/blob/e7003339f73e21d3930f71ceba1e74fb5c0d9ea0/DeviceAdministration/Web/WebApiControllers/TelemetryApiController.cs#L25 
[lnk-sample-device-factory]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Common/Factory/SampleDeviceFactory.cs#L40
[lnk-direct-methods]: ../iot-hub/iot-hub-devguide-direct-methods.md
[lnk-cf-customize]:../iot-accelerators/iot-accelerators-connected-factory-customize.md