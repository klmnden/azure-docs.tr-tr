---
title: Azure IOT Edge sürekli tümleştirme ve sürekli dağıtım | Microsoft Docs
description: Sürekli tümleştirme ve sürekli dağıtım Azure IOT Edge için genel bakış
author: shizn
manager: ''
ms.author: xshi
ms.date: 11/12/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 06dec64a55aaece4cd67ebf0485e34aa206a8936
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2018
ms.locfileid: "51633742"
---
# <a name="continuous-integration-and-continuous-deployment-to-azure-iot-edge"></a>Sürekli tümleştirme ve sürekli dağıtım için Azure IOT Edge

DevOps ile uygulamalarınızı Azure IOT Edge ile kolayca benimseyebilirsiniz [Azure IOT Edge için Azure işlem hatları](https://marketplace.visualstudio.com/items?itemName=vsc-iot.iot-edge-build-deploy) veya [Jenkins için Azure IOT Edge eklentisi](https://plugins.jenkins.io/azure-iot-edge). Bu makalede, sürekli tümleştirme nasıl kullanabileceğinizi gösterir ve sürekli dağıtım özelliklerini Azure işlem hatları ve Microsoft Team Foundation Server (TFS) oluşturmak için test ve uygulamaları hızla ve verimli bir şekilde dağıtmak için Azure IOT Edge. 

Bu makalede, öğreneceksiniz nasıl yapılır:
* Oluşturun ve IOT Edge çözümü bir örnek denetleyin.
* Azure DevOps için Azure IOT Edge uzantısını yükleyin.
* Sürekli Tümleştirme (CI) çözümü derlemek için yapılandırın.
* Çözümü dağıtmak ve yanıtları görüntülemek için sürekli dağıtım (CD) yapılandırın.

Bu makaledeki adımlarda tamamlanması 30 dakika sürer.

![CI ve CD](./media/how-to-ci-cd/cd.png)


## <a name="create-a-sample-azure-iot-edge-solution-using-visual-studio-code"></a>Visual Studio Code kullanarak örnek bir Azure IOT Edge çözüm oluşturma

Bu bölümde, yapı işleminin bir parçası olarak yürütebilen çözüm birim testleri içeren örnek bir IOT Edge oluşturacaksınız. Bu bölümdeki yönergeleri izlemeden önce bölümündeki adımları tamamlamanız [bir IOT Edge çözümü Visual Studio code'da birden çok modül ile geliştirme](tutorial-multiple-modules-in-vscode.md).

1. VS Code komut paleti yazın ve şu komutu çalıştırın **Azure IOT Edge: IOT Edge yeni çözüm**. Çalışma alanı klasörünüzde'ı seçin, çözüm adı sağlayın (varsayılan ad **EdgeSolution**) ve bir C# modülü oluşturun (**FilterModule**) Bu çözümdeki ilk kullanıcı modülü olarak. İlk modülünüz için Docker görüntü deposunu da belirtmeniz gerekir. Varsayılan görüntü deposu yerel bir Docker kayıt defteri bağlıdır (`localhost:5000/filtermodule`). Azure Container Registry'ye değiştirmeniz gerekir (`<your container registry address>/filtermodule`) veya daha fazla sürekli tümleştirme için Docker hub'ı.

    ![ACR ' ayarlayın](./media/how-to-ci-cd/acr.png)

2. VS Code penceresinin, IOT Edge çözüm çalışma alanı yükler. İsteğe bağlı olarak yazın ve çalıştırabilirsiniz **Azure IOT Edge: IOT Edge Modülü Ekle** daha fazla modül eklemek için. Var olan bir `modules` klasöründe bir `.vscode` klasörü ve dağıtım bildirim şablonu dosyası kök klasöründe. Tüm kullanıcı modülü kodları alt klasörü altında olacak `modules`. `deployment.template.json` Dağıtım bildirimi şablonudur. Bazı parametreler bu dosyadaki gelen ayrıştırılacak `module.json`, her modül klasöründe bulunmaktadır.

3. Artık IOT Edge çözüm örneğinizi hazırdır. Varsayılan C# modülü kanal iletisi modül olarak görev yapar. İçinde `deployment.template.json`, bu çözümü içeren iki modül görürsünüz. İleti kaynaklandığı `tempSensor` modülü ve aracılığıyla doğrudan yöneltilen `FilterModule`, ardından IOT hub'ına gönderilen.

4. Bu projeler kaydettikten sonra Azure depoları veya TFS depoya denetleyin.
    
> [!NOTE]
> Azure depoları kullanma hakkında daha fazla bilgi için bkz. [Visual Studio ve Azure depoları ile kodunuzu paylaşmaya](https://docs.microsoft.com/azure/devops/repos/git/share-your-code-in-git-vs?view=vsts).


## <a name="configure-azure-pipelines-for-continuous-integration"></a>Azure işlem hatları için sürekli tümleştirmeyi yapılandırın
Bu bölümde, IOT Edge çözüm örnek değişiklikleri iade ettiğinizde otomatik olarak çalışacak şekilde yapılandırılmış bir derleme işlem hattı oluşturacaksınız ve Azure işlem hatlarında Derleme günlüklerini gösterir.

1. Azure DevOps kuruluşunuz oturum ( **https://dev.azure.com/{your kuruluş} /**) ve örnek uygulamada nereye iade projeyi açın.

    ![Kod iade etme](./media/how-to-ci-cd/init-project.png)

1. Ziyaret [Azure işlem hatları için Azure IOT Edge](https://marketplace.visualstudio.com/items?itemName=vsc-iot.iot-edge-build-deploy) Azure DevOps Market'te. Tıklayın **Ücretsiz edinin** ve Azure DevOps kuruluşunuz veya indirmek için TFS için bu uzantıyı yüklemek için sihirbazı izleyin.

    ![Uzantıyı yükleme](./media/how-to-ci-cd/install-extension.png)

1. Azure işlem hatlarınız açın **derleme ve yayınlama** hub hem de **yapılar** sekmesini, **+ yeni işlem hattı**. Ya da derleme işlem hatlarını zaten varsa, seçin **+ yeni** düğmesi.

    ![Yeni ardışık düzen](./media/how-to-ci-cd/add-new-build.png)

1. İstenirse, seçin **Git** kaynak türü. Ardından, proje, depo ve dal kodunuzu bulunduğu seçin. Seçin **devam**.

    ![Gıt'i seçin](./media/how-to-ci-cd/select-vsts-git.png)

    İçinde **bir şablon seçin** penceresinde seçin **boş bir işlemle başlangıç**.

    ![Bir şablon seçin](./media/how-to-ci-cd/start-with-empty.png)

1. Ardışık Düzen Düzenleyicisi'nde, aracı havuzu seçin. 
    
    * Linux kapsayıcıları için platform amd64, modülleri oluşturmak istiyorsanız seçin **barındırılan Ubuntu 1604**
    * Windows kapsayıcıları için platform amd64, modülleri derleme istiyorsanız seçin **Hosted VS2017** 
    * Linux kapsayıcıları için platform arm32v7, modülleri oluşturmak ister misiniz kümesine kendi Yapı aracınızı tıklayarak gerekirse **Yönet** düğmesi.
    
    ![Yapı aracısını yapılandırın](./media/how-to-ci-cd/configure-env.png)

1. Aracı işinde tıklayın "+" derleme işlem hattı iki görev eklemek için. İlk dandır **Azure IOT Edge**. Ve ikincisi dandır **derleme Yapıtları yayımlama**
    
    ![Görev ekleme](./media/how-to-ci-cd/add-tasks.png)

1. İlk **Azure IOT Edge** görev, güncelleştirme **görünen ad** için **modülü oluşturmak ve anında iletme**hem de **eylem** açılan listesinde seçin **Oluşturun ve gönderin**. İçinde **Module.json dosya** metin kutusuna aşağıdaki yolu ekleyin. Ardından **kapsayıcı kayıt defteri türü**, yapılandırma ve aynı kayıt defteri içinde code(module.json) seçin emin olun. Bu görev oluşturacak ve Çözümdeki tüm modüllerin gönderin ve belirttiğiniz kapsayıcı kayıt defterine yayımlama. Farklı kayıt defterlerinde modüllerinizi gönderilir, birden çok olabilir **modülü oluşturmak ve anında iletme** görevleri. IOT Edge çözüm kod deponuzun kök altında değil durumda, belirttiğiniz **çözüm kök yolu, Edge** içinde derleme tanımı.
    
    ![Derleme ve gönderme](./media/how-to-ci-cd/build-and-push.png)

1. İçinde **derleme Yapıtları yayımlama** görevi derleme görevi tarafından oluşturulan dağıtım dosyası belirtirsiniz. Ayarlama **yayımlama yolu** "config/deployment.json" için. Ayarlarsanız **çözüm kök yolu, Edge** son görevde kök yolu buraya katılın gerekecektir. Örneğin, Edge çözüm kök yolu ". / edgesolution", ardından **yayımlama yolu** olmalıdır ". / edgesolution/config/deployment.json". `deployment.json` Yoksayılabilir kırmızı hata satır metin kutusuna, bu nedenle, dosya derleme zamanında oluşturulur. 

    ![Yapıt yayımlama](./media/how-to-ci-cd/publish-build-artifacts.png)

1. Açık **Tetikleyicileri** sekmesini ve açma **sürekli tümleştirme** tetikleyici. Kodunuzu içeren dal dahil olduğundan emin olun.

    ![Tetikleyici yapılandırın](./media/how-to-ci-cd/configure-trigger.png)

    Yeni derleme işlem hattı kaydedin. **Kaydet** düğmesine tıklayın.


## <a name="configure-azure-pipelines-for-continuous-deployment"></a>Azure işlem hatları için sürekli dağıtımı yapılandırma
Bu bölümde, derleme işlem hattı, yapıtlar düştüğünde otomatik olarak çalışacak şekilde yapılandırılmış bir yayın işlem hattı oluşturacaksınız ve Azure işlem hatlarında dağıtım günlükleri gösterilir.

1. İçinde **yayınlar** sekmesini, **+ yeni işlem hattı**. Veya, yayın işlem hatları zaten varsa, seçin **+ yeni** düğmesi.  

    ![Yayın işlem hattı ekleyin](./media/how-to-ci-cd/add-release-pipeline.png)

    İçinde **bir şablon seçin** penceresinde seçin **ile boş bir proje başlatın.**

    ![Boş bir işlemle Başlat](./media/how-to-ci-cd/start-with-empty-job.png)

2. Ardından, yayın ardışık düzeni ile tek bir aşamada başlatmak: **Aşama 1**. Yeniden adlandırma **Aşama 1** için **QA** ve test ortamı olarak değerlendir. Tipik sürekli dağıtım işlem hattında, genellikle birden çok aşama yok, fazla DevOps uygulamanıza dayalı oluşturabilirsiniz.

    ![Aşama oluşturun](./media/how-to-ci-cd/QA-env.png)

3. Yayın derleme yapılarına bağlar. Tıklayın **Ekle** yapıtları alanında.

    ![Yapıt Ekle](./media/how-to-ci-cd/add-artifacts.png)  
    
    İçinde **bir yapıt sayfasını ekleme**, kaynak türünü seçin **yapı**. Ardından, projeyi ve oluşturduğunuz derleme işlem hattı seçin. Ardından **ekleme**.

    ![Bir yapıt ekleme](./media/how-to-ci-cd/add-an-artifact.png)

    Her seferinde yeni bir derleme kullanılabilir yeni bir yayın oluşturulur böylece, sürekli dağıtım tetikleyicisi açın.

    ![Tetikleyici yapılandırın](./media/how-to-ci-cd/add-a-trigger.png)

4. Gidin **QA aşama** ve bu aşamada görevlerini yapılandırın.

    ![QA görevlerini yapılandırma](./media/how-to-ci-cd/view-stage-tasks.png)

   Dağıtım görevi, yani seçebilirsiniz platformudur küçük harfe duyarlı **Hosted VS2017** veya **barındırılan Ubuntu 1604** içinde **aracı havuzu** (veya tarafından yönetilen herhangi bir aracı kendiniz). Tıklayın "+" ve bir görev ekleyin.

    ![QA için görev ekleyin](./media/how-to-ci-cd/add-task-qa.png)

5. Azure IOT Edge görevde gidin **eylem** açılan listesinden **IOT Edge cihazına dağıtma**. Seçin, **Azure aboneliği** ve giriş, **IOT hub'ı adı**. IOT Edge belirtebilirsiniz **dağıtım kimliği** ve dağıtım **öncelik**. Tek veya birden çok cihaza dağıtmayı seçebilirsiniz. Dağıtımını yapıyorsanız **birden çok cihaz**, cihaz belirtmeniz gereken **hedef koşulu**. Hedef koşul, bir IOT hub uç cihazlarına eşleşecek şekilde bir filtredir. Cihaz etiketleri koşul olarak kullanmak istiyorsanız, karşılık gelen etiketleri cihazlarınızı IOT Hub ile cihaz ikizi güncelleştirmeniz gerekiyor. Aşağıdaki ekran görüntüsünde gösterildiği gibi görev yapılandırması olmalıdır birkaç IOT Edge cihazları 'qa' etiketli olduğunu kabul edelim. 

    ![QA için dağıtma](./media/how-to-ci-cd/deploy-to-qa.png)

    Yeni yayın ardışık düzeni kaydedin. **Kaydet** düğmesine tıklayın. Ve ardından **işlem hattı** ardışık düzene dönmek için.

6. Üretim ortamınız için ikinci aşamasıdır. "PROD" yeni aşama ekleyin için yalnızca "Kalite güvence" Aşama kopyalayın ve kopyalanmış aşamasına Yeniden Adlandır **PROD**,

    ![Aşama kopyalama](./media/how-to-ci-cd/clone-stage.png)

7. Üretim ortamınız için görevleri yapılandırın. Birden çok IOT Edge cihazları 'prod', görev yapılandırmalarında "prod" ve "dağıtma-ürün" olarak dağıtım kimliği ayarlamak için hedef koşulu güncelleştirirken Etiketlenmedi olduğu varsayılır. **Kaydet** düğmesine tıklayın. Ve ardından **işlem hattı** ardışık düzene dönmek için.
    
    ![Üretime dağıtma](./media/how-to-ci-cd/deploy-to-prod.png)

7. Şu anda, bizim derleme yapıtı sürekli olarak üzerinde tetiklenecek **QA** aşama ve ardından **PROD** aşaması. Ancak, çoğu kez bazı test çalışmaları QA cihazlarda ve el ile tümleştirmek için gereken BITS onaylayın. Daha sonra BITS üretim ortamına dağıtılır. Üretim aşamasında aşağıdaki onay ayarlayın.

    1. Açık **dağıtım öncesi koşulları** ayarı paneli.

        ![Açık dağıtım öncesi koşulları](./media/how-to-ci-cd/pre-deploy-conditions.png)    

    2. Ayarlama **etkin** içinde **dağıtım öncesi onayları**. Doldurun **onaylayanlar** giriş. Daha sonra **Kaydet**'e tıklayın.
    
        ![Durumlar belirleyin](./media/how-to-ci-cd/set-pre-deployment-conditions.png)


8. Yayın işlem hattınızı aşağıdakileri olarak ayarlanmış olan şimdi.

    ![Yayın ardışık düzeni](./media/how-to-ci-cd/release-pipeline.png)

    
## <a name="verify-iot-edge-cicd-with-the-build-and-release-pipelines"></a>IOT Edge CI/CD ile yapı doğrulayın ve yayın işlem hatları

Bu bölümde, iş CI/CD işlem hattı yapmak için bir derleme tetikler. Dağıtım başarılı olduktan sonra doğrulayın.

1. Bir derleme işi tetiklemek için kaynak kodu deposu için bir işleme için gönderim veya el ile tetiklersiniz. Tıklayarak, derleme işlem hattı, bir derleme işi tetikleyebilirsiniz **kuyruk** aşağıdaki ekran görüntüsünde gösterildiği gibi düğmesi.

    ![El ile tetikleme](./media/how-to-ci-cd/manual-trigger.png)

2. Derleme işlem hattı başarıyla tamamlanırsa için yayın tetikleyeceğine **QA** aşaması. Derleme işlem hattı günlükleri gidin ve aşağıdaki görmeniz gerekir.

    ![Derleme günlükleri](./media/how-to-ci-cd/build-logs.png)

3. Başarılı dağıtımı **QA** aşama onaylayan bir bildirim tetikleme. Yayın işlem gidin, aşağıdakilere bakın. 

    ![Onay bekleniyor](./media/how-to-ci-cd/pending-approval.png)


4. Onaylayanın onay verdikten sonra bu değişiklik, dağıtılabilir **PROD**.

    ![Üretim için dağıtma](./media/how-to-ci-cd/approve-and-deploy-to-prod.png)


## <a name="next-steps"></a>Sonraki adımlar

* IOT Edge dağıtımı anlamak [IOT Edge dağıtımlarını anlama tek tek cihazlarda veya uygun ölçekte](module-deployment-monitoring.md)
* Oluşturmak, güncelleştirmek veya bir dağıtımda silmek için adımlarında yol [dağıtma ve izleme uygun ölçekte IOT Edge modülleri](how-to-deploy-monitor.md).
