---
title: Birden çok alt ağa sahip bir Azure sanal ağı (Klasik) oluşturmak | Microsoft Docs
description: Azure'da birden çok alt ağ ile sanal ağ (Klasik) oluşturmayı öğrenin.
services: virtual-network
documentationcenter: ''
author: genlin
manager: cshepard
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/31/2018
ms.author: genli
ms.custom: ''
ms.openlocfilehash: e40648ef47b108050486d43eefdb1564786c053e
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67202867"
---
# <a name="create-a-virtual-network-classic-with-multiple-subnets"></a>Birden çok alt ağa sahip bir sanal ağ (Klasik) oluşturmak

> [!IMPORTANT]
> Azure iki sahip [farklı dağıtım modelleri](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) oluşturmak ve bu kaynaklarla çalışmak için: Resource Manager ve klasik. Bu makale klasik dağıtım modelini incelemektedir. Microsoft öneriyor üzerinden en yeni sanal ağlar oluşturma [Resource Manager](quick-create-portal.md) dağıtım modeli.

Bu öğreticide, genel ve özel ayrı alt ağlara sahip bir temel Azure sanal ağ (Klasik) oluşturmayı öğrenin. Sanal makineler ve bir alt ağdaki bulut Hizmetleri gibi Azure kaynaklarını oluşturabilirsiniz. Sanal ağlar (Klasik) oluşturulan kaynakları birbirleriyle ve bir sanal ağa bağlı başka ağlardaki kaynaklarla iletişim kurabilir.

Tüm hakkında daha fazla bilgi [sanal ağ](manage-virtual-network.md) ve [alt](virtual-network-manage-subnet.md) ayarları.

> [!WARNING]
> Sanal ağlar (Klasik), Azure tarafından hemen silinir olduğunda bir [abonelik devre dışı](../billing/billing-subscription-become-disable.md?toc=%2fazure%2fvirtual-network%2ftoc.json#you-reached-your-spending-limit). Sanal ağlar (Klasik) kaynakları sanal ağ içinde mevcut bağımsız olarak silinir. Daha sonra aboneliği yeniden etkinleştirirseniz, sanal ağda var olan kaynakları yeniden oluşturulmalıdır.

Bir sanal ağ (Klasik) kullanarak oluşturabileceğiniz [Azure portalında](#portal), [Azure komut satırı arabirimi (CLI) 1.0](#azure-cli), veya [PowerShell](#powershell).

## <a name="portal"></a>Portal

1. Bir Internet tarayıcısında Git [Azure portalında](https://portal.azure.com). Kullanarak oturum açın, [Azure hesabı](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Azure hesabınız yoksa, oturum açabileceğiniz bir [ücretsiz deneme sürümü](https://azure.microsoft.com/offers/ms-azr-0044p).
2. Tıklayın **kaynak Oluştur** portalında.
3. Girin *sanal ağ* içinde **markette Ara** kutusunun üst kısmındaki **yeni** bölmesi görünür. Tıklayın **sanal ağ** arama sonuçlarında görüntülendiğinde.
4. Seçin **Klasik** içinde **dağıtım modeli seçin** kutusunda **sanal ağ** görünür, ardından bölmesinde **Oluştur**. 
5. Aşağıdaki değerleri girin **sanal ağ oluştur (Klasik)** bölmesi ve ardından **Oluştur**:

    |Ayar|Değer|
    |---|---|
    |Ad|myVnet|
    |Adres alanı|10.0.0.0/16|
    |Alt ağ adı|Genel|
    |Alt ağ adres aralığı|10.0.0.0/24|
    |Kaynak grubu|Bırakın **Yeni Oluştur** seçili ve enter **myResourceGroup**.|
    |Abonelik ve konum|Abonelik ve konum seçin.

    Azure'da yeniyseniz, daha fazla bilgi edinin [kaynak grupları](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group), [abonelikleri](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription), ve [konumları](https://azure.microsoft.com/regions) (olarak da adlandırılan *bölgeleri*).
4. Portalda, sanal ağ oluştururken yalnızca bir alt ağ oluşturabilirsiniz. Bu öğreticide, sanal ağ oluşturduktan sonra ikinci bir alt ağ oluşturun. İnternet'ten erişilebilen kaynaklara daha sonra oluşturacağınız **genel** alt ağ. Internet'ten erişilemez kaynakları de oluşturabilir **özel** alt ağ. İkinci ağı oluşturmak için girin **myVnet** içinde **kaynak Ara** sayfanın üstündeki kutusu. Tıklayın **myVnet** arama sonuçlarında görüntülendiğinde.
5. Tıklayın **alt ağlar** (içinde **ayarları** bölümü) üzerinde **sanal ağ oluştur (Klasik)** bölmesi görünür.
6. Tıklayın **+ Ekle** üzerinde **myVnet - alt ağlar** bölmesi görünür.
7. Girin **özel** için **adı** üzerinde **alt ağ Ekle** bölmesi. Girin **10.0.1.0/24** için **adres aralığı**.  **Tamam**'ı tıklatın.
8. Üzerinde **myVnet - alt ağlar** bölmesinde görebilirsiniz **genel** ve **özel** oluşturduğunuz alt ağlar.
9. **İsteğe bağlı**: Bu öğreticiyi tamamladığınızda, kullanım ücretleri oluşmaması, oluşturduğunuz kaynakları silmek isteyebilirsiniz:
    - Tıklayın **genel bakış** üzerinde **myVnet** bölmesi.
    - Tıklayın **Sil** simgesine **myVnet** bölmesi.
    - Silme işlemini onaylamak için tıklayın **Evet** içinde **silme sanal ağ** kutusu.

## <a name="azure-cli"></a>Azure CLI

1. Yapabilirsiniz veya [Azure CLI'yi yükleme ve yapılandırma](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json), veya Azure Cloud Shell içinde CLI'yı kullanın. Azure Cloud Shell doğrudan Azure portalının içinde çalıştırabileceğiniz ücretsiz bir Bash kabuğudur. Azure CLI, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. CLI komutlarında Yardım almak için şunu yazın `azure <command> --help`. 
2. Bir CLI oturumu Azure'da aşağıdaki komut ile oturum açın. Tıklarsanız **deneyin** kutusunda Cloud Shell açılır. Azure aboneliğiniz için aşağıdaki komutu girerek olmadan oturum:

    ```azurecli-interactive
    azure login
    ```

3. CLI Hizmet Yönetimi modunda olduğundan emin olmak için aşağıdaki komutu girin:

    ```azurecli-interactive
    azure config mode asm
    ```

4. Özel bir alt ağ ile sanal ağ oluşturun:

    ```azurecli-interactive
    azure network vnet create --vnet myVnet --address-space 10.0.0.0 --cidr 16  --subnet-name Private --subnet-start-ip 10.0.0.0 --subnet-cidr 24 --location "East US"
    ```

5. Sanal ağ içinde ortak bir alt ağ oluşturun:

    ```azurecli-interactive
    azure network vnet subnet create --name Public --vnet-name myVnet --address-prefix 10.0.1.0/24
    ```    
    
6. Sanal ağ ve alt ağları gözden geçirin:

    ```azurecli-interactive
    azure network vnet show --vnet myVnet
    ```

7. **İsteğe bağlı**: Bu öğreticiyi tamamladığınızda kullanım ücretleri oluşmaması oluşturduğunuz kaynakları silmek isteyebilirsiniz:

    ```azurecli-interactive
    azure network vnet delete --vnet myVnet --quiet
    ```

> [!NOTE]
> CLI kullanarak bir sanal ağ (Klasik) oluşturmak için bir kaynak grubu belirtemezsiniz rağmen Azure sanal ağı adlı bir kaynak grubu oluşturur. *varsayılan ağ*.

## <a name="powershell"></a>PowerShell

1. En son PowerShell sürümünü [Azure](https://www.powershellgallery.com/packages/Azure) modülü. Azure PowerShell'i kullanmaya yeni başladıysanız [Azure PowerShell'e genel bakış](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json) sayfasını inceleyin.
2. Bir PowerShell oturumu başlatın.
3. PowerShell'de `Add-AzureAccount` komutunu girerek Azure oturumu açın.
4. Aşağıdaki yol ve dosya adı, uygun şekilde değiştirin, sonra mevcut ağ yapılandırma dosyasını dışarı aktar:

    ```powershell
    Get-AzureVNetConfig -ExportToFile c:\azure\NetworkConfig.xml
    ```

5. Genel ve özel alt ağa sahip bir sanal ağ oluşturmak için eklemek için herhangi bir metin düzenleyicisi kullanın **VirtualNetworkSite** ağ yapılandırma dosyasında aşağıdaki öğesi.

    ```xml
    <VirtualNetworkSite name="myVnet" Location="East US">
        <AddressSpace>
          <AddressPrefix>10.0.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Private">
            <AddressPrefix>10.0.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Public">
            <AddressPrefix>10.0.1.0/24</AddressPrefix>
          </Subnet>
        </Subnets>
      </VirtualNetworkSite>
    ```

    Tam gözden [ağ yapılandırma dosyası şeması](https://msdn.microsoft.com/library/azure/jj157100.aspx).

6. Ağ yapılandırma dosyasını içeri aktarın:

    ```powershell
    Set-AzureVNetConfig -ConfigurationPath c:\azure\NetworkConfig.xml
    ```

    > [!WARNING]
    > Değiştirilen ağ yapılandırma dosyasını içeri aktarma, var olan sanal ağları aboneliğinizde (Klasik) değişiklikleri neden olabilir. Yalnızca önceki sanal ağ ekleme ve değiştirme veya yoksa var olan tüm sanal ağları aboneliğinizden kaldırın, emin olun. 

7. Sanal ağ ve alt ağları gözden geçirin:

    ```powershell
    Get-AzureVNetSite -VNetName "myVnet"
    ```

8. **İsteğe bağlı**: Bu öğreticiyi tamamladığınızda kullanım ücretleri oluşmaması oluşturduğunuz kaynakları silmek isteyebilirsiniz. Sanal ağı silmek için 4-6 yeniden, bu zaman kaldırma adımları tamamlayın **VirtualNetworkSite** 5. adımda eklediğiniz öğesi.
 
> [!NOTE]
> PowerShell kullanarak bir sanal ağ (Klasik) oluşturmak için bir kaynak grubu belirtemezsiniz rağmen Azure sanal ağı adlı bir kaynak grubu oluşturur. *varsayılan ağ*.

---

## <a name="next-steps"></a>Sonraki adımlar

- Tüm sanal ağ ve alt ağ ayarları hakkında bilgi edinmek için [sanal ağlarını yönetme](manage-virtual-network.md) ve [sanal ağ alt ağları yönetme](virtual-network-manage-subnet.md). Sanal ağlar ve alt ağlar, bir üretim ortamında farklı gereksinimlerini karşılayacak şekilde kullanarak çeşitli seçeneğiniz vardır.
- Oluşturma bir [Windows](../virtual-machines/windows/classic/createportal-classic.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [Linux](../virtual-machines/linux/classic/createportal-classic.md?toc=%2fazure%2fvirtual-network%2ftoc.json) sanal makine ve mevcut bir sanal ağa bağlayın.
- İki sanal ağ aynı Azure konumunda bağlanmak için oluşturma bir [sanal ağ eşlemesi](create-peering-different-deployment-models.md) sanal ağlar arasında. Bir sanal ağ (Resource Manager) (Klasik) bir sanal ağa eşlenebilir, ancak eşlemesi iki sanal ağ arasında (Klasik) oluşturulamıyor.
- Sanal ağ kullanarak bir şirket içi ağa bağlanma bir [VPN ağ geçidi](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [Azure ExpressRoute](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md?toc=%2fazure%2fvirtual-network%2ftoc.json) bağlantı hattı.
