---
title: Bir edge makine öğrenimi çözümünüzü Azure ve Azure Stack ile oluşturun | Microsoft Docs
description: Azure ve Azure Stack ile Python kullanarak bir kenar machine learning çözümü oluşturmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/07/2018
ms.author: mabrigg
ms.reviewer: Anjay.Ajodha
ms.openlocfilehash: 5a1f0c0ee8a9f6ef6871e19e7722e09f4e96ba7f
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53142079"
---
# <a name="tutorial-create-an-edge-machine-learning-solution-with-azure-and-azure-stack"></a>Öğretici: Bir edge makine öğrenimi çözümünüzü Azure ve Azure Stack ile oluşturma

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Bir edge makine öğrenimi çözümünüzü Azure ve Azure Stack ile oluşturmayı öğrenin.

Bu öğreticide, bir örnek ortama oluşturacaksınız:

> [!div class="checklist"]
> - Bir depolama hesabı ve kapsayıcı bulunan verileri temizleme için oluşturun.
> - Azure portalında bir Ubuntu veri bilimi sanal makinesi (DSVM) oluşturun.
> - Azure Machine Learning hizmetleri oluşturma ve eğitme modelleri için azure'da dağıtın.
> - Azure Machine Learning hizmet hesapları oluşturun.
> - Dağıtma ve Azure Container Registry kullanma.
> - Azure Stack için bir Kubernetes kümesi dağıtın.
> - Verileri bir Azure Stack depolama hesabını ve depolama kuyruğu oluşturun.
> - Verileri temizleme Azure yığını, Azure'a taşımak için yeni Azure Stack bir işlev oluşturun.

**Bu çözümü kullanmak ne zaman**

 -  Kuruluşunuzun DevOps yaklaşımını kullanan veya bir planlanan sahip yakın bir gelecekte.
 -  Azure Stack uygulamanız ve genel bulutta CI/CD uygulamalarını uygulamak istiyorsunuz.
 -  Bulut ve şirket içi ortamlar genelinde CI/CD işlem hattı birleştirmek istediğiniz.
 -  Bulutta veya şirket içi hizmetleri kullanan uygulamaları geliştirme özelliği kullanmanız gerekir.
 -  Bulut ve şirket uygulamaları arasında tutarlı Geliştirici becerilerden yararlanın istiyorsunuz.

> [!Tip]  
> ![karma pillars.png](./media/azure-stack-solution-cloud-burst/hybrid-pillars.png)  
> Microsoft Azure Stack, Azure'nın bir uzantısıdır. Azure Stack çevikliğini ve yenilik bulut bilgi işlem, şirket içi ortamınıza ve hibrit uygulamaları her yerde oluşturup dağıtmayı olanak tanıyan tek hibrit Bulutu sunar.  
> 
> Teknik incelemeyi [karma uygulamaları için tasarım konuları](https://aka.ms/hybrid-cloud-applications-pillars) (yerleştirme, ölçeklenebilirlik, kullanılabilirlik, dayanıklılık, yönetilebilirlik ve güvenlik) yazılım kalitesinin yapı taşları tasarlama, dağıtma ve çalıştırma için gözden geçirmeleri karma uygulamalar. Tasarım konuları, üretim ortamlarında sorunlarını en aza karma uygulama tasarımının en iyi duruma getirme yardımcı olur.

## <a name="prerequisites"></a>Önkoşullar

Bazı bileşenler, bu kullanım örneği oluşturmak için gereken ve hazırlamak için biraz zaman alabilir:

 -  Bir Azure OEM/donanım iş ortağı, bir üretim Azure Stack'te dağıtabilir ve tüm kullanıcıların bir ASDK dağıtabilir.

 -  Bir Azure Stack operatörü gerekir ayrıca App Service'e dağıtım, planlar ve Teklifler oluşturma, bir kiracı aboneliği oluşturmak ve Windows Server 2016 görüntüsü ekleme

 -  Bir hibrit ağ ve App Service Kurulumu gereklidir (daha fazla bilgi edinin [sanal ağlar ile uygulama tümleştirme.](https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet))

 -  Özel [derleme ve yayın Aracısı](https://docs.microsoft.com/vsts/pipelines/agents/agents?view=vsts) için VSTS tümleştirmesi (var olan tüm bileşenlerin kullanılan olun başlamadan önce gereksinimleri karşılayın.) dağıtımdan ayarlanmış olması gerekir

Azure ve Azure Stack biri gereklidir. Devam etmeden önce daha fazla bilgi için şu konularla başlatın:

 -  [Azure'a giriş](https://azure.microsoft.com/overview/what-is-azure/)

 -  [Azure Stack temel kavramları](https://docs.microsoft.com/azure/azure-stack/azure-stack-key-features)

 -  [Azure Stack, karma, CI/CD çözüm Kılavuzu](/azure/azure-stack/user/azure-stack-solution-pipeline)

**Azure**

 -  Bir Azure aboneliği (oluşturma bir [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F))

 -  Tarafından oluşturulan yeni bir Web uygulaması URL'sine [Web uygulaması](https://docs.microsoft.com/azure/app-service/app-service-web-overview) azure'da

 -  Dağıtımı [Azure kapsayıcı Hizmetleri (ACS) azure'da Kubernetes](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-solution-template-kubernetes-deploy)

 -  Azure Machine Learning hizmetinin dağıtımı [2 bölümlü öğretici](https://docs.microsoft.com/azure/machine-learning/service/tutorial-train-models-with-aml)


**Azure Stack**

 -  Azure Stack tümleşik sistemi veya Azure Stack geliştirme Seti'ni dağıtımı.

    - Azure Stack, yüklemek için yönergeler bulun [Azure Stack geliştirme Seti'ni yüklemek](../asdk/asdk-install.md).
     - [https://github.com/mattmcspirit/azurestack/blob/master/deployment/ConfigASDK.ps1](https://github.com/mattmcspirit/azurestack/blob/master/deployment/ConfigASDK.ps1) Bu yüklemenin tamamlanması birkaç saat gerektirebilir.

 -  Dağıtımı [App Service](https://docs.microsoft.com/azure/azure-stack/azure-stack-app-service-deploy) PaaS Hizmetleri Azure stack'e

 -  A [planı/teklifler](https://docs.microsoft.com/azure/azure-stack/azure-stack-plan-offer-quota-overview) Azure Stack ortamında

 -  A [Kiracı aboneliği](https://docs.microsoft.com/azure/azure-stack/azure-stack-subscribe-plan-provision-vm) Azure Stack ortamında

 -  Bir Ubuntu Server VM görüntüsü (kullanılabilir [Azure Stack Marketini](https://buildazure.com/2016/05/04/azure-marketplace-ubuntu-server-16-04-lts/)) bu VM Kubernetes VM'lerin yanı sıra özel bir yapı aracısı olarak Azure Stack'te Kiracı aboneliği oluşturulur. Bu görüntü kullanılamıyor, Azure Stack operatörü, bu ortama eklendiğinden emin olmak için size yardımcı olabilir. (Bu şu anda desteklenmediğinden, ubuntu 18.04 yapısını kullanmayın.)

 -  Kiracı abonelik (daha sonra kullanmak için yeni Web App URL'si unutmayın.) içinde bir Web uygulaması

 -  Dağıtım, VSTS Linux tabanlı özel derleme aracısı sanal makinede, Kiracı aboneliği

 -  Dağıtım bir [Azure kapsayıcı Hizmetleri (ACS) Kubernetes Azure Stack üzerinde](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-solution-template-kubernetes-deploy)

**Geliştirici Araçları**

 -  [VSTS çalışma](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services) (kayıt işlemini "MyFirstProject" adlı bir proje oluşturur)

 -  [Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) yükleme ve [VSTS](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services) oturum açma

 -  [Yerel kopya](https://www.visualstudio.com/docs/git/gitquickstart) projesinin

 -  [Windows 10 için Linux alt](https://docs.microsoft.com/windows/wsl/install-win10) yüklemede (BASH ve SSH)

 -  [Windows için docker](https://download.docker.com/win/stable/Docker%20for%20Windows%20Installer.exe) yükleme

 -  [Azure Machine Learning Workbench (Önizleme)](https://aka.ms/azureml-wb-msi) yükleme

 -  [Python](https://www.python.org/ftp/python/3.7.0/python-3.7.0rc1-amd64.exe) ortamına yükleme

**VSTS**

 -  **VSTS hesabı.** Hızlı bir şekilde derleme, test ve dağıtım için sürekli tümleştirme ayarlayın. VSTS hesapları hakkında daha fazla bilgi için bkz. [VSTS hesabı oluşturma](https://docs.microsoft.com/vsts/accounts/create-account-msa-or-work-student?view=vsts).

 -  **Kod deposu.** GitHub, BitBucket, DropBox, Onedrive ve TFS içinde varolan kod depoları kullanarak VSTS platform geliştirme işlem hattı kolaylaştırmak için birden çok kod depoları yararlanabilirsiniz. Depoları kod hakkında daha fazla bilgi için bkz: [Git ve VSTS ile çalışmaya başlama](https://docs.microsoft.com/vsts/git/gitquickstart?view=vsts&tabs=visual-studio) öğretici.

 -  **Hizmet bağlantı.** Test, yapı veya dağıtım görevleri yürütmek üzere dış veya uzak hizmetlere bağlanın. VSTS hakkında daha fazla bilgi için bkz: hizmet bağlantıları [derleme ve yayınlama için hizmet uç noktası](https://docs.microsoft.com/vsts/build-release/concepts/library/service-endpoints?view=vsts).

 -  **Derleme tanımları.** Otomatik yapı işlemlerini tanımlarken ve bir dizi görev Kataloğu aracılığıyla görevi oluşturun. Derleme hakkında daha fazla bilgi için bkz: tanımları [bir yapı tanımı oluşturun](https://docs.microsoft.com/vsts/build-release/actions/ci-cd-part-1?view=vsts) belgeleri.

 -  **Yayın tanımları.** Uygulama yapıt dağıtıldığı ortamların bir koleksiyon için dağıtım işlemi tanımlayın. Sürüm hakkında daha fazla bilgi için bkz: tanımları [bir yayın tanımı oluşturma](https://docs.microsoft.com/vsts/build-release/actions/ci-cd-part-1?view=vsts) belgeleri.

 -  **VSTS barındırılan Linux aracı havuzu oluşturun.** Hızlı, test uygulamaları derlemeye ve dağıtmaya barındırılan aracı kullanma yönetilir ve sürdürülür. Barındırılan VSTS derleme hakkında daha fazla bilgi için bkz: aracıları [barındırılan aracıları](https://docs.microsoft.com/vsts/build-release/concepts/agents/hosted?view=vsts) belgeleri.

## <a name="step-1-create-a-storage-account"></a>1. Adım: Depolama hesabı oluşturma

Bir depolama hesabı ve kapsayıcı bulunan verileri temizleme için oluşturun.

1.  Oturum [ *Azure portalında*](https://portal.azure.com/).

2.  Azure portalında hizmet menüsünü açın ve sol taraftaki menüyü genişleterek **tüm hizmetleri**. Ekranı aşağı kaydırarak **depolama** ve **depolama hesapları**. İçinde **depolama hesapları** pencere **Ekle**.

3.  Depolama hesabı için bir ad girin.

    > [!Note]  
    > Depolama hesabı adları 3 ile 24 karakter arasında olmalı ve yalnızca sayıyla küçük harf içermelidir. Depolama hesabı adı Azure'da benzersiz olması gerekir. Azure portalı, seçilen depolama hesabı adı zaten kullanımda olup olmadığını gösterir.

4.  Kullanılacak dağıtım modelini belirtin: **Resource Manager**.

5.  Depolama hesabı türünü seçin: **Genel amaçlı V1**, ardından performans katmanını belirtin: **Standart**.

6.  Depolama hesabı için çoğaltma seçeneğini seçin: **GRS**.

7.  Yeni depolama hesabı aboneliği seçin.

8.  Yeni bir kaynak grubu belirtin veya varolan bir kaynak grubunu seçin.

9.  Depolama hesabı için coğrafi konumu seçin.

10. Depolama hesabını oluşturmak için **Oluştur**’u seçin.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image1.png)

11.  Son oluşturulan depolama hesabını seçin.

12.  SELECT deyiminde **Blobları**.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image2.png)

13.  SELECT deyiminde **+ kapsayıcı** ve select deyiminde **kapsayıcı**.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image3.png)

14.  Kapsayıcı adını verin **uploadeddata** erişim türünü seçin **kapsayıcı**.

15.  SELECT deyiminde **oluşturma**.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image4.png)

## <a name="step-2-create-a-data-science-virtual-machine"></a>2. Adım: Bir veri bilimi sanal makinesi oluşturma

Azure portalında bir Ubuntu veri bilimi sanal makinesi (DSVM) oluşturun.

1.  Azure portalında oturum açın [https://portal.azure.com](https://portal.azure.com/)

2.  SELECT deyiminde **+ yeni** bağlantı ve "veri bilimi sanal makinesi için Linux Ubuntu CSP arayın

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image5.png)

1.  Seçin **Linux (Ubuntu) için veri bilimi sanal makinesi** listesi ve izleme ekran DSVM oluşturma için yönergeler.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image6.png)

> [!Important]  
> Seçin **parola** olarak **kimlik doğrulama türü**.

Yeni oluşturulan depolama hesabı aynı kaynak grubunda yeni DSVM yerleştirin. Tüm uç ML nesneleri, bu kaynak grubunda azure'da dağıtılır.

1.  Ayarları yapılandırmak isteğe bağlı özellikleri

    a.  Seçin **depolama hesabı** daha önce oluşturduğunuz.

    b.  Yeni bir **sanal ağ**, **alt**, ve **genel IP** , kaynak grubu adına göre bir adı varsayılan olarak oluşturmanız gerekir.

    c.  Yeni bir **NSG** bu uygulanmış doğru kuralları otomatik olarak oluşturması.

    d.  İçin **tanılama depolama hesabı**, daha önce oluşturduğunuz depolama hesabını seçin.

    > [!Note]  
    > Azure aboneliği için yapılandırılmış ve etkin AAD ile Azure kaynakları için yönetilen kimlikleri de etkinleştirilebilir.

2.  **Tamam**’ı seçin.

### <a name="connect-to-the-dsvm"></a>Değerini DSVM Örneğinize bağlanın

DSVM oluşturulduktan sonra VM'ye Linux için Windows alt sisteminden bağlanın.

```Bash  
    ssh <user>@<DSVM Public IP>
```

1.  Evet, VM bağlantısı onaylamanız istendiğinde yazın.

2.  DSVM için oluşturduğunuz parolayı girin.

### <a name="update-the-dsvm"></a>DSVM güncelleştir 

```Bash  
sudo su 
apt-get update 
apt-get -y upgrade 
apt-get -y dist-upgrade 
apt-get -y autoremove
```

## <a name="step-3-deploy-azure-machine-learning-services"></a>3. adım: Azure Machine Learning Hizmetleri dağıtma

Azure Machine Learning Hizmetleri azure'da dağıtın.

Azure Machine Learning hizmetleri (önizleme) tümleşik ve uçtan uca bir veri bilimi ve gelişmiş analiz çözümüdür. Bu çözüm uzman veri bilimcilerin bulut ölçeğinde veri hazırlamasına, deneme geliştirmesine ve model dağıtmasına yardımcı olur.

Bu bölümde açıklanmaktadır:

> [!div class="checklist"]
> - Azure Machine Learning hizmetleri için hizmet hesapları oluşturma
> - Azure Machine Learning Workbench'i yükleme ve oturumunu açma.
> - Workbench'te proje oluşturma
> - Bu projede betik çalıştırma
> - Komut satırı arabirimine (CLI) erişme

Microsoft Azure portföyünün bir parçası olarak, Azure Machine Learning hizmetleri için bir Azure aboneliği gerekir. Bir Azure aboneliği edinme oluşturma bir [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

Kaynak grupları, sanal makineler ve diğer varlıklar gibi varlıkları oluşturmak için yeterli izinleri gerekir.

Azure Machine Learning Workbench uygulamasını şu işletim sistemlerine yüklenebilir:

 -  Windows 10 veya Windows Server 2016
 -  macOS Sierra veya High Sierra

## <a name="step-4-create-azure-machine-learning-services"></a>4. adım: Azure Machine Learning hizmetleri oluşturma

Azure Machine Learning hizmet hesapları oluşturun.

Azure Machine Learning hesapları sağlamak için Azure portalını kullanın:

1.  Oturum [Azure portalında](https://portal.azure.com/) kullanılacak Azure aboneliği için kimlik bilgilerini kullanarak. Bir Azure aboneliği edinme oluşturma bir [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image7.png)

1.  Portalın sol üst köşesinde bulunan **Kaynak oluştur** düğmesini (+) seçin.

    ![Azure portalında kaynak oluşturma](media/azure-stack-solution-machine-learning/image8.png)

1.  Arama çubuğuna **Machine Learning** yazın. **Machine Learning Denemesi (önizleme)** adlı arama sonucunu seçin.

    ![Azure Machine Learning araması](media/azure-stack-solution-machine-learning/image9.png)

1.  İçinde **Machine Learning denemesi** bölmesinde seçin ve altındaki kaydırma **Oluştur** deneme hesabı tanımlamaya başlayın.

    ![Azure Machine Learning - deneme hesabı oluşturma](media/azure-stack-solution-machine-learning/image10.png)

1.  İçinde **ML denemesi** bölmesinde Machine Learning denemesi hesabı yapılandırın.

    | Ayar | Öğretici için önerilen değer | Açıklama |
    |---------------------------------------|----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | Deneme hesabı adı | Benzersiz ad | Hesabını tanımlayan benzersiz bir ad girin. Denemeyi en iyi şekilde tanımlayan departman ya da proje adı kullanın. Adı 2-32 karakter arasında olmalıdır. Yalnızca alfasayısal karakterler ve kısa çizgi (-) karakteri kullanılabilir. |
    | Abonelik | Abonelik | Deneme için kullanılacak Azure aboneliğini seçin. Birden fazla aboneliğiniz varsa kaynağın faturalanacağı uygun aboneliği seçin. |
    | Kaynak grubu | Kaynak grubu | Abonelikte mevcut bir kaynak grubunu kullanın veya bu deneme hesabı için yeni bir kaynak grubu oluşturmak için bir ad girin. |
    | Konum | Kullanıcılara en yakın bölge | Kullanıcıları ve veri kaynakları için en yakın konumu seçin. |
    | Bilgisayar lisansı sayısı | 2 | Bilgisayar lisansı sayısını girin. [Bilgisayar lisansının fiyatı nasıl etkilediğini](https://azure.microsoft.com/pricing/details/machine-learning/) öğrenin.<br><br>Bu hızlı başlangıçta, yalnızca iki bilgisayar lisansı gereklidir. Bilgisayar lisansları gerektiğinde Azure portalından eklenebilir veya kaldırılabilir. |
    | Depolama hesabı | Benzersiz ad | [Azure depolama hesabı](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=portal) oluşturmak için **Yeni oluştur**'u seçin ve bir ad girin. Ad 3 ile 24 karakter arasında olmalı ve yalnızca alfasayısal karakterler içermelidir. Alternatif olarak, seçin **var olanı kullan** ve listeden var olan depolama hesabını seçin. Depolama hesabı gereklidir ve proje yapıtlarını tutmak ve geçmiş verileri çalıştırmak için kullanılır. |
    | Deneme hesabı için çalışma alanı | IrisGarden<br>(öğreticilerde kullanılan ad) | Bu hesap için çalışma alanına bir ad sağlayın. Adı 2-32 karakter arasında olmalıdır. Yalnızca alfasayısal karakterler ve kısa çizgi (-) karakteri kullanılabilir. Bu çalışma alanı oluşturma, yönetme ve deneyimleri yönetip yayımlamak için gereken araçları içerir. |
    | Çalışma alanının sahibini atama | Hesap | Çalışma alanı sahibi olarak kendi hesabı seçin. |
    | Model Yönetimi Hesabı oluşturma | **işaretle** | Bir Model Yönetimi hesabı oluşturun. Bu model, gerçek zamanlı web Hizmetleri olarak dağıtıp yönetmek için kullanılır. <br><br>İsteğe bağlı olsa da, Model Yönetimi hesabını deneme hesabı ile aynı zamanda oluşturmanız önerilir. |
    | Hesap adı | Benzersiz ad | Model Yönetimi hesabını tanımlayan benzersiz bir ad seçin. Departman kullanın veya denemeyi en iyi proje adını tanımlar. Adı 2-32 karakter arasında olmalıdır. Yalnızca alfasayısal karakterler ve kısa çizgi (-) karakteri kullanılabilir. |
    | Model Yönetimi fiyatlandırma katmanı | **DEVTEST** | Seçin **fiyatlandırma katmanı seçilmedi** yeni Model Yönetimi hesabı fiyatlandırma katmanını belirtmek için. Abonelikte (sınırlı kullanılabilirlik) varsa maliyet tasarrufu için DEVTEST fiyatlandırma katmanını seçin. Yoksa S1 fiyatlandırma katmanını seçin. Fiyatlandırma katmanı seçimini kaydetmek için seçin'i seçin. |
    | Panoya sabitle | işaretli | Seçin **panoya Sabitle** Azure portal'ın ön Pano sayfasında Machine Learning denemesi hesabı kolayca izlenmesine izin vermek için seçeneği. |

    ![Machine Learning Denemesi hesap yapılandırması](media/azure-stack-solution-machine-learning/image11.png)

1.  Model Yönetim hesabıyla birlikte Deneme hesabı oluşturma işlemine başlamak için **Oluştur**'u seçin.

    ![Machine Learning Denemesi hesap yapılandırması](media/azure-stack-solution-machine-learning/image12.png)

    Bu hesap oluşturmak için birkaç dakika sürebilir. Azure portalı araç çubuğundaki bildirim simgesine (zil) seçerek dağıtım işleminin durumunu denetleyin.

    ![Azure portalı bildirimleri](media/azure-stack-solution-machine-learning/image13.png)

### <a name="install-and-log-in-to-workbench"></a>Yükleme ve workbench oturumunu açın 

Azure Machine Learning Workbench, Windows veya macOS için sağlanır. [Desteklenen platformların](https://docs.microsoft.com/azure/machine-learning/service/quickstart-installation) listesine bakın.

> [!Warning]  
> Yüklemeyi tamamlamak için geçici olarak bir saat sürebilir.

1.  En son Workbench yükleyicisini indirin ve başlatın.

    > [!Important]  
    > Yükleyiciyi diske tam olarak indirip oradan çalıştırın. Çalıştırma, doğrudan tarayıcınızın indirme pencere öğesinden.<br>**Üzerinde Windows:<br>**  bir. [AmlWorkbenchSetup.msi](https://aka.ms/azureml-wb-msi) dosyasını indirin.<br>b. Dosya Gezgini'nde indirilen yükleyiciye çift tıklayın.

1.  İzleyin ekrandaki tamamlanma yükleyicideki yönergeleri.

    **Yüklemenin tamamlanması olarak 30 dakika sürebilir.**
    
    `Windows: C:\\Users\\<user>\\AppData\\Local\\AmlWorkbench`
    
    Yükleyiciyi indirin ve Python, Miniconda ve diğer ilgili kitaplıklar gibi gereken tüm bağımlılıkları, ayarlayın. Bu yükleme ayrıca Azure platformlar arası komut satırı aracı veya Azure CLI'yı içerir.

1.  Yükleyicinin son ekranındaki **Workbench'i Başlat** düğmesini seçerek Workbench'i başlatın.

    Yükleyici kapattıysanız, yeniden başlatırken **Machine Learning Workbench** masaüstü kısayolu.

1.  Seçin **Microsoft'ta oturum açma** Azure Machine Learning Workbench'te kimlik doğrulaması için. Azure portalında deneme ve Model Yönetimi hesapları oluşturmak için kullanılan kimlik bilgilerini kullanın.

    Oturum açtıktan sonra Workbench Azure aboneliklerinde bulur ve tüm çalışma alanları ve projeler bu hesapla ilişkilendirilmiş görüntüler ilk deneme hesabını kullanır.

    > [!Tip]  
    > Workbench uygulama penceresinin sol alt köşesindeki simgeyi kullanarak farklı bir deneme hesabına geçin.

### <a name="create-a-new-project-in-workbench"></a>Workbench'te yeni proje oluşturma

1.  Azure Machine Learning Workbench uygulamasını açın ve gerekirse oturum açın.

    - Windows üzerinde başlatırken **Machine Learning Workbench** masaüstü kısayolu.

    - macOS’ta Launchpad’den **Azure ML Workbench**’i seçin.

1.  **PROJELER** bölmesinden artı işaretini (+) seçip **Yeni Proje**’yi seçin.

    ![Yeni çalışma alanı](media/azure-stack-solution-machine-learning/image14.png)

1.  Form alanlarını doldurun ve **Oluştur** düğmesini seçerek Workbench’te yeni projeyi oluşturun.

    | Alan | Öğretici için önerilen değer | Açıklama |
    |-------------------------------------|------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | Proje adı | myIris | Hesabını tanımlayan benzersiz bir ad girin. Departman kullanın veya denemeyi en iyi proje adını tanımlar. Adı 2-32 karakter arasında olmalıdır. Yalnızca alfasayısal karakterler ve kısa çizgi (-) karakteri kullanılabilir. |
    | Proje dizini | c:\Temp\ | Projenin oluşturulduğu dizini belirtin. |
    | Proje açıklaması | Boş bırakın | Projeleri açıklamak için kullanışlı bir isteğe bağlı alan. |
    | Visualstudio.com GIT Deposu URL’si | Boş bırakın | İsteğe bağlı alan. Bir proje kaynak denetimi ve işbirliği için depoları Azure üzerinde bir Git deposu ile ilişkilendirin. |
    | Seçili çalışma alanı | IrisGarden (varsa) | Azure portalında deneme hesabı için oluşturulan çalışma alanını seçin. <br>Hızlı Başlangıç adımlarını kullanarak, çalışma alanı adı Irisgarden tarafından listelenir. Aksi takdirde, çalışma alanı deneme hesabı adı veya tercih edilen hesap adı kullanın. |
    | Proje şablonu | Classifying Iris | Betikleri ve verileri ürünü keşfetmek için kullanılan şablonları içerir. Bu şablon, betikler ve bu belge sitesindeki bu hızlı başlangıçta ve diğer öğreticiler için gerekli verileri içerir. |

    ![Yeni proje](media/azure-stack-solution-machine-learning/image15.png)

1.  Yeni proje oluşturulur ve bu projeyi içeren proje panosu açılır. Proje giriş sayfası, veri kaynakları, dizüstü bilgisayarlar ve kaynak kodu dosyaları keşfedin.

    ![Projeyi açma](media/azure-stack-solution-machine-learning/image16.png)

### <a name="attach-a-dsvm-compute-target"></a>DSVM işlem hedefi ekleme

DSVM oluşturulduktan sonra Azure ML projeye ekleyin.

1.  Azure ML Workbench uygulamasında Azure ML Workbench CLI'yı seçerek başlangıç **dosya**->**PowerShell'i açın**

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image17.png)

1.  Aşağıdaki komut istemi açtı PowerShell'i sonra:

    ```PowerShell  
        az login
    ```

1.  Aşağıdaki istemi alır:

     ![Alternatif metin](media/azure-stack-solution-machine-learning/image18.png)

1.  Site isteminde ayrıntılı olarak göz atın ve sağlanan kodu girin.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image19.png)

1.  İstendiğinde Devam'ı seçin, sonra Azure ML Deneysel hesabı ile ilişkili Azure hesabındaki seçin.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image20.png)

1.  Azure ML Workbench CLI'yı, ardından aşağıdaki istemi gönderir:

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image21.png)

1.  ML hesabı ve çalışma alanı oturum açma başarılı olarak gösterildiğinde DSVM ekleyin.

    ```PowerShell  
        az ml computetarget attach remotedocker --name <compute target name> --address <ip address or FQDN> --username <admin username> --password <admin password>
    ```

    Aşağıdaki uyarı görünür:

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image22.png)

    ```PowerShell  
        # prepare the Docker image on the DSVM 
        az ml experiment prepare -c <compute target name>
    ```

Bu işlem biraz zaman alabilir ve çeşitli docker görüntüleri'ından alınır, bunları kaydeder ve sonra gerekli kodu ve uygulamaları uygular metin önemli miktarda oluşturur.

Denemeleri artık bu DSVM üzerinde çalıştırabilirsiniz.

### <a name="create-a-data-preparation-package"></a>Veri hazırlama paketi oluşturma

Ardından, Azure Machine Learning workbench'te hazırlamaya başlayın. Workbench'te gerçekleştirilen her dönüştürme bir yerel veri hazırlama paketi JSON biçiminde depolanır (\*.dprep dosyası). Bu veri hazırlama paketi, workbench'teki veri hazırlama çalışmalarınız için birincil kapsayıcıdır.

Bu veri hazırlama paketi daha sonra bir çalışma zamanı yerel C gibi devredilebilir\#/CoreCLR, Scala/Spark veya Scala/HDI.

1.  Klasör simgesini seçerek Dosya görünümü açın ve **iris.csv**’yi seçerek dosyayı açın.

    Bu dosya 5 sütun ve 50 satırdan oluşan bir tablo içerir. Dört sütun, sayısal özellik sütunlarıdır. Beşinci sütun dize hedef sütunudur. Sütunların hiçbirinin üst bilgi adı yoktur.

    ![iris.csv](media/azure-stack-solution-machine-learning/image23.png)

1.  Yeni bir veri kaynağı eklemek için **Veri görünümü**’nde artı işaretini (**+**) seçin. **Veri Kaynağı Ekle** sayfası açılır.

    ![Azure Machine Learning Workbench’teki Veri görünümü](media/azure-stack-solution-machine-learning/image24.png)

1.  Seçin **metin dosyaları (\*.csv \*.json, \*.txt.,...)** .

    ![Azure Machine Learning Workbench uygulamasında veri kaynağı](media/azure-stack-solution-machine-learning/image25.png)

1.  **İleri**’yi seçin.

2.  Dosyaya Gözat **iris.csv**seçip **son**. Bu işlem, ayırıcı ve veri türleri gibi parametreler için varsayılan değerleri kullanır.

    > [!Important]  
    > Seçin **iris.csv** dosyasını bu alıştırma için geçerli proje dizininden. Aksi takdirde sonraki adımlar başarısız olabilir.

    ![Iris seçme](media/azure-stack-solution-machine-learning/image26.png)

1.  Adlı yeni bir dosya `*iris-1.dsource` oluşturulur. Dosya ile benzersiz şekilde adlandırılmıştır `-1` örnek proje zaten numaralandırılmamış ile geldiğinden **iris.dsource** dosya.

    Dosya açılır ve veriler gösterilir. Bir dizi sütun üst öğesinden **Column1** için **sütun5**, bu veri kümesine otomatik olarak eklenir. Alt kısma kaydırın ve veri kümesinin son satırının boş olduğuna dikkat edin. CSV dosyasında fazladan bir satır sonu nedeniyle bir satır boştur.

    ![Iris veri görünümü](media/azure-stack-solution-machine-learning/image27.png)

1.  **Ölçümler** düğmesini seçin. Histogramlar oluşturulur ve görüntülenir.

    Seçerek veri görünümüne geçiş **veri** düğmesi.

    ![Iris veri görünümü](media/azure-stack-solution-machine-learning/image28.png)

1.  Histogramları gözlemleyin. Her sütun için eksiksiz bir istatistik hesaplanmıştır.

    ![Iris veri görünümü](media/azure-stack-solution-machine-learning/image29.png)

1.  **Hazırla** düğmesini seçerek bir veri hazırlama paketi oluşturmaya başlayın. **Hazırla** iletişim kutusu açılır.

    Örnek Proje içeren bir **iris.dprep** varsayılan olarak seçili veri hazırlama dosyası.

    ![Iris veri görünümü](media/azure-stack-solution-machine-learning/image30.png)

1.  Seçerek yeni bir veri hazırlama paketi oluşturma **+ yeni veri hazırlama paketi** menüsünde.

    ![Iris veri görünümü](media/azure-stack-solution-machine-learning/image31.png)

1.  Paket adı için yeni bir değer girin (**iris-1**’i kullanın) ve ardından **Tamam**’ı seçin.

    Adlı yeni bir veri hazırlama paketi **iris-1.dprep** oluşturulup veri hazırlama düzenleyicisinde açılır.

    ![Iris veri görünümü](media/azure-stack-solution-machine-learning/image32.png)

    Ardından, veri hazırlama gereklidir.

1.  Üst bilgi metnini düzenlenebilir hale getirmek için her bir sütun başlığını seçin. Ardından her sütunu aşağıdaki gibi yeniden adlandırın:

    Sırayla girin **Sepal uzunluğu**, **Sepal genişliği**, **Petal uzunluğu**, **Petal genişliği**, ve **türler** beş sütun için sırasıyla.

    ![Sütunları yeniden adlandırma](media/azure-stack-solution-machine-learning/image33.png)

1.  Benzersiz değerleri sayın:

    1.  **Türler** sütununu seçin

    2.  Seçmek için sağ tıklayın.

    3.  Seçin **değer sayıları** menüsünde.

        Verilerin altında **Denetçiler** bölmesi açılır. Dört çubuklu bir histogram görüntülenir. Hedef sütunda dört farklı değer bulunur: **Iris-virginica**, **Iris-versicolor**,**Iris-setosa**ve **(null)** değeri.

    ![Değer Sayıları seçme](media/azure-stack-solution-machine-learning/image34.png)

    ![Değer sayısı histogramı](media/azure-stack-solution-machine-learning/image35.png)

1.  Null değerleri filtreleyip dışarıda bırakmak için "(null)" çubuğunu ve sonra da eksi işaretini (**-**) seçin.

    Bunun üzerine, (null) satırının rengi gri olur ve filtrenin uygulandığını gösterir.

    ![Null değerleri filtrenin dışında bırakma](media/azure-stack-solution-machine-learning/image36.png)

1.  **ADIMLAR** bölmesinde ayrıntılı olarak anlatılan tek tek veri hazırlama adımlarına dikkat edin. Sütunları adlandırıp null değerli satırları filtreledikten gibi her eylem bir veri hazırlama adımı olarak kaydedilir. Ayarlarını değiştirebilir, adımları yeniden sıralayabilir ve adımları kaldırabilirsiniz her bir adımı düzenleyerek.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image37.png)

1.  Veri hazırlama düzenleyicisini kapatın. Grafik simgesiyle gösterilen **iris-1** sekmesini kapatmak için sekmedeki **x** simgesini seçin. İş otomatik olarak kaydedilir **iris-1.dprep** altında gösterilen dosya **veri hazırlıkları** başlığı.

    ![Kapat](media/azure-stack-solution-machine-learning/image38.png)

### <a name="generate-python-code-to-invoke-a-data-preparation-package"></a>Bir veri hazırlama paketini çağırmak için Python kodu oluşturma

Veri hazırlama paketinin çıkışı doğrudan Python'da veya Jupyter Notebook’ta incelenebilir. Paketler, yerel Python, Spark (Docker dahil) ve HDInsight gibi birden fazla çalışma zamanı arasında çalıştırılabilir.

1.  Veri Hazırlıkları sekmesinden **iris-1.dprep** dosyasını bulun.

2.  **iris-1.dprep** dosyasına sağ tıklayın ve bağlam menüsünden **Veri Erişim Kodu Dosyası Oluştur**’u seçin.

    ![Kod oluşturma](media/azure-stack-solution-machine-learning/image39.png)

    Adlı yeni bir dosya **iris-1.py** veri hazırlık paketi olarak oluşturduğunuz mantığı çağırır kodu aşağıdaki satırlarla açılır:

    ```Python
    # Use the Azure Machine Learning data preparation package
    from azureml.dataprep import package

    # Use the Azure Machine Learning data collector to log various metrics
    from azureml.logging import get_azureml_logger
    logger = get_azureml_logger()

    # This call will load the referenced package and return a DataFrame.
    # If run in a PySpark environment, this call returns a
    # Spark DataFrame. If not, it will return a Pandas DataFrame.
    df = package.run('iris-1.dprep', dataflow_idx=0)
    # Remove this line and add code that uses the DataFrame
    df.head(10)
    ```

    Bu kodun çalıştırıldığı bağlama bağlı olarak drep farklı bir DataFrame türünü temsil eder:

    -  Python çalışma zamanında yürütülürken bir [pandas DataFrame](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html) kullanılır.

    -  Spark bağlamında çalıştırılırken bir [Spark DataFrame](https://spark.apache.org/docs/latest/sql-programming-guide.html) kullanılır.

### <a name="review-irissklearnpy-and-the-configuration-files"></a>iris_sklearn.py dosyasını ve yapılandırma dosyalarını gözden geçirme

1.  Proje Aç seçin **dosyaları** düğmesine (klasör simgesi) en sol bölmedeki proje klasöründe dosya listesini açın.

    ![Azure Machine Learning Workbench projesini açma](media/azure-stack-solution-machine-learning/image40.png)

1.  **iris_sklearn.py** Python betik dosyasını seçin.

    ![Betik seçme](media/azure-stack-solution-machine-learning/image41.png)

    Kod, Workbench içinde yeni bir metin düzenleyici sekmesinde açılır.

    > [!Note]  
    > Bu basit proje sıklıkla güncelleştirildiğinden görüntülenen kodu tam olarak yukarıdaki kodla aynı olmayabilir.

    ![Dosya açma](media/azure-stack-solution-machine-learning/image42.png)

1.  Python betik kodunu inceleyerek kodlama stili hakkında bilgi edinin.

    **iris_sklearn.py** betiği aşağıdaki görevleri gerçekleştirir:  

    -  **iris.dprep** adlı varsayılan veri hazırlama paketini yükleyerek bir [pandas DataFrame](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html) oluşturun.

    -   Sorunun çözülmesini zorlaştırmak için rastgele özellikler ekler. Iris yaklaşık %100 doğrulukla kolayca sınıflandırılan küçük bir veri kümesi olduğundan rastgele hale getirilmesi gerekir.

    -  Thescikit learnmachine öğrenimi kitaplığını kullanarak bir Lojistik regresyon modeli oluşturmak için kullanır. Bu kitaplık varsayılan olarak Azure Machine Learning Workbench ile birlikte gelir.

    -  [Pickle](https://docs.python.org/3/library/pickle.html) kitaplığını kullanarak modeli `outputs` klasöründeki dosyada seri hale getirir.

    -  Serileştirilmiş modeli yükler ve sonra bellekte tekrar seri durumdan çıkarır.

    -  Seri durumdan çıkarılmış modeli kullanarak yeni kayıtla ilgili bir tahminde bulunur.

    -  İki grafik, bir karışıklık matrisi ve bir çok sınıflı alıcı çalıştırma özellikleri (ROC) eğrisi, kullanarak çizer [matplotlib](https://matplotlib.org/) kitaplığı ve ardından bunları theoutputsfolder kaydeder. Bu kitaplık ortamında değil görünür değilse'ı yükleyin.

    -  Çalıştırma geçmişindeki düzenli hale getirme hızını ve model doğruluğunu otomatik olarak çizer. Düzenleme hızını kaydetme ve doğruluğu günlüklerde modelleme işlemi boyunca `run_logger` nesnesi kullanılır.

### <a name="run-irissklearnpy-in-the-local-environment"></a>İris_sklearn.PY dosyasını yerel ortamda çalıştırma

1.  Azure Machine Learning komut satırı arabirimini (CLI) başlatın:

    1.  Azure Machine Learning Workbench’i başlatın.

    2.  Workbench menüsünden **Dosya**> **Komut İstemini Aç**’ı seçin.

    Azure Machine Learning komut satırı arabirimi (CLI) penceresi proje klasöründe başlar `C:\Temp\\myIris\` Windows üzerinde. Bu proje bir öğreticinin 1. bölümünde oluşturduğunuz aynıdır.

    > [!Important]  
    > Sonraki adımları gerçekleştirmek için bu CLI penceresini kullanın.

1.  CLI penceresinde Python çizim kitaplığını yükleyin **matplotlib**, zaten yüklü değilse.

    **iris_sklearn.py** betiği iki Python paketine bağımlıdır: **scikit-learn** ve **matplotlib**. **Scikit-bilgi** paket kolaylık sağlamak için Azure Machine Learning Workbench tarafından yüklenir. Yine de yüklenmesini **matplotlib** gerekli olabilir.

    Yüklemeden devam ederek, **matplotlib**, bu öğreticideki kod hala başarılı bir şekilde çalıştırabilirsiniz. Ancak, kod geçmiş görselleştirmelerinde gösterilen şekilde karışıklık matrisi çıktısını ve çok sınıflı ROC eğrisi çizimlerini oluşturamaz.

    ```CLI
    pip install matplotlib
    python -m pip install --upgrade pip
    ```

    Bu yükleme yaklaşık bir dakika sürer.

1.  Workbench uygulamasına dönün.

2.  **iris_sklearn.py** adlı sekmeyi bulun.

    ![Betiği içeren sekmeyi bulun](media/azure-stack-solution-machine-learning/image43.png)

1.  Bu sekmenin araç çubuğunda, seçin **yerel** andiris_sklearn.pyas çalıştırılacak betik için yürütme ortamı olarak. Bunlar zaten seçilmiş olabilir.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image44.png)

1.  Sağ tarafına için araç ve enter0.01in **bağımsız değişkenleri** alan.

    Bu değer mantıksal regresyon modelini Kurallaştırma oranını karşılık gelir.

    ![Yerel ve betik seçeneği](media/azure-stack-solution-machine-learning/image45.png)

1.  **Çalıştır** düğmesini seçin. Anında bir iş zamanlanır. İş, workbench penceresinin sağ tarafındaki **İşler** bölmesinde listelenir.

    ![Yerel ve betik seçeneği](media/azure-stack-solution-machine-learning/image46.png)

    Birkaç dakika sonra işin durumu değişir **gönderiliyor**, **çalıştıran**ve son olarak için **tamamlandı**.

1.  **İşler** bölmesindeki iş durumu metninde **Tamamlandı** öğesini seçin.

    ![sklearn çalıştırma](media/azure-stack-solution-machine-learning/image47.png)

    Bir pencere açılır ve çalıştırmanın standart çıktı (stdout) metni görüntülenir. Stdout metnini kapatmak için seçin **kapatmak** (**x**) düğmesine sağ üst kısmındaki açılır pencere.

    ![Standart çıktı](media/azure-stack-solution-machine-learning/image48.png)

1.  Aynı iş durumunda, **işleri** bölmesinde mavi metin **iris_sklearn.py seçin \[n\] **(* n * çalıştırma numarasıdır) hemen üzerinde  **Tamamlanan** durumunun ve başlangıç zamanının. **Çalıştırma Özellikleri** penceresi açılır ve bu çalıştırma için aşağıdaki bilgileri gösterir:

    -  **Çalıştırma Özellikleri** bilgileri

    -  **Çıkışlar**

    -  **Ölçümler**

    -  Varsa **Görselleştirmeler**

    -  **Günlükler**

    Çalıştırma tamamlandığında açılır pencerede aşağıdaki sonuçlar gösterilir:

    > [!Note]  
    > Öğretici, önceden belirlenen eğitim bazı rastgele seçimler sunulan tam sonuçları burada gösterilen sonuçlardan farklı olabilir.

    ```Python  
        Python version: 3.5.2 |Continuum Analytics, Inc.| (default, Jul  5 2016, 11:41:13) [MSC v.1900 64 bit (AMD64)]

        Iris dataset shape: (150, 5)
        Regularization rate is 0.01
        LogisticRegression(C=100.0, class_weight=None, dual=False, fit_intercept=True,
            intercept_scaling=1, max_iter=100, multi_class='ovr', n_jobs=1,
            penalty='l2', random_state=None, solver='liblinear', tol=0.0001,
            verbose=0, warm_start=False)
        Accuracy is 0.6792452830188679

        ==========================================
        Serialize and deserialize using the outputs folder.

        Export the model to model.pkl
        Import the model from model.pkl
        New sample: [[3.0, 3.6, 1.3, 0.25]]
        Predicted class is ['Iris-setosa']
        Plotting confusion matrix...
        Confusion matrix in text:
        [[50  0  0]
        [ 1 37 12]
        [ 0  4 46]]
        Confusion matrix plotted.
        Plotting ROC curve....

        ROC curve plotted.
        Confusion matrix and ROC curve plotted. See them in Run History details pane.

    ```

1.  **Çalıştırma Özellikleri** sekmesini kapatın ve **iris_sklearn.py** sekmesine dönün.

1.  Diğer çalıştırmalar için tekrarlayın.

**Bağımsız Değişkenler** alanına `0.001` ile `10` arasında bir dizi değer girin. **Çalıştır**'ı seçerek kodu birkaç kez daha yürütün. Her değiştirildiği bağımsız değişken değeri her zaman içinde farklı bulguları kaynaklanan koddaki Lojistik regresyon modeline iletilir.

### <a name="review-the-run-history-in-detail"></a>Çalıştırma geçmişinin ayrıntılarını gözden geçirme

Azure Machine Learning Workbench'te her betik yürütme bir çalıştırma geçmişi kaydına dahil edilir. Açarak belirli bir betiğin çalıştırma geçmişini görüntülemek **çalıştırmaları** görünümü.

1.  **Çalıştırmalar** listesini açmak için sol araç çubuğundaki **Çalıştırmalar** düğmesini (saat simgesi) seçin. Ardından **iris_sklearn.py** gösterilecek **çalıştırma Panosu** ofiris_sklearn.py.

    ![Çalıştırma görünümü](media/azure-stack-solution-machine-learning/image49.png)

1.  **Çalıştırma Panosu** sekmesi açılır.

    Birden fazla çalıştırma sonucunda toplanan istatistikleri gözden geçirin. Graflar sekmenin en üstünde işlenir. Her çalıştırma ardışık bir numaraya sahiptir ve çalıştırma ayrıntıları ekranın altındaki tabloda listelenir.

    ![Çalıştırma panosu](media/azure-stack-solution-machine-learning/image50.png)

1.  Tabloyu filtreleyip herhangi bir grafiğe tıklayarak her bir çalıştırmanın durumunu, süresini, kesinliğini ve kurallaştırma oranını görüntüleyebilirsiniz.

2.  **Çalıştırmalar** tablosundaki iki veya daha fazla çalıştırmanın yanında bulunan onay kutularını seçin. Ayrıntılı bir karşılaştırma bölmesi açmak için **Karşılaştır** düğmesini seçin. Yan yana karşılaştırmayı gözden geçirin.

3.  **Çalıştırma Panosu**’na geri dönmek için **Karşılaştırma** bölmesinin sol üst kısmındaki **Çalıştırma Listesi** geri düğmesini seçin.

    ![Çalıştırma listesine dönme](media/azure-stack-solution-machine-learning/image51.png)

1.  Ayrıntılarını görmek için çalıştırmalardan birini seçin. Seçilen çalıştırmanın istatistikleri **Çalıştırma Özellikleri** bölümünde listelenir. Çıktı klasörüne yazılmış olan dosyalar listelenir **çıkışları** bölüm ve buradan dosyalarını indirir.

    ![Çalıştırma ayrıntıları](media/azure-stack-solution-machine-learning/image52.png)

Karışıklık matrisi ve çok sınıflı ROC eğrisi çizimleri **Görselleştirmeler** bölümünde oluşturulur. Tüm günlük dosyalarına **Günlükler** bölümünden de ulaşabilirsiniz.

### <a name="run-scripts-in-the-azure-machine-learning-ml-workbench-cli-window"></a>Betikleri Azure Machine Learning (ML) Workbench CLI penceresinde çalıştırma

1.  Azure Machine Learning komut satırı arabirimini (CLI) başlatın:

    1.  Azure Machine Learning Workbench’i başlatın.

    2.  Workbench menüsünden **Dosya** > **Komut İstemini Aç**’ı seçin.

    CLI istemi başlatır proje klasöründeki `C:\\Temp\\myIris` Windows üzerinde. Bu, öğreticinin 1. bölümünde oluşturduğunuz projedir.

    > [!Important]  
    > Sonraki adımları gerçekleştirmek için bu CLI penceresini kullanın.

1.  CLI penceresinde Azure oturumunu açın. [az login hakkında daha fazla bilgi edinin](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest).

    Zaten oturum açtıysanız bu adımı atlayın.

1.  Komut isteminde şunu girin:

    ```CLI
        az login
    ```

    Bu komut bir tarayıcıda kullanmak için bir kod döndürür [ https://aka.ms/devicelogin ](https://aka.ms/devicelogin).

1.  Git [ https://aka.ms/devicelogin ](https://aka.ms/devicelogin) tarayıcıda. CLI'yı alınan aldığınız kodu girin.

    Workbench uygulaması ve CLI bağımsız kimlik bilgisi önbellekleri Azure kaynaklarında kimlik doğrulaması yapılırken kullanın. Kimlik doğrulaması ve önbelleğe alınan belirteç süresi dolana kadar yeniden gerekli değildir.

1.  Kuruluş birden çok Azure aboneliği (kuruluş ortamı) varsa, kullanılacak aboneliği ayarlayın. Aboneliğin abonelik Kimliğini kullanarak ayarlayın ve sonra sınayın.

1.  Bu komutu kullanarak her erişilen bir Azure aboneliğini listeleyin:

    ```CLI
        az account list -o table 
    ```

    **Az hesabı listesi** komutu, oturum açma bilgilerinizle kullanılabilen Aboneliklerin listesini döndürür. Varsa birden fazla kullanılan abonelik için abonelik kimliği değerini tanımlayın.

1.  Varsayılan hesap olarak kullanılan Azure aboneliğini ayarlayın:

    ```CLI
        az account set -s <the-subscription-id
    ```

    <-Abonelik-kimliği > için kullanılan abonelik kimliği değerini olduğu. Köşeli ayraçları dahil etmeyin.

1.  Geçerli aboneliğin ayrıntılarını isteyerek yeni abonelik ayarını onaylayın.

    ```CLI
        az account show
    ```

1.  CLI penceresinde **iris_sklearn.py** betiğini bir deney olarak gönderin.

    İris_sklearn.PY betiği yerel işlem bağlamına göre yürütülür.

1.  Windows'da:

    ```CLI
        az ml experiment submit -c local .\\iris_sklearn.py
    ```

1.  Macos'ta:

    ```CLI
        az ml experiment submit -c local iris_sklearn.py
    ```

    Çıktı aşağıdakine benzer olmalıdır: 

    ```
        RunId: myIris_1521077190506

    Executing user inputs .....
    ===========================

    Python version: 3.5.2 |Continuum Analytics, Inc.| (default, Jul  2 2016, 17:52:12) 
    [GCC 4.2.1 Compatible Apple LLVM 4.2 (clang-425.0.28)]

    Iris dataset shape: (150, 5)
    Regularization rate is 0.01
    LogisticRegression(C=100.0, class_weight=None, dual=False, fit_intercept=True,
            intercept_scaling=1, max_iter=100, multi_class='ovr', n_jobs=1,
            penalty='l2', random_state=None, solver='liblinear', tol=0.0001,
            verbose=0, warm_start=False)
    Accuracy is 0.6792452830188679

    ==========================================
    Serialize and deserialize using the outputs folder.

    Export the model to model.pkl
    Import the model from model.pkl
    New sample: [[3.0, 3.6, 1.3, 0.25]]
    Predicted class is ['Iris-setosa']
    Plotting confusion matrix...
    Confusion matrix in text:
    [[50  0  0]
    [ 1 37 12]
    [ 0  4 46]]
    Confusion matrix plotted.
    Plotting ROC curve....
    ROC curve plotted.
    Confusion matrix and ROC curve plotted. See them in Run History details page.

    Execution Details
    =================
    RunId: myIris_1521077190506

    ```

6.  Çıktıyı gözden geçirin. Workbench komut dosyası çalıştığında tümü aynı çıktının ve bunun sonucunda olmalıdır.

7.  Workbench’e geri dönün ve:

    Proje dosyalarını listelemek için sol bölmedeki klasör simgesini seçin.  **run.py** adlı Python betiğini açın. Bu betik çeşitli düzenleme hızları üzerinde döngü için kullanışlıdır. 

    ![Çalıştırma listesine dönme](media/azure-stack-solution-machine-learning/image53.png)

1.  Denemeyi bu hızlarla birden çok kez çalıştırın.

    Bu komut dosyasını başlatır` aniris_sklearn.pyjob` düzenleme hızı o ile `10.0` (gerçekten büyük bir sayı). Hızı en küçük olana kadar betik ardından oranı yarı aşağıdakileri çalıştırın ve benzeri için keser `0.005`. Betik aşağıdaki kodu içerir:

    ![Çalıştırma listesine dönme](media/azure-stack-solution-machine-learning/image54.png)

1.  **run.py** betiğini komut satırından aşağıdaki gibi çalıştırın:

    ```CLI
        python run.py
    ```
Bu komut gönderir `iris_sklearn.py` birden çok kez ile farklı düzenleme hızlarıyla

Zaman `run.py` tamamlandığında, farklı ölçümlerin grafiklerini workbench'teki çalıştırma geçmişi liste görünümünde görüntülenir.

### <a name="run-scripts-in-an-ubuntu-based-data-science-virtual-machine-dsvm-on-azure"></a>Azure'da bir Ubuntu tabanlı veri bilimi sanal makinesi (DSVM içinde) betikleri çalıştırma

Betiği uzak Linux makine üzerinde bir Docker kapsayıcısında yürütmek için uzak makinede çalıştırılacak SSH erişimine (kullanıcı adı ve parola) gereklidir. Ayrıca, makinede Docker altyapısı yüklü ve çalışır durumda olmalıdır.

1.  Oluşturulan Düzenle<your DSVM Name>.runconfigfile underaml_configand varsayılan valuePySparktoPython framework değiştirin:

    ```yaml  
    Framework: Python
    ```
1.  Hedef kullanarak çalıştırdığınız komutu olarak önce CLI penceresinde*<DSVM>* bu kez iris_sklearn.py uzak bir Docker kapsayıcısında yürütmek için: (Alternatif <DSVM> ile veri bilimi VM adı köşeli ayraçları olmadan).

    ```CLI
        az ml experiment submit -c <DSVM> iris_sklearn.py
    ```

Uzak Linux VM'de yürütülmenin dışında adocker-pythonenvironment gibi komutu yürütür. CLI penceresinde aynı çıktı bilgileri görüntülenir.

### <a name="download-the-model-pickle-file"></a>Model pickle dosyasını indirme

Öğreticinin önceki bölümünde **iris_sklearn.py** betiği Machine Learning Workbench'te yerel olarak çalıştırılmıştı. Bu eylem lojistik regresyon modelini popüler Python nesne seri hale getirme paketi olan [pickle](https://docs.python.org/3/library/pickle.html) kullanarak seri hale getirmişti.

1.  Machine Learning Workbench uygulamasını açın. Açılacağını **Myıris** ve öğretici serisinin önceki bölümlerinde oluşturulan proje.

2.  Proje açıldıktan sonra seçin **dosyaları** düğmesine (klasör simgesi) sol bölmedeki proje klasöründe dosya listesini açın.

3.  **iris_sklearn.py** dosyasını seçin. Python kodu workbench içinde yeni bir metin düzenleyici sekmesinde açılır.

4.  **iris_sklearn.py** dosyasını gözden geçirerek pickle dosyasının oluşturulduğu yeri bulun. Ctrl+F kısayolunu seçerek **Bul** iletişim kutusunu açın ve sonra Python kodunda **pickle** sözcüğünü bulun.

    Bu kod parçacığı, pickle çıktı dosyasının nasıl oluşturulduğunu gösterir. Pickle çıktı dosyası diskte **model.pkl** olarak adlandırılır.

    ```Python
        print("Export the model to model.pkl")
        f = open('./outputs/model.pkl', 'wb')
        pickle.dump(clf1, f)
        f.close()

    ```

1.  Önceki bir çalıştırmanın çıktı dosyalarının arasında model pickle dosyasını bulun.

    Zaman **iris_sklearn.py** betik çalıştırın, model dosyası yazılan **çıkarır** ada sahip klasör **model.pkl**. Bu klasör yerel proje klasöründe ve betiği çalıştırmayı seçtiğiniz yürütme ortamında bulunur. 

    1. Dosyayı bulmak için seçin **çalıştırmaları** düğmesine (saat simgesi) listesini açmak için sol bölmenin **tüm çalıştırmalar**.  

    2. **Tüm Çalıştırmalar** sekmesi açılır. Çalıştırmalar tablosunda hedefin olduğu son çalıştırmalar birini **yerel** betik adının ise **iris_sklearn.py**.  

    3. **Çalıştırma Özellikleri** bölmesi açılır. Bölmenin sağ üst kısmında fark **çıkışları** bölümü. d\. Pickle dosyasını indirmek için onay kutusunun yanındaki seçin **model.pkl** dosya ve ardından **indirme**. Dosyayı proje klasöründen kök dizinine kaydedin. Sonraki adımlarda bu dosya gereklidir.  

    ![Pickle dosyasını indirme](media/azure-stack-solution-machine-learning/image55.png)

### <a name="get-scoring-script-and-schema-files"></a>Puanlama betiği ve şema dosyalarını alma

Web hizmetini model dosyasıyla birlikte dağıtmak için Puanlama betik gereklidir. İsteğe bağlı olarak, web hizmeti giriş verileri için bir şema gereklidir. Puanlama betiği geçerli klasörde yer alan **model.pkl** dosyasını yükler ve yeni tahminler üretmek için kullanır.

1.  Machine Learning Workbench uygulamasını açın. Açılacağını **Myıris** ve öğretici serisinin önceki bölümünde oluşturduğunuz proje.

2.  Proje açıldıktan sonra seçin **dosyaları** düğmesine (klasör simgesi) sol bölmedeki proje klasöründe dosya listesini açın.

3.  **score_iris.py** dosyasını seçin. Python betiği açılır. Bu dosya puanlama dosyası olarak kullanılır.

    ![Puanlama dosyası](media/azure-stack-solution-machine-learning/image56.png)

1.  Şema dosyasını almak için betiği çalıştırın. Komut çubuğundan **yerel** ortamını ve **score_iris.py** betiğini seçip **Çalıştır** seçeneğini belirleyin.

    Bu betik, bir JSON dosyası oluşturur. **çıkışları** bölümünde, model tarafından gerekli giriş verileri şemasını yakalar.

1.  **Proje Panosu** bölmesinin sağ tarafındaki **İşler** bölmesini inceleyin. En son **score_iris.py** işinin yeşil **Tamamlandı** durumunu göstermesini bekleyin. Ardından, çalıştırma ayrıntılarını görmek için en son iş çalıştırmasına ait **score_iris.py** bağlantısını seçin.

2.  **Çalıştırma Özellikleri** bölmesinin **Çıktılar** bölümünde yeni oluşturulan **service_schema.json** dosyasını seçin. Dosya adının yanındaki onay kutusunu ve sonra **İndir**’i seçin. Dosyayı projenizin kök klasörüne kaydedin.

3.  Önceki sekmeye dönün ve **score_iris.py** betiği. Veri koleksiyonu kullanarak model girişlerini ve tahminlerini web hizmetinden yakalanabilir. Aşağıdaki adımlar veri koleksiyonuyla yakından ilgilidir.

4.  Imports class dosya üstündeki kodu gözden **ModelDataCollector**. Model veri koleksiyonu işlevini içerdiği:

    ```Python  
        from azureml.datacollector import ModelDataCollector
    ```

1.  **init()** işlevindeki **ModelDataCollector** örneği oluşturan aşağıdaki kod satırlarını gözden geçirin:

    ```Python  
        global inputs_dc, prediction_dc
        inputs_dc = ModelDataCollector('model.pkl',identifier="inputs")
        prediction_dc = ModelDataCollector('model.pkl', identifier="prediction")
    ```

1.  **run(input_df)** işlevindeki giriş ve tahmin verilerini toplayan aşağıdaki kod satırlarını gözden geçirin:

    ```Python  
        inputs_dc.collect(input_df)
        prediction_dc.collect(pred)
    ```

Şimdi, modeli hazır hale getirmek için ortamı hazırlama.

## <a name="step-5-deploy-and-use-azure-container-registry"></a>5. adım: Azure Container Registry’yi dağıtma ve kullanma

Dağıtma ve Azure Container Registry kullanma.

**az acr create** komutuyla Azure Container kayıt defteri oluşturun. Kaynak defteri adı Azure’da benzersiz olmalı ve 5-50 arası alfasayısal karakter içermelidir. Kaynak grubu aynıdır.

```CLI
    az acr create --resource-group <ResourceGroup> --name  <acrName> --sku Basic
```

### <a name="container-registry-login"></a>Kapsayıcı kayıt defterinde oturum açma

ACR örneğinde oturum açmak için **az acr login** komutunu kullanın. Kapsayıcı kayıt defterine oluşturulduğunda verilen benzersiz adı sağlayın.

```CLI
    az acr login --name <acrName>
```

Komut döndürür bir ' tamamlandığında oturum açma başarılı iletisi.

### <a name="prepare-to-operationalize-locally-for-development-and-testing-the-service"></a>Yerel olarak geliştirme ve test etme hizmetinin operasyonel hale getirme

Kullanım *yerel mod* yerel bilgisayarda ve geliştirme ve test için Docker kapsayıcılarında çalıştırmak için dağıtım.

Modeli hazır hale getirmek için aşağıdaki adımları tamamlamak üzere Docker motorunun yerel ortamda çalışıyor olması gerekir. Kullanım `-h` karşılık gelen yardım iletisini görüntülemek için her bir komutun sonunda bayrağı.

> [!Note]  
> Docker altyapısının yerel olarak kullanılabilir durumda değilse, dağıtım için Azure'da bir küme oluşturarak işleme devam ve küme yeniden kullanmak için tutmak veya devam eden ücretlerden kaçınmak için öğreticiden sonra silin.

> [!Note]  
> Yerel olarak dağıtılan web Hizmetleri, Azure Portal'ın Hizmetler listesinde görünmez. Bunlar yerel makinede Docker’da çalıştırılır.

1.  Komut satırı arabirimini (CLI) açın. Machine Learning Workbench uygulamasının **Dosya** menüsünde **Komut İstemini Aç**’ı seçin.

    Komut satırı istemi geçerli proje klasörü konumu açılır **c:\\temp\\Myıris**.

1.  Azure kaynak sağlayıcısının emin **Microsoft.ContainerRegistry** aboneliğe kayıtlı. 3. adımda bir ortam oluşturmadan önce bu kaynak sağlayıcısını kaydedin. Bunu zaten aşağıdaki komutu kullanarak kayıtlı olup olmadığını denetleyin:

    ```CLI
        az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table
    ```

    Bu çıkış görüntüleyin:

    ```CLI
        Provider                                    Status 
        --------                                    --------
        Microsoft.Authorization                     Registered 
        Microsoft.ContainerRegistry                 Registered 
        microsoft.insights                          Registered 
        Microsoft.MachineLearningExperimentation    Registered 
    ```

    Varsa **Microsoft.ContainerRegistry** aşağıdaki komutu kaydedilmemiş kullanılır:

    ```CLI
    az provider register --namespace Microsoft.ContainerRegistry
    ```

    Kayıt işlemi birkaç dakika sürebilir. Denetlemek için önceki kayıt durumu **az sağlayıcı listesi** komutunu veya aşağıdaki komutu:

    ```CLI
    az provider show -n Microsoft.ContainerRegistry
    ```

    Çıktının üçüncü satırında **"registrationState": "Registering"**. Birkaç dakika bekleyin ve tekrar **Göster** çıktıyı görüntüler kadar komutu **"registrationState": "Kayıtlı.**

1.  Ortamı oluşturun. Bu adımı her ortam için bir kez çalıştırın.

    Aşağıdaki Kurulum komutu, aboneliğe katkıda bulunan erişimi gerektirir. Dağıtma kaynak grubuna katkıda bulunan erişimi gereklidir. Kaynak grubu adı, Kurulum komutunun bir parçası gflag kullanarak belirtin.

    ```CLI
    az ml env setup -n <new deployment environment name> --location <e.g. eastus2> -g <existing resource group name>
    ```

    Ekrandaki talimatları izleyerek Docker görüntülerini depolamak için bir depolama hesabı, Docker görüntülerini listeleyen bir Azure kapsayıcı kayıt defteri ve telemetri verilerini toplayan bir Azure Application Insights hesabı sağlayın. **Cswitch kullanıyorsanız, komut ayrıca bir Container Service kümesi oluşturma**.

    Küme adından ortamı tanımlayabilirsiniz bir yoludur. Konumun, Azure portalından oluşturduğunuz Model Yönetimi hesabının konumu ile aynı olması gerekir.

    Ortamın başarıyla kurulduğundan emin olmak üzere durumu denetlemek için şu komutu kullanın:

    ```CLI
    az ml env show -n <deployment environment name> -g <existing resource group name>
    ```

    5. adımda ortamı ayarlama önce gösterildiği gibi "Hazırlama durumu" "Başarılı" değeri olduğundan emin olun:

    ![Hazırlama Durumu](media/azure-stack-solution-machine-learning/image57.png)

1.  Ortamı ayarlayın.

    Kurulum tamamlandıktan sonra aşağıdaki komutu kullanarak ortamı çalışır duruma getirmek için gereken ortam değişkenlerini ayarlayın. Daha önce 3. adımda kullanılan ortam adının aynısını kullanın. Kurulum işlemi tamamlandıktan sonra komut penceresinde verilen kaynak grubu adının aynısını kullanın.

    ```CLI
        az ml env set -n <deployment environment name> -g <existing resource group name>
    ```

1.  Yerel web hizmeti dağıtımı için çalışır hale getirilen ortamı uygun yapılandırmasını doğrulamak için aşağıdaki komutu girin:

    ```CLI
    az ml env show
    ```

    Şimdi, gerçek zamanlı web hizmetini oluşturun.

    > [!Note]  
    > Model Yönetimi hesabınızı ve ortamınızı sonraki web hizmeti dağıtımları için yeniden kullanın. Her web hizmeti oluşturmaya gerek yoktur. Bir hesap veya ortamla ilişkilendirilmiş birden fazla web hizmeti olabilir.

### <a name="create-a-real-time-web-service-by-using-separate-commands"></a>Ayrı komutlar kullanarak gerçek zamanlı bir web hizmeti oluşturma

Alternatif olarak **az ml service oluşturma gerçek zamanlı** daha önce gösterildiği gibi bu adımları da gerçekleştirilebilir ayrı olarak komutu.

İlk olarak modeli kaydedin. Sonra bildirimi oluşturun, Docker görüntüsünü derleyin ve web hizmetini oluşturun. Bu adım adım yaklaşım, her adımda daha fazla esneklik sağlar. Ayrıca, önceki adımlarda oluşturulan varlıkları tekrar.

1. pickle dosyasının adını belirterek modeli kaydedin.

    ```CLI
    az ml model register --model model.pkl --name model.pkl
    ```

    Bu komut bir model kimliği oluşturur.

2.  Bir bildirim oluşturun.

    Bir bildirim oluşturmak için aşağıdaki komutu kullanın ve önceki adımdan gelen model kimliği çıktısını sağlayın: 
    
    ```CLI
    az ml manifest create --manifest-name <new manifest name> -f score_iris.py -r python -i <model ID> -s service_schema.json
    ```

    Bu komut bir bildirim kimliği oluşturur.

3.  Bir Docker görüntüsü oluşturun.

    Bir Docker görüntüsü oluşturmak için aşağıdaki komutu kullanın ve önceki adımdan gelen bildirim kimliği değeri çıktısını sağlayın. Conda bağımlılıklarını aracılığıyla da kullanılabilir `-c` geçin. 
    
    ```CLI
    az ml image create -n irisimage --manifest-id <manifest ID> -c aml_config\conda_dependencies.yml
    ```
    
    Bu komut bir Docker görüntüsü kimliği oluşturur.

2.  Hizmeti oluşturun.

    Bir hizmet oluşturmak için aşağıdaki komutu kullanın ve önceki adımdan gelen görüntü kimliği çıktısını sağlayın: 
    
    ```CLI
    az ml service create realtime --image-id <image ID> -n irisapp --collect-model-data true
    ```
    
    Bu komut bir web hizmeti kimliği oluşturur.
    
    Ardından, web hizmetini çalıştırma.

## <a name="step-6-deploy-a-kubernetes-cluster-to-azure-stack"></a>6. adım: Azure Stack için bir Kubernetes kümesi dağıtma

Azure Stack için bir Kubernetes kümesi dağıtın.

Kubernetes Azure Stack'te Azure kapsayıcı Hizmetleri (ACS) altyapısı tarafından oluşturulan Azure Resource Manager şablonlarını kullanarak yüklenebilir. [*Kubernetes* ](https://kubernetes.io/) dağıtımı otomatik hale getirmek için bir açık kaynak sistemi ölçeklendirme ve uygulamaların kapsayıcıları yönetme. A [ *kapsayıcı* ](https://www.docker.com/what-container) görüntü, bir VM ile benzerlik bulunur. Bir VM, kod, kod, belirli kitaplıkları ve ayarları yürütmek için çalışma zamanı gibi bir uygulamayı çalıştırmak için gerekli kaynaklara kapsayıcı görüntüsünü içerir.

Kubernetes için kullanın:

 -  Saniyeler içinde dağıtılabilir yüksek düzeyde ölçeklenebilir ve yükseltilebilir, uygulamaları geliştirin.

 -  Uygulama tasarımını basitleştirmek ve farklı Helm uygulamalar tarafından ve güvenilirliği geliştirmek. [*Helm* ](https://github.com/kubernetes/helm) , yükleme ve Kubernetes uygulamaların yaşam döngüsünü yönetmenize yardımcı olur açık kaynaklı paketleme aracıdır.

 -  Kolayca izleyin ve ölçek ile uygulamaların durumunu Tanıla ve işlevselliği yükseltin.

> [!Note]  
> Küme yükleme yaklaşık bir saat, lütfen plan uygun olacaktır.

### <a name="prerequisites-for-kubernetes-cluster-deployment"></a>Kubernetes Küme dağıtımı için Önkoşullar

Başlamak için izinleri ve Azure Stack hazırlık onaylayın:

1.  Doğrulamak **Kubernetes kümesi oluşturma (Önizleme)** öğesi Azure Stack Marketi'nde kullanılabilir. Bu öğe yoksa, bu öğe Azure Stack ortamına eklemek için bir Azure Stack operatörü ile çalışır.

2.  Azure Active Directory (Azure AD) kiracısı uygulamalar oluşturma olanağı doğrulayın. İzinler, Kubernetes dağıtımı için gereklidir.

    İzinleri denetleme ile ilgili yönergeler için bkz: [ *denetleyin Azure Active Directory izinlerini*](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal).

3.  Azure Stack Linux VM'de oturum açmak için SSH ortak ve özel anahtar çifti oluşturun. Ortak anahtar kümesi oluşturulurken gereklidir. Yönergeler için bkz. [SSH için bir kimlik doğrulama anahtarını oluşturmak](#generate-an-authenticatio-key-for-ssh).

4.  Azure Stack Kiracı Portalı'nda aboneliğin geçerli olduğunu ve yeterli genel IP vardır onay yeni bir uygulama eklemek kullanılabilir ele alır.

    Küme için bir Azure Stack dağıtılamıyor **yönetici** abonelik. Kullanım bir **kullanıcı** abonelik.

###  <a name="generate-an-authentication-key-for-ssh"></a>SSH için bir kimlik doğrulama anahtarı oluştur

Gelen Linux oturumu için Windows alt sistemi içinde aşağıdaki komutları bir kimlik doğrulama anahtarını oluşturmak için kullanın: 

1. Şunu yazın:

    ```Bash  
    ssh-keygen -t rsa
    ```
    
2. Seçin `id_rsa` istendiğinde. 
3. İstendiğinde, bir parola oluşturun. Daha sonra kullanmak için bu parolayı unutmayın. 
    Çıktı aşağıdakine benzer: 
    
    ```Bash  
    Your identification has been saved in `id_rsa`.
    Your public key has been saved in `id_rsa.pub`. 
    The key fingerprint is: SHA256:lUtUUjzaqWqGeolEPKeBmsnrhcNGM9Dn2OxYatt05SE  <user>@<machine-name>
    The key's randomart image is:  
    ```
    ![Alternatif metin](media/azure-stack-solution-machine-learning/image58.png)

4. Anahtarı oluşturduktan sonra aşağıdaki komutları kullanarak anahtar bilgisini yapıştırın: 
    ```Bash
    cat id_rsa.pub
    ```
    Çıktı aşağıdakine benzer: 
    ```Bash
    ssh-rsa  AAAAB3NzaC1yc2EAAAADAQABAAABAQDHaWrAktR3BlQ356T8Qvv8O2Q/U/hsXQwS9xJbuduuc9lkJwddzgmNCyM9PooZWYtGVXyHU6SC3YH1XkwqGtKhtPb03d24hmhX1euAaqIqHHSp4WgUS5s1gNsp37L8QCSGNCnF31FWavI8bnjOjccUkMowKl8iyGN++5UyQgNuynEVUbFJjrntoDL66HUu88xDxTcVB7rqDr2QKFwYJkg4HSoHYpezJd7W8kcmv33eh0xs8nlDA7Dzu7zXpyVc54bH9XtPR6EUXaQa+UqKaNlQNiJqEs+1X/zNuK9V6NLVNiO0h3jCHI3K8o4VnZyRKHCQProasiiPLUUJ6SF/RTLN  <user>@<machine-name>
    ```
    
5. Oluşturulan anahtar SSH ortak anahtarı alanına kopyalayın.

### <a name="create-a-service-principal-in-azure-ad"></a>Azure AD'de hizmet sorumlusu oluşturma

1.  Oturum açmak için genel [ *Azure portalında*](http://portal.azure.com/).

2.  Azure Stack örneği ile ilişkili Azure AD kiracısı kullanarak oturum açın.

3.  Azure AD uygulaması oluşturun.

    1. Seçin **Azure Active Directory** > **+ uygulama kayıtları**> **yeni uygulama kaydı**.
    2. Girin bir **adı** uygulama. 
    3. Seçin **Web uygulaması / API**. 
    4. Girin `http://localhost` için **oturum açma URL'si**. 
    5. **Oluştur**’u seçin.

1.  **Uygulama Kimliği**’ni not alın. Kümeyi oluştururken bu kimliği gereklidir ve başvurulan **hizmet sorumlusu istemci kimliği**.

2.  Seçin **ayarları** > **anahtarları**.

    1. Girin **açıklama**. 
    2. Seçin **her zaman geçerli olsun** için **Expires**. 
    3. **Kaydet**’i seçin. Anahtar dize not edin. Anahtar dize kümesi oluşturulurken gereklidir ve başvurulan **hizmet sorumlusu istemci parolası**.

### <a name="give-the-service-principal-access"></a>Hizmet sorumlusu erişimi verin

Kaynakları oluşturabilir, böylece aboneliği için hizmet sorumlusu erişimi verin.

1.  Oturum [Yönetim Portalı](https://adminportal.local.azurestack.external/).

2.  Seçin **diğer hizmetler** > ** Kullanıcı aboneliklerini ** > **+ Ekle**.

3.  Oluşturduğunuz aboneliği seçin.

4.  Seçin **erişim denetimi (IAM)** > seçin **+ Ekle**.

5.  Seçin **sahibi** rol.

6.  İçin hizmet sorumlusu oluşturulan uygulama adı seçin. Ad, gerekirse arama kutusuna yazın.

7.  **Kaydet**’i seçin.

8.  Açık [Azure Stack portalı](https://portal.local.azurestack.external).

9.  Seçin **+ yeni** > **işlem** > **Kubernetes kümesi**. **Oluştur**’u seçin.

    ![Çözüm Şablonu Dağıt](media/azure-stack-solution-machine-learning/image59.png)

10. Seçin **Temelleri** , Kubernetes kümesi oluşturun.

    ![Çözüm Şablonu Dağıt](media/azure-stack-solution-machine-learning/image60.png)

11. Girin **Linux VM yönetici kullanıcı adı**. Kubernetes kümesinin parçası olan bir Linux sanal makineleri ve DVM için kullanıcı adı.

12. Girin **SSH ortak anahtarı** DVM ve Kubernetes kümesinin bir parçası olarak oluşturulan tüm Linux makinelerinin yetkilendirme için kullanılır.

13. Girin **Yöneticisi profili, DNS ön eki** bölgeye benzersiz. Bu, bir bölgede benzersiz olmalıdır adı, örneğin `ask8s-12345`. Seçmek deneyin aynı kaynak grubu adı en iyi yöntem.

    > [!Note]  
    > Her küme için yeni ve benzersiz ana profili DNS ön ekini kullanın.

14. Girin **aracı havuzu profili sayısı**. Sayıyı kümedeki aracı sayısını içerir. 4'e 1'den olabilir.

15. Girin **hizmet sorumlusu ClientID** bu Kubernetes Azure bulut sağlayıcısı tarafından kullanılır.

16. Girin **hizmet sorumlusu istemci parolası** hizmet sorumlusu uygulama oluştururken oluşturulur.

17. Girin **Kubernetes Azure bulut sağlayıcısı sürümü**. Kubernetes Azure sağlayıcısı sürümüdür. Azure Stack, her Azure Stack sürümü için özel bir Kubernetes yapı serbest bırakır.

18. Seçin **abonelik** kimliği

19. Yeni bir kaynak grubu adını girin veya mevcut bir kaynak grubunu seçin. Kaynak adı alfasayısal ve küçük harf olması gerekir.

20. Seçin **konumu** kaynak grubu. Bu, Azure Stack yükleme için seçilen bölgedir.

### <a name="specify-the-azure-stack-settings"></a>Azure Stack ayarlarını belirtin

1.  Seçin **Azure Stack damga ayarları**.

    ![Çözüm Şablonu Dağıt](media/azure-stack-solution-machine-learning/image61.png)

2.  Girin **Kiracı Azure Resource Manager uç noktasını**. Bu, bir Kubernetes kümesi için kaynak grubu oluşturmak için bağlanmak için Azure Resource Manager uç noktadır. Uç noktasından Azure Stack operatörü için tümleşik bir sistem gereklidir. Azure Stack geliştirme Seti'ni (ASDK için), kullanın `https://management.local.azurestack.external`.

3.  Girin **Kiracı kimliği** Kiracı. Bkz: [ *Kiracı kimliği alma* ](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal) Kiracı kimliği değeri bulunacak.

### <a name="install-kubectl-on-windows-powershell-environment"></a>Windows PowerShell ortamında kubectl yükleyin

WSL ortamında WSL ortamında kubectl yüklemek için aşağıdaki komutları çalıştırın.

```PowerShell  
Install-script -name install-kubectl -scope CurrentUser -force
Install-kubectl.ps1 -downloadlocation "C:\Users\<Current User>\Documents\Kube"
```

### <a name="install-kubectl-on-the-windows-subsystem-for-linux-environment"></a>Kubectl Linux ortamı için Windows alt sisteminde yükleyin.

WSL ortamında WSL ortamında kubectl yüklemek için aşağıdaki komutları çalıştırın.

```Bash  
    apt-get update && apt-get install -y apt-transport-https
    curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add
    sudo apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"
    apt-get update
    apt-get install -y kubectl
```

### <a name="configure-kubectl"></a>Kubectl yapılandırma

Bulmak ve bir Kubernetes kümesine erişmek kubectl sırada bir*kubeconfig'i denetleyin dosya* gereklidir. Bir küme kube-up.sh kullanarak veya Minikube Küme dağıtımı oluşturulduğunda otomatik olarak oluşturulur. Bkz: [ *Başlarken kılavuzları* ](https://kubernetes.io/docs/setup/) kümeleri oluşturma hakkında daha fazla bilgi için. Başka bir kullanıcı tarafından oluşturulan bir kümeye erişmek için bkz: [ *paylaşımı kümeye erişim belge*](https://kubernetes.io/docs/tasks/access-application-cluster/configure-access-multiple-clusters/). Varsayılan olarak, kubectl yapılandırma konumundaki **~.kube/config**.

### <a name="check-the-kubectl-configuration"></a>Kubectl yapılandırmasını denetleyin

Küme durumunu alarak, kubectl düzgün yapılandırıldığını kontrol edin:

```Bash  
kubectl cluster-info
```

kubectl yanıtı URL'sini görüntülendiğinde kümeye erişmek için doğru yapılandırılmamış.

kubectl doğru yapılandırılmış veya aşağıdaki ileti görüntülenirse, bir Kubernetes kümesine bağlanmak mümkün değildir:

```Bash  
The connection to the server <server-name:port> was refused -  did you specify the right host or port?
```

Örneğin, bir Kubernetes kümesi yerel bir dizüstü bilgisayarda çalışırken, bir aracı gerekebilir (minikube veya benzer) yukarıda belirtilen komutları yeniden çalıştırılacak.

Kubectl küme-info döndürürse yanıtı URL'sini küme değildir ancak yine de uygun yapılandırma için erişilebilir, onay kullanarak:

```Bash  
    kubectl cluster-info dump
```

### <a name="enable-shell-autocompletion"></a>Kabuk otomatik tamamlama etkinleştir

kubectl Kabuk etkinleştirme hızlı ve kolay hale otomatik tamamlama desteği bulunur.

Tamamlama betik kubectl tarafından oluşturulur ve profili erişilebilir.

### <a name="connect-to-the-kubernetes-cluster"></a>Kubernetes kümesine bağlanma

Kubernetes'in komut satırı istemcisini kümeye bağlanmak için (**kubectl**) gereklidir. Bağlanma ve kümeyi yönetmek için yönergeleri bulunur [Azure Container Service belgeleri.](https://docs.microsoft.com/azure/container-service/dcos-swarm/)

Hiçbir SSH istemcisini Kubernetes kümesine bağlanmak için kullanılabilir. Windows alt sistemi için Linux (WSL) önerilir. Kubernetes kümesine bağlanma makine küme için oluşturulan kaynak grubunda olacaktır ve edge ml yığın vmd ön eki ile başlar.

```Bash  
ssh <user>@vmd-edge-ml-stack.<FQDN of Kubernetes Machine in  Azure Stack>
```

Kubernetes makineye bağlandıktan sonra Kubernetes yapılandırma dosyası almak için şu makineyi çalıştırın.

```Bash  
sudo find / -name \*kubeconfig\* -type f
```

Çıktı görünür aşağıdakine benzer:

```Bash  
/var/lib/waagent/custom-script/download/0/acs-engine/_output/edgemlstack/kubeconfig/kubeconfig.<regionname>.json
```

Bu dosyaları yol bilgisi kopyalayın ve ardından yukarıdaki kopyalanan kubeconfig'i denetleyin dosya yolunda yapıştırarak aşağıdaki komutu çalıştırın:

```Bash  
sudo cat  /var/lib/waagent/custom-script/download/0/acs-engine/_output/edgemlstack/kubeconfig/kubeconfig.<regionname>.json
```

Çıktı, büyük bir ham JSON içeriği olan bir metin bloğu olacaktır. Bu çıkış metni kopyalayın ve bir JSON dosyası olarak kaydederek bu kod bir Visual Studio dosyasına yapıştırın. Bu, yerel olarak saklanan kubeconfig.json dosyasında sonuçlanır. (/ mnt/c/Kullanıcı/Kaydet<current user>kubeconfig.json olarak Kube dizin/belgeler /)

### <a name="configure-the-kubernetes-cluster"></a>Kubernetes kümesini yapılandırın

Yeni bir WSL oturumunda, yerel bir JSON dosyası alındığında kümesini yapılandırmak için aşağıdaki komutları kullanın.

```Bash  
    cd /mnt/c/users/<current user>/documents/Kube
    kubectl proxy
    kubectl create -f https://raw.githubusercontent.com/kubernetes/dashboard/master/src/deploy/recommended/kubernetes-dashboard.yaml
    kubectl proxy
    set KUBECONFIG="/mnt/c/users/<current user>/documents/Kube/kubeconfig.json"
    kubectl.exe config view
```

Kubernetes yapılandırma ayarlarını (çıktıyı aşağıya bakın) tanımlanır.

![Alternatif metin](media/azure-stack-solution-machine-learning/image62.png)

Yerel proxy hizmetini başlatın:

```Bash  
kubectl proxy
```

Şu adreste kubernetes küme kullanıcı arabirimini göz atın: `https://localhost:8001`.

![Alternatif metin](media/azure-stack-solution-machine-learning/image63.png)

Artık kapsayıcı ve şirket içi görebilirsiniz bulutta yer alan bir kapsayıcı dağıtmak için bir yer var.

![Alternatif metin](media/azure-stack-solution-machine-learning/image64.png)

Özelleştirme **iris_deployment.yaml** dosyası (bulunan /*c/mnt/kullanıcı/<current user>/belgeler/Kube dizin*) şekilde **webservicename** ve kapsayıcıları  **Görüntü** ve **adı** tercih ettiğiniz herhangi bir kod Düzenleyicisi'ni kullanarak dağıtım eşleşmesi.

![Alternatif metin](media/azure-stack-solution-machine-learning/image65.png)

Kapsayıcı bağlantı noktası olarak **5001.**

![Alternatif metin](media/azure-stack-solution-machine-learning/image66.png)

Ve ardından oluşturun **imagePullSecret**:

### <a name="create-a-secret-in-the-cluster-that-holds-the-authorization-token"></a>Yetkilendirme belirteci tutan kümedeki bir gizli dizi oluşturma

Bir Kubernetes kümesi gizli anahtarını kullanır **docker-registry** özel bir görüntüyü çekin kapsayıcı kayıt defteri ile kimlik doğrulaması türü.

Adlandırma, bu gizli dizi oluşturma **azuremlcr**:

```Bash  
kubectl create secret docker-registry azuremlcr --docker-server=<your-registry-server> --docker-username=<your-name> --docker-password= "<your-pword>" --docker-email=<your-email>
```

Bulun:

- **< your-registry-server zaman >** Azure kapsayıcı kayıt defteri **oturum açma sunucusu**.
- **< uygulamanızın-adı >** Azure kapsayıcı kayıt defteri **Username**.
- **< pword bilgisayarınızı >** Azure kapsayıcı kayıt defteri **parola**. Tırnak işaretleri paroladır emin olun.
- **< bilgisayarınızı e-posta >** kapsayıcıya okuma/yazma erişimi olan kullanıcı.
- Bu bilgiler bulabilirsiniz **Azure Container** **kayıt defteri** altında **erişim anahtarlarını**.
- Gizli dizi olarak docker kimlik bilgileri kümeye artık ayarlanmış **azuremlcr**.

Kaydet **iris_deployment.yaml** dosyası (bulunan /*c/mnt/kullanıcı/<current user>/belgeler/Kube dizin*).

### <a name="create-the-kubernetes-container"></a>Kubernetes kapsayıcı oluşturma

```Bash  
kubectl.exe create -f /mnt/c/users/<current  user>/documents/Kube/iris_deployment.yaml
```

![Alternatif metin](media/azure-stack-solution-machine-learning/image67.png)

Dağıtım durumunu kontrol edin:

```Bash  
Kubectl get deployments
```

![Alternatif metin](media/azure-stack-solution-machine-learning/image68.png)

Dağıtım biraz zaman alabilir.

### <a name="configure-azure-devops-to-deploy-automatically"></a>Azure DevOps, otomatik olarak dağıtmak için yapılandırma

#### <a name="create-a-team-project"></a>Bir takım projesi oluşturma

1.  Olun [proje koleksiyonu yöneticileri grubunun üyeliği.](https://docs.microsoft.com/vsts/organizations/security/set-project-collection-level-permissions?view=vsts) Takım projeleri oluşturmak için **yeni projeler oluştur** izni ayarlanmalıdır **izin**.

2.  Proje sayfasından seçin **yeni proje**.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image69.png)

1.  Projeyi adlandırın **HybridMLIris**.

2.  İlk kaynak denetimi türü olarak seçin **Git**

3.  Bir işlem seçip seçin **Oluştur**.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image70.png)

### <a name="import-some-code--create-repository"></a>Bazı kod oluşturma depo içeri aktarma

Bir Git deposu için YAML kod gereklidir.

#### <a name="add-user-to-the-git-repo"></a>GIT deposu için kullanıcı ekleme

1.  Varsayılan proje panodan oluşturmak Git kimlik bilgileri seçin.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image71.png)

1.  Git kimlik bilgileri kaydetmek gerekli olması ve parolayı girin.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image72.png)

1.  Deposu seçerek başlatın **başlatmak** düğmesi ve oluşturma bir **Benioku** dosya.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image73.png)

#### <a name="clone-the-git-repository-locally-and-upload-the-code"></a>Yerel Git deposunu kopyalayın ve kodu yükleyin. 

1.  Makinenin içinde bir dizin oluşturun `c:\\users\\<User>\\source\\repos\\hybridMLIris`ve depoyu kopyalama

    ```Bash  
    sudo mkdir /mnt/c/users/<User>/source sudo mkdir /mnt/c/users/<User>/source/repos sudo mkdir /mnt/c/users/<User>/source/repos/hybridMLIris cd /mnt/c/users/<User>/source/repos/hybridMLIris sudo git clone  https://<yourvstssite>.visualstudio.com/HybridMLIris/_git/HybridMLIris
    ```

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image74.png)

1.  Yeni kopyalanan depoya gidin:

    ```Bash  
    ls
    cd ./HybridMLIris
    ```
    
    ![Alternatif metin](media/azure-stack-solution-machine-learning/image75.png)

1.  Kopyalama **iris_deployment.yaml** depoya dosya.

    ```Bash  
    cp /mnt/c/users/<User>/documents/Kube/iris_deployment.yaml  /mnt/c/users/<User>/source/repos/HybridMLIris/HybridMLIris/iris_deployment.yaml
    ``` 

1.  Git'te değişikliği işleyin

    ```Bash  
    git add . git commit -m Added Deployment YAML git push
    ```

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image76.png)

### <a name="prepare-the-private-build-and-release-agent-for-vsts-integration"></a>Özel derleme ve yayın Aracısı VSTS tümleştirmesi için hazırlama

#### <a name="prerequisites"></a>Önkoşullar

VSTS, bir hizmet sorumlusu kullanarak Azure Resource Manager karşı doğrular. Azure Stack aboneliğine kaynaklarında sağlayabilmesi VSTS, katılımcı durumu gerektirir. \ **gibi kimlik doğrulamasını etkinleştirmek için yapılandırılması gereken üst düzey adımlar şunlardır:**

1.  Hizmet sorumlusu oluşturulacak veya mevcut bir kullanılabilir.

2.  Kimlik doğrulaması anahtarları için hizmet sorumlusu oluşturulması gerekir.

3.  Azure Stack aboneliğine katkıda bulunan rolünün bir parçası olması SPN izin vermek için rol tabanlı erişim denetimi doğrulanması gerekir.

4.  SPN bilgilerin yanı sıra Azure Stack uç noktaları kullanarak yeni bir hizmet tanımı vsts'de oluşturulması gerekir.

#### <a name="service-principal-creation"></a>Hizmet sorumlusu oluşturma

Başvurmak [hizmet sorumlusu oluşturma yönergeleri](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications) hizmet sorumlusu oluşturma ve uygulama türü için Web uygulaması/API'ı seçin.

**Erişim anahtarı oluşturma**

Bir hizmet sorumlusu kimlik doğrulaması için bir anahtar gerektiriyor, bir anahtar oluşturmak için bu bölümdeki adımları izleyin.

1.  Gelen **uygulama kayıtları** uygulamayı Azure Active Directory'de'ı seçin.

    ![uygulama seçme](media/azure-stack-solution-machine-learning/image77.png)

1.  Değerini not edin **uygulama kimliği Hizmet uç noktası VSTS'de yapılandırma değeri kullanılır.**

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image78.png)

1.  Kimlik doğrulama anahtarını oluşturmak için **Ayarlar**'ı seçin.

    ![ayarları seçme](media/azure-stack-solution-machine-learning/image79.png)

1.  **Anahtarlar**’ı seçin.

    ![anahtarları seçme](media/azure-stack-solution-machine-learning/image80.png)

1.  Anahtar için bir açıklama ve süre sağlayın. İşiniz bittiğinde **Kaydet**’i seçin.

    ![anahtarı kaydetme](media/azure-stack-solution-machine-learning/image81.png)

Anahtar kaydedildikten sonra, anahtarın değeri görüntülenir. Daha sonra gerektiğinde bu değeri kopyalayın. **Anahtar değerini** uygulamayla kimliği, uygulama olarak oturum açmak için gereklidir. Uygulama, alabildiği anahtar değeri Store.

![Alternatif metin](media/azure-stack-solution-machine-learning/image82.png)

#### <a name="get-tenant-id"></a>Kiracı Kimliğinizi alma

Hizmet uç noktası yapılandırmasının bir parçası olarak, VSTS gerektirir **Kiracı kimliği** karşılık gelen Azure Stack damga dağıtılmış bir AAD dizini. Kiracı kimliği toplamak için bu bölümdeki adımları izleyin

1.  **Azure Active Directory**'yi seçin.

    ![azure active directory'yi seçme](media/azure-stack-solution-machine-learning/image83.png)

1.  Kiracı Kimliğini almak için seçin **özellikleri** Azure AD kiracısı için.

    ![Azure AD özelliklerini seçme](media/azure-stack-solution-machine-learning/image84.png)

1.  **Dizin kimliği**'ni kopyalayın. Bu değer Kiracı kimliğidir.

    ![kiracı kimliği](media/azure-stack-solution-machine-learning/image85.png)

Azure Stack aboneliğine kaynakları dağıtmak için hizmet sorumlusu haklar

Abonelikte bulunan kaynaklara erişmek için uygulamanızı bir role atayın. Uygulama için doğru izinlere rolünü karar verin. Kullanılabilir roller hakkında bilgi edinmek için [RBAC: Yerleşik roller](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles).

Abonelik, kaynak grubu veya kaynak düzeyinde kapsamı ayarlayın. Daha düşük düzeyde kapsam için izinler devralınmıştır. Örneğin, okuyucu rolünün bir kaynak grubu için bir uygulama eklendiğinde, kaynak grubunu ve içerdiği tüm kaynakları okumak sağlar.

1.  Kapsamın uygulamayı atamak istediğiniz düzeye gidin. Örneğin abonelik kapsamında bir rol atamak için seçin **abonelikleri**.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image86.jpeg)

1.  Seçin **abonelik** (kaynak grubu veya kaynak) uygulama atayın.

    ![Abonelik atama için seçin](media/azure-stack-solution-machine-learning/image87.png)

1.  Seçin **erişim denetimi (IAM)**.

    ![erişim seçin](media/azure-stack-solution-machine-learning/image88.png)

1.  **Add (Ekle)** seçeneğini belirleyin.

    ![Ekle'yi seçin](media/azure-stack-solution-machine-learning/image89.png)

1.  Uygulama atamak için rolü seçin. Aşağıdaki görüntüde **sahibi** rol.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image90.png)

1.  Varsayılan olarak, Azure Active Directory uygulamaları kullanılabilir seçenekleri görüntülenmiyor. Uygulamayı bulmak için **adı sağlayın** arama alanını ve bu seçeneği belirleyin.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image91.png)

1.  Seçin **Kaydet** rol atama tamamlanması. Uygulama, ilgili kapsam için bir role atanan kullanıcılar listesinde görüntülenir.

### <a name="role-based-access-control"></a>Rol Tabanlı Access Control

Azure rol tabanlı erişim denetimi (RBAC), Azure ve Azure Stack için ayrıntılı erişim yönetimi sağlar. RBAC kullanarak, yalnızca kullanıcıların işlerini yapmak için gereken erişim miktarını verilebilir. Rol tabanlı erişim denetimi hakkında daha fazla bilgi için bkz. [Azure abonelik kaynaklarına erişimi yönetme](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal?toc=%252fazure%252factive-directory%252ftoc.json).

**VSTS aracı havuzları**

Her bir aracı tek tek yönetmek yerine aracıları düzenlenmiştir **aracı havuzları**. Bir aracı havuzu paylaşım sınırı, havuzdaki tüm aracılar için tanımlar. VSTS'de, aracı havuzları VSTS hesabına takım projeleri arasında paylaşılan şekilde belirlenir. Aracı havuzları ve bir öğretici VSTS oluşturma hakkında daha fazla bilgi için bkz [aracı havuzları oluşturma ve sıralar.](https://docs.microsoft.com/vsts/build-release/concepts/agents/pools-queues?view=vsts)

**Azure Stack için aPersonal erişim belirteci (PAT) Ekle**

 -  VSTS hesabı ve Seç oturum **hesap profili** adı.

 -  Seçin **Güvenliği Yönet** için erişim belirteci oluşturma sayfası.

![Alternatif metin](media/azure-stack-solution-machine-learning/image92.png)

![Alternatif metin](media/azure-stack-solution-machine-learning/image93.jpeg)

![Alternatif metin](media/azure-stack-solution-machine-learning/image94.jpeg)

> [!Note]  
> Belirteç bilgileri edinin. Bunu yeniden bu ekranı bırakarak sonra gösterilmez.

1.  Kopyalama **belirteci**.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image95.png)

#### <a name="install-the-vsts-build-agent-on-the-azure-stack-hosted-build-server"></a>Azure Stack'te VSTS derleme aracısı yükleme barındırılan derleme sunucusu

1.  Bağlanma **derleme sunucusu** Azure Stack konakta dağıtılmış.

    > [!Note]  
    > Azure Stack barındırılan yapı sunucusunun genel Internet'ten erişilebilir olduğundan emin olun. Aksi durumda, Azure Stack erişim elde etmek için operatör ile çalışır.

    ```Bash  
    ssh <user>@<buildserver.publicip>
    ```

2.  Derleme Ubuntu sunucusu (18.04) en son sürüme yükseltme:

    ```Bash  
    sudo su
    apt-get update
    apt-get upgrade
    apt-get dist-upgrade
    apt-get autoremove
    do-release-upgrade -d
    ```

    > [!Note]  
    > Bu işlem biraz zaman alabilir.

2.  Önkoşul uygulamalar için yapı sunucusuna yükleyin. Bash Kabuk Ubuntu, aşağıdaki komutları derleme sunucusu çalıştırma tabanlı:

    ```Bash  
    wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.asc.gpg
    sudo mv microsoft.asc.gpg /etc/apt/trusted.gpg.d/
    wget -q https://packages.microsoft.com/config/ubuntu/18.04/prod.list 
    sudo mv prod.list /etc/apt/sources.list.d/microsoft-prod.list
    sudo chown root:root /etc/apt/trusted.gpg.d/microsoft.asc.gpg
    sudo chown root:root /etc/apt/sources.list.d/microsoft-prod.list
    sudo apt-get install apt-transport-https
    sudo apt-get update
    sudo apt-get install liblttng-ust0 libcurl3 libssl1.0.0 curl lsb-release libkrb5-3 zlib1g libicu60 aspnetcore-runtime-2.1 dotnet-sdk-2.1
    wget https://github.com/PowerShell/PowerShell/releases/download/v6.1.0-preview.3/powershell-preview_6.1.0-preview.3-1.ubuntu.18.04_amd64.deb
    sudo dpkg -i powershell-preview_6.1.0-preview.3-1.ubuntu.18.04_amd64.deb
    sudo apt-get install -f
    AZ_REPO=$(lsb_release -cs)
    echo "deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ $AZ_REPO main" | \
        sudo tee /etc/apt/sources.list.d/azure-cli.list
    curl -L https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add –
    sudo apt-get update && sudo apt-get install azure-cli
    ```

2.  Karşıdan yükleyip kullanarak bir hizmet olarak yapı aracısını dağıtma bir **kişisel erişim belirteci (PAT)** ve VM Yöneticisi farklı çalıştır hesabı.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image96.png)

    ```Bash  
        cd \home\<user>
        sudo mkdir myagent && cd myagent
        wget https://vstsagentpackage.azureedge.net/agent/2.134.2/vsts-agent-linux-x64-2.134.2.tar.gz
        sudo tar zxvf vsts-agent-linux-x64-2.134.2.tar.gz
    ```

2.  Ayıklanan derleme aracısı klasörüne gidin. Aşağıdaki kodu çalıştırın.

    ```Bash  
        cd ..
        sudo chmod -R 777 myagent
        cd myagent
        ./config.sh
    ```

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image97.png)

2.  Sonra **./config.sh**bittiğinde, çalışma aşağıdaki sunucu önyükleme hizmetini etkinleştirmek için kod ve hizmeti başlatın:

    ```Bash  
    sudo ./svc.sh install
    sudo ./svc.sh start
    ```

Aracı, artık VSTS klasöründe görülebilir.

#### <a name="endpoint-creation-permissions"></a>Uç nokta oluşturma izinleri

VSTO yapılar yığına Azure hizmet uygulamaları dağıtabilirsiniz böylece kullanıcılar uç noktaları oluşturabilirsiniz. Ardından, Azure Stack ile bağlanır yapı aracısı VSTS bağlanır.

![Alternatif metin](media/azure-stack-solution-machine-learning/image98.png)

1.  Üzerinde **ayarları** menüsünde **güvenlik**.

2.  İçinde **VSTS grupları** listesi seçin sol taraftaki **uç noktasını oluşturanlar**.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image99.png)

3.  Üzerinde **üyeler sekmesinin** seçin **+ Ekle**.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image100.png)

1.  Tür **kullanıcıadı** ve listeden bir kullanıcı adı seçin.

2.  Seçin **değişiklikleri kaydetmek**.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image101.png)

3.  İçinde **VSTS grupları** listesi seçin sol taraftaki **uç nokta yöneticileri**.

4.  Üzerinde **üyeler sekmesinin** seçin **+ Ekle**.

5.  Tür a **kullanıcı adı** ve listeden kullanıcı seçin.

6.  Seçin **değişiklikleri kaydedin.**

    ![buchatech](media/azure-stack-solution-machine-learning/image102.jpeg)

    Azure stack'teki yapı aracısı, sonra Azure Stack ile iletişim için uç nokta bilgileri ileten VSTS, gelen yönergeleri kazanır.

    Azure Stack bağlantısı için VSTS artık hazırdır.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image103.png)

### <a name="configure-build-and-release-definitions"></a>Derleme ve yayın tanımlarını yapılandırma

Bağlantı kurulur, el ile oluşturulan Azure uç noktası, AKS ve Azure Container Registry'ye derleme ve yayın tanımları.

#### <a name="create-the-build-definition-for-the-yaml-code"></a>YAML koduna için derleme tanımı oluşturma

1.  Derleme ve yayınlama hub yapılar bölümünde seçin ve yeni bir tanımı oluşturun.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image104.png)

1.  VSTS Gıt'i seçin ve daha önce oluşturduğunuz depoyu seçin.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image105.png)

1.  Şablon olarak boş bir işlem hattı seçin

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image106.png)

1.  Derleme adı **kopyalama Yapıt** ve Azure Stack yapı sunucu için aracı kuyruğu seçin.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image107.png)

1.  İşlemde Aşama 1'i seçin ve yeniden adlandırın **kopyalama Yapıt**, ardından **görev ekleme** aşaması için:

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image108.png)

1.  Seçin **derleme Yapıtları yayımlama** gelen **yardımcı programı** listesinde ve seçin **Ekle**.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image109.png)

1.  Seçin **yayımlama yolu** seçip **iris_deployment.yaml** dosya.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image110.png)

1.  Yapıt adı **iris_deployment** olmasını yayımlama konumu seçip **Azure işlem hatları**.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image111.png)

1.  Seçin **Kaydet ve kuyruğa**.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image112.png)

1.  Derleme kimliği seçerek derleme durumunu denetleyin

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image113.png)

Başarı aşağıdakine benzer görünecektir:

![Alternatif metin](media/azure-stack-solution-machine-learning/image114.png)

#### <a name="create-the-release-definition-for-the-yaml-code"></a>YAML koduna için sürüm tanımı oluşturun

1.  Sürümler bölümü altında derleme ve yayınlama hub yeni bir tanımı seçin

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image115.png)

1.  Boş bir işlem hattı, bir şablon olarak seçin.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image106.png)

1.  Ad ortamını Azure Stack.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image116.png)

1.  Yeni Yapıt seçerek ekleyin **Yapıtları** ve **+ Ekle**

2.  Derleme kaynak türü seçin ve **HybridMLIris** projesi olarak.

3.  Ardından kaynağı daha önce oluşturduğunuz derleme tanımı seçin.

4.  Ardından **ekleme**.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image117.png)

1.  Azure Stack ortamları seçin ve ardından yeni bir görev Azure Stack'e ekleme

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image118.png)


1.  Aracı aşaması üzerinde Azure Stack barındırılan yapı sunucuda aracı kuyruğu ayarlayın.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image119.png)

1.  Yeni bir görev eklemek için bu aşama, Kubernetes dağıtma görevi için Dağıt'ı seçin ve Ekle'yi seçin.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image120.png)


1.  Adlandırın **Kubectl uygulamak** (varsayılan adı) ve Uygula komutu seçin.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image121.png)

    Şimdi yeni bir Kubernetes hizmet bağlantısı oluşturun.

#### <a name="create-kubernetes-service-endpoint"></a>Kubernetes hizmet uç noktası oluşturma

1.  Kubernetes hizmeti bağlantısı altında seçin **+ yeni** düğmesini ve**Kubernetes**listeden. Bu uç noktaya bağlanmak için kullanabileceğiniz**VSTS**ve**Azure Container Service (AKS)**.

2.  **Bağlantı adı**: Bağlantı adı belirtin.

3.  **Sunucu URL'si**: Kapsayıcı hizmeti adresini formathttp belirtin: / / {API sunucusu adresi}

4.  **Kubeconfig'i denetleyin**: Kubeconfig'i denetleyin değeri almak için yönetici ayrıcalığı ile başlatılan bir komut isteminde aşağıdaki Azure komutları çalıştırın.

    > [!Important]  
    > Sonraki adımları gerçekleştirmek için bu CLI penceresini kullanın.

6.  CLI penceresinde Azure oturumunu açın. [az login hakkında daha fazla bilgi edinin](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest).

7.  Komut isteminde şunu girin:

    ```CLI
        az login
    ```

10. Bu komut bir tarayıcıda kullanmak için bir kod döndürür <https://aka.ms/devicelogin>.

11. Git <https://aka.ms/devicelogin> tarayıcıda. Sorulduğunda CLI'dan tarayıcıya alınan kod girin.

    ![Kubernetes hizmet uç noktası](media/azure-stack-solution-machine-learning/image122.png)

1.  Erişim kimlik bilgilerini almak için Kubernetes kümesi için komut isteminde aşağıdaki komutu yazın.

### <a name="azure-ml-workbench-cli"></a>Azure ML Workbench CLI

az aks get-credentials resource-group <yourResourceGroup> adı <yourazurecontainerservice>

![Kubernetes hizmet uç noktası](media/azure-stack-solution-machine-learning/image123.png)

1.  Gidin **.kube**giriş dizini altında klasör (örn: C:\\kullanıcılar\\<user>\\belgeleri\\Kube)

2.  İçeriğini kopyalayın**config**dosyası ve Kubernetes bağlantısı penceresine yapıştırın. Seçin**Tamam**düğmesi.

    ![Kubernetes hizmet uç noktası](media/azure-stack-solution-machine-learning/image124.png)
    

3.  Kubernetes uç noktası oluşturup seçilen bir yapılandırma dosyası eklemek için kullanım yapılandırma dosyaları onay kutusunu seçin. Ardından bağlı Yapıtlar iris_deployment.yaml dosyasına göz atın.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image125.png)

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image126.png)

4.  Yayın tanımını kaydedin.

#### <a name="check-the-status-of-the-release-definition"></a>Yayın tanımı durumunu denetleyin. 

VSTS yayın tanımı kaydedildikten sonra otomatik olarak bir yayın kazandırın.

Hızlı komut WSL komut isteminde çalışmasını ve Kubernetes UI denetimi dağıtım durumunu denetleyin.

```Bash  
kubectl get deployments
```

Çıkış dağıtma sürecinde, şuna benzer olmalıdır.

![Alternatif metin](media/azure-stack-solution-machine-learning/image127.png)

```Bash  
kubectl proxy
```

Kubernetes UI çalıştırıldıktan sonra dağıtımı daha göz atın [ **https://localhost:8001/** ](https://localhost:8001/) gidin **iş yükleri -> çoğaltma kümeleri**.

![Alternatif metin](media/azure-stack-solution-machine-learning/image128.png)

### <a name="deploy-the-yaml-service"></a>YAML hizmetini dağıtma

#### <a name="upload-the-irisserviceyaml-to-the-repository-and-sync-changes"></a>Depoyu ve eşitleme değişiklikleri iris_service.yaml karşıya yükleme

1.  Yeni kopyalanan depoya gidin:

    ```Bash  
    cd /mnt/c/users/<User>/source/repos/HybridMLIris/HybridMLIris/
    ```

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image75.png)

1.  Kopyalama **iris_service.yaml** depoya dosya.

    ```Bash  
    cp /mnt/c/users/<User>/documents/Kube/iris_service.yaml  /mnt/c/users/<User>/source/repos/HybridMLIris/HybridMLIris/iris_service.yaml
    ```

1.  Git'te değişikliği işleyin

    ```Bash  
    git add .
    git commit -m "Added Service YAML" 
    git push
    ```

![Alternatif metin](media/azure-stack-solution-machine-learning/image129.png)

#### <a name="update-the-build-definition-for-the-yaml-code"></a>YAML koduna derleme tanımını güncelleştirin

1.  Derleme ve yayınlama hub yapılar bölümünde seçin ve daha önce oluşturduğunuz tanımı seçin.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image130.png)

2.  Tanımını düzenlemek için Düzenle düğmesini seçin.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image131.png)

3.  **Bir görev ekleyin** aşamasına. Seçin **derleme Yapıtları yayımlama** gelen **yardımcı programı** listesinde ve seçin **Ekle**.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image108.png)

4.  Adlandırın **Kubectl uygulamak** (varsayılan adı) ve Uygula komutu seçin.



#### <a name="update-the-release-definition-for-the-yaml-code"></a>YAML koduna yayın tanımını güncelleştirin

1.  Derleme ve yayınlama hub theReleases bölümünde seçin ve daha önce oluşturduğunuz yayın tanımı seçin. Sonra Düzenle bağlantısını seçin.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image132.png)

1.  Ortamı seçin **Azure Stack** sonra Azure Stack için yeni bir görev ekleyin.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image133.png)

1.  Ekle bir **yeni görev** Bu aşama için seçin **kubernetes'e dağıtma** altında görev **Dağıt** seçip **Ekle**.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image134.png)

1.  Adlandırın **Kubectl uygulamak** (varsayılan adı) ve Uygula komutu seçin.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image109.png)

1.  Azure Stack daha önce oluşturduğunuz bağlantı Kubernates hizmet bağlantı ayarlayın ve ardından **yapılandırma dosyalarını kullanın** bir yapılandırma dosyası eklemek için onay kutusu. Bağlı Yapıtlar iris_service.yaml dosyasına göz atın.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image135.png)
    <!-- -->
    ![alternatif metin](media/azure-stack-solution-machine-learning/image136.png)

1.  Yayın tanımını kaydedin.

#### <a name="check-the-status-of-the-release-definition"></a>Yayın tanımı durumunu denetleyin

VSTS yayın tanımı kaydedildikten sonra otomatik olarak bir yayın kazandırın.

Hızlı komut WSL komut isteminde çalışmasını ve Kubernetes UI denetimi dağıtım durumunu denetleyin.

```Bash  
kubectl get deployments
```

Çıkış dağıtma sürecinde, şuna benzer olmalıdır.

![Alternatif metin](media/azure-stack-solution-machine-learning/image127.png)


```Bash  
kubectl proxy
```

Kubernetes UI çalıştırıldıktan sonra dağıtımı daha göz atın [ **https://localhost:8001/** ](https://localhost:8001/) gidin **iş yükleri -> çoğaltma kümeleri**.

![Alternatif metin](media/azure-stack-solution-machine-learning/image137.png)


### <a name="kubernetes-scoring-and-validation"></a>Kubernetes Puanlama ve doğrulama

Kubernetes kullanıcı arabirimini Başlat

```Bash  
kubectl proxy
```

Kubernetes kullanıcı Arabirimine Gözat sonra Git **dağıtımları** -> **Iris-dağıtım** -> **yeni çoğaltma kümesi**  ->  **Iris dağıtım xxxxxxxxx** (xs olduğu dağıtım kimliği).

![Alternatif metin](media/azure-stack-solution-machine-learning/image138.png)

Ardından gidin **Hizmetleri** seçip **dış uç noktası** çalıştığını doğrulamak için hizmet.

![Alternatif metin](media/azure-stack-solution-machine-learning/image139.png)

Aşağıdakine benzer bir doğrulama iletisi görüntülenmelidir:

![Alternatif metin](media/azure-stack-solution-machine-learning/image140.png)

#### <a name="create-azure-stack-scoring-function-app-in-the-azure-stack-portal"></a>Azure Stack portalında işlev uygulaması Puanlama Azure yığını oluşturma

Her bir işlevin yürütülmesini barındıran bir işlev uygulaması gerekir. Bir işlev uygulaması, daha kolay yönetimi, dağıtım ve kaynakların paylaşımı için bir mantıksal birim olarak gruplandırma işlevine izin verir.

1.  Azure Stack Kullanıcı Portalı'ndan seçin **+ yeni** düğmesi sol üst köşesinde bulunan üzerinde ardından **Web + mobil** >**işlev uygulaması**.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image141.png)

1.  İşlev adı **veri işlevleri** ve kalan Machine Learning ile aynı kaynak grubunda içerik yerleştirin. Tüketim için yeni bir app service planı otomatik olarak oluşturma ve uygulama depolaması için daha önce oluşturduğunuz depolama hesabını kullanırsınız araç sağlar.

    ![Yeni işlev uygulaması ayarlarını tanımlama](media/azure-stack-solution-machine-learning/image142.png)

1.  Seçin **Oluştur**işlev uygulamasını sağlamak ve dağıtmak için.

2.  Portalın sağ üst köşesindeki Bildirim simgesini seçin ve **Dağıtım başarılı** iletisini bekleyin.

    ![Yeni işlev uygulaması ayarlarını tanımlama](media/azure-stack-solution-machine-learning/image143.png)

1.  Seçin **kaynağa Git** yeni işlev uygulaması görüntülemek için.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image144.png)

1.  Seçerek yeni bir işlev oluşturma **işlevleri**, ardından **+ yeni işlev** düğmesi.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image145.png)

1.  HTTP tetikleyicisini seçin

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image146.png)

1.  Seçin **C\#**  dil ve işlev adı: **temiz-puanı-data**, yetki düzeyi ayarlanmış ve **anonim**.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image147.png)

1.  Kopyala-yapıştır örnek içeriğini temizlemek puanı-verisi işlevdeki kod.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image148.png)

#### <a name="use-postman-to-validate-functions"></a>İşlevleri doğrulamak için Postman'ı kullanma

Kbernetes ve işlevleri ayarlama emin olmak için doğru ücretsiz Postman uygulamasını test etmek ve şemaları ve işlevlerini doğrulamak için kullanabilirsiniz. Bu işlemi başlatmak için önce bazı bilgiler toplayın, Kubernetes örneğinden gerekir.

1.  Kubernetes kullanıcı Arabirimine Gözat sonra Git **dağıtımları** -> **Iris-dağıtım** -> **yeni çoğaltma kümesi**  ->  **Iris dağıtım xxxxxxxxx** (xs olduğu dağıtım kimliği)

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image138.png)

1.  Ardından gidin **Hizmetleri** ve kopyalama **dış uç noktası**.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image149.png)

1.  Postman uygulamasını indirip [burada](https://www.getpostman.com/apps) gerekirse.

2.  Postman uygulamasında oturum açın ve yeni dosya iletişim kutusunu kapatın.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image150.png)

1.  Postman uygulamasında POST seçin...

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image151.png)

1.  Yapıştırma **dış uç noktası** altında postman uygulamasını URL'sini **istek URL** ekleme  **\\puanı** aşağıda gösterildiği gibi URL'nin sonuna.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image152.png)

1.  Seçin **gövdesi** sekmesini ve ardından veri türü olarak **ham**, ardından **JSON**.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image153.png)

1.  Bir web tarayıcısından gidin **dış uç noktası**. Aşağıdaki URL'ye ekleniyor **/swagger.json** Bu kurulum test etmek için kullanılan hizmetleri Swagger dosyası yol açar.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image154.png)

1.  Listelenen örneği kopyalayın **Swagger.JSON** dosya.

2.  Postman uygulamasında örneğe gönderinin gövdesine yapıştırın ve seçin **Gönder**. Aşağıda gösterilene benzer bir değer döndürmelidir.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image155.png)

## <a name="step-7-create-an-azure-stack-storage-account-and-storage-queue"></a>7. adım: Bir Azure Stack depolama hesabını ve depolama kuyruğu oluşturun

Verileri bir Azure Stack depolama hesabını ve depolama kuyruğu oluşturun.

1.  Azure Stack kullanıcı portalında oturum açın. (Her Azure Stack sahip benzersiz bir portal URL)

2.  Azure Stack Kullanıcı Portalı'nda hizmet menüsünü açın ve sol taraftaki menüyü genişleterek **tüm hizmetleri**. Ekranı aşağı kaydırarak **depolama** ve **depolama hesapları**. İçinde **depolama hesapları** pencere **Ekle**.

3.  Depolama hesabı için bir ad girin.

4.  Depolama hesabı için çoğaltma seçeneğini seçin: **LRS**.

5.  Yeni bir kaynak grubu belirtin veya varolan bir kaynak grubunu seçin.

6.  Seçin **yerel** depolama hesabı için konum.

7.  Seçin **Oluştur**depolama hesabı oluşturmak için.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image156.png)

1.  Son oluşturulan depolama hesabını seçin.

2.  SELECT deyiminde **kuyrukları**.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image157.png)

1.  SELECT deyiminde **+ kuyruk** ve seçin ve kuyruk adı **Tamam.**

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image158.png)

1.  Alma **bağlantı dizesi** depolama kuyruğu için ve kopyalayın.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image159.png)

1.  Azure işlev uygulaması'na gidin ve ardından **uygulama ayarları**.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image160.png)

1.  İşlev uygulaması, uygulama ayarları içinde uygulama ayarları aşağı kaydırın, seçerek ve **+ yeni ayar Ekle**.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image161.png)

1.  Depolama hesabının adını girin **adı** sonuna ekleme alanı `_STORAGE`.

Bu, bir depolama hesabı uç noktası olduğunu anlamak için uygulamayı sağlar.

1.  Bağlantı dizesi içine yapıştırın **değer** alan.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image162.png)

1.  Uygulama ayarları üstüne kadar kaydırın ve **Kaydet**.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image163.png)

### <a name="update-the-scoring-function-to-use-storage-queue"></a>Depolama kuyruğu kullanmak için Puanlama işlevi

1.  SELECT deyiminde **tümleştirme** işlevde ve seçimini **alma** seçeneği.

2.  **Kaydet**’i seçin.

3.  Ardından **+ yeni çıkış** çıkışları öğesinden.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image164.png)

1.  Ardından **Azure kuyruk depolama** ve **seçin**.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image165.png)

1.  Güncelleştirme **kuyruk adı** daha önce oluşturduğunuz ve ardından bir depolama kuyruğuna **depolama hesabı bağlantısı** seçin ve daha önce oluşturduğunuz depolama hesabı bağlantısı için **kaydedin.**

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image166.png)

## <a name="step-8-create-a-function-to-handle-clean-data"></a>8. adım: Temiz verileri işlemek için bir işlev oluşturma

Verileri temizleme Azure yığını, Azure'a taşımak için yeni bir Azure Stack işlevi oluşturun.

1.  Seçerek yeni bir işlev oluşturma **işlevleri**, ardından **+ yeni işlev** düğmesi.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image167.png)

1.  Seçin **Zamanlayıcı tetikleyicisi**.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image168.png)

1.  Seçin **C\#**  dil ve işlev adı: **karşıya yükleme azure** ve zamanlama ayarlamak **0 0 \*/1 \* \* \***  hangi CRON saatte bir gösterimidir.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image169.png)

### <a name="get-the-connection-string-to-the-azure-hosted-storage-account"></a>Barındırılan Azure depolama hesabı bağlantı dizesini alın

1.  Gözat <https://portal.azure.com> seçip **Azure depolama hesabı** daha önce oluşturduğunuz.

2.  Seçin **erişim anahtarları**, ardından kopyalama **bağlantı dizesi** depolama hesabı için.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image170.png)

### <a name="update-the-upload-to-azure-function-to-use-the-azure-hosted-storage"></a>Karşıya yükleme azure'a işlevi barındırılan Azure depolama kullanmak için güncelleştirme

1.  Azure işlev uygulaması'na gidin ve ardından **uygulama ayarları**.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image171.png)

1.  İşlev uygulaması, uygulama ayarları içinde uygulama ayarları aşağı kaydırın, seçerek ve **+ yeni ayar Ekle**.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image172.png)

1.  Depolama hesabının adını girin **adı** sonuna ekleme alan; _depolama

Bu, bir depolama hesabı uç noktası olduğunu anlamak için uygulamayı sağlar.

1.  Azure barındırılan depolama hesabı bağlantı dizesi içine yapıştırın **değer** alan.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image173.png)

1.  Uygulama ayarları üstüne kadar kaydırın ve **Kaydet**.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image174.png)

1.  Geri gidin **karşıya yükleme azure** işlevi.

2.  SELECT deyiminde **tümleştirme** işlev üzerinde.

3.  Ardından **+ yeni çıkış** çıkışları öğesinden.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image175.png)

1.  Ardından **Azure Blob Depolama** ve **seçin**.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image176.png)

1.  Güncelleştirme **yolu** şu biçimde daha önce oluşturduğunuz depolama kapsayıcısına: **uploadeddata / {guid rand} .txt**ve ardından **depolama hesabı bağlantısı** için Azure depolama hesabı bağlantısı seçin ve daha önce oluşturulan **kaydedin.**

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image177.png)

1.  Kopyala-yapıştır örnek içeriğini kod için **karşıya yükleme azure** işleve.

2.  Depolama hesabı bağlantı dizesi işaret edecek şekilde gerektiği gibi değiştirin.

3.  Kaydet ve kodu çalıştırın.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image178.png)

1.  Verileri Azure buluta ayrıştırılmış görmek için barındırılan Azure depolama hesabı denetleyin: Başarı benzer şekilde görünür aşağıda.

    ![Alternatif metin](media/azure-stack-solution-machine-learning/image179.png)

Veri hassas verilerin Azure Stack barındırılan Kubernetes Machine Learning tarafından ayıklanır ve şirket içi Azure Stack, Azure Stack barındırılan işlev uygulamaları, aracılığıyla kaynaklardan Azure genel buluta yüklenen ve bir edge/bağlantısız yüklemeler için verileri hazırlamak Senaryo.

## <a name="next-steps"></a>Sonraki adımlar

 - Azure bulut desenleri hakkında daha fazla bilgi için bkz: [bulut tasarımı desenleri](https://docs.microsoft.com/azure/architecture/patterns).
