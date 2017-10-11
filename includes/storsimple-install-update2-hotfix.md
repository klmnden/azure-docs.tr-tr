<!--author=alkohli last changed: 03/17/16-->

#### <a name="to-download-hotfixes"></a>Düzeltmeleri indirmek için
Microsoft Update Kataloğu'ndan yazılım güncelleştirmesi indirmek için aşağıdaki adımları uygulayın.

1. Internet Explorer'ı başlatın ve [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com) adresine gidin.
2. Microsoft Update Kataloğu’nu bu bilgisayarda ilk kez kullanıyorsanız, sorulduğunda **Yükle**’ye tıklayarak Microsoft Update Kataloğu eklentisini yükleyin.
    ![Katalog yükleyin](./media/storsimple-install-update2-hotfix/HCS_InstallCatalog-include.png)
3. Microsoft Update Kataloğu arama kutusuna, örneğin, yüklemek istediğiniz düzeltme Bilgi Bankası (KB) sayısını girin **3121901**ve ardından **arama**.
   
    Düzeltme listesi göründüğünde, örneğin, **StorSimple 8000 serisi için toplu yazılım paketini güncelleştirme 2.0**.
   
    ![Katalogda arama](./media/storsimple-install-update2-hotfix/HCS_SearchCatalog1-include.png)
4. **Ekle**'ye tıklayın. Güncelleştirme sepete eklenir.
5. Ek düzeltmeleri arayın yukarıdaki tabloda listelenen (**3121900**, **3080728**, **3090322**, ve **3121899**) ve her ekleyin Sepeti.
6. **Sepeti Görüntüle**’ye tıklayın.
7. **İndir**’e tıklayın. İndirilen öğelerin görünmesini istediğiniz yerel konumu belirtin veya **Gözat** seçeneğiyle konumu bulun. Güncelleştirmeler belirtilen konuma indirilir ve güncelleştirme ile aynı adı taşıyan alt klasöre yerleştirilir. Klasör, cihazdan erişilebilen bir ağ paylaşımına da kopyalanabilir.

> [!NOTE]
> Düzeltmeleri tüm olası hata iletilerini eş denetleyicisinden algılamak için her iki denetleyicilerinden erişilebilir olması gerekir.
> 
> 

#### <a name="to-install-and-verify-regular-mode-hotfixes"></a>Normal mod düzeltmelerini yüklemek ve doğrulamak için
Normal mod düzeltmelerini yüklemek ve doğrulamak için aşağıdaki adımları gerçekleştirin. İçin bunları Azure Portalı'nı kullanarak zaten yüklü değilse,'ın İleri atlayabilirsiniz [yükleme ve Bakım modu düzeltmeleri doğrulama](#to-install-and-verify-maintenance-mode-hotfixes).

1. Düzeltmeleri yüklemek için StorSimple cihazı seri konsolunuzdaki Windows PowerShell arabirimine erişin. [Seri konsola bağlanmak için PuTTy kullanma](../articles/storsimple/storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console) bölümündeki ayrıntılı yönergeleri izleyin. Komut isteminde **Enter** tuşuna basın.
2. Cihazda tam erişimle oturum açmak için **Seçenek 1**’i belirleyin.
3. Düzeltmeyi yüklemek için komut istemine şunu yazın:
   
    `Start-HcsHotfix -Path <path to update file> -Credential <credentials in domain\username format>`
   
    Yukarıdaki komuttaki paylaşım yolunda DNS yerine IP kullanın. Kimlik bilgisi parametresi yalnızca kimliği doğrulanmış bir paylaşımdan erişiyorsanız kullanılır.
   
    Paylaşımlara erişmek için kimlik bilgisi parametresini kullanmanız önerilir. “Herkese” açık paylaşımlar bile genellikle kimliği doğrulanmamış kullanıcılara açık değildir.
   
    Örnek çıktı aşağıda gösterilmiştir.
   
    ```
    Controller0>Start-HcsHotfix -Path \\10.100.100.100\share
    \hcsmdssoftwareupdate.exe -Credential contoso\John

    Confirm

    This operation starts the hotfix installation and could reboot one or
    both of the controllers. If the device is serving I/Os, these will not
    be disrupted. Are you sure you want to continue?
    [Y] Yes [N] No [?] Help (default is "Y"): Y
    ```
4. Düzeltme yüklemesini onaylamak için sorulduğunda **Y** yazın.
5. `Get-HcsUpdateStatus` cmdlet'ini kullanarak güncelleştirmeyi izleyin.
   
    Devam etmekte olan güncelleştirme aşağıdaki örnek çıktıda gösterilir. Güncelleştirme devam ederken `RunInprogress` değeri `True` olacaktır.
   
    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : True
    LastHotfixTimestamp : 12/21/2015 10:36:13 PM
    LastUpdateTimestamp : 12/21/2015 10:35:25 PM
    Controller0Events   :
    Controller1Events   :
    ```
   
     Aşağıdaki örnek çıktıda güncelleştirmenin tamamlandığı gösterilir. Güncelleştirme tamamlandığında `RunInProgress` değeri `False` olacaktır.
   
    ```
    Controller1>Get-HcsUpdateStatus

    RunInprogress       : False
    LastHotfixTimestamp : 12/21/2015 10:59:13 PM
    LastUpdateTimestamp : 12/21/2015 10:35:25 PM
    Controller0Events   :
    Controller1Events   :
    ```
   
   > [!NOTE]
   > Bazı durumlarda cmdlet, güncelleştirme hala devam ediyorsa `False` raporu gönderir. Düzeltmenin tamamlandığından emin olmak için birkaç dakika bekleyin, bu komutu yeniden çalıştırın ve `RunInProgress` değerinin `False` olduğunu doğrulayın. Değer değiştiyse düzeltme tamamlanmıştır.

6. Yazılım güncelleştirmesi tamamlandıktan sonra adım 3-5, yüklemek ve SaaS aracısı ve MDS Aracısı izlemek için yineleyin. Emin `all-hcsmdssoftwareupdate_0b438ddf0d5b686aada2378b754fac8c7f2160e9.exe` önce yüklü `all-cismdsagentupdatebundle_f98e62f4d56c79e2a6644d027af7a2393a93827a.exe`.
7. Sistem yazılım sürümleri doğrulayın. Şunu yazın:
   
    `Get-HcsSystem`
   
    Aşağıdaki sürümleri görmeniz gerekir:
   
   * HcsSoftwareVersion: 6.3.9600.17673
   * CisAgentVersion: 1.0.9150.0
   * MdsAgentVersion: 30.0.4698.13
     
     Sürüm numaraları güncelleştirmeyi uyguladıktan sonra değiştirmezseniz düzeltmeyi uygulamak başarısız olduğunu gösterir. Bunu görmeniz durumunda daha fazla yardım için lütfen [Microsoft Desteği](../articles/storsimple/storsimple-contact-microsoft-support.md)’ne başvurun.
8. Kalan normal modu düzeltmeleri yüklemek için 3-5 adımlarını tekrarlayın.
   
   * LSI sürücüsü - KB3121900
   * Storport güncelleştirmesi - KB3080728
   * Spaceport güncelleştirmesi - KB3090322

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
        Name: Update2-8100-SHG0997879L76YD
        Software Version: 6.3.9600.17664
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
4. Yükleme tamamlandıktan sonra, bakım modu düzeltmesinin yüklendiği denetleyici yeniden başlatılır. Tam erişimle seçenek 1 olarak oturum açın ve disk üretici yazılımı sürümünü doğrulayın. Şunu yazın:
   
   `Get-HcsFirmwareVersion`
   
   Beklenen disk üretici yazılımı sürümleri şunlardır:
   
   `XMGG, XGEG, KZ50, F6C2, VR08`
   
   Örnek çıktı aşağıda gösterilmiştir.
   
       -----------------------MAINTENANCE MODE------------------------
       Microsoft Azure StorSimple Appliance Model 8100
       Name: Update2-8100-SHG0997879L76YD
       Software Version: 6.3.9600.17664
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

