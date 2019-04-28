---
title: OpenShift Azure önkoşullarına içinde | Microsoft Docs
description: Azure'da OpenShift dağıtmak için Önkoşullar.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldwongms
manager: mdotson
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/19/2019
ms.author: haroldw
ms.openlocfilehash: d8a9b82e51c837af6343ddf851545d02299aa527
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61473894"
---
# <a name="common-prerequisites-for-deploying-openshift-in-azure"></a>Azure'da OpenShift dağıtmak için genel Önkoşullar

Bu makalede, OpenShift kapsayıcı platformu veya OKD azure'da dağıtmak için ortak önkoşulları açıklanır.

OpenShift yüklenmesini Ansible playbook'ları kullanır. Ansible, yükleme adımlarını tamamlamak için tüm küme konaklarına bağlanmak için güvenli Kabuk (SSH) kullanır.

Ansible, SSH bağlantısını uzak Konaklara yaptığında, bir parola girin olamaz. Bu nedenle, özel anahtarı ile ilişkili bir parola (parola) sahip olamaz veya dağıtım başarısız olur.

Azure Resource Manager şablonları sanal makineler (VM) dağıtmak için aynı ortak anahtara erişim tüm VM'ler için kullanılır. Karşılık gelen özel anahtar, tüm playbook'ları da yürüten VM olması gerekir. Bu eylem güvenli bir şekilde gerçekleştirmek için bir Azure anahtar kasası VM'ye özel anahtarı geçirmek için kullanılır.

Kapsayıcılar için kalıcı depolama için bir gereksinimi varsa, kalıcı birimleri gerekli değildir. OpenShift Azure sanal sabit diskleri (VHD'ler) için kalıcı birimleri destekler, ancak Azure ilk bulut sağlayıcısı olarak yapılandırılması gerekir.

Bu modelde, OpenShift:

- Bir VHD nesnesi, bir Azure depolama hesabı veya yönetilen disk oluşturur.
- Bir VM VHD bağlaması ve birimi biçimlendirir.
- Pod birime bağlar.

Bu yapılandırmanın çalışması için OpenShift Azure'da bu görevleri gerçekleştirmek için izinler gerekiyor. Bir hizmet sorumlusu, bu amaçla kullanılır. Hizmet sorumlusu, kaynaklara izinlerine sahip Azure Active Directory'de bir güvenlik hesabıdır.

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

[az group create](/cli/azure/group) komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Anahtar Kasası'nı barındıracak bir adanmış kaynak grubu kullanmanız gerekir. Bu grubun içine OpenShift küme kaynakları dağıtmak bir kaynak grubundan ayrıdır.

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

## <a name="prerequisites-applicable-only-to-resource-manager-template"></a>Önkoşullar yalnızca Resource Manager şablonuna uygulanabilir

Gizli dizileri için SSH özel anahtarının oluşturulması gerekir (**sshPrivateKey**), Azure AD İstemci gizli anahtarı (**aadClientSecret**), OpenShift yönetici parolası (**openshiftPassword** ) ve Red Hat Abonelik Yöneticisi parolası veya etkinleştirme anahtarı (**rhsmPasswordOrActivationKey**).  Özel SSL sertifikalarını kullandıysanız, ayrıca, daha sonra altı ek gizli dizileri oluşturulması - gerekecektir **routingcafile**, **routingcertfile**, **routingkeyfile**, **mastercafile**, **mastercertfile**, ve **masterkeyfile**.  Bu parametreler, daha ayrıntılı olarak açıklanacaktır.

Şablon belirli bir gizli dizi adları şekilde başvuruyor, **gerekir** kalın adları listelenen (büyük/küçük harfe duyarlı yukarıda) kullanın.

### <a name="custom-certificates"></a>Özel sertifikalar

Varsayılan olarak, şablon OpenShift web konsolu ve Yönlendirme etki alanı için otomatik olarak imzalanan sertifikaları kullanarak OpenShift kümesi dağıtır. Özel SSL sertifikalarını kullanmak istiyorsanız, 'custom' için ' routingCertType' ve 'custom' için ' masterCertType' ayarlayın.  .Pem biçiminde CA, sertifika ve anahtar dosyaları için sertifikalar gerekir.  Bir ancak diğer özel sertifikaları kullanmak mümkündür.

Bu dosyalar Key Vault gizli dizileri depolamak gerekir.  Özel anahtarı için kullanılan aynı anahtar kasası kullanın.  Gizli adları 6 ek girişler gerektiren yerine belirli bir gizli dizi adları SSL sertifikası dosyaların her biri için kullanılacak kodlanmış şablonudur.  Aşağıdaki tabloda yer alan bilgileri kullanarak sertifika verileri Store.

| Gizli dizi adı      | Sertifika dosyası   |
|------------------|--------------------|
| mastercafile     | Ana CA dosyası     |
| mastercertfile   | ana sertifika dosyası   |
| masterkeyfile    | Ana anahtar dosyası    |
| routingcafile    | Yönlendirme CA dosyası    |
| routingcertfile  | Yönlendirme sertifika dosyası  |
| routingkeyfile   | Yönlendirme anahtar dosyası   |

Azure CLI kullanarak gizli dizileri oluşturun. Aşağıda bir örnek verilmiştir.

```bash
az keyvault secret set --vault-name KeyVaultName -n mastercafile --file ~/certificates/masterca.pem
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, aşağıdaki konular ele:
> [!div class="checklist"]
> * OpenShift küme için SSH anahtarları yönetmek için bir anahtar kasası oluşturun.
> * Azure bulut çözümü sağlayıcısı tarafından kullanılmak üzere hizmet sorumlusu oluşturun.

Ardından, OpenShift kümesi dağıtın:

- [OpenShift kapsayıcı platformu dağıtma](./openshift-container-platform.md)
- [OpenShift kapsayıcı platformu kendi kendini yöneten bir Market teklifi dağıtma](./openshift-marketplace-self-managed.md)