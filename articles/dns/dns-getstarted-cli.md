---
title: Hızlı başlangıç - Azure CLI kullanarak Azure DNS bölgesi ve kaydı oluşturma
description: Hızlı başlangıç - Azure DNS'te DNS bölgesi ve kaydı oluşturma hakkında bilgi edinin. Bu, Azure CLI kullanarak ilk DNS bölgenizi ve kaydınızı oluşturmaya ve yönetmeye yönelik adım adım bir kılavuzdur.
services: dns
author: vhorne
ms.service: dns
ms.topic: quickstart
ms.date: 3/11/2019
ms.author: victorh
ms.openlocfilehash: 7a2c300e30050e7e46a2b2c724258539df85e410
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66111342"
---
# <a name="quickstart-create-an-azure-dns-zone-and-record-using-azure-cli"></a>Hızlı Başlangıç: Bir Azure DNS bölgesi ve kaydı Azure CLI kullanarak oluşturma

Bu makale Windows, Mac ve Linux platformlarında kullanılabilen Azure CLI'yi kullanarak ilk DNS bölgenizi ve kaydınızı oluşturma adımlarında size rehberlik yapacaktır. Ayrıca, [Azure portal](dns-getstarted-portal.md) veya [Azure PowerShell](dns-getstarted-powershell.md) kullanarak aşağıdaki adımları gerçekleştirebilirsiniz.

DNS bölgesi belirli bir etki alanıyla ilgili DNS kayıtlarını barındırmak için kullanılır. Etki alanınızı Azure DNS'de barındırmaya başlamak için bir DNS bölgesi oluşturmanız gerekir. Ardından bu DNS bölgesinde etki alanınız için tüm DNS kayıtları oluşturulur. Son olarak, DNS bölgenizi Internet'te yayımlamak için etki alanının ad sunucularını yapılandırmanız gerekir. Bu adımların her biri aşağıda açıklanmıştır.

Azure DNS artık özel DNS bölgelerini de destekler (şu anda genel önizlemede). Özel DNS bölgeleri hakkında daha fazla bilgi için bkz. [Özel etki alanları için Azure DNS'i kullanma](private-dns-overview.md). Özel bir DNS bölgesi oluşturma örneği için bkz. [CLI kullanarak Azure DNS özel bölgeleriyle çalışmaya başlama](./private-dns-getstarted-cli.md).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-the-resource-group"></a>Kaynak grubunu oluşturma

DNS bölgesini oluşturmadan önce, DNS bölgesini içerecek kaynak grubunu oluşturun.

```azurecli
az group create --name MyResourceGroup --location "East US"
```

## <a name="create-a-dns-zone"></a>DNS bölgesi oluşturma

DNS bölgesi, `az network dns zone create` komutu kullanılarak oluşturulur. Bu komutla ilgili yardım içeriğini görmek için `az network dns zone create -h` yazın.

Aşağıdaki örnekte adlı bir DNS bölgesi oluşturur *contoso.xyz* kaynak grubundaki *MyResourceGroup*. Değerleri kendinizinkilerle değiştirerek DNS bölgesini oluşturmak için örneği kullanın.

```azurecli
az network dns zone create -g MyResourceGroup -n contoso.xyz
```

## <a name="create-a-dns-record"></a>DNS kaydı oluşturma

DNS kaydı oluşturmak için `az network dns record-set [record type] add-record` komutunu kullanın. A kayıtları hakkında yardım için bkz. `azure network dns record-set A add-record -h`.

Aşağıdaki örnek, "MyResourceGroup" kaynak grubunda "contoso.xyz" DNS bölgesinde "www" göreli adı olan bir kayıt oluşturur. Kayıt kümesi tam olarak nitelenmiş adını "www.contoso.xyz" dir. Kayıt türü "A", "10.10.10.10" IP adresi ve varsayılan TTL 3600 saniye (1 saat) değeri var.

```azurecli
az network dns record-set a add-record -g MyResourceGroup -z contoso.xyz -n www -a 10.10.10.10
```

## <a name="view-records"></a>Kayıtları görüntüleme

Bölgenizdeki DNS kayıtlarını listelemek için şu komutu çalıştırın:

```azurecli
az network dns record-set list -g MyResourceGroup -z contoso.xyz
```

## <a name="test-the-name-resolution"></a>Ad çözümlemesini test etme

Test 'A' kaydı test DNS bölgesi olduğuna göre ad çözümlemesi adında bir araç ile test edebilirsiniz *nslookup*. 

**DNS ad çözümlemesini test etmek için:**

1. Bölgeniz için ad sunucuları listesini almak için aşağıdaki cmdlet'i çalıştırın:

   ```azurecli
   az network dns record-set ns show --resource-group MyResourceGroup --zone-name contoso.xyz --name @
   ```

1. Ad sunucusu adlarını biri önceki adımın çıktıdan kopyalayın.

1. Bir komut istemi açın ve aşağıdaki komutu çalıştırın:

   ```
   nslookup www.contoso.xyz <name server name>
   ```

   Örneğin:

   ```
   nslookup www.contoso.xyz ns1-08.azure-dns.com.
   ```

   Aşağıdaki ekrana benzer bir şey görmeniz gerekir:

   ![nslookup](media/dns-getstarted-portal/nslookup.PNG)

Ana bilgisayar adı **www\.contoso.xyz** çözümler **10.10.10.10**yapılandırdığınız şekilde. Bu sonuç, ad çözümlemesi doğru şekilde çalıştığını doğrular.

## <a name="delete-all-resources"></a>Tüm kaynakları silme

Artık gerekmediğinde, kaynak grubunu silerek bu hızlı başlangıçta oluşturulan tüm kaynakları silebilirsiniz:

```azurecli
az group delete --name MyResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Artık Azure CLI kullanarak ilk DNS bölgenizi ve kaydınızı oluşturduğunuza göre, özel etki alanında bir web uygulaması için kayıtlar oluşturabilirsiniz.

> [!div class="nextstepaction"]
> [Özel etki alanında web uygulaması için DNS kayıtları oluşturma](./dns-web-sites-custom-domain.md)
