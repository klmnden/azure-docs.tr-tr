---
title: DNS bölgelerini ve kayıtlarını koruma | Microsoft Docs
description: Microsoft Azure DNS'te DNS bölgelerini ve kayıt nasıl Koruyabileceğiniz ayarlar.
services: dns
documentationcenter: na
author: vhorne
manager: jeconnoc
ms.assetid: 190e69eb-e820-4fc8-8e9a-baaf0b3fb74a
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/20/2016
ms.author: victorh
ms.openlocfilehash: ff20c16c89ca5bf27bfddc654119b428cc425d2d
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39173483"
---
# <a name="how-to-protect-dns-zones-and-records"></a>DNS bölgelerini ve kayıtlarını koruma

DNS bölgelerini ve kayıtlarını kritik kaynaklardır. Bir DNS bölgesi veya hatta yalnızca tek bir DNS kaydı silindiğinde, toplam kesintisi neden olabilir.  Bu nedenle kritik DNS bölgeleri ve kayıtları, yetkilendirilmemiş veya yanlışlıkla yapılan değişikliklere karşı korumalı önemlidir.

Bu makalede, nasıl Azure DNS, DNS bölgeleri ve kayıtları bu değişikliklere karşı korumanıza olanak tanır açıklanmaktadır.  Biz Azure Resource Manager tarafından sağlanan iki güçlü güvenlik özellikleri geçerlidir: [rol tabanlı erişim denetimi](../role-based-access-control/overview.md) ve [kaynak kilitleri](../azure-resource-manager/resource-group-lock-resources.md).

## <a name="role-based-access-control"></a>Rol tabanlı erişim denetimi

Azure rol tabanlı erişim denetimi (RBAC) ayrıntılı erişim yönetimi için Azure kullanıcıları, grupları ve kaynaklar sağlar. RBAC kullanarak, kullanıcıların işlerini yapması için gereken tam erişim miktarını verebilirsiniz. RBAC erişimi yönetmenize nasıl yardımcı olduğunu hakkında daha fazla bilgi için bkz. [rol tabanlı erişim denetimi nedir](../role-based-access-control/overview.md).

### <a name="the-dns-zone-contributor-role"></a>'DNS bölgesi katkıda bulunanı' rolü

'DNS bölgesi katkıda bulunanı' rolü, DNS kaynaklarını yönetmek için Azure tarafından sağlanan yerleşik bir roldür.  DNS bölgesi katkıda bulunan izinleri bir kullanıcı veya grup için atama DNS kaynakları, ancak herhangi bir tür değil kaynakları yönetmek bu grubu sağlar.

Örneğin, kaynak grubu 'myzones' Contoso Corporation için beş bölgeleri içeren varsayalım. DNS Yöneticisi, bu kaynak grubunda 'DNS bölgesi katkıda bulunanı' izinleri verme, bu DNS bölgelerini üzerinde tam denetim sağlar. Ayrıca, örneğin DNS Yöneticisi oluşturulamıyor veya sanal makineleri durdurmanın gereksiz izinler verme önler.

RBAC izinlerinin atamak için en basit yolu [Azure portalından](../role-based-access-control/role-assignments-portal.md).  Kaynak grubu için 'Erişim denetimi (IAM)' dikey penceresini açın, ardından 'Ekle' seçeneğine tıklayın ardından 'DNS bölgesi katkıda bulunanı' rolü seçin ve izinleri vermek için gerekli kullanıcıları veya grupları seçin.

![Azure portal aracılığıyla kaynak grubu düzeyinde RBAC](./media/dns-protect-zones-recordsets/rbac1.png)

İzinleri de olabilir [Azure PowerShell kullanarak verilen](../role-based-access-control/role-assignments-powershell.md):

```powershell
# Grant 'DNS Zone Contributor' permissions to all zones in a resource group
New-AzureRmRoleAssignment -SignInName "<user email address>" -RoleDefinitionName "DNS Zone Contributor" -ResourceGroupName "<resource group name>"
```

Eşdeğer komut ayrıca olduğundan [Azure CLI aracılığıyla](../role-based-access-control/role-assignments-cli.md):

```azurecli
# Grant 'DNS Zone Contributor' permissions to all zones in a resource group
azure role assignment create --signInName "<user email address>" --roleName "DNS Zone Contributor" --resourceGroup "<resource group name>"
```

### <a name="zone-level-rbac"></a>Bölge düzeyi RBAC

Azure RBAC kuralları, bir aboneliğe, kaynak grubu veya ayrı bir kaynak için uygulanabilir. Azure DNS söz konusu olduğunda, ilgili kaynağı tek bir DNS bölgesi veya hatta ayrı bir kayıt kümesi olabilir.

Örneğin, "contoso.com" bölgesi ve CNAME kayıtları her müşteri hesabı için oluşturulan bir'subzone customers.contoso.com' kaynak grubu 'myzones' içerdiğini varsayın.  Bu CNAME kayıtları yönetmek için kullanılan hesap yalnızca 'customers.contoso.com' bölgesinde kayıt oluşturma izni atanması, diğer bölgelere erişimi olmamalıdır.

Azure portalı üzerinden bir bölge düzeyinde RBAC izinlerinin verilmesi.  Bölge 'Erişim denetimi (IAM)' dikey penceresini açın, ardından 'Ekle' seçeneğine tıklayın ardından 'DNS bölgesi katkıda bulunanı' rolü seçin ve izinleri vermek için gerekli kullanıcıları veya grupları seçin.

![DNS bölgesi düzeyi RBAC Azure portal aracılığıyla](./media/dns-protect-zones-recordsets/rbac2.png)

İzinleri de olabilir [Azure PowerShell kullanarak verilen](../role-based-access-control/role-assignments-powershell.md):

```powershell
# Grant 'DNS Zone Contributor' permissions to a specific zone
New-AzureRmRoleAssignment -SignInName "<user email address>" -RoleDefinitionName "DNS Zone Contributor" -ResourceGroupName "<resource group name>" -ResourceName "<zone name>" -ResourceType Microsoft.Network/DNSZones
```

Eşdeğer komut ayrıca olduğundan [Azure CLI aracılığıyla](../role-based-access-control/role-assignments-cli.md):

```azurecli
# Grant 'DNS Zone Contributor' permissions to a specific zone
azure role assignment create --signInName <user email address> --roleName "DNS Zone Contributor" --resource-name <zone name> --resource-type Microsoft.Network/DNSZones --resource-group <resource group name>
```

### <a name="record-set-level-rbac"></a>Kayıt düzeyi RBAC

Bir adım daha fazla ayrıntıya. "Contoso.com" bölgesi tepesindeki MX ve TXT kayıtlarını erişmesi gereken Contoso Corporation, posta Yöneticisi göz önünde bulundurun.  Diğer MX veya TXT kaydı veya diğer herhangi bir tür herhangi bir kayıt erişimi geliştirmiyor.  Azure DNS kayıt kümesi düzeyinde izinleri tam olarak posta yönetici erişmesi kayıtları atamanıza olanak sağlar.  Posta yönetici ihtiyacı ve herhangi bir değişiklik yapmak silemiyor tam denetim izni verilir.

Kayıt kümesi düzeyi RBAC izinlerinin kayıt kümesi dikey penceresinde 'Kullanıcılar' düğmesini kullanarak Azure portal aracılığıyla yapılandırılabilir:

![RBAC Azure portal aracılığıyla düzeyi kayıt kümesi](./media/dns-protect-zones-recordsets/rbac3.png)

Kayıt kümesi düzeyi RBAC izinlerinin de olabilir [Azure PowerShell kullanarak verilen](../role-based-access-control/role-assignments-powershell.md):

```powershell
# Grant permissions to a specific record set
New-AzureRmRoleAssignment -SignInName "<user email address>" -RoleDefinitionName "DNS Zone Contributor" -Scope "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/dnszones/<zone name>/<record type>/<record name>"
```

Eşdeğer komut ayrıca olduğundan [Azure CLI aracılığıyla](../role-based-access-control/role-assignments-cli.md):

```azurecli
# Grant permissions to a specific record set
azure role assignment create --signInName "<user email address>" --roleName "DNS Zone Contributor" --scope "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/dnszones/<zone name>/<record type>/<record name>"
```

### <a name="custom-roles"></a>Özel roller

Yerleşik 'DNS bölgesi katkıda bulunanı' rolü bir DNS kaynağı üzerinde tam denetim sağlar. Kendi müşteri bile daha ayrıntılı bir denetim sağlamak için Azure rolü oluşturmak mümkündür.

Yeniden her Contoso Corporation müşteri hesabı için bir CNAME kaydı ' % s'bölgesinde 'customers.contoso.com' oluşturulduğu örneği göz önünde bulundurun.  Bu CNAME yönetmek için kullanılan hesabın, CNAME kayıtları yönetme izni verilmelidir.  Ardından, kayıtları (MX kaydı değiştirme gibi) diğer tür değiştirme veya bölge düzeyinde bölge silme gibi işlemleri silemiyor.

Aşağıdaki örnek, CNAME kayıtları yönetmek için bir özel rol tanımını gösterir:

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

Actions özelliği aşağıdaki DNS özel izinleri tanımlar:

* `Microsoft.Network/dnsZones/CNAME/*` CNAME kayıtları üzerinde tam denetim verir
* `Microsoft.Network/dnsZones/read` DNS bölgelerini okuyabilirler, ancak bunları CNAME oluşturulduğu bölgenin görmesini sağlayarak değiştirmek için izin verir.

Kalan eylemleri kopyalandığı [yerleşik DNS bölgesi katkıda bulunanı rolü](../role-based-access-control/built-in-roles.md#dns-zone-contributor).

> [!NOTE]
> Yine de bunları güncelleştirilecek sağlayan etkili bir denetim değilken kayıt kümelerini silmek engellemek için özel bir RBAC rolü kullanarak. Kayıt kümeleri silinmesini engeller ancak değiştirilmesini engel olmaz.  İzin verilen değişiklikleri ekleme ve bir 'empty' kayıt kümesi bırakmak için tüm kayıtları kaldırarak dahil olmak üzere kayıt kümesinden kayıt kaldırma içerir. Bu kaydı bir DNS çözümlemesi bakış açısından kümesi silme aynı etkiye sahiptir.

Özel rol tanımları, şu anda Azure portalı üzerinden tanımlanamaz. Bu rol tanımını temel alan özel bir rol, Azure PowerShell kullanarak oluşturulabilir:

```powershell
# Create new role definition based on input file
New-AzureRmRoleDefinition -InputFile <file path>
```

Ayrıca Azure CLI oluşturulabilir:

```azurecli
# Create new role definition based on input file
azure role create -inputfile <file path>
```

Rol sonra yerleşik rolleri aynı şekilde bu makalenin önceki bölümlerinde açıklandığı şekilde atanabilir.

Oluşturma, yönetme ve özel roller atama hakkında daha fazla bilgi için bkz. [Azure rbac'de özel roller](../role-based-access-control/custom-roles.md).

## <a name="resource-locks"></a>Kaynak kilitleri

RBAC ek olarak, Azure Resource Manager, başka türde bir güvenlik denetimi, yani 'lock' Kaynakları özelliği destekler. Burada denetimine izin RBAC kuralları belirli kullanıcılar ve gruplar eylemleri, kaynak kilitleri kaynağa uygulanır ve tüm kullanıcılar ve roller etkili olur. Daha fazla bilgi için bkz. [Azure Resource Manager ile kaynakları kilitleme](../azure-resource-manager/resource-group-lock-resources.md).

Kaynak kilidi iki tür vardır: **DoNotDelete** ve **salt okunur**. Bu işlem, bir DNS bölgesine veya bireysel bir kayıt kümesi için de uygulanabilir.  Aşağıdaki bölümlerde bazı yaygın senaryolar ve bunları desteklemek nasıl açıklanmaktadır kaynak kilitleri kullanma.

### <a name="protecting-against-all-changes"></a>Tüm değişikliklere karşı koruma

Yapılan her değişikliği engellemek için bir salt okunur kilidi bölge için geçerli olur.  Bu, yeni bir kayıt kümesi silindi veya değiştirilmesini öğesinden oluşturulan ve var olan kayıt kümelerini olmasını önler.

Bölge düzeyi kaynak kilitleri, Azure portalı üzerinden oluşturulabilir.  DNS bölgesi dikey penceresinden 'Kilitleri','ı tıklatın. ardından 'Ekle':

![Azure portal aracılığıyla bölge düzeyinde kaynak kilitleri](./media/dns-protect-zones-recordsets/locks1.png)

Bölge düzeyi kaynak kilitleri, Azure PowerShell oluşturulabilir:

```powershell
# Lock a DNS zone
New-AzureRmResourceLock -LockLevel <lock level> -LockName <lock name> -ResourceName <zone name> -ResourceType Microsoft.Network/DNSZones -ResourceGroupName <resource group name>
```

Azure kaynak kilitlerinin yapılandırılması, Azure CLI şu anda desteklenmiyor.

### <a name="protecting-individual-records"></a>Kayıtları tek tek koruma

Mevcut bir DNS kayıt karşı değişiklik kümesi önlemek için kayıt kümesi için bir salt okunur kilidi uygulayın.

> [!NOTE]
> Bir kayıt kümesine DoNotDelete kilit uygulama etkili bir denetimi değil. Kayıt kümesi silinmesini engeller, ancak bu, değiştirilmesini önlemez.  İzin verilen değişiklikleri ekleme ve bir 'empty' kayıt kümesi bırakmak için tüm kayıtları kaldırarak dahil olmak üzere kayıt kümesinden kayıt kaldırma içerir. Bu kaydı bir DNS çözümlemesi bakış açısından kümesi silme aynı etkiye sahiptir.

Kayıt kümesi düzeyinde kaynak kilitleri için şu anda yalnızca Azure PowerShell kullanılarak yapılandırılır.  Bunlar, Azure portalı veya Azure CLI içinde desteklenmez.

```powershell
# Lock a DNS record set
New-AzureRmResourceLock -LockLevel <lock level> -LockName "<lock name>" -ResourceName "<zone name>/<record set name>" -ResourceType "Microsoft.Network/DNSZones/<record type>" -ResourceGroupName "<resource group name>"
```

### <a name="protecting-against-zone-deletion"></a>Bölge silinmeye karşı koruma

Azure DNS'de bölge silindiğinde, bu bölgedeki tüm kayıt kümelerini da silinir.  Bu işlem geri alınamaz.  Kritik bir bölgenin yanlışlıkla silme önemli iş etkisine sahip olma olasılığı vardır.  Bu nedenle bölgenin yanlışlıkla silinmesine karşı korumak çok önemlidir.

Bir bölgeye DoNotDelete kilit uygulama bölge silinmesini engeller.  Ancak kilit alt kaynaklar tarafından devralınır olduğundan, ayrıca tüm kayıt kümelerini olabilecek istenmeyen siliniyor, ilk alan engeller.  Kayıtları hala var olan kayıt kümesinden kaldırılabilir beri Ayrıca, yukarıdaki not açıklandığı gibi ayrıca etkisiz olur.

Alternatif olarak, bir kayıt kümesi SOA kaydı kümesi gibi bölgesinde DoNotDelete kilidi uygulamak göz önünde bulundurun.  Kayıt kümelerini silmeden bölgesi silinemiyor olduğundan, bu sağlarken hala kayıt kümeleri serbestçe değiştirilecek bölge içinde bölge silme karşı korur. Bir bölgeyi silme denenirse, Azure Resource Manager bu SOA kayıt kümesi da siler ve SOA kilitli olduğundan çağrı engeller algılar.  Hiçbir kayıt kümeleri silinir.

Aşağıdaki PowerShell komutu belirli bir bölgenin SOA kaydı DoNotDelete Kilitle oluşturur:

```powershell
# Protect against zone delete with DoNotDelete lock on the record set
New-AzureRmResourceLock -LockLevel DoNotDelete -LockName "<lock name>" -ResourceName "<zone name>/@" -ResourceType" Microsoft.Network/DNSZones/SOA" -ResourceGroupName "<resource group name>"
```

Bölgenin yanlışlıkla silinmesini engellemek için başka bir işleç emin olmak için özel bir rolü kullanarak yoludur ve bölgelerinizi yönetmek için kullanılan hizmet hesaplarını bölge izinlerini silme izniniz yok. Bir bölge silmeniz gerekirse, bir iki adımlı silme, ilk ekibi tarafından verilmesinin bölge silme izinlerine (yanlış bölgeyi silmeden önlemek için kapsamında bölge,) ve ikinci bölgesini silmek için zorunlu kılabilir.

Bu ikinci yaklaşım kilitleri oluşturmayı unutmayın gerek kalmadan hesaplar tarafından erişilen tüm bölgelerde çalıştığını avantajına sahiptir. Bunun gibi abonelik sahibi, bölge silme izinlerine sahip herhangi bir hesabı kritik bir bölgenin yanlışlıkla yine de silebilirsiniz dezavantajı vardır.

Aynı zamanda, bir DNS bölgesi koruma savunma yaklaşım her iki yaklaşım - kaynak kilitleri ve özel roller - kullanmak mümkündür.

## <a name="next-steps"></a>Sonraki adımlar

* RBAC ile çalışma hakkında daha fazla bilgi için bkz. [Azure portalında erişim yönetimini kullanmaya başlama](../role-based-access-control/overview.md).
* Kaynak kilitleri ile çalışma hakkında daha fazla bilgi için bkz. [Azure Resource Manager ile kaynakları kilitleme](../azure-resource-manager/resource-group-lock-resources.md).

