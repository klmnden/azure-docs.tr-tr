---
title: Azure Service Fabric kapsayıcı Hizmetleri için gMSA ayarlama | Microsoft Docs
description: Artık Azure Service Fabric'te çalışan bir kapsayıcı için gMSA ayarlama hakkında bilgi alın.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: timlt
editor: ''
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/23/2018
ms.author: aljo, subramar
ms.openlocfilehash: 4ad697e01ef9e023232e2a2a16e4584a2779f84a
ms.sourcegitcommit: 7f7c2fe58c6cd3ba4fd2280e79dfa4f235c55ac8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2019
ms.locfileid: "56806309"
---
# <a name="set-up-gmsa-for-windows-containers-running-on-service-fabric"></a>Service Fabric üzerinde çalışan Windows kapsayıcıları için gMSA ayarlama

Bir kimlik bilgisi belirtimi dosyası gMSA ' (Grup yönetilen hizmet hesapları) ayarlamak için (`credspec`) kümedeki tüm düğümlere yerleştirilir. VM uzantısı kullanarak tüm düğümlerinde dosya kopyalanabilir.  `credspec` Dosya gMSA hesap bilgilerini içermesi gerekir. Daha fazla bilgi için `credspec` bkz [hizmet hesapları](https://github.com/MicrosoftDocs/Virtualization-Documentation/tree/live/windows-server-container-tools/ServiceAccounts). Kimlik bilgisi belirtimi ve `Hostname` etiketi, uygulama bildiriminde belirtilir. `Hostname` Etiketi kapsayıcısı altında çalışan gMSA hesabı adıyla eşleşmelidir.  `Hostname` Etiketi diğer Kerberos kimlik doğrulaması kullanarak etki alanı Hizmetleri'nde kendi kimliğini doğrulamak kapsayıcı sağlar.  Belirtmek için bir örnek `Hostname` ve `credspec` uygulama bildirimi aşağıdaki kod parçacığında gösterilir:

```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="process" Hostname="gMSAAccountName">
    <SecurityOption Value="credentialspec=file://WebApplication1.json"/>
  </ContainerHostPolicies>
</Policies>
```
Sonraki adım olarak, bu makaleleri okuyun:

* [Service fabric'e Windows Server 2016 üzerinde bir Windows kapsayıcısı dağıtma](service-fabric-get-started-containers.md)
* [Linux üzerinde Service Fabric için bir Docker kapsayıcısı dağıtma](service-fabric-get-started-containers-linux.md)
