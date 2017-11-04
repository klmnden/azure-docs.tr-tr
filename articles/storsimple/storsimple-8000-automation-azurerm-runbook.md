---
title: "StorSimple cihazları yönetmek için Azure Otomasyonu Runbook'u kullanma | Microsoft Docs"
description: "Azure Otomasyonu Runbook'u StorSimple işleri otomatikleştirmek için nasıl kullanılacağını öğrenin"
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 10/23/2017
ms.author: alkohli
ms.openlocfilehash: cfd0e4dbb6a4f24df5ba42cd45f9c16fbe5b493c
ms.sourcegitcommit: e6029b2994fa5ba82d0ac72b264879c3484e3dd0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2017
---
# <a name="use-azure-automation-runbooks-to-manage-storsimple-devices"></a>StorSimple cihazları yönetmek için Azure Otomasyonu runbook'ları kullanma

Bu makalede, Azure Otomasyon çalışma kitabı StorSimple 8000 serisi Cihazınızı Azure portalında yönetmek için nasıl kullanıldığını açıklar. Bu runbook yürütmek ortamınızı yapılandırma adımları boyunca size yol için bir örnek runbook dahildir.


## <a name="configure-add-and-run-azure-runbook"></a>Yapılandırma, eklemek ve Azure runbook çalıştırma

Bu bölümde, StorSimple için Windows PowerShell komut dosyası örneği alır ve komut dosyasını bir runbook'a nasıl aktarılacağı ve ardından yayımlama ve runbook yürütmek için gereken çeşitli adımlarını ayrıntılı şekilde açıklar.

### <a name="prerequisites"></a>Ön koşullar

Başlamadan önce şunları yapın:

* StorSimple cihaz Yöneticisi hizmetiniz ile ilişkili etkin bir Azure aboneliğiniz StorSimple 8000 serisi aygıtla kayıtlı.


* Windows PowerShell, bilgisayarınızda yüklü 5.0 (ya da Windows Server barındırması için StorSimple birini kullanıyorsanız).


### <a name="create-automation-runbook-module-in-windows-powershell"></a>Windows PowerShell'de Otomasyon runbook modülü oluşturun

StorSimple 8000 serisi aygıt yönetimi için bir otomasyon modülü oluşturmak için aşağıdaki adımları gerçekleştirin:

1. Başlatma Windows PowerShell. Yeni bir klasör oluşturun ve yeni klasöre dizini değiştirin.

    ```
        mkdir C:\scripts\StorSimpleSDKTools
        cd C:\scripts\StorSimpleSDKTools
    ```    
2. [NuGet CLI karşıdan](http://www.nuget.org/downloads) önceki adımda oluşturduğunuz klasörü altında. Çeşitli sürümü vardır _nuget.exe_. SDK'sına karşılık gelen sürümünü seçin. Her karşıdan yükleme bağlantısı doğrudan işaret eden bir _.exe_ dosya. Sağ tıklayın ve dosyayı bilgisayarınıza kaydedin emin yerine tarayıcıdan çalışıyor.

    Ayrıca, karşıdan yüklemek ve betik daha önce oluşturduğunuz aynı klasörde depolamak için aşağıdaki komutu çalıştırabilirsiniz.
    
    ```
        wget https://dist.nuget.org/win-x86-commandline/latest/nuget.exe -Out C:\scripts\StorSimpleSDKTools\nuget.exe
    ```
3. Bağımlı SDK'sını indirin.

    ```
        C:\scripts\StorSimpleSDKTools\nuget.exe install Microsoft.Azure.Management.Storsimple8000series
        C:\scripts\StorSimpleSDKTools\nuget.exe install Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.28.3
        C:\scripts\StorSimpleSDKTools\nuget.exe install Microsoft.Rest.ClientRuntime.Azure.Authentication -Version 2.2.9-preview
    ```    
4. Komut dosyası örneği GitHub projeden indirin.

    ```
        wget https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Monitor-Backups.ps1 -Out Monitor-Backups.ps1

    ```
5. StorSimple 8000 serisi aygıt yönetimi için bir Azure Otomasyonu Runbook modülü oluşturun. Windows Powershell penceresinde aşağıdaki komutları yazın:

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

6. Bir Otomasyon modül zip dosyası oluşturulur doğrulayın `C:\scripts\StorSimpleSDKTools`.

    ![doğrulama-otomasyon-modülü](./media/storsimple-8000-automation-azurerm-runbook/verify-automation-module.png)

7. Windows PowerShell ile automation modülü oluşturulduğunda aşağıdaki çıkış sunulur. 

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

### <a name="import-publish-and-run-automation-runbook"></a>İçeri aktarma, yayımlama ve Otomasyon runbook çalıştırma

1. Azure portalında bir Azure farklı çalıştır Otomasyon hesabı oluşturun. Bunu yapmak için şu adrese gidin **Azure Market > her şeyi** arayın ve sonra **Otomasyon**. Seçin **Automation hesapları**.

    ![Arama Otomasyon](./media/storsimple-8000-automation-azurerm-runbook/automation1.png)

2. İçinde **Automation hesabı Ekle** dikey penceresinde:

    1. Tedarik **adı** Otomasyon hesabınızın.
    2. Seçin **abonelik** StorSimple cihaz Yöneticisi hizmetinize bağlı.
    3. Yeni bir kaynak grubu oluşturun veya varolan bir kaynak grubu seçin.
    4. Seçin bir **konumu** (mümkünse hizmetinizi çalıştığı aynı).
    5. Varsayılan adı bırakın **Çalıştır hesabı oluştur** seçeneği belirlenmiş.
    5. İsteğe bağlı olarak kontrol **panoya Sabitle**. **Oluştur**'a tıklayın.

        ![Otomasyon-hesap oluştur](./media/storsimple-8000-automation-azurerm-runbook/create-automation-account.png)

    Otomasyon hesabı başarıyla oluşturulduktan sonra size bildirilir. Bir Otomasyon hesabı oluşturma hakkında daha fazla bilgi için Git [bir farklı çalıştır hesabı oluşturma](https://docs.microsoft.com/azure/automation/automation-create-runas-account).

3. Oluşturduğunuz automation hesabını StorSimple cihaz Yöneticisi hizmeti erişebildiğinden emin olmak için Otomasyon hesabı için uygun izinleri atamanız gerekir. Git **erişim denetimi** StorSimple Aygıt Yöneticisi'ni hizmetinizde. Tıklatın **+ Ekle** ve Azure Otomasyon hesabınızın adını sağlayın. **Kaydet** ayarlar.

    ![izinleri-automation-hesabı Ekle](./media/storsimple-8000-automation-azurerm-runbook/goto-add-roles.png)

4. Yeni oluşturulan hesabında Git **paylaşılan kaynakları > modülleri** tıklatıp **+ Ekle Modülü**.

5. İçinde **Ekle Modülü** dikey penceresinde, daraltılmış modülü konumuna göz atın ve seçin ve modülü açın. **Tamam** düğmesine tıklayın.

    ![Modül Ekle](./media/storsimple-8000-automation-azurerm-runbook/add-module.png)

6. Git **işlem Otomasyonu > runbook'ları ve tıklatın + runbook Ekle**. İçinde **runbook Ekle** dikey penceresinde tıklatın **mevcut bir runbook'u içeri aktar**. Windows PowerShell komut dosyası için noktasına **Runbook dosyası**. Runbook türü otomatik olarak seçilir. Bir ad ve runbook için isteğe bağlı bir açıklama sağlayın. **Oluştur**'a tıklayın.

    ![Modül Ekle](./media/storsimple-8000-automation-azurerm-runbook/import-runbook.png)

7. Runbook, runbook'ları listesine eklenir. Seçin ve bu runbook'ı tıklatın.

    ![tıklatın yeni-runbook](./media/storsimple-8000-automation-azurerm-runbook/verify-runbook-created.png)

8. Runbook'u düzenlemek ve tıklayın **Test bölmesi**. StorSimple cihaz Yöneticisi hizmeti, StorSimple cihazı ve abonelik adını adı gibi parametreler belirtin. **Başlat** test. Rapor Çalıştır tamamlandığında oluşturulur. Daha fazla bilgi için Git [bir runbook'u test etme](../automation/automation-first-runbook-textual-powershell.md#step-3---test-the-runbook).

    ![runbook sınaması](./media/storsimple-8000-automation-azurerm-runbook/test-runbook.png)

9. Test Bölmesi'nda runbook'un çıkışı inceleyin. Karşılanan bölmesini kapatın. Tıklatın **Yayımla** ve onaylamanız istendiğinde, onaylayın ve runbook yayımlayabilirsiniz.

    ![runbook yayımlama](./media/storsimple-8000-automation-azurerm-runbook/publish-runbook.png)


## <a name="next-steps"></a>Sonraki adımlar

[StorSimple Cihazınızı yönetmek için Aygıt Yöneticisi'ni StorSimple hizmeti](storsimple-8000-manager-service-administration.md).