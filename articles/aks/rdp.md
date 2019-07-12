---
title: Azure Kubernetes Service (AKS) kümesini Windows Server düğümleri RDP
description: RDP bağlantısı sorunlarını giderme ve bakım görevleri için Windows Server düğümleri ile Azure Kubernetes Service (AKS) kümesi oluşturmayı öğrenin.
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 06/04/2019
ms.author: mlearned
ms.openlocfilehash: 0238278b81255d735f8a950ca307d0e05100cfec
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67614571"
---
# <a name="connect-with-rdp-to-azure-kubernetes-service-aks-cluster-windows-server-nodes-for-maintenance-or-troubleshooting"></a>Küme Windows Server düğümleri Bakımı veya sorun giderme için Azure Kubernetes Service (AKS) için RDP ile bağlanma

Azure Kubernetes Service (AKS) kümenizi yaşam döngüsü boyunca, bir AKS Windows Server düğümüne erişmek gerekebilir. Bu erişim, bakım, günlük toplama veya diğer sorun giderme işlemleri için olabilir. RDP kullanarak Windows Server AKS düğümleri erişebilirsiniz. Alternatif olarak, küme oluşturma sırasında kullanılan anahtar çiftinin erişiminiz AKS Windows Server düğümlerine erişmek için SSH kullanmak istediğiniz adımları izleyebilirsiniz [Azure Kubernetes Service (AKS) kümesi düğümleri içine SSH][ssh-steps]. Güvenlik nedenleriyle, AKS düğümleri internet'e açık değildir.

Windows Server düğüm desteği şu anda aks'deki önizlemeye sunulmuştur.

Bu makalede, özel IP adreslerini kullanarak bir AKS düğümü ile RDP bağlantısı oluşturulacağını gösterir.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, bir Windows Server düğüm ile var olan bir AKS kümesi olduğunu varsayar. Bir AKS kümesi gerekirse makaleye bakın [Azure CLI kullanarak bir Windows kapsayıcısı ile bir AKS kümesi oluşturma][aks-windows-cli]. You need the Windows administrator username and password for the Windows Server node you want to troubleshoot. You also need an RDP client such as [Microsoft Remote Desktop][rdp-mac].

Ayrıca Azure CLI Sürüm 2.0.61 gerekir veya daha sonra yüklü ve yapılandırılmış. Çalıştırma `az --version` sürümü bulmak için. Gerekirse yüklemek veya yükseltmek bkz [Azure CLI yükleme][install-azure-cli].

## <a name="deploy-a-virtual-machine-to-the-same-subnet-as-your-cluster"></a>Kümeniz aynı alt ağa bir sanal makine dağıtma

AKS kümenizin Windows Server düğümler dışarıdan erişilebilir IP adresi yok. Bir RDP bağlantısı için genel olarak erişilebilir bir IP adresine sahip bir sanal makine, Windows Server düğümler aynı alt ağda dağıtabilirsiniz.

Aşağıdaki örnekte adlı bir sanal makine oluşturulmaktadır *myVM* içinde *myResourceGroup* kaynak grubu.

İlk olarak, Windows Server düğüm havuzu tarafından kullanılan alt ağ alın. Alt ağ kimliğini almak için alt ağın adı gerekir. Alt ağın adını almak için sanal ağ adı gereklidir. Sanal ağ adına, kümeniz için alt ağlar listesi sorgulayarak alın. Küme sorgu adı gerekir. Azure Cloud Shell'de aşağıdaki çalıştırarak bunların hepsini alabilirsiniz:

```azurecli-interactive
CLUSTER_RG=$(az aks show -g myResourceGroup -n myAKSCluster --query nodeResourceGroup -o tsv)
VNET_NAME=$(az network vnet list -g $CLUSTER_RG --query [0].name -o tsv)
SUBNET_NAME=$(az network vnet subnet list -g $CLUSTER_RG --vnet-name $VNET_NAME --query [0].name -o tsv)
SUBNET_ID=$(az network vnet subnet show -g $CLUSTER_RG --vnet-name $VNET_NAME --name $SUBNET_NAME --query id -o tsv)
```

SUBNET_ID olduğuna göre VM oluşturmak için aynı Azure Cloud Shell penceresinde aşağıdaki komutu çalıştırın:

```azurecli-interactive
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image win2019datacenter \
    --admin-username azureuser \
    --admin-password myP@ssw0rd12 \
    --subnet $SUBNET_ID \
    --query publicIpAddress -o tsv
```

Aşağıdaki örnek çıktı, sanal makine başarıyla oluşturuldu ve sanal makinenin genel IP adresini görüntüler gösterilir.

```console
13.62.204.18
```

Sanal makinenin genel IP adresini kaydedin. Daha sonraki bir adımda bu adresi kullanın.

## <a name="get-the-node-address"></a>Düğüm adresi alın

Bir Kubernetes kümesini yönetmek için kullandığınız [kubectl][kubectl], Kubernetes komut satırı istemcisi. Azure Cloud Shell kullanıyorsanız `kubectl` zaten yüklü. Yüklenecek `kubectl` yerel olarak, [az aks yükleme-cli][az-aks-install-cli] komutu:
    
```azurecli-interactive
az aks install-cli
```

Yapılandırmak için `kubectl` Kubernetes kümenize bağlanmak için [az aks get-credentials][az-aks-get-credentials] komutu. Bu komut, kimlik bilgilerini indirir ve Kubernetes CLI'yi bunları kullanacak şekilde yapılandırır.

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

İç IP adresi kullanarak Windows Server düğümlerinin listesinde [kubectl alma][kubectl-get] komutu:

```console
kubectl get nodes -o wide
```

Aşağıdaki örnek çıktıda Windows Server düğümler dahil olmak üzere, bir kümedeki tüm düğümleri iç IP adreslerini gösterir.

```console
$ kubectl get nodes -o wide
NAME                                STATUS   ROLES   AGE   VERSION   INTERNAL-IP   EXTERNAL-IP   OS-IMAGE                    KERNEL-VERSION      CONTAINER-RUNTIME
aks-nodepool1-42485177-vmss000000   Ready    agent   18h   v1.12.7   10.240.0.4    <none>        Ubuntu 16.04.6 LTS          4.15.0-1040-azure   docker://3.0.4
aksnpwin000000                      Ready    agent   13h   v1.12.7   10.240.0.67   <none>        Windows Server Datacenter   10.0.17763.437
```

Sorun giderme istediğiniz Windows Server düğümünün iç IP adresini kaydedin. Daha sonraki bir adımda bu adresi kullanın.

## <a name="connect-to-the-virtual-machine-and-node"></a>Sanal makine ve düğümüne bağlanma

Daha önce bir RDP istemcisi gibi kullanarak oluşturduğunuz sanal makinenin genel IP adresine bağlanın [Microsoft Uzak Masaüstü][rdp-mac].

![Görüntü bir RDP istemcisi kullanarak sanal makineye bağlanma](media/rdp/vm-rdp.png)

Sanal makinenizi sanal makineye bağlandıktan sonra bağlanma *iç IP adresi* , sanal makine içinde bir RDP istemcisi kullanarak sorun giderme istediğiniz Windows Server düğümünün.

![Görüntü bir RDP istemcisi kullanarak Windows Server düğümüne bağlanma](media/rdp/node-rdp.png)

Şimdi, Windows Server düğümüne bağlanır.

![Windows Server düğümünde cmd penceresinden görüntüsü](media/rdp/node-session.png)

Artık herhangi bir sorun giderme komutunu çalıştırabilirsiniz *cmd* penceresi. Windows sunucu düğümlerine Windows Server Core kullandığından, yok tam GUI veya diğer GUI araçları bir Windows Server düğüme RDP bağlandığında.

## <a name="remove-rdp-access"></a>RDP erişimini Kaldır

İşiniz bittiğinde, RDP bağlantısı için Windows Server düğümünün çıkmak sonra sanal makineye RDP oturumundan çıkın. Her iki RDP oturumları çıktıktan sonra sanal makineyle Sil [az vm delete][az-vm-delete] komutu:

```azurecli-interactive
az vm delete --resource-group myResourceGroup --name myVM
```

## <a name="next-steps"></a>Sonraki adımlar

Ek sorun giderme verilerini gerekiyorsa [Kubernetes ana düğüm günlüklerini görüntüleyin][view-master-logs] or [Azure Monitor][azure-monitor-containers].

<!-- EXTERNAL LINKS -->
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[rdp-mac]: https://aka.ms/rdmac

<!-- INTERNAL LINKS -->
[aks-windows-cli]: windows-container-cli.md
[az-aks-install-cli]: /cli/azure/aks?view=azure-cli-latest#az-aks-install-cli
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-vm-delete]: /cli/azure/vm#az-vm-delete
[azure-monitor-containers]: ../azure-monitor/insights/container-insights-overview.md
[install-azure-cli]: /cli/azure/install-azure-cli
[ssh-steps]: ssh.md
[view-master-logs]: view-master-logs.md
