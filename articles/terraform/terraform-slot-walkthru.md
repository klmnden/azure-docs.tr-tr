---
title: Azure sağlayıcısını dağıtım yuvası ile Terraform
description: Terraform Azure sağlayıcısını dağıtım yuvası ile kullanma hakkında
keywords: terraform, devops, sanal makine, Azure, dağıtım yuvaları
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.date: 4/05/2018
ms.topic: article
ms.openlocfilehash: 3a018dbaf90801604b13efcf8bd7afb6dbc68659
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
ms.locfileid: "31416872"
---
# <a name="use-terraform-to-provision-infrastructure-with-azure-deployment-slots"></a>Azure dağıtım yuvaları altyapısıyla sağlamak için Terraform kullanın

Kullanabileceğiniz [Azure dağıtım yuvası](/azure/app-service/web-sites-staged-publishing) uygulamanız farklı sürümleri arasında değiştirilecek. Bu özelliği bozuk dağıtımları etkisini en aza indirmenize yardımcı olur. 

Bu makalede, iki uygulama dağıtımıyla GitHub ve Azure adım adım ilerlemenizi sağlayarak dağıtım yuvaları örnek kullanımını gösterir. Bir uygulamayı bir üretim yuvasına barındırılır. İkinci uygulama hazırlama yuvası içinde barındırılır. (Adları "üretim" ve "Hazırlama" rasgele ve senaryonuz temsil eden istediğiniz herhangi bir şey olabilir.) Dağıtım yuvaları yapılandırdıktan sonra gerektiği gibi iki yuvaları arasında takas için Terraform kullanabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

- **Azure aboneliği**: Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.

- **GitHub hesabı**: gereksinim duyduğunuz bir [GitHub](http://www.github.com) çatallaştırma ve GitHub deposuna test kullanmak için hesap.

## <a name="create-and-apply-the-terraform-plan"></a>Oluşturma ve Terraform planı uygulama

1. Gözat [Azure portal](http://portal.azure.com).

1. Açık [Azure bulut Kabuk](/azure/cloud-shell/overview). Bir ortam önceden seçmediyseniz, seçin **Bash** ortamınız olarak.

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

1. Kullanım `ls` her iki dizini başarıyla oluşturuldu doğrulamak için komutu bash.

    ![Dizinleri oluşturduktan sonra bulut Kabuğu](./media/terraform-slot-walkthru/cloud-shell-after-creating-dirs.png)

1. Değiştirme dizinleri `deploy` dizin.

    ```bash
    cd deploy
    ```

1. Kullanarak [VI Düzenleyicisi](https://www.debian.org/doc/manuals/debian-tutorial/ch-editor.html), adlı bir dosya oluşturun `deploy.tf`. Bu dosya içerir [Terraform yapılandırma](https://www.terraform.io/docs/configuration/index.html).

    ```bash
    vi deploy.tf
    ```

1. Ekleme modu seçerek t anahtarı girin.

1. Aşağıdaki kod düzenleyicisine yapıştırın:

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

1. INSERT modundan çıkmak için Esc tuşuna seçin.

1. Dosyayı kaydedin ve aşağıdaki komutu girerek VI düzenleyiciden çıkın:

    ```bash
    :wq
    ```

1. Dosyayı oluşturduğunuza göre içeriğini doğrulayın.

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

1. İçinde tanımlanan kaynakları sağlamak `deploy.tf` yapılandırma dosyası. (Eylem girerek onaylayın `yes` isteminde.)

    ```bash
    terraform apply
    ```

1. Bulut Kabuğu penceresini kapatın.

1. Azure portal'ın ana menüde seçin **kaynak grupları**.

    ![Portalı'nda "Kaynak grupları" seçimi](./media/terraform-slot-walkthru/resource-groups-menu-option.png)

1. Üzerinde **kaynak grupları** sekmesine **slotDemoResourceGroup**.

    ![Kaynak grubu Terraform tarafından oluşturulan](./media/terraform-slot-walkthru/resource-group.png)

Artık Terraform oluşturduğu tüm kaynakları görürsünüz.

![Terraform tarafından oluşturulan kaynakları](./media/terraform-slot-walkthru/resources.png)

## <a name="fork-the-test-project"></a>Çatalı test projesi

Oluşturma ve ve bu moddan dağıtım yuvaları takas test etmeden önce oluşturduğunuz test projesinin github'dan çatallaştırma gerekir.

1. Gözat [terraform harika bağlantıların github'da](https://github.com/Azure/awesome-terraform).

1. Çatalı **terraform harika** deposu.

    ![Çatalı GitHub terraform harika depo](./media/terraform-slot-walkthru/fork-repo.png)

1. Ortamınıza dizisinde çatallaştırmak için tüm komut istemlerini izleyin.

## <a name="deploy-from-github-to-your-deployment-slots"></a>Github'dan dağıtım yuvalarına Dağıt

Test projesi depoyu çatallaştırma sonra aşağıdaki adımları aracılığıyla dağıtım yuvaları yapılandırın:

1. Azure portal'ın ana menüde seçin **kaynak grupları**.

1. Seçin **slotDemoResourceGroup**.

1. Seçin **slotAppService**.

1. Seçin **dağıtım seçenekleri**.

    ![Bir uygulama hizmeti kaynağı için dağıtım seçenekleri](./media/terraform-slot-walkthru/deployment-options.png)

1. Üzerinde **dağıtım seçeneği** sekmesine **Kaynağı Seç**ve ardından **GitHub**.

    ![Dağıtım kaynağı seçin](./media/terraform-slot-walkthru/select-source.png)

1. Azure bağlantısı yapar ve tüm seçeneklerini görüntüler sonra seçin **yetkilendirme**.

1. Üzerinde **yetkilendirme** sekmesine **Authorize**ve Azure, GitHub hesabınızda erişmesi gereken kimlik bilgilerini sağlayın. 

1. GitHub kimlik bilgileriniz Azure doğruladıktan sonra bir ileti görünür ve yetkilendirme işlemi tamamlandı söyler. Seçin **Tamam** kapatmak için **yetkilendirme** sekmesi.

1. Seçin **kuruluşunuz seçin** ve kuruluşunuzu seçin.

1. Seçin **Seç proje**.

1. Üzerinde **Seç proje** sekmesine **terraform harika** projesi.

    ![Terraform harika projesini seçin](./media/terraform-slot-walkthru/choose-project.png)

1. Seçin **Seç şube**.

1. Üzerinde **Seç şube** sekmesine **ana**.

    ![Ana dala seçin](./media/terraform-slot-walkthru/choose-branch-master.png)

1. Üzerinde **dağıtım seçeneği** sekmesine **Tamam**.

Bu noktada, üretim yuvasına dağıtmış. Hazırlama yuvası dağıtmak için yalnızca aşağıdaki değişiklikleri ile bu bölümdeki tüm önceki adımları gerçekleştirin:

- 3. adımda seçin **slotAppServiceSlotOne** kaynak.

- Adım 13'de, ana dala yerine çalışma dalı seçin.

    ![Çalışma dalı seçin](./media/terraform-slot-walkthru/choose-branch-working.png)

## <a name="test-the-app-deployments"></a>Uygulama dağıtımlarını test

Önceki bölümlerde iki yuvaları--ayarlamak**slotAppService** ve **slotAppServiceSlotOne**--GitHub farklı dallarda dağıtım yapmak. Şimdi bunların başarıyla dağıtıldı doğrulamak için web apps önizlemede.

İki kez aşağıdaki adımları gerçekleştirin. 3. adımda seçtiğiniz **slotAppService** ilk kez ve ardından **slotAppServiceSlotOne** ikinci kez.

1. Azure portal'ın ana menüde seçin **kaynak grupları**.

1. Seçin **slotDemoResourceGroup**.

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

1. Azure portalına ayrı bir sekmede döndür.

1. Bulut Kabuğu'nu açın.

1. Değiştirme dizinleri **clouddrive/takas** dizin.

    ```bash
    cd clouddrive/swap
    ```

1. Adlı bir dosya oluşturun VI Düzenleyicisi'ni kullanarak `swap.tf`.

    ```bash
    vi swap.tf
    ```

1. Ekleme modu seçerek t anahtarı girin.

1. Aşağıdaki kod düzenleyicisine yapıştırın:

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

1. INSERT modundan çıkmak için Esc tuşuna seçin.

1. Dosyayı kaydedin ve aşağıdaki komutu girerek VI düzenleyiciden çıkın:

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

1. İçinde tanımlanan kaynakları sağlamak `swap.tf` yapılandırma dosyası. (Eylem girerek onaylayın `yes` isteminde.)

    ```bash
    terraform apply
    ```

1. Terraform tamamlandıktan sonra tarayıcıya dönüş yuvaları takas işleme **slotAppService** web app ve sayfayı yenileyin. 

Web uygulaması, **slotAppServiceSlotOne** yuvası hazırlama ve üretim yuvasıyla takas şimdi yeşil olarak çizilir. 

![Dağıtım yuvaları takas](./media/terraform-slot-walkthru/slots-swapped.png)

Uygulama özgün üretim sürümüne geri dönmek için oluşturulan Terraform planı yeniden `swap.tf` yapılandırma dosyası.

```bash
terraform apply
```

Uygulama takas sonra özgün yapılandırmanın bakın.