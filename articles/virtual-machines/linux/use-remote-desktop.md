---
title: "Azure'da bir Linux VM, Uzak Masaüstü'nü kullanın | Microsoft Docs"
description: "Yükleme ve uzak grafik araçları kullanarak azure'da bir Linux VM bağlanmak için Masaüstü (xrdp) yapılandırma hakkında bilgi edinin"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: iainfou
ms.openlocfilehash: 33b9c36d4600646c36a519e647bd8cc8b6d68666
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="install-and-configure-remote-desktop-to-connect-to-a-linux-vm-in-azure"></a>Yükleme ve azure'da bir Linux VM bağlanmak için Uzak Masaüstü yapılandırma
Azure'daki Linux sanal makineleri (VM'ler) genellikle bir güvenli Kabuk (SSH) bağlantısı kullanarak komut satırından yönetilir. Linux veya hızlı sorun giderme senaryoları için yeni, Uzak Masaüstü kullanımını daha kolay olabilir. Bu makalede yüklemek ve bir masaüstü ortamını yapılandırma ayrıntıları ([xfce](https://www.xfce.org)) ve Uzak Masaüstü'nü ([xrdp](http://www.xrdp.org)) Resource Manager dağıtım modeli kullanarak, Linux VM için.


## <a name="prerequisites"></a>Önkoşullar
Bu makalede, azure'da var olan bir Linux VM gerektirir. Bir VM oluşturmanız gerekiyorsa, aşağıdaki yöntemlerden birini kullanın:

- [Azure CLI 2.0](quick-create-cli.md)
- [Azure portalı](quick-create-portal.md)


## <a name="install-a-desktop-environment-on-your-linux-vm"></a>Bir masaüstü ortamı, Linux VM olanağına yükle
Çoğu Linux VM'ler için Azure'da varsayılan olarak yüklenen bir masaüstü ortamı yok. Linux VM'ler genellikle bir masaüstü ortamı yerine SSH bağlantısını kullanılarak yönetilir. Seçebileceğiniz Linux çeşitli masaüstü ortamlarında vardır. Tercih ettiğiniz Masaüstü ortamının bağlı olarak, bir için 2 GB disk alanı kullanabilir ve yüklemek ve tüm gerekli paketleri yapılandırmak için 5-10 dakika.

Aşağıdaki örnekte basit yükler [xfce4](https://www.xfce.org/) bir Ubuntu VM Masaüstü ortamı. Diğer dağıtımlar biraz farklılık için komutları (kullanmak `yum` Red Hat Enterprise Linux üzerinde yükleme ve uygun yapılandırma `selinux` kuralları ya da kullanım `zypper` SUSE üzerinde örneğin yüklemek için).

İlk olarak, SSH, VM. Aşağıdaki örnek adlı VM'ye bağlayan *myvm.westus.cloudapp.azure.com* kullanıcı adıyla *azureuser*:

```bash
ssh azureuser@myvm.westus.cloudapp.azure.com
```

Windows kullanıyorsanız ve SSH kullanma hakkında daha fazla bilgi için bkz: [SSH kullanma anahtarları ile Windows](ssh-from-windows.md).

Ardından, xfce kullanarak yükleyin `apt` gibi:

```bash
sudo apt-get update
sudo apt-get install xfce4
```

## <a name="install-and-configure-a-remote-desktop-server"></a>Uzak Masaüstü sunucusu yükleme ve yapılandırma
Yüklü bir masaüstü ortamı sahip olduğunuza göre gelen bağlantılarını dinlemek için bir Uzak Masaüstü hizmetini yapılandırın. [xrdp](http://xrdp.org) çoğu Linux dağıtımları hakkında kullanılabilir ve çalışır iyi xfce ile bir açık kaynak Uzak Masaüstü Protokolü (RDP) sunucusudur. Xrdp Ubuntu VM üzerinde aşağıdaki gibi yükleyin:

```bash
sudo apt-get install xrdp
```

Xrdp oturumunuz başlattığınızda kullanmak için hangi masaüstü ortamını bildirin. Xrdp xfce gibi masaüstü ortamı olarak kullanmak üzere yapılandırın:

```bash
echo xfce4-session >~/.xsession
```

Değişikliklerin etkili şekilde xrdp hizmetini yeniden başlatın:

```bash
sudo service xrdp restart
```


## <a name="set-a-local-user-account-password"></a>Bir yerel kullanıcı hesabı parolasını ayarlama
VM'nizi oluşturduğunuzda kullanıcı hesabınız için bir parola oluşturduysanız, bu adımı atlayın. Yalnızca SSH anahtar kimlik doğrulaması kullanmak ve yok bir yerel hesap parola ayarlayın, VM'nize oturum açmak için xrdp kullanmadan önce bir parola belirtmek istiyorsanız. xrdp kimlik doğrulaması için SSH anahtarları kabul edemez. Aşağıdaki örnekte kullanıcı hesabı için bir parola belirtir *azureuser*:

```bash
sudo passwd azureuser
```

> [!NOTE]
> Bir parola belirterek SSHD yapılandırmanızı şu anda mevcut değilse parola kullanarak oturum açamayan izin verecek şekilde güncelleştirmez. Güvenlik açısından bakıldığında, anahtar tabanlı kimlik doğrulaması kullanarak SSH tüneli ile VM'nize bağlanmak istediğiniz ve xrdp için bağlayın. Bu durumda, Uzak Masaüstü trafiğine izin veren bir ağ güvenlik grubu kural oluşturma ile ilgili aşağıdaki adımı atlayın.


## <a name="create-a-network-security-group-rule-for-remote-desktop-traffic"></a>Uzak Masaüstü trafiği için ağ güvenlik grubu kural oluşturma
Grup Kuralı gereksinimlerini, oluşturulacak Linux VM, ağ güvenliği ulaşmak Uzak Masaüstü trafiğine izin vermek için VM ulaşmak TCP bağlantı noktası 3389 sağlar. Ağ güvenlik grubu kuralları hakkında daha fazla bilgi için bkz: [bir ağ güvenlik grubu nedir?](../../virtual-network/virtual-networks-nsg.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Ayrıca [bir ağ güvenlik grubu kural oluşturmak için Azure portal'ı kullanmanızı](../windows/nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Aşağıdaki örnek, bir ağ güvenlik grubu kural ile oluşturur [az vm Aç-port](/cli/azure/vm#az_vm_open_port) bağlantı noktasında *3389*.

```azurecli
az vm open-port --resource-group myResourceGroup --name myVM --port 3389
```


## <a name="connect-your-linux-vm-with-a-remote-desktop-client"></a>Linux VM ile bir Uzak Masaüstü İstemcisi Bağlan
Yerel, Uzak Masaüstü İstemcisi'ni açın ve IP adresi ya da Linux VM DNS adı. Kullanıcı adı ve parola kullanıcı hesabı için VM üzerinde şu şekilde girin:

![Uzak Masaüstü İstemcisi'ni kullanarak xrdp Bağlan](./media/use-remote-desktop/remote-desktop-client.png)

Kimlik doğrulandıktan sonra xfce masaüstü ortamında yüklemek ve aşağıdaki örneğe benzer:

![xrdp aracılığıyla xfce Masaüstü ortamı](./media/use-remote-desktop/xfce-desktop-environment.png)


## <a name="troubleshoot"></a>Sorun giderme
Uzak Masaüstü İstemcisi'ni kullanarak, Linux VM'ime bağlanamıyorum kullanırsanız `netstat` VM gibi RDP bağlantıları dinliyor doğrulamak için Linux VM'de:

```bash
sudo netstat -plnt | grep rdp
```

Aşağıdaki örnek, beklendiği gibi 3389 numaralı TCP bağlantı noktasında dinleme VM gösterir:

```bash
tcp     0     0      127.0.0.1:3350     0.0.0.0:*     LISTEN     53192/xrdp-sesman
tcp     0     0      0.0.0.0:3389       0.0.0.0:*     LISTEN     53188/xrdp
```

Varsa *xrdp sesman* hizmet dinlemiyor, bir Ubuntu VM hizmeti aşağıdaki gibi yeniden başlatın:

```bash
sudo service xrdp restart
```

Gözden geçirme oturum açtığında */var/log* neden ilişkin göstergeleri için Ubuntu VM üzerinde hizmeti yanıt vermiyor olabilir. Syslog hataları görüntülemek için bir Uzak Masaüstü bağlantı girişimi sırasında da izleyebilirsiniz:

```bash
tail -f /var/log/syslog
```

Red Hat Enterprise Linux ve SUSE gibi diğer Linux dağıtımları Hizmetleri ve gözden geçirmek için alternatif günlük dosyası konumları yeniden başlatmak için farklı yollar olabilir.

Herhangi bir yanıt, Uzak Masaüstü İstemcisi almaz ve sistem günlüğündeki tüm olayları görmüyorsanız, Uzak Masaüstü trafik VM ulaşamıyor Bu davranış gösterir. TCP bağlantı noktası 3389 üzerinde izin vermek için bir kural olmasını sağlamak için ağ güvenlik grubu kurallarınızı gözden geçirin. Daha fazla bilgi için bkz: [uygulama bağlantı sorunlarını giderme](../windows/troubleshoot-app-connection.md).


## <a name="next-steps"></a>Sonraki adımlar
Oluşturma ve Linux VM'ler ile SSH anahtarları kullanma hakkında daha fazla bilgi için bkz: [oluşturma SSH anahtarları Azure Linux VM'ler için](mac-create-ssh-keys.md).

Windows SSH kullanma hakkında daha fazla bilgi için bkz: [SSH kullanma anahtarları ile Windows](ssh-from-windows.md).

