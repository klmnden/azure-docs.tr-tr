---
title: Azure Service Fabric kapsayıcı Hizmetleri için gMSA ayarlama | Microsoft Docs
description: Artık Azure Service Fabric'te çalışan bir kapsayıcı için gMSA ayarlama hakkında bilgi alın.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: ''
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/20/2019
ms.author: aljo, subramar
ms.openlocfilehash: ae8c5c8ec1e16669b3cbdde8b3eaa3d5dbb7c4de
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58668560"
---
# <a name="set-up-gmsa-for-windows-containers-running-on-service-fabric"></a>Service Fabric üzerinde çalışan Windows kapsayıcıları için gMSA ayarlama

Bir kimlik bilgisi belirtimi dosyası gMSA ' (Grup yönetilen hizmet hesapları) ayarlamak için (`credspec`) kümedeki tüm düğümlere yerleştirilir. VM uzantısı kullanarak tüm düğümlerinde dosya kopyalanabilir.  `credspec` Dosya gMSA hesap bilgilerini içermesi gerekir. Daha fazla bilgi için `credspec` bkz [kimlik bilgisi belirtimi oluşturma](https://docs.microsoft.com/en-us/virtualization/windowscontainers/manage-containers/manage-serviceaccounts#create-a-credential-spec). Kimlik bilgisi belirtimi ve `Hostname` etiketi, uygulama bildiriminde belirtilir. `Hostname` Etiketi kapsayıcısı altında çalışan gMSA hesabı adıyla eşleşmelidir.  `Hostname` Etiketi diğer Kerberos kimlik doğrulaması kullanarak etki alanı Hizmetleri'nde kendi kimliğini doğrulamak kapsayıcı sağlar.  Belirtmek için bir örnek `Hostname` ve `credspec` uygulama bildirimi aşağıdaki kod parçacığında gösterilir:

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
