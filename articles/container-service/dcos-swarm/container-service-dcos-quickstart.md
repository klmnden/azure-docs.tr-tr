---
title: "Azure kapsayıcı hizmeti hızlı başlangıç - DC/OS kümesi dağıtma | Microsoft Docs"
description: "Azure kapsayıcı hizmeti hızlı başlangıç - DC/OS kümesi dağıtma"
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, Container’lar, Mikro hizmetler, Kumernetes, DC/OS, Azure"
ms.assetid: 
ms.service: container-service
ms.devlang: azurecli
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/04/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 8070d224fe6281e61f67483d4f1dd905a2ab99eb
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-a-dcos-cluster"></a>DC/OS kümesi dağıtma

DC/OS çalışan modern ve kapsayıcılı uygulamaları için Dağıtılmış bir platform sağlar. Azure kapsayıcı hizmeti ile basit ve hızlı bir üretim hazır DC/OS kümesi sağlama. Bu hızlı başlangıç ayrıntıları DC/OS dağıtmak için gereken temel adımlarda, küme ve temel iş yükü çalıştırın.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

Bu öğretici, Azure CLI 2.0.4 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükseltme gerekiyorsa, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

## <a name="log-in-to-azure"></a>Azure'da oturum açma 

[az login](/cli/azure/#login) komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group#create) komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. 

Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli
az group create --name myResourceGroup --location eastus
```

## <a name="create-dcos-cluster"></a>DC/OS kümesi oluşturma

DC/OS kümesi ile oluşturma [az acs oluşturmak](/cli/azure/acs#create) komutu.

Aşağıdaki örnek, adlandırılmış bir DC/OS kümesi oluşturur *myDCOSCluster* ve zaten mevcut değilse SSH anahtarları oluşturur. Belirli bir anahtar kümesini kullanmak için `--ssh-key-value` seçeneğini kullanın.  

```azurecli
az acs create --orchestrator-type dcos --resource-group myResourceGroup --name myDCOSCluster --generate-ssh-keys
```

Sınırlı deneme sürümünde olduğu gibi bazı durumlarda, bir Azure aboneliğinin Azure kaynaklarına sınırlı erişimi olur. Dağıtım sınırlı kullanılabilir çekirdek sayısı nedeniyle başarısız olursa, `--agent-count 1` öğesini [az acs create](/cli/azure/acs#create) komutuna ekleyerek varsayılan aracı sayısını azaltın. 

Birkaç dakika sonra komut tamamlandıktan ve dağıtım hakkında bilgi döndürür.

## <a name="connect-to-dcos-cluster"></a>DC/OS kümesine bağlanma

DC/OS kümesi oluşturulduktan sonra bir SSH tüneli üzerinden erişimleri olabilir. DC/OS asıl ortak IP adresini döndürmek için aşağıdaki komutu çalıştırın. Bu IP adresi bir değişkende depolanan ve sonraki adımda kullanılır.

```azurecli
ip=$(az network public-ip list --resource-group myResourceGroup --query "[?contains(name,'dcos-master')].[ipAddress]" -o tsv)
```

SSH tüneli oluşturmak için aşağıdaki komutu çalıştırın ve izleyin ekrandaki yönergeleri. Bağlantı noktası 80 kullanımda olduğundan komut başarısız olur. Tünel bağlantı noktası gibi bir içinde değil kullanımına güncelleştirme `85:localhost:80`. 

```azurecli
sudo ssh -i ~/.ssh/id_rsa -fNL 80:localhost:80 -p 2200 azureuser@$ip
```

SSH tüneli göz atarak test edilebilir `http://localhost`. Bir bağlantı noktası, diğer 80 oluşturulduğunu konumun eşleşecek şekilde ayarlayın. 

SSH tüneli başarıyla oluşturulduysa, DC/OS portal döndürülür.

![DCOS KULLANICI ARABİRİMİ](./media/container-service-dcos-quickstart/dcos-ui.png)

## <a name="install-dcos-cli"></a>DC/OS CLI'yi yükleyin

DC/OS komut satırı arabirimi, DC/OS kümesi komut satırından yönetmek için kullanılır. DC/OS CLI kullanarak yükleme [az acs dcos yükleme-CLI](/azure/acs/dcos#install-cli) komutu. Azure CloudShell kullanıyorsanız, DC/OS CLI zaten yüklü. 

Azure CLI macOS ya da Linux üzerinde çalıştırıyorsanız, sudo komutu çalıştırmanız gerekebilir.

```azurecli
az acs dcos install-cli
```

CLI kümeyle kullanılabilmesi için önce SSH tüneli kullanacak şekilde yapılandırılmalıdır. Bunu yapmak için bağlantı noktası gerekiyorsa ayarlayarak aşağıdaki komutu çalıştırın.

```azurecli
dcos config set core.dcos_url http://localhost
```

## <a name="run-an-application"></a>Bir uygulamayı çalıştırın

Bir ACS DC/OS kümesi için mekanizma zamanlama Marathon varsayılandır. Marathon, uygulama başlatma ve DC/OS kümesinde uygulama durumunu yönetmek için kullanılır. Marathon aracılığıyla bir uygulama zamanlamak için adlı bir dosya oluşturun *marathon app.json*ve aşağıdaki içeriği buraya kopyalayın. 

```json
{
  "id": "demo-app",
  "cmd": null,
  "cpus": 1,
  "mem": 32,
  "disk": 0,
  "instances": 1,
  "container": {
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        {
          "containerPort": 80,
          "hostPort": 80,
          "protocol": "tcp",
          "name": "80",
          "labels": null
        }
      ]
    },
    "type": "DOCKER"
  },
  "acceptedResourceRoles": [
    "slave_public"
  ]
}
```

Uygulamayı DC/OS kümesinde çalışacak şekilde zamanlamak için aşağıdaki komutu çalıştırın.

```azurecli
dcos marathon app add marathon-app.json
```

Uygulama dağıtımı durumunu görmek için aşağıdaki komutu çalıştırın.

```azurecli
dcos marathon app list
```

Zaman **BEKLEYEN** sütun değeri geçer *True* için *yanlış*, uygulama dağıtımı tamamlandı.

```azurecli
ID     MEM  CPUS  TASKS  HEALTH  DEPLOYMENT  WAITING  CONTAINER  CMD   
/test   32   1     1/1    ---       ---      False      DOCKER   None
```

DC/OS kümesi aracıların genel IP adresi alın.

```azurecli
az network public-ip list --resource-group myResourceGroup --query "[?contains(name,'dcos-agent')].[ipAddress]" -o tsv
```

Bu adrese gözatma varsayılan NGINX site döndürür.

![NGINX](./media/container-service-dcos-quickstart/nginx.png)

## <a name="delete-dcos-cluster"></a>DC/OS kümesi Sil

Artık gerekli olduğunda, kullanabileceğiniz [az grubu Sil](/cli/azure/group#delete) kaynak grubu, DC/OS kümesi kaldırmak için komut ve ilişkili tüm kaynakları.

```azurecli
az group delete --name myResourceGroup --no-wait
```

## <a name="next-steps"></a>Sonraki adımlar

Bu Hızlı Başlangıç, DC/OS kümesi dağıttıktan sonra ve basit bir Docker kapsayıcısı kümede çalışan. Azure kapsayıcı hizmeti hakkında daha fazla bilgi için ACS öğreticileri devam edin.

> [!div class="nextstepaction"]
> [Bir ACS DC/OS kümesi yönetme](container-service-dcos-manage-tutorial.md)