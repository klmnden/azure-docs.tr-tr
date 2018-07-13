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
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38756286"
---
Depolama je omezeno disk alanı veya sabit bir sınır üzerinde *en büyük sayı* dizinleri, belge veya diğer üst düzey kaynaklar, hangisi önce gelirse. Aşağıdaki tabloda, depolama sınırları belgelenir. Dizin, belgeler ve diğer nesneler üzerinde en fazla limitleri için bkz [kaynak sınırları](../articles/search/search-limits-quotas-capacity.md#index-limits).

| Kaynak | Ücretsiz | Temel&nbsp;<sup>1</sup> | S1 | S2 | S3 | S3&nbsp;HD&nbsp;<sup>2</sup> |
| -------- | --- | --- | --- | --- | --- | --- |
| Hizmet düzeyi sözleşmesi (SLA) <sup>3</sup>  |Hayır |Evet |Evet |Evet |Evet |Evet |
| Bölüm başına depolama |50 MB |2 GB |25 GB |100 GB |200 GB |200 GB |
| Hizmet başına bölüm |Yok |1 |12 |12 |12 |3 |
| Bölüm boyutu |Yok |2 GB |25 GB |100 GB |200 GB |200 GB |
| Çoğaltmalar |Yok |3 |12 |12 |12 |12 |

<sup>1</sup> temel bir sabit disk bölümü vardır. Bu katman ek SUs daha yüksek sorgu iş yükleri için çok kopya tahsis etmek için kullanılır.

<sup>2</sup> S3 HD’nin sabit sınırı 3 bölümdür ve S3’ün bölüm sınırından düşüktür. S3 HD dizin sınırı çok daha yüksek olduğu için daha düşük bir bölüm sınırı uygulanmaktadır. Hem işlem kaynakları (depolama ve işleme) hem de içerik (dizinler ve belgeler) için hizmet sınırları mevcut olduğu için içerik sınırına ilk önce ulaşılır.

<sup>3</sup> olarak ayrılmış kaynaklarda Faturalanabilir Hizmetleri için hizmet düzeyi sözleşmeleri (SLA) sunulur. Ücretsiz Hizmetleri ve önizleme özellikleri, SLA vardır. Hizmetiniz için yeterli yedeklilik sağlarken, Faturalanabilir hizmetler için SLA etkili olur. İki veya daha fazla çoğaltma için sorgu (okuma) SLA gereklidir. Üç veya daha fazla çoğaltma, sorgu ve dizin oluşturma (okuma-yazma) SLA'sı için gereklidir. Bölüm sayısı, bir SLA husus değil. 