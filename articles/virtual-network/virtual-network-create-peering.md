---
title: Bir Azure sanal ağı - Resource Manager - eşlemesi oluşturmak aynı abonelik | Microsoft Docs
description: Aynı Azure aboneliğindeki mevcut Resource Manager aracılığıyla oluşturulan sanal ağlar arasında eşleme sanal ağ oluşturmayı öğrenin.
services: virtual-network
documentationcenter: ''
author: jimdial
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: 026bca75-2946-4c03-b4f6-9f3c5809c69a
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: anavin;jdial
ms.openlocfilehash: 70fe948070147c01922fab68fb55a0f00c26a0f3
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2018
ms.locfileid: "29767160"
---
# <a name="create-a-virtual-network-peering---resource-manager-same-subscription"></a>Bir sanal ağ eşlemesi - oluşturmak Resource Manager, aynı abonelik

Bu öğreticide, Resource Manager aracılığıyla oluşturulan sanal ağlar arasında eşleme sanal ağ oluşturmayı öğrenin. Her iki sanal ağlar aynı abonelikte yok. Kaynaklar aynı sanal ağda gibi davranarak birbirleri ile aynı bant genişliği ve gecikme süresi ile iletişim kurmak için farklı sanal ağlar iki sanal ağlar etkinleştirir kaynaklarında eşleme. Daha fazla bilgi edinmek [sanal ağ eşlemesi](virtual-network-peering-overview.md). 

Sanal ağlar aynı ya da farklı olup, abonelikleri ve hangi bağlı olarak farklı bir sanal ağ eşlemesi oluşturmak için aşağıdaki adımları [Azure dağıtım modeli](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) sanal ağlar üzerinden oluşturulur. Aşağıdaki tabloda senaryodan tıklayarak diğer senaryolarda eşliği bir sanal ağ oluşturmayı öğrenin:

|Azure dağıtım modeli  | Azure aboneliği  |
|--------- |---------|
|[Her iki kaynak yöneticisi](create-peering-different-subscriptions.md) |Fark|
|[Bir kaynak yöneticisi, bir Klasik](create-peering-different-deployment-models.md) |Aynı|
|[Bir kaynak yöneticisi, bir Klasik](create-peering-different-deployment-models-subscriptions.md) |Fark|

Klasik dağıtım modeli aracılığıyla dağıtılan iki sanal ağ arasında bir sanal ağ eşlemesi oluşturulamıyor. Klasik dağıtım modeli aracılığıyla her ikisi de oluşturulan sanal ağlara bağlanmak gerekiyorsa, Azure kullanabilirsiniz [VPN ağ geçidi](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) sanal ağlara bağlanma. 

Bu öğretici, sanal ağlar aynı bölgede eşlere. Sanal ağlar farklı bölgelerde eş yeteneği şu anda önizlemede değil. Bölümündeki adımları tamamlamanız [kaydetmek genel sanal ağ eşlemesi için](#register) sanal ağlar farklı bölgelerde veya eşleme başarısız eş denemeden önce. İle bir Azure sanal ağlar farklı bölgelerde bağlanma özelliği [VPN ağ geçidi](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) genel olarak kullanılabilir ve kayıt gerektirmez.

Kullanabileceğiniz [Azure portal](#portal), Azure [komut satırı arabirimi](#cli) (CLI) Azure [PowerShell](#powershell), veya bir [Azure Resource Manager şablonu](#template)bir sanal ağ eşlemesi oluşturmak için. Doğrudan seçeceğiniz araç kullanarak bir sanal ağ eşlemesi oluşturma adımlarını gitmek için önceki aracı bağlantılardan herhangi birine tıklayın.

## <a name="portal"></a>Eşlemesi - oluşturmak Azure portalı

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
4. Adım 3'te yeniden aşağıdaki değerleri belirtme 2-3 adımları tamamlayın:
    - **Ad**: *myVnet2*
    - **Adres alanı**: *10.1.0.0/16*
    - **Alt ağ adı**: *varsayılan*
    - **Alt ağ adres aralığı**: *10.1.0.0/24*
    - **Abonelik**: aboneliğinizi seçin
    - **Kaynak grubu**: seçin **var olanı kullan** seçip *myResourceGroup*
    - **Konum**: *Doğu ABD*
5. İçinde **arama kaynakları** kutusunu türü portalı üstündeki *myResourceGroup*. Tıklatın **myResourceGroup** arama sonuçlarında görüntülendiğinde. Bir dikey pencere görünür **myresourcegroup** kaynak grubu. Kaynak grubu, önceki adımlarda oluşturduğunuz iki sanal ağ içeriyor.
6. Tıklatın **myVNet1**.
7. İçinde **myVnet1** görünür, dikey tıklayın **eşlemeler** dikey pencerenin sol tarafındaki seçenekleri dikey listesinden.
8. İçinde **myVnet1 - eşlemeler** görünen dikey tıklayın **+ Ekle**
9. İçinde **Ekle eşliği** görünür, dikey girin veya aşağıdaki seçenekleri belirleyin ve ardından **Tamam**:
     - **Name**: *myVnet1ToMyVnet2*
     - **Sanal ağ dağıtım modeli**: seçin **Resource Manager**. 
     - **Abonelik**: aboneliğinizi seçin
     - **Sanal ağ**: tıklatın **sanal ağ seçin**, ardından **myVnet2**.
     - **Sanal ağ erişimine izin ver:** emin **etkin** seçilir.
    Diğer bir ayarları, bu öğreticide kullanılır. Tüm eşleme ayarları hakkında bilgi için okuma [sanal ağ eşlemeleri yönetme](virtual-network-manage-peering.md#create-a-peering).
10. ' I tıklattıktan sonra **Tamam** önceki adımda **Ekle eşliği** dikey penceresi kapanır ve gördüğünüz **myVnet1 - eşlemeler** dikey penceresini yeniden. Birkaç saniye sonra oluşturduğunuz eşliği dikey penceresinde görüntülenir. **Başlatılan** içinde listelenen **EŞLİĞİ durumu** sütunu için **myVnet1ToMyVnet2** sizin eşlemeyi oluşturuldu. Vnet1'den vnet2'ye eşlenen, ancak şimdi myVnet1 myVnet2 eş gerekir. Eşlemeyi birbirleri ile iletişim kurmak üzere sanal ağ kaynaklarında etkinleştirmek için her iki yönde oluşturulmalıdır.
11. Yeniden myVnet2 için 5-10 adımları tamamlayın. Eşlemeyi ad *myVnet2ToMyVnet1*.
12. ' I tıklattıktan sonra birkaç saniye **Tamam** MyVnet2 için eşlemesi oluşturmak için **myVnet2ToMyVnet1** , yeni oluşturduğunuz eşliği ile listelendiğini **bağlı** içinde  **Eşleme durumu** sütun.
13. Adım 5-7 MyVnet1 için yeniden tamamlayın. **EŞLİĞİ durumu** için **myVnet1ToVNet2** eşliği olan şimdi de **bağlı**. Gördükten sonra Eşleme başarıyla kurulduktan **bağlı** içinde **EŞLİĞİ durum** sütun eşlemesi içindeki iki sanal ağlar için.
14. **İsteğe bağlı**: sanal makineler oluştururken bu öğreticide kapsanmayan olsa, her sanal ağ içinde bir sanal makine oluşturun ve bir sanal makineden diğerine, bağlanabilirliği doğrulamak için bağlanın.
15. **İsteğe bağlı**: Bu öğreticide oluşturduğunuz kaynaklarını silmek için adımları tamamlamanız [silmek kaynakları](#delete-portal) bu makalenin.

Ya da sanal ağ içinde oluşturduğunuz Azure kaynaklarını IP adreslerini birbirleri ile iletişim kuramıyor. Varsayılan Azure ad çözümlemesi için sanal ağlar kullanıyorsanız, sanal ağlarda bulunan kaynaklar sanal ağlar arasında adlarını çözmek mümkün değildir. Bir eşlemesindeki sanal ağlar arasında adları çözümlemek dilerseniz kendi DNS sunucusu oluşturmanız gerekir. Nasıl ayarlanacağını öğrenin [kendi DNS sunucu kullanılarak ad çözümleme](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

## <a name="cli"></a>Eşlemesi - oluşturmak Azure CLI

Aşağıdaki komut dosyası:

- Azure CLI Sürüm 2.0.4 gerektirir veya sonraki bir sürümü. Sürüm bulmak için Çalıştır `az --version` komutu. Yükseltme gerekiyorsa, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json).
- Bir Bash kabuğunda çalışır. Windows istemcisi üzerinde Azure CLI betikleri çalıştırma seçenekleri için bkz [Windows Azure CLI çalıştıran](../virtual-machines/windows/cli-options.md?toc=%2fazure%2fvirtual-network%2ftoc.json). 

CLI ve bağımlılıklarını yüklemek yerine, Azure bulut Kabuğu'nu kullanabilirsiniz. Azure Cloud Shell doğrudan Azure portalının içinde çalıştırabileceğiniz ücretsiz bir Bash kabuğudur. Azure CLI, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Tıklatın **deneyin** , oturumu bir bulut Kabuk çağırır aşağıdaki Azure hesabınızla oturum açabildiğinizden komut düğmesi. Betik yürütmek için tıklatın **kopyalama** düğmesine tıklayın ve bulut kabuğundan içeriğini yapıştırın.

1. Bir kaynak grubu ve iki sanal ağ oluşturun.

    ```azurecli-interactive
    #!/bin/bash

    # Variables for common values used throughout the script.
    rgName="myResourceGroup"
    location="eastus"

    # Create a resource group.
    az group create \
      --name $rgName \
      --location $location

    # Create virtual network 1.
    az network vnet create \
      --name myVnet1 \
      --resource-group $rgName \
      --location $location \
      --address-prefix 10.0.0.0/16

    # Create virtual network 2.
    az network vnet create \
      --name myVnet2 \
      --resource-group $rgName \
      --location $location \
      --address-prefix 10.1.0.0/16
    ```

2. İki sanal ağlar arasında eşlemeyi bir sanal ağ oluşturun.
 
    ```azurecli-interactive
    # Get the id for VNet1.
    vnet1Id=$(az network vnet show \
      --resource-group $rgName \
      --name myVnet1 \
      --query id --out tsv)

    # Get the id for VNet2.
    vnet2Id=$(az network vnet show \
      --resource-group $rgName \
      --name myVnet2 \
      --query id \
      --out tsv)

    # Peer VNet1 to VNet2.
    az network vnet peering create \
      --name myVnet1ToMyVnet2 \
      --resource-group $rgName \
      --vnet-name myVnet1 \
      --remote-vnet-id $vnet2Id \
      --allow-vnet-access

    # Peer VNet2 to VNet1.
    az network vnet peering create \
      --name myVnet2ToMyVnet1 \
      --resource-group $rgName \
      --vnet-name myVnet2 \
      --remote-vnet-id $vnet1Id \
      --allow-vnet-access
    ```

3. Betiği çalıştırdıktan sonra her bir sanal ağ eşlemeleri gözden geçirin. 

    ```azurecli-interactive
    az network vnet peering list \
      --resource-group myResourceGroup \
      --vnet-name myVnet1 \
      --output table
    ```
    
    Değiştirme önceki komutu yeniden çalıştırın *myVnet1* ile *myVnet2*. Her ikisi de çıktısını gösterir komutları **bağlı** içinde **PeeringState** sütun.

     Ya da sanal ağ içinde oluşturduğunuz Azure kaynaklarını IP adreslerini birbirleri ile iletişim kuramıyor. Varsayılan Azure ad çözümlemesi için sanal ağlar kullanıyorsanız, sanal ağlarda bulunan kaynaklar sanal ağlar arasında adlarını çözmek mümkün değildir. Bir eşlemesindeki sanal ağlar arasında adları çözümlemek dilerseniz kendi DNS sunucusu oluşturmanız gerekir. Nasıl ayarlanacağını öğrenin [kendi DNS sunucu kullanılarak ad çözümleme](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

4. **İsteğe bağlı**: sanal makineler oluştururken bu öğreticide kapsanmayan olsa, her sanal ağ içinde bir sanal makine oluşturun ve bir sanal makineden diğerine, bağlanabilirliği doğrulamak için bağlanın.
5. **İsteğe bağlı**: Bu öğreticide oluşturduğunuz kaynaklarını silmek için adımları tamamlamanız [silmek kaynakları](#delete-cli) bu makalede.


## <a name="powershell"></a>Eşlemesi - oluşturmak PowerShell

1. PowerShell [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) modülünün en son sürümünü yükleyin. Azure PowerShell'i kullanmaya yeni başladıysanız [Azure PowerShell'e genel bakış](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json) sayfasını inceleyin.
2. Bir PowerShell oturumu başlatmak için **Başlat**'a gidin, **powershell** yazın ve **PowerShell**'e tıklayın.
3. PowerShell'de `login-azurermaccount` komutunu girerek Azure oturumu açın. İle oturum için kullandığınız hesabın, bir sanal ağ eşlemesi oluşturmak için gerekli izinleri olmalıdır. Bkz: [izinleri](#permissions) Ayrıntılar için bu makalenin.
4. Bir kaynak grubu ve iki sanal ağ oluşturun. Betik yürütmek için aşağıdaki komutu kopyalayın, PowerShell içinde yapıştırın ve ardından basın `Enter` son satırında ekranda göründükten sonra:

    ```powershell
    # Variables for common values used throughout the script.
    $rgName='myResourceGroup'
    $location='eastus'

    # Create a resource group.
    New-AzureRmResourceGroup `
      -Name $rgName `
      -Location $location

    # Create virtual network 1.
    $vnet1 = New-AzureRmVirtualNetwork `
      -ResourceGroupName $rgName `
      -Name 'myVnet1' `
      -AddressPrefix '10.0.0.0/16' `
      -Location $location

    # Create virtual network 2.
    $vnet2 = New-AzureRmVirtualNetwork `
      -ResourceGroupName $rgName `
      -Name 'myVnet2' `
      -AddressPrefix '10.1.0.0/16' `
      -Location $location
    ```

5. İki sanal ağlar arasında eşlemeyi bir sanal ağ oluşturun. Aşağıdaki komutu kopyalayın, PowerShell yapıştırın ve ardından basın `Enter` son satırında ekranda göründükten sonra:
    ```powershell
    # Peer VNet1 to VNet2.
    Add-AzureRmVirtualNetworkPeering `
      -Name 'myVnet1ToMyVnet2' `
      -VirtualNetwork $vnet1 `
      -RemoteVirtualNetworkId $vnet2.Id

    # Peer VNet2 to VNet1.
    Add-AzureRmVirtualNetworkPeering `
      -Name 'myVnet2ToMyVnet1' `
      -VirtualNetwork $vnet2 `
      -RemoteVirtualNetworkId $vnet1.Id
    ```
6. Sanal ağ alt ağlarını gözden geçirmek için aşağıdaki komutu kopyalayın, PowerShell yapıştırın ve ardından basın `Enter`:

    ```powershell
    Get-AzureRmVirtualNetworkPeering `
      -ResourceGroupName myResourceGroup `
      -VirtualNetworkName myVnet1 `
      | Format-Table VirtualNetworkName, PeeringState
    ```

    Değiştirme önceki komutu yeniden çalıştırın *myVnet1* ile *myVnet2*. Her ikisi de çıktısını gösterir komutları **bağlı** içinde **PeeringState** sütun.
7. **İsteğe bağlı**: sanal makineler oluştururken bu öğreticide kapsanmayan olsa, her sanal ağ içinde bir sanal makine oluşturun ve bir sanal makineden diğerine, bağlanabilirliği doğrulamak için bağlanın.
8. **İsteğe bağlı**: Bu öğreticide oluşturduğunuz kaynaklarını silmek için adımları tamamlamanız [silmek kaynakları](#delete-powershell) bu makalede.

Ya da sanal ağ içinde oluşturduğunuz Azure kaynaklarını IP adreslerini birbirleri ile iletişim kuramıyor. Varsayılan Azure ad çözümlemesi için sanal ağlar kullanıyorsanız, sanal ağlarda bulunan kaynaklar sanal ağlar arasında adlarını çözmek mümkün değildir. Bir eşlemesindeki sanal ağlar arasında adları çözümlemek dilerseniz kendi DNS sunucusu oluşturmanız gerekir. Nasıl ayarlanacağını öğrenin [kendi DNS sunucu kullanılarak ad çözümleme](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

## <a name="template"></a>Eşlemesi - oluşturmak Resource Manager şablonu

1. Başvuru [bir sanal ağ eşlemesi oluşturma](https://azure.microsoft.com/resources/templates/201-vnet-to-vnet-peering) Resource Manager şablonu. Şablonla birlikte verilen talimatları kullanarak şablonu Azure portalı, PowerShell veya Azure CLI aracılığıyla dağıtabilirsiniz. Bir sanal ağ eşlemesi oluşturmak için gerekli izinlere sahip bir hesap kullanarak şablonu dağıtmak hangisi aracı için oturum açma seçin. Bkz: [izinleri](#permissions) Ayrıntılar için bu makalenin.
2. **İsteğe bağlı**: sanal makineler oluştururken bu öğreticide kapsanmayan olsa, her sanal ağ içinde bir sanal makine oluşturun ve bir sanal makineden diğerine, bağlanabilirliği doğrulamak için bağlanın.
3. **İsteğe bağlı**: Bu öğreticide oluşturduğunuz kaynaklarını silmek için adımları tamamlamanız [silmek kaynakları](#delete) Azure portalı, PowerShell veya Azure CLI kullanarak, bu makalenin bölümüne.

## <a name="permissions"></a>İzinleri

Bir sanal ağ eşlemesi oluşturmak için kullandığınız hesaplarının gerekli rol veya izinleri olması gerekir. Örneğin, VNet1 ve VNet2 adlı iki sanal ağ eşlemesi, hesabınızı aşağıdaki en düşük rolü veya her sanal ağ izinlerini atanmalıdır:
    
|Sanal ağ|Rol|İzinler|
|---|---|---|
|VNet1|[Ağ Katılımcısı](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write|
|VNet2|[Ağ Katılımcısı](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/peer|

Daha fazla bilgi edinmek [yerleşik roller](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) ve belirli izinler atama [özel roller](../active-directory/role-based-access-control-custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (yalnızca Resource Manager).

## <a name="delete"></a>Kaynakları silin
Bu öğreticiyi tamamladığınızda, kullanım ücret ödememeniz öğreticide oluşturduğunuz kaynakları silmek isteyebilirsiniz. Bir kaynak grubunun silinmesi, kaynak grubunda bulunan tüm kaynakları siler.

### <a name="delete-portal"></a>Azure portalı

1. Portal arama kutusuna **myResourceGroup**. Arama sonuçlarında tıklatın **myResourceGroup**.
2. Üzerinde **myResourceGroup** dikey penceresinde tıklatın **silmek** simgesi.
3. İçinde silmeyi onaylamak için **türü kaynak grubu adı** kutusuna **myResourceGroup**ve ardından **silmek**.

### <a name="delete-cli"></a>Azure CLI

Aşağıdaki komutu girin:

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

### <a name="delete-powershell"></a>PowerShell

Aşağıdaki komutu girin:

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -force
```

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
