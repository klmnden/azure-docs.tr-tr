---
title: Azure SQL veri ambarı'nda denetim | Microsoft Docs
description: Denetim ve Azure SQL veri ambarı'nda denetimini ayarlama hakkında bilgi edinin.
services: sql-data-warehouse
author: KavithaJonnakuti
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 04/11/2018
ms.author: kavithaj
ms.reviewer: igorstan
ms.openlocfilehash: ef791bdfafbbd49cacad1a75c7171b9a030df2a3
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55456230"
---
# <a name="auditing-in-azure-sql-data-warehouse"></a>Azure SQL veri ambarı'nda denetleme

Denetim ve Azure SQL veri ambarı'nda denetimini ayarlama hakkında bilgi edinin.

## <a name="what-is-auditing"></a>Denetim?
SQL veri ambarı denetim kaydı bir denetim veritabanınızdaki olayları Azure depolama hesabınızda oturum sağlar. Denetim mevzuatla uyumluluk, veritabanı etkinliğini anlama ve uyuşmazlıkları ve işletme sorunlarını veya şüpheli güvenlik ihlallerini anormal durumları kavramak yardımcı olabilir.

Denetleme araçları etkinleştirme ve uyumluluk standartlarını kıldığı kolaylaştırmak ancak Uyumluluk garanti etmez. Azure hakkında daha fazla bilgi bu standartlara uyumluluk programları için bkz: [Azure Güven Merkezi](https://azure.microsoft.com/support/trust-center/compliance/).

## <a id="subheading-1"></a>Denetim temelleri
SQL veri ambarı veritabanı denetimi yapmanıza olanak sağlar:

* **Korumak** seçili olayların bir denetim kaydı. Denetlenecek veritabanı eylemlerin kategoriler tanımlayabilirsiniz.
* **Rapor** veritabanı etkinlikleri. Önceden yapılandırılmış raporları ve Panosu, etkinlik ve olay Raporlama ile hızla gerçekleştirmek için kullanabilirsiniz.
* **Analiz** raporlar. Şüpheli olayları, olağan dışı etkinliği ve eğilimleri bulabilirsiniz.

Denetim günlükleri, Azure depolama hesabında depolanır. Bir denetim günlük tutma süresi tanımlayabilirsiniz.


## <a id="subheading-4"></a>Sunucu düzeyinde ve veritabanı düzeyinde denetim ilkesi tanımlama

Bir denetim ilkesi, ilke varsayılan sunucu olarak veya belirli bir veritabanı için tanımlanabilir:

* Bir sunucu İlkesi **tüm mevcut ve yeni oluşturulan veritabanları için geçerlidir** sunucusunda.

* Varsa *sunucu blob denetimi etkinse*, onu *her zaman veritabanına uygulanır*. Veritabanı denetim ayarları bağımsız olarak veritabanı denetlenecektir.

* Veritabanında denetimi etkinleştirme, sunucuda etkinleştirmenin yanı sıra mu *değil* geçersiz kılabilir veya sunucu blob denetimi ayarlarından herhangi birini değiştirin. Her iki denetimleri yan yana bulunur. Diğer bir deyişle, veritabanı iki kez paralel olarak denetlenir; Sunucu İlkesi ve bir kez veritabanı İlkesi tarafından bir kez.

> [!NOTE]
> Etkinleştirmenizi öneririz **yalnızca sunucu düzeyinde blob denetimi** ve devre dışı tüm veritabanları için veritabanı düzeyi denetimi bırakın.
> Hem sunucu denetimi hem de veritabanı birlikte denetimi etkinleştirmelerini kaçınmanız gerekir:
> * Farklı bir kullanmak istediğiniz *depolama hesabı* veya *saklama süresi* belirli bir veritabanı için.
> * Olay türleri ya da sunucu üzerindeki bir veritabanına geri kalanından farklı kategorilere belirli bir veritabanı için Denetim istiyorsunuz. Örneğin, yalnızca belirli bir veritabanı için denetlenmesi gereken tablo eklemeleri olabilir.
> * Şu anda tehdit algılama, kullanmak istediğiniz yalnızca veritabanı düzeyi denetimi ile desteklenir.

> [!IMPORTANT]
>Bir Azure SQL veri ambarı veya bir Azure SQL veri ambarı'na da olan bir sunucuyu denetlemeyi etkinleştirme **sürdürülüyor veri ambarında sonuçlanır**, burada, daha önce duraklatıldı durumunda bile. **Denetim yeniden etkinleştirdikten sonra veri ambarını duraklatmak emin olun**.

## <a id="subheading-5"></a>Tüm veritabanları için sunucu düzeyi denetimi ayarlamanız

Bir sunucu denetim ilkesinin uygulanacağı **tüm mevcut ve yeni oluşturulan veritabanları** sunucusunda.

Aşağıdaki bölümde, Denetim Azure portalını kullanarak yapılandırmayı açıklar.

1. [Azure Portal](https://portal.azure.com) gidin.
2. Git **SQL server** denetlemek istediğiniz (önemli, SQL server olmayan bir belirli veritabanı/DW görüntülediğinizden emin olun). İçinde **güvenlik** menüsünde **denetim ve tehdit algılama**.

    ![Gezinti bölmesi][6]
4. İçinde *denetim ve tehdit algılama* dikey penceresinde için **denetim** seçin **ON**. Bu denetim ilkesi, bu sunucudaki tüm mevcut ve yeni oluşturulan veritabanları için geçerli olacaktır.

    ![Gezinti bölmesi][7]
5. Açmak için **denetim günlükleri depolama** dikey penceresinde **depolama ayrıntıları**. Seçin veya burada günlükleri kaydedileceği konum ve saklama dönemi (eski günlüklerin silinecek) ardından Azure depolama hesabı oluşturun. Daha sonra, **Tamam**'a tıklayın.

    ![Gezinti bölmesi][8]

    > [!IMPORTANT]
    > Sunucu düzeyi denetim günlüklerine yazılır **ekleme Blobları** Azure aboneliğinizde bir Azure Blob Depolama alanında.
    >
    > * **Premium depolama** şu anda **desteklenmiyor** tarafından ekleme Blobları.
    > * **Sanal ağ içindeki depolama** şu anda **desteklenmiyor**.

8. **Kaydet**’e tıklayın.



## <a id="subheading-2"></a>Tek bir veritabanı için veritabanı düzeyi denetimi ayarlamanız

İlke varsayılan sunucu olarak veya belirli bir veritabanı için denetim ilkesi tanımlayabilirsiniz.

Sunucu düzeyi denetimi kullanın ve veritabanı düzeyinde denetim, açıklandığı önerilir [sunucu düzeyinde veritabanı düzeyinde denetim ilkesi tanımlama](#subheading-4)

Denetimi kullanıyorsanız onay denetim ayarlamadan önce bir ["Alt düzey istemci"](sql-data-warehouse-auditing-downlevel-clients.md).


1. Başlatma [Azure portalında](https://portal.azure.com).
2. Git **ayarları** denetlemek istediğiniz SQL veri ambarı için. Seçin **denetim ve tehdit algılama**.

    ![][1]
3. Ardından, tıklayarak denetimini etkinleştirme **ON** düğmesi.

    ![][3]
4. Denetim yapılandırma panelinde seçin **depolama ayrıntıları** denetim günlükleri depolama panelini açmak için. Günlükleri ve saklama süresi için Azure depolama hesabı seçin.
    >[!TIP]
    >En iyi önceden yapılandırılmış raporları şablonlarından için denetlenen tüm veritabanları için aynı depolama hesabını kullanın.

    ![][4]

5. Tıklayın **Tamam** depolama ayrıntıları yapılandırmayı kaydetmek için düğme.
6. Altında **tarafından olay günlüğü**, tıklayın **başarı** ve **hatası** tüm olaylarını günlüğe kaydedecek ya da tek tek olay kategorilerini seçin.
7. Bir veritabanı için Denetim yapılandırıyorsanız, verileri denetleme düzgün yakalandıktan emin olmak için istemci bağlantı dizesini değiştirmek gerekebilir. Denetleme [bağlantı dizesinde değişiklik Server FQDN](sql-data-warehouse-auditing-downlevel-clients.md) konu alt düzey istemci bağlantıları için.
8. **Tamam** düğmesine tıklayın.

## <a id="subheading-3"></a>Denetim günlüklerini ve raporları analiz edin

### <a name="server-level-policy-audit-logs"></a>Sunucu düzeyindeki ilke Denetim günlükleri
Sunucu düzeyi denetim günlüklerine yazılır **ekleme Blobları** Azure aboneliğinizde bir Azure Blob Depolama alanında. Adlı bir kapsayıcı içinde blob dosyaları koleksiyonu olarak kaydedilirler **sqldbauditlogs**.

Depolama klasörü hiyerarşisi hakkında daha fazla ayrıntı için bkz: adlandırma kuralları ve günlük biçimi, [Blob denetim günlük biçimi başvurusu](https://go.microsoft.com/fwlink/?linkid=829599).

Blob günlükleri denetleme görüntülemek için kullanabileceğiniz birkaç yöntem vardır:

* Kullanım **Denetim dosyalarını birleştirmek** SQL Server Management Studio'da (SSMS 17 ile başlayarak):
    1. SSMS menüden **dosya** > **açık** > **Denetim dosyalarını birleştirmek**.

    2. **Denetim dosyalarını eklemek** iletişim kutusu açılır. Birini **Ekle** denetim dosyalar yerel diskten birleştirme ya da Azure Depolama'ya aktarın seçmek için seçenekleri. Azure depolama ayrıntıları ve hesap anahtarınızı sağlamanız gerekir.

    3. Birleştirmek için tüm dosyaları ekledikten sonra tıklayın **Tamam** birleştirme işlemi tamamlamak için.

    4. Ssms'de, burada, görüntülemek ve analiz edin, yapabilir XEL'e bakın veya CSV dosyasına veya bir tabloya dışarı birleştirilmiş dosyayı açar.

* Kullanım [eşitleme uygulama](https://github.com/Microsoft/Azure-SQL-DB-auditing-OMS-integration) , oluşturduk. Bu, Azure'da çalışan ve SQL denetim günlüklerini Log Analytics'e göndermek için Log Analytics genel API'leri kullanır. Eşitleme uygulama SQL denetim günlüklerini Log Analytics'e tüketimi için Log Analytics Panosu iter.

* Power BI'ı kullanın. Görüntüleyebilir ve denetim günlüğü verilerini Power bı'da çözümleyin. Daha fazla bilgi edinin [Power BI ve indirilebilir bir şablon erişim](https://blogs.msdn.microsoft.com/azuresqldbsupport/2017/05/26/sql-azure-blob-auditing-basic-power-bi-dashboard/).

* Azure depolama blob kapsayıcısını Portalı aracılığıyla ya da bir aracı gibi kullanarak günlük dosyalarını indirin [Azure Depolama Gezgini](http://storageexplorer.com/).
    * Günlük dosyasını yerel olarak indirdikten sonra ssms'de günlüklerini çözümleme açmak ve görüntülemek için dosyayı çift tıklayabilirsiniz.
    * Ayrıca, Azure Depolama Gezgini aracılığıyla aynı anda birden çok dosyayı indirebilirsiniz. Belirli bir alt klasörü sağ tıklayıp **Kaydet** yerel bir klasöre kaydedin.

* Ek yöntemleri:
   * Çeşitli dosyalar veya günlük dosyaları içeren alt indirdikten sonra bunları yerel olarak daha önce açıklanan SSMS birleştirme Denetim dosyalarını yönergelerinde açıklandığı birleştirebilirsiniz.

   * Blob görünümü denetimi programlı olarak kaydeder:

     * Kullanım [genişletilmiş olaylar okuyucu](https://blogs.msdn.microsoft.com/extended_events/2011/07/20/introducing-the-extended-events-reader/) C# Kitaplığı.
     * [Sorgu genişletilmiş olaylar dosyaları](https://sqlscope.wordpress.com/2014/11/15/reading-extended-event-files-using-client-side-tools-only/) PowerShell kullanarak.



<br>
### <a name="database-level-policy-audit-logs"></a>Veritabanı düzeyinde İlkesi denetim günlükleri
Veritabanı düzeyinde denetim günlüklerini toplu Store tablolarla koleksiyonundaki bir **SQLDBAuditLogs** Kurulum sırasında seçtiğiniz Azure depolama hesabı ön eki. Günlük dosyaları gibi bir araç kullanarak görüntüleyebileceğiniz [Azure Depolama Gezgini](http://azurestorageexplorer.codeplex.com).

Önceden yapılandırılmış Pano rapor şablonu olarak kullanılabilir bir [indirilebilir Excel elektronik tablosu](https://go.microsoft.com/fwlink/?LinkId=403540) günlük verilerinin hızla analiz etmenize yardımcı olmak için. Denetim günlüklerinizi şablonu kullanmak için Excel 2013 veya üzeri ile yapabilecekleriniz Power Query ihtiyacınız [buradan indirin](https://www.microsoft.com/download/details.aspx?id=39379).

Şablon kurgusal örnek verileri içerdiğinden ve denetim günlüğünüzün doğrudan Azure depolama hesabınızdan içeri aktarmak için Power Query ' ayarlayabilirsiniz.

## <a id="subheading-4"></a>Depolama anahtarını yeniden üretme
Üretim ortamında, depolama anahtarlarınızı düzenli aralıklarla yenileyin olasılığı düşüktür. Anahtarlarınızı yenilerken, ilkeyi kaydetmek gerekir. İşlemi aşağıdaki gibidir:

1. Bölüm denetimi önceki kurulumunda açıklanan yapılandırma Panosu denetimde değiştirme **depolama erişim anahtarı** gelen *birincil* için *ikincil* ve  **Kaydet**.

   ![][4]
2. Depolama yapılandırması Masası'na gidin ve **yeniden** *birincil erişim anahtarı*.
3. Denetim yapılandırma paneline dönün
4. geçiş **depolama erişim anahtarı** gelen *ikincil* için *birincil* basın **Kaydet**.
4. UI depolama geri gidin ve **yeniden** *ikincil erişim anahtarı* (sonraki için hazırlık olarak anahtarlarını yenileme döngüsü.

## <a id="subheading-5"></a>Otomasyon (PowerShell/REST API'si)
Ayrıca, aşağıdaki Otomasyon araçları kullanarak Azure SQL veri ambarı'nda denetim yapılandırabilirsiniz:

* **PowerShell cmdlet'leri**:

   * [Get-AzureRMSqlDatabaseAuditingPolicy](/powershell/module/azurerm.sql/get-azurermsqldatabaseauditingpolicy)
   * [Get-AzureRMSqlServerAuditingPolicy](/powershell/module/azurerm.sql/Get-AzureRMSqlServerAuditingPolicy)
   * [Remove-AzureRMSqlDatabaseAuditing](/powershell/module/azurerm.sql/Remove-AzureRMSqlDatabaseAuditing)
   * [Remove-AzureRMSqlServerAuditing](/powershell/module/azurerm.sql/Remove-AzureRMSqlServerAuditing)
   * [Set-AzureRMSqlDatabaseAuditingPolicy](/powershell/module/azurerm.sql/Set-AzureRMSqlDatabaseAuditingPolicy)
   * [Set-AzureRMSqlServerAuditingPolicy](/powershell/module/azurerm.sql/Set-AzureRMSqlServerAuditingPolicy)
   * [Use-AzureRMSqlServerAuditingPolicy](/powershell/module/azurerm.sql/Use-AzureRMSqlServerAuditingPolicy)


## <a name="downlevel-clients-support-for-auditing-and-dynamic-data-masking"></a>Ve dinamik veri maskeleme için alt düzey istemci desteği
TDS yeniden yönlendirmeyi destekleyen SQL istemcilerinin denetim çalışır.

TDS 7.4 uygulayan herhangi bir istemciyi yeniden yönlendirme de desteklemelidir. Bunun istisnası, JDBC, yeniden yönlendirme özelliği tam olarak desteklenmiyor ve hangi yeniden yönlendirmesi Node.JS için Tedious uygulanmadı 4.0 içerir.

"Alt düzey istemciler" için TDS sürüm 7.3 destekler ve aşağıdaki bağlantı dizesinde sunucunun FQDN şu şekilde değiştirin:

- Bağlantı dizesindeki özgün sunucunun FQDN: <*sunucu adı*>. database.windows.net
- Bağlantı dizesindeki değiştirilmiş sunucusu FQDN'si: <*sunucu adı*> .database. **güvenli**. windows.net

"Alt düzey istemciler" kısmi bir listesine içerir:

* .NET 4.0 ve aşağıdaki
* ODBC 10.0 ve altı.
* JDBC (JDBC TDS 7.4 desteklese de TDS yeniden yönlendirme özelliği tam olarak desteklenmiyor)
* (Node.JS için) tedious

**Açıklama:** Önceki sunucunun FQDN değişikliği her bir veritabanındaki (geçici azaltma) bir yapılandırma adımı gerek olmadan bir SQL sunucu düzeyi denetimi ilkesi uygulamak için de yararlı olabilir.     




<!--Anchors-->
[Database Auditing basics]: #subheading-1
[Set up auditing for your database]: #subheading-2
[Analyze audit logs and reports]: #subheading-3
[Define server-level vs. database-level auditing policy]: #subheading-4
[Set up server-level auditing for all databases]: #subheading-5

<!--Image references-->
[1]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing.png
[2]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-inherit.png
[3]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-enable.png
[4]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-storage-account.png
[5]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-dashboard.png
[6]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-server_1_overview.png
[7]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-server_2_enable.png
[8]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-server_3_storage.png
