---
title: "Kaynak sağlayıcısı REST API kullanarak Azure IOT hub oluşturma | Microsoft Docs"
description: "IOT hub'ı oluşturmak için kaynak sağlayıcısı REST API kullanma"
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 52814ee5-bc10-4abe-9eb2-f8973096c2d8
ms.service: iot-hub
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: e443259507aacbefca141be4c9c1688ab19bf6ec
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2017
---
# <a name="create-an-iot-hub-using-the-resource-provider-rest-api-net"></a>Kaynak sağlayıcısı REST API (.NET) kullanılarak IOT hub oluşturma

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

Kullanabileceğiniz [IOT hub'ı kaynak sağlayıcısı REST API] [ lnk-rest-api] oluşturmak ve Azure IOT hub'ları programlı olarak yönetmek için. Bu öğretici, IOT hub'ı kaynak sağlayıcısı REST API IOT hub'ı bir C# programı oluşturmak için nasıl kullanılacağını gösterir.

> [!NOTE]
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Azure Resource Manager ve klasik](../azure-resource-manager/resource-manager-deployment-model.md).  Bu makalede, Azure Resource Manager dağıtım modeli kullanılarak yer almaktadır.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Visual Studio 2015 veya Visual Studio 2017.
* Etkin bir Azure hesabı. <br/>Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.
* [Azure PowerShell 1.0] [ lnk-powershell-install] veya sonraki bir sürümü.

[!INCLUDE [iot-hub-prepare-resource-manager](../../includes/iot-hub-prepare-resource-manager.md)]

## <a name="prepare-your-visual-studio-project"></a>Visual Studio projenizi hazırlama

1. Visual Studio'da kullanarak bir Visual C# Windows Klasik Masaüstü projesi oluşturma **konsol uygulaması (.NET Framework)** proje şablonu. Proje adı **CreateIoTHubREST**.

2. Çözüm Gezgini'nde, projeye sağ tıklayın ve ardından **NuGet paketlerini Yönet**.

3. NuGet Paket Yöneticisi'nde denetleyin **INCLUDE yayın öncesi**ve **Gözat** sayfası Ara **Microsoft.Azure.Management.ResourceManager**. Paketi seçin, **yüklemek**, **değişiklikleri gözden** tıklatın **Tamam**, ardından **kabul ediyorum** lisanslar kabul etmek için.

4. NuGet Paket Yöneticisi'nde arayın **Microsoft.IdentityModel.Clients.activedirectory tarafından**.  ' I tıklatın **yüklemek**, **değişiklikleri gözden** tıklatın **Tamam**, ardından **kabul ediyorum** lisans kabul etmek için.

5. Program.cs içinde varolan **kullanarak** deyimleri ile aşağıdaki kodu:

    ```csharp
    using System;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Text;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Newtonsoft.Json;
    using Microsoft.Rest;
    using System.Linq;
    using System.Threading;
    ```

6. Program.cs içinde yer tutucu değerlerini değiştirerek aşağıdaki statik değişkenler ekleyin. Not yapılan **ApplicationId**, **Subscriptionıd**, **Tenantıd**, ve **parola** Bu öğreticinin önceki. **Kaynak grubu adı** IOT hub'ı oluşturduğunuzda kullandığınız kaynak grubunun adı. Önceden varolan veya yeni bir kaynak grubu kullanabilirsiniz. **IOT hub'ı adı** gibi oluşturduğunuz IOT Hub adı **MyIoTHub**. IOT hub'ınızı adı genel olarak benzersiz olması gerekir. **Dağıtım adı** dağıtımı için bir ad olduğu gibi **Deployment_01**.

    ```csharp
    static string applicationId = "{Your ApplicationId}";
    static string subscriptionId = "{Your SubscriptionId}";
    static string tenantId = "{Your TenantId}";
    static string password = "{Your application Password}";

    static string rgName = "{Resource group name}";
    static string iotHubName = "{IoT Hub name including your initials}";
    ```
[!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

[!INCLUDE [iot-hub-get-access-token](../../includes/iot-hub-get-access-token.md)]

## <a name="use-the-resource-provider-rest-api-to-create-an-iot-hub"></a>IOT hub'ı oluşturmak için kaynak sağlayıcısı REST API kullanın

Kullanım [IOT hub'ı kaynak sağlayıcısı REST API] [ lnk-rest-api] IOT hub'ı, kaynak grubu oluşturmak için. Var olan bir IOT hub'ına değişiklik yapmak için kaynak sağlayıcısı REST API de kullanabilirsiniz.

1. Aşağıdaki yöntemi ekleyin:

    ```csharp
    static void CreateIoTHub(string token)
    {

    }
    ```

2. Aşağıdaki kodu ekleyin **CreateIoTHub** yöntemi. Bu kod oluşturur bir **HttpClient** nesne kimlik doğrulama belirteci üst bilgileri ile:

    ```csharp
    HttpClient client = new HttpClient();
    client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", token);
    ```

3. Aşağıdaki kodu ekleyin **CreateIoTHub** yöntemi. Bu kod, IOT hub'ı oluşturmak için açıklar ve bir JSON temsili üretir. IOT hub'ı destekleyen konumları geçerli listesi için bkz [Azure durumu][lnk-status]:

    ```csharp
    var description = new
    {
      name = iotHubName,
      location = "East US",
      sku = new
      {
        name = "S1",
        tier = "Standard",
        capacity = 1
      }
    };

    var json = JsonConvert.SerializeObject(description, Formatting.Indented);
    ```

4. Aşağıdaki kodu ekleyin **CreateIoTHub** yöntemi. Bu kod Azure REST isteği gönderir. Kod yanıtı denetler ve dağıtım görev durumunu izlemek için kullanabileceğiniz URL'sini alır:

    ```csharp
    var content = new StringContent(JsonConvert.SerializeObject(description), Encoding.UTF8, "application/json");
    var requestUri = string.Format("https://management.azure.com/subscriptions/{0}/resourcegroups/{1}/providers/Microsoft.devices/IotHubs/{2}?api-version=2016-02-03", subscriptionId, rgName, iotHubName);
    var result = client.PutAsync(requestUri, content).Result;

    if (!result.IsSuccessStatusCode)
    {
      Console.WriteLine("Failed {0}", result.Content.ReadAsStringAsync().Result);
      return;
    }

    var asyncStatusUri = result.Headers.GetValues("Azure-AsyncOperation").First();
    ```

5. Sonuna aşağıdaki kodu ekleyin **CreateIoTHub** yöntemi. Bu kodu kullanır **asyncStatusUri** adresi alınan dağıtım tamamlanmasını beklemek için önceki adımda:

    ```csharp
    string body;
    do
    {
      Thread.Sleep(10000);
      HttpResponseMessage deploymentstatus = client.GetAsync(asyncStatusUri).Result;
      body = deploymentstatus.Content.ReadAsStringAsync().Result;
    } while (body == "{\"status\":\"Running\"}");
    ```

6. Sonuna aşağıdaki kodu ekleyin **CreateIoTHub** yöntemi. Bu kod IOT hub oluşturulur ve bunları konsola yazdırır anahtarları alır:

    ```csharp
    var listKeysUri = string.Format("https://management.azure.com/subscriptions/{0}/resourceGroups/{1}/providers/Microsoft.Devices/IotHubs/{2}/IoTHubKeys/listkeys?api-version=2016-02-03", subscriptionId, rgName, iotHubName);
    var keysresults = client.PostAsync(listKeysUri, null).Result;

    Console.WriteLine("Keys: {0}", keysresults.Content.ReadAsStringAsync().Result);
    ```

## <a name="complete-and-run-the-application"></a>Tamamlayın ve uygulamayı çalıştırın

Çağırarak uygulamayı şimdi tamamlayabilirsiniz **CreateIoTHub** oluşturmak ve çalıştırmak için önce yöntemi.

1. Sonuna aşağıdaki kodu ekleyin **ana** yöntemi:

    ```csharp
    CreateIoTHub(token.AccessToken);
    Console.ReadLine();
    ```

2. Tıklatın **yapı** ve ardından **yapı çözümü**. Hataları düzeltin.

3. Tıklatın **hata ayıklama** ve ardından **hata ayıklamayı Başlat** uygulamayı çalıştırın. Çalıştırmak için dağıtım için birkaç dakika sürebilir.

4. Uygulamanızı yeni IOT hub'ı eklediğinizi doğrulamak için ziyaret [Azure portal] [ lnk-azure-portal] ve kaynakları listesini görüntüleyin. Alternatif olarak, kullanın **Get-AzureRmResource** PowerShell cmdlet'i.

> [!NOTE]
> Bu örnek uygulama S1 standart IOT için faturalandırılır hub'ı ekler. İşiniz bittiğinde, IOT hub'ı aracılığıyla silebilirsiniz [Azure portal] [ lnk-azure-portal] veya kullanarak **Kaldır AzureRmResource** bitirdiğinizde PowerShell cmdlet'i.

## <a name="next-steps"></a>Sonraki adımlar
Kaynak sağlayıcısı REST API kullanarak IOT hub'ı dağıttığınız artık daha ayrıntılı incelemek isteyebilirsiniz:

* Özelliklerinin tamamı hakkında okuyun [IOT hub'ı kaynak sağlayıcısı REST API][lnk-rest-api].
* Okuma [Azure Resource Manager'a genel bakış] [ lnk-azure-rm-overview] Azure Kaynak Yöneticisi'nin özellikleri hakkında daha fazla bilgi edinmek için.

IOT Hub için geliştirme hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [C SDK Giriş][lnk-c-sdk]
* [Azure IOT SDK'ları][lnk-sdks]

Daha fazla IOT hub'ı özelliklerini keşfetmek için bkz:

* [Bir aygıt ile Azure IOT kenar benzetimini yapma][lnk-iotedge]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-powershell-install]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps
[lnk-rest-api]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-azure-rm-overview]: ../azure-resource-manager/resource-group-overview.md

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
