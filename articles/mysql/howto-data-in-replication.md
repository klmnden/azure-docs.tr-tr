---
title: MySQL için Azure veritabanına veri çoğaltmak için verileri, çoğaltmayı yapılandırmak için.
description: Bu makalede, verileri, Azure veritabanı için MySQL için çoğaltma ayarlama açıklar.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 05/18/2018
ms.openlocfilehash: f9517cb552130e340310abc4affdad8bdadc26fe
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35265760"
---
# <a name="how-to-configure-azure-database-for-mysql-data-in-replication"></a>Azure veritabanı için MySQL veri çoğaltma yapılandırma

Bu makalede, verileri, MySQL hizmeti tarafından Azure veritabanı çoğaltmasında yapılandırma birincil nasıl ayarlanacağını ve çoğaltma sunucuları öğreneceksiniz.

Bu makalede, MySQL sunucuları ve veritabanlarıyla konusunda en az biraz deneyim sahibi olduğunuzu varsayar.

## <a name="create-a-mysql-server-to-be-used-as-replica"></a>Çoğaltma olarak kullanılacak bir MySQL server oluşturun

1. MySQL sunucusu için yeni bir Azure veritabanı oluştur

   Yeni bir MySQL server (örneğin oluşturun "replica.mysql.database.azure.com"). Başvurmak [Azure portalını kullanarak MySQL sunucusu için bir Azure veritabanı oluşturun](quickstart-create-mysql-server-database-using-azure-portal.md) sunucu oluşturma. Bu sunucu, verileri, çoğaltma "çoğaltma" sunucusudur.

   > [!IMPORTANT]
   > Bu sunucu genel amaçlı veya bellek için iyileştirilmiş fiyatlandırma katmanlarına oluşturulması gerekir.
   > 

2. Aynı kullanıcı hesapları ve karşılık gelen ayrıcalıkları oluşturun

   Kullanıcı hesapları birincil sunucudan çoğaltma sunucusuna çoğaltılmaz. Çoğaltma sunucusuna erişimi olan kullanıcılara sağlamaya planlıyorsanız, bu yeni Azure veritabanı MySQL sunucusu için oluşturulan tüm hesaplar ve karşılık gelen ayrıcalıkları el ile oluşturmanız gerekir.

## <a name="configure-the-primary-server"></a>Birincil sunucuyu yapılandırma

1. İkili günlüğünü etkinleştirme

   İkili günlük birincil aşağıdaki komutu çalıştırarak etkinleştirilmiş olup olmadığını denetleyin: 

   ```sql
   SHOW VARIABLES LIKE 'log_bin';
   ```

   Varsa değişkeni [ `log_bin` ](https://dev.mysql.com/doc/refman/8.0/en/replication-options-binary-log.html#sysvar_log_bin) döndürülen değer "ON ile" ikili günlük sunucunuzda etkinleşir. 

   Varsa `log_bin` olan "OFF" değeriyle döndürdü, ikili my.cnf dosyanızı şekilde düzenleyerek günlüğü, Aç `log_bin=ON` ve değişikliğin etkili olması, sunucuyu yeniden başlatın.

2. Birincil sunucu ayarları

   Verileri, çoğaltma parametresi gerektirir `lower_case_table_names` birincil ve çoğaltma sunucuları arasında tutarlı olmalıdır. Bu parametre Azure veritabanında MySQL için varsayılan olarak 1'dir. 

   ```sql
   SET GLOBAL lower_case_table_names = 1;
   ```

3. Yeni bir çoğaltma rolü oluşturun ve izni ayarlamak

   Birincil sunucuda çoğaltma ayrıcalıklarıyla yapılandırılmış olan bir kullanıcı hesabı oluşturun. Bu SQL komutlarını veya MySQL çalışma ekranı gibi bir araç aracılığıyla yapılabilir. Bu kullanıcı oluşturulurken belirtilmesi ihtiyaç duyacağınız SSL ile çoğaltma planlama olup olmadığını göz önünde bulundurun. Anlamak için MySQL belgelere bakın nasıl [kullanıcı hesapları ekleme](https://dev.mysql.com/doc/refman/5.7/en/adding-users.html) birincil sunucunuzda. 

   Aşağıdaki komutları oluşturulan yeni çoğaltma rolü herhangi makineden yalnızca birincil kendisini barındıran makine birincil erişebilir. Bu belirterek yapılır "syncuser@'%'" create kullanıcı komutu. Daha fazla bilgi edinmek için MySQL belgelerine bakın [hesap adlarını belirtme](https://dev.mysql.com/doc/refman/5.7/en/account-names.html).

   **SQL komutu**

   *SSL ile çoğaltma*

   Tüm kullanıcı bağlantıları için SSL gerektirmek için bir kullanıcı oluşturmak için aşağıdaki komutu kullanın: 

   ```sql
   CREATE USER 'syncuser'@'%' IDENTIFIED BY 'yourpassword';
   GRANT REPLICATION SLAVE ON *.* TO ' syncuser'@'%' REQUIRE SSL;
   ```

   *SSL olmadan çoğaltma*

   SSL bağlantıları için tüm gerekli değilse, bir kullanıcı oluşturmak için aşağıdaki komutu kullanın:

   ```sql
   CREATE USER 'syncuser'@'%' IDENTIFIED BY 'yourpassword';
   GRANT REPLICATION SLAVE ON *.* TO ' syncuser'@'%';
   ```

   **MySQL çalışma ekranı**

   MySQL çalışma ekranı çoğaltma rolü oluşturmak için açık **kullanıcılar ve ayrıcalıkları** gelen panel **Yönetim** paneli. Tıklayın **hesabı Ekle**. 
 
   ![Kullanıcılar ve ayrıcalıkları](./media/howto-data-in-replication/users_privileges.png)

   Kullanıcı türü **oturum açma adı** alan. 

   ![Eşitleme kullanıcı](./media/howto-data-in-replication/syncuser.png)
 
   Tıklayın **yönetici rollerini** panel ve ardından **çoğaltma bağımlı** listesinden **genel ayrıcalıkları**. Tıklayın **Uygula** çoğaltma rolü oluşturmak için.

   ![Çoğaltma bağımlı](./media/howto-data-in-replication/replicationslave.png)


4. Salt okunur moda birincil sunucuyu ayarlama

   Veritabanını dökümü başlatmadan önce sunucunun salt okunur modda yerleştirilmesi gerekir. Salt okunur modundayken, birincil yazma işlemler işleyemiyor olacaktır. İş üzerindeki etkileri değerlendirmek ve yoğun olmayan bir saat içinde gerekirse, salt okunur penceresi zamanlayabilirsiniz.

   ```sql
   FLUSH TABLES WITH READ LOCK;
   SET GLOBAL read_only = ON;
   ```

5. İkili günlük dosyasının adını ve uzaklık Al

   Çalıştırma [ `show master status` ](https://dev.mysql.com/doc/refman/5.7/en/show-master-status.html) geçerli ikili günlük dosyasının adını ve uzaklık belirlemek için komutu.
    
   ```sql
   show master status;
   ```
   Sonuçları aşağıdaki gibi olması gerekir. Sonraki adımlarda kullanılacak şekilde ikili dosya adını not emin olun.

   ![Ana durum sonuçları](./media/howto-data-in-replication/masterstatus.png)
 
## <a name="dump-and-restore-primary-server"></a>Döküm ve birincil sunucuyu geri yükle

1. Birincil sunucudan tüm veritabanları dökümü

   Birincil sunucudan döküm veritabanlarına mysqldump kullanabilirsiniz. Ayrıntılar için başvurmak [dökümü & Geri](concepts-migrate-dump-restore.md). MySQL kitaplığı dökümü ve kitaplığı test etmek gerekli değildir.

2. Okuma/yazma modu için birincil sunucuyu ayarlama

   Birincil veritabanına yazılan sonra değiştirmek okuma/yazma modu için MySQL server geri.

   ```sql
   SET GLOBAL read_only = OFF;
   UNLOCK TABLES;
   ```

3. Döküm dosyası yeni bir sunucuya geri yükleme

   Döküm dosyası MySQL hizmeti için Azure veritabanında oluşturulan sunucusu geri yükleyin. Başvurmak [dökümü & Geri](concepts-migrate-dump-restore.md) MySQL sunucusu için bir döküm dosyası geri yükleme için. Döküm dosyası büyükse, çoğaltma sunucunuz ile aynı bölgedeki azure'da bir sanal makineye yükleyin. Sanal makineden MySQL sunucusu için Azure veritabanına geri.

## <a name="link-primary-and-replica-servers-to-start-data-in-replication"></a>Bağlantı birincil ve çoğaltma sunucusu verileri, çoğaltmayı Başlat

1. Birincil sunucusunu ayarlama

   Tüm verileri, çoğaltma işlevlerini saklı yordamlar tarafından yapılır. Tüm işlemleri bulabilirsiniz [verileri, çoğaltma saklı yordamları](reference-data-in-stored-procedures.md). Saklı yordamlar MySQL Kabuk veya MySQL çalışma ekranı çalıştırabilirsiniz. 

   İçin iki sunucu bağlantı ve çoğaltma, hedef çoğaltma sunucusuna MySQL hizmeti için Azure DB'de oturum açma başlatmak ve dış birincil sunucu olarak ayarlayın. Bu kullanılarak yapılır `mysql.az_replication_change_primary` MySQL sunucusu için Azure DB üzerinde saklı yordamı.

   ```sql
   CALL mysql.az_replication_change_primary('<master_host>', '<master_user>', '<master_password>', 3306, '<master_log_file>', <master_log_pos>, '<master_ssl_ca>');
   ```

   - master_host: birincil sunucusunun ana bilgisayar adı
   - master_user: birincil sunucu için kullanıcı adı
   - master_password: birincil sunucusu için parola
   - master_log_file: çalışmasını ikili günlük dosyası adı `show master status`
   - master_log_pos: çalışmasını ikili günlük konumu `show master status`
   - master_ssl_ca: CA sertifikasının bağlamı. SSL, boş bir dize geçişinde kullanılmıyorsa.
       - Bu parametre bir değişken olarak geçirmek için önerilir. Daha fazla bilgi için aşağıdaki örneklere bakın.

   **Örnekler**

   *SSL ile çoğaltma*

   Değişkeni `@cert` aşağıdaki MySQL komutlarını çalıştırarak oluşturulur: 

   ```sql
   SET @cert = '-----BEGIN CERTIFICATE-----
   PLACE YOUR PUBLIC KEY CERTIFICATE’S CONTEXT HERE
   -----END CERTIFICATE-----'
   ```

   SSL ile çoğaltma "sirketb.com" etki alanında barındırılan bir birincil sunucu için MySQL Azure veritabanı'nda barındırılan bir çoğaltma sunucusu arasında ayarlayın. Bu saklı yordam kopyada çalıştırılır. 

   ```sql
   CALL mysql.az_replication_change_primary('primary.companya.com', 'syncuser', 'P@ssword!', 3306, 'mysql-bin.000002', 120, @cert);
   ```
   *SSL olmadan çoğaltma*

   SSL olmadan çoğaltma "sirketb.com" etki alanında barındırılan bir birincil sunucu için MySQL Azure veritabanı'nda barındırılan bir çoğaltma sunucusu arasında ayarlayın. Bu saklı yordam kopyada çalıştırılır.

   ```sql
   CALL mysql.az_replication_change_primary('primary.companya.com', 'syncuser', 'P@ssword!', 3306, 'mysql-bin.000002', 120, '');
   ```

2. Çoğaltmayı Başlat

   Çağrı `mysql.az_replication_start` saklı yordamı çoğaltmayı başlatır.

   ```sql
   CALL mysql.az_replication_start;
   ```

3. Çoğaltma durumunu denetleme

   Çağrı [ `show slave status` ](https://dev.mysql.com/doc/refman/5.7/en/show-slave-status.html) çoğaltma durumunu görüntülemek için çoğaltma sunucusunda komutu.
    
   ```sql
   show slave status;
   ```

   Varsa durumunu `Slave_IO_Running` ve `Slave_SQL_Running` "Evet" ve değeri `Seconds_Behind_Master` "0", çoğaltma iyi çalışma. `Seconds_Behind_Master` Çoğaltma nasıl geç olduğunu gösterir. Değer değilse "0" geldiğini çoğaltma güncelleştirmeleri işliyor. 

## <a name="other-stored-procedures"></a>Diğer saklı yordamlar

### <a name="stop-replication"></a>Çoğaltmayı durdur

Birincil ve çoğaltma sunucusu arasında çoğaltmayı durdurmak için aşağıdaki saklı yordamı kullanın:

```sql
CALL mysql.az_replication_stop;
```

### <a name="remove-replication-relationship"></a>Çoğaltma ilişkisini kaldırın

Birincil ve çoğaltma sunucusu arasındaki ilişkiyi kaldırmak için aşağıdaki depolanan yordamı kullanın:

```sql
CALL mysql.az_replication_remove_primary;
```

### <a name="skip-replication-error"></a>Çoğaltma hatası atla

Bir çoğaltma hatası atlayın ve devam etmek çoğaltma izin vermek için aşağıdaki saklı yordamı kullanın:
    
```sql
CALL mysql.az_replication_skip_counter;
```