<!--author=SharS last changed: 02/04/2016-->

#### Birim oluşturmak için

1. Cihaz **Hızlı Başlangıç** sayfasında, **Birim ekle**’ye tıklayın. Bu, Birim ekleme sihirbazını başlatır.

2. Birim ekleme sihirbazının **Temel Ayarlar**’ı altında şunları yapın:
   1. Biriminize bir **Ad** verin.
   2. Biriminiz için GB veya TB cinsinden **Sağlanan Kapasite**’yi belirtin. Birim kapasitesi, fiziksel aygıt için 1 GB ile 64 TB arasında olmalıdır.
   3. Açılan listede biriminiz için **Kullanım Türü**’nü seçin. 
   4. Bu birimi arşiv verileri için kullanıyorsanız **Bu birimi daha az sıklıkta erişilen arşiv verileri için kullanın** onay kutusunu seçin. Diğer tüm kullanım durumları için **Katmanlı Birim**’i seçmeniz yeterlidir. (Katmanlı birimlere daha önce birincil birimler adı veriliyordu).

        ![Birim ekle](./media/storsimple-create-volume/ScreenshotUpdate1VolumeFlow.png)

    4. Ok simgesine tıklama ![ok simgesi](./media/storsimple-create-volume/HCS_ArrowIcon-include.png) tıklayarak sonraki sayfaya gidin.

3. **Ek Ayarlar** iletişim kutusuna yeni bir erişim denetimi kaydı (ACR) ekleyin:
   1. ACR’nize bir **Ad** verin.
   2. **iSCSI Başlatıcısı Adı** altında, Windows konağınızın iSCSI Tam Adını (IQN) girin. IQN’niz yoksa [Windows Server konağının IQN’sini al](#get-the-iqn-of-a-windows-server-host)’a gidin.
   3. **Bu birim için varsayılan yedeklemeyi etkinleştir** onay kutusunu seçerek varsayılan yedeği etkinleştirmenizi öneririz. Varsayılan yedek her gün 22: 30'da (aygıt saat) yürütülen bir ilkenin yanı sıra bu birimin bir bulut anlık görüntüsünü de oluşturur.

        > [AZURE.NOTE] Yedek burada etkinleştirildikten sonra geri alınamaz. Bu ayarı değiştirmek için birimi değiştirmeniz gerekir.

        ![Birim ekle](./media/storsimple-create-volume/AddVolume2-include.png)

4. Onay simgesine tıklayın ![onay simgesi](./media/storsimple-create-volume/HCS_CheckIcon-include.png). Belirtilen ayarlarla bir birim oluşturulacaktır.

![Kullanılabilir video](./media/storsimple-create-volume/Video_icon.png) **Kullanılabilir video**

Bir StorSimple biriminin nasıl oluşturulduğunu gösteren bir video izlemek için [buraya](https://azure.microsoft.com/documentation/videos/create-a-storsimple-volume/) tıklayın.




<!--HONumber=Jun16_HO2-->


