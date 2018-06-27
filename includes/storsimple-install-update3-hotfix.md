<!--author=alkohli last changed: 09/01/16-->

#### <a name="to-download-hotfixes"></a>Düzeltmeleri indirmek için
Microsoft Update Kataloğu'ndan yazılım güncelleştirmesi indirmek için aşağıdaki adımları uygulayın.

1. Internet Explorer'ı başlatın ve gidin [ http://catalog.update.microsoft.com ](http://catalog.update.microsoft.com).
2. Microsoft Update Kataloğu’nu bu bilgisayarda ilk kez kullanıyorsanız, sorulduğunda **Yükle**’ye tıklayarak Microsoft Update Kataloğu eklentisini yükleyin.
    ![Katalog yükleyin](./media/storsimple-install-update2-hotfix/HCS_InstallCatalog-include.png)
3. Microsoft Update Kataloğu arama kutusuna, örneğin, yüklemek istediğiniz düzeltme Bilgi Bankası (KB) sayısını girin **3186843**ve ardından **arama**.
   
    Düzeltme listesi göründüğünde, örneğin, **StorSimple 8000 serisi için toplu yazılım paketini güncelleştirme 3.0**.
   
    ![Katalogda arama](./media/storsimple-install-update2-hotfix/HCS_SearchCatalog1-include.png)
4. **Ekle**'ye tıklayın. Güncelleştirme sepete eklenir.
5. Ek düzeltmeleri arayın yukarıdaki tabloda listelenen (**3186859**) ve her Sepete ekleyin.
6. **Sepeti Görüntüle**’ye tıklayın.
7. **İndir**’e tıklayın. İndirilen öğelerin görünmesini istediğiniz yerel konumu belirtin veya **Gözat** seçeneğiyle konumu bulun. Güncelleştirmeleri belirtilen konuma indirilir ve güncelleştirme aynı ada sahip bir alt klasöre yerleştirilir. Klasör, cihazdan erişilebilen bir ağ paylaşımına da kopyalanabilir.

> [!NOTE]
> Düzeltmeleri tüm olası hata iletilerini eş denetleyicisinden algılamak için her iki denetleyicilerinden erişilebilir olması gerekir.
> 
> 

#### <a name="to-install-and-verify-regular-mode-hotfixes"></a>Normal mod düzeltmelerini yüklemek ve doğrulamak için
Normal mod düzeltmelerini yüklemek ve doğrulamak için aşağıdaki adımları gerçekleştirin. İçin bunları Azure Portalı'nı kullanarak zaten yüklü değilse,'ın İleri atlayabilirsiniz [yükleme ve Bakım modu düzeltmeleri doğrulama](#to-install-and-verify-maintenance-mode-hotfixes).

1. Düzeltmeleri yüklemek için StorSimple cihazı seri konsolunuzdaki Windows PowerShell arabirimine erişin. [Seri konsola bağlanmak için PuTTy kullanma](../articles/storsimple/storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console) bölümündeki ayrıntılı yönergeleri izleyin. Komut isteminde **Enter** tuşuna basın.
2. Seçenek 1, seçin **oturum oturum tam erişim**. Düzeltmeyi ilk olarak edilgen denetleyiciye yüklemeniz önerilir.
3. Düzeltmeyi yüklemek için komut istemine şunu yazın:
   
    `Start-HcsHotfix -Path <path to update file> -Credential <credentials in domain\username format>`
   
    Yukarıdaki komuttaki paylaşım yolunda DNS yerine IP kullanın. Kimlik bilgisi parametresi yalnızca kimliği doğrulanmış bir paylaşımdan erişiyorsanız kullanılır.
   
    Paylaşımlara erişmek için kimlik bilgisi parametresini kullanmanız önerilir. “Herkese” açık paylaşımlar bile genellikle kimliği doğrulanmamış kullanıcılara açık değildir.
   
    İstendiğinde parolayı belirtin.
   
    Örnek çıktı aşağıda gösterilmiştir.
   
        ````
        Controller0>Start-HcsHotfix -Path \\10.100.100.100\share
        \hcsmdssoftwareupdate.exe -Credential contoso\John
   
        Confirm
   
        This operation starts the hotfix installation and could reboot one or
        both of the controllers. If the device is serving I/Os, these will not
        be disrupted. Are you sure you want to continue?
        [Y] Yes [N] No [?] Help (default is "Y"): Y
   
        ````
4. Düzeltme yüklemesini onaylamak için sorulduğunda **Y** yazın.
5. `Get-HcsUpdateStatus` cmdlet'ini kullanarak güncelleştirmeyi izleyin. Güncelleştirme ilk olarak edilgen denetleyicide tamamlanır. Edilgen denetleyici güncelleştirildikten sonra yük devretme gerçekleştirilir ve bundan sonra güncelleştirme diğer denetleyiciye uygulanır. Her iki denetleyici de güncelleştirildiğinde güncelleştirme tamamlanır.
   
    Devam etmekte olan güncelleştirme aşağıdaki örnek çıktıda gösterilir. Güncelleştirme devam ederken `RunInprogress` değeri `True` olacaktır.

    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : True
    LastHotfixTimestamp :
    LastUpdateTimestamp : 8/29/2016 2:04:02 AM
    Controller0Events   :
    Controller1Events   :
    ```
   
     Aşağıdaki örnek çıktıda güncelleştirmenin tamamlandığı gösterilir. Güncelleştirme tamamlandığında `RunInProgress` değeri `False` olacaktır.
   
    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : False
    LastHotfixTimestamp : 8/30/2016 9:15:55 AM
    LastUpdateTimestamp : 8/30/2016 9:06:07 AM
    Controller0Events   :
    Controller1Events   :
    ```

    > [!NOTE] 
    > Bazı durumlarda cmdlet, güncelleştirme hala devam ediyorsa `False` raporu gönderir. Düzeltmenin tamamlandığından emin olmak için birkaç dakika bekleyin, bu komutu yeniden çalıştırın ve `RunInProgress` değerinin `False` olduğunu doğrulayın. Değer değiştiyse düzeltme tamamlanmıştır.

1. Yazılım güncelleştirmesi tamamlandıktan sonra sistem yazılımı sürümlerini doğrulayın. Şunu yazın:
   
    `Get-HcsSystem`
   
    Aşağıdaki sürümleri görmeniz gerekir:
   
   * `HcsSoftwareVersion: 6.3.9600.17759`
   * `CisAgentVersion:  1.0.9343.0`
   * `MdsAgentVersion: 30.0.4698.16`
     
     Sürüm numaraları güncelleştirmeyi uyguladıktan sonra değiştirmezseniz düzeltmeyi uygulamak başarısız olduğunu gösterir. Bunu görmeniz durumunda daha fazla yardım için lütfen [Microsoft Desteği](../articles/storsimple/storsimple-contact-microsoft-support.md)’ne başvurun.
     
     > [!IMPORTANT]
     > Kalan güncelleştirmeleri uygulamadan önce etkin denetleyiciyi `Restart-HcsController` cmdlet’i ile yeniden başlatmanız gerekir. 
     > 
     > 
2. LSI sürücü ve bellenim düzeltmeyi yüklemek için 3 ile 5 arasındaki adımları yineleyin **KB3186859**. Düzeltme yüklendikten sonra `Get-HcsSystem` cmdlet'i. LSI sürümü olmalıdır:
   
   * `Lsisas2Version: 2.0.78.00`
3. Storport ve Spaceport güncelleştirmeyi yüklemek için 3 ile 5 arasındaki adımları yineleyin **KB3121261**.
4. Güncelleştirme 2 veya daha önceki bir sürümünden güncelleştiriyorsanız indirmek gerekir:
   
   * iSCSI düzeltme KB3146621 kullanma
   * WMI düzeltme KB3103616 kullanma

#### <a name="to-install-and-verify-maintenance-mode-hotfixes"></a>Bakım modu düzeltmelerini yüklemek ve doğrulamak için
Disk Bellenim güncelleştirmeleri yüklemek için KB3121899 kullanın. Bunlar kesintiye uğratan güncelleştirmelerdir ve tamamlanması yaklaşık 30 dakika sürer. Bunları cihaz seri konsoluna bağlanarak planlı bakım penceresinde yüklemeyi seçebilirsiniz.

Disk üretici yazılımınız zaten güncelse bu güncelleştirmeleri yüklemeniz gerekmez. Güncelleştirmelerin mevcut olup olmadığını ve güncelleştirmelerin kesintiye uğratıp (bakım modu) uğratmayacağını (normal mod) denetlemek için cihaz seri konsolundan `Get-HcsUpdateAvailability` cmdlet’ini çalıştırın.

Disk üretici yazılımı güncelleştirmelerini yüklemek için aşağıdaki yönergeleri izleyin.

1. Cihazın bakım moduna. Windows PowerShell uzaktan iletişimini bakım modundaki bir aygıta bağlanırken kullanmaması gerektiğini unutmayın. Bunun yerine bu cmdlet'i cihaz seri konsol üzerinden bağlandığında aygıt denetleyicisinde çalıştırın. Şunu yazın:
   
    `Enter-HcsMaintenanceMode`
   
    Örnek çıktı aşağıda gösterilmiştir.
   
        Controller0>Enter-HcsMaintenanceMode
        Checking device state...
   
        In maintenance mode, your device will not service IOs and will be disconnected from the Microsoft Azure StorSimple Manager service. Entering maintenance mode will end the current session and reboot both controllers, which takes a few minutes to complete. Are you sure you want to enter maintenance mode?
        [Y] Yes [N] No (Default is "Y"): Y
   
        -----------------------MAINTENANCE MODE------------------------
        Microsoft Azure StorSimple Appliance Model 8100
        Name: Update2-8100-SHG0997879L76673
        Copyright (C) 2014 Microsoft Corporation. All rights reserved.
        You are connected to Controller0 - Passive
        ---------------------------------------------------------------
   
        Serial Console Menu
        [1] Log in with full access
        [2] Log into peer controller with full access
        [3] Connect with limited access
        [4] Change language
        Please enter your choice>
   
    İki denetleyiciye bakım moduna ardından yeniden başlatın.
2. Disk üretici yazılımı güncelleştirmesini yüklemek için şunu yazın:
   
    `Start-HcsHotfix -Path <path to update file> -Credential <credentials in domain\username format>`
   
    Örnek çıktı aşağıda gösterilmiştir.
   
        Controller1>Start-HcsHotfix -Path \\10.100.100.100\share\DiskFirmwarePackage.exe -Credential contoso\john
        Enter Password:
        WARNING: In maintenance mode, hotfixes should be installed on each controller sequentially. After the hotfix is installed on this controller, install it on the peer controller.
        Confirm
        This operation starts a hotfix installation and could reboot one or both of the controllers. By installing new updates you agree to, and accept any additional terms associated with, the new functionality listed in the release notes (https://go.microsoft.com/fwLink/?LinkID=613790). Are you sure you want to continue?
        [Y] Yes [N] No (Default is "Y"): Y
        WARNING: Installation is currently in progress. This operation can take several minutes to complete.
3. `Get-HcsUpdateStatus` komutunu kullanarak yükleme ilerleme durumunu izleyin. `RunInProgress` değeri `False` olarak değiştiğinde güncelleştirme tamamlanır.
4. Yükleme tamamlandıktan sonra, bakım modu düzeltmesinin yüklendiği denetleyici yeniden başlatılır. Seçenek 1, oturum oturum **oturum oturum tam erişim**ve disk bellenim sürümü doğrulayın. Şunu yazın:
   
   `Get-HcsFirmwareVersion`
   
   Beklenen disk üretici yazılımı sürümleri şunlardır:
   
   `XMGG, XGEG, KZ50, F6C2, VR08`
   
   Örnek çıktı aşağıda gösterilmiştir.
   
       -----------------------MAINTENANCE MODE------------------------
       Microsoft Azure StorSimple Appliance Model 8100
       Name: Update2-8100-SHG0997879L76YD
       Software Version: 6.3.9600.17705
       Copyright (C) 2014 Microsoft Corporation. All rights reserved.
       You are connected to Controller1
       ---------------------------------------------------------------
   
       Controller1>Get-HcsFirmwareVersion
   
       Controller0 : TalladegaFirmware
         ActiveBIOS:0.45.0006
         BackupBIOS:0.45.0008
         MainCPLD:17.0.0005
         ActiveBMCRoot:2.0.000E
         BackupBMCRoot:2.0.000E
         BMCBoot:2.0.0001
         LsiFirmware:19.00.00.00
         LsiBios:07.37.00.00
         Battery1Firmware:06.29
         Battery2Firmware:06.29
         DomFirmware:X231600
         CanisterFirmware:3.5.0.32
         CanisterBootloader:5.03
         CanisterConfigCRC:0xD1B030A4
         CanisterVPDStructure:0x06
         CanisterGEMCPLD:0x17
         CanisterVPDCRC:0xEE3504B4
         MidplaneVPDStructure:0x0C
         MidplaneVPDCRC:0xA6BD4F64
         MidplaneCPLD:0x10
         PCM1Firmware:1.00|1.05
         PCM1VPDStructure:0x05
         PCM1VPDCRC:0x41BEF99C
         PCM2Firmware:1.00|1.05
         PCM2VPDStructure:0x05
         PCM2VPDCRC:0x41BEF99C
   
         DisksFirmware
         SEAGATE:ST400FM0073:XGEG
         SEAGATE:ST400FM0073:XGEG
         SEAGATE:ST400FM0073:XGEG
         SEAGATE:ST400FM0073:XGEG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
   
    Yazılım sürümünün güncelleştirildiğinden emin olmak için ikinci denetleyicide `Get-HcsFirmwareVersion` komutunu çalıştırın. Bundan sonra bakım modundan çıkabilirsiniz. Bunu yapmak için, her bir cihaz denetleyicisi için aşağıdaki komutu yazın:
   
   `Exit-HcsMaintenanceMode`
5. Bakım modu çıktığınızda denetleyicilerini yeniden başlatın. Disk üretici yazılımı güncelleştirmeleri başarıyla uygulanıp cihaz bakım modundan çıktıktan sonra, klasik Azure portalına geri dönün. Portal 24 saat için Bakım modu güncelleştirmeleri yüklü gösterilmeyebilir unutmayın.

