---
title: "Oluşturma ve Java kullanarak Azure sanal makinesi yönetme | Microsoft Docs"
description: "Bir sanal makine ve tüm destekleyici kaynakları dağıtmak için Java ve Azure Resource Manager'ı kullanın."
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: davidmu
ms.openlocfilehash: b9e739a07c5863577285fb3a221b372b385c6762
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="create-and-manage-windows-vms-in-azure-using-java"></a>Oluşturma ve Java kullanarak azure'da Windows sanal makineleri yönetme

Bir [Azure sanal makine](overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (VM) çeşitli destekleyici Azure kaynakları gerekir. Bu makalede, oluşturma, yönetme ve Java kullanarak VM kaynakları silme yer almaktadır. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Bir Maven projesi oluşturun
> * Bağımlılıkları ekleyin.
> * Kimlik bilgileri oluşturun
> * Kaynak oluşturma
> * Yönetim görevlerini gerçekleştirme
> * Kaynakları silin
> * Uygulamayı çalıştırma

Bu adımların tamamlanması yaklaşık 20 dakika sürer.

## <a name="create-a-maven-project"></a>Bir Maven projesi oluşturun

1. Zaten yapmadıysanız, yükleme [Java](http://www.oracle.com/technetwork/java/javase/downloads/index.html).
2. Yükleme [Maven](http://maven.apache.org/download.cgi).
3. Yeni bir klasör ve projeyi oluşturun:
    
    ```
    mkdir java-azure-test
    cd java-azure-test
    
    mvn archetype:generate -DgroupId=com.fabrikam -DartifactId=testAzureApp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

## <a name="add-dependencies"></a>Bağımlılıkları ekleyin.

1. Altında `testAzureApp` klasörü, açık `pom.xml` dosya ve derleme yapılandırmasını eklemek &lt;proje&gt; uygulamanızın oluşturulmasını etkinleştirmek için:

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

2. Azure Java SDK'sını erişmek için gerekli bağımlılıkları ekleyin.

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

## <a name="create-credentials"></a>Kimlik bilgileri oluşturun

Bu adım başlamadan önce erişimi olduğundan emin olun bir [Active Directory hizmet asıl](../../azure-resource-manager/resource-group-create-service-principal-portal.md). Uygulama kimliği, kimlik doğrulama anahtarı ve ihtiyacınız Kiracı kimliği bir sonraki adımda kaydetmelisiniz.

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

    Değiştir  **&lt;abonelik kimliği&gt;**  , abonelik tanımlayıcısı ile  **&lt;uygulama kimliği&gt;**  Active Directory Uygulama tanımlayıcısı ile  **&lt;kimlik doğrulama anahtarı&gt;**  uygulama anahtarla ve  **&lt;Kiracı kimliği&gt;**  , Kiracı tanımlayıcısı.

2. Dosyayı kaydedin.
3. Kimlik doğrulama dosyasının tam yolu ile Kabuk AZURE_AUTH_LOCATION adlı bir ortam değişkeni ayarlayın.

### <a name="create-the-management-client"></a>Yönetim istemcisi oluşturma

1. Açık `App.java` altında dosya `src\main\java\com\fabrikam` ve bu paket bildirimi en üstte olduğundan emin olun:

    ```java
    package com.fabrikam.testAzureApp;
    ```

2. Paket altında bu deyimine deyimleri içeri aktarın:
   
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

2. İstekleri yapmanız gereken Active Directory kimlik bilgileri oluşturmak için bu kodu uygulama sınıfı ana yöntemine ekleyin:
   
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

### <a name="create-the-availability-set"></a>Kullanılabilirlik kümesi oluştur

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
### <a name="create-the-public-ip-address"></a>Ortak IP adresi oluştur

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

Bir alt ağ ve sanal ağ oluşturmak için ana yöntem try bloğunda bu kodu ekleyin:

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

### <a name="create-the-network-interface"></a>Ağ arabirimi oluştur

Bir sanal makinenin sanal ağ iletişim kurmak için bir ağ arabirimi gerekiyor.

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

Tüm destekleyici kaynakları oluşturduğunuza göre bir sanal makine oluşturabilirsiniz.

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
> Bu öğretici, Windows Server işletim sistemi sürümünü çalıştıran bir sanal makine oluşturur. Diğer görüntüleri seçme hakkında daha fazla bilgi için bkz: [erişin ve seçin, Windows PowerShell ve Azure CLI Azure sanal makine görüntülerini](../linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
> 
>

Varolan bir diski bir Market görüntüsü yerine kullanmak istiyorsanız, bu kodu kullanın: 

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

Bir sanal makine yaşam döngüsü sırasında başlatma, durdurma veya bir sanal makine silme gibi yönetim görevleri çalıştırmak isteyebilirsiniz. Ayrıca, yinelenen veya karmaşık görevleri otomatikleştirmek için kod oluşturmak isteyebilirsiniz.

VM ile herhangi bir şey yapmanız gerektiğinde bir örneğini almanız gerekir. Bu kod main yöntemini deneyin bloğunu ekleyin:

```java
VirtualMachine vm = azure.virtualMachines().getByResourceGroup("myResourceGroup", "myVM");
```

### <a name="get-information-about-the-vm"></a>VM hakkında bilgi alın

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

Bir sanal makineyi durdurun ve tüm ayarları korumak ancak bunun için sizden ücret devam ya da sanal makineyi durdurun ve bunu serbest bırakma. Bir sanal makine serbest bırakıldığında, kendisiyle ilişkili tüm kaynakları da deallocated ve fatura onun için edilir.

Serbest bırakma olmadan sanal makineyi durdurmak için ana yöntem try bloğunda bu kodu ekleyin:

```java
System.out.println("Stopping vm...");
vm.powerOff();
System.out.println("Press enter to continue...");
input.nextLine();
```

Sanal makine ayırması istiyorsanız, bu kod kapalı çağrısı değiştirin:

```java
vm.deallocate();
```

### <a name="start-the-vm"></a>VM Başlat

Sanal makineyi başlatmak için ana yöntem try bloğunda bu kodu ekleyin:

```java
System.out.println("Starting vm...");
vm.start();
System.out.println("Press enter to continue...");
input.nextLine();
```

### <a name="resize-the-vm"></a>VM'yi yeniden boyutlandırın

Dağıtım pek çok görünüşünün sanal makineniz için bir boyut karar verirken dikkate alınmalıdır. Daha fazla bilgi için bkz: [VM boyutları](sizes.md).  

Sanal makine boyutunu değiştirmek için ana yöntem try bloğunda bu kodu ekleyin:

```java
System.out.println("Resizing vm...");
vm.update()
    .withSize(VirtualMachineSizeTypes.STANDARD_DS2)
    .apply();
System.out.println("Press enter to continue...");
input.nextLine();
```

### <a name="add-a-data-disk-to-the-vm"></a>VM için bir veri diski Ekle

Bir veri diski boyutu 2 GB ise, bir LUN 0 ve ReadWrite önbelleğe alma türü sahip sanal makine eklemek için ana yöntem try bloğunda bu kodu ekleyin:

```java
System.out.println("Adding data disk...");
vm.update()
    .withNewDataDisk(2, 0, CachingTypes.READ_WRITE)
    .apply();
System.out.println("Press enter to delete resources...");
input.nextLine();
```

## <a name="delete-resources"></a>Kaynakları silin

Azure'da kullanılan kaynaklar için ücretlendirildiğinizden, her zaman artık gerekli olmayan kaynakları silmek için iyi bir uygulamadır. Sanal makineler ve destekleyici tüm kaynakları silmek istiyorsanız, tüm yapmanız gereken olan kaynak grubunu silebilirsiniz.

1. Kaynak grubunu silmek için ana yöntem try bloğunda bu kodu ekleyin:
   
```java
System.out.println("Deleting resources...");
azure.resourceGroups().deleteByName("myResourceGroup");
```

2. App.java dosyasını kaydedin.

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Tamamlamak için bu konsol uygulamasını tamamen çalıştırın yaklaşık beş dakika sürer.

1. Uygulamayı çalıştırmak için bu Maven komutunu kullanın:

    ```
    mvn compile exec:java
    ```

2. Tuşuna önce **Enter** kaynakları silme başlatmak için Azure portalında kaynakların oluşturulmasını doğrulamak için birkaç dakika sürebilir. Dağıtım hakkında bilgi için dağıtım durumunu tıklatın.


## <a name="next-steps"></a>Sonraki adımlar
* Kullanma hakkında daha fazla bilgi edinin [Java için Azure kitaplıkları](https://docs.microsoft.com/en-us/java/azure/java-sdk-azure-overview).

