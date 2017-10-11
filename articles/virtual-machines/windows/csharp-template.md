---
title: "C# ve Resource Manager şablonu kullanarak bir VM'i dağıtma | Microsoft Docs"
description: "Bir Azure VM dağıtmak için C# ve Resource Manager şablonu kullanmak için öğrenin."
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: bfba66e8-c923-4df2-900a-0c2643b81240
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: davidmu
ms.openlocfilehash: bd1c860db026f948202cd1f3aa763b4547c597b4
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="deploy-an-azure-virtual-machine-using-c-and-a-resource-manager-template"></a>C# ve Resource Manager şablonu kullanarak bir Azure sanal makine dağıtma
Bu makalede, C# kullanarak Azure Resource Manager şablonu dağıtma gösterilmektedir. Oluşturduğunuz şablon, tek bir alt ağ ile yeni bir sanal ağ içinde Windows Server çalıştıran tek bir sanal makine dağıtır.

Sanal makine kaynağı ayrıntılı bir açıklaması için bkz: [sanal makineleri bir Azure Resource Manager şablonunda](template-description.md). Bir şablona tüm kaynaklar hakkında daha fazla bilgi için bkz: [Azure Resource Manager şablonu Kılavuzu](../../azure-resource-manager/resource-manager-template-walkthrough.md).

Bu adımları yapmak için yaklaşık 10 dakika sürer.

## <a name="create-a-visual-studio-project"></a>Visual Studio projesi oluşturma

Bu adımda, Visual Studio yüklenmiş ve şablonu dağıtmak için kullanılan bir konsol uygulaması oluşturun emin olun.

1. Henüz yapmadıysanız, yükleme [Visual Studio](https://docs.microsoft.com/visualstudio/install/install-visual-studio). Seçin **.NET masaüstü geliştirme** iş yükleri sayfa ve ardından **yükleme**. Özet olarak, gördüğünüz **.NET Framework 4-4.6 geliştirme araçları** sizin için otomatik olarak seçilir. Visual Studio'nun zaten yüklediyseniz, Visual Studio Başlatıcısı'nı kullanarak .NET iş yükü ekleyebilirsiniz.
2. Visual Studio'da sırasıyla **dosya** > **yeni** > **proje**.
3. İçinde **şablonları** > **Visual C#**seçin **konsol uygulaması (.NET Framework)**, girin *myDotnetProject* projesinin adı, proje konumunu seçin ve ardından **Tamam**.

## <a name="install-the-packages"></a>Paket yüklemek için

NuGet paketleri, bu adımları tamamlamak için gereken kitaplıklar yüklemek için kolay bir yoludur. Visual Studio'da ihtiyacınız kitaplıkları almak için bu adımları uygulayın:

1. Tıklatın **Araçları** > **Nuget Paket Yöneticisi**ve ardından **Paket Yöneticisi Konsolu**.
2. Konsolunda aşağıdaki komutları yazın:

    ```
    Install-Package Microsoft.Azure.Management.Fluent
    Install-Package WindowsAzure.Storage
    ```

## <a name="create-the-files"></a>Dosyaları oluşturma

Bu adımda, kaynakları dağıtan bir şablon dosyası ve şablon parametre değerlerini sağlayan bir parametre dosyası oluşturun. Ayrıca Azure Resource Manager işlemlerini gerçekleştirmek için kullanılan bir yetkilendirme dosyasını oluşturursunuz.

### <a name="create-the-template-file"></a>Şablon dosyası oluşturma

1. Çözüm Gezgini'nde sağ *myDotnetProject* > **Ekle** > **yeni öğe**ve ardından **metin dosyası** içinde *Visual C# öğeleri*. Dosya adı *CreateVMTemplate.json*ve ardından **Ekle**.
2. Bu JSON kodunu oluşturduğunuz dosyaya ekleyin:

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "adminUsername": { "type": "string" },
        "adminPassword": { "type": "securestring" }
      },
      "variables": {
        "vnetID": "[resourceId('Microsoft.Network/virtualNetworks','myVNet')]", 
        "subnetRef": "[concat(variables('vnetID'),'/subnets/mySubnet')]", 
      },
      "resources": [
        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/publicIPAddresses",
          "name": "myPublicIPAddress",
          "location": "[resourceGroup().location]",
          "properties": {
            "publicIPAllocationMethod": "Dynamic",
            "dnsSettings": {
              "domainNameLabel": "myresourcegroupdns1"
            }
          }
        },
        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/virtualNetworks",
          "name": "myVNet",
          "location": "[resourceGroup().location]",
          "properties": {
            "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
            "subnets": [
              {
                "name": "mySubnet",
                "properties": { "addressPrefix": "10.0.0.0/24" }
              }
            ]
          }
        },
        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/networkInterfaces",
          "name": "myNic",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[resourceId('Microsoft.Network/publicIPAddresses/', 'myPublicIPAddress')]",
            "[resourceId('Microsoft.Network/virtualNetworks/', 'myVNet')]"
          ],
          "properties": {
            "ipConfigurations": [
              {
                "name": "ipconfig1",
                "properties": {
                  "privateIPAllocationMethod": "Dynamic",
                  "publicIPAddress": { "id": "[resourceId('Microsoft.Network/publicIPAddresses','myPublicIPAddress')]" },
                  "subnet": { "id": "[variables('subnetRef')]" }
                }
              }
            ]
          }
        },
        {
          "apiVersion": "2016-04-30-preview",
          "type": "Microsoft.Compute/virtualMachines",
          "name": "myVM",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[resourceId('Microsoft.Network/networkInterfaces/', 'myNic')]"
          ],
          "properties": {
            "hardwareProfile": { "vmSize": "Standard_DS1" },
            "osProfile": {
              "computerName": "myVM",
              "adminUsername": "[parameters('adminUsername')]",
              "adminPassword": "[parameters('adminPassword')]"
            },
            "storageProfile": {
              "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "2012-R2-Datacenter",
                "version": "latest"
              },
              "osDisk": {
                "name": "myManagedOSDisk",
                "caching": "ReadWrite",
                "createOption": "FromImage"
              }
            },
            "networkProfile": {
              "networkInterfaces": [
                {
                  "id": "[resourceId('Microsoft.Network/networkInterfaces','myNic')]"
                }
              ]
            }
          }
        }
      ]
    }
    ```

3. CreateVMTemplate.json dosyasını kaydedin.

### <a name="create-the-parameters-file"></a>Parametreler dosyası oluşturma

Şablonda tanımlı kaynak parametreleri için değerleri belirtmek için değerleri içeren bir parametre dosyası oluşturun.

1. Çözüm Gezgini'nde sağ *myDotnetProject* > **Ekle** > **yeni öğe**ve ardından **metin dosyası** içinde *Visual C# öğeleri*. Dosya adı *Parameters.json*ve ardından **Ekle**.
2. Bu JSON kodunu oluşturduğunuz dosyaya ekleyin:

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "adminUserName": { "value": "azureuser" },
        "adminPassword": { "value": "Azure12345678" }
      }
    }
    ```

4. Parameters.json dosyasını kaydedin.

### <a name="create-the-authorization-file"></a>Yetkilendirme dosyası oluşturma

Bir şablonu dağıtmadan önce erişimi olduğundan emin olun bir [Active Directory hizmet asıl](../../resource-group-authenticate-service-principal.md). Hizmet sorumlusu istekleri için Azure Resource Manager kimlik doğrulaması için belirteç alın. Uygulama kimliği, kimlik doğrulama anahtarı ve yetkilendirme dosyasında gereken Kiracı kimliği de kaydetmeniz gerekir.

1. Çözüm Gezgini'nde sağ *myDotnetProject* > **Ekle** > **yeni öğe**ve ardından **metin dosyası** içinde *Visual C# öğeleri*. Dosya adı *azureauth.properties*ve ardından **Ekle**.
2. Bu yetkilendirme özellikleri ekleyin:

    ```
    subscription=<subscription-id>
    client=<application-id>
    key=<authentication-key>
    tenant=<tenant-id>
    managementURI=https://management.core.windows.net/
    baseURL=https://management.azure.com/
    authURL=https://login.windows.net/
    graphURL=https://graph.windows.net/
    ```

    Değiştir  **&lt;abonelik kimliği&gt;**  , abonelik tanımlayıcısı ile  **&lt;uygulama kimliği&gt;**  Active Directory Uygulama tanımlayıcısı ile  **&lt;kimlik doğrulama anahtarı&gt;**  uygulama anahtarla ve  **&lt;Kiracı kimliği&gt;**  , Kiracı tanımlayıcısı.

3. Azureauth.properties dosyasını kaydedin.
4. Windows, oluşturduğunuz örneğin komut kullanılabilir aşağıdaki PowerShell yetkilendirme dosyasının tam yolunu AZURE_AUTH_LOCATION adında bir ortam değişkeni ayarlayın:

    ```
    [Environment]::SetEnvironmentVariable("AZURE_AUTH_LOCATION", "C:\Visual Studio 2017\Projects\myDotnetProject\myDotnetProject\azureauth.properties", "User")
    ```
    
## <a name="create-the-management-client"></a>Yönetim istemcisi oluşturma

1. Oluşturduğunuz proje için Program.cs dosyasını açın ve bu dosyanın üst kısmında using deyimleri varolan deyimleri ekleyin:

    ```
    using Microsoft.Azure.Management.Compute.Fluent;
    using Microsoft.Azure.Management.Compute.Fluent.Models;
    using Microsoft.Azure.Management.Fluent;
    using Microsoft.Azure.Management.ResourceManager.Fluent;
    using Microsoft.Azure.Management.ResourceManager.Fluent.Core;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```

2. Yönetim istemcisi oluşturmak için bu kodu Main yöntemine ekleyin:

    ```
    var credentials = SdkContext.AzureCredentialsFactory
        .FromFile(Environment.GetEnvironmentVariable("AZURE_AUTH_LOCATION"));

    var azure = Azure
        .Configure()
        .WithLogLevel(HttpLoggingDelegatingHandler.Level.Basic)
        .Authenticate(credentials)
        .WithDefaultSubscription();
    ```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Uygulama için değerleri belirtmek için kodu Main yöntemine ekleyin:

```
var groupName = "myResourceGroup";
var location = Region.USWest;

var resourceGroup = azure.ResourceGroups.Define(groupName)
    .WithRegion(location)
    .Create();
```

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Azure depolama hesabından şablonu ve parametre dağıtılır. Bu adımda, hesabı oluşturun ve dosyaları karşıya yükleme. 

Hesabı oluşturmak için bu kodu Main yöntemine ekleyin:

```
string storageAccountName = SdkContext.RandomResourceName("st", 10);

Console.WriteLine("Creating storage account...");
var storage = azure.StorageAccounts.Define(storageAccountName)
    .WithRegion(Region.USWest)
    .WithExistingResourceGroup(resourceGroup)
    .Create();

var storageKeys = storage.GetKeys();
string storageConnectionString = "DefaultEndpointsProtocol=https;"
    + "AccountName=" + storage.Name
    + ";AccountKey=" + storageKeys[0].Value
    + ";EndpointSuffix=core.windows.net";

var account = CloudStorageAccount.Parse(storageConnectionString);
var serviceClient = account.CreateCloudBlobClient();

Console.WriteLine("Creating container...");
var container = serviceClient.GetContainerReference("templates");
container.CreateIfNotExistsAsync().Wait();
var containerPermissions = new BlobContainerPermissions()
    { PublicAccess = BlobContainerPublicAccessType.Container };
container.SetPermissionsAsync(containerPermissions).Wait();

Console.WriteLine("Uploading template file...");
var templateblob = container.GetBlockBlobReference("CreateVMTemplate.json");
templateblob.UploadFromFile("..\\..\\CreateVMTemplate.json");

Console.WriteLine("Uploading parameters file...");
var paramblob = container.GetBlockBlobReference("Parameters.json");
paramblob.UploadFromFile("..\\..\\Parameters.json");
```

## <a name="deploy-the-template"></a>Şablonu dağıtma

Şablon ve oluşturulan depolama hesabı parametrelerinden dağıtın. 

Şablonu dağıtmak için bu kodu Main yöntemine ekleyin:

```
var templatePath = "https://" + storageAccountName + ".blob.core.windows.net/templates/CreateVMTemplate.json";
var paramPath = "https://" + storageAccountName + ".blob.core.windows.net/templates/Parameters.json";
var deployment = azure.Deployments.Define("myDeployment")
    .WithExistingResourceGroup(groupName)
    .WithTemplateLink(templatePath, "1.0.0.0")
    .WithParametersLink(paramPath, "1.0.0.0")
    .WithMode(Microsoft.Azure.Management.ResourceManager.Fluent.Models.DeploymentMode.Incremental)
    .Create();
Console.WriteLine("Press enter to delete the resource group...");
Console.ReadLine();
```

## <a name="delete-the-resources"></a>Kaynakları silme

Azure'da kullanılan kaynaklar için ücretlendirildiğinizden, her zaman artık gerekli olmayan kaynakları silmek için iyi bir uygulamadır. Her kaynak ayrı ayrı bir kaynak grubundan silmeniz gerekmez. Kaynak grubunu silin ve tüm kaynakları otomatik olarak silinir. 

Kaynak grubunu silmek için bu kodu Main yöntemine ekleyin:

```
azure.ResourceGroups.DeleteByName(groupName);
```

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Tamamlamak için bu konsol uygulamasını tamamen çalıştırın yaklaşık beş dakika sürer. 

1. Konsol uygulamasını çalıştırmak için tıklatın **Başlat**.

2. Tuşuna önce **Enter** kaynakları silme başlatmak için Azure portalında kaynakların oluşturulmasını doğrulamak için birkaç dakika sürebilir. Dağıtım hakkında bilgi için dağıtım durumunu tıklatın.

## <a name="next-steps"></a>Sonraki adımlar
* Dağıtım ile ilgili sorunlar varsa, bir sonraki adım bakmak için olacaktır [ortak Azure dağıtım hataları Azure Resource Manager ile ilgili sorunları giderme](../../resource-manager-common-deployment-errors.md).
* Bir sanal makine ve destekleyici kaynaklarıyla gözden geçirerek dağıtmayı öğrenin [bir Azure sanal makine kullanarak C# dağıtmak](csharp.md).
