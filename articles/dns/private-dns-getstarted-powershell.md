---
title: Azure PowerShell kullanarak Azure DNS özel bölgesi oluşturma
description: Bu makalede, oluşturun ve Azure DNS'te özel DNS bölgesi ve kaydı test. Bu kılavuzda, Azure PowerShell kullanarak ilk özel DNS bölgenizi ve kaydınızı oluşturup yönetmeniz için adım adım talimatlar sunulmaktadır.
services: dns
author: vhorne
ms.service: dns
ms.topic: article
ms.date: 06/14/2019
ms.author: victorh
ms.openlocfilehash: 9d79ed28bd331b723755e1c17233aa82421ad1d7
ms.sourcegitcommit: 72f1d1210980d2f75e490f879521bc73d76a17e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/14/2019
ms.locfileid: "67147887"
---
# <a name="create-an-azure-dns-private-zone-using-azure-powershell"></a>Azure PowerShell kullanarak Azure DNS özel bölgesi oluşturma

[!INCLUDE [private-dns-public-preview-notice](../../includes/private-dns-public-preview-notice.md)]

Bu makalede, Azure PowerShell kullanarak ilk özel DNS bölgesi ve kaydınızı oluşturma adımları gösterilmektedir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

DNS bölgesi belirli bir etki alanıyla ilgili DNS kayıtlarını barındırmak için kullanılır. Etki alanınızı Azure DNS'de barındırmaya başlamak için bir DNS bölgesi oluşturmanız gerekir. Ardından bu DNS bölgesinde etki alanınız için tüm DNS kayıtları oluşturulur. Sanal ağınızda özel bir DNS bölgesi yayımlamak için bölge içindeki kaynakları çözümleme izni olan sanal ağların listesini belirtmeniz gerekir.  Bunlar adlandırılır *bağlı* sanal ağları. Otomatik kayıt etkin olduğunda, bir sanal makine oluşturulan her değiştiğinde Azure DNS de bölge kayıtları güncelleştirir, ' IP adresi ya da silinir.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * DNS özel bölgesi oluşturma
> * Test amaçlı sanal makineleri oluşturma
> * Ek bir DNS kaydı oluşturma
> * Özel bölgeyi test etme

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

Tercih ederseniz, bu yordamı kullanarak tamamlayabilirsiniz [Azure CLI](private-dns-getstarted-cli.md).

## <a name="create-the-resource-group"></a>Kaynak grubunu oluşturma

Öncelikle DNS bölgesini içerecek kaynak grubunu oluşturun: 

```azurepowershell
New-AzResourceGroup -name MyAzureResourceGroup -location "eastus"
```

## <a name="create-a-dns-private-zone"></a>DNS özel bölgesi oluşturma

DNS bölgesi, `New-AzPrivateDnsZone` cmdlet’i kullanılarak oluşturulur.

Aşağıdaki örnekte adlı bir sanal ağ oluşturur **myAzureVNet**. Adlı bir DNS bölgesi oluşturur ardından **private.contoso.com** içinde **MyAzureResourceGroup** kaynak grubu, bağlantılar DNS bölgesine **MyAzureVnet** sanal ağ ve Otomatik kayıt sağlar.

```azurepowershell
$backendSubnet = New-AzVirtualNetworkSubnetConfig -Name backendSubnet -AddressPrefix "10.2.0.0/24"
$vnet = New-AzVirtualNetwork `
  -ResourceGroupName MyAzureResourceGroup `
  -Location eastus `
  -Name myAzureVNet `
  -AddressPrefix 10.2.0.0/16 `
  -Subnet $backendSubnet

$zone = New-AzPrivateDnsZone -Name private.contoso.com -ResourceGroupName MyAzureResourceGroup

$link = New-AzPrivateDnsVirtualNetworkLink -ZoneName private.contoso.com `
  -ResourceGroupName MyAzureResourceGroup -Name "mylink" `
  -VirtualNetworkId $vnet.id -EnableRegistration
```

Ad çözümlemesi (otomatik konak adı kayıt) yalnızca bir alan oluşturmak istiyorsanız, atlayabilirsiniz `-EnableRegistration` parametresi.

### <a name="list-dns-private-zones"></a>DNS özel bölgelerini listeleme

`Get-AzPrivateDnsZone` içinden bölge adını atarak bir kaynak grubundaki tüm bölgeleri numaralandırabilirsiniz. Bu işlem, bölge nesnelerinin bir dizisini döndürür.

```azurepowershell
$zones = Get-AzPrivateDnsZone -ResourceGroupName MyAzureResourceGroup
$zones
```

`Get-AzPrivateDnsZone` içinden hem bölge adını hem de kaynak grubunu atarak, Azure aboneliğindeki tüm bölgeleri numaralandırabilirsiniz.

```azurepowershell
$zones = Get-AzPrivateDnsZone
$zones
```

## <a name="create-the-test-virtual-machines"></a>Test amaçlı sanal makineleri oluşturma

Şimdi özel DNS bölgenizi test etmek için iki sanal makine oluşturun:

```azurepowershell
New-AzVm `
    -ResourceGroupName "myAzureResourceGroup" `
    -Name "myVM01" `
    -Location "East US" `
    -subnetname backendSubnet `
    -VirtualNetworkName "myAzureVnet" `
    -addressprefix 10.2.0.0/24 `
    -OpenPorts 3389

New-AzVm `
    -ResourceGroupName "myAzureResourceGroup" `
    -Name "myVM02" `
    -Location "East US" `
    -subnetname backendSubnet `
    -VirtualNetworkName "myAzureVnet" `
    -addressprefix 10.2.0.0/24 `
    -OpenPorts 3389
```

İşlemin tamamlanması birkaç dakika sürebilir.

## <a name="create-an-additional-dns-record"></a>Ek bir DNS kaydı oluşturma

`New-AzPrivateDnsRecordSet` cmdlet’ini kullanarak kayıt kümeleri oluşturabilirsiniz. Aşağıdaki örnek, göreli adı ile bir kayıt oluşturur **db** DNS bölgesinde **private.contoso.com**, kaynak grubundaki **MyAzureResourceGroup**. Kayıt kümesinin tam adıdır **db.private.contoso.com**. Kayıt türü "A", IP adresi "10.2.0.4" ve TTL 3600 saniyedir.

```azurepowershell
New-AzPrivateDnsRecordSet -Name db -RecordType A -ZoneName private.contoso.com `
   -ResourceGroupName MyAzureResourceGroup -Ttl 3600 `
   -PrivateDnsRecords (New-AzPrivateDnsRecordConfig -IPv4Address "10.2.0.4")
```

### <a name="view-dns-records"></a>DNS kayıtlarını görüntüleme

Bölgenizdeki DNS kayıtlarını listelemek için şu komutu çalıştırın:

```azurepowershell
Get-AzPrivateDnsRecordSet -ZoneName private.contoso.com -ResourceGroupName MyAzureResourceGroup
```

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
   Approximate round trip times in milliseconds:
       Minimum = 0ms, Maximum = 0ms, Average = 0ms
   PS C:\>
   ```

## <a name="delete-all-resources"></a>Tüm kaynakları silme

Artık gerekli değilse silin **MyAzureResourceGroup** bu makalede oluşturduğunuz kaynakları silmek için kaynak grubu.

```azurepowershell
Remove-AzResourceGroup -Name MyAzureResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, dağıtılan bir DNS kaydı oluşturan özel bir DNS bölgesi ve bölge test.
Artık özel DNS bölgeleri hakkında daha fazla bilgi edinebilirsiniz.

* [Azure DNS'yi özel etki alanları için kullanma](private-dns-overview.md)
