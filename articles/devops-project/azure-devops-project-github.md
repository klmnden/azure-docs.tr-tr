---
title: Azure DevOps Projesi ile mevcut kodunuz için CI/CD işlem hattı oluşturma | VSTS Öğreticisi
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
ms.openlocfilehash: 8c92b45cd3949e56515286c963b035e3c449835b
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37967448"
---
# <a name="create-a-cicd-pipeline-for-your-existing-code-with-the-azure-devops-project"></a>Azure DevOps Projesi ile mevcut kodunuz için CI/CD işlem hattı oluşturma

Azure DevOps Projesi, mevcut kodunuzu ve Git deponuzu getirebildiğiniz ya da Azure’da bir sürekli tümleştirme (CI) ve sürekli teslim (CD) işlem hattı oluşturmak üzere örnek uygulamalardan birini seçtiğiniz basitleştirilmiş bir deneyim sunar.

Yapacaklarınız:

> [!div class="checklist"]
> * Azure DevOps projesi oluşturma
> * GitHub deponuza erişimi yapılandırma ve çerçeve seçme
> * GitHub'ı ve Azure aboneliğini yapılandırma 
> * GitHub'daki değişiklikleri işleme ve Azure'a otomatik olarak dağıtma
> * VSTS CI/CD işlem hattını inceleme
> * Application Insights izlemeyi yapılandırma

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/) aracılığıyla ücretsiz bir abonelik alabilirsiniz.
* .NET, Java, PHP, Node, Python veya statik web kodu içeren bir GitHub'a veya harici Git deposuna erişin.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Azure DevOps Projesi VSTS'de bir CI/CD işlem hattı oluşturur.  **Yeni VSTS** hesabı oluşturabilir veya **mevcut bir hesabı** kullanabilirsiniz.  Azure DevOps Projesi ayrıca tercih ettiğiniz **Azure aboneliğinde** **Azure kaynakları** oluşturur.

1. [Microsoft Azure portalında](https://portal.azure.com) oturum açın.

1. Sol gezinti çubuğunda **+ Yeni** simgesini seçin ve ardından **DevOps projesini** arayın.  **Oluştur**’u seçin.

    ![Sürekli Teslim Başlatılıyor](_img/azure-devops-project-github/fullbrowser.png)

1. **Kendi kodunuzu getirin**’i seçin.  İşiniz bittiğinde **İleri**’yi seçin.

## <a name="configure-access-to-your-github-repository-and-choose-a-framework"></a>GitHub deponuza erişimi yapılandırma ve çerçeve seçme

1. **GitHub**'ı seçin.  İsteğe bağlı olarak, **harici Git deposu** da seçebilirsiniz.  Uygulamanızı içeren **deponuzu** ve **dalınızı** seçin.

1. **Web çerçevenizi** seçin.  İşiniz bittiğinde **İleri**’yi seçin.

    ![.NET Framework](_img/azure-devops-project-github/webframework.png)

1. Önceki adımlarda seçtiğiniz uygulama çerçevesi, burada bulabileceğiniz Azure hizmeti dağıtımı hedefi türünü belirler.  Dilediğiniz **hedef hizmeti** seçin.  İşiniz bittiğinde **İleri**’yi seçin.

## <a name="configure-vsts-and-an-azure-subscription"></a>GitHub'ı ve Azure aboneliğini yapılandırma 

1. **Yeni** bir VSTS hesabı oluşturun veya **mevcut** bir hesabı kullanın.  VSTS projeniz için bir **ad** seçin.  **Azure aboneliğinizi**, **konumunuzu** ve uygulamanız için bir **ad** seçin.  İşiniz bittiğinde **Bitti**’yi seçin.

    ![VSTS bilgilerini girme](_img/azure-devops-project-github/vstsazureinfo.png)

1. Birkaç dakika içinde **proje panosu** Azure portalda yüklenir.  VSTS hesabınızdaki bir depoda örnek uygulama ayarlanır, bir derleme yürütülür ve uygulamanız Azure’a dağıtılır.  Bu pano GitHub **kod deponuza**, **VSTS CI/CD işlem hattına** ve **Azure’daki uygulamanıza** görünürlük sağlar.  Panonun sağ tarafında çalışan uygulamanızı görüntülemek için **Gözat**’ı seçin.

    ![Pano görünümü](_img/azure-devops-project-github/dashboardnopreview.png) 
    
Azure DevOps projesi otomatik olarak bir CI derlemesi ve yayın tetikleyicisi yapılandırır.  Kodunuz, GitHub deponuzda veya diğer harici depoda kalır.  

## <a name="commit-changes-to-github-and-automatically-deploy-to-azure"></a>GitHub'daki değişiklikleri işleme ve Azure'a otomatik olarak dağıtma 

Artık en son çalışmanızı otomatik olarak web sitenize dağıtan bir CI/CD işlemiyle uygulamanız üzerinde bir ekiple birlikte çalışmaya hazırsınız.  GitHub deposunda yapılan her değişiklik VSTS'de bir derleme başlatır ve bir VSTS Release Management tanımı Azure'da bir dağıtım yürütür.

1.  Uygulamanızda bir değişiklik yapın ve değişikliği GitHub deponuza **işleyin**.
2.  Birkaç dakika içinde VSTS'de bir derleme başlatılır.  DevOps proje panosuyla veya VSTS hesabınızı kullanarak tarayıcıda derleme durumunu izleyebilirsiniz.
3.  Derleme tamamlandığında, değişikliklerinizi gördüğünüzü doğrulamak için tarayıcıda **uygulamanızı yenileyin**.

## <a name="examine-the-vsts-cicd-pipeline"></a>VSTS CI/CD işlem hattını inceleme

Azure DevOps projesi, VSTS hesabınızda otomatik olarak tam bir VSTS CI/CD işlem hattı yapılandırdı.  İşlem hattını gerektiği şekilde keşfedin ve özelleştirin.  VSTS derlemesi ve yayın tanımları ile kendinizi alıştırmak için aşağıdaki adımları izleyin.

1. Azure DevOps projesi panosunun **üst** kısmından **Derleme İşlem Hatları**’nı seçin.  Bu bağlantı, bir tarayıcı sekmesi açar ve yeni projeniz için VSTS derleme tanımını açar.

1. Fare imlecini **Durum** alanının yanındaki derleme tanımının sağına taşıyın. Görüntülenen **üç noktayı** seçin.  Bu eylem, yeni bir derlemeyi kuyruğa alma, derlemeyi duraklatma ve derleme tanımını düzenleme gibi çeşitli etkinlikleri başlatabileceğiniz bir menüyü açar.

1. **Düzenle**’yi seçin.

1. Bu görünümden derleme tanımınızın **çeşitli görevlerini inceleyin**.  Derleme, Git deposundan kaynak getirme, bağımlılıkları geri yükleme ve dağıtım için kullanılan çıkışları yayımlama gibi çeşitli görevleri gerçekleştirir.

1. Derleme tanımının üst kısmında **derleme tanımı adını** seçin.

1. Derleme tanımınızın **adını** daha açıklayıcı bir şeyle değiştirin.  **Kaydet ve kuyruğa al**’ı ve **Kaydet**’i seçin.

1. Derleme tanımı adınızın altında **Geçmiş**’i seçin.  Derleme için yaptığınız son değişikliklere ait denetim kaydını görürsünüz.  VSTS, derleme tanımında yapılan değişiklikleri izler ve sürümleri karşılaştırmanızı sağlar.

1. **Tetikleyiciler**’i seçin.  Azure DevOps projesi otomatik olarak bir CI tetikleyicisi oluşturdu. Depoya yönelik her işleme yeni bir derleme başlatır.  İsteğe bağlı olarak dalları CI işlemine dahil etmeyi veya işlemden hariç tutmayı seçebilirsiniz.

1. **Saklama**’yı seçin.  Senaryonuza bağlı olarak, belirli sayıdaki derlemeleri saklayacak veya kaldıracak ilkeleri belirtebilirsiniz.

1. **Derleme ve Yayın**’ı ve ardından **Yayınlar**’ı seçin.  Azure DevOps projesi Azure'a yönelik dağıtımları yönetmek için bir VSTS yayın oluşturur.

1. Tarayıcının sol tarafında, yayın tanımınızın yanındaki **üç noktayı** ve ardından **Düzenle**’yi seçin.

1. Yayın tanımı, yayın işlemini tanımlayan bir **işlem hattı** içerir.  **Yapıtlar**'ın altında **Bırak**’ı seçin.  Önceki adımlarda incelediğiniz derleme tanımı, yapıt için kullanılan çıkışı üretir. 

1. **Bırak** simgesinin sağ tarafında **Sürekli dağıtım tetikleyicisi**’ni seçin.  Bu sürüm tanımı, yeni bir derleme yapıtı kullanılabilir olduğunda bir dağıtım yürüten CD tetikleyicisini etkinleştirdi.  İsteğe bağlı olarak, dağıtımlarınızın el ile yürütme gerektirmesi için tetikleyiciyi devre dışı bırakabilirsiniz. 

1. Tarayıcının sol tarafında **Görevler**’i seçin.  Görevler, dağıtım işleminizin gerçekleştirdiği etkinliklerdir.  Bu örnekte, **Azure App Service**’e dağıtmak üzere bir görev oluşturuldu.

1. Tarayıcının sağ tarafında **Yayınları görüntüle**’yi seçin.  Bu görünümde yayın geçmişi gösterilir.

1. Yayınlarınızdan birinin yanındaki **üç nokta** simgesini seçin ve sonra da **Aç**'ı seçin.  Bu görünümde keşfedilebilecek sürüm özeti, ilişkili iş öğeleri ve testler gibi çeşitli menüler vardır.

1. **İşlemeler**'i seçin.  Bu görünümde, belirli bir dağıtımla ilişkilendirilmiş kod işlemeleri gösterilir. 

1. **Günlükler**’i seçin.  Günlüklerde, dağıtım işlemiyle ilgili yararlı bilgiler bulunur.  Bunlar hem dağıtım sırasında hem de sonrasında görüntülenebilir.

## <a name="configure-azure-application-insights-monitoring"></a>Application Insights izlemeyi yapılandırma

Azure Application Insights ile, uygulamanızın performansını ve kullanımını kolayca izleyebilirsiniz.  Azure DevOps projesi uygulamanız için otomatik olarak bir Application Insights kaynağı yapılandırır.  Gerekirse başka uyarılar ve izleme özellikleri de yapılandırabilirsiniz.

1. Azure portalında **Azure DevOps Projesi** panosuna gidin.  Panonun sağ alt kısmında uygulamanız için **Application Insights** bağlantısını seçin.

1. Azure portalında **Application Insights** dikey penceresi açılır.  Bu görünüm uygulamanızın kullanım, performans ve kullanılabilirlik izleme bilgilerini içerir.

    ![Application Insights](_img/azure-devops-project-github/appinsights.png) 

1. **Zaman aralığı**'nı ve ardından **Son saat**'i seçin.  Sonuçları filtrelemek için **Güncelleştir**'i seçin.  Şimdi son 60 dakikanın tüm etkinliğini görürsünüz.  Zaman aralığından çıkmak için **x** işaretini seçin.

1. **Uyarılar**'ı ve ardından **+ Ölçüm uyarısı ekle**'yi seçin.  

1. Uyarı için bir **Ad** girin.

1. **Kaynak Değişikliği**'nin açılan listesini seçin.  **App Service kaynağınızı** seçin.

1. Varsayılan uyarı, **1 saniyeden uzun sunucu yanıt süresi** içindir.  Çeşitli uyarı ölçümlerini incelemek için **Ölçüm** açılan listesini seçin.  Çeşitli uyarıları kolayca yapılandırıp uygulamanızın izleme özelliklerini geliştirebilirsiniz.

1. **E-posta sahipleri, katkıda bulunanlar ve okuyucular yoluyla bildir** onay kutusunu seçin.  İsteğe bağlı olarak, Azure mantıksal uygulamasını yürüterek bir uyarı verildiğinde ek eylemler gerçekleştirebilirsiniz.

1. Uyarıyı oluşturmak için **Tamam**'ı seçin.  Birkaç dakika içinde uyarı panoda etkin olarak gösterilir.  Uyarılar alanından **çıkın** ve **Application Insights dikey penceresine** dönün.

1. **Kullanılabilirlik** öğesini ve ardından **+ Test ekle**'yi seçin. 

1. **Test adı** girin ve **Oluştur**'u seçin.  Uygulamanızın kullanılabilirliğini doğrulamak için basit bir ping testi oluşturulur.  Birkaç dakika sonra, test sonuçları sağlanır ve Application Insights panosu bir kullanılabilirlik durumu görüntüler.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında, Azure DevOps projesi panosundaki **Sil** işlevini kullanarak bu hızlı başlangıçta oluşturulmuş olan Azure App Service’i ve ilgili kaynakları silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide CI/CD işleminizi yapılandırdığınızda, bir derleme ve yayın tanımı VSTS projenizde otomatik olarak oluşturulur. Ekibinizin gereksinimlerine uygun olarak bu derleme ve yayın tanımlarını istediğiniz gibi değiştirebilirsiniz. Şunları öğrendiniz:

> [!div class="checklist"]
> * Azure DevOps projesi oluşturma
> * GitHub deponuza erişimi yapılandırma ve çerçeve seçme
> * GitHub'ı ve Azure aboneliğini yapılandırma 
> * GitHub'daki değişiklikleri işleme ve Azure'a otomatik olarak dağıtma
> * VSTS CI/CD işlem hattını inceleme
> * Application Insights izlemeyi yapılandırma

VSTS işlem hattı hakkında daha fazla bilgi için şu öğreticiye bakın:

> [!div class="nextstepaction"]
> [CD işlemini özelleştirme](https://docs.microsoft.com/vsts/pipelines/release/define-multistage-release-process?view=vsts)
