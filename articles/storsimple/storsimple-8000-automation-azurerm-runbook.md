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
ms.openlocfilehash: 93c77b5f678c4e6b3170d2c7612bef3f104f0b6b
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2019
ms.locfileid: "58002603"
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

    ```
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

    ```
            # set path variables
            $downloadDir = "C:\scripts\StorSimpleSDKTools"
            $moduleDir = "$downloadDir\AutomationModule\Microsoft.Azure.Management.StorSimple8000Series"

            #don't change the folder name "Microsoft.Azure.Management.StorSimple8000Series"
            mkdir "$moduleDir"
            copy "$downloadDir\Microsoft.IdentityModel.Clients.ActiveDirectory.2.28.3\lib\net45\Microsoft.IdentityModel.Clients.ActiveDirectory*.dll" $moduleDir
            copy "$downloadDir\Microsoft.Rest.ClientRuntime.Azure.3.3.7\lib\net452\Microsoft.Rest.ClientRuntime.Azure*.dll" $moduleDir
            copy "$downloadDir\Microsoft.Rest.ClientRuntime.2.3.8\lib\net452\Microsoft.Rest.ClientRuntime*.dll" $moduleDir
            copy "$downloadDir\Newtonsoft.Json.6.0.8\lib\net45\Newtonsoft.Json*.dll" $moduleDir
            copy "$downloadDir\Microsoft.Rest.ClientRuntime.Azure.Authentication.2.2.9-preview\lib\net45\Microsoft.Rest.ClientRuntime.Azure.Authentication*.dll" $moduleDir
            copy "$downloadDir\Microsoft.Azure.Management.Storsimple8000series.1.0.0\lib\net452\Microsoft.Azure.Management.Storsimple8000series*.dll" $moduleDir

            #Don't change the name of the Archive
            compress-Archive -Path "$moduleDir" -DestinationPath Microsoft.Azure.Management.StorSimple8000Series.zip

    ```

6. Bir Otomasyon modül zip dosyası oluşturulduğunu doğrulayın `C:\scripts\StorSimpleSDKTools`.

    ![doğrulama-otomasyon-modülü](./media/storsimple-8000-automation-azurerm-runbook/verify-automation-module.png)

7. Otomasyon modülü, Windows PowerShell oluşturulduğunda aşağıdaki çıkış sunulur. 

    ```
    -----------------------------------------
    Windows PowerShell
    Copyright (C) 2016 Microsoft Corporation. All rights reserved.

    PS C:\WINDOWS\system32> mkdir C:\scripts\StorSimpleSDKTools

        Directory: C:\scripts

    Mode                LastWriteTime         Length Name
    ----                -------------         ------ ----
    d-----       10/18/2017   8:43 AM                StorSimpleSDKTools


    PS C:\WINDOWS\system32> cd c:\scripts\StorSimpleSDKTools
    PS C:\scripts\StorSimpleSDKTools> wget https://dist.nuget.org/win-x86-commandline/latest/nuget.exe -Out C:\scripts\StorS
    impleSDKTools\nuget.exe
    PS C:\scripts\StorSimpleSDKTools> C:\scripts\StorSimpleSDKTools\nuget.exe install Microsoft.Azure.Management.Storsimple8
    000series

    -------------------------------------------
    CUT              CUT  
    -------------------------------------------
    Successfully installed 'Microsoft.Azure.Management.Storsimple8000series 1.0.0' to C:\scripts\StorSimpleSDKTools
    Executing nuget actions took 1.77 sec
    PS C:\scripts\StorSimpleSDKTools> C:\scripts\StorSimpleSDKTools\nuget.exe install Microsoft.IdentityModel.Clients.Active
    Directory -Version 2.28.3

    -------------------------------------------
    CUT              CUT  
    -------------------------------------------
    Successfully installed 'Microsoft.IdentityModel.Clients.ActiveDirectory 2.28.3' to C:\scripts\StorSimpleSDKTools
    Executing nuget actions took 927.64 ms
    PS C:\scripts\StorSimpleSDKTools> C:\scripts\StorSimpleSDKTools\nuget.exe install Microsoft.Rest.ClientRuntime.Azure.Aut
    hentication -Version 2.2.9-preview

    -------------------------------------------
    CUT              CUT  
    -------------------------------------------
    Successfully installed 'Microsoft.Rest.ClientRuntime.Azure.Authentication 2.2.9-preview' to C:\scripts\StorSimpleSDKTool
    s
    Executing nuget actions took 717.48 ms
    PS C:\scripts\StorSimpleSDKTools> wget https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Mo
    nitor-Backups.ps1 -Out Monitor-Backups.ps1
    PS C:\scripts\StorSimpleSDKTools> # set path variables
    PS C:\scripts\StorSimpleSDKTools>             $downloadDir = "C:\scripts\StorSimpleSDKTools"
    PS C:\scripts\StorSimpleSDKTools>             $moduleDir = "$downloadDir\AutomationModule\Microsoft.Azure.Management.Sto
    rSimple8000Series"
    PS C:\scripts\StorSimpleSDKTools>
    PS C:\scripts\StorSimpleSDKTools>             #don't change the folder name "Microsoft.Azure.Management.StorSimple8000Se
    ries"
    PS C:\scripts\StorSimpleSDKTools>             mkdir "$moduleDir"

        Directory: C:\scripts\StorSimpleSDKTools\AutomationModule


    Mode                LastWriteTime         Length Name
    ----                -------------         ------ ----
    d-----       10/18/2017   8:48 AM                Microsoft.Azure.Management.StorSimple8000Series

    PS C:\scripts\StorSimpleSDKTools>             copy "$downloadDir\Microsoft.IdentityModel.Clients.ActiveDirectory.2.28.3\
    lib\net45\Microsoft.IdentityModel.Clients.ActiveDirectory*.dll" $moduleDir
    PS C:\scripts\StorSimpleSDKTools>             copy "$downloadDir\Microsoft.Rest.ClientRuntime.Azure.3.3.7\lib\net452\Mic
    rosoft.Rest.ClientRuntime.Azure*.dll" $moduleDir
    PS C:\scripts\StorSimpleSDKTools>             copy "$downloadDir\Microsoft.Rest.ClientRuntime.2.3.8\lib\net452\Microsoft
    .Rest.ClientRuntime*.dll" $moduleDir
    PS C:\scripts\StorSimpleSDKTools>             copy "$downloadDir\Newtonsoft.Json.6.0.8\lib\net45\Newtonsoft.Json*.dll" $
    moduleDir
    PS C:\scripts\StorSimpleSDKTools>             copy "$downloadDir\Microsoft.Rest.ClientRuntime.Azure.Authentication.2.2.9
    -preview\lib\net45\Microsoft.Rest.ClientRuntime.Azure.Authentication*.dll" $moduleDir
    PS C:\scripts\StorSimpleSDKTools>             copy "$downloadDir\Microsoft.Azure.Management.Storsimple8000series.1.0.0\l
    ib\net452\Microsoft.Azure.Management.Storsimple8000series*.dll" $moduleDir
    PS C:\scripts\StorSimpleSDKTools>
    PS C:\scripts\StorSimpleSDKTools>             #Don't change the name of the Archive
    PS C:\scripts\StorSimpleSDKTools>             compress-Archive -Path "$moduleDir" -DestinationPath Microsoft.Azure.Manag
    ement.StorSimple8000Series.zip
    PS C:\scripts\StorSimpleSDKTools>

    -------------------------------------------

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
   5. İsteğe bağlı olarak denetleme **panoya Sabitle**. **Oluştur**’a tıklayın.

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