---
title: Azure Dev Spaces ile CI/CD kullanma
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
author: DrEsteban
ms.author: stevenry
ms.date: 12/17/2018
ms.topic: conceptual
manager: gwallace
description: Azure’da kapsayıcılar ve mikro hizmetlerle hızlı Kubernetes geliştirme
keywords: Docker, Kubernetes, Azure, AKS, Azure Container Service, kapsayıcılar
ms.openlocfilehash: 01e1401c5054eb56d4e2313b5e03ce5a36d1b301
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67704076"
---
# <a name="use-cicd-with-azure-dev-spaces"></a>Azure geliştirme alanları ile CI/CD kullanın

Bu makalede, sürekli tümleştirme/sürekli dağıtım (CI/CD) geliştirme etkin alanları ile Azure Kubernetes Service (AKS) için kurma size yol gösterir. AKS için CI/CD taahhüt kod kaynak deponuza itildiği otomatik olarak dağıtılacak uygulama güncelleştirmeleri sağlar. Temel bir uygulama ile çalışmak takım için güncel tutmak CI/CD geliştirme alanları ile birlikte kullanarak etkin küme kullanışlıdır.

![Örnek CI/CD diyagramı](../media/common/ci-cd-simple.png)

Bu makalede, Azure DevOps ile size kılavuzluk eder ancak aynı kavramlar Jenkins, TeamCity, vb. gibi CI/CD sistemlerine uygular.

## <a name="prerequisites"></a>Önkoşullar
* [Azure geliştirme etkin alanları ile Azure Kubernetes Service (AKS) kümesi](../get-started-netcore.md)
* [Azure geliştirme alanları CLI](upgrade-tools.md)
* [Bir proje olan Azure DevOps kuruluşu](https://docs.microsoft.com/azure/devops/user-guide/sign-up-invite-teammates?view=vsts)
* [Azure Container Registry (ACR)](../../container-registry/container-registry-get-started-azure-cli.md)
    * Azure Container Registry [yönetici hesabı](../../container-registry/container-registry-authentication.md#admin-account) ayrıntı yok
* [Azure kapsayıcı kayıt defterinizden çekmek için AKS kümenizi Yetkilendir](../../container-registry/container-registry-auth-aks.md)

## <a name="download-sample-code"></a>Örnek kodu indirin
Zaman için örnek kod GitHub depomuzda deponun bir çatalını oluşturalım. Git https://github.com/Azure/dev-spaces seçip **çatal**. İşlem çatalı tamamlandıktan sonra **kopya** yerel depo çatalı sürümünüz. Varsayılan olarak _ana_ dal işaretli, ancak bazı zamandan tasarruf değişiklikler ekledik _azds_updates_ dalı ayrıca çatalınızla sırasında aktarıldı. _Azds_updates_ dal güncelleştirmeleri el ile CI/CD sisteminin dağıtımını kolaylaştırarak bazı önceden yapılmış YAML ve JSON dosyaları yanı sıra geliştirme alanları Öğretici bölüm yapmanızı isteriz içerir. Gibi bir komut kullanabilirsiniz `git checkout -b azds_updates origin/azds_updates` kullanıma almaya _azds_updates_ yerel deponuzda dal.

## <a name="dev-spaces-setup"></a>Geliştirme alanları Kurulum
Adlı yeni bir alan oluşturabilirsiniz _geliştirme_ kullanarak `azds space select` komutu. _Geliştirme_ alanı kod değişikliklerinizi gönderme için CI/CD işlem hattınızı tarafından kullanılır. Oluşturmak için de kullanılacak _alt alanları_ göre _geliştirme_.

```cmd
azds space select -n dev
```

Bir üst geliştirme alanı seçmeniz istendiğinde seçin  _\<hiçbiri\>_ .

Geliştirme alanınız oluşturulduktan sonra ana bilgisayar soneki belirlemeniz gerekir. Kullanım `azds show-context` Azure geliştirme alanları giriş denetleyicisinin ana bilgisayar soneki gösterilecek komutu.

```cmd
$ azds show-context
Name   ResourceGroup    DevSpace  HostSuffix
-----  ---------------  --------  ----------------------
MyAKS  MyResourceGroup  dev       fedcba098.eus.azds.io
```

Yukarıdaki örnekte, ana bilgisayar soneki olan _fedcba098.eus.azds.io_. Bu değer daha sonra yayın tanımı oluşturulurken kullanılır.

_Geliştirme_ geliştiriciler oluşturabilmeniz alanı, temel havuzun en son durumu her zaman içerir _alt alanları_ gelen _geliştirme_ kendi yalıtılmış değişiklikleri test etmek için daha büyük uygulama bağlamında. Bu kavram, geliştirme alanları öğreticilerde daha ayrıntılı olarak ele alınmıştır.

## <a name="creating-the-build-definition"></a>Derleme tanımı oluşturma
Azure DevOps takım projenizi bir web tarayıcısı açın ve gidin _işlem hatları_ bölümü. İlk olarak, profil fotoğrafınız Azure DevOps sitesinin sağ üst kısımdaki'a tıklayın, Önizleme Özellikler bölmesini açın ve devre dışı _yeni YAML işlem hattı oluşturma deneyimini_:

![Açılış Önizleme özellikleri bölmesi](../media/common/preview-feature-open.png)

Devre dışı bırakma seçeneği:

![Yeni YAML işlem hattı oluşturma seçeneği deneyimi](../media/common/yaml-pipeline-preview-feature.png)

> [!Note]
> Azure DevOps _yeni YAML işlem hattı oluşturma deneyimini_ önizleme özelliği çakışmalarını önceden tanımlanmış oluştururken şu anda işlem hatları oluşturun. Şimdilik, önceden tanımlanmış bir yapı işlem hattını dağıtmak için devre dışı bırakmak gerekir.

İçinde _azds_updates_ eklediğimiz basit bir dal [Azure işlem hattı YAML](https://docs.microsoft.com/azure/devops/pipelines/yaml-schema?view=vsts&tabs=schema) için gerekli olan derleme adımları tanımlar *mywebapi* ve *webfrontend* .

Seçtiğiniz dile bağlı olarak, benzer bir yolda iade YAML işlem hattı olmuştur: `samples/dotnetcore/getting-started/azure-pipelines.dotnetcore.yml`

Bu dosyadan bir işlem hattı oluşturmak için:
1. DevOps proje ana sayfanızda, işlem hatlarına gidin > oluşturur.
1. Oluşturma seçeneğini bir **yeni** işlem hattı oluşturma.
1. Seçin **GitHub** gerekli ve seçin, kaynak olarak GitHub hesabınızı yetki _azds_updates_ geliştirme alanları örnek uygulaması depo çatalı sürümünüz daldan.
1. Seçin **yapılandırmayı kod olarak**, veya **YAML**, şablonunuzu olarak.
1. Artık, derleme işlem hattı için bir yapılandırma sayfasıyla da sunulur. Gezinmek için dile özgü yolunu yukarıda belirtildiği gibi **YAML dosyası yolu** kullanarak **...**  düğmesi. Örneğin: `samples/dotnetcore/getting-started/azure-pipelines.dotnet.yml`.
1. Git **değişkenleri** sekmesi.
1. El ile eklemeniz _dockerId_ bir değişken olarak olan kullanıcı adını, [Azure Container Registry yönetici hesabı](../../container-registry/container-registry-authentication.md#admin-account). (Makale önkoşullarda belirtildiği)
1. El ile eklemeniz _dockerPassword_ bir değişken olarak olduğu parolasını, [Azure Container Registry yönetici hesabı](../../container-registry/container-registry-authentication.md#admin-account). Belirttiğinizden emin olun _dockerPassword_ (kilit simgesini seçerek) bir gizli dizi olarak güvenlik amacıyla.
1. Seçin **Kaydet ve kuyruğa**.

Artık otomatik olarak oluşturacak bir CI çözüm sahip *mywebapi* ve *webfrontend* gönderiliyor herhangi bir güncelleştirme için _azds_updates_ çatalınızın GitHub dal. Docker görüntülerini gönderilen Azure portalında gezinme, Azure Container Registry'nize seçme ve gözatma doğrulayabilirsiniz **depoları** sekmesi. Bu görüntüleri oluşturmak ve kapsayıcı kayıt defterinizde görünmesi birkaç dakika sürebilir.

![Azure Container Registry depoları](../media/common/ci-cd-images-verify.png)

## <a name="creating-the-release-definition"></a>Yayın tanımı oluşturma

1. DevOps proje ana sayfanızda, işlem hatlarına gidin > yayınlar
1. Bir yayın tanımına henüz içermeyen yeni bir DevOps projesi içinde çalışıyorsanız, önce devam etmeden önce bir boş yayın tanımı oluşturmak gerekir. Var olan bir yayın tanımına sahip kadar içeri aktarma seçeneği kullanıcı Arabiriminde görüntülemez.
1. Sol tarafta, tıklayın **+ yeni** düğmesine ve ardından'a tıklayın **bir işlem hattı alma**.
1. Tıklayın **Gözat** seçip `samples/release.json` projenizden.
1. **Tamam** düğmesine tıklayın. Bildirim ardışık düzen Bölmesi ile yayın tanımı düzenleme sayfası yüklendi. Ayrıca unutmayın, yine de yapılandırılmalıdır kümeye özgü ayrıntıları gösteren bazı kırmızı uyarı simgeleri olur.
1. Ardışık Düzen bölmesi sol tarafta tıklayın **bir yapıt ekleme** Kabarcık.
1. İçinde **kaynak** açılır menüsünde, yapı seçin, daha önce oluşturduğunuz kanal.
1. İçin **varsayılan sürüm**, seçin **etiketlerle derleme işlem hattı varsayılan daldan en son**.
1. Bırakın **etiketleri** boş.
1. Ayarlama **kaynak diğer adı** için `drop`. **Kaynak diğer adı** değeri, önceden tanımlanmış sürüm görevler tarafından kullanılır, ayarlamanız gerekir.
1.           **Ekle**'yi tıklatın.
1. Şimdi yeni oluşturulan ışık Şimşek simgesine tıklayın `drop` yapıt kaynağı, aşağıda gösterildiği gibi:

    ![Yayın yapıtı sürekli dağıtım kurulumu](../media/common/release-artifact-cd-setup.png)
1. Etkinleştirme **sürekli dağıtım tetikleyicisi**.
1. Üzerine **görevleri** sonraki için sekmesinde **işlem hattı** tıklatıp _geliştirme_ düzenlemek için _geliştirme_ hazırlama görevleri.
1. Doğrulama **Azure Resource Manager** altında seçili **bağlantı türü.** ve kırmızı renkte vurgulanmış üç açılan menü denetimlerini görebilirsiniz: ![Yayın tanımı Kurulumu](../media/common/release-setup-tasks.png)
1. Azure geliştirme alanları ile kullandığınız Azure aboneliğini seçin. Tıklaymanız gerekebilir **Authorize**.
1. Azure geliştirme alanları ile kullanmakta olduğunuz küme ve kaynak grubu seçin.
1. Tıklayarak **aracı işi**.
1. Seçin **barındırılan Ubuntu 1604** altında **aracı havuzu**.
1. Üzerine **görevleri** en üstte seçiciyi _prod_ düzenlemek için _prod_ hazırlama görevleri.
1. Doğrulama **Azure Resource Manager** altında seçili **bağlantı türü.** ve Azure aboneliği, kaynak grubu ve Azure Dev alanları ile kullanmakta olduğunuz küme seçin.
1. Tıklayarak **aracı işi**.
1. Seçin **barındırılan Ubuntu 1604** altında **aracı havuzu**.
1. Tıklayın **değişkenleri** sürümünüzü değişkenleri güncelleştirmek için sekmesinde.
1. Değerini güncelleştirin **DevSpacesHostSuffix** gelen **UPDATE_ME** , ana bilgisayar soneki için. Ana bilgisayar soneki çalıştırdığınızda görüntülenen `azds show-context` önceki komutu.
1. Tıklayın **Kaydet** üst sağ ve **Tamam**.
1. Tıklayın **+ yayın** (Kaydet düğmesinin yanındaki), ve **yayınlamaya**.
1. Altında **Yapıtları**, en son derlemeden, derleme işlem hattı seçildiğini doğrulayın.
1.           **Oluştur**'a tıklayın.

Dağıtımı, otomatik sürüm işlemi hemen başlar *mywebapi* ve *webfrontend* grafikler için Kubernetes kümesi _geliştirme_ en üst düzey alanı. Azure DevOps web portalında sürümünüzün ilerleme durumunu izleyebilirsiniz:

1. Gidin **yayınlar** bölümüne **işlem hatları**.
1. Sürüm ardışık örnek uygulama için tıklayın.
1. En son sürüm adına tıklayın.
1. Üzerine **geliştirme** altında kutusunda **aşamaları** tıklatıp **günlükleri**.

Tüm görevler tamamlandığında yayını gerçekleştirilir.

> [!TIP]
> Sürümünüzü bir hata iletisi ile başarısız olursa ister *yükseltme başarısız oldu: koşul için beklenirken zaman aşımı oluştu*, kümenizdeki pod'ların inceleyerek deneyin [Kubernetes panosunu kullanarak](../../aks/kubernetes-dashboard.md). Pod'ların testlerden görürseniz hata iletileri ile başlamak istediğiniz *"azdsexample.azurecr.io/mywebapi:122" görüntü çekmek için başarısız oldu: rpc hata: kod = bilinmeyen desc arka plan programı hata yanıttan =: Alma https://azdsexample.azurecr.io/v2/mywebapi/manifests/122: yetkisiz: kimlik doğrulaması gerekli*, kümenizi, Azure Container registry'den yetkilendirilmedi nedeni olabilir. Tamamladığınızdan emin olun [yetkilendirme, Azure Container Registry'den çekmek için AKS kümenizi](../../container-registry/container-registry-auth-aks.md) önkoşul.

Artık geliştirme alanları örnek uygulamaları kendi GitHub çatalınız için bir tam otomatik CI/CD işlem hattı sahipsiniz. Her zaman, işlemeyi ve göndermeyi kod, derleme işlem hattı oluşturacak ve anında iletme *mywebapi* ve *webfrontend* özel ACR Örneğinizdeki görüntüleri. Yayın işlem hattı, her uygulama için Helm grafiği dağıtacağınız sonra _geliştirme_ geliştirme alanları özellikli kümenizdeki alanı.

## <a name="accessing-your-dev-services"></a>Erişim, _geliştirme_ Hizmetleri
Dağıtımdan sonra _geliştirme_ sürümünü *webfrontend* gibi genel bir URL ile erişilebilir: `http://dev.webfrontend.fedcba098.eus.azds.io`. Bu URL'yi çalıştırarak bulabilirsiniz `azds list-uri` komutu: 

```cmd
$ azds list-uris

Uri                                           Status
--------------------------------------------  ---------
http://dev.webfrontend.fedcba098.eus.azds.io  Available
```

## <a name="deploying-to-production"></a>Üretim dağıtımı

El ile belirli bir sürüme yükseltmek için _prod_ Bu öğreticide oluşturulan CI/CD sistemini kullanarak:
1. Gidin **yayınlar** bölümüne **işlem hatları**.
1. Sürüm ardışık örnek uygulama için tıklayın.
1. En son sürüm adına tıklayın.
1. Üzerine **prod** altında kutusunda **aşamaları** tıklatıp **Dağıt**.
    ![Üretime Yükselt](../media/common/prod-promote.png)
1. Üzerine **prod** altında yeniden kutusunda **aşamaları** tıklatıp **günlükleri**.

Tüm görevler tamamlandığında yayını gerçekleştirilir.

_Prod_ CI/CD ardışık düzen aşaması erişim sağlamak için geliştirme alanları giriş denetleyicisine yerine yük dengeleyicisini kullanır _prod_ Hizmetleri. Hizmetleri dağıtılan içinde _prod_ aşama, DNS adları yerine IP adresi olarak erişilebilir. Bir üretim ortamında, kendi DNS yapılandırmasına bağlı olarak, hizmetlerini barındırmak için kendi giriş denetleyicisi oluşturmak tercih edebilirsiniz.

IP webfrontend hizmetinin belirlemek için tıklayın **yazdırma webfrontend genel IP** günlük çıktısını genişletmek için adım. Erişim için çıkış günlüğü görüntülenen IP **webfrontend** uygulama.

```cmd
...
2019-02-25T22:53:02.3237187Z webfrontend can be accessed at http://52.170.231.44
2019-02-25T22:53:02.3320366Z ##[section]Finishing: Print webfrontend public IP
...
```

## <a name="dev-spaces-instrumentation-in-production"></a>Üretimde geliştirme alanları izleme
Geliştirme alanları izleme tasarlanmış olsa da _değil_ in the way of uygulamanızın normal işlem almak için geliştirme alanları ile etkin bir Kubernetes ad alanı, üretim iş yüklerinizin çalıştırmanızı öneririz. Bu tür bir Kubernetes ad alanı kullanmak anlamına gelir, ya da oluşturmalıdır üretim using namespace `kubectl` , CLI veya ilk Helm dağıtım sırasında oluşturmak CI/CD sisteminiz izin. _Seçme_ veya aksi halde geliştirme alanları'nı kullanarak bir alan oluşturma araçları bu ad alanına geliştirme alanları izleme ekleyeceksiniz.

'Dev' ortam özelliği geliştirmeyi destekleyen bir örnek ad alanı yapısı işte _ve_ üretim, tek bir Kubernetes kümesi içinde:

![Örnek ad alanı yapısı](../media/common/cicd-namespaces.png)

> [!Tip]
> Zaten oluşturduysanız bir `prod` alan ve yalnızca geliştirme alanları İzleme'den (silmeden!) hariç üzere gibi aşağıdaki geliştirme alanları CLI komutu ile bunu yapabilirsiniz:
>
> `azds space remove -n prod --no-delete`
>
> Tüm pod'ların silmeniz gerekebilir `prod` geliştirme alanları izleme oluşturulması için bunu yaptıktan sonra ad alanı.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure geliştirme alanları'nı kullanarak takım geliştirme hakkında bilgi edinin](../team-development-netcore.md)