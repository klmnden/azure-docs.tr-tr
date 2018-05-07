<!--author=alkohli last changed: 9/17/15-->

#### <a name="to-add-a-storage-account-in-storsimple-8000-series-update-10"></a>StorSimple 8000 Series Update 1.0 sürümünde depolama hesabı eklemek için
1. StorSimple Yöneticisi hizmet giriş sayfasında hizmetinizi seçip çift tıklayın. Böylece **Hızlı Başlangıç** sayfasına gideceksiniz. **Yapılandır** sayfasını seçin.
2. **Depolama hesabı ekleyin/düzenleyin**’e tıklayın.
3. **Depolama Hesabı Ekle/Düzenle** iletişim kutusunda **Yeni ekle**’ye tıklayın.
4. **Sağlayıcı** alanında, uygun bulut hizmeti sağlayıcısını seçin. Desteklenen sağlayıcılar Azure, Amazon S3, Amazon S3 - RRS, HP ve OpenStack’tir. Bulut hizmeti sağlayıcılarınızın depolama hesabıyla ilişkili konumu ve kimlik bilgilerini belirtin. Kimlik bilgileri için sunulan alanlar, belirttiğiniz bulut hizmeti sağlayıcısına bağlı olarak farklı olacaktır. 
   
   * Bulut hizmeti sağlayıcısı olarak Azure’u seçtiyseniz, Microsoft Azure Storage hesabınız için **Ad** ve birincil **Erişim Tuşu**’nu verin. Azure hesabı için konum otomatik olarak doldurulur.
     
        ![Azure depolama hesabı ekleme](./media/storsimple-configure-new-storage-account-u1/AddAzureStorageaccount-include.png)
   * Amazon S3 veya Amazon S3-RRS seçtiyseniz, kullanımı kolay bir **Depolama Hesabı adı**, **Erişim Tuşu** ve **Gizli Anahtar** verin. Amazon S3 ve Amazon S3-RRS için şu konumlar desteklenir:
     
     * ABD Standart
     * ABD Batı (Oregon)
     * ABD Batı (Kuzey California)
     * AB (İrlanda)
     * Asya Pasifik (Singapur)
     * Asya Pasifik (Sidney)
     * Asya Pasifik (Tokyo)
     * Güney Amerika (Sao Paulo)
       
       ![Amazon depolama hesabı ekleme](./media/storsimple-configure-new-storage-account-u1/AddAmazonStorageaccount-include.png)
   * Bulut hizmeti sağlayıcısı olarak HP seçtiyseniz, kullanımı kolay bir **Depolama Hesabı adı**, **Kiracı Kimliği**, **Kullanıcı Adı** ve **Parola** verin. HP için aşağıdaki konumlar desteklenir:
     
     * ABD Doğu
     * ABD Batı
       
       ![HP depolama hesabı ekleme](./media/storsimple-configure-new-storage-account-u1/AddHPStorageaccount-include.png)
   * Bulut hizmet sağlayıcısı olarak **Openstack** seçtiyseniz bir **Konak Adı**, **Erişim Tuşu** ve **Gizli Anahtar** verin.
     
     > [!NOTE]
     > Azure dışında tüm bulut hizmeti sağlayıcıları için kullanımı kolay bir ada izin verilir. Kullanımı kolay farklı adlar kullanabilir ve aynı kimlik bilgisi kümesiyle birden fazla depolama hesabı oluşturabilirsiniz.
     > 
     > 
     
        ![Openstack depolama hesabı ekleme](./media/storsimple-configure-new-storage-account-u1/AddOpenstackStorageaccount-include.png)
5. Cihazınız ve bulut arasındaki ağ iletişimi için güvenli bir kanal oluşturmak için **SSL Modunu Etkinleştir**’i seçin. Yalnızca özel bir bulutta işlem yapıyorsanız**SSL Modunu Etkinleştir** onay kutusunu temizleyin.
   
   > [!NOTE]
   > Sağlayıcınız olarak HP kullanıyorsanız SSL her zaman etkin olacaktır.
   > 
   > 
6. Onay simgesine tıklayın ![onay simgesi](./media/storsimple-configure-new-storage-account/HCS_CheckIcon-include.png). Depolama hesabı sorunsuz oluşturulduktan sonra size bildirilecek.
7. Yeni oluşturulan depolama hesabı **Depolama hesapları** altında, **Yapılandır** sayfasında görüntülenir. Yeni depolama hesabını kaydetmek için **Kaydet**’e tıklayın. Onayınız istendiğinde **Tamam**’a tıklayın.

