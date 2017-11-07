---
title: "Önceden yapılandırılmış çözümleri özelleştirme | Microsoft Docs"
description: "Azure IOT paketi önceden yapılandırılmış çözümleri Özelleştirme Kılavuzu verilmektedir."
services: 
suite: iot-suite
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 4653ae53-4110-4a10-bd6c-7dc034c293a8
ms.service: iot-suite
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: corywink
ms.openlocfilehash: 52645f7d7934c9b9cf628fec1c0edc763ce98796
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2017
---
# <a name="customize-a-preconfigured-solution"></a>Önceden yapılandırılmış bir çözümü özelleştirme

Azure IOT paketi ile sağlanan önceden yapılandırılmış çözümler bir uçtan uca çözümü sunmak için birlikte çalışan suite içinde hizmetlerini gösterme. Bu başlangıç noktasından genişletmek ve belirli senaryolar için çözümü özelleştirme çeşitli yerlerde vardır. Aşağıdaki bölümlerde bu ortak özelleştirme noktalarını açıklanmaktadır.

## <a name="find-the-source-code"></a>Kaynak kodu bulun

Önceden yapılandırılmış çözümler için kaynak kodunu şu depoları Github'da kullanılabilir:

* Uzaktan izleme: [https://www.github.com/Azure/azure-iot-remote-monitoring](https://github.com/Azure/azure-iot-remote-monitoring)
* Tahmine dayalı Bakım: [https://github.com/Azure/azure-iot-predictive-maintenance](https://github.com/Azure/azure-iot-predictive-maintenance)
* Bağlı Fabrika: [https://github.com/Azure/azure-iot-connected-factory](https://github.com/Azure/azure-iot-connected-factory)

Önceden yapılandırılmış çözümler için kaynak kodunu desenleri ve Azure IOT paketi kullanarak bir IOT çözüm uçtan uca işlevselliğini uygulamak için kullanılan yöntemler göstermek için sağlanmıştır. Derleme ve GitHub depolarının çözümlerinde dağıtma hakkında daha fazla bilgi bulabilirsiniz.

## <a name="change-the-preconfigured-rules"></a>Önceden yapılandırılmış kurallarını değiştirin

Uzaktan izleme çözümü üç içeren [Azure akış analizi](https://azure.microsoft.com/services/stream-analytics/) cihaz bilgilerini, telemetri ve çözümdeki kuralları mantığı işlemek için işler.

Üç akış analizi işleri ve bunların söz dizimi ayrıntılı olarak açıklanmıştır [Uzaktan izleme çözümünde gezinme önceden yapılandırılmış](iot-suite-v1-remote-monitoring-sample-walkthrough.md). 

Bu işleri doğrudan mantığı alter düzenleyin veya senaryonuz için belirli mantığı ekleyin. Akış analizi işleri şu şekilde bulabilirsiniz:

1. Git [Azure portal](https://portal.azure.com).
2. IOT çözümünüzü aynı ada sahip kaynak grubuna gidin. 
3. Değiştirmek istediğiniz Azure akış analizi işi seçin. 
4. Seçerek işini durdurma **durdurmak** , komutları kümesi. 
5. Giriş, sorgu ve çıkış düzenleyin.
   
    Basit bir değişiklikle için sorgu değiştirmektir **kuralları** kullanmak için iş bir **"<"** yerine bir **">"**. Çözüm portalı görüntülenmeye **">"** kullandığınızda, kural düzenleme, ancak temel alınan iş değişikliği nedeniyle davranışı nasıl çevrilmiş dikkat edin.
6. İşi Başlat

> [!NOTE]
> İşlerini değiştirilmesine Pano başarısız olmasına neden olabilir şekilde Uzaktan izleme Panosu belirli verilere, bağlıdır.

## <a name="add-your-own-rules"></a>Kendi kurallarınızı ekleme

Önceden yapılandırılmış Azure akış analizi işi değiştirmenin yanı sıra, yeni işleri ekleyin veya yeni sorgular için var olan işler eklemek için Azure portalını kullanabilirsiniz.

## <a name="customize-devices"></a>Aygıtları özelleştirme

En yaygın uzantısı etkinlikler senaryonuz için belirli cihazlar ile çalışmaktadır. Cihazları ile çalışmak için birkaç yöntem vardır. Senaryonuz eşleşecek şekilde bir sanal cihaz değiştirilmesine veya kullanarak bu yöntemleri dahil [IOT cihaz SDK'sı] [ IoT Device SDK] çözüme fiziksel Cihazınızı bağlamak için.

Cihaz ekleme için adım adım yönergeler için bkz: [IOT paketi bağlanan cihazları](iot-suite-v1-connecting-devices.md) makale ve [C SDK'sı örneği izleme uzaktan](https://github.com/Azure/azure-iot-sdk-c/tree/master/serializer/samples/remote_monitoring). Bu örnek, Uzaktan izleme önceden yapılandırılmış çözümü ile çalışmak üzere tasarlanmıştır.

### <a name="create-your-own-simulated-device"></a>Sanal cihazınız oluşturma

Dahil [Uzaktan izleme çözümünün kaynak kodu](https://github.com/Azure/azure-iot-remote-monitoring), .NET simulator değil. Bu simulator çözümün bir parçası sağlanan olur ve farklı meta veriler, telemetri, Gönder ve farklı komutları ve yöntemleri yanıt şekilde değiştirebilirsiniz.

Önceden yapılandırılmış Uzaktan izleme çözümü önceden yapılandırılmış benzeticisinde sıcaklık ve nem telemetri yayan için bir cihazın benzetimini yapar. Benzetici değiştirebileceğiniz [Simulator.WebJob](https://github.com/Azure/azure-iot-remote-monitoring/tree/master/Simulator/Simulator.WebJob) proje GitHub deposuna çatallanmış zaman.

### <a name="available-locations-for-simulated-devices"></a>Sanal cihazlar için kullanılabilir konumları

Varsayılan konumlar kümesidir Seattle/Redmond, Washington, Amerika Birleşik Devletleri. Bu konumları değiştirebilirsiniz [SampleDeviceFactory.cs][lnk-sample-device-factory].

### <a name="add-a-desired-property-update-handler-to-the-simulator"></a>İstenen özellik güncelleştirme işleyicisi simulator ekleyin

Çözüm Portalı'nda bir aygıt için istenen bir özellik için bir değer ayarlayabilirsiniz. Bu özelliği işlemek için cihaz sorumluluğunda değiştirme isteği cihaz istenen özellik değeri aldığında olur. Özellik değeri değişikliği istenen özelliği aracılığıyla desteği eklemek için bir işleyici simulator eklemeniz gerekir.

Simulator işleyicilerini içeren **SetPointTemp** ve **TelemetryInterval** ayarlayarak güncelleştirebilirsiniz özellikleri istenen çözüm portalı değerleri.

Aşağıdaki örnek, işleyicisi gösterir **SetPointTemp** özelliğinde istenen **CoolerDevice** sınıfı:

```csharp
protected async Task OnSetPointTempUpdate(object value)
{
    var telemetry = _telemetryController as ITelemetryWithSetPointTemperature;
    telemetry.SetPointTemperature = Convert.ToDouble(value);

    await SetReportedPropertyAsync(SetPointTempPropertyName, telemetry.SetPointTemperature);
}
```

Bu yöntem telemetri noktası sıcaklık güncelleştirir ve değişikliği geri IOT Hub'ına bildirilen özelliği ayarlanarak bildirir.

Önceki örnekte düzeni izleyerek kendi özelliklerinizi için kendi işleyicilerinizi ekleyebilirsiniz.

Ayrıca istenen özelliği işleyicisine aşağıdaki örnekte gösterildiği gibi bağlamanız gerekir **CoolerDevice** Oluşturucusu:

```csharp
_desiredPropertyUpdateHandlers.Add(SetPointTempPropertyName, OnSetPointTempUpdate);
```

Unutmayın **SetPointTempPropertyName** olan "Config.SetPointTemp" tanımlanan bir sabit.

### <a name="add-support-for-a-new-method-to-the-simulator"></a>Yeni bir yöntem için destek simulator Ekle

Yeni bir desteği eklemek için simulator özelleştirebilirsiniz [yöntemi (doğrudan yöntemi)][lnk-direct-methods]. Gereken iki temel adımlar verilmiştir:

- Benzetici, IOT hub yönteminin ayrıntılarla önceden yapılandırılmış çözümde bildirmeniz gerekir.
- Simulator çağırdığınızda, ondan yöntem çağrısının işlemek için kod içermelidir **cihaz ayrıntıları** Masası Çözüm Gezgini'nde ya da bir iş.

Çözüm kullanan önceden yapılandırılmış Uzaktan izleme *özellikleri bildirilen* IOT hub'ına desteklenen yöntemlerden ayrıntılarını göndermek için. Çözüm arka ucu yöntem çağrılarına geçmişini yanı sıra her bir cihaz tarafından desteklenen tüm yöntemleri listesini tutar. Bu cihazlar hakkındaki bilgileri görüntülemek ve çözüm portalında yöntemleri çağırma.

Bir aygıt bir yöntem destekler, cihaz yönteme ayrıntılarını eklemeniz gerekir IOT hub'ı bildirmek için **SupportedMethods** bildirilen özellikleri düğümünde:

```json
"SupportedMethods": {
  "<method signature>": "<method description>",
  "<method signature>": "<method description>"
}
```

Yöntem imzası aşağıdaki biçime sahiptir: `<method name>--<parameter #0 name>-<parameter #1 type>-...-<parameter #n name>-<parameter #n type>`. Örneğin, belirtmek için **InitiateFirmwareUpdate** yöntemi bekliyor adlı bir dize parametresi **FwPackageURI**, aşağıdaki yöntemi imzası kullanın:

```json
InitiateFirmwareUpate--FwPackageURI-string: "description of method"
```

Desteklenen parametre türleri listesi için bkz: **CommandTypes** altyapısı projesi sınıfta.

Bir yöntem silmek için yöntem imzası ayarlamak `null` bildirilen özellikleri.

> [!NOTE]
> Çözüm arka ucu, aldığında, yalnızca desteklenen yöntemleri hakkında bilgi güncelleştirmeleri bir *aygıt bilgileri* aygıttan ileti.

Aşağıdaki kod örnek alarak **SampleDeviceFactory** ortak proje sınıfında bir yöntem listesine eklemek nasıl gösterir **SupportedMethods** aygıt tarafından gönderilen bildirilen özellikleri:

```csharp
device.Commands.Add(new Command(
    "InitiateFirmwareUpdate",
    DeliveryType.Method,
    "Updates device Firmware. Use parameter 'FwPackageUri' to specifiy the URI of the firmware file, e.g. https://iotrmassets.blob.core.windows.net/firmwares/FW20.bin",
    new[] { new Parameter("FwPackageUri", "string") }
));
```

Bu kod parçacığını ayrıntılarını ekler **InitiateFirmwareUpdate** çözüm portalı ve gerekli yöntem parametreleri ayrıntılarını görüntülemek için metin dahil yöntemi.

Simulator bildirilen özellikleri, IOT hub'ına simulator başladığında desteklenen yöntemlerin listesi dahil olmak üzere gönderir.

Simulator koduna desteklediği her bir yöntemi için bir işleyici ekleyin. Varolan işleyiciler görebilirsiniz **CoolerDevice** Simulator.WebJob projesinde sınıfı. Aşağıdaki örnek, işleyicisi gösterir **InitiateFirmwareUpdate** yöntemi:

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

Yöntem işleyici adları ile başlamalı `On` ardından yöntemin adı. **MethodRequest** parametresi çözüm arka ucu ile yöntemi çağırma geçirilen herhangi bir parametre içeriyor. Dönüş değeri türü olmalıdır **görev&lt;MethodResponse&gt;**. **BuildMethodResponse** yardımcı program yöntemi, dönüş değeri oluşturmanıza yardımcı olur.

Yöntem işleyicisinin içinden, olabilir:

- Zaman uyumsuz bir görevi başlatın.
- İstenen özelliklerinden almak *cihaz çifti* IOT hub.
- Bir tek bildirilen özelliğini kullanarak güncelleştirme **SetReportedPropertyAsync** yönteminde **CoolerDevice** sınıfı.
- Birden çok bildirilen özellikleri oluşturarak güncelleştirmek bir **TwinCollection** örneği ve arama **Transport.UpdateReportedPropertiesAsync** yöntemi.

Bellenim güncelleştirme sabitlerini aşağıdaki adımları gerçekleştirir:

- Cihazın üretici yazılımı güncelleştirme isteğini kabul edemiyor denetler.
- Zaman uyumsuz olarak bellenim güncelleştirme işlemi başlatır ve işlem tamamlandığında, telemetri sıfırlar.
- Hemen istek aygıt tarafından kabul belirtmek için "FirmwareUpdate kabul" iletisi döndürür.

### <a name="build-and-use-your-own-physical-device"></a>Derleme ve (fiziksel) aygıtınızı kullanın

[Azure IOT SDK'ları](https://github.com/Azure/azure-iot-sdks) kitaplıkları çok sayıda aygıt türleri (diller ve işletim sistemleri) bağlamak için IOT çözümleriyle sağlar.

## <a name="modify-dashboard-limits"></a>Pano sınırları değiştirme

### <a name="number-of-devices-displayed-in-dashboard-dropdown"></a>Pano açılır listede görüntülenen aygıt sayısı

200 varsayılandır. Bu sayıyı değiştirin [DashboardController.cs][lnk-dashboard-controller].

### <a name="number-of-pins-to-display-in-bing-map-control"></a>Sayısını Bing harita denetiminde görüntüleme

200 varsayılandır. Bu sayıyı değiştirin [TelemetryApiController.cs][lnk-telemetry-api-controller-01].

### <a name="time-period-of-telemetry-graph"></a>Süre telemetri grafik

Varsayılan değer 10 dakikadır. Bu değeri değiştirebilirsiniz [TelmetryApiController.cs][lnk-telemetry-api-controller-02].

## <a name="manually-set-up-application-roles"></a>Uygulama rolleri el ile ayarlamanız

Aşağıdaki yordam nasıl ekleneceğini açıklar **yönetici** ve **ReadOnly** uygulama rolleri önceden yapılandırılmış bir çözüm için. Azureiotsuite.com sitesinden zaten sağlanan önceden yapılandırılmış çözümler içerdiğini unutmayın **yönetici** ve **ReadOnly** rolleri.

Üyeleri **ReadOnly** rolü Pano ve cihaz listesini görebilirsiniz ancak cihazları eklemek, cihaz özniteliklerine değiştirmek veya komutlarını göndermek için izin verilmiyor.  Üyeleri **yönetici** rol Çözümdeki tüm işlevlere tam erişimi vardır.

1. Git [Klasik Azure portalı][lnk-classic-portal].
2. **Active Directory**’yi seçin.
3. Çözümünüzü sağlarken kullandığınız AAD Kiracı adına tıklayın.
4. **Uygulamalar**'a tıklayın.
5. Önceden yapılandırılmış çözümünüzün adıyla eşleşen uygulamanın adına tıklayın. Uygulamanızı listede görmüyorsanız, seçin **Şirketimin sahip olduğu uygulamalar** içinde **Göster** onay işareti açılır ve'ı tıklatın.
6. Sayfanın alt kısmındaki tıklatın **yönetmek bildirim** ve ardından **karşıdan bildirim**.
7. Bu yordam bir .json dosyası yerel makinenize indirir. Tercih ettiğiniz bir metin düzenleyicisinde düzenlemek için bu dosyayı açın.
8. .Json dosyası üçüncü satırında, görebilirsiniz:

   ```json
   "appRoles" : [],
   ```
   Bu satırı aşağıdaki kodla değiştirin:

   ```json
   "appRoles": [
   {
   "allowedMemberTypes": [
   "User"
   ],
   "description": "Administrator access to the application",
   "displayName": "Admin",
   "id": "a400a00b-f67c-42b7-ba9a-f73d8c67e433",
   "isEnabled": true,
   "value": "Admin"
   },
   {
   "allowedMemberTypes": [
   "User"
   ],
   "description": "Read only access to device information",
   "displayName": "Read Only",
   "id": "e5bbd0f5-128e-4362-9dd1-8f253c6082d7",
   "isEnabled": true,
   "value": "ReadOnly"
   } ],
   ```

9. (Mevcut dosyanın üzerine yazabilirsiniz) güncelleştirilmiş .json dosyasını kaydedin.
10. Klasik Azure portalı, sayfanın sonundaki seçin **yönetmek bildirim** sonra **karşıya bildirim** önceki adımda kaydettiğiniz .json dosyası karşıya yüklemek için.
11. Artık eklemiş olduğunuz **yönetici** ve **ReadOnly** uygulamanız için rolleri.
12. Bu rollerden birinin dizininizdeki bir kullanıcı atamak için bkz: [azureiotsuite.com sitesindeki izinler][lnk-permissions].

## <a name="feedback"></a>Geri Bildirim

Bir özelleştirme zorunda görmek bu belgedeki kapsanan istiyorsunuz? Özellik önerileri eklemek [kullanıcı sesi](https://feedback.azure.com/forums/321918-azure-iot), ya da bu makaleye yorum. 

## <a name="next-steps"></a>Sonraki adımlar

Önceden yapılandırılmış çözümleri özelleştirme seçenekleri hakkında daha fazla bilgi için bkz:

* [Mantıksal uygulama, Azure IOT paketi uzaktan önceden yapılandırılmış izleme çözümüne bağlama][lnk-logicapp]
* [Önceden yapılandırılmış Uzaktan izleme çözümü ile dinamik telemetri kullanın][lnk-dynamic]
* [Önceden yapılandırılmış Uzaktan izleme çözümü cihaz bilgileri meta veriler][lnk-devinfo]
* [Bağlı Fabrika çözüm OPC UA sunucularınızdan veri biçimini Özelleştir][lnk-cf-customize]

[lnk-logicapp]: iot-suite-v1-logic-apps-tutorial.md
[lnk-dynamic]: iot-suite-v1-dynamic-telemetry.md
[lnk-devinfo]: iot-suite-v1-remote-monitoring-device-info.md

[IoT Device SDK]: https://azure.microsoft.com/documentation/articles/iot-hub-sdks-summary/
[lnk-permissions]: iot-suite-v1-permissions.md
[lnk-dashboard-controller]: https://github.com/Azure/azure-iot-remote-monitoring/blob/3fd43b8a9f7e0f2774d73f3569439063705cebe4/DeviceAdministration/Web/Controllers/DashboardController.cs#L27
[lnk-telemetry-api-controller-01]: https://github.com/Azure/azure-iot-remote-monitoring/blob/3fd43b8a9f7e0f2774d73f3569439063705cebe4/DeviceAdministration/Web/WebApiControllers/TelemetryApiController.cs#L27
[lnk-telemetry-api-controller-02]: https://github.com/Azure/azure-iot-remote-monitoring/blob/e7003339f73e21d3930f71ceba1e74fb5c0d9ea0/DeviceAdministration/Web/WebApiControllers/TelemetryApiController.cs#L25 
[lnk-sample-device-factory]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Common/Factory/SampleDeviceFactory.cs#L40
[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-direct-methods]: ../iot-hub/iot-hub-devguide-direct-methods.md
[lnk-cf-customize]: iot-suite-connected-factory-customize.md