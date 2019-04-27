---
title: "Öğretici: Azure DevOps projeleri ile Azure Kubernetes Service'e ASP.NET Core uygulamaları dağıtma"
description: Azure DevOps projeleri, Azure'da kullanmaya başlamak kolaylaştırır. DevOps projeleri ile ASP.NET Core uygulamanızı birkaç Hızlı adımda Azure Kubernetes Service (AKS) ile dağıtabilirsiniz.
ms.author: mlearned
ms.manager: douge
ms.prod: devops
ms.technology: devops-cicd
ms.topic: tutorial
ms.date: 07/09/2018
author: mlearned
monikerRange: vsts
ms.openlocfilehash: 2aa103b36f60a84aaafc47f03a6cf6d5b6b66160
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60554958"
---
# <a name="tutorial-deploy-aspnet-core-apps-to-azure-kubernetes-service-with-azure-devops-projects"></a>Öğretici: Azure DevOps projeleri ile Azure Kubernetes Service'e ASP.NET Core uygulamaları dağıtma

Azure DevOps projeleri, mevcut kodunuzu ve Git deposuna taşıyın veya sürekli tümleştirme (CI) ve azure'a sürekli teslim (CD) işlem hattı oluşturmak için örnek bir uygulama seçin basitleştirilmiş bir deneyim sunar. 

DevOps projeleri ayrıca:
* Azure Kubernetes Service (AKS) gibi Azure kaynaklarını otomatik olarak oluşturur.
* Oluşturur ve bir yayın ardışık düzeni için CI/CD bir derleme ve yayın işlem hattı ayarlar Azure DevOps yapılandırır.
* İzleme için Azure Application Insights kaynağı oluşturur.
* Sağlar [kapsayıcılar için Azure İzleyici](https://docs.microsoft.com/azure/azure-monitor/insights/container-insights-overview) AKS kümesinde kapsayıcı iş yüklerinin performansını izlemeniz gerekir

Bu öğreticide şunları yapacaksınız:

> [!div class="checklist"]
> * AKS için ASP.NET Core uygulaması dağıtmak için DevOps projeleri'ni kullanın
> * Azure DevOps ve Azure aboneliğinin yapılandırın 
> * AKS kümesini inceleme
> * CI işlem hattını inceleme
> * CD işlem hattını inceleme
> * Değişiklikleri Git'e kaydedin ve otomatik olarak Azure'a dağıtın
> * Kaynakları temizleme

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/) aracılığıyla ücretsiz bir abonelik alabilirsiniz.

## <a name="use-devops-projects-to-deploy-an-aspnet-core-app-to-aks"></a>AKS için ASP.NET Core uygulaması dağıtmak için DevOps projeleri'ni kullanın

DevOps projeleri, Azure işlem hatlarında bir CI/CD işlem hattı oluşturur. Yeni bir Azure DevOps kuruluş oluşturun veya mevcut bir kuruluşa kullanın. DevOps projeleri, seçtiğiniz Azure aboneliğinde bir AKS kümesi gibi Azure kaynaklarını da oluşturur.

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Sol bölmede seçin **kaynak Oluştur**.

1. Arama kutusuna **DevOps projeleri**ve ardından **Oluştur**.

    ![DevOps projeleri Panosu](_img/azure-devops-project-github/fullbrowser.png)

1. Seçin **.NET**ve ardından **sonraki**.

1. Altında **uygulama çerçevesini seçin**seçin **ASP.NET Core**.

1. Seçin **Kubernetes hizmeti**ve ardından **sonraki**. 

## <a name="configure-azure-devops-and-an-azure-subscription"></a>Azure DevOps ve Azure aboneliğinin yapılandırın

1. Yeni bir Azure DevOps kuruluş oluşturun veya mevcut bir kuruluşa seçin. 

1. Azure DevOps projeniz için bir ad girin. 

1. Azure aboneliğinizi seçin.

1. Ek Azure yapılandırma ayarları görüntülemek ve AKS kümesi için düğüm sayısını belirlemek için seçmek için **değişiklik**.  
    Bu bölme, türü ve konumu Azure hizmetlerini yapılandırmak için çeşitli seçenekleri görüntüler.
 
1. Azure yapılandırma alanı çıkın ve ardından **Bitti**.  
    Birkaç dakika sonra işlemi tamamlandı. Azure DevOps kuruluşunuzdaki Git deposunda bir örnek ASP.NET Core uygulaması ayarlayın, bir AKS kümesi oluşturulmakta, CI/CD işlem hattı yürütülür ve uygulamanız Azure'a dağıtılır. 

    Azure DevOps projesi Pano, tüm bu tamamlandıktan sonra Azure portalında görüntülenir. DevOps projeleri panoya doğrudan gidebilirsiniz **tüm kaynakları** Azure portalında. 

    Bu pano, Azure DevOps kod deposu, CI/CD işlem hattınızı ve AKS kümenizi görünürlük sağlar. Azure DevOps işlem hattınızı içinde ek CI/CD seçenekleri yapılandırabilirsiniz. Sağ taraftaki seçin **Gözat** çalışan uygulamanızı görüntülemek için.

## <a name="examine-the-aks-cluster"></a>AKS kümesini inceleme

DevOps projeleri'ni keşfedin ve özelleştirebileceğiniz bir AKS kümesi, otomatik olarak yapılandırır. AKS kümesiyle tanımak için aşağıdakileri yapın:

1. DevOps projeleri panosuna gidin.

1. Sağ taraftaki AKS hizmeti seçin.  
    AKS kümesi için bir bölme açılır. Bu görünümden kapsayıcı durumunun izlenmesi, günlük arama ve Kubernetes panosunu açmak gibi çeşitli eylemleri gerçekleştirebilirsiniz.

1. Sağ taraftaki seçin **görünümü Kubernetes panosunu**.  
    İsteğe bağlı olarak, Kubernetes panosunu açmak için adımları izleyin.

## <a name="examine-the-ci-pipeline"></a>CI işlem hattını inceleme

DevOps projeleri, bir CI/CD işlem hattı, Azure DevOps kuruluşunuzda otomatik olarak yapılandırır. İşlem hattını inceleyebilir ve özelleştirebilirsiniz. Kendisiyle tanımak için aşağıdakileri yapın:

1. DevOps projeleri panosuna gidin.

1. DevOps projeleri panonun üst kısmında seçin **derleme işlem hatlarını**.  
    Derleme işlem hattı yeni projeniz için bir tarayıcı sekmesi görüntülenir.

1. İşaret **durumu** alan ve ardından üç nokta (...) seçin.  
    Bir derleme duraklatma ve derleme işlem hattı düzenleme yeni bir derleme kuyruğa alma gibi çeşitli seçenekler bir menü görüntüler.

1. **Düzenle**’yi seçin.

1. Bu bölmede, derleme işlem hattı için çeşitli görevleri inceleyebilirsiniz.  
    Derleme, bağımlılıklarını geri yükleme ve yayımlama depo çıkarır Git getirilirken kaynaklardan dağıtımları için kullanılan gibi çeşitli görevleri gerçekleştirir.

1. Derleme işlem hattı üstünde derleme işlem hattı adı seçin.

1. Bir şeyler daha açıklayıcı, select, derleme işlem hattı adını değiştirmek **Kaydet ve kuyruğa**ve ardından **Kaydet**.

1. Derleme işlem hattı adınızın altında **Geçmiş**’i seçin.  
    Bu bölme bir denetim kaydı derleme için en son değişikliği görüntüler. Azure DevOps derleme işlem hattı için yapılan değişiklikleri izler ve sürümleri karşılaştırmanızı sağlar.

1. **Tetikleyiciler**’i seçin.  
    DevOps projeleri CI tetikleyicisini otomatik olarak oluşturur ve depoya her işleme, yeni bir derleme başlar. İsteğe bağlı olarak, dahil etmek veya dallar CI işleminden hariç tutmak seçim yapabilirsiniz.

1. **Saklama**’yı seçin.  
    Senaryonuza bağlı olarak, saklamak veya belirli bir sayıda derlemeleri kaldırmak için ilkeleri belirtebilirsiniz.

## <a name="examine-the-cd-release-pipeline"></a>CD yayın ardışık düzeni inceleyin

DevOps projeleri, otomatik olarak oluşturur ve Azure DevOps kuruluşunuzdan Azure aboneliğinize dağıtmak için gerekli adımları yapılandırır. Bu adımlar, Azure DevOps, Azure aboneliğiniz için kimlik doğrulaması için bir Azure hizmet bağlantısı yapılandırmayı içerir. Otomasyon, ayrıca Azure'da CD sağlayan bir yayın ardışık düzeni oluşturur. Yayın ardışık düzeni hakkında daha fazla bilgi için aşağıdakileri yapın:

1. Seçin **derleme ve yayın**ve ardından **yayınlar**.  
    DevOps projeleri, azure'da dağıtımlarını yönetmek için bir yayın ardışık düzeni oluşturur.

1. Yayın işlem hattınızı yanındaki üç nokta (...) seçin ve ardından **Düzenle**.  
    Yayın işlem hattı, yayın işlemini tanımlayan bir *işlem hattı* içerir.

1. **Yapıtlar**’ın altında **Bırak**’ı seçin.  
    Önceki adımlarda incelenirken derleme işlem hattı yapıt için kullanılan bir çıktı üretir. 

1. Sağ tarafında **bırak** simgesini seçme **sürekli dağıtım tetikleyicisi**.  
    Bu yayın işlem hattı, her seferinde yeni bir derleme yapıtının kullanılabilir olan bir dağıtımın yürüttüğü etkin bir CD tetikleyicisine sahiptir. İsteğe bağlı olarak, el ile yürütme dağıtımlarınızı gerektirir böylece tetikleyiciyi devre dışı bırakabilirsiniz. 

1. Sağ taraftaki seçin **yayınları görüntüleyebilir** yayınlar geçmişini görüntülemek için.

1. Bir yayın yanındaki üç nokta (...) seçin ve ardından **açık**.  
    Yayın özeti ilişkili iş öğeleri ve test gibi çeşitli menüleri keşfedebilirsiniz.

1. **İşlemeler**'i seçin.  
    Bu görünüm, bu dağıtımla ilişkilendirilmiş kodu yürütmeleri gösterir. Dağıtımlar arasındaki işleme farklılıklarını görmek için yayınları karşılaştırın.

1. **Günlükler**’i seçin.  
    Günlüklerde, dağıtım işlemiyle ilgili yararlı bilgiler bulunur. Bunları, sırasında ve sonrasında dağıtımları görüntüleyebilirsiniz.

## <a name="commit-changes-to-azure-repos-and-automatically-deploy-them-to-azure"></a>Değişiklikleri Azure depolara ve otomatik olarak Azure'a dağıtın 

 > [!NOTE]
 > Aşağıdaki yordam, CI/CD işlem hattı basit metin değişikliği yaparak test eder.

Artık otomatik olarak en son iş sitenize dağıtan bir CI/CD işlem kullanarak uygulamanızı bir ekip ile işbirliği yapmaya hazır. Her değişiklik Git deposu için Azure DevOps bir yapı başlatır ve Azure'a dağıtılacak bir CD işlem hattı yürütür. Bu bölümdeki yordamı izleyin veya değişiklikleri deponuza işlemek için başka bir teknik kullanın. Örneğin, en sevdiğiniz aracı veya IDE Git deposunda kopyalayabilirsiniz ve ardından bu depoya itme değiştirir.

1. Azure DevOps menüde **kod** > **dosyaları**, deponuza gidin.

1. Git *görünümler/giriş* dizin yanındaki üç nokta (...) seçin *Index.cshtml* dosya ve ardından **Düzenle**.

1. Div etiketlerinden birini bazı metinler ekleme gibi bu dosyaya bir değişiklik yapın. 

1. Sağ üst kısımdaki seçin **işleme**ve ardından **işleme** değişikliğiniz yeniden göndermek için.  
    Birkaç dakika sonra Azure DevOps bir yapı başlatır ve değişiklikleri dağıtmak için bir yayın yürütür. DevOps projeleri Panoda veya tarayıcı Azure DevOps kuruluşunuz ile derleme durumunu izleyin.

1. Yayın tamamlandığında, değişikliklerinizi doğrulamak için uygulamanıza yenileyin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Test yapıyorsanız, kaynaklarınızı temizleyerek fatura ücretler tahakkuk önleyebilirsiniz. Artık gerekli değilse, bu öğreticide oluşturduğunuz kaynaklar ve AKS kümesini silebilirsiniz. Bunu yapmak için **Sil** DevOps projeleri Pano işlevselliği.

> [!IMPORTANT]
> Aşağıdaki yordam, kaynakları kalıcı olarak siler. *Sil* işlevselliği, hem Azure hem de Azure DevOps, DevOps projeleri, proje tarafından oluşturulan verileri yok eder ve onu almak mümkün olmayacaktır. Yönergeleri dikkatle yalnızca okuduktan sonra bu yordamı kullanın.

1. Azure portalında DevOps projeleri panoya gidin.
2. Sağ üst kısımdaki seçin **Sil**. 
3. İstemde, seçin **Evet** için *kalıcı olarak silmek* kaynakları.

## <a name="next-steps"></a>Sonraki adımlar

Ekibinizin gereksinimlerine uygun olarak bu derleme ve yayın işlem hatlarını istediğiniz gibi değiştirebilirsiniz. Ayrıca bu CI/CD desenini diğer işlem hatlarınızda şablon olarak kullanabilirsiniz. Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * AKS için ASP.NET Core uygulaması dağıtmak için DevOps projeleri'ni kullanın
> * Azure DevOps ve Azure aboneliğinin yapılandırın 
> * AKS kümesini inceleme
> * CI işlem hattını inceleme
> * CD işlem hattını inceleme
> * Değişiklikleri Git'e kaydedin ve otomatik olarak Azure'a dağıtın
> * Kaynakları temizleme

Kubernetes panosunu kullanma hakkında daha fazla bilgi için bkz:

> [!div class="nextstepaction"]
> [Kubernetes panosunu kullanma](https://docs.microsoft.com/azure/aks/kubernetes-dashboard)
