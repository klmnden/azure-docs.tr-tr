---
title: "CLI kullanarak bir DNS bölgesi oluşturma | Microsoft Belgeleri"
description: "Azure DNS&quot;de DNZ bölgesi oluşturmayı öğrenin. Bu kılavuzda, Azure CLI&quot;yi kullanarak ilk DNS bölgenizi oluşturmanız ve yönetmeniz için adım adım talimatlar sunulmaktadır."
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
ms.date: 12/21/2016
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: f156e4d4c5ffddb7e93ebf21baa75864e0e260e9
ms.openlocfilehash: d00792a4bb19e194dbbcee8b9c11e4a744891388

---

# <a name="create-an-azure-dns-zone-using-cli"></a>CLI kullanarak bir Azure DNS bölgesi oluşturma

> [!div class="op_single_selector"]
> * [Azure Portal](dns-getstarted-create-dnszone-portal.md)
> * [PowerShell](dns-getstarted-create-dnszone.md)
> * [Azure CLI](dns-getstarted-create-dnszone-cli.md)

Bu makale Windows, Mac ve Linux platformlarında kullanılabilen çok platformlu Azure CLI'yi kullanarak DNS bölgesi oluşturma adımlarında size rehberlik yapacaktır. [Azure PowerShell](dns-getstarted-create-dnszone.md) veya [Azure portalı](dns-getstarted-create-dnszone-portal.md) kullanarak da bir DNS bölgesi oluşturabilirsiniz.

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

[!INCLUDE [dns-cli-setup](../../includes/dns-cli-setup-include.md)]


## <a name="create-a-dns-zone"></a>DNS bölgesi oluşturma

DNS bölgesi, `azure network dns zone create` komutu kullanılarak oluşturulur. Bu komutla ilgili yardım içeriğini görmek için `azure network dns zone create -h` yazın.

Aşağıdaki örnek, *MyResourceGroup* adlı kaynak grubunda *contoso.com* adlı bir DNS bölgesi oluşturur. Değerleri kendinizinkilerle değiştirerek DNS bölgesini oluşturmak için örneği kullanın.

```azurecli
azure network dns zone create MyResourceGroup contoso.com
```

## <a name="verify-your-dns-zone"></a>DNS bölgenizi doğrulama

### <a name="view-records"></a>Kayıtları görüntüleme

Bir DNS bölgesinin oluşturulması aşağıdaki DNS kayıtlarını da oluşturur:

* *Yetki Başlangıcı* (SOA) kaydı. Bu kayıt, her DNS bölgesinin kök dizininde bulunur.
* Yetkili ad sunucusu (NS) kayıtları. Bu kayıtlar, bölgeyi hangi ad sunucularının barındırdığını gösterir. Azure DNS bir ad sunucuları havuzu kullanır ve böylece farklı ad sunucuları Azure DNS’deki farklı bölgelere atanabilir. Daha fazla bilgi için bkz. [Bir etki alanını Azure DNS’ye devretme](dns-domain-delegation.md).

Bu kayıtları görüntülemek için `azure network dns-record-set list` kullanın:

```azurecli
azure network dns record-set list MyResourceGroup contoso.com

info:    Executing command network dns record-set list
+ Looking up the DNS Record Sets
data:    Name                            : @
data:    Type                            : NS
data:    TTL                             : 172800
data:    Records:
data:      ns1-01.azure-dns.com.
data:      ns2-01.azure-dns.net.
data:      ns3-01.azure-dns.org.
data:      ns4-01.azure-dns.info.
data:
data:    Name                            : @
data:    Type                            : SOA
data:    TTL                             : 3600
data:    Email                           : azuredns-hostmaster.microsoft.com
data:    Host                            : ns1-01.azure-dns.com.
data:    Serial Number                   : 2
data:    Refresh Time                    : 3600
data:    Retry Time                      : 300
data:    Expire Time                     : 2419200
data:    Minimum TTL                     : 300
data:
info:    network dns record-set list command OK
```

> [!NOTE]
> Bir DNS Bölgesinin kökündeki (veya *tepesindeki*) kayıt kümeleri, kayıt kümesinin adı olarak **@** kullanır.

### <a name="test-name-servers"></a>Ad sunucularını test etme

nslookup, dig veya `Resolve-DnsName` PowerShell cmdlet'i gibi DNS araçlarını kullanarak DNS bölgenizin Azure DNS ad sunucularında mevcut olup olmadığını kontrol edebilirsiniz.

Azure DNS'de henüz yeni bölgeyi kullanmak için etki alanınızı devretmediyseniz DNS sorgusunu bölgenizin ad sunucularından birine doğrudan yönlendirmeniz gerekir. Bölgenizin ad sunucuları NS kayıtlarında belirtilir ve `azure network dns record-set list` tarafından belirlenir.

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




<!--HONumber=Feb17_HO1-->


