---
title: Bir Linux VM Azure Active Directory kimlik bilgileriyle oturum açın | Microsoft Docs
description: Bu nasıl yapılır kullanıcı oturumları için Azure Active Directory kimlik doğrulamasını kullanmak için bir Linux VM oluşturma ve yapılandırma konusunda bilgi edinin
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/17/2018
ms.author: cynthn
ms.openlocfilehash: d1db228f4c73cc00cd32ca6ae5b86056db68f05b
ms.sourcegitcommit: c884e2b3746d4d5f0c5c1090e51d2056456a1317
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60148960"
---
# <a name="log-in-to-a-linux-virtual-machine-in-azure-using-azure-active-directory-authentication-preview"></a>Azure Active Directory kimlik doğrulama (Önizleme) kullanarak Azure'da bir Linux sanal makinede oturum açın

Azure'da Linux sanal makineleri (VM'ler) güvenliğini artırmak için Azure Active Directory (AD) kimlik doğrulamasıyla tümleştirilebilir. Linux VM'ler için Azure AD kimlik doğrulaması kullandığınızda, merkezi olarak denetleyebilir ve verdiği veya erişimini engellediği Vm'lere ilkeleri zorunlu tutmanıza. Bu makalede, Azure AD kimlik doğrulamasını kullanmak için bir Linux VM oluşturma ve yapılandırma işlemini göstermektedir.

> [!NOTE]
> Bu özellik Önizleme aşamasındadır ve üretim sanal makineleri veya iş yükleri ile kullanım için önerilmez. Test edildikten sonra atmak beklediğiniz bir test sanal makinesi üzerinde bu özelliği kullanın.

Linux sanal makineleri, azure'da oturum açmak için Azure AD kimlik doğrulamasını kullanmanın birçok avantajı vardır dahil olmak üzere:

- **Gelişmiş güvenlik için:**
  - Azure Linux Vm'leri için oturum açmak için Kurumsal AD kimlik bilgilerinizi kullanabilirsiniz. Yerel yönetici hesapları oluşturma ve kimlik bilgisi ömrünü yönetmek için gerek yoktur.
  - Yerel yönetici hesapları, güvenme azaltarak, kimlik bilgisi kaybı/hırsızlığı hakkında zayıf kimlik bilgileri vb. yapılandırma kullanıcılar endişelenmeniz gerekmez.
  - Parola karmaşıklığını ve parola ömrü ilkelerini Azure AD dizininiz için yapılandırılan güvenli Linux Vm'leri de yardımcı olur.
  - Azure sanal makineleri daha güvenli oturum açma için çok faktörlü kimlik doğrulamasını yapılandırabilirsiniz.
  - Linux Vm'leri Azure Active Directory ile oturum açmak için özelliği kullanan müşteriler için de kullanılabilir [Federasyon Hizmetleri](../../active-directory/hybrid/how-to-connect-fed-whatis.md).

- **Sorunsuz bir işbirliği:** Rol tabanlı erişim denetimi (RBAC) sahip olan belirli bir sanal Makinenin yönetici ayrıcalıklarıyla veya normal bir kullanıcı olarak oturum belirtebilirsiniz. Kullanıcılar katılın veya takımınızın bırakın, uygun erişim vermek sanal makine için RBAC İlkesi güncelleştirebilirsiniz. Bu deneyim Vm'leri gereksiz SSH ortak anahtarlarını kaldırmak için temizleme gerek kalmadan daha çok daha kolaydır. Çalışanlar, kuruluşunuzun bırakın ve kullanıcı hesabı devre dışı ya da Azure AD'deki kaldırıldı, artık kaynaklarınıza erişimi yok.

## <a name="supported-azure-regions-and-linux-distributions"></a>Desteklenen Azure bölgeleri ve Linux dağıtımları

Aşağıdaki Linux dağıtımı şu anda bu özellik Önizleme sırasında desteklenmektedir:

| Dağıtım | Version |
| --- | --- |
| CentOS | CentOS 6, CentOS 7 |
| Debian | Debian 9 |
| openSUSE | openSUSE Leap 42.3 |
| RedHat Enterprise Linux | RHEL 6, RHEL 7 | 
| SUSE Linux Enterprise Server | SLES 12 |
| Ubuntu Server | Ubuntu 14.04 LTS, Ubuntu Server 16.04 ve Ubuntu Server 18.04 |


Aşağıdaki Azure bölgeleri, şu anda bu özellik Önizleme sırasında desteklenmektedir:

- Tüm genel Azure bölgeleri

>[!IMPORTANT]
> Bu önizleme özelliğini kullanmak için yalnızca desteklenen bir Linux distro dağıtmak ve desteklenen bir Azure bölgesinde. Bu özellik, Azure kamu veya bağımsız bulutlarda desteklenmiyor.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz Bu öğretici, Azure CLI Sürüm 2.0.31 çalıştırdığınız gerektirir veya üzeri. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme]( /cli/azure/install-azure-cli).

## <a name="create-a-linux-virtual-machine"></a>Linux sanal makinesi oluşturma

Bir kaynak grubu oluşturun [az grubu oluşturma](/cli/azure/group#az-group-create), ardından ile VM oluşturma [az vm oluşturma](/cli/azure/vm#az-vm-create) desteklenen distro kullanarak ve desteklenen bir bölgede. Aşağıdaki örnekte adlı bir VM dağıtır *myVM* kullanan *Ubuntu 16.04 LTS* adlı bir kaynak grubu içinde *myResourceGroup* içinde *southcentralus*  bölge. Aşağıdaki örneklerde, kendi kaynak grubu ve gerektiğinde bir sanal makine adı sağlayabilirsiniz.

```azurecli-interactive
az group create --name myResourceGroup --location southcentralus

az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

VM’yi ve destekleyici kaynakları oluşturmak birkaç dakika sürer.

## <a name="install-the-azure-ad-login-vm-extension"></a>Azure AD oturum açma VM uzantısı yükleme

Bir Linux VM Azure AD kimlik bilgileriyle oturum açmak için Azure Active Directory oturum açma VM uzantısı'nı yükleyin. VM, Azure sanal makinelerinde dağıtım sonrası yapılandırma ve otomasyon görevleri sunan küçük uygulamalar uzantılarıdır. Kullanım [az vm uzantısı kümesi](/cli/azure/vm/extension#az-vm-extension-set) yüklemek için *AADLoginForLinux* adlı VM uzantısı *myVM* içinde *myResourceGroup* kaynak Grup:

```azurecli-interactive
az vm extension set \
    --publisher Microsoft.Azure.ActiveDirectory.LinuxSSH \
    --name AADLoginForLinux \
    --resource-group myResourceGroup \
    --vm-name myVM
```

*ProvisioningState* , *başarılı* sanal makinede uzantı yüklendiğinde gösterilir.

## <a name="configure-role-assignments-for-the-vm"></a>VM için rol atamalarını yapılandırma

Azure rol tabanlı erişim denetimi (RBAC) İlkesi kullanan sanal Makineye oturum açabilirsiniz belirler. İki RBAC rollerini VM oturum açma yetkisi vermek için kullanılır:

- **Sanal Makine Yöneticisi oturum açma**: Atanan bu role sahip kullanıcılar bir Azure sanal makinesinde Windows Yöneticisi veya Linux kök kullanıcı ayrıcalıkları ile oturum açabilir.
- **Sanal makine kullanıcı oturum açma**: Atanan bu role sahip kullanıcılar normal kullanıcı ayrıcalıkları olan bir Azure sanal makinesinde oturum açabilir.

> [!NOTE]
> VM'ye SSH üzerinden oturum açmasına izin vermek için ya da atamanız gerekir *Sanal Makine Yöneticisi oturum açma* veya *sanal makine kullanıcı oturum açma* rol. Bir Azure kullanıcısı ile *sahibi* veya *katkıda bulunan* bir VM için atanan rollerin otomatik olarak sanal Makineye SSH üzerinden oturum açmak için ayrıcalıkları yoktur.

Aşağıdaki örnekte [az rol ataması oluşturma](/cli/azure/role/assignment#az-role-assignment-create) atamak *Sanal Makine Yöneticisi oturum açma* geçerli Azure kullanıcınız için VM rolü. Azure hesabınızı etkin kullanıcı adı ile elde edilen [az hesabı Göster](/cli/azure/account#az-account-show)ve *kapsam* ile bir önceki adımda oluşturulan VM'ye ayarlanır [az vm show](/cli/azure/vm#az-vm-show). Kapsam, bir kaynak grubu veya abonelik düzeyinde atanabilir ve normal RBAC devralma izinler uygulayabilirsiniz. Daha fazla bilgi için [rol tabanlı erişim denetimleri](../../role-based-access-control/overview.md)

```azurecli-interactive
username=$(az account show --query user.name --output tsv)
vm=$(az vm show --resource-group myResourceGroup --name myVM --query id -o tsv)

az role assignment create \
    --role "Virtual Machine Administrator Login" \
    --assignee $username \
    --scope $vm
```

> [!NOTE]
> AAD etki alanı ve oturum açma kullanıcı adı etki alanı eşleşmiyorsa, kullanıcı hesabınızın nesne Kimliğini belirtmeniz gerekir *--atanan nesne kimliği*, kullanıcı adı için yalnızca *--atanan*. Kullanıcı hesabınız için nesne Kimliğini almak [az ad kullanıcı listesi](/cli/azure/ad/user#az-ad-user-list).

Azure abonelik kaynaklarınıza erişimi yönetmek için RBAC kullanma hakkında daha fazla bilgi için bkz [Azure CLI](../../role-based-access-control/role-assignments-cli.md), [Azure portalında](../../role-based-access-control/role-assignments-portal.md), veya [Azure PowerShell](../../role-based-access-control/role-assignments-powershell.md).

Azure AD kullanarak Linux sanal makineye oturum açmak belirli bir kullanıcı için multi-Factor authentication'ı gerektirecek şekilde de yapılandırabilirsiniz. Daha fazla bilgi için [bulutta Azure multi Factor Authentication kullanmaya başlama](../../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).

## <a name="log-in-to-the-linux-virtual-machine"></a>Linux sanal makinesinde oturum açın

İlk olarak, ile sanal makinenizin genel IP adresini görüntülemek [az vm show](/cli/azure/vm#az-vm-show):

```azurecli-interactive
az vm show --resource-group myResourceGroup --name myVM -d --query publicIps -o tsv
```

Azure AD kimlik bilgilerinizi kullanarak Azure Linux sanal makinesinde oturum açın. `-l` Parametresi kendi Azure AD hesabı adresini belirtmenize olanak sağlar. Tüm küçük harflerle hesabı adresleri girilmelidir. Önceki komutta alınan sanal makinenizin genel IP adresini kullanın:

```azurecli-interactive
ssh -l azureuser@contoso.onmicrosoft.com publicIps
```

Bir kerelik kullanım kodu ile Azure ad oturum açmanız istenir [ https://microsoft.com/devicelogin ](https://microsoft.com/devicelogin). Kopyalama ve cihaz oturum açma sayfasına, aşağıdaki örnekte gösterildiği gibi tek seferlik kullanım kodu yapıştırın:

```bash
~$ ssh -l azureuser@contoso.onmicrosoft.com 13.65.237.247
To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code FJS3K6X4D to authenticate. Press ENTER when ready.
```

İstendiğinde, oturum açma sayfasında Azure AD oturum açma kimlik bilgilerinizi girin. Kimliğiniz başarıyla doğrulandı, web tarayıcısında aşağıdaki ileti gösterilir:

    You have signed in to the Microsoft Azure Linux Virtual Machine Sign-In application on your device.

Kapat tarayıcı penceresinin SSH istemine geri dönmek ve basın **Enter** anahtarı. Artık Azure Linux sanal makine rolü izinleri ile gibi atanmış olarak oturum açmış *VM kullanıcı* veya *VM Yöneticisi*. Kullanıcı hesabınızın atanırsa *Sanal Makine Yöneticisi oturum açma* kullanabileceğiniz rol `sudo` kök ayrıcalıkları gerektiren komutlarını çalıştırmak.

## <a name="sudo-and-aad-login"></a>Sudo ve AAD oturum açma

Sudo, ilk kez ikinci kez kimlik doğrulaması istenir. Sudo çalıştırmak için yeniden kimlik doğrulaması olmasını istemiyorsanız sudoers dosyanızı düzenleyebileceğiniz `/etc/sudoers.d/aad_admins` ve bu satırı değiştirin:

```bash
%aad_admins ALL=(ALL) ALL
```
Bu satırla:

```bash
%aad_admins ALL=(ALL) NOPASSWD:ALL
```


## <a name="troubleshoot-sign-in-issues"></a>Oturum açma sorunlarını giderme

Bazı yaygın hatalar, SSH ile Azure AD kimlik bilgileriyle çalıştığınızda atanan yok RBAC rolü içeren ve yinelenen oturum ister. Bu sorunları düzeltmek için aşağıdaki bölümleri kullanın.

### <a name="access-denied-rbac-role-not-assigned"></a>Erişim reddedildi: RBAC rolü atanmadı

SSH isteminiz aşağıdaki hatayı görürseniz, kullanıcı ya da veren sanal makine için yapılandırılmış RBAC ilkelerini doğrulamak *Sanal Makine Yöneticisi oturum açma* veya *sanal makine kullanıcı oturum açma*rolü:

```bash
login as: azureuser@contoso.onmicrosoft.com
Using keyboard-interactive authentication.
To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code FJX327AXD to authenticate. Press ENTER when ready.
Using keyboard-interactive authentication.
Access denied:  to sign-in you be assigned a role with action 'Microsoft.Compute/virtualMachines/login/action', for example 'Virtual Machine User Login'
Access denied
```

### <a name="continued-ssh-sign-in-prompts"></a>Devam eden SSH oturum açma yönergeleri

Bir web tarayıcısında kimlik doğrulaması adımını başarıyla tamamlanırsa, hemen yeni bir kod ile yeniden oturum açmanız istenebilir. Bu hata genellikle SSH isteminde belirttiğiniz oturum açma adı ve Azure AD ile oturum açtığı hesabı arasında bir uyuşmazlık nedeniyle oluşur. Bu sorunu düzeltmek için:

- SSH isteminde belirttiğiniz oturum açma adının doğru olduğunu doğrulayın. Oturum açma adı bir yazım yanlışı SSH isteminde belirttiğiniz oturum açma adı ve Azure AD ile oturum açtığı hesabı arasında bir uyuşmazlık neden olabilir. Örneğin, yazdığınız *azuresuer\@contoso.onmicrosoft.com* yerine *azureuser\@contoso.onmicrosoft.com*.
- Birden çok kullanıcı hesabı varsa, farklı bir kullanıcı hesabı tarayıcı penceresinde Azure AD'de oturum açarken sağlamayan emin olun.
- Linux büyük küçük harfe duyarlı bir işletim sistemidir. Arasında bir fark 'Azureuser@contoso.onmicrosoft.com've'azureuser@contoso.onmicrosoft.com', bir uyuşmazlığı neden. SSH isteminde doğru büyük küçük harf duyarlılığı ile UPN belirttiğinizden emin olun.

## <a name="preview-feedback"></a>Önizleme geri bildirim

Bu önizleme özelliğini veya rapor üzerinde kullanarak sorunları hakkındaki görüşlerinizi paylaşın [Azure AD geri bildirim Forumu](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=166032)

## <a name="next-steps"></a>Sonraki adımlar

Azure Active Directory hakkında daha fazla bilgi için bkz. [Azure Active Directory nedir](../../active-directory/fundamentals/active-directory-whatis.md)
