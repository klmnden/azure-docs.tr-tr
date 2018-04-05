---
title: Sanal Ağ eşlemesi ile - sanal ağlara bağlanabilir Azure portalı | Microsoft Docs
description: Bu makalede, eşliği, Azure Portalı'nı kullanarak sanal ağ ile sanal ağlara bağlanabilir öğrenin.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I want to connect two virtual networks so that virtual machines in one virtual network can communicate with virtual machines in the other virtual network.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/13/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: b864c71a62289b3abef13a98b52683f7d928b8e1
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="connect-virtual-networks-with-virtual-network-peering-using-the-azure-portal"></a>Azure Portalı'nı kullanarak sanal ağ eşlemesi ile sanal ağlara bağlanabilir

Sanal ağlar birbirlerine sanal ağ eşlemesi ile bağlayabilirsiniz. Sanal ağlar eşlendikten sonra iki sanal ağlarda bulunan kaynaklar kaynaklar aynı sanal ağda değilmiş gibi aynı gecikme süresi ve bant genişliği ile birbirleri ile iletişim kuramıyor. Bu makalede, bilgi nasıl yapılır:

> [!div class="checklist"]
> * İki sanal ağ oluşturma
> * Sanal Ağ eşlemesi iki sanal ağlara bağlanabilir
> * Her sanal ağ içinde bir sanal makine (VM) dağıtma
> * VM'ler arasında iletişim

Tercih ederseniz, bu makalede kullanarak tamamlayabilirsiniz [Azure CLI](tutorial-connect-virtual-networks-cli.md) veya [Azure PowerShell](tutorial-connect-virtual-networks-powershell.md).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="log-in-to-azure"></a>Azure'da oturum açma 

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="create-virtual-networks"></a>Sanal ağlar oluşturma

1. Seçin **+ kaynak oluşturma** üst üzerinde köşe Azure portalının sol.
2. Seçin **ağ**ve ardından **sanal ağ**.
3. Girin veya seçin, aşağıdaki bilgileri, diğer ayarlar için varsayılan değerleri kabul edin ve ardından **oluşturma**:

    |Ayar|Değer|
    |---|---|
    |Ad|myVirtualNetwork1|
    |Adres alanı|10.0.0.0/16|
    |Abonelik| Aboneliğinizi seçin.|
    |Kaynak grubu| Seçin **Yeni Oluştur** ve girin *myResourceGroup*.|
    |Konum| Seçin **Doğu ABD**.|
    |Alt Ağ Adı|Subnet1|
    |Alt ağ adres aralığı|10.0.0.0/24|

      ![Sanal ağ oluşturma](./media/tutorial-connect-virtual-networks-portal/create-virtual-network.png)

4. Adım 1-3 aşağıdaki değişikliklerle yeniden tamamlayın:

    |Ayar|Değer|
    |---|---|
    |Ad|myVirtualNetwork2|
    |Adres alanı|10.1.0.0/16|
    |Kaynak grubu| Seçin **var olanı kullan** ve ardından **myResourceGroup**.|
    |Alt ağ adres aralığı|10.1.0.0/24|

## <a name="peer-virtual-networks"></a>Eş sanal ağlar

1. Azure portalı üstündeki arama kutusuna yazmaya başlayın *MyVirtualNetwork1*. Zaman **myVirtualNetwork1** arama sonuçlarında görünür.
2. Seçin **eşlemeler**altında **ayarları**ve ardından **+ Ekle**, aşağıdaki resimde gösterildiği gibi:

    ![Eşlemesi oluşturma](./media/tutorial-connect-virtual-networks-portal/create-peering.png)

3. Girin veya seçin, aşağıdaki bilgileri, diğer ayarlar için varsayılan değerleri kabul edin ve ardından **Tamam**.

    |Ayar|Değer|
    |---|---|
    |Ad|myVirtualNetwork1-myVirtualNetwork2|
    |Abonelik| Aboneliğinizi seçin.|
    |Sanal ağ|Seçilecek myVirtualNetwork2 - *myVirtualNetwork2* sanal ağ, select **sanal ağ**seçeneğini belirleyip **myVirtualNetwork2**.|

    ![Eşleme ayarları](./media/tutorial-connect-virtual-networks-portal/peering-settings.png)

    **EŞLİĞİ durumu** olan *başlatılan*, aşağıdaki resimde gösterildiği gibi:

    ![Eşleme durumu](./media/tutorial-connect-virtual-networks-portal/peering-status.png)

    Durum görmüyorsanız, tarayıcınızı yenileyin.

4. İçinde **arama** kutusunda Azure portalının en üstünde, yazmaya başlayın *MyVirtualNetwork2*. Zaman **myVirtualNetwork2** arama sonuçlarında görünür.
5. 2-3 adımları yeniden aşağıdaki değişiklikleri tamamlayın ve ardından **Tamam**:

    |Ayar|Değer|
    |---|---|
    |Ad|myVirtualNetwork2-myVirtualNetwork1|
    |Sanal ağ|myVirtualNetwork1|

    **EŞLİĞİ durumu** olan *bağlı*. Azure eşleme durumunu da değişir *myVirtualNetwork2 myVirtualNetwork1* gelen eşleme *başlatılan* için *bağlandı.* Sanal Ağ eşlemesi değil tam olarak kurulduğunda her iki sanal ağ eşleme durumu kadar *bağlandı.* 

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Sonraki adımda aralarında iletişim kurabilmesi için bir VM her sanal ağ oluşturun.

### <a name="create-the-first-vm"></a>İlk VM oluşturma

1. Seçin **+ kaynak oluşturma** üst üzerinde köşe Azure portalının sol.
2. **İşlem**'i seçin ve sonra da **Windows Server 2016 Datacenter**'ı seçin. Farklı bir işletim sistemi seçebilirsiniz, ancak geri kalan adımları seçtiğiniz varsayın **Windows Server 2016 Datacenter**. 
3. Girin veya seçin, için aşağıdaki bilgileri **Temelleri**diğer ayarlar için varsayılan değerleri kabul edin ve ardından **oluşturma**:

    |Ayar|Değer|
    |---|---|
    |Ad|myVm1|
    |Kullanıcı adı| Seçtiğiniz kullanıcı adı girin.|
    |Parola| Seçtiğiniz bir parola girin. Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.|
    |Kaynak grubu| Seçin **var olanı kullan** ve ardından **myResourceGroup**.|
    |Konum| Seçin **Doğu ABD**.|
4. VM boyutu altında seçin **bir boyutu seçin**.
5. İçin aşağıdaki değerleri seçin **ayarları**seçeneğini belirleyip **Tamam**:
    |Ayar|Değer|
    |---|---|
    |Sanal ağ| myVirtualNetwork1 - henüz seçili değilse seçin **sanal ağ** ve ardından **myVirtualNetwork1** altında **Seç sanal ağ**.|
    |Alt ağ| Subnet1 - henüz seçili değilse seçin **alt** ve ardından **Subnet1** altında **alt ağ seçin**.|
    
    ![Sanal makine ayarları](./media/tutorial-connect-virtual-networks-portal/virtual-machine-settings.png)
 
6. Altında **oluşturma** içinde **Özet**seçin **oluşturma** VM dağıtımı başlatmak için.

### <a name="create-the-second-vm"></a>İkinci VM oluşturma

Adım 1-6 aşağıdaki değişikliklerle yeniden tamamlayın:

|Ayar|Değer|
|---|---|
|Ad | myVm2|
|Sanal ağ | myVirtualNetwork2|

Sanal makineleri oluşturmak için birkaç dakika sürebilir. Her iki VM oluşturulana kadar kalan adımlara devam etmeyin.

## <a name="communicate-between-vms"></a>VM'ler arasında iletişim

1. İçinde *arama* kutusunda portalın en üstünde, yazmaya başlayın *myVm1*. Zaman **myVm1** arama sonuçlarında görünür.
2. Uzak Masaüstü bağlantı oluşturmak *myVm1* seçerek VM **Bağlan**, aşağıdaki resimde gösterildiği gibi:

    ![Sanal makineye bağlanma](./media/tutorial-connect-virtual-networks-portal/connect-to-virtual-machine.png)  

3. VM'e bağlanmak için indirilen RDP dosyasını açın. İstenirse, seçin **Bağlan**.
4. Kullanıcı adını ve VM oluştururken belirttiğiniz parolayı girin (seçmek için gerek duyabileceğiniz **daha fazla seçenek**, ardından **farklı bir hesap kullan**, VM oluşturduğunuz sırada girdiğiniz kimlik bilgileri belirtmek için), ardından **Tamam**.
5. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Seçin **Evet** bağlantı ile devam etmek için.
6. Bir sonraki adımda ping ile iletişim kurmak için kullanılan *myVm2* VM'den *myVm1* VM. Varsayılan olarak Windows Güvenlik Duvarı üzerinden engellenir Internet Denetim İletisi Protokolü (ICMP) ping komutunu kullanır. Üzerinde *myVm1* VM, böylece bu VM ping ICMP Windows Güvenlik Duvarı üzerinden etkinleştirme *myVm2* PowerShell kullanarak bir sonraki adımda:

    ```powershell
    New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
    ```
    
    Bu makalede yer alan VM'ler arasında iletişim kurmak için ping kullanılsa da ICMP üretim dağıtımları için Windows Güvenlik Duvarı aracılığıyla izin vererek önerilmez.

7. Bağlanmak için *myVm2* VM, bir komut isteminden aşağıdaki komutu girin *myVm1* VM:

    ```
    mstsc /v:10.1.0.4
    ```
    
8. Ping etkinleştirilmiş olduğundan *myVm1*, bu IP adresine göre şimdi ping:

    ```
    ping 10.0.0.4
    ```
    
9. Hem sizin RDP oturumların bağlantısını kesmek *myVm1* ve *myVm2*.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olduğunda, kaynak grubu ve içerdiği tüm kaynaklar Sil: 

1. Girin *myResourceGroup* içinde **arama** portalı üstündeki kutusu. Gördüğünüzde **myResourceGroup** arama sonuçlarında seçin.
2. **Kaynak grubunu sil**'i seçin.
3. Girin *myResourceGroup* için **türü kaynak grubu adı:** seçip **silmek**.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, sanal ağ eşlemesi ile iki ağ aynı Azure bölgesinde bağlanma öğrendiniz. Ayrıca farklı sanal ağlar eş [bölgeler desteklenen](virtual-network-manage-peering.md#cross-region) ve [farklı Azure abonelikleri](create-peering-different-subscriptions.md#portal), yanı sıra oluşturma [hub ve bağlı bileşen ağ tasarımları](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) ile eşleme. Sanal Ağ eşlemesi hakkında daha fazla bilgi için bkz: [sanal ağ eşleme genel bakış](virtual-network-peering-overview.md) ve [sanal ağ eşlemeleri yönetme](virtual-network-manage-peering.md).

Kendi bilgisayarınızı VPN üzerinden sanal bir ağa bağlayın ve sanal ağ veya eşlenmiş sanal ağlar kaynakları ile etkileşim için bkz: [bilgisayarınızı bir sanal ağa bağlamak](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
