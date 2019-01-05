---
title: 'Hızlı Başlangıç: Azure Application Gateway ile web trafiğini yönlendirme - Azure CLI | Microsoft Docs'
description: Web trafiğini arka uç havuzundaki sanal makinelere yönlendiren bir Azure Application Gateway oluşturmak üzere Azure CLI’sini kullanmayı öğrenin.
services: application-gateway
author: vhorne
manager: jpconnock
editor: ''
tags: azure-resource-manager
ms.service: application-gateway
ms.devlang: azurecli
ms.topic: quickstart
ms.workload: infrastructure-services
ms.date: 12/13/2018
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 59c7781efa8aaa6405ef3cb021ca2123d94ad61b
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54035500"
---
# <a name="quickstart-direct-web-traffic-with-azure-application-gateway---azure-cli"></a>Hızlı Başlangıç: Azure Application Gateway - Azure CLI ile doğrudan web trafiği

Bu hızlı başlangıçta hızlı bir şekilde, arka uç havuzunda iki sanal makine ile bir uygulama ağ geçidi oluşturmak için Azure CLI'yı kullanmayı gösterir. Ardından doğru bir şekilde çalışıp çalışmadığından emin olmak için test edersiniz. Azure Application Gateway ile belirli kaynaklar tarafından uygulama web trafiği doğrudan: dinleyici bağlantı noktalarına atama, kuralları oluşturma ve kaynakları bir arka uç havuzuna ekleme.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz Azure CLI 2.0.4 sürüm çalıştırın veya üzeri. Sürümü bulmak için çalıştırın **az--version**. Yükleme veya yükseltme hakkında daha fazla bilgi için bkz: [Azure CLI yükleme]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure'da, bir kaynak grubu için ilgili kaynakları ayırın. Kullanarak bir kaynak grubu oluşturma [az grubu oluşturma](/cli/azure/group#az-group-create). 

Aşağıdaki örnek *eastus* konumunda *myResourceGroupAG* adlı bir kaynak grubu oluşturur.

```azurecli-interactive 
az group create --name myResourceGroupAG --location eastus
```

## <a name="create-network-resources"></a>Ağ kaynakları oluşturma 

Uygulama ağ geçidi, bir sanal ağ oluşturduğunuzda, diğer kaynaklarla iletişim kurabilir. Uygulama ağ geçidini oluştururken aynı zamanda bir sanal makine oluşturabilirsiniz. Bu örnekte iki alt ağ oluşturacaksınız: biri uygulama ağ geçidi ve diğer sanal makineler için. 

Sanal ağ ve alt ağ oluşturmak için kullandığınız [az ağ sanal ağ oluşturma](/cli/azure/network/vnet#az-network-vnet-create). Çalıştırma [az network public-IP oluşturma](/cli/azure/network/public-ip#az-public-ip-create) genel IP adresi oluşturmak için.

```azurecli-interactive
az network vnet create \
  --name myVNet \
  --resource-group myResourceGroupAG \
  --location eastus \
  --address-prefix 10.0.0.0/16 \
  --subnet-name myAGSubnet \
  --subnet-prefix 10.0.1.0/24
az network vnet subnet create \
  --name myBackendSubnet \
  --resource-group myResourceGroupAG \
  --vnet-name myVNet   \
  --address-prefix 10.0.2.0/24
az network public-ip create \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress
```

## <a name="create-backend-servers"></a>Arka uç sunucular oluşturma

Bu örnekte, Azure application gateway için arka uç sunucular olarak kullanan iki sanal makine oluşturun. 

### <a name="create-two-virtual-machines"></a>İki sanal makine oluşturma

Yükleme [NGINX web sunucusunu](https://docs.nginx.com/nginx/) uygulamayı doğrulamak için sanal makinelerde ağ geçidi başarıyla oluşturuldu. Cloud-init yapılandırma dosyasını kullanarak NGINX'i yükleyebilir ve bir "Merhaba Dünya" Node.js uygulaması bir Linux sanal makinesinde çalıştırmak için kullanabilirsiniz. Cloud-init hakkında daha fazla bilgi için bkz: [azure'da sanal makineler için Cloud-init desteğine](https://docs.microsoft.com/azure/virtual-machines/linux/using-cloud-init).

Azure Cloud Shell'de kopyalayın ve adlı bir dosya aşağıdaki yapılandırmayı yapıştırın *cloud-init.txt*. Girin *Düzenleyicisi cloud-init.txt* dosyası oluşturmak için.

```yaml
#cloud-config
package_upgrade: true
packages:
  - nginx
  - nodejs
  - npm
write_files:
  - owner: www-data:www-data
  - path: /etc/nginx/sites-available/default
    content: |
      server {
        listen 80;
        location / {
          proxy_pass http://localhost:3000;
          proxy_http_version 1.1;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection keep-alive;
          proxy_set_header Host $host;
          proxy_cache_bypass $http_upgrade;
        }
      }
  - owner: azureuser:azureuser
  - path: /home/azureuser/myapp/index.js
    content: |
      var express = require('express')
      var app = express()
      var os = require('os');
      app.get('/', function (req, res) {
        res.send('Hello World from host ' + os.hostname() + '!')
      })
      app.listen(3000, function () {
        console.log('Hello world app listening on port 3000!')
      })
runcmd:
  - service nginx restart
  - cd "/home/azureuser/myapp"
  - npm init
  - npm install express -y
  - nodejs index.js
```

[az network nic oluşturma](/cli/azure/network/nic#az-network-nic-create) ile ağ arabirimlerini oluşturun. Sanal makineler oluşturmak için kullandığınız [az vm oluşturma](/cli/azure/vm#az-vm-create).

```azurecli-interactive
for i in `seq 1 2`; do
  az network nic create \
    --resource-group myResourceGroupAG \
    --name myNic$i \
    --vnet-name myVNet \
    --subnet myBackendSubnet
  az vm create \
    --resource-group myResourceGroupAG \
    --name myVM$i \
    --nics myNic$i \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt
done
```

## <a name="create-the-application-gateway"></a>Uygulama ağ geçidi oluşturma

Kullanarak bir uygulama ağ geçidi oluşturma [az ağ application-gateway oluşturma](/cli/azure/network/application-gateway#az-application-gateway-create). Azure CLI ile bir uygulama ağ geçidi oluşturduğunuzda, belirttiğiniz kapasite SKU ve HTTP gibi yapılandırma bilgilerini ayarlar. Azure, özel IP adresleri ağ arabirimlerinin sonra uygulama ağ geçidinin arka uç havuzuna sunucuları olarak ekler.

```azurecli-interactive
address1=$(az network nic show --name myNic1 --resource-group myResourceGroupAG | grep "\"privateIpAddress\":" | grep -oE '[^ ]+$' | tr -d '",')
address2=$(az network nic show --name myNic2 --resource-group myResourceGroupAG | grep "\"privateIpAddress\":" | grep -oE '[^ ]+$' | tr -d '",')
az network application-gateway create \
  --name myAppGateway \
  --location eastus \
  --resource-group myResourceGroupAG \
  --capacity 2 \
  --sku Standard_Medium \
  --http-settings-cookie-based-affinity Enabled \
  --public-ip-address myAGPublicIPAddress \
  --vnet-name myVNet \
  --subnet myAGSubnet \
  --servers "$address1" "$address2"
```

Bu uygulama ağ geçidi oluşturmak için Azure'da 30 dakikaya kadar sürebilir. Oluşturulduktan sonra aşağıdaki ayarlarda görüntüleyebileceğiniz **ayarları** bölümünü **uygulama ağ geçidi** sayfası:

- **appGatewayBackendPool**: Bulunan **arka uç havuzları** sayfası. Bu, gerekli arka uç havuzu belirtir.
- **appGatewayBackendHttpSettings**: Bulunan **HTTP ayarları** sayfası. Bu, uygulama ağ geçidi iletişim için 80 numaralı bağlantı noktasını ve HTTP protokolünü kullanan belirtir.
- **appGatewayHttpListener**: Bulunan **dinleyicileri sayfa**. İle ilişkili varsayılan dinleyici belirtir **appGatewayBackendPool**.
- **appGatewayFrontendIP**: Bulunan **ön uç IP yapılandırmaları** sayfası. Buna atar *myAGPublicIPAddress* için **appGatewayHttpListener**.
- **rule1**: Bulunan **kuralları** sayfası. İle ilişkili varsayılan yönlendirme kuralı belirtir **appGatewayHttpListener**.

## <a name="test-the-application-gateway"></a>Uygulama ağ geçidini test etme

Azure uygulama ağ geçidi oluşturmak için bir NGINX web sunucusunu gerektirmez, ancak bu hızlı başlangıçta Azure uygulama ağ geçidi başarıyla oluşturulmuş olup olmadığını doğrulamak için yüklü. Yeni uygulama ağ geçidi genel IP adresini almak için kullanın [az ağ public-ip show](/cli/azure/network/public-ip#az-network-public-ip-show). 

```azurepowershell-interactive
az network public-ip show \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress \
  --query [ipAddress] \
  --output tsv
``` 

Kopyalama ve genel IP adresi, tarayıcınızın adres çubuğuna yapıştırın.
    
![Uygulama ağ geçidini test etme](./media/quick-create-cli/application-gateway-nginxtest.png)

Tarayıcıyı yenileyin, ikinci bir sanal makine adını görmeniz gerekir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Application gateway ile oluşturduğunuz kaynaklara artık ihtiyacınız olduğunda kullanın [az grubu Sil](/cli/azure/group#az-group-delete) kaynak grubunu kaldırmak için komutu. Kaynak grubu kaldırarak, ayrıca uygulama ağ geçidi ve tüm ilgili kaynakları kaldırın.

```azurecli-interactive 
az group delete --name myResourceGroupAG
```
 
## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure CLI kullanarak bir uygulama ağ geçidi ile web trafiğini yönetme](./tutorial-manage-web-traffic-cli.md)

