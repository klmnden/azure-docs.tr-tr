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
ms.date: 12/05/2016
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: bfbffe7843bc178cdf289c999925c690ab82e922
ms.openlocfilehash: 5bbd490925e5e25f10044af55af49daa494ee026

---

# <a name="create-an-azure-dns-zone-using-cli"></a>CLI kullanarak bir Azure DNS bölgesi oluşturma

> [!div class="op_single_selector"]
> * [Azure Portal](dns-getstarted-create-dnszone-portal.md)
> * [PowerShell](dns-getstarted-create-dnszone.md)
> * [Azure CLI](dns-getstarted-create-dnszone-cli.md)

Bu makale Windows, Mac ve Linux platformlarında kullanılabilen çok platformlu Azure CLI'yi kullanarak DNS bölgesi oluşturma adımlarında size rehberlik yapacaktır. PowerShell veya Azure portalını kullanarak da bir DNS bölgesi oluşturabilirsiniz.

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]


## <a name="before-you-begin"></a>Başlamadan önce

Yapılandırmanıza başlamadan önce aşağıdaki öğelerin bulunduğunu doğrulayın.

* Azure aboneliği. Henüz Azure aboneliğiniz yoksa [MSDN abonelik avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.
* Windows, Linux veya MAC platformlarında kullanılabilen Azure CLI'nin son sürümünü yüklemeniz gerekir. Daha fazla bilgi için bkz. [Azure CLI'yı yükleme](../xplat-cli-install.md).

## <a name="step-1---sign-in-and-create-a-resource-group"></a>1. Adım - Oturum açma ve bir kaynak grubu oluşturma

### <a name="switch-cli-mode"></a>CLI moduna geçme

Azure DNS, Azure Resource Manager'ı kullanır. ARM komutlarını kullanmak için CLI moduna geçtiğinizden emin olun.

```azurecli
azure config mode arm
```

### <a name="sign-in-to-your-azure-account"></a>Azure hesabınızda oturum açma

Kimlik bilgilerinizle kimliğinizi doğrulamanız istenir. Yalnızca OrgID hesaplarını kullanabileceğinizi göz önünde bulundurun.

```azurecli
azure login
```

### <a name="select-the-subscription"></a>Aboneliği seçme

Hesapla ilişkili abonelikleri kontrol edin.

```azurecli
azure account list
```

Hangi Azure aboneliğinizin kullanılacağını seçin.

```azurecli
azure account set "subscription name"
```

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure Resource Manager, tüm kaynak gruplarının bir konum belirtmesini gerektirir. Bu, kaynak grubunda kaynaklar için varsayılan konum olarak kullanılır. Ancak tüm DNS kaynakları bölgesel değil de global olduğundan, kaynak grubu konumu seçiminin Azure DNS üzerinde hiçbir etkisi yoktur.

Var olan bir kaynak grubunu kullanıyorsanız bu adımı atlayabilirsiniz.

```azurecli
azure group create -n myresourcegroup --location "West US"
```

### <a name="register-resource-provider"></a>Kaynak sağlayıcısını kaydetme

Azure DNS hizmeti, Microsoft.Network kaynak sağlayıcısı tarafından yönetilir. Azure DNS'yi kullanmadan önce, Azure aboneliğinizin bu kaynak sağlayıcısını kullanmak için kayıtlı olması gerekir. Bu, her bir abonelik için tek seferlik bir işlemdir.

```azurecli
azure provider register --namespace Microsoft.Network
```

## <a name="step-2---create-a-dns-zone"></a>2. Adım - Bir DNS bölgesi oluşturma

DNS bölgesi, `azure network dns zone create` komutu kullanılarak oluşturulur. Bu komutla ilgili yardım içeriğini görmek için `azure network dns zone create -h` yazın.

Aşağıdaki örnek, *MyResourceGroup* adlı kaynak grubunda *contoso.com* adlı bir DNS bölgesi oluşturur. Değerleri kendinizinkilerle değiştirerek DNS bölgesini oluşturmak için örneği kullanın.

```azurecli
azure network dns zone create MyResourceGroup contoso.com
```

## <a name="step-3---verify"></a>3. Adım - Doğrulama

### <a name="view-records"></a>Kayıtları görüntüleme

Bir DNS bölgesinin oluşturulması aşağıdaki DNS kayıtlarını da oluşturur:

* 'Yetki Başlangıcı' (SOA) kaydı. Bu, her DNS bölgesinin kökünde bulunur.
* Yetkili ad sunucusu (NS) kayıtları. Bunlar, bölgeyi hangi ad sunucularının barındırdığını gösterir. Azure DNS bir ad sunucuları havuzu kullanır ve böylece farklı ad sunucuları Azure DNS'deki farklı bölgelere atanabilir. Daha fazla bilgi için bkz. [Azure DNS'ye bir etki alanı devretme](dns-domain-delegation.md) 

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

Azure DNS'de henüz yeni bölgeyi kullanmak için etki alanınızı devretmediyseniz DNS sorgusunu bölgenizin ad sunucularından birine doğrudan yönlendirmeniz gerekir. Bölgenizin ad sunucuları, yukarıda "azure network dns record-set show" olarak listelenip NS kayıtlarında verilir. Aşağıdaki komutta bölgeniz için doğru değerleri değiştirdiğinizden emin olun.

Aşağıdaki örnek, DNS bölgesi için atanmış ad sunucularını kullanarak domain.contoso.com sorgulamak için "dig" kullanır. Sorgunun bir ad sunucusuna işaret etmesi gerekiyor ve bunun için "dig" kullanarak *@\<bölge için ad sunucusu\>* ve bölge adlandırmasını kullandık.

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

## <a name="next-steps"></a>Sonraki adımlar

Bir DNS bölgesi oluşturduktan sonra, İnternet etki alanınız için DNS kayıtlarını oluşturmak amacıyla [kayıt kümeleri ve kayıtlar](dns-getstarted-create-recordset-cli.md) oluşturun.




<!--HONumber=Dec16_HO2-->


