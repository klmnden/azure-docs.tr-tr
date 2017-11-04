<!--author=SharS last changed: 9/17/15-->

#### <a name="to-install-maintenance-mode-updates-via-windows-powershell-for-storsimple"></a>StorSimple için Windows PowerShell aracılığıyla Bakım modu güncelleştirmeleri yüklemek için
1. Henüz yapmadıysanız, seçenek 1 ve cihaz seri konsoluna erişim **oturum oturum tam erişim**. 
2. Parolayı yazın. Varsayılan parola **Parola1**.
3. Komut istemine yazın:
   
     `Get-HcsUpdateAvailability` 
4. Güncelleştirmeler varsa ve güncelleştirmeleri kesintiye uğratan veya benzer olup bildirilir. Kesintiye uğratan güncelleştirmeleri uygulamak için aygıt bakım moduna almanız gerekir. Bkz: [2. adım: girin Bakım modu](../articles/storsimple/storsimple-update-device.md#step2) yönergeler için.
5. Cihazınızı bakım modunda olduğunda komut istemine yazın:`Start-HcsUpdate`
6. Onayınız istenir. Güncelleştirmeleri onayladıktan sonra şu anda erişen denetleyicisine yüklenir. Güncelleştirmeler yüklendikten sonra denetleyici yeniden başlatılır. 
7. Güncelleştirmelerinin durumunu izleyin. Şunu yazın:
   
    `Get-HcsUpdateStatus`
   
    Varsa `RunInProgress` olan `True`, güncelleştirme devam ediyor. Varsa `RunInProgress` olan `False`, güncelleştirme tamamlandı gösterir.  
8. Güncelleştirme geçerli denetleyicisine yüklenir ve yeniden başlatıldı, diğer denetleyicisine bağlanmak ve 1 ile 6 arasındaki adımları gerçekleştirin.
9. Hem denetleyicileri güncelleştirildikten sonra bakım modundan çıkın. Bkz: [4. adım: çıkış Bakım modu](../articles/storsimple/storsimple-update-device.md#step4) yönergeler için.

