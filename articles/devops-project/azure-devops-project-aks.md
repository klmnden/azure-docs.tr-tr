---
title: Azure DevOps Projesi ile ASP.NET Core Uygulamanızı Azure Kubernetes Service’e (AKS) dağıtma | VSTS Öğreticisi
description: DevOps Projesi, Azure’u kullanmaya başlamayı kolaylaştırır. Azure DevOps projesi, Azure Kubernetes Service (AKS) ile birkaç hızlı adımda ASP.NET Core Uygulamanızı dağıtmanızı kolaylaştırır.
ms.author: mlearned
ms.manager: douge
ms.prod: devops
ms.technology: devops-cicd
ms.topic: tutorial
ms.date: 07/09/2018
author: mlearned
monikerRange: vsts
ms.openlocfilehash: 98c585cc0c6136496fe190cd3eceb27fd856af21
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37967429"
---
# <a name="tutorial--deploy-your-aspnet-core-app-to-azure-kubernetes-service-aks-with-the-azure-devops-project"></a>Öğretici: Azure DevOps Projesi ile ASP.NET Core Uygulamanızı Azure Kubernetes Service’e (AKS) dağıtma

Azure DevOps Projesi, mevcut kodunuzu ve Git deponuzu getirdiğiniz ya da Azure’da bir sürekli tümleştirme (CI) ve sürekli teslim (CD) işlem hattı oluşturmak üzere örnek uygulamalardan birini seçtiğiniz basitleştirilmiş bir deneyim sunar.  DevOps projesi AKS gibi Azure kaynaklarını otomatik olarak oluşturur, CI için derleme tanımı içeren bir VSTS yayın işlem hattı oluşturur ve yapılandırır, CD için yayın tanımı oluşturur ve ardından izleme için Azure Application Insights kaynağı oluşturur.

Yapacaklarınız:

> [!div class="checklist"]
> * ASP.NET Core Uygulaması ve AKS için Azure DevOps projesi oluşturma
> * VSTS'yi ve Azure aboneliğini yapılandırma 
> * AKS kümesini inceleme
> * VSTS CI Derleme tanımını inceleme
> * VSTS CD Release Management tanımını inceleme
> * VSTS'deki değişiklikleri işleme ve Azure'a otomatik olarak dağıtma
> * Kaynakları temizleme

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/) aracılığıyla ücretsiz bir abonelik alabilirsiniz.

## <a name="create-an-azure-devops-project-for-an-aspnet-core-app-and-aks"></a>ASP.NET Core Uygulaması ve AKS için Azure DevOps projesi oluşturma

Azure DevOps Projesi VSTS'de bir CI/CD işlem hattı oluşturur.  **Yeni VSTS** hesabı oluşturabilir veya **mevcut bir hesabı** kullanabilirsiniz.  Azure DevOps Projesi ayrıca tercih ettiğiniz **Azure aboneliğinde** AKS kümesi gibi **Azure kaynakları** oluşturur.

1. [Microsoft Azure portalında](https://portal.azure.com) oturum açın.

1. Sol gezinti çubuğunda **Kaynak oluştur** simgesini seçin ve ardından **DevOps projesini** arayın.  **Oluştur**’u seçin.

    ![Sürekli Teslim Başlatılıyor](_img/azure-devops-project-aks/fullbrowser.png)

1. **.NET**'i ve sonra da **İleri**'yi seçin.

1. **Uygulama çerçevesi seçin** alanında **ASP.NET Core**'u seçin ve sonra da **İleri**'yi seçin.

1. **Kubernetes Hizmeti**’ni ve ardından **İleri**’yi seçin.  

## <a name="configure-vsts-and-an-azure-subscription"></a>VSTS'yi ve Azure aboneliğini yapılandırma

1. **Yeni** VSTS hesabı oluşturun veya **mevcut** bir hesabı kullanın.  VSTS projeniz için bir **ad** seçin.  

1. **Azure aboneliğinizi** seçin.

1. Diğer Azure yapılandırma seçeneklerini görmek için **Değiştir** bağlantısını seçin ve **Kubernetes kümesi** için **düğüm sayısını** tanımlayın.  Burada, Azure hizmetlerinin türünü ve konumunu yapılandırmaya yönelik çeşitli seçenekler vardır.
 
1. Azure yapılandırma alanından çıkın ve **Bitti**'yi seçin.

1. İşlemin tamamlanması birkaç dakika sürer.  VSTS hesabınızdaki bir depoda örnek ASP.NET Core uygulaması ayarlanır; AKS kümesi oluşturulur; bir derleme ve yayın yürütülür; uygulamanız Azure’a dağıtılır.  

    Tamamlandıktan sonra, Azure portalına Azure DevOps **proje panosu** yüklenir.  **Azure DevOps Proje Panosu**'na doğrudan **Azure portalı** içindeki **Tüm kaynaklar**'dan da gidebilirsiniz.  

    Bu pano VSTS **kod deponuza**, **VSTS CI/CD işlem hattına** ve **AKS kümesine** görünürlük sağlar.  VSTS'de başka seçenekleri de yapılandırabilirsiniz.  Panonun sağ tarafında çalışan uygulamanızı görüntülemek için **Gözat**’ı seçin.

## <a name="examine-the-aks-cluster"></a>AKS kümesini inceleme

Azure DevOps Projesi otomatik olarak bir AKS kümesi yapılandırır.  Kümeyi inceleyebilir ve özelleştirebilirsiniz.  AKS’ye alışmak için aşağıdaki adımları izleyin.

1. **Azure DevOps Projesi panosuna** gidin.

1. DevOps projesi panosunun sağ tarafında **Kubernetes hizmeti**'ni seçin.

1. AKS kümesi için bir dikey pencere açılır.  Bu görünümde **kapsayıcının durumunu izleme** ve **günlüklerde arama yapma** gibi çeşitli eylemler gerçekleştirebilir ve **Kubernetes panosunu** açabilirsiniz.

1. Ekranın sağ tarafında **Kubernetes panosunu görüntüle**’yi seçin.  İsteğe bağlı olarak Kubernetes panosunu açmak için sağlanan adımları izleyin.

## <a name="examine-the-vsts-ci-build-definition"></a>VSTS CI Derleme tanımını inceleme

Azure DevOps Projesi, VSTS hesabınızda otomatik olarak tam bir VSTS CI/CD işlem hattı yapılandırır.  İşlem hattını inceleyebilir ve özelleştirebilirsiniz.  VSTS derleme tanımına alışmak için aşağıdaki adımları izleyin.

1. **Azure DevOps Projesi panosuna** gidin.

1. **Azure DevOps projesi panosunun** **üst** kısmından **Derleme İşlem Hatları**’nı seçin.  Bu bağlantı, bir tarayıcı sekmesi açar ve yeni projeniz için VSTS derleme tanımını açar.

1. Fare imlecini **Durum** alanının yanındaki derleme tanımının sağına taşıyın. Görüntülenen **üç noktayı** seçin.  Bu eylem, **yeni derlemeyi kuyruğa alma**, **derlemeyi duraklatma** ve **derleme tanımını düzenleme** gibi bazı etkinlikleri gerçekleştirmenizi sağlayan menüyü açar.

1. **Düzenle**’yi seçin.

1. Bu görünümden derleme tanımınızın **çeşitli görevlerini inceleyin**.  Derleme, VSTS Git deposundan kaynak getirme, bağımlılıkları geri yükleme ve dağıtım için kullanılan çıkışları yayımlama gibi çeşitli görevleri gerçekleştirir.

1. Derleme tanımının üst kısmında **derleme tanımı adını** seçin.

1. Derleme tanımınızın **adını** daha açıklayıcı bir şeyle değiştirin.  **Kaydet ve kuyruğa al**’ı ve ardından **Kaydet**’i seçin.

1. Derleme tanımı adınızın altında **Geçmiş**’i seçin.  Derleme için yaptığınız son değişikliklere ait denetim kaydını görürsünüz.  VSTS, derleme tanımında yapılan değişiklikleri izler ve yayınları karşılaştırmanızı sağlar.

1. **Tetikleyiciler**’i seçin.  Azure DevOps projesi otomatik olarak bir CI tetikleyicisi oluşturdu. Depoya yönelik her işleme yeni bir derleme başlatır.  İsteğe bağlı olarak dalları CI işlemine dahil etmeyi veya işlemden hariç tutmayı seçebilirsiniz.

1. **Saklama**’yı seçin.  Senaryonuza bağlı olarak, belirli sayıdaki derlemeleri saklayacak veya kaldıracak ilkeleri belirtebilirsiniz.

## <a name="examine-the-vsts-cd-release-management-definition"></a>VSTS CD Release Management tanımını inceleme

Azure DevOps Projesi, VSTS hesabınızdan Azure aboneliğinize dağıtım için gereken adımları otomatik olarak oluşturur ve yapılandırır.  Bu adımlar Azure aboneliğinde VSTS'nin kimliğini doğrulamak için bir Azure hizmet bağlantısı yapılandırmayı içerir.  Otomasyon bir VSTS Yayın Tanımı da oluşturur ve yayın Azure’a CD sağlar.  VSTS yayın tanımı hakkında daha fazla inceleme yapmak için aşağıdaki adımları izleyin.

1. **Derleme ve Yayın**’ı ve ardından **Yayınlar**’ı seçin.  Azure DevOps projesi Azure'a yönelik dağıtımları yönetmek için bir VSTS yayın tanımı oluşturmuştur.

1. Tarayıcının sol tarafında, yayın tanımınızın yanındaki **üç noktayı** ve ardından **Düzenle**’yi seçin.

1. Yayın tanımı, yayın işlemini tanımlayan bir **işlem hattı** içerir.  **Yapıtlar**’ın altında **Bırak**’ı seçin.  Önceki adımlarda incelediğiniz derleme tanımı, yapıt için kullanılan çıkışı üretir. 

1. **Bırak** simgesinin sağ tarafında **Sürekli dağıtım tetikleyicisi** **simgesini** seçin (şimşek şeklindedir).  Bu yayın tanımının etkin bir CD tetikleyicisi vardır.  Tetikleyici, her yeni derleme yapısı kullanılabilir olduğunda bir dağıtım oluşturur.  İsteğe bağlı olarak, dağıtımlarınızın el ile yürütme gerektirmesi için tetikleyiciyi devre dışı bırakabilirsiniz. 

1. Tarayıcının sağ tarafında **Yayınları görüntüle**’yi seçin.  Bu görünümde yayın geçmişi gösterilir.

1. Yayınlarınızdan birinin yanındaki **üç nokta** simgesini seçin ve sonra da **Aç**'ı seçin.  Bu görünümde keşfedilebilecek **yayın özeti**, **ilişkili iş öğeleri** ve **Testler** gibi çeşitli menüler vardır.

1. **İşlemeler**'i seçin.  Bu görünümde, belirli bir dağıtımla ilişkilendirilmiş kod işlemeleri gösterilir. Dağıtımlar arasındaki işleme farklılıklarını görmek için yayınları karşılaştırabilirsiniz.

1. **Günlükler**’i seçin.  Günlüklerde, dağıtım işlemiyle ilgili yararlı bilgiler bulunur.  Bunlar hem dağıtım sırasında hem de sonrasında görüntülenebilir.

## <a name="commit-changes-to-vsts-and-automatically-deploy-to-azure"></a>VSTS'deki değişiklikleri işleme ve Azure'a otomatik olarak dağıtma 

 > [!NOTE]
 > Aşağıdaki adımlarda, web uygulamanızda yapılan basit bir metin değişikliğiyle CI/CD işlem hattı test edilir.

Artık en son çalışmanızı otomatik olarak web sitenize dağıtan bir CI/CD işlemiyle uygulamanız üzerinde bir ekiple birlikte çalışmaya hazırsınız.  VSTS git deposunda yapılan her değişiklik VSTS'de bir derleme başlatır ve bir VSTS Release Management tanımı değişikliklerinizi Azure'a dağıtır.  Değişikliklerinizi deponuza işlemek için aşağıdaki adımları izleyin veya başka teknikler kullanın.  Örneğin, Git deposunu tercih ettiğiniz araca veya IDE'ye **kopyalayabilir** ve ardından bu değişiklikleri depoya gönderebilirsiniz.

1. VSTS menüsünde **Kod**'u ve **Dosyalar**'ı seçin, sonra da deponuza gidin.
1. **Views\Home** dizinine gidin, **Index.cshtml** dosyasının yanındaki **üç nokta** simgesini seçin ve sonra da **Düzenle**'yi seçin.

1. Dosyada bir değişiklik yapın; örneğin, **div etiketlerinden** birinin içinde biraz metin ekleyin.  Sağ üst kısımda **İşle**'yi seçin.  Değişikliğinizi göndermek için **İşle**'yi yeniden seçin. 

1. Birkaç dakika içinde **VSTS'de derleme başlatılır** ve ardından değişiklerin dağıtılması için bir yayın yürütülür.  DevOps proje panosuyla veya VSTS hesabınızı kullanarak tarayıcıda **derleme durumunu** izleyebilirsiniz.

1. Yayın tamamlandığında, değişikliklerinizi gördüğünüzü doğrulamak için tarayıcıda **uygulamanızı yenileyin**.

## <a name="clean-up-resources"></a>Kaynakları temizleme

 > [!NOTE]
 > Aşağıdaki adımlar kaynakları kalıcı olarak siler.  Bu işlevi ancak bilgi istemlerini dikkatle okuduktan sonra kullanın.

Test yapıyorsanız, oluşan ücretlerden kaçınmak için kaynakları temizleyebilirsiniz.  Artık gerekli olmadığında, Azure DevOps Projesi panosundaki **Sil** işlevini kullanarak bu öğreticide oluşturulmuş olan Azure Kubernetes kümesini ve ilgili kaynakları silebilirsiniz.  **Dikkatli olun**; sil işlevi hem Azure'da hem de VSTS'de Azure DevOps Projesi tarafından oluşturulan verileri yok eder ve yok edildikten sonra bu verileri geri alamazsınız.

1. **Azure portalında** **Azure DevOps Projesi**'ne gidin.
2. Panonun **sağ üst** tarafında **Sil**'i seçin.  İstemi okuduktan sonra, kaynakları **kalıcı olarak silmek** için **Evet**'i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Ekibinizin gereksinimlerine uygun olarak bu derleme ve yayın tanımlarını istediğiniz gibi değiştirebilirsiniz. Ayrıca bu CI/CD desenini diğer projelerinizde şablon olarak kullanabilirsiniz.  Şunları öğrendiniz:

> [!div class="checklist"]
> * ASP.NET Core Uygulaması ve AKS için Azure DevOps projesi oluşturma
> * VSTS'yi ve Azure aboneliğini yapılandırma 
> * AKS kümesini inceleme
> * VSTS CI Derleme tanımını inceleme
> * VSTS CD Release Management tanımını inceleme
> * VSTS'deki değişiklikleri işleme ve Azure'a otomatik olarak dağıtma
> * Kaynakları temizleme

Kubernetes panosunu kullanma hakkında daha fazla bilgi edinmek için aşağıya bakın:

> [!div class="nextstepaction"]
> [Kubernetes panosunu kullanma](https://docs.microsoft.com/vsts/pipelines/release/define-multistage-release-process?view=vsts)
