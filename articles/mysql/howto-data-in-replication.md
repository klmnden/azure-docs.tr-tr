---
title: MySQL için Azure veritabanı'na veri çoğaltmak için veri çoğaltma yapılandırın.
description: Bu makalede, veri çoğaltma için Azure veritabanı için MySQL ayarlanacağını açıklar.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 08/31/2018
ms.openlocfilehash: 7d56d7f8fcbd53d4f69863d260591ef80f3d7188
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61459105"
---
# <a name="how-to-configure-azure-database-for-mysql-data-in-replication"></a>Azure veritabanını MySQL veri çoğaltma için yapılandırma

Bu makalede, veri çoğaltma hizmeti MySQL için Azure veritabanı'nda ana ve çoğaltma sunucuları yapılandırma tarafından nasıl ayarlanacağını öğreneceksiniz. Veri çoğaltma, verileri şirket içi sanal makineleri veya diğer bulut sağlayıcılarının hizmet MySQL için Azure veritabanı'nda çoğaltmayı içine barındırılan veritabanı Hizmetleri çalıştıran ana bir MySQL sunucusu eşitlemenize olanak tanır. 

Bu makalede, MySQL sunucuları ve veritabanları ile konusunda en azından biraz deneyim sahibi olduğunuzu varsayar.

## <a name="create-a-mysql-server-to-be-used-as-replica"></a>Çoğaltma olarak kullanılacak bir MySQL sunucusu oluşturma

1. MySQL sunucusu için yeni bir Azure veritabanı oluşturma

   (Ör. yeni bir MySQL sunucusu oluşturma "replica.mysql.database.azure.com"). Başvurmak [Azure portalını kullanarak MySQL sunucusu için Azure veritabanı oluşturma](quickstart-create-mysql-server-database-using-azure-portal.md) sunucu oluşturma. Bu sunucu, veri çoğaltma "çoğaltma" sunucusudur.

   > [!IMPORTANT]
   > MySQL sunucusu için Azure veritabanı genel amaçlı veya bellek için iyileştirilmiş fiyatlandırma katmanında oluşturulması gerekir.
   > 

2. Aynı kullanıcı hesapları ve karşılık gelen ayrıcalıkları oluşturun

   Kullanıcı hesapları ana sunucu ile çoğaltma sunucusuna çoğaltılmaz. Çoğaltma sunucusuna erişimi olan kullanıcıları sağlama planlıyorsanız, bu yeni MySQL server için Azure veritabanı oluşturulan tüm hesaplar ve karşılık gelen ayrıcalıkları el ile oluşturmanız gerekir.

## <a name="configure-the-master-server"></a>Ana sunucuyu yapılandırma
Aşağıdaki adımlar, hazırlama ve MySQL server barındırılan şirket içinde bir sanal makine veya veri çoğaltma için diğer bulut sağlayıcıları tarafından barındırılan bir veritabanı hizmeti yapılandırın. Bu veri çoğaltma "ana" sunucusudur. 

1. İkili günlük özelliğini açar

   İkili günlük ana aşağıdaki komutu çalıştırarak etkinleştirilmiş olup olmadığını denetleyin: 

   ```sql
   SHOW VARIABLES LIKE 'log_bin';
   ```

   Değişken [ `log_bin` ](https://dev.mysql.com/doc/refman/8.0/en/replication-options-binary-log.html#sysvar_log_bin) döndürülen değer "ON ile" ikili günlük sunucuda etkin. 

   Varsa `log_bin` olduğundan döndürülen değer "OFF" ile bu nedenle my.cnf dosyanızı düzenleyerek günlüğü ikili, açma `log_bin=ON` ve değişikliğin etkili olması sunucunuzu yeniden başlatın.

2. Ana sunucu ayarları

   Veri çoğaltma parametresi gerektirir `lower_case_table_names` ana ve çoğaltma sunucuları arasında tutarlı olması. Bu parametre, MySQL için Azure veritabanı'nda varsayılan olarak 1 olur. 

   ```sql
   SET GLOBAL lower_case_table_names = 1;
   ```

3. Yeni bir çoğaltma rolü oluştur ve izni ayarlayın

   Çoğaltma ayrıcalıkları ile yapılandırılan ana sunucu üzerinde bir kullanıcı hesabı oluşturun. Bu SQL komutlarını veya MySQL Workbench gibi bir araç aracılığıyla yapılabilir. Bu kullanıcı oluştururken belirtilmesi gerektiğinden SSL ile yineleme planlama olup olmadığını göz önünde bulundurun. Anlamak için MySQL belgeleri için başvurmak için nasıl [kullanıcı hesaplarını eklemek](https://dev.mysql.com/doc/refman/5.7/en/adding-users.html) ana sunucunuz üzerindeki. 

   Aşağıdaki komutları oluşturulan yeni çoğaltma rolü herhangi bir makineden, yalnızca yöneticisini barındıran bilgisayarın asıl erişebilir. Bu belirtilerek yapılır "syncuser@'%'" oluşturma kullanıcı komutu. MySQL belgeleri hakkında daha fazla bilgi için bkz. [hesap adlarını belirten](https://dev.mysql.com/doc/refman/5.7/en/account-names.html).

   **SQL komutu**

   *SSL ile çoğaltma*

   Tüm kullanıcı bağlantılar için SSL gerektirmek için bir kullanıcı oluşturmak için aşağıdaki komutu kullanın: 

   ```sql
   CREATE USER 'syncuser'@'%' IDENTIFIED BY 'yourpassword';
   GRANT REPLICATION SLAVE ON *.* TO ' syncuser'@'%' REQUIRE SSL;
   ```

   *SSL olmayan çoğaltma*

   SSL tüm bağlantılar için gerekli değilse, bir kullanıcı oluşturmak için aşağıdaki komutu kullanın:

   ```sql
   CREATE USER 'syncuser'@'%' IDENTIFIED BY 'yourpassword';
   GRANT REPLICATION SLAVE ON *.* TO ' syncuser'@'%';
   ```

   **MySQL Workbench**

   MySQL Workbench uygulamasında çoğaltma rolü oluşturmak için açık **kullanıcılar ve ayrıcalıkları** gelen panelinde **Yönetim** paneli. Ardından **hesabı Ekle**. 
 
   ![Kullanıcılar ve ayrıcalıkları](./media/howto-data-in-replication/users_privileges.png)

   Kullanıcı türü **oturum açma adı** alan. 

   ![Kullanıcıyı Eşitle](./media/howto-data-in-replication/syncuser.png)
 
   Tıklayarak **yönetim rolleri** paneli ve ardından **çoğaltma bağımlı** listesinden **genel ayrıcalıkları**. Ardından **Uygula** çoğaltma rolü oluşturmak için.

   ![Çoğaltma bağımlı](./media/howto-data-in-replication/replicationslave.png)


4. Ana sunucu salt okunur moda ayarlayın.

   Veritabanının ölçeğini dökümü başlamadan önce sunucunun salt okunur modunda yerleştirilmesi gerekir. Salt okunur modundayken, ana tüm yazma işlemleri mümkün olmayacaktır. İş etkisini değerlendirin ve salt okunur penceresi yoğun olmayan bir saat içinde gerekirse Zamanla.

   ```sql
   FLUSH TABLES WITH READ LOCK;
   SET GLOBAL read_only = ON;
   ```

5. İkili günlük dosyası adını ve uzaklık alma

   Çalıştırma [ `show master status` ](https://dev.mysql.com/doc/refman/5.7/en/show-master-status.html) uzaklığı ve geçerli ikili günlük dosyası adını belirlemek için komutu.
    
   ```sql
   show master status;
   ```
   Sonuçlar aşağıdaki gibi olmalıdır. Sonraki adımlarda kullanılacak gibi ikili dosya adını not aldığınızdan emin olun.

   ![Ana durum sonuçları](./media/howto-data-in-replication/masterstatus.png)
 
## <a name="dump-and-restore-master-server"></a>Döküm ve ana sunucusunu geri yükleme

1. Ana sunucu tüm veritabanlarından dökümü

   Mysqldump döküm veritabanlarına, ana daldan kullanabilirsiniz. Ayrıntılar için başvurmak [dökümü & Geri](concepts-migrate-dump-restore.md). MySQL kitaplığı dökümü ve bir test kitaplığının gereksizdir.

2. Okuma/yazma modu için ana sunucu kümesi

   Asıl veritabanına yazılan sonra değiştirmek için okuma/yazma modu MySQL sunucusu geri.

   ```sql
   SET GLOBAL read_only = OFF;
   UNLOCK TABLES;
   ```

3. Döküm dosyasını yeni sunucuya geri yükleme

   Bilgi döküm dosyası MySQL hizmeti için Azure veritabanı'nda oluşturulan sunucuya geri yükleyin. Başvurmak [dökümü & Geri](concepts-migrate-dump-restore.md) bir döküm dosyası için bir MySQL sunucusu geri yükleme için. Bilgi döküm dosyası büyükse, çoğaltma sunucunuz ile aynı bölgede azure'da bir sanal makineye yükleyin. Sanal makineden MySQL sunucusu için Azure veritabanına geri yükleyin.

## <a name="link-master-and-replica-servers-to-start-data-in-replication"></a>Veri çoğaltma başlatmak için Bağlantı Yöneticisi ve çoğaltma sunucuları

1. Ana sunucusunu ayarlama

   Tüm veri çoğaltma işlevleri saklı yordamlar tarafından gerçekleştirilir. Tüm işlemleri bulabilirsiniz [verileri, çoğaltma saklı yordamları](reference-data-in-stored-procedures.md). Saklı yordamları MySQL kabuğu veya MySQL Workbench içinde çalıştırabilirsiniz. 

   İki sunucu bağlantı çoğaltması, hedef çoğaltma sunucusuna MySQL hizmeti için Azure DB'de oturum açma başlatmak ve dış örneği ana sunucu olarak ayarlamak için. Bu kullanılarak yapılır `mysql.az_replication_change_master` bir MySQL sunucusu için Azure veritabanı üzerinde saklı yordam.

   ```sql
   CALL mysql.az_replication_change_master('<master_host>', '<master_user>', '<master_password>', 3306, '<master_log_file>', <master_log_pos>, '<master_ssl_ca>');
   ```

   - master_host: ana sunucusunun konak adı
   - master_user: ana sunucu için kullanıcı adı
   - master_password: ana sunucu için parola
   - master_log_file: çalışmasını ikili günlük dosyası adı `show master status`
   - master_log_pos: çalışmasını ikili günlük konumu `show master status`
   - master_ssl_ca: CA sertifikanın bağlamı. SSL kullanılmıyorsa, boş bir dize geçirin.
       - Bu parametrede bir değişken olarak geçirmek için önerilir. Daha fazla bilgi için aşağıdaki örneklere bakın.

> [!NOTE]
> Ana sunucusu Azure VM'deki barındırılıyorsa "birbirleri ile iletişim kurmak ana ve çoğaltma sunucuları izin vermek için"on"Azure hizmetlerine erişime izin ver" olarak ayarlayın. Bu ayar, gelen değiştirilebilir **bağlantı güvenliği** seçenekleri. Başvurmak [portalını kullanarak güvenlik duvarı kurallarını yönetme](howto-manage-firewall-using-portal.md) daha fazla bilgi için.

   **Örnekler**

   *SSL ile çoğaltma*

   Değişken `@cert` MySQL aşağıdaki komutları çalıştırarak oluşturulur: 

   ```sql
   SET @cert = '-----BEGIN CERTIFICATE-----
   PLACE YOUR PUBLIC KEY CERTIFICATE’S CONTEXT HERE
   -----END CERTIFICATE-----'
   ```

   MySQL için Azure veritabanı'nda barındırılan bir çoğaltma sunucusu ile "sirketb.com" etki alanında barındırılan bir ana sunucu arasında SSL ile çoğaltma ayarlanır. Bu saklı yordamı, çoğaltma üzerinde çalıştırılır. 

   ```sql
   CALL mysql.az_replication_change_master('master.companya.com', 'syncuser', 'P@ssword!', 3306, 'mysql-bin.000002', 120, @cert);
   ```
   *SSL olmayan çoğaltma*

   MySQL için Azure veritabanı'nda barındırılan bir çoğaltma sunucusu ile "sirketb.com" etki alanında barındırılan bir ana sunucu arasında SSL olmadan çoğaltma ayarlanır. Bu saklı yordamı, çoğaltma üzerinde çalıştırılır.

   ```sql
   CALL mysql.az_replication_change_master('master.companya.com', 'syncuser', 'P@ssword!', 3306, 'mysql-bin.000002', 120, '');
   ```

1. Çoğaltmayı Başlat

   Çağrı `mysql.az_replication_start` saklı yordamı, çoğaltma başlatmak için.

   ```sql
   CALL mysql.az_replication_start;
   ```

1. Çoğaltma durumunu denetleme

   Çağrı [ `show slave status` ](https://dev.mysql.com/doc/refman/5.7/en/show-slave-status.html) çoğaltma durumunu görüntülemek için çoğaltma sunucusundaki komutu.
    
   ```sql
   show slave status;
   ```

   Varsa durumunu `Slave_IO_Running` ve `Slave_SQL_Running` "Evet"'i ve değerini `Seconds_Behind_Master` "0", çoğaltma düzgün çalışıyor. `Seconds_Behind_Master` çoğaltmanın nasıl geç olduğunu gösterir. Değer ise "0" geldiğini çoğaltma güncelleştirmeleri işliyor. 

## <a name="other-stored-procedures"></a>Diğer saklı yordamlar

### <a name="stop-replication"></a>Çoğaltmayı durdur

Ana ve çoğaltma sunucusu arasında çoğaltmayı durdurmak için aşağıdaki depolanan yordamı kullanın:

```sql
CALL mysql.az_replication_stop;
```

### <a name="remove-replication-relationship"></a>Çoğaltma ilişkisini kaldır

Ana ve çoğaltma sunucusu arasındaki ilişkiyi kaldırmak için aşağıdaki depolanan yordamı kullanın:

```sql
CALL mysql.az_replication_remove_master;
```

### <a name="skip-replication-error"></a>Çoğaltma hatası atla

Bir çoğaltma hatası atla ve devam etmek çoğaltma izin vermek için aşağıdaki depolanan yordamı kullanın:
    
```sql
CALL mysql.az_replication_skip_counter;
```

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [veri çoğaltma](concepts-data-in-replication.md) MySQL için Azure veritabanı. 
