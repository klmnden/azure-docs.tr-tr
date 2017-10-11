---
title: "Java ile Azure IOT kenar modülü oluşturma | Microsoft Docs"
description: "Bu öğretici nasıl en son Azure IOT kenar Maven paketlerini kullanma bırak veri dönüştürücü modülü yazılacağını gösterir."
services: iot-hub
author: junyi
manager: timlt
ms.service: iot-hub
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 06/28/2017
ms.author: junyi
ms.openlocfilehash: 0c430272225d79737baec2be15ed7c93991cdeac
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="create-an-azure-iot-edge-module-with-java"></a>Java ile Azure IOT kenar modülü oluşturun

Bu öğretici bir Java Azure IOT sınır için bir modül nasıl oluşturabilir gösterir.

Bu öğreticide, biz ortam kurulumu ve nasıl yazılacağını yükselteceğinizi bir [bırak](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) veri dönüştürücü modülü en son Azure IOT kenar Maven paketleri kullanma.

## <a name="prerequisites"></a>Ön koşullar

Bu bölümde IOT kenar modülü geliştirme ortamınızı ayarlayacaksınız. Her ikisi de geçerli *64-bit Windows* ve *64-bit Linux (Ubuntu/Debian 8)* işletim sistemleri.

Aşağıdaki yazılımlar gereklidir:

* [Git istemci](https://git-scm.com/downloads).
* [**x64** JDK](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
* [Maven](https://maven.apache.org/install.html).

Komut satırı bir terminal penceresi açın ve aşağıdaki depoyu kopyalayın:

1. `git clone https://github.com/Azure-Samples/iot-edge-samples.git`.
2. `cd iot-edge-samples/java/simulated_ble`

## <a name="overall-architecture"></a>Genel mimari

Azure IOT kenar platform yoğun uyarlar [Von Neumann mimarisi](https://en.wikipedia.org/wiki/Von_Neumann_architecture). Hangi tüm Azure IOT kenar mimarisi, işleyen giriş ve çıkışı üretir bir sistem anlamına gelir; ve tek tek her modül küçük bir giriş-çıkış alt olduğunu. Bu öğreticide, biz aşağıdaki iki modüller getirir:

1. Sanal alan bir modül [bırak](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) sinyal ve biçimlendirilmiş dönüştürür [JSON](https://en.wikipedia.org/wiki/JSON) ileti.
2. Alınan yazdırır bir modül [JSON](https://en.wikipedia.org/wiki/JSON) ileti.

Aşağıdaki resimde bu proje için tipik uçtan uca veri akışı görüntüler:

![Veri akışı üç modülleri arasında](media/iot-hub-iot-edge-create-module/dataflow.png "giriş: benzetimli bırak Modülü; İşlemci: Dönüştürücü Modülü; Çıktı: Yazıcı Modülü")

## <a name="understanding-the-code"></a>Kodu anlama

### <a name="maven-project-structure"></a>Maven projesi yapısı

Azure IOT kenar paketleri Maven üzerinde dayandığından, içeren tipik bir Maven projesi yapısı oluşturmak ihtiyacımız bir `pom.xml` dosyası.

POM devraldığı `com.microsoft.azure.gateway.gateway-module-base` tüm çalışma zamanı ikili dosyaları, ağ geçidi yapılandırma dosyası yolu ve yürütme davranışını içeren bir modülü projesi tarafından gerekli bağımlılıkların bildirir paket. Bu, bize çok fazla kaydeder ve yazma ve kod satırlarını yüzlerce tekrar tekrar yeniden yazma gereksinimini ortadan kaldırır.

Biz gerekli bağımlılıkları/eklenti ve aşağıdaki kod parçacığında gösterildiği gibi bizim modülü tarafından kullanılacak yapılandırma dosyasının adını bildirerek pom.xml dosyasını güncelleştirmeniz gerekir.

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <!-- Inherit from parent -->
  <parent>
    <groupId>com.microsoft.azure.gateway</groupId>
    <artifactId>gateway-module-base</artifactId>
    <version>1.0.1</version>
  </parent>
  
  <groupId>com.microsoft.azure.gateway</groupId>
  <artifactId>ble-converter</artifactId>
  <version>1.0</version>
  <packaging>jar</packaging>

  <!-- Set the filename of the Azure IoT Edge configuration located
       under ./src/main/resources/gateway/ which is used in parent -->
  <properties>
    <gw.config.fileName>gw-config.json</gw.config.fileName>
  </properties>

  <!-- Re-declare dependencies used in parent -->
  <dependencies>
    <dependency>
      <groupId>com.microsoft.azure.gateway</groupId>
      <artifactId>gateway-java-binding</artifactId>
    </dependency>
    <dependency>
      <groupId>${dependency.runtime.group}</groupId>
      <artifactId>${dependency.runtime.name}</artifactId>
    </dependency>
  </dependencies>

  <!-- Re-declare plugins used in parent -->
  <build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-dependency-plugin</artifactId>
      </plugin>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-resources-plugin</artifactId>
      </plugin>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-shade-plugin</artifactId>
      </plugin>
      <plugin>
        <groupId>org.codehaus.mojo</groupId>
        <artifactId>exec-maven-plugin</artifactId>
      </plugin>
    </plugins>
  </build>
</project>
```

### <a name="basic-understanding-of-an-azure-iot-edge-module"></a>Bir Azure IOT kenar modülün temel anlama

Bir veri işleri için işlemci olarak Azure IOT kenar modülü davranabilirsiniz: girişi almak, işlemesi ve çıktı üretir.

Giriş (gibi hareket algılayıcısı) donanım verilerden bir ileti diğer modüller ya da (örneğin, düzenli aralıklarla bir Zamanlayıcı tarafından oluşturulan bir rastgele sayı) başka bir şey olabilir.

Giriş benzer bir çıkış, tetikleyebilir donanım davranış (gibi yanıp sönen LED), diğer modüller ya da (örneğin, yazdırma konsoluna) başka bir şey için bir ileti.

Modüller birbirleri ile kullanarak iletişim `com.microsoft.azure.gateway.messaging.Message` sınıfı. **İçerik** , bir `Message` istediğiniz verilerin herhangi bir tür temsil etme işleyebilen bir bayt dizisi. **Özellikler** mevcuttur `Message` ve yalnızca bir dize-dize eşleme. Düşünebilirsiniz **özellikleri** bir HTTP isteğinin ya da meta veri dosyasının başlığı olarak.

Java Azure IOT kenar modülünde geliştirmek için hangi devraldığı yeni bir modül sınıfı oluşturmanız gerekir `com.microsoft.azure.gateway.core.GatewayModule` ve gerekli soyut yöntemleri uygulamak `receive()` ve `destroy()`. Bu noktada, ayrıca isteğe bağlı uygulamayı seçebilir `start()` veya `create()` de yöntemleri. Aşağıdaki kod parçacığını bir Azure IOT kenar modülü yazma başlayacağınızı gösterir.

```java
import com.microsoft.azure.gateway.core.Broker;
import com.microsoft.azure.gateway.core.GatewayModule;
import com.microsoft.azure.gateway.messaging.Message;

public class MyEdgeModule extends GatewayModule {
  public MyEdgeModule(long address, Broker broker, String configuration) {
    /* Let the GatewayModule do the dirty work of initialization. It's also
       a good time to parse your own configuration defined in Azure IoT Edge
       configuration file (typically ./src/main/resources/gateway/gw-config.json) */
    super(address, broker, configuration);
  }

  @Override
  public void start() {
    /* Acquire the resources you need. If you don't
       need any resources, you may omit this method. */
  }

  @Override
  public void destroy() {
    /* It's time to release all resources. This method is required. */
  }

  @Override
  public void receive(Message message) {
    /* Logic to process the input message. This method is required. */
    // ...
    /* Use publish() method to do the output. You are
       allowed to publish your new Message instance. */
    this.publish(message);
  }
}
```

### <a name="converter-module"></a>Dönüştürücü Modülü

| Girdi                    | İşlemci                              | Çıktı                 | Kaynak dosya            |
| ------------------------ | -------------------------------------- | ---------------------- | ---------------------- |
| Sıcaklık veri iletisi | Ayrıştırma ve yeni bir JSON ileti oluşturun | Yapı JSON iletisi | `ConverterModule.java` |

Bu modül tipik bir Azure IOT kenar modüldür. Diğer modüllerden sıcaklık iletileri kabul eder (bir donanım modülü veya bu bizim benzetimli bırak modülü durumda); ve (sıcaklık uyarı tetiklemesi ve benzeri için ihtiyacımız özelliğini ayarlama ileti kimliği ekleme dahil) yapılandırılmış bir JSON iletisi sıcaklık iletisinde normalleştirir.

```java
@Override
public void receive(Message message) {
  try {
    JSONObject messageFromBle = new JSONObject(new String(message.getContent()));
    double temperature = messageFromBle.getDouble("temperature");
    Map<String, String> inputProperties = message.getProperties();

    HashMap<String, String> properties = new HashMap<>();
    properties.put("source", inputProperties.get("source"));
    properties.put("macAddress", inputProperties.get("macAddress"));
    properties.put("temperatureAlert", temperature > 30 ? "true" : "false");

    String content = String.format(
        "{ \"deviceId\": \"Intel NUC Gateway\", \"messageId\": %d, \"temperature\": %f }",
        ++this.messageCount, temperature);

    this.publish(new Message(content.getBytes(), properties));
  } catch (Exception ex) {
    ex.printStackTrace();
  }
}
```

### <a name="printer-module"></a>Yazıcı Modülü

| Girdi                          | İşlemci | Çıktı                     | Kaynak dosya          |
| ------------------------------ | --------- | -------------------------- | -------------------- |
| Diğer modüllerden herhangi bir iletisi | Yok       | İleti konsola oturum | `PrinterModule.java` |

Bu, terminal penceresi alınan iletileri çıkarır basit ve açıklayıcı, bir modüldür.

```java
@Override
public void receive(Message message) {
  System.out.println(message.toString());
}
```

### <a name="azure-iot-edge-configuration"></a>Azure IOT kenar yapılandırma

Modülleri çalıştırmadan önce son adım, Azure IOT kenar yapılandırmak ve modülleri arasında bağlantı kurmak için ' dir.

İlk başvurduğu (itibaren Azure IOT kenar destekler yükleyicilerini farklı dillerde) bizim Java yükleyicisi bildirmek ihtiyacımız kendi `name` bölümlerde daha sonra.

```json
"loaders": [{
  "type": "java",
  "name": "java",
  "configuration": {
    "jvm.options": {
      "library.path": "./"
    }
  }
}]
```

Biz bizim yükleyicilerini bildirdikten sonra biz de bizim modülleri bildirmeniz gerekir. Benzer yükleyicileri bildirmek için bunlar ayrıca tarafından başvurulabilir kendi `name` özniteliği. Bir modül bildirirken (hangi önce tanımladığımız biri olmalıdır) kullanması gereken yükleyicisi belirtmek ihtiyacımız ve-(bizim modül normalleştirilmiş sınıf adı olmalıdır) için giriş noktası her modül. `simulated_device` Azure IOT kenar çekirdek çalışma zamanı paketinde bulunan bir yerel modül modülüdür. Her zaman içermelidir `args` JSON'dosyası, olsa bile `null`.

```json
"modules": [
  {
    "name": "simulated_device",
    "loader": {
      "name": "native",
      "entrypoint": {
        "module.path": "simulated_device"
      }
    },
    "args": {
      "macAddress": "01:02:03:03:02:01",
      "messagePeriod": 500
    }
  },
  {
    "name": "converter",
    "loader": {
      "name": "java",
      "entrypoint": {
        "class.name": "com/microsoft/azure/gateway/ConverterModule",
        "class.path": "./ble-converter-1.0-with-deps.jar"
      }
    },
    "args": null
  },
  {
    "name": "print",
    "loader": {
      "name": "java",
      "entrypoint": {
        "class.name": "com/microsoft/azure/gateway/PrinterModule",
        "class.path": "./ble-converter-1.0-with-deps.jar"
      }
    },
    "args": null
  }
]
```

Yapılandırma sonunda bağlantı kurun. Her bağlantı ile ifade edilen `source` ve `sink`. Önceden tanımlanmış bir modül her ikisi de başvurmalısınız. Çıktı ileti `source` modülü girişi için iletilir `sink` modülü.

```json
"links": [
  {
    "source": "simulated_device",
    "sink": "converter"
  },
  {
    "source": "converter",
    "sink": "print"
  }
]
```

## <a name="running-the-modules"></a>Modülleri çalıştırma

Kullanım `mvn package` içine her şeyi oluşturmak için `target/` klasör. `mvn clean package`Ayrıca temiz bir yapı için tavsiye edilir.

Kullanım `mvn exec:exec` Azure IOT kenar ve çalıştırmak için sıcaklık veri ve tüm özellikler sabit bir oranda konsola yazdırılır gözlemlemek.

Uygulamayı istiyorsanız, basın `<Enter>` anahtarı.

> [!IMPORTANT]
> IOT sınır ağ geçidi sonlandırmak için Ctrl + C kullanmak için önerilmez. Bu, olağan dışı ciddiyeti işlemin neden olabilir ve.

