---
title: "MySQL için Azure veritabanı çevrimiçi geçiş | Microsoft Docs"
description: "Bu makalede, MySQL için bir MySQL veritabanının Azure veritabanı çevrimiçi bir geçiş gerçekleştirme ve ilk yükleme ve sürekli veri eşitleme kaynak veritabanından hedef veritabanına Attunity çoğaltmak için Microsoft Migrations kullanılarak nasıl ayarlanacağı açıklanır."
services: mysql
author: HJToland3
ms.author: jtoland
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 01/03/2018
ms.openlocfilehash: e3a30510e1b3070bc320faa6071cf546a2024e2f
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2018
---
# <a name="online-migration-to-azure-database-for-mysql"></a>MySQL için Azure veritabanı çevrimiçi geçiş
Microsoft Migrations, Attunity ve Hayır Azure veritabanı geçiş hizmeti ile birlikte sağlanan Microsoft birlikte sponsorlu, birleşik bir sunumu için Attunity Replicate kullanarak MySQL için Azure veritabanına varolan MySQL veritabanınızı geçirebilirsiniz Microsoft müşterileri için ek bir maliyet. Attunity çoğaltmak için Microsoft Migrations veritabanı geçiş en düşük kapalı kalma süresi de sağlar ve geçiş işlemi sırasında işlevsel olması kaynak veritabanı devam eder.

Attunity Replicate çeşitli kaynakları ve hedefler arasında veri eşitlemeye sağlayan bir veri çoğaltma ile her veritabanı tablosu ilişkili veri ve şema oluşturma komut dosyasında yayılıyor aracıdır. Attunity Replicate herhangi bir yapı (örneğin, SP, Tetikleyiciler, İşlevler, vb.) yayılması değil ya da dönüştürme, örneğin, T-SQL için böyle yapılara barındırılan PL/SQL kodu.

> [!NOTE]
> Attunity Replicate geçiş senaryoları, geniş kapsamlı bir kümesini destekler, ancak belirli bir alt kaynak/hedef çifti için destek Microsoft Migrations Attunity Replicate odaklanmıştır.

Çevrimiçi bir geçiş gerçekleştirmeye işlemine genel bakış içerir:

1. **MySQL kaynak şema geçirmek** kullanarak MySQL için Azure veritabanı [MySQL çalışma ekranı](https://www.mysql.com/products/workbench/).

2. **İlk yükleme ve sürekli veri eşitleme kaynak veritabanından hedef veritabanına ayarlama** Attunity çoğaltmak için Microsoft Migrations kullanarak. Bunu yaptığınızda bu nedenle kaynak veritabanı üzerinde Azure uygulamalarınızı hedef MySQL veritabanına geçiş hazırlarken salt okunur olarak ayarlanması gerekir süresini en aza indirir.

Microsoft Migrations teklifi için Attunity çoğaltmak hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:
 - Microsoft Migrations Attunity Replicate [web sayfası](https://aka.ms/attunity-replicate).
 - [Karşıdan](http://discover.attunity.com/download-replicate-microsoft-lp6657.html) Microsoft geçişler için Attunity çoğaltılır.
 - Azure veritabanı için MySQL MySQL'den geçirmeyi Attunity kullanma hakkında adım adım yönergeler için bkz [veritabanı Geçiş Kılavuzu](https://datamigration.microsoft.com/scenario/mysql-to-azuremysql).