---
title: Azure Kubernetes hizmet (AKS) ve Terraform Kubernetes kümesi oluşturma
description: Azure Kubernetes hizmeti ve Terraform ile Kubernetes kümesi oluşturmayı gösteren öğretici
keywords: terraform, devops, sanal makine, azure, kubernetes
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.date: 06/11/2018
ms.topic: article
ms.openlocfilehash: bd00a0cc8446802a03570edd58949a46c0769101
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35304201"
---
# <a name="create-a-kubernetes-cluster-with-azure-kubernetes-service-and-terraform"></a>Azure Kubernetes hizmeti ve Terraform ile Kubernetes kümesi oluşturma
[Azure Kubernetes hizmet (AKS)](/azure/aks/) kapsayıcı orchestration uzmanlık olmadan kapsayıcılı uygulamaları dağıtmak ve yönetmek kolay ve hızlı hale barındırılan Kubernetes ortamınıza yönetir. Ayrıca, kaynakları isteğe bağlı olarak sağlama, yükseltme ve ölçeklendirme işlemlerini uygulamalarınızı çevrimdışı duruma geçirmeden yaparak sürekliliği olan işlemlerin ve bakımların yükünü ortadan kaldırır.

Bu öğreticide, oluşturma şu görevleri gerçekleştirmek öğrenin bir [Kubernetes](https://www.redhat.com/en/topics/containers/what-is-kubernetes) kullanarak küme [Terraform](http://terraform.io) ve AKS:

> [!div class="checklist"]
> * HCL (HashiCorp dil) bir Kubernetes kümesi tanımlamak için kullanın
> * Terraform ve AKS Kubernetes küme oluşturmak için kullanın
> * Kubernetes küme kullanılabilirliğini test etmek için kubectl aracını kullanma

## <a name="prerequisites"></a>Önkoşullar

- **Azure aboneliği**: Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.

- **Terraform yapılandırma**: makalesindeki yönergeleri izleyin [Terraform ve Azure erişimi yapılandırma](/azure/virtual-machines/linux/terraform-install-configure)

- **Azure hizmet sorumlusu**: bölümündeki yönergeleri izleyin **hizmet sorumlusu oluşturma** makalenin bölümünde [Azure, Azure CLI 2.0 ile hizmet sorumlusu oluşturmak](/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest#create-the-service-principal). AppID, displayName, şifresi ve Kiracı değerlerini not edin.

## <a name="create-the-directory-structure"></a>Dizin yapısını oluşturun
İlk adım alıştırma için Terraform yapılandırma dosyalarını tutan dizinin oluşturmaktır.

1. Gözat [Azure portal](http://portal.azure.com).

1. Açık [Azure bulut Kabuk](/azure/cloud-shell/overview). Bir ortam önceden seçmediyseniz, seçin **Bash** ortamınız olarak.

    ![Bulut Kabuk isteminde](./media/terraform-create-k8s-cluster-with-tf-and-aks/azure-portal-cloud-shell-button-min.png)

1. Değiştirme dizinleri `clouddrive` dizin.

    ```bash
    cd clouddrive
    ```

1. Adlı bir dizin oluşturun `terraform-aks-k8s`.

    ```bash
    mkdir terraform-aks-k8s
    ```

1. Dizinleri yeni dizine değiştirin:

    ```bash
    cd terraform-aks-k8s
    ```

## <a name="declare-the-azure-provider"></a>Azure sağlayıcı bildirme
Azure sağlayıcı bildirir Terraform yapılandırma dosyası oluşturun.

1. Bulut Kabuğu'nda adlı bir dosya oluşturun `main.tf`.

    ```bash
    vi main.tf
    ```

1. Ekleme modu seçerek t anahtarı girin.

1. Aşağıdaki kod düzenleyicisine yapıştırın:

    ```JSON
    provider "azurerm" {
        version = "=1.5.0"
    }

    terraform {
        backend "azurerm" {}
    }

    ```

1. Seçerek Ekle modundan çık **Esc** anahtarı.

1. Dosyayı kaydedin ve aşağıdaki komutu girerek VI düzenleyiciden çıkın:

    ```bash
    :wq
    ```

## <a name="define-a-kubernetes-cluster"></a>Kubernetes küme tanımlayın
Kubernetes küme kaynaklarını bildirir Terraform yapılandırma dosyası oluşturun.

1. Bulut Kabuğu'nda adlı bir dosya oluşturun `k8s.tf`.

    ```bash
    vi k8s.tf
    ```

1. Ekleme modu seçerek t anahtarı girin.

1. Aşağıdaki kod düzenleyicisine yapıştırın:

    ```JSON
    resource "azurerm_resource_group" "k8s" {
        name     = "${var.resource_group_name}"
        location = "${var.location}"
    }

    resource "azurerm_kubernetes_cluster" "k8s" {
        name                = "${var.cluster_name}"
        location            = "${azurerm_resource_group.k8s.location}"
        resource_group_name = "${azurerm_resource_group.k8s.name}"
        dns_prefix          = "${var.dns_prefix}"

        linux_profile {
            admin_username = "ubuntu"

            ssh_key {
            key_data = "${file("${var.ssh_public_key}")}"
            }
        }

        agent_pool_profile {
            name            = "default"
            count           = "${var.agent_count}"
            vm_size         = "Standard_D2"
            os_type         = "Linux"
            os_disk_size_gb = 30
        }

        service_principal {
            client_id     = "${var.client_id}"
            client_secret = "${var.client_secret}"
        }

        tags {
            Environment = "Development"
        }
    }
    ```

    Önceki kod, küme, konum ve resource_group_name adını ayarlar. Ayrıca, -, kümeye erişmek için kullanılan tam etki alanı adı (FQDN) bir parçası forms - dns_prefix değer ayarlanır.

    **Linux_profile** kayıt SSH kullanarak alt düğümlerin içine imzalamayı etkinleştirmek ayarlarını yapılandırmanıza olanak sağlar.

    AKS ile yalnızca çalışan düğümleri için ücret ödersiniz. **Agent_pool_profile** kayıt ayrıntıları bu çalışan düğümleri için yapılandırır. **Agent_pool_profile kaydı** oluşturmak için çalışan düğüm sayısı ve çalışan düğümleri türünü içerir. Yukarı veya küme gelecekte ölçeklendirme gerekiyorsa, değişiklik **sayısı** bu kayıt değeri.

1. Seçerek Ekle modundan çık **Esc** anahtarı.

1. Dosyayı kaydedin ve aşağıdaki komutu girerek VI düzenleyiciden çıkın:

    ```bash
    :wq
    ```

## <a name="declare-the-variables"></a>Değişkenleri bildirme

1. Bulut Kabuğu'nda adlı bir dosya oluşturun `variables.tf`.

    ```bash
    vi variables.tf
    ```

1. Ekleme modu seçerek t anahtarı girin.

1. Aşağıdaki kod düzenleyicisine yapıştırın:

    ```JSON
    variable "client_id" {}
    variable "client_secret" {}

    variable "agent_count" {
        default = 3
    }

    variable "ssh_public_key" {
        default = "~/.ssh/id_rsa.pub"
    }

    variable "dns_prefix" {
        default = "k8stest"
    }

    variable cluster_name {
        default = "k8stest"
    }

    variable resource_group_name {
        default = "azure-k8stest"
    }

    variable location {
        default = "Central US"
    }
    ```

1. Seçerek Ekle modundan çık **Esc** anahtarı.

1. Dosyayı kaydedin ve aşağıdaki komutu girerek VI düzenleyiciden çıkın:

    ```bash
    :wq
    ```

## <a name="create-a-terraform-output-file"></a>Terraform çıkış dosyası oluşturma
[Terraform çıkarır](https://www.terraform.io/docs/configuration/outputs.html) Terraform bir planı uygulanır ve aracılığıyla sorgulanabilir vurgulanır değerleri kullanıcıya tanımlamanıza izin `terraform output` komutu. Bu bölümde, kümeye erişimine izin veren bir çıkış dosyası oluşturma [kubectl](https://kubernetes.io/docs/reference/kubectl/overview/).

1. Bulut Kabuğu'nda adlı bir dosya oluşturun `output.tf`.

    ```bash
    vi output.tf
    ```

1. Ekleme modu seçerek t anahtarı girin.

1. Aşağıdaki kod düzenleyicisine yapıştırın:

    ```JSON
    output "client_key" {
        value = "${azurerm_kubernetes_cluster.k8s.kube_config.0.client_key}"
    }

    output "client_certificate" {
        value = "${azurerm_kubernetes_cluster.k8s.kube_config.0.client_certificate}"
    }

    output "cluster_ca_certificate" {
        value = "${azurerm_kubernetes_cluster.k8s.kube_config.0.cluster_ca_certificate}"
    }

    output "cluster_username" {
        value = "${azurerm_kubernetes_cluster.k8s.kube_config.0.username}"
    }

    output "cluster_password" {
        value = "${azurerm_kubernetes_cluster.k8s.kube_config.0.password}"
    }

    output "kube_config" {
        value = "${azurerm_kubernetes_cluster.k8s.kube_config_raw}"
    }

    output "host" {
        value = "${azurerm_kubernetes_cluster.k8s.kube_config.0.host}"
    }
    ```

1. Seçerek Ekle modundan çık **Esc** anahtarı.

1. Dosyayı kaydedin ve aşağıdaki komutu girerek VI düzenleyiciden çıkın:

    ```bash
    :wq
    ```

## <a name="set-up-azure-storage-to-store-terraform-state"></a>Terraform durumunu depolamak için Azure depolama alanı ayarlama
Terraform parçaları durumu yerel olarak aracılığıyla `terraform.tfstate` dosya. Bu desen bir tek kişi ortamda iyi çalışır. Ancak, daha kullanışlı bir çok kişili ortamında durumu kullanarak sunucu üzerinde izlemeniz gereken [Azure depolama](/azure/storage/). Bu bölümde, gerekli depolama hesabı bilgilerini (hesap adı ve hesap anahtarı) almak ve Terraform durum bilgilerini depolanacağı bir depolama kapsayıcısı oluşturun.

1. Azure portalında seçin **tüm hizmetleri** soldaki menüde.

1. Seçin **depolama hesapları**.

1. Üzerinde **depolama hesapları** sekmesinde, Terraform olduğu durumunu depolamak için depolama hesabının adını seçin. Örneğin, bulut Kabuk ilk kez açıldığında oluşturulan depolama hesabı kullanabilirsiniz.  Genellikle bulut Kabuk tarafından oluşturulan depolama hesabı adı ile başlayan `cs` sayılardan ve harflerden oluşan rastgele bir dize tarafından izlenen. **Daha sonra gerektikçe seçin, depolama hesabının adını unutmayın.**

1. Depolama hesabı sekmesinde seçin **erişim anahtarları**.

    ![Depolama hesap menüsü](./media/terraform-create-k8s-cluster-with-tf-and-aks/storage-account.png)

1. Not **key1** **anahtar** değeri. (Anahtar sağındaki simgesini seçerek değeri panoya kopyalar.)

    ![Depolama hesabı erişim tuşları](./media/terraform-create-k8s-cluster-with-tf-and-aks/storage-account-access-key.png)

1. Bulut Kabuğu'nda Azure depolama hesabınızdaki bir kapsayıcı oluşturmak (Değiştir &lt;YourAzureStorageAccountName > ve &lt;YourAzureStorageAccountAccessKey > yer tutucularını Azure depolama hesabınız için uygun değerlerle ).

    ```bash
    az storage container create -n tfstate --account-name <YourAzureStorageAccountName> --account-key <YourAzureStorageAccountKey>
    ```

## <a name="create-the-kubernetes-cluster"></a>Kubernetes kümesi oluşturma
Bu bölümde, nasıl kullanacağınızı öğrenin `terraform init` kaynak oluşturmak için komutu tanımlanan önceki kısımlarında oluşturduğunuz yapılandırma dosyaları.

1. Bulut Kabuğu'nda Terraform başlatılamıyor (Değiştir &lt;YourAzureStorageAccountName > ve &lt;YourAzureStorageAccountAccessKey > Azure depolama hesabınız için uygun değerlerle yer tutucular).

    ```bash
    terraform init -backend-config="storage_account_name=<YourAzureStorageAccountName>" -backend-config="container_name=tfstate" -backend-config="access_key=<YourStorageAccountAccessKey>" -backend-config="key=codelab.microsoft.tfstate" 
    ```
    
    `terraform init` Komut arka ve sağlayıcı eklentisi başlatma başarısını görüntüler:

    !["Terraform init" sonuçları örneği](./media/terraform-create-k8s-cluster-with-tf-and-aks/terraform-init-complete.png)

1. Çalıştırma `terraform plan` altyapı öğeleri tanımlar Terraform planı oluşturmak için komutu. Komut iki değeri ister: **var.client_id** ve **var.client_secret**. İçin **var.client_id** değişkeni girin **AppID** , hizmet sorumlusu ile ilişkili değer. İçin **var.client_secret** değişkeni girin **parola** , hizmet sorumlusu ile ilişkili değer.

    ```bash
    terraform plan -out out.plan
    ```

    `terraform plan` Komutu çalıştırdığınızda, oluşturulacak kaynakları görüntüler `terraform apply` komutu:

    !["Terraform planı" sonuçları örneği](./media/terraform-create-k8s-cluster-with-tf-and-aks/terraform-plan-complete.png)

1. Çalıştırma `terraform apply` Kubernetes küme oluşturmak için plana uygulamak için komutu. Kubernetes küme oluşturma işlemi bulut Kabuk oturum zaman aşımına uğramadan bunun sonucunda, birkaç dakika sürebilir. Bulut kabuk oturumu zaman aşımına uğrarsa bölümündeki adımları takip edebilirsiniz ["bir bulut Kabuk zaman aşımı kurtarmak"](#recover-from-a-dloud-shell-timeout) , öğreticiyi tamamlamak etkinleştirmek için.

    ```bash
    terraform apply out.plan
    ```

    `terraform apply` Komut yapılandırma dosyalarında tanımlanan kaynakları oluşturma sonuçlarını görüntüler:

    !["Terraform uygulamak" sonuçları örneği](./media/terraform-create-k8s-cluster-with-tf-and-aks/terraform-apply-complete.png)

1. Azure portalında seçin **tüm hizmetleri** yeni Kubernetese kümeniz için oluşturulan kaynakları görmek için soldaki menüde.

    ![Bulut Kabuk isteminde](./media/terraform-create-k8s-cluster-with-tf-and-aks/k8s-resources-created.png)

## <a name="recover-from-a-cloud-shell-timeout"></a>Bir bulut Kabuk zaman aşımı Kurtar
Bulut kabuk oturumu zaman aşımına uğrarsa kurtarmak için aşağıdaki adımları gerçekleştirebilirsiniz:

1. Bir bulut kabuk oturumu başlatın.

1. Terraform yapılandırma dosyalarını içeren dizini değiştirin.

    ```bash
    cd /clouddrive/terraform-aks-k8s
    ```

1. Şu komutu çalıştırın:

    ```bash
    export KUBECONFIG=./azurek8s
    ```
    
## <a name="test-the-kubernetes-cluster"></a>Kubernetes küme test
Kubernetes Araçlar, yeni oluşturulan bir küme doğrulamak için kullanılabilir.

1. Terraform durumundan Kubernetes yapılandırmasını almak ve bu kubectl bir dosyada saklayabilir okuyabilirsiniz.

    ```bash
    echo "$(terraform output kube_config)" > ./azurek8s
    ```

1. Bir ortam değişkeni doğru config kubectl Çekmeleri şekilde ayarlayın.

    ```bash
    export KUBECONFIG=./azurek8s
    ```

1. Küme durumunu doğrulayın.

    ```bash
    kubectl get nodes
    ```

    Çalışan düğümü ayrıntılarını görmeniz gerekir ve tüm durumuna sahip olmalıdır **hazır**aşağıdaki görüntüde gösterildiği gibi:

    ![Kubectl aracı Kubernetes kümenizi durumunu doğrulamak sağlar](./media/terraform-create-k8s-cluster-with-tf-and-aks/kubectl-get-nodes.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Terraform ve AKS Kubernetes küme oluşturmak için nasıl kullanılacağı hakkında bilgi edindiniz. Azure üzerinde Terraform hakkında daha fazla bilgi edinmenize yardımcı olması için bazı ek kaynaklar aşağıda verilmiştir: 

 [Microsoft.com Terraform Hub](https://docs.microsoft.com/azure/terraform/)  
 [Terraform Azure sağlayıcı belgeleri](http://aka.ms/terraform)  
 [Terraform Azure sağlayıcı kaynağı](http://aka.ms/tfgit)  
 [Terraform Azure modülleri](http://aka.ms/tfmodules)