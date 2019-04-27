---
title: "Hızlı Başlangıç: Azure DevOps projeleri'ni kullanarak Go programlama dili için CI/CD işlem hattı oluşturma"
description: DevOps projeleri, Azure'da kullanmaya başlamak kolaylaştırır. Birkaç Hızlı adımda bir Azure hizmeti üzerinde Go programlama dili web uygulaması başlatın yardımcı olur.
ms.prod: devops
ms.technology: devops-cicd
services: vsts
documentationcenter: vs-devops-build
author: mlearned
manager: douge
editor: ''
ms.assetid: ''
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 07/09/2018
ms.author: mlearned
ms.custom: mvc
monikerRange: vsts
ms.openlocfilehash: ca068b39c222a210e261234a132a0506f6b023bb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60554984"
---
# <a name="create-a-cicd-pipeline-for-go-by-using-azure-devops-projects"></a>Azure DevOps projeleri'ni kullanarak Go için bir CI/CD işlem hattı oluşturma

Azure DevOps projeleri'ni kullanarak sürekli tümleştirme (CI) ve Git uygulamanız için sürekli teslim (CD) yapılandırın. DevOps projeleri, bir Azure DevOps derleme ve yayın'in başlangıç yapılandırmasını basitleştirir işlem hattı.

Azure aboneliğiniz yoksa, [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/) üzerinden ücretsiz edinebilirsiniz.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

DevOps projeleri, Azure işlem hatlarında bir CI/CD işlem hattı oluşturur. Yeni bir Azure DevOps kuruluş oluşturun veya mevcut bir kuruluşa kullanın. DevOps projeleri, Azure kaynaklarını da tercih ettiğiniz bir Azure aboneliği oluşturur.

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Sol bölmede seçin **kaynak Oluştur**.

1. Arama kutusuna **DevOps projesi**ve ardından **Oluştur**.

    ![DevOps projeleri Panosu](_img/azure-devops-project-github/fullbrowser.png)

## <a name="select-a-sample-app-and-azure-service"></a>Bir örnek uygulaması ve Azure hizmeti seçin

1. Seçin **Git** sample uygulamasını ve ardından **sonraki**.  
    
1. **Basit Go uygulaması** varsayılan çerçevedir. **İleri**’yi seçin.  
    Daha önce seçtiğiniz, uygulama altyapısı, dağıtım için kullanılabilir olan Azure hizmet dağıtım hedefleri türünü belirler. 
    
1. Azure hizmeti varsayılan değerini bırakın ve seçin **sonraki**.

## <a name="configure-azure-devops-and-an-azure-subscription"></a>Azure DevOps ve Azure aboneliğinin yapılandırın 

1. Yeni bir ücretsiz Azure DevOps kuruluş oluşturun veya mevcut bir kuruluşa seçin. 

1. Azure DevOps projeniz için bir ad girin. 

1. Azure aboneliği ve konumu seçin, uygulamanız için bir ad girin ve ardından **Bitti**.  
    DevOps projeleri Pano, birkaç dakika sonra Azure portalında görüntülenir. Azure DevOps kuruluşunuzdaki bir depodaki örnek bir uygulama kümesi, bir derleme yürütülür ve uygulamanız Azure'a dağıtılır. 
    
    Pano, kod deposu, CI/CD işlem hattınızı ve uygulamanızı Azure'a görünürlük sağlar. Sağ taraftaki seçin **Gözat** çalışan uygulamanızı görüntülemek için.

    ![Pano görünümü](_img/azure-devops-project-go/dashboardnopreview.png) 

## <a name="commit-your-code-changes-and-execute-the-cicd"></a>Kod değişikliklerinizi işleyin ve CI/CD yürütün

DevOps projeleri, Azure depoları veya GitHub Git deposu oluşturur. Depo görüntülemek ve uygulamanıza kod değişiklikleri yapmak için aşağıdakileri yapın:

1. DevOps projeleri, soldaki, ana dal için bağlantıyı seçin.  
    Bağlantıyı yeni oluşturulan bir Git deposu için bir görünümü açılır.

1. Depo kopya URL'si görüntülemek için seçin **kopya** sağ üst köşedeki.  
    Sık kullandığınız IDE'de Git deponuzu kopyalayabilirsiniz. Sonraki birkaç adımda, kod değişiklikleri yapıp doğrudan ana dala işlemek için web tarayıcısını kullanabilirsiniz.

1. Solda, Git *views/index.html* dosya ve ardından **Düzenle**.

1. Dosyaya bir değişiklik yapın. Örneğin, metin içinde div etiketlerinden birini değiştirin.

1. Seçin **işleme**ve ardından değişikliklerinizi kaydedin.

1. Tarayıcınızda DevOps projeleri panoya gidin.  
    Devam eden bir yapı olmalıdır. Yaptığınız değişiklikleri otomatik olarak oluşturulur ve bir CI/CD işlem hattı dağıtılır.

## <a name="examine-the-cicd-pipeline"></a>CI/CD işlem hattı inceleyin

DevOps projeleri, eksiksiz bir CI/CD işlem hattı Azure depolarda otomatik olarak yapılandırır. İşlem hattını gerektiği şekilde keşfedin ve özelleştirin. Azure DevOps yapıyla hakkında bilgilenmeli ve yayın işlem hatları için aşağıdakileri yapın:

1. DevOps projeleri panosuna gidin.

1. Sayfanın üstünde seçin **derleme işlem hatlarını**.  
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

1. Seçin **derleme ve yayın**ve ardından **yayınlar**.  
    DevOps projeleri, azure'da dağıtımlarını yönetmek için bir yayın ardışık düzeni oluşturur.

1. Yayın işlem hattınızı yanındaki üç nokta (...) seçin ve ardından **Düzenle**.  
    Yayın işlem hattı, yayın işlemini tanımlayan bir *işlem hattı* içerir.

1. **Yapıtlar**’ın altında **Bırak**’ı seçin.  
    Denetlenen derleme işlem hattı daha önce yapıt için kullanılan bir çıktı üretir. 

1. Sağ tarafında **bırak** simgesini seçme **sürekli dağıtım tetikleyicisi**.  
    Bu yayın işlem hattı, her seferinde yeni bir derleme yapıtının kullanılabilir olan bir dağıtımın yürüttüğü etkin bir CD tetikleyicisine sahiptir. İsteğe bağlı olarak, el ile yürütme dağıtımlarınızı gerektirir böylece tetikleyiciyi devre dışı bırakabilirsiniz. 

1. Sol tarafta seçin **görevleri**.  
    Görevler gerçekleştirir, dağıtım işlemi etkinliklerdir. Bu örnekte, Azure App Service'e dağıtmak için bir görev oluşturulur.

1. Sağ taraftaki seçin **yayınları görüntüleyebilir** yayınlar geçmişini görüntülemek için.

1. Bir yayın yanındaki üç nokta (...) seçin ve ardından **açık**.  
    Yayın özeti ilişkili iş öğeleri ve test gibi çeşitli menüleri keşfedebilirsiniz.

1. **İşlemeler**'i seçin.  
    Bu görünüm, bu dağıtımla ilişkilendirilmiş kodu yürütmeleri gösterir. 

1. **Günlükler**’i seçin.  
    Günlüklerde, dağıtım işlemiyle ilgili yararlı bilgiler bulunur. Bunları, sırasında ve sonrasında dağıtımları görüntüleyebilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, Azure App Service örneği ve bu hızlı başlangıçta oluşturduğunuz kaynaklar silebilirsiniz. Bunu yapmak için **Sil** DevOps projeleri Pano işlevselliği.

## <a name="next-steps"></a>Sonraki adımlar

Derleme değiştirme hakkında daha fazla bilgi edinin ve yayın işlem hatları, takımınızın ihtiyaçlarını karşılamak üzere için bkz:

> [!div class="nextstepaction"]
> [Çok aşamalı sürekli dağıtım (CD) işlem hattınızı tanımlayın](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts)
