---
title: "Bir Azure sanal ağ eşlemesi oluşturma - farklı dağıtım modelleri - farklı abonelikleri | Microsoft Docs"
description: "Farklı Azure aboneliklerinde mevcut farklı Azure dağıtım modelleri aracılığıyla oluşturulan sanal ağlar arasında eşleme sanal ağ oluşturmayı öğrenin."
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/15/2017
ms.author: jdial;anavin
ms.openlocfilehash: e69ed1011fb0e9efdce115d1618c59c5bb86e224
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="create-a-virtual-network-peering---different-deployment-models-and-subscriptions"></a>Sanal Ağ eşlemesi bir - farklı oluşturmak dağıtım modelleri ve abonelikleri

Bu öğreticide, farklı dağıtım modeli oluşturulan sanal ağlar arasında eşleme sanal ağ oluşturmayı öğrenin. Sanal ağlar farklı Aboneliklerde mevcut. Kaynaklar aynı sanal ağda gibi davranarak birbirleri ile aynı bant genişliği ve gecikme süresi ile iletişim kurmak için farklı sanal ağlar iki sanal ağlar etkinleştirir kaynaklarında eşleme. Daha fazla bilgi edinmek [sanal ağ eşlemesi](virtual-network-peering-overview.md).

Sanal ağlar aynı ya da farklı olup, abonelikleri ve hangi bağlı olarak farklı bir sanal ağ eşlemesi oluşturmak için aşağıdaki adımları [Azure dağıtım modeli](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) sanal ağlar üzerinden oluşturulur. Aşağıdaki tabloda senaryodan tıklayarak diğer senaryolarda eşliği bir sanal ağ oluşturmayı öğrenin:

|Azure dağıtım modeli  | Azure aboneliği  |
|--------- |---------|
|[Her iki kaynak yöneticisi](virtual-network-create-peering.md) |Aynı|
|[Her iki kaynak yöneticisi](create-peering-different-subscriptions.md) |Farklı|
|[Bir kaynak yöneticisi, bir Klasik](create-peering-different-deployment-models.md) |Aynı|

Klasik dağıtım modeli aracılığıyla dağıtılan iki sanal ağ arasında bir sanal ağ eşlemesi oluşturulamıyor. Farklı Aboneliklerde mevcut farklı dağıtım modeli üzerinden oluşturulan sanal ağlar arası özelliği şu anda önizlemede değil. Bu öğreticiyi tamamlamak için öncelikle [kaydetmek](#register) yeteneği kullanmak için. Bu öğretici, mevcut sanal ağlar aynı bölgede kullanır. Sanal ağlar farklı bölgelerde eş olanağı da önizlemede değil. Bu özelliği kullanmak için ayrıca gerekir [kaydetmek](#register) onun için. İki yetenekleri bağımsızdır. Bu öğreticiyi tamamlamak için yalnızca farklı Aboneliklerde mevcut farklı dağıtım modeli üzerinden oluşturulan sanal ağlar arası kapasitesi için kaydetmeniz gerekir. 

Farklı Aboneliklerde bulunan sanal ağlar arasında eşleme sanal bir ağ oluştururken, abonelikler her ikisi de aynı Azure Active Directory Kiracı ilişkilendirilmiş olması gerekir. Azure Active Directory kiracısı zaten sahip değilseniz, hızlı bir şekilde yapabilecekleriniz [oluşturmak](../active-directory/develop/active-directory-howto-tenant.md?toc=%2fazure%2fvirtual-network%2ftoc.json#start-from-scratch). 

Sanal ağlara bağlanma özelliği oluşturulan dağıtım modeli, farklı dağıtım modeli, farklı bölgelerde veya aynı ilişkili abonelik aracılığıyla ya da farklı Azure Active Directory Kiracı Azure kullanarak [VPN ağ geçidi](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) Önizleme sürümü ve kayıt gerektirmez.

Kullanabileceğiniz [Azure portal](#portal), Azure [komut satırı arabirimi](#cli) (CLI) veya Azure [PowerShell](#powershell) bir sanal ağ eşlemesi oluşturmak için. Doğrudan seçeceğiniz araç kullanarak bir sanal ağ eşlemesi oluşturma adımlarını gitmek için önceki aracı bağlantılardan herhangi birine tıklayın.

## <a name="portal"></a>Eşlemesi - oluşturmak Azure portalı

Bu öğretici, her abonelik için farklı hesaplar kullanır. Her iki abonelikler için izinlere sahip bir hesap kullanıyorsanız, tüm adımlar için aynı hesabı kullanmak, portal dışında günlüğü için adımları atlayın ve sanal ağlar için başka bir kullanıcı izinlerini atamak için adımları atlayın. Aşağıdaki adımları gerçekleştirmeden önce önizleme için kaydetmeniz gerekir. Kaydetmek için adımları tamamlamanız [Önizleme için kaydetmek](#register) bu makalenin. Önizleme için her iki abonelikler kaydettirmezseniz kalan adımları başarısız.
 
1. Oturum [Azure portal](https://portal.azure.com) UserA olarak. İle oturum için kullandığınız hesabın, bir sanal ağ eşlemesi oluşturmak için gerekli izinleri olmalıdır. Bkz: [izinleri](#permissions) Ayrıntılar için bu makalenin.
2. Tıklatın **+ yeni**, tıklatın **ağ**, ardından **sanal ağ**.
3. İçinde **sanal ağ oluştur** dikey penceresinde girin veya aşağıdaki ayarları için değerleri seçin ve ardından **oluşturma**:
    - **Ad**: *myVnetA*
    - **Adres alanı**: *10.0.0.0/16*
    - **Alt ağ adı**: *varsayılan*
    - **Alt ağ adres aralığı**: *10.0.0.0/24*
    - **Abonelik**: A. abonelik seçin
    - **Kaynak grubu**: seçin **Yeni Oluştur** ve girin *myResourceGroupA*
    - **Konum**: *Doğu ABD*
4. İçinde **arama kaynakları** kutusunu türü portalı üstündeki *myVnetA*. Tıklatın **myVnetA** arama sonuçlarında görüntülendiğinde. Bir dikey pencere görünür **myVnetA** sanal ağ.
5. İçinde **myVnetA** görünür, dikey tıklayın **erişim denetimi (IAM)** dikey pencerenin sol tarafındaki seçenekleri dikey listesinden.
6. İçinde **myVnetA - erişim denetimi (IAM)** görünür, dikey tıklayın **+ Ekle**.
7. İçinde **izinleri eklemek** görünen seçin dikey **ağ Katılımcısı** içinde **rol** kutusu.
8. İçinde **seçin** kutusuna UserB seçin veya aramak için Kullanıcıb'in e-posta adresini yazın. Gösterilen kullanıcılar için eşlemeyi ayarlama ayarladığınız sanal ağ aynı Azure Active Directory kiracısındaki listesidir. Listesinde göründüğünde Kullanıcıb'ı tıklatın.
9. **Kaydet** düğmesine tıklayın.
10. UserA olarak portal dışında oturum sonra UserB oturum açın.
11. Tıklatın **+ yeni**, türü *sanal ağ* içinde **Market arama** kutusuna ve ardından **sanal ağ** arama sonuçlarında.
12. İçinde **sanal ağ** görünen seçin dikey **Klasik** içinde **dağıtım modeli seçin** kutusuna ve ardından **oluşturma**.
13.   Görüntülenen oluşturma sanal ağ (Klasik) kutusunda, aşağıdaki değerleri girin:

    - **Ad**: *myVnetB*
    - **Adres alanı**: *10.1.0.0/16*
    - **Alt ağ adı**: *varsayılan*
    - **Alt ağ adres aralığı**: *10.1.0.0/24*
    - **Abonelik**: Abonelik B. seçin
    - **Kaynak grubu**: seçin **Yeni Oluştur** ve girin *myResourceGroupB*
    - **Konum**: *Doğu ABD*

14. İçinde **arama kaynakları** kutusunu türü portalı üstündeki *myVnetB*. Tıklatın **myVnetB** arama sonuçlarında görüntülendiğinde. Bir dikey pencere görünür **myVnetB** sanal ağ.
15. İçinde **myVnetB** görünür, dikey tıklayın **özellikleri** dikey pencerenin sol tarafındaki seçenekleri dikey listesinden. Kopya **kaynak kimliği**, bir sonraki adımda kullanılır. Kaynak Kimliği aşağıdaki örneğe benzer: /subscriptions/<Susbscription ID>/resourceGroups/myResoureGroupB/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnetB
16. 5-9 myVnetB, girmek için adımları **Kullanıcıa** 8. adımda.
17. Portal UserB olarak dışında oturum ve UserA oturum açın.
18. İçinde **arama kaynakları** kutusunu türü portalı üstündeki *myVnetA*. Tıklatın **myVnetA** arama sonuçlarında görüntülendiğinde. Bir dikey pencere görünür **myVnet** sanal ağ.
19. Tıklatın **myVnetA**.
20. İçinde **myVnetA** görünür, dikey tıklayın **eşlemeler** dikey pencerenin sol tarafındaki seçenekleri dikey listesinden.
21. İçinde **myVnetA - eşlemeler** görünen dikey tıklayın **+ Ekle**
22. İçinde **Ekle eşliği** görünür, dikey girin veya aşağıdaki seçenekleri belirleyin ve ardından **Tamam**:
     - **Ad**: *myVnetAToMyVnetB*
     - **Sanal ağ dağıtım modeli**: seçin **Klasik**.
     - **Kaynak Kimliğimi biliyorum**: Bu onay kutusunu işaretleyin.
     - **Kaynak Kimliği**: 15. adımdan myVnetB kaynak Kimliğini girin.
     - **Sanal ağ erişimine izin ver:** emin **etkin** seçilir.
    Diğer bir ayarları, bu öğreticide kullanılır. Tüm eşleme ayarları hakkında bilgi için okuma [sanal ağ eşlemeleri yönetme](virtual-network-manage-peering.md#create-a-peering).
23. ' I tıklattıktan sonra **Tamam** önceki adımda **Ekle eşliği** dikey penceresi kapanır ve gördüğünüz **myVnetA - eşlemeler** dikey penceresini yeniden. Birkaç saniye sonra oluşturduğunuz eşliği dikey penceresinde görüntülenir. **Bağlı** içinde listelenen **EŞLİĞİ durumu** sütunu için **myVnetAToMyVnetB** sizin eşlemeyi oluşturuldu. Eşlemeyi şimdi oluşturulur. Sanal ağ (Resource Manager) sanal ağ (Klasik) eş gerek yoktur.

    Ya da sanal ağ içinde oluşturduğunuz Azure kaynaklarını IP adreslerini birbirleri ile iletişim kuramıyor. Varsayılan Azure ad çözümlemesi için sanal ağlar kullanıyorsanız, sanal ağlarda bulunan kaynaklar sanal ağlar arasında adlarını çözmek mümkün değildir. Bir eşlemesindeki sanal ağlar arasında adları çözümlemek dilerseniz kendi DNS sunucusu oluşturmanız gerekir. Nasıl ayarlanacağını öğrenin [kendi DNS sunucu kullanılarak ad çözümleme](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

24. **İsteğe bağlı**: sanal makineler oluştururken bu öğreticide kapsanmayan olsa, her sanal ağ içinde bir sanal makine oluşturun ve bir sanal makineden diğerine, bağlanabilirliği doğrulamak için bağlanın.
25. **İsteğe bağlı**: Bu öğreticide oluşturduğunuz kaynaklarını silmek için adımları tamamlamanız [silmek kaynakları](#delete-portal) bu makalenin.

## <a name="cli"></a>Eşlemesi - oluşturmak Azure CLI

Bu öğretici, her abonelik için farklı hesaplar kullanır. Her iki abonelikler için izinlere sahip bir hesap kullanıyorsanız, tüm adımlar için aynı hesabı kullanmak, günlük Azure dışında için adımları atlayın ve rol atamalarını kullanıcı oluşturma komut satırlarını kaldırın. Değiştir UserA@azure.com ve UserB@azure.com tüm UserA ve userb adlı için kullanmakta olduğunuz kullanıcı adları ile aşağıdaki betikler. 

Aşağıdaki adımları gerçekleştirmeden önce önizleme için kaydetmeniz gerekir. Kaydetmek için adımları tamamlamanız [Önizleme için kaydetmek](#register) bu makalenin. Önizleme için her iki abonelikler kaydettirmezseniz kalan adımları başarısız.

1. [Yükleme](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) sanal ağ (Klasik) oluşturmak için Azure CLI 1.0.
2. CLI oturumu açın ve Azure'a UserB kullanarak olarak oturum açma `azure login` komutu.
3. CLI girerek Hizmet Yönetimi modunda çalışacak `azure config mode asm` komutu.
4. Sanal ağ (Klasik) oluşturmak için aşağıdaki komutu girin:
 
    ```azurecli
    azure network vnet create --vnet myVnetB --address-space 10.1.0.0 --cidr 16 --location "East US"
    ```
5. Geri kalan adımları tamamlanmış bir bash Azure CLI 2.0.4 kabuğunda veya üstü gerekir [yüklü](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json), veya Azure bulut Kabuğu'nu kullanarak. Azure Cloud Shell doğrudan Azure portalının içinde çalıştırabileceğiniz ücretsiz bir Bash kabuğudur. Azure CLI, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Tıklatın **deneyin** bir bulut, size Azure hesabınızda oturum Kabuğu'nu açar, izleme komut düğmesi. Bir Windows istemcisinde CLI betikleri çalıştırma seçenekleri bash için bkz: [Windows Azure CLI çalıştıran](../virtual-machines/windows/cli-options.md?toc=%2fazure%2fvirtual-network%2ftoc.json). 
6. Aşağıdaki komut dosyası, bilgisayarınızdaki bir metin düzenleyicisine kopyalayın. Değiştir `<SubscriptionB-Id>` abonelik kimliğinizi içeren Aboneliğinizi kimliği bilmiyorsanız, girin `az account show` komutu. Değeri **kimliği** çıktıda, abonelik kimliği olan Değiştirilen betiği kopyalayın, CLI 2.0 oturumunuza yapıştırın ve ardından basın `Enter`. 

    ```azurecli-interactive
    az role assignment create \
      --assignee UserA@azure.com \
      --role "Classic Network Contributor" \
      --scope /subscriptions/<SubscriptionB-Id>/resourceGroups/Default-Networking/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnetB
    ```

    Sanal ağ (Klasik) 4. adımda oluşturduğunuz zaman, Azure sanal ağında oluşturulan *varsayılan ağ* kaynak grubu.
7. Azure ve oturum açma dışında UserB CLI 2.0 UserA olarak oturum açın.
8. Bir kaynak grubu ve sanal ağ (Resource Manager) oluşturun. Aşağıdaki komutu kopyalayın, CLI oturumunuza yapıştırın ve sonra basın `Enter`. 

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

9. Farklı dağıtım modelleri arasında oluşturulan iki sanal ağlar arasında eşleme sanal ağ oluşturun. Aşağıdaki komut dosyası, bilgisayarınızdaki bir metin düzenleyicisine kopyalayın. Değiştir `<SubscriptionB-id>` aboneliğinizle kimliği. Aboneliğinizi kimliği bilmiyorsanız, girin `az account show` komutu. Değeri **kimliği** çıktıda, abonelik kimliği olan Oluşturulan Azure adlı adım 4'te bir kaynak grubunda oluşturduğunuz sanal ağ (Klasik) *varsayılan ağ*. CLI oturumunuzda değiştirilmiş betiğini yapıştırın ve tuşuna basın `Enter`.

    ```azurecli-interactive
    # Peer VNet1 to VNet2.
    az network vnet peering create \
      --name myVnetAToMyVnetB \
      --resource-group $rgName \
      --vnet-name myVnetA \
      --remote-vnet-id  /subscriptions/<SubscriptionB-id>/resourceGroups/Default-Networking/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnetB \
      --allow-vnet-access
    ```

10. Betiği çalıştırdıktan sonra sanal ağ (Resource Manager) için eşleme gözden geçirin. Aşağıdaki komutu kopyalayın ve CLI oturumunuzda yapıştırın:

    ```azurecli-interactive
    az network vnet peering list \
      --resource-group $rgName \
      --vnet-name myVnetA \
      --output table
    ```
    Çıktısında **bağlı** içinde **PeeringState** sütun.

    Ya da sanal ağ içinde oluşturduğunuz Azure kaynaklarını IP adreslerini birbirleri ile iletişim kuramıyor. Varsayılan Azure ad çözümlemesi için sanal ağlar kullanıyorsanız, sanal ağlarda bulunan kaynaklar sanal ağlar arasında adlarını çözmek mümkün değildir. Bir eşlemesindeki sanal ağlar arasında adları çözümlemek dilerseniz kendi DNS sunucusu oluşturmanız gerekir. Nasıl ayarlanacağını öğrenin [kendi DNS sunucu kullanılarak ad çözümleme](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

11. **İsteğe bağlı**: sanal makineler oluştururken bu öğreticide kapsanmayan olsa, her sanal ağ içinde bir sanal makine oluşturun ve bir sanal makineden diğerine, bağlanabilirliği doğrulamak için bağlanın.
12. **İsteğe bağlı**: Bu öğreticide oluşturduğunuz kaynaklarını silmek için adımları tamamlamanız [silmek kaynakları](#delete-cli) bu makalede.

## <a name="powershell"></a>Eşlemesi - oluşturmak PowerShell

Bu öğretici, her abonelik için farklı hesaplar kullanır. Her iki abonelikler için izinlere sahip bir hesap kullanıyorsanız, tüm adımlar için aynı hesabı kullanmak, günlük Azure dışında için adımları atlayın ve rol atamalarını kullanıcı oluşturma komut satırlarını kaldırın. Değiştir UserA@azure.com ve UserB@azure.com tüm UserA ve userb adlı için kullanmakta olduğunuz kullanıcı adları ile aşağıdaki betikler. 

Aşağıdaki adımları gerçekleştirmeden önce önizleme için kaydetmeniz gerekir. Kaydetmek için adımları tamamlamanız [Önizleme için kaydetmek](#register) bu makalenin. Önizleme için her iki abonelikler kaydettirmezseniz kalan adımları başarısız.

1. PowerShell'ın en son sürümünü yüklemek [Azure](https://www.powershellgallery.com/packages/Azure) ve [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) modüller. Azure PowerShell'i kullanmaya yeni başladıysanız [Azure PowerShell'e genel bakış](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json) sayfasını inceleyin.
2. Bir PowerShell oturumu başlatın.
3. PowerShell'de, Kullanıcıb'in abonelik UserB olarak girerek oturum `Add-AzureAccount` komutu.
4. PowerShell ile bir sanal ağ (Klasik) oluşturmak için yeni bir oluşturun veya varolan, bir ağ yapılandırma dosyasını değiştirmeniz gerekir. Bilgi edinmek için nasıl [dışarı aktarma, güncelleştirme ve ağ yapılandırma dosyalarını içe](virtual-networks-using-network-configuration-file.md). Dosya aşağıdaki içermelidir **VirtualNetworkSite** öğesi Bu öğreticide kullanılan sanal ağ için:

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
    > Değiştirilen ağ yapılandırma dosyasını içeri varolan sanal ağlar (aboneliğinizde Klasik) yapılan değişiklikler neden olabilir. Yalnızca önceki sanal ağ ekleyin ve verme değiştirir veya var olan tüm sanal ağları aboneliğinizden kaldırırsanız, emin olun. 

5. Oturum açtığınızda Kullanıcıb'in abonelik girerek Resource Manager komutlarını kullanmak için UserB olarak `login-azurermaccount` komutu.
6. Kullanıcıa izinleri sanal ağa B. kopyalama bir metin düzenleyicisi PC ve değiştirmek için aşağıdaki komut dosyası atamak `<SubscriptionB-id>` B. abonelik kimliği Abonelik kimliği bilmiyorsanız, girin `Get-AzureRmSubscription` görüntülemek için komutu. Değeri **kimliği** döndürülen çıkış, abonelik kimliği. Oluşturulan Azure adlı adım 4'te bir kaynak grubunda oluşturduğunuz sanal ağ (Klasik) *varsayılan ağ*. Betik yürütmek için değiştirilmiş betiği kopyalayın, PowerShell yapıştırın ve ardından basın `Enter`.
    
    ```powershell 
    New-AzureRmRoleAssignment `
      -SignInName UserA@azure.com `
      -RoleDefinitionName "Classic Network Contributor" `
      -Scope /subscriptions/<SubscriptionB-id>/resourceGroups/Default-Networking/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnetB
    ```

7. Oturum UserB olarak Azure dışında ve oturum açma UserA olarak Kullanıcıa'nın aboneliğine girerek `login-azurermaccount` komutu. İle oturum için kullandığınız hesabın, bir sanal ağ eşlemesi oluşturmak için gerekli izinleri olmalıdır. Bkz: [izinleri](#permissions) Ayrıntılar için bu makalenin.
8. Aşağıdaki komut dosyası kopyalama için PowerShell içinde yapıştırma ve tuşuna basarak (Resource Manager) sanal ağ oluşturma `Enter`:

    ```powershell
    # Variables for common values
      $rgName='MyResourceGroupA'
      $location='eastus'

    # Create a resource group.
    New-AzureRmResourceGroup `
      -Name $rgName `
      -Location $location

    # Create virtual network A.
    $vnetA = New-AzureRmVirtualNetwork `
      -ResourceGroupName $rgName `
      -Name 'myVnetA' `
      -AddressPrefix '10.0.0.0/16' `
      -Location $location
    ```

9. Kullanıcıb izinleri myVnetA için atayın. Bir metin düzenleyicisi PC ve değiştirmek için aşağıdaki komutu kopyalayın `<SubscriptionA-Id>` A. abonelik kimliği Abonelik kimliği bilmiyorsanız, girin `Get-AzureRmSubscription` görüntülemek için komutu. Değeri **kimliği** döndürülen çıkış, abonelik kimliği. Komut dosyasının değiştirilmiş sürümünü PowerShell'de yapıştırın ve sonra basın `Enter` çalıştırmak üzere.

    ```powershell
    New-AzureRmRoleAssignment `
      -SignInName UserB@azure.com `
      -RoleDefinitionName "Network Contributor" `
      -Scope /subscriptions/<SubscriptionA-Id>/resourceGroups/myResourceGroupA/providers/Microsoft.Network/VirtualNetworks/myVnetA
    ```

10. Aşağıdaki komut dosyası, bilgisayarınızdaki bir metin düzenleyicisine kopyalayın ve değiştirme `<SubscriptionB-id>` B. abonelik kimliği MyVNetB myVnetA eş için değiştirilmiş betiği kopyalayın, PowerShell yapıştırın ve ardından basın `Enter`.

    ```powershell
    Add-AzureRmVirtualNetworkPeering `
      -Name 'myVnetAToMyVnetB' `
      -VirtualNetwork $vnetA `
      -RemoteVirtualNetworkId /subscriptions/<SubscriptionB-id>/resourceGroups/Default-Networking/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnetB
    ```

11. Aşağıdaki komut dosyası kopyalama, PowerShell içinde yapıştırma ve tuşuna basarak myVnetA eşleme durumunu görüntüleme `Enter`.

    ```powershell
    Get-AzureRmVirtualNetworkPeering `
      -ResourceGroupName $rgName `
      -VirtualNetworkName myVnetA `
      | Format-Table VirtualNetworkName, PeeringState
    ```

    Durumu **bağlı**. Dönüşür **bağlı** myVnetA için eşliği myVnetB ayarladıktan sonra.

    Ya da sanal ağ içinde oluşturduğunuz Azure kaynaklarını IP adreslerini birbirleri ile iletişim kuramıyor. Varsayılan Azure ad çözümlemesi için sanal ağlar kullanıyorsanız, sanal ağlarda bulunan kaynaklar sanal ağlar arasında adlarını çözmek mümkün değildir. Bir eşlemesindeki sanal ağlar arasında adları çözümlemek dilerseniz kendi DNS sunucusu oluşturmanız gerekir. Nasıl ayarlanacağını öğrenin [kendi DNS sunucu kullanılarak ad çözümleme](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

12. **İsteğe bağlı**: sanal makineler oluştururken bu öğreticide kapsanmayan olsa, her sanal ağ içinde bir sanal makine oluşturun ve bir sanal makineden diğerine, bağlanabilirliği doğrulamak için bağlanın.
13. **İsteğe bağlı**: Bu öğreticide oluşturduğunuz kaynaklarını silmek için adımları tamamlamanız [silmek kaynakları](#delete-powershell) bu makalede.

## <a name="permissions"></a>İzinleri

Bir sanal ağ eşlemesi oluşturmak için kullandığınız hesaplarının gerekli rol veya izinleri olması gerekir. Örneğin, myVnetA ve myVnetB adlı iki sanal ağ eşlemesi, hesabınızı aşağıdaki en düşük rolü veya her sanal ağ izinlerini atanmalıdır:
    
|Sanal ağ|Dağıtım modeli|Rol|İzinler|
|---|---|---|---|
|myVnetA|Resource Manager|[Ağ Katılımcısı](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write|
| |Klasik|[Klasik Ağ Katılımcısı](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|Yok|
|myVnetB|Resource Manager|[Ağ Katılımcısı](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/peer|
||Klasik|[Klasik Ağ Katılımcısı](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|Microsoft.ClassicNetwork/virtualNetworks/peer|

Daha fazla bilgi edinmek [yerleşik roller](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) ve belirli izinler atama [özel roller](../active-directory/role-based-access-control-custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (yalnızca Resource Manager).

## <a name="delete"></a>Kaynakları silin
Bu öğreticiyi tamamladığınızda, kullanım ücret ödememeniz öğreticide oluşturduğunuz kaynakları silmek isteyebilirsiniz. Bir kaynak grubunun silinmesi, kaynak grubunda bulunan tüm kaynakları siler.

### <a name="delete-portal"></a>Azure portalı

1. Portal arama kutusuna **myResourceGroupA**. Arama sonuçlarında tıklatın **myResourceGroupA**.
2. Üzerinde **myResourceGroupA** dikey penceresinde tıklatın **silmek** simgesi.
3. İçinde silmeyi onaylamak için **türü kaynak grubu adı** kutusuna **myResourceGroupA**ve ardından **silmek**.
4. İçinde **arama kaynakları** kutusunu türü portalı üstündeki *myVnetB*. Tıklatın **myVnetB** arama sonuçlarında görüntülendiğinde. Bir dikey pencere görünür **myVnetB** sanal ağ.
5. İçinde **myVnetB** dikey penceresinde tıklatın **silmek**.
6. Silme işlemini onaylamak için tıklatın **Evet** içinde **Delete sanal ağ** kutusu.

### <a name="delete-cli"></a>Azure CLI

1. Aşağıdaki komut ile sanal ağ (Resource Manager) silmek için CLI 2.0 kullanarak oturum açın:

    ```azurecli-interactive
    az group delete --name myResourceGroupA --yes
    ```

2. Azure sanal ağ (Klasik) aşağıdaki komutları ile silmek için Azure CLI 1.0 kullanarak oturum açın:

    ```azurecli
    azure config mode asm 

    azure network vnet delete --vnet myVnetB --quiet
    ```

### <a name="delete-powershell"></a>PowerShell

1. PowerShell komut isteminde (Resource Manager) sanal ağı silmek için aşağıdaki komutu girin:

    ```powershell
    Remove-AzureRmResourceGroup -Name myResourceGroupA -Force
    ```

2. Sanal ağı silmek için PowerShell ile (Klasik), mevcut bir ağ yapılandırma dosyasını değiştirmeniz gerekir. Bilgi edinmek için nasıl [dışarı aktarma, güncelleştirme ve ağ yapılandırma dosyalarını içe](virtual-networks-using-network-configuration-file.md). Bu öğreticide kullanılan sanal ağ için aşağıdaki VirtualNetworkSite öğeyi kaldırın:

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
    > Değiştirilen ağ yapılandırma dosyasını içeri varolan sanal ağlar (aboneliğinizde Klasik) yapılan değişiklikler neden olabilir. Önceki sanal ağı yalnızca kaldırmak ve verme değiştirir veya diğer varolan sanal ağlara aboneliğinizden kaldırırsanız, emin olun. 

## <a name="register"></a>Önizleme için kaydolun

Farklı Aboneliklerde bulunan farklı Azure dağıtım modelleri aracılığıyla oluşturulan sanal ağlar arası özelliği şu anda önizlemede değil. Önizleme özellikleri genel yayın aynı düzeyde kullanılabilirlik ve güvenilirlik özellikleri olarak olmayabilir. Kullanılabilirlik ve önizleme özellikleri durumunu en güncel bildirimleri için denetleme [Azure sanal ağı güncelleştirir](https://azure.microsoft.com/updates/?product=virtual-network) sayfası. 

Kullanabilmeniz için önce çapraz abonelik arası dağıtım modelini özelliği için kaydetmeniz gerekir. Azure PowerShell veya Azure CLI kullanarak eş istediğiniz her sanal ağ kullanılıyor abonelik içindeki aşağıdaki adımları tamamlayın:

### <a name="powershell"></a>PowerShell

1. PowerShell [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) modülünün en son sürümünü yükleyin. Azure PowerShell'i kullanmaya yeni başladıysanız [Azure PowerShell'e genel bakış](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json) sayfasını inceleyin.
2. Bir PowerShell oturumu başlatın ve oturum açma Azure kullanmaya `Login-AzureRmAccount` komutu.
3. Eş istediğiniz her sanal ağ içinde önizleme için aşağıdaki komutları girerek aboneliğinizin kaydedin:

    ```powershell
    Register-AzureRmProviderFeature `
      -FeatureName AllowClassicCrossSubscriptionPeering `
      -ProviderNamespace Microsoft.Network
    
    Register-AzureRmResourceProvider `
      -ProviderNamespace Microsoft.Network
    ```
4. Aşağıdaki komutu girerek Önizleme için kayıtlı olduklarını doğrulayın:

    ```powershell    
    Get-AzureRmProviderFeature `
      -FeatureName FeatureName AllowClassicCrossSubscriptionPeering `
      -ProviderNamespace Microsoft.Network
    ```

    Bu makalenin kadar Portal, Azure CLI, PowerShell veya Resource Manager şablonu bölümlerdeki adımları tamamlamayın **RegistrationState** önceki komutların girdikten sonra aldığınız çıktı  **Kayıtlı** hem abonelikler için.

> [!NOTE]
> Bu öğretici, mevcut sanal ağlar aynı bölgede kullanır. Sanal ağlar farklı bölgelerde eş olanağı da önizlemede değil. Çapraz bölge veya genel eşliği kaydetmek için adım 1-4 yeniden kullanarak tamamlamak `-FeatureName AllowGlobalVnetPeering` yerine `-FeatureName AllowClassicCrossSubscriptionPeering`. İki yetenekleri birbirinden bağımsızdır. Her ikisi de kullanmak istemediğiniz sürece her ikisi için kaydetmek gerekmez. Yetenek sınırlı sayıda bölgeler (başlangıçta, ABD Batı Orta Kanada merkezi ve ABD Batı 2) içinde kullanılabilir.

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

> [!NOTE]
> Bu öğretici, mevcut sanal ağlar aynı bölgede kullanır. Sanal ağlar farklı bölgelerde eş olanağı da önizlemede değil. Çapraz bölge veya genel eşliği kaydetmek için adımları 1-5 yeniden kullanarak tamamlamak `--name AllowGlobalVnetPeering` yerine `--name AllowClassicCrossSubscriptionPeering`. İki yetenekleri birbirinden bağımsızdır. Her ikisi de kullanmak istemediğiniz sürece her ikisi için kaydetmek gerekmez. Yetenek sınırlı sayıda bölgeler (başlangıçta, ABD Batı Orta Kanada merkezi ve ABD Batı 2) içinde kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

- Baştan sona ile önemli öğrenmeniz [sanal ağ eşleme kısıtlamaları ve davranışları](virtual-network-manage-peering.md#requirements-and-constraints) için üretim eşleme sanal ağ oluşturmadan önce kullanın.
- Tüm hakkında bilgi edinin [sanal ağ eşleme ayarları](virtual-network-manage-peering.md#create-a-peering).
- Bilgi edinmek için nasıl [bir hub oluşturmak ve ağ topolojisine bağlı bileşen](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) sanal ağ eşlemesi ile.
