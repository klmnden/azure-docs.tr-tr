---
title: Azure RBAC rolü tanımlarında anlama | Microsoft Docs
description: Azure kaynaklarının ayrıntılı erişim yönetimi için rol tabanlı erişim denetimi (RBAC) rol tanımlarında öğrenin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: ''
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/18/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: ''
ms.openlocfilehash: 1d594b91b85a1bad3bbaa69bc27e62a4829a5661
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37438026"
---
# <a name="understand-role-definitions"></a>Rol tanımlarını anlama

Bir rolü nasıl çalıştığını anlamak çalışıyorsanız veya kendi oluşturduğunuz [özel rol](custom-roles.md), rolleri nasıl tanımlandığını anlamak yararlı olabilir. Bu makalede rol tanımları ayrıntılarını açıklar ve bazı örnekler sağlar.

## <a name="role-definition-structure"></a>Rol tanımı yapısı

A *rol tanımı* izinleri oluşan bir koleksiyondur. Bazen yalnızca adında bir *rol*. Rol tanımı gibi okunur gerçekleştirilebilecek işlemleri listeler, yazma ve silme. Ayrıca, temel alınan verilerde ilgili işlemleri ve gerçekleştirilemiyor operations listeleyebilirsiniz. Rol tanımı aşağıdaki yapıya sahiptir:

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

İşlem, aşağıdaki biçime sahiptir ve dizeleri ile belirtilir:

- `{Company}.{ProviderName}/{resourceType}/{action}`

`{action}` Bir işlemi dizenin bir kaynak türü üzerinde gerçekleştirebileceğiniz işlemleri türünü belirtir. Örneğin, aşağıdaki alt dize ortaya göreceğiniz `{action}`:

| Eylem alt dize    | Açıklama         |
| ------------------- | ------------------- |
| `*` | Joker karakter dizesi ile eşleşen tüm işlemleri erişim verir. |
| `read` | Etkinleştirir okuma işlemlerini (GET). |
| `write` | Yazma işlemlerini (PUT, POST ve PATCH) etkinleştirir. |
| `delete` | Etkinleştirir silme işlemlerine (Sil). |

İşte [katkıda bulunan](built-in-roles.md#contributor) rol tanımı JSON biçiminde. Joker karakter (`*`) altında işlem `actions` bu role atanan asıl tüm eylemleri gerçekleştirebilir veya diğer bir deyişle, her şeyi yönetebilmek gösterir. Azure, yeni kaynak türleri ekledikçe bu gelecekte tanımlanan Eylemler içerir. İşlem altında `notActions` gelen çıkartılır `actions`. Durumunda, [katkıda bulunan](built-in-roles.md#contributor) rolü `notActions` kaynaklara erişimini yönetmek ve ayrıca kaynaklara erişim atamak için bu rolün olasılığını ortadan kaldırır.

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

## <a name="management-and-data-operations-preview"></a>Yönetim ve veri işlemlerini (Önizleme)

Rol tabanlı erişim denetimi yönetim işlemleri için belirtilen `actions` ve `notActions` bir rol tanımı özellikleri. Azure'da yönetim işlemleri bazı örnekleri aşağıda verilmiştir:

- Bir depolama hesabına erişimi yönetme
- Oluşturmak, güncelleştirmek veya bir blob kapsayıcısını Sil
- Bir kaynak grubunu ve tüm kaynaklarını silin

Yönetim erişimi verilerinize alınmadı. Bu ayrım joker karakterlerle rolleri engeller (`*`) öğesinden verilerinize sınırsız erişimi. Örneğin, bir kullanıcının bir [okuyucu](built-in-roles.md#reader) rolü bir abonelikte, ardından depolama hesabını görüntüleyebilirler ancak varsayılan olarak, temel alınan verileri görüntüleyemezsiniz.

Daha önce rol tabanlı erişim denetimi, veri işlemlerinde kullanılmadı. Veri işlemleri için yetkilendirme kaynak sağlayıcılar arasında değiştirilen. Yönetim işlemleri için kullanılan aynı rol tabanlı erişim denetimi yetkilendirme modeli, veri işlemlerini (şu anda önizlemede) şekilde genişletilmiştir.

Veri işlemlerini desteklemek için yeni veri özellikleri rol tanımı yapısına eklendi. Veri işlemleri belirtilir `dataActions` ve `notDataActions` özellikleri. Bu veri özellikleri ekleyerek, yönetim ve veriler arasında ayrım korunur. Bu joker karakterlerle geçerli rol atamaları engeller (`*`) gelen aniden için verilere sahip. İşte belirtilebilir bazı veri işlemleri `dataActions` ve `notDataActions`:

- Bir kapsayıcıdaki blobları listesini okuma
- Bir kapsayıcıda bir depolama blobu yazma
- Kuyruktaki bir iletiyi sil

İşte [depolama Blob verileri Okuyucu (Önizleme)](built-in-roles.md#storage-blob-data-reader-preview) işlemlerini hem de içerir. rol tanımı `actions` ve `dataActions` özellikleri. Bu rol blob kapsayıcısını ve ayrıca temel alınan blob verilerini okumanıza izin verir.

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

Yalnızca veri işlemleri eklenebilir `dataActions` ve `notDataActions` özellikleri. Kaynak sağlayıcıları tanımlamak hangi veri işlemleri ayarlayarak işlemlerdir `isDataAction` özelliğini `true`. İşlemlerinin bir listesini görmek için burada `isDataAction` olduğu `true`, bkz: [kaynak sağlayıcısı işlemleri](resource-provider-operations.md). Veri işlemleri olmayan rollerini sağlamak için gerekli değildir `dataActions` ve `notDataActions` özellikleri rol tanımı içinde.

Tüm yönetim işlemi API çağrıları için yetkilendirme, Azure Resource Manager tarafından işlenir. Veri işlem API çağrıları için yetkilendirme bir kaynak sağlayıcısı veya Azure Resource Manager tarafından işlenir.

### <a name="data-operations-example"></a>Veri işlem örneği

Yönetim ve veri işlemlerini nasıl çalıştığını daha iyi anlamak için belirli bir örnek düşünelim. Alice atanmış [sahibi](built-in-roles.md#owner) abonelik kapsamında bir rol. Bob atanmış [depolama Blob verileri katkıda bulunan (Önizleme)](built-in-roles.md#storage-blob-data-contributor-preview) bir depolama hesabı kapsamda rol. Aşağıdaki diyagramda, bu örnek gösterir.

![Rol tabanlı erişim denetimi, hem yönetim hem de veri işlemlerini destekleyecek şekilde genişletildi](./media/role-definitions/rbac-management-data.png)

[Sahibi](built-in-roles.md#owner) Alice için rol ve [depolama Blob verileri katkıda bulunan (Önizleme)](built-in-roles.md#storage-blob-data-contributor-preview) rolde Bob için aşağıdaki eylemleri:

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

Alice, joker karakter olduğundan (`*`) eylem bir abonelik kapsamda kendi devralmak aşağı tüm yönetim eylemleri gerçekleştirmek her etkinleştirmek için. Ancak, Esra'nın veri işlemleri gerçekleştiremezsiniz. Örneğin, varsayılan olarak, bir kapsayıcı içinde BLOB'ları Alice okunamıyor, ancak Filiz okuma, yazma ve kapsayıcıları silmek.

Bob'ın izinler için yalnızca sınırlı `actions` ve `dataActions` belirtilen [depolama Blob verileri katkıda bulunan (Önizleme)](built-in-roles.md#storage-blob-data-contributor-preview) rol. Rol tabanlı, Bob hem yönetim hem de veri işlemleri gerçekleştirebilirsiniz. Örneğin, Bob okumak, yazma ve belirtilen depolama hesabında kapsayıcıları silmek ve he ayrıca okuma, yazma ve blobları silin.

### <a name="what-tools-support-using-rbac-for-data-operations"></a>Veri işlemleri için RBAC kullanarak hangi araçları destekliyor?

Görüntülemek ve veri işlemleriyle çalışmak için doğru SDK'lar ve Araçlar sürümlerinin olması gerekir:

| Aracı  | Sürüm  |
|---------|---------|
| [Azure PowerShell](/powershell/azure/install-azurerm-ps) | 5.6.0 veya sonraki bir sürümü |
| [Azure CLI](/cli/azure/install-azure-cli) | 2.0.30 veya üzeri |
| [.NET için Azure](/dotnet/azure/) | 2.8.0-Preview veya üzeri |
| [Go için Azure SDK](/go/azure/azure-sdk-go-install) | sının 15.0.0 veya üzeri |
| [Java için Azure](/java/azure/) | 1.9.0 veya üzeri |
| [Python için Azure](/python/azure) | 0.40.0 veya üzeri |
| [Ruby için Azure SDK](https://rubygems.org/gems/azure_sdk) | 0.17.1 veya üzeri |

## <a name="actions"></a>Eylemler

`actions` İznini rol gerçekleştirilmesine izin veren yönetim işlemleri belirtir. Azure kaynak sağlayıcılarının güvenli kılınabilir işlemlerini belirleme işlemi dizelerden oluşan bir koleksiyondur. Kullanılabilir yönetim işlemlerini bazı örnekleri aşağıda verilmiştir `actions`.

| İşlem dize    | Açıklama         |
| ------------------- | ------------------- |
| `*/read` | Okuma işlemleri için tüm kaynak türleri, tüm Azure kaynak sağlayıcılarının erişimi verir.|
| `Microsoft.Compute/*` | Tüm işlemler Microsoft.Compute kaynak sağlayıcısındaki tüm kaynak türleri için erişim verir.|
| `Microsoft.Network/*/read` | Okuma işlemleri Microsoft.Network kaynak Sağlayıcısı'nda tüm kaynak türleri için erişimi verir.|
| `Microsoft.Compute/virtualMachines/*` | Veren tüm sanal makineler ve alt işlemlerin için kaynak türleri erişin.|
| `microsoft.web/sites/restart/Action` | Bir web uygulaması yeniden erişimi verir.|

## <a name="notactions"></a>notActions

`notActions` İzni hariç tutulan yönetim işlemlerini belirler izin verilen gelen `actions`. Kullanım `notActions` izin vermek istediğiniz işlem kümesini kısıtlı işlemleri hariç tutarak daha kolay tanımlı değilse izni. Bir rol (etkili izinleri) tarafından verilen erişimi çıkarılmasıyla hesaplanır `notActions` işlemlerinden `actions` operations.

> [!NOTE]
> Bir işlemde dışlayan bir rolü atanmış bir kullanıcı, `notActions`ve kullanıcının bu işlemi gerçekleştirmek için izin verilir aynı işlemi erişim veren ikinci bir rol atanır. `notActions` bir reddetme değil kural – yalnızca belirli işlemleri dışarıda gerektiğinde, izin verilen işlemler kümesi oluşturmak için uygun şekilde öyledir.
>

## <a name="dataactions-preview"></a>dataActions (Önizleme)

`dataActions` İznini rolü, bu nesnenin içinde verilerinizin gerçekleştirilecek sağlar. veri işlemleri belirtir. Örneğin, bir kullanıcının blob veri erişimi bir depolama hesabına Okuma, bunlar bu depolama hesabında BLOB'ları okuma. Kullanılabilir veri işlemleri bazı örnekleri aşağıda verilmiştir `dataActions`.

| İşlem dize    | Açıklama         |
| ------------------- | ------------------- |
| `Microsoft.Storage/storageAccounts/ blobServices/containers/blobs/read` | Bir blobu veya BLOB listesini döndürür. |
| `Microsoft.Storage/storageAccounts/ blobServices/containers/blobs/write` | Bir blobu yazmanın sonucunu döndürür. |
| `Microsoft.Storage/storageAccounts/ queueServices/queues/messages/read` | Bir ileti döndürür. |
| `Microsoft.Storage/storageAccounts/ queueServices/queues/messages/*` | Bir ileti veya yazma veya bir iletiyi silmenin sonucunu döndürür. |

## <a name="notdataactions-preview"></a>notDataActions (Önizleme)

`notDataActions` İzni hariç tutulan veri işlemleri belirtir izin verilen gelen `dataActions`. Bir rol (etkili izinleri) tarafından verilen erişimi çıkarılmasıyla hesaplanır `notDataActions` işlemlerinden `dataActions` operations. Her kaynak sağlayıcısı, ilgili veri işlemleri gerçekleştirmek için API kümesi sağlar.

> [!NOTE]
> Bir kullanıcı bir veri işlemi dışlayan bir rol atanıp atanmadığını `notDataActions`ve kullanıcının bu veri işlemi gerçekleştirmek için izin verilir aynı veri işleme erişim veren ikinci bir rol atanır. `notDataActions` bir reddetme değil kural –, dışlanacak belirli veri işlemleriyle ihtiyacınız olduğunda izin verilen veri işlemleri bir dizi oluşturmak için yalnızca uygun bir yoldur.
>

## <a name="assignablescopes"></a>assignableScopes

`assignableScopes` Özellik rol atama için kullanılabilir olduğunu kapsamlar (yönetim gruplarına (şu anda önizlemede), abonelik, kaynak grupları veya kaynak) belirtir. Rol atama için uygun abonelikleri yalnızca yapabileceğiniz veya ve değil dağınıklığı kullanıcı gerektiren kaynak grupları aboneliklere veya kaynak gruplarına geri kalanı için karşılaşabilirsiniz. En az bir yönetim kullanmalısınız grup, aboneliği, kaynak grubu veya kaynak kimliği.

Yerleşik rolleri `assignableScopes` kök kapsamı ayarlayın (`"/"`). Kök kapsam rolü tüm kapsamlarda atama için uygun olduğunu gösterir. Geçerli atanabilir kapsamlarla örnekleri şunlardır:

| Senaryo | Örnek |
|----------|---------|
| Rol ataması tek bir abonelik için kullanılabilir | `"/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"` |
| Rol ataması iki abonelik de kullanılabilir | `"/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e", "/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624"` |
| Rol ataması yalnızca ağ kaynak grubundaki kullanılabilir | `"/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network"` |
| Rol atamasını tüm kapsamlar için kullanılabilir | `"/"` |

Hakkında bilgi için `assignableScopes` özel roller için bkz: [özel roller](custom-roles.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Yerleşik roller](built-in-roles.md)
* [Özel roller](custom-roles.md)
* [Azure Resource Manager kaynak sağlayıcısı işlemleri](resource-provider-operations.md)
