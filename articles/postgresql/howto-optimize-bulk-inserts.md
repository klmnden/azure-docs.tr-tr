---
title: PostgreSQL için Azure veritabanı'nda toplu eklemeler en iyi duruma getirme
description: Bu makalede, PostgreSQL için Azure veritabanı toplu INSERT işlemlerine nasıl iyileştirebileceğiniz de açıklanır.
author: dianaputnam
ms.author: dianas
editor: jasonwhowell
ms.service: postgresql
ms.topic: conceptual
ms.date: 10/22/2018
ms.openlocfilehash: 2fe3c3cc71823880d71223334b89816199561ca9
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2018
ms.locfileid: "51629359"
---
# <a name="optimizing-bulk-inserts-and-use-of-transient-data-on-azure-database-for-postgresql-server"></a>Toplu eklemeler ve PostgreSQL sunucusu için Azure veritabanı üzerinde geçici veri kullanımını en iyi duruma getirme 
Bu makalede, toplu ekleme işlemleri ve PostgreSQL sunucusu için Azure veritabanı üzerinde geçici veri kullanımını nasıl iyileştirebileceğiniz de açıklanır.

## <a name="using-unlogged-tables"></a>Kütüğe aktarılmamış tablolarını kullanma
Geçici verileri içeren veya büyük veri kümelerini toplu olarak eklemek, iş yükü işlemlerini sahip müşteriler için kütüğe aktarılmamış tabloları kullanmayı düşünün.

Kütüğe aktarılmamış tablolar, etkili bir şekilde toplu eklemeler en iyi duruma getirmek için kullanılabilir bir PostgreSQL özelliğidir. Yazma tamamlanan günlüğe kaydetme (varsayılan olarak, kararlılık ve dayanıklılık iki ACID özellikleri sağlayan WAL), PostgreSQL kullanır. Bir kütüğe aktarılmamış tablosuna, ortalama PostgreSQL işlem günlüğüne yazmadan eklemeleri yaptığınız ekleme, kendisi bu tablolar daha hızlı normal tablolar yapmadan bir g/ç işlemdir.

Aşağıda kütüğe aktarılmamış bir tablo oluşturmak için Seçenekler şunlardır:
- Söz dizimi kullanarak yeni kütüğe aktarılmamış tablo oluşturun: `CREATE UNLOGGED TABLE <tableName>`
- Mevcut bir dönüştürme söz dizimini kullanarak bir kütüğe aktarılmamış tablosu için tablo günlüğe: `ALTER <tableName> SET UNLOGGED`.  Bu söz dizimini kullanarak ters çevrilebilen: `ALTER <tableName> SET LOGGED`

## <a name="unlogged-table-tradeoff"></a>Kütüğe aktarılmamış tablo düşüş
Kütüğe aktarılmamış tablolar kilitlenme açısından güvenli değildir. Kütüğe aktarılmamış bir tablo, bir çökmeden sonra veya bir şekilde çoğaltamaması kapatma tabi otomatik olarak kesilir. Ayrıca bir kütüğe aktarılmamış tablosunun bekleme sunucularına çoğaltılmaz. Bir kütüğe aktarılmamış tablosunda oluşturulan tüm dizinlerin de otomatik olarak kütüğe aktarılmamış.  Ekleme işlemi tamamlandıktan sonra INSERT dayanıklı olacak şekilde oturum için tabloyu dönüştürebiliriz.

Ancak, bazı müşteri iş yüklerinin biz yaklaşık olarak yüzde 15-20'si performans iyileştirmesi kütüğe aktarılmamış tabloları kullanırken karşılaşılan.

## <a name="next-steps"></a>Sonraki adımlar
İş yükünüz için geçici veri kullanımını gözden geçirin ve büyük toplu ekler.  

Aşağıdaki PostgreSQL belgeleri - gözden [tablo SQL komutları oluşturma](https://www.postgresql.org/docs/current/static/sql-createtable.html)