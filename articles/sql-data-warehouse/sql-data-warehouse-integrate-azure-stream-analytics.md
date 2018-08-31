---
title: SQL veri ambarı ile Azure Stream Analytics'i kullanma | Microsoft Docs
description: Azure Stream Analytics ile Azure SQL veri ambarı çözümleri geliştirmek için kullanma hakkında ipuçları.
services: sql-data-warehouse
author: kavithaj
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: consume
ms.date: 04/17/2018
ms.author: kavithaj
ms.reviewer: igorstan
ms.openlocfilehash: 6e9a6e9c7407939ea9e76cad569e870d578b37f9
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2018
ms.locfileid: "43307370"
---
# <a name="use-azure-stream-analytics-with-sql-data-warehouse"></a>SQL veri ambarı ile Azure Stream Analytics'i kullanma
Azure Stream Analytics, akış verileri bulutta üzerinden düşük gecikme süreli, yüksek oranda kullanılabilir ve ölçeklenebilir karmaşık olay işleme sağlayan tam olarak yönetilen bir hizmettir. Okuyarak temellerini öğrenebilirsiniz [Azure Stream analytics'e giriş][Introduction to Azure Stream Analytics]. Stream Analytics ile izleyerek uçtan uca çözüm oluşturmaya nasıl ardından öğrenebilirsiniz [Azure Stream Analytics'i kullanmaya başlama] [ Get started using Azure Stream Analytics] öğretici.

Bu makalede, akış analizi işlerinizi çıkış havuzu olarak Azure SQL Data Warehouse veritabanınıza kullanmayı öğreneceksiniz.

## <a name="prerequisites"></a>Önkoşullar
Aşağıdaki adımlarda ilk olarak, üzerinde işlem [Azure Stream Analytics'i kullanmaya başlama] [ Get started using Azure Stream Analytics] öğretici.  

1. Bir olay hub'ı girdi oluşturma
2. Yapılandırma ve olay Oluşturucu uygulamasını başlatma
3. Bir Stream Analytics işi sağlama
4. İş Girişi ve sorgu belirtin

Ardından, bir Azure SQL Data Warehouse veritabanı oluşturma

## <a name="specify-job-output-azure-sql-data-warehouse-database"></a>İş çıktısı belirtin: Azure SQL veri ambarı veritabanı
### <a name="step-1"></a>1. Adım
Stream Analytics işinizi tıklayın **çıkış** sayfasının ve ardından üst **Çıktı Ekle**.

### <a name="step-2"></a>2. Adım
SQL veritabanı'nı seçin ve İleri'ye tıklayın.

![][add-output]

### <a name="step-3"></a>3. Adım
Sonraki sayfasında aşağıdaki değerleri girin:

* *Çıkış diğer adı*: Bu iş çıktısı için bir kolay ad girin.
* *Abonelik*:
  * SQL Data Warehouse veritabanınıza Stream Analytics işiyle aynı abonelikte gerekiyorsa geçerli abonelikten SQL veritabanını kullan'ı seçin.
  * Farklı bir abonelikte, veritabanınızı ise SQL veritabanını kullan başka bir aboneliği seçin.
* *Veritabanı*: hedef veritabanının adını belirtin.
* *Sunucu adı*: yalnızca belirtilen veritabanı sunucusu adını belirtin. Bunu bulmak için Azure portalını kullanabilirsiniz.

![][server-name]

* *Kullanıcı adı*: veritabanı için yazma izinlerine sahip hesabın kullanıcı adını belirtin.
* *Parola*: Belirtilen kullanıcı hesabı için parola sağlayın.
* *Tablo*: veritabanında hedef tablonun adını belirtin.

![][add-database]

### <a name="step-4"></a>4. Adım
Bu iş çıktısı eklemek ve Stream Analytics, veritabanına başarıyla bağlantı kurabildiğimizi doğrulamak için onay işaretine tıklayın.

![][test-connection]

Veritabanı bağlantısı başarılı olduğunda portalın alt kısmındaki bir bildirim görürsünüz. Veritabanı bağlantısını test etmek için alttaki Bağlantıyı Sına tıklayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Tümleştirme genel bakış için bkz. [SQL veri ambarı tümleştirmesine genel bakış][SQL Data Warehouse integration overview].

Geliştirme ile ilgili daha fazla ipucu için bkz. [SQL Veri Ambarı’nda geliştirmeye genel bakış][SQL Data Warehouse development overview].

<!--Image references-->

[add-output]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/add-output.png
[server-name]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/dw-server-name.png
[add-database]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/add-database.png
[test-connection]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/test-connection.png

<!--Article references-->

[Introduction to Azure Stream Analytics]: ../stream-analytics/stream-analytics-introduction.md
[Get started using Azure Stream Analytics]: ../stream-analytics/stream-analytics-real-time-fraud-detection.md
[SQL Data Warehouse development overview]:  ./sql-data-warehouse-overview-develop.md
[SQL Data Warehouse integration overview]:  ./sql-data-warehouse-overview-integrate.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Stream Analytics documentation]: http://azure.microsoft.com/documentation/services/stream-analytics/
