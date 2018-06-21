---
title: Oluşturulan ortak FabricClient özel durumları | Microsoft Docs
description: Genel özel durumlar ve FabricClient API'ları, uygulama ve küme yönetimi işlemleri gerçekleştirirken atılabilen hataları açıklar.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: bb821313-b221-479f-b08e-36cf07e60a07
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/20/2018
ms.author: ryanwi
ms.openlocfilehash: e854ed42b6af8bc090950e8399e3229e202a2ed0
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36293421"
---
# <a name="common-exceptions-and-errors-when-working-with-the-fabricclient-apis"></a>Genel özel durumlar ve FabricClient API'leri ile çalışırken hataları
[FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient#System_Fabric_FabricClient) API'leri yöneticilerin Service Fabric uygulama, hizmet veya küme üzerinde yönetim görevlerini gerçekleştirmek küme ve uygulama. Örneğin, uygulama dağıtımı, yükseltme ve kaldırma, bir küme durumu denetleniyor veya bir hizmet test etme. Uygulama geliştiricilerinin ve küme yöneticileri FabricClient API'ları Service Fabric kümesi ve uygulamaları yönetmek için Araçlar geliştirmek için kullanabilirsiniz.

Farklı türlerde FabricClient kullanılarak gerçekleştirilen işlemler vardır.  Her yöntem hataları hatalı giriş nedeniyle, çalışma zamanı hataları veya geçici altyapı sorunlarını istisnalar atabilirsiniz.  Belirli bir yöntemle hangi özel durumlar bulmak için API Başvurusu belgelerine bakın. Çoğu tarafından oluşturulan bazı özel durumlar, ancak, farklı [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient#System_Fabric_FabricClient) API'leri. Aşağıdaki tabloda FabricClient API'ları arasında ortak olan özel durumlar listelenir.

| Özel durum | Ne zaman oluşturulur |
| --- |:--- |
| [System.Fabric.FabricObjectClosedException](https://docs.microsoft.com/dotnet/api/system.fabric.fabricobjectclosedexception#System_Fabric_FabricObjectClosedException) |[FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient#System_Fabric_FabricClient) nesne kapalı durumda değil. Elden [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient#System_Fabric_FabricClient) nesne kullanıyorsanız ve yeni bir örneğini [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient#System_Fabric_FabricClient) nesnesi. |
| [System.TimeoutException](https://docs.microsoft.com/dotnet/core/api/system.timeoutexception#System_TimeoutException) |İşlem zaman aşımına uğradı. [OperationTimedOut](https://docs.microsoft.com/dotnet/api/system.fabric.fabricerrorcode#System_Fabric_FabricErrorCode) işlemi tamamlamak için MaxOperationTimeout birden yönlendirdiğinde döndürülür. |
| [System.UnauthorizedAccessException](https://docs.microsoft.com/dotnet/core/api/system.unauthorizedaccessexception#System_UnauthorizedAccessException) |İşlem için erişim denetimi başarısız oldu. E_ACCESSDENIED döndürülür. |
| [System.Fabric.FabricException](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception#System_Fabric_FabricException) |İşlem gerçekleştirilirken bir çalışma zamanı hatası oluştu. FabricClient yöntemlerden herhangi birini olası atabilirsiniz [FabricException](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception#System_Fabric_FabricException), [ErrorCode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception#System_Fabric_FabricException_ErrorCode) özelliği, özel durum tam nedenini gösterir. Hata kodları tanımlanmış [FabricErrorCode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricerrorcode#System_Fabric_FabricErrorCode) numaralandırması. |
| [System.Fabric.FabricTransientException](https://docs.microsoft.com/dotnet/api/system.fabric.fabrictransientexception#System_Fabric_FabricTransientException) |İşlem bir tür geçici hata durumu nedeniyle başarısız oldu. Örneğin, bir çekirdek çoğaltmalarının geçici olarak erişilemediği için bir işlem başarısız olabilir. Geçici özel durumlar yeniden denenebilir başarısız işlemler için karşılık gelir. |

Bazı ortak [FabricErrorCode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricerrorcode#System_Fabric_FabricErrorCode) içinde döndürülen hatalar bir [FabricException](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception#System_Fabric_FabricException):

| Hata | Koşul |
| --- |:--- |
| CommunicationError |Bir iletişim hatası işlemin başarısız, işlemi yeniden denemek neden oldu. |
| InvalidCredentialType |Kimlik bilgisi türü geçersiz. |
| InvalidX509FindType |X509FindType geçersiz. |
| InvalidX509StoreLocation |X509 depolama konumu geçersiz. |
| InvalidX509StoreName |X509 depo adı geçersiz. |
| InvalidX509Thumbprint |X509 sertifika parmak izi dizesi geçersiz. |
| InvalidProtectionLevel |Koruma düzeyi geçersiz. |
| InvalidX509Store |Sertifika deposu açılamıyor X509. |
| InvalidSubjectName |Konu adı geçersiz. |
| InvalidAllowedCommonNameList |Ortak ad liste dizesinin biçimi geçersiz. Virgülle ayrılmış bir liste olması gerekir. |

