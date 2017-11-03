---
title: "Azure IOT hub'ı doğrudan yöntemleri (.NET/düğüm) kullanın | Microsoft Docs"
description: "Azure IOT hub'ı doğrudan yöntemlerinin nasıl kullanılacağını. Doğrudan bir yöntem ve Azure IOT hizmeti SDK'sını doğrudan yöntemini çağıran bir hizmet uygulaması uygulamak .NET için içeren bir sanal cihaz uygulamasının uygulamak için Azure IOT cihaz SDK'sı Node.js için kullanın."
services: iot-hub
documentationcenter: 
author: nberdy
manager: timlt
editor: 
ms.assetid: ab035b8e-bff8-4e12-9536-f31d6b6fe425
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/30/2017
ms.author: nberdy
ms.openlocfilehash: 76f1d32b4afeacae1488b4cf28be6c8cf7f4ea37
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-direct-methods-netnode"></a>Doğrudan yöntemleri (.NET/düğüm) kullanın
[!INCLUDE [iot-hub-selector-c2d-methods](../../includes/iot-hub-selector-c2d-methods.md)]

Bu öğreticide, biz bir .NET ve bir Node.js konsol uygulaması geliştirme olacak:

* **CallMethodOnDevice.sln**, sanal cihaz uygulamasının bir yöntemi çağırır ve yanıt görüntüler .NET arka uç uygulaması.
* **SimulatedDevice.js**, daha önce oluşturulan cihaz kimliğiyle IOT hub'ınıza bağlanan bir cihaza benzetim ve bulut tarafından çağrılan yöntemi yanıtlaması bir Node.js uygulaması.

> [!NOTE]
> [IoT Hub SDK'ları][lnk-hub-sdks] makalesi, hem cihazlarınızda hem de çözüm arka ucunuzda çalıştırılacak uygulamalar oluşturmak için kullanabileceğiniz Azure IoT SDK’ları hakkında bilgi içerir.
> 
> 

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Visual Studio 2015 veya Visual Studio 2017.
* Node.js 4.0.x sürümü veya sonraki bir sürüm.
* Etkin bir Azure hesabı. (Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a>Sanal cihaz uygulaması oluşturma
Bu bölümde, çözüm tarafından arka uç olarak adlandırılan bir yönteme yanıt bir Node.js konsol uygulaması oluşturun.

1. **simulateddevice** adlı yeni bir boş klasör oluşturun. Komut isteminizde aşağıdaki komutu kullanarak **simulateddevice** klasöründe bir package.json dosyası oluşturun. Tüm varsayılanları kabul edin:
   
    ```
    npm init
    ```
2. Komut isteminizde **simulateddevice** klasörü yüklemek için aşağıdaki komutu çalıştırın, **azure IOT cihaz** ve **azure-IOT-cihaz-mqtt** paketler:
   
    ```
        npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Bir metin düzenleyicisi kullanarak, bir dosyada oluşturmak **simulateddevice** klasörü ve adlandırın **SimulatedDevice.js**.
4. Aşağıdaki `require` deyimlerini **SimulatedDevice.js** dosyasının başlangıcına ekleyin:
   
    ```
    'use strict';
   
    var Mqtt = require('azure-iot-device-mqtt').Mqtt;
    var DeviceClient = require('azure-iot-device').Client;
    ```
5. Ekleme bir **connectionString** değişkeni ve oluşturmak için kullanın bir **DeviceClient** örneği. Değiştir **{cihaz bağlantı dizesi}** cihaz bağlantısı, üretildi dize *bir cihaz kimliği oluşturma* bölümü:
   
    ```
    var connectionString = '{device connection string}';
    var client = DeviceClient.fromConnectionString(connectionString, Mqtt);
    ```
6. Cihazda doğrudan yöntemi uygulamak için aşağıdaki işlevi ekleyin:
   
    ```
    function onWriteLine(request, response) {
        console.log(request.payload);
   
        response.send(200, 'Input was written to log.', function(err) {
            if(err) {
                console.error('An error occurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.' );
            }
        });
    }
    ```
7. IOT hub'ınıza bağlantıyı açın ve yöntemi dinleyicisi başlatılamıyor:
   
    ```
    client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');
            client.onDeviceMethod('writeLine', onWriteLine);
        }
    });
    ```
8. **SimulatedDevice.js** dosyasını kaydedin ve kapatın.

> [!NOTE]
> Sade ve basit bir anlatım gözetildiği için bu öğretici herhangi bir yeniden deneme ilkesi uygulamaz. Üretim kodunda yeniden deneme ilkelerini (bağlantı yeniden deneme), önerilen MSDN makalesinde uygulamalıdır [geçici hata işleme][lnk-transient-faults].
> 
> 

## <a name="call-a-direct-method-on-a-device"></a>Bir cihazda doğrudan bir yöntem çağrısı
Bu bölümde, sanal cihaz uygulamasının bir yöntemi çağırır ve yanıt görüntüleyen bir .NET konsol uygulaması oluşturun.

1. Visual Studio'da **Konsol Uygulaması** proje şablonunu kullanarak geçerli çözüme bir Visual C# Windows Klasik Masaüstü projesi ekleyin. .NET Framework sürümünün 4.5.1 veya sonraki bir sürüm olduğundan emin olun. Proje adı **CallMethodOnDevice**.
   
    ![Yeni Visual C# Windows Klasik Masaüstü projesi][10]
2. Çözüm Gezgini'nde sağ **CallMethodOnDevice** proje ve ardından **NuGet paketlerini Yönet...** .
3. **NuGet Paket Yöneticisi** penceresinde **Gözat**'ı seçin, **microsoft.azure.devices**'ı aratın, **Microsoft.Azure.Devices** paketini yüklemek için **Yükle**'yi seçin ve kullanım koşullarını kabul edin. Bu yordam ile [Azure IoT hizmet SDK'sı][lnk-nuget-service-sdk] NuGet paketi ve bağımlılıkları indirilir, yüklenir ve bu pakete bir başvuru eklenir.
   
    ![NuGet Paket Yöneticisi penceresi][11]

4. Aşağıdaki `using` deyimlerini **Program.cs** dosyasının üst kısmına ekleyin:
   
        using System.Threading.Tasks;
        using Microsoft.Azure.Devices;
5. **Program** sınıfına aşağıdaki alanları ekleyin. Yer tutucu değerini, önceki bölümde hub için oluşturduğunuz IoT Hub bağlantı dizesiyle değiştirin.
   
        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";
6. **Program** sınıfına aşağıdaki yöntemi ekleyin:
   
        private static async Task InvokeMethod()
        {
            var methodInvocation = new CloudToDeviceMethod("writeLine") { ResponseTimeout = TimeSpan.FromSeconds(30) };
            methodInvocation.SetPayloadJson("'a line to be written'");

            var response = await serviceClient.InvokeDeviceMethodAsync("myDeviceId", methodInvocation);

            Console.WriteLine("Response status: {0}, payload:", response.Status);
            Console.WriteLine(response.GetPayloadAsJson());
        }
   
    Bu yöntem adı ile doğrudan bir yöntem çağırır `writeLine` üzerinde `myDeviceId` aygıt. Ardından, konsol aygıtta tarafından sağlanan yanıt yazar. Nasıl yanıt aygıta için zaman aşımı değeri belirlemek mümkün olduğunu unutmayın.
7. Son olarak, **Main** yöntemine aşağıdaki satırları ekleyin:
   
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
        InvokeMethod().Wait();
        Console.WriteLine("Press Enter to exit.");
        Console.ReadLine();

## <a name="run-the-applications"></a>Uygulamaları çalıştırma
Şimdi uygulamaları çalıştırmaya hazırsınız.

1. Visual Studio Çözüm Gezgini'nde Çözümünüze sağ tıklayın ve ardından **başlangıç projelerini Ayarla...** . Seçin **tek başlangıç projesi**ve ardından **CallMethodOnDevice** açılır menüsünde projesinde.

2. Bir komut isteminde **simulateddevice** klasörü, IOT Hub'ınızı gelen yöntem çağrıları için dinleme başlatmak için aşağıdaki komutu çalıştırın:
   
    ```
    node SimulatedDevice.js
    ```
   Bekleme açmak sanal cihaz için:![][7]
3. Artık, cihazın bağlı ve .NET çalıştırmak için yöntem çağrılarına, bekleyen **CallMethodOnDevice** sanal cihaz uygulamasının yöntemi çağırmak için uygulama. Konsolda yazılmış aygıt yanıtı görmeniz gerekir.
   
    ![][8]
4. Cihaz daha sonra bu iletiyi yazdırma tarafından yönteme tepki verdiğini:
   
    ![][9]

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, Azure portalında yeni bir IoT hub'ı yapılandırdınız ve ardından IoT hub'ının kimlik kayıt defterinde bir cihaz kimliği oluşturdunuz. Bu cihaz kimliğini bulut tarafından çağrılan yöntemler tepki vermek sanal cihaz uygulamasının sağlamak için kullanılır. Ayrıca aygıtta yöntemleri çağırır ve aygıttan yanıt görüntüleyen bir uygulama oluşturduğunuz. 

IoT Hub’ı kullanmaya başlamak ve diğer IoT senaryolarını keşfetmek için bkz:

* [IOT Hub ile çalışmaya başlama]
* [Birden çok aygıta işleri zamanla][lnk-devguide-jobs]

Çözüm ve zamanlama yöntemini çağıran birden fazla cihazda, IOT genişletmek öğrenmek için bkz: [zamanlama ve yayın işleri] [ lnk-tutorial-jobs] Öğreticisi.

<!-- Images. -->
[7]: ./media/iot-hub-csharp-node-direct-methods/run-simulated-device.png
[8]: ./media/iot-hub-csharp-node-direct-methods/netserviceapp.png
[9]: ./media/iot-hub-csharp-node-direct-methods/methods-output.png

[10]: ./media/iot-hub-csharp-node-direct-methods/direct-methods-csharp1.png
[11]: ./media/iot-hub-csharp-node-direct-methods/direct-methods-csharp2.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-devguide-methods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[Send Cloud-to-Device messages with IoT Hub]: iot-hub-csharp-csharp-c2d.md
[Process Device-to-Cloud messages]: iot-hub-csharp-csharp-process-d2c.md
[IOT Hub ile çalışmaya başlama]: iot-hub-node-node-getstarted.md
