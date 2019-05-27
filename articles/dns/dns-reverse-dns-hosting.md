---
title: Azure DNS'de ters DNS arama bölgeleri barındırma | Microsoft Docs
description: Azure DNS, IP aralıkları için ters DNS arama bölgeleri barındırmak için kullanmayı öğrenin
services: dns
documentationcenter: na
author: vhorne
manager: jeconnoc
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/29/2017
ms.author: victorh
ms.openlocfilehash: cb2f04c692d4b5f385a89ba6a3071c20ef1bdf21
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66143598"
---
# <a name="host-reverse-dns-lookup-zones-in-azure-dns"></a>Ters DNS arama bölgeleri barındırma Azure DNS'de

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu makalede, Azure DNS, atanan IP aralıkları için ters DNS arama bölgeleri barındırma açıklanmaktadır. Geriye doğru arama bölgeleri tarafından temsil edilen IP aralıklarını kuruluşunuz için genellikle ISS'niz tarafından atanmalıdır.

Ters DNS, Azure hizmetine atanmış bir Azure'a ait IP adresi için yapılandırmak üzere bkz [Azure'da barındırılan hizmetler için yapılandırma ters DNS](dns-reverse-dns-for-azure-services.md).

Bu makalede okumadan önce sahibi olmalısınız [geriye doğru DNS ve Azure desteği'na genel bakış](dns-reverse-dns-overview.md).

Bu makalede Azure portalı, Azure PowerShell, Azure Klasik CLI veya Azure CLI kullanarak ilk geriye doğru arama DNS bölgenizi ve kaydınızı oluşturma adımlarında size kılavuzluk eder.

## <a name="create-a-reverse-lookup-dns-zone"></a>DNS geriye doğru arama bölgesi oluşturma

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Üzerinde **Hub** menüsünde **yeni** > **ağ**ve ardından **DNS bölgesi**.

   !["DNS bölgesi" seçimi](./media/dns-reverse-dns-hosting/figure1.png)

1. İçinde **DNS bölgesi oluştur** bölmesinde, DNS bölgenizi adlandırın. Bölge adı farklı IPv4 ve IPv6 ön ekleri için hazırlanmış. Yönergeler için kullanmak [IPv4](#ipv4) veya [IPv6](#ipv6) bölgenizin ad. İşlemi tamamladığınızda, seçin **Oluştur** bölgesi oluşturmak için.

### <a name="ipv4"></a>IPv4

Bir IPv4 geriye doğru arama bölgesinin adını temsil ettiği IP aralığına dayalıdır. Şu biçimde olmalıdır: `<IPv4 network prefix in reverse order>.in-addr.arpa`. Örnekler için bkz [geriye doğru DNS ve Azure desteği'na genel bakış](dns-reverse-dns-overview.md#ipv4).

> [!NOTE]
> Azure DNS'de Sınıfsız ters DNS arama bölgeleri oluştururken, kısa çizgi kullanmanız gerekir (`-`) eğik çizgi yerine (`/`) bölge adı.
>
> Örneğin, IP aralığı 192.0.2.128/26 için kullanmalısınız `128-26.2.0.192.in-addr.arpa` bölge adı yerine `128/26.2.0.192.in-addr.arpa`.
>
> DNS standartları iki yöntem de desteklese de, Azure DNS için eğik çizgi içeren DNS bölge adları desteklemez (`/`) karakter.

Aşağıdaki örnekte adlı bir sınıf C geriye doğru DNS bölgesi oluşturma işlemi gösterilmektedir `2.0.192.in-addr.arpa` Azure portalından Azure DNS'de:

 ![Doldurulmuş kutusuyla "DNS bölgesi oluşturma" bölmesi](./media/dns-reverse-dns-hosting/figure2.png)

**Kaynak grubu konumu** kaynak grubunun konumunu tanımlar. DNS bölgesini herhangi bir etkisi yoktur. DNS bölgesinin konumu her zaman “genel” şeklindedir ve gösterilmez.

Aşağıdaki örnekler, Azure PowerShell ve Azure CLI kullanarak bu görevi tamamlamak gösterilmektedir.

#### <a name="powershell"></a>PowerShell

```powershell
New-AzDnsZone -Name 2.0.192.in-addr.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azure-classic-cli"></a>Azure klasik CLI

```azurecli
azure network dns zone create MyResourceGroup 2.0.192.in-addr.arpa
```

#### <a name="azure-cli"></a>Azure CLI'si

```azurecli
az network dns zone create -g MyResourceGroup -n 2.0.192.in-addr.arpa
```

### <a name="ipv6"></a>IPv6

Bir IPv6 geriye doğru arama bölgesi adı şu biçimde olmalıdır: `<IPv6 network prefix in reverse order>.ip6.arpa`.  Örnekler için bkz [geriye doğru DNS ve Azure desteği'na genel bakış](dns-reverse-dns-overview.md#ipv6).


Aşağıdaki örnekte adlı bir IPv6 geriye doğru DNS arama bölgesi oluşturma işlemi gösterilmektedir `0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa` Azure portalından Azure DNS'de:

 ![Doldurulmuş kutusuyla "DNS bölgesi oluşturma" bölmesi](./media/dns-reverse-dns-hosting/figure3.png)

**Kaynak grubu konumu** kaynak grubunun konumunu tanımlar. DNS bölgesini herhangi bir etkisi yoktur. DNS bölgesinin konumu her zaman “genel” şeklindedir ve gösterilmez.

Aşağıdaki örnekler, Azure PowerShell ve Azure CLI kullanarak bu görevi tamamlamak gösterilmektedir.

#### <a name="powershell"></a>PowerShell

```powershell
New-AzDnsZone -Name 0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azure-classic-cli"></a>Azure klasik CLI

```azurecli
azure network dns zone create MyResourceGroup 0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

#### <a name="azure-cli"></a>Azure CLI'si

```azurecli
az network dns zone create -g MyResourceGroup -n 0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

## <a name="delegate-a-reverse-dns-lookup-zone"></a>DNS geriye doğru arama bölgesini temsilci seçme

Ters DNS Arama bölgenizi oluşturduğunuza göre bölge ana bölgeden bir temsilci emin olmanız gerekir. DNS temsilcisi, ters DNS arama bölgesini barındıran ad sunucularını bulmak DNS ad çözümleme işlemi sağlar. Bu ad sunucularını sonra adres aralığındaki IP adresleri için ters DNS sorgularına yanıt verebilir.

İleriye doğru arama bölgeleri için bir DNS bölgesi için temsilci seçme işleminin açıklanan [etki alanınızı Azure DNS'ye devretme](dns-delegate-domain-azure-dns.md). Geriye doğru arama bölgeleri için temsilci seçme, aynı şekilde çalışır. Tek fark etki alanı adı kayıt yerine IP aralığınızı sağlanan ISS ile ad sunucularını yapılandırmak ihtiyacınız olan.

## <a name="create-a-dns-ptr-record"></a>Bir DNS PTR kaydı oluştur

### <a name="ipv4"></a>IPv4

Aşağıdaki örnekte Azure DNS'de ters DNS bölgesindeki bir PTR kaydı oluşturma işleminde size kılavuzluk eder. Diğer kayıt türleri ve var olan kayıtların değiştirilmesi hakkında bilgi için bkz. [Azure portalı kullanarak DNS kayıtlarını ve kayıt kümelerini yönetme](dns-operations-recordsets-portal.md).

1. Üst kısmındaki **DNS bölgesi** bölmesinde **+ kayıt kümesi** açmak için **kayıt kümesi Ekle** bölmesi.

   ![Kayıt kümesi oluşturmak için düğme](./media/dns-reverse-dns-hosting/figure4.png)

1. Kayıt için bir PTR kaydı kümesi adı, IPv4 adresi geri kalanını ters sırada olması gerekiyor. 

   Bu örnekte, ilk üç sekizlisinin aynı bölge adı (.2.0.192) bir parçası olarak önceden doldurulur. Bu nedenle, yalnızca son sekizli içinde sağlanan **adı** kutusu. Örneğin, kayıt kümenizi adlandırın **15** 192.0.2.15, IP adresi olan bir kaynak için.  
1. İçin **türü**seçin **PTR**.  
1. İçin **etki alanı adı**, IP kullanan kaynak tam etki alanı adı (FQDN) girin.
1. Seçin **Tamam** DNS oluşturma bölmesinin en altında kaydedin.

   ![Doldurulmuş kutusuyla "kayıt kümesi Ekle" bölmesi](./media/dns-reverse-dns-hosting/figure5.png)

Aşağıdaki örneklerde, PowerShell veya Azure CLI kullanarak bu görevi tamamlamak gösterilmektedir.

#### <a name="powershell"></a>PowerShell

```powershell
New-AzDnsRecordSet -Name 15 -RecordType PTR -ZoneName 2.0.192.in-addr.arpa -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzDnsRecordConfig -Ptrdname "dc1.contoso.com")
```
#### <a name="azure-classic-cli"></a>Azure klasik CLI

```azurecli
azure network dns record-set add-record MyResourceGroup 2.0.192.in-addr.arpa 15 PTR --ptrdname dc1.contoso.com  
```

#### <a name="azure-cli"></a>Azure CLI'si

```azurecli
    az network dns record-set ptr add-record -g MyResourceGroup -z 2.0.192.in-addr.arpa -n 15 --ptrdname dc1.contoso.com
```

### <a name="ipv6"></a>IPv6

Aşağıdaki örnek yeni bir PTR kaydı oluşturma sürecinde yardımcı olur. Diğer kayıt türleri ve var olan kayıtların değiştirilmesi hakkında bilgi için bkz. [Azure portalı kullanarak DNS kayıtlarını ve kayıt kümelerini yönetme](dns-operations-recordsets-portal.md).

1. Üst kısmındaki **DNS bölgesi** bölmesinde **+ kayıt kümesi** açmak için **kayıt kümesi Ekle** bölmesi.

   ![Kayıt kümesi oluşturmak için düğme](./media/dns-reverse-dns-hosting/figure6.png)

2. Kayıt için bir PTR kaydı kümesi adı, IPv6 adresi geri kalanını ters sırada olması gerekiyor. Sıfır bir sıkıştırma içermesi gerekir. 

   Bu örnekte, IPv6'ın ilk 64 bit (0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa) bölge adı bir parçası olarak önceden doldurulur. Bu nedenle, yalnızca son 64 bit içinde sağlanan **adı** kutusu. Son 64 bit IP adresinin ters sırada her onaltılık sayı arasında sınırlayıcı olarak noktayla girilir. Örneğin, kayıt kümenizi adlandırın **e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f** 2001:0db8:abdc:0000:f524:10bc:1af9:405e, IP adresi olan bir kaynak için.  
3. İçin **türü**seçin **PTR**.  
4. İçin **etki alanı adı**, IP kullanan kaynağın FQDN'sini girin.
5. Seçin **Tamam** DNS oluşturma bölmesinin en altında kaydedin.

![Doldurulmuş kutusuyla "kayıt kümesi Ekle" bölmesi](./media/dns-reverse-dns-hosting/figure7.png)

Aşağıdaki örneklerde, PowerShell veya Azure CLI kullanarak bu görevi tamamlamak gösterilmektedir.

#### <a name="powershell"></a>PowerShell

```powershell
New-AzDnsRecordSet -Name "e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f" -RecordType PTR -ZoneName 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzDnsRecordConfig -Ptrdname "dc2.contoso.com")
```

#### <a name="azure-classic-cli"></a>Azure klasik CLI

```
azure network dns record-set add-record MyResourceGroup 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f PTR --ptrdname dc2.contoso.com 
```
 
#### <a name="azure-cli"></a>Azure CLI'si

```azurecli
    az network dns record-set ptr add-record -g MyResourceGroup -z 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa -n e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f --ptrdname dc2.contoso.com
```

## <a name="view-records"></a>Kayıtları görüntüleme

Oluşturduğunuz kayıtları görüntülemek için DNS bölgenizi Azure portalında göz atın. Alt kısmında **DNS bölgesi** bölmesinde, DNS bölgesine ait kayıtları görebilirsiniz. ' % S'varsayılan NS ve SOA kayıtları yanı sıra, oluşturduğunuz tüm kayıtları görürsünüz. Her bölgede oluşturulan NS ve SOA kayıtlarının. 

### <a name="ipv4"></a>IPv4

**DNS bölgesi** bölmesinde IPv4 PTR kayıtları gösterir:

![IPv4 kayıtlarla "DNS bölgesi" bölmesi](./media/dns-reverse-dns-hosting/figure8.png)

Aşağıdaki örneklerde, PowerShell veya Azure CLI kullanarak PTR kayıtları görüntülemek gösterilmektedir.

#### <a name="powershell"></a>PowerShell

```powershell
Get-AzDnsRecordSet -ZoneName 2.0.192.in-addr.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azure-classic-cli"></a>Azure klasik CLI

```azurecli
    azure network dns record-set list MyResourceGroup 2.0.192.in-addr.arpa
```

#### <a name="azure-cli"></a>Azure CLI'si

```azurecli
    azure network dns record-set list -g MyResourceGroup -z 2.0.192.in-addr.arpa
```

### <a name="ipv6"></a>IPv6

**DNS bölgesi** bölmesi IPv6 PTR kayıtları gösterir:

![IPv6 kayıtlarını "DNS bölgesi" bölmesi](./media/dns-reverse-dns-hosting/figure9.png)

Aşağıdaki örneklerde, PowerShell veya Azure CLI kullanarak kayıtları görüntülemek gösterilmektedir.

#### <a name="powershell"></a>PowerShell

```powershell
Get-AzDnsRecordSet -ZoneName 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azure-classic-cli"></a>Azure klasik CLI

```azurecli
    azure network dns record-set list MyResourceGroup 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

#### <a name="azure-cli"></a>Azure CLI'si

```azurecli
    azure network dns record-set list -g MyResourceGroup -z 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

## <a name="faq"></a>SSS

### <a name="can-i-host-reverse-dns-lookup-zones-for-my-isp-assigned-ip-blocks-on-azure-dns"></a>My ISS atanmış IP Blok Azure DNS için ters DNS arama bölgeleri barındırabilir miyim?

Evet. Azure DNS'de kendi IP aralıkları için (ARPA) geriye doğru arama bölgeleri barındırma tam olarak desteklenir.

Bu makalede açıklandığı gibi Azure DNS'de geriye doğru arama bölgesi oluşturun ve ardından ISS'nize çalışmak [bölgeyi devredin](dns-domain-delegation.md). Bundan sonra her geriye doğru arama PTR kayıtlarını diğer kayıt türleri aynı şekilde yönetebilirsiniz.

### <a name="how-much-does-hosting-my-reverse-dns-lookup-zone-cost"></a>Ne kadar my ters DNS Arama bölge maliyeti barındırma mu?

Azure DNS, ISS atanmış IP Blok ücretlendirilir için ters DNS arama bölgesini barındıran [standart Azure DNS ücretler](https://azure.microsoft.com/pricing/details/dns/).

### <a name="can-i-host-reverse-dns-lookup-zones-for-both-ipv4-and-ipv6-addresses-in-azure-dns"></a>IPv4 ve IPv6 adresleri Azure DNS'de ters DNS arama bölgeleri ana bilgisayar?

Evet. Bu makalede, Azure DNS'de IPv4 ve IPv6 geriye doğru DNS arama bölgeleri oluşturma açıklanmaktadır.

### <a name="can-i-import-an-existing-reverse-dns-lookup-zone"></a>Bir var olan DNS geriye doğru arama bölgesi alabilirim?

Evet. Azure CLI, Azure DNS ile var olan DNS bölgeleri içeri aktarmak için kullanabilirsiniz. Bu yöntem, hem ileriye doğru arama bölgelerinin hem de geriye doğru arama bölgeleri için çalışır.

Daha fazla bilgi için [içeri ve dışarı aktarma Azure CLI kullanarak DNS bölge dosyasını](dns-import-export.md).

## <a name="next-steps"></a>Sonraki adımlar

Ters DNS hakkında daha fazla bilgi için bkz. [Wikipedia geriye doğru DNS araması](https://en.wikipedia.org/wiki/Reverse_DNS_lookup).
<br>
Bilgi edinmek için nasıl [, Azure Hizmetleri için ters DNS kayıtlarını yönetme](dns-reverse-dns-for-azure-services.md).
