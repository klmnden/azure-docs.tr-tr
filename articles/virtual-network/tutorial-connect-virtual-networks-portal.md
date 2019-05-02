---
title: Sanal ağ eşlemesi ile sanal ağları bağlama - öğretici - Azure portalı | Microsoft Docs
description: Bu öğreticide, Azure portalını kullanarak sanal ağ eşlemesi ile sanal ağların nasıl bağlanacağını öğreneceksiniz.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: twooley
editor: ''
tags: azure-resource-manager
Customer intent: I want to connect two virtual networks so that virtual machines in one virtual network can communicate with virtual machines in the other virtual network.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 08/16/2018
ms.author: kumud
ms.custom: ''
ms.openlocfilehash: 943cad871330e2f3b6e13b33dca582ab545fe4be
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64726575"
---
# <a name="tutorial-connect-virtual-networks-with-virtual-network-peering-using-the-azure-portal"></a>Öğretici: Azure portalını kullanarak sanal ağ eşlemesi ile sanal ağları birbirine bağlama

Sanal ağ eşlemesi ile sanal ağları birbirine bağlayabilirsiniz. Bu sanal ağlar aynı bölgede veya farklı bölgelerde (Genel Sanal Ağ Eşleme olarak da bilinir) olabilir. Sanal ağlar eşlendikten sonra, kaynaklar aynı sanal ağ üzerindeymiş gibi, aynı gecikme süresi ve bant genişliği ile her iki sanal ağdaki kaynaklar birbiriyle iletişim kurabilir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * İki sanal ağ oluşturma
> * Sanal ağ eşlemesi iki sanal ağı bağlama
> * Her sanal ağa sanal makine (VM) dağıtma
> * Sanal makineler arasında iletişim

Tercih ederseniz, [Azure CLI](tutorial-connect-virtual-networks-cli.md) veya [Azure PowerShell](tutorial-connect-virtual-networks-powershell.md) kullanarak bu öğreticiyi tamamlayabilirsiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="log-in-to-azure"></a>Azure'da oturum açma 

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="create-virtual-networks"></a>Sanal ağlar oluşturma

1. Azure portalının sol üst köşesinde bulunan **+ Kaynak oluştur** seçeneğini belirleyin.
2. **Ağ**’ı ve sonra **Sanal ağ**’ı seçin.
3. Aşağıdaki bilgileri girin veya seçin, kalan ayarlar için varsayılan değerleri kabul edin ve sonra **Oluştur**’u seçin:

    |Ayar|Değer|
    |---|---|
    |Ad|myVirtualNetwork1|
    |Adres alanı|10.0.0.0/16|
    |Abonelik| Aboneliğinizi seçin.|
    |Kaynak grubu| **Yeni oluştur**’u seçin ve *myResourceGroup* değerini girin.|
    |Location| **Doğu ABD**’yi seçin.|
    |Alt Ağ Adı|Subnet1|
    |Alt Ağ Adresi aralığı|10.0.0.0/24|

      ![Sanal ağ oluşturma](./media/tutorial-connect-virtual-networks-portal/create-virtual-network.png)

4. Aşağıdaki değişikliklerle birlikte 1.-3. adımları tekrar tamamlayın:

    |Ayar|Değer|
    |---|---|
    |Ad|myVirtualNetwork2|
    |Adres alanı|10.1.0.0/16|
    |Kaynak grubu| **Mevcut olanı kullan**’ı seçin ve **myResourceGroup** seçeneğini belirleyin.|
    |Alt Ağ Adresi aralığı|10.1.0.0/24|

## <a name="peer-virtual-networks"></a>Sanal ağları eşleme

1. Azure portalının üst kısmındaki Arama kutusuna *MyVirtualNetwork1* yazmaya başlayın. Arama sonuçlarında **myVirtualNetwork1** görüntülendiğinde bunu seçin.
2. **AYARLAR** bölümünden **Eşdüzey hizmet sağlamaları**’nı seçin ve sonra aşağıdaki resimde gösterildiği gibi **+ Ekle** seçeneğini belirleyin:

    ![Eşleme oluşturma](./media/tutorial-connect-virtual-networks-portal/create-peering.png)

3. Aşağıdaki bilgileri girin veya seçin, kalan ayarlar için varsayılan değerleri kabul edin ve sonra **Tamam**’ı seçin.

    |Ayar|Değer|
    |---|---|
    |Ad|myVirtualNetwork1-myVirtualNetwork2|
    |Abonelik| Aboneliğinizi seçin.|
    |Sanal ağ|myVirtualNetwork2 - *myVirtualNetwork2* sanal ağını seçmek için **Sanal ağ**’ı seçin ve sonra **myVirtualNetwork2** seçeneğini belirleyin. Aynı bölgede veya farklı bir bölgede bulunan bir sanal ağı seçebilirsiniz.|

    ![Eşleme ayarları](./media/tutorial-connect-virtual-networks-portal/peering-settings.png)

    Aşağıdaki resimde gösterildiği gibi **EŞLEME DURUMU**, *Başlatıldı* durumundadır:

    ![Eşleme durumu](./media/tutorial-connect-virtual-networks-portal/peering-status.png)

    Durumu görmüyorsanız tarayıcınızı yenileyin.

4. Azure portalının üst kısmındaki **Arama** kutusuna *MyVirtualNetwork2* yazmaya başlayın. Arama sonuçlarında **myVirtualNetwork2** görüntülendiğinde bunu seçin.
5. Aşağıdaki değişikliklerle birlikte 2.-3. adımları tamamlayın ve sonra **Tamam**’ı seçin:

    |Ayar|Değer|
    |---|---|
    |Ad|myVirtualNetwork2-myVirtualNetwork1|
    |Sanal ağ|myVirtualNetwork1|

    **EŞLEME DURUMU**, *Bağlanıldı* durumundadır. Azure, *myVirtualNetwork2-myVirtualNetwork1* eşlemesi için eşleme durumunu *Başlatıldı* durumundan *Bağlanıldı* durumuna da geçirmiştir. Her iki sanal ağın da eşleme durumu *Bağlanıldı* olana kadar sanal ağ eşlemesi tam olarak oluşturulmuş olmaz. 

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Sonraki bir adımda aralarında iletişim kurabilmeniz için her sanal ağ üzerinde bir sanal makine oluşturun.

### <a name="create-the-first-vm"></a>Birinci sanal makineyi oluşturma

1. Azure portalının sol üst köşesinde bulunan **+ Kaynak oluştur** seçeneğini belirleyin.
2. **İşlem**'i seçin ve sonra da **Windows Server 2016 Datacenter**'ı seçin. Farklı bir işletim sistemi seçebilirsiniz, ancak geri kalan adımlarda, **Windows Server 2016 Datacenter** seçeneğini belirlediğiniz varsayılır. 
3. **Temel Bilgiler** için aşağıdaki bilgileri girin veya seçin, kalan ayarlar için varsayılan değerleri kabul edin ve sonra **Oluştur**’u seçin:

    |Ayar|Değer|
    |---|---|
    |Ad|myVm1|
    |Kullanıcı adı| Seçtiğiniz bir kullanıcı adını girin.|
    |Parola| Seçtiğiniz bir parolayı girin. Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.|
    |Kaynak grubu| **Mevcut olanı kullan**’ı seçin ve **myResourceGroup** seçeneğini belirleyin.|
    |Location| **Doğu ABD**’yi seçin.|
4. **Boyut seçin** bölümünden bir sanal makine boyutu seçin.
5. **Ayarlar** için aşağıdaki değerleri seçin ve **Tamam**’a tıklayın:

    |Ayar|Değer|
    |---|---|
    |Sanal ağ| myVirtualNetwork1 - Önceden seçili değilse **Sanal ağ**’ı seçin ve sonra **Sanal ağ seç** bölümünden **myVirtualNetwork1** seçeneğini belirleyin.|
    |Alt ağ| Subnet1 - Önceden seçili değilse **Alt ağ**’ı seçin ve sonra **Alt ağ seç** bölümünden **Subnet1** seçeneğini belirleyin.|
    
    ![Sanal makine ayarları](./media/tutorial-connect-virtual-networks-portal/virtual-machine-settings.png)
 
6. **Özet**’in **Oluştur** bölümünde **Oluştur**’u seçerek sanal makine dağıtımını başlatın.

### <a name="create-the-second-vm"></a>İkinci sanal makineyi oluşturma

Aşağıdaki değişikliklerle birlikte 1.-6. adımları tekrar tamamlayın:

|Ayar|Değer|
|---|---|
|Ad | myVm2|
|Sanal ağ | myVirtualNetwork2|

Sanal makinelerin oluşturulması birkaç dakika sürebilir. Her iki sanal makine de oluşturulmadan kalan adımlara devam etmeyin.

## <a name="communicate-between-vms"></a>Sanal makineler arasında iletişim

1. Portalın üst kısmındaki *Arama* kutusuna *myVm1* yazmaya başlayın. Arama sonuçlarında **myVm1** görüntülendiğinde bunu seçin.
2. Aşağıdaki resimde gösterildiği gibi, **Bağlan**’ı seçerek *myVm1* sanal makinesine uzak masaüstü bağlantısı oluşturun:

    ![Sanal makineye bağlanma](./media/tutorial-connect-virtual-networks-portal/connect-to-virtual-machine.png)  

3. Sanal makineye bağlanmak için indirilen RDP dosyasını açın. İstendiğinde **Bağlan**’ı seçin.
4. Sanal makineyi oluştururken belirttiğiniz kullanıcı adını ve parolayı girin (sanal makineyi oluştururken girdiğiniz kimlik bilgilerini belirtmek için **Diğer seçenekler**’i ve sonra **Farklı bir hesap kullan**’ı seçmeniz gerekebilir), ardından **Tamam**’ı seçin.
5. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Bağlantıya devam etmek için **Evet**’i seçin.
6. Sonraki bir adımda, *myVm1* sanal makinesinden *myVm2* sanal makinesiyle iletişim kurmak için ping kullanılır. Ping, varsayılan olarak Windows Güvenlik Duvarı üzerinden reddedilen İnternet Denetim İletisi Protokolü’nü (ICMP) kullanır. *myVm1* sanal makinesinde, Windows güvenlik duvarı üzerinden ICMP’yi etkinleştirin; böylece sonraki bir adımda PowerShell kullanarak *myVm2* sanal makinesinden bu sanal makineye ping komutu gönderebilirsiniz:

    ```powershell
    New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
    ```
    
    Bu öğreticide sanal makineler arasında iletişim kurmak için ping kullanılsa da, üretim dağıtımları için Windows Güvenlik Duvarı üzerinden ICMP’ye izin verilmesi önerilmez.

7. *myVm2* sanal makinesine bağlanmak için, *myVm1* sanal makinesinde bir komut isteminden aşağıdaki komutu girin:

    ```
    mstsc /v:10.1.0.4
    ```
    
8. *myVm1* sanal makinesinde ping’i etkinleştirdiğinizden şimdi IP adresine göre ping komutu gönderebilirsiniz:

    ```
    ping 10.0.0.4
    ```
    
9. Hem *myVm1* hem de *myVm2* sanal makinesine yönelik RDP oturumlarınızın bağlantısını kesin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu ve içerdiği tüm kaynakları silin: 

1. Portalın üst kısmındaki **Ara** kutusuna *myResourceGroup* değerini girin. Arama sonuçlarında **myResourceGroup** seçeneğini gördüğünüzde bunu seçin.
2. **Kaynak grubunu sil**'i seçin.
3. **KAYNAK GRUBU ADINI YAZIN:** için *myResourceGroup* girin ve **Sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, sanal ağ eşlemesi ile aynı Azure bölgesindeki iki ağın nasıl bağlanacağını öğrendiniz. Farklı [desteklenen bölgelerde](virtual-network-manage-peering.md#cross-region) ve [farklı Azure aboneliklerinde](create-peering-different-subscriptions.md#portal) sanal ağları eşleyebilir ve eşleme ile [hub ve bağlı bileşen ağ tasarımları](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) oluşturabilirsiniz. Sanal ağ eşlemesi hakkında daha fazla bilgi için bkz. [Sanal ağ eşlemesine genel bakış](virtual-network-peering-overview.md) ve [Sanal ağ eşlemelerini yönetme](virtual-network-manage-peering.md).

Kendi bilgisayarınızı VPN üzerinden bir sanal ağa bağlamak ve bir sanal ağdaki veya eşlenmiş sanal ağlardaki kaynaklarla etkileşim kurmak için bkz. [Bilgisayarınızı sanal ağa bağlama](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
