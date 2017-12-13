
1. Ayrıcalıkları yükseltmek için şunu yazın:
   
        sudo -s
   
    Parolanızı girin.
2. MySQL Community Server edition'ı yüklemek için şunu yazın:
   
        zypper install mysql-community-server
   
    MySQL indirilir ve yüklenir bekleyin.
3. Sistem önyüklendiğinde başlatmak için MySQL ayarlamak için şunu yazın:
   
        insserv mysql
4. MySQL arka plan programı (mysqld), bu komutla el ile başlatın:
   
        rcmysql start
   
    MySQL arka plan programı durumunu denetlemek için şunu yazın:
   
        rcmysql status
   
    MySQL arka plan programı durdurmak için şunu yazın:
   
        rcmysql stop
   
   > [!IMPORTANT]
   > Yükleme sonrasında, MySQL kök parola varsayılan olarak boştur. Çalıştırmanızı öneririz **mysql\_güvenli\_yükleme**, güvenli MySQL yardımcı olan bir komut dosyası. Komut dosyası MySQL kök parolasını değiştirmek, anonim kullanıcı hesaplarını kaldırın, uzak kök oturum açmalar devre dışı bırakmak, test veritabanlarını kaldırmanız ve ayrıcalıkları tablo yeniden ister. Bu seçeneklerin Tümüne Evet yanıtlayın ve kök parola değiştirme önerilir.
   > 
   > 
5. MySQL yükleme betiği komut dosyasını çalıştırmak için bunu yazın:
   
        mysql_secure_installation
6. MySQL için oturum açın:
   
        mysql -u root -p
   
    (Önceki adımda değiştirdiğiniz) MySQL kök parolayı girin ve veritabanıyla etkileşim için SQL deyimleri nerede sorun içeren bir istem sunulur.
7. Yeni bir MySQL kullanıcı oluşturmak için aşağıdaki komutu çalıştırın **mysql >** istemi:
   
        CREATE USER 'mysqluser'@'localhost' IDENTIFIED BY 'password';
   
    Not, noktalı virgül (;) satırın sonuna komutu bitiş için önemlidir.
8. Bir veritabanı oluşturmak ve vermek için `mysqluser` , kullanıcı izinlerini vermek aşağıdaki komutlar:
   
        CREATE DATABASE testdatabase;
        GRANT ALL ON testdatabase.* TO 'mysqluser'@'localhost' IDENTIFIED BY 'password';
   
    Veritabanı kullanıcı adları ve parolalar yalnızca veritabanına bağlanırken komut dosyaları tarafından kullanıldığını unutmayın.  Veritabanı kullanıcı hesabı adları mutlaka gerçek kullanıcı hesapları sistem üzerindeki temsil etmiyor.
9. Başka bir bilgisayardan oturum açmak için şunu yazın:
   
        GRANT ALL ON testdatabase.* TO 'mysqluser'@'<ip-address>' IDENTIFIED BY 'password';
   
    Burada `ip-address` MySQL için bağlandığınız bilgisayarın IP adresidir.
10. MySQL veritabanı yönetim yardımcı programı'ndan çıkmak için şunu yazın:
    
        quit

## <a name="add-an-endpoint"></a>Bir uç nokta ekleme
1. MySQL yüklendikten sonra MySQL uzaktan erişmek için bir uç nokta yapılandırmanız gerekir. Oturum [Azure portal][AzurePortal]. Tıklatın **sanal makineleri**, yeni bir sanal makine adına tıklayın ve ardından **uç noktaları**.
2. Tıklatın **Ekle** sayfanın sonundaki.
3. Protokolüyle "MySQL" adlı bir uç nokta ekleyin **TCP**, ve **ortak** ve **özel** bağlantı noktası "3306" ayarlayın.
4. Uzaktan bilgisayarınızdan sanal makineye bağlanmak için şunu yazın:
   
        mysql -u mysqluser -p -h <yourservicename>.cloudapp.net
   
    Örneğin, bu öğreticide oluşturduğunuz sanal makine kullanarak, bu komutu yazın:
   
        mysql -u mysqluser -p -h testlinuxvm.cloudapp.net

[MySQLDocs]: http://dev.mysql.com/doc/
[AzurePortal]: http://portal.azure.com

[Image9]: ./media/install-and-run-mysql-on-opensuse-vm/LinuxVmAddEndpointMySQL.png
