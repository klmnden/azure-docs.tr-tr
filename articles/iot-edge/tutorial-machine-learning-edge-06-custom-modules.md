---
title: Özel modüller - Machine Learning, Azure IOT Edge üzerinde oluşturup | Microsoft Docs
description: Oluşturun ve machine learning modeli aracılığıyla yaprak cihazlarından veri işlemek ve ardından IOT Hub'ına öngörüleri gönderin IOT Edge modüllerini dağıtmak.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/13/2019
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 571a72d396c003bab363682559464715b180458a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67081395"
---
# <a name="tutorial-create-and-deploy-custom-iot-edge-modules"></a>Öğretici: Oluşturma ve dağıtma özel IOT Edge modülleri

> [!NOTE]
> Bu makale bir serinin IOT Edge üzerinde Azure Machine Learning'i kullanma hakkında bir öğretici için parçasıdır. Bu makalede doğrudan gelmiş, başlangıç öneriyoruz [makaleyi](tutorial-machine-learning-edge-01-intro.md) serisindeki en iyi sonuçlar için.

Bu makalede, yaprak cihazlardan ileti almak, machine learning modeli veri çalıştırın ve ardından IOT Hub'ına ınsights iletin üç IOT Edge modülleri oluşturuyoruz.

IOT Edge hub'ı modül için modülü iletişimi kolaylaştırır. IOT Edge kullanarak hub'a bir ileti aracısı olarak modülleri birbirinden bağımsız tutar. Modüller yalnızca üzerinde iletileri ve bunlar iletileri yazma çıkışları kabul girişleri belirtmeniz gerekir.

Bizim için dört şeyler gerçekleştirmek için IOT Edge cihazı istiyoruz:

* Yaprak cihazlardan veri alma
* Veri gönderen bir cihaz için RUL tahmin edin
* İletiyi yalnızca rul değerini hesaplaması için cihaz ile IOT Hub'ına gönderme (Bu işlev RUL bazı seviyenin altındaysa yalnızca veri gönderecek şekilde değiştirilmesi)
* IOT Edge cihazı üzerindeki yerel bir dosyaya yaprak cihaz verilerini kaydedin. Bu veri dosyası, makine öğrenme modelinin eğitim iyileştirmek için dosya karşıya yükleme IOT Hub'ına düzenli olarak yüklenir. Karşıya dosya yükleme yerine sabit bir ileti akışıyla daha uygun maliyetlidir.

Bu görevleri gerçekleştirmek için üç özel modüller kullanırız:

* **RUL sınıflandırıcı:** Oluşturduğumuz turboFanRulClassifier Modülü [eğitme ve bir Azure Machine Learning modelini dağıtma](tutorial-machine-learning-edge-04-train-model.md) "amlInput" ve "amlOutput" adlı bir çıktı adlı giriş sunan bir standart machine learning modül. "amlInput" gibi tam olarak ACI tabanlı web hizmetine gönderdik girişi aramak için girdi bekliyor. Benzer şekilde, web hizmeti olarak aynı verileri "amlOutput" döndürür.

* **Avro Yazan:** Bu modül, "avroModuleInput" giriş iletileri alır ve daha sonra yüklemek için IOT hub'ı diske Avro biçiminde iletiyi almaya devam.

* **Yönlendirici Modülü:** Yönlendirici modülü aşağı akış yaprak cihazlarıyla iletileri alır, ardından biçimlendirir ve bir sınıflandırıcı iletileri gönderir. Modül bir sınıflandırıcı tanımından iletileri alır ve Avro yazıcı modülü iletiyi iletir. Son olarak, modül RUL tahmin yalnızca IOT Hub'ına gönderir.

  * Girişler:
    * **deviceInput**: yaprak cihazlardan iletileri alır
    * **rulInput:** "amlOutput" iletiler alır

  * Çıkışlar:
    * **Sınıflandırma:** "amlInput" iletileri gönderir
    * **writeAvro:** "avroModuleInput" iletileri gönderir
    * **toIotHub:** iletileri bağlı IOT Hub'ına geçirir. $ Yukarı Akış, iletileri gönderir

Aşağıdaki diyagramda, modüller, girişler, çıkışlar ve IOT Edge hub'ı yollar için tam çözüm gösterilmektedir:

![IOT Edge üç modülleri mimarisi diyagramı](media/tutorial-machine-learning-edge-06-custom-modules/modules-diagram.png)

Bu makaledeki adımlarda, genellikle bir bulut geliştiricisi tarafından gerçekleştirilir.

## <a name="create-a-new-iot-edge-solution"></a>Yeni bir IOT Edge çözümü oluşturun

Bizim iki Azure not defterleri saniye yürütme sırasında oluşturulan ve yayımlanan RUL modelimizi içeren bir kapsayıcı görüntüsü. Parçaları görüntüyü Azure IOT Edge modülü olarak dağıtılabilir hale getirmek için yerleşik görüntü oluşturma işleminin bir parçası olarak Azure Machine Learning. Bu adımda, "Azure Machine Learning" modülü kullanarak bir Azure IOT Edge çözümü oluşturacaksınız ve modül Azure not defterlerini kullanarak yayımlanan resmin üzerine gelin.

1. Geliştirme makinenize Uzak Masaüstü oturumu açın.

2. Klasör Aç **C:\\kaynak\\IoTEdgeAndMlSample** Visual Studio code'da.

3. Explorer panelinde (boşluk) sağ tıklayın ve seçin **yeni IOT Edge çözüm**.

    ![Yeni bir IOT Edge çözümü oluşturun](media/tutorial-machine-learning-edge-06-custom-modules/new-edge-solution-command.png)

4. Varsayılan çözüm adı kabul **EdgeSolution**.

5. Seçin **Azure Machine Learning** modülü şablon olarak.

6. Modül adı **turbofanRulClassifier**.

7. Machine learning çalışma alanı seçin.

8. Azure not defteri çalıştırılırken oluşturulan görüntüyü seçin.

9. Söyleyin; çözüm arayın ve oluşturulan dosyaları dikkat edin:

   * **Deployment.Template.JSON:** Bu dosya, her bir çözüm içindeki modül tanımını içerir. Bu dosyadaki dikkat edilmesi gereken üç bölüm bulunur:

     * **Kayıt defteri kimlik bilgileri:** Çözümünüzde kullanmakta olduğunuz özel kapsayıcı kayıt defterleri kümesini tanımlar. Sağ şimdi onu, machine learning çalışma alanı, kayıt defterinden Azure Machine Learning görüntünüzü saklandığı olduğu içermelidir. Herhangi bir sayıda kapsayıcı kayıt defterleri olabilir, ancak kolaylık olması için bu bir kayıt defteri tüm modüller için kullanacağız

       ```json
       "registryCredentials": {
         "<your registry>": {
           "username": "$CONTAINER_REGISTRY_USERNAME_<your registry>",
           "password": "$CONTAINER_REGISTRY_PASSWORD_<your registry>",
           "address": "<your registry>.azurecr.io”
         }
       }
       ```

     * **Modüller:** Bu bölümde, bu çözüm ile Git kullanıcı tanımlı modülleri kümesini içerir. Bu bölümde şu anda iki modül içerdiğini görürsünüz: tempSensor ve turbofanRulClassifier. TempSensor Visual Studio Code şablon tarafından yüklendi, ancak bu çözüm için ihtiyacımız yok. TempSensor modül tanımı modülleri bölümünden silebilirsiniz. Görüntüyü kapsayıcı kayıt defterinizde turbofanRulClassifier modül tanımı işaret unutmayın. Daha fazla modülleri çözümü eklediğinizde, bu bölümde gösterilir.

       ```json
       "modules": {
         "tempSensor": {
           "version": "1.0",
           "type": "docker",
           "status": "running",
           "restartPolicy": "always",
           "settings": {
             "image": "mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0",
             "createOptions": {}
           }
         },
         "turbofanRulClassifier": {
           "version": "1.0",
           "type": "docker",
           "status": "running",
           "restartPolicy": "always",
           "settings": {
             "image": "<your registry>.azurecr.io/edgemlsample:1",
             "createOptions": {}
           }
         }
       }
       ```

     * **Yollar:** biz rotalarla bir bit Bu öğreticide çalışacaksınız. Modüller birbirleri ile nasıl iletişim kuracağını yolları tanımlayın. Şablon tarafından tanımlanan iki rotalar ihtiyacımız yönlendirme ile eşleşmiyor. İlk rota, IOT Hub'ına (Yukarı Akış$) sınıflandırıcı çıktısından tüm verileri gönderir. Başka bir yolun yalnızca sildik tempSensor için ' dir. İki varsayılan yolları silin.

       ```json
       "$edgeHub": {
         "properties.desired": {
           "schemaVersion": "1.0",
           "routes": {
             "turbofanRulClassifierToIoTHub": "FROM /messages/modules/turbofanRulClassifier/outputs/\* INTO $upstream",
             "sensorToturbofanRulClassifier": "FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\\"/modules/turbofanRulClassifier/inputs/input1\\")"
           },
           "storeAndForwardConfiguration": {
             "timeToLiveSecs": 7200
           }
         }
       }
       ```

   * **Deployment.Debug.Template.JSON:** deployment.template.json hata ayıklama sürümünü bu dosyasıdır. Biz deployment.template.json gelen tüm değişiklikler bu dosyaya yansıması olmalıdır.

   * **.env:** kullanıcı adı ve parola kayıt defterinizin erişmek için burada sağlamanız bu dosyasıdır.

      ```env
      CONTAINER_REGISTRY_USERNAME_<your registry name>=<ACR username>
      CONTAINER_REGISTRY_PASSWORD_<your registry name>=<ACR password>
      ```

10. Visual Studio Code Gezgini'nde deployment.template.json dosyaya sağ tıklayın ve seçin **IOT Edge Çözümü Derle**.

11. Bu komut config klasörü deployment.amd64.json dosyasıyla oluşturduğunu görürsünüz. Bu çözüm için somut bir dağıtım şablonu dosyasıdır.

## <a name="add-router-module"></a>Yönlendirici modül Ekle

Ardından, yönlendirici modülü çözüm ekleriz. Yönlendirici modülü Çözümümüzü için çeşitli sorumlulukları işler:

* **Yaprak cihazlardan iletilerini:** yönlendirici modülü iletileri aşağı akış cihazlardan IOT Edge cihazına geldiğinde, iletiyi alır ve ileti yönlendirme işlemlerini başlar.
* **RUL sınıflandırıcı modülü ileti gönderme:** bir aşağı akış CİHAZDAN yeni bir ileti alındığında, yönlendirici modülü ileti RUL sınıflandırıcı bekliyor biçimine dönüştürür. Yönlendirici RUL sınıflandırıcı RUL tahmin için ileti gönderir. Sınıflandırıcı tahmin yaptıktan sonra yeniden yönlendirici modülü iletiyi gönderir.
* **RUL iletileri IOT Hub'ına gönderme:** yönlendirici bir sınıflandırıcı tanımından ileti aldığında, iletiyi yalnızca önemli bilgileri, cihaz kimliği ve RUL, içerecek şekilde dönüştürür ve IOT hub'ına kısaltılmış iletisi gönderir. Yalnızca RUL tahmin (örneğin 100'den az döngüleri RUL olduğunda) bir eşiğin altına düştüğünde size burada tamamlamadıysanız, bir diğer iyileştirme iletileri IOT hub'a gönderir. Bu şekilde filtreleme iletileri hacmini azaltmak ve IOT hub'ın maliyeti azaltmak.
* **Avro yazıcı modülü ileti gönder:** aşağı akış cihaz tarafından gönderilen tüm verileri korumak için yönlendirici modülü kalıcı hale getirmek ve IOT hub'ı dosyasını kullanarak verileri karşıya Avro yazıcı modülü bir sınıflandırıcı tanımından alınan tüm ileti gönderir karşıya yükleyin.

> [!NOTE]
> Modül sorumlulukları açıklamasını sıralı görünen işleme kalmasına neden olabilir, ancak ileti/olay tabanlı akış olduğu. Bizim yönlendirici modülü gibi bir düzenleme modülü ihtiyacımız nedeni budur.

### <a name="create-module-and-copy-files"></a>Modül ve kopyalama dosyaları oluşturma

1. Visual Studio code'da modüller klasörüne sağ tıklayın ve seçin **IOT Edge Modülü Ekle**.

2. Seçin  **C# Modülü**.

3. Modül adı **turbofanRouter**.

4. Docker görüntüsü deponuz için bir istem göründüğünde, machine learning çalışma alanına kayıt defterini kullanın (kayıt defteri registryCredentials düğümünde bulabilirsiniz, *deployment.template.json* dosyası). Bu değer tam kayıt defterine gibi adresidir  **\<kayıt defterinizin\>.azurecr.io/turbofanrouter**.

    > [!NOTE]
    > Bu makalede, Azure Container Registry eğitmek ve bizim sınıflandırıcı dağıtmak için kullandığımız Azure Machine Learning hizmeti çalışma tarafından oluşturulan kullanırız. Bu, yalnızca kolaylık sağlamaya yöneliktir. Biz, yeni bir kapsayıcı kayıt defteri oluşturmuş olabileceğiniz ve yayımlanan bizim modüller vardır.

5. Visual Studio Code'da yeni bir terminal penceresi açın (**görünümü** > **Terminal**) ve modülleri dizinden dosyaları kopyalayın.

    ```cmd
    copy c:\source\IoTEdgeAndMlSample\EdgeModules\modules\turbofanRouter\*.cs c:\source\IoTEdgeAndMlSample\EdgeSolution\modules\turbofanRouter\
    ```

6. Program.cs üzerine yazmak isteyip istemediğiniz sorulduğunda basın `y` ve ardından isabet `Enter`.

### <a name="build-router-module"></a>Yönlendirici Modülü

1. Visual Studio Code'da seçin **Terminal** > **varsayılan derleme görevi'ni yapılandırma**.

2. Tıklayarak **şablondan tasks.json dosyası oluştur**.

3. Tıklayarak **.NET Core**.

4. Tasks.JSON açıldığında içeriğiyle değiştirin:

    ```json
    {
      // See https://go.microsoft.com/fwlink/?LinkId=733558
      // for the documentation about the tasks.json format
      "version": "2.0.0",
      "tasks": [
        {
          "label": "build",
          "command": "dotnet",
          "type": "shell",
          "group": {
            "kind": "build",
            "isDefault": true
          },
          "args": [
            "build",
            "${workspaceFolder}/modules/turbofanRouter"
          ],
          "presentation": {
            "reveal": "always"
          },
          "problemMatcher": "$msCompile"
        }
      ]
    }
    ```

5. Kaydet ve tasks.json kapatın.

6. Birlikte çalışma derleme `Ctrl + Shift + B` veya **Terminal** > **derleme görevi Çalıştır**.

### <a name="set-up-module-routes"></a>Modül rotalar ayarlayabilir

Yukarıda belirtildiği gibi yapılandırılan yollar IOT Edge çalışma zamanı kullanan *deployment.template.json* modüller arasındaki iletişimi gevşek yönetmek için dosya eşleşmiş. Bu bölümde, biz turbofanRouter modülü için rotalar ayarlama konusunda ayrıntılı. Biz, giriş rotaları ilk kapsar ve çıktılar üzerinde taşıyın.

#### <a name="inputs"></a>Girişler

1. Program.cs dosyasının Init() yöntemi, biz modülü için iki geri aramaları kaydetme:

   ```csharp
   await ioTHubModuleClient.SetInputMessageHandlerAsync(EndpointNames.FromLeafDevice, LeafDeviceInputMessageHandler, ioTHubModuleClient);
   await ioTHubModuleClient.SetInputMessageHandlerAsync(EndpointNames.FromClassifier, ClassifierCallbackMessageHandler, ioTHubModuleClient);
   ```

2. İlk geri gönderilen iletileri dinler **deviceInput** havuz. Yukarıdaki diyagramdan iletileri yönlendirmek için bu giriş için herhangi bir yaprak CİHAZDAN istediğimizi bakın. İçinde *deployment.template.json* turbofanRouter modüldeki bir IOT Edge modülü tarafından "deviceInput" adlı giriş gönderilmedi IOT Edge cihazı tarafından alınan herhangi bir ileti yönlendirmek için edge hub'ı belirten bir rota ekleyin:

   ```json
   "leafMessagesToRouter": "FROM /messages/* WHERE NOT IS_DEFINED($connectionModuleId) INTO BrokeredEndpoint(\"/modules/turbofanRouter/inputs/deviceInput\")"
   ```

3. Sonraki iletiler için bir rota rulClassifier modülünden turbofanRouter modülünü ekleyin:

   ```json
   "classifierToRouter": "FROM /messages/modules/classifier/outputs/amloutput INTO BrokeredEndpoint(\"/modules/turbofanRouter/inputs/rulInput\")"
   ```

#### <a name="outputs"></a>Çıkışlar

Dört ek yolları yönlendirici modül çıkışları işlemek için $edgeHub rota parametresi ekleyin.

1. Program.cs modülü istemci yol kullanılarak RUL sınıflandırıcı ileti göndermek için kullandığı SendMessageToClassifier() yöntemi tanımlar:

   ```json
   "routerToClassifier": "FROM /messages/modules/turbofanRouter/outputs/classOutput INTO BrokeredEndpoint(\"/modules/classifier/inputs/amlInput\")"
   ```

2. SendRulMessageToIotHub() modülü istemci rota aracılığıyla IOT hub'ına cihaz için yalnızca RUL veri göndermek için kullanır:

   ```json
   "routerToIoTHub": "FROM /messages/modules/turboFanRouter/outputs/hubOutput INTO $upstream"
   ```

3. SendMessageToAvroWriter() modülü istemci avroFileWriter modülüne eklenen RUL verilerle ileti göndermek için kullanır.

   ```json
   "routerToAvro": "FROM /messages/modules/turbofanRouter/outputs/avroOutput INTO BrokeredEndpoint(\"/modules/avroFileWriter/inputs/avroModuleInput\")"
   ```

4. HandleBadMessage() nerede bunlar daha sonra kullanmak üzere yönlendirilebilir IOT Hub başarısız yukarı akışla iletiler gönderir.

   ```json
   "deadLetter": "FROM /messages/modules/turboFanRouter/outputs/deadMessages INTO $upstream"
   ```

Birlikte, "$edgeHub" geçen tüm yollar ile düğümü aşağıdaki JSON gibi görünmelidir:

```json
"$edgeHub": {
  "properties.desired": {
    "schemaVersion": "1.0",
    "routes": {
      "leafMessagesToRouter": "FROM /messages/* WHERE NOT IS_DEFINED($connectionModuleId) INTO BrokeredEndpoint(\"/modules/turbofanRouter/inputs/deviceInput\")",
      "classifierToRouter": "FROM /messages/modules/turbofanRulClassifier/outputs/amlOutput INTO BrokeredEndpoint(\"/modules/turbofanRouter/inputs/rulInput\")",
      "routerToClassifier": "FROM /messages/modules/turbofanRouter/outputs/classOutput INTO BrokeredEndpoint(\"/modules/turbofanRulClassifier/inputs/amlInput\")",
      "routerToIoTHub": "FROM /messages/modules/turboFanRouter/outputs/hubOutput INTO $upstream",
      "routerToAvro": "FROM /messages/modules/turbofanRouter/outputs/avroOutput INTO BrokeredEndpoint(\"/modules/avroFileWriter/inputs/avroModuleInput\")",
      "deadLetter": "FROM /messages/modules/turboFanRouter/outputs/deadMessages INTO $upstream"
    },
    "storeAndForwardConfiguration": {
      "timeToLiveSecs": 7200
    }
  }
}
```

> [!NOTE]
> Aşağıdaki ek yol oluşturulan turbofanRouter modülü ekleme: `turbofanRouterToIoTHub": "FROM /messages/modules/turbofanRouter/outputs/* INTO $upstream`. Bu rota kaldırmak için yalnızca yolları bırakarak deployment.template.json dosyanızda yukarıdaki.

#### <a name="copy-routes-to-deploymentdebugtemplatejson"></a>Deployment.debug.template.json kopyalama yolları

Son adım olarak, bizim dosyaları eşitlenmiş şekilde tutmanızı sağlayacak deployment.template.json deployment.debug.template.json içinde yapılan değişiklikleri yansıtır.

## <a name="add-avro-writer-module"></a>Avro yazıcı Modülü Ekle

Avro yazıcı modülü iletileri depolamak ve dosyaları karşıya yüklemek için sunduğumuz çözümde iki sorumlulukları vardır.

* **Store iletileri**: Avro yazıcı modülü ileti aldığında, iletiyi Avro biçimi yerel dosya sisteminde yazar. Bir dizini (büyük/küçük harf bu /data/avrofiles) bir modülün kapsayıcı yoluna bağlar bağlama bağlama kullanırız. Bu bağlama için yerel bir yol yazmak modül sağlar (/ avrofiles) ve bu dosyaların doğrudan IOT Edge CİHAZDAN erişilebilir.

* **Dosyaları karşıya yükleme**: bir Azure depolama hesabına dosya yüklemek için Azure IOT hub'ı karşıya dosya yükleme özelliğiyle Avro yazıcı modülü kullanır. Dosya başarıyla karşıya yüklendikten sonra modülü dosyayı diskten siler.

### <a name="create-module-and-copy-files"></a>Modül ve kopyalama dosyaları oluşturma

1. Komut paletini içinde arayın ve ardından seçin **Python: Yorumlayıcıyı seçme**.

1. C: bulunan yorumlayıcı seçin\\Python37.

1. Yeniden komut paletini açın ve arayın ve ardından seçin **Terminal: Varsayılan kabuğunu seçin**.

1. İstendiğinde, **komut istemi**.

1. Yeni bir terminal kabuğunu açın **Terminal** > **yeni Terminal**.

1. Visual Studio code'da modüller klasörüne sağ tıklayın ve seçin **IOT Edge Modülü Ekle**.

1. **Python Modülü**'nü seçin.

1. ' % S'modülü "avroFileWriter" olarak adlandırın.

1. Docker görüntüsü deponuz için bir istem göründüğünde, aynı kayıt defteri, yönlendirici modülü eklerken kullanılan kullanın.

1. Dosyaları örnek modülünden çözümün içine kopyalayın.

   ```cmd
   copy C:\source\IoTEdgeAndMlSample\EdgeModules\modules\avroFileWriter\*.py C:\source\IoTEdgeAndMlSample\EdgeSolution\modules\avroFileWriter\
   ```

1. Main.PY üzerine istenirse türü `y` ve ardından isabet `Enter`.

1. Filemanager.PY ve schema.py çözüme eklenmiş ve güncelleştirilmiş main.py dikkat edin.

> [!NOTE]
> Bir Python dosyası açtığınızda pylint yüklemeniz istenebilir. Bu öğreticiyi tamamlamak için lint yüklemeniz gerekmez.

### <a name="bind-mount-for-data-files"></a>Veri dosyaları için bağlama bağlama

Giriş belirtildiği gibi cihazın dosya sistemine Avro dosyalarını yazmak için bağlama bağlama varlığını yazıcı modülü kullanır.

#### <a name="add-directory-to-device"></a>Cihaz için Dizin Ekle

1. IOT Edge cihazınıza VM bağlanmak SSH kullanarak.

   ```bash
   ssh -l <user>@IoTEdge-<extension>.<region>.cloudapp.azure.com
   ```

2. Kaydedilen yaprak cihaz iletileri tutar dizini oluşturun.

   ```bash
   sudo mkdir -p /data/avrofiles
   ```

3. Kapsayıcı tarafından yazılabilir yapmak için dizin izinlerini güncelleştirin.

   ```bash
   sudo chmod ugo+rw /data/avrofiles
   ```

4. Dizini doğrulama artık kullanıcı, Grup ve sahibi için yazma (w) izni vardır.

   ```bash
   ls -la /data
   ```

   ![Dizin izinlerini avrofiles](media/tutorial-machine-learning-edge-06-custom-modules/avrofiles-directory-permissions.png)

#### <a name="add-directory-to-the-module"></a>Dizin Modülü Ekle

Modülün kapsayıcıya dizine eklemek için avroFileWriter modülle ilişkili dockerfile'ları değiştirir. Üç dockerfile'ları modülle ilişkili vardır: Dockerfile.AMD64 Dockerfile.amd64.debug ve Dockerfile.arm32v7. Hata ayıklama veya arm32 cihazına dağıtmak istediğimiz durumunda, bu dosyaları eşitlenmiş durumda tutulmalıdır. Bu makalede, yalnızca Dockerfile.amd64 üzerinde odaklanın.

1. Geliştirme makinenizde açın **Dockerfile.amd64** dosya.

2. Dosya, aşağıdaki örneğe benzer görünür olacak şekilde değiştirin:

   ```dockerfile
   FROM ubuntu:xenial  

   WORKDIR /app

   RUN apt-get update && apt-get install -y --no-install-recommends libcurl4-openssl-dev
   python3-pip libboost-python1.58-dev libpython3-dev && rm -rf /var/lib/apt/lists/*

   RUN pip3 install --upgrade pip
   COPY requirements.txt ./
   RUN pip install -r requirements.txt

   COPY . .

   RUN useradd -ms /bin/bash moduleuser
   RUN mkdir /avrofiles && chown moduleuser /avrofiles
   USER moduleuser

   CMD [ "python3", "-u", "./main.py" ]
   ```

   `mkdir` Ve `chown` komutları /avrofiles görüntüde adlı en üst düzey bir dizin oluşturmak için Docker derleme işlemi isteyin ve ardından moduleuser bu dizine sahibi olmak için. Modülü kullanıcı ile görüntüsüne eklendikten sonra bu komutlar eklenen önemlidir `useradd` komutunu ve bağlam anahtarlara moduleuser (kullanıcı moduleuser) önce.

3. Dockerfile.amd64.debug ve Dockerfile.arm32v7 karşılık gelen değişiklikleri yapın.

#### <a name="update-the-module-configuration"></a>Modül yapılandırması güncelleştirme

Bağlama oluşturmanın son deployment.template.json (ve deployment.debug.template.json) dosyaları ile bağlama bilgileri güncelleştirmek için adımdır.

1. Deployment.Template.JSON açın.

2. Modül tanımı avroFileWriter ekleyerek değiştirin `Binds` kapsayıcı dizini /avrofiles sınır cihazı yerel dizine işaret eden bir parametre. Modül tanımınızı şu örnekle eşleşmelidir:

   ```json
   "avroFileWriter": {
     "version": "1.0",
     "type": "docker",
     "status": "running",
     "restartPolicy": "always",
     "settings": {
       "image": "${MODULES.avroFileWriter}",
       "createOptions": {
         "HostConfig": {
           "Binds": [
             "/data/avrofiles:/avrofiles"
           ]
         }
       }
     }
   }
   ```

3. Deployment.debug.template.json için karşılık gelen değişiklikleri yapın.

### <a name="bind-mount-for-access-to-configyaml"></a>Erişim için bağlama için config.yaml bağlama

Yazıcı modülü için bir daha fazla bağlama eklemek ihtiyacımız var. Bu bağlama modülü IOT Edge cihazı /etc/iotedge/config.yaml dosyasından bağlantı dizesini okuma erişimi sağlar. Bir sı: Iothubclient oluşturun, böylece karşıya yükleme diyoruz için bağlantı dizesi ihtiyacımız\_blob\_IOT hub'ına dosyaları karşıya yüklemek için zaman uyumsuz yöntem. Bu bağlama eklemek için adımları, önceki bölümde olanlara benzerdir.

#### <a name="update-directory-permission"></a>Dizin izni güncelleştir

1. IOT Edge cihazınıza SSH kullanarak bağlanın.

   ```bash
   ssh -l <user>@IoTEdge-<extension>.<region>.cloudapp.azure.com
   ```

2. Okuma izni config.yaml dosyaya ekleyin.

   ```bash
   sudo chmod +r /etc/iotedge/config.yaml
   ```

3. Doğrulama izinlerin doğru ayarlandığını.

   ```bash
   ls -la /etc/iotedge/
   ```

4. Config.yaml izinleri olduğundan emin olun **- r--r--r--** .

#### <a name="add-directory-to-module"></a>Modül için Dizin Ekle

1. Geliştirme makinenizde açın **Dockerfile.amd64** dosya.

2. Ek Ekle `mkdir` ve `chown` dosyaya kadar diğer bir deyişle gibi görünüyor komutları:

   ```dockerfile
   FROM ubuntu:xenial

   WORKDIR /app

   RUN apt-get update && apt-get install -y --no-install-recommends libcurl4-openssl-dev
   python3-pip libboost-python1.58-dev libpython3-dev && rm -rf /var/lib/apt/lists/\*

   RUN pip3 install --upgrade pip
   COPY requirements.txt ./
   RUN pip install -r requirements.txt

   COPY . .

   RUN useradd -ms /bin/bash moduleuser
   RUN mkdir /avrofiles && chown moduleuser /avrofiles
   RUN mkdir -p /app/iotconfig && chown moduleuser /app/iotconfig

   USER moduleuser

   CMD "python3", "-u", "./main.py"]
   ```

3. Dockerfile.amd64.debug ve Dockerfile.arm32v7 karşılık gelen değişiklikleri yapın.

#### <a name="update-the-module-configuration"></a>Modül yapılandırması güncelleştirme

1. Açık **deployment.template.json** dosya.

2. İkinci bir satıra ekleyerek avroFileWriter modül tanımını değiştirme `Binds` (/ etc/iotedge) cihazda yerel dizin kapsayıcı dizini (/ uygulama/iotconfig) işaret eden bir parametre.

   ```json
   "avroFileWriter": {
     "version": "1.0",
     "type": "docker",
     "status": "running",
     "restartPolicy": "always",
     "settings": {
       "image": "${MODULES.avroFileWriter}",
       "createOptions": {
         "HostConfig": {
           "Binds": [
             "/data/avrofiles:/avrofiles",
             "/etc/iotedge:/app/iotconfig"
           ]
         }
       }
     }
   }
   ```

3. Deployment.debug.template.json için karşılık gelen değişiklikleri yapın.

## <a name="install-dependencies"></a>Bağımlılıkları yükleme

Yazıcı modülü bir bağımlılık iki Python kitaplıkları, fastavro ve PyYAML alır. Biz geliştirme makinemizi bağımlılıkları yükler ve Docker yapı işlemi bunları bizim modülün görüntüsüne yükleyip istemeniz gerekir.

### <a name="pyyaml"></a>PyYAML

1. Geliştirme makinenizde açın **requirements.txt** dosya ve pyyaml ekleyin.

   ```txt
   azure-iothub-device-client~=1.4.3
   pyyaml
   ```

2. Açık **Dockerfile.amd64** dosya ve ekleme bir `pip install` setuptools yükseltmek komutu.

   ```dockerfile
   FROM ubuntu:xenial

   WORKDIR /app

   RUN apt-get update && \
       apt-get install -y --no-install-recommends libcurl4-openssl-dev python3-pip libboost-python1.58-dev libpython3-dev && \
       rm -rf /var/lib/apt/lists/\*

   RUN pip3 install --upgrade pip
   RUN pip install -U pip setuptools
   COPY requirements.txt ./
   RUN pip install -r requirements.txt

   COPY . .

   RUN useradd -ms /bin/bash moduleuser
   RUN mkdir /avrofiles && chown moduleuser /avrofiles
   RUN mkdir -p /app/iotconfig && chown moduleuser /app/iotconfig
   USER moduleuser

   CMD [ "python3", "-u", "./main.py" ]
   ```

3. Dockerfile.amd64.debug için karşılık gelen değişiklikleri yapın. <!--may not be necessary. Add 'if needed'?-->

4. Visual Studio Code'da bir terminal açıp yazarak pyyaml yerel olarak yükle

   ```cmd
   pip install pyyaml
   ```

### <a name="fastavro"></a>Fastavro

1. Requirements.txt içinde fastavro pyyaml sonra ekleyin.

   ```txt
   azure-iothub-device-client~=1.4.3
   pyyaml
   fastavro
   ```

2. Fastavro terminal Visual Studio Code kullanarak, geliştirme makinenize yükleyin.

   ```cmd
   pip install fastavro
   ```

## <a name="reconfigure-iot-hub"></a>IOT hub'ı yeniden yapılandırın

IOT Edge cihazı ve modülleri sisteme sunarak hangi veriler gönderilir, hub'ına ve hangi amaçla ilgili bizim beklentileri değiştirdik. Hub'ında yeni bizim gerçeklik ile dağıtılacak yönlendirme yeniden yapılandırmanız gerekir.

> [!NOTE]
> Biz hub bazı hub ayarlar, özellikle dosya karşıya yükleme için düzgün çalışması için avroFileWriter modülü için doğru ayarlanmış olması gerekir, modülleri dağıtmadan önce yeniden yapılandırın

### <a name="set-up-route-for-rul-messages-in-iot-hub"></a>IOT hub'ında RUL iletileri için rota ayarlama

Yönlendirici ve yerinde sınıflandırıcı yalnızca cihaz kimliği ve cihaz için RUL tahmin içeren normal iletileri almak isteriz. Burada cihazların durumunu izlemek, raporlar ve uyarılar gerektiğinde harekete kendi depolama konumuna RUL verileri yönlendirmek istiyoruz. Aynı zamanda, doğrudan henüz geçerli depolama konuma yönlendirmek devam etmek için sunduğumuz IOT Edge cihazına eklenmemiş bir yaprak cihaz tarafından hala gönderilen herhangi bir cihaza veri istiyoruz.

#### <a name="create-a-rul-message-route"></a>RUL ileti yönlendirme oluşturma

1. Azure portalında, IOT Hub'ınıza gidin.

2. Sol gezinti bölmesinden seçin **ileti yönlendirme**.

3. **Add (Ekle)** seçeneğini belirleyin.

4. Rota adı **RulMessageRoute**.

5. Seçin **Ekle** yanındaki **uç nokta** Seçici ve **Blob Depolama**.

6. İçinde **depolama uç noktası ekleme** formunda, uç nokta adı **ruldata**.

7. Seçin **bir kapsayıcı seçin**.

8. Gibi adlı Bu öğretici boyunca kullanılan depolama hesabı seçin **iotedgeandml\<benzersiz soneki\>** .

9. Seçin **ruldata** kapsayıcı ve tıklatın **seçin**.

10. Tıklayın **Oluştur** depolama uç noktayı oluşturun.

11. İçin **yönlendirme sorgu**, aşağıdaki sorguyu girin:

    ```sql
    IS_DEFINED($body.PredictedRul) AND NOT IS_DEFINED($body.OperationalSetting1)
    ```

12. Genişletin **Test** bölümüne ve ardından **ileti gövdesi** bölümü. İleti, beklenen iletilerle Bu örnekle değiştirin:

    ```json
    {
      "ConnectionDeviceId": "aaLeafDevice_1",
      "CorrelationId": "b27e97bb-06c5-4553-a064-e9ad59c0fdd3",
      "PredictedRul": 132.62721409309165,
      "CycleTime": 64.0
    }
    ```

13. Seçin **Test rota**. Test başarılı olursa, "ileti sorguyu eşleşmedi." konusuna bakın

14. **Kaydet**’e tıklayın.

#### <a name="update-turbofandevicetostorage-route"></a>Güncelleştirme turbofanDeviceToStorage yol

Biz istemediğiniz yeni tahmin veri bizim eski depolama konumuna yönlendirme, bu nedenle, önlemek için rota güncelleştirme.

1. IOT hub'ında **ileti yönlendirme** sayfasında **yollar** sekmesi.

2. Seçin **turbofanDeviceDataToStorage**, veya örneğin adı, ilk cihaz verileri yönlendirmek için verdiğiniz.

3. Yönlendirme sorgusu güncelleştir

   ```sql
   IS_DEFINED($body.OperationalSetting1)
   ```

4. Genişletin **Test** bölümüne ve ardından **ileti gövdesi** bölümü. İleti, beklenen iletilerle Bu örnekle değiştirin:

   ```json
   {
     "Sensor13": 2387.96,
     "OperationalSetting1": -0.0008,
     "Sensor6": 21.61,
     "Sensor11": 47.2,
     "Sensor9": 9061.45,
     "Sensor4": 1397.86,
     "Sensor14": 8140.39,
     "Sensor18": 2388.0,
     "Sensor12": 522.87,
     "Sensor2": 642.42,
     "Sensor17": 391.0,
     "OperationalSetting3": 100.0,
     "Sensor1": 518.67,
     "OperationalSetting2": 0.0002,
     "Sensor20": 39.03,
     "DeviceId": 19.0,
     "Sensor5": 14.62,
     "PredictedRul": 212.00132402791962,
     "Sensor8": 2388.01,
     "Sensor16": 0.03,
     "CycleTime": 42.0,
     "Sensor21": 23.3188,
     "Sensor15": 8.3773,
     "Sensor3": 1580.09,
     "Sensor10": 1.3,
     "Sensor7": 554.57,
     "Sensor19": 100.0
   }
   ```

5. Seçin **Test rota**. Test başarılı olursa, "ileti sorguyu eşleşmedi." konusuna bakın

6. **Kaydet**’i seçin.

### <a name="configure-file-upload"></a>Karşıya dosya yüklemeyi yapılandırma

Dosya depolama alanına yüklemek dosya yazıcı modülü etkinleştirmek için IOT hub'ı dosya karşıya yükleme özelliğini yapılandırın.

1. IOT hub'ınızdaki sol Gezgin seçin **karşıya dosya yükleme**.

2. Seçin **Azure depolama kapsayıcısı**.

3. Listeden depolama hesabınızı seçin.

4. Seçin **uploadturbofanfiles** kapsayıcı ve tıklatın **seçin**.

5. **Kaydet**’i seçin. Kayıt tamamlandığında portal size bildirir.

> [!Note]
> Biz Bu öğretici için karşıya yükleme bildirimini kapatma değildir, ancak bkz [dosya karşıya yükleme bildirim alma](../iot-hub/iot-hub-java-java-file-upload.md#receive-a-file-upload-notification) bildirim dosyasını işlemek birleştiremiyorsa yükleme.

## <a name="build-publish-and-deploy-modules"></a>Oluşturun, yayımlayın ve modülleri dağıtma

Yapılandırma değişikliklerini yaptık, görüntülerinizi oluşturmak ve bunları müşterilerimizin Azure kapsayıcı kayıt defterine yayımlamak hazırız. Yapı işlemi deployment.template.json dosyası modüllerine oluşturulması gerektiğini belirlemek için kullanır. Ayarlar sürümü dahil olmak üzere her bir modül için modülü klasörünü module.json dosyasında bulunur. Dockerfile'ları üzerinde ilk çalıştıran bir Docker derleme module.json dosyasında bulunan görüntü oluşturmak için geçerli yapılandırma ile eşleşen derleme işlemi. Ardından bu görüntüyü kayıt defterine module.json dosyasından module.json dosyasındaki bir eşleşen bir sürüm etiketi ile yayımlar. Son olarak, IOT Edge cihazına dağıtacağız yapılandırmaya özgü dağıtım bildirimini (örneğin, deployment.amd64.json) üretir. IOT Edge cihazı dağıtım bildirimi ve temel bilgileri yönergelerini modülleri indirir, yapılandırmak ve istediğiniz özellikleri ayarlayın okur. Bu dağıtım yöntemi, farkında olmanız gereken iki yan etkilere sahiptir:

* **Dağıtım gecikme:** yeniden başlatılmadan önce IOT Edge çalışma zamanı istenen özelliklerini değişikliği tanıması gerekir olduğundan, miktar kadar çalışma zamanı tarafından toplanır ve IOT Edge güncelleştirme başlar modüllerinizi dağıttıktan sonra zaman alabilir cihaz.

* **Modül sürümlerini konular:** bir modülün kapsayıcı yeni bir sürümü önceki modül olarak aynı sürüm etiketleri kullanarak kapsayıcı kayıt defterinizin yayımlarsanız Çalışma Zamanı Modülü yeni sürümünü karşıdan yüklemez. Bunu, Dağıtım bildiriminden sürüm etiketi yerel görüntüyü ve istediğiniz görüntüyü bir karşılaştırmasını yapar. Bu sürümlerden eşleşirse, çalışma zamanı herhangi bir eylemi alır. Bu nedenle, yeni değişiklikler dağıtmak istediğiniz her zaman modülünüzün sürümün artırmak önemlidir. Sürüm değiştirerek Artır **sürüm** özelliği altında **etiketi** değiştiriyorsanız, modül module.json dosyasındaki özellik. Ardından derleme ve modül yayımlayın.

    ```json
    {
      "$schema-version": "0.0.1",
      "description": "",
      "image": {
        "repository": "<your registry>.azurecr.io/avrofilewriter",
        "tag": {
          "version": "0.0.1",
          "platforms": {
            "amd64": "./Dockerfile.amd64",
            "amd64.debug": "./Dockerfile.amd64.debug",
            "arm32v7": "./Dockerfile.arm32v7"
          }
        },
        "buildOptions": []
      },
      "language": "python"
    }
    ```

### <a name="build-and-publish"></a>Derleme ve yayımlama

1. VM geliştirme Visual Studio Code'da bir terminal penceresi Visual Studio Code ve kapsayıcı kayıt defterinizde oturum açın.

   ```cmd
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```

1. Visual Studio code'da deployment.template.json üzerinde sağ tıklatın ve seçin **derleme ve anında iletme IOT Edge çözüm**.

### <a name="view-modules-in-the-registry"></a>Kayıt defterinde modülleri görüntüleme

Yapılandırma başarıyla tamamlandıktan sonra biz bizim yayımlanan modülleri gözden geçirmek için Azure portalını kullanmanız mümkün olacaktır.

1. Azure portalında, Azure Machine Learning hizmeti çalışma alanınıza gidin ve için köprüyü tıklatarak **kayıt defteri**.

    ![Makine öğrenme hizmeti çalışma alanında kayıt defterinize gidin](media/tutorial-machine-learning-edge-06-custom-modules/follow-registry-link.png)

2. Kayıt defteri yan Gezgin seçin **depoları**.

3. Her iki modülü de oluşturduğunuz, Not **avrofilewriter** ve **turbofanrouter**, depoları olarak görünür.

4. Seçin **turbofanrouter** 0.0.1-amd64 etiketlenmiş bir görüntü yayımladığınız not edin.

   ![İlk etiketli versiyonunu turbofanrouter görüntüleyin](media/tutorial-machine-learning-edge-06-custom-modules/tagged-image-turbofanrouter-repo.png)

### <a name="deploy-modules-to-iot-edge-device"></a>Modüller IOT Edge cihazına dağıtma

Biz yerleşik ve modülleri IOT Edge cihazına dağıtacağız artık modülleri çözümümüz, yapılandırılmış.

1. Visual Studio Code'da sağ tıklayın **deployment.amd64.json** config klasöründeki dosya.

2. Seçin **tek cihaz için dağıtım oluşturma**.

3. IOT Edge Cihazınızı seçin **aaTurboFanEdgeDevice**.

4. Visual Studio kod Gezgini Azure IOT Hub cihazları panelinde yenileyin. Üç yeni modüller ancak değil ancak çalışan dağıtıldığını görmeniz gerekir.

5. Birkaç dakika sonra tekrar yenileyin ve modülleri görürsünüz.

   ![Visual Studio Code'da çalışan modüller görünümü](media/tutorial-machine-learning-edge-06-custom-modules/view-running-modules-list.png)

> [!NOTE]
> Bu, başlangıç ve kapatma sürekli çalışır duruma geçirmek modüller için birkaç dakika sürebilir. Bu süre boyunca, IOT Edge hub'ı modülü ile bağlantı kurmaya çalışırken başlatıp modülleri görebilirsiniz.

## <a name="diagnosing-failures"></a>Hataları tanılama

Bu bölümde, biz ne bir modül veya modülleri ile sorun oluştu anlamaya yönelik bazı teknikleri paylaşın. Genellikle bir hata ilk durum Visual Studio code'da gelen anlaþýlmaz.

### <a name="identify-failed-modules"></a>Başarısız modülleri belirle

* **Visual Studio kodu:** Azure IOT Hub cihazları masasında arayın. Modüllerinin çoğu çalışır durumda olan ancak bir durduruldu, durduruldu modülün daha fazla araştırmak gerekir. Tüm modüller, durdurulmuş bir durumda, uzun bir süre için varsa, hata de gösterebilir.

* **Azure portalı:** Portalı'nda IOT hub'ınıza giderek ve ardından cihaz ayrıntıları sayfasına bulma (altında IOT Edge, cihazınızın içine ayrıntıya) bir modülü bir hata bildirdi veya hiçbir şey IOT hub'ına bildirdi bulabilirsiniz.

### <a name="diagnosing-from-the-device"></a>CİHAZDAN tanılama

IOT Edge cihazı açtıktan tarafından modüllerinizi durumuyla ilgili bilgileri daha iyi bir miktarda erişim elde. Cihaz görüntülerinde ve kapsayıcıları incelemek bize Docker komutlarını kullandığımız ana mekanizma var.

1. Tüm çalışan kapsayıcıları listesi. Modülü karşılık gelen bir ada sahip her modül için bir kapsayıcı görmek bekliyoruz. Ayrıca, bu komut, beklentisi ile eşleşebilir. Bu nedenle sürüm dahil olmak üzere kapsayıcı tam görüntüyü listeler. Ayrıca, komut "container" için "Görüntü" getirilmesiyle görüntüleri listeleyebilirsiniz.

   ```bash
   sudo docker container ls
   ```

2. Bir kapsayıcı için günlükleri alın. Bu komut, ne olursa olsun, StdErr ve kapsayıcıdaki StdOut yazılmış çıkarır. Bu komut, çalışmaya ve ardından herhangi bir nedenden dolayı sonlanmış kapsayıcılar için çalışır. EdgeAgent veya edgeHub kapsayıcılarla olduğunu anlamak için kullanışlıdır.

   ```bash
   sudo docker container logs <container name>
   ```

3. Bir kapsayıcı inceleyin. Bu komut bir sürü görüntü ile ilgili bilgi sağlar. Ne aradığınıza bağlı olarak, veriler filtrelenebilir. Örnek olarak, bağlamalar avroFileWriter üzerinde doğru olup olmadığını görmek istiyorsanız komutunun kullanabilirsiniz:

   ```bash
   sudo docker container inspect -f "{{ json .Mounts }}" avroFileWriter | python -m json.tool
   ```

4. Bir çalışan kapsayıcıya bağlanın. Bu komutu çalıştırırken kapsayıcıyı incelemek isterseniz yararlı olabilir:

   ```bash
   sudo docker exec -it avroFileWriter bash
   ```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir IOT Edge çözümü üç modülleri ile Visual Studio code'da, bir sınıflandırıcı, yönlendirici ve bir dosya yazıcı/yükleyici oluşturduk. Biz edge cihazında birbirleri ile iletişim kurmak için modülleri izin vermek için rotalar ayarlayabilir, sınır cihazı yapılandırmasını değiştirilebilir ve bağımlılıkları yükleme ve bağlama takar modülleri kapsayıcıları için eklemek için dockerfile'ları güncelleştirildi. Ardından, yapılandırma türüne göre bizim iletileri yönlendirmek için ve karşıya dosya yükleme işlemleri işlemek için IOT Hub'ın güncelleştirdik. Her yerde modülleri IOT Edge cihazına dağıtılan ve modülleri düzgün çalışmasını güvence altına.

Aşağıdaki sayfalarda daha fazla bilgi bulunabilir:

* [IOT Edge'de yollar oluşturmak ve modülleri dağıtma hakkında bilgi edinin](module-composition.md)
* [IOT Hub ileti yönlendirme sorgusu söz dizimi](../iot-hub/iot-hub-devguide-routing-query-syntax.md)
* [IOT Hub ileti yönlendirme: artık ileti gövdesinde yönlendirme ile](https://azure.microsoft.com/blog/iot-hub-message-routing-now-with-routing-on-message-body/)
* [IoT Hub ile dosyaları karşıya yükleme](../iot-hub/iot-hub-devguide-file-upload.md)
* [Cihazınızı IOT Hub ile buluta dosyaları karşıya yükleme](../iot-hub/iot-hub-python-python-file-upload.md)

Veri göndermeye başlamak ve çözümünüzü iş başında görmek için sonraki makaleye geçin.

> [!div class="nextstepaction"]
> [Saydam bir ağ geçidi üzerinden veri gönderme](tutorial-machine-learning-edge-07-send-data-to-hub.md)
