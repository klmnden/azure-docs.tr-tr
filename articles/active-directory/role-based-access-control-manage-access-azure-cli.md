---
title: "Rol tabanlı erişim denetimi (RBAC) Azure CLI ile yönetme | Microsoft Docs"
description: "Rolleri ve rol eylemlerin listesini içeren ve rolleri için abonelik ve uygulama kapsamları atama rol tabanlı erişim denetimi (RBAC) Azure komut satırı arabirimi ile yönetmeyi öğrenin."
services: active-directory
documentationcenter: 
author: andredm7
manager: mtillman
ms.assetid: 3483ee01-8177-49e7-b337-4d5cb14f5e32
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.author: andredm
ms.reviewer: rqureshi
ms.openlocfilehash: b99264eb69f115db6e334b6aceae6ed897202d56
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
---
# <a name="manage-role-based-access-control-with-the-azure-command-line-interface"></a>Rol tabanlı erişim denetimini Azure komut satırı arabirimi ile yönetme
> [!div class="op_single_selector"]
> * [PowerShell](role-based-access-control-manage-access-powershell.md)
> * [Azure CLI](role-based-access-control-manage-access-azure-cli.md)
> * [REST API](role-based-access-control-manage-access-rest.md)


Abonelik ve ayrıntılı bir düzeyde kaynaklarınıza erişimi yönetmek için rol tabanlı erişim denetimi (RBAC) Azure portalında ve Azure Resource Manager API'sini kullanabilirsiniz. Bu özellik ile bazı roller belirli bir kapsamda atayarak Active Directory Kullanıcıları, grupları veya hizmet asıl adı için erişim izni verebilir.

RBAC yönetmek için Azure komut satırı arabirimi (CLI) kullanmadan önce aşağıdaki önkoşullara sahip olmalıdır:

* Azure CLI Sürüm 0.8.8 veya sonraki bir sürümü. En son sürümünü yükleyin ve Azure aboneliğinizle ilişkilendirmek için bkz: [Azure CLI'yi yükleme ve yapılandırma](../cli-install-nodejs.md).
* Azure Resource Manager'da Azure CLI. Git [Resource Manager ile Azure CLI kullanarak](../xplat-cli-azure-resource-manager.md) daha fazla ayrıntı için.

## <a name="list-roles"></a>Liste rolleri
### <a name="list-all-available-roles"></a>Kullanılabilir tüm roller listesinde
Kullanılabilir tüm roller listelemek için kullanın:

        azure role list

Aşağıdaki örnek listesini gösterir *kullanılabilir tüm roller*.

```
azure role list --json | jq '.[] | {"roleName":.properties.roleName, "description":.properties.description}'
```

![RBAC Azure komut satırı - azure rol listesi - ekran görüntüsü](./media/role-based-access-control-manage-access-azure-cli/1-azure-role-list.png)

### <a name="list-actions-of-a-role"></a>Bir rolün liste eylemleri
Bir rolü Eylemler listelemek için kullanın:

    azure role show "<role name>"

Aşağıdaki örnek, eylemleri gösterir *katkıda bulunan* ve *sanal makine Katılımcısı* rolleri.

```
azure role show "contributor" --json | jq '.[] | {"Actions":.properties.permissions[0].actions,"NotActions":properties.permissions[0].notActions}'

azure role show "virtual machine contributor" --json | jq '.[] | .properties.permissions[0].actions'
```

![RBAC Azure komut satırı - azure rol Göster - ekran görüntüsü](./media/role-based-access-control-manage-access-azure-cli/1-azure-role-show.png)

## <a name="list-access"></a>Liste erişim
### <a name="list-role-assignments-effective-on-a-resource-group"></a>Liste rol atamalarını bir kaynak grubu üzerinde etkin
Bir kaynak grubunda mevcut rol atamalarını listelemek için kullanın:

    azure role assignment list --resource-group <resource group name>

Aşağıdaki örnek, rol atamalarında gösterir *pharma satış projecforcast* grubu.

```
azure role assignment list --resource-group pharma-sales-projecforcast --json | jq '.[] | {"DisplayName":.properties.aADObject.displayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'
```

![RBAC Azure komut satırı - grubuna göre azure rol ataması listesi - ekran görüntüsü](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-assignment-list-1.png)

### <a name="list-role-assignments-for-a-user"></a>Bir kullanıcı için rol atamalarını listesi
Belirli bir kullanıcı için rol atamalarını ve kullanıcı gruplarına atanır atamalarını listelemek için kullanın:

    azure role assignment list --signInName <user email>

Ayrıca komut değiştirerek gruplarından devralınan rol atamalarını görebilirsiniz:

    azure role assignment list --expandPrincipalGroups --signInName <user email>

Aşağıdaki örnekte verilen rol atamalarını  *sameert@aaddemo.com*  kullanıcı. Bu kullanıcıya atanan rollerin ve gruplardan devralınan rolleri içerir.

```
azure role assignment list --signInName sameert@aaddemo.com --json | jq '.[] | {"DisplayName":.properties.aADObject.DisplayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'

azure role assignment list --expandPrincipalGroups --signInName sameert@aaddemo.com --json | jq '.[] | {"DisplayName":.properties.aADObject.DisplayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'
```

![RBAC Azure komut satırı - kullanıcı tarafından azure rol ataması listesi - ekran görüntüsü](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-assignment-list-2.png)

## <a name="grant-access"></a>Erişim verme
Atamak istediğiniz rolü tanımladıktan sonra erişim vermek için kullanın:

    azure role assignment create

### <a name="assign-a-role-to-group-at-the-subscription-scope"></a>Grup aboneliği kapsamında bir rol atayın
Bir grup aboneliği kapsamında bir rol atamak için kullanın:

    azure role assignment create --objectId  <group object id> --roleName <name of role> --subscription <subscription> --scope <subscription/subscription id>

Aşağıdaki örnek atar *okuyucu* rolüne *Christine Koch'ın takım* adresindeki *abonelik* kapsam.

![RBAC Azure komut satırı - azure rol ataması oluşturma grupla - ekran görüntüsü](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-1.png)

### <a name="assign-a-role-to-an-application-at-the-subscription-scope"></a>Uygulamanın abonelik kapsamında bir rol atayın
Bir uygulama abonelik kapsamında bir rol atamak için kullanın:

    azure role assignment create --objectId  <applications object id> --roleName <name of role> --subscription <subscription> --scope <subscription/subscription id>

Aşağıdaki örnek verir *katkıda bulunan* rolüne bir *Azure AD* seçilen aboneliğe ilişkin uygulama.

 ![RBAC Azure komut satırı - azure rol ataması oluşturma uygulama tarafından](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-2.png)

### <a name="assign-a-role-to-a-user-at-the-resource-group-scope"></a>Kaynak grubu kapsamındaki bir kullanıcıya rol atama
Kaynak grubu kapsamındaki bir kullanıcıya rol atamak için kullanın:

    azure role assignment create --signInName  <user email address> --roleName "<name of role>" --resourceGroup <resource group name>

Aşağıdaki örnek verir *sanal makine Katılımcısı* rolüne  *samert@aaddemo.com*  kullanıcı *Pharma satış ProjectForcast* Kaynak Grup kapsamı.

![RBAC Azure komut satırı - azure rol ataması oluşturma kullanıcı - ekran görüntüsü](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-3.png)

### <a name="assign-a-role-to-a-group-at-the-resource-scope"></a>Kaynak kapsamdaki bir gruba rol atama
Rol kaynak kapsamdaki bir gruba atamak için kullanın:

    azure role assignment create --objectId <group id> --role "<name of role>" --resource-name <resource group name> --resource-type <resource group type> --parent <resource group parent> --resource-group <resource group>

Aşağıdaki örnek verir *sanal makine Katılımcısı* rolüne bir *Azure AD* grubunun bir *alt*.

![RBAC Azure komut satırı - azure rol ataması oluşturma grupla - ekran görüntüsü](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-4.png)

## <a name="remove-access"></a>Erişimi kaldırma
Bir rol atamasını kaldırmak için kullanın:

    azure role assignment delete --objectId <object id to from which to remove role> --roleName "<role name>"

Aşağıdaki örnek kaldırır *sanal makine Katılımcısı* gelen rol atamasını  *sammert@aaddemo.com*  kullanıcı *Pharma satış ProjectForcast* kaynak Grup.
Örneğin bu durumda bir grup aboneliği üzerinde rol ataması kaldırır.

![RBAC Azure komut satırı - azure rol ataması delete - ekran görüntüsü](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-assignment-delete.png)

## <a name="create-a-custom-role"></a>Özel bir rol oluşturun
Özel bir rol oluşturmak için kullanın:

    azure role create --inputfile <file path>

Aşağıdaki örnek adlı özel bir rol oluşturur *sanal makine işleci*. Bu özel rolü tüm okuma işlemlerini erişim veren *Microsoft.Compute*, *Microsoft.Storage*, ve *Microsoft.Network* kaynak sağlayıcıları ve verir erişmek için Başlat, yeniden başlatın ve sanal makineleri izleyin. Bu özel rolü iki Aboneliklerde kullanılabilir. Bu örnek bir JSON dosyası bir giriş olarak kullanır.

![JSON - özel rol tanımı - ekran görüntüsü](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-create-1.png)

![RBAC Azure komut satırı - azure rol oluşturma - ekran görüntüsü](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-create-2.png)

## <a name="modify-a-custom-role"></a>Özel bir rol değiştirme
Özel bir rol değiştirmek için önce kullanın `azure role list` rol tanımı almak için komutu. İkinci olarak, rol tanımı dosyasına istediğiniz değişiklikleri yapın. Son olarak, `azure role set` değiştirilmiş rol tanımı kaydetmek için.

    azure role set --inputfile <file path>

Aşağıdaki örnek, *Microsoft.Insights/diagnosticSettings/* işlemi için **Eylemler**ve bir Azure aboneliğine **AssignableScopes** , Sanal makine işletmeni özel rolü.

![JSON - özel rol tanımını - ekran görüntüsü değiştirme](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-set-1.png)

![RBAC Azure komut satırı - azure rol set - ekran görüntüsü](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-set2.png)

## <a name="delete-a-custom-role"></a>Bir özel rolü silmeyi
Bir özel rolü silmek için önce kullanın `azure role list` belirlemek için komut **kimliği** rolünün. Ardından, `azure role delete` belirterek bu rolü silmek için komutu **kimliği**.

Aşağıdaki örnek kaldırır *sanal makine işleci* özel rol.

![RBAC Azure komut satırı - azure rol silme - ekran görüntüsü](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-delete.png)

## <a name="list-custom-roles"></a>Liste özel roller
Bir kapsamda atama için kullanılabilen rolleri listelemek için kullanın `azure role list` komutu.

Aşağıdaki komut, seçili Abonelikteki ataması için kullanılabilen tüm rolleri listeler.

```
azure role list --json | jq '.[] | {"name":.properties.roleName, type:.properties.type}'
```

![RBAC Azure komut satırı - azure rol listesi - ekran görüntüsü](./media/role-based-access-control-manage-access-azure-cli/5-azure-role-list1.png)

Aşağıdaki örnekte, *sanal makine işleci* de özel rol kullanılamaz *Production4* abonelik bu aboneliği değil çünkü **AssignableScopes** rolünün.

```
azure role list --json | jq '.[] | if .properties.type == "CustomRole" then .properties.roleName else empty end'
```

![RBAC Azure komut satırı - özel roller için azure rol listesi - ekran görüntüsü](./media/role-based-access-control-manage-access-azure-cli/5-azure-role-list2.png)

## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]

