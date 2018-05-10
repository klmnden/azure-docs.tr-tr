---
title: Dağıtmak, izlemek modülleri için Azure IOT kenar | Microsoft Docs
description: Edge cihazlarda çalışan modülleri yönetme
services: iot-edge
keywords: ''
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 12/07/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 6d024dfdd661d6bebe7d163b96659d6e169cc5cc
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="deploy-and-monitor-iot-edge-modules-at-scale---preview"></a>Dağıtma ve IOT kenar modülleri ölçekte izleme - Önizleme

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
1. Seçin **IOT kenar (Önizleme)**.
1. Seçin **IOT kenar dağıtımını Ekle**.

Bir dağıtım oluşturmak için beş adım vardır. Aşağıdaki bölümlerde her birini yol. 

### <a name="step-1-name-and-label"></a>1. adım: Ad ve etiket

1. Dağıtımınızı benzersiz bir ad verin. Boşluk ve şu geçersiz karakterlerden kaçının: `& ^ [ ] { } \ | " < > /`.
1. Dağıtımlarınızı izlemenize yardımcı olması için etiketler ekleyin. Etiketler **adı**, **değeri** dağıtımınızı tanımlamak çiftleri. Örneğin, `HostPlatform, Linux` veya `Version, 3.0.1`.
1. Seçin **sonraki** iki adımına geçmek için. 

### <a name="step-2-add-modules-optional"></a>2. adım: (İsteğe bağlı) modülleri ekleme

İki tür bir dağıtıma eklediğiniz modülü vardır. İlk Depolama hesabı veya akış analizi gibi bir Azure hizmet dışına dayalı bir modüldür. İkinci kendi kodunuzu dışına dayalı bir modüldür. Bir dağıtım için her iki türdeki birden çok modüller ekleyebilirsiniz. 

Bir dağıtım hiçbir modüllerle oluşturursanız, var olan tüm modülleri aygıtlardan kaldırır. 

>[!NOTE]
>Azure Machine Learning ve Azure işlevleri otomatik Azure hizmet dağıtımı henüz desteklemiyoruz. Bu hizmetlerin dağıtımınız için el ile eklemek için özel modülü dağıtımı kullanın. 

Azure Stream Analytics bir modül eklemek için aşağıdaki adımları izleyin:
1. Seçin **alma Azure Stream Analytics IOT kenar Modülü**.
1. Dağıtmak istediğiniz Azure hizmet örnekleri seçmek için aşağı açılır menüler kullanın.
1. Seçin **kaydetmek** modülünüzün dağıtıma eklenecek. 

Bir modül olarak özel kod eklemek veya bir Azure hizmeti modülü el ile eklemek için aşağıdaki adımları izleyin:
1. **IoT Edge modülü ekle**’yi seçin.
1. Modülünüzün vermek bir **adı**.
1. İçin **görüntü URI** alanında, Docker kapsayıcısı görüntü modülünüzün için girin. 
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
1. Seçin **kaydetmek** modülünüzün dağıtıma eklenecek. 

Yapılandırılmış bir dağıtım için tüm modüllerin olduktan sonra Seç **sonraki** üç adımına geçmek için.

### <a name="step-3-specify-routes-optional"></a>3. adım: (İsteğe bağlı) rotaları belirtin

Modüller birbirleri ile dağıtım içinde iletişim kurma biçimini yolları tanımlayın. Dağıtımınız için herhangi bir yol belirtin ve ardından **sonraki** dört adımına geçmek için. 

### <a name="step-4-target-devices"></a>Adım 4: Hedef cihazlar

Bu dağıtım alması gereken belirli cihazları hedeflemek için aygıtlardan etiketleri özelliğini kullanın. 

Birden çok dağıtım aynı aygıt hedefleyebilir olduğundan, her bir dağıtım bir öncelik numarası vermesi gerekir. Şimdiye kadar bir çakışma varsa, en yüksek öncelikli dağıtım kazanır. İki dağıtım aynı öncelik numarasını varsa, çoğu oluşturulmuş bir son kazanır. 

1. Dağıtım için pozitif bir tamsayı girin **öncelik**.
1. Girin bir **hedef koşulu** hangi aygıtların bu dağıtım ile hedeflenir belirlemek için. Koşul cihaz çifti etiketlere göre ve ifade biçimi eşleşmesi gerekir. Örneğin, `tags.environment='test'`. 
1. Seçin **sonraki** son adımına geçmek için.

### <a name="step-5-review-template"></a>5. adım: Gözden geçirme şablonu

Dağıtım bilgileri gözden geçirin ve ardından **gönderme**.

## <a name="monitor-a-deployment"></a>Bir dağıtımını izleme

Bir dağıtımın ayrıntılarını görüntülemek ve onu çalıştıran aygıtları izlemek için aşağıdaki adımları kullanın:

1. Oturum [Azure portal] [ lnk-portal] ve IOT hub'ına gidin. 
1. Seçin **IOT kenar (Önizleme)**.
1. Seçin **IOT kenar dağıtımları**. 

   ![IOT kenar dağıtımları görüntüle][1]

1. Dağıtım listesini inceleyin. Her dağıtım için aşağıdaki ayrıntıları görüntüleyebilirsiniz:
   * **Kimliği** -dağıtımın adını.
   * **Hedef koşulu** -hedeflenen cihazlar tanımlamak için kullanılan etiketi.
   * **Öncelik** -dağıtıma atanan öncelik numarası.
   * **IOT kenar aracı durumu** -dağıtım ve bunların sistem sağlığı durumları alınan cihaz sayısı. 
   * **Sağlıksız modülleri** -hatalarını raporlama dağıtım modülleri sayısı. 
   * **Oluşturulma zamanı** -dağıtım oluşturulduğu gelen zaman damgası. Bu zaman damgası iki dağıtım aynı önceliğe sahip olduğunuzda TIES ayırmak için kullanılır. 
1. İzlemek istediğiniz dağıtımı seçin.  
1. Dağıtım ayrıntıları inceleyin. Dağıtım alınan aygıtları belirli ayrıntılarını görüntülemek için sekmeleri kullanabilirsiniz: 
   * **Hedeflenen** -hedef koşuluyla eşleşen sınır cihazları. 
   * **Uygulanan** - daha yüksek öncelikli başka bir dağıtım tarafından hedeflenen değil sınır cihazları hedeflenen. Bunlar dağıtım aldığınız aygıtlardır. 
   * **Başarı bildirimi** - modülleri başarıyla dağıtıldı hizmete geri bildirilen sınır cihazları uygulanır. 
   * **Hata Raporlama** - uygulanan sınır cihazları rapor geri hizmeti bir veya daha fazla modülleri başarıyla dağıtıldı değil. Daha fazla hata araştırmak için bu cihazlar için uzaktan bağlanmak ve günlük dosyalarını görüntülemek gerekir. 
   * **Sağlıksız modülleri raporlama** - uygulanan sınır cihazları rapor geri hizmeti bir veya daha fazla modülleri başarıyla dağıtıldı, ancak şimdi hata raporlama. 

## <a name="modify-a-deployment"></a>Bir dağıtım değiştirme

Bir dağıtım değişiklik yaptığınızda, değişiklikler hemen tüm hedeflenen cihazlar için çoğaltılır. 

Hedef durumu güncelleştirirseniz, aşağıdaki güncelleştirmeleri oluşur:
* Bir aygıt, eski hedef durumu karşılamadı, ancak yeni hedef koşulunu ve en yüksek öncelikli cihaz için bu dağıtım ise, bu dağıtım cihaza uygulanır. 
* Şu anda bu dağıtım artık çalıştıran bir cihazda hedef koşulu karşılıyorsa, bu dağıtım kaldırır ve sonraki en yüksek öncelik dağıtımı alır. 
* Şu anda bu dağıtım artık çalıştıran bir cihazda hedef koşulunu ve diğer dağıtımlar hedef koşulu karşılamıyor, hiçbir değişiklik cihazda oluşur. Aygıt geçerli modülleri kendi geçerli durumunda çalışmaya devam eder, ancak artık bu dağıtımın bir parçası olarak yönetilmez. Diğer dağıtım hedef koşulunu sonra bu dağıtım kaldırır ve yeni alır. 

Bir dağıtım değiştirmek için aşağıdaki adımları kullanın: 

1. Oturum [Azure portal] [ lnk-portal] ve IOT hub'ına gidin. 
1. Seçin **IOT kenar (Önizleme)**.
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
1. Seçin **IOT kenar (Önizleme)**.
1. Seçin **IOT kenar dağıtımları**. 

   ![IOT kenar dağıtımları görüntüle][1]

1. Silmek istediğiniz dağıtım seçmek için onay kutusunu kullanın. 
1. **Sil**’i seçin.
1. Bir istem Bu eylem bu dağıtımı silin ve tüm aygıtlar için önceki duruma dönmek size bildirir.  Bu, daha düşük öncelikli bir dağıtım uygulayacağı anlamına gelir.  Başka bir dağıtım hedefliyorsa, modülünüz kaldırılır. Müşteriler, bunu yapmak isterseniz, sıfır modüllerle bir dağıtımını oluşturun ve aynı cihazlara dağıtmak ihtiyaç duyar. Seçin **Evet** devam etmek istiyorsanız. 

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
