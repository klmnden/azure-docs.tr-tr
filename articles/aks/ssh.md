---
title: Azure Kubernetes Service (AKS) kümesi düğümleri içine SSH
description: Sorun giderme ve bakım görevleri için Azure Kubernetes Service (AKS) küme düğümleri ile bir SSH bağlantısı oluşturmayı öğrenin.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 05/24/2019
ms.author: iainfou
ms.openlocfilehash: 57eacca75d711c5125a2856a7b6219cd2ec5306b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66242026"
---
# <a name="connect-with-ssh-to-azure-kubernetes-service-aks-cluster-nodes-for-maintenance-or-troubleshooting"></a>Küme düğümleri Bakımı veya sorun giderme için Azure Kubernetes Service (AKS) için SSH ile bağlanma

Azure Kubernetes Service (AKS) kümenizi yaşam döngüsü boyunca, bir AKS düğümü erişimi gerekebilir. Bu erişim, bakım, günlük toplama veya diğer sorun giderme işlemleri için olabilir. AKS düğümlerini (şu anda önizlemede aks'deki) Windows Server düğümler dahil olmak üzere, SSH kullanarak erişebilirsiniz. Ayrıca [Uzak Masaüstü Protokolü (RDP) bağlantıları kullanarak Windows Server düğümlere bağlanma][aks-windows-rdp]. Güvenlik nedenleriyle, AKS düğümleri internet'e açık değildir.

Bu makalede, özel IP adreslerini kullanarak bir AKS düğümü ile bir SSH bağlantısı oluşturulacağını gösterir.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, var olan bir AKS kümesi olduğunu varsayar. AKS hızlı bir AKS kümesi gerekirse bkz [Azure CLI kullanarak] [ aks-quickstart-cli] veya [Azure portalını kullanarak][aks-quickstart-portal].

Ayrıca Azure CLI Sürüm 2.0.64 gerekir veya daha sonra yüklü ve yapılandırılmış. Çalıştırma `az --version` sürümü bulmak için. Gerekirse yüklemek veya yükseltmek bkz [Azure CLI yükleme][install-azure-cli].

## <a name="add-your-public-ssh-key"></a>SSH ortak anahtarınızı ekleme

Varsayılan olarak, SSH anahtarları alınan veya oluşturulan ve AKS kümesi oluşturma düğümlere eklenmesi. AKS kümenizi oluştururken kullandığınız olanlardan farklı SSH anahtarları belirtmeniz gerekiyorsa, genel SSH anahtarınızı Linux AKS düğümlerine ekleyin. Gerekirse, bir SSH anahtarı kullanarak oluşturabilirsiniz [macOS veya Linux] [ ssh-nix] veya [Windows][ssh-windows]. PuTTY genel anahtar çifti oluşturmak için kullandığınız, bir OpenSSH içindeki anahtar çiftinden kaydetmek yerine varsayılan PuTTy özel anahtar biçimi (.ppk dosyasını) biçimlendirin.

> [!NOTE]
> SSH anahtarları can şu anda yalnızca Azure CLI kullanarak Linux düğümlere eklenmesi. Windows Server düğümleri kullanırsanız, AKS kümesi oluşturduğunuzda sağlanan SSH anahtarlarını kullanma ve üzerinde. adıma atlayın [AKS düğümü adresini almak nasıl](#get-the-aks-node-address). Veya, [Uzak Masaüstü Protokolü (RDP) bağlantıları kullanarak Windows Server düğümlere bağlanma][aks-windows-rdp].

AKS düğümleri özel IP adresini almak için adımları farklı çalıştırdığınız AKS kümesi türüne göre:

* AKS kümeler için adımları [normal AKS kümeler için IP adresini alma](#add-ssh-keys-to-regular-aks-clusters).
* Sanal makine ölçek kümeleri, birden çok düğüm havuzları veya Windows Server kapsayıcı desteği gibi kullanan, AKS içinde herhangi bir önizleme özelliği kullanırsanız [sanal makine ölçek kümesi tabanlı AKS kümeleri için adımları](#add-ssh-keys-to-virtual-machine-scale-set-based-aks-clusters).

### <a name="add-ssh-keys-to-regular-aks-clusters"></a>Normal AKS kümeye SSH anahtarları Ekle

Bir Linux AKS düğümüne SSH anahtarınızı eklemek için aşağıdaki adımları tamamlayın:

1. Kaynak grubu adını kullanarak AKS kümesi kaynaklarınız için alma [az aks show][az-aks-show]. Kendi temel kaynak grubunun ve AKS küme adı sağlayın. Küme adı adlı değişkene atanan *CLUSTER_RESOURCE_GROUP*:

    ```azurecli-interactive
    CLUSTER_RESOURCE_GROUP=$(az aks show --resource-group myResourceGroup --name myAKSCluster --query nodeResourceGroup -o tsv)
    ```

1. AKS küme kaynak grubu kullanarak Vm'leri listelemek [az vm listesini] [ az-vm-list] komutu. Bu VM'ler, AKS düğümleri şunlardır:

    ```azurecli-interactive
    az vm list --resource-group $CLUSTER_RESOURCE_GROUP -o table
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
      --resource-group $CLUSTER_RESOURCE_GROUP \
      --name aks-nodepool1-79590246-0 \
      --username azureuser \
      --ssh-key-value ~/.ssh/id_rsa.pub
    ```

### <a name="add-ssh-keys-to-virtual-machine-scale-set-based-aks-clusters"></a>Sanal makine ölçek kümesi tabanlı AKS kümeye SSH anahtarları Ekle

Bir sanal makine ölçek kümesinin bir parçası olan bir Linux AKS düğümüne SSH anahtarınızı eklemek için aşağıdaki adımları tamamlayın:

1. Kaynak grubu adını kullanarak AKS kümesi kaynaklarınız için alma [az aks show][az-aks-show]. Kendi temel kaynak grubunun ve AKS küme adı sağlayın. Küme adı adlı değişkene atanan *CLUSTER_RESOURCE_GROUP*:

    ```azurecli-interactive
    CLUSTER_RESOURCE_GROUP=$(az aks show --resource-group myResourceGroup --name myAKSCluster --query nodeResourceGroup -o tsv)
    ```

1. Ardından, sanal makine ölçek kümesi kullanarak AKS kümenizin için alın [az vmss listesi] [ az-vmss-list] komutu. Sanal makine ölçek kümesi adı adlı değişkene atanan *SCALE_SET_NAME*:

    ```azurecli-interactive
    SCALE_SET_NAME=$(az vmss list --resource-group $CLUSTER_RESOURCE_GROUP --query [0].name -o tsv)
    ```

1. Bir sanal makine ölçek kümesi düğümlerine SSH anahtarlarınızı eklemek için [az vmss uzantı kümesi] [ az-vmss-extension-set] komutu. Küme kaynak grubunu ve sanal makine ölçek kümesi adı, önceki komutlardan sağlanır. Varsayılan olarak, AKS düğümleri için kullanıcı adı: *azureuser*. Gerekirse, kendi SSH ortak anahtar konumunu, konumu gibi güncelleştirme *~/.ssh/id_rsa.pub*:

    ```azurecli-interactive
    az vmss extension set  \
        --resource-group $CLUSTER_RESOURCE_GROUP \
        --vmss-name $SCALE_SET_NAME \
        --name VMAccessForLinux \
        --publisher Microsoft.OSTCExtensions \
        --version 1.4 \
        --protected-settings "{\"username\":\"azureuser\", \"ssh_key\":\"$(cat ~/.ssh/id_rsa.pub)\"}"
    ```

1. SSH anahtarı kullanarak düğümlerine uygulamak [az vmss update-instances] [ az-vmss-update-instances] komutu:

    ```azurecli-interactive
    az vmss update-instances --instance-ids '*' \
        --resource-group $CLUSTER_RESOURCE_GROUP \
        --name $SCALE_SET_NAME
    ```

## <a name="get-the-aks-node-address"></a>AKS düğüm adresi alın

AKS düğümleri genel olarak internet'e açık değildir. AKS düğümleri için SSH için özel IP adresini kullanın. Sonraki adımda, yardımcı pod olanak sağlayan, AKS kümenizin SSH düğümü için bu özel IP adresi oluşturun. AKS düğümleri özel IP adresini almak için adımları farklı çalıştırdığınız AKS kümesi türüne göre:

* AKS kümeler için adımları [normal AKS kümeler için IP adresini alma](#ssh-to-regular-aks-clusters).
* Sanal makine ölçek kümeleri, birden çok düğüm havuzları veya Windows Server kapsayıcı desteği gibi kullanan, AKS içinde herhangi bir önizleme özelliği kullanırsanız [sanal makine ölçek kümesi tabanlı AKS kümeleri için adımları](#ssh-to-virtual-machine-scale-set-based-aks-clusters).

### <a name="ssh-to-regular-aks-clusters"></a>Normal AKS kümeleri için SSH

Bir AKS kümesi düğümü kullanma özel IP adresini görüntüleyin [az vm-IP-adreslerini] [ az-vm-list-ip-addresses] komutu. Önceki elde kendi AKS küme kaynak grubu adı girin [az aks show] [ az-aks-show] . adım:

```azurecli-interactive
az vm list-ip-addresses --resource-group $CLUSTER_RESOURCE_GROUP -o table
```

Aşağıdaki örnek çıktıda, AKS düğümleri özel IP adreslerini gösterir:

```
VirtualMachine            PrivateIPAddresses
------------------------  --------------------
aks-nodepool1-79590246-0  10.240.0.4
```

### <a name="ssh-to-virtual-machine-scale-set-based-aks-clusters"></a>Sanal makine ölçek kümesi tabanlı AKS kümeler için SSH

İç IP adresi kullanarak düğümlerin listesinde [kubectl Al komutu][kubectl-get]:

```console
kubectl get nodes -o wide
```

Aşağıdaki örnek çıktıda bir Windows Server düğümü ile birlikte kümeyi tüm düğümleri iç IP adreslerini gösterir.

```console
$ kubectl get nodes -o wide

NAME                                STATUS   ROLES   AGE   VERSION   INTERNAL-IP   EXTERNAL-IP   OS-IMAGE                    KERNEL-VERSION      CONTAINER-RUNTIME
aks-nodepool1-42485177-vmss000000   Ready    agent   18h   v1.12.7   10.240.0.4    <none>        Ubuntu 16.04.6 LTS          4.15.0-1040-azure   docker://3.0.4
aksnpwin000000                      Ready    agent   13h   v1.12.7   10.240.0.67   <none>        Windows Server Datacenter   10.0.17763.437
```

Sorun giderme istediğiniz düğümün iç IP adresini kaydedin. Daha sonraki bir adımda bu adresi kullanın.

## <a name="create-the-ssh-connection"></a>SSH bağlantısı oluşturun

Bir AKS düğümü için bir SSH bağlantısı oluşturmak için bir yardımcı pod AKS kümenizin çalıştırın. Bu yardımcı pod kümeyi ve ardından ek SSH düğümü erişimi SSH erişimi sağlar. Oluşturma ve bu Yardımcısı pod kullanmak için aşağıdaki adımları tamamlayın:

1. Çalıştıran bir `debian` kapsayıcı görüntü ve terminal oturumu ekleyebilir. Bu kapsayıcı, AKS kümesinde herhangi bir düğüm ile bir SSH oturumu oluşturmak için kullanılabilir:

    ```console
    kubectl run -it --rm aks-ssh --image=debian
    ```

    > [!TIP]
    > Windows Server düğümleri (şu anda önizlemede aks'deki) kullanırsanız, düğüm Seçicisi Debian kapsayıcı Linux düğümde şu şekilde zamanlamak için komuta ekleyin:
    >
    > `kubectl run -it --rm aks-ssh --image=debian --overrides='{"apiVersion":"apps/v1","spec":{"template":{"spec":{"nodeSelector":{"beta.kubernetes.io/os":"linux"}}}}}'`

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
[aks-windows-rdp]: rdp.md
[ssh-nix]: ../virtual-machines/linux/mac-create-ssh-keys.md
[ssh-windows]: ../virtual-machines/linux/ssh-from-windows.md
[az-vmss-list]: /cli/azure/vmss#az-vmss-list
[az-vmss-extension-set]: /cli/azure/vmss/extension#az-vmss-extension-set
[az-vmss-update-instances]: /cli/azure/vmss#az-vmss-update-instances
