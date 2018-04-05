---
title: include dosyası
description: include dosyası
services: search
author: HeidiSteen
ms.service: search
ms.topic: include
ms.date: 04/04/2018
ms.author: heidist
ms.custom: include file
ms.openlocfilehash: b4062aab5a453505ef4586f422a124d4bbf715cb
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
Depolama kısıtlanması disk alanı veya bir sabit sınır tarafından üzerinde *en fazla* dizinleri, belge veya diğer üst düzey kaynakları, hangisi önce gelirse. Aşağıdaki tabloda, depolama sınırları belgeler. Dizinleri, belgeler ve diğer nesneleri maksimum sınırları için bkz: [sınırları kaynak tarafından](../articles/search/search-limits-quotas-capacity.md#index-limits).

| Kaynak | Ücretsiz | Temel&nbsp;<sup>1</sup> | S1 | S2 | S3 | S3&nbsp;HD&nbsp;<sup>2</sup> |
| -------- | --- | --- | --- | --- | --- | --- |
| Hizmet düzeyi sözleşmesi (SLA) <sup>3</sup>  |Hayır |Evet |Evet |Evet |Evet |Evet |
| Bölüm başına depolama |50 MB |2 GB |25 GB |100 GB |200 GB |200 GB |
| Hizmet başına bölüm |Yok |1 |12 |12 |12 |3 |
| Bölüm boyutu |Yok |2 GB |25 GB |100 GB |200 GB |200 GB |
| Çoğaltmalar |Yok |3 |12 |12 |12 |12 |

<sup>1</sup> basic bir sabit disk bölümü vardır. Bu katman ek SUs artırılmış sorgu iş yükleri için daha fazla çoğaltmaları tahsis etmek için kullanılır.

<sup>2</sup> S3 HD’nin sabit sınırı 3 bölümdür ve S3’ün bölüm sınırından düşüktür. S3 HD dizin sınırı çok daha yüksek olduğu için daha düşük bir bölüm sınırı uygulanmaktadır. Hem işlem kaynakları (depolama ve işleme) hem de içerik (dizinler ve belgeler) için hizmet sınırları mevcut olduğu için içerik sınırına ilk önce ulaşılır.

<sup>3</sup> hizmet düzeyi sözleşmelerine (SLA) ile ayrılmış kaynaklarda Faturalanabilir Hizmetleri için sunulur. Ücretsiz Hizmetler ve önizleme özellikleri hiçbir SLA vardır. Hizmetiniz için yeterli artıklık sağladığınızda Faturalanabilir Hizmetleri için SLA etkili olur. İki veya daha fazla çoğaltmaları için (okuma) sorgu SLA gereklidir. Üç veya daha fazla çoğaltmalar, sorgu ve dizin oluşturma (okuma-yazma) SLA için gereklidir. Bölüm sayısı bir SLA önem verilmez. 