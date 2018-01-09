---
title: "Oluşturma ve Azure'a Linux VHD yükleme | Microsoft Docs"
description: "Oluşturma ve bir Azure sanal sabit Klasik dağıtım modeli kullanarak Linux işletim sistemini içeren disk (VHD) yükleme"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
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
ms.author: iainfou
ms.openlocfilehash: 49cf4f1718e4dce1e86aa3c8921eaa8af5f16192
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2018
---
# <a name="creating-and-uploading-a-virtual-hard-disk-that-contains-the-linux-operating-system"></a>Linux İşletim Sistemini İçeren Bir Sanal Sabit Disk Oluşturma ve Karşıya Yükleme
> [!IMPORTANT] 
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli kullanarak yer almaktadır. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. Ayrıca [Azure Kaynak Yöneticisi'ni kullanarak bir özel disk görüntüsü karşıya](../upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Bu makalede oluşturmak ve Azure'da sanal makineler oluşturmak için kendi görüntünüzü kullanabilmek için bir sanal sabit disk (VHD) karşıya gösterilmektedir. Bu görüntüyü temel alarak birden çok sanal makine oluşturmak için kullanabileceğiniz şekilde işletim sistemini hazırla öğrenin. 


## <a name="prerequisites"></a>Önkoşullar
Bu makalede, aşağıdaki öğelerin bulunduğunu varsayar:

* **Bir .vhd dosyası yüklü Linux işletim sistemi** -yüklü olduğu bir [Linux Azure destekli dağıtım](../endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (veya bkz [desteklenmeyen dağıtımlarla bilgi](../create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) VHD biçiminde bir sanal disk için. Bir VM ve VHD oluşturmak için birden çok araç mevcuttur:
  * Yükleme ve yapılandırma [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) veya [KVM](http://www.linux-kvm.org/page/RunningKVM), alma, resim biçimi olarak VHD kullanmaya dikkat edin. Gerekirse, [bir görüntüyü dönüştürme](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) kullanarak `qemu-img convert`.
  * Hyper-V de kullanabilirsiniz [Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) veya [Windows Server 2012/2012 R2 üzerinde](https://technet.microsoft.com/library/hh846766.aspx).

> [!NOTE]
> Azure'da yeni VHDX biçimi desteklenmiyor. Bir VM oluşturduğunuzda, VHD biçiminde belirtin. Gerekirse, VHD kullanarak VHDX diskleri dönüştürebilirsiniz [ `qemu-img convert` ](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) veya [ `Convert-VHD` ](https://technet.microsoft.com/library/hh848454.aspx) PowerShell cmdlet'i. Ayrıca, Azure karşıya yüklemeden önce statik VHD'ler gibi diskleri dönüştürmeniz gerekir böylece dinamik VHD yüklemeyi desteklemez. Araçları gibi kullanabilir [Git için Azure VHD yardımcı programları](https://github.com/Microsoft/azure-vhd-utils-for-go) Azure'a karşıya yükleme işlemi sırasında dinamik diskleri dönüştürme.

* **Azure komut satırı arabirimi** -en yeni [Azure komut satırı arabirimi](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2) VHD yüklemek için.

<a id="prepimage"> </a>

## <a name="step-1-prepare-the-image-to-be-uploaded"></a>1. adım: karşıya yüklenecek görüntüsünü hazırlama
Azure çeşitli Linux dağıtımları destekler (bkz [destekli dağıtımlar](../endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). Aşağıdaki makaleler Azure üzerinde desteklenen çeşitli Linux dağıtımları hazırlanma konusunda size kılavuzluk eder. Aşağıdaki Kılavuzlar'ndaki adımları tamamladıktan sonra Azure'a yüklenmeye hazır bir VHD dosyasını bulduktan sonra tekrar buraya gelin:

* **[CentOS tabanlı dağıtımlar](../create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Debian Linux](../debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Oracle Linux](../oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Red Hat Enterprise Linux](../redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[SLES & openSUSE](../suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Ubuntu](../create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Diğer - desteklenmeyen dağıtımlarla](../create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**

> [!NOTE]
> İçinde 'Sürümleri desteklenir' altında belirtilen SLA geçerlidir yalnızca doğrulanan dağıtımları birini yapılandırmayla kullanıldığında, Linux işletim sistemi çalıştıran sanal makineler için Azure platformu ayrıntıları [Azure-Endorsed dağıtımları Linux'ta](../endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Tüm Linux dağıtımları Azure görüntü Galerisi, gerekli yapılandırma ile doğrulanan dağıtımları ' dir.
> 
> 

Ayrıca bkz.  **[Linux yükleme notları](../create-upload-generic.md#general-linux-installation-notes)**  için Azure Linux görüntüleri hazırlama hakkında daha fazla genel ipuçları için.

<a id="connect"> </a>

## <a name="step-2-prepare-the-connection-to-azure"></a>2. adım: Azure bağlantısı hazırlama
Azure CLI Klasik dağıtım modelinde kullandığınızdan emin olun (`azure config mode asm`), hesabınızda oturum açın:

```azurecli
azure login
```


<a id="upload"> </a>

## <a name="step-3-upload-the-image-to-azure"></a>3. adım: resim Azure'a yükleyin.
VHD dosyanızı karşıya yüklemek için bir depolama hesabı gerekir. Mevcut bir depolama hesabını ya da seçebilirsiniz veya [yeni bir tane oluşturun](../../../storage/common/storage-create-storage-account.md).

Aşağıdaki komutu kullanarak görüntüyü karşıya yükleme için Azure CLI kullanın:

```azurecli
azure vm image create <ImageName> `
    --blob-url <BlobStorageURL>/<YourImagesFolder>/<VHDName> `
    --os Linux <PathToVHDFile>
```

Önceki örnekte:

* **BlobStorageURL** kullanmayı düşünüyorsanız depolama hesabı URL'si
* **YourImagesFolder** , görüntüleri depolamak istediğiniz kapsayıcıdır blob depolama içinde
* **VHDName** sanal sabit diski tanımlamak için Portalı'nda görünen etiket.
* **PathToVHDFile** makinenizde .vhd dosyasının tam yolu ve adıdır.

Aşağıdaki komut, tam bir örnek gösterilmektedir:

```azurecli
azure vm image create myImage `
    --blob-url https://mystorage.blob.core.windows.net/vhds/myimage.vhd `
    --os Linux /home/ahmet/myimage.vhd
```

## <a name="step-4-create-a-vm-from-the-image"></a>4. adım: bir VM görüntüsünü oluşturma
Kullanarak bir VM oluşturun `azure vm create` normal bir VM ile aynı şekilde. Önceki adımda görüntünüzü verdiğiniz adı belirtin. Aşağıdaki örnekte, kullandığımız **myImage** önceki adımda belirtilen görüntü adı:

```azurecli
azure vm create --userName ops --password P@ssw0rd! --vm-size Small --ssh `
    --location "West US" "myDeployedVM" myImage
```

Kendi sanal makineleri oluşturmak için kendi kullanıcı adı + parola, konum, DNS adı ve görüntü adı sağlayın.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için bkz: [Azure Klasik dağıtım modeli için Azure CLI başvurusu](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).

[Step 1: Prepare the image to be uploaded]:#prepimage
[Step 2: Prepare the connection to Azure]:#connect
[Step 3: Upload the image to Azure]:#upload
