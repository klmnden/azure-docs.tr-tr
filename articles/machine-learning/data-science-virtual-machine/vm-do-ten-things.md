---
title: "Üzerinde veri bilimi sanal makinede Azure yapabilir on nokta | Microsoft Docs"
description: "Çeşitli veri keşfi ve modelleme görev veri bilimi sanal makine üzerinde gerçekleştirin."
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 145dfe3e-2bd2-478f-9b6e-99d97d789c62
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/04/2017
ms.author: gokuma;weig;bradsev
ms.openlocfilehash: 622bb5971a6ad774e770f00d2d9f44999b844d12
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="ten-things-you-can-do-on-the-data-science-virtual-machine"></a>Veri bilimi Sanal Makinesi üzerinde yapabileceğiniz on işlem

Microsoft Veri bilimi sanal makine (DSVM), çeşitli veri keşfi ve modelleme görevleri gerçekleştirmenizi sağlar güçlü veri bilimi geliştirme ortamıdır. Ortam zaten yerleşik ve çözümleme şirket için hızlı bir şekilde kullanmaya başlamak kolaylaştıran çeşitli popüler veri analiz araçları ile beraberinde gelen Bulut veya karma dağıtımlar. DSVM yakından çok sayıda Azure hizmetiyle çalışır ve okuma ve Azure, Azure SQL Data Warehouse, Azure Data Lake, Azure Storage veya Azure Cosmos veritabanı üzerinde depolanan verileri işlemek edebilir. Bunu Azure Machine Learning ve Azure Data Factory gibi diğer analiz araçları da kullanabilirsiniz.

Bu makalede biz size, DSVM çeşitli veri bilimi görevlerini gerçekleştirmek ve diğer Azure hizmetleriyle etkileşime için nasıl kullanılacağını yol. Üzerinde DSVM yapabileceği şeylerden bazıları şunlardır:

1. Verileri araştırmak ve yerel olarak Microsoft R Server, Python kullanarak DSVM modellerinde geliştirin
2. R ölçeklenebilirlik ve performans için tasarlanmış bir kurumsal hazır sürümü verilerinizde Python 2, Python 3, Microsoft R kullanarak bir tarayıcı denemeniz için Jupyter not defteri kullanın
3. İstemci uygulamaları basit web Hizmetleri arabirimi kullanarak Modellerinizi erişebilmesi için R ve Python Azure Machine Learning kullanılarak oluşturulan modelleri faaliyete
4. Azure portal veya Powershell kullanarak Azure kaynaklarınızı yönetmek
5. Depolama alanını genişletmek ve büyük ölçekli veri kümeleri paylaşmak /, DSVM takılabilir bir sürücüde olarak Azure File storage oluşturarak tüm ekibinizin arasında kod
6. Kod GitHub kullanarak takımınızla paylaşmak ve önceden yüklenmiş Git istemciler - Git Bash Git GUI kullanarak deponuza erişebilirsiniz.
7. Çeşitli Azure veri ve Analiz Hizmetleri Azure blob depolama gibi Azure Data Lake, Azure Hdınsight (Hadoop), Azure Cosmos DB, Azure SQL Data Warehouse & veritabanları erişim
8. Raporları ve panoyu Power BI üzerinde DSVM önceden yüklenmiş Masaüstü'nü kullanarak oluşturmak ve bunları buluta dağıtma
9. Proje gereksinimlerinizi karşılamak için DSVM dinamik ölçeklendirme
10. Ek araçlar sanal makinenize yükleyin   

> [!NOTE]
> Bu makalede listelenen ek veri depolama ve Analiz Hizmetleri birçoğu için ek kullanım ücretleri uygulanır. Lütfen [Azure fiyatlandırma](https://azure.microsoft.com/pricing/) Ayrıntılar için sayfa.
> 
> 

**Önkoşullar**

* Bir Azure aboneliği gerekir. Ücretsiz deneme için kaydolabilirsiniz [burada](https://azure.microsoft.com/free/).
* Azure portalındaki veri bilimi sanal makine sağlamaya yönelik yönergeler [bir sanal makine oluşturma](https://portal.azure.com/#create/microsoft-ads.standard-data-science-vmstandard-data-science-vm).

## <a name="1-explore-data-and-develop-models-using-microsoft-r-server-or-python"></a>1. Verileri araştırmak ve modeller Microsoft R Server veya Python kullanarak geliştirme
R ve Python gibi diller DSVM üzerinde veri analizi yapmak için kullanabilirsiniz.

R için Başlat menüsünde veya Masaüstü bulunabilir "Revolution R Kurumsal 8.0" adlı bir IDE kullanabilirsiniz. Microsoft açık kaynak/CRAN-ölçeklenebilir analizi ve paralel öbekli analiz yaparak izin verilen bellek boyutundan büyük veri çözümleme yeteneğini etkinleştirmek için R üstünde ek kitaplıklara sağlamıştır. Choice benzer, R IDE de yükleyebilirsiniz [Rstudio'dan](https://www.rstudio.com/products/rstudio-desktop/).

Python için Visual Studio Community Python araçları Visual Studio (PTVS) uzantısı önceden yüklenmiş olan sürümü gibi bir IDE kullanabilirsiniz. Varsayılan olarak, yalnızca temel bir Python 2.7 (olmadan herhangi bir analiz kitaplığı SciKit, Pandas gibi) üzerinde PTVS yapılandırılır. Anaconda Python 2.7 ve 3.5 etkinleştirmek için aşağıdakileri yapmanız gerekir:

* Her sürüm için özel ortamları giderek oluşturma **Araçları** -> **Python Araçları** -> **Python ortamları** ve ardından "**+ özel**" Visual Studio 2015 Community Edition'ın
* Bir açıklama girin ve ortam olarak önek yollarını ayarlamak *c:\anaconda* Anaconda Python 2.7 veya *c:\anaconda\envs\py35* Anaconda Python 3.5 için
* Tıklatın **otomatik algıla** ve ardından **Uygula** ortamı kaydetmek için.

Özel ortam kurulumu nasıl Visual Studio'da göründüğünü aşağıda verilmiştir.

![PTVS Kurulumu](./media/vm-do-ten-things/PTVSSetup.png)

Bkz: [PTVS belgelerine](https://github.com/Microsoft/PTVS/wiki/Selecting-and-Installing-Python-Interpreters#hey-i-already-have-an-interpreter-on-my-machine-but-ptvs-doesnt-seem-to-know-about-it) Python ortamları oluşturma hakkında daha fazla ayrıntı için.

Artık, yeni bir Python projesi oluşturmak için ayarlanır. Gidin **dosya** -> **yeni** -> **proje** -> **Python** ve oluşturduğunuz Python uygulama türünü seçin. Geçerli projenin Python ortamı istenen sürüm (Anaconda 2.7 ya da 3.5) ayarlayabilirsiniz: sağ **Python ortamı**seçin **Ekle/Kaldır Python ortamları**ve proje ile ilişkilendirmek için istediğiniz ortamı seçin. Ürün PTVS ile çalışma hakkında daha fazla bilgi bulabilirsiniz [belgelerine](https://github.com/Microsoft/PTVS/wiki) sayfası.

## <a name="2-using-a-jupyter-notebook-to-explore-and-model-your-data-with-python-or-r"></a>2. Keşfetmek ve Python veya R verilerinizin model için bir Jupyter Not Defteri kullanarak
Jupyter not defteri veri keşfi ve modelleme için bir tarayıcı tabanlı "IDE" sağlayan güçlü bir ortamdır. Python 2, Python 3 ya da R (hem açık kaynak hem de Microsoft R Server) bir Jupyter not defteri kullanabilirsiniz.

Başlat menüsü simgesini Jupyter not defteri tıklatıldığında başlatmak için / Masaüstü simgesi başlıklı **Jupyter not defteri**. Üzerinde DSVM de gözatabilirsiniz "https://localhost:9999 /" Jüpiter dizüstü erişmek için. İçin bir parola ister, sağlanan yönergeleri kullanın. ***Jupyter not defteri sunucuda güçlü bir parola oluşturmak nasıl*** bölümünü [Microsoft Veri bilimi sanal makine sağlama](provision-vm.md) Jupyter not defteri erişmek için güçlü bir parola oluşturmak için konu. 

Not Defteri açtıktan sonra DSVM önceden paketlenmiş birkaç örnek dizüstü bilgisayarlar içeren bir dizini görmeniz gerekir. Artık şunları yapabilirsiniz:

* Not kodu görmek için tıklatın.
* Her bir hücre tuşlarına basarak yürütme **SHIFT-ENTER**.
* Tüm Not tıklayarak çalıştırın **hücre** -> **çalıştırın**
* Jupyter simgesini (sol üst köşesinde) ve ardından yeni bir not defteri oluşturma **yeni** sağındaki ve not defteri dili (tekrar olarak da bilinir) seçme düğmesi.   

> [!NOTE]
> Python 2.7, Python 3.5 ve r şu anda destekliyoruz Hem açık kaynak R yanı sıra kuruluş programlama R çekirdek destekleyen ölçeklenebilir Microsoft R Server.   
> 
> 

Keşfetmek not defterinde olduktan sonra verilerinizi model derleme, test seçiminizi kitaplıklarını kullanarak modeli.

## <a name="3-build-models-using-r-or-python-and-operationalize-them-using-azure-machine-learning"></a>3. R veya Python ve Operationalize Azure Machine Learning kullanarak bunları kullanarak modelleri oluşturma
Yerleşik ve modelinizi doğrulanması sonra sonraki adım genellikle üretime dağıtmak olacaktır. Bu, istemci uygulamalarının modeli tahminlerin gerçek zamanlı veya toplu iş modu temelinde çağırma olanak sağlar. Azure Machine Learning R veya Python ile oluşturulmuş bir model faaliyete geçirmek için bir mekanizma sağlar.

Azure Machine Learning modelinizde faaliyete, giriş parametreleri geçirin ve çıkış modelden Öngörüler alma REST çağrılarını istemcilerin bir web hizmeti açıktır.   

> [!NOTE]
> Henüz Azure Machine Learning için kaydolmadıysanız, boş bir çalışma alanı veya standart çalışma alanını ziyaret ederek edinebilirsiniz [Azure Machine Learning Studio](https://studio.azureml.net/) giriş sayfası ve tıklayarak üzerinde "Get Started".   
> 
> 

### <a name="build-and-operationalize-python-models"></a>Derleme ve Python faaliyete modelleri
SciKit öğrenin kitaplığını kullanarak basit bir model oluşturur bir Python Jupyter not defterinde geliştirilmiş kod parçacığı aşağıda verilmiştir.

    #IRIS classification
    from sklearn import datasets
    from sklearn import svm
    clf = svm.SVC()
    iris = datasets.load_iris()
    X, y = iris.data, iris.target
    clf.fit(X, y)

Azure Machine Learning ile python Modellerinizi dağıtmak için kullanılan yöntemi modeli tahmin bir işlevine sarmalar ve Azure Machine Learning çalışma alanı kimliği, API anahtarı ve giriş belirtmek ve dönüş parametreleri önceden yüklenmiş Azure Machine Learning python kitaplığı tarafından sağlanan özniteliklerle süsler.  

    from azureml import services
    @services.publish(workspaceid, auth_token)
    @services.types(sep_l = float, sep_w = float, pet_l=float, pet_w=float)
    @services.returns(int) #0, or 1, or 2
    def predictIris(sep_l, sep_w, pet_l, pet_w):
     inputArray = [sep_l, sep_w, pet_l, pet_w]
    return clf.predict(inputArray)

Bir istemci web hizmeti çağrıları şimdi yapabilirsiniz. REST API istekleri oluşturmak kolaylık sarmalayıcıları vardır. Web hizmeti kullanmak için örnek kod aşağıda verilmiştir.

    # Consume through web service URL and keys
    from azureml import services
    @services.service(url, api_key)
    @services.types(sep_l = float, sep_w = float, pet_l=float, pet_w=float)
    @services.returns(float)
    def IrisPredictor(sep_l, sep_w, pet_l, pet_w):
    pass

    IrisPredictor(3,2,3,4)


> [!NOTE]
> Azure Machine Learning kitaplığı yalnızca şu anda Python 2.7 üzerinde desteklenir.   
> 
> 

### <a name="build-and-operationalize-r-models"></a>Derleme ve faaliyete R modelleri
Python için nasıl yapıldığında için benzer bir şekilde üzerinde veri bilimi sanal makine ya da başka bir yerde Azure Machine Learning üzerine yerleşik R modelleri dağıtabilirsiniz. Kendi adımlar şunlardır:

* Çalışma alanınızı sağlamak üzere settings.json oluşturma Kimliğini ve kimlik doğrulama belirteci aşağıdaki kod örneğinde gösterildiği gibi.
* modelin işlevi tahmin etmek için bir sarmalayıcı yazma.
* çağrı ```publishWebService``` işlevi sarmalayıcı geçirmek için Azure Machine Learning Kitaplığı'nda.  

Ayarlama, yapı, yayımlama ve bir Azure Machine Learning web hizmeti olarak modeli kullanmak için kullanılan yordam ve kod parçacıkları aşağıdadır.

#### <a name="setup"></a>Kurulum
1. Machine Learning R paketi yazarak yüklemek ```install.packages("AzureML")``` Revolution R Kurumsal 8.0 IDE veya R IDE.
2. Gelen RTools karşıdan [burada](https://cran.r-project.org/bin/windows/Rtools/). Sıkıştırma yardımcı programı, R paketi Machine Learning faaliyete geçirmenin yol (ve adlandırılmış zip.exe) gerekir.
3. Adlı bir dizin altında settings.json dosyası oluşturma ```.azureml``` giriş dizininize altında ve Azure Machine Learning alanınızdan parametreleri girin:

Settings.JSON dosya yapısı:

    {"workspace":{
    "id"                  : "ENTER YOUR AZUREML WORKSPACE ID",
    "authorization_token" : "ENTER YOUR AZUREML AUTH TOKEN"
    }}


#### <a name="build-a-model-in-r-and-publish-it-in-azure-machine-learning"></a>R bir model oluşturmak ve Azure Machine Learning ile yayımlama
    library(AzureML)
    ws <- workspace(config="~/.azureml/settings.json")

    if(!require("lme4")) install.packages("lme4")
    library(lme4)
    set.seed(1)
    train <- sleepstudy[sample(nrow(sleepstudy), 120),]
    m <- lm(Reaction ~ Days + Subject, data = train)

    # Define a prediction function to publish based on the model:
    sleepyPredict <- function(newdata){
          predict(m, newdata=newdata)
    }

    ep <- publishWebService(ws, fun = sleepyPredict, name="sleepy lm", inputSchema = sleepstudy, data.frame=TRUE)

#### <a name="consume-the-model-deployed-in-azure-machine-learning"></a>Azure Machine Learning ile dağıtılan model kullanma
Bir istemci uygulaması modelden kullanmak için Azure Machine Learning kitaplığı adı kullanarak yayımlanan web hizmeti bakmak için kullanırız `services` uç nokta belirlemek için API çağrısı. Yalnızca çağrı sonra `consume` işlev ve tahmin için veri çerçevede geçirin.
Aşağıdaki kod, bir Azure Machine Learning web hizmeti olarak yayımlanan modelini kullanmak için kullanılır.

    library(AzureML)
    library(lme4)
    ws <- workspace(config="~/.azureml/settings.json")

    s <-  services(ws, name = "sleepy lm")
    s <- tail(s, 1) # use the last published function, in case of duplicate function names

    ep <- endpoints(ws, s)

    # OK, try this out, and compare with raw data
    ans = consume(ep, sleepstudy)$ans

Azure Machine Learning R Kitaplığı hakkında daha fazla bilgi bulunabilir [burada](https://cran.r-project.org/web/packages/AzureML/AzureML.pdf).

## <a name="4-administer-your-azure-resources-using-azure-portal-or-powershell"></a>4. Azure portal veya Powershell kullanarak Azure kaynaklarınızı yönetmek
DSVM yalnızca analytics çözümünüzü sanal makinede yerel olarak oluşturmanıza olanak sağlayan, ancak aynı zamanda Microsoft Azure bulut hizmetlerini erişmenize olanak tanır. Azure birkaç işlem, depolama, veri analizi Hizmetleri ve yönetmek ve sizin DSVM erişim diğer hizmetler sağlar.

Üzerine gelin ve tarayıcınızı Azure aboneliği ve bulut kaynaklarınızı yönetmek için [Azure portal](https://portal.azure.com). Azure aboneliği ve kaynakların bir komut dosyası aracılığıyla yönetmek için Azure Powershell de kullanabilirsiniz.
"Microsoft Azure Powershell" başlıklı Başlat menüsünden veya masaüstündeki kısayoldan Azure Powershell komutunu çalıştırabilirsiniz. Başvurmak [Microsoft Azure Powershell belgelerine](../../powershell-azure-resource-manager.md) Azure aboneliği ve Windows Powershell betiklerini kullanarak kaynakları nasıl yönetebileceğiniz hakkında daha fazla bilgi.

## <a name="5-extend-your-storage-space-with-a-shared-file-system"></a>5. Depolama alanınızı paylaşılan dosya sistemiyle genişletme
Veri bilimcilerine büyük veri kümeleri, kod veya başka kaynaklara takım içindeki paylaşabilirsiniz. DSVM yaklaşık 70 GB alanınız vardır. Depolama alanınızın genişletmek için Azure dosya hizmeti kullanabilirsiniz ve ya da üzerinde DSVM bağlayın veya bir REST API üzerinden erişim.   

> [!NOTE]
> Azure dosya hizmeti paylaşımı maksimum alan 5 TB ve tek tek dosya boyutu sınırı 1 TB.   
> 
> 

Bir Azure dosya hizmeti paylaşımı oluşturmak için Azure PowerShell'i kullanabilirsiniz. Burada, Azure dosya hizmeti paylaşımı oluşturmak için Azure PowerShell altında çalıştırmak için betik verilmiştir.

    # Authenticate to Azure.
    Login-AzureRmAccount
    # Select your subscription
    Get-AzureRmSubscription –SubscriptionName "<your subscription name>" | Select-AzureRmSubscription
    # Create a new resource group.
    New-AzureRmResourceGroup -Name <dsvmdatarg>
    # Create a new storage account. You can reuse existing storage account if you wish.
    New-AzureRmStorageAccount -Name <mydatadisk> -ResourceGroupName <dsvmdatarg> -Location "<Azure Data Center Name For eg. South Central US>" -Type "Standard_LRS"
    # Set your current working storage account
    Set-AzureRmCurrentStorageAccount –ResourceGroupName "<dsvmdatarg>" –StorageAccountName <mydatadisk>

    # Create a Azure File Service Share
    $s = New-AzureStorageShare <<teamsharename>>
    # Create a directory under the FIle share. You can give it any name
    New-AzureStorageDirectory -Share $s -Path <directory name>
    # List the share to confirm that everything worked
    Get-AzureStorageFile -Share $s


Azure dosya paylaşımının oluşturduğunuza göre herhangi bir sanal makine azure'da bağlayabilir. VM gecikme süresi ve veri aktarımı ücretlerine önlemek için depolama hesabı aynı Azure veri merkezinde olduğunu önerilir. Azure Powershell çalıştırabilirsiniz DSVM üzerinde sürücüyü bağlamak için komutlar şunlardır.

    # Get storage key of the storage account that has the Azure file share from Azure portal. Store it securely on the VM to avoid prompted in next command.
    cmdkey /add:<<mydatadisk>>.file.core.windows.net /user:<<mydatadisk>> /pass:<storage key>

    # Mount the Azure file share as Z: drive on the VM. You can chose another drive letter if you wish
    net use z:  \\<mydatadisk>.file.core.windows.net\<<teamsharename>>


Artık VM normal bir sürücüye gibi bu sürücüyü erişebilirsiniz.

## <a name="6-share-code-with-your-team-using-github"></a>6. Kod GitHub kullanarak takımınızla paylaşmak
GitHub Geliştirici topluluğu tarafından paylaşılan çeşitli teknolojiler kullanılarak farklı araçlar için örnek kod ve kaynakları çok bulabileceğiniz bir kod depodur. Git, izlemek ve kod dosyaları sürümlerini depolamak için teknoloji olarak kullanır. GitHub ayrıca görüntüleme ve kod katkıda erişimine sahip ekibinizin paylaşılan kodu ve belgeler depolamak, sürüm denetimi uygulamak ve ayrıca denetlemek için kendi depo oluşturabileceğiniz platformudur. Lütfen şu adresi ziyaret [GitHub yardım sayfalarına](https://help.github.com/) Git kullanma hakkında daha fazla bilgi için. GitHub ekibinizle işbirliği, topluluk tarafından geliştirilmiş kodu kullanın ve kod topluluğa katkıda bulunma yolları biri olarak kullanabilirsiniz.

DSVM zaten iyi GUI GitHub deposuna erişmek için komut satırı hem istemci araçları ile yüklenen gelir. Git ve GitHub ile çalışmak için komut satırı aracı, Git Bash adı verilir. DSVM üzerinde yüklü visual Studio Git uzantısına sahip. Başlat menüsünde ve masaüstünde bu araçları için başlangıç simge bulabilirsiniz.

Kullandığınız bir GitHub depodan kodu indirmek için ```git clone``` komutu. Örneğin, eklendiğinde geçerli dizine Microsoft tarafından yayımlanan veri bilimi havuzu karşıdan yüklemek için aşağıdaki komutu çalıştırabilirsiniz ```git-bash```.

    git clone https://github.com/Azure/Azure-MachineLearning-DataScience.git

Visual Studio'da aynı kopyalama işlemi yapabilirsiniz. Aşağıdaki ekran görüntüsünde, Visual Studio Git ve GitHub araçlara erişmek gösterilmiştir.

![Visual Studio'da Git](./media/vm-do-ten-things/VSGit.PNG)

Git, GitHub deposunu kullanılabilir çeşitli kaynaklardan github.com'u üzerinde çalışmak için kullanma hakkında daha fazla bilgi bulabilirsiniz. [Kopya sayfası](https://training.github.com/kit/downloads/github-git-cheat-sheet.pdf) yararlı bir başvurudur.

## <a name="7-access-various-azure-data-and-analytics-services"></a>7. Çeşitli Azure verileri ve çözümlemeler hizmetlere erişme
### <a name="azure-blob"></a>Azure Blob
Azure blob verileri büyük ve küçük için güvenilir ve ekonomik bulut Depolama ' dir. Bize nasıl, verileri Azure Blob ve bir Azure Blob depolanan verilere erişmek için taşıyabilirsiniz arayın.

**Önkoşul**

* **Azure Blob storage hesabınızdan oluşturma [Azure portal](https://portal.azure.com).**

![Create_Azure_Blob](./media/vm-do-ten-things/Create_Azure_Blob.PNG)

* Önceden yüklenmiş komut satırı AzCopy aracı da bulunur onaylayın ```C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy.exe```. Bu aracı çalıştırırken tam komut yolu yazarak önlemek için yol ortam değişkenine azcopy.exe içeren dizini ekleyebilirsiniz. AzCopy aracı hakkında daha fazla bilgi için lütfen [AzCopy belgeleri](../../storage/common/storage-use-azcopy.md)
* Azure Storage Gezgini Aracı'nı başlatın. Adresten yüklenebilir [Microsoft Azure Storage Gezgini](http://storageexplorer.com/). 

![AzureStorageExplorer_v4](./media/vm-do-ten-things/AzureStorageExplorer_v4.png)

**Verileri Azure Blob VM'den taşımak: AzCopy**

Blob depolama ve yerel dosyalarınızı arasında verileri taşımak için AzCopy komut satırında kullanabilirsiniz veya PowerShell:

    AzCopy /Source:C:\myfolder /Dest:https://<mystorageaccount>.blob.core.windows.net/<mycontainer> /DestKey:<storage account key> /Pattern:abc.txt

Değiştir **C:\myfolder** dosyanızı depolandığı yola **mystorageaccount** blob depolama hesabı adı için **mycontainer** kapsayıcı adı için **depolama hesabı anahtarı** blob depolama erişim anahtarınızı için. Depolama hesabı kimlik bilgilerinizi bulabilirsiniz [Azure portal](https://portal.azure.com).

![StorageAccountCredential_v2](./media/vm-do-ten-things/StorageAccountCredential_v2.png)

PowerShell veya bir komut isteminden AzCopy komutunu çalıştırın. Bazı örnek kullanım AzCopy komut şöyledir:

    # Copy *.sql from local machine to a Azure Blob
    "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:"c:\Aaqs\Data Science Scripts" /Dest:https://[ENTER STORAGE ACCOUNT].blob.core.windows.net/[ENTER CONTAINER] /DestKey:[ENTER STORAGE KEY] /S /Pattern:*.sql

    # Copy back all files from Azure Blob container to Local machine

    "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Dest:"c:\Aaqs\Data Science Scripts\temp" /Source:https://[ENTER STORAGE ACCOUNT].blob.core.windows.net/[ENTER CONTAINER] /SourceKey:[ENTER STORAGE KEY] /S



Bir Azure blob kopyalamak için AzCopy komutu çalıştırdıktan sonra Azure depolama Gezgini'nde, dosya programları kısa süre içinde görürsünüz.

![AzCopy_run_finshed_Storage_Explorer_v3](./media/vm-do-ten-things/AzCopy_run_finshed_Storage_Explorer_v3.png)

**Verileri Azure Blob VM'den taşımak: Azure Storage Gezgini**

Yerel dosya verileri VM'nizi Azure Depolama Gezgini'ni kullanarak da yükleyebilirsiniz:

* Verileri bir kapsayıcıya yüklemek için hedef kapsayıcıyı seçin ve **karşıya** düğmesini.![ Depolama Gezgini'nde karşıya yükle](./media/vm-do-ten-things/storage-accounts.png)
* Tıklayın **...**  sağındaki **dosyaları** kutusunda, dosya sisteminden karşıya tıklatıp bir veya birden çok dosya seçin **karşıya** dosyaları karşıya başlamaya.![ BLOB dosya karşıya yükleme](./media/vm-do-ten-things/upload-files-to-blob.png)

**Verileri Azure Blob'tan okuyun: Machine Learning okuyucu Modülü**

Azure Machine Learning Studio'da kullanabileceğiniz bir **veri içeri aktarma modülü** blob üzerinden veri okunamıyor.

![AML_ReaderBlob_Module_v3](./media/vm-do-ten-things/AML_ReaderBlob_Module_v3.png)

**Verileri Azure Blob'tan okuyun: Python ODBC**

Kullanabileceğiniz **BlobService** Jupyter Not Defteri veya Python programında blob verilerini doğrudan okumak için kitaplık.

İlk olarak, gerekli paketleri içeri aktarın:

    import pandas as pd
    from pandas import Series, DataFrame
    import numpy as np
    import matplotlib.pyplot as plt
    from time import time
    import pyodbc
    import os
    from azure.storage.blob import BlobService
    import tables
    import time
    import zipfile
    import random

Ardından Azure Blob hesabı kimlik bilgilerinizi Tak ve Kullan veri Blobundan okuyun:

    CONTAINERNAME = 'xxx'
    STORAGEACCOUNTNAME = 'xxxx'
    STORAGEACCOUNTKEY = 'xxxxxxxxxxxxxxxx'
    BLOBNAME = 'nyctaxidataset/nyctaxitrip/trip_data_1.csv'
    localfilename = 'trip_data_1.csv'
    LOCALDIRECTORY = os.getcwd()
    LOCALFILE =  os.path.join(LOCALDIRECTORY, localfilename)

    #download from blob
    t1 = time.time()
    blob_service = BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
    blob_service.get_blob_to_path(CONTAINERNAME,BLOBNAME,LOCALFILE)
    t2 = time.time()
    print(("It takes %s seconds to download "+BLOBNAME) % (t2 - t1))

    #unzipping downloaded files if needed
    #with zipfile.ZipFile(ZIPPEDLOCALFILE, "r") as z:
    #    z.extractall(LOCALDIRECTORY)

    df1 = pd.read_csv(LOCALFILE, header=0)
    df1.columns = ['medallion','hack_license','vendor_id','rate_code','store_and_fwd_flag','pickup_datetime','dropoff_datetime','passenger_count','trip_time_in_secs','trip_distance','pickup_longitude','pickup_latitude','dropoff_longitude','dropoff_latitude']
    print 'the size of the data is: %d rows and  %d columns' % df1.shape

Veriler, veri çerçeve olarak okunur:

![IPNB_data_readin](./media/vm-do-ten-things/IPNB_data_readin.PNG)

### <a name="azure-data-lake"></a>Azure Data Lake
Azure Data Lake Store, büyük veri analitik iş yükleri ve uyumlu olan Hadoop dağıtılmış dosya sistemi (HDFS) için hiper ölçekli bir depodur. Hadoop ekosistemine ve Azure Data Lake Analytics ile çalışır. Nasıl Azure Data Lake Store verilerini taşımak ve Azure Data Lake Analytics'i kullanarak analizler çalıştırır gösteriyoruz.

**Önkoşul**

* İçinde Azure Data Lake Analytics oluşturma [Azure portal](https://portal.azure.com).

![Azure_Data_Lake_Create_v2](./media/vm-do-ten-things/Azure_Data_Lake_Create_v2.png)

* **Azure Data Lake Araçları** içinde **Visual Studio** şu anda bulunan [bağlantı](https://www.microsoft.com/download/details.aspx?id=49504) Visual Studio Community sanal makinede olan sürümü zaten yüklü. Visual Studio başlangıç ve Azure aboneliğinizde oturum sonra Azure Data Analytics hesabınızı ve Visual Studio'nun sol panelinde depolama görmeniz gerekir.

![Azure_Data_Lake_PlugIn_v2](./media/vm-do-ten-things/Azure_Data_Lake_PlugIn_v2.PNG)

**Data Lake VM'den veri taşıma: Azure Data Lake Gezgini**

Kullanabileceğiniz **Azure Data Lake Explorer** sanal makinenizde yerel dosyalarında verileri Data Lake depolama alanına yüklemek için.

![Azure_Data_Lake_UploadData](./media/vm-do-ten-things/Azure_Data_Lake_UploadData.PNG)

Ayrıca, veri taşıma için veya Azure Data Lake kullanarak productionize için veri ardışık oluşturabilirsiniz [Azure veri Factory(ADF)](https://azure.microsoft.com/services/data-factory/). Biz, bu başvuru [makale](https://azure.microsoft.com/blog/creating-big-data-pipelines-using-azure-data-lake-and-azure-data-factory/) verileri oluşturmak için adımlarda size yol göstermesi için ardışık düzenleri.

**Verileri Azure Blob'tan Data Lake okuyun: U-SQL**

Verileriniz Azure Blob depolama alanına bulunuyorsa, U-SQL sorgusunda Azure depolama blobunu gelen verileri doğrudan okuyabilir. U-SQL sorgusu oluşturma önce blob storage hesabı, Azure Data Lake bağlı olduğundan emin olun. Git **Azure portal**, Azure Data Lake Analytics panonuz bulmak, tıklatın **veri kaynağı Ekle**, depolama türü seçin **Azure Storage** , Azure depolama hesabı adı ve anahtarınız takın. Ardından depolama hesabında depolanan verileri başvuru yapabilir.

![Depolama hesabı ve anahtarı girin](./media/vm-do-ten-things/Link_Blob_to_ADLA_v2.PNG)

Visual Studio'da veri blob depolama alanından okuyun, bazı veri değişikliği yapın, mühendislik özellik ve çıktı sonuç verilerini Azure Data Lake veya Azure Blob Storage. Blob depolama birimindeki verileri başvurduğunuzda kullanmak **wasb: / /**; Azure Data Lake, kullanım verileri başvurduğunuzda **swbhdfs: / /**

![Veri çerçevesi](./media/vm-do-ten-things/USQL_Read_Blob_v2.PNG)

Visual Studio'da aşağıdaki U-SQL sorgularını kullanabilirsiniz:

    @a =
        EXTRACT medallion string,
                hack_license string,
                vendor_id string,
                rate_code string,
                store_and_fwd_flag string,
                pickup_datetime string,
                dropoff_datetime string,
                passenger_count int,
                trip_time_in_secs double,
                trip_distance double,
                pickup_longitude string,
                pickup_latitude string,
                dropoff_longitude string,
                dropoff_latitude string

        FROM "wasb://<Container name>@<Azure Blob Storage Account Name>.blob.core.windows.net/<Input Data File Name>"
        USING Extractors.Csv();

    @b =
        SELECT vendor_id,
        COUNT(medallion) AS cnt_medallion,
        SUM(passenger_count) AS cnt_passenger,
        AVG(trip_distance) AS avg_trip_dist,
        MIN(trip_distance) AS min_trip_dist,
        MAX(trip_distance) AS max_trip_dist,
        AVG(trip_time_in_secs) AS avg_trip_time
        FROM @a
        GROUP BY vendor_id;

    OUTPUT @b   
    TO "swebhdfs://<Azure Data Lake Storage Account Name>.azuredatalakestore.net/<Folder Name>/<Output Data File Name>"
    USING Outputters.Csv();

    OUTPUT @b   
    TO "wasb://<Container name>@<Azure Blob Storage Account Name>.blob.core.windows.net/<Output Data File Name>"
    USING Outputters.Csv();



Sorgunuz sunucuya gönderildikten sonra işin durumunu gösteren bir diyagram görüntülenir.

![İş durumu diyagramı](./media/vm-do-ten-things/USQL_Job_Status.PNG)

**Data Lake veri sorgulama: U-SQL**

Veri kümesi Azure Data Lake alınan sonra kullanabileceğiniz [U-SQL dili](../../data-lake-analytics/data-lake-analytics-u-sql-get-started.md) sorgulamak ve verileri araştırmak için. U-SQL dili T-SQL benzer, ancak bazı özellikler C# üzerinden böylece kullanıcılar özelleştirilmiş modüller, kullanıcı tanımlı işlevler ve vb. yazabilirsiniz birleştirir. Önceki adımda komut dosyalarını kullanabilirsiniz.

Sorgu sonra tripdata_summary sunucusuna gönderilir. CSV, kısa süre içinde bulunabilir **Azure Data Lake Explorer**, sağ tarafından veri dosya önizleme.

![Azure Data Lake Gezgini'ndeki Dosya](./media/vm-do-ten-things/USQL_create_summary.png)

Dosya bilgileri görmek için:

![Dosya Özeti](./media/vm-do-ten-things/USQL_tripdata_summary.png)

### <a name="hdinsight-hadoop-clusters"></a>Hdınsight Hadoop kümeleri
Azure Hdınsight bir yönetilen Apache Hadoop, Spark, HBase ve Storm ilgili bulut hizmetidir. Azure Hdınsight kümeleri veri bilimi sanal makineden kolayca çalışabilirsiniz.

**Önkoşul**

* Azure Blob storage hesabınızdan oluşturma [Azure portal](https://portal.azure.com). Bu depolama hesabı, Hdınsight kümeleri verilerini depolamak için kullanılır.

![Azure Blob storage hesabı oluşturma](./media/vm-do-ten-things/Create_Azure_Blob.PNG)

* Azure Hdınsight Hadoop kümelerini gelen özelleştirme [Azure portalı](../team-data-science-process/customize-hadoop-cluster.md)
  
  * Hdınsight kümenizle oluşturulduğunda oluşturulan depolama hesabı bağlamanız gerekir. Bu depolama hesabı küme içinde işlenebilecek verilerine erişmek için kullanılır.

![Hdınsight kümesi ile oluşturulan depolama hesabı bağlantı](./media/vm-do-ten-things/Create_HDI_v4.PNG)

* Etkinleştirmelisiniz **uzaktan erişim** oluşturulduktan sonra kümenin baş düğümüne. Burada belirttiğiniz (oluşturulduktan konumundaki küme için belirtilen kullanılanlardan farklı) uzaktan erişim kimlik bilgilerini Hatırla: sonraki yordamda gerekir.

![Uzaktan erişimi etkinleştirin](./media/vm-do-ten-things/Create_HDI_dashboard_v3.PNG)

* Bir Azure Machine Learning çalışma alanı oluşturun. Makine öğrenimi denemelerine bu Machine Learning çalışma alanında depolanır. Vurgulanan seçenekleri Portalı'nda aşağıdaki ekran görüntüsünde gösterildiği gibi seçin:

![Bir Azure Machine Learning çalışma alanı oluşturma](./media/vm-do-ten-things/Create_ML_Space.PNG)

* Ardından parametreleri için çalışma alanınızı girin

![Machine Learning çalışma alanı parametreleri girin](./media/vm-do-ten-things/Create_ML_Space_step2_v2.PNG)

* IPython Not Defteri kullanarak verileri karşıya yükleyin. İlk gerekli paketleri içeri aktarmak, kimlik bilgilerini takın, bir db depolama hesabınızı oluşturmak ve sonra veri HDI kümelerine yükleme.

        #Import required Packages
        import pyodbc
        import time as time
        import json
        import os
        import urllib
        import urllib2
        import warnings
        import re
        import pandas as pd
        import matplotlib.pyplot as plt
        from azure.storage.blob import BlobService
        warnings.filterwarnings("ignore", category=UserWarning, module='urllib2')


        #Create the connection to Hive using ODBC
        SERVER_NAME='xxx.azurehdinsight.net'
        DATABASE_NAME='nyctaxidb'
        USERID='xxx'
        PASSWORD='xxxx'
        DB_DRIVER='Microsoft Hive ODBC Driver'
        driver = 'DRIVER={' + DB_DRIVER + '}'
        server = 'Host=' + SERVER_NAME + ';Port=443'
        database = 'Schema=' + DATABASE_NAME
        hiveserv = 'HiveServerType=2'
        auth = 'AuthMech=6'
        uid = 'UID=' + USERID
        pwd = 'PWD=' + PASSWORD
        CONNECTION_STRING = ';'.join([driver,server,database,hiveserv,auth,uid,pwd])
        connection = pyodbc.connect(CONNECTION_STRING, autocommit=True)
        cursor=connection.cursor()


        #Create Hive database and tables
        queryString = "create database if not exists nyctaxidb;"
        cursor.execute(queryString)

        queryString = """
                        create external table if not exists nyctaxidb.trip
                        (
                            medallion string,
                            hack_license string,
                            vendor_id string,
                            rate_code string,
                            store_and_fwd_flag string,
                            pickup_datetime string,
                            dropoff_datetime string,
                            passenger_count int,
                            trip_time_in_secs double,
                            trip_distance double,
                            pickup_longitude double,
                            pickup_latitude double,
                            dropoff_longitude double,
                            dropoff_latitude double)  
                        PARTITIONED BY (month int)
                        ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\\n'
                        STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/trip' TBLPROPERTIES('skip.header.line.count'='1');
                    """
        cursor.execute(queryString)

        queryString = """
                        create external table if not exists nyctaxidb.fare
                        (
                            medallion string,
                            hack_license string,
                            vendor_id string,
                            pickup_datetime string,
                            payment_type string,
                            fare_amount double,
                            surcharge double,
                            mta_tax double,
                            tip_amount double,
                            tolls_amount double,
                            total_amount double)
                        PARTITIONED BY (month int)
                        ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\\n'
                        STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/fare' TBLPROPERTIES('skip.header.line.count'='1');
                    """
        cursor.execute(queryString)


        #Upload data from blob storage to HDI cluster
        for i in range(1,13):
            queryString = "LOAD DATA INPATH 'wasb:///nyctaxitripraw2/trip_data_%d.csv' INTO TABLE nyctaxidb2.trip PARTITION (month=%d);"%(i,i)
            cursor.execute(queryString)
            queryString = "LOAD DATA INPATH 'wasb:///nyctaxifareraw2/trip_fare_%d.csv' INTO TABLE nyctaxidb2.fare PARTITION (month=%d);"%(i,i)  
            cursor.execute(queryString)


* Alternatif olarak, bu izleyebilirsiniz [izlenecek](../team-data-science-process/hive-walkthrough.md) HDI kümesine NYC ücreti verileri yüklemek için. Önemli adımlar şunlardır:
  
  * AzCopy: daraltılmış CSV ait ortak blob üzerinden, yerel bir klasöre indirin
  * AzCopy: sıkıştırması açılmış CSV ait yerel klasörden HDI kümesine karşıya yükle
  * Hadoop küme baş düğümüne oturum ve keşif veri analizi için hazırlama

Veriler HDI kümesine yüklendikten sonra verilerinizi Azure Storage Gezgini kontrol edebilirsiniz. Ve HDI kümesi içinde oluşturulan bir veritabanı nyctaxidb vardır.

**Veri keşfi: Python Hive sorguları**

Verileri Hadoop kümesinde olduğundan, Hive araştırması yapmak ve mühendislik özelliğini kullanarak Hadoop kümeleri ve sorgu veritabanına bağlanmak için pyodbc paketi kullanabilirsiniz. Önkoşul adımda oluşturduğumuz varolan tablolardan görüntüleyebilirsiniz.

    queryString = """
        show tables in nyctaxidb2;
        """
    pd.read_sql(queryString,connection)


![Var olan tabloları görüntüleme](./media/vm-do-ten-things/Python_View_Existing_Tables_Hive_v3.PNG)

Kayıt sayısı her ay ve sıklığını bakalım Eğimli ya da seyahat tablosunda yok:

    queryString = """
        select month, count(*) from nyctaxidb.trip group by month;
        """
    results = pd.read_sql(queryString,connection)

    %matplotlib inline

    results.columns = ['month', 'trip_count']
    df = results.copy()
    df.index = df['month']
    df['trip_count'].plot(kind='bar')


![Çizim kayıtları her ay sayısı](./media/vm-do-ten-things/Exploration_Number_Records_by_Month_v3.PNG)

    queryString = """
        SELECT tipped, COUNT(*) AS tip_freq
        FROM
        (
            SELECT if(tip_amount > 0, 1, 0) as tipped, tip_amount
            FROM nyctaxidb.fare
        )tc
        GROUP BY tipped;
        """
    results = pd.read_sql(queryString,connection)

    results.columns = ['tipped', 'trip_count']
    df = results.copy()
    df.index = df['tipped']
    df['trip_count'].plot(kind='bar')


![İpucu frekansların çizim](./media/vm-do-ten-things/Exploration_Frequency_tip_or_not_v3.PNG)

Ayrıca toplama konumu ve dropoff konumu arasındaki mesafeyi işlem ve seyahat uzaklık karşılaştırın.

    queryString = """
                    select pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude, trip_distance, trip_time_in_secs,
                        3959*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
                        *radians(180)/180/2),2)-cos(pickup_latitude*radians(180)/180)
                        *cos(dropoff_latitude*radians(180)/180)*pow(sin((dropoff_longitude-pickup_longitude)*radians(180)/180/2),2)))
                        /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*radians(180)/180/2),2)
                        +cos(pickup_latitude*radians(180)/180)*cos(dropoff_latitude*radians(180)/180)*
                        pow(sin((dropoff_longitude-pickup_longitude)*radians(180)/180/2),2))) as direct_distance
                        from nyctaxidb.trip
                        where month=1
                            and pickup_longitude between -90 and -30
                            and pickup_latitude between 30 and 90
                            and dropoff_longitude between -90 and -30
                            and dropoff_latitude between 30 and 90;
                """
    results = pd.read_sql(queryString,connection)
    results.head(5)


![Toplama ve dropoff tablosu](./media/vm-do-ten-things/Exploration_compute_pickup_dropoff_distance_v2.PNG)

    results.columns = ['pickup_longitude', 'pickup_latitude', 'dropoff_longitude',
                       'dropoff_latitude', 'trip_distance', 'trip_time_in_secs', 'direct_distance']
    df = results.loc[results['trip_distance']<=100] #remove outliers
    df = df.loc[df['direct_distance']<=100] #remove outliers
    plt.scatter(df['direct_distance'], df['trip_distance'])


![Seyahat uzaklığı alma/dropoff uzaklığı çizim](./media/vm-do-ten-things/Exploration_direct_distance_trip_distance_v2.PNG)

Şimdi şimdi modelleme için bir aşağı örneklenen (% 1) veri kümesi hazırlayın. Machine Learning okuyucu modülünde size bu verileri kullanabilirsiniz.

        queryString = """
        create  table if not exists nyctaxi_downsampled_dataset_testNEW (
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        pickup_hour string,
        pickup_week string,
        weekday string,
        passenger_count int,
        trip_time_in_secs double,
        trip_distance double,
        pickup_longitude double,
        pickup_latitude double,
        dropoff_longitude double,
        dropoff_latitude double,
        direct_distance double,
        payment_type string,
        fare_amount double,
        surcharge double,
        mta_tax double,
        tip_amount double,
        tolls_amount double,
        total_amount double,
        tipped string,
        tip_class string
        )
        row format delimited fields terminated by ','
        lines terminated by '\\n'
        stored as textfile;
        """
        cursor.execute(queryString)

        --- now insert contents of the join into the preceding internal table

        queryString = """
        insert overwrite table nyctaxi_downsampled_dataset_testNEW
        select
        t.medallion,
        t.hack_license,
        t.vendor_id,
        t.rate_code,
        t.store_and_fwd_flag,
        t.pickup_datetime,
        t.dropoff_datetime,
        hour(t.pickup_datetime) as pickup_hour,
        weekofyear(t.pickup_datetime) as pickup_week,
        from_unixtime(unix_timestamp(t.pickup_datetime, 'yyyy-MM-dd HH:mm:ss'),'u') as weekday,
        t.passenger_count,
        t.trip_time_in_secs,
        t.trip_distance,
        t.pickup_longitude,
        t.pickup_latitude,
        t.dropoff_longitude,
        t.dropoff_latitude,
        t.direct_distance,
        f.payment_type,
        f.fare_amount,
        f.surcharge,
        f.mta_tax,
        f.tip_amount,
        f.tolls_amount,
        f.total_amount,
        if(tip_amount>0,1,0) as tipped,
        if(tip_amount=0,0,
        if(tip_amount>0 and tip_amount<=5,1,
        if(tip_amount>5 and tip_amount<=10,2,
        if(tip_amount>10 and tip_amount<=20,3,4)))) as tip_class
        from
        (
        select
        medallion,
        hack_license,
        vendor_id,
        rate_code,
        store_and_fwd_flag,
        pickup_datetime,
        dropoff_datetime,
        passenger_count,
        trip_time_in_secs,
        trip_distance,
        pickup_longitude,
        pickup_latitude,
        dropoff_longitude,
        dropoff_latitude,
        3959*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
        radians(180)/180/2),2)-cos(pickup_latitude*radians(180)/180)
        *cos(dropoff_latitude*radians(180)/180)*pow(sin((dropoff_longitude-pickup_longitude)*radians(180)/180/2),2)))
        /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*radians(180)/180/2),2)
        +cos(pickup_latitude*radians(180)/180)*cos(dropoff_latitude*radians(180)/180)*pow(sin((dropoff_longitude-pickup_longitude)*radians(180)/180/2),2))) as direct_distance,
        rand() as sample_key

        from trip
        where pickup_latitude between 30 and 90
            and pickup_longitude between -90 and -30
            and dropoff_latitude between 30 and 90
            and dropoff_longitude between -90 and -30
        )t
        join
        (
        select
        medallion,
        hack_license,
        vendor_id,
        pickup_datetime,
        payment_type,
        fare_amount,
        surcharge,
        mta_tax,
        tip_amount,
        tolls_amount,
        total_amount
        from fare
        )f
        on t.medallion=f.medallion and t.hack_license=f.hack_license and t.pickup_datetime=f.pickup_datetime
        where t.sample_key<=0.01
        """
        cursor.execute(queryString)

Bir süre sonra verileri Hadoop kümeleri yüklenen görebilirsiniz:

    queryString = """
        select * from nyctaxi_downsampled_dataset limit 10;
        """
    cursor.execute(queryString)
    pd.read_sql(queryString,connection)


![Veri tablosu](./media/vm-do-ten-things/DownSample_Data_For_Modeling_v2.PNG)

**Machine Learning kullanarak HDI veri okuma: okuyucu Modülü**

De kullanabilirsiniz **okuyucu** Hadoop kümesi veritabanına erişmek için Machine Learning Studio'da modülü. HDI kümeleri ve yapı lık makine HDI kümelerde veritabanını kullanarak modelleri öğrenme etkinleştirmek için Azure depolama hesabı kimlik bilgilerini takın.

![Okuyucu modülü özellikleri](./media/vm-do-ten-things/AML_Reader_Hive.PNG)

Puanlanmış veri kümesi ardından görüntülenebilir:

![Puanlanmış veri kümesini görüntülemek](./media/vm-do-ten-things/AML_Model_Results.PNG)

### <a name="azure-sql-data-warehouse--databases"></a>Azure SQL Data Warehouse & veritabanları
Azure SQL Data Warehouse kurumsal sınıf SQL Server deneyimi hizmetiyle bir esnek veri ambarı gibidir.

Bu konuda sağlanan yönergeleri izleyerek, Azure SQL Data Warehouse sağlayabilirsiniz [makale](../../sql-data-warehouse/sql-data-warehouse-get-started-provision.md). Azure SQL veri ambarı sağlamak sonra bunu kullanabilirsiniz [izlenecek](../team-data-science-process/sqldw-walkthrough.md) verileri karşıya yükleme, keşfi ve modelleme kullanarak verileri SQL Data Warehouse içinde yapmak için.

#### <a name="azure-cosmos-db"></a>Azure Cosmos DB
Azure Cosmos DB, bulutta bir NoSQL veritabanıdır. JSON gibi belgelerle çalışmanıza olanak sağlar ve depolamak ve belgeleri sorgu olanak sağlar.

Azure Cosmos DB DSVM erişim için aşağıdaki koşullar başına adımları gerçekleştirmeniz gerekir.

1. Azure Cosmos DB Python SDK'sını yükleyin (çalıştırmak ```pip install pydocumentdb``` komut isteminden)
2. Bir Azure Cosmos DB hesap oluşturup bir veritabanından [Azure portalı](https://portal.azure.com)
3. "Azure Cosmos DB geçiş aracı" indirin [burada](http://www.microsoft.com/downloads/details.aspx?FamilyID=cda7703a-2774-4c07-adcc-ad02ddc1a44d) ve tercih ettiğiniz bir dizine ayıklayın
4. Alma depolanmış JSON verilerini (volcano) bir [ortak blob](https://cahandson.blob.core.windows.net/samples/volcano.json) Cosmos Geçiş Aracı (dtui.exe Cosmos DB geçiş aracı yüklendiği dizininden) için şu komutu parametreler ile DB içine. Bu parametreler ile kaynak ve hedef konumu girin:
   
    /s:JsonFile /s.Files:https://cahandson.blob.core.windows.net/samples/volcano.json /t:DocumentDBBulk /t.ConnectionString:AccountEndpoint=https://[DocDBAccountName].documents.azure.com:443/; AccountKey = [[anahtar]; veritabanı volcano /t.Collection:volcano1 =

Bir kez veri içe aktardıktan sonra Jupyter için gidip başlıklı not defteri açın *DocumentDBSample* Azure Cosmos DB erişmek ve bazı temel sorgulama yapmak için python kodunu içerir. Cosmos DB hakkında daha fazla hizmet adresini ziyaret ederek bilgi [belge sayfasının](https://docs.microsoft.com/azure/cosmos-db/).

## <a name="8-build-reports-and-dashboard-using-the-power-bi-desktop"></a>8. Raporları ve panoyu Power BI Desktop kullanarak derleme
Bize Power bı'da önceki Cosmos DB örnekte görsel veri almak için gördüğümüz Volcano JSON dosyası görselleştirin. Ayrıntılı adımlar kullanılabilir [Power BI makale](../../cosmos-db/powerbi-visualize.md). Üst düzey adımlar şunlardır:

1. Power BI Desktop açın ve "Veri Al" yapın. URL olarak belirtin: https://cahandson.blob.core.windows.net/samples/volcano.json
2. Bir liste olarak alınan JSON kayıtlar görmeniz gerekir
3. Power BI ile aynı çalışabilmeniz için listeyi bir tabloya Dönüştür
4. (Bir sütunun sağdaki "sol ve sağ ok" simgesiyle) genişletme simgesine tıklayarak sütunları Genişlet
5. Konum "Kayıt" alanını olduğuna dikkat edin. Kaydı'nı genişletin ve yalnızca koordinatları seçin. Liste sütununu koordinat değil
6. İki öğe formülü kullanarak koordinat listesi alanı birleştirme virgülle ayrı LatLong sütun listesi koordinat sütun dönüştürmek için yeni bir sütun ekleyin ```Text.From([coordinates]{1})&","&Text.From([coordinates]{0})```.
7. Son olarak dönüştürmek ```Elevation``` ondalık ve sütun **Kapat** ve **Uygula**.

Adımları önceki yerine komut dosyaları Gelişmiş bir sorgu dili veri dönüşümleri yazmaya izin veren bir Power BI düzenleyicide kullanılan adımları out aşağıdaki kodu yapıştırabilirsiniz.

    let
        Source = Json.Document(Web.Contents("https://cahandson.blob.core.windows.net/samples/volcano.json")),
        #"Converted to Table" = Table.FromList(Source, Splitter.SplitByNothing(), null, null, ExtraValues.Error),
        #"Expanded Column1" = Table.ExpandRecordColumn(#"Converted to Table", "Column1", {"Volcano Name", "Country", "Region", "Location", "Elevation", "Type", "Status", "Last Known Eruption", "id"}, {"Volcano Name", "Country", "Region", "Location", "Elevation", "Type", "Status", "Last Known Eruption", "id"}),
        #"Expanded Location" = Table.ExpandRecordColumn(#"Expanded Column1", "Location", {"coordinates"}, {"coordinates"}),
        #"Added Custom" = Table.AddColumn(#"Expanded Location", "LatLong", each Text.From([coordinates]{1})&","&Text.From([coordinates]{0})),
        #"Changed Type" = Table.TransformColumnTypes(#"Added Custom",{{"Elevation", type number}})
    in
        #"Changed Type"



Artık Power BI veri modelinizi verilere sahip. Power BI desktop aşağıdaki gibi görünmelidir:

![Power BI desktop](./media/vm-do-ten-things/PowerBIVolcanoData.png)

Raporlar ve veri modelini kullanarak görselleştirmeleri oluşturmaya başlayabilirsiniz. Bu adımları izleyebilirsiniz [Power BI makale](../../cosmos-db/powerbi-visualize.md#build-the-reports) bir rapor oluşturmak için. Sonuç aşağıdakine benzer bir rapordur.

![Power BI Desktop rapor görünümü - Power BI Bağlayıcısı](./media/vm-do-ten-things/power_bi_connector_pbireportview2.png)

## <a name="9-dynamically-scale-your-dsvm-to-meet-your-project-needs"></a>9. Proje gereksinimlerinizi karşılamak için DSVM dinamik ölçeklendirme
Proje gereksinimlerinizi karşılayacak şekilde DSVM yukarı ve aşağı ölçeklendirebilirsiniz. VM Akşam veya hafta sonları kullanın gerekmiyorsa, yalnızca VM'yi kapatabilirsiniz [Azure portal](https://portal.azure.com).

> [!NOTE]
> VM yalnızca işletim sistemi kapatma düğmesini kullanırsanız, bilgi işlem ücretleri.  
> 
> 

Bazı büyük ölçekli analiz işlemek ve VM boyutları CPU çekirdekleri, bellek kapasitesi ve (katı hal sürücüleri dahil), bilgi işlem ve bütçe gereksinimlerinize uyan disk türleri açısından büyük seçimine bulabilirsiniz daha fazla CPU ve/veya bellek ve/veya disk kapasitesine ihtiyacınız gerekiyorsa. Veritabanlarının saatlik işlem fiyatlandırma birlikte tam listesini VM'ler edinilebilir [Azure Virtual Machines fiyatlandırması](https://azure.microsoft.com/pricing/details/virtual-machines/) sayfası.

Benzer şekilde, VM işleme kapasitesi, gereksinimini azaltır varsa (örneğin: bir Hadoop veya bir Spark kümesi için bir ana iş yükü taşınmış), kümeden aşağı ölçeklenebilen [Azure portal](https://portal.azure.com) ve VM örneği ayarlarına giderek. Bir ekran görüntüsü aşağıda verilmiştir.

![VM örneği ayarları](./media/vm-do-ten-things/VMScaling.PNG)

## <a name="10-install-additional-tools-on-your-virtual-machine"></a>10. Ek araçlar sanal makinenize yükleyin
Biz yalnızca kaynaklar için ödeme tarafından kullandığınız adresine birçok ortak veri analizi gereksinimlerini ve önleme tarafından size zaman kazandırır zorunda yükleyin ve tek tek ortamınızı yapılandırın ve, tasarruf sorgulayabilmesi inanıyoruz çeşitli araçlar paket.

Analytics ortamınızı artırmak için bu makaledeki profili diğer Azure veri ve Analiz Hizmetleri yararlanabilirsiniz. Bazı durumlarda, gereksinimlerinize özel bazı üçüncü taraf araçları dahil olmak üzere ek araçlar gerektirebilir anlayın. Gereksinim duyduğunuz yeni Araçları'nı yüklemek için sanal makine üzerinde tam yönetim erişimi var. Python ve önceden yüklü R ayrıca ek paketleri yükleyebilirsiniz. Python için ya da kullanabilirsiniz ```conda``` veya ```pip```. R için kullandığınız ```install.packages()``` R konsol veya IDE kullanın ve seçin "**paketleri** -> **yükleme paketleri...** ".

## <a name="summary"></a>Özet
Microsoft Veri bilimi sanal makinede yapabileceği şeylerden bazıları şunlardır. Etkili analytics ortamı olmak için yapabileceğiniz birçok şey daha vardır.

