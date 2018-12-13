---
title: Azure Portal - Azure IOT Edge modüllerini dağıtmak | Microsoft Docs
description: Modüller IOT Edge cihazına dağıtmak için Azure portalını kullanma
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/06/2018
ms.topic: conceptual
ms.reviewer: menchi
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: fab85b74fb53bec07843abf31aac1a8495d0fcfb
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53093991"
---
# <a name="deploy-azure-iot-edge-modules-from-the-azure-portal"></a>Azure portalından Azure IOT Edge modüllerini dağıtmak

IOT Edge modülleri, iş mantığı ile oluşturduktan sonra bunları ucuna çalışılacak cihazlarınıza dağıtmak istiyorsanız. Toplamak ve veri işlemek için birlikte çalışan birden çok modül varsa, bunları tamamını aynı anda dağıtabilir ve bunları bağlayan yönlendirme kurallarını bildirin. 

Bu makalede, nasıl Azure portalında bir dağıtım bildirimi oluşturmak ve IOT Edge cihazına dağıtım gönderme sırasında size kılavuzluk eder gösterilmektedir. Birden fazla cihazda kendi paylaşılan etiketlere göre hedefleyen bir dağıtım oluşturma hakkında daha fazla bilgi için bkz. [dağıtma ve izleme uygun ölçekte IOT Edge modülleri](how-to-deploy-monitor.md)

## <a name="prerequisites"></a>Önkoşullar

* Bir [IOT hub'ı](../iot-hub/iot-hub-create-through-portal.md) Azure aboneliğinizdeki. 
* Bir [IOT Edge cihazı](how-to-register-device-portal.md) yüklü olan bir IOT Edge çalışma zamanı ile. 

## <a name="select-your-device"></a>Cihazınızı seçin

1. Oturum [Azure portalında](https://portal.azure.com) ve IOT hub'ınıza gidin.
2. Seçin **IOT Edge** menüsünde.
3. Hedef cihazın cihazlar listesinden numarasını tıklayın. 
4. **Modülleri Ayarlama**'yı seçin.

## <a name="configure-a-deployment-manifest"></a>Bir dağıtım bildirimi yapılandırma

Bir dağıtım bildirimi dağıtmak için modülleri ve modül ikizlerini istenen özellikleri arasında verilerin nasıl aktığını modüllerine açıklayan bir JSON belgesidir. Nasıl iş dağıtım bildirimleri ve bunların nasıl oluşturulacağı hakkında daha fazla bilgi için bkz. [nasıl IOT Edge modülleri, yapılandırılmış, yeniden kaldırılabilir ve anlamak](module-composition.md).

Azure portalı, JSON belgesini el ile oluşturmak yerine dağıtım bildirimini oluşturmada size yol gösterir. bir sihirbaz vardır. Üç adım vardır: **modül eklemek**, **yolları belirtin**, ve **gözden geçirin, dağıtım**. 

### <a name="add-modules"></a>Modül Ekle

1. İçinde **kayıt defteri ayarları** Bölümü sayfasının, modül görüntüleri içeren herhangi bir özel kapsayıcı kayıt defterleri erişmek için kimlik bilgilerini sağlayın. 
2. İçinde **dağıtım modülleri** sayfasında bölümünü **Ekle**. 
3. Modüller, türler, aşağı açılan listeden bakın: 
   * **IOT Edge Modülü** -varsayılan seçenek.
   * **Azure Stream Analytics Modülü** -yalnızca bir Azure Stream Analytics iş yükünden oluşturulan modüller. 
4. Seçin **IOT Edge Modülü**.
5. Modül için bir ad belirtin ve ardından kapsayıcı görüntüsünü belirtin. Örneğin: 
   * **Ad** -tempSensor
   * **Görüntü URI'si** -mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0
6. Gerekirse, isteğe bağlı alanları doldurun. Kapsayıcı hakkında daha fazla bilgi seçenekleri, yeniden başlatma ilkesi oluşturabilir ve istenen durumunu görmek için [EdgeAgent istenen özelliklerini](module-edgeagent-edgehub.md#edgeagent-desired-properties). Modül ikizi hakkında daha fazla bilgi için bkz. [tanımlayın veya güncelleştirme istenen özelliklerini](module-composition.md#define-or-update-desired-properties).
7. **Kaydet**’i seçin.
8. Dağıtımınız için ek modüller eklemek için 2-6 adımlarını yineleyin. 
9. Seçin **sonraki** yollar bölüme geçmek için.

### <a name="specify-routes"></a>Rota belirtme

Varsayılan olarak sihirbaz size bir yol olarak adlandırılan **rota** ve tanımlanmış olarak **FROM /* Yukarı Akış $ **, modüllerin tarafından çıkış iletileri IOT hub'ına gönderilen anlamına gelir.  

Ekleme veya yolları alınan bilgilerle güncelleştirme [bildirmek yollar](module-composition.md#declare-routes), ardından **sonraki** gözden geçirme bölüme geçmek için.

### <a name="review-deployment"></a>Dağıtım gözden geçirin

Oluşturulan gözden geçirme bölümü gösterir, JSON dağıtım bildirimi önceki iki bölümlerde seçimlerinize göre. İki modül bildirildi, eklemediğim olduğunu unutmayın: **$edgeAgent** ve **$edgeHub**. Bu iki modül oluşturan [IOT Edge çalışma zamanı](iot-edge-runtime.md) ve her dağıtımda gerekli varsayılanlardır. 

Dağıtım bilgilerinizi gözden geçirin ve ardından **Gönder**. 

## <a name="view-modules-on-your-device"></a>Cihazınızda modülleri görüntüleme

Cihazınıza modülleri dağıttıktan sonra bunların tümünün görüntüleyebilirsiniz **cihaz ayrıntıları** portal sayfası. Bu sayfa, her dağıtılan modülü yanı sıra dağıtım durumu ve çıkış kodu gibi yararlı bilgiler adını görüntüler. 

## <a name="next-steps"></a>Sonraki adımlar

Bilgi nasıl [dağıtma ve izleme uygun ölçekte IOT Edge modülleri](how-to-deploy-monitor.md)
