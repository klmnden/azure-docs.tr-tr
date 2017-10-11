---
title: "DNS bölgeleri ve kayıtları koruma | Microsoft Docs"
description: "DNS bölgeleri ve Microsoft Azure DNS kayıt kümelerini korumak nasıl."
services: dns
documentationcenter: na
author: jtuliani
manager: carmonm
ms.assetid: 190e69eb-e820-4fc8-8e9a-baaf0b3fb74a
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/20/2016
ms.author: jonatul
ms.openlocfilehash: 0b7040d6273b3a6b85cd55850d596807226b87fc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-protect-dns-zones-and-records"></a>DNS bölgeleri ve kayıtları korumak nasıl

DNS bölgeleri ve kayıtları kritik kaynaklardır. Bir DNS bölgesine veya bile yalnızca tek bir DNS kaydı silinirken bir toplam hizmet kesintisine neden olabilir.  Bu nedenle kritik DNS bölgeleri ve kayıtları yetkisiz veya yanlışlıkla değişikliklere karşı korumalı önemlidir.

Bu makalede, Azure DNS'ye nasıl, DNS bölgeleri ve kayıtları bu değişikliklere karşı korumanıza olanak tanır açıklanmaktadır.  Biz Azure Resource Manager tarafından sağlanan iki güçlü güvenlik özelliklerini uygulayın: [rol tabanlı erişim denetimi](../active-directory/role-based-access-control-what-is.md) ve [kaynak kilitleri](../azure-resource-manager/resource-group-lock-resources.md).

## <a name="role-based-access-control"></a>Rol tabanlı erişim denetimi

Azure rol tabanlı erişim denetimi (RBAC) Azure kullanıcıları, grupları ve kaynakları için ayrıntılı erişim yönetimi sağlar. RBAC kullanarak, kullanıcıların işlerini yapmak için gereksinim duyduğu tam olarak erişim miktarını verebilirsiniz. RBAC erişimi yönetmenize nasıl yardımcı olduğunu hakkında daha fazla bilgi için bkz: [rol tabanlı erişim denetimi nedir](../active-directory/role-based-access-control-what-is.md).

### <a name="the-dns-zone-contributor-role"></a>'DNS bölge katılımcı' rolü

'DNS bölge katılımcı' rolü DNS kaynakları yönetmek için Azure tarafından sağlanan yerleşik bir roldür.  Bir kullanıcı veya grup için DNS bölge katkıda bulunan izinleri atama DNS kaynakları, ancak kaynakları diğer herhangi bir tür değil yönetmek bu grubu sağlar.

Örneğin, Contoso Corporation için beş bölgeleri 'myzones' kaynak grubu içeren varsayalım. DNS Yöneticisi, kaynak grubuna 'DNS bölge katkıda bulunan' izin verme, bu DNS bölgeleri üzerinde tam denetim sağlar. Örneğin DNS Yöneticisi oluşturmak veya sanal makineleri durdurmayı gereksiz izinleri verme önler.

RBAC izinler atamak için en basit yolu [Azure Portalı aracılığıyla](../active-directory/role-based-access-control-configure.md).  Kaynak grubu için 'Erişim denetimi (IAM)' dikey penceresini açın, ardından 'Add','i tıklatın ardından 'DNS bölge katılımcı' rolü seçin ve gerekli kullanıcıları veya grupları izinleri vermek için seçin.

![Azure Portalı aracılığıyla kaynak grubu düzeyinde RBAC](./media/dns-protect-zones-recordsets/rbac1.png)

İzinleri de olabilir [Azure PowerShell kullanarak verilen](../active-directory/role-based-access-control-manage-access-powershell.md):

```powershell
# Grant 'DNS Zone Contributor' permissions to all zones in a resource group
New-AzureRmRoleAssignment -SignInName "<user email address>" -RoleDefinitionName "DNS Zone Contributor" -ResourceGroupName "<resource group name>"
```

Eşdeğer ayrıca komuttur [Azure CLI aracılığıyla kullanılabilen](../active-directory/role-based-access-control-manage-access-azure-cli.md):

```azurecli
# Grant 'DNS Zone Contributor' permissions to all zones in a resource group
azure role assignment create --signInName "<user email address>" --roleName "DNS Zone Contributor" --resourceGroup "<resource group name>"
```

### <a name="zone-level-rbac"></a>Bölge düzeyi RBAC

Azure RBAC kuralları, bir abonelik, bir kaynak grubu veya tek başına bir kaynak için uygulanabilir. Azure DNS söz konusu olduğunda, o kaynak tek tek bir DNS bölgesine veya hatta tek tek bir kayıt kümesi olabilir.

Örneğin, "contoso.com" bölgesi ve CNAME kayıtları her müşteri hesabı için oluşturulan bir'subzone customers.contoso.com' kaynak grubu 'myzones' bulunduğunu varsayın.  Bu CNAME kayıtları yönetmek için kullanılan hesabın yalnızca 'customers.contoso.com' bölgesinde kayıtları oluşturmak için izinlerin atanması, erişim için diğer bölgeler olmamalıdır.

Bölge düzeyi RBAC izinleri Azure portalı üzerinden verilebilir.  Bölge için 'Erişim denetimi (IAM)' dikey penceresini açın, ardından 'Add','ı tıklatın ardından 'DNS bölge katılımcı' rolü seçin ve gerekli kullanıcıları veya grupları izinleri vermek için seçin.

![Azure Portalı aracılığıyla düzeyi RBAC DNS bölgesi](./media/dns-protect-zones-recordsets/rbac2.png)

İzinleri de olabilir [Azure PowerShell kullanarak verilen](../active-directory/role-based-access-control-manage-access-powershell.md):

```powershell
# Grant 'DNS Zone Contributor' permissions to a specific zone
New-AzureRmRoleAssignment -SignInName "<user email address>" -RoleDefinitionName "DNS Zone Contributor" -ResourceGroupName "<resource group name>" -ResourceName "<zone name>" -ResourceType Microsoft.Network/DNSZones
```

Eşdeğer ayrıca komuttur [Azure CLI aracılığıyla kullanılabilen](../active-directory/role-based-access-control-manage-access-azure-cli.md):

```azurecli
# Grant 'DNS Zone Contributor' permissions to a specific zone
azure role assignment create --signInName <user email address> --roleName "DNS Zone Contributor" --resource-name <zone name> --resource-type Microsoft.Network/DNSZones --resource-group <resource group name>
```

### <a name="record-set-level-rbac"></a>Kayıt düzeyini RBAC ayarlayın

Biz bir adım daha fazla gidebilirsiniz. "Contoso.com" bölgenin tepesinde MX ve TXT kayıtları erişmesi gereken Contoso Corporation, posta Yöneticisi göz önünde bulundurun.  Aynen diğer MX veya TXT kaydı veya diğer herhangi bir türdeki herhangi bir kayıt erişimi gerekmez.  Azure DNS, tam olarak posta yönetici erişmesi kayıtları kayıt kümesi düzeyinde izinler atamak sağlar.  Posta yönetici kendisinin gerekir ve başka değişiklikler yapmak alamıyor tam denetim verilir.

Kayıt kümesi düzeyinde RBAC izinler kayıt kümesi dikey penceresinde 'Kullanıcılar' düğmesini kullanarak Azure Portalı aracılığıyla yapılandırılabilir:

![Düzey RBAC Azure portalı üzerinden kayıt kümesi](./media/dns-protect-zones-recordsets/rbac3.png)

Kayıt kümesi düzeyinde RBAC izinler de olabilir [Azure PowerShell kullanarak verilen](../active-directory/role-based-access-control-manage-access-powershell.md):

```powershell
# Grant permissions to a specific record set
New-AzureRmRoleAssignment -SignInName "<user email address>" -RoleDefinitionName "DNS Zone Contributor" -Scope "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/dnszones/<zone name>/<record type>/<record name>"
```

Eşdeğer ayrıca komuttur [Azure CLI aracılığıyla kullanılabilen](../active-directory/role-based-access-control-manage-access-azure-cli.md):

```azurecli
# Grant permissions to a specific record set
azure role assignment create --signInName "<user email address>" --roleName "DNS Zone Contributor" --scope "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/dnszones/<zone name>/<record type>/<record name>"
```

### <a name="custom-roles"></a>Özel roller

Yerleşik 'DNS bölge katılımcı' rolü DNS kaynak üzerinde tam denetim sağlar. Kendi müşteri bile geçirmiş denetimi sağlamak için Azure rolleri oluşturmak mümkündür.

Yeniden her Contoso Corporation müşteri hesabı için bir CNAME kaydı 'customers.contoso.com' bölgesinde oluşturulduğu örneği göz önünde bulundurun.  Bu CNAME'ler yönetmek için kullanılan hesabın CNAME kayıtlarına yalnızca yönetme izni verilmelidir.  Ardından, (MX kayıtları değiştirme gibi) diğer türleri kayıtlarını değiştirme veya bölge düzeyinde bölge silme gibi işlemleri gerçekleştirmek alamıyor.

Aşağıdaki örnek, CNAME kayıtlarına yalnızca yönetmek için özel rol tanımını gösterir:

```json
{
    "Name": "DNS CNAME Contributor",
    "Id": "",
    "IsCustom": true,
    "Description": "Can manage DNS CNAME records only.",
    "Actions": [
        "Microsoft.Network/dnsZones/CNAME/*",
        "Microsoft.Network/dnsZones/read",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
    ],
    "NotActions": [
    ],
    "AssignableScopes": [
        "/subscriptions/ c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ]
}
```

Eylemler özelliği aşağıdaki DNS özgü izinleri tanımlar:

* `Microsoft.Network/dnsZones/CNAME/*`CNAME kayıtları üzerinde tam denetim verir
* `Microsoft.Network/dnsZones/read`DNS bölgelerini okumak için ancak bunları CNAME oluşturulmaktadır bölge görmenizi etkinleştirme değiştirmek için izin verir.

Kalan Eylemler kopyalandığı [DNS bölge katkıda bulunan yerleşik rolü](../active-directory/role-based-access-built-in-roles.md#dns-zone-contributor).

> [!NOTE]
> Kayıt kümeleri hala güncelleştirilmesi vermeden etkili bir denetim olmamasına karşın silme engellemek için özel bir RBAC rolü kullanma. Kayıt kümeleri silinmesini engeller, ancak değiştirilen veren'in engel olmaz.  İzin verilen değişiklikleri ekleme ve kaldırma 'empty' kayıt kümesi bırakmak tüm kayıtları dahil olmak üzere kayıt kümesinden kayıt kaldırma içerir. Bu kayıt DNS çözümlemesi görüş açısından kümesine silme aynı etkiye sahiptir.

Özel rol tanımları şu anda Azure portalı üzerinden tanımlanamaz. Bu rol tanımına dayalı özel bir rol, Azure PowerShell kullanarak oluşturulabilir:

```powershell
# Create new role definition based on input file
New-AzureRmRoleDefinition -InputFile <file path>
```

Ayrıca Azure CLI oluşturulabilir:

```azurecli
# Create new role definition based on input file
azure role create -inputfile <file path>
```

Rol sonra yerleşik roller aynı şekilde bu makalenin önceki bölümlerinde açıklandığı gibi atanabilir.

Oluşturma, yönetme ve özel roller atama hakkında daha fazla bilgi için bkz: [Azure rbac'de özel roller](../active-directory/role-based-access-control-custom-roles.md).

## <a name="resource-locks"></a>Kaynak kilitleri

RBAC yanı sıra, Azure Resource Manager başka bir tür güvenlik denetimi, yani 'lock' Kaynakları özelliği destekler. Burada RBAC, denetimine izin verme kuralları belirli kullanıcılar ve gruplar eylemleri, kaynak kilitleri kaynağa uygulanır ve tüm kullanıcılar ve roller etkili olur. Daha fazla bilgi için bkz. [Azure Resource Manager ile kaynakları kilitleme](../azure-resource-manager/resource-group-lock-resources.md).

Kaynak kilidi iki tür vardır: **DoNotDelete** ve **salt okunur**. Bu, bir DNS bölgesine veya tek bir kayıt kümesine uygulanabilir.  Aşağıdaki bölümlerde birçok yaygın senaryolar ve bunları destekleyen anlatan kaynak kilitleri kullanma.

### <a name="protecting-against-all-changes"></a>Tüm değişiklikleri karşı koruma

Yapılan değişiklikleri önlemek için salt okunur kilit bölgeye uygulanır.  Bu yeni kayıt kümeleri değiştirilmiş veya silinmiş gelen oluşturulan ve varolan kaydı kümeleri engeller.

Bölge düzeyi kaynak kilitleri Azure portalı üzerinden oluşturulabilir.  DNS bölge dikey penceresinden 'Kilitleri','ı tıklatın ardından 'Ekle':

![Bölge düzeyi kaynak kilitleri Azure Portalı aracılığıyla](./media/dns-protect-zones-recordsets/locks1.png)

Bölge düzeyi kaynak kilitleri Azure PowerShell ile de oluşturulabilir:

```powershell
# Lock a DNS zone
New-AzureRmResourceLock -LockLevel <lock level> -LockName <lock name> -ResourceName <zone name> -ResourceType Microsoft.Network/DNSZones -ResourceGroupName <resource group name>
```

Azure kaynak kilitleri yapılandırma, Azure CLI şu anda desteklenmiyor.

### <a name="protecting-individual-records"></a>Kayıtları tek tek koruma

Var olan bir DNS kayıt karşı değişiklik kümesi önlemek için kayıt kümesi salt okunur kilit uygulayın.

> [!NOTE]
> Bir kayıt kümesine DoNotDelete kilit uygulama etkin bir denetimi değil. Kayıt kümesine silinmesini engeller, ancak bu, değiştirilmesini engellemez.  İzin verilen değişiklikleri ekleme ve kaldırma 'empty' kayıt kümesi bırakmak tüm kayıtları dahil olmak üzere kayıt kümesinden kayıt kaldırma içerir. Bu kayıt DNS çözümlemesi görüş açısından kümesine silme aynı etkiye sahiptir.

Kayıt kümesi düzeyi kaynak kilitleri için şu anda yalnızca Azure PowerShell kullanılarak yapılandırılır.  Azure portalında veya Azure CLI desteklenmez.

```powershell
# Lock a DNS record set
New-AzureRmResourceLock -LockLevel <lock level> -LockName "<lock name>" -ResourceName "<zone name>/<record set name>" -ResourceType "Microsoft.Network/DNSZones/<record type>" -ResourceGroupName "<resource group name>"
```

### <a name="protecting-against-zone-deletion"></a>Bölge silinmeye karşı koruma

Azure DNS'de bölge silindiğinde, bölgedeki tüm kayıt kümelerinin de silinir.  Bu işlem geri alınamaz.  Kritik bir bölgenin yanlışlıkla silinmesi önemli iş etkisine sahip olma olasılığı vardır.  Bu nedenle yanlışlıkla bölge silinmeye karşı korumak çok önemlidir.

Bir bölgeye DoNotDelete kilit uygulama bölge silinmesini engeller.  Ancak, kilit alt kaynaklar tarafından devralınır olduğundan, ayrıca tüm kayıt kümelerini bölgesinde siliniyor, gelen istenmeyen olabilen engeller.  Kayıtları hala mevcut kayıt kümelerinden kaldırılabilir Ayrıca, yukarıdaki not açıklandığı gibi bu da etkisiz olduğu.

Alternatif olarak, bölgenin SOA kayıt kümesi gibi kümesindeki bir kayda DoNotDelete kilit uygulamayı düşünün.  Kayıt kümeleri de silmeden bölge silinemiyor olduğundan, bu hala serbestçe değiştirilecek bölge içinde kayıt kümeleri vererek bölge silme korur. Bölgeyi silmek için bir girişimde, Azure Resource Manager bunu SOA kayıt kümesi de siler ve SOA kilitlendiğinden çağrı engeller algılar.  Hiçbir kayıt kümelerini silinir.

Aşağıdaki PowerShell komutunu verilen bölgenin SOA kaydına karşı DoNotDelete Kilitle oluşturur:

```powershell
# Protect against zone delete with DoNotDelete lock on the record set
New-AzureRmResourceLock -LockLevel DoNotDelete -LockName "<lock name>" -ResourceName "<zone name>/@" -ResourceType" Microsoft.Network/DNSZones/SOA" -ResourceGroupName "<resource group name>"
```

Yanlışlıkla bölge silinmesini önlemek için başka bir işleç emin olmak için özel bir güvenlik rolünü kullanarak yoludur ve bölgelerinizi yönetmek için kullanılan hizmet hesaplarını izinleri bölge yok. Bir bölge silmek gerektiğinde bir iki adımlı silme, ilk izni veriliyor bölge Sil (kapsamındaki izinler yanlış bölgeyi silmeden önlemek için bölge,) ve ikinci bölgeyi silme uygulayabilir.

Bu ikinci yaklaşımı kilitleri oluşturmak anımsamak zorunda kalmadan hesaplar tarafından erişilen tüm bölgelerde çalıştığını avantajına sahiptir. Abonelik sahibi gibi bölge silme izinlerine sahip herhangi bir hesabı kritik bir bölge hala yanlışlıkla silebilirsiniz dezavantajı vardır.

Aynı zamanda, savunma yaklaşım DNS bölge koruma için her iki yaklaşımın - kaynak kilitler ve özel roller - kullanmak da mümkündür.

## <a name="next-steps"></a>Sonraki adımlar

* RBAC ile çalışma hakkında daha fazla bilgi için bkz: [Azure portalında erişim yönetimini kullanmaya başlama](../active-directory/role-based-access-control-what-is.md).
* Kaynak kilitleri ile çalışma hakkında daha fazla bilgi için bkz: [Azure Resource Manager ile kaynakları kilitleme](../azure-resource-manager/resource-group-lock-resources.md).

