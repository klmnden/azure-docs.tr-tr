<!--author=alkohli last changed: 08/16/2016-->

#### <a name="to-create-a-volume"></a>Birim oluşturmak için
1. Cihaz **Hızlı Başlangıç** sayfasında **Birim ekle**’ye tıklayarak Birim ekleme sihirbazını başlatın.
2. Birim ekleme sihirbazının **Temel Ayarlar**’ı altında:
   
   1. Biriminiz için bir **Ad** yazın.
   2. Açılan listede biriminiz için **Kullanım Türü**’nü seçin. Yerel garantilerin, düşük gecikme sürelerinin ve yüksek performansın gerektiği iş yükleri için **Yerel olarak sabitlenmiş** bir birim seçin. Diğer tüm veriler için **Katmanlı** bir birim seçin. Bu birimi arşiv verileri için kullanıyorsanız **Bu birimi daha az sıklıkta erişilen arşiv verileri için kullanın**’ı işaretleyin. 
      
       Yerel olarak sabitlenmiş bir birim sıkı hazırlanmıştır ve birimdeki birincil verilerin cihazda yerel kalmasını ve buluta dağıtılmamasını sağlar.  Yerel olarak sabitlenmiş bir birim oluşturursanız cihaz, istenen boyutta birimi sağlamak için yerel katmanlardaki kullanılabilir alanı denetler. Yerel olarak sabitlenmiş birim oluşturma işlemi var olan verileri cihazdan buluta dağılmasını kapsayabilir ve birim oluşturmak için geçen süre uzun olabilir. Toplam süre hazırlanan birimin boyutuna, kullanılabilir ağ bant genişliğine ve cihazınızdaki verilere bağlıdır. 
      
       Katmanlı birim ölçülü sayıda kaynak kullanarak sağlanır ve hızlı oluşturulabilir. Arşiv verileri için hedeflenen katmanlı birime yönelik olarak **Bu birimi daha az sıklıkta erişilen arşiv verileri için kullan**’ın seçilmesi, biriminizle ilgili yinelenenleri kaldırma öbek boyutunu 512 KB olarak değiştirir. Bu alan işaretli değilse, ilgili katmanlı birim 64 KB öbek boyutu kullanır. Daha büyük bir yinelenenleri kaldırma öbek boyutu cihazın büyük arşiv verilerinin buluta aktarımını hızlandırmasını sağlar.
   3. Biriminiz için **Sağlanan Kapasite**’yi belirtin. Seçili birim türünü temel alan kullanılabilir kapasiteyi not edin. Belirtilen birim boyutu kullanılabilir alanı aşmamalıdır.
      
       8100 cihazında 8,5 TB'a kadar yerel olarak sabitlenmiş birim ya da 200 TB'a kadar katmanlı birim sağlayabilirsiniz. Daha büyük olan 8600 cihazında 22,5 TB'a kadar yerel olarak sabitlenmiş birim ya da 500 TB'a kadar katmanlı birim sağlayabilirsiniz. Katmanlı birimlerin çalışma kümesini barındırmak için cihazda yerel alan gerektiğinden, yerel olarak sabitlenmiş birimlerin oluşturulması, katmanlı birimlerin sağlanması için kullanılabilecek alanı etkiler. Bu nedenle, yerel olarak sabitlenmiş bir birim oluşturursanız, katmanlı birimlerin oluşturulması için gereken boş alan azalır. Benzer şekilde, katmanlı birim oluşturulduysa, yerel olarak sabitlenmiş birimlerin oluşturulması için kullanılabilir alan azalır.
      
       8100 cihazınızda 8,5 TB boyutunda (izin verilen en yüksek boyut) yerel olarak sabitlenmiş bir birim sağlarsanız, cihazdaki kullanılabilir yerel alanın tümünü kullanmış olursunuz. Katmanlı birimin çalışan kümesinin barındıracak cihazda yerel alan olmadığından bu noktadan sonra herhangi bir katmanlı birim oluşturamazsınız. Var olan katmanlı birimler kullanılabilir alanı de etkiler. Örneğin, zaten 106 TB boyutunda katmanlı birimlerin bulunduğu bir 8100 cihazınız varsa, yerel olarak sabitlenmiş birimlerin kullanabileceği yalnızca 4 TB’lık alan kalır.
      
       Aşağıdaki resimde, yerel olarak sabitlenmiş birimin **Temel Ayarlar** iletişim kutusu gösterilmektedir.
      
        ![Yerel birim ekleme](./media/storsimple-create-volume-u2/add-local-volume-include.png)
      
       Aşağıdaki resimde, katmanlı birimin **Temel Ayarlar** iletişim kutusu gösterilmektedir.
      
        ![Yerel birim ekleme](./media/storsimple-create-volume-u2/add-tiered-volume-include.png)
   
   1. Ok simgesine ![ok simgesi](./media/storsimple-create-volume-u2/HCS_ArrowIcon-include.png) tıklayarak sonraki sayfaya gidin.
3. **Ek Ayarlar** iletişim kutusuna yeni bir erişim denetimi kaydı (ACR) ekleyin:
   
   1. ACR’nize bir **Ad** verin.
   2. **iSCSI Başlatıcısı Adı** altında, Windows konağınızın iSCSI Tam Adını (IQN) girin. IQN’niz yoksa [Windows Server konağının IQN’sini al](#get-the-iqn-of-a-windows-server-host)’a gidin.
   3. **Bu birim için varsayılan yedekleme etkinleştirilsin mi?** altında **Etkinleştir** onay kutusunu seçin. Varsayılan yedek, her gün saat 22.30'da (cihazın saati) yürütülen bir ilke oluşturarak bu birimin bir bulut anlık görüntüsünü oluşturur.
      
      > [!NOTE]
      > Yedek burada etkinleştirildikten sonra geri alınamaz. Bu ayarı değiştirmek için birimi düzenlemeniz gerekir.
      > 
      > 
      
      ![Birim ekle](./media/storsimple-create-volume-u2/AddVolumeAdditionalSettings1.png)
4. Onay simgesine tıklayarak ![onay simgesi](./media/storsimple-create-volume-u2/HCS_CheckIcon-include.png). Belirtilen ayarlarla bir birim oluşturulur.



<!--HONumber=Nov16_HO2-->


