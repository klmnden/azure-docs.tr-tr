---
title: Şablon (.NET) kullanarak Azure IOT Hub oluşturma | Microsoft Docs
description: Bir C# programı ile IOT hub'ı oluşturmak için bir Azure Resource Manager şablonu kullanma
author: robinsh
manager: philmea
ms.author: robinsh
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: conceptual
ms.date: 08/08/2017
ms.openlocfilehash: 84446090da2feaee3005b4ef90ace77b468a3f1a
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59792599"
---
# <a name="create-an-iot-hub-using-azure-resource-manager-template-net"></a>Azure Resource Manager şablonu (.NET) kullanarak IOT hub oluşturma

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

Azure Resource Manager'ı oluşturma ve Azure IOT hub'ları programlı olarak yönetmek için kullanabilirsiniz. Bu öğreticide bir Azure Resource Manager şablonu bir IOT hub'ı bir C# programı oluşturma için nasıl kullanılacağını gösterir.

> [!NOTE]
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır:  [Azure Resource Manager ve klasik](../azure-resource-manager/resource-manager-deployment-model.md).  Bu makalede Azure Resource Manager dağıtım modelini incelemektedir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Visual Studio 2015 veya Visual Studio 2017.
* Etkin bir Azure hesabı. <br/>Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.
* Bir [Azure depolama hesabı] [ lnk-storage-account] , Azure Resource Manager şablonu dosyalarını depolayabileceğiniz.
* [Azure PowerShell 1.0] [ lnk-powershell-install] veya üzeri.

[!INCLUDE [iot-hub-prepare-resource-manager](../../includes/iot-hub-prepare-resource-manager.md)]

## <a name="prepare-your-visual-studio-project"></a>Visual Studio projenizi hazırlama

1. Bir Visual C# Windows Klasik Masaüstü projesi kullanarak Visual Studio'da oluşturma **konsol uygulaması (.NET Framework)** proje şablonu. Projeyi adlandırın **CreateIoTHub**.

2. Çözüm Gezgini'nde projenize sağ tıklayın ve ardından **NuGet paketlerini Yönet**.

3. NuGet Paket Yöneticisi'nde kontrol **INCLUDE yayın öncesi**ve **Gözat** sayfa arama **Microsoft.Azure.Management.ResourceManager**. Paketi seçin, **yükleme**, **değişiklikleri gözden geçir** tıklayın **Tamam**, ardından **kabul ediyorum** lisanslar kabul etmek için.

4. NuGet Paket Yöneticisi'nde arayın **Microsoft.IdentityModel.Clients.activedirectory**.  Tıklayın **yükleme**, **değişiklikleri gözden** tıklayın **Tamam**, ardından **kabul ediyorum** lisansı kabul edin.

5. Program.cs içinde varolan **kullanarak** deyimlerini aşağıdaki kod ile:

    ```csharp
    using System;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    ```

6. Program.cs içinde yer tutucu değerlerini değiştirerek aşağıdaki statik değişkenler ekleyin. Bir Not **ApplicationId**, **Subscriptionıd**, **Tenantıd**, ve **parola** Bu öğreticide daha önce. **Azure depolama hesabınızın adını** , Azure Resource Manager şablonu dosyalarını depoladığınız Azure depolama hesabının adıdır. **Kaynak grubu adı** , IOT hub'ı oluştururken kullandığınız kaynak grubunun adıdır. Önceden mevcut veya yeni bir kaynak grubu adı olabilir. **Dağıtım adı** dağıtımı için bir ad olduğu gibi **Deployment_01**.

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

## <a name="submit-a-template-to-create-an-iot-hub"></a>IOT hub'ı oluşturmak için bir şablon gönderme

Bir JSON şablonu ve parametre dosyası, kaynak grubunuzda bir IOT hub'ı oluşturmak için kullanın. Var olan bir IOT hub'ına değişiklik yapmak için bir Azure Resource Manager şablonunu da kullanabilirsiniz.

1. Çözüm Gezgini'nde projenize sağ tıklayın, **Ekle**ve ardından **yeni öğe**. Adlı bir JSON dosyası ekleme **template.json** projenize.

2. Standart IOT hub'ına eklemek için **Doğu ABD** bölge içeriklerini **template.json** aşağıdaki kaynak tanımına sahip. IOT hub'ı destekleyen bölgeleri güncel listesi için bkz. [Azure durumu][lnk-status]:

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

3. Çözüm Gezgini'nde projenize sağ tıklayın, **Ekle**ve ardından **yeni öğe**. Adlı bir JSON dosyası ekleme **parameters.json** projenize.

4. Öğesinin içeriğini değiştirin **parameters.json** yeni IOT hub'ı için bir ad gibi ayarlar aşağıdaki parametre bilgilerle **{adınızın baş harflerini} mynewiothub**. Adı veya baş harfleri içermelidir için IOT hub'ı adı genel olarak benzersiz olması gerekir:

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

5. İçinde **Sunucu Gezgini**, Azure aboneliğinize bağlayın ve Azure depolama hesabınızdaki adlı bir kapsayıcı oluşturma **şablonları**. İçinde **özellikleri** ayarlayın **genel okuma erişimini** izinlerini **şablonları** kapsayıcıya **Blob**.

6. İçinde **Sunucu Gezgini**, sağ **şablonları** kapsayıcı ve ardından **görünüm Blob kapsayıcısını**. Tıklayın **Blob karşıya** düğme, iki dosyayı seçmeniz **parameters.json** ve **templates.json**ve ardından **açık** karşıya yüklemek için JSON dosyaları **şablonları** kapsayıcı. JSON verilerini içeren BLOB URL'ler şunlardır:

    ```csharp
    https://{Your storage account name}.blob.core.windows.net/templates/parameters.json
    https://{Your storage account name}.blob.core.windows.net/templates/template.json
    ```
7. Program.cs'ye aşağıdaki yöntemi ekleyin:

    ```csharp
    static void CreateIoTHub(ResourceManagementClient client)
    {

    }
    ```

8. Aşağıdaki kodu ekleyin **CreateIoTHub** yöntemi için Azure Resource Manager şablonunu ve parametre dosyalarını göndermek için:

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

9. Aşağıdaki kodu ekleyin **CreateIoTHub** durumunu ve anahtarları yeni IOT hub'ı görüntüler yöntemi:

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

Çağırarak artık uygulama tamamlayabilirsiniz **CreateIoTHub** yöntemi önce derleyin ve çalıştırın.

1. Sonuna aşağıdaki kodu ekleyin **ana** yöntemi:

    ```csharp
    CreateIoTHub(client);
    Console.ReadLine();
    ```

2. Tıklayın **derleme** ardından **yapı çözümü**. Hataları düzeltin.

3. Tıklayın **hata ayıklama** ardından **hata ayıklamayı Başlat** uygulamayı çalıştırın. Bu, dağıtımın çalıştırmak birkaç dakika sürebilir.

4. Uygulamanızı yeni IOT hub'ı doğrulamak için ziyaret [Azure portalında] [ lnk-azure-portal] ve kaynakların listesini görüntüleyin. Alternatif olarak, **Get-AzResource** PowerShell cmdlet'i.

> [!NOTE]
> Bu örnek uygulama, bir S1 standart IOT için faturalandırılırsınız Hub ekler. IOT hub'ı aracılığıyla silebilirsiniz [Azure portalında] [ lnk-azure-portal] kullanarak veya **Remove-AzResource** işiniz bittiğinde PowerShell cmdlet'i.

## <a name="next-steps"></a>Sonraki adımlar
Bir C# programı ile bir Azure Resource Manager şablonu kullanarak bir IOT hub'ı dağıttığınız artık daha iyi keşfedilebilmesi isteyebilirsiniz:

* Özellikleri hakkında okuyun [IOT hub'ı kaynak sağlayıcısı REST API'si][lnk-rest-api].
* Okuma [Azure Resource Manager'a genel bakış] [ lnk-azure-rm-overview] Azure Resource Manager'ın özellikleri hakkında daha fazla bilgi edinmek için.
* JSON söz dizimi ve özelliklerini şablonlarında kullanmak üzere bkz [Microsoft.Devices kaynak türleri](/azure/templates/microsoft.devices/iothub-allversions).

İçin IOT Hub ile geliştirme hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [C SDK'ya giriş][lnk-c-sdk]
* [Azure IOT SDK'ları][lnk-sdks]

Daha fazla IOT Hub'ın özelliklerini keşfetmek için bkz:

* [Azure IOT Edge ile sınır cihazlarına Al dağıtma][lnk-iotedge]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-powershell-install]: /powershell/azure/install-Az-ps
[lnk-rest-api]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-azure-rm-overview]: ../azure-resource-manager/resource-group-overview.md
[lnk-storage-account]:../storage/common/storage-create-storage-account.md

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
