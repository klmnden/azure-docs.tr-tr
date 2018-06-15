---
title: Azure CLI 2.0 kullanarak Azure DNS Özel Bölgeleri’ni kullanmaya başlama | Microsoft Docs
description: Azure DNS'te Özel DNS bölgesi ve kaydı oluşturma hakkında bilgi edinin. Bu kılavuzda, Azure CLI 2.0 kullanarak ilk DNS Özel bölgenizi ve kaydınızı oluşturup yönetmeniz için adım adım talimatlar sunulmaktadır.
services: dns
documentationcenter: na
author: KumuD
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: fb0aa0a6-d096-4d6a-b2f6-eda1c64f6182
ms.service: dns
ms.devlang: azurecli
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2018
ms.author: kumud
ms.openlocfilehash: d10f3201adb972468d8de7e66a940232ed19562d
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
ms.locfileid: "30191458"
---
# <a name="get-started-with-azure-dns-private-zones-using-azure-cli-20"></a>Azure CLI 2.0 kullanarak Azure DNS Özel Bölgeleri’ni kullanmaya başlama

> [!div class="op_single_selector"]
> * [PowerShell](private-dns-getstarted-powershell.md)
> * [Azure CLI 2.0](private-dns-getstarted-cli.md)

Bu makale, Windows, Mac ve Linux platformlarında kullanılabilen platformlar arası Azure CLI 2.0'ı kullanarak ilk DNS özel bölgenizi ve kaydınızı oluşturma adımlarında size rehberlik yapacaktır. Ayrıca Azure PowerShell kullanarak aşağıdaki adımları gerçekleştirebilirsiniz.

[!INCLUDE [private-dns-public-preview-notice](../../includes/private-dns-public-preview-notice.md)]

DNS bölgesi belirli bir etki alanıyla ilgili DNS kayıtlarını barındırmak için kullanılır. Etki alanınızı Azure DNS'de barındırmaya başlamak için bir DNS bölgesi oluşturmanız gerekir. Ardından bu DNS bölgesinde etki alanınız için tüm DNS kayıtları oluşturulur. Sanal ağınızda özel bir DNS bölgesi yayımlamak için bölge içindeki kaynakları çözümleme izni olan sanal ağların listesini belirtmeniz gerekir.  Buna 'çözümleme sanal ağları' adı verilir.  Bir VM oluşturulduğunda, IP’yi değiştirdiğinde veya yok edildiğinde Azure DNS'te ana bilgisayar adı kayıtlarının tutulacağı bir sanal ağ da belirtebilirsiniz.  Buna 'kayıt sanal ağı' adı verilir.

Bu yönergelerde, Azure CLI 2.0’ı önceden yükleyip bunda oturum açtığınız ve ayrıca Özel Bölgeleri destekleyen gerekli CLI uzantısını yüklediğiniz varsayılır. Yardım için bkz. [Azure CLI 2.0 ile DNS bölgelerini yönetme](dns-operations-dnszones-cli.md).

## <a name="to-installuse-azure-dns-private-zones-feature-public-preview"></a>Azure DNS Özel Bölgeleri özelliğini yüklemek/kullanmak için (Genel Önizleme)
Azure DNS Özel Bölge özelliği, Azure CLI uzantısı aracılığıyla Genel Önizlemede yayınlanır. "Dns" Azure CLI uzantısını yükleme 

```
az extension add --name dns
``` 

## <a name="create-the-resource-group"></a>Kaynak grubunu oluşturma

DNS bölgesini oluşturmadan önce, DNS Bölgesi’ni içerecek bir kaynak grubu oluşturulur. Aşağıda, komut gösterilmektedir.

```azurecli
az group create --name MyResourceGroup --location "West US"
```

## <a name="create-a-dns-private-zone"></a>DNS özel bölgesi oluşturma

DNS özel bölgesi, `az network dns zone create` komutu kullanılarak oluşturulur. Bu komutla ilgili yardım içeriğini görmek için `az network dns zone create --help` yazın.

Aşağıdaki örnek, *MyResourceGroup* adlı kaynak grubunda *contoso.local* adlı bir DNS özel bölgesi oluşturur ve resolution-vnets parametresini kullanarak DNS bölgesini *MyAzureVnet* adlı sanal ağda kullanılabilir hale getirir (bağlantı oluşturur). Değerleri kendinizinkilerle değiştirerek DNS bölgesini oluşturmak için örneği kullanın.

```azurecli
az network dns zone create -g MyResourceGroup -n contoso.local --zone-type Private --resolution-vnets MyAzureVnet
```

Not: Yukarıdaki örnekte, "MyAzureVnet" adlı sanal ağ, özel bölgeyle aynı Kaynak Grubuna ve Aboneliğe aittir. Farklı bir Kaynak Grubuna veya Aboneliğe ait sanal ağın bağlantısını oluşturmanız gerekiyorsa, resolution-vnets parametresi için yalnızca sanal ağ adı yerine tam Azure Resource Manager kimliğini belirtmeniz gerekir. 

Azure'un bölgede otomatik olarak ana bilgisayar adı kayıtları oluşturmasını istiyorsanız, *resolution-vnets* yerine *registration-vnets* parametresini kullanın.  Kayıt sanal ağlarında çözümleme otomatik olarak etkinleştirilir.

```azurecli
az network dns zone create -g MyResourceGroup -n contoso.local --zone-type Private --registration-vnets MyAzureVnet
```

## <a name="create-a-dns-record"></a>DNS kaydı oluşturma

DNS kaydı oluşturmak için `az network dns record-set [record type] add-record` komutunu kullanın. Örneğin, A kayıtlarına ilişkin yardım için bkz. `azure network dns record-set A add-record --help`.

Aşağıdaki örnekte, "MyResourceGroup" kaynak grubu içindeki "contoso.local" adlı DNS Bölgesinde göreli adı "ip1" olan bir kaynak oluşturulmaktadır. "ip1.contoso.local", kayıt kümesinin tam adıdır. Kayıt türü "A" ve IP adresi "10.0.0.1"dir.

```azurecli
az network dns record-set a add-record -g MyResourceGroup -z contoso.local -n ip1 -a 10.0.0.1
```

Diğer kayıt türleri, birden fazla kayıt içerek kayıt kümeleri, alternatif TTL değerleri ve var olan kayıtların değiştirilmesi hakkında bilgi için bkz. [Azure CLI 2.0 kullanarak DNS kayıtlarını ve kayıt kümelerini yönetme](dns-operations-recordsets-cli.md).

## <a name="view-records"></a>Kayıtları görüntüleme

Bölgenizdeki DNS kayıtlarını listelemek için şu seçenekleri kullanın:

```azurecli
az network dns record-set list -g MyResourceGroup -z contoso.com
```

## <a name="get-a-dns-private-zone"></a>DNS özel bölgesi alma

DNS özel bölgesi almak için `az network dns zone show` komutunu kullanın. Yardım için bkz. `az network dns zone show --help`.

Aşağıdaki örnek, *MyResourceGroup* kaynak grubundan *contoso.local* DNS bölgesini ve bunun ilişkili verilerini döndürür. 

```azurecli
az network dns zone show --resource-group MyResourceGroup --name contoso.local
```

Aşağıdaki örnek, yanıttır.

```json
{
  "etag": "00000002-0000-0000-3d4d-64aa3689d201",
  "id": "/subscriptions/147a22e9-2356-4e56-b3de-1f5842ae4a3b/resourceGroups/MyResourceGroup/providers/Microsoft.Network/dnszones/contoso.local",
  "location": "global",
  "maxNumberOfRecordSets": 5000,
  "name": "contoso.local",
  "nameServers": null,
  "numberOfRecordSets": 1,
  "registrationVirtualNetworks": [],
  "resolutionVirtualNetworks": [
    {
      "additionalProperties": {},
      "id": "/subscriptions/147a22e9-2356-4e56-b3de-1f5842ae4a3b/resourceGroups/MyResourceGroup/providers/Microsoft.Network/virtualNetworks/MyAzureVnet",
      "resourceGroup": "MyResourceGroup"
    }
  ]
  "resourceGroup": "MyResourceGroup",
  "tags": {},
  "type": "Microsoft.Network/dnszones",
  "zoneType": "Private"
}
```

`az network dns zone show` tarafından DNS kayıtlarının döndürülmediğini unutmayın. DNS kayıtlarını listelemek için `az network dns record-set list` komutunu kullanın.


## <a name="list-dns-zones"></a>DNS bölgelerini listeleme

DNS bölgelerini numaralandırmak için `az network dns zone list` komutunu kullanın. Yardım için bkz. `az network dns zone list --help`.

Kaynak grubu belirtildiğinde yalnızca kaynak grubu içindeki bölgeler listelenir:

```azurecli
az network dns zone list --resource-group MyResourceGroup
```

Kaynak grubu atıldığında, abonelikteki tüm bölgeler listelenir:

```azurecli
az network dns zone list 
```

## <a name="update-a-dns-zone"></a>DNS bölgesini güncelleştirme

`az network dns zone update` komutu kullanılarak bir DNS bölgesi kaynağı üzerinde değişiklikler yapılabilir. Yardım için bkz. `az network dns zone update --help`.

Bu komut, bölgedeki DNS kayıt kümelerinin herhangi birini güncelleştirmez (bkz. [DNS kayıtlarını yönetme](dns-operations-recordsets-cli.md)). Yalnızca bölge kaynağının özelliklerini güncelleştirmek için kullanılır. Özel bölgeler için, bir bölgeyle bağlantılı Kayıt veya Çözümleme sanal ağlarını güncelleştirebilirsiniz. 

Aşağıdaki örnekte, bir DNS özel bölgesiyle bağlantılı Çözümleme sanal ağının nasıl güncelleştirileceği gösterilmektedir. Mevcut bağlantılı Çözümleme sanal ağı, belirtilen yeni sanal ağ ile değiştirilir.

```azurecli
az network dns zone update --resource-group MyResourceGroup --name contoso.local --zone-type Private --resolution-vnets MyNewAzureVnet
```

## <a name="delete-a-dns-zone"></a>DNS bölgesini silme

`az network dns zone delete` komutu kullanılarak DNS bölgeleri silinebilir. Yardım için bkz. `az network dns zone delete --help`.

> [!NOTE]
> Bir DNS bölgesi silindiğinde, bölge içindeki tüm DNS kayıtları da silinir. Bu işlem geri alınamaz. DNS bölgesi kullanımdaysa, bölge silindiğinde bölgeyi kullanan hizmetler başarısız olur.
>
>Bölgenin yanlışlıkla silinmesine karşı koruma oluşturmak için bkz. [DNS bölgelerini ve kayıtlarını koruma](dns-protect-zones-recordsets.md).

Bu komut, onay ister. İsteğe bağlı `--yes` anahtarı, bu onay istemini gizler.

Aşağıdaki örnekte, *MyResourceGroup* kaynak grubundan *contoso.local* bölgesinin nasıl silineceği gösterilmektedir.

```azurecli
az network dns zone delete --resource-group myresourcegroup --name contoso.local
```

## <a name="delete-all-resources"></a>Tüm kaynakları silme
 
Bu makalede oluşturulan tüm kaynakları silmek için, aşağıdaki adımları izleyin:

```azurecli
az group delete --name MyResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Azure DNS hakkında daha fazla bilgi için bkz. [Azure DNS'e genel bakış](dns-overview.md).

Azure DNS’te DNS bölgelerini yönetme hakkında daha fazla bilgi için bkz. [Azure CLI 2.0 ile Azure DNS’te DNS bölgelerini yönetme](dns-operations-dnszones-cli.md).

Azure DNS’te DNS kayıtlarını yönetme hakkında daha fazla bilgi için bkz. [Azure CLI 2.0 ile Azure DNS’te DNS kayıtlarını yönetme](dns-operations-recordsets-cli.md).
