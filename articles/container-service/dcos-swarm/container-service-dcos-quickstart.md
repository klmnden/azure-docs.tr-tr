---
title: (KULLANIM DIŞI) Azure Container Service hızlı başlangıç - DC/OS kümesi dağıtma
description: Azure Container Service Hızlı Başlangıç - DC/OS Kümesi Dağıtma
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: quickstart
ms.date: 02/26/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: efaf82c3f378f572c289b587dbe5df1923a58c62
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61457065"
---
# <a name="deprecated-deploy-a-dcos-cluster"></a>(KULLANIM DIŞI) DC/OS kümesi dağıtma

[!INCLUDE [ACS deprecation](../../../includes/container-service-deprecation.md)]

DC/OS, modern ve kapsayıcılı uygulamalar çalıştırmak için dağıtılmış bir platform sunar. Azure Container Service ile üretime hazır bir DC/OS kümesinin dağıtımı basit ve hızlıdır. Bu hızlı başlangıçta, DC/OS kümesi dağıtmak ve temel iş yükü çalıştırmak için gerekli temel adımlar ayrıntılı olarak açıklanır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

Bu öğretici, Azure CLI 2.0.4 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükseltme gerekiyorsa, bkz. [Azure CLI yükleme]( /cli/azure/install-azure-cli). 

## <a name="log-in-to-azure"></a>Azure'da oturum açma 

[az login](/cli/azure/reference-index#az-login) komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group#az-group-create) komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. 

Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli
az group create --name myResourceGroup --location eastus
```

## <a name="create-dcos-cluster"></a>DC/OS kümesi oluşturma

[az acs create](/cli/azure/acs#az-acs-create) komutuyla bir DC/OS kümesi oluşturun.

Aşağıdaki örnekte *myDCOSCluster* adlı bir DC/OS kümesi ve henüz yoksa SSH anahtarları oluşturulur. Belirli bir anahtar kümesini kullanmak için `--ssh-key-value` seçeneğini kullanın.  

```azurecli
az acs create --orchestrator-type dcos --resource-group myResourceGroup --name myDCOSCluster --generate-ssh-keys
```

Sınırlı deneme sürümünde olduğu gibi bazı durumlarda, bir Azure aboneliğinin Azure kaynaklarına sınırlı erişimi olur. Dağıtım sınırlı kullanılabilir çekirdek sayısı nedeniyle başarısız olursa, `--agent-count 1` öğesini [az acs create](/cli/azure/acs#az-acs-create) komutuna ekleyerek varsayılan aracı sayısını azaltın. 

Birkaç dakika sonra komut tamamlanır ve dağıtım hakkında bilgi döndürür.

## <a name="connect-to-dcos-cluster"></a>DC/OS kümesine bağlanma

DC/OS kümesi oluşturulduktan sonra bir SSH tüneli üzerinden kümeye erişilebilir. DC/OS ana kümesinin genel IP adresini döndürmek için aşağıdaki komutu çalıştırın. Bu IP adresi bir değişkende depolanır ve bir sonraki adımda kullanılır.

```azurecli
ip=$(az network public-ip list --resource-group myResourceGroup --query "[?contains(name,'dcos-master')].[ipAddress]" -o tsv)
```

SSH tüneli oluşturmak için aşağıdaki komutu çalıştırın ve ekrandaki yönergeleri izleyin. 80 bağlantı noktası zaten kullanımdaysa komut başarısız olur. Tünel oluşturulacak bağlantı noktasını kullanımda olmayan bir bağlantı noktası olarak (`85:localhost:80` gibi) güncelleştirin. 

```azurecli
sudo ssh -i ~/.ssh/id_rsa -fNL 80:localhost:80 -p 2200 azureuser@$ip
```

SSH tüneli `http://localhost` yoluna göz atılarak test edilebilir. 80 dışında bir bağlantı noktası kullanılırsa konumu eşleşecek şekilde ayarlayın. 

SSH tüneli başarıyla oluşturulursa DC/OS portalı döndürülür.

![DCOS KULLANICI ARABİRİMİ](./media/container-service-dcos-quickstart/dcos-ui.png)

## <a name="install-dcos-cli"></a>DC/OS CLI’yı yükleyin

DC/OS komut satırı arabirimi, bir DC/OS kümesini komut satırından yönetmek için kullanılır. [az acs dcos install-cli](/cli/azure/acs/dcos#az-acs-dcos-install-cli) komutunu kullanarak DC/OS CLI’yı yükleyin. Azure CloudShell kullanıyorsanız DC/OS CLI zaten yüklüdür. 

Azure CLI’yı macOS ya da Linux’ta çalıştırıyorsanız komutu sudo ile çalıştırmanız gerekebilir.

```azurecli
az acs dcos install-cli
```

CLI’nın kümeyle kullanılabilmesi için önce SSH tünelini kullanacak şekilde yapılandırılması gerekir. Bunu yapmak için, gerekirse bağlantı noktasını değiştirerek aşağıdaki komutu çalıştırın.

```azurecli
dcos config set core.dcos_url http://localhost
```

## <a name="run-an-application"></a>Bir uygulamayı çalıştırma

Bir ACS DC/OS kümesi için varsayılan zamanlama mekanizması Marathon’dur. DC/OS kümesinde bir uygulamayı başlatmak ve uygulamanın durumunu yönetmek için Marathon kullanılır. Marathon aracılığıyla uygulama zamanlamak için *marathon-app.json* adlı bir dosya oluşturun ve aşağıdaki içeriği buna kopyalayın. 

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

Uygulamanın dağıtım durumunu görmek için aşağıdaki komutu çalıştırın.

```azurecli
dcos marathon app list
```

**WAITING** sütunundaki *True* değeri *False* olarak değiştiğinde uygulama dağıtımı tamamlanmış olur.

```azurecli
ID     MEM  CPUS  TASKS  HEALTH  DEPLOYMENT  WAITING  CONTAINER  CMD   
/test   32   1     1/1    ---       ---      False      DOCKER   None
```

DC/OS kümesi aracılarının genel IP adresini alın.

```azurecli
az network public-ip list --resource-group myResourceGroup --query "[?contains(name,'dcos-agent')].[ipAddress]" -o tsv
```

Bu adrese göz atıldığında varsayılan NGINX sitesi döndürülür.

![NGINX](./media/container-service-dcos-quickstart/nginx.png)

## <a name="delete-dcos-cluster"></a>DC/OS kümesini silme

Artık gerekli değilse, [az group delete](/cli/azure/group#az-group-delete) komutunu kullanarak kaynak grubunu, DC/OS kümesini ve tüm ilgili kaynakları kaldırabilirsiniz.

```azurecli
az group delete --name myResourceGroup --no-wait
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta bir DC/OS kümesi dağıttınız ve kümede basit bir Docker kapsayıcısı çalıştırdınız. Azure Container Service hakkında daha fazla bilgi edinmek için ACS öğreticilerine geçin.

> [!div class="nextstepaction"]
> [Bir ACS DC/OS Kümesini Yönetme](container-service-dcos-manage-tutorial.md)