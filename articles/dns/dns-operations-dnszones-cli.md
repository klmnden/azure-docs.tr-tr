---
title: "Azure DNS'de - Azure CLI 2.0 DNS bölgelerini yönetme | Microsoft Docs"
description: "DNS bölgelerini Azure CLI 2.0 kullanarak yönetebilirsiniz. Bu makalede, güncelleştirme, silme ve Azure DNS üzerinde DNS bölgeleri oluşturma gösterilmektedir."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 8ab63bc4-5135-4ed8-8c0b-5f0712b9afed
ms.service: dns
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/27/2017
ms.author: gwallace
ms.openlocfilehash: 988f6ff5158eb754cbedeb58a45fcc31f0026f03
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2017
---
# <a name="how-to-manage-dns-zones-in-azure-dns-using-the-azure-cli-20"></a>Azure CLI 2.0 kullanan Azure DNS'de DNS bölgelerini yönetme

> [!div class="op_single_selector"]
> * [Portal](dns-operations-dnszones-portal.md)
> * [PowerShell](dns-operations-dnszones.md)
> * [Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md)
> * [Azure CLI 2.0](dns-operations-dnszones-cli.md)


Bu kılavuz, Windows, Mac ve Linux için kullanılabilir olduğu platformlar arası Azure CLI kullanarak DNS bölgelerini yönetme gösterir. DNS bölgelerini kullanarak da yönetebilirsiniz [Azure PowerShell](dns-operations-dnszones.md) ya da Azure portal.

## <a name="cli-versions-to-complete-the-task"></a>Görevi tamamlamak için kullanılacak CLI sürümleri

Görevi aşağıdaki CLI sürümlerinden birini kullanarak tamamlayabilirsiniz:

* [Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md): Klasik ve kaynak yönetimi dağıtım modellerine yönelik CLI'mız.
* [Azure CLI 2.0](dns-operations-dnszones-cli.md): Kaynak yönetimi dağıtım modeline yönelik yeni nesil CLI'mız.

## <a name="introduction"></a>Giriş

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

## <a name="set-up-azure-cli-20-for-azure-dns"></a>Azure DNS için Azure CLI 2.0'ı ayarlama

### <a name="before-you-begin"></a>Başlamadan önce

Yapılandırmanıza başlamadan önce aşağıdaki öğelerin bulunduğunu doğrulayın.

* Azure aboneliği. Henüz Azure aboneliğiniz yoksa [MSDN abonelik avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.

* Azure CLI 2.0, Windows, Linux veya Mac için kullanılabilir en son sürümünü yükleyin Daha fazla bilgi için bkz. [Azure CLI 2.0'ı yükleme](https://docs.microsoft.com/cli/azure/install-az-cli2).

### <a name="sign-in-to-your-azure-account"></a>Azure hesabınızda oturum açma

Bir konsol penceresi açın ve kimlik bilgilerinizi girerek kimliğinizi doğrulayın. Daha fazla bilgi için bkz. Azure CLI üzerinden Azure'da oturum açma

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

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure Resource Manager, tüm kaynak gruplarının bir konum belirtmesini gerektirir. Bu, kaynak grubunda kaynaklar için varsayılan konum olarak kullanılır. Ancak tüm DNS kaynakları bölgesel değil de global olduğundan, kaynak grubu konumu seçiminin Azure DNS üzerinde hiçbir etkisi yoktur.

Var olan bir kaynak grubunu kullanıyorsanız bu adımı atlayabilirsiniz.

```azurecli
az group create --name myresourcegroup --location "West US"
```

## <a name="getting-help"></a>Yardım alma

Azure DNS ile ilgili tüm CLI 2.0 komutları başlayın `az network dns`. Yardım her komutu kullanmak için mevcut `--help` seçeneği (kısa form `-h`).  Örneğin:

```azurecli
az network dns --help
az network dns zone --help
az network dns zone create --help
```

## <a name="create-a-dns-zone"></a>DNS bölgesi oluşturma

DNS bölgesi, `az network dns zone create` komutu kullanılarak oluşturulur. Yardım için bkz. `az network dns zone create -h`.

Aşağıdaki örnek adlı bir DNS bölgesi oluşturur *contoso.com* kaynak grubunda *MyResourceGroup*:

```azurecli
az network dns zone create --resource-group MyResourceGroup --name contoso.com
```

### <a name="to-create-a-dns-zone-with-tags"></a>Etiketle bir DNS bölgesi oluşturmak için

Aşağıdaki örnekte, iki DNS bölgesi oluşturmak gösterilmiştir [Azure Resource Manager etiketlerine](dns-zones-records.md#tags), *proje demo =* ve *env = test*, kullanarak `--tags` parametre (kısa form `-t`):

```azurecli
az network dns zone create --resource-group MyResourceGroup --name contoso.com --tags "project=demo" "env=test"
```

## <a name="get-a-dns-zone"></a>Bir DNS bölgesini alın

Bir DNS bölgesi almak kullanın `az network dns zone show`. Yardım için bkz. `az network dns zone show --help`.

Aşağıdaki örnek DNS bölgesi verir *contoso.com* ve ilişkili verileri kaynak grubundan *MyResourceGroup*. 

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

DNS kayıtlarını tarafından döndürülen değil Not `az network dns zone show`. DNS kayıtlarını listesinde, kullanmak için `az network dns record-set list`.


## <a name="list-dns-zones"></a>Liste DNS bölgeleri

DNS bölgelerini numaralandırmak için kullanmanız `az network dns zone list`. Yardım için bkz. `az network dns zone list --help`.

Kaynak grubu belirterek yalnızca kaynak grubunda bölgelerini listeler:

```azurecli
az network dns zone list --resource-group MyResourceGroup
```

Kaynak grubu atlama Abonelikteki tüm bölgeler listeler:

```azurecli
az network dns zone list 
```

## <a name="update-a-dns-zone"></a>Bir DNS bölgesini güncelleştirme

Kullanarak bir DNS bölgesi kaynak değişiklikler yapılabilir `az network dns zone update`. Yardım için bkz. `az network dns zone update --help`.

Bu komut bölge içinde DNS kayıt kümelerini hiçbirini güncelleştirmez (bkz [yönetmek DNS kayıtlarını nasıl](dns-operations-recordsets-cli.md)). Yalnızca, bölge kaynak özelliklerini güncelleştirmek için kullanılır. Bu özellikleri şu anda sınırlı [Azure Resource Manager 'etiketleri'](dns-zones-records.md#tags) bölge kaynak için.

Aşağıdaki örnek, bir DNS bölgesi etiketleri güncelleştirmek gösterilmiştir. Varolan etiketleri belirtilen değeriyle değiştirilir.

```azurecli
az network dns zone update --resource-group myresourcegroup --name contoso.com --set tags.team=support
```

## <a name="delete-a-dns-zone"></a>Bir DNS bölgesi Sil

DNS bölgeleri kullanılarak silinebilir `az network dns zone delete`. Yardım için bkz. `az network dns zone delete --help`.

> [!NOTE]
> Bir DNS bölgesi silme bölgedeki tüm DNS kayıtlarını siler. Bu işlem geri alınamaz. DNS bölgesi kullanımdaysa, bölge silindiğinde bölge kullanarak Hizmetleri başarısız olur.
>
>Yanlışlıkla bölge silinmeye karşı korumak için bkz: [DNS bölgeleri ve kayıtları korumak nasıl](dns-protect-zones-recordsets.md).

Bu komut onaylamanızı ister. İsteğe bağlı `--yes` anahtar bu istemi gizler.

Aşağıdaki örnekte, bölgenin silmek gösterilmiştir *contoso.com* kaynak grubundan *MyResourceGroup*.

```azurecli
az network dns zone delete --resource-group myresourcegroup --name contoso.com
```

## <a name="next-steps"></a>Sonraki adımlar

Bilgi edinmek için nasıl [kayıt kümelerini ve kayıtları yönetmek](dns-getstarted-create-recordset-cli.md) DNS bölgesinde.

Bilgi edinmek için nasıl [etki alanınızı Azure DNS'ye temsilci](dns-domain-delegation.md).

