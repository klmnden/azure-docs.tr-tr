---
title: Azure Kubernetes Service (AKS) kümesini Windows Server düğümleri RDP
description: RDP bağlantısı sorunlarını giderme ve bakım görevleri için Windows Server düğümleri ile Azure Kubernetes Service (AKS) kümesi oluşturmayı öğrenin.
services: container-service
author: tylermsft
ms.service: container-service
ms.topic: article
ms.date: 05/06/2019
ms.author: twhitney
ms.openlocfilehash: 6b5ebbab717a3db7c9b50549d2762df61c274131
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66307355"
---
# <a name="connect-with-rdp-to-azure-kubernetes-service-aks-cluster-windows-server-nodes-for-maintenance-or-troubleshooting"></a>Küme Windows Server düğümleri Bakımı veya sorun giderme için Azure Kubernetes Service (AKS) için RDP ile bağlanma

Azure Kubernetes Service (AKS) kümenizi yaşam döngüsü boyunca, bir AKS Windows Server düğümüne erişmek gerekebilir. Bu erişim, bakım, günlük toplama veya diğer sorun giderme işlemleri için olabilir. RDP kullanarak Windows Server AKS düğümleri erişebilirsiniz. Alternatif olarak, küme oluşturma sırasında kullanılan anahtar çiftinin erişiminiz AKS Windows Server düğümlerine erişmek için SSH kullanmak istediğiniz adımları izleyebilirsiniz [Azure Kubernetes Service (AKS) kümesi düğümleri içine SSH] [ssh-steps]. Güvenlik nedenleriyle, AKS düğümleri internet'e açık değildir.

Windows Server düğüm desteği şu anda aks'deki önizlemeye sunulmuştur.

Bu makalede, özel IP adreslerini kullanarak bir AKS düğümü ile RDP bağlantısı oluşturulacağını gösterir.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, bir Windows Server düğüm ile var olan bir AKS kümesi olduğunu varsayar. Bir AKS kümesi gerekirse makaleye bakın [Azure CLI kullanarak bir Windows kapsayıcısı ile bir AKS kümesi oluşturma][aks-windows-cli]. Sorun giderme için istediğiniz Windows Server düğümü için Windows yönetici kullanıcı adı ve parola gerekir. Ayrıca bir RDP istemcisi aşağıdaki gibi ihtiyacınız [Microsoft Uzak Masaüstü][rdp-mac].

Ayrıca Azure CLI Sürüm 2.0.61 gerekir veya daha sonra yüklü ve yapılandırılmış. Çalıştırma `az --version` sürümü bulmak için. Gerekirse yüklemek veya yükseltmek bkz [Azure CLI yükleme][install-azure-cli].

## <a name="deploy-a-virtual-machine-to-the-same-subnet-as-your-cluster"></a>Kümeniz aynı alt ağa bir sanal makine dağıtma

AKS kümenizin Windows Server düğümler dışarıdan erişilebilir IP adresi yok. Bir RDP bağlantısı için genel olarak erişilebilir bir IP adresine sahip bir sanal makine, Windows Server düğümler aynı alt ağda dağıtabilirsiniz.

Aşağıdaki örnekte adlı bir sanal makine oluşturulmaktadır *myVM* içinde *myResourceGroup* kaynak grubu. Değiştirin *$SUBNET_ID* , Windows Server düğüm havuzu tarafından kullanılan alt ağ kimliği.

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

Bir Kubernetes kümesini yönetmek için kullandığınız [kubectl][kubectl], Kubernetes komut satırı istemcisi. Azure Cloud Shell kullanıyorsanız `kubectl` zaten yüklü. Yüklenecek `kubectl` yerel olarak, [az aks yükleme-cli] [ az-aks-install-cli] komutu:
    
```azurecli-interactive
az aks install-cli
```

`kubectl` istemcisini Kubernetes kümenize bağlanacak şekilde yapılandırmak için [az aks get-credentials][az-aks-get-credentials] komutunu kullanın. Bu komut, kimlik bilgilerini indirir ve Kubernetes CLI'yi bunları kullanacak şekilde yapılandırır.

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

İç IP adresi kullanarak Windows Server düğümlerinin listesinde [kubectl alma] [ kubectl-get] komutu:

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

İşiniz bittiğinde, RDP bağlantısı için Windows Server düğümünün çıkmak sonra sanal makineye RDP oturumundan çıkın. Her iki RDP oturumları çıktıktan sonra sanal makineyle Sil [az vm delete] [ az-vm-delete] komutu:

```azurecli-interactive
az vm delete --resource-group myResourceGroup --name myVM
```

## <a name="next-steps"></a>Sonraki adımlar

Ek sorun giderme verilerini gerekiyorsa [Kubernetes ana düğüm günlüklerini görüntüleyin] [ view-master-logs] veya [Azure İzleyici][azure-monitor-containers].

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
