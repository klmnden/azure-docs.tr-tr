---
title: Visual Studio Azure kaynak grubu projeleri dağıtma & Oluştur
description: Bir Azure kaynak grubu projesi oluşturma ve kaynakları Azure'a dağıtmak için Visual Studio'yu kullanın.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: quickstart
ms.date: 06/20/2019
ms.author: tomfitz
ms.openlocfilehash: 8677d906375853bdde5c192c86dacc7479f2e31e
ms.sourcegitcommit: 5cb0b6645bd5dff9c1a4324793df3fdd776225e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67311132"
---
# <a name="creating-and-deploying-azure-resource-groups-through-visual-studio"></a>Visual Studio aracılığıyla Azure kaynak grupları oluşturma ve dağıtma

Visual Studio ile altyapınızı ve kodlarınızı Azure’a dağıtan bir proje oluşturabilirsiniz. Örneğin, web ana bilgisayarı, web sitesi ve web sitesi için kod da dağıtabilirsiniz. Visual Studio genelde karşılaşılan senaryoların dağıtılması için birçok farklı başlangıç şablonu sağlar. Bu makalede, bir web uygulaması dağıtın.  

Bu makalede nasıl kullanılacağını gösterir [Visual Studio 2019 veya üzeri yüklü ASP.NET iş yükleri ve Azure geliştirme](/visualstudio/install/install-visual-studio?view=vs-2019). Visual Studio 2017'yi kullanıyorsanız, deneyiminiz büyük ölçüde aynıdır.

## <a name="create-azure-resource-group-project"></a>Azure Kaynak Grubu projesi oluşturma

Bu bölümde, bir Azure kaynak grubu projesi ile oluşturduğunuz bir **Web uygulaması** şablonu.

1. Visual Studio'da **dosya**, **yeni**, ve **proje**. Seçin **Azure kaynak grubu** proje şablonu ve **sonraki**.

    ![Proje oluşturma](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-project.png)

1. Projenize bir ad verin. Ayarları, büyük olasılıkla bir sakınca yoktur, ancak bunları yapmak için gözden diğer varsayılan ortamınız için çalışırlar. İşiniz bittiğinde **Oluştur**’u seçin.

    ![Proje oluşturma](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/name-project.png)

1. Azure Resource Manager’da dağıtmak istediğiniz şablonu seçin. Dağıtmak istediğiniz proje türüne bağlı olarak çok sayıda farklı seçeneğiniz olduğunu unutmayın. Bu makalede seçin **Web uygulaması** şablon ve **Tamam**.

    ![Şablon seçme](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-project.png)

    Seçtiğiniz şablon sadece başlangıçtır; senaryonuzun gereksinimlerini karşılamak üzere kaynak ekleyebilir ve kaldırabilirsiniz.

1. Visual Studio, web uygulaması için bir kaynak grubu dağıtım projesi oluşturur. Projeniz için dosyaları görmek için dağıtım projesindeki düğümlere bakın.

    ![düğümleri gösterme](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-items.png)

    Web uygulaması şablonunu seçtiğiniz olduğundan aşağıdaki dosyaları göreceksiniz:

   | Dosya adı | Açıklama |
   | --- | --- |
   | Deploy-AzureResourceGroup.ps1 |Azure Resource Manager’da dağıtılacak PowerShell komutlarını çalıştıran PowerShell betiği. Visual Studio, şablonunuzu dağıtmak için bu PowerShell Betiği kullanır. |
   | WebSite.json |Azure’da dağıtmak istediğiniz altyapıyı tanımlayan Resource Manager şablonu ve dağıtım sırasında sağlayabileceğiniz parametreler. Resource Manager’ın kaynakları doğru sırayla dağıtmasını sağlamak için kaynaklarınız arasındaki bağımlılıkları da tanımlar. |
   | WebSite.parameters.json |Şablon tarafından gereken değerleri içeren bir parametre dosyası. Her bir dağıtımı özelleştirmek için parametre değerlerini geçirirsiniz. |

    Tüm kaynak grubu dağıtım projeleri bu temel dosyaları içerir. Diğer projeler diğer işlevleri desteklemek için ek dosyalar içerebilir.

## <a name="customize-resource-manager-template"></a>Resource Manager şablonunu özelleştirme

Bir dağıtım projesi, dağıtmak istediğiniz kaynakları tanımlayan Resource Manager şablonunu değiştirerek özelleştirebilirsiniz. Resource Manager şablonu bileşenleri hakkında daha fazla bilgi edinmek için [Azure Resource Manager şablonları yazma](resource-group-authoring-templates.md).

1. Şablonunuzda çalışmak için açın **WebSite.json**.

1. Visual Studio düzenleyicisi, Resource Manager şablonu düzenleme konusunda size yardımcı olan araçlar sağlar. **JSON Ana Hattı** penceresi, şablonunuzda tanımlanan bileşenleri görmenizi kolaylaştırır.

   ![JSON ana hattı Göster](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-json-outline.png)

1. Şablon kısmına gitmek için anahat içinde bir öğe seçin.

   ![JSON'a gitme](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/navigate-json.png)

1. JSON Ana Hattı penceresinin üst tarafında bulunan **Kaynak Ekle** düğmesini seçerek veya **kaynaklar**’a sağ tıklayıp **Yeni Kaynak Ekle**’yi seçerek yeni kaynak ekleyebilirsiniz.

   ![Kaynak ekle](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-resource.png)

1. Seçin **depolama hesabı** ve bir ad verin. 11 karakterden uzun olmayan ve yalnızca sayı ile küçük harf içeren bir ad belirtin.

   ![Depolama ekleme](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-storage.png)

1. Yalnızca kaynak eklenmediğini, aynı zamanda depolama hesabı türü için bir parametre ve depolama hesabı adı bir değişken eklendiğini unutmayın.

   ![Ana hattı Göster](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-new-items.png)

1. Depolama hesabı türü parametresi izin verilen türler ve varsayılan tür ile önceden tanımlanmış olmalıdır. Bu değerleri bırakabilir veya senaryonuz için düzenleyebilirsiniz. Bu şablon aracılığıyla herkesin **Premium_LRS** depolama hesabı dağıtmasını istemiyorsanız izin verilen türlerden bunu kaldırın.

   ```json
   "demoaccountType": {
     "type": "string",
     "defaultValue": "Standard_LRS",
     "allowedValues": [
       "Standard_LRS",
       "Standard_ZRS",
       "Standard_GRS",
       "Standard_RAGRS"
     ]
   }
   ```

1. Visual Studio, şablonu düzenlerken kullanılabilir özelliklerini anlamanıza yardımcı olması için IntelliSense de sağlar. Örneğin, App Service planınızın özelliklerini düzenlemek için **HostingPlan** kaynağına gidin ve **resources** için bir değer ekleyin. IntelliSense’in kullanılabilir değerleri gösterdiğini ve bu değerler için bir açıklama sunduğunu unutmayın.

   ![IntelliSense'i Göster](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-intellisense.png)

   Ayarlayabileceğiniz **numberOfWorkers** 1 ve dosyayı kaydedin.

   ```json
   "properties": {
     "name": "[parameters('hostingPlanName')]",
     "numberOfWorkers": 1
   }
   ```

1. Açık **WebSite.parameters.json** dosya. Parametre dosyasını, dağıtım sırasında dağıtılan kaynak özelleştirme değerleri geçirmek için kullanın. Barındırma planı bir ad verin ve dosyayı kaydedin.

   ```json
   {
     "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
     "contentVersion": "1.0.0.0",
     "parameters": {
       "hostingPlanName": {
         "value": "demoHostPlan"
       }
     }
   }
   ```

## <a name="deploy-project-to-azure"></a>Projeyi Azure'a dağıtma

Şimdi bir kaynak grubu için projenizi dağıtmaya hazırsınız.

Varsayılan olarak, AzureRM modülünü projedeki PowerShell Betiği (AzureResourceGroup.ps1 dağıtma) kullanır. Yine de AzureRM Modülü yüklü ve kullanmaya devam etmek istiyorsanız, bu varsayılan komut dosyasını kullanabilirsiniz. Bu betik ile çözümünüzü dağıtmak için Visual Studio arabirimini kullanabilirsiniz.

Ancak, yeni Project.json'dan packagereference'a varsa [Az modül](/powershell/azure/new-azureps-module-az), projenize yeni bir betik eklemeniz gerekir. Az modül kullanan bir komut dosyası eklemek için Kopyala [Dağıt AzTemplate.ps1](https://github.com/Azure/azure-quickstart-templates/blob/master/Deploy-AzTemplate.ps1) komut dosyası ve projenize ekleyin. Bu betik, dağıtım için kullanmak için bunu bir PowerShell Konsolu yerine Visual Studio'nun dağıtım arabirimini kullanarak çalıştırmanız gerekir.

Bu makalede her iki yaklaşım gösterilmektedir. Bu makalede AzureRM modülü betik olarak varsayılan betik ve Az modül betik olarak yeni betik ifade eder.

### <a name="az-module-script"></a>Az modül betiği

Az modül betiği için bir PowerShell konsolu açın ve çalıştırın:

```powershell
.\Deploy-AzTemplate.ps1 -ArtifactStagingDirectory . -Location centralus -TemplateFile WebSite.json -TemplateParametersFile WebSite.parameters.json
```

### <a name="azurerm-module-script"></a>AzureRM modülü betiği

AzureRM modülü betik için Visual Studio kullanın:

1. Dağıtım proje düğümünün kısayol menüsünde **Dağıt** > **Yeni** seçeneklerini belirleyin.

    ![Yeni dağıtım menü öğesi](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/deploy.png)

1. **Kaynak Grubuna Dağıt** iletişim kutusu görüntülenir. **Kaynak Grubu** açılır kutusunda, mevcut bir kaynak grubu seçin veya yeni bir tane oluşturun. **Dağıt**'ı seçin.

    ![Kaynak grubu iletişim kutusu için dağıtma](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployment.png)

1. **Çıktı** pencerelerinde dağıtımın durumunu görürsünüz. Dağıtım tamamlandığında son ileti aşağıdakine benzer bir ifadeyle dağıtımın başarılı olduğunu belirtir:

   ```output
   18:00:58 - Successfully deployed template 'website.json' to resource group 'ExampleAppDeploy'.
   ```

## <a name="view-deployed-resources"></a>Dağıtılan kaynakları görüntüle

Şimdi sonuçlarını denetleyin.

1. Bir tarayıcıda [Azure portal](https://portal.azure.com/)’ı açın ve hesabınızda oturum açın. Kaynak grubunu görmek için **Kaynak grupları**’nu ve dağıttığınız kaynak grubunu seçin.

1. Dağıtılan tüm kaynakları görürsünüz. Depolama hesabı adının, ilgili kaynağı eklerken belirttiğiniz adla tam olarak aynı olmadığına dikkat edin. Depolama hesabı benzersiz olmalıdır. Şablon, benzersiz bir ad oluşturmak için belirttiğiniz ada otomatik olarak bir karakter dizesi ekler.

    ![Kaynakları Göster](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployed-resources.png)

## <a name="add-code-to-project"></a>Proje için kod ekleyin

Bu noktada, uygulamanız için altyapı dağıttınız, ancak proje ile dağıtılan gerçek bir kod yoktur.

1. Visual Studio çözümünüze bir proje ekleyin. Çözüme sağ tıklayın ve **Ekle** > **Yeni Proje** öğesini seçin.

    ![Proje ekleme](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-project.png)

1. Ekleme bir **ASP.NET Core Web uygulaması**.

    ![Web uygulaması Ekle](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-app.png)

1. Web uygulamanıza bir ad verin ve seçin **Oluştur**.

    ![Web uygulaması adı](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/name-web-app.png)

1. Seçin **Web uygulaması** ve **oluşturma**.

    ![Web uygulaması seçin](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-project-type.png)

1. Visual Studio web uygulamanızı oluşturduktan sonra her iki projeyi de çözümde görürsünüz.

    ![projeleri göster](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-projects.png)

1. Bundan böyle kaynak grubu projenizin yeni projeyi tanıdığından emin olmanız gerekir. Kaynak grubu projenize (ExampleAppDeploy) geri dönün. **Başvurular**’a sağ tıklayın ve **Başvuru Ekle**’yi seçin.

    ![Başvuru ekleme](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-new-reference.png)

1. Oluşturduğunuz web uygulaması projesini seçin.

   ![Başvuru ekleme](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-reference.png)

   Bir başvuru ekleyerek, web uygulama projesini kaynak grubu projesine bağlar ve otomatik olarak bazı özelliklerini ayarlar. Bu özellikleri başvurunun **Özellikler** penceresinde görürsünüz. **Dosya Yolu Ekle** paketin oluşturulduğu yerin yolunu içerir. Klasörü (ExampleApp) ve dosyayı (package.zip) not edin. Uygulamayı dağıtırken parametre olarak ileteceğiniz için bu değerleri bilmeniz gerekir.

   ![başvurusuna bakın](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/see-reference.png)

1. Şablonunuza (WebSite.json) geri dönün ve şablona bir kaynak ekleyin.

    ![Kaynak ekle](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-resource-2.png)

1. Bu kez **Web Apps için Web Dağıtımı**’nı seçin. 

    ![Web ekleme dağıtma](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-web-deploy.png)

   Şablonunuzu kaydedin.

1. Şablonunuza bazı yeni parametre yok. Bunlar, önceki adımda eklendi. Değer sağlamanız gerekmez **_artifactsLocation** veya **_artifactsLocationSasToken** çünkü bu değerleri otomatik olarak oluşturulur. Ancak, klasör ve dosya adını dağıtım paketini içeren yola ayarlamanız gerekir. Bu parametre adları ile bitemez **PackageFolder** ve **PackageFileName**. Adın ilk kısmı eklediğiniz Web dağıtımı kaynak adıdır. Bu makalede, bunlar adlı **ExampleAppPackageFolder** ve **ExampleAppPackageFileName**. 

   Açık **Website.parameters.json** ve başvuru özelliklerinde gördüğünüz değerleri bu parametrelerin ayarlayın. Ayarlama **ExampleAppPackageFolder** klasörün adı. Ayarlama **ExampleAppPackageFileName** ZIP dosyasının adı.

   ```json
   {
     "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
     "contentVersion": "1.0.0.0",
     "parameters": {
       "hostingPlanName": {
         "value": "demoHostPlan"
       },
       "ExampleAppPackageFolder": {
         "value": "ExampleApp"
       },
       "ExampleAppPackageFileName": {
         "value": "package.zip"
       }
     }
   }
   ```

## <a name="deploy-code-with-infrastructure"></a>Altyapı ile kod dağıtma

Proje kodunu eklenmiş olduğunuzdan, dağıtımınız bu kez biraz farklıdır. Dağıtım sırasında Resource Manager'ın erişebileceği bir yere projeniz için yapıtları hazırlama. Yapıtlar bir depolama hesabı için hazırlanmıştır.

### <a name="az-module-script"></a>Az modül betiği

Bir küçük değişiklik Az modül betiği kullanıyorsanız, şablonunuza yapmanıza gerek yoktur. Bu betik bir eğik çizgi yapıtları konumuna ekler, ancak şablonunuzu bu eğik çizgi gerekmiyor. WebSite.json açın ve MSDeploy uzantısının özelliklerini bulun. Adlı bir özellik olan **packageUri**. Yapıtları konumu ve paket klasörünü arasında eğik çizgi kaldırın.

Gibi görünmelidir:

```json
"packageUri": "[concat(parameters('_artifactsLocation'), parameters('ExampleAppPackageFolder'), '/', parameters('ExampleAppPackageFileName'), parameters('_artifactsLocationSasToken'))]",
```

Bildirim yok önceki örnekte hiçbir `'/',` arasında **parameters('_artifactsLocation')** ve **parameters('ExampleAppPackageFolder')** .

Projeyi yeniden derleyin. Proje derleme dağıtmak için gereken dosyaları hazırlama klasörüne eklenen emin olur.

Artık, bir PowerShell konsolu açın ve çalıştırın:

```powershell
.\Deploy-AzTemplate.ps1 -ArtifactStagingDirectory .\bin\Debug\staging\ExampleAppDeploy -Location centralus -TemplateFile WebSite.json -TemplateParametersFile WebSite.parameters.json -UploadArtifacts -StorageAccountName <storage-account-name>
```

### <a name="azurerm-module-script"></a>AzureRM modülü betiği

AzureRM modülü betik için Visual Studio kullanın:

1. Yeniden dağıtmak için seçin **Dağıt**ve daha önce dağıttığınız kaynak grubunu.

    ![Proje yeniden dağıtma](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/redeploy.png)

1. Bu kaynak grubu için Dağıtılmış depolama hesabını seçin **Yapıt depolama hesabı**.

   ![Web yeniden dağıtma](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/redeploy-web-app.png)

## <a name="view-web-app"></a>Görünüm web uygulaması

1. Dağıtım tamamlandıktan sonra portalda web uygulamanızı seçin. Yeni siteye göz atmak için URL’yi seçin.

   ![Site Gözat](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/browse-site.png)

1. Varsayılan ASP.NET uygulamasını başarıyla dağıttığınızdan emin olun.

   ![Dağıtılmış uygulamayı gösterme](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployed-app.png)

## <a name="add-operations-dashboard"></a>İşlem Panosu ekleme

Yalnızca Visual Studio arabirimi aracılığıyla kullanılabilir olan kaynaklarla sınırlı olmazsınız. Şablonunuza özel bir kaynak ekleyerek dağıtımınızı özelleştirebilirsiniz. Kaynak eklemeyi göstermek için dağıttığınız kaynağı yönetmek üzere bir işlem panosu eklersiniz.

1. WebSite.json dosyasını açın ve sonra depolama hesabı kaynağı ancak kapatmadan önce aşağıdaki JSON ekleyin `]` Kaynaklar bölümünün.

   ```json
    ,{
      "properties": {
        "lenses": {
          "0": {
            "order": 0,
            "parts": {
              "0": {
                "position": {
                  "x": 0,
                  "y": 0,
                  "colSpan": 4,
                  "rowSpan": 6
                },
                "metadata": {
                  "inputs": [
                    {
                      "name": "resourceGroup",
                      "isOptional": true
                    },
                    {
                      "name": "id",
                      "value": "[resourceGroup().id]",
                      "isOptional": true
                    }
                  ],
                  "type": "Extension/HubsExtension/PartType/ResourceGroupMapPinnedPart"
                }
              },
              "1": {
                "position": {
                  "x": 4,
                  "y": 0,
                  "rowSpan": 3,
                  "colSpan": 4
                },
                "metadata": {
                  "inputs": [],
                  "type": "Extension[azure]/HubsExtension/PartType/MarkdownPart",
                  "settings": {
                    "content": {
                      "settings": {
                        "content": "__Customizations__\n\nUse this dashboard to create and share the operational views of services critical to the application performing. To customize simply pin components to the dashboard and then publish when you're done. Others will see your changes when you publish and share the dashboard.\n\nYou can customize this text too. It supports plain text, __Markdown__, and even limited HTML like images <img width='10' src='https://portal.azure.com/favicon.ico'/> and <a href='https://azure.microsoft.com' target='_blank'>links</a> that open in a new tab.\n",
                        "title": "Operations",
                        "subtitle": "[resourceGroup().name]"
                      }
                    }
                  }
                }
              }
            }
          }
        },
        "metadata": {
          "model": {
            "timeRange": {
              "value": {
                "relative": {
                  "duration": 24,
                  "timeUnit": 1
                }
              },
              "type": "MsPortalFx.Composition.Configuration.ValueTypes.TimeRange"
            }
          }
        }
      },
      "apiVersion": "2015-08-01-preview",
      "name": "[concat('ARM-',resourceGroup().name)]",
      "type": "Microsoft.Portal/dashboards",
      "location": "[resourceGroup().location]",
      "tags": {
        "hidden-title": "[concat('OPS-',resourceGroup().name)]"
      }
    }
   ```

1. Projenizi yeniden dağıtın.

1. Dağıtım tamamlandıktan sonra portalda panonuzu görüntüleyin. Seçin **Pano** ve, dağıtılmış bir tanesini seçin.

   ![Özel Pano](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/view-custom-dashboards.png)

1. Özelleştirilmiş Pano görürsünüz.

   ![Özel Pano](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/Ops-DemoSiteGroup-dashboard.png)

RBAC gruplarını kullanarak panoya erişimi yönetebilirsiniz. Ayrıca dağıtıldıktan sonra panonun görünümünü özelleştirebilirsiniz. Ancak kaynak grubunuza yeniden dağıttığınızda pano şablonunuzdaki varsayılan durumuna sıfırlanır. Pano oluşturma hakkında daha fazla bilgi için bkz. [Program aracılığıyla Azure Panoları oluşturma](../azure-portal/azure-portal-dashboards-create-programmatically.md).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık Azure kaynakları gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

1. Azure portalından seçin **kaynak grupları** sol menüden.

1. Kaynak grubu adını seçin.

1. Üstteki menüden **Kaynak grubunu sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Visual Studio kullanarak nasıl şablonlar oluşturulacağını ve dağıtılacağını öğrendiniz. Sonraki öğreticide şifrelenmiş bir Azure Depolama hesabı oluşturmak için şablon referansından nasıl bilgi bulacağınız gösterilmektedir.

> [!div class="nextstepaction"]
> [Şifrelenmiş depolama hesabı oluşturma](./resource-manager-tutorial-create-encrypted-storage-accounts.md)
