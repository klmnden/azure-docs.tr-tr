---
title: Azure DevOps Projesi ile ASP.NET Core Uygulamanızı Azure Kubernetes Service’a (AKS) dağıtma | Azure DevOps Öğreticisi
description: Azure DevOps Projesi, Azure’ı kullanmaya başlamayı kolaylaştırır. Azure DevOps Projesi, Azure Kubernetes Service (AKS) ile birkaç hızlı adımda ASP.NET Core Uygulamanızı dağıtmanızı kolaylaştırır.
ms.author: mlearned
ms.manager: douge
ms.prod: devops
ms.technology: devops-cicd
ms.topic: tutorial
ms.date: 07/09/2018
author: mlearned
monikerRange: vsts
ms.openlocfilehash: 55ea101b3a03fdb7fc375c4594cab36d4cd79978
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2018
ms.locfileid: "44299126"
---
# <a name="tutorial--deploy-your-aspnet-core-app-to-azure-kubernetes-service-aks-with-the-azure-devops-project"></a>Öğretici: Azure DevOps Projesi ile ASP.NET Core Uygulamanızı Azure Kubernetes Service’e (AKS) dağıtma

Azure DevOps Projesi, mevcut kodunuzu ve Git deponuzu getirdiğiniz ya da Azure’da bir sürekli tümleştirme (CI) ve sürekli teslim (CD) işlem hattı oluşturmak üzere örnek uygulamalardan birini seçtiğiniz basitleştirilmiş bir deneyim sunar.  DevOps projesi AKS gibi Azure kaynaklarını otomatik olarak oluşturur, CI/CD için derleme ve yayın işlem hattı içeren bir Azure DevOps Services yayın işlem hattı oluşturur ve yapılandırır ve ardından izleme için Azure Application Insights kaynağı oluşturur.

Yapacaklarınız:

> [!div class="checklist"]
> * ASP.NET Core Uygulaması ve AKS için Azure DevOps Projesi oluşturma
> * Azure DevOps Services ve bir Azure aboneliği yapılandırma 
> * AKS kümesini inceleme
> * Azure DevOps Services CI işlem hattını inceleme
> * Azure DevOps Services CD işlem hattını inceleme
> * Git’e değişiklikleri işleme ve Azure’a otomatik olarak dağıtma
> * Kaynakları temizleme

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/) aracılığıyla ücretsiz bir abonelik alabilirsiniz.

## <a name="create-an-azure-devops-project-for-an-aspnet-core-app-and-aks"></a>ASP.NET Core Uygulaması ve AKS için Azure DevOps Projesi oluşturma

Azure DevOps Projesi, Azure’da bir CI/CD işlem hattı oluşturur.  Yeni bir **Azure DevOps Services** kuruluşu oluşturabilir veya **var olan bir kuruluşu** kullanabilirsiniz.  Azure DevOps Projesi ayrıca tercih ettiğiniz **Azure aboneliğinde** AKS kümesi gibi **Azure kaynakları** oluşturur.

1. [Microsoft Azure portalında](https://portal.azure.com) oturum açın.

1. Sol gezinti çubuğunda **Kaynak oluştur** simgesini seçin ve ardından **DevOps Projesini** arayın.  **Oluştur**’u seçin.

    ![Sürekli Teslim Başlatılıyor](_img/azure-devops-project-aks/fullbrowser.png)

1. **.NET**'i ve sonra da **İleri**'yi seçin.

1. **Uygulama çerçevesi seçin** alanında **ASP.NET Core**'u seçin ve sonra da **İleri**'yi seçin.

1. **Kubernetes Hizmeti**’ni ve ardından **İleri**’yi seçin.  

## <a name="configure-azure-devops-services-and-an-azure-subscription"></a>Azure DevOps Services ve bir Azure aboneliği yapılandırma

1. **Yeni** bir Azure DevOps kuruluşu oluşturun veya **var olan** bir kuruluşu seçin.  Projeniz için bir **ad** seçin.  

1. **Azure aboneliğinizi** seçin.

1. Diğer Azure yapılandırma seçeneklerini görmek için **Değiştir** bağlantısını seçin ve **Kubernetes kümesi** için **düğüm sayısını** tanımlayın.  Burada, Azure hizmetlerinin türünü ve konumunu yapılandırmaya yönelik çeşitli seçenekler vardır.
 
1. Azure yapılandırma alanından çıkın ve **Bitti**'yi seçin.

1. İşlemin tamamlanması birkaç dakika sürer.  Azure DevOps Services kuruluşunuzdaki bir Azure Repos Git deposunda örnek ASP.NET Core uygulaması ayarlanır, AKS kümesi oluşturulur, bir CI/CD işlem hattı yürütülür, uygulamanız Azure’a dağıtılır.  

    Tamamlandıktan sonra, Azure portala Azure DevOps **Proje panosu** yüklenir.  **Azure DevOps Proje Panosu**'na doğrudan **Azure portalı** içindeki **Tüm kaynaklar**'dan da gidebilirsiniz.  

    Azure Repos **kod deponuza**, **Azure DevOps Services CI/CD işlem hattına** ve **AKS kümesindeki uygulamanıza** görünürlük sağlar.  Azure DevOps Services işlem hattınızda başka CI/CD seçenekleri de yapılandırabilirsiniz.  Panonun sağ tarafında çalışan uygulamanızı görüntülemek için **Gözat**’ı seçin.

## <a name="examine-the-aks-cluster"></a>AKS kümesini inceleme

Azure DevOps Projesi otomatik olarak bir AKS kümesi yapılandırır.  Kümeyi inceleyebilir ve özelleştirebilirsiniz.  AKS’ye alışmak için aşağıdaki adımları izleyin.

1. **Azure DevOps Projesi panosuna** gidin.

1. DevOps Projesi panosunun sağ tarafında **Kubernetes hizmetini** seçin.

1. AKS kümesi için bir dikey pencere açılır.  Bu görünümde **kapsayıcının durumunu izleme** ve **günlüklerde arama yapma** gibi çeşitli eylemler gerçekleştirebilir ve **Kubernetes panosunu** açabilirsiniz.

1. Ekranın sağ tarafında **Kubernetes panosunu görüntüle**’yi seçin.  İsteğe bağlı olarak Kubernetes panosunu açmak için sağlanan adımları izleyin.

## <a name="examine-the-azure-devops-services-ci-pipeline"></a>Azure DevOps Services CI işlem hattını inceleme

Azure DevOps Projesi, Azure DevOps kuruluşunuzda otomatik olarak bir Azure CI/CD işlem hattı yapılandırır.  İşlem hattını inceleyebilir ve özelleştirebilirsiniz.  Azure DevOps Services CI/CD işlem hattına alışmak için aşağıdaki adımları izleyin.

1. **Azure DevOps Projesi panosuna** gidin.

1. **Azure DevOps Projesi panosunun** **üst** kısmından **Derleme İşlem Hatları**’nı seçin.  Bu bağlantı bir tarayıcı sekmesi açar ve yeni projeniz için Azure DevOps Services derleme işlem hattını açar.

1. Fare imlecini, **Durum** alanının yanındaki derleme işlem hattının sağına götürün. Görüntülenen **üç noktayı** seçin.  Bu eylem, **yeni derlemeyi kuyruğa alma**, **derlemeyi duraklatma** ve **derleme işlem hattını düzenleme** gibi bazı etkinlikleri gerçekleştirmenizi sağlayan menüyü açar.

1. **Düzenle**’yi seçin.

1. Bu görünümden, derleme işlem hattınızın **çeşitli görevlerini inceleyin**.  Derleme; Azure DevOps Services Repos Git deposundan kaynaklar alma, bağımlılıkları geri yükleme ve dağıtımlar için kullanılan çıktıları yayımlama gibi çeşitli görevler gerçekleştirir.

1. Derleme işlem hattının üst kısmında **derleme işlem hattı adını** seçin.

1. Derleme işlem hattınızın **adını** daha açıklayıcı bir şeyle değiştirin.  **Kaydet ve sıraya al**’ı ve ardından **Kaydet**’i seçin.

1. Derleme işlem hattı adınızın altında **Geçmiş**’i seçin.  Derleme için yaptığınız son değişikliklere ait denetim kaydını görürsünüz.  Azure DevOps Services, derleme işlem hattında yapılan değişiklikleri izler ve sürümleri karşılaştırmanızı sağlar.

1. **Tetikleyiciler**’i seçin.  Azure DevOps Projesi otomatik olarak bir CI tetikleyicisi oluşturdu. Depoya yönelik her işleme yeni bir derleme başlatır.  İsteğe bağlı olarak dalları CI işlemine dahil etmeyi veya işlemden hariç tutmayı seçebilirsiniz.

1. **Saklama**’yı seçin.  Senaryonuza bağlı olarak, belirli sayıdaki derlemeleri saklayacak veya kaldıracak ilkeleri belirtebilirsiniz.

## <a name="examine-the-azure-devops-services-cd-release-pipeline"></a>Azure DevOps Services CD yayın işlem hattını inceleme

Azure DevOps Projesi, Azure DevOps Services kuruluşunuzdan Azure aboneliğinize dağıtım için gereken adımları otomatik olarak oluşturur ve yapılandırır.  Bu adımlar Azure aboneliğinizle Azure DevOps Services’ın kimliğini doğrulamak için bir Azure hizmet bağlantısı yapılandırmayı içerir.  Otomasyon bir Azure DevOps Services Yayın işlem hattı da oluşturur ve Yayın işlem hattı Azure’a CD sağlar.  Azure DevOps Services Yayın işlem hattı hakkında daha fazla inceleme yapmak için aşağıdaki adımları izleyin.

1. **Derleme ve Yayın**’ı ve ardından **Yayınlar**’ı seçin.  Azure DevOps projesi Azure’a yönelik dağıtımları yönetmek için bir Azure DevOps Services yayın işlem hattı oluşturdu.

1. Tarayıcının sol tarafında, yayın işlem hattınızın yanındaki **üç noktayı** ve ardından **Düzenle**’yi seçin.

1. Yayın işlem hattı, yayın işlemini tanımlayan bir **işlem hattı** içerir.  **Yapıtlar**’ın altında **Bırak**’ı seçin.  Önceki adımlarda incelediğiniz derleme işlem hattı, yapıt için kullanılan çıkışı üretir. 

1. **Bırak** simgesinin sağ tarafında **Sürekli dağıtım tetikleyicisi** **simgesini** seçin (şimşek şeklindedir).  Bu yayın işlem hattının etkin bir CD tetikleyicisi vardır.  Tetikleyici, her yeni derleme yapısı kullanılabilir olduğunda bir dağıtım oluşturur.  İsteğe bağlı olarak, dağıtımlarınızın el ile yürütme gerektirmesi için tetikleyiciyi devre dışı bırakabilirsiniz. 

1. Tarayıcının sağ tarafında **Yayınları görüntüle**’yi seçin.  Bu görünümde yayın geçmişi gösterilir.

1. Yayınlarınızdan birinin yanındaki **üç nokta** simgesini seçin ve sonra da **Aç**'ı seçin.  Bu görünümde keşfedilebilecek **yayın özeti**, **ilişkili iş öğeleri** ve **Testler** gibi çeşitli menüler vardır.

1. **İşlemeler**'i seçin.  Bu görünümde, belirli bir dağıtımla ilişkilendirilmiş kod işlemeleri gösterilir. Dağıtımlar arasındaki işleme farklılıklarını görmek için yayınları karşılaştırabilirsiniz.

1. **Günlükler**’i seçin.  Günlüklerde, dağıtım işlemiyle ilgili yararlı bilgiler bulunur.  Bunlar hem dağıtım sırasında hem de sonrasında görüntülenebilir.

## <a name="commit-changes-to-azure-devops-services-and-automatically-deploy-to-azure"></a>Azure DevOps Services’a değişiklikleri işleme ve Azure’a otomatik olarak dağıtma 

 > [!NOTE]
 > Aşağıdaki adımlarda, web uygulamanızda yapılan basit bir metin değişikliğiyle CI/CD işlem hattı test edilir.

Artık en son çalışmanızı otomatik olarak web sitenize dağıtan bir CI/CD işlemiyle uygulamanız üzerinde bir ekiple birlikte çalışmaya hazırsınız.  Azure DevOps Services Git deposunda yapılan her bir değişiklik Azure DevOps Services içinde bir derleme başlatır ve CD işlem hattı, değişikliklerinizi Azure’a dağıtır.  Değişikliklerinizi deponuza işlemek için aşağıdaki adımları izleyin veya başka teknikler kullanın.  Örneğin, Git deposunu tercih ettiğiniz araca veya IDE'ye **kopyalayabilir** ve ardından bu değişiklikleri depoya gönderebilirsiniz.

1. Azure DevOps Services menüsünde **Kod**'u ve **Dosyalar**'ı seçin, sonra da deponuza gidin.
1. **Views\Home** dizinine gidin, **Index.cshtml** dosyasının yanındaki **üç nokta** simgesini seçin ve sonra da **Düzenle**'yi seçin.

1. Dosyada bir değişiklik yapın; örneğin, **div etiketlerinden** birinin içinde biraz metin ekleyin.  Sağ üst kısımda **İşle**'yi seçin.  Değişikliğinizi göndermek için **İşle**'yi yeniden seçin. 

1. Birkaç dakika içinde **Azure DevOps Services’ta derleme başlatılır** ve ardından değişiklerin dağıtılması için bir yayın yürütülür.  DevOps Proje panosuyla veya Azure DevOps Services kuruluşunuzla tarayıcıda **derleme durumunu** izleyebilirsiniz.

1. Yayın tamamlandığında, değişikliklerinizi gördüğünüzü doğrulamak için tarayıcıda **uygulamanızı yenileyin**.

## <a name="clean-up-resources"></a>Kaynakları temizleme

 > [!NOTE]
 > Aşağıdaki adımlar kaynakları kalıcı olarak siler.  Bu işlevi ancak bilgi istemlerini dikkatle okuduktan sonra kullanın.

Test yapıyorsanız, oluşan ücretlerden kaçınmak için kaynakları temizleyebilirsiniz.  Artık gerekli olmadığında, Azure DevOps Projesi panosundaki **Sil** işlevini kullanarak bu öğreticide oluşturulmuş olan Azure Kubernetes kümesini ve ilgili kaynakları silebilirsiniz.  **Dikkatli olun**; sil işlevi hem Azure'da hem de Azure DevOps Services'te Azure DevOps Projesi tarafından oluşturulan verileri yok eder ve yok edildikten sonra bu verileri geri alamazsınız.

1. **Azure portalında** **Azure DevOps Projesi**'ne gidin.
2. Panonun **sağ üst** tarafında **Sil**'i seçin.  İstemi okuduktan sonra, kaynakları **kalıcı olarak silmek** için **Evet**'i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Ekibinizin gereksinimlerine uygun olarak bu derleme ve yayın işlem hatlarını istediğiniz gibi değiştirebilirsiniz. Ayrıca bu CI/CD desenini diğer işlem hatlarınızda şablon olarak kullanabilirsiniz.  Şunları öğrendiniz:

> [!div class="checklist"]
> * ASP.NET Core Uygulaması ve AKS için Azure DevOps Projesi oluşturma
> * Azure DevOps Services ve bir Azure aboneliği yapılandırma 
> * AKS kümesini inceleme
> * Azure DevOps Services CI işlem hattını inceleme
> * Azure DevOps Services CD işlem hattını inceleme
> * Git’e değişiklikleri işleme ve Azure’a otomatik olarak dağıtma
> * Kaynakları temizleme

Kubernetes panosunu kullanma hakkında daha fazla bilgi edinmek için aşağıya bakın:

> [!div class="nextstepaction"]
> [Kubernetes panosunu kullanma](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts)
