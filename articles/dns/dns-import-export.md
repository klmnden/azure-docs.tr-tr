---
title: Alma ve Azure CLI 2.0 kullanan Azure DNS'ye bir etki alanı bölge dosyasına verme | Microsoft Docs
description: İçeri aktarma ve Azure CLI 2.0 kullanarak Azure DNS'ye bir DNS bölge dosyasına dışarı aktarma hakkında bilgi edinin
services: dns
documentationcenter: na
author: KumudD
manager: timlt
ms.assetid: f5797782-3005-4663-a488-ac0089809010
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/30/2018
ms.author: kumud
ms.openlocfilehash: 3aee4e20b43d946101e692f0dca76b07e04dbb7a
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
ms.locfileid: "34069386"
---
# <a name="import-and-export-a-dns-zone-file-using-the-azure-cli-20"></a>Alma ve Azure CLI 2.0 kullanan bir DNS bölge dosyasına verme 

Bu makalede, Azure CLI 2.0 kullanan Azure DNS için DNS bölge dosyalarını vermek ve almak nasıl açıklanmaktadır.

## <a name="introduction-to-dns-zone-migration"></a>DNS bölge geçişe giriş

Bir DNS bölge dosyasına bölgedeki her etki alanı adı sistemi (DNS) kaydı ayrıntılarını içeren bir metin dosyasıdır. DNS kayıtlarını DNS sistemler arasında aktarmak için uygun hale getirme standart bir biçimdedir. Bir bölge dosyası kullanarak içine veya dışına Azure DNS'ye bir DNS bölgesi aktarmak için bir hızlı, güvenilir ve kolay yoludur.

Azure DNS, içeri aktarma ve Azure komut satırı arabirimi (CLI) kullanarak bölge dosyaları dışarı aktarma destekler. Bölge dosyası alma **değil** Azure PowerShell veya Azure Portalı aracılığıyla şu anda desteklenmiyor.

Azure CLI 2.0 Azure hizmetleri yönetmek için kullanılan bir platformlar arası komut satırı aracıdır. Windows, Mac ve Linux platformlarından için kullanılabilir [Azure indirmeler sayfası](https://azure.microsoft.com/downloads/). Platformlar arası desteği önemlidir içeri ve dışarı aktarma bölge dosyaları için çünkü en yaygın adı sunucu yazılımı [BAĞLAMAK](https://www.isc.org/downloads/bind/), genellikle Linux üzerinde çalışır.


## <a name="obtain-your-existing-dns-zone-file"></a>Var olan DNS bölge dosyanızı alın

Azure DNS'yi bir DNS bölge dosyasına içeri aktarmadan önce bölge dosyasının bir kopyasını edinmeniz gerekir. DNS bölgesi şu anda barındırıldığı üzerinde bu dosyanın kaynağına bağlıdır.

* DNS bölgenizi (örneğin, etki alanı kayıt şirketi, ayrılmış DNS barındırma sağlayıcısı veya alternatif bulut sağlayıcısı) bir iş ortağı hizmeti tarafından barındırılıyorsa, bu hizmetin DNS bölge dosya yükleme olanağı sağlamalıdır.
* DNS bölgenizi Windows DNS üzerinde barındırılıyorsa, bölge dosyaları için varsayılan klasördür **%systemroot%\system32\dns**. Her bölge dosyasının tam yolunu da gösterir **genel** DNS konsolunun sekmesi.
* DNS bölgenizi BIND kullanarak barındırılıyorsa, her bölge için bölge dosyasının konumu BIND yapılandırma dosyasında belirtilen **named.conf**.

> [!NOTE]
> GoDaddy indirilen bölge dosyaları biraz standart olmayan bir biçime sahip. Azure DNS'yi bu bölge dosyalarını içeri aktarmadan önce bunu düzeltmeniz gerekir.
>
> RDATA her DNS kaydının DNS adlarını tam olarak nitelenmiş adlar belirtilmiş, ancak bir sonlandırma yok "." Bu, göreli adları olarak diğer DNS sistemler tarafından yorumlanan anlamına gelir. Sonlandırma eklenecek bölge dosyasını düzenlemeniz gerekir "." Azure DNS'yi almadan önce adlarıyla.
>
> Örneğin, "www 3600 CNAME contoso.com ORMANINDAKİ" CNAME kaydı "CNAME contoso.com 3600 www." değiştirilmesi gerekir
> (bir sonlandırma ile ".").

## <a name="import-a-dns-zone-file-into-azure-dns"></a>Azure DNS'yi bir DNS bölge dosyasına alın

Bir bölge dosyasını içeri bir zaten mevcut değilse yeni bir bölge, Azure DNS'de oluşturur. Bölgesi zaten varsa, bölge dosyası kayıt kümelerinde mevcut kayıt kümeleriyle birleştirilmesi gerekir.

### <a name="merge-behavior"></a>Davranış birleştirme

* Varsayılan olarak, var olan ve yeni kayıt kümelerini birleştirilir. Birleştirilmiş bir kayıt kümesi içinde aynı kayıtlar XML'deki yinelenen.
* Kayıt kümeleri birleştirildiğinde, yaşam süresi (TTL) önceden var olan kayıt kümelerinin kullanılır.
* Yetki (SOA) parametreleri başlangıcı (dışında `host`) her zaman alınan bölge dosyasından alınır. Benzer şekilde, bölgenin tepesinde ayarlamak ad sunucusu kaydı için TTL her zaman alınan bölge dosyasından alınır.
* İçeri aktarılan bir CNAME kaydı, aynı ada sahip varolan bir CNAME kaydı değiştirmez.  
* Bir CNAME kaydı ve başka bir kayıtla aynı adlı ancak farklı türü (bakılmaksızın, varolan veya yeni) arasında bir çakışma ortaya etkinleştirildiğinde, varolan bir kaydı korunur. 

### <a name="additional-information-about-importing"></a>İçeri aktarma hakkında ek bilgi

Aşağıdaki notlar, bölge hakkında ek teknik ayrıntılar alma işlemi sunar.

* `$TTL` Yönergesi isteğe bağlıdır ve desteklenmiyor. Durumlarda `$TTL` yönergesi verilen, açık bir TTL olmadan kayıtları varsayılan TTL 3600 saniyelik alınan kümesi. Aynı kayıt kümesindeki iki kayıt farklı TTLs belirttiğinizde, daha düşük değer kullanılır.
* `$ORIGIN` Yönergesi isteğe bağlıdır ve desteklenmiyor. Durumlarda `$ORIGIN` , kullanılan varsayılan değer: komut satırında belirtilen bölge adı ayarlanmadı (sonlandırma artı ".").
* `$INCLUDE` Ve `$GENERATE` yönergeleri desteklenmez.
* Bu kayıt türleri desteklenir: A, AAAA, CNAME, MX, NS, SOA, SRV ve TXT.
* Bir bölge oluşturulduğunda SOA kaydı Azure DNS tarafından otomatik olarak oluşturulur. Bir bölge dosyasını içe aktardığınızda, tüm SOA parametreleri bölge dosyasından alınır *dışında* `host` parametresi. Bu parametre, Azure DNS tarafından sağlanan değeri kullanır. Bu durum, bu parametre Azure DNS tarafından sağlanan birincil ad sunucusu başvurması gerekir çünkü.
* Bölge oluşturulduğunda bölgenin tepesinde ayarlamak ad sunucusu kaydı da Azure DNS tarafından otomatik olarak oluşturulur. Yalnızca bu kayıt kümesi TTL alınır. Bu kayıtları Azure DNS tarafından sağlanan ad sunucusu adlarını içerir. Kayıt verileri tarafından alınan bölge dosyasında yer alan değerler yazılmaz.
* Genel Önizleme sırasında Azure DNS yalnızca tek dize TXT kaydı destekler. Çok dizeli TXT kayıtlarının birleştirilmiş ve 255 karakter olarak kesildi.

### <a name="cli-format-and-values"></a>CLI biçim ve değerleri

Bir DNS bölgesi almak için Azure CLI komut biçimi şöyledir:

```azurecli
az network dns zone import -g <resource group> -n <zone name> -f <zone file name>
```

Değerler:

* `<resource group>` kaynak grubunun Azure DNS bölgesinde adıdır.
* `<zone name>` Bölge adıdır.
* `<zone file name>` İçeri aktarılacak bölge dosyası yolu/adıdır.

Bu ada sahip bir bölge kaynak grubunda mevcut değilse sizin için oluşturulur. Bölgesi zaten varsa, içeri aktarılan kayıt kümelerini var olan kayıt kümeleri ile birleştirilir. 


### <a name="step-1-import-a-zone-file"></a>1. Adım Bir bölge dosyasını içeri aktarma

Bölge için bir bölge dosyasına aktarmak için **contoso.com**.

1. Zaten yoksa, bir Resource Manager kaynak grubu oluşturmanız gerekir.

    ```azurecli
    az group create --group myresourcegroup -l westeurope
    ```

2. Bölge almak için **contoso.com** dosyasından **contoso.com.txt** kaynak grubunda yeni bir DNS bölgesi içinde **myresourcegroup**, komut çalışır `az network dns zone import` .<BR>Bu komut bölge dosyasını yükler ve onu ayrıştırır. Komutu bir dizi komut bölgesi oluşturmak için Azure DNS hizmeti yürütür ve tüm kayıt bölgeyi ayarlar. Komut konsol penceresinde herhangi bir hata veya uyarı birlikte ilerlemeyi raporlar. Kayıt kümeleri serisinde oluşturulduğundan, büyük bölge dosyasını içeri aktarmak için birkaç dakika sürebilir.

    ```azurecli
    az network dns zone import -g myresourcegroup -n contoso.com -f contoso.com.txt
    ```

### <a name="step-2-verify-the-zone"></a>2. Adım Bölge doğrulayın

Dosyayı içeri aktardıktan sonra DNS bölgesi doğrulamak için aşağıdaki yöntemlerden birini kullanabilirsiniz:

* Aşağıdaki Azure CLI komutunu kullanarak kayıtları listeleyebilirsiniz:

    ```azurecli
    az network dns record-set list -g myresourcegroup -z contoso.com
    ```

* PowerShell cmdlet'ini kullanarak kayıtları listeleyebilirsiniz `Get-AzureRmDnsRecordSet`.
* Kullanabileceğiniz `nslookup` kayıtlar için ad çözümlemesini doğrulayın. Bölge henüz temsilci değil çünkü doğru Azure DNS ad sunucuları açıkça belirtmeniz gerekir. Aşağıdaki örnek, bölgenize atanan ad sunucusu adlarını almak gösterilmiştir. Bu da kullanarak "www" kayıt sorgulama gösterilmektedir `nslookup`.

    ```azurecli
    az network dns record-set ns list -g myresourcegroup -z  --output json 
    ```

    ```json
    [
      {
       .......
       "name": "@",
        "nsRecords": [
          {
            "additionalProperties": {},
            "nsdname": "ns1-03.azure-dns.com."
          },
          {
            "additionalProperties": {},
            "nsdname": "ns2-03.azure-dns.net."
          },
          {
            "additionalProperties": {},
            "nsdname": "ns3-03.azure-dns.org."
          },
          {
            "additionalProperties": {},
            "nsdname": "ns4-03.azure-dns.info."
          }
        ],
        "resourceGroup": "myresourcegroup",
        "ttl": 86400,
        "type": "Microsoft.Network/dnszones/NS"
      }
    ]
    ```

    ```cmd
    nslookup www.contoso.com ns1-03.azure-dns.com

        Server: ns1-01.azure-dns.com
        Address:  40.90.4.1

        Name:www.contoso.com
        Addresses:  134.170.185.46
        134.170.188.221
    ```

### <a name="step-3-update-dns-delegation"></a>3. Adım DNS temsilcisini güncelleştir

Bölge düzgün bir şekilde içeri olduğunu doğruladıktan sonra Azure DNS ad sunucularını işaret edecek şekilde DNS temsilcisini güncelleştirmeniz gerekir. Daha fazla bilgi için bkz: [DNS temsilcisini güncelleştirmesi](dns-domain-delegation.md).

## <a name="export-a-dns-zone-file-from-azure-dns"></a>Azure DNS'yi bir DNS bölge dosyasına dışarı aktarma

Bir DNS bölgesi almak için Azure CLI komut biçimi şöyledir:

```azurecli
az network dns zone export -g <resource group> -n <zone name> -f <zone file name>
```

Değerler:

* `<resource group>` kaynak grubunun Azure DNS bölgesinde adıdır.
* `<zone name>` Bölge adıdır.
* `<zone file name>` dışa aktarılacak bölge dosyası yolu/adıdır.

Olarak bölge alma işleminde, ilk oturum için aboneliğinizi seçin ve Resource Manager modunu kullanmak için Azure CLI yapılandırın.

### <a name="to-export-a-zone-file"></a>Bir bölge dosyasına dışarı aktarmak için

Varolan bir Azure DNS bölgesi dışarı aktarmak için **contoso.com** kaynak grubunda **myresourcegroup** dosyaya **contoso.com.txt** (geçerli), çalışma klasörü `azure network dns zone export`. Bu komutun bölge içindeki kayıt kümelerini numaralandırır ve sonuçları bir BIND uyumlu bölge dosyasına dışarı aktarmak için Azure DNS hizmeti çağırır.

    ```
    az network dns zone export -g myresourcegroup -n contoso.com -f contoso.com.txt
    ```
