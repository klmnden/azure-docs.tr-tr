---
title: Yapılandırma erişim - Azure HDInsight küme değişiklikleri
description: Hassas küme yapılandırma alanları erişmek için değişiklikler hakkında bilgi edinin.
author: tylerfox
ms.author: tyfox
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/12/2019
ms.openlocfilehash: 85a6737ee111975d4c7ecf078673280127bfd0fd
ms.sourcegitcommit: 5f348bf7d6cf8e074576c73055e17d7036982ddb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2019
ms.locfileid: "59610250"
---
# <a name="changes-to-cluster-configuration-access"></a>Küme yapılandırması erişimi değişiklikleri

Azure HDInsight SDK'sının en son sürüm, hassas bilgileri almak için daha fazla ayrıntılı rol tabanlı erişimi destekleyen bazı önemli değişiklikler getirir. Bu bölüm, bazı değiştikçe **eylem gerekli** etkilenen yöntemlerden birini kullanmanız durumunda.

## <a name="sdk-for-net-500"></a>.NET 5.0.0 için SDK'sı

- [`ConfigurationOperationsExtensions.Get`](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.hdinsight.configurationsoperationsextensions.get?view=azure-dotnet) olacak **artık dönüş hassas parametreleri** depolama anahtarları (çekirdek-site) veya HTTP kimlik bilgilerini (ağ geçidi) gibi.
    - Hassas parametreleri de dahil olmak üzere tüm yapılandırmaları kullanın `ConfigurationOperationsExtensions.List` ileride.  Not 'Reader' rolüne sahip kullanıcılar bu yöntemi kullanmak mümkün olmayacaktır. Bu küme için hassas bilgileri hangi kullanıcıların erişeceği ayrıntılı denetim sağlar. 
    - Yalnızca HTTP ağ geçidi kimlik bilgilerini almak için kullanın `ClusterOperationsExtensions.GetGatewaySettings`. 
- [`ConfigurationsOperationsExtensions.Update`](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.hdinsight.configurationsoperationsextensions.update?view=azure-dotnet) kullanım dışı bırakıldı ve almıştır `ClusterOperationsExtensions.UpdateGatewaySettings`. 
- [`ConfigurationsOperationsExtensions.EnableHttp`](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.hdinsight.configurationsoperationsextensions.enablehttp?view=azure-dotnet) ve [ `DisableHttp` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.hdinsight.configurationsoperationsextensions.disablehttp?view=azure-dotnet) artık kullanım dışı bırakılmıştır. Bu yöntemler artık gerekmeyen için HTTP artık her zaman etkindir.