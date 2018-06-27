<!--author=SharS last changed: 06/22/2016-->

### <a name="to-configure-and-register-the-device"></a>Cihazı yapılandırmak ve kaydetmek için
1. StorSimple cihazı seri konsolunuzdaki Windows PowerShell arabirimine erişin. Talimatlar için bkz. [Cihaz seri konsoluna bağlanmak için PuTTY kullanma](../articles/storsimple/storsimple-8000-deployment-walkthrough-gov-u2.md#use-putty-to-connect-to-the-device-serial-console). **Yordamı hatasız takip ettiğinizden emin olun; aksi taktirde konsola erişemezsiniz.**
2. Açılan oturumda komut istemi almak için bir kez **Enter** tuşuna basın.
3. Cihazınızda ayarlamak istediğiniz dili seçmeniz istenecektir. Dili belirtip **Enter** tuşuna basın.
   
    ![StorSimple cihazı yapılandırma ve kaydetme 1](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegisterYourDevice1-gov-include.png)
4. Sunulan seri konsol menüsünde seçeneği 1, **oturum oturum tam erişim**.
   
    ![StorSimple kayıt cihazı 2](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegisterYourDevice2-gov-include.png)
5. Cihazınız için en düşük gerekli ağ ayarlarını yapılandırmak için aşağıdaki adımları gerçekleştirin.
   
   > [!IMPORTANT]
   > Bu yapılandırma adımları, cihazın etkin denetleyicisinde gerçekleştirilmelidir. Seri konsol menüsü, bant iletisindeki denetleyici durumunu belirtir. Olan değil bağlanırsanız etkin denetleyiciye kesin ve etkin denetleyiciye bağlayın.
   
   1. Komut istemine parolanızı yazın. Varsayılan cihaz parolası **Password1**’dir.
   2. Aşağıdaki komutu yazın:
      
        `Invoke-HcsSetupWizard`
   3. Cihazın ağ ayarlarını yapılandırmanıza yardımcı olacak bir kurulum sihirbazı görüntülenir. Aşağıdaki bilgileri sağlayın:
      
      * Veri 0 ağ arabirimi için IP adresi
      * Alt ağ maskesi
      * Ağ geçidi
      * Birincil DNS sunucusu için IP adresi
      * Birincil NTP sunucusu için IP adresi
      
      > [!NOTE]
      > Alt ağ maskesi ve DNS ayarlarının uygulanması için birkaç dakika beklemeniz gerekebilir.
    
   4. İsteğe bağlı olarak, web proxy sunucunuzu yapılandırın.
      
      > [!IMPORTANT]
      > Web proxy yapılandırması isteğe bağlı olsa da, bir web proxy kullanıyorsanız, yalnızca burada yapılandırabilirsiniz olduğunu unutmayın. Daha fazla bilgi için [Cihazınız için web ara sunucusunu yapılandırma](../articles/storsimple/storsimple-configure-web-proxy.md)’ya gidin.
     
6. Kurulum sihirbazından çıkmak için Ctrl + C tuşlarına basın.
8. (Bu ortak Azure Klasik portalında varsayılan işaret ettiğinden) cihaz Microsoft Azure kamu Portalı'na işaret etmek için aşağıdaki cmdlet'i çalıştırın. Bu, hem denetleyicileri yeniden başlatılır. Her denetleyici ne zaman yeniden görebilmeniz için aynı anda hem denetleyicileri bağlanmak için iki PuTTY oturumu kullanmanızı öneririz.
   
    `Set-CloudPlatform -AzureGovt_US`
   
   Bir onay iletisi görürsünüz. Varsayılanı kabul (**Y**).
9. Kurulum devam etmek için aşağıdaki cmdlet'i çalıştırın:
   
    `Invoke-HcsSetupWizard`
   
    ![Resume Kurulum Sihirbazı](./media/storsimple-configure-and-register-device-gov-u2/HCS_ResumeSetup-gov-include.png)
   
10. Ağ ayarlarını kabul edin. Her ayar kabul ettikten sonra bir doğrulama iletisi görürsünüz.
11. Güvenlik nedenleriyle, cihaz yönetici parolasının süresi ilk oturumun ardından dolar; artık bunu değiştirmeniz gerekir. İstendiğinde cihaz yöneticisi parolasını verin. Geçerli bir cihaz yöneticisi parolası 8-15 karakter arasında olmalıdır. Parolada aşağıdakilerden üçü olmalıdır: küçük harf, büyük harf, rakam ve özel karakter.
    
    <br/>![StorSimple kayıt cihazı 5](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegisterYourDevice5_gov-include.png)
12. Kurulum sihirbazının son adımı cihazınızı StorSimple Cihaz Yöneticisi hizmetine kaydeder. Bunun için makalesinde aldığınız hizmet kayıt anahtarı gerekir [2. adım: Hizmet kayıt anahtarını alın](../articles/storsimple/storsimple-8000-deployment-walkthrough-gov-u2.md#step-2-get-the-service-registration-key). Kayıt anahtarını verdikten sonra cihazın kaydolması için 2-3 dakika beklemeniz gerekebilir.
    
    > [!NOTE]
    > Kurulum sihirbazından çıkmak için istediğiniz zaman Ctrl + C tuşlarına basabilirsiniz. Tüm ağ ayarlarını (Data 0 için IP adresi, alt ağ maskesi ve ağ geçidi) girdiyseniz girişleriniz korunur.
    
    ![StorSimple kayıt ilerleme durumu](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegistrationProgress-gov-include.png)
13. Cihaz kaydedildikten sonra Hizmet Verileri Şifreleme anahtarı görüntülenir. Bu anahtarı kopyalayın ve güvenli bir konuma kaydedin. **Bu anahtar StorSimple cihaz Yöneticisi hizmetiyle ek cihazlar kaydetmek için hizmet kayıt anahtarı gereklidir.** Bu anahtar hakkında daha fazla bilgi için bkz. [StorSimple güvenliği](../articles/storsimple/storsimple-8000-security.md).
    
    ![StorSimple kayıt cihazı 7](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegisterYourDevice7_gov-include.png)
    > [!IMPORTANT]
    > Seri konsol penceresinden metin kopyalamak için metni seçmeniz yeterlidir. Artık bunu panoya veya metin düzenleyicilere yapıştırabilirsiniz.
    > 
    > Kullanmayın **Ctrl + C** hizmet verileri şifreleme anahtarını kopyalamak için. Kullanarak **Ctrl + C** Kurulum sihirbazından çıkmak neden olur. Sonuç olarak, cihaz yöneticisi parolası değiştirilmez ve cihaz varsayılan parolasına döner.
    
14. Seri konsoldan çıkın.
15. Azure Kamu portalına geri dönün ve aşağıdaki adımları tamamlayın:
    
    1. StorSimple Cihaz Yöneticisi hizmetinize gidin.
    2. **Cihazlar**’a tıklayın. Aygıtlar listesinden ddeploying olduğunuz cihazı tanımlayın. Cihaz başarıyla hizmete durumu arayarak bağlandığını doğrulayın. Cihazın durumu **Çevrimiçi** olmalıdır.
            
        Cihazın durumu **Çevrimdışı** olduğu durumda, birkaç dakikada cihazın çevrimiçi olması bekleyin.
       
        Birkaç dakika sonra cihaz yine de çevrimdışıysa, güvenlik duvarı ağınızın [StorSimple cihazınız için ağ gereksinimlerinde](../articles/storsimple/storsimple-8000-system-requirements.md) açıklandığı şekilde yapılandırıldığından emin olun.
       
        StorSimple Cihaz Yöneticisi Hizmeti ile cihazlar arasındaki iletişim için hizmet veri yolu tarafından kullanılan 9354 bağlantı noktasının giden iletişim için açık olduğunu doğrulayın.

