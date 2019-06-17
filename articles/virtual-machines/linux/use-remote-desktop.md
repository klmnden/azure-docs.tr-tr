---
title: Azure'da bir Linux VM'ye Uzak Masaüstü kullanma | Microsoft Docs
description: Yükleme ve uzak grafik araçları kullanarak azure'da bir Linux VM'ye bağlanmak için Masaüstü (xrdp) yapılandırma hakkında bilgi edinin
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/30/2018
ms.author: cynthn
ms.openlocfilehash: 56aa06ade50f6c0eb1467b1295cbebb907023398
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65209368"
---
# <a name="install-and-configure-remote-desktop-to-connect-to-a-linux-vm-in-azure"></a>Yükleme ve yapılandırma azure'da bir Linux VM'ye bağlanmak için Uzak Masaüstü
Azure'da Linux sanal makineleri (VM'ler), genellikle bir güvenli Kabuk (SSH) bağlantısı kullanarak komut satırından yönetilir. Yeni Linux veya hızlı sorun giderme senaryoları için Uzak Masaüstü kullanımını daha kolay olabilir. Bu makalede ayrıntılı bir masaüstü ortamını yüklemek ve yapılandırmak nasıl ([xfce](https://www.xfce.org)) ve Uzak Masaüstü ([xrdp](https://www.xrdp.org)) Resource Manager dağıtım modelini kullanarak Linux VM.


## <a name="prerequisites"></a>Önkoşullar
Bu makalede, azure'daki mevcut bir Ubuntu 16.04 LTS sanal gerektirir. Bir sanal makine oluşturmanız gerekiyorsa, aşağıdaki yöntemlerden birini kullanın:

- [Azure CLI](quick-create-cli.md)
- [Azure portalı](quick-create-portal.md)


## <a name="install-a-desktop-environment-on-your-linux-vm"></a>Linux sanal makinenizde bir masaüstü ortamını yükleyin
Azure'da çoğu Linux Vm'leri varsayılan olarak yüklenen bir masaüstü ortamı yok. Linux Vm'leri, yaygın olarak masaüstü ortamı yerine SSH bağlantısı kullanılarak yönetilir. Seçebileceğiniz Linux çeşitli masaüstü ortamlarında vardır. Seçtiğiniz masaüstü ortamı bağlı olarak biri için 2 GB disk alanı kullanmak ve yüklemek ve gerekli tüm paketleri yapılandırmak için 5-10 dakika sürer.

Aşağıdaki örnek, basit yükler [xfce4](https://www.xfce.org/) bir Ubuntu 16.04 LTS sanal masaüstü ortamında. Diğer dağıtımları biraz farklılık için komutları (kullanın `yum` Red Hat Enterprise Linux üzerinde yükleyip uygun yapılandırma `selinux` kuralları veya kullanım `zypper` SUSE üzerinde örneğin yüklemek için).

İlk olarak, sanal makinenize yönelik SSH. Aşağıdaki örnekte adlı VM'ye bağlayan *myvm.westus.cloudapp.azure.com* kullanıcı adıyla *azureuser*. Kendi değerlerinizi kullanın:

```bash
ssh azureuser@myvm.westus.cloudapp.azure.com
```

Windows kullanıyorsanız ve SSH kullanma hakkında daha fazla bilgiye ihtiyacınız varsa bkz [Windows ile SSH kullanma anahtarları](ssh-from-windows.md).

Ardından, xfce kullanarak yükleyin `apt` gibi:

```bash
sudo apt-get update
sudo apt-get install xfce4
```

## <a name="install-and-configure-a-remote-desktop-server"></a>Bir Uzak Masaüstü sunucusunu yükleme ve yapılandırma
Yüklü bir masaüstü ortamını olduğuna göre bir Uzak Masaüstü hizmeti, gelen bağlantıları için dinlemek üzere yapılandırın. [xrdp](http://xrdp.org) çoğu Linux dağıtımlarında kullanılabilir ve çalışır xfce ile iyi bir açık kaynak Uzak Masaüstü Protokolü (RDP) sunucusudur. Xrdp Ubuntu sanal makinenizde aşağıdaki gibi yükleyin:

```bash
sudo apt-get install xrdp
sudo systemctl enable xrdp
```

Oturumunuz başlattığınızda kullanmak için hangi masaüstü ortamı xrdp söyleyin. Xrdp xfce masaüstü ortamınızı kullanacak şekilde yapılandırın:

```bash
echo xfce4-session >~/.xsession
```

Değişikliklerin etkili şekilde xrdp hizmetini yeniden başlatın:

```bash
sudo service xrdp restart
```


## <a name="set-a-local-user-account-password"></a>Bir yerel kullanıcı hesabı parolasını ayarlayın
VM'nizi oluşturduğunuzda, kullanıcı hesabınız için bir parola oluşturduysanız bu adımı atlayın. Varsa yalnızca SSH anahtar kimlik doğrulamasını kullanmak ve ayarlamak, sanal makinenizde oturum açmak için xrdp kullanmadan önce bir parola belirtin. bir yerel hesap parolası yoktur. kimlik doğrulaması için SSH anahtarları xrdp kabul edemez. Aşağıdaki örnek, bir kullanıcı hesabının parolasını belirtir *azureuser*:

```bash
sudo passwd azureuser
```

> [!NOTE]
> Bir parola belirterek SSHD yapılandırmanızı şu anda mevcut değilse parolayla girişleri izin verecek şekilde güncelleştirmez. Güvenlik açısından bakıldığında, sanal makinenizde bir SSH tüneli anahtar tabanlı kimlik doğrulaması kullanarak bağlanmak istediğiniz ve xrdp için bağlanın. Bu durumda, Uzak Masaüstü trafiğine izin veren bir ağ güvenlik grubu kuralı oluşturma ile ilgili şu adımı atlayın.


## <a name="create-a-network-security-group-rule-for-remote-desktop-traffic"></a>Uzak Masaüstü trafiği için ağ güvenlik grubu kuralı oluşturma
Linux VM, ağ güvenliği ulaşmak Uzak Masaüstü trafiğine izin vermek için Grup Kuralı gereksinimlerini, oluşturulacak sanal makinenize ulaşmasına 3389 numaralı bağlantı noktasındaki TCP sağlar. Ağ güvenlik grubu kuralları hakkında daha fazla bilgi için bkz: [bir ağ güvenlik grubu nedir?](../../virtual-network/security-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Ayrıca [bir ağ güvenlik grubu kuralı oluşturmak için Azure portal'ı kullanmanızı](../windows/nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Aşağıdaki örnek bir ağ güvenlik grubu kuralı oluşturur [az vm open-port](/cli/azure/vm#az-vm-open-port) noktasında *3389*. Azure CLI üzerinden aşağıdaki ağ güvenlik grubu kuralı olmayan sanal makinenize yönelik SSH oturumunu açın:

```azurecli
az vm open-port --resource-group myResourceGroup --name myVM --port 3389
```


## <a name="connect-your-linux-vm-with-a-remote-desktop-client"></a>İle bir uzak masaüstü istemcisini Linux VM'nize bağlanın
Yerel, Uzak Masaüstü İstemcisi'ni açın ve IP adresini veya DNS adını Linux VM'nize bağlanın. Kullanıcı hesabı için kullanıcı adı ve parola sanal makinenizde aşağıdaki gibi girin:

![İçin uzak masaüstü istemcisini kullanarak xrdp bağlanma](./media/use-remote-desktop/remote-desktop-client.png)

Kimlik doğrulandıktan sonra xfce masaüstü ortamını yükleyin ve aşağıdaki örneğe benzer:

![xrdp aracılığıyla xfce Masaüstü ortamı](./media/use-remote-desktop/xfce-desktop-environment.png)

RDP istemcinizi ağ düzeyi kimlik doğrulamayı (NLA) kullanıyorsa, bu bağlantı ayarı devre dışı bırakmanız gerekebilir. XRDP NLA şu anda desteklemiyor. NLA, gibi destekleyen alternatif RDP çözümleri de bakabilirsiniz [FreeRDP](https://www.freerdp.com).


## <a name="troubleshoot"></a>Sorun giderme
Uzak masaüstü istemcisini kullanarak Linux VM'nize bağlanamıyorsanız, kullanın `netstat` Linux vm'nize sanal makinenize RDP bağlantıları için şu şekilde dinlediğini doğrulamak için:

```bash
sudo netstat -plnt | grep rdp
```

Aşağıdaki örnek, beklendiği gibi 3389 numaralı TCP bağlantı noktası üzerinde dinleme VM gösterir:

```bash
tcp     0     0      127.0.0.1:3350     0.0.0.0:*     LISTEN     53192/xrdp-sesman
tcp     0     0      0.0.0.0:3389       0.0.0.0:*     LISTEN     53188/xrdp
```

Varsa *xrdp sesman* hizmet dinleme değil, bir Ubuntu sanal makinesinde hizmeti şu şekilde yeniden başlatın:

```bash
sudo service xrdp restart
```

Gözden geçirme'de oturum açması */var/log* neden seçeceğine göstergeleri için Ubuntu VM'NİZİN üzerindeki hizmet yanıt vermiyor olabilir. Syslog hataları görüntülemek için bir Uzak Masaüstü bağlantı denemesi sırasında da izleyebilirsiniz:

```bash
tail -f /var/log/syslog
```

Red Hat Enterprise Linux ve SUSE gibi diğer Linux dağıtımlarına, hizmetleri ve gözden geçirmek için alternatif bir günlük dosyası konumları yeniden başlatmak için farklı şekilde olabilir.

Uzak Masaüstü istemcisinde herhangi bir yanıt almazsınız ve sistem günlüğüne meydana gelen olayları görmüyorsanız, Uzak Masaüstü trafiğine VM ulaşamıyor Bu davranış gösterir. 3389 numaralı bağlantı noktası üzerinde TCP'ye izin vermek için bir kuralı olduğundan emin olun, ağ güvenlik grubu kurallarını gözden geçirin. Daha fazla bilgi için [uygulama bağlantı sorunlarını giderme](../windows/troubleshoot-app-connection.md).


## <a name="next-steps"></a>Sonraki adımlar
Oluşturma ve Linux Vm'leri ile SSH anahtarları kullanma hakkında daha fazla bilgi için bkz. [SSH anahtarları oluşturma azure'da Linux VM'ler için](mac-create-ssh-keys.md).

Windows SSH kullanma hakkında daha fazla bilgi için bkz: [Windows ile SSH kullanma anahtarları](ssh-from-windows.md).

