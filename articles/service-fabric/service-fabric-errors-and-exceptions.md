---
title: Sık karşılaşılan FabricClient özel durumları durum | Microsoft Docs
description: Sık karşılaşılan özel durumlar ve FabricClient API'leri tarafından uygulama ve küme yönetim işlemleri gerçekleştirilirken muhtemel hataları açıklanır.
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
ms.openlocfilehash: 9e29f05c71f9dfe0bcd79135deb30d713fb3abb0
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54477111"
---
# <a name="common-exceptions-and-errors-when-working-with-the-fabricclient-apis"></a>Sık karşılaşılan özel durumlar ve FabricClient API'leri ile çalışırken hataları
[FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) API'leri bir Service Fabric uygulama, hizmet veya küme yönetim görevlerini gerçekleştirmek küme ve uygulama yöneticileri etkinleştirin. Örneğin, uygulama dağıtımı, yükseltme ve kaldırma, bir küme sistem durumu denetimi ya da bir hizmeti sınama. FabricClient API'leri, uygulama geliştiriciler ve küme yöneticileri Service Fabric kümesi ve uygulamaları yönetmek için Araçlar geliştirmeniz için kullanabilirsiniz.

Farklı türlerde FabricClient kullanarak gerçekleştirilen işlemler vardır.  Her yöntem hataları hatalı giriş nedeniyle, çalışma zamanı hataları veya geçici altyapı sorunları için özel durumlar.  Belirli bir yöntemle özel hangi durumlar bulmak için API başvuru belgelerine bakın. Birçok tarafından oluşturulan bazı özel durumlar, ancak farklı [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) API'leri. Aşağıdaki tabloda FabricClient API'leri arasında ortak olan özel durumlar listelenir.

| Özel durum | Zaman oluşturulur |
| --- |:--- |
| [System.Fabric.FabricObjectClosedException](https://docs.microsoft.com/dotnet/api/system.fabric.fabricobjectclosedexception#System_Fabric_FabricObjectClosedException) |[FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) nesnedir kapalı durumda. Elden [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) kullanıyorsanız ve yeni bir nesne [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) nesne. |
| [System.TimeoutException](https://docs.microsoft.com/dotnet/core/api/system.timeoutexception#System_TimeoutException) |İşlem zaman aşımına uğradı. [OperationTimedOut](https://docs.microsoft.com/dotnet/api/system.fabric.fabricerrorcode#System_Fabric_FabricErrorCode) işlemi MaxOperationTimeout tamamlanması uzun sürerse döndürülür. |
| [System.UnauthorizedAccessException](https://docs.microsoft.com/dotnet/core/api/system.unauthorizedaccessexception#System_UnauthorizedAccessException) |İşlem için erişim denetimi başarısız oldu. E_ACCESSDENIED döndürülür. |
| [System.Fabric.FabricException](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception#System_Fabric_FabricException) |İşlem gerçekleştirilirken bir çalışma zamanı hatası oluştu. FabricClient yöntemlerden herhangi birini potansiyel olarak oluşturabilecek [FabricException](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception#System_Fabric_FabricException), [ErrorCode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception#System_Fabric_FabricException_ErrorCode) özelliği, özel durum tam nedenini gösterir. Hata kodları tanımlanmış [FabricErrorCode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricerrorcode#System_Fabric_FabricErrorCode) sabit listesi. |
| [System.Fabric.FabricTransientException](https://docs.microsoft.com/dotnet/api/system.fabric.fabrictransientexception#System_Fabric_FabricTransientException) |İşlem, bir tür bir geçici hata durumu nedeniyle başarısız oldu. Örneğin, bir çekirdeği geçici olarak erişilebilir olmadığı için işlem başarısız olabilir. Geçici özel durumlar denenebilecek başarısız işlemler karşılık gelir. |

Bazı ortak [FabricErrorCode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricerrorcode#System_Fabric_FabricErrorCode) içinde döndürülen hatalar bir [FabricException](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception#System_Fabric_FabricException):

| Hata | Koşul |
| --- |:--- |
| CommunicationError |Bir iletişim hatası, işlemin başarısız, işlemi yeniden deneyin neden oldu. |
| InvalidCredentialType |Kimlik bilgisi türü geçersiz. |
| InvalidX509FindType |X509FindType geçersiz. |
| InvalidX509StoreLocation |X509 deposu konumu geçersiz. |
| InvalidX509StoreName |X509 depo adı geçersiz. |
| InvalidX509Thumbprint |X509 sertifika parmak izi dizesi geçersiz. |
| InvalidProtectionLevel |Koruma düzeyi geçersiz. |
| InvalidX509Store |Sertifika deposu açılamıyor X509. |
| InvalidSubjectName |Konu adı geçersiz. |
| InvalidAllowedCommonNameList |Ortak ad liste dizesinin biçimi geçersiz. Virgülle ayrılmış bir liste olmalıdır. |

