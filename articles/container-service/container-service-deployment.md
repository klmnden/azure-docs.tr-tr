<properties
   pageTitle="Azure Kapsayıcı Hizmeti kümesini dağıtma | Microsoft Azure"
   description="Azure portal, Azure CLI veya PowerShell kullanarak Azure Kapsayıcı Hizmeti kümesini dağıtma."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, Kapsayıcılar, Mikro hizmetler, Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/13/2016"
   ms.author="rogardle"/>

# Azure Kapsayıcı Hizmeti kümesini dağıtma

Azure Kapsayıcı Hizmeti popüler açık kaynak kapsayıcı kümeleme ve düzenleme çözümlerinin hızlı dağıtımını sağlar. Azure Kapsayıcı Hizmeti’ni kullanarak, Azure Resource Manager şablonları ya da Azure portal ile DC/OS ve Docker Swarm kümeleri dağıtabilirsiniz. Bu kümeleri, Azure Sanal Makine Ölçekleme Kümeleri kullanarak dağıtabilirsiniz, böylece kümeler Azure ağ ve depolama sunumlarından yararlanabilir. Azure Kapsayıcı Hizmeti’ne erişmek için bir Azure aboneliği gerekir. Bir aboneliğiniz yoksa, [ücretsiz deneme için kaydolabilirsiniz](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935).

Bu belge size [Azure portal](#creating-a-service-using-the-azure-portal), [Azure komut satırı arabirimi (CLI)](#creating-a-service-using-the-azure-cli) ve [Azure PowerShell modülü](#creating-a-service-using-powershell) kullanarak Azure Kapsayıcı Hizmeti kümesi dağıtmayı adım adım gösterir.  

## Azure portalı kullanarak bir hizmet oluşturma

Azure portalda oturum açın, **Yeni**’yi seçin ve Azure Market’te **Azure Container Service**’i arayın.

![Dağıtım oluşturma 1](media/acs-portal1.png)  <br />

**Azure Container Service**’i seçin ve **Oluştur**’a tıklayın.

![Dağıtım oluşturma 2](media/acs-portal2.png)  <br />

Aşağıdaki bilgileri girin:

- **Kullanıcı adı**: Bu, Azure Container Service kümesindeki her sanal makine ve sanal makine ölçek grubunda bir hesap için kullanılacak kullanıcı adıdır.
- **Abonelik**: Bir Azure aboneliği seçin.
- **Kaynak grubu**: Yeni bir kaynak grubu seçin ya da yeni bir tane oluşturun.
- **Konum**: Azure Container Service dağıtımı için Azure bölgesini seçin.
- **SSH ortak anahtarı** – Azure Container Service sanal makinelerine karşı kimlik doğrulaması için kullanılacak ortak anahtarı ekleyin. Bu anahtarın satır sonu içermemesi ve “ssh-rsa” öneki ve “username@domain” soneki içermesi çok önemlidir. Şunun gibi görünmelidir: **ssh-rsa AAAAB3Nz...<...>...UcyupgH azureuser@linuxvm**. Güvenli Kabuk (SSH) anahtarları oluşturma yönergeleri için bkz. [Linux]( https://azure.microsoft.com/documentation/articles/virtual-machines-linux-ssh-from-linux/) ve [Windows]( https://azure.microsoft.com/documentation/articles/virtual-machines-linux-ssh-from-windows/) makaleleri.

Devam etmeye hazır olduğunuzda **Tamam**’a tıklayın.

![Dağıtım oluşturma 3](media/acs-portal3.png)  <br />

Orchestration türünü seçin. Seçenekler şunlardır:

- **DC/OS**: DC/OS kümesi dağıtır.
- **Swarm** Docker Swarm kümesi dağıtır.

Devam etmeye hazır olduğunuzda **Tamam**’a tıklayın.

![Dağıtım oluşturma 4](media/acs-portal4.png)  <br />

Aşağıdaki bilgileri girin:

- **Ana sunucu sayısı**: Kümedeki ana sunucu sayısı.
- **Aracı sayısı**: Docker Swarm için bu, aracı ölçek grubundaki başlangıç aracıları sayısıdır. DC/OS için bu, özel ölçek grubundaki başlangıç aracıları sayısıdır. Ayrıca, önceden belirlenen sayıda aracı içeren bir ortak ölçek kümesi oluşturulur. Bu ortak ölçek kümesindeki aracıların sayısı, kümede kaç tane ana sunucu oluşturulduğuna göre belirlenir; bir ana sunucu için bir ortak aracı ve üç ya da beş ana sunucu için iki ortak sunucu.
- **Aracı sanal makine boyutu**: Aracı sanal makinelerinin boyutudur.
- **DNS öneki**: Hizmet için tam uygun etki alanı adlarının temel parçalarına önek olarak eklemek için kullanılacak world benzersiz adıdır.

Devam etmeye hazır olduğunuzda **Tamam**’a tıklayın.

![Dağıtım oluşturma 5](media/acs-portal5.png)  <br />

Hizmet doğrulama tamamlandıktan sonra **Tamam**’a tıklayın.

![Dağıtım oluşturma 6](media/acs-portal6.png)  <br />

Dağıtım işlemini başlatmak için **Oluştur**’a tıklayın.

![Dağıtım oluşturma 7](media/acs-portal7.png)  <br />

Azure portalda dağıtımı sabitlemeyi seçtiyseniz dağıtım durumunu görebilirsiniz.

![Dağıtım oluşturma 8](media/acs-portal8.png)  <br />

Dağıtım tamamlandığında, Azure Kapsayıcı Hizmeti kümesi kullanım için hazırdır.

## Azure CLI kullanarak bir hizmet oluşturma

Komut satırını kullanarak Azure Kapsayıcı Hizmeti’nin bir örneğini oluşturmak için bir Azure aboneliği gerekir. Bir aboneliğiniz yoksa, [ücretsiz deneme için kaydolabilirsiniz](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935). Ayrıca Azure CLI [yüklemiş](../xplat-cli-install.md) ve [yapılandırmış](../xplat-cli-connect.md) olmanız gerekir.

DC/OS veya Docker Swarm kümesi dağıtmak için GitHub’da aşağıdaki şablonlardan birini seçin. Bu şablonların her ikisinin de, varsayılan orchestrator seçimi dışında, aynı olduğunu unutmayın.

* [DC/OS şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-mesos)
* [Swarm şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm)

Sonra, Azure CLI’nın bir Azure aboneliğine bağlı olduğundan emin olun. Aşağıdaki komutu kullanarak bunu yapabilirsiniz:

```bash
azure account show
```
Bir Azure hesabı döndürülmezse, CLI için Azure’da oturum açmak üzere aşağıdaki komutu kullanın.

```bash
azure login -u user@domain.com
```

Ardından, Azure Resource Manager'ı kullanacak şekilde Azure CLI araçlarını yapılandırın.

```bash
azure config mode arm
```

Aşağıdaki komutla bir Azure kaynak grubu ve Kapsayıcı Hizmeti Kümesi oluşturun, buradaki ifadelerin anlamları şu şekildedir:

- **RESOURCE_GROUP** bu hizmet için kullanmak istediğiniz kaynak grubunun adıdır.
- **LOCATION** kaynak grubu ve Azure Container Service dağıtımının oluşturulacağı Azure bölgesidir.
- **TEMPLATE_URI** dağıtım dosyasının konumudur. Bu, GitHub kullanıcı arabirimi işaretçisi değil Raw dosyası olmalıdır. Bu URL’yi bulmak için GitHub’da azuredeploy.json dosyasını seçin ve **Raw** düğmesine tıklayın.

> [AZURE.NOTE] Bu komutu çalıştırdığınızda, kabuk sizden dağıtım parametre değerlerini ister.

```bash
azure group create -n RESOURCE_GROUP DEPLOYMENT_NAME -l LOCATION --template-uri TEMPLATE_URI
```

### Şablon parametrelerini belirtin

Komutun bu sürümü parametreleri etkileşimli olarak tanımlamanızı gerektirir. JSON biçimli dize gibi, parametreleri sağlamak istiyorsanız bunu `-p` anahtarını kullanarak yapabilirsiniz. Örneğin:

 ```bash
azure group deployment create RESOURCE_GROUP DEPLOYMENT_NAME --template-uri TEMPLATE_URI -p '{ "param1": "value1" … }'
```

Alternatif olarak, `-e` anahtarını kullanarak JSON biçimli parametreler dosyası sağlayabilirsiniz.

```bash
azure group deployment create RESOURCE_GROUP DEPLOYMENT_NAME --template-uri TEMPLATE_URI -e PATH/FILE.JSON
```

`azuredeploy.parameters.json` adlı örnek bir parametreler dosyası görmek için, GitHub’da Azure Kapsayıcı Hizmeti şablonları ile arayın.

## PowerShell kullanarak bir hizmet oluşturma

PowerShell ile de bir Azure Kapsayıcı Hizmeti kümesi dağıtabilirsiniz. Bu belge [Azure PowerShell modülü](https://azure.microsoft.com/blog/azps-1-0/) sürüm 1.0’ı temel alır.

DC/OS veya Docker Swarm kümesi dağıtmak için aşağıdaki şablonlardan birini seçin. Bu şablonların her ikisinin de, varsayılan orchestrator seçimi dışında, aynı olduğunu unutmayın.

* [DC/OS şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-mesos)
* [Swarm şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm)

Azure aboneliğinizde küme oluşturmadan önce, PowerShell oturumunuz için Azure’da oturum açıldığını doğrulayın. Bunu `Get-AzureRMSubscription` komutuyla yapabilirsiniz.

```powershell
Get-AzureRmSubscription
```

Azure’da oturum açmanız gerekiyorsa `Login-AzureRMAccount` komutunu kullanın:

```powershell
Login-AzureRmAccount
```

Yeni bir kaynak grubuna dağıtıyorsanız, önce kaynak grubunu oluşturmalısınız. Yeni bir kaynak grubu oluşturmak için `New-AzureRmResourceGroup` komutunu kullanın ve kaynak grubu adı ile hedef bölgesini belirtin:

```powershell
New-AzureRmResourceGroup -Name GROUP_NAME -Location REGION
```

Bir kaynak grubu oluşturduktan sonra, kümenizi aşağıdaki komutla oluşturabilirsiniz. İstenen şablon URI’si `-TemplateUri` parametresi için belirtilir. Bu komutu çalıştırdığınızda, PowerShell sizden dağıtım parametre değerlerini ister.

```powershell
New-AzureRmResourceGroupDeployment -Name DEPLOYMENT_NAME -ResourceGroupName RESOURCE_GROUP_NAME -TemplateUri TEMPLATE_URI
```

### Şablon parametrelerini belirtin

PowerShell hakkında bilginiz varsa, eksi işareti (-) yazarak ve ardından SEKME tuşuna basarak bir cmdlet için kullanılabilir parametrelerde gezinebileceğinizi bilirsiniz. Bu işlev şablonunuzda tanımladığınız parametreler için de geçerlidir. Şablon adını yazmanızın hemen ardından, cmdlet şablonu getirir, parametreleri ayrıştırır ve şablon parametrelerini dinamik olarak komuta ekler. Bu, şablon parametre değerlerini belirtmeyi kolaylaştırır. Ve gerekli parametre değerini unutursanız, PowerShell sizden değeri ister.

Aşağıda parametreleri içeren tam komut verilmiştir. Kaynakların adları için kendi değerlerinizi sağlayabilirsiniz.

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName RESOURCE_GROUP_NAME-TemplateURI TEMPLATE_URI -adminuser value1 -adminpassword value2 ....
```

## Sonraki adımlar

Artık çalışan bir kümeniz olduğuna göre, bağlantı ve yönetim ayrıntıları için aşağıdaki belgelere bakın:

- [Azure Kapsayıcı Hizmeti kümesine bağlanma](container-service-connect.md)
- [Azure Container Service ve DC/OS ile çalışma](container-service-mesos-marathon-rest.md)
- [Azure Container Service ve Docker Swarm ile çalışma](container-service-docker-swarm.md)



<!--HONumber=sep16_HO2-->


