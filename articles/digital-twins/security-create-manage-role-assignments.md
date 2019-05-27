---
title: Oluşturma ve rol atamalarında Azure dijital İkizlerini yönetme | Microsoft Docs
description: Oluşturun ve Azure dijital İkizlerini rol atamalarında yönetin.
author: lyrana
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 12/26/2018
ms.author: lyrana
ms.custom: seodec18
ms.openlocfilehash: 72155799971760e9ddc93746dceafb1ea554d88b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66162101"
---
# <a name="create-and-manage-role-assignments-in-azure-digital-twins"></a>Oluşturma ve Azure dijital İkizlerini rol atamalarını yönetme

Azure dijital İkizlerini kullanır rol tabanlı erişim denetimi ([RBAC](./security-role-based-access-control.md)) kaynaklara erişimi yönetmek için.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="role-assignments-overview"></a>Rol atamaları genel bakış

Her rol ataması aşağıdaki tanımına uyan:

```JSON
{
  "roleId": "00e00ad7-00d4-4007-853b-b9968ad000d1",
  "objectId": "be2c6daa-a3a0-0c0a-b0da-c000000fbc5f",
  "objectIdType": "ServicePrincipalId",
  "path": "/",
  "tenantId": "00f000bf-86f1-00aa-91ab-2d7cd000db47"
}
```

Aşağıdaki tabloda her bir öznitelik açıklanmaktadır:

| Öznitelik | Ad | Gerekli | Tür | Açıklama |
| --- | --- | --- | --- | --- |
| Rol Kimliği | Rol tanımı tanımlayıcısı | Evet | String | İstenen rol atama benzersiz kimliği. Rol tanımları ve bunların tanımlayıcısı sistem API sorgulama veya aşağıdaki tabloda gözden geçirme bulun. |
| objectId | Nesne tanımlayıcısı | Evet | String | Bir Azure Active Directory Kimliğini, hizmet sorumlusu nesne kimliği veya etki alanı adı. Hangi veya rol ataması atandığı. Rol ataması, ilişkili türüne göre biçimlendirilmelidir. İçin `DomainName` objectIdType, nesne kimliği ile başlamalıdır `“@”` karakter. |
| objectIdType | Nesne tanımlayıcı türü | Evet | String | Nesne tanımlayıcısı kullanılan tür. Bkz: **ObjectIdTypes desteklenen** aşağıda. |
| path | Alan yolu | Evet | String | Tam erişim yolu `Space` nesne. `/{Guid}/{Guid}` bunun bir örneğidir. Tanımlayıcı için tüm grafı rol ataması gerekiyorsa belirtin `"/"`. Bu karakteri kök belirler ancak kullanımı önerilmez. Her zaman ilkesine en düşük öncelik ilkesini uygulayın. |
| Kiracı kimliği | Kiracı tanımlayıcısı | Değişir | String | Çoğu durumda, bir Azure Active Directory Kiracı kimliği İçin izin verilmeyen `DeviceId` ve `TenantId` ObjectIdTypes. Gerekli `UserId` ve `ServicePrincipalId` ObjectIdTypes. DomainName ObjectIdType için isteğe bağlı. |

### <a name="supported-role-definition-identifiers"></a>Desteklenen rol tanımı tanımlayıcıları

Her rol ataması rol tanımı Azure dijital İkizlerini ortamınızdaki bir varlık ile ilişkilendirir.

[!INCLUDE [digital-twins-roles](../../includes/digital-twins-roles.md)]

### <a name="supported-object-identifier-types"></a>Desteklenen bir nesne tanımlayıcı türleri

Daha önce **objectIdType** özniteliği tanıtılmıştır.

[!INCLUDE [digital-twins-object-types](../../includes/digital-twins-object-id-types.md)]

## <a name="role-assignment-operations"></a>Rol atama işlemleri

Azure dijital İkizlerini tam destekleyen *Oluştur*, *okuma*, ve *Sil* işlemleri için rol atamalarını. *Güncelleştirme* rol atamaları ekleme, rol atamalarını kaldırma veya değiştirme işlemleri işlenmesini [uzamsal zeka graf](./concepts-objectmodel-spatialgraph.md) rol atamaları veren düğümlerine erişmek için.

![Rol ataması uç noktaları][1]

Sağlanan Swagger başvuru belgeleri, ayrıca tüm kullanılabilir API uç noktalarını, istek işlemleri ve tanımları hakkında bilgi içerir.

[!INCLUDE [Digital Twins Swagger](../../includes/digital-twins-swagger.md)]

[!INCLUDE [Digital Twins Management API](../../includes/digital-twins-management-api.md)]

<div id="grant"></div>

### <a name="grant-permissions-to-your-service-principal"></a>Hizmet sorumlunuzu izinleri verme

Hizmet sorumlusu izinleri veriliyor genellikle Azure dijital İkizlerini kullanmaya çalışırken atacağız ilk adımlarından biridir. Bunu kapsar:

1. Azure Örneğinize PowerShell aracılığıyla oturum.
1. Hizmet sorumlusu bilgileri alınıyor.
1. İstenen rol, hizmet sorumlusu atama.

Uygulama Kimliğinizi, Azure Active Directory'de sağlanır. Ve hakkında daha fazla yapılandırma Active Directory'de bir Azure dijital çiftleri sağlama okuyun [hızlı](./quickstart-view-occupancy-dotnet.md).

Uygulama Kimliğini aldıktan sonra aşağıdaki PowerShell komutlarını çalıştırın:

```shell
Login-AzAccount
Get-AzADServicePrincipal -ApplicationId  <ApplicationId>
```

Bir kullanıcıyla **yönetici** rol ardından atayabilirsiniz alan Yönetici rolü için bir kullanıcı kimliği doğrulanmış bir HTTP POST isteği URL'sini sağlayarak:

```plaintext
YOUR_MANAGEMENT_API_URL/roleassignments
```

Aşağıdaki JSON gövdesi ile:

```JSON
{
  "roleId": "98e44ad7-28d4-4007-853b-b9968ad132d1",
  "objectId": "YOUR_SERVICE_PRINCIPLE_OBJECT_ID",
  "objectIdType": "ServicePrincipalId",
  "path": "YOUR_PATH",
  "tenantId": "YOUR_TENANT_ID"
}
```

<div id="all"></div>

### <a name="retrieve-all-roles"></a>Tüm rolleri alma

![Sistem rolleri][2]

Kullanılabilir tüm roller (rol tanımları) listelemek için kimliği doğrulanmış bir HTTP GET isteği olun:

```plaintext
YOUR_MANAGEMENT_API_URL/system/roles
```

Başarılı bir istek girişi atanabilir her rol için bir JSON dizisi döndürür:

```JSON
[
    {
        "id": "3cdfde07-bc16-40d9-bed3-66d49a8f52ae",
        "name": "DeviceAdministrator",
        "permissions": [
            {
                "notActions": [],
                "actions": [
                    "Read",
                    "Create",
                    "Update",
                    "Delete"
                ],
                "condition": "@Resource.Type Any_of {'Device', 'DeviceBlobMetadata', 'DeviceExtendedProperty', 'Sensor', 'SensorBlobMetadata', 'SensorExtendedProperty'} || ( @Resource.Type == 'ExtendedType' && (!Exists @Resource.Category || @Resource.Category Any_of { 'DeviceSubtype', 'DeviceType', 'DeviceBlobType', 'DeviceBlobSubtype', 'SensorBlobSubtype', 'SensorBlobType', 'SensorDataSubtype', 'SensorDataType', 'SensorDataUnitType', 'SensorPortType', 'SensorType' } ) )"
            },
            {
                "notActions": [],
                "actions": [
                    "Read"
                ],
                "condition": "@Resource.Type == 'Space' && @Resource.Category == 'WithoutSpecifiedRbacResourceTypes' || @Resource.Type Any_of {'ExtendedPropertyKey', 'SpaceExtendedProperty', 'SpaceBlobMetadata', 'SpaceResource', 'Matcher'}"
            }
        ],
        "accessControlPath": "/system",
        "friendlyPath": "/system",
        "accessControlType": "System"
    }
]
```

<div id="check"></div>

### <a name="check-a-specific-role-assignment"></a>Özel rol atamasını denetleyin

Özel rol atamasını denetlemek için kimliği doğrulanmış bir HTTP GET isteği olun:

```plaintext
YOUR_MANAGEMENT_API_URL/roleassignments/check?userId=YOUR_USER_ID&path=YOUR_PATH&accessType=YOUR_ACCESS_TYPE&resourceType=YOUR_RESOURCE_TYPE
```

| **Parametre değeri** | **Gerekli** |  **Tür** |  **Açıklama** |
| --- | --- | --- | --- |
| YOUR_USER_ID |  True | String |   UserId objectIdType için objectID. |
| YOUR_PATH | True | String |   Erişimi denetlemek için seçilen yolu. |
| YOUR_ACCESS_TYPE |  True | String |   Denetlenecek erişim türü. |
| YOUR_RESOURCE_TYPE | True | String |  Denetlenecek kaynak. |

Başarılı bir isteği bir Boole değeri döndürür `true` veya `false` erişim türü için belirtilen yol ve kaynak kullanıcıya atanmış olup olmadığını belirtmek için.

### <a name="get-role-assignments-by-path"></a>Yol tarafından rol atamalarını Al

Tüm rol atamalarını yolunu almak için kimliği doğrulanmış bir HTTP GET isteği olun:

```plaintext
YOUR_MANAGEMENT_API_URL/roleassignments?path=YOUR_PATH
```

| Değer | Şununla değiştir |
| --- | --- |
| YOUR_PATH | Alanı tam yolu |

Başarılı bir istek seçili ile ilişkili her rol atamasına sahip bir JSON dizisi döndürür **yolu** parametresi:

```JSON
[
    {
        "id": "0000c484-698e-46fd-a3fd-c12aa11e53a1",
        "roleId": "98e44ad7-28d4-4007-853b-b9968ad132d1",
        "objectId": "0de38846-1aa5-000c-a46d-ea3d8ca8ee5e",
        "objectIdType": "UserId",
        "path": "/"
    }
]
```

### <a name="revoke-a-permission"></a>İzni iptal et

Bir alıcı izinlerini iptal etmek için kimliği doğrulanmış bir HTTP DELETE isteği yaparak rol atamasını silin:

```plaintext
YOUR_MANAGEMENT_API_URL/roleassignments/YOUR_ROLE_ASSIGNMENT_ID
```

| Parametre | Şununla değiştir |
| --- | --- |
| *YOUR_ROLE_ASSIGNMENT_ID* | **Kimliği** kaldırmak için rol atama |

Başarılı bir DELETE isteği 204 yanıt durumu döndürülür. Rol ataması tarafından kaldırılmasını doğrulayın [denetimi](#check) olup rol ataması hala tutar.

### <a name="create-a-role-assignment"></a>Rol ataması oluşturma

Bir rol ataması oluşturmak için kimliği doğrulanmış bir HTTP POST isteği URL'sini olun:

```plaintext
YOUR_MANAGEMENT_API_URL/roleassignments
```

JSON gövdesi aşağıdaki şemaya uygun olduğunu doğrulayın:

```JSON
{
  "roleId": "YOUR_ROLE_ID",
  "objectId": "YOUR_OBJECT_ID",
  "objectIdType": "YOUR_OBJECT_ID_TYPE",
  "path": "YOUR_PATH",
  "tenantId": "YOUR_TENANT_ID"
}
```

Başarılı bir istek 201 yanıtında durumu ile birlikte döndüreceği **kimliği** yeni oluşturulan rolü atama:

```JSON
"d92c7823-6e65-41d4-aaaa-f5b32e3f01b9"
```

## <a name="configuration-examples"></a>Yapılandırma örnekleri

Aşağıdaki örnekler birkaç yaygın olarak karşılaşılan rol ataması senaryolarda, JSON gövde yapılandırmak nasıl ekleyebileceğiniz gösterilmektedir.

* **Örnek**: Bir kullanıcının yönetimsel erişim zemini Kiracı alanı gerekir.

   ```JSON
   {
    "roleId": "98e44ad7-28d4-4007-853b-b9968ad132d1",
    "objectId" : " 0fc863aa-eb51-4704-a312-7d635d70e000",
    "objectIdType" : "UserId",
    "tenantId": " a0c20ae6-e830-4c60-993d-a00ce6032724",
    "path": "/ 000e349c-c0ea-43d4-93cf-6b00abd23a44/ d84e82e6-84d5-45a4-bd9d-006a000e3bab"
   }
   ```

* **Örnek**: Bir uygulama cihazlardan ve sensörlerden sahte test senaryolarında çalışır.

   ```JSON
   {
    "roleId": "98e44ad7-28d4-0007-853b-b9968ad132d1",
    "objectId" : "cabf7aaa-af0b-41c5-000a-ce2f4c20000b",
    "objectIdType" : "ServicePrincipalId",
    "tenantId": " a0c20ae6-e000-4c60-993d-a91ce6000724",
    "path": "/"
   }
    ```

* **Örnek**: Bir etki alanının parçası olan tüm kullanıcılar, boşluk, algılayıcılar ve kullanıcılar için okuma erişimi alırsınız. Bu erişim, karşılık gelen ilgili nesneleri içerir.

   ```JSON
   {
    "roleId": " b1ffdb77-c635-4e7e-ad25-948237d85b30",
    "objectId" : "@microsoft.com",
    "objectIdType" : "DomainName",
    "path": "/000e349c-c0ea-43d4-93cf-6b00abd23a00"
   }
   ```

## <a name="next-steps"></a>Sonraki adımlar

- Azure dijital İkizlerini rol-bağlı-erişim denetimi gözden geçirmek için okuma [temel erişim denetimi rol](./security-authenticating-apis.md).

- Azure dijital İkizlerini API kimlik doğrulaması hakkında bilgi edinmek için [API kimlik doğrulaması](./security-authenticating-apis.md).

<!-- Images -->
[1]: media/security-roles/roleassignments.png
[2]: media/security-roles/system.png
