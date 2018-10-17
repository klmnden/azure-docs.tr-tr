---
title: Azure DevOps Projesi ile ASP.NET Uygulamanızı Azure Sanal Makineler'e dağıtma | Azure DevOps Services Öğreticisi
description: DevOps Projesi, Azure’u kullanmaya başlamayı kolaylaştırır. Azure DevOps Projesi, ASP.NET Uygulamanızın birkaç hızlı adımda Azure Sanal Makineleri'ne dağıtılmasını kolaylaştırır.
ms.author: mlearned
ms.manager: douge
ms.prod: devops
ms.technology: devops-cicd
ms.topic: tutorial
ms.date: 07/09/2018
author: mlearned
monikerRange: vsts
ms.openlocfilehash: b05e2c2c46aa9bfa8c92d3d3c5c83d018c547b9f
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2018
ms.locfileid: "44299143"
---
# <a name="tutorial--deploy-your-aspnet-app-to-azure-virtual-machines-with-the-azure-devops-project"></a>Öğretici: Azure DevOps Projesi ile ASP.NET Uygulamanızı Azure Sanal Makineler'e dağıtma

Azure DevOps Projesi, mevcut kodunuzu ve Git deponuzu getirdiğiniz ya da Azure’da bir sürekli tümleştirme (CI) ve sürekli teslim (CD) işlem hattı oluşturmak üzere örnek uygulamalardan birini seçtiğiniz basitleştirilmiş bir deneyim sunar.  DevOps Projesi yeni bir Azure sanal makinesi gibi Azure kaynaklarını otomatik olarak oluşturur, CI için derleme işlem hattı içeren bir Azure DevOps yayın işlem hattı oluşturur ve yapılandırır, CD için yayın işlem hattı oluşturur ve ardından izleme için Azure Application Insights kaynağı oluşturur.

Yapacaklarınız:

> [!div class="checklist"]
> * ASP.NET Uygulaması için Azure DevOps Projesi oluşturma
> * Azure DevOps Services ve bir Azure aboneliği yapılandırma 
> * Azure DevOps Services CI işlem hattını inceleme
> * Azure DevOps Services CD işlem hattını inceleme
> * Azure DevOps Services’a değişiklikleri işleme ve Azure’a otomatik olarak dağıtma
> * Application Insights izlemeyi yapılandırma
> * Kaynakları temizleme

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/) aracılığıyla ücretsiz bir abonelik alabilirsiniz.

## <a name="create-an-azure-devops-project-for-an-aspnet-app"></a>ASP.NET Uygulaması için Azure DevOps Projesi oluşturma

Azure DevOps Projesi, Azure'de bir CI/CD işlem hattı oluşturur.  **Yeni bir Azure DevOps Services** kuruluşu oluşturabilir veya **var olan bir kuruluşu** kullanabilirsiniz.  Azure DevOps Projesi ayrıca tercih ettiğiniz **Azure aboneliğinde** sanal makineler gibi **Azure kaynakları** oluşturur.

1. [Microsoft Azure portalında](https://portal.azure.com) oturum açın.

1. Sol gezinti çubuğunda **+ Yeni** simgesini seçin ve ardından **DevOps Projesini** arayın.  **Oluştur**’u seçin.

    ![Sürekli Teslim Başlatılıyor](_img/azure-devops-project-github/fullbrowser.png)

1. **.NET**'i ve sonra da **İleri**'yi seçin.

1. **Uygulama çerçevesi seçin** alanında **ASP.NET**'i seçin ve sonra da **İleri**'yi seçin. 

1. Önceki adımlarda seçtiğiniz uygulama çerçevesi, burada bulabileceğiniz Azure hizmeti dağıtımı hedefi türünü belirler.  **Sanal makine**'yi ve ardından **İleri**'yi seçin.

## <a name="configure-azure-devops-services-and-an-azure-subscription"></a>Azure DevOps Services ve bir Azure aboneliği yapılandırma

1. **Yeni** bir Azure DevOps Services kuruluşu oluşturun veya **var olan** bir kuruluşu seçin.  Azure DevOps projeniz için bir **ad** seçin.  

1. Azure **abonelik Hizmetlerinizi** seçin.  İsterseniz **Değiştir** bağlantısını seçebilir ve ardından Azure kaynaklarının konumunu değiştirme gibi başka yapılandırma ayrıntıları girebilirsiniz.
 
1. Yeni Azure sanal makine kaynağınız için **Sanal makine adı**, **Kullanıcı adı** ve **Parola** girin, sonra da **Bitti**'yi seçin.

1. Azure sanal makinesinin hazır olması birkaç dakika alabilir.  Azure DevOps Services kuruluşunuzdaki bir depoda örnek ASP.NET uygulaması ayarlanır, bir derleme ve yayın yürütülür, uygulamanız yeni oluşturulan Azure VM’sine dağıtılır.  

    Tamamlandıktan sonra, Azure portalına Azure DevOps **proje panosu** yüklenir.  **Azure DevOps Proje Panosu**'na doğrudan **Azure portalı** içindeki **Tüm kaynaklar**'dan da gidebilirsiniz.  

    Bu pano Azure DevOps Services **kod deponuza**, **Azure CI/CD işlem hattına** ve **Azure’da çalışan uygulamanıza** görünürlük sağlar.    

    ![Pano görünümü](_img/azure-devops-project-vms/dashboardnopreview.png)

1. Azure DevOps Projesi CI derlemesini ve tüm kod değişikliklerini deponuza otomatik olarak dağıtan bir yayın tetikleyicisini otomatik olarak yapılandırır.  Azure DevOps’ta başka seçenekleri de yapılandırabilirsiniz.  Panonun sağ tarafında çalışan uygulamanızı görüntülemek için **Gözat**’ı seçin.
    
## <a name="examine-the-azure-devops-services-ci-pipeline"></a>Azure DevOps Services CI işlem hattını inceleme
 
Azure DevOps Projesi, Azure DevOps Services kuruluşunuzda otomatik olarak tam bir Azure CI/CD işlem hattı yapılandırır.  İşlem hattını inceleyebilir ve özelleştirebilirsiniz.  Azure DevOps Services derleme işlem hattına alışmak için aşağıdaki adımları izleyin.

1. **Azure DevOps Projesi panosunun** **üst** kısmından **Derleme İşlem Hatları**’nı seçin.  Bu bağlantı bir tarayıcı sekmesi açar ve yeni projeniz için Azure DevOps Services derleme işlem hattını açar.

1. Fare imlecini **Durum** alanının yanındaki derleme işlem hattının sağına taşıyın. Görüntülenen **üç noktayı** seçin.  Bu eylem, **yeni derlemeyi kuyruğa alma**, **derlemeyi duraklatma** ve **derleme işlem hattını düzenleme** gibi bazı etkinlikleri gerçekleştirmenizi sağlayan menüyü açar.

1. **Düzenle**’yi seçin.

1. Bu görünümden derleme işlem hattınızın **çeşitli görevlerini inceleyin**.  Derleme, Azure DevOps Services Git deposundan kaynak getirme, bağımlılıkları geri yükleme ve dağıtım için kullanılan çıkışları yayımlama gibi çeşitli görevleri yürütür.

1. Derleme işlem hattının üst kısmında **derleme işlem hattı adını** seçin.

1. Derleme işlem hattınızın **adını** daha açıklayıcı bir şeyle değiştirin.  **Kaydet ve sıraya al**’ı ve ardından **Kaydet**’i seçin.

1. Derleme işlem hattı adınızın altında **Geçmiş**’i seçin.  Derleme için yaptığınız son değişikliklere ait denetim kaydını görürsünüz.  Azure DevOps Services, derleme işlem hattında yapılan değişiklikleri izler ve sürümleri karşılaştırmanızı sağlar.

1. **Tetikleyiciler**’i seçin.  Azure DevOps Projesi otomatik olarak bir CI tetikleyicisi oluşturur ve depoya yönelik her işleme yeni bir derleme başlatır.  İsteğe bağlı olarak dalları CI işlemine dahil etmeyi veya işlemden hariç tutmayı seçin.

1. **Saklama**’yı seçin.  Senaryonuza bağlı olarak, belirli sayıdaki derlemeleri saklayacak veya kaldıracak ilkeleri belirtebilirsiniz.

## <a name="examine-the-azure-devops-services-cd-pipeline"></a>Azure DevOps Services CD işlem hattını inceleme

Azure DevOps Projesi, Azure DevOps Services kuruluşunuzdan Azure aboneliğinize dağıtım için gereken adımları otomatik olarak oluşturur ve yapılandırır.  Bu adımlar Azure aboneliğinde Azure DevOps Services’ın kimliğini doğrulamak için bir Azure hizmet bağlantısı yapılandırmayı içerir.  Otomasyon, ayrıca bir Azure DevOps Services CD işlem hattı oluşturur ve bu işlem hattı Azure sanal makinesine CD sağlar.  Azure DevOps Services CD işlem hattı hakkında daha fazla inceleme yapmak için aşağıdaki adımları izleyin.

1. **Derleme ve Yayın**’ı ve ardından **Yayınlar**’ı seçin.  Azure DevOps projesi Azure'a yönelik dağıtımları yönetmek için bir Azure DevOps Services yayın işlem hattı oluşturdu.

1. Tarayıcının sol tarafında, yayın işlem hattınızın yanındaki **üç noktayı** ve ardından **Düzenle**’yi seçin.

1. Yayın işlem hattı, yayın işlemini tanımlayan bir **işlem hattı** içerir.  **Yapıtlar**’ın altında **Bırak**’ı seçin.  Önceki adımlarda incelediğiniz derleme işlem hattı, yapıt için kullanılan çıkışı üretir. 

1. **Bırak** simgesinin sağ tarafında **Sürekli dağıtım tetikleyicisi** **simgesini** seçin (şimşek şeklindedir).  Bu yayın işlem hattının etkin bir CD tetikleyicisi vardır.  Tetikleyici, her yeni derleme yapısı kullanılabilir olduğunda bir dağıtım başlatır.  İsteğe bağlı olarak, dağıtımlarınızın el ile yürütme gerektirmesi için tetikleyiciyi devre dışı bırakabilirsiniz. 

1. Tarayıcının sol tarafında **Görevler**’i ve **ortamınızı** seçin.  

1. Görevler, dağıtım işleminizin yürütüldüğü etkinliklerdir ve **Aşamalar** halinde gruplandırılırlar.  Bu yayın işlem hattı için iki **Aşama** vardır.  İlk aşama dağıtım için VM'yi yapılandıran **Azure Kaynak Grubu Dağıtımı** görevini içerir ve yeni VM'yi **Azure DevOps Dağıtım Grubu**'na ekler.  Azure DevOps’taki VM dağıtım grubu, **dağıtım hedefi** makinelerinin mantıksal gruplarını yönetir.

1. İkinci aşamada, VM'de IIS Web Sitesi oluşturmak için **IIS Web Uygulamasını Yönetme** görevi oluşturulur.  Siteyi dağıtmak için ikinci bir **IIS Web Uygulamasını Dağıtma** görevi oluşturulur.

1. Tarayıcının sağ tarafında **Yayınları görüntüle**’yi seçin.  Bu görünümde yayın geçmişi gösterilir.

1. Yayınlarınızdan birinin yanındaki **üç nokta** simgesini seçin ve sonra da **Aç**'ı seçin.  Bu görünümde keşfedilebilecek **yayın özeti**, **ilişkili iş öğeleri** ve **Testler** gibi çeşitli menüler vardır.

1. **İşlemeler**'i seçin.  Bu görünümde, belirli bir dağıtımla ilişkilendirilmiş kod işlemeleri gösterilir. Dağıtımlar arasındaki işleme farklılıklarını görmek için yayınları karşılaştırın.

1. **Günlükler**’i seçin.  Günlüklerde, dağıtım işlemiyle ilgili yararlı bilgiler bulunur.  Bunlar hem dağıtım sırasında hem de sonrasında görüntülenebilir.

## <a name="commit-changes-to-azure-devops-services-and-automatically-deploy-to-azure"></a>Azure DevOps Services’a değişiklikleri işleme ve Azure’a otomatik olarak dağıtma 

Artık en son çalışmanızı otomatik olarak web sitenize dağıtan bir CI/CD işlemiyle uygulamanız üzerinde bir ekiple birlikte çalışmaya hazırsınız.  Azure DevOps Services Git deposunda yapılan her bir değişiklik Azure DevOps Services içinde bir derleme başlatır ve bir Azure DevOps Services CD işlem hattı Azure VM’nize bir dağıtım yürütür.  Değişikliklerinizi deponuza işlemek için aşağıdaki adımları izleyin veya başka teknikler kullanın.  Kod değişiklikleri CI/CD işlemini başlatır ve yeni değişikliklerinizi otomatik olarak Azure VM’sinde IIS web sitesine dağıtır.

1. Azure DevOps Services menüsünde **Kod**'u seçin, sonra da deponuza gidin.

1. **Views\Home** dizinine gidin, **Index.cshtml** dosyasının yanındaki **üç nokta** simgesini seçin ve sonra da **Düzenle**'yi seçin.

1. Dosyada bir değişiklik yapın; örneğin, **div etiketlerinden** birinin içinde biraz metin ekleyin.  Sağ üst kısımda **İşle**'yi seçin.  Değişikliğinizi göndermek için **İşle**'yi yeniden seçin. 

1. Birkaç dakika içinde **Azure DevOps Services üzerinde derleme başlatılır** ve ardından değişiklerin dağıtılması için bir yayın yürütülür.  DevOps Proje panosuyla veya Azure DevOps Services kuruluşunuzla tarayıcıda **derleme durumunu** izleyebilirsiniz.

1. Yayın tamamlandığında, değişikliklerinizi gördüğünüzü doğrulamak için tarayıcıda **uygulamanızı yenileyin**.

## <a name="configure-azure-application-insights-monitoring"></a>Application Insights izlemeyi yapılandırma

Azure Application Insights ile, uygulamanızın performansını ve kullanımını kolayca izleyebilirsiniz.  Azure DevOps Projesi uygulamanız için otomatik olarak bir Application Insights kaynağı yapılandırır.  Gerekirse başka uyarılar ve izleme özellikleri de yapılandırabilirsiniz.

1. **Azure portalında** **Azure DevOps Projesi** panosuna gidin.  Panonun sağ alt kısmında uygulamanız için **Application Insights** bağlantısını seçin.

1. Azure portalında **Application Insights** dikey penceresi açılır.  Bu görünüm uygulamanızın kullanım, performans ve kullanılabilirlik izleme bilgilerini içerir.

    ![Application Insights](_img/azure-devops-project-github/appinsights.png) 

1. **Zaman aralığı**'nı ve ardından **Son saat**'i seçin.  Sonuçları filtrelemek için **Güncelleştir**'i seçin.  Şimdi son 60 dakikanın tüm etkinliğini görürsünüz.  Zaman aralığından çıkmak için **x** işaretini seçin.

1. Panonun üst kısmında **Uyarılar**'ı ve başka kullanışlı bağlantıları bulabilirsiniz.  **Uyarılar**'ı ve ardından **+ Ölçüm uyarısı ekle**'yi seçin.

1. Uyarı için bir **Ad** girin.

1. Varsayılan uyarı, **1 saniyeden uzun sunucu yanıt süresi** içindir.  Çeşitli uyarı ölçümlerini incelemek için **Ölçüm** açılan listesini seçin.  Örneğin, ASP.NET uygulaması için **ASP.NET isteği yürütme süresini** yapılandırabilirsiniz.  Çeşitli uyarıları kolayca yapılandırıp uygulamanızın izleme özelliklerini geliştirebilirsiniz.

1. **E-posta sahipleri, katkıda bulunanlar ve okuyucular yoluyla bildir** onay kutusunu seçin.  İsteğe bağlı olarak, Azure mantıksal uygulamasını yürüterek bir uyarı verildiğinde ek eylemler gerçekleştirebilirsiniz.

1. Uyarıyı oluşturmak için **Tamam**'ı seçin.  Birkaç dakika içinde uyarı panoda etkin olarak gösterilir.  Uyarılar alanından **çıkın** ve **Application Insights dikey penceresine** dönün.

1. **Kullanılabilirlik** öğesini ve ardından **+ Test ekle**'yi seçin. 

1. **Test adı** girin ve **Oluştur**'u seçin.  Bu işlem, uygulamanızın kullanılabilirliğini doğrulamak için basit bir ping testi oluşturur.  Birkaç dakika sonra, test sonuçları sağlanır ve Application Insights panosu bir kullanılabilirlik durumu görüntüler.

## <a name="clean-up-resources"></a>Kaynakları temizleme

 > [!NOTE]
 > Aşağıdaki adımlar kaynakları kalıcı olarak siler.  Bu işlevi ancak bilgi istemlerini dikkatle okuduktan sonra kullanın.

Test yapıyorsanız,tahakkuk eden ücretleri ödemekten kaçınmak için kaynakları temizleyebilirsiniz.  Artık gerekli olmadığında, Azure DevOps Projesi panosundaki **Sil** işlevini kullanarak bu öğreticide oluşturulmuş olan Azure sanal makinesini ve ilgili kaynakları silebilirsiniz.  **Dikkatli olun**. Sil işlevi hem Azure'da hem de Azure DevOps’ta Azure DevOps Projesi tarafından oluşturulan verileri yok eder ve yok edildikten sonra bu verileri geri alamazsınız.

1. **Azure portalında** **Azure DevOps Projesi**'ne gidin.
2. Panonun **sağ üst** tarafında **Sil**'i seçin.  İstemi okuduktan sonra, kaynakları **kalıcı olarak silmek** için **Evet**'i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Ekibinizin gereksinimlerine uygun olarak bu derleme ve yayın işlem hatlarını istediğiniz gibi değiştirebilirsiniz. Ayrıca bu CI/CD desenini diğer projelerinizde şablon olarak kullanabilirsiniz.  Şunları öğrendiniz:

> [!div class="checklist"]
> * ASP.NET Uygulaması için Azure DevOps Projesi oluşturma
> * Azure DevOps Services ve bir Azure aboneliği yapılandırma 
> * Azure DevOps Services CI işlem hattını inceleme
> * Azure DevOps Services CD işlem hattını inceleme
> * Azure DevOps Services’a değişiklikleri işleme ve Azure’a otomatik olarak dağıtma
> * Application Insights izlemeyi yapılandırma
> * Kaynakları temizleme

Azure CI/CD işlem hattı hakkında daha fazla bilgi için şu öğreticiye bakın:

> [!div class="nextstepaction"]
> [CD işlemini özelleştirme](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts)
