---
title: include dosyası
description: include dosyası
services: digital-twins
author: kingdomofends
ms.service: digital-twins
ms.topic: include
ms.date: 12/28/2018
ms.author: adgera
ms.custom: include file
ms.openlocfilehash: e81b8fb06240d566e46ca0b45a0910e014dee329
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66119020"
---
Rol tabanlı erişim denetimi, erişim izinleri ve rolleri yönetmek için bir devralma temelli güvenlik stratejisidir. Bloğun rolleri üst rollerden izinleri devralır. Üst rolden devralınan olmadan izinler de atanabilir. Bir rol gerektiği şekilde özelleştirmek için de atanabilir.

Örneğin, bir alanı yöneticisi, belirtilen bir alan için tüm işlemleri çalıştırmak için küresel erişim gerekebilir. Tüm düğümleri altında veya alanı içinde erişim içerir. Cihaz yükleyici yalnızca ihtiyaç duyabilirsiniz *okuma* ve *güncelleştirme* cihazlardan ve sensörlerden izinleri.

Her durumda da, rolleri verilen *en fazla ve tam olarak gerekli erişim* ilkesine en düşük öncelik ilkesini başına görevlerini yerine getirmek için. Bu ilkeye göre bir kimlik verilir *yalnızca*:

* İşini tamamlamak için gereken erişim miktarı.
* Rol uygun ve kendi işlemi gerçekleştirirken sınırlıdır.

>[!IMPORTANT]
> Her zaman ilkesine en düşük öncelik ilkesini uygulayın.

İzlemek için iki diğer önemli rol tabanlı erişim denetimi uygulamalar:

> [!div class="checklist"]
> * Rol atamaları, her rol için doğru izinlere sahip olduğunu doğrulamak için düzenli aralıklarla denetleyin.
> * Kişiler rolleri veya atamaları değiştirdiğinizde rollerini ve atamalarını temizleyin.