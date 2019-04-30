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
ms.openlocfilehash: 30c6fc189ebcd497a214828f65213a55cefdf03f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61464905"
---
Depolama je omezeno disk alanı veya sabit bir sınır üzerinde *en büyük sayı* dizinleri, belge veya diğer üst düzey kaynaklar, hangisi önce gelirse. Aşağıdaki tabloda, depolama sınırları belgelenir. Dizin, belgeler ve diğer nesneler üzerinde en fazla limitleri için bkz [kaynak sınırları](../articles/search/search-limits-quotas-capacity.md#index-limits).

| Kaynak | Ücretsiz | Temel<sup>1</sup> | S1 | S2 | S3 | S3&nbsp;HD<sup>2</sup> | L1 | L2 |
| -------- | --- | --- | --- | --- | --- | --- | --- | --- |
| Hizmet düzeyi sözleşmesi (SLA)<sup>3</sup>  |Hayır |Evet |Evet |Evet |Evet |Evet |Evet |Evet |
| Bölüm başına depolama |50 MB |2 GB |25 GB |100 GB |200 GB |200 GB |1 TB |2 TB |
| Hizmet başına bölüm |Yok |1 |12 |12 |12 |3 |12 |12 |
| Bölüm boyutu |Yok |2 GB |25 GB |100 GB |200 GB |200 GB |1 TB |2 TB |
| Çoğaltmalar |Yok |3 |12 |12 |12 |12 |12 |12 |

<sup>1</sup> temel bir sabit disk bölümü vardır. Bu katman daha yüksek sorgu iş yükleri için çok kopya tahsis etmek için ek arama birimi kullanılır.

<sup>2</sup> S3 hd'nin sabit üç bölüm sınırının, S3'ün bölüm sınırından düşüktür. S3 HD dizin sınırı çok daha yüksek olduğu için daha düşük bir bölüm sınırı uygulanmaktadır. Hem işlem kaynakları (depolama ve işleme) hem de içerik (dizinler ve belgeler) için hizmet sınırları mevcut olduğu için içerik sınırına ilk önce ulaşılır.

<sup>3</sup> olarak ayrılmış kaynaklarda Faturalanabilir Hizmetleri için hizmet düzeyi sözleşmeleri sunulur. Ücretsiz Hizmetleri ve önizleme özellikleri, SLA vardır. Hizmetiniz için yeterli yedeklilik sağlarken, Faturalanabilir hizmetler için SLA etkili olur. İki veya daha fazla çoğaltma için sorgu (okuma) SLA'lar gereklidir. Üç veya daha fazla çoğaltma, sorgu ve dizin oluşturma (okuma-yazma) SLA'ları için gereklidir. Bölüm sayısı, bir SLA husus değildir. 