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
ms.date: 04/12/2017
ms.author: kumud
ms.openlocfilehash: d73a42fd0f41c20b516c0348c86b40202fd06f53
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="delegate-a-domain-to-azure-dns"></a>Azure DNS'ye bir etki alanı devretme

Azure DNS, bir DNS bölgesi barındırmanızı ve Azure'de bir etki alanı için DNS kayıtlarını yönetmenizi sağlar. Bir etki alanının DNS sorgularının Azure DNS'ye erişmesi için, etki alanının Azure DNS'ye üst etki alanından devredilmiş olması gerekir. Azure DNS'nin etki alanı kayıt şirketi olmadığını unutmayın. Bu makalede etki alanınızı Azure DNS’ye devretme işlemi açıklanır.

Bir kayıt şirketinden satın alınan etki alanları için, kayıt şirketiniz bu NS kayıtlarını ayarlama seçeneğini sunar. Azure DNS'de aynı etki alanı adıyla DNS bölgesi oluşturmak için bir etki alanına sahip olmanız gerekmez. Ancak Azure DNS'ye temsilci seçmeyi kayıt şirketi ile ayarlamak için etki alanına sahip olmanız gerekir.

Örneğin, "contoso.net" etki alanını satın aldığınızı ve Azure DNS'de "contoso.net" adlı bir bölge oluşturduğunuzu varsayalım. Etki alanı sahibi olarak, kayıt şirketiniz size etki alanınız için ad sunucusu adreslerini (yani NS kayıtlarını) yapılandırma seçeneğini sunar. Kayıt şirketi bu NS kayıtlarını üst etki alanında (bu durumda “.net”te) depolar. Ardından, dünya genelindeki istemciler “contoso.net”teki DNS kayıtlarını çözümlemeye çalışırken Azure DNS bölgesindeki etki alanınıza yönlendirebilir.

## <a name="create-a-dns-zone"></a>DNS bölgesi oluşturma

1. Azure portalında oturum açın
1. Hub menüsünde **Yeni > Ağ >** ve ardından **DNS bölgesi**’ne tıklayarak DNS bölgesi oluştur sayfasını açın.

    ![DNS bölgesi](./media/dns-domain-delegation/dns.png)

1. **DNS bölgesi oluştur** sayfasında aşağıdaki değerleri girin ve **Oluştur**’a tıklayın:

   | **Ayar** | **Değer** | **Ayrıntılar** |
   |---|---|---|
   |**Ad**|contoso.net|DNS bölgesinin adı|
   |**Abonelik**|[Aboneliğiniz]|Uygulama ağ geçidinin oluşturulacağı bir abonelik seçin.|
   |**Kaynak grubu**|**Yeni oluştur:** contosoRG|Bir kaynak grubu oluşturun. Kaynak grubu adı, seçili abonelik içinde benzersiz olmalıdır. Kaynak grupları hakkında daha fazla bilgi için, [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups)’a genel bakış makalesini okuyun.|
   |**Konum**|Batı ABD||

> [!NOTE]
> Kaynak grubu, kaynak grubunun konumunu ifade eder ve DNS bölgesini etkilemez. DNS bölgesinin konumu her zaman "genel" şeklindedir ve gösterilmez.

## <a name="retrieve-name-servers"></a>Ad sunucularını alma

DNS bölgenizi Azure DNS'ye devretmeden önce, bölgenizin ad sunucusu adlarını bilmeniz gerekir. Azure DNS, her bölge oluşturmada bir havuzdan ad sunucuları ayırır.

1. Oluşturulan DNS bölgesiyle, Azure Portal **Sık Kullanılanlar** bölmesinde, **Tüm kaynaklar**’a tıklayın. **Tüm kaynaklar** sayfasında **contoso.net** DNS bölgesine tıklayın. Seçili abonelikte zaten çeşitli kaynaklar varsa, uygulama ağ geçidine kolaylıkla erişmek için Ada göre filtrele... kutusuna **contoso.net** girebilirsiniz. 

1. DNS bölgesi sayfasından ad sunucularını alın. Bu örnekte, "contoso.net" bölgesine "ns1-01.azure-dns.com", "ns2-01.azure-dns.net", "ns3-01.azure-dns.org" ve "ns4-01.azure-dns.info" ad sunucuları atanmıştır:

 ![Dns-nameserver](./media/dns-domain-delegation/viewzonens500.png)

Azure DNS, atanan ad sunucularını içeren yetkili NS kayıtlarını bölgenizde otomatik olarak oluşturur.  Ad sunucusu adlarını Azure PowerShell veya Azure CLI aracılığıyla görmek için bu kayıtları almanız yeterlidir.

Aşağıdaki örneklerde, PowerShell ve Azure CLI ile Azure DNS içindeki bir bölgenin ad sunucularını alma adımları da sağlanır.

### <a name="powershell"></a>PowerShell

```powershell
# The record name "@" is used to refer to records at the top of the zone.
$zone = Get-AzureRmDnsZone -Name contoso.net -ResourceGroupName contosoRG
Get-AzureRmDnsRecordSet -Name "@" -RecordType NS -Zone $zone
```

Aşağıdaki örnek, yanıttır.

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

Aşağıdaki örnek, yanıttır.

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

Artık DNS bölgesi oluşturulduğuna ve ad sunucularınız olduğuna göre, üst etki alanının Azure DNS ad sunucularıyla güncelleştirilmesi gerekir. Her kayıt şirketi, bir etki alanının ad sunucusu kayıtlarını değiştirmek için kendi DNS yönetim araçlarına sahiptir. Kayıt şirketinin DNS yönetim sayfasında NS kayıtlarını düzenleyin ve NS kayıtlarını Azure DNS'nin oluşturduklarıyla değiştirin.

Bir etki alanını Azure DNS'ye devrederken Azure DNS tarafından sağlanan ad sunucusu adlarını kullanmanız gerekir. Etki alanınızın adından bağımsız olarak her zaman dört sunucu adını da kullanmanız önerilir. Etki alanı temsilcisi, sunucu adının etki alanınızla aynı üst düzey etki alanını kullanmasını gerektirmez.

Azure DNS ad sunucusu IP adresleri gelecekte değişebileceği için, bu IP adreslerine işaret ederken "birleştirici kayıtlar"ı kullanmayın. Kendi bölgenizdeki ad sunucusu adlarını kullanan ve bazen "gösterim ad sunucuları" olarak adlandırılan temsilci seçimleri, Azure DNS'de şu anda desteklenmemektedir.

## <a name="verify-name-resolution-is-working"></a>Ad çözümlemesinin çalıştığını doğrulama

Temsilci seçmeyi tamamladıktan sonra, bölgenizin SOA kaydını (bölge oluşturulduğunda bu da otomatik olarak oluşturulur) sorgulamak için "nslookup" gibi bir araç kullanarak ad çözümlemesinin çalışıp çalışmadığını doğrulayabilirsiniz.

Azure DNS ad sunucularını belirtmeniz gerekmez; temsil doğru şekilde ayarlandıysa, normal DNS çözümleme işlemi ad sunucularını otomatik olarak bulur.

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

## <a name="delegate-sub-domains-in-azure-dns"></a>Azure DNS'de alt etki alanlarını devretme

Ayrı bir alt bölge kurmak istiyorsanız Azure DNS'de bir alt etki alanını devredebilirsiniz. Örneğin, Azure DNS'de ayarladığınız ve devrettiğiniz "contoso.net" için "partners.contoso.net" olarak ayrı bir alt bölge ayarlamak istediğinizi varsayalım.

1. Azure DNS'de "partners.contoso.net" alt bölgesini oluşturun.
2. Azure DNS'de alt bölgeyi barındıran ad sunucularını almak için, alt bölgedeki yetkili NS kayıtlarını arayın.
3. Üst bölgeden alt bölgeye işaret eden NS kayıtlarını yapılandırarak alt bölgeyi devredin.

### <a name="create-a-dns-zone"></a>DNS bölgesi oluşturma

1. Azure portalında oturum açın
1. Hub menüsünde **Yeni > Ağ >** ve ardından **DNS bölgesi**’ne tıklayarak DNS bölgesi oluştur sayfasını açın.

    ![DNS bölgesi](./media/dns-domain-delegation/dns.png)

1. **DNS bölgesi oluştur** sayfasında aşağıdaki değerleri girin ve **Oluştur**’a tıklayın:

   | **Ayar** | **Değer** | **Ayrıntılar** |
   |---|---|---|
   |**Ad**|partners.contoso.net|DNS bölgesinin adı|
   |**Abonelik**|[Aboneliğiniz]|Uygulama ağ geçidinin oluşturulacağı bir abonelik seçin.|
   |**Kaynak grubu**|**Varolanı kullan:** contosoRG|Bir kaynak grubu oluşturun. Kaynak grubu adı, seçili abonelik içinde benzersiz olmalıdır. Kaynak grupları hakkında daha fazla bilgi için, [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups)’a genel bakış makalesini okuyun.|
   |**Konum**|Batı ABD||

> [!NOTE]
> Kaynak grubu, kaynak grubunun konumunu ifade eder ve DNS bölgesini etkilemez. DNS bölgesinin konumu her zaman "genel" şeklindedir ve gösterilmez.

### <a name="retrieve-name-servers"></a>Ad sunucularını alma

1. Oluşturulan DNS bölgesiyle, Azure Portal **Sık Kullanılanlar** bölmesinde, **Tüm kaynaklar**’a tıklayın. **Tüm kaynaklar** sayfasında **partners.contoso.net** DNS bölgesine tıklayın. Seçili abonelikte zaten çeşitli kaynaklar varsa, DNS bölgesine kolaylıkla erişmek için Ada göre filtrele... kutusuna **partners.contoso.net** girebilirsiniz.

1. DNS bölgesi sayfasından ad sunucularını alın. Bu örnekte, "contoso.net" bölgesine "ns1-01.azure-dns.com", "ns2-01.azure-dns.net", "ns3-01.azure-dns.org" ve "ns4-01.azure-dns.info" ad sunucuları atanmıştır:

 ![Dns-nameserver](./media/dns-domain-delegation/viewzonens500.png)

Azure DNS, atanan ad sunucularını içeren yetkili NS kayıtlarını bölgenizde otomatik olarak oluşturur.  Ad sunucusu adlarını Azure PowerShell veya Azure CLI aracılığıyla görmek için bu kayıtları almanız yeterlidir.

### <a name="create-name-server-record-in-parent-zone"></a>Üst bölgede ad sunucusu kaydı oluşturma

1. Azure Portal’da **contoso.net** DNS bölgesine gidin.
1. **+ Kayıt kümesi**’ne tıklayın
1. **Kaynak kümesi ekle** sayfasında aşağıdaki değerleri girin, ardından **Tamam**’a tıklayın:

   | **Ayar** | **Değer** | **Ayrıntılar** |
   |---|---|---|
   |**Ad**|iş ortakları|Alt DNS bölgesinin adı|
   |**Tür**|NS|Ad sunucusu kayıtları için NS kullanın.|
   |**TTL**|1|Yaşam süresi.|
   |**TTL birimi**|Saat|yaşam süresi birimi olarak saati ayarlar|
   |**AD SUNUCUSU**|{partners.contoso.net bölgesinden ad sunucuları}|Partners.contoso.net bölgesindeki 4 ad sunucusunun adını da girin. |

   ![Dns-nameserver](./media/dns-domain-delegation/partnerzone.png)


### <a name="delegating-sub-domains-in-azure-dns-with-other-tools"></a>Diğer araçlarla Azure DNS'de alt etki alanlarını devretme

Aşağıdaki örneklerde, PowerShell ve CLI ile Azure DNS’de alt etki alanlarını devretme adımları sağlanır:

#### <a name="powershell"></a>PowerShell

Aşağıdaki PowerShell örneğinde bunun nasıl çalıştığı gösterilmektedir. Aynı adımlar Azure portalı veya platformlar arası Azure CLI yoluyla gerçekleştirilebilir.

```powershell
# Create the parent and child zones. These can be in same resource group or different resource groups as Azure DNS is a global service.
$parent = New-AzureRmDnsZone -Name contoso.net -ResourceGroupName contosoRG
$child = New-AzureRmDnsZone -Name partners.contoso.net -ResourceGroupName contosoRG

# Retrieve the authoritative NS records from the child zone as shown in the next example. This contains the name servers assigned to the child zone.
$child_ns_recordset = Get-AzureRmDnsRecordSet -Zone $child -Name "@" -RecordType NS

# Create the corresponding NS record set in the parent zone to complete the delegation. The record set name in the parent zone matches the child zone name, in this case "partners".
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

# Create the parent and child zones. These can be in same resource group or different resource groups as Azure DNS is a global service.
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

# Create a ns record for each name server.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns1-09.azure-dns.com.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns2-09.azure-dns.net.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns3-09.azure-dns.org.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns4-09.azure-dns.info.
```

## <a name="delete-all-resources"></a>Tüm kaynakları silme

Bu makalede oluşturulan tüm kaynakları silmek için, aşağıdaki adımları tamamlayın:

1. Azure Portal **Sık Kullanılanlar** bölmesinde, **Tüm kaynaklar**’a tıklayın. Tüm kaynaklar sayfasında **contosorg** kaynak grubuna tıklayın. Seçili abonelikte zaten çeşitli kaynaklar varsa, kaynak grubuna kolaylıkla erişmek için **Ada göre filtrele...** kutusuna **contosorg** girebilirsiniz.
1. **contosorg** sayfasında **Sil** düğmesine tıklayın.
1. Portal, silmek istediğinizi onaylamak için kaynak grubunun adını yazmanızı gerektirir. Kaynak grubu adı için *contosorg* yazın ve **Sil**'e tıklayın. Bir kaynak grubunun silinmesiyle, kaynak grubu içerisindeki tüm kaynaklar silinir, bu nedenle, silmeden önce kaynak grubunun içeriğini onaylamayı hiçbir zaman unutmayın. Portal, kaynak grubu içinde yer alan tüm kaynakları siler ve sonra kaynak grubunu siler. Bu işlem birkaç dakika sürer.

## <a name="next-steps"></a>Sonraki adımlar

[DNS bölgelerini yönetme](dns-operations-dnszones.md)

[DNS kayıtlarını yönetme](dns-operations-recordsets.md)
