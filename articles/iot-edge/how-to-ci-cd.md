---
title: Sürekli tümleştirme ve sürekli dağıtım - Azure IOT Edge | Microsoft Docs
description: Sürekli tümleştirme ve sürekli dağıtım - Azure IOT Edge ile Azure DevOps, Azure işlem hatlarını ayarlama
author: shizn
manager: philmea
ms.author: xshi
ms.date: 01/22/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: f449449c542ce6ac04daa58ff37a3577f0d75aee
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61222056"
---
# <a name="continuous-integration-and-continuous-deployment-to-azure-iot-edge"></a>Sürekli tümleştirme ve sürekli dağıtım için Azure IOT Edge

DevOps, Azure IOT Edge uygulamalarınızı Azure işlem hatları yerleşik Azure IOT Edge görevleri ile kolayca benimseyebilirsiniz. Bu makalede, derleme, test etme ve uygulamaları, Azure IOT Edge için hızlı ve verimli bir şekilde dağıtmak için sürekli tümleştirme ve sürekli dağıtım özellikleri Azure işlem hatlarını nasıl kullanabileceğinizi gösterir. 

Bu makalede, Azure işlem hatları için yerleşik Azure IOT Edge görevleri IOT Edge çözümünüz için iki işlem hattı oluşturmak için nasıl kullanılacağını öğrenin. İlk kodunuzu alır ve modül görüntülerinizi kapsayıcı kayıt defterinize gönderme ve bir dağıtım bildirimi oluşturma çözümü derler. İkinci modüllerinizi hedeflenen IOT Edge cihazlarına dağıtılır.  

![Geliştirme ve üretim için diyagram - CI ve CD dallar](./media/how-to-ci-cd/cd.png)


## <a name="prerequisites"></a>Önkoşullar

* Bir Azure depoları deposu. Yoksa, şunları yapabilirsiniz [projenizde yeni Git deposu oluşturma](https://docs.microsoft.com/azure/devops/repos/git/create-new-repo?view=vsts&tabs=new-nav).
* IOT Edge çözümünü, kaydedilen ve deponuza gönderdiniz. Bu makalede test etmek için yeni örnek bir çözüm oluşturmak istiyorsanız, adımları [geliştirme ve hata ayıklama modülleri Visual Studio code'da](how-to-vs-code-develop-module.md) veya [geliştirme ve hata ayıklama C# Visual Studio'daki modüller](how-to-visual-studio-develop-csharp-module.md).
   * Bu makale için ihtiyacınız olan IOT Edge şablonları Visual Studio Code veya Visual Studio tarafından oluşturulan çözüm klasörü. Derleme, anında iletme, dağıtmak veya devam etmeden önce bu kodda hata ayıklama gerek yoktur. Bu işlemleri Azure işlem hatlarında ayarlayacağınız. 
   * Yeni bir çözüm oluşturuyorsanız, deponuzda yerel olarak ilk kopyalayın. Ardından, bir çözüm oluşturduğunuzda, doğrudan depo klasörü oluşturmak seçebilirsiniz. Kolayca, işleyin ve buradan yeni dosyalar gönderin. 
* Modül görüntüleri burada gönderebilmek için bir kapsayıcı kayıt defteri. Kullanabileceğiniz [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) veya bir üçüncü taraf kayıt defteri. 
* Etkin bir [IOT hub'ı](../iot-hub/iot-hub-create-through-portal.md) ile en az ayrı test ve Üretim dağıtımı aşamaları test etmek için IOT Edge cihazları. IOT Edge cihazı oluşturmak için hızlı başlangıç makalelerini takip edebilirsiniz [Linux](quickstart-linux.md) veya [Windows](quickstart.md)


Azure depoları kullanma hakkında daha fazla bilgi için bkz. [kodunuzu Visual Studio ve Azure depoları ile paylaşma](https://docs.microsoft.com/azure/devops/repos/git/share-your-code-in-git-vs?view=vsts)

## <a name="configure-continuous-integration"></a>Sürekli tümleştirmeyi yapılandırın
Bu bölümde, yeni bir derleme işlem hattı oluşturun. IOT Edge çözüm örnek değişiklikleri iade edin ve derleme günlükleri yayımlayacak otomatik olarak çalıştırmak için işlem hattı yapılandırın.

>[!NOTE]
>Bu makalede, Azure DevOps görsel tasarımcı kullanır. Bu bölümdeki adımları izlemeden önce yeni YAML işlem hattı oluşturma deneyimi için önizleme özelliğini kapatın. 
>1. Azure DevOps, profil simgesine ve ardından seçin **Önizleme özellikleri**.
>2. Kapatma **yeni YAML işlem hattı oluşturma deneyimini** devre dışı. 
>
>Daha fazla bilgi için [derleme işlem hattı oluşturma](https://docs.microsoft.com/azure/devops/pipelines/get-started-designer?view=vsts&tabs=new-nav#create-a-build-pipeline).

1. Azure DevOps kuruluşunuz oturum (**https:\//dev.azure.com/{your kuruluş} /**) ve IOT Edge çözüm deponuzu içeren projeyi açın.

   Bu makale için oluşturduğumuz adlı bir depo **IoTEdgeRepo**. Bu depoyu içeren **IoTEdgeSolution** hangi bir modül için kod adlı **filtermodule**. 

   ![DevOps projenizi açın](./media/how-to-ci-cd/init-project.png)

2. Azure işlem hatlarına projenize gidin. Açık **yapılar** sekmenize **yeni işlem hattı**. Ya da derleme işlem hatlarını zaten varsa, seçin **yeni** düğmesi. Ardından **yeni derleme işlem hattı**.

    ![Yeni derleme işlem hattı oluşturma](./media/how-to-ci-cd/add-new-build.png)

3. İşlem hattınızı oluşturmak için istemleri izleyin. 

   1. Yeni derleme işlem hattı için kaynak bilgileri sağlayın. Seçin **Azure depoları Git** proje, depo ve dal IOT Edge çözüm kodunuzu bulunduğu kaynak olarak ardından seçin. Ardından, **devam**. 

      ![İşlem hattı kaynağınızı seçin](./media/how-to-ci-cd/pipeline-source.png)

   2. Seçin **boş iş** şablon yerine. 

      ![Boş bir işlemle başlangıç](./media/how-to-ci-cd/start-with-empty.png)

4. İşlem hattınızı oluşturduktan sonra işlem hattı düzenleyicisine alınır. İşlem hattı Tanımınızda, hedef platforma göre doğru aracı havuzu seçin: 
    
   * Linux kapsayıcıları için platform amd64, modülleri oluşturmak istiyorsanız seçin **barındırılan Ubuntu 1604**

   * Windows 1809 kapsayıcılar için platform amd64, modülleri oluşturmak ister misiniz için gerekirse [şirket içinde barındırılan Windows aracısında ayarlama](https://docs.microsoft.com/azure/devops/pipelines/agents/v2-windows?view=vsts).

   * Linux kapsayıcıları için platform arm32v7, modülleri oluşturmak ister misiniz için gerekirse [Linux üzerinde şirket içinde barındırılan aracı ayarlama](https://blogs.msdn.microsoft.com/iotdev/2018/11/13/setup-azure-iot-edge-ci-cd-pipeline-with-arm-agent/).
    
     ![Derleme aracı havuzu yapılandırma](./media/how-to-ci-cd/configure-env.png)

5. İşlem hattınızı adında bir iş ile önceden yapılandırılmış olarak gelir **1 aracı işi**. Artı işaretini seçin (**+**) üç görev projeye eklemek için: **Azure IOT Edge** iki kez ve **derleme Yapıtları yayımlama** sonra. (Görmek için her görevin adının üzerine gelme **Ekle** düğmesi.)

   ![Azure IOT Edge görev ekleyin](./media/how-to-ci-cd/add-iot-edge-task.png)

   Tüm üç görev eklendiğinde, aracı işi aşağıdaki örnekteki gibi görünür:
    
   ![Derleme işlem hattı üç görevleri](./media/how-to-ci-cd/add-tasks.png)

6. İlk seçin **Azure IOT Edge** düzenlemek için görev. Bu görev hedef platform çözümüyle tüm modüllerdeki derlemeler belirlediğiniz, ayrıca oluşturur **deployment.json** IOT Edge cihazlarınıza söyler dağıtım yapılandırma dosyası.

   * **Görünen ad**: Varsayılan değerleri kabul **Azure IOT Edge - derleme modül görüntüleri**.
   * **Eylem**: Varsayılan değerleri kabul **derleme modül görüntüleri**. 
   * **. template.json dosyasını**: Öğesinin üç noktasını (**...** ) gidin **deployment.template.json** depodaki IOT Edge çözümünüzü içeren dosya. 
   * **Varsayılan platform**: Modüllerinizi, hedefte IOT Edge cihazı için uygun platformu seçin. 
   * **Çıkış değişkenleri**: Çıkış değişkenleri deployment.json dosyanızın oluşturulacağı dosya yolu yapılandırmak için kullanabileceğiniz bir başvuru adı içerir. Başvuru adı gibi bir şey akılda kalıcı kümesine **edge**. 

7. İkinci seçin **Azure IOT Edge** düzenlemek için görev. Bu görev tüm modül görüntüleri, seçtiğiniz kapsayıcı kayıt defterine iletir. Ayrıca, kapsayıcı kayıt defteri kimlik bilgilerini ekler **deployment.json** IOT Edge Cihazınızı modül görüntüleri erişebilmesi için dosya. 

   * **Görünen ad**: Eylem alanını değiştirdiğinde görünen adı otomatik olarak güncelleştirilir. 
   * **Eylem**: Seçmek için açılan listeyi kullanın **modül görüntüleri itme**. 
   * **Kapsayıcı kayıt defteri türü**: Modül görüntülerinizi depolamak için kullandığınız bir kapsayıcı kayıt defteri türü seçin. Seçtiğiniz hangi kayıt defteri türüne bağlı olarak, form değişiklikleri. Seçerseniz **Azure Container Registry**, Azure aboneliği ve kapsayıcı kayıt defterinizin adı seçmek için açılır listeleri kullanın. Seçerseniz **genel kapsayıcı kayıt defteri**seçin **yeni** bir kayıt defteri hizmeti bağlantısı oluşturmak için. 
   * **. template.json dosyasını**: Öğesinin üç noktasını (**...** ) gidin **deployment.template.json** depodaki IOT Edge çözümünüzü içeren dosya. 
   * **Varsayılan platform**: Yerleşik modülü görüntülerinizi aynı platformu seçin.

   Modül görüntüleri barındırmak için birden çok kapsayıcı kayıt defterleri varsa, bu görev yinelenen, farklı bir kapsayıcı kayıt defteri seçin ve kullanmak gereken **atlama modul** bu olmayan görüntüleri atlamak için Gelişmiş ayarları belirli kayıt defteri.

8. Seçin **derleme Yapıtları yayımlama** düzenlemek için görev. Derleme görevi tarafından oluşturulan dağıtım dosya dosya yolunu belirtin. Ayarlama **yayımlama yolu** derleme modülü görevde ayarladığınız Çıkış değişkeni eşleşecek değer. Örneğin, `$(edge.DEPLOYMENT_FILE_PATH)`. Diğer değerleri varsayılan bırakın. 

9. Açık **Tetikleyicileri** sekmesini ve kutuyu **sürekli tümleştirmeyi etkinleştir**. Kodunuzu içeren dal dahil olduğundan emin olun.

    ![Sürekli Tümleştirme tetikleyici Aç](./media/how-to-ci-cd/configure-trigger.png)

10. Yeni derleme işlem hattı ile Kaydet **Kaydet** düğmesi.

Bu işlem hattı, artık yeni kod deponuza göndererek otomatik olarak çalışacak şekilde yapılandırılmıştır. İşlem hattı yapıtları yayımlama son görevi, bir yayın ardışık düzeni tetikler. Yayın işlem hattı oluşturmak için sonraki bölüme geçin. 

## <a name="configure-continuous-deployment"></a>Sürekli dağıtımı yapılandırma
Bu bölümde, derleme işlem hattı, yapıtlar düştüğünde otomatik olarak çalışacak şekilde yapılandırılmış bir yayın işlem hattı oluşturursunuz ve dağıtım günlüklerini Azure işlem hatlarında gösterilir.

Bu bölümde, iki farklı aşamalar, test dağıtımları için diğeri üretim dağıtımları için oluşturun. 

### <a name="create-test-stage"></a>Test aşaması oluşturma

Yeni işlem hattı oluşturma ve kalite güvencesi (kapsayan QA) dağıtımlar için ilk aşama yapılandırın. 

1. İçinde **yayınlar** sekmesini, **+ yeni işlem hattı**. Veya, yayın işlem hatları zaten varsa, seçin **+ yeni** düğmesini tıklatın ve seçin **+ yeni yayın işlem**.  

    ![Yayın işlem hattı ekleyin](./media/how-to-ci-cd/add-release-pipeline.png)

2. Bir şablon seçin isteyip istemediğiniz sorulduğunda başlamak seçin bir **boş iş**.

    ![Boş bir işlemle Başlat](./media/how-to-ci-cd/start-with-empty-job.png)

3. Yeni yayın işlem hattınızı adlı bir aşama ile başlatır **Aşama 1**. Aşama 1'e Yeniden Adlandır **QA** ve test ortamı olarak değerlendir. Genellikle, sürekli dağıtım işlem hatları, birden çok aşama vardır. Daha fazla DevOps uygulamanıza dayalı oluşturabilirsiniz. Yeniden adlandırıldıktan sonra aşama ayrıntıları penceresini kapatın. 

    ![Test ortamı aşama oluşturun](./media/how-to-ci-cd/QA-env.png)

4. Yayın derleme işlem hattı tarafından yayımlanan derleme yapıtları bağlayın. Tıklayın **Ekle** yapıtları alanında.

   ![Yapıt Ekle](./media/how-to-ci-cd/add-artifacts.png)  
    
5. İçinde **bir yapıt sayfasını ekleme**, kaynak türünü seçin **yapı**. Ardından, projeyi ve oluşturduğunuz derleme işlem hattı seçin. Ardından **Ekle**'yi seçin.

   ![Bir derleme yapıtı Ekle](./media/how-to-ci-cd/add-an-artifact.png)

6. Yapıt tetikleyici açın ve sürekli dağıtım tetikleyicisi etkinleştirmek için iki durumlu düğmeyi seçin. Artık, yeni bir derleme kullanılabilir her zaman yeni bir yayın oluşturulur.

   ![Sürekli dağıtım tetikleyicisi yapılandırın](./media/how-to-ci-cd/add-a-trigger.png)

7. **QA** aşama bir iş ve görevleri sıfır ile yapılandırılmış. İşlem hattı menüden **görevleri** ardından **QA** aşaması.  Bu aşamada görevleri yapılandırmanız için iş ve görev sayısını seçin.

    ![QA görevlerini yapılandırma](./media/how-to-ci-cd/view-stage-tasks.png)

8. Kalite kontrol aşamasında varsayılan görmelisiniz **aracı işi**. Aracı işi ayrıntılarını yapılandırabilirsiniz, ancak dağıtım görevini kullanabilirsiniz platform/küçük harfe duyarlı olduğundan **Hosted VS2017** veya **barındırılan Ubuntu 1604** içinde **aracı havuzu**(veya kendiniz tarafından yönetilen herhangi bir aracı). 

9. Artı işaretini seçin (**+**) bir görev eklemek için. Arama ve ekleme **Azure IOT Edge**. 

    ![QA için görev ekleyin](./media/how-to-ci-cd/add-task-qa.png)

10. Yeni Azure IOT Edge görevini seçin ve aşağıdaki değerleri yapılandırın:

    * **Görünen ad**: Eylem alanını değiştirdiğinde görünen adı otomatik olarak güncelleştirilir. 
    * **Eylem**: Seçmek için açılan listeyi kullanın **IOT Edge cihazına dağıtma**. Eylem değerini değiştirme görev görünen adı eşleşecek şekilde güncelleştirir.
    * **Azure aboneliği**: IOT Hub'ınızı içeren aboneliği seçin.
    * **IOT hub'ı adı**: IOT hub'ınızı seçin. 
    * **Tek/birden çok cihazı seçin**: Bir veya birden çok cihazı için dağıtmak için sürüm ardışık isteyip istemediğinizi seçin. 
      * Tek bir cihaza dağıtırsanız, girin **IOT Edge cihaz Kimliğine**. 
      * Birden çok aygıta dağıtıyorsanız, cihaz belirtin **hedef koşulu**. Hedef koşul, bir IOT hub uç cihazlarına eşleşecek şekilde bir filtredir. Cihaz etiketleri koşul olarak kullanmak istiyorsanız, karşılık gelen etiketleri cihazlarınızı IOT Hub ile cihaz ikizi güncelleştirmeniz gerekiyor. Güncelleştirme **IOT Edge dağıtımı kimliği** ve **IOT Edge dağıtımı öncelikli** Gelişmiş ayarları. Birden çok cihaz için bir dağıtım oluşturma hakkında daha fazla bilgi için bkz. [otomatik dağıtımlar anlayın IOT Edge](module-deployment-monitoring.md).

11. Seçin **Kaydet** yeni yayın ardışık düzeni için yaptığınız değişiklikleri kaydedin. İade için işlem hattı görünümü seçerek **işlem hattı** menüsünde. 

### <a name="create-production-stage"></a>Üretim aşaması oluşturma

İkinci aşamada Üretim dağıtımı için yayın işlem hattınızı oluşturursunuz. 

1. Üretim için ikinci aşama, QA aşama kopyalayarak olun. İmlecinizi QA aşama gelin ve ardından Kopyala düğmesini seçin. 

    ![Aşama kopyalama](./media/how-to-ci-cd/clone-stage.png)

2. Adlı yeni bir aşama seçin **kopyalama, QA**, özelliklerini açın. Aşama adı için değiştirme **PROD**, üretim için. Aşama Özellikler penceresini kapatın. 

3. Üretim aşaması görevleri açmak için seçmeniz **görevleri** işlem hattı Menüsü'nden seçin **PROD** aşaması. 

4. Azure IOT Edge, üretim ortamınız için yapılandırma görevi seçin. Farklı cihaz veya cihaz üretim kümesini hedeflemek istediğiniz dışında dağıtım ayarlarını büyük olasılıkla ve PROD, QA için aynıdır. Cihaz Kimliği alanı ya da üretim cihazlarınız için hedef koşulu ve dağıtım kimlik alanlarını güncelleştirin. 

5. İle kaydedin **Kaydet** düğmesi. Ve ardından **işlem hattı** işlem hattı görünümüne geri dönmek için.
    
6. Derleme yapıtının bu yayın ardışık düzeni şu anda yapılandırılmış yol tetikleyecek **QA** aşama ve ardından **PROD** yeni bir derleme tamamlandığı her seferinde hazırlayın. Ancak, genellikle bazı test çalışmaları QA cihazlarda tümleştirmek istediğiniz ve üretim için dağıtım el ile onaylama. Üretim aşaması için bir onay koşulu oluşturmak için aşağıdaki adımları kullanın:

    1. Açık **dağıtım öncesi koşulları** ayarlar paneli.

        ![Açık dağıtım öncesi koşulları](./media/how-to-ci-cd/pre-deploy-conditions.png)    

    2. İki durumlu **dağıtım öncesi onayları** için koşul **etkin**. Bir veya daha fazla kullanıcı ya da gruplara eklemek **onaylayanlar** alan ve istediğiniz diğer onay ilkelerinizi dilediğiniz gibi özelleştirebilirsiniz. Değişikliklerinizi kaydetmek için dağıtım öncesi koşulları Masası'nı kapatın.
    
       ![Durumlar belirleyin](./media/how-to-ci-cd/set-pre-deployment-conditions.png)


7. Yayın işlem hattınızı Kaydet **Kaydet** düğmesi. 

    
## <a name="verify-iot-edge-cicd-with-the-build-and-release-pipelines"></a>IOT Edge CI/CD ile yapı doğrulayın ve yayın işlem hatları

Bir derleme işi tetiklemek için kaynak kodu deposu için bir işleme için gönderim veya el ile tetiklersiniz. Bu bölümde, çalışır durumda olduğunu test etmek için CI/CD işlem hattını el ile tetiklersiniz. Ardından dağıtımın başarılı olduğunu doğrulayın.

1. Bu makalenin başında oluşturduğunuz derleme işlem hattı gidin. 

2. Seçerek, derleme işlem hattı, bir derleme işi tetikleyebilirsiniz **kuyruk** aşağıdaki ekran görüntüsünde gösterildiği gibi düğmesi.

    ![El ile tetikleme](./media/how-to-ci-cd/manual-trigger.png)

3. İlerleme durumunu izlemek için derleme işi seçin. Derleme işlem hattı başarıyla tamamlanırsa için bir yayın tetiklenir. **QA** aşaması. 

    ![Derleme günlükleri](./media/how-to-ci-cd/build-logs.png)

4. Başarılı dağıtımı **QA** aşama onaylayan bir bildirim tetikler. Modülleri başarıyla cihazda veya QA aşamayla hedeflenen cihazlara dağıttığınız doğrulayın. Ardından, yayın işlem ve üretim aşamasına seçerek Git sürüm için onay vermek için gidin **PROD** düğmesine tıklayıp ardından **Onayla**. 

    ![Onay bekleniyor](./media/how-to-ci-cd/pending-approval.png)

5. Onaylayanın onay verdikten sonra bu değişiklik, dağıtılabilir **PROD**.

## <a name="next-steps"></a>Sonraki adımlar

* IOT Edge dağıtımı anlamak [IOT Edge dağıtımlarını anlama tek tek cihazlarda veya uygun ölçekte](module-deployment-monitoring.md)
* Oluşturmak, güncelleştirmek veya bir dağıtımda silmek için adımlarında yol [dağıtma ve izleme uygun ölçekte IOT Edge modülleri](how-to-deploy-monitor.md).
