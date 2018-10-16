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
ms.openlocfilehash: 075587df445b46c8b03c5448cebf8cd8b12f1179
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49324299"
---
Rol tabanlı erişim denetimi, erişim izinleri ve rolleri yönetmek için bir devralma temelli güvenlik stratejisidir. Bloğun rolleri üst rollerden izinleri devralır. Bir üst rolden devralınmış olmadan izinler de atanabilir. Bir rol gerektiği şekilde özelleştirmek için de atanabilir.

Örneğin, bir **alan yönetici** (altında veya içindeki tüm düğümleri dahil) belirtilen bir alan için tüm işlemleri çalıştırmak için genel erişimi ise gerekebilir bir **cihaz yükleyici** yalnızcagerekebilir*okuma* ve *güncelleştirme* cihazlardan ve sensörlerden izinleri.

Her durumda da, rolleri verilmelidir **en fazla ve tam olarak gerekli erişim** başına görevlerini yerine getirmek için **en az ayrıcalık prensibi**, bir kimlik veren *yalnızca*:

* İşini tamamlamak için gereken erişim miktarı.
* Rol uygun ve kendi işlemi gerçekleştirirken sınırlıdır.

>[!IMPORTANT]
> **Her zaman ilkesine en düşük öncelik ilkesini uygulayın**.

İzlemek için iki diğer önemli rol tabanlı erişim denetimi uygulamalar:

> [!div class="checklist"]
> * Rol atamaları, her rol için doğru izinlere sahip olduğunu doğrulamak için düzenli aralıklarla denetleyin.
> * Rollerini ve atamalarını kişiler rolleri veya atamaları değiştikçe temizlenmiş.