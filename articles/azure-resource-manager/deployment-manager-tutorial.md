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
ms.date: 10/04/2018
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 5d18a1f86e1d870db64199c575450dd475590b55
ms.sourcegitcommit: b4a46897fa52b1e04dd31e30677023a29d9ee0d9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49394491"
---
# <a name="tutorial-use-azure-deployment-manager-with-resource-manager-templates-private-preview"></a>Öğretici: Azure Deployment Manager’ı Resource Manager şablonlarıyla kullanma (Özel önizleme)

[Azure Deployment Manager](./deployment-manager-overview.md)’ı kullanarak uygulamalarınızı birden çok bölgede nasıl dağıtacağınızı öğrenin. Deployment Manager’ı kullanmak için iki şablon oluşturmanız gerekir:

* **Topoloji şablonu**: Uygulamalarınızı Azure kaynaklarını ve bunların dağıtılacağı yeri açıklar.
* **Dağıtım şablonu**: Uygulamalarınızı dağıtırken uygulanacak adımları açıklar.

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

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Ön koşullar

Bu makaleyi tamamlamak için gerekenler:

* [Azure Resource Manager şablonlarını](./resource-group-overview.md) geliştirme konusunda deneyim.
* Azure Dağıtım Yöneticisi özel önizleme aşamasındadır. Azure Deployment Manager'ı kullanarak kaydolmak için [kayıt sayfasını](https://aka.ms/admsignup) doldurun. 
* Azure PowerShell. Daha fazla bilgi için bkz. [Azure PowerShell kullanmaya başlayın](https://docs.microsoft.com/powershell/azure/get-started-azureps).
* Deployment Manager cmdlet'leri. Bu yayın öncesi cmdlet’leri yüklemek için PowerShellGet’in en son sürümü gereklidir. En son sürümü edinmek için bkz. [PowerShellGet’i Yükleme](/powershell/gallery/installing-psget). PowerShellGet’i yükledikten sonra PowerShell penceresini kapatın. Bir PowerShell penceresi açın ve şu komutu çalıştırın:

    ```
    Install-Module -Name AzureRM.DeploymentManager -AllowPrerelease
    ```
* [Microsoft Azure Depolama Gezgini](https://go.microsoft.com/fwlink/?LinkId=708343&clcid=0x409). Azure Depolama Gezgini gerekli değildir, ancak işinizi kolaylaştırır.

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

- **ADMTemplates**: burada, aşağıdakileri içeren Deployment Manager şablonları yer alır:
    - CreateADMServiceTopology.json
    - CreateADMServiceTopology.Parameters.json
    - CreateADMRollout.json
    - CreateADMRollout.Parameters.json
- **ArtifactStore**: hem şablon yapıtlarını hem de ikili dosya yapıtlarını içerir. [Yapıtları hazırlama](#prepare-the-artifacts) bölümüne bakın.

İki şablon kümesi olduğuna dikkat edin.  Kümelerin biri, hizmet topolojisini ve piyasaya çıkarmayı dağıtmak için kullanılan Deployment Manager şablonlarıdır. Diğer küme ise web hizmetlerini ve depolama hesaplarını oluşturmak için hizmetten çağrılır.

## <a name="prepare-the-artifacts"></a>Yapıtları hazırlama

İndirilen ArtifactStore klasörü iki klasör içerir:

![Azure Deployment Manager öğreticisi yapıt kaynağı diyagramı](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-artifact-source-diagram.png)

- **Şablonlar** klasörü: şablon yapıtlarını içerir. **1.0.0.0** ve **1.0.0.1** ikili dosya yapıtlarının iki sürümünü temsil eder. Her sürümde her bir hizmete yönelik bir klasör vardır (Hizmet Doğu ABD ve Hizmet Batı ABD). Her hizmette depolama hesabı oluşturmak için bir şablon çifti ve web uygulaması oluşturmak için başka bir şablon çifti vardır. Web uygulaması şablonu, web uygulaması dosyalarını içeren sıkıştırılmış bir paket çağırır. Sıkıştırılmış dosya, ikili dosyalar klasöründe depolanan bir ikili dosya yapıtıdır.
- **İkili dosyalar** klasörü: ikili dosya yapıtları içerir. **1.0.0.0** ve **1.0.0.1** ikili dosya yapıtlarının iki sürümünü temsil eder. Her sürümde, web uygulamasını Batı ABD konumunda oluşturmaya yönelik bir zip dosyası ve web uygulamasını Doğu ABD konumunda oluşturmaya yönelik başka bir zip dosyası vardır.

İki sürüm (1.0.0.0 ve 1.0.0.1) [sürüm dağıtımı](#deploy-the-revision) içindir. Şablon yapıtları ve ikili dosya yapıtlarının iki sürümü olsa da, iki sürüm arasında yalnızca ikili dosya yapıtları farklıdır. Gerçekte, ikili dosya yapıtları şablon yapıtlarına göre daha sık güncelleştirilir.

1. **\ArtifactStore\templates\1.0.0.0\ServiceWUS\CreateStorageAccount.json** dosyasını bir metin düzenleyicisinde açın. Depolama hesabı oluşturmaya yönelik temel bir şablondur.  
2. **\ArtifactStore\templates\1.0.0.0\ServiceWUS\CreateWebApplication.json** dosyasını açın. 

    ![Azure Deployment Manager öğreticisi web uygulaması şablonu oluşturma](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-create-web-application-packageuri.png)

    Şablon, web uygulamasının dosyalarını içeren bir dağıtım paketi çağırır. Bu öğreticide, sıkıştırılmış paket yalnızca bir index.html dosyası içermektedir.
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

1. Bir Azure Storage hesabı oluşturun. Yönergeler için bkz. [Hızlı Başlangıç: Azure portalı kullanarak blobları yükleme, indirme ve listeleme](../storage/blobs/storage-quickstart-blobs-portal.md).
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
> Kullanıcı tarafından atanmış yönetilen kimlik, [piyasaya çıkarma](#create-the-rollout-template) ile aynı konumda olmalıdır. Şu anda piyasaya çıkarma gibi Deployment Manager kaynakları yalnızca Orta ABD veya Doğu ABD 2’de oluşturulabilir.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. [Kullanıcı tarafından atanmış bir yönetilen kimlik](../active-directory/managed-identities-azure-resources/overview.md) oluşturun.
3. Portalda sol menüden **Abonelikler**’i ve ardından aboneliğinizi seçin.
4. **Erişim denetimi (IAM)** öğesini ve ardından **Ekle**’yi seçin
5. Aşağıdaki değerleri yazın veya seçin:

    ![Azure Deployment Manager öğreticisi kullanıcı tarafından atanmış yönetilen kimlik erişim denetimi](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-access-control.png)

    - **Rol**: yapıt dağıtımını (web uygulamaları ve depolama hesapları) tamamlamak için yeterli izinleri verin. Bu öğreticideki **Katkıda Bulunanı** seçin. Gerçekte, izinleri asgari seviyeyle sınırlamak istersiniz.
    - **Erişimi atama hedefi**: **Kullanıcı Tarafından Atanmış Yönetilen Kimlik** öğesini seçin.
    - Öğreticide daha önce oluşturduğunuz kullanıcı tarafından atanmış yönetilen kimliği seçin.
6. **Kaydet**’i seçin.

## <a name="create-the-service-topology-template"></a>Hizmet topolojisi şablonunu oluşturma

**\ADMTemplates\CreateADMServiceTopology.json** dosyasını açın.

### <a name="the-parameters"></a>Parametreler

Şablon aşağıdaki parametreleri içerir:

![Azure Deployment Manager öğreticisi topoloji şablonu parametreleri](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-topology-template-parameters.png)

- **namePrefix**: Bu ön ek, Deployment Manager kaynaklarının adlarını oluşturmak için kullanılır. Örneğin, "jdoe" ön eki kullanıldığında hizmet topolojisi adı **jdoe**ServiceTopology olur.  Kaynak adları bu şablonun değişkenler bölümünde tanımlanır.
- **azureResourcelocation**: Öğreticiyi basitleştirmek için, aksi belirtilmedikçe tüm kaynaklar bu konumu paylaşır. Şu anda Azure Deployment Manager kaynakları yalnızca **Orta ABD** veya **Doğu ABD 2**’de oluşturulabilir.
- **artifactSourceSASLocation**: Hizmet birimi şablonu ve parametre dosyalarının dağıtım için depolandığı Blob kapsayıcısının SAS URI’si.  [Yapıtları hazırlama](#prepare-the-artifacts) bölümüne bakın.
- **templateArtifactRoot**: Şablonların ve parametrelerin depolandığı Blob kapsayıcısına olan yol uzaklığı. Varsayılan değer: **templates/1.0.0.0**. [Yapıtları hazırlama](#prepare-the-artifacts) bölümünde açıklanan klasör yapısını değiştirmek istemiyorsanız bu değeri değiştirmeyin. Bu öğreticide göreli yollar kullanılır.  Tam yol **artifactSourceSASLocation**, **templateArtifactRoot** ve **templateArtifactSourceRelativePath** (veya **parametersArtifactSourceRelativePath**) birleştirilerek oluşturulur.
- **targetSubscriptionID**: Deployment Manager kaynaklarının dağıtılıp faturalandırılacağı abonelik kimliği. Bu öğreticide abonelik kimliğinizi kullanın.

### <a name="the-variables"></a>Değişkenler

Değişkenler bölümü kaynakların adlarını, iki hizmete yönelik Azure konumlarını: **Hizmet WUS** ve **Hizmet EUS**, ayrıca yapıt yollarını tanımlar:

![Azure Deployment Manager öğreticisi topoloji şablonu değişkenleri](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-topology-template-variables.png)

Yapıt yollarını depolama hesabına yüklediğiniz klasör yapısıyla karşılaştırın. Yapıt yollarının göreli yollar olduğuna dikkat edin. Tam yol **artifactSourceSASLocation**, **templateArtifactRoot** ve **templateArtifactSourceRelativePath** (veya **parametersArtifactSourceRelativePath**) birleştirilerek oluşturulur.

### <a name="the-resources"></a>Kaynaklar

Kök düzeyde tanımlı iki kaynak vardır: *yapıt kaynağı* ve *hizmet topolojisi*.

Yapıt kaynağı tanımı:

![Azure Deployment Manager öğreticisi topoloji şablonu kaynakları yapıt kaynağı](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-topology-template-resources-artifact-source.png)

Aşağıdaki ekran görüntüsünde hizmet topolojisinin, hizmetlerin ve hizmet birimi tanımlarının yalnızca bazı bölümleri gösterilmektedir:

![Azure Deployment Manager öğreticisi topoloji şablonu kaynakları hizmet topolojisi](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-topology-template-resources-service-topology.png)

- **artifactSourceId**, yapıt kaynağını hizmet topolojisi kaynağıyla ilişkilendirmek için kullanılır.
- **dependsOn**: Tüm hizmet topolojisi kaynakları yapıt kaynağına bağlıdır.
- **artifacts** şablon yapıtlarını işaret eder.  Burada göreli yollar kullanılır. Tam yol artifactSourceSASLocation (yapıt kaynağında tanımlıdır), artifactRoot (yapıt kaynağında tanımlıdır) ve templateArtifactSourceRelativePath (veya parametersArtifactSourceRelativePath) birleştirilerek oluşturulur.

### <a name="topology-parameters-file"></a>Topoloji parametre dosyası

Topoloji şablonuyla kullanılan bir parametre dosyası oluşturursunuz.

1. **\ADMTemplates\CreateADMServiceTopology.Parameters** öğesini Visual Studio Code’da veya herhangi bir metin düzenleyicisinde açın.
2. Parametre değerlerini doldurun:

    - **namePrefix**: 4-5 karakterden oluşan bir dize girin. Bu ön ek, benzersiz Azure kaynağı adları oluşturmak için kullanılır.
    - **azureResourceLocation**: Azure konumlarını kullanmaya alışık değilseniz, bu öğreticideki **centralus**’u kullanın.
    - **artifactSourceSASLocation**: Hizmet birimi şablonu ve parametreler dosyalarının dağıtım için depolandığı kök dizine (Blob kapsayıcısı) SAS URI’sini girin.  [Yapıtları hazırlama](#prepare-the-artifacts) bölümüne bakın.
    - **templateArtifactRoot**: Yapıtların klasör yapısını değiştirmediğiniz sürece bu öğreticideki **templates/1.0.0.0** klasörünü kullanın.
    - **tragetScriptionID**: Azure abonelik kimliğinizi girin.

> [!IMPORTANT]
> Topoloji şablonu ve piyasaya çıkarma şablonu bazı ortak parametreleri paylaşır. Bu parametreler aynı değerlere sahip olmalıdır. Bu parametreler: **namePrefix**, **azureResourceLocation**, ve **artifactSourceSASLocation** (her iki yapıt kaynağı da bu öğreticide aynı depolama hesabını kullanır).

## <a name="create-the-rollout-template"></a>Piyasaya çıkarma şablonunu oluşturma

**\ADMTemplates\CreateADMRollout.json** dosyasını açın.

### <a name="the-parameters"></a>Parametreler

Şablon aşağıdaki parametreleri içerir:

![Azure Deployment Manager öğreticisi piyasaya çıkarma şablonu parametreleri](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-rollout-template-parameters.png)

- **namePrefix**: Bu ön ek, Deployment Manager kaynaklarının adlarını oluşturmak için kullanılır. Örneğin, "jdoe" ön eki kullanıldığında piyasaya çıkarma adı **jdoe**Rollout olur.  Adlar şablonun değişkenler bölümünde tanımlanır.
- **azureResourcelocation**: Öğreticiyi basitleştirmek için tüm Deployment Manager kaynakları aksi belirtilmedikçe bu konumu paylaşır. Şu anda Azure Deployment Manager kaynakları yalnızca **Orta ABD** veya **Doğu ABD 2**’de oluşturulabilir.
- **artifactSourceSASLocation**: Hizmet birimi şablonu ve parametreler dosyalarının dağıtım için depolandığı kök dizin (Blob kapsayıcısı) SAS URI’si.  [Yapıtları hazırlama](#prepare-the-artifacts) bölümüne bakın.
- **binaryArtifactRoot**:  Varsayılan değer: **binaries/1.0.0.0**. [Yapıtları hazırlama](#prepare-the-artifacts) bölümünde açıklanan klasör yapısını değiştirmek istemiyorsanız bu değeri değiştirmeyin. Bu öğreticide göreli yollar kullanılır.  Tam yol, CreateWebApplicationParameters.json dosyasında belirtilen **artifactSourceSASLocation**, **binaryArtifactRoot** ve **deployPackageUri** birleştirilerek oluşturulur.  [Yapıtları hazırlama](#prepare-the-artifacts) bölümüne bakın.
- **managedIdentityID**: Dağıtım işlemlerini gerçekleştiren, kullanıcı tarafından atanmış yönetilen kimlik. Bkz. [Kullanıcı tarafından atanmış yönetilen kimlik oluşturma](#create-the-user-assigned-managed-identity).

### <a name="the-variables"></a>Değişkenler

Değişkenler bölümü kaynakların adlarını tanımlar. Hizmet topolojisi adının, hizmet adlarının ve hizmet birimi adlarının [topoloji şablonunda](#create-the-service-topology-template) tanımlı adlarla eşleştiğinden emin olun.

![Azure Deployment Manager öğreticisi piyasaya çıkarma şablonu değişkenleri](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-rollout-template-variables.png)

### <a name="the-resources"></a>Kaynaklar

Kök düzeyde tanımlı üç kaynak vardır: yapıt kaynağı, adım ve piyasaya çıkarma.

Yapıt kaynağı tanımı, topoloji şablonunda tanımlı olanla aynıdır.  Daha fazla bilgi için bkz. [Hizmet topolojisi şablonunu oluşturma](#create-the-service-topology-tempate).

Aşağıdaki ekran görüntüsünde bekleme adımı tanımı gösterilmektedir:

![Azure Deployment Manager öğreticisi piyasaya çıkarma şablonu kaynakları beklemede adımı](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-rollout-template-resources-wait-step.png)

Süre [ISO 8601 standardını](https://en.wikipedia.org/wiki/ISO_8601#Durations) kullanmaktadır. **PT1M** (büyük harf zorunludur) 1 dakikalık bekleme süresine örnektir. 

Aşağıdaki ekran görüntüsünde piyasaya çıkarma tanımının yalnızca bazı bölümleri gösterilmektedir:

![Azure Deployment Manager öğreticisi piyasaya çıkarma şablonu kaynakları piyasaya çıkarma](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-rollout-template-resources-rollout.png)

- **dependsOn**: Piyasaya çıkarma kaynağı yapıt kaynağına ve tanımlanan adımlara bağlıdır.
- **artifactSourceId**: yapıt kaynağını piyasaya çıkarma kaynağıyla ilişkilendirmek için kullanılır.
- **targetServiceTopologyId**: hizmet topolojisi kaynağını piyasaya çıkarma kaynağıyla ilişkilendirmek için kullanılır.
- **deploymentTargetId**: Hizmet topolojisi kaynağının hizmet birimi kaynağı kimliğidir.
- **preDeploymentSteps** ve **postDeploymentSteps**: tüm piyasaya çıkarma kaynaklarını içerir. Şablonda bir bekleme adımı çağrılır.
- **dependsOnStepGroups**: adım grupları arasındaki bağımlılıkları yapılandırır.

### <a name="rollout-parameters-file"></a>Piyasaya çıkarma parametre dosyası

Piyasaya çıkarma şablonuyla kullanılan bir parametre dosyası oluşturursunuz.

1. **\ADMTemplates\CreateADMRollout.Parameters** öğesini Visual Studio Code’da veya herhangi bir metin düzenleyicisinde açın.
2. Parametre değerlerini doldurun:

    - **namePrefix**: 4-5 karakterden oluşan bir dize girin. Bu ön ek, benzersiz Azure kaynağı adları oluşturmak için kullanılır.
    - **azureResourceLocation**: Şu anda Azure Deployment Manager kaynakları yalnızca **Orta ABD** veya **Doğu ABD 2**’de oluşturulabilir.
    - **artifactSourceSASLocation**: Hizmet birimi şablonu ve parametreler dosyalarının dağıtım için depolandığı kök dizine (Blob kapsayıcısı) SAS URI’sini girin.  [Yapıtları hazırlama](#prepare-the-artifacts) bölümüne bakın.
    - **binaryArtifactRoot**: Yapıtların klasör yapısını değiştirmediğiniz sürece bu öğreticideki **binaries/1.0.0.0** klasörünü kullanın.
    - **managedIdentityID**: Kullanıcı tarafından atanmış yönetilen kimliği girin. Bkz. [Kullanıcı tarafından atanmış yönetilen kimlik oluşturma](#create-the-user-assigned-managed-identity). Söz dizimi aşağıdaki gibidir:

        ```
        "/subscriptions/<SubscriptionID>/resourcegroups/<ResourceGroupName>/providers/Microsoft.ManagedIdentity/userassignedidentities/<ManagedIdentityName>"
        ```

> [!IMPORTANT]
> Topoloji şablonu ve piyasaya çıkarma şablonu bazı ortak parametreleri paylaşır. Bu parametreler aynı değerlere sahip olmalıdır. Bu parametreler: **namePrefix**, **azureResourceLocation**, ve **artifactSourceSASLocation** (her iki yapıt kaynağı da bu öğreticide aynı depolama hesabını kullanır).

## <a name="deploy-the-templates"></a>Şablonları dağıtma

Azure PowerShell şablonları dağıtmak için kullanılabilir. 

1. Hizmet topolojisini dağıtmak için betiği çalıştırın.

    ```powershell
    $deploymentName = "<Enter a Deployment Name>"
    $resourceGroupName = "<Enter a Resource Group Name>"
    $location = "Central US"  
    $filePath = "<Enter the File Path to the Downloaded Tutorial Files>"
    
    # Create a resource group
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    
    # Create the service topology
    New-AzureRmResourceGroupDeployment `
        -Name $deploymentName `
        -ResourceGroupName $resourceGroupName `
        -TemplateFile "$filePath\ADMTemplates\CreateADMServiceTopology.json" `
        -TemplateParameterFile "$filePath\ADMTemplates\CreateADMServiceTopology.Parameters.json"
    ```

2. Azure portalı kullanarak hizmet topolojisinin ve temel kaynakların başarıyla oluşturulduğunu doğrulayın:

    ![Azure Deployment Manager öğreticisi dağıtılan hizmet topolojisi kaynakları](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-deployed-topology-resources.png)

    Kaynakları görmek için **Gizli türleri göster** seçeneği belirlenmelidir.

3. Piyasaya çıkarma şablonunu dağıtın:

    ```powershell
    # Create the rollout
    New-AzureRmResourceGroupDeployment `
        -Name $deploymentName `
        -ResourceGroupName $resourceGroupName `
        -TemplateFile "$filePath\ADMTemplates\CreateADMRollout.json" `
        -TemplateParameterFile "$filePath\ADMTemplates\CreateADMRollout.Parameters.json"
    ```

4. Aşağıdaki PowerShell betiğini kullanarak piyasaya çıkarma ilerleme durumunu kontrol edin:

    ```powershell
    # Get the rollout status
    $rolloutname = "<Enter the Rollout Name>"
    Get-AzureRmDeploymentManagerRollout `
        -ResourceGroupName $resourceGroupName `
        -Name $rolloutName
    ```

    Bu cmdlet'in çalıştırılabilmesi için Deployment Manager PowerShell cmdlet'lerinin yüklü olması gerekir. [Ön koşullara](#prerequisite) bakın.

    Aşağıdaki örnekte çalışma durumu gösterilmektedir:
    
    ```
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
3. [Şablonları dağıtma](#deploy-the-templates) bölümünde anlatıldığı gibi piyasaya çıkarmayı yeniden dağıtın.
4. [Dağıtımı doğrulama](#verify-the-deployment) bölümünde anlatıldığı gibi dağıtımı doğrulayın. Web sayfası 1.0.0.1 sürümünü gösterir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık Azure kaynakları gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

1. Azure portalda, sol menüden **Kaynak grubu**’nu seçin.
2. Bu öğreticide oluşturulan kaynak gruplarını daraltmak için **Ada göre filtrele** alanını kullanın. 3-4 adet olacaktır:

    - **&lt;namePrefix>rg**: Deployment Manager kaynaklarını içerir.
    - **&lt;namePrefix>ServiceWUSrg**: ServiceWUS tarafından tanımlanan kaynakları içerir.
    - **&lt;namePrefix>ServiceEUSrg**: ServiceEUS tarafından tanımlanan kaynakları içerir.
    - Kullanıcı tanımlı yönetilen kimlik için kaynak grubu.
3. Kaynak grubu adını seçin.  
4. Üstteki menüden **Kaynak grubunu sil**’i seçin.
5. Bu öğretici tarafından oluşturulan diğer kaynak gruplarını silmek için son iki adımı tekrarlayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure Deployment Manager'ı kullanmayı öğrendiniz. Daha fazla bilgi edinmek için bkz. [Azure Resource Manager belgeleri](/azure/azure-resource-manager/).