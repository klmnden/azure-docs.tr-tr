---
title: Sanal ağ - hızlı başlangıç - Azure portalında oluşturma
titlesuffix: Azure Virtual Network
description: Bu hızlı başlangıçta, Azure portalını kullanarak sanal ağ oluşturmayı öğreneceksiniz. Bir sanal ağ, sanal makineler gibi Azure kaynaklarının sağlar, güvenli bir şekilde birbiriyle ve internet ile iletişim
services: virtual-network
documentationcenter: virtual-network
author: KumudD
tags: azure-resource-manager
Customer intent: I want to create a virtual network so that virtual machines can securely communicate with each other and with the internet.
ms.service: virtual-network
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 07/08/2019
ms.author: kumud
ms.openlocfilehash: bbc40ae358a6ac7f58e01de997728db21c7eb3bc
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67839706"
---
# <a name="quickstart-create-a-virtual-network-using-the-azure-portal"></a>Hızlı Başlangıç: Azure portalını kullanarak bir sanal ağ oluşturma

Bir sanal ağ özel ağınızın azure'da temel yapı taşıdır. Bu, sanal makineleri (güvenli bir şekilde birbiriyle ve internet ile iletişim kurmak için VM) gibi Azure kaynaklarını sağlar. Bu hızlı başlangıçta, Azure portalını kullanarak bir sanal ağ oluşturulacağını öğreneceksiniz. Daha sonra iki sanal makine sanal ağa dağıtabilir, güvenli bir şekilde iki VM arasında iletişim kurmak ve internet'ten Vm'lere bağlanma.


Azure aboneliğiniz yoksa şimdi [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure Portal](https://portal.azure.com) oturum açın.

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

1. Ekranın sol üst tarafında seçin **kaynak Oluştur** > **ağ** > **sanal ağ**.

1. İçinde **sanal ağ oluştur**bu bilgileri seçin veya girin:

    | Ayar | Value |
    | ------- | ----- |
    | Ad | Girin *myVirtualNetwork*. |
    | Adres alanı | Girin *10.1.0.0/16*. |
    | Subscription | Aboneliğinizi seçin.|
    | Resource group | Seçin **Yeni Oluştur**, girin *myResourceGroup*, ardından **Tamam**. |
    | Location | **Doğu ABD**’yi seçin.|
    | Alt ağ - adı | Girin *myVirtualSubnet*. |
    | Alt Ağ - Adres aralığı | Girin *10.1.0.0/24*. |

1. Geri kalan varsayılan ve select bırakın **Oluştur**.

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Sanal ağ üzerinde iki sanal makine oluşturun:

### <a name="create-the-first-vm"></a>Birinci sanal makineyi oluşturma

1. Ekranın sol üst tarafında seçin **kaynak Oluştur** > **işlem** > **Windows Server 2019 Datacenter**.

1. İçinde **temel bilgileri - sanal makine oluşturma**bu bilgileri seçin veya girin:

    | Ayar | Değer |
    | ------- | ----- |
    | **PROJE AYRINTILARI** | |
    | Subscription | Aboneliğinizi seçin. |
    | Resource group | Seçin **myResourceGroup**. Bu önceki bölümde oluşturduğunuz. |
    | **ÖRNEK AYRINTILARI** |  |
    | Sanal makine adı | Girin *myVm1*. |
    | Bölge | **Doğu ABD**’yi seçin. |
    | Kullanılabilirlik seçenekleri | Varsayılan değeri bırakın **gerekli altyapı artıklık**. |
    | Image | Varsayılan değeri bırakın **Windows Server 2019 Datacenter**. |
    | Size | Varsayılan değeri bırakın **standart DS1 v2**. |
    | **YÖNETİCİ HESABI** |  |
    | Kullanıcı Adı | Seçtiğiniz bir kullanıcı adı girin. |
    | istemcisiyle yönetilen bir cihaz için) | Seçtiğiniz bir parolayı girin. Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.|
    | Parolayı onaylayın | Parolayı yeniden girin. |
    | **GELEN BAĞLANTI NOKTASI KURALLARI** |  |
    | Ortak gelen bağlantı noktası | Varsayılan değeri bırakın **hiçbiri**. |
    | **TASARRUF EDİN** |  |
    | Zaten bir Windows lisansınız var mı? | Varsayılan değeri bırakın **Hayır**. |

1. Seçin **sonraki: Diskleri**.

1. İçinde **- sanal makine diskleri oluşturma**, varsayılan değerleri bırakın ve seçin **sonraki: Ağ**.

1. İçinde **ağ - sanal makine oluşturma**, bu bilgileri seçin:

    | Ayar | Value |
    | ------- | ----- |
    | Sanal ağ | Varsayılan değeri bırakın **myVirtualNetwork**. |
    | Subnet | Varsayılan değeri bırakın **myVirtualSubnet (10.1.0.0/24)** . |
    | Genel IP | Varsayılan değeri bırakın **(yeni) myVm-ip**. |
    | Ortak gelen bağlantı noktası | Seçin **Seçili bağlantı noktalarına izin**. |
    | Gelen bağlantı noktası seçin | Seçin **HTTP** ve **RDP**.

1. Seçin **sonraki: Yönetim**.

1. İçinde **Yönetimi - Sanal makine oluşturma**, için **tanılama depolama hesabı**seçin **Yeni Oluştur**.

1. İçinde **depolama hesabı oluşturma**bu bilgileri seçin veya girin:

    | Ayar | Value |
    | ------- | ----- |
    | Ad | Girin *myvmstorageaccount*. Bu adı alınmışsa, benzersiz bir ad oluşturun.|
    | Hesap türü | Varsayılan değeri bırakın **depolama (genel amaçlı v1)** . |
    | Performans | Varsayılan değeri bırakın **standart**. |
    | Çoğaltma | Varsayılan değeri bırakın **yerel olarak yedekli depolama (LRS)** . |

1. **Tamam**’ı seçin

1. **İncele ve oluştur**’u seçin. Adresine yönlendirilirsiniz **gözden geçir + Oluştur** sayfa burada Azure yapılandırmanızı doğrular.

1. Gördüğünüzde **doğrulama başarılı** ileti, select **Oluştur**.

### <a name="create-the-second-vm"></a>İkinci sanal makineyi oluşturma

1. 1\. ve 9 yukarıdaki adımları tamamlayın.

    > [!NOTE]
    > İçinde için adım 2 **sanal makine adı**, girin *myVm2*.
    >
    > 7 ' için adımda **tanılama depolama hesabı**, seçtiğinizden emin olun **myvmstorageaccount**.

1. **İncele ve oluştur**’u seçin. Adresine yönlendirilirsiniz **gözden geçir + Oluştur** sayfası ve Azure yapılandırmanızı doğrular.

1. Gördüğünüzde **doğrulama başarılı** ileti, select **Oluştur**.

## <a name="connect-to-a-vm-from-the-internet"></a>İnternet'ten bir sanal makineye bağlanma

Oluşturduktan sonra *myVm1*, internet'e bağlanın.

1. Portalın arama çubuğunda girin *myVm1*.

1. **Bağlan** düğmesini seçin.

    ![Sanal makineye bağlanma](./media/quick-create-portal/connect-to-virtual-machine.png)

    Seçtikten sonra **Connect** düğmesi **sanal makineye bağlanma** açılır.

1. Seçin **RDP dosyasını indir**. Azure, bir Uzak Masaüstü Protokolü oluşturur ( *.rdp*) dosya ve bilgisayarınıza yükler.

1. İndirilen açın *.rdp* dosya.

    1. İstendiğinde **Bağlan**’ı seçin.

    1. Kullanıcı adı ve sanal makine oluştururken belirttiğiniz parolayı girin.

        > [!NOTE]
        > Seçmek için gerek duyabileceğiniz **daha fazla seçenek** > **farklı bir hesap kullan**sanal Makineyi oluştururken girdiğiniz kimlik bilgilerini belirtmek için.

1. **Tamam**’ı seçin.

1. İşlemde, oturum açma sırasında bir sertifika uyarısı alabilirsiniz. Bir sertifika uyarısı alırsanız seçin **Evet** veya **devam**.

1. VM masaüstüne göründükten sonra yerel masaüstüne geri dönmek için simge durumuna küçültün.

## <a name="communicate-between-vms"></a>Sanal makineler arasında iletişim

1. Uzak masaüstünde *myVm1*, PowerShell'i açın.

1. `ping myVm2` yazın.

    Şuna benzer bir ileti alırsınız:

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

    Bu komut ICMP olanak sağlar. Windows Güvenlik Duvarı üzerinden gelen:

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

    Den yanıt alırsınız *myVm1*, üzerinde Windows Güvenlik Duvarı üzerinden ICMP'ye izin *myVm1* adım 3'te VM.

1. *myVm2* ile uzak masaüstü bağlantısını kapatın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bitirdiğinizde sanal ağı ve Vm'leri kullanarak kaynak grubunu ve içerdiği tüm kaynakları silin:

1. Girin *myResourceGroup* içinde **arama** kutusunu seçin ve portal üst kısmındaki **myResourceGroup** Arama sonuçlarından.

1. **Kaynak grubunu sil**'i seçin.

1. Girin *myResourceGroup* için **kaynak grubu adını yazın** seçip **Sil**.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, varsayılan bir sanal ağ ve iki VM oluşturdunuz. İnternet'ten bir sanal makineye bağladınız ve iki VM arasında güvenli bir şekilde iletilir. Sanal ağ ayarları hakkında daha fazla bilgi için [Sanal ağı yönetme](manage-virtual-network.md) başlıklı konuya bakın.

Varsayılan olarak, Azure sanal makineler arasında Kısıtlanmamış güvenli iletişim sağlar. Buna karşılık, yalnızca gelen Uzak Masaüstü bağlantıları için Windows Vm'leri internet'ten izin verir. Farklı türde bir VM ağ iletişimini yapılandırma hakkında daha fazla bilgi edinmek için Git [ağ trafiğini filtreleme](tutorial-filter-network-traffic.md) öğretici.
