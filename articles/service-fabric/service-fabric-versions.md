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
ms.openlocfilehash: 961904c6988596a5ba73940a154d96856636a0db
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65150908"
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

## <a name="supported-version-names"></a>Desteklenen sürüm adları

Aşağıdaki tabloda, Service Fabric ve bunların karşılık gelen sürüm numaralarını sürüm adlarını listeler.

| Sürüm adı | Windows sürüm numarası | Linux sürüm numarası |
| --- | --- | --- |
| 5.3 RTO | 5.3.121.9494 | NA |
| 5.3 CU1 | 5.3.204.9494 | NA |
| 5.3 CU2 | 5.3.301.9590 | NA |
| 5.3 CU3 | 5.3.311.9590 | NA |
| 5.4 CU2 | 5.4.164.9494 | NA |
| 5.5 CU1 | 5.5.216.0    | NA |
| 5.5 CU2 | 5.5.219.0    | NA |
| 5.5 CU3 | 5.5.227.0    | NA |
| 5.5 CU4 | 5.5.232.0    | NA |
| 5.6 RTO | 5.6.204.9494 | NA |
| 5.6 CU2 | 5.6.210.9494 | NA |
| 5.6 CU3 | 5.6.220.9494 | NA |
| 5.7 RTO | 5.7.198.9494 | NA |
| 5.7 CU4 | 5.7.221.9494 | NA |
| 6.0 RTO | 6.0.211.9494 | 6.0.120.1 |
| 6.0 CU1 | 6.0.219.9494 | 6.0.127.1 |
| 6.0 CU2 | 6.0.232.9494 | 6.0.133.1 |
| 6.1 CU1 | 6.1.456.9494 | 6.1.183.1 |
| 6.1 CU2 | 6.1.467.9494 | 6.1.185.1 |
| 6.1 CU3 | 6.1.472.9494 | NA |
| 6.1 CU4 | 6.1.480.9494 | 6.1.187.1 |
| 6.2 RTO | 6.2.269.9494 | 6.2.184.1 | 
| 6.2 CU1 | 6.2.274.9494 | 6.2.191.1 |
| 6.2 CU2 | 6.2.283.9494 | 6.2.194.1 |
| 6.2 CU3 | 6.2.301.9494 | 6.2.199.1 |
| 6.3 RTO | 6.3.162.9494 | 6.3.119.1 |
| 6.3 CU1 | 6.3.176.9494 | 6.3.124.1 |
| 6.3 CU1 | 6.3.187.9494 | 6.3.129.1 |
| 6.4 RTO | 6.4.617.9590 | 6.4.625.1 |
| 6.4 CU2 | 6.4.622.9590 | NA |
| 6.4 CU3 | 6.4.637.9590 | 6.4.634.1 |
| 6.4 CU4 | 6.4.644.9590 | 6.4.639.1 |
| 6.4 CU5 | 6.4.654.9590 | 6.4.649.1 |
