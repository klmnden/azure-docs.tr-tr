<!--author=alkohli last changed:02/22/16-->

#### <a name="to-attach-the-sas-cables"></a>SAS kabloları eklemek için
1. Birincil ve ebod tanımlayın. İki kasaları kendi ilgili geri düzlemleri bakarak tanımlanabilir. Aşağıdaki resim yönergeler için bkz. 
   
    ![Düzlemi birincil ve ebod](./media/storsimple-sas-cable-8600/HCSBackplaneofprimaryandEBODenclosure.png)
   
    **Birincil ve EBOD kutularının görünümünü yedekleyin**
   
   | Etiket | Açıklama |
   |:--- |:--- |
   | 1 |Birincil kasası |
   | 2 |EBOD muhafazası |
2. Birincil ve EBOD kutularının seri numaralarını bulun. Seri numarası etiketinin her kasa için geri kulak yapıştırılmış. Seri numaraları üzerinde hem kasaları aynı olmalıdır. [Microsoft Support başvurun](../articles/storsimple/storsimple-contact-microsoft-support.md) hemen seri numaralarını eşleşmiyorsa. Seri numaralarını bulmak için aşağıdaki çizime bakın.
   
    ![Seri numarası etiketinin yerini belirten arka görünümü](./media/storsimple-sas-cable-8600/HCSRearviewofenclosureindicatinglocationofserialnumbersticker.png)
   
    **Seri numarası etiketinin konumu**
   
   | Etiket | Açıklama |
   |:--- |:--- |
   | 1 |Muhafaza kulak |
3. EBOD muhafazası birincil muhafaza şu şekilde bağlanmak için sağlanan SAS kabloları kullanın:
   
   1. Birincil muhafaza ve EBOD muhafazası dört SAS bağlantı noktalarını belirleyin. SAS bağlantı noktasını birincil muhafaza EBOD etiketlenir ve bağlantı noktası a EBOD muhafazası üzerinde aşağıdaki çizimde, kablolama SAS gösterildiği gibi karşılık gelir.
   2. EBOD bağlantı noktası için bağlantı noktası A. bağlanmak için sağlanan SAS kabloları kullanın
   3. EBOD bağlantı noktası 0 denetleyicisinde EBOD denetleyici 0 bağlantı noktası A bağlanması gerekir. EBOD Denetleyici 1 üzerinde bağlantı noktası A'ya EBOD bağlantı noktası 1 denetleyicisinde bağlı olması gerekir. Aşağıdaki çizim kılavuzu için bkz. 
      
      ![Cihazınız için SAS kabloları](./media/storsimple-sas-cable-8600/HCSSAScablingforyourdevice.png)
      
      **SAS kabloları**
      
      | Etiket | Açıklama |
      |:--- |:--- |
      | A |Birincil kasası |
      | B |EBOD muhafazası |
      | 1 |Denetleyici 0 |
      | 2 |Denetleyici 1 |
      | 3 |EBOD denetleyici 0 |
      | 4 |EBOD Denetleyici 1 |
      | 5, 6 |Birincil kasası (etiketli EBOD) SAS bağlantı noktaları |
      | 7, 8 |EBOD muhafazası (bağlantı noktası A) SAS bağlantı noktaları |

