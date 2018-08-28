---
title: Azure Data Lake Analytics, birden çok kullanıcı için güvenli hale getirme
description: Azure Data Lake Analytics işlerini çalıştırmak için birden çok kullanıcı yapılandırmayı öğrenin.
ms.service: data-lake-analytics
services: data-lake-analytics
author: matt1883
ms.author: mahi
ms.reviewer: jasonwhowell
ms.topic: conceptual
ms.date: 05/30/2018
ms.openlocfilehash: c6b86e25602f36896855d2593952609904396879
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43051593"
---
# <a name="configure-user-access-to-job-information-to-job-information-in-azure-data-lake-analytics"></a>Azure Data Lake Analytics iş bilgilerini iş bilgileri için kullanıcı erişimini yapılandırma 

Azure Data Lake Analytics işlerini çalıştırmak için birden çok kullanıcı hesabı veya hizmet sorumluları kullanabilirsiniz. 

Ayrıntılı iş bilgilerini görmek bu kullanıcılar için kullanıcıların iş klasörlerinin içeriğini okuyabilir gerekir. İş klasörleri bulunan `/system/` dizin. 

Gerekli izinleri yapılandırılmamışsa, kullanıcı bir hata görebilirsiniz: `Graph data not available - You don't have permissions to access the graph data.` 

## <a name="configure-user-access-to-job-information"></a>İş bilgileri için kullanıcı erişimini yapılandırma

Kullanabileceğiniz **kullanıcı ekleme sihirbazını** klasörlerde ACL'leri yapılandırmak için. Daha fazla bilgi için [yeni kullanıcı ekleme](data-lake-analytics-manage-use-portal.md#add-a-new-user).

Gerekiyorsa daha ayrıntılı bir denetim veya komut dosyası izinleri gerek sonra güvenli klasörleri gibi:

1. GRANT **yürütme** izinleri (ACL erişim) aracılığıyla Kök klasörde:
   - /
   
2. GRANT **yürütme** ve **okuma** iş klasörleri içeren klasörleri (aracılığıyla, bir erişim ACL'si ve varsayılan ACL) izinleri. Örneğin, 25 Mayıs 2018'de çalışan belirli bir işin erişilecek bu klasörleri gerekir:
   - / Sistem
   - / Sistem/jobservice
   - /System/jobservice/Jobs
   - /System/jobservice/Jobs/Usql
   - /System/jobservice/Jobs/Usql/2018
   - /System/jobservice/Jobs/Usql/2018/05
   - /System/jobservice/Jobs/Usql/2018/05/25
   - /System/jobservice/Jobs/Usql/2018/05/25/11
   - /System/jobservice/Jobs/Usql/2018/05/25/11/01
   - Sistem/jobservice/iş/Usql/2018/05/25/11/01/b074bd7a-1448-d879-9d75-f562b101bd3d

## <a name="next-steps"></a>Sonraki adımlar
[Yeni kullanıcı ekleme](data-lake-analytics-manage-use-portal.md#add-a-new-user)
