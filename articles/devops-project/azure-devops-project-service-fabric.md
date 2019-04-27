---
title: "Öğretici: Azure DevOps projeleri'ni kullanarak ASP.NET Core uygulamanızı Azure Service Fabric'e dağıtma"
description: Azure DevOps projeleri, Azure'da kullanmaya başlamak kolaylaştırır. DevOps projeleri ile Azure Service Fabric için birkaç Hızlı adımda azure'da ASP.NET Core uygulamanızı dağıtabilirsiniz.
ms.author: mlearned
ms.manager: douge
ms.prod: devops
ms.technology: devops-cicd
ms.topic: tutorial
ms.date: 07/09/2018
author: mlearned
monikerRange: vsts
ms.openlocfilehash: 8ba217cb9ce849e57b15d3e6cc73529c78bf340e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60554956"
---
# <a name="tutorial-deploy-your-aspnet-core-app-to-azure-service-fabric-by-using-azure-devops-projects"></a>Öğretici: Azure DevOps projeleri'ni kullanarak ASP.NET Core uygulamanızı Azure Service Fabric'e dağıtma

Azure DevOps projeleri, mevcut kodunuzu ve Git deposuna taşıyın veya sürekli tümleştirme (CI) ve azure'a sürekli teslim (CD) işlem hattı oluşturmak için örnek bir uygulama seçin basitleştirilmiş bir deneyim sunar. 

DevOps projeleri ayrıca:
* Azure Service Fabric gibi Azure kaynaklarını otomatik olarak oluşturur.
* Oluşturur ve yayın işlem hattı bir CI/CD işlem hattı ayarlar Azure DevOps yapılandırır.
* İzleme için Azure Application Insights kaynağı oluşturur.

Bu öğreticide şunları yapacaksınız:

> [!div class="checklist"]
> * Bir ASP.NET Core uygulaması oluşturma ve Service Fabric'e dağıtma DevOps projeleri'ni kullanın
> * Azure DevOps ve Azure aboneliğinin yapılandırın 
> * CI işlem hattını inceleme
> * CD işlem hattını inceleme
> * Git’e değişiklikleri işleme ve Azure’a otomatik olarak dağıtma
> * Kaynakları temizleme

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/) aracılığıyla ücretsiz bir abonelik alabilirsiniz.

## <a name="use-devops-projects-to-create-an-aspnet-core-app-and-deploy-it-to-service-fabric"></a>Bir ASP.NET Core uygulaması oluşturma ve Service Fabric'e dağıtma DevOps projeleri'ni kullanın

DevOps projeleri, Azure işlem hatlarında bir CI/CD işlem hattı oluşturur. Yeni bir Azure DevOps kuruluş oluşturun veya mevcut bir kuruluşa kullanın. DevOps projeleri, tercih ettiğiniz bir Azure aboneliği bir Service Fabric kümesi gibi Azure kaynaklarını da oluşturur.

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Sol bölmede seçin **kaynak Oluştur**.

1. Arama kutusuna **DevOps projeleri**ve ardından **Oluştur**.

    ![DevOps projeleri Panosu](_img/azure-devops-project-github/fullbrowser.png)

1. Seçin **.NET**ve ardından **sonraki**.

1. Altında **uygulama çerçevesini seçin**seçin **ASP.NET Core**ve ardından **sonraki**.

1. Seçin **Service Fabric kümesi**ve ardından **sonraki**. 

## <a name="configure-azure-devops-and-an-azure-subscription"></a>Azure DevOps ve Azure aboneliğinin yapılandırın

1. Yeni bir Azure DevOps kuruluş oluşturun veya mevcut bir kuruluşa seçin. 

1. Azure DevOps projeniz için bir ad girin. 

1. Azure aboneliğinizi seçin.

1. Ek Azure yapılandırma ayarlarını görüntülemek ve düğüm sanal makine boyutu ve işletim sistemi için Service Fabric kümesini tanımlamak için seçin **değişiklik**.  
    Bu bölme, türü ve konumu Azure hizmetlerini yapılandırmak için çeşitli seçenekleri görüntüler.
 
1. Azure yapılandırma alanı çıkın ve ardından **Bitti**.  
    Birkaç dakika sonra işlemi tamamlandı. Azure DevOps kuruluşunuzdaki Git deposunda bir örnek ASP.NET Core uygulaması ayarlayın, bir Service Fabric kümesi oluşturulur, CI/CD işlem hattı yürütülür ve uygulamanız Azure'a dağıtılır. 

    DevOps projeleri Pano, tüm bu tamamlandıktan sonra Azure portalında görüntülenir. DevOps projeleri panoya doğrudan gidebilirsiniz **tüm kaynakları** Azure portalında. 

    Bu pano, Azure DevOps kod deposu, CI/CD işlem hattınızı ve Service Fabric kümenizi görünürlük sağlar. Azure depoları, CI/CD işlem hattı için ek seçenekleri yapılandırabilirsiniz. Sağ taraftaki seçin **Gözat** çalışan uygulamanızı görüntülemek için.

## <a name="examine-the-ci-pipeline"></a>CI işlem hattını inceleme

DevOps projeleri, bir CI/CD işlem hattı Azure işlem hatlarında otomatik olarak yapılandırır. İşlem hattını inceleyebilir ve özelleştirebilirsiniz. Kendisiyle tanımak için aşağıdakileri yapın:

1. DevOps projesi panosuna gidin.

1. DevOps projeleri panonun üst kısmında seçin **derleme işlem hatlarını**.  
    Derleme işlem hattı yeni projeniz için bir tarayıcı sekmesi görüntülenir.

1. İşaret **durumu** alan ve ardından üç nokta (...) seçin.  
    Bir derleme duraklatma ve derleme işlem hattı düzenleme yeni bir derleme kuyruğa alma gibi çeşitli seçenekler bir menü görüntüler.

1. **Düzenle**’yi seçin.

1. Bu bölmede, derleme işlem hattı için çeşitli görevleri inceleyebilirsiniz.  
    Derleme, bağımlılıklarını geri yükleme ve yayımlama depo çıkarır Git getirilirken kaynaklardan dağıtımları için kullanılan gibi çeşitli görevleri gerçekleştirir.

1. Derleme işlem hattı üstünde derleme işlem hattı adı seçin. 

1. Derleme işlem hattı adınızın altında **Geçmiş**’i seçin.  
    Bu bölme bir denetim kaydı derleme için en son değişikliği görüntüler. Azure DevOps derleme işlem hattı için yapılan değişiklikleri izler ve sürümleri karşılaştırmanızı sağlar.

1. **Tetikleyiciler**’i seçin.  
    DevOps projeleri CI tetikleyicisini otomatik olarak oluşturur ve depoya her işleme, yeni bir derleme başlar. İsteğe bağlı olarak, dahil etmek veya dallar CI işleminden hariç tutmak seçim yapabilirsiniz.

1. **Saklama**’yı seçin.  
    Senaryonuza bağlı olarak, saklamak veya belirli bir sayıda derlemeleri kaldırmak için ilkeleri belirtebilirsiniz.

## <a name="examine-the-cd-pipeline"></a>CD işlem hattını inceleme

DevOps projeleri, otomatik olarak oluşturur ve Azure DevOps kuruluşunuzdan Azure aboneliğinize dağıtmak için gerekli adımları yapılandırır. Bu adımlar, Azure DevOps, Azure aboneliğiniz için kimlik doğrulaması için bir Azure hizmet bağlantısı yapılandırmayı içerir. Otomasyon, ayrıca Azure'da CD sağlayan bir yayın ardışık düzeni oluşturur. Yayın ardışık düzeni hakkında daha fazla bilgi için aşağıdakileri yapın:

1. Seçin **derleme ve yayın**ve ardından **yayınlar**.  
    DevOps projeleri, azure'da dağıtımlarını yönetmek için bir yayın ardışık düzeni oluşturur.

1. Yayın işlem hattınızı yanındaki üç nokta (...) seçin ve ardından **Düzenle**.  
    Yayın işlem hattı, yayın işlemini tanımlayan bir *işlem hattı* içerir.

1. **Yapıtlar**’ın altında **Bırak**’ı seçin.  
    Denetlenen derleme işlem hattı daha önce yapıt için kullanılan bir çıktı üretir. 

1. Sağ tarafında **bırak** simgesini seçme **sürekli dağıtım tetikleyicisi**.  
    Bu yayın işlem hattı, her seferinde yeni bir derleme yapıtının kullanılabilir olan bir dağıtımın yürüttüğü etkin bir CD tetikleyicisine sahiptir. İsteğe bağlı olarak, el ile yürütme dağıtımlarınızı gerektirir böylece tetikleyiciyi devre dışı bırakabilirsiniz. 

1. Sağ taraftaki seçin **yayınları görüntüleyebilir** yayınlar geçmişini görüntülemek için.

1. Bir yayın yanındaki üç nokta (...) seçin ve ardından **açık**.  
    Yayın özeti ilişkili iş öğeleri ve test gibi çeşitli menüleri keşfedebilirsiniz.

1. **İşlemeler**'i seçin.  
    Bu görünüm, bu dağıtımla ilişkilendirilmiş kodu yürütmeleri gösterir. Dağıtımlar arasındaki işleme farklılıklarını görmek için yayınları karşılaştırın.

1. **Günlükler**’i seçin.  
    Günlüklerde, dağıtım işlemiyle ilgili yararlı bilgiler bulunur. Bunları, sırasında ve sonrasında dağıtımları görüntüleyebilirsiniz.

## <a name="commit-changes-to-git-and-automatically-deploy-them-to-azure"></a>Değişiklikleri Git'e kaydedin ve otomatik olarak Azure'a dağıtın 

 > [!NOTE]
 > Aşağıdaki yordam, CI/CD işlem hattı basit metin değişikliği yaparak test eder.

Artık otomatik olarak en son iş sitenize dağıtan bir CI/CD işlem kullanarak uygulamanızı bir ekip ile işbirliği yapmaya hazır. Bir yapı her değişiklik için Git deposu başlar ve bir yayın değişikliklerinizi Azure'a dağıtır. Bu bölümdeki yordamı izleyin veya değişiklikleri deponuza işlemek için başka bir teknik kullanın. Örneğin, en sevdiğiniz aracı veya IDE Git deposunda kopyalayabilirsiniz ve ardından bu depoya itme değiştirir.

1. Azure DevOps menüde **kod** > **dosyaları**, deponuza gidin.

1. Git *görünümler/giriş* dizin yanındaki üç nokta (...) seçin *Index.cshtml* dosya ve ardından **Düzenle**.

1. Div etiketlerinden birini bazı metinler ekleme gibi bu dosyaya bir değişiklik yapın. 

1. Sağ üst kısımdaki seçin **işleme**ve ardından **işleme** değişikliğiniz yeniden göndermek için.  
    Birkaç dakika sonra bir yapı başlatır ve ardından değişiklikleri dağıtmak için bir yayın yürütür. DevOps projeleri Panoda veya tarayıcı Azure DevOps gerçek zamanlı günlük kaydı ile yapı durumunu izleyebilirsiniz.

1. Yayın tamamlandığında, değişikliklerinizi doğrulamak için uygulamanıza yenileyin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Test yapıyorsanız, kaynaklarınızı temizleyerek fatura ücretler tahakkuk önleyebilirsiniz. Artık gerekli değilse, bu öğreticide oluşturduğunuz kaynaklar ve Azure Service Fabric kümesini silebilirsiniz. Bunu yapmak için **Sil** DevOps projeleri Pano işlevselliği.

> [!IMPORTANT]
> Aşağıdaki yordam, kaynakları kalıcı olarak siler. *Sil* işlevselliği, hem Azure hem de Azure DevOps, DevOps projeleri, proje tarafından oluşturulan verileri yok eder ve onu almak mümkün olmayacaktır. Yönergeleri dikkatle yalnızca okuduktan sonra bu yordamı kullanın.

1. Azure portalında DevOps projeleri panoya gidin.
1. Sağ üst kısımdaki seçin **Sil**. 
1. İstemde, seçin **Evet** için *kalıcı olarak silmek* kaynakları.

## <a name="next-steps"></a>Sonraki adımlar

Ekibinizin gereksinimlerine uygun olarak bu Azure CI/CD işlem hatlarını istediğiniz gibi değiştirebilirsiniz. Ayrıca bu CI/CD desenini diğer işlem hatlarınızda şablon olarak kullanabilirsiniz. Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Bir ASP.NET Core uygulaması oluşturma ve Service Fabric'e dağıtma DevOps projeleri'ni kullanın
> * Azure DevOps ve Azure aboneliğinin yapılandırın 
> * CI işlem hattını inceleme
> * CD işlem hattını inceleme
> * Değişiklikleri Git'e kaydedin ve otomatik olarak Azure'a dağıtın
> * Kaynakları temizleme

Service Fabric ve mikro hizmetler hakkında daha fazla bilgi için bkz:

> [!div class="nextstepaction"]
> [Uygulamaları oluşturmak için bir mikro hizmetler yaklaşımı kullanma](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts)
