---
title: "OpenShift kaynak Azure'a dağıtma | Microsoft Docs"
description: "Azure sanal makinelerine OpenShift kaynak dağıtmayı öğrenin."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: jbinder
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 
ms.author: jbinder
ms.openlocfilehash: e03da05625e440eab29ccc28a2343d3433fc7607
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-openshift-origin-to-azure-virtual-machines"></a>Azure sanal makinelerine OpenShift kaynak dağıtma 

[OpenShift kaynak](https://www.openshift.org/) üzerinde oluşturulan bir açık kaynak kapsayıcı platformdur [Kubernetes](https://kubernetes.io/). Dağıtma, ölçeklendirme ve çok kiracılı uygulamalara çalışan işlemini basitleştirir. 

Bu kılavuz, OpenShift kaynak üzerinde Azure Azure CLI ve Azure Resource Manager şablonları kullanarak sanal makineleri dağıtmayı açıklar. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * OpenShift küme için SSH anahtarları yönetmek için bir KeyVault oluşturun.
> * Azure VM'ler OpenShift kümede dağıtın. 
> * Yükleme ve yapılandırma [OpenShift CLI](https://docs.openshift.org/latest/cli_reference/index.html#cli-reference-index) kümeyi yönetmek için.
> * OpenShift dağıtım özelleştirin.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

Bu hızlı başlangıç Azure CLI Sürüm 2.0.8 gerektirir veya sonraki bir sürümü. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="log-in-to-azure"></a>Azure'da oturum açma 
Oturum açtığınızda, Azure aboneliğinizle [az oturum açma](/cli/azure/#login) komut ve izleyin ekrandaki yönergeleri veya tıklatın **deneyin** bulut Kabuğu'nu kullanmak için.

```azurecli 
az login
```
## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group#create) komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. 

Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-key-vault"></a>Anahtar kasası oluşturma
Küme için SSH anahtarları depolamak için bir KeyVault oluşturma [az keyvault oluşturma](/cli/azure/keyvault#create) komutu.  

```azurecli 
az keyvault create --resource-group myResourceGroup --name myKeyVault \
       --enabled-for-template-deployment true \
       --location eastus
```

## <a name="create-an-ssh-key"></a>SSH anahtarı oluşturma 
Bir SSH anahtarı OpenShift kaynak kümesine erişimi güvenli hale getirmek için gereklidir. Kullanarak bir SSH anahtar çifti oluşturma `ssh-keygen` komutu. 
 
 ```bash
ssh-keygen -f ~/.ssh/openshift_rsa -t rsa -N ''
```

> [!NOTE]
> Oluşturduğunuz SSH anahtar çiftiniz bir parola olmaması gerekir.

Windows, SSH anahtarları hakkında daha fazla bilgi için [SSH oluşturma Windows anahtarları](/azure/virtual-machines/linux/ssh-from-windows).

## <a name="store-ssh-private-key-in-key-vault"></a>Anahtar kasasına SSH özel anahtar depolama
OpenShift dağıtım OpenShift asıl güvenli erişim için oluşturulan SSH anahtarı kullanır. SSH anahtarı güvenli bir şekilde almasını dağıtımını etkinleştirmek için aşağıdaki komutu kullanarak anahtar kasasına anahtarı depolar.

# <a name="enabled-for-template-deployment"></a>Şablon dağıtımı için etkin
```azurecli
az keyvault secret set --vault-name KeyVaultName --name OpenShiftKey --file ~/.ssh/openshift.rsa
```

## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma 
OpenShift bir kullanıcı adı ve parola veya bir hizmet sorumlusu kullanarak Azure ile iletişim kurar. Bir Azure hizmet sorumlusu uygulamaları, hizmetleri ve OpenShift gibi Otomasyon araçları ile birlikte kullanabileceğiniz bir güvenlik kimliğidir. Denetim ve hizmet sorumlusu Azure'da gerçekleştirebilirsiniz ne gibi işlemler için izinler tanımlar. Yalnızca bir kullanıcı adı ve parola sağlayarak üzerinden güvenliğini artırmak için bu örnek temel bir hizmet sorumlusu oluşturur.

Bir hizmet sorumlusu ile oluşturma [az ad sp oluşturma-için-rbac](/cli/azure/ad/sp#create-for-rbac) ve OpenShift gereken kimlik bilgilerini çıktı:

```azurecli
az ad sp create-for-rbac --name openshiftsp \
          --role Contributor --password {strong password} \
          --scopes $(az group show --name myResourceGroup --query id)
```
Komuttan döndürülen AppID özelliği not edin.
```json
{
  "appId": "a487e0c1-82af-47d9-9a0b-af184eb87646d",
  "displayName": "openshiftsp",
  "name": "http://openshiftsp",
  "password": {strong password},
  "tenant": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"
}
```
 > [!WARNING] 
 > Güvenli olmayan bir parola oluşturmayın.  [Azure AD parola kuralları ve kısıtlamaları](/azure/active-directory/active-directory-passwords-policy) yönergelerini izleyin.

Hizmet sorumluları hakkında daha fazla bilgi için bkz: [bir Azure hizmet sorumlusu Azure CLI 2.0 ile oluşturun.](/cli/azure/create-an-azure-service-principal-azure-cli)

## <a name="deploy-the-openshift-origin-template"></a>OpenShift kaynak şablonu dağıtma
Sonraki OpenShift bir Azure Resource Manager şablonu kullanarak kaynak dağıtın. 

> [!NOTE] 
> Aşağıdaki komutu az CLI 2.0.8 gerektirir veya sonraki bir sürümü. CLI az doğrulayabilirsiniz sürümüyle `az --version` komutu. CLI Sürüm güncelleştirmek için bkz: [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli).

Kullanım `appId` daha önce oluşturduğunuz için hizmet sorumlusu değerinden `aadClientId` parametresi.

```azurecli 
az group deployment create --name myOpenShiftCluster \
      --template-uri https://raw.githubusercontent.com/Microsoft/openshift-origin/master/azuredeploy.json \
      --params \ 
        openshiftMasterPublicIpDnsLabel=myopenshiftmaster \
        infraLbPublicIpDnsLabel=myopenshiftlb \
        openshiftPassword=Pass@word!
        sshPublicKey=~/.ssh/openshift_rsa.pub \
        keyVaultResourceGroup=myResourceGroup \
        keyVaultName=myKeyVault \
        keyVaultSecret=OpenShiftKey \
        aadClientId={appId} \
        aadClientSecret={strong password} 
```
Dağıtımın tamamlanması 20 dakika sürebilir. Dağıtım tamamlandığında OpenShift asıl DNS adını ve OpenShift konsol URL'sini terminale yazdırılır.

```json
{
  "OpenShift Console Uri": "http://openshiftlb.cloudapp.azure.com:8443/console",
  "OpenShift Master SSH": "ocpadmin@myopenshiftmaster.cloudapp.azure.com"
}
```
## <a name="connect-to-the-openshift-cluster"></a>OpenShift kümeye bağlanın
Dağıtım tamamlandığında, tarayıcı kullanarak OpenShift Konsolu'na bağlanmak `OpenShift Console Uri`. Alternatif olarak, aşağıdaki komutu kullanarak OpenShift ana bağlanabilir.

```bash
$ ssh ocpadmin@myopenshiftmaster.cloudapp.azure.com
```

## <a name="clean-up-resources"></a>Kaynakları temizleme
Artık gerekli olduğunda, kullanabileceğiniz [az grubu Sil](/cli/azure/group#delete) OpenShift küme, kaynak grubu kaldırmak için komut ve ilişkili tüm kaynakları.

```azurecli 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğretici, öğrenilmiş nasıl için:
> [!div class="checklist"]
> * OpenShift küme için SSH anahtarları yönetmek için bir KeyVault oluşturun.
> * Azure VM'ler OpenShift kümede dağıtın. 
> * Yükleme ve yapılandırma [OpenShift CLI](https://docs.openshift.org/latest/cli_reference/index.html#cli-reference-index) kümeyi yönetmek için.

Şimdi bu OpenShift kaynak küme dağıtılır. İlk Uygulamanızı dağıtmak ve OpenShift araçlarını kullanma hakkında bilgi almak için OpenShift öğreticileri izleyebilirsiniz. Bkz: [OpenShift kaynağı ile çalışmaya başlama](https://docs.openshift.org/latest/getting_started/index.html) başlamak için. 
