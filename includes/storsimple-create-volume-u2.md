<!--author=SharS last changed: 02/29/2016-->

#### Birim oluşturmak için

1. Cihaz **Hızlı Başlangıç** sayfasında, **Birim ekle**’ye tıklayın. Bu, Birim ekleme sihirbazını başlatır.

2. Birim ekleme sihirbazının **Temel Ayarlar**’ı altında:

    4. Biriminiz için bir **Ad** yazın.
    5. Açılan listede biriminiz için **Kullanım Türü**’nü seçin. Yerel garantilerin, düşük gecikme sürelerinin ve yüksek performansın gerektiği iş yükleri için **Yerel olarak sabitlenmiş** bir birim seçin. Diğer tüm veriler için **Katmanlı** bir birim seçin. Bu birimi arşiv verileri için kullanıyorsanız **Bu birimi daha az sıklıkta erişilen arşiv verileri için kullanın**’ı işaretleyin. 
    
        Yerel olarak sabitlenmiş bir birim sıkı hazırlanmıştır ve birimdeki birincil verilerin cihazda yerel kalmasını ve buluta dağıtılmamasını sağlar.  Yerel olarak sabitlenmiş bir birim oluşturursanız, istenen boyutta birimi hazırlamak için cihaz yerel katmanlarda kullanılabilir alanı denetler. Yerel olarak sabitlenmiş birim oluşturma işlemi var olan verileri cihazdan buluta dağılmasını kapsayabilir ve birim oluşturmak için geçen süre uzun olabilir. Toplam süre hazırlanan birimin boyutuna, kullanılabilir ağ bant genişliğine ve cihazınızdaki verilere bağlıdır. 

        Katmanlı birim sıkı hazırlanmıştır ve çok hızlı oluşturulabilir. Arşiv verileri için katmanlı birim kullanıyorsanız, **Bu birimi daha az sıklıkta erişilen arşiv verileri için kullanın**’ın seçilmesi, biriminizle ilgili yinelenenleri kaldırma öbek boyutunu 512 KB olarak değiştirir. Bu alan işaretli değilse, ilgili katmanlı birim 64 KB öbek boyutu kullanır. Daha büyük bir yinelenenleri kaldırma öbek boyutu cihazın büyük arşiv verilerinin buluta aktarımını hızlandırmasını sağlar.

    3. Biriminiz için **Sağlanan Kapasite**’yi belirtin. Seçili birim türünü temel alan kullanılabilir kapasiteyi not edin. Belirtilen birim boyutu kullanılabilir alanı aşmamalıdır.

        8100 cihazda yerel olarak sabitlenmiş birimleri 8 TB'ye kadar, katmanlı birimleri de 200 TB'ye kadar hazırlayabilirsiniz. Daha büyük olan 8600 cihazında yerel olarak sabitlenmiş birimleri 20 TB'ye kadar, katmanlı birimleri de 500 TB'ye kadar hazırlayabilirsiniz. Katmanlı birimlerin çalışma kümesini barındırmak için cihazda yerel alan gerektiğinden, yerel olarak sabitlenmiş birimlerin oluşturması katmanlı birimlerin sağlanması için uygun boş alanları etkiler. Bu nedenle, yerel olarak sabitlenmiş bir birim oluşturursanız, katmanlı birimlerin oluşturulması için gereken boş alan azalır. Benzer şekilde, katmanlı birim oluşturulduysa, yerel olarak sabitlenmiş birimlerin oluşturulması için kullanılabilir alan azalır. 

        8100 cihazınızda 8 TB (izin verilen en yüksek boyut) yerel olarak sabitlenmiş bir birim hazırlıyorsanız, cihazdaki uygun yerel alanın tümünü boşaltırsınız. Katmanlı birimin çalışan kümesinin barındıracak cihazda yerel alan olmadığından bu noktadan sonra herhangi bir katmanlı birim oluşturamazsınız. Var olan katmanlı birimler kullanılabilir alanı de etkiler. Örneğin, zaten 100 TB katmanlı birimlerin bulunduğu 8100 cihazınız varsa, alanın yalnızca 4 TB’lik bölümü yerel olarak sabitlenmiş birimler için uygundur.

        Aşağıdaki resimde, yerel olarak sabitlenmiş birimin **Temel Ayarlar** iletişim kutusu gösterilmektedir.

         ![Yerel birim ekleme](./media/storsimple-create-volume-u2/add-local-volume-include.png)

        Aşağıdaki resimde, katmanlı birimin **Temel Ayarlar** iletişim kutusu gösterilmektedir.

         ![Yerel birim ekleme](./media/storsimple-create-volume-u2/add-tiered-volume-include.png)

   4. Ok simgesine ![ok simgesi](./media/storsimple-create-volume-u2/HCS_ArrowIcon-include.png) tıklayarak sonraki sayfaya gidin.


3. **Ek Ayarlar** iletişim kutusuna yeni bir erişim denetimi kaydı (ACR) ekleyin:

    1. ACR’nize bir **Ad** verin.
    2. **iSCSI Başlatıcısı Adı** altında, Windows konağınızın iSCSI Tam Adını (IQN) girin. IQN’niz yoksa [Windows Server konağının IQN’sini al](#get-the-iqn-of-a-windows-server-host)’a gidin.
    3. **Bu birim için varsayılan yedekleme etkinleştirilsin mi?** altında **Etkinleştir** onay kutusunu seçin. Varsayılan yedek her gün 22: 30'da (aygıt saat) yürütülen bir ilkenin yanı sıra bu birimin bir bulut anlık görüntüsünü de oluşturur.
     
     > [AZURE.NOTE] Yedek burada etkinleştirildikten sonra geri alınamaz. Bu ayarı değiştirmek için birimi değiştirmeniz gerekir.

     ![Birim ekle](./media/storsimple-create-volume-u2/AddVolumeAdditionalSettings1.png)

4. Onay simgesine tıklayın ![onay simgesi](./media/storsimple-create-volume-u2/HCS_CheckIcon-include.png). Belirtilen ayarlarla bir birim oluşturulacaktır.





<!--HONumber=Jun16_HO2-->


