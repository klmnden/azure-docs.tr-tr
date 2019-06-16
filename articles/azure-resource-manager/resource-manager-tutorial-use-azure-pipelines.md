---
title: Azure işlem hatları ile sürekli tümleştirme | Microsoft Docs
description: Sürekli olarak derle, test ve Azure Resource Manager şablonlarını dağıtma hakkında bilgi edinin.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: carmonm
editor: ''
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 06/12/2019
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 85dc0476da12bea64610b6910b0682fef00f4b5a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67057795"
---
# <a name="tutorial-continuous-integration-of-azure-resource-manager-templates-with-azure-pipelines"></a>Öğretici: Azure Resource Manager şablonlarının Azure işlem hatları ile sürekli tümleştirme

Azure işlem hatları sürekli derleyip dağıtmanızı Azure Resource Manager şablonu projeleri için kullanmayı öğrenin.

Azure DevOps iş planlama, kod geliştirme üzerinde işbirliği yapın ve uygulamaları geliştirmek ve dağıtmak için takımları desteklemek için geliştirici hizmetleri sağlar. Geliştiriciler, Azure DevOps Hizmetleri kullanarak bulutta çalışabilir. Azure DevOps tümleştirilmiş web tarayıcınızı veya istemci IDE erişebildiği özellikleri sağlar. Azure işlem hattı, bu özelliklerden biridir. Azure işlem hatları olan tam özellikli bir sürekli tümleştirme (CI) ve sürekli teslim (CD) hizmeti. Bu, tercih edilen Git sağlayıcısı ile çalıştığından ve çoğu büyük bulut Hizmetleri için dağıtabilirsiniz. Daha sonra derleme, test ve Microsoft Azure, Google Cloud Platform veya Amazon Web Hizmetleri, kodunuzun dağıtımını otomatik hale getirebilirsiniz.

Bu öğretici, yeni Azure DevOps Hizmetleri ve Azure işlem hatlarını olan Azure Resource Manager şablonu geliştiricileri için tasarlanmıştır. GitHub ve DevOps ile biliyorsanız, adımına atlayabilirsiniz [işlem hattı oluşturma](#create-a-pipeline).

> [!NOTE]
> Bir proje adı seçin. Öğreticiyi incelemek, birini değiştirin **AzureRmPipeline** projenizin adına sahip.

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * GitHub deposu hazırlama
> * Azure DevOps projesi oluşturma
> * Bir Azure işlem hattı oluşturma
> * İşlem hattı dağıtımı doğrulama
> * Güncelleştirme şablonu ve yeniden dağıtma
> * Kaynakları temizleme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Bu makaleyi tamamlamak için gerekenler:

* **Bir GitHub hesabı**, burada, bir depo şablonlarınızı oluşturmak için bunu kullanın. Yoksa, şunları yapabilirsiniz [ücretsiz oluşturun](https://github.com). GitHub depoları kullanma hakkında daha fazla bilgi için bkz. [derleme GitHub depoları](/azure/devops/pipelines/repos/github).
* **Git’i yükleyin**. Bu öğretici yönerge kullanan *Git Bash* veya *Git Kabuğu*. Yönergeler için [yükleme Git]( https://www.atlassian.com/git/tutorials/install-git).
* **Azure DevOps kuruluş**. Yoksa, bir ücretsiz oluşturabilirsiniz. Bkz: [bir kuruluş veya proje koleksiyonu oluşturarak]( https://docs.microsoft.com/azure/devops/organizations/accounts/create-organization?view=azure-devops).
* **[Visual Studio Code](https://code.visualstudio.com/) ile Resource Manager araçları uzantısını**. Bkz. [Uzantıyı yükleme](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites).

## <a name="prepare-a-github-repository"></a>GitHub deposu hazırlama

GitHub, Resource Manager şablonları dahil olmak üzere proje kaynak kodunuz depolamak için kullanılır. Desteklenen diğer depolar için bkz. [Azure DevOps tarafından desteklenen depoları](/azure/devops/pipelines/repos/?view=azure-devops#supported-repository-types).

### <a name="create-a-github-repository"></a>Bir GitHub deposu oluşturun

Bir GitHub hesabı yoksa bkz [önkoşulları](#prerequisites).

1. Oturum [GitHub](https://github.com).
2. Sağ üst köşede hesap görüntünüzü seçin ve ardından **depolarınızı**.

    ![GitHub deposunu Azure Resource Manager Azure DevOps Azure işlem hatları oluşturma](./media/resource-manager-tutorial-use-azure-pipelines/azure-resource-manager-devops-pipelines-github-repository.png)

1. Seçin **yeni**, yeşil bir düğme.
1. İçinde **depo adı**, depo adı girin.  Örneğin, **AzureRmPipeline depo**. Herhangi bir değiştirmeyi unutmayın **AzureRmPipeline** projenizin adına sahip. Seçebilirsiniz **genel** veya **özel** Bu öğreticide gitmek için. Ve ardından **Oluştur depo**.
1. URL'sini yazın. Depo URL'si şu biçimdedir:

    ```url
    https://github.com/[YourAccountName]/[YourRepositoryName]
    ```

Bu depo şeklinde adlandırılan bir *uzak depo*. Her biri aynı projenin geliştiriciler kopyalayabilirsiniz onun kendi *yerel depo*ve uzak depoya değişiklikleri birleştirin.

### <a name="clone-the-remote-repository"></a>Uzak depoyu kopyalama

1. Git kabuğu veya Git Bash'i açın.  [Ön koşullara](#prerequisites) bakın.
1. Geçerli klasörünüzü doğrulayın **github**.
1. Şu komutu çalıştırın:

    ```bash
    git clone https://github.com/[YourAccountName]/[YourGitHubRepositoryName]
    cd [YourGitHubRepositoryName]
    mkdir CreateAzureStorage
    cd CreateAzureStorage
    pwd
    ```

    Değiştirin **[YourAccountName]** hesap adı ile GitHub ve Değiştir **[YourGitHubRepositoryName]** depo adınızı, önceki yordamda oluşturduğunuz ile.

    Aşağıdaki ekran görüntüleri bir örnek gösterilmektedir.

    ![GitHub bash Azure Resource Manager Azure DevOps Azure işlem hatları oluşturma](./media/resource-manager-tutorial-use-azure-pipelines/azure-resource-manager-devops-pipelines-github-bash.png)

**CreateAzureStorage** şablon depolandığı klasöre bir klasördür. **Pwd** komut klasör yolu gösterir. Aşağıdaki yordamda şablona kaydetmek yoludur.

### <a name="download-a-quickstart-template"></a>Hızlı Başlangıç şablonu yükle

Bir şablon oluşturmak yerine, karşıdan yükleyebileceğiniz bir [Hızlı Başlangıç şablonu]( https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json). Bu şablon, bir Azure depolama hesabı oluşturur.

1. Visual Studio Code'u açın. [Ön koşullara](#prerequisites) bakın.
2. Şablon ile aşağıdaki URL'yi açın:

    ```URL
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
    ```

3. Dosyayı Farklı Kaydet **azuredeploy.json** için **CreateAzureStorage** klasör. İşlem hattı, oldukları gibi hem klasör adı ve dosya adı kullanılır.  Bu adlar değiştirirseniz, işlem hattında kullanılan adları güncelleştirmeniz gerekir.

### <a name="push-the-template-to-the-remote-repository"></a>Uzak depoya gönderme şablonu

Azuredeploy.json yerel depoya eklendi. Ardından, uzak depoya şablonunu karşıya yükleyin.

1. Açık *Git Kabuğu* veya *Git Bash*, bunu açık değil.
1. Yerel deponuzda CreateAzureStorage klasörüne dizin değiştirin.
1. Doğrulama **azuredeploy.json** klasöründe bir dosyadır.
1. Şu komutu çalıştırın:

    ```bash
    git add .
    git commit -m “Add a new create storage account template.”
    git push origin master
    ```

    LF ile ilgili bir uyarı alabilirsiniz. Bu uyarıyı yoksayabilirsiniz. **Ana** ana daldır.  Genellikle her güncelleme için bir dalı oluşturursunuz. Öğreticiyi basitleştirmek için ana dala doğrudan kullanın.
1. GitHub deponuza tarayıcıdan göz atın.  URL  **https://github.com/ [YourAccountName] / [YourGitHubRepository]** . Göreceksiniz **CreateAzureStorage** klasörü ve **Azuredeploy.json** klasörün içindeki.

Şu ana kadar bir GitHub deposuna oluşturulur ve depoya bir şablonu karşıya.

## <a name="create-a-devops-project"></a>DevOps projesi oluşturma

Sonraki yordama devam etmeden önce bir DevOps kuruluş gereklidir.  Yoksa, bkz. [önkoşulları](#prerequisites).

1. Oturum [Azure DevOps](https://dev.azure.com).
1. Soldan bir DevOps kuruluş seçin.

    ![Azure Resource Manager Azure DevOps Azure işlem hatları Azure DevOps projesi oluşturma](./media/resource-manager-tutorial-use-azure-pipelines/azure-resource-manager-devops-pipelines-create-devops-project.png)

1. **Create project** (Proje oluştur) öğesini seçin. Tüm projelerin yoksa, oluşturma proje sayfası otomatik olarak açılır.
1. Aşağıdaki değerleri girin:

    * **Proje adı**: bir proje adı girin. Öğreticinin çok başında seçtiğiniz proje adını kullanabilirsiniz.
    * **Sürüm denetimi**: Seçin **Git**. Genişletmeniz gerekebilir **Gelişmiş** görmek için **sürüm denetimi**.

    Diğer özellikler için varsayılan değeri kullanın.
1. **Create project** (Proje oluştur) öğesini seçin.

Projeleri Azure'a dağıtmak için kullanılan bir hizmet bağlantısı oluşturun.

1. Seçin **proje ayarları** soldaki menünün altındaki.
1. Seçin **hizmet bağlantıları** altında **işlem hatları**.
1. Seçin **yeni hizmet bağlantısı**ve ardından **AzureResourceManager**.
1. Aşağıdaki değerleri girin:

    * **Bağlantı adı**: bir bağlantı adı girin. Örneğin, **AzureRmPipeline conn**. Bu adı yazın, işlem hattınızı oluşturduğunuzda adı gerekir.
    * **Kapsam düzeyi**: seçin **abonelik**.
    * **Abonelik**: aboneliğinizi seçin.
    * **Kaynak grubu**: Boş bırakın.
    * **Bu bağlantıyı kullanmak tüm işlem hatları izin**. (Seçili)
1. **Tamam**’ı seçin.

## <a name="create-a-pipeline"></a>İşlem hattı oluşturma

Şimdiye kadar aşağıdaki görevleri tamamladınız.  GitHub ve DevOps konusunda bilgi sahibi olduğunuz için önceki bölümlerde atlarsanız, devam etmeden önce görevleri tamamlamanız gerekir.

- Bir GitHub deposu oluşturun ve kaydedin [Bu şablon](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json) için **CreateAzureStorage** klasöründe depo.
- DevOps projesi oluşturun ve bir Azure Resource Manager hizmet bağlantısı oluşturun.

Bir şablonu dağıtmak için bir adım ile bir işlem hattı oluşturmak için:

1. Seçin **işlem hatları** sol menüden.
1. Seçin **yeni işlem hattı**.
1. Gelen **Connect** sekmesinde **GitHub**. İstenirse, GitHub kimlik bilgilerinizi girin ve ardından yönergeleri izleyin. Aşağıdaki ekran görürseniz, seçin **yalnızca depo seçin**ve deponuzu seçtiğiniz önce listede olduğunu doğrulayın **onaylamak ve yükleme**.

    ![Azure Resource Manager Azure DevOps Azure işlem hatları yalnızca depo seçin](./media/resource-manager-tutorial-use-azure-pipelines/azure-resource-manager-devops-pipelines-only-select-repositories.png)

1. Gelen **seçin** sekmesinde, depoyu seçin.  Varsayılan ad **[YourAccountName] / [YourGitHubRepositoryName]** .
1. Gelen **yapılandırma** sekmesinde **başlangıç işlem hattı**. Bu gösterir **azure pipelines.yml** iki betik adımlarla ardışık düzen dosyası.
1. Değiştirin **adımları** bölümü aşağıdaki YAML ile:

    ```yaml
    steps:
    - task: AzureResourceGroupDeployment@2
      inputs:
        azureSubscription: '[YourServiceConnectionName]'
        action: 'Create Or Update Resource Group'
        resourceGroupName: '[EnterANewResourceGroupName]'
        location: 'Central US'
        templateLocation: 'Linked artifact'
        csmFile: 'CreateAzureStorage/azuredeploy.json'
        deploymentMode: 'Incremental'
    ```

    Gibi görünmesi:

    ![Azure Resource Manager Azure DevOps Azure işlem hatları yaml](./media/resource-manager-tutorial-use-azure-pipelines/azure-resource-manager-devops-pipelines-yml.png)

    Aşağıdaki değişiklikleri yapın:

    * **azureSubscription**: değeri önceki yordamda oluşturduğunuz hizmet bağlantısı ile güncelleştirin.
    * **Eylem**: **oluşturma veya güncelleştirme kaynak grubu** 2 Eylemler - 1 eylem yok. Yeni bir kaynak grubu adı sağlanmazsa, bir kaynak grubu oluşturmak; 2. Belirtilen şablonu dağıtın.
    * **resourceGroupName**: yeni bir kaynak grubunu adını belirtmelisiniz. Örneğin, **AzureRmPipeline-rg**.
    * **Konum**: kaynak grubu konumunu belirtin.
    * **templateLocation**: zaman **bağlantılı yapıt** belirtilirse, görev, doğrudan bağlı deposundan şablon dosyası arar.
    * **csmFile** şablon dosya yolu. Tüm şablonda tanımlanan parametrelerin varsayılan değerlere sahip bir şablon parametreleri dosyası belirtmeniz gerekmez.

    Görev hakkında daha fazla bilgi için bkz. [Azure kaynak grubu dağıtım görevi](/azure/devops/pipelines/tasks/deploy/azure-resource-group-deployment)
1. **Kaydet ve çalıştır**’ı seçin.
1. Seçin **Kaydet ve Çalıştır** yeniden. YAML dosyasının bir kopyasını bağlı deposuna kaydedilir. Deponuza göz atma tarafından YAML dosyası görebilirsiniz.
1. İşlem hattı başarılı bir şekilde yürütüldü doğrulayın.

    ![Azure Resource Manager Azure DevOps Azure işlem hatları yaml](./media/resource-manager-tutorial-use-azure-pipelines/azure-resource-manager-devops-pipelines-status.png)

## <a name="verify-the-deployment"></a>Dağıtımı doğrulama

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Kaynak grubunu açın. İşlem hattı YAML dosyası içinde belirtilen adıdır.  Oluşturduğunuz bir depolama hesabına göreceksiniz.  Depolama hesabı adı ile başlayan **depolamak**.
1. Açmak için depolama hesabı adı seçin.
1. Seçin **özellikleri**. Bildirim **SKU** olduğu **Standard_LRS**.

    ![Azure Resource Manager Azure DevOps Azure işlem hatları portal doğrulama](./media/resource-manager-tutorial-use-azure-pipelines/azure-resource-manager-devops-pipelines-portal-verification.png)

## <a name="update-and-redeploy"></a>Güncelleştirme ve yeniden dağıtma

Şablonu güncelleştirin ve değişiklikleri uzak depoya gönderin, işlem hattını otomatik olarak kaynakları, depolama hesabı bu durumda güncelleştirir.

1. Açık **azuredeploy.json** Visual Studio code'da yerel deponuzdan.
1. Güncelleştirme **defaultValue** , **storageAccountType** için **Standard_GRS**. Aşağıdaki ekran görüntüsüne bakın:

    ![Azure Resource Manager Azure DevOps Azure işlem hatları yaml güncelleştir](./media/resource-manager-tutorial-use-azure-pipelines/azure-resource-manager-devops-pipelines-update-yml.png)

1. Değişiklikleri kaydedin.
1. Git Bash/Kabuğu'ndan aşağıdaki komutları çalıştırarak, değişiklikleri uzak depoya gönderin.

    ```bash
    git pull origin master
    git add .
    git commit -m “Add a new create storage account template.”
    git push origin master
    ```

    İlk komut, yerel depoya uzak depo ile eşitler. İşlem hattı YAML dosyası uzak depoya eklendi unutmayın.

    Uzak depo güncelleştirildi'nın ana dalı ile işlem hattını yeniden harekete geçirilir.

Değişiklikleri doğrulamak için depolama hesabının SKU kontrol edebilirsiniz.  Bkz: [dağıtımı doğrulama](#verify-the-deployment).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık Azure kaynakları gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

1. Azure portalda, sol menüden **Kaynak grubu**’nu seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin.
4. Üstteki menüden **Kaynak grubunu sil**’i seçin.

GitHub deposunu ve Azure DevOps projesi silmek isteyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure Resource Manager şablonu dağıtmak için bir Azure DevOps işlem hattı oluşturun. Azure kaynaklarını birden fazla bölgede dağıtma ve güvenli dağıtım uygulamalarını kullanma hakkında bilgi edinmek için bkz.

> [!div class="nextstepaction"]
> [Azure Deployment Manager’ı kullanma](./resource-manager-tutorial-deploy-vm-extensions.md)
