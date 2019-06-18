---
title: Pod'ları Azure Kubernetes Service (AKS) tarafından kullanılmak üzere bir NFS (ağ dosya sistemi) Ubuntu sunucusu oluşturma
description: El ile pod'ları Azure Kubernetes Service (AKS) ile kullanım için bir NFS Ubuntu Linux Server birim oluşturmayı öğrenin
services: container-service
author: ozboms
ms.service: container-service
ms.topic: article
ms.date: 4/25/2019
ms.author: obboms
ms.openlocfilehash: 55eb5b0b98a4097d2f300bacabbfef3b0a32b27b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65468507"
---
# <a name="manually-create-and-use-an-nfs-network-file-system-linux-server-volume-with-azure-kubernetes-service-aks"></a>El ile oluşturup Azure Kubernetes Service (AKS) ile bir Linux sunucusu (ağ dosya sistemi) NFS birimi kullanın
Kapsayıcılar arasında veri paylaşımı, genellikle, kapsayıcı tabanlı hizmet ve uygulamaların gerekli bir bileşen olan. Genellikle bir dış kalıcı hacim aynı bilgiyi erişmesi gereken çeşitli pod'ları var.    
Azure dosyaları isteğe bağlı olsa da, bir Azure sanal makinesinde bir NFS sunucusunun oluşturma başka bir kalıcı paylaşılan depolama biçimidir. 

Bu makale, bir NFS sunucusunun bir Ubuntu sanal makinesi üzerinde oluşturma işlemini gösterir. Ve ayrıca bu paylaşılan dosya sistemine, AKS kapsayıcı erişim verin.

## <a name="before-you-begin"></a>Başlamadan önce
Bu makalede, var olan bir AKS kümesi olduğunu varsayar. AKS hızlı bir AKS kümesi gerekirse bkz [Azure CLI kullanarak] [ aks-quickstart-cli] veya [Azure portalını kullanarak][aks-quickstart-portal].

AKS kümenizi, NFS sunucusu olarak aynı veya eşlenmiş sanal ağlarda bulunan canlı gerekecektir. Küme, VM ile aynı sanal AĞA olabilen var olan bir VNET içinde oluşturulmalıdır.

Mevcut bir VNET ile yapılandırma adımları belgelerinde açıklanmıştır: [AKS kümesi mevcut bir VNET oluşturma] [ aks-virtual-network] ve [VNET eşlemesi ile sanal ağları bağlama][peer-virtual-networks]

Ayrıca, Ubuntu Linux sanal makinesi (örneğin, 18.04 LTS) oluşturduğunuz varsayılır. Ayarları ve boyutu dilediğiniz gibi olabilir ve Azure ile dağıtılabilir. Linux Hızlı Başlangıç için bkz: [linux VM Yönetimi][linux-create].

AKS kümenizi ilk dağıtırsanız, Azure otomatik olarak sanal ağ alanı, Ubuntu makineyi dağıtırken yönetilmelerini dolduracaktır aynı sanal ağ içinden Canlı. Ancak, bunun yerine eşlenen ağlarla çalışmak istiyorsanız, yukarıdaki belgelerine bakın.

## <a name="deploying-the-nfs-server-onto-a-virtual-machine"></a>NFS sunucusunu bir sanal makineye dağıtma
Betik, Ubuntu sanal makinesi içinde bir NFS sunucusunu ayarlamak için şu şekildedir:
```bash
#!/bin/bash

# This script should be executed on Linux Ubuntu Virtual Machine

EXPORT_DIRECTORY=${1:-/export/data}
DATA_DIRECTORY=${2:-/data}
AKS_SUBNET=${3:-*}

echo "Updating packages"
apt-get -y update

echo "Installing NFS kernel server"

apt-get -y install nfs-kernel-server

echo "Making data directory ${DATA_DIRECTORY}"
mkdir -p ${DATA_DIRECTORY}

echo "Making new directory to be exported and linked to data directory: ${EXPORT_DIRECTORY}"
mkdir -p ${EXPORT_DIRECTORY}

echo "Mount binding ${DATA_DIRECTORY} to ${EXPORT_DIRECTORY}"
mount --bind ${DATA_DIRECTORY} ${EXPORT_DIRECTORY}

echo "Giving 777 permissions to ${EXPORT_DIRECTORY} directory"
chmod 777 ${EXPORT_DIRECTORY}

parentdir="$(dirname "$EXPORT_DIRECTORY")"
echo "Giving 777 permissions to parent: ${parentdir} directory"
chmod 777 $parentdir

echo "Appending bound directories into fstab"
echo "${DATA_DIRECTORY}    ${EXPORT_DIRECTORY}   none    bind  0  0" >> /etc/fstab

echo "Appending localhost and Kubernetes subnet address ${AKS_SUBNET} to exports configuration file"
echo "/export        ${AKS_SUBNET}(rw,async,insecure,fsid=0,crossmnt,no_subtree_check)" >> /etc/exports
echo "/export        localhost(rw,async,insecure,fsid=0,crossmnt,no_subtree_check)" >> /etc/exports

nohup service nfs-kernel-server restart
```
Sunucu (betik nedeniyle) yeniden başlatılır ve AKS NFS sunucusuna bağlayabilir.

>[!IMPORTANT]  
>Değiştirdiğinizden emin olun **AKS_SUBNET** kümenizden doğru bir başka veya "*" tüm bağlantı noktaları ve bağlantılar, NFS sunucusu açılır.

Sanal makinenizin oluşturduktan sonra yukarıdaki betik bir dosyaya kopyalayın. Yerel makinenizden taşıdıktan sonra veya VM kullanarak betiği olduğunda: 
```console
scp /path/to/script_file username@vm-ip-address:/home/{username}
```
Betiğinizi, sanal olduktan sonra ssh VM'ye ve kullanabilirsiniz komutu yürütün:
```console
sudo ./nfs-server-setup.sh
```
İzin reddedildi hatası nedeniyle yürütme başarısız olursa, yürütme izni komutu ile ayarlayın:
```console
chmod +x ~/nfs-server-setup.sh
```

## <a name="connecting-aks-cluster-to-nfs-server"></a>NFS sunucusu için bağlantı AKS kümesi
Kalıcı hacim ve birime erişmek nasıl belirten kalıcı hacim talep sağlayarak ki kümemizde için NFS sunucusu bağlanabilirsiniz.  
Aynı veya eşlenmiş sanal ağlardaki iki hizmetlerine bağlanan gereklidir. Aynı sanal ağda küme kurma talimatları, burada: [AKS kümesi mevcut bir VNET oluşturma][aks-virtual-network]

Bunlar aynı sanal ağdaki (veya eşlendikten sonra), AKS kümenizin kalıcı hacim ve kalıcı hacim talep sağlanması gerekir. Kapsayıcıları, ardından kendi yerel dizin NFS sürücüye bağlayabilir.

(Bu tanımı aynı sanal ağda küme ve VM olduğu varsayılmıştır) kalıcı hacim için bir örnek kubernetes tanımı aşağıda verilmiştir:

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: <NFS_NAME>
  labels:
    type: nfs
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteMany
  nfs:
    server: <NFS_INTERNAL_IP>
    path: <NFS_EXPORT_FILE_PATH>
```
Değiştirin **NFS_INTERNAL_IP**, **NFS_NAME** ve **NFS_EXPORT_FILE_PATH** NFS sunucusu bilgileriyle.

Kalıcı hacim talep dosyası da gerekir. Dahil etmek bir örnek aşağıda verilmiştir:

>[!IMPORTANT]  
>**"storageClassName"** boş kalması gereken dize veya talep çalışmaz.

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: <NFS_NAME>
spec:
  accessModes:
    - ReadWriteMany
  storageClassName: ""
  resources:
    requests:
      storage: 1Gi
  selector: 
    matchLabels:
      type: nfs
```

## <a name="troubleshooting"></a>Sorun giderme
Bir kümeden sunucuya bağlanamıyorsanız, bir sorun dışarı aktarılan dizini veya üst olabilir, sunucuya erişmek için yeterli izinlere sahip değil.

Dışarı aktarma dizininize hem kendi ana dizini 777 izinlere sahip olduğunuzu denetleyin.

Aşağıdaki komutu çalıştırarak izinleri denetleyebilirsiniz ve dizinleri olmalıdır *'drwxrwxrwx'* izinleri:
```console
ls -l
```

## <a name="more-information"></a>Daha fazla bilgi
Tam bir kılavuza alın veya NFS sunucusu kurulumunuzu hatalarını ayıklamanıza yardımcı olmak için kapsamlı bir öğreticiden yararlanmayı aşağıda verilmiştir:
  - [NFS Öğreticisi][nfs-tutorial]

## <a name="next-steps"></a>Sonraki adımlar

İlişkili en iyi yöntemler için bkz: [en iyi uygulamalar için depolama ve yedekleme aks'deki][operator-best-practices-storage].

<!-- LINKS - external -->
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/volumes/
[linux-create]: https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-vm
[nfs-tutorial]: https://help.ubuntu.com/community/SettingUpNFSHowTo#Pre-Installation_Setup
[aks-virtual-network]: https://docs.microsoft.com/azure/aks/configure-kubenet#create-an-aks-cluster-in-the-virtual-network
[peer-virtual-networks]: https://docs.microsoft.com/azure/virtual-network/tutorial-connect-virtual-networks-portal

<!-- LINKS - internal -->
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[operator-best-practices-storage]: operator-best-practices-storage.md
