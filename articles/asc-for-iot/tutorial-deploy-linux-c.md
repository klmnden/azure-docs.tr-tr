---
title: Yüklemek ve IOT aracı Önizleme ASC, Linux C Aracısı dağıtmak için öğretici | Microsoft Docs
description: ASC IOT aracının hem 32-bit hem de 64-bit Linux üzerinde yüklemeyi öğrenin.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: 3ccf2aec-106a-4d2c-8079-5f3e8f2afdcb
ms.service: ascforiot
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/26/2019
ms.author: mlottner
ms.openlocfilehash: 04b41791915946820c9c7a1666ab10e880731e4b
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58541585"
---
# <a name="tutorial-deploy-asc-for-iot-c-based-security-agent-for-linux"></a>Öğretici: Linux için aracıyı IOT C tabanlı güvenlik için ASC dağıtma

> [!IMPORTANT]
> ASC IOT için şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bu öğreticide, yüklemek ve dağıtmak için Linux IOT C tabanlı güvenlik aracısında ASC açıklanmaktadır.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz: 
> [!div class="checklist"]
> * Yükleme
> * Dağıtımı doğrulama
> * Aracıyı kaldırma
> * Sorun giderme 

## <a name="prerequisites"></a>Önkoşullar

Diğer platformlar ve aracı flavours için [doğru güvenlik aracı seçin](select-deploy-agent.md).

1. Güvenlik aracısı dağıtmak için yüklemek istediğiniz makinede yerel yönetici hakları gereklidir.

1. [Bir güvenlik modülünüzü oluşturmak](quickstart-create-security-twin.md) aygıt için.

## <a name="installation"></a>Yükleme 

Yükleme ve güvenlik aracısı dağıtmak için aşağıdakileri yapın:

1. Makinenizden en son sürümü indirin [Github](https://aka.ms/iot-security-github-cs).

1. Paket içeriğini ayıklayın ve gidin _/installation_ klasör.

1. Çalışan izinleri ekleyin **InstallSecurityAgent betik** aşağıdakileri çalıştırarak:
    
    `chmod +x InstallSecurityAgent.sh` 

1. Ardından çalıştırın: 

   ```
   ./InstallSecurityAgent.sh -i -aui <authentication identity>  -aum <authentication method> -f <file path> -hn <host name>  -di <device id> -cl <certificate location kind>
   ```
   
   Bkz: [kimlik doğrulamasını yapılandırma](concept-security-agent-authentication-methods.md) kimlik doğrulama parametreleri hakkında daha fazla bilgi.

Bu betik şunları yapar:

1. Önkoşulları yükler.

2. Hizmet kullanıcısı (devre dışı etkileşimli oturum açma ile) ekler.

3. Aracı olarak yükleyen bir **arka plan programı** -cihazın kullandığı varsayılır **systemd** hizmet yönetimi için.

4. Yapılandırır **sudoers** kök olarak bazı görevleri gerçekleştirmek üzere aracı izin vermek için.

5. Aracı kimlik doğrulama parametreleriyle yapılandırır. 

Ek Yardım için komut dosyasını çalıştırmak help parametresini ile: 
    
    `./InstallSecurityAgent.sh --help`

### <a name="uninstall-the-agent"></a>Aracıyı kaldırma

Aracıyı kaldırmak için – u parametresiyle betiği çalıştırın:

    `./InstallSecurityAgent.sh -u`. 

```
 .\InstallSecurityAgent.sh –uninstall / -u
``` 

## <a name="troubleshooting"></a>Sorun giderme
Dağıtım durumunu denetleyin:

```
systemctl status ASCIoTAgent.service
```


## <a name="next-steps"></a>Sonraki adımlar
- ASC IOT hizmeti için okuma [genel bakış](overview.md)
- ASC hakkında daha fazla bilgi edinmek için IOT [mimarisi](architecture.md)
- Etkinleştirme [hizmeti](quickstart-onboard-iot-hub.md)
- Okuma [SSS](resources-frequently-asked-questions.md)
- Anlamak [güvenlik uyarıları](concept-security-alerts.md)