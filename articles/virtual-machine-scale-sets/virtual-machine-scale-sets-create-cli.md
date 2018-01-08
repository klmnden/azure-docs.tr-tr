---
title: "Azure CLI 2.0 ile Sanal Makine Ölçek Kümesi Oluşturma | Microsoft Docs"
description: "Azure PowerShell ile hızlıca sanal makine ölçek kümesi oluşturmayı öğrenin"
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: get-started-article
ms.date: 12/19/2017
ms.author: iainfou
ms.openlocfilehash: 222bfa1c3070fa4634cf5c48d934a6387c48a4b0
ms.sourcegitcommit: 901a3ad293669093e3964ed3e717227946f0af96
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
---
# <a name="create-a-virtual-machine-scale-set-with-the-azure-cli-20"></a>Azure CLI 2.0 ile Sanal Makine Ölçek Kümesi oluşturma
Sanal makine ölçek kümesi, birbiriyle aynı ve otomatik olarak ölçeklendirilen sanal makine kümesi dağıtmanızı ve yönetmenizi sağlar. Ölçek kümesi içindeki VM sayısını el ile ölçeklendirebilir veya CPU, bellek isteği ya da ağ trafiği gibi kaynak kullanımını temel alan otomatik ölçeklendirme kuralları tanımlayabilirsiniz. Bu başlangıç makalesinde Azure CLI 2.0 ile bir sanal makine ölçek kümesi oluşturacaksınız. Ölçek kümesi oluşturmak için [Azure PowerShell](virtual-machine-scale-sets-create-powershell.md) veya [Azure portalı](virtual-machine-scale-sets-create-portal.md) da kullanabilirsiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici için Azure CLI 2.0.20 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 


## <a name="create-a-scale-set"></a>Ölçek kümesi oluşturma
Ölçek kümesi oluşturabilmek için [az group create](/cli/azure/group#create) ile bir kaynak grubu oluşturun. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

Bu adımda [az vmss create](/cli/azure/vmss#create) ile bir sanal makine ölçek kümesi oluşturun. Aşağıdaki örnek *myScaleSet* adlı bir ölçek kümesini ve yoksa SSH anahtarlarını oluşturur:

```azurecli-interactive 
az vmss create \
  --resource-group myResourceGroup \
  --name myScaleSet \
  --image UbuntuLTS \
  --upgrade-policy-mode automatic \
  --admin-username azureuser \
  --generate-ssh-keys
```

Tüm ölçek kümesi kaynaklarının ve VM'lerin oluşturulup yapılandırılması birkaç dakika sürer.


## <a name="install-nginx-webserver"></a>NGINX web sunucusunu yükleme
Ölçek kümenizi test etmek için Özel Betik Uzantısı'nı kullanarak VM örneklerine NGINX uygulamasını yükleyen bir betik indirip çalıştırın. Bu uzantı dağıtım sonrası yapılandırma, yazılım yükleme veya diğer yapılandırma/yönetim görevleri için kullanışlıdır. Daha fazla bilgi için bkz. [Özel Betik Uzantısı'na genel bakış](../virtual-machines/linux/extensions-customscript.md).

NGINX uygulamasını yükleyen Özel Betik Uzantısı'nı aşağıdaki şekilde uygulayın:

```azurecli-interactive
az vmss extension set \
  --publisher Microsoft.Azure.Extensions \
  --version 2.0 \
  --name CustomScript \
  --resource-group myResourceGroup \
  --vmss-name myScaleSet \
  --settings '{"fileUris":["https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/automate_nginx.sh"],"commandToExecute":"./automate_nginx.sh"}'
```


## <a name="allow-web-traffic"></a>Web trafiğine izin verme
Web trafiğinin web sunucunuza ulaşmasına izin vermek için [az network lb rule create](/cli/azure/network/lb/rule#create) ile bir yük dengeleyici kuralı oluşturun. Aşağıdaki örnek *myLoadBalancerRuleWeb* adlı bir kural oluşturur:

```azurecli-interactive 
az network lb rule create \
  --resource-group myResourceGroup \
  --name myLoadBalancerRuleWeb \
  --lb-name myScaleSetLB \
  --backend-pool-name myScaleSetLBBEPool \
  --backend-port 80 \
  --frontend-ip-name loadBalancerFrontEnd \
  --frontend-port 80 \
  --protocol tcp
```


## <a name="test-your-web-server"></a>Web sunucunuzu test etme
Web sunucunuzu çalışır halde görmek için [az network public-ip show](/cli/azure/network/public-ip#show) ile yük dengeleyicinizin genel IP adresini alın. Aşağıdaki örnek ölçek kümesinin bir parçası olarak oluşturulan *myScaleSetLBPublicIP* için IP adresini alır:

```azurecli-interactive 
az network public-ip show \
  --resource-group myResourceGroup \
  --name myScaleSetLBPublicIP \
  --query [ipAddress] \
  --output tsv
```

Yük dengeleyicinin genel IP adresini bir web tarayıcısına girin. Aşağıdaki örnekte gösterildiği gibi yük dengeleyici trafiği VM örneklerinizden birine dağıtır:

![NGINX varsayılan web sayfası](media/virtual-machine-scale-sets-create-cli/running-nginx-site.png)


## <a name="clean-up-resources"></a>Kaynakları temizleme
Artık gerekli değilse, aşağıdaki gibi [az group delete](/cli/azure/group#delete) komutunu kullanarak kaynak grubunu, ölçek kümesini tüm ilgili kaynakları kaldırabilirsiniz:

```azurecli-interactive 
az group delete --name myResourceGroup
```


## <a name="next-steps"></a>Sonraki adımlar
Bu başlangıç makalesinde basit bir ölçek kümesi oluşturdunuz ve Özel Betik Uzantısı'nı kullanarak VM örneklerine basit bir NGINX web sunucusu yüklediniz. Daha fazla ölçeklenebilirlik ve otomasyon için ölçek kümenizi aşağıdaki nasıl yapılır makaleleriyle genişletin:

- [Uygulamanızı sanal makine ölçek kümelerine dağıtma](virtual-machine-scale-sets-deploy-app.md)
- [Azure CLI](virtual-machine-scale-sets-autoscale-cli.md), [Azure PowerShell](virtual-machine-scale-sets-autoscale-powershell.md) veya [Azure portalı](virtual-machine-scale-sets-autoscale-portal.md) ile otomatik ölçeklendirme
- [Ölçek kümesi VM örnekleriniz için otomatik işletim sistemi yükseltmelerini kullanma](virtual-machine-scale-sets-automatic-upgrade.md)