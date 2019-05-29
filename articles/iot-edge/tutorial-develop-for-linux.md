---
title: Linux cihazlar - Azure IOT Edge modülü geliştirme | Microsoft Docs
description: Bu öğreticide Linux kapsayıcıları için Linux cihazları kullanarak IOT Edge modülleri geliştirmek için geliştirme makine ve bulut kaynaklarını ayarlama yoluyla açıklanmaktadır.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 04/26/2019
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 11fa72f5853350c76b2a8d0aa4fd7b96b598b670
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66303860"
---
# <a name="tutorial-develop-iot-edge-modules-for-linux-devices"></a>Öğretici: IOT Edge modülleri Linux cihazlar için geliştirme

Visual Studio Code geliştirip IOT Edge çalıştıran Linux cihazlar için kod dağıtmak için kullanın. 

Hızlı Başlangıç makalelerini, bir Linux sanal makinesini kullanarak bir IOT Edge cihazı oluşturdunuz ve Azure Market'ten önceden oluşturulmuş bir modül dağıttınız. Bu öğreticide, geliştirme ve IOT Edge cihazına kendi kodunuzu dağıtmak için neler aracılığıyla açıklanmaktadır. Bu öğreticide, belirli programlama dilleri ya da Azure hizmetleri hakkında daha fazla ayrıntıya gider tüm diğer öğreticiler, kullanışlı bir önkoşuldur. 

Bu öğreticide örnek dağıtma bir **C modülü Linux cihazına**. En az bir önkoşul olduğundan bu örnek, böylece doğru kitaplıkları yüklü olup hakkında endişelenmeden geliştirme araçları hakkında bilgi edinebilirsiniz seçildi. Ardından geliştirme kavramları anladığınızda, tercih ettiğiniz dil veya ayrıntılara inin yönelik Azure hizmeti seçin. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Geliştirme makinenizde ayarlayın.
> * Yeni bir proje oluşturmak için Visual Studio Code için IOT Edge araçlarını kullanın.
> * Projenizi bir kapsayıcı olarak ve bir Azure container Registry'de depolayın.
> * Kodunuzu IOT Edge cihazına dağıtma. 


[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="key-concepts"></a>Önemli kavramlar

Bu öğretici, IOT Edge modülü geliştirmeden açıklar. Bir *IOT Edge Modülü*, veya bazı durumlarda yalnızca *Modülü* kısa için yürütülebilir kod içeren bir kapsayıcıdır. Bir veya daha fazla modül bir IOT Edge cihazına dağıtabilirsiniz. Modüller, veri temizleme işlemleri ya da bir IOT hub'ına ileti göndermek ya da veri analizi, sensörlerden alınan verileri almak gibi belirli görevleri gerçekleştirin. Daha fazla bilgi için [anlamak Azure IOT Edge modülleri](iot-edge-modules.md).

IOT Edge modülleri geliştirirken, bir geliştirme makineniz ve IOT Edge cihazı modülü sonunda dağıtılacağı hedef arasındaki farkı anlamak önemlidir. İşletim sistemi (OS) modülü kodunuzu saklamak için yapı kapsayıcı eşleşmelidir *hedef cihaz*. Örneğin, en sık karşılaşılan bir senaryodur IOT Edge çalıştıran bir Linux cihazı hedeflemeniz amaçlanıyorsa bir Windows bilgisayarda bir modül geliştirme kişidir. Bu durumda, kapsayıcı işletim sistemi Linux olacaktır. Bu öğreticide ilerlerken, arasındaki farkı akılda tutulması *geliştirme makine işletim sistemi* ve *kapsayıcı işletim sistemi*.

Bu öğreticide, Linux IOT Edge çalıştıran cihazlara hedefler. Geliştirme makinenizde Linux kapsayıcıları çalıştırabilirsiniz sürece, tercih ettiğiniz geliştirme makine işletim sistemi kullanabilirsiniz. Linux cihazlar için bu öğreticiyi kullanın, bu nedenle geliştirmek için Visual Studio Code kullanılması önerilir. Destek iki aracı arasındaki farklılıkları olsa da Visual Studio'yu kullanabilirsiniz.

Aşağıdaki tabloda desteklenen geliştirme senaryoları için **Linux kapsayıcıları** Visual Studio Code ve Visual Studio.

|   | Visual Studio Code | Visual Studio 2017/2019 |
| - | ------------------ | ------------------ |
| **Linux cihaz mimarisi** | Linux AMD64 <br> Linux ARM32 | Linux AMD64 <br> Linux ARM32 |
| **Azure Hizmetleri** | Azure İşlevleri <br> Azure Stream Analytics <br> Azure Machine Learning |   |
| **Diller** | C <br> C# <br> Java <br> Node.js <br> Python | C <br> C# |
| **Daha fazla bilgi** | [Visual Studio Code için Azure IOT Edge](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) | [Azure IOT Edge için Visual Studio 2017 Araçları](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vsiotedgetools), [Azure IOT Edge için Visual Studio 2019 araçları](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vs16iotedgetools) |

Bu öğretici, Visual Studio Code için geliştirme adımlarını öğretir. Bunun yerine Visual Studio kullanmayı tercih ediyorsanız, yönergeleri başvurmak [kullanan Visual Studio geliştirme ve modülleri, Azure IOT Edge için hata ayıklama için 2019](how-to-visual-studio-develop-module.md).

## <a name="prerequisites"></a>Önkoşullar

Bir geliştirme makinesi:

* Kendi bilgisayarınıza veya sanal makine, geliştirme tercihlerinize bağlı olarak kullanabilirsiniz.
* Bir kapsayıcı altyapısı çalıştırabilirsiniz çoğu işletim sistemi, Linux cihazları için IOT Edge modülleri geliştirmek için kullanılabilir. Bu öğreticide bir Windows bilgisayar kullanır, ancak MacOS veya Linux'ta bilinen farklar çıkış noktaları. 
* Yükleme [Git](https://git-scm.com/), modül şablon paketleri, bu öğreticinin ilerleyen bölümlerinde çekmek için.  

Linux üzerinde Azure IOT Edge cihazı:

* IOT Edge geliştirme makinenizde çalışmaz, ancak ayrı bir cihaz kullanmanız önerilir. Daha fazla geliştirme makineniz ve IOT Edge cihazı arasındaki bu fark doğru bir şekilde doğru dağıtım senaryosu yansıtır ve farklı kavramları düz tutmaya yardımcı olur.
* Kullanılabilir ikinci bir cihazınız yoksa, Azure ile IOT Edge cihazı oluşturmak için hızlı başlangıç makalesi kullanmak bir [Linux sanal makinesi](quickstart-linux.md).

Bulut kaynakları:

* Ücretsiz veya standart katman [IOT hub'ı](../iot-hub/iot-hub-create-through-portal.md) azure'da. 

## <a name="install-container-engine"></a>Kapsayıcı Altyapısı'nı

IOT Edge modülleri, kapsayıcı altyapısı oluşturmak ve kapsayıcıları yönetmek için geliştirme makinenizde gereken şekilde kapsayıcıları olarak paketlenir. Çok sayıda özellik ve popülerliğini kapsayıcısını altyapısı olarak nedeniyle, Docker masaüstü geliştirme için kullanmanızı öneririz. Bir Windows cihazda Docker masaüstü ile Windows kapsayıcıları ile Linux kapsayıcıları arasında IOT Edge cihazları farklı türleri için modüller kolayca geliştirebilmeniz geçiş yapabilirsiniz. 

Geliştirme makinenizde yüklemek için Docker belgeleri kullanın: 

* [Windows için Docker Masaüstü yükleme](https://docs.docker.com/docker-for-windows/install/)

  * İçin Docker Masaüstü Windows yüklediğinizde, Linux veya Windows kapsayıcıları kullanmak isteyip istemediğinizi sorulur. Bu karar, kolay bir anahtar kullanarak herhangi bir zamanda değiştirilebilir. Bu öğreticide, Linux cihazlarını bizim modülleri hedeflediğiniz çünkü Linux kapsayıcıları kullanırız. Daha fazla bilgi için [Windows ve Linux kapsayıcılar arasında geçiş](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers).

* [Mac için Docker Masaüstü yükleme](https://docs.docker.com/docker-for-mac/install/)

* Okuma [hakkında Docker CE](https://docs.docker.com/install/) çeşitli Linux platformlarında yükleme bilgileri.

## <a name="set-up-vs-code-and-tools"></a>VS Code ve araçları ayarlama

IOT Edge modülleri geliştirmek için Visual Studio Code için IOT uzantıları kullanın. Bu uzantıları proje şablonları sağlar, dağıtım bildirimini oluşturulmasını otomatikleştirin ve izleyin ve IOT Edge cihazları yönetmenize olanak sağlar. Bu bölümde, Visual Studio Code ve IOT uzantısını yükleyin ve ardından IOT Hub dahilindeki Visual Studio Code yönetmek için Azure hesabınızı ayarlama. 

1. Yükleme [Visual Studio Code](https://code.visualstudio.com/) geliştirme makinenizde. 

2. Yükleme tamamlandıktan sonra seçin **görünümü** > **uzantıları**. 

3. Arama **Azure IOT Araçları**, hangi olan gerçekten yardımcı uzantıları koleksiyonu etkileşim IOT Hub ve IOT cihazları yanı ile IOT Edge modülleri geliştirme. 

4. **Yükle**’yi seçin. Tek başına dahil edilen her bir uzantı yükler. 

5. Uzantıları bittiğinde yüklemeden, seçerek komut paletini açın **görünümü** > **komut paleti**. 

6. Komut paletini içinde arayın ve seçin **Azure: Oturum**. Azure hesabınızda oturum açmak için yönergeleri izleyin. 

7. Komut paletinden tekrar aramak ve seçmek **Azure IOT Hub: IOT hub'ını seçin**. Azure aboneliğinizi ve IOT hub'ı seçmek için yönergeleri izleyin. 

7. Visual Studio kod Gezgini bölüm ya da sol taraftaki etkinlik çubuğundaki simgesini veya seçerek açın **görünümü** > **Gezgini**. 

8. Explorer bölümünün altında daraltılmış genişletin **Azure IOT Hub cihazları** menüsü. Cihazlar ve komut paletini seçili IOT hub'ı ile ilişkili IOT Edge cihazları görmeniz gerekir. 

   ![IOT hub'ınızda cihazları görüntüle](./media/tutorial-develop-for-linux/view-iot-hub-devices.png)

[!INCLUDE [iot-edge-create-container-registry](../../includes/iot-edge-create-container-registry.md)]

## <a name="create-a-new-module-project"></a>Yeni Modül projesi oluşturma

Azure IOT araçları uzantısı proje şablonları için desteklenen tüm IOT Edge modülü dilleri Visual Studio code'da sağlar. Bu şablonları tüm dosyaları ve IOT Edge test etmek için bir çalışma modül dağıtmak için ihtiyacınız olan kod veya kendi iş mantığına sahip şablonu özelleştirmek için bir başlangıç noktası sağlar. 

Bu öğretici için yüklemek için en az önkoşulları içerdiğinden C modülü şablonu kullanırız. 

### <a name="create-a-project-template"></a>Bir proje şablonu oluşturma

Visual Studio Code komut paletini içinde arayın ve seçin **Azure IOT Edge: Yeni bir IOT Edge çözüm**. Komut istemlerini izleyin ve çözümünüzü oluşturmak için aşağıdaki değerleri kullanın: 

   | Alan | Değer |
   | ----- | ----- |
   | Klasör seçin | Geliştirme makinenizde VS Code'un çözüm dosyalarını oluşturmak için kullanacağı konumu seçin. |
   | Çözüm adı sağlayın | Çözümünüz için açıklayıcı bir ad girin veya varsayılan değerleri kabul **EdgeSolution**. |
   | Modül şablonunu seçin | Seçin **C Modülü**. |
   | Modül adı sağlayın | Varsayılan değerleri kabul **SampleModule**. |
   | Modül için Docker görüntü deposunu sağlama | Görüntü deposu, kapsayıcı kayıt defterinizin adını ve kapsayıcı görüntünüzün adını içerir. Kapsayıcı görüntünüzü, son adımda sağladığınız adından doldurulur. **localhost:5000** yerine Azure kapsayıcı kayıt defterinizden alacağınız oturum açma sunucusu değerini yazın. Oturum açma sunucusunu Azure portalda kapsayıcı kayıt defterinizin Genel bakış sayfasından alabilirsiniz. <br><br> Son görüntü deposuna benzer \<kayıt defteri adı\>.azurecr.io/samplemodule. |
 
   ![Docker görüntü deposunu sağlama](./media/tutorial-develop-for-linux/image-repository.png)

Visual Studio kod penceresinde yeni çözümünüzü yüklendikten sonra oluşturduğu dosyaları tanımak için bir dakikanızı ayırın: 

* **.Vscode** klasörde adlı bir dosya **launch.json**, modülleri hata ayıklamak için kullanılır.
* **Modülleri** klasör, çözümünüzde her bir modül için bir klasör içerir. Yalnızca şu anda, **SampleModule**, veya modülü, ad verdi. Ana program kodu, modül meta verileri ve birkaç Docker dosya SampleModule klasör içerir. 
* **.Env** dosyası, kapsayıcı kayıt defterinizde kimlik bilgilerini tutar. Kapsayıcı görüntülerini çekerken erişim sahip olacak şekilde, bu kimlik bilgileri ile IOT Edge Cihazınızı paylaşılır. 
* **Deployment.debug.template.json** dosya ve **deployment.template.json** dosya yardımcı şablonları oluşturma olan bir dağıtım bildirimi. A *dağıtım bildirimi* tam olarak bir cihaza dağıtılan istediğiniz modülleri, nasıl yapılandırılacağı ve birbirleriyle ve bulut ile nasıl kurabilmek tanımlayan bir dosyadır. Şablon dosyaları için bazı değerler işaretçileri kullanın. Doğru dağıtım bildirimine şablon dönüştürdüğünüzde, işaretçilerin diğer çözüm dosyalarından alınan değerleri ile değiştirilir. İki ortak yer tutucuları dağıtım şablonunuzda bulun: 

  * Kayıt defteri kimlik bilgileri bölümünde, bir çözüm oluşturduğunuzda sağladığınız bilgilerden autofilled adresidir. Ancak, kullanıcı adı ve parola değişkenleri .env dosyasında depolanan başvuru. Güvenlik için .env dosyasında git yoksayıldı, ancak dağıtım şablonu değil olarak budur. 
  * Çözümü oluşturduğunuzda, görüntü deposuna sağladığınız olsa bile SampleModule bölümünde, kapsayıcı görüntüsü doldurulmuş değil. Bu yer tutucu işaret **module.json** SampleModule klasörü içinde dosya. Bu dosyaya giderseniz, görüntü alanını depo, aynı zamanda sürümü ve kapsayıcı platformu oluşur bir etiket değeri içermiyor görürsünüz. Sürüm, Geliştirme döngüsünün bir parçası el ile yineleyebilirsiniz ve size tanıtan bir değiştirici daha sonra bu bölümde kullanarak kapsayıcı platformu seçin. 

### <a name="provide-your-registry-credentials-to-the-iot-edge-agent"></a>IOT Edge Aracısı ile kayıt defteri kimlik bilgilerinizi sağlayın

Ortam dosyası, kapsayıcı kayıt defterinizin kimlik bilgilerini depolar ve bu bilgileri IoT Edge çalışma zamanı ile paylaşır. Çalışma zamanı, kapsayıcı görüntülerinizi IOT Edge cihazı üzerine çıkarmak için bu kimlik bilgileri gerekir. 

IOT Edge uzantısını kapsayıcı kayıt defteri kimlik bilgilerini Azure çekme ve bunları ortam dosyasında doldurmak çalışır. Kimlik bilgilerinizi zaten dahil olup olmadığını denetleyin. Aksi durumda, şimdi ekleyin:

1. Açık **.env** modülü çözümünüzdeki dosya. 
2. Ekleme **kullanıcıadı** ve **parola** Azure kapsayıcı kayıt defterinizden kopyaladığınız değerleri.
3. .Env dosyaya yaptığınız değişiklikleri kaydedin. 

### <a name="select-your-target-architecture"></a>Hedef Mimarinizi seçin

Şu anda, Visual Studio Code, C modülleri Linux AMD64 ve Linux ARM32v7 cihazlar için geliştirebilirsiniz. Kapsayıcı yerleşik olarak bulunur ve çalışan nasıl etkilediği için her bir çözüm ile hedeflediğiniz hangi mimari seçmeniz gerekir. Linux AMD64 varsayılandır. 

1. Komut paletini açın ve arama **Azure IOT Edge: Varsayılan hedef Platform için Edge çözümü ayarlayın**, veya pencerenin alt kısmındaki kenar çubuğu kısayol simgesini seçin. 

   ![Kenar Çubuğu mimarisi simgesini seçin](./media/tutorial-develop-for-linux/select-architecture.png)

2. Komut paletini hedef mimari seçeneklerini listeden seçin. Bu öğreticide, bir Ubuntu sanal makinesi varsayılan tutacak şekilde IOT Edge cihazı kullandığımız **amd64**. 

### <a name="review-the-sample-code"></a>Örnek kodu gözden geçirin

Oluşturduğunuz çözüm şablonu, bir IOT Edge modülü için örnek kod içerir. Bu örnek modülü yalnızca ileti alır ve bunları geçirdiği sonra. İşlem hattı işlevleri modüller birbirleri ile nasıl iletişim kuracağını olan IOT Edge, önemli bir kavramdır gösterir.

Her modülü birden çok olabilir *giriş* ve *çıkış* kuyrukları bildirilen kodları. IOT Edge hub'ı kullanarak cihaz üzerinde çalışan bir modülün çıkışına iletilerden modüllerinin bir veya daha fazla giriş yönlendirir. Giriş ve çıkışları bildirmek için belirli bir dil diller arasında farklılık gösterir ancak tüm modüller arasında kavramı aynıdır. Modüller arasında yönlendirme hakkında daha fazla bilgi için bkz. [bildirmek yollar](module-composition.md#declare-routes).

1. Açık **main.c** içinde olan dosya **modülleri/SampleModules/** klasör. 

2. IOT Hub C SDK'sı işlevini kullanan [SetInputMessageCallback](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/iothub-module-client-ll-h/iothubmoduleclient-ll-setinputmessagecallback) giriş kuyrukları modülü başlatılamadı. Bu işlevin main.c dosyasında arayın.

3. SetInputMessageCallback işlev Oluşturucu gözden geçirin ve bir giriş sırası adlı bkz **input1** kodda başlatılır. 

   ![SetInputMessageCallback oluşturucuda giriş adını bulma](./media/tutorial-develop-for-linux/declare-input-queue.png)

4. Modül çıkış sıraları benzer şekilde başlatılır. Arama [SendEventToOutputAsync](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/iothub-module-client-ll-h/iothubmoduleclient-ll-sendeventtooutputasync) main.c dosyasındaki işlevi. 

5. SendEventToOutputAsync işlev Oluşturucu gözden geçirin ve bir çıkış kuyruğuna adlı bkz **output1** kodda başlatılır. 

   ![Çıkış adı SendEventToOutputAsync içinde Bul](./media/tutorial-develop-for-linux/declare-output-queue.png)

6. Açık **deployment.template.json** dosya.

7. Bulma **modülleri** $edgeAgent özelliğini istenen özellikleri. 

   Burada listelenen iki modül olması gerekir. İlk **tempSensor**, hangi tüm şablonlarda modüllerinizi test etmek için kullanabileceğiniz sanal sıcaklık verilerini sağlamak için varsayılan olarak eklenir. İkinci **SampleModule** bu çözümün bir parçası oluşturduğunuz modülü.

7. Dosyanın sonunda bulmak için istenen Özellikler **$edgeHub** modülü. 

   IOT Edge hub'ı modülündeki işlevlerin bir dağıtımdaki tüm modüller arasında iletileri yönlendirmek için biridir. Değerleri gözden **yollar** özelliği. İlk yol **SampleModuleToIoTHub**, bir joker karakter kullanan ( **\*** ) SampleModule modüldeki tüm çıkış sıraları gelen iletileri belirtmek için. Bu iletiler kısımlarda *Yukarı Akış $* , IOT hub'ı gösteren ayrılmış bir ad olduğu. İkinci rota, sensorToSampleModule, tempSensor modülünden gelen iletileri alır ve bunları yönlendirir *input1* başlatılan SampleModule kodunda gördüğünüz giriş sırası. 

   ![Yollar deployment.template.json gözden geçirin](./media/tutorial-develop-for-linux/deployment-routes.png)

## <a name="build-and-push-your-solution"></a>Oluşturun ve çözümünüzü gönderin

Modül kodunuzu ve bazı önemli dağıtım kavramları anlamak için dağıtım şablonu gözden geçirdim. Artık, SampleModule kapsayıcı görüntüsünü oluşturma ve kapsayıcı kayıt defterinizde göndermek hazırsınız. Visual Studio Code için IOT araçları uzantısı ile bu adım Ayrıca şablon dosyasındaki bilgiler ve çözüm dosyalarını modülü bilgilerinden göre dağıtım bildirimi oluşturur. 

### <a name="sign-in-to-docker"></a>Docker'da oturum açın

Kapsayıcı kayıt defterinde depolanan kapsayıcı görüntünüzü itmeden Docker kayıt defteri kimlik bilgilerini sağlayın. 

1. Visual Studio Code tümleşik Terminalini açmak **görünümü** > **Terminal**.

2. Docker için kayıt defterini oluşturduktan sonra kaydedilmiş Azure kapsayıcı kayıt defteri kimlik bilgileri ile oturum açın. 

   ```cmd/sh
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```

   Kullanılmasını öneren bir güvenlik uyarısı alabilirsiniz `--password-stdin`. Bu en iyi uygulama, üretim senaryoları için önerilir, bu öğreticinin kapsamı dışında olan. Daha fazla bilgi için [docker oturum açma](https://docs.docker.com/engine/reference/commandline/login/#provide-a-password-using-stdin) başvuru.

### <a name="build-and-push"></a>Derleme ve gönderme 

Bir kapsayıcı görüntüsüne çözüm kodunu açmak için zaman, bu nedenle visual Studio Code artık, kapsayıcı kayıt defterine erişebilir. 

1. Visual Studio Code Gezgininde sağ **deployment.template.json** seçin ve dosya **derleme ve anında iletme IOT Edge çözüm**. 

   ![Oluşturun ve IOT Edge modülleri gönderin](./media/tutorial-develop-for-linux/build-and-push-modules.png)

   Derleme ve gönderme komut üç işlemi başlatır. İlk olarak, adlı bir çözüm içinde yeni bir klasör oluşturur **config** , tam bir dağıtım bildirimi, yerleşik dağıtım şablonu bilgilerinin kullanıma ve diğer çözüm dosyalarını içerir. İkinci olarak, çalıştığında `docker build` uygun dockerfile, hedef mimari için temel kapsayıcı görüntüsünü oluşturmak için. Ardından, çalışan `docker push` kapsayıcı kayıt defterinize görüntü deposuna gönderin. 

   Bu işlem ilk kez birkaç dakika sürebilir, ancak sonraki komutları çalıştırmak daha hızlıdır. 

2. Açık **deployment.amd64.json** yeni oluşturulan config klasöründeki dosya. Farklı bir mimari seçerseniz, farklı olacaktır, böylece dosya hedef mimari yansıtır.

3. Artık yer tutucular olan iki parametre değerlerini uygun oturum doldurulur dikkat edin. **RegistryCredentials** kayıt defteri kullanıcı kimliğiniz ve parolanızı .env dosyasında çekilen bölüm vardır. **SampleModule** module.json dosyanın adı, sürümü ve mimarisi etiketi ile tam görüntü deposuna sahip. 

4. Açık **module.json** SampleModule klasöründeki dosya. 

5. Modül görüntüsü için sürüm numarasını değiştirin. (Sürüm, $schema-sürümü.) Örneğin, düzeltme eki sürüm numarasını Artır **0.0.2** artırmadığı küçük bir düzeltme modülü kodda yaptık. 

   >[!TIP]
   >Modül sürümleri, sürüm denetimi etkinleştirme ve güncelleştirmeleri üretim ortamına dağıtmadan önce küçük bir cihaz kümesini değişiklikleri test etmek sağlar. Modül sürümü oluşturma ve gönderme önce artırmanız yoksa, kapsayıcı kayıt defterinizde depoda üzerine yazın. 

6. Module.json dosyaya yaptığınız değişiklikleri kaydedin.

7. Sağ **deployment.template.json** yeniden dosya ve yeniden seçin **derleme ve anında iletme IOT Edge çözüm**.

8. Açık **deployment.amd64.json** yeniden dosya. Derleme ve gönderme komutu yeniden çalıştırıldığında yeni bir dosya oluşturulmadıysa dikkat edin. Bunun yerine, aynı dosya değişiklikleri yansıtacak şekilde güncelleştirildi. SampleModule görüntü artık 0.0.2 için işaret kapsayıcı sürümü. 

9. Daha fazla derleme ve gönderme komutu ne yaptığını doğrulamak için Git [Azure portalında](https://portal.azure.com) ve kapsayıcı kayıt defterinize gidin. 

10. Kapsayıcı kayıt defteriniz seçin **depoları** ardından **samplemodule**. Görüntünün her iki sürümü kayıt defterine gönderildi doğrulayın.

   ![Kapsayıcı kayıt defterine iki görüntü sürümünü görüntüleme](./media/tutorial-develop-for-linux/view-repository-versions.png)

<!--Alternative steps: Use VS Code Docker tools to view ACR images with tags-->

### <a name="troubleshoot"></a>Sorun gider

Oluştururken ve modülü görüntünüzü gönderilirken hatalarla karşılaşırsanız, geliştirme makinenizde Docker yapılandırmasını yapmak genellikle sahiptir. Yapılandırmanızı gözden geçirmek için aşağıdaki denetimleri kullanın: 

* Vermedi çalıştırdığınızda `docker login` kapsayıcı kayıt defterinizden kopyaladığınız kimlik bilgilerini kullanarak komut? Bu kimlik bilgileri, Azure'da oturum açmak için kullandığınız yapılandırılanlardan farklı. 
* Kapsayıcı deponuza doğru mu? Doğru kapsayıcı kayıt defterinizin adı ve doğru modül adı var mı? Açık **module.json** denetlenecek SampleModule klasöründeki dosya. Depo değeri gibi görünmelidir  **\<kayıt defteri adı\>.azurecr.io/samplemodule**. 
* Yerine farklı bir ad kullandıysanız **SampleModule** adı, modül için çözümün tutarlı?
* Oluşturmakta olduğunuz kapsayıcıları aynı türde makineniz çalışıyor mu? Bu öğreticide Visual Studio Code yazmalıdır Linux IOT Edge cihazları için olduğundan **amd64** veya **arm32v7** kenar çubuğu olarak ve Linux kapsayıcılarını Docker Masaüstü çalıştırılmalıdır. Visual Studio code'da C modülleri Windows kapsayıcıları desteği yoktur. 

## <a name="deploy-modules-to-device"></a>Modüller cihazına dağıtma

Doğrulamanız yapıldı, oluşturulmuş kapsayıcı görüntüleri bir cihaz için dağıtılacağı zaman, bu nedenle, kapsayıcı kayıt defterinde depolanır. IOT Edge Cihazınızı ve çalışıyor olduğundan emin olun. 

1. Visual Studio Code Gezgininde Azure IOT Hub cihazları bölümü genişletin. 

2. Sağ tıklayın, dağıtın ve ardından istediğiniz IOT Edge cihazı **tek cihaz için dağıtım oluşturma**. 

   ![Tek bir cihaz için dağıtım oluşturma](./media/tutorial-develop-for-linux/create-deployment.png)

3. Dosya Gezgini'nde gidin **config** klasör seçip **deployment.amd64.json** dosya. 

   Kapsayıcı kayıt defteri kimlik bilgilerini veya resim değerleri modülü içinde yok deployment.template.json dosyanın kullanmayın. Bir Linux ARM32 cihaz hedefliyorsanız, dağıtım bildirimini deployment.arm32v7.json adlandırılacaktır. 

4. IOT Edge cihazınız için Ayrıntılar'ı genişletin, sonra da genişletin **modülleri** cihazınız için liste. 

5. Cihazınızda SampleModule modülleri ve tempSensor görene kadar cihaz Görünümü güncelleştirmek için Yenile düğmesini kullanın. 

   Bu iki modülü de başlatmak birkaç dakika sürebilir. IOT Edge çalışma zamanı, yeni bir dağıtım bildirimi almak için kapsayıcı Çalışma Zamanı Modülü görüntülerden aşağıya çekin ve sonra her yeni modül başlatın. 

   ![IOT Edge Cihazınızda çalışan modülleri görüntüleme](./media/tutorial-develop-for-linux/view-running-modules.png)

## <a name="view-messages-from-device"></a>Bir CİHAZDAN iletilerini görüntüleme

SampleModule kod kendi giriş kuyruk aracılığıyla iletileri alır ve bunları boyunca kendi çıkış kuyruğuna aktarır. Dağıtım bildirimi iletileri tempSensor SampleModule için geçirilen ve IOT Hub'ına iletilen SampleModule iletileri yollar bildirilmiş. Visual Studio Code için Azure IOT araçları, IOT hub'da tek cihazlarınızdan geldikçe iletilerini görmek izin verin. 

1. Visual Studio Code Gezgininde sağ tıklayın, izleyin ve ardından istediğiniz IOT Edge cihazı **Başlat yerleşik olay uç nokta izleme**. 

2. Visual Studio Code, IOT hub'da gelen iletileri görmek için çıkış penceresine bakın. 

   ![Gelen cihaz bulut iletilerini görüntüleme](./media/tutorial-develop-for-linux/view-d2c-messages.png)

## <a name="view-changes-on-device"></a>Cihazda değişiklikleri görüntüleme

Komutları, Cihazınızda neler olduğunu görmek istiyorsanız, bu bölümde IOT Edge çalışma zamanı ve Cihazınızda çalışan modüllerin incelemek için kullanın. 

Bu bölümde, IOT Edge cihazı için geliştirme makinenize komutlardır. IOT Edge cihazınız için bir sanal makine kullanıyorsanız, artık kendisine bağlayın. Azure'da, sanal makinenin Genel Bakış sayfasına gidin ve seçin **Connect** güvenli Kabuk bağlantı erişmek için. 

* Cihaza dağıtılan tüm modülleri görüntülemek ve bunların durumunu kontrol edin:

   ```bash
   iotedge list
   ```

   Dört modül görmeniz gerekir: iki IOT Edge Çalışma Zamanı Modülü, tempSensor ve SampleModule. Dört olarak listelenmiş olmalıdır.

* Belirli bir modül için günlükleri inceleyin:

   ```bash
   iotedge logs <module name>
   ```

   IOT Edge modülleri, büyük/küçük harf duyarlıdır. 

   Bunlar işlem iletileri SamplModule günlükleri ve tempSensor göstermelidir. Diğer modüllerin kendi günlükleri dağıtım bildiriminin uygulama hakkında bilgi sahiptir; bu nedenle başlatmak için edgeAgent modülü sorumludur. Herhangi bir modülden listede veya çalışmadığından, edgeAgent günlükleri muhtemelen hataları olacaktır. EdgeHub modülü, IOT hub'ı ve modüller arasındaki iletişimi sorumludur. Modülleri ayarlama ve çalışıyor, ancak iletileri, IOT hub'da gelen olmayan edgeHub günlükleri muhtemelen hataları olacaktır. 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, geliştirme makinenizde Visual Studio kodu ayarlamak ve ondan sizin ilk IOT Edge modülü dağıtılan. Temel kavramları biliyorsanız, üzerinden geçen verileri analiz etmek için bir modül işlevselliği ekleme deneyin. Tercih ettiğiniz dili seçin: 

> [!div class="nextstepaction"] 
> [C](tutorial-c-module.md)
> [C#](tutorial-csharp-module.md)
> [Java](tutorial-java-module.md)
> [Node.js](tutorial-node-module.md)
> [Python](tutorial-python-module.md)