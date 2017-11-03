---
title: "Azure DNS'de - Azure CLI 1.0 DNS bölgelerini yönetme | Microsoft Docs"
description: "DNS bölgelerini Azure CLI 1.0 kullanarak yönetebilirsiniz. Bu makalede, güncelleştirme, silme ve Azure DNS üzerinde DNS bölgeleri oluşturma gösterilmektedir."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 8ab63bc4-5135-4ed8-8c0b-5f0712b9afed
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/21/2016
ms.author: gwallace
ms.openlocfilehash: 588c87749f049eff5b9e0729f6769c8367ba41e4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-manage-dns-zones-in-azure-dns-using-the-azure-cli-10"></a>Azure CLI 1.0 kullanarak Azure DNS'de DNS bölgelerini yönetme

> [!div class="op_single_selector"]
> * [Portal](dns-operations-dnszones-portal.md)
> * [PowerShell](dns-operations-dnszones.md)
> * [Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md)
> * [Azure CLI 2.0](dns-operations-dnszones-cli.md)

Bu kılavuz, Windows, Mac ve Linux için kullanılabilir olduğu platformlar arası Azure CLI 1.0 kullanarak DNS bölgelerini yönetme gösterir. DNS bölgelerini kullanarak da yönetebilirsiniz [Azure PowerShell](dns-operations-dnszones.md) ya da Azure portal.

## <a name="cli-versions-to-complete-the-task"></a>Görevi tamamlamak için kullanılacak CLI sürümleri

Görevi aşağıdaki CLI sürümlerinden birini kullanarak tamamlayabilirsiniz:

* [Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md): Klasik ve kaynak yönetimi dağıtım modellerine yönelik CLI'mız.
* [Azure CLI 2.0](dns-operations-dnszones-cli.md): Kaynak yönetimi dağıtım modeline yönelik yeni nesil CLI'mız.

## <a name="introduction"></a>Giriş

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

[!INCLUDE [dns-cli-setup](../../includes/dns-cli-setup-include.md)]

## <a name="getting-help"></a>Yardım alma

Azure DNS ile ilgili tüm CLI 1.0 komutları başlayın `azure network dns`. Yardım her komutu kullanmak için mevcut `--help` seçeneği (kısa form `-h`).  Örneğin:

```azurecli
azure network dns -h
azure network dns zone -h
azure network dns zone create -h
```

## <a name="create-a-dns-zone"></a>DNS bölgesi oluşturma

DNS bölgesi, `azure network dns zone create` komutu kullanılarak oluşturulur. Yardım için bkz. `azure network dns zone create -h`.

Aşağıdaki örnek adlı bir DNS bölgesi oluşturur *contoso.com* kaynak grubunda *MyResourceGroup*:

```azurecli
azure network dns zone create MyResourceGroup contoso.com
```

### <a name="to-create-a-dns-zone-with-tags"></a>Etiketle bir DNS bölgesi oluşturmak için

Aşağıdaki örnekte, iki DNS bölgesi oluşturmak gösterilmiştir [Azure Resource Manager etiketlerine](dns-zones-records.md#tags), *proje demo =* ve *env = test*, kullanarak `--tags` parametre (kısa form `-t`):

```azurecli
azure network dns zone create MyResourceGroup contoso.com -t "project=demo";"env=test"
```

## <a name="get-a-dns-zone"></a>Bir DNS bölgesini alın

Bir DNS bölgesi almak kullanın `azure network dns zone show`. Yardım için bkz. `azure network dns zone show -h`.

Aşağıdaki örnek DNS bölgesi verir *contoso.com* ve ilişkili verileri kaynak grubundan *MyResourceGroup*. 

```azurecli
azure network dns zone show MyResourceGroup contoso.com
```

Aşağıdaki örnek, yanıttır.

```
info:    Executing command network dns zone show
+ Looking up the dns zone "contoso.com"
data:    Id                              : /subscriptions/.../contoso.com
data:    Name                            : contoso.com
data:    Type                            : Microsoft.Network/dnszones
data:    Location                        : global
data:    Number of record sets           : 2
data:    Max number of record sets       : 5000
data:    Name servers:
data:        ns1-01.azure-dns.com.
data:        ns2-01.azure-dns.net.
data:        ns3-01.azure-dns.org.
data:        ns4-01.azure-dns.info.
data:    Tags                            : project=demo;env=test
info:    network dns zone show command OK
```

DNS kayıtlarını tarafından döndürülen değil Not `azure network dns zone show`. DNS kayıtlarını listesinde, kullanmak için `azure network dns record-set list`.


## <a name="list-dns-zones"></a>Liste DNS bölgeleri

DNS bölgelerini numaralandırmak için kullanmanız `azure network dns zone list`. Yardım için bkz. `azure network dns zone list -h`.

Kaynak grubu belirterek yalnızca kaynak grubunda bölgelerini listeler:

```azurecli
azure network dns zone list MyResourceGroup
```

Kaynak grubu atlama Abonelikteki tüm bölgeler listeler:

```azurecli
azure network dns zone list 
```

## <a name="update-a-dns-zone"></a>Bir DNS bölgesini güncelleştirme

Kullanarak bir DNS bölgesi kaynak değişiklikler yapılabilir `azure network dns zone set`. Yardım için bkz. `azure network dns zone set -h`.

Bu komut bölge içinde DNS kayıt kümelerini hiçbirini güncelleştirmez (bkz [yönetmek DNS kayıtlarını nasıl](dns-operations-recordsets-cli-nodejs.md)). Yalnızca, bölge kaynak özelliklerini güncelleştirmek için kullanılır. Bu özellikleri şu anda sınırlı [Azure Resource Manager 'etiketleri'](dns-zones-records.md#tags) bölge kaynak için.

Aşağıdaki örnek, bir DNS bölgesi etiketleri güncelleştirmek gösterilmiştir. Varolan etiketleri belirtilen değeriyle değiştirilir.

```azurecli
azure network dns zone set MyResourceGroup contoso.com -t "team=support"
```

## <a name="delete-a-dns-zone"></a>Bir DNS bölgesi Sil

DNS bölgeleri kullanılarak silinebilir `azure network dns zone delete`. Yardım için bkz. `azure network dns zone delete -h`.

> [!NOTE]
> Bir DNS bölgesi silme bölgedeki tüm DNS kayıtlarını siler. Bu işlem geri alınamaz. DNS bölgesi kullanımdaysa, bölge silindiğinde bölge kullanarak Hizmetleri başarısız olur.
>
>Yanlışlıkla bölge silinmeye karşı korumak için bkz: [DNS bölgeleri ve kayıtları korumak nasıl](dns-protect-zones-recordsets.md).

Bu komut onaylamanızı ister. İsteğe bağlı `--quiet` geçiş (kısa form `-q`) bu istemi gizler.

Aşağıdaki örnekte, bölgenin silmek gösterilmiştir *contoso.com* kaynak grubundan *MyResourceGroup*.

```azurecli
azure network dns zone delete MyResourceGroup contoso.com
```

## <a name="next-steps"></a>Sonraki adımlar

Bilgi edinmek için nasıl [kayıt kümelerini ve kayıtları yönetmek](dns-getstarted-create-recordset-cli-nodejs.md) DNS bölgesinde.

Bilgi edinmek için nasıl [etki alanınızı Azure DNS'ye temsilci](dns-domain-delegation.md).

