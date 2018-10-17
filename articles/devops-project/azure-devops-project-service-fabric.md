---
title: Azure DevOps Projesi ile ASP.NET Core Uygulamanızı Azure Service Fabric'e dağıtma | Azure DevOps Services Öğreticisi
description: DevOps Projesi, Azure’u kullanmaya başlamayı kolaylaştırır. Azure DevOps Projesi, Azure Service Fabric ile birkaç hızlı adımda ASP.NET Core Uygulamanızı dağıtmanızı kolaylaştırır.
ms.author: mlearned
ms.manager: douge
ms.prod: devops
ms.technology: devops-cicd
ms.topic: tutorial
ms.date: 07/09/2018
author: mlearned
monikerRange: vsts
ms.openlocfilehash: 6a0539b2213b99e09021a4f90d914172eac62560
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2018
ms.locfileid: "44300409"
---
# <a name="tutorial--deploy-your-aspnet-core-app-to-azure-service-fabric-with-the-azure-devops-project"></a>Öğretici: Azure DevOps Projesi ile ASP.NET Core Uygulamanızı Azure Service Fabric'e dağıtma

Azure DevOps Projesi, mevcut kodunuzu ve Git deponuzu getirdiğiniz ya da Azure’da bir sürekli tümleştirme (CI) ve sürekli teslim (CD) işlem hattı oluşturmak üzere örnek uygulamalardan birini seçtiğiniz basitleştirilmiş bir deneyim sunar.  DevOps Projesi Azure Service Fabric gibi Azure kaynaklarını otomatik olarak oluşturur, Azure DevOps’ta bir CI/CD işlem hattı ayarlamayı içeren bir yayın işlem hattı oluşturur ve yapılandırır ve ardından izleme için Azure Application Insights kaynağı oluşturur.

Yapacaklarınız:

> [!div class="checklist"]
> * ASP.NET Core Uygulaması ve Service Fabric için Azure DevOps Projesi oluşturma
> * Azure DevOps Services ve bir Azure aboneliği yapılandırma 
> * CI işlem hattını inceleme
> * CD işlem hattını inceleme
> * Git’e değişiklikleri işleme ve Azure’a otomatik olarak dağıtma
> * Kaynakları temizleme

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/) aracılığıyla ücretsiz bir abonelik alabilirsiniz.

## <a name="create-an-azure-devops-project-for-an-aspnet-core-app-and-service-fabric"></a>ASP.NET Core Uygulaması ve Service Fabric için Azure DevOps Projesi oluşturma

Azure DevOps Projesi bir CI/CD işlem hattı oluşturur.  **Yeni bir Azure DevOps Services** kuruluşu oluşturabilir veya **var olan bir kuruluşu** kullanabilirsiniz.  Azure DevOps Projesi ayrıca tercih ettiğiniz **Azure aboneliğinde** Service Fabric kümesi gibi **Azure kaynakları** oluşturur.

1. [Microsoft Azure portalında](https://portal.azure.com) oturum açın.

1. Sol gezinti çubuğunda **Kaynak oluştur** simgesini seçin ve ardından **DevOps Projesini** arayın.  **Oluştur**’u seçin.

    ![Sürekli Teslim Başlatılıyor](_img/azure-devops-project-service-fabric/fullbrowser.png)

1. **.NET**'i ve sonra da **İleri**'yi seçin.

1. **Uygulama çerçevesi seçin** alanında **ASP.NET Core**'u seçin ve sonra da **İleri**'yi seçin.

1. **Service Fabric Kümesi**'ni ve ardından **İleri**'yi seçin.  

## <a name="configure-azure-devops-services-and-an-azure-subscription"></a>Azure DevOps Services ve bir Azure aboneliği yapılandırma

1. **Yeni** bir Azure DevOps Services kuruluşu oluşturun veya **var olan** bir kuruluşu seçin.  Azure DevOps projeniz için bir **ad** seçin.  

1. **Azure aboneliğinizi** seçin.

1. Ek Azure yapılandırma ayarlarını görmek için **Değiştir** bağlantısını seçin ve **Service Fabric kümesi** için **düğüm sanal makine boyutu** ve **işletim sistemi** belirtin.  Burada, Azure hizmetlerinin türünü ve konumunu yapılandırmaya yönelik çeşitli seçenekler vardır.
 
1. Azure yapılandırma alanından çıkın ve **Bitti**'yi seçin.

1. İşlemin tamamlanması birkaç dakika sürer.  Azure DevOps Services kuruluşunuzdaki bir depoda örnek ASP.NET Core uygulaması ayarlanır, Service Fabric kümesi oluşturulur, bir CI/CD işlem hattı yürütülür, uygulamanız Azure’a dağıtılır.  

    Tamamlandıktan sonra, Azure portala Azure DevOps **Proje panosu** yüklenir.  **Azure DevOps Proje Panosu**'na doğrudan **Azure portalı** içindeki **Tüm kaynaklar**'dan da gidebilirsiniz.  

    Bu pano Azure DevOps Services **kod deponuza**, **Azure DevOps Services CI/CD işlem hattına** ve **Service Fabric kümesine** görünürlük sağlar.  Azure DevOps Services üzerinde başka seçenekleri de yapılandırabilirsiniz.  Panonun sağ tarafında çalışan uygulamanızı görüntülemek için **Gözat**’ı seçin.

## <a name="examine-the-azure-devops-services-ci-pipeline"></a>Azure DevOps Services CI işlem hattını inceleme

Azure DevOps Projesi, Azure DevOps Services kuruluşunuzda otomatik olarak bir Azure DevOps Services CI/CD işlem hattı yapılandırır.  İşlem hattını inceleyebilir ve özelleştirebilirsiniz.  Azure DevOps Services derleme işlem hattına alışmak için aşağıdaki adımları izleyin.

1. **Azure DevOps Projesi panosuna** gidin.

1. **Azure DevOps Projesi panosunun** **üst** kısmından **Derleme İşlem Hatları**’nı seçin.  Bu bağlantı bir tarayıcı sekmesi açar ve yeni projeniz için Azure DevOps Services derleme işlem hattını açar.

1. Fare imlecini **Durum** alanının yanındaki derleme işlem hattının sağına taşıyın. Görüntülenen **üç noktayı** seçin.  Bu eylem, **yeni bir derlemeyi sıraya alma**, **derleme duraklatma** ve **derleme işlem hattını düzenleme** gibi birkaç etkinliği başlatabileceğiniz bir menüyü açar.

1. **Düzenle**’yi seçin.

1. Bu görünümden derleme işlem hattınızın **çeşitli görevlerini inceleyin**.  Derleme, Azure DevOps Services Git deposundan kaynak getirme, bağımlılıkları geri yükleme ve dağıtım için kullanılan çıkışları yayımlama gibi çeşitli görevleri yürütür.

1. Derleme işlem hattının üst kısmında **derleme işlem hattı adını** seçin. 

1. Derleme işlem hattı adınızın altında **Geçmiş**’i seçin.  Derleme için yaptığınız son değişikliklere ait denetim kaydını görürsünüz.  Azure DevOps Services, derleme işlem hattında yapılan değişiklikleri izler ve sürümleri karşılaştırmanızı sağlar.

1. **Tetikleyiciler**’i seçin.  Azure DevOps Projesi otomatik olarak bir CI tetikleyicisi oluşturur ve depoya yönelik her işleme yeni bir derleme başlatır.  İsteğe bağlı olarak dalları CI işlemine dahil etmeyi veya işlemden hariç tutmayı seçebilirsiniz.

1. **Saklama**’yı seçin.  Senaryonuza bağlı olarak, belirli sayıdaki derlemeleri saklayacak veya kaldıracak ilkeleri belirtebilirsiniz.

## <a name="examine-the-azure-devops-services-cd-pipeline"></a>Azure DevOps Services CD işlem hattını inceleme

Azure DevOps Projesi, Azure DevOps Services kuruluşunuzdan Azure aboneliğinize dağıtım için gereken adımları otomatik olarak oluşturur ve yapılandırır.  Bu adımlar Azure aboneliğinde Azure DevOps Services’ın kimliğini doğrulamak için bir Azure hizmet bağlantısı yapılandırmayı içerir.  Otomasyon bir Azure DevOps Services Yayın işlem hattı da oluşturur bu yayın Azure'a CD sağlar.  Azure DevOps Services Yayın işlem hattı hakkında daha fazla inceleme yapmak için aşağıdaki adımları izleyin.

1. **Derleme ve Yayın**’ı ve ardından **Yayınlar**’ı seçin.  Azure DevOps projesi Azure'a yönelik dağıtımları yönetmek için bir Azure DevOps Services yayın işlem hattı oluşturdu.

1. Tarayıcının sol tarafında, yayın işlem hattınızın yanındaki **üç noktayı** ve ardından **Düzenle**’yi seçin.

1. Yayın işlem hattı, yayın işlemini tanımlayan bir **işlem hattı** içerir.  **Yapıtlar**’ın altında **Bırak**’ı seçin.  Önceki adımlarda incelediğiniz derleme işlem hattı, yapıt için kullanılan çıkışı üretir. 

1. **Bırak** simgesinin sağ tarafında **Sürekli dağıtım tetikleyicisi** **simgesini** seçin (şimşek şeklindedir).  Bu yayın işlem hattının etkin bir CD tetikleyicisi vardır.  Tetikleyici, her yeni derleme yapısı kullanılabilir olduğunda bir dağıtım başlatır.  İsteğe bağlı olarak, dağıtımlarınızın el ile yürütme gerektirmesi için tetikleyiciyi devre dışı bırakabilirsiniz. 

1. Tarayıcının sağ tarafında **Yayınları görüntüle**’yi seçin.  Bu görünümde yayın geçmişi gösterilir.

1. Yayınlarınızdan birinin yanındaki **üç nokta** simgesini seçin ve sonra da **Aç**'ı seçin.  Bu görünümde keşfedilebilecek **yayın özeti**, **ilişkili iş öğeleri** ve **Testler** gibi çeşitli menüler vardır.

1. **İşlemeler**'i seçin.  Bu görünümde, belirli bir dağıtımla ilişkilendirilmiş kod işlemeleri gösterilir. Dağıtımlar arasındaki işleme farklılıklarını görmek için yayınları karşılaştırabilirsiniz.

1. **Günlükler**’i seçin.  Günlüklerde, dağıtım işlemiyle ilgili yararlı bilgiler bulunur.  Bunlar hem dağıtım sırasında hem de sonrasında görüntülenebilir.

## <a name="commit-changes-to-git-and-automatically-deploy-to-azure"></a>Git’e değişiklikleri işleme ve Azure’a otomatik olarak dağıtma 

 > [!NOTE]
 > Aşağıdaki adımlarda, web uygulamanızda yapılan basit bir metin değişikliğiyle CI/CD işlem hattı test edilir.

Artık en son çalışmanızı otomatik olarak web sitenize dağıtan bir CI/CD işlemiyle uygulamanız üzerinde bir ekiple birlikte çalışmaya hazırsınız.  Git deposunda yapılan her değişiklik bir derleme başlatır ve bir yayın değişikliklerinizi Azure’a dağıtır.  Değişikliklerinizi deponuza işlemek için aşağıdaki adımları izleyin veya başka teknikler kullanın.  Örneğin, Git deposunu tercih ettiğiniz araca veya IDE'ye **kopyalayabilir** ve ardından bu değişiklikleri depoya gönderebilirsiniz.

1. Azure DevOps Services menüsünde **Kod**'u ve **Dosyalar**'ı seçin, sonra da deponuza gidin.
1. **Views\Home** dizinine gidin, **Index.cshtml** dosyasının yanındaki **üç nokta** simgesini seçin ve sonra da **Düzenle**'yi seçin.

1. Dosyada bir değişiklik yapın; örneğin, **div etiketlerinden** birinin içinde biraz metin ekleyin.  Sağ üst kısımda **İşle**'yi seçin.  Değişikliğinizi göndermek için **İşle**'yi yeniden seçin. 

1. Birkaç dakika içinde **derleme başlatılır** ve ardından değişiklerin dağıtılması için bir yayın yürütülür.  DevOps proje panosuyla veya Azure DevOps Services gerçek zamanlı günlük tutma ile tarayıcıda **derleme durumunu** izleyebilirsiniz.

1. Yayın tamamlandığında, değişikliklerinizi gördüğünüzü doğrulamak için tarayıcıda **uygulamanızı yenileyin**.

## <a name="clean-up-resources"></a>Kaynakları temizleme

 > [!NOTE]
 > Aşağıdaki adımlar kaynakları kalıcı olarak siler.  Bu işlevi ancak bilgi istemlerini dikkatle okuduktan sonra kullanın.

Test yapıyorsanız,tahakkuk eden ücretleri ödemekten kaçınmak için kaynakları temizleyebilirsiniz.  Artık gerekli olmadığında, Azure DevOps Projesi panosundaki **Sil** işlevini kullanarak bu öğreticide oluşturulmuş olan Azure Service Fabric kümesini ve ilgili kaynakları silebilirsiniz.  **Dikkatli olun**. Sil işlevi hem Azure hem de Azure DevOps Services üzerinde Azure DevOps Projesi tarafından oluşturulan verileri yok eder ve yok edildikten sonra bu verileri geri alamazsınız.

1. **Azure portalında** **Azure DevOps Projesi**'ne gidin.
2. Panonun **sağ üst** tarafında **Sil**'i seçin.  İstemi okuduktan sonra, kaynakları **kalıcı olarak silmek** için **Evet**'i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Ekibinizin gereksinimlerine uygun olarak bu Azure CI/CD işlem hatlarını istediğiniz gibi değiştirebilirsiniz. Ayrıca bu CI/CD desenini diğer projelerinizde şablon olarak kullanabilirsiniz.  Şunları öğrendiniz:

> [!div class="checklist"]
> * ASP.NET Core Uygulaması ve Service Fabric için Azure DevOps Projesi oluşturma
> * Azure DevOps Services ve bir Azure aboneliği yapılandırma 
> * CI işlem hattını inceleme
> * CD işlem hattını inceleme
> * Git’e değişiklikleri işleme ve Azure’a otomatik olarak dağıtma
> * Kaynakları temizleme

Service Fabric ve mikro hizmetler hakkında daha fazla bilgi edinmek için aşağıya bakın:

> [!div class="nextstepaction"]
> [Uygulamaları oluşturmak için bir mikro hizmetler yaklaşımı kullanma](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts)
