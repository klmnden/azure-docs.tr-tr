---
title: Dağıtmak, izlemek modülleri için Azure IOT kenar | Microsoft Docs
description: Edge cihazlarda çalışan modülleri yönetme
keywords: ''
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/07/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: be52a57f10f286bded9a31d84b36a49717b94006
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37029766"
---
# <a name="deploy-and-monitor-iot-edge-modules-at-scale-using-the-azure-portal"></a>Dağıtma ve IOT kenar modülleri Azure portalını kullanarak ölçekte izleme

[!INCLUDE [iot-edge-how-to-deploy-monitor-selector](../../includes/iot-edge-how-to-deploy-monitor-selector.md)]

Azure IOT kenar kenarına analytics taşımanızı sağlar ve böylece yönetmek ve her biri fiziksel olarak erişmek zorunda kalmadan, IOT sınır cihazları izlemek bulut arabirimi sağlar. Cihazları uzaktan yönetmek için nesnelerin interneti çözümleri daha büyük ve daha karmaşık büyüyor giderek önemli bir özelliktir. Azure IOT kenar eklediğiniz kaç cihaz olsun, iş hedeflerinize destekleyecek şekilde tasarlanmıştır.

Tek bir cihazı yönetebilir ve modülleri dağıtmadan birer birer. Ancak, büyük ölçekli aygıtları değişiklik yapmak istiyorsanız, oluşturabileceğiniz bir **IOT kenar otomatik dağıtım**, IOT Hub'ındaki otomatik aygıt yönetiminin bir parçası olduğu. Dağıtımlar, birden fazla modülü aynı anda birden çok cihazlara dağıtma, durum ve modülleri sağlığını izlemek ve gerektiğinde değişiklikleri yapmanıza olanak sağlayan dinamik işlemlerdir. 

## <a name="identify-devices-using-tags"></a>Etiketleri kullanarak cihazları belirlemek

Bir dağıtımı oluşturmadan önce hangi cihazların etkilenmesini istediğiniz belirtmek kullanabilmek için gerekir. Azure IOT kenar tanımlayan kullanarak cihazları **etiketleri** cihaz çiftine. Her cihazı birden fazla etikete sahip olabilir ve çözümünüz için anlamlı herhangi bir şekilde tanımlayabilirsiniz. Örneğin, akıllı bina kampüs yönetiyorsanız, bir cihaza aşağıdaki etiketlerin ekleyebilirsiniz:

```json
"tags":{
    "location":{
        "building": "20",
        "floor": "2"
    },
    "roomtype": "conference",
    "environment": "prod"
}
```

Cihaz çiftlerini ve etiketleri hakkında daha fazla bilgi için bkz: [IOT hub'ı Anlama ve Kullan cihaz çiftlerini][lnk-device-twin].

## <a name="create-a-deployment"></a>Bir dağıtımı oluşturma

1. İçinde [Azure portal][lnk-portal], IOT hub'ınızı gidin. 
1. Seçin **IOT kenar**.
1. Seçin **IOT kenar dağıtımını Ekle**.

Bir dağıtım oluşturmak için beş adım vardır. Aşağıdaki bölümlerde her birini yol. 

### <a name="step-1-name-and-label"></a>1. adım: Ad ve etiket

1. Dağıtımınızı en fazla 128 küçük harf olan benzersiz bir ad verin. Boşluk ve şu geçersiz karakterlerden kaçının: `& ^ [ ] { } \ | " < > /`.
1. Dağıtımlarınızı izlemenize yardımcı olması için etiketler ekleyin. Etiketler **adı**, **değeri** dağıtımınızı tanımlamak çiftleri. Örneğin, `HostPlatform, Linux` veya `Version, 3.0.1`.
1. Seçin **sonraki** iki adımına geçmek için. 

### <a name="step-2-add-modules-optional"></a>2. adım: (İsteğe bağlı) modülleri ekleme

İki tür bir dağıtıma eklediğiniz modülü vardır. İlk Depolama hesabı veya akış analizi gibi bir Azure hizmet dışına dayalı bir modüldür. İkinci kendi kodunuzu dışına dayalı bir modüldür. Bir dağıtım için her iki türdeki birden çok modüller ekleyebilirsiniz. 

Bir dağıtım hiçbir modüllerle oluşturursanız, herhangi bir geçerli modül aygıtlardan kaldırır. 

>[!NOTE]
>Azure Machine Learning ve Azure işlevleri otomatik Azure hizmet dağıtımı henüz desteklemiyoruz. Bu hizmetlerin dağıtımınız için el ile eklemek için özel modülü dağıtımı kullanın. 

Azure Stream Analytics bir modül eklemek için aşağıdaki adımları izleyin:
1. İçinde **dağıtım modülleri** bölüm sayfasında, tıklatın **Ekle**.
1. Seçin **Azure akış analizi Modülü**.
1. Seçin, **abonelik** açılır menüsünden.
1. Seçin, **kenar iş** açılır menüsünden.
1. Seçin **kaydetmek** modülünüzün dağıtıma eklenecek. 

Bir modül olarak özel kod eklemek veya bir Azure hizmeti modülü el ile eklemek için aşağıdaki adımları izleyin:
1. İçinde **kayıt defteri ayarları** bölüm sayfasında, bu dağıtım için modülü görüntüleri içeren tüm özel kapsayıcı kayıt defterleri için adları ve kimlik bilgilerini sağlayın. Docker görüntü için contrainer kayıt defteri kimlik bilgisi bulamazsa kenar Aracısı hata 500 rapor eder.
1. İçinde **dağıtım modülleri** bölüm sayfasında, tıklatın **Ekle**.
1. Seçin **IOT kenar Modülü**.
1. Modülünüzün vermek bir **adı**.
1. İçin **görüntü URI** alanında, kapsayıcı görüntü modülünüzün için girin. 
1. Herhangi belirtmek **kapsayıcı oluşturma seçenekleri** , bayraklarıdır kapsayıcıya. Daha fazla bilgi için bkz: [docker oluşturma][lnk-docker-create].
1. Seçmek için aşağı açılan menüsünü kullanın bir **yeniden İlkesi**. Aşağıdaki seçeneklerden birini seçin: 
   * **Her zaman** -herhangi bir nedenle kapanması durumunda modülünü her zaman yeniden başlatılır.
   * **Hiçbir zaman** -herhangi bir nedenle kapanması durumunda modülünü başlatmaz.
   * **Üzerinde başarısız oldu-** -düzgün bir şekilde kapatır değil, ancak bu, çökmesi durumunda modülünü yeniden başlatır. 
   * **Üzerinde-sağlıksız** -kilitlenmesine veya sağlıksız durum döndürür, modül yeniden başlatır. Bu sistem durumu işlevi uygulamak için her modül için hazır. 
1. Seçmek için aşağı açılan menüsünü kullanın **istenen durum** modülü için. Aşağıdaki seçeneklerden birini seçin:
   * **Çalışan** -bu varsayılan seçenektir. Modül, hemen dağıtılan sonra çalışmaya başlar.
   * **Durdurulmuş** -dağıtılan sonra modülün bağlı olarak adlandırılan siz veya başka bir modül tarafından başlatmaya kadar boşta kalır.
1. Seçin **etkinleştirmek** herhangi bir etiket veya istenen özellikleri modülü çifti eklemek istiyorsanız. 
1. Girin **ortam değişkenleri** bu modül için. Ortam değişkenleri bir modül yapılandırma kolaylaştırmak için ek bilgileri sağlayın.
1. Seçin **kaydetmek** modülünüzün dağıtıma eklenecek. 

Yapılandırılmış bir dağıtım için tüm modüllerin olduktan sonra Seç **sonraki** üç adımına geçmek için.

### <a name="step-3-specify-routes-optional"></a>3. adım: (İsteğe bağlı) rotaları belirtin

Modüller birbirleri ile dağıtım içinde iletişim kurma biçimini yolları tanımlayın. Sihirbaz size varsayılan olarak bir yol adı **rota** ve olarak tanımlanan **FROM /* upstream $ **, herhangi bir modül tarafından çıkış herhangi bir ileti IOT hub'ına gönderilen anlamına gelir.  

Ekleme veya yolları veriler ile güncelleştirecek [bildirme yolları](module-composition.md#declare-routes)seçeneğini belirleyip **sonraki** gözden geçirme bölümüne devam etmek için.


### <a name="step-4-target-devices"></a>Adım 4: Hedef cihazlar

Bu dağıtım alması gereken belirli cihazları hedeflemek için aygıtlardan etiketleri özelliğini kullanın. 

Birden çok dağıtım aynı aygıt hedefleyebilir olduğundan, her bir dağıtım bir öncelik numarası vermesi gerekir. Şimdiye kadar bir çakışma varsa, (yüksek değerleri daha yüksek öncelik gösterir) en yüksek öncelikli dağıtım kazanır. İki dağıtım aynı öncelik numarasını varsa, çoğu oluşturulmuş bir son kazanır. 

1. Dağıtım için pozitif bir tamsayı girin **öncelik**. İki veya daha fazla dağıtım aynı aygıtta hedeflenen gelmesi durumunda, en yüksek sayısal değer ile dağıtım önceliği geçerli olur.
1. Girin bir **hedef koşulu** hangi aygıtların bu dağıtım ile hedeflenir belirlemek için. Koşul cihaz çifti etiketlere göre veya cihaz çifti özellikleri istenen ve ifade biçimi eşleşmelidir. Örneğin, `tags.environment='test'` veya `properties.desired.devicemodel='4000x'`. 
1. Seçin **sonraki** son adımına geçmek için.

### <a name="step-5-review-template"></a>5. adım: Gözden geçirme şablonu

Dağıtım bilgileri gözden geçirin ve ardından **gönderme**.

## <a name="monitor-a-deployment"></a>Bir dağıtımını izleme

Bir dağıtımın ayrıntılarını görüntülemek ve onu çalıştıran aygıtları izlemek için aşağıdaki adımları kullanın:

1. Oturum [Azure portal] [ lnk-portal] ve IOT hub'ına gidin. 
1. Seçin **IOT kenar**.
1. Seçin **IOT kenar dağıtımları**. 

   ![IOT kenar dağıtımları görüntüle][1]

1. Dağıtım listesini inceleyin. Her dağıtım için aşağıdaki ayrıntıları görüntüleyebilirsiniz:
   * **Kimliği** -dağıtımın adını.
   * **Hedef koşulu** -hedeflenen cihazlar tanımlamak için kullanılan etiketi.
   * **Öncelik** -dağıtıma atanan öncelik numarası.
   * **Sistem ölçümleri** - **hedeflenen** hedefleme koşuluyla eşleşen IOT hub'da cihaz çiftlerini sayısını belirtir ve **uygulanan** olan aygıtların sayısını belirtir Dağıtım içeriğini kendi modülü çiftlerini IOT Hub'ındaki uygulanan. 
   * **Cihaz ölçümleri** -sınır cihazları başarı veya IOT kenar istemci çalışma zamanı hatalarını raporlama dağıtım sayısı.
   * **Oluşturulma zamanı** -dağıtım oluşturulduğu gelen zaman damgası. Bu zaman damgası iki dağıtım aynı önceliğe sahip olduğunuzda TIES ayırmak için kullanılır. 
2. İzlemek istediğiniz dağıtımı seçin.  
3. Dağıtım ayrıntıları inceleyin. Dağıtım ayrıntılarını gözden geçirmek için sekmeleri kullanabilirsiniz.

## <a name="modify-a-deployment"></a>Bir dağıtım değiştirme

Bir dağıtım değişiklik yaptığınızda, değişiklikler hemen tüm hedeflenen cihazlar için çoğaltılır. 

Hedef durumu güncelleştirirseniz, aşağıdaki güncelleştirmeleri oluşur:
* Bir aygıt, eski hedef durumu karşılamadı, ancak yeni hedef koşulunu ve en yüksek öncelikli cihaz için bu dağıtım ise, bu dağıtım cihaza uygulanır. 
* Şu anda bu dağıtım artık çalıştıran bir cihazda hedef koşulu karşılıyorsa, bu dağıtım kaldırır ve sonraki en yüksek öncelik dağıtımı alır. 
* Şu anda bu dağıtım artık çalıştıran bir cihazda hedef koşulunu ve diğer dağıtımlar hedef koşulu karşılamıyor, hiçbir değişiklik cihazda oluşur. Aygıt geçerli modülleri kendi geçerli durumunda çalışmaya devam eder, ancak artık bu dağıtımın bir parçası olarak yönetilmez. Diğer dağıtım hedef koşulunu sonra bu dağıtım kaldırır ve yeni alır. 

Bir dağıtım değiştirmek için aşağıdaki adımları kullanın: 

1. Oturum [Azure portal] [ lnk-portal] ve IOT hub'ına gidin. 
1. Seçin **IOT kenar**.
1. Seçin **IOT kenar dağıtımları**. 

   ![IOT kenar dağıtımları görüntüle][1]

1. Değiştirmek istediğiniz dağıtımı seçin. 
1. Güncelleştirmeleri aşağıdaki alanları olun: 
   * Hedef durumu 
   * Etiketler 
   * Öncelik 
1. **Kaydet**’i seçin.
1. Adımları [bir dağıtımını izleme] [ anchor-monitor] dağıtmadan değişiklikleri izlemek için. 

## <a name="delete-a-deployment"></a>Bir dağıtımı silin

Bir dağıtım sildiğinizde, tüm cihazlar üzerinde kendi sonraki en yüksek öncelikli dağıtım alın. Ardından aygıtlarınızı başka bir dağıtım hedef koşulu karşılamıyorsa, dağıtım silindiğinde modülleri kaldırılmaz. 

1. Oturum [Azure portal] [ lnk-portal] ve IOT hub'ına gidin. 
1. Seçin **IOT kenar**.
1. Seçin **IOT kenar dağıtımları**. 

   ![IOT kenar dağıtımları görüntüle][1]

1. Silmek istediğiniz dağıtım seçmek için onay kutusunu kullanın. 
1. **Sil**’i seçin.
1. Bir istem Bu eylem bu dağıtımı silin ve tüm aygıtlar için önceki duruma dönmek size bildirir.  Bu, daha düşük öncelikli bir dağıtım uygulayacağı anlamına gelir.  Başka bir dağıtım hedefliyorsa, modülünüz kaldırılır. Bir dağıtım sıfır modüllerle cihazınızdan tüm modülleri kaldırmak ve aynı cihazlara dağıtmak istiyorsanız. Seçin **Evet** devam etmek için. 

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinmek [modülleri kenar cihazlara dağıtma][lnk-deployments].

<!-- Images -->
[1]: ./media/how-to-deploy-monitor/iot-edge-deployments.png

<!-- Links -->
[lnk-device-twin]: ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-portal]: https://portal.azure.com
[lnk-docker-create]: https://docs.docker.com/engine/reference/commandline/create/
[lnk-deployments]: module-deployment-monitoring.md

<!-- Anchor links -->
[anchor-monitor]: #monitor-a-deployment
