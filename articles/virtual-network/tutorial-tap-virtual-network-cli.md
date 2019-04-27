---
title: Oluşturma, değiştirme veya silme bir sanal ağ TAP - Azure CLI | Microsoft Docs
description: Oluşturma, değiştirme veya bir sanal ağ Azure CLI kullanarak DOKUNUN silme öğrenin.
services: virtual-network
documentationcenter: na
author: karthikananth
manager: ganesr
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/18/2018
ms.author: kaanan
ms.openlocfilehash: 3d95a9ea555cceda82530eb5c487eeb993c1a678
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60743199"
---
# <a name="work-with-a-virtual-network-tap-using-the-azure-cli"></a>Bir DOKUNUN Azure CLI kullanarak sanal ağ ile çalışma

Azure sanal ağ TAP (Terminal erişim noktası) bir ağ paketi Toplayıcı veya Analiz aracı, sanal makine ağ trafiğini sürekli akışı sağlar. Toplayıcı veya Analiz aracı tarafından sağlanan bir [ağ sanal Gereci](https://azure.microsoft.com/solutions/network-appliances/) iş ortağı. Sanal ağ TAP ile çalışacak şekilde doğrulanmış olan iş ortağı çözümlerini bir listesi için bkz. [iş ortağı çözümleri](virtual-network-tap-overview.md#virtual-network-tap-partner-solutions). 

## <a name="create-a-virtual-network-tap-resource"></a>Sanal ağ TAP kaynak oluşturma

Okuma [önkoşulları](virtual-network-tap-overview.md#prerequisites) bir sanal ağ TAP kaynak oluşturmadan önce. İçinde izleyen komutları çalıştırabilirsiniz [Azure Cloud Shell](https://shell.azure.com/bash), veya Azure komut satırı arabirimi (CLI) bilgisayarınızdan çalıştırarak. Azure Cloud Shell, bilgisayarınızda Azure CLI yükleme gerektirmeyen bir ücretsiz etkileşimli kabuk ' dir. Azure için uygun olan bir hesapla oturum açmalısınız [izinleri](virtual-network-tap-overview.md#permissions). Bu makale Azure CLI Sürüm 2.0.46 gerekir veya üzeri. Yüklü sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli). Sanal ağ DOKUNUN, şu anda bir uzantısı olarak kullanılabilir. Çalışmanız için gereken uzantıyı yüklemek için `az extension add -n virtual-network-tap`. Azure CLI'yi yerel olarak çalıştırıyorsanız, aynı zamanda çalıştırmak ihtiyacınız `az login` Azure ile bir bağlantı oluşturmak için.

1. Sonraki adımlardan birinde kullanılan bir değişkenin aboneliğinize Kimliğini alın:

   ```azurecli-interactive
   subscriptionId=$(az account show \
   --query id \
   --out tsv)
   ```

2. Bir sanal ağ TAP kaynak oluşturmak için kullanacağınız abonelik kimliğini ayarlayın.

   ```azurecli-interactive
   az account set --subscription $subscriptionId
   ```

3. Bir sanal ağ TAP kaynak oluşturmak için kullanacağınız abonelik Kimliğini yeniden kaydedin. Bir DOKUNUN kaynak oluştururken bir kayıt hatası alırsanız, aşağıdaki komutu çalıştırın:

   ```azurecli-interactive
   az provider register --namespace Microsoft.Network --subscription $subscriptionId
   ```

4. Hedef sanal ağ TAP'için bir ağ sanal Gereci Toplayıcı veya Analiz aracı için ağ arabiriminde ise-

   - Ağ sanal Gereci ait ağ arabirimi IP yapılandırması sonraki adımlardan birinde kullanılan bir değişkenin içine alın. DOKUNUN trafiği toplayacak uç noktası kimliğidir. Aşağıdaki örnek Kimliğini alır. *ipconfig1* adlı bir ağ arabirimi IP yapılandırması *myNetworkInterface*, adlı bir kaynak grubu içinde *myResourceGroup*:

      ```azurecli-interactive
       IpConfigId=$(az network nic ip-config show \
       --name ipconfig1 \
       --nic-name myNetworkInterface \
       --resource-group myResourceGroup \
       --query id \
       --out tsv)
      ```

   - Hedef ve isteğe bağlı bağlantı noktası özelliği IP yapılandırmasının Kimliğini kullanarak westcentralus azure bölgesinde sanal ağ TAP'ı oluşturun. Bağlantı noktası, hedef bağlantı noktası üzerinde ağ arabirimi IP yapılandırması yere DOKUNUN trafiği alınır belirtir:  

      ```azurecli-interactive
       az network vnet tap create \
       --resource-group myResourceGroup \
       --name myTap \
       --destination $IpConfigId \
       --port 4789 \
       --location westcentralus
      ```

5. Hedef sanal ağ TAP'için bir azure iç yük dengeleyici ise:
  
   - Azure iç yük dengeleyici ön uç IP yapılandırması sonraki adımlardan birinde kullanılan bir değişkenin içine alın. DOKUNUN trafiği toplayacak uç noktası kimliğidir. Aşağıdaki örnek Kimliğini alır. *frontendipconfig1* adlı bir yük dengeleyici için ön uç IP yapılandırması *myInternalLoadBalancer*, adlı bir kaynak grubu içinde  *myResourceGroup*:

      ```azurecli-interactive
      FrontendIpConfigId=$(az network lb frontend-ip show \
      --name frontendipconfig1 \
      --lb-name myInternalLoadBalancer \
      --resource-group myResourceGroup \
      --query id \
      --out tsv)
      ```
   - Sanal ağ TAP hedef ve isteğe bağlı bağlantı noktası özelliği ön uç IP yapılandırması Kimliğini kullanarak oluşturun. Bağlantı noktası, hedef bağlantı noktası yere DOKUNUN trafiği alınır, ön uç IP yapılandırması üzerinde belirtir:  

      ```azurecli-interactive
      az network vnet tap create \
      --resource-group myResourceGroup \
      --name myTap \
      --destination $FrontendIpConfigId \
      --port 4789 \
     --location westcentralus
     ```

6. Sanal ağ TAP oluşturulmasını onaylayın:

   ```azurecli-interactive
   az network vnet tap show \
   --resource-group myResourceGroup
   --name myTap
   ```

## <a name="add-a-tap-configuration-to-a-network-interface"></a>Bir ağ arabirimi için bir DOKUNUN Yapılandırması Ekle

1. Mevcut bir sanal ağ TAP kaynak Kimliğini alın. Aşağıdaki örnek, bir sanal ağ TAP adlı alır. *myTap* adlı bir kaynak grubu içinde *myResourceGroup*:

   ```azurecli-interactive
   tapId=$(az network vnet tap show \
   --name myTap \
   --resource-group myResourceGroup \
   --query id \
   --out tsv)
   ```

2. İzlenen sanal makinenin ağ arabiriminde bir DOKUNUN yapılandırması oluşturun. Aşağıdaki örnek bir DOKUNUN Yapılandırması adlı bir ağ arabirimi oluşturur *myNetworkInterface*:

   ```azurecli-interactive
   az network nic vtap-config create \
   --resource-group myResourceGroup \
   --nic myNetworkInterface \
   --vnet-tap $tapId \
   --name mytapconfig \
   --subscription subscriptionId
   ```

3. DOKUNUN yapılandırmanın oluşturulmasını onaylayın:

   ```azurecli-interactive
   az network nic vtap-config show \
   --resource-group myResourceGroup \
   --nic-name myNetworkInterface \
   --name mytapconfig \
   --subscription subscriptionId
   ```

## <a name="delete-the-tap-configuration-on-a-network-interface"></a>Bir ağ arabiriminde DOKUNUN yapılandırmasını Sil

   ```azure-cli-interactive
   az network nic vtap-config delete \
   --resource-group myResourceGroup \
   --nic myNetworkInterface \
   --name myTapConfig \
   --subscription subscriptionId
   ```

## <a name="list-virtual-network-taps-in-a-subscription"></a>Bir Abonelikteki sanal ağlar dokunduğunda

   ```azurecli-interactive
   az network vnet tap list
   ```

## <a name="delete-a-virtual-network-tap-in-a-resource-group"></a>Bir kaynak grubundaki sanal ağ TAP Sil

   ```azurecli-interactive
   az network vnet tap delete \
   --resource-group myResourceGroup \
   --name myTap
   ```
