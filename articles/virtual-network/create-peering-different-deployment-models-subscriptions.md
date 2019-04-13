---
title: Bir Azure sanal ağ eşlemesi oluşturma - farklı dağıtım modelleri - farklı abonelikler
titlesuffix: Azure Virtual Network
description: Farklı Azure aboneliklerinde mevcut Azure farklı dağıtım modelleriyle oluşturulmuş sanal ağlar arasında eşleme bir sanal ağ oluşturmayı öğrenin.
services: virtual-network
documentationcenter: ''
author: jimdial
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/15/2017
ms.author: jdial;anavin
ms.openlocfilehash: 1a066e6b75d527dcdf1b211c0ebb76a2d4520eb7
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/12/2019
ms.locfileid: "59528221"
---
# <a name="create-a-virtual-network-peering---different-deployment-models-and-subscriptions"></a>Oluşturma bir sanal ağ eşlemesi - farklı dağıtım modelleri ve abonelikler

Bu öğreticide, farklı dağıtım modelleriyle oluşturulmuş sanal ağlar arasındaki eşleme bir sanal ağ oluşturmayı öğrenin. Farklı Aboneliklerde bulunan sanal ağlar yok. Kaynaklar aynı sanal ağda gibi davranarak birbiriyle aynı bant genişliği ve gecikme süresi ile iletişim kurmak için farklı sanal ağlardaki iki sanal ağları etkinleştirir kaynak eşlemesi. Daha fazla bilgi edinin [sanal ağ eşlemesi](virtual-network-peering-overview.md).

Sanal ağlar aynı veya farklı olup, abonelikleri ve hangi bağlı olarak farklı bir sanal ağ eşlemesi oluşturmak için adımları [Azure dağıtım modeli](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) sanal ağlar üzerinden oluşturulur. Diğer senaryolarda aşağıdaki tabloda senaryo tıklayarak eşlemesi sanal ağ oluşturmayı öğrenin:

|Azure dağıtım modeli  | Azure aboneliği  |
|--------- |---------|
|[Her iki kaynak yöneticisi](tutorial-connect-virtual-networks-portal.md) |Aynı|
|[Her iki kaynak yöneticisi](create-peering-different-subscriptions.md) |Fark|
|[Bir Resource Manager, diğeri Klasik](create-peering-different-deployment-models.md) |Aynı|

Bir sanal ağ eşlemesi iki sanal ağı Klasik dağıtım modeliyle dağıtılan arasında oluşturulamıyor. Bu öğreticide, mevcut sanal ağlar aynı bölgede kullanılır. Bu öğreticide, aynı bölgedeki sanal ağlar eşler. Ayrıca farklı sanal ağlarda eş [desteklenen bölgeler](virtual-network-manage-peering.md#cross-region). Önerilen ile kendinizi alıştırın [eşleme gereksinimleri ve kısıtlamaları](virtual-network-manage-peering.md#requirements-and-constraints) önce sanal ağları eşleme.

Farklı Aboneliklerde bulunan sanal ağlar arasında eşleme bir sanal ağ oluştururken, aboneliklerin her ikisi de aynı Azure Active Directory kiracısı ile ilişkilendirilmesi gerekir. Azure Active Directory kiracısı zaten yoksa, şunları kolayca yapabilirsiniz. [oluşturmak](../active-directory/develop/quickstart-create-new-tenant.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-new-azure-ad-tenant). Farklı Aboneliklerdeki sanal ağlara bağlayabilirsiniz ve farklı Azure Active Directory Kiracı Azure kullanarak [VPN ağ geçidi](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Kullanabileceğiniz [Azure portalında](#portal), Azure [komut satırı arabirimi](#cli) (CLI) veya Azure [PowerShell](#powershell) bir sanal ağ eşlemesi oluşturmak için. Doğrudan, tercih ettiğiniz araç kullanarak bir sanal ağ eşlemesi oluşturma adımlarını gitmek için önceki aracı bağlantılardan herhangi birine tıklayın.

## <a name="portal"></a>Oluşturma eşleme - Azure portalı

Bu öğreticide, farklı hesapları her abonelik için kullanılır. İki abonelik için izinleri olan bir hesap kullanıyorsanız, tüm adımlar için aynı hesabı kullanmak, günlük portalından adımları atlayın ve sanal ağlar için başka bir kullanıcı izinleri atama adımlarını atlayın.

1. Oturum [Azure portalında](https://portal.azure.com) UserA olarak. Oturum açmada hesabın, bir sanal ağ eşlemesi oluşturmak için gerekli izinleri olmalıdır. İzinleri listesi için bkz. [sanal ağ eşleme izinleri](virtual-network-manage-peering.md#permissions).
2. Tıklayın **+ yeni**, tıklayın **ağ**, ardından **sanal ağ**.
3. İçinde **sanal ağ oluştur** dikey penceresinde girin veya aşağıdaki ayarları için değerleri seçin, ardından tıklayın **Oluştur**:
    - **Adı**: *myVnetA*
    - **Adres alanı**: *10.0.0.0/16*
    - **Alt ağ adı**: *varsayılan*
    - **Alt ağ adres aralığı**: *10.0.0.0/24*
    - **Abonelik**: A aboneliği seçin
    - **Kaynak grubu**: Seçin **Yeni Oluştur** girin *myResourceGroupA*
    - **Konum**: *Doğu ABD*
4. İçinde **kaynak Ara** türü portalın üst kısmındaki kutusu *myVnetA*. Tıklayın **myVnetA** arama sonuçlarında görüntülendiğinde. Bir dikey pencere görünür **myVnetA** sanal ağ.
5. İçinde **myVnetA** görüntülenen dikey **erişim denetimi (IAM)** dikey dikey pencerenin sol tarafındaki Seçenekleri listesinden.
6. İçinde **myVnetA - erişim denetimi (IAM)** görüntülenen dikey **+ rol ataması Ekle**.
7. İçinde **rol ataması Ekle** görüntülenirse, seçin dikey **ağ Katılımcısı** içinde **rol** kutusu.
8. İçinde **seçin** kutusuna Kullanıcıb seçin veya aramak için Kullanıcıb'in e-posta adresini yazın. Kullanıcılara gösterilen listesi için eşleme ayarlama işlemini ayarladığınız sanal ağ aynı Azure Active Directory kiracısı arasındadır. Listesinde göründüğünde Kullanıcıb'ye tıklayın.
9. **Kaydet**’e tıklayın.
10. Portalından UserA olarak oturum açın ve ardından UserB oturum açın.
11. Tıklayın **+ yeni**, türü *sanal ağ* içinde **markette Ara** kutusunu işaretleyip tıklayın **sanal ağ** arama sonuçlarında.
12. İçinde **sanal ağ** görüntülenirse, seçin dikey **Klasik** içinde **dağıtım modeli seçin** kutusunu işaretleyip tıklayın **Oluştur**.
13. Görüntülenen Oluştur sanal ağ (Klasik) kutusunda, aşağıdaki değerleri girin:

    - **Adı**: *myVnetB*
    - **Adres alanı**: *10.1.0.0/16*
    - **Alt ağ adı**: *varsayılan*
    - **Alt ağ adres aralığı**: *10.1.0.0/24*
    - **Abonelik**: B aboneliği seçin
    - **Kaynak grubu**: Seçin **Yeni Oluştur** girin *myResourceGroupB*
    - **Konum**: *Doğu ABD*

14. İçinde **kaynak Ara** türü portalın üst kısmındaki kutusu *myVnetB*. Tıklayın **myVnetB** arama sonuçlarında görüntülendiğinde. Bir dikey pencere görünür **myVnetB** sanal ağ.
15. İçinde **myVnetB** görüntülenen dikey **özellikleri** dikey dikey pencerenin sol tarafındaki Seçenekleri listesinden. Kopyalama **kaynak kimliği**, bir sonraki adımda kullanılır. Kaynak Kimliği, aşağıdaki örneğe benzer: `/subscriptions/<Subscription ID>/resourceGroups/myResourceGroupB/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnetB`
16. MyVnetB girme, 5-9 adımlarını tamamlamak **Kullanıcıa** 8. adımda.
17. Portalda UserB olarak dışında oturum ve UserA oturum açın.
18. İçinde **kaynak Ara** türü portalın üst kısmındaki kutusu *myVnetA*. Tıklayın **myVnetA** arama sonuçlarında görüntülendiğinde. Bir dikey pencere görünür **myVnet** sanal ağ.
19. Tıklayın **myVnetA**.
20. İçinde **myVnetA** görüntülenen dikey **eşlemeler** dikey dikey pencerenin sol tarafındaki Seçenekleri listesinden.
21. İçinde **myVnetA - eşlemeler** görünen dikey **+ Ekle**
22. İçinde **Ekle eşlemesi** görüntülenen dikey girin veya aşağıdaki seçenekleri belirleyin, ardından tıklayın **Tamam**:
     - **Adı**: *myVnetAToMyVnetB*
     - **Sanal ağ dağıtım modeli**:  Seçin **Klasik**.
     - **Kaynak Kimliğimi biliyorum**: Bu kutuyu işaretleyin.
     - **Kaynak Kimliği**: 15. adımdan myVnetB kaynak Kimliğini girin.
     - **Sanal ağ erişimine izin ver:** Emin **etkin** seçilir.
    Diğer bir ayarları, bu öğreticide kullanılır. Tüm eşleme ayarları hakkında bilgi edinmek için [sanal ağ eşlemelerini yönetme](virtual-network-manage-peering.md#create-a-peering).
23. ' I tıklattıktan sonra **Tamam** önceki adımda **Eşlemesi Ekle** dikey penceresi kapanır ve gördüğünüz **myVnetA - eşlemeler** yeniden dikey. Birkaç saniye sonra oluşturduğunuz eşleme dikey penceresinde görünür. **Bağlı** listelenir **eşleme durumu** sütunu için **myVnetAToMyVnetB** , eşleme oluşturulur. Eşleme artık gerçekleştirilir. Sanal ağ (Resource Manager) (Klasik) sanal ağ ile eşleyebilme gerek yoktur.

    Her iki sanal ağ içinde oluşturduğunuz Azure kaynaklarını IP adresleri birbirleriyle iletişim kurabilir. Varsayılan Azure ad çözümlemesi için sanal ağlar kullanıyorsanız, kaynakların sanal ağlarda bulunan sanal ağlar arasında adlarını çözümlemek alamıyoruz. Bir eşleme de sanal ağlar arasında adlarını çözümlemek dilerseniz kendi DNS sunucunuzu oluşturmanız gerekir. Nasıl ayarlanacağını öğrenin [kendi DNS sunucunuzu kullanarak ad çözümlemesi](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server).

24. **İsteğe bağlı**: Bu öğreticide sanal makinelerin oluşturulmasını kapsamında olmayan da, her bir sanal ağ içinde bir sanal makine oluşturun ve bir sanal makineden diğerine, bağlantıyı doğrulamak için bağlanın.
25. **İsteğe bağlı**: Bu öğreticide oluşturduğunuz kaynakları silmek için'ndaki adımları tamamlayın. [Sil kaynakları](#delete-portal) bu makalenin.

## <a name="cli"></a>Oluşturma eşleme - Azure CLI

Bu öğreticide, farklı hesapları her abonelik için kullanılır. İki abonelik için izinleri olan bir hesap kullanıyorsanız, aynı hesabı kullanmak için tüm adımları, azure'dan günlüğe kaydetme adımları atlayın ve kullanıcı rol atamaları oluşturan komut satırlarını kaldırın. Değiştirin UserA@azure.com ve UserB@azure.com tüm Kullanıcıa ve kullanıcıb'in için kullanmakta olduğunuz kullanıcı adları ile aşağıdaki betikler. Azure Klasik CLI ve Azure CLI kullanarak aşağıdaki adımları tamamlayın. Yalnızca'i seçerek Azure Cloud shell'den adımları tamamlayabilirsiniz **deneyin** herhangi biri aşağıdaki adımları veya yükleyerek düğmesini [Klasik CLI](/cli/azure/install-classic-cli) ve [CLI](/cli/azure/install-azure-cli) ve Yerel bilgisayarınızda komutları.

1. Cloud Shell otomatik olarak, Azure'da oturum açtığı için Cloud Shell kullanıyorsanız, 2. adıma atlayın. Bir komut oturumu açın ve uygulamada oturum açma azure'a `azure login` komutu.
2. Klasik CLI Hizmet Yönetimi modunda girerek çalıştırın `azure config mode asm` komutu.
3. Sanal ağ (Klasik) oluşturmak için Klasik aşağıdaki CLI komutunu girin:

    ```azurecli
    azure network vnet create --vnet myVnetB --address-space 10.1.0.0 --cidr 16 --location "East US"
    ```
4. Azure CLI (Klasik CLI değil) ile bir bash kabuğunu kullanarak kalan adımları tamamlanmış olmalıdır.
5. Aşağıdaki komut dosyasını bilgisayarınızdaki bir metin düzenleyicisine kopyalayın. Değiştirin `<SubscriptionB-Id>` abonelik kimliğinizi Aboneliğinizi kimliği bilmiyorsanız, girin `az account show` komutu. Değeri **kimliği** çıktısında olduğundan, abonelik kimliği Değiştirilmiş betiği kopyalayın, CLI oturumunuza yapıştırın ve sonra basın `Enter`.

    ```azurecli-interactive
    az role assignment create \
      --assignee UserA@azure.com \
      --role "Classic Network Contributor" \
      --scope /subscriptions/<SubscriptionB-Id>/resourceGroups/Default-Networking/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnetB
    ```

    Sanal ağ (Klasik) 4. adımda oluşturduğunuz zaman, Azure sanal ağ içinde oluşturulmuş *varsayılan ağ* kaynak grubu.
6. Kullanıcıb'in Azure ve oturum açma, CLI UserA olarak oturum açın.
7. Bir kaynak grubunu ve sanal ağ (Resource Manager) oluşturun. Aşağıdaki betiği kopyalayın, CLI oturumunuza yapıştırın ve sonra basın `Enter`.

    ```azurecli-interactive
    #!/bin/bash

    # Variables for common values used throughout the script.
    rgName="myResourceGroupA"
    location="eastus"

    # Create a resource group.
    az group create \
      --name $rgName \
      --location $location

    # Create virtual network A (Resource Manager).
    az network vnet create \
      --name myVnetA \
      --resource-group $rgName \
      --location $location \
      --address-prefix 10.0.0.0/16

    # Get the id for myVnetA.
    vNetAId=$(az network vnet show \
      --resource-group $rgName \
      --name myVnetA \
      --query id --out tsv)

    # Assign UserB permissions to myVnetA.
    az role assignment create \
      --assignee UserB@azure.com \
      --role "Network Contributor" \
      --scope $vNetAId
    ```

8. Farklı dağıtım modelleriyle oluşturulan iki sanal ağ arasında eşleme bir sanal ağ oluşturun. Aşağıdaki komut dosyasını bilgisayarınızdaki bir metin düzenleyicisine kopyalayın. Değiştirin `<SubscriptionB-id>` aboneliğinizle kimliği. Aboneliğinizi kimliği bilmiyorsanız, girin `az account show` komutu. Değeri **kimliği** çıktısında olduğundan, abonelik kimliği Azure adlı adım 4'te bir kaynak grubunda oluşturulan sanal ağ (Klasik) oluşturulan *varsayılan ağ*. CLI oturumunuzda değiştirilmiş betiği yapıştırın ve sonra basın `Enter`.

    ```azurecli-interactive
    # Peer VNet1 to VNet2.
    az network vnet peering create \
      --name myVnetAToMyVnetB \
      --resource-group $rgName \
      --vnet-name myVnetA \
      --remote-vnet-id  /subscriptions/<SubscriptionB-id>/resourceGroups/Default-Networking/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnetB \
      --allow-vnet-access
    ```

9. Betiği çalıştırdıktan sonra sanal ağ (Resource Manager) için eşleme gözden geçirin. Aşağıdaki betiği kopyalayın ve CLI oturumunuzda yapıştırın:

    ```azurecli-interactive
    az network vnet peering list \
      --resource-group $rgName \
      --vnet-name myVnetA \
      --output table
    ```
    Çıktıda gösterildiği **bağlı** içinde **PeeringState** sütun.

    Her iki sanal ağ içinde oluşturduğunuz Azure kaynaklarını IP adresleri birbirleriyle iletişim kurabilir. Varsayılan Azure ad çözümlemesi için sanal ağlar kullanıyorsanız, kaynakların sanal ağlarda bulunan sanal ağlar arasında adlarını çözümlemek alamıyoruz. Bir eşleme de sanal ağlar arasında adlarını çözümlemek dilerseniz kendi DNS sunucunuzu oluşturmanız gerekir. Nasıl ayarlanacağını öğrenin [kendi DNS sunucunuzu kullanarak ad çözümlemesi](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server).

10. **İsteğe bağlı**: Bu öğreticide sanal makinelerin oluşturulmasını kapsamında olmayan da, her bir sanal ağ içinde bir sanal makine oluşturun ve bir sanal makineden diğerine, bağlantıyı doğrulamak için bağlanın.
11. **İsteğe bağlı**: Bu öğreticide oluşturduğunuz kaynakları silmek için'ndaki adımları tamamlayın. [Sil kaynakları](#delete-cli) bu makaledeki.

## <a name="powershell"></a>Oluşturma eşleme - PowerShell

Bu öğreticide, farklı hesapları her abonelik için kullanılır. İki abonelik için izinleri olan bir hesap kullanıyorsanız, aynı hesabı kullanmak için tüm adımları, azure'dan günlüğe kaydetme adımları atlayın ve kullanıcı rol atamaları oluşturan komut satırlarını kaldırın. Değiştirin UserA@azure.com ve UserB@azure.com tüm Kullanıcıa ve kullanıcıb'in için kullanmakta olduğunuz kullanıcı adları ile aşağıdaki betikler. 

1. En son PowerShell sürümünü [Azure](https://www.powershellgallery.com/packages/Azure) ve [Az](https://www.powershellgallery.com/packages/Az) modüller. Azure PowerShell'i kullanmaya yeni başladıysanız [Azure PowerShell'e genel bakış](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json) sayfasını inceleyin.
2. Bir PowerShell oturumu başlatın.
3. PowerShell'de, UserB olarak B'nin abonelik girerek oturum `Add-AzureAccount` komutu. Oturum açmada hesabın, bir sanal ağ eşlemesi oluşturmak için gerekli izinleri olmalıdır. İzinleri listesi için bkz. [sanal ağ eşleme izinleri](virtual-network-manage-peering.md#permissions).
4. PowerShell ile bir sanal ağ (Klasik) oluşturmak için yeni oluşturun veya mevcut, bir ağ yapılandırma dosyasını değiştirmeniz gerekir. Bilgi edinmek için nasıl [dışarı aktarma, güncelleştirme ve ağ yapılandırma dosyalarını içe aktarma](virtual-networks-using-network-configuration-file.md). Aşağıdaki dosya içermelidir **VirtualNetworkSite** öğesi Bu öğreticide kullanılan sanal ağ için:

    ```xml
    <VirtualNetworkSite name="myVnetB" Location="East US">
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

5. Resource Manager komutları girerek kullanılacak kullanıcıb'in B'nin aboneliğe oturum `Connect-AzAccount` komutu.
6. Kullanıcıa izinleri aşağıdaki komut dosyasını bir metin düzenleyicisi, PC ve Değiştir sanal ağ b kopya atama `<SubscriptionB-id>` kimliğine sahip abonelik b ' % S'abonelik kimliği bilmiyorsanız, girin `Get-AzSubscription` görüntülemek için komutu. Değeri **kimliği** döndürülen çıktısında, abonelik kimliğidir. Azure adlı adım 4'te bir kaynak grubunda oluşturulan sanal ağ (Klasik) oluşturulan *varsayılan ağ*. Betiği yürütmek için değiştirilmiş betiği kopyalayın, PowerShell yapıştırın ve ardından basın `Enter`.
    
    ```powershell 
    New-AzRoleAssignment `
      -SignInName UserA@azure.com `
      -RoleDefinitionName "Classic Network Contributor" `
      -Scope /subscriptions/<SubscriptionB-id>/resourceGroups/Default-Networking/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnetB
    ```

7. Oturum UserB olarak azure'dan ve oturum açma kullanıcı a'nın aboneliğe UserA olarak girerek `Connect-AzAccount` komutu. Oturum açmada hesabın, bir sanal ağ eşlemesi oluşturmak için gerekli izinleri olmalıdır. İzinleri listesi için bkz. [sanal ağ eşleme izinleri](virtual-network-manage-peering.md#permissions).
8. Sanal ağ (Resource Manager) aşağıdaki betiği kopyalama içinde PowerShell yapıştırma ve tuşuna basarak oluşturma `Enter`:

    ```powershell
    # Variables for common values
      $rgName='MyResourceGroupA'
      $location='eastus'

    # Create a resource group.
    New-AzResourceGroup `
      -Name $rgName `
      -Location $location

    # Create virtual network A.
    $vnetA = New-AzVirtualNetwork `
      -ResourceGroupName $rgName `
      -Name 'myVnetA' `
      -AddressPrefix '10.0.0.0/16' `
      -Location $location
    ```

9. İçin myVnetA Kullanıcıb izinleri atayın. PC ve Değiştir bir metin düzenleyicisi için aşağıdaki betiği kopyalayın `<SubscriptionA-Id>` A. abonelik kimliği ' % S'abonelik kimliği bilmiyorsanız, girin `Get-AzSubscription` görüntülemek için komutu. Değeri **kimliği** döndürülen çıktısında, abonelik kimliğidir. PowerShell'de komut dosyasının değiştirilmiş sürümünü yapıştırın ve sonra basın `Enter` çalıştırmak üzere.

    ```powershell
    New-AzRoleAssignment `
      -SignInName UserB@azure.com `
      -RoleDefinitionName "Network Contributor" `
      -Scope /subscriptions/<SubscriptionA-Id>/resourceGroups/myResourceGroupA/providers/Microsoft.Network/VirtualNetworks/myVnetA
    ```

10. Aşağıdaki betiği bilgisayarınıza bir metin düzenleyicisine kopyalayın ve Değiştir `<SubscriptionB-id>` kimliğine sahip abonelik b MyVnetA myVNetB için eşlenecek değiştirilmiş betiği kopyalayın, PowerShell yapıştırın ve ardından basın `Enter`.

    ```powershell
    Add-AzVirtualNetworkPeering `
      -Name 'myVnetAToMyVnetB' `
      -VirtualNetwork $vnetA `
      -RemoteVirtualNetworkId /subscriptions/<SubscriptionB-id>/resourceGroups/Default-Networking/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnetB
    ```

11. Aşağıdaki komut dosyası kopyalama, PowerShell yapıştırma ve tuşuna basarak myVnetA eşleme durumunu görüntülemek `Enter`.

    ```powershell
    Get-AzVirtualNetworkPeering `
      -ResourceGroupName $rgName `
      -VirtualNetworkName myVnetA `
      | Format-Table VirtualNetworkName, PeeringState
    ```

    Durumu **bağlı**. Değişir **bağlı** myVnetA için eşleme myVnetB ayarladıktan sonra.

    Her iki sanal ağ içinde oluşturduğunuz Azure kaynaklarını IP adresleri birbirleriyle iletişim kurabilir. Varsayılan Azure ad çözümlemesi için sanal ağlar kullanıyorsanız, kaynakların sanal ağlarda bulunan sanal ağlar arasında adlarını çözümlemek alamıyoruz. Bir eşleme de sanal ağlar arasında adlarını çözümlemek dilerseniz kendi DNS sunucunuzu oluşturmanız gerekir. Nasıl ayarlanacağını öğrenin [kendi DNS sunucunuzu kullanarak ad çözümlemesi](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server).

12. **İsteğe bağlı**: Bu öğreticide sanal makinelerin oluşturulmasını kapsamında olmayan da, her bir sanal ağ içinde bir sanal makine oluşturun ve bir sanal makineden diğerine, bağlantıyı doğrulamak için bağlanın.
13. **İsteğe bağlı**: Bu öğreticide oluşturduğunuz kaynakları silmek için'ndaki adımları tamamlayın. [Sil kaynakları](#delete-powershell) bu makaledeki.

## <a name="delete"></a>Kaynakları silme
Bu öğreticiyi tamamladığınızda, kullanım ücret ödememeniz öğreticide oluşturulan kaynakları silmek isteyebilirsiniz. Bir kaynak grubunun silinmesi kaynak grubundaki tüm kaynakları da siler.

### <a name="delete-portal"></a>Azure portal

1. Portal arama kutusuna **myResourceGroupA**. Arama sonuçlarında tıklayın **myResourceGroupA**.
2. Üzerinde **myResourceGroupA** dikey penceresinde tıklayın **Sil** simgesi.
3. İçinde silmeyi onaylamak için **kaynak grubu adını yazın** kutusuna **myResourceGroupA**ve ardından **Sil**.
4. İçinde **kaynak Ara** türü portalın üst kısmındaki kutusu *myVnetB*. Tıklayın **myVnetB** arama sonuçlarında görüntülendiğinde. Bir dikey pencere görünür **myVnetB** sanal ağ.
5. İçinde **myVnetB** dikey penceresinde tıklayın **Sil**.
6. Silme işlemini onaylamak için tıklayın **Evet** içinde **silme sanal ağ** kutusu.

### <a name="delete-cli"></a>Azure CLI

1. Sanal ağ (Resource Manager) aşağıdaki komutla silmek için CLI'yı kullanarak Azure'da oturum açın:

   ```azurecli-interactive
   az group delete --name myResourceGroupA --yes
   ```

2. Aşağıdaki komutları kullanarak sanal ağ (Klasik) silmek için Klasik CLI kullanarak Azure'da oturum açın:

   ```azurecli-interactive
   azure config mode asm

   azure network vnet delete --vnet myVnetB --quiet
   ```

### <a name="delete-powershell"></a>PowerShell

1. PowerShell komut isteminde, sanal ağ (Resource Manager) silmek için aşağıdaki komutu girin:

   ```powershell
   Remove-AzResourceGroup -Name myResourceGroupA -Force
   ```

2. Sanal ağını silmek için PowerShell ile (Klasik), mevcut bir ağ yapılandırma dosyasını değiştirmeniz gerekir. Bilgi edinmek için nasıl [dışarı aktarma, güncelleştirme ve ağ yapılandırma dosyalarını içe aktarma](virtual-networks-using-network-configuration-file.md). Bu öğreticide kullanılan sanal ağ için aşağıdaki VirtualNetworkSite öğeyi kaldırın:

   ```xml
   <VirtualNetworkSite name="myVnetB" Location="East US">
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