---
title: "Bir kullanıcı tarafından tanımlanan rota için rota ağ trafiği ağ sanal gereç - Azure CLI oluşturun | Microsoft Docs"
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
ms.openlocfilehash: adfcf53f9fca0efafb538edfd65b95313dcf1559
ms.sourcegitcommit: e38120a5575ed35ebe7dccd4daf8d5673534626c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2017
---
# <a name="create-a-user-defined-route---azure-cli"></a>Bir kullanıcı tanımlı yol - Azure CLI oluşturma

Bu öğreticide, iki arasında trafiği yönlendirmek için kullanıcı tanımlı yollar oluşturmayı öğrenin [sanal ağ](virtual-networks-overview.md) alt ağ sanal gereç aracılığıyla. Bir ağ sanal gereç bir güvenlik duvarı gibi bir ağ uygulaması çalıştıran bir sanal makinedir. Bir Azure sanal ağında dağıtabilirsiniz önceden yapılandırılmış ağ sanal Gereçleri hakkında daha fazla bilgi için bkz: [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances).

Bir sanal ağ alt ağları oluşturduğunuzda, Azure varsayılan oluşturur [sistem yolları](virtual-networks-udr-overview.md#system-routes) aşağıdaki resimde gösterildiği gibi birbirleri ile iletişim kurmak için tüm alt kaynaklar sağlamak:

![Varsayılan yolları](./media/create-user-defined-route/default-routes.png)

Bu öğreticide, bir sanal ağ genel, özel ve DMZ alt ağlar, aşağıdaki resimde gösterildiği gibi oluşturun. Web sunucuları için ortak bir alt ağ genellikle dağıtılmış olabilir ve bir uygulama veya veritabanı sunucusu için özel bir alt ağda dağıtılmış olabilir. DMZ alt ağı sanal gereç olarak davranacak bir sanal makine oluşturun ve isteğe bağlı olarak, ağ sanal gereç aracılığıyla iletişim her alt ağda bir sanal makine oluşturun. Ortak ve özel alt ağlar arasındaki tüm trafik, aşağıdaki resimde gösterildiği gibi gereç aracılığıyla yönlendirilir:

![Kullanıcı tanımlı yollar](./media/create-user-defined-route/user-defined-routes.png)

Bu makale, bir kullanıcı tarafından tanımlanan rota kullanıcı tanımlı yollar oluştururken kullanmanızı öneririz dağıtım modeli Resource Manager dağıtım modeli üzerinden oluşturmak için adımları sağlar. Bir kullanıcı tarafından tanımlanan rota (Klasik) oluşturmak ihtiyacınız varsa bkz [bir kullanıcı tarafından tanımlanan rota (Klasik) oluşturmak](virtual-network-create-udr-classic-cli.md). Azure'nın dağıtım modeliyle bilmiyorsanız bkz [anlamak Azure dağıtım modelleri](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Kullanıcı tanımlı yollar hakkında daha fazla bilgi için bkz: [kullanıcı tanımlı yollar genel bakış](virtual-networks-udr-overview.md#user-defined).

## <a name="create-routes-and-network-virtual-appliance"></a>Yollar ve ağ sanal uygulaması oluşturma

Windows, Linux veya macOS komutlar yürütülürken olup azure CLI komutları aynıdır. Ancak, işletim sistemi Kabukları arasında komut dosyası farklar vardır. Aşağıdaki adımlar komut yürütün bir yükleme ve Azure CLI komutlarının bir Bash kabuğunda yürütülmesini gerektirir. Seçebilir ya da [yükleyin ve Azure CLI yapılandırma](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json) PC veya yalnızca tıklatın **deneyin** herhangi bir Azure bulut Kabuğu'nda komut dosyalarını çalıştırmak için komut düğmesi.
 
1. **Önkoşul**: içindeki adımları tamamlayarak iki alt ağa sahip bir sanal ağ oluşturma [bir sanal ağ oluşturma](#create-a-virtual-network).
2. Azure CLI bilgisayarınızdan çalıştırılıyorsa, Azure ile oturum `az login` komutu. Bulut Kabuğu'nu kullanarak, otomatik olarak oturum açtınız.
3. Kalan adımlar boyunca kullanılan birkaç değişkenlerini ayarlayın:

    ```azurecli-interactive
    #Set variables used in the script.
    rgName="myResourceGroup"
    location="eastus"
    ```

4. Oluşturma bir *DMZ* önkoşul olarak oluşturulan sanal ağ alt:

    ```azurecli-interactive
    az network vnet subnet create \
      --name DMZ \
      --address-prefix 10.0.2.0/24 \
      --vnet-name myVnet \
      --resource-group $rgName
    ```

5. NVA sanal makine oluşturun. CLI oluşturur ağ arabirimine statik genel ve özel IP adresleri atayın. Statik adresler için sanal makinenin kullanım ömrü değiştirmeyin. Nva'nın Linux veya Windows işletim sistemini çalıştıran bir sanal makine olabilir. Sanal makine oluşturmak için her iki işletim sistemi için komut dosyasını kopyalayın ve CLI yapıştırın. Bir Windows VM oluşturma yapıştırırsanız komut dosyasını bir metin düzenleyicisine değerini değiştirin *Admınpassword* değişkeni sonra CLI içine yapıştırma değiştirilen metin.

    **Linux**

    ```azurecli-interactive
    az vm create \
      --resource-group $rgName \
      --name myVm-Nva \
      --image UbuntuLTS \
      --private-ip-address 10.0.2.4 \
      --public-ip-address myPublicIp-myVm-Nva \
      --public-ip-address-allocation static \
      --subnet DMZ \
      --vnet-name myVnet \
      --admin-username azureuser \
      --generate-ssh-keys
    ```

    **Windows**

    ```azurecli-interactive
    AdminPassword=ChangeToYourPassword
    az vm create \
      --resource-group $rgName \
      --name myVm-Nva \
      --image win2016datacenter \
      --private-ip-address 10.0.2.4 \
      --public-ip-address myPublicIp-myVm-Nva \
      --public-ip-address-allocation static \
      --subnet DMZ \
      --vnet-name myVnet \
      --admin-username azureuser \
      --admin-password $AdminPassword      
    ```

6. NVA'ın ağ arabirimi için IP iletimini etkinleştirin. IP iletme ağ arabirimi için kaynak/hedef IP adresi denetlemek Azure neden olur. Bu ayar, bir IP adresi, aldığı NIC dışında giden trafiğe etkinleştirmezseniz Azure tarafından bırakılır.

    ```azurecli-interactive
    az network nic update \
      --name myVm-NvaVMNic \
      --resource-group $rgName \
      --ip-forwarding true
    ```

7. İçin bir yol tablosu oluşturmanız *ortak* alt ağ.

    ```azurecli-interactive
    az network route-table create \
      --name myRouteTable-Public \
      --resource-group $rgName
    ```
    
8. Varsayılan olarak, Azure yollar sanal ağ içindeki tüm alt ağlar arasında trafiği. Ortak alt ağ trafiği özel alt ağa NVA yerine doğrudan özel alt ağına yönlendirilmesi yönlendirme Azure'nın varsayılan değeri değiştirmek için bir yol oluşturun.

    ```azurecli-interactive    
    az network route-table route create \
      --name ToPrivateSubnet \
      --resource-group $rgName \
      --route-table-name myRouteTable-Public \
      --address-prefix 10.0.1.0/24 \
      --next-hop-type VirtualAppliance \
      --next-hop-ip-address 10.0.2.4
    ```

9. İlişkilendirme *myRouteTable ortak* yol tablosuna *ortak* alt ağ. Bir alt ağ için bir yol tablosu ilişkilendirme rota tablosunda alt ağ yollarını göre tüm giden trafiği yönlendirmek Azure neden olur. Bir alt ağ için ilişkili sıfır veya bir yol tablosu olabilir ancak bir yol tablosu sıfır veya birden çok alt ağlara ilişkili olabilir.

    ```azurecli-interactive
    az network vnet subnet update \
      --name Public \
      --vnet-name myVnet \
      --resource-group $rgName \
      --route-table myRouteTable-Public
    ```

10. İçin yol tablosu oluşturmanız *özel* alt ağ.

    ```azurecli-interactive
    az network route-table create \
      --name myRouteTable-Private \
      --resource-group $rgName
    ```
      
11. Bir rota için rota trafiğinden oluşturma *özel* alt ağına *ortak* NVA sanal makine alt ağa.

    ```azurecli-interactive
    az network route-table route create \
      --name ToPublicSubnet \
      --resource-group $rgName \
      --route-table-name myRouteTable-Private \
      --address-prefix 10.0.0.0/24 \
      --next-hop-type VirtualAppliance \
      --next-hop-ip-address 10.0.2.4
    ```

12. Yol tablosu ilişkilendirmek *özel* alt ağ.

    ```azurecli-interactive
    az network vnet subnet update \
      --name Private \
      --vnet-name myVnet \
      --resource-group $rgName \
      --route-table myRouteTable-Private
    ```
    
13. **İsteğe bağlı:** ortak ve özel alt ağların bir sanal makine oluşturun ve sanal makineler arasındaki iletişim ağ sanal gereç aracılığıyla içindeki adımları tamamlayarak yönlendirilen doğrulamak [yönlendirmeDoğrula](#validate-routing).
14. **İsteğe bağlı**: Bu öğreticide oluşturduğunuz kaynaklarını silmek için adımları tamamlamanız [silmek kaynakları](#delete-resources).

## <a name="validate-routing"></a>Yönlendirme doğrula

1. Henüz yapmadıysanız, bölümündeki adımları tamamlamanız [oluşturma yollar ve ağ sanal gereç](#create-routes-and-network-virtual-appliance).
2. Tıklatın **deneyin** Azure bulut Kabuğu'nu açar, aşağıdaki kutusu düğmesini. İstenirse, Azure kullanarak oturum açtığınız, [Azure hesabı](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Azure hesabınız yoksa [ücretsiz denemeye](https://azure.microsoft.com/offers/ms-azr-0044p) kaydolabilirsiniz. Azure bulut Kabuk önceden Azure komut satırı arabirimi ücretsiz bash kabuğunda ' dir. 

    Aşağıdaki komut, birinde iki sanal makine oluşturma *ortak* alt ağı ve birinde *özel* alt ağ. Komut ayrıca ağ arabirimi üzerinden trafiği yönlendirmek IP iletme nva'nın işletim sistemi etkinleştirmek için işletim sistemi içinde ağ arabirimi için etkinleştirin. Genellikle üretim NVA yönlendirme önce trafiği inceler, ancak bu öğreticide, basit nva'nın yalnızca trafiği incelemek olmadan yönlendirir. 

    Tıklatın **kopya** düğmesini **Linux** veya **Windows** izleyin ve komut dosyası içeriğini bir metin düzenleyicisine yapıştırın, komut dosyaları. Parolasını değiştirme *Admınpassword* değişkeni sonra Azure bulut Kabuk içine yapıştırma komut dosyası. Seçili ağ sanal gereç 5. adımda oluşturduğunuz zaman işletim sistemi için komut dosyasını çalıştırmak [oluşturma yollar ve ağ sanal gereç](#create-routes-and-network-virtual-appliance). 

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
      
      > [!NOTE]
      > Önceki adımlar, Azure özel IP adresleri arasında yönlendirme onaylamak etkinleştirin. İleri veya proxy istiyorsanız, genel IP trafiği ağ sanal gereç aracılığıyla adresleri:
      > - Gereci ağ adresi çevirisi veya proxy özelliği sağlamanız gerekir. Ağ adresi çevirisi, kaynak IP adresini kendi ve bu isteği genel IP adresine iletmek Gereci çevrilmelidir durumunda. Gereci olup ağ adresi kaynak adresi çevrilen ya da proxy, ağ sanal gereç ait özel IP adresi genel bir IP adresi için Azure çevirir. Azure kullanan özel IP adresleri için ortak IP adresleri çevirmek için farklı yöntemler hakkında daha fazla bilgi için bkz: [giden bağlantılar anlama](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
      > - Ek bir rota öneki gibi yol tablosundaki: 0.0.0.0/0, sonraki atlama türü değerinin VirtualAppliance ve sonraki atlama IP adresi 10.0.2.4 (olarak önceki örnek komut dosyası).
      >
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

Bu öğreticiyi tamamladığınızda, böylece kullanım ücretlerine tabi yok, oluşturduğunuz kaynakları silmek isteyebilirsiniz. Bir kaynak grubunun silinmesi, kaynak grubunda bulunan tüm kaynakları siler. CLI oturumunda aşağıdaki komutu girin:

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Sonraki adımlar

- Oluşturma bir [yüksek oranda kullanılabilir bir ağ sanal Gereci](/azure/architecture/reference-architectures/dmz/nva-ha?toc=%2fazure%2fvirtual-network%2ftoc.json).
- Ağ sanal Gereçleri genellikle birden çok ağ arabirimleri ve atanmış IP adresleri vardır. Bilgi nasıl [ağ arabirimlerini varolan bir sanal makineye ekleyin](virtual-network-network-interface-vm.md#vm-add-nic) ve [var olan bir ağ arabirimi için IP adreslerini ekleyin](virtual-network-network-interface-addresses.md#add-ip-addresses). Tüm sanal makine boyutlarını kendisine bağlı en az iki ağ arabirimi kurabiliyor olsa bile, her sanal makine boyutu ağ arabirimleri sayısı üst destekler. Kaç tane ağ arabirimlerinin her sanal makine boyutu destekler bilgi edinmek için [Windows](../virtual-machines/windows/sizes.md?toc=%2Fazure%2Fvirtual-network%2Ftoc.json) ve [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) sanal makine boyutları. 
- Tüneli trafik şirket içi aracılığıyla zorlamak için kullanıcı tanımlı bir yol oluşturma bir [siteden siteye VPN bağlantısı](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
