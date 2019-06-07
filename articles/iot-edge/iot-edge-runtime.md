---
title: Çalışma zamanı cihazlar - Azure IOT Edge nasıl yönettiğini öğrenin | Microsoft Docs
description: Bilgi nasıl modülleri, güvenlik, iletişim ve cihazlarınızda raporlama Azure IOT Edge çalışma zamanı tarafından yönetilir
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/06/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 423825540c02d2788de7a6148ddcec3c654fd450
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66754797"
---
# <a name="understand-the-azure-iot-edge-runtime-and-its-architecture"></a>Azure IOT Edge çalışma zamanı ve mimarisini anlama

IOT Edge çalışma zamanı, bir cihaz IOT Edge cihazının kapatma programlar koleksiyonudur. Toplu olarak, IOT Edge çalışma zamanı bileşenleri IOT Edge cihazları ucuna çalıştırmak için kod almayı etkinleştirin ve sonuçları iletişim. 

IOT Edge çalışma zamanı, IOT Edge cihazlarında aşağıdaki işlevleri gerçekleştirir:

* Yükleme ve cihazda iş yüklerini güncelleştirin.
* Cihazda Azure IOT Edge güvenlik standartlarını korur.
* Emin [IOT Edge modülleri](iot-edge-modules.md) her zaman çalışıyor.
* Bulutta Uzaktan izleme için modül durumunu rapor.
* Aşağı Akış yaprak cihazlarıyla IOT Edge cihazları arasındaki iletişimi kolaylaştırır.
* IOT Edge cihazı bulunan modüller arasındaki iletişimi kolaylaştırır.
* IOT Edge cihazı ve bulut arasındaki iletişimi kolaylaştırır.

![Çalışma zamanı öngörüleri ve IOT hub'ına modül durumunu iletişim kurar.](./media/iot-edge-runtime/Pipeline.png)

IOT Edge çalışma zamanı sorumluluklarını iki kategoriye ayrılır: iletişim ve modül yönetimi. Bu iki rolden IOT Edge çalışma zamanı'nın parçası olan iki bileşenleri tarafından gerçekleştirilir. *IOT Edge hub'ı* iletişimi için sorumlu olduğu sırada *IOT Edge Aracısı* dağıtır ve modülleri izler. 

IOT Edge hub'ı hem de IOT Edge Aracısı, bir IOT Edge cihaz üzerinde çalışan yalnızca diğer modüllerin gibi modüllerdir. 

## <a name="iot-edge-hub"></a>IOT Edge hub'ı

IOT Edge hub'ı Azure IOT Edge çalışma zamanını oluşturan iki modül biridir. IOT hub'ın yerel bir ara sunucu olarak IOT hub'ı aynı protokol uç noktalarını göstererek görür. Bu tutarlılık anlamına istemciler (olmadığını cihazlar veya modülleri) IOT Edge çalışma zamanı, IOT Hub'ına gibi bağlanabilirsiniz. 

>[!NOTE]
> IOT Edge hub'ı, MQTT veya AMQP kullanarak bağlanma istemcileri destekler. HTTP kullanan istemcileri desteklemez. 

IOT Edge hub'ı yerel olarak çalışan IOT Hub'ın tam bir sürüm değil. IOT Edge hub'ı sessizce IOT Hub'ına atar bazı şeyler vardır. Örneğin, bir cihaz ilk kez bağlanmaya çalıştığında IOT Edge hub'ı kimlik doğrulama isteklerini IOT Hub'ına iletir. Birinciden sonra bağlantının kurulduğundan, güvenlik bilgilerini IOT Edge hub'ı tarafından yerel olarak önbelleğe alınır. Daha sonraki bağlantılar, CİHAZDAN buluta kimlik doğrulaması yapmak zorunda kalmadan izin verilir. 

IOT Edge hub'ı buluta kaç gerçek bağlantı yapılan iyileştirir, IOT Edge çözümünüzün bant genişliğini azaltmak üzere kullanır. IOT Edge hub'ı, modülleri veya yaprak cihazlar gibi istemcilerden mantıksal bağlantıları alır ve bunları bulut tek bir fiziksel bağlantı için birleştirir. Bu işlemin ayrıntılarını çözümün geri kalanı için saydamdır. İstemciler, bunların tümü aynı bağlantı üzerinden gönderilen olsa da kendi bağlantı buluta sahip oldukları düşünün. 

![IOT Edge hub'ı olan IOT Hub ile fiziksel cihazlar arasında ağ geçidi](./media/iot-edge-runtime/Gateway.png)

IOT Edge hub'ı, IOT Hub'ına bağlı olup olmadığını belirleyebilirsiniz. Bağlantı kaybedilirse IOT Edge hub'a iletileri veya ikizi güncelleştirmeleri yerel olarak kaydeder. Bağlantı yeniden sonra tüm verileri eşitler. Bu geçici önbelleği için kullanılan konum, bir IOT Edge hub'ın modül ikizi özelliği tarafından belirlenir. Önbelleğin boyutunu değil tavan ve cihaz depolama kapasitesine sahip sürece büyüyecektir. Daha fazla bilgi için [çevrimdışı özellikleri](offline-capabilities.md).

### <a name="module-communication"></a>Modül iletişimi

IOT Edge hub'ı modül için modülü iletişimi kolaylaştırır. IOT Edge kullanarak hub'a bir ileti aracısı olarak modülleri birbirinden bağımsız tutar. Modüller yalnızca üzerinde iletileri ve bunlar iletileri yazma çıkışları kabul girişleri belirtmeniz gerekir. Modüller sırayla bu çözüme özel veri işleme, çözüm geliştirici bu girişler ve çıkışlar birleştirmek. 

![IOT Edge hub'ı modülü modülü iletişimi kolaylaştırır.](./media/iot-edge-runtime/module-endpoints.png)

IOT Edge hub'ına veri göndermek için bir modül SendEventAsync yöntemi çağırır. İletiyi göndermek için hangi çıkış ilk bağımsız değişken belirtir. Aşağıdaki sözde kod bir ileti gönderir **output1**:

   ```csharp
   ModuleClient client = new ModuleClient.CreateFromEnvironmentAsync(transportSettings); 
   await client.OpenAsync(); 
   await client.SendEventAsync(“output1”, message); 
   ```

Bir ileti almak için belirli bir girdi gelen iletileri işleyen bir geri çağırma kaydedin. Alınan tüm iletileri işlemek için kullanılacak işlev messageProcessor aşağıdaki sözde kod kaydeder **input1**:

   ```csharp
   await client.SetInputMessageHandlerAsync(“input1”, messageProcessor, userContext);
   ```

Tercih ettiğiniz SDK dili için API Başvurusu ModuleClient sınıfı ve kendi iletişim yöntemleri hakkında daha fazla bilgi için bkz: [C#](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.moduleclient?view=azure-dotnet), [C ve Python](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/iothub-module-client-h), [Java](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device.moduleclient?view=azure-java-stable), veya [Node.js](https://docs.microsoft.com/javascript/api/azure-iot-device/moduleclient?view=azure-node-latest).

Çözüm Geliştirici nasıl IOT Edge hub'ı modüller arasında iletileri geçirir belirleyen kuralları belirtmek için sorumludur. Yönlendirme kuralları bulutta tanımlanır ve cihaz ikizi, IOT Edge hub'ına gönderilen. IOT hub'ı yolları aynı sözdizimi, Azure IOT edge'deki modüller arasında tanımlamak için kullanılır. Daha fazla bilgi için [modülleri dağıtma ve IOT Edge'de rotaları oluşturmak hakkında bilgi edinin](module-composition.md).   

![IOT Edge hub'ı aracılığıyla modüller arasında Git](./media/iot-edge-runtime/module-endpoints-with-routes.png)

## <a name="iot-edge-agent"></a>IOT Edge Aracısı

IOT Edge, Azure IOT Edge çalışma zamanını oluşturan yapan başka bir modül aracısıdır. Modülleri örnekleme, çalışmaya devam sağlama ve IOT Hub'ına modüllerinin durumunu raporlamaya sorumludur. Yalnızca diğer modüllerin gibi bu yapılandırma verilerini depolamak için kendi modül ikizi IOT Edge Aracısı'nı kullanır. 

[IOT Edge güvenlik arka plan programı](iot-edge-security-manager.md) cihaz başlangıçta IOT Edge Aracısı'nı başlatır. Aracı kendi modül ikizi, IOT Hub'ından alır ve dağıtım bildirimini inceler. Dağıtım bildirimi başlatılması gereken modülleri bildiren bir JSON dosyasıdır. 

Dağıtım bildirimi içinde her öğe bir modülle ilgili belirli bilgileri içerir ve modülün yaşam döngüsü denetlemek için IOT Edge aracısı tarafından kullanılır. Bazı daha ilgi çekici özellikleri şunlardır: 

* **Settings.image** – modülü başlatmak için IOT Edge Aracısı'nı kullanan bir kapsayıcı görüntüsü. Görüntünün bir parolayla korunuyorsa, IOT Edge Aracısı kapsayıcı kayıt defteri kimlik bilgileri ile yapılandırılması gerekir. Uzaktan dağıtım bildirimini kullanarak kapsayıcı kayıt defteri yapılandırılabilir veya IOT Edge cihazında kimlik bilgilerini güncelleştirerek `config.yaml` IOT Edge program klasöründeki dosya.
* **settings.createOptions** – bir modülün kapsayıcısı başlatılırken Moby kapsayıcı daemon doğrudan geçirilen bir dize. İletme veya birimleri bir modülün kapsayıcıya bağlama bağlantı noktası gibi gelişmiş yapılandırmalar için seçenekleri ekleme özelliği sağlar.  
* **Durum** – durumu IOT Edge Aracısı Modülü yerleştirir. Genellikle, bu değeri şuna ayarlı *çalıştıran* çoğu kişi IOT Edge Aracısı tüm modüller cihazda hemen başlatmak için istediğiniz kadar. Ancak, yapının başlangıç durumunun durdurulması ve IOT Edge Aracısı bir modülün başlatmak için bahsetmenin gelecekteki bir zamanı için beklemesi için bir modül belirtebilirsiniz. IOT Edge Aracısı'nı her modül durumunu buluta bildirilen özellikler geri bildirir. İstenen özellik ve bildirilen özellik arasında bir fark davranan bir cihaz bir göstergesidir. Desteklenen durumlar şunlardır:
   * İndiriliyor
   * Çalışıyor
   * İyi durumda değil
   * Başarısız
   * Durduruldu
* **restartPolicy** : nasıl IOT Edge Aracısı bir modülün yeniden başlatır. Olası değerler şunlardır:
   * `never` – IOT Edge Aracısı modülü başlatmaz.
   * `on-failure` -Modül çökerse, IOT Edge Aracısı'nı yeniden başlatır. Modül temiz bir şekilde kapanırsa, IOT Edge Aracısı, yeniden başlatılmaz.
   * `on-unhealthy` -Modül kilitlendiğinde veya eğer iyi durumda olmadığı kabul, IOT Edge Aracısı'nı yeniden başlatır.
   * `always` -Module kilitlenmeler, sağlıksız olduğu kanısına varılır veya herhangi bir şekilde kapanıyorsa öyle kapanır, IOT Edge Aracısı'nı yeniden başlatır. 

IOT Edge çalışma zamanı yanıtı IOT hub'a gönderir. Olası yanıtların listesi aşağıda verilmiştir:
  * 200 - TAMAM
  * 400 - dağıtım yapılandırması bozuk veya geçersiz.
  * 417 - cihaz, bir dağıtım yapılandırma kümesi yok.
  * 412 - dağıtım yapılandırma şeması sürümü geçersiz.
  * 406 - IOT Edge cihaz çevrimdışı veya göndermiyor durum raporları ' dir.
  * 500 - IOT Edge çalışma zamanı'nda hata oluştu.

Daha fazla bilgi için [modülleri dağıtma ve IOT Edge'de rotaları oluşturmak hakkında bilgi edinin](module-composition.md).   

### <a name="security"></a>Güvenlik

IOT Edge Aracısı bir IOT Edge cihazının güvenliği kritik rol oynar. Örneğin, bir modülün görüntüsüne başlatmadan önce doğrulama gibi eylemleri gerçekleştirir. 

Azure IOT Edge güvenlik çerçevesi hakkında daha fazla bilgi için okuyun [IOT Edge Güvenlik Yöneticisi](iot-edge-security-manager.md).

## <a name="next-steps"></a>Sonraki adımlar

[Azure IOT Edge modüllerini anlama](iot-edge-modules.md)
