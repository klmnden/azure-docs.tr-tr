---
title: Azure SQL veritabanı denetimini kullanmaya başlama | Microsoft Docs
description: Azure SQL veritabanı denetimi veritabanı olaylarını bir denetim günlüğüne izlemek için kullanın.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: vainolo
ms.author: arib
ms.reviewer: vanto
manager: craigg
ms.date: 04/16/2019
ms.openlocfilehash: 3efdf5c256a22529c9d19e9ae1dce5d2db9516a5
ms.sourcegitcommit: be9fcaace62709cea55beb49a5bebf4f9701f7c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65827765"
---
# <a name="get-started-with-sql-database-auditing"></a>SQL veritabanı denetimini kullanmaya başlayın

Azure için Denetim [SQL veritabanı](sql-database-technical-overview.md) ve [SQL veri ambarı](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) veritabanı olaylarını izler ve bir denetim günlüğüne, Azure depolama hesabı, OMS çalışma alanına veya olay hub'ları yazar. Ayrıca denetleme:

- Mevzuatla uyumluluk, veritabanı etkinliğini anlama ve tutarsızlıklar ve işletme sorunlarını veya şüpheli güvenlik ihlallerini anomalileri kavramanıza yardımcı olur.

- Etkinleştirir ve uyumluluk garanti etmez ancak uyumluluk standardını da kıldığı kolaylaştırır. Azure hakkında daha fazla bilgi bu standartlara uyumluluk programları için bkz: [Azure Güven Merkezi](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942) burada bulabilirsiniz SQL veritabanı uyumluluk sertifikaları en güncel listesi.


> [!NOTE] 
> Bu konu başlığı, Azure SQL sunucusunun yanı sıra Azure SQL sunucusu üzerinde oluşturulmuş olan SQL Veritabanı ve SQL Veri Ambarı veritabanları için de geçerlidir. Kolaylık açısından, hem SQL Veritabanı hem de SQL Veri Ambarı için SQL Veritabanı terimi kullanılmaktadır.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]


## <a id="subheading-1"></a>Azure SQL veritabanı denetimi genel bakış

SQL veritabanı denetimi kullanabilirsiniz:

- **Korumak** seçili olayların bir denetim kaydı. Denetlenecek veritabanı eylemlerin kategoriler tanımlayabilirsiniz.
- **Rapor** veritabanı etkinlikleri. Etkinlik ve olay Raporlama ile hızla gerçekleştirmek için önceden yapılandırılmış raporları ve panoyu kullanabilirsiniz.
- **Analiz** raporlar. Şüpheli olayları, olağan dışı etkinliği ve eğilimleri bulabilirsiniz.

> [!IMPORTANT]
> Denetim günlüklerine yazılır **ekleme Blobları** Azure aboneliğinizde Azure Blob Depolama alanında.
>
> - Tüm depolama türü (v1, v2, blob) desteklenir.
> - Tüm depolama çoğaltması yapılandırmaları desteklenir.
> - **Premium depolama** şu anda **desteklenmiyor**.
> - **Sanal ağ içindeki depolama** şu anda **desteklenmiyor**.
> - **Bir güvenlik duvarının arkasındaki depolama** şu anda **desteklenmiyor**

## <a id="subheading-8"></a>Sunucu düzeyinde ve veritabanı düzeyinde denetim ilkesi tanımlama

Bir denetim ilkesi, ilke varsayılan sunucu olarak veya belirli bir veritabanı için tanımlanabilir:

- Bir sunucu ilkesi sunucusundaki tüm mevcut ve yeni oluşturulan veritabanları için geçerlidir.

- Varsa *sunucu blob denetimi etkinse*, onu *her zaman veritabanına uygulanır*. Veritabanı denetim ayarları bağımsız olarak veritabanı denetlenecektir.

- Veritabanını veya veri ambarını BLOB denetimi etkinleştirme, sunucuda etkinleştirmenin yanı sıra mu *değil* geçersiz kılabilir veya sunucu blob denetimi ayarlarından herhangi birini değiştirin. Her iki denetimleri yan yana bulunur. Diğer bir deyişle, veritabanı iki kez paralel olarak denetlenir; Sunucu İlkesi ve bir kez veritabanı İlkesi tarafından bir kez.

   > [!NOTE]
   > Sunucu blob denetimi hem veritabanı birlikte blob denetimi etkinleştirmelerini kaçınmanız gerekir:
    > - Farklı bir kullanmak istediğiniz *depolama hesabı* veya *saklama süresi* belirli bir veritabanı için.
    > - Olay türleri ya da sunucu üzerindeki bir veritabanına geri kalanından farklı kategorilere belirli bir veritabanı için Denetim istiyorsunuz. Örneğin, yalnızca belirli bir veritabanı için denetlenmesi gereken tablo eklemeleri olabilir.
   >
   > Aksi takdirde, yalnızca sunucu düzeyinde blob denetimi etkinleştirme ve devre dışı tüm veritabanları için veritabanı düzeyi denetimi bırakın önerilir.

## <a id="subheading-2"></a>Veritabanınız için denetimi ayarlamanız

Aşağıdaki bölümde, Denetim Azure portalını kullanarak yapılandırmayı açıklar.

1. [Azure Portal](https://portal.azure.com) gidin.
2. Gidin **denetim** , SQL veritabanı/sunucu bölmesinde güvenlik başlığı.

    <a id="auditing-screenshot"></a>![Gezinti Bölmesi][1]

3. Bir sunucu Denetim İlkesi ayarlamak isterseniz, seçebileceğiniz **sunucu ayarlarını görüntüleme** bağlantısı veritabanı denetim sayfası. Ardından görüntüleyebilir veya sunucunun denetim ayarları değiştirebilirsiniz. Sunucu denetim ilkeleri bu sunucudaki tüm mevcut ve yeni oluşturulan veritabanları için geçerlidir.

    ![Gezinti bölmesi][2]

4. Veritabanı düzeyinde denetimini etkinleştirmek isterseniz, geçiş **denetim** için **ON**.

    Sunucu denetimi etkinse, veritabanı ile yapılandırılmış denetim yan yana sunucu denetimi ile bulunur.

    ![Gezinti bölmesi][3]

5. **Yeni** -denetim günlükleri yazılacağı şimdi yapılandırmak için birçok seçeneğiniz vardır. Bir Azure depolama hesabına, Azure İzleyici günlüklerine tüketimi için bir Log Analytics çalışma alanına veya olay hub'ına olay hub'ı kullanarak tüketimi için günlükleri yazabilirsiniz. Her denetim günlüklerine yazılır ve bu seçenekleri herhangi bir birleşimini yapılandırabilirsiniz.

   > [!WARNING]
   > Log Analytics için denetimi etkinleştirme alımı ücretlerine göre ücret ödenmesini gerektirir. Lütfen bu kullanarak ilişkili maliyetini dikkat edilmesi [seçeneği](https://azure.microsoft.com/pricing/details/monitor/), veya denetim depolama, bir Azure depolama hesabında oturum göz önünde bulundurun.

    ![Depolama Seçenekleri](./media/sql-database-auditing-get-started/auditing-select-destination.png)

6. Yazma denetim yapılandırmak için günlükleri bir depolama hesabına, select **depolama** açın **depolama ayrıntıları**. Burada günlükleri kaydedileceği konum ve saklama süresi ardından Azure depolama hesabını seçin. Eski olan günlükler silinir. Daha sonra, **Tamam**'a tıklayın.

    ![depolama hesabı](./media/sql-database-auditing-get-started/auditing_select_storage.png)

7. Yazma denetim yapılandırmak için bir Log Analytics çalışma alanına, select günlükleri **Log Analytics (Önizleme)** açın **Log Analytics ayrıntıları**. Seçin veya burada günlüklerine yazılır ve ardından Log Analytics çalışma alanı **Tamam**.

    ![Log Analytics çalışma alanı](./media/sql-database-auditing-get-started/auditing_select_oms.png)

8. Yazma denetim yapılandırmak için bir olay hub'ına select günlükleri **olay hub'ı (Önizleme)** açın **olay hub'ı ayrıntılarını**. Burada günlüklerine yazılır ve ardından olay hub'ı seçin **Tamam**. Olay hub'ı veritabanı ve sunucu aynı bölgede olduğundan emin olun.

    ![Olay hub'ı](./media/sql-database-auditing-get-started/auditing_select_event_hub.png)

9. **Kaydet**’e tıklayın.
10. Denetlenen olayları özelleştirmek isterseniz, bunu aracılığıyla yapabilirsiniz [PowerShell cmdlet'leri](#subheading-7) veya [REST API](#subheading-9).
11. Denetim ayarlarınızı yapılandırdıktan sonra yeni tehdit algılama özelliğini açmak ve güvenlik uyarıları alacak e-postalar yapılandırın. Tehdit algılama kullandığınızda, olası güvenlik tehditlerini gösteren anormal veritabanı etkinliklerini etkin uyarılar alırsınız. Daha fazla bilgi için [tehdit algılamayı kullanmaya başlama](sql-database-threat-detection-get-started.md).

> [!IMPORTANT]
> Duraklatılmış bir Azure SQL veri ambarı denetlemeyi etkinleştirme mümkün değildir. Bunu etkinleştirmek için duraklatma kaldırın, veri ambarı.

> [!WARNING]
> Azure SQL veri ambarı olan bir sunucuyu denetimi etkinleştirme **olduğu veri ambarı yeniden yeniden duraklatıldı ve devam ettirilmesini sonuçlanır** , faturalandırma ücretleri tabi.

## <a id="subheading-3"></a>Denetim günlüklerini ve raporları analiz edin

Azure İzleyici günlüklerine denetim günlüklerini yazma izni seçerseniz:

- Kullanım [Azure portalında](https://portal.azure.com).  İlgili veritabanı açın. Veritabanının üst kısmındaki **denetim** sayfasında **denetim günlüklerini görüntüle**.

    ![Denetim günlüklerini görüntüle](./media/sql-database-auditing-get-started/7_auditing_get_started_blob_view_audit_logs.png)

- Ardından tıklayarak **OMS'de açın** en üstündeki **Denetim kayıtlarını** sayfa burada özelleştirebilirsiniz zaman aralığını ve arama sorgusu Log Analytics'te günlükleri görünümü açılır.

    ![Log Analytics'i Aç](./media/sql-database-auditing-get-started/auditing_open_in_oms.png)

- Alternatif olarak, denetim günlüklerini Log Analytics dikey penceresinden de erişebilirsiniz. Log Analytics çalışma alanınızın açın ve altında **genel** bölümünde **günlükleri**. Gibi basit bir sorgu başlatın: *"SQLSecurityAuditEvents" arama* denetim görüntülemek üzere günlüğe kaydeder.
    Burada da kullanabilirsiniz [Azure İzleyici günlükleri](../log-analytics/log-analytics-log-search.md) Gelişmiş aramaları, Denetim günlüğü verileri temelinde çalıştırılacak. Azure İzleyici günlüklerine tüm iş yüklerinizde ve sunucularınızda milyonlarca kaydı kolayca analiz etmek için tümleşik arama ve özel panoları kullanarak gerçek zamanlı operasyonel içgörüler sunar. Azure İzleyici günlüklerine arama dili ve komutlar hakkında başka yararlı bilgiler için bkz. [Azure İzleyici günlüklerini Arama başvurusu](../log-analytics/log-analytics-log-search.md).

Denetim günlükleri Olay Hub'ına yazma seçerseniz:

- Denetim günlükleri verileri olay hub'ı kullanmak için olayları kullanma ve bir hedef yazmak için bir akış ayarlamanız gerekir. Daha fazla bilgi için [Azure Event Hubs belgeleri](https://docs.microsoft.com/azure/event-hubs/).
- Denetim günlükleri Olay Hub'ındaki gövdesinde yakalanan [Apache Avro](https://avro.apache.org/) olayları ve UTF-8 kodlaması ile JSON biçimlendirme kullanılarak depolanabilir. Denetim günlüklerini okumak için kullanabileceğiniz [Avro Araçları](https://docs.microsoft.com/azure/event-hubs/event-hubs-capture-overview#use-avro-tools) veya bu biçim işleme benzer araçları.

Denetim günlükleri bir Azure depolama hesabına yazma seçerseniz, günlükleri görüntülemek için kullanabileceğiniz birkaç yöntem vardır:

- Denetim günlükleri, Kurulum sırasında seçtiğiniz hesabında toplanır. Denetim günlükleri gibi bir araç kullanarak keşfedebilirsiniz [Azure Depolama Gezgini](https://storageexplorer.com/). Azure depolama alanında, Denetim günlükleri adlı bir kapsayıcı içinde blob dosyaları koleksiyonu olarak kaydedilir **sqldbauditlogs**. Depolama klasörü hiyerarşisi hakkında daha fazla ayrıntı için bkz: adlandırma kuralları ve günlük biçimi, [Blob denetim günlük biçimi başvurusu](https://go.microsoft.com/fwlink/?linkid=829599).

- Kullanım [Azure portalında](https://portal.azure.com).  İlgili veritabanı açın. Veritabanının üst kısmındaki **denetim** sayfasında **denetim günlüklerini görüntüle**.

    ![Gezinti bölmesi][7]

    **Denetim kayıtları** içinden şunları yapabileceksiniz günlükleri görüntülemek, açılır.

  - Belirli tarihleri tıklatarak görüntüleyebileceğiniz **filtre** en üstündeki **Denetim kayıtlarını** sayfası.
  - Tarafından oluşturulan denetim kayıtları arasında geçiş yapabilirsiniz *sunucu denetim ilkesi* ve *veritabanı Denetim İlkesi* geçiş yaparak **kaynak denetim**.
  - SQL ekleme Denetim kayıtlarını denetleyerek ilgili yalnızca görüntüleyebileceğiniz **Show yalnızca SQL ekleme kayıtlarını denetim** onay kutusu.

       ![Gezinti bölmesi][8]

- Sistem işlevi kullanın **sys.fn_get_audit_file** (Denetim günlüğü verileri tablo biçiminde döndürmek için T-SQL). Bu işlev kullanma hakkında daha fazla bilgi için bkz. [sys.fn_get_audit_file](https://docs.microsoft.com/sql/relational-databases/system-functions/sys-fn-get-audit-file-transact-sql).

- Kullanım **Denetim dosyalarını birleştirmek** SQL Server Management Studio'da (SSMS 17 ile başlayarak):
    1. SSMS menüden **dosya** > **açık** > **Denetim dosyalarını birleştirmek**.

        ![Gezinti bölmesi][9]
    2. **Denetim dosyalarını eklemek** iletişim kutusu açılır. Birini **Ekle** denetim dosyalar yerel diskten birleştirme ya da Azure Depolama'ya aktarın seçmek için seçenekleri. Azure depolama ayrıntıları ve hesap anahtarınızı sağlamanız gerekir.

    3. Birleştirmek için tüm dosyaları ekledikten sonra tıklayın **Tamam** birleştirme işlemi tamamlamak için.

    4. Ssms'de, burada, görüntülemek ve analiz edin, yapabilir XEL'e bakın veya CSV dosyasına veya bir tabloya dışarı birleştirilmiş dosyayı açar.

- Power BI'ı kullanın. Görüntüleyebilir ve denetim günlüğü verilerini Power bı'da çözümleyin. Daha fazla bilgi ve indirilebilir bir şablon erişmek için bkz: [Çözümle denetim günlüğü verilerini Power bı'da](https://blogs.msdn.microsoft.com/azuresqldbsupport/20../../sql-azure-blob-auditing-basic-power-bi-dashboard/).
- Azure depolama blob kapsayıcısını Portalı aracılığıyla ya da bir aracı gibi kullanarak günlük dosyalarını indirin [Azure Depolama Gezgini](https://storageexplorer.com/).
  - Günlük dosyasını yerel olarak indirdikten sonra ssms'de günlüklerini çözümleme açmak ve görüntülemek için dosyaya çift tıklayın.
  - Ayrıca, Azure Depolama Gezgini aracılığıyla aynı anda birden çok dosyayı indirebilirsiniz. Bunu yapmak için belirli bir alt klasörü sağ tıklatın ve seçin **Kaydet** yerel bir klasöre kaydedin.

- Ek yöntemleri:

  - Çeşitli dosyalar veya günlük dosyaları içeren alt indirdikten sonra bunları yerel olarak daha önce açıklanan SSMS birleştirme Denetim dosyalarını yönergelerinde açıklandığı birleştirebilirsiniz.
  - Blob görünümü denetimi programlı olarak kaydeder:

    - [Sorgu genişletilmiş olaylar dosyaları](https://sqlscope.wordpress.com/20../../reading-extended-event-files-using-client-side-tools-only/) PowerShell kullanarak.

## <a id="subheading-5"></a>Üretim uygulamaları

<!--The description in this section refers to preceding screen captures.-->

### <a id="subheading-6">Coğrafi çoğaltmalı veritabanı denetimi</a>

Birincil veritabanında Denetimi etkinleştirdiğinizde, coğrafi olarak çoğaltılmış veritabanları ile ikincil veritabanı özdeş bir denetim ilkesi gerekir. İkincil veritabanı üzerinde denetim sağlayarak denetimi ayarlamanız mümkündür **ikincil sunucu**, birincil veritabanından bağımsız olarak.

- Sunucu düzeyinde (**önerilen**): Hem de denetimi açma **birincil sunucu** yanı sıra **ikincil sunucu** -birincil ve ikincil veritabanları her bağımsız olarak kendi ilgili sunucu düzeyindeki ilke göre denetlenir.
- Veritabanı düzeyinde: İkincil veritabanları için veritabanı düzeyi denetimi yalnızca birincil veritabanı denetim ayarları'ndan yapılandırılabilir.
  - Denetim etkinleştirilmelidir *birincil veritabanının kendisi*, sunucu değil.
  - Birincil veritabanında denetimi etkinleştirildikten sonra ayrıca ikincil veritabanı üzerinde etkin hale.

    >[!IMPORTANT]
    >Veritabanı düzeyi denetimi ile ikincil veritabanı için depolama ayarlarını bölgeler arası trafik neden birincil veritabanı için aynı olacaktır. Yalnızca sunucu düzeyi denetimi etkinleştirin ve devre dışı tüm veritabanları için veritabanı düzeyi denetimi bırakın öneririz.
    > [!WARNING]
    > Olay hub'ı veya Azure İzleyici günlüklerine sunucu düzeyinde denetim günlükleri için hedef kullanarak ikincil coğrafi çoğaltmalı veritabanı şu anda desteklenmiyor.

### <a id="subheading-6">Depolama anahtarını yeniden üretme</a>

Üretim ortamında, depolama anahtarlarınızı düzenli aralıklarla yenileyin olasılığı düşüktür. Denetim günlükleri, Azure depolama alanına yazarken, denetim ilkenizi anahtarlarınızı yenilerken kaydetmeniz gerekir. İşlemi aşağıdaki gibidir:

1. Açık **depolama ayrıntıları**. İçinde **depolama erişim anahtarı** kutusunda **ikincil**, tıklatıp **Tamam**. Ardından **Kaydet** denetim yapılandırma sayfanın üstünde.

    ![Gezinti bölmesi][5]
2. Depolama Yapılandırması sayfasına gidin ve birincil erişim tuşunu yeniden oluşturun.

    ![Gezinti bölmesi][6]
3. İkincil siteden birincil depolama erişim anahtarı yeniden denetim yapılandırma sayfasına geçin ve ardından Git **Tamam**. Ardından **Kaydet** denetim yapılandırma sayfanın üstünde.
4. Depolama Yapılandırması sayfasına dönün ve (hazırlığında sonraki anahtarın yenileme döngüsü) ikincil erişim tuşunu yeniden oluşturun.

## <a name="additional-information"></a>Ek Bilgiler

- Günlük hakkındaki ayrıntılar için biçimi, depolama klasör hiyerarşisini ve adlandırma kuralları için bkz: [Blob denetim günlük biçimi başvurusu](https://go.microsoft.com/fwlink/?linkid=829599).

    > [!IMPORTANT]
    > Azure SQL veritabanı ve denetim karakteri alanlar için veri 4000 karakter bir denetim kaydı depolar. Zaman **deyimi** veya **data_sensitivity_information** denetlenebilir bir eylemin getirdiği en çok 4000 karakter içeriyordur, ilk 4000 karakterden ötesinde herhangi bir veri  **kesilmiş ve değil denetlenen**.

- Denetim günlüklerine yazılır **ekleme Blobları** Azure aboneliğinizde bir Azure Blob Depolama:
  - **Premium depolama** şu anda **desteklenmiyor** tarafından ekleme Blobları.
  - **Sanal ağ içindeki depolama** şu anda **desteklenmiyor**.

- Varsayılan Denetim İlkesi, tüm eylemleri ve tüm sorguların ve saklanan yordamların veritabanı hem de karşı başarılı ve başarısız oturum açma bilgileri yürütülen denetleyeceksiniz Eylem grupları, aşağıdaki kümesi içerir:

    BATCH_COMPLETED_GROUP<br>
    SUCCESSFUL_DATABASE_AUTHENTICATION_GROUP<br>
    FAILED_DATABASE_AUTHENTICATION_GROUP

    Bölümünde anlatıldığı gibi eylemleri ve Eylem grupları, PowerShell kullanarak farklı türleri için denetimi yapılandırabilirsiniz [Azure PowerShell kullanarak yönetme SQL veritabanı denetimi](#subheading-7) bölümü.

- AAD kimlik doğrulamasını kullanırken oturum açma bilgileri kayıt olacak başarısız *değil* SQL denetim günlüğünde görünür. Başarısız oturum açma Denetim kayıtlarını görüntülemek için ziyaret etmesi gerekir [Azure Active Directory portalında]( ../active-directory/reports-monitoring/reference-sign-ins-error-codes.md), bu olayların ayrıntıları kaydeder.


## <a id="subheading-7"></a>Azure PowerShell kullanarak SQL veritabanı denetimini yönetme

**PowerShell cmdlet'leri (WHERE yan tümcesi desteği, ek bir filtreleme dahil)**:

- [Veritabanı Denetim İlkesi (Set-AzSqlDatabaseAuditing) güncelle](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabaseauditing)
- [Sunucu denetimi İlkesi (Set-AzSqlServerAuditing) güncelle](https://docs.microsoft.com/powershell/module/az.sql/set-azsqlserverauditing)
- [Veritabanı Denetim İlkesi alın (Get-AzSqlDatabaseAuditing)](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabaseauditing)
- [Denetim İlkesi (Get-AzSqlServerAuditing) Server'ı edinin](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlserverauditing)

Bir komut dosyası örneği için bkz [PowerShell kullanarak denetim ve tehdit algılamayı yapılandırma](scripts/sql-database-auditing-and-threat-detection-powershell.md).

## <a id="subheading-9"></a>REST API'sini kullanarak SQL veritabanı denetimini yönetme

**REST API**:

- [Veritabanı Denetim İlkesi güncelle](https://docs.microsoft.com/rest/api/sql/database%20auditing%20settings/createorupdate)
- [Oluşturma veya güncelleştirme Denetim ilkesi sunucusu](https://docs.microsoft.com/rest/api/sql/server%20auditing%20settings/createorupdate)
- [Veritabanı denetim ilkesi alma](https://docs.microsoft.com/rest/api/sql/database%20auditing%20settings/get)
- [Denetim İlkesi Server'ı edinin](https://docs.microsoft.com/rest/api/sql/server%20auditing%20settings/get)

Burada yan tümcesi destek ek filtreleme ile genişletilmiş İlkesi:

- [Oluşturma veya güncelleştirme veritabanı *Genişletilmiş* Denetim İlkesi](https://docs.microsoft.com/rest/api/sql/database%20extended%20auditing%20settings/createorupdate)
- [Oluşturma veya güncelleştirme sunucusu *Genişletilmiş* Denetim İlkesi](https://docs.microsoft.com/rest/api/sql/server%20auditing%20settings/createorupdate)
- [Veritabanı Al *Genişletilmiş* Denetim İlkesi](https://docs.microsoft.com/rest/api/sql/database%20extended%20auditing%20settings/get)
- [Sunucu Al *Genişletilmiş* Denetim İlkesi](https://docs.microsoft.com/rest/api/sql/server%20auditing%20settings/get)

## <a id="subheading-10"></a>ARM şablonları kullanarak SQL veritabanı denetimini yönetme

Kullanarak Azure SQL veritabanı denetimi yönetebileceğiniz [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) Bu örneklerde gösterildiği gibi şablonları:

- [Denetim günlükleri, Azure Blob Depolama hesabına yazma etkinleştirilmiş denetim ile bir Azure SQL sunucusu dağıtma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-sql-auditing-server-policy-to-blob-storage)
- [Denetim günlüklerini Log Analytics'e yazmak için etkin bir denetim ile bir Azure SQL sunucusu dağıtma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-sql-auditing-server-policy-to-oms)
- [Denetim günlükleri Olay hub'ları yazmak için etkin bir denetim ile bir Azure SQL sunucusu dağıtma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-sql-auditing-server-policy-to-eventhub)

> [!NOTE]
> Bağlantılı örnekleri üzerinde ortak dış depodan olan ve 'olarak Is', sağlanan garanti olmadan ve tüm Microsoft destek programı/hizmeti altında desteklenmez.

<!--Anchors-->
[Azure SQL Database Auditing overview]: #subheading-1
[Set up auditing for your database]: #subheading-2
[Analyze audit logs and reports]: #subheading-3
[Practices for usage in production]: #subheading-5
[Storage Key Regeneration]: #subheading-6
[Manage SQL database auditing using Azure PowerShell]: #subheading-7
[Blob/Table differences in Server auditing policy inheritance]: (#subheading-8)
[Manage SQL database auditing using REST API]: #subheading-9
[Manage SQL database auditing using ARM templates]: #subheading-10

<!--Image references-->
[1]: ./media/sql-database-auditing-get-started/1_auditing_get_started_settings.png
[2]: ./media/sql-database-auditing-get-started/2_auditing_get_started_server_inherit.png
[3]: ./media/sql-database-auditing-get-started/3_auditing_get_started_turn_on.png
[4]: ./media/sql-database-auditing-get-started/4_auditing_get_started_storage_details.png
[5]: ./media/sql-database-auditing-get-started/5_auditing_get_started_storage_key_regeneration.png
[6]: ./media/sql-database-auditing-get-started/6_auditing_get_started_regenerate_key.png
[7]: ./media/sql-database-auditing-get-started/7_auditing_get_started_blob_view_audit_logs.png
[8]: ./media/sql-database-auditing-get-started/8_auditing_get_started_blob_audit_records.png
[9]: ./media/sql-database-auditing-get-started/9_auditing_get_started_ssms_1.png
[10]: ./media/sql-database-auditing-get-started/10_auditing_get_started_ssms_2.png