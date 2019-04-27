---
title: Statik grup üyelik türü değiştirme dinamik - Azure Active Directory | Microsoft Docs
description: Gruplar ve bir kural başvuru otomatik olarak doldurmak için üyelik kuralları oluşturmak nasıl.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 03/18/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: dd753ca4994975302a0bc6fede61964f80196d7c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60472080"
---
# <a name="change-static-group-membership-to-dynamic-in-azure-active-directory"></a>Azure Active Directory'de dinamik statik grup üyeliğini değiştirme

Bir grubun üyeliğini statik olan dinamik (veya tersi) değiştirebileceğiniz, Azure Active Directory (Azure AD). Grup için tüm mevcut başvuruların hala geçerli olduğundan bu nedenle azure AD sistemdeki aynı grubu adını ve Kimliğini tutar. Bunun yerine yeni bir grubu oluşturursanız, bu başvuruları güncelleştirin gerekecektir. Dinamik grup üyeliği, yönetim ek yükü ekleme ve kullanıcıların kaldırılmasını ortadan kaldırır. Bu makale, mevcut gruplar için dinamik üyelik Azure AD yönetim merkezini ya da PowerShell cmdlet'lerini kullanarak statik dönüştürme bildirir.

> [!WARNING]
> Varolan bir statik grup için dinamik bir grup değiştirilirken, tüm mevcut üyelerin grubundan kaldırılır ve yeni üyeler eklemek için üyelik kuralını sonra işlenir. Grubu, uygulamaları veya kaynaklarına erişimi denetlemek için kullanılıyorsa, üyelik kuralı tam olarak işlendiği kadar özgün üye erişimi kaybedebilir dikkat edin.
>
> Yeni grup üyeliği beklenildiği gibi gittiğinden emin olmak için önceden yeni üyelik kuralını test etmenizi öneririz.

## <a name="change-the-membership-type-for-a-group"></a>Bir grubu için üyelik türünü değiştirme

1. Oturum [Azure AD yönetim merkezini](https://aad.portal.azure.com) genel yönetici veya kiracınızdaki Kullanıcı Yöneticisi olan bir hesapla.
2. Seçin **grupları**.
3. Gelen **tüm grupları** listesinde, değiştirmek istediğiniz grubu açın.
4. Seçin **özellikleri**.
5. Üzerinde **özellikleri** grubu seç sayfasını bir **üyelik türü** atanan (statik), dinamik kullanıcı ya da, istediği üyelik türüne bağlı olarak dinamik cihaz. Dinamik üyelik için basit bir kuralı seçeneklerini belirleyin veya kendiniz bir üyelik kuralı yazma kuralı Oluşturucusu'nu kullanabilirsiniz. 

Bir kullanıcı grubu için dinamik üyeliği statik olan bir grubu değiştirme örneği adımlardır.

1. Üzerinde **özellikleri** seçili grubunuzun seçin sayfasında bir **üyelik türü** , **dinamik kullanıcı**, gruba değişiklikleri açıklayan iletişim Evet'i seçin devam etmek için üyelik. 
  
   ![dinamik kullanıcı üyelik türünü seçin](./media/groups-change-type/select-group-to-convert.png)
  
2. Seçin **dinamik sorgu Ekle**ve sonra kural sağlayın.
  
   ![dinamik grup için kural girin](./media/groups-change-type/enter-rule.png)
  
3. Bir kural oluşturduktan sonra seçin **Sorgu Ekle** sayfanın alt kısmındaki.
4. Seçin **Kaydet** üzerinde **özellikleri** yaptığınız değişiklikleri kaydetmek grup için sayfa. **Üyelik türü** grubunu Grup listesinde hemen güncelleştirilir.

> [!TIP]
> Girdiğiniz üyelik kuralı yanlışsa grubu dönüştürme başarısız olabilir. Kural sistem tarafından neden kabul edilemez bir açıklama içeren portalının sağ üst köşede bir bildirim görüntülenir. Bunu nasıl geçerli hale getirmek için bir kural ayarlayabilirsiniz dikkatli bir şekilde anlamak için okuyun. Kuralın söz dizimi örneklerini ve desteklenen özellikleri, işleçler ve değerleri bir üyelik kuralı için tam bir listesi için bkz: [Azure Active Directory'de gruplar için dinamik Üyelik kuralları](groups-dynamic-membership.md).

## <a name="change-membership-type-for-a-group-powershell"></a>(PowerShell) grubu için üyelik türünü değiştir

> [!NOTE]
> Cmdlet'leri kullanmak için gerekir, dinamik Grup özelliklerini değiştirmek için **önizleme sürümünü** [Azure AD PowerShell sürüm 2](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0). Önizlemeyi yükleyebilirsiniz [PowerShell Galerisi](https://www.powershellgallery.com/packages/AzureADPreview).

Var olan bir grubuna üyelik Yönetimi geçiş işlevleri örneği aşağıda verilmiştir. Bu örnekte, bakım doğru GroupTypes özelliği yönetmek ve korumak için dinamik üyelik ilgisiz herhangi bir değeri getirilir.

```powershell
#The moniker for dynamic groups as used in the GroupTypes property of a group object
$dynamicGroupTypeString = "DynamicMembership"

function ConvertDynamicGroupToStatic
{
    Param([string]$groupId)

    #existing group types
    [System.Collections.ArrayList]$groupTypes = (Get-AzureAdMsGroup -Id $groupId).GroupTypes

    if($groupTypes -eq $null -or !$groupTypes.Contains($dynamicGroupTypeString))
    {
        throw "This group is already a static group. Aborting conversion.";
    }


    #remove the type for dynamic groups, but keep the other type values
    $groupTypes.Remove($dynamicGroupTypeString)

    #modify the group properties to make it a static group: i) change GroupTypes to remove the dynamic type, ii) pause execution of the current rule
    Set-AzureAdMsGroup -Id $groupId -GroupTypes $groupTypes.ToArray() -MembershipRuleProcessingState "Paused"
}

function ConvertStaticGroupToDynamic
{
    Param([string]$groupId, [string]$dynamicMembershipRule)

    #existing group types
    [System.Collections.ArrayList]$groupTypes = (Get-AzureAdMsGroup -Id $groupId).GroupTypes

    if($groupTypes -ne $null -and $groupTypes.Contains($dynamicGroupTypeString))
    {
        throw "This group is already a dynamic group. Aborting conversion.";
    }
    #add the dynamic group type to existing types
    $groupTypes.Add($dynamicGroupTypeString)

    #modify the group properties to make it a static group: i) change GroupTypes to add the dynamic type, ii) start execution of the rule, iii) set the rule
    Set-AzureAdMsGroup -Id $groupId -GroupTypes $groupTypes.ToArray() -MembershipRuleProcessingState "On" -MembershipRule $dynamicMembershipRule
}
```
Statik bir grup yapmak için:

```powershell
ConvertDynamicGroupToStatic "a58913b2-eee4-44f9-beb2-e381c375058f"
```

Dinamik bir grup yapmak için:

```powershell
ConvertStaticGroupToDynamic "a58913b2-eee4-44f9-beb2-e381c375058f" "user.displayName -startsWith ""Peter"""
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makaleler, Azure Active Directory içinde grupları hakkında ek bilgi sağlar.

* [Var olan grupları görme](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Yeni grup oluşturma ve üye ekleme](../fundamentals/active-directory-groups-create-azure-portal.md)
* [Bir grubun ayarlarını yönetme](../fundamentals/active-directory-groups-settings-azure-portal.md)
* [Bir grubun üyeliklerini yönetme](../fundamentals/active-directory-groups-membership-azure-portal.md)
* [Bir gruptaki kullanıcılar için dinamik kuralları yönetme](groups-dynamic-membership.md)
