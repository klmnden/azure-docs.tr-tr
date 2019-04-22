---
title: Azure Service Fabric küme sürümleri hakkında bilgi edinin | Microsoft Docs
description: Desteklenen Azure Service Fabric küme sürümleri
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chakdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: troubleshooting
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 9/24/2018
ms.author: aljo
ms.openlocfilehash: d99000e1682b662f4bceb28096395243c894282f
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59681616"
---
# <a name="supported-service-fabric-versions"></a>Service Fabric desteklenen sürümler

Kümenizi, desteklenen bir Service Fabric sürümü her zaman çalışır durumda olduğundan emin olun. Gibi ve biz Service Fabric yeni bir sürümünü duyurmaktan zaman önceki sürümü en az 60 gün o tarihten sonra destek sonu için işaretlenir. Yeni yayınlar duyurulan [Service Fabric Ekibi blogunda](https://blogs.msdn.microsoft.com/azureservicefabric/).

Desteklenen bir Service Fabric sürümünü çalıştırıyor kümenizin tutmak hakkında ayrıntılı bilgi aşağıdaki belgelere bakın.

- [Bir Azure kümesinde Service Fabric sürümüne yükseltin](service-fabric-cluster-upgrade.md)
- [Bir tek başına windows server kümesinde Service Fabric sürümüne yükseltin](service-fabric-cluster-upgrade-windows-server.md)

## <a name="supported-versions"></a>Desteklenen sürümler

Aşağıdaki tabloda, desteklenen bir Service Fabric sürümleri ve Destek bitiş tarihlerinin listeler.

| **Kümedeki Service Fabric çalışma zamanı** | **Doğrudan küme sürümünden yükseltme yapabilirsiniz** |**Uyumlu SDK / NuGet paket sürümleri** | **Destek sonu tarihi** |
| --- | --- |--- | --- |
| 5.3.121 önce tüm küme sürümleri | 5.1.158* |2.3 sürümü küçüktür veya eşittir |20 Ocak 2017 |
| 5.3.* | 5.1.158.* |2.3 sürümü küçüktür veya eşittir |24 Şubat 2017 |
| 5.4.* | 5.1.158.* |Sürüm 2.4 küçüktür veya eşittir |10,2017 olabilir       |
| 5.5.* | 5.4.164.* |Sürüm 2.5 küçüktür veya eşittir |Ağustos 10,2017    |
| 5.6.* | 5.4.164.* |Sürüm 2.6 küçüktür veya eşittir |Ekim 13,2017   |
| 5.7.* | 5.4.164.* |Sürüm 2.7 küçüktür veya eşittir |Aralık 15,2017  |
| 6.0.* | 5.6.205.* |Sürüm 2.8 küçüktür veya eşittir |30,2018 Mart     |
| 6.1.* | 5.7.221.* |Sürüm 3.0 küçüktür veya eşittir |Temmuz 15,2018      |
| 6.2.* | 6.0.232.* |Sürüm 3.1 küçüktür veya eşittir |Ekim 26,2018   |
| 6.3.* | 6.1.480.* |Sürüm 3.2 küçüktür veya eşittir |31,2019 Mart  |
| 6.4.* | 6.2.301.* |Sürüm 3.3 küçüktür veya eşittir |Geçerli sürümü ve bu nedenle bitiş tarihi |

## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

 Aşağıdaki tabloda, desteklenen bir Service Fabric sürümleri için desteklenen işletim sistemleri listelenmektedir.

| **İşletim Sistemi** | **En erken desteklenen Service Fabric sürümü** |
| --- | --- |
| Windows Server 2012 R2 | Tüm sürümler |
| Windows Server 2016 | Tüm sürümler |
| Windows Server 1709 | 6.0 |
| Windows Server 1803 | 6.4 |
| Windows Server 1809 | 6.4.654.9590 |
| Windows Server 2019 | 6.4.654.9590 |
| Linux Ubuntu 16.04 | 6.0 |

