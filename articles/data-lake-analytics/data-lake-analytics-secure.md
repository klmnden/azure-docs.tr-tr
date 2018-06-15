---
title: Azure Data Lake Analytics birden çok kullanıcı için güvenli
description: Azure Data Lake Analytics işlerini çalıştırmak için birden çok kullanıcı yapılandırmayı öğrenin.
ms.service: data-lake-analytics
services: data-lake-analytics
author: matt1883
ms.author: mahi
manager: kfile
editor: jasonwhowell
ms.topic: conceptual
ms.date: 05/30/2018
ms.openlocfilehash: 1d92e6d0e619584dedcbc9a66450c25dd1ac8b75
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34701422"
---
# <a name="configure-user-access-to-job-information-to-job-information-in-azure-data-lake-analytics"></a>Azure Data Lake Analytics işi bilgileri iş bilgilere kullanıcı erişimi yapılandırma 

Azure Data Lake Analytics işlerini çalıştırmak için birden çok hizmet asıl adı veya kullanıcı hesaplarını kullanabilirsiniz. 

Ayrıntılı iş bilgilerini görmek aynı olan kullanıcılar için kullanıcılar iş klasörlerinin içeriğini okumak gerekir. İş klasörleri bulunur `/system/` dizin. 

Gerekli izinleri yapılandırılmamışsa, kullanıcı bir hata görebilirsiniz: `Graph data not available - You don't have permissions to access the graph data.` 

## <a name="configure-user-access-to-job-information"></a>İş bilgileri kullanıcı erişimi yapılandırma

Kullanabileceğiniz **Kullanıcı Ekleme Sihirbazı** klasörlerde ACL'leri yapılandırmak için. Daha fazla bilgi için bkz: [yeni bir kullanıcı eklemek](data-lake-analytics-manage-use-portal.md#add-a-new-user).

Gerekiyorsa daha ayrıntılı bir denetim veya komut dosyası izinleri gerek sonra güvenli klasörler gibi:

1. GRANT **yürütme** kök klasördeki izinleri (aracılığıyla ACL erişimi):
   - /
   
2. GRANT **yürütme** ve **okuma** iş klasörleri içeren klasörlerin (ACL access ve varsayılan bir ACL) üzerinden izinleri. Örneğin, 25 May 2018 üzerinde çalışan belirli bir iş için bu klasörleri erişilmesi gerekir:
   - / Sistem
   - / Sistem/jobservice
   - /System/jobservice/Jobs
   - /System/jobservice/Jobs/Usql
   - /System/jobservice/Jobs/Usql/2018
   - /System/jobservice/Jobs/Usql/2018/05
   - /System/jobservice/Jobs/Usql/2018/05/25
   - /System/jobservice/Jobs/Usql/2018/05/25/11
   - /System/jobservice/Jobs/Usql/2018/05/25/11/01
   - Sistem/jobservice/işleri/Usql/2018/05/25/11/01/b074bd7a-1448-d879-9d75-f562b101bd3d

## <a name="next-steps"></a>Sonraki adımlar
[Yeni kullanıcı ekleme](data-lake-analytics-manage-use-portal.md#add-a-new-user)
