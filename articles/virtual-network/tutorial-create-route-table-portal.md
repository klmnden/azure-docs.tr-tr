---
title: Rota ağ trafiği - öğretici - Azure portalı
titlesuffix: Azure Virtual Network
description: Bu öğreticide, Azure portalını kullanarak bir yönlendirme tablosu ile ağ trafiğini yönlendirme hakkında bilgi edineceksiniz.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: twooley
editor: ''
tags: azure-resource-manager
Customer intent: I want to route traffic from one subnet, to a different subnet, through a network virtual appliance.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 12/12/2018
ms.author: kumud
ms.custom: mvc
ms.openlocfilehash: 855adccf036f731de12810fe0f5287186048ddb0
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62098650"
---
# <a name="tutorial-route-network-traffic-with-a-route-table-using-the-azure-portal"></a>Öğretici: Azure portalını kullanarak bir yönlendirme tablosu ile ağ trafiğini yönlendirme

Varsayılan olarak bir sanal ağ içindeki tüm alt ağlar arasında trafiği Azure yönlendirir. Azure’ın varsayılan yönlendirmesini geçersiz kılmak için kendi yönlendirmelerinizi oluşturabilirsiniz. Örneğin, bir ağ sanal gereci üzerinden alt ağlar arasındaki trafiği yönlendirmek isteyebilirsiniz. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Yönlendirme tablosu oluşturma
> * Yönlendirme oluşturma
> * Birden fazla alt ağa sahip bir sanal ağ oluşturma
> * Yönlendirme tablosunu bir alt ağ ile ilişkilendirme
> * Trafiği yönlendiren bir NVA oluşturma
> * Sanal makineleri (VM) farklı alt ağlara dağıtma
> * NVA aracılığıyla trafiği bir alt ağdan başka birine yönlendirme

Tercih ederseniz Bu öğretici kullanarak tamamlayabilir [Azure CLI](tutorial-create-route-table-cli.md) veya [Azure PowerShell](tutorial-create-route-table-powershell.md).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure Portal](https://portal.azure.com) oturum açın.

## <a name="create-a-route-table"></a>Yönlendirme tablosu oluşturma

1. Ekranın sol üst tarafında seçin **kaynak Oluştur** > **ağ** > **yol tablosu**.

1. İçinde **Create yol tablosu**bu bilgileri seçin veya girin:

    | Ayar | Değer |
    | ------- | ----- |
    | Ad | Girin *myRouteTablePublic*. |
    | Abonelik | Aboneliğinizi seçin. |
    | Kaynak grubu | Seçin **Yeni Oluştur**, girin *myResourceGroup*seçip *Tamam*. |
    | Location | Varsayılan değeri bırakın **Doğu ABD**.
    | BGP rota yayma | Varsayılan değeri bırakın **etkin**. |

1. **Oluştur**’u seçin.

## <a name="create-a-route"></a>Yönlendirme oluşturma

1. Portalın arama çubuğunda girin *myRouteTablePublic*.

1. Arama sonuçlarında **myRouteTablePublic** göründüğünde seçin.

1. İçinde **myRouteTablePublic** altında **ayarları**seçin **yollar** > **+ Ekle**.

    ![Yönlendirme ekleme](./media/tutorial-create-route-table-portal/add-route.png)

1. İçinde **yönlendirme Ekle**bu bilgileri seçin veya girin:

    | Ayar | Değer |
    | ------- | ----- |
    | Yönlendirme adı | Girin *ToPrivateSubnet*. |
    | Adres ön eki | Girin *10.0.1.0/24*. |
    | Sonraki atlama türü | **Sanal gereç**’i seçin. |
    | Sonraki atlama adresi | Girin *10.0.2.4*. |

1. **Tamam**’ı seçin.

## <a name="associate-a-route-table-to-a-subnet"></a>Yönlendirme tablosunu bir alt ağ ile ilişkilendirme

Bir yönlendirme tablosunu bir alt ağ ilişkilendirmeden önce bir sanal ağ ve alt ağ oluşturmanız gerekir.

### <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

1. Ekranın sol üst tarafında seçin **kaynak Oluştur** > **ağ** > **sanal ağ**.

1. İçinde **sanal ağ oluştur**bu bilgileri seçin veya girin:

    | Ayar | Değer |
    | ------- | ----- |
    | Ad | Girin *myVirtualNetwork*. |
    | Adres alanı | Girin *10.0.0.0/16*. |
    | Abonelik | Aboneliğinizi seçin. |
    | Kaynak grubu | Seçin ***var olanı Seç*** > **myResourceGroup**. |
    | Location | Varsayılan değeri bırakın **Doğu ABD**. |
    | Alt ağ - adı | Girin *genel*. |
    | Alt Ağ - Adres aralığı | Girin *10.0.0.0/24*. |

1. Geri kalan seçin ve varsayılan değerleri bırakın **Oluştur**.

### <a name="add-subnets-to-the-virtual-network"></a>Alt ağlar sanal ağa ekleyin.

1. Portalın arama çubuğunda girin *myVirtualNetwork*.

1. Arama sonuçlarında **myVirtualNetwork** göründüğünde seçin.

1. İçinde **myVirtualNetwork**altında **ayarları**seçin **alt ağlar** > **+ alt ağ**.

    ![Alt ağ ekleme](./media/tutorial-create-route-table-portal/add-subnet.png)

1. İçinde **alt ağ Ekle**, bu bilgileri girin:

    | Ayar | Değer |
    | ------- | ----- |
    | Ad | Girin *özel*. |
    | Adres alanı | Girin *10.0.1.0/24*. |

1. Diğer varsayılan ayarları olduğu gibi bırakın ve **Tamam**’ı seçin.

1. Seçin **+ alt ağ** yeniden. Bu kez, bu bilgileri girin:

    | Ayar | Değer |
    | ------- | ----- |
    | Ad | Girin *DMZ*. |
    | Adres alanı | Girin *10.0.2.0/24*. |

1. Geri kalan varsayılan değerleri bırakın ve Seç gibi son **Tamam**.

    Azure, üç alt gösterir: **Genel**, **özel**, ve **DMZ**.

### <a name="associate-myroutetablepublic-to-your-public-subnet"></a>Genel alt ağınız için myRouteTablePublic ilişkilendirin

1. Seçin **genel**.

1. İçinde **genel**seçin **yol tablosu** > **MyRouteTablePublic** > **Kaydet**.

    ![Yönlendirme tablosunu ilişkilendirme](./media/tutorial-create-route-table-portal/associate-route-table.png)

## <a name="create-an-nva"></a>NVA oluşturma

Nva'ları, Yönlendirme ve güvenlik duvarı iyileştirme gibi ağ işlevlerini yardımcı vm'leridir. İsterseniz, farklı bir işletim sistemi seçebilirsiniz. Bu öğreticide kullandığınız varsayılır **Windows Server 2016 Datacenter**.

1. Ekranın sol üst tarafında seçin **kaynak Oluştur** > **işlem** > **Windows Server 2016 Datacenter**.

1. İçinde **temel bilgileri - sanal makine oluşturma**bu bilgileri seçin veya girin:

    | Ayar | Değer |
    | ------- | ----- |
    | **PROJE AYRINTILARI** | |
    | Abonelik | Aboneliğinizi seçin. |
    | Kaynak grubu | Seçin **myResourceGroup**. |
    | **ÖRNEK AYRINTILARI** |  |
    | Sanal makine adı | Girin *myVmNva*. |
    | Bölge | **Doğu ABD**’yi seçin. |
    | Kullanılabilirlik seçenekleri | Varsayılan değeri bırakın **gerekli altyapı artıklık**. |
    | Image | Varsayılan değeri bırakın **Windows Server 2016 Datacenter**. |
    | Boyut | Varsayılan değeri bırakın **standart DS1 v2**. |
    | **YÖNETİCİ HESABI** |  |
    | Kullanıcı adı | Seçtiğiniz bir kullanıcı adını girin. |
    | Parola | Seçtiğiniz bir parolayı girin. Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.|
    | Parolayı Onayla | Parolayı yeniden girin. |
    | **GELEN BAĞLANTI NOKTASI KURALLARI** |  |
    | Ortak gelen bağlantı noktası | Varsayılan değeri bırakın **hiçbiri**.
    | **TASARRUF EDİN** |  |
    | Zaten bir Windows lisansınız var mı? | Varsayılan değeri bırakın **Hayır**. |

1. Seçin **sonraki: Diskleri**.

1. İçinde **- sanal makine diskleri oluşturma**, gereksinimleriniz için doğru olan ayarlarını seçin.

1. Seçin **sonraki: Ağ**.

1. İçinde **ağ - sanal makine oluşturma**, bu bilgileri seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | Sanal ağ | Varsayılan değeri bırakın **myVirtualNetwork**. |
    | Alt ağ | Seçin **DMZ (10.0.2.0/24)**. |
    | Genel IP | Seçin **hiçbiri**. Genel bir IP adresi gerekmez. VM, internet üzerinden bağlanabilirsiniz olmaz.|

1. Geri kalan seçin ve varsayılan değerleri bırakın **sonraki: Yönetim**.

1. İçinde **Yönetimi - Sanal makine oluşturma**, için **tanılama depolama hesabı**seçin **Yeni Oluştur**.

1. İçinde **depolama hesabı oluşturma**bu bilgileri seçin veya girin:

    | Ayar | Değer |
    | ------- | ----- |
    | Ad | Girin *mynvastorageaccount*. |
    | Hesap türü | Varsayılan değeri bırakın **depolama (genel amaçlı v1)**. |
    | Performans | Varsayılan değeri bırakın **standart**. |
    | Çoğaltma | Varsayılan değeri bırakın **yerel olarak yedekli depolama (LRS)**.

1. **Tamam**’ı seçin

1. **İncele ve oluştur**’u seçin. Adresine yönlendirilirsiniz **gözden geçir + Oluştur** sayfası ve Azure yapılandırmanızı doğrular.

1. Gördüğünüzde, **doğrulama başarılı**seçin **Oluştur**.

    Sanal makinenin oluşturulması birkaç dakika sürer. Azure VM oluşturma işlemini tamamlayana kadar devam edin yok. **Devam ettiği dağıtımıdır** sayfasında, dağıtım ayrıntıları gösterilir.

1. Sanal makinenizin hazır olduğunda seçin **kaynağa Git**.

## <a name="turn-on-ip-forwarding"></a>IP iletimi üzerinde Aç

IP iletme için açma *myVmNva*. Azure ağ trafiğini gönderdiğinde *myVmNva*, trafik farklı bir IP adresi için hedeflenen, IP iletme doğru konuma trafiği gönderir.

1. Üzerinde **myVmNva**altında **ayarları**seçin **ağ**.

1. Select **myvmnva123**. Azure sanal Makineniz için oluşturulan ağ arabirimini olmasıdır. Onu benzersiz yapmak için sayıdan oluşan bir dize olması.

    ![VM ağı](./media/tutorial-create-route-table-portal/virtual-machine-networking.png)

1. Altında **ayarları**seçin **IP yapılandırmaları**.

1. Üzerinde **myvmnva123 - IP yapılandırmaları**, için **IP iletme**seçin **etkin** seçip **Kaydet**.

    ![IP iletmeyi etkinleştirme](./media/tutorial-create-route-table-portal/enable-ip-forwarding.png)

## <a name="create-public-and-private-virtual-machines"></a>Genel ve özel sanal makine oluşturma

Genel bir VM ile özel bir VM sanal ağ oluşturun. Azure'nın yönlendirdiğini görmek üzere bunları daha sonra kullanacağınız *genel* alt ağ trafiği *özel* NVA aracılığıyla alt ağ.

1-12 adımlarını tamamlamanız [bir NVA oluşturma](#create-an-nva). Aynı ayarların çoğu kullanın. Bu değerleri farklı olan olanlardır:

| Ayar | Değer |
| ------- | ----- |
| **GENEL SANAL MAKİNE** | |
| TEMEL BİLGİLERİ |  |
| Sanal makine adı | Girin *myVmPublic*. |
| AĞ İLETİŞİMİ | |
| Alt ağ | Seçin **ortak (10.0.0.0/24)**. |
| Genel IP adresi | Varsayılanı kabul edin. |
| Ortak gelen bağlantı noktası | Seçin **Seçili bağlantı noktalarına izin**. |
| Gelen bağlantı noktası seçin | Seçin **HTTP** ve **RDP**. |
| YÖNETİM | |
| Tanılama depolama hesabı | Varsayılan değeri bırakın **mynvastorageaccount**. |
| **ÖZEL VM** | |
| TEMEL BİLGİLERİ |  |
| Sanal makine adı | Girin *myVmPrivate*. |
| AĞ İLETİŞİMİ | |
| Alt ağ | Seçin **özel (10.0.1.0/24)**. |
| Genel IP adresi | Varsayılanı kabul edin. |
| Ortak gelen bağlantı noktası | Seçin **Seçili bağlantı noktalarına izin**. |
| Gelen bağlantı noktası seçin | Seçin **HTTP** ve **RDP**. |
| YÖNETİM | |
| Tanılama depolama hesabı | Varsayılan değeri bırakın **mynvastorageaccount**. |

Azure *myVmPublic* VM’yi oluştururken *myVmPrivate* VM’yi oluşturabilirsiniz. Azure, her iki VM'yi oluşturma işlemini tamamlayana kadar adımların kalanını geçmeyin.

## <a name="route-traffic-through-an-nva"></a>Trafiği NVA üzerinden yönlendirme

### <a name="sign-in-to-myvmprivate-over-remote-desktop"></a>Uzak Masaüstü üzerinden myVmPrivate için oturum açın

1. Portalın arama çubuğunda girin *myVmPrivate*.

1. Arama sonuçlarında **myVmPrivate** VM göründüğünde seçin.

1. Seçin **Connect** bir Uzak Masaüstü bağlantısı oluşturmak için *myVmPrivate* VM.

1. İçinde **sanal makineye bağlanma**seçin **RDP dosyasını indir**. Azure, bir Uzak Masaüstü Protokolü oluşturur (*.rdp*) dosya ve bilgisayarınıza yükler.

1. İndirilen açın *.rdp* dosya.

    1. İstendiğinde **Bağlan**’ı seçin.

    1. Kullanıcı adı ve özel sanal makine oluştururken belirttiğiniz parolayı girin.

    1. Seçmeniz gerekebilir **daha fazla seçenek** > **farklı bir hesap kullan**, özel sanal makine kimlik bilgilerini kullanın.

1. **Tamam**’ı seçin.

    İşlemde, oturum açma sırasında bir sertifika uyarısı alabilirsiniz.

1. Seçin **Evet** VM'ye bağlanma.

### <a name="enable-icmp-through-the-windows-firewall"></a>Windows Güvenlik Duvarı üzerinden ICMP'yi etkinleştirin

Daha sonraki bir adımda yönlendirmeyi test etmek için izleme yönlendirme aracı kullanmanız. İzleme yönlendirmesi, Windows Güvenlik Duvarı varsayılan olarak reddeder Internet Denetim İletisi Protokolü (ICMP) kullanır. Windows Güvenlik Duvarı üzerinden ICMP'yi etkinleştirin.

1. Uzak masaüstünde *myVmPrivate*, PowerShell'i açın.

1. Şu komutu girin:

    ```powershell
    New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
    ```

    Bu öğreticide yönlendirmeyi test etmek için izleme yönlendirme kullandığınız. Üretim ortamları için Windows Güvenlik Duvarı üzerinden ICMP'ye izin verilmesi önerilmez.

### <a name="turn-on-ip-forwarding-within-myvmnva"></a>MyVmNva içinde IP iletimini Aç

[Üzerindeki IP iletme etkinleştirilmiş](#turn-on-ip-forwarding) Azure'ı kullanarak sanal makinenin ağ arabirimi için. Sanal makinenin işletim sistemi, ağ trafiğini iletebilmesi de vardır. IP iletme için açma *myVmNva* kullanıcının işletim sistemi şu komutlarla VM.

1. Bir komut isteminden *myVmPrivate* VM, bir Uzak Masaüstü Bağlantısı açın *myVmNva* VM:

    ```cmd
    mstsc /v:myvmnva
    ```

1. Makinesindeki powershell'den *myVmNva*, üzerinde IP iletmeyi etkinleştirmek için şu komutu girin:

    ```powershell
    Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters -Name IpEnableRouter -Value 1
    ```

1. Yeniden *myVmNva* VM. Görev çubuğundan seçin **Başlat düğmesine** > **güç düğmesi**, **diğer (Planlı)** > **devam**.

    Aynı zamanda uzak masaüstü oturumunun bağlantısını keser.

1. Sonra *myVmNva* VM yeniden başlatıldığında, Uzak Masaüstü oturumu oluşturmak *myVmPublic* VM. Bağlantısı devam *myVmPrivate* VM, bir komut istemi açın ve şu komutu çalıştırın:

    ```cmd
    mstsc /v:myVmPublic
    ```
1. Uzak masaüstünde *myVmPublic*, PowerShell'i açın.

1. ICMP, bu komutu girerek Windows Güvenlik Duvarı aracılığıyla etkinleştirin:

    ```powershell
    New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
    ```

## <a name="test-the-routing-of-network-traffic"></a>Ağ trafiğini yönlendirme testi

İlk olarak, gelen ağ trafiği yönlendirmesini şimdi test *myVmPublic* VM *myVmPrivate* VM.

1. Makinesindeki powershell'den *myVmPublic* VM, şu komutu girin:

    ```powershell
    tracert myVmPrivate
    ```

    Yanıt şu örneğe benzer:

    ```powershell
    Tracing route to myVmPrivate.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.1.4]
    over a maximum of 30 hops:

    1    <1 ms     *        1 ms  10.0.2.4
    2     1 ms     1 ms     1 ms  10.0.1.4

    Trace complete.
    ```

    İlk atlamanın için 10.0.2.4 olduğunu görebilirsiniz. NVA'ın özel IP adresi var. İkinci atlama için özel IP adresi olduğu *myVmPrivate* VM: 10.0.1.4. Rota için daha önce eklediğiniz *myRouteTablePublic* yönlendirme tablosuna ve ilişkili *genel* alt ağ. Sonuç olarak, Azure, trafiği NVA aracılığıyla ve doğrudan için gönderilen *özel* alt ağ.

1. *myVmPublic* VM ile uzak masaüstü oturumunu kapatın. *myVmPrivate* VM bağlantınız hala açıktır.

1. Bir komut isteminden *myVmPrivate* VM, şu komutu girin:

    ```cmd
    tracert myVmPublic
    ```

    Gelen ağ trafiğini yönlendirme testleri *myVmPrivate* VM *myVmPublic* VM. Yanıt şu örneğe benzer:

    ```cmd
    Tracing route to myVmPublic.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.0.4]
    over a maximum of 30 hops:

    1     1 ms     1 ms     1 ms  10.0.0.4

    Trace complete.
    ```

    Azure, trafiği yönlendirir doğrudan gördüğünüz *myVmPrivate* VM *myVmPublic* VM. Varsayılan olarak Azure, trafiği doğrudan alt ağlar arasında yönlendirir.

1. *myVmPrivate* VM ile uzak masaüstü oturumunu kapatın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu ve sahip tüm kaynakları silin:

1. Portalın arama çubuğunda girin *myResourceGroup*.

1. Arama sonuçlarında **myResourceGroup** seçeneğini gördüğünüzde bunu seçin.

1. **Kaynak grubunu sil**'i seçin.

1. **KAYNAK GRUBU ADINI YAZIN:** için *myResourceGroup* girin ve **Sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide bir yönlendirme tablosu oluşturup bir alt ağ ile ilişkilendirdiniz. Bir genel alt ağdan özel alt ağa trafiği yönlendiren basit bir NVA oluşturdunuz. Bunun nasıl yapılacağını bildiğiniz, öğesinden farklı önceden yapılandırılmış Nva'lar dağıtabileceğiniz [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking). Bunlar yararlı bulabilirsiniz birçok ağ işlevlerini uygulayın. Yönlendirme hakkında daha fazla bilgi için bkz. [Yönlendirmeye genel bakış](virtual-networks-udr-overview.md) ve [Yönlendirme tablosunu yönetme](manage-route-table.md).

Azure sanal ağ içinde çok sayıda Azure kaynağına dağıtabilmenize karşın, bazı PaaS hizmetlerinin kaynakları bir sanal ağa dağıtamazsınız. Bazı Azure PaaS hizmetlerinin kaynaklarına erişimi kısıtlamak mümkündür. Kısıtlama yalnızca yine de trafiği bir sanal ağ alt ağından olmalıdır. Azure PaaS kaynaklarına erişimi kısıtlama hakkında bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [PaaS kaynaklarına ağ erişimini kısıtlama](tutorial-restrict-network-access-to-resources.md)
