---
title: Azure DevOps projeleri - Azure IOT Edge ile CI/CD işlem hattı | Microsoft Docs
description: Azure DevOps projeleri, Azure'da kullanmaya başlamak kolaylaştırır. Birkaç Hızlı adımda, tercih ettiğiniz bir Azure IOT Edge uygulama başlatma yardımcı olur.
author: shizn
manager: ''
ms.author: xshi
ms.date: 01/22/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 02977032c5975de4098600ddbebccfcbb9b0fafd
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60595503"
---
# <a name="create-a-cicd-pipeline-for-iot-edge-with-azure-devops-projects-preview"></a>Azure DevOps projeleri (Önizleme) ile IOT Edge için CI/CD işlem hattı oluşturma

DevOps projeleri ile sürekli tümleştirme (CI) ve IOT Edge uygulamanız için sürekli teslim (CD) yapılandırın. DevOps projeleri derleme ve yayın işlem hattı Azure işlem hatları, başlangıç yapılandırmasını basitleştirir.

Etkin bir Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free) oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

DevOps projeleri, Azure DevOps bir CI/CD işlem hattı oluşturur. Yeni bir Azure DevOps kuruluş oluşturun veya mevcut bir kuruluşa kullanın. DevOps projeleri, Azure kaynaklarını da tercih ettiğiniz bir Azure aboneliği oluşturur.

1. [Microsoft Azure portalında](https://portal.azure.com) oturum açın.

1. Sol bölmede seçin **kaynak Oluştur**ve ardından arama **DevOps projeleri**.  

1.  **Oluştur**’u seçin.

## <a name="create-a-new-application-pipeline"></a>Yeni uygulama işlem hattı oluşturma 

1. Azure IOT Edge modul yazılabilir [ C# ](tutorial-csharp-module.md), [Node.js](tutorial-node-module.md), [Python](tutorial-python-module.md), [C](tutorial-c-module.md) ve [Java](tutorial-java-module.md). Yeni bir uygulamayı başlatmak için tercih ettiğiniz dili seçin: **.NET**, **Node.js**, **Python**, **C**, veya **Java**. Devam etmek için **İleri**’yi seçin.

   ![Yeni bir uygulama oluşturmak için dil seçin](./media/how-to-devops-project/select-language.png)

2. Seçin **basit IOT (Önizleme)** uygulama çerçevesi ve ardından olarak **sonraki**.

   ![Basit IOT framework seçin](media/how-to-devops-project/select-iot.png)

3. Seçin **IOT Edge** uygulamanızı dağıtır ve ardından Azure hizmeti olarak **sonraki**.

   ![IOT Edge hizmeti seçin](media/how-to-devops-project/select-iot-edge.png)

4. Yeni bir ücretsiz Azure DevOps kuruluş oluşturun veya mevcut bir kuruluşa seçin.

   1. Projeniz için bir ad sağlayın. 

   2. Azure DevOps kuruluşunuzu seçin. Mevcut bir kuruluşa yoksa seçin **ek ayarlar** yeni bir tane de oluşturabilirsiniz. 

   3. Azure aboneliğinizi seçin.

   4. Projenizin adına göre oluşturulan IOT Hub adını kullanın veya kendi sağlayın.

   5. Seçin **ek ayarlar** DevOps projeleri, sizin adınıza oluşturan Azure kaynaklarını yapılandırmak için.

   6. Seçin **Bitti** projenizi oluşturmayı tamamlayın. 

   ![Ad ve uygulama oluşturma](media/how-to-devops-project/select-devops.png)

DevOps projeleri Pano, birkaç dakika sonra Azure portalında görüntülenir. İlerleme durumunu görmek için proje adını seçin. Sayfayı yenilemeniz gerekebilir. Azure DevOps kuruluşunuzdaki bir depodaki bir örnek IOT Edge uygulama ayarlayın, bir derleme yürütülür ve uygulamanızı IOT Edge cihazına dağıtılır. Bu pano, kod deposu, CI/CD işlem hattı ve uygulamanızı azure'da görünürlük sağlar.

   ![DevOps portalında uygulamayı görüntüle](./media/how-to-devops-project/devops-portal.png)


## <a name="commit-code-changes-and-execute-cicd"></a>Kod değişikliklerini işleme ve CI/CD’yi yürütme

DevOps projeleri, Azure depolarda projeniz için bir Git deposu oluşturuldu. Bu bölümde, depo görüntülemek ve uygulamanızda kod değişikliği yapın.

1. Projeniz için oluşturulan depoya gitmek için **depoları** proje panosunun menüsünde.  

   ![Azure depolarda oluşturulan görünümü depo](./media/how-to-devops-project/view-repositories.png)

2. Aşağıdaki adımlarda, kod değişiklik yapmak için web tarayıcısını kullanarak aracılığıyla yol. Bunun yerine yerel deponuzu kopyalamak isteyip istemediğinizi seçin **kopya** üstten sağ. Git deponuzu Visual Studio Code veya tercih ettiğiniz geliştirme aracınızı kopyalamak için sağlanan URL kullanın. 

3. Depo zaten adlı bir modül için kod içeren **SampleModule** oluşturma işleminde seçtiğiniz uygulama diline bağlı olarak. Açık **modules/SampleModule/module.json** dosya.

   ![Azure depolarındaki açık module.json dosyası](./media/how-to-devops-project/open-module-json.png)

4. Seçin **Düzenle**ve ardından değişiklik `"version"` altında `"tag"`. Örneğin, kendisine güncelleştirebilirsiniz `"version": "${BUILD_BUILDID}"` kullanmak için [Azure DevOps yapı değişkenleri](https://docs.microsoft.com/azure/devops/pipelines/build/variables?view=vsts#build-variables) , Azure IOT Edge modülü resim etiketi bir parçası olarak.

   ![Yapı değişkenleri kabul edecek şekilde sürümü Düzenle](media/how-to-devops-project/update-module-json.png)

5. Seçin **işleme**ve ardından değişikliklerinizi kaydedin.

6. Tarayıcınızda, Azure portalında DevOps projeleri panonuza geri dönün. Bir derlemenin sürdüğünü görüyor olmanız gerekir. Yaptığınız değişiklikleri otomatik olarak oluşturulur ve bir CI/CD işlem hattı dağıtılır.

    ![Devam eden durumunu görüntüle](media/how-to-devops-project/ci-cd-in-progress.png)

## <a name="examine-the-cicd-pipeline"></a>CI/CD işlem hattı inceleyin

Azure DevOps projeleri, önceki bölümlerde, IOT Edge uygulamanız için eksiksiz bir CI/CD işlem hattı otomatik olarak yapılandırılır. Ardından bu derleme işlem hattı değişiklikleri dosyalarından birini yürüterek test. Şimdi keşfedin ve işlem hattı gerektiği gibi özelleştirin. Azure DevOps yapıyla hakkında bilgilenmeli ve yayın işlem hatları için aşağıdaki adımları uygulayın.

1. DevOps projenizi derleme işlem hatlarını görüntülemek için seçin **derleme işlem hatlarını** proje panosunun menüsünde. Bu bağlantı, bir tarayıcı sekmesi açar ve Azure DevOps yeni projeniz için işlem hattı oluşturun.

   ![Azure işlem hatları hatlarında yapı görüntüle](./media/how-to-devops-project/view-build-pipelines.png)

2. **Düzenle**’yi seçin.

    ![Derleme işlem hattı Düzenle](media/how-to-devops-project/click-edit-button.png)

3. Açılan bölmede, derleme işlem hattı çalıştığında oluşan görevleri inceleyebilirsiniz. Derleme işlem hattı Git deposundan kaynakları alma gibi çeşitli görevleri gerçekleştirir, IOT Edge modül görüntüleri oluşturmaya, IOT Edge modülleri, bir kapsayıcı kayıt defterine gönderme ve yayımlama çıktısını alır, dağıtımları için kullanılır. Azure DevOps Azure IOT Edge görevleri hakkında daha fazla bilgi için bkz: [sürekli tümleştirme için Azure işlem hatları yapılandırma](how-to-ci-cd.md#configure-continuous-integration).

4. Seçin **işlem hattı** derleme işlem hattı, işlem hattı ayrıntılarını açmak için üst kısmındaki üst bilgisi. Derleme işlem hattınızı adını daha açıklayıcı bir şeyle değiştirin.

   ![İşlem hattı ayrıntılarını Düzenle](./media/how-to-devops-project/edit-build-pipeline.png)

5. Seçin **Kaydet ve kuyruğa**ve ardından **Kaydet**.

6. Derleme işlem hattı menüsünü **Tetikleyicileri** menüsünde. DevOps projeleri CI tetikleyicisini otomatik olarak oluşturulan ve depoya her işleme, yeni bir yapı başlatır.  İsteğe bağlı olarak dalları CI işlemine dahil etmeyi veya işlemden hariç tutmayı seçebilirsiniz.

7. **Saklama**’yı seçin. Senaryonuza bağlı olarak, saklamak veya belirli bir sayıda derlemeleri kaldırmak için ilkeleri belirtebilirsiniz.

8. Seçin **geçmişi**. Bir denetim kaydı yapı son değişikliklerin geçmişi paneli içerir. Azure işlem hatları için derleme işlem hattı yapılan değişiklikleri izler ve sürümleri karşılaştırmanızı sağlar.

9. Bitirdiğinizde derleme işlem hattı keşfetmeye karşılık gelen yayın işlem hattına gidin. Seçin **yayınlar** altında **işlem hatları**, ardından **Düzenle** işlem hattı ayrıntılarını görüntülemek için.

    ![Yayın işlem hattını görüntüle](media/how-to-devops-project/release-pipeline.png)

10. **Yapıtlar**’ın altında **Bırak**’ı seçin. Bu yapıt izleyen önceki adımlarda incelenirken derleme işlem hattı çıktısını kaynağıdır. 

11. Yanındaki **bırak** simgesini seçme **sürekli dağıtım tetikleyicisi** bir Şimşek gibi görünüyor. Bu yayın ardışık düzeni, bir dağıtım var. her seferinde yeni bir derleme yapıtının kullanılabilir çalıştığı tetikleyici etkinleştirdi. İsteğe bağlı olarak, el ile yürütme dağıtımlarınızı gerektirir böylece tetikleyiciyi devre dışı bırakabilirsiniz.  

12. Yayın işlem hattınızı menüsünü **görevleri** seçin **geliştirme** aşağı açılan listeden aşaması. DevOps projeleri, bir IOT hub'ı oluşturan, bir IOT Edge cihazı, hub'ında oluşturur, derleme işlem hattı örnek modülünden dağıtır ve IOT Edge cihazınız olarak çalıştırmak için bir sanal makine sağlar, için bir yayın aşama oluşturulur. CD için Azure IOT Edge görevleri hakkında daha fazla bilgi edinmek için [sürekli dağıtım için Azure işlem hatları yapılandırma](how-to-ci-cd.md#configure-continuous-deployment).

    ![Sürekli dağıtım görevlerini görüntüle](media/how-to-devops-project/dev-release.png)

13. Sağ tarafta seçin **yayınları görüntüleyebilir**. Bu görünümde yayın geçmişi gösterilir.

14. Daha fazla bilgi görüntülemek için bir yayın adı seçin.


## <a name="clean-up-resources"></a>Kaynakları temizleme

Azure App Service ve artık gerekmediğinde, oluşturduğunuz ilgili diğer kaynakları silebilirsiniz. Kullanım **Sil** DevOps projeleri Pano işlevselliği.

## <a name="next-steps"></a>Sonraki adımlar
* Azure IOT Edge üzerinde Azure DevOps için görevleri öğrenin [sürekli tümleştirme ve sürekli dağıtım için Azure IOT Edge](how-to-ci-cd.md)
* IOT Edge dağıtımı anlamak [IOT Edge dağıtımlarını anlama tek tek cihazlarda veya uygun ölçekte](module-deployment-monitoring.md)
* Oluşturmak, güncelleştirmek veya bir dağıtımda silmek için adımlarında yol [dağıtma ve izleme uygun ölçekte IOT Edge modülleri](how-to-deploy-monitor.md).
