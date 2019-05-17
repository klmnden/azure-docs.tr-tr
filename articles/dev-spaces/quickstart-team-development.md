---
title: Azure geliştirme alanları kullanarak Kubernetes üzerinde takım geliştirme
titleSuffix: Azure Dev Spaces
author: zr-msft
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.author: zarhoads
ms.date: 04/25/2019
ms.topic: quickstart
description: Kapsayıcılar ve azure'da mikro hizmetler ile Kubernetes geliştirme ekibi
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, kapsayıcılar, Helm, hizmet kafes, ağ hizmeti Yönlendirme, kubectl, k8s
manager: jeconnoc
ms.openlocfilehash: 94083639ca769d12b04c4dc316a9f9867e4209b1
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65765238"
---
# <a name="quickstart-team-development-on-kubernetes-using-azure-dev-spaces"></a>Hızlı Başlangıç: Azure geliştirme alanları kullanarak Kubernetes üzerinde takım geliştirme

Bu kılavuzda şunların nasıl yapıldığını öğreneceksiniz:

- Azure'da yönetilen bir Kubernetes kümesinde Azure geliştirme alanları ayarlayın.
- Birden fazla mikro hizmetin bulunan büyük uygulamaları geliştirme boşlukla dağıtın.
- Tek bir mikro hizmet, bir yalıtılmış geliştirme alanında tam uygulamasının bağlam içinde test edin.

![Azure geliştirme alanları takım geliştirme](media/azure-dev-spaces/collaborate-graphic.gif)

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz hesap](https://azure.microsoft.com/free) oluşturabilirsiniz.
- [Yüklü Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest).
- [Helm 2.13 veya üzeri yüklü](https://github.com/helm/helm/blob/master/docs/install.md).

## <a name="create-an-azure-kubernetes-service-cluster"></a>Azure Kubernetes Service kümesi oluşturma

Bir AKS kümesinde oluşturmalısınız bir [bölge desteklenen](https://docs.microsoft.com/azure/dev-spaces/#a-rapid,-iterative-kubernetes-development-experience-for-teams). Aşağıdaki komutları adlı bir kaynak grubu oluşturacaksınız *MyResourceGroup* ve adlı bir AKS kümesi *MyAKS*.

```cmd
az group create --name MyResourceGroup --location eastus
az aks create -g MyResourceGroup -n MyAKS --location eastus --node-vm-size Standard_DS2_v2 --node-count 1 --disable-rbac --generate-ssh-keys
```

*MyAKS* kümesi de tek bir düğüm, oluşturulan kullanarak *Standard_DS2_v2* boyuta ve RBAC ile devre dışı.

## <a name="enable-azure-dev-spaces-on-your-aks-cluster"></a>Azure geliştirme alanları AKS kümenizde etkinleştirme

Kullanım `use-dev-spaces` komutunu kullanarak AKS kümenizde geliştirme alanları etkinleştirme ve yönergeleri izleyin. Geliştirme alanları etkinleştirir komut *MyAKS* içinde küme *MyResourceGroup* adlı bir geliştirme alanı oluşturur ve grup *geliştirme*.

```cmd
az aks use-dev-spaces -g MyResourceGroup -n MyAKS --space dev --yes
```

## <a name="get-sample-application-code"></a>Örnek uygulama kodunu alma

Bu makalede, kullandığınız [örnek uygulaması, Azure geliştirme alanları bisiklet paylaşımı](https://github.com/Azure/dev-spaces/tree/master/samples/BikeSharingApp) Azure geliştirme alanları göstermek için.

Uygulamasını github'dan kopyalayın ve alt dizinine gidin:

```cmd
git clone https://github.com/Azure/dev-spaces
cd dev-spaces/samples/BikeSharingApp/
```

## <a name="retrieve-the-hostsuffix-for-dev"></a>Almak için HostSuffix *geliştirme*

Kullanım `azds show-context` HostSuffix için gösterilecek komut *geliştirme*.

```cmd
$ azds show-context

Name                ResourceGroup     DevSpace  HostSuffix
------------------  ----------------  --------  -----------------------
MyAKS               MyResourceGroup   dev       fedcab0987.eus.azds.io
```

## <a name="update-the-helm-chart-with-your-hostsuffix"></a>Helm grafiği, HostSuffix ile güncelleştirme

Açık [charts/values.yaml](https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/charts/values.yaml) ve tüm örneklerini `<REPLACE_ME_WITH_HOST_SUFFIX>` daha önce aldığınız HostSuffix değerine sahip. Yaptığınız değişiklikleri kaydedin ve dosyayı kapatın.

## <a name="run-the-sample-application-in-kubernetes"></a>Örnek uygulamayı Kubernetes'te çalıştırma

Azure'da Kubernetes örnek uygulamayı çalıştırmak için komutlar, mevcut bir işlemin parçası olan ve Azure geliştirme alanları araç üzerinde hiçbir bağımlılık vardır. Bu durumda, bu örnek uygulama çalıştırmak için kullanılan araçları Helm, ancak diğer araçları, bir küme içinde bir ad alanındaki tüm uygulamanızı çalıştırmak için kullanılabilir. Helm komutları adlı geliştirme alanı hedeflediğiniz *geliştirme* daha önce oluşturduğunuz ancak bu geliştirme ayrıca Kubernetes ad alanıdır. Sonuç olarak, geliştirme alanları başka diğer ad alanları ile aynı araçları tarafından hedeflenebilir.

Bir uygulamayı bir kümede dağıtılması için kullanılan araçları bağımsız olarak çalışmaya başladıktan sonra ekip geliştirme için Azure geliştirme alanları kullanabilirsiniz.

Kullanım `helm init` ve `helm install` ayarlama ve kümenizde örnek uygulamayı yüklemek için komutları.

```cmd
cd charts/
helm init --wait
helm install -n bikesharing . --dep-up --namespace dev --atomic --wait
```

`helm install` Komutun tamamlanması birkaç dakika sürebilir. Komut çıktısı tamamlandığında kümeye dağıtılan tüm hizmetlerin durumunu gösterir:

```cmd
$ cd charts/
$ helm init --wait
...
Happy Helming!

$ helm install -n bikesharing . --dep-up --namespace dev --atomic --wait

Hang tight while we grab the latest from your chart repositories...
...
NAME               READY  UP-TO-DATE  AVAILABLE  AGE
bikes              1/1    1           1          4m32s
bikesharingweb     1/1    1           1          4m32s
billing            1/1    1           1          4m32s
gateway            1/1    1           1          4m32s
reservation        1/1    1           1          4m32s
reservationengine  1/1    1           1          4m32s
users              1/1    1           1          4m32s
```

Sonra örnek, kümenizde uygulama yüklenir ve geliştirme kümenizde etkin alanları olduğundan kullanın `azds list-uris` örnek uygulama URL'lerini görüntülemek için komut *geliştirme* , o anda seçili.

```cmd
$ azds list-uris
Uri                                                 Status
--------------------------------------------------  ---------
http://dev.bikesharingweb.fedcab0987.eus.azds.io/  Available
http://dev.gateway.fedcab0987.eus.azds.io/         Available
```

Gidin *bikesharingweb* genel URL'den açarak hizmet `azds list-uris` komutu. Yukarıdaki örnekte, genel URL'si *bikesharingweb* hizmetidir `http://dev.bikesharingweb.fedcab0987.eus.azds.io/`. Seçin *Aurelia Briggs (müşteri)* kullanıcı. Metin gördüğünüz doğrulayın *yüksek Aurelia Briggs | Oturum kapatma* en üstünde.

![Azure geliştirme alanları bisiklet paylaşımı örnek uygulaması](media/quickstart-team-development/bikeshare.png)

## <a name="create-child-dev-spaces"></a>Alt geliştirme alanları oluşturma

Kullanım `azds space select` altında iki alt alanları oluşturmak için komut *geliştirme*:

```cmd
azds space select -n dev/azureuser1 -y
azds space select -n dev/azureuser2 -y
```

Yukarıdaki komutlar altında iki alt alanları oluşturmak *geliştirme* adlı *azureuser1* ve *azureuser2*. Bu iki alt alanları geliştirici için ayrı geliştirme alanları temsil *azureuser1* ve *azureuser2* örnek uygulamaya değişiklikler yapmak için kullanılacak.

Kullanım `azds space list` tüm geliştirme alanlarını listeleyin ve onaylamak için komut *dev/azureuser2* seçilir.

```cmd
$ azds space list
Name            Selected
--------------  --------
default         False
dev             False
dev/azureuser1  False
dev/azureuser2  True
```

Kullanım `azds list-uris` , seçili alanı URL'leri örnek uygulama için görüntülenecek *dev/azureuser2*.

```cmd
$ azds list-uris
Uri                                                             Status
--------------------------------------------------              ---------
http://azureuser2.s.dev.bikesharingweb.fedcab0987.eus.azds.io/  Available
http://azureuser2.s.dev.gateway.fedcab0987.eus.azds.io/         Available
```

URL'leri tarafından görüntülenen onaylayın `azds list-uris` komut *azureuser2.s.dev* önek. Bu ön ek, seçilen geçerli alan olduğunu onaylar *azureuser2*, bir alt öğesi olan *geliştirme*.

Gidin *bikesharingweb* hizmetine *dev/azureuser2* genel URL'den açarak geliştirme alanı `azds list-uris` komutu. Yukarıdaki örnekte, genel URL'si *bikesharingweb* hizmetidir `http://azureuser2.s.dev.bikesharingweb.fedcab0987.eus.azds.io/`. Seçin *Aurelia Briggs (müşteri)* kullanıcı. Metin gördüğünüz doğrulayın *yüksek Aurelia Briggs | Oturumu Kapat* en üstünde.

## <a name="update-code"></a>Kodu güncelleştirme

Açık *BikeSharingWeb/components/Header.js* bir metin düzenleyici içindeki metni değiştirip [span öğesi ile `userSignOut` className](https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/BikeSharingWeb/components/Header.js#L16).

```html
<span className="userSignOut">
    <Link href="/devsignin"><span tabIndex="0">Welcome {props.userName} | Sign out</span></Link>
</span>
```

Yaptığınız değişiklikleri kaydedin ve dosyayı kapatın.

## <a name="build-and-run-the-updated-bikesharingweb-service-in-the-devazureuser2-dev-space"></a>Derleme ve çalıştırma güncelleştirilmiş bikesharingweb hizmet *dev/azureuser2* geliştirme alanı

Gidin *BikeSharingWeb /* dizin ve çalışma `azds up` komutu.

```cmd
$ cd ../BikeSharingWeb/
$ azds up

Using dev space 'dev/azureuser2' with target 'MyAKS'
Synchronizing files...2s
...
Service 'bikesharingweb' port 'http' is available at http://azureuser2.s.dev.bikesharingweb.fedcab0987.eus.azds.io/
Service 'bikesharingweb' port 80 (http) is available at http://localhost:54256
...
```

Bu komut derlenir ve çalışır *bikesharingweb* hizmeti *dev/azureuser2* geliştirme alanı. Ek olarak bu hizmet çalışır *bikesharingweb* çalışan hizmet *geliştirme* ve yalnızca istekleri için kullanılan *azureuser2.s* URL ön eki. Yönlendirme alt ile üst arasında geliştirme alanları işleyişi hakkında daha fazla bilgi için bkz: [nasıl Azure geliştirme alanları çalışır ve olan yapılandırılmış](how-dev-spaces-works.md).

Gidin *bikesharingweb* hizmetine *dev/azureuser2* çıkışında görüntülenen genel URL açarak geliştirme alanı `azds up` komutu. Seçin *Aurelia Briggs (müşteri)* kullanıcı. Sağ üst köşedeki güncelleştirilmiş metinde gördüğünüz doğrulayın. Sayfayı yenileyin veya bu değişiklik hemen görmüyorsanız, tarayıcınızın önbelleğini temizlemeniz gerekebilir.

![Güncelleştirilen azure geliştirme alanları bisiklet paylaşımı örnek uygulaması](media/quickstart-team-development/bikeshare-update.png)

## <a name="verify-other-dev-spaces-are-unchanged"></a>Diğer geliştirme alanları değişmeden doğrulayın

Varsa `azds up` komutu hala çalışıyor, basın *Ctrl + c*.

```cmd
$ azds list-uris --all
Uri                                                             Status
--------------------------------------------------              ---------
http://azureuser1.s.dev.bikesharingweb.fedcab0987.eus.azds.io/  Available
http://azureuser1.s.dev.gateway.fedcab0987.eus.azds.io/         Available
http://azureuser2.s.dev.bikesharingweb.fedcab0987.eus.azds.io/  Available
http://azureuser2.s.dev.gateway.fedcab0987.eus.azds.io/         Available
http://dev.bikesharingweb.fedcab0987.eus.azds.io/               Available
http://dev.gateway.fedcab0987.eus.azds.io/                      Available
```

Gidin *geliştirme* sürümünü *bikesharingweb* tarayıcınızda seçin *Aurelia Briggs (müşteri)* kullanıcı olarak ve sağ üst köşedeki orijinal metni gördüğünüz doğrulayın Köşe. Bu adımları yineleyin *dev/azureuser1* URL'si. Değişikliklerin uygulandığı yalnızca bildirimi *dev/azureuser2* sürümünü *bikesharingweb*. Bu yalıtım yapılan değişikliklerin *dev/azureuser2* sağlar *azureuser2* etkilemeden değişiklik *azureuser1*.

Bu değişikliklerin yansıtılan *geliştirme* ve *dev/azureuser1*, ekibinizin mevcut iş akışı veya CI/CD işlem hattı izlemelidir. Örneğin, bu iş akışı, sürüm denetimi sisteminiz değişikliğiniz yapılıyor ve bir CI/CD işlem hattı'nı kullanarak veya Helm gibi araç güncelleştirmesi dağıtma gerektirebilir.

## <a name="clean-up-your-azure-resources"></a>Azure kaynaklarınızı temizleme

```cmd
az group delete --name MyResourceGroup --yes --no-wait
```

## <a name="next-steps"></a>Sonraki adımlar

Azure Dev Spaces uygulamasının birden fazla kapsayıcı arasında daha karmaşık uygulamalar geliştirmenize nasıl yardımcı olduğunu ve farklı alanlarda kodunuzun farklı sürümleri veya dallarıyla çalışarak işbirliğine dayalı geliştirme deneyimini nasıl kolaylaştırabildiğinizi öğrenin.

> [!div class="nextstepaction"]
> [Birden çok kapsayıcı ve takım geliştirme ile çalışma](multi-service-nodejs.md)
