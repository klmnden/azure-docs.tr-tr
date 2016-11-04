---
title: Etki alanınızı Azure DNS'ye devretme | Microsoft Docs
description: Etki alanı temsilcisi seçiminin nasıl değiştirileceğini ve etki alanı barındırma sağlamak üzere Azure DNS ad sunucularının nasıl kullanılacağını anlayın.
services: dns
documentationcenter: na
author: sdwheeler
manager: carmonm
editor: ''

ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/30/2016
ms.author: sewhee

---
# Azure DNS'ye bir etki alanı devretme
Azure DNS, bir DNS bölgesi barındırmanızı ve Azure'de bir etki alanı için DNS kayıtlarını yönetmenizi sağlar. Bir etki alanının DNS sorgularının Azure DNS'ye erişmesi için, etki alanının Azure DNS'ye üst etki alanından devredilmiş olması gerekir. Azure DNS'nin etki alanı kayıt şirketi olmadığını unutmayın. Bu makalede, etki alanı temsilcisi seçmenin nasıl çalıştığı ve etki alanlarının Azure DNS'ye nasıl devredildiği açıklanmaktadır.

## DNS temsilcisi seçme nasıl çalışır?
### Etki alanları ve bölgeler
Etki Alanı Adı Sistemi, bir etki alanları hiyerarşisidir. Hiyerarşi, adı yalnızca "**.**" olan "kök" etki alanından başlar.  Bunun altında "com", "net", "org", "uk" veya "jp" gibi en üst düzey etki alanları bulunur.  Bunların altında "org.uk" veya "co.jp" gibi ikinci düzey etki alanları bulunur.  Etki alanları bu hiyerarşi sıralamasıyla devam eder. DNS hiyerarşisindeki etki alanları, ayrı DNS bölgeleri kullanılarak barındırılır. Bu bölgeler global olarak dağıtılır ve dünya genelindeki DNS ad sunucuları tarafından barındırılır.

**DNS bölgesi**

Etki alanı, Etki Alanı Adı Sistemi'nde yer alan "contoso.com" gibi benzersiz bir addır. DNS bölgesi, belirli bir etki alanına ait DNS kayıtlarını barındırmak için kullanılır. Örneğin, "contoso.com" etki alanı, "mail.contoso.com" (bir posta sunucusu için) ve "www.contoso.com" (bir web sitesi için) gibi bir dizi DNS kaydını içerebilir.

**Etki alanı kayıt şirketi**

Etki alanı kayıt şirketi, İnternet etki alanı adlarını sağlayabilen bir şirkettir. Bu şirketler kullanmak istediğiniz İnternet etki alanının kullanılabilir olup olmadığını doğrular ve bu etki alanını satın almanızı sağlar. Etki alanı adı kaydedildikten sonra, etki alanı adının yasal sahibi olursunuz. Bir İnternet etki alanına zaten sahipseniz Azure DNS'ye devretmek için geçerli etki alanı kayıt şirketini kullanırsınız.

> [!NOTE]
> Belirli bir etki alanı adının kime ait olduğu veya bir etki alanını satın alma hakkında daha fazla bilgi edinmek için bkz. [Azure AD'de İnternet etki alanı yönetimi](https://msdn.microsoft.com/library/azure/hh969248.aspx).
> 
> 

### Çözümleme ve temsilci seçme
İki tür DNS sunucusu bulunur:

* *Yetkili* DNS sunucusu DNS bölgelerini barındırır. Bu sunucu, yalnızca bu bölgelerdeki kayıtlar için DNS sorgularını yanıtlar.
* *Özyinelemeli* DNS sunucusu DNS bölgelerini barındırmaz. Bu sunucu, tüm DNS sorgularını yanıtlamak için yetkili DNS sunucularını çağırarak ihtiyacı olan verileri toplar.

> [!NOTE]
> Azure DNS, bir yetkili DNS hizmeti sağlar.  Bir özyinelemeli DNS hizmeti sağlamaz.
> 
> Azure’daki Bulut Hizmetleri ve Sanal Makineler, Azure altyapısının bir parçası olarak ayrıca sağlanan, özyinelemeli bir DNS hizmetini kullanacak şekilde otomatik olarak yapılandırılmıştır.  Bu DNS ayarlarını değiştirme hakkında bilgi edinmek için bkz. [Azure’da Ad Çözümlemesi](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).
> 
> 

Bilgisayarlar veya mobil cihazlarda bulunan DNS istemcileri, istemci uygulamaları için gereken tüm DNS sorgularını gerçekleştirmek için genellikle özyinelemeli bir DNS sunucusu çağırır.

Özyinelemeli bir DNS sunucusu "www.contoso.com" gibi bir DNS kaydı için bir sorguyu aldığında öncelikle "contoso.com" etki alanı için bölgeyi barındıran ad sunucusunu bulması gerekir. Bunu yapmak için kök adı sunucularından başlar ve buradan "com" bölgesini barındıran ad sunucularını bulur. Ardından, "contoso.com" bölgesini barındıran ad sunucularını bulmak için "com" ad sunucularını sorgular.  Son olarak, bu ad sunucularını "www.contoso.com" için sorgulayabilir.

Bu işlem DNS adını çözümleme olarak adlandırılır. Net olarak ifade etmek gerekirse DNS çözümlemesi CNAME'leri izleme gibi ek adımları içerir ancak DNS temsilcisi seçmenin nasıl çalıştığını anlamak için bu önemli değildir.

Bir üst bölge, bir alt bölgenin ad sunucularına nasıl "işaret eder"? Bunu, NS kaydı olarak adlandırılan (NS "ad sunucusu" anlamına gelir) özel bir DNS kaydı türünü kullanarak yapar. Örneğin, kök bölgesi "com" için NS kayıtları içerir ve "com" bölgesi için ad sunucularını gösterir. Buna karşılık "com" bölgesi, "contoso.com" bölgesi için ad sunucularını gösteren "contoso.com"un NS kayıtlarını içerir. Bir üst bölge içindeki bir alt bölge için NS kayıtlarının ayarlamasına etki alanını devretme adı verilir.

![Dns-nameserver](./media/dns-domain-delegation/image1.png)

Her temsilci seçimi aslında NS kayıtlarının iki kopyasını içerir, bunlardan biri üst bölgede bulunup alt bölgeyi işaret ederken diğeri de alt bölgede yer alır. "Contoso.com" bölgesi, "contoso.com"a ait NS kayıtlarını içerir ("com"daki NS kayıtlarına ek olarak). Bunlar yetkili NS kayıtları olarak adlandırılır ve alt bölgenin tepesinde durur.

## Azure DNS'ye bir etki alanı devretme
Azure DNS'de DNS bölgenizi oluşturduktan sonra, Azure DNS'yi bölgenizin ad çözümlemesinin yetkili kaynağı yapmak için üst bölgedeki NS kayıtlarını ayarlamanız gerekir. Bir kayıt şirketinden satın alınan etki alanları için, kayıt şirketiniz bu NS kayıtlarını ayarlama seçeneğini sunar.

> [!NOTE]
> Azure DNS'de bir etki alanı adını kullanarak DNS bölgesi oluşturmak için bu etki alanına sahip olmanız gerekmez. Ancak Azure DNS'ye temsilci seçmeyi kayıt şirketi ile ayarlamak için etki alanına sahip olmanız gerekir.
> 
> 

Örneğin, "contoso.com" etki alanını satın aldığınızı ve Azure DNS'de "contoso.com" adlı bir bölge oluşturduğunuzu varsayalım. Etki alanı sahibi olarak, kayıt şirketiniz size etki alanınız için ad sunucusu adreslerini (yani NS kayıtlarını) yapılandırma seçeneğini sunar. Kayıt şirketi bu NS kayıtlarını üst etki alanında (bu durumda ".com"da) depolar. Ardından, dünya genelindeki istemciler "contoso.com"daki DNS kayıtlarını çözümlemeye çalışırken Azure DNS bölgesindeki etki alanınıza yönlendirilir.

### Ad sunucusu adlarını bulma
DNS bölgenizi Azure DNS'ye devretmeden önce, bölgenizin ad sunucusu adlarını bilmeniz gerekir. Azure DNS, her bölge oluşturmada bir havuzdan ad sunucuları ayırır.

Azure portalı, bölgenize atanan ad sunucularını görmenin en kolay yoludur.  Bu örnekte, "contoso.net" bölgesine "ns1-01.azure-dns.com", "ns2-01.azure-dns.net", "ns3-01.azure-dns.org" ve "ns4-01.azure-dns.info" ad sunucuları atanmıştır:

 ![Dns-nameserver](./media/dns-domain-delegation/viewzonens500.png)

Azure DNS, atanan ad sunucularını içeren yetkili NS kayıtlarını bölgenizde otomatik olarak oluşturur.  Ad sunucusu adlarını Azure PowerShell veya Azure CLI aracılığıyla görmek için bu kayıtları almanız yeterlidir.

Azure PowerShell'i kullanarak, yetkili NS kayıtları şu şekilde alınabilir. "@" kayıt adının, bölgenin tepesindeki kayıtları ifade etmek için kullanıldığını unutmayın.

    PS> $zone = Get-AzureRmDnsZone –Name contoso.net –ResourceGroupName MyResourceGroup
    PS> Get-AzureRmDnsRecordSet –Name “@” –RecordType NS –Zone $zone

    Name              : @
    ZoneName          : contoso.net
    ResourceGroupName : MyResourceGroup
    Ttl               : 3600
    Etag              : 5fe92e48-cc76-4912-a78c-7652d362ca18
    RecordType        : NS
    Records           : {ns1-01.azure-dns.com, ns2-01.azure-dns.net, ns3-01.azure-dns.org,
                        ns4-01.azure-dns.info}
    Tags              : {}

Yetkili NS kayıtlarını almak ve böylelikle bölgenize atanan ad sunucularını keşfetmek için platformlar arası Azure CLI'yı kullanabilirsiniz:

    C:\> azure network dns record-set show MyResourceGroup contoso.net @ NS
    info:    Executing command network dns record-set show
        + Looking up the DNS Record Set "@" of type "NS"
    data:    Id                              : /subscriptions/.../resourceGroups/MyResourceGroup/providers/Microsoft.Network/dnszones/contoso.net/NS/@
    data:    Name                            : @
    data:    Type                            : Microsoft.Network/dnszones/NS
    data:    Location                        : global
    data:    TTL                             : 172800
    data:    NS records
    data:        Name server domain name     : ns1-01.azure-dns.com.
    data:        Name server domain name     : ns2-01.azure-dns.net.
    data:        Name server domain name     : ns3-01.azure-dns.org.
    data:        Name server domain name     : ns4-01.azure-dns.info.
    data:
    info:    network dns record-set show command OK

### Temsilci seçmeyi ayarlama
Her kayıt şirketi, bir etki alanının ad sunucusu kayıtlarını değiştirmek için kendi DNS yönetim araçlarına sahiptir. Kayıt şirketinin DNS yönetim sayfasında NS kayıtlarını düzenleyin ve NS kayıtlarını Azure DNS'nin oluşturduklarıyla değiştirin.

Bir etki alanını Azure DNS'ye devrederken Azure DNS tarafından sağlanan ad sunucusu adlarını kullanmanız gerekir.  Etki alanınızın adından bağımsız olarak her zaman 4 sunucu adını da kullanmanız gerekir.  Etki alanı temsilcisi, sunucu adının etki alanınızla aynı üst düzey etki alanını kullanmasını gerektirmez.

Azure DNS ad sunucusu IP adresleri gelecekte değişebileceği için, bu IP adreslerine işaret ederken "birleştirici kayıtlar"ı kullanmamanız gerekir. Kendi bölgenizdeki ad sunucusu adlarını kullanan ve bazen "gösterim ad sunucuları" olarak adlandırılan temsilci seçimleri, Azure DNS'de şu anda desteklenmemektedir.

### Ad çözümlemesinin çalıştığını doğrulama
Temsilci seçmeyi tamamladıktan sonra, bölgenizin SOA kaydını (bölge oluşturulduğunda bu da otomatik olarak oluşturulur) sorgulamak için "nslookup" gibi bir araç kullanarak ad çözümlemesinin çalışıp çalışmadığını doğrulayabilirsiniz.

Temsilci seçme doğru şekilde ayarlandığında normal DNS çözümleme işlemi ad sunucularını otomatik olarak bulur, bu nedenle Azure DNS ad sunucularını belirtmek zorunda olmadığınızı unutmayın.

    nslookup –type=SOA contoso.com

    Server: ns1-04.azure-dns.com
    Address: 208.76.47.4

    contoso.com
    primary name server = ns1-04.azure-dns.com
    responsible mail addr = msnhst.microsoft.com
    serial = 1
    refresh = 900 (15 mins)
    retry = 300 (5 mins)
    expire = 604800 (7 days)
    default TTL = 300 (5 mins)

## Azure DNS'de alt etki alanlarını devretme
Ayrı bir alt bölge kurmak istiyorsanız Azure DNS'de bir alt etki alanını devredebilirsiniz. Örneğin, Azure DNS'de ayarladığınız ve devrettiğiniz "contoso.com" için "partners.contoso.com" olarak ayrı bir alt bölge ayarlamak istediğinizi varsayalım.

Bir alt etki alanının ayarlanmasında normal bir temsilci seçmeye benzer bir süreç izlenmektedir. Tek fark, 3. adımda NS kayıtlarının bir etki alanı kayıt şirketi aracılığıyla değil de Azure DNS'deki "contoso.com" üst bölgesinde oluşturulması gerekmesidir.

1. Azure DNS'de "partners.contoso.com" alt bölgesini oluşturun.
2. Azure DNS'de alt bölgeyi barındıran ad sunucularını almak için, alt bölgedeki yetkili NS kayıtlarını arayın.
3. Üst bölgeden alt bölgeye işaret eden NS kayıtlarını yapılandırarak alt bölgeyi devredin.

### Bir alt etki alanını devretme
Aşağıdaki PowerShell örneğinde bunun nasıl çalıştığı gösterilmektedir. Aynı adımlar Azure Portal veya platformlar arası Azure CLI yoluyla gerçekleştirilebilir.

#### 1. Adım Üst ve alt bölgeleri oluşturma
Öncelikle, üst ve alt bölgeleri oluşturuyoruz. Bunlar aynı kaynak grubunda veya farklı kaynak gruplarında olabilir.

    $parent = New-AzureRmDnsZone -Name contoso.com -ResourceGroupName RG1
    $child = New-AzureRmDnsZone -Name partners.contoso.com -ResourceGroupName RG1

#### 2. Adım NS kayıtlarını alma
Ardından, bir sonraki örnekte gösterildiği gibi alt bölgeden yetkili NS kayıtlarını alıyoruz.  Bu, alt bölgeye atanan ad sunucularını içerir.

    $child_ns_recordset = Get-AzureRmDnsRecordSet -Zone $child -Name "@" -RecordType NS

#### 3. Adım Alt bölgeyi devretme
Temsilci seçmeyi tamamlamak için üst bölgede karşılık gelen NS kayıt kümesini oluşturun. Üst bölgedeki kayıt kümesi adının alt bölgenin adıyla (bu durumda "partners" ile) eşleştiğine dikkat edin.

    $parent_ns_recordset = New-AzureRmDnsRecordSet -Zone $parent -Name "partners" -RecordType NS -Ttl 3600
    $parent_ns_recordset.Records = $child_ns_recordset.Records
    Set-AzureRmDnsRecordSet -RecordSet $parent_ns_recordset

### Ad çözümlemesinin çalıştığını doğrulama
Alt bölgenin SOA kaydına bakarak her şeyin doğru şekilde ayarlandığını doğrulayabilirsiniz.

    nslookup –type=SOA partners.contoso.com

    Server: ns1-08.azure-dns.com
    Address: 208.76.47.8

    partners.contoso.com
        primary name server = ns1-08.azure-dns.com
        responsible mail addr = msnhst.microsoft.com
        serial = 1
        refresh = 900 (15 mins)
        retry = 300 (5 mins)
        expire = 604800 (7 days)
        default TTL = 300 (5 mins)

## Sonraki adımlar
[DNS bölgelerini yönetme](dns-operations-dnszones.md)

[DNS kayıtlarını yönetme](dns-operations-recordsets.md)

<!--HONumber=Oct16_HO1-->


