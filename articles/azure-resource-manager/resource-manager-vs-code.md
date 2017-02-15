---
title: "Resource Manager şablonları ile VS Code kullanma | Microsoft Belgeleri"
description: "Azure Resource Manager şablonları oluşturmak için Visual Studio Code kurulumunu gösterir."
services: azure-resource-manager
documentationcenter: na
author: cmatskas
manager: timlt
editor: tysonn
ms.assetid: 78f2aa22-df1d-41bd-92ec-dabd1175db88
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/03/2017
ms.author: chmatsk;tomfitz
translationtype: Human Translation
ms.sourcegitcommit: 10c7051c9b1218081d95cb10403006bfd95126ba
ms.openlocfilehash: 2ac1c2cce7a9e045990894b0bbaa045df3d48954


---
# <a name="working-with-azure-resource-manager-templates-in-visual-studio-code"></a>Visual Studio Code’da Azure Resource Manager Şablonları ile Çalışma
Azure Resource Manager şablonları bir kaynağı ve ilgili bağımlılıkları tanımlayan JSON dosyalarıdır. Bu dosyalar bazen büyük ve karmaşık olabilir, bu nedenle araç desteği önemlidir. Visual Studio Code yeni, basit, açık kaynaklı ve çapraz platformlu bir kod düzenleyicisidir. Resource Manager şablonlarını [yeni bir uzantı](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools) ile oluşturup düzenlemeyi destekler. VS Code her yerde çalışır ve yalnızca Resource Manager şablonlarınızı Azure aboneliğinize dağıtmak istediğinizde İnternet erişimi gerektirir.

Henüz VS Code’a sahip değilseniz [https://code.visualstudio.com/](https://code.visualstudio.com/) adresinden yükleyebilirsiniz.

## <a name="install-the-resource-manager-extension"></a>Resource Manager uzantısını yükleme
VS Code’da JSON şablonları ile çalışmak için bir uzantı yüklemeniz gerekir. Aşağıdaki adımlar Resource Manager JSON şablonları için dil desteğini indirip yükler:

1. VS Code’u başlatın 
2. Quick Open’ı açın (Ctrl+P) 
3. Şu komutu çalıştırın: 
   
        ext install msazurermtools.azurerm-vscode-tools
4. Uzantının etkinleştirilmesi istendiğinde VS Code’u yeniden başlatın. 
   
   İşlem tamam!

## <a name="set-up-resource-manager-snippets"></a>Resource Manager parçacıklarını ayarlama
Önceki adımlarda araç desteği yüklendi, ancak şimdi VS Code’un JSON şablon parçacıklarını kullanacak şekilde yapılandırılması gerekmektedir.

1. [azure-xplat-arm-tooling](https://raw.githubusercontent.com/Azure/azure-xplat-arm-tooling/master/VSCode/armsnippets.json) deposundaki dosyanın içeriklerini panonuza kopyalayın.
2. VS Code’u başlatın 
3. VS Code’da JSON kod parçacıkları dosyasını açmak için **Dosya** -> **Tercihler** -> **Kullanıcı Kod Parçacıkları** -> **JSON** yolunu izleyin. İsterseniz **F1**’e basıp **tercihler** yazabilir ve **Tercihler: Kod Parçacıkları**’nı seçebilirsiniz.
   
    ![tercih parçacıkları](./media/resource-manager-vs-code/preferences-snippets.png)
   
    Seçeneklerden **JSON**’u seçin.
   
    ![json seçme](./media/resource-manager-vs-code/select-json.png)
4. Adım 1’deki dosyanın içeriklerini kullanıcı parçacıkları dosyanızda son "}" işaretinin önüne yapıştırın 
5. JSON dosyasının sorunsuz göründüğünden ve başka bir yerde dalgalı çizgiler olmadığından emin olun. 
6. Kullanıcı parçacıkları dosyasını kaydedip kapatın.

Resource Manager parçacıklarını kullanmaya başlamak için yapmanız gerekenler bunlardır. Daha sonra, bu kurulumu testten geçireceğiz.

## <a name="work-with-template-in-vs-code"></a>VS Code’da şablon ile çalışma
Bir şablonla çalışmaya başlamanın en kolay yolu [Github](https://github.com/Azure/azure-quickstart-templates) üzerinde mevcut olan Hızlı Başlangıç Şablonlarından birini seçmeniz veya kendi şablonlarınızdan birini kullanmanızdır. Kaynak gruplarınızdan herhangi biri için portal aracılığıyla [bir şablonu dışarı aktarabilirsiniz](resource-manager-export-template.md). 

1. Şablonu bir kaynak grubundan dışarı aktardıysanız ayıklanan dosyaları VS Code’da açın.
   
    ![dosyaları göster](./media/resource-manager-vs-code/show-files.png)
2. Düzenleyebilmek için template.json dosyasını açın ve daha fazla kaynak ekleyin. `"resources": [` ifadesinden sonra enter tuşuna basarak yeni bir satır başlatın. **arm** yazarsanız bir seçenek listesi açılır. Bu seçenekler yüklediğiniz şablon parçacıklarıdır. 
   
    ![parçacıkları göster](./media/resource-manager-vs-code/type-snippets.png)
3. İstediğiniz parçacığı seçin. Bu makale için **arm-ip** öğesini seçerek yeni bir ortak IP adresi oluşturuyorum. Yeni oluşturulan kaynağın kapanan parantezinden `}` sonra bir virgül koyarak şablon söz diziminizin geçerli olduğundan emin olun.
   
     ![virgül ekle](./media/resource-manager-vs-code/add-comma.png)
4. VS Code’da yerleşik IntelliSense bulunur. Şablonlarınızı düzenlerken VS Code kullanılabilir değerler önerir. Örneğin, şablonunuza bir değişkenler bölümü eklemek için `""` (iki çift tırnak) ekleyin ve bu çift tırnakların arasında **Ctrl+Boşluk tuşuna** basın. **Değişkenler** dahil olmak üzere bazı seçenekler sunulur.
   
    ![değişken ekleme](./media/resource-manager-vs-code/add-variables.png)
5. IntelliSense, kullanılabilir değerler veya işlevler de önerebilir. Bir parametre değerine özellik ayarlamak için `"[]"` ve **Ctrl+Boşluk tuşu** ile bir ifade oluşturun. Bir işlevin adını yazarak başlayabilirsiniz. İstediğiniz işlevi bulduğunuzda **Tab** tuşunu seçin.
   
    ![parametre ekleme](./media/resource-manager-vs-code/select-parameters.png)
6. Şablonunuzdaki kullanılabilir parametrelerin listesini görmek için işlev içinde **Ctrl+Boşluk tuşu** birleşimine yeniden basın.
   
    ![parametre ekleme](./media/resource-manager-vs-code/select-avail-parameters.png)
7. Şablonunuzda şema doğrulama sorunları varsa düzenleyicide bilindik dalgalı çizgiler görürsünüz. **Ctrl+Shift+M** yazarak veya sol alttaki durum çubuğunda bulunan karakterleri seçerek hata ve uyarı listesini görüntüleyebilirsiniz.
   
    ![hatalar](./media/resource-manager-vs-code/errors.png)
   
    Şablonunuzun doğrulanması söz dizimi sorunlarını algılamanıza yardımcı olabilir; gözardı edebileceğiniz hatalar da görebilirsiniz. Bazı durumlarda düzenleyici, şablonunuzu güncel olmayan bir şema ile karşılaştırır ve bu nedenle doğru olduğunu bilseniz bile bir hata bildirir. Örneğin, Resource Manager’a kısa süre önce bir işlevin eklendiğini, ancak şemanın güncelleştirilmediğini varsayalım. Dağıtım sırasında işlevin doğru çalışmasına rağmen düzenleyici bir hata bildirir.
   
    ![hata iletisi](./media/resource-manager-vs-code/unrecognized-function.png)

## <a name="deploy-your-new-resources"></a>Yeni kaynaklarınızı dağıtma
Şablonunuz hazır olduğunda aşağıdaki yönergelerle yeni kaynakları dağıtabilirsiniz: 

### <a name="windows"></a>Windows
1. Bir PowerShell komut istemi açın 
2. Oturum açmak için şunu yazın: 
   
  ```powershell
  Login-AzureRmAccount
  ```

3. Birden çok aboneliğiniz varsa şununla aboneliklerin listesini alın:

  ```powershell 
  Get-AzureRmSubscription
  ```
   
    Ve kullanılacak aboneliği seçin.

  ```powershell
  Select-AzureRmSubscription -SubscriptionId <Subscription Id>
  ```

4. Parameters.json dosyanızdaki parametreleri güncelleştirin
5. Şablonunuzu Azure’da dağıtmak için Deploy.ps1 dosyasını çalıştırın

### <a name="osxlinux"></a>OSX/Linux
1. Bir terminal penceresi açın 
2. Oturum açmak için şunu yazın:

  ```azurecli
  azure login
  ```

3. Birden çok aboneliğiniz varsa doğru aboneliği seçmek için şunu yapın:

  ```azurecli
  azure account set <subscriptionNameOrId> 
  ```

4. Parameters.json dosyasındaki parametreleri güncelleştirin.
5. Şablonu dağıtmak için şunu çalıştırın:

  ```azurecli 
  azure group deployment create -f <PathToTemplate>
  ``` 

## <a name="next-steps"></a>Sonraki adımlar
* Şablonlar hakkında daha fazla bilgi edinmek için bkz. [Azure Resource Manager şablonları yazma](resource-group-authoring-templates.md).
* Şablon işlevleri hakkında bilgi edinmek için bkz. [Azure Resource Manager şablonu işlevleri](resource-group-template-functions.md).
* Visual Studio Code ile çalışmaya ilişkin daha fazla örnek için [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 Connect [tanıtımında](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/) yer alan [Visual Studio Code ile bulut uygulamaları derleme](https://github.com/Microsoft/HealthClinic.biz/wiki/Build-cloud-apps-with-Visual-Studio-Code) bölümüne bakın. HealthClinic.biz tanıtımından daha fazla hızlı başlangıç ipuçları için bkz. [Azure Geliştirici Araçları Hızlı Başlangıç İpuçları](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).




<!--HONumber=Jan17_HO1-->


