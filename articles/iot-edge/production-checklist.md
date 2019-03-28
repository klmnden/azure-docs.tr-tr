---
title: Cihazlar ve dağıtımları üretim - Azure IOT Edge için hazırlama | Microsoft Docs
description: Azure IOT Edge çözümünüzü geliştirmeden üretime, uygun sertifikaları ile cihazlarınızı ayarlama ve sonraki kodu güncelleştirmeleri için bir dağıtım planı yapma gibi ele öğrenin.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 11/28/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: c64db6b35aa2f1daa4484f137c8505b1415c5a0b
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58521763"
---
# <a name="prepare-to-deploy-your-iot-edge-solution-in-production"></a>IOT Edge çözümünüzü üretim ortamında dağıtmaya hazırlanma

IOT Edge çözümünüzü geliştirmeden üretime almaya hazır olduğunuzda, devam eden performansı için yapılandırıldığından emin olun.

Bu makalede sağlanan bilgiler, tüm eşit değil. Önceliğini belirlemeye yardımcı olmak için işi iki bölüme ayırın listeleriyle her bölümü başlatır: **önemli** üretime geçmeden önce tamamlanması veya **yararlı** bilmenizi için.

## <a name="device-configuration"></a>Cihaz yapılandırması

IOT Edge cihazları, bir dizüstü bilgisayar bir sunucu üzerinde çalışan bir sanal makineye Raspberry Pi'yi her şey olabilir. Fiziksel veya sanal bir bağlantı üzerinden erişim cihaza sahip olabilir veya uzun süre için yalıtılmış olabilir. Her iki durumda da, uygun bir şekilde gerçekleştirmek için yapılandırılmış olduğundan emin olmanız gerekir. 

* **Önemli**
    * Üretim sertifika yükleme
    * Bir cihaz yönetimi planı
    * Moby kapsayıcı altyapısı olarak kullanma

* **Yararlı**
    * Yukarı Akış Protokolü seçin

### <a name="install-production-certificates"></a>Üretim sertifika yükleme

Her IOT Edge Cihazınızı üretim ortamına yüklü bir cihaz sertifika yetkilisi (CA) sertifikası gerekir. Bu CA sertifikası, sonra IOT Edge çalışma zamanına config.yaml dosyasında bildirilir. Geliştirme ve IOT Edge daha kolay test yapmak için sertifika config.yaml dosyasında bildirilmiş olan çalışma zamanı geçici sertifikalar oluşturur. Ancak, bu geçici sertifikalar üç ay sonra sona erer ve üretim senaryoları için güvenli değildir. 

Cihaz CA sertifikası rolünü anlamak için bkz: [Azure IOT Edge nasıl kullandığı sertifikalar](iot-edge-certs.md).

Bir IOT Edge cihazında sertifikaları yüklemek ve bunlara config.yaml dosyasından başvurmak hakkında daha fazla bilgi için bkz. [saydam bir ağ geçidi olarak görev yapacak bir IOT Edge cihazı yapılandırma](how-to-create-transparent-gateway.md). Cihaz veya bir ağ geçidi olarak kullanılacak geçiyor olmadığını sertifikaları yapılandırma adımları aynıdır. Bu makalede, yalnızca test için örnek sertifikalarını oluşturmak için betikler sağlar. Bu örnek sertifikaları üretim ortamında kullanmayın. 

### <a name="have-a-device-management-plan"></a>Bir cihaz yönetimi planı

Herhangi bir CİHAZDAN üretime yerleştirmeden önce gelecekteki güncelleştirmeleri yönetmek için nasıl gideceğinizi bilmeniz gerekir. Bir IOT Edge cihazı için güncelleştirilecek bileşenlerin listesini içerebilir:

* Cihaz üretici yazılımını
* İşletim sistemi kitaplığı
* Moby gibi kapsayıcı altyapısı
* IOT Edge arka plan programı
* CA sertifikaları

IOT Edge arka plan programı güncelleştirmek adımlar için bkz: [IOT Edge çalışma zamanı güncelleştirmesi](how-to-update-iot-edge.md). IOT Edge arka plan programı güncelleştirmek için geçerli olan yöntemler, fiziksel ya da SSH erişimini IOT Edge cihazı gerektirir. Güncelleştirmek için birçok cihazınız varsa güncelleştirme adımlar için bir komut dosyası eklemeyi düşünün veya Ansible gibi bir Otomasyon aracı güncelleştirmeleri uygun ölçekte gerçekleştirmek için kullanın.

### <a name="use-moby-as-the-container-engine"></a>Moby kapsayıcı altyapısı olarak kullanma

Bir cihazda bir kapsayıcı altyapısı olan herhangi bir IOT Edge cihaz için bir önkoşuldur. Yalnızca moby altyapısı, üretim ortamında desteklenir. Docker gibi diğer kapsayıcı altyapıları ile IOT Edge çalışma ve bu altyapılar geliştirme için kullanılacak normaldir. Azure IOT Edge ile kullanıldığında moby altyapısı dağıtılabilir ve Microsoft bu altyapısı için bakım sağlar. Diğer kapsayıcı altyapıları kullanarak bir IOT Edge cihazında desteklenmiyor.

### <a name="choose-upstream-protocol"></a>Yukarı Akış Protokolü seçin

Protokol (ve bu nedenle kullanılan bağlantı noktası) için Edge aracısı ve Edge hub'ı için IOT hub'ına Yukarı Akış iletişimi yapılandırılabilir. Varsayılan protokol AMQP olmakla birlikte, ağ kurulumunuza bağlı olarak değiştirmek isteyebilirsiniz. 

İki çalışma zamanı modülü hem de sahip bir **UpstreamProtocol** ortam değişkeni. Değişken için geçerli değerler şunlardır: 

* MQTT
* AMQP
* MQTTWS
* AMQPWS

Cihaz üzerinde config.yaml dosya Edge Aracısı UpstreamProtocol değişkenini yapılandırın. Örneğin, IOT Edge Cihazınızı bir proxy sunucunun arkasındaki engelleyen AMQP bağlantı noktaları, IOT hub'ı ilk bağlantıyı oluşturmak için (AMQPWS) WebSocket üzerinden AMQP kullanmayı Edge Aracısı'nı yapılandırmanız gerekebilir. 

IOT Edge Cihazınızı bağlayan sonra UpstreamProtocol değişken her iki çalışma zamanı modülü için gelecek dağıtımlarda yapılandırmasına devam etmek emin olun. Bu işlem örneği sağlanan [bir proxy sunucu üzerinden iletişim kurmak için IOT Edge cihazı yapılandırma](how-to-configure-proxy-support.md).

## <a name="deployment"></a>Dağıtım

* **Yararlı**
    * Yukarı Akış Protokolü ile tutarlı olması
    * Edge hub'ı tarafından kullanılan bellek alanını azaltın
    * Hata ayıklama sürümleri modül görüntüleri kullanmayın

### <a name="be-consistent-with-upstream-protocol"></a>Yukarı Akış Protokolü ile tutarlı olması

IOT Edge Cihazınızda AMQP varsayılandan farklı bir protokol kullanmanız için Edge Aracısı'nı yapılandırdıysanız, aynı protokolü tüm sonraki dağıtımlarda bildirmeniz gerekir. Örneğin, IOT Edge Cihazınızı bir proxy sunucunun arkasındaki engelleyen AMQP bağlantı noktaları, büyük olasılıkla cihazı (AMQPWS) WebSocket üzerinden AMQP üzerinden bağlanmak için yapılandırılmış. Edge aracısı ve Edge hub'ı için aynı APQPWS Protokolü yapılandırmazsanız, cihaza modülleri dağıttığınızda, AMQP varsayılan ayarları geçersiz kılar ve yeniden bağlanmayı engelliyor. 

Edge aracısı ve Edge hub'ı modülleri UpstreamProtocol ortamı değişkeni yapılandırmanız yeterlidir. Ek modüllerin her protokolü, çalışma zamanı modülleri ayarlanır benimseyin. 

Bu işlem örneği sağlanan [bir proxy sunucu üzerinden iletişim kurmak için IOT Edge cihazı yapılandırma](how-to-configure-proxy-support.md).

### <a name="reduce-memory-space-used-by-edge-hub"></a>Edge hub'ı tarafından kullanılan bellek alanını azaltın

Kısıtlanmış cihazlarla sınırlı bellek dağıtıyorsanız, Edge hub'ı daha kolaylaştırılmış bir kapasitede çalıştırmak ve daha az disk alanı kullanmak için yapılandırabilirsiniz. Bu yapılandırmalar Edge hub'ı performansını ancak limit, bu nedenle çalışır, çözümünüz için doğru dengeyi bulmak. 

#### <a name="dont-optimize-for-performance-on-constrained-devices"></a>Kısıtlanmış cihazlarda performansı iyileştirme

Bu büyük boyutta bellek ayırmaya çalışır böylece Edge hub'ı varsayılan olarak, performans için optimize edilmiştir. Bu yapılandırma, Raspberry Pi gibi küçük cihazlarda kararlılık sorunlara neden olabilir. Kısıtlanmış kaynaklara sahip cihazları dağıtıyorsanız ayarlamak isteyebilirsiniz **OptimizeForPerformance** ortam değişkenine **false** Edge hub'ı üzerinde. 

Daha fazla bilgi için [kaynaktaki kararlılık sorunlarını cihazları kısıtlı](troubleshoot.md#stability-issues-on-resource-constrained-devices).

#### <a name="disable-unused-protocols"></a>Kullanılmayan protokollerini devre dışı bırakın

Edge hub'ı performansını iyileştirmek ve bellek kullanımını azaltmak için başka bir protokol heads çözümünüzde kullanmadığınız herhangi bir protokol için devre dışı bırakmak için yoludur. 

Protokol heads dağıtım bildirimlerinizi Edge hub'ı modülü için boolean ortam değişkenlerini ayarlayarak yapılandırılır. Üç değişkenleri şunlardır:

* **amqpSettings__enabled**
* **mqttSettings__enabled**
* **httpSettings__enabled**

Tüm üç değişkenleriniz *iki alt çizgi* ve true veya false olarak ayarlanabilir. 

#### <a name="reduce-storage-time-for-messages"></a>İletiler için depolama daraltır

Bunlar IOT Hub'ına herhangi bir nedenle teslim edilemiyor, Edge hub'ı modülü iletileri geçici olarak depolar. Ne kadar süreyle Edge hub'ı teslim edilmeyen iletiler için bunları sona izin vermeden önce tutan yapılandırabilirsiniz. Cihazınızda bellek endişeleriniz varsa, düşürebilirsiniz **timeToLiveSecs** Edge hub'ı modül ikizi değeri. 

İki saat olduğu 7200 saniye timeToLiveSecs parametresinin varsayılan değeri var. 

### <a name="do-not-use-debug-versions-of-module-images"></a>Hata ayıklama sürümleri modül görüntüleri kullanmayın

Test senaryolarından üretim senaryoları taşınırken hata ayıklama yapılandırmaları dağıtım bildirimlerinden kaldırmayı unutmayın. Dağıtım bildirimleri modül görüntüleri hiçbiri olup olmadığını denetleyin  **\.hata ayıklama** soneki. Oluşturma, hata ayıklama için modüller bağlantı noktalarını ortaya çıkarmak için seçenekleri eklediyseniz bu oluşturma kaldırma seçenekleri de. 

## <a name="container-management"></a>Kapsayıcı yönetimi

* **Önemli**
    * Kapsayıcı kayıt defterinizde erişimi yönetme
    * Sürümleri yönetmek için etiketleri kullanma

### <a name="manage-access-to-your-container-registry"></a>Kapsayıcı kayıt defterinizde erişimi yönetme

Modüller için üretim IOT Edge cihazları dağıtmadan önce böylece dışarıdaki kişilerin erişmek veya kapsayıcı görüntülerinizi değişiklik kapsayıcı kayıt defterinizde erişim denetim emin olun. Özel, genel, kapsayıcı kayıt defteri, kapsayıcı görüntülerini yönetmek için kullanın. 

Öğreticiler ve diğer belgeler, biz, geliştirme makinenizde kullanırken, IOT Edge Cihazınızda aynı kapsayıcı kayıt defteri kimlik bilgilerini kullanmak için isteyin. Bu yönergeler yalnızca test ve geliştirme ortamları daha kolay ayarlama yardımcı olmak için tasarlanmıştır ve bir üretim senaryosunda gelmelidir değil. Azure Container Registry önerir [kimlik doğrulama ile hizmet sorumluları](../container-registry/container-registry-auth-service-principal.md) , uygulamalar veya hizmetler çekme kapsayıcı görüntülerini otomatik olarak veya başka türlü katılımsız şekilde, IOT Edge cihazları gibi. Kapsayıcı kayıt defterinizde salt okunur erişimi olan bir hizmet sorumlusu oluşturma ve bu kullanıcı adını belirtin ve parola dağıtım bildirimi içinde.

### <a name="use-tags-to-manage-versions"></a>Sürümleri yönetmek için etiketleri kullanma

Bir etiket, docker kapsayıcıları sürümleri arasında ayrım yapmak için kullanabileceğiniz bir docker kavramıdır. Etiketlerdir gibi sonekleri **1.0** bir kapsayıcı deposuna ucunda gidin. Örneğin, **mcr.microsoft.com/azureiotedge-agent:1.0**. Etiketleri değişebilir ve takımınızın ilerletme, modül görüntüleri güncelleştirme olarak izlemek için bir kuralı kabul ediyorum için herhangi bir zamanda başka bir kapsayıcıya işaret edecek şekilde değiştirilebilir. 

Etiketler, IOT Edge cihazlarınıza güncelleştirmeleri uygulamak için de yardımcı olur. Kapsayıcı kayıt defterinizde bir modül güncelleştirilmiş bir sürümünü gönderdiğinizde, etiket artırın. Ardından, yeni bir dağıtım artan etiketiyle cihazlarınıza gönderin. Kapsayıcı altyapısı, yeni bir sürüm olarak artan etiketi algılar ve en son Modül sürümü cihazınıza çeker. 

Bir etiket kuralı örneği için bkz. [IOT Edge çalışma zamanı güncelleştirmesi](how-to-update-iot-edge.md#understand-iot-edge-tags) IOT Edge çalışırken etiketleri ve belirli etiketlere sürümünü izlemek için nasıl kullandığını öğrenin. 

## <a name="networking"></a>Ağ

* **Yararlı**
    * Giden/gelen yapılandırmayı gözden geçir
    * Beyaz liste bağlantıları
    * Bir proxy üzerinden iletişimi yapılandırma

### <a name="review-outboundinbound-configuration"></a>Giden/gelen yapılandırmayı gözden geçir

Azure IOT Hub ve IOT Edge arasındaki iletişim kanallarını, her zaman giden olacak şekilde yapılandırılır. IOT Edge senaryolar için yalnızca üç bağlantı gereklidir. Kapsayıcı altyapısı, kapsayıcı kayıt defteri (veya kayıt defterleri ile) modül görüntüleri tutan bağlanması gerekir. IOT Edge çalışma zamanı, IOT Hub ile cihaz yapılandırma bilgilerini almak ve iletileri ve telemetri göndermek için bağlantı gerekiyor. Ve IOT Edge arka plan programı otomatik sağlama kullanırsanız, cihaz sağlama Hizmeti'ne bağlanması gerekir. Daha fazla bilgi için [güvenlik duvarı ve bağlantı noktası yapılandırma kuralları](troubleshoot.md#firewall-and-port-configuration-rules-for-iot-edge-deployment).

### <a name="whitelist-connections"></a>Beyaz liste bağlantıları

Beyaz liste bağlantıları açıkça IOT Edge cihazları yapılan ağ kurulumunuz gerektiriyorsa, aşağıdaki IOT Edge bileşenlerin listesini gözden geçirin:

* **IOT Edge Aracısı** IOT Hub'ı kalıcı bir AMQP/MQTT bağlantı muhtemelen WebSockets üzerinden açılır. 
* **IOT Edge hub'ı** kalıcı tek bir AMQP bağlantısı veya IOT Hub'ı birden fazla MQTT bağlantı WebSockets üzerinden büyük olasılıkla açılır. 
* **IOT Edge arka plan programı** IOT hub'ına aralıklı HTTPS çağrıları yapar. 

Her üç durumda desen DNS adıyla eşleşmesi \*.azure devices.net. 

Ayrıca, **Container altyapısı** HTTPS üzerinden kapsayıcı kayıt defterleri için çağrılar. IOT Edge çalışma zamanı kapsayıcı görüntülerini almak için DNS mcr.microsoft.com adıdır. Kapsayıcı altyapısı diğer kayıt defterleri için Dağıtımda yapılandırılmış olarak bağlanır. 

Bu denetim, güvenlik duvarı kuralları için bir başlangıç noktasıdır:

   | URL (\* joker =) | Giden TCP bağlantı noktaları | Kullanım |
   | ----- | ----- | ----- |
   | mcr.microsoft.com  | 443 | Microsoft kapsayıcı kayıt defteri |
   | Global.Azure cihazları provisioning.net  | 443 | DPS erişim (isteğe bağlı) |
   | \*. azurecr.io | 443 | Kişisel ve 3. taraf kapsayıcı kayıt defterleri |
   | \*.blob.core.windows.net | 443 | Görüntü deltalarını indirin | 
   | \*.Azure devices.net | 5671, 8883, 443 | IOT hub'ı erişim |
   | \*. docker.io  | 443 | Docker Hub erişim (isteğe bağlı) |

### <a name="configure-communication-through-a-proxy"></a>Bir proxy üzerinden iletişimi yapılandırma

Cihazlarınızı bir ara sunucu kullanıyorsa bir ağ üzerinde dağıtılıp kullanacaksanız, bunların IOT Hub ile kapsayıcı kayıt defterleri ulaşmak için Ara sunucu iletişim kurabilmesi gerekir. Daha fazla bilgi için [bir proxy sunucu üzerinden iletişim kurmak için IOT Edge cihazı yapılandırma](how-to-configure-proxy-support.md).

## <a name="solution-management"></a>Çözüm Yönetimi

* **Yararlı**
    * Günlükleri ve tanılamayı ayarlama
    * Test ve CI/CD işlem hatları göz önünde bulundurun

### <a name="set-up-logs-and-diagnostics"></a>Günlükleri ve tanılamayı ayarlama

Linux üzerinde IOT Edge arka plan programının günlüklerini sürücü günlüğü varsayılan kullanır. Komut satırı aracı kullanabilirsiniz `journalctl` arka plan programı'nı sorgulamak için günlüğe kaydeder. Windows üzerinde PowerShell tanılama IOT Edge arka plan programı kullanır. Kullanım `Get-WinEvent` arka planından sorgu günlükleri. IOT Edge modülleri, varsayılan günlük kaydı için JSON sürücüsü kullanın.  

IOT Edge dağıtımı test ederken cihazlarınızı günlüklerini almak ve sorunları gidermek için genellikle erişebilirsiniz. Dağıtım senaryosunda, bu seçeneği olmayabilir. Cihazlarınızı üretimde hakkında bilgi toplamak için nasıl gideceğinizi göz önünde bulundurun. Diğer modüller bilgilerini toplar ve buluta gönderen bir günlük modülü kullanmak bir seçenektir. Bir örneği, bir günlük modülü [logspout loganalytics](https://github.com/veyalla/logspout-loganalytics), ya da kendi tasarlayabilirsiniz. 

### <a name="place-limits-on-log-size"></a>Günlük boyutu sınırları yerleştirin

Varsayılan olarak, kapsayıcı günlük boyutu sınırları Moby container altyapısı ayarlı değil. Zaman içinde bu cihazın günlükleriyle dolmaya ve disk boş alan tükeniyor neden olabilir. Bunu önlemek için aşağıdaki seçenekleri göz önünde bulundurun:

**Seçenek: Tüm kapsayıcı modüller için geçerli olan genel sınırlarını ayarlama**

Tüm kapsayıcı logfiles kapsayıcı altyapısı günlük seçenekleri boyutunu sınırlandırabilirsiniz. Aşağıdaki örnek log sürücü ayarlar `json-file` boyutu ve dosya sayısı sınırı (önerilen):

    {
        "log-driver": "json-file",
        "log-opts": {
            "max-size": "10m",
            "max-file": "3"
        }
    }

Bu bilgiler adlı bir dosyaya ekleyin (veya ekleme) `daemon.json` ve cihaz platformunuz için doğru konuma yerleştirin.

| Platform | Konum |
| -------- | -------- |
| Linux | `/etc/docker/` |
| Windows | `C:\ProgramData\iotedge-moby-data\config\` |

Kapsayıcı altyapısı değişikliklerin etkili olması için yeniden başlatılması gerekiyor.

**Seçenek: Her kapsayıcı modülü için günlük ayarları**

Bu nedenle de yapabilirsiniz **createOptions** her modülü. Örneğin:

    "createOptions": {
        "HostConfig": {
            "LogConfig": {
                "Type": "json-file",
                "Config": {
                    "max-size": "10m",
                    "max-file": "3"
                }
            }
        }
    }


**Linux sistemleri üzerindeki ek seçenekler**

* Günlükleri göndermek için kapsayıcı altyapısını yapılandırma `systemd` [günlük](https://docs.docker.com/config/containers/logging/journald/) ayarlayarak `journald` varsayılan günlük kaydı sürücüsü. 

* Düzenli aralıklarla eski günlüklerin logrotate aracı yükleyerek cihazınızdan kaldırın. Aşağıdaki dosya belirtimi kullanın: 

   ```
   /var/lib/docker/containers/*/*-json.log{
       copytruncate
       daily
       rotate7
       delaycompress
       compress
       notifempty
       missingok
   }
   ```

### <a name="consider-tests-and-cicd-pipelines"></a>Test ve CI/CD işlem hatları göz önünde bulundurun

En verimli IOT Edge dağıtım senaryosu için üretim dağıtımınızı test ve CI/CD işlem hatları ile tümleştirme göz önünde bulundurun. Azure IOT Edge, Azure DevOps dahil olmak üzere, birden fazla CI/CD platformları destekler. Daha fazla bilgi için [sürekli tümleştirme ve sürekli dağıtım için Azure IOT Edge](how-to-ci-cd.md).

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [IOT Edge otomatik dağıtım](module-deployment-monitoring.md).
* IOT Edge nasıl desteklediğini görün [sürekli tümleştirme ve sürekli dağıtım](how-to-ci-cd.md).
