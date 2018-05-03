Yüklemek ve Windows Server çalıştıran bir sanal makinede MongoDB çalıştırmak için aşağıdaki adımları izleyin.

> [!IMPORTANT]
> Kimlik doğrulaması ve IP adresi bağlama gibi MongoDB güvenlik özellikleri varsayılan olarak etkin değildir. Güvenlik özellikleri, MongoDB üretim ortamına dağıtmadan önce etkinleştirilmelidir.  Daha fazla bilgi için bkz: [güvenlik ve kimlik doğrulama](http://www.mongodb.org/display/DOCS/Security+and+Authentication).
>
>

1. Uzak Masaüstü kullanarak sanal makineye bağlandıktan sonra Internet Explorer'dan açmak **Başlat** sanal makine menüsünde.
2. Seçin **Araçları** sağ üst köşesinde düğmesini.  İçinde **Internet Seçenekleri**seçin **güvenlik** sekmesini tıklatın ve ardından **Güvenilen siteler** simgesi ve son olarak tıklayın **siteleri** düğmesi. Ekleme *https://\*. mongodb.org* Güvenilen siteler listesine.
3. Git [yüklemeleri - MongoDB](https://www.mongodb.com/download-center#community).
4. Bul **geçerli durağan sürümü** , **Community Server**, en son seçin **64-bit** Windows sütununda sürümü. Karşıdan yükle ve MSI yükleyicisi çalıştırın.
5. MongoDB genellikle C:\Program Files\MongoDB yüklenir. Ortam değişkenleri için masaüstünde arayın ve PATH değişkenine MongoDB ikili dosyalarının yolunu ekleyin. Örneğin, C:\Program Files\MongoDB\Server\3.4\bin ikili dosyaları makinenizde bulabilirsiniz.
6. MongoDB veri ve günlük dizinleri veri disketi (sürücü gibi **F:**), önceki adımlarda oluşturduğunuz. Gelen **Başlat**seçin **komut istemi** bir komut istemi penceresi açın.  Şunu yazın:

        C:\> F:
        F:\> mkdir \MongoData
        F:\> mkdir \MongoLogs
7. Veritabanı çalıştırmak için çalıştırın:

        F:\> C:
        C:\> mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log

    Tüm günlük iletilerini yönlendirilir *F:\MongoLogs\mongolog.log* dosya mongod.exe sunucu başlar ve günlük dosyalarını preallocates gibi. Günlük dosyaları erişinceye ve bağlantıları dinlemeyi başlatmak MongoDB için birkaç dakika sürebilir. MongoDB örneğinizin çalışırken komut istemini bu görevde odaklanmış kalır.
8. MongoDB Yönetim Kabuğu'nu başlatmak için başka bir komut penceresinde açın **Başlat** ve aşağıdaki komutları yazın:

        C:\> cd \my_mongo_dir\bin  
        C:\my_mongo_dir\bin> mongo  
        >db  
        test
        > db.foo.insert( { a : 1 } )  
        > db.foo.find()  
        { _id : ..., a : 1 }  
        > show dbs  
        ...  
        > show collections  
        ...  
        > help  

    Veritabanı Ekle tarafından oluşturulur.
9. Alternatif olarak, bir hizmet olarak mongod.exe yükleyebilirsiniz:

        C:\> mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log --logappend  --install

    Bir hizmeti yüklendi "Mongo VT" açıklaması ile MongoDB adlı. `--logpath` Seçeneği, çalışan hizmetin çıktı görüntülemek için bir komut penceresi sahip olmadığından bir günlük dosyası belirtmek için kullanılmalıdır.  `--logappend` Seçeneği, hizmet yeniden varolan günlük dosyasına eklenecek çıkışına neden belirtir.  `--dbpath` Seçeneği veri dizininin konumunu belirtir. Daha fazla hizmeti ile ilgili komut satırı seçenekleri için bkz [hizmeti ile ilgili komut satırı seçenekleri][MongoWindowsSvcOptions].

    Hizmeti başlatmak için şu komutu çalıştırın:

        C:\> net start MongoDB
10. MongoDB yüklü ve çalışan, MongoDB için uzaktan bağlanabilmesi için bir bağlantı noktası Windows Güvenlik Duvarı'nı açmak ihtiyacınız olduğunu.  Gelen **Başlat** menüsünde, select **Yönetimsel Araçlar** ve ardından **Gelişmiş Güvenlik Özellikli Windows Güvenlik Duvarı**.
11. bir) sol bölmede seçin **gelen kuralları**.  İçinde **Eylemler** sağdaki seçin bölmesinde **yeni kural...** .

    ![Windows Güvenlik Duvarı][Image1]

    b) içinde **yeni gelen kuralı Sihirbazı**seçin **bağlantı noktası** ve ardından **sonraki**.

    ![Windows Güvenlik Duvarı][Image2]

    c) select **TCP** ve ardından **belirli yerel bağlantı noktaları**.  ' I tıklatın ve "27017" (MongoDB dinlediği varsayılan bağlantı noktası) bir bağlantı noktası belirtin **sonraki**.

    ![Windows Güvenlik Duvarı][Image3]

    d) select **bağlantıya izin** tıklatıp **sonraki**.

    ![Windows Güvenlik Duvarı][Image4]

    e) tıklatın **sonraki** yeniden.

    ![Windows Güvenlik Duvarı][Image5]

    f) "MongoPort" gibi kural için bir ad belirtin ve tıklatın **son**.

    ![Windows Güvenlik Duvarı][Image6]

12. Sanal makineyi oluşturduğunuzda MongoDB için bir uç nokta yapılandırmadıysanız, bunu şimdi yapabilirsiniz. Güvenlik duvarı kuralı ve uç noktası için MongoDB uzaktan bağlanabilmesi gerekir.

  Azure portalında tıklatın **sanal makineleri (Klasik)**, yeni bir sanal makine adına tıklayın ve ardından **uç noktaları**.

    ![Uç Noktalar][Image7]

13. **Ekle**'ye tıklayın.

14. Protokol "Mongo" adında bir uç nokta ekleyin **TCP**, her ikisi **ortak** ve **özel** bağlantı noktası "27017" ayarlayın. Bu bağlantı noktası açmak, MongoDB uzaktan erişilmesine izin verir.

    ![Uç Noktalar][Image9]

> [!NOTE]
> Bağlantı noktası 27017 MongoDB tarafından kullanılan varsayılan bağlantı noktasıdır. Bu varsayılan bağlantı noktası belirterek değiştirebileceğiniz `--port` mongod.exe sunucunun başlatırken parametresi. Güvenlik Duvarı'nda aynı bağlantı noktası numarası ve yukarıdaki yönergeleri "Mongo" uç verdiğinizden emin olun.
>
>

[MongoDownloads]: http://www.mongodb.org/downloads

[MongoWindowsSvcOptions]: http://www.mongodb.org/display/DOCS/Windows+Service


[Image1]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall1.png
[Image2]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall2.png
[Image3]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall3.png
[Image4]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall4.png
[Image5]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall5.png
[Image6]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall6.png
[Image7]: ./media/install-and-run-mongo-on-win2k8-vm/menusendpointadd.png
<!-- Removed 03/08/2017. Not in new portal. -->
<!-- [Image8]: ./media/install-and-run-mongo-on-win2k8-vm/WinVmAddEndpoint2.png
-->
[Image9]: ./media/install-and-run-mongo-on-win2k8-vm/newendpointdetails.png
