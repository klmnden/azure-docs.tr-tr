---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 03/06/2019
ms.author: alkohli
ms.openlocfilehash: 348f7bdd333da4f4a6cb41a438b7aee08d6a6bbb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66161180"
---
İstemci işletim sistemine bağlı olarak cihaza uzaktan bağlanmak için yordamlar farklıdır.

### <a name="remotely-connect-from-a-windows-client"></a>Bir Windows istemcisinde uzaktan bağlanma

Başlamadan önce Windows İstemcisi Windows PowerShell 5.0 veya üzeri çalıştırıldığından emin olun.

Bir Windows istemcisinden uzaktan bağlanmak için aşağıdaki adımları izleyin.

1. Bir Windows PowerShell oturumu yönetici olarak çalıştırın.
2. Windows Uzaktan Yönetim hizmeti, istemci üzerinde çalıştığından emin olun. Komut istemine şunları yazın:

    `winrm quickconfig`

3. Bir değişken için cihazı IP adresi atayın.

    $ip = "< device_ip >"

    Değiştirin `<device_ip>` cihazınızın IP adresine sahip.

4. Cihazınızın IP adresini istemcinin güvenilir konak listesine eklemek için aşağıdaki komutu yazın:

    `Set-Item WSMan:\localhost\Client\TrustedHosts $ip -Concatenate -Force`

5. Cihazda bir Windows PowerShell oturumu başlatın:

    `Enter-PSSession -ComputerName $ip -Credential $ip\EdgeUser -ConfigurationName Minishell`

6. İstendiğinde parolayı belirtin. Yerel web kullanıcı Arabirimi ile imzalamak için kullanılan aynı şifreyi kullanın. Varsayılan yerel web kullanıcı Arabirimi parola *Password1*. Uzak PowerShell kullanarak cihaza başarıyla bağlandığınızda, şu örnek çıktıyı görürsünüz:  

    ```
    Windows PowerShell
    Copyright (C) Microsoft Corporation. All rights reserved.
    
    PS C:\WINDOWS\system32> winrm quickconfig
    WinRM service is already running on this machine.
    PS C:\WINDOWS\system32> $ip = "10.100.10.10"
    PS C:\WINDOWS\system32> Set-Item WSMan:\localhost\Client\TrustedHosts $ip -Concatenate -Force
    PS C:\WINDOWS\system32> Enter-PSSession -ComputerName $ip -Credential $ip\EdgeUser -ConfigurationName Minishell

    WARNING: The Windows PowerShell interface of your device is intended to be used only for the initial network configuration. Please engage Microsoft Support if you need to access this interface to troubleshoot any potential issues you may be experiencing. Changes made through this interface without involving Microsoft Support could result in an unsupported configuration.
    [10.100.10.10]: PS>
    ```

### <a name="remotely-connect-from-a-linux-client"></a>Bir Linux istemcisinden uzaktan bağlanma

Bağlanmak için kullandığınız Linux istemcide:

- [Linux için en son PowerShell Core yükleme](https://docs.microsoft.com/powershell/scripting/install/installing-powershell-core-on-linux?view=powershell-6) SSH uzaktan iletişim özelliğini Al github'dan. 
- [Yalnızca yükleme `gss-ntlmssp` NTLM modülü paketinden](https://github.com/Microsoft/omi/blob/master/Unix/doc/setup-ntlm-omi.md). Ubuntu istemciler için aşağıdaki komutu kullanın:
    - `sudo apt-get install gss-ntlmssp`

Daha fazla bilgi için Git [SSH üzerinden PowerShell uzaktan iletişimi](https://docs.microsoft.com/powershell/scripting/learn/remoting/ssh-remoting-in-powershell-core?view=powershell-6).

Uzaktan bir NFS istemcisinden bağlanmak için aşağıdaki adımları izleyin.

1. PowerShell oturumu açmak için şunu yazın:

    `sudo pwsh`
 
2. Uzak istemci kullanarak bağlanmak için aşağıdakileri yazın:

    `Enter-PSSession -ComputerName $ip -Authentication Negotiate -ConfigurationName Minishell -Credential ~\EdgeUser`

    İstendiğinde, cihazınızın içine imzalamak için kullanılan parolayı belirtin.
 
> [!NOTE]
> Bu yordam, Mac işletim sisteminde çalışmaz.
