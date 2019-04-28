---
title: Tehdit algılama - Azure SQL veri ambarı | Microsoft Docs
description: Tehdit algılamayı yapılandırma ve Azure SQL veri ambarı'nda şüpheli olayları inceleyin.
services: sql-data-warehouse
author: KavithaJonnakuti
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: security
ms.date: 04/17/2018
ms.author: kavithaj
ms.reviewer: igorstan
ms.openlocfilehash: a3204c2705ba7b83c4fe22ab6bdd15c11eeeeda5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61421156"
---
# <a name="threat-detection-in-azure-sql-data-warehouse"></a>Azure SQL veri ambarı tehdit algılama
Tehdit algılamayı yapılandırma ve Azure SQL veri ambarı'nda şüpheli olayları inceleyin.

## <a name="what-is-threat-detection"></a>Tehdit algılama nedir
Tehdit algılama, veritabanına olası güvenlik tehditlerini gösteren anormal veritabanı etkinliklerini algılar. 

Tehdit Algılama ile sunulan yeni güvenlik katmanı müşterilerin anormal etkinliklerde güvenlik uyarısı oluşturarak potansiyel tehditleri tespit etmesini ve onlara karşı harekete geçmesini sağlar. Kullanıcılar, kullanarak şüpheli etkinlikleri araştırabilirsiniz [Azure SQL veri ambarı denetimi](sql-data-warehouse-auditing-overview.md) oldukları erişim, güvenlik ihlali veya veri ambarındaki açığından yararlanma girişimi sonucunda varsa belirlemek için.
Tehdit algılama, veri ambarına Uzman güvenlik veya Gelişmiş Güvenlik izleme sistemlerine ihtiyaç duymadan potansiyel tehditlerle basitleştirir.

Örneğin, Tehdit Algılama olası SQL ekleme girişimlerini gösteren belirli anormal veritabanı etkinliklerini algılar. SQL ekleme, İnternet üzerindeki en yaygın Web uygulaması güvenlik sorunlarından biridir ve veri temelli uygulamalara saldırmak için kullanılır. Saldırganlar uygulama giriş alanlarına kötü amaçlı SQL deyimleri eklemek ya da veritabanındaki verileri ihlal etmek veya değiştirmek amacıyla uygulamanın güvenlik açıklarından yararlanır.

## <a name="set-up-threat-detection-for-your-database"></a>Veritabanınız için tehdit algılama ayarlama
1. Adresinden Azure portalında başlatma [ https://portal.azure.com ](https://portal.azure.com).
2. İzlemek istediğiniz SQL veri ambarı yapılandırma dikey penceresine gidin. Ayarlar dikey penceresinde **Denetim ve Tehdit Algılama**’yı seçin.
   
    ![Gezinti bölmesi](media/sql-data-warehouse-security-threat-detection/1_td_click_on_settings.png)
3. İçinde **denetim ve tehdit algılama** yapılandırma dikey penceresini aç **ON** denetimi görüntüleyin tehdit algılama ayarları.
   
    ![Gezinti bölmesi](media/sql-data-warehouse-security-threat-detection/2_td_turn_on_auditing.png)
4. Kapatma **ON** tehdit algılama.
5. Anormal veri ambarı etkinliklerinin algılanması üzerine güvenlik uyarıları alacak e-postaların listesini yapılandırın.
6. Tıklayın **Kaydet** içinde **denetim ve tehdit algılama** Kaydet yeni veya güncelleştirilmiş denetim ve tehdit algılama ilkesini yapılandırma dikey penceresi.
   
    ![Gezinti bölmesi](media/sql-data-warehouse-security-threat-detection/3_td_turn_on_threat_detection.png)

## <a name="explore-anomalous-data-warehouse-activities-upon-detection-of-a-suspicious-event"></a>Kuşkulu bir etkinlik algılandığında anormal veri ambarı etkinlikleri keşfedin
1. Anormal veritabanı etkinliklerinin algılanması üzerine bir e-posta bildirimi alırsınız. <br/>
   E-postada şüpheli güvenlik olayına ilişkin anormal etkinliklerin niteliği, veritabanı adı, sunucu adı ve olay zamanı gibi bilgiler verilir. Bunun yanında, veritabanı için geçerli olabilecek tehditleri araştırmak ve azaltmak üzere olası nedenler ve önerilen eylemler hakkında bilgi verilir.<br/>
   
    ![Gezinti bölmesi](media/sql-data-warehouse-security-threat-detection/4_td_email.png)
2. E-postada tıklayarak **Azure SQL denetim günlüğü** bağlantı, Azure portalını başlatın ve şüpheli olayın zamanına yakın ilgili denetim kayıtlarını göster.
   
    ![Gezinti bölmesi](media/sql-data-warehouse-security-threat-detection/5_td_audit_records.png)
3. SQL deyimi gibi şüpheli veritabanı etkinlikleri hakkında daha fazla ayrıntı görüntülemek için denetim kayıtlarına tıklayın hata nedeni ve istemci IP'si.
   
    ![Gezinti bölmesi](media/sql-data-warehouse-security-threat-detection/6_td_audit_record_details.png)
4. Denetim Kayıtları dikey penceresinde **Excel’de Aç**’a tıklayarak, içeri aktarılmak üzere önceden yapılandırılmış bir Excel dosyası açın ve şüpheli olayın zamanına yakın denetim günlüğü üzerinde daha derin bir analiz gerçekleştirin.<br/>
   **Not:** Excel 2010 veya üzeri, Power Query ve **hızlı birleştirme** ayarı gereklidir
   
    ![Gezinti bölmesi](media/sql-data-warehouse-security-threat-detection/7_td_audit_records_open_excel.png)
5. **Hızlı Birleştir** ayarını yapılandırmak için: **POWER QUERY** şerit sekmesinde **Seçenekler** öğesini seçerek Seçenekler iletişim kutusunu görüntüleyin. Gizlilik bölümünü seçin ve ikinci seçeneği belirleyin: 'Gizlilik Düzeylerini yoksayın ve potansiyel performansı geliştirin':
   
    ![Gezinti bölmesi](media/sql-data-warehouse-security-threat-detection/8_td_excel_fast_combine.png)
6. SQL denetim günlüklerini yüklemek için ayarlar sekmesindeki parametrelerin doğru ayarlandığından emin olun ve sonra 'Veri' şeridini seçip 'Tümünü Yenile' düğmesine tıklayın.
   
    ![Gezinti bölmesi](media/sql-data-warehouse-security-threat-detection/9_td_excel_parameters.png)
7. Sonuçlar, algılanan anormal etkinliklerin daha derin bir analizini gerçekleştirmenize ve güvenlik olayının uygulamanızdaki etkisini azaltmanıza olanak tanıyan **SQL Denetim Günlüklerini** sayfasında görünür.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla güvenlik için bilgi [güvenli bir veri ambarı](sql-data-warehouse-overview-manage-security.md).
