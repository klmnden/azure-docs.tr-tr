---
title: İçeri aktarma ve Azure CLI kullanarak Azure DNS'ye bir etki alanı bölgesi dosyası dışarı | Microsoft Docs
description: İçeri aktarma ve Azure CLI kullanarak Azure DNS ile DNS bölge dosyasını dışarı aktarma hakkında bilgi edinin
services: dns
author: vhorne
ms.service: dns
ms.date: 4/3/2019
ms.author: victorh
ms.topic: conceptual
ms.openlocfilehash: b65b70e7a994d7d49b2282d7e193fe6e7b84cfca
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67612764"
---
# <a name="import-and-export-a-dns-zone-file-using-the-azure-cli"></a>Azure CLI kullanarak DNS bölge dosyasını içeri ve dışarı

Bu makalede, alma ve Azure CLI kullanarak Azure DNS için DNS bölge dosyalarının verme konusunda yol göstermektedir.

## <a name="introduction-to-dns-zone-migration"></a>DNS bölgesi geçişe giriş

DNS bölge dosyasını bölgedeki her etki alanı adı sistemi (DNS) kaydı ayrıntılarını içeren bir metin dosyasıdır. DNS kayıtlarını DNS sistemler arasında aktarmak için uygun hale getirme standart bir biçimdedir. Bir bölge dosyası içine veya dışına Azure DNS, DNS bölgesi aktarmak için hızlı, güvenilir ve uygun şekilde kullanmaktır.

Azure DNS, alma ve Azure komut satırı arabirimi (CLI) kullanarak bölge dosyalarının verme destekler. Bölge dosyasını içeri aktarma **değil** Azure PowerShell veya Azure portalı şu anda desteklenmiyor.

Azure CLI'yı Azure hizmetlerini yönetmek için kullanılan bir platformlar arası komut satırı aracıdır. Windows, Mac ve Linux platformlarını için kullanılabilir [Azure indirme sayfası](https://azure.microsoft.com/downloads/). Platformlar arası destek, içeri aktarma ve bölge dosyalarını dışarı aktarmak için önemlidir çünkü en sık kullanılan adı sunucu yazılımı [bağlama](https://www.isc.org/downloads/bind/), genellikle Linux üzerinde çalışır.

## <a name="obtain-your-existing-dns-zone-file"></a>Var olan, DNS bölge dosyasını edinin

Azure DNS ile DNS bölge dosyasını içeri aktarmadan önce bölge dosyasının bir kopyasını elde etmek gerekir. DNS bölgesi şu anda barındırıldığı üzerindeki bu dosyanın kaynağına bağlıdır.

* DNS bölgenizi (örneğin, etki alanı kayıt şirketi, özel DNS barındırma sağlayıcısı veya diğer bulut sağlayıcısı) bir iş ortağı hizmeti tarafından barındırılıyorsa, bu hizmetin DNS bölge dosyasını indirme olanağı sağlamanız gerekir.
* DNS bölgenizi Windows DNS üzerinde barındırılıyorsa, bölge dosyaları için varsayılan klasördür **%systemroot%\system32\dns**. Her bölge dosyasının tam yolunu da gösterir **genel** DNS konsolunun sekmesi.
* DNS bölgenizi bağlama kullanarak tarafından barındırılıyorsa, her bölge için bölgesi dosyasının konumu bağlama yapılandırma dosyasında belirtilen **named.conf**.

## <a name="import-a-dns-zone-file-into-azure-dns"></a>Azure DNS ile DNS bölge dosyasını içeri aktarma

Bölge dosyasını içeri biri zaten mevcut değilse yeni bir bölgeye, Azure DNS'de oluşturur. Bölge zaten varsa, kayıt kümelerini bölge dosyasındaki var olan kayıt kümeleri birleştirilmesi gerekir.

### <a name="merge-behavior"></a>Birleştirme davranışı

* Varsayılan olarak, mevcut ve yeni kayıt kümelerini birleştirilir. Birleştirilmiş kayıt kümesi içinde aynı kayıt XML'deki yinelenen.
* Kayıt kümelerini birleştirildiğinde, yaşam süresi (TTL) önceden var olan kayıt kümeleri kullanılır.
* Yetki (SOA) parametreleri başlangıcı (dışında `host`) her zaman alınan bölge dosyasından alınmıştır. Benzer şekilde, bölge tepesinde ayarlamak ad sunucusu kaydı için TTL her zaman alınan bölge dosyasından alınır.
* İçeri aktarılan bir CNAME kaydı, aynı ada sahip mevcut bir CNAME kaydı değiştirmez.  
* CNAME kaydı ve başka bir kayıtla aynı ada ancak farklı türde (bağımsız olarak, mevcut veya yeni) arasında bir çakışma ortaya çıkar, varolan bir kaydı tutulur. 

### <a name="additional-information-about-importing"></a>İçeri aktarma hakkında ek bilgi

Aşağıdaki notlar, bölgeye ilişkin ek teknik ayrıntılar alma işlemi sunar.

* `$TTL` Yönergesi, isteğe bağlıdır ve desteklenir. Hiçbir `$TTL` yönergesi verilmiştir, açık bir TTL olmadan kayıtlarını içeri aktarılan kümesine bir varsayılan TTL değeri 3600 saniyedir. Aynı kayıt kümesindeki iki kaydı farklı TTL'ler belirttiğinizde, daha düşük değer kullanılır.
* `$ORIGIN` Yönergesi, isteğe bağlıdır ve desteklenir. Hiçbir `$ORIGIN` , kullanılan varsayılan değer: komut satırında belirtilen bölge adı ayarlanmış (sonlandırma artı ".").
* `$INCLUDE` Ve `$GENERATE` yönergeleri desteklenmez.
* Bu kayıt türleri desteklenir: A, AAAA, CAA, CNAME, MX, NS, SOA, SRV ve TXT.
* Bir bölge oluşturulduğunda SOA kaydı, Azure DNS tarafından otomatik olarak oluşturulur. Bölge dosyasını içeri aktardığınızda, tüm SOA parametreleri bölge dosyasından alınmıştır *dışında* `host` parametresi. Bu parametre, Azure DNS tarafından sağlanan değer kullanır. Bu parametre, Azure DNS tarafından sağlanan birincil ad sunucusuna başvurmalıdır olmasıdır.
* Bölge oluşturulduğunda bölgenin tepesinde ayarlamak ad sunucusu kaydı ayrıca Azure DNS tarafından otomatik olarak oluşturulur. Yalnızca bu kayıt kümesinin TTL aktarılır. Bu kayıtlar, Azure DNS tarafından sağlanan ad sunucusu adlarını içerir. Kayıt verilerini tarafından alınan bölge dosyasında yer alan değerleri yazılmaz.
* Genel Önizleme süresince Azure DNS TXT kayıtlarının yalnızca tek dize destekler. Çok dizeli TXT kayıtları birleştirilmiş ve 255 karakter olarak kesildi.

### <a name="cli-format-and-values"></a>CLI biçimi ve değerleri

Bir DNS bölgesi almak için Azure CLI komut biçimi şöyledir:

```azurecli
az network dns zone import -g <resource group> -n <zone name> -f <zone file name>
```

Değerler:

* `<resource group>` Azure DNS'de bölge için kaynak grubunun adı var.
* `<zone name>` Bölge adıdır.
* `<zone file name>` Bölge dosyasını içeri aktarılacak yol/adıdır.

Bu ada sahip bir bölge kaynak grubunda mevcut değilse sizin için oluşturulur. Bölge zaten varsa, içeri aktarılan kayıt kümeleri mevcut kayıt kümeleri ile birleştirilir. 

### <a name="step-1-import-a-zone-file"></a>1\.Adım Bölge dosyasını içeri aktarma

Bölge için bir bölge dosyasını içeri aktarmak için **contoso.com**.

1. Zaten yoksa, bir Resource Manager kaynak grubu oluşturmanız gerekir.

    ```azurecli
    az group create --group myresourcegroup -l westeurope
    ```

2. Bölgeyi almak için **contoso.com** dosyasından **contoso.com.txt** kaynak grubunda yeni bir DNS bölgesi halinde **myresourcegroup**, komut çalışır `az network dns zone import` .<BR>Bu komut, bölge dosyasını yükler ve onu ayrıştırır. Azure DNS hizmeti bölgesi oluşturmak için bir dizi komut komutu yürütür ve tüm kayıt bölgeyi ayarlar. Komut, tüm hata ve uyarıların yanı sıra konsol penceresinde ilerlemeyi raporlar. Kayıt kümelerini dizisinde oluşturulduğundan, bu büyük bölge dosyasını içeri aktarmak için birkaç dakika sürebilir.

    ```azurecli
    az network dns zone import -g myresourcegroup -n contoso.com -f contoso.com.txt
    ```

### <a name="step-2-verify-the-zone"></a>2\.Adım Bölge doğrulayın

DNS bölgesi dosyasını içeri aktardıktan sonra doğrulamak için aşağıdaki yöntemlerden birini kullanabilirsiniz:

* Aşağıdaki Azure CLI komutunu kullanarak kayıtları listeleyebilirsiniz:

    ```azurecli
    az network dns record-set list -g myresourcegroup -z contoso.com
    ```

* Azure CLI komutunu kullanarak kayıtları listeleyebilirsiniz `az network dns record-set ns list`.
* Kullanabileceğiniz `nslookup` kayıtlar için ad çözümlemesini doğrulayın. Bölge henüz temsilci değildir çünkü doğru Azure DNS ad sunucularını açıkça belirtmeniz gerekir. Aşağıdaki örnek bölgesi için atanmış ad sunucusu adlarını almak nasıl gösterir. Ayrıca bu sorguyu kullanarak "www" kayıt işlemini gösterir `nslookup`.

    ```azurecli
    az network dns record-set ns list -g myresourcegroup -z contoso.com  --output json 
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

### <a name="step-3-update-dns-delegation"></a>Adım 3. DNS temsilcisini güncelleştir

Bölge doğru şekilde içeri aktarıldığını doğruladıktan sonra Azure DNS ad sunucularına işaret edecek şekilde DNS temsilcisini güncelleştirmeniz gerekiyor. Daha fazla bilgi için bkz [DNS temsilcini güncelleştirmeyi](dns-domain-delegation.md).

## <a name="export-a-dns-zone-file-from-azure-dns"></a>DNS bölge dosyasını Azure DNS'den dışarı aktarma

Bir DNS bölgesi dışarı aktarmak için Azure CLI komut biçimi şöyledir:

```azurecli
az network dns zone export -g <resource group> -n <zone name> -f <zone file name>
```

Değerler:

* `<resource group>` Azure DNS'de bölge için kaynak grubunun adı var.
* `<zone name>` Bölge adıdır.
* `<zone file name>` dışa aktarılacak bölge dosyası yolu/adıdır.

Olarak bölge içeri ile ilk oturum açma için aboneliğinizi seçin ve Resource Manager modunu kullanmak için Azure CLI'yı yapılandırın.

### <a name="to-export-a-zone-file"></a>Bölge dosyasını dışarı aktarmak için

Mevcut Azure DNS bölgesi dışarı aktarmak için **contoso.com** kaynak grubundaki **myresourcegroup** dosyasına **contoso.com.txt** (geçerli), çalışma klasörü `azure network dns zone export`. Bu komut, bölgedeki kayıt kümelerini listeleme ve sonuçları bir bağlama ile uyumlu bölge dosyasına dışarı aktarmak için Azure DNS hizmeti çağırır.

```
az network dns zone export -g myresourcegroup -n contoso.com -f contoso.com.txt
```

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi edinmek için nasıl [kayıt kümeleri ve kayıtları yönetmek](dns-getstarted-create-recordset-cli.md) DNS bölgenizdeki.

* Bilgi edinmek için nasıl [etki alanınızı Azure DNS'ye devretme](dns-domain-delegation.md).
