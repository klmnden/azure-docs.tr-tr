<properties
    pageTitle="IoT Hub cihaz yönetimine başlama | Microsoft Azure"
    description="Cihaz yönetimi için C# ile Azure IoT Hub ile çalışmaya başlama öğreticisi. Cihaz yönetimi uygulamak için Microsoft Azure IoT SDK'ları ile Azure IoT Hub ve C# kullanın."
    services="iot-hub"
    documentationCenter=".net"
    authors="juanjperez"
    manager="timlt"
    editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="dotnet"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="04/29/2016"
 ms.author="juanpere"/>

# Node.js kullanarak Azure IoT Hub cihaz yönetimini kullanmaya başlama (önizleme)

[AZURE.INCLUDE [iot-hub-device-management-get-started-selector](../../includes/iot-hub-device-management-get-started-selector.md)]

## Giriş
Azure IoT Hub cihaz yönetimini kullanmaya başlamak için Azure IoT Hub oluşturmanız, IoT Hub'da cihaz hazırlamanız, birden fazla sanal cihazı başlatmanız ve bu cihazları cihaz yönetimi örnek kullanıcı arabiriminde görüntülemeniz gerekir. Bu öğretici, bu adımlarda size yol gösterir.

> [AZURE.NOTE]  Var olan IoT Hub'larında henüz cihaz yönetimi işlevleri olmadığından, var olan bir IoT Hub'ınız olsa bile cihaz yönetimi işlevlerini etkinleştirmek için yeni bir IoT Hub oluşturmanız gerekir. Cihaz yönetimi genel olarak kullanılabilir olduğunda, var olan tüm IoT Hub'ları cihaz yönetimi işlevlerini edinecek şekilde yükseltilir.

## Ön koşullar

Bu öğretici bir Ubuntu Linux geliştirme makinesi kullandığınızı varsayar.

Adımları tamamlamak için aşağıdaki yazılımların yüklü olması gerekir:

- Git

- gcc (sürüm 4.9 veya üstü). `gcc --version` komutunu kullanarak ortamınıza yüklenmiş geçerli sürümü doğrulayabilirsiniz. Ubuntu 14.04’te gcc sürümünüzü yükseltme hakkında bilgi için bkz. <http://askubuntu.com/questions/466651/how-do-i-use-the-latest-gcc-4-9-on-ubuntu-14-04>.

- [CMake](https://cmake.org/download/) (2.8 veya sonraki bir sürümü). `cmake --version` komutunu kullanarak ortamınıza yüklenmiş geçerli sürümü doğrulayabilirsiniz.

- Node.js 6.1.0 veya üstü.  Platformunuza yönelik Node.js dosyasını <https://nodejs.org/> adresinden yükleyin.

- Etkin bir Azure aboneliği. Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü][lnk-free-trial].

## Cihaz yönetimi etkinleştirilmiş bir IoT Hub oluşturma

Sanal cihazlarınızın bağlanması için cihaz yönetimi etkinleştirilmiş bir IoT Hub oluşturmanız gerekir. Aşağıdaki adımlarda, Azure portalını kullanarak bu görevi nasıl tamamlayacağınız gösterilir.

1.  [Azure portalında] oturum açın.
2.  Harf çubuğunda **Yeni**'ye tıklayın, ardından **Nesnelerin İnterneti**'ne tıklayın ve sonra **Azure IoT Hub**'a tıklayın.

    ![][img-new-hub]

3.  **IoT Hub** dikey penceresinde IoT Hub'ınız için yapılandırmayı seçin.

    ![][img-configure-hub]

  -   **Ad** kutusunda IoT Hub'ınız için bir ad girin. **Ad** geçerli ve kullanılabilir durumdaysa **Ad** kutusunun yanında yeşil bir onay işareti görünür.
  -   Bir **Fiyatlandırma ve ölçek katmanı** seçin. Bu öğretici için belirli bir katman gerekmez.
  -   **Kaynak grubunda** yeni bir kaynak grubu oluşturun veya var olan bir kaynak grubunu seçin. Daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma].
  -   **Cihaz Yönetimini Etkinleştirme** için kutuyu işaretleyin.
  -   **Konum**'da IoT Hub'ınızı barındıracak konumu seçin. IoT Hub cihaz yönetimi, genel önizleme sırasında yalnızca Doğu ABD, Kuzey Avrupa ve Doğu Asya'da kullanılabilir. Gelecekte tüm bölgelerde kullanılabilecek.

  > [AZURE.NOTE]  **Cihaz Yönetimini Etkinleştirme** için kutuyu işaretlemezseniz örnekler çalışmaz.

4.  IoT Hub yapılandırma seçeneklerinizi belirlediğinizde **Oluştur**'a tıklayın. Azure'ın IoT Hub'ınızı oluşturması birkaç dakika sürebilir. Durumu denetlemek için **Başlangıç Panosu** veya **Bildirimler** panelinde ilerlemeyi izleyebilirsiniz.

    ![][img-monitor]

5.  IoT Hub'ı başarılı bir şekilde oluşturduğunuzda, yeni IoT Hub'ın dikey penceresini açın, **Ana bilgisayar adını** not edin ve ardından **Anahtarlar** simgesine tıklayın.

    ![][img-keys]

6.  **iothubowner** ilkesine tıklayın, ardından **iothubowner** dikey penceresindeki bağlantı dizesini kopyalayın ve not edin. Bu öğreticinin geri kalanını tamamlamak için buna ihtiyacınız olacağından, bunu daha sonra erişebileceğiniz bir konuma kopyalayın.

    > [AZURE.NOTE] Üretim senaryolarında **iothubowner** kimlik bilgilerini kullanmadığınızdan emin olun.

    ![][img-connection]

Cihaz yönetimi etkinleştirilmiş bir IoT Hub oluşturdunuz. Bu öğreticinin geri kalanını tamamlamak için bağlantı dizesi gerekir.

## Örnekleri oluşturun ve IoT Hub'ınızda cihazları sağlayın.

Bu bölümde sanal cihazı ve örnekleri oluşturan ve IoT Hub'ınızın cihaz kayıt defterinde yeni bir cihaz kimlikleri kümesi sağlayan bir betik çalıştıracaksınız. Cihaz kayıt defterinde girişi olmayan bir cihaz IoT Hub'a bağlanamaz.

IoT Hub'ınızda örnekleri oluşturmak ve cihazları sağlamak için, aşağıdaki adımları izleyin:

1.  Bir kabuk açın.

2.  GitHub deposunu kopyalayın. **Hiçbir boşluk olmayan bir dizine kopyaladığınızdan emin olun.**

      ```
      git clone --recursive --branch dmpreview https://github.com/Azure/azure-iot-sdks.git
      ```

3.  **azure-iot-sdks** deposunu kopyaladığınız kök klasörden **azure-iot-sdks/c/build_all/linux** dizinine gidin ve aşağıdaki komutu yürüterek önkoşul paketlerini ve bağımlı kitaplıkları yükleyin:

      ```
      ./setup.sh
      ```


4.  **azure-iot-sdks** deposunu kopyaladığınız kök klasörden **azure-iot-sdks/node/service/samples** dizinine gidin ve yer tutucu değerini önceki bölümdeki bağlantı dizenizle değiştirerek aşağıdaki komutu yürütün:

      ```
      ./setup.sh <IoT Hub Connection String>
      ```

Bu betik şunları yapar:

1.  Sanal cihazı oluşturmak için gereken make dosyalarını derlemek üzere **cmake**'i çalıştırır. Yürütülebilir dosya **azure-iot-sdks/node/service/samples/cmake/iotdm\_client/samples/iotdm\_simple\_sample** konumundadır. Kaynak dosyalarının **azure-iot-sdks/c/iotdm\_client/samples/iotdm\_simple\_sample** klasöründe olduğunu unutmayın.

2.  Sanal cihaz yürütülebilir dosyası **iotdm\_simple\_sample**'ı oluşturur.

3.  Gerekli paketleri yüklemek için `npm install` çalıştırır.

4.  IoT Hub'ınızda cihaz kimliklerini sağlamak için `node generate_devices.js` çalıştırır. Cihazlar **sampledevices.json**'da açıklanır. Cihazlar sağlandıktan sonra, kimlik bilgileri **devicecreds.txt** dosyasında (**azure-iot-sdks/node/service/samples** dizininde bulunur) depolanır.

## Sanal cihazlarınızı başlatma

Artık cihazlar cihaz kayıt defterine eklendiğine göre, sanal yönetilen cihazları başlatabilirsiniz. Azure IoT Hub'da sağlanan her bir cihaz kimliği için bir sanal cihazın başlatılması gerekir.

Bir kabuk kullanarak **azure-iot-sdks/node/service/samples** dizinine gidin ve şunu çalıştırın:

  ```
  ./simulate.sh
  ```

Bu betik, **devicecreds.txt** dosyasında listelenen her bir cihaza yönelik **iotdm\_simple\_sample**'ı başlatmak üzere çalışmanız için gereken komutları çıkarır. Lütfen her bir sanal cihaz için ayrı bir terminal penceresinden komutları tek tek çalıştırın. Sanal cihaz, siz komut penceresini kapatana kadar çalışmaya devam eder.

**iotdm\_simple\_sample** uygulaması, Azure IoT Hub tarafından yönetilebilen IoT cihazlarının oluşturulmasını etkinleştiren C için Azure IoT Hub cihaz yönetimi istemci kitaplığı kullanılarak oluşturulur. Cihaz üreticileri, cihaz özelliklerini raporlamak ve cihaz işlerinin gerekli kıldığı yürütme eylemlerini uygulamak için bu kitaplığı kullanabilir. Bu kitaplık, açık kaynaklı Azure IoT Hub SDK'larının parçası olarak teslim edilen bir bileşendir.

**simulate.sh**'yi çalıştırdığınızda, çıktı penceresinde bir veri akışı görürsünüz. Bu çıktı, uygulamaya özgü geri çağırma işlevlerindeki **printf** deyimlerinin yanı sıra gelen ve giden trafiği de gösterir. Böylece örnek uygulamanın kodu çözülmüş paketleri nasıl işlediğinin yanı sıra gelen ve giden trafiği de görmenize olanak sağlanır. Cihaz IoT Hub'a bağlandığında, hizmet cihazdaki kaynakları gözlemlemeye otomatik olarak başlar. Ardından, IoT Hub DM istemci kitaplığı, cihazdan en son değerleri almak için cihaz geri çağırmalarını çalıştırır.

Aşağıda **iotdm\_simple\_sample** örnek uygulamasının çıktısı bulunur. En üstte IoT Hub'a bağlanan **Device11-7ce4a850** kimliğine sahip cihazı gösteren başarılı bir **REGISTERED** iletisi görürsünüz.

> [AZURE.NOTE]  Daha az ayrıntılı çıktı elde etmek için tekil yapılandırmayı oluşturun ve çalıştırın.

![][img-output]

Sonraki bölümlerde yer alan öğreticileri tamamlarken tüm sanal cihazların çalışıyor olduğundan emin olun.

## Cihaz yönetimi örnek kullanıcı arabirimini çalıştırma

IoT Hub hazırladığınıza ve hem çalışan hem de yönetim için hazırlanmış birkaç sanal cihaza sahip olduğunuza göre cihaz yönetimi örnek kullanıcı arabirimini dağıtabilirsiniz. Cihaz yönetimi örnek kullanıcı arabirimi etkileşimli bir kullanıcı arabirimi deneyimi oluşturmak üzere cihaz yönetim API’lerinin nasıl kullanılacağına ilişkin çalışan bir örnek sağlar.  Cihaz yönetimi örnek kullanıcı arabirimi hakkında [bilinen sorunlar](https://github.com/Azure/azure-iot-device-management#knownissues) ile birlikte daha fazla bilgi için [Azure IoT cihaz yönetimi kullanıcı arabirimi][lnk-dm-github] GitHub deposuna bakın.

Cihaz yönetimi örnek kullanıcı arabirimini almak, derlemek ve çalıştırmak için aşağıdaki adımları izleyin:

1. Bir kabuk açın.

2. `node --version` yazarak önkoşullar bölümüne uygun şekilde Node.js 6.1.0 veya üstünü yüklediğinizi onaylayın.

3. Kabukta aşağıdaki komutu çalıştırarak Azure IoT cihaz yönetimi kullanıcı arabirimi GitHub deposunu kopyalayın:

    ```
    git clone https://github.com/Azure/azure-iot-device-management.git
    ```
    
4. Kopyaladığınız Azure IoT cihaz yönetimi kullanıcı arabirimi deposu örneğinin kök klasöründe bağımlı paketler almak için aşağıdaki komutu çalıştırın:

    ```
    npm install
    ```

5. npm yükleme komutu tamamlandığında kabukta aşağıdaki komutu çalıştırarak kodu derleyin:

    ```
    npm run build
    ```

6. Kopyalanan klasörün kökünde config.json dosyasını açmak için bir metin düzenleyicisi kullanın. "&lt;BAĞLANTI DİZENİZ BURAYA&gt;" metnini önceki bölümde verilen IoT Hub bağlantı dizenizle değiştirin ve dosyayı kaydedin.

7. Cihaz yönetimi UX uygulamasını başlatmak için kabukta aşağıdaki komutu çalıştırın:

    ```
    npm run start
    ```

8. Komut istemi "Hizmetler başlatıldı" durumunu bildirdiğinde bir web tarayıcısı açın ve sanal cihazlarınızı görüntülemek için <http://127.0.0.1:3003> URL’sinde bulunan cihaz yönetimi uygulamasına gidin.

    ![][img-dm-ui]

Sonraki cihaz yönetimi öğreticisine geçerken sanal cihazları ve cihaz yönetimi uygulamasını çalışır durumda bırakın.


## Sonraki adımlar

IoT Hub kullanmaya başlamaya devam etmek için bkz. [Ağ Geçidi SDK’sı ile çalışmaya başlama][lnk-gateway-SDK].

Azure IoT Hub cihaz yönetimi özellikleri hakkında daha fazla bilgi almak için [Örnek kullanıcı arabirimi kullanarak Azure IoT Hub cihaz yönetimini keşfetme][lnk-sample-ui] öğreticisine bakın.

<!-- images and links -->
[img-new-hub]: media/iot-hub-device-management-get-started-node/image1.png
[img-configure-hub]: media/iot-hub-device-management-get-started-node/image2.png
[img-monitor]: media/iot-hub-device-management-get-started-node/image3.png
[img-keys]: media/iot-hub-device-management-get-started-node/image4.png
[img-connection]: media/iot-hub-device-management-get-started-node/image5.png
[img-output]: media/iot-hub-device-management-get-started-node/image6.png
[img-dm-ui]: media/iot-hub-device-management-get-started-node/dmui.png

[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[Azure portalında]: https://portal.azure.com/
[Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma]: ../azure-portal/resource-group-portal.md
[lnk-dm-github]: https://github.com/Azure/azure-iot-device-management
[lnk-sample-ui]: iot-hub-device-management-ui-sample.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md


<!--HONumber=Aug16_HO1-->


