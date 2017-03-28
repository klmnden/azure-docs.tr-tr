---
title: "Azure CLI 1.0 ile Azure DNS kullanmaya başlama | Microsoft Belgeleri"
description: "Azure DNS&quot;te DNS bölgesi ve kaydı oluşturma hakkında bilgi edinin. Bu kılavuzda, Azure CLI 1.0 kullanarak ilk DNS bölgenizi ve kaydınızı oluşturup yönetmeniz için adım adım talimatlar sunulmaktadır."
services: dns
documentationcenter: na
author: jtuliani
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: fb0aa0a6-d096-4d6a-b2f6-eda1c64f6182
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/10/2017
ms.author: jonatul
translationtype: Human Translation
ms.sourcegitcommit: 424d8654a047a28ef6e32b73952cf98d28547f4f
ms.openlocfilehash: f84ac7f1fa20af2e495df89609c810107c65583a
ms.lasthandoff: 03/22/2017

---

# <a name="get-started-with-azure-dns-using-azure-cli-10"></a>Azure CLI 1.0 kullanarak Azure DNS ile çalışmaya başlama

> [!div class="op_single_selector"]
> * [Azure portal](dns-getstarted-portal.md)
> * [PowerShell](dns-getstarted-powershell.md)
> * [Azure CLI 1.0](dns-getstarted-cli-nodejs.md)
> * [Azure CLI 2.0](dns-getstarted-cli.md)

Bu makale Windows, Mac ve Linux platformlarında kullanılabilen platformlar arası Azure CLI 1.0'ı kullanarak ilk DNS bölgenizi ve kaydınızı oluşturma adımlarında size rehberlik yapacaktır. Ayrıca, Azure portal veya Azure PowerShell kullanarak aşağıdaki adımları gerçekleştirebilirsiniz.

DNS bölgesi belirli bir etki alanıyla ilgili DNS kayıtlarını barındırmak için kullanılır. Etki alanınızı Azure DNS'de barındırmaya başlamak için bir DNS bölgesi oluşturmanız gerekir. Ardından bu DNS bölgesinde etki alanınız için tüm DNS kayıtları oluşturulur. Son olarak, DNS bölgenizi Internet'te yayımlamak için etki alanının ad sunucularını yapılandırmanız gerekir. Bu adımların her biri aşağıda açıklanmıştır.

Bu yönergeler, Azure CLI 1.0’ı zaten yüklediğinizi ve oturum açtığınızı varsayar. Yardım için bkz. [Azure CLI 1.0 ile DNS bölgelerini yönetme](dns-operations-dnszones-cli-nodejs.md).


## <a name="create-a-dns-zone"></a>DNS bölgesi oluşturma

DNS bölgesi, `azure network dns zone create` komutu kullanılarak oluşturulur. Bu komutla ilgili yardım içeriğini görmek için `azure network dns zone create -h` yazın.

Aşağıdaki örnek, *MyResourceGroup* adlı kaynak grubunda *contoso.com* adlı bir DNS bölgesi oluşturur. Değerleri kendinizinkilerle değiştirerek DNS bölgesini oluşturmak için örneği kullanın.

```azurecli
azure network dns zone create MyResourceGroup contoso.com
```


## <a name="create-a-dns-record"></a>DNS kaydı oluşturma

DNS kaydı oluşturmak için `azure network dns record-set add-record` komutunu kullanın. Yardım için bkz. `azure network dns record-set add-record -h`.

Aşağıdaki örnekte, "MyResourceGroup" kaynak grubu içindeki "contoso.com" DNS Bölgesinde göreli adı "www" olan bir kaynak oluşturulmaktadır. "www.contoso.com", kayıt kümesinin tam adıdır. Kayıt türü "A", IP adresi "1.2.3.4" ve varsayılan TTL değeri 3600 saniyedir (1 saat).

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com www A -a 1.2.3.4
```

Diğer kayıt türleri, birden fazla kayıt içerek kayıt kümeleri, alternatif TTL değerleri ve var olan kayıtların değiştirilmesi hakkında bilgi için bkz. [Azure CLI 1.0 kullanarak DNS kayıtlarını ve kayıt kümelerini yönetme](dns-operations-recordsets-cli-nodejs.md).


## <a name="view-records"></a>Kayıtları görüntüleme

Bölgenizdeki DNS kayıtlarını listelemek için şu seçenekleri kullanın:

```azurecli
azure network dns record-set list MyResourceGroup contoso.com
```


## <a name="update-name-servers"></a>Ad sunucularını güncelleştirme

DNS bölgenizin ve kayıtlarınızın doğru şekilde ayarlandığına karar verdikten sonra, Azure DNS ad sunucularını kullanmak için etki alanınızın adını yapılandırmanız gerekir. Bunun yapılması, İnternet üzerindeki diğer kullanıcıların DNS kayıtlarınızı bulmasını sağlar.

Bölgenizin ad sunucuları `azure network dns zone show` komutu tarafından belirtilir:

```azurecli
azure network dns zone show MyResourceGroup contoso.com

info:    Executing command network dns zone show
+ Looking up the dns zone "contoso.com"
data:    Id                              : /subscriptions/a385a691-bd93-41b0-8084-8213ebc5bff7/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com
data:    Name                            : contoso.com
data:    Type                            : Microsoft.Network/dnszones
data:    Location                        : global
data:    Number of record sets           : 3
data:    Max number of record sets       : 5000
data:    Name servers:
data:        ns1-01.azure-dns.com.
data:        ns2-01.azure-dns.net.
data:        ns3-01.azure-dns.org.
data:        ns4-01.azure-dns.info.
data:    Tags                            :
info:    network dns zone show command OK
```

Bu ad sunucuları, etki alanı adı kayıt şirketi (etki alanı adını satın aldığınız şirket) ile birlikte yapılandırılmalıdır. Kayıt şirketiniz, etki alanı için ad sunucularını ayarlama seçeneğini sunar. Daha fazla bilgi için bkz. [Etki alanınızı Azure DNS’e devretme](dns-domain-delegation.md).


## <a name="next-steps"></a>Sonraki adımlar

Azure DNS hakkında daha fazla bilgi için bkz. [Azure DNS'e genel bakış](dns-overview.md).

Azure DNS’te DNS bölgelerini yönetme hakkında daha fazla bilgi için bkz. [Azure CLI 1.0 ile Azure DNS’te DNS bölgelerini yönetme](dns-operations-dnszones-cli-nodejs.md).

Azure DNS’te DNS kayıtlarını yönetme hakkında daha fazla bilgi için bkz. [Azure CLI 1.0 ile Azure DNS’te DNS kayıtlarını yönetme](dns-operations-recordsets-cli-nodejs.md).


