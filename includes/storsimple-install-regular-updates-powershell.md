<!--author=SharS last changed: 11/18/16-->

#### <a name="to-install-regular-updates-via-windows-powershell-for-storsimple"></a>StorSimple için Windows PowerShell aracılığıyla düzenli olarak güncelleştirmeleri yüklemek için
1. Cihaz seri konsoluna açıp seçenek 1, **oturum oturum tam erişim**. Parolayı yazın. Varsayılan parola *Parola1*. 
2. Komut istemine yazın:
   
     `Get-HcsUpdateAvailability`
   
    Güncelleştirmeler varsa ve güncelleştirmeleri kesintiye uğratan veya benzer olup bildirilir.
3. Komut istemine yazın:
   
     `Start-HcsUpdate`
   
    Güncelleştirme işlemi başlar.

> [!IMPORTANT]
> * Bu komut, yalnızca normal güncelleştirmeler için geçerlidir. Bu komut yalnızca bir denetleyicisinde çalıştırın, ancak her iki denetleyicileri güncelleştirilir. 
> * Denetleyici yük devretmesi güncelleştirme işlemi sırasında fark edebilirsiniz; Ancak, yük devretme sistem kullanılabilirliğini veya işlem etkilemez.
> 
> 

