---
title: Azure DNS'de - Azure CLI DNS bölgelerini yönetme | Microsoft Docs
description: Azure CLI kullanarak DNS bölgelerini yönetebilirsiniz. Bu makalede, güncelleştirme, silme ve Azure DNS DNS bölgeleri oluşturma gösterilmektedir.
services: dns
documentationcenter: na
author: vhorne
manager: timlt
ms.assetid: 8ab63bc4-5135-4ed8-8c0b-5f0712b9afed
ms.service: dns
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/27/2017
ms.author: victorh
ms.openlocfilehash: df741b34e1268c547723af87401760197d395780
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61293838"
---
# <a name="how-to-manage-dns-zones-in-azure-dns-using-the-azure-cli"></a>Azure CLI ile Azure DNS'te DNS bölgelerini yönetme

> [!div class="op_single_selector"]
> * [Portal](dns-operations-dnszones-portal.md)
> * [PowerShell](dns-operations-dnszones.md)
> * [Azure CLI](dns-operations-dnszones-cli.md)


Bu kılavuz, Windows, Mac ve Linux için kullanılabildiği platformlar arası Azure CLI kullanarak DNS bölgelerini yönetme işlemi gösterilmektedir. Kullanarak DNS bölgelerini de yönetebilirsiniz [Azure PowerShell](dns-operations-dnszones.md) veya Azure portalında.

Bu kılavuz, özellikle Genel DNS bölgelerini ile ilgilidir. Azure CLI kullanarak Azure DNS özel bölgelerini yönetme hakkında daha fazla bilgi için bkz: [Azure CLI kullanarak Azure DNS özel bölgelerini kullanmaya başlama](private-dns-getstarted-cli.md).

## <a name="introduction"></a>Giriş

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

## <a name="set-up-azure-cli-for-azure-dns"></a>Azure DNS için Azure CLI'yi ayarlama

### <a name="before-you-begin"></a>Başlamadan önce

Yapılandırmanıza başlamadan önce aşağıdaki öğelerin bulunduğunu doğrulayın.

* Azure aboneliği. Henüz Azure aboneliğiniz yoksa [MSDN abonelik avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.

* Windows, Linux veya Mac için Azure CLI'nin son sürümünü yüklemeniz gerekir. Daha fazla bilgi için bkz. [Azure CLI'yı yükleme](https://docs.microsoft.com/cli/azure/install-az-cli2).

### <a name="sign-in-to-your-azure-account"></a>Azure hesabınızda oturum açma

Bir konsol penceresi açın ve kimlik bilgilerinizi girerek kimliğinizi doğrulayın. Daha fazla bilgi için bkz. [Azure CLI üzerinden Azure’da oturum açma](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest)

```
az login
```

### <a name="select-the-subscription"></a>Aboneliği seçme

Hesapla ilişkili abonelikleri kontrol edin.

```
az account list
```

Hangi Azure aboneliğinizin kullanılacağını seçin.

```azurecli
az account set --subscription "subscription name"
```

### <a name="optional-to-installuse-azure-dns-private-zones-feature-public-preview"></a>İsteğe bağlı: Azure DNS Özel Bölgeleri özelliğini yüklemek/kullanmak için (Genel Önizleme)
Azure DNS Özel Bölge özelliği, Azure CLI uzantısı aracılığıyla Genel Önizlemede yayınlanır. "Dns" Azure CLI uzantısını yükleme 
```
az extension add --name dns
``` 

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure Resource Manager, tüm kaynak gruplarının bir konum belirtmesini gerektirir. Bu, kaynak grubunda kaynaklar için varsayılan konum olarak kullanılır. Ancak tüm DNS kaynakları bölgesel değil de global olduğundan, kaynak grubu konumu seçiminin Azure DNS üzerinde hiçbir etkisi yoktur.

Var olan bir kaynak grubunu kullanıyorsanız bu adımı atlayabilirsiniz.

```azurecli
az group create --name myresourcegroup --location "West US"
```

## <a name="getting-help"></a>Yardım alma

Azure DNS ile ilgili tüm Azure CLI komutları ile başlayan `az network dns`. Her komutu kullanmak için Yardım kullanılabilir `--help` seçeneği (kısa form `-h`).  Örneğin:

```azurecli
az network dns --help
az network dns zone --help
az network dns zone create --help
```

## <a name="create-a-dns-zone"></a>DNS bölgesi oluşturma

DNS bölgesi, `az network dns zone create` komutu kullanılarak oluşturulur. Yardım için bkz. `az network dns zone create -h`.

Aşağıdaki örnekte adlı bir DNS bölgesi oluşturur *contoso.com* adlı kaynak grubunda *MyResourceGroup*:

```azurecli
az network dns zone create --resource-group MyResourceGroup --name contoso.com
```

### <a name="to-create-a-dns-zone-with-tags"></a>Etiketler ile bir DNS bölgesi oluşturma

Aşağıdaki örnek, iki DNS bölgesi oluşturma işlemi gösterilmektedir [Azure Resource Manager etiketleri](dns-zones-records.md#tags), *project = demo* ve *env = test*, kullanarak `--tags` parametresi ( kısa form `-t`):

```azurecli
az network dns zone create --resource-group MyResourceGroup --name contoso.com --tags "project=demo" "env=test"
```

## <a name="get-a-dns-zone"></a>Bir DNS bölgesi alma

Bir DNS bölgesi almak için kullanın `az network dns zone show`. Yardım için bkz. `az network dns zone show --help`.

Aşağıdaki örnek, DNS bölgesini döndürür *contoso.com* ve ilişkili verileri kaynak grubundan *MyResourceGroup*. 

```azurecli
az network dns zone show --resource-group myresourcegroup --name contoso.com
```

Aşağıdaki örnek, yanıttır.

```json
{
  "etag": "00000002-0000-0000-3d4d-64aa3689d201",
  "id": "/subscriptions/147a22e9-2356-4e56-b3de-1f5842ae4a3b/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com",
  "location": "global",
  "maxNumberOfRecordSets": 5000,
  "name": "contoso.com",
  "nameServers": [
    "ns1-04.azure-dns.com.",
    "ns2-04.azure-dns.net.",
    "ns3-04.azure-dns.org.",
    "ns4-04.azure-dns.info."
  ],
  "numberOfRecordSets": 4,
  "resourceGroup": "myresourcegroup",
  "tags": {},
  "type": "Microsoft.Network/dnszones"
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

Bu komut, bölgedeki DNS kayıt kümelerinin herhangi birini güncelleştirmez (bkz. [DNS kayıtlarını yönetme](dns-operations-recordsets-cli.md)). Yalnızca bölge kaynağının özelliklerini güncelleştirmek için kullanılır. Bu özellikler şu anda sınırlı [Azure Resource Manager 'etiketleri'](dns-zones-records.md#tags) bölge kaynağı için.

Aşağıdaki örnek, bir DNS bölgesi etiketleri güncelleştirmek gösterilmektedir. Mevcut olan etiketlerin belirtilen değerle değiştirilir.

```azurecli
az network dns zone update --resource-group myresourcegroup --name contoso.com --set tags.team=support
```

## <a name="delete-a-dns-zone"></a>DNS bölgesini silme

`az network dns zone delete` komutu kullanılarak DNS bölgeleri silinebilir. Yardım için bkz. `az network dns zone delete --help`.

> [!NOTE]
> Bir DNS bölgesi silindiğinde, bölge içindeki tüm DNS kayıtları da silinir. Bu işlem geri alınamaz. DNS bölgesi kullanımdaysa, bölge silindiğinde bölgeyi kullanan hizmetler başarısız olur.
>
>Bölgenin yanlışlıkla silinmesine karşı koruma oluşturmak için bkz. [DNS bölgelerini ve kayıtlarını koruma](dns-protect-zones-recordsets.md).

Bu komut, onay ister. İsteğe bağlı `--yes` anahtarı, bu onay istemini gizler.

Aşağıdaki örnek bölgesinin nasıl silineceği gösterilir *contoso.com* kaynak grubundan *MyResourceGroup*.

```azurecli
az network dns zone delete --resource-group myresourcegroup --name contoso.com
```

## <a name="next-steps"></a>Sonraki adımlar

Bilgi edinmek için nasıl [kayıt kümeleri ve kayıtları yönetmek](dns-getstarted-create-recordset-cli.md) DNS bölgenizdeki.

Bilgi edinmek için nasıl [etki alanınızı Azure DNS'ye devretme](dns-domain-delegation.md).

