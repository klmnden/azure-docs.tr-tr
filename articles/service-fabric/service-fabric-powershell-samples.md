---
title: Azure PowerShell Örnekleri - Service Fabric | Microsoft Docs
description: Azure PowerShell Örnekleri - Service Fabric
services: service-fabric
documentationcenter: service-fabric
author: athinanthny
manager: chackdan
editor: ''
tags: ''
ms.assetid: b48d1137-8c04-46e0-b430-101e07d7e470
ms.service: service-fabric
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: service-fabric
ms.date: 11/29/2018
ms.author: atsenthi
ms.custom: mvc
ms.openlocfilehash: 8a3a80fb6ae20eddc3237d986ecda1d4cb5b65a5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60773249"
---
# <a name="azure-powershell-samples"></a>Azure PowerShell örnekleri

Aşağıdaki tablo, Service Fabric kümelerini, uygulamalarını ve hizmetlerini oluşturup yöneten PowerShell betiği örneklerinin bağlantılarını içerir.

[!INCLUDE [links to azure cli and service fabric cli](../../includes/service-fabric-powershell.md)]

| | |
|-|-|
| **Küme oluşturma** ||
| [Küme oluşturma (Azure)](./scripts/service-fabric-powershell-create-secure-cluster-cert.md)| Bir Azure Service Fabric kümesi oluşturur. |
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
