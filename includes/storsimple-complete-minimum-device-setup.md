<!--author=alkohli last changed: 9/17/15-->

#### En düşük StorSimple cihaz kurulumunu tamamlamak için

1. **Cihazlar** sayfasında cihazı seçin, belirli cihaz sayfasına gitmek için cihaz adına bitişik oka tıklayın. 

    ![Çevrimiçi cihazın yer aldığı cihazlar sayfası](./media/storsimple-complete-minimum-device-setup/HCS_DevicesPageM-include.png) 

2. Cihazın hızlı başlangıç sayfasına erişmek için hızlı başlangıç simgesine ![Hızlı Başlangıç simgesi](./media/storsimple-complete-minimum-device-setup/HCS_QuickStartIcon-include.png) tıklayın. **Cihazı yapılandır** sihirbazını başlatmak için **Cihaz kurulumunu tamamla**’ya tıklayın.

    ![Cihaz hızlı başlangıç sayfası](./media/storsimple-complete-minimum-device-setup/Device_Quick_Start_page_1M.png)

2. **Temel Ayarlar** sayfasında şunları yapın:
  1. Cihazınızın **kolay adını** sağlayın. Varsayılan cihaz adı, cihaz modeli ve seri numarası gibi bilgileri yansıtır. Cihazı yönetmek için En fazla 64 karakterlik bir kolay ad atayabilirsiniz.
  2. Cihazın dağıtıldığı coğrafi konum temelinde **saat dilimini** ayarlayın. Cihazınız zamanlanan tüm işlemler için bu saat dilimini kullanır.
  3. **DNS Ayarları** altında **ikincil DNS Sunucusu** için bir adres girin. IPv6 kullanıyorsanız, bu alan Windows PowerShell arabiriminde sağlanan IPv6 önekini temel alarak doldurulacaktır. 
  İkincil DNS sunucusu yapılandırılmadıysa, cihaz yapılandırmanızı kaydetmenize izin verilmez.
  4. iSCSI etkin arabirimlerin altında iSCSI için en az bir ağ etkinleştirin. En az bir ağ arabiriminin bulut etkin olması ve bir arabirimin de iSCSI etkin olması gerekir. DATA 0 otomatik olarak bulut etkindir.
 
      ![StorSimple en düşük cihaz kurulumu temel ayarları](./media/storsimple-complete-minimum-device-setup/HCS_MinDeviceSetupBasicSettings1-include.png)

3. Ok simgesine tıklayın. ![StorSimple ok simgesi](./media/storsimple-complete-minimum-device-setup/HCS_ArrowIcon-include.png)

4. **Ağ Arabirimleri** sayfasında, Denetleyici 0 ve Denetleyici 1 için sabit IP adreslerini verin. DATA 0 arabirimi IPv4 için yapılandırılmışsa sabit IP adreslerinin IPv4 biçiminde verilmesi gerekir. IPv6 yapılandırması için bir önek sağladıysanız bu alanlar otomatik olarak sabit IP adresleriyle doldurulur.


    > [AZURE.NOTE] 
    > 
    > - Denetleyici sabit IP adreslerinin, cihaz IP adresinin erişebildiği alt ağda boş IP’ler olması gerekir.
    > - Denetleyicinin sabit IP adresleri cihaz güncelleştirmelerine hizmet etmesi için kullanılır; bu nedenle de sabit IP'ler yönlendirilebilir ve İnternet'e bağlanabilir olmalıdırlar.

    ![StorSimple en düşük cihaz kurulumu ağ arabirimleri](./media/storsimple-complete-minimum-device-setup/HCS_MinDeviceSetupNetworkInterfaces2-include.png)

5. Onay simgesine ![StorSimple onay simgesi](./media/storsimple-complete-minimum-device-setup/HCS_CheckIcon-include.png) tıklayın.
  Cihazın **Hızlı Başlangıç** sayfasına döneceksiniz.

 > [AZURE.NOTE] **Yapılandır** sayfasına erişerek herhangi bir zaman diğer tüm cihaz ayarlarını değiştirebilirsiniz.

![Kullanılabilir video](./media/storsimple-complete-minimum-device-setup/Video_icon.png) **Kullanılabilir video**

En düşük cihaz kurulumunun nasıl tamamlandığını gösteren bir videoyu izlemek için [buraya](https://azure.microsoft.com/documentation/videos/minimum-storsimple-device-setup/) tıklayın.

<!--HONumber=Sep16_HO3-->


