---
title: "Oluşturma ve FreeBSD VM görüntüsü yükleme | Microsoft Docs"
description: "Oluşturma ve sanal sabit bir Azure sanal makine oluşturmak için FreeBSD işletim sistemi içeren bir disk (VHD) yükleme hakkında bilgi edinin"
services: virtual-machines-linux
documentationcenter: 
author: thomas1206
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 1ef30f32-61c1-4ba8-9542-801d7b18e9bf
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/08/2017
ms.author: huishao
ms.openlocfilehash: 7b41826f071174df8f00af56a228e0f31c3cfe2f
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2017
---
# <a name="create-and-upload-a-freebsd-vhd-to-azure"></a>Oluşturun ve Azure'a FreeBSD VHD yükleyin
Bu makalede nasıl oluşturulacağı ve bir sanal sabit FreeBSD işletim sistemini içeren disk (VHD) yüklemek gösterilmektedir. Karşıya yüklediğiniz sonra bunu kendi görüntünüzü Azure'da bir sanal makine (VM) oluşturmak için kullanabilirsiniz.

> [!IMPORTANT] 
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli kullanarak yer almaktadır. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. Resource Manager modelini kullanarak bir VHD'yi karşıya yükleme hakkında daha fazla bilgi için bkz: [burada](../upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="prerequisites"></a>Ön koşullar
Bu makalede, aşağıdaki öğelerin bulunduğunu varsayar:

* **Bir Azure aboneliği**--bir hesabınız yoksa yalnızca birkaç dakika içinde bir oluşturabilirsiniz. Bir MSDN aboneliğiniz varsa, bkz: [Visual Studio aboneleri için aylık Azure kredi](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). Aksi takdirde, bilgi nasıl [ücretsiz bir deneme hesabı oluşturma](https://azure.microsoft.com/pricing/free-trial/).  
* **Azure PowerShell Araçları**--Azure PowerShell modülü yüklenir ve Azure aboneliğinizi kullanmak üzere yapılandırılır. Modülünü yüklemek için bkz [Azure indirmeleri](https://azure.microsoft.com/downloads/). Modülünün yüklenmesi ve yapılandırılması açıklar bir öğretici burada kullanılabilir. Kullanım [Azure indirmeleri](https://azure.microsoft.com/downloads/) VHD yüklemek için cmdlet'i.
* **Bir .vhd dosyası yüklü FreeBSD işletim sistemi**--desteklenen bir FreeBSD işletim sistemi sanal sabit diske yüklenmesi gerekir. Birden çok araç, .vhd dosyaları oluşturmak için mevcut. Örneğin, .vhd dosyası oluşturun ve işletim sistemini yüklemek için Hyper-V gibi bir sanallaştırma çözümü kullanabilirsiniz. Yükleme ve Hyper-V kullanma hakkında yönergeler için bkz: [Hyper-V yükleyin ve sanal makine oluşturma](http://technet.microsoft.com/library/hh846766.aspx).

> [!NOTE]
> Azure'da yeni VHDX biçimi desteklenmiyor. Hyper-V Yöneticisi'ni veya cmdlet'ini kullanarak disk VHD biçimine dönüştürebilirsiniz [convert-vhd](https://technet.microsoft.com/library/hh848454.aspx). Ayrıca, bir [FreeBSD Hyper-V ile kullanma hakkında MSDN'de Öğreticisi](http://blogs.msdn.com/b/kylie/archive/2014/12/25/running-freebsd-on-hyper-v.aspx).
>
>

Bu görev aşağıdaki dört adımları içerir:

## <a name="step-1-prepare-the-image-for-upload"></a>1. adım: görüntüyü karşıya yükleme için hazırlama
FreeBSD işletim sisteminin yüklü olduğu sanal makine üzerinde aşağıdaki yordamları tamamlayın:

1. DHCP'yi etkinleştirin.

        # echo 'ifconfig_hn0="SYNCDHCP"' >> /etc/rc.conf
        # service netif restart
2. SSH etkinleştirin.

    SSH sunucusu yüklü ve önyükleme sırasında başlatılacak şekilde yapılandırılmış olduğundan emin olun. Varsayılan olarak FreeBSD disk yüklemesinden sonra etkin. 
3. Bir seri Konsolu ayarlayın.

        # echo 'console="comconsole vidconsole"' >> /boot/loader.conf
        # echo 'comconsole_speed="115200"' >> /boot/loader.conf
4. Sudo yükleyin.

    Kök hesap Azure'da devre dışıdır. Bu, sudo ayrıcalıksız bir kullanıcıdan komutları yükseltilmiş ayrıcalıklarla çalıştırmanıza olanak sağlamak gereken anlamına gelir.

        # pkg install sudo
   
5. Azure Aracısı önkoşulları.

        # pkg install python27  
        # pkg install Py27-setuptools  
        # ln -s /usr/local/bin/python2.7 /usr/bin/python   
        # pkg install git
6. Azure aracısını yükleyin.

    En son sürümü Azure Aracısı'nın her zaman bulunabilir [github](https://github.com/Azure/WALinuxAgent/releases). Sürüm 2.0.10 resmi olarak destekler FreeBSD 10 & 10.1 ve 2.1.4 sürüm + (2.2.x dahil) resmi olarak destekler FreeBSD 10.2 ve sonraki sürümlerinde.

        # git clone https://github.com/Azure/WALinuxAgent.git  
        # cd WALinuxAgent  
        # git tag  
        …
        WALinuxAgent-2.0.16
        …
        v2.1.4
        v2.1.4.rc0
        v2.1.4.rc1

    2.0 için 2.0.16 örnek olarak kullanalım:

        # git checkout WALinuxAgent-2.0.16
        # python setup.py install  
        # ln -sf /usr/local/sbin/waagent /usr/sbin/waagent  

    2.1 için şimdi bir örnek olarak 2.1.4 kullanabilirsiniz:

        # git checkout v2.1.4
        # python setup.py install  
        # ln -sf /usr/local/sbin/waagent /usr/sbin/waagent  
        # ln -sf /usr/local/sbin/waagent2.0 /usr/sbin/waagent2.0

   > [!IMPORTANT]
   > Azure aracısını yükledikten sonra çalıştığını doğrulamak için iyi bir fikirdir:
   >
   >

        # waagent -version
        WALinuxAgent-2.1.4 running on freebsd 10.3
        Python: 2.7.11
        # ps auxw | grep waagent
        root   639   0.0  0.5 104620 17520 u0- I    05:17    0:00.20 python /usr/local/sbin/waagent -daemon (python2.7)
        # cat /var/log/waagent.log
7. Sistem sağlamayı sonlandırın.

    Temizlemeyi ve sağlama işleminin için uygun hale sisteme sağlamayı sonlandırın. Aşağıdaki komut, son sağlanan kullanıcı hesabını ve ilişkili veriler de siler:

        # echo "y" |  /usr/local/sbin/waagent -deprovision+user  
        # echo  'waagent_enable="YES"' >> /etc/rc.conf

    Şimdi, VM kapatma.

## <a name="step-2-prepare-the-connection-to-azure"></a>2. adım: Azure bağlantısı hazırlama
Azure CLI Klasik dağıtım modelinde kullandığınızdan emin olun (`azure config mode asm`), hesabınızda oturum açın:

```azurecli
azure login
```


<a id="upload"> </a>


## <a name="step-3-upload-the-vhd-file"></a>3. adım: .vhd dosyasını karşıya yükle

VHD dosyanızı karşıya yüklemek için bir depolama hesabı gerekir. Mevcut bir depolama hesabını ya da seçebilirsiniz veya [yeni bir tane oluşturun](../../../storage/common/storage-create-storage-account.md).

İçinde Blob Depolama alanınızın .vhd dosyasını karşıya yüklediğinde, herhangi bir yere yerleştirebilirsiniz. Dosyayı karşıya yüklediğinizde kullanacağınız bazı şartları şunlardır:

* **BlobStorageURL** 2. adımda oluşturduğunuz depolama hesabı için URL.
* **YourImagesFolder** istediğiniz, görüntüleri depolamak için Blob storage kapsayıcısına olduğu.
* **VHDName** sanal sabit diski tanımlamak için Azure Klasik portalında görünen etiket.
* **PathToVHDFile** .vhd dosyasının tam yolu ve adıdır.

Önceki adımda kullanılan Azure PowerShell penceresinden yazın:

        Add-AzureVhd -Destination "<BlobStorageURL>/<YourImagesFolder>/<VHDName>.vhd" -LocalFilePath <PathToVHDFile>

## <a name="step-4-create-a-vm-with-the-uploaded-vhd-file"></a>4. adım: karşıya yüklenen .vhd dosyası bir VM oluşturma
.Vhd dosyasını yükledikten sonra bunu bir görüntü olarak aboneliğinizle ilişkili olan ve bu özel görüntü ile bir sanal makine oluşturmak özel resimler listesine ekleyebilirsiniz.

1. Önceki adımda kullanılan Azure PowerShell penceresinden yazın:

        Add-AzureVMImage -ImageName <Your Image's Name> -MediaLocation <location of the VHD> -OS <Type of the OS on the VHD>

   > [!NOTE]
   > Linux işletim sistemi türü olarak kullanın. Azure PowerShell'ın geçerli sürümü yalnızca "Linux" veya "Windows" parametre olarak kabul eder.
   >
   >
2. Yukarıdaki adımları tamamladıktan sonra seçtiğinizde yeni görüntü listelenir **görüntüleri** Klasik Azure portalı sekmesinde.  

    ![Bir görüntü seçin](./media/freebsd-create-upload-vhd/addfreebsdimage.png)
3. Galeriden bir sanal makine oluşturun. Bu yeni görüntüyü artık altında kullanılabilir **görüntülerim**.
4. Yeni görüntüyü seçin. Ardından, bir ana bilgisayar adı, parola, SSH anahtarı ve benzeri ayarlamak için istemleri geçer.

    ![Özel görüntü](./media/freebsd-create-upload-vhd/createfreebsdimageinazure.png)
5. Sağlama tamamlandıktan sonra Azure'da çalışan FreeBSD VM görürsünüz.

    ![Azure FreeBSD görüntüde](./media/freebsd-create-upload-vhd/freebsdimageinazure.png)
