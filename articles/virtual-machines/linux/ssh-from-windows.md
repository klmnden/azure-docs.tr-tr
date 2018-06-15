---
title: Linux VM'ler için SSH anahtarları kullanma | Microsoft Docs
description: Oluştur ve Azure Linux sanal makineye bağlanmak için bir Windows bilgisayarda SSH anahtarları kullanma hakkında bilgi edinin.
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
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/20/2018
ms.locfileid: "31601031"
---
# <a name="how-to-use-ssh-keys-with-windows-on-azure"></a>Windows Azure ile SSH anahtarları kullanma

Bu makalede oluşturmak ve güvenli Kabuk (SSH) anahtarları oluşturmak ve azure'da bir Linux sanal makine (VM) bağlanmak için bir Windows bilgisayarda kullanmak için yollar sağlar. Bir Linux veya macOS istemciden SSH anahtarları kullanmak için bkz: [hızlı](mac-create-ssh-keys.md) veya [ayrıntılı](create-ssh-keys-detailed.md) Kılavuzu.

[!INCLUDE [virtual-machines-common-ssh-overview](../../../includes/virtual-machines-common-ssh-overview.md)]

[!INCLUDE [virtual-machines-common-ssh-support](../../../includes/virtual-machines-common-ssh-support.md)]

## <a name="windows-packages-and-ssh-clients"></a>Windows paketleri ve SSH istemcileri
Bağlanmak ve Azure kullanarak Linux VM'ler yönetmek bir *SSH istemcisi*. Linux veya macOS genellikle çalıştıran bilgisayarlar SSH komutların oluşturmak ve SSH anahtarları yönetmek ve SSH bağlantısını yapmak için bir paketi vardır. 

Windows bilgisayarları her zaman yüklü karşılaştırılabilir SSH komutları sahip değildir. Dahil Windows 10 sürümleri [Linux için Windows alt](https://docs.microsoft.com/windows/wsl/about) çalıştırın ve yardımcı programlar bir Bash kabuğunda içinde yerel olarak bir SSH istemcisi gibi erişim olanak sağlar. 

Bir şey Windows dışındaki Bash kullanmak isterseniz, yerel olarak yükleyebilirsiniz ortak Windows SSH istemcilerini aşağıdaki paketlerinde bulunur:

* [puTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/)
* [Windows için Git](https://git-for-windows.github.io/)
* [MobaXterm](http://mobaxterm.mobatek.net/)
* [Cygwin](https://cygwin.com/)

Başka bir seçenek SSH yardımcı programları Bash'te kullanılabilir kullanmaktır [Azure bulut Kabuk](../../cloud-shell/overview.md). 

* Bulut Kabuk web tarayıcınızı şu erişim [ https://shell.azure.com ](https://shell.azure.com) veya [Azure portal](https://portal.azure.com). 
* Erişim bulut Kabuk olarak terminal durumundan Visual Studio Code içinde yükleyerek [Azure hesabı uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account).

## <a name="create-an-ssh-key-pair"></a>SSH anahtar çifti oluşturma
Bu bölümde, Windows'da bir SSH anahtar çifti oluşturmak için iki seçenek gösterir.

### <a name="create-ssh-keys-with-ssh-keygen"></a>Ssh-keygen ile SSH anahtarları oluşturma

Windows için Bash veya Gitbash'i (veya Bash Azure bulut Kabuğu'nda) gibi bir komut kabuğu çalıştırabilirsiniz, kullanarak bir SSH anahtar çifti oluşturma `ssh-keygen` komutu. Aşağıdaki komutu yazın ve komut istemlerini yanıtlayın. SSH anahtar çifti geçerli konumdan birinde varsa, bu dosyaların üzerine yazılır. 

```bash
ssh-keygen -t rsa -b 2048
```

Daha fazla arka plan ve bilgi için bkz: [hızlı](mac-create-ssh-keys.md) veya [ayrıntılı](create-ssh-keys-detailed.md) anahtarları oluşturmak için adımları `ssh-keygen`.

### <a name="create-ssh-keys-with-puttygen"></a>PuTTYgen ile SSH anahtarları oluşturma

SSH anahtarları oluşturmak için bir GUI tabanlı aracı kullanmayı tercih ederseniz, birlikte PuTTYgen anahtarı Oluşturucu kullanabilirsiniz [PuTTY indirme paketi](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html). 

Bir SSH RSA anahtar çifti ile PuTTYgen oluşturmak için:

1. PuTTYgen başlatın.

2. Tıklatın **oluşturmak**. Varsayılan olarak PuTTYgen 2048 bit SSH-2 RSA anahtarı oluşturur.

4. Bazı rastgele anahtar oluşturmak için boş alanı üzerinden fare.

5. Ortak anahtar oluşturulduktan sonra isteğe bağlı olarak girin ve bir parola onaylayın. SSH anahtarınızı VM'ye kimlik doğrulaması sırasında için parola istenir. Birisi özel anahtarınızı elde ederse bir parola, bunlar herhangi bir VM veya bu anahtarı kullanan hizmet oturum açabilir. Bir parola oluşturmanız önerilir. Ancak, parolayı unutursanız, bunu kurtarma yolu yoktur.

6. Ortak anahtar pencerenin üst kısmında görüntülenir. Kopyalayın ve bir Linux VM oluşturduğunuzda, bu tek satır biçimi ortak anahtar Azure portalında veya Azure Resource Manager şablonu yapıştırın. Tıklatarak **ortak anahtarı Kaydet** bilgisayarınıza bir kopyasını kaydetmek için:

    ![PuTTY ortak anahtar dosyasını kaydedin](./media/ssh-from-windows/save-public-key.png)

7. İsteğe bağlı olarak, özel anahtarı PuTTy özel anahtar biçiminde (.ppk dosyasını) kaydetmek için tıklatın **özel anahtarı Kaydet**. .Ppk dosyasını gereksinim PuTTY daha sonra VM için bir SSH bağlantısı için kullanmak istediğiniz biri.

    ![PuTTY özel anahtar dosyasını kaydedin](./media/ssh-from-windows/save-ppk-file.png)

    Özel anahtarı OpenSSH biçiminde kaydetmek istiyorsanız, çok sayıda SSH istemcileri tarafından kullanılan özel anahtar biçiminde tıklatın **dönüşümleri** > **OpenSSH dışarı aktarma anahtarı**.

## <a name="provide-ssh-public-key-when-deploying-a-vm"></a>SSH ortak anahtarı bir VM dağıtırken sağlayın

Kimlik doğrulaması için SSH anahtarları kullanan bir Linux VM oluşturmak için Azure Portalı'nı veya başka yöntemler kullanarak VM oluşturulurken SSH ortak anahtarınızı sağlar.

Aşağıdaki örnekte nasıl kopyalayıp bir Linux VM oluşturduğunuzda, bu ortak anahtar Azure portalına gösterir. Ortak anahtar genellikle sonra depolanır `~/.ssh/authorized_keys` , yeni bir VM üzerinde.

   ![Azure portalında bir VM oluşturduğunuzda ortak anahtarı kullan](./media/ssh-from-windows/use-public-key-azure-portal.png)


## <a name="connect-to-your-vm"></a>VM'nize bağlanmak

Windows, Linux VM için bir SSH bağlantısı yapmanın bir yolu, bir SSH istemcisi kullanmaktır. Bu tercih edilen Windows sisteminizde yüklü olan bir SSH istemcisi varsa veya Azure bulut Kabuk Bash'te SSH araçları kullandığınız yöntemdir. GUI tabanlı bir araç tercih ederseniz, PuTTY ile bağlanabilir.  

### <a name="use-an-ssh-client"></a>Bir SSH İstemcisi'ni kullanın
Azure VM'nizi dağıtılan ortak anahtar ve özel anahtarı yerel sisteminizde, IP adresi veya VM DNS adını kullanarak, VM SSH ile. Değiştir *azureuser* ve *myvm.westus.cloudapp.azure.com* aşağıdaki komut yönetici kullanıcı adı ve tam etki alanı adı (veya IP adresi ile):

```bash
ssh azureuser@myvm.westus.cloudapp.azure.com
```

Anahtar çifti oluşturduğunuzda, bir parola yapılandırdıysanız, oturum açma işlemi sırasında istendiğinde parolayı girin.

### <a name="connect-with-putty"></a>PuTTY ile bağlanma

Yüklediyseniz [PuTTY indirme paketi](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) ve PuTTY özel anahtar (.ppk dosyasını) daha önce oluşturulan Linux VM PuTTY ile bağlanabilir.

1. Putty'yi başlatın.

2. Ana bilgisayar adı veya IP adresi, VM'nin Azure portalından doldurun:

    ![PuTTY yeni bağlantı Aç](./media/ssh-from-windows/putty-new-connection.png)

3. Seçmeden önce **açık**, tıklatın **bağlantı** > **SSH** > **Auth** sekmesi. Göz atın ve PuTTY özel alt anahtarınızı (.ppk dosyası) seçin:

    ![Kimlik doğrulaması için PuTTY özel anahtarınızı seçin](./media/ssh-from-windows/putty-auth-dialog.png)

4. Tıklatın **açık** VM'nize bağlanmak için.

## <a name="next-steps"></a>Sonraki adımlar

* Ayrıntılı adımlar, seçenekleri ve SSH anahtarları ile çalışmaya ilişkin gelişmiş örnekler için bkz [SSH oluşturmak için ayrıntılı adımlar anahtar çiftleri](create-ssh-keys-detailed.md).

* SSH anahtarları oluşturma ve Linux VM'ler için SSH bağlantısını yapmak için Azure bulut Kabuğu'nda PowerShell de kullanabilirsiniz. Bkz: [PowerShell quickstart](../../cloud-shell/quickstart-powershell.md#ssh).

* SSH kullanarak Linux Vm'lerinizi bağlanmak için bkz: konusunda sorun yaşıyorsanız [sorun giderme SSH bağlantı Azure Linux VM'de](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
