---
title: Azure DevOps Projesi ile mevcut kodunuz için CI/CD işlem hattı oluşturma | Azure DevOps Services Öğreticisi
description: DevOps Projesi, Azure’u kullanmaya başlamayı kolaylaştırır. Birkaç hızlı adımda, tercih ettiğiniz bir Azure hizmetinde uygulama başlatmak için kendi kodunuzu ve GitHub deposunu kullanmanıza yardımcı olur.
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
ms.openlocfilehash: 02b6823a46c94edb0ba28c7a2a8b9ae0efc44ae8
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2018
ms.locfileid: "49406101"
---
# <a name="tutorial--create-a-cicd-pipeline-for-your-existing-code-with-the-azure-devops-project"></a>Öğretici: Azure DevOps Projesi ile mevcut kodunuz için CI/CD işlem hattı oluşturma

Azure DevOps Projesi, mevcut kodunuzu ve Git deponuzu getirebildiğiniz ya da Azure’da bir sürekli tümleştirme (CI) ve sürekli teslim (CD) işlem hattı oluşturmak üzere örnek uygulamalardan birini seçtiğiniz basitleştirilmiş bir deneyim sunar.

Yapacaklarınız:

> [!div class="checklist"]
> * Azure DevOps Projesi oluşturma
> * GitHub deponuza erişimi yapılandırma ve çerçeve seçme
> * Azure DevOps Services ve bir Azure aboneliği yapılandırma 
> * GitHub'daki değişiklikleri işleme ve Azure'a otomatik olarak dağıtma
> * Azure DevOps Services CI/CD işlem hattını inceleme
> * Application Insights izlemeyi yapılandırma

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/) aracılığıyla ücretsiz bir abonelik alabilirsiniz.
* .NET, Java, PHP, Node, Python veya statik web kodu içeren bir GitHub'a veya harici Git deposuna erişin.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Azure DevOps Projesi, Azure DevOps Services’te bir CI/CD işlem hattı oluşturur.  Yeni bir **Azure DevOps Services** kuruluşu oluşturabilir veya **var olan bir kuruluşu** kullanabilirsiniz.  Azure DevOps Projesi ayrıca tercih ettiğiniz **Azure aboneliğinde** **Azure kaynakları** oluşturur.

1. [Microsoft Azure portalında](https://portal.azure.com) oturum açın.

1. Sol gezinti çubuğunda **+ Yeni** simgesini seçin ve ardından **DevOps Projesini** arayın.  **Oluştur**’u seçin.

    ![Sürekli Teslim Başlatılıyor](_img/azure-devops-project-github/fullbrowser.png)

1. **Kendi kodunuzu getirin**’i seçin.  İşiniz bittiğinde **İleri**’yi seçin.

## <a name="configure-access-to-your-github-repository-and-choose-a-framework"></a>GitHub deponuza erişimi yapılandırma ve çerçeve seçme

1. **GitHub**'ı seçin.  İsteğe bağlı olarak, **harici Git deposu** da seçebilirsiniz.  Uygulamanızı içeren **deponuzu** ve **dalınızı** seçin.

1. **Web çerçevenizi** seçin.  İşiniz bittiğinde **İleri**’yi seçin.

    ![.NET Framework](_img/azure-devops-project-github/webframework.png)

1. Önceki adımlarda seçtiğiniz uygulama çerçevesi, burada bulabileceğiniz Azure hizmeti dağıtımı hedefi türünü belirler.  Dilediğiniz **hedef hizmeti** seçin.  İşiniz bittiğinde **İleri**’yi seçin.

## <a name="configure-azure-devops-services-and-an-azure-subscription"></a>Azure DevOps Services ve bir Azure aboneliği yapılandırma 

1. **Yeni** bir Azure DevOps Services kuruluşu oluşturun veya **var olan** bir kuruluşu seçin.  Azure DevOps Projeniz için bir **ad** seçin.  **Azure aboneliğinizi**, **konumunuzu** ve uygulamanız için bir **ad** seçin.  İşiniz bittiğinde **Bitti**’yi seçin.

1. **Azure DevOps Projesi panosu** birkaç dakika içinde Azure portala yüklenir.  Azure DevOps Services kuruluşunuzdaki bir depoda örnek uygulama ayarlanır, bir derleme yürütülür ve uygulamanız Azure’a dağıtılır.  Bu pano GitHub **kod deponuza**, **Azure DevOps Services CI/CD işlem hattına** ve **Azure’daki uygulamanıza** görünürlük sağlar.  Panonun sağ tarafında çalışan uygulamanızı görüntülemek için **Gözat**’ı seçin.

    ![Pano görünümü](_img/azure-devops-project-github/dashboardnopreview.png) 
    
Azure DevOps Projesi, otomatik olarak bir CI derleme ve yayın tetiklemesi yapılandırır.  Kodunuz, GitHub deponuzda veya diğer harici depoda kalır.  

## <a name="commit-changes-to-github-and-automatically-deploy-to-azure"></a>GitHub'daki değişiklikleri işleme ve Azure'a otomatik olarak dağıtma 

Artık en son çalışmanızı otomatik olarak web sitenize dağıtan bir CI/CD işlemiyle uygulamanız üzerinde bir ekiple birlikte çalışmaya hazırsınız.  GitHub deposunda yapılan her değişiklik Azure DevOps’ta bir derleme oluşturmaya başlar ve bir Azure DevOps CD işlem hattı, Azure’de bir dağıtım yürütür.

1.  Uygulamanızda bir değişiklik yapın ve değişikliği GitHub deponuza **işleyin**.
2.  Birkaç dakika içinde Azure DevOps Services’te bir derleme başlatılır.  Azure DevOps proje panosuyla veya Azure DevOps Services kuruluşunuzla tarayıcıda derleme durumunu izleyebilirsiniz.
3.  Derleme tamamlandığında, değişikliklerinizi gördüğünüzü doğrulamak için tarayıcıda **uygulamanızı yenileyin**.

## <a name="examine-the-azure-devops-services-cicd-pipeline"></a>Azure DevOps Services CI/CD işlem hattını inceleme

Azure DevOps Projesi, Azure DevOps Services kuruluşunuzda otomatik olarak bir Azure DevOps Services CI/CD işlem hattı yapılandırdı.  İşlem hattını gerektiği şekilde keşfedin ve özelleştirin.  Azure DevOps Services derleme ve yayın işlem hattı hakkında bilgi edinmek aşağıdaki adımları izleyin.

1. Azure DevOps Project panosunun**üst** kısmında **Derleme İşlem Hattı**’nı seçin.  Bu bağlantı bir tarayıcı sekmesi açar ve yeni projeniz için Azure DevOps Services derleme işlem hattını açar.

1. Fare imlecini, **Durum** alanının yanındaki derleme işlem hattının sağına götürün. Görüntülenen **üç noktayı** seçin.  Bu eylem, yeni bir derlemeyi sıraya alma, derleme duraklatma ve derleme işlem hattını düzenleme gibi birkaç etkinliği başlatabileceğiniz bir menüyü açar.

1. **Düzenle**’yi seçin.

1. Bu görünümden derleme işlem hattınızın **çeşitli görevlerini inceleyin**.  Derleme, Git deposundan kaynak getirme, bağımlılıkları geri yükleme ve dağıtım için kullanılan çıkışları yayımlama gibi çeşitli görevleri gerçekleştirir.

1. Derleme işlem hattının üst kısmında **derleme işlem hattı adını** seçin.

1. Derleme işlem hattınızın **adını** daha açıklayıcı bir şeyle değiştirin.  **Kaydet ve sıraya al**’ı ve ardından **Kaydet**’i seçin.

1. Derleme işlem hattı adınızın altında **Geçmiş**’i seçin.  Derleme için yaptığınız son değişikliklere ait denetim kaydını görürsünüz.  Azure DevOps Services, derleme işlem hattında yapılan değişiklikleri izler ve sürümleri karşılaştırmanızı sağlar.

1. **Tetikleyiciler**’i seçin.  Azure DevOps Projesi otomatik olarak bir CI tetikleyicisi oluşturur ve depoya yönelik her işleme yeni bir derleme başlatır.  İsteğe bağlı olarak dalları CI işlemine dahil etmeyi veya işlemden hariç tutmayı seçebilirsiniz.

1. **Saklama**’yı seçin.  Senaryonuza bağlı olarak, belirli sayıdaki derlemeleri saklayacak veya kaldıracak ilkeleri belirtebilirsiniz.

1. **Derleme ve Yayın**’ı ve ardından **Yayınlar**’ı seçin.  Azure DevOps projesi Azure'a yönelik dağıtımları yönetmek için bir Azure DevOps Services yayın işlem hattı oluşturdu.

1. Tarayıcının sol tarafında, yayın işlem hattınızın yanındaki **üç noktayı** ve ardından **Düzenle**’yi seçin.

1. Yayın işlem hattı, yayın işlemini tanımlayan bir **işlem hattı** içerir.  **Yapıtlar**’ın altında **Bırak**’ı seçin.  Önceki adımlarda incelediğiniz derleme işlem hattı, yapıt için kullanılan çıkışı üretir. 

1. **Bırak** simgesinin sağ tarafında **Sürekli dağıtım tetikleyicisi**’ni seçin.  Bu yayın işlem hattı, yeni bir derleme yapıtı kullanılabilir olduğunda bir dağıtım yürüten CD tetikleyicisini etkinleştirdi.  İsteğe bağlı olarak, dağıtımlarınızın el ile yürütme gerektirmesi için tetikleyiciyi devre dışı bırakabilirsiniz. 

1. Tarayıcının sol tarafında **Görevler**’i seçin.  Görevler, dağıtım işleminizin gerçekleştirdiği etkinliklerdir.  Bu örnekte, **Azure App Service**’e dağıtmak üzere bir görev oluşturuldu.

1. Tarayıcının sağ tarafında **Yayınları görüntüle**’yi seçin.  Bu görünümde yayın geçmişi gösterilir.

1. Yayınlarınızdan birinin yanındaki **üç nokta** simgesini seçin ve sonra da **Aç**'ı seçin.  Bu görünümde keşfedilebilecek yayın özeti, ilişkili iş öğeleri ve testler gibi çeşitli menüler vardır.

1. **İşlemeler**'i seçin.  Bu görünümde, belirli bir dağıtımla ilişkilendirilmiş kod işlemeleri gösterilir. 

1. **Günlükler**’i seçin.  Günlüklerde, dağıtım işlemiyle ilgili yararlı bilgiler bulunur.  Bunlar hem dağıtım sırasında hem de sonrasında görüntülenebilir.

## <a name="configure-azure-application-insights-monitoring"></a>Application Insights izlemeyi yapılandırma

Azure Application Insights ile, uygulamanızın performansını ve kullanımını kolayca izleyebilirsiniz.  Azure DevOps Projesi uygulamanız için otomatik olarak bir Application Insights kaynağı yapılandırır.  Gerekirse başka uyarılar ve izleme özellikleri de yapılandırabilirsiniz.

1. Azure portalında **Azure DevOps Projesi** panosuna gidin.  Panonun sağ alt kısmında uygulamanız için **Application Insights** bağlantısını seçin.

1. Azure portalında **Application Insights** dikey penceresi açılır.  Bu görünüm uygulamanızın kullanım, performans ve kullanılabilirlik izleme bilgilerini içerir.

    ![Application Insights](_img/azure-devops-project-github/appinsights.png) 

1. **Zaman aralığı**'nı ve ardından **Son saat**'i seçin.  Sonuçları filtrelemek için **Güncelleştir**'i seçin.  Şimdi son 60 dakikanın tüm etkinliğini görürsünüz.  Zaman aralığından çıkmak için **x** işaretini seçin.

1. **Uyarılar**'ı ve ardından **+ Ölçüm uyarısı ekle**'yi seçin.  

1. Uyarı için bir **Ad** girin.

1. **Kaynak Değişikliği**'nin açılan listesini seçin.  **App Service kaynağınızı seçin.**
<!-- Could you please confirm if this should be "Source Alter on" instead of "Source Alert on"? -->
1. Varsayılan uyarı, **1 saniyeden uzun sunucu yanıt süresi** içindir.  Çeşitli uyarı ölçümlerini incelemek için **Ölçüm** açılan listesini seçin.  Çeşitli uyarıları kolayca yapılandırıp uygulamanızın izleme özelliklerini geliştirebilirsiniz.

1. **E-posta sahipleri, katkıda bulunanlar ve okuyucular yoluyla bildir** onay kutusunu seçin.  İsteğe bağlı olarak, Azure mantıksal uygulamasını yürüterek bir uyarı verildiğinde ek eylemler gerçekleştirebilirsiniz.

1. Uyarıyı oluşturmak için **Tamam**'ı seçin.  Birkaç dakika içinde uyarı panoda etkin olarak gösterilir.  Uyarılar alanından **çıkın** ve **Application Insights dikey penceresine** dönün.

1. **Kullanılabilirlik** öğesini ve ardından **+ Test ekle**'yi seçin. 

1. **Test adı** girin ve **Oluştur**'u seçin.  Uygulamanızın kullanılabilirliğini doğrulamak için basit bir ping testi oluşturulur.  Birkaç dakika sonra, test sonuçları sağlanır ve Application Insights panosu bir kullanılabilirlik durumu görüntüler.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında, Azure DevOps projesi panosundaki **Sil** işlevini kullanarak bu hızlı başlangıçta oluşturulmuş olan Azure App Service’i ve ilgili kaynakları silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide CI/CD işleminizi yapılandırdığınızda, Azure DevOps Projenizde bir derleme ve yayın işlem hattı otomatik olarak oluşturulur. Ekibinizin ihtiyaçlarını karşılamak için bu derleme ve yayın işlem hatlarını değiştirebilirsiniz. Şunları öğrendiniz:

> [!div class="checklist"]
> * Azure DevOps Projesi oluşturma
> * GitHub deponuza erişimi yapılandırma ve çerçeve seçme
> * Azure DevOps Services ve bir Azure aboneliği yapılandırma 
> * GitHub'daki değişiklikleri işleme ve Azure'a otomatik olarak dağıtma
> * Azure DevOps Services CI/CD işlem hattını inceleme
> * Application Insights izlemeyi yapılandırma

Azure DevOps Services CI/CD işlem hattı hakkında daha fazla bilgi edinmek için şu eğiticiye bakın:

> [!div class="nextstepaction"]
> [CD işlemini özelleştirme](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts)
