---
title: Bir Azure sanal ağ eşleme - Resource Manager - farklı abonelikler oluşturun
titlesuffix: Azure Virtual Network
description: Resource Manager aracılığıyla oluşturulan Azure farklı Aboneliklerde bulunan sanal ağlar arasında eşleme bir sanal ağ oluşturmayı öğrenin.
services: virtual-network
documentationcenter: ''
author: anavinahar
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/09/2019
ms.author: anavin
ms.openlocfilehash: cf414cf08771090990775d124e27222e51f786e2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66122009"
---
# <a name="create-a-virtual-network-peering---resource-manager-different-subscriptions"></a>Bir sanal ağ eşleme - oluşturma-Resource Manager, farklı abonelikler

Bu öğreticide, Resource Manager ile oluşturulmuş sanal ağlar arasındaki eşleme bir sanal ağ oluşturmayı öğrenin. Farklı Aboneliklerde bulunan sanal ağlar yok. Kaynaklar aynı sanal ağda gibi davranarak birbiriyle aynı bant genişliği ve gecikme süresi ile iletişim kurmak için farklı sanal ağlardaki iki sanal ağları etkinleştirir kaynak eşlemesi. Daha fazla bilgi edinin [sanal ağ eşlemesi](virtual-network-peering-overview.md).

Sanal ağlar aynı veya farklı olup, abonelikleri ve hangi bağlı olarak farklı bir sanal ağ eşlemesi oluşturmak için adımları [Azure dağıtım modeli](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) sanal ağlar üzerinden oluşturulur. Aşağıdaki tablodan senaryo'ı seçerek diğer senaryolarda eşleme bir sanal ağ oluşturmayı öğrenin:

|Azure dağıtım modeli  | Azure aboneliği  |
|--------- |---------|
|[Her iki kaynak yöneticisi](tutorial-connect-virtual-networks-portal.md) |Aynı|
|[Bir Resource Manager, diğeri Klasik](create-peering-different-deployment-models.md) |Aynı|
|[Bir Resource Manager, diğeri Klasik](create-peering-different-deployment-models-subscriptions.md) |Fark|

Bir sanal ağ eşlemesi iki sanal ağı Klasik dağıtım modeliyle dağıtılan arasında oluşturulamıyor. Klasik dağıtım modeliyle her ikisi de oluşturulan sanal ağlar bağlanmanız gerekirse, bir Azure kullanabileceğiniz [VPN ağ geçidi](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) sanal ağları bağlamak için.

Bu öğreticide, aynı bölgedeki sanal ağlar eşler. Ayrıca farklı sanal ağlarda eş [desteklenen bölgeler](virtual-network-manage-peering.md#cross-region). Önerilen ile kendinizi alıştırın [eşleme gereksinimleri ve kısıtlamaları](virtual-network-manage-peering.md#requirements-and-constraints) önce sanal ağları eşleme.

Kullanabileceğiniz [Azure portalında](#portal), Azure [komut satırı arabirimi](#cli) (CLI), Azure [PowerShell](#powershell), veya bir [Azure Resource Manager şablonu](#template)bir sanal ağ eşlemesi oluşturmak için. Doğrudan, tercih ettiğiniz araç kullanarak bir sanal ağ eşlemesi oluşturma adımlarını gitmek için önceki aracı bağlantılardan birini seçin.

## <a name="portal"></a>Oluşturma eşleme - Azure portalı

Eşlemek istediğiniz sanal ağlar farklı Azure Active Directory kiracılarıyla ilişkili Aboneliklerdeki varsa, bu makalede CLI ve PowerShell bölümdeki adımları izleyin. Portal, Active Directory kiracıların bilet farklı aboneliklere ait sanal ağları eşleyebilme desteği yok. 

Cloud Shell sınırlamaları abonelikleri ve kiracılar hangi nedeniyle genel farklı Azure Active Directory kiracılarındaki aboneliklere ait sanal ağ arasında VNet eşlemesi veya sanal ağ eşlemesi çalışmaz geçişi içinde olduğuna dikkat edin. PowerShell veya CLI'yı kullanın.

Aşağıdaki adımlar, her abonelik için farklı hesaplar kullanma. İki abonelik için izinleri olan bir hesap kullanıyorsanız, tüm adımlar için aynı hesabı kullanmak, günlük portalından adımları atlayın ve sanal ağlar için başka bir kullanıcı izinleri atama adımlarını atlayın.

1. Oturum [Azure portalında](https://portal.azure.com) olarak *Kullanıcıa*. Oturum açmada hesabın, bir sanal ağ eşlemesi oluşturmak için gerekli izinleri olmalıdır. İzinleri listesi için bkz. [sanal ağ eşleme izinleri](virtual-network-manage-peering.md#permissions).
2. Seçin **+ kaynak Oluştur**seçin **ağ**ve ardından **sanal ağ**.
3. Seçin veya aşağıdaki ayarları için aşağıdaki örneği değerleri girin ve ardından **Oluştur**:
    - **Adı**: *myVnetA*
    - **Adres alanı**: *10.0.0.0/16*
    - **Alt ağ adı**: *varsayılan*
    - **Alt ağ adres aralığı**: *10.0.0.0/24*
    - **Abonelik**: A aboneliği seçin
    - **Kaynak grubu**: Seçin **Yeni Oluştur** girin *myResourceGroupA*
    - **Konum**: *Doğu ABD*
4. İçinde **kaynak Ara** türü portalın üst kısmındaki kutusu *myVnetA*. Seçin **myVnetA** arama sonuçlarında görüntülendiğinde. 
5. Seçin **erişim denetimi (IAM)** seçenekleri sol taraftaki dikey listesinden.
6. Altında **myVnetA - erişim denetimi (IAM)** seçin **+ rol ataması Ekle**.
7. Seçin **ağ Katılımcısı** içinde **rol** kutusu.
8. İçinde **seçin** kutusunda, seçin *Kullanıcıb*, veya aramak için Kullanıcıb'in e-posta adresini yazın.
9. **Kaydet**’i seçin.
10. Altında **myVnetA - erişim denetimi (IAM)** seçin **özellikleri** seçenekleri sol taraftaki dikey listesinden. Kopyalama **kaynak kimliği**, bir sonraki adımda kullanılır. Kaynak Kimliği, aşağıdaki örneğe benzer: `/subscriptions/<Subscription Id>/resourceGroups/myResourceGroupA/providers/Microsoft.Network/virtualNetworks/myVnetA`.
11. Portalından UserA olarak oturum açın ve ardından UserB oturum açın.
12. 2-3 girerek veya adım 3'te aşağıdaki değerleri seçerek, adımları tamamlayın:

    - **Adı**: *myVnetB*
    - **Adres alanı**: *10.1.0.0/16*
    - **Alt ağ adı**: *varsayılan*
    - **Alt ağ adres aralığı**: *10.1.0.0/24*
    - **Abonelik**: B aboneliği seçin
    - **Kaynak grubu**: Seçin **Yeni Oluştur** girin *myResourceGroupB*
    - **Konum**: *Doğu ABD*

13. İçinde **kaynak Ara** türü portalın üst kısmındaki kutusu *myVnetB*. Seçin **myVnetB** arama sonuçlarında görüntülendiğinde.
14. Altında **myVnetB**seçin **özellikleri** seçenekleri sol taraftaki dikey listesinden. Kopyalama **kaynak kimliği**, bir sonraki adımda kullanılır. Kaynak Kimliği, aşağıdaki örneğe benzer: `/subscriptions/<Subscription ID>/resourceGroups/myResourceGroupB/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnetB`.
15. Seçin **erişim denetimi (IAM)** altında **myVnetB**ve ardından myVnetB, girmek için 5-10 adımları tamamlayın **Kullanıcıa** 8. adımda.
16. Portalda UserB olarak dışında oturum ve UserA oturum açın.
17. İçinde **kaynak Ara** türü portalın üst kısmındaki kutusu *myVnetA*. Seçin **myVnetA** arama sonuçlarında görüntülendiğinde.
18. Seçin **myVnetA**.
19. Altında **ayarları**seçin **eşlemeler**.
20. Altında **myVnetA - eşlemeler**seçin **+ Ekle**
21. Altında **Ekle eşlemesi**, girin veya seçin, aşağıdaki seçeneklerden sonra seçin **Tamam**:
     - **Adı**: *myVnetAToMyVnetB*
     - **Sanal ağ dağıtım modeli**:  **Resource Manager**’ı seçin.
     - **Kaynak Kimliğimi biliyorum**: Bu kutuyu işaretleyin.
     - **Kaynak Kimliği**: Adımda 14 kaynak Kimliğini girin.
     - **Sanal ağ erişimine izin ver:** Emin **etkin** seçilir.
    Diğer bir ayarları, bu öğreticide kullanılır. Tüm eşleme ayarları hakkında bilgi edinmek için [sanal ağ eşlemelerini yönetme](virtual-network-manage-peering.md#create-a-peering).
22. Oluşturduğunuz eşleme seçtikten sonra kısa bir beklemeden görünür **Tamam** önceki adımda. **Başlatılan** listelenir **eşleme durumu** sütunu için **myVnetAToMyVnetB** , eşleme oluşturulur. MyVnetA myVnetB için eşlenmiş, ancak artık myVnetB myVnetA için eş gerekir. Eşleme birbirleri ile iletişim kurmak için sanal ağlarda bulunan kaynaklar etkinleştirmek için her iki yönde oluşturulmalıdır.
23. Portalından UserA olarak oturum açın ve UserB oturum açın.
24. Yeniden myVnetB için 17-21 adımları tamamlayın. Adımda 21, eşleme adı *myVnetBToMyVnetA*seçin *myVnetA* için **sanal ağ**, adım 10'dan kimliği girin **kaynak kimliği**kutusu.
25. Seçtikten sonra birkaç saniye **Tamam** myVnetB için eşlemesi oluşturmak için **myVnetBToMyVnetA** oluşturdunuz eşlemesi ile listelendiğini **bağlı** içinde**Eşleme durumu** sütun.
26. Portalda UserB olarak dışında oturum ve UserA oturum açın.
27. 17-19. adımları tekrar tamamlayın. **Eşleme durumu** için **myVnetAToVNetB** eşleme özelliği artık **bağlı**. Gördükten sonra Eşleme başarıyla oluşturulmuş **bağlı** içinde **eşleme durumu** eşlemesindeki her iki sanal ağ için sütun. Her iki sanal ağ içinde oluşturduğunuz Azure kaynaklarını IP adresleri birbirleriyle iletişim kurabilir. Varsayılan Azure ad çözümlemesi için sanal ağlar kullanıyorsanız, kaynakların sanal ağlarda bulunan sanal ağlar arasında adlarını çözümlemek alamıyoruz. Bir eşleme de sanal ağlar arasında adlarını çözümlemek dilerseniz kendi DNS sunucunuzu oluşturmanız gerekir. Nasıl ayarlanacağını öğrenin [kendi DNS sunucunuzu kullanarak ad çözümlemesi](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server).
28. **İsteğe bağlı**: Bu öğreticide sanal makinelerin oluşturulmasını kapsamında olmayan da, her bir sanal ağ içinde bir sanal makine oluşturun ve bir sanal makineden diğerine, bağlantıyı doğrulamak için bağlanın.
29. **İsteğe bağlı**: Bu öğreticide oluşturduğunuz kaynakları silmek için'ndaki adımları tamamlayın. [Sil kaynakları](#delete-portal) bu makalenin.

## <a name="cli"></a>Oluşturma eşleme - Azure CLI

Bu öğreticide, farklı hesapları her abonelik için kullanılır. İki abonelik için izinleri olan bir hesap kullanıyorsanız, aynı hesabı kullanmak için tüm adımları, azure'dan günlüğe kaydetme adımları atlayın ve kullanıcı rol atamaları oluşturan komut satırlarını kaldırın. Değiştirin UserA@azure.com ve UserB@azure.com tüm Kullanıcıa ve kullanıcıb'in için kullanmakta olduğunuz kullanıcı adları ile aşağıdaki betikler. Sanal ağlar farklı Aboneliklerde olması ve aboneliklerin farklı Azure Active Directory kiracılarıyla ilişkili, devam etmeden önce aşağıdaki adımları tamamlayın:
 - Her Active Directory kiracısı kullanıcı ekleme bir [Konuk kullanıcı](../active-directory/b2b/add-users-administrator.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-guest-users-to-the-directory) karşı ve Azure Active Directory kiracısı içinde.
 - Diğer Azure Active Directory kiracısı Konuk kullanıcı daveti kabul etmek her kullanıcının vardır.

Aşağıdaki komut dosyaları:

- Azure CLI 2.0.4 sürüm gerektirir veya üzeri. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükseltmeniz gerekirse bkz [Azure CLI yükleme](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json).
- Bir Bash kabuğunda çalışır. Windows istemcisi üzerinde Azure CLI betiklerini çalıştırma seçenekleri için bkz. [Windows’da Azure CLI'yi yükleme](/cli/azure/install-azure-cli-windows).

CLI'yı ve bağımlılıklarını yüklemek yerine Azure Cloud Shell'i kullanabilirsiniz. Azure Cloud Shell doğrudan Azure portalının içinde çalıştırabileceğiniz ücretsiz bir Bash kabuğudur. Azure CLI, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Seçin **deneyin** , Azure hesabınızda oturum bir Cloud Shell çağıran aşağıdaki komut düğmesi.

1. CLI oturumu açın ve Azure'da kullanarak UserA olarak oturum açma `azure login` komutu. Oturum açmada hesabın, bir sanal ağ eşlemesi oluşturmak için gerekli izinleri olmalıdır. İzinleri listesi için bkz. [sanal ağ eşleme izinleri](virtual-network-manage-peering.md#permissions).
2. Aşağıdaki betiği bilgisayarınıza bir metin düzenleyicisine kopyalayın, yerine `<SubscriptionA-Id>` kimliği, SubscriptionA ile ardından değiştirilmiş betiği kopyalayın, CLI oturumu ve ENTER tuşuna yapıştırın `Enter`. Aboneliğinizi kimliği bilmiyorsanız, girin `az account show` komutu. Değeri **kimliği** çıktısında olduğundan, abonelik kimliği

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

3. Azure'dan kullanarak UserA olarak oturum `az logout` komutunu ve ardından Azure UserB olarak oturum açın. Oturum açmada hesabın, bir sanal ağ eşlemesi oluşturmak için gerekli izinleri olmalıdır. İzinleri listesi için bkz. [sanal ağ eşleme izinleri](virtual-network-manage-peering.md#permissions).
4. MyVnetB oluşturun. Betik içeriği 2. adımda bilgisayarınızdaki bir metin düzenleyicisine kopyalayın. Değiştirin `<SubscriptionA-Id>` SubscriptionB kimliği. Değişiklik 10.0.0.0/16 10.1.0.0/16, tüm B dair değişikliği ve değiştirilmiş betiği A. kopyalanacak tüm Bs yapıştırın, içinde, CLI oturumu ve ENTER tuşuna `Enter`.
5. Azure'dan UserB olarak oturum açın ve Azure UserA olarak oturum açın.
6. Bir sanal ağ için myVnetB myVnetA eşlemesi oluşturun. Aşağıdaki komut dosyası içeriğini bilgisayarınızdaki bir metin düzenleyicisine kopyalayın. Değiştirin `<SubscriptionB-Id>` SubscriptionB kimliği. Betiği çalıştırmak için değiştirilmiş betiği kopyalayın, CLI oturumunuza yapıştırın ve Enter tuşuna basın.

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

    Durumu **başlatılan**. Değişir **bağlı** eşdüzey hizmet sağlama için myVnetA myVnetB oluşturduktan sonra.

8. Kullanıcıa azure'dan dışarı oturum ve Azure UserB olarak oturum açın.
9. MyVnetB myVnetA için eşlemesi oluşturun. Betik içeriği 6. adımda bilgisayarınızdaki bir metin düzenleyicisine kopyalayın. Değiştirin `<SubscriptionB-Id>` SubscriptionA ve tüm B ve a için tüm Bs dair değişikliği kimliği Değişiklikleri yaptıktan sonra değiştirilmiş betiği kopyalayın, CLI oturumu ve ENTER tuşuna yapıştırın `Enter`.
10. MyVnetB eşleme durumunu görüntüleyin. Betik içeriği 7. adımda bilgisayarınızdaki bir metin düzenleyicisine kopyalayın. İçin kaynak grubunu ve sanal ağ adları, B değişiklik A betiği kopyalayın, CLI oturumunuz için de değiştirilmiş betiği yapıştırın ve tuşuna `Enter`. Eşleme durumu **bağlı**. Eşleme durumu myVnetA değişiklikleri **bağlı** myVnetB eşleme için myVnetA oluşturduktan sonra. Kullanıcıa oturumunuzu geri azure'a ve tam yeniden myVnetA eşleme durumunu doğrulamak için 7. adımda. 

    > [!NOTE]
    > Eşleme durumunu olana kadar eşleme yapılandırılmadı **bağlı** her iki sanal ağ için.

11. **İsteğe bağlı**: Bu öğreticide sanal makinelerin oluşturulmasını kapsamında olmayan da, her bir sanal ağ içinde bir sanal makine oluşturun ve bir sanal makineden diğerine, bağlantıyı doğrulamak için bağlanın.
12. **İsteğe bağlı**: Bu öğreticide oluşturduğunuz kaynakları silmek için'ndaki adımları tamamlayın. [Sil kaynakları](#delete-cli) bu makaledeki.

Her iki sanal ağ içinde oluşturduğunuz Azure kaynaklarını IP adresleri birbirleriyle iletişim kurabilir. Varsayılan Azure ad çözümlemesi için sanal ağlar kullanıyorsanız, kaynakların sanal ağlarda bulunan sanal ağlar arasında adlarını çözümlemek alamıyoruz. Bir eşleme de sanal ağlar arasında adlarını çözümlemek dilerseniz kendi DNS sunucunuzu oluşturmanız gerekir. Nasıl ayarlanacağını öğrenin [kendi DNS sunucunuzu kullanarak ad çözümlemesi](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server).

## <a name="powershell"></a>Oluşturma eşleme - PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu öğreticide, farklı hesapları her abonelik için kullanılır. İki abonelik için izinleri olan bir hesap kullanıyorsanız, aynı hesabı kullanmak için tüm adımları, azure'dan günlüğe kaydetme adımları atlayın ve kullanıcı rol atamaları oluşturan komut satırlarını kaldırın. Değiştirin UserA@azure.com ve UserB@azure.com tüm Kullanıcıa ve kullanıcıb'in için kullanmakta olduğunuz kullanıcı adları ile aşağıdaki betikler.
Sanal ağlar farklı Aboneliklerde olması ve aboneliklerin farklı Azure Active Directory kiracılarıyla ilişkili, devam etmeden önce aşağıdaki adımları tamamlayın:
 - Her Active Directory kiracısı kullanıcı ekleme bir [Konuk kullanıcı](../active-directory/b2b/add-users-administrator.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-guest-users-to-the-directory) karşı ve Azure Active Directory kiracısı içinde.
 - Ters Active Directory kiracısı Konuk kullanıcı daveti kabul etmek her kullanıcının vardır.

1. Azure PowerShell sürüm 1.0.0 sahip olduğunuzdan emin olun veya yüksek. Çalıştırarak bunu yapabilirsiniz `Get-Module -Name Az` PowerShell'in en son sürümünü yüklemenizi öneririz [Az modül](/powershell/azure/install-az-ps). Azure PowerShell'i kullanmaya yeni başladıysanız [Azure PowerShell'e genel bakış](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json) sayfasını inceleyin. 
2. Bir PowerShell oturumu başlatın.
3. PowerShell'de, Azure'da UserA girerek oturum `Connect-AzAccount` komutu. Oturum açmada hesabın, bir sanal ağ eşlemesi oluşturmak için gerekli izinleri olmalıdır. İzinleri listesi için bkz. [sanal ağ eşleme izinleri](virtual-network-manage-peering.md#permissions).
4. Bir kaynak grubunu ve sanal ağ a kopyalama bir metin düzenleyicisi için aşağıdaki betiği bilgisayarınıza oluşturun. Değiştirin `<SubscriptionA-Id>` SubscriptionA kimliği. Aboneliğinizi kimliği bilmiyorsanız, girin `Get-AzSubscription` görüntülemek için komutu. Değeri **kimliği** döndürülen çıktısında, abonelik kimliğidir. Betiği yürütmek için değiştirilmiş betiği kopyalayın, PowerShell yapıştırın ve ardından basın `Enter`.

    ```powershell
    # Create a resource group.
    New-AzResourceGroup `
      -Name MyResourceGroupA `
      -Location eastus

    # Create virtual network A.
    $vNetA = New-AzVirtualNetwork `
      -ResourceGroupName MyResourceGroupA `
      -Name 'myVnetA' `
      -AddressPrefix '10.0.0.0/16' `
      -Location eastus

    # Assign UserB permissions to myVnetA.
    New-AzRoleAssignment `
      -SignInName UserB@azure.com `
      -RoleDefinitionName "Network Contributor" `
      -Scope /subscriptions/<SubscriptionA-Id>/resourceGroups/myResourceGroupA/providers/Microsoft.Network/VirtualNetworks/myVnetA
    ```

5. Kullanıcıa azure'dan çıkış ve UserB oturum. Oturum açmada hesabın, bir sanal ağ eşlemesi oluşturmak için gerekli izinleri olmalıdır. İzinleri listesi için bkz. [sanal ağ eşleme izinleri](virtual-network-manage-peering.md#permissions).
6. Betik içeriği 4. adımda bilgisayarınızdaki bir metin düzenleyicisine kopyalayın. Değiştirin `<SubscriptionA-Id>` b değişiklik 10.0.0.0/16 10.1.0.0/16 için abonelik Kimliğine sahip. Tüm B ve tüm Bs seçeceğine A'ya değiştirme Betiği yürütmek için değiştirilmiş betiği kopyalayın, PowerShell içinde yapıştırın ve ardından basın `Enter`.
7. Azure'dan kullanıcıb'in kullanıma ve Kullanıcıa ' oturum.
8. MyVnetA myVnetB için eşlemesi oluşturun. Aşağıdaki komut dosyasını bilgisayarınızdaki bir metin düzenleyicisine kopyalayın. Değiştirin `<SubscriptionB-Id>` kimliğine sahip abonelik b Betiği yürütmek için değiştirilmiş betiği kopyalayın, PowerShell yapıştırın ve ardından basın `Enter`.

   ```powershell
   # Peer myVnetA to myVnetB.
   $vNetA=Get-AzVirtualNetwork -Name myVnetA -ResourceGroupName myResourceGroupA
   Add-AzVirtualNetworkPeering `
     -Name 'myVnetAToMyVnetB' `
     -VirtualNetwork $vNetA `
     -RemoteVirtualNetworkId "/subscriptions/<SubscriptionB-Id>/resourceGroups/myResourceGroupB/providers/Microsoft.Network/virtualNetworks/myVnetB"
   ```

9. MyVnetA eşleme durumunu görüntüleyin.

    ```powershell
    Get-AzVirtualNetworkPeering `
      -ResourceGroupName myResourceGroupA `
      -VirtualNetworkName myVnetA `
      | Format-Table VirtualNetworkName, PeeringState
    ```

    Durumu **başlatılan**. Değişir **bağlı** myVnetA için eşleme myVnetB ayarladıktan sonra.

10. Kullanıcıa azure'dan çıkış ve UserB oturum.
11. MyVnetB myVnetA için eşlemesi oluşturun. Betik içeriği 8. adımda bilgisayarınızdaki bir metin düzenleyicisine kopyalayın. Değiştirin `<SubscriptionB-Id>` abonelik A ve B ve a için tüm Bs dair tüm değişiklik kimliği Betiği yürütmek için değiştirilmiş betiği kopyalayın, PowerShell yapıştırın ve ardından basın `Enter`.
12. MyVnetB eşleme durumunu görüntüleyin. Betik içeriği 9. adımda bilgisayarınızdaki bir metin düzenleyicisine kopyalayın. Kaynak grubunu ve sanal ağ adları için B değişiklik A. Betiği çalıştırmak için PowerShell içinde değiştirilmiş betiği yapıştırın ve tuşuna `Enter`. Durumu **bağlı**. Eşleme durumu **myVnetA** değişikliklerini **bağlı** gelen eşleme oluşturduktan sonra **myVnetB** için **myVnetA**. Kullanıcıa oturumunuzu geri azure'a ve tam yeniden myVnetA eşleme durumunu doğrulamak için 9. adım.

    > [!NOTE]
    > Eşleme durumunu olana kadar eşleme yapılandırılmadı **bağlı** her iki sanal ağ için.

    Her iki sanal ağ içinde oluşturduğunuz Azure kaynaklarını IP adresleri birbirleriyle iletişim kurabilir. Varsayılan Azure ad çözümlemesi için sanal ağlar kullanıyorsanız, kaynakların sanal ağlarda bulunan sanal ağlar arasında adlarını çözümlemek alamıyoruz. Bir eşleme de sanal ağlar arasında adlarını çözümlemek dilerseniz kendi DNS sunucunuzu oluşturmanız gerekir. Nasıl ayarlanacağını öğrenin [kendi DNS sunucunuzu kullanarak ad çözümlemesi](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server).

13. **İsteğe bağlı**: Bu öğreticide sanal makinelerin oluşturulmasını kapsamında olmayan da, her bir sanal ağ içinde bir sanal makine oluşturun ve bir sanal makineden diğerine, bağlantıyı doğrulamak için bağlanın.
14. **İsteğe bağlı**: Bu öğreticide oluşturduğunuz kaynakları silmek için'ndaki adımları tamamlayın. [Sil kaynakları](#delete-powershell) bu makaledeki.

## <a name="template"></a>Eşleme - oluşturma Resource Manager şablonu

Sanal ağlar farklı Aboneliklerde olması ve aboneliklerin farklı Azure Active Directory kiracılarıyla ilişkili, devam etmeden önce aşağıdaki adımları tamamlayın:
 - Her Active Directory kiracısı kullanıcı ekleme bir [Konuk kullanıcı](../active-directory/b2b/add-users-administrator.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-guest-users-to-the-directory) karşı ve Azure Active Directory kiracısı içinde.
 - Ters Active Directory kiracısı Konuk kullanıcı daveti kabul etmek her kullanıcının vardır.

1. Sanal ağ oluşturma ve uygun atamak için [izinleri](virtual-network-manage-peering.md#permissions), adımları tamamlamanız [portalı](#portal), [Azure CLI](#cli), veya [PowerShell](#powershell)bu makalenin bölümler.
2. Yerel bilgisayarınızdaki bir dosyaya aşağıdaki metni kaydedin. Değiştirin `<subscription ID>` Kullanıcıa'nın abonelik kimliğinizle Örneğin dosya vnetpeeringA.json kaydedebilir.

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

3. Azure'a UserA olarak oturum açın ve şablon kullanarak dağıtma [portalı](../azure-resource-manager/resource-group-template-deploy-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#deploy-resources-from-custom-template), [PowerShell](../azure-resource-manager/resource-group-template-deploy.md?toc=%2fazure%2fvirtual-network%2ftoc.json#deploy-local-template), veya [Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json#deploy-local-template). Örnek json metin için 2. adımda kaydettiğiniz dosyanın adını belirtin.
4. Adım 2 ' örnek JSON dosyasını bilgisayarınızdaki bir dosyaya kopyalayın ve ile başlayan satırlar değişiklik:
   - **Ad**: Değişiklik *myVnetA/myVnetAToMyVnetB* için *myVnetB/myVnetBToMyVnetA*.
   - **Kimliği**: Değiştirin `<subscription ID>` Kullanıcıb'in abonelik kimliği ve değişiklik *myVnetB* için *myVnetA*.
5. 3. adım tam yeniden Azure'a UserB oturum açmış.
6. **İsteğe bağlı**: Bu öğreticide sanal makinelerin oluşturulmasını kapsamında olmayan da, her bir sanal ağ içinde bir sanal makine oluşturun ve bir sanal makineden diğerine, bağlantıyı doğrulamak için bağlanın.
7. **İsteğe bağlı**: Bu öğreticide oluşturduğunuz kaynakları silmek için'ndaki adımları tamamlayın. [Sil kaynakları](#delete) bölümü Azure portalı, PowerShell veya Azure CLI'yı kullanarak, bu makalenin.

## <a name="delete"></a>Kaynakları silme
Bu öğreticiyi tamamladığınızda, kullanım ücret ödememeniz öğreticide oluşturulan kaynakları silmek isteyebilirsiniz. Bir kaynak grubunun silinmesi kaynak grubundaki tüm kaynakları da siler.

### <a name="delete-portal"></a>Azure portal

1. Azure portalında UserA oturum açın.
2. Portal arama kutusuna **myResourceGroupA**. Arama sonuçlarında seçin **myResourceGroupA**.
3. **Sil**’i seçin.
4. İçinde silmeyi onaylamak için **kaynak grubu adını yazın** kutusuna **myResourceGroupA**ve ardından **Sil**.
5. Portalından UserA olarak oturum açın ve UserB oturum açın.
6. MyResourceGroupB için 2-4 arası adımları tamamlayın.

### <a name="delete-cli"></a>Azure CLI

1. Azure UserA olarak oturum açın ve aşağıdaki komutu yürütün:

   ```azurecli-interactive
   az group delete --name myResourceGroupA --yes
   ```

2. Azure'dan UserA olarak oturum açın ve UserB oturum açın.
3. Aşağıdaki komutu yürütün:

   ```azurecli-interactive
   az group delete --name myResourceGroupB --yes
   ```

### <a name="delete-powershell"></a>PowerShell

1. Azure UserA olarak oturum açın ve aşağıdaki komutu yürütün:

   ```powershell
   Remove-AzResourceGroup -Name myResourceGroupA -force
   ```

2. Azure'dan UserA olarak oturum açın ve UserB oturum açın.
3. Aşağıdaki komutu yürütün:

   ```powershell
   Remove-AzResourceGroup -Name myResourceGroupB -force
   ```

## <a name="next-steps"></a>Sonraki adımlar

- Kapsamlı ile önemli bilgilenmeli [sanal ağ eşleme kısıtlamaları ve davranışları](virtual-network-manage-peering.md#requirements-and-constraints) bir sanal ağ eşlemesi için üretim oluşturmadan önce kullanın.
- Hakkında bilgi edinin [sanal ağ eşleme ayarları](virtual-network-manage-peering.md#create-a-peering).
- Bilgi edinmek için nasıl [bir hub oluşturmak ve ağ topolojisi](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) sanal ağ eşlemesi ile.
