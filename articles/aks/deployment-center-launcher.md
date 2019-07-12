---
title: Azure App Service'te dağıtım Merkezi'ni kullanın
description: Azure DevOps Dağıtım Merkezi'nde uygulamanız için sağlam bir Azure DevOps işlem hattı ayarlamayı basitleştirir.
ms.author: puagarw
ms.manager: douge
ms.prod: devops
ms.technology: devops-cicd
ms.topic: tutorial
ms.date: 07/12/2019
author: pulkitaggarwl
monikerRange: vsts
ms.openlocfilehash: 8d1e467906b74c97c8b4f4e5c14af0814dd098f7
ms.sourcegitcommit: 470041c681719df2d4ee9b81c9be6104befffcea
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2019
ms.locfileid: "67854641"
---
# <a name="deployment-center-launcher"></a>Dağıtım Merkezi Başlatıcısı

Azure DevOps Dağıtım Merkezi'nde uygulamanız için sağlam bir DevOps işlem hattı, yedekleme ayarı basitleştirir. Varsayılan olarak, bir DevOps işlem hattı, uygulama güncelleştirmelerini kubernetes kümesine dağıtmak için yapılandırır. Varsayılan yapılandırılmış DevOps işlem hattı ve daha zengin DevOps özellikleri - dağıtma, ek Azure kaynaklarını hazırlama, betikleri çalıştırma, uygulamanızı yükseltmek veya hatta ek doğrulama testleri çalıştırma önce bir onayları Ekle genişletebilirsiniz.

Bu öğreticide şunları yapacaksınız:

> [!div class="checklist"]
> * K8s kümeye, uygulama güncelleştirmeleri dağıtmak için bir DevOps işlem hattı yapılandırın
> * CI işlem hattını inceleme
> * CD işlem hattını inceleme
> * Kaynakları temizleme

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Aracılığıyla ücretsiz edinebilirsiniz [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/)

* Azure Kubernetes Service (AKS) kümesi

## <a name="create-aks-cluster"></a>AKS kümesi oluşturma

1. Oturum açma için sizin [Azure portalı](https://portal.azure.com/)

1. Seçin [Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) düğmesi Azure portalının sağ üst köşesinde yer alan menüdeki.

1. AKS kümesi oluşturmak için aşağıdaki komutları çalıştırın.

    ```cmd
    # The below command creates Resource Group in the south india location

    az group create --name azooaks --location southindia

    # The below command creates a cluster named azookubectl with one node. 

    az aks create --resource-group azooaks --name azookubectl --node-count 1 --enable-addons monitoring --generate-ssh-keys
    ```

## <a name="deploy-application-updates-to-k8s-cluster"></a>Uygulama güncelleştirmeleri K8s kümesine dağıtma

1. Yukarıdaki için oluşturulan kaynak grubuna gidin.

1. AKS kümesi seçin ve Sol dikey penceredeki ayarlar altında tıklayarak **Dağıtım Merkezi (Önizleme)** . Tıklayarak **başlama**.

   ![ayarlar](media/deployment-center-launcher/settings.png)

1. Kodun konumu seçin ve tıklayın **sonraki**. Şu anda desteklenen depoları olan **[Azure depoları](https://docs.microsoft.com/azure/devops/repos/index?view=azure-devops)** ve **GitHub**. Depo seçime göre aşağıdaki adımları izleyebilirsiniz.

    Azure depoları, kodunuzu yönetmek için kullanabileceğiniz bir sürüm denetimi araçları kümesidir. Yazılım projeniz büyük ya da küçük olsun, sürüm denetimi olabildiğince çabuk kullanmak iyi bir fikirdir.

    - **Azure depoları**: Bir depo, varolan projenin ve kuruluş seçin.

        ![Azure Repos](media/deployment-center-launcher/azure-repos.gif)

    - **GitHub**: Yetkilendirme ve GitHub hesabınız için depoyu seçin.

        ![GitHub](media/deployment-center-launcher/github.gif)


1. Depo çözümlemek ve Dockerfile algılamak için ekleyeceğiz. Güncelleştirmek istiyorsanız, belirtilen bağlantı noktası numarasını düzenleyebilirsiniz.

    ![Uygulama ayarları](media/deployment-center-launcher/application-settings.png)

    Dockerfile depo içermiyorsa, sistemin bir işleme için bir ileti görüntülenir. 

    ![Dockerfile](media/deployment-center-launcher/dockerfile.png)

1. Seçin veya mevcut bir kapsayıcı kayıt defteri oluşturmak ve tıklayarak **son**. İşlem hattı otomatik olarak oluşturulur ve bir yapı içinde sıra [Azure işlem hatları](https://docs.microsoft.com/azure/devops/pipelines/index?view=azure-devops).

    Azure işlem hatları, otomatik olarak oluşturun ve kod projenizi test ve diğer kullanıcılar için kullanılabilir hale getirmek için kullanabileceğiniz bir bulut hizmetidir. Azure işlem hatları, sürekli tümleştirme (CI) ve sürekli teslim (CD) sürekli olarak ve tutarlı bir şekilde test kodunuzu ve herhangi bir hedef sevk birleştirir.

    ![Container Kayıt Defteri](media/deployment-center-launcher/container-registry.png)

1. Devam eden işlem hattı görmek için bağlantıya tıklayın.

1. Dağıtım tamamlandıktan sonra gösterildiği gibi başarılı günlüklerini görürsünüz.

    ![Günlükler](media/deployment-center-launcher/logs.png)

## <a name="examine-the-ci-pipeline"></a>CI işlem hattını inceleme

Dağıtım Merkezi yapılandırır, Azure DevOps kuruluşunuzun CI / CD işlem hattı otomatik olarak. İşlem hattı incelediniz ve özelleştirilebilir. 

1. Dağıtım Merkezi panosuna gidin.  

1. Projeniz için derleme işlem hattı görüntülemek için başarılı günlükleri, listeden derleme sayısına tıklayın. 

1. Sağ üst köşesinden ellipsis(...) tıklayın. Bir menü, yeni bir yapıyı kuyruğa yapı koruma ve derleme işlem hattı düzenleme gibi çeşitli seçenekler gösterilmektedir. Seçin **düzenleme işlem hattı**. 

1. Bu bölmede, derleme işlem hattı için farklı görevlerle inceleyebilirsiniz. Yapı, Git deposundan kaynakları toplanıyor, görüntü oluşturma, kapsayıcı kayıt defterine görüntü gönderme ve dağıtımları için kullanılan çıkış yayımlama gibi çeşitli görevleri gerçekleştirir.

1. Derleme işlem hattı üst kısmındaki derleme işlem hattı adı seçin.

1. Derleme işlem hattı adınız bir şeyle daha açıklayıcı seçeneğini değiştirmek **Kaydet ve kuyruğa**ve ardından **Kaydet**.

1. Seçin **geçmişi** derleme işlem hattınızı altında. Bu bölme bir denetim kaydı en son derleme değişikliklerinizin gösterir. Azure DevOps derleme işlem hattı için yapılan değişiklikleri izler ve sürümleri karşılaştırmanızı sağlar.

1. Seçin **Tetikleyicileri**. İsteğe bağlı olarak, dallar eklenebilen veya CI işleminden hariç tutuldu.

1. Seçin **bekletme**. Tutun veya yapılar senaryonuza bağlı olarak bir dizi kaldırma ilkeleri belirtebilirsiniz.

## <a name="examine-the-cd-pipeline"></a>CD işlem hattını inceleme

Dağıtım Merkezi oluşturur ve Azure aboneliğinizde, Azure DevOps kuruluşunuzdan gerekli adımları otomatik olarak yapılandırır. Bu adımlar, Azure aboneliğinizin Azure DevOps ile kimlik doğrulaması için bir Azure hizmet bağlantısı kurma içerir. Otomasyon, ayrıca Azure'da CD sağlayan bir yayın ardışık düzeni oluşturur.

1. Seçin **işlem hatları**ve ardından **yayınlar**.

1. Yayın ardışık düzeni düzenlemek için tıklayın **Düzenle** .

1. Seçin **bırak** gelen **Yapıtları**. Önceki adımlarda incelenir, yapı işlem hattı yapıt için kullanılan bir çıktı üretir. 

1. Seçin **sürekli dağıtım** sağ tarafındaki tetikleyici **bırak** simgesi. Bu yayın ardışık düzeni, yeni bir derleme yapıtının kullanılabilir olduğunda, bir dağıtım çalışan etkin bir CD tetikleyicisine sahiptir. İsteğe bağlı olarak, el ile yürütme dağıtımlarınız için zorunlu tutmak için tetikleyiciyi devre dışı bırakabilirsiniz.

1. Tüm görevler için işlem hattınızı incelemek için tıklayın **görevleri**. Yayın tiller ortamını ayarlar, imagePullSecrets yapılandırır, Helm araçları yükler ve Helm grafikleri K8s kümeye dağıtır.

1. Yayınları geçmişini görüntülemek için tıklayın **yayınları görüntüleyebilir**. 

1. Özeti görmek için tıklayın **yayın**. Herhangi bir sürüm summary, gibi birden çok menü keşfetmek için aşama ilişkili iş öğeleri ve testleri tıklayın. 

1. **İşlemeler**'i seçin. Bu görünüm, bu dağıtımla ilgili kod tamamlama gösterir. Dağıtımlar arasında işleme farkları görmek için yayınları karşılaştırın.

1. **Günlükler**’i seçin. Günlükleri yararlı dağıtım bilgilerini içerir. Sırasında ve sonrasında dağıtımlar bunları görüntüleyebilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde, oluşturduğunuz ilgili kaynakları silebilirsiniz. DevOps projeleri Panoda Delete işlevlerini kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Ekibinizin gereksinimlerine uygun olarak bu derleme ve yayın işlem hatlarını istediğiniz gibi değiştirebilirsiniz. Ayrıca bu CI/CD desenini diğer işlem hatlarınızda şablon olarak kullanabilirsiniz. Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * K8s kümeye, uygulama güncelleştirmeleri dağıtmak için bir DevOps işlem hattı yapılandırın
> * CI işlem hattını inceleme
> * CD işlem hattını inceleme
> * Kaynakları temizleme
