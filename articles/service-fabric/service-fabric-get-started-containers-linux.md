---
title: "Linux üzerinde Azure Service Fabric kapsayıcı uygulaması oluşturma | Microsoft Docs"
description: "Azure Service Fabric üzerinde ilk Linux kapsayıcı uygulamanızı oluşturun.  Uygulamanızla bir Docker görüntüsü oluşturun, görüntüyü bir kapsayıcı kayıt defterine iletin, bir Service Fabric kapsayıcı uygulaması oluşturup dağıtın."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/28/2017
ms.author: ryanwi
ms.translationtype: HT
ms.sourcegitcommit: 9afd12380926d4e16b7384ff07d229735ca94aaa
ms.openlocfilehash: e4ca1e8df8337e578e014cedec68553e6fc56299
ms.contentlocale: tr-tr
ms.lasthandoff: 07/15/2017

---

# <a name="create-your-first-service-fabric-container-application-on-linux"></a>Linux üzerinde ilk Service Fabric kapsayıcı uygulamanızı oluşturma
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started-containers.md)
> * [Linux](service-fabric-get-started-containers-linux.md)

Bir Service Fabric kümesindeki Linux kapsayıcısında mevcut olan bir uygulamayı çalıştırmak için uygulamanızda herhangi bir değişiklik yapılması gerekmez. Bu makalede, Python [Flask](http://flask.pocoo.org/) web uygulaması içeren bir Docker görüntüsü oluşturma ve bunu Service Fabric kümesine dağıtma işlemlerinde size yol gösterilir.  Ayrıca, kapsayıcıya alınmış uygulamanızı [Azure Container Registry](/azure/container-registry/) aracılığıyla paylaşırsınız.  Bu makale Docker hakkında temel bir anlayışınızın olduğunu varsayar. [Docker’a Genel Bakış](https://docs.docker.com/engine/understanding-docker/) makalesini okuyarak Docker hakkında bilgi edinebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar
* Şunları çalıştıran bir geliştirme bilgisayarı:
  * [Service Fabric SDK’sı ve araçları](service-fabric-get-started-linux.md).
  * [Linux için Docker CE](https://docs.docker.com/engine/installation/#prior-releases). 

* Azure Container Registry’deki bir kayıt defteri - Azure aboneliğinizde [Kapsayıcı kayıt defteri oluşturun](../container-registry/container-registry-get-started-portal.md). 

## <a name="define-the-docker-container"></a>Docker kapsayıcısını tanımlama
Docker Hub’ında bulunan [Python görüntüsünü](https://hub.docker.com/_/python/) temel alan bir görüntü oluşturun. 

Docker kapsayıcınızı bir Dockerfile içinde tanımlayın. Dockerfile, kapsayıcınızın içindeki ortamı ayarlama, çalıştırmak istediğiniz uygulamayı yükleme ve bağlantı noktalarını eşleme yönergelerini içerir. Dockerfile, görüntüyü oluşturan `docker build` komutunun girdisidir. 

Boş bir dizin oluşturun ve *Dockerfile* dosyasını oluşturun (dosya uzantısı kullanmayın). Aşağıdakini *Dockerfile* dosyasına ekleyin ve değişikliklerinizi kaydedin:

```
# Use an official Python runtime as a base image
FROM python:2.7-slim

# Set the working directory to /app
WORKDIR /app

# Copy the current directory contents into the container at /app
ADD . /app

# Install any needed packages specified in requirements.txt
RUN pip install -r requirements.txt

# Make port 80 available outside this container
EXPOSE 80

# Define environment variable
ENV NAME World

# Run app.py when the container launches
CMD ["python", "app.py"]
```

Daha fazla bilgi için [Dockerfile başvurusunu](https://docs.docker.com/engine/reference/builder/) okuyun.

## <a name="create-a-simple-web-application"></a>Basit web uygulaması oluşturma
Bağlantı noktası 80 üzerinden dinleyen ve "Merhaba Dünya!" başlığını döndüren bir Flask web uygulaması oluşturun.  Aynı dizinde, *requirements.txt* dosyasını oluşturun.  Aşağıdakini dosyaya ekleyin ve değişikliklerinizi kaydedin:
```
Flask
```

Ayrıca, *app.py* dosyasını da oluşturun ve aşağıdakini ekleyin:

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello():
    
    return 'Hello World!'

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=80)
```

## <a name="build-the-image"></a>Görüntü oluşturma
Web uygulamanızı çalıştıran görüntüyü oluşturmak için `docker build` komutunu çalıştırın. Bir PowerShell penceresi açıp *c:\temp\helloworldapp* dizinine gidin. Şu komutu çalıştırın:

```bash
docker build -t helloworldapp .
```

Bu komut Dockerfile içindeki yönergeleri kullanarak yeni görüntüyü oluşturur ve "helloworldapp" olarak adlandırır (-t etiketi). Görüntü oluşturulduğunda, temel görüntü Docker Hub’ından alınır ve uygulamanızı temel görüntünün üstüne ekleyen yeni bir görüntü oluşturulur.  

Oluşturma komutu tamamlandıktan sonra, yeni görüntü üzerindeki bilgileri görmek için `docker images` komutunu çalıştırın:

```bash
$ docker images
    
REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
helloworldapp                 latest              86838648aab6        2 minutes ago       194 MB
```

## <a name="run-the-application-locally"></a>Uygulamayı yerel olarak çalıştırma
Kapsayıcıya alınmış uygulamanızı kapsayıcı kayıt defterine göndermeden önce uygulamanın yerel olarak çalıştığını doğrulayın.  

Bilgisayarınızın 4000 numaralı bağlantı noktasını kapsayıcının ortaya konan 80 numaralı bağlantı noktasına eşleyerek uygulamayı çalıştırın:

```bash
docker run -d -p 4000:80 --name my-web-site helloworldapp
```

*name*, çalışan kapsayıcıya bir ad verir (kapsayıcı kimliği yerine).

Çalışan kapsayıcıya bağlanın.  4000 numaralı bağlantı noktasında döndürülen IP adresine işaret eden bir web tarayıcısı açın (örneğin, "http://localhost:4000"). "Hello World!" başlığının tarayıcıda gösterildiğini görürsünüz.

![Merhaba Dünya!][hello-world]

Kapsayıcınızı durdurmak için şu komutu çalıştırın:

```bash
docker stop my-web-site
```

Kapsayıcıyı geliştirme makinenizden silin:

```bash
docker rm my-web-site
```

## <a name="push-the-image-to-the-container-registry"></a>Görüntüyü kapsayıcı kayıt defterine gönderme
Uygulamanın Docker'da çalıştığını doğruladıktan sonra, görüntüyü Azure Container Registry'de kayıt defterine gönderin.

Kapsayıcı kayıt defterinizde [kayıt defteri kimlik bilgileriniz](../container-registry/container-registry-authentication.md) ile oturum açmak için `docker login` komutunu çalıştırın.

Aşağıdaki örnekte, bir Azure Active Directory [hizmet sorumlusunun](../active-directory/active-directory-application-objects.md) kimliği ve parolası geçirilmiştir. Örneğin, bir otomasyon senaryosu için kayıt defterinize bir hizmet sorumlusu atamış olabilirsiniz.  İsterseniz, kayıt defteri kullanıcı kimliğiniz ve parolanızı kullanarak oturum açabilirsiniz.

```bash
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

Aşağıdaki komut, görüntünün kayıt defterinize ait tam yolu içeren bir etiketini veya diğer adını oluşturur. Bu örnek, kayıt defterinin kökünde dağınıklığı önlemek için `samples` ad alanına görüntüyü yerleştirir.

```bash
docker tag helloworldapp myregistry.azurecr.io/samples/helloworldapp
```

Görüntüyü kapsayıcı kayıt defterinize gönderin:

```bash
docker push myregistry.azurecr.io/samples/helloworldapp
```

## <a name="package-the-docker-image-with-yeoman"></a>Docker görüntüsünü Yeoman ile paketleme
Linux için Service Fabric SDK’sı uygulamanızı oluşturmayı ve kapsayıcı görüntüsü eklemeyi kolaylaştıran bir [Yeoman](http://yeoman.io/) oluşturucu içerir. Şimdi Yeoman kullanarak *SimpleContainerApp* adlı tek bir Docker kapsayıcısı olan bir uygulama oluşturalım.

Service Fabric kapsayıcı uygulaması oluşturmak için, terminal penceresini açın ve `yo azuresfcontainer` komutunu çalıştırın.  

Uygulamanızı adlandırın (örneğin, "mycontainer"). 

Kapsayıcı kayıt defterindeki kapsayıcı görüntüsünün URL'sini sağlayın (örneğin, ""). 

Bu görüntüde iş yükü giriş noktası tanımlanmış olduğundan, giriş komutlarının açıkça belirtilmesi gerekir (komutlar kapsayıcının içinde çalıştırılır ve bu da başlatma sonrasında kapsayıcıyı çalışır durumda tutar). 

"1" örnek sayısı belirtin.

![Kapsayıcılar için Service Fabric Yeoman oluşturucusu][sf-yeoman]

## <a name="configure-port-mapping-and-container-repository-authentication"></a>Bağlantı noktası eşlemesini ve kapsayıcı deposu kimlik doğrulamasını yapılandırma
Kapsayıcı içinde alınmış hizmetinize iletişim için bir uç nokta gerekir.  Şimdi ServiceManifest.xml dosyasındaki `Endpoint` öğesine protokol, bağlantı noktası ve tür ekleyin. Bu makalede kapsayıcı hizmeti 4000 numaralı bağlantı noktasında dinler: 

```xml
<Endpoint Name="myserviceTypeEndpoint" UriScheme="http" Port="4000" Protocol="http"/>
```
`UriScheme` değerinin sağlanması, kapsayıcı uç noktasını bulunabilirlik için Service Fabric Adlandırma hizmetine otomatik olarak kaydeder. Bu makalenin sonunda tam bir ServiceManifest.xml örnek dosyası verilmiştir. 

ApplicationManifest.xml dosyasının `ContainerHostPolicies` kısmında bir `PortBinding` ilkesi belirterek kapsayıcı bağlantı noktasından ana bilgisayar bağlantı noktasına eşleme yapılandırın.  Bu makalede `ContainerPort` değeri 80 (Dockerfile içinde belirtildiği gibi kapsayıcı 80 numaralı bağlantı noktasını gösterir) ve `EndpointRef` değeri "myserviceTypeEndpoint"’tir (hizmet bildiriminde tanımlanan uç nokta).  4000 numaralı bağlantı noktasında hizmete gelen istekler, kapsayıcı üzerindeki 80 numaralı bağlantı noktasıyla eşlenir.  Kapsayıcınızın özel bir depoda kimlik doğrulaması yapması gerekiyorsa, `RepositoryCredentials` öğesini ekleyin.  Bu makale için, myregistry.azurecr.io kapsayıcı kayıt defterine ait hesap adını ve parolayı ekleyin. 

```xml
<Policies>
    <ContainerHostPolicies CodePackageRef="Code">
        <RepositoryCredentials AccountName="myregistry" Password="=P==/==/=8=/=+u4lyOB=+=nWzEeRfF=" PasswordEncrypted="false"/>
        <PortBinding ContainerPort="80" EndpointRef="myserviceTypeEndpoint"/>
    </ContainerHostPolicies>
</Policies>
```

## <a name="build-and-package-the-service-fabric-application"></a>Service Fabric uygulamasını oluşturma ve paketleme
Service Fabric Yeoman şablonları, uygulamayı terminalden oluşturmak için kullanabileceğiniz bir [Gradle](https://gradle.org/) derleme betiği içerir. Uygulamayı derlemek ve paketlemek için şu komutu çalıştırın:

```bash
cd mycontainer
gradle
```

## <a name="deploy-the-application"></a>Uygulamayı dağıtma
Uygulama oluşturulduktan sonra Azure CLI kullanarak yerel kümeye dağıtabilirsiniz.

Yerel Service Fabric kümesine bağlanın.

```bash
azure servicefabric cluster connect
```

Uygulama paketini kümenin görüntü deposuna kopyalamak, uygulama türünü kaydetmek ve uygulamanın bir örneğini oluşturmak için şablonda verilen yükleme betiğini kullanın.

```bash
./install.sh
```

Bir tarayıcı açın ve http://localhost:19080/Explorer adresindeki Service Fabric Explorer’a gidin (Vagrant’ı Mac OS X üzerinde kullanıyorsanız localhost ifadesini sanal makinenin özel IP’si ile değiştirin). Uygulamalar düğümünü genişletin ve şu anda uygulamanızın türü için bir giriş ve bu türün ilk örneği için başka bir giriş olduğuna dikkat edin.

Çalışan kapsayıcıya bağlanın.  4000 numaralı bağlantı noktasında döndürülen IP adresine işaret eden bir web tarayıcısı açın (örneğin, "http://localhost:4000"). "Hello World!" başlığının tarayıcıda gösterildiğini görürsünüz.

![Merhaba Dünya!][hello-world]

## <a name="clean-up"></a>Temizleme
Yerel dağıtım kümesinden uygulama örneğini silmek ve uygulama türünün kaydını silmek için şablonda sağlanan kaldırma betiğini kullanın.

```bash
./uninstall.sh
```

Görüntüyü kapsayıcı kayıt defterine gönderdikten sonra yerel görüntüyü geliştirme bilgisayarınızdan silebilirsiniz:

```
docker rmi helloworldapp
docker rmi myregistry.azurecr.io/samples/helloworldapp
```

## <a name="complete-example-service-fabric-application-and-service-manifests"></a>Tam Service Fabric uygulaması ve hizmet bildirimleri örneği
Bu makalede kullanılan tam hizmet ve uygulama bildirimleri aşağıda verilmiştir.

### <a name="servicemanifestxml"></a>ServiceManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="myservicePkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType.
         The UseImplicitHost attribute indicates this is a guest service. -->
    <StatelessServiceType ServiceTypeName="myserviceType" UseImplicitHost="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <!-- Follow this link for more information about deploying Windows containers 
      to Service Fabric: https://aka.ms/sfguestcontainers -->
      <ContainerHost>
        <ImageName>myregistry.azurecr.io/samples/helloworldapp</ImageName>
        <Commands></Commands>
      </ContainerHost>
    </EntryPoint>
    <!-- Pass environment variables to your container: -->
    
    <EnvironmentVariables>
      <!--
      <EnvironmentVariable Name="VariableName" Value="VariableValue"/>
      -->
    </EnvironmentVariables>
    
  </CodePackage>

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to 
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="myserviceTypeEndpoint" UriScheme="http" Port="4000" Protocol="http"/>
    </Endpoints>
  </Resources>
</ServiceManifest>
```
### <a name="applicationmanifestxml"></a>ApplicationManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest ApplicationTypeName="mycontainerType"
                     ApplicationTypeVersion="1.0.0"
                     xmlns="http://schemas.microsoft.com/2011/01/fabric"
                     xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion 
       should match the Name and Version attributes of the ServiceManifest element defined in the 
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="myservicePkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <ContainerHostPolicies CodePackageRef="Code">
        <RepositoryCredentials AccountName="myregistry" Password="=P==/==/=8=/=+u4lyOB=+=nWzEeRfF=" PasswordEncrypted="false"/>
        <PortBinding ContainerPort="80" EndpointRef="myserviceTypeEndpoint"/>
      </ContainerHostPolicies>
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <!-- The section below creates instances of service types, when an instance of this 
         application type is created. You can also create one or more instances of service type using the 
         ServiceFabric PowerShell module.
         
         The attribute ServiceTypeName below must match the name defined in the imported ServiceManifest.xml file. -->
    <Service Name="myservice">
      <!-- On a local development cluster, set InstanceCount to 1.  On a multi-node production 
      cluster, set InstanceCount to -1 for the container service to run on every node in 
      the cluster.
      -->
      <StatelessService ServiceTypeName="myserviceType" InstanceCount="1">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
</ApplicationManifest>
```

## <a name="next-steps"></a>Sonraki adımlar
* [Service Fabric’te kapsayıcı](service-fabric-containers-overview.md) çalıştırma hakkında daha fazla bilgi edinin.
* [Kapsayıcı içinde .NET uygulaması dağıtma](service-fabric-host-app-in-a-container.md) öğreticisini okuyun.
* Service Fabric [uygulama yaşam döngüsü](service-fabric-application-lifecycle.md) hakkında bilgi edinin.
* GitHub’da [Service Fabric kapsayıcı kod örneklerine](https://github.com/Azure-Samples/service-fabric-dotnet-containers) bakın.

[hello-world]: ./media/service-fabric-get-started-containers-linux/HelloWorld.png
[sf-yeoman]: ./media/service-fabric-get-started-containers-linux/YoSF.png

