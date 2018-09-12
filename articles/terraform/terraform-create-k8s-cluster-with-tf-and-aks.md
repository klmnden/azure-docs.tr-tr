---
title: Azure Kubernetes Service (AKS) ve Terraform ile bir Kubernetes kümesi oluşturma
description: Azure Kubernetes Service ve Terraform ile Kubernetes Kümesi oluşturmayı gösteren öğretici
services: terraform
ms.service: terraform
keywords: terraform, devops, sanal makine, azure, kubernetes
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 06/11/2018
ms.openlocfilehash: 8a997c88943b0273d3136dbf02a784fbdb982527
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2018
ms.locfileid: "43666816"
---
# <a name="create-a-kubernetes-cluster-with-azure-kubernetes-service-and-terraform"></a>Azure Kubernetes Service ve Terraform ile bir Kubernetes kümesi oluşturma
[Azure Kubernetes Service (AKS)](/azure/aks/), barındırılan Kubernetes ortamınızı yöneterek kapsayıcılı uygulamaları, kapsayıcı yönetimi uzmanlığı gerekmeden hızla ve kolayca dağıtma olanağı sunar. Ayrıca, kaynakları isteğe bağlı olarak sağlama, yükseltme ve ölçeklendirme işlemlerini uygulamalarınızı çevrimdışı duruma geçirmeden yaparak sürekliliği olan işlemlerin ve bakımların yükünü ortadan kaldırır.

Bu öğreticide aşağıdaki [Terraform](http://terraform.io) ve AKS'yi kullanarak [Kubernetes](https://www.redhat.com/en/topics/containers/what-is-kubernetes) kümesi oluşturma görevlerini gerçekleştirmeyi öğreneceksiniz:

> [!div class="checklist"]
> * HCL (HashiCorp Language) ile Kubernetes kümesi tanımlama
> * Terraform ve AKS ile Kubernetes kümesi oluşturma
> * kubectl aracıyla bir Kubernetes kümesinin kullanılabilirlik durumunu test etme

## <a name="prerequisites"></a>Ön koşullar

- **Azure aboneliği**: Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.

- **Terraform'u yapılandırma**: [Terraform'u yükleme ve Azure erişimini yapılandırma](/azure/virtual-machines/linux/terraform-install-configure) makalesindeki yönergeleri izleyin

- **Azure hizmet sorumlusu**: [Azure CLI 2.0 ile Azure hizmet sorumlusu oluşturma](/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest#create-the-service-principal) makalesinin **Hizmet sorumlusunu oluşturma** bölümündeki yönergeleri izleyin. appId, displayName, password ve tenant değerlerini not edin.

## <a name="create-the-directory-structure"></a>Dizin yapısını oluşturma
İlk adım, bu alıştırmadaki Terraform yapılandırma dosyalarınızı barındıracak olan dizini oluşturmaktır.

1. [Azure portala](http://portal.azure.com) gidin.

1. [Azure Cloud Shell](/azure/cloud-shell/overview)'i açın. Önceden bir ortam seçmediyseniz **Bash** ortamını seçin.

    ![Cloud Shell istemi](./media/terraform-create-k8s-cluster-with-tf-and-aks/azure-portal-cloud-shell-button-min.png)

1. `clouddrive` dizinine geçin.

    ```bash
    cd clouddrive
    ```

1. `terraform-aks-k8s` adlı bir dizin oluşturun.

    ```bash
    mkdir terraform-aks-k8s
    ```

1. Dizinleri yeni dizinle değiştirin:

    ```bash
    cd terraform-aks-k8s
    ```

## <a name="declare-the-azure-provider"></a>Azure sağlayıcısını tanımlama
Azure sağlayıcısını tanımlayan Terraform yapılandırma dosyasını yapılandırın.

1. Cloud Shell'de `main.tf` adlı bir dosya oluşturun.

    ```bash
    vi main.tf
    ```

1. I tuşunu seçerek ekleme moduna geçin.

1. Aşağıdaki kodu düzenleyiciye yapıştırın:

    ```JSON
    provider "azurerm" {
        version = "=1.5.0"
    }

    terraform {
        backend "azurerm" {}
    }

    ```

1. **Esc** tuşuna basarak ekleme modundan çıkın.

1. Dosyayı kaydedin ve aşağıdaki komutu girerek VI düzenleyicisini kapatın:

    ```bash
    :wq
    ```

## <a name="define-a-kubernetes-cluster"></a>Kubernetes kümesi tanımlama
Kubernetes kümesinin kaynaklarını tanımlayan Terraform yapılandırma dosyasını oluşturun.

1. Cloud Shell'de `k8s.tf` adlı bir dosya oluşturun.

    ```bash
    vi k8s.tf
    ```

1. I tuşunu seçerek ekleme moduna geçin.

1. Aşağıdaki kodu düzenleyiciye yapıştırın:

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

    Yukarıdaki kod kümenin adını, konumunu ve resource_group_name değerini belirler. Ayrıca kümeye erişmek için kullanılan tam etki alanı adının (FQDN) bir bölümünü oluşturan dns_prefix değeri de ayarlanır.

    **linux_profile** kaydı, SSH kullanarak çalışan düğümlerinde oturum açmanızı sağlayan ayarları yapılandırmanızı sağlar.

    AKS ile yalnızca çalışan düğümleri için ödeme yaparsınız. **agent_pool_profile** kaydı bu çalışan düğümlerinin ayrıntılarını yapılandırmanızı sağlar. **agent_pool_profile record** ile oluşturulacak çalışan düğümü sayısı ve çalışan düğümlerinin türü belirtilir. İleride kümenin ölçeğini artırmanız veya azaltmanız gerekirse bu kaydın içindeki **count** değerini değiştirebilirsiniz.

1. **Esc** tuşuna basarak ekleme modundan çıkın.

1. Dosyayı kaydedin ve aşağıdaki komutu girerek VI düzenleyicisini kapatın:

    ```bash
    :wq
    ```

## <a name="declare-the-variables"></a>Değişkenleri tanımlama

1. Cloud Shell'de `variables.tf` adlı bir dosya oluşturun.

    ```bash
    vi variables.tf
    ```

1. I tuşunu seçerek ekleme moduna geçin.

1. Aşağıdaki kodu düzenleyiciye yapıştırın:

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

1. **Esc** tuşuna basarak ekleme modundan çıkın.

1. Dosyayı kaydedin ve aşağıdaki komutu girerek VI düzenleyicisini kapatın:

    ```bash
    :wq
    ```

## <a name="create-a-terraform-output-file"></a>Terraform çıkış dosyası oluşturma
[Terraform çıkışları](https://www.terraform.io/docs/configuration/outputs.html), Terraform bir plan uyguladığında kullanıcı için vurgulanabilecek değerleri tanımlamanızı sağlar. Bu değerler `terraform output` komutuyla sorgulanabilir. Bu bölümde [kubectl](https://kubernetes.io/docs/reference/kubectl/overview/) ile kümeye erişmenizi sağlayan bir çıkış dosyası oluşturacaksınız.

1. Cloud Shell'de `output.tf` adlı bir dosya oluşturun.

    ```bash
    vi output.tf
    ```

1. I tuşunu seçerek ekleme moduna geçin.

1. Aşağıdaki kodu düzenleyiciye yapıştırın:

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

1. **Esc** tuşuna basarak ekleme modundan çıkın.

1. Dosyayı kaydedin ve aşağıdaki komutu girerek VI düzenleyicisini kapatın:

    ```bash
    :wq
    ```

## <a name="set-up-azure-storage-to-store-terraform-state"></a>Terraform durumunu depolamak için Azure depolama alanı ayarlama
Terraform, durumu `terraform.tfstate` dosyasıyla yerel olarak izler. Bu model tek kişilik bir ortamda iyi çalışır. Ancak birden çok kişinin bulunduğu bir ortamda [Azure depolamayı](/azure/storage/) kullanarak durumu sunucuda izlemeniz gerekebilir. Bu bölümde gerekli depolama hesabı bilgilerini (hesap adı ve hesap anahtarı) alacak ve Terraform durum bilgilerinin depolanacağı bir depolama kapsayıcısı oluşturacaksınız.

1. Azure portalda sol taraftaki menüden **Tüm hizmetler**'i seçin.

1. **Depolama hesapları**’nı seçin.

1. **Depolama hesapları** sekmesinde Terraform durum bilgilerinin depolanacağı depolama hesabının adını seçin. Örneğin Cloud Shell'i ilk açtığınızda oluşturulmuş olan depolama hesabını kullanabilirsiniz.  Cloud Shell tarafından oluşturulan depolama hesabı genellikle `cs` ile başlar ve sonrasında rastgele sayı ve harf dizesi bulunur. **Daha sonra ihtiyacınız olacağından seçtiğiniz depolama hesabının adını not edin.**

1. Depolama hesabı sekmesinde **Erişim anahtarları**'nı seçin.

    ![Depolama hesabı adı](./media/terraform-create-k8s-cluster-with-tf-and-aks/storage-account.png)

1. **key1** **anahtar** değerini not edin. (Anahtarın sağ tarafındaki simgeyi seçtiğinizde değer panoya kopyalanır.)

    ![Depolama hesabı erişim anahtarları](./media/terraform-create-k8s-cluster-with-tf-and-aks/storage-account-access-key.png)

1. Cloud Shell'de Azure depolama hesabınızda bir kapsayıcı oluşturun (&lt;YourAzureStorageAccountName> ve &lt;YourAzureStorageAccountAccessKey> yer tutucularının yerine Azure depolama hesabınıza ait değerleri girin).

    ```bash
    az storage container create -n tfstate --account-name <YourAzureStorageAccountName> --account-key <YourAzureStorageAccountKey>
    ```

## <a name="create-the-kubernetes-cluster"></a>Kubernetes kümesi oluşturma
Bu bölümde `terraform init` komutunu kullanarak önceki bölümlerde oluşturduğunuz yapılandırma dosyalarında tanımlanan kaynakları oluşturmayı öğreneceksiniz.

1. Cloud Shell'de Terraform'u başlatın (&lt;YourAzureStorageAccountName> ve &lt;YourAzureStorageAccountAccessKey> yer tutucularının yerine Azure depolama hesabınıza ait değerleri girin).

    ```bash
    terraform init -backend-config="storage_account_name=<YourAzureStorageAccountName>" -backend-config="container_name=tfstate" -backend-config="access_key=<YourStorageAccountAccessKey>" -backend-config="key=codelab.microsoft.tfstate" 
    ```
    
    `terraform init` komutu arka uç ve sağlayıcı eklentisinin başarıyla başlatıldığını gösterir:

    !["terraform init" komutunun sonuçları](./media/terraform-create-k8s-cluster-with-tf-and-aks/terraform-init-complete.png)

1. `terraform plan` komutunu çalıştırarak altyapı öğelerini tanımlayan Terraform planını oluşturun. Komut için iki değer gerekir: **var.client_id** ve **var.client_secret**. **var.client_id** değişkeni için hizmet sorumlunuzla ilişkilendirilmiş olan **appId** değerini girin. **var.client_secret** değişkeni için hizmet sorumlunuzla ilişkilendirilmiş olan **password** değerini girin.

    ```bash
    terraform plan -out out.plan
    ```

    `terraform plan` komutu, `terraform apply` komutunu çalıştırdığınızda oluşturularak kaynakları gösterir:

    !["terraform plan" komutunun sonuçları](./media/terraform-create-k8s-cluster-with-tf-and-aks/terraform-plan-complete.png)

1. Kubernetes kümesini oluşturma planını uygulamak için `terraform apply` komutunu çalıştırın. Kubernetes kümesi oluşturma işlemi birkaç dakika sürebilir ve bu durum Cloud Shell oturumunun zaman aşımına uğramasına neden olabilir. Cloud Shell oturumu zaman aşımına uğrarsa öğreticiyi tamamlamak için ["Zaman aşımına uğrayan Cloud Shell oturumunu kurtarma"](#recover-from-a-dloud-shell-timeout) bölümündeki adımları izleyin.

    ```bash
    terraform apply out.plan
    ```

    `terraform apply` komutu, yapılandırma dosyalarınızda tanımlı kaynakların oluşturulmasının sonuçlarını gösterir:

    !["terraform apply" komutunun sonuçları](./media/terraform-create-k8s-cluster-with-tf-and-aks/terraform-apply-complete.png)

1. Azure portalda soldaki menüden **Tüm hizmetler**'i seçerek yeni Kubernetes kümeniz için oluşturulan kaynakları görebilirsiniz.

    ![Cloud Shell istemi](./media/terraform-create-k8s-cluster-with-tf-and-aks/k8s-resources-created.png)

## <a name="recover-from-a-cloud-shell-timeout"></a>Zaman aşımına uğrayan Cloud Shell oturumunu kurtarma
Cloud Shell oturumu zaman aşımına uğrarsa kurtarmak için aşağıdaki adımları gerçekleştirebilirsiniz:

1. Cloud Shell oturumu başlatın.

1. Terraform yapılandırma dosyalarınızı içeren dizine geçin.

    ```bash
    cd /clouddrive/terraform-aks-k8s
    ```

1. Şu komutu çalıştırın:

    ```bash
    export KUBECONFIG=./azurek8s
    ```
    
## <a name="test-the-kubernetes-cluster"></a>Kubernetes kümesini test etme
Yeni oluşturulan kümeyi doğrulamak için Kubernetes araçlarını kullanabilirsiniz.

1. Terraform durumundaki Kubernetes yapılandırmasını alın ve kubectl tarafından okunabilecek bir dosyaya kaydedin.

    ```bash
    echo "$(terraform output kube_config)" > ./azurek8s
    ```

1. kubectl aracının doğru yapılandırmayı alabilmesi için bir ortam değişkeni ayarlayın.

    ```bash
    export KUBECONFIG=./azurek8s
    ```

1. Kümenin durumunu doğrulayın.

    ```bash
    kubectl get nodes
    ```

    Çalışan düğümlerinin ayrıntılarını görmeniz ve bu düğümlerin durumunun aşağıdaki görüntüde olduğu gibi **Ready** (Hazır) olması gerekir:

    ![kubectl aracı, Kubernetes kümenizin durumunu doğrulamanızı sağlar](./media/terraform-create-k8s-cluster-with-tf-and-aks/kubectl-get-nodes.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede Terraform ve AKS ile Kubernetes kümesi oluşturmayı öğrendiniz. Aşağıdaki kaynaklardan Azure'da Terraform kullanımı hakkında daha fazla bilgi edinebilirsiniz: 

 [Microsoft.com Terraform Hub'ı](https://docs.microsoft.com/azure/terraform/)  
 [Terraform Azure sağlayıcı belgeleri](http://aka.ms/terraform)  
 [Terraform Azure sağlayıcı kaynağı](http://aka.ms/tfgit)  
 [Terraform Azure modülleri](http://aka.ms/tfmodules)