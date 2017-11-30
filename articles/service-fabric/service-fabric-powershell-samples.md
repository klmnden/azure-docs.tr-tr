---
title: "Azure PowerShell örnekleri - Service Fabric | Microsoft Docs"
description: "Azure PowerShell örnekleri - Service Fabric"
services: service-fabric
documentationcenter: service-fabric
author: rwike77
manager: timlt
editor: 
tags: 
ms.assetid: b48d1137-8c04-46e0-b430-101e07d7e470
ms.service: service-fabric
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: service-fabric
ms.date: 11/28/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 738677def8b0eb70cfcab46e6fe57f9a344867a5
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="azure-powershell-samples"></a>Azure PowerShell örnekleri

Aşağıdaki tabloda Service Fabric kümeleri, uygulamaları ve hizmetleri oluşturmak ve yönetmek PowerShell komut dosyaları örnekleri bağlantılarını içerir.

[!INCLUDE [links to azure cli and service fabric cli](../../includes/service-fabric-powershell.md)]

| | |
|-|-|
| **Küme oluşturma** ||
| [Bir küme (Azure) oluşturma](./scripts/service-fabric-powershell-create-secure-cluster-cert.md)| Azure Service Fabric kümesi oluşturur. |
| **Küme, düğümleri ve altyapı yönetme** ||
| [Bir uygulama sertifika Ekle](./scripts/service-fabric-powershell-add-application-certificate.md)| Bir kümedeki tüm düğümler için bir uygulama X.509 sertifikası ekler. |
| [Güncelleştirme kümedeki sanal makineleri RDP bağlantı noktası aralığı](./scripts/service-fabric-powershell-change-rdp-port-range.md)|Dağıtılan bir kümedeki sanal makineleri küme düğümünde RDP bağlantı noktası aralığı değiştirir.|
| [Yönetici kullanıcı adı ve parola küme düğümü VM'ler için güncelleştirme](./scripts/service-fabric-powershell-change-rdp-user-and-pw.md) | Yönetici kullanıcı adı ve parola küme düğümü VM'ler için güncelleştirir. |
| [Yük dengeleyicide bağlantı noktası açma](./scripts/service-fabric-powershell-open-port-in-load-balancer.md) | Belirli bir bağlantı noktasını gelen trafiğe izin verecek şekilde Azure yük dengeleyici, bir uygulama bağlantı noktasını açın. |
| [Bir gelen ağ güvenlik grubu kuralı oluşturma](./scripts/service-fabric-powershell-add-nsg-rule.md) | Belirli bir bağlantı noktası üzerindeki kümeye gelen trafiğe izin vermek için bir gelen ağ güvenlik grubu kural oluşturun. |
| **Uygulamaları yönetme** ||
| [Bir uygulamayı dağıtma](./scripts/service-fabric-powershell-deploy-application.md)| Bir küme için bir uygulamayı dağıtın.|
| [Uygulama yükseltme](./scripts/service-fabric-powershell-upgrade-application.md)| Uygulama yükseltme.|
| [Bir uygulamayı kaldırma](./scripts/service-fabric-powershell-remove-application.md)| Bir uygulama bir kümeden kaldırın.|
