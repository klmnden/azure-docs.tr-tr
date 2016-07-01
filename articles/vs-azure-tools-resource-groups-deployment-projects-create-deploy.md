<properties
   pageTitle="Azure Resource Group Visual Studio projeleri | Microsoft Azure"
   description="Azure kaynak grubu projesi oluşturmak ve kaynakları Azure'a dağıtmak için Visual Studio'yu kullanın."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn" />
<tags
   ms.service="azure-resource-manager"
   ms.devlang="multiple"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/17/2016"
   ms.author="tomfitz" />

# Visual Studio aracılığıyla Azure kaynak grupları oluşturma ve dağıtma

Visual Studio ve [Azure SDK](https://azure.microsoft.com/downloads/) ile altyapınızı ve kodlarınızı Azure’a dağıtan bir proje oluşturabilirsiniz. Örneğin, uygulamanızın web ana bilgisayarını, web sitesini ve veritabanını tanımlayabilir ve kodlarını ve altyapısını dağıtabilirsiniz. Ayrıca, bir Sanal Makine, Sanal Ağ ve Depolama Hesabı tanımlayabilir ve bu altyapıyı ve Sanal Makinede yürütülen betiği dağıtabilirsiniz. **Azure Kaynak Grubu** dağıtım projesi gerekli tüm kaynakları tek, tekrarlanabilir bir işlemde dağıtmanıza olanak tanır. Kaynakların dağıtılması ve yönetilmesi hakkında daha fazla bilgi için bkz. [Azure Resource Manager’a genel bakış](resource-group-overview.md).

Azure Kaynak Grubu projeleri, Azure’da dağıtılan kaynakları tanımlayan Azure Resource Manager JSON şablonları içerir. Resource Manager şablonu bileşenleri hakkında daha fazla bilgi edinmek için [Azure Resource Manager şablonları yazma](resource-group-authoring-templates.md). Visual Studio bu şablonları düzenlemenize olanak tanır ve şablonlarla çalışmayı basitleştiren araçlar sunar.

Bu konuda, bir web uygulaması ve SQL Database’i nasıl dağıtacağınızı öğreneceksiniz. Ancak, bu adımlar herhangi bir türdeki kaynağı dağıtmakla neredeyse aynıdır. Bir Sanal Makineyi ve ilgili kaynaklarını kolayca dağıtabilirsiniz. Visual Studio genelde karşılaşılan senaryoların dağıtılması için birçok farklı başlangıç şablonu sağlar.

Bu makale, Visual Studio 2015 Güncelleştirme 2 ve .NET 2.9 için Microsoft Azure SDK kullanılarak yazılmıştır. Azure SDK 2.9 ile Visual Studio 2013 kullanıyorsanız, deneyiminiz büyük ölçüde aynı olacaktır. Azure SDK 2.6 veya sonraki sürümlerini kullanabilirsiniz. Ancak, deneyiminiz bu makalede gösterilenden farklı olabilir. Adımları uygulamaya başlamadan önce [Azure SDK](https://azure.microsoft.com/downloads/)’nın en son sürümünü yüklemenizi kesinlikle öneririz. 

## Azure Kaynak Grubu projesi oluşturma

Bu yordamda, bir **Web uygulaması + SQL** şablonu ile Azure Kaynak Grubu projesi oluşturacaksınız.

1. Visual Studio’da, **Dosya**, **Yeni Proje**’yi ve ardından **C#** veya **Visual Basic** seçeneğini belirleyin. Daha sonra **Bulut** ve ardından **Azure Kaynak Grubu** projesi seçeneğini belirleyin.

    ![Bulut Dağıtım Projesi](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-project.png)

1. Azure Resource Manager’da dağıtmak istediğiniz şablonu seçin. Dağıtmak istediğiniz proje türüne bağlı olarak çok sayıda farklı seçeneğiniz olduğunu unutmayın. Bu konu için **Web uygulaması + SQL** şablonunu seçeceğiz.

    ![Şablon seçme](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-project.png)

    Seçtiğiniz şablon sadece başlangıçtır; senaryonuzun gereksinimlerini karşılamak üzere kaynak ekleyebilir ve kaldırabilirsiniz.

    >[AZURE.NOTE] Kullanılabilir şablonların listesini çevrimiçi olarak alınabilir ve bu listede değişiklik yapılabilir.

    Visual Studio, web uygulaması ve SQL Database için bir kaynak grubu dağıtım projesi oluşturur.

1. Oluşturulan öğeleri görmek için dağıtım projesindeki düğümleri genişletin.

    ![düğümleri gösterme](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-items.png)

    Bu örnek için Web uygulaması + SQL şablonunu seçtiğimiz için, aşağıdaki dosyaları göreceksiniz. 

  	|Dosya adı|Açıklama|
  	|---|---|
  	|Deploy-AzureResourceGroup.ps1|Azure Resource Manager’da dağıtılacak PowerShell komutlarını çağıran PowerShell betiği.<br />**Not** Bu PowerShell betiği şablonunuzu dağıtmak için Visual Studio tarafından kullanılır. Bu betikte yaptığınız tüm değişiklikler Visual Studio’daki dağıtımı da etkiler, bu nedenle dikkatli olun.|
  	|WebSiteSQLDatabase.json|Azure’da dağıtmak istediğiniz altyapıyı tanımlayan Resource Manager şablonu ve dağıtım sırasında sağlayabileceğiniz parametreler. Doğru sırayla dağıtılmalarını sağlamak için kaynaklarınız arasındaki bağımlılıkları da tanımlar.|
  	|WebSiteSQLDatabase.parameters.json|Şablon tarafından gereken değerleri içeren bir parametre dosyası. Bunlar, her bir dağıtımı özelleştirmek için geçirdiğiniz değerlerdir.|

    Tüm kaynak grubu dağıtım projeleri bu temel dosyaları içerir. Diğer projeler diğer işlevleri desteklemek için ek dosyalar içerebilir.

## Resource Manager şablonunu özelleştirme

Dağıtmak istediğiniz kaynakları tanımlayan JSON şablonlarını değiştirerek dağıtım projesini özelleştirebilirsiniz. JSON, JavaScript Nesne Gösterimi anlamına gelir ve birlikte çalışması kolay bir sıralanmış veri biçimidir. JSON dosyaları her dosyanın üst kısmına başvuran bir şema kullanır. Daha iyi anlayabilmek için şemayı indirip analiz edebilirsiniz. Şema, hangi öğelere izin verildiğini, alan türlerini ve biçimlerini, numaralandırılmış değerlerin olası değerlerini ve benzer konuları tanımlar. Resource Manager şablonu bileşenleri hakkında daha fazla bilgi edinmek için [Azure Resource Manager şablonları yazma](resource-group-authoring-templates.md).

Şablonunuzda çalışmak için **WebSiteSQLDatabase.json** dosyasını açın.

Visual Studio düzenleyicisi, Resource Manager şablonu düzenleme konusunda size yardımcı olan araçlar sağlar. **JSON Ana Hattı** penceresi, şablonunuzda tanımlanan bileşenleri görmenizi kolaylaştırır.

![JSON ana hattını göster](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-json-outline.png)

Ana hattaki herhangi bir bileşeni seçtiğinizde, şablonun ilgili parçasına gidersiniz ve ilgili JSON vurgulanır.

![JSON’a gitme](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/navigate-json.png)

JSON Ana Hattı penceresinin üst tarafında bulunan **Kaynak Ekle** düğmesini seçerek veya **kaynaklar**’a sağ tıklayıp **Yeni Kaynak Ekle**’yi seçerek şablonunuza yeni kaynak ekleyebilirsiniz.

![kaynak ekle](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-resource.png)

Bu öğreticide, **Depolama Hesabı**’nı seçin ve bir ad verin. Depolama hesabı adı yalnızca sayı ve küçük harflerden oluşmalıdır ve 24 karakterden daha uzun olmalıdır. Proje, sağladığınız ada 13 karakterden oluşan benzersiz bir dize ekler. Bu nedenle adınızın 11 karakteri geçmediğinden emin olun. 

![depolama ekleme](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-storage.png)

Yalnızca kaynak eklenmediğini, aynı zamanda depolama hesabı türü için bir parametre ve depolama hesabı adı bir değişken eklendiğini unutmayın.

![ana hattı göster](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-new-items.png)

**storageType** parametresi izin verilen türler ve varsayılan tür ile önceden tanımlanmıştır. Bu değerleri bırakabilir veya senaryonuz için düzenleyebilirsiniz. Bu şablon aracılığıyla herkesin **Premium_LRS** depolama hesabına erişmesine izin vermek istemiyorsanız, tek yapmanız gereken aşağıda gösterildiği gibi izin verilen türlerden bunu kaldırmaktır. 

    "storageType": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_ZRS",
        "Standard_GRS",
        "Standard_RAGRS"
      ]
    }

Visual Studio, şablonu düzenlerken hangi özellikleri kullanabileceğinizi anlamanıza yardımcı olmak için IntelliSense’i kullanmanıza da olanak tanır. Örneğin, App Service planınızın özelliklerini düzenlemek için **HostingPlan** kaynağına gidin ve **resources** için yeni bir değer ekleyin. IntelliSense’in kullanılabilir değerleri gösterdiğini ve bu değerler için bir açıklama sunduğunu unutmayın.

![IntelliSense’i göster](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-intellisense.png)

**numberOfWorkers** değerini 1 olarak ayarlayabilirsiniz.

    "properties": {
      "name": "[parameters('hostingPlanName')]",
      "numberOfWorkers": 1
    }

## Azure’da Kaynak Grubu projesi dağıtma

Artık, projenizi dağıtmaya hazırsınız. Bir Azure Kaynak Grubu projesi dağıttığınızda, bunu Azure’daki web uygulamaları, veritabanları ve benzeri kaynakların mantıksal bir gruplandırması olan Azure kaynak grubuna dağıtmış olursunuz.

1. Dağıtım proje düğümünün kısayol menüsünde **Dağıt** > **Yeni Dağıtım** seçeneklerini belirleyin.

    ![Dağıt, Yeni Dağıtım menü öğesi](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/deploy.png)

    **Kaynak Grubuna Dağıt** iletişim kutusu görüntülenir.

    ![Kaynak Grubuna Dağıt İletişim Kutusu](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployment.png)

1. **Kaynak Grubu** açılır kutusunda, mevcut bir kaynak grubu seçin veya yeni bir tane oluşturun. Bir kaynak grubu oluşturmak için **Kaynak Grubu** açılır kutusunu açın **Yeni Oluştur ...** seçeneğini belirleyin.

    ![Kaynak Grubuna Dağıt İletişim Kutusu](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-new-group.png)

    **Kaynak Grubu Oluştur** iletişim kutusu görüntülenir. Grubunuz için bir ad ve konum girin ve **Oluştur** düğmesini seçin.

    ![Kaynak Grubu Oluştur İletişim Kutusu](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-resource-group.png)
   
1. **Parametreleri Düzenle** düğmesini seçerek dağıtım parametrelerini düzenleyebilirsiniz. Parametre değerlerini sağlayın ve **Kaydet** düğmesini seçin.

    ![Parametreleri Düzenle İletişim Kutusu](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/provide-parameters.png)
    
    **Parolaları parametre dosyasına düz metin olarak kaydet** seçeneği güvenli değildir.

1. Projeyi Azure’da dağıtmak için **Dağıt** düğmesini seçin. **Çıkış** penceresinde dağıtımın ilerleme durumunu görebilirsiniz. Dağıtımın tamamlanması yapılandırmanıza bağlı olarak birkaç dakika sürebilir. İstendiğinde PowerShell konsolunda veritabanı yönetici parolasını girin. Dağıtımınızdaki ilerleme durdurulursa, PowerShell konsolunda parola girmediğiniz için işlem bekletiliyor olabilir.

    >[AZURE.NOTE] Azure PowerShell cmdlet'lerini yüklemeniz istenebilir. Azure kaynak gruplarını dağıtmak için bu cmdlet'ler gerekli olduğundan, bunları yüklemeniz gerekir.
    
1. Dağıtım tamamlandığında, **Çıkış** penceresinde şuna benzer bir ileti görürsünüz:

        ...
        15:19:19 - DeploymentName     : websitesqldatabase-0212-2318
        15:19:19 - CorrelationId      : 6cb43be5-86b4-478f-9e2c-7e7ce86b26a2
        15:19:19 - ResourceGroupName  : DemoSiteGroup
        15:19:19 - ProvisioningState  : Succeeded
        ...

1. Bir tarayıcıda [Azure Portal](https://portal.azure.com/)’ı açın ve hesabınızda oturum açın. Kaynak grubunu görmek için **Kaynak grupları**’nu ve dağıttığınız kaynak grubunu seçin.

    ![grup seçme](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-group.png)

1. Dağıtılan tüm kaynakları görürsünüz.

    ![kaynakları göster](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployed-resources.png)

1. Değişiklikler yaptıysanız ve projenizi yeniden dağıtmak istiyorsanız, doğrudan Azure kaynak grubu projesinin kısayol menüsünden mevcut kaynak grubunu seçebilirsiniz. Kısayol menüsünde, **Dağıt**’ı ve ardından henüz dağıttığınız kaynak grubunu seçin.

    ![Dağıtılan Azure kaynak grubu](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/redeploy.png)

## Altyapınızla kodları dağıtma

Bu noktada, uygulamanız için altyapı dağıttınız, ancak proje ile dağıtılan gerçek bir kod yoktur. Bu konuda, bir web uygulaması ve SQL Database tablolarını dağıtım sırasında nasıl dağıtacağınız gösterilir. Bir web uygulaması yerine bir Sanal Makine dağıtıyorsanız, dağıtımının bir parçası olarak bazı kodlar çalıştırmak isteyebilirsiniz. Bir web uygulaması için kod dağıtma veya Sanal Makine kurma işlemi neredeyse aynıdır.

1. Visual Studio çözümünüzde, bir **ASP.NET Web uygulaması** ekleyin. 

    ![web uygulaması ekleme](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-app.png)
    
1. **MVC**’yi seçin ve kaynak grubu projesi bu görevi gerçekleştireceğinden **Bulutta barındır** alanını temizleyin.

    ![MVC’yi seçme](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-mvc.png)
    
1. Web uygulamanız oluşturulduktan sonra kaynak grubu projesindeki bir başvuruyu web uygulaması projesine ekleyin.

    ![başvuru ekleme](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-reference.png)
    
    Bir başvuru ekleyerek, web uygulama projesini kaynak grubu projesine bağlar ve otomatik olarak üç anahtar özellik ayarlamış olursunuz.  
    
    - **Ek Özellikler**, Azure Storage’a gönderilecek web dağıtımı paketi hazırlama konumunu içerir. 
    - **Dosya Yolu Ekle** paketin oluşturulacağı yerin yolunu içerir.  **Hedefleri Ekle** dağıtımın yürütüleceği komutu içerir. 
    - **Build;Package** varsayılan değeri, dağıtımın bir web dağıtımı paketi (package.zip) oluşturmasını sağlar.  
    
    Dağıtım paketi oluşturmak için gereken bilgileri özelliklerden elde ettiği için bir yayımlama profili gerekmez.
    
      ![başvuruya bakma](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/see-reference.png)
      
1. Şablona yeni bir kaynak ekleyin ve bu defa **Web Apps için Web Dağıtımı** seçeneğini belirleyin. 

    ![web dağıtımı ekleme](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-web-deploy.png)
    
1. Kaynak grubu projenizi kaynak grubuna yeniden dağıtın. Bu defa, bazı yeni parametre bulunur. **_artifactsLocation** ve **_artifactsLocationSasToken** değerleri otomatik olarak oluşturulduğundan, bunlar için değer sağlamanız gerekmez. Klasör ve dosya adını dağıtım paketini içeren yola ayarlayın.

    ![web dağıtımı ekleme](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/set-new-parameters.png)
    
    **Yapıt depolama hesabı** için bu kaynak grubu ile dağıtılmış bir tanesini kullanabilirsiniz.
    
Dağıtım tamamlandıktan sonra siteye göz attığınızda varsayılan ASP.NET uygulamasının dağıtılmış olduğunda dikkat edin.

![dağıtılmış uygulamayı gösterme](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployed-app.png)

## Sonraki adımlar

- Portalı kullanarak kaynaklarınızı yönetme hakkında daha fazla bilgi için bkz. [Azure Portal’ıkullanarak Azure kaynaklarınızı yönetme](./azure-portal/resource-group-portal.md).
- Şablonlar hakkında daha fazla bilgi edinmek için bkz. [Azure Resource Manager şablonları yazma](resource-group-authoring-templates.md).



<!--HONumber=Jun16_HO2-->


