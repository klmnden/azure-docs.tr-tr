---
title: Tehdit algılama - Azure SQL Data Warehouse | Microsoft Docs
description: Tehdit algılama yapılandırmak ve Azure SQL Data Warehouse şüpheli olayları inceleyin.
services: sql-data-warehouse
author: kavithaj
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: kavithaj
ms.reviewer: igorstan
ms.openlocfilehash: 8dc1ef0432536c6bfd4fe069406cd057ca069ea2
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="threat-detection-in-azure-sql-data-warehouse"></a>Azure SQL Data Warehouse tehdit algılama
Tehdit algılama yapılandırmak ve Azure SQL Data Warehouse şüpheli olayları inceleyin.

## <a name="what-is-threat-detection"></a>Tehdit algılama nedir
Tehdit Algılama, veritabanına ilişkin olası güvenlik tehditlerini gösteren anormal veritabanı etkinliklerini algılar. 

Tehdit Algılama ile sunulan yeni güvenlik katmanı müşterilerin anormal etkinliklerde güvenlik uyarısı oluşturarak potansiyel tehditleri tespit etmesini ve onlara karşı harekete geçmesini sağlar. Kullanıcıları kullanarak şüpheli olayları Araştır [Azure SQL veri ambarı denetimi](sql-data-warehouse-auditing-overview.md) bunlar erişim, ihlal ya da veri ambarındaki yararlanma girişimi sonucu olmadığını belirlemek için.
Tehdit algılama adresi olası tehditlere Uzman güvenlik olması veya sistemleri izleme Gelişmiş Güvenlik yönetmek zorunda kalmadan veri ambarına basit kolaylaştırır.

Örneğin, Tehdit Algılama olası SQL ekleme girişimlerini gösteren belirli anormal veritabanı etkinliklerini algılar. SQL ekleme, İnternet üzerindeki en yaygın Web uygulaması güvenlik sorunlarından biridir ve veri temelli uygulamalara saldırmak için kullanılır. Saldırganlar uygulama giriş alanlarına kötü amaçlı SQL deyimleri eklemek ya da veritabanındaki verileri ihlal etmek veya değiştirmek amacıyla uygulamanın güvenlik açıklarından yararlanır.

## <a name="set-up-threat-detection-for-your-database"></a>Tehdit algılama veritabanınız için ayarlama
1. Azure portalında başlatma [ https://portal.azure.com ](https://portal.azure.com).
2. İzlemek istediğiniz SQL veri ambarı yapılandırma dikey penceresine gidin. Ayarlar dikey penceresinde **Denetim ve Tehdit Algılama**’yı seçin.
   
    ![Gezinti bölmesi](media/sql-data-warehouse-security-threat-detection/1_td_click_on_settings.png)
3. İçinde **denetim ve tehdit algılama** yapılandırma dikey Aç **ON** denetleme, hangi görüntüler tehdit algılama ayarlar.
   
    ![Gezinti bölmesi](media/sql-data-warehouse-security-threat-detection/2_td_turn_on_auditing.png)
4. Kapatma **ON** tehdit algılama.
5. Güvenlik Uyarıları anormal veri ambarı etkinlikler algılandığında alacak e-postaları listesini yapılandırın.
6. Tıklatın **kaydetmek** içinde **denetim ve tehdit algılama** yeni veya güncelleştirilmiş denetim kaydetmek ve tehdit algılama ilkesi için yapılandırma dikey penceresi.
   
    ![Gezinti bölmesi](media/sql-data-warehouse-security-threat-detection/3_td_turn_on_threat_detection.png)

## <a name="explore-anomalous-data-warehouse-activities-upon-detection-of-a-suspicious-event"></a>Anormal veri ambarı etkinlikleri şüpheli bir olay algılandığında keşfedin
1. Anormal veritabanı etkinliklerini algılandığında bir e-posta bildirimi alırsınız. <br/>
   E-postada şüpheli güvenlik olayına ilişkin anormal etkinliklerin niteliği, veritabanı adı, sunucu adı ve olay zamanı gibi bilgiler verilir. Bunun yanında, veritabanı için geçerli olabilecek tehditleri araştırmak ve azaltmak üzere olası nedenler ve önerilen eylemler hakkında bilgi verilir.<br/>
   
    ![Gezinti bölmesi](media/sql-data-warehouse-security-threat-detection/4_td_email.png)
2. E-posta ile tıklayın **Azure SQL denetim günlüğü** bağlantı, hangi Azure Portalı'nı başlatın ve ilgili denetim kayıtları şüpheli olay sırada geçici gösterir.
   
    ![Gezinti bölmesi](media/sql-data-warehouse-security-threat-detection/5_td_audit_records.png)
3. Denetim kayıtlarının SQL deyimini gibi şüpheli veritabanı etkinlikleri hakkında daha fazla ayrıntı görüntülemek için tıklatın başarısızlık nedeni ve istemci IP.
   
    ![Gezinti bölmesi](media/sql-data-warehouse-security-threat-detection/6_td_audit_record_details.png)
4. Denetim Kayıtları dikey penceresinde **Excel’de Aç**’a tıklayarak, içeri aktarılmak üzere önceden yapılandırılmış bir Excel dosyası açın ve şüpheli olayın zamanına yakın denetim günlüğü üzerinde daha derin bir analiz gerçekleştirin.<br/>
   **Not:** Excel 2010 veya üzeri, güç sorgu ve **hızlı Birleştir** ayarı gereklidir
   
    ![Gezinti bölmesi](media/sql-data-warehouse-security-threat-detection/7_td_audit_records_open_excel.png)
5. **Hızlı Birleştir** ayarını yapılandırmak için: **POWER QUERY** şerit sekmesinde **Seçenekler** öğesini seçerek Seçenekler iletişim kutusunu görüntüleyin. Gizlilik bölümünü seçin ve ikinci seçeneği belirleyin: 'Gizlilik Düzeylerini yoksayın ve potansiyel performansı geliştirin':
   
    ![Gezinti bölmesi](media/sql-data-warehouse-security-threat-detection/8_td_excel_fast_combine.png)
6. SQL denetim günlüklerini yüklemek için ayarlar sekmesindeki parametrelerin doğru ayarlandığından emin olun ve sonra 'Veri' şeridini seçip 'Tümünü Yenile' düğmesine tıklayın.
   
    ![Gezinti bölmesi](media/sql-data-warehouse-security-threat-detection/9_td_excel_parameters.png)
7. Sonuçlar, algılanan anormal etkinliklerin daha derin bir analizini gerçekleştirmenize ve güvenlik olayının uygulamanızdaki etkisini azaltmanıza olanak tanıyan **SQL Denetim Günlüklerini** sayfasında görünür.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla güvenlik için bilgi [bir veri ambarı güvenli](sql-data-warehouse-overview-manage-security.md).
