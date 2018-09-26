Yüklemek ve Windows Server çalıştıran bir sanal makinede MongoDB çalıştırmak için aşağıdaki adımları izleyin.

> [!IMPORTANT]
> Kimlik doğrulaması ve IP adresi bağlama gibi MongoDB güvenlik özellikleri varsayılan olarak etkin değildir. Güvenlik özellikleri, MongoDB, bir üretim ortamına dağıtmadan önce etkinleştirilmesi gerekir.  Daha fazla bilgi için [güvenlik ve kimlik doğrulama](http://www.mongodb.org/display/DOCS/Security+and+Authentication).
>
>

1. Uzak Masaüstü kullanarak sanal makineye bağlandıktan sonra Internet Explorer'da açın **Başlat** sanal makine menüsünde.
2. Seçin **Araçları** sağ üst köşedeki düğmesi.  İçinde **Internet Seçenekleri**seçin **güvenlik** sekmesine tıklayın ve ardından **Güvenilen siteler** simgesini ve son olarak tıklayın **siteleri** düğmesi. Ekleme *https://\*. mongodb.org* Güvenilen siteler listesine.
3. Git [indirmeler - MongoDB](https://www.mongodb.com/download-center#community).
4. Bulma **son kararlı sürüm** , **Community Server**, en son seçin **64-bit** Windows sütunda sürümü. İndir ve sonra MSI yükleyicisini çalıştırın.
5. MongoDB, genellikle C:\Program Files\MongoDB yüklenir. Masaüstünde ortam değişkenleri için arama yapın ve MongoDB ikili dosyaların yolunu PATH değişkenine ekleyin. Örneğin, ikili dosyaları makinenizde C:\Program Files\MongoDB\Server\3.4\bin bulabilirsiniz.
6. Veri diski, MongoDB veri ve günlük dizini oluşturmak (sürücü gibi **F:**) önceki adımlarda oluşturduğunuz. Gelen **Başlat**seçin **komut istemi** bir komut istemi penceresi açın.  Şunu yazın:

        C:\> F:
        F:\> mkdir \MongoData
        F:\> mkdir \MongoLogs
7. Veritabanı'nı çalıştırmak için aşağıdakini çalıştırın:

        F:\> C:
        C:\> mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log

    Tüm günlük iletilerini yönlendirilmiş *F:\MongoLogs\mongolog.log* mongod.exe sunucusu başlatır ve günlük dosyalarını preallocates dosya. Bu günlük dosyaları erişinceye ve bağlantıları dinlemeyi başlatmak MongoDB için birkaç dakika sürebilir. MongoDB örneğinizin çalışırken komut isteminde bu görevde odaklanmış kalır.
8. MongoDB Yönetim Kabuğu'nu başlatmak için başka bir komut penceresinden açın **Başlat** ve aşağıdaki komutları yazın:

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

    Veritabanı ekleme tarafından oluşturulur.
9. Alternatif olarak, hizmet olarak mongod.exe yükleyebilirsiniz:

        C:\> mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log --logappend  --install

    Bir hizmeti yüklü "Mongo DB" açıklaması ile MongoDB adlı. `--logpath` Çalışan hizmetin çıkışını görüntülemek için bir komut penceresi sahip olmadığından, bir günlük dosyası belirtmek için seçeneği kullanılmalıdır.  `--logappend` Seçeneği, hizmetin yeniden başlatılması mevcut günlük dosyasına eklenecek çıkış neden olduğunu belirtir.  `--dbpath` Seçeneği veri dizininin konumunu belirtir. Hizmetle ilgili daha fazla komut satırı seçenekleri için bkz [hizmetle ilgili komut satırı seçenekleri][MongoWindowsSvcOptions].

    Hizmeti başlatmak için şu komutu çalıştırın:

        C:\> net start MongoDB
10. Bu nedenle MongoDB yüklenir ve Windows Güvenlik Duvarı'nda bir bağlantı noktası açmanıza gerek çalıştıran, MongoDB için uzaktan bağlanabilirsiniz.  Gelen **Başlat** menüsünde **Yönetimsel Araçlar** ardından **Gelişmiş Güvenlik Özellikli Windows Güvenlik Duvarı**.
11. bir) sol bölmede seçin **gelen kuralları**.  İçinde **eylemleri** bölmesinde sağ Select **yeni kuralı...** .

    ![Windows Güvenlik Duvarı][Image1]

    (b) içinde **yeni gelen kuralı Sihirbazı**seçin **bağlantı noktası** ve ardından **sonraki**.

    ![Windows Güvenlik Duvarı][Image2]

    c) select **TCP** ardından **belirli yerel bağlantı noktaları**.  ' A tıklayın ve "27017" (MongoDB dinlediği varsayılan bağlantı noktası) bağlantı noktasını belirtin **sonraki**.

    ![Windows Güvenlik Duvarı][Image3]

    d) select **bağlantıya izin** tıklatıp **sonraki**.

    ![Windows Güvenlik Duvarı][Image4]

    e) tıklayın **sonraki** yeniden.

    ![Windows Güvenlik Duvarı][Image5]

    f) "MongoPort" gibi bir kuralı için bir ad belirtin ve tıklayın **son**.

    ![Windows Güvenlik Duvarı][Image6]

12. Sanal makine oluşturduğunuzda bir uç nokta için MongoDB yapılandırmadıysanız, bunu şimdi yapabilirsiniz. Güvenlik duvarı kuralı ve Uzaktan Mongodb'ye bağlanmak uç hem de gerekir.

  Azure portalında **sanal makineler (Klasik)** yeni bir sanal makine adına tıklayın ve ardından **uç noktaları**.

    ![Uç Noktalar][Image7]

13. **Ekle**'ye tıklayın.

14. Protokol "Mongo" adlı bir uç nokta ekleyin **TCP**, her ikisi **genel** ve **özel** "27017" için bağlantı noktası ayarlayın. Bu bağlantı noktası açma MongoDB uzaktan erişilmesine izin verir.

    ![Uç Noktalar][Image9]

> [!NOTE]
> Bağlantı noktası 27017 MongoDB tarafından kullanılan varsayılan bağlantı noktasıdır. Bu varsayılan bağlantı noktası belirterek değiştirebileceğiniz `--port` mongod.exe sunucuyu başlatma sırasında parametre. Güvenlik duvarında aynı bağlantı noktası numarası ve "Mongo" uç noktası önceki yönergelerde yer verdiğinizden emin olun.
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
