---
title: "Jenkins ile azure'da geliştirme işlem hattı oluşturma | Microsoft Docs"
description: "Her kod tamamlama Github'dan çeker ve uygulamanızı çalıştırmak için yeni bir Docker kapsayıcısı derlemeler Azure'da Jenkins sanal makine oluşturma hakkında bilgi edinin"
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
ms.date: 12/15/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: d73599164589d672d6d6cde57e4a5b40774aca19
ms.sourcegitcommit: c87e036fe898318487ea8df31b13b328985ce0e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2017
---
# <a name="how-to-create-a-development-infrastructure-on-a-linux-vm-in-azure-with-jenkins-github-and-docker"></a>Jenkins, GitHub ve Docker ile azure'da bir Linux VM üzerinde bir geliştirme altyapısı oluşturma
Uygulama geliştirme, derleme ve test aşaması otomatikleştirmek için sürekli tümleştirme ve dağıtım (CI/CD) ardışık düzen kullanabilirsiniz. Bu öğreticide, Azure VM temelinde CI/CD işlem hattı oluşturmak için nasıl dahil:

> [!div class="checklist"]
> * Jenkins VM oluşturma
> * Yükleme ve Jenkins yapılandırma
> * GitHub ile Jenkins arasında Web kancası tümleştirme oluşturma
> * Oluşturma ve GitHub yürütme Jenkins yapı işlerden tetikler
> * Uygulamanız için Docker görüntü oluşturma
> * Yeni Docker görüntü ve uygulamayı çalıştıran güncelleştirmeleri GitHub yürütme yapı doğrulayın


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Yüklemek ve CLI yerel olarak kullanmak seçerseniz, Bu öğretici, Azure CLI Sürüm 2.0.22 çalıştırmasını gerektirir veya sonraki bir sürümü. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

## <a name="create-jenkins-instance"></a>Jenkins örneği oluşturma
Önceki bir öğretici içinde [Linux sanal bir makinede ilk önyükleme özelleştirmek nasıl](tutorial-automate-vm-deployment.md), bulut init VM özelleştirmesiyle otomatikleştirmek öğrendiniz. Bu öğretici bir bulut init dosya Jenkins ve Docker bir VM yüklemek için kullanır. Jenkins sürekli tümleştirme (CI) ve kesintisiz teslim (CD) etkinleştirmek için Azure ile sorunsuz bir şekilde tümleşen bir popüler açık kaynak Otomasyon sunucusudur. Jenkins kullanma hakkında daha fazla öğretici için bkz: [Jenkins Azure hub'ında](https://docs.microsoft.com/azure/jenkins/).

Geçerli kabuğunuzu adlı bir dosya oluşturun *bulut init jenkins.txt* ve aşağıdaki yapılandırma yapıştırın. Örneğin, yerel makinenizde olmayan bulut kabuğunda dosyası oluşturun. Girin `sensible-editor cloud-init-jenkins.txt` dosyası oluşturun ve kullanılabilir düzenleyicileri listesini görmek için. Tüm bulut init dosyanın doğru şekilde kopyalandığından emin olun özellikle ilk satırı:

```yaml
#cloud-config
package_upgrade: true
write_files:
  - path: /etc/systemd/system/docker.service.d/docker.conf
    content: |
      [Service]
        ExecStart=
        ExecStart=/usr/bin/dockerd
  - path: /etc/docker/daemon.json
    content: |
      {
        "hosts": ["fd://","tcp://127.0.0.1:2375"]
      }
runcmd:
  - wget -q -O - https://jenkins-ci.org/debian/jenkins-ci.org.key | apt-key add -
  - sh -c 'echo deb http://pkg.jenkins-ci.org/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
  - apt-get update && apt-get install jenkins -y
  - curl -sSL https://get.docker.com/ | sh
  - usermod -aG docker azureuser
  - usermod -aG docker jenkins
  - service jenkins restart
```

Bir VM oluşturmadan önce bir kaynak grubuyla oluşturmanız [az grubu oluşturma](/cli/azure/group#create). Aşağıdaki örnek, bir kaynak grubu oluşturur *myResourceGroupJenkins* içinde *eastus* konumu:

```azurecli-interactive 
az group create --name myResourceGroupJenkins --location eastus
```

Şimdi bir VM oluşturmak [az vm oluşturma](/cli/azure/vm#create). Kullanım `--custom-data` bulut init yapılandırma dosyanızda geçirmek için parametre. Tam yolunu belirtmeniz *bulut init jenkins.txt* mevcut çalışma dizininizi dışında dosyasını kaydettiyseniz.

```azurecli-interactive 
az vm create --resource-group myResourceGroupJenkins \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init-jenkins.txt
```

Oluşturulan ve yapılandırılacak VM için birkaç dakika sürer.

VM ulaşmak web trafiğine izin vermek için kullanın [az vm Aç-port](/cli/azure/vm#open-port) bağlantı noktasını açmak için *8080* Jenkins trafiği ve bağlantı noktası için *1337* bir örnek uygulamayı çalıştırmak için kullanılan Node.js uygulaması için:

```azurecli-interactive 
az vm open-port --resource-group myResourceGroupJenkins --name myVM --port 8080 --priority 1001
az vm open-port --resource-group myResourceGroupJenkins --name myVM --port 1337 --priority 1002
```


## <a name="configure-jenkins"></a>Jenkins yapılandırın
Jenkins örneğinizi erişmek için VM genel IP adresi alın:

```azurecli-interactive 
az vm show --resource-group myResourceGroupJenkins --name myVM -d --query [publicIps] --o tsv
```

Güvenlik nedeniyle, Jenkins yüklemeyi başlatmak için VM üzerindeki bir metin dosyasında depolanan ilk yönetici parolasını girmeniz gerekir. SSH, VM için önceki adımda elde edilen ortak IP adresi kullanın:

```bash
ssh azureuser@<publicIps>
```

Görünüm `initialAdminPassword` , Jenkins yükleyin ve onu kopyalamak için:

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Dosya henüz kullanılabilir durumda değilse, Jenkins ve Docker yüklemeyi tamamlamak bulut başlatma birkaç dakika daha bekleyin.

Şimdi bir web tarayıcısı açın ve gidin `http://<publicIps>:8080`. İlk Jenkins Kurulum aşağıdaki gibi tamamlayın:

- Kullanıcı adı girin **yönetici**, ardından sağlayın *initialAdminPassword* VM önceki adımda elde.
- Seçin **yönetmek Jenkins**, ardından **eklentileri yönetme**.
- Seçin **kullanılabilir**, ardından arama *GitHub* üstte metin kutusuna. Onay kutusunu için *GitHub eklentisi*seçeneğini belirleyip **şimdi yükleyip yeniden başlatma sonrasında**.
- Onay kutusunu için **yeniden yükleme tamamlandığında ve herhangi bir iş çalışmadığı zaman Jenkins**, eklenti yükleyene kadar işlem bekleme tamamlandıktan sonra.


## <a name="create-github-webhook"></a>GitHub Web kancası oluşturma
GitHub ile tümleştirme yapılandırmak için açın [Node.js Hello World örnek uygulaması](https://github.com/Azure-Samples/nodejs-docs-hello-world) Azure örneklerinden depoyu gelen. Kendi GitHub hesabınızda depoyu çatallaştırma için seçin **çatalı** sağ üst köşesindeki düğmesi.

Oluşturduğunuz çatalı içinde bir Web kancası oluşturun:

- Seçin **ayarları**seçeneğini belirleyip **tümleştirmeler & Hizmetleri** sol taraftaki.
- Seçin **Hizmet Ekle**, enter *Jenkins* filtre kutusuna.
- Seçin *Jenkins (GitHub eklentisi)*
- İçin **Jenkins kanca URL**, girin `http://<publicIps>:8080/github-webhook/`. Sondaki eklediğinizden emin olun /
- Seçin **Hizmet Ekle**

![GitHub Web kancası, forked deposuna ekleme](media/tutorial-jenkins-github-docker-cicd/github_webhook.png)


## <a name="create-jenkins-job"></a>Jenkins işi oluşturma
Bir olay Jenkins yanıt kodu yürütme gibi Github'da sağlamak için Jenkins işi oluşturun. 

Jenkins sitenizi seçin **yeni işleri oluşturmak** giriş sayfasından:

- Girin *HelloWorld* iş adı olarak. Seçin **Serbest stilde proje**seçeneğini belirleyip **Tamam**.
- Altında **genel** bölümünde, select **GitHub** proje ve çatallanmış depodaki URL'nizi girin *https://github.com/iainfoulds/nodejs-docs-hello-world*
- Altında **kaynak kodu Yönetimi** bölümünde, select **Git**, forked depodaki girin *.git* URL gibi *https://github.com/iainfoulds/nodejs-docs-hello-world.git*
- Altında **yapı tetikleyicileri** bölümünde, select **GITscm yoklama için GitHub kanca tetikleyici**.
- Altında **yapı** bölümünde, seçin **Ekle derleme adımı**. Seçin **Kabuk yürütme**, enter `echo "Testing"` içinde komut penceresine.
- Seçin **kaydetmek** işleri pencerenin altındaki.


## <a name="test-github-integration"></a>Test GitHub tümleştirme
Jenkins GitHub tümleşme test etmek için bir değişiklik, çatalı uygulayın. 

Geri Github'da web kullanıcı Arabirimi, forked depoyu seçin ve ardından **index.js** dosya. Satır 6 okunacak şekilde bu dosyayı düzenlemek için Kurşun Kalem simgesini seçin:

```nodejs
response.end("Hello World!");
```

Değişikliklerinizi uygulamak için seçin **değişiklikleri** altındaki düğmesini.

Jenkins içinde altında yeni bir yapı başlatır **yapı geçmiş** iş sayfanızı sol alt köşesindeki bölümü. Yapı numarası bağlantısına seçip **konsol çıkış** sol boyutu. Jenkins alan, kodunuzun Github'dan çekildiğinde adımları görüntüleyebilir ve yapı eylemi ileti çıkaran `Testing` Konsolu'na. Github'da, her bir işleme yapıldığında Web kancası için Jenkins ulaştığında ve bu şekilde yeni bir derlemeyi tetiklemeyi.


## <a name="define-docker-build-image"></a>Docker derleme görüntüyü tanımlayın
GitHub yürütme üzerinde göre çalışan Node.js uygulaması görmenizi sağlar uygulamayı çalıştırmak için Docker görüntüsünü oluşturabilirsiniz. Görüntü uygulamanın çalıştığı kapsayıcı yapılandırma tanımlayan bir Dockerfile yerleşik olarak bulunur. 

Bir önceki adımda oluşturduğunuz iş sonra adlı Jenkins çalışma dizini için VM SSH bağlantısı değiştirin. Bu örnekte, adlandırılması *HelloWorld*.

```bash
cd /var/lib/jenkins/workspace/HelloWorld
```

Bu çalışma alanı dizindeki bir dosya oluşturmak `sudo sensible-editor Dockerfile` ve aşağıdaki içeriğini yapıştırın. Tüm Dockerfile doğru şekilde kopyalandığından emin olun özellikle ilk satırı:

```yaml
FROM node:alpine

EXPOSE 1337

WORKDIR /var/www
COPY package.json /var/www/
RUN npm install
COPY index.js /var/www/
```

Alpine Linux kullanarak temel Node.js görüntü, Hello World uygulama çalıştıran çıkarır bağlantı noktası 1337 sonra uygulama dosyalarını kopyalar ve onu başlatır bu Dockerfile kullanır.


## <a name="create-jenkins-build-rules"></a>Jenkins derleme kuralları oluşturma
Önceki adımda, bir ileti konsola çıktı temel bir Jenkins yapı kuralı oluşturuldu. Bizim Dockerfile kullanın ve uygulamayı çalıştırmak için derleme adımı oluşturmak olanak sağlar.

Geri Jenkins örneğinizi içinde bir önceki adımda oluşturduğunuz işi seçin. Seçin **yapılandırma** aşağı kaydırın ve sol taraftaki **yapı** bölümü:

- Var olan kaldırmak `echo "Test"` adım oluşturma. Varolan derleme adımı kutusu sağ üst köşesinde bulunan kırmızı çarpı seçin.
- Seçin **Ekle derleme adımı**seçeneğini belirleyip **Kabuk yürütme**
- İçinde **komutu** kutusunda, aşağıdaki Docker komutları girin, ardından seçin **kaydetmek**:

  ```bash
  docker build --tag helloworld:$BUILD_NUMBER .
  docker stop helloworld && docker rm helloworld
  docker run --name helloworld -p 1337:1337 helloworld:$BUILD_NUMBER node /var/www/index.js &
  ```

Docker derleme adımları imaj ve Jenkins ile yapı numarası görüntüleri bir geçmişini sağlamak için etiketi oluşturun. Uygulamayı çalıştıran herhangi bir varolan kapsayıcıdaki durdurulur ve sonra kaldırılır. Yeni bir kapsayıcı görüntüsünü kullanarak başlatılır ve Node.js uygulamanızı github'da son işlemeleri göre çalıştırır.


## <a name="test-your-pipeline"></a>İşlem hattınızı test
Eylem tüm ardışık görmek için düzenleme *index.js* , forked GitHub deposuna dosya yeniden ve seçin **Commit değişiklik**. Jenkins için GitHub Web kancası üzerinde dayalı yeni bir iş başlatır. Docker görüntü oluşturma ve yeni bir kapsayıcı uygulamanızı başlatmak için birkaç saniye sürer.

Gerekirse, VM genel IP adresi yeniden alın:

```azurecli-interactive 
az vm show --resource-group myResourceGroupJenkins --name myVM -d --query [publicIps] --o tsv
```

Bir web tarayıcısı açın ve girin `http://<publicIps>:1337`. Node.js uygulamanızı görüntülenir ve en son işlemeleri GitHub çatalı içinde aşağıdaki gibi yansıtır:

![Çalışan Node.js uygulaması](media/tutorial-jenkins-github-docker-cicd/running_nodejs_app.png)

Artık başka bir düzenleme yapın *index.js* dosya Github'da ve değişikliği uygulayın. İş Jenkins içinde tamamlanması birkaç saniye bekleyin, sonra da yeni bir kapsayıcı şu şekilde çalışan, uygulamanın güncelleştirilmiş sürümünü görmek için web tarayıcınızı yenileyin:

![Başka bir GitHub yürütme sonrasında çalışan Node.js uygulaması](media/tutorial-jenkins-github-docker-cicd/another_running_nodejs_app.png)


## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, bir Jenkins yapı her kod tamamlama çalıştırın ve uygulamanızı test etmek için Docker kapsayıcısı dağıtmak için GitHub yapılandırılmış. Şunları öğrendiniz:

> [!div class="checklist"]
> * Jenkins VM oluşturma
> * Yükleme ve Jenkins yapılandırma
> * GitHub ile Jenkins arasında Web kancası tümleştirme oluşturma
> * Oluşturma ve GitHub yürütme Jenkins yapı işlerden tetikler
> * Uygulamanız için Docker görüntü oluşturma
> * Yeni Docker görüntü ve uygulamayı çalıştıran güncelleştirmeleri GitHub yürütme yapı doğrulayın

Jenkins Visual Studio Team Services ile tümleştirme hakkında daha fazla bilgi için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Jenkins ve Team Services ile uygulamaları dağıtma](tutorial-build-deploy-jenkins.md)