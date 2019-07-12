---
title: 'Öğretici: Azure DevOps projeleri ile Azure Cosmos DB tarafından desteklenen Node.js uygulamaları dağıtma'
description: Azure DevOps projeleri, Azure'da kullanmaya başlamak kolaylaştırır. DevOps projeleri ile birkaç Hızlı adımda azure'da güçlendirilmiş Windows Web uygulaması için Azure Cosmos DB Node.js uygulamanızın dağıtabilirsiniz.
ms.author: mlearned
ms.manager: douge
ms.prod: devops
ms.technology: devops-cicd
ms.topic: tutorial
ms.date: 07/11/2019
author: mlearned
monikerRange: vsts
ms.openlocfilehash: 4310807423600b96078ee48a04a5ad6dab68cd7e
ms.sourcegitcommit: af31deded9b5836057e29b688b994b6c2890aa79
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67813069"
---
# <a name="deploy-nodejs-apps-powered-by-azure-cosmos-db-with-devops-projects"></a>DevOps projeleri ile Azure Cosmos DB tarafından desteklenen Node.js uygulamaları dağıtma

Azure DevOps projeleri, mevcut kodunuzu ve Git deposuna taşıyın veya sürekli tümleştirme (CI) ve azure'a sürekli teslim (CD) işlem hattı oluşturmak için örnek bir uygulama seçin kolaylaştırılmış bir deneyim sunar.

DevOps projeleri ayrıca:

* Azure kaynaklarını otomatik olarak Azure Cosmos DB, Application Insights, App Service ve App Service gibi oluşturun.

* Oluşturur ve yayın işlem hattı, Azure DevOps için CI/CD yapılandırır.

Bu öğreticide şunları yapacaksınız:

> [!div class="checklist"]
> * Azure Cosmos DB tarafından desteklenen bir Node.js uygulamasını dağıtmak için DevOps projeleri'ni kullanın
> * Azure DevOps ve Azure aboneliğinin yapılandırın
> * Azure Cosmos DB inceleyin
> * CI işlem hattını inceleme
> * CD işlem hattını inceleme
> * Değişiklikleri Git'e kaydedin ve otomatik olarak Azure'a dağıtın
> * Kaynakları temizleme

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Aracılığıyla ücretsiz edinebilirsiniz [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/)

## <a name="use-devops-projects-to-deploy-nodejs-app"></a>Node.js uygulamasını dağıtmak için DevOps projeleri'ni kullanın

DevOps projeleri, Azure işlem hatlarında bir CI/CD işlem hattı oluşturur. Yeni bir Azure DevOps kuruluş oluşturun veya mevcut bir kuruluşa kullanın. DevOps projeleri, tercih ettiğiniz bir Azure aboneliği, Azure Cosmos DB, Application Insights, App Service ve App Service planı gibi Azure kaynaklarının da oluşturur.

1. [Azure portalda](https://portal.azure.com) oturum açma

1. Sol bölmede bölümü seçin **kaynak Oluştur**.

1. Arama kutusuna **DevOps projeleri**ve ardından **Ekle**.

   ![DevOps Projeleri](_img/azure-devops-project-cosmos-db/devops-project.png)

1. Seçin **Node.js** çalışma zamanı ve ardından olarak **sonraki**. Altında **uygulama çerçevesini seçin**seçin **Express.js**.

1. Bölüm etkinleştirme **bir veritabanı Ekle** için **Cosmos DB** tıklayın **sonraki**.

    ![Veritabanı Ekle](_img/azure-devops-project-cosmos-db/add-database.png)

    Cosmos DB gibi çeşitli uygulama çerçeveleri destekler **Express.js**, **örnek Node.js uygulamasını**, ve **Sail.js**. Bu öğreticide göz önünde bulundurun sağlar **Express.js**.

1. Uygulamayı dağıtmak için bir Azure hizmeti seçin. Kapsayıcılar için Windows Web uygulaması, Kubernetes hizmeti ve Web uygulaması gibi farklı hizmetler var. Bu öğreticide, kullanacağız **Windows Web uygulama**. Tıklayarak **sonraki**.

## <a name="configure-azure-devops-and-azure-subscription"></a>Azure DevOps ve Azure aboneliği yapılandırma

1. Azure DevOps projeniz için bir ad girin.

1. Yeni bir Azure DevOps kuruluş oluşturun veya mevcut bir kuruluşa seçin.

1. Azure aboneliğinizi seçin.

1. Ek Azure yapılandırma ayarlarını görüntülemek ve fiyatlandırma katmanı ve konumunu tanımlamak için ek ayarları'nı tıklatın. Bu bölme fiyatlandırma katmanı konumunu ve Azure hizmetlerini yapılandırmak için çeşitli seçenekleri görüntüler.

1. Azure yapılandırma alanı çıkın ve ardından **Bitti**.

1. Birkaç dakika sonra işlemi tamamlandı. Azure DevOps kuruluşunuzdaki Git deposunda bir örnek Node.js uygulamasını ayarlayın, bir Azure Cosmos DB, App Service, App Service planı ve Application Insights oluşturulur, CI/CD işlem hattı yürütülür ve uygulamanız Azure'a dağıtılır.

   Azure DevOps projesi Pano, tüm bu tamamlandıktan sonra Azure portalında görüntülenir. DevOps projeleri panoya doğrudan gidebilirsiniz **tüm kaynakları** Azure portalında.

   Bu pano, Azure DevOps kod deposu, CI/CD işlem hattı ve Azure Cosmos DB görünürlük sağlar. Azure DevOps işlem hattınızı içinde ek CI/CD seçenekleri yapılandırabilirsiniz. Sağ taraftaki seçin **Azure Cosmos DB** görüntülemek için.

## <a name="examine-the-azure-cosmos-db"></a>Azure Cosmos DB inceleyin

DevOps projeleri'ni keşfedin ve özelleştirebileceğiniz, Cosmos DB, otomatik olarak yapılandırır. Cosmos DB ile kendinizi alıştırın için aşağıdakileri yapın:

1. DevOps projeleri panosuna gidin.

    ![DevOps projeleri Panosu](_img/azure-devops-project-cosmos-db/devops-project-dashboard.png)

1. Sağ tarafta, Cosmos DB seçin. Cosmos DB için bir bölme açılır. Bu görünümden, izleme ve günlük arama işlemleri gibi çeşitli eylemler gerçekleştirebilirsiniz.

    ![İşlev Uygulaması](_img/azure-devops-project-cosmos-db/cosmos-db.png)

## <a name="examine-the-ci-pipeline"></a>CI işlem hattını inceleme

DevOps projeleri, bir CI/CD işlem hattı, Azure DevOps kuruluşunuzda otomatik olarak yapılandırır. İşlem hattını inceleyebilir ve özelleştirebilirsiniz. Kendisiyle tanımak için aşağıdakileri yapın:

1. DevOps projeleri panosuna gidin.

1. Altında köprüye tıklayın **yapı**. Derleme işlem hattı yeni projeniz için bir tarayıcı sekmesi görüntülenir.

    ![Oluşturma](_img/azure-devops-project-cosmos-db/build.png)

1. **Düzenle**’yi seçin. Bu bölmede, derleme işlem hattı için çeşitli görevleri inceleyebilirsiniz. Birim testleri çalıştırma ve yayımlama uygulaması oluşturma, Git deposundan getirilirken kaynak kodu, dağıtımları için kullanılan çıkarır gibi yapı çeşitli görevleri gerçekleştirir.

1. **Tetikleyiciler**’i seçin. DevOps projeleri CI tetikleyicisini otomatik olarak oluşturur ve depoya her işleme, yeni bir derleme başlar. İsteğe bağlı olarak, dahil etmek veya dallar CI işleminden hariç tutmak seçim yapabilirsiniz.

1. **Saklama**’yı seçin. Senaryonuza bağlı olarak, saklamak veya belirli bir sayıda derlemeleri kaldırmak için ilkeleri belirtebilirsiniz.

1. Derleme işlem hattı üstünde derleme işlem hattı adı seçin.

1. Derleme işlem hattınızı adını daha açıklayıcı bir şeyle değiştirin ve ardından **Kaydet** gelen **Kaydet ve kuyruğa** açılır.

1. Derleme işlem hattı adınızın altında **Geçmiş**’i seçin. Bu bölme bir denetim kaydı derleme için en son değişikliği görüntüler. Azure DevOps derleme işlem hattı için yapılan değişiklikleri izler ve sürümleri karşılaştırmanızı sağlar.

## <a name="examine-the-cd-release-pipeline"></a>CD yayın ardışık düzeni inceleyin

DevOps projeleri, otomatik olarak oluşturur ve Azure DevOps kuruluşunuzdan Azure aboneliğinize dağıtmak için gerekli adımları yapılandırır. Bu adımlar, Azure DevOps, Azure aboneliğiniz için kimlik doğrulaması için bir Azure hizmet bağlantısı yapılandırmayı içerir. Otomasyon, ayrıca Azure'da CD sağlayan bir yayın ardışık düzeni oluşturur. Yayın ardışık düzeni hakkında daha fazla bilgi için aşağıdakileri yapın:

1. Gidin **işlem hatları | Yayınları**.

1. Tıklayarak **Düzenle**.

1. **Yapıtlar**’ın altında **Bırak**’ı seçin. Önceki adımlarda incelenirken derleme işlem hattı yapıt için kullanılan bir çıktı üretir.

1. Sağ tarafında **bırak** simgesini seçme **sürekli dağıtım tetikleyicisi**. Bu yayın işlem hattı, her seferinde yeni bir derleme yapıtının kullanılabilir bir dağıtımın yürüttüğü CD tetikleyicisi etkinleştirdi. İsteğe bağlı olarak, el ile yürütme dağıtımlarınızı gerektirir böylece tetikleyiciyi devre dışı bırakabilirsiniz.

1. Sağ taraftaki bölümü seçin **yayınları görüntüleyebilir** yayınlar geçmişini görüntülemek için.

1. İşlem hattı görüntüler sürüm üzerinde tıklayın. Sürümü denetlemek için herhangi bir ortamda tıklayın **özeti, işlemeler**, ilişkili **iş öğelerini**.

1. **İşlemeler**'i seçin. Bu görünüm, bu dağıtımla ilişkilendirilmiş kodu yürütmeleri gösterir. Dağıtımlar arasındaki işleme farklılıklarını görmek için yayınları karşılaştırın.

1. Seçin **günlükleri görüntüleyebilir**. Günlüklerde, dağıtım işlemiyle ilgili yararlı bilgiler bulunur. Bunları, sırasında ve sonrasında dağıtımları görüntüleyebilirsiniz.

## <a name="commit-code-changes-and-execute-cicd"></a>Kod değişikliklerini işleme ve CI/CD’yi yürütme

> [!NOTE]
> Aşağıdaki yordam, CI/CD işlem hattı basit metin değişikliği yaparak test eder.

Artık otomatik olarak Azure App Service için en son iş dağıtan bir CI/CD işlem kullanarak uygulamanızı bir ekip ile işbirliği yapmaya hazır. Her değişiklik Git deposu için Azure DevOps bir yapı başlatır ve Azure'a dağıtılacak bir CD işlem hattı yürütür. Bu bölümdeki yordamı izleyin veya değişiklikleri deponuza işlemek için başka bir teknik kullanın. Örneğin, en sevdiğiniz aracı veya IDE Git deposunda kopyalayabilirsiniz ve ardından bu depoya itme değiştirir.

1. Azure DevOps menüde **depoları | Dosyaları**, deponuza gidin.

1. Oluşturma işleminde seçtiğiniz uygulama diline göre kod deposu zaten var. Açık **Application/views/index.pug** dosya.

1. Seçin **Düzenle**ve ardından değişiklik **satır numarası 15** . Örneğin, kendisine güncelleştirebilirsiniz **My ilk Azure Cosmos DB tarafından desteklenen Azure App Service dağıtımı**

1. Sağ üst kısımdaki seçin **işleme**ve ardından **işleme** değişikliğiniz yeniden göndermek için.

     Birkaç dakika sonra Azure DevOps bir yapı başlatır ve değişiklikleri dağıtmak için bir yayın yürütür. DevOps projeleri Panoda veya tarayıcı Azure DevOps kuruluşunuz ile derleme durumunu izleyin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde, oluşturduğunuz ilgili kaynakları silebilirsiniz. Kullanım **Sil** DevOps projeleri Pano işlevselliği.

## <a name="next-steps"></a>Sonraki adımlar

Ekibinizin gereksinimlerine uygun olarak bu derleme ve yayın işlem hatlarını istediğiniz gibi değiştirebilirsiniz. Ayrıca bu CI/CD desenini diğer işlem hatlarınızda şablon olarak kullanabilirsiniz. Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Azure Cosmos DB tarafından desteklenen bir Node.js uygulamasını dağıtmak için DevOps projeleri'ni kullanın
> * Azure DevOps ve Azure aboneliğinin yapılandırın 
> * Azure Cosmos DB inceleyin
> * CI işlem hattını inceleme
> * CD işlem hattını inceleme
> * Değişiklikleri Git'e kaydedin ve otomatik olarak Azure'a dağıtın
> * Kaynakları temizleme
