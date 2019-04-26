---
title: Çalışma zamanı cihazlar - Azure IOT Edge nasıl yönettiğini öğrenin | Microsoft Docs
description: Bilgi nasıl modülleri, güvenlik, iletişim ve cihazlarınızda raporlama Azure IOT Edge çalışma zamanı tarafından yönetilir
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 03/13/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: bb2df9c32d5adc8160da82148e4a66a4ab68d182
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60363708"
---
# <a name="understand-the-azure-iot-edge-runtime-and-its-architecture"></a>Azure IOT Edge çalışma zamanı ve mimarisini anlama

IOT Edge çalışma zamanı, IOT Edge cihazı kabul edilebilmesi için bir cihaz üzerinde yüklenmesi gereken programlar koleksiyonudur. Toplu olarak, IOT Edge çalışma zamanı bileşenlerini IOT Edge cihazları ucuna çalıştırmak için kod almayı etkinleştirin ve sonuçları iletişim. 

IOT Edge çalışma zamanı, IOT Edge cihazlarında aşağıdaki işlevleri gerçekleştirir:

* Yükleme ve cihazda iş yüklerini güncelleştirin.
* Cihazda Azure IOT Edge güvenlik standartlarını korur.
* Emin [IOT Edge modülleri](iot-edge-modules.md) her zaman çalışıyor.
* Bulutta Uzaktan izleme için modül durumunu rapor.
* Aşağı Akış yaprak cihazlarıyla IOT Edge cihazları arasındaki iletişimi kolaylaştırır.
* IOT Edge cihazı bulunan modüller arasındaki iletişimi kolaylaştırır.
* IOT Edge cihazı ve bulut arasındaki iletişimi kolaylaştırır.

![Çalışma zamanı öngörüleri ve IOT hub'ına modül durumunu iletişim kurar.](./media/iot-edge-runtime/Pipeline.png)

IOT Edge çalışma zamanı sorumluluklarını iki kategoriye ayrılır: iletişim ve modül yönetimi. Bu iki rolden IOT Edge çalışma zamanını oluşturan iki bileşen tarafından gerçekleştirilir. *IOT Edge hub'ı* iletişimi için sorumlu olduğu sırada *IOT Edge Aracısı* dağıtır ve modülleri izler. 

IOT Edge hub'ı hem de IOT Edge Aracısı, bir IOT Edge cihaz üzerinde çalışan yalnızca diğer modüllerin gibi modüllerdir. 

## <a name="iot-edge-hub"></a>IOT Edge hub'ı

IOT Edge hub'ı Azure IOT Edge çalışma zamanını oluşturan iki modül biridir. IOT hub'ın yerel bir ara sunucu olarak IOT hub'ı aynı protokol uç noktalarını göstererek görür. Bu tutarlılık anlamına istemciler (olmadığını cihazlar veya modülleri) IOT Edge çalışma zamanı, IOT Hub'ına gibi bağlanabilirsiniz. 

>[!NOTE]
> IOT Edge hub'ı, MQTT veya AMQP kullanarak bağlanma istemcileri destekler. HTTP kullanan istemcileri desteklemez. 

IOT Edge hub'ı yerel olarak çalışan IOT Hub'ın tam bir sürüm değil. IOT Edge hub'ı sessizce IOT Hub'ına atar bazı şeyler vardır. Örneğin, bir cihaz ilk kez bağlanmaya çalıştığında IOT Edge hub'ı kimlik doğrulama isteklerini IOT Hub'ına iletir. Birinciden sonra bağlantının kurulduğundan, güvenlik bilgilerini IOT Edge hub'ı tarafından yerel olarak önbelleğe alınır. Daha sonraki bağlantılar, CİHAZDAN buluta kimlik doğrulaması yapmak zorunda kalmadan izin verilir. 

>[!NOTE]
>Bir cihazın kimliğini doğrulamak her denediğinde çalışma zamanı'e bağlı olması gerekir.

IOT Edge hub'ı buluta kaç gerçek bağlantı yapılan iyileştirir, IOT Edge çözümünüzün bant genişliğini azaltmak üzere kullanır. IOT Edge hub'ı, modülleri veya yaprak cihazlar gibi istemcilerden mantıksal bağlantıları alır ve bunları bulut tek bir fiziksel bağlantı için birleştirir. Bu işlemin ayrıntılarını çözümün geri kalanı için saydamdır. İstemciler, bunların tümü aynı bağlantı üzerinden gönderilen olsa da kendi bağlantı buluta sahip oldukları düşünün. 

![IOT Edge hub'ı olan IOT Hub ile fiziksel cihazlar arasında ağ geçidi](./media/iot-edge-runtime/Gateway.png)

IOT Edge hub'ı, IOT Hub'ına bağlı olup olmadığını belirleyebilirsiniz. Bağlantı kaybedilirse IOT Edge hub'a iletileri veya ikizi güncelleştirmeleri yerel olarak kaydeder. Bağlantı yeniden sonra tüm verileri eşitler. Bu geçici önbelleği için kullanılan konum, bir IOT Edge hub'ın modül ikizi özelliği tarafından belirlenir. Önbelleğin boyutunu değil tavan ve cihaz depolama kapasitesine sahip sürece büyüyecektir. 

### <a name="module-communication"></a>Modül iletişimi

IOT Edge hub'ı modül için modülü iletişimi kolaylaştırır. IOT Edge kullanarak hub'a bir ileti aracısı olarak modülleri birbirinden bağımsız tutar. Modüller yalnızca üzerinde iletileri ve bunlar iletileri yazma çıkışları kabul girişleri belirtmeniz gerekir. Bir çözüm geliştirici bu girişlerin bitiştirir ve böylece modüller sırayla bu çözüme özel veri işleme birlikte çıkarır. 

![IOT Edge hub'ı modülü modülü iletişimi kolaylaştırır.](./media/iot-edge-runtime/module-endpoints.png)

IOT Edge hub'ına veri göndermek için bir modül SendEventAsync yöntemi çağırır. İletiyi göndermek için hangi çıkış ilk bağımsız değişken belirtir. Aşağıdaki sözde kod output1 üzerinde bir ileti gönderir:

   ```csharp
   ModuleClient client = new ModuleClient.CreateFromEnvironmentAsync(transportSettings); 
   await client.OpenAsync(); 
   await client.SendEventAsync(“output1”, message); 
   ```

Bir ileti almak için belirli bir girdi gelen iletileri işleyen bir geri çağırma kaydedin. Aşağıdaki sözde kod üzerinde input1 alınan tüm iletileri işlemek için kullanılacak işlev messageProcessor kaydeder:

   ```csharp
   await client.SetInputMessageHandlerAsync(“input1”, messageProcessor, userContext);
   ```

Tercih ettiğiniz SDK dili için API Başvurusu ModuleClient sınıfı ve kendi iletişim yöntemleri hakkında daha fazla bilgi için bkz: [C#](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.moduleclient?view=azure-dotnet), [C ve Python](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/iothub-module-client-h), [Java](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device.moduleclient?view=azure-java-stable), veya [Node.js](https://docs.microsoft.com/javascript/api/azure-iot-device/moduleclient?view=azure-node-latest).

Çözüm Geliştirici nasıl IOT Edge hub'ı modüller arasında iletileri geçirir belirleyen kuralları belirtmek için sorumludur. Yönlendirme kuralları bulutta tanımlanır ve cihaz ikizi, IOT Edge hub'ına gönderilen. IOT hub'ı yolları aynı sözdizimi, Azure IOT edge'deki modüller arasında tanımlamak için kullanılır. 

<!--- For more info on how to declare routes between modules, see []. --->   

![IOT Edge hub'ı aracılığıyla modüller arasında Git](./media/iot-edge-runtime/module-endpoints-with-routes.png)

## <a name="iot-edge-agent"></a>IOT Edge Aracısı

IOT Edge, Azure IOT Edge çalışma zamanını oluşturan yapan başka bir modül aracısıdır. Modülleri örnekleme, çalışmaya devam sağlama ve IOT Hub'ına modüllerinin durumunu raporlamaya sorumludur. Yalnızca diğer modüllerin gibi bu yapılandırma verilerini depolamak için kendi modül ikizi IOT Edge Aracısı'nı kullanır. 

[IOT Edge güvenlik arka plan programı](iot-edge-security-manager.md) cihaz başlangıçta IOT Edge Aracısı'nı başlatır. Aracı kendi modül ikizi, IOT Hub'ından alır ve dağıtım bildirimini inceler. Dağıtım bildirimi başlatılması gereken modülleri bildiren bir JSON dosyasıdır. 

Dağıtım bildirimi içinde her öğe bir modülle ilgili belirli bilgileri içerir ve modülün yaşam döngüsü denetlemek için IOT Edge aracısı tarafından kullanılır. Bazı daha ilgi çekici özellikleri şunlardır: 

* **Settings.image** – modülü başlatmak için IOT Edge Aracısı'nı kullanan bir kapsayıcı görüntüsü. Görüntünün bir parolayla korunuyorsa, IOT Edge Aracısı kapsayıcı kayıt defteri kimlik bilgileri ile yapılandırılması gerekir. Uzaktan dağıtım bildirimini kullanarak kapsayıcı kayıt defteri yapılandırılabilir veya IOT Edge cihazında kimlik bilgilerini güncelleştirerek `config.yaml` IOT Edge program klasöründeki dosya.
* **settings.createOptions** – doğrudan Docker Daemon programını bir modülün kapsayıcı başlatma sırasında geçirilen bir dize. Bu özellik Docker seçenekleri ekleme iletme veya birimleri bir modülün kapsayıcıya bağlama bağlantı noktası gibi gelişmiş seçenekleri sağlar.  
* **Durum** – durumu IOT Edge Aracısı Modülü yerleştirir. Bu değer genellikle kümesine *çalıştıran* çoğu kişi IOT Edge Aracısı tüm modüller cihazda hemen başlatmak için istediğiniz kadar. Ancak, yapının başlangıç durumunun durdurulması ve IOT Edge Aracısı bir modülün başlatmak için bahsetmenin gelecekteki bir zamanı için beklemesi için bir modül belirtebilirsiniz. IOT Edge Aracısı'nı her modül durumunu buluta bildirilen özellikler geri bildirir. İstenen özellik ve bildirilen özellik arasında bir fark davranan bir cihaz bir göstergesidir. Desteklenen durumlar şunlardır:
   * İndiriliyor
   * Çalışıyor
   * İyi durumda değil
   * Başarısız
   * Durduruldu
* **restartPolicy** : nasıl IOT Edge Aracısı bir modülün yeniden başlatır. Olası değerler şunlardır:
   * Hiçbir zaman – IOT Edge Aracısı'nı hiçbir zaman modülü yeniden başlatır.
   * onFailure - modül çökerse, IOT Edge Aracısı'nı yeniden başlatır. Modül temiz bir şekilde kapanırsa, IOT Edge Aracısı, yeniden başlatılmaz.
   * Sağlıksız modül kilitleniyor veya - sağlıksız çalışıp, IOT Edge Aracısı'nı yeniden başlatır.
   * Modül kilitlenmeler, sağlıksız olarak kabul veya herhangi bir şekilde kapanıyorsa öyle kapanır, her zaman - IOT Edge Aracısı'nı yeniden başlatır. 

IOT Edge çalışma zamanı yanıtı IOT hub'a gönderir. Olası yanıtların listesi aşağıda verilmiştir:
  * 200 - TAMAM
  * 400 - dağıtım yapılandırması bozuk veya geçersiz.
  * 417 - cihaz, bir dağıtım yapılandırma kümesi yok.
  * 412 - dağıtım yapılandırma şeması sürümü geçersiz.
  * 406 - IOT Edge cihaz çevrimdışı veya göndermiyor durum raporları ' dir.
  * 500 - IOT Edge çalışma zamanı'nda hata oluştu.

### <a name="security"></a>Güvenlik

IOT Edge Aracısı bir IOT Edge cihazının güvenliği kritik rol oynar. Örneğin, bir modülün görüntüsüne başlatmadan önce doğrulama gibi eylemleri gerçekleştirir. 

Azure IOT Edge güvenlik çerçevesi hakkında daha fazla bilgi için okuyun [IOT Edge Güvenlik Yöneticisi](iot-edge-security-manager.md)

## <a name="next-steps"></a>Sonraki adımlar

[Azure IOT Edge sertifikaları anlama](iot-edge-certs.md)
