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
ms.openlocfilehash: 429f5c9f939a802184a6513a63fb9115abf4b235
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2018
---
# <a name="minimal-downtime-migration-to-azure-database-for-mysql"></a>MySQL için Azure veritabanı en düşük kapalı kalma geçiş
Microsoft Migrations, Attunity ve Hayır Azure veritabanı geçiş hizmeti ile birlikte sağlanan Microsoft birlikte sponsorlu, birleşik bir sunumu için Attunity Replicate kullanarak MySQL için Azure veritabanına varolan MySQL veritabanınızı geçirebilirsiniz Microsoft müşterileri için ek bir maliyet. Attunity çoğaltmak için Microsoft Migrations veritabanı geçiş en düşük kapalı kalma süresi de sağlar ve geçiş işlemi sırasında işlevsel olması kaynak veritabanı devam eder.

Attunity Replicate çeşitli kaynakları ve hedefler arasında veri eşitlemeye sağlayan bir veri çoğaltma ile her veritabanı tablosu ilişkili veri ve şema oluşturma komut dosyasında yayılıyor aracıdır. Attunity Replicate herhangi bir yapı (örneğin, SP, Tetikleyiciler, İşlevler, vb.) yayılması değil ya da dönüştürme, örneğin, T-SQL için böyle yapılara barındırılan PL/SQL kodu.

> [!NOTE]
> Attunity Replicate geçiş senaryoları, geniş kapsamlı bir kümesini destekler, ancak belirli bir alt kaynak/hedef çifti için destek Microsoft Migrations Attunity Replicate odaklanmıştır.

En düşük kapalı kalma geçiş gerçekleştirmeye işlemine genel bakış içerir:

1. **MySQL kaynak şema geçirme** kullanarak MySQL veritabanı için bir Azure veritabanı [MySQL çalışma ekranı](https://www.mysql.com/products/workbench/).

2. **İlk yükleme ve sürekli veri eşitleme kaynak veritabanından hedef veritabanına ayarlama** Attunity çoğaltmak için Microsoft Migrations kullanarak. Bunun yapılması kaynak veritabanı salt okunur olarak Azure üzerinde uygulamalarınızı hedef MySQL veritabanına geçiş yapmak hazırlanırken ayarlanmalıdır süresini en aza indirir.

Microsoft Migrations teklifi için Attunity çoğaltmak hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:
 - Microsoft Migrations Attunity Replicate [web sayfası](https://aka.ms/attunity-replicate).
 - [Karşıdan](http://discover.attunity.com/download-replicate-microsoft-lp6657.html) Microsoft geçişler için Attunity çoğaltılır.
 - Attunity Replicate [topluluk](https://microsoft.attunity.com/), Hızlı Başlangıç Kılavuzu, eğitim ve destek.
 - Azure veritabanı için MySQL MySQL'den geçirmeyi Attunity kullanma hakkında adım adım yönergeler için bkz [veritabanı Geçiş Kılavuzu](https://datamigration.microsoft.com/scenario/mysql-to-azuremysql).