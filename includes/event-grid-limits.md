---
title: include dosyası
description: include dosyası
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: include
ms.date: 04/30/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 2425d9455606f5eabba4b89cfa238b7a928be30e
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2018
ms.locfileid: "51285764"
---
####

Event Grid sistem konuları ve özel konular için aşağıdaki sınırlar geçerlidir *değil* olay etki alanları.

| Kaynak | Sınır |
| --- | --- |
| Azure aboneliği başına özel konu sayısı | 100 |
| Konu başına olay aboneliği sayısı | 500 |
| Yayımlama hızı özel bir konu (giriş) | konu başına saniye başına 5.000 olayları |

####

Aşağıdaki sınırlar yalnızca olay etki alanları için geçerlidir.

| Kaynak | Sınır |
| --- | --- |
| Olay etki alanı başına konuları | Genel Önizleme sırasında 1000 |
| Bir etki alanı içinde konu başına olay aboneliği sayısı | 50 genel Önizleme sırasında |
| Etki alanı kapsamına olay abonelikleri | 50 genel Önizleme sırasında |
| Bir olay etki alanı için (giriş) oranı yayımlama | Genel Önizleme sırasında saniye başına 5.000 olayları |