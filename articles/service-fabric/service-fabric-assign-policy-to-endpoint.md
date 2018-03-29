---
title: Erişim ilkeleri Azure Service Fabric hizmet uç noktalarına Ata | Microsoft Docs
description: Güvenlik atama hakkında bilgi edinin erişim ilkeleri HTTP veya HTTPS Uç noktalara Service Fabric hizmetinizde.
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: ''
ms.assetid: 4242a1eb-a237-459b-afbf-1e06cfa72732
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/21/2018
ms.author: mfussell
ms.openlocfilehash: 65f47d238d4e591ddde8937d0eb3c55931c01c3f
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="assign-a-security-access-policy-for-http-and-https-endpoints"></a>HTTP ve HTTPS uç noktaları için bir güvenlik erişim ilkesi atama
Bir Çalıştır ilkesi uygulamak ve hizmet bildirimi HTTP uç noktası kaynakları bildirir, belirtmelisiniz bir **SecurityAccessPolicy**.  **SecurityAccessPolicy** Bu uç noktaları için ayrılmış bağlantı noktaları olarak hizmet çalıştıran kullanıcı hesabı için doğru şekilde kısıtlanır sağlar. Aksi takdirde, **http.sys** hizmetine erişiminiz yok ve istemciden çağrıları hatalarıyla alın. Aşağıdaki örnek, adlı bir uç nokta Customer1 hesabı uygular **EndpointName**, sağlayan, tam erişim hakları.

```xml
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
   <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
</Policies>
```

Bir HTTPS uç noktası için aynı zamanda istemciye döndürmek için sertifika adını belirtir. Sertifika kullanarak başvuru **EndpointBindingPolicy**.  Sertifika tanımlanan **sertifikaları** uygulama bildirimi bölümü.

```xml
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
  <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
  <!--EndpointBindingPolicy is needed if the EndpointName is secured with https -->
  <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
</Policies
```

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
Sonraki adımlar için aşağıdaki makaleler okuyun:
* [Uygulama modeli anlama](service-fabric-application-model.md)
* [Bir hizmet bildirimi kaynakları belirtme](service-fabric-service-manifest-resources.md)
* [Bir uygulamayı dağıtma](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
