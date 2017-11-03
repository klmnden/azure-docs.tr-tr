<!--author=SharS last changed: 03/17/2016-->

#### <a name="to-download-hotfixes"></a>Düzeltmeleri indirmek için
Yazılım güncelleştirme karşıdan yüklemek için aşağıdaki adımları gerçekleştirin.

1. Internet Explorer'ı başlatın ve [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com) adresine gidin.
2. Microsoft Update Kataloğu’nu bu bilgisayarda ilk kez kullanıyorsanız, sorulduğunda **Yükle**’ye tıklayarak Microsoft Update Kataloğu eklentisini yükleyin.
    ![Katalog yükleyin](./media/storsimple-install-update-option-1/HCS_InstallCatalog-include.png)
3. Microsoft Update Kataloğu arama kutusuna, örneğin, yüklemek istediğiniz düzeltme Bilgi Bankası (KB) sayısını girin **3063418**ve ardından **arama**.
4. Göreceğiniz **StorSimple güncelleştirme 1.2 Gereci güncelleştirme** paket. **Ekle**'ye tıklayın. Güncelleştirme sepete eklenir.
5. Ek düzeltmeleri arayın yukarıdaki tabloda listelenen (**3043005** ve **3063416**) ve her Sepete ekleyin.
6. **Sepeti Görüntüle**’ye tıklayın.
   
    ![Sepeti görüntüle](./media/storsimple-install-update-option-1/HCS_InstallBasket-include.png)
7. **İndir**’e tıklayın. İndirilen öğelerin görünmesini istediğiniz yerel konumu belirtin veya **Gözat** seçeneğiyle konumu bulun. Güncelleştirmeler belirtilen konuma indirilir ve güncelleştirme ile aynı adı taşıyan alt klasöre yerleştirilir. Klasör, cihazdan erişilebilen bir ağ paylaşımına da kopyalanabilir.

> [!NOTE]
> Düzeltmeleri tüm olası hata iletilerini eş denetleyicisinden algılamak için her iki denetleyicilerinden erişilebilir olması gerekir.
> 
> 

#### <a name="to-install-and-verify-regular-mode-hotfixes"></a>Normal mod düzeltmelerini yüklemek ve doğrulamak için
Yükleme ve normal modu düzeltmeleri doğrulamak için aşağıdaki adımları gerçekleştirin. İçin bunları Azure Portalı'nı kullanarak zaten yüklü değilse,'ın İleri atlayabilirsiniz [yükleme ve Bakım modu düzeltmeleri doğrulama](#to-install-and-verify-maintenance-mode-hotfixes).

1. Yazılım güncelleştirmesini yüklemek için Windows PowerShell arabirimi StorSimple cihaz seri konsoluna erişin. [Seri konsola bağlanmak için PuTTy kullanma](../articles/storsimple/storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console) bölümündeki ayrıntılı yönergeleri izleyin. Komut isteminde **Enter** tuşuna basın.
2. Cihazda tam erişimle oturum açmak için **Seçenek 1**’i belirleyin.
3. Komut isteminde güncelleştirme paketini yüklemek için aşağıdakileri yazın:
   
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
    LastHotfixTimestamp : 9/02/2015 10:36:13 PM
    LastUpdateTimestamp : 9/02/2015 10:35:25 PM
    Controller0Events   :
    Controller1Events   :
    ```
   
     Aşağıdaki örnek çıktıda güncelleştirmenin tamamlandığı gösterilir. Güncelleştirme tamamlandığında `RunInProgress` değeri `False` olacaktır.

    ```
    Controller1>Get-HcsUpdateStatus

    RunInprogress       : False
    LastHotfixTimestamp : 9/02/2015 10:56:13 PM
    LastUpdateTimestamp : 9/02/2015 10:35:25 PM
    Controller0Events   :
    Controller1Events   :
    ```
   
   > [!NOTE]
   > Bazı durumlarda cmdlet, güncelleştirme hala devam ediyorsa `False` raporu gönderir. Düzeltmenin tamamlandığından emin olmak için birkaç dakika bekleyin, bu komutu yeniden çalıştırın ve `RunInProgress` değerinin `False` olduğunu doğrulayın. Değer değiştiyse düzeltme tamamlanmıştır.
   > 
   > 
6. Yazılım güncelleştirmesi tamamlandıktan sonra sistem yazılımı sürümlerini doğrulayın. Aşağıdaki komutu yazın:
   
    `Get-HcsSystem`
   
    Aşağıdaki sürümleri görmeniz gerekir:
   
   * HcsSoftwareVersion: 6.3.9600.17584
   * CisAgentVersion: 1.0.9049.0
   * MdsAgentVersion: 26.0.4696.1433
     
     Sürüm numaraları güncelleştirmeyi uyguladıktan sonra değiştirmezseniz düzeltmeyi uygulamak başarısız olduğunu gösterir. Bunu görmeniz durumunda daha fazla yardım için lütfen [Microsoft Desteği](../articles/storsimple/storsimple-contact-microsoft-support.md)’ne başvurun.
7. Kalan normal modu düzeltme (KB3043005) yüklemek için 3-5 adımlarını tekrarlayın.

#### <a name="to-install-and-verify-maintenance-mode-hotfixes"></a>Bakım modu düzeltmelerini yüklemek ve doğrulamak için
Disk Bellenim güncelleştirmeleri yüklemek için KB3063416 kullanın. Bunlar kesintiye uğratan güncelleştirmeleri ve tamamlanması yaklaşık 30-45 dakika sürer. Bunları cihaz seri konsoluna bağlanarak planlı bakım penceresinde yüklemeyi seçebilirsiniz.

Disk üretici yazılımı güncelleştirmelerini yüklemek için aşağıdaki yönergeleri izleyin.

1. Cihazın bakım moduna. Windows PowerShell uzaktan iletişimini bakım modundaki bir aygıta bağlanırken kullanmaması gerektiğini unutmayın. Bu cmdlet cihaz seri Konsolu aracılığıyla bağlandığında aygıt denetleyicisinde çalıştırmanız gerekir. Şunu yazın:
   
    `Enter-HcsMaintenanceMode`
   
    Örnek çıktı aşağıda gösterilmiştir.
   
        Controller0>Enter-HcsMaintenanceMode
        Checking device state...
   
        In maintenance mode, your device will not service IOs and will be disconnected from the Microsoft Azure StorSimple Manager service. Entering maintenance mode will end the current session and reboot both controllers, which takes a few minutes to complete. Are you sure you want to enter maintenance mode?
        [Y] Yes [N] No (Default is "Y"): Y
   
        -----------------------MAINTENANCE MODE------------------------
        Microsoft Azure StorSimple Appliance Model 8100
        Name: Update1-8100-SHG0997879L76YD
        Software Version: 6.3.9600.17584
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
        This operation starts a hotfix installation and could reboot one or both of the controllers. Are you sure you want to continue?
        [Y] Yes [N] No (Default is "Y"): Y
        WARNING: Installation is currently in progress. This operation can take several minutes to complete.
3. `Get-HcsUpdateStatus` komutunu kullanarak yükleme ilerleme durumunu izleyin. `RunInProgress` değeri `False` olarak değiştiğinde güncelleştirme tamamlanır.
4. Yükleme tamamlandıktan sonra Bakım modu düzeltme yüklü olduğu denetleyicisi yeniden başlatılacak. Tam erişimle seçenek 1 olarak oturum açın ve disk üretici yazılımı sürümünü doğrulayın. Şunu yazın:
   
   `Get-HcsFirmwareVersion`
   
   Beklenen disk üretici yazılımı sürümleri şunlardır:
   
   `XMGG, XGEE, KZ50, F6C2, VR08`
   
   Yazılım sürümünün güncelleştirildiğinden emin olmak için ikinci denetleyicide `Get-HcsFirmwareVersion` komutunu çalıştırın. Bundan sonra bakım modundan çıkabilirsiniz. Her aygıt denetleyicisi için aşağıdaki komutu yazın:
   
   `Exit-HcsMaintenanceMode`
5. Bakım modu çıktığınızda denetleyicilerini yeniden başlatın. Disk üretici yazılımı güncelleştirmeleri başarıyla uygulanıp cihaz bakım modundan çıktıktan sonra, klasik Azure portalına geri dönün. Portal 24 saat için Bakım modu güncelleştirmeleri yüklü gösterilmeyebilir unutmayın.

