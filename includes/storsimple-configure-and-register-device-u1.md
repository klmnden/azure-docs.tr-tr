<!--author=alkohli last changed: 02/22/2016-->


### Cihazı yapılandırmak ve kaydetmek için

1. StorSimple cihazı seri konsolunuzdaki Windows PowerShell arabirimine erişin. Talimatlar için bkz. [Cihaz seri konsoluna bağlanmak için PuTTY kullanma](#use-putty-to-connect-to-the-device-serial-console). **Yordamı hatasız takip ettiğinizden emin olun; aksi taktirde konsola erişemezsiniz.**

2. Açılan oturumda komut istemi almak için bir kez Enter tuşuna basın. 

3. Cihazınızda ayarlamak istediğiniz dili seçmeniz istenecektir. Dili belirtip Enter tuşuna basın. 

    ![StorSimple cihazı yapılandırma ve kaydetme 1](./media/storsimple-configure-and-register-device-u1/HCS_RegisterYourDevice1-U1-include.png)

4. Verilen seri konsol menüsünde tam erişimle oturum açmak için 1 seçeneğini belirleyin. 

    ![StorSimple kayıt cihazı 2](./media/storsimple-configure-and-register-device-u1/HCS_RegisterYourDevice2_U1-include.png)
  
     Cihazınız için en düşük gerekli ağ ayarlarını yapılandırmak için 5-12 arası adımları tamamlayın. **Bu yapılandırma adımları, cihazın etkin denetleyicisinde gerçekleştirilmelidir.** Seri konsol menüsü, bant iletisindeki denetleyici durumunu belirtir. Etkin denetleyiciye bağlı değilseniz, bağlantıyı kesip etkin denetleyiciye bağlayın.

5. Komut istemine parolanızı yazın. Varsayılan cihaz parolası **Password1**’dir.

6. Şu komutu yazın: `Invoke-HcsSetupWizard`. 

7. Cihazın ağ ayarlarını yapılandırmanıza yardımcı olacak bir kurulum sihirbazı görüntülenir. Aşağıdaki bilgileri verin: 
   - DATA 0 ağ arabirimi için IP adresi
   - Alt ağ maskesi
   - Ağ geçidi
   - Birincil DNS sunucusu için IP adresi
    
        İşlemin her adımından sonra, sistemin ağ ayarlarını doğruladığını unutmayın.
   
      > [AZURE.NOTE] Uygulanacak alt ağ maskesi ve DNS ayarları için dakika beklemeniz gerekebilir. “Data 0 ağ bağlantısını denetleyin” hata iletisi alırsanız, etkin denetleyicinizin DATA 0 ağ arabirimindeki fiziksel ağ bağlantısını işaretleyin.

8. (İsteğe bağlı), web ara sunucusunu yapılandırın. Web ara sunucusunun yapılandırması isteğe bağlı olsa **bir web ara sunucu kullanıyorsanız, burada yalnızca bunu yapılandırabileceğinizi unutmayın**. Daha fazla bilgi için [Cihazınız için web ara sunucusunu yapılandırma](../articles/storsimple/storsimple-configure-web-proxy.md)’ya gidin.

9. Cihazınız için Birincil NTP sunucusunu yapılandırın. Bulut hizmeti sağlayıcılarıyla doğrulanabileceği şekilde cihazınızı saati eşitlemesi gerektiğinden NTP sunucuları gereklidir. Ağınızın, NTP trafiğini veri merkezinizden İnternete geçirilmesine izin vermesini sağlayın. Bu yapılamıyorsa dahili bir NTP sunucusu belirtin. 
 
10. Güvenlik nedenleriyle, cihaz yönetici parolasının süresi ilk oturumun ardından dolar; artık bunu değiştirmeniz gerekir. İstendiğinde cihaz yöneticisi parolasını verin. Geçerli bir cihaz yöneticisi parolası 8-15 karakter arasında olmalıdır. Parolada aşağıdakilerden üçü olmalıdır: küçük harf, büyük harf, rakam ve özel karakter.

    <br/>![StorSimple kayıt cihazı 5](./media/storsimple-configure-and-register-device-u1/HCS_RegisterYourDevice5_U1-include.png)

11. Kurulum sihirbazının son adımı cihazınızı StorSimple Yöneticisi hizmetine kaydeder. Bunun için, 2. adımda aldığınız hizmet kayıt anahtarı gerekir. Kayıt anahtarını verdikten sonra cihazın kaydolması için 2-3 dakika beklemeniz gerekebilir.

      > [AZURE.NOTE] Kurulum sihirbazından çıkmak için istediğiniz zaman Ctrl + C tuşlarına basabilirsiniz. Tüm ağ ayarlarını (Data 0 için IP adresi, alt ağ maskesi ve ağ geçidi) girdiyseniz girişleriniz korunur.

    ![StorSimple kayıt cihazı 6](./media/storsimple-configure-and-register-device-u1/HCS_RegisterYourDevice6_U1-include.png)

12. Cihaz kaydedildikten sonra Hizmet Verileri Şifreleme anahtarı görüntülenir. Bu anahtarı kopyalayın ve güvenli bir konuma kaydedin. **StorSimple Yöneticisi hizmetiyle ek cihazlar kaydetmek için hizmet kayıt anahtarıyla birlikte bu anahtar da gerekecektir.** Bu anahtar hakkında daha fazla bilgi için bkz. [StorSimple güvenliği](../articles/storsimple/storsimple-security.md).
    
    ![StorSimple kayıt cihazı 7](./media/storsimple-configure-and-register-device-u1/HCS_RegisterYourDevice7_U1-include.png)    

      > [AZURE.NOTE] Seri konsol penceresinden metin kopyalamak için metni seçmeniz yeterlidir. Artık bunu panoya veya metin düzenleyicilere yapıştırabilirsiniz. Hizmet verileri şifreleme anahtarını kopyalamak için CTRL + C tuşlarını kullanmayın. Ctrl + C tuşlarının kullanılması kurulum sihirbazından çıkmanıza neden olur. Sonuç olarak, cihaz yöneticisi parolası değiştirilmez ve cihaz varsayılan parolasına döner.

13. Seri konsoldan çıkın.

14. Klasik Azure portalına dönün ve aşağıdaki adımları tamamlayın:
  1. **Hızlı Başlangıç** sayfasına erişmek için StorSimple Yöneticisi hizmetinize çift tıklayın.
  2. **Bağlı cihazları görüntüle**’ye tıklayın.
  3. **Cihazlar** sayfasında, durumu arayarak cihazın hizmete sorunsuz bağlandığını doğrulayın. Cihazın durumu **Çevrimiçi** olmalıdır.
   
        ![StorSimple Cihazları sayfası](./media/storsimple-configure-and-register-device-u1/HCS_DevicesPageM_U1-include.png) 
  
        Cihazın durumu **Çevrimdışı** olduğu durumda, birkaç dakikada cihazın çevrimiçi olması bekleyin. 

        Birkaç dakika sonra cihaz yine de çevrimdışıysa, güvenlik duvarı ağınızın [StorSimple cihazınız için ağ gereksinimlerinde](../articles/storsimple/storsimple-system-requirements.md) açıklandığı şekilde yapılandırıldığından emin olun. 

        Bu hizmet veri yolu tarafından StorSimple Yöneticisi Hizmet - Cihaz iletişimi için hizmet veri yolu tarafından kullanıldığından, 9354 bağlantı noktasının giden iletişim için açık olduğunu doğrulayın.
     
       



<!--HONumber=Jun16_HO2-->


