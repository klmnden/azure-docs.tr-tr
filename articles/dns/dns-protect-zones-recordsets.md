---
title: Azure DNS bölgelerini ve kayıtlarını koruma
description: Microsoft Azure DNS'te DNS bölgelerini ve kayıt nasıl Koruyabileceğiniz ayarlar.
services: dns
author: WenJason
ms.service: dns
ms.topic: article
origin.date: 12/4/2018
ms.date: 03/04/2019
ms.author: v-jay
ms.openlocfilehash: 9340a43eb88b4be03c0f0ccc0d07a32f22a9001c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66121429"
---
# <a name="how-to-protect-dns-zones-and-records"></a>DNS bölgelerini ve kayıtlarını koruma

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

DNS bölgelerini ve kayıtlarını kritik kaynaklardır. Bir DNS bölgesi veya hatta yalnızca tek bir DNS kaydı silindiğinde, toplam kesintisi neden olabilir.  Bu nedenle kritik DNS bölgeleri ve kayıtları, yetkilendirilmemiş veya yanlışlıkla yapılan değişikliklere karşı korumalı önemlidir.

Bu makalede, nasıl Azure DNS, DNS bölgeleri ve kayıtları bu değişikliklere karşı korumanıza olanak tanır açıklanmaktadır.  Biz Azure Resource Manager tarafından sağlanan iki güçlü güvenlik özellikleri geçerlidir: [rol tabanlı erişim denetimi](../role-based-access-control/overview.md) ve [kaynak kilitleri](../azure-resource-manager/resource-group-lock-resources.md).

## <a name="role-based-access-control"></a>Rol tabanlı erişim denetimi

Azure rol tabanlı erişim denetimi (RBAC) ayrıntılı erişim yönetimi için Azure kullanıcıları, grupları ve kaynaklar sağlar. RBAC kullanarak, kullanıcıların işlerini yapması için gereken tam erişim miktarını verebilirsiniz. RBAC erişimi yönetmenize nasıl yardımcı olduğunu hakkında daha fazla bilgi için bkz. [rol tabanlı erişim denetimi nedir](../role-based-access-control/overview.md).

### <a name="the-dns-zone-contributor-role"></a>DNS bölgesi katkıda bulunanı rolü

DNS bölgesi katkıda bulunanı rolü, DNS kaynaklarını yönetmek için Azure tarafından sağlanan yerleşik bir roldür.  DNS bölgesi katkıda bulunan izinleri bir kullanıcı veya grup için atama DNS kaynakları, ancak herhangi bir tür değil kaynakları yönetmek bu grubu sağlar.

Örneğin, kaynak grubunu varsayalım *myzones* beş bölgeleri için Contoso Corporation içerir. DNS Yöneticisi, bu kaynak grubu için DNS bölgesi katkıda bulunan izinleri verme, bu DNS bölgelerini üzerinde tam denetim sağlar. Ayrıca, örneğin DNS Yöneticisi oluşturulamıyor veya sanal makineleri durdurmanın gereksiz izinler verme önler.

RBAC izinlerinin atamak için en basit yolu [Azure portalından](../role-based-access-control/role-assignments-portal.md).  Açık **erişim denetimi (IAM)** için kaynak grubu, ardından **Ekle**, ardından **DNS bölgesi katkıda bulunanı** rol ve vermek için gerekli kullanıcıları veya grupları seçin izinler.

![Azure portal aracılığıyla kaynak grubu düzeyinde RBAC](./media/dns-protect-zones-recordsets/rbac1.png)

İzinleri de olabilir [Azure PowerShell kullanarak verilen](../role-based-access-control/role-assignments-powershell.md):

```azurepowershell
# Grant 'DNS Zone Contributor' permissions to all zones in a resource group
New-AzRoleAssignment -SignInName "<user email address>" -RoleDefinitionName "DNS Zone Contributor" -ResourceGroupName "<resource group name>"
```

Eşdeğer komut ayrıca olduğundan [Azure CLI aracılığıyla](../role-based-access-control/role-assignments-cli.md):

```azurecli
# Grant 'DNS Zone Contributor' permissions to all zones in a resource group
azure role assignment create --signInName "<user email address>" --roleName "DNS Zone Contributor" --resourceGroup "<resource group name>"
```

### <a name="zone-level-rbac"></a>Bölge düzeyi RBAC

Azure RBAC kuralları, bir aboneliğe, kaynak grubu veya ayrı bir kaynak için uygulanabilir. Azure DNS söz konusu olduğunda, ilgili kaynağı tek bir DNS bölgesi veya hatta ayrı bir kayıt kümesi olabilir.

Örneğin, kaynak grubunu varsayalım *myzones* bölge içeren *contoso.com* ve bir subzone *customers.contoso.com* hangi CNAME kayıtları her biri için oluşturulur Müşteri hesabı.  Bu CNAME kayıtları yönetmek için kullanılan hesabın kayıt oluşturma izni atanmalıdır *customers.contoso.com* yalnızca bölge, diğer bölgelere erişimi olmamalıdır.

Azure portalı üzerinden bir bölge düzeyinde RBAC izinlerinin verilmesi.  Açık **erişim denetimi (IAM)** bölge seçip **Ekle**, ardından **DNS bölgesi katkıda bulunanı** rol ve izinleri vermek için gerekli kullanıcıları veya grupları seçin.

![DNS bölgesi düzeyi RBAC Azure portal aracılığıyla](./media/dns-protect-zones-recordsets/rbac2.png)

İzinleri de olabilir [Azure PowerShell kullanarak verilen](../role-based-access-control/role-assignments-powershell.md):

```azurepowershell
# Grant 'DNS Zone Contributor' permissions to a specific zone
New-AzRoleAssignment -SignInName "<user email address>" -RoleDefinitionName "DNS Zone Contributor" -ResourceGroupName "<resource group name>" -ResourceName "<zone name>" -ResourceType Microsoft.Network/DNSZones
```

Eşdeğer komut ayrıca olduğundan [Azure CLI aracılığıyla](../role-based-access-control/role-assignments-cli.md):

```azurecli
# Grant 'DNS Zone Contributor' permissions to a specific zone
azure role assignment create --signInName <user email address> --roleName "DNS Zone Contributor" --resource-name <zone name> --resource-type Microsoft.Network/DNSZones --resource-group <resource group name>
```

### <a name="record-set-level-rbac"></a>Kayıt düzeyi RBAC

Bir adım daha fazla ayrıntıya. MX ve TXT kayıtlarını contoso.com bölgenin tepesindeki erişmesi gereken Contoso Corporation, posta Yöneticisi göz önünde bulundurun.  Diğer MX veya TXT kaydı veya diğer herhangi bir tür herhangi bir kayıt erişimi geliştirmiyor.  Azure DNS kayıt kümesi düzeyinde izinleri tam olarak posta yönetici erişmesi kayıtları atamanıza olanak sağlar.  Posta yönetici ihtiyacı ve herhangi bir değişiklik yapmak silemiyor tam denetim izni verilir.

Azure portalı üzerinden kayıt kümesi düzeyi RBAC izinlerinin yapılandırılabilir kullanarak **kullanıcılar** kayıt kümesi sayfasında düğmesi:

![RBAC Azure portal aracılığıyla düzeyi kayıt kümesi](./media/dns-protect-zones-recordsets/rbac3.png)

Kayıt kümesi düzeyi RBAC izinlerinin de olabilir [Azure PowerShell kullanarak verilen](../role-based-access-control/role-assignments-powershell.md):

```azurepowershell
# Grant permissions to a specific record set
New-AzRoleAssignment -SignInName "<user email address>" -RoleDefinitionName "DNS Zone Contributor" -Scope "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/dnszones/<zone name>/<record type>/<record name>"
```

Eşdeğer komut ayrıca olduğundan [Azure CLI aracılığıyla](../role-based-access-control/role-assignments-cli.md):

```azurecli
# Grant permissions to a specific record set
azure role assignment create --signInName "<user email address>" --roleName "DNS Zone Contributor" --scope "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/dnszones/<zone name>/<record type>/<record name>"
```

### <a name="custom-roles"></a>Özel roller

Yerleşik DNS bölgesi katkıda bulunanı rolü, DNS kaynak üzerinde tam denetim sağlar. Kendi müşteri bile daha ayrıntılı bir denetim sağlamak için Azure rolü oluşturmak mümkündür.

Tekrar bir CNAME bölgesinde kayıt örneği göz önünde bulundurun *customers.contoso.com* her Contoso Corporation müşteri hesabı için oluşturulur.  Bu CNAME yönetmek için kullanılan hesabın, CNAME kayıtları yönetme izni verilmelidir.  Ardından, kayıtları (MX kaydı değiştirme gibi) diğer tür değiştirme veya bölge düzeyinde bölge silme gibi işlemleri silemiyor.

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
        "Microsoft.Resources/subscriptions/resourceGroups/read"
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
> Yine de bunları güncelleştirilecek sağlayan etkili bir denetim değilken kayıt kümelerini silmek engellemek için özel bir RBAC rolü kullanarak. Kayıt kümeleri silinmesini engeller ancak değiştirilmesini engel olmaz.  İzin verilen değişiklikleri ekleme ve boş bir kayıt kümesi bırakmak için tüm kayıtları kaldırarak dahil olmak üzere kayıt kümesinden kayıt kaldırma içerir. Bu kaydı bir DNS çözümlemesi bakış açısından kümesi silme aynı etkiye sahiptir.

Özel rol tanımları, şu anda Azure portalı üzerinden tanımlanamaz. Bu rol tanımını temel alan özel bir rol, Azure PowerShell kullanarak oluşturulabilir:

```azurepowershell
# Create new role definition based on input file
New-AzRoleDefinition -InputFile <file path>
```

Ayrıca Azure CLI oluşturulabilir:

```azurecli
# Create new role definition based on input file
azure role create -inputfile <file path>
```

Rol sonra yerleşik rolleri aynı şekilde bu makalenin önceki bölümlerinde açıklandığı şekilde atanabilir.

Oluşturma, yönetme ve özel roller atama hakkında daha fazla bilgi için bkz. [Azure rbac'de özel roller](../role-based-access-control/custom-roles.md).

## <a name="resource-locks"></a>Kaynak kilitleri

RBAC ek olarak, Azure Resource Manager, başka bir tür güvenlik denetimi, yani kaynakları kilitleme özelliği destekler. Burada denetimine izin RBAC kuralları belirli kullanıcılar ve gruplar eylemleri, kaynak kilitleri kaynağa uygulanır ve tüm kullanıcılar ve roller etkili olur. Daha fazla bilgi için bkz. [Azure Resource Manager ile kaynakları kilitleme](../azure-resource-manager/resource-group-lock-resources.md).

Kaynak kilidi iki tür vardır: **CanNotDelete** ve **salt okunur**. Bu işlem, bir DNS bölgesine veya bireysel bir kayıt kümesi için de uygulanabilir.  Aşağıdaki bölümlerde bazı yaygın senaryolar ve bunları desteklemek nasıl açıklanmaktadır kaynak kilitleri kullanma.

### <a name="protecting-against-all-changes"></a>Tüm değişikliklere karşı koruma

Yapılan her değişikliği engellemek için bir salt okunur kilidi bölge için geçerli olur.  Bu, yeni bir kayıt kümesi silindi veya değiştirilmesini öğesinden oluşturulan ve var olan kayıt kümelerini olmasını önler.

Bölge düzeyi kaynak kilitleri, Azure portalı üzerinden oluşturulabilir.  DNS bölgesi sayfasından seçin **kilitler**, ardından **+ Ekle**:

![Azure portal aracılığıyla bölge düzeyinde kaynak kilitleri](./media/dns-protect-zones-recordsets/locks1.png)

Bölge düzeyi kaynak kilitleri, Azure PowerShell oluşturulabilir:

```azurepowershell
# Lock a DNS zone
New-AzResourceLock -LockLevel <lock level> -LockName <lock name> -ResourceName <zone name> -ResourceType Microsoft.Network/DNSZones -ResourceGroupName <resource group name>
```

Azure kaynak kilitlerinin yapılandırılması, Azure CLI şu anda desteklenmiyor.

### <a name="protecting-individual-records"></a>Kayıtları tek tek koruma

Mevcut bir DNS kayıt karşı değişiklik kümesi önlemek için kayıt kümesi için bir salt okunur kilidi uygulayın.

> [!NOTE]
> Bir kayıt kümesine CanNotDelete kilit uygulama etkili bir denetimi değil. Kayıt kümesi silinmesini engeller, ancak bu, değiştirilmesini önlemez.  İzin verilen değişiklikleri ekleme ve boş bir kayıt kümesi bırakmak için tüm kayıtları kaldırarak dahil olmak üzere kayıt kümesinden kayıt kaldırma içerir. Bu kaydı bir DNS çözümlemesi bakış açısından kümesi silme aynı etkiye sahiptir.

Kayıt kümesi düzeyinde kaynak kilitleri için şu anda yalnızca Azure PowerShell kullanılarak yapılandırılır.  Bunlar, Azure portalı veya Azure CLI içinde desteklenmez.

```azurepowershell
# Lock a DNS record set
New-AzResourceLock -LockLevel <lock level> -LockName "<lock name>" -ResourceName "<zone name>/<record set name>" -ResourceType "Microsoft.Network/DNSZones/<record type>" -ResourceGroupName "<resource group name>"
```

### <a name="protecting-against-zone-deletion"></a>Bölge silinmeye karşı koruma

Azure DNS'de bölge silindiğinde, bu bölgedeki tüm kayıt kümelerini da silinir.  Bu işlem geri alınamaz.  Kritik bir bölgenin yanlışlıkla silme önemli iş etkisine sahip olma olasılığı vardır.  Bu nedenle bölgenin yanlışlıkla silinmesine karşı korumak çok önemlidir.

Bir bölgeye CanNotDelete kilit uygulama bölge silinmesini engeller.  Ancak kilit alt kaynaklar tarafından devralınır olduğundan, ayrıca tüm kayıt kümelerini olabilecek istenmeyen siliniyor, ilk alan engeller.  Kayıtları hala var olan kayıt kümesinden kaldırılabilir beri Ayrıca, yukarıdaki not açıklandığı gibi ayrıca etkisiz olur.

Alternatif olarak, bir kayıt kümesi SOA kaydı kümesi gibi bölgesinde CanNotDelete kilidi uygulamak göz önünde bulundurun.  Kayıt kümelerini silmeden bölgesi silinemiyor olduğundan, bu sağlarken hala kayıt kümeleri serbestçe değiştirilecek bölge içinde bölge silme karşı korur. Bir bölgeyi silme denenirse, Azure Resource Manager bu SOA kayıt kümesi da siler ve SOA kilitli olduğundan çağrı engeller algılar.  Hiçbir kayıt kümeleri silinir.

Aşağıdaki PowerShell komutu belirli bir bölgenin SOA kaydı CanNotDelete Kilitle oluşturur:

```azurepowershell
# Protect against zone delete with CanNotDelete lock on the record set
New-AzResourceLock -LockLevel CanNotDelete -LockName "<lock name>" -ResourceName "<zone name>/@" -ResourceType" Microsoft.Network/DNSZones/SOA" -ResourceGroupName "<resource group name>"
```

Bölgenin yanlışlıkla silinmesini engellemek için başka bir işleç emin olmak için özel bir rolü kullanarak yoludur ve bölgelerinizi yönetmek için kullanılan hizmet hesaplarını bölge izinlerini silme izniniz yok. Bir bölge silmeniz gerekirse, bir iki adımlı silme, ilk ekibi tarafından verilmesinin bölge silme izinlerine (yanlış bölgeyi silmeden önlemek için kapsamında bölge,) ve ikinci bölgesini silmek için zorunlu kılabilir.

Bu ikinci yaklaşım kilitleri oluşturmayı unutmayın gerek kalmadan hesaplar tarafından erişilen tüm bölgelerde çalıştığını avantajına sahiptir. Bunun gibi abonelik sahibi, bölge silme izinlerine sahip herhangi bir hesabı kritik bir bölgenin yanlışlıkla yine de silebilirsiniz dezavantajı vardır.

Aynı zamanda, bir DNS bölgesi koruma savunma yaklaşım her iki yaklaşım - kaynak kilitleri ve özel roller - kullanmak mümkündür.

## <a name="next-steps"></a>Sonraki adımlar

* RBAC ile çalışma hakkında daha fazla bilgi için bkz. [Azure portalında erişim yönetimini kullanmaya başlama](../role-based-access-control/overview.md).
* Kaynak kilitleri ile çalışma hakkında daha fazla bilgi için bkz. [Azure Resource Manager ile kaynakları kilitleme](../azure-resource-manager/resource-group-lock-resources.md).
