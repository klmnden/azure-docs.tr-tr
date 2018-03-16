---
title: "Azure Service Fabric ortam değişkenleri | Microsoft Docs"
description: "Service Fabric ortam değişkenleri için başvuru belgeleri"
documentationcenter: .net
author: mikkelhegn
manager: msfussell
editor: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/07/2017
ms.author: mikhegn
ms.openlocfilehash: a9faefb43b9d5da81dddef8f326a3867b32842f7
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="service-fabric-environment-variables"></a>Service Fabric ortam değişkenleri

Service Fabric her hizmet örneği için ayarlanmış yerleşik ortam değişkeni yok. Ortam değişkenlerinin tam listesi aşağıdadır:

| Ortam değişkeni                         | Açıklama                                                            | Örnek                                                              |
|----------------------------------------------|------------------------------------------------------------------------|----------------------------------------------------------------------|
| Fabric_ApplicationName                       | Uygulamanın doku URI adı                                 | fabric: / MyApplication                                                |
| Fabric_CodePackageName                       | İşlemin ait olduğu kod paketi adı              | Kod                                                                 |
| Fabric_Endpoint\_IPOrFQDN\_*ServiceEndpointName*     | IP adresi veya FQDN bitiş noktası                                 | 10.0.0.1                                                     |
| Fabric\_Endpoint\_*ServiceEndpointName*              | Uç noktası için bağlantı noktası numarası                                  | 8234                                                                 |
| Fabric_Folder_App_Log                        | Günlük klasörü                                                             | C:\\\\Data\\\\_App\\\\_Node_0\\\\MyApplicationType_App12\\\\log      |
| Fabric_Folder_App_Temp                       | Geçici klasörü                                                            | C:\\\\Data\\\\_App\\\\_Node_0\\\\MyApplicationType_App12\\\\temp     |
| Fabric_Folder_App_Work                       | Çalışma klasörü                                                            | C:\\\\Data\\\\_App\\\\_Node_0\\\\MyApplicationType_App12\\\\work     |
| Fabric_Folder_Application                    | Uygulamalar giriş klasörü                                           | C:\\\\Data\\\\_App\\\\_Node_0\\\\MyApplicationType_App12             |
| Fabric_IsContainerHost                       | İşlem bir kapsayıcı olup olmadığını belirten bir Boole                   | false                                                                |
| Fabric_NodeId                                | İşlem çalışan düğümünün düğüm kimliği                            | bf865279ba277deb864a976fbf4c200e                                     |
| Fabric_NodeIPOrFQDN                          | IP veya FQDN belirtildiği gibi kümedeki bir düğümün dosya bildirimi. | localhost veya 10.0.0.1                                                |
| Fabric_NodeName                              | İşlem çalışan düğümünün düğüm adı                          | _Node_0                                                              |
| Fabric_ServiceName                           | Hizmet ExclusiveProcess modunda barındırılıyorsa hizmetin adıdır. Bu değişken değeri, yalnızca hizmet ServicePackageActivationMode ExclusiveProcess ile oluşturursanız, kullanılabilir.  | MyService                                               |
| Fabric_ServicePackageActivationId            | The ServicePackageActivationId                                         | A GUID                                                               |
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