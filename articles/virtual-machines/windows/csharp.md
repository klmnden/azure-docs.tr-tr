---
title: Oluşturma ve C# kullanarak Azure sanal makinesi yönetme | Microsoft Docs
description: Bir sanal makine ve tüm destekleyici kaynakları dağıtmak için C# ve Azure Resource Manager'ı kullanın.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: 87524373-5f52-4f4b-94af-50bf7b65c277
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: cynthn
ms.openlocfilehash: ce05d097aa69aa1aadb8450e40722448bc5a7de0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61402050"
---
# <a name="create-and-manage-windows-vms-in-azure-using-c"></a>C# kullanarak azure'da Windows Vm'leri oluşturma ve yönetme #

Bir [Azure sanal makine](overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) çeşitli destekleyici Azure kaynakları (VM) gerekir. Bu makale, oluşturma, yönetme ve C# kullanarak VM kaynakları silme kapsar. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Visual Studio projesi oluşturma
> * Paketi yükleyin
> * Kimlik bilgileri oluşturma
> * Kaynak oluşturma
> * Yönetim görevlerini gerçekleştirme
> * Kaynakları silme
> * Uygulamayı çalıştırma

Bu adımların tamamlanması yaklaşık 20 dakika sürer.

## <a name="create-a-visual-studio-project"></a>Visual Studio projesi oluşturma

1. Henüz yapmadıysanız, yükleme [Visual Studio](https://docs.microsoft.com/visualstudio/install/install-visual-studio). Seçin **.NET masaüstü geliştirme** iş yükleri sayfası ve ardından **yükleme**. Özet olarak, gördüğünüz gibi **.NET Framework 4-4.6 geliştirme araçları** sizin için otomatik olarak seçilir. Visual Studio zaten yüklediyseniz, Visual Studio Başlatıcısı'nı kullanarak .NET iş yükü ekleyebilirsiniz.
2. Visual Studio’da, **Dosya** > **Yeni** > **Proje**’ye tıklayın.
3. İçinde **şablonları** > **Visual C#** seçin **konsol uygulaması (.NET Framework)**, girin *myDotnetProject* adı Proje, projenin konumunu seçin ve ardından **Tamam**.

## <a name="install-the-package"></a>Paketi yükleyin

NuGet paketlerini bu adımları tamamlamak için gereken kitaplıklarını yüklemek için en kolay yoludur. Visual Studio'da ihtiyacınız kitaplıkları edinmek için şu adımları uygulayın:

1. Tıklayın **Araçları** > **Nuget Paket Yöneticisi**ve ardından **Paket Yöneticisi Konsolu**.
2. Konsolunda aşağıdaki komutu yazın:

    ```
    Install-Package Microsoft.Azure.Management.Fluent
    ```

## <a name="create-credentials"></a>Kimlik bilgileri oluşturma

Bu adım başlamadan önce erişimi olduğundan emin olun bir [Active Directory Hizmet sorumlusu](../../active-directory/develop/howto-create-service-principal-portal.md). Uygulama kimliği, kimlik doğrulama anahtarı ve gereken Kiracı kimliği daha sonraki bir adımda kaydetmelisiniz.

### <a name="create-the-authorization-file"></a>Yetkilendirme dosyası oluşturma

1. Çözüm Gezgini'nde sağ *myDotnetProject* > **Ekle** > **yeni öğe**ve ardından **metindosyası** içinde *Visual C# öğeleri*. Dosya adı *azureauth.properties*ve ardından **Ekle**.
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

    Değiştirin **&lt;subscrıptıon-ID&gt;** , abonelik tanımlayıcısı ile **&lt;uygulama-kimliği&gt;** ile Active Directory uygulaması tanımlayıcı, **&lt;kimlik doğrulama anahtarı&gt;** uygulama anahtarına sahip ve **&lt;Kiracı-kimliği&gt;** Kiracı tanımlayıcısı ile.

3. Azureauth.properties dosyayı kaydedin. 
4. Windows, oluşturduğunuz yetkilendirme dosyasının tam yolu AZURE_AUTH_LOCATION adında bir ortam değişkeni ayarlayın. Örneğin, aşağıdaki PowerShell komutu kullanılabilir:

    ```
    [Environment]::SetEnvironmentVariable("AZURE_AUTH_LOCATION", "C:\Visual Studio 2017\Projects\myDotnetProject\myDotnetProject\azureauth.properties", "User")
    ```

### <a name="create-the-management-client"></a>Yönetim istemcisi oluşturma

1. Oluşturduğunuz proje için Program.cs dosyasını açın ve ardından bu dosyasının en üstüne using deyimlerini mevcut deyimlerini ekleyin:

    ```
    using Microsoft.Azure.Management.Compute.Fluent;
    using Microsoft.Azure.Management.Compute.Fluent.Models;
    using Microsoft.Azure.Management.Fluent;
    using Microsoft.Azure.Management.ResourceManager.Fluent;
    using Microsoft.Azure.Management.ResourceManager.Fluent.Core;
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

## <a name="create-resources"></a>Kaynak oluşturma

### <a name="create-the-resource-group"></a>Kaynak grubunu oluşturma

Tüm kaynaklar içinde bulunması gereken bir [kaynak grubu](../../azure-resource-manager/resource-group-overview.md).

Uygulama için değerleri belirtin ve kaynak grubu oluşturmak için bu kodu Main yöntemine ekleyin:

```
var groupName = "myResourceGroup";
var vmName = "myVM";
var location = Region.USWest;
    
Console.WriteLine("Creating resource group...");
var resourceGroup = azure.ResourceGroups.Define(groupName)
    .WithRegion(location)
    .Create();
```

### <a name="create-the-availability-set"></a>Kullanılabilirlik kümesi oluşturma

[Kullanılabilirlik kümeleri](tutorial-availability-sets.md) uygulamanız tarafından kullanılan sanal makinelerin bakımını kolaylaştırır.

Kullanılabilirlik kümesi oluşturmak için bu kodu Main yöntemine ekleyin:

```
Console.WriteLine("Creating availability set...");
var availabilitySet = azure.AvailabilitySets.Define("myAVSet")
    .WithRegion(location)
    .WithExistingResourceGroup(groupName)
    .WithSku(AvailabilitySetSkuTypes.Managed)
    .Create();
```

### <a name="create-the-public-ip-address"></a>Genel IP adresi oluşturma

A [genel IP adresi](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) sanal makineyle iletişim kurmak için gereklidir.

Sanal makine için genel IP adresi oluşturmak için bu kodu Main yöntemine ekleyin:
   
```
Console.WriteLine("Creating public IP address...");
var publicIPAddress = azure.PublicIPAddresses.Define("myPublicIP")
    .WithRegion(location)
    .WithExistingResourceGroup(groupName)
    .WithDynamicIP()
    .Create();
```

### <a name="create-the-virtual-network"></a>Sanal ağ oluşturma

Bir sanal makine bir alt ağda olmalıdır bir [sanal ağ](../../virtual-network/virtual-networks-overview.md).

Bir alt ağ ve sanal ağ oluşturmak isterseniz bu kod Main yöntemine ekleyin:

```
Console.WriteLine("Creating virtual network...");
var network = azure.Networks.Define("myVNet")
    .WithRegion(location)
    .WithExistingResourceGroup(groupName)
    .WithAddressSpace("10.0.0.0/16")
    .WithSubnet("mySubnet", "10.0.0.0/24")
    .Create();
```

### <a name="create-the-network-interface"></a>Ağ arabirimini oluşturun

Bir sanal makinenin sanal ağda iletişim kurabilmek için ağ arabirimi gerekiyor.

Bir ağ arabirimi oluşturmak için bu kodu Main yöntemine ekleyin:

```
Console.WriteLine("Creating network interface...");
var networkInterface = azure.NetworkInterfaces.Define("myNIC")
    .WithRegion(location)
    .WithExistingResourceGroup(groupName)
    .WithExistingPrimaryNetwork(network)
    .WithSubnet("mySubnet")
    .WithPrimaryPrivateIPAddressDynamic()
    .WithExistingPrimaryPublicIPAddress(publicIPAddress)
    .Create();
 ```

### <a name="create-the-virtual-machine"></a>Sanal makineyi oluşturma

Oluşturduğunuz tüm destekleyici kaynakları, bir sanal makine oluşturabilirsiniz.

Sanal makine oluşturmak için bu kodu Main yöntemine ekleyin:

```
Console.WriteLine("Creating virtual machine...");
azure.VirtualMachines.Define(vmName)
    .WithRegion(location)
    .WithExistingResourceGroup(groupName)
    .WithExistingPrimaryNetworkInterface(networkInterface)
    .WithLatestWindowsImage("MicrosoftWindowsServer", "WindowsServer", "2012-R2-Datacenter")
    .WithAdminUsername("azureuser")
    .WithAdminPassword("Azure12345678")
    .WithComputerName(vmName)
    .WithExistingAvailabilitySet(availabilitySet)
    .WithSize(VirtualMachineSizeTypes.StandardDS1)
    .Create();
```

> [!NOTE]
> Bu öğretici, Windows Server işletim sistemi sürümünü çalıştıran bir sanal makine oluşturur. Diğer görüntüleri seçme hakkında daha fazla bilgi için bkz: [Windows PowerShell ve Azure CLI ile Azure sanal makine görüntülerine erişin ve seçin](../linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
> 
>

Var olan bir diski yerine bir Market görüntüsü kullanmak istiyorsanız, bu kodu kullanın:

```
var managedDisk = azure.Disks.Define("myosdisk")
    .WithRegion(location)
    .WithExistingResourceGroup(groupName)
    .WithWindowsFromVhd("https://mystorage.blob.core.windows.net/vhds/myosdisk.vhd")
    .WithSizeInGB(128)
    .WithSku(DiskSkuTypes.PremiumLRS)
    .Create();

azure.VirtualMachines.Define("myVM")
    .WithRegion(location)
    .WithExistingResourceGroup(groupName)
    .WithExistingPrimaryNetworkInterface(networkInterface)
    .WithSpecializedOSDisk(managedDisk, OperatingSystemTypes.Windows)
    .WithExistingAvailabilitySet(availabilitySet)
    .WithSize(VirtualMachineSizeTypes.StandardDS1)
    .Create();
```

## <a name="perform-management-tasks"></a>Yönetim görevlerini gerçekleştirme

Bir sanal makinenin yaşam döngüsü boyunca, sanal makineyi başlatmak, durdurmak veya silmek gibi yönetim görevleri gerçekleştirmek isteyebilirsiniz. Ayrıca yinelemeli veya karmaşık görevleri otomatikleştirmek için kod oluşturmak isteyebilirsiniz.

VM ile herhangi bir şey yapmanız gerektiğinde bir örneğini almanız gerekir:

```
var vm = azure.VirtualMachines.GetByResourceGroup(groupName, vmName);
```

### <a name="get-information-about-the-vm"></a>VM hakkında bilgi edinin

Sanal makine hakkında bilgi almak için bu kodu Main yöntemine ekleyin:

```
Console.WriteLine("Getting information about the virtual machine...");
Console.WriteLine("hardwareProfile");
Console.WriteLine("   vmSize: " + vm.Size);
Console.WriteLine("storageProfile");
Console.WriteLine("  imageReference");
Console.WriteLine("    publisher: " + vm.StorageProfile.ImageReference.Publisher);
Console.WriteLine("    offer: " + vm.StorageProfile.ImageReference.Offer);
Console.WriteLine("    sku: " + vm.StorageProfile.ImageReference.Sku);
Console.WriteLine("    version: " + vm.StorageProfile.ImageReference.Version);
Console.WriteLine("  osDisk");
Console.WriteLine("    osType: " + vm.StorageProfile.OsDisk.OsType);
Console.WriteLine("    name: " + vm.StorageProfile.OsDisk.Name);
Console.WriteLine("    createOption: " + vm.StorageProfile.OsDisk.CreateOption);
Console.WriteLine("    caching: " + vm.StorageProfile.OsDisk.Caching);
Console.WriteLine("osProfile");
Console.WriteLine("  computerName: " + vm.OSProfile.ComputerName);
Console.WriteLine("  adminUsername: " + vm.OSProfile.AdminUsername);
Console.WriteLine("  provisionVMAgent: " + vm.OSProfile.WindowsConfiguration.ProvisionVMAgent.Value);
Console.WriteLine("  enableAutomaticUpdates: " + vm.OSProfile.WindowsConfiguration.EnableAutomaticUpdates.Value);
Console.WriteLine("networkProfile");
foreach (string nicId in vm.NetworkInterfaceIds)
{
    Console.WriteLine("  networkInterface id: " + nicId);
}
Console.WriteLine("vmAgent");
Console.WriteLine("  vmAgentVersion" + vm.InstanceView.VmAgent.VmAgentVersion);
Console.WriteLine("    statuses");
foreach (InstanceViewStatus stat in vm.InstanceView.VmAgent.Statuses)
{
    Console.WriteLine("    code: " + stat.Code);
    Console.WriteLine("    level: " + stat.Level);
    Console.WriteLine("    displayStatus: " + stat.DisplayStatus);
    Console.WriteLine("    message: " + stat.Message);
    Console.WriteLine("    time: " + stat.Time);
}
Console.WriteLine("disks");
foreach (DiskInstanceView disk in vm.InstanceView.Disks)
{
    Console.WriteLine("  name: " + disk.Name);
    Console.WriteLine("  statuses");
    foreach (InstanceViewStatus stat in disk.Statuses)
    {
        Console.WriteLine("    code: " + stat.Code);
        Console.WriteLine("    level: " + stat.Level);
        Console.WriteLine("    displayStatus: " + stat.DisplayStatus);
        Console.WriteLine("    time: " + stat.Time);
    }
}
Console.WriteLine("VM general status");
Console.WriteLine("  provisioningStatus: " + vm.ProvisioningState);
Console.WriteLine("  id: " + vm.Id);
Console.WriteLine("  name: " + vm.Name);
Console.WriteLine("  type: " + vm.Type);
Console.WriteLine("  location: " + vm.Region);
Console.WriteLine("VM instance status");
foreach (InstanceViewStatus stat in vm.InstanceView.Statuses)
{
    Console.WriteLine("  code: " + stat.Code);
    Console.WriteLine("  level: " + stat.Level);
    Console.WriteLine("  displayStatus: " + stat.DisplayStatus);
}
Console.WriteLine("Press enter to continue...");
Console.ReadLine();
```

### <a name="stop-the-vm"></a>VM’yi durdurma

Sanal makineyi durdurma ve tüm ayarlarını koruyabilirsiniz ancak için ücretlendirilmeye devam ya da sanal makineyi durdurma ve bunu serbest bırakın. Bir sanal makine serbest bırakıldığında onunla ilişkili tüm kaynakları serbest ve faturalandırma uçları için ayrıca olur.

Serbest bırakılıyor olmadan sanal makineyi durdurmak için bu kodu Main yöntemine ekleyin:

```
Console.WriteLine("Stopping vm...");
vm.PowerOff();
Console.WriteLine("Press enter to continue...");
Console.ReadLine();
```

Sanal makineyi serbest bırakmak isterseniz bu kod kopyalanabilmesi çağrısını değiştirin:

```
vm.Deallocate();
```

### <a name="start-the-vm"></a>VM’yi başlatma

Sanal makineyi başlatmak için bu kodu Main yöntemine ekleyin:

```
Console.WriteLine("Starting vm...");
vm.Start();
Console.WriteLine("Press enter to continue...");
Console.ReadLine();
```

### <a name="resize-the-vm"></a>VM'yi yeniden boyutlandırın

Birçok yönden dağıtımının sanal makineniz için bir boyutuna karar verirken dikkate alınmalıdır. Daha fazla bilgi için [VM boyutları](sizes.md).  

Sanal makinenin boyutunu değiştirmek için bu kodu Main yöntemine ekleyin:

```
Console.WriteLine("Resizing vm...");
vm.Update()
    .WithSize(VirtualMachineSizeTypes.StandardDS2) 
    .Apply();
Console.WriteLine("Press enter to continue...");
Console.ReadLine();
```

### <a name="add-a-data-disk-to-the-vm"></a>VM'ye veri diski ekleme

Sanal makineye veri diski eklemek için han bir LUN 0 ReadWrite önbelleğe alma türü ve boyutu 2 GB olan bir veri diski eklemek için Main yöntemi için bu kodu ekleyin:

```
Console.WriteLine("Adding data disk to vm...");
vm.Update()
    .WithNewDataDisk(2, 0, CachingTypes.ReadWrite) 
    .Apply();
Console.WriteLine("Press enter to delete resources...");
Console.ReadLine();
```

## <a name="delete-resources"></a>Kaynakları silme

Azure'da kullanılan kaynaklar için ücretlendirilirsiniz, her zaman artık gerekli olmayan kaynakları silmek için iyi bir uygulama olmasıdır. Sanal makineleri ve tüm destekleyici kaynakları silmek isterseniz, tek yapmanız gereken olan kaynak grubunu silin.

Kaynak grubunu silmek için bu kodu Main yöntemine ekleyin:

```
azure.ResourceGroups.DeleteByName(groupName);
```

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Bu son tamamlanması tamamen başından çalıştırmak bu konsol uygulamasını yaklaşık beş dakika sürer. 

1. Konsol uygulamasını çalıştırmak için tıklayın **Başlat**.

2. Basmadan önce **Enter** kaynakları silme başlatmak için Azure portalında kaynaklarının oluşturulmasını doğrulamak için birkaç dakika sürebilir. Dağıtım durumu, dağıtım hakkında bilgi için tıklayın.

## <a name="next-steps"></a>Sonraki adımlar
* Bilgileri kullanarak bir sanal makine oluşturmak için bir şablon kullanma avantajından yararlanmak [C# ve Resource Manager şablonu kullanarak bir Azure sanal makine dağıtma](csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Kullanma hakkında daha fazla bilgi edinin [.NET için Azure kitaplıkları](https://docs.microsoft.com/dotnet/azure/?view=azure-dotnet).

