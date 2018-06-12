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
ms.date: 05/18/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: ''
ms.openlocfilehash: 9bb7808f2b483fe9cd7d22c6df3fe80d4a98f1f4
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35266865"
---
# <a name="understand-role-definitions"></a>Rol tanımlarını anlama

Bir rolü nasıl çalıştığını anlamak çalışıyorsanız veya kendi oluşturuyorsanız, [özel rol](custom-roles.md), rolleri nasıl tanımlanan anlamaya yardımcı olur. Bu makalede rol tanımları ayrıntılarını açıklar ve bazı örnekler sağlar.

## <a name="role-definition-structure"></a>Rol tanımı yapısı

A *rol tanımı* izinleri koleksiyonudur. Bazı durumlarda yalnızca adlandırılan bir *rol*. Rol tanımı gibi okuma gerçekleştirilebilir işlemleri listeler, yazma ve silme. Ayrıca, temel alınan veri ile ilgili işlemler ve gerçekleştirilemiyor operations listeleyebilirsiniz. Rol tanımı aşağıdaki yapıya sahiptir:

```
assignableScopes []
description
id
name
permissions []
  actions []
  dataActions []
  notActions []
  notDataActions []
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
        "dataActions": [],
        "notActions": [
          "Microsoft.Authorization/*/Delete",
          "Microsoft.Authorization/*/Write",
          "Microsoft.Authorization/elevateAccess/Action"
        ],
        "notDataActions": []
      }
    ],
    "roleName": "Contributor",
    "roleType": "BuiltInRole",
    "type": "Microsoft.Authorization/roleDefinitions"
  }
]
```

## <a name="management-and-data-operations-preview"></a>Yönetim ve veri işlemleri (Önizleme)

Rol tabanlı erişim denetimi yönetimi işlemleri için belirtilen `actions` ve `notActions` bölümler bir rol tanımı. Azure yönetim işlemlerini bazı örnekleri şunlardır:

- Bir depolama hesabına erişimi yönetme
- Oluşturmak, güncelleştirmek ya da bir blob kapsayıcısından silin
- Bir kaynak grubu ve tüm kaynaklarını silmek

Yönetim erişimi verilerinize alınmadı. Bu ayrım joker karakterlerle rolleri engeller (`*`) verilerinizi sınırsız erişimi gelen. Örneğin, bir kullanıcının bir [okuyucu](built-in-roles.md#reader) rol bir abonelikte sonra bunlar depolama hesabı görüntüleyebilirsiniz, ancak varsayılan olarak, temel alınan veri görüntüleyemezsiniz.

Daha önce rol tabanlı erişim denetimi için veri işlemleri kullanılmadı. Veri işlemleri için yetkilendirme kaynak sağlayıcılar arasında farklılık. Yönetim işlemleri için kullanılan aynı rol tabanlı erişim denetimi yetkilendirme modeli veri işlemleri (şu anda önizlemede) şekilde genişletilmiştir.

Veri işlemleri desteklemek için yeni veri bölümleri rol tanımı yapısına eklenmiştir. Veri işlemleri belirtilir `dataActions` ve `notDataActions` bölümler. Bu veri bölümleri ekleyerek, yönetim ve veriler arasında ayrım korunur. Bu joker karakterlerle geçerli rol atamalarını önler (`*`) gelen aniden için verilere sahip. İçinde belirtilen bazı veri işlemleri işte `dataActions` ve `notDataActions`:

- BLOB'ları bir kapsayıcıda listesini okur
- Bir depolama blob bir kapsayıcıda yazma
- Bir kuyruktaki ileti silme

Burada [depolama Blob veri Okuyucu (Önizleme)](built-in-roles.md#storage-blob-data-reader-preview) işlemleri hem de içeren rol tanımı `actions` ve `dataActions` bölümler. Bu rol, blob kapsayıcısında ve ayrıca temel blob verileri okumak sağlar.

```json
[
  {
    "additionalProperties": {},
    "assignableScopes": [
      "/"
    ],
    "description": "Allows for read access to Azure Storage blob containers and data.",
    "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/2a2b9908-6ea1-4ae2-8e65-a410df84e7d1",
    "name": "2a2b9908-6ea1-4ae2-8e65-a410df84e7d1",
    "permissions": [
      {
        "actions": [
          "Microsoft.Storage/storageAccounts/blobServices/containers/read"
        ],
        "additionalProperties": {},
        "dataActions": [
          "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read"
        ],
        "notActions": [],
        "notDataActions": []
      }
    ],
    "roleName": "Storage Blob Data Reader (Preview)",
    "roleType": "BuiltInRole",
    "type": "Microsoft.Authorization/roleDefinitions"
  }
]
```

Yalnızca veri işlemleri eklenebilir `dataActions` ve `notDataActions` bölümler. Kaynak sağlayıcıları tanımlamak hangi veri işlemleri ayarlayarak işlemleridir `isDataAction` özelliğine `true`. İşlemlerin bir listesini görmek için burada `isDataAction` olan `true`, bkz: [kaynak sağlayıcısı işlemleri](resource-provider-operations.md). Veri işlemleri olmayan rollerini sağlamak için gerekli değildir `dataActions` ve `notDataActions` bölümler rol tanımı içinde.

Yetkilendirme tüm yönetim işlemi API çağrıları için Azure Resource Manager tarafından işlenir. Yetkilendirme verileri işlemi API çağrıları için bir kaynak sağlayıcısı veya Azure Resource Manager tarafından işlenir.

### <a name="data-operations-example"></a>Veri işlemleri örneği

Yönetim ve veri işlemlerini nasıl çalıştığını daha iyi anlamak için belirli bir örneği şimdi göz önünde bulundurun. Alice atanan [sahibi](built-in-roles.md#owner) abonelik kapsamdaki rol. Bob atanan [depolama Blob veri katkıda bulunan (Önizleme)](built-in-roles.md#storage-blob-data-contributor-preview) bir depolama hesabı kapsamda rol. Aşağıdaki diyagramda bu örnek gösterilmektedir.

![Rol tabanlı erişim denetimi yönetim ve veri işlemlerini desteklemek için genişletilmiş](./media/role-definitions/rbac-management-data.png)

[Sahibi](built-in-roles.md#owner) Alice için rol ve [depolama Blob veri katkıda bulunan (Önizleme)](built-in-roles.md#storage-blob-data-contributor-preview) rolde Bob için aşağıdaki eylemleri:

Sahip

&nbsp;&nbsp;&nbsp;&nbsp;Eylemler<br>
&nbsp;&nbsp;&nbsp;&nbsp;`*`

Depolama Blob Verileri Katkıda Bulunan (Önizleme)

&nbsp;&nbsp;&nbsp;&nbsp;Eylemler<br>
&nbsp;&nbsp;&nbsp;&nbsp;`Microsoft.Storage/storageAccounts/blobServices/containers/delete`<br>
&nbsp;&nbsp;&nbsp;&nbsp;`Microsoft.Storage/storageAccounts/blobServices/containers/read`<br>
&nbsp;&nbsp;&nbsp;&nbsp;`Microsoft.Storage/storageAccounts/blobServices/containers/write`<br>
&nbsp;&nbsp;&nbsp;&nbsp;DataActions<br>
&nbsp;&nbsp;&nbsp;&nbsp;`Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete`<br>
&nbsp;&nbsp;&nbsp;&nbsp;`Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read`<br>
&nbsp;&nbsp;&nbsp;&nbsp;`Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write`

Alice bir joker karakter olduğundan (`*`) eylem bir abonelik kapsamda kendi devralmak aşağı her tüm yönetim eylemlerini gerçekleştirmek etkinleştirmek için. Ancak, Alice veri işlemlerini gerçekleştiremezsiniz. Örneğin, varsayılan olarak, Alice bir kapsayıcısı içinde BLOB'ları okunamıyor, ancak kendisinin okuma, yazma ve kapsayıcıları silin.

Bob'un izinler için yalnızca sınırlı `actions` ve `dataActions` belirtilen [depolama Blob veri katkıda bulunan (Önizleme)](built-in-roles.md#storage-blob-data-contributor-preview) rol. Bob rolüne dayalı, yönetim ve veri işlemleri gerçekleştirebilir. Örneğin, Bob okuma, yazma ve belirtilen depolama hesabında kapsayıcıları silin ve daha da okuma, yazma ve BLOB'ları silme.

### <a name="what-tools-support-using-rbac-for-data-operations"></a>Hangi Araçlar veri işlemleri için RBAC kullanarak destekliyor?

Görüntülemek ve veri işlemleri ile çalışmak için Araçlar ya da SDK'ları doğru sürümlerine sahip olmalıdır:

| Aracı  | Sürüm  |
|---------|---------|
| [Azure PowerShell](/powershell/azure/install-azurerm-ps) | 5.6.0 veya üzeri |
| [Azure CLI](/cli/azure/install-azure-cli) | 2.0.30 veya daha yenisi |
| [.NET için Azure](/dotnet/azure/) | 2.8.0-Preview veya daha yenisi |
| [Git için Azure SDK'sı](/go/azure/azure-sdk-go-install) | 15.0.0 veya üzeri |
| [Java için Azure](/java/azure/) | 1.9.0 veya üzeri |
| [Python için Azure](/python/azure) | 0.40.0 veya üzeri |
| [Ruby için Azure SDK](https://rubygems.org/gems/azure_sdk) | 0.17.1 veya üzeri |

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

## <a name="dataactions-preview"></a>dataActions (Önizleme)

`dataActions` İznini rol için bu nesne içinde verilerinize erişim verir veri işlemleri belirtir. Örneğin, bir kullanıcı okuma blob veri erişimi bir depolama hesabı, bunlar bu depolama hesabındaki BLOB bilgi edinebilirsiniz. İşte bazı örnekler, kullanılabilir veri işlemlerinin `dataActions`.

| İşlem dize    | Açıklama         |
| ------------------- | ------------------- |
| `Microsoft.Storage/storageAccounts/ blobServices/containers/blobs/read` | Bir blob veya BLOB listesini döndürür. |
| `Microsoft.Storage/storageAccounts/ blobServices/containers/blobs/write` | Bir blob yazma sonucunu döndürür. |
| `Microsoft.Storage/storageAccounts/ queueServices/queues/messages/read` | Bir ileti döndürür. |
| `Microsoft.Storage/storageAccounts/ queueServices/queues/messages/*` | Bir ileti veya yazma veya silme bir ileti sonucunu döndürür. |

## <a name="notdataactions-preview"></a>notDataActions (Önizleme)

`notDataActions` İznini hariç tutulan veri işlemleri belirtir izin verilen gelen `dataActions`. Bir rol (yürürlükte olan izinlerini) tarafından verilen erişim çıkarılmasıyla hesaplanır `notDataActions` işlemlerinden `dataActions` işlemleri. Her kaynak sağlayıcısı, ilgili veri işlemleri gerçekleştirmek için API'ler sağlar.

> [!NOTE]
> Bir kullanıcı bir veri işleminde dışlar bir rolü atanıp atanmadığını `notDataActions`ve aynı veri işlem erişim, veri işlemi gerçekleştirmek için kullanıcının izin veren ikinci bir rol atanır. `notDataActions` bir reddetme değil kural –, çıkarılacak belirli veri işlemleriyle gerektiğinde izin verilen veri işlemleri kümesi oluşturmak için yalnızca uygun bir yoldur.
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
        "dataActions": [],
        "notActions": [],
        "notDataActions": []
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
  "DataActions":  [

                  ],
  "NotDataActions":  [

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
