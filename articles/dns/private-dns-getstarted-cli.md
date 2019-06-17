---
title: Azure CLI kullanarak Azure DNS'de özel bölge oluşturma
description: Bu yordamda oluşturun ve Azure DNS'te özel DNS bölgesi ve kaydı test. Bu kılavuzda, Azure CLI kullanarak ilk özel DNS bölgenizi ve kaydınızı oluşturup yönetmeniz için adım adım talimatlar sunulmaktadır.
services: dns
author: vhorne
ms.service: dns
ms.topic: article
ms.date: 6/13/2019
ms.author: victorh
ms.openlocfilehash: d882a9c40efc5e9bcb1a5e1c02f1ac73970d57db
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67076422"
---
# <a name="create-an-azure-dns-private-zone-using-the-azure-cli"></a>Azure CLI kullanarak Azure DNS'de özel bölge oluşturma

[!INCLUDE [private-dns-public-preview-notice](../../includes/private-dns-public-preview-notice.md)]

Bu yordamı ilk özel DNS bölgesi ve Azure CLI kullanarak kayıt oluşturma adımlarında size kılavuzluk eder.

DNS bölgesi belirli bir etki alanıyla ilgili DNS kayıtlarını barındırmak için kullanılır. Etki alanınızı Azure DNS'de barındırmaya başlamak için bir DNS bölgesi oluşturmanız gerekir. Ardından bu DNS bölgesinde etki alanınız için tüm DNS kayıtları oluşturulur. Sanal ağınızda özel bir DNS bölgesi yayımlamak için bölge içindeki kaynakları çözümleme izni olan sanal ağların listesini belirtmeniz gerekir.  Bunlar adlandırılır *bağlı* sanal ağları. Otomatik kayıt etkin olduğunda, bir sanal makine oluşturulan her değiştiğinde Azure DNS de bölge kayıtları güncelleştirir, ' IP adresi ya da silinir.

Bu yordamda, şunların nasıl yapılır:

> [!div class="checklist"]
> * DNS özel bölgesi oluşturma
> * Test amaçlı sanal makineleri oluşturma
> * Ek bir DNS kaydı oluşturma
> * Özel bölgeyi test etme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

Tercih ederseniz, bu yordamı kullanarak tamamlayabilirsiniz [Azure PowerShell](private-dns-getstarted-powershell.md).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-the-resource-group"></a>Kaynak grubunu oluşturma

Öncelikle DNS bölgesini içerecek kaynak grubunu oluşturun: 

```azurecli
az group create --name MyAzureResourceGroup --location "East US"
```

## <a name="create-a-dns-private-zone"></a>DNS özel bölgesi oluşturma

Aşağıdaki örnekte adlı bir sanal ağ oluşturur **myAzureVNet**. Adlı bir DNS bölgesi oluşturur ardından **private.contoso.com** içinde **MyAzureResourceGroup** kaynak grubu, bağlantılar DNS bölgesine **MyAzureVnet** sanal ağ ve Otomatik kayıt sağlar.

```azurecli
az network vnet create \
  --name myAzureVNet \
  --resource-group MyAzureResourceGroup \
  --location eastus \
  --address-prefix 10.2.0.0/16 \
  --subnet-name backendSubnet \
  --subnet-prefixes 10.2.0.0/24

az network private-dns zone create -g MyAzureResourceGroup \
   -n private.contoso.com

az network private-dns link vnet create -g MyAzureResourceGroup -n MyDNSLink \
   -z private.contoso.com -v myAzureVNet -e true
```

Ad çözümlemesi (otomatik konak adı kayıt) yalnızca bir alan oluşturmak istiyorsanız, kullanabileceğinizi `-e false` parametresi.

### <a name="list-dns-private-zones"></a>DNS özel bölgelerini listeleme

DNS bölgelerini numaralandırmak için `az network private-dns zone list` komutunu kullanın. Yardım için bkz. `az network dns zone list --help`.

Kaynak grubu belirtildiğinde yalnızca kaynak grubu içindeki bölgeler listelenir:

```azurecli
az network private-dns zone list \
  -g MyAzureResourceGroup
```

Kaynak grubu atıldığında, abonelikteki tüm bölgeler listelenir:

```azurecli
az network private-dns zone list 
```

## <a name="create-the-test-virtual-machines"></a>Test amaçlı sanal makineleri oluşturma

Şimdi özel DNS bölgenizi test etmek için iki sanal makine oluşturun:

```azurecli
az vm create \
 -n myVM01 \
 --admin-username AzureAdmin \
 -g MyAzureResourceGroup \
 -l eastus \
 --subnet backendSubnet \
 --vnet-name myAzureVnet \
 --nsg NSG01 \
 --nsg-rule RDP \
 --image win2016datacenter

az vm create \
 -n myVM02 \
 --admin-username AzureAdmin \
 -g MyAzureResourceGroup \
 -l eastus \
 --subnet backendSubnet \
 --vnet-name myAzureVnet \
 --nsg NSG01 \
 --nsg-rule RDP \
 --image win2016datacenter
```

İşlemin tamamlanması birkaç dakika sürebilir.

## <a name="create-an-additional-dns-record"></a>Ek bir DNS kaydı oluşturma

DNS kaydı oluşturmak için `az network private-dns record-set [record type] add-record` komutunu kullanın. A kaydı ekleme konusunda yardım almak için bkz. `az network private-dns record-set A add-record --help`.

 Aşağıdaki örnek, göreli adı ile bir kayıt oluşturur **db** DNS bölgesinde **private.contoso.com**, kaynak grubundaki **MyAzureResourceGroup**. Kayıt kümesinin tam adıdır **db.private.contoso.com**. Kayıt türü "A" ve IP adresi "10.2.0.4" olarak belirlenmiştir.

```azurecli
az network private-dns record-set a add-record \
  -g MyAzureResourceGroup \
  -z private.contoso.com \
  -n db \
  -a 10.2.0.4
```

### <a name="view-dns-records"></a>DNS kayıtlarını görüntüleme

Bölgenizdeki DNS kayıtlarını listelemek için şu komutu çalıştırın:

```azurecli
az network private-dns record-set list \
  -g MyAzureResourceGroup \
  -z private.contoso.com
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
   Approximate round trip times in milli-seconds:
       Minimum = 0ms, Maximum = 0ms, Average = 0ms
   PS C:\>
   ```

## <a name="delete-all-resources"></a>Tüm kaynakları silme

Artık gerekli değilse silin **MyAzureResourceGroup** bu yordamda oluşturduğunuz kaynakları silmek için kaynak grubu.

```azurecli
az group delete --name MyAzureResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu yordamda dağıtılmış bir DNS kaydı oluşturan özel bir DNS bölgesi ve bölge test.
Artık özel DNS bölgeleri hakkında daha fazla bilgi edinebilirsiniz.

> [!div class="nextstepaction"]
> [Azure DNS'yi özel etki alanları için kullanma](private-dns-overview.md)
