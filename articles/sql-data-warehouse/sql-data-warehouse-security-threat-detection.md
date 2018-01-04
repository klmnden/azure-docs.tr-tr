---
title: "SQL veri ambarı tehdit algılama ile çalışmaya başlama"
description: "Tehdit algılama ile çalışmaya nasıl başlayacağınız"
services: sql-data-warehouse
documentationcenter: 
author: ronortloff
manager: jhubbard
editor: 
ms.assetid: c9073dd9-6c62-4735-8457-dfb9f859c900
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: security
ms.date: 10/31/2016
ms.author: rortloff;barbkess
ms.openlocfilehash: 7f5dab6936e8cac10ac7a4a7dc4c3be116de5ad5
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="get-started-with-threat-detection"></a>Tehdit algılama ile çalışmaya başlama
> [!div class="op_single_selector"]
> * [Denetim](sql-data-warehouse-auditing-overview.md)
> * [Tehdit algılama](sql-data-warehouse-security-threat-detection.md)
> 
> 

## <a name="overview"></a>Genel Bakış
Tehdit algılama veritabanına olası güvenlik tehditlerini gösteren anormal veritabanı etkinliklerini algılar. Tehdit algılama Önizleme aşamasındadır ve SQL Data Warehouse için desteklenir.

Tehdit algılama, müşterilerin algılamak ve anormal etkinlikler güvenlik uyarıları sağlayarak göründüklerinde olası risklere yanıt sağlayan bir güvenlik yeni bir katman sağlar. Kullanıcıları kullanarak şüpheli olayları Araştır [Azure SQL veri ambarı denetimi](sql-data-warehouse-auditing-overview.md) bunlar erişim, ihlal ya da veri ambarındaki yararlanma girişimi sonucu olmadığını belirlemek için.
Tehdit algılama adresi olası tehditlere Uzman güvenlik olması veya sistemleri izleme Gelişmiş Güvenlik yönetmek zorunda kalmadan veri ambarına basit kolaylaştırır.

Örneğin, tehdit algılama olası SQL ekleme girişimlerini gösteren belirli anormal veritabanı etkinliklerini algılar. SQL ekleme veri güdümlü uygulamaları saldırmak için kullanılıyorsa Internet üzerinde ortak Web uygulaması güvenlik sorunlarını biridir. Saldırganlar ihlal veya veritabanındaki verileri değiştirmek için uygulama giriş alanları, kötü amaçlı SQL deyimleri eklemesine uygulama güvenlik açıkları yararlanın.

## <a name="set-up-threat-detection-for-your-database"></a>Tehdit algılama veritabanınız için ayarlama
1. Azure portalında başlatma [https://portal.azure.com](https://portal.azure.com).
2. İzlemek istediğiniz SQL veri ambarı yapılandırma dikey penceresine gidin. Ayarlar dikey penceresinde seçin **denetim ve tehdit algılama**.
   
    ![Gezinti Bölmesi][1]
3. İçinde **denetim ve tehdit algılama** yapılandırma dikey Aç **ON** denetleme, hangi görüntüler tehdit algılama ayarlar.
   
    ![Gezinti Bölmesi][2]
4. Kapatma **ON** tehdit algılama.
5. Güvenlik Uyarıları anormal veri ambarı etkinlikler algılandığında alacak e-postaları listesini yapılandırın.
6. Tıklatın **kaydetmek** içinde **denetim ve tehdit algılama** yeni veya güncelleştirilmiş denetim kaydetmek ve tehdit algılama ilkesi için yapılandırma dikey penceresi.
   
    ![Gezinti Bölmesi][3]

## <a name="explore-anomalous-data-warehouse-activities-upon-detection-of-a-suspicious-event"></a>Anormal veri ambarı etkinlikleri şüpheli bir olay algılandığında keşfedin
1. Anormal veritabanı etkinliklerini algılandığında bir e-posta bildirimi alırsınız. <br/>
   E-posta anormal etkinlikler, veritabanı adı, sunucu adını ve olay süresi yapısını dahil olmak üzere şüpheli güvenlik olayı üzerinde bilgi sağlar. Ayrıca, olası nedenler bilgileri sağlarız ve önerilen eylemleri araştırmak ve veritabanına olası tehdidi azaltmak için.<br/>
   
    ![Gezinti Bölmesi][4]
2. E-posta ile tıklayın **Azure SQL denetim günlüğü** bağlantı, hangi Azure Portalı'nı başlatın ve ilgili denetim kayıtları şüpheli olay sırada geçici gösterir.
   
    ![Gezinti Bölmesi][5]
3. Denetim kayıtlarının SQL deyimini gibi şüpheli veritabanı etkinlikleri hakkında daha fazla ayrıntı görüntülemek için tıklatın başarısızlık nedeni ve istemci IP.
   
    ![Gezinti Bölmesi][6]
4. Denetim kayıtlarının dikey penceresinde tıklayın **Excel'de açın** açmak için önceden yapılandırılmış excel almak ve daha derin denetim günlüğü şüpheli olay sırada geçici analizini çalıştırmak için şablon.<br/>
   **Not:** Excel 2010 veya üzeri, güç sorgu ve **hızlı Birleştir** ayarı gereklidir
   
    ![Gezinti Bölmesi][7]
5. Yapılandırmak için **hızlı Birleştir** - ayarlamayı **POWER QUERY** Şerit sekmesi, select **seçenekleri** Seçenekleri iletişim kutusunu görüntülemek için. Gizlilik bölümünü seçin ve ikinci seçeneği 'Gizlilik düzeylerini yoksayın ve potansiyel performansı geliştirin' - belirtin:
   
    ![Gezinti Bölmesi][8]
6. SQL denetim günlüklerini yüklemek için sekme doğru ayarlandığından ve 'Data' Şerit'i seçin ve 'Tümünü Yenile' düğmesini tıklatın ayarlarını parametrelerinde emin olun.
   
    ![Gezinti Bölmesi][9]
7. Sonuçları görünür **SQL denetim günlüklerini** algılandı anormal etkinlikler daha derin çözümlenmesi çalıştırın ve güvenlik olay uygulamanızda etkisini olanak tanıyan sayfası.

<!--Image references-->
[1]: ./media/sql-data-warehouse-security-threat-detection/1_td_click_on_settings.png
[2]: ./media/sql-data-warehouse-security-threat-detection/2_td_turn_on_auditing.png
[3]: ./media/sql-data-warehouse-security-threat-detection/3_td_turn_on_threat_detection.png
[4]: ./media/sql-data-warehouse-security-threat-detection/4_td_email.png
[5]: ./media/sql-data-warehouse-security-threat-detection/5_td_audit_records.png
[6]: ./media/sql-data-warehouse-security-threat-detection/6_td_audit_record_details.png
[7]: ./media/sql-data-warehouse-security-threat-detection/7_td_audit_records_open_excel.png
[8]: ./media/sql-data-warehouse-security-threat-detection/8_td_excel_fast_combine.png
[9]: ./media/sql-data-warehouse-security-threat-detection/9_td_excel_parameters.png
