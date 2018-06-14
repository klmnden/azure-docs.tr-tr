---
title: Sanal ağ oluşturma - hızlı başlangıç - Azure portalı | Microsoft Docs
description: Bu hızlı başlangıçta, Azure portalını kullanarak sanal ağ oluşturmayı öğreneceksiniz. Sanal ağ, sanal makineler gibi Azure kaynaklarının birbiriyle ve İnternet ile özel olarak iletişim kurmasına olanak sağlar.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I want to create a virtual network so that virtual machines can communicate with privately with each other and with the internet.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/09/2018
ms.author: jdial
ms.custom: mvc
ms.openlocfilehash: 7107dc72686004141d8bea0083089cba065a9f4c
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
ms.locfileid: "30841410"
---
# <a name="quickstart-create-a-virtual-network-using-the-azure-portal"></a>Hızlı başlangıç: Azure portalını kullanarak bir sanal ağ oluşturma

Sanal ağ, sanal makineler (VM) gibi Azure kaynaklarının birbiriyle ve İnternet ile özel olarak iletişim kurmasına olanak sağlar. Bu hızlı başlangıçta, sanal ağ oluşturmayı öğreneceksiniz. Bir sanal ağ oluşturduktan sonra, sanal ağa iki sanal makine dağıtacaksınız. Ardından İnternet’ten bir sanal makineye bağlanacak ve iki sanal makine arasında özel olarak iletişim kuracaksınız.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="log-in-to-azure"></a>Azure'da oturum açma 

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

1. Azure portalının sol üst köşesinde bulunan **+ Kaynak oluştur** seçeneğini belirleyin.
2. **Ağ**’ı ve sonra **Sanal ağ**’ı seçin.
3. Aşağıdaki bilgileri girin veya seçin, kalan ayarlar için varsayılan değerleri kabul edin ve sonra **Oluştur**’u seçin:

    |Ayar|Değer|
    |---|---|
    |Adı|myVirtualNetwork|
    |Abonelik| Aboneliğinizi seçin.|
    |Kaynak grubu| **Yeni oluştur**’u seçin ve *myResourceGroup* değerini girin.|
    |Konum| **Doğu ABD**’yi seçin.|

    ![Sanal ağınızla ilgili temel bilgileri girin](./media/quick-create-portal/create-virtual-network.png)

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Sanal ağ üzerinde iki sanal makine oluşturun:

### <a name="create-the-first-vm"></a>Birinci sanal makineyi oluşturma

1. Azure portalının sol üst köşesinde bulunan **+ Kaynak oluştur** seçeneğini belirleyin.
2. **İşlem**'i seçin ve sonra da **Windows Server 2016 Datacenter**'ı seçin.
3. Aşağıdaki bilgileri girin veya seçin, kalan ayarlar için varsayılan değerleri kabul edin ve sonra **Tamam**’ı seçin:

    |Ayar|Değer|
    |---|---|
    |Adı|myVm1|
    |Kullanıcı adı| Seçtiğiniz bir kullanıcı adını girin.|
    |Parola| Seçtiğiniz bir parolayı girin. Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.|
    |Abonelik| Aboneliğinizi seçin.|
    |Kaynak grubu| **Mevcut olanı kullan**’ı seçin ve **myResourceGroup** seçeneğini belirleyin.|
    |Konum| **Doğu ABD**’yi seçin|

    ![Sanal makine temel bilgileri](./media/quick-create-portal/virtual-machine-basics.png)

4. Sanal makine için bir boyut seçin ve **Seç** seçeneğini belirleyin.
5. **Ayarlar** bölümünde tüm varsayılan değerleri kabul edin ve **Tamam**’ı seçin.

    ![Sanal makine ayarları](./media/quick-create-portal/virtual-machine-settings.png)

6. **Özet**’in **Oluştur** bölümünde **Oluştur**’u seçerek sanal makine dağıtımını başlatın. Sanal makinenin dağıtılması birkaç dakika sürer. 

### <a name="create-the-second-vm"></a>İkinci sanal makineyi oluşturma

1.-6. adımları tekrar tamamlayın, ancak 3. adımda sanal makineye *myVm2* adını verin.

## <a name="connect-to-a-vm-from-the-internet"></a>İnternet'ten bir sanal makineye bağlanma

1. *myVm1* oluşturulduktan sonra buna bağlanın. Azure portalının en üstüne *myVm1* değerini girin. Arama sonuçlarında **myVm1** görüntülendiğinde bunu seçin. **Bağlan** düğmesini seçin.

    ![Sanal makineye bağlanma](./media/quick-create-portal/connect-to-virtual-machine.png)

2. **Bağlan** düğmesini seçmenizin ardından bir Uzak Masaüstü Protokolü (.rdp) dosyası oluşturulur ve bilgisayarınıza indirilir.  
3. İndirilen rdp dosyasını açın. İstendiğinde **Bağlan**’ı seçin. Sanal makine oluştururken belirttiğiniz kullanıcı adını ve parolayı girin. Sanal makineyi oluştururken girdiğiniz kimlik bilgilerini belirtmek için **Diğer seçenekler**’i ve sonra **Farklı bir hesap kullan** seçeneğini belirlemeniz gerekebilir. 
4. **Tamam**’ı seçin.
5. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Uyarı alırsanız, bağlantıya devam etmek için **Evet**’i veya **Bağlan**’ı seçin.

## <a name="communicate-between-vms"></a>Sanal makineler arasında iletişim

1. PowerShell’den `ping myvm2` komutunu girin. Ping, İnternet Denetim İletisi Protokolü’nü (ICMP) kullandığından ve varsayılan olarak Windows güvenlik duvarı üzerinden ICMP’ye izin verilmediğinden ping başarısız olur.
2. *myVm2*’nin sonraki bir adımda *myVm1*’e ping komutu göndermesine izin vermek için, PowerShell’den, Windows güvenlik duvarı üzerinden gelen ICMP’ye izin veren aşağıdaki komutu girin:

    ```powershell
    New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
    ```

3. *myVm1* ile uzak masaüstü bağlantısını kapatın. 

4. [İnternet'ten bir sanal makineye bağlanma](#connect-to-a-vm-from-the-internet) bölümündeki adımları tekrar tamamlayın, ancak *myVm2*’ye bağlanın. Bir komut isteminden `ping myvm1` komutunu girin.

    Önceki bir adımda *myVm1* sanal makinesinde Windows güvenlik duvarı üzerinden ICMP’ye izin verdiğinizden, *myVm1*’den yanıt alırsınız.

5. *myVm2* ile uzak masaüstü bağlantısını kapatın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu ve içerdiği tüm kaynakları silin:

1. Portalın üst kısmındaki **Ara** kutusuna *myResourceGroup* değerini girin. Arama sonuçlarında **myResourceGroup** seçeneğini gördüğünüzde bunu seçin.
2. **Kaynak grubunu sil**'i seçin.
3. **KAYNAK GRUBU ADINI YAZIN:** için *myResourceGroup* girin ve **Sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, varsayılan bir sanal ağ ve iki sanal makine oluşturdunuz. İnternet’ten bir sanal makineye bağlandınız ve sanal makine ile başka bir sanal makine arasında özel olarak iletişim kurdunuz. Sanal ağ ayarları hakkında daha fazla bilgi için [Sanal ağı yönetme](manage-virtual-network.md) başlıklı konuya bakın.

Varsayılan olarak Azure, sanal makineler arasında kısıtlanmamış özel iletişime olanak sağlar, ancak yalnızca İnternet’ten Windows sanal makinelerine gelen uzak masaüstü bağlantılarına izin verir. Sanal makinelere gelen ve sanal makinelerden giden farklı ağ iletişimi türlerine izin verme veya bunları kısıtlama hakkında bilgi edinmek için [Ağ trafiğini filtreleme](tutorial-filter-network-traffic.md) öğreticisine ilerleyin.