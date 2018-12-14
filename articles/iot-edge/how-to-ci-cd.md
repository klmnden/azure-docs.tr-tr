---
title: Sürekli tümleştirme ve sürekli dağıtım - Azure IOT Edge | Microsoft Docs
description: Sürekli tümleştirme ve sürekli dağıtım - Azure IOT Edge ile Azure DevOps, Azure işlem hatlarını ayarlama
author: shizn
manager: philmea
ms.author: xshi
ms.date: 12/12/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: a714cec5ce05473887f9f06d47c75563bf878081
ms.sourcegitcommit: 85d94b423518ee7ec7f071f4f256f84c64039a9d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2018
ms.locfileid: "53386834"
---
# <a name="continuous-integration-and-continuous-deployment-to-azure-iot-edge"></a>Sürekli tümleştirme ve sürekli dağıtım için Azure IOT Edge

Azure IOT Edge uygulamalarınızı Azure işlem hatları yerleşik Azure IOT Edge görevleri ile DevOps kolayca benimseyebilirsiniz veya [Jenkins için Azure IOT Edge eklentisi](https://plugins.jenkins.io/azure-iot-edge) Jenkins sunucunuzda. Bu makalede, derleme, test etme ve uygulamaları, Azure IOT Edge için hızlı ve verimli bir şekilde dağıtmak için sürekli tümleştirme ve sürekli dağıtım özellikleri Azure işlem hatlarını nasıl kullanabileceğinizi gösterir. 

Bu makalede, öğreneceksiniz nasıl yapılır:
* Oluşturun ve IOT Edge çözümü bir örnek denetleyin.
* Sürekli Tümleştirme (CI) çözümü derlemek için yapılandırın.
* Çözümü dağıtmak ve yanıtları görüntülemek için sürekli dağıtım (CD) yapılandırın.

![Geliştirme ve üretim için diyagram - CI ve CD dallar](./media/how-to-ci-cd/cd.png)


## <a name="create-a-sample-azure-iot-edge-solution-using-visual-studio-code"></a>Visual Studio Code kullanarak örnek bir Azure IOT Edge çözüm oluşturma

Bu bölümde, yapı işleminin bir parçası olarak yürütebilen çözüm birim testleri içeren örnek bir IOT Edge oluşturacaksınız. Bu bölümdeki yönergeleri izlemeden önce bölümündeki adımları tamamlamanız [bir IOT Edge çözümü Visual Studio code'da birden çok modül ile geliştirme](how-to-develop-multiple-modules-vscode.md).

1. VS Code komut paleti yazın ve şu komutu çalıştırın **Azure IOT Edge: Yeni bir IOT Edge çözüm**. Çalışma alanı klasörünüzde'ı seçin, çözüm adı sağlayın (varsayılan ad **EdgeSolution**) ve bir C# modülü oluşturun (**FilterModule**) Bu çözümdeki ilk kullanıcı modülü olarak. İlk modülünüz için Docker görüntü deposunu da belirtmeniz gerekir. Varsayılan görüntü deposu yerel bir Docker kayıt defteri bağlıdır (`localhost:5000/filtermodule`). Azure Container Registry'ye değiştirme (`<your container registry address>/filtermodule`) veya daha fazla sürekli tümleştirme için Docker hub'ı.

    ![Azure kapsayıcı kayıt defterini ayarlayın](./media/how-to-ci-cd/acr.png)

2. VS Code penceresinin, IOT Edge çözüm çalışma alanı yükler. İsteğe bağlı olarak yazın ve çalıştırabilirsiniz **Azure IOT Edge: IOT Edge Modülü Ekle** daha fazla modül eklemek için. Var olan bir `modules` klasöründe bir `.vscode` klasörü ve dağıtım bildirim şablonu dosyası kök klasöründe. Tüm kullanıcı modülü kodları alt klasörü altında olacak `modules`. `deployment.template.json` Dağıtım bildirimi şablonudur. Bazı parametreler bu dosyadaki gelen ayrıştırılacak `module.json`, her modül klasöründe bulunmaktadır.

3. Artık IOT Edge çözüm örneğinizi hazırdır. Varsayılan C# modülü kanal iletisi modül olarak görev yapar. İçinde `deployment.template.json`, bu çözümü içeren iki modül görürsünüz. İleti kaynaklandığı `tempSensor` modülü ve aracılığıyla doğrudan yöneltilen `FilterModule`, ardından IOT hub'ına gönderilen.

4. Bu projeler kaydettikten sonra Azure Depolarınızı işleyin.
    
> [!NOTE]
> Azure depoları kullanma hakkında daha fazla bilgi için bkz. [Visual Studio ve Azure depoları ile kodunuzu paylaşmaya](https://docs.microsoft.com/azure/devops/repos/git/share-your-code-in-git-vs?view=vsts).


## <a name="configure-azure-pipelines-for-continuous-integration"></a>Azure işlem hatları için sürekli tümleştirmeyi yapılandırın
Bu bölümde, IOT Edge çözüm örnek değişiklikleri iade ettiğinizde otomatik olarak çalışacak şekilde yapılandırılmış bir derleme işlem hattı oluşturacaksınız ve Azure işlem hatlarında Derleme günlüklerini gösterir.

1. Azure DevOps kuruluşunuz oturum ( **https://dev.azure.com/{your kuruluş} /**) ve örnek uygulamada nereye iade projeyi açın.

    ![Azure işlem hatları için onay kodu](./media/how-to-ci-cd/init-project.png)

1. Azure işlem hatlarınız açın **yapılar** sekmesini, **+ yeni işlem hattı**. Ya da derleme işlem hatlarını zaten varsa, seçin **+ yeni** düğmesi. Ardından **yeni derleme işlem hattı**.

    ![Yeni derleme işlem hattı oluşturma](./media/how-to-ci-cd/add-new-build.png)

1. İstenirse Azure depoları için kaynağınızı seçin. Ardından, proje, depo ve dal kodunuzu bulunduğu seçin. Seçin **devam**.

    ![Azure depoları Git seçin](./media/how-to-ci-cd/select-vsts-git.png)

    İçinde **bir şablon seçin** penceresinde seçin **boş bir işlemle başlangıç**.

    ![Boş bir işlemle başlangıç](./media/how-to-ci-cd/start-with-empty.png)

1. Ardışık Düzen Düzenleyicisi'nde, aracı havuzu seçin. 
    
    * Linux kapsayıcıları için platform amd64, modülleri oluşturmak istiyorsanız seçin **barındırılan Ubuntu 1604**
    * Windows kapsayıcıları için platform amd64, modülleri derleme istiyorsanız seçin **Hosted VS2017** 
    * Linux kapsayıcıları için platform arm32v7, modülleri oluşturmak ister misiniz kümesine kendi Yapı aracınızı tıklayarak gerekirse **Yönet** düğmesi.
    
    ![Derleme aracı havuzu yapılandırma](./media/how-to-ci-cd/configure-env.png)

1. Aracı işinde tıklayın "+" derleme işlem hattı, üç görev eklemek için. İlk iki arasındadır **Azure IOT Edge**. Ve üçüncü dandır **derleme Yapıtları yayımlama**
    
    ![Derleme işlem hattı için görev ekleyin](./media/how-to-ci-cd/add-tasks.png)

1. İlk **Azure IOT Edge** görev, güncelleştirme **görünen ad** için **Azure IOT Edge - derleme modül görüntüleri**hem de **eylem** açılır listesinden **derleme modül görüntüleri**. İçinde **. template.json dosyasını** denetimi, select **deployment.template.json** IOT Edge çözümünüzü tanımlayan dosya. Ardından **varsayılan platform**, IOT Edge cihazınız olarak aynı platforma seçtiğinizden emin olun. Bu görev, belirtilen hedef platform çözümüyle tüm modüllerdeki oluşturacaksınız. Ve ayrıca **deployment.json** dosya, dosya yolu çıkış değişkenleri bulabilirsiniz. Diğer adı ayarlama `edge` bu değişkeni.
    
    ![Derleme modülü görüntüleri görevi yapılandırma](./media/how-to-ci-cd/build-and-push.png)

1. İkinci **Azure IOT Edge** görev, güncelleştirme **görünen ad** için **Azure IOT Edge - anında iletme modül görüntüleri**hem de **eylem** açılır listesinden **modül görüntüleri itme**. Kapsayıcı kayıt defteri türü seçin, yapılandırma ve aynı kayıt defteri içinde code(module.json) seçin emin olun. İçinde **. template.json dosyasını** denetimi, select **deployment.template.json** IOT Edge çözümünüzü tanımlayan dosya. Ardından **varsayılan platform**, yerleşik modülü görüntüleriniz için aynı platforma seçtiğinizden emin olun. Bu görev, tüm modül görüntüleri, seçtiğiniz kapsayıcı kayıt defterine iletilir. Kapsayıcı kayıt defteri kimlik bilgilerini de ekleyin **deployment.json** dosya, dosya yolu çıkış değişkenleri bulabilirsiniz. Diğer adı ayarlama `edge` bu değişkeni. Modül görüntüleri barındırmak için birden çok kapsayıcı kayıt defterleri varsa, bu görev yinelenen, farklı bir kapsayıcı kayıt defteri seçin ve kullanmak gereken **atlama modul** bu olmayan görüntüleri atlamak için Gelişmiş ayarları belirli kayıt defteri.

    ![Anında iletme modül görüntüleri görevi yapılandırma](./media/how-to-ci-cd/push.png)

1. İçinde **derleme Yapıtları yayımlama** görevi derleme görevi tarafından oluşturulan dağıtım dosyası belirtirsiniz. Ayarlama **yayımlama yolu** için `$(edge.DEPLOYMENT_FILE_PATH)`.

    ![Yapılandırma yapıt görev yayımlama](./media/how-to-ci-cd/publish-build-artifacts.png)

1. Açık **Tetikleyicileri** sekmesini ve açma **sürekli tümleştirme** tetikleyici. Kodunuzu içeren dal dahil olduğundan emin olun.

    ![Sürekli Tümleştirme tetikleyici Aç](./media/how-to-ci-cd/configure-trigger.png)

    Yeni derleme işlem hattı kaydedin. **Kaydet** düğmesine tıklayın.


## <a name="configure-azure-pipelines-for-continuous-deployment"></a>Azure işlem hatları için sürekli dağıtımı yapılandırma
Bu bölümde, derleme işlem hattı, yapıtlar düştüğünde otomatik olarak çalışacak şekilde yapılandırılmış bir yayın işlem hattı oluşturacaksınız ve Azure işlem hatlarında dağıtım günlükleri gösterilir.

1. İçinde **yayınlar** sekmesini, **+ yeni işlem hattı**. Veya, yayın işlem hatları zaten varsa, seçin **+ yeni** düğmesini tıklatın ve tıklatın **+ yeni yayın işlem**.  

    ![Yayın işlem hattı ekleyin](./media/how-to-ci-cd/add-release-pipeline.png)

    İçinde **bir şablon seçin** penceresinde seçin **ile boş bir proje başlatın.**

    ![Boş bir işlemle Başlat](./media/how-to-ci-cd/start-with-empty-job.png)

2. Ardından yayın ardışık düzeni ile tek bir aşamada başlatmak: **1. Aşama**. Yeniden adlandırma **Aşama 1** için **QA** ve test ortamı olarak değerlendir. Tipik sürekli dağıtım işlem hattında, genellikle birden çok aşama yok, fazla DevOps uygulamanıza dayalı oluşturabilirsiniz.

    ![Test ortamı aşama oluşturun](./media/how-to-ci-cd/QA-env.png)

3. Yayın derleme yapılarına bağlar. Tıklayın **Ekle** yapıtları alanında.

    ![Yapıt Ekle](./media/how-to-ci-cd/add-artifacts.png)  
    
    İçinde **bir yapıt sayfasını ekleme**, kaynak türünü seçin **yapı**. Ardından, projeyi ve oluşturduğunuz derleme işlem hattı seçin. Ardından **ekleme**.

    ![Bir derleme yapıtı Ekle](./media/how-to-ci-cd/add-an-artifact.png)

    Her seferinde yeni bir derleme kullanılabilir yeni bir yayın oluşturulur böylece, sürekli dağıtım tetikleyicisi açın.

    ![Sürekli dağıtım tetikleyicisi yapılandırın](./media/how-to-ci-cd/add-a-trigger.png)

4. Gidin **QA aşama** ve bu aşamada görevlerini yapılandırın.

    ![QA görevlerini yapılandırma](./media/how-to-ci-cd/view-stage-tasks.png)

   Dağıtım görevi, yani seçebilirsiniz platformudur küçük harfe duyarlı **Hosted VS2017** veya **barındırılan Ubuntu 1604** içinde **aracı havuzu** (veya tarafından yönetilen herhangi bir aracı kendiniz). Tıklayın "+" ve bir görev ekleyin.

    ![QA için görev ekleyin](./media/how-to-ci-cd/add-task-qa.png)

5. Azure IOT Edge görevde gidin **eylem** açılan listesinden **IOT Edge cihazına dağıtma**. Seçin, **Azure aboneliği** ve giriş, **IOT hub'ı adı**. Tek veya birden çok cihaza dağıtmayı tercih edebilirsiniz. Dağıtımını yapıyorsanız **birden çok cihaz**, cihaz belirtmeniz gereken **hedef koşulu**. Hedef koşul, bir IOT hub uç cihazlarına eşleşecek şekilde bir filtredir. Cihaz etiketleri koşul olarak kullanmak istiyorsanız, karşılık gelen etiketleri cihazlarınızı IOT Hub ile cihaz ikizi güncelleştirmeniz gerekiyor. Güncelleştirme **IOT Edge dağıtımı kimliği** "dağıtma-kalite güvence" içinde için Gelişmiş ayarları. Aşağıdaki ekran görüntüsünde gösterildiği gibi görev yapılandırması olmalıdır birkaç IOT Edge cihazları 'qa' etiketli olduğunu kabul edelim. 

    ![QA için dağıtma](./media/how-to-ci-cd/deploy-to-qa.png)

    Yeni yayın ardışık düzeni kaydedin. **Kaydet** düğmesine tıklayın. Ve ardından **işlem hattı** ardışık düzene dönmek için.

6. Üretim ortamınız için ikinci aşamasıdır. Yeni aşama "PROD" eklemek için "Kalite güvence" Aşama kopyalayın ve kopyalanmış aşamasına Yeniden Adlandır **PROD**,

    ![Aşama kopyalama](./media/how-to-ci-cd/clone-stage.png)

7. Üretim ortamınız için görevleri yapılandırın. Birden çok IOT Edge cihazları 'prod', görev yapılandırmalarında "prod" ve "dağıtma-ürün" Gelişmiş ayarları olarak dağıtım Kimliğini ayarlamak için hedef koşulu güncelleştirirken Etiketlenmedi olduğu varsayılır. **Kaydet** düğmesine tıklayın. Ve ardından **işlem hattı** ardışık düzene dönmek için.
    
    ![Üretime dağıtma](./media/how-to-ci-cd/deploy-to-prod.png)

7. Şu anda, bizim derleme yapıtı sürekli olarak üzerinde tetiklenecek **QA** aşama ve ardından **PROD** aşaması. Ancak, çoğu kez bazı test çalışmaları QA cihazlarda ve el ile tümleştirmek için gereken BITS onaylayın. Daha sonra BITS üretim ortamına dağıtılır. Üretim aşamasında onay aşağıdaki ekran görüntüsünde olarak ayarlayın.

    1. Açık **dağıtım öncesi koşulları** ayarı paneli.

        ![Açık dağıtım öncesi koşulları](./media/how-to-ci-cd/pre-deploy-conditions.png)    

    2. Ayarlama **etkin** içinde **dağıtım öncesi onayları**. Doldurun **onaylayanlar** giriş. Daha sonra **Kaydet**'e tıklayın.
    
        ![Durumlar belirleyin](./media/how-to-ci-cd/set-pre-deployment-conditions.png)


8. Artık yayın işlem hattınızı aşağıdaki ekran görüntüsü ayarlanmış.

    ![Yayın ardışık düzeni](./media/how-to-ci-cd/release-pipeline.png)

    
## <a name="verify-iot-edge-cicd-with-the-build-and-release-pipelines"></a>IOT Edge CI/CD ile yapı doğrulayın ve yayın işlem hatları

Bu bölümde, iş CI/CD işlem hattı yapmak için bir derleme tetikler. Dağıtım başarılı olduktan sonra doğrulayın.

1. Bir derleme işi tetiklemek için kaynak kodu deposu için bir işleme için gönderim veya el ile tetiklersiniz. Tıklayarak, derleme işlem hattı, bir derleme işi tetikleyebilirsiniz **kuyruk** aşağıdaki ekran görüntüsünde gösterildiği gibi düğmesi.

    ![El ile tetikleme](./media/how-to-ci-cd/manual-trigger.png)

2. Derleme işlem hattı başarıyla tamamlanırsa için yayın tetikleyeceğine **QA** aşaması. Derleme işlem hattı günlükleri gidin ve aşağıdaki ekran görmeniz gerekir.

    ![Derleme günlükleri](./media/how-to-ci-cd/build-logs.png)

3. Başarılı dağıtımı **QA** aşama onaylayan bir bildirim tetikleme. Yayın işlem gidin, aşağıdaki ekran görüntüsünde görebilirsiniz. 

    ![Onay bekleniyor](./media/how-to-ci-cd/pending-approval.png)


4. Onaylayanın onay verdikten sonra bu değişiklik, dağıtılabilir **PROD**.

    ![Üretim için dağıtma](./media/how-to-ci-cd/approve-and-deploy-to-prod.png)


## <a name="next-steps"></a>Sonraki adımlar

* IOT Edge dağıtımı anlamak [IOT Edge dağıtımlarını anlama tek tek cihazlarda veya uygun ölçekte](module-deployment-monitoring.md)
* Oluşturmak, güncelleştirmek veya bir dağıtımda silmek için adımlarında yol [dağıtma ve izleme uygun ölçekte IOT Edge modülleri](how-to-deploy-monitor.md).
