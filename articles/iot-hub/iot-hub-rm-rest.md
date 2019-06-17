---
title: Kaynak sağlayıcısı REST API'si kullanarak Azure IOT hub oluşturma | Microsoft Docs
description: IOT hub'ı oluşturmak için kaynak sağlayıcısı REST API'si kullanma
author: robinsh
manager: philmea
ms.author: robinsh
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: conceptual
ms.date: 08/08/2017
ms.openlocfilehash: 6d91f5e61dfd7c3cb4d1869edf0c6cb8c2c85190
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65827488"
---
# <a name="create-an-iot-hub-using-the-resource-provider-rest-api-net"></a>Kaynak sağlayıcısı REST API (.NET) kullanarak IOT hub oluşturma

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

Kullanabileceğiniz [IOT hub'ı kaynak sağlayıcısı REST API'si](https://docs.microsoft.com/rest/api/iothub/iothubresource) oluşturmak ve Azure IOT hub'ları programlı olarak yönetmek için. Bu öğreticide bir C# programı bir IOT hub'ı oluşturmak için IOT hub'ı kaynak sağlayıcısı REST API'si kullanma işlemini gösterir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Visual Studio.

* Etkin bir Azure hesabı. Bir hesabınız yoksa, oluşturabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) yalnızca birkaç dakika içinde.

* [Azure PowerShell 1.0](https://docs.microsoft.com/powershell/azure/install-Az-ps) veya üzeri.

[!INCLUDE [iot-hub-prepare-resource-manager](../../includes/iot-hub-prepare-resource-manager.md)]

## <a name="prepare-your-visual-studio-project"></a>Visual Studio projenizi hazırlama

1. Bir Visual C# Windows Klasik Masaüstü projesi kullanarak Visual Studio'da oluşturma **konsol uygulaması (.NET Framework)** proje şablonu. Projeyi adlandırın **CreateIoTHubREST**.

2. Çözüm Gezgini'nde projenize sağ tıklayın ve ardından **NuGet paketlerini Yönet**.

3. NuGet Paket Yöneticisi'nde kontrol **INCLUDE yayın öncesi**ve **Gözat** sayfa arama **Microsoft.Azure.Management.ResourceManager**. Paketi seçin, **yükleme**, **değişiklikleri gözden geçir** tıklayın **Tamam**, ardından **kabul ediyorum** lisanslar kabul etmek için.

4. NuGet Paket Yöneticisi'nde arayın **Microsoft.IdentityModel.Clients.activedirectory**.  Tıklayın **yükleme**, **değişiklikleri gözden** tıklayın **Tamam**, ardından **kabul ediyorum** lisansı kabul edin.

5. Program.cs içinde varolan **kullanarak** deyimlerini aşağıdaki kod ile:

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

6. Program.cs içinde yer tutucu değerlerini değiştirerek aşağıdaki statik değişkenler ekleyin. Bir Not **ApplicationId**, **Subscriptionıd**, **Tenantıd**, ve **parola** Bu öğreticide daha önce. **Kaynak grubu adı** , IOT hub'ı oluştururken kullandığınız kaynak grubunun adıdır. Önceden var olan veya yeni bir kaynak grubu kullanabilirsiniz. **IOT hub'ı adı** adı gibi oluşturduğunuz IOT Hub'ın **MyIoTHub**. IOT hub'ınızın adını genel olarak benzersiz olmalıdır. **Dağıtım adı** dağıtımı için bir ad olduğu gibi **Deployment_01**.

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

## <a name="use-the-resource-provider-rest-api-to-create-an-iot-hub"></a>IOT hub'ı oluşturmak için kaynak sağlayıcısı REST API'si kullanma

Kullanım [IOT hub'ı kaynak sağlayıcısı REST API'si](https://docs.microsoft.com/rest/api/iothub/iothubresource) kaynak grubunuzda bir IOT hub'ı oluşturmak için. Var olan bir IOT hub'ına değişiklik yapmak için kaynak sağlayıcısı REST API de kullanabilirsiniz.

1. Program.cs'ye aşağıdaki yöntemi ekleyin:

    ```csharp
    static void CreateIoTHub(string token)
    {

    }
    ```

2. Aşağıdaki kodu ekleyin **CreateIoTHub** yöntemi. Bu kod oluşturur bir **HttpClient** üst bilgisindeki kimlik doğrulama belirteci ile nesnesi:

    ```csharp
    HttpClient client = new HttpClient();
    client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", token);
    ```

3. Aşağıdaki kodu ekleyin **CreateIoTHub** yöntemi. Bu kod, IOT hub'ının oluşturulacağı açıklanır ve JSON temsili üretir. IOT hub'ı destekleyen konumları güncel listesi için bkz. [Azure durumu](https://azure.microsoft.com/status/):

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

4. Aşağıdaki kodu ekleyin **CreateIoTHub** yöntemi. Bu kod, Azure REST isteği gönderir. Kod yanıt denetler ve dağıtım görevini durumunu izlemek için kullanabileceğiniz URL'sini alır:

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

5. Sonuna aşağıdaki kodu ekleyin **CreateIoTHub** yöntemi. Bu kod **asyncStatusUri** alınan adresi önceki adımda dağıtımın bitmesini bekleyin:

    ```csharp
    string body;
    do
    {
      Thread.Sleep(10000);
      HttpResponseMessage deploymentstatus = client.GetAsync(asyncStatusUri).Result;
      body = deploymentstatus.Content.ReadAsStringAsync().Result;
    } while (body == "{\"status\":\"Running\"}");
    ```

6. Sonuna aşağıdaki kodu ekleyin **CreateIoTHub** yöntemi. Bu kod, oluşturduğunuz ve bunları konsola yazdırır IOT hub'ının anahtarları alır:

    ```csharp
    var listKeysUri = string.Format("https://management.azure.com/subscriptions/{0}/resourceGroups/{1}/providers/Microsoft.Devices/IotHubs/{2}/IoTHubKeys/listkeys?api-version=2016-02-03", subscriptionId, rgName, iotHubName);
    var keysresults = client.PostAsync(listKeysUri, null).Result;

    Console.WriteLine("Keys: {0}", keysresults.Content.ReadAsStringAsync().Result);
    ```

## <a name="complete-and-run-the-application"></a>Tamamlayın ve uygulamayı çalıştırın

Çağırarak artık uygulama tamamlayabilirsiniz **CreateIoTHub** yöntemi önce derleyin ve çalıştırın.

1. Sonuna aşağıdaki kodu ekleyin **ana** yöntemi:

    ```csharp
    CreateIoTHub(token.AccessToken);
    Console.ReadLine();
    ```

2. Tıklayın **derleme** ardından **yapı çözümü**. Hataları düzeltin.

3. Tıklayın **hata ayıklama** ardından **hata ayıklamayı Başlat** uygulamayı çalıştırın. Bu, dağıtımın çalıştırmak birkaç dakika sürebilir.

4. Uygulamanızı yeni IOT hub'ı eklediğinizi doğrulamak için şurayı ziyaret edin [Azure portalında](https://portal.azure.com/) ve kaynakların listesini görüntüleyin. Alternatif olarak, **Get-AzResource** PowerShell cmdlet'i.

> [!NOTE]
> Bu örnek uygulama, bir S1 standart IOT için faturalandırılırsınız Hub ekler. İşlemi tamamladığınızda, IOT hub'ı aracılığıyla silebilirsiniz [Azure portalı](https://portal.azure.com/) kullanarak veya **Remove-AzResource** işiniz bittiğinde PowerShell cmdlet'i.

## <a name="next-steps"></a>Sonraki adımlar

Kaynak sağlayıcısı REST API'si kullanarak bir IOT hub'ı dağıttığınız artık daha iyi keşfedilebilmesi isteyebilirsiniz:

* Özellikleri hakkında okuyun [IOT hub'ı kaynak sağlayıcısı REST API'si](https://docs.microsoft.com/rest/api/iothub/iothubresource).

* Okuma [Azure Resource Manager'a genel bakış](../azure-resource-manager/resource-group-overview.md) Azure Resource Manager'ın özellikleri hakkında daha fazla bilgi edinmek için.

İçin IOT Hub ile geliştirme hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [C SDK'ya giriş](iot-hub-device-sdk-c-intro.md)

* [Azure IoT SDK’ları](iot-hub-devguide-sdks.md)

Daha fazla IOT Hub'ın özelliklerini keşfetmek için bkz:

* [Yapay ZEKA, Azure IOT Edge ile uç cihazlarına dağıtma](../iot-edge/tutorial-simulate-device-linux.md)