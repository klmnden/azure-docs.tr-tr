---
title: Azure Kubernetes Service (AKS) kümesi düğümleri içine SSH
description: Sorun giderme ve bakım görevleri için Azure Kubernetes Service (AKS) küme düğümleri ile bir SSH bağlantısı oluşturmayı öğrenin.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 03/05/2019
ms.author: iainfou
ms.openlocfilehash: 680e087e80d3e9891e201e7cb474ccfcf7fcc70b
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65072629"
---
# <a name="connect-with-ssh-to-azure-kubernetes-service-aks-cluster-nodes-for-maintenance-or-troubleshooting"></a>Küme düğümleri Bakımı veya sorun giderme için Azure Kubernetes Service (AKS) için SSH ile bağlanma

Azure Kubernetes Service (AKS) kümenizi yaşam döngüsü boyunca, bir AKS düğümü erişimi gerekebilir. Bu erişim, bakım, günlük toplama veya diğer sorun giderme işlemleri için olabilir. Bunlara erişebilmesi için Linux Vm'leri, AKS düğümleri olan SSH kullanarak. Güvenlik nedenleriyle, AKS düğümleri internet'e açık değildir.

Bu makalede, özel IP adreslerini kullanarak bir AKS düğümü ile bir SSH bağlantısı oluşturulacağını gösterir.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, var olan bir AKS kümesi olduğunu varsayar. AKS hızlı bir AKS kümesi gerekirse bkz [Azure CLI kullanarak] [ aks-quickstart-cli] veya [Azure portalını kullanarak][aks-quickstart-portal].

Ayrıca Azure CLI Sürüm 2.0.59 gerekir veya daha sonra yüklü ve yapılandırılmış. Çalıştırma `az --version` sürümü bulmak için. Gerekirse yüklemek veya yükseltmek bkz [Azure CLI yükleme][install-azure-cli].

## <a name="add-your-public-ssh-key"></a>SSH ortak anahtarınızı ekleme

Bir AKS kümesi oluşturduğunuzda varsayılan olarak, SSH anahtarları oluşturulur. AKS kümenizi oluştururken kendi SSH anahtarları belirtmediyseniz, ortak SSH anahtarları için AKS düğümleri ekleyin.

Bir AKS düğümüne SSH anahtarınızı eklemek için aşağıdaki adımları tamamlayın:

1. Kaynak grubu adını kullanarak AKS kümesi kaynaklarınız için alma [az aks show][az-aks-show]. Kendi temel kaynak grubunun ve AKS kümesinin adını sağlayın:

    ```azurecli-interactive
    az aks show --resource-group myResourceGroup --name myAKSCluster --query nodeResourceGroup -o tsv
    ```

1. AKS küme kaynak grubu kullanarak Vm'leri listelemek [az vm listesini] [ az-vm-list] komutu. Bu VM'ler, AKS düğümleri şunlardır:

    ```azurecli-interactive
    az vm list --resource-group MC_myResourceGroup_myAKSCluster_eastus -o table
    ```

    Aşağıdaki örnek çıktıda, AKS düğümleri gösterir:

    ```
    Name                      ResourceGroup                                  Location
    ------------------------  ---------------------------------------------  ----------
    aks-nodepool1-79590246-0  MC_myResourceGroupAKS_myAKSClusterRBAC_eastus  eastus
    ```

1. SSH anahtarlarınız düğüme eklemek için [az vm kullanıcı güncelleştirme] [ az-vm-user-update] komutu. Kaynak grubu adını ve ardından önceki adımda elde edilen AKS düğümleri birini sağlayın. Varsayılan olarak, AKS düğümleri için kullanıcı adı: *azureuser*. Kendi SSH ortak anahtar konumunu, konumunu sağlayın *~/.ssh/id_rsa.pub*, veya SSH ortak anahtarınızı içeriğini yapıştırın:

    ```azurecli-interactive
    az vm user update \
      --resource-group MC_myResourceGroup_myAKSCluster_eastus \
      --name aks-nodepool1-79590246-0 \
      --username azureuser \
      --ssh-key-value ~/.ssh/id_rsa.pub
    ```

## <a name="get-the-aks-node-address"></a>AKS düğüm adresi alın

AKS düğümleri genel olarak internet'e açık değildir. AKS düğümleri için SSH için özel IP adresini kullanın. Sonraki adımda, yardımcı pod olanak sağlayan, AKS kümenizin SSH düğümü için bu özel IP adresi oluşturun.

Bir AKS kümesi düğümü kullanma özel IP adresini görüntüleyin [az vm-IP-adreslerini] [ az-vm-list-ip-addresses] komutu. Önceki elde kendi AKS küme kaynak grubu adı girin [az aks show] [ az-aks-show] . adım:

```azurecli-interactive
az vm list-ip-addresses --resource-group MC_myAKSCluster_myAKSCluster_eastus -o table
```

Aşağıdaki örnek çıktıda, AKS düğümleri özel IP adreslerini gösterir:

```
VirtualMachine            PrivateIPAddresses
------------------------  --------------------
aks-nodepool1-79590246-0  10.240.0.4
```

## <a name="create-the-ssh-connection"></a>SSH bağlantısı oluşturun

Bir AKS düğümü için bir SSH bağlantısı oluşturmak için bir yardımcı pod AKS kümenizin çalıştırın. Bu yardımcı pod kümeyi ve ardından ek SSH düğümü erişimi SSH erişimi sağlar. Oluşturma ve bu Yardımcısı pod kullanmak için aşağıdaki adımları tamamlayın:

1. Çalıştıran bir `debian` kapsayıcı görüntü ve terminal oturumu ekleyebilir. Bu kapsayıcı, AKS kümesinde herhangi bir düğüm ile bir SSH oturumu oluşturmak için kullanılabilir:

    ```console
    kubectl run -it --rm aks-ssh --image=debian
    ```

1. Temel Debian görüntüsünü SSH bileşenlerini içermez. Terminal oturumunda kapsayıcıya bağlandıktan sonra bir SSH istemcisi kullanarak yükleme `apt-get` gibi:

    ```console
    apt-get update && apt-get install openssh-client -y
    ```

1. Pod'ları kullanarak AKS kümenizin üzerinde kapsayıcınıza, bağlı olmayan yeni bir terminal penceresinde, liste [kubectl pod'ları alma] [ kubectl-get] komutu. Önceki adımda oluşturduğunuz pod adıyla başlar *aks-ssh*, aşağıdaki örnekte gösterildiği gibi:

    ```
    $ kubectl get pods
    
    NAME                       READY     STATUS    RESTARTS   AGE
    aks-ssh-554b746bcf-kbwvf   1/1       Running   0          1m
    ```

1. Bu makalede, ilk adımda ortak SSH anahtarının AKS düğümü eklendi. Şimdi, özel SSH anahtarınızı pod kopyalayın. Bu özel anahtar, AKS düğümleri SSH oluşturmak için kullanılır.

    Kendi sağlamak *aks-ssh* pod adı, önceki adımda elde edilen. Gerekirse, değiştirme *~/.ssh/id_rsa* özel SSH anahtarınızı konumu:

    ```console
    kubectl cp ~/.ssh/id_rsa aks-ssh-554b746bcf-kbwvf:/id_rsa
    ```

1. Geri terminal oturumunda, kapsayıcıya izinler kopyalanan güncelleştirme `id_rsa` özel SSH anahtarı salt okunur kullanıcı olmasını sağlayın:

    ```console
    chmod 0600 id_rsa
    ```

1. Şimdi, AKS düğümü için bir SSH bağlantısı oluşturun. AKS düğümleri için varsayılan kullanıcı adı yeniden olan *azureuser*. SSH anahtarı ilk olarak, bağlantıya devam etme istemi güvenilir kabul edin. Ardından, AKS düğümü ile bash istemi sağlanır:

    ```console
    $ ssh -i id_rsa azureuser@10.240.0.4
    
    ECDSA key fingerprint is SHA256:A6rnRkfpG21TaZ8XmQCCgdi9G/MYIMc+gFAuY9RUY70.
    Are you sure you want to continue connecting (yes/no)? yes
    Warning: Permanently added '10.240.0.4' (ECDSA) to the list of known hosts.
    
    Welcome to Ubuntu 16.04.5 LTS (GNU/Linux 4.15.0-1018-azure x86_64)
    
     * Documentation:  https://help.ubuntu.com
     * Management:     https://landscape.canonical.com
     * Support:        https://ubuntu.com/advantage
    
      Get cloud support with Ubuntu Advantage Cloud Guest:
        https://www.ubuntu.com/business/services/cloud
    
    [...]
    
    azureuser@aks-nodepool1-79590246-0:~$
    ```

## <a name="remove-ssh-access"></a>SSH erişimini Kaldır

İşiniz bittiğinde, `exit` SSH oturumundan ardından `exit` etkileşimli kapsayıcı oturumu. Bu kapsayıcı oturumu kapattığında, AKS kümeye SSH erişimi için kullanılan pod silinir.

## <a name="next-steps"></a>Sonraki adımlar

Ek sorun giderme verilerini gerekiyorsa [kubelet günlüklerini görüntüleme] [ view-kubelet-logs] veya [Kubernetes ana düğüm günlüklerini görüntüleyin][view-master-logs].

<!-- EXTERNAL LINKS -->
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get

<!-- INTERNAL LINKS -->
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-vm-list]: /cli/azure/vm#az-vm-list
[az-vm-user-update]: /cli/azure/vm/user#az-vm-user-update
[az-vm-list-ip-addresses]: /cli/azure/vm#az-vm-list-ip-addresses
[view-kubelet-logs]: kubelet-logs.md
[view-master-logs]: view-master-logs.md
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
