---
title: Oluşturma ve Azure'a bir Linux VHD yükleme | Microsoft Docs
description: Oluşturma ve bir Azure sanal Klasik dağıtım modelini kullanarak Linux işletim sistemini içeren sabit disk (VHD) yükleme
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-service-management
ROBOTS: NOINDEX
ms.assetid: 8058ff98-db03-4309-9bf4-69842bd64dd4
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 11/28/2016
ms.author: cynthn
ms.openlocfilehash: cdbe6aa5683ecf9d8bdaf6bbf9503ddc455f03ee
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37928276"
---
# <a name="creating-and-uploading-a-virtual-hard-disk-that-contains-the-linux-operating-system"></a>Linux İşletim Sistemini İçeren Bir Sanal Sabit Disk Oluşturma ve Karşıya Yükleme
> [!IMPORTANT] 
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modelini incelemektedir. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. Ayrıca [Azure Resource Manager kullanarak özel bir disk görüntüsünü karşıya yükleme](../upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Bu makalede oluşturmak ve Azure'da sanal makineler oluşturmak için kendi görüntünüzü kullanabilmeniz için bir sanal sabit disk (VHD) yükleme işlemini gösterir. İşletim sistemi bu görüntüyle birden çok sanal makine oluşturmak için kullanmak üzere hazırlamayı öğrenin. 


## <a name="prerequisites"></a>Önkoşullar
Bu makalede, aşağıdaki öğelerin bulunduğunu varsayar:

* **Linux işletim sistemi yüklü bir .vhd dosyasına** -yüklediğiniz bir [Azure destekli Linux dağıtım](../endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (veya [bilgi dağıtımlarla için](../create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) bir sanal disk için VHD biçimi. VM ve VHD oluşturmak için birden çok araçlar mevcuttur:
  * Yükleme ve yapılandırma [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) veya [KVM](http://www.linux-kvm.org/page/RunningKVM), alma, görüntü biçimi olarak VHD kullanmak için dikkat edin. Gerekirse, [görüntüyü dönüştürme](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) kullanarak `qemu-img convert`.
  * Hyper-V ayrıca kullanabileceğiniz [Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) veya [Windows Server 2012/2012 R2'deki](https://technet.microsoft.com/library/hh846766.aspx).

> [!NOTE]
> Azure'da yeni VHDX biçimi desteklenmiyor. Bir VM oluşturduğunuzda, VHD biçiminde belirtin. Gerekirse, VHD kullanarak VHDX diskler dönüştürülebilir [ `qemu-img convert` ](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) veya [ `Convert-VHD` ](https://technet.microsoft.com/library/hh848454.aspx) PowerShell cmdlet'i. Ayrıca, Azure gibi disklerin statik Vhd'lere karşıya yüklemeden önce dönüştürmeniz gerekmez dinamik VHD'ler, karşıya yükleme desteklemez. Gibi araçları kullanın [GO için Azure VHD'nin Utilities](https://github.com/Microsoft/azure-vhd-utils-for-go) Azure'a yükleme işlemi sırasında dinamik diskleri dönüştürme.

* **Azure komut satırı arabirimi** -en yeni [Azure komut satırı arabirimi](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2) VHD'yi karşıya yüklemek için.

<a id="prepimage"> </a>

## <a name="step-1-prepare-the-image-to-be-uploaded"></a>1. adım: karşıya yüklenecek görüntüsü hazırlama
Azure, çeşitli Linux dağıtımları destekler (bkz [desteklenen dağıtımı](../endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). Aşağıdaki makaleler Azure üzerinde desteklenen çeşitli Linux dağıtımları hazırlama size kılavuzluk eder. Aşağıdaki kılavuzlarda adımları tamamladıktan sonra Azure'a karşıya yüklenmeye hazır bir VHD dosyasını oluşturduktan sonra tekrar buraya gelin:

* **[CentOS tabanlı dağıtımlar](../create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Debian Linux](../debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Oracle Linux](../oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Red Hat Enterprise Linux](../redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[SLES & openSUSE](../suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Ubuntu](../create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Diğer - dağıtımlarla](../create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**

> [!NOTE]
> Desteklenen'sürümler ' altında belirtildiği gibi SLA'sı geçerlidir yalnızca desteklenen dağıtımlarla birini yapılandırma ile kullanıldığında, Linux işletim sistemi çalıştıran sanal makineler için Azure platformunda ayrıntıları [Azure-EndorseddağıtımlardaLinux](../endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Tüm Linux dağıtımları Azure görüntü galerisinde dağıtımlarla gerekli yapılandırma ' dir.
> 
> 

Ayrıca bkz: **[Linux yükleme notları](../create-upload-generic.md#general-linux-installation-notes)** Azure için Linux görüntüleri hazırlama hakkında daha fazla genel ipuçları için.

<a id="connect"> </a>

## <a name="step-2-prepare-the-connection-to-azure"></a>2. adım: Azure bağlantısı hazırlama
Klasik dağıtım modelinde Azure CLI'yı kullandığınızdan emin olun (`azure config mode asm`), hesabınızda oturum açın:

```azurecli
azure login
```


<a id="upload"> </a>

## <a name="step-3-upload-the-image-to-azure"></a>3. adım: daha sonra görüntüyü Azure'a yükleyebilir.
VHD dosyanızı karşıya yüklemek için bir depolama hesabı gerekir. Ya da mevcut bir depolama hesabını seçebilir veya [yeni bir tane oluşturun](../../../storage/common/storage-create-storage-account.md).

Azure CLI, aşağıdaki komutu kullanarak görüntüyü yüklemek için kullanın:

```azurecli
azure vm image create <ImageName> `
    --blob-url <BlobStorageURL>/<YourImagesFolder>/<VHDName> `
    --os Linux <PathToVHDFile>
```

Önceki örnekte:

* **BlobStorageURL** kullanmayı planladığınız depolama hesabı için URL
* **YourImagesFolder** , görüntülerinizi depolamak istediğiniz kapsayıcıdır blob depolama içinde
* **VHDName** sanal sabit diski tanımlamak için Portalı'nda görünen etiket.
* **PathToVHDFile** makinenizde .vhd dosyasının tam yolu ve adıdır.

Aşağıdaki komut, tam bir örnek gösterir:

```azurecli
azure vm image create myImage `
    --blob-url https://mystorage.blob.core.windows.net/vhds/myimage.vhd `
    --os Linux /home/ahmet/myimage.vhd
```

## <a name="step-4-create-a-vm-from-the-image"></a>4. adım: bir görüntüden VM oluşturma
Kullanarak bir VM oluşturun `azure vm create` normal bir VM aynı şekilde. Görüntünüzü önceki adımda belirlediğiniz adı belirtin. Aşağıdaki örnekte **Myımage** önceki adımda belirtilen görüntü adı:

```azurecli
azure vm create --userName ops --password P@ssw0rd! --vm-size Small --ssh `
    --location "West US" "myDeployedVM" myImage
```

Kendi sanal makineler oluşturmak için kendi kullanıcı adı + parola, konumu, DNS adı ve görüntü adı sağlayın.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için [Azure Klasik dağıtım modeli için Azure CLI başvurusu](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).

[Step 1: Prepare the image to be uploaded]:#prepimage
[Step 2: Prepare the connection to Azure]:#connect
[Step 3: Upload the image to Azure]:#upload
