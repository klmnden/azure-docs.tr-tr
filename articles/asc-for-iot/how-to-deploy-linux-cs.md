---
title: Kılavuzu yüklemek ve Linux'ı dağıtmak için C# IOT Önizleme için Azure Güvenlik Merkezi Aracısı | Microsoft Docs
description: IOT aracı için Azure Güvenlik Merkezi hem 32-bit hem de 64-bit Linux üzerinde yüklemeyi öğrenin.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: b0982203-c3c8-4a0b-8717-5b5ac4038d8c
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2019
ms.author: mlottner
ms.openlocfilehash: 808ff912a997a4c09a22048ada7546daab895701
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67618259"
---
# <a name="deploy-azure-security-center-for-iot-c-based-security-agent-for-linux"></a>IOT için Azure Güvenlik Merkezi dağıtma C#-Linux için aracıyı güvenlik tabanlı

> [!IMPORTANT]
> IOT için Azure Güvenlik Merkezi şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bu kılavuz, yüklemek ve IOT için Azure Güvenlik Merkezi (ASC) dağıtmak açıklanmaktadır C#-Linux güvenlik aracı tabanlı.

Bu kılavuzda şunların nasıl yapıldığını öğrenirsiniz: 
> [!div class="checklist"]
> * Yükleme
> * Dağıtımı doğrulama
> * Aracıyı kaldırma
> * Sorun giderme 

## <a name="prerequisites"></a>Önkoşullar

Diğer platformlar ve aracı özellikleri için bkz. [doğru güvenlik aracı seçin](how-to-deploy-agent.md).

1. Güvenlik aracısı dağıtmak için yüklemek istediğiniz makinede yerel yönetici hakları gereklidir. 

1. [Bir güvenlik modülünüzü oluşturmak](quickstart-create-security-twin.md) aygıt için.

## <a name="installation"></a>Yükleme 

Güvenlik aracısını dağıtmak için aşağıdakileri yapın:

1. Makinenizden en son sürümü indirin [Github](https://aka.ms/iot-security-github-cs).

1. Paket içeriğini ayıklayın ve gidin _/Install_ klasör.

1. Çalışan izinleri ekleyin **InstallSecurityAgent betik** çalıştırarak `chmod +x InstallSecurityAgent.sh` 

1. Ardından çalıştırın: 

   ```
   ./InstallSecurityAgent.sh -i -aui <authentication identity>  -aum <authentication method> -f <file path> -hn <host name>  -di <device id> -cl <certificate location kind>
   ```
   
   Bkz: [kimlik doğrulamasını yapılandırma](concept-security-agent-authentication-methods.md) kimlik doğrulama parametreleri hakkında daha fazla bilgi.

Bu betik şunları yapar:

- Önkoşulları yükler.

- Hizmet kullanıcısı (devre dışı etkileşimli oturum açma ile) ekler.

- Aracı olarak yükleyen bir **arka plan programı** -bu cihaz varsayar **systemd** hizmet yönetimi için.

- Yapılandırır **sudoers** kök olarak bazı görevleri gerçekleştirmek üzere aracı izin vermek için.

- Aracı belirtilen kimlik doğrulaması parametrelerle yapılandırır.


Ek Yardım için komut dosyasını çalıştırmak help parametresini ile: `./InstallSecurityAgent.sh --help`

### <a name="uninstall-the-agent"></a>Aracıyı kaldırma

Aracıyı kaldırmak için – u parametresiyle betiği çalıştırın: `./InstallSecurityAgent.sh -u`. 

> [!NOTE]
> Kaldırma yükleme sırasında yüklenen tüm eksik önkoşulları kaldırmaz.

## <a name="troubleshooting"></a>Sorun giderme  

1. Dağıtım durumunu denetleyin:

    `systemctl status ASCIoTAgent.service`

2. Günlük kaydını etkinleştirin.  
   Aracısı'nı başlatmak başarısız olursa daha fazla bilgi için günlük özelliğini açar.

   İle oturum açın:

   1. Yapılandırma dosyasını herhangi bir Linux düzenleyicisinde düzenlemek üzere açın:

        `vi /var/ASCIoTAgent/General.config`

   1. Aşağıdaki değerleri düzenleyin: 

      ```
      <add key="logLevel" value="Debug"/>
      <add key="fileLogLevel" value="Debug"/>
      <add key="diagnosticVerbosityLevel" value="Some" /> 
      <add key="logFilePath" value="IotAgentLog.log"/>
      ```
       **LogFilePath** değerdir yapılandırılabilir. 

       > [!NOTE]
       > Günlüğe kaydetme kapatma öneririz **kapalı** sorun giderme işlemi tamamlandıktan sonra. Günlüğe kaydetme bırakarak **üzerinde** arttıkça günlük dosyasının boyutunu ve veri kullanımı.

   1. Çalıştırarak aracıyı yeniden başlatın:

       `systemctl restart ASCIoTAgent.service`

   1. Hata hakkında daha fazla bilgi için günlük dosyasını görüntüleyin.  

       Günlük dosyası konumu şudur: `/var/ASCIoTAgent/IotAgentLog.log`

       Dosya konumu yolu için seçtiğiniz adına göre değiştirmek **logFilePath** 2. adımda. 

## <a name="next-steps"></a>Sonraki adımlar

- ASC IOT hizmeti için okuma [genel bakış](overview.md)
- ASC hakkında daha fazla bilgi edinmek için IOT [mimarisi](architecture.md)
- Etkinleştirme [hizmeti](quickstart-onboard-iot-hub.md)
- Okuma [SSS](resources-frequently-asked-questions.md)
- Anlamak [uyarıları](concept-security-alerts.md)