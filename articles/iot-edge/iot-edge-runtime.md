---
title: Azure IOT Edge çalışma zamanı anlama | Microsoft Docs
description: Azure IOT Edge çalışma zamanı ve bu uç cihazlarınıza nasıl güçlendirdiğini öğrenin
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 08/13/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 97a2180aaf236d3541cff30d2151f26ce70b14af
ms.sourcegitcommit: b4a46897fa52b1e04dd31e30677023a29d9ee0d9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49393483"
---
# <a name="understand-the-azure-iot-edge-runtime-and-its-architecture"></a>Azure IOT Edge çalışma zamanı ve mimarisini anlama

IOT Edge çalışma zamanı, IOT Edge cihazı kabul edilebilmesi için bir cihaz üzerinde yüklenmesi gereken programlar koleksiyonudur. Toplu olarak, IOT Edge çalışma zamanı bileşenlerini IOT Edge cihazları ucuna çalıştırmak için kod almayı etkinleştirin ve sonuçları iletişim. 

IOT Edge çalışma zamanı, IOT Edge cihazlarında aşağıdaki işlevleri gerçekleştirir:

* Cihazda iş yüklerini yükler ve güncelleştirir.
* Cihazda Azure IoT Edge güvenlik standartlarını korur.
* Sağlar [IOT Edge modülleri](iot-edge-modules.md) her zaman çalışıyor.
* Uzaktan izleme için modül durumunu buluta bildirir.
* Aşağı Akış yaprak cihazlarıyla IOT Edge cihazları arasındaki iletişimi kolaylaştırır.
* IoT Edge cihazında bulunan modüller arasındaki iletişimi kolaylaştırır.
* IoT Edge cihazıyla bulut arasındaki iletişimi kolaylaştırır.

![IOT Edge çalışma zamanı öngörüleri ve IOT hub'ına modül durumunu iletişim kurar.](./media/iot-edge-runtime/Pipeline.png)

IOT Edge çalışma zamanı sorumluluklarını iki kategoriye ayrılır: iletişim ve modül yönetimi. Bu iki rolden IOT Edge çalışma zamanını oluşturan iki bileşen tarafından gerçekleştirilir. IOT Edge hub'ı, IOT Edge Aracısı'nı dağıtma ve izleme modüllerini yönetirken siz iletişimi için sorumludur. 

Edge hub'ı hem Edge Aracısı, bir IOT Edge cihaz üzerinde çalışan yalnızca diğer modüllerin gibi modüllerdir. 

## <a name="iot-edge-hub"></a>IOT Edge hub'ı

Edge hub'ı Azure IOT Edge çalışma zamanını oluşturan iki modül biridir. IOT hub'ın yerel bir ara sunucu olarak IOT hub'ı aynı protokol uç noktalarını göstererek görür. Bu tutarlılık anlamına istemciler (olmadığını cihazlar veya modülleri) IOT Edge çalışma zamanı, IOT Hub'ına gibi bağlanabilirsiniz. 

>[!NOTE]
>Edge hub'ı, MQTT veya AMQP kullanarak bağlanma istemcileri destekler. HTTP kullanan istemcileri desteklemez. 

Edge hub'ı yerel olarak çalışan IOT Hub'ın tam bir sürüm değil. Edge hub'ı sessizce IOT Hub'ına atar bazı şeyler vardır. Örneğin, bir cihaz ilk kez bağlanmaya çalıştığında Edge hub'ı kimlik doğrulama isteklerini IOT Hub'ına iletir. İlk bağlantı kurulduktan sonra güvenlik bilgilerini Edge hub'ı yerel olarak önbelleğe alınır. Daha sonraki bağlantılar, CİHAZDAN buluta kimlik doğrulaması yapmak zorunda kalmadan izin verilir. 

>[!NOTE]
>Bir cihazın kimliğini doğrulamak her denediğinde çalışma zamanı'e bağlı olması gerekir.

Edge hub'ı buluta kaç gerçek bağlantı yapılan iyileştirir, IOT Edge çözümünüzün bant genişliğini azaltmak üzere kullanır. Edge hub'ı, modülleri veya yaprak cihazlar gibi istemcilerden mantıksal bağlantıları alır ve bunları buluta tek bir fiziksel bağlantısı için birleştirir. Bu işlemin ayrıntılarını çözümün geri kalanı için saydamdır. İstemciler, bunların tümü aynı bağlantı üzerinden gönderilen olsa da kendi bağlantı buluta sahip oldukları düşünün. 

![Edge hub'ı birden fazla fiziksel cihazlar ve bulut arasında bir ağ geçidi olarak davranır](./media/iot-edge-runtime/Gateway.png)

Edge hub'ı, IOT Hub'ına bağlı olup olmadığını belirleyebilirsiniz. Bağlantı kaybedilirse Edge hub'a iletileri veya ikizi güncelleştirmeleri yerel olarak kaydeder. Bağlantı yeniden sonra tüm verileri eşitler. Bu geçici önbelleği için kullanılan konum, Edge hub'ın modül ikizi bir özelliği tarafından belirlenir. Önbelleğin boyutunu değil tavan ve cihaz depolama kapasitesine sahip sürece büyüyecektir. 

### <a name="module-communication"></a>Modül iletişimi

Edge hub'ı modül için modülü iletişimi kolaylaştırır. Edge hub'ı kullanarak bir ileti aracısı olarak modülleri birbirinden bağımsız tutar. Modüller yalnızca üzerinde iletileri ve bunlar iletileri yazma çıkışları kabul girişleri belirtmeniz gerekir. Bir çözüm geliştirici bu girişlerin bitiştirir ve böylece modüller sırayla bu çözüme özel veri işleme birlikte çıkarır. 

![Edge hub'ı modülü modülü iletişimi kolaylaştırır.](./media/iot-edge-runtime/ModuleEndpoints.png)

Edge hub'ına veri göndermek için bir modül SendEventAsync yöntemi çağırır. İletiyi göndermek için hangi çıkış ilk bağımsız değişken belirtir. Aşağıdaki sözde kod output1 üzerinde bir ileti gönderir:

   ```csharp
   ModuleClient client = new ModuleClient.CreateFromEnvironmentAsync(transportSettings); 
   await client.OpenAsync(); 
   await client.SendEventAsync(“output1”, message); 
   ```

Bir ileti almak için belirli bir girdi gelen iletileri işleyen bir geri çağırma kaydedin. Aşağıdaki sözde kod üzerinde input1 alınan tüm iletileri işlemek için kullanılacak işlev messageProcessor kaydeder:

   ```csharp
   await client.SetEventHandlerAsync(“input1”, messageProcessor, userContext);
   ```

Çözüm Geliştirici nasıl Edge hub'ı modüller arasında iletileri geçirir belirleyen kuralları belirtmek için sorumludur. Yönlendirme kuralları bulutta tanımlanır ve kendi cihaz ikizi Edge hub'ına gönderilen. IOT hub'ı yolları aynı sözdizimi, Azure IOT edge'deki modüller arasında tanımlamak için kullanılır. 

<!--- For more info on how to declare routes between modules, see []. --->   

![Modüller arasında](./media/iot-edge-runtime/ModuleEndpointsWithRoutes.png)

## <a name="iot-edge-agent"></a>IOT Edge Aracısı

IOT Edge, Azure IOT Edge çalışma zamanını oluşturan yapan başka bir modül aracısıdır. Modülleri örnekleme, çalışmaya devam sağlama ve IOT Hub'ına modüllerinin durumunu raporlamaya sorumludur. Yalnızca diğer modüllerin gibi bu yapılandırma verilerini depolamak için kendi modül ikizi Edge Aracısı'nı kullanır. 

[IOT Edge güvenlik arka plan programı](iot-edge-security-manager.md) cihaz başlangıçta Edge Aracısı'nı başlatır. Aracı kendi modül ikizi, IOT Hub'ından alır ve dağıtım bildirimini inceler. Dağıtım bildirimi başlatılması gereken modülleri bildiren bir JSON dosyasıdır. 

Dağıtım bildirimi içinde her öğe bir modülle ilgili belirli bilgileri içerir ve Edge aracısı tarafından modülün yaşam döngüsü denetlemek için kullanılır. Bazı daha ilgi çekici özellikleri şunlardır: 

* **Settings.image** – modülü başlatmak için Edge Aracısı'nı kullanan bir kapsayıcı görüntüsü. Görüntünün bir parolayla korunuyorsa, Edge Aracısı kapsayıcı kayıt defteri kimlik bilgileri ile yapılandırılması gerekir. Güncelleştirerek için uzaktan dağıtım bildirimini kullanarak kapsayıcı kayıt defteri yapılandırılabilir veya Edge cihazında kimlik bilgilerini `config.yaml` IOT Edge program klasöründeki dosya.
* **settings.createOptions** – doğrudan Docker Daemon programını bir modülün kapsayıcı başlatma sırasında geçirilen bir dize. Bu özellik Docker seçenekleri ekleme iletme veya birimleri bir modülün kapsayıcıya bağlama bağlantı noktası gibi gelişmiş seçenekleri sağlar.  
* **Durum** – Edge Aracısı Modülü yerleştirir durumu. Bu değer genellikle kümesine *çalıştıran* çoğu kişi, Edge Aracısı tüm modüller cihazda hemen başlatmak için istediğiniz kadar. Ancak, yapının başlangıç durumunun durdurulması ve Edge Aracısı bir modülün başlatmak için bahsetmenin gelecekteki bir zamanı için beklemesi için bir modül belirtebilirsiniz. Edge Aracısı her modül durumunu buluta bildirilen özellikler geri bildirir. İstenen özellik ve bildirilen özellik arasında bir fark davranan bir cihaz bir göstergesidir. Desteklenen durumlar şunlardır:
   * İndiriliyor
   * Çalışıyor
   * İyi durumda değil
   * Başarısız
   * Durduruldu
* **restartPolicy** : Edge Aracısı bir modülün yeniden nasıl. Olası değerler şunlardır:
   * Hiçbir zaman – Edge Aracısı'nı hiçbir zaman modülü yeniden başlatır.
   * onFailure - modül çökerse, Edge Aracısı'nı yeniden başlatır. Modül temiz bir şekilde kapanırsa, Edge Aracısı, yeniden başlatılmaz.
   * Sağlıksız modül kilitleniyor veya - sağlıksız çalışıp, Edge Aracısı'nı yeniden başlatır.
   * Modül kilitlenmeler, sağlıksız olarak kabul veya herhangi bir şekilde kapanıyorsa öyle kapanır, her zaman - Edge Aracısı'nı yeniden başlatır. 

IOT Edge çalışma zamanı yanıtı IOT hub'a gönderir. Olası yanıtların listesi aşağıda verilmiştir:
  * 200 - TAMAM
  * 400 - dağıtım yapılandırması bozuk veya geçersiz.
  * 417 - cihaz, bir dağıtım yapılandırma kümesi yok.
  * 412 - dağıtım yapılandırma şeması sürümü geçersiz.
  * 406 - çevrimdışı veya göndermiyor durum raporları edge cihazdır.
  * 500 - edge çalışma zamanı'nda hata oluştu.

### <a name="security"></a>Güvenlik

IOT Edge Aracısı bir IOT Edge cihazının güvenliği kritik rol oynar. Örneğin, bir modülün görüntüsüne başlatmadan önce doğrulama gibi eylemleri gerçekleştirir. 

Azure IOT Edge güvenlik çerçevesi hakkında daha fazla bilgi için okuyun [IOT Edge Güvenlik Yöneticisi](iot-edge-security-manager.md)

## <a name="next-steps"></a>Sonraki adımlar

[Azure IOT Edge sertifikaları anlama](iot-edge-certs.md)
