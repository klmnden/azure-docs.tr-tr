---
title: IOT hub'ı oluşturmak için Azure portalını kullanma | Microsoft Docs
description: Nasıl-oluşturma, yönetme ve Azure IOT hub'ı Azure portalından silin. Fiyatlandırma katmanları için güvenlik, ölçeklendirme ve yapılandırma Mesajlaşma hakkında bilgi içerir.
author: dominicbetts
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: dobett
ms.openlocfilehash: 0b03ae434e93dbab45235fe67c499497e1257064
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/17/2018
ms.locfileid: "42056253"
---
# <a name="create-an-iot-hub-using-the-azure-portal"></a>Azure portalını kullanarak IOT hub oluşturma

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

Bu makalede açıklanır:

* IOT Hub hizmeti Azure Portalı'nda bulmak nasıl.
* Oluşturun ve IOT hub'ları yönetmek nasıl.

## <a name="where-to-find-the-iot-hub-service"></a>IOT Hub hizmeti nerede bulacağını

IOT Hub hizmeti Portalı'nda şu konumlarda bulabilirsiniz:

* Seçin **+ yeni**, ardından **nesnelerin interneti**.
* Market'te seçin **nesnelerin interneti**.

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

Aşağıdaki yöntemleri kullanarak IOT hub'ı oluşturabilirsiniz:

* **+ Yeni** seçeneği aşağıdaki ekran görüntüsünde gösterilen dikey penceresi açılır. Bu yöntem ve Market aracılığıyla IOT hub'ı oluşturmak için adımları aynıdır.

* Market'te seçin **Oluştur** aşağıdaki ekran görüntüsünde gösterilen dikey penceresini açın.

Aşağıdaki bölümlerde, IOT hub oluşturma adımları açıklanmaktadır.

### <a name="choose-the-name-of-the-iot-hub"></a>IOT hub'ı adını seçin

IOT hub'ı oluşturmak için IOT hub'ı adı olmalıdır. Bu ad, tüm IOT hub'ları arasında benzersiz olması gerekir.

[!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

### <a name="choose-the-pricing-tier"></a>Fiyatlandırma katmanı seçin

Bağlı olarak kaç özellikleri birkaç katmanı arasından seçim yapabilirsiniz istediğiniz ve kaç iletileri gönder günde çözümünüz aracılığıyla. Ücretsiz katman, test ve değerlendirme için tasarlanmıştır. Günlük ileti 8000'en fazla ve IOT hub'ına bağlanması 500 CİHAZDAN sağlar. Her Azure aboneliğinin bir IOT Hub ücretsiz katmanında oluşturabilirsiniz. 

Bir katman seçenekleri hakkında daha fazla ayrıntı için bkz: [doğru IOT Hub katmanını seçme](iot-hub-scaling.md).

### <a name="iot-hub-units"></a>IOT hub'ı birimleri

Günlük birim başına izin verilen ileti sayısı, hub'ın fiyatlandırma katmanına bağlıdır. Örneğin, IOT hub'ı 700.000 iletilerinin giriş desteklemek için isterseniz, iki adet S1 katmanı birimi seçin.

### <a name="device-to-cloud-partitions-and-resource-group"></a>Cihaz buluta bölümler ve kaynak grubu

IOT hub'ı bölüm sayısını değiştirebilirsiniz. Varsayılan bölüm sayısı 4'tür; farklı bir numara aşağı açılan listeden seçebilirsiniz.

Açıkça boş bir kaynak grubu oluşturmanız gerekmez. Bir kaynak oluştururken, yeni bir kaynak grubu oluşturmak için seçin veya mevcut bir kaynak grubunu kullanın.

![Azure portalında bir hub'ı oluşturma gösteren ekran görüntüsü](./media/iot-hub-create-through-portal/location1.png)

### <a name="choose-subscription"></a>Abonelik seçin

Azure IOT hub'ı otomatik olarak kullanıcı hesabı bağlı olduğu Azure abonelikleri listelenir. IOT hub'ına ilişkilendirmek için Azure abonelik seçebilirsiniz.

### <a name="choose-the-location"></a>Konum seçin

Konum seçeneği, IOT hub'ı kullanılabilir olduğu bölgelerin bir listesini sağlar.

### <a name="create-the-iot-hub"></a>IOT hub'ı oluşturma

Önceki tüm adımları tamamlandığında, IOT hub'ı oluşturabilirsiniz. Tıklayın **Oluştur** oluşturmak ve belirlediğiniz seçeneklere ile IOT hub'ı dağıtmak için arka uç işlemi başlatmak için.

Uygulamanın, uygun konuma sunucularda çalıştırmak arka uç dağıtım Sürüyor olarak IOT hub'ı oluşturmak için birkaç dakika sürebilir.

## <a name="change-the-settings-of-the-iot-hub"></a>IOT hub'ının ayarlarını değiştirme
<!--robinsh these screenshots are out of date -->

IOT Hub dikey penceresinden oluşturulduktan sonra var olan IOT hub'ının ayarlarını değiştirebilirsiniz.

![IOT hub'ının ayarları gösteren ekran görüntüsü](./media/iot-hub-create-through-portal/portal-settings.png)

**Paylaşılan erişim ilkeleri**: IOT Hub'ına bağlanmak için izinleri cihazlar ve hizmetler için bu ilkeler tanımlayın. Bu ilkeler tıklayarak erişebilirsiniz **paylaşılan erişim ilkeleri** altında **genel**. Bu dikey pencerede, mevcut ilkeleri değiştirme veya yeni bir ilke ekleyin.

### <a name="create-a-policy"></a>İlke oluştur

* Tıklayın **Ekle** bir dikey penceresini açın. Burada, yeni ilke adı ve aşağıdaki resimde gösterildiği gibi bu ilke ile ilişkilendirmek istediğiniz izinleri girebilirsiniz:

    Bu paylaşılan ilkeleriyle ilişkilendirilebilir birkaç izinleri vardır. **Kayıt defterini oku** ve **kayıt defteri yazma** ilkeleri kimlik kayıt defterini okuma ve yazma erişimi hakkı verin. Otomatik olarak yazma seçeneğini seçerken, salt okunur seçeneğini seçer.

    **Hizmetini bağlama** ilkesinin hizmet uç noktaları gibi erişim izni verdiği **CİHAZDAN buluta alma**. **Cihazı bağlayın** İlkesi, IOT Hub cihaz tarafındaki uç noktalarda kullanarak ileti gönderme ve alma için izinler verir.

* Tıklayın **Oluştur** Bu ilke mevcut listesine yeni oluşturulan eklemek için.

   ![Paylaşılan erişim ilkesi ekleme gösteren ekran görüntüsü](./media/iot-hub-create-through-portal/shared-access-policies.png)

## <a name="endpoints"></a>Uç Noktalar

Tıklayın **uç noktaları** değiştirmekte olduğunuz IOT hub'ının bitiş noktaları listesini görüntüleyin. Uç noktalar için iki tür vardır: IOT hub'ına yerleşik uç noktaları ve IOT hub'ına oluşturulduktan sonra eklediğiniz uç noktaları.

![Bir uç nokta ekleme gösteren ekran görüntüsü](./media/iot-hub-create-through-portal/messaging-settings.png)

### <a name="built-in-endpoints"></a>Yerleşik uç noktaları

İki yerleşik uç nokta vardır: **cihaz geri bildirim buluta** ve **olayları**.

* **Cihaz geri bildirim buluta** ayarları: iki subsettings bu ayarı vardır: **bulut cihaz TTL** (zaman yaşam) ve **elde tutma süresi** (saat içinde) iletiler için. İlk IOT hub'ı oluşturduğunuzda, bu her iki ayarın bir saatlik varsayılan değere sahip. Bu ayarları ayarlamak için kaydırıcıları kullanın veya değerleri yazın.

* **Olayları** ayarları: Bu ayar salt okunur bazıları olan birkaç subsettings sahiptir. Aşağıdaki listede, bu ayarları açıklanmaktadır:

  * **Bölümler**: IOT hub'ı oluşturduğunuzda, varsayılan bir değer ayarlanır. Bu ayarı ile bölüm sayısını değiştirebilirsiniz.

  * **Event Hub ile uyumlu ada ve uç nokta**:, IOT hub oluşturulur, bir olay hub'ı, belirli koşullar altında erişim gerekebilir dahili olarak oluşturulur. Event Hub ile uyumlu ada ve uç nokta değerleri özelleştiremezsiniz ancak tıklayarak kopyalayabilirsiniz **kopyalama**.

  * **Elde tutma süresi**: bir gün için varsayılan olarak ayarlanmış ancak bu açılan listeyi kullanarak değiştirebilirsiniz. CİHAZDAN buluta ayarının gün içinde bu değer olur.

  * **Tüketici grupları**: tüketici grupları iletileri bağımsız olarak IOT hub'ından okumak birden fazla okuyucuyu kapsayacak etkinleştirin. Her IOT hub ile varsayılan bir tüketici grubu oluşturulur. Ancak, ekleyebilir veya bu ayarı kullanarak, IOT hub tüketici grupları silin.

  > [!NOTE]
  > Varsayılan bir tüketici grubu düzenlenebilir veya silinemez.

### <a name="custom-endpoints"></a>Özel uç noktalar

Portalı kullanarak IOT hub'ınızda özel uç noktalar ekleyebilirsiniz. Gelen **uç noktaları** dikey penceresinde tıklayın **Ekle** açmak için üstteki **uç noktası ekleme** dikey penceresi. Gerekli bilgileri girin ve ardından tıklayın **Tamam**. Özel uç noktanıza ana listelendiğine **uç noktaları** dikey penceresi.

![Özel uç nokta oluşturma gösteren ekran görüntüsü](./media/iot-hub-create-through-portal/endpoint-creation.png)

Daha fazla bilgi edinebilirsiniz özel uç noktalar hakkında [başvuru - IOT hub uç noktaları]( iot-hub-devguide-endpoints.md).

## <a name="routes"></a>Yollar

Tıklayın **yollar** yönetme nasıl IOT hub'ı, CİHAZDAN buluta iletiler gönderir.

![Yeni bir yol ekleme gösteren ekran görüntüsü](./media/iot-hub-create-through-portal/routes-list.png)

IOT hub'ına tıklayarak yollar ekleyebilirsiniz **Ekle** en üstündeki **yollar*** dikey penceresinde gerekli bilgileri girip tıklayarak **Tamam**. Rotanız sonra ana listelenen **yollar** dikey penceresi. Bir rota yollar listesinde tıklayarak düzenleyebilirsiniz. Bir rota etkinleştirmek için yolların listesinde tıklayın ve ayarlama **etkin** geç **kapalı**. Değişikliği kaydetmek için tıklatın **Tamam** dikey pencerenin alt kısmındaki.

![Yeni bir yönlendirme kuralı düzenleme gösteren ekran görüntüsü](./media/iot-hub-create-through-portal/route-edit.png)

## <a name="delete-the-iot-hub"></a>IOT hub'ını Sil

IOT hub'ına tıklayarak silmek istediğiniz göz atabilirsiniz **Gözat**, silmek için uygun hub'ı seçerek. IOT hub'ı silmek için tıklayın **Sil** düğmesinin altında IOT hub adı.

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT hub'ı yönetme hakkında daha fazla bilgi için bu bağlantıları izleyin:

* [IoT cihazlarını toplu yönetme](iot-hub-bulk-identity-mgmt.md)
* [IOT hub'ı ölçümleri](iot-hub-metrics.md)
* [İşlemleri izleme](iot-hub-operations-monitoring.md)

Daha fazla IOT Hub'ın özelliklerini keşfetmek için bkz:

* [IOT Hub Geliştirici Kılavuzu](iot-hub-devguide.md)
* [Yapay ZEKA, Azure IOT Edge ile uç cihazlarına dağıtma](../iot-edge/tutorial-simulate-device-linux.md)
* [IOT çözümünüzü baştan güvenli hale getirme](../iot-fundamentals/iot-security-ground-up.md)
