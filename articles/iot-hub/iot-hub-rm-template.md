---
title: "Bir şablon (.NET) kullanarak Azure IOT Hub oluşturma | Microsoft Docs"
description: "Bir IOT Hub ile C# programı oluşturmak için bir Azure Resource Manager şablonu kullanma"
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: a447b40c-c728-487e-875d-db554db5adc3
ms.service: iot-hub
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: 0f197a28e0c51b06d0b47a03c29fe1fde0c6b78d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-iot-hub-using-azure-resource-manager-template-net"></a>Azure Resource Manager şablonu (.NET) kullanılarak IOT hub oluşturma

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

Azure Resource Manager oluşturmak ve Azure IOT hub'ları programlı olarak yönetmek için kullanabilirsiniz. Bu öğretici bir Azure Resource Manager şablonu IOT hub'ı bir C# programı oluşturmak için nasıl kullanılacağını gösterir.

> [!NOTE]
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Azure Resource Manager ve klasik](../azure-resource-manager/resource-manager-deployment-model.md).  Bu makalede, Azure Resource Manager dağıtım modeli kullanılarak yer almaktadır.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Visual Studio 2015 veya Visual Studio 2017.
* Etkin bir Azure hesabı. <br/>Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.
* Bir [Azure depolama hesabı] [ lnk-storage-account] Azure Resource Manager şablonu dosyalarınızı depolayabileceğiniz.
* [Azure PowerShell 1.0] [ lnk-powershell-install] veya sonraki bir sürümü.

[!INCLUDE [iot-hub-prepare-resource-manager](../../includes/iot-hub-prepare-resource-manager.md)]

## <a name="prepare-your-visual-studio-project"></a>Visual Studio projenizi hazırlama

1. Visual Studio'da kullanarak bir Visual C# Windows Klasik Masaüstü projesi oluşturma **konsol uygulaması (.NET Framework)** proje şablonu. Proje adı **CreateIoTHub**.

2. Çözüm Gezgini'nde, projeye sağ tıklayın ve ardından **NuGet paketlerini Yönet**.

3. NuGet Paket Yöneticisi'nde denetleyin **INCLUDE yayın öncesi**ve **Gözat** sayfası Ara **Microsoft.Azure.Management.ResourceManager**. Paketi seçin, **yüklemek**, **değişiklikleri gözden** tıklatın **Tamam**, ardından **kabul ediyorum** lisanslar kabul etmek için.

4. NuGet Paket Yöneticisi'nde arayın **Microsoft.IdentityModel.Clients.activedirectory tarafından**.  ' I tıklatın **yüklemek**, **değişiklikleri gözden** tıklatın **Tamam**, ardından **kabul ediyorum** lisans kabul etmek için.

5. Program.cs içinde varolan **kullanarak** deyimleri ile aşağıdaki kodu:

    ```csharp
    using System;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    ```

6. Program.cs içinde yer tutucu değerlerini değiştirerek aşağıdaki statik değişkenler ekleyin. Not yapılan **ApplicationId**, **Subscriptionıd**, **Tenantıd**, ve **parola** Bu öğreticinin önceki. **Azure depolama hesabınızın adını** Azure depolama hesabı, Azure Resource Manager şablon dosyalarını depoladığınız adıdır. **Kaynak grubu adı** IOT hub'ı oluşturduğunuzda kullandığınız kaynak grubunun adı. Önceden varolan veya yeni bir kaynak grubu adı olabilir. **Dağıtım adı** dağıtımı için bir ad olduğu gibi **Deployment_01**.

    ```csharp
    static string applicationId = "{Your ApplicationId}";
    static string subscriptionId = "{Your SubscriptionId}";
    static string tenantId = "{Your TenantId}";
    static string password = "{Your application Password}";
    static string storageAddress = "https://{Your storage account name}.blob.core.windows.net";
    static string rgName = "{Resource group name}";
    static string deploymentName = "{Deployment name}";
    ```

[!INCLUDE [iot-hub-get-access-token](../../includes/iot-hub-get-access-token.md)]

## <a name="submit-a-template-to-create-an-iot-hub"></a>IOT hub'ı oluşturmak için bir şablon gönderin

IOT hub'ı, kaynak grubu oluşturmak için bir JSON şablonu ve parametre dosyası kullanın. Bir Azure Resource Manager şablonu, var olan bir IOT hub'ına değişiklik yapmak için de kullanabilirsiniz.

1. Çözüm Gezgini'nde, projeye sağ tıklayın, **Ekle**ve ardından **yeni öğe**. Adlı bir JSON dosyası ekleme **template.json** projenize.

2. Standart bir IOT hub'ına eklemek için **Doğu ABD** bölge Değiştir **template.json** aşağıdaki kaynak tanımı'yla. IOT hub'ı destekleyen bölgeleri geçerli listesi için bkz: [Azure durumu][lnk-status]:

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "hubName": {
          "type": "string"
        }
      },
      "resources": [
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs",
        "name": "[parameters('hubName')]",
        "location": "East US",
        "sku": {
          "name": "S1",
          "tier": "Standard",
          "capacity": 1
        },
        "properties": {
          "location": "East US"
        }
      }
      ],
      "outputs": {
        "hubKeys": {
          "value": "[listKeys(resourceId('Microsoft.Devices/IotHubs', parameters('hubName')), '2016-02-03')]",
          "type": "object"
        }
      }
    }
    ```

3. Çözüm Gezgini'nde, projeye sağ tıklayın, **Ekle**ve ardından **yeni öğe**. Adlı bir JSON dosyası ekleme **parameters.json** projenize.

4. Değiştir **parameters.json** yeni IOT hub'ı için bir ad gibi ayarlar aşağıdaki parametre bilgilerle **{adınızın baş harflerini} mynewiothub**. Adı veya baş harfleri içermelidir şekilde IOT hub'ı adı genel olarak benzersiz olması gerekir:

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "hubName": { "value": "mynewiothub" }
      }
    }
    ```
  [!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

5. İçinde **Sunucu Gezgini**, Azure aboneliğinize bağlanmak ve Azure depolama hesabınızdaki adlı bir kapsayıcı oluşturmak **şablonları**. İçinde **özellikleri** paneli, Ayarla **herkese okuma erişimi** izinlerini **şablonları** kapsayıcıya **Blob**.

6. İçinde **Sunucu Gezgini**, sağ tıklayın **şablonları** kapsayıcı ve ardından **görünüm Blob kapsayıcısını**. Tıklatın **karşıya Blob** düğmesini tıklatın, şu iki dosyayı seçmeniz **parameters.json** ve **templates.json**ve ardından **açık** karşıya yüklemek için JSON dosyaları **şablonları** kapsayıcı. JSON verilerini içeren BLOB'ları URL'lerini şunlardır:

    ```csharp
    https://{Your storage account name}.blob.core.windows.net/templates/parameters.json
    https://{Your storage account name}.blob.core.windows.net/templates/template.json
    ```
7. Aşağıdaki yöntemi ekleyin:

    ```csharp
    static void CreateIoTHub(ResourceManagementClient client)
    {

    }
    ```

8. Aşağıdaki kodu ekleyin **CreateIoTHub** yöntemi için Azure Resource Manager şablonu ve parametre dosyalarını göndermek için:

    ```csharp
    var createResponse = client.Deployments.CreateOrUpdate(
        rgName,
        deploymentName,
        new Deployment()
        {
          Properties = new DeploymentProperties
          {
            Mode = DeploymentMode.Incremental,
            TemplateLink = new TemplateLink
            {
              Uri = storageAddress + "/templates/template.json"
            },
            ParametersLink = new ParametersLink
            {
              Uri = storageAddress + "/templates/parameters.json"
            }
          }
        });
    ```

9. Aşağıdaki kodu ekleyin **CreateIoTHub** durumunu ve yeni IOT hub'ı tuşları görüntüler yöntemi:

    ```csharp
    string state = createResponse.Properties.ProvisioningState;
    Console.WriteLine("Deployment state: {0}", state);

    if (state != "Succeeded")
    {
      Console.WriteLine("Failed to create iothub");
    }
    Console.WriteLine(createResponse.Properties.Outputs);
    ```

## <a name="complete-and-run-the-application"></a>Tamamlayın ve uygulamayı çalıştırın

Çağırarak uygulamayı şimdi tamamlayabilirsiniz **CreateIoTHub** oluşturmak ve çalıştırmak için önce yöntemi.

1. Sonuna aşağıdaki kodu ekleyin **ana** yöntemi:

    ```csharp
    CreateIoTHub(client);
    Console.ReadLine();
    ```

2. Tıklatın **yapı** ve ardından **yapı çözümü**. Hataları düzeltin.

3. Tıklatın **hata ayıklama** ve ardından **hata ayıklamayı Başlat** uygulamayı çalıştırın. Çalıştırmak için dağıtım için birkaç dakika sürebilir.

4. Uygulamanızı eklenen yeni IOT hub'ı doğrulamak için ziyaret edin [Azure portal] [ lnk-azure-portal] ve kaynakları listesini görüntüleyin. Alternatif olarak, kullanın **Get-AzureRmResource** PowerShell cmdlet'i.

> [!NOTE]
> Bu örnek uygulama S1 standart IOT için faturalandırılır hub'ı ekler. IOT hub'ı aracılığıyla silebilirsiniz [Azure portal] [ lnk-azure-portal] veya kullanarak **Kaldır AzureRmResource** bitirdiğinizde PowerShell cmdlet'i.

## <a name="next-steps"></a>Sonraki adımlar
Bir C# programı ile bir Azure Resource Manager şablonu kullanarak bir IOT hub dağıttığınız artık daha ayrıntılı incelemek isteyebilirsiniz:

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
[lnk-storage-account]:../storage/common/storage-create-storage-account.md

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
