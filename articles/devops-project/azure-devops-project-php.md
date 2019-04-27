---
title: 'Hızlı Başlangıç: Azure DevOps projeleri ile PHP için CI/CD işlem hattı oluşturma'
description: DevOps projeleri, Azure'da kullanmaya başlamak kolaylaştırır. Birkaç hızlı adımda, tercih ettiğiniz bir Azure hizmetinde uygulama başlatmanıza yardımcı olur.
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
ms.openlocfilehash: 82310857276c53c85af033ae32a3aeef4f33c8da
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60555059"
---
# <a name="create-a-cicd-pipeline-for-php-with-azure-devops-projects"></a>Azure DevOps projeleri ile PHP için CI/CD işlem hattı oluşturma

Azure DevOps projeleri, Azure kaynaklarını oluşturan ve sürekli tümleştirme (CI) ve PHP uygulamanızı Azure işlem hatları için sürekli teslim (CD) işlem hattı ayarlar basitleştirilmiş bir deneyim sunar.  

Azure aboneliğiniz yoksa, ücretsiz aracılığıyla edinebilirsiniz [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/).

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

 DevOps projeleri, Azure işlem hatlarında bir CI/CD işlem hattı oluşturur. Ücretsiz ve yeni bir Azure DevOps kuruluş oluşturun veya mevcut bir kuruluşa kullanın. DevOps projeleri, Azure kaynaklarını da tercih ettiğiniz bir Azure aboneliği oluşturur.

1. [Microsoft Azure portalında](https://portal.azure.com) oturum açın.

1. Sol bölmede seçin **kaynak Oluştur** simgesine ve ardından arama **DevOps projeleri**.  

3. **Oluştur**’u seçin.

    ![Sürekli teslim yapılandırması başlatılıyor](_img/azure-devops-project-php/fullbrowser.png)

## <a name="select-a-sample-application-and-azure-service"></a>Örnek uygulama ve Azure hizmeti seçme

1. Örnek PHP uygulamasını seçin.  
        PHP örnekleri birkaç uygulama çerçeveleri seçenekleri içerir. Varsayılan örnek Laravel çerçevedir. 
        
2. Varsayılan ayarı bırakın ve ardından **sonraki**.  

1. İçin Web App kapsayıcıları varsayılan dağıtım hedefidir.  
    Daha önce seçtiğiniz uygulama çerçevesi buradan kullanılabilir Azure hizmeti dağıtımı hedef türünü belirler.  Varsayılan hizmet bırakın ve ardından **sonraki**.
 
## <a name="configure-azure-devops-and-an-azure-subscription"></a>Azure DevOps ve Azure aboneliğinin yapılandırın 

1. Yeni bir Azure DevOps kuruluş oluşturun veya mevcut bir kuruluşa seçin. 

    a. Azure DevOps projeniz için bir ad seçin. 
    
    b. Azure aboneliği ve konumu seçin, uygulamanız için bir ad girin ve ardından **Bitti**.   
        DevOps projeleri Pano, birkaç dakika sonra Azure portalında görüntülenir. Azure DevOps kuruluşunuzdaki bir depodaki örnek bir uygulama kümesi, bir derleme çalışır ve uygulamanızı Azure'a dağıtır. Bu pano, kod deposu, CI/CD işlem hattınızı ve uygulamanızı azure'da görünürlük sağlar.  
        
2. Seçin **Gözat** çalışan uygulamanızı görüntülemek için.

    ![Pano görünümü](_img/azure-devops-project-php/dashboardnopreview.png) 
    
   DevOps projeleri otomatik olarak yapılandırılmış bir CI derleme ve yayın tetikleyicisi.  Artık en son çalışmanızı otomatik olarak web sitenize dağıtan bir CI/CD işlemiyle PHP uygulaması üzerinde bir ekiple birlikte çalışmaya hazırsınız.

## <a name="commit-code-changes-and-execute-cicd"></a>Kod değişikliklerini işleme ve CI/CD’yi yürütme

 DevOps projeleri, Azure depoları veya GitHub Git deposu oluşturur. Depo görüntülemek ve uygulamanıza kod değişikliği yapmanız için aşağıdaki adımları uygulayın:

1. DevOps projeleri panosunun sol tarafta, ana dalınıza bağlantıyı seçin.   
    Bu bağlantı yeni oluşturulan Git deposuna bir görünüm açar.

1. Depo kopya URL'sini görüntülemek için tarayıcının sağ üst kısmından **Kopya**’yı seçin.   
    Git deponuzu en sevdiğiniz IDE’de kopyalayabilirsiniz. Sonraki birkaç adımda yapıp kod değişiklikleri ana dala doğrudan web tarayıcısı kullanın.

1. Sol tarafta, Git **resources/views/welcome.blade.php** dosya.

1. Seçin **Düzenle**, bazı metinler için bir değişiklik yapın.  Örneğin, div etiketlerinden biri için metnin bir kısmını değiştirin.

1. Seçin **işleme**ve ardından değişikliklerinizi kaydedin.

1. Tarayıcınızda DevOps projeleri panoya gidin.  
Devam eden bir yapı görmelisiniz. Yaptığınız değişiklikler otomatik olarak oluşturulur ve bir CI/CD işlem hattı dağıtılır.

## <a name="examine-the-cicd-pipeline"></a>CI/CD işlem hattı inceleyin

 DevOps projeleri, eksiksiz bir CI/CD işlem hattı Azure işlem hatlarında otomatik olarak yapılandırır. İşlem hattını gerektiği şekilde keşfedin ve özelleştirin. Derleme ve yayın işlem hatları ile kendinizi alıştırın için aşağıdakileri yapın:

1. DevOps projeleri panonun üst kısmında seçin **derleme işlem hatlarını**.  
    Bu bağlantı, bir tarayıcı sekmesi ve derleme işlem hattı yeni projeniz için açar.

1. İşaret **durumu** alan ve ardından **üç nokta** (...).  
    Yeni bir yapıyı kuyruğa, bir derleme duraklatma ve derleme işlem hattı düzenleme gibi çeşitli seçenekler, bir menü görüntüler.

1. **Düzenle**’yi seçin.

1. Bu bölmede, derleme işlem hattı için çeşitli görevleri inceleyebilirsiniz.  
    Git deposundan kaynakları getirme gibi görevler, çeşitli yapı çalıştırıldığında, dağıtımları için kullanılan bağımlılıklarını geri yükleme ve yayımlama çıkarır.

1. Derleme işlem hattı üstünde derleme işlem hattı adı seçin.

1. Bir şeyler daha açıklayıcı, select, yapı işlem hattınızı adını değiştirmek **Kaydet ve kuyruğa**ve ardından **Kaydet**.

1. Derleme işlem hattı adınızın altında **Geçmiş**’i seçin.   
    **Geçmişi** bölmesi bir denetim kaydı derleme için en son değişikliği görüntüler. Azure işlem hatları için derleme işlem hattı yapılan değişiklikleri izler ve sürümleri karşılaştırmanızı sağlar.

1. **Tetikleyiciler**’i seçin.  
      DevOps projeleri CI tetikleyicisini otomatik olarak oluşturulan ve depoya her işleme, yeni bir yapı başlatır. İsteğe bağlı olarak dalları CI işlemine dahil etmeyi veya işlemden hariç tutmayı seçebilirsiniz.

1. **Saklama**’yı seçin.   
    Senaryonuza bağlı olarak, saklamak veya belirli bir sayıda derlemeleri kaldırmak için ilkeleri belirtebilirsiniz.

1. Seçin **derleme ve yayın**ve ardından **yayınlar**.  
     DevOps projeleri, azure'da dağıtımlarını yönetmek için bir yayın ardışık düzeni oluşturur.

1. Yayın işlem hattınızı yanındaki üç nokta (...) seçin ve ardından **Düzenle**.  
    Sürüm ardışık yayın işlemini tanımlar. bir işlem hattı içerir. 

12. **Yapıtlar**’ın altında **Bırak**’ı seçin.  
    Önceki adımlarda incelenirken derleme işlem hattı yapıt için kullanılan bir çıktı üretir. 

1. Yanındaki **bırak** simgesini seçme **sürekli dağıtım tetikleyicisi**.   
    Bu yayın işlem hattı yok her seferinde yeni bir derleme yapıtının kullanılabilir bir dağıtım çalıştığı etkin bir CD tetikleyicisine sahiptir.  İsteğe bağlı olarak, el ile yürütme dağıtımlarınızı gerektirir böylece tetikleyiciyi devre dışı bırakabilirsiniz. 

1. Sol tarafta, seçin **görevleri**.  
        Dağıtım işleminizin gerçekleştiren etkinlikler görevlerdir.  Bu örnekte, Azure App Service'e dağıtmak için bir görev oluşturulur.

1. Sağ tarafta seçin **yayınları görüntüleyebilir** yayınlar geçmişini görüntülemek için.

1. Sürümlerinizin birini yanındaki üç nokta (...) seçin ve ardından **açık**.  
        Bu görünümde keşfedilebilecek yayın özeti, ilişkili iş öğeleri ve testler gibi çeşitli menüler vardır.

1. **İşlemeler**'i seçin.  
        Bu görünümde, belirli bir dağıtım ile ilişkili kod tamamlama gösterilir. 

1. **Günlükler**’i seçin.  
        Günlüklerde, dağıtım işlemiyle ilgili yararlı bilgiler bulunur. Bunlar hem dağıtım sırasında hem de sonrasında görüntülenebilir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde, Azure App Service ve diğer ilgili kaynakları silebilirsiniz. Kullanım **Sil** DevOps projeleri Pano işlevselliği.

## <a name="next-steps"></a>Sonraki adımlar

Derleme, CI/CD işlem yapılandırılmış ve yayın işlem hatları otomatik olarak oluşturulan. Ekibinizin ihtiyaçlarını karşılamak için bu derleme ve yayın işlem hatlarını değiştirebilirsiniz. CI/CD işlem hattı hakkında daha fazla bilgi için bu öğreticiye bakın:

> [!div class="nextstepaction"]
> [CD işlemini özelleştirme](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts)
