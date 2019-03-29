---
title: IOT Edge modülü için bir ASC dağıtma | Microsoft Docs
description: Hakkında bilgi edinin ASC IOT güvenlik aracısı IOT Edge üzerinde dağıtın.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: 32a9564d-16fd-4b0d-9618-7d78d614ce76
ms.service: ascforiot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2019
ms.author: mlottner
ms.openlocfilehash: 87094b265a0c30c0349d64e4b956224525c04f74
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2019
ms.locfileid: "58580501"
---
# <a name="deploy-security-module-on-your-iot-edge-device"></a>IOT Edge Cihazınızda güvenlik modül dağıtma

> [!IMPORTANT]
> ASC IOT için şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

IOT için ASC **azureiotsecurity** modülü IOT Edge cihazınız için kapsamlı güvenlik çözümü sağlar.
Güvenlik Modülü toplar, toplar ve eyleme dönüştürülebilir güvenlik önerileri ve uyarılar içinde işletim sistemi ve kapsayıcı sisteminizden ham güvenlik verilerini analiz eder.
Daha fazla bilgi için bkz. [güvenlik modülü IOT Edge için](security-edge-architecture.md).

Bu kılavuzda, IOT Edge Cihazınızda güvenlik modül dağıtma konusunda bilgi edinin.

## <a name="deploy-security-module"></a>Güvenlik modül dağıtma

IOT Edge için IOT güvenlik modül için bir ASC dağıtmak için aşağıdaki adımları kullanın.

### <a name="prerequisites"></a>Önkoşullar

1. Cihazınızın olduğundan emin olun [IOT Edge cihazı kaydedilen](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-register-device-portal).

1. IOT Edge modülü için ASC gerektirir [AuditD framework](https://linux.die.net/man/8/auditd) Edge cihazında yüklü.

   Framework, Edge Cihazınızda aşağıdaki komutu çalıştırarak yükleyin:
   
   `apt-get install auditd audispd-plugins`

### <a name="deployment-using-azure-portal"></a>Azure portalını kullanarak dağıtım

1. Açık **Market** Azure portalında.

1. Arama **azure IOT güvenlik** tıklayın **Azure IOT güvenlik**.

   ![](media/howto/edge_onboarding_7.png)

1. **Oluştur**’a tıklayın.

1. Seçin, **abonelik**, **IOT hub'ı** ve **IOT Edge cihaz adı**, ardından **Oluştur**.

1. Tıklayın **sonraki** için iki kez **gözden geçirme dağıtım**.

1. Emin **edgeHub.settings.createOptions** aşağıdaki gibi yapılandırılır:

   `"createOptions": "{\"HostConfig\":{\"PortBindings\":{\"8883/tcp\":[{\"HostPort\":\"8883\"}],\"443/tcp\":[{\"HostPort\":\"443\"}],\"5671/tcp\":[{\"HostPort\":\"5671\"}]}}}"`

1. Tıklayın **Gönder** dağıtımı tamamlamak için.


## <a name="next-steps"></a>Sonraki adımlar

Yapılandırma seçenekleri hakkında daha fazla bilgi için modül yapılandırması ile ilgili nasıl yapılır kılavuzuna devam edin. 
> [!div class="nextstepaction"]
> [Modül yapılandırması nasıl Kılavuzu](./how-to-agent-configuration.md)