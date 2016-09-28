<!--author=alkohli last changed: 9/17/15-->

#### En düşük StorSimple cihaz kurulumunu tamamlamak için

1. Cihazı seçin ve **Hızlı Başlangıç**’a tıklayın. Cihaz yapılandırma sihirbazını başlatmak için **Cihaz kurulumunu tamamla**’ya tıklayın.

2. Cihaz yapılandırma sihirbazının **Temel Ayarlar** iletişim kutusunda şunları yapın:
  1. Cihazınızın **kolay adını** sağlayın. Varsayılan cihaz adı, cihaz modeli ve seri numarası gibi bilgileri yansıtır. Cihazı yönetmek için En fazla 64 karakterlik bir kolay ad atayabilirsiniz.
  2. Cihazın dağıtıldığı coğrafi konum temelinde **saat dilimini** ayarlayın. Cihazınız zamanlanan tüm işlemler için bu saat dilimini kullanır.
  3. **DNS Ayarları** altında **ikincil DNS Sunucusu** için bir adres girin. IPv6 kullanıyorsanız, bu alan Windows PowerShell arabiriminde sağlanan IPv6 önekini temel alarak doldurulacaktır. 
  İkincil DNS sunucusu yapılandırılmadıysa, cihaz yapılandırmanızı kaydetmenize izin verilmez.
  4. iSCSI etkin arabirimlerin altında iSCSI için en az bir ağ etkinleştirin. En az bir ağ arabiriminin bulut etkin olması ve bir arabirimin de iSCSI etkin olması gerekir. DATA 0 otomatik olarak bulut etkindir.
 
      ![StorSimple en düşük cihaz kurulumu temel ayarları](./media/storsimple-complete-minimum-device-setup-u1/HCS_MinDeviceSetupBasicSettings1-include.png)

3. Ok simgesine tıklayın. ![StorSimple ok simgesi](./media/storsimple-complete-minimum-device-setup/HCS_ArrowIcon-include.png)

4. **Ağ Arabirimleri** iletişim kutusunda, Denetleyici 0 ve Denetleyici 1 için sabit IP adreslerini verin. **Denetleyici sabit IP adreslerinin, cihaz IP adresinin erişebildiği alt ağda boş IP’ler olması gerekir.** DATA 0 arabirimi IPv4 için yapılandırılmışsa sabit IP adreslerinin IPv4 biçiminde verilmesi gerekir. IPv6 yapılandırması için bir önek sağladıysanız bu alanlar otomatik olarak sabit IP adresleriyle doldurulur.


    ![StorSimple en düşük cihaz kurulumu ağ arabirimleri](./media/storsimple-complete-minimum-device-setup-u1/HCS_MinDeviceSetupNetworkInterfaces2-include.png)

    Denetleyicinin sabit IP adresleri cihaz güncelleştirmelerine hizmet etmesi için kullanılır; bu nedenle de sabit IP'ler yönlendirilebilir ve İnternet'e bağlanabilir olmalıdırlar. Sabit denetleyici IP'lerinizin yönlendirilebilir olduğunu [Test HcsmConnection][Test] cmdlet'ini kullanarak denetleyebilirsiniz. Aşağıdaki örnekte sabit denetleyici IP'lerin İnternet'e yönlendirildiği ve Microsoft Update sunucularına erişebildiği gösterilmektedir. 

     ![Yönlendirilebilir IP’leri gösteren Test HcsmConnection](./media/storsimple-complete-minimum-device-setup-u1/Test-HcsmConnectionOutputRegisteredDevice.png)

5. Onay simgesine ![StorSimple onay simgesi](./media/storsimple-complete-minimum-device-setup/HCS_CheckIcon-include.png) tıklayın.
  Cihazın **Hızlı Başlangıç** sayfasına döneceksiniz.

 > [AZURE.NOTE] **Yapılandır** sayfasına erişerek herhangi bir zaman diğer tüm cihaz ayarlarını değiştirebilirsiniz.

<!--Link reference-->
[Test]: https://technet.microsoft.com/library/dn715782(v=wps.630).aspx

<!--HONumber=Sep16_HO3-->


