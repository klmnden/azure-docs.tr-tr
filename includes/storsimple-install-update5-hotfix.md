---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 19d2dedc2ccf7015696504a94f5ef7c43a90d3be
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66171182"
---
#### <a name="to-download-hotfixes"></a>Düzeltmeleri indirmek için

Microsoft Update Kataloğu'ndan yazılım güncelleştirmesi indirmek için aşağıdaki adımları uygulayın.

1. Internet Explorer'ı başlatın ve gidin [ http://catalog.update.microsoft.com ](https://catalog.update.microsoft.com).
2. Microsoft Update Kataloğu’nu bu bilgisayarda ilk kez kullanıyorsanız, sorulduğunda **Yükle**’ye tıklayarak Microsoft Update Kataloğu eklentisini yükleyin.

    ![Katalog yükleme](./media/storsimple-install-update2-hotfix/HCS_InstallCatalog-include.png)

3. Microsoft Update Kataloğu arama kutusuna girin, örneğin indirmek istediğiniz düzeltmenin Bilgi Bankası (KB) numarasını **4037264**ve ardından **arama**.
   
    Düzeltme listesi görüntülenir, örneğin, **StorSimple 8000 serisi için toplu yazılım paketi güncelleştirme 5.0**.
   
    ![Katalogda arama](./media/storsimple-install-update5-hotfix/update-catalog-search.png)

4. **İndir**'e tıklayın. İndirilen öğelerin görünmesini istediğiniz yerel konumu belirtin veya **Gözat** seçeneğiyle konumu bulun. Dosya belirtilen konuma ve klasöre yüklemek için tıklayın. Klasör, cihazdan erişilebilen bir ağ paylaşımına da kopyalanabilir.
5. Diğer düzeltmeleri arayın, yukarıdaki tabloda listelenen (**4037266**) ve yukarıdaki tabloda listelenen karşılık gelen dosyalar belirli klasörlere yükleyin.

> [!NOTE]
> Düzeltmeleri eş denetleyicisinden hata iletilerinin algılamak için her iki denetleyicilerinin erişilebilir olması gerekir.
>
> Düzeltmeler 3 ayrı klasöre kopyalanmalıdır. Örneğin, cihaz CIS/software/AVH Aracısı güncelleştirmesi içinde kopyalanabilir _FirstOrderUpdate_ klasörü, tüm diğer kesintiye uğratmayan güncelleştirmelerin kopyalanmalıdır _SecondOrderUpdate_ klasöründe ve Bakım modu güncelleştirmeleri, kopyalanan _ThirdOrderUpdate_ klasör.

#### <a name="to-install-and-verify-regular-mode-hotfixes"></a>Normal mod düzeltmelerini yüklemek ve doğrulamak için

Normal mod düzeltmelerini yüklemek ve doğrulamak için aşağıdaki adımları gerçekleştirin. Azure portalını kullanarak bunları zaten yüklü değilse, atlayın [Bakım modu düzeltmelerini yüklemek ve doğrulamak](#to-install-and-verify-maintenance-mode-hotfixes).

1. Düzeltmeleri yüklemek için StorSimple cihazı seri konsolunuzdaki Windows PowerShell arabirimine erişin. [Seri konsola bağlanmak için PuTTy kullanma](../articles/storsimple/storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console) bölümündeki ayrıntılı yönergeleri izleyin. Komut isteminde **Enter** tuşuna basın.
2. 1 seçeneğini **tam erişimle oturum açmak**. Düzeltmeyi ilk olarak edilgen denetleyiciye yüklemeniz önerilir.
3. Düzeltmeyi yüklemek için komut istemine şunu yazın:
   
    `Start-HcsHotfix -Path <path to update file> -Credential <credentials in domain\username format>`
   
    Yukarıdaki komuttaki paylaşım yolunda DNS yerine IP kullanın. Kimlik bilgisi parametresi yalnızca kimliği doğrulanmış bir paylaşımdan erişiyorsanız kullanılır.
   
    Paylaşımlara erişmek için kimlik bilgisi parametresini kullanmanız önerilir. “Herkese” açık paylaşımlar bile genellikle kimliği doğrulanmamış kullanıcılara açık değildir.
   
4. İstendiğinde parolayı belirtin. Birinci sipariş güncelleştirmelerini yüklemeye ilişkin örnek çıktı aşağıda gösterilmiştir. İlk sırada güncelleştirmesi için belirli bir dosyaya işaret edecek şekilde gerekir.

    >[!NOTE] 
    > Yüklemelisiniz _HcsSoftwareUpdate.exe_ ilk. Bu yükleme tamamlandıktan sonra yüklemeyi _CisMdsAgentUpdate.exe_.
   
        ```
        Controller0>Start-HcsHotfix -Path \\10.100.100.100\share
        \FirstOrderUpdate\HcsSoftwareUpdate.exe -Credential contoso\John
   
        Confirm
   
        This operation starts the hotfix installation and could reboot one or
        both of the controllers. If the device is serving I/Os, these will not
        be disrupted. Are you sure you want to continue?
        [Y] Yes [N] No [?] Help (default is "Y"): Y
   
        ```
5. Düzeltme yüklemesini onaylamak için sorulduğunda **Y** yazın.
6. `Get-HcsUpdateStatus` cmdlet'ini kullanarak güncelleştirmeyi izleyin. Güncelleştirme ilk olarak edilgen denetleyicide tamamlanır. Edilgen denetleyici güncelleştirildikten sonra yük devretme gerçekleştirilir ve bundan sonra güncelleştirme diğer denetleyiciye uygulanır. Her iki denetleyici de güncelleştirildiğinde güncelleştirme tamamlanır.
   
    Devam etmekte olan güncelleştirme aşağıdaki örnek çıktıda gösterilir. `RunInprogress` Olduğu `True` güncelleştirme sürüyor olduğunda.

    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : True
    LastHotfixTimestamp :
    LastUpdateTimestamp : 07/28/2017 2:04:02 AM
    Controller0Events   :
    Controller1Events   :
    ```
   
     Aşağıdaki örnek çıktıda güncelleştirmenin tamamlandığı gösterilir. `RunInProgress` Olduğu `False` güncelleştirme tamamlandığında.
   
    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : False
    LastHotfixTimestamp : 07/28/2017 9:15:55 AM
    LastUpdateTimestamp : 07/28/2017 9:06:07 AM
    Controller0Events   :
    Controller1Events   :
    ```

    > [!NOTE]
    > Bazı durumlarda cmdlet, güncelleştirme hala devam ediyorsa `False` raporu gönderir. Düzeltmenin tamamlandığından emin olmak için birkaç dakika bekleyin, bu komutu yeniden çalıştırın ve `RunInProgress` değerinin `False` olduğunu doğrulayın. Değer değiştiyse düzeltme tamamlanmıştır.

7. Yazılım güncelleştirmesi tamamlandıktan sonra sistem yazılımı sürümlerini doğrulayın. Tür:
   
    `Get-HcsSystem`
   
    Aşağıdaki sürümleri görmeniz gerekir:
   
   * `FriendlySoftwareVersion: StorSimple 8000 Series Update 5.0`
   * `HcsSoftwareVersion: 6.3.9600.17845`
   
     Güncelleştirme uygulandıktan sonra sürüm numarası değişmezse, düzeltmenin uygulanamadığı anlamına gelir. Bunu görmeniz durumunda daha fazla yardım için lütfen [Microsoft Desteği](../articles/storsimple/storsimple-8000-contact-microsoft-support.md)’ne başvurun.
     
     > [!IMPORTANT]
     > Etkin denetleyiciyi aracılığıyla yeniden başlatmalısınız `Restart-HcsController` İleri güncelleştirmeyi uygulamadan önce cmdlet'i.
     
8. Yüklemek için 3 ile 6 arasındaki adımları yineleyin _CisMDSAgentupdate.exe_ aracıyı indirdiğiniz, _FirstOrderUpdate_ klasör.
8. İkinci sipariş güncelleştirmelerini yüklemek için 3-6. adımları tekrarlayın. 

    > [!NOTE] 
    > İkinci sipariş güncelleştirmelerini yalnızca çalıştırarak birden fazla güncelleştirme yüklenebilir `Start-HcsHotfix cmdlet` ve ikinci sipariş güncelleştirmelerini yerleştirildiği klasör işaret. Cmdlet, klasörde bulunan tüm güncelleştirmeleri yürütür. Bir güncelleştirme zaten yüklüyse, güncelleştirme mantığı bunu saptar ve ilgili güncelleştirmeyi uygulamaz.

    Tüm düzeltmeler yüklendikten sonra `Get-HcsSystem` cmdlet'ini kullanın. Sürümler şunlar olmalıdır:
    
    * `CisAgentVersion:  1.0.9724.0`
    * `MdsAgentVersion: 35.2.2.0`
    * `Lsisas2Version: 2.0.78.00`


#### <a name="to-install-and-verify-maintenance-mode-hotfixes"></a>Bakım modu düzeltmelerini yüklemek ve doğrulamak için

Disk üretici yazılımı güncelleştirmelerini yüklemek için KB4037263 kullanın. Bunlar kesintiye uğratan güncelleştirmelerdir ve tamamlanması yaklaşık 30 dakika sürer. Bunları cihaz seri konsoluna bağlanarak planlı bakım penceresinde yüklemeyi seçebilirsiniz.

> [!NOTE] 
> Disk üretici yazılımınız zaten güncelse bu güncelleştirmeleri yüklemeniz gerekmez. Güncelleştirmelerin mevcut olup olmadığını ve güncelleştirmelerin kesintiye uğratıp (bakım modu) uğratmayacağını (normal mod) denetlemek için cihaz seri konsolundan `Get-HcsUpdateAvailability` cmdlet’ini çalıştırın.

Disk üretici yazılımı güncelleştirmelerini yüklemek için aşağıdaki yönergeleri izleyin.

1. Cihazı bakım moduna alın. 

    > [!NOTE] 
    > Bakım modunda bir cihaza bağlanırken Windows PowerShell uzaktan iletişimini kullanmayın. Bunun yerine cihaz denetleyicisinde cihaz seri konsolu üzerinden bağlanırken bu cmdlet'i çalıştırın.

    Denetleyici bakım moduna almak için şunu yazın:
   
    `Enter-HcsMaintenanceMode`
   
    Örnek çıktı aşağıda gösterilmiştir.
   
        Controller0>Enter-HcsMaintenanceMode
        Checking device state...
   
        In maintenance mode, your device will not service IOs and will be disconnected from the Microsoft Azure StorSimple Manager service. Entering maintenance mode will end the current session and reboot both controllers, which takes a few minutes to complete. Are you sure you want to enter maintenance mode?
        [Y] Yes [N] No (Default is "Y"): Y
   
        -----------------------MAINTENANCE MODE------------------------
        Microsoft Azure StorSimple Appliance Model 8600
        Name: Update4-8600-mystorsimple
        Copyright (C) 2014 Microsoft Corporation. All rights reserved.
        You are connected to Controller0 - Passive
        ---------------------------------------------------------------
   
        Serial Console Menu
        [1] Log in with full access
        [2] Log into peer controller with full access
        [3] Connect with limited access
        [4] Change language
        Please enter your choice>
   
    Bu durumda her iki denetleyici de bakım modunda yeniden başlatılır.
2. Disk üretici yazılımı güncelleştirmesini yüklemek için şunu yazın:
   
    `Start-HcsHotfix -Path <path to update file> -Credential <credentials in domain\username format>`
   
    Örnek çıktı aşağıda gösterilmiştir.
   
        Controller1>Start-HcsHotfix -Path \\10.100.100.100\share\ThirdOrderUpdates\ -Credential contoso\john
        Enter Password:
        WARNING: In maintenance mode, hotfixes should be installed on each controller sequentially. After the hotfix is installed on this controller, install it on the peer controller.
        Confirm
        This operation starts a hotfix installation and could reboot one or both of the controllers. By installing new updates you agree to, and accept any additional terms associated with, the new functionality listed in the release notes (https://go.microsoft.com/fwLink/?LinkID=613790). Are you sure you want to continue?
        [Y] Yes [N] No (Default is "Y"): Y
        WARNING: Installation is currently in progress. This operation can take several minutes to complete.
3. `Get-HcsUpdateStatus` komutunu kullanarak yükleme ilerleme durumunu izleyin. `RunInProgress` değeri `False` olarak değiştiğinde güncelleştirme tamamlanır.
4. Yükleme tamamlandıktan sonra, bakım modu düzeltmesinin yüklendiği denetleyici yeniden başlatılır. Seçenek 1, buna oturum **tam erişimle oturum açmak**ve disk üretici yazılımı sürümünü doğrulayın. Tür:
   
   `Get-HcsFirmwareVersion`
   
   Beklenen disk üretici yazılımı sürümleri şunlardır:
   
   `XMGJ, XGEG, KZ50, F6C2, VR08, N003, 0107`
   
   Örnek çıktı aşağıda gösterilmiştir.
   
       -----------------------MAINTENANCE MODE------------------------
       Microsoft Azure StorSimple Appliance Model 8600
       Name: Update4-8600-mystorsimple
       Software Version: 6.3.9600.17845
       Copyright (C) 2014 Microsoft Corporation. All rights reserved.
       You are connected to Controller1
       ---------------------------------------------------------------
   
       Controller1>Get-HcsFirmwareVersion
   
       Controller0 : TalladegaFirmware
           ActiveBIOS:0.45.0010
              BackupBIOS:0.45.0006
              MainCPLD:17.0.000b
              ActiveBMCRoot:2.0.001F
              BackupBMCRoot:2.0.001F
              BMCBoot:2.0.0002
              LsiFirmware:20.00.04.00
              LsiBios:07.37.00.00
              Battery1Firmware:06.2C
              Battery2Firmware:06.2C
              DomFirmware:X231600
              CanisterFirmware:3.5.0.56
              CanisterBootloader:5.03
              CanisterConfigCRC:0x9134777A
              CanisterVPDStructure:0x06
              CanisterGEMCPLD:0x19
              CanisterVPDCRC:0x142F7DC2
              MidplaneVPDStructure:0x0C
              MidplaneVPDCRC:0xA6BD4F64
              MidplaneCPLD:0x10
              PCM1Firmware:1.00|1.05
              PCM1VPDStructure:0x05
              PCM1VPDCRC:0x41BEF99C
              PCM2Firmware:1.00|1.05
              PCM2VPDStructure:0x05
              PCM2VPDCRC:0x41BEF99C

           EbodFirmware
              CanisterFirmware:3.5.0.56
              CanisterBootloader:5.03
              CanisterConfigCRC:0xB23150F8
              CanisterVPDStructure:0x06
              CanisterGEMCPLD:0x14
              CanisterVPDCRC:0xBAA55828
              MidplaneVPDStructure:0x0C
              MidplaneVPDCRC:0xA6BD4F64
              MidplaneCPLD:0x10
              PCM1Firmware:3.11
              PCM1VPDStructure:0x03
              PCM1VPDCRC:0x6B58AD13
              PCM2Firmware:3.11
              PCM2VPDStructure:0x03
              PCM2VPDCRC:0x6B58AD13

           DisksFirmware
              SmrtStor:TXA2D20800GA6XYR:KZ50
              SmrtStor:TXA2D20800GA6XYR:KZ50
              SmrtStor:TXA2D20800GA6XYR:KZ50
              SmrtStor:TXA2D20800GA6XYR:KZ50
              SmrtStor:TXA2D20800GA6XYR:KZ50
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
   
    Yazılım sürümünün güncelleştirildiğinden emin olmak için ikinci denetleyicide `Get-HcsFirmwareVersion` komutunu çalıştırın. Bundan sonra bakım modundan çıkabilirsiniz. Bunu yapmak için, her bir cihaz denetleyicisi için aşağıdaki komutu yazın:
   
   `Exit-HcsMaintenanceMode`

5. Bakım modundan çıktığınızda denetleyiciler yeniden başlatılır. Sonra disk üretici yazılımı güncelleştirmeleri başarıyla uygulanıp ve cihaz, Bakım modu, Azure portalına geri dönün çıkıldı. Yüklediğiniz bakım modu güncelleştirmeleri, 24 saat boyunca portalda gösterilmeyebilir.

