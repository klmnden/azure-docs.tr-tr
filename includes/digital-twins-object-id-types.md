---
title: include dosyası
description: include dosyası
services: digital-twins
author: kingdomofends
ms.service: digital-twins
ms.topic: include
ms.date: 12/20/2018
ms.author: adgera
ms.custom: include file
ms.openlocfilehash: 40ab53c941a7ac619ebb09d381a4ae0450f26e8b
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67189004"
---
`objectIdType` (Veya **nesne tanımlayıcı türü**) bir role verilen kimlik türünü ifade eder. Gelen apart `DeviceId` ve `UserDefinedFunctionId` türleri, nesne tanımlayıcısı türleri için Azure Active Directory nesnelerin özelliklerini karşılık gelir.

Aşağıdaki tabloda Azure dijital İkizlerini desteklenen nesne tanımlayıcı türlerini içerir:

| Tür | Açıklama |
| --- | --- |
| UserId | Bir rol, bir kullanıcıya atar. |
| DeviceId | Bir rol için bir cihaz atar. |
| DomainName | Bir rol için bir etki alanı adı atar. Her kullanıcı belirtilen etki alanı adına karşılık gelen rolü erişim haklarına sahiptir. |
| TenantId | Bir rol için bir kiracı atar. Belirtilen ait her kullanıcının Azure AD Kiracı kimliği, karşılık gelen rolü erişim haklarına sahip. |
| ServicePrincipalId | Bir rol için bir hizmet sorumlusu nesne kimliği atar. |
| UserDefinedFunctionId | Bir rol, kullanıcı tanımlı bir işlev için (UDF) atar. |