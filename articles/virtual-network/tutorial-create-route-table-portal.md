---
title: Ağ trafiğini yönlendirme - öğretici - Azure portalı | Microsoft Docs
description: Bu öğreticide, Azure portalını kullanarak bir yönlendirme tablosu ile ağ trafiğini yönlendirme hakkında bilgi edineceksiniz.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I want to route traffic from one subnet, to a different subnet, through a network virtual appliance.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/13/2018
ms.author: jdial
ms.custom: mvc
ms.openlocfilehash: 7254e9336fca14daee2021d5bde4c5538509fe35
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="tutorial-route-network-traffic-with-a-route-table-using-the-azure-portal"></a>Öğretici: Azure portalını kullanarak bir yönlendirme tablosu ile ağ trafiğini yönlendirme

Azure varsayılan olarak bir sanal ağ içindeki tüm alt ağlar arasında gerçekleşen trafiği otomatik olarak yönlendirir. Azure’ın varsayılan yönlendirmesini geçersiz kılmak için kendi yönlendirmelerinizi oluşturabilirsiniz. Örneğin, bir ağ sanal gereci üzerinden alt ağlar arasındaki trafiği yönlendirmek isteyebilirsiniz. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Yönlendirme tablosu oluşturma
> * Yönlendirme oluşturma
> * Birden fazla alt ağa sahip bir sanal ağ oluşturma
> * Yönlendirme tablosunu bir alt ağ ile ilişkilendirme
> * Trafiği yönlendiren bir NVA oluşturma
> * Sanal makineleri (VM) farklı alt ağlara dağıtma
> * NVA aracılığıyla trafiği bir alt ağdan başka birine yönlendirme

Tercih ederseniz, [Azure CLI](tutorial-create-route-table-cli.md) veya [Azure PowerShell](tutorial-create-route-table-powershell.md) kullanarak bu öğreticiyi tamamlayabilirsiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="log-in-to-azure"></a>Azure'da oturum açma 

http://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="create-a-route-table"></a>Yönlendirme tablosu oluşturma

1. Azure portalının sol üst köşesinde bulunan **+ Kaynak oluştur** seçeneğini belirleyin.
2. **Ağ**’ı ve sonra **Yönlendirme tablosu**’nu seçin.
3. Aşağıdaki bilgileri girin veya seçin, kalan ayar için varsayılan değeri kabul edin ve sonra **Oluştur**’u seçin:

    |Ayar|Değer|
    |---|---|
    |Adı|myRouteTablePublic|
    |Abonelik| Aboneliğinizi seçin.|
    |Kaynak grubu | **Yeni oluştur**’u seçin ve *myResourceGroup* değerini girin.|
    |Konum|Doğu ABD|
 
    ![Yönlendirme tablosu oluşturma](./media/tutorial-create-route-table-portal/create-route-table.png) 

## <a name="create-a-route"></a>Yönlendirme oluşturma

1. Portalın üst kısmındaki *Kaynak, hizmet ve belgeleri arayın* kutusuna *myRouteTablePublic* yazmaya başlayın. Arama sonuçlarında **myRouteTablePublic** göründüğünde seçin.
2. **AYARLAR** altında **Yönlendirmeler**’i sonra aşağıdaki resimde gösterildiği gibi **+ Ekle**’yi seçin:

    ![Yönlendirme ekleme](./media/tutorial-create-route-table-portal/add-route.png) 
 
3. **Yönlendirme ekle** altında aşağıdaki bilgileri girin veya seçin, kalan ayarlar için varsayılan değerleri kabul edin ve sonra **Oluştur**’u seçin:

    |Ayar|Değer|
    |---|---|
    |Yönlendirme adı|ToPrivateSubnet|
    |Adres ön eki| 10.0.1.0/24|
    |Sonraki atlama türü | **Sanal gereç**’i seçin.|
    |Sonraki atlama adresi| 10.0.2.4|

## <a name="associate-a-route-table-to-a-subnet"></a>Yönlendirme tablosunu bir alt ağ ile ilişkilendirme

Yönlendirme tablosunu bir alt ağ ile ilişkilendirebilmek için bir sanal ağ ve alt ağ oluşturmanız gerekir; daha sonra yönlendirme tablosunu bir alt ağ ile ilişkilendirebilirsiniz:

1. Azure portalının sol üst köşesinde bulunan **+ Kaynak oluştur** seçeneğini belirleyin.
2. **Ağ**’ı ve sonra **Sanal ağ**’ı seçin.
3. **Sanal ağ oluştur** altında aşağıdaki bilgileri girin veya seçin, kalan ayarlar için varsayılan değerleri kabul edin ve sonra **Oluştur**’u seçin:

    |Ayar|Değer|
    |---|---|
    |Adı|myVirtualNetwork|
    |Adres alanı| 10.0.0.0/16|
    |Abonelik | Aboneliğinizi seçin.|
    |Kaynak grubu|**Mevcut olanı kullan**’ı seçin ve **myResourceGroup** seçeneğini belirleyin.|
    |Konum|*Doğu ABD*’yi seçin|
    |Alt ağ adı|Genel|
    |Adres aralığı|10.0.0.0/24|
    
4. Portalın üst kısmındaki **Kaynak, hizmet ve belgeleri arayın** kutusuna *myVirtualNetwork* yazmaya başlayın. Arama sonuçlarında **myVirtualNetwork** göründüğünde seçin.
5. **AYARLAR** altında **Alt Ağlar**’ı ve sonra aşağıdaki resimde gösterildiği gibi **+ Alt Ağ**’ı seçin:

    ![Alt ağ ekleme](./media/tutorial-create-route-table-portal/add-subnet.png) 

6. Aşağıdaki bilgileri seçin veya girin ve sonra **Tamam**’ı seçin:

    |Ayar|Değer|
    |---|---|
    |Adı|Özel|
    |Adres alanı| 10.0.1.0/24|

7. Aşağıdaki bilgileri sağlayarak 5. ve 6. adımları yeniden tamamlayın:

    |Ayar|Değer|
    |---|---|
    |Adı|DMZ|
    |Adres alanı| 10.0.2.0/24|

8. Yukarıdaki adım tamamlandıktan sonra **myVirtualNetwork - Alt Ağlar** kutusu gösterilir. **AYARLAR** altında **Alt Ağlar**’ı ve sonra **Genel**’i seçin.
9. Aşağıdaki resimde gösterildiği gibi **Yönlendirme tablosu**’nu, **MyRouteTablePublic** öğesini ve ardından **Kaydet**’i seçin:

    ![Yönlendirme tablosunu ilişkilendirme](./media/tutorial-create-route-table-portal/associate-route-table.png) 

## <a name="create-an-nva"></a>NVA oluşturma

NVA; yönlendirme, güvenlik duvarı oluşturma veya WAN iyileştirmesi gibi ağ işlevlerini gerçekleştiren bir VM'dir.

1. Azure portalının sol üst köşesinde bulunan **+ Kaynak oluştur** seçeneğini belirleyin.
2. **İşlem**'i seçin ve sonra da **Windows Server 2016 Datacenter**'ı seçin. Farklı bir işletim sistemi seçebilirsiniz, ancak geri kalan adımlarda, **Windows Server 2016 Datacenter** seçeneğini belirlediğiniz varsayılır. 
3. **Temel** için aşağıdaki bilgileri seçin veya girin ve sonra **Tamam**’ı seçin:

    |Ayar|Değer|
    |---|---|
    |Adı|myVmNva|
    |Kullanıcı adı|Seçtiğiniz bir kullanıcı adını girin.|
    |Parola|Seçtiğiniz bir parolayı girin. Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.|
    |Kaynak grubu| **Mevcut olanı kullan**’ı seçin ve *myResourceGroup* seçeneğini belirleyin.|
    |Konum|**Doğu ABD**’yi seçin.|
4. **Boyut seçin** bölümünden bir sanal makine boyutu seçin.
5. **Ayarlar** için aşağıdaki bilgileri seçin veya girin ve sonra **Tamam**’ı seçin:

    |Ayar|Değer|
    |---|---|
    |Sanal ağ|myVirtualNetwork - Seçili değilse **Sanal ağ**’ı seçin ve sonra **Sanal ağ seç** bölümünden **myVirtualNetwork** seçeneğini belirleyin.|
    |Alt ağ|**Alt ağ**’ı ve sonra **Alt ağ seç** altında **DMZ**’yi seçin. |
    |Genel IP adresi| **Genel IP adresi** ve **Genel IP adresi seçin** altında **Yok**’u seçin. İnternet’ten bağlantı kurulmayacağı için bu VM’ye bir genel IP adresi atanmaz.
6. **Özet**’in **Oluştur** bölümünde **Oluştur**’u seçerek sanal makine dağıtımını başlatın.

    Sanal makinenin oluşturulması birkaç dakika sürer. Azure VM oluşturma işlemini tamamlayıp VM hakkında bilgiler içeren bir kutu açmadan sonraki adıma geçmeyin.

7. VM oluşturulduktan sonra açılan kutuda **AYARLAR** altında **Ağ**’ı ve sonra aşağıdaki resimde gösterildiği gibi **myvmnva158** öğesini seçin (Azure’ın sanal makineniz için oluşturduğu ağ arabirimi **myvmnva**’dan sonra farklı bir numaraya sahiptir):

    ![VM ağı](./media/tutorial-create-route-table-portal/virtual-machine-networking.png) 

8. Bir ağ arabiriminin kendi IP adresini hedeflemeden kendisine gönderilen ağ trafiğini iletebilmesi için, ağ arabiriminde IP iletme özelliğinin etkinleştirilmiş olması gerekir. **AYARLAR** altında **IP yapılandırmaları**’nı, **IP iletme** için **Etkin**’i ve sonra aşağıdaki resimde gösterildiği gibi **Kaydet**’i seçin:

    ![IP iletmeyi etkinleştirme](./media/tutorial-create-route-table-portal/enable-ip-forwarding.png) 

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

*Genel* alt ağdan gelen trafiğin daha sonraki bir adımda NVA aracılığıyla *Özel* alt ağa yönlendirildiğini doğrulayabilmek için sanal ağda iki VM oluşturun. [NVA oluşturma](#create-a-network-virtual-appliance) bölümündeki 1-6. adımları tamamlayın. Aşağıdaki değişiklikler dışında 3. ve 5. adımdaki ayarların aynısını kullanın:

|Sanal makine adı      |Alt ağ      | Genel IP adresi     |
|--------- | -----------|---------              |
| myVmPublic  | Genel     | Portal varsayılan değerini kabul edin |
| myVmPrivate | Özel    | Portal varsayılan değerini kabul edin |

Azure *myVmPublic* VM’yi oluştururken *myVmPrivate* VM’yi oluşturabilirsiniz. Azure her iki VM’yi oluşturma işlemini tamamlayana kadar sonraki adımlara geçmeyin.

## <a name="route-traffic-through-an-nva"></a>Trafiği NVA üzerinden yönlendirme

1. Portalın üst kısmındaki *Arama* kutusuna *myVmPrivate* yazmaya başlayın. Arama sonuçlarında **myVmPrivate** VM göründüğünde seçin.
2. Aşağıdaki resimde gösterildiği gibi **Bağlan**’ı seçerek *myVmPrivate* sanal makinesiyle uzak masaüstü bağlantısı oluşturun:

    ![VM’ye bağlanma ](./media/tutorial-create-route-table-portal/connect-to-virtual-machine.png)  

3. Sanal makineye bağlanmak için indirilen RDP dosyasını açın. İstendiğinde **Bağlan**’ı seçin.
4. Sanal makineyi oluştururken belirttiğiniz kullanıcı adını ve parolayı girin (sanal makineyi oluştururken girdiğiniz kimlik bilgilerini belirtmek için **Diğer seçenekler**’i ve sonra **Farklı bir hesap kullan**’ı seçmeniz gerekebilir), ardından **Tamam**’ı seçin.
5. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Bağlantıya devam etmek için **Evet**’i seçin.
6. Sonraki bir adımda yönlendirmeyi test etmek için izleme yönlendirme aracı kullanılacaktır. İzleme yönlendirmesi, Windows Güvenlik Duvarı üzerinden reddedilen İnternet Denetim İletisi Protokolü’nü (ICMP) kullanır. PowerShell’den *myVmPrivate* sanal makinesinde aşağıdaki komutu girerek Windows güvenlik duvarı üzerinden ICMP’yi etkinleştirin:

    ```powershell
    New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
    ```

    Bu öğreticide yönlendirmeyi test etmek için izleme yönlendirme kullanılsa da, üretim dağıtımları için Windows Güvenlik Duvarı üzerinden ICMP’ye izin verilmesi önerilmez.
7. [IP iletmeyi etkinleştirme](#enable-ip-forwarding) bölümünde, VM'nin ağ arabirimi için Azure’da IP iletimini etkinleştirdiniz. VM içindeki işletim sistemi veya VM içinde çalışan bir uygulama da ağ trafiğini iletebilmelidir. *myVmNva* sanal makinesinin işletim sisteminde IP iletmeyi etkinleştirin:

    *myVmPrivate* VM üzerinde bir komut isteminden *myVmNva* VM ile uzak masaüstü bağlantısı:

    ``` 
    mstsc /v:myvmnva
    ```
    
    İşletim sistemi içinde IP iletmeyi etkinleştirmek için *myVmNva* sanal makinesinden PowerShell’de aşağıdaki komutu girin:

    ```powershell
    Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters -Name IpEnableRouter -Value 1
    ```
    
    *myVmNva* sanal makinesini yeniden başlatarak uzak masaüstü oturumunun bağlantısını da kesin.
8. *myVmPrivate* VM bağlantısı devam ederken, *myVmNva* VM yeniden başlatıldıktan sonra *myVmPublic* VM ile bir uzak masaüstü oturumu oluşturun:

    ``` 
    mstsc /v:myVmPublic
    ```
    
    PowerShell’den *myVmPublic* sanal makinesinde aşağıdaki komutu girerek Windows güvenlik duvarı üzerinden ICMP’yi etkinleştirin:

    ```powershell
    New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
    ```

9. Ağ trafiğinin *myVmPrivate* sanal makinesinden *myVmPublic* sanal makinesine yönlendirilmesini test etmek için *myVmPublic* sanal makinesinde PowerShell’den aşağıdaki komutu girin:

    ```
    tracert myVmPrivate
    ```

    Yanıt aşağıdaki örneğe benzer:
    
    ```
    Tracing route to myVmPrivate.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.1.4]
    over a maximum of 30 hops:
        
    1    <1 ms     *        1 ms  10.0.2.4
    2     1 ms     1 ms     1 ms  10.0.1.4
        
    Trace complete.
    ```
      
    İlk atlamanın, NVA özel IP adresi olan 10.0.2.4 olduğunu görebilirsiniz. İkinci atlama ise *myVmPrivate* sanal makinesinin özel IP adresi olan 10.0.1.4’tür. *myRouteTablePublic* yönlendirme tablosuna eklenip *Genel* alt ağ ile ilişkilendirilen yönlendirme, Azure’un trafiği doğrudan *Özel* alt ağına yönlendirmek yerine NVA aracılığıyla yönlendirmesine neden olmuştur.
10.  *myVmPublic* VM ile uzak masaüstü oturumunu kapatın. *myVmPrivate* VM bağlantınız hala açıktır.
11. Ağ trafiğinin *myVmPublic* sanal makinesinden *myVmPrivate* sanal makinesine yönlendirilmesini test etmek için *myVmPrivate* sanal makinesinde bir komut isteminden aşağıdaki komutu girin:

    ```
    tracert myVmPublic
    ```

    Yanıt aşağıdaki örneğe benzer:

    ```
    Tracing route to myVmPublic.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.0.4]
    over a maximum of 30 hops:
    
    1     1 ms     1 ms     1 ms  10.0.0.4
    
    Trace complete.
    ```

    Trafiğin *myVmPrivate* sanal makinesinden *myVmPublic* sanal makinesine doğrudan yönlendirildiğini görebilirsiniz. Varsayılan olarak Azure, trafiği doğrudan alt ağlar arasında yönlendirir.
12. *myVmPrivate* VM ile uzak masaüstü oturumunu kapatın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu ve içerdiği tüm kaynakları silin: 

1. Portalın üst kısmındaki **Ara** kutusuna *myResourceGroup* değerini girin. Arama sonuçlarında **myResourceGroup** seçeneğini gördüğünüzde bunu seçin.
2. **Kaynak grubunu sil**'i seçin.
3. **KAYNAK GRUBU ADINI YAZIN:** için *myResourceGroup* girin ve **Sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide bir yönlendirme tablosu oluşturup bir alt ağ ile ilişkilendirdiniz. Bir genel alt ağdan özel alt ağa trafiği yönlendiren basit bir NVA oluşturdunuz. [Azure Market](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking)’ten güvenlik duvarı ve WAN iyileştirme gibi ağ işlevleri gerçekleştiren, önceden yapılandırılmış çeşitli NVA’lar dağıtın. Yönlendirme hakkında daha fazla bilgi için bkz. [Yönlendirmeye genel bakış](virtual-networks-udr-overview.md) ve [Yönlendirme tablosunu yönetme](manage-route-table.md).


Bir sanal ağ içinde çok sayıda Azure kaynağına dağıtabilmenize karşın, bazı Azure PaaS hizmetlerinin kaynakları bir sanal ağa dağıtılamaz. Yine de, bazı Azure PaaS hizmetlerinin kaynaklarına erişimi yalnızca bir sanal ağ alt ağından gelecek trafikle kısıtlayabilirsiniz. Azure PaaS kaynaklarına erişimi kısıtlama hakkında bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [PaaS kaynaklarına ağ erişimini kısıtlama](tutorial-restrict-network-access-to-resources.md)
