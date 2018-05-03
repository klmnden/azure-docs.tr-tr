<!--author=alkohli last changed: 12/01/15-->


#### <a name="to-configure-and-register-the-device"></a>Cihazı yapılandırmak ve kaydetmek için
1. StorSimple cihazı seri konsolunuzdaki Windows PowerShell arabirimine erişin. Talimatlar için bkz. [Cihaz seri konsoluna bağlanmak için PuTTY kullanma](#use-putty-to-connect-to-the-device-serial-console). **Yordamı hatasız takip ettiğinizden emin olun; aksi taktirde konsola erişemezsiniz.**
2. Açılan oturumda komut istemi almak için bir kez Enter tuşuna basın. 
3. Cihazınızda ayarlamak istediğiniz dili seçmeniz istenecektir. Dili belirtip Enter tuşuna basın. 
   
    ![StorSimple cihazı yapılandırma ve kaydetme 1](./media/storsimple-configure-and-register-device/HCS_RegisterYourDevice1-include.png)
4. Verilen seri konsol menüsünde tam erişimle oturum açmak için 1 seçeneğini belirleyin. 
   
    ![StorSimple kayıt cihazı 2](./media/storsimple-configure-and-register-device/HCS_RegisterYourDevice2-include.png)
   
     Cihazınız için en düşük gerekli ağ ayarlarını yapılandırmak için 5-12 arası adımları tamamlayın. **Bu yapılandırma adımları, cihazın etkin denetleyicisinde gerçekleştirilmelidir.** Seri konsol menüsü, bant iletisindeki denetleyici durumunu belirtir. Etkin denetleyiciye bağlı değilseniz, bağlantıyı kesip etkin denetleyiciye bağlayın.
5. Komut istemine parolanızı yazın. Varsayılan cihaz parolası **Password1**’dir.
6. Aşağıdaki komutu yazın:
   
     `Invoke-HcsSetupWizard` 
7. Cihazın ağ ayarlarını yapılandırmanıza yardımcı olacak bir kurulum sihirbazı görüntülenir. Aşağıdaki bilgileri verin: 
   
   * DATA 0 ağ arabirimi için IP adresi
   * Alt ağ maskesi
   * Ağ geçidi
   * Birincil DNS sunucusu için IP adresi
   * Birincil NTP sunucusu için IP adresi
     
     > [!NOTE]
     > Uygulanacak alt ağ maskesi ve DNS ayarları için dakika beklemeniz gerekebilir. "Cihaz hazır değil." hata iletisi alırsanız, etkin denetleyicinizin DATA 0 ağ arabirimindeki fiziksel ağ bağlantısını işaretleyin.
     > 
     > 
8. (İsteğe bağlı), web ara sunucusunu yapılandırın. Web ara sunucusunun yapılandırması isteğe bağlı olsa **bir web ara sunucu kullanıyorsanız, burada yalnızca bunu yapılandırabileceğinizi unutmayın**. Daha fazla bilgi için [Cihazınız için web ara sunucusunu yapılandırma](../articles/storsimple/storsimple-configure-web-proxy.md)’ya gidin. Bu adım sırasında sorunlarla karşılaşırsanız, sorun giderme kılavuzluğu için bkz. [Web ara sunucunun yapılandırması sırasında hatalar](../articles/storsimple/storsimple-troubleshoot-deployment.md#errors-during-the-optional-web-proxy-settings).

     > [!NOTE]
     > Kurulum sihirbazından çıkmak için istediğiniz zaman Ctrl + C tuşlarına basabilirsiniz. Bu komutu bildirmeden önce uyguladığınız ayarlar korunur.

1. Güvenlik nedenleriyle, cihaz yönetici parolasının süresi ilk oturumun ardından dolar; sonraki oturumlar için bunu değiştirmeniz gerekir. İstendiğinde cihaz yöneticisi parolasını verin. Geçerli bir cihaz yöneticisi parolası 8-15 karakter arasında olmalıdır. Parolada küçük harflerin, büyük harflerin, rakamların ve özel karakterlerin bir karışımı bulunur.
2. StorSimple Snapshot Manager parolası burada da ayarlanır. StorSimple Snapshot Manager çalıştıran Windows konağınızla bir cihazın kimlik doğrulamasını yaptığınızda bu parolayı kullanın. İstendiğinde, 14-15 karakter arası uzunlukta bir parola sağlayın. Parolada aşağıdakilerden üçünün bir birleşimi olmalıdır: küçük harf, büyük harf, rakam ve özel karakter. 
   
   ![StorSimple kayıt cihazı 4](./media/storsimple-configure-and-register-device/HCS_RegisterYourDevice4-include.png)
   
   StorSimple Snapshot Manager parolasını StorSimple Yöneticisi hizmet arabirimden sıfırlayabilirsiniz. Ayrıntılı adımlar için [StorSimple Yöneticisi hizmetini kullanarak StorSimple parolalarını değiştirme](../articles/storsimple/storsimple-change-passwords.md).
   
   Bu adım sırasında sorunları gidermek için sorun giderme kılavuzluğu için bkz. [Parolalarla ilgili hatalar](../articles/storsimple/storsimple-troubleshoot-deployment.md#errors-related-to-device-administrator-and-storsimple-snapshot-manager-passwords).
3. Kurulum sihirbazının son adımı cihazınızı StorSimple Yöneticisi hizmetine kaydeder. Bunun için, 2. adımda aldığınız hizmet kayıt anahtarı gerekir. Kayıt anahtarını verdikten sonra cihazın kaydolması için 2-3 dakika beklemeniz gerekebilir.
   
   Olası cihaz kayıt sorunlarını gidermek için bkz. [Cihaz kaydı sırasında hatalar](../articles/storsimple/storsimple-troubleshoot-deployment.md#errors-during-device-registration). Ayrıntılı sorun giderme için de bkz. [Adım adım sorun giderme örneği](../articles/storsimple/storsimple-troubleshoot-deployment.md#step-by-step-storsimple-troubleshooting-example).
4. Cihaz kaydedildikten sonra Hizmet Verileri Şifreleme anahtarı görüntülenir. Bu anahtarı kopyalayın ve güvenli bir konuma kaydedin.
   
   > [!WARNING]
   > StorSimple Yöneticisi hizmetiyle ek cihazlar kaydetmek için hizmet kayıt anahtarıyla birlikte bu anahtar da gerekecektir. Bu anahtar hakkında daha fazla bilgi için bkz. [StorSimple güvenliği](../articles/storsimple/storsimple-security.md).
   > 
   > 
   
    ![StorSimple kayıt cihazı 6](./media/storsimple-configure-and-register-device/HCS_RegisterYourDevice6-include.png)
   
    Seri konsol penceresinden metin kopyalamak için metni seçmeniz yeterlidir. Artık bunu panoya veya metin düzenleyicilere yapıştırabilirsiniz. Hizmet verileri şifreleme anahtarını kopyalamak için CTRL + C tuşlarını kullanmayın. Ctrl + C tuşlarının kullanılması kurulum sihirbazından çıkmanıza neden olur. Sonuç olarak, cihaz yöneticisi parolası ve StorSimple Snapshot Manager parolası değiştirilmez ve cihaz varsayılan parolalarına döner.
5. Seri konsoldan çıkın.
6. Klasik Azure portalına dönün ve aşağıdaki adımları tamamlayın:
   
   1. **Hızlı Başlangıç** sayfasına erişmek için StorSimple Yöneticisi hizmetinize çift tıklayın.
   2. **Bağlı cihazları görüntüle**’ye tıklayın.
   3. **Cihazlar** sayfasında, durumu arayarak cihazın hizmete sorunsuz bağlandığını doğrulayın. Cihazın durumu **Çevrimiçi** olmalıdır. Cihazın durumu **Çevrimdışı** olduğu durumda, birkaç dakikada cihazın çevrimiçi olması bekleyin.
   
   ![StorSimple Cihazları sayfası](./media/storsimple-configure-and-register-device/HCS_DevicesPageM-include.png) 
   
   > [!IMPORTANT]
   > Cihaz çevrimiçi olduktan sonra, bu adımın başında söktüğünüz ağ kablolarını takın.
   > 
   > 

Cihaz sorunsuz kaydedildikten ve çevrimiçi olmadıktan sonra, ağ bağlantısının sağlıklı olmasını sağlamak için `Test-HcsmConnection -Verbose` komutunu çalıştırabilirsiniz. Bu cmdlet’in ayrıntılı kullanımı için [Test HcsmConnection için cmdlet başvurusu](https://technet.microsoft.com/library/dn715782.aspx)’na gidin.

![Video var](./media/storsimple-configure-and-register-device/Video_icon.png) **Video var**

StorSimple için Windows PowerShell aracılığıyla cihazınızın nasıl yapılandırılacağını ve kaydedileceğini gösteren bir video izlemek için [buraya](https://azure.microsoft.com/documentation/videos/initialize-the-storsimple-appliance/) tıklayın.

