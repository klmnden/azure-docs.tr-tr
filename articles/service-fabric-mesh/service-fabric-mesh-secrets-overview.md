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
ms.openlocfilehash: 4268356db5f46e92862e19d6391cfe5a94511270
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60810629"
---
# <a name="service-fabric-mesh-application-secrets"></a>Service Fabric Mesh uygulama gizli dizilerini
Service Fabric Mesh gizli Azure kaynaklarını destekler. Service Fabric Mesh gizli dizi herhangi bir depolama bağlantı dizeleri, parolalar veya güvenli şekilde iletilmesini ve depolanan gereken diğer değerleri gibi hassas metin bilgi olabilir.

![Kafes gizli dizileri genel bakış][sf-mesh-secrets-overview]

## <a name="mesh-secrets-resources"></a>Gizli dizileri kaynakları mesh
Mesh uygulaması gizli oluşur:
* A **gizli dizileri** kaynağı metin gizli dizileri depolayan bir kapsayıcıdır. İçindeki gizli dizileri **gizli dizileri** kaynak depolanır ve güvenli bir şekilde iletilen.
* Bir veya daha fazla **gizli anahtarları/değerleri** depolanan kaynakları **gizli dizileri** kaynak kapsayıcısı. Her **gizli anahtarları/değerleri** bir sürüm numarası tarafından kaynak ayırt edici.

## <a name="next-steps"></a>Sonraki adımlar 
Service Fabric Mesh gizli dizileri hakkında daha fazla bilgi için bkz:
- [Service Fabric Mesh uygulama parolalarını yönetme](service-fabric-mesh-howto-manage-secrets.md)
- [Service Fabric kaynak modeli giriş](service-fabric-mesh-service-fabric-resources.md)

<!-- pics -->
[sf-mesh-secrets-overview]: ./media/service-fabric-mesh-secrets-overview/MeshAppSecretsOverview.png
