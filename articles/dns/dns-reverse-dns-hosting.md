---
title: "Azure DNS'de DNS arama bölgeleri ters barındırma | Microsoft Docs"
description: "Azure DNS geriye doğru DNS arama bölgeleri IP aralıkları için barındırmak için kullanmayı öğrenin"
services: dns
documentationcenter: na
author: jtuliani
manager: timlt
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/29/2017
ms.author: jonatul
ms.openlocfilehash: 3e10b25d2f9b91c96af2958fef6dc6a4fdbff301
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="hosting-reverse-dns-lookup-zones-in-azure-dns"></a>Azure DNS geriye doğru DNS araması bölgelerinde barındırma

Bu makalede, Azure DNS'de, atanan IP aralıkları için geriye doğru DNS arama bölgeleri barındıran açıklanmaktadır. Geriye doğru arama bölgesi tarafından temsil edilen IP aralıkları, kuruluşunuz için genellikle ISS'niz tarafından atanmış olması gerekir.

Azure Hizmetinize atanmış Azure ait IP adresi için ters DNS yapılandırmak için bkz: [Azure hizmetiniz için ayrılan IP adresleri için geriye doğru arama yapılandırma](dns-reverse-dns-for-azure-services.md).

Bu makalede okumadan önce bu bilgi sahibi olmanız [geriye doğru DNS ve Azure desteği'na genel bakış](dns-reverse-dns-overview.md).

Bu makalede, ilk geriye doğru arama DNS bölgesi oluşturma ve Azure portalı, Azure PowerShell, Azure CLI 1.0 veya Azure CLI 2.0 kullanarak kaydetmek için adımlarda size yol gösterir.

## <a name="create-a-reverse-lookup-dns-zone"></a>Geriye doğru arama DNS bölgesi oluşturma

1. Oturum [Azure portalı](https://portal.azure.com)
1. Hub menüsünde, tıklayıp **yeni** > **ağ** > ve ardından **DNS bölgesi** açmak için **oluşturma DNS bölgesi** Dikey.

   ![DNS bölgesi](./media/dns-reverse-dns-hosting/figure1.png)

1. Üzerinde **oluşturma DNS bölgesi** dikey penceresinde, DNS bölgenizi adı. Bölge adı farklı IPv4 ve IPv6 önekleri için hazırlanmış. Her iki yönergelerini kullanın [IPv4](#ipv4) veya [IPv6](#ipv6) bölgenizi adlandırın. Tıklatın tamamlandığında **oluşturma** bölgesi oluşturmak için.

### <a name="ipv4"></a>IPv4

Bir IPv4 geriye doğru arama bölgesi adı temsil ettiği IP aralığına dayalıdır. Şu biçimde olmalıdır: `<IPv4 network prefix in reverse order>.in-addr.arpa`. Örnekler için bkz: [geriye doğru DNS ve Azure desteği'na genel bakış](dns-reverse-dns-overview.md#ipv4).

> [!NOTE]
> Azure DNS'de Sınıfsız geriye doğru DNS arama bölgeleri oluştururken, kısa çizgi kullanmanız gerekir (`-`) yerine eğik çizgi ('/ ') bölge adı.
>
> Örneğin, IP aralığı 192.0.2.128/26 için kullanmanız gerekir `128-26.2.0.192.in-addr.arpa` yerine bölge adı olarak `128/26.2.0.192.in-addr.arpa`.
>
> Her ikisi de DNS standartları tarafından desteklenir, ancak eğik içeren adlara DNS bölgesi, bunun nedeni (`/`) karakter Azure DNS'de desteklenmiyor.

Aşağıdaki örnek adlı bir 'C sınıfı' geriye doğru DNS bölgesi oluşturmak nasıl gösterir `2.0.192.in-addr.arpa` Azure Portalı aracılığıyla Azure DNS'de:

 ![DNS bölgesi oluşturma](./media/dns-reverse-dns-hosting/figure2.png)

'Kaynak grubu konumu' kaynak grubu için konum tanımlar ve DNS bölgesi üzerinde hiçbir etkisi olmaz. DNS bölge konumu her zaman 'global' dır ve gösterilmiyor.

Aşağıdaki örnekler, Azure PowerShell ve Azure CLI bu görevi tamamlamak nasıl gösterir:

#### <a name="powershell"></a>PowerShell

```powershell
New-AzureRmDnsZone -Name 2.0.192.in-addr.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azure-cli-10"></a>Azure CLI 1.0

```azurecli
azure network dns zone create MyResourceGroup 2.0.192.in-addr.arpa
```

#### <a name="azure-cli-20"></a>Azure CLI 2.0

```azurecli
az network dns zone create -g MyResourceGroup -n 2.0.192.in-addr.arpa
```

### <a name="ipv6"></a>IPv6

Bir IPv6 geriye doğru arama bölgesi adı şu biçimde olmalıdır: `<IPv6 network prefix in reverse order>.ip6.arpa`.  Örnekler için bkz: [geriye doğru DNS ve Azure desteği'na genel bakış](dns-reverse-dns-overview.md#ipv6).


Aşağıdaki örnek adlı bir IPv6 geriye doğru DNS arama bölgesi oluşturmak nasıl gösterir `0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa` Azure Portalı aracılığıyla Azure DNS'de:

 ![DNS bölgesi oluşturma](./media/dns-reverse-dns-hosting/figure3.png)

'Kaynak grubu konumu' kaynak grubu için konum tanımlar ve DNS bölgesi üzerinde hiçbir etkisi olmaz. DNS bölge konumu her zaman 'global' dır ve gösterilmiyor.

Aşağıdaki örnekler, Azure PowerShell ve Azure CLI bu görevi tamamlamak nasıl gösterir:

#### <a name="powershell"></a>PowerShell

```powershell
New-AzureRmDnsZone -Name 0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azurecli-10"></a>AzureCLI 1.0

```azurecli
azure network dns zone create MyResourceGroup 0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

#### <a name="azurecli-20"></a>AzureCLI 2.0

```azurecli
az network dns zone create -g MyResourceGroup -n 0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

## <a name="delegate-a-reverse-dns-lookup-zone"></a>DNS geriye doğru arama bölgesini temsilci seçme

Geriye doğru DNS araması bölgenizi oluşturduktan, bölge üst bölgeden bir temsilci emin olmalısınız. DNS temsilcisi, geriye doğru DNS arama bölgesini barındıran ad sunucularını bulmak DNS ad çözümleme işlemi sağlar. Bu, adres aralığındaki IP adresleri için geriye doğru DNS sorgularını yanıtlamak bu ad sunucuları sağlar.

Bir DNS bölgesi için temsilci seçme işleminin açıklanan ileriye doğru arama bölgeleri için [etki alanınızı Azure DNS'ye temsilci](dns-delegate-domain-azure-dns.md). Geriye doğru arama bölgeleri için temsilci seçme aynı şekilde çalışır. Tek fark, ad sunucuları, etki alanı adı kayıt yerine IP aralığınızı sağlanan ISS ile yapılandırmanız gerekiyor ' dir.

## <a name="create-a-dns-ptr-record"></a>Bir DNS PTR kaydı oluştur

### <a name="ipv4"></a>IPv4

Aşağıdaki örnek, bir geriye doğru DNS bölgesinde Azure DNS PTR kaydı oluşturma işlemi açıklanmaktadır. Diğer kayıt türleri ve var olan kayıtların değiştirilmesi hakkında bilgi için bkz. [Azure portalı kullanarak DNS kayıtlarını ve kayıt kümelerini yönetme](dns-operations-recordsets-portal.md).

1.  **DNS bölgesi** dikey penceresinin üzerindeki **+ Kayıt kümesi**’ni seçerek **Kayıt kümesi ekle** dikey penceresini açın.

 ![DNS bölgesi](./media/dns-reverse-dns-hosting/figure4.png)

1. Üzerinde **kayıt kümesi ekleme** dikey. 
1. Seçin **PTR** kayıttan "**türü**" menüsü.  
1. Kayıt için bir PTR kayıt kümesi adını IPv4 adresi kalan ters sırada olması gerekir. Bu örnekte, ilk üç sekizlisinin bölge adı (.2.0.192) bir parçası olarak önceden doldurulur. Bu nedenle, yalnızca son sekizli ad alanında sağlanır. Örneğin, kayıt kümenizi adlandırabilirsiniz "**15**", IP adresi 192.0.2.15 olan bir kaynak için.  
1. İçindeki "**etki alanı adı**" alanı, IP kullanarak kaynak tam etki alanı adı (FQDN) girin.
1. DNS kaydını oluşturmak için dikey pencerenin altındaki **Tamam**’ı seçin.

 ![kayıt kümesi Ekle](./media/dns-reverse-dns-hosting/figure5.png)

Bu görev, PowerShell ve AzureCLI tamamlamak hakkında örnekler şunlardır:

#### <a name="powershell"></a>PowerShell

```powershell
New-AzureRmDnsRecordSet -Name 15 -RecordType PTR -ZoneName 2.0.192.in-addr.arpa -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Ptrdname "dc1.contoso.com")
```
#### <a name="azurecli-10"></a>AzureCLI 1.0

```azurecli
azure network dns record-set add-record MyResourceGroup 2.0.192.in-addr.arpa 15 PTR --ptrdname dc1.contoso.com  
```

#### <a name="azurecli-20"></a>AzureCLI 2.0

```azurecli
    az network dns record-set ptr add-record -g MyResourceGroup -z 2.0.192.in-addr.arpa -n 15 --ptrdname dc1.contoso.com
```

### <a name="ipv6"></a>IPv6

Aşağıdaki örnek yeni bir 'PTR' kayıt oluşturma sürecinde yardımcı olur. Diğer kayıt türleri ve var olan kayıtların değiştirilmesi hakkında bilgi için bkz. [Azure portalı kullanarak DNS kayıtlarını ve kayıt kümelerini yönetme](dns-operations-recordsets-portal.md).

1. Üstündeki **DNS bölge dikey**seçin **+ kayıt kümesine** açmak için **kayıt kümesi ekleme** dikey.

  ![DNS bölge dikey penceresi](./media/dns-reverse-dns-hosting/figure6.png)

2. Üzerinde **kayıt kümesi ekleme** dikey. 
3. Seçin **PTR** kayıttan "**türü**" menüsü.  
4. Kayıt için bir PTR kayıt kümesi adını rest IPv6 adresinin ters sırada olması gerekir. Sıfır sıkıştırma eklemeniz gerekir. Bu örnekte, ilk 64 bit IPv6 bölge adı (0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa) bir parçası olarak önceden doldurulur. Bu nedenle, yalnızca son 64 bitlik ad alanında verilir. Son 64 bit IP adresinin ters sırada bir süre her onaltılık sayı bölücüyü kullanarak girilir. Örneğin, kayıt kümenizi adlandırabilirsiniz "**e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f**", IP adresi 2001:0db8:abdc:0000:f524:10bc:1af9:405e olan bir kaynak için.  
5. İçindeki "**etki alanı adı**" alanı, IP kullanarak kaynak tam etki alanı adı (FQDN) girin.
6. DNS kaydını oluşturmak için dikey pencerenin altındaki **Tamam**’ı seçin.

![kayıt kümesi dikey ekleme](./media/dns-reverse-dns-hosting/figure7.png)

Bu görev, PowerShell ve AzureCLI tamamlamak hakkında örnekler şunlardır:

#### <a name="powershell"></a>PowerShell

```powershell
New-AzureRmDnsRecordSet -Name "e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f" -RecordType PTR -ZoneName 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Ptrdname "dc2.contoso.com")
```

#### <a name="azurecli-10"></a>AzureCLI 1.0

```
azure network dns record-set add-record MyResourceGroup 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f PTR --ptrdname dc2.contoso.com 
```
 
#### <a name="azurecli-20"></a>AzureCLI 2.0

```azurecli
    az network dns record-set ptr add-record -g MyResourceGroup -z 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa -n e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f --ptrdname dc2.contoso.com
```

## <a name="view-records"></a>Kayıtları görüntüleme

Oluşturduğunuz kayıtları görüntülemek için DNS bölgenizi Azure portalında gidin. Alt kısmında **DNS bölgesi** dikey penceresinde, DNS bölgesi için kayıt görebilirsiniz. Her bölgede oluşturulan varsayılan NS ve SOA kayıtlarının yanı sıra, oluşturduğunuz tüm kayıtları görürsünüz.

### <a name="ipv4"></a>IPv4

IPv4 PTR kayıtları gösteren DNS bölge dikey penceresinde:

![DNS bölge dikey penceresi](./media/dns-reverse-dns-hosting/figure8.png)

Aşağıdaki örnekler, PowerShell veya Azure CLI kullanarak PTR kayıtları görüntülemek nasıl gösterir:

#### <a name="powershell"></a>PowerShell

```powershell
Get-AzureRmDnsRecordSet -ZoneName 2.0.192.in-addr.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azure-cli-10"></a>Azure CLI 1.0

```azurecli
    azure network dns record-set list MyResourceGroup 2.0.192.in-addr.arpa
```

#### <a name="azure-cli-20"></a>Azure CLI 2.0

```azurecli
    azure network dns record-set list -g MyResourceGroup -z 2.0.192.in-addr.arpa
```

### <a name="ipv6"></a>IPv6

IPv6 PTR kayıtları gösteren DNS bölge dikey penceresinde:

![DNS bölge dikey penceresi](./media/dns-reverse-dns-hosting/figure9.png)

PowerShell ve AzureCLI kayıtları görüntülemek nasıl örnekleri aşağıda verilmiştir:

#### <a name="powershell"></a>PowerShell

```powershell
Get-AzureRmDnsRecordSet -ZoneName 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azure-cli-10"></a>Azure CLI 1.0

```azurecli
    azure network dns record-set list MyResourceGroup 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

#### <a name="azure-cli-20"></a>Azure CLI 2.0

```azurecli
    azure network dns record-set list -g MyResourceGroup -z 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

## <a name="faq"></a>SSS

### <a name="can-i-host-reverse-dns-lookup-zones-for-my-isp-assigned-ip-blocks-on-azure-dns"></a>Geriye doğru DNS arama bölgeleri my ISS atanan IP blokları Azure DNS üzerinde barındırabilir?

Evet. Azure DNS'de kendi IP aralıkları için (ARPA) geriye doğru arama bölgeleri barındıran tam olarak desteklenir.

Bu makalede anlatıldığı gibi Azure DNS geriye doğru arama bölgesi oluşturun, sonra Internet servis Sağlayıcınıza ile çalışırsınız [bölgeye temsilci](dns-domain-delegation.md).  Daha sonra PTR kayıtları her geriye doğru arama için diğer kayıt türleri aynı şekilde yönetebilirsiniz.

### <a name="how-much-does-hosting-my-reverse-dns-lookup-zone-cost"></a>Ne kadar my ters DNS araması bölge maliyeti barındırma mu?

Azure DNS, ISS atanan IP Blok adresindeki doludur için geriye doğru DNS arama bölgesini barındıran [standart Azure DNS ücretler](https://azure.microsoft.com/pricing/details/dns/).

### <a name="can-i-host-reverse-dns-lookup-zones-for-both-ipv4-and-ipv6-addresses-in-azure-dns"></a>Azure DNS'de IPv4 ve IPv6 adresleri için geriye doğru DNS arama bölgeleri barındıran?

Evet. Bu makalede, Azure DNS'de hem IPv4 hem de IPv6 geriye doğru DNS arama bölgeleri oluşturma açıklanmaktadır.

### <a name="can-i-import-an-existing-reverse-dns-lookup-zone"></a>Varolan bir geriye doğru DNS araması bölge alabilir miyim?

Evet. Azure CLI, var olan DNS bölgeleri Azure DNS aktarmak için kullanabilirsiniz. Bu, ileriye doğru arama bölgeleri ve geriye doğru arama bölgeleri için çalışır.

Daha fazla bilgi için bkz: [içeri ve dışarı aktarma Azure CLI kullanarak bir DNS bölge dosyasına](dns-import-export.md).

## <a name="next-steps"></a>Sonraki adımlar

Geriye doğru DNS hakkında daha fazla bilgi için bkz: [wikipedia'da ters DNS araması](http://en.wikipedia.org/wiki/Reverse_DNS_lookup).
<br>
Bilgi edinmek için nasıl [, Azure Hizmetleri için ters DNS kayıtlarını yönetme](dns-reverse-dns-for-azure-services.md).
