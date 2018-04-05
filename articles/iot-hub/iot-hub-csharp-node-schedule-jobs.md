---
title: Zamanlama işlerini ile Azure IOT hub'ı (.NET/düğümü) | Microsoft Docs
description: Birden çok aygıta doğrudan bir yöntemi çağırmak için bir Azure IOT Hub işini zamanlamak nasıl. Sanal cihaz uygulamaları ve Azure IOT hizmeti işi çalıştırmak için bir hizmet uygulaması uygulamak .NET SDK'sını uygulamak için Azure IOT cihaz SDK'sı Node.js için kullanın.
services: iot-hub
documentationcenter: .net
author: juanjperez
manager: timlt
editor: ''
ms.assetid: 2233356e-b005-4765-ae41-3a4872bda943
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/10/2017
ms.author: juanpere
ms.openlocfilehash: 360daf918051ce901a81f96d1873dc90af238e19
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="schedule-and-broadcast-jobs-netnodejs"></a>Zamanlama ve yayın işleri (.NET/Node.js)

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

Zamanlama ve milyonlarca cihaza güncelleştirme işleri izlemek için Azure IOT hub'ı kullanın. İşlerini kullanın:

* İstenen özellikleri güncelleştirme
* Güncelleştirme etiketleri
* Doğrudan yöntemleri çağırma

Bir işi şu eylemlerden birini sarmalar ve yürütme cihaz çifti sorgu tarafından tanımlanan bir dizi aygıtları izler. Örneğin, bir arka uç uygulaması, aygıtların yeniden 10.000 cihazlarda doğrudan yöntemini çağırmak için bir iş kullanabilirsiniz. Cihaz kümesini içeren bir cihaz çifti sorgu belirtin ve gelecek bir zamanda çalıştırmak için iş zamanlama. Her aygıt olarak ilerleyişini izler almak ve yeniden başlatma doğrudan yöntemi yürütün.

Bu özelliklerin her biri hakkında daha fazla bilgi için bkz:

* Cihaz çifti ve özellikleri: [cihaz çiftlerini ile çalışmaya başlama] [ lnk-get-started-twin] ve [öğretici: cihaz çifti özellikleri kullanma][lnk-twin-props]
* Doğrudan yöntemleri: [IOT Hub Geliştirici Kılavuzu - doğrudan yöntemleri] [ lnk-dev-methods] ve [Öğreticisi: doğrudan yöntemleri kullanın][lnk-c2d-methods]

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Bu öğretici şunların nasıl yapıldığını gösterir:

* Adlı doğrudan bir yöntem uygulayan bir cihaz uygulaması oluşturma **lockDoor** arka uç uygulama tarafından çağrılabilir. Cihaz uygulaması Ayrıca, arka uç uygulama istenen özellik değişiklikleri alır.
* Aranacak bir işi oluşturan bir arka uç uygulaması oluşturma **lockDoor** birden fazla cihazda yöntemi doğrudan. Başka bir iş birden çok aygıta istenen özelliği güncelleştirmeleri gönderir.

Bu öğreticinin sonunda bir Node.js konsol cihaz uygulaması ve bir .NET (C#) konsol arka uç uygulaması sahiptir:

**simDevice.js** uygular, IOT hub'ına bağlanan **lockDoor** yöntemi ve tanıtıcıları istenen özellik değişikliklerini doğrudan.

**ScheduleJob** çağırmak için işleri kullanan **lockDoor** doğrudan yöntemi ve birden çok aygıta cihaz çifti istenen özelliklerini güncelleştirir.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Visual Studio 2015 veya Visual Studio 2017.
* Node.js 4.0.x sürümü veya sonraki bir sürüm. Makaleyi [geliştirme ortamınızı hazırlama] [ lnk-dev-setup] Node.js Bu öğretici için Windows veya Linux'ta nasıl yükleneceğini açıklar.
* Etkin bir Azure hesabı. Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="schedule-jobs-for-calling-a-direct-method-and-sending-device-twin-updates"></a>Doğrudan bir yöntem çağırma ve cihaz çifti güncelleştirmeleri göndermek için zamanlama işleri

Bu bölümde, işleri çağırmak için kullanır (C# kullanarak) bir .NET konsol uygulaması oluşturma **lockDoor** doğrudan yöntemi ve istenen özelliği güncelleştirmeleri birden fazla cihaza Gönder.

1. Visual Studio'da **Konsol Uygulaması** proje şablonunu kullanarak geçerli çözüme bir Visual C# Windows Klasik Masaüstü projesi ekleyin. Proje adı **ScheduleJob**.

    ![Yeni Visual C# Windows Klasik Masaüstü projesi][img-createapp]

1. Çözüm Gezgini'nde sağ **ScheduleJob** proje ve ardından **NuGet paketlerini Yönet...** .
1. **NuGet Paket Yöneticisi** penceresinde **Gözat**'ı seçin, **microsoft.azure.devices**'ı aratın, **Microsoft.Azure.Devices** paketini yüklemek için **Yükle**'yi seçin ve kullanım koşullarını kabul edin. Bu adım indirir, yükler ve bir başvuru ekler [Azure IOT hizmeti SDK'sını] [ lnk-nuget-service-sdk] NuGet paketi ve bağımlılıklarını.

    ![NuGet Paket Yöneticisi penceresi][img-servicenuget]
1. Aşağıdaki `using` deyimlerini **Program.cs** dosyasının üst kısmına ekleyin:
    
    ```csharp
    using Microsoft.Azure.Devices;
    using Microsoft.Azure.Devices.Shared;
    ```

1. Aşağıdakileri ekleyin `using` deyimi yoksa varsayılan deyimlerinde.

    ```csharp
    using System.Threading;
    using System.Threading.Tasks;
    ```

1. **Program** sınıfına aşağıdaki alanları ekleyin. Yer tutucusunu önceki bölümde oluşturduğunuz hub'ın IOT Hub bağlantı dizesiyle değiştirin.

    ```csharp
    static string connString = "{iot hub connection string}";
    static ServiceClient client;
    static JobClient jobClient;
    ```

1. **Program** sınıfına aşağıdaki yöntemi ekleyin:

    ```csharp
    public static async Task MonitorJob(string jobId)
    {
        JobResponse result;
        do
        {
            result = await jobClient.GetJobAsync(jobId);
            Console.WriteLine("Job Status : " + result.Status.ToString());
            Thread.Sleep(2000);
        } while ((result.Status != JobStatus.Completed) && (result.Status != JobStatus.Failed));
    }
    ```

1. **Program** sınıfına aşağıdaki yöntemi ekleyin:

    ```csharp
    public static async Task StartMethodJob(string jobId)
    {
        CloudToDeviceMethod directMethod = new CloudToDeviceMethod("lockDoor", TimeSpan.FromSeconds(5), TimeSpan.FromSeconds(5));

        JobResponse result = await jobClient.ScheduleDeviceMethodAsync(jobId,
            "deviceId='myDeviceId'",
            directMethod,
            DateTime.Now,
            (long)TimeSpan.FromMinutes(2).TotalSeconds);

        Console.WriteLine("Started Method Job");
    }
    ```

1. **Program** sınıfına aşağıdaki yöntemi ekleyin:

    ```csharp
    public static async Task StartTwinUpdateJob(string jobId)
    {
        var twin = new Twin();
        twin.Properties.Desired["Building"] = "43";
        twin.Properties.Desired["Floor"] = "3";
        twin.ETag = "*";

        JobResponse result = await jobClient.ScheduleTwinUpdateAsync(jobId,
            "deviceId='myDeviceId'",
            twin,
            DateTime.Now,
            (long)TimeSpan.FromMinutes(2).TotalSeconds);

        Console.WriteLine("Started Twin Update Job");
    }
    ```

1. Son olarak, **Main** yöntemine aşağıdaki satırları ekleyin:

    ```csharp
    jobClient = JobClient.CreateFromConnectionString(connString);

    string methodJobId = Guid.NewGuid().ToString();

    StartMethodJob(methodJobId);
    MonitorJob(methodJobId).Wait();
    Console.WriteLine("Press ENTER to run the next job.");
    Console.ReadLine();

    string twinUpdateJobId = Guid.NewGuid().ToString();

    StartTwinUpdateJob(twinUpdateJobId);
    MonitorJob(twinUpdateJobId).Wait();
    Console.WriteLine("Press ENTER to exit.");
    Console.ReadLine();
    ```

1. Çözüm Gezgini'nde açık **başlangıç projelerini Ayarla...**  ve emin olun **eylem** için **ScheduleJob** projedir **Başlat**. Çözümü derleyin.

## <a name="create-a-simulated-device-app"></a>Sanal cihaz uygulaması oluşturma

Bu bölümde, sanal cihaz yeniden başlatma tetikler ve cihaz çifti sorguları cihazları belirlemek üzere etkinleştirmek için bildirilen özelliklerini kullanır ve bunların en son ne zaman yeniden bulut tarafından adlı doğrudan bir yöntem yanıtlaması bir Node.js konsol uygulaması oluşturun.

1. Adlı yeni bir boş klasör oluşturun **simDevice**.  İçinde **simDevice** klasörü, komut isteminde aşağıdaki komutu kullanarak bir package.json dosyası oluşturun.  Tüm varsayılanları kabul edin:

    ```cmd/sh
    npm init
    ```

1. Komut isteminizde **simDevice** klasörü yüklemek için aşağıdaki komutu çalıştırın, **azure IOT cihaz** ve **azure-IOT-cihaz-mqtt** paketler:

    ```cmd/sh
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. Bir metin düzenleyicisi kullanarak yeni bir oluşturma **simDevice.js** dosyasını **simDevice** klasör.

1. Aşağıdaki 'İste' deyimleri başlangıcında eklemek **simDevice.js** dosyası:

    ```nodejs
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```

1. Bir **connectionString** değişkeni ekleyin ve bir **İstemci** örneği oluşturmak için bunu kullanın. Yer tutucuları kurulumunuzu için uygun değerlerle değiştirdiğinizden emin olun.

    ```nodejs
    var connectionString = 'HostName={youriothostname};DeviceId={yourdeviceid};SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

1. İşlemek için aşağıdaki işlevi ekleyin **lockDoor** yöntemi.

    ```nodejs
    var onLockDoor = function(request, response) {
   
        // Respond the cloud app for the direct method
        response.send(200, function(err) {
            if (!err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.');
            }
        });
   
        console.log('Locking Door!');
    };
    ```

1. İşleyicisi kaydetmek için aşağıdaki kodu ekleyin **lockDoor** yöntemi.

    ```nodejs
    client.open(function(err) {
        if (err) {
            console.error('Could not connect to IotHub client.');
        }  else {
            console.log('Client connected to IoT Hub.  Waiting for lockDoor direct method.');
            client.onDeviceMethod('lockDoor', onLockDoor);
        }
    });
    ```

1. Kaydet ve Kapat **simDevice.js** dosya.

> [!NOTE]
> Sade ve basit bir anlatım gözetildiği için bu öğretici herhangi bir yeniden deneme ilkesi uygulamaz. [Geçici Hata İşleme][lnk-transient-faults] adlı MSDN makalesinde önerildiği üzere, üretim kodunda yeniden deneme ilkelerini (üstel geri alma gibi) uygulamanız gerekir.

## <a name="run-the-apps"></a>Uygulamaları çalıştırma

Şimdi uygulamaları çalıştırmaya hazırsınız.

1. Komut isteminde **simDevice** klasörü, yeniden başlatma doğrudan yöntemi için dinleme başlamak için aşağıdaki komutu çalıştırın.

    ```cmd/sh
    node simDevice.js
    ```

1. C# konsol uygulaması çalıştırmak **ScheduleJob** sağ tıklayarak **ScheduleJob** sonra seçerek proje **hata ayıklama** ve **başlangıç yeni örnek**.

1. Cihaz ve arka uç uygulamaları çıktısını görürsünüz.

    ![İşlerini zamanlamak için uygulamaları çalıştırma][img-schedulejobs]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir cihaz ve cihaz çifti'nın özelliklerini güncelleştirmek için doğrudan bir yöntem zamanlamak için bir iş kullanılır.

IOT Hub ve Uzaktan gibi cihaz yönetim düzenleri hava bellenim güncelleştirme kullanmaya Başlarken devam etmek için okuma [öğretici: bir üretici yazılımı güncelleştirmesi nasıl][lnk-fwupdate].

AI sınır cihazları için Azure IOT Edge dağıtma hakkında bilgi edinmek için bkz: [IOT Edge ile çalışmaya başlama][lnk-iot-edge].

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-node-schedule-jobs/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-schedule-jobs/createnetapp.png
[img-schedulejobs]: media/iot-hub-csharp-node-schedule-jobs/schedulejobs.png

[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-props]: iot-hub-node-node-twin-how-to-configure.md
[lnk-c2d-methods]: iot-hub-node-node-direct-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-fwupdate]: iot-hub-node-node-firmware-update.md
[lnk-iot-edge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
