---
title: Azure Service Fabric ortam değişkenlerini | Microsoft Docs
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
ms.openlocfilehash: 1c8400898dba59f312ba9d994ee711a5e241973a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60946731"
---
# <a name="service-fabric-environment-variables"></a>Service Fabric ortam değişkenleri

Service Fabric kümesi her hizmet örneği için bir yerleşik ortam değişkenleri vardır. Ortam değişkenlerinin tam listesini aşağıda verilmiştir:

| Ortam değişkeni                         | Açıklama                                                            | Örnek                                                              |
|----------------------------------------------|------------------------------------------------------------------------|----------------------------------------------------------------------|
| Fabric_ApplicationName                       | Uygulamanın yapı URI'si adı                                 | fabric: / Uygulamam                                                |
| Fabric_CodePackageName                       | İşlem ait olduğu kod paketi adı              | Kod                                                                 |
| Fabric_Endpoint\_IPOrFQDN\_*ServiceEndpointName*     | IP adresi veya FQDN uç noktası                                 | 10.0.0.1                                                     |
| Fabric\_Endpoint\_*ServiceEndpointName*              | Uç noktası için bağlantı noktası numarası                                  | 8234                                                                 |
| Fabric_Folder_App_Log                        | Günlük klasörü                                                             | C:\\\\veri\\\\_uygulama\\\\_Node_0\\\\MyApplicationType_App12\\\\günlüğü      |
| Fabric_Folder_App_Temp                       | Geçici klasörü                                                            | C:\\\\veri\\\\_uygulama\\\\_Node_0\\\\MyApplicationType_App12\\\\temp     |
| Fabric_Folder_App_Work                       | Çalışma klasörü                                                            | C:\\\\veri\\\\_uygulama\\\\_Node_0\\\\MyApplicationType_App12\\\\çalışma     |
| Fabric_Folder_Application                    | Uygulamalar giriş klasörü                                           | C:\\\\veri\\\\_uygulama\\\\_Node_0\\\\MyApplicationType_App12             |
| Fabric_IsContainerHost                       | İşlem, bir kapsayıcı olup olmadığını belirten bir Boole                   | false                                                                |
| Fabric_NodeId                                | İşlem çalışan düğümünün düğüm kimliği                            | bf865279ba277deb864a976fbf4c200e                                     |
| Fabric_NodeIPOrFQDN                          | IP veya belirtilen küme düğümünün bildirim dosyası. | localhost veya 10.0.0.1                                                |
| Fabric_NodeName                              | İşlem çalışan düğümünün düğüm adı                          | _Node_0                                                              |
| Fabric_ServiceName                           | Hizmet ExclusiveProcess modunda barındırılıyorsa service fabric URI adı. Bu değişken değeri, yalnızca hizmeti ile ServicePackageActivationMode ExclusiveProcess oluşturursanız kullanılabilir.  | fabric: / Uygulamam/Hizmetim                                               |
| Fabric_ServicePackageActivationId            | ServicePackageActivationId                                         | BİR GUID                                                               |
| Fabric_ServicePackageName                    | Hizmet paketi işlem adını bir parçasıdır                     | Web1Pkg                                                              |

Service Fabric çalışma zamanı tarafından kullanılan iç ortam değişkenleri:

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
