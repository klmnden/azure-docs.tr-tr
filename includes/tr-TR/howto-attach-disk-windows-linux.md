


## <a name="attach-an-empty-disk"></a>Boş disk ekleme
Boş bir diski ekleme, çünkü Azure .vhd dosyası sizin için oluşturur ve depolama hesabında depolayan bir veri diski eklemek için basit bir yoludur.

1. Tıklatın **sanal makineleri (Klasik)**ve ardından uygun VM seçin.

2. Ayarlar menüsünde tıklatın **diskleri**.

   ![Yeni bir boş diski kullanıma açın](./media/howto-attach-disk-windows-linux/menudisksattachnew.png)

3. Komut çubuğunda **Attach yeni**.  
    **Attach yeni disk** iletişim kutusu görüntülenir.

    ![Yeni bir diski kullanıma açın](./media/howto-attach-disk-windows-linux/newdiskdetail.png)

    Aşağıdaki bilgileri girin:
    - İçinde **dosya adı**, varsayılan adı kabul edin veya başka bir .vhd dosyası için yazın. .Vhd dosyası için başka bir ad yazın olsa bile veri diski otomatik olarak oluşturulan bir ad kullanır.
    - Seçin **türü** veri diski. Tüm sanal makineler standart diskler destekler. Çok sayıda sanal makineler ayrıca premium diskleri destekler.
    - Seçin **boyutu (GB)** veri diski.
    - İçin **ana bilgisayar önbelleğe alma**, hiçbiri seçin veya salt okunur.
    - Bitirmek için Tamam'ı tıklatın.

4. Veri diski oluşturduktan ve bağlı sonra VM diskleri bölümünde listelenir.

   ![Yeni ve boş veri diski başarıyla eklendi](./media/howto-attach-disk-windows-linux/newdiskemptysuccessful.png)

> [!NOTE]
> Bir veri diski ekledikten sonra VM'de oturum açma ve böylece bu kullanılabilir disk başlatma gerekir.

## <a name="how-to-attach-an-existing-disk"></a>Nasıl yapılır: varolan bir diski kullanıma açın
Var olan bir diskin eklenmesi için depolama hesabında bir .vhd olmalıdır. Kullanım [Ekle AzureVhd](https://msdn.microsoft.com/library/azure/dn495173.aspx) .vhd dosyası depolama hesabına yüklemek için cmdlet'i. Oluşturulan ve .vhd dosyasını karşıya sonra bir VM'ye ekleyebilirsiniz.

1. Tıklatın **sanal makineleri (Klasik)**ve ardından uygun sanal makine seçin.

2. Ayarlar menüsünde tıklatın **diskleri**.

3. Komut çubuğunda **Attach varolan**.

    ![Veri diski ekleme](./media/howto-attach-disk-windows-linux/menudisksattachexisting.png)

4. Tıklatın **konumu**. Kullanılabilir depolama hesaplarını görüntüler. Ardından, uygun depolama hesabı listeden seçin.

    ![Disk depolama hesabı sağlayın](./media/howto-attach-disk-windows-linux/existdiskstorageaccounts.png)

5. A **depolama hesabı** disk sürücülerini (VHD) içeren bir veya daha fazla kapsayıcıları tutar. Uygun bir kapsayıcı listeden seçin.

    ![Makineler windows sanal kapsayıcının sağlayın](./media/howto-attach-disk-windows-linux/existdiskcontainers.png)

6. **VHD'ler** paneli kapsayıcısında tutulan disk sürücüleri listeler. Disklerden birini tıklatın ve Seç'i tıklatın.

    ![Sanal makineler-windows için disk görüntüsü belirtin](./media/howto-attach-disk-windows-linux/existdiskvhds.png)

7. **Varolan bir diski İlişti** paneli görüntüler yeniden, depolama hesabı, kapsayıcı ve seçilen sabit sanal makineye eklemek için disk (vhd) bulunduğu konum.

  Ayarlama **ana bilgisayar önbelleğe alma** yok veya okuma yalnızca, ardından Tamam'ı tıklatın.

    ![Veri diski başarıyla eklendi](./media/howto-attach-disk-windows-linux/exisitingdisksuccessful.png)
