---
title: 'Hızlı Başlangıç: Azure Application Gateway ile web trafiğini yönlendirme - Azure CLI | Microsoft Docs'
description: Web trafiğini arka uç havuzundaki sanal makinelere yönlendiren bir Azure Application Gateway oluşturmak üzere Azure CLI’sini kullanmayı öğrenin.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: quickstart
ms.date: 1/8/2019
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: a4f6cc2af7b9e044e5a72767898f876932fbf973
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/12/2019
ms.locfileid: "59528316"
---
# <a name="quickstart-direct-web-traffic-with-azure-application-gateway---azure-cli"></a>Hızlı Başlangıç: Azure Application Gateway - Azure CLI ile doğrudan web trafiği

Bu hızlı başlangıçta bir uygulama ağ geçidi oluşturmak için Azure CLI'yi kullanma gösterilmektedir.  Uygulama ağ geçidi oluşturduktan sonra düzgün çalıştığından emin olmak için test edin. Azure Application Gateway ile bağlantı noktalarına dinleyicileri atama, kuralları oluşturma ve arka uç havuzu için kaynak ekleme, uygulama web trafiği belirli kaynaklara doğrudan. Basitleştirmek amacıyla, bu makalede bir genel ön uç IP ile basit bir Kurulum, konağa tek bir sitede bu uygulama ağ geçidinde temel dinleyiciyi arka uç havuzunu ve temel istek yönlendirme kuralı için kullanılan iki sanal makine kullanılmaktadır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Önkoşullar

### <a name="azure-cli"></a>Azure CLI

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz Azure CLI 2.0.4 sürüm çalıştırın veya üzeri. Sürümü bulmak için çalıştırın **az--version**. Yükleme veya yükseltme hakkında daha fazla bilgi için bkz: [Azure CLI yükleme]( /cli/azure/install-azure-cli).

### <a name="resource-group"></a>Kaynak grubu

Azure'da, bir kaynak grubu için ilgili kaynakları ayırın. Kullanarak bir kaynak grubu oluşturma [az grubu oluşturma](/cli/azure/group#az-group-create). 

Aşağıdaki örnek *eastus* konumunda *myResourceGroupAG* adlı bir kaynak grubu oluşturur.

```azurecli-interactive 
az group create --name myResourceGroupAG --location eastus
```

### <a name="required-network-resources"></a>Gerekli ağ kaynakları 

Oluşturduğunuz kaynaklar arasında iletişim kurmak Azure için sanal ağ gerekir.  Uygulama ağ geçidi alt ağı, yalnızca uygulama ağ geçitleri içerebilir. Başka kaynaklar izin verilir.  Application Gateway için yeni bir alt ağ oluşturun veya var olanı kullanın. Bu örnekte, bu örnekte iki alt ağ oluşturun: bir uygulama ağ geçidi ve diğeri arka uç sunucuları için. Kullanım Örneğinize ilişkin genel veya özel olacak şekilde uygulama ağ geçidi ön uç IP'si yapılandırabilirsiniz. Bu örnekte, genel bir ön uç IP seçeceğiz.

Sanal ağ ve alt ağ oluşturmak için kullandığınız [az ağ sanal ağ oluşturma](/cli/azure/network/vnet#az-network-vnet-create). Çalıştırma [az network public-IP oluşturma](/cli/azure/network/public-ip) genel IP adresi oluşturmak için.

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

### <a name="backend-servers"></a>Arka uç sunucuları

Arka uç ağ, sanal makine ölçek kümeleri, genel IP'ler birleştirilebilir, iç IP'ler, tam etki alanı adlarını (FQDN) ve çok kiracılı arka-Azure App Service gibi biter. Bu örnekte, Azure application gateway için arka uç sunucular olarak kullanılacak iki sanal makine oluşturun. Ayrıca Azure uygulama ağ geçidi başarıyla oluşturuldu doğrulamak için sanal makinelere IIS yüklersiniz.

#### <a name="create-two-virtual-machines"></a>İki sanal makine oluşturma

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

Kullanarak bir uygulama ağ geçidi oluşturma [az ağ application-gateway oluşturma](/cli/azure/network/application-gateway). Azure CLI ile bir uygulama ağ geçidi oluşturduğunuzda, belirttiğiniz kapasite SKU ve HTTP gibi yapılandırma bilgilerini ayarlar. Azure, özel IP adresleri ağ arabirimlerinin sonra uygulama ağ geçidinin arka uç havuzuna sunucuları olarak ekler.

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

Tarayıcıyı yenileyin, ikinci bir sanal makine adını görmeniz gerekir. Uygulama ağ geçidi başarıyla oluşturuldu ve arka uç ile başarıyla bağlanabilmek için geçerli bir yanıt doğrular.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Application gateway ile oluşturduğunuz kaynaklara artık ihtiyacınız olduğunda kullanın [az grubu Sil](/cli/azure/group#az-group-delete) kaynak grubunu kaldırmak için komutu. Kaynak grubu kaldırarak, ayrıca uygulama ağ geçidi ve tüm ilgili kaynakları kaldırın.

```azurecli-interactive 
az group delete --name myResourceGroupAG
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure CLI kullanarak bir uygulama ağ geçidi ile web trafiğini yönetme](./tutorial-manage-web-traffic-cli.md)

