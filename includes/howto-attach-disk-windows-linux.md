


## <a name="attach-an-empty-disk"></a>Boş disk ekleme
Boş disk ekleme, Azure .vhd dosyasını oluşturur ve depolama hesabında depoluyor çünkü bir veri diski eklemek için basit bir yoludur.

1. Tıklayın **sanal makineler (Klasik)** ve ardından uygun sanal Makineyi seçin.

2. Ayarlar menüsünde tıklatın **diskleri**.

   ![Yeni bir boş diski kullanıma açın](./media/howto-attach-disk-windows-linux/menudisksattachnew.png)

3. Komut çubuğunda **iliştirme yeni**.  
    **Yeni disk Attach** iletişim kutusu görüntülenir.

    ![Yeni bir disk ekleme](./media/howto-attach-disk-windows-linux/newdiskdetail.png)

    Aşağıdaki bilgileri doldurun:
    - İçinde **dosya adı**varsayılan adı kabul edin veya başka bir .vhd dosyasının yazın. .Vhd dosya için başka bir ad yazın bile veri diski otomatik olarak oluşturulan bir ad kullanır.
    - Seçin **türü** veri diskinin. Standart diskler, tüm sanal makineleri destekler. Çok sayıda sanal makineyi premium diskleri de destekler.
    - Seçin **boyutu (GB)** veri diskinin.
    - İçin **ana bilgisayar önbelleğe alma**, Yok'u seçin veya salt okunur.
    - Bitirmek için Tamam'a tıklayın.

4. Veri diski oluşturulup eklendikten sonra VM diskleri bölümünde listelenir.

   ![Yeni ve boş veri diski başarıyla eklendi](./media/howto-attach-disk-windows-linux/newdiskemptysuccessful.png)

> [!NOTE]
> Veri diski ekledikten sonra VM'de oturum açma ve kullanılabilir böylece diski başlatmanız gerekir.

## <a name="how-to-attach-an-existing-disk"></a>Nasıl yapılır: var olan bir diski kullanıma açın
Var olan bir diskin eklenmesi için depolama hesabında bir .vhd olmalıdır. Kullanım [Add-AzureVhd](https://msdn.microsoft.com/library/azure/dn495173.aspx) cmdlet'ini .vhd dosyasını depolama hesabına yükleyin. Oluşturulan ve .vhd dosyasını karşıya sonra bir VM'ye ekleyebilirsiniz.

1. Tıklayın **sanal makineler (Klasik)** ve ardından uygun sanal makineyi seçin.

2. Ayarlar menüsünde tıklatın **diskleri**.

3. Komut çubuğunda **iliştirme varolan**.

    ![Veri diski ekleme](./media/howto-attach-disk-windows-linux/menudisksattachexisting.png)

4. Tıklayın **konumu**. Kullanılabilir depolama hesapları görüntülenir. Ardından, uygun bir depolama hesabı listeden seçin.

    ![Disk depolama hesabı sağlayın](./media/howto-attach-disk-windows-linux/existdiskstorageaccounts.png)

5. A **depolama hesabı** disk sürücülerini (VHD) içeren bir veya daha fazla kapsayıcı tutar. Uygun bir kapsayıcı listeden seçin.

    ![Sanal-makineler-windows kapsayıcısı sağlayın](./media/howto-attach-disk-windows-linux/existdiskcontainers.png)

6. **VHD'ler** paneli kapsayıcı içinde tutulan disk sürücüleri listeler. Disklerden birini tıklatın ve Seç'e tıklayın.

    ![Sanal makineler-windows için disk görüntüsü sağlayın](./media/howto-attach-disk-windows-linux/existdiskvhds.png)

7. **Mevcut diski** panelini görüntüler yeniden, depolama hesabı, kapsayıcı ve seçili sanal makineye eklemek için sabit diske (vhd) bulunduğu konum.

  Ayarlama **ana bilgisayar önbelleğe alma** yok veya okuma yalnızca, sonra Tamam'ı tıklatın.

    ![Veri diski başarıyla eklendi](./media/howto-attach-disk-windows-linux/exisitingdisksuccessful.png)
