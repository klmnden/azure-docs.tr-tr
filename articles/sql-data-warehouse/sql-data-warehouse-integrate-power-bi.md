---
title: Power BI ile SQL veri ambarı kullanın | Microsoft Docs
description: Power BI çözümleri geliştirmek için Azure SQL Data Warehouse ile kullanma ipuçları.
services: sql-data-warehouse
documentationcenter: NA
author: mlee3gsd
manager: jhubbard
editor: ''
ms.assetid: b12bee87-2268-40c2-81bf-ab27588b32e8
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 10/31/2016
ms.author: martinle;barbkess
ms.openlocfilehash: 4ea9a2ff0c95a73b348d3b48e9e62957d5cce31c
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
ms.locfileid: "31435298"
---
# <a name="use-power-bi-with-sql-data-warehouse"></a>Power BI ile SQL veri ambarı kullanın
Olarak Azure SQL veritabanı ile SQL veri ambarı doğrudan bağlantı Power BI analitik yeteneklerini yanında aşağı güçlü mantıksal itme yararlanan olanak tanır.  Veri keşfetmenizde doğrudan bağlantı ile sorgular gerçek zamanlı Azure SQL veri ambarı için geri gönderilir.  Bu, birleştirilmiş ölçekli SQL Data Warehouse, terabayt veri göre dakika cinsinden dinamik raporlar oluşturmak için kullanıcıların sağlar.  Ayrıca, Power BI düğmesi Aç giriş doğrudan Power BI kendi SQL veri ambarı'na Azure diğer bölümlerden bilgilerini toplamadan bağlanmasına olanak sağlar.

Doğrudan bağlantı Lütfen Not kullanırken:

* (Daha fazla Ayrıntılar için aşağıya bakın) bağlanırken tam sunucu adını belirtin
* Veritabanı için güvenlik duvarı kurallarının "Azure hizmetlerine erişime izin ver" için yapılandırılmış emin olun.
* Bir sütunun seçilmesi veya bir filtre eklemeden gibi her eylem veri ambarı doğrudan sorgu
* Döşeme yaklaşık 15 (yenileme zamanlanacak gerekmez) dakikada bir yenilenir.
* Soru- cevap veri kümeleri doğrudan bağlanmak için kullanılabilir değil
* Şema değişiklikleri otomatik olarak toplanmaz
* Tüm doğrudan bağlanmak sorguları 2 dakika sonra zaman aşımına uğrar

Deneyimleri geliştirmeye devam ederken bu kısıtlamaları ve notlar değişebilir. Bağlanmak için adımlar aşağıda ayrıntılı olarak açıklanmaktadır.  

## <a name="using-the-open-in-power-bi-button"></a>'Power bı'da Aç' düğmesini kullanarak
SQL veri ambarı ve Power BI arasında taşımak için kolay Power BI düğmesi açıkken yoludur. Bu düğme, sorunsuz bir şekilde yeni pano Power BI'da oluşturmaya başlamak sağlar.  

1. Başlamak için Azure Portalı'ndaki SQL Data Warehouse örneğiniz gidin.
2. "Power BI'da aç" düğmesine tıklayın.
3. Doğrudan oturum açmak yapamıyoruz ya da Power BI hesabınız yoksa, oturum açması gerekir.  
4. SQL Data Warehouse bağlantı sayfası, önceden doldurulmuş SQL veri ambarı bilgileriyle yönlendirilirsiniz.
5. Kimlik bilgilerinizi girdikten sonra tam olarak, SQL Data Warehouse bağlanır.

## <a name="connecting-through-the-power-bi-portal"></a>Power BI Portalı aracılığıyla bağlanma
Power BI düğmesi Aç kullanarak ek olarak, kullanıcılar ayrıca kendi SQL Data Warehouse Power BI Portalı aracılığıyla bağlanabilir.

1. Alt Gezinti Bölmesi ' Veri Al' tıklayın.
2. 'Veritabanlarını' seçin.
3. Bir kez 'Azure SQL Data Warehouse' veritabanlarını sayfasında, seçin ve 'Bağlan' ı.
4. Gerekli bağlantı bilgileri girin.  Sunucu adını ve veritabanı adını Azure Portalı'nda bulunabilir.
5. Power BI, ana sayfasına yönlendirilir ve bağlantınızı 'Veri kümeleri' altında yeni bir giriş yapıldıktan sonra örneğinizi adıyla görüntülenir.  
6. Tüm tabloları ve görünümleri veritabanınızdaki keşfetmek için yeni veri kümesi üzerinde tıklatabilirsiniz. Bir sütunun seçilmesi bir sorgu, visual dinamik olarak oluşturma geri kaynağına gönderir. Bu görsel, yeni bir raporda kaydedilir ve geri panonuza sabitlenir.

<!--Image references-->

<!--Article references-->
[SQL Data Warehouse development overview]:  ./sql-data-warehouse-overview-develop/
[SQL Data Warehouse integration overview]:  ./sql-data-warehouse-overview-integration/

<!--MSDN references-->

<!--Other Web references-->
