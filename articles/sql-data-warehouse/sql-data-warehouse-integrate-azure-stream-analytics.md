---
title: "SQL Data Warehouse ile Azure akış analizi kullanın | Microsoft Docs"
description: "Çözümleri geliştirme için Azure SQL Data Warehouse ile Azure akış analizi kullanımı hakkında ipuçları."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: barbkess
editor: 
ms.assetid: 8aeb2247-20c5-4a29-b327-30a8ce09dfdc
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: c5c0450cba541a9346f023057345c5fc9b147903
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="use-azure-stream-analytics-with-sql-data-warehouse"></a>SQL Data Warehouse ile Azure akış analizi kullanın
Azure Stream Analytics akış verileri bulutta üzerinden düşük gecikmeli, yüksek oranda kullanılabilir, ölçeklenebilir karmaşık olay işlemesi sağlayan tam olarak yönetilen bir hizmettir. Okuyarak temel kavramları öğrenebilirsiniz [Azure Stream Analytics'e giriş][Introduction to Azure Stream Analytics]. Ardından izleyerek akış Analizi ile bir uçtan uca çözüm oluşturmak nasıl öğrenebilirsiniz [Azure Stream Analytics ile çalışmaya başlamak] [ Get started using Azure Stream Analytics] Öğreticisi.

Bu makalede, Azure SQL Data Warehouse veritabanınızı, akış analizi işleri için çıkış havuzu olarak kullanmak üzere öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar
İlk olarak, aşağıdaki adımları çalıştırın [Azure Stream Analytics ile çalışmaya başlamak] [ Get started using Azure Stream Analytics] Öğreticisi.  

1. Olay hub'ı giriş oluştur
2. Yapılandırma ve olay Oluşturucu uygulamasını başlatın
3. Akış analizi işi sağlama
4. İş Girişi ve sorgu belirtin

Ardından, bir Azure SQL Data Warehouse veritabanı oluşturma

## <a name="specify-job-output-azure-sql-data-warehouse-database"></a>İş çıktısı belirtin: Azure SQL veri ambarı veritabanı
### <a name="step-1"></a>1. Adım
Stream Analytics işinde tıklatın **çıkış** sayfa ve ardından üstünden **Çıktı Ekle**.

### <a name="step-2"></a>2. Adım
SQL veritabanı'nı seçin ve İleri'yi tıklatın.

![][add-output]

### <a name="step-3"></a>3. Adım
Sonraki sayfada aşağıdaki değerleri girin:

* *Diğer ad çıktı*: Bu iş çıktısı için bir kolay ad girin.
* *Abonelik*:
  * SQL Data Warehouse veritabanınıza Stream Analytics işi ile aynı abonelikte ise, SQL veritabanını kullan geçerli abonelikten seçin.
  * Veritabanınız farklı bir abonelikte ise, SQL veritabanını kullan başka bir abonelik seçin.
* *Veritabanı*: hedef veritabanının adını belirtin.
* *Sunucu adı*: yalnızca belirtilen veritabanı sunucusu adını belirtin. Bunu bulmak için Azure portalını kullanabilirsiniz.

![][server-name]

* *Kullanıcı adı*: veritabanı için yazma izinlerine sahip bir hesabın kullanıcı adını belirtin.
* *Parola*: Belirtilen kullanıcı hesabı için parola sağlayın.
* *Tablo*: veritabanında hedef tablo adını belirtin.

![][add-database]

### <a name="step-4"></a>4. Adım
Bu iş çıktısı eklemek ve akış analizi veritabanına başarıyla bağlanabildiğini doğrulamak için onay düğmesine tıklayın.

![][test-connection]

Veritabanı bağlantısı başarılı olduğunda, portalın altındaki bir bildirim görürsünüz. Veritabanı bağlantısı test etmek için alttaki Bağlantıyı Sına tıklayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Tümleştirme genel bakış için bkz: [SQL Data Warehouse tümleştirme genel bakış][SQL Data Warehouse integration overview].

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
