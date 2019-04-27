---
title: "Öğretici: Azure DevOps projeleri'ni kullanarak ASP.NET uygulamanızı Azure sanal makineleri dağıtma"
description: DevOps projeleri, Azure'da kullanmaya başlayın ve birkaç Hızlı adımda Azure sanal makinelerine ASP.NET uygulamanızı dağıtmak için kolaylaştırır.
ms.author: mlearned
ms.manager: douge
ms.prod: devops
ms.technology: devops-cicd
ms.topic: tutorial
ms.date: 07/09/2018
author: mlearned
monikerRange: vsts
ms.openlocfilehash: 05643f342d51d99645d3c9204d6e63adcf2a0a73
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60546495"
---
# <a name="tutorial-deploy-your-aspnet-app-to-azure-virtual-machines-by-using-azure-devops-projects"></a>Öğretici: Azure DevOps projeleri'ni kullanarak ASP.NET uygulamanızı Azure sanal makineleri dağıtma

Azure DevOps projeleri, mevcut kodunuzu ve Git deposuna taşıyın veya sürekli tümleştirme (CI) ve azure'a sürekli teslim (CD) işlem hattı oluşturmak için örnek bir uygulama seçin basitleştirilmiş bir deneyim sunar. 

DevOps projeleri ayrıca:
* Yeni bir Azure sanal makine (VM) gibi Azure kaynaklarını otomatik olarak oluşturur.
* Oluşturur ve bir yayın ardışık düzeni için CI derleme işlem hattı içeren Azure DevOps yapılandırır.
* Bir yayın ardışık düzeni ' için CD ayarlar. 
* İzleme için Azure Application Insights kaynağı oluşturur.

Bu öğreticide şunları yapacaksınız:

> [!div class="checklist"]
> * ASP.NET uygulamanızı dağıtmak için DevOps projeleri'ni kullanın
> * Azure DevOps ve Azure aboneliğinin yapılandırın 
> * CI işlem hattını inceleme
> * CD işlem hattını inceleme
> * Değişiklikleri Azure depolara ve otomatik olarak Azure'a dağıtın
> * Application Insights izlemeyi yapılandırma
> * Kaynakları temizleme

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/) aracılığıyla ücretsiz bir abonelik alabilirsiniz.

## <a name="use-devops-projects-to-deploy-your-aspnet-app"></a>ASP.NET uygulamanızı dağıtmak için DevOps projeleri'ni kullanın

DevOps projeleri, Azure işlem hatlarında bir CI/CD işlem hattı oluşturur. Yeni bir Azure DevOps kuruluş oluşturun veya mevcut bir kuruluşa kullanın. DevOps projeleri, sanal makineler gibi Azure kaynaklarının da tercih ettiğiniz bir Azure aboneliği oluşturur.

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Sol bölmede seçin **yeni**.

1. Arama kutusuna **DevOps projeleri**ve ardından **Oluştur**.

    ![DevOps projeleri Panosu](_img/azure-devops-project-github/fullbrowser.png)

1. Seçin **.NET**ve ardından **sonraki**.

1. Altında **bir uygulama çerçevesi seçin**seçin **ASP.NET**ve ardından **sonraki**.  
    Bir önceki adımda seçtiğiniz uygulama çerçevesi buradan kullanılabilir Azure hizmeti dağıtımı hedef türünü belirler. 

1. Sanal makineyi seçin ve ardından **sonraki**.

## <a name="configure-azure-devops-and-an-azure-subscription"></a>Azure DevOps ve Azure aboneliğinin yapılandırın

1. Yeni bir Azure DevOps kuruluş oluşturun veya mevcut bir kuruluşa seçin. 

1. Azure DevOps projeniz için bir ad girin. 

1. Azure aboneliği hizmetlerinizi seçin.  
    İsteğe bağlı olarak seçebileceğiniz **değişiklik** ve Azure kaynaklarını konumu gibi daha fazla yapılandırma ayrıntılarını girin.
 
1. Yeni bir Azure sanal makine kaynak sanal makine adı, kullanıcı adı ve parola girin ve ardından **Bitti**.  
    Birkaç dakika sonra Azure sanal makine hazır hale gelirsiniz. Bir derleme, Azure DevOps kuruluşunuzdaki bir depodaki bir örnek ASP.NET uygulaması ayarlayın ve sürüm yürütülür ve uygulamanız için yeni oluşturulan Azure VM dağıtılır. 

    DevOps projeleri Pano, tamamlandıktan sonra Azure portalında görüntülenir. Doğrudan panoya da gidebilirsiniz **tüm kaynakları** Azure portalında. 

    Pano, Azure DevOps kod deposu, CI/CD işlem hattınızı ve çalışan uygulamanızda Azure görünürlük sağlar.   

    ![Pano görünümü](_img/azure-devops-project-vms/dashboardnopreview.png)

DevOps projeleri bir CI yapısı otomatik olarak yapılandırır ve deponuza kod dağıtan bir yayın tetikleyicisi değiştirir. Azure DevOps’ta başka seçenekleri de yapılandırabilirsiniz. Çalışan uygulamanızı görüntülemek için seçin **Gözat**.
    
## <a name="examine-the-ci-pipeline"></a>CI işlem hattını inceleme
 
DevOps projeleri, bir CI/CD işlem hattı Azure işlem hatlarında otomatik olarak yapılandırılır. İşlem hattını inceleyebilir ve özelleştirebilirsiniz. Derleme işlem hattı çalıştırmasıyla tanımak için aşağıdakileri yapın:

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
    Bu bölme bir denetim kaydı derleme için en son değişikliği görüntüler. Azure DevOps derleme işlem hattı için yapılan değişiklikleri izler ve sürümleri karşılaştırmanızı sağlar.

1. **Tetikleyiciler**’i seçin.  
    DevOps projeleri CI tetikleyicisini otomatik olarak oluşturur ve depoya her işleme, yeni bir derleme başlar. İsteğe bağlı olarak, dahil etmek veya dallar CI işleminden hariç tutmak seçim yapabilirsiniz.

1. **Saklama**’yı seçin.  
    Senaryonuza bağlı olarak, saklamak veya belirli bir sayıda derlemeleri kaldırmak için ilkeleri belirtebilirsiniz.

## <a name="examine-the-cd-pipeline"></a>CD işlem hattını inceleme

DevOps projeleri, otomatik olarak oluşturur ve Azure DevOps kuruluşunuzdan Azure aboneliğinize dağıtmak için gerekli adımları yapılandırır. Bu adımlar, Azure DevOps, Azure aboneliğiniz için kimlik doğrulaması için bir Azure hizmet bağlantısı yapılandırmayı içerir. Otomasyon, ayrıca Azure sanal makinesi için CD sağlayan bir CD işlem hattı oluşturur. Azure DevOps CD işlem hattı hakkında daha fazla bilgi için aşağıdakileri yapın:

1. Seçin **derleme ve yayın**ve ardından **yayınlar**.  
    DevOps projeleri, azure'da dağıtımlarını yönetmek için bir yayın ardışık düzeni oluşturur.

1. Yayın işlem hattınızı yanındaki üç nokta (...) seçin ve ardından **Düzenle**.  
    Yayın işlem hattı, yayın işlemini tanımlayan bir *işlem hattı* içerir.

1. **Yapıtlar**’ın altında **Bırak**’ı seçin.  
    Önceki adımlarda incelenirken derleme işlem hattı, yapıt için kullanılan bir çıktı üretir. 

1. Yanındaki **bırak** simgesini seçme **sürekli dağıtım tetikleyicisi**.  
    Bu yayın işlem hattı, her seferinde yeni bir derleme yapıtının kullanılabilir bir dağıtımın yürüttüğü bir etkin bir CD tetikleyicisine sahiptir. İsteğe bağlı olarak, el ile yürütme dağıtımlarınızı gerektirir böylece tetikleyiciyi devre dışı bırakabilirsiniz. 

1. Sol tarafta seçin **görevleri**ve ardından, ortamınızı seçin.  
    Dağıtım işleminizin yürütür ve aşamalarında gruplandırılmış etkinlikler görevlerdir. Bu yayın işlem hattı iki aşamada gerçekleşir:
    * Birinci aşama iki işlemi gerçekleştiren bir Azure kaynak grubu dağıtımı görev içerir:
      * VM dağıtımı için yapılandırır.
      * Yeni VM, Azure DevOps dağıtım grubuna ekler. Azure DevOps VM Dağıtım grubundaki dağıtım hedef makine mantıksal grupları yönetir.
    * İkinci aşamasında, bir IIS Web uygulamasını Yönetme görev VM'de bir IIS Web sitesi oluşturur. İkinci bir IIS Web uygulamasını dağıtma görevi, siteyi dağıtmak için oluşturulur.

1. Sağ taraftaki seçin **yayınları görüntüleyebilir** yayınlar geçmişini görüntülemek için.

1. Bir yayın yanındaki üç nokta (...) seçin ve ardından **açık**.  
    Yayın özeti ilişkili iş öğeleri ve test gibi çeşitli menüleri keşfedebilirsiniz.

1. **İşlemeler**'i seçin.  
    Bu görünüm, bu dağıtımla ilişkilendirilmiş kodu yürütmeleri gösterir. Dağıtımlar arasındaki işleme farklılıklarını görmek için yayınları karşılaştırın.

1. **Günlükler**’i seçin.  
    Günlüklerde, dağıtım işlemiyle ilgili yararlı bilgiler bulunur. Bunları, sırasında ve sonrasında dağıtımları görüntüleyebilirsiniz.

## <a name="commit-changes-to-azure-repos-and-automatically-deploy-them-to-azure"></a>Değişiklikleri Azure depolara ve otomatik olarak Azure'a dağıtın 

Artık otomatik olarak en son iş sitenize dağıtan bir CI/CD işlem kullanarak uygulamanızı bir ekip ile işbirliği yapmaya hazır. Her değişiklik Git deposu için Azure DevOps bir yapı başlatır ve Azure'a dağıtılacak bir CD işlem hattı yürütür. Bu bölümdeki yordamı izleyin veya değişiklikleri deponuza işlemek için başka bir teknik kullanın. Kod değişikliklerini, CI/CD işlem başlatmak ve otomatik olarak IIS Web sitesine Azure sanal makinesinde yaptığınız değişiklikleri dağıtın.

1. Sol bölmede seçin **kod**, deponuza gidin.

1. Git *görünümler/giriş* dizin yanındaki üç nokta (...) seçin *Index.cshtml* dosya ve ardından **Düzenle**.

1. Div etiketlerinden birini bazı metinler ekleme gibi bu dosyaya bir değişiklik yapın. 

1. Sağ üst kısımdaki seçin **işleme**ve ardından **işleme** değişikliğiniz yeniden göndermek için.  
    Birkaç dakika sonra Azure DevOps bir yapı başlatır ve değişiklikleri dağıtmak için bir yayın yürütür. DevOps projeleri panosunda veya tarayıcı Azure DevOps kuruluşunuz ile derleme durumunu izleyin.

1. Yayın tamamlandığında, değişikliklerinizi doğrulamak için uygulamanızı yenileyin.

## <a name="configure-azure-application-insights-monitoring"></a>Application Insights izlemeyi yapılandırma

Azure Application Insights ile, uygulamanızın performansını ve kullanımını kolayca izleyebilirsiniz. DevOps projeleri, uygulamanız için Application Insights kaynağı otomatik olarak yapılandırır. Gerekirse başka uyarılar ve izleme özellikleri de yapılandırabilirsiniz.

1. Azure portalında DevOps projeleri panoya gidin. 

1. Alt sağ tarafta seçin **Application Insights** uygulamanıza yönelik bağlantı.  
    **Application Insights** bölmesi açılır. Bu görünüm uygulamanızın kullanım, performans ve kullanılabilirlik izleme bilgilerini içerir.

    ![Application Insights bölmesi](_img/azure-devops-project-github/appinsights.png) 

1. Seçin **zaman aralığı**ve ardından **son bir saat**. Sonuçları filtrelemek için seçin **güncelleştirme**.  
    Artık tüm etkinliğinden son 60 dakika görüntüleyebilirsiniz. 
    
1. Zaman aralığı'ndan çıkmak için seçin **x**.

1. Seçin **uyarılar**ve ardından **ölçüm uyarısı Ekle**. 

1. Uyarı için bir ad girin.

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

Test yapıyorsanız, kaynaklarınızı temizleyerek fatura ücretler tahakkuk önleyebilirsiniz. Artık gerekli değilse, bu öğreticide oluşturduğunuz kaynaklar ve Azure sanal makine silebilirsiniz. Bunu yapmak için **Sil** DevOps projesi Pano işlevselliği. 

> [!IMPORTANT]
> Aşağıdaki yordam, kaynakları kalıcı olarak siler. *Sil* işlevselliği, hem Azure hem de Azure DevOps, DevOps projeleri, proje tarafından oluşturulan verileri yok eder ve onu almak mümkün olmayacaktır. Yönergeleri dikkatle yalnızca okuduktan sonra bu yordamı kullanın.

1. Azure portalında DevOps projeleri panoya gidin.
1. Sağ üst kısımdaki seçin **Sil**. 
1. İstemde, seçin **Evet** için *kalıcı olarak silmek* kaynakları.

Ekibinizin gereksinimlerine uygun olarak bu derleme ve yayın işlem hatlarını istediğiniz gibi değiştirebilirsiniz. Ayrıca bu CI/CD desenini diğer işlem hatlarınızda şablon olarak kullanabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * ASP.NET uygulamanızı dağıtmak için DevOps projeleri'ni kullanın
> * Azure DevOps ve Azure aboneliğinin yapılandırın 
> * CI işlem hattını inceleme
> * CD işlem hattını inceleme
> * Değişiklikleri Azure depolara ve otomatik olarak Azure'a dağıtın
> * Application Insights izlemeyi yapılandırma
> * Kaynakları temizleme

CI/CD işlem hattı hakkında daha fazla bilgi için bkz:

> [!div class="nextstepaction"]
> [Çok aşamalı sürekli dağıtım (CD) işlem hattınızı tanımlayın](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts)
