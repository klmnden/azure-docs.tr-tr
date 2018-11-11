---
title: include dosyası
description: include dosyası
services: digital-twins
author: kingdomofends
ms.service: digital-twins
ms.topic: include
ms.date: 09/27/2018
ms.author: adgera
ms.custom: include file
ms.openlocfilehash: 215a6c9ff3fdcfe2cb38728ce100255843d6c26e
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2018
ms.locfileid: "51285740"
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