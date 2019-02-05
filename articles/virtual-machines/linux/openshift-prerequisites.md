---
title: OpenShift Azure önkoşullarına içinde | Microsoft Docs
description: Azure'da OpenShift dağıtmak için Önkoşullar.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldwongms
manager: joraio
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: ''
ms.author: haroldw
ms.openlocfilehash: 25ec82c923ebe322194d868159332ef145727999
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2019
ms.locfileid: "55693015"
---
# <a name="common-prerequisites-for-deploying-openshift-in-azure"></a>Azure'da OpenShift dağıtmak için genel Önkoşullar

Bu makalede, OpenShift kapsayıcı platformu veya OKD azure'da dağıtmak için ortak önkoşulları açıklanır.

OpenShift yüklenmesini Ansible playbook'ları kullanır. Ansible, yükleme adımlarını tamamlamak için tüm küme konaklarına bağlanmak için güvenli Kabuk (SSH) kullanır.

Ansible, uzak ana SSH bağlantı başlattığında, bir parola girin olamaz. Bu nedenle, özel anahtarı ile ilişkili bir parola (parola) sahip olamaz veya dağıtım başarısız olur.

Azure Resource Manager şablonları sanal makineler (VM) dağıtmak için aynı ortak anahtara erişim tüm VM'ler için kullanılır. Tüm playbook'ları da yürüten VM'ye karşılık gelen özel anahtar ekleme gerekir. Güvenli bir şekilde bunun için VM'de oturum özel anahtarı geçirmek için bir Azure anahtar kasası kullanın.

Kapsayıcılar için kalıcı depolama için bir gereksinimi varsa, kalıcı birimleri gerekli değildir. OpenShift için bu özelliği Azure sanal sabit diskleri (VHD) destekler, ancak Azure ilk bulut sağlayıcısı olarak yapılandırılması gerekir.

Bu modelde, OpenShift:

- Bir VHD nesnesi, bir Azure depolama hesabı veya yönetilen disk oluşturur.
- Bir VM VHD bağlaması ve birimi biçimlendirir.
- Pod birime bağlar.

Bu yapılandırmanın çalışması için OpenShift Azure'da bu görevleri gerçekleştirmek için izinler gerekiyor. Bunu bir hizmet sorumlusu ile elde. Hizmet sorumlusu, kaynaklara izinlerine sahip Azure Active Directory'de bir güvenlik hesabıdır.

Hizmet sorumlusu depolama hesapları ve kümeyi oluşturan Vm'lere erişimi olmalıdır. Tüm OpenShift küme kaynaklarını tek bir kaynak grubuna dağıtıyorsanız, hizmet sorumlusu, kaynak grubu için izinler verilebilir.

Bu kılavuz Önkoşullar ile ilişkili yapıtları oluşturmayı açıklar.

> [!div class="checklist"]
> * OpenShift küme için SSH anahtarları yönetmek için bir anahtar kasası oluşturun.
> * Azure bulut sağlayıcısı tarafından kullanılmak üzere hizmet sorumlusu oluşturun.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma 
Azure aboneliğinizde oturum açın [az login](/cli/azure/reference-index) komut ve izleyin ekrandaki yönergeleri izleyin veya tıklatın **deneyin** Cloud Shell kullanmak için.

```azurecli 
az login
```
## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group) komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Anahtar Kasası'nı barındıracak bir adanmış kaynak grubu kullanmak için önerilir. Bu grubun içine OpenShift küme kaynakları dağıtmak bir kaynak grubundan ayrıdır.

Aşağıdaki örnekte adlı bir kaynak grubu oluşturur *keyvaultrg* içinde *eastus* konumu:

```azurecli 
az group create --name keyvaultrg --location eastus
```

## <a name="create-a-key-vault"></a>Bir anahtar kasası oluşturma
Kümeyle için SSH anahtarları depolamak için key vault oluşturma [az keyvault oluşturma](/cli/azure/keyvault) komutu. Anahtar kasası adı genel olarak benzersiz olmalıdır.

Aşağıdaki örnekte adlı bir anahtar kasası oluşturulmaktadır *keyvault* içinde *keyvaultrg* kaynak grubu:

```azurecli 
az keyvault create --resource-group keyvaultrg --name keyvault \
       --enabled-for-template-deployment true \
       --location eastus
```

## <a name="create-an-ssh-key"></a>SSH anahtarı oluşturma 
OpenShift kümeye erişim güvenliğini sağlamak için bir SSH anahtarı gereklidir. Kullanarak SSH anahtar çifti oluşturma `ssh-keygen` komutunu (Linux veya Mac OS x):
 
 ```bash
ssh-keygen -f ~/.ssh/openshift_rsa -t rsa -N ''
```

> [!NOTE]
> SSH anahtar çiftinizi parola olamaz / parola.

Windows üzerinde SSH anahtarları hakkında daha fazla bilgi için bkz. [oluşturma SSH anahtarları üzerinde Windows](/azure/virtual-machines/linux/ssh-from-windows). OpenSSH biçimdeki özel anahtarı dışarı aktardığınızdan emin olun.

## <a name="store-the-ssh-private-key-in-azure-key-vault"></a>SSH özel anahtarı Azure anahtar Kasası'nda Store
OpenShift dağıtım OpenShift asıl güvenli erişim için oluşturulan SSH anahtarı kullanır. Güvenli bir şekilde SSH anahtarı almak için dağıtımı etkinleştirmek için aşağıdaki komutu kullanarak anahtar Key Vault'ta Depolama:

```azurecli
az keyvault secret set --vault-name keyvault --name keysecret --file ~/.ssh/openshift_rsa
```

## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma 
OpenShift, bir kullanıcı adı ve parola veya hizmet sorumlusu kullanarak Azure ile iletişim kurar. Bir Azure hizmet sorumlusu, uygulamaları, hizmetleri ve OpenShift gibi Otomasyon araçları ile kullanabileceğiniz bir güvenlik kimliğidir. Denetleyebilir ve hangi işlemleri için Azure'da hizmet sorumlusu gerçekleştirebilir izinleri tanımlayın. Aboneliğin tümü yerine hizmet sorumlusu belirli kaynak gruplarına izinler kapsam en iyisidir.

Bir hizmet sorumlusu oluşturma [az ad sp create-for-rbac](/cli/azure/ad/sp) ve OpenShift gereken kimlik bilgilerini çıktı.

Aşağıdaki örnek, bir hizmet sorumlusu oluşturur ve openshiftrg adlı bir kaynak grubuna katkıda bulunan izinleri atar.

İlk olarak openshiftrg adlı kaynak grubunu oluşturun:

```azurecli
az group create -l eastus -n openshiftrg
```

Hizmet sorumlusu oluşturma:

```azurecli
scope=`az group show --name openshiftrg --query id`
az ad sp create-for-rbac --name openshiftsp \
      --role Contributor --password {Strong Password} \
      --scopes $scope
```
Windows kullanıyorsanız, yürütme ```az group show --name openshiftrg --query id``` ve çıkış $scope yerine kullanın.

Komuttan döndürülen AppID özelliği dikkat edin:
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

Hizmet sorumluları hakkında daha fazla bilgi için bkz. [Azure, Azure CLI ile hizmet sorumlusu oluşturma](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest).

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, aşağıdaki konular ele:
> [!div class="checklist"]
> * OpenShift küme için SSH anahtarları yönetmek için bir anahtar kasası oluşturun.
> * Azure bulut çözümü sağlayıcısı tarafından kullanılmak üzere hizmet sorumlusu oluşturun.

Ardından, OpenShift kümesi dağıtın:

- [OpenShift kapsayıcı platformu dağıtma](./openshift-container-platform.md)
- [OKD dağıtma](./openshift-okd.md)
