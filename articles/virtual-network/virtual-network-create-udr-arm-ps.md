---
title: "Bir kullanıcı tarafından tanımlanan rota için rota ağ trafiği ağ sanal gereç - PowerShell oluşturun | Microsoft Docs"
description: "Azure'nın varsayılan bir ağ sanal gereç yoluyla ağ trafiği yönlendirerek yönlendirme geçersiz kılmak için bir kullanıcı tarafından tanımlanan rota oluşturmayı öğrenin."
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/16/2017
ms.author: jdial
ms.openlocfilehash: 9696a74ac02688f9004156f6f16b39b37756751d
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="create-a-user-defined-route---powershell"></a>Bir kullanıcı tarafından tanımlanan rota - PowerShell oluşturma

Bu öğreticide, iki arasında trafiği yönlendirmek için kullanıcı tanımlı yollar oluşturmayı öğrenin [sanal ağ](virtual-networks-overview.md) alt ağ sanal gereç aracılığıyla. Bir ağ sanal gereç bir güvenlik duvarı gibi bir ağ uygulaması çalıştıran bir sanal makinedir. Bir Azure sanal ağında dağıtabilirsiniz önceden yapılandırılmış ağ sanal Gereçleri hakkında daha fazla bilgi için bkz: [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances).

Bir sanal ağ alt ağları oluşturduğunuzda, Azure varsayılan oluşturur [sistem yolları](virtual-networks-udr-overview.md#system-routes) aşağıdaki resimde gösterildiği gibi birbirleri ile iletişim kurmak için tüm alt kaynaklar sağlamak:

![Varsayılan yolları](./media/create-user-defined-route/default-routes.png)

Bu öğreticide, bir sanal ağ genel, özel ve DMZ alt ağlar, aşağıdaki resimde gösterildiği gibi oluşturun. Web sunucuları için ortak bir alt ağ genellikle dağıtılmış olabilir ve bir uygulama veya veritabanı sunucusu için özel bir alt ağda dağıtılmış olabilir. DMZ alt ağı sanal gereç olarak davranacak bir sanal makine oluşturun ve isteğe bağlı olarak, ağ sanal gereç aracılığıyla iletişim her alt ağda bir sanal makine oluşturun. Ortak ve özel alt ağlar arasındaki tüm trafik, aşağıdaki resimde gösterildiği gibi gereç aracılığıyla yönlendirilir:

![Kullanıcı tanımlı yollar](./media/create-user-defined-route/user-defined-routes.png)

Bu makale, bir kullanıcı tarafından tanımlanan rota kullanıcı tanımlı yollar oluştururken kullanmanızı öneririz dağıtım modeli Resource Manager dağıtım modeli üzerinden oluşturmak için adımları sağlar. Bir kullanıcı tarafından tanımlanan rota (Klasik) oluşturmak ihtiyacınız varsa bkz [bir kullanıcı tarafından tanımlanan rota (Klasik) oluşturmak](virtual-network-create-udr-classic-ps.md). Azure'nın dağıtım modeliyle bilmiyorsanız bkz [anlamak Azure dağıtım modelleri](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Kullanıcı tanımlı yollar hakkında daha fazla bilgi için bkz: [kullanıcı tanımlı yollar genel bakış](virtual-networks-udr-overview.md#user-defined).

## <a name="create-routes-and-network-virtual-appliance"></a>Yollar ve ağ sanal uygulaması oluşturma

Yükleyebilir ve PowerShell'in en son sürümünü yapılandırabilirsiniz [AzureRM](https://www.powershellgallery.com/packages/AzureRM/) PC veya yalnızca tıklatın modülünü **deneyin** herhangi bir Azure bulut Kabuğu'nda komut dosyalarını çalıştırmak için komut düğmesi. Bulut Kabuğu yüklü PowerShell AzureRM modül sahiptir.
 
1. **Önkoşul**: içindeki adımları tamamlayarak iki alt ağa sahip bir sanal ağ oluşturma [bir sanal ağ oluşturma](#create-a-virtual-network).
2. PowerShell bilgisayarınızdan çalıştırılıyorsa, Azure ile oturum açın, [Azure hesabı](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account) kullanarak `login-azurermaccount` komutu. Bulut Kabuğu'nu kullanarak, otomatik olarak oturum açtınız. Bulut Kabuk önkoşul sanal ağ oluşturulurken kullanılan Bash Kabuğu'ndan değiştirmek için yeniden başlatmanız gerekebilir.
3. Adımlar boyunca kullanılan birkaç değişkenlerini ayarlayın:

    ```azurepowershell-interactive
    $rgName="myResourceGroup"
    $location="eastus"
    ```

4. DMZ alt ağı mevcut sanal ağ oluşturun:

    ```azurepowershell-interactive
    $virtualNetwork = Get-AzureRmVirtualNetwork `
      -Name myVnet `
      -ResourceGroupName $rgName 
    Add-AzureRmVirtualNetworkSubnetConfig `
      -Name 'DMZ' `
      -AddressPrefix "10.0.2.0/24" `
      -VirtualNetwork $virtualNetwork
    $virtualNetwork | Set-AzureRmVirtualNetwork
    ```

5. Ağ sanal gereç sanal makine için bir statik genel IP adresi oluşturun.

    ```azurepowershell-interactive
    $Pip = New-AzureRmPublicIpAddress `
      -AllocationMethod Static `
      -ResourceGroupName $rgName `
      -Location $location `
      -Name myPublicIp-myVm-Nva
    
6. Create a network interface in the *DMZ* subnet, assign a static private IP address to it, and enable IP forwarding for the network interface. By assigning a static IP address to the network interface, you ensure that it doesn't change for the life of the virtual machine the network interface is attached to. IP forwarding must be enabled for each network interface that receives traffic not addressed to an IP address assigned to it.

    ```azurepowershell-interactive
    $virtualNetwork = Get-AzureRmVirtualNetwork `
      -Name myVnet `
      -ResourceGroupName $rgName
    $subnet = Get-AzureRmVirtualNetworkSubnetConfig `
      -Name DMZ `
      -VirtualNetwork $virtualNetwork
    $nic = New-AzureRmNetworkInterface `
      -ResourceGroupName $rgName `
      -Location $location `
      -Name 'myNic-Nva' `
      -SubnetId $subnet.Id `
      -PrivateIpAddress 10.0.2.4 `
      -EnableIPForwarding `
      -PublicIpAddressId $Pip.Id 
    ```

7. NVA sanal makine oluşturun. Nva'nın Linux veya Windows işletim sistemini çalıştıran bir sanal makine olabilir. Sanal makine oluşturmak için her iki işletim sistemi için komut dosyasını kopyalayın ve PowerShell oturumunuza yapıştırın. Bir Windows VM oluşturma yapıştırırsanız komut dosyasını bir metin düzenleyicisine $cred değişkeni için "değerini" olarak değiştirin ve ardından PowerShell oturumunuza değiştirilen metni yapıştırın:

    **Linux**

    ```azurepowershell-interactive
    # Define the admin user name and a blank password.
    $securePassword = ConvertTo-SecureString ' ' -AsPlainText -Force
    $cred = New-Object System.Management.Automation.PSCredential ("azureuser", $securePassword)

    # Define the virtual machine configuration.
    $vmConfig = New-AzureRmVMConfig `
      -VMName 'myVm-Nva' `
      -VMSize Standard_DS2 | `
      Set-AzureRmVMOperatingSystem -Linux `
      -ComputerName 'myVm-Nva' `
      -Credential $cred -DisablePasswordAuthentication| `
      Set-AzureRmVMSourceImage `
      -PublisherName Canonical `
      -Offer UbuntuServer `
      -Skus 14.04.2-LTS `
      -Version latest | `
      Add-AzureRmVMNetworkInterface -Id $nic.Id

    # Configure SSH Keys
    $sshPublicKey = Get-Content "$env:USERPROFILE\.ssh\id_rsa.pub"
    Add-AzureRmVMSshPublicKey -VM $vmconfig -KeyData $sshPublicKey -Path "/home/azureuser/.ssh/authorized_keys"    

    # Create the virtual machine
    $vm = New-AzureRmVM `
      -ResourceGroupName $rgName `
      -Location $location `
      -VM $vmConfig
    ```

    **Windows**

    ```azurepowershell-interactive
    # Create user object
    $cred = Get-Credential -Message "Enter a username and password for the virtual machine."

    $vmConfig = New-AzureRmVMConfig `
      -VMName 'myVm-Nva' `
      -VMSize Standard_DS2 | `
      Set-AzureRmVMOperatingSystem -Windows `
      -ComputerName 'myVm-Nva' `
      -Credential $cred | `
      Set-AzureRmVMSourceImage `
      -PublisherName MicrosoftWindowsServer `
      -Offer WindowsServer `
      -Skus 2016-Datacenter `
      -Version latest | `
      Add-AzureRmVMNetworkInterface -Id $nic.Id
    
    $vm = New-AzureRmVM `
      -ResourceGroupName $rgName `
      -Location $location `
      -VM $vmConfig
    ```

8. Varsayılan olarak, Azure yollar sanal ağ içindeki tüm alt ağlar arasında trafiği. Azure'nın varsayılan, gelen trafik için yönlendirme değiştirmek için bir yol oluşturma *ortak* alt yönlendirilir NVA için yerine doğrudan *özel* alt ağ.

    ```azurepowershell-interactive    
    $routePrivate = New-AzureRmRouteConfig `
      -Name 'ToPrivateSubnet' `
      -AddressPrefix 10.0.1.0/24 `
      -NextHopType VirtualAppliance `
      -NextHopIpAddress $nic.IpConfigurations[0].PrivateIpAddress
    ```

9. İçin bir yol tablosu oluşturmanız *ortak* alt ağ.

    ```azurepowershell-interactive
    $routeTablePublic = New-AzureRmRouteTable `
      -Name 'myRouteTable-Public' `
      -ResourceGroupName $rgName `
      -location $location `
      -Route $routePrivate
    ```
    
10. Ortak alt ağ için yol tablosu ilişkilendirin. Bir alt ağ için bir yol tablosu ilişkilendirme rota tablosunda alt ağ yollarını göre tüm giden trafiği yönlendirmek Azure neden olur. Bir alt ağ için ilişkili sıfır veya bir yol tablosu olabilir ancak bir yol tablosu sıfır veya birden çok alt ağlara ilişkili olabilir.

    ```azurepowershell-interactive
    Set-AzureRmVirtualNetworkSubnetConfig `
      -VirtualNetwork $virtualNetwork `
      -Name 'Public' `
      -AddressPrefix 10.0.0.0/24 `
      -RouteTable $routeTablePublic | `
    Set-AzureRmVirtualNetwork
    ```

11. Gelen trafiği için bir rota oluşturmak *özel* alt ağına *ortak* NVA sanal makine alt ağa.

    ```azurepowershell-interactive    
    $routePublic = New-AzureRmRouteConfig `
      -Name 'ToPublicSubnet' `
      -AddressPrefix 10.0.0.0/24 `
      -NextHopType VirtualAppliance `
      -NextHopIpAddress $nic.IpConfigurations[0].PrivateIpAddress
    ```

12. İçin yol tablosu oluşturmanız *özel* alt ağ.

    ```azurepowershell-interactive
    $routeTablePrivate = New-AzureRmRouteTable `
      -Name 'myRouteTable-Private' `
      -ResourceGroupName $rgName `
      -location $location `
      -Route $routePublic
    ```
      
13. Yol tablosu ilişkilendirmek *özel* alt ağ.

    ```azurepowershell-interactive
    Set-AzureRmVirtualNetworkSubnetConfig `
      -VirtualNetwork $virtualNetwork `
      -Name 'Private' `
      -AddressPrefix 10.0.1.0/24 `
      -RouteTable $routeTablePrivate | `
    Set-AzureRmVirtualNetwork
    ```
    
14. **İsteğe bağlı:** ortak ve özel alt ağların bir sanal makine oluşturun ve sanal makineler arasındaki iletişim ağ sanal gereç aracılığıyla içindeki adımları tamamlayarak yönlendirilen doğrulamak [yönlendirmeDoğrula](#validate-routing).
15. **İsteğe bağlı**: Bu öğreticide oluşturduğunuz kaynaklarını silmek için adımları tamamlamanız [silmek kaynakları](#delete-resources).

## <a name="validate-routing"></a>Yönlendirme doğrula

1. Henüz yapmadıysanız, bölümündeki adımları tamamlamanız [oluşturma yollar ve ağ sanal gereç](#create-routes-and-network-virtual-appliance).
2. Tıklatın **deneyin** Azure bulut Kabuğu'nu açar, aşağıdaki kutusu düğmesini. İstenirse, Azure kullanarak oturum açtığınız, [Azure hesabı](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Azure hesabınız yoksa [ücretsiz denemeye](https://azure.microsoft.com/offers/ms-azr-0044p) kaydolabilirsiniz. Azure bulut Kabuk önceden Azure komut satırı arabirimi ücretsiz bash kabuğunda ' dir. 

    Aşağıdaki komut, birinde iki sanal makine oluşturma *ortak* alt ağı ve birinde *özel* alt ağ. Komut ayrıca ağ arabirimi üzerinden trafiği yönlendirmek IP iletme nva'nın işletim sistemi etkinleştirmek için işletim sistemi içinde ağ arabirimi için etkinleştirin. Genellikle üretim NVA yönlendirme önce trafiği inceler, ancak bu öğreticide, basit nva'nın yalnızca trafiği incelemek olmadan yönlendirir. 

    Tıklatın **kopya** düğmesini **Linux** veya **Windows** izleyin ve komut dosyası içeriğini bir metin düzenleyicisine yapıştırın, komut dosyaları. Parolasını değiştirme *Admınpassword* değişkeni sonra Azure bulut Kabuk içine yapıştırma komut dosyası. 7. adımda ağ sanal gereç oluştururken seçtiğiniz işletim sistemi için komut dosyasını çalıştırmak [oluşturma yollar ve ağ sanal gereç](#create-routes-and-network-virtual-appliance). 

    **Linux**

    ```azurecli-interactive
    #!/bin/bash

    #Set variables used in the script.
    rgName="myResourceGroup"
    location="eastus"
    adminPassword=ChangeToYourPassword
    
    # Create a virtual machine in the Public subnet.
    az vm create \
      --resource-group $rgName \
      --name myVm-Public \
      --image UbuntuLTS \
      --vnet-name myVnet \
      --subnet Public \
      --public-ip-address myPublicIp-Public \
      --admin-username azureuser \
      --admin-password $adminPassword

    # Create a virtual machine in the Private subnet.
    az vm create \
      --resource-group $rgName \
      --name myVm-Private \
      --image UbuntuLTS \
      --vnet-name myVnet \
      --subnet Private \
      --public-ip-address myPublicIp-Private \
      --admin-username azureuser \
      --admin-password $adminPassword

    # Enable IP forwarding for the network interface in the NVA virtual machine's operating system.    
    az vm extension set \
      --resource-group $rgName \
      --vm-name myVm-Nva \
      --name customScript \
      --publisher Microsoft.Azure.Extensions \
      --settings '{"commandToExecute":"sudo sysctl -w net.ipv4.ip_forward=1"}'
    ```

    **Windows**

    ```azurecli-interactive

    #!/bin/bash
    #Set variables used in the script.
    rgName="myResourceGroup"
    location="eastus"
    adminPassword=ChangeToYourPassword
    
    # Create a virtual machine in the Public subnet.
    az vm create \
      --resource-group $rgName \
      --name myVm-Public \
      --image win2016datacenter \
      --vnet-name myVnet \
      --subnet Public \
      --public-ip-address myPublicIp-Public \
      --admin-username azureuser \
      --admin-password $adminPassword

    # Allow pings through the Windows Firewall.
    az vm extension set \
      --publisher Microsoft.Compute \
      --version 1.9 \
      --name CustomScriptExtension \
      --vm-name myVm-Public \
      --resource-group $rgName \
      --settings '{"commandToExecute":"netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow"}'

    # Create a virtual machine in the Private subnet.
    az vm create \
      --resource-group $rgName \
      --name myVm-Private \
      --image win2016datacenter \
      --vnet-name myVnet \
      --subnet Private \
      --public-ip-address myPublicIp-Private \
      --admin-username azureuser \
      --admin-password $adminPassword

    # Allow pings through the Windows Firewall.
    az vm extension set \
      --publisher Microsoft.Compute \
      --version 1.9 \
      --name CustomScriptExtension \
      --vm-name myVm-Private \
      --resource-group $rgName \
      --settings '{"commandToExecute":"netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow"}'

    # Enable IP forwarding for the network interface in the NVA virtual machine's operating system.
    az vm extension set \
      --publisher Microsoft.Compute \
      --version 1.9 \
      --name CustomScriptExtension \
      --vm-name myVm-Nva \
      --resource-group $rgName \
      --settings '{"commandToExecute":"powershell.exe Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters -Name IpEnableRouter -Value 1"}'

    # Restart the NVA virtual machine.
    az vm extension set \
      --publisher Microsoft.Compute \
      --version 1.9 \
      --name CustomScriptExtension \
      --vm-name myVm-Nva \
      --resource-group $rgName \
      --settings '{"commandToExecute":"powershell.exe Restart-Computer -ComputerName myVm-Nva -Force"}'
    ```

3. Sanal makineler ortak ve özel alt arasındaki iletişimi doğrulayın. 

    - Açık bir [SSH](../virtual-machines/linux/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-network%2ftoc.json#connect-to-vm) (Linux) veya [Uzak Masaüstü](../virtual-machines/windows/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-network%2ftoc.json#connect-to-vm) genel IP adresi (Windows) bağlantısı *myVm ortak* sanal makine.
    - Bir komut isteminden *myVm ortak* sanal makine girin `ping myVm-Private`. NVA herkese açık özel alt ağ trafiğini yönlendirir çünkü yanıt alırsınız.
    - Gelen *myVm ortak* sanal makine, sanal makineler ortak ve özel alt arasında bir izleme yolu çalıştırın. Aşağıdaki uygun komutu girin, hangi işletim sistemini yapılandırdığınıza bağlı olarak, sanal makineler ortak ve özel alt yüklenir:
        - **Windows**: bir komut isteminden çalıştırın `tracert myvm-private` komutu.
        - **Ubuntu**: çalıştırmak `tracepath myvm-private` komutu.
      Trafiği 10.0.1.4 (sanal makine özel bir alt ağ) ulaşmadan önce 10.0.2.4 (NVA) geçirir. 
    - Önceki adımları bağlanarak *myVm özel* sanal makine ve ping *myVm ortak* sanal makine. İzleme yolu 10.0.0.4 (sanal makine ortak bir alt ağ) ulaşmadan önce 10.0.2.4 yolculuk iletişimi gösterir.
    - **İsteğe bağlı olarak**: Azure iki sanal makineler arasında sonraki atlama doğrulamak için sonraki atlama yeteneğini Azure Ağ İzleyicisi'ni kullanın. Ağ İzleyicisi'ni kullanmadan önce öncelikle [Azure Ağ İzleyicisi örnek oluşturmak](../network-watcher/network-watcher-create.md?toc=%2fazure%2fvirtual-network%2ftoc.json) içinde kullanmak istediğiniz bölge için. Bu öğreticide, BİZE Doğu bölgesi kullanılır. Bölge için bir Ağ İzleyicisi örneği etkinleştirdikten sonra ortak ve özel alt sanal makineler arasında sonraki atlama bilgileri görmek için aşağıdaki komutu girin:
     
        ```azurecli-interactive
        az network watcher show-next-hop --resource-group myResourceGroup --vm myVm-Public --source-ip 10.0.0.4 --dest-ip 10.0.1.4
        ```

       Çıktıyı döndürür *10.0.2.4* olarak **Nexthopıpaddress** ve *değerinin VirtualAppliance* olarak **nextHopType**.

> [!NOTE]
> Bu öğreticide kavramları göstermek için genel IP adresleri sanal makinelere genel ve özel alt ağları atanmış ve tüm ağ bağlantı noktası erişimi hem de sanal makineler için Azure içinde etkinleştirilir. Üretim kullanımı için sanal makineler oluştururken, genel IP adresleri için devredemezsiniz ve özel alt ağ için ağ trafiği ağ sanal gereç, önünde dağıtma ya da alt ağlara bir ağ güvenlik grubu atanması filtre uygulayabilir, Ağ arabirimi, veya her ikisi de. Ağ güvenlik grupları hakkında daha fazla bilgi için bkz: [ağ güvenlik grupları](virtual-networks-nsg.md).

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

Bu öğretici, varolan bir sanal ağı iki alt ağ ile gerektirir. Tıklatın **deneyin** hızla bir sanal ağ oluşturmak için aşağıdaki kutusundaki düğmesi. Tıklatarak **deneyin** düğmesini açılır [Azure bulut Kabuk](../cloud-shell/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Bu bölümde bulut Kabuk PowerShell veya Bash kabuğunda çalışır ancak Bash kabuğunda sanal ağ oluşturmak için kullanılır. Bash kabuğunda yüklü Azure komut satırı arabirimine sahiptir. Bulut Kabuk tarafından istenirse Azure kullanarak oturum açtığınız, [Azure hesabı](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Azure hesabınız yoksa [ücretsiz denemeye](https://azure.microsoft.com/offers/ms-azr-0044p) kaydolabilirsiniz. Bu öğreticide kullanılan sanal ağ oluşturmak için tıklatın **kopyalama** düğmesini aşağıdaki kutuya sonra Azure bulut kabuğundan komut dosyasını yapıştırın:

```azurecli-interactive
#!/bin/bash

#Set variables used in the script.
rgName="myResourceGroup"
location="eastus"

# Create a resource group.
az group create \
  --name $rgName \
  --location $location

# Create a virtual network with one subnet named Public.
az network vnet create \
  --name myVnet \
  --resource-group $rgName \
  --address-prefixes 10.0.0.0/16 \
  --subnet-name Public \
  --subnet-prefix 10.0.0.0/24

# Create an additional subnet named Private in the virtual network.
az network vnet subnet create \
  --name Private \
  --address-prefix 10.0.1.0/24 \
  --vnet-name myVnet \
  --resource-group $rgName
```

Portal, PowerShell veya Azure Resource Manager şablonu bir sanal ağ oluşturmak için nasıl kullanılacağı hakkında daha fazla bilgi için bkz: [bir sanal ağ oluşturma](virtual-networks-create-vnet-arm-pportal.md).

## <a name="delete-resources"></a>Kaynakları silin

Bu öğreticiyi tamamladığınızda, böylece kullanım ücretlerine tabi yok, oluşturduğunuz kaynakları silmek isteyebilirsiniz. Bir kaynak grubunun silinmesi, kaynak grubunda bulunan tüm kaynakları siler. PowerShell'de aşağıdaki komutu girin:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Sonraki adımlar

- Oluşturma bir [yüksek oranda kullanılabilir bir ağ sanal Gereci](/azure/architecture/reference-architectures/dmz/nva-ha?toc=%2fazure%2fvirtual-network%2ftoc.json).
- Ağ sanal Gereçleri genellikle birden çok ağ arabirimleri ve atanmış IP adresleri vardır. Bilgi nasıl [ağ arabirimlerini varolan bir sanal makineye ekleyin](virtual-network-network-interface-vm.md#vm-add-nic) ve [var olan bir ağ arabirimi için IP adreslerini ekleyin](virtual-network-network-interface-addresses.md#add-ip-addresses). Tüm sanal makine boyutlarını kendisine bağlı en az iki ağ arabirimi kurabiliyor olsa bile, her sanal makine boyutu ağ arabirimleri sayısı üst destekler. Kaç tane ağ arabirimlerinin her sanal makine boyutu destekler bilgi edinmek için [Windows](../virtual-machines/windows/sizes.md?toc=%2Fazure%2Fvirtual-network%2Ftoc.json) ve [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) sanal makine boyutları. 
- Tüneli trafik şirket içi aracılığıyla zorlamak için kullanıcı tanımlı bir yol oluşturma bir [siteden siteye VPN bağlantısı](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
