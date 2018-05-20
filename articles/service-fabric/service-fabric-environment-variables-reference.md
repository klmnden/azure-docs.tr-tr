---
title: Azure Service Fabric ortam değişkenleri | Microsoft Docs
description: Service Fabric ortam değişkenleri için başvuru belgeleri
documentationcenter: .net
author: mikkelhegn
manager: msfussell
editor: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: reference
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/07/2017
ms.author: mikhegn
ms.openlocfilehash: f7c36fec7ff58c225e41899e8264ca1dde95ce7c
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="service-fabric-environment-variables"></a>Service Fabric ortam değişkenleri

Service Fabric her hizmet örneği için ayarlanmış yerleşik ortam değişkeni yok. Ortam değişkenlerinin tam listesi aşağıdadır:

| Ortam değişkeni                         | Açıklama                                                            | Örnek                                                              |
|----------------------------------------------|------------------------------------------------------------------------|----------------------------------------------------------------------|
| Fabric_ApplicationName                       | Uygulamanın doku URI adı                                 | fabric: / MyApplication                                                |
| Fabric_CodePackageName                       | İşlemin ait olduğu kod paketi adı              | Kod                                                                 |
| Fabric_Endpoint\_IPOrFQDN\_*ServiceEndpointName*     | IP adresi veya FQDN bitiş noktası                                 | 10.0.0.1                                                     |
| Doku\_Endpoint\_*ServiceEndpointName*              | Uç noktası için bağlantı noktası numarası                                  | 8234                                                                 |
| Fabric_Folder_App_Log                        | Günlük klasörü                                                             | C:\\\\veri\\\\_uygulama\\\\_Node_0\\\\MyApplicationType_App12\\\\günlük      |
| Fabric_Folder_App_Temp                       | Geçici klasörü                                                            | C:\\\\veri\\\\_uygulama\\\\_Node_0\\\\MyApplicationType_App12\\\\temp     |
| Fabric_Folder_App_Work                       | Çalışma klasörü                                                            | C:\\\\veri\\\\_uygulama\\\\_Node_0\\\\MyApplicationType_App12\\\\çalışma     |
| Fabric_Folder_Application                    | Uygulamalar giriş klasörü                                           | C:\\\\veri\\\\_uygulama\\\\_Node_0\\\\MyApplicationType_App12             |
| Fabric_IsContainerHost                       | İşlem bir kapsayıcı olup olmadığını belirten bir Boole                   | false                                                                |
| Fabric_NodeId                                | İşlem çalışan düğümünün düğüm kimliği                            | bf865279ba277deb864a976fbf4c200e                                     |
| Fabric_NodeIPOrFQDN                          | IP veya FQDN belirtildiği gibi kümedeki bir düğümün dosya bildirimi. | localhost veya 10.0.0.1                                                |
| Fabric_NodeName                              | İşlem çalışan düğümünün düğüm adı                          | _Node_0                                                              |
| Fabric_ServiceName                           | Hizmet ExclusiveProcess modunda barındırılıyorsa hizmetin adıdır. Bu değişken değeri, yalnızca hizmet ServicePackageActivationMode ExclusiveProcess ile oluşturursanız, kullanılabilir.  | MyService                                               |
| Fabric_ServicePackageActivationId            | ServicePackageActivationId                                         | BİR GUID                                                               |
| Fabric_ServicePackageName                    | Hizmet paketi işlem adını parçasıdır                     | Web1Pkg                                                              |

Service Fabric çalışma zamanı tarafından kullanılan iç ortam değişkeni:

- Fabric_ApplicationHostId
- Fabric_ApplicationHostType
- Fabric_ApplicationId
- Fabric_CodePackageInstanceId
- Fabric_CodePackageInstanceSeqNum
- Fabric_RuntimeConnectionAddress
- Fabric_ServicePackageActivationGuid
- Fabric_ServicePackageInstanceId
- Fabric_ServicePackageInstanceSeqNum
- Fabric_ServicePackageVersionInstance
- FabricActivatorAddress
- FabricPackageFileName
- HostedServiceName