---
title: "Birden çok alt ağı ile bir Azure sanal ağ oluşturma | Microsoft Docs"
description: "Birden fazla alt ağı azure'da bir sanal ağ oluşturmayı öğrenin."
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 4ad679a4-a959-4e48-a317-d9f5655a442b
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/26/2017
ms.author: jdial
ms.custom: 
ms.openlocfilehash: f82a95ec9543b2d53ef28bf7f15315e23cf4893a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-virtual-network-with-multiple-subnets"></a>Birden çok alt ağı ile bir sanal ağ oluşturma

Bu öğreticide, ayrı ortak ve özel alt ağları olan temel bir Azure sanal ağ oluşturmayı öğrenin. Sanal ağlar kaynaklarında birbirleriyle ve diğer ağlarda, sanal bir ağa bağlı kaynaklar ile iletişim kurabilir. Sanal makineler, uygulama hizmeti ortamları, sanal makine ölçek kümeleri, Azure Hdınsight ve bulut Hizmetleri gibi Azure kaynaklarını bir sanal ağ içindeki aynı veya farklı alt ağlarda oluşturabilirsiniz. Farklı alt ağlarda kaynakları oluşturma sağlar, bağımsız olarak sahip alt ağlara ve kapatma, filtre ağ trafiği için [ağ güvenlik grubu](virtual-networks-create-nsg-arm-pportal.md)ve [alt ağlar arasında trafiği yönlendirmek](virtual-network-create-udr-arm-ps.md) ağ üzerinden isterseniz bir güvenlik duvarı gibi sanal gereçler. 

Aşağıdaki bölümlerde kullanarak bir sanal ağ oluşturmak için uygulayabileceğiniz adımlar içerir [Azure portal](#portal), Azure komut satırı arabirimi ([Azure CLI](#azure-cli)), [Azure PowerShell](#powershell)ve bir [Azure Resource Manager şablonu](#resource-manager-template). Sonuç, hangi aracı kullanırsanız, sanal ağ oluşturmak için kullandığınız aynıdır. Öğreticinin bu bölüme gitmek için bir aracı bağlantısına tıklayın. Tüm hakkında daha fazla bilgi [sanal ağ](virtual-network-manage-network.md) ve [alt](virtual-network-manage-subnet.md) ayarlar.

Bu makale, yeni sanal ağları oluştururken kullanmanızı öneririz dağıtım modeli Resource Manager dağıtım modeli aracılığıyla bir sanal ağ oluşturmak için adımları sağlar. Sanal ağ (Klasik) oluşturmak ihtiyacınız varsa bkz [bir sanal ağ (Klasik) oluşturmak](create-virtual-network-classic.md). Azure'nın dağıtım modeliyle bilmiyorsanız bkz [anlamak Azure dağıtım modelleri](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="portal"></a>Azure portalı

1. Bir Internet tarayıcısında Git [Azure portal](https://portal.azure.com). Kullanarak oturum açın, [Azure hesabı](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Azure hesabınız yoksa [ücretsiz denemeye](https://azure.microsoft.com/offers/ms-azr-0044p) kaydolabilirsiniz.
2. Portalı'nda tıklatın **+ yeni** > **ağ** > **sanal ağ**.
3. Üzerinde **sanal ağ oluştur** dikey penceresinde, aşağıdaki değerleri girin ve ardından **oluşturma**:

    |Ayar|Değer|
    |---|---|
    |Ad|myVnet|
    |Adres alanı|10.0.0.0/16|
    |Alt ağ adı|Genel|
    |Alt ağ adres aralığı|10.0.0.0/24|
    |Kaynak grubu|Bırakın **Yeni Oluştur** seçili yazıp enter **myResourceGroup**.|
    |Abonelik ve konum|Abonelik ve konum seçin.

    Hakkında daha fazla bilgi için Azure yeniyseniz, [kaynak grupları](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group), [abonelikleri](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription), ve [konumları](https://azure.microsoft.com/regions) (olarak da adlandırılan *bölgeleri*).
4. Bir sanal ağ oluşturduğunuzda, portalda, yalnızca bir alt ağ oluşturabilirsiniz. Sanal ağ oluşturduktan sonra Bu öğreticide, ikinci bir alt ağ oluşturun. Daha sonra Internet'ten erişilebilen kaynaklara oluşturabilirsiniz **ortak** alt ağ. Internet'ten erişilemez kaynakları de oluşturabilir **özel** alt ağ. İkinci alt ağ oluşturmak için **arama kaynakları** kutusuna sayfanın en üstünde **myVnet**. Arama sonuçlarında tıklatın **myVnet**. Aboneliğinizde aynı ada sahip birden çok sanal ağlarınız varsa, her sanal ağ altında listelenen kaynak gruplarını denetleyin. Tıklattığınızdan emin **myVnet** arama kaynak Grubu'na sonucu **myResourceGroup**.
5. Üzerinde **myVnet** dikey altında **ayarları**, tıklatın **alt ağlar**.
6. Üzerinde **myVnet - alt ağlar** dikey penceresinde tıklatın **+ alt**.
7. Üzerinde **alt ağ Ekle** dikey penceresinde için **adı**, girin **özel**. İçin **adres aralığı**, girin **10.0.1.0/24**.  **Tamam** düğmesine tıklayın.
8. Üzerinde **myVnet - alt ağlar** dikey penceresinde alt ağların gözden geçirin. Gördüğünüz **ortak** ve **özel** oluşturduğunuz alt ağlar.
9. **İsteğe bağlı:** tamamlamak altında listelenen ek öğreticileri [sonraki adımlar](#next-steps) ve kapatma, her alt ağının ağ sanal gereç aracılığıyla alt ağlar arasında trafiği yönlendirmek için ağ güvenlik gruplarıyla ağ trafiğine filtre uygulamak için , veya diğer sanal ağları veya şirket içi ağlarda sanal ağa bağlanmak için.
10. **İsteğe bağlı:** içindeki adımları tamamlayarak bu öğreticide oluşturduğunuz kaynakları silmek [silmek kaynakları](#delete-portal).

## <a name="azure-cli"></a>Azure CLI

Windows, Linux veya macOS komutlar yürütülürken olup azure CLI komutları aynıdır. Ancak, işletim sistemi Kabukları arasında komut dosyası farklar vardır. Komut dosyası aşağıdaki adımlarda, bir Bash kabuğunda yürütür. 

1. [Azure CLI'yi yükleme ve yapılandırma](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Yüklü Azure CLI'ın en son sürümüne sahip olun. CLI komutları için Yardım almak için yazın `az <command> --help`. CLI ve ön koşullar yüklemek yerine, Azure bulut Kabuğu'nu kullanabilirsiniz. Azure Cloud Shell doğrudan Azure portalının içinde çalıştırabileceğiniz ücretsiz bir Bash kabuğudur. Bulut Kabuk Azure önceden yüklenmiş ve yapılandırılmış hesabınızla birlikte kullanmak için CLI sahiptir. Bulut Kabuğu'nu kullanmak için bulut Kabuğu'nu tıklatın. (**> _**) düğmesini en üstündeki [portal](https://portal.azure.com) veya tıklatmanız *deneyin* adımları düğmesini. 
2. CLI yerel olarak çalışıyorsa, Azure ile oturum `az login` komutu. Bulut Kabuğu'nu kullanarak, zaten oturum açtınız.
3. Aşağıdaki komut dosyası ve onun açıklamaları gözden geçirin. Tarayıcınızda komut dosyasını kopyalayın ve CLI oturumunuza yapıştırın:

    ```azurecli-interactive
    #!/bin/bash
    
    # Create a resource group.
    az group create \
      --name myResourceGroup \
      --location eastus
    
    # Create a virtual network with one subnet named Public.
    az network vnet create \
      --name myVnet \
      --resource-group myResourceGroup \
      --subnet-name Public
    
    # Create an additional subnet named Private in the virtual network.
    az network vnet subnet create \
      --name Private \
      --address-prefix 10.0.1.0/24 \
      --vnet-name myVnet \
      --resource-group myResourceGroup
    ```
    
4. Betik tamamlandığında çalışan, sanal ağ alt ağlarını gözden geçirin. Aşağıdaki komutu kopyalayın ve ardından CLI oturumunuza yapıştırın:

    ```azurecli
    az network vnet subnet list --resource-group myResourceGroup --vnet-name myVnet --output table
    ```

5. **İsteğe bağlı:** tamamlamak altında listelenen ek öğreticileri [sonraki adımlar](#next-steps) ve kapatma, her alt ağının ağ sanal gereç aracılığıyla alt ağlar arasında trafiği yönlendirmek için ağ güvenlik gruplarıyla ağ trafiğine filtre uygulamak için , veya diğer sanal ağları veya şirket içi ağlarda sanal ağa bağlanmak için.
6. **İsteğe bağlı**: içindeki adımları tamamlayarak bu öğreticide oluşturduğunuz kaynakları silmek [silmek kaynakları](#delete-cli).

## <a name="powershell"></a>PowerShell

1. PowerShell [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) modülünün en son sürümünü yükleyin. Azure PowerShell'i kullanmaya yeni başladıysanız [Azure PowerShell'e genel bakış](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json) sayfasını inceleyin.
2. Bir PowerShell oturumunda Azure ile oturum açın, [Azure hesabı](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account) kullanarak `login-azurermaccount` komutu.

3. Aşağıdaki komut dosyası ve onun açıklamaları gözden geçirin. Tarayıcınızda komut dosyasını kopyalayın ve PowerShell oturumunuza yapıştırın:

    ```powershell
    # Create a resource group.
    New-AzureRmResourceGroup `
      -Name myResourceGroup `
      -Location eastus
    
    # Create the public and private subnets.
    $Subnet1 = New-AzureRmVirtualNetworkSubnetConfig `
      -Name Public `
      -AddressPrefix 10.0.0.0/24
    $Subnet2 = New-AzureRmVirtualNetworkSubnetConfig `
      -Name Private `
      -AddressPrefix 10.0.1.0/24
    
    # Create a virtual network.
    $Vnet=New-AzureRmVirtualNetwork `
      -ResourceGroupName myResourceGroup `
      -Location eastus `
      -Name myVnet `
      -AddressPrefix 10.0.0.0/16 `
      -Subnet $Subnet1,$Subnet2
    ```

4. Sanal ağ alt ağlarını gözden geçirmek için aşağıdaki komutu kopyalayın ve PowerShell oturumunuza yapıştırın:

    ```powershell
    $Vnet.subnets | Format-Table Name, AddressPrefix
    ```

5. **İsteğe bağlı:** tamamlamak altında listelenen ek öğreticileri [sonraki adımlar](#next-steps) ve kapatma, her alt ağının ağ sanal gereç aracılığıyla alt ağlar arasında trafiği yönlendirmek için ağ güvenlik gruplarıyla ağ trafiğine filtre uygulamak için , veya diğer sanal ağları veya şirket içi ağlarda sanal ağa bağlanmak için.
6. **İsteğe bağlı**: içindeki adımları tamamlayarak bu öğreticide oluşturduğunuz kaynakları silmek [silmek kaynakları](#delete-powershell).

## <a name="resource-manager-template"></a>Resource Manager şablonu

Bir Azure Resource Manager şablonu kullanarak bir sanal ağ dağıtabilirsiniz. Şablonlar hakkında daha fazla bilgi edinmek için [Resource Manager nedir](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#template-deployment). Şablon erişmek ve parametreleri hakkında bilgi edinmek için bkz: [iki alt ağa sahip bir sanal ağ oluşturma](https://azure.microsoft.com/resources/templates/101-vnet-two-subnets/) şablonu. Kullanarak şablonu dağıtabilirsiniz [portal](#template-portal), [Azure CLI](#template-cli), veya [PowerShell](#template-powershell).

Şablon dağıttıktan sonra isteğe bağlı adımlar:

1. Tamamlamak altında listelenen ek öğreticileri [sonraki adımlar](#next-steps) her alt ağ bir ağ sanal gereç aracılığıyla alt ağlar arasında trafiği yönlendirmek için ağ güvenlik gruplarıyla ve dışındaki ağ trafiğini filtrelemek için veya sanal bağlanmak için diğer sanal ağlar ve şirket içi ağlara ağ.
2. Tüm alt içindeki adımları tamamlayarak bu öğreticide oluşturduğunuz kaynakları silmek [silmek kaynakları](#delete).

### <a name="template-portal"></a>Azure portalı

1. Tarayıcınızda açın [şablon sayfasına](https://azure.microsoft.com/resources/templates/101-vnet-two-subnets).
2. Tıklatın **Azure'a Dağıt** düğmesi. Azure'a zaten oturum açtınız, Azure portalı oturum açma ekranında oturum açın.
3. Portalı kullanarak oturum açın, [Azure hesabı](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Azure hesabınız yoksa [ücretsiz denemeye](https://azure.microsoft.com/offers/ms-azr-0044p) kaydolabilirsiniz.
4. Aşağıdaki değerleri parametrelerini girin:

    |Parametre|Değer|
    |---|---|
    |Abonelik|Aboneliğinizi seçin|
    |Kaynak grubu|myResourceGroup|
    |Konum|Konum seçin|
    |Vnet adı|myVnet|
    |Vnet adres ön eki|10.0.0.0/16|
    |Subnet1Prefix|10.0.0.0/24|
    |Subnet1Name|Genel|
    |Subnet2Prefix|10.0.1.0/24|
    |Subnet2Name|Özel|

5. Hüküm ve koşulları kabul edin ve ardından **satın alma** sanal ağı dağıtmak için.

### <a name="template-cli"></a>Azure CLI

1. [Azure CLI'yi yükleme ve yapılandırma](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Yüklü Azure CLI'ın en son sürümüne sahip olun. CLI komutları için Yardım almak için yazın `az <command> --help`. CLI ve ön koşullar yüklemek yerine, Azure bulut Kabuğu'nu kullanabilirsiniz. Azure Cloud Shell doğrudan Azure portalının içinde çalıştırabileceğiniz ücretsiz bir Bash kabuğudur. Bulut Kabuk Azure önceden yüklenmiş ve yapılandırılmış hesabınızla birlikte kullanmak için CLI sahiptir. Bulut Kabuğu'nu kullanmak için bulut Kabuğu'nu tıklatın. **> _** en üstündeki düğmesi [portal](https://portal.azure.com), veya tıklatmanız **deneyin** adımları düğmesini. 
2. CLI yerel olarak çalışıyorsa, Azure ile oturum `az login` komutu. Bulut Kabuğu'nu kullanarak, zaten oturum açtınız.
3. Sanal ağ için bir kaynak grubu oluşturmak için aşağıdaki komutu kopyalayın ve CLI oturumunuza yapıştırın:

    ```azurecli-interactive
    az group create --name myResourceGroup --location eastus
    ```
    
4. Aşağıdaki parametreleri seçeneklerden birini kullanarak şablonu dağıtabilirsiniz:
    - **Varsayılan parametre değerlerini**. Aşağıdaki komutu girin:
    
        ```azurecli-interactive
        az group deployment create --resource-group myResourceGroup --name VnetTutorial --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vnet-two-subnets/azuredeploy.json`
        ```
    - **Özel parametre değerlerini**. Karşıdan yükle ve şablonunun dağıtmadan önce şablonu değiştirin. Ayrıca komut satırında parametreleri kullanarak şablonu dağıtmak, veya ayrı parametreleri dosyasıyla şablon dağıtma. Şablonu ve parametre dosyalarını indirmek için tıklayın **Github'da Gözat** düğmesini [iki alt ağa sahip bir sanal ağ oluşturma](https://azure.microsoft.com/resources/templates/101-vnet-two-subnets/) şablon sayfası. Github'da, tıklatın **azuredeploy.parameters.json** veya **azuredeploy.json** dosya. Ardından **Raw** dosyayı görüntülemek için düğmesini. Tarayıcınızda, dosyanın içeriğini kopyalayın. İçeriği bir dosyayı bilgisayarınıza kaydedin. Şablondaki parametre değerlerini değiştirmek veya ayrı parametreleri dosyasıyla şablonu dağıtabilirsiniz.  

    Bu yöntemleri kullanarak şablonları dağıtma hakkında daha fazla bilgi edinmek için şunu yazın `az group deployment create --help`.

### <a name="template-powershell"></a>PowerShell

1. PowerShell [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) modülünün en son sürümünü yükleyin. Azure PowerShell'i kullanmaya yeni başladıysanız [Azure PowerShell'e genel bakış](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json) sayfasını inceleyin.
2. Oturum açmak için bir PowerShell oturumunda, [Azure hesabı](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account), girin `login-azurermaccount`.
3. Sanal ağ için bir kaynak grubu oluşturmak için aşağıdaki komutu girin:

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location eastus
    ```
    
4. Aşağıdaki parametreleri seçeneklerden birini kullanarak şablonu dağıtabilirsiniz:
    - **Varsayılan parametre değerlerini**. Aşağıdaki komutu girin:
    
        ```powershell
        New-AzureRmResourceGroupDeployment -Name VnetTutorial -ResourceGroupName myResourceGroup -TemplateUri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vnet-two-subnets/azuredeploy.json
        ```
        
    - **Özel parametre değerlerini**. Karşıdan yükle ve dağıtmadan önce şablonu değiştirin. Ayrıca komut satırında parametreleri kullanarak şablonu dağıtmak, veya ayrı parametreleri dosyasıyla şablon dağıtma. Şablonu ve parametre dosyalarını indirmek için tıklayın **Github'da Gözat** düğmesini [iki alt ağa sahip bir sanal ağ oluşturma](https://azure.microsoft.com/resources/templates/101-vnet-two-subnets/) şablon sayfası. Github'da, tıklatın **azuredeploy.parameters.json** veya **azuredeploy.json** dosya. Ardından **Raw** dosyayı görüntülemek için düğmesini. Tarayıcınızda, dosyanın içeriğini kopyalayın. İçeriği bir dosyayı bilgisayarınıza kaydedin. Şablondaki parametre değerlerini değiştirmek veya ayrı parametreleri dosyasıyla şablonu dağıtabilirsiniz.  

    Bu yöntemleri kullanarak şablonları dağıtma hakkında daha fazla bilgi edinmek için şunu yazın `Get-Help New-AzureRmResourceGroupDeployment`. 

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

## <a name="next-steps"></a>Sonraki adımlar

- Tüm sanal ağ ve alt ağ ayarları hakkında bilgi edinmek için bkz: [sanal ağlarını yönetmeleri](virtual-network-manage-network.md#view-vnet) ve [sanal ağ alt ağlarını yönetin](virtual-network-manage-subnet.md#create-subnet). Sanal ağlar ve alt ağları farklı gereksinimlerini karşılamak için bir üretim ortamında kullanmadan yönelik çeşitli seçenekleriniz vardır.
- Gelen ve giden alt ağ trafiği oluşturma ve uygulama filtre [ağ güvenlik grubu](virtual-networks-nsg.md) alt ağlara.
- Rota üzerinden oluşturarak ağ sanal gereç, alt ağlar arasında trafiği [kullanıcı tanımlı yollar](virtual-network-create-udr-arm-ps.md) ve her alt ağ için yollar uygulayın.
- Oluşturma bir [Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) var olan bir sanal ağdaki sanal makine.
- Oluşturarak iki sanal ağlara bağlanabilir bir [sanal ağ eşlemesi](virtual-network-peering-overview.md) sanal ağlar arasında.
- Sanal ağ kullanarak bir şirket ağına bağlanan bir [VPN ağ geçidi](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [Azure ExpressRoute](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md?toc=%2fazure%2fvirtual-network%2ftoc.json) hattı.
