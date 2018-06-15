---
title: Sağlama ve mikro beklendiği azure'da dağıtma
description: Mikro tek bir birim olarak Azure App Service ve JSON kaynak grubu şablonları ve PowerShell komut dosyası kullanarak tahmin edilebilir bir şekilde oluşan bir uygulamanın nasıl dağıtılacağı öğrenin.
services: app-service
documentationcenter: ''
author: cephalin
manager: erikre
editor: jimbe
ms.assetid: bb51e565-e462-4c60-929a-2ff90121f41d
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2016
ms.author: cephalin
ms.openlocfilehash: 3719e037f1564411a8f94d1ca962ba1ef6b5d435
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23837102"
---
# <a name="provision-and-deploy-microservices-predictably-in-azure"></a>Sağlama ve mikro beklendiği azure'da dağıtma
Bu öğretici, sağlamak ve oluşan bir uygulamayı dağıtmak gösterilmiştir [mikro](https://en.wikipedia.org/wiki/Microservices) içinde [Azure App Service](/services/app-service/) tek bir birim olarak ve JSON kaynak grubu şablonları ve PowerShell komut dosyası kullanarak bir tahmin edilebilir şekilde. 

Sağlama ve yüksek oranda ayrılmış oluşur büyük ölçekli uygulamalar dağıtırken mikro, Yinelenebilirlik ve öngörülebilirlik başarısı için önemli. [Azure uygulama hizmeti](/services/app-service/) , web uygulamaları, mobil uygulamalar, API apps ve logic apps dahil mikro oluşturmanıza olanak sağlar. [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) veritabanı gibi kaynak bağımlılıkları ile birlikte bir birim olarak tüm mikro yönetmek ve denetim ayarlarını kaynak sağlar. Şimdi, JSON şablonları ve basit PowerShell komut dosyası kullanarak bu tür bir uygulama dağıtabilirsiniz. 

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="what-you-will-do"></a>Ne yapacağını
Öğreticide içeren bir uygulama dağıtır:

* İki web uygulamaları (yani, iki mikro)
* Arka uç SQL veritabanı
* Uygulama ayarları, bağlantı dizeleri ve kaynak denetimi
* Application ınsights, uyarılar, otomatik ölçeklendirmeyi ayarları

## <a name="tools-you-will-use"></a>Kullanacağınız araçları
Bu öğreticide, aşağıdaki araçları kullanın. Araçlar ile ilgili kapsamlı tartışma olmadığından, uçtan uca senaryoyu takılıyor ve yalnızca kısa bir giriş her için size yapacağım ve burada bulabilirsiniz, hakkında daha fazla bilgi. 

### <a name="azure-resource-manager-templates-json"></a>Azure Resource Manager şablonları (JSON)
Örneğin, Azure App Service'te bir web uygulaması oluşturma her zaman, Azure Resource Manager ile bileşen kaynakları tüm kaynak grubu oluşturmak için JSON şablonunu kullanır. Karmaşık bir şablondan [Azure Marketi](/marketplace) gibi [ölçeklenebilir WordPress](/marketplace/partners/wordpress/scalablewordpress/) uygulama MySQL veritabanı, depolama hesapları, uygulama hizmeti planı, web uygulaması, uyarı kuralları, uygulama ayarları, otomatik ölçeklendirme ayarlarını ve daha fazlasını içerebilir ve bu şablonları PowerShell ile kullanılabilir. Karşıdan yükleme ve bu şablonları kullanma hakkında daha fazla bilgi için bkz: [Azure PowerShell kullanarak Azure Resource Manager ile](../powershell-azure-resource-manager.md).

Azure Resource Manager şablonları hakkında daha fazla bilgi için bkz: [Azure Resource Manager şablonları yazma](../azure-resource-manager/resource-group-authoring-templates.md)

### <a name="azure-sdk-26-for-visual-studio"></a>Visual Studio için Azure SDK 2.6
En yeni SDK, JSON düzenleyicisinin Resource Manager şablonu desteğinde iyileştirmeler içerir. Değiştirilmek üzere bu kullanabilir hızlı bir şekilde bir kaynak grubu şablonu sıfırdan oluşturmak veya var olan bir JSON şablonu (örneğin, indirilen galeri şablonu) açmak için parametreler dosyası doldurmak ve hatta kaynak grubundan doğrudan Azure kaynak grubu çözümünü dağıtabilirsiniz.

Daha fazla bilgi için bkz: [Visual Studio için Azure SDK 2.6](https://azure.microsoft.com/blog/2015/04/29/announcing-the-azure-sdk-2-6-for-net/).

### <a name="azure-powershell-080-or-later"></a>Azure PowerShell 0.8.0 veya daha yenisi
Sürüm 0.8.0'den başlayarak, Azure PowerShell'i yükleme Azure modülü yanı sıra Azure Resource Manager modülü içerir. Bu yeni modül, kaynak grubuna dağıtım komut dosyası sağlar.

Daha fazla bilgi için bkz: [Azure PowerShell'i Azure Resource Manager ile kullanma](../powershell-azure-resource-manager.md)

### <a name="azure-resource-explorer"></a>Azure Resource Manager
Bu [önizleme aracını](https://resources.azure.com) aboneliğinizi ve kaynakların tüm kaynak gruplarının JSON tanımları keşfetmeniz için etkinleştirir. Aracı, bir kaynağın JSON tanımlarını düzenleme, kaynakların tüm bir hiyerarşiye silin ve yeni kaynaklar oluşturun.  Bu aracı kullanıma hazır bilgi kaynağı, doğru değerleri, vb. belirli bir tür için ayarlamanız gerekir hangi özelliklerin gösterdiği için şablon yazmak için yararlı olur. Kaynak grubunda bile oluşturabilirsiniz [Azure Portal](https://portal.azure.com/), kaynak grubu templatize yardımcı olması için explorer aracı kendi JSON tanımlarında inceleyin.

### <a name="deploy-to-azure-button"></a>Azure düğmesine dağıtma
Kaynak denetimi için GitHub kullanırsanız, koyabilirsiniz bir [Azure düğmesi Dağıt](https://azure.microsoft.com/blog/2014/11/13/deploy-to-azure-button-for-azure-websites-2/) halinde, Benioku. MD anahtar teslim dağıtımı Azure için kullanıcı Arabirimi sağlar. Tüm basit web uygulaması için bunu yapabilirsiniz olsa da, tüm kaynak grubunuzun depo kök dizininde azuredeploy.json dosyasını koyarak dağıtımı etkinleştirmek için bunu genişletebilirsiniz. Kaynak grubu şablonu içerir, bu JSON dosyası Azure düğmesine dağıtma tarafından kaynak grubu oluşturmak için kullanılır. Bir örnek için bkz: [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) Bu öğreticide kullanacaksınız örnek.

## <a name="get-the-sample-resource-group-template"></a>Örnek kaynak grubunu Şablon Al
Böylece artık şimdi almak için doğru.

1. Gidin [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) uygulama hizmeti örnek.
2. Readme.MD içinde tıklatın **Azure'a Dağıt**.
3. İçin geçen [azure ile dağıtma](https://deploy.azure.com) site ve dağıtım parametreleri giriş istenir. Alanların çoğunu havuz adı ve bazı rastgele dizeleri ile sizin için doldurulacağı dikkat edin. İstediğiniz, ancak SQL Server yönetici oturum açma adı ve parola girmek zorunda tek şey'ye tıklayın, tüm alanları değiştirebilirsiniz **sonraki**.
   
   ![](./media/app-service-deploy-complex-application-predictably/gettemplate-1-deploybuttonui.png)
4. Bundan sonra öğesini **dağıtma** dağıtım işlemini başlatmak üzere. İşlem tamamlanıncaya kadar çalışır sonra http://todoapp tıklayın*XXXX*. azurewebsites.net bağlantı dağıtılmış uygulamanın göz atmak için. 
   
   ![](./media/app-service-deploy-complex-application-predictably/gettemplate-2-deployprogress.png)
   
   Kullanıcı arabirimini uygulamalar yalnızca başladılar çünkü ilk kendisine göz attığınızda biraz yavaş, ancak kendiniz tam olarak işlevsel bir uygulaması olduğunu sağlamak.
5. Yeniden Dağıt sayfasında tıklatın **Yönet** Azure Portalı'nda yeni uygulama görmek için bağlantı.
6. İçinde **Essentials** açılan listesinde, kaynak grubunu bağlantısını tıklatın. Ayrıca, GitHub deposuna altında web uygulaması zaten bağlı unutmayın **harici proje**. 
   
   ![](./media/app-service-deploy-complex-application-predictably/gettemplate-3-portalresourcegroup.png)
7. Kaynak grubu dikey penceresinde olduğunu zaten iki web uygulamaları ve kaynak grubu bir SQL veritabanında unutmayın.
   
   ![](./media/app-service-deploy-complex-application-predictably/gettemplate-4-portalresourcegroupclicked.png)

Hemen bir kısa süre içinde tüm bileşenleri, bağımlılıkları, ayarları, veritabanı ve sürekli yayımlama ile bir tam olarak dağıtılan iki mikro hizmet uygulaması tarafından bir otomatikleştirilmiş düzenlemesi Azure Kaynak Yöneticisi'nde ayarlanır gördüğünüz her şey. Tüm bu iki şey tarafından yapılmıştır:

* Azure düğmesine dağıtma
* Depo kök azuredeploy.JSON

Aynı uygulama dağıtabilirsiniz onlarca, yüzlerce veya binlerce kez ve her zaman tam aynı yapılandırmaya sahip. Yinelenebilirlik ve bu yaklaşım öngörülebilirlik kolaylığı ve güven ile yüksek ölçekli uygulamalar dağıtmanıza olanak sağlar.

## <a name="examine-or-edit-azuredeployjson"></a>AZUREDEPLOY inceleyin (veya Düzenle). JSON
Şimdi GitHub deposuna nasıl ayarlandığı bakalım. Azure .NET SDK'ın JSON Düzenleyicisi'ni kullanarak bunu henüz yüklemediyseniz [Azure .NET SDK 2.6](https://azure.microsoft.com/downloads/), şimdi yapın.

1. Kopya [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) , sık kullanılan git aracını kullanarak deposu. Aşağıdaki ekran görüntüsünde, ı Takım Gezgini'nde Visual Studio 2013 de bunu.
   
   ![](./media/app-service-deploy-complex-application-predictably/examinejson-1-vsclone.png)
2. Depo kökünden azuredeploy.json Visual Studio'da açın. JSON ana hattı bölmesini göremiyorsanız, Azure .NET SDK'yı yüklemeniz gerekir.
   
   ![](./media/app-service-deploy-complex-application-predictably/examinejson-2-vsjsoneditor.png)

Her ayrıntılı JSON biçimini açıklamak için yapacağım değil ancak [daha kaynakları](#resources) bölüm kaynak grubunu şablon dili öğrenme için bağlantılar içeriyor. Burada, yalnızca uygulama dağıtımı için kendi özel şablonunuzu yaparken yardımcı olabilecek ilgi çekici özellikler başlamak göstermek için yapacağım.

### <a name="parameters"></a>Parametreler
Bu parametreler çoğunu ne olduğunu görmek için parametreleri bölümüne göz atın **Azure'a Dağıt** düğmesi girmesini ister. Sitenin arkasında **Azure'a Dağıt** düğmesini azuredeploy.json içinde tanımlanan parametreleri kullanarak UI giriş doldurur. Bu parametreleri kaynak adları, özellik değerleri, vb. gibi kaynak tanımları boyunca kullanılır.

### <a name="resources"></a>Kaynaklar
Kaynakları düğümünde 4 en üst düzey kaynakları, bir SQL Server örneği, bir uygulama hizmeti planı ve iki web uygulamaları gibi tanımlandığından emin görebilirsiniz. 

#### <a name="app-service-plan"></a>App Service planı
JSON içinde basit bir kök düzeyinde kaynakla başlayalım tıklatın. JSON ana hattı adlı uygulama hizmeti planı tıklatın **[hostingPlanName]** karşılık gelen JSON kodunu vurgulayın. 

![](./media/app-service-deploy-complex-application-predictably/examinejson-3-appserviceplan.png)

Unutmayın `type` öğesi belirtir (Bu çağrıldı bir sunucu grubu bir uzun, uzun zaman önce) bir uygulama hizmeti planı dizesi ve diğer öğeleri ve özellikleri JSON dosyasında tanımlanan parametreleri kullanarak doldurulur ve tüm iç içe kaynaklar bu kaynağa sahip değil.

> [!NOTE]
> Değeri ayrıca `apiVersion` JSON kaynak tanımıyla kullanmak için REST API ve hangi sürümü kaynak içinde nasıl biçimlendirilmelidir etkileyebilecek Azure söyler `{}`. 
> 
> 

#### <a name="sql-server"></a>SQL Server
Bundan sonra adlı SQL Server Kaynak öğesini **SQLServer** JSON anahat.

![](./media/app-service-deploy-complex-application-predictably/examinejson-4-sqlserver.png)

Vurgulanan JSON kodunu hakkında aşağıdakileri unutmayın:

* Parametre kullanımı ile oluşturulmuş olan kaynaklar adlı ve birbiriyle tutarlı hale getirir şekilde yapılandırılmış sağlar.
* SQLServer kaynak iki iç içe geçmiş kaynakların varsa, her için farklı bir değere sahip `type`.
* İçindeki iç içe geçmiş kaynakların `“resources”: […]`, veritabanını ve güvenlik duvarı kuralları tanımlandığı, sahip bir `dependsOn` öğesi kök düzeyinde SQLServer kaynak kaynak Kimliğini belirtir. "Başka kaynağı zaten var olması gerekir; bu kaynak, oluşturmadan önce bu Azure Resource Manager bildirir. ve bu diğer kaynak şablonda tanımlanırsa, bir ilk oluşturma".
  
  > [!NOTE]
  > Nasıl kullanılacağı hakkında ayrıntılı bilgi için `resourceId()` işlev, bkz: [Azure Resource Manager şablonu işlevleri](../azure-resource-manager/resource-group-template-functions-resource.md#resourceid).
  > 
  > 
* Etkisini `dependsOn` öğesidir Azure Resource Manager kaynakları paralel olarak oluşturulabilir ve hangi kaynaklara sırayla oluşturulmalıdır bilebilirsiniz. 

#### <a name="web-app"></a>Web uygulaması
Şimdi, daha karmaşık olan gerçek web uygulamaları için kendileri şimdi geçin. Kendi JSON kodunu vurgulamak için JSON ana hattı [variables('apiSiteName')] web uygulaması'ı tıklatın. İşleri çok daha ilginç aldıklarından fark edeceksiniz. Bu amaçla ı tek tek özellikleri hakkında konuşma:

##### <a name="root-resource"></a>Kök kaynak
Web uygulamasının iki farklı kaynaklarına bağlıdır. Bu, yalnızca uygulama hizmeti planı ve SQL Server örneği oluşturulduktan sonra Azure Resource Manager web uygulaması oluşturacak anlamına gelir.

![](./media/app-service-deploy-complex-application-predictably/examinejson-5-webapproot.png)

##### <a name="app-settings"></a>Uygulama ayarları
Uygulama ayarları da iç içe geçmiş bir kaynak olarak tanımlanır.

![](./media/app-service-deploy-complex-application-predictably/examinejson-6-webappsettings.png)

İçinde `properties` öğesi için `config/appsettings`, iki uygulama ayarları biçiminde sahip `“<name>” : “<value>”`.

* `PROJECT`olan bir [KUDU ayarı](https://github.com/projectkudu/kudu/wiki/Customizing-deployments) birden çok proje Visual Studio çözümünde kullanmak için hangi projenizi Azure dağıtım söyler. T, kaynak denetimi daha sonra nasıl yapılandırıldığını gösterir, ancak ToDoApp kodu birden çok proje Visual Studio çözümünde olduğundan, bu ayar ihtiyacımız.
* `clientUrl`uygulama kodu ayarı olarak yalnızca bir uygulama kullanılır.

##### <a name="connection-strings"></a>Bağlantı dizeleri
Bağlantı dizeleri de iç içe geçmiş bir kaynak olarak tanımlanır.

![](./media/app-service-deploy-complex-application-predictably/examinejson-7-webappconnstr.png)

İçinde `properties` öğesi için `config/connectionstrings`, her bir bağlantı dizesini de belirli biçiminde bir ad: değer çifti olarak tanımlanan `“<name>” : {“value”: “…”, “type”: “…”}`. İçin `type` öğesi, olası değerler şunlardır: `MySql`, `SQLServer`, `SQLAzure`, ve `Custom`.

> [!TIP]
> Bağlantı dizesi türleri eksiksiz bir listesi için Azure PowerShell'de aşağıdaki komutu çalıştırın: \[Enum]::GetNames("Microsoft.WindowsAzure.Commands.Utilities.Websites.Services.WebEntities.DatabaseType")
> 
> 

##### <a name="source-control"></a>Kaynak denetimi
Kaynak Denetim ayarları da iç içe geçmiş bir kaynak olarak tanımlanır. Azure Resource Manager sürekli yayımlamayı yapılandırmak için bu kaynak kullanır (uyarısıyla bakın `IsManualIntegration` daha sonra) ve ayrıca uygulama kodu dağıtım JSON dosyasının işlenmesi sırasında otomatik olarak devre dışı tetiklersiniz.

![](./media/app-service-deploy-complex-application-predictably/examinejson-8-webappsourcecontrol.png)

`RepoUrl`ve `branch` oldukça sezgisel olmalıdır ve Git deposu ve yayımlamak için dal adını işaret etmelidir. Yeniden, bu giriş parametreleri tarafından tanımlanır. 

İçinde Not `dependsOn` öğesi web uygulaması kaynak kendisini ek olarak, `sourcecontrols/web` de bağlıdır `config/appsettings` ve `config/connectionstrings`. Bunun nedeni, bir kez `sourcecontrols/web` olan yapılandırılmış, Azure dağıtım işlemi otomatik olarak dağıtma, yapı ve uygulama kodu başlatma dener. Bu nedenle, bu bağımlılık ekleme uygulama kodu çalıştırmadan önce uygulamayı gerekli uygulama ayarlarının ve bağlantı dizeleri erişimi olduğundan emin olun yardımcı olur. 

> [!NOTE]
> Ayrıca `IsManualIntegration` ayarlanır `true`. GitHub depo gerçekte sahibi olmadığınız ve böylece gerçekte sürekli yayımlama yapılandırmak için Azure izni yapılamıyor çünkü bu özellik Bu öğreticide gerekli [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) (yani itme otomatik depo Güncelleştirmeler Azure). Varsayılan değer kullandığınız `false` yalnızca sahibinin GitHub kimlik bilgilerinde yapılandırdıysanız belirtilen depo [Azure portal](https://portal.azure.com/) önce. Diğer bir deyişle, GitHub veya BitBucket herhangi bir uygulama için kaynak denetimi ayarladıysanız, [Azure Portal](https://portal.azure.com/) Azure kimlik bilgilerini Hatırla ve GitHub veya BitBucket herhangi bir uygulamadan gelecekte dağıtmak olduğunda bunları kullanın. ardından daha önce kullanarak, kullanıcı, kimlik bilgileri. Ancak, bunu zaten yapmadıysanız, GitHub veya BitBucket depo sahibinin kimlik bilgileriyle oturum açamıyor çünkü web uygulamanızın kaynak denetim ayarları yapılandırmak Azure Resource Manager çalıştığında, JSON şablon dağıtımı başarısız olur.
> 
> 

## <a name="compare-the-json-template-with-deployed-resource-group"></a>JSON şablonunu dağıtılmış kaynak grubu ile karşılaştırın
Burada, tüm web uygulamanızın dikey aracılığıyla gidebilirsiniz [Azure Portal](https://portal.azure.com/), ancak yalnızca yararlı, Eğer daha değil başka bir aracı yoktur. Git [Azure kaynak Gezgini](https://resources.azure.com) Azure arka gerçekte oldukları gibi aboneliklerinizi, tüm kaynak gruplarının bir JSON temsili imkanı sağlayan Önizleme aracı. Azure kaynak grubunun JSON hiyerarşide oluşturmak için kullanılan şablon dosyası hiyerarşide nasıl karşılık gelen de görebilirsiniz.

Örneğin, ne zaman ı Git [Azure kaynak Gezgini](https://resources.azure.com) aracı ve Explorer'da düğümleri genişletin, kaynak grubu ve ilgili kaynak türlerine altında toplanan kök düzeyinde kaynakları görebilirsiniz.

![](./media/app-service-deploy-complex-application-predictably/ARM-1-treeview.png)

Bir web uygulaması için detaya, web uygulamasını yapılandırma ayrıntılarını benzer görmeye olmalıdır ekran görüntüsü aşağıda:

![](./media/app-service-deploy-complex-application-predictably/ARM-2-jsonview.png)

Yine, iç içe kaynaklar için JSON şablon dosyanızın de çok benzer bir hiyerarşisi olmalıdır ve görmelisiniz uygulama ayarları, bağlantı dizeleri, vb., JSON Bölmesi'nde düzgün yansıtılır. Burada ayarlarını yokluğu JSON dosyanız bir sorunu gösterebilir ve JSON şablon dosyanızın gidermenize yardımcı olabilir.

## <a name="deploy-the-resource-group-template-yourself"></a>Kaynak grubu şablonu kendiniz dağıtma
**Azure'a Dağıt** düğmesi harika olmakla birlikte, yalnızca, zaten azuredeploy.json için GitHub gönderilen durumunda kaynak grubu şablonunun azuredeploy.json dağıtmanıza olanak tanır. Azure .NET SDK'sı, tüm JSON şablon dosyasından doğrudan yerel makinenize dağıtmak araçlar da sağlar. Bunu yapmak için aşağıdaki adımları izleyin:

1. Visual Studio'da sırasıyla **dosya** > **yeni** > **proje**.
2. Tıklatın **Visual C#** > **bulut** > **Azure kaynak grubu**, ardından **Tamam**.
   
   ![](./media/app-service-deploy-complex-application-predictably/deploy-1-vsproject.png)
3. İçinde **Azure Şablonu Seç**seçin **boş şablon** tıklatıp **Tamam**.
4. Azuredeploy.JSON içine sürükleyin **şablonu** yeni projenizin klasör.
   
   ![](./media/app-service-deploy-complex-application-predictably/deploy-2-copyjson.png)
5. Çözüm Gezgini'nde, kopyalanan azuredeploy.json açın.
6. Yalnızca tanıtım amacıyla bazı standart uygulama Insight kaynakları bizim JSON dosyasına tıklayarak ekleyelim **kaynak ekleme**. JSON dosyasının dağıtmada yalnızca ilginizi çekiyorsa dağıtma adımları atlayın.
   
   ![](./media/app-service-deploy-complex-application-predictably/deploy-3-newresource.png)
7. Seçin **Web uygulamaları için Application Insights**, var olan bir App Service planı ve web uygulaması seçili olduğundan emin olun ve ardından **Ekle**.
   
   ![](./media/app-service-deploy-complex-application-predictably/deploy-4-newappinsight.png)
   
   Şimdi birkaç yeni kaynaklar kaynak bağlı olarak, ve ne yaptığını görmek uygulama hizmeti planı ya da web uygulaması bağımlılıklara sahip mümkün olur. Bu kaynaklar, varolan tanımı tarafından etkin değildir ve değiştirmek oluşturacağız.
   
   ![](./media/app-service-deploy-complex-application-predictably/deploy-5-appinsightresources.png)
8. JSON ana hattı tıklatın **Appınsights'dan otomatik ölçeklendirme** kendi JSON kodunu vurgulayın. Uygulama hizmeti planınız için ölçeklendirme ayar budur.
9. Vurgulanan JSON kodunu bulun `location` ve `enabled` özellikleri aşağıda gösterilen şekilde ayarlayın.
   
   ![](./media/app-service-deploy-complex-application-predictably/deploy-6-autoscalesettings.png)
10. JSON ana hattı tıklatın **CPUHigh Appınsights'dan** kendi JSON kodunu vurgulayın. Bu bir uyarıdır.
11. Bulun `location` ve `isEnabled` özellikleri aşağıda gösterilen şekilde ayarlayın. Diğer üç uyarıları (mor bulbs) için aynı işlemi yapın.
    
    ![](./media/app-service-deploy-complex-application-predictably/deploy-7-alerts.png)
12. Artık dağıtmak hazırsınız. Projeye sağ tıklayın ve seçin **dağıtma** > **yeni dağıtım**.
    
    ![](./media/app-service-deploy-complex-application-predictably/deploy-8-newdeployment.png)
13. Önceden yapmadıysanız Azure hesabınızda oturum açın.
14. Aboneliğinizde var olan bir kaynak grubunu seçin veya yeni bir select oluşturun **azuredeploy.json**ve ardından **parametreleri Düzenle**.
    
    ![](./media/app-service-deploy-complex-application-predictably/deploy-9-deployconfig.png)
    
    Artık iyi bir tabloda şablon dosyasında tanımlanan tüm parametreleri düzenlemeniz mümkün olması. Varsayılanları tanımlayan parametreleri zaten varsayılan değerlerine sahip ve izin verilen değerler listesini tanımlayan parametreleri bırakmalar gösterilir.
    
    ![](./media/app-service-deploy-complex-application-predictably/deploy-10-parametereditor.png)
15. Boş parametrelerinde doldurun ve kullanın [ToDoApp için GitHub deposuna adresi](https://github.com/azure-appservice-samples/ToDoApp.git) içinde **repoUrl**. Ardından **kaydetmek**.
    
    ![](./media/app-service-deploy-complex-application-predictably/deploy-11-parametereditorfilled.png)
    
    > [!NOTE]
    > Otomatik ölçeklendirme, içinde sunulan bir özelliktir **standart** katmanı ya da daha yüksek ve plan düzeyinde uyarılar özelliklerdir içinde sunulan **temel** katmanı veya üzeri, ayarlamak gerekir **sku** parametresi **standart** veya **Premium** tüm yeni App Insights kaynaklarınızı yukarı hafif görmeniz için.
    > 
    > 
16. Tıklatın **dağıtmak**. Seçtiyseniz **parolaları kaydetme**, parola parametresi dosyasında kaydedilir **düz metin**. Aksi takdirde dağıtım işlemi sırasında veritabanı parolasını giriş istenir.

Bu kadar! Adresine gitmeniz yeterlidir artık [Azure Portal](https://portal.azure.com/) ve [Azure kaynak Gezgini](https://resources.azure.com) , JSON eklenen otomatik ölçeklendirme ayarı ve yeni uyarıları görmek için aracı dağıtılan uygulama.

Bu bölümdeki adımları uygulamanıza çoğunlukla aşağıdaki gerçekleştirdiniz:

1. Şablon dosyası hazırlandı
2. Şablon dosyasıyla gitmek için bir parametre dosyası oluşturuldu
3. Parametre şablon dosyasını dağıtılan

Son adımda kolayca PowerShell cmdlet tarafından gerçekleştirilir. Visual Studio, uygulama dağıtıldığında ne yaptığını görmek için Scripts\Deploy AzureResourceGroup.ps1 açın. Çok fazla kod var. yoktur, ancak yalnızca parametre şablon dosyasını dağıtmak için gereken tüm ilgili kod vurgulamak için yapacağım.

![](./media/app-service-deploy-complex-application-predictably/deploy-12-powershellsnippet.png)

Son cmdlet `New-AzureResourceGroup`, eylemi gerçekleştiren olur. Tüm bu size Araçları Yardımı ile bulut uygulamanızı beklendiği dağıtmak görece basit olduğunu, göstermek. Aynı parametre dosyasıyla aynı şablona cmdlet'i çalıştırmak her zaman aynı sonucu elde paylaşacağız.

## <a name="summary"></a>Özet
İçinde DevOps, Yinelenebilirlik ve öngörülebilirlik başarılı herhangi mikro oluşan büyük ölçekli Uygulama dağıtımının anahtarları şunlardır. Bu öğreticide, Azure için iki mikro hizmet uygulaması Azure Resource Manager şablonu kullanarak tek bir kaynak grubu olarak dağıttıysanız. Neyse ki, Azure uygulamanızı bir şablona dönüştürme başlatmak ve sağlayabilir ve beklendiği dağıtmak için gereken bilgi verdiği. 

<a name="resources"></a>

## <a name="more-resources"></a>Diğer kaynaklar
* [Azure Resource Manager şablonu dili](../azure-resource-manager/resource-group-authoring-templates.md)
* [Azure Resource Manager şablonları yazma](../azure-resource-manager/resource-group-authoring-templates.md)
* [Azure Resource Manager şablonu işlevleri](../azure-resource-manager/resource-group-template-functions.md)
* [Azure Resource Manager şablonu ile bir uygulamayı dağıtma](../azure-resource-manager/resource-group-template-deploy.md)
* [Azure PowerShell’i Azure Resource Manager ile kullanma](../azure-resource-manager/powershell-azure-resource-manager.md)
* [Azure kaynak grubu dağıtım sorunlarını giderme](../azure-resource-manager/resource-manager-common-deployment-errors.md)

