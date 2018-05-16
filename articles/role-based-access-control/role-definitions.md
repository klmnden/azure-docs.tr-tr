---
title: Azure RBAC rol tanımlarında anlama | Microsoft Docs
description: Rol tabanlı erişim denetimi (RBAC) rol tanımlarında ve Azure kaynaklarının ayrıntılı erişim yönetimi için özel rolleri tanımlama hakkında bilgi edinin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: ''
ms.service: role-based-access-control
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/12/2018
ms.author: rolyon
ms.reviewer: rqureshi
ms.custom: ''
ms.openlocfilehash: 7a9e257d445ff7dadfe27d1c75cde6f58a393397
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="understand-role-definitions"></a>Rol tanımları anlayın

Bir rolü nasıl çalıştığını anlamak çalışıyorsanız veya kendi oluşturuyorsanız, [özel rol](custom-roles.md), rolleri nasıl tanımlanan anlamaya yardımcı olur. Bu makalede rol tanımları ayrıntılarını açıklar ve bazı örnekler sağlar.

## <a name="role-definition-structure"></a>Rol tanımı yapısı

A *rol tanımı* izinleri koleksiyonudur. Bazı durumlarda yalnızca adlandırılan bir *rol*. Rol tanımı gibi okuma gerçekleştirilebilir işlemleri listeler, yazma ve silme. Ayrıca, gerçekleştirilemiyor işlemleri de listeleyebilirsiniz. Rol tanımı aşağıdaki yapıya sahiptir:

```
assignableScopes []
description
id
name
permissions []
  actions []
  notActions []
roleName
roleType
type
```

İşlem aşağıdaki biçime sahip dizelerle belirtilir:

- `Microsoft.{ProviderName}/{ChildResourceType}/{action}`

`{action}` Bir işlemi dizenin bir kaynak türü üzerinde gerçekleştirebileceğiniz işlemler türünü belirtir. Örneğin, aşağıdaki nedenle görürsünüz `{action}`:

| Eylem substring    | Açıklama         |
| ------------------- | ------------------- |
| `*` | Joker karakter dizesi eşleşen tüm işlemlerine erişim verir. |
| `read` | Okuma işlemleri (GET) etkinleştirir. |
| `write` | Yazma işlemleri (PUT, POST ve düzeltme eki) etkinleştirir. |
| `delete` | Silme işlemlerine (Sil) etkinleştirir. |

Burada [katkıda bulunan](built-in-roles.md#contributor) JSON biçiminde rol tanımı. Joker karakter (`*`) altında işlemi `actions` bu role atanan asıl tüm eylemleri gerçekleştirebilir veya diğer bir deyişle, her şeyi yönetebilmek gösterir. Yeni kaynak türlerini Azure ekler olarak bu gelecekte tanımlanan eylemleri içerir. İşlemler altında `notActions` gelen çıkarılır `actions`. Durumunda [katkıda bulunan](built-in-roles.md#contributor) rolü `notActions` kaynaklara erişimini yönetmek ve ayrıca kaynaklara erişim atamak için bu rolün olasılığını ortadan kaldırır.

```json
[
  {
    "additionalProperties": {},
    "assignableScopes": [
      "/"
    ],
    "description": "Lets you manage everything except access to resources.",
    "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c",
    "name": "b24988ac-6180-42a0-ab88-20f7382dd24c",
    "permissions": [
      {
        "actions": [
          "*"
        ],
        "additionalProperties": {},
        "notActions": [
          "Microsoft.Authorization/*/Delete",
          "Microsoft.Authorization/*/Write",
          "Microsoft.Authorization/elevateAccess/Action"
        ],
      }
    ],
    "roleName": "Contributor",
    "roleType": "BuiltInRole",
    "type": "Microsoft.Authorization/roleDefinitions"
  }
]
```

## <a name="management-operations"></a>Yönetim işlemleri

Rol tabanlı erişim denetimi yönetimi işlemleri için belirtilen `actions` ve `notActions` bölümler bir rol tanımı. Azure yönetim işlemlerini bazı örnekleri şunlardır:

- Bir depolama hesabına erişimi yönetme
- Oluşturmak, güncelleştirmek ya da bir blob kapsayıcısından silin
- Bir kaynak grubu ve tüm kaynaklarını silmek

Yönetim erişimi verilerinize alınmadı. Bu ayrım joker karakterlerle rolleri engeller (`*`) verilerinizi sınırsız erişimi gelen. Örneğin, bir kullanıcının bir [okuyucu](built-in-roles.md#reader) rol bir abonelikte sonra bunlar depolama hesabı görüntüleyebilirsiniz, ancak temel alınan veri görüntüleyemez.

## <a name="actions"></a>Eylemler

`actions` İznini rol için erişim verir yönetim işlemlerini belirtir. Azure kaynak sağlayıcıları güvenliği sağlanabilir işlemlerini tanımlayan işlemi dizelerin koleksiyonudur. İşte bazı örnekler, kullanılabilir yönetim işlemlerinin `actions`.

| İşlem dize    | Açıklama         |
| ------------------- | ------------------- |
| `*/read` | verir tüm Azure kaynak sağlayıcıları tüm kaynak türleri için okuma erişimi.|
| `Microsoft.Compute/*` | tüm işlemler Microsoft.Compute kaynak sağlayıcısındaki tüm kaynak türleri için erişim verir.|
| `Microsoft.Network/*/read` | Verir, Microsoft.Network kaynak Sağlayıcısı'nda tüm kaynak türleri için okuma erişimi.|
| `Microsoft.Compute/virtualMachines/*` | verir kaynak türleri için sanal makinelerin ve kendi alt tüm işlemleri erişin.|
| `microsoft.web/sites/restart/Action` | Bir web uygulaması yeniden erişimi verir.|

## <a name="notactions"></a>NotActions

`notActions` İznini hariç tutulan yönetim işlemlerini belirtir izin verilen gelen `actions`. Kullanım `notActions` izin vermek istediğiniz işlem kümesi daha kolay kısıtlı işlemleri hariç tutarak tanımlanmışsa izni. Bir rol (yürürlükte olan izinlerini) tarafından verilen erişim çıkarılmasıyla hesaplanır `notActions` işlemlerinden `actions` işlemleri.

> [!NOTE]
> Bir kullanıcı bir işlemde dışlar bir rolü atanıp atanmadığını `notActions`ve aynı işlem erişim bu işlemi gerçekleştirmek için kullanıcının izin veren ikinci bir rol atanır. `notActions` bir reddetme değil kural – bu belirli işlemleri dışarıda gerektiğinde izin verilen işlem kümesi oluşturmak için yalnızca uygun bir yoldur.
>

## <a name="assignablescopes"></a>AssignableScopes

`assignableScopes` Bölüm rol atama için uygun olduğunu kapsamları (Yönetim grupları (şu anda önizlemede), abonelikler, kaynak grupları veya kaynakları) belirtir. Rol ataması için kullanılabilir abonelikleri yalnızca yapabileceğiniz veya geri kalanı abonelik veya kaynak grupları için ve dağınıklığı kullanıcının gerektiren kaynak grupları karşılaşabilirsiniz. En az bir yönetim kullanmalısınız grup, abonelik, kaynak grubu veya kaynak kimliği

Yerleşik roller sahip `assignableScopes` kök kapsamını Ayarla (`"/"`). Kök kapsam rol atamasını tüm kapsamlar için kullanılabilir olduğunu gösterir. Kendi özel roller kök kapsamı kullanılamaz. Denerseniz, bir Yetkilendirme hatası alırsınız.

Geçerli atanabilir kapsamların örnekleri şunlardır:

| Senaryo | Örnek |
|----------|---------|
| Rol atamasını tek bir abonelik için kullanılabilir | `"/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"` |
| Rol ataması iki abonelikler için kullanılabilir | `"/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e", "/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624"` |
| Rol atamasını yalnızca ağ kaynak grubu için kullanılabilir | `"/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network"` |
| Rol atamasını tüm kapsamlar için kullanılabilir | `"/"` |

## <a name="assignablescopes-and-custom-roles"></a>assignableScopes ve özel roller

`assignableScopes` Özel bir rol kimin oluşturabilir, silebilir, değiştirme veya özel rol görüntülemek denetimleri için bölüm.

| Görev | İşlem | Açıklama |
| --- | --- | --- |
| Özel bir rol oluşturma/silme | `Microsoft.Authorization/ roleDefinition/write` | Bu işlem tüm izni verilen kullanıcıları `assignableScopes` özel rolü oluşturun (silmek bu kapsamları kullanmak için özel roller veya). Örneğin, [sahipleri](built-in-roles.md#owner) ve [kullanıcı erişim yöneticileri](built-in-roles.md#user-access-administrator) Abonelikleri, kaynak grupları ve kaynakları. |
| Özel bir rol değiştirme | `Microsoft.Authorization/ roleDefinition/write` | Bu işlem tüm izni verilen kullanıcıları `assignableScopes` özel rolü bu kapsamları özel rollerinde değiştirebilirsiniz. Örneğin, [sahipleri](built-in-roles.md#owner) ve [kullanıcı erişim yöneticileri](built-in-roles.md#user-access-administrator) Abonelikleri, kaynak grupları ve kaynakları. |
| Özel bir rol görüntüleyin | `Microsoft.Authorization/ roleDefinition/read` | Bu işlem bir kapsamda izni verilen kullanıcıları bu kapsamda atama için uygun olan özel roller görüntüleyebilirsiniz. Tüm yerleşik roller atama için uygun olması özel rollere izin verin. |

## <a name="role-definition-examples"></a>Rol tanımı örnekleri

Aşağıdaki örnekte gösterildiği [okuyucu](built-in-roles.md#reader) Azure CLI kullanarak görüntülendiği gibi rol tanımı:

```json
[
  {
    "additionalProperties": {},
    "assignableScopes": [
      "/"
    ],
    "description": "Lets you view everything, but not make any changes.",
    "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7",
    "name": "acdd72a7-3385-48ef-bd42-f606fba81ae7",
    "permissions": [
      {
        "actions": [
          "*/read"
        ],
        "additionalProperties": {},
        "notActions": [],
      }
    ],
    "roleName": "Reader",
    "roleType": "BuiltInRole",
    "type": "Microsoft.Authorization/roleDefinitions"
  }
]
```

Aşağıdaki örnek, izleme ve Azure PowerShell kullanarak görüntülendiği gibi sanal makine yeniden başlatma için özel bir rol gösterir:

```json
{
  "Name":  "Virtual Machine Operator",
  "Id":  "88888888-8888-8888-8888-888888888888",
  "IsCustom":  true,
  "Description":  "Can monitor and restart virtual machines.",
  "Actions":  [
                  "Microsoft.Storage/*/read",
                  "Microsoft.Network/*/read",
                  "Microsoft.Compute/*/read",
                  "Microsoft.Compute/virtualMachines/start/action",
                  "Microsoft.Compute/virtualMachines/restart/action",
                  "Microsoft.Authorization/*/read",
                  "Microsoft.Resources/subscriptions/resourceGroups/read",
                  "Microsoft.Insights/alertRules/*",
                  "Microsoft.Insights/diagnosticSettings/*",
                  "Microsoft.Support/*"
  ],
  "NotActions":  [

                 ],
  "AssignableScopes":  [
                           "/subscriptions/{subscriptionId1}",
                           "/subscriptions/{subscriptionId2}",
                           "/subscriptions/{subscriptionId3}"
                       ]
}
```

## <a name="see-also"></a>Ayrıca bkz.

* [Yerleşik roller](built-in-roles.md)
* [Özel roller](custom-roles.md)
* [Azure Resource Manager kaynak sağlayıcısı işlemleri](resource-provider-operations.md)
