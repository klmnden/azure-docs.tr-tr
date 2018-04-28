---
title: -Resource Manager - eşliği farklı Aboneliklerde Azure sanal ağı oluşturun | Microsoft Docs
description: Farklı Azure aboneliklerinde mevcut Resource Manager aracılığıyla oluşturulan sanal ağlar arasında eşleme sanal ağ oluşturmayı öğrenin.
services: virtual-network
documentationcenter: ''
author: jimdial
manager: jeconnoc
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
ms.openlocfilehash: 45856f759b7d11a7712a032a00d2d1a4fb2043d2
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="create-a-virtual-network-peering---resource-manager-different-subscriptions"></a>Bir sanal ağ eşlemesi - oluşturma, farklı Aboneliklerde Kaynak Yöneticisi'ni 

Bu öğreticide, Resource Manager aracılığıyla oluşturulan sanal ağlar arasında eşleme sanal ağ oluşturmayı öğrenin. Sanal ağlar farklı Aboneliklerde mevcut. Kaynaklar aynı sanal ağda gibi davranarak birbirleri ile aynı bant genişliği ve gecikme süresi ile iletişim kurmak için farklı sanal ağlar iki sanal ağlar etkinleştirir kaynaklarında eşleme. Daha fazla bilgi edinmek [sanal ağ eşlemesi](virtual-network-peering-overview.md). 

Sanal ağlar aynı ya da farklı olup, abonelikleri ve hangi bağlı olarak farklı bir sanal ağ eşlemesi oluşturmak için aşağıdaki adımları [Azure dağıtım modeli](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) sanal ağlar üzerinden oluşturulur. Aşağıdaki tablodan senaryo seçerek diğer senaryolarda eşleme sanal ağ oluşturmayı öğrenin:

|Azure dağıtım modeli  | Azure aboneliği  |
|--------- |---------|
|[Her iki kaynak yöneticisi](tutorial-connect-virtual-networks-portal.md) |Aynı|
|[Bir kaynak yöneticisi, bir Klasik](create-peering-different-deployment-models.md) |Aynı|
|[Bir kaynak yöneticisi, bir Klasik](create-peering-different-deployment-models-subscriptions.md) |Fark|

Klasik dağıtım modeli aracılığıyla dağıtılan iki sanal ağ arasında bir sanal ağ eşlemesi oluşturulamıyor. Klasik dağıtım modeli aracılığıyla her ikisi de oluşturulan sanal ağlara bağlanmak gerekiyorsa, Azure kullanabilirsiniz [VPN ağ geçidi](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) sanal ağlara bağlanma. 

Bu öğretici, sanal ağlar aynı bölgede eşlere. Ayrıca farklı sanal ağlar eş [bölgeler desteklenen](virtual-network-manage-peering.md#cross-region). 

Kullanabileceğiniz [Azure portal](#portal), Azure [komut satırı arabirimi](#cli) (CLI) Azure [PowerShell](#powershell), veya bir [Azure Resource Manager şablonu](#template)bir sanal ağ eşlemesi oluşturmak için. Doğrudan seçeceğiniz araç kullanarak bir sanal ağ eşlemesi oluşturma adımlarını gitmek için önceki aracı bağlantılardan birini seçin.

## <a name="portal"></a>Eşlemesi - oluşturmak Azure portalı

Bu öğretici, her abonelik için farklı hesaplar kullanır. Her iki abonelikler için izinlere sahip bir hesap kullanıyorsanız, tüm adımlar için aynı hesabı kullanmak, portal dışında günlüğü için adımları atlayın ve sanal ağlar için başka bir kullanıcı izinlerini atamak için adımları atlayın.

1. Oturum [Azure portal](https://portal.azure.com) olarak *Kullanıcıa*. İle oturum için kullandığınız hesabın, bir sanal ağ eşlemesi oluşturmak için gerekli izinleri olmalıdır. İzinlerin bir listesi için bkz: [sanal ağ eşleme izinleri](virtual-network-manage-peering.md#permissions).
2. Seçin **+ kaynak oluşturma**seçin **ağ**ve ardından **sanal ağ**.
3. Seçin veya aşağıdaki ayarları için aşağıdaki örneği değerleri girin ve ardından **oluşturma**:
    - **Ad**: *myVnetA*
    - **Adres alanı**: *10.0.0.0/16*
    - **Alt ağ adı**: *varsayılan*
    - **Alt ağ adres aralığı**: *10.0.0.0/24*
    - **Abonelik**: A. abonelik seçin
    - **Kaynak grubu**: seçin **Yeni Oluştur** ve girin *myResourceGroupA*
    - **Konum**: *Doğu ABD*
4. İçinde **arama kaynakları** kutusunu türü portalı üstündeki *myVnetA*. Seçin **myVnetA** arama sonuçlarında görüntülendiğinde. 
5. Seçin **erişim denetimi (IAM)** seçenekleri sol taraftaki dikey listesinden.
6. Altında **myVnetA - erişim denetimi (IAM)** seçin **+ Ekle**.
7. Seçin **ağ Katılımcısı** içinde **rol** kutusu.
8. İçinde **seçin** kutusunda, seçin *UserB*, veya aramak için Kullanıcıb'in e-posta adresini yazın. Gösterilen kullanıcılar için eşlemeyi ayarlama ayarladığınız sanal ağ aynı Azure Active Directory kiracısındaki listesidir. Kullanıcıb görmüyorsanız, UserB Kullanıcıa'den farklı bir Active Directory kiracısı içinde olduğundan olası değildir. Farklı Active Directory kiracılar sanal ağlara bağlanmak isterseniz bağlanabilir bir [Azure VPN ağ geçidi](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md), bir sanal ağ eşlemesi yerine.
9. **Kaydet**’i seçin.
10. Altında **myVnetA - erişim denetimi (IAM)** seçin **özellikleri** seçenekleri sol taraftaki dikey listesinden. Kopya **kaynak kimliği**, bir sonraki adımda kullanılır. Kaynak Kimliği aşağıdaki örneğe benzer: /subscriptions/<Subscription Id>/resourceGroups/myResourceGroupA/providers/Microsoft.Network/virtualNetworks/myVnetA.
11. UserA olarak portal dışında oturum sonra UserB oturum açın.
12. 2-3 girmek veya 3. adımda aşağıdaki değerleri seçerek, adımları tamamlayın:

    - **Ad**: *myVnetB*
    - **Adres alanı**: *10.1.0.0/16*
    - **Alt ağ adı**: *varsayılan*
    - **Alt ağ adres aralığı**: *10.1.0.0/24*
    - **Abonelik**: Abonelik B. seçin
    - **Kaynak grubu**: seçin **Yeni Oluştur** ve girin *myResourceGroupB*
    - **Konum**: *Doğu ABD*

13. İçinde **arama kaynakları** kutusunu türü portalı üstündeki *myVnetB*. Seçin **myVnetB** arama sonuçlarında görüntülendiğinde.
14. Altında **myVnetB**seçin **özellikleri** seçenekleri sol taraftaki dikey listesinden. Kopya **kaynak kimliği**, bir sonraki adımda kullanılır. Kaynak Kimliği aşağıdaki örneğe benzer: /subscriptions/<Susbscription ID>/resourceGroups/myResoureGroupB/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnetB.
15. Seçin **erişim denetimi (IAM)** altında **myVnetB**ve ardından myVnetB, girmek için 5-10 adımları tamamlayın **Kullanıcıa** 8. adımda.
16. Portal UserB olarak dışında oturum ve UserA oturum açın.
17. İçinde **arama kaynakları** kutusunu türü portalı üstündeki *myVnetA*. Seçin **myVnetA** arama sonuçlarında görüntülendiğinde.
18. Seçin **myVnetA**.
19. Altında **ayarları**seçin **eşlemeler**.
20. Altında **myVnetA - eşlemeler**seçin **+ Ekle**
21. Altında **Ekle eşliği**, girin veya, aşağıdaki seçenekleri belirleyin ve ardından seçin **Tamam**:
     - **Ad**: *myVnetAToMyVnetB*
     - **Sanal ağ dağıtım modeli**: seçin **Resource Manager**.
     - **Kaynak Kimliğimi biliyorum**: Bu onay kutusunu işaretleyin.
     - **Kaynak Kimliği**: 14. adımdan kaynak kimliği girin.
     - **Sanal ağ erişimine izin ver:** emin **etkin** seçilir.
    Diğer bir ayarları, bu öğreticide kullanılır. Tüm eşleme ayarları hakkında bilgi için okuma [sanal ağ eşlemeleri yönetme](virtual-network-manage-peering.md#create-a-peering).
22. Oluşturduğunuz eşliği seçtikten sonra kısa bir bekleme görünür **Tamam** önceki adımda. **Başlatılan** içinde listelenen **EŞLİĞİ durumu** sütunu için **myVnetAToMyVnetB** sizin eşlemeyi oluşturuldu. MyVnetB myVnetA eşlenen, ancak şimdi myVnetA myVnetB eş gerekir. Eşlemeyi birbirleri ile iletişim kurmak üzere sanal ağ kaynaklarında etkinleştirmek için her iki yönde oluşturulmalıdır.
23. UserA olarak portal dışında oturum ve UserB oturum açın.
24. Yeniden myVnetB için 17 21 adımları tamamlayın. Eşlemeyi 21. adımda ad *myVnetBToMyVnetA*seçin *myVnetA* için **sanal ağ**ve 10 adımda kimliği girin **kaynak kimliği** kutusu.
25. Seçtikten sonra birkaç saniye **Tamam** myVnetB için eşlemesi oluşturmak için **myVnetBToMyVnetA** , yeni oluşturduğunuz eşliği ile listelendiğini **bağlı** içinde**EŞLİĞİ durum** sütun.
26. Portal UserB olarak dışında oturum ve UserA oturum açın.
27. Adımları 17-19 yeniden tamamlayın. **EŞLİĞİ durumu** için **myVnetAToVNetB** eşliği olan şimdi de **bağlı**. Gördükten sonra Eşleme başarıyla kurulduktan **bağlı** içinde **EŞLİĞİ durum** sütun eşlemesi içindeki iki sanal ağlar için. Ya da sanal ağ içinde oluşturduğunuz Azure kaynaklarını IP adreslerini birbirleri ile iletişim kuramıyor. Varsayılan Azure ad çözümlemesi için sanal ağlar kullanıyorsanız, sanal ağlarda bulunan kaynaklar sanal ağlar arasında adlarını çözmek mümkün değildir. Bir eşlemesindeki sanal ağlar arasında adları çözümlemek dilerseniz kendi DNS sunucusu oluşturmanız gerekir. Nasıl ayarlanacağını öğrenin [kendi DNS sunucu kullanılarak ad çözümleme](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server).
28. **İsteğe bağlı**: sanal makineler oluştururken bu öğreticide kapsanmayan olsa, her sanal ağ içinde bir sanal makine oluşturun ve bir sanal makineden diğerine, bağlanabilirliği doğrulamak için bağlanın.
29. **İsteğe bağlı**: Bu öğreticide oluşturduğunuz kaynaklarını silmek için adımları tamamlamanız [silmek kaynakları](#delete-portal) bu makalenin.

## <a name="cli"></a>Eşlemesi - oluşturmak Azure CLI

Bu öğretici, her abonelik için farklı hesaplar kullanır. Her iki abonelikler için izinlere sahip bir hesap kullanıyorsanız, tüm adımlar için aynı hesabı kullanmak, günlük Azure dışında için adımları atlayın ve rol atamalarını kullanıcı oluşturma komut satırlarını kaldırın. Değiştir UserA@azure.com ve UserB@azure.com tüm UserA ve userb adlı için kullanmakta olduğunuz kullanıcı adları ile aşağıdaki betikler. Eş istediğiniz sanal ağlar her ikisi de aynı Azure Active Directory Kiracı ilişkili abonelik olması gerekir.  Farklı Active Directory kiracılar sanal ağlara bağlanmak isterseniz bağlanabilir bir [Azure VPN ağ geçidi](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md), bir sanal ağ eşlemesi yerine.

Aşağıdaki komut dosyaları:

- Azure CLI Sürüm 2.0.4 gerektirir veya sonraki bir sürümü. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükseltme gerekiyorsa, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json).
- Bir Bash kabuğunda çalışır. Windows istemcisi üzerinde Azure CLI betikleri çalıştırma seçenekleri için bkz. [Windows’da Azure CLI çalıştırma](../virtual-machines/windows/cli-options.md?toc=%2fazure%2fvirtual-network%2ftoc.json). 

CLI ve bağımlılıklarını yüklemek yerine, Azure bulut Kabuğu'nu kullanabilirsiniz. Azure Cloud Shell doğrudan Azure portalının içinde çalıştırabileceğiniz ücretsiz bir Bash kabuğudur. Azure CLI, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Seçin **deneyin** Azure hesabınızla oturum bir bulut Kabuk çağıran aşağıdaki komut düğmesi. 

1. CLI oturumu açın ve Azure'a Kullanıcıa kullanarak olarak oturum açma `azure login` komutu. İle oturum için kullandığınız hesabın, bir sanal ağ eşlemesi oluşturmak için gerekli izinleri olmalıdır. İzinlerin bir listesi için bkz: [sanal ağ eşleme izinleri](virtual-network-manage-peering.md#permissions).
2. Aşağıdaki komut dosyası, bilgisayarınızdaki bir metin düzenleyicisine kopyalayın, yerine `<SubscriptionA-Id>` kimliği, SubscriptionA ile sonra değiştirilmiş betiği kopyalayın, CLI oturum ve tuşuna yapıştırın `Enter`. Aboneliğinizi kimliği bilmiyorsanız, 'az hesabı Göster' komutunu girin. Değeri **kimliği** çıktıda, abonelik kimliği olan

    ```azurecli-interactive
    # Create a resource group.
    az group create \
      --name myResourceGroupA \
      --location eastus

    # Create virtual network A.
    az network vnet create \
      --name myVnetA \
      --resource-group myResourceGroupA \
      --location eastus \
      --address-prefix 10.0.0.0/16

    # Assign UserB permissions to virtual network A.
    az role assignment create \
      --assignee UserB@azure.com \
      --role "Network Contributor" \
      --scope /subscriptions/<SubscriptionA-Id>/resourceGroups/myResourceGroupA/providers/Microsoft.Network/VirtualNetworks/myVnetA
    ```
    
3. Azure oturumunu Kullanıcıa kullanarak olarak oturum `az logout` komut sonra Azure UserB olarak oturum açın. İle oturum için kullandığınız hesabın, bir sanal ağ eşlemesi oluşturmak için gerekli izinleri olmalıdır. İzinlerin bir listesi için bkz: [sanal ağ eşleme izinleri](virtual-network-manage-peering.md#permissions).
4. MyVnetB oluşturun. Komut dosyası içeriğini 2. adımda, bilgisayarınızdaki bir metin düzenleyicisine kopyalayın. Değiştir `<SubscriptionA-Id>` SubscriptionB kimliği. Değişiklik 10.0.0.0/16 10.1.0.0/16, tüm B için değişiklik ve tüm Bs değiştirilmiş betik A. Kopyalanacak yapıştırın, içinde CLI oturum ve tuşuna `Enter`. 
5. Dışında Azure UserB olarak oturum açın ve Azure UserA olarak oturum açın.
6. MyVnetA myVnetB için eşliği bir sanal ağ oluşturun. Aşağıdaki komut dosyası içeriği, bilgisayarınızdaki bir metin düzenleyicisine kopyalayın. Değiştir `<SubscriptionB-Id>` SubscriptionB kimliği. Betik yürütmek için değiştirilmiş betiği kopyalayın, CLI oturumunuza yapıştırın ve Enter tuşuna basın.
 
    ```azurecli-interactive
        # Get the id for myVnetA.
        vnetAId=$(az network vnet show \
          --resource-group myResourceGroupA \
          --name myVnetA \
          --query id --out tsv)
    
        # Peer myVNetA to myVNetB.
        az network vnet peering create \
          --name myVnetAToMyVnetB \
          --resource-group myResourceGroupA \
          --vnet-name myVnetA \
          --remote-vnet-id /subscriptions/<SubscriptionB-Id>/resourceGroups/myResourceGroupB/providers/Microsoft.Network/VirtualNetworks/myVnetB \
          --allow-vnet-access
    ```

7. MyVnetA eşleme durumunu görüntüleyin.

    ```azurecli-interactive
    az network vnet peering list \
      --resource-group myResourceGroupA \
      --vnet-name myVnetA \
      --output table
    ```

    Durumu **başlatılan**. Dönüşür **bağlı** myVnetA için myVnetB eşliği oluşturduktan sonra.

8. Azure'dan Kullanıcıa çıkışı oturum ve Azure UserB olarak oturum açın.
9. MyVnetB myVnetA için eşleme oluşturun. Komut dosyası içeriğini 6. adımda, bilgisayarınızdaki bir metin düzenleyicisine kopyalayın. Değiştir `<SubscriptionB-Id>` SubscriptionA ve B ve a bilgisayarına tüm Bs ilişkin tüm değişiklik için kimliği Değişiklikleri yaptıktan sonra değiştirilmiş betiği kopyalayın, CLI oturum ve tuşuna yapıştırma `Enter`.
10. MyVnetB eşleme durumunu görüntüleyin. Komut dosyası içeriğini 7. adımda, bilgisayarınızdaki bir metin düzenleyicisine kopyalayın. Değişiklik A için kaynak grubu ve sanal ağ adları, B betiği kopyalayın, CLI oturumunuz için değiştirilmiş betik yapıştırın ve tuşuna `Enter`. Eşleme durumu **bağlı**. MyVnetA eşleme durumu değişiklikleri **bağlı** myVnetB myVnetA için eşliği oluşturduktan sonra. Kullanıcıa oturum geri Azure ve eksiksiz yeniden myVnetA eşleme durumunu doğrulamak için 7. adımda. 

    > [!NOTE]
    > Eşleme duruma gelene kadar eşlemeyi kurulmaz **bağlı** her iki sanal ağlar için.

11. **İsteğe bağlı**: sanal makineler oluştururken bu öğreticide kapsanmayan olsa, her sanal ağ içinde bir sanal makine oluşturun ve bir sanal makineden diğerine, bağlanabilirliği doğrulamak için bağlanın.
12. **İsteğe bağlı**: Bu öğreticide oluşturduğunuz kaynaklarını silmek için adımları tamamlamanız [silmek kaynakları](#delete-cli) bu makalede.

Ya da sanal ağ içinde oluşturduğunuz Azure kaynaklarını IP adreslerini birbirleri ile iletişim kuramıyor. Varsayılan Azure ad çözümlemesi için sanal ağlar kullanıyorsanız, sanal ağlarda bulunan kaynaklar sanal ağlar arasında adlarını çözmek mümkün değildir. Bir eşlemesindeki sanal ağlar arasında adları çözümlemek dilerseniz kendi DNS sunucusu oluşturmanız gerekir. Nasıl ayarlanacağını öğrenin [kendi DNS sunucu kullanılarak ad çözümleme](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server).
 
## <a name="powershell"></a>Eşlemesi - oluşturmak PowerShell

Bu öğretici, her abonelik için farklı hesaplar kullanır. Her iki abonelikler için izinlere sahip bir hesap kullanıyorsanız, tüm adımlar için aynı hesabı kullanmak, günlük Azure dışında için adımları atlayın ve rol atamalarını kullanıcı oluşturma komut satırlarını kaldırın. Değiştir UserA@azure.com ve UserB@azure.com tüm UserA ve userb adlı için kullanmakta olduğunuz kullanıcı adları ile aşağıdaki betikler. Eş istediğiniz sanal ağlar her ikisi de aynı Azure Active Directory Kiracı ilişkili abonelik olması gerekir.  Farklı Active Directory kiracılar sanal ağlara bağlanmak isterseniz bağlanabilir bir [Azure VPN ağ geçidi](../vpn-gateway/vpn-gateway-howto-vnet-vnet-cli.md), bir sanal ağ eşlemesi yerine.

1. PowerShell [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) modülünün en son sürümünü yükleyin. Azure PowerShell'i kullanmaya yeni başladıysanız [Azure PowerShell'e genel bakış](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json) sayfasını inceleyin.
2. Bir PowerShell oturumu başlatın.
3. PowerShell'de, Azure'da UserA girerek oturum açma `Connect-AzureRmAccount` komutu. İle oturum için kullandığınız hesabın, bir sanal ağ eşlemesi oluşturmak için gerekli izinleri olmalıdır. İzinlerin bir listesi için bkz: [sanal ağ eşleme izinleri](virtual-network-manage-peering.md#permissions).
4. Bir kaynak grubu ve sanal ağ A. kopyalama bir metin düzenleyicisi için aşağıdaki betiği bilgisayarınıza oluşturun. Değiştir `<SubscriptionA-Id>` SubscriptionA kimliği. Aboneliğinizi kimliği bilmiyorsanız, girin `Get-AzureRmSubscription` görüntülemek için komutu. Değeri **kimliği** döndürülen çıkış, abonelik kimliği. Betik yürütmek için değiştirilmiş betiği kopyalayın, PowerShell yapıştırın ve ardından basın `Enter`.

    ```powershell
    # Create a resource group.
    New-AzureRmResourceGroup `
      -Name MyResourceGroupA `
      -Location eastus

    # Create virtual network A.
    $vNetA = New-AzureRmVirtualNetwork `
      -ResourceGroupName MyResourceGroupA `
      -Name 'myVnetA' `
      -AddressPrefix '10.0.0.0/16' `
      -Location eastus

    # Assign UserB permissions to myVnetA.
    New-AzureRmRoleAssignment `
      -SignInName UserB@azure.com `
      -RoleDefinitionName "Network Contributor" `
      -Scope /subscriptions/<SubscriptionA-Id>/resourceGroups/myResourceGroupA/providers/Microsoft.Network/VirtualNetworks/myVnetA
    ```

5. Azure'dan Kullanıcıa çıkışı ve UserB oturum. İle oturum için kullandığınız hesabın, bir sanal ağ eşlemesi oluşturmak için gerekli izinleri olmalıdır. İzinlerin bir listesi için bkz: [sanal ağ eşleme izinleri](virtual-network-manage-peering.md#permissions).
6. Komut dosyası içeriğini 4. adımda, bilgisayarınızdaki bir metin düzenleyicisine kopyalayın. Değiştir `<SubscriptionA-Id>` B. değişiklik 10.0.0.0/16 10.1.0.0/16 için abonelik kimliği. Tüm B ve tüm Bs konusunda A'ya değiştirme Betik yürütmek için değiştirilmiş betiği kopyalayın, PowerShell içinde yapıştırın ve ardından basın `Enter`.
7. Azure'dan UserB çıkışı ve UserA oturum.
8. MyVnetA myVnetB için eşleme oluşturun. Aşağıdaki komut dosyası, bilgisayarınızdaki bir metin düzenleyicisine kopyalayın. Değiştir `<SubscriptionB-Id>` B. abonelik kimliği Betik yürütmek için değiştirilmiş betiği kopyalayın, PowerShell yapıştırın ve ardından basın `Enter`.
 
    ```powershell
    # Peer myVnetA to myVnetB.
    $vNetA=Get-AzureRmVirtualNetwork -Name myVnetA -ResourceGroupName myResourceGroupA
    Add-AzureRmVirtualNetworkPeering `
      -Name 'myVnetAToMyVnetB' `
      -VirtualNetwork $vNetA `
      -RemoteVirtualNetworkId "/subscriptions/<SubscriptionB-Id>/resourceGroups/myResourceGroupB/providers/Microsoft.Network/virtualNetworks/myVnetB"
    ```

9. MyVnetA eşleme durumunu görüntüleyin.

    ```powershell
    Get-AzureRmVirtualNetworkPeering `
      -ResourceGroupName myResourceGroupA `
      -VirtualNetworkName myVnetA `
      | Format-Table VirtualNetworkName, PeeringState
    ```

    Durumu **başlatılan**. Dönüşür **bağlı** myVnetA için eşliği myVnetB ayarladıktan sonra.

10. Azure'dan Kullanıcıa çıkışı ve UserB oturum.
11. MyVnetB myVnetA için eşleme oluşturun. Komut dosyası içeriğini 8. adımda, bilgisayarınızdaki bir metin düzenleyicisine kopyalayın. Değiştir `<SubscriptionB-Id>` abonelik A ve B ve a bilgisayarına tüm Bs ilişkin tüm değişiklik kimliği Betik yürütmek için değiştirilmiş betiği kopyalayın, PowerShell yapıştırın ve ardından basın `Enter`.
12. MyVnetB eşleme durumunu görüntüleyin. Komut dosyası içeriğini 9. adımda, bilgisayarınızdaki bir metin düzenleyicisine kopyalayın. Kaynak grubu ve sanal ağ adları için B A Değiştir. Betik yürütmek için PowerShell içinde değiştirilen betiğini yapıştırın ve tuşuna `Enter`. Durumu **bağlı**. Eşleme durumunu **myVnetA** değişikliklerini **bağlı** gelen eşliği oluşturduktan sonra **myVnetB** için **myVnetA**. Kullanıcıa oturum geri Azure ve eksiksiz yeniden myVnetA eşleme durumunu doğrulamak için 9. adım. 

    > [!NOTE]
    > Eşleme duruma gelene kadar eşlemeyi kurulmaz **bağlı** her iki sanal ağlar için.

    Ya da sanal ağ içinde oluşturduğunuz Azure kaynaklarını IP adreslerini birbirleri ile iletişim kuramıyor. Varsayılan Azure ad çözümlemesi için sanal ağlar kullanıyorsanız, sanal ağlarda bulunan kaynaklar sanal ağlar arasında adlarını çözmek mümkün değildir. Bir eşlemesindeki sanal ağlar arasında adları çözümlemek dilerseniz kendi DNS sunucusu oluşturmanız gerekir. Nasıl ayarlanacağını öğrenin [kendi DNS sunucu kullanılarak ad çözümleme](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server).

13. **İsteğe bağlı**: sanal makineler oluştururken bu öğreticide kapsanmayan olsa, her sanal ağ içinde bir sanal makine oluşturun ve bir sanal makineden diğerine, bağlanabilirliği doğrulamak için bağlanın.
14. **İsteğe bağlı**: Bu öğreticide oluşturduğunuz kaynaklarını silmek için adımları tamamlamanız [silmek kaynakları](#delete-powershell) bu makalede.

## <a name="template"></a>Eşlemesi - oluşturmak Resource Manager şablonu

1. Bir sanal ağ oluşturmak ve uygun atamak için [izinleri](virtual-network-manage-peering.md#permissions), bölümündeki adımları tamamlamanız [Portal](#portal), [Azure CLI](#cli), veya [PowerShell](#powershell)bu makalenin bölümler.
2. Bir dosyaya, yerel bilgisayarınızda aşağıdaki metni kaydedin. Değiştir `<subscription ID>` Kullanıcıa'nın aboneliği ID'ye sahip Örneğin vnetpeeringA.json dosyayı kaydedin.

    ```json
    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
    "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "myVnetA/myVnetAToMyVnetB",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "/subscriptions/<subscription ID>/resourceGroups/PeeringTest/providers/Microsoft.Network/virtualNetworks/myVnetB"
                }
            }
            }
        ]
    }
    ```

3. Azure UserA olarak oturum açın ve şablonunu kullanarak dağıtın [portal](../azure-resource-manager/resource-group-template-deploy-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#deploy-resources-from-custom-template), [PowerShell](../azure-resource-manager/resource-group-template-deploy.md?toc=%2fazure%2fvirtual-network%2ftoc.json#deploy-a-template-from-your-local-machine), veya [Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json#deploy-local-template). Örnek json metni için 2. adımda kaydettiğiniz dosya adı belirtin.
4. Adım 2 örnek json bir dosyayı bilgisayarınıza kopyalayın ve ile başlayan satırlar için değişiklikleri yapın:
    - **ad**: değişiklik *myVnetA/myVnetAToMyVnetB* için *myVnetB/myVnetBToMyVnetA*.
    - **Kimliği**: Değiştir `<subscription ID>` Kullanıcıb'in abonelik kimliği ve değişiklik *myVnetB* için *myVnetA*.
5. 3. adım tam yeniden Azure'a UserB oturum açmış.
6. **İsteğe bağlı**: sanal makineler oluştururken bu öğreticide kapsanmayan olsa, her sanal ağ içinde bir sanal makine oluşturun ve bir sanal makineden diğerine, bağlanabilirliği doğrulamak için bağlanın.
7. **İsteğe bağlı**: Bu öğreticide oluşturduğunuz kaynaklarını silmek için adımları tamamlamanız [silmek kaynakları](#delete) Azure portalı, PowerShell veya Azure CLI kullanarak, bu makalenin bölümüne.

## <a name="delete"></a>Kaynakları silin
Bu öğreticiyi tamamladığınızda, kullanım ücret ödememeniz öğreticide oluşturduğunuz kaynakları silmek isteyebilirsiniz. Bir kaynak grubunun silinmesi, kaynak grubunda bulunan tüm kaynakları siler.

### <a name="delete-portal"></a>Azure portalı

1. Azure portalında UserA oturum açın.
2. Portal arama kutusuna **myResourceGroupA**. Arama sonuçlarında seçin **myResourceGroupA**.
3. **Sil**’i seçin.
4. İçinde silmeyi onaylamak için **türü kaynak grubu adı** kutusuna **myResourceGroupA**ve ardından **silmek**.
5. UserA olarak portal dışında oturum ve UserB oturum açın.
6. MyResourceGroupB için 2-4 arası adımları tamamlayın.

### <a name="delete-cli"></a>Azure CLI

1. Azure UserA olarak oturum açın ve aşağıdaki komutu yürütün:

    ```azurecli-interactive
    az group delete --name myResourceGroupA --yes
    ```
2. UserA olarak Azure dışında oturum ve UserB oturum açın.
3. Aşağıdaki komutu yürütün:

    ```azurecli-interactive
    az group delete --name myResourceGroupB --yes
    ```

### <a name="delete-powershell"></a>PowerShell

1. Azure UserA olarak oturum açın ve aşağıdaki komutu yürütün:

    ```powershell
    Remove-AzureRmResourceGroup -Name myResourceGroupA -force
    ```

2. UserA olarak Azure dışında oturum ve UserB oturum açın.
3. Aşağıdaki komutu yürütün:

    ```powershell
    Remove-AzureRmResourceGroup -Name myResourceGroupB -force
    ```

## <a name="next-steps"></a>Sonraki adımlar

- Baştan sona ile önemli öğrenmeniz [sanal ağ eşleme kısıtlamaları ve davranışları](virtual-network-manage-peering.md#requirements-and-constraints) için üretim eşleme sanal ağ oluşturmadan önce kullanın.
- Tüm hakkında bilgi edinin [sanal ağ eşleme ayarları](virtual-network-manage-peering.md#create-a-peering).
- Bilgi edinmek için nasıl [bir hub oluşturmak ve ağ topolojisine bağlı bileşen](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) sanal ağ eşlemesi ile.
