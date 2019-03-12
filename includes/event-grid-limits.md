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
ms.openlocfilehash: 443db1b4609e62fb7c57de417e42a2b4d0737ada
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2019
ms.locfileid: "57554038"
---
Azure Event Grid sistem konuları ve özel konular için aşağıdaki sınırlar geçerlidir *değil* olay etki alanları.

| Kaynak | Sınır |
| --- | --- |
| Azure aboneliği başına özel konu sayısı | 100 |
| Konu başına olay aboneliği sayısı | 500 |
| Yayımlama hızı özel bir konu (giriş) | konu başına saniye başına 5.000 olayları |

Aşağıdaki sınırlar yalnızca olay etki alanları için geçerlidir.

| Kaynak | Sınır |
| --- | --- |
| Olay etki alanı başına konuları | Genel Önizleme sırasında 1000 |
| Bir etki alanı içinde konu başına olay aboneliği sayısı | 50 genel Önizleme sırasında |
| Etki alanı kapsamı olay abonelikleri | 50 genel Önizleme sırasında |
| Bir olay etki alanı (giriş) oranı yayımlama | Genel Önizleme sırasında saniye başına 5.000 olayları |