---
title: Azure portalını kullanarak Azure DNS özel bölgesi oluşturma
description: Bu öğreticide Azure DNS'de özel bir DNS bölgesi oluşturacak ve test edeceksiniz. İlk özel DNS bölgesi ve kaydı Azure portalını kullanarak oluşturma ve yönetme için adım adım kılavuzu budur.
services: dns
author: vhorne
ms.service: dns
ms.topic: tutorial
ms.date: 6/15/2019
ms.author: victorh
ms.openlocfilehash: e3772d8f385ed93c96f8aa2f7ee2ded8f46d4e00
ms.sourcegitcommit: 72f1d1210980d2f75e490f879521bc73d76a17e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/14/2019
ms.locfileid: "67147808"
---
# <a name="tutorial-create-an-azure-dns-private-zone-using-the-azure-portal"></a>Öğretici: Azure portalını kullanarak Azure DNS özel bölgesi oluşturma

Bu öğretici ilk özel DNS bölgesi ve kaydı Azure portalını kullanarak oluşturma adımlarında size kılavuzluk eder.

[!INCLUDE [private-dns-public-preview-notice](../../includes/private-dns-public-preview-notice.md)]

DNS bölgesi belirli bir etki alanıyla ilgili DNS kayıtlarını barındırmak için kullanılır. Etki alanınızı Azure DNS'de barındırmaya başlamak için bir DNS bölgesi oluşturmanız gerekir. Ardından bu DNS bölgesinde etki alanınız için tüm DNS kayıtları oluşturulur. Sanal ağınızda özel bir DNS bölgesi yayımlamak için bölge içindeki kaynakları çözümleme izni olan sanal ağların listesini belirtmeniz gerekir.  Bunlar adlandırılır *bağlı* sanal ağları. Otomatik kayıt etkin olduğunda, bir sanal makine oluşturulan her değiştiğinde Azure DNS de bölge kayıtları güncelleştirir, ' IP adresi ya da silinir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * DNS özel bölgesi oluşturma
> * Sanal ağ oluşturma
> * Sanal ağ bağlama
> * Test amaçlı sanal makineleri oluşturma
> * Ek bir DNS kaydı oluşturma
> * Özel bölgeyi test etme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

Tercih ederseniz Bu öğretici kullanarak tamamlayabilirsiniz [Azure PowerShell](private-dns-getstarted-powershell.md) veya [Azure CLI](private-dns-getstarted-cli.md).

## <a name="create-a-dns-private-zone"></a>DNS özel bölgesi oluşturma

Aşağıdaki örnekte adlı bir DNS bölgesi oluşturur **private.contoso.com** adlı bir kaynak grubunda yer **MyAzureResourceGroup**.

Bir DNS bölgesi bir etki alanı için DNS girişleri içerir. Etki alanınızı Azure DNS'de barındırmaya başlamak için bu etki alanı adı için bir DNS bölgesi oluşturun.

![Özel DNS arama bölgeleri](media/private-dns-portal/search-private-dns.png)

1. Portal arama çubuğunda, türü **özel dns bölgelerini** tuşuna basın ve arama metin kutusuna **Enter**.
1. Seçin **özel DNS bölgesi**.
2. Seçin **özel dns bölgesi oluştur**.

1. Üzerinde **özel DNS bölgesi oluştur** sayfasında yazın veya aşağıdaki değerleri seçin:

   - **Kaynak grubu**: Seçin **Yeni Oluştur**, girin *MyAzureResourceGroup*seçip **Tamam**. Kaynak grubu adı, Azure abonelik içinde benzersiz olmalıdır. 
   -  **Ad**: Tür *private.contoso.com* bu örneğin.
1. İçin **kaynak grubu konumu**seçin **Batı Orta ABD**.

1. Seçin **gözden geçir + Oluştur**.

1. **Oluştur**’u seçin.

Bölgenin oluşturulması birkaç dakika sürebilir.

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

1. Portal sayfasının üst sol tarafta, seçin **kaynak Oluştur**, ardından **ağ**, ardından **sanal ağ**.
2. İçin **adı**, türü **myAzureVNet**.
3. İçin **kaynak grubu**seçin **MyAzureResourceGroup**.
4. İçin **konumu**seçin **Batı Orta ABD**.
5. Diğer varsayılan değerleri kabul edin ve seçin **Oluştur**.

## <a name="link-the-virtual-network"></a>Sanal ağ bağlama

Özel DNS bölgesi bir sanal ağa bağlamak için bir sanal ağa bağlantı oluşturun.

![Sanal Ağ Bağlantısı Ekle](media/private-dns-portal/dns-add-virtual-network-link.png)

1. Açık **MyAzureResourceGroup** kaynak grubu ve select **private.contoso.com** özel bölge.
2. Sol bölmeden **sanal ağ bağlantıları**.
3. **Add (Ekle)** seçeneğini belirleyin.
4. Tür **myLink** için **bağlantı adı**.
5. İçin **sanal ağ**seçin **myAzureVNet**.
6. Seçin **otomatik kaydı etkinleştirme** onay kutusu.
7. **Tamam**’ı seçin.

## <a name="create-the-test-virtual-machines"></a>Test amaçlı sanal makineleri oluşturma

Şimdi özel DNS bölgenizi test etmek için iki sanal makine oluşturun:

1. Portal sayfasının üst sol tarafta, seçin **kaynak Oluştur**ve ardından **Windows Server 2016 Datacenter**.
1. Seçin **MyAzureResourceGroup** kaynak grubu için.
1. Tür **myVM01** - sanal makinenin adı.
1. Seçin **Batı Orta ABD** için **bölge**.
1. Tür **azureadmin** için yönetici kullanıcı adı.
2. Tür **Azure12345678** parola ve parolayı onaylayın.

5. İçin **ortak gelen bağlantı noktası**seçin **Seçili bağlantı noktalarına izin**ve ardından **RDP (3389)** için **seçin gelen bağlantı noktalarının**.
10. Sayfa için diğer varsayılan değerleri kabul edin ve ardından **sonraki: Diskleri >** .
11. Varsayılan değerleri kabul **diskleri** sayfasında'a tıklayın **sonraki: Ağ >** .
1. Emin olun **myAzureVNet** sanal ağ için seçili.
1. Sayfa için diğer varsayılan değerleri kabul edin ve ardından **sonraki: Yönetim >** .
2. İçin **önyükleme tanılaması**seçin **kapalı**diğer Varsayılanları kabul edin ve ardından **gözden geçir + Oluştur**.
1. Ayarları gözden geçirin ve ardından **Oluştur**.

Bu adımları yineleyin ve adlı başka bir sanal makine oluşturma **myVM02**.

Her iki sanal makineler, tamamlanması için birkaç dakika sürer.

## <a name="create-an-additional-dns-record"></a>Ek bir DNS kaydı oluşturma

 Aşağıdaki örnek, göreli adı ile bir kayıt oluşturur **db** DNS bölgesinde **private.contoso.com**, kaynak grubundaki **MyAzureResourceGroup**. Kayıt kümesinin tam adıdır **db.private.contoso.com**. Kayıt türü "A", IP adresi ile olan **myVM01**.

1. Açık **MyAzureResourceGroup** kaynak grubu ve select **private.contoso.com** özel bölge.
2. **+ Kayıt kümesi**’ni seçin.
3. İçin **adı**, türü **db**.
4. İçin **IP adresi**, görmek için IP adresini yazın **myVM01**. Bu, otomatik sanal makine başlatıldığında kayıtlı olması gerekir.
5. **Tamam**’ı seçin.

## <a name="test-the-private-zone"></a>Özel bölgeyi test etme

Ad çözümlemesi için test edebilirsiniz artık, **private.contoso.com** özel bölge.

### <a name="configure-vms-to-allow-inbound-icmp"></a>Sanal makineleri gelen ICMP paketlerine izin verecek şekilde yapılandırma

Ad çözümlemesini test etmek için ping komutunu kullanabilirsiniz. Bunun için iki sanal makinedeki güvenlik duvarını da gelen ICMP paketlerine izin verecek şekilde yapılandırmanız gerekir.

1. myVM01 adlı sanal makineye bağlanın, yönetici ayrıcalıklarıyla bir Windows PowerShell penceresi açın.
2. Şu komutu çalıştırın:

   ```powershell
   New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
   ```

myVM02 için yineleyin.

### <a name="ping-the-vms-by-name"></a>Sanal makinelere ada göre ping gönderme

1. myVM02 Windows PowerShell komut isteminden otomatik olarak kaydedilen ana bilgisayar adını kullanarak myVM01 adlı makineye ping gönderin:
   ```
   ping myVM01.private.contoso.com
   ```
   Şuna benzer bir çıkışla karşılaşmanız gerekir:
   ```
   PS C:\> ping myvm01.private.contoso.com

   Pinging myvm01.private.contoso.com [10.2.0.4] with 32 bytes of data:
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time=1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128

   Ping statistics for 10.2.0.4:
       Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
   Approximate round trip times in milli-seconds:
       Minimum = 0ms, Maximum = 1ms, Average = 0ms
   PS C:\>
   ```
2. Şimdi önceden oluşturduğunuz **db** adına ping gönderin:
   ```
   ping db.private.contoso.com
   ```
   Şuna benzer bir çıkışla karşılaşmanız gerekir:
   ```
   PS C:\> ping db.private.contoso.com

   Pinging db.private.contoso.com [10.2.0.4] with 32 bytes of data:
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128

   Ping statistics for 10.2.0.4:
       Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
   Approximate round trip times in milli-seconds:
       Minimum = 0ms, Maximum = 0ms, Average = 0ms
   PS C:\>
   ```

## <a name="delete-all-resources"></a>Tüm kaynakları silme

Artık gerekmediğinde **MyAzureResourceGroup** kaynak grubunu silerek bu öğreticide oluşturulan kaynakları silebilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide özel DNS bölgesi dağıttınız, DNS kaydı oluşturdunuz ve oluşturduğunuz bölgeyi test ettiniz.
Artık özel DNS bölgeleri hakkında daha fazla bilgi edinebilirsiniz.

> [!div class="nextstepaction"]
> [Azure DNS'yi özel etki alanları için kullanma](private-dns-overview.md)
