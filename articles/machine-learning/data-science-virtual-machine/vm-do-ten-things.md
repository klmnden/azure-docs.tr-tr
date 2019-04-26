---
title: Veri keşfi ve modelleme ile veri bilimi sanal makinesi
titleSuffix: Azure
description: Çeşitli veri keşfi ve modelleme görev veri bilimi sanal makinesi üzerinde gerçekleştirin.
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
editor: cgronlun
ms.custom: seodec18
ms.assetid: 145dfe3e-2bd2-478f-9b6e-99d97d789c62
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: gokuma
ms.openlocfilehash: f30c241feced3031d9ed9791c27c6bb1e1e99efb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60366269"
---
# <a name="ten-things-you-can-do-on-the-windows-data-science-virtual-machine"></a>Windows veri bilimi sanal makinesi üzerinde yapabileceğiniz on işlem

Windows veri bilimi sanal makinesi (DSVM), çeşitli veri keşfi ve modelleme görevleri gerçekleştirmenizi sağlar güçlü veri bilimi geliştirme ortamıdır. Ortam zaten oluşturulmuş ve analiz için şirket içi, hızlı bir şekilde kullanmaya başlamak kolaylaştıran birkaç popüler veri analizi araçları ile birlikte paket gelen Bulut veya karma dağıtımlar. DSVM birçok Azure Hizmetleri ile yakın bir şekilde çalışır ve azure'da, Azure SQL veri ambarı, Azure Data Lake, Azure depolama veya Azure Cosmos DB'de depolanan verileri okumak ve edebilir. Ayrıca, diğer Azure Machine Learning ve Azure Data Factory gibi analiz araçları da yararlanabilirsiniz.

Bu makalede, çeşitli veri bilimi görevlerini gerçekleştirmek ve diğer Azure Hizmetleri ile etkileşim kurmak için DSVM'ye kullanmayı öğreneceksiniz. DSVM'nin yapabileceklerinizden bazıları şunlardır:

1. Verileri araştırmak ve yerel olarak Microsoft ML Server, Python kullanarak DSVM modellerde geliştirin
2. Jupyter Not Defteri, verilerinizde Python 2, Python 3, Microsoft R R performans için tasarlanan bir kurumsal hazır sürümünü kullanarak bir tarayıcı ile deneme gerçekleştirin
3. İstemci uygulamaları basit bir web hizmeti arabirimi modellerinize erişebilmesi için Azure Machine Learning'de R ve Python kullanarak oluşturulmuş modelleri dağıtabilir
4. Azure portalı veya Powershell kullanarak Azure kaynaklarınızı yönetme
5. Depolama alanınızı genişletmek ve büyük ölçekli veri kümeleri paylaşmak / takımınızda DSVM'ye bağlanabilir bir sürücüde olarak bir Azure dosya depolama oluşturarak kodu
6. Kod GitHub kullanan ekibinizle paylaşın ve önceden yüklenmiş Git istemcilerini - Git Bash, Git GUI kullanarak deponuza erişebilirsiniz.
7. Çeşitli Azure veri ve Analiz Hizmetleri gibi Azure blob depolama, Azure Data Lake, Azure HDInsight (Hadoop), Azure Cosmos DB, Azure SQL veri ambarı & veritabanlarına erişme
8. Raporlar ve Pano DSVM'nin önceden yüklenen Power BI Desktop kullanarak oluşturun ve bulutta dağıtın
9. DSVM proje gereksinimlerinizi karşılayacak şekilde dinamik olarak ölçeklendirin
10. Sanal makinenize ek araçları yükleyin   

> [!NOTE]
> Bu makalede listelenen ek veri depolama ve Analiz Hizmetleri birçoğu için ek kullanım ücretleri uygulanır. Başvurmak [Azure fiyatlandırma](https://azure.microsoft.com/pricing/) Ayrıntıları sayfası.
> 
> 

**Önkoşullar**

* Bir Azure aboneliği gerekir. Ücretsiz deneme için kaydolabilirsiniz [burada](https://azure.microsoft.com/free/).
* Azure portalında bir veri bilimi sanal makinesi sağlama yönergeleri [bir sanal makine oluştururken](https://portal.azure.com/#create/microsoft-dsvm.dsvm-windowsserver-2016).


[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="1-explore-data-and-develop-models-using-microsoft-ml-server-or-python"></a>1. Verileri araştırmak ve Microsoft ML Server veya Python kullanarak modeller geliştirin
R ve Python gibi dillerle DSVM üzerinde veri analiz yapmak için kullanabilirsiniz.

R için bir IDE için Visual Studio Başlangıç menüsünde veya masaüstü veya R araçları bulunabilir RStudio gibi kullanabilirsiniz. Microsoft Open-Kaynak/CRAN-ölçeklenebilir analiz ve paralel öbekli analizi yaparak izin verilen bellek boyutu daha büyük veri analiz etmenizi sağlamak için R üzerine ek kitaplıklar sağlamıştır. 

Python için bir IDE olan Visual Studio (PTVS) uzantısı önceden yüklenmiş için Python araçları Visual Studio Community Edition gibi kullanabilirsiniz. Varsayılan olarak, yalnızca Python 3.6, kök conda ortam PTVS üzerinde yapılandırılır. Anaconda Python 2.7 etkinleştirmek için aşağıdakileri yapmanız gerekir:

* Her sürüm için özel ortamlarda giderek oluşturma **Araçları** -> **Python Araçları** -> **Python ortamları** tıklayıp " **+ Özel**"in Visual Studio Community Edition
* Bir açıklama girin ve ortam ön ek yolu olarak ayarlamak *c:\anaconda\envs\python2* Anaconda Python 2.7 için
* Tıklayın **otomatik algıla** ve ardından **Uygula** ortam kaydedin.

Özel ortam Kurulumu Visual Studio'da nasıl göründüğünü aşağıda verilmiştir.

![Ekran görüntüsü, Visual Studio seçili Visual Studio için Python araçları ile](./media/vm-do-ten-things/PTVSSetup.png)

Bkz: [PTVS dokümantasyonu](https://aka.ms/ptvsdocs) Python ortamları oluşturma hakkında daha fazla ayrıntı için.

Artık, yeni Python projesi oluşturmak için ayarlanır. Gidin **dosya** -> **yeni** -> **proje** -> **Python** ve türünü seçin Python uygulaması oluşturuyorsunuz. Sağ tıklayarak (Python 2.7 ya da 3.6) istenen sürüm için geçerli proje için Python ortamı ayarlayabilirsiniz **Python ortamları**u seçerek **Ekle/Kaldır Python ortamları**ve ardından olduğu istenen ortama çekme. Üründe PTVS ile çalışma hakkında daha fazla bilgi bulabilirsiniz [belgeleri](https://aka.ms/ptvsdocs).

## <a name="2-using-a-jupyter-notebook-to-explore-and-model-your-data-with-python-or-r"></a>2. Jupyter Not Defteri kullanarak keşfedin ve Python veya R ile verilerinizi modelleyin
Jupyter not defteri veri keşfi ve modelleme için bir tarayıcı tabanlı "IDE" sağlayan güçlü bir ortamdır. Python 2, 3 Python veya R (açık kaynak ve Microsoft R Server) bir Jupyter not defteri kullanabilirsiniz.

Jupyter not defteri başlatmak için Başlat menüsünde simgesine tıklayın. / Masaüstü simgesi başlıklı **Jupyter not defteri**. DSVM komut satırına komutu da çalıştırabilirsiniz ```jupyter notebook``` dizininden var olan dizüstü bilgisayarlar veya nereye yeni not defteri oluşturmak istiyorsunuz.  

Jupyter başlattıktan sonra DSVM önceden paketlenmiş birkaç örnek not defterleri içeren bir dizine görmeniz gerekir. Artık şunları yapabilirsiniz:

* Not kodu görmek için tıklayın.
* Her hücre tuşlarına basarak yürütme **SHIFT girin**.
* Tıklayarak tüm not defterlerini çalıştırmak **hücre** -> **çalıştırın**
* Jupyter simgesini (sol üst köşesinde) ve ardından yeni bir not defteri oluşturma **yeni** düğmesine sağ ve not defteri dili (çekirdekler olarak da bilinir) seçerek.   

> [!NOTE]
> Şu anda Python 2.7 ve Python 3.6, R, Julia ve PySpark çekirdekleri Jupyter içinde desteklenir. R çekirdek programlama, hem açık kaynak R yanı sıra Microsoft R. yüksek performanslı destekler.   
> 
> 

Keşfedebilirsiniz not defterinde olduktan sonra veri modeli oluşturma, seçtiğiniz kitaplıkları kullanarak modeli test etmek.

## <a name="3-build-models-using-r-or-python-and-operationalize-them-using-azure-machine-learning"></a>3. R veya Python ve Operationalize Azure Machine Learning kullanarak bunları kullanarak modelleri oluşturabilir
Yerleşik ve, model doğrulandığında sonra sonraki adım genellikle üretim ortamına dağıtmak için gelir. Bu, istemci bir gerçek zamanlı veya toplu iş modu olarak model tahminlerinin çağrılacak uygulamalar sağlar. Azure Machine Learning, R veya Python ile derlenen bir modeli kullanıma hazır hale getirmek için bir mekanizma sağlar.

Azure Machine learning'de modelinizi kullanıma hazır hale getirme, bir web hizmeti, giriş parametreleri geçirin ve Öngörüler, çıktı modelden alma REST çağrılarını istemcilerin kullanıma sunulur.   

> [!NOTE]
> Henüz Azure Machine Learning için kaydolmadıysanız, ücretsiz bir çalışma alanı ya da bir standart çalışma ziyaret ederek alabilirsiniz [Azure Machine Learning Studio](https://studio.azureml.net/) giriş sayfası ve tıklamak çubuğunda "kullanmaya başlayın."   
> 
> 

### <a name="build-and-operationalize-python-models"></a>Derleme ve kullanıma hazır hale getirme Python modelleri
Kod SciKit-öğrenme kitaplığını kullanarak basit bir modeli oluşturan bir Python Jupyter Notebook geliştirilen bir parçacığı aşağıda verilmiştir.

```python
#IRIS classification
from sklearn import datasets
from sklearn import svm
clf = svm.SVC()
iris = datasets.load_iris()
X, y = iris.data, iris.target
clf.fit(X, y)
```

Yöntem Modellerinizi python için Azure Machine Learning sarar modelinin tahmin bir işlev uygulamasına dağıtmak için kullanılan ve, Azure Machine Learning belirtmek önceden yüklenmiş Azure Machine Learning python kitaplığı tarafından sağlanan özniteliklerle düzenler Çalışma alanı kimliği, API anahtarı ve giriş ve dönüş parametreleri.  

```python
from azureml import services
@services.publish(workspaceid, auth_token)
@services.types(sep_l = float, sep_w = float, pet_l=float, pet_w=float)
@services.returns(int) #0, or 1, or 2
def predictIris(sep_l, sep_w, pet_l, pet_w):
    inputArray = [sep_l, sep_w, pet_l, pet_w]
return clf.predict(inputArray)
```

Bir istemci, artık web hizmetine çağrı yapabilir. REST API istekleri oluşturmak kullanışlı sarmalayıcıları vardır. Web hizmeti kullanmak için örnek kod aşağıda verilmiştir.

```python
# Consume through web service URL and keys
from azureml import services
@services.service(url, api_key)
@services.types(sep_l = float, sep_w = float, pet_l=float, pet_w=float)
@services.returns(float)
def IrisPredictor(sep_l, sep_w, pet_l, pet_w):
pass

IrisPredictor(3,2,3,4)
```

> [!NOTE]
> Azure Machine Learning kitaplığı yalnızca şu anda Python 2.7 üzerinde desteklenir.   
> 
> 

### <a name="build-and-operationalize-r-models"></a>Derleme ve kullanıma hazır hale getirme R modelleri
Veri bilimi sanal makinesi üzerinde veya başka bir Azure Machine Learning Python için nasıl yapıldığına için benzer bir şekilde R modellerinin dağıtabilirsiniz. Adımlar aşağıdaki gibidir:

* Çalışma alanı kimliği ve kimlik doğrulama sağlamak için settings.json dosya oluşturma belirteci 
* işlevi modelin tahmin etmek için bir sarmalayıcı yazın.
* çağrı ```publishWebService``` işlevi sarmalayıcı içinde geçirmek için Azure Machine Learning Kitaplığı'nda.  

Aşağıda, ayarlama, oluşturun, yayımlayın ve Azure Machine Learning web hizmeti olarak modeli kullanma için kullanılan yordam ve kod parçacıkları verilmiştir.

#### <a name="setup"></a>Kurulum

* Adlı bir dizin altındaki bir settings.json dosyasına oluşturma ```.azureml``` giriş dizininizin altında ve Azure Machine Learning çalışma alanınızdan parametreleri girin:

Settings.JSON dosya yapısı:

```json
{"workspace":{
"id"                  : "ENTER YOUR AZUREML WORKSPACE ID",
"authorization_token" : "ENTER YOUR AZUREML AUTH TOKEN"
}}
```

#### <a name="build-a-model-in-r-and-publish-it-in-azure-machine-learning"></a>R ile model oluşturma ve Azure Machine Learning'de yayımlayın

```r
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
```

#### <a name="consume-the-model-deployed-in-azure-machine-learning"></a>Azure Machine Learning'de dağıtılan modeli kullanma
Bir istemci uygulamasından model kullanmak için Azure Machine Learning kitaplığı adını kullanarak yayımlanan web hizmeti aramak için kullanırız `services` uç nokta belirlemek için API çağrısı. Çağrı `consume` işlev ve tahmin için veri çerçevesi geçirin.
Aşağıdaki kod, yayımlanan bir Azure Machine Learning web hizmeti olarak modeli kullanma için kullanılır.

```r
library(AzureML)
library(lme4)
ws <- workspace(config="~/.azureml/settings.json")

s <-  services(ws, name = "sleepy lm")
s <- tail(s, 1) # use the last published function, in case of duplicate function names

ep <- endpoints(ws, s)

# OK, try this out, and compare with raw data
ans = consume(ep, sleepstudy)$ans
```

Azure Machine Learning R Kitaplığı hakkında daha fazla bilgi bulunabilir [burada](https://cran.r-project.org/web/packages/AzureML/AzureML.pdf).

## <a name="4-administer-your-azure-resources-using-azure-portal-or-powershell"></a>4. Azure portalı veya Powershell kullanarak Azure kaynaklarınızı yönetme
DSVM yalnızca analiz çözümünüzü sanal makinede yerel olarak oluşturmanıza olanak tanır, ancak Ayrıca, Microsoft'un Azure bulut hizmetlerinde erişmenize olanak sağlar. Azure, birkaç işlem, depolama, veri Analiz Hizmetleri ve diğer hizmetler yönetmek ve DSVM'ye erişim sağlar.

Tarayıcınızı kullanırsınız ve fareyle Azure abonelik ve bulut kaynaklarınızı yönetmek için [Azure portalında](https://portal.azure.com). Azure Powershell, Azure aboneliğinize ve kaynaklarınıza bir komut dosyası aracılığıyla yönetmek için de kullanabilirsiniz.
Azure Powershell masaüstündeki kısayoldan veya Başlat menüsünde "Microsoft Azure Powershell." başlıklı çalıştırabilirsiniz Başvurmak [Microsoft Azure Powershell belgeleri](../../powershell-azure-resource-manager.md) Azure aboneliğinizi ve Windows Powershell betiklerini kullanarak kaynaklara nasıl yönetebileceğiniz hakkında daha fazla bilgi.

## <a name="5-extend-your-storage-space-with-a-shared-file-system"></a>5. Paylaşılan bir dosya sistemi ile depolama alanınızı genişletin
Veri bilimcileri, büyük veri kümeleri, kod veya diğer kaynaklar takım içinde paylaşabilirsiniz. DSVM yaklaşık 45 GB kullanılabilir alan vardır. Depolama alanınızı genişletmek için Azure dosya Hizmeti'ne kullanabilirsiniz ve ya da bir veya daha fazla DSVM örneklerinde bağlayın veya bir REST API aracılığıyla erişebilirsiniz.  Ayrıca [Azure portalı](../../virtual-machines/windows/attach-managed-disk-portal.md) veya [Azure Powershell](../../virtual-machines/windows/attach-disk-ps.md) fazladan ayrılmış veri diskleri eklemek için. 

> [!NOTE]
> Azure dosya Hizmeti'ne paylaşımı maksimum alan 5 TB'dir ve tek tek dosya boyutu sınırını 1 TB'tır. 
> 
> 

Azure Powershell, Azure dosya Hizmeti'ne paylaşımı oluşturmak için kullanabilirsiniz. Azure dosya hizmeti paylaşımı oluşturmak için Azure PowerShell altında çalıştırılacak betik aşağıda verilmiştir.

```powershell
# Authenticate to Azure.
Connect-AzAccount
# Select your subscription
Get-AzSubscription –SubscriptionName "<your subscription name>" | Select-AzSubscription
# Create a new resource group.
New-AzResourceGroup -Name <dsvmdatarg>
# Create a new storage account. You can reuse existing storage account if you wish.
New-AzStorageAccount -Name <mydatadisk> -ResourceGroupName <dsvmdatarg> -Location "<Azure Data Center Name For eg. South Central US>" -Type "Standard_LRS"
# Set your current working storage account
Set-AzCurrentStorageAccount –ResourceGroupName "<dsvmdatarg>" –StorageAccountName <mydatadisk>

# Create an Azure File Service Share
$s = New-AzStorageShare <<teamsharename>>
# Create a directory under the FIle share. You can give it any name
New-AzStorageDirectory -Share $s -Path <directory name>
# List the share to confirm that everything worked
Get-AzStorageFile -Share $s
```

Azure dosya paylaşımını oluşturduğunuza göre azure'da herhangi bir sanal makineye takabilirsiniz. Sanal Makinenizin gecikme süresi ve veri aktarım ücretleri önlemek için depolama hesabı aynı Azure veri merkezinde olduğunu önemle tavsiye edilir. Azure Powershell üzerinde çalıştırabileceğiniz DSVM üzerinde sürücüyü bağlamak için komutları aşağıda verilmiştir.

```powershell
# Get storage key of the storage account that has the Azure file share from Azure portal. Store it securely on the VM to avoid prompted in next command.
cmdkey /add:<<mydatadisk>>.file.core.windows.net /user:<<mydatadisk>> /pass:<storage key>

# Mount the Azure file share as Z: drive on the VM. You can chose another drive letter if you wish
net use z:  \\<mydatadisk>.file.core.windows.net\<<teamsharename>>
```

Artık VM üzerinde herhangi bir normal sürücünün olduğu gibi bu sürücü erişebilirsiniz.

## <a name="6-share-code-with-your-team-using-github"></a>6. Kod GitHub kullanan ekibinizle paylaşın
GitHub Geliştirici topluluğu tarafından paylaşılan çeşitli teknolojiler kullanarak farklı araçları için birçok örnek kod ve kaynaklar nerede bulabileceğiniz bir kod deposu bulunur. Git, izlemek ve sürümlerini kod dosyaları depolamak için bir teknoloji olarak kullanır. GitHub Ayrıca kendi deponuzu takımınızın paylaşılan kod ve belgeleri depolamak, sürüm denetimi uygulamak ve ayrıca denetlemek için görüntüleyin ve kodu katkıda bulunan erişimine sahip oluşturabileceğiniz platformudur. Ziyaret [GitHub yardım sayfalarına](https://help.github.com/) Git kullanma hakkında daha fazla bilgi için. GitHub, takımınızla işbirliği yapmanıza, topluluk tarafından geliştirilen kodu kullanın ve kod topluluğa katkıda yollarından biri olarak kullanabilirsiniz.

DSVM zaten iyi GUI GitHub deposuna erişmek için komut satırı hem de istemci araçlarıyla yüklü olarak sunulur. Git ve GitHub ile çalışmak için komut satırı aracı, Git Bash çağrılır. DSVM'nin yüklü visual Studio, Git uzantılarına sahiptir. Başlat menüsünde ve masaüstünde bu araçları için başlangıç simgeler bulabilirsiniz.

Kodu bir GitHub deposundan karşıdan yüklemek için kullandığınız ```git clone``` komutu. Bulunduğunuz sonra Örneğin, veri bilimi depo Microsoft tarafından yayımlanan geçerli dizine indirmek için aşağıdaki komutu çalıştırabilirsiniz ```git-bash```.

    git clone https://github.com/Azure/DataScienceVM.git

Visual Studio'da aynı kopyalama işlemi yapabilirsiniz. Aşağıdaki ekran görüntüsünde, Visual Studio, Git ve GitHub araçlara erişmek gösterilmektedir.

![Ekran görüntüsü, Visual Studio ile görüntülenen GitHub bağlantısı](./media/vm-do-ten-things/VSGit.PNG)

GitHub deponuza kullanılabilen çeşitli kaynaklardan github.com üzerinde çalışmak için Git kullanma hakkında daha fazla bilgi bulabilirsiniz. [Kağıdı](https://services.github.com/on-demand/downloads/github-git-cheat-sheet.pdf) yararlı bir başvurudur.

## <a name="7-access-various-azure-data-and-analytics-services"></a>7. Çeşitli Azure veri ve Analiz Hizmetleri erişim
### <a name="azure-blob"></a>Azure Blob
Azure blob, büyük ve küçük veriler için güvenilir, ekonomik bulut depolamadır. Bu bölümde, Azure Blob ve bir Azure Blob üzerinde depolanan verilere erişmek için nasıl veri taşıyabileceğinizi açıklanmaktadır.

**Önkoşul**

* **Azure Blob Depolama hesabınızı oluşturmak [Azure portalında](https://portal.azure.com).**

![Azure portalında depolama hesabı oluşturma işleminin ekran görüntüsü](./media/vm-do-ten-things/Create_Azure_Blob.PNG)

* Önceden yüklenmiş komut satırı AzCopy aracı konumunda bulunur onaylayın ```C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy.exe```. Bu aracı çalıştırılırken, tam komut yolu yazarak önlemek için yol ortam değişkenine azcopy.exe içeren dizini zaten var. AzCopy aracı hakkında daha fazla bilgi için bkz [AzCopy belgeleri](../../storage/common/storage-use-azcopy.md)
* Azure Depolama Gezgini aracını başlatın. Dan indirilebilir [Microsoft Azure Depolama Gezgini](https://storageexplorer.com/). 

![Azure depolama Gezgini'nin depolama hesabına erişilirken ekran görüntüsü](./media/vm-do-ten-things/AzureStorageExplorer_v4.png)

**Azure blob'a veri taşıma VM'den: AzCopy**

Yerel dosyalarınızı ve blob depolama arasında veri taşımak için AzCopy komut satırı içinde kullanabilirsiniz veya PowerShell:

    AzCopy /Source:C:\myfolder /Dest:https://<mystorageaccount>.blob.core.windows.net/<mycontainer> /DestKey:<storage account key> /Pattern:abc.txt

Değiştirin **C:\myfolder** dosyanızı depolandığı yolu **mystorageaccount** blob depolama hesabı adı için **mycontainer** kapsayıcı adı için **depolama hesabı anahtarı** blob depolama erişim anahtarınızı için. Depolama hesabı kimlik bilgilerinizi bulabilirsiniz [Azure portalında](https://portal.azure.com).

![Depolama hesabı anahtarları ve Azure portalında kapsayıcı bilgileri ekran görüntüsü](./media/vm-do-ten-things/StorageAccountCredential_v2.png)

AzCopy komutu PowerShell'de veya komut isteminden çalıştırın. Bazı örnek kullanım AzCopy komutu aşağıdadır:

```powershell
# Copy *.sql from local machine to an Azure Blob
"C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:"c:\Aaqs\Data Science Scripts" /Dest:https://[ENTER STORAGE ACCOUNT].blob.core.windows.net/[ENTER CONTAINER] /DestKey:[ENTER STORAGE KEY] /S /Pattern:*.sql

# Copy back all files from Azure Blob container to Local machine

"C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Dest:"c:\Aaqs\Data Science Scripts\temp" /Source:https://[ENTER STORAGE ACCOUNT].blob.core.windows.net/[ENTER CONTAINER] /SourceKey:[ENTER STORAGE KEY] /S
```

Bir Azure blobuna kopyalamak için AzCopy komutunu çalıştırdıktan sonra dosyanızı kısa bir süre içinde Azure depolama Gezgini'nde gösterilir bakın.

![CSV dosyası karşıya yüklendi görüntüleme depolama hesabı ekran görüntüsü](./media/vm-do-ten-things/AzCopy_run_finshed_Storage_Explorer_v3.png)

**Azure blob'a veri taşıma VM'den: Azure Depolama Gezgini**

Ayrıca Azure Depolama Gezgini'ni kullanarak, sanal yerel dosyadan verileri karşıya yükleyebilirsiniz:

* Verileri bir kapsayıcıya yüklemek için hedef kapsayıcıyı seçin ve **karşıya** düğmesi.![ Azure depolama Gezgini'nde karşıya yükleme düğmesinin Ekran görüntüsü](./media/vm-do-ten-things/storage-accounts.png)
* Tıklayarak **...**  sağındaki **dosyaları** kutusunda, dosya sisteminden karşıya yükleyin ve bir veya birden çok dosya seçin **karşıya** dosyalar karşıya yüklenirken başlamaya.![ Karşıya yükleme dosyaları iletişim kutusunun ekran görüntüsü](./media/vm-do-ten-things/upload-files-to-blob.png)

**Azure Blob veri okuma: Machine Learning okuyucu Modülü**

Azure Machine Learning Studio'da kullanabileceğiniz bir **verileri içeri aktarma modülü** , blobundan verileri okumak için.

![Machine Learning Studio'da içeri aktarma verileri modülünün ekran görüntüsü](./media/vm-do-ten-things/AML_ReaderBlob_Module_v3.png)

**Azure Blob veri okuma: Python ODBC**

Kullanabileceğiniz **BlobService** doğrudan Jupyter Not Defteri veya Python programında blobdan veri okumak için kitaplığı.

İlk olarak, gerekli paketleri içeri aktarın:

```python
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
```

Ardından Azure Blob hesabı kimlik bilgilerinizi takın ve BLOB'dan veri okuma:

```python
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
```

Verileri bir veri çerçevesi okunan:

![İlk 10 veri satırlarını ekran görüntüsü](./media/vm-do-ten-things/IPNB_data_readin.PNG)

### <a name="azure-data-lake"></a>Azure Data Lake
Azure Data Lake Store, büyük veri analizi iş yükleri ve uyumlu olan Hadoop dağıtılmış dosya sistemi (HDFS) için hiper ölçekli bir depodur. Hadoop, Spark ve Azure Data Lake Analytics ile çalışır. Bu bölümde, verileri Azure Data Lake Store taşıyın ve Azure Data Lake Analytics'i kullanarak Analiz çalıştırma nasıl öğreneceksiniz.

**Önkoşul**

* Azure Data Lake Analytics'te oluşturma [Azure portalında](https://portal.azure.com).

![Data Lake Analytics, Azure portalından oluşturma işleminin ekran görüntüsü](./media/vm-do-ten-things/Azure_Data_Lake_Create_v2.png)

* **Azure Data Lake Araçları** içinde **Visual Studio** şu anda bulunamadı [bağlantı](https://www.microsoft.com/download/details.aspx?id=49504) sanal makinede Visual Studio Community sürümü zaten yüklü. Visual Studio başlangıç ve Azure aboneliğinizde oturum sonra Azure Data Analytics hesabınızı ve depolama Visual Studio'nun sol bölmesinde görmeniz gerekir.

![Visual Studio'da Data Lake Araçları'nın ekran görüntüsü](./media/vm-do-ten-things/Azure_Data_Lake_PlugIn_v2.PNG)

**Data Lake için veri taşıma VM'den: Azure Data Lake Gezgini**

Kullanabileceğiniz **Azure Data Lake Explorer** sanal makinenizde yerel dosyaları verileri Data Lake depolama alanına yüklemek için.

![Dosyaları karşıya yüklemek için Data Lake Explorer'ı kullanarak ekran görüntüsü](./media/vm-do-ten-things/Azure_Data_Lake_UploadData.PNG)

Ayrıca, veri taşıma ya da Azure Data Lake kullanarak hazır hale getirmek için veri işlem hattı oluşturabilirsiniz [Azure veri Factory(ADF)](https://azure.microsoft.com/services/data-factory/). Bu [makale](https://azure.microsoft.com/blog/creating-big-data-pipelines-using-azure-data-lake-and-azure-data-factory/) veri oluşturma adımlarında size kılavuzluk etmesi için işlem hatları.

**Verileri Azure Blobundan Data Lake okuyun: U-SQL**

Verilerinizi Azure Blob Depolama alanında bulunuyorsa, U-SQL sorgusunu Azure depolama blobunda gelen verileri doğrudan okuyabilir. U-SQL sorgusu oluşturma önce Azure Data Lake için blob depolama hesabınıza bağlı olduğundan emin olun. Git **Azure portalında**, Azure Data Lake Analytics panonuzu bulun, tıklayın **veri kaynağı Ekle**, depolama türü seçin **Azure depolama** ve Azure depolama hesabınızdaki takın Adı ve anahtarı. Ardından depolama hesabında depolanan verilere başvurabilirsiniz.

![Veri Kaynağı Ekle iletişim kutusunun ekran görüntüsü](./media/vm-do-ten-things/Link_Blob_to_ADLA_v2.PNG)

Visual Studio'da blob depolama alanından verileri okuma, bazı veri işleme yapmak, özellik Mühendisliği ve Azure Data Lake veya Azure Blob Depolama için sonuç verileri çıktı. Blob depolama alanındaki verilere başvuruda bulunduğunuzda kullanın **wasb: / /**; Azure Data Lake, kullanım verileri başvurduğunuzda **swbhdfs: / /**

![Vurgulanan WASB girdiyle sorgusunun ekran görüntüsü](./media/vm-do-ten-things/USQL_Read_Blob_v2.PNG)

Visual Studio'da aşağıdaki U-SQL sorguları kullanabilirsiniz:

```usql
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
```

Sorgunuzu sunucuya gönderildikten sonra işinizin durumunu gösteren diyagram görüntülenir.

![İş iletişim durumunun ekran görüntüsü](./media/vm-do-ten-things/USQL_Job_Status.PNG)

**Veri Gölü'nde sorgu veri: U-SQL**

Azure Data Lake alınan ve veri kümesi sonra kullanabileceğiniz [U-SQL dili](../../data-lake-analytics/data-lake-analytics-u-sql-get-started.md) sorgulayabilir ve verilerin keşfedin. U-SQL dili için T-SQL benzer, ancak kullanıcılar özelleştirilmiş modülleri, kullanıcı tanımlı işlevler ve vb. yazabilmesi amacıyla bazı C# özellikleri birleştirir. Önceki adımda komut dosyalarını kullanabilirsiniz.

Sonra sorgu tripdata_summary sunucusuna gönderilir. CSV, kısa bir süre içinde bulunabilir **Azure Data Lake Explorer**, sağ tıklama ile veri dosyası önizlenemedi.

![Data Lake Explorer csv dosyasında ekran görüntüsü](./media/vm-do-ten-things/USQL_create_summary.png)

Dosya bilgileri görmek için:

![Dosya Özeti bilgilerini ekran görüntüsü](./media/vm-do-ten-things/USQL_tripdata_summary.png)

### <a name="hdinsight-hadoop-clusters"></a>HDInsight Hadoop kümeleri
Azure HDInsight, bulutta yönetilen bir Apache Hadoop, Spark, HBase ve Storm hizmet özelliğidir. Azure HDInsight kümeleri veri bilimi sanal makineden kolayca çalışabilirsiniz.

**Önkoşul**

* Azure Blob Depolama hesabınızı oluşturmak [Azure portalında](https://portal.azure.com). Bu depolama hesabı, HDInsight kümeleri için verileri depolamak için kullanılır.

![HDInsight'ı Azure portalından oluşturma işleminin ekran görüntüsü](./media/vm-do-ten-things/Create_Azure_Blob.PNG)

* Azure HDInsight Hadoop kümeleri aşıp özelleştirme [Azure portalı](../team-data-science-process/customize-hadoop-cluster.md)
  
  * HDInsight kümenizle oluşturulduğu sırada oluşturduğunuz depolama hesabına bağlayın. Bu depolama hesabı, küme içinde işlenebilecek verilere erişmek için kullanılır.

![HDInsight kümesi ile oluşturduğunuz depolama hesabına bağlama](./media/vm-do-ten-things/Create_HDI_v4.PNG)

* Etkinleştirme **uzaktan erişim** oluşturulduktan sonra kümenin baş düğümüne. Burada belirttiğiniz uzaktan erişim kimlik bilgilerini Hatırla sonraki yordamda ihtiyaç duyacaksınız.

![HDInsight kümesine uzaktan erişimi etkinleştirin](./media/vm-do-ten-things/Create_HDI_dashboard_v3.PNG)

* Bir Azure Machine Learning çalışma alanı oluşturun. Makine öğrenimi denemeleri bu Machine Learning çalışma alanında depolanır. Vurgulanmış seçenekleri Portalı'nda, aşağıdaki ekran görüntüsünde gösterildiği gibi seçin:

![Bir Azure Machine Learning çalışma alanı oluşturma](./media/vm-do-ten-things/Create_ML_Space.PNG)

* Ardından çalışma alanınız için parametreler girin

![Machine Learning çalışma alanı parametreler girin](./media/vm-do-ten-things/Create_ML_Space_step2_v2.PNG)

* Ipython Not Defteri kullanarak verileri karşıya yükleyin. İlk gerekli paketleri içeri aktarın, kimlik bilgilerini takın, depolama hesabınızdaki bir db oluşturun ve ardından HDI kümelerine veri yükleme.

```python
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
```

* Alternatif olarak, bu izleyebilirsiniz [izlenecek](../team-data-science-process/hive-walkthrough.md) HDI kümesi NYC taksi verileri yüklemek için. Ana adımlar şunlardır:
  
  * AzCopy: indirme CSV'ın genel blobundan yerel klasörünüz sıkıştırıldı
  * AzCopy: sıkıştırması açılmış CSV'ye ait yerel klasörden HDI kümesi karşıya yükleme
  * Hadoop kümesinin baş düğümünü oturum ve keşif verileri analize hazırlama

HDI kümesi veriler yüklendikten sonra verilerinizi Azure depolama Gezgini'nde kontrol edebilirsiniz. Ve HDI kümesi içinde oluşturulan bir veritabanı nyctaxidb sahipsiniz.

**Veri keşfi: Python'da Hive sorguları**

Veriler Hadoop kümesi olduğundan, pyodbc paket araştırma yapın ve özellik Mühendisliği için Hive'ı kullanarak Hadoop kümeleri ve sorgu veritabanına bağlanmak için kullanabilirsiniz. Önkoşul adımda oluşturduğumuz var olan tabloları görüntüleyebilirsiniz.

```python
queryString = """
    show tables in nyctaxidb2;
    """
pd.read_sql(queryString,connection)
```

![Var olan tabloları görüntüleme](./media/vm-do-ten-things/Python_View_Existing_Tables_Hive_v3.PNG)

Kayıt sayısı her ay ve sıklığını bakalım Eğimli veya seyahat tablosu içinde değil:

```python
queryString = """
    select month, count(*) from nyctaxidb.trip group by month;
    """
results = pd.read_sql(queryString,connection)

%matplotlib inline

results.columns = ['month', 'trip_count']
df = results.copy()
df.index = df['month']
df['trip_count'].plot(kind='bar')
```

![Çizim kayıtları her ay sayısı](./media/vm-do-ten-things/Exploration_Number_Records_by_Month_v3.PNG)

```python
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
```

![İpucu frekansların çizimi](./media/vm-do-ten-things/Exploration_Frequency_tip_or_not_v3.PNG)

Alma konumu dropoff konum arasındaki mesafeyi Ayrıca işlem ve ardından seyahat uzaklık karşılaştırır.

```python
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
```

![Üst satırları alma ve dropoff tablo](./media/vm-do-ten-things/Exploration_compute_pickup_dropoff_distance_v2.PNG)

```python
results.columns = ['pickup_longitude', 'pickup_latitude', 'dropoff_longitude',
                    'dropoff_latitude', 'trip_distance', 'trip_time_in_secs', 'direct_distance']
df = results.loc[results['trip_distance']<=100] #remove outliers
df = df.loc[df['direct_distance']<=100] #remove outliers
plt.scatter(df['direct_distance'], df['trip_distance'])
```

![Çizim alma/dropoff mesafenin seyahat uzaklık](./media/vm-do-ten-things/Exploration_direct_distance_trip_distance_v2.PNG)

Hemen şimdi bir alt örneklenen (% 1) veri kümesini modelleme için hazırlayın. Machine Learning okuyucu modülü bu verileri kullanabilirsiniz.

```python
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
```

Artık birleştirme içeriğini önceki iç tablosuna Ekle

```python
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
```

Bir süre sonra verileri Hadoop kümelerini yüklendi görebilirsiniz:

```python
queryString = """
    select * from nyctaxi_downsampled_dataset limit 10;
    """
cursor.execute(queryString)
pd.read_sql(queryString,connection)
```

![Üst satırları tablodan veri](./media/vm-do-ten-things/DownSample_Data_For_Modeling_v2.PNG)

**Machine Learning kullanarak HDI veri okuma: okuyucu Modülü**

Ayrıca **okuyucu** Hadoop kümesi veritabanına erişmek için Machine Learning Studio'da modülü. HDI küme ve yapı makine öğrenme modellerini HDI kümelerinde veritabanını kullanarak etkinleştirmek için Azure depolama hesabı kimlik bilgilerini takın.

![Okuyucu modülü özellikleri](./media/vm-do-ten-things/AML_Reader_Hive.PNG)

Puanlanmış veri kümesi görüntülenebilir:

![Puanlanmış veri kümesini görüntülemek](./media/vm-do-ten-things/AML_Model_Results.PNG)

### <a name="azure-sql-data-warehouse--databases"></a>Azure SQL veri ambarı & veritabanları
Azure SQL veri ambarı, kurumsal düzeyde SQL Server deneyimi ile bir hizmet olarak bir elastik veri ambarı teklifidir.

Bu konuda verilen yönergeleri izleyerek, Azure SQL veri ambarı sağlayabilirsiniz [makale](../../sql-data-warehouse/sql-data-warehouse-get-started-provision.md). Azure SQL veri ambarı sağladıktan sonra bu kullanabilirsiniz [izlenecek](../team-data-science-process/sqldw-walkthrough.md) karşıya veri yükleme, keşfi ve modelleme kullanarak verileri SQL Data Warehouse'da yapmak için.

#### <a name="azure-cosmos-db"></a>Azure Cosmos DB
Azure Cosmos DB, bulutta bir NoSQL veritabanıdır. Bu JSON gibi belgelerle birlikte çalışmanıza olanak sağlar ve depolamak ve belgeleri sorgulamanızı sağlar.

Azure Cosmos DB DSVM erişmek için koşullar başına adımları şunlardır:

1. Azure Cosmos DB Python SDK'sı DSVM üzerinde zaten yüklü (çalıştırma ```pip install pydocumentdb --upgrade``` güncelleştirmek için komut isteminden)
2. Azure Cosmos DB hesabı oluşturup bir veritabanından [Azure portalı](https://portal.azure.com)
3. "Azure Cosmos DB geçiş aracı" indirmesine [burada](https://www.microsoft.com/downloads/details.aspx?FamilyID=cda7703a-2774-4c07-adcc-ad02ddc1a44d) ve tercih ettiğiniz bir dizine ayıklayın
4. Depolanmış JSON verilerini (volkan veriler) bir [ortak blob](https://cahandson.blob.core.windows.net/samples/volcano.json) Geçiş Aracı (Cosmos DB geçiş aracı yüklediğiniz dizininden dtui.exe) için aşağıdaki komutu parametreler ile Cosmos DB içinde. Bu parametreleri ile kaynak ve hedef konumu girin:
   
    `/s:JsonFile /s.Files:https://cahandson.blob.core.windows.net/samples/volcano.json /t:DocumentDBBulk /t.ConnectionString:AccountEndpoint=https://[DocDBAccountName].documents.azure.com:443/;AccountKey=[[KEY];Database=volcano /t.Collection:volcano1`

Verileri içeri aktardığınızda, Jupyter için gidip başlıklı not defterini açın *DocumentDBSample* Azure Cosmos DB'ye erişmek ve bazı temel sorgulama yapmak için python kodu içerir. Cosmos DB hakkında daha fazla hizmet ziyaret ederek edinebilirsiniz [belgeleri sayfasını](https://docs.microsoft.com/azure/cosmos-db/).

## <a name="8-build-reports-and-dashboard-using-the-power-bi-desktop"></a>8. Raporlar ve Pano Power BI Desktop kullanarak derleme
Volkan JSON dosyasını önceki Cosmos DB örnekte veri görsel Öngörüler edinmek için Power bı'da görselleştirebilirsiniz. Ayrıntılı adımlar kullanılabilir [Power BI makalesinde](../../cosmos-db/powerbi-visualize.md). Üst düzey adımlar şunlardır:

1. Power BI Desktop'ı açın ve "Verileri alın." URL olarak belirtin: https://cahandson.blob.core.windows.net/samples/volcano.json
2. Bir liste olarak alınan JSON kayıtlar görmeniz gerekir
3. Power BI ile aynı çalışabilmesi için listeyi bir tabloya Dönüştür.
4. Sütunları genişlet simgesini (bir sütunun sağında "sol ve sağ ok" simgesi) tıklayarak genişletin.
5. Konum "Kayıt" alanını olduğuna dikkat edin. Kaydı'nı genişletin ve yalnızca koordinatları'ı seçin. Koordinat listesi sütunudur
6. İki koordinat listesi alanın formülü kullanarak öğeleri birleştirerek virgülle ayrı LatLong sütun listesi koordinat sütunu dönüştürmek için yeni bir sütun ekleyin ```Text.From([coordinates]{1})&","&Text.From([coordinates]{0})```.
7. Son olarak dönüştürmek ```Elevation``` ondalık ve sütun **Kapat** ve **Uygula**.

Yukarıdaki adımları yerine betikleri Power bı'da bir sorgu dilinde veri dönüşümleri yazmaya olanak tanıyan Gelişmiş Düzenleyici'de kullanılan adımları çıkış aşağıdaki kod yapıştırabilirsiniz.

```pqfl
let
    Source = Json.Document(Web.Contents("https://cahandson.blob.core.windows.net/samples/volcano.json")),
    #"Converted to Table" = Table.FromList(Source, Splitter.SplitByNothing(), null, null, ExtraValues.Error),
    #"Expanded Column1" = Table.ExpandRecordColumn(#"Converted to Table", "Column1", {"Volcano Name", "Country", "Region", "Location", "Elevation", "Type", "Status", "Last Known Eruption", "id"}, {"Volcano Name", "Country", "Region", "Location", "Elevation", "Type", "Status", "Last Known Eruption", "id"}),
    #"Expanded Location" = Table.ExpandRecordColumn(#"Expanded Column1", "Location", {"coordinates"}, {"coordinates"}),
    #"Added Custom" = Table.AddColumn(#"Expanded Location", "LatLong", each Text.From([coordinates]{1})&","&Text.From([coordinates]{0})),
    #"Changed Type" = Table.TransformColumnTypes(#"Added Custom",{{"Elevation", type number}})
in
    #"Changed Type"
```

Artık Power BI veri modelinizde verilere sahip. Power BI desktop, aşağıdaki gibi görünmelidir:

![Power BI desktop](./media/vm-do-ten-things/PowerBIVolcanoData.png)

Raporlar ve veri modelini kullanarak görselleştirmeler oluşturmaya başlayabilirsiniz. Bu adımları takip edebilirsiniz [Power BI makalesinde](../../cosmos-db/powerbi-visualize.md#build-the-reports) bir rapor oluşturmak için. Çıktı aşağıdakine benzer bir rapordur.

![Power BI Desktop rapor görünümü - Power BI Bağlayıcısı](./media/vm-do-ten-things/power_bi_connector_pbireportview2.png)

## <a name="9-dynamically-scale-your-dsvm-to-meet-your-project-needs"></a>9. DSVM proje gereksinimlerinizi karşılayacak şekilde dinamik olarak ölçeklendirin
Proje gereksinimlerinizi karşılamak için DSVM yukarı ve aşağı ölçeklendirilebilir. Akşam veya hafta sonları VM kullanılacak gerekmiyorsa, yalnızca VM'yi kapatabilirsiniz [Azure portalında](https://portal.azure.com).

> [!NOTE]
> VM'de yalnızca işletim sistemi kapatma düğmesini kullanırsanız, işlem ücreti alınır.  
> 
> 

Bazı büyük ölçekli analiz işlemek ve daha fazla CPU ve/veya bellek ve/veya disk kapasitesine sahip olmanız gerekiyorsa, derin öğrenme, bellek kapasitesi ve disk türleri (katı hal sürücüleri dahil) VM boyutlarının CPU çekirdekleri, GPU tabanlı örnekler açısından büyük bir seçim bulabilirsiniz Bu, işlem ve bütçe gereksinimlerinizi karşılayın. Fiyatlandırma, saatlik işlem ile birlikte tam VM'lerin listesini edinilebilir [Azure sanal makineler fiyatlandırma](https://azure.microsoft.com/pricing/details/virtual-machines/) sayfası.

Benzer şekilde, VM işleme kapasitesi, gereksinimini azaltır, (örneğin: bir Hadoop veya Spark kümesi için önemli bir iş yükü taşındı), kümeden aşağı ölçeklendirilebilir [Azure portalında](https://portal.azure.com) ve sanal makine Örneğinize ayarlar. Bir ekran görüntüsü aşağıda verilmiştir.

![VM örneği ayarları](./media/vm-do-ten-things/VMScaling.PNG)

## <a name="10-install-additional-tools-on-your-virtual-machine"></a>10. Sanal makinenize ek araçları yükleyin
Common data analytics gereken birçoğunu ele DSVM önceden oluşturulmuş çeşitli araçlar vardır. Önleme tarafından zaman gerek kalmadan yükleyip ortamlarınızda tek tek yapılandırıp kullanmanızı yalnızca kaynaklar için ödeme yaparak paradan tasarruf kazandırır.

Analytics ortamınızı geliştirmek için bu makaledeki profili diğer Azure veri ve Analiz Hizmetleri kullanabilir. Bazı durumlarda, bazı özel üçüncü taraf araçları gibi ek araç gereksinimlerinizi gerektirebilir. İhtiyacınız olan yeni araçları yüklemek için sanal makine üzerinde tam yönetici erişimi var. Ayrıca, önceden yüklü R ve Python ile ek paketleri yükleyebilirsiniz. Python için ya da kullanabilirsiniz ```conda``` veya ```pip```. R için kullanabileceğiniz ```install.packages()``` R konsolunda veya IDE kullanın ve seçin "**paketleri** -> **paketlerini yükle...** ".

## <a name="summary"></a>Özet
Microsoft Veri bilimi sanal makinesi üzerinde yapabileceğiniz şeylerden bazıları şunlardır. Bir etkin analiz ortamını yapmak için yapabileceğiniz çok daha fazla şey vardır.

