---
title: Bir Azure sanal ağı - farklı dağıtım modellerini - eşlemesi oluşturmak aynı abonelik | Microsoft Docs
description: Aynı Azure aboneliğindeki mevcut farklı Azure dağıtım modelleri aracılığıyla oluşturulan sanal ağlar arasında eşleme sanal ağ oluşturmayı öğrenin.
services: virtual-network
documentationcenter: ''
author: jimdial
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: jdial;anavin
ms.openlocfilehash: 2ab027c1159fec369aa7377a24ddd9ef330eab5e
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="create-a-virtual-network-peering---different-deployment-models-same-subscription"></a>Sanal Ağ eşlemesi bir - farklı oluşturmak dağıtım modelleri, aynı abonelik 

Bu öğreticide, farklı dağıtım modeli oluşturulan sanal ağlar arasında eşleme sanal ağ oluşturmayı öğrenin. Her iki sanal ağlar aynı abonelikte yok. Kaynaklar aynı sanal ağda gibi davranarak birbirleri ile aynı bant genişliği ve gecikme süresi ile iletişim kurmak için farklı sanal ağlar iki sanal ağlar etkinleştirir kaynaklarında eşleme. Daha fazla bilgi edinmek [sanal ağ eşlemesi](virtual-network-peering-overview.md). 

Sanal ağlar aynı ya da farklı olup, abonelikleri ve hangi bağlı olarak farklı bir sanal ağ eşlemesi oluşturmak için aşağıdaki adımları [Azure dağıtım modeli](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) sanal ağlar üzerinden oluşturulur. Aşağıdaki tabloda senaryodan tıklayarak diğer senaryolarda eşliği bir sanal ağ oluşturmayı öğrenin:

|Azure dağıtım modeli  | Azure aboneliği  |
|--------- |---------|
|[Her iki kaynak yöneticisi](tutorial-connect-virtual-networks-portal.md) |Aynı|
|[Her iki kaynak yöneticisi](create-peering-different-subscriptions.md) |Fark|
|[Bir kaynak yöneticisi, bir Klasik](create-peering-different-deployment-models-subscriptions.md) |Fark|

Klasik dağıtım modeli aracılığıyla dağıtılan iki sanal ağ arasında bir sanal ağ eşlemesi oluşturulamıyor. Klasik dağıtım modeli aracılığıyla her ikisi de oluşturulan sanal ağlara bağlanmak gerekiyorsa, Azure kullanabilirsiniz [VPN ağ geçidi](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) sanal ağlara bağlanma. 

Bu öğretici, sanal ağlar aynı bölgede eşlere. Sanal ağlar farklı bölgelerde eş yeteneği şu anda önizlemede değil. Bölümündeki adımları tamamlamanız [kaydetmek genel sanal ağ eşlemesi için](#register) sanal ağlar farklı bölgelerde veya eşleme başarısız eş denemeden önce. İle bir Azure sanal ağlar farklı bölgelerde bağlanma özelliği [VPN ağ geçidi](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) genel olarak kullanılabilir ve kayıt gerektirmez.

Kullanabileceğiniz [Azure portal](#portal), Azure [komut satırı arabirimi](#cli) (CLI) Azure [PowerShell](#powershell), veya bir [Azure Resource Manager şablonu](#template)bir sanal ağ eşlemesi oluşturmak için. Doğrudan seçeceğiniz araç kullanarak bir sanal ağ eşlemesi oluşturma adımlarını gitmek için önceki aracı bağlantılardan herhangi birine tıklayın.

## <a name="create-peering---azure-portal"></a>Eşlemesi - oluşturmak Azure portalı

1. [Azure Portal](https://portal.azure.com)’da oturum açın. İle oturum için kullandığınız hesabın, bir sanal ağ eşlemesi oluşturmak için gerekli izinleri olmalıdır. Bkz: [izinleri](#permissions) Ayrıntılar için bu makalenin.
2. Tıklatın **+ yeni**, tıklatın **ağ**, ardından **sanal ağ**.
3. İçinde **sanal ağ oluştur** dikey penceresinde girin veya aşağıdaki ayarları için değerleri seçin ve ardından **oluşturma**:
    - **Ad**: *myVnet1*
    - **Adres alanı**: *10.0.0.0/16*
    - **Alt ağ adı**: *varsayılan*
    - **Alt ağ adres aralığı**: *10.0.0.0/24*
    - **Abonelik**: aboneliğinizi seçin
    - **Kaynak grubu**: seçin **Yeni Oluştur** ve girin *myResourceGroup*
    - **Konum**: *Doğu ABD*
4. **+ Yeni** öğesine tıklayın. İçinde **Market arama** kutusuna *sanal ağ*. Tıklatın **sanal ağ** arama sonuçlarında görüntülendiğinde. 
5. İçinde **sanal ağ** dikey penceresinde, select **Klasik** içinde **dağıtım modeli seçin** kutusuna ve ardından **oluşturma**.
6. İçinde **sanal ağ oluştur** dikey penceresinde girin veya aşağıdaki ayarları için değerleri seçin ve ardından **oluşturma**:
    - **Ad**: *myVnet2*
    - **Adres alanı**: *10.1.0.0/16*
    - **Alt ağ adı**: *varsayılan*
    - **Alt ağ adres aralığı**: *10.1.0.0/24*
    - **Abonelik**: aboneliğinizi seçin
    - **Kaynak grubu**: seçin **var olanı kullan** seçip *myResourceGroup*
    - **Konum**: *Doğu ABD*
7. İçinde **arama kaynakları** kutusunu türü portalı üstündeki *myResourceGroup*. Tıklatın **myResourceGroup** arama sonuçlarında görüntülendiğinde. Bir dikey pencere görünür **myresourcegroup** kaynak grubu. Kaynak grubu, önceki adımlarda oluşturduğunuz iki sanal ağ içeriyor.
8. Tıklatın **myVNet1**.
9. İçinde **myVnet1** görünür, dikey tıklayın **eşlemeler** dikey pencerenin sol tarafındaki seçenekleri dikey listesinden.
10. İçinde **myVnet1 - eşlemeler** görünen dikey tıklayın **+ Ekle**
11. İçinde **Ekle eşliği** görünür, dikey girin veya aşağıdaki seçenekleri belirleyin ve ardından **Tamam**:
     - **Name**: *myVnet1ToMyVnet2*
     - **Sanal ağ dağıtım modeli**: seçin **Klasik**. 
     - **Abonelik**: aboneliğinizi seçin
     - **Sanal ağ**: tıklatın **sanal ağ seçin**, ardından **myVnet2**.
     - **Sanal ağ erişimine izin ver:** emin **etkin** seçilir.
    Diğer bir ayarları, bu öğreticide kullanılır. Tüm eşleme ayarları hakkında bilgi için okuma [sanal ağ eşlemeleri yönetme](virtual-network-manage-peering.md#create-a-peering).
12. ' I tıklattıktan sonra **Tamam** önceki adımda **Ekle eşliği** dikey penceresi kapanır ve gördüğünüz **myVnet1 - eşlemeler** dikey penceresini yeniden. Birkaç saniye sonra oluşturduğunuz eşliği dikey penceresinde görüntülenir. **Bağlı** içinde listelenen **EŞLİĞİ durumu** sütunu için **myVnet1ToMyVnet2** sizin eşlemeyi oluşturuldu.

    Eşlemeyi şimdi oluşturulur. Ya da sanal ağ içinde oluşturduğunuz Azure kaynaklarını IP adreslerini birbirleri ile iletişim kuramıyor. Varsayılan Azure ad çözümlemesi için sanal ağlar kullanıyorsanız, sanal ağlarda bulunan kaynaklar sanal ağlar arasında adlarını çözmek mümkün değildir. Bir eşlemesindeki sanal ağlar arasında adları çözümlemek dilerseniz kendi DNS sunucusu oluşturmanız gerekir. Nasıl ayarlanacağını öğrenin [kendi DNS sunucu kullanılarak ad çözümleme](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server).
13. **İsteğe bağlı**: sanal makineler oluştururken bu öğreticide kapsanmayan olsa, her sanal ağ içinde bir sanal makine oluşturun ve bir sanal makineden diğerine, bağlanabilirliği doğrulamak için bağlanın.
14. **İsteğe bağlı**: Bu öğreticide oluşturduğunuz kaynaklarını silmek için adımları tamamlamanız [silmek kaynakları](#delete-portal) bu makalenin.

## <a name="cli"></a>Eşlemesi - oluşturmak Azure CLI

1. [Yükleme](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) sanal ağ (Klasik) oluşturmak için Azure CLI 1.0.
2. Bir komut oturumu açın ve oturum açma Azure kullanmaya `azure login` komutu.
3. CLI girerek Hizmet Yönetimi modunda çalışacak `azure config mode asm` komutu.
4. Sanal ağ (Klasik) oluşturmak için aşağıdaki komutu girin:
 
    ```azurecli
    azure network vnet create --vnet myVnet2 --address-space 10.1.0.0 --cidr 16 --location "East US"
    ```

5. Bir kaynak grubu ve sanal ağ (Resource Manager) oluşturun. CLI 1.0 veya 2.0 kullanabilirsiniz ([yükleme](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json)). Bu öğreticide, CLI 2.0 2.0 eşlemesi oluşturmak için kullanılması gereken bu yana (Resource Manager) sanal ağ oluşturmak için kullanılır. CLI 2.0.4 CLI ile yerel makineniz betikten veya üstü yüklü aşağıdaki bash yürütün. Windows istemcisi üzerinde CLI betikleri çalıştırma seçenekleri bash için bkz: [Windows Azure CLI çalıştıran](../virtual-machines/windows/cli-options.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Azure bulut Kabuğu'nu kullanarak betik de çalıştırabilirsiniz. Azure Cloud Shell doğrudan Azure portalının içinde çalıştırabileceğiniz ücretsiz bir Bash kabuğudur. Azure CLI, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Tıklatın **deneyin** , oturumu bir bulut Kabuk çağırır aşağıdaki Azure hesabınızla oturum açabildiğinizden komut düğmesi. Betik yürütmek için tıklatın **kopya** düğmesi ve yapıştırma, bulut Kabuğunuzu içeriğine tuşuna `Enter`.

    ```azurecli-interactive
    #!/bin/bash

    # Create a resource group.
    az group create \
      --name myResourceGroup \
      --location eastus

    # Create the virtual network (Resource Manager).
    az network vnet create \
      --name myVnet1 \
      --resource-group myResourceGroup \
      --location eastus \
      --address-prefix 10.0.0.0/16
    ```

6. Farklı dağıtım modelleri arasında oluşturulan iki sanal ağlar arasında eşleme sanal ağ oluşturun. Aşağıdaki komut dosyası, bilgisayarınızdaki bir metin düzenleyicisine kopyalayın. Değiştir `<subscription id>` aboneliğinizle kimliği. Aboneliğinizi kimliği bilmiyorsanız, girin `az account show` komutu. Değeri **kimliği** çıktıda, abonelik kimliği olan CLI oturumunuz için değiştirilmiş betik yapıştırın ve sonra basın `Enter`.

    ```azurecli-interactive
    # Get the id for VNet1.
    vnet1Id=$(az network vnet show \
      --resource-group myResourceGroup \
      --name myVnet1 \
      --query id --out tsv)

    # Peer VNet1 to VNet2.
    az network vnet peering create \
      --name myVnet1ToMyVnet2 \
      --resource-group myResourceGroup \
      --vnet-name myVnet1 \
      --remote-vnet-id /subscriptions/<subscription id>/resourceGroups/Default-Networking/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnet2 \
      --allow-vnet-access
    ```
7. Betiği çalıştırdıktan sonra sanal ağ (Resource Manager) için eşleme gözden geçirin. Aşağıdaki komutu kopyalayın, CLI oturumunuzda yapıştırın ve ardından basın `Enter`:

    ```azurecli-interactive
    az network vnet peering list \
      --resource-group myResourceGroup \
      --vnet-name myVnet1 \
      --output table
    ```
    
    Çıktısında **bağlı** içinde **PeeringState** sütun. 

    Ya da sanal ağ içinde oluşturduğunuz Azure kaynaklarını IP adreslerini birbirleri ile iletişim kuramıyor. Varsayılan Azure ad çözümlemesi için sanal ağlar kullanıyorsanız, sanal ağlarda bulunan kaynaklar sanal ağlar arasında adlarını çözmek mümkün değildir. Bir eşlemesindeki sanal ağlar arasında adları çözümlemek dilerseniz kendi DNS sunucusu oluşturmanız gerekir. Nasıl ayarlanacağını öğrenin [kendi DNS sunucu kullanılarak ad çözümleme](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server).
8. **İsteğe bağlı**: sanal makineler oluştururken bu öğreticide kapsanmayan olsa, her sanal ağ içinde bir sanal makine oluşturun ve bir sanal makineden diğerine, bağlanabilirliği doğrulamak için bağlanın.
9. **İsteğe bağlı**: Bu öğreticide oluşturduğunuz kaynaklarını silmek için adımları tamamlamanız [silmek kaynakları](#delete-cli) bu makalede.

## <a name="powershell"></a>Eşlemesi - oluşturmak PowerShell

1. PowerShell'ın en son sürümünü yüklemek [Azure](https://www.powershellgallery.com/packages/Azure) ve [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) modüller. Azure PowerShell'i kullanmaya yeni başladıysanız [Azure PowerShell'e genel bakış](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json) sayfasını inceleyin.
2. Bir PowerShell oturumu başlatın.
3. PowerShell'de `Add-AzureAccount` komutunu girerek Azure oturumu açın.
4. PowerShell ile bir sanal ağ (Klasik) oluşturmak için yeni bir oluşturun veya varolan, bir ağ yapılandırma dosyasını değiştirmeniz gerekir. Bilgi edinmek için nasıl [dışarı aktarma, güncelleştirme ve ağ yapılandırma dosyalarını içe](virtual-networks-using-network-configuration-file.md). Dosya aşağıdaki içermelidir **VirtualNetworkSite** öğesi Bu öğreticide kullanılan sanal ağ için:

    ```xml
    <VirtualNetworkSite name="myVnet2" Location="East US">
      <AddressSpace>
        <AddressPrefix>10.1.0.0/16</AddressPrefix>
      </AddressSpace>
      <Subnets>
        <Subnet name="default">
          <AddressPrefix>10.1.0.0/24</AddressPrefix>
        </Subnet>
      </Subnets>
    </VirtualNetworkSite>
    ```

    > [!WARNING]
    > Değiştirilen ağ yapılandırma dosyasını içeri varolan sanal ağlar (aboneliğinizde Klasik) yapılan değişiklikler neden olabilir. Yalnızca önceki sanal ağ ekleyin ve verme değiştirir veya var olan tüm sanal ağları aboneliğinizden kaldırırsanız, emin olun. 
5. Oturum açtığınızda girerek (Resource Manager) sanal ağ oluşturmak için Azure `login-azurermaccount` komutu. İle oturum için kullandığınız hesabın, bir sanal ağ eşlemesi oluşturmak için gerekli izinleri olmalıdır. Bkz: [izinleri](#permissions) Ayrıntılar için bu makalenin.
6. Bir kaynak grubu ve sanal ağ (Resource Manager) oluşturun. Betiği kopyalayın, PowerShell içinde yapıştırın ve ardından basın `Enter`.

    ```powershell
    # Create a resource group.
      New-AzureRmResourceGroup -Name myResourceGroup -Location eastus

    # Create the virtual network (Resource Manager).
      $vnet1 = New-AzureRmVirtualNetwork `
      -ResourceGroupName myResourceGroup `
      -Name 'myVnet1' `
      -AddressPrefix '10.0.0.0/16' `
      -Location eastus
    ```

7. Farklı dağıtım modelleri arasında oluşturulan iki sanal ağlar arasında eşleme sanal ağ oluşturun. Aşağıdaki komut dosyası, bilgisayarınızdaki bir metin düzenleyicisine kopyalayın. Değiştir `<subscription id>` aboneliğinizle kimliği. Aboneliğinizi kimliği bilmiyorsanız, girin `Get-AzureRmSubscription` görüntülemek için komutu. Değeri **kimliği** döndürülen çıkış, abonelik kimliği. Betik yürütmek için metin düzenleyici, sonra sağ tıklatın, PowerShell oturumunda değiştirilmiş betiği kopyalayın ve tuşuna basın `Enter`.

    ```powershell
    # Peer VNet1 to VNet2.
    Add-AzureRmVirtualNetworkPeering `
      -Name myVnet1ToMyVnet2 `
      -VirtualNetwork $vnet1 `
      -RemoteVirtualNetworkId /subscriptions/<subscription Id>/resourceGroups/Default-Networking/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnet2
    ```

8. Betiği çalıştırdıktan sonra sanal ağ (Resource Manager) için eşleme gözden geçirin. Aşağıdaki komutu kopyalayın, PowerShell oturumunuzda yapıştırın ve ardından basın `Enter`:

    ```powershell
    Get-AzureRmVirtualNetworkPeering `
      -ResourceGroupName myResourceGroup `
      -VirtualNetworkName myVnet1 `
      | Format-Table VirtualNetworkName, PeeringState
    ```

    Çıktısında **bağlı** içinde **PeeringState** sütun.

    Ya da sanal ağ içinde oluşturduğunuz Azure kaynaklarını IP adreslerini birbirleri ile iletişim kuramıyor. Varsayılan Azure ad çözümlemesi için sanal ağlar kullanıyorsanız, sanal ağlarda bulunan kaynaklar sanal ağlar arasında adlarını çözmek mümkün değildir. Bir eşlemesindeki sanal ağlar arasında adları çözümlemek dilerseniz kendi DNS sunucusu oluşturmanız gerekir. Nasıl ayarlanacağını öğrenin [kendi DNS sunucu kullanılarak ad çözümleme](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server).

9. **İsteğe bağlı**: sanal makineler oluştururken bu öğreticide kapsanmayan olsa, her sanal ağ içinde bir sanal makine oluşturun ve bir sanal makineden diğerine, bağlanabilirliği doğrulamak için bağlanın.
10. **İsteğe bağlı**: Bu öğreticide oluşturduğunuz kaynaklarını silmek için adımları tamamlamanız [silmek kaynakları](#delete-powershell) bu makalede.
 
## <a name="permissions"></a>İzinleri

Bir sanal ağ eşlemesi oluşturmak için kullandığınız hesaplarının gerekli rol veya izinleri olması gerekir. Örneğin, myVnet1 ve myVnet2 adlı iki sanal ağ eşlemesi, hesabınızı aşağıdaki en düşük rolü veya her sanal ağ izinlerini atanmalıdır:
    
|Sanal ağ|Dağıtım modeli|Rol|İzinler|
|---|---|---|---|
|myVnet1|Resource Manager|[Ağ Katılımcısı](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write|
| |Klasik|[Klasik Ağ Katılımcısı](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|Yok|
|myVnet2|Resource Manager|[Ağ Katılımcısı](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/peer|
||Klasik|[Klasik Ağ Katılımcısı](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|Microsoft.ClassicNetwork/virtualNetworks/peer|

Daha fazla bilgi edinmek [yerleşik roller](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) ve belirli izinler atama [özel roller](../active-directory/role-based-access-control-custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (yalnızca Resource Manager).

## <a name="delete"></a>Kaynakları silin
Bu öğreticiyi tamamladığınızda, kullanım ücret ödememeniz öğreticide oluşturduğunuz kaynakları silmek isteyebilirsiniz. Bir kaynak grubunun silinmesi, kaynak grubunda bulunan tüm kaynakları siler.

### <a name="delete-portal"></a>Azure portalı

1. Portal arama kutusuna **myResourceGroup**. Arama sonuçlarında tıklatın **myResourceGroup**.
2. Üzerinde **myResourceGroup** dikey penceresinde tıklatın **silmek** simgesi.
3. İçinde silmeyi onaylamak için **türü kaynak grubu adı** kutusuna **myResourceGroup**ve ardından **silmek**.

### <a name="delete-cli"></a>Azure CLI

1. Azure CLI 2.0 aşağıdaki komutu kullanarak sanal ağ (Resource Manager) silmek için kullanın:

    ```azurecli-interactive
    az group delete --name myResourceGroup --yes
    ```

2. Azure CLI 1.0 aşağıdaki komutlarla sanal ağ (Klasik) silmek için kullanın:

    ```azurecli
    azure config mode asm

    azure network vnet delete --vnet myVnet2 --quiet
    ```

### <a name="delete-powershell"></a>PowerShell

1. Sanal ağ (Resource Manager) silmek için aşağıdaki komutu girin:

    ```powershell
    Remove-AzureRmResourceGroup -Name myResourceGroup -Force
    ```

2. Sanal ağı silmek için PowerShell ile (Klasik), mevcut bir ağ yapılandırma dosyasını değiştirmeniz gerekir. Bilgi edinmek için nasıl [dışarı aktarma, güncelleştirme ve ağ yapılandırma dosyalarını içe](virtual-networks-using-network-configuration-file.md). Bu öğreticide kullanılan sanal ağ için aşağıdaki VirtualNetworkSite öğeyi kaldırın:

    ```xml
    <VirtualNetworkSite name="myVnet2" Location="East US">
      <AddressSpace>
        <AddressPrefix>10.1.0.0/16</AddressPrefix>
      </AddressSpace>
      <Subnets>
        <Subnet name="default">
          <AddressPrefix>10.1.0.0/24</AddressPrefix>
        </Subnet>
      </Subnets>
    </VirtualNetworkSite>
    ```

    > [!WARNING]
    > Değiştirilen ağ yapılandırma dosyasını içeri varolan sanal ağlar (aboneliğinizde Klasik) yapılan değişiklikler neden olabilir. Önceki sanal ağı yalnızca kaldırmak ve verme değiştirir veya diğer varolan sanal ağlara aboneliğinizden kaldırırsanız, emin olun. 

## <a name="register"></a>Genel sanal ağ eşleme Önizleme için kaydolun

Aynı bölgedeki sanal ağları eşleme özelliği genel kullanıma açıktır. Sanal ağlar farklı bölgelerde şu anda önizlemede eşleme. Bkz: [sanal ağı güncelleştirmelerini](https://azure.microsoft.com/en-us/updates/?product=virtual-network) bölgeleri için kullanılabilir. Sanal ağlar bölgeler arasında eş için önce önizleme için (içinde eş istediğiniz her bir sanal ağı olduğundan, abonelik) aşağıdaki adımları tamamlayarak kaydetmeniz gerekir Azure PowerShell veya Azure CLI kullanarak:

### <a name="powershell"></a>PowerShell

1. PowerShell [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) modülünün en son sürümünü yükleyin. Azure PowerShell'i kullanmaya yeni başladıysanız [Azure PowerShell'e genel bakış](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json) sayfasını inceleyin.
2. Bir PowerShell oturumu başlatın ve oturum açma Azure kullanmaya `Login-AzureRmAccount` komutu.
3. Eş istediğiniz her sanal ağ içinde önizleme için aşağıdaki komutları girerek aboneliğinizin kaydedin:

    ```powershell
    Register-AzureRmProviderFeature `
      -FeatureName AllowGlobalVnetPeering `
      -ProviderNamespace Microsoft.Network
    
    Register-AzureRmResourceProvider `
      -ProviderNamespace Microsoft.Network
    ```
4. Aşağıdaki komutu girerek Önizleme için kayıtlı olduklarını doğrulayın:

    ```powershell    
    Get-AzureRmProviderFeature `
      -FeatureName AllowGlobalVnetPeering `
      -ProviderNamespace Microsoft.Network
    ```

    Bu makalenin kadar Portal, Azure CLI, PowerShell veya Resource Manager şablonu bölümlerdeki adımları tamamlamayın **RegistrationState** önceki komutunu girdikten sonra aldığınız çıktı **kayıtlı**  hem abonelikler için.

### <a name="azure-cli"></a>Azure CLI

1. [Azure CLI'yi yükleme ve yapılandırma](/cli/azure/install-azure-cli?toc=%2Fazure%2Fvirtual-network%2Ftoc.json).
2. Sürüm 2.0.18 kullandığınızdan emin olun ya da üst sürümünü girerek Azure CLI `az --version` komutu. Emin değilseniz, en son sürümünü yükleyin.
3. Oturum açtığınızda Azure ile `az login` komutu.
4. Önizleme için aşağıdaki komutları girerek kaydedin:

    ```azurecli-interactive
    az feature register --name AllowGlobalVnetPeering --namespace Microsoft.Network
    az provider register --name Microsoft.Network
    ```

5. Aşağıdaki komutu girerek Önizleme için kayıtlı olduklarını doğrulayın:

    ```azurecli-interactive
    az feature show --name AllowGlobalVnetPeering --namespace Microsoft.Network
    ```

    Bu makalenin kadar Portal, Azure CLI, PowerShell veya Resource Manager şablonu bölümlerdeki adımları tamamlamayın **RegistrationState** önceki komutunu girdikten sonra aldığınız çıktı **kayıtlı**  hem abonelikler için.

## <a name="next-steps"></a>Sonraki adımlar

- Baştan sona ile önemli öğrenmeniz [sanal ağ eşleme kısıtlamaları ve davranışları](virtual-network-manage-peering.md#requirements-and-constraints) için üretim eşleme sanal ağ oluşturmadan önce kullanın.
- Tüm hakkında bilgi edinin [sanal ağ eşleme ayarları](virtual-network-manage-peering.md#create-a-peering).
- Bilgi edinmek için nasıl [bir hub oluşturmak ve ağ topolojisine bağlı bileşen](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) sanal ağ eşlemesi ile.
