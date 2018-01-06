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
ms.openlocfilehash: 5e287cd29fb305e78fe6338782838929007b17fc
ms.sourcegitcommit: 0e1c4b925c778de4924c4985504a1791b8330c71
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/06/2018
---
# <a name="common-prerequisites-for-deploying-openshift-in-azure"></a>OpenShift azure'da dağıtmak için ortak önkoşulları

Bu makalede OpenShift Menşe veya OpenShift kapsayıcı Platform azure'da dağıtmak için ortak önkoşulları açıklanmaktadır.

OpenShift yüklemesini Ansible playbooks kullanır. Ansible yükleme adımlarını tamamlamak için tüm küme konaklarına bağlanmak için güvenli Kabuk (SSH) kullanır.

Uzak ana bilgisayarlara SSH bağlantısını başlattığınızda, bir parola giremezsiniz. Bu nedenle, özel anahtarı kendisiyle ilişkili bir parolaya sahip olamaz veya dağıtımı başarısız olur.

Azure Resource Manager şablonları sanal makineleri (VM'ler) dağıtmak için aynı ortak anahtara erişim tüm VM'ler için kullanılır. Karşılık gelen özel anahtarı tüm playbooks de yürütür VM ekleme gerekir. Güvenli bir şekilde bunun için özel anahtarı VM geçirmek için bir Azure anahtar kasası kullanın.

Kapsayıcıları için kalıcı depolama gereksinimi varsa, kalıcı birimler gerekli değildir. Bu özellik için Azure sanal sabit diskleri (VHD) OpenShift destekler, ancak Azure ilk bulut sağlayıcısı olarak yapılandırılması gerekir. 

Bu modelde, OpenShift:

- Bir Azure depolama hesabında bir VHD oluşturur.
- Bir VM ve biçim birimi VHD'ye bağlar.
- Pod birime bağlar.

Bu yapılandırmanın çalışması için OpenShift Azure'da önceki görevleri gerçekleştirmek için izinler gerekir. Bu hizmet sorumlusu ile elde. Hizmet sorumlusu kaynakları izinleri Azure Active Directory'de güvenlik hesabıdır.

Hizmet sorumlusu depolama hesapları ve kümeyi olun VM'ler erişimi olmalıdır. Tüm OpenShift küme kaynakları tek bir kaynak grubuna dağıtıyorsanız, hizmet sorumlusu bu kaynak grubuna izin verilebilir.

Bu kılavuz Önkoşullar ile ilişkilendirilmiş yapıların oluşturmayı açıklar.

> [!div class="checklist"]
> * OpenShift küme için SSH anahtarları yönetmek için bir anahtar kasası oluşturun.
> * Azure bulut çözümü sağlayıcısı tarafından kullanılmak üzere bir hizmet sorumlusu oluşturun.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma 
Oturum açtığınızda, Azure aboneliğinizle [az oturum açma](/cli/azure/#login) komut ve izleyin ekrandaki yönergeleri veya tıklatın **deneyin** bulut Kabuğu'nu kullanmak için.

```azurecli 
az login
```
## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group#create) komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Anahtar kasası barındırmak için adanmış bir kaynak grubu kullanın. Bu grubun içine OpenShift küme kaynaklarını dağıtma kaynak grubundan ayrıdır. 

Aşağıdaki örnek, bir kaynak grubu oluşturur *keyvaultrg* içinde *eastus* konumu:

```azurecli 
az group create --name keyvaultrg --location eastus
```

## <a name="create-a-key-vault"></a>Bir anahtar kasası oluşturma
Küme için SSH anahtarları depolamak için bir anahtar kasası oluşturma [az keyvault oluşturma](/cli/azure/keyvault#create) komutu. Anahtar kasası adı genel olarak benzersiz olmalıdır.

Aşağıdaki örnek adlı bir anahtar kasası oluşturur *keyvault* içinde *keyvaultrg* kaynak grubu:

```azurecli 
az keyvault create --resource-group keyvaultrg --name keyvault \
       --enabled-for-template-deployment true \
       --location eastus
```

## <a name="create-an-ssh-key"></a>SSH anahtarı oluşturma 
Bir SSH anahtarı OpenShift kaynak kümesine erişimi güvenli hale getirmek için gereklidir. Kullanarak bir SSH anahtarı çifti oluşturma `ssh-keygen` komutunu (Linux veya macOS):
 
 ```bash
ssh-keygen -f ~/.ssh/openshift_rsa -t rsa -N ''
```

> [!NOTE]
> SSH anahtar çiftiniz bir parolaya sahip olamaz.

Windows'da SSH anahtarları hakkında daha fazla bilgi için bkz: [SSH oluşturma Windows anahtarları](/azure/virtual-machines/linux/ssh-from-windows).

## <a name="store-the-ssh-private-key-in-azure-key-vault"></a>Azure anahtar kasası SSH özel anahtar depolama
OpenShift dağıtım OpenShift asıl güvenli erişim için oluşturulan SSH anahtarı kullanır. SSH anahtarı güvenli bir şekilde almasını dağıtımını etkinleştirmek için aşağıdaki komutu kullanarak anahtarı anahtar kasası depola:

```azurecli
az keyvault secret set --vault-name keyvault --name keysecret --file ~/.ssh/openshift_rsa
```

## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma 
OpenShift, bir kullanıcı adı ve parola veya bir hizmet sorumlusu kullanarak Azure ile iletişim kurar. Bir Azure hizmet sorumlusu uygulamaları, hizmetleri ve OpenShift gibi Otomasyon araçları ile birlikte kullanabileceğiniz bir güvenlik kimliğidir. Denetim ve hangi işlemleri için hizmet sorumlusu Azure'da gerçekleştirebilirsiniz izinleri tanımlayın. Yalnızca bir kullanıcı adı ve parola sağlayarak ötesinde güvenliğini artırmak için bu örnek temel bir hizmet sorumlusu oluşturur.

Bir hizmet sorumlusu ile oluşturma [az ad sp oluşturma-için-rbac](/cli/azure/ad/sp#create-for-rbac) ve OpenShift gereken kimlik bilgilerini çıktı.

Aşağıdaki örnek, bir hizmet sorumlusu oluşturur ve katkıda bulunan izinleri myResourceGroup adlı bir kaynak grubuna atar. Windows kullanıyorsanız, yürütme ```az group show --name myResourceGroup --query id``` ayrı ayrı ve çıkış akışı kapsam seçeneği.

```azurecli
az ad sp create-for-rbac --name openshiftsp \
          --role Contributor --password {Strong Password} \
          --scopes $(az group show --name myResourceGroup --query id)
```

Komuttan döndürülen AppID özelliği not edin:
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
 > Güvenli bir parola oluşturduğunuzdan emin olun. [Azure AD parola kuralları ve kısıtlamaları](/azure/active-directory/active-directory-passwords-policy) yönergelerini izleyin.

Hizmet sorumluları hakkında daha fazla bilgi için bkz: [Azure, Azure CLI 2.0 ile hizmet sorumlusu oluşturmak](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest).

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede aşağıdaki konular ele:
> [!div class="checklist"]
> * OpenShift küme için SSH anahtarları yönetmek için bir anahtar kasası oluşturun.
> * Azure bulut çözümü sağlayıcısı tarafından kullanılmak üzere bir hizmet sorumlusu oluşturun.

Ardından, bir OpenShift kümeyi dağıtma:

- [OpenShift kaynak dağıtma](./openshift-origin.md)
- [OpenShift kapsayıcı Platform dağıtma](./openshift-container-platform.md)

