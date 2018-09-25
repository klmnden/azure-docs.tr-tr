---
title: IOT hub'ı oluşturmak için Azure portalını kullanma | Microsoft Docs
description: Nasıl-oluşturma, yönetme ve Azure IOT hub'ı Azure portalından silin. Fiyatlandırma katmanları için güvenlik, ölçeklendirme ve yapılandırma Mesajlaşma hakkında bilgi içerir.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 09/06/2018
ms.author: robinsh
ms.openlocfilehash: 8f08141f5c14a734f89ba91045767e2a36a44fd2
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46985614"
---
# <a name="create-an-iot-hub-using-the-azure-portal"></a>Azure portalını kullanarak IOT hub oluşturma

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

Bu makalede oluşturmak ve IOT hub'ları yönetmek nasıl [Azure portalında](https://portal.azure.com).

Bu öğreticideki adımları uygulayabilmeniz için bir Azure aboneliğinizin olması gerekir. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

1. [Azure Portal](https://portal.azure.com)’da oturum açın. 

2. Seçin +**kaynak Oluştur**, ardından **nesnelerin interneti**.

3. Tıklayın **IOT hub'ı** sağdaki listeden. IOT hub'ı oluşturmak için ilk ekran görürsünüz.

   ![Azure portalında bir hub'ı oluşturma gösteren ekran görüntüsü](./media/iot-hub-create-through-portal/iot-hub-create-screen-basics.png)

   Alanları doldurun.

   **Abonelik**: IOT hub'ınız için kullanılacak aboneliği seçin.

   **Kaynak grubu**: yeni bir kaynak grubu oluşturun veya var olanı kullanın. Yeni bir tane oluşturmak için tıklayın **Yeni Oluştur** ve kullanmak istediğiniz adı girin. Mevcut bir kaynak grubunu kullanmak için **var olanı kullan** ve açılır listeden kaynak grubunu seçin.

   **Bölge**: hub'ınıza açılır listeden yer almasını istediğiniz bölgeyi seçin.

   **IOT hub'ı adı**: IOT Hub'ınızın adını yerleştirin. Bu adın küresel olarak benzersiz olması gerekir. 

   [!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

4. Tıklayın **sonraki: boyut ve ölçek** sonraki ekrana gidin.

   ![Ayarı boyut ve ölçek için Azure portalını kullanarak yeni bir IOT hub'ı gösteren ekran görüntüsü](./media/iot-hub-create-through-portal/iot-hub-create-screen-size-scale.png)

   Bu ekranda varsayılan değerleri alabilir ve tıklamanız yeterli **gözden geçir + Oluştur** altındaki. Veya gerektiği gibi alanları doldurun.

   **Fiyatlandırma ve ölçek katmanı**: bağlı olarak kaç özellikleri birkaç katmanı arasından seçim yapabilirsiniz istediğiniz ve kaç iletileri gönder günde çözümünüz aracılığıyla. Ücretsiz katman, test ve değerlendirme için tasarlanmıştır. Günlük ileti 8000'en fazla ve IOT hub'ına bağlanması 500 CİHAZDAN sağlar. Her Azure aboneliğinin bir IOT Hub ücretsiz katmanında oluşturabilirsiniz. 

   **IOT Hub birimlerinin**: günlük birim başına izin verilen ileti sayısı, hub'ın fiyatlandırma katmanına bağlıdır. Örneğin, IOT hub'ı 700.000 iletilerinin giriş desteklemek için isterseniz, iki adet S1 katmanı birimi seçin.

   Bir katman seçenekleri hakkında daha fazla ayrıntı için bkz: [doğru IOT Hub katmanını seçme](iot-hub-scaling.md).

   **Gelişmiş / CİHAZDAN buluta bölümler**: Bu özellik CİHAZDAN buluta iletileri iletilerin eşzamanlı okuyucu sayısıyla ilgilidir. Çoğu IOT hub'ları, yalnızca dört bölüm gerekir. 

5. Tıklayın **gözden + Oluştur** seçimlerinizi gözden geçirmek için. Bu ekranda gösterilene benzer bir şey görürsünüz.

   ![Yeni IOT hub'ı oluşturmak için bilgileri gözden geçirdikten ekran görüntüsü](./media/iot-hub-create-through-portal/iot-hub-create-review.png)

5. Tıklayın **Oluştur** yeni IOT hub'ınızı oluşturmak için. Hub'ı oluşturuluyor, birkaç dakika sürer.

## <a name="change-the-settings-of-the-iot-hub"></a>IOT hub'ının ayarlarını değiştirme

IOT hub'ı bölmesinden oluşturulduktan sonra var olan IOT hub'ının ayarlarını değiştirebilirsiniz.

![IOT hub'ının ayarları gösteren ekran görüntüsü](./media/iot-hub-create-through-portal/iot-hub-settings-panel.png)

IOT hub'ı için ayarlayabileceğiniz özelliklerden bazıları şunlardır:

**Fiyatlandırma ve ölçek**: farklı bir katmana geçiş veya IOT Hub birimleri ayarlamak için bu özelliği kullanabilirsiniz. 

**İşlem izleme**: açma veya kapatma bir CİHAZDAN buluta iletileri veya Bulut-cihaz iletilerini ilgili olayları günlüğe kaydetme gibi farklı izleme kategorileri açın.

**IP Filtresi**: kabul edilecek veya IOT hub tarafından reddedilen bir IP adresi aralığı belirtin.

**Özellikler**: kopyalayın ve başka bir yerde, kaynak kimliği, kaynak grubu, konum ve benzeri gibi kullanmak özelliklerinin listesini sağlar.

### <a name="shared-access-policies"></a>Paylaşılan erişim ilkeleri

Ayrıca görüntülemek veya paylaşılan erişim ilkeleri listesinde tıklayarak değiştirmek **paylaşılan erişim ilkeleri** içinde **ayarları** bölümü. Bu ilkeler, IOT Hub'ına bağlanmak için cihazlar ve hizmetler için izinler tanımlayın. 

Tıklayın **Ekle** açmak için **bir paylaşılan erişim ilkesi ekleme** dikey penceresi.  Yeni ilke adı ve aşağıdaki resimde gösterildiği gibi bu ilke ile ilişkilendirmek istediğiniz izinleri girebilirsiniz:

![Paylaşılan erişim ilkesi ekleme gösteren ekran görüntüsü](./media/iot-hub-create-through-portal/iot-hub-add-shared-access-policy.png)

* **Kayıt defterini oku** ve **kayıt defteri yazma** ilkeleri kimlik kayıt defterini okuma ve yazma erişimi hakkı verin. Otomatik olarak yazma seçeneğini seçerken, salt okunur seçeneğini seçer.

* **Hizmetini bağlama** ilkesinin hizmet uç noktaları gibi erişim izni verdiği **CİHAZDAN buluta alma**. 

* **Cihazı bağlayın** İlkesi, IOT Hub cihaz tarafındaki uç noktalarda kullanarak ileti gönderme ve alma için izinler verir.

Tıklayın **Oluştur** Bu ilke mevcut listesine yeni oluşturulan eklemek için.

## <a name="message-routing-for-an-iot-hub"></a>Bir IOT hub ileti yönlendirme

Tıklayın **ileti yönlendirme** altında **Mesajlaşma** tanımladığınız yolları ve özel uç noktaları için hub ileti yönlendirme bölmesini görmek için. [İleti yönlendirme](iot-hub-devguide-messages-d2c.md) veriler için uç noktalarınız cihazlarınızdan nasıl gönderilir yönetmenizi sağlar. İlk adım, yeni bir yol eklemektir. Ardından yol mevcut uç nokta ekleme veya, blob depolama gibi desteklenen türleri için yeni bir tane oluşturun. 

![İleti yönlendirme bölmesi](./media/iot-hub-create-through-portal/iot-hub-message-routing.png)

### <a name="routes"></a>Yollar

İlk ileti yönlendirme bölmesinde sekmesinde rotalarıyla kıyaslandığında. Yeni bir yol eklemek için +**Ekle**. Aşağıdaki ekranı görürsünüz. 

![Yeni bir yol ekleme gösteren ekran görüntüsü](./media/iot-hub-create-through-portal/iot-hub-add-route-storage-endpoint.png)

Hub'ınızı adlandırın. Ad, hub için rotalar listesi içinde benzersiz olmalıdır. 

İçin **uç nokta**, aşağı açılan listeden seçin veya yeni bir tane ekleyin. Bu örnekte, bir depolama hesabı ve kapsayıcı zaten var. Bunları bir uç nokta eklemek için +**Ekle** seçin ve uç nokta açılır yanındaki **Blob Depolama**. Depolama hesabı ve kapsayıcı burada belirtilen aşağıdaki ekran gösterilir.

![Yönlendirme kuralı için bir depolama uç noktası ekleme gösteren ekran görüntüsü](./media/iot-hub-create-through-portal/iot-hub-routing-add-storage-endpoint.png)

Tıklayın **bir kapsayıcı seçin** depolama hesabı ve kapsayıcı seçin. Bu alanları seçtikten sonra uç nokta bölmesine döndürür. Geri kalan alanları için varsayılan ayarları kullanın ve **Oluştur** uç nokta için depolama hesabı oluşturma ve yönlendirme kuralları ekleyin.

İçin **veri kaynağı**, cihazın Telemetri iletilerini seçin. 

Ardından, bir yönlendirme sorgu ekleyin. Bu örnekte, adlı bir uygulama özelliğini bulunduran iletileri `level` eşit bir değer ile `critical` depolama hesabına yönlendirilir.

![Yeni bir yönlendirme kuralı kaydediliyor gösteren ekran görüntüsü](./media/iot-hub-create-through-portal/iot-hub-add-route.png)

Tıklayın **Kaydet** yönlendirme kuralını kaydetmek için. İleti yönlendirme bölmesine dönün ve yeni yönlendirme kuralınızı görüntülenir.

### <a name="custom-endpoints"></a>Özel uç noktalar

Tıklayın **özel uç noktalar** sekmesi. Önceden oluşturulmuş özel uç noktalar görürsünüz. Buradan yeni uç nokta ekleyin veya mevcut uç noktalarını silmelisiniz. 

> [!NOTE]
> Bir rota silerseniz, bu yol için atanmış uç noktaları silmez. Uç noktayı silmek için özel uç noktaları sekmesine tıklayın, silmek istediğiniz uç noktayı seçin ve Sil'e tıklayın.
>

Daha fazla bilgi edinebilirsiniz özel uç noktalar hakkında [başvuru - IOT hub uç noktaları](iot-hub-devguide-endpoints.md).

IOT hub'ı için en fazla 10 özel uç noktalar tanımlayabilirsiniz. 

Özel uç noktalar ile yönlendirme kullanma tam bir örneğini görmek için bkz: [ileti IOT Hub ile yönlendirme](tutorial-routing.md).

## <a name="find-a-specific-iot-hub"></a>Belirli bir IOT hub'ı bulun

Aboneliğinizde belirli bir IOT hub'ı bulmak için iki yolu vardır:

1. Kaynak grubunda IOT hub'a ait olduğu biliyorsanız **kaynak grupları**, ardından listeden kaynak grubunu seçin. Kaynak grubu ekran IOT hub'ları da dahil olmak üzere o grubun tüm kaynakları gösterir. İçin aradığınız hub'ı tıklatın.

2. Tıklayın **tüm kaynakları**. Üzerinde **tüm kaynakları** bölmesinde, varsayılan olarak bir açılan listedeki yoktur `All types`. Açılan listeye tıklayın, onay kutusunu temizleyin `Select all`. Bulma `IoT Hub` ve yanıtı denetleyecektir. Açılan liste kutusunu kapatmak için tıklayın ve yalnızca IOT hub'ı gösteren girişler filtrelenir.

## <a name="delete-the-iot-hub"></a>IOT hub'ını Sil

IOT hub'ı silmek için Sil'a tıklayın, istediğiniz IOT hub'ı bulun **Sil** düğmesinin altında IOT hub adı.

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT hub'ı yönetme hakkında daha fazla bilgi için bu bağlantıları izleyin:

* [IOT Hub ile ileti yönlendirme](tutorial-routing.md)
* [IOT hub'ı ölçümleri](iot-hub-metrics.md)
* [İşlemleri izleme](iot-hub-operations-monitoring.md)