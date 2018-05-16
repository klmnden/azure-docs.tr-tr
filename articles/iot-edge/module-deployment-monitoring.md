---
title: Azure IOT köşesi modülleri dağıtma | Microsoft Docs
description: Nasıl modülleri kenar cihazlara dağıttığınız hakkında bilgi edinin
services: iot-edge
keywords: ''
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 10/05/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: ffd3a8e6bde7310f6bdbed0e0f87419c73fcd6fc
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="understand-iot-edge-deployments-for-single-devices-or-at-scale---preview"></a>IOT kenar dağıtımları tek cihazlar için veya ölçekte anlamak - Önizleme

Azure IOT sınır cihazları izleyin bir [cihaz yaşam döngüsü] [ lnk-lifecycle] , IOT cihazları diğer türleri için benzer:

1. IOT sınır cihazları sağlanan, bir işletim sistemine sahip bir aygıt Imaging ve yükleme içerir [IOT kenar çalışma zamanı][lnk-runtime].
1. Aygıtları çalışmak üzere yapılandırılan [IOT kenar modülleri][lnk-modules]ve sistem durumu için izlenen. 
1. Son olarak, değiştirilen veya geçersiz duruma gelir aygıtları kullanımdan.  

Azure IOT kenar IOT kenar cihazlarda çalıştırmak için modülleri yapılandırmanın iki yolu sağlar: biri geliştirme ve (kullandığınız Azure IOT kenar eğitimlerine) tek bir cihaz üzerinde hızlı yineleme için ve biri büyük fleets IOT kenar aygıtları yönetmek için. Bu yaklaşım her ikisi de Azure Portal ve program aracılığıyla kullanılabilir.

Bu makalede yapılandırmasına odaklanır ve aygıtların fleets aşamaları izleme için IOT kenar dağıtımları anılan. Genel dağıtım adımları aşağıdaki gibidir:   

1. Bir işleç modülleri, hem de hedef cihazlar kümesini bir dağıtım tanımlar. Her dağıtım bu bilgileri yansıtan bir dağıtım bildirimi sahiptir. 
1. IOT Hub hizmeti istenen modüllerle yapılandırmak için tüm hedeflenen cihazların iletişim kurar. 
1. IOT Hub hizmeti IOT kenar cihazlardan gelen durum alır ve izlemek işleci için ortaya çıkarır.  Örneğin, bir işleç ne zaman bir uç cihazın başarılı bir şekilde yapılandırılmamış veya bir modülü çalışma zamanı sırasında başarısız olursa görebilirsiniz. 
1. Herhangi bir zamanda hedefleme koşullara uyan yeni IOT sınır cihazları dağıtım için yapılandırılır. Örneğin, sağlanan ve Washington State cihaz grubuna eklenen sonra yeni bir IOT sınır cihazı Washington durumdaki tüm IOT sınır cihazları otomatik olarak hedefler bir dağıtım yapılandırır. 
 
Bu makalede, yapılandırma ve dağıtım izleme dahil edilen her bir bileşen size yol göstermektedir. Oluşturma ve güncelleştirme dağıtımı için bkz [dağıtma ve izleme IOT kenar modülleri ölçekte][lnk-howto].

## <a name="deployment"></a>Dağıtım

Bir dağıtım IOT kenar IOT sınır cihazları hedeflenen kümesine göre örnekleri çalıştırmak için modülü görüntüleri atar. Karşılık gelen başlatma parametrelerle modüllerin listesini dahil etmek için bir IOT kenar dağıtım bildirimi yapılandırarak çalışır. Bir dağıtımı (genellikle cihaz kimliği temel alarak) tek bir cihazı veya bir gruba (etiketlere göre) aygıtların atanabilir. Bir IOT sınır cihazı bir dağıtım bildirimi aldıktan sonra indirir ve ilgili kapsayıcı havuzların modülü kapsayıcı görüntüleri yükler ve bunları uygun şekilde yapılandırır. Bir dağıtımı oluşturulduktan sonra bir işleç hedeflenen cihazların doğru yapılandırıldığından olup olmadığını görmek için dağıtım durumunu izleyebilirsiniz.   

Cihazların dağıtımla yapılandırılacak IOT sınır cihazları olarak sağlanması gerekir. Aşağıdaki önkoşullar bulunmaktadır ve dağıtımda dahil edilmez:
* Temel işletim sistemi
* Docker 
* IOT kenar çalışma zamanı sağlama 

### <a name="deployment-manifest"></a>Dağıtım bildirimi

Hedeflenen IOT kenar cihazlarda yapılandırılması için modülleri açıklayan bir JSON belgesi dağıtım bildirimidir. Gerekli sistem modülleri (özellikle IOT kenar aracısı ve IOT kenar hub) dahil olmak üzere tüm modülleri için yapılandırma meta verilerini içerir.  

Her modül için yapılandırma meta verilerini içerir: 
* Sürüm 
* Tür 
* Durum (örneğin çalışıyor veya durduruldu) 
* İlke yeniden Başlat 
* Görüntü ve kapsayıcı deposu 
* Giriş ve çıkış veri yolları 

### <a name="target-condition"></a>Hedef durumu

Hedef durumu gereksinimlerini karşılayan yeni aygıtları dahil etmek veya artık dağıtım yaşam süresi yapmak aygıtları kaldırmak için sürekli olarak değerlendirilir. Hizmeti herhangi bir hedef koşul değişiklik algılarsa, dağıtım yeniden. Örneğin, hedef koşulu tags.environment olan bir dağıtım A sahip 'üretim' =. Dağıtımı devre dışı kazandırın, 10 üretim aygıtı yok. Modüller, bu 10 cihazların başarıyla yüklenir. IOT kenar aracı durumu 10 toplam cihaz olarak 10 başarıyla yanıtları, 0 yanıtı hatası ve 0 bekleyen yanıtları gösterilir. Tags.environment ile daha fazla 5 cihaz Ekle şimdi 'üretim' =. Hizmet değişikliği algılar ve IOT kenar aracı durumu 15 toplam aygıt, 10 başarıyla olur yanıtları, 0 hata yanıtları ve beş yeni cihazlara dağıtmak çalıştığında 5 bekleyen yanıtlar.

Herhangi bir Boolean koşul cihaz çiftlerini etiketler veya DeviceID hedef cihazlar seçmek için kullanın. Koşul etiketleriyle kullanmak istiyorsanız, "etiketler" eklemeniz gerekir:{} özellikleri ile aynı düzeyde altında cihaz çifti bölümünde. [Cihaz çifti etiketleri hakkında daha fazla bilgi edinin](../iot-hub/iot-hub-devguide-device-twins.md)

Hedef koşul örnekleri:
* DeviceID 'linuxprod1' =
* Tags.Environment 'üretim' =
* Tags.Environment 'üretim' ve tags.location = 'westus' =
* Tags.Environment 'üretim' veya tags.location = 'westus' =
* Tags.operator = 'Gamze' ve tags.environment 'üretim' değil DeviceID = 'linuxprod1' =

Hedef durumu yapısı oluştururken bazı kısıtlar şunlardır:

* Cihaz çiftine etiketler veya DeviceID kullanarak bir hedef durumu yalnızca oluşturabilirsiniz.
* Çift tırnak işareti herhangi bir kısmının hedef durumu izin verilmez. Lütfen tek tırnak işareti kullanın.
* Tek tırnak hedef durumu değerini temsil eder. Bu nedenle, aygıt adı parçası ise başka bir tek tırnaklı tek teklifle kaçış gerekir. Örneğin, için hedef durumu: operator'sDevice DeviceID yazılması gerekir =' işleci '' sDevice'.
* Sayı, harf ve şu karakterleri hedef koşulu values:-:.+%_#* izin verilir? (),=@;$

### <a name="priority"></a>Öncelik

Bir öncelik bir dağıtım diğer dağıtımlar göre hedeflenen bir cihaza uygulanmış olup olmadığını tanımlar. Bir dağıtım önceliği, daha yüksek öncelik belirten büyük sayılarla pozitif bir tamsayı ' dir. Bir IOT sınır cihazı birden fazla dağıtım tarafından hedeflenen en yüksek öncelikli dağıtım geçerlidir.  Daha düşük önceliklere sahip dağıtımlar uygulanmaz ve bunlar birleştirilir.  Bir aygıt ile iki veya daha fazla dağıtım eşit öncelikli hedeflenir (oluşturma zaman damgası tarafından belirlenir) en son oluşturulan dağıtım geçerlidir.

### <a name="labels"></a>Etiketler 

Etiketleri filtre ve dağıtım grubu için kullanabileceğiniz dize anahtar/değer çiftleridir. Bir dağıtım birden çok etiket olabilir. Etiketleri isteğe bağlıdır ve herhangi bir etkisi IOT sınır cihazları gerçek yapılandırmasını yapın. 

### <a name="deployment-status"></a>Dağıtım durumu

Bir dağıtım için hedeflenen tüm IOT sınır cihazı başarıyla uygulanmış olup olmadığını belirlemek için izlenebilir.  Hedeflenen bir sınır cihazı bir veya daha fazla aşağıdaki durum kategorileri içinde görünür: 
* **Hedef** IOT sınır koşulu hedefleme dağıtım eşleşen cihazları gösterir.
* **Gerçek** hedeflenen IOT kenar daha yüksek öncelikli başka bir dağıtım tarafından hedeflenen olmayan cihazları gösterir.
* **Sağlıklı** IOT kenar modülleri başarıyla dağıtıldıktan hizmete geri raporladı cihazları gösterir. 
* **Sağlıksız** aygıtları bildirilen geri hizmeti bir veya modülleri değil dağıtılan başarıyla IOT kenar gösterir. Daha fazla hata araştırmak için bu cihazları uzaktan bağlanmak ve günlük dosyalarını görüntüleyin.
* **Bilinmeyen** IOT kenar bu dağıtımla ilgili durum bildirmedi cihazları gösterir. Daha fazla araştırmak için hizmet bilgileri ve günlük dosyalarını görüntüleyin.

## <a name="phased-rollout"></a>Aşamalı 

Aşamalı bir işleç değişiklikleri IOT sınır cihazları insanın ufkunu genişleten bir dizi yapabildiği dağıtır genel bir işlemdir. Kademeli olarak geniş ölçek önemli değişiklikler yapma riskini azaltmak için değişiklik yapmak için belirtilir.  

Aşamalı aşağıdaki aşamaları ve adımları çalıştırılır: 
1. Bunları sağlama ve cihaz çifti etiketi gibi ayarlayarak IOT sınır cihazları oluşan bir sınama ortamı kurmak `tag.environment='test'`. Sınama ortamı dağıtımını sonunda hedefleyecektir üretim ortamını yansıtması. 
1. İstenen modülleri ve yapılandırmaları da dahil olmak üzere bir dağıtım oluşturun. Hedefleme koşulu test IOT kenar aygıt ortamı hedeflemelidir.   
1. Yeni modül yapılandırması test ortamında doğrulayın.
1. Yeni bir etiket hedefleme durumuna ekleyerek bir alt kümesini üretim IOT sınır cihazları dahil etmek için dağıtım güncelleştirin. Ayrıca, dağıtım önceliği şu anda bu aygıtlar için hedeflenen diğer dağıtımlar değerinden yüksek olduğundan emin olun 
1. Dağıtım durumu görüntüleyerek dağıtım hedeflenen IOT cihazlarda başarılı olduğunu doğrulayın.
1. Kalan tüm üretim IOT sınır cihazları hedeflemek için dağıtım güncelleştirin.

## <a name="rollback"></a>Geri alma

Dağıtımları hatalar veya yapılandırma hataları durumunda geri alınabilir.  Bir dağıtım mutlak modül yapılandırması için bir IOT sınır cihazı tanımladığından hedefi olan tüm modülleri kaldırmaya olsa bile ek dağıtım de aynı aygıt daha düşük bir öncelikte hedeflenecek gerekir.  

Geri alma aşağıdaki sırayla gerçekleştirin: 
1. İkinci dağıtımı sırasında aynı aygıt kümesi hedeflenir onaylayın. Geri alma amacı olan tüm modülleri kaldırmaya ise, ikinci dağıtımı modülleriniz içermemelidir. 
1. Değiştirin veya hedef koşul ifadesi, böylece geri cihazlar artık hedefleme koşula alma istediğiniz dağıtım kaldırın.
1. Dağıtım durumunu görüntüleyerek geri alma işleminin başarılı olduğunu doğrulayın.
   * Toplu geri dağıtım durumu geri alındı cihazlar için artık göstermelidir.
   * İkinci dağıtımı şimdi geri alındı cihazlar için dağıtım durumu eklemeniz gerekir.


## <a name="next-steps"></a>Sonraki adımlar

* Oluştur, Güncelleştir veya bir dağıtımda silmek için izleyeceğiniz adımlarda size yol [dağıtma ve izleme IOT kenar modülleri ölçekte][lnk-howto].
* Gibi diğer IOT kenar kavramları hakkında daha fazla bilgi [IOT kenar çalışma zamanı] [ lnk-runtime] ve [IOT kenar modülleri][lnk-modules].

<!-- Links -->
[lnk-lifecycle]: ../iot-hub/iot-hub-device-management-overview.md
[lnk-runtime]: iot-edge-runtime.md
[lnk-modules]: iot-edge-modules.md
[lnk-howto]: how-to-deploy-monitor.md