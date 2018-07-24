---
title: Hızlı başlangıç - Azure CLI kullanarak Azure DNS bölgesi ve kaydı oluşturma
description: Hızlı başlangıç - Azure DNS'te DNS bölgesi ve kaydı oluşturma hakkında bilgi edinin. Bu, Azure CLI kullanarak ilk DNS bölgenizi ve kaydınızı oluşturmaya ve yönetmeye yönelik adım adım bir kılavuzdur.
services: dns
author: vhorne
ms.service: dns
ms.topic: quickstart
ms.date: 7/16/2018
ms.author: victorh
ms.openlocfilehash: 3fb39558ff99c35786dedc133a9d1d1a450b5928
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39090131"
---
# <a name="quickstart-create-an-azure-dns-zone-and-record-using-azure-cli"></a>Hızlı başlangıç: Azure CLI kullanarak Azure DNS bölgesi ve kaydı oluşturma

Bu makale Windows, Mac ve Linux platformlarında kullanılabilen Azure CLI'yi kullanarak ilk DNS bölgenizi ve kaydınızı oluşturma adımlarında size rehberlik yapacaktır. Ayrıca, [Azure portal](dns-getstarted-portal.md) veya [Azure PowerShell](dns-getstarted-powershell.md) kullanarak aşağıdaki adımları gerçekleştirebilirsiniz.

DNS bölgesi belirli bir etki alanıyla ilgili DNS kayıtlarını barındırmak için kullanılır. Etki alanınızı Azure DNS'de barındırmaya başlamak için bir DNS bölgesi oluşturmanız gerekir. Ardından bu DNS bölgesinde etki alanınız için tüm DNS kayıtları oluşturulur. Son olarak, DNS bölgenizi İnternet'te yayımlamak için etki alanının ad sunucularını yapılandırmanız gerekir. Bu adımların her biri aşağıda açıklanmıştır.

Azure DNS artık özel DNS bölgelerini de destekler (şu anda genel önizlemede). Özel DNS bölgeleri hakkında daha fazla bilgi için bkz. [Özel etki alanları için Azure DNS'i kullanma](private-dns-overview.md). Özel bir DNS bölgesi oluşturma örneği için bkz. [CLI kullanarak Azure DNS özel bölgeleriyle çalışmaya başlama](./private-dns-getstarted-cli.md).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-the-resource-group"></a>Kaynak grubunu oluşturma

DNS bölgesini oluşturmadan önce, DNS bölgesini içerecek kaynak grubunu oluşturun:

```azurecli
az group create --name MyResourceGroup --location "East US"
```

## <a name="create-a-dns-zone"></a>DNS bölgesi oluşturma

DNS bölgesi, `az network dns zone create` komutu kullanılarak oluşturulur. Bu komutla ilgili yardım içeriğini görmek için `az network dns zone create -h` yazın.

Aşağıdaki örnek, *MyResourceGroup* kaynak grubunda *contoso.com* adlı bir DNS bölgesi oluşturur. Değerleri kendinizinkilerle değiştirerek DNS bölgesini oluşturmak için örneği kullanın.

```azurecli
az network dns zone create -g MyResourceGroup -n contoso.com
```

## <a name="create-a-dns-record"></a>DNS kaydı oluşturma

DNS kaydı oluşturmak için `az network dns record-set [record type] add-record` komutunu kullanın. Örneğin, A kayıtlarına ilişkin yardım için bkz. `azure network dns record-set A add-record -h`.

Aşağıdaki örnekte, "MyResourceGroup" kaynak grubu içindeki "contoso.com" DNS Bölgesinde göreli adı "www" olan bir kaynak oluşturulmaktadır. "www.contoso.com", kayıt kümesinin tam adıdır. Kayıt türü "A", IP adresi "1.2.3.4" ve varsayılan TTL değeri 3600 saniyedir (1 saat).

```azurecli
az network dns record-set a add-record -g MyResourceGroup -z contoso.com -n www -a 1.2.3.4
```

## <a name="view-records"></a>Kayıtları görüntüleme

Bölgenizdeki DNS kayıtlarını listelemek için şu seçenekleri kullanın:

```azurecli
az network dns record-set list -g MyResourceGroup -z contoso.com
```

## <a name="update-name-servers"></a>Ad sunucularını güncelleştirme

DNS bölgenizin ve kayıtlarınızın doğru şekilde ayarlandığına karar verdikten sonra, Azure DNS ad sunucularını kullanmak için etki alanınızın adını yapılandırmanız gerekir. Bunun yapılması, İnternet üzerindeki diğer kullanıcıların DNS kayıtlarınızı bulmasını sağlar.

Bölgenizin ad sunucuları `az network dns zone show` komutu tarafından belirtilir. Ad sunucusu adlarını görmek için aşağıdaki örnekte gösterildiği gibi JSON çıktısını kullanın.

```azurecli
az network dns zone show -g MyResourceGroup -n contoso.com -o json

{
  "etag": "00000003-0000-0000-b40d-0996b97ed101",
  "id": "/subscriptions/a385a691-bd93-41b0-8084-8213ebc5bff7/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com",
  "location": "global",
  "maxNumberOfRecordSets": 5000,
  "name": "contoso.com",
  "nameServers": [
    "ns1-01.azure-dns.com.",
    "ns2-01.azure-dns.net.",
    "ns3-01.azure-dns.org.",
    "ns4-01.azure-dns.info."
  ],
  "numberOfRecordSets": 3,
  "resourceGroup": "myresourcegroup",
  "tags": {},
  "type": "Microsoft.Network/dnszones"
}
```

Bu ad sunucuları, etki alanı adı kayıt şirketi (etki alanı adını satın aldığınız şirket) ile birlikte yapılandırılmalıdır. Kayıt şirketiniz, etki alanı için ad sunucularını ayarlama seçeneğini sunar. Daha fazla bilgi için bkz. [Etki alanınızı Azure DNS’e devretme](dns-domain-delegation.md).

## <a name="delete-all-resources"></a>Tüm kaynakları silme
 
Artık gerekmediğinde, kaynak grubunu silerek bu hızlı başlangıçta oluşturulan tüm kaynakları silebilirsiniz:

```azurecli
az group delete --name MyResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Artık Azure CLI kullanarak ilk DNS bölgenizi ve kaydınızı oluşturduğunuza göre, özel etki alanında bir web uygulaması için kayıtlar oluşturabilirsiniz.

> [!div class="nextstepaction"]
> [Özel etki alanında web uygulaması için DNS kayıtları oluşturma](./dns-web-sites-custom-domain.md)
