<!--author=SharS last changed: 02/22/16-->

### <a name="to-configure-and-register-the-device"></a>Cihazı yapılandırmak ve kaydetmek için
1. StorSimple cihazı seri konsolunuzdaki Windows PowerShell arabirimine erişin. Talimatlar için bkz. [Cihaz seri konsoluna bağlanmak için PuTTY kullanma](#use-putty-to-connect-to-the-device-serial-console). **Yordamı hatasız takip ettiğinizden emin olun; aksi taktirde konsola erişemezsiniz.**
2. Açılan oturumda komut istemi almak için bir kez Enter tuşuna basın. 
3. Cihazınızda ayarlamak istediğiniz dili seçmeniz istenecektir. Dili belirtip Enter tuşuna basın. 
   
    ![StorSimple cihazı yapılandırma ve kaydetme 1](./media/storsimple-configure-and-register-device-gov/HCS_RegisterYourDevice1-gov-include.png)
4. Sunulan seri konsol menüsünde seçeneği 1, **oturum oturum tam erişim**. 
   
    ![StorSimple kayıt cihazı 2](./media/storsimple-configure-and-register-device-gov/HCS_RegisterYourDevice2-gov-include.png)
5. Cihazınız için en düşük gerekli ağ ayarlarını yapılandırmak için aşağıdaki adımları gerçekleştirin.
   
   > [!IMPORTANT]
   > Bu yapılandırma adımları, cihazın etkin denetleyicisinde gerçekleştirilmelidir. Seri konsol menüsü, bant iletisindeki denetleyici durumunu belirtir. Olan değil bağlanırsanız etkin denetleyiciye kesin ve etkin denetleyiciye bağlayın.
   > 
   > 
   
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
      > 
      > 
   4. İsteğe bağlı olarak, web proxy sunucunuzu yapılandırın.
      
      > [!IMPORTANT]
      > Web proxy yapılandırması isteğe bağlı olsa da, bir web proxy kullanıyorsanız, yalnızca burada yapılandırabilirsiniz olduğunu unutmayın. Daha fazla bilgi için [Cihazınız için web ara sunucusunu yapılandırma](../articles/storsimple/storsimple-configure-web-proxy.md)’ya gidin. 
      > 
      > 
6. Kurulum sihirbazından çıkmak için Ctrl + C tuşlarına basın.
7. Güncelleştirmeleri gibi yükleyin:
   
   1. Her iki denetleyicilerinde IP'leri ayarlamak için aşağıdaki cmdlet'i kullanın:
      
      `Set-HcsNetInterface -InterfaceAlias Data0 -Controller0IPv4Address <Controller0 IP> -Controller1IPv4Address <Controller1 IP>`
   2. Komut istemine `Get-HcsUpdateAvailability`. Güncelleştirmelerinin kullanılabilir olduğunu bildirilmesi gerekir.
   3. `Start-HcsUpdate` öğesini çalıştırın. Herhangi bir düğümde bu komutu çalıştırabilirsiniz. Güncelleştirmeler ilk denetleyicisinde uygulanır, denetleyici üzerinden başarısız olur ve ardından diğer denetleyicisinde güncelleştirmeler uygulanır.
      
      Çalıştırarak güncelleştirmenin ilerleme durumunu izleyebilirsiniz `Get-HcsUpdateStatus`.    
      
      Devam etmekte olan güncelleştirme aşağıdaki örnek çıktıda gösterilir.
      
      ````
      Controller0>Get-HcsUpdateStatus
      RunInprogress       : True
      LastHotfixTimestamp : 4/13/2015 10:56:13 PM
      LastUpdateTimestamp : 4/13/2015 10:35:25 PM
      Controller0Events   :
      Controller1Events   : 
      ````
      
      Aşağıdaki örnek çıktıda güncelleştirmenin tamamlandığı gösterilir.
      
      ````
      Controller1>Get-HcsUpdateStatus
      
      RunInprogress       : False
      LastHotfixTimestamp : 4/13/2015 10:56:13 PM
      LastUpdateTimestamp : 4/13/2015 10:35:25 PM
      Controller0Events   :
      Controller1Events   :
      
      ````
      
      Windows güncelleştirmeleri de dahil olmak üzere tüm güncelleştirmeleri uygulamanızı 11 saate kadar sürebilir.

8. Tüm güncelleştirmeler başarıyla yüklendikten sonra yazılım güncelleştirmeleri doğru şekilde uygulanıp uygulanmadığını doğrulamak için aşağıdaki cmdlet'i çalıştırın:
   
     `Get-HcsSystem`
   
    Aşağıdaki sürümleri görmeniz gerekir:
   
   * HcsSoftwareVersion: 6.3.9600.17491
   * CisAgentVersion: 1.0.9037.0
   * MdsAgentVersion: 26.0.4696.1433
9. Bellenim güncelleştirme doğru uygulanmış olduğunu onaylamak için aşağıdaki cmdlet'i çalıştırın:
   
    `Start-HcsFirmwareCheck`.
   
     Bellenim durum olmalıdır **UpToDate**.
10. (Bu ortak Azure Klasik portalında varsayılan işaret ettiğinden) cihaz Microsoft Azure kamu Portalı'na işaret etmek için aşağıdaki cmdlet'i çalıştırın. Bu, hem denetleyicileri yeniden başlatılır. Her denetleyici ne zaman yeniden görebilmeniz için aynı anda hem denetleyicileri bağlanmak için iki PuTTY oturumu kullanmanızı öneririz.
    
     `Set-CloudPlatform -AzureGovt_US`
    
    Bir onay iletisi görürsünüz. Varsayılanı kabul (**Y**).
11. Kurulum devam etmek için aşağıdaki cmdlet'i çalıştırın:
    
     `Invoke-HcsSetupWizard`
    
     ![Resume Kurulum Sihirbazı](./media/storsimple-configure-and-register-device-gov/HCS_ResumeSetup-gov-include.png)
    
    Kurulum devam edince Sihirbazı'nı (17469 sürüme karşılık gelir) güncelleştirme 1 sürüm olacaktır. 
12. Ağ ayarlarını kabul edin. Her ayar kabul ettikten sonra bir doğrulama iletisi görürsünüz.
13. Güvenlik nedenleriyle, cihaz yönetici parolasının süresi ilk oturumun ardından dolar; artık bunu değiştirmeniz gerekir. İstendiğinde cihaz yöneticisi parolasını verin. Geçerli bir cihaz yöneticisi parolası 8-15 karakter arasında olmalıdır. Parolada aşağıdakilerden üçü olmalıdır: küçük harf, büyük harf, rakam ve özel karakter.
    
    <br/>![StorSimple kayıt cihazı 5](./media/storsimple-configure-and-register-device-gov/HCS_RegisterYourDevice5_gov-include.png)
14. Kurulum sihirbazının son adımı cihazınızı StorSimple Yöneticisi hizmetine kaydeder. Bunun için makalesinde aldığınız hizmet kayıt anahtarı gerekir [2. adım: Hizmet kayıt anahtarını alın](#step-2-get-the-service-registration-key). Kayıt anahtarını verdikten sonra cihazın kaydolması için 2-3 dakika beklemeniz gerekebilir.
    
    > [!NOTE]
    > Kurulum sihirbazından çıkmak için istediğiniz zaman Ctrl + C tuşlarına basabilirsiniz. Tüm ağ ayarlarını (Data 0 için IP adresi, alt ağ maskesi ve ağ geçidi) girdiyseniz girişleriniz korunur.
    > 
    > 
    
    ![StorSimple kayıt ilerleme durumu](./media/storsimple-configure-and-register-device-gov/HCS_RegistrationProgress-gov-include.png)
15. Cihaz kaydedildikten sonra Hizmet Verileri Şifreleme anahtarı görüntülenir. Bu anahtarı kopyalayın ve güvenli bir konuma kaydedin. **StorSimple Yöneticisi hizmetiyle ek cihazlar kaydetmek için hizmet kayıt anahtarıyla birlikte bu anahtar da gerekecektir.** Bu anahtar hakkında daha fazla bilgi için bkz. [StorSimple güvenliği](../articles/storsimple/storsimple-security.md).
    
    ![StorSimple kayıt cihazı 7](./media/storsimple-configure-and-register-device-gov/HCS_RegisterYourDevice7_gov-include.png)    
    
    > [!IMPORTANT]
    > Seri konsol penceresinden metin kopyalamak için metni seçmeniz yeterlidir. Artık bunu panoya veya metin düzenleyicilere yapıştırabilirsiniz. 
    > 
    > Hizmet verileri şifreleme anahtarını kopyalamak için CTRL + C tuşlarını kullanmayın. Ctrl + C tuşlarının kullanılması kurulum sihirbazından çıkmanıza neden olur. Sonuç olarak, cihaz yöneticisi parolası değiştirilmez ve cihaz varsayılan parolasına döner.
    > 
    > 
16. Seri konsoldan çıkın.
17. Azure Kamu portalına geri dönün ve aşağıdaki adımları tamamlayın:
    
    1. **Hızlı Başlangıç** sayfasına erişmek için StorSimple Yöneticisi hizmetinize çift tıklayın.
    2. **Bağlı cihazları görüntüle**’ye tıklayın.
    3. **Cihazlar** sayfasında, durumu arayarak cihazın hizmete sorunsuz bağlandığını doğrulayın. Cihazın durumu **Çevrimiçi** olmalıdır.
       
        ![StorSimple Cihazları sayfası](./media/storsimple-configure-and-register-device-gov/HCS_DeviceOnline-gov-include.png) 
       
        Cihazın durumu **Çevrimdışı** olduğu durumda, birkaç dakikada cihazın çevrimiçi olması bekleyin. 
       
        Birkaç dakika sonra cihaz yine de çevrimdışıysa, güvenlik duvarı ağınızın [StorSimple cihazınız için ağ gereksinimlerinde](../articles/storsimple/storsimple-system-requirements.md) açıklandığı şekilde yapılandırıldığından emin olun. 
       
        Bu hizmet veri yolu tarafından StorSimple Yöneticisi hizmet-cihaz iletişimi için kullanılan gibi 9354 bağlantı noktasının giden iletişim için açık olduğunu doğrulayın.

