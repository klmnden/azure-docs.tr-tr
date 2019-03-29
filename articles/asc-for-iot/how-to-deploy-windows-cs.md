---
title: ASC Windows yüklemesi için IOT aracı Önizleme | Microsoft Docs
description: ASC IOT aracının 32-bit veya 64-bit Windows cihazlarında yükleme hakkında bilgi edinin.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: 2cf6a49b-5d35-491f-abc3-63ec24eb4bc2
ms.service: ascforiot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/19/2019
ms.author: mlottner
ms.openlocfilehash: 5c3293746fcc52570e708fd4bfab446981d49c24
ms.sourcegitcommit: f8c592ebaad4a5fc45710dadc0e5c4480d122d6f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58621847"
---
# <a name="deploy-an-asc-for-iot-c-based-security-agent-for-windows"></a>IOT için bir ASC dağıtma C#-güvenlik aracı için Windows tabanlı

> [!IMPORTANT]
> ASC IOT için şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bu kılavuz için IOT ASC yükleneceği açıklanmaktadır C#-Windows Güvenlik aracısında bağlı.

Bu kılavuzda şunların nasıl yapıldığını öğrenirsiniz: 
> [!div class="checklist"]
> * Yükleme
> * Dağıtımı doğrulama
> * Aracıyı kaldırma
> * Sorun giderme 

## <a name="prerequisites"></a>Önkoşullar

Diğer platformlar ve aracı flavours için [doğru güvenlik aracı seçin](how-to-deploy-agent.md).

1. Yüklemek istediğiniz makinede yerel yönetici hakları. 

1. [Bir güvenlik modülünüzü oluşturmak](quickstart-create-security-twin.md) aygıt için.

## <a name="installation"></a>Yükleme 

Güvenlik aracı yüklemek için aşağıdakileri yapın:

1. ASC IOT Windows için yüklenecek C# aracı cihaz üzerinde indirme en son sürümü makinenize ASC ile IOT [GitHub deposu](https://github.com/Azure/Azure-IoT-Security-Agent-CS).

2. Paket içeriğini ayıklayın ve/Install klasöre gidin.

3. Windows PowerShell'i yönetici olarak açın. 
    1. Çalışan izinleri çalıştırarak InstallSecurityAgent komut dosyasına ekleyin. ```Unblock-File .\InstallSecurityAgent.ps1```
    
        ve çalıştırın:

    ```
    .\InstallSecurityAgent.ps1 -Install -aui <authentication identity> -aum <authentication method> -f <file path> -hn <host name> -di <device id> -cl <certificate location kind>
    ```
    
    Örneğin:
    
    ```
    .\InstallSecurityAgent.ps1 -Install -aui Device -aum SymmetricKey -f c:\Temp\Key.txt -hn MyIotHub.azure-devices.net -di Mydevice1 -cl store
    ```
    
    Bkz: [kimlik doğrulamasını yapılandırma](concept-security-agent-authentication-methods.md) kimlik doğrulama parametreleri hakkında daha fazla bilgi.

Bu betik şunları yapar:

- Önkoşulları yükler.

- Hizmet kullanıcısı (devre dışı etkileşimli oturum açma ile) ekler.

- Aracı olarak yükleyen bir **sistem hizmeti**.

- Aracı belirtilen kimlik doğrulaması parametrelerle yapılandırır.


Daha fazla bilgi için Get-Help komutu PowerShell'de kullanın. <br>Get-Help örnek:  
    ```Get-Help .\InstallSecurityAgent.ps1```

### <a name="verify-deployment-status"></a>Dağıtım durumunu doğrulayın

- Aracı dağıtım durumunu denetleyin:<br>
    ```sc.exe query "ASC IoT Agent" ```

### <a name="uninstall-the-agent"></a>Aracıyı kaldırma

Aracıyı kaldırmak için:

1. Aşağıdaki PowerShell komut dosyasını Çalıştır **-modu** parametresini **kaldırma**.  

    ```
    .\InstallSecurityAgent.ps1 -Uninstall
    ``` 

## <a name="troubleshooting"></a>Sorun giderme

Aracısı'nı başlatmak başarısız olursa, günlük özelliğini açar (günlüğe kaydetme işlemi *kapalı* varsayılan olarak) daha fazla bilgi için.

Günlüğü etkinleştirmek için:

1. Bir standart dosya Düzenleyicisi'ni kullanarak düzenlemek için yapılandırma dosyası (General.config) açın.

1. Aşağıdaki değerleri düzenleyin:

   ```xml
   <add key="logLevel" value="Debug" />
   <add key="fileLogLevel" value="Debug"/> 
   <add key="diagnosticVerbosityLevel" value="Some" /> 
   <add key="logFilePath" value="IoTAgentLog.log" />
   ```

    > [!NOTE]
    > Günlüğe kaydetme kapatma öneririz **kapalı** sorun giderme işlemi tamamlandıktan sonra. Günlüğe kaydetme bırakarak **üzerinde** arttıkça günlük dosyasının boyutunu ve veri kullanımı. 

1. Aşağıdaki PowerShell veya komut satırı'nı çalıştırarak aracıyı yeniden başlatın:

    **PowerShell**
     ```
     Restart-Service "ASC IoT Agent"
     ```
     
   or

    **CMD**
     ```
     sc.exe stop "ASC IoT Agent" 
     sc.exe start "ASC IoT Agent" 
     ```

1. Hata hakkında daha fazla bilgi için günlük dosyasını gözden geçirin.

   Günlük dosyası konumu: `%WinDir%/System32/IoTAgentLog.log`


## <a name="next-steps"></a>Sonraki adımlar
- ASC IOT hizmeti için okuma [genel bakış](overview.md)
- ASC hakkında daha fazla bilgi edinmek için IOT [mimarisi](architecture.md)
- Etkinleştirme [hizmeti](quickstart-onboard-iot-hub.md)
- Okuma [SSS](resources-frequently-asked-questions.md)
- Anlamak [uyarıları](concept-security-alerts.md)