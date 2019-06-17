---
title: Azure VM'ye SSH bağlantısı sorunlarını giderme | Microsoft Docs
description: Linux çalıştıran bir Azure VM için 'SSH bağlantısı başarısız oldu' veya 'SSH bağlantısı reddedildi' gibi sorunları giderme
keywords: SSH bağlantısını reddetti, ssh hatası, azure, SSH bağlantısı başarısız ssh
services: virtual-machines-linux
documentationcenter: ''
author: genlin
manager: jeconnoc
editor: ''
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.assetid: dcb82e19-29b2-47bb-99f2-900d4cfb5bbb
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: troubleshooting
ms.date: 05/30/2017
ms.author: genli
ms.openlocfilehash: 81e00c4a3b9490a05667d58952f7bdf8945bacdb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61405280"
---
# <a name="troubleshoot-ssh-connections-to-an-azure-linux-vm-that-fails-errors-out-or-is-refused"></a>Hataları, başarısız veya reddedildi Azure Linux VM'ye SSH bağlantısı sorunlarını giderme
Bu makale, bulun ve Secure Shell (SSH) hataları, SSH bağlantı hataları nedeniyle oluşan sorunları düzeltmek olur veya SSH, Linux sanal makinesi (VM) bağlanmaya çalıştığınızda reddedilir. Azure portalı, Azure CLI veya Linux için VM erişimi uzantısı ve bağlantı sorunlarını gidermek için kullanabilirsiniz.

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Bu makalede herhangi bir noktada daha fazla yardıma ihtiyacınız olursa, üzerinde Azure uzmanlarıyla iletişime geçebilirsiniz [Azure MSDN ve Stack Overflow forumları](https://azure.microsoft.com/support/forums/). Alternatif olarak, bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) seçip **Destek**. Azure desteği hakkında daha fazla bilgi için okuma [Microsoft Azure desteği SSS](https://azure.microsoft.com/support/faq/).

## <a name="quick-troubleshooting-steps"></a>Hızlı sorun giderme adımları
Sorun giderme her adımdan sonra sanal Makineye yeniden bağlanmayı deneyin.

1. [SSH yapılandırmasını Sıfırla](#reset-config).
2. [Kimlik bilgilerini sıfırlama](#reset-credentials) kullanıcı.
3. Doğrulama [ağ güvenlik grubu](../../virtual-network/security-overview.md) kurallarının SSH trafiğine izin verir.
   * Emin bir [ağ güvenlik grubu kuralı](#security-rules) (varsayılan olarak, TCP bağlantı noktası 22) SSH trafiğine izin vermek için bulunmaktadır.
   * Bağlantı noktası yeniden yönlendirme kullanamazsınız / Azure load balancer'ı kullanmadan eşleme.
4. Denetleme [VM kaynak durumu](../../resource-health/resource-health-overview.md).
   * Sanal Makinenin iyi durumda olacak şekilde bildirdiğinden emin olmak.
   * Varsa [önyükleme tanılaması etkin](boot-diagnostics.md), VM'yi önyükleme günlüklerde değil raporlama doğrulayın.
5. [VM'yi yeniden başlatın](#restart-vm).
6. [VM'yi yeniden dağıtma](#redeploy-vm).

Daha ayrıntılı sorun giderme adımları ve açıklamaları için okuma devam edin.

## <a name="available-methods-to-troubleshoot-ssh-connection-issues"></a>SSH bağlantı sorunlarını gidermek için kullanılabilir yöntemleri
Kimlik bilgileri veya aşağıdaki yöntemlerden birini kullanarak SSH yapılandırması sıfırlayabilir:

* [Azure portalında](#use-the-azure-portal) - hızlı bir şekilde SSH yapılandırması veya SSH anahtarını sıfırlamak gereken ve Azure araçlarının yüklü olduğu yoksa harika.
* [Azure sanal makine seri Konsolu](https://aka.ms/serialconsolelinux) -VM seri konsol SSH yapılandırma bağımsız olarak çalışır ve etkileşimli konsol ile VM'nize sağlayacaktır. Aslında, "SSH olamaz" özellikle seri konsol neydi durumlardır çözmeye yardımcı olmak için tasarlanmıştır. Daha fazla ayrıntı aşağıdadır.
* [Azure CLI](#use-the-azure-cli) - komut satırında zaten varsa hızlı bir şekilde SSH yapılandırması veya kimlik bilgilerini sıfırlama. Klasik bir sanal makine ile çalışıyorsanız, kullanabileceğiniz [Azure Klasik CLI](#use-the-azure-classic-cli).
* [Azure VMAccessForLinux uzantısını](#use-the-vmaccess-extension) - oluşturma ve SSH yapılandırma veya kullanıcı kimlik bilgilerini sıfırlamak için json tanımı dosyaları yeniden kullanma.

Sorun giderme her adımdan sonra sanal makinenizde yeniden bağlanmayı deneyin. Yine de bağlanamıyorsanız, sonraki adıma deneyin.

## <a name="use-the-azure-portal"></a>Azure portalı kullanma
Azure portalı, yerel bilgisayarınızda herhangi bir aracı yüklemeden SSH yapılandırması veya kullanıcı kimlik bilgilerini sıfırlamak için hızlı bir yol sağlar.

Başlamak için Azure portalında VM'nizi seçin. Ekranı aşağı kaydırarak **destek + sorun giderme** seçin ve bölüm **parolayı Sıfırla** aşağıdaki örnekteki gibi:

![SSH yapılandırması veya Azure portalında kimlik bilgilerini sıfırlama](./media/troubleshoot-ssh-connection/reset-credentials-using-portal.png)

### <a name="a-idreset-config-reset-the-ssh-configuration"></a><a id="reset-config" />SSH yapılandırmasını Sıfırla
SSH yapılandırmasını sıfırlamak için seçin `Reset configuration only` içinde **modu** bölümünde önceki ekran görüntüsünde gösterildiği gibi ardından seçin **güncelleştirme**. Bu işlem tamamlandıktan sonra sanal Makinenizin yeniden erişmeyi deneyin.

### <a name="a-idreset-credentials-reset-ssh-credentials-for-a-user"></a><a id="reset-credentials" />Bir kullanıcı için SSH kimlik bilgilerini sıfırlama
Mevcut bir kullanıcının kimlik bilgilerini sıfırlamak için ya da seçin `Reset SSH public key` veya `Reset password` içinde **modu** bölümde yukarıdaki ekran görüntüsünde gösterildiği gibi. Kullanıcı adı ve bir SSH anahtarı ya da yeni bir parola belirtin ve ardından **güncelleştirme**.

Bu menüde VM'deki sudo ayrıcalıkları olan bir kullanıcı da oluşturabilirsiniz. Yeni kullanıcı adı ve ilişkili parola veya SSH anahtarını girin ve ardından **güncelleştirme**.

### <a name="a-idsecurity-rules-check-security-rules"></a><a id="security-rules" />Güvenlik kurallarını denetleyin

Kullanım [IP akışı doğrulama](../../network-watcher/network-watcher-check-ip-flow-verify-portal.md) için bir ağ güvenlik grubu kuralı veya bir sanal makineye gelen trafiği engelleyip engellemediğini doğrulayın. Ayrıca, gelen "izin ver" NSG emin olmak için etkin güvenlik grubu kurallarını gözden geçirebilirsiniz kuralının mevcut olduğundan ve SSH bağlantı noktası (varsayılan olarak 22) için önceliklendirildiğinden. Daha fazla bilgi için [VM sorunlarını gidermek için geçerli güvenlik kuralları kullanarak trafik akışı](../../virtual-network/diagnose-network-traffic-filter-problem.md).

### <a name="check-routing"></a>Yönlendirmeyi denetleyin

Ağ İzleyicisi'nin [sonraki atlama](../../network-watcher/network-watcher-check-next-hop-portal.md) yönlendiriliyor veya bir sanal makineden bir yol trafiği engelleyen değil olduğunu onaylamak için yeteneği. Ayrıca, bir ağ arabirimi için tüm geçerli rotalar görmek için geçerli rotalar gözden geçirebilirsiniz. Daha fazla bilgi için [VM sorunlarını gidermek için geçerli rotaları kullanma trafik akışı](../../virtual-network/diagnose-network-routing-problem.md).

## <a name="use-the-azure-vm-serial-console"></a>Azure sanal makine seri Konsolu
[Azure sanal makine seri Konsolu](./serial-console-linux.md) Linux sanal makineleri için metin tabanlı bir konsol erişim sağlar. Etkileşimli bir kabuk, SSH bağlantı sorunlarını gidermek için konsolu kullanabilirsiniz. Ulaştığından emin [önkoşulları](./serial-console-linux.md#prerequisites) seri konsol ve deneyin kullanmak için daha fazla bilgi için aşağıdaki komutları, SSH bağlantı sorunlarını giderme.

### <a name="check-that-ssh-is-running"></a>SSH çalışan olup olmadığını kontrol edin
VM'NİZDE SSH çalışıp çalışmadığını doğrulamak için aşağıdaki komutu kullanabilirsiniz:
```
$ ps -aux | grep ssh
```
Herhangi bir çıktı varsa, SSH çalışır duruma.

### <a name="check-which-port-ssh-is-running-on"></a>SSH üzerinde çalıştığı hangi bağlantı noktasını denetle
SSH üzerinde çalıştığı hangi bağlantı noktasını denetlemek için aşağıdaki komutu kullanabilirsiniz:
```
$ sudo grep Port /etc/ssh/sshd_config
```
Çıktınız aşağıdakine benzer görünecektir:
```
Port 22
```

## <a name="use-the-azure-cli"></a>Azure CLI kullanma
Henüz yapmadıysanız, en son yükleme [Azure CLI](/cli/azure/install-az-cli2) ve Azure oturum açma hesabı kullanarak [az login](/cli/azure/reference-index).

Oluşturduysanız ve özel Linux disk görüntüsü karşıya emin [Microsoft Azure Linux Aracısı](../extensions/agent-windows.md) 2.0.5 sürümü veya üzeri yüklü. Galeri görüntüleri kullanılarak oluşturulan VM'ler için bu erişim uzantısı zaten yüklü ve sizin için yapılandırılır.

### <a name="reset-ssh-configuration"></a>SSH yapılandırmasını Sıfırla
Başlangıçta için varsayılan değerlere SSH yapılandırması sıfırlanıyor ve VM'ye SSH sunucuda yeniden deneyin. Bu, kullanıcı hesabı adı, parola veya SSH anahtarlarını değiştirmez.
Aşağıdaki örnekte [az vm kullanıcı reset-ssh](/cli/azure/vm/user) adlı VM üzerinde SSH yapılandırmasını sıfırlamak için `myVM` içinde `myResourceGroup`. Kendi değerlerinizi şu şekilde kullanın:

```azurecli
az vm user reset-ssh --resource-group myResourceGroup --name myVM
```

### <a name="reset-ssh-credentials-for-a-user"></a>Bir kullanıcı için SSH kimlik bilgilerini sıfırlama
Aşağıdaki örnekte [az vm kullanıcı güncelleştirme](/cli/azure/vm/user) kimlik bilgilerini sıfırlamak için `myUsername` belirtilen değere `myPassword`, adlı VM'de `myVM` içinde `myResourceGroup`. Kendi değerlerinizi şu şekilde kullanın:

```azurecli
az vm user update --resource-group myResourceGroup --name myVM \
     --username myUsername --password myPassword
```

SSH anahtar kimlik doğrulamasını kullanırken, belirli bir kullanıcı için SSH anahtarı sıfırlayabilirsiniz. Aşağıdaki örnekte **az vm kümesi-linux-user erişim** depolanmış bir SSH anahtarı güncelleştirmek için `~/.ssh/id_rsa.pub` adlı kullanıcı için `myUsername`, adlı VM'de `myVM` içinde `myResourceGroup`. Kendi değerlerinizi şu şekilde kullanın:

```azurecli
az vm user update --resource-group myResourceGroup --name myVM \
    --username myUsername --ssh-key-value ~/.ssh/id_rsa.pub
```

## <a name="use-the-vmaccess-extension"></a>VMAccess uzantısı kullanma
Linux için VM erişimi uzantısı, gerçekleştirilecek eylemleri tanımlayan bir json dosyası olarak okur. Bu Eylemler, SSHD sıfırlama, SSH anahtarı sıfırlanıyor veya bir kullanıcı eklemeyi içerir. VMAccess uzantısı çağırmak için yine de Azure CLI'yi kullanırsınız, ancak isterseniz birden çok VM arasında json dosyalarını kullanabilirsiniz. Bu yaklaşım, ardından için belirli senaryolar çağrılabilir json dosya deposuna oluşturmanıza olanak sağlar.

### <a name="reset-sshd"></a>SSHD Sıfırla
Adlı bir dosya oluşturun `settings.json` aşağıdaki içeriğe sahip:

```json
{
    "reset_ssh":"True"
}
```

Azure CLI'yı kullanarak, daha sonra arama `VMAccessForLinux` uzantısı json dosyanızı belirterek SSHD bağlantınızı sıfırlayın. Aşağıdaki örnekte [az vm uzantısı kümesi](/cli/azure/vm/extension) adlı VM'de SSHD sıfırlamak için `myVM` içinde `myResourceGroup`. Kendi değerlerinizi şu şekilde kullanın:

```azurecli
az vm extension set --resource-group philmea --vm-name Ubuntu \
    --name VMAccessForLinux --publisher Microsoft.OSTCExtensions --version 1.2 --settings settings.json
```

### <a name="reset-ssh-credentials-for-a-user"></a>Bir kullanıcı için SSH kimlik bilgilerini sıfırlama
SSHD düzgün görünüyorsa tireyle kullanıcısının kimlik bilgileri sıfırlayabilirsiniz. Bir kullanıcının parolasını sıfırlamak için adlı bir dosya oluşturun. `settings.json`. Aşağıdaki örnek, kimlik bilgilerini sıfırlar `myUsername` belirtilen değere `myPassword`. İçine aşağıdaki satırları girin, `settings.json` dosya, kendi değerlerinizi kullanarak:

```json
{
    "username":"myUsername", "password":"myPassword"
}
```

Veya bir kullanıcı için SSH anahtarını sıfırlamak için önce adlı bir dosya oluşturun `settings.json`. Aşağıdaki örnek, kimlik bilgilerini sıfırlar `myUsername` belirtilen değere `myPassword`, adlı VM'de `myVM` içinde `myResourceGroup`. İçine aşağıdaki satırları girin, `settings.json` dosya, kendi değerlerinizi kullanarak:

```json
{
    "username":"myUsername", "ssh_key":"mySSHKey"
}
```

Json dosyanızı oluşturduktan sonra çağırmak için Azure CLI kullanmak `VMAccessForLinux` json dosyanızı belirterek, SSH kullanıcısı kimlik bilgilerini sıfırlamak için uzantı. Aşağıdaki örnekte adlı sanal makine kimlik bilgileri sıfırlar `myVM` içinde `myResourceGroup`. Kendi değerlerinizi şu şekilde kullanın:

```azurecli
az vm extension set --resource-group philmea --vm-name Ubuntu \
    --name VMAccessForLinux --publisher Microsoft.OSTCExtensions --version 1.2 --settings settings.json
```

## <a name="use-the-azure-classic-cli"></a>Klasik Azure CLI kullanma
Henüz kaydolmadıysanız [Klasik Azure CLI yükleme ve Azure aboneliğinize bağlayın](../../cli-install-nodejs.md). Resource Manager moduna gibi kullandığınızdan emin olun:

```azurecli
azure config mode arm
```

Oluşturduysanız ve özel Linux disk görüntüsü karşıya emin [Microsoft Azure Linux Aracısı](../extensions/agent-windows.md) 2.0.5 sürümü veya üzeri yüklü. Galeri görüntüleri kullanılarak oluşturulan VM'ler için bu erişim uzantısı zaten yüklü ve sizin için yapılandırılır.

### <a name="reset-ssh-configuration"></a>SSH yapılandırmasını Sıfırla
SSHD yapılandırması yanlış veya hizmet bir hatayla karşılaştı. SSH yapılandırması, geçerli olduğundan emin olmak için SSHD sıfırlayabilirsiniz. SSHD sıfırlama göz önüne almanız gereken ilk sorun giderme adım olmalıdır.

Aşağıdaki örnekte adlı bir VM üzerinde SSHD sıfırlar `myVM` adlı kaynak grubunda `myResourceGroup`. Kendi sanal makine ve kaynak grubu adları şu şekilde kullanın:

```azurecli
azure vm reset-access --resource-group myResourceGroup --name myVM \
    --reset-ssh
```

### <a name="reset-ssh-credentials-for-a-user"></a>Bir kullanıcı için SSH kimlik bilgilerini sıfırlama
SSHD düzgün görünüyorsa tireyle kullanıcının parolasını sıfırlayabilirsiniz. Aşağıdaki örnek, kimlik bilgilerini sıfırlar `myUsername` belirtilen değere `myPassword`, adlı VM'de `myVM` içinde `myResourceGroup`. Kendi değerlerinizi şu şekilde kullanın:

```azurecli
azure vm reset-access --resource-group myResourceGroup --name myVM \
     --user-name myUsername --password myPassword
```

SSH anahtar kimlik doğrulamasını kullanırken, belirli bir kullanıcı için SSH anahtarı sıfırlayabilirsiniz. Aşağıdaki örnek, depolanmış bir SSH anahtarı güncelleştirmeleri `~/.ssh/id_rsa.pub` adlı kullanıcı için `myUsername`, adlı VM'de `myVM` içinde `myResourceGroup`. Kendi değerlerinizi şu şekilde kullanın:

```azurecli
azure vm reset-access --resource-group myResourceGroup --name myVM \
    --user-name myUsername --ssh-key-file ~/.ssh/id_rsa.pub
```

## <a name="a-idrestart-vm-restart-a-vm"></a><a id="restart-vm" />Bir VM'yi yeniden başlatma
SSH yapılandırması ve kullanıcı kimlik bilgilerini sıfırlamak veya Bunun yapılması bir hatayla karşılaştı, temel alınan bilgi işlem sorunları adresine VM'nin yeniden başlatılmasının deneyebilirsiniz.

### <a name="azure-portal"></a>Azure portal
Azure portalını kullanarak bir VM'yi yeniden başlatmak için VM'nizi seçin ve ardından **yeniden** aşağıdaki örnekteki gibi:

![Azure portalında bir VM'yi yeniden başlatma](./media/troubleshoot-ssh-connection/restart-vm-using-portal.png)

### <a name="azure-cli"></a>Azure CLI
Aşağıdaki örnekte [az vm restart](/cli/azure/vm) adlı VM'yi yeniden başlatmak için `myVM` adlı kaynak grubunda `myResourceGroup`. Kendi değerlerinizi şu şekilde kullanın:

```azurecli
az vm restart --resource-group myResourceGroup --name myVM
```

### <a name="azure-classic-cli"></a>Azure klasik CLI
Aşağıdaki örnekte adlı VM yeniden `myVM` adlı kaynak grubunda `myResourceGroup`. Kendi değerlerinizi şu şekilde kullanın:

```azurecli
azure vm restart --resource-group myResourceGroup --name myVM
```

## <a name="a-idredeploy-vm-redeploy-a-vm"></a><a id="redeploy-vm" />Bir VM'yi yeniden dağıtma
Temel alınan herhangi bir ağ sorunu giderebilir Azure içinde başka bir düğüme VM yeniden dağıtabilirsiniz. Bir sanal makine yeniden dağıtıldığında hakkında daha fazla bilgi için bkz: [sanal makineyi yeni Azure düğümüne yeniden dağıtma](../windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

> [!NOTE]
> Bu işlem tamamlandıktan sonra kısa ömürlü disk verileri kaybedilirse ve sanal makine ile ilişkili olan dinamik IP adresleri güncelleştirilir.
>
>

### <a name="azure-portal"></a>Azure portal
Azure portalını kullanarak bir VM yeniden dağıtmak için VM'nizi seçin ve ekranı aşağı kaydırarak **destek + sorun giderme** bölümü. Seçin **yeniden** aşağıdaki örnekteki gibi:

![Azure portalında bir VM'yi yeniden dağıtma](./media/troubleshoot-ssh-connection/redeploy-vm-using-portal.png)

### <a name="azure-cli"></a>Azure CLI
Aşağıdaki örnek kullanım [az vm redeploy](/cli/azure/vm) adlı VM yeniden dağıtmak için `myVM` adlı kaynak grubunda `myResourceGroup`. Kendi değerlerinizi şu şekilde kullanın:

```azurecli
az vm redeploy --resource-group myResourceGroup --name myVM
```

### <a name="azure-classic-cli"></a>Azure klasik CLI
Aşağıdaki örnekte adlı VM yeniden dağıtır `myVM` adlı kaynak grubunda `myResourceGroup`. Kendi değerlerinizi şu şekilde kullanın:

```azurecli
azure vm redeploy --resource-group myResourceGroup --name myVM
```

## <a name="vms-created-by-using-the-classic-deployment-model"></a>Klasik dağıtım modeli kullanılarak oluşturulan VM'ler
Klasik dağıtım modeli kullanılarak oluşturulan sanal makineler için en sık karşılaşılan SSH bağlantı hataları gidermek için aşağıdaki adımları deneyin. Her adımdan sonra sanal Makineye yeniden bağlanmayı deneyin.

* Uzaktan erişimi sıfırlama [Azure portalında](https://portal.azure.com). Azure portalında VM'nizi seçin ve ardından **uzaktan Sıfırla...** .
* VM’yi yeniden başlatın. Üzerinde [Azure portalında](https://portal.azure.com), VM'nizi seçin ve seçin **yeniden**.

* VM için yeni bir Azure düğümüne yeniden dağıtın. Bir VM'yi yeniden dağıtma hakkında daha fazla bilgi için bkz: [sanal makineyi yeni Azure düğümüne yeniden dağıtma](../windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

    Bu işlem tamamlandıktan sonra kısa ömürlü diskin veri kaybı olmayacağını ve sanal makineyle ilişkili olan dinamik IP adresleri güncelleştirildi.
* Bölümündeki yönergeleri [Linux tabanlı sanal makineler için bir parola veya SSH sıfırlama](../linux/classic/reset-access-classic.md) için:

  * Parolayı veya SSH anahtarını Sıfırla.
  * Oluşturma bir *sudo* kullanıcı hesabı.
  * SSH yapılandırmasını sıfırlayın.
* Herhangi bir platform sorunu sanal makinenin kaynak durumunu denetleyin.<br>
     VM'nizi seçin ve aşağı kaydırarak **ayarları** > **sistem durumu denetleme**.

## <a name="additional-resources"></a>Ek kaynaklar
* Sanal makinenizde sonraki adımları izledikten sonra SSH için hala bulamıyorsanız, bkz. [ayrıntılı sorun giderme adımlarını](detailed-troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) sorununuzu çözmek için ek adımları gözden geçirmek için.
* Uygulama erişimi sorunlarını giderme hakkında daha fazla bilgi için bkz. [bir Azure sanal makinesinde çalışan bir uygulamaya erişim sorunlarını giderme](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* Sanal makineleri, Klasik dağıtım modeli kullanılarak oluşturulmuş sorun giderme hakkında daha fazla bilgi için bkz. [Linux tabanlı sanal makineler için bir parola veya SSH sıfırlama](../linux/classic/reset-access-classic.md).
