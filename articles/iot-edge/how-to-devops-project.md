---
title: Azure DevOps projeleri - Azure IOT Edge ile CI/CD işlem hattı | Microsoft Docs
description: Azure DevOps projeleri, Azure'da kullanmaya başlamak kolaylaştırır. Birkaç Hızlı adımda, tercih ettiğiniz bir Azure IOT Edge uygulama başlatma yardımcı olur.
author: shizn
manager: ''
ms.author: xshi
ms.date: 12/04/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: ebb7e515f9d9205f364d50b3d686c68a2988f86a
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53074223"
---
# <a name="create-a-cicd-pipeline-for-iot-edge-with-azure-devops-projects-preview"></a>Azure DevOps projeleri (Önizleme) ile IOT Edge için CI/CD işlem hattı oluşturma

DevOps projeleri ile sürekli tümleştirme (CI) ve IOT Edge uygulamanız için sürekli teslim (CD) yapılandırın. DevOps projeleri derleme ve yayın işlem hattı Azure işlem hatları, başlangıç yapılandırmasını basitleştirir.

Etkin bir Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free) oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

DevOps projeleri, Azure DevOps bir CI/CD işlem hattı oluşturur. Yeni bir Azure DevOps kuruluş oluşturun veya mevcut bir kuruluşa kullanın. DevOps projeleri, Azure kaynaklarını da tercih ettiğiniz bir Azure aboneliği oluşturur.

1. [Microsoft Azure portalında](https://portal.azure.com) oturum açın.

1. Sol bölmede seçin **kaynak Oluştur** sol gezinti çubuğunda ve ardından arama simgesine **DevOps projeleri**.  

1.  **Oluştur**’u seçin.

## <a name="select-a-sample-application-and-azure-service"></a>Örnek uygulama ve Azure hizmeti seçme

1. Azure IOT Edge modul yazılabilir [ C# ](tutorial-csharp-module.md), [Node.js](tutorial-node-module.md), [Python](tutorial-python-module.md), [C](tutorial-c-module.md) ve [Java](tutorial-java-module.md). Yeni bir uygulamayı başlatmak için tercih ettiğiniz dili seçin. Gelenlere, seçebileceğiniz **.NET**, **Node.js**, **Python**, **C**, veya **Java**ve ardından **Sonraki**.

    ![Yeni bir uygulama oluşturmak için dil seçin](./media/how-to-devops-project/select-language.png)

2. Seçin **basit IOT (Önizleme)** ve ardından **sonraki**.

    ![Basit IOT framework seçin](media/how-to-devops-project/select-iot.png)

3. Seçin **IOT Edge**ve ardından **sonraki**.

    ![IOT Edge hizmeti seçin](media/how-to-devops-project/select-iot-edge.png)

## <a name="configure-azure-devops-and-an-azure-subscription"></a>Azure DevOps ve Azure aboneliğinin yapılandırın

1. Yeni bir ücretsiz Azure DevOps kuruluş oluşturun veya mevcut bir kuruluşa seçin.

    a. Projeniz için bir ad seçin. 

    b. Azure aboneliği ve konumu seçin, uygulamanız için bir ad seçin ve ardından **Bitti**.  

    ![Ad ve uygulama oluşturma](media/how-to-devops-project/select-devops.png)

1. DevOps projeleri Pano, birkaç dakika sonra Azure portalında görüntülenir. Azure DevOps kuruluşunuzdaki bir depodaki bir örnek IOT Edge uygulama ayarlayın, bir derleme yürütülür ve uygulamanızı IOT Edge cihazına dağıtılır. Bu pano, kod deposu, CI/CD işlem hattı ve uygulamanızı azure'da görünürlük sağlar.

    ![DevOps portalında uygulamayı görüntüle](./media/how-to-devops-project/devops-portal.png)


## <a name="commit-code-changes-and-execute-cicd"></a>Kod değişikliklerini işleme ve CI/CD’yi yürütme

DevOps projeleri, Azure depoları veya GitHub Git deposu oluşturuldu. Depo görüntülemek ve uygulamanıza kod değişikliği yapmanız için aşağıdaki adımları uygulayın:

1. DevOps projeleri panosunun sol tarafta, bağlantısını seçin, **ana** dal.  
Bu bağlantı yeni oluşturulan Git deposuna bir görünüm açar.

1. Depo kopya URL'sini görüntülemek için tarayıcının sağ üst kısmından **Kopya**’yı seçin. Git deponuzda VS Code veya diğer tercih edilen araçları kopyalayabilirsiniz. Sonraki birkaç adımda yapmak için web tarayıcısı kullanın ve doğrudan ana dalına işleme kodu değiştirir.

    ![Kopya git deposu](media/how-to-devops-project/clone.png)

1. Tarayıcı sol tarafında, Git **modules/FilterModule/module.json** dosya.

1. Seçin **Düzenle**ve ardından değişiklik `"version"` altında `"tag"`. Örneğin, kendisine güncelleştirebilirsiniz `"version": "${BUILD_BUILDID}"` kullanmak için [Azure DevOps yapı değişkenleri](https://docs.microsoft.com/azure/devops/pipelines/build/variables?view=vsts#build-variables) , Azure IOT Edge modülü resim etiketi bir parçası olarak.

    ![Yapı değişkenleri kabul edecek şekilde sürümü Düzenle](media/how-to-devops-project/update-module-json.png)

1. Seçin **işleme**ve ardından değişikliklerinizi kaydedin.

1. Tarayıcınızda Azure DevOps projesi panoya gidin.  Bir derlemenin sürdüğünü görüyor olmanız gerekir. Yaptığınız değişiklikleri otomatik olarak oluşturulur ve bir CI/CD işlem hattı dağıtılır.

    ![Devam eden durumunu görüntüle](media/how-to-devops-project/ci-cd-in-progress.png)

## <a name="examine-the-cicd-pipeline"></a>CI/CD işlem hattı inceleyin

Önceki adımda, Azure DevOps projeleri, IOT Edge uygulamanız için eksiksiz bir CI/CD işlem hattı otomatik olarak yapılandırılır. İşlem hattını gerektiği şekilde keşfedin ve özelleştirin. Azure DevOps yapıyla hakkında bilgilenmeli ve yayın işlem hatları için aşağıdaki adımları uygulayın.

1. DevOps projeleri panonun üst kısmında seçin **derleme işlem hatlarını**.  
Bu bağlantı, bir tarayıcı sekmesi açar ve Azure DevOps yeni projeniz için işlem hattı oluşturun.

1. **Düzenle**’yi seçin.

    ![Derleme işlem hattı Düzenle](media/how-to-devops-project/click-edit-button.png)

1. Bu bölmede, derleme işlem hattı için çeşitli görevleri inceleyebilirsiniz. Derleme dağıtımları için kullanılan IOT Edge modül görüntüleri oluşturmaya, dağıtmaya IOT Edge modülleri ve kullanılan çıkış yayımlama Git deposundan kaynakları alma gibi çeşitli görevleri gerçekleştirir. CI için Azure IOT Edge görevleri hakkında daha fazla bilgi edinmek için ziyaret edebilirsiniz [sürekli tümleştirme için Azure işlem hatları yapılandırma](https://docs.microsoft.com/azure/iot-edge/how-to-ci-cd#configure-azure-pipelines-for-continuous-integration).

    ![Sürekli Tümleştirme görevlerini görüntüle](media/how-to-devops-project/ci.png)

1. Derleme işlem hattı üstünde derleme işlem hattı adı seçin.

1. Bir şeyler daha açıklayıcı, select, derleme işlem hattı adını değiştirmek **Kaydet ve kuyruğa**ve ardından **Kaydet**.

1. Derleme işlem hattı adınızın altında **Geçmiş**’i seçin.   
İçinde **geçmişi** bölmesinde, bir denetim kaydı yaptığınız son değişiklikler derleme için bkz.  Azure işlem hatları için derleme işlem hattı yapılan değişiklikleri izler ve sürümleri karşılaştırmanızı sağlar.

1. **Tetikleyiciler**’i seçin. DevOps projeleri CI tetikleyicisini otomatik olarak oluşturulan ve depoya her işleme, yeni bir yapı başlatır.  İsteğe bağlı olarak dalları CI işlemine dahil etmeyi veya işlemden hariç tutmayı seçebilirsiniz.

1. **Saklama**’yı seçin. Senaryonuza bağlı olarak, saklamak veya belirli bir sayıda derlemeleri kaldırmak için ilkeleri belirtebilirsiniz.

1. Seçin **yayın** altında **işlem hatları**. DevOps projeleri, Azure IOT Edge dağıtımlarını yönetmek için bir yayın ardışık düzeni oluşturur.

    ![Yayın işlem hattını görüntüle](media/how-to-devops-project/release-pipeline.png)

1. **Düzenle**’yi seçin. Sürüm ardışık yayın işlemini tanımlar. bir işlem hattı içerir.  

1. **Yapıtlar**’ın altında **Bırak**’ı seçin. Önceki adımlarda incelediğiniz derleme işlem hattı, yapıt için kullanılan çıkışı üretir. 

1. Yanındaki **bırak** simgesini seçme **sürekli dağıtım tetikleyicisi**.  
Bu yayın işlem hattı yok her seferinde yeni bir derleme yapıtının kullanılabilir bir dağıtım çalıştığı etkin bir CD tetikleyicisine sahiptir. İsteğe bağlı olarak, el ile yürütme dağıtımlarınızı gerektirir böylece tetikleyiciyi devre dışı bırakabilirsiniz.  

1. Sol tarafta, seçin **görevleri**. Dağıtım işleminizin gerçekleştiren etkinlikler görevlerdir. Bu örnekte, bir görev için Azure IOT Edge modülü görüntülerinizi dağıtmak için oluşturuldu. Azure IOT Edge görevleri hakkında daha fazla bilgi edinmek için CD için ziyaret edebilirsiniz [sürekli dağıtım için Azure işlem hatları yapılandırma](https://docs.microsoft.com/azure/iot-edge/how-to-ci-cd#configure-azure-pipelines-for-continuous-deployment).

    ![Sürekli dağıtım görevlerini görüntüle](media/how-to-devops-project/dev-release.png)

1. Sağ tarafta seçin **yayınları görüntüleyebilir**. Bu görünümde yayın geçmişi gösterilir.

1. Sürümlerinizin birini yanındaki üç nokta (...) seçin ve ardından **açık**.  
Yayın özeti, ilişkili iş öğeleri ve test gibi keşfetmek için birkaç menüleri vardır.

1. **İşlemeler**'i seçin. Bu görünümde, belirli bir dağıtım ile ilişkili kod tamamlama gösterilir. 

1. **Günlükler**’i seçin. Günlüklerde, dağıtım işlemiyle ilgili yararlı bilgiler bulunur. Bunlar hem dağıtım sırasında hem de sonrasında görüntülenebilir.


## <a name="clean-up-resources"></a>Kaynakları temizleme

Azure App Service ve artık gerekmediğinde, oluşturduğunuz ilgili diğer kaynakları silebilirsiniz. Kullanım **Sil** DevOps projeleri Pano işlevselliği.

## <a name="next-steps"></a>Sonraki adımlar
* Azure IOT Edge üzerinde Azure DevOps için görevleri öğrenin [sürekli tümleştirme ve sürekli dağıtım için Azure IOT Edge](how-to-ci-cd.md)
* IOT Edge dağıtımı anlamak [IOT Edge dağıtımlarını anlama tek tek cihazlarda veya uygun ölçekte](module-deployment-monitoring.md)
* Oluşturmak, güncelleştirmek veya bir dağıtımda silmek için adımlarında yol [dağıtma ve izleme uygun ölçekte IOT Edge modülleri](how-to-deploy-monitor.md).
