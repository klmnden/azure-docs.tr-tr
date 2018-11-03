---
title: include dosyası
description: include dosyası
services: azure-digital-twins
author: adamgerard
ms.service: azure-digital-twins
ms.topic: include
ms.date: 09/27/2018
ms.author: adgera
ms.custom: include file
ms.openlocfilehash: a03f2b4e8d216db3764af03dc06b5188289ffc92
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2018
ms.locfileid: "50964212"
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