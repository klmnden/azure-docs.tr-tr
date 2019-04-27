---
title: Sağlama ve mikro hizmetler, tahmin edilebilir bir biçimde - Azure App Service dağıtma
description: Mikro hizmetler tek bir birim olarak Azure App Service'te ve JSON kaynak grubu şablonları ve PowerShell komut dosyası kullanarak tahmin edilebilir bir biçimde oluşan bir uygulamayı nasıl dağıtacağınızı öğrenin.
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
ms.custom: seodec18
ms.openlocfilehash: e6d18222e15f62f12592362827b6dbc4a3d7dfbc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60767007"
---
# <a name="provision-and-deploy-microservices-predictably-in-azure"></a>Sağlayın ve tahmin edilebilir bir biçimde azure'da mikro Hizmetleri dağıtın
Bu öğreticide, sağlamak ve oluşan bir uygulamayı dağıtmak gösterilir [mikro Hizmetler](https://en.wikipedia.org/wiki/Microservices) içinde [Azure App Service](https://azure.microsoft.com/services/app-service/) tek bir birim olarak ve JSON kaynak grubu şablonları kullanarak tahmin edilebilir bir biçimde ve PowerShell komut dosyası. 

Sağlama ve yüksek oranda ayrılmış oluşur büyük ölçekli uygulamaları dağıtırken mikro hizmetler, Yinelenebilirlik ve tahmin edilebilirlik başarısı için önemlidir. [Azure App Service](https://azure.microsoft.com/services/app-service/) web uygulamaları, mobil arka uçlar ve API uygulamaları, mikro hizmetler oluşturmanıza olanak sağlar. [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) veritabanı gibi kaynak bağımlılıkları ile birlikte bir birim olarak tüm mikro Hizmetleri yönetin ve kaynak denetim ayarları sağlar. Şimdi, JSON şablonları ve basit PowerShell komut dosyası kullanarak bu tür bir uygulama dağıtabilirsiniz. 

## <a name="what-you-will-do"></a>Neler yapabileceği
Öğreticide içeren bir uygulama dağıtacaksınız:

* İki App Service uygulamaları (yani, iki mikro)
* Bir SQL veritabanı arka ucu
* Uygulama ayarları, bağlantı dizeleri ve kaynak denetimi
* Application ınsights, uyarılar, otomatik ölçeklendirme ayarları

## <a name="tools-you-will-use"></a>Kullanacağınız araçları
Bu öğreticide, aşağıdaki araçları kullanın. Araçlar kapsamlı tartışma olmadığından, uçtan uca senaryo için devam edin ve yalnızca kısa bir giriş her biri için size tıklıyorum ve burada bulabilirsiniz, daha fazla bilgi. 

### <a name="azure-resource-manager-templates-json"></a>Azure Resource Manager şablonları (JSON)
Örneğin, Azure App Service'te bir uygulama oluşturduğunuzda her Azure Resource Manager kaynak grubunun tamamını sahip bileşen kaynakları oluşturmak için JSON şablonunu kullanır. Karmaşık bir şablondan [Azure Marketi](/azure/marketplace) veritabanı, depolama hesapları, App Service planı, uygulama, uyarı kuralları, uygulama ayarları, otomatik ölçeklendirme ayarları ve daha fazlasını içerebilir ve bu şablonları için kullanılabilir PowerShell ile. Azure Resource Manager şablonları hakkında daha fazla bilgi için bkz. [Azure Resource Manager şablonları yazma](../azure-resource-manager/resource-group-authoring-templates.md)

### <a name="azure-sdk-26-for-visual-studio"></a>Visual Studio için Azure SDK 2.6
En son SDK'yı Resource Manager şablonu destek JSON Düzenleyicisi geliştirmeleri içerir. Değiştirme için bunu kullanın hızla sıfırdan bir kaynak grubu şablonu oluşturun veya var olan bir JSON şablonu (örneğin, bir indirilen galeri şablonu) açın, parametre dosyasını doldurmak ve bile bir Azure kaynağının kaynak grubunun doğrudan dağıtma Grup çözüm.

Daha fazla bilgi için [Visual Studio için Azure SDK 2.6](https://azure.microsoft.com/blog/2015/04/29/announcing-the-azure-sdk-2-6-for-net/).

### <a name="azure-powershell-080-or-later"></a>Azure PowerShell 0.8.0 veya üzeri
Azure PowerShell'i yükleme 0.8.0'ın sürümünden başlayarak, Azure modülünün yanı sıra Azure Resource Manager modülü içerir. Bu yeni modül, kaynak grubunun dağıtım komut dosyası sağlar.

Daha fazla bilgi için [Azure PowerShell'i Azure Resource Manager ile kullanma](../powershell-azure-resource-manager.md)

### <a name="azure-resource-explorer"></a>Azure Resource Manager
Bu [Önizleme aracı](https://resources.azure.com) aboneliğinize ve her bir kaynağın tüm kaynak gruplarının JSON tanımları keşfetmenize olanak sağlar. Aracı'nda bir kaynak JSON tanımını Düzenle, tüm bir hiyerarşiye kaynakları silin ve yeni kaynaklar oluşturan.  Bu araçta hazır bilgi, kaynak, doğru değerleri, vb. belirli bir tür için ayarlanacak ihtiyacınız hangi özelliklerin gösterir çünkü şablon yazma için çok yararlı olur. Kaynak grubunuzda bile oluşturabilirsiniz [Azure portalı](https://portal.azure.com/), ardından kaynak grubu templatize yardımcı olması için Gezgini aracının kendi JSON tanımları inceleyin.

### <a name="deploy-to-azure-button"></a>Azure düğmeye dağıtma
Kaynak denetimi için GitHub'ı kullanıyorsanız koyabilirsiniz bir [Azure'a Dağıt düğmesine](https://azure.microsoft.com/blog/2014/11/13/deploy-to-azure-button-for-azure-websites-2/) içine, Benioku. MD, anahtar teslim dağıtımını azure'da kullanıcı Arabirimi sağlar. Basit bir uygulama için bunu yapabilirsiniz, ancak depo kökünde azuredeploy.json dosyasını koyarak tüm kaynak grubu dağıtımı etkinleştirmek için bunu genişletebilirsiniz. Kaynak grubu şablonu içerir, bu JSON dosyasını Azure'a Dağıt düğmesine göre kaynak grubu oluşturmak için kullanılır. Bir örnek için bkz. [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) örnek, bu öğreticide kullanacaksınız.

## <a name="get-the-sample-resource-group-template"></a>Örnek kaynak grubu şablonu alın
Bu nedenle şimdi geçelim için doğru.

1. Gidin [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) App Service örneği.
2. Şuradaki Readme.MD, tıklayın **azure'a Dağıt**.
3. Adresine yönlendirilirsiniz [azure'a dağıtma](https://deploy.azure.com) site ve dağıtım parametreleri giriş istenir. Alanların çoğu depo adı ve bazı rastgele dizeleri ile sizin için doldurulduğuna dikkat edin. İstediğiniz, ancak SQL Server yönetici oturum açma ve parola girmek zorunda tek şey'a tıklayın, tüm alanları değiştirebilirsiniz **sonraki**.
   
   ![](./media/app-service-deploy-complex-application-predictably/gettemplate-1-deploybuttonui.png)
4. Ardından, **Dağıt** dağıtım işlemini başlatmak için. İşlem tamamlanana kadar çalışır bitince http://todoapp *XXXX*. azurewebsites.net bağlantı dağıtılan uygulamayı göz atmak için. 
   
   ![](./media/app-service-deploy-complex-application-predictably/gettemplate-2-deployprogress.png)
   
   Kullanıcı Arabirimi biraz yavaş, uygulamaları yalnızca başlatıyorsanız çünkü ilk kendisine göz attığınızda, ancak kendiniz tam işlevsel bir uygulama olduğunu ikna.
5. Yeniden Dağıt'a tıklayın **Yönet** Azure Portalı'nda yeni uygulamayı görmek için bağlantı.
6. İçinde **Essentials** açılır listesinde, kaynak grubu bağlantısına tıklayın. Ayrıca uygulamayı GitHub depo altında zaten bağlı olduğu unutmayın **dış proje**. 
   
   ![](./media/app-service-deploy-complex-application-predictably/gettemplate-3-portalresourcegroup.png)
7. Kaynak grubu dikey penceresinde, zaten iki uygulama ve kaynak grubunda bir SQL veritabanı dikkat edin.
   
   ![](./media/app-service-deploy-complex-application-predictably/gettemplate-4-portalresourcegroupclicked.png)

Yalnızca kısa birkaç dakika içinde bir tam dağıtılmış iki mikro hizmet uygulaması, tüm bileşenleri, bağımlılıkları, ayarları, veritabanı ve sürekli yayımlama ile Azure Resource Manager'daki otomatik düzenleme tarafından ayarlanan gördüğünüz her şey. Tüm bu iki şey tarafından yapıldı:

* Azure'a Dağıt düğmesine
* Depo kökünde azuredeploy.JSON

Bu aynı uygulama dağıtabileceğiniz onlarca, yüzlerce veya binlerce kez ve her zaman tam aynı yapılandırmaya sahip. Yinelenebilirliği ve tahmin edilebilirliğini bu yaklaşım, kolayca ve güvenle ile büyük ölçekli uygulamaları dağıtmanıza olanak sağlar.

## <a name="examine-or-edit-azuredeployjson"></a>AZUREDEPLOY inceleyin (veya düzenleyin). JSON
Şimdi GitHub deposu ayarlama göz atalım. Azure .NET SDK, JSON düzenleyicisini kullanacağınız henüz yüklemediyseniz bunu [Azure .NET SDK 2.6](https://azure.microsoft.com/downloads/), şimdi yapın.

1. Kopya [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) , sık kullanılan git aracını kullanarak depo. Aşağıdaki ekran görüntüsünde bu Visual Studio 2013'teki Takım Gezgini'nde yapıyor olabilirim.
   
   ![](./media/app-service-deploy-complex-application-predictably/examinejson-1-vsclone.png)
2. Depo kökünden azuredeploy.json Visual Studio'da açın. JSON ana hattı bölmesini görmüyorsanız, Azure .NET SDK'yı yüklemeniz gerekir.
   
   ![](./media/app-service-deploy-complex-application-predictably/examinejson-2-vsjsoneditor.png)

JSON biçimi, tüm ayrıntıları açıklayın tıklıyorum değil ancak [daha kaynakları](#resources) bölümü, kaynak grubu şablon dili öğrenmek için bağlantılar içeriyor. Burada, yalnızca uygulama dağıtımı için kendi özel şablonunuzu yaparken yardımcı olabilecek ilginç özellikleri başlama Göster tıklıyorum.

### <a name="parameters"></a>Parametreler
Çoğu bu parametre ne olduğunu görmek için parametreler bölümü göz atın **azure'a Dağıt** düğmesi girmenizi ister. Site arkasında **azure'a Dağıt** azuredeploy.json içinde tanımlanan parametrelerin kullanarak UI giriş düğmesi doldurur. Bu parametreler, kaynak adları, özellik değerleri, vb. gibi kaynak tanımları boyunca kullanılır.

### <a name="resources"></a>Kaynaklar
Kaynakları düğümünde 4 en üst düzey kaynaklar, bir SQL Server örneği, bir App Service planı ve iki uygulama da dahil olmak üzere tanımlandığından emin görebilirsiniz. 

#### <a name="app-service-plan"></a>App Service planı
Basit bir kök düzeyinde kaynak JSON başlayalım. JSON ana hattı adlı App Service planı tıklayın **[hostingPlanName]** karşılık gelen JSON kodunu vurgulayın. 

![](./media/app-service-deploy-complex-application-predictably/examinejson-3-appserviceplan.png)

Unutmayın `type` öğesi belirtir (Bu çağrıldı bir sunucu grubu bir uzun, uzun süre önce) bir App Service planı dizesi ve diğer öğeleri ve özellikleri JSON dosyasında tanımlanan parametrelerin kullanarak doldurulur ve bu kaynağın tüm iç içe geçmiş yok kaynaklar.

> [!NOTE]
> Ayrıca bu değeri Not `apiVersion` JSON kaynak tanımı ile kullanmak için REST API ve hangi sürümünü, iç kaynak nasıl biçimlendirilmelidir etkileyebilir Azure'a belirtmiş `{}`. 
> 
> 

#### <a name="sql-server"></a>SQL Server
Ardından, SQL Server Kaynak adlı **SQLServer** JSON ana hattı içinde.

![](./media/app-service-deploy-complex-application-predictably/examinejson-4-sqlserver.png)

Vurgulanan JSON kodunu hakkında aşağıdakileri unutmayın:

* Parametre kullanımı oluşturulan kaynakları adlı ve bunları birbiriyle tutarlı hale getirir şekilde yapılandırılmış sağlar.
* SQLServer kaynak iki iç içe geçmiş kaynaklar varsa, her için farklı bir değere sahip `type`.
* İç içe geçmiş kaynaklar içinde `“resources”: […]`, veritabanı ve güvenlik duvarı kuralları tanımlandığı, sahip bir `dependsOn` öğesi kök düzeyinde SQLServer kaynak kaynak Kimliğini belirtir. "Başka bir kaynak zaten mevcut olmalıdır; bu kaynak, oluşturmadan önce Azure Resource Manager, bu bildirir. ve bu diğer kaynak şablonda tanımlı ise tek ilk oluşturma".
  
  > [!NOTE]
  > Nasıl kullanılacağı hakkında ayrıntılı bilgi için `resourceId()` çalışması için bkz: [Azure Resource Manager şablonu işlevleri](../azure-resource-manager/resource-group-template-functions-resource.md#resourceid).
  > 
  > 
* Etkisini `dependsOn` öğedir Azure Resource Manager kaynakları paralel olarak oluşturulabilir ve hangi kaynakların sırayla oluşturulmalıdır bilebilirsiniz. 

#### <a name="app-service-app"></a>App Service uygulaması
Şimdi, daha karmaşık olan gerçek uygulamalar için kendilerine geçelim. JSON ana hattı JSON kodunu vurgulamak için [variables('apiSiteName')] uygulamanın tıklayın. Şeyler çok daha ilgi çekici alıyorsanız fark edeceksiniz. Bu amaçla miyim tek tek özellikler hakkında konuşacağız:

##### <a name="root-resource"></a>Kök kaynak
Uygulamayı iki farklı kaynaklarına bağlıdır. Başka bir deyişle, yalnızca App Service planı hem de SQL Server örneği oluşturulduktan sonra Azure Resource Manager uygulaması oluşturacaksınız.

![](./media/app-service-deploy-complex-application-predictably/examinejson-5-webapproot.png)

##### <a name="app-settings"></a>Uygulama ayarları
Uygulama ayarları, ayrıca iç içe geçmiş bir kaynak olarak tanımlanır.

![](./media/app-service-deploy-complex-application-predictably/examinejson-6-webappsettings.png)

İçinde `properties` öğesi için `config/appsettings`, iki uygulama ayarı biçiminde sahip `"<name>" : "<value>"`.

* `PROJECT` olan bir [KUDU ayarı](https://github.com/projectkudu/kudu/wiki/Customizing-deployments) hangi proje çoklu proje Visual Studio çözümü içinde kullanmak için Azure dağıtım söyler. Ben, kaynak denetimi daha sonra nasıl yapılandırıldığını gösterir, ancak ToDoApp kodu birden çok proje Visual Studio çözümü içinde olduğundan, bu ayar yapmamız gerekir.
* `clientUrl` uygulama kodu ayarı olarak yalnızca bir uygulama kullanılır.

##### <a name="connection-strings"></a>Bağlantı dizeleri
Bağlantı dizelerini, ayrıca iç içe geçmiş bir kaynak olarak tanımlanır.

![](./media/app-service-deploy-complex-application-predictably/examinejson-7-webappconnstr.png)

İçinde `properties` öğesi için `config/connectionstrings`, her bağlantı dizesini de belirli biçiminde bir ad: değer çifti olarak tanımlanan `"<name>" : {"value": "…", "type": "…"}`. İçin `type` öğesi, olası değerler `MySql`, `SQLServer`, `SQLAzure`, ve `Custom`.

> [!TIP]
> Bağlantı dizesi türleri eksiksiz bir listesi için Azure PowerShell'de aşağıdaki komutu çalıştırın: \[Enum]::GetNames("Microsoft.WindowsAzure.Commands.Utilities.Websites.Services.WebEntities.DatabaseType")
> 
> 

##### <a name="source-control"></a>Kaynak denetimi
Kaynak denetimi ayarlarını da iç içe geçmiş bir kaynak olarak tanımlanır. Azure Resource Manager sürekli yayımlamayı yapılandırmak için bu kaynak kullanır (uyarı bakın `IsManualIntegration` daha sonra) ve ayrıca uygulama kodu dağıtımını JSON dosyasının işlenmesi sırasında otomatik olarak hız kazandırın.

![](./media/app-service-deploy-complex-application-predictably/examinejson-8-webappsourcecontrol.png)

`RepoUrl` ve `branch` düzgün bir sezgisel olmalıdır ve Git deposu ve dalı yayımlamak için adını işaret etmelidir. Yeniden, bu giriş parametreleri tarafından tanımlanır. 

İçinde Not `dependsOn` öğesi kaynağının yanı sıra uygulama, bu `sourcecontrols/web` de bağlıdır `config/appsettings` ve `config/connectionstrings`. Bunun nedeni, sonra `sourcecontrols/web` olan yapılandırılmış, Azure dağıtım işlemi otomatik olarak dağıtma, oluşturma ve uygulama kodu Başlat dener. Bu nedenle, bu bağımlılık ekleme uygulama kodu çalıştırmadan önce uygulamayı gerekli uygulama ayarlarının ve bağlantı dizeleri erişimi olduğundan emin olmanıza yardımcı olur. 

> [!NOTE]
> Ayrıca `IsManualIntegration` ayarlanır `true`. GitHub deposunu gerçekten sahibi olmadığınız ve bu nedenle gerçekte Azure sürekli yayımlama yapılandırmak için izni olamaz çünkü bu özellik Bu öğreticide gerekli [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) (yani anında iletme otomatik depo güncelleştirmeleri Azure). Varsayılan değer kullandığınız `false` sahibinin GitHub kimlik bilgilerini yalnızca yapılandırdıysanız, belirtilen depo için [Azure portalında](https://portal.azure.com/) önce. Diğer bir deyişle, GitHub veya BitBucket herhangi bir uygulama için kaynak denetimine ayarladıysanız [Azure portalı](https://portal.azure.com/) Azure kimlik bilgilerini Hatırla ve herhangi bir uygulama dağıttığınız olduğunda bunları kullanın. ardından daha önce kullanıcı, kimlik bilgilerini kullanma GitHub veya BitBucket gelecekte. Ancak, bunu zaten yapmadıysanız, GitHub veya BitBucket ile depo sahibinin kimlik bilgileriyle oturum açamaz çünkü uygulamanın kaynak denetim ayarları yapılandırmak Azure Resource Manager çalıştığında, JSON şablonu dağıtımı başarısız olur.
> 
> 

## <a name="compare-the-json-template-with-deployed-resource-group"></a>JSON şablonunu dağıtılmış kaynak grubu ile Karşılaştır
Burada, uygulamanın tüm dikey pencerelerde aracılığıyla gidebilirsiniz [Azure portalı](https://portal.azure.com/), ancak yararlı, Eğer öyleyse daha fazla değil başka bir aracı yoktur. Git [Azure kaynak Gezgini](https://resources.azure.com) Azure arka uçtaki gerçekte oldukları gibi aboneliğinizdeki tüm kaynak gruplarını JSON temsili size Önizleme aracı. Ayrıca, Azure kaynak grubunun JSON hiyerarşide hiyerarşi oluşturmak için kullanılan şablon dosyasında nasıl karşılık geldiğini görebilirsiniz.

Örneğin, ne zaman ı Git [Azure kaynak Gezgini](https://resources.azure.com) aracı ve Gezgini'nde düğümleri genişletin, kaynak grubunu ve ilgili kaynak türlerine altında toplanan kök düzeyinde kaynakları görebilirsiniz.

![](./media/app-service-deploy-complex-application-predictably/ARM-1-treeview.png)

Aşağı uygulama ayrıntılarına giderseniz, benzer bir uygulama yapılandırma ayrıntılarını görmek erişebileceğinizi ekran görüntüsü aşağıda:

![](./media/app-service-deploy-complex-application-predictably/ARM-2-jsonview.png)

Yeniden iç içe geçmiş kaynaklar JSON şablon dosyanızda Kullanılanlara çok benzeyen bir hiyerarşisi olmalıdır ve görmelisiniz uygulama ayarları, bağlantı dizeleri, vb., doğru JSON bölmesinde görünür. Burada ayarları olmaması JSON dosyanızı soruna işaret edebilir ve JSON şablon dosyanızın gidermenize yardımcı olabilir.

## <a name="deploy-the-resource-group-template-yourself"></a>Kaynak grubu şablonu kendiniz dağıtma
**Azure'a Dağıt** düğmesidir harika, ancak bunu yalnızca, zaten azuredeploy.json github'a durumunda kaynak grubu şablon azuredeploy.json dağıtmanıza olanak tanır. Azure .NET SDK'sını da yerel makinenizden doğrudan herhangi bir JSON şablon dosyasını dağıtmak araçlar sağlar. Bunu yapmak için aşağıdaki adımları izleyin:

1. Visual Studio’da, **Dosya** > **Yeni** > **Proje**’ye tıklayın.
2. Tıklayın **Visual C#** > **bulut** > **Azure kaynak grubu**, ardından **Tamam**.
   
   ![](./media/app-service-deploy-complex-application-predictably/deploy-1-vsproject.png)
3. İçinde **Azure şablonunu Seç**seçin **boş şablonu** tıklatıp **Tamam**.
4. Azuredeploy.JSON içine sürükleyin **şablon** yeni projenizin klasör.
   
   ![](./media/app-service-deploy-complex-application-predictably/deploy-2-copyjson.png)
5. Kopyalanan azuredeploy.json Çözüm Gezgini'nden açın.
6. Yalnızca gösterim amacıyla, bazı standart Application Insight kaynaklar bizim JSON dosyasına tıklayarak ekleyelim **kaynak Ekle**. JSON dosyası dağıtırken yalnızca ilginizi çeken dağıtma adımları atlayın.
   
   ![](./media/app-service-deploy-complex-application-predictably/deploy-3-newresource.png)
7. Seçin **Web uygulamaları için Application Insights**ardından var olan bir App Service planı ve app seçildiğinden emin olun ve ardından **Ekle**.
   
   ![](./media/app-service-deploy-complex-application-predictably/deploy-4-newappinsight.png)
   
   Artık birkaç yeni kaynaklar kaynak bağlı olarak, ve ne işe yaradığını görmek için App Service planı ya da uygulama bağımlı mümkün olacaktır. Bu kaynaklar, mevcut bir tanımı tarafından etkin değildir ve değiştirmek oluşturacağız.
   
   ![](./media/app-service-deploy-complex-application-predictably/deploy-5-appinsightresources.png)
8. JSON ana hattı tıklayın **Appınsights otomatik ölçeklendirme** JSON kodunu vurgulayın. App Service planınız için ölçeklendirme ayar budur.
9. Vurgulanan JSON kod içinde açıklamayı `location` ve `enabled` özellikleri ve bunları aşağıda gösterilen şekilde ayarlayın.
   
   ![](./media/app-service-deploy-complex-application-predictably/deploy-6-autoscalesettings.png)
10. JSON ana hattı tıklayın **CPUHigh Appınsights** JSON kodunu vurgulayın. Bu bir uyarıdır.
11. Bulun `location` ve `isEnabled` özellikleri ve bunları aşağıda gösterilen şekilde ayarlayın. Diğer üç uyarıları (mor ampullere) için de aynısını yapın.
    
    ![](./media/app-service-deploy-complex-application-predictably/deploy-7-alerts.png)
12. Artık dağıtmaya hazırsınız. Projeye sağ tıklayıp **Dağıt** > **yeni dağıtım**.
    
    ![](./media/app-service-deploy-complex-application-predictably/deploy-8-newdeployment.png)
13. Zaten yapmadıysanız, Azure hesabınızda oturum açın.
14. Aboneliğinizde var olan bir kaynak grubu seçin veya yeni bir select oluşturma **azuredeploy.json**ve ardından **parametreleri Düzenle**.
    
    ![](./media/app-service-deploy-complex-application-predictably/deploy-9-deployconfig.png)
    
    Şimdi güzel bir tablodaki şablon dosyasında tanımlanan tüm parametreleri düzenlemek mümkün olacaktır. Varsayılan değerleri tanımlayan parametreler zaten varsayılan değerlerine sahiptir ve izin verilen değerler listesi tanımlayan parametreler bırakmalar gösterilir.
    
    ![](./media/app-service-deploy-complex-application-predictably/deploy-10-parametereditor.png)
15. Tüm boş parametreleri doldurun ve kullanmak [ToDoApp için GitHub deposunu adresi](https://github.com/azure-appservice-samples/ToDoApp.git) içinde **repoUrl**. ' A tıklayarak **Kaydet**.
    
    ![](./media/app-service-deploy-complex-application-predictably/deploy-11-parametereditorfilled.png)
    
    > [!NOTE]
    > Otomatik ölçeklendirme, sunulan bir özelliktir **standart** katmanı veya daha yüksek ve planı düzeyinde uyarıları olan sunulan özellikler **temel** katmanı veya ayarlamak sonraki ihtiyacınız **sku** parametresi için **standart** veya **Premium** tüm yeni App Insights kaynaklarınızı yukarı açık görmek için.
    > 
    > 
16. Tıklayın **dağıtma**. Seçtiyseniz **parolaları Kaydet**, parola parametre dosyasında kaydedilir **düz metin**. Aksi takdirde, dağıtım işlemi sırasında veritabanı parolasını giriş istenir.

Bu kadar! Artık yalnızca Git gerekiyor [Azure portalı](https://portal.azure.com/) ve [Azure kaynak Gezgini](https://resources.azure.com) otomatik ölçeklendirme ayarları, JSON olarak eklenir ve yeni uyarıları görmek için aracı uygulama dağıtılır.

Bu bölümdeki adımları uygulamanıza çoğunlukla aşağıdakileri gerçekleştirdiniz:

1. Şablon dosyası hazırlandı
2. Şablon dosyasını gitmek için bir parametre dosyası oluşturuldu
3. Parametre dosyası ile şablon dosyası dağıtıldı

Son adım, bir PowerShell cmdlet'i ile kolayca gerçekleştirilir. Uygulamanız dağıtıldığında Visual Studio ne yaptığını görmek için Scripts\Deploy AzureResourceGroup.ps1 açın. Birçok kod vardır, ancak yalnızca parametre dosyası ile şablon dosyasını dağıtmak için gereken ilgili kod vurgulamak için göstereceğim.

![](./media/app-service-deploy-complex-application-predictably/deploy-12-powershellsnippet.png)

Son cmdlet `New-AzureResourceGroup`, eylemi gerçekleştiren olur. Tüm bunlar, araç yardımıyla, tahmin edilebilir bir biçimde bulut uygulamanızı dağıtmak oldukça basit olduğunu ortaya koymalıdır. Aynı parametre dosyası ile aynı şablonu cmdlet komutunu her çalıştırdığınızda, aynı sonucu elde etmek için yedekleyeceksiniz.

## <a name="summary"></a>Özet
DevOps, Yinelenebilirlik ve tahmin edilebilirlik mikro hizmetlerden oluşan yüksek ölçekli uygulamasının başarılı bir dağıtım için anahtarlarıdır. Bu öğreticide, Azure Resource Manager şablonu kullanarak tek bir kaynak grubu olarak azure'a bir iki mikro hizmet uygulaması dağıttınız. Neyse ki bu, Azure uygulamanızda bir şablona dönüştürmeden başlatmak ve sağlayabilir ve tahmin edilebilir bir biçimde dağıtmak için nelere ihtiyacınız bilgi verdiği. 

<a name="resources"></a>

## <a name="more-resources"></a>Diğer kaynaklar
* [Azure Resource Manager şablonu dili](../azure-resource-manager/resource-group-authoring-templates.md)
* [Azure Resource Manager şablonları yazma](../azure-resource-manager/resource-group-authoring-templates.md)
* [Azure Resource Manager şablonu işlevleri](../azure-resource-manager/resource-group-template-functions.md)
* [Azure Resource Manager şablonu ile uygulama dağıtma](../azure-resource-manager/resource-group-template-deploy.md)
* [Azure PowerShell’i Azure Resource Manager ile kullanma](../azure-resource-manager/powershell-azure-resource-manager.md)
* [Azure'da kaynak grubu dağıtımıyla ilgili sorunları giderme](../azure-resource-manager/resource-manager-common-deployment-errors.md)

## <a name="next-steps"></a>Sonraki adımlar

Hakkında bilgi edinmek için bu makalede JSON söz dizimi ve kaynak türleri için özellikleri dağıtılırsa, bkz:

* [Microsoft.Sql/servers](/azure/templates/microsoft.sql/servers)
* [Microsoft.Sql/servers/databases](/azure/templates/microsoft.sql/servers/databases)
* [Microsoft.Sql/servers/firewallRules](/azure/templates/microsoft.sql/servers/firewallrules)
* [Microsoft.Web/serverfarms](/azure/templates/microsoft.web/serverfarms)
* [Microsoft.Web/sites](/azure/templates/microsoft.web/sites)
* [Microsoft.Web/sites/slots](/azure/templates/microsoft.web/sites/slots)
* [Microsoft.Insights/autoscalesettings](/azure/templates/microsoft.insights/autoscalesettings)