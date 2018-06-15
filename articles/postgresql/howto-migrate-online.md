---
title: Azure veritabanı en düşük kapalı kalma geçiş PostgreSQL için
description: Bu makalede, bir PostgreSQL veritabanı dökümü dosyasına ayıklanması, PostgreSQL için Azure veritabanındaki pg_dump oluşturan bir arşiv dosyasına PostgreSQL veritabanına geri ve ilk yük ayarlama en düşük kapalı kalma geçiş işlemini gerçekleştirmek açıklar ve Attunity çoğaltmak için Microsoft Migrations kullanarak hedef veritabanı için sürekli veri eşitleme kaynak veritabanından.
services: postgresql
author: HJToland3
ms.author: jtoland
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 48cf460405ae3985553f9bff29f4fd7abb008196
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
ms.locfileid: "29692098"
---
# <a name="minimal-downtime-migration-to-azure-database-for-postgresql"></a>Azure veritabanı en düşük kapalı kalma geçiş PostgreSQL için
Varolan PostgreSQL veritabanınızı Attunity çoğaltmak için Microsoft Migrations kullanarak PostgreSQL için Azure veritabanına geçirebilirsiniz. Attunity Replicate Attunity ve Microsoft tarafından birleşik bir tekliftir. Azure veritabanı geçiş hizmeti ile birlikte Microsoft müşterileri için hiçbir ek ücret ödemeden dahil edilir. 

Kaynak veritabanı işlemi boyunca işletimsel tutar ve veritabanı geçişler sırasında kapalı kalma süresini en aza indirmek Attunity Replicate yardımcı olur.

Attunity Replicate çeşitli kaynakları ve hedefleri arasında veri eşitlemeye sağlayan bir veri çoğaltma aracıdır. Her veritabanı tablosu ile ilişkili veri ve şema oluşturma komut dosyasında yayar. Attunity Replicate herhangi bir yapı (örneğin, SP, Tetikleyiciler, İşlevler ve benzeri) veya dönüştürme, örneğin, T-SQL böyle yapılara içinde barındırılan PL/SQL kodunu dağıtılmaz.

> [!NOTE]
> Geçiş senaryoları geniş bir dizi Attunity Replicate desteklemesine rağmen belirli bir alt kaynak/hedef çifti için destek odaklanır.

En düşük kapalı kalma geçiş gerçekleştirmeye işlemine genel bakış içerir:

* **PostgreSQL kaynağı şemasını geçirme** kullanarak PostgreSQL veritabanı için bir Azure veritabanı [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) komut - n parametresiyle ve ardından kullanarak [pg_restore](https://www.postgresql.org/docs/9.3/static/app-pgrestore.html) komutu.

* **İlk yükleme ve sürekli veri eşitleme kaynak veritabanından hedef veritabanına ayarlama** Attunity çoğaltmak için Microsoft Migrations kullanarak. Bunun yapılması kaynak veritabanı salt okunur olarak Azure üzerinde uygulamalarınızı hedef PostgreSQL veritabanına geçiş hazırlanırken ayarlanmalıdır süresini en aza indirir.

Microsoft teklifi Migrations Attunity çoğaltmak hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:
 - Git [Attunity çoğaltmak için Microsoft Migrations](https://aka.ms/attunity-replicate) Web sayfası.
 - Karşıdan [Attunity Replicate Microsoft geçişler için](http://discover.attunity.com/download-replicate-microsoft-lp6657.html).
 - Git [Attunity çoğaltmak topluluk](https://aka.ms/attunity-community/) bir Hızlı Başlangıç Kılavuzu, eğitim ve destek için.
 - PostgreSQL PostgreSQL için Azure veritabanına geçirmek için Attunity Replicate kullanma hakkında adım adım yönergeler için bkz: [veritabanı Geçiş Kılavuzu](https://datamigration.microsoft.com/scenario/postgresql-to-azurepostgresql).