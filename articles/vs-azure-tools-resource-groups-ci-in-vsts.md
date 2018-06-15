---
title: VS Team Services Azure kaynak grubu projeleri kullanarak sürekli tümleştirme | Microsoft Docs
description: Visual Studio Team Services sürekli tümleştirme Visual Studio'daki Azure kaynak grubu dağıtım projeleri kullanılarak nasıl ayarlanacağını açıklar.
services: visual-studio-online
documentationcenter: na
author: mlearned
manager: erickson-doug
editor: ''
ms.assetid: b81c172a-be87-4adc-861e-d20b94be9e38
ms.service: azure-resource-manager
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/01/2016
ms.author: mlearned
ms.openlocfilehash: fc5a45c899cd72c051dd08f7db039565a57381a7
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32192953"
---
# <a name="continuous-integration-in-visual-studio-team-services-using-azure-resource-group-deployment-projects"></a>Azure kaynak grubu dağıtım projeleri kullanarak Visual Studio Team Services sürekli tümleştirme
Bir Azure şablonu dağıtmak için çeşitli aşamalarda görevleri: Azure derleme, Test, kopyalama ("Hazırlama" da denir) ve şablon dağıtın. Visual Studio Team Services (VS Team Services) şablonları dağıtmak için iki farklı yolu vardır. Yöntemlerin her ikisi de aynı sonuçları sağlamak, bu nedenle, iş akışınızı en uygun olanı seçin.

1. Azure kaynak grubu dağıtım projesi (dağıtma-AzureResourceGroup.ps1) dahil PowerShell komut dosyasını çalıştırır yapı tanımınız için tek bir adım ekleyin. Betik yapıları kopyalar ve şablon dağıtır.
2. Birden çok VS Team Services adımlar, her bir aşama görevi gerçekleştirme derleme ekleyin.

Bu makalede her iki seçenek gösterilmektedir. İlk seçenek, geliştiriciler Visual Studio ve yaşam döngüsü boyunca sağlayan tutarlılık tarafından kullanılan aynı komut dosyası kullanma avantajına sahiptir. İkinci seçenek yerleşik komut dosyası için uygun bir alternatif sunar. Her iki yordamı, VS Team Services içinde kullanıma Visual Studio dağıtım projesi zaten olduğunu varsayın.

## <a name="copy-artifacts-to-azure"></a>Yapıları Azure'a kopyalama
Şablon dağıtımı için gerekli olan herhangi bir yapı varsa senaryo bağımsız olarak, Azure Resource Manager erişim onlara vermeniz gerekir. Bu yapıtların dosyaları gibi içerebilir:

* İç içe geçmiş şablonları
* Yapılandırma ve DSC komut dosyaları
* Uygulama ikili dosyaları

### <a name="nested-templates-and-configuration-scripts"></a>İç içe geçmiş şablonları ve yapılandırma betikleri
Visual Studio tarafından sağlanan şablonları kullandığınızda (veya Visual Studio parçacıkları ile oluşturulmuş), PowerShell Betiği yalnızca yapıları aşamaları, farklı dağıtımları için kaynakları URI'sini de parameterizes. Komut dosyası sonra Azure güvenli bir kapsayıcıda yapıları kopyalar, bu kapsayıcı için bir SaS belirteci oluşturur ve bu bilgileri şablon dağıtımı açın geçirir. Bkz: [şablon dağıtımı oluşturmak](https://msdn.microsoft.com/library/azure/dn790564.aspx) iç içe geçmiş şablonları hakkında daha fazla bilgi için.  Görevler VS Team Services içinde kullanırken, şablon dağıtımınız için uygun görevleri seçin ve gerekirse, parametre değerlerini hazırlama adımından şablon dağıtımı geçirin.

## <a name="set-up-continuous-deployment-in-vs-team-services"></a>VS Team Services sürekli dağıtım ayarlayın
PowerShell Betiği VS Team Services içinde çağırmak için derleme tanımını güncelleştirmeniz gerekir. Kısaca, adımlar şunlardır: 

1. Yapı tanımı düzenleyin.
2. VS Team Services Azure yetkilendirme ayarlayın.
3. Azure kaynak grubu dağıtım projesi PowerShell Betiği başvuruda bulunan bir Azure PowerShell derleme adımı ekleyin.
4. Değerini *- ArtifactsStagingDirectory* VS Team Services içinde yerleşik bir proje ile çalışmak için parametre.

### <a name="detailed-walkthrough-for-option-1"></a>Seçenek 1 için ayrıntılı kılavuz
Aşağıdaki yordamlar, VS Team Services projenizde PowerShell Betiği çalıştıran tek bir görev kullanarak sürekli dağıtım yapılandırmak için gereken adımlarda size yol. 

1. VS Team Services yapı tanımınızı düzenleyin ve bir Azure PowerShell derleme adımı ekleyin. Yapı tanımı altında seçin **yapı tanımları** kategori ve ardından **Düzenle** bağlantı.
   
   ![Yapı tanımı düzenleme][0]
2. Yeni Ekle **Azure PowerShell** derleme tanımını adıma oluşturmak ve ardından **derleme adımı Ekle...** tıklayın.
   
   ![Derleme adımı ekleme][1]
3. Seçin **dağıtma görev** kategorisi, select **Azure PowerShell** görev ve ardından kendi **Ekle** düğmesi.
   
   ![Görev ekleme][2]
4. Seçin **Azure PowerShell** adım oluşturma ve değerlerini doldurun.
   
   1. VS Team Services için eklenen bir Azure hizmet uç nokta zaten varsa, abonelikte seçin **Azure aboneliği** aşağı açılan liste kutusunu ve ardından sonraki bölüme geçin. 
      
      VS Team Services içinde bir Azure hizmet uç noktası sahip değilseniz, birini eklemeniz gerekir. Bu alt bölümde işlemi boyunca sürer. Azure hesabınızda bir Microsoft hesabı (örneğin, Hotmail) kullanıyorsa, bir hizmet asıl kimlik doğrulaması almak için aşağıdaki adımları izlemelisiniz.
   2. Seçin **Yönet** bağlantısına **Azure aboneliği** aşağı açılan liste kutusu.
      
      ![Azure Aboneliklerini yönetmek][3]
   3. Seçin **Azure** içinde **yeni hizmet uç noktası** aşağı açılan liste kutusu.
      
      ![Yeni hizmet uç noktası][4]
   4. İçinde **Azure aboneliği Ekle** iletişim kutusunda **hizmet sorumlusu** seçeneği.
      
      ![Hizmet asıl seçeneği][5]
   5. Azure abonelik bilgilerinizi ekleyin **Azure aboneliği Ekle** iletişim kutusu. Aşağıdaki öğeler sağlamanız gerekir:
      
      * Abonelik Kimliği
      * Abonelik Adı
      * Hizmet sorumlusu kimliği
      * Hizmet sorumlusu anahtarı
      * Kiracı Kimliği
   6. Tercih ettiğiniz bir ad eklemek **abonelik** adı kutusu. Bu değer daha sonra da görünür **Azure aboneliği** VS Team Services aşağı açılan listesinde. 
   7. Azure abonelik Kimliğinizi bilmiyorsanız, bunu almak için aşağıdaki komutlardan birini kullanabilirsiniz.
      
      PowerShell komut dosyaları için kullanın:
      
      `Get-AzureRmSubscription`
      
      Azure CLI için şunu kullanın:
      
      `azure account show`
   8. Bir hizmet asıl kimliği, hizmet asıl anahtar ve Kiracı kimliği izleme yordamı alabilmeniz için [oluşturma Active Directory uygulaması ve hizmet sorumlusu portal kullanarak](resource-group-create-service-principal-portal.md) veya [bir hizmet sorumlusu Azure Resource Manager ile kimlik doğrulaması](resource-group-authenticate-service-principal.md).
   9. Hizmet sorumlusu kimliği, hizmet asıl anahtar ve Kiracı kimliği değerler ekleyin **Azure aboneliği Ekle** iletişim kutusuna ve ardından **Tamam** düğmesi.
      
      Artık Azure PowerShell betiğini çalıştırmak için kullanılacak geçerli bir hizmet sorumlusu var.
5. Yapı tanımı düzenleyin ve seçin **Azure PowerShell** adım oluşturma. Abonelik seç **Azure aboneliği** aşağı açılan liste kutusu. (Abonelik görünmüyorsa, seçin **yenileme** düğmesini sonraki **Yönet** bağlantıyı.) 
   
   ![Azure PowerShell derleme görevi yapılandırın][8]
6. Dağıtma-AzureResourceGroup.ps1 PowerShell komut dosyası için bir yol belirtin. Bunu yapmak için üç nokta (...) düğmesini seçin **betik yolu** kutusunda, Dağıt AzureResourceGroup.ps1 PowerShell Betiği gidin **betikleri** projenizin klasörü seçin ve ardından **Tamam** düğmesi.    
   
   ![Komut dosyası yolunu seçin][9]
7. Komut dosyası seçtikten sonra Build.StagingDirectory çalışabilmesi yolu komut dosyasına güncelleştirmeniz (aynı dizinde, *ArtifactsLocation* ayarlanır). "$(Build.StagingDirectory)/"betik yolu başlangıcına. ekleyerek bunu yapabilirsiniz
   
    ![Komut dosyası yolu Düzenle][10]
8. İçinde **komut dosyası değişkenleri** kutusunda, aşağıdaki parametre (tek bir satırı) girin. Visual Studio komut dosyasını çalıştırdığınızda, VS parametrelerinde nasıl kullandığını görebilirsiniz **çıkış** penceresi. Bu yapı adımda parametre değerlerini ayarlamak için bir başlangıç noktası olarak kullanabilirsiniz.
   
   | Parametre | Açıklama |
   | --- | --- |
   | -ResourceGroupLocation |Kaynak grubu bulunduğu, gibi coğrafi konum değeri **eastus** veya **'Doğu ABD'**. (Tek tırnak adı bir boşluk ise ekleyin.) Bkz: [Azure bölgeleri](https://azure.microsoft.com/regions/) daha fazla bilgi için. |
   | -ResourceGroupName |Bu dağıtım için kullanılan kaynak grubunun adı. |
   | -UploadArtifacts |Bu parametre, varsa, belirleyen Azure'a yerel sistemden karşıya gerek yapıları. Yalnızca şablon dağıtımı (örneğin, yapılandırma komut dosyaları veya iç içe geçmiş şablonlarını) PowerShell Betiği kullanılarak hazırlamak istediğiniz ek yapıları gerektiriyorsa, bu anahtarı ayarlamanız gerekir. |
   | -StorageAccountName |Bu dağıtım için aşama yapıları için kullanılan depolama hesabı adı. Yapıtlar dağıtım için hazırlama varsa bu parametre yalnızca kullanılır. Bu parametre belirtilirse, önceki dağıtım sırasında bir betik oluşturmadı değilse yeni bir depolama hesabı oluşturulur. Parametresi belirtilirse, depolama hesabı önceden var olmalıdır. |
   | -StorageAccountResourceGroupName |Depolama hesabıyla ilişkili kaynak grubunun adı. StorageAccountName parametresi için bir değer belirtirseniz, bu parametre gereklidir. |
   | -TemplateFile |Azure kaynak grubu dağıtım projesi şablon dosyasında yolu. Esneklik geliştirmek için mutlak bir yol yerine PowerShell komut dosyası konumunu görelidir Bu parametre için bir yol kullanın. |
   | -TemplateParametersFile |Azure kaynak grubu dağıtım projesi Parametreler dosyasında yolu. Esneklik geliştirmek için mutlak bir yol yerine PowerShell komut dosyası konumunu görelidir Bu parametre için bir yol kullanın. |
   | -ArtifactStagingDirectory |Bu parametre, komut dosyası, projenin ikili dosyaları nerede kopyalanacağı klasörü bilmeniz PowerShell olanak sağlar. Bu değer PowerShell betiği tarafından kullanılan varsayılan değerini geçersiz kılar. VS Team Services kullanmak için değeri ayarlayın: - ArtifactStagingDirectory $(Build.StagingDirectory) |
   
   Bir komut bağımsız değişkenleri örneğin (okunabilirlik için ayrılmış çizgi) aşağıdadır:
   
   ```    
   -ResourceGroupName 'MyGroup' -ResourceGroupLocation 'eastus' -TemplateFile '..\templates\azuredeploy.json' 
   -TemplateParametersFile '..\templates\azuredeploy.parameters.json' -UploadArtifacts -StorageAccountName 'mystorageacct' 
   –StorageAccountResourceGroupName 'Default-Storage-EastUS' -ArtifactStagingDirectory '$(Build.StagingDirectory)'    
   ```
   
   İşiniz bittiğinde **komut dosyası değişkenleri** kutusu aşağıdaki listede benzer:
   
   ![Betik bağımsız değişkenleri][11]
9. Azure PowerShell derleme adımı için gerekli olan tüm öğelerin ekledikten sonra seçin **sıra** Projeyi derlemek için düğmesi derleme. **Yapı** ekran PowerShell Betiği çıktısını gösterir.

### <a name="detailed-walkthrough-for-option-2"></a>Seçenek 2 için ayrıntılı kılavuz
Aşağıdaki yordamlar, yerleşik görevlerini kullanarak VS Team Services içinde sürekli dağıtımı yapılandırmak için gereken adımlarda size yol.

1. İki yeni derleme adımları eklemek için VS Team Services yapı tanımı düzenleyin. Yapı tanımı altında seçin **yapı tanımları** kategori ve ardından **Düzenle** bağlantı.
   
   ![Yapı tanımı düzenleme][12]
2. Yapı tanımı kullanılarak için yeni derleme adımları ekleme **derleme adımı Ekle...** tıklayın.
   
   ![Derleme adımı ekleme][13]
3. Seçin **dağıtma** görev kategorisi, select **Azure dosya kopyalama** görev ve ardından kendi **Ekle** düğmesi.
   
   ![Azure dosya kopyalama görevi][14]
4. Seçin **Azure kaynak grubu dağıtımı** görev'ı seçin, **Ekle** düğmesine ve ardından **Kapat** **görev katalog**.
   
   ![Azure kaynak grubu dağıtımı görev ekleme][15]
5. Seçin **Azure dosya kopyalama** görev ve değerlerini doldurun.
   
   VS Team Services için eklenen bir Azure hizmet uç nokta zaten varsa, abonelikte seçin **Azure aboneliği** aşağı açılan liste kutusu. Bir abonelik yoksa bkz [seçeneği 1](#detailed-walkthrough-for-option-1) bir VS Team Services içinde ayarlama hakkında yönergeler için.
   
   * Kaynak - girin **$(Build.StagingDirectory)**
   * Azure bağlantı türü - seçin **Azure Resource Manager**
   * Azure RM abonelik - abonelik için kullanmak istediğiniz depolama hesabını seçin **Azure aboneliği** aşağı açılan liste kutusu. Abonelik görünmüyorsa, seçin **yenileme** düğmesini sonraki **Yönet** bağlantı.
   * Hedef türü - seçin **Azure Blob**
   * Depolama hesabı RM depolama hesabı - seçin yapıları hazırlama için kullanmak istediğiniz
   * Kapsayıcı adı - hazırlama için; kullanmak istediğiniz kapsayıcının adını girin herhangi bir geçerli kapsayıcı adı olabilir ancak bu derleme tanımı için ayrılmış kullanın
   
   Çıkış değerleri için:
   
   * Depolama kapsayıcısı URI - girin **artifactsLocation**
   * Depolama kapsayıcısı SAS belirteci - girin **artifactsLocationSasToken**
   
   ![Azure dosya kopyalama görevi yapılandırmak][16]
6. Seçin **Azure kaynak grubu dağıtımı** adım oluşturma ve değerlerini doldurun.
   
   * Azure bağlantı türü - seçin **Azure Resource Manager**
   * Azure RM abonelik - dağıtım için abonelik Seç **Azure aboneliği** aşağı açılan liste kutusu. Bu genellikle önceki adımda kullanılan aynı abonelik olacaktır
   * Eylem - select **oluşturma veya güncelleştirme kaynak grubu**
   * Kaynak grubu - bir kaynak grubu seçin veya yeni bir kaynak grubu dağıtımı için bir ad girin
   * Konumu - kaynak grubu konumunu seçin
   * Şablon - dağıtılacak şablonunun adını ve yolunu girin eklemeli **$(Build.StagingDirectory)**, örneğin: **$(Build.StagingDirectory/DSC-CI/azuredeploy.json)**
   * Şablon parametreleri - girin kullanılacak parametre adı ve yolu eklenmesini **$(Build.StagingDirectory)**, örneğin: **$(Build.StagingDirectory/DSC-CI/azuredeploy.parameters.json)**
   * Şablon parametreleri geçersiz kıl - girin veya aşağıdaki kodu kopyalayıp yapıştırın:
     
     ```    
     -_artifactsLocation $(artifactsLocation) -_artifactsLocationSasToken (ConvertTo-SecureString -String "$(artifactsLocationSasToken)" -AsPlainText -Force)
     ```
     ![Azure kaynak grubu dağıtım görev yapılandırın][17]
7. Gerekli tüm öğelerin ekledikten sonra derleme tanımını kaydedin ve seçin **sıraya yeni derleme** üstünde.

## <a name="next-steps"></a>Sonraki adımlar
Okuma [Azure Resource Manager'a genel bakış](azure-resource-manager/resource-group-overview.md) Azure Resource Manager ve Azure kaynak grupları hakkında daha fazla bilgi edinmek için.

[0]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough1.png
[1]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough2.png
[2]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough3.png
[3]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough4.png
[4]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough5.png
[5]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough6.png
[8]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough9.png
[9]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough10.png
[10]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough11b.png
[11]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough12.png
[12]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough13.png
[13]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough14.png
[14]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough15.png
[15]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough16.png
[16]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough17.png
[17]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough18.png
