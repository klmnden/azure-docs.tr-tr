---
title: IOT hub'ı oluşturmak için Azure portalını kullanma | Microsoft Docs
description: Nasıl oluşturmak, yönetmek ve Azure Portalı aracılığıyla Azure IOT hub'ları silin. Fiyatlandırma katmanlarına, ölçeklendirme, güvenlik ve yapılandırma Mesajlaşma hakkında bilgi içerir.
services: iot-hub
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: 0909cd2b-4c1e-49e0-b68a-75532caf0a6a
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/01/2018
ms.author: dobett
ms.openlocfilehash: 6ffde076caff6217bf6255c9294eca63d3e39b85
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="create-an-iot-hub-using-the-azure-portal"></a>Azure portalını kullanarak IOT hub oluşturma

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

Bu makalede açıklanır:

* IOT Hub hizmeti Azure Portalı'nda bulmak nasıl.
* Oluşturun ve IOT hub'ları yönetmek nasıl.

## <a name="where-to-find-the-iot-hub-service"></a>IOT Hub hizmeti nerede bulacağını

IOT hub'ı servis Portalı'nda aşağıdaki konumlarda bulabilirsiniz:

* Seçin **+ yeni**, ardından **nesnelerin interneti**.
* Marketi'ndeki seçin **nesnelerin interneti**.

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

IOT hub'ı aşağıdaki yöntemleri kullanarak oluşturabilirsiniz:

* **+ Yeni** seçeneği aşağıdaki ekran görüntüsünde gösterilen dikey penceresini açar. IOT hub'ı Marketi ve bu yöntem aracılığıyla oluşturma adımları aynıdır.
* Marketi'ndeki seçin **oluşturma** aşağıdaki ekran görüntüsünde gösterilen dikey penceresini açın.

Aşağıdaki bölümlerde, IOT hub'ı oluşturma adımları açıklanmaktadır:

### <a name="choose-the-name-of-the-iot-hub"></a>IOT hub adını seçin

IOT hub'ı oluşturmak için IOT hub'ı adı olmalıdır. Bu ad, tüm IOT hub'ları arasında benzersiz olması gerekir.

[!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

### <a name="choose-the-pricing-tier"></a>Fiyatlandırma katmanı seçin

Bağlı olarak kaç özellikleri birkaç katmanları seçebilirsiniz istiyorsanız ve kaç tane iletileri gönder günde çözümünüz aracılığıyla. Ücretsiz katmanı, sınama ve değerlendirme için tasarlanmıştır. Gün başına 8.000 ileti en fazla ve IOT hub'ına bağlı 500 cihazların sağlar. Her Azure aboneliği ücretsiz katmanında bir IOT hub'ı oluşturabilirsiniz. 

Diğer katmanı seçenekleri hakkında daha fazla ayrıntı için bkz: [sağ IOT hub'ı katmanı seçme özelliği](iot-hub-scaling.md).

### <a name="iot-hub-units"></a>IOT hub'ı birimleri

Gün başına birim başına izin verilen ileti sayısını, hub'ın fiyatlandırma katmanında bağlıdır. Örneğin, IOT hub'ı giriş 700.000 iletilerinin desteklemek için isterseniz, iki S1 katmanı birimleri seçin.

### <a name="device-to-cloud-partitions-and-resource-group"></a>Cihaz bulut bölümleri ve kaynak grubu

IOT hub'ı için bölümlerin sayısı değiştirebilirsiniz. Bölüm varsayılan sayısı 4, aşağı açılan listeden farklı bir numara seçebilirsiniz.

Açıkça bir boş bir kaynak grubu oluşturmanız gerekmez. Bir kaynak oluşturduğunuzda, yeni ya da seçin veya varolan bir kaynak grubunu kullanın.

![][5]

### <a name="choose-subscription"></a>Abonelik seçin

Azure IOT hub'ı otomatik olarak kullanıcı hesabı bağlandığı Azure aboneliklerini listeler. IOT hub'ına ilişkilendirmek için Azure aboneliği seçebilirsiniz.

### <a name="choose-the-location"></a>Konumu seçin

Konum seçeneği IOT hub'ı kullanılabilir olduğu bölgelerin bir listesi sağlar.

### <a name="create-the-iot-hub"></a>IOT hub oluşturma

Önceki adımların tümünü tamamlandığı zaman, IOT hub'ı oluşturabilirsiniz. Tıklatın **oluşturma** oluşturmak ve seçtiğiniz seçeneklerle IOT hub'ı dağıtmak için arka uç işlemini başlatmak üzere.

Süren uygun konuma sunucularda çalıştırmak için arka uç dağıtım için IOT hub'ı oluşturmak için birkaç dakika sürebilir.

## <a name="change-the-settings-of-the-iot-hub"></a>IOT hub'ının ayarlarını değiştirme

IOT Hub dikey penceresinden oluşturulduktan sonra var olan IOT hub'ı ayarlarını değiştirebilirsiniz.

![][8]

**Paylaşılan erişim ilkeleri**: IOT Hub'ına bağlanmak için gerekli izinlere cihazları ve Hizmetleri için bu ilkeleri tanımlar. Bu ilkeler tıklatarak erişebilirsiniz **paylaşılan erişim ilkeleri** altında **genel**. Bu dikey pencerede, varolan ilkeleri değiştirmek veya yeni bir ilke ekleme.

### <a name="create-a-policy"></a>İlke oluştur

* Tıklatın **Ekle** bir dikey penceresini açın. Burada yeni ilke adını ve aşağıdaki çizimde gösterildiği gibi bu ilke ile ilişkilendirmek istediğiniz izinleri girebilirsiniz:

    Bu paylaşılan ilkeleriyle ilişkilendirilebilir birkaç izinleri vardır. **Okuma kayıt defteri** ve **kayıt defteri yazma** ilkeleri kimlik kayıt defterini okuma ve yazma erişimi hakkı verin. Yazma seçeneği otomatik olarak seçme okuma seçeneğini seçer.

    **Service bağlanma** İlkesi hizmet uç noktaları gibi erişim izni verir **alma cihaz bulut**. **Aygıtı bağlayın** İlkesi IOT Hub cihaz tarafındaki uç kullanarak ileti gönderme ve alma için izinler verir.

* Tıklatın **oluşturma** bu yeni ilke varolan listesine oluşturulan eklemek için.

![][10]

## <a name="endpoints"></a>Uç Noktalar

Tıklatın **uç noktaları** değiştirdiğiniz IOT hub'ı uç noktaları listesini görüntülemek için. Uç noktaları iki tür vardır: IOT hub'ına yerleşik uç noktaları ve IOT hub'ına oluşturulduktan sonra eklediğiniz uç noktaları.

![][11]

### <a name="built-in-endpoints"></a>Yerleşik uç noktaları

İki yerleşik uç noktalar vardır: **aygıt geri bildirim buluta** ve **olayları**.

* **Cihaz geri bildirim buluta** ayarları: Bu ayar iki subsettings sahiptir: **aygıt TTL bulut** (time-to-live) ve **bekletme süresini** (saat) içindeki iletiler için. İlk IOT hub'ı oluşturduğunuzda, hem bu ayarları bir saatlik varsayılan değere sahip. Bu ayarları ayarlamak için kaydırma çubuklarını kullanın veya değerleri yazın.
* **Olayları** ayarları: Bu ayar salt okunur bazıları aşağıda verilen birkaç subsettings sahiptir. Aşağıdaki listede, bu ayarları açıklanmaktadır:

  * **Bölümler**: IOT hub'ı oluşturduğunuzda varsayılan bir değer ayarlanır. Bu ayar aracılığıyla bölüm sayısı değiştirebilirsiniz.

  * **Olay Hub ile uyumlu ada ve uç nokta**: zaman IOT hub'ı oluşturulur, bir olay hub'ı belirli koşullar altında erişimi gerekebilir dahili olarak oluşturulmuş. Event Hub ile uyumlu adı ve uç nokta değerleri özelleştiremezsiniz ancak tıklayarak kopyalayabilirsiniz **kopya**.

  * **Saklama süresi**: varsayılan olarak bir gün olarak ayarlayın ancak açılan listeyi kullanarak değiştirebilirsiniz. Bu değer, cihaz bulut ayarı için gün olur.

  * **Tüketici grupları**: tüketici grupları iletileri IOT hub'ından bağımsız olarak okumak birden çok okuyucular etkinleştirin. Her IOT hub ile varsayılan bir tüketici grubu oluşturulur. Ancak, eklemek veya bu ayarı kullanarak, IOT hub tüketici grupları silin.

  > [!NOTE]
  > Varsayılan bir tüketici grubu silinmiş veya düzenlenemez.

### <a name="custom-endpoints"></a>Özel uç noktalar

Portalı kullanarak, IOT hub'ına özel uç noktalar ekleyebilirsiniz. Gelen **uç noktaları** dikey penceresinde tıklatın **Ekle** açmak için üst **uç nokta ekleme** dikey. Gerekli bilgileri girin ve ardından **Tamam**. Özel uç noktanızı şimdi ana listelenen **uç noktaları** dikey.

![][13]

Daha fazla bilgiyi özel uç noktalarını hakkında [başvuru - IOT hub uç noktaları][lnk-devguide-endpoints].

## <a name="routes"></a>Yollar

Tıklatın **yollar** nasıl IOT Hub, cihaz bulut iletilerini gönderir yönetmek için.

![][14]

Tıklayarak IOT hub'ınıza yollar ekleyebilirsiniz **Ekle** en üstündeki **yollar*** gerekli bilgileri girerek ve tıklayarak dikey **Tamam**. Rota sonra ana listelenen **yollar** dikey. Bir rota yollar listesinde tıklayarak düzenleyebilirsiniz. Bir rota etkinleştirmek için yollar listesinde tıklatın ve ayarlamak **etkin** geç **devre dışı**. Değişikliği kaydetmek için tıklatın **Tamam** dikey pencerenin altındaki.

![][15]

## <a name="delete-the-iot-hub"></a>IOT hub'ını silmek

IOT hub'ı tıklatarak silmek gözatabilirsiniz **Gözat**ve silmek için uygun hub'ı seçerek. IOT hub'ını silmek için tıklatın **silmek** IOT hub'ı adı altındaki düğme.

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT hub'ı yönetme hakkında daha fazla bilgi için bu bağlantıları izleyin:

* [Toplu IOT cihazları yönetme][lnk-bulk]
* [IOT hub'ı ölçümleri][lnk-metrics]
* [İzleme işlemleri][lnk-monitor]

Daha fazla IOT hub'ı özelliklerini keşfetmek için bkz:

* [IOT Hub Geliştirici Kılavuzu][lnk-devguide]
* [Azure IOT Edge ile sınır cihazlarına Al dağıtma][lnk-iotedge]
* [IOT çözümünüzden zemin oluşturan güvenli][lnk-securing]

[4]: ./media/iot-hub-create-through-portal/create-iothub.png
[5]: ./media/iot-hub-create-through-portal/location1.png
[8]: ./media/iot-hub-create-through-portal/portal-settings.png
[10]: ./media/iot-hub-create-through-portal/shared-access-policies.png
[11]: ./media/iot-hub-create-through-portal/messaging-settings.png
[12]: ./media/iot-hub-create-through-portal/pricing-error.png
[13]: ./media/iot-hub-create-through-portal/endpoint-creation.png
[14]: ./media/iot-hub-create-through-portal/routes-list.png
[15]: ./media/iot-hub-create-through-portal/route-edit.png

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-securing]: iot-hub-security-ground-up.md
[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
