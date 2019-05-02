---
title: Özel IP adresleri Vm'leri - Azure portalı için yapılandırma | Microsoft Docs
description: Azure portalını kullanarak sanal makineleri için özel IP adresleri yapılandırmayı öğrenin.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
editor: tysonn
tags: azure-resource-manager
ms.assetid: 11245645-357d-4358-9a14-dd78e367b494
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/04/2016
ms.author: kumud
ms.openlocfilehash: 31aeab946b9ad740e2f56eb1ecaafd3e76cc42b3
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64723797"
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-using-the-azure-portal"></a>Azure portalını kullanarak bir sanal makine için özel IP adreslerini yapılandırın

> [!div class="op_single_selector"]
> * [Azure portal](virtual-networks-static-private-ip-arm-pportal.md)
> * [PowerShell](virtual-networks-static-private-ip-arm-ps.md)
> * [Azure CLI](virtual-networks-static-private-ip-arm-cli.md)
> * [Azure portalını (Klasik)](virtual-networks-static-private-ip-classic-pportal.md)
> * [PowerShell (Klasik)](virtual-networks-static-private-ip-classic-ps.md)
> * [Azure CLI (Klasik)](virtual-networks-static-private-ip-classic-cli.md)


[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Bu makalede Resource Manager dağıtım modeli anlatılmaktadır. Ayrıca [Klasik dağıtım modelinde statik özel IP adresi yönetme](virtual-networks-static-private-ip-classic-pportal.md).

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Aşağıdaki örnek adımlarda, önceden oluşturulmuş basit bir ortam bekler. Açıklanan test ortamı adımları bu belgede gösterildiği çalıştırmak istiyorsanız, öncelikle yapı [sanal ağ oluşturma](quick-create-portal.md).

## <a name="how-to-create-a-vm-for-testing-static-private-ip-addresses"></a>Statik özel IP adresleri test etmek için bir VM oluşturma
Statik özel IP adresi, Azure portalını kullanarak Resource Manager dağıtım modunda bir VM oluşturma sırasında ayarlanamıyor. İlk VM oluşturma ardından kendi özel IP statik olarak ayarlayın.

Adlı bir VM oluşturmak için *DNS01* içinde *ön uç* adlı bir sanal ağ alt ağı *TestVNet*, şu adımları izleyin:

1. Tarayıcıdan https://portal.azure.com adresine gidin ve gerekiyorsa Azure hesabınızla oturum açın.
2. Tıklayın **kaynak Oluştur** > **işlem** > **Windows Server 2012 R2 Datacenter**, dikkat **bir dağıtım seçin Model** listesinde zaten gösterir **Resource Manager**ve ardından **Oluştur**, aşağıdaki resimde görüldüğü gibi.
   
    ![Azure portalını kullanarak VM oluşturma](./media/virtual-networks-static-ip-arm-pportal/figure01.png)
3. İçinde **Temelleri** bölmesinde, VM oluşturmak için adını girin (*DNS01* senaryoda), yerel yönetici hesabı ve parola, aşağıdaki resimde görüldüğü gibi.
   
    ![Temel bilgileri bölmesi](./media/virtual-networks-static-ip-arm-pportal/figure02.png)
4. Emin olun **konumu** seçilmişse *Orta ABD*, ardından **var olanı Seç** altında **kaynak grubu**, ardından**Kaynak grubu** yeniden ardından *TestRG*ve ardından **Tamam**.
   
    ![Temel bilgileri bölmesi](./media/virtual-networks-static-ip-arm-pportal/figure03.png)
5. İçinde **bir boyut seçin** bölmesinde **standart A1**ve ardından **seçin**.
   
    ![Bir boyut bölmesinde seçin](./media/virtual-networks-static-ip-arm-pportal/figure04.png)    
6. İçinde **ayarları** bölmesinde, özelliklerini aşağıdaki değerleri ayarlayın ve ardından mutlaka **Tamam**.
   
    -**Depolama hesabı**: *vnetstorage*
   
   * **Ağ**: *TestVNet*
   * **Alt ağ**: *Ön uç*
     
     ![Bir boyut bölmesinde seçin](./media/virtual-networks-static-ip-arm-pportal/figure05.png)     
7. İçinde **özeti** bölmesinde tıklayın **Tamam**. Panonuzda görüntülenen aşağıdaki kutucuğa dikkat edin.
   
    ![Azure portalını kullanarak VM oluşturma](./media/virtual-networks-static-ip-arm-pportal/figure06.png)

Önerilen, statik olarak bir sanal makinenin işletim sistemi içinde Azure sanal makinesine atanmış özel IP sürece atamayın, gerekli olduğunda gibi [birden çok IP adresleri atama bir Windows VM'ye](virtual-network-multiple-ip-addresses-portal.md). İşletim sistemi içinde özel IP adresini el ile ayarlamanız durumunda Azure atanmış özel IP adresiyle aynı adresi olduğundan emin olun [ağ arabirimi](virtual-network-network-interface-addresses.md#change-ip-address-settings), ya da sanal makineye bağlantı kaybedebilir. Daha fazla bilgi edinin [özel IP adresi](virtual-network-network-interface-addresses.md#private) ayarları. Hiçbir zaman el ile bir Azure sanal makinesi sanal makinenin işletim sistemi içinde atanan genel IP adresi atamanız gerekir.

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Bir sanal makine için statik özel IP adresi bilgilerini alma
Yukarıdaki adımları ile oluşturulan sanal makine için statik özel IP adresi bilgilerini görüntülemek için aşağıdaki adımları uygulayın.

1. Azure portalından tıklayın **TÜMÜNE Gözat** > **sanal makineler** > **DNS01** > **tümayarlar**  >  **Ağ arabirimleri** ve listelenen yalnızca ağ arabiriminden'ye tıklayın.
   
    ![VM kutucuğu dağıtma](./media/virtual-networks-static-ip-arm-pportal/figure07.png)
2. İçinde **ağ arabirimi** bölmesinde tıklayın **tüm ayarlar** > **IP adresleri** ve fark **atama** ve  **IP adresi** değerleri.
   
    ![VM kutucuğu dağıtma](./media/virtual-networks-static-ip-arm-pportal/figure08.png)

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Mevcut bir VM'ye statik özel IP adresi ekleme
Yukarıdaki adımları kullanarak oluşturulan VM'ye statik özel IP adresi eklemek için aşağıdaki adımları izleyin:

1. Gelen **IP adresleri** , yukarıda gösterilen bölmesi **statik** altında **atama**.
2. Tür *192.168.1.101* için **IP adresi**ve ardından **Kaydet**.
   
    ![Azure portalını kullanarak VM oluşturma](./media/virtual-networks-static-ip-arm-pportal/figure09.png)

> [!NOTE]
> Tıkladıktan sonra eğer **Kaydet**, ataması hala ayarlanır fark **dinamik**, girdiğiniz IP adresi zaten kullanılıyor anlamına gelir. Farklı bir IP adresi deneyin.
> 
> 

Önerilen, statik olarak bir sanal makinenin işletim sistemi içinde Azure sanal makinesine atanmış özel IP sürece atamayın, gerekli olduğunda gibi [birden çok IP adresleri atama bir Windows VM'ye](virtual-network-multiple-ip-addresses-portal.md). İşletim sistemi içinde özel IP adresini el ile ayarlamanız durumunda Azure atanmış özel IP adresiyle aynı adresi olduğundan emin olun [ağ arabirimi](virtual-network-network-interface-addresses.md#change-ip-address-settings), ya da sanal makineye bağlantı kaybedebilir. Daha fazla bilgi edinin [özel IP adresi](virtual-network-network-interface-addresses.md#private) ayarları. Hiçbir zaman el ile bir Azure sanal makinesi sanal makinenin işletim sistemi içinde atanan genel IP adresi atamanız gerekir.

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>VM'den özel statik IP adresi kaldırma
Yukarıda oluşturulan sanal makine statik özel IP adresini kaldırmak için aşağıdaki adımı tamamlayın:

Gelen **IP adresleri** , yukarıda gösterilen bölmesi **dinamik** altında **atama**ve ardından **Kaydet**.

## <a name="set-ip-addresses-within-the-operating-system"></a>İşletim sistemi içinde IP adreslerini ayarlayın

Önerilen, statik olarak bir sanal makinenin işletim sistemi içinde Azure sanal makinesine atanmış özel IP sürece atamayın, gerekli olduğunda gibi [birden çok IP adresleri atama bir Windows VM'ye](virtual-network-multiple-ip-addresses-portal.md). İşletim sistemi içinde özel IP adresini el ile ayarlamanız durumunda Azure atanmış özel IP adresiyle aynı adresi olduğundan emin olun [ağ arabirimi](virtual-network-network-interface-addresses.md#change-ip-address-settings), ya da sanal makineye bağlantı kaybedebilir. Daha fazla bilgi edinin [özel IP adresi](virtual-network-network-interface-addresses.md#private) ayarları. Hiçbir zaman el ile bir Azure sanal makinesi sanal makinenin işletim sistemi içinde atanan genel IP adresi atamanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Yönetme hakkında daha fazla bilgi [IP adresi ayarları](virtual-network-network-interface-addresses.md).

