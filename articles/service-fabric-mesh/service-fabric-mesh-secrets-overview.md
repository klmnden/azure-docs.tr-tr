---
title: Depolama ve Azure Service Fabric kullanarak uygulama gizli dizilerini Mesh | Microsoft Docs
description: Depolama ve Service Fabric Mesh gizli dizileri kullanma.
services: service-fabric-mesh
keywords: gizli dizi
author: v-steg
ms.author: v-steg
ms.date: 10/25/2018
ms.topic: conceptual
ms.service: service-fabric-mesh
manager: jeanpaul.connock
ms.openlocfilehash: b6db9b6cb2ccbf1c8d29edb817d35677109ae755
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2019
ms.locfileid: "57768630"
---
# <a name="service-fabric-mesh-application-secrets"></a>Service Fabric Mesh uygulama gizli dizilerini
Service Fabric Mesh gizli Azure kaynaklarını destekler. Service Fabric Mesh gizli dizi herhangi bir depolama bağlantı dizeleri, parolalar veya güvenli şekilde iletilmesini ve depolanan gereken diğer değerleri gibi hassas metin bilgi olabilir.

![Kafes gizli dizileri genel bakış][sf-mesh-secrets-overview]

## <a name="mesh-secrets-resources"></a>Gizli dizileri kaynakları mesh
Mesh uygulaması gizli oluşur:
* A **gizli dizileri** kaynağı metin gizli dizileri depolayan bir kapsayıcıdır. İçindeki gizli dizileri **gizli dizileri** kaynak depolanır ve güvenli bir şekilde iletilen.
* Bir veya daha fazla **gizli anahtarları/değerleri** depolanan kaynakları **gizli dizileri** kaynak kapsayıcısı. Her **gizli anahtarları/değerleri** bir sürüm numarası tarafından kaynak ayırt edici.

## <a name="inline-or-stored-in-azure-key-vault"></a>Satır içi veya depolanan Azure Key Vault
- Mesh uygulama ve hizmet kaynaklarını Azure Key Vault gizli dizileri erişebilmesi için Yönetilen hizmet kimliği (MSI) ile Azure Active Directory (AAD) içerir.
- Parolalara ve sertifikalara ilkeleriyle alındı-üzerinde otomatik olabilir.

## <a name="next-steps"></a>Sonraki adımlar 
Service Fabric Mesh gizli dizileri hakkında daha fazla bilgi için bkz:
- [Service Fabric Mesh uygulama parolalarını yönetme](service-fabric-mesh-howto-manage-secrets.md)
- [Service Fabric kaynak modeli giriş](service-fabric-mesh-service-fabric-resources.md)

<!-- pics -->
[sf-mesh-secrets-overview]: ./media/service-fabric-mesh-secrets-overview/MeshAppSecretsOverview.png
