---
title: Azure IOT Hub cihazı sağlama hizmeti cihazları yeniden sağlamak nasıl | Microsoft Docs
description: Cihaz sağlama hizmeti örneği Cihazınızda yeniden sağlamak nasıl
author: wesmc7777
ms.author: v-yiso
origin.date: 04/04/2019
ms.date: 05/06/2019
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: timlt
ms.openlocfilehash: 92680a453d93c8dc0189c6ae376449a8e7a22076
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60627363"
---
# <a name="how-to-reprovision-devices"></a>Cihazları yeniden sağlamak nasıl

Bir IOT çözümünü yaşam döngüsü boyunca, cihazlar IOT hub'ları arasında taşımak için yaygındır. Bu taşıma nedenleri, aşağıdaki senaryolar şunları içerebilir:

* **Coğrafi konum**: Bir cihaz konumlar arasında hareket ettikçe, ağ gecikmesi cihaz sağlayarak geliştirilmiştir bir IOT hub yakın her yere geçirilmiş.

* **Çok kiracılılık**: Bir cihaz aynı IOT çözüm içinde kullanılabilir, ancak yeniden atandı veya bir yeni müşteri veya müşteri siteye kiralanmış. Bu yeni müşteri, farklı bir IOT hub'ı kullanarak hizmet.

* **Çözüm değişiklik**: Bir cihaz yeni veya güncelleştirilmiş bir IOT çözüm taşınmış. Cihazın diğer arka uç bileşenlerine bağlı yeni bir IOT hub ile iletişim kurmak yeniden atama gerektirebilir. 

* **Karantina**: Bir çözüm değişiklik benzer. Yapıyor, gizliliği ihlal edilmiş veya güncel olmayan bir cihaz IOT hub'ı tüm yapabileceğini olduğu güncelleştirin ve uyumluluk dönmek için atanabilir. Cihazın düzgün sonra ana hub'a sonra geçirilir.

Çıkış daha fazla ayrıntılı genel bakış için bkz: [IOT Hub cihaz kavramları çıkış](concepts-device-reprovision.md).


## <a name="configure-the-enrollment-allocation-policy"></a>Kayıt ayırma ilkesini yapılandırma

Ayırma ilkesini nasıl kaydıyla ilişkili cihazları ayrılan, veya kaldırılacak, atanan bir IOT hub'a bir kez kullanılması belirler.

Ayırma ilkesi cihaz kaydı için aşağıdakileri yapılandırın:

1. Oturum [Azure portalında](https://portal.azure.com) ve cihaz sağlama hizmeti Örneğinize gidin.

2. Tıklayın **kayıtları Yönet**ve çıkış için yapılandırmak istediğiniz bireysel kayıt ve kayıt grubunu tıklatın. 

3. Altında **nasıl hub'lara cihazları atamak istediğinizi seçin**, aşağıdaki ayırma ilkelerden birini seçin:

    * **En düşük gecikme**: Bu ilke cihazları bağlı IOT Hub cihaz ile IOT hub'ı arasında düşük gecikme süresi iletişimin sonuçlanır atar. Bu seçenek, cihazın konumuna göre en yakın IOT hub ile iletişim kurmak etkinleştirir. 
    
    * **Eşit ağırlıklı dağılım**: Bu ilke, her bağlı IOT hub'ına atanma ayırma ağırlığı göre bağlı IOT hub'lar genelinde cihazlara dağıtır. Bu ilke, Bakiye cihazlar arasında bu hub'larında ayarlamak ayırma ağırlıklara göre bağlı hub grubu sağlar. Yalnızca bir IOT hub'a aygıtları sağlıyorsanız bu ayarı öneririz. Bu varsayılan ayardır. 
    
    * **Statik yapılandırma**: Bu ilke, kayıt girişindeki sağlanacak bir cihaz için istenen IOT hub'ı listelenmesi gerektirir. Bu ilke, cihazlara atamak istediğiniz tek bir belirli IOT hub'a belirtmenizi sağlar.

4. Altında **bu Grup atanabilen IOT hub'ları seçin**, ayırma ilkenizle dahil istediğiniz bağlantılı IOT hub'ı seçin. Yeni bir bağlı IOT hub'ı kullanarak isteğe bağlı olarak, ekleme **yeni bir IOT Hub bağlantı** düğmesi.

    İle **en düşük gecikme** ayırma ilkesi, seçtiğiniz hub'ları dahil edilecek cihaz atama için en yakın hub belirlemek için gecikme süresi değerlendirme.

    İle **eşit ağırlıklı dağılım** ayırma ilkesini cihazlar üzerinde yapılandırılmış ayırma ağırlıkları tabanlı seçtiğiniz hub'lar genelinde Dengeli ve bunların geçerli cihaz yük olacaktır.

    İle **statik yapılandırma** ayırma ilkesini, istediğiniz atanan cihazlar IOT hub'ı seçin.

4. Tıklayın **Kaydet**, veya reprovisioning ilkesini ayarlamak için sonraki bölüme geçin.

    ![Kayıt ayırma ilkesini seçin](./media/how-to-reprovision/enrollment-allocation-policy.png)



## <a name="set-the-reprovisioning-policy"></a>Reprovisioning İlkesi ayarlama

1. Oturum [Azure portalında](https://portal.azure.com) ve cihaz sağlama hizmeti Örneğinize gidin.

2. Tıklayın **kayıtları Yönet**ve çıkış için yapılandırmak istediğiniz bireysel kayıt ve kayıt grubunu tıklatın.

3. Altında **farklı bir IOT hub'a yeniden sağlama ele alınması için cihaz verilerini nasıl istediğinizi seçin**, aşağıdaki reprovisioning ilkelerden birini seçin:

    * **Yeniden sağlama ve veri geçişi**: Bu ilke, cihaz kayıt girişi ile ilişkili yeni bir sağlama isteği gönderdiğinizde eylemi gerçekleştirir. Kayıt girdisi yapılandırmasına bağlı olarak, cihazın başka bir IOT hub'ına atanabilir. İlk IOT hub ile cihaz kaydı, cihaz IOT hub'ları değişiyorsa kaldırılacak. Bu ilk IOT hub'ından tüm cihaz durumu bilgilerini yeni IOT hub'ına üzerinden geçirilecektir. Geçiş sırasında cihazın durumu olarak raporlanır **atama**

    * **Yeniden sağlama ve ilk yapılandırmaya Sıfırla**: Bu ilke, cihaz kayıt girişi ile ilişkili yeni bir sağlama isteği gönderdiğinizde eylemi gerçekleştirir. Kayıt girdisi yapılandırmasına bağlı olarak, cihazın başka bir IOT hub'ına atanabilir. İlk IOT hub ile cihaz kaydı, cihaz IOT hub'ları değişiyorsa kaldırılacak. Cihaz sağlanırken sağlama hizmeti örneği alınan ilk yapılandırma verileri, yeni IOT hub'ına sağlanır. Geçiş sırasında cihazın durumu olarak raporlanır **atama**.

4. Tıklayın **Kaydet** Değişikliklerinize bağlı cihaz çıkış etkinleştirmek için.

    ![Kayıt ayırma ilkesini seçin](./media/how-to-reprovision/reprovisioning-policy.png)



## <a name="send-a-provisioning-request-from-the-device"></a>CİHAZDAN bir sağlama İsteği Gönder

Sağlama cihazlar için sırasıyla önceki bölümde yapılan yapılandırma değişikliklerinin bağlı olarak, bu cihazlar çıkış istemeniz gerekir. 

Ne sıklıkta bir cihaz sağlama isteği gönderir, senaryoya bağlıdır. Ancak, program bir sağlama hizmeti örneği yeniden başlatma ve destek için bir sağlama isteği göndermek için cihazlar için önerilir bir [yöntemi](../iot-hub/iot-hub-devguide-direct-methods.md) isteğe bağlı olarak sağlama el ile tetiklemek için. Sağlama ayrıca tetiklenen ayarlayarak bir [özelliği istenen](../iot-hub/iot-hub-devguide-device-twins.md#desired-property-example). 

Bir kayıt girişi reprovisioning ilke, cihaz sağlama hizmeti örneği sağlama bu isteklerin nasıl işlediğini ve cihaz durumu verilerini çıkış sırasında taşınıp taşınmadığını belirler. Bireysel kayıtlar ve kayıt grupları için aynı ilkeleri kullanılabilir:

Örneğin bir önyükleme işlemi sırasında bir CİHAZDAN istekleri sağlama gönderme kodunu görmek [otomatik sağlama bir simülasyon cihazı](quick-create-simulated-device.md).


## <a name="next-steps"></a>Sonraki adımlar

- Reprovisioning daha fazla bilgi edinmek için [IOT Hub cihaz kavramları çıkış](concepts-device-reprovision.md) 
- Daha fazla sağlama kaldırmayı bilgi edinmek için [nasıl daha önce otomatik olarak sağlanan cihazları sağlamasını kaldırmak](how-to-unprovision-devices.md) 











