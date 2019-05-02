---
title: 'Öğretici: PostgreSQL için Azure veritabanı geçiş hizmeti için çevrimiçi bir RDS PostgreSQL için Azure veritabanı geçişi kullanma | Microsoft Docs'
description: Bir çevrimiçi geçiş RDS PostgreSQL için Azure veritabanı PostgreSQL için Azure veritabanı geçiş hizmeti kullanarak gerçekleştirmek öğrenin.
services: dms
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: article
ms.date: 04/26/2019
ms.openlocfilehash: 71610aa9916519338c564127616f4569aff70aaa
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64925343"
---
# <a name="tutorial-migrate-rds-postgresql-to-azure-database-for-postgresql-online-using-dms"></a>Öğretici: RDS PostgreSQL, PostgreSQL için Azure veritabanı'na geçirme çevrimiçi DMS kullanarak

Azure veritabanı geçiş hizmeti örneğine RDS PostgreSQL veritabanları geçirmek için kullanabileceğiniz [PostgreSQL için Azure veritabanı](https://docs.microsoft.com/azure/postgresql/) kaynak veritabanı geçiş sırasında çevrimiçi kalırken. Diğer bir deyişle, uygulamanın en düşük kapalı kalma süresiyle geçiş gerçekleştirilebilir. Bu öğreticide, geçiş **DVD kiralama** Azure veritabanı geçiş hizmetinin çevrimiçi geçiş etkinliğini kullanarak örnek veritabanına RDS PostgreSQL 9.6 örneğinden PostgreSQL için Azure veritabanı.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
>
> * Örnek şeması pg_dump yardımcı programını kullanarak geçirin.
> * Azure veritabanı geçiş Hizmeti'nin bir örneğini oluşturun.
> * Azure veritabanı geçiş hizmeti kullanarak bir geçiş projesi oluşturun.
> * Geçişi çalıştırma.
> * Geçişi izleme.

> [!NOTE]
> Azure veritabanı geçiş hizmeti çevrimiçi bir geçiş gerçekleştirmek için Premium fiyatlandırma katmanını temel alan bir örneği oluşturmanız gerekir. Azure veritabanı geçiş hizmeti daha fazla bilgi için bkz. [fiyatlandırma](https://azure.microsoft.com/pricing/details/database-migration/) sayfası.

> [!IMPORTANT]
> En iyi geçiş deneyimi için Microsoft, Azure Veritabanı Geçiş Hizmeti’nin bir örneğini hedef veritabanıyla aynı Azure bölgesinde oluşturmayı önerir. Verileri bölgeler veya coğrafyalar arasında taşımak, geçiş sürecini yavaşlatabilir ve hatalara neden olabilir.

[!INCLUDE [online-offline](../../includes/database-migration-service-offline-online.md)]

Bu makalede, bir çevrimiçi geçiş bir şirket içi PostgreSQL örneğinden PostgreSQL için Azure veritabanı'na gerçekleştirmek açıklar.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakileri yapmanız gerekir:

* İndirme ve yükleme [PostgreSQL community sürümünü](https://www.postgresql.org/download/) 9.5, 9.6 veya 10. Kaynak PostgreSQL Server sürümü olmalıdır 9.5.11 9.6.7, 10 veya üzeri. Daha fazla bilgi için bkz [desteklenen PostgreSQL veritabanı sürümlere](https://docs.microsoft.com/azure/postgresql/concepts-supported-versions).

    Ayrıca, RDS PostgreSQL sürümü, sürüm PostgreSQL için Azure veritabanı eşleşmesi gerekir. Örneğin, RDS PostgreSQL 9.5.11.5 yalnızca sürüm 9.6.7 değil de, 9.5.11 PostgreSQL için Azure veritabanı geçiş yapabilirsiniz.

    > [!NOTE]
    > PostgreSQL için sürüm 10, şu anda DMS yalnızca PostgreSQL için Azure veritabanı 10.3 sürümüne geçişini destekler. PostgreSQL daha yeni sürümlerini çok yakında desteklemeyi planlıyoruz.

* Bir örneğini oluşturmak [PostgreSQL için Azure veritabanı](https://docs.microsoft.com/azure/postgresql/quickstart-create-server-database-portal). Lütfen şuna bakın [bölümü](https://docs.microsoft.com/azure/postgresql/quickstart-create-server-database-portal#connect-to-the-postgresql-server-using-pgadmin) pgAdmin kullanarak PostgreSQL Sunucusu'na bağlanma hakkında daha fazla ayrıntı için belgenin.
* Kullanarak şirket içi kaynak sunucularınıza siteden siteye bağlantı sağlar Azure Resource Manager dağıtım modelini kullanarak bir Azure sanal ağı (VNet) için Azure veritabanı geçiş hizmeti oluşturma [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).
* VNet ağ güvenlik grubu kurallarınızı aşağıdaki gelen iletişim bağlantı noktaları için Azure veritabanı geçiş hizmeti engelleme emin olun: 443, 53, 9354, 12000 yanı sıra 445. Azure VNet NSG trafik filtreleme hakkında daha fazla ayrıntı için bkz [ağ güvenlik grupları ile ağ trafiğini filtreleme](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg).
* [Windows Güvenlik Duvarınızı veritabanı altyapısı erişimi](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access) için yapılandırın.
* Varsayılan olarak TCP bağlantı noktası olan 5432 kaynak PostgreSQL sunucusuna erişmek Azure veritabanı geçiş hizmeti, Windows Güvenlik Duvarı'nı açın.
* Kaynak veritabanlarınızın önünde bir güvenlik duvarı cihazı kullanıyorsanız, Azure Veritabanı Geçiş Hizmeti'nin geçiş amacıyla kaynak veritabanlarına erişmesi için güvenlik duvarı kuralları eklemeniz gerekebilir.
* Sunucu düzeyinde oluşturma [güvenlik duvarı kuralı](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) için Azure veritabanı geçiş hizmeti hedef veritabanlarına erişim izni vermek Azure veritabanı PostgreSQL sunucusu için. Azure veritabanı geçiş hizmeti için kullanılan sanal ağ alt ağ aralığını belirtin.

### <a name="set-up-aws-rds-postgresql-for-replication"></a>Çoğaltma için AWS RDS PostgreSQL ' ayarlayın

1. Yeni bir parametre grubu oluşturmak için AWS tarafından makalede verilen yönergeleri izleyin. [DB parametresi gruplarıyla çalışma](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_WorkingWithParamGroups.html).
2. Azure veritabanı geçiş hizmeti olan kaynağına bağlanmak için ana kullanıcı adı kullanın. Ana kullanıcı hesabı dışında bir hesap kullanırsanız, hesabın rds_superuser rol ve rds_replication rol olmalıdır. Rds_replication rol mantıksal yuvalarını yönetmek için izinler veren ve mantıksal yuvalarını kullanarak veri akışı.
3. Aşağıdaki yapılandırmaya sahip yeni bir parametre grubu oluştur: bir. 1 DB parametresi grubunuzdaki rds.logical_replication parametresini ayarlayın.
    b. max_wal_senders [eşzamanlı görev sayısı] - = max_wal_senders parametresini çalıştırın, 10 görevleri önerilir eşzamanlı görev sayısını ayarlar.
    c. max_replication_slots – = 5 yuvalarına [yuva sayısı], öneri set.
4. RDS PostgreSQL örneği oluşturulan bir parametre grubu ile ilişkilendirin.

## <a name="migrate-the-schema"></a>Geçiş şeması

1. Kaynak veritabanında şema ayıklayın ve tablo şemalarını, dizinleri ve saklı yordamlar gibi tüm veritabanı nesnelerinin Geçişi tamamlamak için hedef veritabanı için geçerlidir.

    Yalnızca şemayı geçirmek için en kolay yolu, -s seçeneği ile pg_dump kullanmaktır. Daha fazla bilgi için [örnekler](https://www.postgresql.org/docs/9.6/app-pgdump.html#PG-DUMP-EXAMPLES) Postgres pg_dump öğreticide.
    
    ```
    pg_dump -o -h hostname -U db_username -d db_name -s > your_schema.sql
    ```
    
    Örneğin, bir şema dosyası için döküm için **dvdrental** veritabanı, aşağıdaki komutu kullanın:
    
    ```
    pg_dump -o -h localhost -U postgres -d dvdrental -s  > dvdrentalSchema.sql
    ```

2. PostgreSQL için Azure veritabanı hedef hizmeti boş bir veritabanı oluşturun. Bağlanmak ve bir veritabanı oluşturmak için aşağıdaki makalelerden birine bakın:

    * [Azure portalını kullanarak PostgreSQL sunucusu için Azure veritabanı oluşturma](https://docs.microsoft.com/azure/postgresql/quickstart-create-server-database-portal)
    * [Azure CLI aracını kullanarak PostgreSQL için Azure Veritabanı oluşturma](https://docs.microsoft.com/azure/postgresql/quickstart-create-server-database-azure-cli)

3. PostgreSQL için Azure veritabanı olan hedef hizmete Şemayı içeri aktarın. Şema döküm dosyasını geri yüklemek için aşağıdaki komutu çalıştırın:

    ```
    psql -h hostname -U db_username -d db_name < your_schema.sql
    ```

    Örneğin:

    ```
    psql -h mypgserver-20170401.postgres.database.azure.com  -U postgres -d dvdrental < dvdrentalSchema.sql
    ```

4. Şemanızda yabancı anahtarlar varsa geçişe ilişkin ilk yük ve sürekli eşitleme başarısız olur. Bırakma yabancı anahtarı betiği ayıklayın ve yabancı anahtarı betiği (PostgreSQL için Azure veritabanı) hedef eklemek için PgAdmin veya psql aşağıdaki betiği çalıştırın:

    ```
    SET group_concat_max_len = 8192;
        SELECT SchemaName, GROUP_CONCAT(DropQuery SEPARATOR ';\n') as DropQuery, GROUP_CONCAT(AddQuery SEPARATOR ';\n') as AddQuery
        FROM
        (SELECT 
        KCU.REFERENCED_TABLE_SCHEMA as SchemaName,    
        KCU.TABLE_NAME,
        KCU.COLUMN_NAME,
        CONCAT('ALTER TABLE ', KCU.TABLE_NAME, ' DROP FOREIGN KEY ', KCU.CONSTRAINT_NAME) AS DropQuery,
        CONCAT('ALTER TABLE ', KCU.TABLE_NAME, ' ADD CONSTRAINT ', KCU.CONSTRAINT_NAME, ' FOREIGN KEY (`', KCU.COLUMN_NAME, '`) REFERENCES `', KCU.REFERENCED_TABLE_NAME, '` (`', KCU.REFERENCED_COLUMN_NAME, '`) ON UPDATE ',RC.UPDATE_RULE, ' ON DELETE ',RC.DELETE_RULE) AS AddQuery
        FROM INFORMATION_SCHEMA.KEY_COLUMN_USAGE KCU, information_schema.REFERENTIAL_CONSTRAINTS RC
        WHERE
          KCU.CONSTRAINT_NAME = RC.CONSTRAINT_NAME
          AND KCU.REFERENCED_TABLE_SCHEMA = RC.UNIQUE_CONSTRAINT_SCHEMA
      AND KCU.REFERENCED_TABLE_SCHEMA = 'sakila') Queries
      GROUP BY SchemaName;
    ```
        
5. (Olan ikinci sütundaki) bırakma yabancı anahtar, yabancı anahtarı silmek için sorgu sonucu çalıştırın.

6. Tetikleyiciler (INSERT nebo update tetikleyicisi) verileri varsa, veri kaynağından çoğaltılmadan önce hedef veri bütünlüğü zorlar. Tetikleyiciler tüm tablolarda devre dışı bırakmak için önerilir *hedefindeki* geçiş ve sonra etkinleştirme sırasında geçiş tamamlandıktan sonra Tetikleyiciler tamamlayın.

    Hedef veritabanı Tetikleyicileri devre dışı bırakmak için:

    ```
    SELECT Concat('DROP TRIGGER ', Trigger_Name, ';') FROM  information_schema.TRIGGERS WHERE TRIGGER_SCHEMA = 'your_schema';
    ```

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Microsoft.DataMigration kaynak sağlayıcısını kaydetme

1. Azure portal'da oturum açın, **Tüm hizmetler** seçeneğini belirleyin ve ardından **Abonelikler**'i seçin.

   ![Portal aboneliklerini gösterme](media/tutorial-rds-postgresql-server-azure-db-for-postgresql-online/portal-select-subscription1.png)

2. Azure veritabanı geçiş hizmeti örneği oluşturun ve ardından istediğiniz aboneliği seçin **kaynak sağlayıcıları**.

    ![Kaynak sağlayıcılarını gösterme](media/tutorial-rds-postgresql-server-azure-db-for-postgresql-online/portal-select-resource-provider.png)

3. "migration" araması yapın ve **Microsoft.DataMigration** öğesinin sağ tarafındaki **Kaydet**'i seçin.

    ![Kaynak sağlayıcısını kaydetme](media/tutorial-rds-postgresql-server-azure-db-for-postgresql-online/portal-register-resource-provider.png)

## <a name="create-an-instance-of-azure-database-migration-service"></a>Azure veritabanı geçiş hizmeti örneği oluşturma

1. Azure portalda +**Kaynak oluştur**'u seçin, Azure Veritabanı Geçiş Hizmeti araması yapın ve açılan listeden **Azure Veritabanı Geçiş Hizmeti**'ni seçin.

    ![Azure Market](media/tutorial-rds-postgresql-server-azure-db-for-postgresql-online/portal-marketplace.png)

2. **Azure Veritabanı Geçiş Hizmeti** ekranında **Oluştur**'u seçin.

    ![Azure Veritabanı Geçiş Hizmeti örneğini oluşturma](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dms-create1.png)
  
3. **Geçiş Hizmeti oluşturun** ekranında hizmet için bir ad belirtin, aboneliği ve yeni ya da var olan bir kaynak grubunu seçin.

4. Azure veritabanı geçiş hizmeti örneğini oluşturmak istediğiniz konumu seçin.

5. Mevcut bir VNet seçin veya yeni bir tane oluşturun.

    VNet kaynak PostgreSQL örneği ve ' % s'hedef Azure veritabanını erişim örneği PostgreSQL için Azure veritabanı geçiş hizmeti sağlar.

    Azure portalında VNet oluşturma hakkında daha fazla bilgi için bkz [Azure portalını kullanarak bir sanal ağ oluşturma](https://aka.ms/DMSVnet).

6. Bir fiyatlandırma katmanı seçin. Bu çevrimiçi geçiş için Premium seçtiğinizden emin olun: 4vCores fiyatlandırma katmanı.

    Maliyetler ve fiyatlandırma katmanları hakkında daha fazla bilgi için [fiyatlandırma sayfasına](https://aka.ms/dms-pricing) bakın. Doğru Azure veritabanı geçiş Service fiyatlandırma katmanı seçme yardıma gereksinim duyarsanız, posta önerilere bakın [burada](https://go.microsoft.com/fwlink/?linkid=861067).

     ![Azure Veritabanı Geçiş Hizmeti örneği ayarlarını yapılandırma](media/tutorial-rds-postgresql-server-azure-db-for-postgresql-online/dms-settings4.png)

7. Hizmeti oluşturmak için **Oluştur**’u seçin.

## <a name="create-a-migration-project"></a>Geçiş projesi oluşturma

Hizmet oluşturulduktan sonra Azure portaldan bulun, açın ve yeni bir geçiş projesi oluşturun.

1. Azure portalda **Tüm hizmetler**'i seçin, Azure Veritabanı Geçiş Hizmeti araması yapın ve **Azure Veritabanı Geçiş Hizmeti**'ni seçin.

      ![Azure Veritabanı Geçiş Hizmeti’nin tüm örneklerini bulma](media/tutorial-rds-postgresql-server-azure-db-for-postgresql-online/dms-search.png)

2. **Azure Veritabanı Geçiş Hizmeti** ekranında oluşturduğunuz Azure Veritabanı Geçiş Hizmeti örneğinin adını arayın ve sonuçlardan bu örneği seçin.

     ![Azure Veritabanı Geçiş Hizmeti örneğinizi bulma](media/tutorial-rds-postgresql-server-azure-db-for-postgresql-online/dms-instance-search.png)

3. +**Yeni Geçiş Projesi**'ni seçin.
4. Üzerinde **yeni geçiş projesi** ekranında, proje için bir ad belirtin **kaynak sunucu türü** metin kutusunda **PostgreSQL için AWS RDS**ve ardından **Hedef sunucu türü** metin kutusunda **PostgreSQL için Azure veritabanı**.
5. İçinde **etkinlik türünü seçin** bölümünden **çevrimiçi veri geçişi**.

    > [!IMPORTANT]
    > Seçtiğinizden emin olun **çevrimiçi veri geçişi**; çevrimdışı geçişler, bu senaryo için desteklenmez.

    ![Veritabanı Geçiş Hizmeti Projesi Oluşturma](media/tutorial-rds-postgresql-server-azure-db-for-postgresql-online/dms-create-project5.png)

    > [!NOTE]
    > Alternatif olarak, seçebileceğiniz **yalnızca proje oluştur** geçiş projenizi oluşturmak ve daha sonra geçiş yürütmek için.

6. **Kaydet**’i seçin.

7. Projeyi oluşturmak ve geçiş etkinliğini çalıştırmak için **Etkinlik oluştur ve çalıştır**'ı seçin.

    > [!NOTE]
    > Lütfen çevrimiçi geçiş projesi oluşturma dikey penceresinde kurmak için gerekli Önkoşullar not edin.

## <a name="specify-source-details"></a>Kaynak ayrıntılarını belirtme

* Üzerinde **geçiş kaynağı ayrıntıları** ekranında, kaynak PostgreSQL örneğine ilişkin bağlantı bilgilerini belirtin.

   ![Kaynak Ayrıntıları](media/tutorial-rds-postgresql-server-azure-db-for-postgresql-online/dms-source-details4.png)

## <a name="specify-target-details"></a>Hedef ayrıntılarını belirtme

1. Seçin **Kaydet**ve ardından **hedef ayrıntıları** ekranında, önceden sağlanmış ve olan PostgreSQL sunucusu için Azure veritabanı hedef için bağlantı ayrıntılarını belirt **DVD Bisiklet** şema pg_dump kullanılarak dağıtılabilir.

    ![Hedef seçme](media/tutorial-rds-postgresql-server-azure-db-for-postgresql-online/dms-select-target4.png)

2. **Kaydet**'i seçin ve **Hedef veritabanlarıyla eşleyin** ekranında geçiş yapılacak kaynak ve hedef veritabanlarını eşleyin.

    Hedef veritabanını kaynak veritabanıyla aynı veritabanı adı içeriyorsa, Azure veritabanı geçiş hizmeti hedef veritabanı varsayılan olarak seçer.

    ![Hedef veritabanlarıyla eşleyin](media/tutorial-rds-postgresql-server-azure-db-for-postgresql-online/dms-map-targets-activity5.png)

3. **Kaydet**'i seçin, **Geçiş özeti** ekranındaki **Etkinlik adı** metin kutusunda geçiş etkinliği için bir ad belirtin ve ardından, kaynak ve hedef ayrıntılarının önceden belirttiğiniz ayrıntılarla eşleştiğinden emin olmak üzere özeti gözden geçirin.

    ![Geçiş Özeti](media/tutorial-rds-postgresql-server-azure-db-for-postgresql-online/dms-migration-summary1.png)

## <a name="run-the-migration"></a>Geçişi çalıştırma

* **Geçişi çalıştır**'ı seçin.

    Geçiş etkinliği penceresi açılır ve etkinliğin **Durum** bilgisi **Başlatılıyor** olarak belirlenir.

## <a name="monitor-the-migration"></a>Geçişi izleme

1. Geçiş etkinliği ekranında **Yenile**'yi seçerek, gösterilen verileri, geçişin **Durum** bilgisi **Çalıştırılıyor** olana kadar güncelleştirebilirsiniz.

    ![Etkinlik durumu - çalışan](media/tutorial-rds-postgresql-server-azure-db-for-postgresql-online/dms-activity-status3.png)

2. Altında **veritabanı adı**, geçiş durumu almak için belirli bir veritabanı seçin **tam veri yüklemesi** ve **artımlı veri eşitleme** operations.

    **Tam veri yüklemesi** ilk yük geçiş durumu gösterilmektedir ancak **artımlı veri eşitleme** gösterir değişiklik verilerini yakalama (CDC) durumu.

    ![Stok ekran - tam veri yükleme](media/tutorial-rds-postgresql-server-azure-db-for-postgresql-online/dms-inventory-full-load.png)

    ![Stok ekran - artımlı veri eşitleme](media/tutorial-rds-postgresql-server-azure-db-for-postgresql-online/dms-inventory-incremental.png)

## <a name="perform-migration-cutover"></a>Tam geçiş gerçekleştirme

İlk tam yükleme işlemi tamamlandıktan sonra veritabanlarının işaretlenmiş olduğundan **Cutover hazır**.

1. Veritabanı geçişini tamamlamaya hazır olduğunuzda **Tam Geçişi Başlat** seçeneğini belirleyin.

    ![Kesme baştan başlayın](media/tutorial-rds-postgresql-server-azure-db-for-postgresql-online/dms-inventory-start-cutover.png)
 
2. **Bekleyen değişiklikler** sayacı **0** değerini gösterene kadar bekleyerek kaynak veritabanına gelen tüm işlemleri durdurduğunuzdan emin olun.
3. **Onayla**'yı ve ardından, **Uygula**'yı seçin.
4. Veritabanı geçiş durumu görüntülendiğinde **tamamlandı**, PostgreSQL veritabanı için yeni hedef Azure veritabanını uygulamalarınıza bağlanın.

PostgreSQL için bir şirket içi PostgreSQL için Azure veritabanı örneğinin çevrimiçi geçiş tamamlanmıştır.

## <a name="next-steps"></a>Sonraki adımlar

- Azure Veritabanı Geçiş Hizmeti hakkında bilgi için [What is the Azure Database Migration Service? (Azure Veritabanı Geçiş Hizmeti nedir?)](https://docs.microsoft.com/azure/dms/dms-overview) başlıklı makaleye bakın.
- PostgreSQL için Azure veritabanı hakkında bilgi için bkz [PostgreSQL için Azure veritabanı nedir?](https://docs.microsoft.com/azure/postgresql/overview).
- Diğer sorular için e-posta [isteyin Azure veritabanı geçişlerini](mailto:AskAzureDatabaseMigrations@service.microsoft.com) diğer adı.
