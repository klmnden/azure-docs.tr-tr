<!--author=alkohli last changed: 08/04/17-->

#### <a name="to-install-an-update-from-the-azure-portal"></a>Azure portalından bir güncelleştirmeyi yüklemek için

1. StorSimple hizmet sayfasında cihazınızı seçin.

    ![Cihazı seçin](./media/storsimple-8000-install-update5-via-portal/update1.png)

2. Gidin **aygıt ayarları** > **aygıt güncelleştirmeleri**.

    ![Cihaz güncelleştirmeleri tıklatın](./media/storsimple-8000-install-update5-via-portal/update2.png)

2. Yeni güncelleştirmeler varsa bir bildirim görüntülenir. Alternatif olarak, içinde **aygıt güncelleştirmeleri** dikey penceresinde tıklatın **Güncelleştirmeleri tara**. Kullanılabilir güncelleştirmeleri taramak için bir iş oluşturulur. İş başarıyla tamamlandığında size bildirilir.

    ![Cihaz güncelleştirmeleri tıklatın](./media/storsimple-8000-install-update5-via-portal/update3.png)

3. Bir güncelleştirmeyi cihazınıza uygulamadan önce sürüm notlarını gözden geçirmeniz önerilir. Güncelleştirmeleri uygulamak için tıklatın **güncelleştirmelerini yükleme**. İçinde **düzenli olarak güncelleştirmeleri Onayla** dikey penceresinde, güncelleştirmeleri uygulamadan önce tamamlamak için önkoşulları gözden geçirin. Cihaz güncelleştirin ve ardından hazır olduğunu belirtmek için onay kutusunu işaretleyin **yükleme**.

    ![Cihaz güncelleştirmeleri tıklatın](./media/storsimple-8000-install-update5-via-portal/update4.png)

6. Bir dizi önkoşul denetimi başlatılır. Bu denetimler şunlardır:
   
   * Her iki cihaz denetleyicisinin de sağlıklı ve çevrimiçi olduğunu doğrulamaya yönelik **denetleyici durumu denetimleri**.
   * StorSimple cihazınızdaki tüm donanım bileşenlerinin sağlıklı olduğunu doğrulamaya yönelik **donanım bileşeni durum denetimleri**.
   * DATA 0’ın cihazınızda etkin olduğunu doğrulamaya yönelik **DATA 0 denetimleri**. Bu arabirim etkin değilse etkinleştirmeniz ve sonra yeniden denemeniz gerekir.

    Güncelleştirme karşıdan ve yüklü değilse yalnızca tüm denetimlerden başarıyla tamamlanır. Denetimler devam ederken size bildirilir. Ardından prechecks başarısız olursa hatanın nedenlerini ile sağlanacaktır. Bu sorunları giderin ve işlemi yeniden deneyin. Bu sorunları kendi başınıza çözemiyorsanız Microsoft Desteği’ne başvurmanız gerekebilir.

7. Prechecks başarıyla tamamlandıktan sonra bir güncelleştirme işi oluşturulur. Güncelleştirme işi başarıyla oluşturulduğunda size bildirilir.
   
    ![Güncelleştirme işi oluşturma](./media/storsimple-8000-install-update5-via-portal/update6.png)
   
    Bundan sonra güncelleştirme, cihazınıza uygulanır.

9. Güncelleştirmenin tamamlanması birkaç saat sürer. Güncelleştirme işini seçin ve **Ayrıntılar**’a tıklayarak dilediğiniz zaman işin ayrıntılarını görüntüleyin.

    ![Güncelleştirme işi oluşturma](./media/storsimple-8000-install-update5-via-portal/update8.png)

     Ayrıca güncelleştirme işten ilerlemesini izleyebilirsiniz **Aygıt Ayarları > işleri**. Üzerinde **işleri** dikey penceresinde, güncelleştirme ilerleme durumunu görebilirsiniz.

     ![Güncelleştirme işi oluşturma](./media/storsimple-8000-install-update5-via-portal/update7.png)

10. İş tamamlandıktan sonra gidin **Aygıt Ayarları > aygıt güncelleştirmeleri**. Yazılım sürümü şimdi güncelleştirilmesi gerekir.

