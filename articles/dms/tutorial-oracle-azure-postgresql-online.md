---
title: 'Öğretici: PostgreSQL için Azure veritabanı, Oracle, çevrimiçi bir geçiş gerçekleştirmek için Azure veritabanı geçiş hizmeti kullanın. | Microsoft Docs'
description: Bir çevrimiçi geçiş Oracle şirket içi veya sanal makinelerde Azure veritabanı PostgreSQL için Azure veritabanı geçiş hizmeti kullanarak gerçekleştirmeyi öğrenin.
services: dms
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: article
ms.date: 05/06/2019
ms.openlocfilehash: 6f94fa8b5c0d972d9cdbe86c480a712f7e44c29f
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65158339"
---
# <a name="tutorial-migrate-oracle-to-azure-database-for-postgresql-online-using-dms-preview"></a>Öğretici: Oracle, PostgreSQL için Azure veritabanı'na geçirme çevrimiçi DMS (Önizleme) kullanma

Azure veritabanı geçiş hizmeti veritabanlarını Oracle veritabanları barındırılan şirket içi veya sanal makineleri geçirmek için kullanabileceğiniz [PostgreSQL için Azure veritabanı](https://docs.microsoft.com/azure/postgresql/) en düşük kapalı kalma süresi. Diğer bir deyişle, uygulamanın en düşük kapalı kalma süresiyle geçiş tamamlayabilirsiniz. Bu öğreticide, geçiş **ik** örnek veritabanını bir şirket içi veya sanal makine örneği Oracle 11 g PostgreSQL için Azure veritabanı için Azure veritabanı geçiş hizmetinin çevrimiçi geçiş etkinliğini kullanarak.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Ora2pg Aracı'nı kullanarak geçiş çaba değerlendirin.
> * Örnek şemanın ora2pg Aracı'nı kullanarak geçirin.
> * Azure veritabanı geçiş Hizmeti'nin bir örneğini oluşturun.
> * Azure veritabanı geçiş hizmeti kullanarak bir geçiş projesi oluşturun.
> * Geçişi çalıştırma.
> * Geçişi izleme.

> [!NOTE]
> Azure veritabanı geçiş hizmeti çevrimiçi bir geçiş gerçekleştirmek için Premium fiyatlandırma katmanını temel alan bir örneği oluşturmanız gerekir.

> [!IMPORTANT]
> En iyi geçiş deneyimi için Microsoft, hedef veritabanı ile aynı Azure bölgesindeki Azure veritabanı geçiş hizmeti örneği oluşturmayı önerir. Verileri bölgeler veya coğrafyalar arasında taşımak, geçiş sürecini yavaşlatabilir ve hatalara neden olabilir.

[!INCLUDE [online-offline](../../includes/database-migration-service-offline-online.md)]

Bu makalede, bir çevrimiçi geçiş Oracle'dan PostgreSQL için Azure veritabanı'na gerçekleştirmek açıklar.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakileri yapmanız gerekir:

* İndirme ve yükleme [Oracle 11g sürüm 2 (Standard Edition, sürüm bir standart veya Enterprise Edition)](https://www.oracle.com/technetwork/database/enterprise-edition/downloads/index.html).
* Örneği indirmek **ik** veritabanını [burada](https://docs.oracle.com/database/121/COMSC/installation.htm#COMSC00002).
* Karşıdan yükleyip ora2pg üzerinde [Windows](https://github.com/Microsoft/DataMigrationTeam/blob/master/Whitepapers/Steps%20to%20Install%20ora2pg%20on%20Windows.pdf) veya [Linux](https://github.com/Microsoft/DataMigrationTeam/blob/master/Whitepapers/Steps%20to%20Install%20ora2pg%20on%20Linux.pdf).
* [PostgreSQL için Azure Veritabanı’nda örnek oluşturma](https://docs.microsoft.com/azure/postgresql/quickstart-create-server-database-portal).
* Örneğine bağlanın ve bu yönergeyi kullanarak veritabanı oluşturma [belge](https://docs.microsoft.com/azure/postgresql/tutorial-design-database-using-azure-portal).
* Kullanarak şirket içi kaynak sunucularınıza siteden siteye bağlantı sağlar Azure Resource Manager dağıtım modelini kullanarak bir Azure sanal ağı (VNet) için Azure veritabanı geçiş hizmeti oluşturma [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways). Sanal ağ oluşturma hakkında daha fazla bilgi için bkz. [sanal ağ belgeleri](https://docs.microsoft.com/azure/virtual-network/)ve özellikle adım adım ayrıntılar sağlayan hızlı başlangıç makalelerini.

  > [!NOTE]
  > Microsoft Ağ eşlemesi ile ExpressRoute kullanıyorsanız, sanal ağ kurulumu sırasında şu Hizmet Ekle [uç noktaları](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview) hangi hizmet sağlanacağı alt ağ için:
  > * Hedef veritabanı uç noktası (örneğin, SQL uç noktası, Cosmos DB uç noktası vb.)
  > * Depolama uç noktası
  > * Service bus uç noktası
  >
  > Azure veritabanı geçiş hizmeti internet bağlantısı olmadığı için bu gerekli bir yapılandırmadır.

* VNet ağ güvenlik grubu (NSG) kurallarınız için Azure veritabanı geçiş hizmeti aşağıdaki gelen iletişim bağlantı noktalarını engelleme emin olun: 443, 53, 9354, 445, 12000. Azure VNet NSG trafik filtreleme hakkında daha fazla ayrıntı için bkz [ağ güvenlik grupları ile ağ trafiğini filtreleme](https://docs.microsoft.com/azure/virtual-network/virtual-network-vnet-plan-design-arm).
* [Windows Güvenlik Duvarınızı veritabanı altyapısı erişimi](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access) için yapılandırın.
* Varsayılan olarak TCP bağlantı noktası 1521 kaynak Oracle sunucusuna erişmek Azure veritabanı geçiş hizmeti, Windows Güvenlik Duvarı'nı açın.
* Kaynak veritabanlarınız önünde bir güvenlik duvarı Gereci kullanırken oluşan kaynak veritabanları geçiş için erişmek Azure veritabanı geçiş hizmeti izin vermek için güvenlik duvarı kuralları eklemeniz gerekebilir.
* Sunucu düzeyinde oluşturma [güvenlik duvarı kuralı](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) hedef veritabanları için Azure veritabanı geçiş hizmeti erişmesine izin vermek PostgreSQL için Azure veritabanı. Azure veritabanı geçiş hizmeti için kullanılan sanal ağ alt ağ aralığını belirtin.
* Kaynak Oracle veritabanlarına erişim sağlar.

  > [!NOTE]
  > DBA rol Oracle kaynağına bağlanmak bir kullanıcı için gereklidir.

  * Arşiv Yinele günlükleri, değişiklik verilerini yakalama için Azure veritabanı geçiş hizmeti artımlı eşitleme için gereklidir. Oracle kaynağını yapılandırmak için aşağıdaki adımları izleyin:
    * Aşağıdaki komutu çalıştırarak SYSDBA'ın ayrıcalık kullanarak oturum açın:

      ```
      sqlplus (user)/(password) as sysdba
      ```

    * Aşağıdaki komutu çalıştırarak veritabanı örneği kapatın.

      ```
      SHUTDOWN IMMEDIATE;
      ```

      Onay için bekleyin `'ORACLE instance shut down'`.

    * Etkinleştirmek veya devre dışı aşağıdaki komutu çalıştırarak arşivleme bu veritabanına bağlama ve yeni örnek Başlat (ancak açılmaz):

      ```
      STARTUP MOUNT;
      ```

      Veritabanına bağlı olması gerekir; 'Oracle örneğini kullanmaya' onaylanmasını bekleyin.

    * Aşağıdaki komutu çalıştırarak modu arşivleme veritabanı değiştirin:

      ```
      ALTER DATABASE ARCHIVELOG;
      ```

    * Veritabanı normal işlemler için aşağıdaki komutu çalıştırarak açın:

      ```
      ALTER DATABASE OPEN;
      ```

      Gösterilecek yay dosya için yeniden başlatmanız gerekebilir.

    * Doğrulamak için aşağıdaki komutu çalıştırın:

      ```
      SELECT log_mode FROM v$database;
      ```

      Bir yanıt alması gereken `'ARCHIVELOG'`. Yanıt ise `'NOARCHIVELOG'`, gereksinim karşılanmamış sonra.

  * Aşağıdaki seçeneklerden birini kullanarak çoğaltma için günlüğü ek etkinleştirin.

    * **Seçenek 1**.
      PK ve benzersiz bir dizin ile tüm tabloları karşılamak için veritabanı düzeyinde ek günlük kaydı değiştirin. Algılama sorguyu döndürür `'IMPLICIT'`.

      ```
      ALTER DATABASE ADD SUPPLEMENTAL LOG DATA (PRIMARY KEY, UNIQUE) COLUMNS;
      ```

      Tablo düzeyi ek değişiklik günlüğü. Veri işleme ve BA veya benzersiz dizinler olmayan tablolar için çalıştırın.

      ```
      ALTER TABLE [TABLENAME] ADD SUPPLEMENTAL LOG DATA (ALL) COLUMNS;
      ```

    * **Seçenek 2**.
      Tüm tabloları karşılamak için veritabanı düzeyinde ek günlük kaydı değiştirebilir ve algılama sorgunun döndürdüğü `'YES'`.

      ```
      ALTER DATABASE ADD SUPPLEMENTAL LOG DATA;
      ```

      Tablo düzeyi ek değişiklik günlüğü. Her tablo için yalnızca bir deyim çalıştırmak için aşağıdaki mantığı uygulayın.

      Tabloda birincil anahtar varsa:

      ```
      ALTER TABLE xxx ADD SUPPLEMENTAL LOG DATA (PRIMARY KEY) COLUMNS;
      ```

      Tablonun benzersiz bir dizin varsa:

      ```
      ALTER TABLE xxx ADD SUPPLEMENTAL LOG GROUP (first unique index columns) ALWAYS;
      ```

      Aksi takdirde, aşağıdaki komutu çalıştırın:

      ```
      ALTER TABLE xxx ADD SUPPLEMENTAL LOG DATA (ALL) COLUMNS;
      ```

    Doğrulamak için aşağıdaki komutu çalıştırın:

      ```
      SELECT supplemental_log_data_min FROM v$database;
      ```

    Bir yanıt alması gereken `'YES'`.

> [!IMPORTANT]
> Bu senaryo genel Önizleme sürümü için Azure veritabanı geçiş hizmeti Oracle sürümü destekleyen 10g veya 11g. 12 c ya da üzeri unutmamalısınız Oracle sürümü çalıştıran müşteriler, 8 bağlanmak için Oracle ODBC sürücüsü için izin verilen en düşük kimlik doğrulama protokolü olmalıdır. Bir Oracle kaynağı olan 12 c sürüm veya daha sonra kimlik doğrulama protokolü şu şekilde yapılandırmanız gerekir:
>
> * SQLNET güncelleştirin. ORA:
>
>    ```
>    SQLNET.ALLOWED_LOGON_VERSION_CLIENT = 8
>    SQLNET.ALLOWED_LOGON_VERSION_SERVER = 8
>    ```
>
> * Yeni ayarların etkili olması için bilgisayarınızı yeniden başlatın.
> * Mevcut kullanıcılar için Parola Değiştir:
>
>    ```
>    ALTER USER system IDENTIFIED BY {pswd}
>    ```
>
>   Daha fazla bilgi için bkz: sayfa [burada](http://www.dba-oracle.com/t_allowed_login_version_server.htm).
>
> Son olarak, kimlik doğrulama protokolü değiştirme istemci kimlik doğrulaması etkileyebilir unutmayın.

## <a name="assess-the-effort-for-an-oracle-to-azure-database-for-postgresql-migration"></a>PostgreSQL geçiş için Azure veritabanı Oracle çaba değerlendirmek

Oracle'dan PostgreSQL için Azure veritabanı'na geçirmek için gereken çabayı değerlendirmek için ora2pg kullanmanızı öneririz. Kullanım `ora2pg -t SHOW_REPORT` Oracle nesneleri, tahmin edilen taşıma maliyeti (Geliştirici gün) ve dönüştürme işleminin bir parçası olarak özel dikkat etmeniz gereken bazı veritabanı nesnelerini listeleyen bir rapor oluşturmak için yönergesi.

Çoğu müşteri, değerlendirme raporunu gözden geçirme ve otomatik ve el ile dönüştürme çaba dikkate genişliğinize zaman geçirir.

Yapılandırma ve bir değerlendirme raporu oluşturmak için ora2pg çalıştırma hakkında bilgi için bkz: **Premigration: Değerlendirme** bölümünü [Kitapçığı PostgreSQL için Azure veritabanı, Oracle](https://github.com/Microsoft/DataMigrationTeam/blob/master/Whitepapers/Oracle%20to%20Azure%20PostgreSQL%20Migration%20Cookbook.pdf). Bir örnek ora2pg değerlendirme raporu, başvuru için kullanılabilir [burada](http://ora2pg.darold.net/report.html).

## <a name="export-the-oracle-schema"></a>Oracle şema

PostgreSQL için Azure veritabanı ile uyumlu bir şema için Oracle şema ve diğer Oracle nesneleri (türleri, yordamları, işlevleri, vb.) dönüştürmek için ora2pg kullanmanızı öneririz. ora2pg belirli veri türlerini önceden tanımlamanıza yardımcı olmak için birçok yönergeleri içerir. Örneğin, kullanabileceğiniz `DATA_TYPE` yönergesi tüm NUMBER(*,0) NUMERIC(38) yerine bigint ile değiştirilecek.

Her veritabanı nesnelerinin .sql dosyaları dışarı aktarmak için ora2pg çalıştırabilirsiniz. Ardından, Azure veritabanı'na psql kullanarak PostgreSQL için aktarmadan önce .sql dosyaları inceleyebilirsiniz veya yürütebilirsiniz. PgAdmin SQL betiğinde.

```
psql -f [FILENAME] -h [AzurePostgreConnection] -p 5432 -U [AzurePostgreUser] -d database 
```

Örneğin:

```
psql -f %namespace%\schema\sequences\sequence.sql -h server1-server.postgres.database.azure.com -p 5432 -U username@server1-server -d database
```

Yapılandırma ve şema dönüştürme için ora2pg çalıştırma hakkında bilgi için bkz: **geçiş: Şema ve veri** bölümünü [Kitapçığı PostgreSQL için Azure veritabanı, Oracle](https://github.com/Microsoft/DataMigrationTeam/blob/master/Whitepapers/Oracle%20to%20Azure%20PostgreSQL%20Migration%20Cookbook.pdf).

## <a name="set-up-the-schema-in-azure-database-for-postgresql"></a>PostgreSQL için Azure veritabanı'nda şema ayarlama

PostgreSQL schema.table.column küçük saklar. varsayılan olarak, tüm üst durumlarda schema.table.column Oracle tutar. Azure veritabanı geçiş veri taşıma Oracle'dan PostgreSQL için Azure veritabanına başlamak hizmeti için schema.table.column Oracle kaynak aynı büyük/küçük harf biçiminde olmalıdır.

"S" şeması olarak Oracle kaynak varsa, örneğin,." ÇALIŞANLAR"." EMPLOYEE_ID", ardından PostgreSQL şema aynı biçimi kullanmanız gerekir.

Büyük/küçük harf biçimi schema.table.column Oracle hem PostgreSQL için Azure veritabanı için aynı olduğundan emin olmak için aşağıdaki adımları kullanmanızı öneririz.

> [!NOTE]
> Farklı bir yaklaşım, bir büyük harf şema türetmek için kullanabilirsiniz. Geliştirmek ve bu adımı otomatikleştirmek için çalışıyoruz.

1. Şemaları ile küçük harflerden ora2pg kullanarak dışarı aktarın. Tablo oluşturma sql komut bir şema büyük harf "Şema" ile el ile oluşturun.
2. Tetikleyiciler, dizileri, yordamlar, türleri ve işlevleri gibi Oracle nesneleri geri kalanı, PostgreSQL için Azure veritabanı'na içeri aktarın.
3. Tablo ve sütun üst çalışması yapmak için aşağıdaki betiği çalıştırın:

   ```
   -- INPUT: schema name
   set schema.var = “HR”;

   -- Generate statements to rename tables and columns
   SELECT 1, 'SET search_path = "' ||current_setting('schema.var')||'";'
   UNION ALL 
   SELECT 2, 'alter table "'||c.relname||'" rename '||a.attname||' to "'||upper(a.attname)||'";'
   FROM pg_class c
   JOIN pg_attribute a ON a.attrelid = c.oid
   JOIN pg_type t ON a.atttypid = t.oid
   LEFT JOIN pg_catalog.pg_constraint r ON c.oid = r.conrelid
    AND r.conname = a.attname
   WHERE c.relnamespace = (select oid from pg_namespace where nspname=current_setting('schema.var')) AND a.attnum > 0 AND c.relkind ='r'
   UNION ALL
   SELECT 3, 'alter table '||c.relname||' rename to "'||upper(c.relname)||'";'
   FROM pg_catalog.pg_class c
    LEFT JOIN pg_catalog.pg_namespace n ON n.oid = c.relnamespace
   WHERE c.relkind ='r' AND n.nspname=current_setting('schema.var')
   ORDER BY 1;
   ```

* Çalıştırılacak tam yükleme için hedef veritabanında yabancı anahtarı bırakın. Başvurmak **örnek şema geçişi** makalenin bölümü [burada](https://docs.microsoft.com/azure/dms/tutorial-postgresql-azure-postgresql-online) drop yabancı anahtarı için kullanabileceğiniz bir komut dosyası.
* Tam Yük ve eşitleme için Azure veritabanı geçiş hizmeti kullanın.
* Örneği PostgreSQL için Azure veritabanı hedef veri kaynağı ile yakalanır, veritabanı Azure veritabanı geçiş hizmeti tam geçiş gerçekleştirin.
* ŞEMAYI, tablo ve sütun (PostgreSQL için Azure veritabanı için şema uygulama sorgu için bu şekilde olması gerekiyorsa) küçük, yapmak için aşağıdaki betiği çalıştırın:

  ```
  -- INPUT: schema name
  set schema.var = hr;
  
  -- Generate statements to rename tables and columns
  SELECT 1, 'SET search_path = "' ||current_setting('schema.var')||'";'
  UNION ALL
  SELECT 2, 'alter table "'||c.relname||'" rename "'||a.attname||'" to '||lower(a.attname)||';'
  FROM pg_class c
  JOIN pg_attribute a ON a.attrelid = c.oid
  JOIN pg_type t ON a.atttypid = t.oid
  LEFT JOIN pg_catalog.pg_constraint r ON c.oid = r.conrelid
     AND r.conname = a.attname
  WHERE c.relnamespace = (select oid from pg_namespace where nspname=current_setting('schema.var')) AND a.attnum > 0 AND c.relkind ='r'
  UNION ALL
  SELECT 3, 'alter table "'||c.relname||'" rename to '||lower(c.relname)||';'
  FROM pg_catalog.pg_class c
     LEFT JOIN pg_catalog.pg_namespace n ON n.oid = c.relnamespace
  WHERE c.relkind ='r' AND n.nspname=current_setting('schema.var')
  ORDER BY 1;
  ```

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Microsoft.DataMigration kaynak sağlayıcısını kaydetme

1. Azure portal'da oturum açın, **Tüm hizmetler** seçeneğini belirleyin ve ardından **Abonelikler**'i seçin.

   ![Portal aboneliklerini gösterme](media/tutorial-oracle-azure-postgresql-online/portal-select-subscriptions.png)

2. Azure Veritabanı Geçiş Hizmeti örneğini oluşturmak istediğiniz aboneliği seçin ve sonra **Kaynak sağlayıcıları**’nı seçin.

    ![Kaynak sağlayıcılarını gösterme](media/tutorial-oracle-azure-postgresql-online/portal-select-resource-provider.png)

3. "migration" araması yapın ve **Microsoft.DataMigration** öğesinin sağ tarafındaki **Kaydet**'i seçin.

    ![Kaynak sağlayıcısını kaydetme](media/tutorial-oracle-azure-postgresql-online/portal-register-resource-provider.png)

## <a name="create-a-dms-instance"></a>DMS örneği oluşturma

1. Azure portalda +**Kaynak oluştur**'u seçin, Azure Veritabanı Geçiş Hizmeti araması yapın ve açılan listeden **Azure Veritabanı Geçiş Hizmeti**'ni seçin.

    ![Azure Market](media/tutorial-oracle-azure-postgresql-online/portal-marketplace.png)

2. **Azure Veritabanı Geçiş Hizmeti** ekranında **Oluştur**'u seçin.

    ![Azure Veritabanı Geçiş Hizmeti örneğini oluşturma](media/tutorial-oracle-azure-postgresql-online/dms-create2.png)
  
3. **Geçiş Hizmeti oluşturun** ekranında hizmet için bir ad belirtin, aboneliği ve yeni ya da var olan bir kaynak grubunu seçin.

4. Mevcut bir VNet seçin veya yeni bir tane oluşturun.

    Sanal ağ, Azure veritabanı geçiş hizmeti Oracle kaynağına erişimi olan ve ' % s'hedef Azure veritabanını PostgreSQL örneği için sağlar.

    Azure portalında VNet oluşturma hakkında daha fazla bilgi için bkz [Azure portalını kullanarak bir sanal ağ oluşturma](https://aka.ms/DMSVnet).

5. Fiyatlandırma katmanını seçin.

    Maliyetler ve fiyatlandırma katmanları hakkında daha fazla bilgi için [fiyatlandırma sayfasına](https://aka.ms/dms-pricing) bakın.

    ![Azure Veritabanı Geçiş Hizmeti örneği ayarlarını yapılandırma](media/tutorial-oracle-azure-postgresql-online/dms-settings5.png)

6. Hizmeti oluşturmak için **Oluştur**’u seçin.

## <a name="create-a-migration-project"></a>Geçiş projesi oluşturma

Hizmet oluşturulduktan sonra Azure portaldan bulun, açın ve yeni bir geçiş projesi oluşturun.

1. Azure portalda **Tüm hizmetler**'i seçin, Azure Veritabanı Geçiş Hizmeti araması yapın ve **Azure Veritabanı Geçiş Hizmeti**'ni seçin.

    ![Azure Veritabanı Geçiş Hizmeti’nin tüm örneklerini bulma](media/tutorial-oracle-azure-postgresql-online/dms-search.png)

2. **Azure Veritabanı Geçiş Hizmeti** ekranında oluşturduğunuz Azure Veritabanı Geçiş Hizmeti örneğinin adını arayın ve sonuçlardan bu örneği seçin.

    ![Azure Veritabanı Geçiş Hizmeti örneğinizi bulma](media/tutorial-oracle-azure-postgresql-online/dms-instance-search.png)

3. +**Yeni Geçiş Projesi**'ni seçin.
4. Üzerinde **yeni geçiş projesi** projesi için bir ad belirtin, ekran **kaynak sunucu türü** metin kutusunda **Oracle**, **hedef sunucu türü**  metin kutusunda **PostgreSQL için Azure veritabanı**.
5. İçinde **etkinlik türünü seçin** bölümünden **çevrimiçi veri geçişi**.

   ![Veritabanı Geçiş Hizmeti Projesi Oluşturma](media/tutorial-oracle-azure-postgresql-online/dms-create-project5.png)

   > [!NOTE]
   > Alternatif olarak, seçebileceğiniz **yalnızca proje oluştur** geçiş projenizi oluşturmak ve daha sonra geçiş yürütmek için.

6. Seçin **Kaydet**, başarıyla bir çevrimiçi geçişi gerçekleştirmek ve ardından seçmek için Azure veritabanı geçiş hizmeti kullanmak için gereksinimler Not **oluşturma ve çalıştırma etkinliğinin**.

## <a name="specify-source-details"></a>Kaynak ayrıntılarını belirtme

* Üzerinde **Kaynak Ayrıntıları Ekle** ekranında, kaynak Oracle örneğini ilişkin bağlantı bilgilerini belirtin.

  ![Kaynak Ayrıntıları Ekleyin ekranı](media/tutorial-oracle-azure-postgresql-online/dms-add-source-details.png)

## <a name="upload-oracle-oci-driver"></a>Oracle OCI sürücü karşıya yükleme

1. Seçin **Kaydet**ve ardından **yükleme OCI sürücü** ekranında, Oracle hesabınızda oturum açın ve sürücüsünü indir **basiclite instantclient için windows.x64 12.2.0.1.0.zip**(37,128,586 bayt (SHA1 sağlama toplamı: 865082268) öğesinden [burada](https://www.oracle.com/technetwork/topics/winx64soft-089540.html#ic_winx64_inst).
2. Sürücü, paylaşılan bir klasöre indirin.

   Minimum salt okunur erişim ile belirtilen kullanıcı adı ile paylaşılan bir klasöre emin olun. Azure veritabanı geçiş hizmeti erişir ve belirttiğiniz kullanıcı adı kimliğine bürünerek OCI sürücü yüklemek için paylaşımından okur.

   Belirttiğiniz kullanıcı adı, bir Windows kullanıcı hesabı olmalıdır.

   ![OCI sürücü yüklemesi](media/tutorial-oracle-azure-postgresql-online/dms-oci-driver-install.png)

## <a name="specify-target-details"></a>Hedef ayrıntılarını belirtme

1. Seçin **Kaydet**ve ardından **hedef ayrıntıları** ekranında, ' % s'hedef Azure veritabanını PostgreSQL sunucusu için bağlantı ayrıntılarını belirtmek için Azure veritabanı örneğini önceden sağlanmış olan PostgreSQL hangi **ik** şema dağıtıldı.

    ![Hedef ayrıntıları ekranı](media/tutorial-oracle-azure-postgresql-online/dms-add-target-details1.png)

2. **Kaydet**'i seçin ve **Hedef veritabanlarıyla eşleyin** ekranında geçiş yapılacak kaynak ve hedef veritabanlarını eşleyin.

    Hedef veritabanını kaynak veritabanıyla aynı veritabanı adı içeriyorsa, Azure veritabanı geçiş hizmeti hedef veritabanı varsayılan olarak seçer.

    ![Hedef veritabanlarıyla eşleyin](media/tutorial-oracle-azure-postgresql-online/dms-map-target-details.png)

3. **Kaydet**'i seçin, **Geçiş özeti** ekranındaki **Etkinlik adı** metin kutusunda geçiş etkinliği için bir ad belirtin ve ardından, kaynak ve hedef ayrıntılarının önceden belirttiğiniz ayrıntılarla eşleştiğinden emin olmak üzere özeti gözden geçirin.

    ![Geçiş Özeti](media/tutorial-oracle-azure-postgresql-online/dms-migration-summary.png)

## <a name="run-the-migration"></a>Geçişi çalıştırma

* **Geçişi çalıştır**'ı seçin.

  Geçiş etkinliği penceresi açılır ve etkinliğin **Durum** bilgisi **Başlatılıyor** olarak değişir.

## <a name="monitor-the-migration"></a>Geçişi izleme

1. Geçiş etkinliği ekranında **Yenile**'yi seçerek, gösterilen verileri, geçişin **Durum** bilgisi **Çalıştırılıyor** olana kadar güncelleştirebilirsiniz.

     ![Etkinlik durumu - çalışan](media/tutorial-oracle-azure-postgresql-online/dms-activity-running.png)

2. Altında **veritabanı adı**, geçiş durumu almak için belirli bir veritabanı seçin **tam veri yüklemesi** ve **artımlı veri eşitleme** operations.

    Tam veri yüklemesi ilk yük geçiş durumunu, Artımlı veri eşitleme ise değişiklik verilerini yakalama (CDC) durumunu gösterir.

     ![Etkinlik Durumu - Tam yük tamamlandı](media/tutorial-oracle-azure-postgresql-online/dms-activity-full-load-completed.png)

     ![Etkinlik Durumu - Artımlı veri eşitleme](media/tutorial-oracle-azure-postgresql-online/dms-activity-incremental-data-sync.png)

## <a name="perform-migration-cutover"></a>Tam geçiş gerçekleştirme

İlk Tam yük tamamlandıktan sonra, veritabanları **Geçiş için hazır** olarak işaretlenir.

1. Veritabanı geçişini tamamlamaya hazır olduğunuzda **Tam Geçişi Başlat** seçeneğini belirleyin.

2. **Bekleyen değişiklikler** sayacı **0** değerini gösterene kadar bekleyerek kaynak veritabanına gelen tüm işlemleri durdurduğunuzdan emin olun.

   ![Tam geçişi başlat](media/tutorial-oracle-azure-postgresql-online/dms-start-cutover.png)

3. **Onayla**'yı ve ardından, **Uygula**'yı seçin.
4. Veritabanı geçiş durumu görüntülendiğinde **tamamlandı**, PostgreSQL örneği için yeni hedef Azure veritabanını uygulamalarınıza bağlanın.

 > [!NOTE]
 > Varsayılan olarak PostgreSQL schema.table.column küçük olduğundan, büyük harf ' küçük harflere betik kullanarak geri dönebilirsiniz **PostgreSQL için Azure veritabanı'nda şema ayarlama** bu makalenin önceki kısımlarında bölümü.

## <a name="next-steps"></a>Sonraki adımlar

* PostgreSQL için Azure Veritabanı'na yönelik çevrimiçi geçiş gerçekleştirirken karşılaşılan bilinen sorunlar ve sınırlamalar hakkında bilgi için [PostgreSQL için Azure Veritabanı geçiş işlemleri ile ilgili bilinen sorunlar ve geçici çözümler](known-issues-azure-postgresql-online.md) başlıklı makaleye bakın.
* Azure Veritabanı Geçiş Hizmeti hakkında bilgi için [What is the Azure Database Migration Service? (Azure Veritabanı Geçiş Hizmeti nedir?)](https://docs.microsoft.com/azure/dms/dms-overview) başlıklı makaleye bakın.
* PostgreSQL için Azure veritabanı hakkında bilgi için bkz [PostgreSQL için Azure veritabanı nedir?](https://docs.microsoft.com/azure/postgresql/overview).
