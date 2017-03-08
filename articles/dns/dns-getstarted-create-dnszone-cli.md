---
title: "Azure CLI 2.0 kullanarak bir DNS bölgesi oluşturma | Microsoft Docs"
description: "Azure DNS&quot;de DNZ bölgesi oluşturmayı öğrenin. Bu kılavuzda, Azure CLI 2.0 kullanarak ilk DNS bölgenizi oluşturmanız ve yönetmeniz için adım adım talimatlar sunulmaktadır."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 1514426a-133c-491a-aa27-ee0962cea9dc
ms.service: dns
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/27/2017
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 1481fcb070f383d158c5a6ae32504e498de4a66b
ms.openlocfilehash: 00a4dfbe070129ce6dc84f006d793007d55dba3f
ms.lasthandoff: 02/28/2017

---

# <a name="create-an-azure-dns-zone-using-azure-cli-20"></a>Azure CLI 2.0 kullanarak bir Azure DNS bölgesi oluşturma

> [!div class="op_single_selector"]
> * [Azure portal](dns-getstarted-create-dnszone-portal.md)
> * [PowerShell](dns-getstarted-create-dnszone.md)
> * [Azure CLI 1.0](dns-getstarted-create-dnszone-cli-nodejs.md)
> * [Azure CLI 2.0](dns-getstarted-create-dnszone-cli.md)

Bu makale Windows, Mac ve Linux platformlarında kullanılabilen çok platformlu Azure CLI'yi kullanarak DNS bölgesi oluşturma adımlarında size rehberlik yapacaktır. [Azure PowerShell](dns-getstarted-create-dnszone.md) veya [Azure portalı](dns-getstarted-create-dnszone-portal.md) kullanarak da bir DNS bölgesi oluşturabilirsiniz.

## <a name="cli-versions-to-complete-the-task"></a>Görevi tamamlamak için kullanılacak CLI sürümleri

Görevi aşağıdaki CLI sürümlerinden birini kullanarak tamamlayabilirsiniz:

* [Azure CLI 1.0](dns-getstarted-create-dnszone-cli-nodejs.md): Klasik ve kaynak yönetimi dağıtım modellerine yönelik CLI'mız.
* [Azure CLI 2.0](dns-getstarted-create-dnszone-cli.md): Kaynak yönetimi dağıtım modeline yönelik yeni nesil CLI'mız.

## <a name="introduction"></a>Giriş

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

## <a name="set-up-azure-cli-20-for-azure-dns"></a>Azure DNS için Azure CLI 2.0'ı ayarlama

### <a name="before-you-begin"></a>Başlamadan önce

Yapılandırmanıza başlamadan önce aşağıdaki öğelerin bulunduğunu doğrulayın.

* Azure aboneliği. Henüz Azure aboneliğiniz yoksa [MSDN abonelik avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.

* Windows, Linux veya Mac için Azure CLI'nin son sürümünü yüklemeniz gerekir. Daha fazla bilgi için bkz. [Azure CLI 2.0'ı yükleme](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).

### <a name="sign-in-to-your-azure-account"></a>Azure hesabınızda oturum açma

Bir konsol penceresi açın ve kimlik bilgilerinizi girerek kimliğinizi doğrulayın. Daha fazla bilgi için bkz. Azure CLI üzerinden Azure'da oturum açma

```azurecli
az login
```

### <a name="select-the-subscription"></a>Aboneliği seçme

Hesapla ilişkili abonelikleri kontrol edin.

```azurecli
az account list
```

### <a name="choose-which-of-your-azure-subscriptions-to-use"></a>Hangi Azure aboneliğinizin kullanılacağını seçin.

```azurecli
az account set --subscription "subscription name"
```

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure Resource Manager, tüm kaynak gruplarının bir konum belirtmesini gerektirir. Bu, kaynak grubunda kaynaklar için varsayılan konum olarak kullanılır. Ancak tüm DNS kaynakları bölgesel değil de global olduğundan, kaynak grubu konumu seçiminin Azure DNS üzerinde hiçbir etkisi yoktur.

Var olan bir kaynak grubunu kullanıyorsanız bu adımı atlayabilirsiniz.

```azurecli
az group create --name myresourcegroup --location "West US"
```

## <a name="create-a-dns-zone"></a>DNS bölgesi oluşturma

DNS bölgesi, `az network dns zone create` komutu kullanılarak oluşturulur. Bu komutla ilgili yardım içeriğini görmek için `az network dns zone create --help` yazın.

Aşağıdaki örnek, *MyResourceGroup* adlı kaynak grubunda *contoso.com* adlı bir DNS bölgesi oluşturur. Değerleri kendinizinkilerle değiştirerek DNS bölgesini oluşturmak için örneği kullanın.

```azurecli
az network dns zone create --resource-group myresourcegroup --name contoso.com
```

## <a name="verify-your-dns-zone"></a>DNS bölgenizi doğrulama

### <a name="view-records"></a>Kayıtları görüntüleme

Bir DNS bölgesinin oluşturulması aşağıdaki DNS kayıtlarını da oluşturur:

* *Yetki Başlangıcı* (SOA) kaydı. Bu kayıt, her DNS bölgesinin kök dizininde bulunur.
* Yetkili ad sunucusu (NS) kayıtları. Bu kayıtlar, bölgeyi hangi ad sunucularının barındırdığını gösterir. Azure DNS bir ad sunucuları havuzu kullanır ve böylece farklı ad sunucuları Azure DNS’deki farklı bölgelere atanabilir. Daha fazla bilgi için bkz. [Bir etki alanını Azure DNS’ye devretme](dns-domain-delegation.md).

Bu kayıtları görüntülemek için `az network dns record-set list` kullanın:

```azurecli
az network dns record-set list --resource-group MyResourceGroup --zone-name contoso.com
```

`az network dns record-set list` yanıtı aşağıda gösterilmektedir:

```json
[
  {
    "etag": "510f4711-8b7f-414e-b8d5-c0383ea3d62e",
    "id": "/subscriptions/147a22e9-2356-4e56-b3de-1f5842ae4a3b/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/NS/@",
    "metadata": null,
    "name": "@",
    "nsRecords": [
      {
        "nsdname": "ns1-04.azure-dns.com."
      },
      {
        "nsdname": "ns2-04.azure-dns.net."
      },
      {
        "nsdname": "ns3-04.azure-dns.org."
      },
      {
        "nsdname": "ns4-04.azure-dns.info."
      }
    ],
    "resourceGroup": "myresourcegroup",
    "ttl": 172800,
    "type": "Microsoft.Network/dnszones/NS"
  },
  {
    "etag": "0e48e82f-f194-4d3e-a864-7e109a95c73a",
    "id": "/subscriptions/147a22e9-2356-4e56-b3de-1f5842ae4a3b/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/SOA/@",
    "metadata": null,
    "name": "@",
    "resourceGroup": "myresourcegroup",
    "soaRecord": {
      "email": "azuredns-hostmaster.microsoft.com",
      "expireTime": 2419200,
      "host": "ns1-04.azure-dns.com.",
      "minimumTtl": 300,
      "refreshTime": 3600,
      "retryTime": 300,
      "serialNumber": 1
    },
    "ttl": 3600,
    "type": "Microsoft.Network/dnszones/SOA"
  }
]
```

> [!NOTE]
> Bir DNS Bölgesinin kökündeki (veya *tepesindeki*) kayıt kümeleri, kayıt kümesinin adı olarak **@** kullanır.

### <a name="test-name-servers"></a>Ad sunucularını test etme

nslookup, dig veya `Resolve-DnsName` PowerShell cmdlet'i gibi DNS araçlarını kullanarak DNS bölgenizin Azure DNS ad sunucularında mevcut olup olmadığını kontrol edebilirsiniz.

Azure DNS'de henüz yeni bölgeyi kullanmak için etki alanınızı devretmediyseniz DNS sorgusunu bölgenizin ad sunucularından birine doğrudan yönlendirmeniz gerekir. Bölgenizin ad sunucuları NS kayıtlarında belirtilir ve `az network dns record-set list` tarafından belirlenir.

Aşağıdaki örnek, DNS bölgesi için atanmış ad sunucularını kullanarak domain.contoso.com sorgulamak için "dig" kullanır. Bölgeniz için doğru değerleri değiştirdiğinizden emin olun.

```
  > dig @ns1-01.azure-dns.com contoso.com
  
  <<>> DiG 9.10.2-P2 <<>> @ns1-01.azure-dns.com contoso.com
(1 server found)
global options: +cmd
  Got answer:
->>HEADER<<- opcode: QUERY, status: NOERROR, id: 60963
  flags: qr aa rd; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1
  WARNING: recursion requested but not available

  OPT PSEUDOSECTION:
  EDNS: version: 0, flags:; udp: 4000
  QUESTION SECTION:
contoso.com.                        IN      A

  AUTHORITY SECTION:
contoso.com.         3600     IN      SOA     ns1-01.azure-dns.com. azuredns-hostmaster.microsoft.com. 1 3600 300 2419200 300

Query time: 93 msec
SERVER: 208.76.47.5#53(208.76.47.5)
WHEN: Tue Jul 21 16:04:51 Pacific Daylight Time 2015
MSG SIZE  rcvd: 120
```

## <a name="next-steps"></a>Sonraki adımlar

DNS bölgesi oluşturduktan sonra bölgenizde [DNS kayıt kümeleri ve kayıtları oluşturun](dns-getstarted-create-recordset-cli.md).


