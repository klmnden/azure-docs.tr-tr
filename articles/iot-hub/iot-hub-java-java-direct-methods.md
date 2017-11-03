---
title: "Azure IOT hub'ı doğrudan yöntemleri (Java) kullanın | Microsoft Docs"
description: "Azure IOT hub'ı doğrudan yöntemlerinin nasıl kullanılacağını. Doğrudan bir yöntem ve Azure IOT hizmeti doğrudan yöntemini çağıran bir hizmet uygulaması uygulamak için Java SDK'sını içeren bir sanal cihaz uygulamasının uygulamak için Azure IOT cihaz SDK'sı Java için kullanın."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: ab035b8e-bff8-4e12-9536-f31d6b6fe425
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: 4fb759ecd7767c126bc22165494652039ba1caa4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-direct-methods-java"></a>Doğrudan yöntemleri (Java) kullanın

[!INCLUDE [iot-hub-selector-c2d-methods](../../includes/iot-hub-selector-c2d-methods.md)]

Bu öğreticide, iki Java konsol uygulamaları oluşturun:

* **Invoke-doğrudan-method**, sanal cihaz uygulamasının bir yöntemi çağırır ve yanıt görüntüleyen bir Java arka uç uygulaması.
* **simulated-device**, oluşturduğunuz cihaz kimliğiyle IOT hub'ınıza bağlanan bir cihaza benzetim bir Java uygulaması. Bu uygulama arka uçtan çağrılan doğrudan yanıt verir.

> [!NOTE]
> Aygıtları ve çözüm arka ucunuz çalıştırmak için uygulamalar oluşturmak için kullanabileceğiniz SDK'ları hakkında daha fazla bilgi için bkz: [Azure IOT SDK'ları][lnk-hub-sdks].

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Java SE 8. <br/> [Geliştirme ortamınızı hazırlama][lnk-dev-setup], bu öğretici için Java'nın Windows veya Linux'ta nasıl yükleneceğini açıklar.
* Maven 3.  <br/> [Geliştirme ortamınızı hazırlama][lnk-dev-setup], bu öğretici için [Maven][lnk-maven]'ın Windows veya Linux'ta nasıl yükleneceğini açıklar.
* [Node.js sürümünü 0.10.0 veya üzeri](http://nodejs.org).

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a>Sanal cihaz uygulaması oluşturma

Bu bölümde, çözüm tarafından arka uç olarak adlandırılan bir yönteme yanıt bir Java konsol uygulaması oluşturun.

1. İot-java-doğrudan-method adlı boş bir klasör oluşturun.

1. İot-java-doğrudan-method klasöründe adlı bir Maven projesi oluşturun **simulated-device** , komut isteminde aşağıdaki komutu kullanarak. Aşağıdaki komut tek ve uzun bir komut şöyledir:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. Komut isteminizde simulated-device klasörüne gidin.

1. Bir metin düzenleyicisi kullanarak simulated-device klasöründeki pom.xml dosyasını açın ve aşağıdaki bağımlılıkları **bağımlılıklar** düğümüne ekleyin. Bu bağımlılık, IOT hub ile iletişim kurmak için uygulamanızda IOT cihaz istemci paketini kullanmanıza olanak sağlar:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.3.32</version>
    </dependency>
    ```

    > [!NOTE]
    > [Maven arama][lnk-maven-device-search] kullanarak en yeni **iot-device-client** sürümünü kontrol edebilirsiniz.

1. Aşağıdakileri ekleyin **yapı** düğümünden sonraki **bağımlılıkları** düğümü. Bu yapılandırma, uygulamanızı oluşturmak için Java 1.8 kullanmak için Maven bildirir:

    ```xml
    <build>
      <plugins>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.3</version>
          <configuration>
            <source>1.8</source>
            <target>1.8</target>
          </configuration>
        </plugin>
      </plugins>
    </build>
    ```

1. pom.xml dosyasını kaydedin ve kapatın.

1. Bir metin düzenleyicisi kullanarak simulated-device\src\main\java\com\mycompany\app\App.java dosyasını açın.

1. Aşağıdaki **içeri aktarma** deyimlerini dosyaya ekleyin:

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.*;

    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.Scanner;
    ```

1. Aşağıdaki sınıf düzeyi değişkenleri **App** sınıfına ekleyin. Değiştirme `{youriothubname}` , IOT hub'ı adıyla ve `{yourdevicekey}` içinde oluşturulan cihaz anahtarla değeri *bir cihaz kimliği oluşturma* bölümü:

    ```java
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myDeviceID;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String deviceId = "myDeviceId";
    private static final int METHOD_SUCCESS = 200;
    private static final int METHOD_NOT_DEFINED = 404;
    ```

    Bu örnek uygulama, bir **DeviceClient** nesnesi örneğini oluşturduğunda **prokotol** değişkenini kullanır. 

1. Bir durum kodu IOT hub'ınıza döndürmek için aşağıdaki iç içe geçmiş sınıf ekleme **uygulama** sınıfı:

    ```java
    protected static class DirectMethodStatusCallback implements IotHubEventCallback
    {
      public void execute(IotHubStatusCode status, Object context)
      {
        System.out.println("IoT Hub responded to device method operation with status " + status.name());
      }
    }
    ```

1. Çözüm arka uçtan doğrudan yöntem çağrılarını işlemek için aşağıdaki iç içe geçmiş sınıf ekleme **uygulama** sınıfı:

    ```java
    protected static class DirectMethodCallback implements com.microsoft.azure.sdk.iot.device.DeviceTwin.DeviceMethodCallback
    {
      @Override
      public DeviceMethodData call(String methodName, Object methodData, Object context)
      {
        DeviceMethodData deviceMethodData;
        switch (methodName)
        {
          case "writeLine" :
          {
            int status = METHOD_SUCCESS;
            System.out.println(new String((byte[])methodData));
            deviceMethodData = new DeviceMethodData(status, "Executed direct method " + methodName);
            break;
          }
          default:
          {
            int status = METHOD_NOT_DEFINED;
            deviceMethodData = new DeviceMethodData(status, "Not defined direct method " + methodName);
          }
        }
        return deviceMethodData;
      }
    }
    ```

1. Oluşturmak için bir **DeviceClient** ve için doğrudan yöntem çağrılarına dinleme, eklemeniz bir **ana** yönteme **uygulama** sınıfı:

    ```java
    public static void main(String[] args)
      throws IOException, URISyntaxException
    {
      System.out.println("Starting device sample...");

      DeviceClient client = new DeviceClient(connString, protocol);

      try
      {
        client.open();
        client.subscribeToDeviceMethod(new DirectMethodCallback(), null, new DirectMethodStatusCallback(), null);
        System.out.println("Subscribed to direct methods. Waiting...");
      }
      catch (Exception e)
      {
        System.out.println("On exception, shutting down \n" + " Cause: " + e.getCause() + " \n" +  e.getMessage());
        client.close();
        System.out.println("Shutting down...");
      }

      System.out.println("Press any key to exit...");
      Scanner scanner = new Scanner(System.in);
      scanner.nextLine();
      scanner.close();
      client.close();
      System.out.println("Shutting down...");
    }
    ```

1. Simulated-device\src\main\java\com\mycompany\app\App.java dosyasını kaydedin ve kapatın

1. Yapı **simulated-device** uygulama ve olan hataları düzeltin. Komut isteminde, simulated-device klasörüne gidin ve aşağıdaki komutu çalıştırın:

    `mvn clean package -DskipTests`

## <a name="call-a-direct-method-on-a-device"></a>Bir cihazda doğrudan bir yöntem çağrısı

Bu bölümde, doğrudan bir yöntemi çağırır ve yanıt görüntüleyen bir Java konsol uygulaması oluşturun. Bu konsol uygulamasını IOT doğrudan yöntemini çağırmak için Hub'ınıza bağlanır.

1. İot-java-doğrudan-method klasöründe adlı bir Maven projesi oluşturun **Invoke-doğrudan-method** , komut isteminde aşağıdaki komutu kullanarak. Aşağıdaki komut tek ve uzun bir komut şöyledir:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=invoke-direct-method -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. Komut isteminizde Invoke doğrudan yöntemi klasöre gidin.

1. Bir metin düzenleyicisi kullanarak Invoke doğrudan yöntemi klasöründeki pom.xml dosyasını açın ve aşağıdaki bağımlılığı eklemek **bağımlılıkları** düğümü. Bu bağımlılık, IOT hub ile iletişim kurmak için uygulamanızda IOT-service-client paketini kullanmanıza olanak sağlar:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
      <type>jar</type>
    </dependency>
    ```

    > [!NOTE]
    > [Maven arama][lnk-maven-service-search] kullanarak en yeni **iot-service-client** sürümünü kontrol edebilirsiniz.

1. Aşağıdakileri ekleyin **yapı** düğümünden sonraki **bağımlılıkları** düğümü. Bu yapılandırma, uygulamanızı oluşturmak için Java 1.8 kullanmak için Maven bildirir:

    ```xml
    <build>
      <plugins>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.3</version>
          <configuration>
            <source>1.8</source>
            <target>1.8</target>
          </configuration>
        </plugin>
      </plugins>
    </build>
    ```

1. pom.xml dosyasını kaydedin ve kapatın.

1. Bir metin düzenleyicisi kullanarak invoke-direct-method\src\main\java\com\mycompany\app\App.java dosyasını açın.

1. Aşağıdaki **içeri aktarma** deyimlerini dosyaya ekleyin:

    ```java
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceMethod;
    import com.microsoft.azure.sdk.iot.service.devicetwin.MethodResult;
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;

    import java.io.IOException;
    import java.util.concurrent.TimeUnit;
    ```

1. Aşağıdaki sınıf düzeyi değişkenleri **App** sınıfına ekleyin. Değiştir `{youriothubconnectionstring}` , not ettiğiniz, IOT hub bağlantı dizesine sahip *IOT Hub oluşturma* bölümü:

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    public static final String methodName = "writeLine";
    public static final Long responseTimeout = TimeUnit.SECONDS.toSeconds(30);
    public static final Long connectTimeout = TimeUnit.SECONDS.toSeconds(5);
    public static final String payload = "a line to be written";
    ```

1. Sanal cihaz için yöntemini çağırmak için aşağıdaki kodu ekleyin **ana** yöntemi:

    ```java
    System.out.println("Starting sample...");
    DeviceMethod methodClient = DeviceMethod.createFromConnectionString(iotHubConnectionString);

    try
    {
        System.out.println("Invoke direct method");
        MethodResult result = methodClient.invoke(deviceId, methodName, responseTimeout, connectTimeout, payload);

        if(result == null)
        {
            throw new IOException("Direct method invoke returns null");
        }
        System.out.println("Status=" + result.getStatus());
        System.out.println("Payload=" + result.getPayload());
    }
    catch (IotHubException e)
    {
        System.out.println(e.getMessage());
    }

    System.out.println("Shutting down sample...");
    ```

1. İnvoke-direct-method\src\main\java\com\mycompany\app\App.java dosyasını kaydedin ve kapatın

1. Yapı **Invoke-doğrudan-method** uygulama ve olan hataları düzeltin. Komut isteminde, Invoke doğrudan yöntemi klasöre gidin ve aşağıdaki komutu çalıştırın:

    `mvn clean package -DskipTests`

## <a name="run-the-apps"></a>Uygulamaları çalıştırma

Şimdi konsol uygulamaları çalıştırmaya hazırsınız.

1. Simulated-device klasöründeki bir komut isteminde IOT hub'ınızı gelen yöntem çağrıları için dinleme başlamak için aşağıdaki komutu çalıştırın:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Java IOT hub'ı sanal cihaz uygulamasının doğrudan yöntem çağrılarını dinlemek üzere][8]

1. Invoke doğrudan yöntemi klasöründeki bir komut isteminde IOT hub'ından sanal Cihazınızda bir yöntemi çağırmak için aşağıdaki komutu çalıştırın:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Doğrudan bir yöntemi çağırmak için Java IOT Hub hizmeti uygulaması][7]

1. Sanal cihaz doğrudan yöntem çağrısı yanıt verir:

    ![Java IOT hub'ı sanal cihaz uygulamasının doğrudan yöntem çağrısı yanıt verir.][9]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure portalında yeni bir IoT hub'ı yapılandırdınız ve ardından IoT hub'ının kimlik kayıt defterinde bir cihaz kimliği oluşturdunuz. Bu cihaz kimliğini bulut tarafından çağrılan yöntemler tepki vermek sanal cihaz uygulamasının sağlamak için kullanılır. Ayrıca aygıtta yöntemleri çağırır ve aygıttan yanıt görüntüleyen bir uygulama oluşturduğunuz.

Diğer IOT senaryolarını keşfetmeye bkz [zamanlama işlerini birden çok aygıta][lnk-devguide-jobs].

Çözüm ve zamanlama yöntemini çağıran birden fazla cihazda, IOT genişletmek öğrenmek için bkz: [zamanlama ve yayın işleri] [ lnk-tutorial-jobs] Öğreticisi.

<!-- Images. -->
[7]: ./media/iot-hub-java-java-direct-methods/invoke-method.png
[8]: ./media/iot-hub-java-java-direct-methods/device-listen.png
[9]: ./media/iot-hub-java-java-direct-methods/device-respond.png

<!-- Links -->
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md
[lnk-maven]: https://maven.apache.org/what-is-maven.html
[lnk-hub-sdks]: iot-hub-devguide-sdks.md

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-maven-service-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22
[lnk-maven-device-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22
