---
title: Windows ile Linux Vm'leri için SSH anahtarlarını kullanma | Microsoft Docs
description: Oluşturmak ve azure'da Linux sanal makinesine bağlanmak için bir Windows bilgisayarda SSH anahtarlarını kullanma hakkında bilgi edinin.
services: virtual-machines-linux
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-service-management,azure-resource-manager
ms.assetid: 2cacda3b-7949-4036-bd5d-837e8b09a9c8
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: danlep
ms.openlocfilehash: d0762f80267fa927681344a3e0de78b0800c8306
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38630220"
---
# <a name="how-to-use-ssh-keys-with-windows-on-azure"></a>Azure'da Windows ile SSH anahtarlarını kullanma

Bu makalede, oluşturmak ve güvenli Kabuk (SSH) anahtarları oluşturmak ve azure'da bir Linux sanal makinesi (VM) bağlamak için bir Windows bilgisayarda yollarını açıklar. Bir Linux veya Macos'ta istemciden SSH anahtarları kullanmak için bkz: [hızlı](mac-create-ssh-keys.md) veya [ayrıntılı](create-ssh-keys-detailed.md) Kılavuzu.

[!INCLUDE [virtual-machines-common-ssh-overview](../../../includes/virtual-machines-common-ssh-overview.md)]

[!INCLUDE [virtual-machines-common-ssh-support](../../../includes/virtual-machines-common-ssh-support.md)]

## <a name="windows-packages-and-ssh-clients"></a>Windows paketlerini ve SSH istemcileri
Bağlanmak ve kullanarak azure'da Linux sanal makineleri yönetme bir *SSH istemcisi*. SSH komut oluşturma ve SSH anahtarlarını yönetme ve SSH bağlantılarına takımının bir Linux veya Macos'ta genellikle çalıştıran bilgisayarlar vardır. 

Windows bilgisayarlar, her zaman yüklü karşılaştırılabilir SSH komutları yoktur. İçeren Windows 10 sürümlerini [Linux için Windows alt sistemi](https://docs.microsoft.com/windows/wsl/about) çalıştırın ve yardımcı programları gibi bir SSH istemcisi bir Bash kabuğunda içinde yerel olarak erişim sağlar. 

Bash için Windows dışında bir şey kullanmak istiyorsanız, yerel olarak yükleyebilirsiniz ortak Windows SSH istemcileri aşağıdaki paketlerde dahildir:

* [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/)
* [Windows için Git](https://git-for-windows.github.io/)
* [MobaXterm](http://mobaxterm.mobatek.net/)
* [Cygwin](https://cygwin.com/)

Başka bir seçenek SSH yardımcı programları bash hizmetinde kullanılabilir kullanmaktır [Azure Cloud Shell](../../cloud-shell/overview.md). 

* Cloud Shell web tarayıcınızda erişim [ https://shell.azure.com ](https://shell.azure.com) veya [Azure portalında](https://portal.azure.com). 
* Erişim Cloud Shell olarak Visual Studio Code içindeki bir terminal yükleyerek [Azure hesabı uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account).

## <a name="create-an-ssh-key-pair"></a>SSH anahtar çifti oluşturma
Bu bölümde, Windows üzerinde SSH anahtar çifti oluşturmak için iki seçenek gösterir.

### <a name="create-ssh-keys-with-ssh-keygen"></a>Ssh-keygen ile SSH anahtarları oluşturma

Bash için Windows veya GitBash (veya Azure Cloud Shell'deki Bash hizmetinde) gibi bir komut kabuğunu çalıştırabileceğiniz, bir SSH anahtar çiftini kullanarak oluşturma `ssh-keygen` komutu. Aşağıdaki komutu yazın ve istemlerini yanıtlayın. Geçerli konumu bir SSH anahtar çiftiniz varsa bu dosyaların üzerine yazılır. 

```bash
ssh-keygen -t rsa -b 2048
```

Daha fazla arka plan ve bilgi için bkz. [hızlı](mac-create-ssh-keys.md) veya [ayrıntılı](create-ssh-keys-detailed.md) anahtarları oluşturmak için adımları `ssh-keygen`.

### <a name="create-ssh-keys-with-puttygen"></a>PuTTYgen ile SSH anahtarları oluşturma

SSH anahtarları oluşturmak için bir GUI tabanlı aracı kullanmayı tercih ederseniz, birlikte PuTTYgen anahtarı Oluşturucu kullanabilirsiniz [PuTTY yükleme paketini](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html). 

PuTTYgen ile bir SSH RSA anahtar çifti oluşturmak için:

1. PuTTYgen başlatın.

2. Tıklayın **oluşturmak**. Varsayılan olarak, 2048 bit SSH-2 RSA anahtarı PuTTYgen oluşturur.

4. Fare üzerindeyken bazı rastgele anahtarı oluşturmak için boş alan.

5. Ortak anahtar oluşturulduktan sonra isteğe bağlı olarak girin ve bir parolayı onaylayın. VM'ye SSH anahtarınız doğrulandığında için parola istenir. Birisi özel anahtarınızı alırsa bir parola olmadığında, bunlar herhangi bir VM veya bu anahtarı kullanan bir hizmet için oturum açabilir. Bir parola oluşturmanız önerilir. Ancak, parolayı unutursanız, bunu kurtarma yolu yoktur.

6. Ortak anahtarı, pencerenin en üstünde görüntülenir. Kopyalayın ve bir Linux VM'si oluşturduğunuzda bu tek satırlı biçimde ortak anahtarı Azure portalı veya Azure Resource Manager şablonu yapıştırın. Ayrıca **ortak anahtarı Kaydet** bilgisayarınıza bir kopyasını kaydetmek için:

    ![PuTTY ortak anahtar dosyasını Kaydet](./media/ssh-from-windows/save-public-key.png)

7. İsteğe bağlı olarak, özel anahtarı PuTTy özel anahtar biçiminde (.ppk dosyasını) kaydetmek için tıklatın **özel anahtarı Kaydet**. .Ppk dosyasını ihtiyacınız olan PuTTY daha sonra VM'ye SSH bağlantısı için kullanmak istediğiniz.

    ![PuTTY özel anahtar dosyasını seçin](./media/ssh-from-windows/save-ppk-file.png)

    Özel anahtarı OpenSSH biçiminde kaydetmek istiyorsanız, birçok SSH istemcileri tarafından kullanılan özel anahtar biçimi tıklayın **dönüştürmeler** > **OpenSSH anahtarını dışarı aktar**.

## <a name="provide-ssh-public-key-when-deploying-a-vm"></a>Bir VM dağıtılırken SSH ortak anahtarı sağlayın

Kimlik doğrulaması için SSH anahtarları kullanan bir Linux VM oluşturmak için Azure portalı veya diğer yöntemleri kullanarak VM oluşturma sırasında SSH ortak anahtarınızı sağlayın.

Aşağıdaki örnek nasıl kopyalayıp bir Linux VM'si oluşturduğunuzda bu ortak anahtar Azure portalına gösterir. Ortak anahtarı genellikle ardından depolanan `~/.ssh/authorized_keys` yeni vm'nizde.

   ![Azure portalında bir VM oluşturduğunuzda ortak anahtarı kullanır.](./media/ssh-from-windows/use-public-key-azure-portal.png)


## <a name="connect-to-your-vm"></a>Sanal Makinenize bağlanın

Windows, Linux VM'ye SSH bağlantısından yapmanın bir yolu, bir SSH istemcisi kullanmaktır. Tercih edilen yöntem Windows sisteminizde bir SSH istemcisi yüklü veya Azure Cloud Shell'deki bash hizmetinde SSH araçları kullanırsanız budur. GUI tabanlı bir araç tercih ederseniz, PuTTY ile bağlanabilirsiniz.  

### <a name="use-an-ssh-client"></a>Bir SSH istemcisi kullanma
Ortak anahtarı, Azure VM üzerinde dağıtılmış ve yerel sisteminizde, IP adresini veya VM'nizi DNS adını kullanarak sanal makinenize yönelik SSH özel anahtarı ile. Değiştirin *azureuser* ve *myvm.westus.cloudapp.azure.com* yönetici kullanıcı adı ve tam etki alanı adı (veya IP adresi) ile aşağıdaki komutta:

```bash
ssh azureuser@myvm.westus.cloudapp.azure.com
```

Anahtar çiftinizi oluştururken bir parola yapılandırdıysanız, oturum açma işlemi sırasında istendiğinde parolayı girin.

### <a name="connect-with-putty"></a>PuTTY ile bağlanma

Yüklediyseniz [PuTTY yükleme paketini](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) ve PuTTY özel anahtar (.ppk dosyasını) daha önce oluşturulan PuTTY ile Linux VM'ye bağlanabilirsiniz.

1. Putty'yi başlatın.

2. Konak adı veya Azure portalından vm'nizin IP adresini girin:

    ![PuTTY yeni bağlantı Aç](./media/ssh-from-windows/putty-new-connection.png)

3. Seçmeden önce **açık**, tıklayın **bağlantı** > **SSH** > **Auth** sekmesi. Göz atın ve kendi PuTTY özel anahtar (.ppk dosyasını) seçin:

    ![Kimlik doğrulaması için PuTTY özel anahtarınızı seçin](./media/ssh-from-windows/putty-auth-dialog.png)

4. Tıklayın **açık** VM'nize bağlanmak için.

## <a name="next-steps"></a>Sonraki adımlar

* Ayrıntılı adımlar, seçenekleri ve Gelişmiş SSH anahtarları ile çalışma örnekleri için bkz. [SSH oluşturmaya ilişkin ayrıntılı adımlar anahtar çifti](create-ssh-keys-detailed.md).

* SSH anahtarları oluşturma ve Linux Vm'leri için SSH bağlantıları oluşturmak için Azure Cloud Shell'de PowerShell de kullanabilirsiniz. Bkz: [PowerShell Hızlı Başlangıç](../../cloud-shell/quickstart-powershell.md#ssh).

* SSH kullanarak Linux Vm'lerinizi bağlanmak için bkz yaşıyorsanız [sorun giderme SSH bağlantıları için bir Azure Linux VM](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
