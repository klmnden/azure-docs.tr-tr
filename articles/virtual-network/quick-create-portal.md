---
title: Sanal ağ - hızlı başlangıç - Azure portalında oluşturma
titlesuffix: Azure Virtual Network
description: Bu hızlı başlangıçta, Azure portalını kullanarak sanal ağ oluşturmayı öğreneceksiniz. Bir sanal ağ, sanal makineler gibi Azure kaynaklarının sağlar, birbiriyle ve internet ile özel olarak iletişim.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
tags: azure-resource-manager
Customer intent: I want to create a virtual network so that virtual machines can communicate with privately with each other and with the internet.
ms.service: virtual-network
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 11/30/2018
ms.author: kumud
ms.openlocfilehash: 346299dff8354bfca56a1f348c8f66e90da89632
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60391416"
---
# <a name="quickstart-create-a-virtual-network-using-the-azure-portal"></a>Hızlı Başlangıç: Azure portalını kullanarak bir sanal ağ oluşturma

Sanal ağ, sanal makineleri (özel olarak birbiriyle ve internet ile iletişim kurmak için VM) gibi Azure kaynaklarını sağlar. Bu hızlı başlangıçta, sanal ağ oluşturmayı öğreneceksiniz. Bir sanal ağ oluşturduktan sonra, sanal ağa iki sanal makine dağıtacaksınız. Ardından Vm'lere internet'ten bağlanabilir ve iki VM arasında özel olarak iletişim kurar.

Azure aboneliğiniz yoksa şimdi [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure Portal](https://portal.azure.com) oturum açın.

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

1. Ekranın sol üst tarafında seçin **kaynak Oluştur** > **ağ** > **sanal ağ**.

1. İçinde **sanal ağ oluştur**bu bilgileri seçin veya girin:

    | Ayar | Değer |
    | ------- | ----- |
    | Ad | Girin *myVirtualNetwork*. |
    | Adres alanı | Girin *10.1.0.0/16*. |
    | Abonelik | Aboneliğinizi seçin.|
    | Kaynak grubu | Seçin **Yeni Oluştur**, girin *myResourceGroup*, ardından **Tamam**. |
    | Location | **Doğu ABD**’yi seçin.|
    | Alt ağ - adı | Girin *myVirtualSubnet*. |
    | Alt Ağ - Adres aralığı | Girin *10.1.0.0/24*. |

1. Geri kalan seçin ve varsayılan değerleri bırakın **Oluştur**.

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Sanal ağ üzerinde iki sanal makine oluşturun:

### <a name="create-the-first-vm"></a>Birinci sanal makineyi oluşturma

1. Ekranın sol üst tarafında seçin **kaynak Oluştur** > **işlem** > **Windows Server 2016 Datacenter**.

1. İçinde **temel bilgileri - sanal makine oluşturma**bu bilgileri seçin veya girin:

    | Ayar | Değer |
    | ------- | ----- |
    | **PROJE AYRINTILARI** | |
    | Abonelik | Aboneliğinizi seçin. |
    | Kaynak grubu | Seçin **MyResourceGroup**. Son bölümde oluşturduğunuz. |
    | **ÖRNEK AYRINTILARI** |  |
    | Sanal makine adı | Girin *myVm1*. |
    | Bölge | **Doğu ABD**’yi seçin. |
    | Kullanılabilirlik seçenekleri | Varsayılan değeri bırakın **gerekli altyapı artıklık**. |
    | Image | Varsayılan değeri bırakın **Windows Server 2016 Datacenter**. |
    | Boyut | Varsayılan değeri bırakın **standart DS1 v2**. |
    | **YÖNETİCİ HESABI** |  |
    | Kullanıcı adı | Seçtiğiniz bir kullanıcı adını girin. |
    | Parola | Seçtiğiniz bir parolayı girin. Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.|
    | Parolayı Onayla | Parolayı yeniden girin. |
    | **GELEN BAĞLANTI NOKTASI KURALLARI** |  |
    | Ortak gelen bağlantı noktası | Varsayılan değeri bırakın **hiçbiri**. |
    | **TASARRUF EDİN** |  |
    | Zaten bir Windows lisansınız var mı? | Varsayılan değeri bırakın **Hayır**. |

1. Seçin **sonraki: Diskleri**.

1. İçinde **- sanal makine diskleri oluşturma**, varsayılan değerleri bırakın ve seçin **sonraki: Ağ**.

1. İçinde **ağ - sanal makine oluşturma**, bu bilgileri seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | Sanal ağ | Varsayılan değeri bırakın **myVirtualNetwork**. |
    | Alt ağ | Varsayılan değeri bırakın **myVirtualSubnet (10.1.0.0/24)**. |
    | Genel IP | Varsayılan değeri bırakın **(yeni) myVm-ip**. |
    | Ağ güvenlik bağlantı noktaları | Seçin **Seçili bağlantı noktalarına izin**. |
    | Gelen bağlantı noktası seçin | Seçin **HTTP** ve **RDP**.

1. Seçin **sonraki: Yönetim**.

1. İçinde **Yönetimi - Sanal makine oluşturma**, için **tanılama depolama hesabı**seçin **Yeni Oluştur**.

1. İçinde **depolama hesabı oluşturma**bu bilgileri seçin veya girin:

    | Ayar | Değer |
    | ------- | ----- |
    | Ad | Girin *myvmstorageaccount*. |
    | Hesap türü | Varsayılan değeri bırakın **depolama (genel amaçlı v1)**. |
    | Performans | Varsayılan değeri bırakın **standart**. |
    | Çoğaltma | Varsayılan değeri bırakın **yerel olarak yedekli depolama (LRS)**. |

1. **Tamam**’ı seçin

1. **İncele ve oluştur**’u seçin. Adresine yönlendirilirsiniz **gözden geçir + Oluştur** sayfası ve Azure yapılandırmanızı doğrular.

1. Gördüğünüzde, **doğrulama başarılı**seçin **Oluştur**.

### <a name="create-the-second-vm"></a>İkinci sanal makineyi oluşturma

1. 1. ve 9 yukarıdaki adımları tamamlayın.

    > [!NOTE]
    > İçinde için adım 2 **sanal makine adı**, girin *myVm2*.
    >
    > 7 ' için adımda **tanılama depolama hesabı**, seçtiğinizden emin olun **myvmstorageaccount**.

1. **İncele ve oluştur**’u seçin. Adresine yönlendirilirsiniz **gözden geçir + Oluştur** sayfası ve Azure yapılandırmanızı doğrular.

1. Gördüğünüzde, **doğrulama başarılı**seçin **Oluştur**.

## <a name="connect-to-a-vm-from-the-internet"></a>İnternet'ten bir sanal makineye bağlanma

Oluşturduktan sonra *myVm1*, internet üzerinden ona bağlanabilirsiniz.

1. Portalın arama çubuğunda girin *myVm1*.

1. **Bağlan** düğmesini seçin.

    ![Sanal makineye bağlanma](./media/quick-create-portal/connect-to-virtual-machine.png)

    Seçtikten sonra **Connect** düğmesi **sanal makineye bağlanma** açılır.

1. Seçin **RDP dosyasını indir**. Azure, bir Uzak Masaüstü Protokolü oluşturur (*.rdp*) dosya ve bilgisayarınıza yükler.

1. İndirilen açın *.rdp* dosya.

    1. İstendiğinde **Bağlan**’ı seçin.

    1. Sanal makine oluştururken belirttiğiniz kullanıcı adını ve parolayı girin.

        > [!NOTE]
        > Seçmek için gerek duyabileceğiniz **daha fazla seçenek** > **farklı bir hesap kullan**sanal Makineyi oluştururken girdiğiniz kimlik bilgilerini belirtmek için.

1. **Tamam**’ı seçin.

1. İşlemde, oturum açma sırasında bir sertifika uyarısı alabilirsiniz. Bir sertifika uyarısı alırsanız seçin **Evet** veya **devam**.

1. VM masaüstüne göründükten sonra yerel masaüstüne geri dönmek için simge durumuna küçültün.

## <a name="communicate-between-vms"></a>Sanal makineler arasında iletişim

1. Uzak masaüstünde *myVm1*, PowerShell'i açın.

1. `ping myVm2` yazın.

    Bu ileti gibi geri alırsınız:

    ```powershell
    Pinging myVm2.0v0zze1s0uiedpvtxz5z0r0cxg.bx.internal.clouda
    Request timed out.
    Request timed out.
    Request timed out.
    Request timed out.

    Ping statistics for 10.1.0.5:
    Packets: Sent = 4, Received = 0, Lost = 4 (100% loss),
    ```

    `ping` Başarısız olur, çünkü `ping` Internet Denetim İletisi Protokolü (ICMP) kullanır. Varsayılan olarak, Windows Güvenlik Duvarı üzerinden ICMP'ye izin verilmiyor.

1. İzin vermek için *myVm2* ping *myVm1* daha sonraki bir adımda bu komutu girin:

    ```powershell
    New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
    ```

    Komut ICMP'ye izin veriyor Windows Güvenlik Duvarı üzerinden gelen:

1. *myVm1* ile uzak masaüstü bağlantısını kapatın.

1. [İnternet'ten bir sanal makineye bağlanma](#connect-to-a-vm-from-the-internet) bölümündeki adımları tekrar tamamlayın, ancak *myVm2*’ye bağlanın.

1. Bir komut isteminden `ping myvm1` komutunu girin.

    Bu ileti gibi geri alırsınız:

    ```powershell
    Pinging myVm1.0v0zze1s0uiedpvtxz5z0r0cxg.bx.internal.cloudapp.net [10.1.0.4] with 32 bytes of data:
    Reply from 10.1.0.4: bytes=32 time=1ms TTL=128
    Reply from 10.1.0.4: bytes=32 time<1ms TTL=128
    Reply from 10.1.0.4: bytes=32 time<1ms TTL=128
    Reply from 10.1.0.4: bytes=32 time<1ms TTL=128

    Ping statistics for 10.1.0.4:
        Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
    Approximate round trip times in milli-seconds:
        Minimum = 0ms, Maximum = 1ms, Average = 0ms
    ```

    Önceki bir adımda *myVm1* sanal makinesinde Windows güvenlik duvarı üzerinden ICMP’ye izin verdiğinizden, *myVm1*’den yanıt alırsınız.

1. *myVm2* ile uzak masaüstü bağlantısını kapatın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Sanal ağ ve sanal makineleri ile işiniz bittiğinde, kaynak grubunu ve içerdiği tüm kaynakları silin:

1. Portalın üst kısmındaki **Ara** kutusuna *myResourceGroup* değerini girin.

1. Arama sonuçlarında **myResourceGroup** seçeneğini gördüğünüzde bunu seçin.

1. **Kaynak grubunu sil**'i seçin.

1. Girin *myResourceGroup* için **kaynak grubu adını yazın** seçip **Sil**.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, varsayılan bir sanal ağ ve iki sanal makine oluşturdunuz. İnternet'ten bir sanal makineye bağladınız ve iki VM arasında özel olarak iletilir. Sanal ağ ayarları hakkında daha fazla bilgi için [Sanal ağı yönetme](manage-virtual-network.md) başlıklı konuya bakın.

Varsayılan olarak, Azure sanal makineler arasında Kısıtlanmamış özel iletişime olanak sağlar. Buna karşılık, yalnızca gelen Uzak Masaüstü bağlantıları için Windows Vm'leri internet'ten izin verir. Farklı türde bir VM ağ iletişimini yapılandırma hakkında daha fazla bilgi edinmek için Git [ağ trafiğini filtreleme](tutorial-filter-network-traffic.md) öğretici.