---
title: "Bir kullanıcı tarafından tanımlanan rota için rota ağ trafiği ağ sanal gereç - Azure portalında oluşturun | Microsoft Docs"
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
ms.openlocfilehash: 736e48f9651d89a1f4e8e0ae72cdffebb8e9c6e0
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="create-a-user-defined-route---azure-portal"></a>Azure portalında bir kullanıcı tarafından tanımlanan rota - oluşturun

Bu öğreticide, iki arasında trafiği yönlendirmek için kullanıcı tanımlı yollar oluşturmayı öğrenin [sanal ağ](virtual-networks-overview.md) alt ağ sanal gereç aracılığıyla. Bir ağ sanal gereç bir güvenlik duvarı gibi bir ağ uygulaması çalıştıran bir sanal makinedir. Bir Azure sanal ağında dağıtabilirsiniz önceden yapılandırılmış ağ sanal Gereçleri hakkında daha fazla bilgi için bkz: [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances).

Bir sanal ağ alt ağları oluşturduğunuzda, Azure varsayılan oluşturur [sistem yolları](virtual-networks-udr-overview.md#system-routes) aşağıdaki resimde gösterildiği gibi birbirleri ile iletişim kurmak için tüm alt kaynaklar sağlamak:

![Varsayılan yolları](./media/create-user-defined-route/default-routes.png)

Bu öğreticide, bir sanal ağ genel, özel ve DMZ alt ağlar, aşağıdaki resimde gösterildiği gibi oluşturun. Web sunucuları için ortak bir alt ağ genellikle dağıtılmış olabilir ve bir uygulama veya veritabanı sunucusu için özel bir alt ağda dağıtılmış olabilir. DMZ alt ağı sanal gereç olarak davranacak bir sanal makine oluşturun ve isteğe bağlı olarak, ağ sanal gereç aracılığıyla iletişim her alt ağda bir sanal makine oluşturun. Ortak ve özel alt ağlar arasındaki tüm trafik, aşağıdaki resimde gösterildiği gibi gereç aracılığıyla yönlendirilir:

![Kullanıcı tanımlı yollar](./media/create-user-defined-route/user-defined-routes.png)

Bu makale, bir kullanıcı tarafından tanımlanan rota kullanıcı tanımlı yollar oluştururken kullanmanızı öneririz dağıtım modeli Resource Manager dağıtım modeli üzerinden oluşturmak için adımları sağlar. Bir kullanıcı tarafından tanımlanan rota (Klasik) oluşturmak ihtiyacınız varsa bkz [bir kullanıcı tarafından tanımlanan rota (Klasik) oluşturmak](virtual-network-create-udr-classic-ps.md). Azure'nın dağıtım modeliyle bilmiyorsanız bkz [anlamak Azure dağıtım modelleri](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Kullanıcı tanımlı yollar hakkında daha fazla bilgi için bkz: [kullanıcı tanımlı yollar genel bakış](virtual-networks-udr-overview.md#user-defined).

## <a name="create-routes-and-network-virtual-appliance"></a>Yollar ve ağ sanal uygulaması oluşturma

1. **Önkoşul**: içindeki adımları tamamlayarak iki alt ağa sahip bir sanal ağ oluşturma [bir sanal ağ oluşturma](#create-a-virtual-network).
2. Bir Internet tarayıcısında sanal ağ oluşturduktan sonra Git [Azure portal](https://portal.azure.com). Kullanarak oturum açın, [Azure hesabı](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account).
3. Üzerinde **arama kaynakları** kutusuna portalın en üstünde *myResourceGroup*. Tıklatın **myResourceGroup** arama sonuçlarında görüntülendiğinde. Kaynak grubu önkoşul bir parçası olarak oluşturuldu.
4. Tıklatın **myVnet**, aşağıdaki resimde gösterildiği gibi:

    ![Ağ arabirimi ayarları](./media/create-user-defined-route/virtual-network.png)

5. Ağ sanal uygulaması için bir alt ağ oluşturun:
 
    - Altında **myVnet**, tıklatın **alt ağlar** altında **ayarları** sol tarafındaki.
    - Tıklatın **+ alt**, aşağıdaki resimde gösterildiği gibi:

        ![Alt ağlar](./media/create-user-defined-route/subnets.png) 
    - Altında aşağıdaki değerleri girin **alt ağ Ekle**, ardından **Tamam**:

        |Ayar|Değer|
        |-----|-----|
        |Ad|DMZ|
        |Adres aralığı (CIDR bloğu)|10.0.2.0/24|

6. Bir ağ sanal gereç sanal makine oluşturun:

    - Portalın sol tarafta tıklatın **+ yeni**, ardından **işlem**ve ardından **Windows Server 2016 Datacenter** veya **Ubuntu Server 16.04 LTS**.
    - Aşağıdaki değerleri girin **Temelleri** görünür, ardından dikey **Tamam**.

        |Ayar|Değer|
        |---|---|
        |Ad|myVm Nva|
        |Kullanıcı adı|azureuser|
        |Parola ve parolayı onayla|Seçtiğiniz bir parola|
        |Abonelik|Aboneliğinizi seçin|
        |Kaynak grubu|Tıklatın **var olanı kullan**seçeneğini belirleyip **myResourceGroup**|
        |Konum|Doğu ABD|
    - Üzerinde **bir boyutu seçin** görünür, dikey tıklayın **DS1_V2 standart**, ardından **seçin**.
    - Üzerinde **ayarları** görünür, dikey tıklayın **sanal ağ**. Tıklatın **myVnet** içinde **Seç sanal ağ** görünür dikey.
    - Üzerinde **ayarları** dikey penceresinde tıklatın **alt**. Tıklatın **DMZ** üzerinde **alt ağ seçin** görünür dikey. 
    - Diğer ayarlar için varsayılan değerleri bırakın ve tıklatın **Tamam**.
    - Tıklatın **oluşturma** üzerinde **oluşturma** görünür dikey. Sanal makinenin dağıtımını birkaç dakika sürer.

    > [!NOTE]
    > Sanal makine oluşturma, yanı sıra Azure portalına genel bir IP adresi oluşturur ve sanal makine için varsayılan olarak atar. Sanal makine bir üretim ortamında dağıtma, bir ortak IP adresi sanal makineyi atamamayı seçebilirsiniz. Bunun yerine, sanal ağ içindeki diğer sanal makinelerden ağ sanal gereç erişebilir. Genel IP adresleri hakkında daha fazla bilgi için bkz: [genel bir IP adresi yönetmek](virtual-network-public-ip-address.md).

7. Özel bir statik IP adresi atayın ve ağ arabirimi için önceki adımda oluşturduğunuz portal iletme IP etkinleştirin. 
    - Üzerinde **arama kaynakları** kutusuna sayfanın en üstünde *myVm Nva*.
    - Tıklatın **myVm Nva** arama sonuçlarında görüntülendiğinde.
    - Tıklatın **ağ** altında **ayarları** sol tarafındaki.
    - Ağ arabirimi altında adına tıklayın **myVm-Nva - ağ arabirimleri**. Ad **myvm nva***X*, burada *X* portal tarafından atanan bir sayıdır.
    - Tıklatın **IP yapılandırmaları** altında **ayarları** aşağıdaki resimde gösterildiği gibi ağ arabirimi için:

        ![Ağ arabirimi ayarları](./media/create-user-defined-route/network-interface-settings.png)
        
    - Tıklatın **etkin** için **IP iletimini** ayarlamak, ardından **kaydetmek**. Kendisine atanmış bir IP adresi ele değil trafiği alır her ağ arabirimi için IP iletimini yeniden etkinleştirilmesi gerekir. Azure'nın kaynak/hedef onay ağ arabirimi için IP iletimini etkinleştirme devre dışı bırakır.
    - Tıklatın **ipconfig1** IP yapılandırmaları listesinde.
    - Tıklatın **statik** için **atama** altında özel IP adresinin **ipconfig1**, aşağıdaki resimde gösterildiği gibi:

        ![IP yapılandırması](./media/create-user-defined-route/ip-configuration.png)
    - Önceki resimde gösterildiği gibi girin *10.0.2.4* altında **IP adresi** içinde **özel IP adresi ayarlarını**. Ağ arabirimi için statik bir IP adresi atayarak adresi ömrü boyunca ağ arabiriminin bağlı olduğu sanal makine değiştirmez sağlar. Girilen adres, ağ arabirimi olarak DMZ alt ağdaki başka bir kaynak için şu anda atanmış değil. 
    - Yapılandırmayı kaydetmek için tıklatın **kaydetmek** altında **ipconfig1**. Ağ arabirimi kaydedilen Portalı'nda bir bildirim kadar ipconfig1 kutusunu kapatmayın.
 
8. İki yol tablolarını oluşturun. Yönlendirme tabloları sıfır veya daha fazla yol içerir:

    - Portalın sol tarafta tıklatın **+ yeni** > **ağ** > **yol tablosu**.
    - Altında aşağıdaki değerleri girin **bir yol tablosu oluşturmanız**ve ardından **oluşturma**:

        |Ayar|Değer|
        |---|---|
        |Ad|myRouteTable-genel|
        |Abonelik|Aboneliğinizi seçin|
        |Kaynak grubu|Seçin **var olanı kullan**seçeneğini belirleyip **myResourceGroup**|
        |Konum|Doğu ABD|
    
    - 8. adım, önceki alt adımlar yeniden tamamlayın, ancak yol tablosu adı *myRouteTable özel*.
9. İçin yollar ekleme *myRouteTable ortak* rota tablosu ve rota tablosuna ilişkilendirmek *ortak* alt ağ:

    - Üzerinde **arama kaynakları** kutusuna portalın en üstünde *myRouteTable ortak*. Tıklatın **myRouteTable ortak** arama sonuçlarında görüntülendiğinde.
    - Altında **myRouteTable ortak**, tıklatın **yollar** listesinde **ayarları**.
    - Tıklatın **+ Ekle** altında **myRouteTable genel - yollar**.
    -  Varsayılan olarak, Azure yollar sanal ağ içindeki tüm alt ağlar arasında trafiği. Azure'nın varsayılan, gelen trafik için yönlendirme değiştirmek için bir yol oluşturma *ortak* alt yönlendirilir NVA yerine doğrudan *özel* alt ağ. Aşağıdaki değerleri girin **Add yolun** görünür ve ardından dikey **Tamam**:

        |Ayar|Değer|Açıklama|
        |---|---|---|
        |Rota adı|ToPrivateSubnet|Bu rota ağ sanal gereç aracılığıyla özel alt ağ trafiğini yönlendirir.|
        |Adres ön eki|10.0.1.0/24| Bu adres ön eki içindeki tüm adreslere gönderilen tüm trafik (10.0.1.0 - 10.0.1.254) ağ sanal gereç gönderilir.|
        |Sonraki atlama türü| Seçin **sanal Gereci**||
        |Sonraki atlama adresi|10.0.2.4| Statik özel IP adresi ağ sanal gereç bağlı ağ arabirimi. İçin bir adres belirtebilirsiniz yalnızca atlama türü **sanal Gereci**.|

    - Altında **myRouteTable ortak**, tıklatın **alt ağlar** altında **ayarları**. 
    - Tıklatın **+ ilişkilendir** altında **myRouteTable Genel - alt ağlar**.
    - Tıklatın **sanal ağ** altında **ilişkilendirmek alt**, ardından **myVnet**.
    - ' I tıklatın **alt** altında **ilişkilendirmek alt**, ardından **ortak** altında **alt ağ seçin**. 
    - Yapılandırmayı kaydetmek için tıklatın **Tamam** altında **ilişkilendirmek alt**. Bir alt ağ için ilişkili sıfır veya bir yol tablosu olabilir.
10. Arama tamamlandı adımında 9 yeniden **myRouteTable özel**, aşağıdaki ayarlarla bir rota oluşturma ve ardından yönlendirme tablosuna ilişkilendirme **özel** alt **myVnet** sanal ağ:

    |Ayar|Değer|Açıklama|
    |---|---|---|
    |Rota adı|ToPublicSubnet|Bu rota ağ sanal gereç aracılığıyla ortak alt ağ trafiğini yönlendirir.|
    |Adres ön eki|10.0.0.0/24| Bu adres ön eki içindeki tüm adreslere gönderilen tüm trafik (10.0.0.0 - 10.0.1.254) ağ sanal gereç gönderilir.|
    |Sonraki atlama türü| Seçin **sanal Gereci**||
    |Sonraki atlama adresi|10.0.2.4||

    Tüm kaynakları özel ve ortak alt arasındaki ağ trafiğini ağ sanal gereç aracılığıyla akar. 
11. **İsteğe bağlı:** ortak ve özel alt ağların bir sanal makine oluşturun ve sanal makineler arasındaki iletişim ağ sanal gereç aracılığıyla içindeki adımları tamamlayarak yönlendirilen doğrulamak [yönlendirmeDoğrula](#validate-routing). 
12. **İsteğe bağlı**: içindeki adımları tamamlayarak bu öğreticide oluşturduğunuz kaynakları silmek [silmek kaynakları](#delete-resources).

## <a name="validate-routing"></a>Yönlendirme doğrula

1. Henüz yapmadıysanız, bölümündeki adımları tamamlamanız [oluşturma yollar ve ağ sanal gereç](#create-routes-and-network-virtual-appliance).
2. Tıklatın **deneyin** Azure bulut Kabuğu'nu açar, aşağıdaki kutusu düğmesini. İstenirse, Azure kullanarak oturum açtığınız, [Azure hesabı](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Azure hesabınız yoksa [ücretsiz denemeye](https://azure.microsoft.com/offers/ms-azr-0044p) kaydolabilirsiniz. Azure bulut Kabuk önceden Azure komut satırı arabirimi ücretsiz bash kabuğunda ' dir. 

    Aşağıdaki komut, birinde iki sanal makine oluşturma *ortak* alt ağı ve birinde *özel* alt ağ. Komut ayrıca ağ arabirimi üzerinden trafiği yönlendirmek IP iletme nva'nın işletim sistemi etkinleştirmek için işletim sistemi içinde ağ arabirimi için etkinleştirin. Genellikle üretim NVA yönlendirme önce trafiği inceler, ancak bu öğreticide, basit nva'nın yalnızca trafiği incelemek olmadan yönlendirir. 

    Tıklatın **kopya** düğmesini **Linux** veya **Windows** izleyin ve komut dosyası içeriğini bir metin düzenleyicisine yapıştırın, komut dosyaları. Parolasını değiştirme *Admınpassword* değişkeni sonra Azure bulut Kabuk içine yapıştırma komut dosyası. Seçili ağ sanal gereç yordamının 6. adımında oluşturduğunuz zaman işletim sistemi için komut dosyasını çalıştırmak [oluşturma yollar ve ağ sanal gereç](#create-routes-and-network-virtual-appliance). 

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

Bu öğreticiyi tamamladığınızda, böylece kullanım ücretlerine tabi yok, oluşturduğunuz kaynakları silmek isteyebilirsiniz. Bir kaynak grubunun silinmesi, kaynak grubunda bulunan tüm kaynakları siler. Portal açıkken, aşağıdaki adımları tamamlayın:

1. Portal arama kutusuna **myResourceGroup**. Arama sonuçlarında tıklatın **myResourceGroup**.
2. Üzerinde **myResourceGroup** dikey penceresinde tıklatın **silmek** simgesi.
3. İçinde silmeyi onaylamak için **türü kaynak grubu adı** kutusuna **myResourceGroup**ve ardından **silmek**.

## <a name="next-steps"></a>Sonraki adımlar

- Oluşturma bir [yüksek oranda kullanılabilir bir ağ sanal Gereci](/azure/architecture/reference-architectures/dmz/nva-ha?toc=%2fazure%2fvirtual-network%2ftoc.json).
- Ağ sanal Gereçleri genellikle birden çok ağ arabirimleri ve atanmış IP adresleri vardır. Bilgi nasıl [ağ arabirimlerini varolan bir sanal makineye ekleyin](virtual-network-network-interface-vm.md#vm-add-nic) ve [var olan bir ağ arabirimi için IP adreslerini ekleyin](virtual-network-network-interface-addresses.md#add-ip-addresses). Tüm sanal makine boyutlarını kendisine bağlı en az iki ağ arabirimi kurabiliyor olsa bile, her sanal makine boyutu ağ arabirimleri sayısı üst destekler. Kaç tane ağ arabirimlerinin her sanal makine boyutu destekler bilgi edinmek için [Windows](../virtual-machines/windows/sizes.md?toc=%2Fazure%2Fvirtual-network%2Ftoc.json) ve [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) sanal makine boyutları. 
- Tüneli trafik şirket içi aracılığıyla zorlamak için kullanıcı tanımlı bir yol oluşturma bir [siteden siteye VPN bağlantısı](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
