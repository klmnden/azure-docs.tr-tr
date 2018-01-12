---
title: "Tehdit algılama - Azure SQL veritabanı | Microsoft Docs"
description: "Tehdit Algılama, veritabanına ilişkin olası güvenlik tehditlerini gösteren anormal veritabanı etkinliklerini algılar."
services: sql-database
documentationcenter: 
author: rmatchoro
manager: shaik
editor: v-romcal
ms.service: sql-database
ms.custom: security
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: On Demand
ms.date: 06/19/2017
ms.author: ronmat
ms.openlocfilehash: 889f65a796aee20d7902964b8c47af46dd9149cb
ms.sourcegitcommit: 71fa59e97b01b65f25bcae318d834358fea5224a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2018
---
# <a name="sql-database-threat-detection"></a>SQL veritabanı tehdit algılama

SQL tehdit algılama erişmek veya veritabanlarını yararlanmak için alışılmadık ve zararlı denemeleri gösteren anormal etkinlikler algılar.

## <a name="overview"></a>Genel Bakış

SQL tehdit algılama, müşterilerin algılamak ve anormal etkinlikler güvenlik uyarıları sağlayarak göründüklerinde olası risklere yanıt sağlayan bir güvenlik yeni bir katman sağlar.  Kullanıcıların şüpheli veritabanı etkinliklerini, olası güvenlik açıkları ve SQL ekleme saldırıları yanı sıra anormal veritabanı erişimi desenleri bağlı bir uyarı alırsınız. SQL tehdit algılama uyarıları şüpheli etkinlik ayrıntılarını sağlayın ve eylem araştırmak ve tehdidi azaltmak nasıl öneririz. Kullanıcıları kullanarak şüpheli olayları Araştır [SQL veritabanı denetimi](sql-database-auditing.md) bunlar erişim, ihlal ya da veritabanındaki verileri yararlanma girişimi sonucu olmadığını belirlemek için. Tehdit algılama, veritabanına Uzman güvenlik olması veya sistemleri izleme Gelişmiş Güvenlik yönetmek zorunda kalmadan adresi olası tehditlere kolaylaştırır.

Örneğin, SQL ekleme veri güdümlü uygulamaları saldırmak için kullanılıyorsa Internet üzerinde ortak Web uygulaması güvenlik sorunlarını biridir. Saldırganlar uygulama giriş alanları, kötü amaçlı SQL deyimleri eklemesine uygulama güvenlik açıkları ihlal veya veritabanındaki verileri değiştirme yararlanın.

SQL tehdit algılama uyarıları ile tümleşir [Azure Güvenlik Merkezi](https://azure.microsoft.com/en-us/services/security-center/), ve $15/düğümü/SQL veritabanı her korumalı burada ay, adresindeki Azure Güvenlik Merkezi standart katmanına aynı fiyatla faturalandırılır her korumalı SQL veritabanı sunucusu Sunucu, bir düğüm olarak sayılır.  

## <a name="set-up-threat-detection-for-your-database-in-the-azure-portal"></a>Tehdit algılama Azure portalında veritabanınız için ayarlama
1. Azure portalında başlatma [https://portal.azure.com](https://portal.azure.com).
2. İzlemek istediğiniz SQL veritabanı yapılandırma sayfasına gidin. Ayarlar sayfasında seçin **denetim ve tehdit algılama**. 
    ![Gezinti Bölmesi][1]
3. İçinde **denetim ve tehdit algılama** yapılandırma sayfası açmak **ON** görüntü tehdit algılama ayarlarını denetleme.
  
    ![Gezinti Bölmesi][2]
4. Kapatma **ON** tehdit algılama.
5. Güvenlik Uyarıları anormal veritabanı etkinliklerini algılandığında almak için e-postaları listesini yapılandırın.
6. Tıklatın **kaydetmek** içinde **denetim ve tehdit algılama** yeni veya güncelleştirilmiş denetim ve tehdit algılama ayarlarını kaydetmek için sayfa.
       
    ![Gezinti Bölmesi][3]

## <a name="set-up-threat-detection-using-powershell"></a>PowerShell kullanarak tehdit saptamayı ayarlama

Bir komut dosyası örneği için bkz: [denetim ve tehdit algılama PowerShell kullanarak yapılandırmanız](scripts/sql-database-auditing-and-threat-detection-powershell.md).

## <a name="explore-anomalous-database-activities-upon-detection-of-a-suspicious-event"></a>Şüpheli bir olay algılandığında anormal veritabanı etkinliklerini keşfedin
1. Anormal veritabanı etkinliklerini algılandığında bir e-posta bildirimi alırsınız. <br/>
   E-posta anormal etkinlikler, veritabanı adı, sunucu adını, uygulama adı ve olay süresi yapısını dahil olmak üzere şüpheli güvenlik olay hakkında bilgi sağlar. Ayrıca, e-posta olası nedenleri hakkında bilgi sağlar ve önerilen eylemleri araştırmak ve veritabanına olası tehdidi azaltmak için.<br/>
     
    ![Gezinti Bölmesi][4]
2. E-posta uyarısı SQL denetim günlüğü doğrudan bir bağlantı içerir. Bu bağlantıyı tıklatmak, Azure Portalı'nı başlatır ve şüpheli olay sırada çevresinde SQL denetim kayıtlarının açar. Bir denetim kaydı yürütüldü SQL deyimlerini bulmayı kolaylaştıracak şüpheli veritabanı etkinliklerini daha fazla ayrıntı görüntülemek için tıklatın (kimin, ne yaptığını ve ne zaman) ve olay yasal veya kötü amaçlı olup olmadığını belirleme (örn. uygulama Güvenlik Açığı SQL Yerleştirmeye yararlanan, birisi ihlal hassas verileri, vb.).<br/>
   ![Gezinti Bölmesi][5]


## <a name="explore-threat-detection-alerts-for-your-database-in-the-azure-portal"></a>Tehdit algılama uyarıları veritabanınızı Azure portalında keşfedin

SQL veritabanı tehdit algılama, uyarıları ile tümleşir [Azure Güvenlik Merkezi](https://azure.microsoft.com/en-us/services/security-center/). Azure portalında veritabanı sayfada canlı bir SQL güvenlik kutucuğu etkin tehditleri durumunu izler. 

   ![Gezinti Bölmesi][6]
   
1. SQL tıklayarak güvenlik kutucuğu, Azure Güvenlik Merkezi uyarıları sayfasına başlatır ve veritabanı üzerinde algılanan etkin SQL tehditleri genel bir bakış sağlar. 

  ![Gezinti Bölmesi][7]

2. Belirli bir uyarıya tıklayarak, ek ayrıntıları ve bu tehdit araştırma ve gelecekte tehditlere düzeltme eylemleri sağlar.

  ![Gezinti Bölmesi][8]


## <a name="next-steps"></a>Sonraki adımlar

* Tehdit algılama hakkında daha fazla bilgi edinmek için bkz [Azure blogu](https://azure.microsoft.com/en-us/blog/azure-sql-database-threat-detection-general-availability-in-spring-2017/) 
* Daha fazla bilgi edinmek [Azure SQL veritabanı denetimi](sql-database-auditing.md)
* Daha fazla bilgi edinmek [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro)
* Fiyatlandırma hakkında daha fazla ayrıntı için bkz: [SQL Database fiyatlandırması sayfası](https://azure.microsoft.com/en-us/pricing/details/sql-database/)  
* PowerShell komut dosyası örneği için bkz: [PowerShell kullanarak denetim ve tehdit algılama yapılandırın](scripts/sql-database-auditing-and-threat-detection-powershell.md)



<!--Image references-->
[1]: ./media/sql-database-threat-detection/1_td_click_on_settings.png
[2]: ./media/sql-database-threat-detection/2_td_turn_on_auditing.png
[3]: ./media/sql-database-threat-detection/3_td_turn_on_threat_detection.png
[4]: ./media/sql-database-threat-detection/4_td_email.png
[5]: ./media/sql-database-threat-detection/5_td_audit_record_details.png
[6]: ./media/sql-database-threat-detection/6_td_security_tile_view_alerts.png
[7]: ./media/sql-database-threat-detection/7_td_SQL_security_alerts_list.png
[8]: ./media/sql-database-threat-detection/8_td_SQL_security_alert_details.png


