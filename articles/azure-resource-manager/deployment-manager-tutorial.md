---
title: Azure Deployment Manager’ı Resource Manager şablonlarıyla kullanma | Microsoft Docs
description: Azure kaynaklarını dağıtmak için Azure Deployment Manager ile Resource Manager şablonlarını kullanın.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 04/02/2019
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: a4f14a1e68042704ca8e8c49f1bd76b722c90d4d
ms.sourcegitcommit: e6d53649bfb37d01335b6bcfb9de88ac50af23bd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65466293"
---
# <a name="tutorial-use-azure-deployment-manager-with-resource-manager-templates-public-preview"></a>Öğretici: Resource Manager şablonları (genel Önizleme) ile Azure Deployment Manager'ı kullanın

[Azure Deployment Manager](./deployment-manager-overview.md)’ı kullanarak uygulamalarınızı birden çok bölgede nasıl dağıtacağınızı öğrenin. Deployment Manager'ı kullanmak için iki şablonu oluşturmak gerekir:

* **Topoloji şablonu**: Uygulamalarınızı Azure kaynaklarını ve bunların dağıtılacağı yeri açıklar.
* **Dağıtım şablonu**: Uygulamalarınızı dağıtırken uygulanacak adımları açıklar.

> [!IMPORTANT]
> Kanarya yeni Azure özellikleri test etmek için aboneliğinizi işaretlenmişse Kanarya bölgeye dağıtma yalnızca Azure Dağıtım Yöneticisi'ni kullanabilirsiniz. 

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Senaryoyu anlama
> * Öğretici dosyalarını indirme
> * Yapıtları hazırlama
> * Kullanıcı tanımlı yönetilen kimliği oluşturma
> * Hizmet topolojisi şablonunu oluşturma
> * Piyasaya çıkarma şablonunu oluşturma
> * Şablonları dağıtma
> * Dağıtımı doğrulama
> * Yeni sürümü dağıtma
> * Kaynakları temizleme

Azure Deployment Manager REST API Başvurusu bulunabilir [burada](https://docs.microsoft.com/rest/api/deploymentmanager/).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu makaleyi tamamlamak için gerekenler:

* [Azure Resource Manager şablonlarını](./resource-group-overview.md) geliştirme konusunda deneyim.
* Azure Dağıtım Yöneticisi özel önizleme aşamasındadır. Azure Deployment Manager'ı kullanarak kaydolmak için [kayıt sayfasını](https://aka.ms/admsignup) doldurun. 
* Azure PowerShell. Daha fazla bilgi için bkz. [Azure PowerShell kullanmaya başlayın](https://docs.microsoft.com/powershell/azure/get-started-azureps).
* Deployment Manager cmdlet'leri. Bu yayın öncesi cmdlet’leri yüklemek için PowerShellGet’in en son sürümü gereklidir. En son sürümü edinmek için bkz. [PowerShellGet’i Yükleme](/powershell/gallery/installing-psget). PowerShellGet’i yükledikten sonra PowerShell penceresini kapatın. Yeni yükseltilmiş bir PowerShell penceresi açın ve aşağıdaki komutu kullanın:

    ```powershell
    Install-Module -Name Az.DeploymentManager
    ```

* [Microsoft Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/). Azure Depolama Gezgini gerekli değildir, ancak işinizi kolaylaştırır.

## <a name="understand-the-scenario"></a>Senaryoyu anlama

Hizmet topolojisi şablonu, hizmetinizi oluşturan Azure kaynaklarını ve bunların dağıtılacağı yeri açıklar. Hizmet topolojisi tanımı şu hiyerarşiye sahiptir:

* Hizmet topolojisi
  * Hizmetler
    * Hizmet birimleri

Aşağıdaki diyagramda, bu öğreticide kullanılan hizmet topolojisi gösterilmektedir:

![Azure Deployment Manager öğreticisi senaryo diyagramı](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-scenario-diagram.png)

Batı ABD ve doğu ABD konumlarında ayrılmış iki hizmet vardır.  Her hizmetin iki hizmet birimi vardır: web uygulaması ön ucu ve arka uç için depolama hesabı. Hizmet birimi tanımları, web uygulamaları ve depolama hesapları oluşturmaya yönelik şablon ve parametre dosyalarının bağlantıları içerir.

## <a name="download-the-tutorial-files"></a>Öğretici dosyalarını indirme

1. Bu öğreticide kullanılan [şablonları ve yapıtları](https://armtutorials.blob.core.windows.net/admtutorial/ADMTutorial.zip) indirin.
2. Dosyaları bilgisayarınızdaki konuma ayıklayın.

Kök klasörde iki klasör vardır:

* **ADMTemplates**: burada, aşağıdakileri içeren Deployment Manager şablonları yer alır:
  * CreateADMServiceTopology.json
  * CreateADMServiceTopology.Parameters.json
  * CreateADMRollout.json
  * CreateADMRollout.Parameters.json
* **ArtifactStore**: hem şablon yapıtlarını hem de ikili dosya yapıtlarını içerir. [Yapıtları hazırlama](#prepare-the-artifacts) bölümüne bakın.

İki şablon kümesi olduğuna dikkat edin.  Kümelerin biri, hizmet topolojisini ve piyasaya çıkarmayı dağıtmak için kullanılan Deployment Manager şablonlarıdır. Diğer küme ise web hizmetlerini ve depolama hesaplarını oluşturmak için hizmetten çağrılır.

## <a name="prepare-the-artifacts"></a>Yapıtları hazırlama

İndirilen ArtifactStore klasörü iki klasör içerir:

![Azure Deployment Manager öğreticisi yapıt kaynağı diyagramı](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-artifact-source-diagram.png)

* **Şablonlar** klasörü: şablon yapıtlarını içerir. **1.0.0.0** ve **1.0.0.1** ikili dosya yapıtlarının iki sürümünü temsil eder. Her sürümde her bir hizmete yönelik bir klasör vardır (Hizmet Doğu ABD ve Hizmet Batı ABD). Her hizmette depolama hesabı oluşturmak için bir şablon çifti ve web uygulaması oluşturmak için başka bir şablon çifti vardır. Web uygulaması şablonu, web uygulaması dosyalarını içeren sıkıştırılmış bir paket çağırır. Sıkıştırılmış dosya, ikili dosyalar klasöründe depolanan bir ikili dosya yapıtıdır.
* **İkili dosyalar** klasörü: ikili dosya yapıtları içerir. **1.0.0.0** ve **1.0.0.1** ikili dosya yapıtlarının iki sürümünü temsil eder. Her sürümde, web uygulamasını Batı ABD konumunda oluşturmaya yönelik bir zip dosyası ve web uygulamasını Doğu ABD konumunda oluşturmaya yönelik başka bir zip dosyası vardır.

İki sürüm (1.0.0.0 ve 1.0.0.1) [sürüm dağıtımı](#deploy-the-revision) içindir. Şablon yapıtları ve ikili dosya yapıtlarının iki sürümü olsa da, iki sürüm arasında yalnızca ikili dosya yapıtları farklıdır. Gerçekte, ikili dosya yapıtları şablon yapıtlarına göre daha sık güncelleştirilir.

1. **\ArtifactStore\templates\1.0.0.0\ServiceWUS\CreateStorageAccount.json** dosyasını bir metin düzenleyicisinde açın. Depolama hesabı oluşturmaya yönelik temel bir şablondur.  
2. **\ArtifactStore\templates\1.0.0.0\ServiceWUS\CreateWebApplication.json** dosyasını açın. 

    ![Azure Deployment Manager öğreticisi web uygulaması şablonu oluşturma](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-create-web-application-packageuri.png)

    Şablon, web uygulamasının dosyalarını içeren bir dağıtım paketi çağırır. Bu öğreticide, sıkıştırılmış paket, yalnızca bir index.html dosyası içerir.
3. **\ArtifactStore\templates\1.0.0.0\ServiceWUS\CreateWebApplicationParameters.json** dosyasını açın. 

    ![Azure Deployment Manager öğreticisi web uygulaması şablonu parametreleri containerRoot](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-create-web-application-parameters-deploypackageuri.png)

    deployPackageUri parametresinin değeri dağıtım paketinin yoludur. Parametre bir **$containerRoot** değişkeni içerir. $containerRoot değeri, [piyasaya çıkarma şablonunda](#create-the-rollout-template) yapıt kaynağı SAS konumu, yapıt kökü ve deployPackageUri birleştirilerek sağlanır.
4. **\ArtifactStore\binaries\1.0.0.0\helloWorldWebAppWUS.zip\index.html** dosyasını açın.  

    ```html
    <html>
      <head>
        <title>Azure Deployment Manager tutorial</title>
      </head>
      <body>
        <p>Hello world from west U.S.!</p>
        <p>Version 1.0.0.0</p>
      </body>
    </html>
    ```

    Html dosyasında konum ve sürüm bilgileri gösterilir. 1.0.0.1 klasöründeki ikili dosyada "Sürüm 1.0.0.1" gösterilir. Hizmeti dağıttıktan sonra bu sayfalara göz atabilirsiniz.
5. Diğer yapıt dosyalarını inceleyin. Senaryoyu daha iyi anlamanıza yardımcı olur.

Şablon yapıtları hizmet topolojisi şablonu tarafından kullanılır ve ikili dosya yapıtları piyasaya çıkarma şablonu tarafından kullanılır. Topoloji şablonu ve piyasaya çıkarma şablonu bir yapıt kaynağı Azure kaynağı tanımlar. Bu kaynak, Resource Manager’ı dağıtımda kullanılan şablona ve ikili dosya yapıtlarına yönlendirmek için kullanılır. Öğreticiyi basitleştirmek amacıyla, hem şablon yapıtları hem de ikili dosya yapıtlarını depolamak için bir depolama hesabı kullanılmıştır. Her iki yapıt kaynağı aynı depolama hesabını işaret eder.

1. Bir Azure Storage hesabı oluşturun. Yönergeler için bkz. [hızlı başlangıç: Karşıya yükleme, indirme ve Azure portalını kullanarak blobları listeleme](../storage/blobs/storage-quickstart-blobs-portal.md).
2. Depolama hesabında bir blob kapsayıcısı oluşturun.
3. İki klasör (ikili dosyalar ve şablonlar) ile iki klasörün içeriğini blob kapsayıcısına kopyalayın. [Microsoft Azure Depolama Gezgini](https://go.microsoft.com/fwlink/?LinkId=708343&clcid=0x409) sürükleyip bırakma özelliğini destekler.
4. Aşağıdaki yönergeleri kullanarak kapsayıcının SAS konumunu alın:

    1. Azure Depolama Gezgini'nden blob kapsayıcısına gidin.
    2. Sol bölmede blob kapsayıcısına sağ tıklayın ve **Paylaşılan Erişim İmzasını Al** öğesini seçin.
    3. **Başlangıç zamanını** ve **Geçerlilik sonu zamanı** yapılandırın.
    4. **Oluştur**’u seçin.
    5. URL’nin kopyasını oluşturun. Bu URL, iki parametre dosyasındaki ([topoloji parametre dosyası](#topology-parameters-file) ve [piyasaya çıkarma parametre dosyası](#rollout-parameters-file)) bir alanı doldurmak için gereklidir.

## <a name="create-the-user-assigned-managed-identity"></a>Kullanıcı tarafından atanmış yönetilen kimliği oluşturma

Öğreticinin sonraki bölümlerinde bir piyasaya çıkarma dağıtacaksınız. Dağıtım eylemlerini gerçekleştirmek (örneğin web uygulamalarını ve depolama hesabını dağıtma) için kullanıcı tarafından atanmış bir yönetilen kimlik gereklidir. Bu kimliğe, hizmeti dağıttığınız Azure aboneliğine erişim izni verilmelidir ve kimliğin yapıt dağıtımını tamamlamak için yeterli izni olmalıdır.

Kullanıcı tarafından atanmış yönetilen bir kimlik oluşturmanız ve aboneliğiniz için erişim denetimini yapılandırmanız gerekir.

> [!IMPORTANT]
> Kullanıcı tarafından atanmış yönetilen kimlik, [piyasaya çıkarma](#create-the-rollout-template) ile aynı konumda olmalıdır. Şu anda piyasaya çıkarma gibi Deployment Manager kaynakları yalnızca Orta ABD veya Doğu ABD 2’de oluşturulabilir. Ancak, bu yalnızca Deployment Manager Kaynakları (örneğin, service topolojisi, hizmetleri, hizmet birimi, dağıtım ve adımları) için geçerlidir. Hedef kaynaklarınız için desteklenen tüm Azure bölgelerine dağıtılabilir. Bu öğreticide, örneğin, Orta ABD için dağıtılan Deployment Manager kaynakları, ancak Hizmetleri, Doğu ABD ve Batı ABD için dağıtılır. Bu kısıtlama, gelecekte yükseltilmiş.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. [Kullanıcı tarafından atanmış bir yönetilen kimlik](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md) oluşturun.
3. Portalda sol menüden **Abonelikler**’i ve ardından aboneliğinizi seçin.
4. Seçin **erişim denetimi (IAM)** ve ardından **rol ataması Ekle**.
5. Aşağıdaki değerleri yazın veya seçin:

    ![Azure Deployment Manager öğreticisi kullanıcı tarafından atanmış yönetilen kimlik erişim denetimi](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-access-control.png)

    * **Rol**: yapıt dağıtımını (web uygulamaları ve depolama hesapları) tamamlamak için yeterli izinleri verin. Bu öğreticideki **Katkıda Bulunanı** seçin. Gerçekte, izinleri asgari seviyeyle sınırlamak istersiniz.
    * **Erişimi atama hedefi**: **Kullanıcı Tarafından Atanmış Yönetilen Kimlik** öğesini seçin.
    * Öğreticide daha önce oluşturduğunuz kullanıcı tarafından atanmış yönetilen kimliği seçin.
6. **Kaydet**’i seçin.

## <a name="create-the-service-topology-template"></a>Hizmet topolojisi şablonunu oluşturma

**\ADMTemplates\CreateADMServiceTopology.json** dosyasını açın.

### <a name="the-parameters"></a>Parametreler

Şablon aşağıdaki parametreleri içerir:

![Azure Deployment Manager öğreticisi topoloji şablonu parametreleri](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-topology-template-parameters.png)

* **namePrefix**: Bu ön ek, Deployment Manager kaynakların adlarını oluşturmak için kullanılır. Örneğin, "jdoe" ön eki kullanıldığında hizmet topolojisi adı **jdoe**ServiceTopology olur.  Kaynak adları bu şablonun değişkenler bölümünde tanımlanır.
* **azureResourcelocation**: Öğreticiyi basitleştirmek için aksi belirtilmediği sürece bu konuma tüm kaynakları paylaşır. Şu anda Azure Deployment Manager kaynakları yalnızca **Orta ABD** veya **Doğu ABD 2**’de oluşturulabilir.
* **artifactSourceSASLocation**: Dağıtım için hizmeti birim şablon ve parametreleri dosyalarının depolandığı Blob kapsayıcısı SAS URI'si.  [Yapıtları hazırlama](#prepare-the-artifacts) bölümüne bakın.
* **templateArtifactRoot**: Şablon ve parametre depolandığı Blob kapsayıcısından uzaklık yolu. Varsayılan değer: **templates/1.0.0.0**. [Yapıtları hazırlama](#prepare-the-artifacts) bölümünde açıklanan klasör yapısını değiştirmek istemiyorsanız bu değeri değiştirmeyin. Bu öğreticide göreli yollar kullanılır.  Tam yol **artifactSourceSASLocation**, **templateArtifactRoot** ve **templateArtifactSourceRelativePath** (veya **parametersArtifactSourceRelativePath**) birleştirilerek oluşturulur.
* **Targetsubscriptionıd**: Deployment Manager kaynakları dağıtılır ve faturalandırılır için önerilere şu abonelik kimliği. Bu öğreticide abonelik kimliğinizi kullanın.

### <a name="the-variables"></a>Değişkenler

Değişkenler bölümünde kaynakların Azure konumları iki hizmeti için adları tanımlar: **Hizmet WUS** ve **hizmet EUS**ve yapıt yolları:

![Azure Deployment Manager öğreticisi topoloji şablonu değişkenleri](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-topology-template-variables.png)

Yapıt yollarını depolama hesabına yüklediğiniz klasör yapısıyla karşılaştırın. Yapıt yollarının göreli yollar olduğuna dikkat edin. Tam yol **artifactSourceSASLocation**, **templateArtifactRoot** ve **templateArtifactSourceRelativePath** (veya **parametersArtifactSourceRelativePath**) birleştirilerek oluşturulur.

### <a name="the-resources"></a>Kaynaklar

Kök düzeyde tanımlı iki kaynak vardır: *yapıt kaynağı* ve *hizmet topolojisi*.

Yapıt kaynağı tanımı:

![Azure Deployment Manager öğreticisi topoloji şablonu kaynakları yapıt kaynağı](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-topology-template-resources-artifact-source.png)

Aşağıdaki ekran görüntüsünde hizmet topolojisinin, hizmetlerin ve hizmet birimi tanımlarının yalnızca bazı bölümleri gösterilmektedir:

![Azure Deployment Manager öğreticisi topoloji şablonu kaynakları hizmet topolojisi](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-topology-template-resources-service-topology.png)

* **artifactSourceId**, yapıt kaynağını hizmet topolojisi kaynağıyla ilişkilendirmek için kullanılır.
* **dependsOn**: Tüm service topolojisi kaynakları yapıt kaynağı kaynağına bağımlı.
* **artifacts** şablon yapıtlarını işaret eder.  Burada göreli yollar kullanılır. Tam yol artifactSourceSASLocation (yapıt kaynağında tanımlıdır), artifactRoot (yapıt kaynağında tanımlıdır) ve templateArtifactSourceRelativePath (veya parametersArtifactSourceRelativePath) birleştirilerek oluşturulur.

### <a name="topology-parameters-file"></a>Topoloji parametre dosyası

Topoloji şablonuyla kullanılan bir parametre dosyası oluşturursunuz.

1. **\ADMTemplates\CreateADMServiceTopology.Parameters** öğesini Visual Studio Code’da veya herhangi bir metin düzenleyicisinde açın.
2. Parametre değerlerini doldurun:

    * **namePrefix**: 4-5 karakter içeren bir dize girin. Bu ön ek, benzersiz Azure kaynağı adları oluşturmak için kullanılır.
    * **azureResourceLocation**: Azure konumları ile ilgili bilgi sahibi değilseniz kullanın **centralus** bu öğreticideki.
    * **artifactSourceSASLocation**: SAS URI'sini kök dizinine (Blob kapsayıcısı), dağıtım için hizmeti birim şablon ve parametreleri dosyalarının depolandığı yeri girin.  [Yapıtları hazırlama](#prepare-the-artifacts) bölümüne bakın.
    * **templateArtifactRoot**: Yapıtlar klasör yapısını değiştirmediğiniz sürece, kullanın **templates/1.0.0.0** bu öğreticideki.
    * **targetScriptionID**: Azure abonelik kimliğinizi girin

> [!IMPORTANT]
> Topoloji şablonu ve piyasaya çıkarma şablonu bazı ortak parametreleri paylaşır. Bu parametreler aynı değerlere sahip olmalıdır. Bu parametreler: **namePrefix**, **azureResourceLocation**, ve **artifactSourceSASLocation** (her iki yapıt kaynağı da bu öğreticide aynı depolama hesabını kullanır).

## <a name="create-the-rollout-template"></a>Piyasaya çıkarma şablonunu oluşturma

**\ADMTemplates\CreateADMRollout.json** dosyasını açın.

### <a name="the-parameters"></a>Parametreler

Şablon aşağıdaki parametreleri içerir:

![Azure Deployment Manager öğreticisi piyasaya çıkarma şablonu parametreleri](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-rollout-template-parameters.png)

* **namePrefix**: Bu ön ek, Deployment Manager kaynakların adlarını oluşturmak için kullanılır. Örneğin, "jdoe" ön eki kullanıldığında piyasaya çıkarma adı **jdoe**Rollout olur.  Adlar şablonun değişkenler bölümünde tanımlanır.
* **azureResourcelocation**: Öğreticiyi basitleştirmek için aksi belirtilmediği sürece bu konuma tüm Deployment Manager kaynakları paylaşır. Şu anda Azure Deployment Manager kaynakları yalnızca **Orta ABD** veya **Doğu ABD 2**’de oluşturulabilir.
* **artifactSourceSASLocation**: SAS URI'sini dağıtım için hizmeti birim şablon ve parametreleri dosyalarının depolandığı kök dizinine (Blob kapsayıcısı).  [Yapıtları hazırlama](#prepare-the-artifacts) bölümüne bakın.
* **binaryArtifactRoot**:  Varsayılan değer **binaries/1.0.0.0**. [Yapıtları hazırlama](#prepare-the-artifacts) bölümünde açıklanan klasör yapısını değiştirmek istemiyorsanız bu değeri değiştirmeyin. Bu öğreticide göreli yollar kullanılır.  Tam yol, CreateWebApplicationParameters.json dosyasında belirtilen **artifactSourceSASLocation**, **binaryArtifactRoot** ve **deployPackageUri** birleştirilerek oluşturulur.  [Yapıtları hazırlama](#prepare-the-artifacts) bölümüne bakın.
* **managedIdentityID**: Kullanıcı tarafından atanan yönetilen dağıtım eylemleri gerçekleştiren kimliği. Bkz. [Kullanıcı tarafından atanmış yönetilen kimlik oluşturma](#create-the-user-assigned-managed-identity).

### <a name="the-variables"></a>Değişkenler

Değişkenler bölümü kaynakların adlarını tanımlar. Hizmet topolojisi adının, hizmet adlarının ve hizmet birimi adlarının [topoloji şablonunda](#create-the-service-topology-template) tanımlı adlarla eşleştiğinden emin olun.

![Azure Deployment Manager öğreticisi piyasaya çıkarma şablonu değişkenleri](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-rollout-template-variables.png)

### <a name="the-resources"></a>Kaynaklar

Kök düzeyde tanımlı üç kaynak vardır: yapıt kaynağı, adım ve piyasaya çıkarma.

Yapıt kaynağı tanımı, topoloji şablonunda tanımlı olanla aynıdır.  Daha fazla bilgi için bkz. [Hizmet topolojisi şablonunu oluşturma](#create-the-service-topology-template).

Aşağıdaki ekran görüntüsünde bekleme adımı tanımı gösterilmektedir:

![Azure Deployment Manager öğreticisi piyasaya çıkarma şablonu kaynakları beklemede adımı](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-rollout-template-resources-wait-step.png)

Süre [ISO 8601 standardını](https://en.wikipedia.org/wiki/ISO_8601#Durations) kullanmaktadır. **PT1M** (büyük harf zorunludur) 1 dakikalık bekleme süresine örnektir. 

Aşağıdaki ekran görüntüsünde piyasaya çıkarma tanımının yalnızca bazı bölümleri gösterilmektedir:

![Azure Deployment Manager öğreticisi piyasaya çıkarma şablonu kaynakları piyasaya çıkarma](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-rollout-template-resources-rollout.png)

* **dependsOn**: Dağıtım kaynağı yapıt kaynağı kaynak ve tanımlanan adımların hiçbirini bağlıdır.
* **artifactSourceId**: yapıt kaynağını piyasaya çıkarma kaynağıyla ilişkilendirmek için kullanılır.
* **targetServiceTopologyId**: hizmet topolojisi kaynağını piyasaya çıkarma kaynağıyla ilişkilendirmek için kullanılır.
* **deploymentTargetId**: Bu hizmet birimi kaynak service topolojisi kaynak kimliğidir.
* **preDeploymentSteps** ve **postDeploymentSteps**: tüm piyasaya çıkarma kaynaklarını içerir. Şablonda bir bekleme adımı çağrılır.
* **dependsOnStepGroups**: adım grupları arasındaki bağımlılıkları yapılandırır.

### <a name="rollout-parameters-file"></a>Piyasaya çıkarma parametre dosyası

Piyasaya çıkarma şablonuyla kullanılan bir parametre dosyası oluşturursunuz.

1. **\ADMTemplates\CreateADMRollout.Parameters** öğesini Visual Studio Code’da veya herhangi bir metin düzenleyicisinde açın.
2. Parametre değerlerini doldurun:

    * **namePrefix**: 4-5 karakter içeren bir dize girin. Bu ön ek, benzersiz Azure kaynağı adları oluşturmak için kullanılır.
    * **azureResourceLocation**: Şu anda Azure Deployment Manager kaynakları yalnızca **Orta ABD** veya **Doğu ABD 2**’de oluşturulabilir.
    * **artifactSourceSASLocation**: SAS URI'sini kök dizinine (Blob kapsayıcısı), dağıtım için hizmeti birim şablon ve parametreleri dosyalarının depolandığı yeri girin.  [Yapıtları hazırlama](#prepare-the-artifacts) bölümüne bakın.
    * **binaryArtifactRoot**: Yapıtlar klasör yapısını değiştirmediğiniz sürece, kullanın **binaries/1.0.0.0** bu öğreticideki.
    * **managedIdentityID**: Kullanıcı tarafından atanan bir yönetilen kimlik girin. Bkz. [Kullanıcı tarafından atanmış yönetilen kimlik oluşturma](#create-the-user-assigned-managed-identity). Sözdizimi şöyledir:

        ```
        "/subscriptions/<SubscriptionID>/resourcegroups/<ResourceGroupName>/providers/Microsoft.ManagedIdentity/userassignedidentities/<ManagedIdentityName>"
        ```

> [!IMPORTANT]
> Topoloji şablonu ve piyasaya çıkarma şablonu bazı ortak parametreleri paylaşır. Bu parametreler aynı değerlere sahip olmalıdır. Bu parametreler: **namePrefix**, **azureResourceLocation**, ve **artifactSourceSASLocation** (her iki yapıt kaynağı da bu öğreticide aynı depolama hesabını kullanır).

## <a name="deploy-the-templates"></a>Şablonları dağıtma

Azure PowerShell şablonları dağıtmak için kullanılabilir. 

1. Hizmet topolojisini dağıtmak için betiği çalıştırın.

    ```azurepowershell
    $resourceGroupName = "<Enter a Resource Group Name>"
    $location = "Central US"  
    $filePath = "<Enter the File Path to the Downloaded Tutorial Files>"

    # Create a resource group
    New-AzResourceGroup -Name $resourceGroupName -Location "$location"

    # Create the service topology
    New-AzResourceGroupDeployment `
        -ResourceGroupName $resourceGroupName `
        -TemplateFile "$filePath\ADMTemplates\CreateADMServiceTopology.json" `
        -TemplateParameterFile "$filePath\ADMTemplates\CreateADMServiceTopology.Parameters.json"
    ```

    > [!NOTE]
    > `New-AzResourceGroupDeployment` zaman uyumsuz bir çağrıdır. Başarı iletisi yalnızca dağıtım başarıyla başladı anlamına gelir. Dağıtımı doğrulamak için adım 2 ve 4. adım, bu yordamın bakın.

2. Azure portalı kullanarak hizmet topolojisinin ve temel kaynakların başarıyla oluşturulduğunu doğrulayın:

    ![Azure Deployment Manager öğreticisi dağıtılan hizmet topolojisi kaynakları](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-deployed-topology-resources.png)

    Kaynakları görmek için **Gizli türleri göster** seçeneği belirlenmelidir.

3. <a id="deploy-the-rollout-template"></a>Dağıtım şablonu dağıtın:

    ```azurepowershell
    # Create the rollout
    New-AzResourceGroupDeployment `
        -ResourceGroupName $resourceGroupName `
        -TemplateFile "$filePath\ADMTemplates\CreateADMRollout.json" `
        -TemplateParameterFile "$filePath\ADMTemplates\CreateADMRollout.Parameters.json"
    ```

4. Aşağıdaki PowerShell betiğini kullanarak piyasaya çıkarma ilerleme durumunu kontrol edin:

    ```azurepowershell
    # Get the rollout status
    $rolloutname = "<Enter the Rollout Name>" # "adm0925Rollout" is the rollout name used in this tutorial
    Get-AzDeploymentManagerRollout `
        -ResourceGroupName $resourceGroupName `
        -Name $rolloutName `
        -Verbose
    ```

    Bu cmdlet'in çalıştırılabilmesi için Deployment Manager PowerShell cmdlet'lerinin yüklü olması gerekir. Önkoşullara bakın. Verbose anahtar tamamını görebilmek için kullanılabilir çıktı.

    Aşağıdaki örnekte çalışma durumu gösterilmektedir:

    ```
    VERBOSE:

    Status: Succeeded
    ArtifactSourceId: /subscriptions/<AzureSubscriptionID>/resourceGroups/adm0925rg/providers/Microsoft.DeploymentManager/artifactSources/adm0925ArtifactSourceRollout
    BuildVersion: 1.0.0.0

    Operation Info:
        Retry Attempt: 0
        Skip Succeeded: False
        Start Time: 03/05/2019 15:26:13
        End Time: 03/05/2019 15:31:26
        Total Duration: 00:05:12

    Service: adm0925ServiceEUS
        TargetLocation: EastUS
        TargetSubscriptionId: <AzureSubscriptionID>

        ServiceUnit: adm0925ServiceEUSStorage
            TargetResourceGroup: adm0925ServiceEUSrg

            Step: Deploy
                Status: Succeeded
                StepGroup: stepGroup3
                Operation Info:
                    DeploymentName: 2F535084871E43E7A7A4CE7B45BE06510adm0925ServiceEUSStorage
                    CorrelationId: 0b6f030d-7348-48ae-a578-bcd6bcafe78d
                    Start Time: 03/05/2019 15:26:32
                    End Time: 03/05/2019 15:27:41
                    Total Duration: 00:01:08
                Resource Operations:

                    Resource Operation 1:
                    Name: txq6iwnyq5xle
                    Type: Microsoft.Storage/storageAccounts
                    ProvisioningState: Succeeded
                    StatusCode: OK
                    OperationId: 64A6E6EFEF1F7755

    ...

    ResourceGroupName       : adm0925rg
    BuildVersion            : 1.0.0.0
    ArtifactSourceId        : /subscriptions/<SubscriptionID>/resourceGroups/adm0925rg/providers/Microsoft.DeploymentManager/artifactSources/adm0925ArtifactSourceRollout
    TargetServiceTopologyId : /subscriptions/<SubscriptionID>/resourceGroups/adm0925rg/providers/Microsoft.DeploymentManager/serviceTopologies/adm0925ServiceTopology
    Status                  : Running
    TotalRetryAttempts      : 0
    OperationInfo           : Microsoft.Azure.Commands.DeploymentManager.Models.PSRolloutOperationInfo
    Services                : {adm0925ServiceEUS, adm0925ServiceWUS}
    Name                    : adm0925Rollout
    Type                    : Microsoft.DeploymentManager/rollouts
    Location                : centralus
    Id                      : /subscriptions/<SubscriptionID>/resourcegroups/adm0925rg/providers/Microsoft.DeploymentManager/rollouts/adm0925Rollout
    Tags                    :
    ```

    Piyasaya çıkarma başarıyla dağıtıldıktan sonra, her hizmet için bir adet olmak üzere iki kaynak grubunun daha oluşturulduğunu göreceksiniz.

## <a name="verify-the-deployment"></a>Dağıtımı doğrulama

1. [Azure portalı](https://portal.azure.com) açın.
2. Piyasaya çıkarma dağıtımı tarafından oluşturulan yeni kaynak gruplarındaki yeni oluşturulan web uygulamalarına gidin.
3. Web uygulamasını bir web tarayıcıda açın. Index.html dosyasında konumu ve sürümü doğrulayın.

## <a name="deploy-the-revision"></a>Düzeltmeyi dağıtma

Web uygulamasının yeni bir sürümüne (1.0.0.1) sahip olduğunuzda. Web uygulamasını yeniden dağıtmak için aşağıdaki yordamı kullanabilirsiniz.

1. CreateADMRollout.Parameters.json dosyasını açın.
2. **binaryArtifactRoot** öğesini **binaries/1.0.0.1** olarak güncelleştirin.
3. [Şablonları dağıtma](#deploy-the-rollout-template) bölümünde anlatıldığı gibi piyasaya çıkarmayı yeniden dağıtın.
4. [Dağıtımı doğrulama](#verify-the-deployment) bölümünde anlatıldığı gibi dağıtımı doğrulayın. Web sayfası 1.0.0.1 sürümünü gösterir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık Azure kaynakları gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

1. Azure portalda, sol menüden **Kaynak grubu**’nu seçin.
2. Bu öğreticide oluşturulan kaynak gruplarını daraltmak için **Ada göre filtrele** alanını kullanın. 3-4 adet olacaktır:

    * **&lt;namePrefix>rg**: Deployment Manager kaynaklarını içerir.
    * **&lt;namePrefix>ServiceWUSrg**: ServiceWUS tarafından tanımlanan kaynakları içerir.
    * **&lt;namePrefix>ServiceEUSrg**: ServiceEUS tarafından tanımlanan kaynakları içerir.
    * Kullanıcı tanımlı yönetilen kimlik için kaynak grubu.
3. Kaynak grubu adını seçin.  
4. Üstteki menüden **Kaynak grubunu sil**’i seçin.
5. Bu öğretici tarafından oluşturulan diğer kaynak gruplarını silmek için son iki adımı tekrarlayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure Deployment Manager'ı kullanmayı öğrendiniz. Azure Dağıtım Yöneticisi'nde sistem durumu izleme tümleştirmek için bkz: [Öğreticisi: Azure Dağıtım Yöneticisi'nde sistem durumu denetimi kullanın](./deployment-manager-tutorial-health-check.md).
