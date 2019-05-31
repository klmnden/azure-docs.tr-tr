---
title: include dosyası
description: include dosyası
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: include
ms.date: 05/22/2019
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 3f94481e6a8550479788d92c744327e1dc3b58c4
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66376912"
---
Azure Event Grid sistem konuları ve özel konular için aşağıdaki sınırlar geçerlidir *değil* olay etki alanları.

| Resource | Sınır |
| --- | --- |
| Azure aboneliği başına özel konu sayısı | 100 |
| Konu başına olay aboneliği sayısı | 500 |
| Yayımlama hızı özel bir konu (giriş) | konu başına saniye başına 5.000 olayları |
| İstekleri yayımlama | saniye başına 250 |
| Olay boyutu | 64 KB için genel kullanılabilirlik (GA) destekler. 1 MB desteği şu anda Önizleme aşamasındadır. |

Aşağıdaki sınırlar yalnızca olay etki alanları için geçerlidir.

| Resource | Sınır |
| --- | --- |
| Olay etki alanı başına konuları | Genel Önizleme sırasında 1000 |
| Bir etki alanı içinde konu başına olay aboneliği sayısı | 50 genel Önizleme sırasında |
| Etki alanı kapsamı olay abonelikleri | 50 genel Önizleme sırasında |
| Bir olay etki alanı (giriş) oranı yayımlama | Genel Önizleme sırasında saniye başına 5.000 olayları |
| İstekleri yayımlama | saniye başına 250 |