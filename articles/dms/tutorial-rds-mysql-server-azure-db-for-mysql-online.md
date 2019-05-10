---
title: 'Öğretici: MySQL için Azure veritabanı geçiş hizmeti için çevrimiçi bir RDS MySQL için Azure veritabanı geçişi kullanma | Microsoft Docs'
description: Bir çevrimiçi geçiş RDS Mysql'i Azure veritabanı'na MySQL için Azure veritabanı geçiş hizmeti kullanarak gerçekleştirmek öğrenin.
services: dms
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: article
ms.date: 05/08/2019
ms.openlocfilehash: e971fd160a43be088f6d3c4a9fb6fddc7dd769b0
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65415683"
---
# <a name="tutorial-migrate-rds-mysql-to-azure-database-for-mysql-online-using-dms"></a>Öğretici: RDS Mysql'i MySQL için Azure veritabanı'na geçirme çevrimiçi DMS kullanarak

RDS Mysql'i örneğine veritabanlarını geçirmek için Azure veritabanı geçiş hizmeti kullanabilirsiniz [MySQL için Azure veritabanı](https://docs.microsoft.com/azure/mysql/) kaynak veritabanı geçiş sırasında çevrimiçi kalırken. Diğer bir deyişle, uygulamanın en düşük kapalı kalma süresiyle geçiş gerçekleştirilebilir. Bu öğreticide, geçiş **çalışanlar** Azure veritabanı geçiş hizmetinin çevrimiçi geçiş etkinliğini kullanarak örnek veritabanına RDS Mysql'i örneğinden MySQL için Azure veritabanı.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Örnek şeması mysqldump ve mysql yardımcı programını kullanarak geçirin.
> * Azure veritabanı geçiş Hizmeti'nin bir örneğini oluşturun.
> * Azure veritabanı geçiş hizmeti kullanarak bir geçiş projesi oluşturun.
> * Geçişi çalıştırma.
> * Geçişi izleme.

> [!NOTE]
> Azure veritabanı geçiş hizmeti çevrimiçi bir geçiş gerçekleştirmek için Premium fiyatlandırma katmanını temel alan bir örneği oluşturmanız gerekir. Azure veritabanı geçiş hizmeti daha fazla bilgi için bkz. [fiyatlandırma](https://azure.microsoft.com/pricing/details/database-migration/) sayfası.

> [!IMPORTANT]
> En iyi geçiş deneyimi için Microsoft, Azure Veritabanı Geçiş Hizmeti’nin bir örneğini hedef veritabanıyla aynı Azure bölgesinde oluşturmayı önerir. Verileri bölgeler veya coğrafyalar arasında taşımak, geçiş sürecini yavaşlatabilir ve hatalara neden olabilir.

[!INCLUDE [online-offline](../../includes/database-migration-service-offline-online.md)]

Bu makalede, bir çevrimiçi geçiş RDS Mysql'i bir örnekten MySQL için Azure veritabanı'na gerçekleştirmek açıklar.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakileri yapmanız gerekir:

* Kaynak MySQL server desteklenen bir MySQL community sürümü çalıştırdığından emin olun. Mysql yardımcı programı veya MySQL Workbench kullanarak MySQL örneğinin sürümü belirlemek için komutu çalıştırın:

    ```
    SELECT @@version;
    ```

    Daha fazla bilgi için bkz [MySQL sürümleri için desteklenen Azure veritabanı'na](https://docs.microsoft.com/azure/mysql/concepts-supported-versions).

* İndirme ve yükleme [MySQL **çalışanlar** örnek veritabanını](https://dev.mysql.com/doc/employee/en/employees-installation.html).
* Bir örneğini oluşturmak [MySQL için Azure veritabanı](https://docs.microsoft.com/azure/mysql/quickstart-create-mysql-server-database-using-azure-portal).
* Kullanarak şirket içi kaynak sunucularınıza siteden siteye bağlantı sağlar Azure Resource Manager dağıtım modelini kullanarak bir Azure sanal ağı (VNet) için Azure veritabanı geçiş hizmeti oluşturma [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways). Sanal ağ oluşturma hakkında daha fazla bilgi için bkz. [sanal ağ belgeleri](https://docs.microsoft.com/azure/virtual-network/)ve özellikle hızlı başlangıç makalelerini ile adım adım ayrıntıları.
* VNet ağ güvenlik grubu kurallarınızı aşağıdaki gelen iletişim bağlantı noktaları için Azure veritabanı geçiş hizmeti engelleme emin olun: 443, 53, 9354, 12000 yanı sıra 445. Azure VNet NSG trafik filtreleme hakkında daha fazla ayrıntı için bkz [ağ güvenlik grupları ile ağ trafiğini filtreleme](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg).
* [Windows Güvenlik Duvarınızı veritabanı altyapısı erişimi](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access) için yapılandırın.
* Varsayılan olarak TCP 3306 numaralı bağlantı noktası olan kaynak MySQL sunucusuna erişmek Azure veritabanı geçiş hizmeti, Windows Güvenlik Duvarı'nı açın.
* Kaynak veritabanlarınızın önünde bir güvenlik duvarı cihazı kullanıyorsanız, Azure Veritabanı Geçiş Hizmeti'nin geçiş amacıyla kaynak veritabanlarına erişmesi için güvenlik duvarı kuralları eklemeniz gerekebilir.
* Sunucu düzeyinde oluşturma [güvenlik duvarı kuralı](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) için Azure veritabanı geçiş hizmeti hedef veritabanlarına erişim izni vermek Azure veritabanını MySQL sunucusu için. Azure veritabanı geçiş hizmeti için kullanılan sanal ağ alt ağ aralığını belirtin.

> [!NOTE]
> MySQL için Azure veritabanı, yalnızca Innodb tabloları destekler. MyISAM tablolar için Innodb dönüştürmek için lütfen bkz [MyISAM Innodb için dönüştürme tablolardan](https://dev.mysql.com/doc/refman/5.7/en/converting-tables-to-innodb.html) .

### <a name="set-up-aws-rds-mysql-for-replication"></a>Çoğaltma için AWS RDS Mysql'i ayarlama ayarlayın

1. Yeni bir parametre grubu oluşturmak için AWS tarafından makalede verilen yönergeleri izleyin. [MySQL veritabanı günlük dosyalarını](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_LogAccess.Concepts.MySQL.html), **ikili günlük biçimini** bölümü.
2. Aşağıdaki yapılandırmaya sahip yeni bir parametre grubu oluşturun:
    * binlog_format = satır
    * binlog_checksum = NONE
3. Yeni parametre grubunu kaydedin.

## <a name="migrate-the-schema"></a>Geçiş şeması

1. Kaynak veritabanında şema ayıklayın ve tablo şemalarını, dizinleri ve saklı yordamlar gibi tüm veritabanı nesnelerinin Geçişi tamamlamak için hedef veritabanı için geçerlidir.

    Yalnızca şemayı geçirmek için en kolay yolu mysqldump kullanmaktır--no-veri parametresine sahip. Şema geçirmek için komut şöyledir:

    ```
    mysqldump -h [servername] -u [username] -p[password] --databases [db name] --no-data > [schema file path]
    ```
    
    Örneğin, bir şema dosyası için döküm için **çalışanlar** veritabanı, aşağıdaki komutu kullanın:
    
    ```
    mysqldump -h 10.10.123.123 -u root -p --databases employees --no-data > d:\employees.sql
    ```

2. MySQL için Azure veritabanı olan hedef hizmete Şemayı içeri aktarın. Şema döküm dosyasını geri yüklemek için aşağıdaki komutu çalıştırın:

    ```
    mysql.exe -h [servername] -u [username] -p[password] [database]< [schema file path]
    ```

    Örneğin, için Şemayı içeri aktarmak için **çalışanlar** veritabanı:

    ```
    mysql.exe -h shausample.mysql.database.azure.com -u dms@shausample -p employees < d:\employees.sql
    ```

3. Şemanızda yabancı anahtarlar varsa geçişe ilişkin ilk yük ve sürekli eşitleme başarısız olur. Bırakma yabancı anahtarı betiği ayıklayın ve yabancı anahtarı betiği (MySQL için Azure veritabanı) hedef eklemek için MySQL Workbench uygulamasında aşağıdaki betiği çalıştırın:

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
      AND KCU.REFERENCED_TABLE_SCHEMA = ['SchemaName') Queries
      GROUP BY SchemaName;
    ```

4. (Olan ikinci sütundaki) bırakma yabancı anahtar, yabancı anahtarı silmek için sorgu sonucu çalıştırın.

5. Tetikleyiciler (INSERT nebo update tetikleyicisi) verileri varsa, veri kaynağından çoğaltılmadan önce hedef veri bütünlüğü zorlar. Tetikleyiciler tüm tablolarda devre dışı bırakmak için önerilir *hedefindeki* geçiş ve sonra etkinleştirme sırasında geçiş tamamlandıktan sonra Tetikleyiciler tamamlayın.

    Hedef veritabanı Tetikleyicileri devre dışı bırakmak için:

    ```
    select concat ('alter table ', event_object_table, ' disable trigger ', trigger_name)
    from information_schema.triggers;
    ```

6. Tüm tabloları sabit listesi veri türü örneği varsa, geçici olarak 'karakter değişen' veri türü'hedef tablosundaki güncelleştirmenizi öneririz. Veri kopyalama tamamlandığında, daha sonra veri türü sabit listesine geri dönün.

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Microsoft.DataMigration kaynak sağlayıcısını kaydetme

1. Azure portal'da oturum açın, **Tüm hizmetler** seçeneğini belirleyin ve ardından **Abonelikler**'i seçin.

   ![Portal aboneliklerini gösterme](media/tutorial-rds-mysql-server-azure-db-for-mysql-online/portal-select-subscription1.png)

2. Azure veritabanı geçiş hizmeti örneği oluşturun ve ardından istediğiniz aboneliği seçin **kaynak sağlayıcıları**.

    ![Kaynak sağlayıcılarını gösterme](media/tutorial-rds-mysql-server-azure-db-for-mysql-online/portal-select-resource-provider.png)

3. "migration" araması yapın ve **Microsoft.DataMigration** öğesinin sağ tarafındaki **Kaydet**'i seçin.

    ![Kaynak sağlayıcısını kaydet](media/tutorial-rds-mysql-server-azure-db-for-mysql-online/portal-register-resource-provider.png)

## <a name="create-an-instance-of-azure-database-migration-service"></a>Azure veritabanı geçiş hizmeti örneği oluşturma

1. Azure portalda +**Kaynak oluştur**'u seçin, Azure Veritabanı Geçiş Hizmeti araması yapın ve açılan listeden **Azure Veritabanı Geçiş Hizmeti**'ni seçin.

    ![Azure Market](media/tutorial-rds-mysql-server-azure-db-for-mysql-online/portal-marketplace.png)

2. **Azure Veritabanı Geçiş Hizmeti** ekranında **Oluştur**'u seçin.

    ![Azure Veritabanı Geçiş Hizmeti örneğini oluşturma](media/tutorial-rds-mysql-server-azure-db-for-mysql-online/dms-create1.png)
  
3. **Geçiş Hizmeti oluşturun** ekranında hizmet için bir ad belirtin, aboneliği ve yeni ya da var olan bir kaynak grubunu seçin.

4. Azure veritabanı geçiş hizmeti örneğini oluşturmak istediğiniz konumu seçin.

5. Mevcut bir VNet seçin veya yeni bir tane oluşturun.

    Sanal ağ erişimi kaynak MySQL örneği ve ' % s'hedef Azure veritabanını MySQL örneği için Azure veritabanı geçiş hizmeti sağlar.

    Azure portalında VNet oluşturma hakkında daha fazla bilgi için bkz [Azure portalını kullanarak bir sanal ağ oluşturma](https://aka.ms/DMSVnet).

6. Bir fiyatlandırma katmanı seçin. Bu çevrimiçi geçiş için Premium seçtiğinizden emin olun: 4vCores fiyatlandırma katmanı.

    ![Azure Veritabanı Geçiş Hizmeti örneği ayarlarını yapılandırma](media/tutorial-rds-mysql-server-azure-db-for-mysql-online/dms-settings3.png)

7. Hizmeti oluşturmak için **Oluştur**’u seçin.

## <a name="create-a-migration-project"></a>Geçiş projesi oluşturma

Hizmet oluşturulduktan sonra Azure portaldan bulun, açın ve yeni bir geçiş projesi oluşturun.

1. Azure portalda **Tüm hizmetler**'i seçin, Azure Veritabanı Geçiş Hizmeti araması yapın ve **Azure Veritabanı Geçiş Hizmeti**'ni seçin.

      ![Azure Veritabanı Geçiş Hizmeti’nin tüm örneklerini bulma](media/tutorial-rds-mysql-server-azure-db-for-mysql-online/dms-search.png)

2. **Azure Veritabanı Geçiş Hizmeti** ekranında oluşturduğunuz Azure Veritabanı Geçiş Hizmeti örneğinin adını arayın ve sonuçlardan bu örneği seçin.

     ![Azure Veritabanı Geçiş Hizmeti örneğinizi bulma](media/tutorial-rds-mysql-server-azure-db-for-mysql-online/dms-instance-search.png)

3. +**Yeni Geçiş Projesi**'ni seçin.
4. Üzerinde **yeni geçiş projesi** projesi için bir ad belirtin, ekran **kaynak sunucu türü** metin kutusunda **MySQL**ve ardından **hedef Sunucu türü** metin kutusunda **AzureDbForMySQL**.
5. İçinde **etkinlik türünü seçin** bölümünden **çevrimiçi veri geçişi**.

    > [!IMPORTANT]
    > Seçtiğinizden emin olun **çevrimiçi veri geçişi**; çevrimdışı geçişler, bu senaryo için desteklenmez.

    ![Veritabanı Geçiş Hizmeti Projesi Oluşturma](media/tutorial-rds-mysql-server-azure-db-for-mysql-online/dms-create-project6.png)

    > [!NOTE]
    > Alternatif olarak, seçebileceğiniz **yalnızca proje oluştur** geçiş projenizi oluşturmak ve daha sonra geçiş yürütmek için.

6. **Kaydet**’i seçin.

7. Projeyi oluşturmak ve geçiş etkinliğini çalıştırmak için **Etkinlik oluştur ve çalıştır**'ı seçin.

    > [!NOTE]
    > Lütfen çevrimiçi geçiş projesi oluşturma dikey penceresinde kurmak için gerekli Önkoşullar not edin.

## <a name="specify-source-details"></a>Kaynak ayrıntılarını belirtme

* Üzerinde **geçiş kaynağı ayrıntıları** ekranında, kaynak MySQL örneğine ilişkin bağlantı bilgilerini belirtin.

   ![Kaynak Ayrıntıları](media/tutorial-rds-mysql-server-azure-db-for-mysql-online/dms-source-details5.png)

## <a name="specify-target-details"></a>Hedef ayrıntılarını belirtme

1. Seçin **Kaydet**ve ardından **hedef ayrıntıları** ekranında, önceden sağlanmış ve olan MySQL sunucusu için Azure veritabanı hedef için bağlantı ayrıntılarını belirt **Çalışanlar** şema MySQLDump kullanılarak dağıtılabilir.

    ![Hedef seçme](media/tutorial-rds-mysql-server-azure-db-for-mysql-online/dms-select-target5.png)

2. **Kaydet**'i seçin ve **Hedef veritabanlarıyla eşleyin** ekranında geçiş yapılacak kaynak ve hedef veritabanlarını eşleyin.

    Hedef veritabanını kaynak veritabanıyla aynı veritabanı adı içeriyorsa, Azure veritabanı geçiş hizmeti hedef veritabanı varsayılan olarak seçer.

    ![Hedef veritabanlarıyla eşleyin](media/tutorial-rds-mysql-server-azure-db-for-mysql-online/dms-map-targets-activity5.png)

3. **Kaydet**'i seçin, **Geçiş özeti** ekranındaki **Etkinlik adı** metin kutusunda geçiş etkinliği için bir ad belirtin ve ardından, kaynak ve hedef ayrıntılarının önceden belirttiğiniz ayrıntılarla eşleştiğinden emin olmak üzere özeti gözden geçirin.

    ![Geçiş Özeti](media/tutorial-rds-mysql-server-azure-db-for-mysql-online/dms-migration-summary2.png)

## <a name="run-the-migration"></a>Geçişi çalıştırma

* **Geçişi çalıştır**'ı seçin.

    Geçiş etkinliği penceresi açılır ve etkinliğin **Durum** bilgisi **Başlatılıyor** olarak belirlenir.

## <a name="monitor-the-migration"></a>Geçişi izleme

1. Geçiş etkinliği ekranında **Yenile**'yi seçerek, gösterilen verileri, geçişin **Durum** bilgisi **Çalıştırılıyor** olana kadar güncelleştirebilirsiniz.

    ![Etkinlik durumu - çalışan](media/tutorial-rds-mysql-server-azure-db-for-mysql-online/dms-activity-status4.png)

2. Altında **veritabanı adı**, geçiş durumu almak için belirli bir veritabanı seçin **tam veri yüklemesi** ve **artımlı veri eşitleme** operations.

    **Tam veri yüklemesi** ilk yük geçiş durumu gösterilmektedir ancak **artımlı veri eşitleme** gösterir değişiklik verilerini yakalama (CDC) durumu.

    ![Stok ekran - tam veri yükleme](media/tutorial-rds-mysql-server-azure-db-for-mysql-online/dms-inventory-full-load.png)

    ![Stok ekran - artımlı veri eşitleme](media/tutorial-rds-mysql-server-azure-db-for-mysql-online/dms-inventory-incremental.png)

## <a name="perform-migration-cutover"></a>Tam geçiş gerçekleştirme

İlk tam yükleme işlemi tamamlandıktan sonra veritabanlarının işaretlenmiş olduğundan **Cutover hazır**.

1. Veritabanı geçişini tamamlamaya hazır olduğunuzda **Tam Geçişi Başlat** seçeneğini belirleyin.

    ![Kesme baştan başlayın](media/tutorial-rds-mysql-server-azure-db-for-mysql-online/dms-inventory-start-cutover.png)

2. **Bekleyen değişiklikler** sayacı **0** değerini gösterene kadar bekleyerek kaynak veritabanına gelen tüm işlemleri durdurduğunuzdan emin olun.
3. **Onayla**'yı ve ardından, **Uygula**'yı seçin.
4. Veritabanı geçiş durumu görüntülendiğinde **tamamlandı**, yeni hedef Azure veritabanını MySQL veritabanı için uygulamalarınıza bağlanın.

MySQL için Azure veritabanı şirket içi örneği çevrimiçi geçişiniz için MySQL tamamlanmıştır.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Veritabanı Geçiş Hizmeti hakkında bilgi için [What is the Azure Database Migration Service? (Azure Veritabanı Geçiş Hizmeti nedir?)](https://docs.microsoft.com/azure/dms/dms-overview) başlıklı makaleye bakın.
* MySQL için Azure veritabanı hakkında bilgi için bkz [MySQL için Azure veritabanı nedir?](https://docs.microsoft.com/azure/mysql/overview).
* Diğer sorular için e-posta [isteyin Azure veritabanı geçişlerini](mailto:AskAzureDatabaseMigrations@service.microsoft.com) diğer adı.
