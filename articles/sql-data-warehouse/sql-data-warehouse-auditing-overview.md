---
title: "Azure SQL veri ambarı'nda denetim | Microsoft Docs"
description: "Azure SQL Data Warehouse'da denetimi ile çalışmaya başlama"
services: sql-data-warehouse
documentationcenter: 
author: ronortloff
manager: jhubbard
editor: 
ms.assetid: 0e6af148-b218-4b43-bb5f-907917d20330
ms.service: sql-data-warehouse
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: security
ms.date: 08/21/2017
ms.author: rortloff;barbkess
ms.openlocfilehash: f851c82ebeaa647f663d499a4d327c3479e36121
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="auditing-in-azure-sql-data-warehouse"></a>Azure SQL veri ambarı'nda denetleme
> [!div class="op_single_selector"]
> * [Denetim](sql-data-warehouse-auditing-overview.md)
> * [Tehdit algılama](sql-data-warehouse-security-threat-detection.md)
> 
> 

SQL veri ambarı denetim kayıtla Azure depolama hesabınızdaki denetim veritabanınızdaki olayları günlüğe sağlar. Denetim, yönetmeliklere uygunluğu korumanıza, veritabanı etkinliklerini anlamanıza ve ifade eden tutarsızlıklar ve iş endişeleri veya güvenlik ihlalleri hakkında daha fazla bilgi kavramanıza yardımcı olabilir. Ayrıca SQL Data Warehouse denetim Microsoft Power BI ile ayrıntıya raporlama ve analiz için ile tümleştirir.

Denetleme Araçları etkinleştirmek ve uyumluluk standartlarına bağlılığı kolaylaştırmak ancak Uyumluluk garanti etmez. Bu destek standartları uyumluluğu Azure hakkında daha fazla bilgi programlar için bkz: <a href="http://azure.microsoft.com/support/trust-center/compliance/" target="_blank">Azure Güven Merkezi</a>.

* [Veritabanı denetim temelleri]
* [Veritabanınız için denetimi ayarlamanız]
* [Denetim günlüklerini ve raporları analiz eder.]

## <a id="subheading-1"></a>Azure SQL veri ambarı veritabanı denetimi temelleri
SQL veri ambarı veritabanı denetimi yapmanıza olanak sağlar:

* **Korumak** seçili olayların bir denetim izi. Denetlenecek veritabanı eylemleri kategorilerini tanımlayabilirsiniz.
* **Rapor** veritabanı etkinlik. Etkinlik ve olay Raporlama ile hızlı bir şekilde başlamak için önceden yapılandırılmış raporları ve panoyu kullanabilirsiniz.
* **Analiz** raporlar. Şüpheli olayları, olağan dışı etkinliği ve eğilimleri bulabilirsiniz.

Aşağıdaki olay kategorisi için Denetim yapılandırabilirsiniz:

**Düz SQL** ve **parametreli SQL** , toplanan denetim günlüklerini olarak sınıflandırılan  

* **Veri erişimi**
* **Şema değişiklikleri (DDL)**
* **Veri değişiklikleri (DML)**
* **Hesapları, rolleri ve izinleri (DCL)**
* **Saklı yordam**, **oturum açma** ve **işlem yönetimi**.

Her bir olay kategorisi için Denetim, **başarı** ve **hatası** işlemleri ayrı olarak yapılandırılır.

Etkinlikleri ve denetlenen olayları hakkında daha fazla bilgi için bkz: <a href="http://go.microsoft.com/fwlink/?LinkId=506733" target="_blank">denetim günlük biçimi başvurusu (doc dosya indirme)</a>.

Denetim günlükleri, Azure depolama hesabında depolanır. Bir denetim günlük Bekletme dönemi tanımlayabilirsiniz.

Bir denetim ilkesi, belirli bir veritabanı veya varsayılan sunucu ilkesi olarak tanımlanabilir. Varsayılan sunucu denetim ilkesi tanımlanmış bir özel geçersiz kılma veritabanı denetim ilkesi bulunmayan tüm veritabanları için bir sunucu üzerinde uygulanır.

Denetimi kullanıyorsanız onay denetim ayarlamadan önce bir ["Alt düzey istemci."](sql-data-warehouse-auditing-downlevel-clients.md)

## <a id="subheading-2"></a>Veritabanınız için denetimi ayarlamanız
1. Başlatma <a href="https://portal.azure.com" target="_blank">Azure portal</a>.
2. Git **ayarları** denetlemek istediğiniz SQL Data Warehouse dikey. İçinde **ayarları** dikey penceresinde, select **denetim ve tehdit algılama**.
   
    ![][1]
3. Ardından, tıklayarak denetimi etkinleştirmek **ON** düğmesi.
   
    ![][3]
4. Denetim yapılandırma dikey penceresinde, seçin **depolama ayrıntıları** denetim günlüklerini depolama dikey penceresini açın. Günlükleri kaydedileceği Azure depolama hesabı ve, bekletme süresini seçin. 
>[!TIP]
>En iyi önceden yapılandırılmış raporları şablonları almak için tüm denetlenen veritabanları için aynı depolama hesabı kullanın.
   
    ![][4]
5. Tıklatın **Tamam** depolama ayrıntıları yapılandırmayı kaydetmek için düğmesi.
6. Altında **tarafından olay günlüğü**, tıklatın **başarı** ve **hatası** tüm olayları günlüğe kaydedin veya bireysel olay kategorilerini seçin.
7. Bir veritabanı için Denetim yapılandırıyorsanız, verileri denetleme doğru yakalanır emin olmak için istemci bağlantı dizesi alter gerekebilir. Denetleme [bağlantı dizesinde değişiklik sunucu FDQN](sql-data-warehouse-auditing-downlevel-clients.md) alt düzey istemci bağlantıları için konu.
8. **Tamam** düğmesine tıklayın.

## <a id="subheading-3"></a>Denetim günlüklerini ve raporları analiz eder.
Denetim günlüklerini toplanan deposu tablolarla koleksiyonunda bir **SQLDBAuditLogs** Kurulum sırasında seçtiğiniz Azure depolama hesabındaki öneki. Bir aracı gibi kullanarak günlük dosyalarını görüntüleyebilirsiniz <a href="http://azurestorageexplorer.codeplex.com/" target="_blank">Azure Storage Gezgini</a>.

Önceden yapılandırılmış Pano rapor şablonu olarak kullanılabilir bir <a href="http://go.microsoft.com/fwlink/?LinkId=403540" target="_blank">indirilebilir Excel elektronik tablosu</a> günlük verileri hızlı bir şekilde çözümlemenize yardımcı olacak. Denetim günlüklerinizi şablonu kullanmak için Excel 2013 veya üstü ve indirebilirsiniz Power Query ihtiyacınız <a href="http://www.microsoft.com/download/details.aspx?id=39379">burada</a>.

Şablon kurgusal örnek veriler olan ve doğrudan Azure storage hesabınızdan, Denetim günlüğü almak için Power Query ayarlayabilirsiniz.

## <a id="subheading-4"></a>Depolama anahtarını yeniden üretme
Üretimde büyük bir olasılıkla depolama anahtarlarınızı düzenli aralıklarla yenileyin. Anahtarlarınızı yenilerken İlkesi kaydetmeniz gerekir. İşlem aşağıdaki gibidir:

1. (Bölüm denetim kurulumunda yukarıda) denetim yapılandırma dikey penceresinde geçiş **depolama erişim tuşu** gelen *birincil* için *ikincil* ve **KAYDETMEK**.

   ![][4]
2. Depolama yapılandırma dikey penceresine gidin ve **yeniden** *birincil erişim anahtarını*.
3. Denetim yapılandırma dikey penceresine geri dönün, geçiş **depolama erişim tuşu** gelen *ikincil* için *birincil* ve basın **KAYDETMEK**.
4. UI depolama geri gidin ve **yeniden** *ikincil erişim anahtarını* (sonraki için hazırlık olarak anahtarları döngüsü yenileyin.

## <a id="subheading-5"></a>Otomasyon (PowerShell/REST API)
Ayrıca, aşağıdaki Otomasyon araçları kullanarak Azure SQL Data Warehouse'da denetim yapılandırabilirsiniz:

* **PowerShell cmdlet'leri**:

   * [Get-AzureRMSqlDatabaseAuditingPolicy][101]
   * [Get-AzureRMSqlServerAuditingPolicy][102]
   * [Remove-AzureRMSqlDatabaseAuditing][103]
   * [Remove-AzureRMSqlServerAuditing][104]
   * [Set-AzureRMSqlDatabaseAuditingPolicy][105]
   * [Set-AzureRMSqlServerAuditingPolicy][106]
   * [Kullanım AzureRMSqlServerAuditingPolicy][107]

<!--Anchors-->
[Veritabanı denetim temelleri]: #subheading-1
[Veritabanınız için denetimi ayarlamanız]: #subheading-2
[Denetim günlüklerini ve raporları analiz eder.]: #subheading-3


<!--Image references-->
[1]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing.png
[2]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-inherit.png
[3]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-enable.png
[4]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-storage-account.png
[5]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-dashboard.png


<!--Link references-->
[101]: /powershell/module/azurerm.sql/get-azurermsqldatabaseauditingpolicy
[102]: /powershell/module/azurerm.sql/Get-AzureRMSqlServerAuditingPolicy
[103]: /powershell/module/azurerm.sql/Remove-AzureRMSqlDatabaseAuditing
[104]: /powershell/module/azurerm.sql/Remove-AzureRMSqlServerAuditing
[105]: /powershell/module/azurerm.sql/Set-AzureRMSqlDatabaseAuditingPolicy
[106]: /powershell/module/azurerm.sql/Set-AzureRMSqlServerAuditingPolicy
[107]: /powershell/module/azurerm.sql/Use-AzureRMSqlServerAuditingPolicy