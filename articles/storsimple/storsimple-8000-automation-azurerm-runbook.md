---
title: StorSimple cihazları yönetmek için Azure Otomasyonu Runbook'u kullanma | Microsoft Docs
description: Azure Otomasyonu Runbook'u StorSimple işleri otomatikleştirmek için kullanmayı öğrenin
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 10/23/2017
ms.author: alkohli
ms.openlocfilehash: 30d70bb7e1f868060e3b287a0cdfb117c585b9ba
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59790085"
---
# <a name="use-azure-automation-runbooks-to-manage-storsimple-devices"></a>StorSimple cihazları yönetmek için Azure Otomasyonu runbook'ları kullanma

Bu makalede, Azure Otomasyonu runbook'ları Azure portalında StorSimple 8000 serisi Cihazınızı yönetmek için nasıl kullanıldığını açıklar. Örnek runbook'un bu runbook'u çalıştırmak için ortamınızı yapılandırma adımları uygulamak için dahil edilir.


## <a name="configure-add-and-run-azure-runbook"></a>Yapılandırma, ekleyin ve Azure runbook çalıştırma

Bu bölümde, StorSimple için Windows PowerShell Betiği örneği alır ve betiği bir runbook'a nasıl aktarılacağı ve daha sonra runbook'u çalıştırmak için gereken çeşitli adımlarını ayrıntılı şekilde açıklar.

### <a name="prerequisites"></a>Önkoşullar

Başlamadan önce şunları yapın:

* StorSimple cihaz Yöneticisi hizmetiniz ile ilişkili etkin bir Azure aboneliğine sahip bir StorSimple 8000 serisi cihaz kaydedilir.

* Bilgisayarınızda yüklü olan Windows PowerShell 5.0 (veya Windows Server kullanarak StorSimple'ınızı barındırıyorsa).

### <a name="create-automation-runbook-module-in-windows-powershell"></a>Windows PowerShell'de Automation runbook modülü oluşturma

StorSimple 8000 serisi cihaz yönetimi için bir otomasyon modülü oluşturmak için aşağıdaki adımları gerçekleştirin:

1. Başlatma Windows PowerShell. Yeni bir klasör oluşturun ve yeni klasörüne dizin değiştirin.

    ```powershell
        mkdir C:\scripts\StorSimpleSDKTools
        cd C:\scripts\StorSimpleSDKTools
    ```

2. [NuGet CLI'yı indirme](https://www.nuget.org/downloads) önceki adımda oluşturduğunuz klasöre altında. Çeşitli sürümleri vardır _nuget.exe_. SDK'nızı için karşılık gelen sürümü seçin. Her bir indirme bağlantısı doğrudan işaret eden bir _.exe_ dosya. Sağ tıklayın ve dosyayı bilgisayarınıza kaydetmek mutlaka yerine tarayıcıda çalışıyor.

    Ayrıca, karşıdan yüklemek ve betik, daha önce oluşturduğunuz aynı klasöre depolamak için aşağıdaki komutu çalıştırabilirsiniz.

    ```
        wget https://dist.nuget.org/win-x86-commandline/latest/nuget.exe -Out C:\scripts\StorSimpleSDKTools\nuget.exe
    ```

3. Bağımlı SDK'sını indirin.

    ```
        C:\scripts\StorSimpleSDKTools\nuget.exe install Microsoft.Azure.Management.Storsimple8000series
        C:\scripts\StorSimpleSDKTools\nuget.exe install Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.28.3
        C:\scripts\StorSimpleSDKTools\nuget.exe install Microsoft.Rest.ClientRuntime.Azure.Authentication -Version 2.2.9-preview
    ```

4. Betik örnek GitHub projesinden indirmeniz gerekir.

    ```
        wget https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Monitor-Backups.ps1 -Out Monitor-Backups.ps1
    ```

5. StorSimple 8000 serisi cihaz yönetimi için bir Azure Otomasyonu Runbook modülünü oluşturun. Windows Powershell penceresinde aşağıdaki komutları yazın:

    ```powershell
        # set path variables
        $downloadDir = "C:\scripts\StorSimpleSDKTools"
        $moduleDir = "$downloadDir\AutomationModule\Microsoft.Azure.Management.StorSimple8000Series"

        #don't change the folder name "Microsoft.Azure.Management.StorSimple8000Series"
        mkdir "$moduleDir"
        Copy-Item "$downloadDir\Microsoft.IdentityModel.Clients.ActiveDirectory.2.28.3\lib\net45\Microsoft.IdentityModel.Clients.ActiveDirectory*.dll" $moduleDir
        Copy-Item "$downloadDir\Microsoft.Rest.ClientRuntime.Azure.3.3.7\lib\net452\Microsoft.Rest.ClientRuntime.Azure*.dll" $moduleDir
        Copy-Item "$downloadDir\Microsoft.Rest.ClientRuntime.2.3.8\lib\net452\Microsoft.Rest.ClientRuntime*.dll" $moduleDir
        Copy-Item "$downloadDir\Newtonsoft.Json.6.0.8\lib\net45\Newtonsoft.Json*.dll" $moduleDir
        Copy-Item "$downloadDir\Microsoft.Rest.ClientRuntime.Azure.Authentication.2.2.9-preview\lib\net45\Microsoft.Rest.ClientRuntime.Azure.Authentication*.dll" $moduleDir
        Copy-Item "$downloadDir\Microsoft.Azure.Management.Storsimple8000series.1.0.0\lib\net452\Microsoft.Azure.Management.Storsimple8000series*.dll" $moduleDir

        #Don't change the name of the Archive
        compress-Archive -Path "$moduleDir" -DestinationPath Microsoft.Azure.Management.StorSimple8000Series.zip
    ```

6. Bir Otomasyon modül zip dosyası oluşturulduğunu doğrulayın `C:\scripts\StorSimpleSDKTools`.

    ![doğrulama-otomasyon-modülü](./media/storsimple-8000-automation-azurerm-runbook/verify-automation-module.png)

7. Otomasyon modülü, Windows PowerShell oluşturulduğunda aşağıdaki çıkış sunulur.

    ```powershell
    mkdir C:\scripts\StorSimpleSDKTools
    ```

    ```Output
        Directory: C:\scripts

    Mode                LastWriteTime         Length Name
    ----                -------------         ------ ----
    d-----       10/18/2017   8:43 AM                StorSimpleSDKTools
    ```

    ```powershell
    wget https://dist.nuget.org/win-x86-commandline/latest/nuget.exe -Out C:\scripts\StorSimpleSDKTools\nuget.exe
    ```

    ```powershell
    C:\scripts\StorSimpleSDKTools\nuget.exe install Microsoft.Azure.Management.Storsimple8000series
    ```

    ```Output
    -------------------------------------------
    CUT              CUT
    -------------------------------------------
    Successfully installed 'Microsoft.Azure.Management.Storsimple8000series 1.0.0' to C:\scripts\StorSimpleSDKTools
    Executing nuget actions took 1.77 sec
    ```

    ```powershell
    C:\scripts\StorSimpleSDKTools\nuget.exe install Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.28.3
    ```

    ```Output
    -------------------------------------------
    CUT              CUT
    -------------------------------------------
    Successfully installed 'Microsoft.IdentityModel.Clients.ActiveDirectory 2.28.3' to C:\scripts\StorSimpleSDKTools
    Executing nuget actions took 927.64 ms
    ```

    ```powershell
    C:\scripts\StorSimpleSDKTools\nuget.exe install Microsoft.Rest.ClientRuntime.Azure.Authentication -Version 2.2.9-preview
    ```

    ```Output
    -------------------------------------------
    CUT              CUT
    -------------------------------------------
    Successfully installed 'Microsoft.Rest.ClientRuntime.Azure.Authentication 2.2.9-preview' to C:\scripts\StorSimpleSDKTools
    Executing nuget actions took 717.48 ms
    ```

    ```powershell
    wget https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Monitor-Backups.ps1 -Out Monitor-Backups.ps1
    # set path variables
    $downloadDir = "C:\scripts\StorSimpleSDKTools"
    $moduleDir = "$downloadDir\AutomationModule\Microsoft.Azure.Management.StorSimple8000Series"
    #don't change the folder name "Microsoft.Azure.Management.StorSimple8000Series"
    mkdir "$moduleDir"
    ```

    ```Output
        Directory: C:\scripts\StorSimpleSDKTools\AutomationModule

    Mode                LastWriteTime         Length Name
    ----                -------------         ------ ----
    d-----       10/18/2017   8:48 AM                Microsoft.Azure.Management.StorSimple8000Series
    ```

    ```powershell
    Copy-Item "$downloadDir\Microsoft.IdentityModel.Clients.ActiveDirectory.2.28.3\lib\net45\Microsoft.IdentityModel.Clients.ActiveDirectory*.dll" $moduleDir
    Copy-Item "$downloadDir\Microsoft.Rest.ClientRuntime.Azure.3.3.7\lib\net452\Microsoft.Rest.ClientRuntime.Azure*.dll" $moduleDir
    Copy-Item "$downloadDir\Microsoft.Rest.ClientRuntime.2.3.8\lib\net452\Microsoft.Rest.ClientRuntime*.dll" $moduleDir
    Copy-Item "$downloadDir\Newtonsoft.Json.6.0.8\lib\net45\Newtonsoft.Json*.dll" $moduleDir
    Copy-Item "$downloadDir\Microsoft.Rest.ClientRuntime.Azure.Authentication.2.2.9-preview\lib\net45\Microsoft.Rest.ClientRuntime.Azure.Authentication*.dll" $moduleDir
    Copy-Item "$downloadDir\Microsoft.Azure.Management.Storsimple8000series.1.0.0\lib\net452\Microsoft.Azure.Management.Storsimple8000series*.dll" $moduleDir
    #Don't change the name of the Archive
    compress-Archive -Path "$moduleDir" -DestinationPath Microsoft.Azure.Management.StorSimple8000Series.zip
    ```

### <a name="import-publish-and-run-automation-runbook"></a>İçeri aktarma, yayınlama ve Otomasyon runbook'u çalıştırma

1. Azure portalında bir Azure farklı çalıştır Otomasyon hesabı oluşturun. Bunu yapmak için Git **Azure Market > her şeyi** bulun **Otomasyon**. Seçin **Otomasyon hesapları**.

    ![Arama Otomasyon](./media/storsimple-8000-automation-azurerm-runbook/automation1.png)

2. İçinde **Otomasyon hesabı Ekle** dikey penceresinde:

   1. Tedarik **adı** Otomasyon hesabınızın.
   2. Seçin **abonelik** StorSimple cihaz Yöneticisi hizmetinize bağlı.
   3. Yeni bir kaynak grubu oluşturun veya mevcut bir kaynak grubundan'ı seçin.
   4. Seçin bir **konumu** (Eğer Mümkünse, hizmetinize çalıştığı aynı).
   5. Varsayılan değeri bırakın **Çalıştır hesabı oluştur** seçeneği belirlenmiş.
   6. İsteğe bağlı olarak denetleme **panoya Sabitle**. **Oluştur**’a tıklayın.

       ![Otomasyon-hesabı oluştur](./media/storsimple-8000-automation-azurerm-runbook/create-automation-account.png)

      Otomasyon hesabı başarıyla oluşturulduktan sonra size bildirilir. Bir Otomasyon hesabı oluşturma hakkında daha fazla bilgi için Git [bir farklı çalıştır hesabı oluşturma](https://docs.microsoft.com/azure/automation/automation-create-runas-account).

3. Oluşturduğunuz Otomasyon hesabını StorSimple cihaz Yöneticisi hizmeti erişebildiğinden emin olmak için Otomasyon hesabınıza uygun izinleri atamanız gerekir. Git **erişim denetimi** StorSimple cihaz Yöneticisi hizmetinizde. Tıklayın **+ Ekle** ve Azure Otomasyonu hesabınızı adını sağlayın. **Kaydet** ayarlar.

    ![izinleri-Otomasyon-hesabı Ekle](./media/storsimple-8000-automation-azurerm-runbook/goto-add-roles.png)

4. Yeni oluşturulan hesabın Git **paylaşılan kaynaklar > modülleri** tıklatıp **+ Ekle Modülü**.

5. İçinde **Ekle Modülü** dikey penceresinde, sıkıştırılmış modülünün konumuna göz atın ve seçin ve modülün açın. **Tamam** düğmesine tıklayın.

    ![Modül Ekle](./media/storsimple-8000-automation-azurerm-runbook/add-module.png)

6. Git **süreç otomasyonu > runbook'ları ve tıklayın + runbook Ekle**. İçinde **runbook Ekle** dikey penceresinde tıklayın **mevcut bir runbook'u içeri aktar**. İçin Windows PowerShell komut dosyasını işaret **Runbook dosyası**. Runbook türü otomatik olarak seçilir. Bir ad ve runbook için isteğe bağlı bir açıklama sağlayın. **Oluştur**’a tıklayın.

    ![Modül Ekle](./media/storsimple-8000-automation-azurerm-runbook/import-runbook.png)

7. Runbook, runbook'ları listesine eklenir. Seçin ve bu runbook'ı tıklatın.

    ![Click-yeni-runbook](./media/storsimple-8000-automation-azurerm-runbook/verify-runbook-created.png)

8. Runbook'u düzenleyebilir ve **Test bölmesi**. StorSimple cihaz Yöneticisi hizmeti, StorSimple cihazı ve aboneliğin adını adı gibi parametreler belirtin. **Başlangıç** test. Çalıştırma tamamlandığında rapor oluşturulur. Daha fazla bilgi için Git [bir runbook'u test etme](../automation/automation-first-runbook-textual-powershell.md#step-3---test-the-runbook).

    ![Test-runbook](./media/storsimple-8000-automation-azurerm-runbook/test-runbook.png)

9. Runbook'u test bölmesi çıktısı inceleyin. Memnun bölmesini kapatın. Tıklayın **Yayımla** ve onaylamanız istendiğinde, onaylayın ve runbook'u yayımlayamadı.

    ![publish-runbook](./media/storsimple-8000-automation-azurerm-runbook/publish-runbook.png)

## <a name="next-steps"></a>Sonraki adımlar

[StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi'ni kullanın hizmet](storsimple-8000-manager-service-administration.md).
