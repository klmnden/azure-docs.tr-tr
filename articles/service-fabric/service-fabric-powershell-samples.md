---
title: Azure PowerShell Örnekleri - Service Fabric | Microsoft Docs
description: Azure PowerShell Örnekleri - Service Fabric
services: service-fabric
documentationcenter: service-fabric
author: rwike77
manager: timlt
editor: ''
tags: ''
ms.assetid: b48d1137-8c04-46e0-b430-101e07d7e470
ms.service: service-fabric
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: service-fabric
ms.date: 04/09/2018
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 92dbdf2d2e3bdc48cda9ef0e0a53212cc7ea4cb7
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="azure-powershell-samples"></a>Azure PowerShell örnekleri

Aşağıdaki tablo, Service Fabric kümelerini, uygulamalarını ve hizmetlerini oluşturup yöneten PowerShell betiği örneklerinin bağlantılarını içerir.

[!INCLUDE [links to azure cli and service fabric cli](../../includes/service-fabric-powershell.md)]

| | |
|-|-|
| **Küme oluşturma** ||
| [Küme oluşturma (Azure)](./scripts/service-fabric-powershell-create-secure-cluster-cert.md)| Bir Azure Service Fabric kümesi oluşturur. |
|[Test kümesi oluşturma (Azure)](./scripts/service-fabric-powershell-create-test-cluster.md)| Azure’da üç düğümlü bir test Service Fabric kümesi oluşturur.|
| **Kümeyi, düğümleri ve altyapıyı yönetme** ||
| [Uygulama sertifikası ekleme](./scripts/service-fabric-powershell-add-application-certificate.md)| Bir kümedeki tüm düğümlere uygulama X.509 sertifikası ekler. |
| [Küme sanal makinelerinde RDP bağlantı noktası aralığını güncelleştirme](./scripts/service-fabric-powershell-change-rdp-port-range.md)|Dağıtılan bir kümedeki küme düğümü sanal makinelerinde RDP bağlantı noktası aralığını değiştirir.|
| [Küme düğümü sanal makineleri için yönetici kullanıcı adını ve parolasını güncelleştirme](./scripts/service-fabric-powershell-change-rdp-user-and-pw.md) | Küme düğümü sanal makineleri için yönetici kullanıcı adını ve parolasını güncelleştirir. |
| [Yük dengeleyicide bağlantı noktası açma](./scripts/service-fabric-powershell-open-port-in-load-balancer.md) | Belirli bir bağlantı noktasında gelen trafiğe izin vermek için Azure yük dengeleyicide bir uygulama bağlantı noktasını açın. |
| [Gelen ağ güvenlik grubu kuralı oluşturma](./scripts/service-fabric-powershell-add-nsg-rule.md) | Belirli bir bağlantı noktası üzerindeki kümeye gelen trafiğe izin vermek için bir gelen ağ güvenlik grubu kuralı oluşturun. |
| **Uygulamaları yönetme** ||
| [Uygulama dağıtma](./scripts/service-fabric-powershell-deploy-application.md)| Kümeye bir uygulama dağıtın.|
| [Uygulamayı yükseltme](./scripts/service-fabric-powershell-upgrade-application.md)| Uygulamayı yükseltin.|
| [Uygulamayı kaldırma](./scripts/service-fabric-powershell-remove-application.md)| Bir kümeden uygulamayı kaldırın.|
