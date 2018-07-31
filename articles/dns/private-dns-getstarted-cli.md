---
title: Azure CLI kullanarak Azure DNS'de özel bölge oluşturma
description: Bu öğreticide Azure DNS'de özel bir DNS bölgesi oluşturacak ve test edeceksiniz. Bu kılavuzda, Azure CLI kullanarak ilk özel DNS bölgenizi ve kaydınızı oluşturup yönetmeniz için adım adım talimatlar sunulmaktadır.
services: dns
author: vhorne
ms.service: dns
ms.topic: tutorial
ms.date: 7/25/2018
ms.author: victorh
ms.openlocfilehash: 023a1ecb6afc49dd20a14d57558d72a44779dbe9
ms.sourcegitcommit: c2c64fc9c24a1f7bd7c6c91be4ba9d64b1543231
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2018
ms.locfileid: "39257574"
---
# <a name="create-an-azure-dns-private-zone-using-azure-cli"></a>Azure CLI kullanarak Azure DNS'de özel bölge oluşturma

Bu öğreticide, Azure CLI kullanarak ilk özel DNS bölgesi ve kaydınızı oluşturma adımları gösterilmektedir.

[!INCLUDE [private-dns-public-preview-notice](../../includes/private-dns-public-preview-notice.md)]

DNS bölgesi belirli bir etki alanıyla ilgili DNS kayıtlarını barındırmak için kullanılır. Etki alanınızı Azure DNS'de barındırmaya başlamak için bir DNS bölgesi oluşturmanız gerekir. Ardından bu DNS bölgesinde etki alanınız için tüm DNS kayıtları oluşturulur. Sanal ağınızda özel bir DNS bölgesi yayımlamak için bölge içindeki kaynakları çözümleme izni olan sanal ağların listesini belirtmeniz gerekir.  Bunlara *çözümleme sanal ağları* adı verilir. Bir VM oluşturulduğunda, IP’yi değiştirdiğinde veya silindiğinde Azure DNS'te ana bilgisayar adı kayıtlarının tutulacağı bir sanal ağ da belirtebilirsiniz.  Buna *kayıt sanal ağı* adı verilir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * DNS özel bölgesi oluşturma
> * Test amaçlı sanal makineleri oluşturma
> * Ek bir DNS kaydı oluşturma
> * Özel bölgeyi test etme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

Tercih ederseniz, bu öğreticiyi [Azure PowerShell](private-dns-getstarted-powershell.md) kullanarak tamamlayabilirsiniz.


[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-the-resource-group"></a>Kaynak grubunu oluşturma

Öncelikle DNS bölgesini içerecek kaynak grubunu oluşturun: 

```azurecli
az group create --name MyAzureResourceGroup --location "East US"
```

## <a name="create-a-dns-private-zone"></a>DNS özel bölgesi oluşturma

**ZoneType** parametresi için *Özel* değeriyle birlikte `az network dns zone create` komutu kullanılarak bir DNS bölgesi oluşturulur. Aşağıdaki örnek **MyAzureResourceGroup** adlı kaynak grubunda **contoso.local** adlı bir DNS bölgesi oluşturur ve DNS bölgesini **MyAzureVnet** adlı sanal ağda kullanılabilir hale getirir.

**ZoneType** parametresi atılırsa bölge bir genel bölge olarak oluşturulur; bu nedenle bir özel bölge oluşturmanız gerekiyorsa bu gereklidir.

```azurecli
az network vnet create \
  --name myAzureVNet \
  --resource-group MyAzureResourceGroup \
  --location eastus \
  --address-prefix 10.2.0.0/16 \
  --subnet-name backendSubnet \
  --subnet-prefix 10.2.0.0/24

az network dns zone create -g MyAzureResourceGroup \
   -n contoso.local \
  --zone-type Private \
  --registration-vnets myAzureVNet
```

Yalnızca ad çözümlemesi için bir bölge oluşturmak istiyorsanız (otomatik ana bilgisayar adı oluşturma olmadan) *registration-vnets* parametresi yerine *resolution-vnets* parametresini kullanabilirsiniz.

> [!NOTE]
> Bu durumda otomatik olarak oluşturulan ana bilgisayar adı kayıtlarını görmezsiniz. Ancak daha sonra test ederek var olduklarından emin olabilirsiniz.

### <a name="list-dns-private-zones"></a>DNS özel bölgelerini listeleme

DNS bölgelerini numaralandırmak için `az network dns zone list` komutunu kullanın. Yardım için bkz. `az network dns zone list --help`.

Kaynak grubu belirtildiğinde yalnızca kaynak grubu içindeki bölgeler listelenir:

```azurecli
az network dns zone list \
  --resource-group MyAzureResourceGroup
```

Kaynak grubu atıldığında, abonelikteki tüm bölgeler listelenir:

```azurecli
az network dns zone list 
```

## <a name="create-the-test-virtual-machines"></a>Test amaçlı sanal makineleri oluşturma

Şimdi özel DNS bölgenizi test etmek için iki sanal makine oluşturun:

```azurecli
az vm create \
 -n myVM01 \
 --admin-username test-user \
 -g MyAzureResourceGroup \
 -l eastus \
 --subnet backendSubnet \
 --vnet-name myAzureVnet \
 --image win2016datacenter

az vm create \
 -n myVM02 \
 --admin-username test-user \
 -g MyAzureResourceGroup \
 -l eastus \
 --subnet backendSubnet \
 --vnet-name myAzureVnet \
 --image win2016datacenter
```

İşlemin tamamlanması birkaç dakika sürebilir.

## <a name="create-an-additional-dns-record"></a>Ek bir DNS kaydı oluşturma

DNS kaydı oluşturmak için `az network dns record-set [record type] add-record` komutunu kullanın. A kaydı ekleme konusunda yardım almak için bkz. `azure network dns record-set A add-record --help`.

 Aşağıdaki örnekte, **MyAzureResourceGroup** kaynak grubu içindeki **contoso.local** DNS Bölgesinde göreli adı **db** olan bir kaynak oluşturulmaktadır. **db.contoso.local**, kayıt kümesinin tam adıdır. Kayıt türü "A" ve IP adresi "10.2.0.4" olarak belirlenmiştir.

```azurecli
az network dns record-set a add-record \
  -g MyAzureResourceGroup \
  -z contoso.local \
  -n db \
  -a 10.2.0.4
```

### <a name="view-dns-records"></a>DNS kayıtlarını görüntüleme

Bölgenizdeki DNS kayıtlarını listelemek için şu komutu çalıştırın:

```azurecli
az network dns record-set list \
  -g MyAzureResourceGroup \
  -z contoso.local
```
Test amaçlı oluşturduğunuz iki sanal makine için otomatik olarak oluşturulan A kayıtlarını görmeyeceğinizi unutmayın.

## <a name="test-the-private-zone"></a>Özel bölgeyi test etme

Şimdi **contoso.local** özel bölgenizin ad çözümlemesini test edebilirsiniz.

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
   ping myVM01.contoso.local
   ```
   Şuna benzer bir çıkışla karşılaşmanız gerekir:
   ```
   PS C:\> ping myvm01.contoso.local

   Pinging myvm01.contoso.local [10.2.0.4] with 32 bytes of data:
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
   ping db.contoso.local
   ```
   Şuna benzer bir çıkışla karşılaşmanız gerekir:
   ```
   PS C:\> ping db.contoso.local

   Pinging db.contoso.local [10.2.0.4] with 32 bytes of data:
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

```azurecli
az group delete --name MyAzureResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide özel DNS bölgesi dağıttınız, DNS kaydı oluşturdunuz ve oluşturduğunuz bölgeyi test ettiniz.
Artık özel DNS bölgeleri hakkında daha fazla bilgi edinebilirsiniz.

> [!div class="nextstepaction"]
> [Azure DNS'yi özel etki alanları için kullanma](private-dns-overview.md)