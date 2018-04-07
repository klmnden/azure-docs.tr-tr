---
title: Azure sağlayıcısını dağıtım yuvası ile Terraform
description: Azure sağlayıcısı dağıtım yuvası Öğreticisi ile Terraform
keywords: terraform, devops, sanal makine, Azure, dağıtım yuvaları
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.date: 4/05/2018
ms.topic: article
ms.openlocfilehash: 34b16b5fb2b5b574d166693db346ebba15eaa1f9
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="using-terraform-to-provision-infrastructure-with-azure-deployment-slots"></a>Azure dağıtım yuvaları altyapısıyla sağlamak için Terraform kullanma

[Azure dağıtım yuvası](/azure/app-service/web-sites-staged-publishing) - uygulamanızı üretim ve hazırlama - bozuk dağıtımları etkisini en aza indirmek için gibi farklı sürümleri arasında takas olanak sağlar. Bu makalede, iki uygulama dağıtımıyla GitHub ve Azure adım adım ilerlemenizi sağlayarak dağıtım yuvaları örnek kullanımını gösterir. İkinci uygulama "Hazırlama" yuvasında barındırılan sırada bir uygulama bir "üretim yuvasına", barındırılır. (Adları "üretim" ve "Hazırlama" rasgele ve senaryonuz temsil eden istediğiniz herhangi bir şey olabilir.) Dağıtım yuvaları yapılandırdıktan sonra gerektiğinde arasında iki yuvaları takas için Terraform kullanabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

- **Azure aboneliği** - oluşturmak bir Azure aboneliğiniz yoksa bir [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) başlamadan önce.

- **GitHub hesabı** - A [GitHub](http://www.github.com) hesap çatallaştırma ve GitHub deposuna test kullanmak için gereklidir.

## <a name="create-and-apply-the-terraform-plan"></a>Oluşturma ve Terraform planı uygulama

1. Gözat [Azure portalı](http://portal.azure.com)

1. Açık [Azure bulut Kabuk](/azure/cloud-shell/overview)seçip - daha önce - yapılmamışsa **Bash** ortamınız olarak.

    ![Bulut Kabuk isteminde](./media/terraform-slot-walkthru/azure-portal-cloud-shell-button-min.png)

1. Değiştirme dizinleri `clouddrive` dizin.

    ```bash
    cd clouddrive
    ```

1. Adlı bir dizin oluşturun `deploy`.

    ```bash
    mkdir deploy
    ```

1. Adlı bir dizin oluşturun `swap`.

    ```bash
    mkdir swap
    ```

1. Her iki dizini başarıyla kullanılarak oluşturulmuş olduğunu doğrulayın `ls` komutu bash.

    ![Dizinleri oluşturduktan sonra bulut Kabuğu](./media/terraform-slot-walkthru/cloud-shell-after-creating-dirs.png)

1. Değiştirme dizinleri `deploy` dizin.

    ```bash
    cd deploy
    ```

1. Kullanarak [VI Düzenleyicisi](https://www.debian.org/doc/manuals/debian-tutorial/ch-editor.html), adlı bir dosya oluşturun `deploy.tf`, hangi içerecek [Terraform yapılandırma](https://www.terraform.io/docs/configuration/index.html).

    ```bash
    vi deploy.tf
    ```

1. Ekleme modu harf tuşlarına basarak girin `i` anahtarı.

1. Aşağıdaki kod düzenleyicisine yapıştırın:

    ```JSON
    # Configure the Azure Provider
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

1. Tuşuna  **&lt;Esc >** Ekle modundan çıkmak için anahtar.

1. Dosyayı kaydedin ve tuşlarına basarak ve ardından aşağıdaki komutu girerek VI düzenleyiciden çıkmak  **&lt;Enter >**:

    ```bash
    :wq
    ```

1. Dosya oluşturulduktan sonra içeriğinin doğrulayabilirsiniz.

    ```bash
    cat deploy.tf
    ```

1. Terraform başlatır.

    ```bash
    terraform init
    ```

1. Terraform planı oluşturun.

    ```bash
    terraform plan
    ```

1. Tanımlanan kaynakları sağlamak `deploy.tf` yapılandırma dosyası. (Eylem girerek onaylayın `yes` isteminde.)

    ```bash
    terraform apply
    ```

1. Bulut Kabuğu penceresini kapatın.

1. Azure portal ana menüde, seçin **kaynak grupları**.

    ![Azure portal kaynak grupları](./media/terraform-slot-walkthru/resource-groups-menu-option.png)

1. Üzerinde **kaynak grupları** sekmesine **slotDemoResourceGroup**.

    ![Kaynak grubu Terraform tarafından oluşturulan](./media/terraform-slot-walkthru/resource-group.png)

Tamamlandığında, Terraform tarafından oluşturulan tüm kaynaklara bakın.

![Terraform tarafından oluşturulan kaynakları](./media/terraform-slot-walkthru/resources.png)

## <a name="fork-the-test-project"></a>Çatalı test projesi

Oluşturma ve ve bu moddan dağıtım yuvaları takas test etmeden önce oluşturduğunuz test projesinin github'dan çatallaştırma gerekir.

1. Gözat [terraform harika bağlantıların github'da](https://github.com/Azure/awesome-terraform).

1. Çatalı **terraform harika depodaki**.

    ![Çatalı GitHub terraform harika depo](./media/terraform-slot-walkthru/fork-repo.png)

1. Ortamınıza dizisinde çatallaştırmak için tüm komut istemlerini izleyin.

## <a name="deploy-from-github-to-your-deployment-slots"></a>Github'dan dağıtım yuvalarına Dağıt

Bir kez test proje deposu çatalı dağıtım yuvaları aşağıdaki adımları aracılığıyla yapılandırın:

1. Azure portal ana menüde, seçin **kaynak grupları**.

1. Select **slotDemoResourceGroup**.

1. Select **slotAppService**.

1. Seçin **dağıtım seçenekleri**.

    ![Bir uygulama hizmeti kaynağı için dağıtım seçenekleri](./media/terraform-slot-walkthru/deployment-options.png)

1. Üzerinde **dağıtım seçeneği** sekmesine **Kaynağı Seç**ve ardından **GitHub**.

    ![Dağıtım kaynağı seçin](./media/terraform-slot-walkthru/select-source.png)

1. Azure bağlantısı yapar ve tüm seçeneklerini görüntüler sonra seçin **yetkilendirme**.

1. Üzerinde **yetkilendirme** sekmesine **Authorize**ve GitHub hesabınıza erişmek Azure için gereken kimlik bilgilerini sağlayın. 

1. GitHub kimlik bilgileriniz Azure doğruladıktan sonra Yetkilendirme işlemi tamamlanmış belirten bir ileti görüntüler. Seçin **Tamam** kapatmak için **yetkilendirme** sekmesi.

1. Seçin **kuruluşunuz seçin** ve kuruluşunuzu seçin.

1. Seçin **Seç proje**.

1. Üzerinde **Seç proje** sekmesine **terraform harika** projesi.

    ![Terraform harika projesini seçin](./media/terraform-slot-walkthru/choose-project.png)

1. Seçin **Seç şube**.

1. Üzerinde **Seç şube** sekmesine **ana**.

    ![Ana dala seçin](./media/terraform-slot-walkthru/choose-branch-master.png)

1. Üzerinde **dağıtım seçeneği** sekmesine **Tamam**.

Bu noktada, üretim yuvasına dağıtmış. Hazırlama yuvası dağıtmak için yalnızca aşağıdaki değişiklikleri ile bu bölümdeki tüm önceki adımları gerçekleştirin:

- In step 3, **slotAppServiceSlotOne** resource.

- Adım 13'de, ana dala yerine "çalışıyor" dalı seçin.

    ![Dal çalışma seçin](./media/terraform-slot-walkthru/choose-branch-working.png)

## <a name="test-the-app-deployments"></a>Uygulama dağıtımlarını test

Önceki bölümlerde iki yuvaları - ayarlamak **slotAppService** ve **slotAppServiceSlotOne** - GitHub farklı dallarda dağıtım yapmak. Şimdi bunların başarıyla dağıtıldı doğrulamak için web apps önizlemede.

İki kez burada 3. adımda seçtiğiniz aşağıdaki adımları gerçekleştirin **slotAppService** ilk kez ve ardından **slotAppServiceSlotOne** ikinci kez:

1. Azure portal ana menüde, seçin **kaynak grupları**.

1. Select **slotDemoResourceGroup**.

1. Şunlardan birini seçin **slotAppService** veya **slotAppServiceSlotOne**.

1. Genel bakış sayfasında seçin **URL**.

    ![Uygulama oluşturmak için URL genel bakış sekmesinde seçin](./media/terraform-slot-walkthru/resource-url.png)

> [!NOTE]
> Github'dan sitenizi oluşturup dağıtacak Azure birkaç dakika sürebilir.
>
>

İçin **slotAppService** web uygulaması, bir sayfa başlığı ile mavi bir sayfaya bakın **yuvası Demo uygulamayı 1**. İçin **slotAppServiceSlotOne** web uygulaması, bir sayfa başlığı ile yeşil bir sayfaya bakın **yuvası Demo uygulamayı 2**.

![Bunlar doğru dağıtılan test etmek için uygulamaları Önizleme](./media/terraform-slot-walkthru/app-preview.png)

## <a name="swap-the-two-deployment-slots"></a>İki dağıtım yuvalarını değiştirme

İki dağıtım yuvası takas sınamak için aşağıdaki adımları gerçekleştirin:
 
1. Geçiş çalıştıran tarayıcı sekmesinde **slotAppService** (mavi sayfa uygulamayla). 

1. Ayrı bir sekmesinde Azure Portalı'na dönün.

1. Bulut Kabuğu'nu açın.

1. Değiştirme dizinleri **clouddrive/takas** dizin.

    ```bash
    cd clouddrive/swap
    ```

1. Adlı bir dosya oluşturun VI Düzenleyicisi'ni kullanarak `swap.tf`.

    ```bash
    vi swap.tf
    ```

1. Ekleme modu harf tuşlarına basarak girin `i` anahtarı.

1. Aşağıdaki kod düzenleyicisine yapıştırın:

    ```JSON
    # Configure the Azure Provider
    provider "azurerm" { }

    # Swap the production slot and the staging slot
    resource "azurerm_app_service_active_slot" "slotDemoActiveSlot" {
        resource_group_name   = "slotDemoResourceGroup"
        app_service_name      = "slotAppService"
        app_service_slot_name = "slotappServiceSlotOne"
    }
    ```

1. Tuşuna  **&lt;Esc >** Ekle modundan çıkmak için anahtar.

1. Dosyayı kaydedin ve tuşlarına basarak ve ardından aşağıdaki komutu girerek VI düzenleyiciden çıkmak  **&lt;Enter >**:

    ```bash
    :wq
    ```

1. Terraform başlatır.

    ```bash
    terraform init
    ```

1. Terraform planı oluşturun.

    ```bash
    terraform plan
    ```

1. Tanımlanan kaynakları sağlamak `swap.tf` yapılandırma dosyası. (Eylem girerek onaylayın `yes` isteminde.)

    ```bash
    terraform apply
    ```

1. Tarayıcıya dönüş yuvaları takas Terraform tamamladığında işlerken **slotAppService** web app ve sayfayı yenileyin. 

Web uygulaması, **slotAppServiceSlotOne** yuvası hazırlama ve üretim yuvasıyla takas yeşil'şimdi oluşturur. 

![Dağıtım yuvaları takas](./media/terraform-slot-walkthru/slots-swapped.png)

Uygulama özgün üretim sürümüne geri dönmek için oluşturulan Terraform planı yeniden `swap.tf` yapılandırma dosyası.

```bash
terraform apply
```

Takas sonra özgün yapılandırmanın bakın.