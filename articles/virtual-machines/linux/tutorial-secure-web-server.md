---
title: "Bir web sunucusu ile Azure SSL sertifikalarının güvenli | Microsoft Docs"
description: "Azure'da bir Linux VM üzerinde SSL sertifikalarını NGINX web sunucusuyla güvenli öğrenin"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/17/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 4e3ad8a5c08b739d8b2c6e224db0c8f88c1893ba
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="secure-a-web-server-with-ssl-certificates-on-a-linux-virtual-machine-in-azure"></a>Bir web sunucusu ile SSL sertifikalarını azure'da bir Linux sanal makinede güvenli
Web sunucularının güvenliğini sağlamak için bir Güvenli Yuva daha sonra (SSL) sertifikası web trafiğini şifrelemek için kullanılabilir. Bu SSL sertifikalarını Azure anahtar kasası depolanabilir ve Azure'da, Linux sanal makineleri (VM'ler) sertifikalarının güvenli dağıtımlar sağlar. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Azure anahtar kasası oluşturma
> * Oluşturmak veya bir sertifika anahtar Kasası'na Karşıya Yükle
> * Bir VM oluşturun ve NGINX web sunucusu yükleme
> * Sertifika VM ekleme ve NGINX SSL bağlaması ile yapılandırma

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Yüklemek ve CLI yerel olarak kullanmak seçerseniz, Bu öğretici, Azure CLI Sürüm 2.0.4 çalıştırmasını gerektirir veya sonraki bir sürümü. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli).  


## <a name="overview"></a>Genel Bakış
Azure anahtar kasası, şifreleme anahtarlarını ve gizli anahtarları, bu tür sertifikalar veya parolaları koruma sağlar. Anahtar kasası, sertifika yönetimi işlemini kolaylaştırmaya yardımcı olur ve bu sertifikaları erişim anahtarlarını denetim kurmanızı sağlar. Anahtar kasası içinde otomatik olarak imzalanan bir sertifika oluşturun veya zaten sahip olduğunuz bir varolan, güvenilen bir sertifika yükleyin.

Sertifikaları içeren özel bir VM görüntüsü kullanarak yerine desteklenmiş, sertifikaları çalışan VM ekleme. Bu işlem en güncel sertifikaları dağıtımı sırasında bir web sunucusunda yüklü olan sağlar. Yenileme veya bir sertifikayı değiştirin, yeni bir özel VM görüntüsü oluşturmanız gerekmez. Ek sanal makineleri oluştururken en son sertifikaları otomatik olarak eklenmiş. Tüm işlem sırasında sertifikalar hiçbir zaman Azure platformu bırakın veya bir komut dosyası, komut satırı geçmiş veya şablonda sunulur.


## <a name="create-an-azure-key-vault"></a>Azure anahtar kasası oluşturma
Bir anahtar kasası ve sertifikaları oluşturmadan önce bir kaynak grubuyla oluşturmanız [az grubu oluşturma](/cli/azure/group#create). Aşağıdaki örnek, bir kaynak grubu oluşturur *myResourceGroupSecureWeb* içinde *eastus* konumu:

```azurecli-interactive 
az group create --name myResourceGroupSecureWeb --location eastus
```

Ardından, bir anahtar kasası ile oluşturma [az keyvault oluşturma](/cli/azure/keyvault#create) ve bir VM dağıttığınızda kullanım için etkinleştirin. Her anahtar kasası benzersiz bir ad gerektirir ve tüm küçük olmalıdır. Değiştir  *<mykeyvault>*  aşağıdaki örnekte kendi benzersiz bir anahtar kasası ad ile:

```azurecli-interactive 
keyvault_name=<mykeyvault>
az keyvault create \
    --resource-group myResourceGroupSecureWeb \
    --name $keyvault_name \
    --enabled-for-deployment
```

## <a name="generate-a-certificate-and-store-in-key-vault"></a>Bir sertifika oluşturmak ve anahtar kasasına depolamak
Üretim kullanımı için güvenilen bir sağlayıcı tarafından imzalanmış geçerli bir sertifika almanız gerekir [az keyvault sertifika alma](/cli/azure/certificate#import). Bu öğretici için aşağıdaki örnek, otomatik olarak imzalanan bir sertifika ile nasıl oluşturabileceğiniz gösterir [az keyvault sertifika oluştur](/cli/azure/certificate#create) varsayılan sertifika ilkesi kullanır:

```azurecli-interactive 
az keyvault certificate create \
    --vault-name $keyvault_name \
    --name mycert \
    --policy "$(az keyvault certificate get-default-policy)"
```

### <a name="prepare-a-certificate-for-use-with-a-vm"></a>Bir VM ile kullanılmak üzere bir sertifika hazırlama
VM sırasında sertifikayı kullanmak üzere işlem oluşturma, sertifikanızla kimliği elde [az keyvault gizli listesi sürümleri](/cli/azure/keyvault/secret#list-versions). Sertifikayla Dönüştür [az vm biçimi-gizli](/cli/azure/vm#format-secret). Aşağıdaki örnek sonraki adımlarda kullanım kolaylığı için değişkenleri bu komutların çıktısı atar:

```azurecli-interactive 
secret=$(az keyvault secret list-versions \
          --vault-name $keyvault_name \
          --name mycert \
          --query "[?attributes.enabled].id" --output tsv)
vm_secret=$(az vm format-secret --secret "$secret")
```

### <a name="create-a-cloud-init-config-to-secure-nginx"></a>NGINX güvenli hale getirmek için bir bulut init yapılandırması oluştur
[Bulut init](https://cloudinit.readthedocs.io) ilk kez önyükleme gibi bir Linux VM özelleştirmek için yaygın olarak kullanılan bir yaklaşımdır. Bulut init paketleri yüklemek ve dosyaları yazma veya kullanıcılar ve güvenlik yapılandırmak için kullanabilirsiniz. Bulut init ilk önyükleme işlemi sırasında çalışırken, ek adımlar veya yapılandırmanızı uygulamak için gerekli aracıların yok.

VM, sertifikaları oluşturduğunuzda ve anahtarlar depolanır korumalı içinde */var/lib/waagent/* dizin. Sertifika VM ekleme ve web sunucusu yapılandırma otomatikleştirmek için bulut init kullanın. Bu örnekte, yükleyin ve NGINX web sunucusu yapılandırın. Aynı işlem yüklemek ve Apache yapılandırmak için kullanabilirsiniz. 

Adlı bir dosya oluşturun *bulut-init-web-server.txt* ve aşağıdaki yapılandırma yapıştırın:

```yaml
#cloud-config
package_upgrade: true
packages:
  - nginx
write_files:
  - owner: www-data:www-data
  - path: /etc/nginx/sites-available/default
    content: |
      server {
        listen 443 ssl;
        ssl_certificate /etc/nginx/ssl/mycert.cert;
        ssl_certificate_key /etc/nginx/ssl/mycert.prv;
      }
runcmd:
  - secretsname=$(find /var/lib/waagent/ -name "*.prv" | cut -c -57)
  - mkdir /etc/nginx/ssl
  - cp $secretsname.crt /etc/nginx/ssl/mycert.cert
  - cp $secretsname.prv /etc/nginx/ssl/mycert.prv
  - service nginx restart
```

### <a name="create-a-secure-vm"></a>Güvenli bir VM oluşturma
Şimdi bir VM oluşturmak [az vm oluşturma](/cli/azure/vm#create). Sertifika verileri ile anahtar Kasası'ndan eklenen `--secrets` parametresi. Bulut init config ile geçirdiğiniz `--custom-data` parametre:

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroupSecureWeb \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init-web-server.txt \
    --secrets "$vm_secret"
```

Oluşturulacak VM, yüklemek için paketleri ve uygulamayı başlatmak için birkaç dakika sürer. VM oluşturduğunuzda not edin `publicIpAddress` Azure CLI tarafından görüntülenir. Bu adresi bir web tarayıcısında, siteye erişmek için kullanılır.

VM ulaşmak güvenli web trafiği izin vermek için 443 numaralı bağlantı noktasını Internet'ten gelen ile açmayı [az vm Aç-port](/cli/azure/vm#open-port):

```azurecli-interactive 
az vm open-port \
    --resource-group myResourceGroupSecureWeb \
    --name myVM \
    --port 443
```


### <a name="test-the-secure-web-app"></a>Güvenli web uygulamayı test etme
Bir web tarayıcısı açın ve girin artık *https://<publicIpAddress>*  adres çubuğundaki. Kendi ortak sağlamak VM IP adresinden oluşturma işlemi. Kendinden imzalı bir sertifika kullanılması durumunda güvenlik uyarısı kabul edin:

![Web tarayıcısı güvenlik uyarısını kabul edin](./media/tutorial-secure-web-server/browser-warning.png)

Güvenli NGINX sitenizi sonra aşağıdaki örnekte olduğu gibi görüntülenir:

![Çalışan güvenli NGINX sitesini görüntüle](./media/tutorial-secure-web-server/secured-nginx.png)


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure anahtar kasasında depolanan bir SSL sertifikası ile bir NGINX web sunucusu güvenli. Şunları öğrendiniz:

> [!div class="checklist"]
> * Azure anahtar kasası oluşturma
> * Oluşturmak veya bir sertifika anahtar Kasası'na Karşıya Yükle
> * Bir VM oluşturun ve NGINX web sunucusu yükleme
> * Sertifika VM ekleme ve NGINX SSL bağlaması ile yapılandırma

Önceden derlenmiş sanal makine kod örnekleri görmek için bu bağlantıyı izleyin.

> [!div class="nextstepaction"]
> [Windows sanal makine kod örnekleri](./cli-samples.md)