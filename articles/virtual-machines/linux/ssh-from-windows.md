---
title: Windows ile Linux Vm'leri için SSH anahtarlarını kullanma | Microsoft Docs
description: Oluşturmak ve azure'da Linux sanal makinesine bağlanmak için bir Windows bilgisayarda SSH anahtarlarını kullanma hakkında bilgi edinin.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-service-management,azure-resource-manager
ms.assetid: 2cacda3b-7949-4036-bd5d-837e8b09a9c8
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 11/26/2018
ms.author: cynthn
ms.openlocfilehash: 0ac62a99f5735647f67917d441645e30444b3818
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61473705"
---
# <a name="how-to-use-ssh-keys-with-windows-on-azure"></a>Azure'da Windows ile SSH anahtarlarını kullanma

Bu makalede oluşturma ve kullanma yolları *secure shell* (SSH) anahtarları oluşturmak ve azure'da bir Linux sanal makinesi (VM) bağlamak için bir Windows bilgisayarda. Bir Linux veya Macos'ta istemciden SSH anahtarları kullanmak için bkz: [hızlı](mac-create-ssh-keys.md) veya [ayrıntılı](create-ssh-keys-detailed.md) Kılavuzu.

[!INCLUDE [virtual-machines-common-ssh-overview](../../../includes/virtual-machines-common-ssh-overview.md)]

[!INCLUDE [virtual-machines-common-ssh-support](../../../includes/virtual-machines-common-ssh-support.md)]

## <a name="windows-packages-and-ssh-clients"></a>Windows paketlerini ve SSH istemcileri
Bağlanmak ve kullanarak azure'da Linux sanal makineleri yönetme bir *SSH istemcisi*. SSH komut oluşturma ve SSH anahtarlarını yönetme ve SSH bağlantılarına takımının bir Linux veya Macos'ta genellikle çalıştıran bilgisayarlar vardır. 

Windows bilgisayarlar, her zaman yüklü karşılaştırılabilir SSH komutları yoktur. Windows 10 'un en son sürümlerini sağlamak [OpenSSH istemcisi komutları](https://blogs.msdn.microsoft.com/commandline/2018/03/07/windows10v1803/) oluşturmak ve SSH anahtarlarını yönetme ve bir komut isteminden SSH bağlantıları oluşturmak için. Yeni Windows 10 sürümleri de dahil [Linux için Windows alt sistemi](https://docs.microsoft.com/windows/wsl/about) çalıştırın ve yardımcı programları gibi bir SSH istemcisi bir Bash kabuğunda içinde yerel olarak erişmek için. 

Yerel olarak yükleyebilirsiniz diğer ortak Windows SSH istemcileri aşağıdaki paketlerinde bulunur:

* [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/)
* [Windows için Git](https://git-for-windows.github.io/)
* [MobaXterm](https://mobaxterm.mobatek.net/)
* [Cygwin](https://cygwin.com/)

Bash hizmetinde kullanılabilir SSH yardımcı programları da kullanabilirsiniz [Azure Cloud Shell](../../cloud-shell/overview.md). 

* Cloud Shell web tarayıcınızda erişim [ https://shell.azure.com ](https://shell.azure.com) veya [Azure portalında](https://portal.azure.com). 
* Erişim Cloud Shell olarak Visual Studio Code içindeki bir terminal yükleyerek [Azure hesabı uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account).

## <a name="create-an-ssh-key-pair"></a>SSH anahtar çifti oluşturma
Windows üzerinde SSH anahtar çifti oluşturmak için iki seçenek aşağıdaki bölümlerde açıklanmaktadır. Kabuk komutu kullanabilirsiniz (`ssh-keygen`) veya bir GUI aracını (PuTTYgen).

### <a name="create-ssh-keys-with-ssh-keygen"></a>Ssh-keygen ile SSH anahtarları oluşturma

Windows üzerinde SSH istemci araçlarını destekleyen bir komut kabuğunu çalıştırın (veya Azure Cloud Shell'i kullanırsanız) kullanarak bir SSH anahtar çifti oluşturma `ssh-keygen` komutu. Aşağıdaki komutu yazın ve istemlerini yanıtlayın. Seçilen konumda bir SSH anahtar çiftiniz varsa bu dosyaların üzerine yazılır. 

```bash
ssh-keygen -t rsa -b 2048
```

Daha fazla arka plan ve bilgi için bkz. [hızlı](mac-create-ssh-keys.md) veya [ayrıntılı](create-ssh-keys-detailed.md) kullanarak SSH anahtarları oluşturma adımları `ssh-keygen`.

### <a name="create-ssh-keys-with-puttygen"></a>PuTTYgen ile SSH anahtarları oluşturma

SSH anahtarları oluşturmak için bir GUI tabanlı aracı kullanmayı tercih ederseniz, birlikte PuTTYgen anahtarı Oluşturucu kullanabilirsiniz [PuTTY yükleme paketini](https://www.chiark.greenend.org.uk/~sgtatham/putty/download.html). 

PuTTYgen ile bir SSH RSA anahtar çifti oluşturmak için:

1. PuTTYgen başlatın.

2. Tıklayın **oluşturmak**. Varsayılan olarak, 2048 bit SSH-2 RSA anahtarı PuTTYgen oluşturur.

4. Rastgele olma durumu için anahtar sağlamak için boş alanı fareyi hareket ettirin.

5. Ortak anahtar oluşturulduktan sonra isteğe bağlı olarak girin ve bir parolayı onaylayın. VM ile özel SSH anahtarı kimlik doğrulaması sırasında için parola istenir. Birisi özel anahtarınızı alırsa bir parola olmadığında, bunlar herhangi bir VM veya bu anahtarı kullanan bir hizmet için oturum açabilir. Bir parola oluşturmanız önerilir. Ancak, parolayı unutursanız, bunu kurtarma yolu yoktur.

6. Ortak anahtarı, pencerenin en üstünde görüntülenir. Tüm bu ortak anahtarı kopyalayın ve bir Linux VM oluşturduğunuzda, Azure portalında veya Azure Resource Manager şablonu yapıştırın. Belirleyebilirsiniz **ortak anahtarı Kaydet** bilgisayarınıza bir kopyasını kaydetmek için:

    ![PuTTY ortak anahtar dosyasını Kaydet](./media/ssh-from-windows/save-public-key.png)

7. Özel anahtarı PuTTy özel anahtar biçiminde (.ppk dosyasını) kaydetmek için isteğe bağlı olarak seçin **özel anahtarı Kaydet**. Daha sonra VM'ye SSH bağlantısı için PuTTY kullanmak üzere .ppk dosyasını gerekir.

    ![PuTTY özel anahtar dosyasını seçin](./media/ssh-from-windows/save-ppk-file.png)

    Özel anahtarı OpenSSH biçiminde kaydetmek istiyorsanız, birçok SSH istemcileri tarafından kullanılan özel anahtar biçimi seçin **dönüştürmeler** > **OpenSSH anahtarını dışarı aktar**.

## <a name="provide-an-ssh-public-key-when-deploying-a-vm"></a>Bir VM dağıtılırken bir SSH ortak anahtarı sağlayın

Kimlik doğrulaması için SSH anahtarları kullanan bir Linux VM oluşturmak için Azure portalı veya diğer yöntemleri kullanarak VM oluşturma sırasında SSH ortak anahtarınızı sağlayın.

Aşağıdaki örnek nasıl kopyalayıp bir Linux VM'si oluşturduğunuzda bu ortak anahtar Azure portalına gösterir. Ortak anahtarı genellikle ~/.ssh/authorized_key dizininde yeni sanal makinenize daha sonra depolanır.

   ![Azure portalında bir VM oluşturduğunuzda ortak anahtarı kullanır.](./media/ssh-from-windows/use-public-key-azure-portal.png)


## <a name="connect-to-your-vm"></a>Sanal makinenize bağlanma

Windows, Linux VM'ye SSH bağlantısından yapmanın bir yolu, bir SSH istemcisi kullanmaktır. Windows sisteminizde yüklü olan bir SSH istemcisi varsa veya Azure Cloud Shell'deki bash hizmetinde SSH Araçları'nı kullanıyorsanız tercih edilen yöntem budur. GUI tabanlı bir araç tercih ederseniz, PuTTY ile bağlanabilirsiniz.  

### <a name="use-an-ssh-client"></a>Bir SSH istemcisi kullanma
Ortak anahtarı, Azure VM üzerinde dağıtılmış ve yerel sisteminizde, IP adresini veya VM'nizi DNS adını kullanarak sanal makinenize yönelik SSH özel anahtarı ile. Değiştirin *azureuser* ve *myvm.westus.cloudapp.azure.com* yönetici kullanıcı adı ve tam etki alanı adı (veya IP adresi) ile aşağıdaki komutta:

```bash
ssh azureuser@myvm.westus.cloudapp.azure.com
```

Anahtar çiftinizi oluştururken bir parola yapılandırdıysanız, oturum açma işlemi sırasında istendiğinde parolayı girin.

VM tam zamanında erişim ilkesi kullanıyorsa, sanal Makineye bağlanmadan önce erişim istemeniz gerekir. Tam zamanında İlkesi hakkında daha fazla bilgi için bkz: [yalnızca kullanarak sanal makine erişimini yönetme zamanında İlkesi](../../security-center/security-center-just-in-time.md).

### <a name="connect-with-putty"></a>PuTTY ile bağlanma

Yüklediyseniz [PuTTY yükleme paketini](https://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) ve bir PuTTY özel anahtar (.ppk) dosyası, daha önce oluşturulan bir Linux VM PuTTY ile bağlanabilirsiniz.

1. Putty'yi başlatın.

2. Konak adı veya Azure portalından vm'nizin IP adresini girin:

    ![PuTTY yeni bağlantı Aç](./media/ssh-from-windows/putty-new-connection.png)

3. Seçin **bağlantı** > **SSH** > **Auth** kategorisi. Göz atın ve kendi PuTTY özel anahtar (.ppk dosyasını) seçin:

    ![Kimlik doğrulaması için PuTTY özel anahtarınızı seçin](./media/ssh-from-windows/putty-auth-dialog.png)

4. Tıklayın **açık** VM'nize bağlanmak için.

## <a name="next-steps"></a>Sonraki adımlar

* Ayrıntılı adımlar, seçenekleri ve Gelişmiş SSH anahtarları ile çalışma örnekleri için bkz. [SSH oluşturmaya ilişkin ayrıntılı adımlar anahtar çifti](create-ssh-keys-detailed.md).

* SSH anahtarları oluşturma ve Linux Vm'leri için SSH bağlantıları oluşturmak için Azure Cloud Shell'de PowerShell de kullanabilirsiniz. Bkz: [PowerShell Hızlı Başlangıç](../../cloud-shell/quickstart-powershell.md#ssh).

* SSH kullanarak Linux Vm'lerinizi bağlanmak için bkz zorlanıyorsanız [sorun giderme SSH bağlantıları için bir Azure Linux VM](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
