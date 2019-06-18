---
title: Azure veritabanı üzerinde toplu eklemeler PostgreSQL - tek bir sunucu için en iyi duruma getirme
description: Bu makalede, PostgreSQL - tek bir sunucu için Azure veritabanı toplu INSERT işlemlerine nasıl iyileştirebileceğiniz de açıklanır.
author: dianaputnam
ms.author: dianas
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: c1ae29f7c498a79af09aaaf6d7aeae29561aa500
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65067041"
---
# <a name="optimize-bulk-inserts-and-use-transient-data-on-an-azure-database-for-postgresql---single-server"></a>Geçici veri PostgreSQL - tek bir sunucu için Azure veritabanı üzerinde kullanın ve toplu eklemeler en iyi duruma getirme 
Bu makalede, toplu ekleme işlemleri en iyi duruma getirmek ve geçici veri PostgreSQL sunucusu için Azure veritabanı üzerinde kullanmak nasıl açıklanmaktadır.

## <a name="use-unlogged-tables"></a>Kütüğe aktarılmamış tablolarını kullanma
Geçici verileri içeren veya büyük veri kümelerini toplu olarak eklemek, iş yükü işlemlerini varsa kütüğe aktarılmamış tabloları kullanarak göz önünde bulundurun.

Kütüğe aktarılmamış tablolar, etkili bir şekilde toplu eklemeler en iyi duruma getirmek için kullanılabilir bir PostgreSQL özelliğidir. PostgreSQL yazma tamamlanan günlüğe kaydetme (WAL) kullanır. Kararlılık ve dayanıklılık, varsayılan olarak sağlar. Kararlılık, tutarlılık, yalıtım ve dayanıklılık ACID özelliklerini olun. 

Bir kütüğe aktarılmamış tablo anlamına gelir, PostgreSQL ekleme hareket halinde yazmadan ekler, kendisi bir g/ç işlemdir günlüğe kaydetmez. Sonuç olarak, bu tablolar sıradan tablolardan daha önemli ölçüde daha hızlıdır.

Kütüğe aktarılmamış bir tablo oluşturmak için aşağıdaki seçenekleri kullanın:
- Söz dizimi kullanarak yeni bir kütüğe aktarılmamış tablo oluşturma `CREATE UNLOGGED TABLE <tableName>`.
- Mevcut bir dönüştürme kütüğe aktarılmamış bir tabloya sözdizimini kullanarak tablo oturum açmış `ALTER TABLE <tableName> SET UNLOGGED`.  

İşlemi geri almak için söz dizimini kullanın `ALTER TABLE <tableName> SET LOGGED`.

## <a name="unlogged-table-tradeoff"></a>Kütüğe aktarılmamış tablo düşüş
Kütüğe aktarılmamış tabloları kilitlenme açısından güvenli değildir. Kütüğe aktarılmamış bir tablo, bir çökmeden sonra veya bir şekilde çoğaltamaması kapatma tabi otomatik olarak kesilir. Bir kütüğe aktarılmamış tablosunun ayrıca çoğaltılmadığından, bekleme sunucuları için. Bir kütüğe aktarılmamış tablosunda oluşturulan tüm dizinlerin de otomatik olarak kütüğe aktarılmamış. Sonra ekleme işlemi tamamlandıktan, oturum ekleme dayanıklı olmasını tabloya Dönüştür.

Kütüğe aktarılmamış tabloları kullanıldığında bazı müşteri iş yüklerinin yaklaşık yüzde 15 ila yüzde 20 performans iyileştirmesi karşılaşmıştır.

## <a name="next-steps"></a>Sonraki adımlar
İş yükünüz için geçici veri kullanımını gözden geçirin ve büyük toplu ekler. Aşağıdaki PostgreSQL belgelere bakın:
 
- [Tablo SQL komutları oluşturma](https://www.postgresql.org/docs/current/static/sql-createtable.html)
