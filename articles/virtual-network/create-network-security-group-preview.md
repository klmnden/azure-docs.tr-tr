---
title: "Azure ile ağ trafiği filtrelemek ağ ve uygulama güvenlik grupları (Önizleme) | Microsoft Docs"
description: "Sanal makineler ve dışındaki ağ ve uygulama güvenlik grupları ağ trafiği türü kısıtlamak için (Önizleme) oluşturmayı öğrenin."
services: virtual-network
documentationcenter: 
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/03/2017
ms.author: jdial
ms.custom: 
ms.openlocfilehash: 9aea299738eb5cac6fe6d3b633707862d978fff0
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2017
---
# <a name="filter-network-traffic-with-network-and-application-security-groups-preview"></a>Ağ ve uygulama güvenlik grupları (Önizleme) ile ağ trafiği filtreleme

Bu öğreticide, sanal makine grupları uygulama güvenlik grupları ile gelen ve giden ağ trafiği filtrelemek için ağ güvenlik grupları oluşturmayı öğrenin. Ağ güvenlik grupları ve uygulama güvenlik grupları hakkında daha fazla bilgi için bkz: [güvenliğine genel bakış](security-overview.md). 

Aşağıdaki bölümlerde, güvenlik grupları Azure komut satırı arabirimini kullanarak ağ oluşturmak için uygulayabileceğiniz adımlar içerir ([Azure CLI](#azure-cli)) ve [Azure PowerShell](#powershell). Sonuç, ağ güvenlik grupları oluşturmak için kullandığınız hangi aracı kullanırsanız aynıdır. Öğreticinin bu bölüme gitmek için bir aracı bağlantısına tıklayın. 

Bu makale, ağ güvenlik grupları ağ güvenlik grupları oluştururken kullanmanızı öneririz dağıtım modeli Resource Manager dağıtım modeli üzerinden oluşturmak için adımları sağlar. Bir ağ güvenlik grubu (Klasik) oluşturmak ihtiyacınız varsa bkz [bir ağ güvenlik grubu (Klasik) oluşturun](virtual-networks-create-nsg-classic-ps.md). Azure'nın dağıtım modeliyle bilmiyorsanız bkz [anlamak Azure dağıtım modelleri](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

> [!NOTE]
> Bu öğretici, şu anda Önizleme sürümünde olan ağ güvenlik grubu özelliklerini kullanır. Önizleme sürümü özellikleri aynı kullanılabilirlik ve güvenilirlik genel yayın özellikleri olarak yok. Önizleme sırasında özellikler yalnızca aşağıdaki bölgelerde kullanılabilir: WestCentralUS. Ağ güvenlik grupları yalnızca özellikleri genel kullanarak sürüm uygulamak isterseniz bkz [bir ağ güvenlik grubu oluşturun](virtual-networks-create-nsg-arm-pportal.md). 

## <a name="azure-cli"></a>Azure CLI

Windows, Linux veya macOS komutlar yürütülürken olup azure CLI komutları aynıdır. Ancak, işletim sistemi Kabukları arasında komut dosyası farklar vardır. Aşağıdaki adımlarda, betikleri, bir Bash kabuğunda yürütün. 

1. [Azure CLI'yi yükleme ve yapılandırma](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. Sürüm 2.0.18 kullandığınızdan emin olun ya da üst sürümünü girerek Azure CLI `az --version` komutu. Emin değilseniz, en son sürümünü yükleyin.
3. Oturum açtığınızda Azure ile `az login` komutu.
4. Önizleme için aşağıdaki komutları girerek kaydedin:
    
    ```azurecli-interactive
    az feature register --name AllowApplicationSecurityGroups --namespace Microsoft.Network
    az provider register --namespace Microsoft.Network
    ``` 

5. Aşağıdaki komutu girerek Önizleme için kayıtlı olduklarını doğrulayın:

    ```azurecli-interactive
    az feature show --name AllowApplicationSecurityGroups --namespace Microsoft.Network
    ```

    > [!WARNING]
    > Kaydı tamamlamak için bir saat sürebilir. Kadar kalan adımlara devam etmeyin *kayıtlı* görünür **durumu** önceki komuttan döndürülen çıkışı. Kayıtlı önce devam ederseniz, kalan adımları başarısız.

6. Bir kaynak grubu oluşturmak için aşağıdaki bash betiğini çalıştırın:

    ```azurecli-interactive
    #!/bin/bash
    
    az group create \
      --name myResourceGroup \
      --location westcentralus
    ```

7. Üç uygulama güvenlik grupları, her sunucu türü için bir tane oluşturun:

    ```azurecli-interactive
    az network asg create \
      --resource-group myResourceGroup \
      --name WebServers \
      --location westcentralus  

    az network asg create \
      --resource-group myResourceGroup \
      --name AppServers \
      --location westcentralus

    az network asg create \
      --resource-group myResourceGroup \
      --name DatabaseServers \
      --location westcentralus
    ```

8. Bir ağ güvenlik grubu oluşturun:

    ```azurecli-interactive
    az network nsg create \
      --resource-group myResourceGroup \
      --name myNsg \
      --location westcentralus
    ```

9. Uygulama güvenlik grupları hedef olarak ayarlama, NSG içinde güvenlik kurallarını oluşturun:
    
    ```azurecli-interactive    
    az network nsg rule create \
      --resource-group myResourceGroup \
      --nsg-name myNsg \
      --name WebRule \
      --priority 200 \
      --access "Allow" \
      --direction "inbound" \
      --source-address-prefixes "Internet" \
      --destination-asgs "WebServers" \
      --destination-port-ranges 80 \
      --protocol "TCP"

    az network nsg rule create \
      --resource-group myResourceGroup \
      --nsg-name myNsg \
      --name AppRule \
      --priority 300 \
      --access "Allow" \
      --direction "inbound" \
      --source-asgs "WebServers" \
      --destination-asgs "AppServers" \
      --destination-port-ranges 443 \
      --protocol "TCP"  

    az network nsg rule create \
      --resource-group myResourceGroup \
      --nsg-name myNsg \
      --name DatabaseRule \
      --priority 400 \
      --access "Allow" \
      --direction "inbound" \
      --source-asgs "AppServers" \
      --destination-asgs "DatabaseServers" \
      --destination-port-ranges 1336 \
      --protocol "TCP" 
    ``` 

10. Sanal ağ oluşturma: 
    
    ```azurecli-interactive
    az network vnet create \
      --name myVnet \
      --resource-group myResourceGroup \
      --subnet-name mySubnet \
      --address-prefix 10.0.0.0/16 \
      --location westcentralus
    ```

11. Sanal ağ alt ağına ağ güvenlik grubu ilişkilendirin:

    ```azurecli-interactive
    az network vnet subnet update \
      --name mySubnet \
      --resource-group myResourceGroup \
      --vnet-name myVnet \
      --network-security-group myNsg
    ```
    
12. Üç ağ arabirimlerinin her sunucu türü için bir tane oluşturun: 

    ```azurecli-interactive
    az network nic create \
      --resource-group myResourceGroup \
      --name myNic1 \
      --vnet-name myVnet \
      --subnet mySubnet \
      --location westcentralus \
      --application-security-groups "WebServers" "AppServers"

    az network nic create \
      --resource-group myResourceGroup \
      --name myNic2 \
      --vnet-name myVnet \
      --subnet mySubnet \
      --location westcentralus \
      --application-security-groups "AppServers"

    az network nic create \
      --resource-group myResourceGroup \
      --name myNic3 \
      --vnet-name myVnet \
      --subnet mySubnet \
      --location westcentralus \
      --application-security-groups "DatabaseServers"
    ```

    Ağ arabirimi bir üyesi olan uygulama güvenlik grubunu temel alan ağ arabirimi, yalnızca 9. adımda oluşturduğunuz karşılık gelen güvenlik kuralı uygulanır. Örneğin, yalnızca *WebRule* için etkilidir *myNic1*, ağ arabiriminin bir üyesi olduğundan *Web sunucuları* uygulama güvenlik grubu ve kural belirtir *Web sunucuları* hedefi olarak uygulama güvenlik grubu. *AppRule* ve *DatabaseRule* için kuralları uygulanmaz *myNic1*, ağ arabirimi üyesi olmadığından *AppServers*ve *DatabaseServers* uygulama güvenlik grupları.

13. İlgili ağ arabiriminin, her sanal makineye ekleniyor her sunucu türü için bir sanal makine oluşturun. Bu örnek, Windows sanal makineleri oluşturur, ancak değiştirebileceğiniz *win2016datacenter* için *UbuntuLTS* Linux sanal makineleri yerine oluşturmak için.

    ```azurecli-interactive
    # Update for your admin password
    AdminPassword=ChangeYourAdminPassword1

    az vm create \
      --resource-group myResourceGroup \
      --name myWebVm \
      --location westcentralus \
      --nics myNic1 \
      --image win2016datacenter \
      --admin-username azureuser \
      --admin-password $AdminPassword

    az vm create \
      --resource-group myResourceGroup \
      --name myAppVm \
      --location westcentralus \
      --nics myNic2 \
      --image win2016datacenter \
      --admin-username azureuser \
      --admin-password $AdminPassword

    az vm create \
      --resource-group myResourceGroup \
      --name myDatabaseVm \
      --location westcentralus \
      --nics myNic3 \
      --image win2016datacenter \
      --admin-username azureuser \
      --admin-password $AdminPassword    
    ```

14. **İsteğe bağlı**: içindeki adımları tamamlayarak bu öğreticide oluşturduğunuz kaynakları silmek [silmek kaynakları](#delete-cli).

## <a name="powershell"></a>PowerShell

1. Yükleme ve yapılandırma [PowerShell](/powershell/azure/install-azurerm-ps).
2. Sürüm 4.4.0 veya daha yüksek AzureRm modülün yüklü emin olun. Girerek yüklü sürümünüzü kontrol edebilirsiniz `Get-Module -ListAvailable AzureRM` komutu. AzureRM modülünden en son sürümünü yüklemek veya yükseltmek gerekiyorsa, yükleme [PowerShell Galerisi](https://www.powershellgallery.com/packages/AzureRM).
3. Bir PowerShell oturumunda Azure ile oturum açın, [Azure hesabı](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account) kullanarak `login-azurermaccount` komutu.
4. Önizleme için aşağıdaki komutları girerek kaydedin:
    
    ```powershell
    Register-AzureRmProviderFeature -FeatureName AllowApplicationSecurityGroups -ProviderNamespace Microsoft.Network
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
    ``` 

5. Aşağıdaki komutu girerek Önizleme için kayıtlı olduklarını doğrulayın:

    ```powershell
    Get-AzureRmProviderFeature -FeatureName AllowApplicationSecurityGroups -ProviderNamespace Microsoft.Network
    ```

    > [!WARNING]
    > Kaydı tamamlamak için bir saat sürebilir. Kadar kalan adımlara devam etmeyin *kayıtlı* görünür **RegistrationState** önceki komuttan döndürülen çıkışı. Kayıtlı önce devam ederseniz, kalan adımları başarısız.
        
6. Kaynak grubu oluşturun:

    ```powershell
    New-AzureRmResourceGroup `
      -Name myResourceGroup `
      -Location westcentralus
    ```

7. Üç uygulama güvenlik grupları, her sunucu türü için bir tane oluşturun:

    ```powershell
    $webAsg = New-AzureRmApplicationSecurityGroup `
      -ResourceGroupName myResourceGroup `
      -Name WebServers `
      -Location westcentralus
  
    $appAsg = New-AzureRmApplicationSecurityGroup `
      -ResourceGroupName myResourceGroup `
      -Name AppServers `
      -Location westcentralus

    $databaseAsg = New-AzureRmApplicationSecurityGroup `
      -ResourceGroupName myResourceGroup `
      -Name DatabaseServers `
      -Location westcentralus
    ```

8. Her sunucu türü için güvenlik kuralları oluşturun.
    
    ```powershell
    $webRule = New-AzureRmNetworkSecurityRuleConfig `
      -Name "WebRule" `
      -Access Allow `
      -Protocol Tcp `
      -Direction Inbound `
      -Priority 200 `
      -SourceAddressPrefix Internet `
      -SourcePortRange * `
      -DestinationApplicationSecurityGroupId $webAsg.id `
      -DestinationPortRange 80  

    $appRule = New-AzureRmNetworkSecurityRuleConfig `
      -Name "AppRule" `
      -Access Allow `
      -Protocol Tcp `
      -Direction Inbound `
      -Priority 300 `
      -SourceApplicationSecurityGroupId $webAsg.id `
      -SourcePortRange * `
      -DestinationApplicationSecurityGroupId $appAsg.id `
      -DestinationPortRange 443 

    $databaseRule = New-AzureRmNetworkSecurityRuleConfig `
      -Name "DatabaseRule" `
      -Access Allow `
      -Protocol Tcp `
      -Direction Inbound `
      -Priority 400 `
      -SourceApplicationSecurityGroupId $appAsg.id `
      -SourcePortRange * `
      -DestinationApplicationSecurityGroupId $databaseAsg.id `
      -DestinationPortRange 1336    
    ``` 

9. Bir ağ güvenlik grubu oluşturun:

    ```powershell
    $nsg = New-AzureRmNetworkSecurityGroup `
      -ResourceGroupName myResourceGroup `
      -Location westcentralus `
      -Name myNsg `
      -SecurityRules $WebRule,$AppRule,$DatabaseRule
    ```

10. Bir alt ağ yapılandırması oluşturun ve ona ağ güvenlik grubu ilişkilendirin:
    
    ```powershell
    $subnet = New-AzureRmVirtualNetworkSubnetConfig `
      -AddressPrefix 10.0.0.0/24 `
      -Name mySubnet `
      -NetworkSecurityGroup $nsg
    ```

11. Sanal ağ oluşturma: 
    
    ```powershell
    $vNet = New-AzureRmVirtualNetwork `
      -Name myVnet `
      -AddressPrefix '10.0.0.0/16' `
      -Subnet $subnet `
      -ResourceGroupName myResourceGroup `
      -Location westcentralus
    ```

12. Üç ağ arabirimlerinin her sunucu türü için bir tane oluşturun. 

    ```powershell
    $nic1 = New-AzureRmNetworkInterface `
      -Name myNic1 `
      -ResourceGroupName myResourceGroup `
      -Location westcentralus `
      -Subnet $vNet.Subnets[0] `
      -ApplicationSecurityGroup $webAsg,$appAsg

    $nic2 = New-AzureRmNetworkInterface `
      -Name myNic2 `
      -ResourceGroupName myResourceGroup `
      -Location westcentralus `
      -Subnet $vNet.Subnets[0] `
      -ApplicationSecurityGroup $appAsg

    $nic3 = New-AzureRmNetworkInterface `
      -Name myNic3 `
      -ResourceGroupName myResourceGroup `
      -Location westcentralus `
      -Subnet $vNet.Subnets[0] `
      -ApplicationSecurityGroup $databaseAsg
    ```

    Ağ arabirimi bir üyesi olan uygulama güvenlik grubunu temel alan ağ arabirimi, yalnızca adım 8'de oluşturulan karşılık gelen güvenlik kuralı uygulanır. Örneğin, yalnızca *WebRule* için etkilidir *myNic1*, ağ arabiriminin bir üyesi olduğundan *Web sunucuları* uygulama güvenlik grubu ve kural belirtir *Web sunucuları* hedefi olarak uygulama güvenlik grubu. *AppRule* ve *DatabaseRule* için kuralları uygulanmaz *myNic1*, ağ arabirimi üyesi olmadığından *AppServers*ve *DatabaseServers* uygulama güvenlik grupları.

13. İlgili ağ arabiriminin, her sanal makineye ekleniyor her sunucu türü için bir sanal makine oluşturun. Bu örnek, Windows sanal makineleri oluşturur, ancak komut dosyasını çalıştırmadan önce değiştirebilirsiniz *-Windows* için *- Linux*, *MicrosoftWindowsServer* için*Kurallı*, *Windows Server* için *UbuntuServer* ve *2016 Datacenter* için *14.04.2-LTS*Linux sanal makineleri yerine oluşturmak için.

    ```powershell
    # Create user object
    $cred = Get-Credential -Message "Enter a username and password for the virtual machine."
   
    # Create the web server virtual machine configuration and virtual machine.
    $webVmConfig = New-AzureRmVMConfig `
      -VMName myWebVm `
      -VMSize Standard_DS1_V2 | `
    Set-AzureRmVMOperatingSystem -Windows `
      -ComputerName myWebVm `
      -Credential $cred | `
    Set-AzureRmVMSourceImage `
      -PublisherName MicrosoftWindowsServer `
      -Offer WindowsServer `
      -Skus 2016-Datacenter `
      -Version latest | `
    Add-AzureRmVMNetworkInterface `
      -Id $nic1.Id
    New-AzureRmVM `
      -ResourceGroupName myResourceGroup `
      -Location westcentralus `
      -VM $webVmConfig

    # Create the app server virtual machine configuration and virtual machine.
    $appVmConfig = New-AzureRmVMConfig `
      -VMName myAppVm `
      -VMSize Standard_DS1_V2 | `
    Set-AzureRmVMOperatingSystem -Windows `
      -ComputerName myAppVm `
      -Credential $cred | `
    Set-AzureRmVMSourceImage `
      -PublisherName MicrosoftWindowsServer `
      -Offer WindowsServer `
      -Skus 2016-Datacenter `
      -Version latest | `
    Add-AzureRmVMNetworkInterface `
      -Id $nic2.Id
    New-AzureRmVM `
      -ResourceGroupName myResourceGroup `
      -Location westcentralus `
      -VM $appVmConfig

    # Create the database server virtual machine configuration and virtual machine.
    $databaseVmConfig = New-AzureRmVMConfig `
      -VMName myDatabaseVm `
      -VMSize Standard_DS1_V2 | `
    Set-AzureRmVMOperatingSystem -Windows `
      -ComputerName mydatabaseVm `
      -Credential $cred | `
    Set-AzureRmVMSourceImage `
      -PublisherName MicrosoftWindowsServer `
      -Offer WindowsServer `
      -Skus 2016-Datacenter `
      -Version latest | `
    Add-AzureRmVMNetworkInterface `
      -Id $nic3.Id
    New-AzureRmVM `
      -ResourceGroupName myResourceGroup `
      -Location westcentralus `
      -VM $databaseVmConfig
    ```

14. **İsteğe bağlı**: içindeki adımları tamamlayarak bu öğreticide oluşturduğunuz kaynakları silmek [silmek kaynakları](#delete-cli).

## <a name="delete"></a>Kaynakları silin

Bu öğreticiyi tamamladığınızda, böylece kullanım ücretlerine tabi yok, oluşturduğunuz kaynakları silmek isteyebilirsiniz. Bir kaynak grubunun silinmesi, kaynak grubunda bulunan tüm kaynakları siler.

### <a name="delete-portal"></a>Azure portalı

1. Portal arama kutusuna **myResourceGroup**. Arama sonuçlarında tıklatın **myResourceGroup**.
2. Üzerinde **myResourceGroup** dikey penceresinde tıklatın **silmek** simgesi.
3. İçinde silmeyi onaylamak için **türü kaynak grubu adı** kutusuna **myResourceGroup**ve ardından **silmek**.

### <a name="delete-cli"></a>Azure CLI

CLI oturumunda aşağıdaki komutu girin:

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

### <a name="delete-powershell"></a>PowerShell

Bir PowerShell oturumunda aşağıdaki komutu girin:

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

