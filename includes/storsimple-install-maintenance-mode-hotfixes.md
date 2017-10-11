<!--author=SharS last changed: 9/17/15-->

#### <a name="to-install-maintenance-mode-hotfixes-via-windows-powershell-for-storsimple"></a>StorSimple için Windows PowerShell aracılığıyla Bakım modu düzeltmeler yüklemek için
> [!IMPORTANT]
> Bakım modunda bir denetleyici ve ardından diğer denetleyicisi ilk düzeltmeyi uygulamanız gerekir.
> 
> 

1. Cihazın bakım moduna yerleştirin. Bkz: [2. adım: girin Bakım modu](../articles/storsimple/storsimple-update-device.md#step2) bakım moduna hakkında yönergeler için.
2. Düzeltmeyi uygulamak için şunu yazın:
   
     `Start-HcsHotfix` 
3. İstendiğinde, düzeltme dosyalarını içeren ağ paylaşılan klasörün yolunu sağlayın.
4. Onayınız istenir. Tür **Y** düzeltme yüklemeye devam etmek için.
5. Bir denetleyicisinde düzeltmeyi uyguladıktan sonra diğer denetleyiciye oturum açın. Önceki denetleyici için yaptığınız gibi düzeltmeyi uygulayın.
6. Düzeltmeleri uyguladıktan sonra bakım modundan çıkın. Bkz: [4. adım: çıkış Bakım modu](../articles/storsimple/storsimple-update-device.md#step4) yönergeler için.

