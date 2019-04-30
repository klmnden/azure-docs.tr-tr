---
title: Azure Service Fabric hizmet uç noktaları için erişim ilkeleri atama | Microsoft Docs
description: Güvenlik atama hakkında bilgi edinin, Service Fabric hizmeti, HTTP veya HTTPS uç noktaları için erişim ilkeleri.
services: service-fabric
documentationcenter: .net
author: athinanthny
manager: chackdan
editor: ''
ms.assetid: 4242a1eb-a237-459b-afbf-1e06cfa72732
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/21/2018
ms.author: atsenthi
ms.openlocfilehash: 3e892e443f5e3309add48f939f26ba14eaf5a51b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60614198"
---
# <a name="assign-a-security-access-policy-for-http-and-https-endpoints"></a>HTTP ve HTTPS Uç noktalara yönelik güvenlik erişim ilkesi atama
Bir farklı Çalıştırma İlkesi uygulama ve hizmet bildirimi HTTP uç noktası kaynakları bildirir, belirtmelisiniz bir **SecurityAccessPolicy**.  **SecurityAccessPolicy** Bu uç noktaları için ayrılan bağlantı noktaları olarak hizmetini çalıştıran kullanıcı hesabının doğru şekilde kısıtlanmıştır sağlar. Aksi takdirde, **http.sys** Erişim hizmetine sahip değil ve istemciden çağrıları hataları alırsınız. Aşağıdaki örnekte adlı bir uç nokta Customer1 hesabı uygulanır **Uçnoktaadı**, sağlayan, tam erişim hakları.

```xml
<Policies>
  <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
  <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
  <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
</Policies>
```

Bir HTTPS uç noktası için aynı zamanda istemciye döndürmek için sertifika adını belirtir. Sertifikayı kullanarak başvuru **EndpointBindingPolicy**.  Sertifika tanımlanan **sertifikaları** uygulama bildiriminin.

```xml
<Policies>
  <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
  <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
  <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
  <!--EndpointBindingPolicy is needed if the EndpointName is secured with https -->
  <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
</Policies>
```

> [!WARNING] 
> HTTPS kullanırken, aynı bağlantı noktası ve aynı düğüme dağıtılan sertifika farklı hizmet örnekleri (uygulamayı bağımsız olarak) için kullanmayın. Farklı uygulama örneklerinin aynı bağlantı noktası kullanarak iki farklı hizmet yükseltme bir yükseltme hatasına neden olur. Daha fazla bilgi için [HTTPS uç noktaları ile birden çok uygulama yükseltme ](service-fabric-application-upgrade.md#upgrading-multiple-applications-with-https-endpoints).
> 

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
Sonraki adımlar için bu makaleleri okuyun:
* [Uygulama modelini anlama](service-fabric-application-model.md)
* [Bir hizmet bildiriminde kaynakları belirtme](service-fabric-service-manifest-resources.md)
* [Uygulama dağıtma](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
