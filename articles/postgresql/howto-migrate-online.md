---
title: "Azure veritabanı en düşük kapalı kalma geçiş PostgreSQL için | Microsoft Docs"
description: "Bu makalede, bir PostgreSQL veritabanı dökümü dosyasına ayıklanması, PostgreSQL için Azure veritabanındaki pg_dump oluşturan bir arşiv dosyasına PostgreSQL veritabanına geri ve ilk yük ayarlama en düşük kapalı kalma geçiş işlemini gerçekleştirmek açıklar ve Attunity çoğaltmak için Microsoft Migrations kullanarak hedef veritabanı için sürekli veri eşitleme kaynak veritabanından."
services: postgresql
author: HJToland3
ms.author: jtoland
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 01/04/2018
ms.openlocfilehash: 8c98c58042e7f1d1726eaad6ee03d1531b6c910e
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2018
---
# <a name="minimal-downtime-migration-to-azure-database-for-postgresql"></a>Azure veritabanı en düşük kapalı kalma geçiş PostgreSQL için
Microsoft Migrations, Attunity ve Azure veritabanı geçiş hizmeti ile birlikte sağlanan Microsoft birlikte sponsorlu, birleşik bir sunumu için Attunity Replicate kullanarak PostgreSQL için Azure veritabanına varolan PostgreSQL veritabanınızı geçirebilirsiniz Microsoft müşterileri için hiçbir ek ücret ödemeden. Attunity çoğaltmak için Microsoft Migrations veritabanı geçiş en düşük kapalı kalma süresi de sağlar ve geçiş işlemi sırasında işlevsel olması kaynak veritabanı devam eder.

Attunity Replicate çeşitli kaynakları ve hedefler arasında veri eşitlemeye sağlayan bir veri çoğaltma ile her veritabanı tablosu ilişkili veri ve şema oluşturma komut dosyasında yayılıyor aracıdır. Attunity Replicate herhangi bir yapı (örneğin, SP, Tetikleyiciler, İşlevler, vb.) yayılması değil ya da dönüştürme, örneğin, T-SQL için böyle yapılara barındırılan PL/SQL kodu.

> [!NOTE]
> Attunity Replicate geçiş senaryoları, geniş kapsamlı bir kümesini destekler, ancak belirli bir alt kaynak/hedef çifti için destek Microsoft Migrations Attunity Replicate odaklanmıştır.

En düşük kapalı kalma geçiş gerçekleştirmeye işlemine genel bakış içerir:

1. **PostgreSQL kaynağı şemasını geçirme** kullanarak PostgreSQL veritabanı için bir Azure veritabanı [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) komut - n parametresiyle ve ardından kullanarak [pg_restore](https://www.postgresql.org/docs/9.3/static/app-pgrestore.html) komutu.

2. **İlk yükleme ve sürekli veri eşitleme kaynak veritabanından hedef veritabanına ayarlama** Attunity çoğaltmak için Microsoft Migrations kullanarak. Bunun yapılması kaynak veritabanı salt okunur olarak Azure üzerinde uygulamalarınızı hedef PostgreSQL veritabanına geçiş hazırlanırken ayarlanmalıdır süresini en aza indirir.

Microsoft Migrations teklifi için Attunity çoğaltmak hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:
 - Microsoft Migrations Attunity Replicate [web sayfası](https://aka.ms/attunity-replicate).
 - [Karşıdan](http://discover.attunity.com/download-replicate-microsoft-lp6657.html) Microsoft geçişler için Attunity çoğaltılır.
 - Attunity Replicate [topluluk](https://microsoft.attunity.com/), Hızlı Başlangıç Kılavuzu, eğitim ve destek.
 - PostgreSQL PostgreSQL için Azure veritabanına geçirmek için Attunity kullanma hakkında adım adım yönergeler için bkz [veritabanı Geçiş Kılavuzu](https://datamigration.microsoft.com/scenario/postgresql-to-azurepostgresql).