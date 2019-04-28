---
title: Terraform ile Azure sağlayıcısı dağıtım yuvaları
description: Terraform ile Azure sağlayıcısı dağıtım yuvalarını kullanma öğreticisi
services: terraform
ms.service: azure
keywords: terraform, devops, sanal makine, Azure, dağıtım yuvaları
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 4/05/2018
ms.openlocfilehash: 08e90a69791b0555a6497166f6008e8619f40704
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60889347"
---
# <a name="use-terraform-to-provision-infrastructure-with-azure-deployment-slots"></a>Azure dağıtım yuvalarıyla altyapı sağlamak için Terraform'u kullanma

[Azure dağıtım yuvalarını](/azure/app-service/deploy-staging-slots) kullanarak uygulamanızın farklı sürümleri arasında geçiş yapabilirsiniz. Bu özellik, bozuk dağıtımlarının etkisini en aza indirmenize yardımcı olur. 

Bu makalede iki uygulamanın GitHub ve Azure ile gerçekleştirilen dağıtım adımları ile dağıtım yuvası kullanımı gösterilmektedir. Uygulamaların biri üretim yuvasında barındırılmaktadır. İkinci uygulama ise bir hazırlama yuvasında barındırılmaktadır. ("üretim" ve "hazırlama" adları rastgele verilmiştir ve senaryonuzu niteleyen faklı ifadeler de kullanılabilir.) Dağıtım yuvalarınızı yapılandırdıktan sonra Terraform'u kullanarak iki yuva arasında geçiş yapabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

- **Azure aboneliği**: Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.

- **GitHub hesabı**: Gereksinim duyduğunuz bir [GitHub](https://www.github.com) hesabı çatalını oluşturmanız ve GitHub deposunu testi kullanın.

## <a name="create-and-apply-the-terraform-plan"></a>Terraform planını oluşturma ve uygulama

1. [Azure portala](https://portal.azure.com) gidin.

1. [Azure Cloud Shell](/azure/cloud-shell/overview)'i açın. Önceden bir ortam seçmediyseniz **Bash** ortamını seçin.

    ![Cloud Shell istemi](./media/terraform-slot-walkthru/azure-portal-cloud-shell-button-min.png)

1. `clouddrive` dizinine geçin.

    ```bash
    cd clouddrive
    ```

1. `deploy` adlı bir dizin oluşturun.

    ```bash
    mkdir deploy
    ```

1. `swap` adlı bir dizin oluşturun.

    ```bash
    mkdir swap
    ```

1. `ls` Bash komutunu kullanarak iki dizini de başarıyla oluşturduğunuzu doğrulayın.

    ![Dizinler oluşturulduktan sonra Cloud Shell](./media/terraform-slot-walkthru/cloud-shell-after-creating-dirs.png)

1. `deploy` dizinine geçin.

    ```bash
    cd deploy
    ```

1. [vi editor](https://www.debian.org/doc/manuals/debian-tutorial/ch-editor.html) uygulamasını kullanarak `deploy.tf` adlı bir dosya oluşturun. Bu dosya [Terraform yapılandırmasını](https://www.terraform.io/docs/configuration/index.html) barındıracaktır.

    ```bash
    vi deploy.tf
    ```

1. I tuşunu seçerek ekleme moduna geçin.

1. Aşağıdaki kodu düzenleyiciye yapıştırın:

    ```JSON
    # Configure the Azure provider
    provider "azurerm" { }

    resource "azurerm_resource_group" "slotDemo" {
        name = "slotDemoResourceGroup"
        location = "westus2"
    }

    resource "azurerm_app_service_plan" "slotDemo" {
        name                = "slotAppServicePlan"
        location            = "${azurerm_resource_group.slotDemo.location}"
        resource_group_name = "${azurerm_resource_group.slotDemo.name}"
        sku {
            tier = "Standard"
            size = "S1"
        }
    }

    resource "azurerm_app_service" "slotDemo" {
        name                = "slotAppService"
        location            = "${azurerm_resource_group.slotDemo.location}"
        resource_group_name = "${azurerm_resource_group.slotDemo.name}"
        app_service_plan_id = "${azurerm_app_service_plan.slotDemo.id}"
    }

    resource "azurerm_app_service_slot" "slotDemo" {
        name                = "slotAppServiceSlotOne"
        location            = "${azurerm_resource_group.slotDemo.location}"
        resource_group_name = "${azurerm_resource_group.slotDemo.name}"
        app_service_plan_id = "${azurerm_app_service_plan.slotDemo.id}"
        app_service_name    = "${azurerm_app_service.slotDemo.name}"
    }
    ```

1. Ekleme modundan çıkmak için Esc tuşunu seçin.

1. Dosyayı kaydedin ve aşağıdaki komutu girerek VI düzenleyicisini kapatın:

    ```bash
    :wq
    ```

1. Dosyayı oluşturduğunuza göre şimdi içeriğini doğrulayabilirsiniz.

    ```bash
    cat deploy.tf
    ```

1. Terraform'u başlatın.

    ```bash
    terraform init
    ```

1. Terraform planını oluşturun.

    ```bash
    terraform plan
    ```

1. `deploy.tf` yapılandırma dosyasında tanımlanan kaynakları sağlayın. (İsteme `yes` girerek eylemi onaylayın.)

    ```bash
    terraform apply
    ```

1. Cloud Shell penceresini kapatın.

1. Azure portalın ana menüsünde **Kaynak grupları**’nı seçin.

    ![Portalda "Kaynak grupları" seçimi](./media/terraform-slot-walkthru/resource-groups-menu-option.png)

1. **Kaynak grupları** sekmesinde **slotDemoResourceGroup** girişini seçin.

    ![Terraform tarafından oluşturulan kaynak grubu](./media/terraform-slot-walkthru/resource-group.png)

Burada Terraform tarafından oluşturulan tüm kaynakları görebilirsiniz.

![Terraform tarafından oluşturulan kaynaklar](./media/terraform-slot-walkthru/resources.png)

## <a name="fork-the-test-project"></a>Test projesi için çatal oluşturma

Dağıtım yuvası oluşturma ve değiştirme işlevlerini test edebilmek için GitHub'daki test projesi için bir çatal oluşturmanız gerekir.

1. [GitHub'daki awesome-terraform deposuna](https://github.com/Azure/awesome-terraform) gidin.

1. **awesome-terraform** deposundan çatal oluşturun.

    ![GitHub awesome-terraform deposundan çatal oluşturma](./media/terraform-slot-walkthru/fork-repo.png)

1. İstemleri takip ederek ortamınızda çatal oluşturun.

## <a name="deploy-from-github-to-your-deployment-slots"></a>GitHub'dan dağıtım yuvalarınıza dağıtım yapma

Test projesi deposundan çatal oluşturduktan sonra aşağıdaki adımları izleyerek dağıtım yuvalarını yapılandırın:

1. Azure portalın ana menüsünde **Kaynak grupları**’nı seçin.

1. **slotDemoResourceGroup** öğesini seçin.

1. **slotAppService** öğesini seçin.

1. **Dağıtım seçenekleri**'ni seçin.

    ![App Service kaynağı dağıtım seçenekleri](./media/terraform-slot-walkthru/deployment-options.png)

1. **Dağıtım seçeneği** sekmesinde **Kaynak Seç**'i ve ardından **GitHub**'ı seçin.

    ![Dağıtım kaynağı seçme](./media/terraform-slot-walkthru/select-source.png)

1. Azure bağlantıyı kurduktan ve tüm seçenekleri görüntüledikten sonra **Yetkilendirme**'yi seçin.

1. **Yetkilendirme** sekmesinde **Yetkilendir**'i seçin ve Azure'ın GitHub hesabınıza erişmesi için ihtiyaç duyduğu kimlik bilgilerini girin. 

1. Azure, GitHub kimlik bilgilerinizi doğruladıktan sonra yetkilendirme işleminin tamamlandığını belirten bir ileti görüntülenir. **Tamam**'ı seçerek **Yetkilendirme** sekmesini kapatın.

1. **Kuruluşunuzu seçin**'i ve ardından kuruluşunuzu seçin.

1. **Proje seçin**'i belirleyin.

1. **Proje seçin** sekmesinde **awesome-terraform** projesini seçin.

    ![awesome-terraform projesini seçin](./media/terraform-slot-walkthru/choose-project.png)

1. **Dal seçin**'i belirleyin.

1. **Dal seçin** sekmesinde **master** seçeneğini belirleyin.

    ![master dalını seçin](./media/terraform-slot-walkthru/choose-branch-master.png)

1. **Dağıtım seçeneği** sekmesinde **Tamam**'ı seçin.

Bu işlemlerle üretim yuvasını dağıtmış oldunuz. Hazırlama yuvasını dağıtmak için bu bölümdeki tüm adımları gerçekleştirin ancak aşağıdaki değişiklikleri yapın:

- 3. adımda **slotAppServiceSlotOne** kaynağını seçin.

- 13. adımda master dalı yerine working dalını seçin.

    ![working dalını seçin](./media/terraform-slot-walkthru/choose-branch-working.png)

## <a name="test-the-app-deployments"></a>Uygulama dağıtımlarını test etme

Önceki bölümlerde GitHub'daki farklı dallardan dağıtım yapmak için **slotAppService** ve **slotAppServiceSlotOne** olmak üzere iki yuva ayarladınız. Şimdi web uygulamalarının önizlemesini yaparak başarıyla dağıtıldıklarını doğrulayalım.

Aşağıdaki adımları 2 kez gerçekleştirin. 3. adımda önce **slotAppService** öğesini ve ardından **slotAppServiceSlotOne** öğesini seçin.

1. Azure portalın ana menüsünde **Kaynak grupları**’nı seçin.

1. **slotDemoResourceGroup** öğesini seçin.

1. **slotAppService** veya **slotAppServiceSlotOne** öğesini seçin.

1. Genel bakış sayfasında **URL**'yi seçin.

    ![Uygulamayı oluşturmak için genel bakış sayfasındaki URL'yi seçin](./media/terraform-slot-walkthru/resource-url.png)

> [!NOTE]
> Azure'ın siteyi GitHub'dan derlemesi ve dağıtması birkaç dakika sürebilir.
>
>

**slotAppService** web uygulaması için **Slot Demo App 1** başlığına sahip mavi bir sayfa görürsünüz. **slotAppServiceSlotOne** web uygulaması için **Slot Demo App 2** başlığına sahip mavi bir sayfa görürsünüz.

![Uygulamaların önizlemesini yaparak doğru dağıtıldığından emin olun](./media/terraform-slot-walkthru/app-preview.png)

## <a name="swap-the-two-deployment-slots"></a>İki dağıtım yuvasını değiştirme

İki dağıtım yuvasını değiştirme testi için aşağıdaki adımları gerçekleştirin:
 
1. **slotAppService** (mavi sayfalı uygulama) uygulamasını çalıştıran tarayıcı sekmesine geçin. 

1. Ayrı bir sekmede Azure portala gidin.

1. Cloud Shell'i açın.

1. **clouddrive/swap** dizinine geçin.

    ```bash
    cd clouddrive/swap
    ```

1. vi editor uygulamasını kullanarak `swap.tf` adlı bir dosya oluşturun.

    ```bash
    vi swap.tf
    ```

1. I tuşunu seçerek ekleme moduna geçin.

1. Aşağıdaki kodu düzenleyiciye yapıştırın:

    ```JSON
    # Configure the Azure provider
    provider "azurerm" { }

    # Swap the production slot and the staging slot
    resource "azurerm_app_service_active_slot" "slotDemoActiveSlot" {
        resource_group_name   = "slotDemoResourceGroup"
        app_service_name      = "slotAppService"
        app_service_slot_name = "slotappServiceSlotOne"
    }
    ```

1. Ekleme modundan çıkmak için Esc tuşunu seçin.

1. Dosyayı kaydedin ve aşağıdaki komutu girerek VI düzenleyicisini kapatın:

    ```bash
    :wq
    ```

1. Terraform'u başlatın.

    ```bash
    terraform init
    ```

1. Terraform planını oluşturun.

    ```bash
    terraform plan
    ```

1. `swap.tf` yapılandırma dosyasında tanımlanan kaynakları sağlayın. (İsteme `yes` girerek eylemi onaylayın.)

    ```bash
    terraform apply
    ```

1. Terraform yuva değiştirme işlemini tamamladıktan sonra **slotAppService** web uygulamasını çalıştıran tarayıcı sekmesine gidip sayfayı yenileyin. 

**slotAppServiceSlotOne** hazırlama yuvasındaki web uygulaması, üretim yuvasıyla değiştirilmiştir ve yeşil olarak görünür. 

![Dağıtım yuvaları değiştirildi](./media/terraform-slot-walkthru/slots-swapped.png)

Uygulamanın özgün üretim sürümüne dönmek için `swap.tf` yapılandırma dosyasından oluşturduğunuz Terraform planını yeniden uygulayın.

```bash
terraform apply
```

Uygulama değiştirildikten sonra özgün yapılandırmayı görürsünüz.