---
title: Bağımlı kaynaklarla Azure Resource Manager şablonları oluşturma | Microsoft Docs
description: Birden fazla kaynakla bir Azure Resource Manager şablonu oluşturmayı ve Azure portalı kullanarak dağıtmayı öğrenin
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 08/27/2018
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 7509ed46ba07cd8250f82f8eb258d18e3f4a1ee6
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43107114"
---
# <a name="tutorial-create-azure-resource-manager-templates-with-dependent-resources"></a>Öğretici: Bağımlı kaynaklarla Azure Resource Manager şablonları oluşturma

Birden fazla kaynağı dağıtmak için kullanabileceğiniz bir Azure Resource Manager şablonu oluşturmayı öğrenin.  Şablonu oluşturduktan sonra Azure portaldan Cloud Shell kullanarak dağıtacaksınız.

Kaynakların bazıları başka bir kaynak var olana kadar dağıtılamaz. Örneğin depolama hesabı ve ağ arabirimi oluşturulmadan sanal makineyi oluşturamazsınız. Bu ilişkiyi, kaynakların birini diğer kaynaklara bağımlı hale getirerek tanımlarsınız. Resource Manager, kaynaklar arasındaki bağımlılıkları değerlendirir ve bunları bağımlılık sırasına göre dağıtır. Resource Manager, birbirine bağımlı olmayan kaynakları paralel olarak dağıtır. Daha fazla bilgi için bkz. [Azure Resource Manager şablonlarındaki kaynakları dağıtma sırasını belirleme](./resource-group-define-dependencies.md).

> [!div class="checklist"]
> * Hızlı başlangıç şablonunu açma
> * Şablonu keşfetme
> * Şablonu dağıtma
> * Kaynakları temizleme

Bu öğreticideki yönergeler bir sanal makine, bir sanal ağ ve ek birkaç bağımlı kaynak oluşturmaktadır. 

## <a name="prerequisites"></a>Ön koşullar

Bu makaleyi tamamlamak için gerekenler:

* [Visual Studio Code](https://code.visualstudio.com/).
* Resource Manager Araçları uzantısı.  Bkz. [Uzantıyı yükleme](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites)

## <a name="open-a-quickstart-template"></a>Hızlı başlangıç şablonunu açma

Azure Hızlı Başlangıç Şablonları, Resource Manager şablonları için bir depolama alanıdır. Sıfırdan bir şablon oluşturmak yerine örnek bir şablon bulabilir ve bunu özelleştirebilirsiniz. Bu öğreticide kullanılan şablonun adı: [Deploy a simple Windows VM](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/) (Basit bir Windows sanal makinesi dağıtma).

1. Visual Studio Code’dan **Dosya**>**Dosya Aç**’ı seçin.
2. **Dosya adı**’na şu URL’yi yapıştırın:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json
    ```
3. Dosyayı açmak için **Aç**’ı seçin.
4. **Dosya**>**Farklı Kaydet**'i seçerek dosyanın bir kopyasını yerel bilgisayarınıza **azuredeploy.json** adıyla kaydedin.

## <a name="explore-the-template"></a>Şablonu keşfetme

Bu bölümdeki şablonu inceledikten sonra şu soruları yanıtlamaya çalışın:

- Bu şablonda kaç adet Azure kaynağı tanımlanmıştır?
- Kaynaklardan biri bir Azure depolama hesabıdır.  Tanım, son öğreticide kullanılan tanıma benziyor mu?
- Bu şablonda tanımlanan kaynaklara ilişkin şablon başvurularını bulabilir misiniz?
- Kaynakların bağımlılıklarını bulabilir misiniz?

1. Visual Studio Code'da birinci düzeydeki öğeleri ve **resources** içindeki ikinci düzey öğeleri görene kadar öğeleri daraltın:

    ![Visual Studio Code Azure Resource Manager şablonları](./media/resource-manager-tutorial-create-templates-with-dependent-resources/resource-manager-template-visual-studio-code.png)

    Şablonun tanımladığı beş kaynak vardır.
2. İlk kaynağı genişletin. Bir depolama hesabıdır. Tanım, son öğreticinin başında kullanılan tanımla aynı olacaktır.

    ![Visual Studio Code Azure Resource Manager şablonları depolama hesabı tanımı](./media/resource-manager-tutorial-create-templates-with-dependent-resources/resource-manager-template-storage-account-definition.png)

3. İkinci kaynağı genişletin. Kaynak türü: **Microsoft.Network/publicIPAddresses**. Şablon başvurusunu bulmak için, [şablon referansı](https://docs.microsoft.com/azure/templates/) bölümüne göz atın, **Başlığa göre filtrele** alanına **genel IP adresi** veya **genel IP adresleri** girin. Kaynak tanımını şablon başvurusu ile karşılaştırın.

    ![Visual Studio Code Azure Resource Manager şablonları genel IP adresi tanımı](./media/resource-manager-tutorial-create-templates-with-dependent-resources/resource-manager-template-public-ip-address-definition.png)
4. Bu şablonda tanımlanan diğer kaynaklara ilişkin şablon başvurularını bulmak için son adımı yineleyin.  Kaynak tanımlarını başvurular ile karşılaştırın.
5. Dördüncü kaynağı genişletin:

    ![Visual Studio Code Azure Resource Manager şablonları dependson](./media/resource-manager-tutorial-create-templates-with-dependent-resources/resource-manager-template-visual-studio-code-dependson.png)

    dependsOn öğesi, kaynaklardan birini diğer kaynaklardan birine veya daha fazlasına bağımlı olarak tanımlamanızı sağlar. Bu örnekte bu kaynak networkInterface olmuştur.  İki farklı kaynağa bağımlıdır:

    * publicIPAddress
    * virtualNetwork

6. Beşinci kaynağı genişletin. Bu kaynak bir sanal makinedir. İki farklı kaynağa bağımlıdır:

    * storageAccount
    * networkInterface

Aşağıdaki diyagramda bu şablondaki kaynaklar ve bağımlılık bilgileri gösterilmiştir:

![Visual Studio Code Azure Resource Manager şablonları bağımlılık diyagramı](./media/resource-manager-tutorial-create-templates-with-dependent-resources/resource-manager-template-visual-studio-code-dependency-diagram.png)

Bağımlılıkların belirtilmesi, Resource Manager'ın çözümü verimli bir şekilde dağıtmasını sağlar. Depolama hesabı, genel IP adresi ve sanal ağ herhangi bir bağımlılığa sahip olmadığından paralel olarak dağıtılır. Genel IP adresi ve sanal ağ dağıtıldıktan sonra ağ arabirimi oluşturulur. Resource Manager, diğer tüm kaynaklar dağıtıldıktan sonra sanal makineyi dağıtır.

## <a name="deploy-the-template"></a>Şablonu dağıtma

Şablonları dağıtmak için birçok yöntem vardır.  Bu öğreticide Azure portaldan Cloud Shell'i kullanacaksınız.

1. [Azure portalda](https://portal.azure.com) oturum açma
2. Aşağıdaki resimde gösterildiği gibi, sağ üst köşeden **Cloud Shell**’i seçin:

    ![Azure portal Cloud Shell](./media/resource-manager-tutorial-create-templates-with-dependent-resources/azure-portal-cloud-shell.png)
3. Cloud Shell'in sol üst köşesinden **PowerShell**'i seçin.  Bu öğreticide PowerShell'i kullanacaksınız.
4. **Yeniden başlat**'ı seçin
5. Cloud Shell'de **Dosya yükle**'yi seçin:

    ![Azure portal Cloud shell dosya karşıya yükleme](./media/resource-manager-tutorial-create-templates-with-dependent-resources/azure-portal-cloud-shell-upload-file.png)
6. Öğreticide daha önce kaydettiğiniz dosyayı seçin. Varsayılan ad **azuredeploy.json** olur.  Aynı ada sahip bir dosya varsa bildirim gösterilmeden eski dosyanın üzerine yazılır.
7. Dosyanın başarıyla yüklendiğini doğrulamak için Cloud Shell'de aşağıdaki komutu çalıştırın. 

    ```shell
    ls
    ```

    ![Azure portal Cloud Shell dosya listeleme](./media/resource-manager-tutorial-create-templates-with-dependent-resources/azure-portal-cloud-shell-list-file.png)

    Ekranda gösterilen dosya adı: azuredeploy.json.

8. Cloud Shell'de aşağıdaki komutu çalıştırarak JSON dosyasının içeriğini doğrulayın:

    ```shell
    cat azuredeploy.json
    ```
9. Cloud Shell'de aşağıdaki PowerShell komutlarını çalıştırın:

    ```powershell
    $resourceGroupName = "<Enter the resource group name>"
    $location = "<Enter the Azure location>"
    $vmAdmin = "<Enter the admin username>"
    $vmPassword = "<Enter the password>"
    $dnsLabelPrefix = "<Enter the prefix>"

    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    $vmPW = ConvertTo-SecureString -String $vmPassword -AsPlainText -Force
    New-AzureRmResourceGroupDeployment -Name mydeployment0710 -ResourceGroupName $resourceGroupName `
        -TemplateFile azuredeploy.json -adminUsername $vmAdmin -adminPassword $vmPW `
        -dnsLabelPrefix $dnsLabelPrefix
    ```
    Örnek dağıtımın ekran görüntüsü:

    ![Azure portal Cloud Shell şablon dağıtımı](./media/resource-manager-tutorial-create-templates-with-dependent-resources/azure-portal-cloud-shell-deploy-template.png)

    Ekran görüntüsünde şu değerler kullanılır:

    * **$resourceGroupName**: myresourcegroup0710. 
    * **$location**: eastus2
    * **&lt;DeployName>**: mydeployment0710
    * **&lt;TemplateFile>**: azuredeploy.json
    * **Template parameter**s:

        * **adminUsername**: JohnDole
        * **adminPassword**: Pass@word123
        * **dnsLabelPrefix**: myvm0710

10. Yeni oluşturulan sanal makineyi listelemek için aşağıdaki PowerShell komutunu çalıştırın:

    ```powershell
    Get-AzureRmVM -Name SimpleWinVM -ResourceGroupName <ResourceGroupName>
    ```

    Sanal makine adı şablon içinde **SimpleWinVM** olarak kodlanmıştır ve değiştirilemez.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık Azure kaynakları gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

1. Azure portalda, sol menüden **Kaynak grubu**’nu seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin.  Kaynak grubundaki toplam altı kaynak görüyor olmalısınız.
4. Üstteki menüden **Kaynak grubunu sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide sanal makine, sanal ağ ve bağımlı kaynaklar oluşturmak için bir şablon geliştirdiniz ve dağıttınız. Şablonlar hakkında daha fazla bilgi edinmek için bkz. [Azure Resource Manager şablonlarının yapısını ve söz dizimini anlama](./resource-group-authoring-templates.md).