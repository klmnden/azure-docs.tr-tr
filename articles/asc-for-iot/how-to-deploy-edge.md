---
title: Azure Güvenlik Merkezi için IOT Edge modülü dağıtma | Microsoft Docs
description: IOT Edge üzerinde IOT güvenlik aracısı için bir Azure Güvenlik Merkezi dağıtma hakkında bilgi edinin.
services: asc-for-iot
ms.service: ascforiot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 32a9564d-16fd-4b0d-9618-7d78d614ce76
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/1/2019
ms.author: mlottner
ms.openlocfilehash: 85e342f08e5402e50e5b0dfd1fe2df90337f29ca
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66254302"
---
# <a name="deploy-a-security-module-on-your-iot-edge-device"></a>IOT Edge Cihazınızda güvenlik modül dağıtma

> [!IMPORTANT]
> IOT için Azure Güvenlik Merkezi şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

**IOT için Azure Güvenlik Merkezi (ASC)** modülü IOT Edge cihazınız için kapsamlı güvenlik çözümü sağlar.
Güvenlik Modülü toplar, toplar ve eyleme dönüştürülebilir güvenlik önerileri ve uyarılar içinde işletim sistemi ve kapsayıcı sisteminizden ham güvenlik verilerini analiz eder.
Daha fazla bilgi için bkz. [güvenlik modülü IOT Edge için](security-edge-architecture.md).

Bu kılavuzda, IOT Edge Cihazınızda güvenlik modül dağıtma konusunda bilgi edinin.

## <a name="deploy-security-module"></a>Güvenlik modül dağıtma

IOT Edge için IOT güvenlik modül için bir ASC dağıtmak için aşağıdaki adımları kullanın.

### <a name="prerequisites"></a>Önkoşullar

- IOT hub'ına, cihazınızın olduğundan emin olun [IOT Edge cihazı kaydedilen](https://docs.microsoft.com/azure/iot-edge/how-to-register-device-portal).

- IOT Edge modülü için ASC gerektirir [AuditD framework](https://linux.die.net/man/8/auditd) IOT Edge cihazında yüklü.

    - Framework, IOT Edge Cihazınızda aşağıdaki komutu çalıştırarak yükleyin:
   
      `sudo apt-get install auditd audispd-plugins`
   
    - Aşağıdaki komutu çalıştırarak AuditD etkin olduğunu doğrulayın:
   
      `sudo systemctl status auditd`
      
        Beklenen yanıt `active (running)`. 

### <a name="deployment-using-azure-portal"></a>Azure portalını kullanarak dağıtım

1. Azure portalından açın **Market**.

1. Seçin **nesnelerin interneti**, ardından aramak **IOT için Azure Güvenlik Merkezi** ve bu seçeneği belirleyin.

   ![IOT için Select Azure Güvenlik Merkezi](media/howto/edge-onboarding-8.png)

1. Tıklayın **Oluştur** dağıtımını yapılandırmak için. 

1. Azure'u seçin **abonelik** , IOT Hub'ın ardından, **IOT hub'ı**.<br>Seçin **cihazlara dağıtma** tek bir cihazı hedeflemeniz ya da seçmek için **uygun ölçekte dağıtma** birden fazla cihazı hedefleyin ve **Oluştur**. Uygun ölçekte dağıtma hakkında daha fazla bilgi için bkz. [nasıl dağıtılacağı](https://docs.microsoft.com/azure/iot-edge/how-to-deploy-monitor). 

    >[!Note] 
    >Seçtiyseniz **uygun ölçekte dağıtma**, devam etmeden önce cihaz adı ve ayrıntılarını eklemek **Modül Ekle** sekmesinde aşağıdaki yönergeleri izleyin.     

IOT için Azure Güvenlik Merkezi için bir IOT Edge dağıtımı oluşturmak için üç adım vardır. Aşağıdaki bölümlerde, her birini yol. 

#### <a name="step-1-add-modules"></a>1. adım: Modül Ekle

1. Gelen **Ekle modülleri** sekmesinde **dağıtım modülleri** alanı tıklayın **AzureSecurityCenterforIoT**. 
   
1. Değişiklik **adı** için **azureiotsecurity**.
1. Değişiklik **görüntü URI** için **mcr.microsoft.com/ascforiot/azureiotsecurity:0.0.3**.
1. Doğrulama **kapsayıcı oluşturma seçenekleri** değeri ayarı:      
    ``` json
    {
        "NetworkingConfig": {
            "EndpointsConfig": {
                "host": {}
            }
        },
        "HostConfig": {
            "Privileged": true,
            "NetworkMode": "host",
            "PidMode": "host",
            "Binds": [
                "/:/host"
            ]
        }
    }    
    ```
1. Doğrulayın **istenen özellikler kümesi modül ikizi** seçilir ve yapılandırma nesnesine değiştirin:
      
    ``` json
      "properties.desired": {
        "azureiot*com^securityAgentConfiguration^1*0*0": {
        }
      }
      ```

1. **Kaydet**’e tıklayın.
1. Sekmesini seçin ve En Alta kadar kaydır **Gelişmiş Edge çalışma zamanı ayarları Yapılandır**.
   
   >[!Note]
   > Yapmak **değil** AMQP iletişim IOT Edge hub'ı için devre dışı bırakın.
   > Azure Güvenlik Merkezi IOT modülü IOT Edge hub'ı ile AMQP iletişim gerektirir.
   
1. Değişiklik **görüntü** altında **Edge hub'ı** için **mcr.microsoft.com/ascforiot/edgehub:1.0.9-preview**.

   >[!Note]
   > IOT modülü için Azure Güvenlik Merkezi SDK'sı üzerinde sürüm 1,20 tabanlı IOT Edge Hub çatalı oluşturulan bir sürümünü gerektirir.
   > IOT Edge hub'ı görüntüsünü değiştirerek IOT Edge Cihazınızı çatalı oluşturulan sürüm resmi olarak IOT Edge hizmet tarafından desteklenmeyen IOT Edge Hub'ın en son kararlı sürüm yerine talimatını vermiş olursunuz.

1. Doğrulama **oluşturma seçenekleri** ayarlanır: 
         
    ``` json
    {
      "HostConfig": {
        "PortBindings": {
          "8883/tcp": [{"HostPort": "8883"}],
          "443/tcp": [{"HostPort": "443"}],
          "5671/tcp": [{"HostPort": "5671"}]
        }
      }
    }
    ```
      
1. **Kaydet**’e tıklayın.
   
1. **İleri**’ye tıklayın.

#### <a name="step-2-specify-routes"></a>2. adım: Rota belirtme 

1. İçinde **yolları belirtin** sekmesinde, belirleyin **ASCForIoTToIoTHub** yönlendirmek **"ÖĞESİNDEN/iletileri/modülleri/azureiotsecurity/\* $ Yukarı Akış"** , tıklayın **Sonraki**.

   ![Rota belirtme](media/howto/edge-onboarding-9.png)

#### <a name="step-3-review-deployment"></a>3. adım: Dağıtım gözden geçirin

1. İçinde **gözden dağıtım** sekmesinde, dağıtım bilgilerinizi gözden geçirin ve ardından seçin **Gönder** dağıtımı tamamlamak için.

## <a name="diagnostic-steps"></a>Tanılama adımları

Bir sorunla karşılaşırsanız, kapsayıcı günlüklerini güvenlik modülü IOT Edge cihazı durumu hakkında bilgi edinmek için en iyi yoludur. Bilgi toplamak için bu bölümdeki komutları ve araçları kullanın.

### <a name="verify-the-required-containers-are-installed-and-functioning-as-expected"></a>Gerekli kapsayıcıları yüklü ve beklendiği gibi çalıştığını doğrulayın

1. IOT Edge Cihazınızda aşağıdaki komutu çalıştırın:
    
     `sudo docker ps`
   
1. Kapsayıcıların çalıştığından emin olun:
   
   | Ad | GÖRÜNTÜ |
   | --- | --- |
   | azureiotsecurity | MCR.microsoft.com/ascforiot/azureiotsecurity:0.0.3 |
   | edgeHub | mcr.microsoft.com/ascforiot/edgehub:1.0.9-preview |
   | edgeAgent | mcr.microsoft.com/azureiotedge-agent:1.0 |
   
   Gereken en düşük kapsayıcı yok, IOT Edge dağıtım bildiriminin bir önerilen ayarlarla hizalanır denetleyin. Daha fazla bilgi için [dağıtma IOT Edge Modülü](#deployment-using-azure-portal).

### <a name="inspect-the-module-logs-for-errors"></a>Modül günlüklerini hatalar için inceleyin
   
1. IOT Edge Cihazınızda aşağıdaki komutu çalıştırın:

   `sudo docker logs azureiotsecurity`
   
1. Daha ayrıntılı günlükleri için aşağıdaki ortam değişkenine ekleyin **azureiotsecurity** modülü dağıtımı: `logLevel=Debug`.

## <a name="next-steps"></a>Sonraki adımlar

Yapılandırma seçenekleri hakkında daha fazla bilgi için modül yapılandırması ile ilgili nasıl yapılır kılavuzuna devam edin. 
> [!div class="nextstepaction"]
> [Modül yapılandırması ile ilgili nasıl yapılır Kılavuzu](./how-to-agent-configuration.md)
