---
title: Azure dijital İkizlerini cihaz bağlantısı ve kimlik doğrulaması'nı anlama | Microsoft Docs
description: Azure dijital İkizlerini bağlama ve cihazların kimliklerini doğrulamak için kullanın
author: lyrana
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 11/13/2018
ms.author: lyrana
ms.openlocfilehash: f032e3ebf6a10411057cd6d41df0cad6248f328b
ms.sourcegitcommit: 542964c196a08b83dd18efe2e0cbfb21a34558aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2018
ms.locfileid: "51636246"
---
# <a name="create-and-manage-role-assignments"></a>Rol atamalarını oluşturma ve yönetme

Azure dijital İkizlerini kullanır rol tabanlı erişim denetimi ([RBAC](./security-role-based-access-control.md)) kaynaklara erişimi yönetmek için.

Her rol ataması içerir:

* **Nesne tanımlayıcısı**: bir Azure Active Directory Kimliğini, hizmet sorumlusu nesne kimliği veya etki alanı adı
* **Nesne tanımlayıcı türü**
* **Rol tanımı kimliği**
* **Alan yolu**
* **Kiracı kimliği**: Çoğu durumda, Azure Active Directory Kiracı kimliği

[!INCLUDE [Digital Twins Management API](../../includes/digital-twins-management-api.md)]

## <a name="role-definition-identifiers"></a>Rol tanımı tanımlayıcıları

Aşağıdaki tabloda, hangi system/roller API sorgulayarak alınabilir gösterilmektedir.

| **Rol** | **tanımlayıcı** |
| --- | --- |
| Alanı yöneticisi | 98e44ad7-28d4-4007-853b-b9968ad132d1 |
| Kullanıcı Yöneticisi| dfaac54c-f583-4dd2-b45d-8d4bbc0aa1ac |
| Cihaz Yöneticisi | 3cdfde07-bc16-40d9-bed3-66d49a8f52ae |
| Anahtar Yöneticisi | 5a0b1afc-e118-4068-969f-b50efb8e5da6 |
| Belirteç yönetici | 38a3bb21-5424-43b4-b0bf-78ee228840c3 |
| Kullanıcı | b1ffdb77-c635-4e7e-ad25-948237d85b30 |
| Destek Uzmanı | 6e46958b-dc62-4e7c-990c-c3da2e030969 |
| Cihaz yükleyici | b16dd9fe-4efe-467b-8c8c-720e2ff8817c |
| Ağ geçidi cihazı | d4c69766-e9bd-4e61-BFC1-d8b6e686c7a8 |

## <a name="supported-objectidtypes"></a>Desteklenen ObjectIdTypes

Desteklenen `ObjectIdTypes`:

* `UserId`
* `DeviceId`
* `DomainName`
* `TenantId`
* `ServicePrincipalId`
* `UserDefinedFunctionId`

## <a name="create-a-role-assignment"></a>Rol ataması oluşturma

```plaintext
HTTP POST YOUR_MANAGEMENT_API_URL/roleassignments
```

| **Ad** | **Gerekli** | **Tür** | **Açıklama** |
| --- | --- | --- | --- |
| Rol Kimliği| Evet |Dize | Rol tanımı tanımlayıcısı. Rol tanımları ve bunların tanımlayıcıları sistem API'si sorgulayarak bulun. |
| objectId | Evet |Dize | Nesne kimliği, ilişkili türüne göre biçimlendirilmelidir rol ataması. İçin `DomainName` ObjectIdType, nesne kimliği ile başlamalıdır `“@”` karakter. |
| objectIdType | Evet |Dize | Rol ataması türü. Bu tabloda aşağıdaki satırları biri olmalıdır. |
| Kiracı kimliği | Değişir | Dize |Kiracı tanımlayıcısı. İçin izin verilmeyen `DeviceId` ve `TenantId` ObjectIdTypes. Gerekli `UserId` ve `ServicePrincipalId` ObjectIdTypes. DomainName ObjectIdType için isteğe bağlı. |
| yol * | Evet | Dize |Tam erişim yolu `Space` nesne. `/{Guid}/{Guid}` bunun bir örneğidir. Tanımlayıcı için tüm grafı rol ataması gerekiyorsa belirtin `"/"`. Bu karakteri kök belirler ancak kullanımı önerilmez. Her zaman ilkesine en düşük öncelik ilkesini uygulayın. |

## <a name="sample-configuration"></a>Örnek yapılandırma

Bu örnekte, bir kullanıcının yönetimsel erişim zemini Kiracı alanı gerekir.

  ```JSON
    {
      "RoleId": "98e44ad7-28d4-4007-853b-b9968ad132d1",
      "ObjectId" : " 0fc863bb-eb51-4704-a312-7d635d70e599",
      "ObjectIdType" : "UserId",
      "TenantId": " a0c20ae6-e830-4c60-993d-a91ce6032724",
      "Path": "/ 091e349c-c0ea-43d4-93cf-6b57abd23a44/ d84e82e6-84d5-45a4-bd9d-006a118e3bab"
    }
  ```

Bu örnekte, bir uygulama cihazlardan ve sensörlerden sahte test senaryolarında çalışır.

  ```JSON
    {
      "RoleId": "98e44ad7-28d4-4007-853b-b9968ad132d1",
      "ObjectId" : "cabf7acd-af0b-41c5-959a-ce2f4c26565b",
      "ObjectIdType" : "ServicePrincipalId",
      "TenantId": " a0c20ae6-e830-4c60-993d-a91ce6032724",
      "Path": "/"
    }
  ```

Bir etki alanının parçası olan tüm kullanıcılar, boşluk, algılayıcılar ve kullanıcılar için okuma erişimi alırsınız. Bu erişim, karşılık gelen ilgili nesneleri içerir.

  ```JSON
    {
      "RoleId": " b1ffdb77-c635-4e7e-ad25-948237d85b30",
      "ObjectId" : "@microsoft.com",
      "ObjectIdType" : "DomainName",
      "Path": "/091e349c-c0ea-43d4-93cf-6b57abd23a44"
    }
  ```

EDİNİN bir rol ataması alınamıyor.

```plaintext
HTTP GET YOUR_MANAGEMENT_API_URL/roleassignments?path=YOUR_PATH
```

| **Ad** | **İçinde** | **Gerekli** |    **Tür** |  **Açıklama** |
| --- | --- | --- | --- | --- |
| YOUR_PATH | Yol | True | Dize |    Alanı tam yolu |

Bir rol atamasını silmek için DELETE kullanın.

```plaintext
HTTP DELETE YOUR_MANAGEMENT_API_URL/roleassignments/YOUR_ROLE_ID
```

| **Ad** | **İçinde** | **Gerekli** | **Tür** | **Açıklama** |
| --- | --- | --- | --- | --- |
| YOUR_ROLE_ID | Yol | True | Dize | Rol atama kimliği |

## <a name="next-steps"></a>Sonraki adımlar

Azure dijital İkizlerini güvenliği hakkında bilgi edinmek için [API kimlik doğrulaması](./security-authenticating-apis.md).
