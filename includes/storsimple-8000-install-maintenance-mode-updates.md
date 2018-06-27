### <a name="install-maintenance-mode-updates-via-windows-powershell-for-storsimple"></a>StorSimple için Windows PowerShell aracılığıyla Bakım modu güncelleştirmeleri yükle

StorSimple cihazına Bakım modu güncelleştirmeleri uyguladığınızda, tüm g/ç istekleri duraklatıldı. Geçici olmayan rasgele erişim belleği (NVRAM) gibi hizmetleri veya Küme hizmeti durdurulur. Girin ya da bu modundan çıkmak hem denetleyicileri yeniden başlatın. Bu mod çıktığınızda, tüm hizmetleri sürdürme ve iyi durumda. (Bu işlem birkaç dakika sürebilir.)

> [!IMPORTANT]
> * Bakım modu girmeden önce her iki aygıt denetleyicileri Azure portalında sağlıklı olduğunu doğrulayın. Denetleyici sağlam, değilse [Microsoft Destek birimine başvurun](../articles/storsimple/storsimple-8000-contact-microsoft-support.md) sonraki adımlar için.
> * Bakım modunda olduğunda, önce bir denetleyici ve ardından diğer denetleyicisi güncelleştirmeniz gerekir.

1. Seri konsoluna bağlanmak için PuTTY kullanın. [Seri konsola bağlanmak için PuTTy kullanma](../articles/storsimple/storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console) bölümündeki ayrıntılı yönergeleri izleyin. Komut isteminde **Enter** tuşuna basın. Seçenek 1 **oturum oturum tam erişim**.

2. Bakım moduna denetleyicisi için şunu yazın:
    
    `Enter-HcsMaintenanceMode`

    İki denetleyiciye bakım moduna yeniden başlatın.

3. Bakım modu güncelleştirmeleri yükleyin. Şunu yazın:

    `Start-HcsUpdate`

    Onayınız istenir. Güncelleştirmeleri onayladıktan sonra bunlar şu anda erişen denetleyicisine yüklenir. Güncelleştirmeler yüklendikten sonra denetleyici yeniden başlatır.

4. Güncelleştirmelerinin durumunu izleyin. Eş denetleyiciye geçerli denetleyicisi güncelleştiriyor ve diğer komutları işleyebilir değil olarak oturum açın. Şunu yazın:

    `Get-HcsUpdateStatus`

    Varsa `RunInProgress` olan `True`, güncelleştirme devam ediyor. Varsa `RunInProgress` olan `False`, güncelleştirme tamamlandı gösterir.

5. Disk Bellenim güncelleştirmeleri başarıyla uygulandıktan ve güncelleştirilmiş denetleyicisi yeniden başlatıldıktan sonra disk bellenim sürümü doğrulayın. Güncelleştirilmiş denetleyicisinde yazın:

    `Get-HcsFirmwareVersion`
   
    Beklenen disk bellenim sürümleri şunlardır:  `XMGJ, XGEG, KZ50, F6C2, VR08, N003, 0107`

6. Bakım modundan çıkın. Her aygıt denetleyicisi için aşağıdaki komutu yazın:

    `Exit-HcsMaintenanceMode`

    Bakım modundan çıktığınızda denetleyiciler yeniden başlatılır.

7. Azure portalına geri dönün. Portal, 24 saat için Bakım modu güncelleştirmeleri yüklü göstermeyebilir.