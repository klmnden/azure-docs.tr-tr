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
ms.date: 01/16/2018
ms.author: rortloff;barbkess
ms.openlocfilehash: 5400f29d8c7579809ef7b2a084115473df7baa85
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2018
---
# <a name="auditing-in-azure-sql-data-warehouse"></a>Azure SQL veri ambarı'nda denetleme

SQL veri ambarı denetim kayıtla Azure depolama hesabınızdaki denetim veritabanınızdaki olayları günlüğe sağlar. Denetim, yönetmeliklere uygunluğu korumanıza, veritabanı etkinliklerini anlamanıza ve ifade eden tutarsızlıklar ve iş endişeleri veya güvenlik ihlalleri hakkında daha fazla bilgi kavramanıza yardımcı olabilir. Ayrıca SQL Data Warehouse denetim Microsoft Power BI ile raporlama ve analiz için ile tümleştirir.

Denetleme Araçları etkinleştirmek ve uyumluluk standartlarına bağlılığı kolaylaştırmak ancak Uyumluluk garanti etmez. Bu destek standartları uyumluluğu Azure hakkında daha fazla bilgi programlar için bkz: <a href="http://azure.microsoft.com/support/trust-center/compliance/" target="_blank">Azure Güven Merkezi</a>.

## <a id="subheading-1"></a>Denetim temelleri
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

Varsayılan sunucu ilkesi olarak veya belirli bir veritabanı için bir denetim ilkesi tanımlayabilirsiniz. Bir özel geçersiz kılma veritabanı denetim ilkesi tanımlı olmayan sunucudaki tüm veritabanları için bir varsayılan sunucu denetim ilkesi uygulanır.

Denetimi kullanıyorsanız onay denetim ayarlamadan önce bir ["Alt düzey istemci."](sql-data-warehouse-auditing-downlevel-clients.md)

## <a id="subheading-2"></a>Veritabanınız için denetimi ayarlamanız
1. Başlatma <a href="https://portal.azure.com" target="_blank">Azure portal</a>.
2. Git **ayarları** denetlemek istediğiniz SQL Data Warehouse için. Seçin **denetim ve tehdit algılama**.
   
    ![][1]
3. Ardından, tıklayarak denetimi etkinleştirmek **ON** düğmesi.
   
    ![][3]
4. Denetim yapılandırmasını panelinde seçin **depolama ayrıntıları** denetim günlüklerini depolama panelini açmak için. Günlükleri ve Bekletme dönemi için Azure depolama hesabı seçin. 
>[!TIP]
>En iyi önceden yapılandırılmış raporları şablonları almak için tüm denetlenen veritabanları için aynı depolama hesabı kullanın.
   
    ![][4]
5. Tıklatın **Tamam** depolama ayrıntıları yapılandırmayı kaydetmek için düğmesi.
6. Altında **tarafından olay günlüğü**, tıklatın **başarı** ve **hatası** tüm olayları günlüğe kaydedin veya bireysel olay kategorilerini seçin.
7. Bir veritabanı için Denetim yapılandırıyorsanız, verileri denetleme doğru yakalanır emin olmak için istemci bağlantı dizesi alter gerekebilir. Denetleme [bağlantı dizesinde değişiklik sunucu FDQN](sql-data-warehouse-auditing-downlevel-clients.md) alt düzey istemci bağlantıları için konu.
8. **Tamam**’a tıklayın.

## <a id="subheading-3"></a>Denetim günlüklerini ve raporları analiz eder.
Denetim günlüklerini toplanan deposu tablolarla koleksiyonunda bir **SQLDBAuditLogs** Kurulum sırasında seçtiğiniz Azure depolama hesabındaki öneki. Bir aracı gibi kullanarak günlük dosyalarını görüntüleyebilirsiniz <a href="http://azurestorageexplorer.codeplex.com/" target="_blank">Azure Storage Gezgini</a>.

Önceden yapılandırılmış Pano rapor şablonu olarak kullanılabilir bir <a href="http://go.microsoft.com/fwlink/?LinkId=403540" target="_blank">indirilebilir Excel elektronik tablosu</a> günlük verileri hızlı bir şekilde çözümlemenize yardımcı olacak. Denetim günlüklerinizi şablonu kullanmak için Excel 2013 veya üstü ve indirebilirsiniz Power Query ihtiyacınız <a href="http://www.microsoft.com/download/details.aspx?id=39379">burada</a>.

Şablon kurgusal örnek veriler olan ve doğrudan Azure storage hesabınızdan, Denetim günlüğü almak için Power Query ayarlayabilirsiniz.

## <a id="subheading-4"></a>Depolama anahtarını yeniden üretme
Üretimde büyük bir olasılıkla depolama anahtarlarınızı düzenli aralıklarla yenileyin. Anahtarlarınızı yenilerken İlkesi kaydetmeniz gerekir. İşlem aşağıdaki gibidir:

1. Bölüm denetimi önceki kurulumunda açıklanan yapılandırma Panosu denetimde değiştirme **depolama erişim tuşu** gelen *birincil* için *ikincil* ve  **Kaydet**.

   ![][4]
2. Depolama yapılandırması Masası'na gidin ve **yeniden** *birincil erişim anahtarını*.
3. Denetim yapılandırmasını Masası'na geri gidin, 
4. geçiş **depolama erişim tuşu** gelen *ikincil* için *birincil* ve basın **KAYDETMEK**.
4. UI depolama geri gidin ve **yeniden** *ikincil erişim anahtarını* (sonraki için hazırlık olarak anahtarları döngüsü yenileyin.

## <a id="subheading-5"></a>Otomasyon (PowerShell/REST API)
Ayrıca, aşağıdaki Otomasyon araçları kullanarak Azure SQL Data Warehouse'da denetim yapılandırabilirsiniz:

* **PowerShell cmdlet'leri**:

   * [Get-AzureRMSqlDatabaseAuditingPolicy](/powershell/module/azurerm.sql/get-azurermsqldatabaseauditingpolicy)
   * [Get-AzureRMSqlServerAuditingPolicy](/powershell/module/azurerm.sql/Get-AzureRMSqlServerAuditingPolicy)
   * [Remove-AzureRMSqlDatabaseAuditing](/powershell/module/azurerm.sql/Remove-AzureRMSqlDatabaseAuditing)
   * [Remove-AzureRMSqlServerAuditing](/powershell/module/azurerm.sql/Remove-AzureRMSqlServerAuditing)
   * [Set-AzureRMSqlDatabaseAuditingPolicy](/powershell/module/azurerm.sql/Set-AzureRMSqlDatabaseAuditingPolicy)
   * [Set-AzureRMSqlServerAuditingPolicy](/powershell/module/azurerm.sql/Set-AzureRMSqlServerAuditingPolicy)
   * [Use-AzureRMSqlServerAuditingPolicy](/powershell/module/azurerm.sql/Use-AzureRMSqlServerAuditingPolicy)


## <a name="downlevel-clients-support-for-auditing-and-dynamic-data-masking"></a>Denetime ve dinamik veri maskeleme için alt düzey istemci desteği
TDS yeniden yönlendirmeyi destekleyen SQL istemcilerinin denetim çalışır.

TDS 7.4 uygulayan herhangi bir istemci yeniden yönlendirme de desteklemelidir. Bu özel durumlar yeniden yönlendirme özelliğini tam olarak desteklenmez ve Node.JS hangi yeniden yönlendirmesi için Tedious uygulanmadı JDBC 4.0 içerir.

"Alt düzey istemciler" için TDS sürüm 7.3 desteklemek ve aşağıda sunucunun FQDN bağlantı dizesinde aşağıdaki gibi değiştirin:

- Bağlantı dizesindeki özgün sunucunun FQDN: <*sunucu adı*>. database.windows.net
- Bağlantı dizesindeki değiştirilmiş sunucu FQDN: <*sunucu adı*> .database. **güvenli**. windows.net

"Alt düzey istemciler" kısmi bir listesine içerir:

* .NET 4.0 ve aşağıdaki
* ODBC 10.0 ve aşağıdaki.
* JDBC (JDBC TDS 7.4 desteklerken, bu TDS yeniden yönlendirme özelliğini tam olarak desteklenmez)
* Can sıkıcı (için Node.JS)

**Açıklama:** önceki sunucu FDQN değişikliği bir yapılandırma gereksinimini adım olmadan her veritabanı (geçici azaltma) de bir SQL Server düzeyi denetim ilkesi uygulamak için yararlı olabilir.     




<!--Anchors-->
[Database Auditing basics]: #subheading-1
[Set up auditing for your database]: #subheading-2
[Analyze audit logs and reports]: #subheading-3


<!--Image references-->
[1]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing.png
[2]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-inherit.png
[3]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-enable.png
[4]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-storage-account.png
[5]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-dashboard.png


