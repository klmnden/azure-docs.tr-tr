---
title: include dosyası
description: include dosyası
services: digital-twins
author: kingdomofends
ms.service: digital-twins
ms.topic: include
ms.date: 12/20/2018
ms.author: adgera
ms.custom: include file
ms.openlocfilehash: 7e4760990229433b2ea40fadd0d17de0b52fcb36
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66162144"
---
Azure dijital İkizlerini kullanılabilir rolleri aşağıdaki tabloda açıklanmaktadır:

| **Rol** | **Açıklama** | **tanımlayıcı** |
| --- | --- | --- |
| Alanı yöneticisi | *OLUŞTURMA*, *okuma*, *güncelleştirme*, ve *Sil* belirtilen alan ve altındaki tüm düğümleri için izni. Genel izni. | 98e44ad7-28d4-4007-853b-b9968ad132d1 |
| Kullanıcı Yöneticisi| *OLUŞTURMA*, *okuma*, *güncelleştirme*, ve *Sil* kullanıcılar ve kullanıcı ile ilgili nesneler için izni. *Okuma* alanları izni. | dfaac54c-f583-4dd2-b45d-8d4bbc0aa1ac |
| Cihaz Yöneticisi | *OLUŞTURMA*, *okuma*, *güncelleştirme*, ve *Sil* cihazları ve cihaz ile ilgili nesneler için izni. *Okuma* alanları izni. | 3cdfde07-bc16-40d9-bed3-66d49a8f52ae |
| Anahtar Yöneticisi | *OLUŞTURMA*, *okuma*, *güncelleştirme*, ve *Sil* erişim anahtarlarını izni. *Okuma* alanları izni. | 5a0b1afc-e118-4068-969f-b50efb8e5da6 |
| Belirteç yönetici |  *Okuma* ve *güncelleştirme* erişim anahtarlarını izni. *Okuma* alanları izni. | 38a3bb21-5424-43b4-b0bf-78ee228840c3 |
| Kullanıcı |  *Okuma* alanları, algılayıcılar ve kullanıcılar bunlara karşılık gelen içeren izni ilgili nesneleri. | b1ffdb77-c635-4e7e-ad25-948237d85b30 |
| Destek Uzmanı |  *Okuma* erişim anahtarlarını dışında her şeyi izni. | 6e46958b-dc62-4e7c-990c-c3da2e030969 |
| Cihaz yükleyici | *Okuma* ve *güncelleştirme* cihazlardan ve sensörlerden, bunlara karşılık gelen içeren izni ilgili nesneleri. *Okuma* alanları izni. | b16dd9fe-4efe-467b-8c8c-720e2ff8817c |
| Ağ geçidi cihazı | *OLUŞTURMA* sensörlerden izni. *Okuma* cihazlardan ve sensörlerden, bunlara karşılık gelen içeren izni ilgili nesneleri. | d4c69766-e9bd-4e61-bfc1-d8b6e686c7a8 |