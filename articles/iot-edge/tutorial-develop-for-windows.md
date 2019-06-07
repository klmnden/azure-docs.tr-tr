---
title: Windows cihazlarda - Azure IOT Edge modülü geliştirme | Microsoft Docs
description: Bu öğretici, Windows cihazları için Windows kapsayıcıları kullanarak IOT Edge modülleri geliştirmek için geliştirme makine ve bulut kaynaklarını ayarlama yoluyla açıklar
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 04/20/2019
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 81d660857eff63e0dfeeda400b168ea424152081
ms.sourcegitcommit: f9448a4d87226362a02b14d88290ad6b1aea9d82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66808610"
---
# <a name="tutorial-develop-iot-edge-modules-for-windows-devices"></a>Öğretici: Windows cihazları için IoT Edge modülleri geliştirme

Geliştirip IOT Edge çalıştıran Windows cihazlar için kod dağıtmak için Visual Studio'yu kullanın.

Hızlı Başlangıç, bir Windows sanal makine kullanarak bir IOT Edge cihazı oluşturdunuz ve Azure Market'ten önceden oluşturulmuş bir modül dağıttınız. Bu öğreticide, geliştirme ve IOT Edge cihazına kendi kodunuzu dağıtmak için neler aracılığıyla açıklanmaktadır. Bu öğreticide, belirli programlama dilleri ya da Azure hizmetleri hakkında daha fazla ayrıntıya gider tüm diğer öğreticiler, kullanışlı bir önkoşuldur. 

Bu öğreticide örnek dağıtma bir **C modülü bir Windows cihazına**. Böylece, doğru kitaplıkları yüklü olduğu konusunda endişelenmenize gerek kalmadan geliştirme araçları hakkında bilgi edinebilirsiniz Bu örnekte, kolaylık olması için seçilmiştir. Ardından geliştirme kavramları anladığınızda, tercih ettiğiniz dil veya ayrıntılara inin yönelik Azure hizmeti seçin. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Geliştirme makinenizde ayarlayın.
> * Yeni bir proje oluşturmak için Visual Studio için IOT Edge araçları kullanın.
> * Projenizi bir kapsayıcı olarak ve bir Azure container Registry'de depolayın.
> * Kodunuzu IOT Edge cihazına dağıtma. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="key-concepts"></a>Önemli kavramlar

Bu öğretici, IOT Edge modülü geliştirmeden açıklar. Bir *IOT Edge Modülü*, veya bazı durumlarda yalnızca *Modülü* kısa için yürütülebilir kod içeren bir kapsayıcıdır. Bir veya daha fazla modül bir IOT Edge cihazına dağıtabilirsiniz. Modüller, veri temizleme işlemleri ya da bir IOT hub'ına ileti göndermek ya da veri analizi, sensörlerden alınan verileri almak gibi belirli görevleri gerçekleştirin. Daha fazla bilgi için [anlamak Azure IOT Edge modülleri](iot-edge-modules.md).

IOT Edge modülleri geliştirirken, bir geliştirme makineniz ve IOT Edge cihazı modülü sonunda dağıtılacağı hedef arasındaki farkı anlamak önemlidir. İşletim sistemi (OS) modülü kodunuzu saklamak için yapı kapsayıcı eşleşmelidir *hedef cihaz*. Windows kapsayıcı geliştirme için Windows kapsayıcıları yalnızca Windows işletim sistemlerinde çalışan çünkü bu kavramı basittir. Ancak, örneğin, Windows geliştirme makinenizde Linux IOT Edge cihazları için modüller oluşturmak için kullanabilirsiniz. Bu senaryoda, geliştirme makinenize Linux kapsayıcıları çalıştığı emin olmanız gerekir. Bu öğreticide ilerlerken, arasındaki farkı akılda tutulması *geliştirme makine işletim sistemi* ve *kapsayıcı işletim sistemi*.

Bu öğretici, IOT Edge çalıştıran Windows cihazlara hedefler. Windows IOT Edge cihazları, Windows kapsayıcıları kullanın. Windows cihazlar için bu öğreticiyi kullanın, bu nedenle geliştirmek için Visual Studio kullanmanızı öneririz. Destek iki aracı arasındaki farklılıkları olsa da Visual Studio Code kullanabilirsiniz.

Aşağıdaki tabloda desteklenen geliştirme senaryoları için **Windows kapsayıcıları** Visual Studio Code ve Visual Studio.

|   | Visual Studio Code | Visual Studio 2017/2019 |
| - | ------------------ | ------------------ |
| **Azure Hizmetleri** | Azure İşlevleri <br> Azure Stream Analytics |   |
| **Diller** | C#(desteklenen hata ayıklamaya değil) | C <br> C# |
| **Daha fazla bilgi** | [Visual Studio Code için Azure IOT Edge](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) | [Azure IOT Edge için Visual Studio 2017 Araçları](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vsiotedgetools), [Azure IOT Edge için Visual Studio 2019 araçları](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vs16iotedgetools) |

Bu öğretici için Visual Studio 2019 geliştirme adımlarını öğretir. Bunun yerine Visual Studio Code kullanmayı tercih ediyorsanız, yönergeleri başvurmak [kullanan Visual Studio geliştirme ve modülleri, Azure IOT Edge için hata ayıklama için kod](how-to-vs-code-develop-module.md). Visual Studio 2017 (sürüm 15.7 veya üzeri) kullanıyorsanız, plrease yükleyip [Visual Studio 2017 için Azure IOT Edge Araçları](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vsiotedgetools).

## <a name="prerequisites"></a>Önkoşullar

Bir geliştirme makinesi:

* Windows 10 1809 güncelleştirme ile ya da daha yeni.
* Kendi bilgisayarınıza veya sanal makine, geliştirme tercihlerinize bağlı olarak kullanabilirsiniz.
* [Git](https://git-scm.com/)'i yükleyin. 
* Vcpkg aracılığıyla x64 Windows için Azure IOT C SDK'sını yükleyin:

   ```powershell
   git clone https://github.com/Microsoft/vcpkg
   cd vcpkg
   .\bootstrap-vcpkg.bat
   .\vcpkg install azure-iot-sdk-c:x64-windows
   .\vcpkg --triplet x64-windows integrate install
   ```

<!--vcpkg only required for C development-->

Bir Windows Azure IOT Edge cihazında:

* IOT Edge geliştirme makinenizde çalışmaz, ancak ayrı bir cihaz kullanmanız önerilir. Daha fazla geliştirme makineniz ve IOT Edge cihazı arasındaki bu fark doğru bir şekilde doğru dağıtım senaryosu yansıtır ve farklı kavramları düz tutmaya yardımcı olur.
* Kullanılabilir ikinci bir cihazınız yoksa, Azure ile IOT Edge cihazı oluşturmak için hızlı başlangıç makalesi kullanmak bir [Windows sanal makine](quickstart.md).

Bulut kaynakları:

* Ücretsiz veya standart katman [IOT hub'ı](../iot-hub/iot-hub-create-through-portal.md) azure'da. 

## <a name="install-container-engine"></a>Kapsayıcı Altyapısı'nı

IOT Edge modülleri, kapsayıcı altyapısı oluşturmak ve kapsayıcıları yönetmek için geliştirme makinenizde gereken şekilde kapsayıcıları olarak paketlenir. Çok sayıda özellik ve popülerliğini kapsayıcısını altyapısı olarak nedeniyle, Docker masaüstü geliştirme için kullanmanızı öneririz. Bir Windows bilgisayarda Docker masaüstü ile Windows kapsayıcılarını ve Linux kapsayıcılar arasında kolayca IOT Edge cihazları farklı türleri için modülleri geliştirebilmeniz geçiş yapabilirsiniz. 

Geliştirme makinenizde yüklemek için Docker belgeleri kullanın: 

* [Windows için Docker Masaüstü yükleme](https://docs.docker.com/docker-for-windows/install/)

  * İçin Docker Masaüstü Windows yüklediğinizde, Linux veya Windows kapsayıcıları kullanmak isteyip istemediğinizi sorulur. Bu öğretici için kullanmak **Windows kapsayıcıları**. Daha fazla bilgi için [Windows ve Linux kapsayıcılar arasında geçiş](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers).


## <a name="set-up-visual-studio-and-tools"></a>Visual Studio ve araçları ayarlama

IOT Edge modülleri geliştirmek için Visual Studio 2019 IOT uzantıları kullanın. Bu uzantıları proje şablonları sağlar, dağıtım bildirimini oluşturulmasını otomatikleştirin ve izleyin ve IOT Edge cihazları yönetmenize olanak sağlar. Bu bölümde, Visual Studio'yu ve IOT Edge uzantısını yükleme ardından IOT Hub dahilindeki Visual Studio yönetmek için Azure hesabınızı ayarlayın. 

1. Geliştirme makinenizde Visual Studio yoksa [yükleme Visual Studio 2019](https://docs.microsoft.com/visualstudio/install/install-visual-studio) aşağıdaki iş yükleri ile: 

   * Azure geliştirme
   * C++ ile masaüstü geliştirme
   * .NET core platformlar arası geliştirme

1. Visual Studio 2019 geliştirme makinenizde zaten varsa. Bağlantısındaki [değiştirme Visual Studio](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) zaten yoksa, gerekli iş yüklerini eklemek için.

2. İndirme ve yükleme [Azure IOT Edge araçlarını](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vs16iotedgetools) uzantısı için Visual Studio 2019. 

3. Tesislerinize tamamlandığında Visual Studio 2019 açıp seçin **kod olmadan devam**.

4. Seçin **görünümü** > **Cloud Explorer**. 

5. Cloud explorer'ın profili simgesini seçin ve zaten oturum açmadıysanız Azure hesabınızda oturum açın. 

6. Oturum açtıktan sonra Azure abonelikleri listelenir. Cloud explorer ile erişme ve ardından istediğiniz abonelikleri seçin **Uygula**. 

7. Aboneliğiniz, ardından genişletin **IOT hub'ları**, ardından IOT hub. IOT cihazlarınızı listesini görmeniz gerekir ve bunları yönetmek için bu Gezgini'ni kullanabilirsiniz. 

   ![Cloud Explorer erişim IOT hub'ı kaynakları](./media/tutorial-develop-for-windows/cloud-explorer-view-hub.png)

[!INCLUDE [iot-edge-create-container-registry](../../includes/iot-edge-create-container-registry.md)]

## <a name="create-a-new-module-project"></a>Yeni Modül projesi oluşturma

Azure IOT Edge araçları uzantısı proje şablonları için desteklenen tüm IOT Edge modülü dilleri Visual Studio'da sağlar. Bu şablonları tüm dosyaları ve IOT Edge test etmek için bir çalışma modül dağıtmak için ihtiyacınız olan kod veya kendi iş mantığına sahip şablonu özelleştirmek için bir başlangıç noktası sağlar. 

1. Seçin **dosya** > **yeni** > **proje...**

2. Yeni Proje penceresinde, 2. Yeni Proje penceresinde, arama **IOT Edge** projesini ve ardından **Azure IOT Edge (Windows amd64)** proje. **İleri**’ye tıklayın. 

   ![Yeni Azure IOT Edge projesi oluşturma](./media/tutorial-develop-for-windows/new-project.png)

3. Yapılandırma, yeni proje penceresini benzer bir şey açıklayıcı projeyi ve çözümü yeniden adlandır **CTutorialApp**. Tıklayın **Oluştur** projeyi oluşturmak için.

   ![Yeni bir Azure IOT Edge proje yapılandırma](./media/tutorial-develop-for-windows/configure-project.png)
 

4. IOT Edge uygulama ve modül penceresinde, projenize aşağıdaki değerleri yapılandırın: 

   | Alan | Değer |
   | ----- | ----- |

   | Bir şablon seçin | Seçin **C Modülü**. | | Modül proje adı | Varsayılan değerleri kabul **IoTEdgeModule1**. | | Docker görüntü deposuna | Görüntü deposu, kapsayıcı kayıt defterinizin adını ve kapsayıcı görüntünüzü adını içerir. Kapsayıcı görüntünüzü modülü proje adı değerini doldurulur. **localhost:5000** yerine Azure kapsayıcı kayıt defterinizden alacağınız oturum açma sunucusu değerini yazın. Oturum açma sunucusunu Azure portalda kapsayıcı kayıt defterinizin Genel bakış sayfasından alabilirsiniz. <br><br> Son görüntü deposuna benzer \<kayıt defteri adı\>.azurecr.io/iotedgemodule1. |

   ![Hedef cihaz, modül türü ve kapsayıcı kayıt defteri için projenizi yapılandırın](./media/tutorial-develop-for-windows/add-application-and-module.png)

5. Seçin **Tamam** yaptığınız değişiklikleri uygulamak için. 

Yeni projeniz Visual Studio penceresinde yüklendikten sonra oluşturduğu dosyaları tanımak için bir dakikanızı ayırın: 

* IOT Edge projesinde adlı **AzureIoTEdgeApp1.Windows.Amd64**.
    * **Modülleri** klasörü projeye dahil modülleri işaretçileri içerir. Bu durumda, yalnızca IoTEdgeModule1 olması gerekir. 
    * **Deployment.template.json** bir dağıtım bildirimi oluşturmanıza yardımcı olması için bir şablon dosyasıdır. A *dağıtım bildirimi* tam olarak bir cihaza dağıtılan istediğiniz modülleri, nasıl yapılandırılacağı ve birbirleriyle ve bulut ile nasıl kurabilmek tanımlayan bir dosyadır. 
* Bir IOT Edge modülü proje **IoTEdgeModule1**.
    * **Main.c** dosya proje şablonu ile birlikte gelen varsayılan C modülü kodunu içerir. Varsayılan modülü bir kaynaktan girdi olarak alır ve boyunca IOT Hub'ına geçirir. 
    * **Module.json** tam görüntü deposu dahil olmak üzere, modül dosyası tutma ayrıntılarını görüntü sürümü ve her biri için kullanılacak hangi Dockerfile desteklenen platform.

### <a name="provide-your-registry-credentials-to-the-iot-edge-agent"></a>IOT Edge Aracısı ile kayıt defteri kimlik bilgilerinizi sağlayın

IOT Edge çalışma zamanı, IOT Edge cihaza, kapsayıcı görüntüleri çekmek için kayıt defteri kimlik bilgileriniz gerekir. Bu kimlik bilgileri için dağıtım şablonu ekleyin. 

1. Açık **deployment.template.json** dosya.

2. Bulma **registryCredentials** $edgeAgent özelliğinde istenen özellikleri. 

3. Özelliği bu biçim izleyen, kimlik bilgileriyle güncelleştirin: 

   ```json
   "registryCredentials": {
     "<registry name>": {
       "username": "<username>",
       "password": "<password>",
       "address": "<registry name>.azurecr.io"
     }
   }
   ```

4. Deployment.template.json dosyayı kaydedin. 

### <a name="review-the-sample-code"></a>Örnek kodu gözden geçirin

Oluşturduğunuz çözüm şablonu, bir IOT Edge modülü için örnek kod içerir. Bu örnek modülü yalnızca ileti alır ve bunları geçirdiği sonra. İşlem hattı işlevleri modüller birbirleri ile nasıl iletişim kuracağını olan IOT Edge, önemli bir kavramdır gösterir.

Her modülü birden çok olabilir *giriş* ve *çıkış* kuyrukları bildirilen kodları. IOT Edge hub'ı kullanarak cihaz üzerinde çalışan bir modülün çıkışına iletilerden modüllerinin bir veya daha fazla giriş yönlendirir. Giriş ve çıkışları bildirmek için belirli bir dil diller arasında farklılık gösterir ancak tüm modüller arasında kavramı aynıdır. Modüller arasında yönlendirme hakkında daha fazla bilgi için bkz. [bildirmek yollar](module-composition.md#declare-routes).

1. İçinde **main.c** dosya, bulma **SetupCallbacksForModule** işlevi.

2. Bu işlev, gelen iletileri almak için bir giriş sırasını ayarlar. C SDK'sı modülü istemci işlevi çağırdığı [SetInputMessageCallback](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/iothub-module-client-ll-h/iothubmoduleclient-ll-setinputmessagecallback). Bu işlev gözden geçirin ve adlı bir giriş sırası başlatır bkz **input1**. 

   ![SetInputMessageCallback oluşturucuda giriş adını bulma](./media/tutorial-develop-for-windows/declare-input-queue.png)

3. Ardından, bulma **InputQueue1Callback** işlevi.

4. Bu işlev, alınan iletileri işler ve bunları birlikte geçirmek için bir çıkış sırasını ayarlar. C SDK'sı modülü istemci işlevi çağırdığı [SendEventToOutputAsync](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/iothub-module-client-ll-h/iothubmoduleclient-ll-sendeventtooutputasync). Bu işlev gözden geçirin ve adlı bir çıkış kuyruğuna başlatır bkz **output1**. 

   ![Çıkış adı SendEventToOutputAsync oluşturucuda Bul](./media/tutorial-develop-for-windows/declare-output-queue.png)

5. Açık **deployment.template.json** dosya.

6. Bulma **modülleri** $edgeAgent özelliğini istenen özellikleri. 

   Burada listelenen iki modül olması gerekir. İlk **tempSensor**, hangi tüm şablonlarda modüllerinizi test etmek için kullanabileceğiniz sanal sıcaklık verilerini sağlamak için varsayılan olarak eklenir. İkinci **IotEdgeModule1** bu projenin bir parçası olarak oluşturulan modülü.

   Bu modüller özelliği, cihaz veya cihazlara dağıtım modüllerine eklenmelidir bildirir. 

7. Bulma **yollar** $edgeHub özelliğini istenen özellikleri. 

   IOT Edge hub'ı modülü bir dağıtımdaki tüm modüller arasında iletileri yönlendirmek için ise işlevlerden biri. Yollar özellik değerleri gözden geçirin. İlk yol **IotEdgeModule1ToIoTHub**, bir joker karakter kullanan ( **\*** ) IoTEdgeModule1 modüldeki herhangi bir çıkış kuyruğuna gelen iletiler eklenecek. Bu iletiler kısımlarda *Yukarı Akış $* , IOT hub'ı gösteren ayrılmış bir ad olduğu. İkinci rota **sensorToIotEdgeModule1**tempSensor modülünden gelen iletileri alır ve bunları yönlendirir *input1* IotEdgeModule1 modülünün giriş sırası. 

   ![Yollar deployment.template.json gözden geçirin](./media/tutorial-develop-for-windows/deployment-routes.png)


## <a name="build-and-push-your-solution"></a>Oluşturun ve çözümünüzü gönderin

Modül kodunuzu ve bazı önemli dağıtım kavramları anlamak için dağıtım şablonu gözden geçirdim. Artık, IotEdgeModule1 kapsayıcı görüntüsünü oluşturma ve kapsayıcı kayıt defterinizde göndermek hazırsınız. Visual Studio için IOT araçları uzantısı ile bu adım Ayrıca şablon dosyasındaki bilgiler ve çözüm dosyalarını modülü bilgilerinden göre dağıtım bildirimi oluşturur. 

### <a name="sign-in-to-docker"></a>Docker'da oturum açın

Kapsayıcı görüntünüzü kayıt defterinde depolanan gönderebilirsiniz, böylece kapsayıcınızı geliştirme makinenizde Docker kayıt defteri kimlik bilgilerini sağlayın. 

1. PowerShell veya komut istemi açın.

2. Docker için kayıt defterini oluşturduktan sonra kaydedilmiş Azure kapsayıcı kayıt defteri kimlik bilgileri ile oturum açın. 

   ```cmd
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```

   Kullanılmasını öneren bir güvenlik uyarısı alabilirsiniz `--password-stdin`. Bu en iyi uygulama, üretim senaryoları için önerilir, bu öğreticinin kapsamı dışında olan. Daha fazla bilgi için [docker oturum açma](https://docs.docker.com/engine/reference/commandline/login/#provide-a-password-using-stdin) başvuru.

### <a name="build-and-push"></a>Derleme ve gönderme

Geliştirme makinenizde kapsayıcı kayıt defterinizde artık erişimi olduğundan ve IOT Edge cihazlarınıza güncelleştirilecektir. Bu, bir kapsayıcı görüntüsüne proje kodunu etkinleştirmek zamanı geldi. 

1. Sağ **AzureIotEdgeApp1.Windows.Amd64** proje klasörü ve select **derleme ve anında iletme IOT Edge modülleri**. 

   ![Oluşturun ve IOT Edge modülleri gönderin](./media/tutorial-develop-for-windows/build-and-push-modules.png)

   Derleme ve gönderme komut üç işlemi başlatır. İlk olarak, adlı bir çözüm içinde yeni bir klasör oluşturur **config** , tam bir dağıtım bildirimi, yerleşik dağıtım şablonu bilgilerinin kullanıma ve diğer çözüm dosyalarını içerir. İkinci olarak, çalıştığında `docker build` uygun dockerfile, hedef mimari için temel kapsayıcı görüntüsünü oluşturmak için. Ardından, çalışan `docker push` kapsayıcı kayıt defterinize görüntü deposuna gönderin. 

   Bu işlem ilk kez birkaç dakika sürebilir, ancak sonraki komutları çalıştırmak daha hızlıdır. 

2. Açık **deployment.windows amd64.json** bir klasörde yeni oluşturulan yapılandırma dosyası. (Yapılandırma klasörü Visual Studio'daki Çözüm Gezgini'nde görünmeyebilir. Bu durum geçerli seçin **tüm dosyaları göster** Çözüm Gezgini görev çubuğunda simge.)

3. Bulma **görüntü** IotEdgeModule1 bölümün parametresi. Module.json dosya adı, sürümü ve mimarisi etiketle tam görüntü deposuna görüntü içerdiğine dikkat edin.

4. Açık **module.json** IotEdgeModule1 klasöründeki dosya. 

5. Modül görüntüsü için sürüm numarasını değiştirin. (Sürüm, $schema-sürümü.) Örneğin, düzeltme eki sürüm numarasını Artır **0.0.2** artırmadığı küçük bir düzeltme modülü kodda yaptık. 

   >[!TIP]
   >Modül sürümleri, sürüm denetimi etkinleştirme ve güncelleştirmeleri üretim ortamına dağıtmadan önce küçük bir cihaz kümesini değişiklikleri test etmek sağlar. Modül sürümü oluşturma ve gönderme önce artırmanız yoksa, kapsayıcı kayıt defterinizde depoda üzerine yazın. 

6. Module.json dosyaya yaptığınız değişiklikleri kaydedin.

7. Sağ **AzureIotEdgeApp1.Windows.Amd64** proje klasörü yeniden ve yeniden seçin **derleme ve anında iletme IOT Edge modülleri**. 

8. Açık **deployment.windows amd64.json** yeniden dosya. Derleme ve gönderme komutu yeniden çalıştırıldığında yeni bir dosya oluşturulmadıysa dikkat edin. Bunun yerine, aynı dosya değişiklikleri yansıtacak şekilde güncelleştirildi. IotEdgeModule1 görüntü artık 0.0.2 için işaret kapsayıcı sürümü. Bu dağıtım bildirimi içinde yeni bir sürümü çekmek için bir modülün IOT Edge cihazı nasıl size değişikliktir. 

9. Daha fazla derleme ve gönderme komutu ne yaptığını doğrulamak için Git [Azure portalında](https://portal.azure.com) ve kapsayıcı kayıt defterinize gidin. 

10. Kapsayıcı kayıt defteriniz seçin **depoları** ardından **iotedgemodule1**. Görüntünün her iki sürümü kayıt defterine gönderildi doğrulayın.

    ![Kapsayıcı kayıt defterine iki görüntü sürümünü görüntüleme](./media/tutorial-develop-for-windows/view-repository-versions.png)

### <a name="troubleshoot"></a>Sorun giderme

Oluştururken ve modülü görüntünüzü gönderilirken hatalarla karşılaşırsanız, geliştirme makinenizde Docker yapılandırmasını yapmak genellikle sahiptir. Yapılandırmanızı gözden geçirmek için aşağıdaki denetimleri kullanın: 

* Vermedi çalıştırdığınızda `docker login` kapsayıcı kayıt defterinizden kopyaladığınız kimlik bilgilerini kullanarak komut? Bu kimlik bilgileri, Azure'da oturum açmak için kullandığınız yapılandırılanlardan farklı. 
* Kapsayıcı deponuza doğru mu? Doğru kapsayıcı kayıt defterinizin adı ve doğru modül adı var mı? Açık **module.json** denetlenecek IotEdgeModule1 klasöründeki dosya. Depo değeri gibi görünmelidir  **\<kayıt defteri adı\>.azurecr.io/iotedgemodule1**. 
* Yerine farklı bir ad kullandıysanız **IotEdgeModule1** adı, modül için çözümün tutarlı?
* Oluşturmakta olduğunuz kapsayıcıları aynı türde makineniz çalışıyor mu? Bu öğreticide, Visual Studio dosyalar Windows IOT Edge cihazları için olduğundan **windows-amd64** uzantısı ve Docker Masaüstü, Windows kapsayıcıları çalıştırılmalıdır. 

## <a name="deploy-modules-to-device"></a>Modüller cihazına dağıtma

Doğrulamanız yapıldı, oluşturulmuş kapsayıcı görüntüleri bir cihaz için dağıtılacağı zaman, bu nedenle, kapsayıcı kayıt defterinde depolanır. IOT Edge Cihazınızı ve çalışıyor olduğundan emin olun. 

1. Visual Studio'daki bulut Gezgini'ni açın ve IOT hub'ınız için ayrıntıları genişletin. 

2. Dağıtmak istediğiniz cihazın adını seçin. İçinde **eylemleri** listesinden **dağıtım oluşturma**.

   ![Tek bir cihaz için dağıtım oluşturma](./media/tutorial-develop-for-windows/create-deployment.png)


3. Dosya Gezgini'nde, proje ve seçin config klasörüne gidin **deployment.windows amd64.json** dosya. Bu dosya genellikle bulunur `C:\Users\<username>\source\repos\AzureIotEdgeApp1\AzureIotEdgeApp1.Windows.Amd64\config\deployment.windows-amd64.json`

   Tam Modül resim değerleri kaynakta yok deployment.template.json dosyanın kullanmayın. 

4. Ayrıntılar için IOT Edge Cihazınızı modülleri Cihazınızda görmek için Cloud Explorer'ı genişletin.

5. Kullanım **Yenile** IotEdgeModule1 modülleri ve tempSensor dağıtılan görmek için cihaz durumunu güncelleştirmek için düğmesini Cihazınızı. 


   ![IOT Edge Cihazınızda çalışan modülleri görüntüleme](./media/tutorial-develop-for-windows/view-running-modules.png)

## <a name="view-messages-from-device"></a>Bir CİHAZDAN iletilerini görüntüleme

IotEdgeModule1 kod kendi giriş kuyruk aracılığıyla iletileri alır ve bunları boyunca kendi çıkış kuyruğuna aktarır. Dağıtım bildirimi iletileri tempSensor IotEdgeModule1 için geçirilen ve IOT Hub'ına iletilen IotEdgeModule1 iletileri yollar bildirilmiş. Visual Studio için Azure IOT Edge araçları, IOT hub'da tek cihazlarınızdan geldikçe iletilerini görmek izin verin. 

1. Visual Studio cloud explorer'ın dağıtım IOT Edge cihazı adını seçin. 

2. İçinde **eylemleri** menüsünde **Başlat yerleşik olay uç nokta izleme**.

3. İzleme **çıkış** bölümde, IOT hub'da gelen iletileri görmek için Visual Studio'da. 

   Bu iki modülü de başlatmak birkaç dakika sürebilir. IOT Edge çalışma zamanı, yeni bir dağıtım bildirimi almak için kapsayıcı Çalışma Zamanı Modülü görüntülerden aşağıya çekin ve sonra her yeni modül başlatın. Varsa, 

   ![Gelen cihaz bulut iletilerini görüntüleme](./media/tutorial-develop-for-windows/view-d2c-messages.png)

## <a name="view-changes-on-device"></a>Cihazda değişiklikleri görüntüleme

Komutları, Cihazınızda neler olduğunu görmek istiyorsanız, bu bölümde IOT Edge çalışma zamanı ve Cihazınızda çalışan modüllerin incelemek için kullanın. 

Bu bölümde, IOT Edge cihazı için geliştirme makinenize komutlardır. IOT Edge cihazınız için bir sanal makine kullanıyorsanız, artık kendisine bağlayın. Azure'da, sanal makinenin Genel Bakış sayfasına gidin ve seçin **Connect** Uzak Masaüstü Bağlantısı erişmek için. Cihaz üzerinde çalıştırmak için bir komut veya PowerShell penceresi açın `iotedge` komutları.

* Cihaza dağıtılan tüm modülleri görüntülemek ve bunların durumunu kontrol edin:

   ```cmd
   iotedge list
   ```

   Dört modül görmeniz gerekir: iki IOT Edge Çalışma Zamanı Modülü, tempSensor ve IotEdgeModule1. Dört olarak listelenmiş olmalıdır.

* Belirli bir modül için günlükleri inceleyin:

   ```cmd
   iotedge logs <module name>
   ```

   IOT Edge modülleri, büyük/küçük harf duyarlıdır. 

   Bunlar işlem iletileri IotEdgeModule1 günlükleri ve tempSensor göstermelidir. Diğer modüllerin kendi günlükleri dağıtım bildiriminin uygulama hakkında bilgi sahiptir; bu nedenle başlatmak için edgeAgent modülü sorumludur. Herhangi bir modülden listede veya çalışmadığından, edgeAgent günlükleri muhtemelen hataları olacaktır. EdgeHub modülü, IOT hub'ı ve modüller arasındaki iletişimi sorumludur. Modülleri ayarlama ve çalışıyor, ancak iletileri, IOT hub'da gelen olmayan edgeHub günlükleri muhtemelen hataları olacaktır. 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, geliştirme makinenizde Visual Studio 2019 ' ayarlayın ve ondan sizin ilk IOT Edge modülü dağıtılan. Temel kavramları biliyorsanız, üzerinden geçen verileri analiz etmek için bir modül işlevselliği ekleme deneyin. Tercih ettiğiniz dili seçin: 

> [!div class="nextstepaction"] 
> [C](tutorial-c-module-windows.md)
> [C#](tutorial-csharp-module-windows.md)
