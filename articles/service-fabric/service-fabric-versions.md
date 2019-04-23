---
title: Azure Service Fabric'te desteklenen küme sürümleri | Microsoft Docs
description: Azure Service fabric'te küme sürümleri hakkında bilgi edinin.
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
ms.openlocfilehash: 75e95737eecb9407a80103d1cad00d4987fe7091
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59998830"
---
# <a name="supported-service-fabric-versions"></a>Service Fabric desteklenen sürümler

Kümenizi desteklenen bir Azure Service Fabric sürümü her zaman çalışır durumda olduğundan emin olun. En az 60 gün size yeni bir Service Fabric, önceki sürümü için destek sürümünü duyurmaktan sonra sona erer. Yeni sürümler duyuruları bulacaksınız [Service Fabric ekibi blogu](https://blogs.msdn.microsoft.com/azureservicefabric/).

Desteklenen bir Service Fabric sürümünü çalıştırıyor kümenizin nasıl hakkında ayrıntılı bilgi için aşağıdaki belgelere bakın:

- [Bir Azure Service Fabric kümesini yükseltme](service-fabric-cluster-upgrade.md)
- [Tek başına Windows Server kümenizde çalışan Service Fabric sürümüne yükseltin](service-fabric-cluster-upgrade-windows-server.md)

## <a name="supported-versions"></a>Desteklenen sürümler

Aşağıdaki tablo, Service Fabric ve Destek bitiş tarihlerinin sürümlerini listeler.

| Kümedeki Service Fabric çalışma zamanı | Doğrudan küme sürümünden yükseltme yapabilirsiniz |Uyumlu SDK veya NuGet Paket sürümü | Destek sonu |
| --- | --- |--- | --- |
| 5.3.121 önce tüm küme sürümleri | 5.1.158.* |2.3 sürümü küçüktür veya eşittir |20 Ocak 2017 |
| 5.3.* | 5.1.158.* |2.3 sürümü küçüktür veya eşittir |24 Şubat 2017 |
| 5.4.* | 5.1.158.* |Sürüm 2.4 küçüktür veya eşittir |10 Mayıs 2017       |
| 5.5.* | 5.4.164.* |Sürüm 2.5 küçüktür veya eşittir |Ağustos 10,2017    |
| 5.6.* | 5.4.164.* |Sürüm 2.6 küçüktür veya eşittir |Ekim 13,2017   |
| 5.7.* | 5.4.164.* |Sürüm 2.7 küçüktür veya eşittir |15 Aralık 2017  |
| 6.0.* | 5.6.205.* |Sürüm 2.8 küçüktür veya eşittir |30 Mart 2018     |
| 6.1.* | 5.7.221.* |Sürüm 3.0 küçüktür veya eşittir |15 Temmuz 2018      |
| 6.2.* | 6.0.232.* |Sürüm 3.1 küçüktür veya eşittir |26 Ekim 2018   |
| 6.3.* | 6.1.480.* |Sürüm 3.2 küçüktür veya eşittir |31 Mart 2019  |
| 6.4.* | 6.2.301.* |Sürüm 3.3 küçüktür veya eşittir |Geçerli sürümü, bu nedenle bitiş tarihi |

## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

Aşağıdaki tabloda, desteklenen bir Service Fabric sürümleri için desteklenen işletim sistemleri listelenmektedir.

| İşletim sistemi | En erken desteklenen Service Fabric sürümü |
| --- | --- |
| Windows Server 2012 R2 | Tüm sürümler |
| Windows Server 2016 | Tüm sürümler |
| Windows Server 1709 | 6.0 |
| Windows Server 1803 | 6.4 |
| Windows Server 1809 | 6.4.654.9590 |
| Windows Server 2019 | 6.4.654.9590 |
| Linux Ubuntu 16.04 | 6.0 |

