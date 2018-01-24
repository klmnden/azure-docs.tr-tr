---
title: "MySQL için Azure veritabanı en düşük kapalı kalma geçiş | Microsoft Docs"
description: "Bu makalede MySQL için bir MySQL veritabanı Azure veritabanı en düşük kapalı kalma geçişini gerçekleştirmek ve ilk yükleme ve sürekli veri eşitleme kaynak veritabanından hedef veritabanına Attunity çoğaltmak için Microsoft kullanarak ayarlamak açıklar Geçişler."
services: mysql
author: HJToland3
ms.author: jtoland
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 01/04/2018
ms.openlocfilehash: d23628fd8446f6e7e0e5ed14b98da13c09b2d592
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="minimal-downtime-migration-to-azure-database-for-mysql"></a>MySQL için Azure veritabanı en düşük kapalı kalma geçiş
Mevcut MySQL veritabanınız için Microsoft Migrations Attunity Replicate kullanarak MySQL için Azure veritabanına geçirebilirsiniz. Attunity Replicate Attunity ve Microsoft tarafından birleşik bir tekliftir. Azure veritabanı geçiş hizmeti ile birlikte Microsoft müşterileri için hiçbir ek ücret ödemeden dahil edilir. 

Kaynak veritabanı işlemi boyunca işletimsel tutar ve veritabanı geçişler sırasında kapalı kalma süresini en aza indirmek Attunity Replicate yardımcı olur.

Attunity Replicate çeşitli kaynakları ve hedefleri arasında veri eşitlemeye sağlayan bir veri çoğaltma aracıdır. Her veritabanı tablosu ile ilişkili veri ve şema oluşturma komut dosyasında yayar. Attunity Replicate herhangi bir yapı (örneğin, SP, Tetikleyiciler, İşlevler ve benzeri) veya dönüştürme, örneğin, T-SQL böyle yapılara içinde barındırılan PL/SQL kodunu dağıtılmaz.

> [!NOTE]
> Geçiş senaryoları geniş bir dizi Attunity Replicate desteklemesine rağmen belirli bir alt kaynak/hedef çifti için destek odaklanır.

En düşük kapalı kalma geçiş gerçekleştirmeye işlemine genel bakış içerir:

* **MySQL kaynak şema geçirme** MySQL için bir Azure veritabanı yönetilen veritabanı hizmeti kullanarak [MySQL çalışma ekranı](https://www.mysql.com/products/workbench/).

* **İlk yükleme ve sürekli veri eşitleme kaynak veritabanından hedef veritabanına ayarlama** Attunity çoğaltmak için Microsoft Migrations kullanarak. Bunun yapılması kaynak veritabanı salt okunur olarak Azure üzerinde uygulamalarınızı hedef MySQL veritabanına geçiş yapmak hazırlanırken ayarlanmalıdır süresini en aza indirir.

Microsoft teklifi Migrations Attunity çoğaltmak hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:
 - Git [Attunity çoğaltmak için Microsoft Migrations](https://aka.ms/attunity-replicate) Web sayfası.
 - Karşıdan [Attunity Replicate Microsoft geçişler için](http://discover.attunity.com/download-replicate-microsoft-lp6657.html).
 - Git [Attunity çoğaltmak topluluk](https://aka.ms/attunity-community) bir Hızlı Başlangıç Kılavuzu, eğitim ve destek için.
 - MySQL veritabanınız için Azure veritabanı için MySQL geçirmek için Attunity Replicate kullanma hakkında adım adım yönergeler için bkz: [veritabanı Geçiş Kılavuzu](https://datamigration.microsoft.com/scenario/mysql-to-azuremysql).