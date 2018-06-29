---
title: Azure Kubernetes hizmet (AKS) küme düğümleri içine SSH
description: Bir SSH bağlantısı ile bir Azure Kubernetes hizmet (AKS) küme düğümleri oluşturun.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 04/06/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 8f4c70814566a963d86c119044f9ed3ef3238483
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37099407"
---
# <a name="ssh-into-azure-kubernetes-service-aks-cluster-nodes"></a>Azure Kubernetes hizmet (AKS) küme düğümleri içine SSH

Bazen, bir Azure Kubernetes hizmet (AKS) düğümü bakım, günlük toplama veya diğer sorun giderme işlemleri için erişim gerekebilir. AKS düğümleri Internet'e açık değildir. Bir AKS düğümle bir SSH bağlantısı oluşturmak için bu belgede ayrıntılı adımları kullanın.

## <a name="reset-ssh-keys"></a>SSH anahtarlarını sıfırlama

SSH anahtarları olmadan bir AKS dağıtmış olmanız veya uygun SSH anahtarları erişimi değil, bunlar Azure portalını kullanarak sıfırlanabilir.

AKS kümenize göz atın, AKS düğümü (sanal makine) seçin ve seçin **parola sıfırlama** SSH ortak anahtarını sıfırlamak için.

![Sıfırlama parolası düğmesiyle AKS VM](media/aks-ssh/reset-password.png)

Seçin **sıfırlama SSH ortak anahtarı**, olan AKS küme kullanıcı adı girin **azueruser** varsayılan ve SSH ortak anahtarını kopyalama. Seçin **güncelleştirme** tamamlandığında.

![Sıfırlama parolası düğmesiyle AKS portal VM](media/aks-ssh/reset-password-2.png)

SSH anahtarı sıfırlama sonra karşılık gelen özel anahtarı kullanarak bir SSH bağlantısı oluşturabilirsiniz.

## <a name="get-aks-node-address"></a>AKS düğüm adresi alın

AKS küme düğümünü kullanarak bir IP adresi al `az vm list-ip-addresses` komutu. Kaynak grubu adı AKS kaynak grubu adıyla değiştirin.

```console
$ az vm list-ip-addresses --resource-group MC_myAKSCluster_myAKSCluster_eastus -o table

VirtualMachine            PrivateIPAddresses
------------------------  --------------------
aks-nodepool1-42032720-0  10.240.0.6
aks-nodepool1-42032720-1  10.240.0.5
aks-nodepool1-42032720-2  10.240.0.4
```

## <a name="create-ssh-connection"></a>SSH bağlantısı oluşturma

Çalıştırma `debian` kapsayıcı görüntü ve terminal oturumu eklemektir. Kapsayıcı, ardından bir SSH oturumu AKS kümedeki herhangi bir düğüm oluşturmak için kullanılabilir.

```console
kubectl run -it --rm aks-ssh --image=debian
```

Bir SSH istemcisi kapsayıcısında yükleyin.

```console
apt-get update && apt-get install openssh-client -y
```

İkinci Terminali açın ve yeni oluşturulan pod adını almak için tüm pod'ları listeler.

```console
$ kubectl get pods

NAME                       READY     STATUS    RESTARTS   AGE
aks-ssh-554b746bcf-kbwvf   1/1       Running   0          1m
```

Özel SSH anahtarına pod kopyalayın, pod adını uygun değerle değiştirin.

```console
kubectl cp ~/.ssh/id_rsa aks-ssh-554b746bcf-kbwvf:/id_rsa
```

Güncelleştirme `id_rsa` dosya salt okunur kullanıcı olmasını sağlayın.

```console
chmod 0600 id_rsa
```

Artık bir SSH bağlantısı AKS düğüme oluşturun. Varsayılan kullanıcı adı bir AKS kümesi için `azureuser`. Küme oluşturma sırasında bu hesabı değiştirilmişse, uygun yönetici kullanıcı adı yerine koyun.

```console
$ ssh -i id_rsa azureuser@10.240.0.6

Welcome to Ubuntu 16.04.3 LTS (GNU/Linux 4.11.0-1016-azure x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

48 packages can be updated.
0 updates are security updates.


*** System restart required ***
Last login: Tue Feb 27 20:10:38 2018 from 167.220.99.8
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

azureuser@aks-nodepool1-42032720-0:~$
```

## <a name="remove-ssh-access"></a>SSH erişimini kaldırma

İşiniz bittiğinde, SSH oturumu ve ardından etkileşimli kapsayıcı oturum çıkın. Bu eylem AKS kümeden SSH erişim için kullanılan pod siler.
