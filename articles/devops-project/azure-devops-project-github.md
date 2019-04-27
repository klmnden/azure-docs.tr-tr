---
title: "Öğretici: Azure DevOps projeleri'ni kullanarak mevcut kodunuz için CI/CD işlem hattı oluşturma"
description: Azure DevOps projeleri, Azure'da kullanmaya başlamak kolaylaştırır. DevOps projeleri birkaç Hızlı adımda, tercih ettiğiniz bir Azure hizmeti üzerinde uygulama başlatın, kendi kod ve GitHub deposunun yardımcı olur.
services: vsts
documentationcenter: vs-devops-build
ms.author: mlearned
ms.manager: douge
editor: ''
ms.assetid: ''
ms.workload: web
ms.prod: devops
ms.technology: devops-cicd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/09/2018
author: mlearned
ms.custom: mvc
monikerRange: vsts
ms.openlocfilehash: 88ee15a3b5cc53542d9e098dee485b8a526bb9a6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60555360"
---
# <a name="tutorial-create-a-cicd-pipeline-for-your-existing-code-by-using-azure-devops-projects"></a>Öğretici: Azure DevOps projeleri'ni kullanarak mevcut kodunuz için CI/CD işlem hattı oluşturma

Azure DevOps projeleri, mevcut kodunuzu ve Git deposuna taşıyın veya sürekli tümleştirme (CI) ve azure'a sürekli teslim (CD) işlem hattı oluşturmak için örnek bir uygulama seçin basitleştirilmiş bir deneyim sunar.

Yapacaklarınız:

> [!div class="checklist"]
> * Bir CI/CD işlem hattı oluşturmak için DevOps projeleri'ni kullanın
> * GitHub deponuzda erişimi yapılandırma ve bir çerçeve seçin
> * Azure DevOps ve Azure aboneliğinin yapılandırın 
> * Değişiklikleri Github'a işleyin ve otomatik olarak Azure'a dağıtın
> * Azure işlem hatları CI/CD işlem hattı inceleyin
> * Application Insights izlemeyi yapılandırma
> * Kaynakları temizleme

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/) aracılığıyla ücretsiz bir abonelik alabilirsiniz.
* Bir GitHub veya .NET, Java, PHP, düğüm, Python veya statik web kodu içeren harici bir Git deposundan erişim.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Azure DevOps projeleri, Azure işlem hatlarında bir CI/CD işlem hattı oluşturur. Yeni bir Azure DevOps kuruluş oluşturun veya mevcut bir kuruluşa kullanın. Azure DevOps projeleri, Azure kaynaklarını da tercih ettiğiniz bir Azure aboneliği oluşturur.

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Sol bölmede seçin **yeni**.

1. Arama kutusuna **DevOps projeleri**ve ardından **Oluştur**.

    ![DevOps projeleri Panosu](_img/azure-devops-project-github/fullbrowser.png)

1. Seçin **kendi kodunuzu getirin**ve ardından **sonraki**.

## <a name="configure-access-to-your-github-repo-and-choose-a-framework"></a>GitHub deponuzda erişimi yapılandırma ve bir çerçeve seçin

1. Şunlardan birini seçin **GitHub** veya dış bir Git deposundan ve deponuzu ve uygulamanızı içeren dalı seçin.

1. Web Çerçevenizi seçin ve ardından **sonraki**.

    ![.NET Framework](_img/azure-devops-project-github/webframework.png)

    Daha önce seçtiğiniz uygulama çerçevesi buradan kullanılabilir Azure hizmeti dağıtımı hedef türünü belirler. 
    
1. Hedef hizmeti seçin ve ardından **sonraki**.

## <a name="configure-azure-devops-and-an-azure-subscription"></a>Azure DevOps ve Azure aboneliğinin yapılandırın 

1. Yeni bir Azure DevOps kuruluş oluşturun veya mevcut bir kuruluşa seçin.

    a. Azure DevOps projeniz için bir ad girin. 
    
    b. Azure aboneliği ve konumu seçin, uygulamanız için bir ad girin ve ardından **Bitti**.

    DevOps projeleri Pano, birkaç dakika sonra Azure portalında görüntülenir. Azure DevOps kuruluşunuzdaki bir depodaki örnek bir uygulama kümesi, bir derleme yürütülür ve uygulamanızı Azure'a dağıtılır. Bu pano, GitHub kod deposu, CI/CD işlem hattı ve uygulamanızı azure'da görünürlük sağlar. 
    
1. Seçin **Gözat** çalışan uygulamanızı görüntülemek için.

    ![DevOps projeleri Pano görünümü](_img/azure-devops-project-github/dashboardnopreview.png) 
    
Azure DevOps projeleri otomatik olarak yapılandırır bir CI derleme ve yayın tetikleyicisi. Kodunuzu GitHub deponuza veya başka bir dış depoya kalır. 

## <a name="commit-changes-to-github-and-automatically-deploy-them-to-azure"></a>Değişiklikleri Github'a işleyin ve otomatik olarak Azure'a dağıtın 

Artık otomatik olarak en son iş sitenize dağıtan bir CI/CD işlem kullanarak uygulamanızı bir ekip ile işbirliği yapmaya hazır. GitHub deposuna her değişiklik, Azure DevOps bir yapı başlatır ve Azure'a dağıtılacak bir CD işlem hattı yürütür.

1. Uygulamanızda değişikliği yapın ve ardından değişikliği GitHub deponuza işleyin.  
    Birkaç dakika sonra Azure işlem hatlarında bir derleme başlar. DevOps projeleri panosunda yapı durumunu izleyebilir veya tarayıcıda Azure DevOps kuruluşunuz ile izleyebilirsiniz.

1. Derleme tamamlandıktan sonra değişikliklerinizi doğrulamak için uygulamanızı yenileyin.

## <a name="examine-the-azure-pipelines-cicd-pipeline"></a>Azure işlem hatları CI/CD işlem hattı inceleyin

Azure DevOps projeleri, bir CI/CD işlem hattı Azure işlem hatlarında otomatik olarak yapılandırır. İşlem hattını gerektiği şekilde keşfedin ve özelleştirin. Derleme ve yayın işlem hatları ile kendinizi alıştırın için aşağıdakileri yapın:

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
    Derleme için yaptığınız son değişikliklere ait denetim kaydını görürsünüz. Azure DevOps derleme işlem hattı için yapılan değişiklikleri izler ve sürümleri karşılaştırmanızı sağlar.

1. **Tetikleyiciler**’i seçin.  
    Azure DevOps projeleri CI tetikleyicisini otomatik olarak oluşturur ve depoya her işleme, yeni bir derleme başlatır. İsteğe bağlı olarak, dahil etmek veya dallar CI işleminden hariç tutmak seçim yapabilirsiniz.

1. **Saklama**’yı seçin.  
        Senaryonuza bağlı olarak, saklamak veya belirli bir sayıda derlemeleri kaldırmak için ilkeleri belirtebilirsiniz.

1. Seçin **derleme ve yayın**ve ardından **yayınlar**.  
    Azure DevOps projeleri, azure'da dağıtımlarını yönetmek için bir yayın ardışık düzeni oluşturur.

1. Yayın işlem hattınızı yanındaki üç nokta (...) seçin ve ardından **Düzenle**.  
    Yayın işlem hattı, yayın işlemini tanımlayan bir *işlem hattı* içerir. 
    
1. **Yapıtlar**’ın altında **Bırak**’ı seçin.  
    Önceki adımlarda incelenirken derleme işlem hattı yapıt için kullanılan bir çıktı üretir. 

1. Yanındaki **bırak** simgesini seçme **sürekli dağıtım tetikleyicisi**.  
    Bu yayın işlem hattı, yeni bir derleme yapıtı kullanılabilir olduğunda bir dağıtım yürüten CD tetikleyicisini etkinleştirdi. İsteğe bağlı olarak, el ile yürütme dağıtımlarınızı gerektirir böylece tetikleyiciyi devre dışı bırakabilirsiniz. 

1. Sol tarafta seçin **görevleri**.  
    Dağıtım işleminizin yürütür etkinlikleri görevlerdir. Bu örnekte, Azure App Service'e dağıtmak için bir görev oluşturulur.

1. Sağ taraftaki seçin **yayınları görüntüleyebilir** yayınlar geçmişini görüntülemek için.

1. Bir yayın yanındaki üç nokta (...) seçin ve ardından **açık**.  
    Yayın özeti, ilişkili iş öğeleri ve test gibi keşfetmek için birkaç menüleri vardır.

1. **İşlemeler**'i seçin.  
    Bu görünüm, bu dağıtımla ilişkilendirilmiş kodu yürütmeleri gösterir. 

1. **Günlükler**’i seçin.  
    Günlüklerde, dağıtım işlemiyle ilgili yararlı bilgiler bulunur. Bunları, sırasında ve sonrasında dağıtımları görüntüleyebilirsiniz.

## <a name="configure-azure-application-insights-monitoring"></a>Application Insights izlemeyi yapılandırma

Azure Application Insights ile, uygulamanızın performansını ve kullanımını kolayca izleyebilirsiniz. Azure DevOps projeleri, uygulamanız için Application Insights kaynağı otomatik olarak yapılandırır. Gerekirse başka uyarılar ve izleme özellikleri de yapılandırabilirsiniz.

1. Azure portalında DevOps projeleri panoya gidin. 

1. Alt sağ tarafta seçin **Application Insights** uygulamanıza yönelik bağlantı.  
    **Application Insights** bölmesi açılır. Bu görünüm uygulamanızın kullanım, performans ve kullanılabilirlik izleme bilgilerini içerir.

    ![Application Insights bölmesi](_img/azure-devops-project-github/appinsights.png) 

1. Seçin **zaman aralığı**ve ardından **son bir saat**. Sonuçları filtrelemek için seçin **güncelleştirme**.  
    Artık tüm etkinliğinden son 60 dakika görüntüleyebilirsiniz. Zaman aralığı'ndan çıkmak için seçin **x**.

1. Seçin **uyarılar**ve ardından **ölçüm uyarısı Ekle**. 

1. Uyarı için bir ad girin.

1. İçinde **kaynak Alter üzerinde** aşağı açılan listesinden, **App Service kaynak.** <!-- Please confirm whether this should be "Source Alter on" or "Source Alert on" -->

1. İçinde **ölçüm** aşağı açılan listesinde, çeşitli uyarı ölçümlerini inceleyin.  
    Varsayılan uyarı, **1 saniyeden uzun sunucu yanıt süresi** içindir. Çeşitli uyarıları kolayca yapılandırıp uygulamanızın izleme özelliklerini geliştirebilirsiniz.

1. Seçin **bildirim e-posta sahipleri, Katkıda Bulunanlar ve okuyucular aracılığıyla** onay kutusu.  
    İsteğe bağlı olarak, Azure logic app yürüterek bir uyarı gösterildiğinde ek eylemler gerçekleştirebilirsiniz.

1. Seçin **Tamam** uyarı oluşturmak için.  
    Birkaç dakika sonra Panoda etkin olarak bir uyarı görünür.
    
1. Çıkış **uyarılar** alanı geri dönerek **Application Insights** bölmesi.

1. Seçin **kullanılabilirlik**ve ardından **Ekle test**. 

1. Test adı girin ve ardından **Oluştur**.  
    Uygulamanızın kullanılabilirliğini doğrulamak için basit bir ping testi oluşturulur. Birkaç dakika sonra, test sonuçları sağlanır ve Application Insights panosu bir kullanılabilirlik durumu görüntüler.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, Azure App service ve Bu öğreticide oluşturduğunuz ilgili kaynakları silin. Bunu yapmak için **Sil** DevOps projeleri Pano işlevselliği.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, CI/CD işlem yapılandırıldığında, bir derleme ve yayın işlem hattınızı otomatik olarak oluşturulmuş Azure DevOps projeleri. Ekibinizin ihtiyaçlarını karşılamak için bu derleme ve yayın işlem hatlarını değiştirebilirsiniz. Şunları öğrendiniz:

> [!div class="checklist"]
> * Bir CI/CD işlem hattı oluşturmak için DevOps projeleri'ni kullanın
> * GitHub deponuzda erişimi yapılandırma ve bir çerçeve seçin
> * Azure DevOps ve Azure aboneliğinin yapılandırın 
> * Değişiklikleri Github'a işleyin ve otomatik olarak Azure'a dağıtın
> * Azure işlem hatları CI/CD işlem hattı inceleyin
> * Application Insights izlemeyi yapılandırma
> * Kaynakları temizleme

CI/CD işlem hattı hakkında daha fazla bilgi için bkz:

> [!div class="nextstepaction"]
> [Çok aşamalı sürekli dağıtım (CD) işlem hattınızı tanımlayın](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts)
