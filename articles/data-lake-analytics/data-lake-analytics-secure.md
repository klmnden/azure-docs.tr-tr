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
ms.openlocfilehash: 9fbc94259d6fdfb6758204efd6e6f0a346dc58da
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60813378"
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
   - /system/jobservice/jobs
   - /system/jobservice/jobs/Usql
   - /system/jobservice/jobs/Usql/2018
   - /system/jobservice/jobs/Usql/2018/05
   - /system/jobservice/jobs/Usql/2018/05/25
   - /system/jobservice/jobs/Usql/2018/05/25/11
   - /system/jobservice/jobs/Usql/2018/05/25/11/01
   - /system/jobservice/jobs/Usql/2018/05/25/11/01/b074bd7a-1448-d879-9d75-f562b101bd3d

## <a name="next-steps"></a>Sonraki adımlar
[Yeni kullanıcı ekleme](data-lake-analytics-manage-use-portal.md#add-a-new-user)
