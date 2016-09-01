<properties
   pageTitle="CLI kullanarak bir DNS bölgesi oluşturma | Microsoft Azure"
   description="CLI'yi kullanarak DNS etki alanınızı barındırmaya başlamak üzere Azure DNS için DNS bölgelerinin adım adım nasıl oluşturulacağını öğrenin"
   services="dns"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/16/2016"
   ms.author="cherylmc"/>

# CLI kullanarak bir Azure DNS bölgesi oluşturma


> [AZURE.SELECTOR]
- [Azure Portalı](dns-getstarted-create-dnszone-portal.md)
- [PowerShell](dns-getstarted-create-dnszone.md)
- [Azure CLI](dns-getstarted-create-dnszone-cli.md)


Bu makale, CLI kullanarak bir DNS bölgesi oluşturma adımları boyunca size yol gösterir. PowerShell veya Azure portalını kullanarak da bir DNS bölgesi oluşturabilirsiniz. 

[AZURE.INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)] 


## Başlamadan önce

Bu yönergelerde Microsoft Azure CLI kullanılmaktadır. Azure DNS komutlarını kullanmak için en son Azure CLI (0.9.8 veya üstü) sürümüne güncelleştirdiğinizden emin olun. Şu anda bilgisayarınızda hangi Azure CLI sürümünün yüklü olduğunu denetlemek için `azure -v` yazın.

## 1. Adım - Azure CLI'yı ayarlama

### 1. Azure CLI'yı yükleme

Windows, Linux veya Mac için Azure CLI'yı yükleyebilirsiniz. Azure CLI'yı kullanarak Azure DNS'yi yönetmeniz için öncelikle aşağıdaki adımların tamamlanması gerekir. Daha fazla bilgi için bkz. [Azure CLI'yı yükleme](../xplat-cli-install.md). DNS komutları için Azure CLI 0.9.8 veya sonraki bir sürümü gerekir.

CLI'deki tüm ağ sağlayıcı komutları aşağıdaki komut kullanılarak bulunabilir:

    Azure network

### 2. CLI moduna geçme

Azure DNS, Azure Resource Manager'ı kullanır. ARM komutlarını kullanmak için CLI moduna geçtiğinizden emin olun.

    Azure config mode arm

### 3. Azure hesabınızda oturum açma

Kimlik bilgilerinizle kimliğinizi doğrulamanız istenir. Yalnızca ORGID hesaplarını kullanabileceğinizi göz önünde bulundurun.

    Azure login -u "username"

### 4. Aboneliği seçme

Hangi Azure aboneliğinizin kullanılacağını seçin.

    Azure account set "subscription name"

### 5. Kaynak grubu oluşturma

Azure Resource Manager, tüm kaynak gruplarının bir konum belirtmesini gerektirir. Bu, kaynak grubunda kaynaklar için varsayılan konum olarak kullanılır. Ancak tüm DNS kaynakları bölgesel değil de global olduğundan, kaynak grubu konumu seçiminin Azure DNS üzerinde hiçbir etkisi yoktur.

Var olan bir kaynak grubunu kullanıyorsanız bu adımı atlayabilirsiniz. 

    Azure group create -n myresourcegroup --location "West US"


### 6. Kaydolma

Azure DNS hizmeti, Microsoft.Network kaynak sağlayıcısı tarafından yönetilir. Azure DNS'yi kullanmadan önce, Azure aboneliğinizin bu kaynak sağlayıcısını kullanmak için kayıtlı olması gerekir. Bu, her bir abonelik için tek seferlik bir işlemdir.

    Azure provider register --namespace Microsoft.Network


## 2. Adım - Bir DNS bölgesi oluşturma

DNS bölgesi, `azure network dns zone create` komutu kullanılarak oluşturulur. İsteğe bağlı olarak bir DNS bölgesini etiketlerle birlikte oluşturabilirsiniz. Etiketler, bir ad-değer çifti listesidir ve Azure Resource Manager tarafından faturalama veya gruplandırma amaçları için kaynakları etiketlemek üzere kullanılır. Etiketler hakkında daha fazla bilgi için bkz. [Etiketleri kullanarak Azure kaynaklarınızı düzenleme](../resource-group-using-tags.md). 

Azure DNS'de bölge adları, sonlandıran **"."** işareti olmadan belirtilmelidir. Örneğin, "**contoso.com.**" yerine "**contoso.com**" kullanılmalıdır.


### Bir DNS bölgesi oluşturmak için

Aşağıdaki örnek, *MyResourceGroup* adlı kaynak grubunda *contoso.com* adlı bir DNS bölgesi oluşturur. 

Değerleri kendinizinkilerle değiştirerek DNS bölgenizi oluşturmak için örneği kullanın.

    Azure network dns zone create myresourcegroup contoso.com

### Bir DNS bölgesi ve etiketler oluşturma.

Azure DNS CLI'si, isteğe bağlı *-Tag* parametresi kullanılarak belirtilen DNS bölgelerinin etiketlerini destekler. Aşağıdaki örnek, project = demo ve env = test şeklindeki iki etiketle bir DNS bölgesinin nasıl oluşturulacağını gösterir.

Değerleri kendinizinkilerle değiştirerek bir DNS bölgesi ve etiket oluşturmak için aşağıdaki örneği kullanın.

    Azure network dns zone create myresourcegroup contoso.com -t "project=demo";"env=test"

## Kayıtları görüntüleme

Bir DNS bölgesinin oluşturulması aşağıdaki DNS kayıtlarını da oluşturur:

- "Yetki Başlangıcı" (SOA) kaydı. Bu, her DNS bölgesinin kökünde bulunur.

- Yetkili ad sunucusu (NS) kayıtları. Bunlar, bölgeyi hangi ad sunucularının barındırdığını gösterir. Azure DNS bir ad sunucuları havuzu kullanır ve böylece farklı ad sunucuları Azure DNS'deki farklı bölgelere atanabilir. Daha fazla bilgi için bkz. [Azure DNS'ye bir etki alanı devretme](dns-domain-delegation.md) 

Bu kayıtları görüntülemek için `azure network dns-record-set show` kullanın.<BR>
*Kullanım: network dns record-set show <resource-group> <dns-zone-name> <name> <type>*


Aşağıdaki örnekte, komutu *myresourcegroup* kaynak grubu, *"@"* kayıt kümesi adı (bir kök kaydı için) ile çalıştırır ve *SOA* yazarsanız aşağıdaki çıkış sunulur:


    azure network dns record-set show myresourcegroup "contoso.com" "@" SOA
    info:    Executing command network dns-record-set show
    + Looking up the DNS record set "@"
    data:    Id                              : /subscriptions/#######################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/SOA/@
    data:    Name                            : @
    data:    Type                            : Microsoft.Network/dnszones/SOA
    data:    Location                        : global
    data:    TTL                             : 3600
    data:    SOA record:
    data:      Email                         : msnhst.microsoft.com
    data:      Expire time                   : 604800
    data:      Host                          : edge1.azuredns-cloud.net
    data:      Minimum TTL                   : 300
    data:      Refresh time                  : 900
    data:      Retry time                    : 300
    data:                                    :
<BR>
Bölge ile oluşturulan NS kayıtlarını görüntülemek için aşağıdaki komutu kullanın:

    azure network dns record-set show myresourcegroup "contoso.com" "@" NS
    info:    Executing command network dns-record-set show
    + Looking up the DNS record set "@"
    data:    Id                              : /subscriptions/#######################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/NS/@
    data:    Name                            : @
    data:    Type                            : Microsoft.Network/dnszones/NS
    data:    Location                        : global
    data:    TTL                             : 3600
    data:    NS records
    data:        Name server domain name     : ns1-05.azure-dns.com
    data:        Name server domain name     : ns2-05.azure-dns.net
    data:        Name server domain name     : ns3-05.azure-dns.org
    data:        Name server domain name     : ns4-05.azure-dns.info
    data:
    info:    network dns-record-set show command OK

>[AZURE.NOTE] Bir DNS Bölgesinin kökündeki (veya *tepesindeki*) kayıt kümeleri, kayıt kümesinin adı olarak **@** kullanır.

## Test etme

DNS bölgenizi test etmek için nslookup, DIG veya `Resolve-DnsName` PowerShell cmdlet gibi DNS araçlarını kullanabilirsiniz.

Azure DNS'de yeni bölgeyi kullanmak için etki alanınızı henüz devretmediyseniz DNS sorgusunu bölgenizin ad sunucularından birine doğrudan yönlendirmeniz gerekir. Bölgenizin ad sunucuları, yukarıda "azure network dns record-set show" olarak listelenip NS kayıtlarında verilir. Aşağıdaki komutta bölgeniz için doğru değerleri değiştirdiğinizden emin olun.

Aşağıdaki örnek, DNS bölgesi için atanmış ad sunucularını kullanarak domain.contoso.com sorgulamak için DIG kullanır. Sorgunun bir ad sunucusuna işaret etmesi gerekiyor ve bunun için DIG kullanarak *@<name server for the zone>* ve bölge adını kullandık.

     <<>> DiG 9.10.2-P2 <<>> @ns1-05.azure-dns.com contoso.com
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
    contoso.com.         300     IN      SOA     edge1.azuredns-cloud.net.
    msnhst.microsoft.com. 6 900 300 604800 300

    Query time: 93 msec
    SERVER: 208.76.47.5#53(208.76.47.5)
    WHEN: Tue Jul 21 16:04:51 Pacific Daylight Time 2015
    MSG SIZE  rcvd: 120

## Sonraki adımlar

Bir DNS bölgesi oluşturduktan sonra, İnternet etki alanınız için ad çözümlemesini başlatmak amacıyla [kayıt kümeleri ve kayıtlar](dns-getstarted-create-recordset-cli.md) oluşturun.



<!--HONumber=Aug16_HO4-->


