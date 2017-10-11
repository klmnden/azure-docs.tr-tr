---
title: "Azure kapsayıcı Hizmeti'nde Kubernetes bir yay önyükleme uygulamasını dağıtma | Microsoft Docs"
description: "Bu öğreticide, Microsoft Azure üzerinde bir Kubernetes yay önyükleme uygulamada dağıtma adımları küme olsa size yol gösterir."
services: container-service
documentationcenter: java
author: rmcmurray
manager: cfowler
editor: 
ms.assetid: 
ms.service: container-service
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 08/04/2017
ms.author: asirveda;robmcm
ms.custom: mvc
ms.openlocfilehash: 7f726436b2d459b8c16abb02e07de099abfd8974
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-a-spring-boot-application-on-a-kubernetes-cluster-in-the-azure-container-service"></a>Azure Container Service’te bir Kubernetes Kümesi üzerinde Spring Boot Uygulaması dağıtma

 **[Yay Framework]**  , Java geliştiricilerinin web, mobil ve API uygulamaları oluşturma yardımcı olan bir popüler açık kaynak çerçevedir. Bu öğretici kullanılarak oluşturulmuş bir örnek uygulama kullanır [yay önyükleme], kuralı güdümlü bir yaklaşım yay hızlıca başlamak için kullanma.

**[Kubernetes]**  ve  **[Docker]**  olan, geliştiricilerin açık kaynak çözümleri otomatikleştirmek dağıtım, ölçeklendirme ve çalışan kendi uygulamalarını Yönetimi kapsayıcı.

Bu öğreticide, geliştirmek ve Microsoft Azure yay önyükleme uygulama dağıtmak için bu iki yaygın olarak kullanılan, açık kaynaklı teknolojiler birleştirme olsa açıklanmaktadır. Daha belirgin olarak kullandığınız  *[yay önyükleme]*  uygulama geliştirme için  *[Kubernetes]*  kapsayıcı dağıtım ve [ Azure kapsayıcı Hizmeti'ni (ACS)] uygulamanızı barındırmak için.

### <a name="prerequisites"></a>Ön koşullar

* Bir Azure aboneliği; bir Azure aboneliği zaten sahip değilseniz, etkinleştirebilir, [MSDN abone Avantajlarınızı] veya kaydolun bir [ücretsiz Azure hesabı].
* [Azure komut satırı arabirimi (CLI)].
* Güncel bir [Java Geliştirme Seti (JDK)].
* Apache'nın [Maven] aracını (sürüm 3) yapılandırma.
* A [Git] istemci.
* A [Docker] istemci.

> [!NOTE]
>
> Bu öğretici sanallaştırma gereksinimleri nedeniyle, bir sanal makinede bu makaledeki adımları izleyin olamaz; sanallaştırma özellikleri etkin bir fiziksel bilgisayar kullanmanız gerekir.
>

## <a name="create-the-spring-boot-on-docker-getting-started-web-app"></a>Yay önyükleme Docker Başlarken web uygulaması oluştur

Aşağıdaki adımlar yay önyükleme web uygulaması oluşturma ve yerel olarak test size yol.

1. Bir komut istemi açın ve uygulamanızı tutun ve bu dizine değiştirmek için yerel bir dizin oluşturun; Örneğin:
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   --ya da--
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. Kopya [Docker Başlarken yay önyüklemede] örnek proje dizine.
   ```
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. Projeyi Dizin Değiştir.
   ```
   cd gs-spring-boot-docker
   cd complete
   ```

1. Derleme ve örnek uygulamayı çalıştırma için Maven kullanın.
   ```
   mvn package spring-boot:run
   ```

1. Web uygulaması http://localhost: 8080 veya aşağıdaki göz atarak test `curl` komutu:
   ```
   curl http://localhost:8080
   ```

1. Aşağıdaki ileti görürsünüz: **Hello Docker World**

   ![Örnek uygulamayı yerel olarak göz atın][SB01]

## <a name="create-an-azure-container-registry-using-the-azure-cli"></a>Azure CLI kullanarak bir Azure kapsayıcı kayıt defteri oluşturma

1. Bir komut istemi açın.

1. Azure hesabınızda oturum açın:
   ```azurecli
   az login
   ```

1. Bu öğreticide kullanılan Azure kaynakları için bir kaynak grubu oluşturun.
   ```azurecli
   az group create --name=wingtiptoys-kubernetes --location=eastus
   ```

1. Bir özel Azure kapsayıcı kayıt defteri kaynak grubunu oluşturun. Öğreticinin daha sonraki adımlarda bu kayıt defteri için Docker görüntüsü olarak örnek uygulaması iter. Değiştir `wingtiptoysregistry` kayıt için benzersiz bir ad ile.
   ```azurecli
   az acr create --admin-enabled --resource-group wingtiptoys-kubernetes--location eastus \
    --name wingtiptoysregistry --sku Basic
   ```

## <a name="push-your-app-to-the-container-registry"></a>Uygulamanız için kapsayıcı kayıt itme

1. Maven yüklemeniz için yapılandırma dizinine gidin (varsayılan ~/.m2/ veya C:\Users\username\.m2) ve açık *settings.xml* dosyasını bir metin düzenleyicisiyle.

1. Kapsayıcı kaydınız parolasını Azure CLI üzerinden alır.
   ```azurecli
   az acr credential show --name wingtiptoysregistry --query passwords[0]
   ```

   ```json
   {
  "name": "password",
  "value": "AbCdEfGhIjKlMnOpQrStUvWxYz"
   }
   ```

1. Azure kapsayıcı kayıt defteri kimliğinizi ve parolanızı yeni bir ekleme `<server>` koleksiyonunda *settings.xml* dosya.
`id` Ve `username` kayıt adı. Kullanım `password` değerinden (tırnaklar olmadan) önceki komutu.

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry</id>
         <username>wingtiptoysregistry</username>
         <password>AbCdEfGhIjKlMnOpQrStUvWxYz</password>
      </server>
   </servers>
   ```

1. Yay önyükleme uygulamanız için projeyi dizinine gidin (örneğin, "*C:\SpringBoot\gs-spring-boot-docker\complete*"veya"*/users/robert/SpringBoot/gs-spring-boot-docker/complete* ") ve açın *pom.xml* dosyasını bir metin düzenleyicisiyle.

1. Güncelleştirme `<properties>` koleksiyonunda *pom.xml* Azure kapsayıcı kayıt için oturum açma sunucusu değerle dosyası.

   ```xml
   <properties>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
   </properties>
   ```

1. Güncelleştirme `<plugins>` koleksiyonunda *pom.xml* dosya böylece `<plugin>` Azure kapsayıcı kayıt için oturum açma sunucusu adresi ve kayıt defteri adını içerir.

   ```xml
   <plugin>
      <groupId>com.spotify</groupId>
      <artifactId>docker-maven-plugin</artifactId>
      <version>0.4.11</version>
      <configuration>
         <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
         <dockerDirectory>src/main/docker</dockerDirectory>
         <resources>
            <resource>
               <targetPath>/</targetPath>
               <directory>${project.build.directory}</directory>
               <include>${project.build.finalName}.jar</include>
            </resource>
         </resources>
         <serverId>wingtiptoysregistry</serverId>
         <registryUrl>https://wingtiptoysregistry.azurecr.io</registryUrl>
      </configuration>
   </plugin>
   ```

1. Yay önyükleme uygulamanız için projeyi dizinine gidin ve Docker kapsayıcısı oluşturmak ve kayıt defterine görüntü gönderme için aşağıdaki komutu çalıştırın:

   ```
   mvn package docker:build -DpushImage
   ```

> [!NOTE]
>
>  Maven görüntü Azure'a iter, aşağıdakilerden birini benzer bir hata iletisini alabilirsiniz:
>
> * `[ERROR] Failed to execute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: no basic auth credentials`
>
> * `[ERROR] Failed to execute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: Incomplete Docker registry authorization credentials. Please provide all of username, password, and email or none.`
>
> Bu hata alırsanız, Azure için Docker komut satırından oturum açın.
>
> `docker login -u wingtiptoysregistry -p "AbCdEfGhIjKlMnOpQrStUvWxYz" wingtiptoysregistry.azurecr.io`
>
> Ardından, kapsayıcı gönderin:
>
> `docker push wingtiptoysregistry.azurecr.io/gs-spring-boot-docker`

## <a name="create-a-kubernetes-cluster-on-acs-using-the-azure-cli"></a>Üzerinde Azure CLI kullanarak ACS Kubernetes küme oluşturma

1. Azure kapsayıcı Hizmeti'nde bir Kubernetes kümesi oluşturun. Aşağıdaki komut oluşturur bir *kubernetes* kümesi *wingtiptoys kubernetes* kaynak grubu ile *wingtiptoys containerservice* küme adı olarak ve *wingtiptoys kubernetes* DNS öneki:
   ```azurecli
   az acs create --orchestrator-type=kubernetes --resource-group=wingtiptoys-kubernetes \ 
    --name=wingtiptoys-containerservice --dns-prefix=wingtiptoys-kubernetes
   ```
   Bu komutun tamamlanması biraz zaman alabilir.

1. Yükleme `kubectl` Azure CLI kullanarak. Linux kullanıcıları bu komutla önek gerekebilir `sudo` Kubernetes CLI dağıtır beri `/usr/local/bin`.
   ```azurecli
   az acs kubernetes install-cli
   ```

1. Kubernetes web arabiriminden kümenizi yönetebilirsiniz küme yapılandırma bilgilerini karşıdan yüklemek ve `kubectl`. 
   ```azurecli
   az acs kubernetes get-credentials --resource-group=wingtiptoys-kubernetes  \ 
    --name=wingtiptoys-containerservice
   ```

## <a name="deploy-the-image-to-your-kubernetes-cluster"></a>Yansıma Kubernetes kümeye dağıtma

Uygulamasını kullanarak bu öğreticinin dağıtır `kubectl`, Kubernetes web arabirimi aracılığıyla keşfetmeniz için izin.

### <a name="deploy-with-the-kubernetes-web-interface"></a>Kubernetes web arabirimiyle dağıtma

1. Bir komut istemi açın.

1. Yapılandırma Web sitesi Kubernetes kümeniz için varsayılan tarayıcınızda açın:
   ```
   az acs kubernetes browse --resource-group=wingtiptoys-kubernetes --name=wingtiptoys-containerservice
   ```

1. Kubernetes yapılandırma Web tarayıcınızda oturum açtığında, bağlantısını tıklatın **kapsayıcılı uygulamasını dağıtmak**:

   ![Kubernetes yapılandırma Web sitesi][KB01]

1. Zaman **kapsayıcılı uygulamasını dağıtmak** sayfası görüntülenirse, aşağıdaki seçenekleri belirtin:

   a. Seçin **aşağıda uygulama ayrıntılarını belirtin**.

   b. Yay önyükleme uygulama adınızı girin **uygulama adı**; örneğin: "*gs yay önyükleme docker*".

   c. Oturum açma sunucusu ve kapsayıcı görüntünüze için daha önce girin **kapsayıcı görüntü**; örneğin: "*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*".

   d. Seçin **dış** için **hizmet**.

   e. İç ve dış bağlantı noktaları belirtin **bağlantı noktası** ve **hedef bağlantı noktası** metin kutuları.

   ![Kubernetes yapılandırma Web sitesi][KB02]


1. Tıklatın **dağıtma** kapsayıcıyı dağıtmak için.

   ![Kapsayıcı dağıtma][KB05]

1. Uygulamanız dağıtıldıktan sonra altında listelenen yay önyükleme uygulamanızı görürsünüz **Hizmetleri**.

   ![Kubernetes Hizmetleri][KB06]

1. Bağlantısına tıklarsanız **dış uç noktalar**, Azure üzerinde çalışan yay önyükleme uygulamanızı görebilirsiniz.

   ![Kubernetes Hizmetleri][KB07]

   ![Azure ile ilgili örnek uygulaması Gözat][SB02]


### <a name="deploy-with-kubectl"></a>Kubectl ile dağıtma

1. Bir komut istemi açın.

1. Kullanarak, kapsayıcı Kubernetes kümede çalışmasını `kubectl run` komutu. Uygulamanızı Kubernetes için hizmet adı ve tam görüntü adı verin. Örneğin:
   ```
   kubectl run gs-spring-boot-docker --image=wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest
   ```
   Bu komutta:

   * Kapsayıcı adı `gs-spring-boot-docker` hemen sonra belirtilen `run` komutu

   * `--image` Parametresi, birleştirilmiş oturum açma sunucusu ve görüntü adı olarak belirtir`wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest`

1. Harici olarak kullanarak Kubernetes kümenizi kullanıma `kubectl expose` komutu. Hizmet adı, uygulamaya erişmek için kullanılan genel kullanıma yönelik TCP bağlantı noktası ve uygulamanızı dinlediği iç hedef bağlantı noktası belirtin. Örneğin:
   ```
   kubectl expose deployment gs-spring-boot-docker --type=LoadBalancer --port=80 --target-port=8080
   ```
   Bu komutta:

   * Kapsayıcı adı `gs-spring-boot-docker` hemen sonra belirtilen `expose deployment` komutu

   * `--type` Parametresi, kümenin yük dengeleyici kullandığını belirtir

   * `--port` Parametresi, genel kullanıma yönelik TCP bağlantı noktası 80 belirtir. Uygulamanın bu bağlantı noktasına erişim.

   * `--target-port` Parametresi, 8080 iç TCP bağlantı noktasını belirtir. Yük Dengeleyici, bu bağlantı noktasında uygulamanıza isteklerini iletir.

1. Kümeye uygulama dağıtıldığında, dış IP adresine sorgulamak ve web tarayıcınızda açın:

   ```
   kubectl get services -o jsonpath={.items[*].status.loadBalancer.ingress[0].ip} --namespace=${namespace}
   ```

   ![Azure ile ilgili örnek uygulaması Gözat][SB02]


## <a name="next-steps"></a>Sonraki adımlar

Azure üzerinde yay önyükleme kullanma hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Yay önyükleme uygulamasını Azure App Service'e dağıtma](../../app-service/app-service-deploy-spring-boot-web-app-on-azure.md)
* [Azure kapsayıcı Hizmeti'nde Linux'ta yay önyükleme uygulamasını dağıtma](container-service-deploy-spring-boot-app-on-linux.md)

Azure’u Java ile kullanma hakkında daha fazla bilgi edinmek için bkz. [Azure Java Geliştirici Merkezi] ve [Visual Studio Team Services için Java Araçları].

Docker örnek proje yay önyükleme hakkında daha fazla bilgi için bkz: [Docker Başlarken yay önyüklemede].

Aşağıdaki bağlantılar yay önyükleme uygulamaları oluşturma hakkında ek bilgi sağlar:

* Basit bir yay önyükleme uygulama oluşturma hakkında daha fazla bilgi için https://start.spring.io/ adresindeki yay Initializr bakın.

Aşağıdaki bağlantılar Kubernetes Azure ile kullanma hakkında ek bilgi sağlar:

* [Kapsayıcı hizmeti Kubernetes kümede kullanmaya başlama](https://docs.microsoft.com/azure/container-service/container-service-kubernetes-walkthrough)
* [Azure kapsayıcı hizmeti ile Kubernetes web kullanıcı arabirimini kullanma](https://docs.microsoft.com/azure/container-service/container-service-kubernetes-ui)

Kubernetes komut satırı arabirimini kullanma hakkında daha fazla bilgi bulunur **kubectl** adresindeki Kullanıcı Kılavuzu'na <https://kubernetes.io/docs/user-guide/kubectl/>.

Kubernetes Web sitesi özel kayıt defterleri görüntüleri görüşmeniz çeşitli makaleler vardır:

* [Hizmet yapılandırma pod'ları için hesapları]
* [Ad alanları]
* [Özel bir kayıt defterinden görüntüyü çekme]

Azure ile özel Docker görüntülerinizi kullanımı için ek örnekler için bkz: [Linux Azure Web uygulaması için özel bir Docker görüntü kullanarak].

<!-- URL List -->

[Azure komut satırı arabirimi (CLI)]: /cli/azure/overview
[ Azure kapsayıcı Hizmeti'ni (ACS)]: https://azure.microsoft.com/services/container-service/
[Azure Java Geliştirici Merkezi]: https://azure.microsoft.com/develop/java/
[Azure portal]: https://portal.azure.com/
[Create a private Docker container registry using the Azure portal]: /azure/container-registry/container-registry-get-started-portal
[Linux Azure Web uygulaması için özel bir Docker görüntü kullanarak]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[ücretsiz Azure hesabı]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Geliştirme Seti (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Visual Studio Team Services için Java Araçları]: https://java.visualstudio.com/
[Kubernetes]: https://kubernetes.io/
[Kubernetes Command-Line Interface (kubectl)]: https://kubernetes.io/docs/user-guide/kubectl-overview/
[Maven]: http://maven.apache.org/
[MSDN abone Avantajlarınızı]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[yay önyükleme]: http://projects.spring.io/spring-boot/
[Docker Başlarken yay önyüklemede]: https://github.com/spring-guides/gs-spring-boot-docker
[Yay Framework]: https://spring.io/
[Hizmet yapılandırma pod'ları için hesapları]: https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/
[Ad alanları]: https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/
[Özel bir kayıt defterinden görüntüyü çekme]: https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/

<!-- IMG List -->

[SB01]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/SB01.png
[SB02]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/SB02.png

[AR01]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/AR01.png
[AR02]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/AR02.png
[AR03]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/AR03.png
[AR04]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/AR04.png

[KB01]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB01.png
[KB02]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB02.png
[KB03]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB03.png
[KB04]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB04.png
[KB05]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB05.png
[KB06]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB06.png
[KB07]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB07.png
