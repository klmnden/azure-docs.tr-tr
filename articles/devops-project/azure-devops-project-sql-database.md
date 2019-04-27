---
title: "Öğretici: Azure DevOps projeleri'ni kullanarak ASP.NET uygulaması ve Azure SQL veritabanı kod dağıtma"
description: DevOps projeleri, Azure'da kullanmaya başlamak kolaylaştırır. DevOps projeleri ile ASP.NET uygulaması ve Azure SQL veritabanı kod birkaç Hızlı adımda dağıtabilirsiniz.
ms.author: mlearned
ms.manager: douge
ms.prod: devops
ms.technology: devops-cicd
ms.topic: tutorial
ms.date: 07/09/2018
author: mlearned
monikerRange: vsts
ms.openlocfilehash: 0d05a2f3de92791572f0a5e6313777b5388af3df
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60554957"
---
# <a name="tutorial-deploy-your-aspnet-app-and-azure-sql-database-code-by-using-azure-devops-projects"></a>Öğretici: Azure DevOps projeleri'ni kullanarak ASP.NET uygulaması ve Azure SQL veritabanı kod dağıtma

Azure DevOps projeleri, mevcut kodunuzu ve Git deposuna taşıyın veya sürekli tümleştirme (CI) ve azure'a sürekli teslim (CD) işlem hattı oluşturmak için örnek bir uygulama seçin basitleştirilmiş bir deneyim sunar. 

DevOps projeleri ayrıca:
* Bir Azure SQL veritabanı gibi Azure kaynaklarını otomatik olarak oluşturur.
* Oluşturur ve Azure için CI derleme işlem hattı içeren işlem hattı, bir yayın ardışık düzeni yapılandırır.
* Bir yayın ardışık düzeni ' için CD ayarlar. 
* İzleme için Azure Application Insights kaynağı oluşturur.

Bu öğreticide şunları yapacaksınız:

> [!div class="checklist"]
> * ASP.NET uygulaması ve Azure SQL veritabanı kod dağıtmak için Azure DevOps projeleri'ni kullanın
> * Azure DevOps ve Azure aboneliğinin yapılandırın 
> * CI işlem hattını inceleme
> * CD işlem hattını inceleme
> * Değişiklikleri Azure depolara ve otomatik olarak Azure'a dağıtın
> * Azure SQL veritabanı'na bağlanma 
> * Kaynakları temizleme

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/) aracılığıyla ücretsiz bir abonelik alabilirsiniz.

## <a name="create-a-project-in-devops-projects-for-an-aspnet-app-and-an-azure-sql-database"></a>DevOps projeleri, ASP.NET uygulaması ve bir Azure SQL veritabanı için bir proje oluşturun

DevOps projeleri, Azure işlem hatlarında bir CI/CD işlem hattı oluşturur. Yeni bir Azure DevOps kuruluş oluşturun veya mevcut bir kuruluşa kullanın. DevOps projeleri, tercih ettiğiniz bir Azure aboneliği bir Azure SQL veritabanı gibi Azure kaynaklarını da oluşturur.

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Sol bölmede seçin **kaynak Oluştur**.

1. Arama kutusuna **DevOps projeleri**ve ardından **Oluştur**.

    ![DevOps projeleri Panosu](_img/azure-devops-project-github/fullbrowser.png)

1. Seçin **.NET**ve ardından **sonraki**.

1. Altında **uygulama çerçevesini seçin**seçin **ASP.NET**.

1. Seçin **bir veritabanı Ekle**ve ardından **sonraki**.  
    Bir önceki adımda seçtiğiniz uygulama çerçevesi buradan kullanılabilir Azure hizmeti dağıtımı hedef türünü belirler. 
    
1. **İleri**’yi seçin.

## <a name="configure-azure-devops-and-an-azure-subscription"></a>Azure DevOps ve Azure aboneliğinin yapılandırın

1. Yeni bir Azure DevOps kuruluş oluşturun veya mevcut bir kuruluşa seçin. 

1. Azure DevOps projeniz için bir ad girin. 

1. Azure aboneliği hizmetlerinizi seçin.  
    İsteğe bağlı olarak, ek Azure yapılandırma ayarları görüntülemek ve kullanıcı adını tanımlamak için **veritabanı sunucusu oturum açma ayrıntıları** seçebileceğiniz bölümünde **değişiklik**. Sonraki adımlar için kullanıcı adı, bu öğreticide Store. Bu isteğe bağlı bir adım gerçekleştirirseniz, seçtiğiniz önce Azure yapılandırma alanı çıkmak **Bitti**.
 
1. **Done** (Bitti) öğesini seçin.  
    Birkaç dakika sonra işlemi tamamlanır ve Azure portalında DevOps projeleri Pano açılır. Doğrudan panoya da gidebilirsiniz **tüm kaynakları** Azure portalında. Sağ taraftaki seçin **Gözat** çalışan uygulamanızı görüntülemek için.
    
## <a name="examine-the-ci-pipeline"></a>CI işlem hattını inceleme

DevOps projeleri, eksiksiz bir CI/CD işlem hattı Azure depolarda otomatik olarak yapılandırır. İşlem hattını inceleyebilir ve özelleştirebilirsiniz. Azure DevOps derleme işlem hattı çalıştırmasıyla tanımak için aşağıdakileri yapın:

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
    Bu bölme bir denetim kaydı derleme için en son değişikliği görüntüler. Azure işlem hatları için derleme işlem hattı yapılan değişiklikleri izler ve sürümleri karşılaştırmanızı sağlar.

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
    Önceki adımlarda incelenirken derleme işlem hattı yapıt için kullanılan bir çıktı üretir. 

1. Sağ tarafında **bırak** simgesini seçme **sürekli dağıtım tetikleyicisi**.  
    Bu yayın işlem hattı, her seferinde yeni bir derleme yapıtının kullanılabilir olan bir dağıtımın yürüttüğü etkin bir CD tetikleyicisine sahiptir. İsteğe bağlı olarak, el ile yürütme dağıtımlarınızı gerektirir böylece tetikleyiciyi devre dışı bırakabilirsiniz. 

    DevOps projeleri, rastgele bir SQL parolası ayarlar ve yayın işlem hattı kullanır.
    
1. Sol tarafta seçin **değişkenleri**. 

   > [!NOTE]
   > Yalnızca SQL Server parola değiştiyse, aşağıdaki adımı uygulayın. Tek bir parola değişkeni yoktur.
  
1. Yanındaki **değer** kutusuna, asma kilit simgesini seçin, yeni bir parola girin ve ardından **Kaydet**.

1. Sol tarafta seçin **görevleri**ve ardından, ortamınızı seçin.  
    Dağıtım işleminizin yürütür ve aşamalarında gruplanmış olan etkinlikler görevlerdir. Bu yayın ardışık düzeni içeren bir tek aşaması olan bir *Azure App Service dağıtma* ve *Azure SQL veritabanı dağıtımı* görev.

1. Seçin *Azure SQL Yürüt* görev ve SQL dağıtımı için kullanılan çeşitli özelliklerini inceleyin.  
    Altında **dağıtım paketi**, görev kullanan bir *SQL DACPAC* dosya.

1. Sağ taraftaki seçin **yayınları görüntüleyebilir** yayınlar geçmişini görüntülemek için.

1. Bir yayın yanındaki üç nokta (...) seçin ve ardından **açık**.  
     Yayın özeti ilişkili iş öğeleri ve test gibi çeşitli menüleri keşfedebilirsiniz.

1. **İşlemeler**'i seçin.  
     Bu görünüm, bu dağıtımla ilişkilendirilmiş kodu yürütmeleri gösterir. Dağıtımlar arasındaki işleme farklılıklarını görmek için yayınları karşılaştırın.

1. **Günlükler**’i seçin.  
     Günlüklerde, dağıtım işlemiyle ilgili yararlı bilgiler bulunur. Bunları, sırasında ve sonrasında dağıtımları görüntüleyebilirsiniz.

## <a name="commit-changes-to-azure-repos-and-automatically-deploy-them-to-azure"></a>Değişiklikleri Azure depolara ve otomatik olarak Azure'a dağıtın 

 > [!NOTE]
 > Aşağıdaki yordamda basit metin değişikliği ile CI/CD işlem hattı test eder. SQL dağıtım işlemini test etmek için isteğe bağlı olarak bir SQL Server şemasını değiştirmek tabloya yapabilirsiniz.

Artık otomatik olarak en son iş sitenize dağıtan bir CI/CD işlem kullanarak uygulamanızı bir ekip ile işbirliği yapmaya hazır. Her değişiklik Git deposu için Azure DevOps bir yapı başlatır ve Azure'a dağıtılacak bir CD işlem hattı yürütür. Bu bölümdeki yordamı izleyin veya değişiklikleri deponuza işlemek için başka bir teknik kullanın. Kod değişiklikleri, CI/CD işlem başlatmak ve değişikliklerinizi Azure'a otomatik olarak dağıtır.

1. Sol bölmede seçin **kod**, depoya gidin.

1. Git *SampleWebApplication\Views\Home* dizin yanındaki üç nokta (...) seçin *Index.cshtml* dosya ve ardından **Düzenle**. 

1. Div etiketlerinden birini bazı metinler ekleme gibi bu dosyaya bir değişiklik yapın. 

1. Sağ üst kısımdaki seçin **işleme**ve ardından **işleme** değişikliğiniz yeniden göndermek için.  
    Birkaç dakika sonra Azure DevOps bir yapı başlatır ve değişiklikleri dağıtmak için bir yayın yürütür. DevOps projeleri panosunda veya tarayıcı Azure DevOps kuruluşunuz ile derleme durumunu izleyin.

1. Yayın tamamlandığında, değişikliklerinizi doğrulamak için uygulamanızı yenileyin.

## <a name="connect-to-the-azure-sql-database"></a>Azure SQL veritabanı'na bağlanma

Azure SQL veritabanına bağlanmak için uygun izinlere ihtiyacınız var.

1. DevOps projeleri Panoda seçin **SQL veritabanı** SQL veritabanı yönetim sayfasına gidin.
   
1. Seçin **sunucu güvenlik duvarını Ayarla**ve ardından **istemci IP'si Ekle**. 

1. **Kaydet**’i seçin.  
    İstemci IP artık SQL Server Azure kaynağa erişebilir.

1. Geri Git **SQL veritabanı** bölmesi. 

1. Sağ taraftaki ilişkin yapılandırma sayfasına gitmek için sunucu adını seçin **SQL Server**.

1. Seçin **parolayı Sıfırla**SQL Sunucu Yöneticisi oturumu için bir parola girin ve ardından **Kaydet**.  
    Bu öğreticinin ilerleyen bölümlerinde kullanmak için bu parola tuttuğunuzdan emin olun.

    Şimdi isteğe bağlı olarak SQL Server Management Studio veya Visual Studio gibi istemci araçlarını SQL Server ve Azure SQL veritabanına bağlanmak için kullanabilirsiniz. Bağlanmak için **Sunucu adı** özelliğini kullanın.

    DevOps projeleri başlangıçta proje yapılandırıldığında veritabanı kullanıcı adı değiştirmediyseniz, kullanıcı adınız e-posta adresinizi yerel parçasıdır. Örneğin, e-posta adresinizi ise *CanEtikan\@microsoft.com*, kullanıcı adınız olduğu *CanEtikan*.

   > [!NOTE]
   > SQL oturum açma parolasını değiştirirseniz, "CD işlem hattı inceleyin" bölümünde açıklandığı gibi yayın işlem hattı değişken parolayı değiştirmeniz gerekir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Test yapıyorsanız, kaynaklarınızı temizleyerek fatura ücretler tahakkuk önleyebilirsiniz. Artık gerekli değilse, bu öğreticide oluşturduğunuz kaynaklar ve Azure SQL veritabanı silebilirsiniz. Bunu yapmak için **Sil** DevOps projeleri Pano işlevselliği.

> [!IMPORTANT]
> Aşağıdaki yordam, kaynakları kalıcı olarak siler. *Sil* işlevselliği, hem Azure hem de Azure DevOps, DevOps projeleri, proje tarafından oluşturulan verileri yok eder ve onu almak mümkün olmayacaktır. Yönergeleri dikkatle yalnızca okuduktan sonra bu yordamı kullanın.

1. Azure portalında DevOps projeleri panoya gidin.
2. Sağ üst kısımdaki seçin **Sil**. 
3. İstemde, seçin **Evet** için *kalıcı olarak silmek* kaynakları.

## <a name="next-steps"></a>Sonraki adımlar

Ekibinizin gereksinimlerine uygun olarak bu derleme ve yayın işlem hatlarını istediğiniz gibi değiştirebilirsiniz. Ayrıca bu CI/CD desenini diğer işlem hatlarınızda şablon olarak kullanabilirsiniz. Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * ASP.NET uygulaması ve Azure SQL veritabanı kod dağıtmak için Azure DevOps projeleri'ni kullanın
> * Azure DevOps ve Azure aboneliğinin yapılandırın 
> * CI işlem hattını inceleme
> * CD işlem hattını inceleme
> * Değişiklikleri Azure depolara ve otomatik olarak Azure'a dağıtın
> * Azure SQL veritabanı'na bağlanma 
> * Kaynakları temizleme

CI/CD işlem hattı hakkında daha fazla bilgi için bkz:

> [!div class="nextstepaction"]
> [Çok aşamalı sürekli dağıtım (CD) işlem hattınızı tanımlayın](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts)

## <a name="videos"></a>Videolar

> [!VIDEO https://channel9.msdn.com/Events/Build/2018/BRK3308/player]
