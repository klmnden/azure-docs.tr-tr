---
title: Kılavuzu yüklemek ve IOT aracı Önizleme için Azure Güvenlik Merkezi'nin Linux C Aracısı dağıtmak için | Microsoft Docs
description: IOT aracı için Azure Güvenlik Merkezi hem 32-bit hem de 64-bit Linux üzerinde yüklemeyi öğrenin.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: 3ccf2aec-106a-4d2c-8079-5f3e8f2afdcb
ms.service: ascforiot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/28/2019
ms.author: mlottner
ms.openlocfilehash: 147813ae096114b4dfc1a20d2e0a70639aa82445
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58754447"
---
# <a name="deploy-azure-security-center-for-iot-c-based-security-agent-for-linux"></a>Linux için aracıyı IOT C tabanlı güvenlik için Azure Güvenlik Merkezi dağıtma

> [!IMPORTANT]
> IOT için Azure Güvenlik Merkezi şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bu kılavuz, yüklemek ve Linux aracısını IOT C tabanlı güvenlik için Azure Güvenlik Merkezi (ASC) dağıtmak nasıl açıklar.

Bu kılavuzda şunların nasıl yapıldığını öğrenirsiniz: 
> [!div class="checklist"]
> * Yükleme
> * Dağıtımı doğrulama
> * Aracıyı kaldırma
> * Sorun giderme 

## <a name="prerequisites"></a>Önkoşullar

Diğer platformlar ve aracı özellikleri için bkz. [doğru güvenlik aracı seçin](how-to-deploy-agent.md).

1. Güvenlik aracısını dağıtmak için (sudo) üzerinde yüklemek istediğiniz makinede yerel yönetici hakları gereklidir.

1. [Bir güvenlik modülünüzü oluşturmak](quickstart-create-security-twin.md) aygıt için.

## <a name="installation"></a>Yükleme 

Yükleme ve güvenlik aracısı dağıtmak için aşağıdakileri yapın:


1. Makinenizden en son sürümü indirin [Github](https://aka.ms/iot-security-github-c).

1. Paket içeriğini ayıklayın ve gidin _/Install_ klasör.

1. Çalışan izinleri ekleyin **InstallSecurityAgent betik** aşağıdakileri çalıştırarak:
    
   ```
   chmod +x InstallSecurityAgent.sh
   ```

1. Ardından çalıştırın: 

   ```
   ./InstallSecurityAgent.sh -aui <authentication identity> -aum <authentication method> -f <file path> -hn <host name> -di <device id> -i
   ```
   
   Bkz: [kimlik doğrulamasını yapılandırma](concept-security-agent-authentication-methods.md) kimlik doğrulama parametreleri hakkında daha fazla bilgi.

Bu betik şunları yapar:

1. Önkoşulları yükler.

2. Hizmet kullanıcısı (devre dışı etkileşimli oturum açma ile) ekler.

3. Aracı olarak yükleyen bir **arka plan programı** -cihazın kullandığı varsayılır **systemd** hizmet yönetimi için.

4. Aracı kimlik doğrulama parametreleriyle yapılandırır. 

Ek Yardım için komut dosyasını çalıştırmak help parametresini ile: 
    
    ./InstallSecurityAgent.sh --help

### <a name="uninstall-the-agent"></a>Aracıyı kaldırma

Aracıyı kaldırmak için komut dosyasını çalıştır-parametresini kaldırın:

    ./InstallSecurityAgent.sh -–uninstall

## <a name="troubleshooting"></a>Sorun giderme
Dağıtım durumunu denetleyin:

    systemctl status ASCIoTAgent.service


## <a name="next-steps"></a>Sonraki adımlar
- ASC IOT hizmeti için okuma [genel bakış](overview.md)
- ASC hakkında daha fazla bilgi edinmek için IOT [mimarisi](architecture.md)
- Etkinleştirme [hizmeti](quickstart-onboard-iot-hub.md)
- Okuma [SSS](resources-frequently-asked-questions.md)
- Anlamak [güvenlik uyarıları](concept-security-alerts.md)