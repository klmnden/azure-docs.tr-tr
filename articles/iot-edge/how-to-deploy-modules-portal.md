---
title: Azure IOT kenar modülleri (portal) dağıtma | Microsoft Docs
description: Modülleri IOT kenar cihazına dağıtmak için Azure portalını kullanın
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/06/2018
ms.topic: conceptual
ms.reviewer: menchi
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 4082189d451f670c1ae3f76b8ec785d8bd0518b3
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036486"
---
# <a name="deploy-azure-iot-edge-modules-from-the-azure-portal"></a>Azure IOT kenar modülleri Azure portalından dağıtma

IOT kenar modülleri, iş mantığı ile oluşturduktan sonra bunları sınırında çalışmaya aygıtlarınıza dağıtmak istiyorsanız. Toplamak ve verileri işlemek için birlikte çalışan birden fazla modülü varsa, bunları aynı anda dağıtması ve bunları bağlayan yönlendirme kurallarını bildirin. 

Bu makalede, nasıl Azure portalında dağıtım bildirimi oluşturma ve dağıtım IOT kenar cihazına Ftp'den sırasında size kılavuzluk eder gösterilmektedir. Kullanıcıların paylaşılan etiketlere göre birden çok aygıt hedefleyen bir dağıtımı oluşturma hakkında daha fazla bilgi için bkz [dağıtma ve izleme ölçekte IOT kenar modülleri](how-to-deploy-monitor.md)

## <a name="prerequisites"></a>Önkoşullar

* Bir [IOT hub'ı](../iot-hub/iot-hub-create-through-portal.md) Azure aboneliğinizde. 
* Bir [IOT sınır cihazı](how-to-register-device-portal.md) yüklü IOT kenar çalışma zamanı. 

## <a name="select-your-device"></a>Cihazınızı seçin

1. Oturum [Azure portal](https://portal.azure.com) ve IOT hub'ına gidin.
2. Seçin **IOT kenar** menüsünde.
3. Aygıtların listesi hedef aygıttan Kimliğini tıklayın. 
4. **Modülleri Ayarlama**'yı seçin.

## <a name="configure-a-deployment-manifest"></a>Bir dağıtım bildirimini yapılandırın

Bir dağıtım bildirimi modüllerine dağıtmak için modüller ve modül çiftlerini istenen özelliklerini arasında veri akışını açıklayan bir JSON belgedir. Dağıtım iş nasıl bildirimleri ve bunların nasıl oluşturulacağı hakkında daha fazla bilgi için bkz: [nasıl IOT kenar modülleri, yapılandırılmış, yeniden ve kullanılabilecek anlamak](module-composition.md).

Azure portalı, JSON belgesini el ile oluşturmak yerine dağıtım bildirimi oluşturmada size yol gösterir. bir sihirbazına sahiptir. Üç adım vardır: **modülleri ekleme**, **belirtin yolları**, ve **dağıtım gözden geçirmeniz**. 

### <a name="add-modules"></a>Modüller ekleme

1. İçinde **kayıt defteri ayarları** bölüm sayfasının modülü görüntülerinizi içeren tüm özel kapsayıcı kayıt defterleri erişmek için kimlik bilgilerini sağlayın. 
2. İçinde **dağıtım modülleri** sayfasında, bölümünü **Ekle**. 
3. Modül türü aşağı açılan listeden seçin: 
   * **IOT kenar Modülü** -varsayılan seçeneği.
   * **Azure Stream Analytics Modülü** -yalnızca bir Azure akış analizi yükünden oluşturulan modüller. 

4. Modül için bir ad sağlayın, sonra kapsayıcı görüntüsünü belirtin. Örneğin: 
   * **Ad** -tempSensor
   * **Görüntü URI** -mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0
5. Gerekirse, isteğe bağlı alanları doldurun. Kapsayıcı hakkında daha fazla bilgi seçenekleri, yeniden başlatma ilkesi oluşturabilir ve istenen durumunu görmek için [EdgeAgent istenen özellikleri](module-edgeagent-edgehub.md#edgeagent-desired-properties). Modül çifti hakkında daha fazla bilgi için bkz: [tanımlayın veya güncelleştirme istenen özellikleri](module-composition.md#define-or-update-desired-properties).
6. **Kaydet**’i seçin.
7. Dağıtımınız için ek modülleri eklemek için 2-6 adımlarını yineleyin. 
8. Seçin **sonraki** yollar bölümüne devam etmek için.

### <a name="specify-routes"></a>Rotaları belirtin

Sihirbaz size varsayılan olarak bir yol adı **rota** ve olarak tanımlanan **FROM /* upstream $ **, herhangi bir modül tarafından çıkış herhangi bir ileti IOT hub'ına gönderilen anlamına gelir.  

Ekleme veya yolları veriler ile güncelleştirecek [bildirme yolları](module-composition.md#declare-routes)seçeneğini belirleyip **sonraki** gözden geçirme bölümüne devam etmek için.

### <a name="review-deployment"></a>Dağıtım gözden geçirin

Oluşturulan gözden geçirme bölüm gösterir, JSON dağıtım bildirim seçimlerinizi önceki iki bölümlerde temel. Var olan iki modül bildirilen, ekleyemiyor Not: **$edgeAgent** ve **$edgeHub**. Bu iki modülleri oluşturan [IOT kenar çalışma zamanı](iot-edge-runtime.md) ve her dağıtımda gerekli varsayılan değerler. 

Dağıtım bilgileri gözden geçirin ve ardından **gönderme**. 

## <a name="view-modules-on-your-device"></a>Aygıtınızdaki modül görüntüle

Aygıtınıza modülleri dağıttıktan sonra sonra bunların tümünün görüntüleyebilirsiniz **cihaz ayrıntıları** portal sayfası. Bu sayfayı her dağıtılan modülü yanı sıra dağıtım durumu ve çıkış kodu gibi yararlı bilgileri adını görüntüler. 

## <a name="next-steps"></a>Sonraki adımlar

Bilgi nasıl [dağıtma ve izleme ölçekte IOT kenar modülleri](how-to-deploy-monitor.md)
