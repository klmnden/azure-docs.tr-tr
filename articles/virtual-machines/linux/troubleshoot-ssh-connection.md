---
title: "Bir Azure VM için SSH bağlantı sorunlarını giderme | Microsoft Docs"
description: "Linux çalıştıran bir Azure VM için 'SSH bağlantısı başarısız oldu' veya 'SSH bağlantı reddedildi' gibi sorunları gidermek nasıl."
keywords: "SSH bağlantı reddedildi, ssh hatası, azure ssh, SSH bağlantısı başarısız oldu"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.assetid: dcb82e19-29b2-47bb-99f2-900d4cfb5bbb
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: iainfou
ms.openlocfilehash: b7fe6dadb444ebbe6af6239562f507e451f9f605
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="troubleshoot-ssh-connections-to-an-azure-linux-vm-that-fails-errors-out-or-is-refused"></a>Bir Azure Linux VM, hatalar, başarısız olur veya reddedilir SSH bağlantı sorunlarını giderme
Güvenli Kabuk (SSH), SSH bağlantı hataları hatalarla veya bir Linux sanal makine (VM) bağlanmaya çalıştığınızda SSH reddetti çeşitli nedenleri vardır. Bu makalede bulmanıza ve sorunları düzeltin yardımcı olur. Azure portalı, Azure CLI ya da Linux VM erişim uzantısını ve bağlantı sorunlarını gidermek için kullanabilirsiniz.

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Bu makalede herhangi bir noktada daha fazla yardıma gereksinim duyarsanız, üzerinde Azure uzmanlar başvurabilirsiniz [MSDN Azure ve yığın taşması forumları](http://azure.microsoft.com/support/forums/). Alternatif olarak, Azure destek olay dosya. Git [Azure Destek sitesi](http://azure.microsoft.com/support/options/) seçip **alma desteği**. Azure desteği hakkında daha fazla bilgi için okuma [Microsoft Azure desteği ile ilgili SSS](http://azure.microsoft.com/support/faq/).

## <a name="quick-troubleshooting-steps"></a>Hızlı sorun giderme adımları
Sorun giderme her adımdan sonra VM yeniden bağlanmayı deneyin.

1. SSH yapılandırmasını sıfırlayın.
2. Kullanıcının kimlik bilgilerini sıfırlayın.
3. Doğrulama [ağ güvenlik grubu](../../virtual-network/virtual-networks-nsg.md) kuralları SSH trafiğine izin verir.
   * Bir ağ güvenlik grubu kural (varsayılan olarak, TCP bağlantı noktası 22) SSH trafiğe izin verecek şekilde bulunduğundan emin olun.
   * Bağlantı noktası yeniden yönlendirmesine kullanamazsınız / Azure yük dengeleyici kullanmadan eşleme.
4. Denetleme [VM kaynak durumu](../../resource-health/resource-health-overview.md). 
   * VM'yi sağlıklı olacak şekilde bildirdiğinden emin olmak.
   * Önyükleme tanılaması etkin varsa, VM önyükleme hataları günlüklerine bildirmeyen doğrulayın.
5. VM’yi yeniden başlatın.
6. VM yeniden dağıtın.

Daha ayrıntılı sorun giderme adımları ve açıklamalar için okuma devam edin.

## <a name="available-methods-to-troubleshoot-ssh-connection-issues"></a>SSH bağlantı sorunlarını gidermek için kullanılabilir yöntemleri
Kimlik bilgileri veya aşağıdaki yöntemlerden birini kullanarak SSH yapılandırmasını sıfırlayabilirsiniz:

* [Azure portal](#use-the-azure-portal) - SSH yapılandırmasını veya SSH anahtarı hızlı bir şekilde sıfırlamanız gerekir ve Azure araçlarının yüklü olduğu yoksa harika.
* [Azure CLI 2.0](#use-the-azure-cli-20) - komut satırında zaten varsa hızla SSH yapılandırması veya kimlik bilgilerini sıfırlayın. Aynı zamanda [Azure CLI 1.0](#use-the-azure-cli-10)
* [Azure VMAccessForLinux uzantısını](#use-the-vmaccess-extension) - oluşturmak ve SSH yapılandırma ya da kullanıcı kimlik bilgilerini sıfırlamak için json tanım dosyalarını yeniden kullanabilirsiniz.

Sorun giderme her adımdan sonra VM yeniden bağlanmayı deneyin. Hala bağlanamıyorsanız, sonraki adıma deneyin.

## <a name="use-the-azure-portal"></a>Azure portalı kullanma
Azure portalı, yerel bilgisayarınızda herhangi bir aracı yüklemeden SSH yapılandırması veya kullanıcı kimlik bilgilerini sıfırlamak için hızlı bir yoludur.

Azure portalında, VM'yi seçin. Ekranı aşağı kaydırarak **destek + sorun giderme** bölümünde ve seçin **parola sıfırlama** aşağıdaki örnekteki gibi:

![SSH yapılandırması veya Azure Portalı'ndaki kimlik bilgilerini sıfırlama](./media/troubleshoot-ssh-connection/reset-credentials-using-portal.png)

### <a name="reset-the-ssh-configuration"></a>SSH yapılandırmasını sıfırlayın
İlk adım olarak, seçin `Reset configuration only` gelen **modu** açılır menü önceki ekran görüntüsü gibi ardından **sıfırlama** düğmesi. VM'yi yeniden erişmek bu işlem tamamlandıktan sonra deneyin.

### <a name="reset-ssh-credentials-for-a-user"></a>Bir kullanıcının SSH kimlik bilgilerini sıfırlama
Mevcut bir kullanıcının kimlik bilgilerini sıfırlamak için ya da seçin `Reset SSH public key` veya `Reset password` gelen **modu** önceki ekran olduğu gibi açılır menü. Kullanıcı adı ve bir SSH anahtarı veya yeni bir parola belirtin ve ardından **sıfırlama** düğmesi.

Bu menüden VM sudo ayrıcalıklarına sahip bir kullanıcı da oluşturabilirsiniz. Yeni kullanıcı adı ve ilişkili parolayı veya SSH anahtarını girin ve ardından **sıfırlama** düğmesi.

## <a name="use-the-azure-cli-20"></a>Azure CLI 2.0 kullanın
Henüz yapmadıysanız, en son yükleme [Azure CLI 2.0](/cli/azure/install-az-cli2) ve bir Azure hesabı kullanarak oturum açma [az oturum açma](/cli/azure/#login).

Oluşturulan ve özel Linux disk görüntü karşıya emin olun [Microsoft Azure Linux Aracısı](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) sürüm 2.0.5 veya üstü yüklü. Galeri görüntüleri kullanılarak oluşturulan VM'ler için bu erişim uzantısı zaten yüklenmiş ve sizin için yapılandırılmış.

### <a name="reset-ssh-configuration"></a>SSH yapılandırmasını sıfırlayın
Başlangıçta yapabilecekleriniz SSH yapılandırmasını varsayılan değerlere sıfırlanıyor ve VM SSH sunucusunda yeniden deneyin. Bu kullanıcı hesabı adı, parola veya SSH anahtarları değiştirmez olduğunu unutmayın.
Aşağıdaki örnek kullanır [az vm kullanıcı sıfırlama-ssh](/cli/azure/vm/user#reset-ssh) adlı VM üzerinde SSH yapılandırmasını sıfırlamak için `myVM` içinde `myResourceGroup`. Kendi değerlerinizi aşağıdaki şekilde kullanın:

```azurecli
az vm user reset-ssh --resource-group myResourceGroup --name myVM
```

### <a name="reset-ssh-credentials-for-a-user"></a>Bir kullanıcının SSH kimlik bilgilerini sıfırlama
Aşağıdaki örnek kullanır [az vm kullanıcı güncelleştirme](/cli/azure/vm/user#update) kimlik bilgilerini sıfırlamak için `myUsername` belirtilen değere `myPassword`, adlı VM üzerinde `myVM` içinde `myResourceGroup`. Kendi değerlerinizi aşağıdaki şekilde kullanın:

```azurecli
az vm user update --resource-group myResourceGroup --name myVM \
     --username myUsername --password myPassword
```

SSH anahtar kimlik doğrulaması kullanıyorsanız, belirli bir kullanıcı için SSH anahtarı sıfırlayabilirsiniz. Aşağıdaki örnek kullanır **az vm erişim kümesi linux kullanıcı** depolanan SSH anahtarı güncelleştirmek için `~/.ssh/id_rsa.pub` adlı kullanıcı için `myUsername`, adlı VM üzerinde `myVM` içinde `myResourceGroup`. Kendi değerlerinizi aşağıdaki şekilde kullanın:

```azurecli
az vm user update --resource-group myResourceGroup --name myVM \
    --username myUsername --ssh-key-value ~/.ssh/id_rsa.pub
```

## <a name="use-the-vmaccess-extension"></a>VMAccess uzantısını kullanın
Linux VM erişim uzantısını gerçekleştirilecek eylemleri tanımlayan bir json dosyası olarak okur. Bu eylemler SSHD sıfırlama, bir SSH anahtarı sıfırlanıyor veya bir kullanıcı ekleme içerir. Hala VMAccess uzantısını çağırmak için Azure CLI kullanın, ancak isterseniz arasında birden çok VM json dosyaları kullanabilirsiniz. Bu yaklaşım, bir depo sonra için senaryoları çağrılabilir json dosyalarınızın oluşturmanıza olanak sağlar.

### <a name="reset-sshd"></a>SSHD Sıfırla
Adlı bir dosya oluşturun `settings.json` aşağıdaki içeriğe sahip:

```json
{  
    "reset_ssh":"True"
}
```

Azure CLI kullanarak, ardından çağırırsınız `VMAccessForLinux` json dosyanız belirterek SSHD bağlantınızı sıfırlamaya uzantısı. Aşağıdaki örnek kullanır [az vm uzantısı kümesi](/cli/azure/vm/extension#set) SSHD adlı VM üzerinde sıfırlamak için `myVM` içinde `myResourceGroup`. Kendi değerlerinizi aşağıdaki şekilde kullanın:

```azurecli
az vm extension set --resource-group philmea --vm-name Ubuntu \
    --name VMAccessForLinux --publisher Microsoft.OSTCExtensions --version 1.2 --settings settings.json
```

### <a name="reset-ssh-credentials-for-a-user"></a>Bir kullanıcının SSH kimlik bilgilerini sıfırlama
Doğru çalışması için SSHD görünür tireyle kullanıcısının kimlik bilgileri sıfırlayabilirsiniz. Bir kullanıcının parolasını sıfırlamak için adlı bir dosya oluşturun `settings.json`. Aşağıdaki örnek kimlik bilgilerini sıfırlar `myUsername` belirtilen değere `myPassword`. Aşağıdaki satırlarına girin, `settings.json` dosya, kendi değerlerini kullanarak:

```json
{
    "username":"myUsername", "password":"myPassword"
}
```

Veya SSH anahtarı bir kullanıcı için sıfırlamak için önce adlı bir dosya oluşturun `settings.json`. Aşağıdaki örnek kimlik bilgilerini sıfırlar `myUsername` belirtilen değere `myPassword`, adlı VM üzerinde `myVM` içinde `myResourceGroup`. Aşağıdaki satırlarına girin, `settings.json` dosya, kendi değerlerini kullanarak:

```json
{
    "username":"myUsername", "ssh_key":"mySSHKey"
}
```

Json dosyanızı oluşturduktan sonra çağırmak için Azure CLI kullanın `VMAccessForLinux` uzantısı json dosyanız belirterek SSH kullanıcı kimlik bilgilerinizi sıfırlayın. Aşağıdaki örnek adlı VM üzerinde kimlik bilgilerini sıfırlar `myVM` içinde `myResourceGroup`. Kendi değerlerinizi aşağıdaki şekilde kullanın:

```azurecli
az vm extension set --resource-group philmea --vm-name Ubuntu \
    --name VMAccessForLinux --publisher Microsoft.OSTCExtensions --version 1.2 --settings settings.json
```

## <a name="use-the-azure-cli-10"></a>Azure CLI 1.0 kullanın
Henüz yapmadıysanız [Azure CLI 1.0 yükleyin ve Azure aboneliğinize bağlanmak](../../cli-install-nodejs.md). Resource Manager moduna gibi kullandığınızdan emin olun:

```azurecli
azure config mode arm
```

Oluşturulan ve özel Linux disk görüntü karşıya emin olun [Microsoft Azure Linux Aracısı](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) sürüm 2.0.5 veya üstü yüklü. Galeri görüntüleri kullanılarak oluşturulan VM'ler için bu erişim uzantısı zaten yüklenmiş ve sizin için yapılandırılmış.

### <a name="reset-ssh-configuration"></a>SSH yapılandırmasını sıfırlayın
SSHD yapılandırması yanlış veya hizmet bir hatayla karşılaştı. SSH yapılandırması geçerli olduğundan emin olmak için SSHD sıfırlayabilirsiniz. SSHD sıfırlama aldığınız ilk sorun giderme adımı olmalıdır.

Aşağıdaki örnek SSHD adlı bir VM'de sıfırlar `myVM` kaynak grubunda adlı `myResourceGroup`. Kendi VM ve kaynak grubu adları şu şekilde kullanın:

```azurecli
azure vm reset-access --resource-group myResourceGroup --name myVM \
    --reset-ssh
```

### <a name="reset-ssh-credentials-for-a-user"></a>Bir kullanıcının SSH kimlik bilgilerini sıfırlama
SSHD düzgün çalışması için görünürse, bir tireyle kullanıcı için parolayı sıfırlayabilirsiniz. Aşağıdaki örnek kimlik bilgilerini sıfırlar `myUsername` belirtilen değere `myPassword`, adlı VM üzerinde `myVM` içinde `myResourceGroup`. Kendi değerlerinizi aşağıdaki şekilde kullanın:

```azurecli
azure vm reset-access --resource-group myResourceGroup --name myVM \
     --user-name myUsername --password myPassword
```

SSH anahtar kimlik doğrulaması kullanıyorsanız, belirli bir kullanıcı için SSH anahtarı sıfırlayabilirsiniz. Aşağıdaki örnek depolanan SSH anahtarını güncelleştirir `~/.ssh/id_rsa.pub` adlı kullanıcı için `myUsername`, adlı VM üzerinde `myVM` içinde `myResourceGroup`. Kendi değerlerinizi aşağıdaki şekilde kullanın:

```azurecli
azure vm reset-access --resource-group myResourceGroup --name myVM \
    --user-name myUsername --ssh-key-file ~/.ssh/id_rsa.pub
```


## <a name="restart-a-vm"></a>Bir VM’yi yeniden başlatma
SSH yapılandırması ve kullanıcı kimlik bilgilerini sıfırlamak veya Bunun yapılması bir hatayla karşılaştı, işlem sorunları temel adres için VM yeniden başlatmayı deneyebilirsiniz.

### <a name="azure-portal"></a>Azure portalına
Azure portalını kullanarak bir VM'yi yeniden başlatmak için VM seçin ve **yeniden** aşağıdaki örnekteki gibi düğmesi:

![Azure portalında bir VM yeniden başlatma](./media/troubleshoot-ssh-connection/restart-vm-using-portal.png)

### <a name="azure-cli-10"></a>Azure CLI 1.0
Aşağıdaki örnek adlı VM yeniden `myVM` kaynak grubunda adlı `myResourceGroup`. Kendi değerlerinizi aşağıdaki şekilde kullanın:

```azurecli
azure vm restart --resource-group myResourceGroup --name myVM
```

### <a name="azure-cli-20"></a>Azure CLI 2.0
Aşağıdaki örnek kullanır [az vm yeniden başlatma](/cli/azure/vm#restart) adlı VM'yi yeniden başlatmak için `myVM` kaynak grubunda adlı `myResourceGroup`. Kendi değerlerinizi aşağıdaki şekilde kullanın:

```azurecli
az vm restart --resource-group myResourceGroup --name myVM
```


## <a name="redeploy-a-vm"></a>Bir VM’yi yeniden dağıtma
Temel ağ sorunları düzeltebilir Azure içinde başka bir düğüme VM yeniden dağıtabilirsiniz. Bir VM dağıtarak hakkında daha fazla bilgi için bkz: [dağıtmanız sanal makineyi yeni Azure düğümüne](../windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

> [!NOTE]
> Bu işlem tamamlandıktan sonra kısa ömürlü disk veriler kaybolur ve sanal makineyle ilişkili olan dinamik IP adreslerini güncelleştirilir.
> 
> 

### <a name="azure-portal"></a>Azure portalına
Azure Portalı'nı kullanarak bir VM yeniden dağıtmak için VM seçin ve ekranı aşağı kaydırarak **destek + sorun giderme** bölümü. Tıklatın **dağıtmanız** aşağıdaki örnekteki gibi düğmesi:

![Azure portalında bir VM'yi yeniden dağıtın](./media/troubleshoot-ssh-connection/redeploy-vm-using-portal.png)

### <a name="azure-cli-10"></a>Azure CLI 1.0
Aşağıdaki örnek adlı VM yeniden dağıtır `myVM` kaynak grubunda adlı `myResourceGroup`. Kendi değerlerinizi aşağıdaki şekilde kullanın:

```azurecli
azure vm redeploy --resource-group myResourceGroup --name myVM
```

### <a name="azure-cli-20"></a>Azure CLI 2.0
Aşağıdaki örnek kullanım [az vm dağıtın](/cli/azure/vm#redeploy) adlı VM yeniden dağıtmak için `myVM` kaynak grubunda adlı `myResourceGroup`. Kendi değerlerinizi aşağıdaki şekilde kullanın:

```azurecli
az vm redeploy --resource-group myResourceGroup --name myVM
```

## <a name="vms-created-by-using-the-classic-deployment-model"></a>Klasik dağıtım modeli kullanılarak oluşturulan sanal makineleri
Klasik dağıtım modeli kullanılarak oluşturulmuş olan VM'ler için en sık karşılaşılan SSH bağlantı hataları gidermek için aşağıdaki adımları deneyin. Her adımdan sonra VM yeniden bağlanmayı deneyin.

* Uzaktan Erişim'sıfırlama [Azure portal](https://portal.azure.com). Azure Portal'daki VM seçin ve tıklatın **uzaktan Sıfırla...**  düğmesi.
* VM’yi yeniden başlatın. Üzerinde [Azure portal](https://portal.azure.com), VM'yi seçin ve tıklatın **yeniden** düğmesi.
    
* Yeni bir Azure düğümlü VM yeniden dağıtın. Bir VM yeniden dağıtma hakkında daha fazla bilgi için bkz: [dağıtmanız sanal makineyi yeni Azure düğümüne](../windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
  
    Bu işlem tamamlandıktan sonra kısa ömürlü disk veriler kaybolur ve sanal makineyle ilişkili olan dinamik IP adreslerini güncelleştirilir.
* ' Ndaki yönergeleri izleyin [Linux tabanlı sanal makineler için bir parola veya SSH sıfırlamaya](classic/reset-access-classic.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) için:
  
  * Parola veya SSH anahtarını sıfırlayın.
  * Oluşturma bir *sudo* kullanıcı hesabı.
  * SSH yapılandırmasını sıfırlayın.
* Platform sorunları için VM'nin kaynak sistem durumunu denetleyin.<br>
     VM seçin ve aşağı kaydırarak **ayarları** > **durum denetimi**.

## <a name="additional-resources"></a>Ek kaynaklar
* VM'nize sonra adımları izledikten sonra SSH için hala erişemiyorsanız bkz [sorun giderme adımlarını ayrıntılı](detailed-troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) sorunu gidermek için ek adımlar gözden geçirmek için.
* Uygulama erişimi sorunlarını giderme hakkında daha fazla bilgi için bkz: [bir Azure sanal makine üzerinde çalışan bir uygulamaya erişim sorunlarını giderme](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* Klasik dağıtım modeli kullanılarak oluşturulan sanal makineler sorunlarını giderme hakkında daha fazla bilgi için bkz: [Linux tabanlı sanal makineler için bir parola veya SSH sıfırlamaya](classic/reset-access-classic.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

