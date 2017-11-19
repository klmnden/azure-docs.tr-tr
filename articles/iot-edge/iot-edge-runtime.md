---
title: "Azure IOT kenar çalışma zamanı anlama | Microsoft Docs"
description: "Azure IOT kenar çalışma zamanı ve nasıl kenar aygıtlarınızı güçlendirir hakkında bilgi edinin"
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 10/05/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 7b37f9e103644d2492f69f4a4cc80d3fd57d4aa4
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="understand-the-azure-iot-edge-runtime-and-its-architecture---preview"></a>Azure IOT kenar çalışma zamanı ve mimarisini anlama - Önizleme

IOT kenar çalışma zamanı cihazı IOT sınır cihazı kabul edilebilmesi için için yüklenmesi gereken programlar koleksiyonudur. Topluca IOT kenar çalışma zamanı bileşenleri sınırında çalıştırmak için kod almak IOT sınır cihazları etkinleştirin ve sonuçları iletişim. 

IOT kenar çalışma zamanı IOT kenar cihazlarda aşağıdaki işlevleri gerçekleştirir:

* Yükler ve iş yükleri cihazda güncelleştirir.
* Azure IOT kenar güvenlik standartları cihazda tutar.
* Sağlar [IOT kenar modülleri][lnk-modülleri] her zaman çalışması.
* Uzaktan izleme buluta Modül durumu raporları.
* Aşağı Akış yaprak aygıtlar ve IOT sınır cihazı arasındaki iletişimi kolaylaştırır.
* IOT sınır cihazı modülleri arasındaki iletişimi kolaylaştırır.
* IOT sınır cihazı ve bulut arasındaki iletişimi kolaylaştırır.

![IOT kenar çalışma zamanı Öngörüler ve IOT Hub'ına Modül durumu iletişim kurar][1]

IOT kenar çalışma zamanı sorumluluklarını iki kategoriye ayrılır: modülü yönetimi ve iletişim. Bu iki rol IOT kenar çalışma zamanı iki bileşen tarafından gerçekleştirilir. IOT kenar Aracısı dağıtma ve izleme modüllerini yönetir sırada kenar IOT hub'ı iletişim için sorumludur. 

Edge Aracısı'nı ve kenar hub'ı, bir IOT kenar cihazda çalışan yalnızca herhangi başka bir modül gibi modüllerdir. Modülleri nasıl çalıştığı hakkında daha fazla bilgi için bkz: [lnk-modülleri]. 

## <a name="iot-edge-hub"></a>IOT kenar hub

Edge hub'ı Azure IOT kenar çalışma zamanı yapmak iki modülleri biridir. Yerel bir ara IOT hub'ın IOT hub'ı aynı protokol uç noktalarını göstererek görür. Bu tutarlılık anlamına istemcileri (olup olmadığını aygıtları veya modülleri) IOT Hub'ına gibi IOT kenar çalışma zamanına bağlanabilir. 

>[!NOTE]
> Genel Önizleme sırasında kenar Hub MQTT kullanarak bağlanan istemciler yalnızca destekler.

Edge hub'ın IOT Hub'ın yerel olarak çalışan tam sürümü değil. Edge hub'ı sessizce IOT Hub'ına temsilciler bazı şeyler vardır. Örneğin, bir cihaz ilk bağlanmaya çalıştığında kenar hub IOT Hub'ına kimlik doğrulama isteklerini iletir. İlk bağlantı kurulduktan sonra güvenlik bilgileri kenar hub tarafından yerel olarak önbelleğe alınır. Bu aygıttan sonraki bağlantılar için bulut kimlik doğrulaması yapmak zorunda kalmadan izin verilir. 

>[!NOTE]
> Bir aygıtın kimliğini doğrulamaya çalışır her zaman genel Önizleme sırasında çalışma zamanı bağlanması gerekir.

Kaç tane gerçek bağlantı buluta yapılan kenar hub'ı en iyi duruma getirir, IOT kenar çözümünüzü bant genişliğini azaltmak üzere kullanır. Edge hub modülleri veya yaprak cihazları gibi istemcilerden mantıksal bağlantılar alır ve bunları tek bir fiziksel bağlantı bulut için bir araya getirir. Bu işlem ayrıntılarını, çözüm geri kalanı için saydamdır. İstemciler, bunların tümü aynı bağlantı üzerinden gönderilen olsa da kendi bağlantı buluta sahip oldukları düşünün. 

![Edge hub'ı birden çok fiziksel cihazlar ve bulut arasında bir ağ geçidi olarak görev yapar][2]

Edge hub IOT Hub'ına bağlı olup olmadığını belirleyebilirsiniz. Bağlantı kaybolursa, kenar hub iletileri veya twin güncelleştirmeleri yerel olarak kaydeder. Bir bağlantı kurulur kurulmaz sonra tüm verileri eşitler. Bu geçici önbelleği için kullanılan konumu bir sınır hub'ın modülü twin özelliği tarafından belirlenir. Önbelleğin boyutunu değil tutulabilir ve cihaz depolama kapasitesine sahip sürece büyüyecektir. 

>[!NOTE]
>Genel kullanılabilirlik geçirilmeden önce ek önbelleğe alma parametreler üzerinde denetim ekleme ürününe eklenir.

### <a name="module-communication"></a>Modül iletişimi

Edge Hub modül için modülü iletişimi kolaylaştırır. İleti Aracısı Edge hub'ı kullanarak modülleri birbirinden bağımsız tutar. Modüller yalnızca üzerinde iletileri ve bunların iletileri yazma çıkışları kabul girişleri belirtmeniz gerekir. Bir çözüm geliştirici sonra bu girişleri bitiştirir ve böylece bu çözüme belirli sırayla verileri modülleri işleme birlikte çıkartır. 

![Edge Hub modülü modülü iletişimi kolaylaştırır][3]

Edge hub'ına veri göndermek için bir modül SendEventAsync yöntemini çağırır. İlk bağımsız değişken ileti göndermek için hangi çıkış belirtir. Aşağıdaki yarı kodu output1 üzerinde bir ileti gönderir:

    DeviceClient client = new DeviceClient.CreateFromConnectionString(moduleConnectionString, settings); 
    await client.OpenAsync(); 
    await client.SendEventAsync(“output1”, message); 

Bir ileti almak için belirli bir giriş, gelen iletileri işleyen bir geri çağırma kaydedin. Aşağıdaki yarı kodu üzerinde input1 alınan tüm iletileri işlemek için kullanılacak işlevi messageProcessor kaydeder:

    await client.SetEventHandlerAsync(“input1”, messageProcessor, userContext);
    
Çözüm Geliştirici kenar hub iletileri modülleri arasında nasıl geçirir belirleyen kuralları belirtmek için sorumludur. Yönlendirme kuralları bulutta tanımlanır ve cihaz çiftine kenar hub'ına gönderilen. IOT hub'ı rotalar için aynı sözdizimini Azure IOT kenar modülleri arasında rotaları tanımlamak için kullanılır. 

<!--- For more info on how to declare routes between modules, see []. --->   

![Modülleri arasındaki yolları][4]

## <a name="iot-edge-agent"></a>IOT kenar Aracısı

Azure IOT kenar çalışma zamanı yapar başka bir modül IOT kenar aracısıdır. Modülleri başlatmasını, çalışmaya devam olduktan ve modülleri durumunu IOT Hub'ına geri raporlama sorumludur. Yalnızca herhangi başka bir modül gibi bu yapılandırma verilerini depolamak için kendi modülü twin kenar Aracısı'nı kullanır. 

Edge Aracısı yürütülmesi başlamak için azure-IOT-edge-çalışma zamanı-ctl.py başlangıç komutunu çalıştırın. Aracı kendi modülü twin IOT Hub'ından alır ve modülleri sözlük inceler. Modülleri sözlük başlatılması gerekir modülleri koleksiyonudur. 

Modülleri sözlükteki her öğe bir modül hakkında belirli bilgiler içerir ve kenar aracı tarafından modülün yaşam döngüsü denetlemek için kullanılır. Daha ilginç özelliklerden bazıları şunlardır: 

* **Settings.image** – kapsayıcı görüntünün modülü başlatmak için sınır Aracısı'nı kullanır. Görüntünün bir parolayla korunuyorsa kenar Aracısı kapsayıcı kayıt defteri için kimlik bilgileri ile yapılandırılması gerekir. Edge aracısını yapılandırmak için aşağıdaki komutu kullanın:`azure-iot-edge-runtime-ctl.py –configure`
* **settings.createOptions** – doğrudan Docker daemon bir modülün kapsayıcısı başlatılırken geçirilen bir dize. Bu özellik Docker seçenekleri ekleme iletme veya bir modülün kapsayıcıya birimleri bağlama bağlantı noktası gibi gelişmiş seçenekler sağlar.  
* **Durum** – kenar Aracısı Modülü yerleştirir durumu. Bu değer genellikle ayarlamak *çalıştıran* tüm modülleri cihazda hemen başlatmak için sınır Aracısı çoğu kişi istediğiniz şekilde. Ancak, bir modül başlatmak için sınır Aracısı bildirmek gelecekteki bir süre bekleyin ve durdurulması bir modül ilk durumunu belirtebilirsiniz. Edge Aracısı her modül durumunu bildirilen özelliklerinde buluta geri raporlar. İstenen özelliği ve bildirilen özelliği arasında bir fark bir gösterge veya hatalı davranan bir aygıtı değil. Desteklenen durumlar şunlardır:
   * İndirme
   * Çalışıyor
   * İyi durumda değil
   * Başarısız
   * Durduruldu
* **restartPolicy** – nasıl bir modül kenar aracıyı yeniden başlatır. Olası değerler şunlardır:
   * – Kenar Aracısı'nı hiçbir zaman modülü yeniden başlatmaz.
   * onFailure - modülü çökerse, kenar Aracısı'nı yeniden başlatır. Modül düzgün bir şekilde kapanırsa, kenar Aracısı, yeniden başlatmaz.
   * Modül çöküyor veya sağlıksız - sağlıksız kabul edilip kenar aracıyı yeniden başlatır.
   * Modül kilitlenmeler, sağlıksız kabul edilir veya herhangi bir şekilde kapanır, her zaman - Edge Aracısı'nı yeniden başlatır. 
   
### <a name="security"></a>Güvenlik

IOT kenar Aracısı'nı bir IOT uç cihazın güvenliği kritik rol oynar. Örneğin, bir modülün görüntü başlatmadan önce doğrulama gibi işlemleri gerçekleştirir. Bu özellikler V2 özellikleri genel kullanılabilirliğine eklenir. 

<!-- For more information about the Azure IoT Edge security framework, see []. -->

## <a name="next-steps"></a>Sonraki adımlar

- [Azure IOT kenar modülleri anlamak][lnk-modülleri]

<!-- Images -->
[1]: ./media/iot-edge-runtime/pipeline.png
[2]: ./media/iot-edge-runtime/gateway.png
[3]: ./media/iot-edge-runtime/ModuleEndpoints.png
[4]: ./media/iot-edge-runtime/ModuleEndpointsWithRoutes.png

<!-- Links -->
[lnk-modülleri]: iot-edge-modules.md