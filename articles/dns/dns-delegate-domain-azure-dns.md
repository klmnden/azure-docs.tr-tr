---
title: "Etki alanınızı Azure DNS'ye devretme | Microsoft Belgeleri"
description: "Etki alanı temsilcisi seçiminin nasıl değiştirileceğini ve etki alanı barındırma sağlamak üzere Azure DNS ad sunucularının nasıl kullanılacağını anlayın."
services: dns
documentationcenter: na
author: KumudD
manager: jeconnoc
ms.assetid: 257da6ec-d6e2-4b6f-ad76-ee2dde4efbcc
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/18/2017
ms.author: kumud
ms.openlocfilehash: a13d9b360e2c43aa2a3d4f62215e4570ce42cd5b
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="delegate-a-domain-to-azure-dns"></a>Azure DNS'ye bir etki alanı devretme

Azure DNS’yi kullanarak bir DNS bölgesi barındırabilir ve Azure'da bir etki alanının DNS kayıtlarını yönetebilirsiniz. Bir etki alanının DNS sorgularının Azure DNS'ye ulaşması için, etki alanının Azure DNS'ye üst etki alanından devredilmiş olması gerekir. Azure DNS'nin etki alanı kayıt şirketi olmadığını unutmayın. Bu makalede etki alanınızı Azure DNS’ye devretme işlemi açıklanır.

Bir kayıt şirketinden satın alınan etki alanları için, kayıt şirketiniz NS kayıtlarını ayarlama seçeneği sunar. Azure DNS'de aynı etki alanı adıyla DNS bölgesi oluşturmak için bir etki alanına sahip olmanız gerekmez. Ancak Azure DNS'ye temsilci seçmeyi kayıt şirketi ile ayarlamak için etki alanına sahip olmanız gerekir.

Örneğin, contoso.net etki alanını satın aldığınızı ve Azure DNS'de contoso.net adlı bir bölge oluşturduğunuzu varsayalım. Etki alanının sahibi olduğunuzdan, kayıt şirketiniz size etki alanınız için ad sunucusu adreslerini (yani NS kayıtlarını) yapılandırma seçeneğini sunar. Kayıt şirketi bu NS kayıtlarını üst etki alanında (.net) depolar. Ardından, dünya genelindeki istemciler contoso.net’teki DNS kayıtlarını çözümlemeye çalışırken Azure DNS bölgesindeki etki alanınıza yönlendirilebilir.

## <a name="create-a-dns-zone"></a>DNS bölgesi oluşturma

1. Azure Portal’da oturum açın.
1. **Hub** menüsünde **Yeni** > **Ağ** > **DNS bölgesi**’ni seçerek **DNS bölgesi oluştur** sayfasını açın.

   ![DNS bölgesi](./media/dns-domain-delegation/dns.png)

1. **DNS bölgesi oluştur** sayfasında aşağıdaki değerleri girin ve **Oluştur**’u seçin:

   | **Ayar** | **Değer** | **Ayrıntılar** |
   |---|---|---|
   |**Ad**|contoso.net|DNS bölgesinin adını sağlayın.|
   |**Abonelik**|[Aboneliğiniz]|Uygulama ağ geçidinin oluşturulacağı bir abonelik seçin.|
   |**Kaynak grubu**|**Yeni oluştur:** contosoRG|Bir kaynak grubu oluşturun. Kaynak grubu adı, seçtiğiniz abonelik içinde benzersiz olmalıdır. Kaynak grupları hakkında daha fazla bilgi için, [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups)’a genel bakış makalesini okuyun.|
   |**Konum**|Batı ABD||

> [!NOTE]
> Kaynak grubunun konumunu ifade eder ve DNS bölgesini etkilemez. DNS bölgesinin konumu her zaman “genel” şeklindedir ve gösterilmez.

## <a name="retrieve-name-servers"></a>Ad sunucularını alma

DNS bölgenizi Azure DNS'ye devretmeden önce, bölgenizin ad sunucularını bilmeniz gerekir. Azure DNS, her bölge oluşturmada bir havuzdan ad sunucuları ayırır.

1. Oluşturulan DNS bölgesiyle, Azure Portal **Sık Kullanılanlar** bölmesinde, **Tüm kaynaklar**’ı seçin. **Tüm kaynaklar** sayfasında **contoso.net** DNS bölgesini seçin. Seçtiğiniz abonelikte zaten çeşitli kaynaklar varsa, uygulama ağ geçidine kolaylıkla erişmek için **Ada göre filtrele** kutusuna **contoso.net** adresini girebilirsiniz. 

1. DNS bölgesi sayfasından ad sunucularını alın. Bu örnekte, contoso.net bölgesine ns1-01.azure-dns.com, ns2-01.azure-dns.net, ns3-01.azure-dns.org ve ns4-01.azure-dns.info ad sunucuları atanmıştır:

   ![Ad sunucularının listesi](./media/dns-domain-delegation/viewzonens500.png)

Azure DNS, bölgenizdeki yetkili NS kayıtlarını atanan ad sunucularını içerecek şekilde otomatik olarak oluşturur. Ad sunucularını Azure PowerShell veya Azure CLI aracılığıyla görmek için bu kayıtları alın.

Aşağıdaki örneklerde, PowerShell ve Azure CLI kullanılarak Azure DNS içindeki bir bölgenin ad sunucularını alma adımları sağlanır.

### <a name="powershell"></a>PowerShell

```powershell
# The record name "@" is used to refer to records at the top of the zone.
$zone = Get-AzureRmDnsZone -Name contoso.net -ResourceGroupName contosoRG
Get-AzureRmDnsRecordSet -Name "@" -RecordType NS -Zone $zone
```

Yanıt aşağıdaki örnek olur:

```
Name              : @
ZoneName          : contoso.net
ResourceGroupName : contosorg
Ttl               : 172800
Etag              : 03bff8f1-9c60-4a9b-ad9d-ac97366ee4d5
RecordType        : NS
Records           : {ns1-07.azure-dns.com., ns2-07.azure-dns.net., ns3-07.azure-dns.org.,
                    ns4-07.azure-dns.info.}
Metadata          :
```

### <a name="azure-cli"></a>Azure CLI

```azurecli
az network dns record-set list --resource-group contosoRG --zone-name contoso.net --type NS --name @
```

Yanıt aşağıdaki örnek olur:

```json
{
  "etag": "03bff8f1-9c60-4a9b-ad9d-ac97366ee4d5",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contosoRG/providers/Microsoft.Network/dnszones/contoso.net/NS/@",
  "metadata": null,
  "name": "@",
  "nsRecords": [
    {
      "nsdname": "ns1-07.azure-dns.com."
    },
    {
      "nsdname": "ns2-07.azure-dns.net."
    },
    {
      "nsdname": "ns3-07.azure-dns.org."
    },
    {
      "nsdname": "ns4-07.azure-dns.info."
    }
  ],
  "resourceGroup": "contosoRG",
  "ttl": 172800,
  "type": "Microsoft.Network/dnszones/NS"
}
```

## <a name="delegate-the-domain"></a>Etki alanını devretme

Artık DNS bölgesi oluşturulduğuna ve ad sunucularınız olduğuna göre, üst etki alanını Azure DNS ad sunucularıyla güncelleştirmeniz gerekir. Her kayıt şirketi, bir etki alanının ad sunucusu kayıtlarını değiştirmek için kendi DNS yönetim araçlarına sahiptir. Kayıt şirketinin DNS yönetim sayfasında NS kayıtlarını düzenleyin ve NS kayıtlarını Azure DNS'nin oluşturduklarıyla değiştirin.

Bir etki alanını Azure DNS'ye devrederken Azure DNS tarafından sağlanan ad sunucularını kullanmanız gerekir. Etki alanınızın adından bağımsız olarak dört ad sunucusunun tamamını kullanmanız önerilir. Etki alanı temsilcisi, bir ad sunucusunun etki alanınızla aynı üst düzey etki alanını kullanmasını gerektirmez.

Azure DNS ad sunucusu IP adresleri gelecekte değişebileceği için, bu IP adreslerini gösterirken *birleştirici kayıtlar*ı kullanmayın. Kendi bölgenizdeki ad sunucularını kullanan ve bazen *gösterim ad sunucuları* olarak adlandırılan temsilci seçimleri şu anda Azure DNS'de desteklenmemektedir.

## <a name="verify-that-name-resolution-is-working"></a>Ad çözümlemesinin çalıştığını doğrulama

Temsilci seçmeyi tamamladıktan sonra, bölgenizin SOA kaydını sorgulamak için nslookup gibi bir araç kullanarak ad çözümlemesinin çalışıp çalışmadığını doğrulayabilirsiniz. (SOA kaydı, bölge oluşturulurken otomatik olarak oluşturulur.)

Azure DNS ad sunucularını belirtmeniz gerekmez. Temsilci seçimi doğru ayarlanmışsa normal DNS çözümleme işlemi ad sunucularını otomatik olarak bulur.

```
nslookup -type=SOA contoso.com
```

Aşağıda, önceki komuttan bir yanıt örneği gösterilmektedir:

```
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
```

## <a name="delegate-subdomains-in-azure-dns"></a>Azure DNS'de alt etki alanlarını devretme

Ayrı bir alt bölge kurmak istiyorsanız Azure DNS'de bir alt etki alanını devredebilirsiniz. Örneğin, Azure DNS'de contoso.net adresini ayarlayıp devrettiğinizi varsayalım. Şimdi, partners.contoso.net adresli ayrı bir alt bölge ayarlamak istiyorsunuz diyelim.

1. Azure DNS'de partners.contoso.net alt bölgesini oluşturun.
2. Azure DNS'de alt bölgeyi barındıran ad sunucularını almak için, alt bölgedeki yetkili NS kayıtlarını arayın.
3. Üst bölgeden alt bölgeye işaret eden NS kayıtlarını yapılandırarak alt bölgeyi devredin.

### <a name="create-a-dns-zone"></a>DNS bölgesi oluşturma

1. Azure Portal’da oturum açın.
1. **Hub** menüsünde **Yeni** > **Ağ** > **DNS bölgesi**’ni seçerek **DNS bölgesi oluştur** sayfasını açın.

   ![DNS bölgesi](./media/dns-domain-delegation/dns.png)

1. **DNS bölgesi oluştur** sayfasında aşağıdaki değerleri girin ve **Oluştur**’u seçin:

   | **Ayar** | **Değer** | **Ayrıntılar** |
   |---|---|---|
   |**Ad**|partners.contoso.net|DNS bölgesinin adını sağlayın.|
   |**Abonelik**|[Aboneliğiniz]|Uygulama ağ geçidinin oluşturulacağı bir abonelik seçin.|
   |**Kaynak grubu**|**Varolanı kullan:** contosoRG|Bir kaynak grubu oluşturun. Kaynak grubu adı, seçtiğiniz abonelik içinde benzersiz olmalıdır. Kaynak grupları hakkında daha fazla bilgi için, [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups)’a genel bakış makalesini okuyun.|
   |**Konum**|Batı ABD||

> [!NOTE]
> Kaynak grubunun konumunu ifade eder ve DNS bölgesini etkilemez. DNS bölgesinin konumu her zaman “genel” şeklindedir ve gösterilmez.

### <a name="retrieve-name-servers"></a>Ad sunucularını alma

1. Oluşturulan DNS bölgesiyle, Azure Portal **Sık Kullanılanlar** bölmesinde, **Tüm kaynaklar**’ı seçin. **Tüm kaynaklar** sayfasında **partners.contoso.net** DNS bölgesini seçin. Seçtiğiniz abonelikte zaten çeşitli kaynaklar varsa, DNS bölgesine kolaylıkla erişmek için **Ada göre filtrele** kutusuna **partners.contoso.net** adresini girebilirsiniz.

1. DNS bölgesi sayfasından ad sunucularını alın. Bu örnekte, contoso.net bölgesine ns1-01.azure-dns.com, ns2-01.azure-dns.net, ns3-01.azure-dns.org ve ns4-01.azure-dns.info ad sunucuları atanmıştır:

   ![Ad sunucularının listesi](./media/dns-domain-delegation/viewzonens500.png)

Azure DNS, bölgenizdeki yetkili NS kayıtlarını atanan ad sunucularını içerecek şekilde otomatik olarak oluşturur.  Ad sunucularını Azure PowerShell veya Azure CLI aracılığıyla görmek için bu kayıtları alın.

### <a name="create-a-name-server-record-in-the-parent-zone"></a>Üst bölgede bir ad sunucusu kaydı oluşturma

1. Azure portalında **contoso.net** DNS bölgesine gidin.
1. **+ Kayıt kümesi**’ni seçin.
1. **Kaynak kümesi ekle** sayfasında aşağıdaki değerleri girin ve **Tamam**’ı seçin.

   | **Ayar** | **Değer** | **Ayrıntılar** |
   |---|---|---|
   |**Ad**|iş ortakları|Alt DNS bölgesinin adını girin.|
   |**Tür**|NS|Ad sunucusu kayıtları için NS kullanın.|
   |**TTL**|1|Yaşam süresini girin.|
   |**TTL birimi**|Saat|Yaşam süresi birimini saat olarak ayarlayın.|
   |**AD SUNUCUSU**|{partners.contoso.net bölgesinden ad sunucuları}|partners.contoso.net bölgesindeki dört ad sunucusunun da adını girin. |

   ![Ad sunucusu kaydı için değerler](./media/dns-domain-delegation/partnerzone.png)


### <a name="delegate-subdomains-in-azure-dns-by-using-other-tools"></a>Azure DNS'de diğer araçları kullanarak alt etki alanlarını devretme

Aşağıdaki örneklerde, PowerShell ve Azure CLI kullanılarak Azure DNS’de alt etki alanlarını devretme adımları sağlanır.

#### <a name="powershell"></a>PowerShell

Aşağıdaki PowerShell örneğinde bunun nasıl çalıştığı gösterilmektedir. Aynı adımları Azure portalı veya platformlar arası Azure CLI aracılığıyla tamamlayabilirsiniz.

```powershell
# Create the parent and child zones. These can be in the same resource group or different resource groups, because Azure DNS is a global service.
$parent = New-AzureRmDnsZone -Name contoso.net -ResourceGroupName contosoRG
$child = New-AzureRmDnsZone -Name partners.contoso.net -ResourceGroupName contosoRG

# Retrieve the authoritative NS records from the child zone as shown in the next example. This information contains the name servers assigned to the child zone.
$child_ns_recordset = Get-AzureRmDnsRecordSet -Zone $child -Name "@" -RecordType NS

# Create the corresponding NS record set in the parent zone to complete the delegation. The record set name in the parent zone matches the child zone name (in this case, "partners").
$parent_ns_recordset = New-AzureRmDnsRecordSet -Zone $parent -Name "partners" -RecordType NS -Ttl 3600
$parent_ns_recordset.Records = $child_ns_recordset.Records
Set-AzureRmDnsRecordSet -RecordSet $parent_ns_recordset
```

Alt bölgenin SOA kaydına bakarak her şeyin doğru şekilde ayarlandığını doğrulamak için `nslookup` kullanın.

```
nslookup -type=SOA partners.contoso.com
```

```
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
```

#### <a name="azure-cli"></a>Azure CLI

```azurecli
#!/bin/bash

# Create the parent and child zones. These can be in the same resource group or different resource groups, because Azure DNS is a global service.
az network dns zone create -g contosoRG -n contoso.net
az network dns zone create -g contosoRG -n partners.contoso.net
```

Çıkıştan, `partners.contoso.net` bölgesinin ad sunucularını alın.

```
{
  "etag": "00000003-0000-0000-418f-250de2b2d201",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contosorg/providers/Microsoft.Network/dnszones/partners.contoso.net",
  "location": "global",
  "maxNumberOfRecordSets": 5000,
  "name": "partners.contoso.net",
  "nameServers": [
    "ns1-09.azure-dns.com.",
    "ns2-09.azure-dns.net.",
    "ns3-09.azure-dns.org.",
    "ns4-09.azure-dns.info."
  ],
  "numberOfRecordSets": 2,
  "resourceGroup": "contosorg",
  "tags": {},
  "type": "Microsoft.Network/dnszones"
}
```

Her ad sunucusu için kayıt kümesini ve NS kayıtlarını oluşturun.

```azurecli
#!/bin/bash

# Create the record set
az network dns record-set ns create --resource-group contosorg --zone-name contoso.net --name partners

# Create an NS record for each name server.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns1-09.azure-dns.com.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns2-09.azure-dns.net.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns3-09.azure-dns.org.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns4-09.azure-dns.info.
```

## <a name="delete-all-resources"></a>Tüm kaynakları silme

Bu makalede oluşturulan tüm kaynakları silmek için, aşağıdaki adımları tamamlayın:

1. Azure Portal **Sık Kullanılanlar** bölmesinde, **Tüm kaynaklar**’ı seçin. **Tüm kaynaklar** sayfasında, **contosorg** kaynak grubunu seçin. Seçtiğiniz abonelikte zaten çeşitli kaynaklar varsa, uygulama kaynak grubuna kolaylıkla erişmek için **Ada göre filtrele** kutusuna **contosorg** değerini girebilirsiniz.
1. **contosorg** sayfasında **Sil** düğmesini seçin.
1. Portal, silmek istediğinizi onaylamak için kaynak grubunun adını yazmanızı gerektirir. Kaynak grubu adı için **contosorg** yazıp **Sil**'i seçin. 

Bir kaynak grubunun silinmesi, kaynak grubundaki tüm kaynakların silinmesine neden olur. Bir kaynak grubunu silmeden önce her zaman grubun içeriğini onayladığınızdan emin olun. Portal, kaynak grubundaki tüm kaynakları sildikten sonra kaynak grubunu siler. Bu işlem birkaç dakika sürer.

## <a name="next-steps"></a>Sonraki adımlar

[DNS bölgelerini yönetme](dns-operations-dnszones.md)

[DNS kayıtlarını yönetme](dns-operations-recordsets.md)
