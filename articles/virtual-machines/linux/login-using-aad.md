---
title: Oturum açtığınızda Azure Active Directory kimlik bilgilerine sahip bir Linux VM | Microsoft Docs
description: Bu nasıl yapılır içinde oluşturmak ve bir Linux VM kullanıcı oturumları için Azure Active Directory kimlik doğrulaması kullanacak şekilde yapılandırma hakkında bilgi edinin
services: virtual-machines-linux
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/16/2018
ms.author: iainfou
ms.openlocfilehash: 96cc7aeb5fd1c64dc3793a801a4a5b759e7558b9
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34652881"
---
# <a name="log-in-to-a-linux-virtual-machine-in-azure-using-azure-active-directory-authentication-preview"></a>Azure Active Directory kimlik doğrulaması (Önizleme) kullanarak Azure Linux sanal makinede oturum açın

Azure'daki Linux sanal makineleri (VM'ler) güvenliğini artırmak için Azure Active Directory (AD) kimlik doğrulamasıyla tümleştirilebilir. Linux VM'ler için Azure AD kimlik doğrulaması kullandığınızda, merkezi olarak denetlemek ve VM'ler için erişime izin verdiğini veya ilkeleri zorunlu tutmanıza. Bu makalede oluşturma ve Azure AD kimlik doğrulaması kullanmak için bir Linux VM yapılandırma gösterilmektedir.

> [!NOTE]
> Bu özellik Önizleme aşamasındadır ve üretim sanal makine ya da iş yükleri ile kullanım için önerilmez. Sınama sonra atmak için beklediğiniz bir test sanal makinede bu özelliği kullanın.

Linux VM'ler için Azure'da, oturum açmak için Azure AD kimlik doğrulaması kullanarak birçok avantaj vardır dahil olmak üzere:

- **Gelişmiş güvenlik:**
  - Azure Linux VM'ler için oturum açmak için Kurumsal AD kimlik bilgilerinizi kullanın. Yerel yönetici hesapları oluşturmak ve kimlik bilgisi ömrünü yönetmek için gerek yoktur.
  - Yerel yönetici hesapları, bağımlılık azaltarak kimlik kaybı/hırsızlığına hakkında zayıf kimlik bilgilerini vb. yapılandırma kullanıcılar endişelenmeniz gerekmez.
  - Parola karmaşıklığını ve parola Ömrü ilkeleri, Azure AD dizini için yapılandırılmış güvenli Linux VM'ler de yardımcı olur.
  - Azure sanal makineler için daha güvenli oturum açma için çok faktörlü kimlik doğrulaması yapılandırabilirsiniz.
  - Azure Active Directory ile Linux VM'ler için oturum açma özelliği kullanan müşteriler için de çalışır [Federasyon Hizmetleri](../../active-directory/connect/active-directory-aadconnectfed-whatis.md).

- **Sorunsuz işbirliği:** With Role-Based erişim denetimi (RBAC) belirtebilirsiniz kimin yönetici ayrıcalıklarıyla veya normal bir kullanıcı olarak belirli bir VM'de oturum açabilirsiniz. Kullanıcıların katılma veya ekibiniz bırakın, uygun şekilde erişim vermek sanal makine için RBAC İlkesi güncelleştirebilirsiniz. Bu deneyim gereksiz SSH ortak anahtarları kaldırmak için sanal makineleri Temizle gerek kalmadan daha çok daha kolaydır. Çalışanlar, kuruluşunuzun bırakın ve kullanıcı hesapları devre dışı ya da Azure AD'den kaldırılan artık kaynaklarınıza erişimi.

### <a name="supported-azure-regions-and-linux-distributions"></a>Desteklenen Azure bölgeleri ve Linux dağıtımları

Aşağıdaki Linux dağıtımları şu anda bu özellik Önizleme sırasında desteklenir:

| Dağıtım | Sürüm |
| --- | --- |
| CentOS | CentOS 6.9 ve CentOS 7.4 |
| RedHat Enterprise Linux | RHEL 7 | 
| Ubuntu Server | Ubuntu 14.04 LTS, Ubuntu Server 16.04 ve Ubuntu Server 17.10 |

Aşağıdaki Azure bölgeleri şu anda bu özellik Önizleme sırasında desteklenir:

- Tüm genel Azure bölgeleri

>[!IMPORTANT]
> Bu önizleme özelliğini kullanmak için yalnızca desteklenen bir Linux distro dağıtmak ve desteklenen bir Azure bölgesi. Özelliği Azure kamu veya sovereign bulut desteklenmez.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Yüklemek ve CLI yerel olarak kullanmak seçerseniz, Bu öğretici, Azure CLI Sürüm 2.0.31 çalıştırmasını gerektirir veya sonraki bir sürümü. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli).

## <a name="create-a-linux-virtual-machine"></a>Linux sanal makinesi oluşturma

Sahip bir kaynak grubu oluşturma [az grubu oluşturma](/cli/azure/group#az-group-create), bir VM oluşturma [az vm oluşturma](/cli/azure/vm#az-vm-create) desteklenen distro kullanarak ve desteklenen bir bölgede. Aşağıdaki örnek adlı bir VM dağıtır *myVM* kullanan *Ubuntu 16.04 LTS* bir kaynak grubuna adlı *myResourceGroup* içinde *southcentralus*  bölge. Aşağıdaki örneklerde, kendi kaynak grubu ve gerektiği gibi VM adları sağlayabilir.

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

## <a name="install-the-azure-ad-login-vm-extension"></a>Azure AD oturum açma VM uzantısı yükleyin

Bir Linux VM Azure AD kimlik bilgileriyle oturum açmak için Azure Active Directory günlük VM uzantısı'nda yükleyin. VM uzantıları Azure sanal makinelerde dağıtım sonrası yapılandırma ve Otomasyon görevlerini sağlayan küçük uygulamalardır. Kullanım [az vm uzantısı kümesi](/cli/azure/vm/extension#az-vm-extension-set) yüklemek için *AADLoginForLinux* adlı VM uzantısı *myVM* içinde *myResourceGroup* kaynak Grup:

```azurecli-interactive
az vm extension set \
    --publisher Microsoft.Azure.ActiveDirectory.LinuxSSH \
    --name AADLoginForLinux \
    --resource-group myResourceGroup \
    --vm-name myVM
```

*ProvisioningState* , *başarılı* VM uzantısı yüklendikten sonra gösterilir.

## <a name="configure-role-assignments-for-the-vm"></a>VM rol atamalarını yapılandırın

Azure rol tabanlı erişim denetimi (RBAC) ilkesi kimin VM'ye oturum belirler. İki RBAC rolü VM oturum açma yetkilendirmek için kullanılır:

- **Sanal makine yönetici oturum açma**: Bu role atanmış olan kullanıcılar oturum açtığınızda Windows yönetici ya da Linux kök kullanıcı ayrıcalıkları olan bir Azure sanal makinesi.
- **Sanal makine kullanıcı oturum açma**: Bu role atanmış olan kullanıcılar oturum açtığınızda normal kullanıcı ayrıcalıkları olan bir Azure sanal makinesi.

> [!NOTE]
> VM SSH üzerinde oturum açmak bir kullanıcının izin vermek ya da atamanız gerekir *sanal makine yönetici oturum açma* veya *sanal makine kullanıcı oturum açma* rol. Bir Azure kullanıcıyla *sahibi* veya *katkıda bulunan* bir VM için atanan rollerin otomatik olarak ayrıcalıklarına sahip değil VM SSH üzerinde oturum açmak için.

Aşağıdaki örnek kullanır [az rol ataması oluşturma](/cli/azure/role/assignment#az-role-assignment-create) atamak için *sanal makine yönetici oturum açma* geçerli Azure kullanıcınız için VM rolü. Etkin Azure hesabınızda kullanıcı adı ile elde edilen [az hesabı Göster](/cli/azure/account#az-account-show)ve *kapsam* ile bir önceki adımda oluşturulan VM ayarlamak [az vm Göster](/cli/azure/vm#az-vm-show). Kapsam, bir kaynak grubu veya abonelik düzeyinde atanabilir ve normal RBAC devralma izinlerine uygular. Daha fazla bilgi için bkz: [rol tabanlı erişim denetimleri](../../azure-resource-manager/resource-group-overview.md#access-control)

```azurecli-interactive
username=$(az account show --query user.name --output tsv)
vm=$(az vm show --resource-group myResourceGroup --name myVM --query id -o tsv)

az role assignment create \
    --role "Virtual Machine Administrator Login" \
    --assignee $username \
    --scope $vm
```

> [!NOTE]
> AAD etki alanı ve oturum açma kullanıcı adı etki alanı eşleşmiyorsa, kullanıcı hesabınızla nesne kimliği belirtmelisiniz *--atanan nesne kimliği*, yalnızca kullanıcı adı için *--atanan*. Kullanıcı hesabınız için nesne kimliği elde edebilirsiniz [az ad kullanıcı listesi](/cli/azure/ad/user#az-ad-user-list).

RBAC Azure aboneliği kaynaklarınıza erişimi yönetmek için nasıl kullanılacağı hakkında daha fazla bilgi için bkz [Azure CLI 2.0](../../role-based-access-control/role-assignments-cli.md), [Azure portal](../../role-based-access-control/role-assignments-portal.md), veya [Azure PowerShell](../../role-based-access-control/role-assignments-powershell.md).

Azure AD Linux sanal makinede oturum açmak belirli bir kullanıcı için çok faktörlü kimlik doğrulama gerektirecek şekilde de yapılandırabilirsiniz. Daha fazla bilgi için bkz: [bulutta Azure multi-Factor Authentication kullanmaya başlama](../../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).

## <a name="log-in-to-the-linux-virtual-machine"></a>Linux sanal makinede oturum açın

İlk olarak, VM ile genel IP adresi görüntülemek [az vm Göster](/cli/azure/vm#az-vm-show):

```azurecli-interactive
az vm show --resource-group myResourceGroup --name myVM -d --query publicIps -o tsv
```

Azure AD kimlik bilgilerinizi kullanarak Azure Linux sanal makine oturum açın. `-l` Parametresi kendi Azure AD hesabı adresini belirtmenize olanak sağlar. Önceki komut çıktısında olarak VM genel IP adresi belirtin:

```azurecli-interactive
ssh -l azureuser@contoso.onmicrosoft.com publicIps
```

Bir kerelik kullanım kodu ile Azure ad oturum açmanız istenir [ https://microsoft.com/devicelogin ](https://microsoft.com/devicelogin). Kopyalayın ve aşağıdaki örnekte gösterildiği gibi tek seferlik kullanım kodu cihaz oturum açma sayfasına yapıştırın:

```bash
~$ ssh -l azureuser@contoso.onmicrosoft.com 13.65.237.247
To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code FJS3K6X4D to authenticate. Press ENTER when ready.
```

İstendiğinde, oturum açma sayfasında Azure AD oturum açma kimlik bilgilerinizi girin. Aşağıdaki iletiyi başarıyla doğrulandı web tarayıcısında gösterilir:

    You have signed in to the Microsoft Azure Linux Virtual Machine Sign-In application on your device.

Kapat tarayıcı penceresini SSH komut istemi dönün ve basın **Enter** anahtarı. Artık rol izinleri ile Azure Linux sanal makine gibi atanmış olarak oturum *VM kullanıcı* veya *VM yönetici*. Kullanıcı hesabınızın atanmışsa *sanal makine yönetici oturum açma* kullanabileceğiniz rol `sudo` kök ayrıcalıkları gerektiren komutları çalıştırmak için.

## <a name="troubleshoot-sign-in-issues"></a>Oturum açma sorunlarını giderme

SSH için Azure AD kimlik bilgileriyle çalıştığınızda bazı yaygın hatalar atanan hiçbir RBAC rolü eklemek ve yinelenen oturum ister. Bu sorunları düzeltmek için aşağıdaki bölümleri kullanın.

### <a name="access-denied-rbac-role-not-assigned"></a>Erişim reddedildi: RBAC rolü atanmamış

SSH satırına aşağıdaki hatayla karşılaşırsanız, bilgisayarınızda yüklü olduğunu doğrulamak [RBAC ilkelerinin yapılandırılması](#configure-rbac-policy-for-the-virtual-machine) kullanıcı ya da veren VM *sanal makine yönetici oturum açma* veya *sanal Kullanıcı oturum açma makine* rol:

```bash
login as: azureuser@contoso.onmicrosoft.com
Using keyboard-interactive authentication.
To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code FJX327AXD to authenticate. Press ENTER when ready.
Using keyboard-interactive authentication.
Access denied:  to sign-in you be assigned a role with action 'Microsoft.Compute/virtualMachines/login/action', for example 'Virtual Machine User Login'
Access denied
```

### <a name="continued-ssh-sign-in-prompts"></a>Devam eden SSH oturum açma komut istemleri

Bir web tarayıcısında kimlik doğrulama adımı başarılı şekilde gerçekleştirirseniz, hemen yeni bir kod yeniden oturum açmanız istenebilir. Bu hata genellikle SSH isteminde belirtilen oturum açma adı ve Azure ad ile oturum hesabı arasında bir uyuşmazlık kaynaklanır. Bu sorunu gidermek için:

- SSH isteminde belirtilen oturum açma adının doğru olduğundan emin olun. Oturum açma adı bir yazım hatasından SSH isteminde belirtilen oturum açma adı ve Azure ad ile oturum hesabı arasında uyuşmazlığa neden olabilir. Örneğin, yazdığınız *azuresuer@contoso.onmicrosoft.com* yerine *azureuser@contoso.onmicrosoft.com*.
- Birden çok kullanıcı hesabı varsa, farklı bir kullanıcı hesabı tarayıcı penceresinde Azure AD ile oturum açarken sağlamıyorsa emin olun.
- Linux büyük küçük harfe duyarlı bir işletim sistemidir. Arasında bir fark 'Azureuser@contoso.onmicrosoft.com've'azureuser@contoso.onmicrosoft.com', hangi bir uyuşmazlığı neden olabilir. SSH isteminde doğru büyük küçük harf duyarlılığı ile UPN belirttiğinizden emin olun.

## <a name="preview-feedback"></a>Önizleme geri bildirim

Bu önizleme özelliği veya rapor üzerinde kullanarak sorunları hakkında geri bildirim paylaşmak [Azure AD görüş Forumu](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=166032)

## <a name="next-steps"></a>Sonraki adımlar

Azure Active Directory hakkında daha fazla bilgi için bkz: [Azure Active Directory nedir](../../active-directory/active-directory-whatis.md) ve [nasıl Azure Active Directory ile çalışmaya başlama](../../active-directory/get-started-azure-ad.md)
