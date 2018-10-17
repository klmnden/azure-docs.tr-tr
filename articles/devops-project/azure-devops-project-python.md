---
title: Azure DevOps Projesi ile Python için CI/CD işlem hattı oluşturma | Hızlı Başlangıç
description: DevOps Projesi, Azure’u kullanmaya başlamayı kolaylaştırır. Birkaç hızlı adımda, tercih ettiğiniz bir Azure hizmetinde uygulama başlatmanıza yardımcı olur.
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
ms.openlocfilehash: 46f65772ed4cb70b80674ae39629f52694be49e1
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2018
ms.locfileid: "47405658"
---
# <a name="create-a-cicd-pipeline-for-python-with-the-azure-devops-project"></a>Azure DevOps Projesi ile Python için CI/CD işlem hattı oluşturma

Azure DevOps Projesi, Azure DevOps Services’te Python uygulamanız için Azure kaynakları oluşturan ve sürekli tümleştirme (CI) ve sürekli teslim (CD) işlem hattı ayarlayan basitleştirilmiş bir deneyim sunar.  

Azure aboneliğiniz yoksa, [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/) üzerinden ücretsiz edinebilirsiniz.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Azure DevOps Projesi, Azure'da bir CI/CD işlem hattı oluşturur.  Yeni bir ücretsiz **Azure DevOps Services** kuruluşu oluşturabilir veya **var olan bir kuruluşu** kullanabilirsiniz.  DevOps Projesi ayrıca tercih ettiğiniz **Azure aboneliğinde** **Azure kaynakları** oluşturur.

1. [Microsoft Azure portalında](https://portal.azure.com) oturum açın.

1. Sol gezinti çubuğunda **Kaynak oluştur** simgesini seçin ve ardından **DevOps Projesini** arayın.  **Oluştur**’u seçin.

    ![Sürekli Teslim yapılandırmasını başlatma](_img/azure-devops-project-python/fullbrowser.png)

## <a name="select-a-sample-application-and-azure-service"></a>Örnek uygulama ve Azure hizmeti seçme

1. **Python** örnek uygulamasını seçin.  Python örnekleri birkaç uygulama çerçevesi seçeneği içerir.

1. Varsayılan örnek çerçeve **Django**’dur. Varsayılan ayarı değiştirmeyin ve **İleri**’yi seçin.  

1. **Kapsayıcılar için Web App** varsayılan dağıtım hedefidir.  Önceki adımlarda seçtiğiniz uygulama çerçevesi, burada bulabileceğiniz Azure hizmeti dağıtımı hedefi türünü belirler.  Varsayılan hizmeti değiştirmeyin ve ardından **İleri**’yi seçin.
 
## <a name="configure-azure-devops-services-and-an-azure-subscription"></a>Azure DevOps Services ve bir Azure aboneliği yapılandırma 

1. **Yeni** bir Azure DevOps Services kuruluşu oluşturun veya **var olan** bir kuruluşu seçin.  Azure DevOps projeniz için bir **ad** seçin.  **Azure aboneliğinizi**, **konumunuzu** ve uygulamanız için bir **ad** seçin.  İşiniz bittiğinde **Bitti**’yi seçin.

1. Birkaç dakika içinde **proje panosu** Azure portalda yüklenir.  Azure DevOps Services kuruluşunuzdaki bir depoda örnek uygulama ayarlanır, bir derleme yürütülür ve uygulamanız Azure’a dağıtılır.  Bu pano **kod deponuza**, **Azure CI/CD işlem hattına** ve **Azure’daki uygulamanıza** görünürlük sağlar.  Panonun sağ tarafında çalışan uygulamanızı görüntülemek için **Gözat**’ı seçin.

    ![Pano görünümü](_img/azure-devops-project-python/dashboardnopreview.png) 
    
Azure DevOps Projesi, otomatik olarak bir CI derleme ve yayın tetiklemesi yapılandırır.  Artık en son çalışmanızı otomatik olarak web sitenize dağıtan bir CI/CD işlemiyle Python uygulaması üzerinde bir ekiple birlikte çalışmaya hazırsınız.

## <a name="commit-code-changes-and-execute-cicd"></a>Kod değişikliklerini işleme ve CI/CD’yi yürütme

Azure DevOps Projesi, Azure DevOps Services kuruluşunuzda veya GitHub hesabınızda bir Git deposu oluşturdu.  Depoyu görüntülemek ve uygulamanızda kod değişiklikleri yapmak için aşağıdaki adımları izleyin.

1. DevOps Projesi panosunun sol tarafında **ana** dalınızın bağlantısını seçin.  Bu bağlantı yeni oluşturulan Git deposuna bir görünüm açar.

1. Depo kopya URL'sini görüntülemek için tarayıcının sağ üst kısmından **Kopya**’yı seçin. Git deponuzu en sevdiğiniz IDE’de kopyalayabilirsiniz.  Sonraki birkaç adımda, kod değişiklikleri yapıp doğrudan ana dala işlemek için web tarayıcısını kullanabilirsiniz.

1. Tarayıcının sol tarafında **app/templates/app/index.html** dosyasına gidin.

1. **Düzenle**’yi seçin ve metnin bir kısmında değişiklik yapın.  Örneğin, div etiketlerinden biri için metnin bir kısmını değiştirin.

1. **İşle**’yi seçin ve ardından değişikliklerinizi kaydedin.

1. Tarayıcınızda **Azure DevOps Projesi panosuna** gidin.  Bir derlemenin sürdüğünü görüyor olmanız gerekir.  Az önce yaptığınız değişiklikler otomatik olarak bir Azure CI/CD işlem hattı yoluyla derlenir ve dağıtılır.

## <a name="examine-the-azure-cicd-pipeline"></a>Azure CI/CD işlem hattını inceleme

Azure DevOps Projesi, Azure DevOps Services kuruluşunuzda otomatik olarak tam bir Azure CI/CD işlem hattı yapılandırdı.  İşlem hattını gerektiği şekilde keşfedin ve özelleştirin.  Azure DevOps Services derleme ve yayın işlem hattı hakkında bilgi edinmek aşağıdaki adımları izleyin.

1. Azure DevOps Project panosunun**üst** kısmında **Derleme İşlem Hattı**’nı seçin.  Bu bağlantı bir tarayıcı sekmesi açar ve yeni projeniz için Azure DevOps Services derleme işlem hattını açar.

1. Fare imlecini, **Durum** alanının yanındaki derleme işlem hattının sağına götürün. Görüntülenen **üç noktayı** seçin.  Bu eylem, yeni bir derlemeyi sıraya alma, derleme duraklatma ve derleme işlem hattını düzenleme gibi birkaç etkinliği başlatabileceğiniz bir menüyü açar.

1. **Düzenle**’yi seçin.

1. Bu görünümden derleme işlem hattınızın **çeşitli görevlerini inceleyin**.  Derleme, Git deposundan kaynak getirme, bağımlılıkları geri yükleme ve dağıtım için kullanılan çıkışları yayımlama gibi çeşitli görevleri yürütür.

1. Derleme işlem hattının üst kısmında **derleme işlem hattı adı**’nı seçin.

1. Derleme işlem hattınızın **adını** daha açıklayıcı bir şeyle değiştirin.  **Kaydet ve sıraya al**’ı ve ardından **Kaydet**’i seçin.

1. Derleme işlem hattı adınızın altında **Geçmiş**’i seçin.  Derleme için yaptığınız son değişikliklere ait denetim kaydını görürsünüz.  Azure DevOps Services, derleme işlem hattında yapılan değişiklikleri izler ve sürümleri karşılaştırmanızı sağlar.

1. **Tetikleyiciler**’i seçin.  Azure DevOps Projesi otomatik olarak bir CI tetikleyicisi oluşturdu. Depoya yönelik her işleme yeni bir derleme başlatır.  İsteğe bağlı olarak dalları CI işlemine dahil etmeyi veya işlemden hariç tutmayı seçebilirsiniz.

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

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında, Azure DevOps projesi panosundaki **Sil** işlevini kullanarak bu hızlı başlangıçta oluşturulmuş olan Azure App Service’i ve ilgili kaynakları silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

CI/CD işleminizi bu hızlı başlangıçta yapılandırdığınızda, Azure DevOps Projenizde otomatik olarak bir derleme ve yayın işlem hattı oluşturulur. Ekibinizin ihtiyaçlarını karşılamak için bu derleme ve yayın işlem hatlarını değiştirebilirsiniz. Daha fazla bilgi edinmek için bu öğreticiye bakın:

> [!div class="nextstepaction"]
> [CD işlemini özelleştirme](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts)
