---
title: Oluşturma ve Java kullanarak bir Azure sanal makinesi yönetme | Microsoft Docs
description: Bir sanal makine ve tüm destekleyici kaynakları dağıtmak için Java ve Azure Resource Manager'ı kullanın.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: cynthn
ms.openlocfilehash: b02fd8f012dee2436f4f276e05185428008508a1
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67722583"
---
# <a name="create-and-manage-windows-vms-in-azure-using-java"></a>Java kullanarak azure'da Windows Vm'leri oluşturma ve yönetme

Bir [Azure sanal makine](overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) çeşitli destekleyici Azure kaynakları (VM) gerekir. Bu makale, oluşturma, yönetme ve Java kullanarak VM kaynakları silme kapsar. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Bir Maven projesi oluşturun
> * Bağımlılıkları ekleyin
> * Kimlik bilgileri oluşturma
> * Kaynak oluşturma
> * Yönetim görevlerini gerçekleştirme
> * Kaynakları silme
> * Uygulamayı çalıştırma

Bu adımların tamamlanması yaklaşık 20 dakika sürer.

## <a name="create-a-maven-project"></a>Bir Maven projesi oluşturun

1. Zaten yapmadıysanız, yükleme [Java](https://aka.ms/azure-jdks).
2. Yükleme [Maven](https://maven.apache.org/download.cgi).
3. Yeni bir klasör ve projeyi oluşturun:
    
    ```
    mkdir java-azure-test
    cd java-azure-test
    
    mvn archetype:generate -DgroupId=com.fabrikam -DartifactId=testAzureApp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

## <a name="add-dependencies"></a>Bağımlılıkları ekleyin

1. Altında `testAzureApp` açık klasör `pom.xml` dosya ve derleme yapılandırmasına ekleyin &lt;proje&gt; uygulamanızı oluşturmayı etkinleştirmek için:

    ```xml
    <build>
      <plugins>
        <plugin>
            <groupId>org.codehaus.mojo</groupId>
            <artifactId>exec-maven-plugin</artifactId>
            <configuration>
                <mainClass>com.fabrikam.testAzureApp.App</mainClass>
            </configuration>
        </plugin>
      </plugins>
    </build>
    ```

2. Azure Java SDK'sı erişmek için gerekli bağımlılıkları ekleyin.

    ```xml
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure</artifactId>
      <version>1.1.0</version>
    </dependency>
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-mgmt-compute</artifactId>
      <version>1.1.0</version>
    </dependency>
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-mgmt-resources</artifactId>
      <version>1.1.0</version>
    </dependency>
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-mgmt-network</artifactId>
      <version>1.1.0</version>
    </dependency>
    <dependency>
      <groupId>com.squareup.okio</groupId>
      <artifactId>okio</artifactId>
      <version>1.13.0</version>
    </dependency>
    <dependency>
      <groupId>com.nimbusds</groupId>
      <artifactId>nimbus-jose-jwt</artifactId>
      <version>3.6</version>
    </dependency>
    <dependency>
      <groupId>net.minidev</groupId>
      <artifactId>json-smart</artifactId>
      <version>1.0.6.3</version>
    </dependency>
    <dependency>
      <groupId>javax.mail</groupId>
      <artifactId>mail</artifactId>
      <version>1.4.5</version>
    </dependency>
    ```

3. Dosyayı kaydedin.

## <a name="create-credentials"></a>Kimlik bilgileri oluşturma

Bu adım başlamadan önce erişimi olduğundan emin olun bir [Active Directory Hizmet sorumlusu](../../active-directory/develop/howto-create-service-principal-portal.md). Uygulama kimliği, kimlik doğrulama anahtarı ve gereken Kiracı kimliği daha sonraki bir adımda kaydetmelisiniz.

### <a name="create-the-authorization-file"></a>Yetkilendirme dosyası oluşturma

1. Adlı bir dosya oluşturun `azureauth.properties` ve bu özellikleri ekleyin:

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

2. Dosyayı kaydedin.
3. İle kimlik doğrulama dosyasının tam yolu, kabuk AZURE_AUTH_LOCATION adlı bir ortam değişkeni ayarlayın.

### <a name="create-the-management-client"></a>Yönetim istemcisi oluşturma

1. Açık `App.java` altında dosya `src\main\java\com\fabrikam` ve bu paket bildirimi en üstünde olduğundan emin olun:

    ```java
    package com.fabrikam.testAzureApp;
    ```

2. Paket bildirimi altında eklemeniz içeri aktarma deyimleri:
   
    ```java
    import com.microsoft.azure.management.Azure;
    import com.microsoft.azure.management.compute.AvailabilitySet;
    import com.microsoft.azure.management.compute.AvailabilitySetSkuTypes;
    import com.microsoft.azure.management.compute.CachingTypes;
    import com.microsoft.azure.management.compute.InstanceViewStatus;
    import com.microsoft.azure.management.compute.DiskInstanceView;
    import com.microsoft.azure.management.compute.VirtualMachine;
    import com.microsoft.azure.management.compute.VirtualMachineSizeTypes;
    import com.microsoft.azure.management.network.PublicIPAddress;
    import com.microsoft.azure.management.network.Network;
    import com.microsoft.azure.management.network.NetworkInterface;
    import com.microsoft.azure.management.resources.ResourceGroup;
    import com.microsoft.azure.management.resources.fluentcore.arm.Region;
    import com.microsoft.azure.management.resources.fluentcore.model.Creatable;
    import com.microsoft.rest.LogLevel;
    import java.io.File;
    import java.util.Scanner;
    ```

2. İsteğinde bulunmak için gereken Active Directory kimlik bilgilerini oluşturmak için App sınıfının main yöntemi için bu kodu ekleyin:
   
    ```java
    try {
        final File credFile = new File(System.getenv("AZURE_AUTH_LOCATION"));
        Azure azure = Azure.configure()
            .withLogLevel(LogLevel.BASIC)
            .authenticate(credFile)
            .withDefaultSubscription();
    } catch (Exception e) {
        System.out.println(e.getMessage());
        e.printStackTrace();
    }

    ```

## <a name="create-resources"></a>Kaynak oluşturma

### <a name="create-the-resource-group"></a>Kaynak grubunu oluşturma

Tüm kaynaklar içinde bulunması gereken bir [kaynak grubu](../../azure-resource-manager/resource-group-overview.md).

Uygulama için değerleri belirtin ve kaynak grubu oluşturmak için ana yöntem try bloğunda bu kodu ekleyin:

```java
System.out.println("Creating resource group...");
ResourceGroup resourceGroup = azure.resourceGroups()
    .define("myResourceGroup")
    .withRegion(Region.US_EAST)
    .create();
```

### <a name="create-the-availability-set"></a>Kullanılabilirlik kümesi oluşturma

[Kullanılabilirlik kümeleri](tutorial-availability-sets.md) uygulamanız tarafından kullanılan sanal makinelerin bakımını kolaylaştırır.

Kullanılabilirlik kümesi oluşturmak için ana yöntem try bloğunda bu kodu ekleyin:

```java
System.out.println("Creating availability set...");
AvailabilitySet availabilitySet = azure.availabilitySets()
    .define("myAvailabilitySet")
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("myResourceGroup")
    .withSku(AvailabilitySetSkuTypes.MANAGED)
    .create();
```
### <a name="create-the-public-ip-address"></a>Genel IP adresi oluşturma

A [genel IP adresi](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) sanal makineyle iletişim kurmak için gereklidir.

Sanal makine için genel IP adresi oluşturmak için ana yöntem try bloğunda bu kodu ekleyin:

```java
System.out.println("Creating public IP address...");
PublicIPAddress publicIPAddress = azure.publicIPAddresses()
    .define("myPublicIP")
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("myResourceGroup")
    .withDynamicIP()
    .create();
```

### <a name="create-the-virtual-network"></a>Sanal ağ oluşturma

Bir sanal makine bir alt ağda olmalıdır bir [sanal ağ](../../virtual-network/virtual-networks-overview.md).

Bir alt ağ ve sanal ağ oluşturmak isterseniz ana yöntem try bloğunda bu kodu ekleyin:

```java
System.out.println("Creating virtual network...");
Network network = azure.networks()
    .define("myVN")
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("myResourceGroup")
    .withAddressSpace("10.0.0.0/16")
    .withSubnet("mySubnet","10.0.0.0/24")
    .create();
```

### <a name="create-the-network-interface"></a>Ağ arabirimini oluşturun

Bir sanal makinenin sanal ağda iletişim kurabilmek için ağ arabirimi gerekiyor.

Bir ağ arabirimi oluşturmak için ana yöntem try bloğunda bu kodu ekleyin:

```java
System.out.println("Creating network interface...");
NetworkInterface networkInterface = azure.networkInterfaces()
    .define("myNIC")
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("myResourceGroup")
    .withExistingPrimaryNetwork(network)
    .withSubnet("mySubnet")
    .withPrimaryPrivateIPAddressDynamic()
    .withExistingPrimaryPublicIPAddress(publicIPAddress)
    .create();
```

### <a name="create-the-virtual-machine"></a>Sanal makineyi oluşturma

Oluşturduğunuz tüm destekleyici kaynakları, bir sanal makine oluşturabilirsiniz.

Sanal makine oluşturmak için ana yöntem try bloğunda bu kodu ekleyin:

```java
System.out.println("Creating virtual machine...");
VirtualMachine virtualMachine = azure.virtualMachines()
    .define("myVM")
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("myResourceGroup")
    .withExistingPrimaryNetworkInterface(networkInterface)
    .withLatestWindowsImage("MicrosoftWindowsServer", "WindowsServer", "2012-R2-Datacenter")
    .withAdminUsername("azureuser")
    .withAdminPassword("Azure12345678")
    .withComputerName("myVM")
    .withExistingAvailabilitySet(availabilitySet)
    .withSize("Standard_DS1")
    .create();
Scanner input = new Scanner(System.in);
System.out.println("Press enter to get information about the VM...");
input.nextLine();
```

> [!NOTE]
> Bu öğretici, Windows Server işletim sistemi sürümünü çalıştıran bir sanal makine oluşturur. Diğer görüntüleri seçme hakkında daha fazla bilgi için bkz: [Windows PowerShell ve Azure CLI ile Azure sanal makine görüntülerine erişin ve seçin](../linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
> 
>

Var olan bir diski yerine bir Market görüntüsü kullanmak istiyorsanız, bu kodu kullanın: 

```java
ManagedDisk managedDisk = azure.disks.define("myosdisk")
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("myResourceGroup")
    .withWindowsFromVhd("https://mystorage.blob.core.windows.net/vhds/myosdisk.vhd")
    .withSizeInGB(128)
    .withSku(DiskSkuTypes.PremiumLRS)
    .create();

azure.virtualMachines.define("myVM")
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("myResourceGroup")
    .withExistingPrimaryNetworkInterface(networkInterface)
    .withSpecializedOSDisk(managedDisk, OperatingSystemTypes.Windows)
    .withExistingAvailabilitySet(availabilitySet)
    .withSize(VirtualMachineSizeTypes.StandardDS1)
    .create();
```

## <a name="perform-management-tasks"></a>Yönetim görevlerini gerçekleştirme

Bir sanal makinenin yaşam döngüsü boyunca, sanal makineyi başlatmak, durdurmak veya silmek gibi yönetim görevleri gerçekleştirmek isteyebilirsiniz. Ayrıca yinelemeli veya karmaşık görevleri otomatikleştirmek için kod oluşturmak isteyebilirsiniz.

VM ile herhangi bir şey yapmanız gerektiğinde bir örneğini almanız gerekir. Bu kodu ana yöntem için try bloğu ekleyin:

```java
VirtualMachine vm = azure.virtualMachines().getByResourceGroup("myResourceGroup", "myVM");
```

### <a name="get-information-about-the-vm"></a>VM hakkında bilgi edinin

Sanal makine hakkında bilgi almak için ana yöntem try bloğunda bu kodu ekleyin:

```java
System.out.println("hardwareProfile");
System.out.println("    vmSize: " + vm.size());
System.out.println("storageProfile");
System.out.println("  imageReference");
System.out.println("    publisher: " + vm.storageProfile().imageReference().publisher());
System.out.println("    offer: " + vm.storageProfile().imageReference().offer());
System.out.println("    sku: " + vm.storageProfile().imageReference().sku());
System.out.println("    version: " + vm.storageProfile().imageReference().version());
System.out.println("  osDisk");
System.out.println("    osType: " + vm.storageProfile().osDisk().osType());
System.out.println("    name: " + vm.storageProfile().osDisk().name());
System.out.println("    createOption: " + vm.storageProfile().osDisk().createOption());
System.out.println("    caching: " + vm.storageProfile().osDisk().caching());
System.out.println("osProfile");
System.out.println("    computerName: " + vm.osProfile().computerName());
System.out.println("    adminUserName: " + vm.osProfile().adminUsername());
System.out.println("    provisionVMAgent: " + vm.osProfile().windowsConfiguration().provisionVMAgent());
System.out.println("    enableAutomaticUpdates: " + vm.osProfile().windowsConfiguration().enableAutomaticUpdates());
System.out.println("networkProfile");
System.out.println("    networkInterface: " + vm.primaryNetworkInterfaceId());
System.out.println("vmAgent");
System.out.println("  vmAgentVersion: " + vm.instanceView().vmAgent().vmAgentVersion());
System.out.println("    statuses");
for(InstanceViewStatus status : vm.instanceView().vmAgent().statuses()) {
    System.out.println("    code: " + status.code());
    System.out.println("    displayStatus: " + status.displayStatus());
    System.out.println("    message: " + status.message());
    System.out.println("    time: " + status.time());
}
System.out.println("disks");
for(DiskInstanceView disk : vm.instanceView().disks()) {
    System.out.println("  name: " + disk.name());
    System.out.println("  statuses");
    for(InstanceViewStatus status : disk.statuses()) {
        System.out.println("    code: " + status.code());
        System.out.println("    displayStatus: " + status.displayStatus());
        System.out.println("    time: " + status.time());
    }
}
System.out.println("VM general status");
System.out.println("  provisioningStatus: " + vm.provisioningState());
System.out.println("  id: " + vm.id());
System.out.println("  name: " + vm.name());
System.out.println("  type: " + vm.type());
System.out.println("VM instance status");
for(InstanceViewStatus status : vm.instanceView().statuses()) {
    System.out.println("  code: " + status.code());
    System.out.println("  displayStatus: " + status.displayStatus());
}
System.out.println("Press enter to continue...");
input.nextLine();
```

### <a name="stop-the-vm"></a>VM’yi durdurma

Sanal makineyi durdurma ve tüm ayarlarını koruyabilirsiniz ancak için ücretlendirilmeye devam ya da sanal makineyi durdurma ve bunu serbest bırakın. Bir sanal makine serbest bırakıldığında onunla ilişkili tüm kaynakları serbest ve faturalandırma uçları için ayrıca olur.

Serbest bırakılıyor olmadan sanal makineyi durdurmak için ana yöntem try bloğunda bu kodu ekleyin:

```java
System.out.println("Stopping vm...");
vm.powerOff();
System.out.println("Press enter to continue...");
input.nextLine();
```

Sanal makineyi serbest bırakmak isterseniz bu kod kopyalanabilmesi çağrısını değiştirin:

```java
vm.deallocate();
```

### <a name="start-the-vm"></a>VM’yi başlatma

Sanal makineyi başlatmak için ana yöntem try bloğunda bu kodu ekleyin:

```java
System.out.println("Starting vm...");
vm.start();
System.out.println("Press enter to continue...");
input.nextLine();
```

### <a name="resize-the-vm"></a>VM'yi yeniden boyutlandırın

Birçok yönden dağıtımının sanal makineniz için bir boyutuna karar verirken dikkate alınmalıdır. Daha fazla bilgi için [VM boyutları](sizes.md).  

Sanal makinenin boyutunu değiştirmek için ana yöntem try bloğunda bu kodu ekleyin:

```java
System.out.println("Resizing vm...");
vm.update()
    .withSize(VirtualMachineSizeTypes.STANDARD_DS2)
    .apply();
System.out.println("Press enter to continue...");
input.nextLine();
```

### <a name="add-a-data-disk-to-the-vm"></a>VM'ye veri diski ekleme

Boyutu 2 GB ise, bir LUN 0 ve ReadWrite önbelleğe alma türü olan sanal makineye veri diski eklemek için ana yöntem try bloğunda bu kodu ekleyin:

```java
System.out.println("Adding data disk...");
vm.update()
    .withNewDataDisk(2, 0, CachingTypes.READ_WRITE)
    .apply();
System.out.println("Press enter to delete resources...");
input.nextLine();
```

## <a name="delete-resources"></a>Kaynakları silme

Azure'da kullanılan kaynaklar için ücretlendirilirsiniz, her zaman artık gerekli olmayan kaynakları silmek için iyi bir uygulama olmasıdır. Sanal makineleri ve tüm destekleyici kaynakları silmek isterseniz, tek yapmanız gereken olan kaynak grubunu silin.

1. Kaynak grubunu silmek için ana yöntem try bloğunda bu kodu ekleyin:
   
    ```java
    System.out.println("Deleting resources...");
    azure.resourceGroups().deleteByName("myResourceGroup");
    ```

2. App.java dosyasını kaydedin.

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Bu son tamamlanması tamamen başından çalıştırmak bu konsol uygulamasını yaklaşık beş dakika sürer.

1. Uygulamayı çalıştırmak için bu Maven komutunu kullanın:

    ```
    mvn compile exec:java
    ```

2. Basmadan önce **Enter** kaynakları silme başlatmak için Azure portalında kaynaklarının oluşturulmasını doğrulamak için birkaç dakika sürebilir. Dağıtım durumu, dağıtım hakkında bilgi için tıklayın.


## <a name="next-steps"></a>Sonraki adımlar
* Kullanma hakkında daha fazla bilgi edinin [Java için Azure kitaplıkları](https://docs.microsoft.com/java/azure/java-sdk-azure-overview).

