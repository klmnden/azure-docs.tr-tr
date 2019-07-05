---
title: Azure portalından - Azure IOT Edge otomatik dağıtımlar oluşturmayı | Microsoft Docs
description: Cihazları IOT Edge grupları için otomatik dağıtımlar oluşturmak için Azure portalını kullanma
keywords: ''
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/17/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 83e2490821f59adeb37958c6c31403121a40274e
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67540895"
---
# <a name="deploy-and-monitor-iot-edge-modules-at-scale-using-the-azure-portal"></a>Dağıtma ve Azure portalını kullanarak ölçekte IOT Edge modülleri izleme

Oluşturma bir **IOT Edge otomatik dağıtım** tek seferde birçok cihaz için devam eden dağıtımları yönetmek için Azure portalında. IOT Edge için otomatik dağıtımlar parçası olan [otomatik cihaz Yönetimi](/azure/iot-hub/iot-hub-automatic-device-management) IOT hub'ı özelliğidir. Birden çok cihaz için birden çok modül dağıtma, sistem modüllerinin izlemek ve gerektiğinde değişiklik olanak sağlayan dinamik işlemler dağıtımlarıdır. 

Daha fazla bilgi için [otomatik dağıtımlar tek tek cihazlarda veya uygun ölçekte IOT Edge anlamak](module-deployment-monitoring.md).

## <a name="identify-devices-using-tags"></a>Etiketleri kullanarak cihazları belirleyin

Bir dağıtımı oluşturmadan önce değiştirmek istediğiniz hangi cihazların belirtebilmek sahip. Azure IOT Edge kullanarak cihazları tanımlar **etiketleri** cihaz ikizinde. Her cihaz, çözümünüz için mantıklı olan herhangi bir şekilde tanımlayan birden çok etiketi olabilir. Örneğin, bir akıllı binalar, kampüs yönetiyorsanız, bir cihaza aşağıdaki etiketler ekleyebilirsiniz:

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

Cihaz ikizleri ve etiketleri hakkında daha fazla bilgi için bkz: [IOT hub'daki cihaz ikizlerini kavrama ve kullanma](../iot-hub/iot-hub-devguide-device-twins.md).

## <a name="create-a-deployment"></a>Bir dağıtım oluşturun

1. İçinde [Azure portalında](https://portal.azure.com), IOT hub'ınıza gidin. 
1. Seçin **IOT Edge**.
1. Seçin **IOT Edge dağıtımı Ekle**.

Bir dağıtımı oluşturmak için beş adım vardır. Aşağıdaki bölümlerde, her birini yol. 

### <a name="step-1-name-and-label"></a>1\. adım: Ad ve etiket

1. Dağıtımınızı en çok 128 küçük harf olan benzersiz bir ad verin. Boşluk ve şu geçersiz karakterlerden kaçının: `& ^ [ ] { } \ | " < > /`.
1. Dağıtımlarınızı izlenmesine yardımcı olması için anahtar-değer çiftleri olarak etiketler ekleyebilirsiniz. Örneğin, **HostPlatform** ve **Linux**, veya **sürüm** ve **3.0.1**.
1. Seçin **sonraki** iki adıma geçmeye. 

### <a name="step-2-add-modules-optional"></a>2\. adım: (İsteğe bağlı) Modül Ekle

Bir dağıtım için en fazla 20 modüller ekleyebilirsiniz. 

Hiçbir modül olmadan bir dağıtım oluşturursanız, hedef cihazları geçerli modüllerin kaldırır. 

Azure Stream Analytics'ten bir modül eklemek için aşağıdaki adımları izleyin:

1. İçinde **dağıtım modülleri** Bölümü sayfasına tıklayın **Ekle**.
1. Seçin **Azure Stream Analytics Modülü**.
1. Seçin, **abonelik** aşağı açılan menüden.
1. IOT seçin **Edge işi** aşağı açılan menüden.
1. Seçin **Kaydet** modülünüzde dağıtıma eklenecek. 

Bir modül olarak özel kod ekleyin veya bir Azure hizmeti modülü el ile eklemek için şu adımları izleyin:

1. İçinde **kapsayıcı kayıt defteri ayarları** bölüm sayfasında, bu dağıtım için modül görüntüleri içeren herhangi bir özel kapsayıcı kayıt defterleri için adları ve kimlik bilgilerini sağlayın. Kapsayıcı kayıt defteri kimlik bilgisi için bir Docker görüntüsü bulamazsanız, IOT Edge Aracısı 500 hata rapor eder.
1. İçinde **dağıtım modülleri** Bölümü sayfasına tıklayın **Ekle**.
1. Seçin **IOT Edge Modülü**.
1. Modülünüzün vermek bir **adı**.
1. İçin **görüntü URI'si** modülünüzde için kapsayıcı görüntüsü girin. 
1. Belirtmek **kapsayıcı oluşturma seçenekleri** geçirilecek kapsayıcıya. Daha fazla bilgi için [docker oluşturma](https://docs.docker.com/engine/reference/commandline/create/).
1. Seçmek için açılan menüyü kullanın. bir **yeniden ilke**. Aşağıdaki seçeneklerden birini seçin: 
   * **Her zaman** -herhangi bir nedenle kapanması durumunda modülü her zaman yeniden başlatılır.
   * **Hiçbir zaman** -herhangi bir nedenle kapanması durumunda modülün başlatmaz.
   * **Başarısızlık** -modülü, düzgün şekilde kapanması değil, ancak bu, bir çökme gerçekleşirse yeniden başlatır. 
   * **Üzerinde-sağlıksız** -kilitlenmesine veya sistem durumunun iyi olmadığını döndürür, modül yeniden başlatır. Bu sistem durumu işlevi uygulamak için her modül aittir. 
1. Seçmek için açılan menüyü kullanın **istenen durum** modülü için. Aşağıdaki seçeneklerden birini seçin:
   * **çalışan** -çalışan, varsayılan seçenektir. Modül hemen dağıtıldıktan sonra çalışan başlar.
   * **Durduruldu** -dağıtıldıktan sonra modülü üzerinde siz veya başka bir modül tarafından başlatılmak kadar boşta kalır.
1. Seçin **istenen özellikler kümesi modül ikizi** etiket veya diğer özellikler için modül ikizi eklemek istiyorsanız.
1. Girin **ortam değişkenlerini** bu modül için. Ortam değişkenleri, bir modül için yapılandırma bilgilerini sağlayın.
1. Seçin **Kaydet** modülünüzde dağıtıma eklenecek. 

Yapılandırılmış bir dağıtım için tüm modüllerin oluşturduktan sonra seçin **sonraki** üç adım taşımak için.

### <a name="step-3-specify-routes-optional"></a>3\. adım: Rota (isteğe bağlı) belirtme

Modüller birbirleri ile dağıtımında iletişim kurma biçimini yolları tanımlayın. Varsayılan olarak sihirbaz size bir yol olarak adlandırılan **rota** ve tanımlanmış olarak **FROM /\* Yukarı Akış $**, modüllerin tarafından çıkış iletileri IOT hub'ına gönderilen anlamına gelir.  

Ekleme veya yolları alınan bilgilerle güncelleştirme [bildirmek yollar](module-composition.md#declare-routes), ardından **sonraki** gözden geçirme bölüme geçmek için.

### <a name="step-4-specify-metrics-optional"></a>4\. Adım: Ölçümler (isteğe bağlı) belirtin

Bir cihaz, uygulama yapılandırma içeriği sonucunda geri bildirebilir çeşitli durumları özeti sayıları ölçümleri sağlar.

1. İçin bir ad girin **ölçüm adı**.

1. İçin bir sorgu girin **ölçüm ölçütleri**. Sorguyu IOT Edge hub'ı modül ikizi üzerinde alan [bildirilen özellikler](module-edgeagent-edgehub.md#edgehub-reported-properties). Ölçüm, sorgu tarafından döndürülen satır sayısını temsil eder.

   Örneğin:

   ```sql
   SELECT deviceId FROM devices
     WHERE properties.reported.lastDesiredStatus.code = 200
   ```

### <a name="step-5-target-devices"></a>5\. Adım: Hedef cihazlar

Bu dağıtım alması gereken belirli cihazları hedeflemek için etiketler özelliği cihazlarınızdan kullanın. 

Birden çok dağıtım aynı cihazı hedefleyebilir olduğundan, her dağıtım öncelik numarası vermesi gerekir. Şimdiye kadar bir çakışma varsa (daha büyük değerler daha yüksek bir önceliği gösterir) en yüksek öncelikli dağıtım kazanır. İki dağıtım aynı öncelik numarasına sahip olmak, çoğu oluşturulmuş bir son kazanır. 

1. Dağıtım için pozitif bir tamsayı girin **öncelik**.
1. Girin bir **hedef koşulu** hangi cihazların bu dağıtım ile hedeflenecek belirlemek için. Koşul, cihaz ikizi etiketlere göre veya cihaz çiftinin bildirilen özellikler ve ifade biçim ile eşleşmesi. Örneğin, `tags.environment='test'` veya `properties.reported.devicemodel='4000x'`. 
1. Seçin **sonraki** son adıma geçmek için.

### <a name="step-6-review-deployment"></a>6\. Adım: Dağıtım gözden geçirin

Dağıtım bilgilerinizi gözden geçirin ve ardından **Gönder**.

## <a name="deploy-modules-from-azure-marketplace"></a>Azure Market'ten modülleri dağıtma

Azure Market, burada, göz çok çeşitli Kurumsal uygulamaları ve onaylanmış ve Azure üzerinde çalıştırmak için en iyi duruma getirilmiş çözümleri aracılığıyla bir çevrimiçi uygulama ve hizmet Marketi olan dahil olmak üzere [IOT Edge modülleri](https://azuremarketplace.microsoft.com/marketplace/apps/category/internet-of-things?page=1&subcategories=iot-edge-modules). Azure Market de erişilebilir altında Azure portalından **kaynak Oluştur**.

Azure Market ya da Azure portalında bir IOT Edge modülü dağıtabilirsiniz:

1. Modülü bulun ve dağıtım işlemine başlar.

   * Azure portalı: Modülü bulun ve seçin **Oluştur**.

   * Azure Market:

     1. Modülü bulun ve seçin **şimdi edinin**.
     1. İşaretleyerek sağlayıcının koşulları kullanımı ve gizlilik ilkesini kabul **devam**.

1. Aboneliğiniz ve hedef cihaza bağlı olduğu IOT Hub'ı seçin.

1. Seçin **uygun ölçekte dağıtın**.

1. Yeni bir dağıtım veya varolan bir dağıtımına bir kopyasını modül eklemek isteyip istemediğinizi seçin; kopyalama, mevcut dağıtım listeden seçin.

1. Seçin **Oluştur** ölçekli olarak bir dağıtım oluşturma işlemi devam etmek için. Bir dağıtımda olduğu gibi aynı ayrıntılarını belirtin mümkün olacaktır.

## <a name="monitor-a-deployment"></a>Bir dağıtımını izleme

Bir dağıtımın ayrıntılarını görüntülemek ve onu çalıştıran cihazları izlemek için aşağıdaki adımları kullanın:

1. Oturum [Azure portalında](https://portal.azure.com) ve IOT hub'ınıza gidin. 
1. Seçin **IOT Edge**.
1. Seçin **IOT Edge dağıtımları**. 

   ![IOT Edge dağıtımları görüntüle](./media/how-to-deploy-monitor/iot-edge-deployments.png)

1. Dağıtım listesini inceleyin. Her bir dağıtım için aşağıdaki ayrıntıları görüntüleyebilirsiniz:
   * **Kimliği** -dağıtım adı.
   * **Hedef koşul** -hedeflenen cihazları tanımlamak için kullanılan etiketi.
   * **Öncelik** -bir dağıtıma atanan öncelik numarası.
   * **Sistem ölçümlerini** - **hedeflenen** hedefleme koşula uyan IOT hub'da cihaz ikizlerini belirtir ve **uygulanan** sahip cihazların sayısını belirtir Dağıtım içeriğini kendi modül ikizlerini IOT hub'ında uygulanan. 
   * **Cihaz ölçümleri** -IOT Edge cihazları başarı veya IOT Edge istemci çalışma zamanı hata raporlama dağıtım sayısı.
   * **Özel ölçümler** -IOT Edge cihazları raporlama dağıtım için tanımlanan tüm ölçümleri verilerini dağıtım sayısı.
   * **Oluşturma zamanı** -dağıtım oluşturulduğu zaman damgası. Bu zaman damgasından iki dağıtım aynı önceliğe sahip olduğunuzda TIES ayırmak için kullanılır. 
1. İzlemek istediğiniz dağıtımı seçin.  
1. Dağıtım ayrıntılarını inceleyin. Dağıtım ayrıntılarını gözden geçirmek için sekmeleri kullanabilirsiniz.

## <a name="modify-a-deployment"></a>Bir dağıtım değiştirme

Bir dağıtım değiştirdiğinizde değişiklikler hemen hedeflenen tüm cihazlara çoğaltın. 

Hedef koşul güncelleştirme aşağıdaki güncelleştirmeleri oluşur:

* Ardından, bir cihaz, eski hedef koşul karşılanmadıysa, ancak yeni hedef koşulunu ve bu dağıtım bu cihaz için en yüksek öncelikli ise, bu dağıtım cihaza uygulanır. 
* Şu anda bu dağıtım artık çalıştıran bir cihaza hedef koşulu karşılıyorsa, bu dağıtım kaldırır ve sonraki en yüksek öncelikli dağıtımı alır. 
* Şu anda bu dağıtım artık çalıştıran bir cihaza hedef koşulu karşılayan ve diğer tüm dağıtımları, hedef koşulu yerine getirmeyen, hiçbir değişiklik cihazda gerçekleşir. Cihaz, geçerli alt modüller kendi geçerli durumunda çalışmaya devam eder ancak artık bu dağıtımın bir parçası olarak yönetilmez. Başka bir dağıtım hedef koşulu karşılayan sonra bu dağıtım kaldırır ve yeni alır. 

Bir dağıtım değiştirmek için aşağıdaki adımları kullanın: 

1. Oturum [Azure portalında](https://portal.azure.com) ve IOT hub'ınıza gidin. 
1. Seçin **IOT Edge**.
1. Seçin **IOT Edge dağıtımları**. 

   ![IOT Edge dağıtımları görüntüle](./media/how-to-deploy-monitor/iot-edge-deployments.png)

1. Değiştirmek istediğiniz dağıtımı seçin. 
1. Aşağıdaki alanları güncelleştirmeleri yapın: 
   * Hedef koşul
   * Ölçümleri - değiştirebilir veya tanımlamış olduğunuz ya da yenilerini eklemek ölçümlerini Sil.
   * Etiketler
   * Öncelik
1. **Kaydet**’i seçin.
1. Bağlantısındaki [bir dağıtımını izleme](#monitor-a-deployment) dağıtmadan değişiklikleri izlemek için. 

## <a name="delete-a-deployment"></a>Dağıtımı Sil

Bir dağıtım sildiğinizde, sonraki en yüksek öncelikli dağıtım üzerinde herhangi bir cihaza yararlanın. Daha sonra başka bir dağıtım hedef koşulu cihazlarınızı karşılamıyorsa, dağıtım silindiğinde modülleri kaldırılmaz. 

1. Oturum [Azure portalında](https://portal.azure.com) ve IOT hub'ınıza gidin. 
1. Seçin **IOT Edge**.
1. Seçin **IOT Edge dağıtımları**. 

   ![IOT Edge dağıtımları görüntüle](./media/how-to-deploy-monitor/iot-edge-deployments.png)

1. Silmek istediğiniz dağıtım seçmek için onay kutusunu kullanın. 
1. **Sil**’i seçin.
1. Bir komut istemi Bu eylem bu dağıtımı silin ve tüm cihazlar için önceki duruma geri bildirir.  Bu, daha düşük öncelikli bir dağıtım uygulayacağı anlamına gelir.  Başka bir dağıtımda henüz hedeflenmişse, hiçbir modülün kaldırılacak. Cihazınızın tüm modülleri kaldırmak istiyorsanız, sıfır modülleriyle bir dağıtımını oluşturun ve aynı cihazlara dağıtın. Seçin **Evet** devam etmek için. 

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin [modülleri IOT Edge cihazlarına dağıtma](module-deployment-monitoring.md).
