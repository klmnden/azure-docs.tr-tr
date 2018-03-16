---
title: "SQL veritabanı uyarıları oluşturmak için Azure portalını kullanma | Microsoft Docs"
description: "Belirttiğiniz koşullar karşılandığında, bildirimler ya da Otomasyon tetikleyebilir SQL veritabanı uyarılar oluşturmak için Azure Portalı'nı kullanın."
author: aamalvea
manager: craigg
services: sql-database
ms.service: sql-database
ms.custom: monitor and tune
ms.topic: article
ms.date: 06/06/2017
ms.author: aamalvea
ms.openlocfilehash: 611b88c540902bc7a72d53671dacd098d9798b48
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="use-azure-portal-to-create-alerts-for-azure-sql-database-and-data-warehouse"></a>Azure SQL veritabanı ve veri ambarı için uyarıları oluşturmak için Azure portalını kullanma

## <a name="overview"></a>Genel Bakış
Bu makalede Azure portalını kullanarak Azure SQL veritabanı ve veri ambarı uyarılarını ayarlama gösterilmiştir. Bu makalede ayrıca uyarı dönemleri ayarlamak için en iyi yöntemler sağlar.    

İzleme ölçümlerini ya da olayları, Azure hizmetlerinizi göre bir uyarı alabilirsiniz.

* **Ölçüm değerleri** -herhangi bir yönde atadığınız bir eşik değeri, belirtilen bir ölçüm kestiği olduğunda uyarı tetikler. Diğer bir deyişle, her ikisi de tetikler koşul ilk ve ardından daha sonra ne zaman, koşul artık karşılanıp zaman.    
* **Etkinlik günlüğü olaylarını** -bir uyarıyı tetiklemek *her* olay veya yalnızca belirli bir sayıda olayları oluşur.

Tetikler, aşağıdakileri yapmak için bir uyarı yapılandırabilirsiniz:

* Hizmet yöneticisini ve ortak Yöneticiler e-posta bildirimleri gönder
* Belirttiğiniz ek e-postalar için e-posta gönderin.
* bir Web kancası çağırın

Yapılandırma ve uyarı kuralları kullanma hakkında bilgi edinin

* [Azure Portal](../monitoring-and-diagnostics/insights-alerts-portal.md)
* [PowerShell](../monitoring-and-diagnostics/insights-alerts-powershell.md)
* [Komut satırı arabirimi (CLI)](../monitoring-and-diagnostics/insights-alerts-command-line-interface.md)
* [Azure monitör REST API'si](https://msdn.microsoft.com/library/azure/dn931945.aspx)

## <a name="create-an-alert-rule-on-a-metric-with-the-azure-portal"></a>Azure portal ile bir ölçüm bir uyarı kuralı oluşturma
1. İçinde [portal](https://portal.azure.com/), izleme ilgilenen kaynak bulup seçin.
2. Bu adım, SQL DB ve SQL DW esnek havuzları için farklıdır: 

   - **SQL DB yalnız & CA esnek havuzları**: seçin **uyarıları** veya **uyarı kuralları** izleme bölümünde. Metin ve simge farklı kaynaklar için biraz değişebilir.  
   
     ![İzleme](../monitoring-and-diagnostics/media/insights-alerts-portal/AlertRulesButton.png)
  
   - **YALNIZCA SQL DW**: seçin **izleme** ortak görevler bölümünün altında. Tıklatın **DWU kullanım** grafik.

     ![ORTAK GÖREVLER](../monitoring-and-diagnostics/media/insights-alerts-portal/AlertRulesButtonDW.png)

3. Seçin **uyarı Ekle** komut ve alanları doldurun.
   
    ![Uyarı Ekle](../monitoring-and-diagnostics/media/insights-alerts-portal/AddDBAlertPage.png)
4. **Ad** , uyarı kuralı ve seçin bir **açıklama**, bildirim e-postalarda da gösterir.
5. Seçin **ölçüm** izlemek ve ardından istediğiniz bir **koşulu** ve **eşik** ölçüm için değer. Ayrıca tercih **süresi** ölçüm kuralı uyarı Tetikleyicileri önce karşılanması gereken süre. Bu nedenle örneğin "PT5M" dönemi kullanıyorsanız ve Uyarınız % 80 CPU görünüyor, uyarı ne zaman tetikler **ortalama** CPU % 80 ' 5 dakika için açıldı. İlk tetikleyici oluşur sonra ortalama CPU üzerinde 5 dakika % 80 aşağısına olduğunda yeniden tetikler. CPU ölçüm, 1 dakikada oluşur. Desteklenen saat windows için aşağıdaki tabloya başvurun ve toplama yazın ve her uyarı kullanır değil tüm uyarıları ortalama değer kullanın.   
6. Denetleme **e-posta sahipleri...**  yöneticileri ve ortak Yöneticiler uyarı oluşturulduğunda e-posta gönderilip istiyorsanız.
7. Uyarı oluşturulduğunda bir bildirim almak için ek e-postaları istiyorsanız, bunları ekleyin **ek yönetici email(s)** alan. Birden çok e-postaları - noktalı virgülle ayırın  *email@contoso.com;email2@contoso.com*
8. Geçerli bir URI koymak **Web kancası** adlı uyarı oluşturulduğunda istiyorsanız alan.
9. Seçin **Tamam** uyarı oluşturmak için yapıldığında.   

Birkaç dakika içinde uyarı etkindir ve daha önce açıklandığı şekilde tetikler.

## <a name="managing-your-alerts"></a>Uyarılarınızı yönetme
Bir uyarı oluşturduktan sonra bunu seçebilirsiniz ve:

* Ölçüm eşiği ve önceki gün gerçek değerleri gösteren bir grafik görüntüleyin.
* Düzenleyin veya silin.
* **Devre dışı** veya **etkinleştirmek** , geçici olarak durdurmak veya bu uyarı için bildirimleri almaya devam etmek istiyorsanız.


## <a name="sql-database-alert-values"></a>SQL veritabanı uyarı değerleri

| Kaynak Türü | Ölçüm Adı | Kolay Ad | Toplama türü | En az uyarı zaman penceresi|
| --- | --- | --- | --- | --- |
| SQL veritabanı | cpu_percent | CPU yüzdesi | Ortalama | 5 dakika |
| SQL veritabanı | physical_data_read_percent | Veri G/Ç yüzdesi | Ortalama | 5 dakika |
| SQL veritabanı | log_write_percent | Günlük g/ç yüzdesi | Ortalama | 5 dakika |
| SQL veritabanı | dtu_consumption_percent | DTU yüzdesi | Ortalama | 5 dakika |
| SQL veritabanı | depolama | Toplam veritabanı boyutu | Maksimum | 30 dakika |
| SQL veritabanı | connection_successful | Başarılı bağlantıları | Toplam | 10 dakika |
| SQL veritabanı | connection_failed | Başarısız bağlantı sayısı | Toplam | 10 dakika |
| SQL veritabanı | blocked_by_firewall | Güvenlik Duvarı tarafından engellendi | Toplam | 10 dakika |
| SQL veritabanı | Kilitlenme | Kilitlenmeleri | Toplam | 10 dakika |
| SQL veritabanı | storage_percent | Veri boyutu yüzdesi | Maksimum | 30 dakika |
| SQL veritabanı | xtp_storage_percent | Bellek içi OLTP depolama percent(Preview) | Ortalama | 5 dakika |
| SQL veritabanı | workers_percent | Çalışanlar yüzdesi | Ortalama | 5 dakika |
| SQL veritabanı | sessions_percent | Oturumları yüzde | Ortalama | 5 dakika |
| SQL veritabanı | dtu_limit | DTU sınırı | Ortalama | 5 dakika |
| SQL veritabanı | dtu_used | Kullanılan DTU | Ortalama | 5 dakika |
||||||
| Esnek havuz | cpu_percent | CPU yüzdesi | Ortalama | 10 dakika |
| Esnek havuz | physical_data_read_percent | Veri G/Ç yüzdesi | Ortalama | 10 dakika |
| Esnek havuz | log_write_percent | Günlük g/ç yüzdesi | Ortalama | 10 dakika |
| Esnek havuz | dtu_consumption_percent | DTU yüzdesi | Ortalama | 10 dakika |
| Esnek havuz | storage_percent | Depolama yüzdesi | Ortalama | 10 dakika |
| Esnek havuz | workers_percent | Çalışanlar yüzdesi | Ortalama | 10 dakika |
| Esnek havuz | eDTU_limit | eDTU sınırı | Ortalama | 10 dakika |
| Esnek havuz | storage_limit | Depolama sınırı | Ortalama | 10 dakika |
| Esnek havuz | eDTU_used | kullanılan eDTU | Ortalama | 10 dakika |
| Esnek havuz | storage_used | Kullanılan depolama | Ortalama | 10 dakika |
||||||               
| SQL Veri Ambarı | cpu_percent | CPU yüzdesi | Ortalama | 10 dakika |
| SQL Veri Ambarı | physical_data_read_percent | Veri G/Ç yüzdesi | Ortalama | 10 dakika |
| SQL Veri Ambarı | depolama | Toplam veritabanı boyutu | Maksimum | 10 dakika |
| SQL Veri Ambarı | connection_successful | Başarılı bağlantıları | Toplam | 10 dakika |
| SQL Veri Ambarı | connection_failed | Başarısız bağlantı sayısı | Toplam | 10 dakika |
| SQL Veri Ambarı | blocked_by_firewall | Güvenlik Duvarı tarafından engellendi | Toplam | 10 dakika |
| SQL Veri Ambarı | service_level_objective | Veritabanının hizmet düzeyi hedefi | Toplam | 10 dakika |
| SQL Veri Ambarı | dwu_limit | dwu limit | Maksimum | 10 dakika |
| SQL Veri Ambarı | dwu_consumption_percent | DWU yüzdesi | Ortalama | 10 dakika |
| SQL Veri Ambarı | dwu_used | Kullanılan DWU | Ortalama | 10 dakika |
||||||


## <a name="next-steps"></a>Sonraki adımlar
* [Azure izleme genel bir bakış elde](../monitoring-and-diagnostics/monitoring-overview.md) toplamak ve izlemek bilgi türlerini de dahil olmak üzere.
* Daha fazla bilgi edinmek [Web kancalarını uyarıları yapılandırma](../monitoring-and-diagnostics/insights-webhooks-alerts.md).
* Alma bir [tanılama günlükleri'ne genel bakış](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) hizmetinizde ayrıntılı yüksek sıklıkta ölçümleri toplamak ve.
* Alma bir [ölçümleri toplama genel bakış](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) hizmetinizi kullanılabilir ve yanıt verebilir durumda olduğundan emin olmak için.
