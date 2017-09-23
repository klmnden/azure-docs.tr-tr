<!--author=alkohli last changed: 02/06/17-->

#### <a name="to-install-an-update-from-the-azure-portal"></a>Azure portalından bir güncelleştirmeyi yüklemek için

1. StorSimple hizmet sayfasında cihazınızı seçin. **Cihazlar** > **Bakım**’a gidin.
2. Sayfanın alt kısmındaki **Güncelleştirmeleri Tara**'ya tıklayın. Kullanılabilir güncelleştirmeleri taramak için bir iş oluşturulur. İş başarıyla tamamlandığında size bildirilir.
3. Aynı sayfadaki **Yazılım Güncelleştirmeleri** bölümünde, yeni yazılım güncelleştirmeleri mevcuttur. Bir güncelleştirmeyi cihazınıza uygulamadan önce sürüm notlarını gözden geçirmeniz önerilir.
4. Sayfanın alt kısmındaki **Güncelleştirmeleri Yükle**’ye ve ardından **Tamam**’a tıklayın.
5. **Güncelleştirmeleri yükle** iletişim kutusunda önerileri izlediğinizden emin olun, ardından **Yukarıdaki şartı anlıyorum ve cihazımı yükseltmeye hazırım**’ı seçin ve onay düğmesine tıklayın.
   
    ![Onay iletisi](./media/storsimple-install-update2-via-portal/InstallUpdate12_2M.png)
6. Bir dizi önkoşul denetimi başlatılır. Bu denetimler şunlardır:
   
   * Her iki cihaz denetleyicisinin de sağlıklı ve çevrimiçi olduğunu doğrulamaya yönelik **denetleyici durumu denetimleri**.
   * StorSimple cihazınızdaki tüm donanım bileşenlerinin sağlıklı olduğunu doğrulamaya yönelik **donanım bileşeni durum denetimleri**.
   * DATA 0’ın cihazınızda etkin olduğunu doğrulamaya yönelik **DATA 0 denetimleri**. Bu arabirim etkin değilse etkinleştirmeniz ve sonra yeniden denemeniz gerekir.
   * DATA 2 ve DATA 3 ağ arabirimlerinin etkin olmadığını doğrulamaya yönelik **DATA 2 ve DATA 3 denetimleri**. Bu arabirimler etkinse, bunları devre dışı bıraktıktan sonra cihazınızı güncelleştirmeyi denemeniz gerekir. Bu denetim yalnızca GA yazılımı çalıştıran bir cihazdan güncelleştirme yapıyorsanız gerçekleştirilir. 0.1, 0.2 veya 0.3 sürümlerini çalıştıran cihazlarda bu denetim gerekli değildir.
   * Güncelleştirme 1’den önceki bir sürümü çalıştıran tüm cihazlarda **ağ geçidi denetimi**. Bu denetim, güncelleştirme 1 öncesi yazılımları çalıştıran tüm cihazlarda gerçekleştirilir, ancak DATA 0’dan başka bir ağ arabirimi için yapılandırılmış ağ geçidi bulunan cihazlarda başarısız olur.
     
     Tüm denetimler başarıyla tamamlanırsa güncelleştirme uygulanır. Denetimler devam ederken size bildirilir.
     
     ![Denetim öncesi bildirim](./media/storsimple-install-update2-via-portal/InstallUpdate12_3M.png)
     
     Denetimlerin başarısız olduğu durumların bir örneği aşağıda verilmiştir. Her iki cihaz denetleyicisinin de sağlıklı ve çevrimiçi olduğunu doğrulamanız gerekir. Ayrıca, donanım bileşenlerinin durumunu denetlemeniz gerekir. Bu örnekte, Denetleyici 0 ile Denetleyici 1 bileşenleri dikkat gerektirmektedir. Bu sorunları kendi başınıza çözemiyorsanız Microsoft Desteği’ne başvurmanız gerekebilir.
     
       ![Denetimler başarısız oldu](./media/storsimple-install-update2-via-portal/HCS_PreUpgradeChecksFailed-include.png)
7. Denetimler başarıyla tamamlandıktan sonra bir güncelleştirme işi oluşturulur. Güncelleştirme işi başarıyla oluşturulduğunda size bildirilir.
   
    ![Güncelleştirme işi oluşturma](./media/storsimple-install-update2-via-portal/InstallUpdate12_44M.png)
   
    Bundan sonra güncelleştirme, cihazınıza uygulanır.
    
8. Güncelleştirme işinin ilerleme durumunu izlemek için **İşi Görüntüle**’ye tıklayın. **İşler** sayfasında güncelleştirme ilerleme durumunu görebilirsiniz.
9. Güncelleştirmenin tamamlanması birkaç saat sürer. Güncelleştirme işini seçin ve **Ayrıntılar**’a tıklayarak dilediğiniz zaman işin ayrıntılarını görüntüleyin.
10. İş tamamlandıktan sonra **Bakım** sayfasına gidin ve **Yazılım Güncelleştirmeleri**’ne inin.

