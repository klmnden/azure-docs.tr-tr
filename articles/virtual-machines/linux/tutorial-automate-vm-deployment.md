---
title: "İlk önyükleme azure'da bir Linux VM özelleştirme | Microsoft Docs"
description: "Bulut Init ve anahtar kasası customze Linux VM'ler için Azure'da önyükleme ilk kez kullanmayı öğrenin"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/13/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 83773e513ee2c92da733df05cd17dda2940a28cd
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2017
---
# <a name="how-to-customize-a-linux-virtual-machine-on-first-boot"></a>İlk önyükleme Linux sanal makine özelleştirme
Bir önceki öğreticide öğrenilen nasıl NGINX SSH bir sanal makine (VM) ve el ile yükleyin. VM'ler hızlı ve tutarlı bir şekilde oluşturmak için tür Otomasyon genellikle istendiğini. İlk önyükleme bir VM özelleştirmek için ortak bir yaklaşım kullanmaktır [bulut init](https://cloudinit.readthedocs.io). Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Bir bulut init yapılandırma dosyası oluşturma
> * Bir bulut init dosyası kullanan bir VM oluşturma
> * VM oluşturulduktan sonra çalışan bir Node.js uygulaması görüntüleyin
> * Sertifikaları güvenli bir şekilde anahtar kasası kullanın
> * Güvenli NGINX dağıtımlarında bulut init otomatikleştirme


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Yüklemek ve CLI yerel olarak kullanmak seçerseniz, Bu öğretici, Azure CLI Sürüm 2.0.4 çalıştırmasını gerektirir veya sonraki bir sürümü. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli).  



## <a name="cloud-init-overview"></a>Cloud-init genel bakış
[Bulut init](https://cloudinit.readthedocs.io) ilk kez önyükleme gibi bir Linux VM özelleştirmek için yaygın olarak kullanılan bir yaklaşımdır. Bulut init paketleri yüklemek ve dosyaları yazma veya kullanıcılar ve güvenlik yapılandırmak için kullanabilirsiniz. Bulut init ilk önyükleme işlemi sırasında çalışırken, ek adımlar veya yapılandırmanızı uygulamak için gerekli aracıların yok.

Bulut init dağıtımları üzerinde de çalışır. Örneğin, kullanmadığınız **get apt yükleme** veya **yum yükleme** bir paketi yüklemek için. Bunun yerine, yüklemek için paketlerin listesini tanımlayabilirsiniz. Bulut init otomatik olarak seçtiğiniz distro için yerel paket Yönetim Aracı'nı kullanır.

Bulut dahil ve Azure'a sağladıkları görüntülerinde çalışma başlatma almak için ortaklarımızın ile çalışıyoruz. Aşağıdaki tabloda Azure platform görüntüleri geçerli bulut init kullanılabilirliğine özetlenmektedir:

| Diğer ad | Yayımcı | Sunduğu | SKU | Sürüm |
|:--- |:--- |:--- |:--- |:--- |:--- |
| UbuntuLTS |Canonical |UbuntuServer |16.04 LTS |en son |
| UbuntuLTS |Canonical |UbuntuServer |14.04.5-LTS |en son |
| CoreOS |CoreOS |CoreOS |Dengeli |en son |
| | OpenLogic | CentOS | 7 CI | en son |
| | RedHat | RHEL | 7 HAM-CI | en son


## <a name="create-cloud-init-config-file"></a>Bulut init yapılandırma dosyası oluşturma
Bulut-init uygulamada görmek için NGINX yükler ve bir basit 'Hello World' Node.js uygulaması çalıştıran bir VM oluşturun. Aşağıdaki bulut init yapılandırma gerekli paketleri yükler, bir Node.js uygulaması oluşturur sonra başlatmak ve uygulamayı başlatır.

Geçerli kabuğunuzu adlı bir dosya oluşturun *bulut init.txt* ve aşağıdaki yapılandırma yapıştırın. Örneğin, yerel makinenizde olmayan bulut kabuğunda dosyası oluşturun. İstediğiniz herhangi bir düzenleyicisini kullanabilirsiniz. Girin `sensible-editor cloud-init.txt` dosyası oluşturun ve kullanılabilir düzenleyicileri listesini görmek için. Tüm bulut init dosyanın doğru şekilde kopyalandığından emin olun özellikle ilk satırı:

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

Bulut init yapılandırma seçenekleri hakkında daha fazla bilgi için bkz: [bulut init yapılandırma örnekleri](https://cloudinit.readthedocs.io/en/latest/topics/examples.html).

## <a name="create-virtual-machine"></a>Sanal makine oluşturma
Bir VM oluşturmadan önce bir kaynak grubuyla oluşturmanız [az grubu oluşturma](/cli/azure/group#create). Aşağıdaki örnek, bir kaynak grubu oluşturur *myResourceGroupAutomate* içinde *eastus* konumu:

```azurecli-interactive 
az group create --name myResourceGroupAutomate --location eastus
```

Şimdi bir VM oluşturmak [az vm oluşturma](/cli/azure/vm#create). Kullanım `--custom-data` bulut init yapılandırma dosyanızda geçirmek için parametre. Tam yolunu belirtmeniz *bulut init.txt* mevcut çalışma dizininizi dışında dosyasını kaydettiyseniz yapılandırma. Aşağıdaki örnek, adlandırılmış bir VM'nin oluşturur *myAutomatedVM*:

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroupAutomate \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt
```

Oluşturulacak VM, yüklemek için paketleri ve uygulamayı başlatmak için birkaç dakika sürer. Azure CLI sorusu döndükten sonra çalışmaya devam arka plan görevleri vardır. Başka bir birkaç uygulamaya erişmek için dakika olabilir. VM oluşturduğunuzda not edin `publicIpAddress` Azure CLI tarafından görüntülenir. Bu adres, Node.js uygulaması bir web tarayıcısı aracılığıyla erişmek için kullanılır.

Bağlantı noktası 80 Internet'ten açın, VM ulaşmak web trafiğe izin verecek şekilde [az vm Aç-port](/cli/azure/vm#open-port):

```azurecli-interactive 
az vm open-port --port 80 --resource-group myResourceGroupAutomate --name myVM
```

## <a name="test-web-app"></a>Test web uygulaması
Bir web tarayıcısı açın ve girin artık *http://<publicIpAddress>*  adres çubuğundaki. Kendi ortak sağlamak VM IP adresinden oluşturma işlemi. Node.js uygulamanızı aşağıdaki örnekte olduğu gibi görüntülenir:

![Çalışan NGINX sitesini görüntüle](./media/tutorial-automate-vm-deployment/nginx.png)


## <a name="inject-certificates-from-key-vault"></a>Anahtar kasası sertifikaları Ekle
Bu isteğe bağlı bir bölüm nasıl güvenli bir şekilde Azure anahtar kasası sertifikaları depolamak ve VM dağıtımı sırasında Ekle gösterir. Sertifikaları içeren özel bir görüntü kullanarak yerine desteklenmiş, bu işlem en güncel sertifikaları ilk önyükleme üzerinde bir VM eklenmiş sağlar. İşlemi sırasında sertifika hiçbir zaman Azure platformu bırakır veya bir komut dosyası, komut satırı geçmiş veya şablonda sunulur.

Azure anahtar kasası, şifreleme anahtarlarını ve gizli anahtarları, sertifikalar veya parolaları gibi koruma sağlar. Anahtar kasası anahtar yönetimi işlemini kolaylaştırmaya yardımcı olur ve erişmek ve veri şifreleme anahtarları denetim kurmanızı sağlar. Bu senaryo oluşturmak ve bir sertifika kullanmak için bazı anahtar kasası kavramları tanıtır, ancak anahtar kasası kullanma hakkında ayrıntılı bir genel bakış değildir.

Aşağıdaki adımlar, nasıl gösterir:

- Azure anahtar kasası oluşturma
- Oluşturmak veya bir sertifika anahtar Kasası'na Karşıya Yükle
- Bir VM içinde eklemesine sertifikasından gizli anahtar oluşturma
- Bir VM oluşturun ve sertifika Ekle

### <a name="create-an-azure-key-vault"></a>Azure anahtar kasası oluşturma
İlk olarak, bir anahtar kasası ile oluşturma [az keyvault oluşturma](/cli/azure/keyvault#create) ve bir VM dağıttığınızda kullanım için etkinleştirin. Her anahtar kasası benzersiz bir ad gerektirir ve tüm küçük olmalıdır. Değiştir *mykeyvault* aşağıdaki örnekte kendi benzersiz bir anahtar kasası ad ile:

```azurecli-interactive 
keyvault_name=mykeyvault
az keyvault create \
    --resource-group myResourceGroupAutomate \
    --name $keyvault_name \
    --enabled-for-deployment
```

### <a name="generate-certificate-and-store-in-key-vault"></a>Sertifika oluşturmak ve anahtar kasasına depolamak
Üretim kullanımı için güvenilen bir sağlayıcı tarafından imzalanmış geçerli bir sertifika almanız gerekir [az keyvault sertifika alma](/cli/azure/keyvault/certificate#import). Bu öğretici için aşağıdaki örnek, otomatik olarak imzalanan bir sertifika ile nasıl oluşturabileceğiniz gösterir [az keyvault sertifika oluştur](/cli/azure/keyvault/certificate#create) varsayılan sertifika ilkesi kullanır:

```azurecli-interactive 
az keyvault certificate create \
    --vault-name $keyvault_name \
    --name mycert \
    --policy "$(az keyvault certificate get-default-policy)"
```


### <a name="prepare-certificate-for-use-with-vm"></a>VM ile kullanmak için sertifika hazırlama
VM sırasında sertifikayı kullanmak üzere işlem oluşturma, sertifikanızla kimliği elde [az keyvault gizli listesi sürümleri](/cli/azure/keyvault/secret#list-versions). VM önyüklemede ekleme, böylece sertifikayla dönüştürmek için belirli bir biçimde sertifikanın gerekir [az vm biçimi-gizli](/cli/azure/vm#format-secret). Aşağıdaki örnek sonraki adımlarda kullanım kolaylığı için değişkenleri bu komutların çıktısı atar:

```azurecli-interactive 
secret=$(az keyvault secret list-versions \
          --vault-name $keyvault_name \
          --name mycert \
          --query "[?attributes.enabled].id" --output tsv)
vm_secret=$(az vm format-secret --secret "$secret")
```


### <a name="create-cloud-init-config-to-secure-nginx"></a>NGINX güvenli hale getirmek için bulut init yapılandırma oluşturma
VM, sertifikaları oluşturduğunuzda ve anahtarlar depolanır korumalı içinde */var/lib/waagent/* dizin. Sertifika VM ekleme ve NGINX yapılandırma otomatikleştirmek için güncelleştirilmiş bulut init config önceki örnekten kullanabilirsiniz.

Adlı bir dosya oluşturun *bulut init secured.txt* ve aşağıdaki yapılandırma yapıştırın. Yeniden bulut Kabuğu'nu kullanırsanız, bulut init yapılandırma dosyası yok ve yerel makinenizde değil oluşturun. Kullanım `sensible-editor cloud-init-secured.txt` dosyası oluşturun ve kullanılabilir düzenleyicileri listesini görmek için. Tüm bulut init dosyanın doğru şekilde kopyalandığından emin olun özellikle ilk satırı:

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
        listen 443 ssl;
        ssl_certificate /etc/nginx/ssl/mycert.cert;
        ssl_certificate_key /etc/nginx/ssl/mycert.prv;
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
  - secretsname=$(find /var/lib/waagent/ -name "*.prv" | cut -c -57)
  - mkdir /etc/nginx/ssl
  - cp $secretsname.crt /etc/nginx/ssl/mycert.cert
  - cp $secretsname.prv /etc/nginx/ssl/mycert.prv
  - service nginx restart
  - cd "/home/azureuser/myapp"
  - npm init
  - npm install express -y
  - nodejs index.js
```

### <a name="create-secure-vm"></a>Güvenli VM oluşturma
Şimdi bir VM oluşturmak [az vm oluşturma](/cli/azure/vm#create). Sertifika verileri ile anahtar Kasası'ndan eklenen `--secrets` parametresi. Önceki örnekte olduğu gibi aynı zamanda bulut init config ile geçirdiğiniz `--custom-data` parametre:

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroupAutomate \
    --name myVMSecured \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init-secured.txt \
    --secrets "$vm_secret"
```

Oluşturulacak VM, yüklemek için paketleri ve uygulamayı başlatmak için birkaç dakika sürer. Azure CLI sorusu döndükten sonra çalışmaya devam arka plan görevleri vardır. Başka bir birkaç uygulamaya erişmek için dakika olabilir. VM oluşturduğunuzda not edin `publicIpAddress` Azure CLI tarafından görüntülenir. Bu adres, Node.js uygulaması bir web tarayıcısı aracılığıyla erişmek için kullanılır.

VM ulaşmak güvenli web trafiği izin vermek için 443 numaralı bağlantı noktasını Internet'ten gelen ile açmayı [az vm Aç-port](/cli/azure/vm#open-port):

```azurecli-interactive 
az vm open-port \
    --resource-group myResourceGroupAutomate \
    --name myVMSecured \
    --port 443
```

### <a name="test-secure-web-app"></a>Test güvenli web uygulaması
Bir web tarayıcısı açın ve girin artık *https://<publicIpAddress>*  adres çubuğundaki. Kendi ortak sağlamak VM IP adresinden oluşturma işlemi. Kendinden imzalı bir sertifika kullanılması durumunda güvenlik uyarısı kabul edin:

![Web tarayıcısı güvenlik uyarısını kabul edin](./media/tutorial-automate-vm-deployment/browser-warning.png)

Güvenli NGINX siteniz ve Node.js uygulaması ardından aşağıdaki örnekte olduğu gibi görüntülenir:

![Çalışan güvenli NGINX sitesini görüntüle](./media/tutorial-automate-vm-deployment/secured-nginx.png)


## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, VM'ler ilk önyüklemesi bulut init ile yapılandırılır. Şunları öğrendiniz:

> [!div class="checklist"]
> * Bir bulut init yapılandırma dosyası oluşturma
> * Bir bulut init dosyası kullanan bir VM oluşturma
> * VM oluşturulduktan sonra çalışan bir Node.js uygulaması görüntüleyin
> * Sertifikaları güvenli bir şekilde anahtar kasası kullanın
> * Güvenli NGINX dağıtımlarında bulut init otomatikleştirme

Özel VM görüntüleri oluşturma hakkında bilgi almak için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Özel VM görüntüleri oluşturma](./tutorial-custom-images.md)
