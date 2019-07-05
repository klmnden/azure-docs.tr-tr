---
title: Veri çoğaltma MariaDB için Azure veritabanı'nda yapılandırma | Microsoft Docs
description: Bu makalede, veri çoğaltma Azure veritabanı'nda MariaDB için ayarlanacağını açıklar.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 21e8a88cc6f03b4d54a6c5299b0b6be36cc32d6d
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67444799"
---
# <a name="configure-data-in-replication-in-azure-database-for-mariadb"></a>Veri çoğaltma MariaDB için Azure veritabanı'nda yapılandırma

Bu makalede, veri çoğaltma Azure veritabanı'nda MariaDB için ana ve çoğaltma sunucuları yapılandırma tarafından nasıl ayarlanacağı açıklanır. Bu makalede, MariaDB sunucular ve veritabanları ile konusunda biraz deneyim sahibi olduğunuzu varsayar.

MariaDB hizmeti için Azure veritabanı'nda bir çoğaltma oluşturmak için bir ana MariaDB sunucusu şirket içi, sanal makineler (VM) veya Bulut veritabanı Hizmetleri verilerini veri çoğaltma eşitler.

> [!NOTE]
> Ana sunucunuz 10.2 veya sonraki sürümlerde ise, veri çoğaltma kullanarak ayarlamanızı öneririz [genel işlem kimliği](https://mariadb.com/kb/en/library/gtid/).


## <a name="create-a-mariadb-server-to-use-as-a-replica"></a>Bir çoğaltma olarak kullanmak üzere bir MariaDB sunucusu oluşturma

1. MariaDB sunucusu (örneğin, replica.mariadb.database.azure.com) için yeni bir Azure veritabanı oluşturun. Veri çoğaltma çoğaltma sunucusunda sunucusudur.

    Sunucu oluşturma hakkında bilgi edinmek için [MariaDB server için Azure veritabanı Azure portalını kullanarak oluşturma](quickstart-create-mariadb-server-database-using-azure-portal.md).

   > [!IMPORTANT]
   > MariaDB için Azure veritabanı genel amaçlı veya bellek için iyileştirilmiş fiyatlandırma katmanında oluşturmanız gerekir.

2. Aynı kullanıcı hesaplarını ve karşılık gelen ayrıcalıkları oluşturun.
    
    Kullanıcı hesapları ana sunucu ile çoğaltma sunucusuna çoğaltılmadığından. Çoğaltma sunucusu için kullanıcı erişim sağlamak için el ile tüm hesapları ve karşılık gelen ayrıcalıkları MariaDB sunucu için yeni oluşturulan Azure veritabanı oluşturmanız gerekir.

## <a name="configure-the-master-server"></a>Ana sunucuyu yapılandırma

Aşağıdaki adımlar, hazırlama ve MariaDB barındırılan şirket içi, bir VM'de veya bir bulut veritabanı hizmeti veri çoğaltma için yapılandırma. MariaDB, ana veritabanında veri çoğaltma sunucusudur.

1. İkili günlük özelliğini açar.
    
    İkili günlük ana etkin olup olmadığını görmek için aşağıdaki komutu girin:

   ```sql
   SHOW VARIABLES LIKE 'log_bin';
   ```

   Değişken [ `log_bin` ](https://mariadb.com/kb/en/library/replication-and-binary-log-server-system-variables/#log_bin) değeri döndürür `ON`, ikili günlük sunucuda etkin.

   Varsa `log_bin` değeri döndürür `OFF`, Düzen **my.cnf** dosya böylece `log_bin=ON` ikili günlük özelliğini açar. Etkili değişiklik yapmak için sunucuyu yeniden başlatın.

2. Ana sunucu ayarlarını yapılandırın.

    Veri çoğaltma parametresi gerektirir `lower_case_table_names` ana ve çoğaltma sunucuları arasında tutarlı olması. `lower_case_table_names` Parametrenin ayarlanmış `1` MariaDB için Azure veritabanı'nda varsayılan olarak.

   ```sql
   SET GLOBAL lower_case_table_names = 1;
   ```

3. Yeni bir çoğaltma rolü oluşturmak ve izinlerini ayarlayabilirsiniz.

   Çoğaltma ayrıcalıkları ile yapılandırılan ana sunucu üzerinde bir kullanıcı hesabı oluşturun. SQL komutlarını veya MySQL Workbench kullanarak bir hesap oluşturabilirsiniz. SSL ile çoğaltmak planlıyorsanız, kullanıcı hesabı oluşturduğunuzda bu belirtmeniz gerekir.
   
   Ana sunucunuz üzerinde kullanıcı hesaplarını ekleme konusunda bilgi edinmek için [MariaDB belgeleri](https://mariadb.com/kb/en/library/create-user/).

   Yeni çoğaltma rolü, aşağıdaki komutları kullanarak herhangi bir makineden, yalnızca yöneticisini barındıran bilgisayarın asıl erişebilirsiniz. Bu erişim için belirtin **syncuser\@'%'** komut bir kullanıcı oluşturun.
   
   MariaDB belgeleri hakkında daha fazla bilgi için bkz. [hesap adlarını belirten](https://mariadb.com/kb/en/library/create-user/#account-names).

   **SQL komutu**

   - SSL ile çoğaltma

       Tüm kullanıcı bağlantılar için SSL gerektirmek için bir kullanıcı oluşturmak için aşağıdaki komutu girin:

       ```sql
       CREATE USER 'syncuser'@'%' IDENTIFIED BY 'yourpassword';
       GRANT REPLICATION SLAVE ON *.* TO ' syncuser'@'%' REQUIRE SSL;
       ```

   - SSL olmayan çoğaltma

       SSL tüm bağlantılar için gerekli değilse, bir kullanıcı oluşturmak için aşağıdaki komutu girin:
    
       ```sql
       CREATE USER 'syncuser'@'%' IDENTIFIED BY 'yourpassword';
       GRANT REPLICATION SLAVE ON *.* TO ' syncuser'@'%';
       ```

   **MySQL Workbench**

   MySQL Workbench uygulamasında çoğaltma rolü oluşturmak için **Yönetim** bölmesinde **kullanıcılar ve ayrıcalıkları**. Ardından **hesabı Ekle**.
 
   ![Kullanıcılar ve ayrıcalıkları](./media/howto-data-in-replication/users_privileges.png)

   Bir kullanıcı adını girin **oturum açma adı** alan.

   ![Kullanıcıyı Eşitle](./media/howto-data-in-replication/syncuser.png)
 
   Seçin **yönetim rolleri** paneli ve ardından listesini **genel ayrıcalıkları**seçin **çoğaltma bağımlı**. Seçin **Uygula** çoğaltma rolü oluşturmak için.

   ![Çoğaltma bağımlı](./media/howto-data-in-replication/replicationslave.png)


4. Ana sunucu salt okunur moda ayarlayın.

   Bir veritabanı dökümü önce sunucunun salt okunur modda yerleştirilmelidir. Salt okunur modundayken, ana tüm yazma işlemleri işlenemiyor. İş etkisi önlemeye yardımcı olmak için salt okunur penceresi sırasında yoğun olmayan bir saat zamanlayın.

   ```sql
   FLUSH TABLES WITH READ LOCK;
   SET GLOBAL read_only = ON;
   ```

5. Uzaklık ve geçerli ikili günlük dosyası adını alın.

   Uzaklık ve geçerli ikili günlük dosyası adını belirlemek için komutu çalıştırmak [ `show master status` ](https://mariadb.com/kb/en/library/show-master-status/).
    
   ```sql
   show master status;
   ```
   Sonuçlar aşağıdaki tabloya benzer olmalıdır:
   
   ![Ana durum sonuçları](./media/howto-data-in-replication/masterstatus.png)

   Sonraki adımlarda kullanılacağı çünkü ikili dosya adını not edin.
   
6. GTID konumu (isteğe bağlı, GTID çoğaltma için gerekli) alın.

   İşlevi çalıştırmak [ `BINLOG_GTID_POS` ](https://mariadb.com/kb/en/library/binlog_gtid_pos/) GTID konumu uzaklığı ve karşılık gelen binlog dosya adı için alınamıyor.
  
    ```sql
    select BINLOG_GTID_POS('<binlog file name>', <binlog offset>);
    ```
 

## <a name="dump-and-restore-the-master-server"></a>Döküm ve ana sunucuyu geri yükleme

1. Ana sunucu tüm veritabanlarından dökümü.

   Mysqldump ana sunucu tüm veritabanlarından kullanın. MySQL kitaplığı dökümü ve bir test kitaplığının gerekmez.

    Daha fazla bilgi için [döküm ve geri yükleme](howto-migrate-dump-restore.md).

2. Okuma/yazma modu için ana sunucu olarak ayarlayın.

   Asıl veritabanına yazılan sonra değiştirmek için okuma/yazma modu MariaDB sunucusu geri.

   ```sql
   SET GLOBAL read_only = OFF;
   UNLOCK TABLES;
   ```

3. Bilgi döküm dosyası, yeni sunucuya geri yükleyin.

   Bilgi döküm dosyası MariaDB hizmeti için Azure veritabanı'nda oluşturulan sunucuya geri yükleyin. Bkz: [dökümü & Geri](howto-migrate-dump-restore.md) MariaDB sunucuya bir döküm dosyası geri yükleme için.

   Bilgi döküm dosyası büyükse, azure'da bir sanal makinesi için çoğaltma sunucunuz ile aynı bölgede karşıya. Sanal makineden MariaDB için Azure veritabanı için geri yükleyin.

## <a name="link-the-master-and-replica-servers-to-start-data-in-replication"></a>Veri çoğaltma başlatmak için ana ve çoğaltma sunucuları bağlama

1. Ana sunucu olarak ayarlayın.

   Tüm veri çoğaltma işlevleri saklı yordamlar tarafından gerçekleştirilir. Tüm işlemleri bulabilirsiniz [verileri, çoğaltma saklı yordamları](reference-data-in-stored-procedures.md). Saklı yordamlar MySQL kabuğu veya MySQL Workbench içinde çalıştırabilirsiniz.

   Bağlantı iki sunucu ve çoğaltma başlatmak için hedef çoğaltma sunucusuna MariaDB hizmeti için Azure DB'de oturum açın. Ardından, dış örneği kullanarak ana sunucu olarak ayarlamak `mysql.az_replication_change_master` veya `mysql.az_replication_change_master_with_gtid` MariaDB sunucusu için Azure veritabanı üzerinde saklı yordam.

   ```sql
   CALL mysql.az_replication_change_master('<master_host>', '<master_user>', '<master_password>', 3306, '<master_log_file>', <master_log_pos>, '<master_ssl_ca>');
   ```
   
   or
   
   ```sql
   CALL mysql.az_replication_change_master_with_gtid('<master_host>', '<master_user>', '<master_password>', 3306, '<master_gtid_pos>', '<master_ssl_ca>');
   ```

   - master_host: ana sunucusunun konak adı
   - master_user: ana sunucu için kullanıcı adı
   - master_password: ana sunucu için parola
   - master_log_file: çalışmasını ikili günlük dosyası adı `show master status`
   - master_log_pos: çalışmasını ikili günlük konumu `show master status`
   - master_gtid_pos: Çalışan GTID konumu `select BINLOG_GTID_POS('<binlog file name>', <binlog offset>);`
   - master_ssl_ca: CA sertifikanın bağlamı. SSL kullanmıyorsanız boş bir string.* geçirin
    
    
    \* Bir değişken olarak geçirme master_ssl_ca parametresinde öneririz. Daha fazla bilgi için aşağıdaki örneklere bakın.

   **Örnekler**

   - SSL ile çoğaltma

       Değişkeni oluşturun `@cert` aşağıdaki komutları çalıştırarak:

       ```sql
       SET @cert = '-----BEGIN CERTIFICATE-----
       PLACE YOUR PUBLIC KEY CERTIFICATE’S CONTEXT HERE
       -----END CERTIFICATE-----'
       ```

       SSL ile çoğaltma etki alanı sirketb.com içinde barındırılan bir ana sunucu MariaDB için Azure veritabanı'nda barındırılan bir çoğaltma sunucusu arasında ayarlayın. Bu saklı yordamı, çoğaltma üzerinde çalıştırılır.
    
       ```sql
       CALL mysql.az_replication_change_master('master.companya.com', 'syncuser', 'P@ssword!', 3306, 'mariadb-bin.000016', 475, @cert);
       ```
   - SSL olmayan çoğaltma

       Etki alanı sirketb.com içinde barındırılan bir ana sunucu MariaDB için Azure veritabanı'nda barındırılan bir çoğaltma sunucusu arasında SSL olmadan çoğaltma ayarlanır. Bu saklı yordamı, çoğaltma üzerinde çalıştırılır.

       ```sql
       CALL mysql.az_replication_change_master('master.companya.com', 'syncuser', 'P@ssword!', 3306, 'mariadb-bin.000016', 475, '');
       ```

2. Çoğaltma başlatır.

   Çağrı `mysql.az_replication_start` saklı yordamı, çoğaltma başlatma.

   ```sql
   CALL mysql.az_replication_start;
   ```

3. Çoğaltma durumunu kontrol edin.

   Çağrı [ `show slave status` ](https://mariadb.com/kb/en/library/show-slave-status/) çoğaltma durumunu görüntülemek için çoğaltma sunucusundaki komutu.
    
   ```sql
   show slave status;
   ```

   Varsa `Slave_IO_Running` ve `Slave_SQL_Running` durumdaki `yes`, değeri `Seconds_Behind_Master` olduğu `0`, çoğaltma çalıştığından. `Seconds_Behind_Master` çoğaltmanın nasıl geç olduğunu gösterir. Değer yoksa `0`, ardından çoğaltma güncelleştirmeleri işliyor.

4. (Yalnızca GTID olmadan çoğaltma için gerekli) veri çoğaltma daha güvenli hale getirmek için karşılık gelen sunucu değişkenleri güncelleştirin.
    
    MariaDB, yerel çoğaltma sınırlaması nedeniyle ayarlamalısınız [ `sync_master_info` ](https://mariadb.com/kb/en/library/replication-and-binary-log-system-variables/#sync_master_info) ve [ `sync_relay_log_info` ](https://mariadb.com/kb/en/library/replication-and-binary-log-system-variables/#sync_relay_log_info) GTID senaryosu olmadan çoğaltma değişkenlerde.

    Bağımlı sunucunuzun denetleyin `sync_master_info` ve `sync_relay_log_info` veri çoğaltma olduğundan emin olmak için değişkenleri kararlı ve değişkenlerini ayarlamak `1`.
    
## <a name="other-stored-procedures"></a>Diğer saklı yordamlar

### <a name="stop-replication"></a>Çoğaltmayı Durdur

Ana ve çoğaltma sunucusu arasında çoğaltmayı durdurmak için aşağıdaki depolanan yordamı kullanın:

```sql
CALL mysql.az_replication_stop;
```

### <a name="remove-the-replication-relationship"></a>Çoğaltma ilişkisini kaldır

Ana ve çoğaltma sunucusu arasındaki ilişkiyi kaldırmak için aşağıdaki depolanan yordamı kullanın:

```sql
CALL mysql.az_replication_remove_master;
```

### <a name="skip-the-replication-error"></a>Çoğaltma hatası atla

Bir çoğaltma hatası atlayın ve çoğaltma izin vermek için aşağıdaki depolanan yordamı kullanın:
    
```sql
CALL mysql.az_replication_skip_counter;
```

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinin [veri çoğaltma](concepts-data-in-replication.md) MariaDB için Azure veritabanı.
