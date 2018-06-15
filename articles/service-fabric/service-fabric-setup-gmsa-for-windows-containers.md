---
title: Azure Service Fabric kapsayıcı hizmetlerini gMSA Kurulumu | Microsoft Docs
description: Artık Azure Service Fabric çalıştıran kapsayıcı için gMSA Kurulum öğrenin.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: ''
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/23/2018
ms.author: subramar
ms.openlocfilehash: e4cd0b42e21609f88edc28c8fd7b5c433d56b3c1
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34209102"
---
# <a name="set-up-gmsa-for-windows-containers-running-on-service-fabric"></a>GMSA Service Fabric üzerinde çalışan Windows kapsayıcıları için ayarlama

Bir kimlik bilgisi belirtimi dosyası gmsa'yı (Grup yönetilen hizmet hesapları) ayarlamak için (`credspec`) kümedeki tüm düğümlere yerleştirilir. Dosya bir VM uzantısı kullanılarak tüm düğümlerde kopyalanabilir.  `credspec` Dosya gMSA hesabı bilgileri içermesi gerekir. Daha fazla bilgi için `credspec` dosya için bkz: [hizmet hesaplarını](https://github.com/MicrosoftDocs/Virtualization-Documentation/tree/live/windows-server-container-tools/ServiceAccounts). Kimlik bilgisi belirtimi ve `Hostname` etiketi, uygulama bildiriminde belirtilir. `Hostname` Etiketi kapsayıcı çalıştığı gMSA hesabı adı eşleşmelidir.  `Hostname` Etiketi Kerberos kimlik doğrulaması kullanarak etki alanındaki diğer hizmetler için kendi kimliğini doğrulamak kapsayıcı sağlar.  Belirtmek için bir örnek `Hostname` ve `credspec` uygulama bildirimi aşağıdaki kod parçacığında gösterilir:

```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="process" Hostname="gMSAAccountName">
    <SecurityOption Value="credentialspec=file://WebApplication1.json"/>
  </ContainerHostPolicies>
</Policies>
```
Sonraki adım olarak, aşağıdaki makaleler okuyun:

* [Windows Server 2016 Service Fabric Windows kapsayıcı dağıtma](service-fabric-get-started-containers.md)
* [Service Fabric Linux'ta Docker kapsayıcısı dağıtma](service-fabric-get-started-containers-linux.md)
