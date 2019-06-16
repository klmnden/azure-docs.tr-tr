---
title: Oluşturma bir Azure sanal ağ eşlemesi - farklı dağıtım modelleri - aynı abonelik | Microsoft Docs
description: Aynı Azure aboneliğinde mevcut Azure farklı dağıtım modelleriyle oluşturulmuş sanal ağlar arasında eşleme bir sanal ağ oluşturmayı öğrenin.
services: virtual-network
documentationcenter: ''
author: KumudD
manager: twooley
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/15/2018
ms.author: kumud;anavin
ms.openlocfilehash: 56474ee56051c3b0b7482e81b0174b7945537654
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64694722"
---
# <a name="create-a-virtual-network-peering---different-deployment-models-same-subscription"></a>Oluşturma bir sanal ağ eşlemesi - farklı dağıtım modelleri, aynı abonelik

Bu öğreticide, farklı dağıtım modelleriyle oluşturulmuş sanal ağlar arasındaki eşleme bir sanal ağ oluşturmayı öğrenin. Her iki sanal ağ, aynı abonelikte yok. Kaynaklar aynı sanal ağda gibi davranarak birbiriyle aynı bant genişliği ve gecikme süresi ile iletişim kurmak için farklı sanal ağlardaki iki sanal ağları etkinleştirir kaynak eşlemesi. Daha fazla bilgi edinin [sanal ağ eşlemesi](virtual-network-peering-overview.md).

Sanal ağlar aynı veya farklı olup, abonelikleri ve hangi bağlı olarak farklı bir sanal ağ eşlemesi oluşturmak için adımları [Azure dağıtım modeli](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) sanal ağlar üzerinden oluşturulur. Diğer senaryolarda aşağıdaki tabloda senaryo tıklayarak eşlemesi sanal ağ oluşturmayı öğrenin:

|Azure dağıtım modeli  | Azure aboneliği  |
|--------- |---------|
|[Her iki kaynak yöneticisi](tutorial-connect-virtual-networks-portal.md) |Aynı|
|[Her iki kaynak yöneticisi](create-peering-different-subscriptions.md) |Farklı|
|[Bir Resource Manager, diğeri Klasik](create-peering-different-deployment-models-subscriptions.md) |Farklı|

Bir sanal ağ eşlemesi iki sanal ağı Klasik dağıtım modeliyle dağıtılan arasında oluşturulamıyor. Klasik dağıtım modeliyle her ikisi de oluşturulan sanal ağlar bağlanmanız gerekirse, bir Azure kullanabileceğiniz [VPN ağ geçidi](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) sanal ağları bağlamak için.

Bu öğreticide, aynı bölgedeki sanal ağlar eşler. Ayrıca farklı sanal ağlarda eş [desteklenen bölgeler](virtual-network-manage-peering.md#cross-region). Önerilen ile kendinizi alıştırın [eşleme gereksinimleri ve kısıtlamaları](virtual-network-manage-peering.md#requirements-and-constraints) önce sanal ağları eşleme.

Azure portalı, Azure kullanabileceğiniz [komut satırı arabirimi](#cli) (CLI), Azure [PowerShell](#powershell), veya bir sanal ağ eşlemesi oluşturmak için bir Azure Resource Manager şablonu. Doğrudan, tercih ettiğiniz araç kullanarak bir sanal ağ eşlemesi oluşturma adımlarını gitmek için önceki aracı bağlantılardan herhangi birine tıklayın.

## <a name="create-peering---azure-portal"></a>Oluşturma eşleme - Azure portalı

1. [Azure Portal](https://portal.azure.com) oturum açın. Hesabı ile bir sanal ağ eşlemesi oluşturmak için gerekli izinleri olmalıdır. İzinleri listesi için bkz. [sanal ağ eşleme izinleri](virtual-network-manage-peering.md#requirements-and-constraints).
2. Tıklayın **+ yeni**, tıklayın **ağ**, ardından **sanal ağ**.
3. İçinde **sanal ağ oluştur** dikey penceresinde girin veya aşağıdaki ayarları için değerleri seçin, ardından tıklayın **Oluştur**:
    - **Adı**: *myVnet1*
    - **Adres alanı**: *10.0.0.0/16*
    - **Alt ağ adı**: *varsayılan*
    - **Alt ağ adres aralığı**: *10.0.0.0/24*
    - **Abonelik**: Aboneliğinizi seçme
    - **Kaynak grubu**: Seçin **Yeni Oluştur** girin *myResourceGroup*
    - **Konum**: *Doğu ABD*
4. **+ Yeni** öğesine tıklayın. İçinde **markette Ara** kutusuna *sanal ağ*. Tıklayın **sanal ağ** arama sonuçlarında görüntülendiğinde.
5. İçinde **sanal ağ** dikey penceresinde **Klasik** içinde **dağıtım modeli seçin** kutusuna ve ardından **Oluştur**.
6. İçinde **sanal ağ oluştur** dikey penceresinde girin veya aşağıdaki ayarları için değerleri seçin, ardından tıklayın **Oluştur**:
    - **Adı**: *myVnet2*
    - **Adres alanı**: *10.1.0.0/16*
    - **Alt ağ adı**: *varsayılan*
    - **Alt ağ adres aralığı**: *10.1.0.0/24*
    - **Abonelik**: Aboneliğinizi seçme
    - **Kaynak grubu**: Seçin **var olanı kullan** seçip *myResourceGroup*
    - **Konum**: *Doğu ABD*
7. İçinde **kaynak Ara** türü portalın üst kısmındaki kutusu *myResourceGroup*. Tıklayın **myResourceGroup** arama sonuçlarında görüntülendiğinde. Bir dikey pencere görünür **myresourcegroup** kaynak grubu. Kaynak grubu, önceki adımlarda oluşturulan iki sanal ağı içerir.
8. Tıklayın **myVNet1**.
9. İçinde **myVnet1** görüntülenen dikey **eşlemeler** dikey dikey pencerenin sol tarafındaki Seçenekleri listesinden.
10. İçinde **myVnet1 - eşlemeler** görünen dikey **+ Ekle**
11. İçinde **Ekle eşlemesi** görüntülenen dikey girin veya aşağıdaki seçenekleri belirleyin, ardından tıklayın **Tamam**:
     - **Adı**: *myVnet1ToMyVnet2*
     - **Sanal ağ dağıtım modeli**:  Seçin **Klasik**.
     - **Abonelik**: Aboneliğinizi seçme
     - **Sanal ağ**:  Tıklayın **bir sanal ağ seçin**, ardından **myVnet2**.
     - **Sanal ağ erişimine izin ver:** Emin **etkin** seçilir.
    Diğer bir ayarları, bu öğreticide kullanılır. Tüm eşleme ayarları hakkında bilgi edinmek için [sanal ağ eşlemelerini yönetme](virtual-network-manage-peering.md#create-a-peering).
12. ' I tıklattıktan sonra **Tamam** önceki adımda **Eşlemesi Ekle** dikey penceresi kapanır ve gördüğünüz **myVnet1 - eşlemeler** yeniden dikey. Birkaç saniye sonra oluşturduğunuz eşleme dikey penceresinde görünür. **Bağlı** listelenir **eşleme durumu** sütunu için **myVnet1ToMyVnet2** , eşleme oluşturulur.

    Eşleme artık gerçekleştirilir. Her iki sanal ağ içinde oluşturduğunuz Azure kaynaklarını IP adresleri birbirleriyle iletişim kurabilir. Varsayılan Azure ad çözümlemesi için sanal ağlar kullanıyorsanız, kaynakların sanal ağlarda bulunan sanal ağlar arasında adlarını çözümlemek belirtemiyor. Bir eşleme de sanal ağlar arasında adlarını çözümlemek dilerseniz kendi DNS sunucunuzu oluşturmanız gerekir. Nasıl ayarlanacağını öğrenin [kendi DNS sunucunuzu kullanarak ad çözümlemesi](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server).
13. **İsteğe bağlı**: Sanal makine oluşturma, Bu öğretici kapsamında değildir ancak her bir sanal ağ içinde bir sanal makine oluşturun ve bir sanal makineden, bağlantıyı doğrulamak için bağlantı.
14. **İsteğe bağlı**: Bu öğreticide oluşturduğunuz kaynakları silmek için'ndaki adımları tamamlayın. [Sil kaynakları](#delete-portal) bu makalenin.

## <a name="cli"></a>Oluşturma eşleme - Azure CLI

Azure Klasik CLI ve Azure CLI kullanarak aşağıdaki adımları tamamlayın. Yalnızca'i seçerek Azure Cloud shell'den adımları tamamlayabilirsiniz **deneyin** herhangi biri aşağıdaki adımları veya yükleyerek düğmesini [Klasik CLI](/cli/azure/install-cli-version-1.0?toc=%2fazure%2fvirtual-network%2ftoc.json) ve [CLI](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json) ve Yerel bilgisayarınızda komutları.

1. Cloud Shell otomatik olarak, Azure'da oturum açtığı için Cloud Shell kullanıyorsanız, 2. adıma atlayın. Bir komut oturumu açın ve uygulamada oturum açma azure'a `azure login` komutu.
2. CLI Hizmet Yönetimi modunda girerek çalıştırın `azure config mode asm` komutu.
3. Sanal ağ (Klasik) oluşturmak için aşağıdaki komutu girin:

   ```azurecli-interactive
   azure network vnet create --vnet myVnet2 --address-space 10.1.0.0 --cidr 16 --location "East US"
   ```

4. CLI, Klasik CLI kullanarak aşağıdaki bash CLI betiği yürütün. Windows bilgisayarda CLI betikleri çalıştırma seçenekleri bash için bkz: [Windows üzerinde Azure CLI'yı yükleme](/cli/azure/install-azure-cli-windows).

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

5. CLI kullanarak farklı dağıtım modelleriyle oluşturulan iki sanal ağ arasında eşleme bir sanal ağ oluşturun. Aşağıdaki komut dosyasını bilgisayarınızdaki bir metin düzenleyicisine kopyalayın. Değiştirin `<subscription id>` abonelik kimliğinizi Abonelik Kimliğinizi bilmiyorsanız, girin `az account show` komutu. Değeri **kimliği** çıktısında, abonelik kimliğidir. CLI oturumunuz için de değiştirilmiş betiği yapıştırın ve sonra basın `Enter`.

   ```azurecli-interactive
   # Get the ID for VNet1.
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

6. Betiği çalıştırdıktan sonra sanal ağ (Resource Manager) için eşleme gözden geçirin. Aşağıdaki komutu kopyalayın, CLI oturumunuzda yapıştırın ve ardından basın `Enter`:

   ```azurecli-interactive
   az network vnet peering list \
     --resource-group myResourceGroup \
     --vnet-name myVnet1 \
     --output table
   ```

   Çıktıda gösterildiği **bağlı** içinde **PeeringState** sütun.

   Her iki sanal ağ içinde oluşturduğunuz Azure kaynaklarını IP adresleri birbirleriyle iletişim kurabilir. Varsayılan Azure ad çözümlemesi için sanal ağlar kullanıyorsanız, kaynakların sanal ağlarda bulunan sanal ağlar arasında adlarını çözümlemek belirtemiyor. Bir eşleme de sanal ağlar arasında adlarını çözümlemek dilerseniz kendi DNS sunucunuzu oluşturmanız gerekir. Nasıl ayarlanacağını öğrenin [kendi DNS sunucunuzu kullanarak ad çözümlemesi](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server).
7. **İsteğe bağlı**: Sanal makine oluşturma, Bu öğretici kapsamında değildir ancak her bir sanal ağ içinde bir sanal makine oluşturun ve bir sanal makineden, bağlantıyı doğrulamak için bağlantı.
8. **İsteğe bağlı**: Bu öğreticide oluşturduğunuz kaynakları silmek için'ndaki adımları tamamlayın. [Sil kaynakları](#delete-cli) bu makaledeki.

## <a name="powershell"></a>Oluşturma eşleme - PowerShell

1. En son PowerShell sürümünü [Azure](https://www.powershellgallery.com/packages/Azure) ve [Az](https://www.powershellgallery.com/packages/Az/) modüller. Azure PowerShell'i kullanmaya yeni başladıysanız [Azure PowerShell'e genel bakış](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json) sayfasını inceleyin.
2. Bir PowerShell oturumu başlatın.
3. PowerShell'de, Azure'da oturum girerek `Add-AzureAccount` komutu. Hesabı ile bir sanal ağ eşlemesi oluşturmak için gerekli izinleri olmalıdır. İzinleri listesi için bkz. [sanal ağ eşleme izinleri](virtual-network-manage-peering.md#requirements-and-constraints).
4. PowerShell ile bir sanal ağ (Klasik) oluşturmak için yeni oluşturun veya mevcut, bir ağ yapılandırma dosyasını değiştirmeniz gerekir. Bilgi edinmek için nasıl [dışarı aktarma, güncelleştirme ve ağ yapılandırma dosyalarını içe aktarma](virtual-networks-using-network-configuration-file.md). Aşağıdaki dosya içermelidir **VirtualNetworkSite** öğesi Bu öğreticide kullanılan sanal ağ için:

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
    > Değiştirilen ağ yapılandırma dosyasını içeri aktarma, var olan sanal ağları aboneliğinizde (Klasik) değişiklikleri neden olabilir. Yalnızca önceki sanal ağ ekleme ve değiştirme veya yoksa var olan tüm sanal ağları aboneliğinizden kaldırın, emin olun.
5. Azure'da oturum aç girerek sanal ağ (Resource Manager) oluşturmak için `Connect-AzAccount` komutu. Hesabı ile bir sanal ağ eşlemesi oluşturmak için gerekli izinleri olmalıdır. İzinleri listesi için bkz. [sanal ağ eşleme izinleri](virtual-network-manage-peering.md#requirements-and-constraints).
6. Bir kaynak grubunu ve sanal ağ (Resource Manager) oluşturun. Tuşuna basarak betiği kopyalayın ve PowerShell içinde yapıştırın `Enter`.

    ```powershell
    # Create a resource group.
      New-AzResourceGroup -Name myResourceGroup -Location eastus

    # Create the virtual network (Resource Manager).
      $vnet1 = New-AzVirtualNetwork `
      -ResourceGroupName myResourceGroup `
      -Name 'myVnet1' `
      -AddressPrefix '10.0.0.0/16' `
      -Location eastus
    ```

7. Farklı dağıtım modelleriyle oluşturulan iki sanal ağ arasında eşleme bir sanal ağ oluşturun. Aşağıdaki komut dosyasını bilgisayarınızdaki bir metin düzenleyicisine kopyalayın. Değiştirin `<subscription id>` abonelik kimliğinizi Abonelik Kimliğinizi bilmiyorsanız, girin `Get-AzSubscription` görüntülemek için komutu. Değeri **kimliği** döndürülen çıktısında, abonelik kimliğidir. Betiği yürütmek için metin düzenleyici, sonra sağ tıklatın, bir PowerShell oturumunda değiştirilmiş betiği kopyalayın ve tuşuna `Enter`.

    ```powershell
    # Peer VNet1 to VNet2.
    Add-AzVirtualNetworkPeering `
      -Name myVnet1ToMyVnet2 `
      -VirtualNetwork $vnet1 `
      -RemoteVirtualNetworkId /subscriptions/<subscription Id>/resourceGroups/Default-Networking/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnet2
    ```

8. Betiği çalıştırdıktan sonra sanal ağ (Resource Manager) için eşleme gözden geçirin. Tuşuna basın aşağıdaki komutu kopyalayın ve PowerShell oturumunuzda yapıştırın `Enter`:

    ```powershell
    Get-AzVirtualNetworkPeering `
      -ResourceGroupName myResourceGroup `
      -VirtualNetworkName myVnet1 `
      | Format-Table VirtualNetworkName, PeeringState
    ```

    Çıktıda gösterildiği **bağlı** içinde **PeeringState** sütun.

    Her iki sanal ağ içinde oluşturduğunuz Azure kaynaklarını IP adresleri birbirleriyle iletişim kurabilir. Varsayılan Azure ad çözümlemesi için sanal ağlar kullanıyorsanız, kaynakların sanal ağlarda bulunan sanal ağlar arasında adlarını çözümlemek belirtemiyor. Bir eşleme de sanal ağlar arasında adlarını çözümlemek dilerseniz kendi DNS sunucunuzu oluşturmanız gerekir. Nasıl ayarlanacağını öğrenin [kendi DNS sunucunuzu kullanarak ad çözümlemesi](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server).

9. **İsteğe bağlı**: Sanal makine oluşturma, Bu öğretici kapsamında değildir ancak her bir sanal ağ içinde bir sanal makine oluşturun ve bir sanal makineden, bağlantıyı doğrulamak için bağlantı.
10. **İsteğe bağlı**: Bu öğreticide oluşturduğunuz kaynakları silmek için'ndaki adımları tamamlayın. [Sil kaynakları](#delete-powershell) bu makaledeki.

## <a name="delete"></a>Kaynakları silme

Bu öğreticiyi tamamladığınızda, kullanım ücret ödememeniz öğreticide oluşturulan kaynakları silmek isteyebilirsiniz. Bir kaynak grubunun silinmesi kaynak grubundaki tüm kaynakları da siler.

### <a name="delete-portal"></a>Azure portal

1. Portal arama kutusuna **myResourceGroup**. Arama sonuçlarında tıklayın **myResourceGroup**.
2. Üzerinde **myResourceGroup** dikey penceresinde tıklayın **Sil** simgesi.
3. İçinde silmeyi onaylamak için **kaynak grubu adını yazın** kutusuna **myResourceGroup**ve ardından **Sil**.

### <a name="delete-cli"></a>Azure CLI

1. Sanal ağ (Resource Manager) silmek için Azure CLI ile aşağıdaki komutu kullanın:

    ```azurecli-interactive
    az group delete --name myResourceGroup --yes
    ```

2. Klasik CLI, aşağıdaki komutları kullanarak sanal ağ (Klasik) silmek için kullanın:

    ```azurecli-interactive
    azure config mode asm

    azure network vnet delete --vnet myVnet2 --quiet
    ```

### <a name="delete-powershell"></a>PowerShell

1. Sanal ağ (Resource Manager) silmek için aşağıdaki komutu girin:

    ```powershell
    Remove-AzResourceGroup -Name myResourceGroup -Force
    ```

2. Sanal ağını silmek için PowerShell ile (Klasik), mevcut bir ağ yapılandırma dosyasını değiştirmeniz gerekir. Bilgi edinmek için nasıl [dışarı aktarma, güncelleştirme ve ağ yapılandırma dosyalarını içe aktarma](virtual-networks-using-network-configuration-file.md). Bu öğreticide kullanılan sanal ağ için aşağıdaki VirtualNetworkSite öğeyi kaldırın:

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
    > Değiştirilen ağ yapılandırma dosyasını içeri aktarma, var olan sanal ağları aboneliğinizde (Klasik) değişiklikleri neden olabilir. Yalnızca önceki sanal ağı kaldırmak ve değiştirmek veya yoksa diğer mevcut sanal ağlara aboneliğinizden kaldırın, emin olun.

## <a name="next-steps"></a>Sonraki adımlar

- Kapsamlı ile önemli bilgilenmeli [sanal ağ eşleme kısıtlamaları ve davranışları](virtual-network-manage-peering.md#requirements-and-constraints) bir sanal ağ eşlemesi için üretim oluşturmadan önce kullanın.
- Hakkında bilgi edinin [sanal ağ eşleme ayarları](virtual-network-manage-peering.md#create-a-peering).
- Bilgi edinmek için nasıl [bir hub oluşturmak ve ağ topolojisi](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) sanal ağ eşlemesi ile.
