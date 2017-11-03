---
title: "Azure önkoşulları OpenShift | Microsoft Docs"
description: "OpenShift azure'da dağıtmak için Önkoşullar."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldw
manager: najoshi
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 
ms.author: haroldw
ms.openlocfilehash: 0c90b8a6d17fa293b6708d942afd35e1333623cb
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2017
---
# <a name="common-prerequisites-for-openshift-in-azure"></a>Azure'da OpenShift ortak önkoşulları

Azure'da OpenShift dağıtırken OpenShift Menşe veya OpenShift kapsayıcı Platform dağıtmakta olduğunuz bağımsız olarak birkaç genel ön koşullar vardır.

OpenShift yüklemesini Ansible playbooks gerçekleştirilir. Ansible SSH yükleme adımlarını tamamlamak için kümesinin parçası olacak tüm ana bilgisayarları bağlamak için kullanır.
SSH bağlantısı için uzak ana bilgisayarların başlatıldığında bir parola girmesini yolu yoktur. Bu nedenle, kendisiyle ilişkili bir parola özel anahtara sahip olamaz veya dağıtımı başarısız olur.
Tüm sanal makineleri Resource Manager şablonları dağıtılan olduğundan, aynı ortak anahtara erişim tüm VM'ler için kullanılır. Tüm playbooks de yürütüyor VM karşılık gelen özel anahtar eklemesine gerekir.
Güvenli bir şekilde bunun için bir Azure anahtar kasası özel anahtar VM geçirmek için kullanırız.

Kapsayıcıları için kalıcı depolama gereksinimi varsa, ardından kalıcı birimler (PV) gereklidir. Bu PV'ler kalıcı depolama çeşit tarafından yedeklenmesi gerekir. Bu özellik için Azure diskleri (VHD) OpenShift destekler, ancak Azure ilk bulut sağlayıcısı olarak yapılandırılması gerekir. Bu modelde, OpenShift olur:

- Bir VHD nesnesi bir Azure depolama hesabı oluşturun
- Bir VM VHD'nin ve birimi biçimlendirme
- Pod birimine bağlama

Bunun çalışması için OpenShift Azure'da önceki görevleri gerçekleştirmek için izinler gerekir. Bunun için bir hizmet sorumlusu gereklidir. Hizmet asıl (SP) kaynakları izinleri Azure Active Directory'de bir güvenlik hesabı olabilir.
Hizmet sorumlusu depolama hesapları ve kümeyi olun VM'ler erişimi olmalıdır. SP tüm OpenShift küme kaynakları tek bir kaynak grubuna dağıtılırsa, bu kaynak grubuna izin verilebilir.

Bu kılavuz, önkoşulların ile ilişkili yapıları oluşturmayı açıklar.

> [!div class="checklist"]
> * OpenShift küme için SSH anahtarları yönetmek için bir KeyVault oluşturun.
> * Azure bulut sağlayıcısı tarafından kullanılmak üzere bir hizmet sorumlusu oluşturun.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="log-in-to-azure"></a>Azure'da oturum açma 
Oturum açtığınızda, Azure aboneliğinizle [az oturum açma](/cli/azure/#login) komut ve izleyin ekrandaki yönergeleri veya tıklatın **deneyin** bulut Kabuğu'nu kullanmak için.

```azurecli 
az login
```
## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group#create) komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Bir özel kaynak grubu - OpenShift küme kaynaklarını içine dağıtılan kaynak grubundan ayrı anahtar kasası barındırmak için kullanılır önerilir. 

Aşağıdaki örnek, bir kaynak grubu oluşturur *keyvaultrg* içinde *eastus* konumu.

```azurecli 
az group create --name keyvaultrg --location eastus
```

## <a name="create-a-key-vault"></a>Bir anahtar kasası oluşturma
Küme için SSH anahtarları depolamak için bir KeyVault oluşturma [az keyvault oluşturma](/cli/azure/keyvault#create) komutu. Anahtar kasası adı genel olarak benzersiz olmalıdır.

Aşağıdaki örnek adlı keyvault oluşturur *keyvault* içinde *keyvaultrg* kaynak grubu.

```azurecli 
az keyvault create --resource-group keyvaultrg --name keyvault \
       --enabled-for-template-deployment true \
       --location eastus
```

## <a name="create-an-ssh-key"></a>SSH anahtarı oluşturma 
Bir SSH anahtarı OpenShift kaynak kümesine erişimi güvenli hale getirmek için gereklidir. Kullanarak bir SSH anahtar çifti oluşturma `ssh-keygen` komutunu (Linux veya Mac).
 
 ```bash
ssh-keygen -f ~/.ssh/openshift_rsa -t rsa -N ''
```

> [!NOTE]
> Oluşturduğunuz SSH anahtar çiftiniz bir parola olmaması gerekir.

Windows, SSH anahtarları hakkında daha fazla bilgi için [SSH oluşturma Windows anahtarları](/azure/virtual-machines/linux/ssh-from-windows).

## <a name="store-ssh-private-key-in-key-vault"></a>Anahtar kasasına SSH özel anahtar depolama
OpenShift dağıtım OpenShift asıl güvenli erişim için oluşturulan SSH anahtarı kullanır. SSH anahtarı güvenli bir şekilde almasını dağıtımını etkinleştirmek için aşağıdaki komutu kullanarak anahtar kasasına anahtar depola:

```azurecli
az keyvault secret set --vault-name keyvault --name keysecret --file ~/.ssh/openshift.rsa
```

## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma 
OpenShift bir kullanıcı adı ve parola veya bir hizmet sorumlusu kullanarak Azure ile iletişim kurar. Bir Azure hizmet sorumlusu uygulamaları, hizmetleri ve OpenShift gibi Otomasyon araçları ile birlikte kullanabileceğiniz bir güvenlik kimliğidir. Denetim ve hizmet sorumlusu Azure'da gerçekleştirebilirsiniz ne gibi işlemler için izinler tanımlar. Yalnızca bir kullanıcı adı ve parola sağlayarak üzerinden güvenliğini artırmak için bu örnek temel bir hizmet sorumlusu oluşturur.

Bir hizmet sorumlusu ile oluşturma [az ad sp oluşturma-için-rbac](/cli/azure/ad/sp#create-for-rbac) ve OpenShift gereken kimlik bilgilerini çıktı.

Aşağıdaki örnek, bir hizmet sorumlusu oluşturur ve katkıda bulunan izinleri myResourceGroup adlı bir kaynak grubuna atar. Windows kullanıyorsanız, yürütme ```az group show --name myResourceGroup --query id``` ayrı ayrı ve çıkış akışı kapsam seçeneği.

```azurecli
az ad sp create-for-rbac --name openshiftsp \
          --role Contributor --password {Strong Password} \
          --scopes $(az group show --name myResourceGroup --query id)
```

Komuttan döndürülen AppID özelliği not edin.
```json
{
  "appId": "11111111-abcd-1234-efgh-111111111111",            
  "displayName": "openshiftsp",
  "name": "http://openshiftsp",
  "password": {Strong Password},
  "tenant": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"
}
```
 > [!WARNING] 
 > Güvenli olmayan bir parola oluşturmayın.  [Azure AD parola kuralları ve kısıtlamaları](/azure/active-directory/active-directory-passwords-policy) yönergelerini izleyin.

Hizmet sorumluları hakkında daha fazla bilgi için bkz: [bir Azure hizmet sorumlusu Azure CLI 2.0 ile oluşturun.](/cli/azure/create-an-azure-service-principal-azure-cli)

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede aşağıdaki konular ele:
> [!div class="checklist"]
> * OpenShift küme için SSH anahtarları yönetmek için bir KeyVault oluşturun.
> * Azure bulut sağlayıcısı tarafından kullanılmak üzere bir hizmet sorumlusu oluşturun.

Şimdi, bir OpenShift kümeyi dağıtma gidin

- [OpenShift kaynak dağıtma](./openshift-origin.md)
- [OpenShift kapsayıcı Platform dağıtma](./openshift-container-platform.md)

