---
title: Azure Active Directory Tümleştirmesi ile Azure Cosmos DB'de rol tabanlı erişim denetimi
description: Azure Cosmos DB veritabanı koruması ile Active directory ile tümleştirme (RBAC) nasıl sağladığını öğrenin.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: mjbrown
ms.openlocfilehash: 971d2ec96906a3309963495dd1af5d293a71f265
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66243512"
---
# <a name="role-based-access-control-in-azure-cosmos-db"></a>Azure Cosmos DB'de rol tabanlı erişim denetimi

Azure Cosmos DB, Azure Cosmos DB'de ortak yönetim senaryoları için yerleşik rol tabanlı erişim denetimi (RBAC) sağlar. Azure Active Directory'de bir profili olan bir kullanıcı bu RBAC rolleri atayabilirsiniz kullanıcıları, grupları, hizmet sorumluları veya yönetilen kaynaklar ve Azure Cosmos DB kaynaklarını işlemleri için erişim vermek veya reddetmek için kimlikleri. Azure Cosmos hesapları, veritabanları, kapsayıcıları erişimi içerir ve (üretilen iş) denetim düzlemi erişimi yalnızca Rol atamaları belirlenir.

## <a name="built-in-roles"></a>Yerleşik roller

Azure Cosmos DB tarafından desteklenen yerleşik roller şunlardır:

|**Yerleşik rol**  |**Açıklama**  |
|---------|---------|
|[DocumentDB hesabı Katılımcısı](../role-based-access-control/built-in-roles.md#documentdb-account-contributor)   | Azure Cosmos DB hesapları yönetebilirsiniz.  |
|[Cosmos DB hesabı okuyucusu](../role-based-access-control/built-in-roles.md#cosmos-db-account-reader-role)  | Azure Cosmos DB hesabı verileri okuyabilir.        |
|[Cosmos yedekleme işletmeni](../role-based-access-control/built-in-roles.md#cosmosbackupoperator)     |  Bir Azure Cosmos veritabanı veya bir kapsayıcı için geri yükleme isteği gönderebilirsiniz.       |
|[Cosmos DB işleci](../role-based-access-control/built-in-roles.md#cosmos-db-operator)  | Azure Cosmos hesapları, veritabanları ve kapsayıcıları sağlayabilirsiniz ancak verilere erişmek için gereken anahtarları erişemez.         |

> [!IMPORTANT]
> Azure Cosmos DB'de RBAC desteği yalnızca denetim düzlemi işlemleri için geçerlidir. Veri düzlemi işlemleri, ana anahtarları veya kaynak belirteçleri kullanılarak güvenli hale getirilir. Daha fazla bilgi için bkz: [Azure Cosmos DB'de veri erişiminin güvenliğini sağlama](secure-access-to-data.md)

## <a name="identity-and-access-management-iam"></a>Kimlik ve erişim yönetimi (IAM)

**Erişim denetimi (IAM)** bölmesinde Azure portalında Azure Cosmos kaynaklar üzerinde rol tabanlı erişim denetimini yapılandırmak için kullanılır. Roller, kullanıcıları, grupları, hizmet sorumluları ve Active Directory'deki yönetilen kimlikleri için uygulanır. Bireyler ve gruplar için yerleşik veya özel roller kullanabilirsiniz. Azure portalında erişim denetimi (IAM) kullanarak Active Directory Tümleştirme (RBAC) aşağıdaki ekran gösterilir:

![Veritabanı güvenliği gösteren Azure portalı - erişim denetimi (IAM)](./media/role-based-access-control/database-security-identity-access-management-rbac.png)

## <a name="custom-roles"></a>Özel roller

Ek olarak yerleşik roller, kullanıcılar ayrıca oluşturabilirsiniz [özel roller](../role-based-access-control/custom-roles.md) azure'da ve bu rolleri, Active Directory kiracılarının içindeki tüm Aboneliklerdeki hizmet sorumluları uygulayın. Özel roller kullanıcılara RBAC rolü tanımları sağlayıcısı işlemleri özel bir kaynak kümesi oluşturmak için bir yol sağlar. Hangi işlemleri için bkz. Azure Cosmos DB, özel roller oluşturmak için kullanılabilir olduğunu öğrenmek için [Azure Cosmos DB kaynak sağlayıcısı işlemleri](../role-based-access-control/resource-provider-operations.md#microsoftdocumentdb)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure kaynakları için rol tabanlı erişim denetimi (RBAC) nedir](../role-based-access-control/overview.md)
- [Azure kaynakları için özel roller](../role-based-access-control/custom-roles.md)
- [Azure Cosmos DB kaynak sağlayıcısı işlemleri](../role-based-access-control/resource-provider-operations.md#microsoftdocumentdb)
