---
title: Cihaz grupları - Azure IOT Edge için otomatik dağıtım | Microsoft Docs
description: Otomatik dağıtımlar, paylaşılan etiketlere göre cihaz grupları yönetmek için Azure IOT Edge'de kullanın.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 09/27/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 376ee74732daf526b31129fa8c93cbaa32350eae
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60318215"
---
# <a name="understand-iot-edge-automatic-deployments-for-single-devices-or-at-scale"></a>IOT Edge otomatik dağıtımlar tek tek cihazlarda veya uygun ölçekte anlama

Azure IOT Edge cihazları izleyin bir [cihaz yaşam döngüsü](../iot-hub/iot-hub-device-management-overview.md) olan diğer IOT cihazları türlerine benzer:

1. Bir işletim sistemi ile bir cihaz görüntüleme ve yükleme yeni IOT Edge cihaz sağlama [IOT Edge çalışma zamanı](iot-edge-runtime.md).
2. Cihazlarını çalışacak şekilde yapılandırabilirsiniz [IOT Edge modülleri](iot-edge-modules.md)ve sonra kendi durumunu izleyebilir. 
3. Son olarak, değiştirilen veya kalkacak cihazları devre dışı bırakın.  

Azure IOT Edge modüllerinin IOT Edge cihazlarında çalışmasını yapılandırmak için iki yol sağlar: biri geliştirme ve tek bir cihaz üzerinde hızlı yinelemeler için (Bu yöntem Azure IOT Edge'de kullanılan [öğreticiler](tutorial-deploy-function.md)), ve büyük bir yönetmek için fleets IOT Edge cihazları. Bu yaklaşımların her ikisi de Azure portalında hem de programsal olarak kullanılabilir. Gruplar veya çok sayıda cihazları hedeflemek için hangi cihazların kullanarak modüllerinizi dağıtmak istediğinizi belirtebilirsiniz [etiketleri](../iot-edge/how-to-deploy-monitor.md#identify-devices-using-tags) cihaz ikizinde. Aşağıdaki adımlarda tanımlanan etiketler özelliği aracılığıyla bir Washington eyaletinde cihaz grubuna bir dağıtım bahsedeceğiz. 

Bu makalede yapılandırmasına odaklanılmıştır ve cihazların filolarına aşamaları izleme için IOT Edge otomatik dağıtımlar anılan. Genel dağıtım adımlar aşağıdaki gibidir: 

1. Bir işleç modülleri, hem de hedef cihazlar kümesini tanımlayan bir dağıtım tanımlar. Her dağıtım, bu bilgileri yansıtan bir dağıtım bildirimi sahiptir. 
2. IOT Hub hizmeti, tüm hedeflenen cihazların istenen modülleriyle yapılandırılacakları ile iletişim kurar. 
3. IOT Hub hizmeti, IOT Edge cihazları alır ve işleci için kullanılabilir hale getirir.  Örneğin, bir işleç bir uç cihazın ne zaman başarılı bir şekilde yapılandırılmamış veya bir modülü çalışma zamanı sırasında başarısız olursa görebilirsiniz. 
4. Herhangi bir zamanda hedefleme koşullara uyan IOT Edge cihazları yeni dağıtım için yapılandırılmış. Örneğin, sağlanan ve Washington eyaletinde cihaz grubuna eklenen yeni bir IOT Edge cihazı otomatik olarak, Washington eyaletinde tüm IOT Edge cihazları hedefleyen bir dağıtım yapılandırır. 
 
Bu makalede, yapılandırma ve dağıtım izleme alan her bir bileşeni açıklar. Oluşturma ve dağıtımı güncelleştirme yönergeleri için bkz [dağıtma ve izleme uygun ölçekte IOT Edge modülleri](how-to-deploy-monitor.md).

## <a name="deployment"></a>Dağıtım

IOT Edge otomatik dağıtım, IOT Edge modül görüntüleri hedeflenen bir IOT Edge cihazlarının örnekler olarak çalıştırmak için atar. Karşılık gelen başlatma parametreleri ile modüllerin listesini dahil etmek için bir IOT Edge dağıtımı bildirimi yapılandırarak çalışır. (Cihaz Kimliğine göre) tek bir cihaz veya cihazları (etiketlere göre) bir grup için bir dağıtım atanabilir. IOT Edge cihazı bir dağıtım bildirimi aldıktan sonra indirir ve karşılık gelen kapsayıcı depolarından kapsayıcı görüntüleri yükler ve buna göre yapılandırır. Bir dağıtımı oluşturulduktan sonra operatörün hedeflenen cihazlar düzgün yapılandırılmış olup olmadığını görmek için dağıtım durumunu izleyebilirsiniz.

IOT Edge cihazları yalnızca bir dağıtım ile yapılandırılabilir. Dağıtımı almadan önce aşağıdaki önkoşulların cihazda olmalıdır:

* Temel işletim sistemi
* Moby ya da Docker gibi bir kapsayıcı yönetim sistemi
* IOT Edge çalışma zamanı sağlama 

### <a name="deployment-manifest"></a>Dağıtım bildirimi

Bir dağıtım bildirimi hedeflenen IOT Edge cihazlarında yapılandırılması için modülleri açıklayan bir JSON belgesidir. Gerekli sistem modüllerine (özellikle IOT Edge aracısı ve IOT Edge hub'ı) dahil olmak üzere tüm modüller için yapılandırma meta verilerini içeriyor.  

Her bir modül için yapılandırma meta verilerini içerir: 

* Sürüm 
* Tür 
* Durum (örneğin, çalışıyor veya durduruldu) 
* Yeniden başlatma ilkesi 
* Görüntü ve kapsayıcı kayıt defteri
* Giriş ve çıkış veri yolları 

Bir özel kapsayıcı kayıt defteri modülü görüntüsü depolanırsa, IOT Edge aracısı kayıt defteri kimlik bilgilerini tutar. 

### <a name="target-condition"></a>Hedef koşul

Hedef koşulu, dağıtımın kullanım ömrü boyunca sürekli olarak değerlendirilir. Gereksinimleri karşılayan yeni cihazları dahil edin ve artık yapan herhangi bir mevcut cihaza kaldırılır. Dağıtım Hizmeti herhangi bir hedef koşulu değişiklik algılarsa yeniden başlatılır. 

Örneğin, bir A hedef koşulu tags.environment dağıtımınız = 'prod'. Dağıtımı devre dışı yaslanıp, 10 üretim cihaz bulunur. Modüller, bu 10 cihazları başarıyla yüklenir. IOT Edge aracı durumu, toplam cihaz sayısı 10, 10 başarılı yanıtlar, 0 hata yanıtları ve 0 bekleyen yanıtlar gösterilir. Şimdi tags.environment ile beş daha fazla cihaz Ekle 'prod' =. Hizmet değişikliği algılar ve beş yeni cihazlara dağıtmak çalıştığında, IOT Edge aracı durumu 15 toplam cihaz sayısı, 10 başarılı yanıtlar, 0 hata yanıtları ve 5 bekleyen yanıtlar olur.

Herhangi bir Boolean koşul, hedef cihazları seçmek için cihaz ikizlerini etiketleri veya DeviceID kullanın. Etiketleri koşul kullanmak istiyorsanız, "tags" eklemeniz gerekir:{} cihaz ikizi özelliklerini aynı düzeyde altında bölümünde. [Cihaz ikizindeki etiket hakkında daha fazla bilgi edinin](../iot-hub/iot-hub-devguide-device-twins.md)

Hedef koşul örnekleri:

* DeviceID = 'linuxprod1'
* Tags.Environment = 'prod'
* Tags.Environment = 'prod' AND tags.location = 'westus'
* Tags.Environment = 'prod' veya tags.location = 'westus'
* Tags.operator 'John' = ve tags.environment = 'prod' değil DeviceID = 'linuxprod1'

Hedef koşul oluştururken bazı kısıtlar şunlardır:

* Cihaz ikizinde etiketleri veya DeviceID kullanarak bir hedef koşulu yalnızca oluşturabilirsiniz.
* Çift tırnak işareti, herhangi bir bölümünü hedef koşulu izin verilmez. Tek tırnak işareti kullanın.
* Tek tırnak, hedef koşulu değerlerini temsil eder. Bu nedenle, cihaz adı bir parçası ise tek tırnak işareti ile başka bir tek tırnak işareti kaçış gerekir. Örneğin, bir cihazı hedeflemeniz adlı `operator'sDevice`, yazma `deviceId='operator''sDevice'`.
* Sayı, harf ve şu karakterleri hedef koşulu değerlerde izin verilir: `-:.+%_#*?!(),=@;$`.

### <a name="priority"></a>Öncelik

Öncelikli bir dağıtım hedeflenen cihaza göre diğer dağıtımlar uygulanması gerektiğini tanımlar. Dağıtım ile daha büyük sayılar daha yüksek öncelikli belirten pozitif bir tamsayı önceliktir. IOT Edge cihazı birden fazla dağıtım tarafından hedeflendiğinde, en yüksek önceliğe sahip dağıtım uygulanır.  Daha düşük önceliklere sahip dağıtımlar uygulanmaz veya bunlar birleştirilir.  Bir cihaz ile eşit önceliğe iki veya daha fazla dağıtım henüz hedeflenmişse, (oluşturma zaman damgası tarafından belirlenir) en son oluşturulan dağıtım uygulanır.

### <a name="labels"></a>Etiketler 

Etiketleri filtresi ve dağıtım grubu için kullanabileceğiniz dize anahtar/değer çiftleridir. Bir dağıtım, birden çok etiket olabilir. Etiket isteğe bağlıdır ve herhangi bir etkisi IOT Edge cihazları gerçek yapılandırmasını yapın. 

### <a name="deployment-status"></a>Dağıtım durumu

Bir dağıtım için hedeflenen tüm IOT Edge cihazı başarıyla uygulanmış olup olmadığını belirlemek için de izlenebilir.  Hedeflenen bir sınır cihazı, bir veya daha fazla aşağıdaki durum kategorileri içinde görünür: 

* **Hedef** dağıtım koşulu hedefleyen eşleşen cihazları IOT Edge gösterir.
* **Gerçek** hedeflenen IOT Edge daha yüksek öncelikli başka bir dağıtım tarafından hedeflenmeyen cihazlar gösterilir.
* **Sağlıklı** IOT Edge modülleri başarıyla dağıtıldığını hizmete geri bildirdiniz cihazları gösterir. 
* **Sağlıksız** IOT Edge cihazları bildirilen geri hizmet bir veya modülleri değil dağıtılan başarıyla gösterir. Daha fazla hata araştırmak, bu cihazlar için uzaktan bağlanma ve günlük dosyalarını görüntülemek için.
* **Bilinmeyen** IOT Edge, bu dağıtımla ilgili herhangi bir durum bildirmedi cihazları gösterir. Daha fazla araştırmak için hizmet bilgileri ve günlük dosyalarını görüntüleyin.

## <a name="phased-rollout"></a>Aşamalı dağıtımı 

Aşamalı yapabildiği değişiklikleri operatörün insanın ufkunu genişleten bir IOT Edge cihazları kümesine dağıtır, genel bir işlemdir. Aşamalı olarak geniş ölçek bozucu değişiklikler yapma riskini azaltmak için değişiklik olmaktır.  

Aşamalı aşağıdaki aşamaları ve adımları çalıştırılır: 

1. Bunları sağlama ve cihaz ikizi etiketi gibi ayarlayarak IOT Edge cihazları, bir test ortamı kurmak `tag.environment='test'`. Test ortamına dağıtım sonunda hedefleyen üretim ortamına yansıması olmalıdır. 
2. İstenen modülleri ve yapılandırmaları da dahil olmak üzere bir dağıtım oluşturun. IOT Edge cihazı ortam test hedefleme koşul hedeflemelidir.   
3. Yeni modül yapılandırması test ortamında doğrulayın.
4. Dağıtım hedefleme koşula yeni bir etiket ekleyerek bir alt kümesini üretim IOT Edge cihazları içerecek şekilde güncelleştirin. Ayrıca, dağıtım önceliğini şu anda bu cihazları hedefleyen diğer dağıtımlar daha yüksek olduğundan emin olun 
5. Dağıtım durumu görüntüleyerek dağıtım hedeflenen IOT Cihazlarında başarılı olduğunu doğrulayın.
6. Kalan tüm üretim IOT Edge cihazları hedeflemek için dağıtım güncelleştirin.

## <a name="rollback"></a>Geri alma

Bir hata veya yanlış yapılandırmalarını geri alırsanız, dağıtımları alınabilir.  Bir dağıtım mutlak modül yapılandırması için bir IOT Edge cihazı tanımladığından, tüm modülleri kaldırmak için hedef olsa bile, ek bir dağıtım de aynı cihaza daha düşük bir öncelikte hedeflenmesi gerekir.  

Geri alma işlemleri, aşağıdaki sırayla gerçekleştirin: 

1. İkinci bir dağıtım aynı cihazı kümesinin hedeflenir onaylayın. Geri alma amacı, tüm modülleri kaldırmak için ise, ikinci dağıtımı modüllerin içermemelidir. 
2. Değiştirin veya hedef koşul ifadesi, böylece cihazlar artık hedefleme koşula uyan geri almak istediğiniz dağıtımın kaldırın.
3. Dağıtım durumunu görüntüleyerek geri alma başarılı olduğunu doğrulayın.
   * Toplu geri dağıtım durumu geri alındı cihazlar için artık göstermelidir.
   * İkinci dağıtımı artık geri alındı cihazlar için dağıtım durumunu içermelidir.


## <a name="next-steps"></a>Sonraki adımlar

* Oluşturmak, güncelleştirmek veya bir dağıtımda silmek için adımlarında yol [dağıtma ve izleme uygun ölçekte IOT Edge modülleri](how-to-deploy-monitor.md).
* Gibi diğer IOT Edge kavramları hakkında daha fazla bilgi [IOT Edge çalışma zamanı](iot-edge-runtime.md) ve [IOT Edge modülleri](iot-edge-modules.md).

