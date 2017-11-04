## <a name="install-wordpress"></a>WordPress yükleme

Yığın denemek istiyorsanız, örnek bir uygulama yükleyin. Örnek olarak, aşağıdaki adımları açık kaynak yüklemek [WordPress](https://wordpress.org/) Web siteleri ve Web günlükleri oluşturmak için platform. Denemek için diğer iş yükleri içerir [Drupal](http://www.drupal.org) ve [Moodle](https://moodle.org/). 

Kavram kanıtı için bu WordPress kurulur. Daha fazla bilgi ve üretim yüklemesine yönelik ayarlar için bkz: [WordPress belgelerine](https://codex.wordpress.org/Main_Page). 



### <a name="install-the-wordpress-package"></a>WordPress paketini yükle

Şu komutu çalıştırın:

```bash
sudo apt install wordpress
```

### <a name="configure-wordpress"></a>WordPress’i yapılandırma

WordPress MySQL ve PHP kullanacak şekilde yapılandırın. Tercih ettiğiniz bir metin düzenleyicisinde açın ve dosyayı oluşturmak için aşağıdaki komutu çalıştırın `/etc/wordpress/config-localhost.php`:

```bash
sudo sensible-editor /etc/wordpress/config-localhost.php
```
Veritabanı parolasını değiştirme dosyası aşağıdaki satırları kopyalamak *yourPassword* (diğer değerleri değiştirmeden bırakın). Ardından dosyayı kaydedin.

```php
<?php
define('DB_NAME', 'wordpress');
define('DB_USER', 'wordpress');
define('DB_PASSWORD', 'yourPassword');
define('DB_HOST', 'localhost');
define('WP_CONTENT_DIR', '/usr/share/wordpress/wp-content');
?>
```

Bir çalışma dizini içinde bir metin dosyası oluşturun `wordpress.sql` WordPress veritabanını yapılandırmak için: 

```bash
sudo sensible-editor wordpress.sql
```

Veritabanı parolasını değiştirerek aşağıdaki komutları ekleme *yourPassword* (diğer değerleri değiştirmeden bırakın). Ardından dosyayı kaydedin.

```sql
CREATE DATABASE wordpress;
GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,DROP,ALTER
ON wordpress.*
TO wordpress@localhost
IDENTIFIED BY 'yourPassword';
FLUSH PRIVILEGES;
```


Veritabanını oluşturmak için aşağıdaki komutu çalıştırın:

```bash
cat wordpress.sql | sudo mysql --defaults-extra-file=/etc/mysql/debian.cnf
```

Komut tamamlandığında, dosyayı silin `wordpress.sql`.

WordPress yükleme için web sunucusu belge kökü Taşı:

```bash
sudo ln -s /usr/share/wordpress /var/www/html/wordpress

sudo mv /etc/wordpress/config-localhost.php /etc/wordpress/config-default.php
```

Şimdi WordPress Kurulumu tamamlamak ve platformda yayımlayın. Bir tarayıcı açın ve gidin `http://yourPublicIPAddress/wordpress`. VM ortak IP adresini değiştirin. Bu görüntüsüne benzer görünmelidir.

![WordPress yükleme sayfası](./media/virtual-machines-linux-tutorial-wordpress/wordpressstartpage.png)