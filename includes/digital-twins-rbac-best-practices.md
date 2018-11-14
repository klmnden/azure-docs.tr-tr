---
title: include dosyası
description: include dosyası
services: digital-twins
author: kingdomofends
ms.service: digital-twins
ms.topic: include
ms.date: 11/13/2018
ms.author: adgera
ms.custom: include file
ms.openlocfilehash: 42aa275e692e4e2e9b7ca38825c828c1f56247fb
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2018
ms.locfileid: "51628225"
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