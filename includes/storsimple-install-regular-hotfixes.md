<!--author=SharS last changed: 9/17/15-->

#### <a name="to-install-regular-hotfixes-via-windows-powershell-for-storsimple"></a>StorSimple için Windows PowerShell aracılığıyla normal düzeltmeler yüklemek için
1. Cihaz seri konsoluna bağlanmak. Daha fazla bilgi için bkz: [1. adım: seri konsoluna bağlanmak](../articles/storsimple/storsimple-update-device.md#step1).
2. Seri konsol menüsünde seçeneğini 1, **oturum oturum tam erişim**. Parolayı yazın. Varsayılan parola **Parola1**.
3. Komut istemine yazın:
   
    ```
    Start-HcsHotfix
    ```
   
    > [!IMPORTANT]
    >
    > Bu komut, yalnızca normal düzeltmeleri uygular. Bu komut yalnızca bir denetleyicisinde çalıştırın, ancak her iki denetleyicileri güncelleştirilir.
    > Denetleyici yük devretmesi güncelleştirme işlemi sırasında fark edebilirsiniz; Ancak, yük devretme sistem kullanılabilirliğini veya işlem etkilemez.

4. İstendiğinde, düzeltme dosyalarını içeren ağ paylaşılan klasörün yolunu sağlayın.
5. Onayınız istenir. Tür **Y** düzeltme yüklemeye devam etmek için.

