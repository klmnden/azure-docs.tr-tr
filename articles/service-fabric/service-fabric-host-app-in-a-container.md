---
title: Azure Service Fabric’e kapsayıcıdaki bir .NET uygulamasını dağıtma | Microsoft Docs
description: Visual Studio’daki bir .NET uygulamasını Docker Kapsayıcısı’nda paketlemeyi öğretir. Bu yeni “kapsayıcı” uygulaması, daha sonra bir Service Fabric kümesine dağıtılır.
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/23/2018
ms.author: mikhegn
ms.openlocfilehash: 04a6fbc56d3c65cfb53339c4178dfa36e2aeb4ea
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="deploy-a-net-application-in-a-windows-container-to-azure-service-fabric"></a>Azure Service Fabric’e Windows kapsayıcısındaki bir .NET uygulamasını dağıtma

Bu öğreticide, Azure’daki bir Windows kapsayıcısında bulunan ASP.NET uygulamasını dağıtma işlemi gösterilir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Visual Studio'da Docker projesi oluşturma
> * Mevcut uygulamayı kapsayıcılı hale getirme
> * Visual Studio ve VSTS ile sürekli tümleştirme kurulumu

## <a name="prerequisites"></a>Ön koşullar

1. Windows 10’da kapsayıcıları çalıştırabilmek için [Docker Windows CE](https://store.docker.com/editions/community/docker-ce-desktop-windows?tab=description)’yi yükleyin.
2. [Windows 10 Kapsayıcıları hızlı başlangıç][link-container-quickstart] ile kendinizi alıştırın.
3. [Fabrikam Fiber CallCenter][link-fabrikam-github] örnek uygulamasını indirin.
4. [Azure PowerShell][link-azure-powershell-install]'i yükleyin
5. [Visual Studio 2017 için Sürekli Teslim Araçları uzantısını][link-visualstudio-cd-extension] yükleyin
6. Bir [Azure aboneliği][link-azure-subscription] ve [Visual Studio Team Services hesabı][link-vsts-account] oluşturun. 
7. [Azure’da küme oluşturun](service-fabric-tutorial-create-vnet-and-windows-cluster.md)

## <a name="create-a-cluster-on-azure"></a>Azure’da küme oluşturma
Service Fabric uygulamaları, ağ bağlantılı sanal veya fiziksel makinelerin bulunduğu bir kümede çalışır. Uygulamanızı oluşturup dağıtmadan önce, [Azure’da çalışan bir Service Fabric kümesini kurun](service-fabric-tutorial-create-vnet-and-windows-cluster.md). Kümeyi oluştururken, çalışan kapsayıcıları destekleyen (Kapsayıcılar ile Windows Server 2016 Datacenter gibi) bir SKU seçin.

## <a name="containerize-the-application"></a>Uygulamayı kapsayıcılı hale getirme

Artık Azure’da çalışan bir Service Fabric kümesine sahip olduğunuza göre, kapsayıcılı hale getirilmiş bir uygulama oluşturup dağıtmaya hazırsınız. Uygulamamızı kapsayıcıda çalıştırmaya başlamak için, Visual Studio’daki projeye **Docker Desteği** eklememiz gerekiyor. Uygulamaya **Docker Desteği** eklediğinizde iki şey olur. İlk olarak, projeye _Dockerfile_ eklenir. Bu yeni dosya, kapsayıcı görüntüsünün nasıl oluşturulduğunu açıklar. Daha sonra, çözüme yeni bir _docker-compose_ projesi eklenir. Yeni proje birkaç docker-compose dosyası içerir. Docker-compose dosyaları, kapsayıcının nasıl çalıştığını açıklamak için kullanılabilir.

[Visual Studio Kapsayıcı Araçları][link-visualstudio-container-tools] ile çalışma hakkında daha fazla bilgi edinin.

### <a name="add-docker-support"></a>Docker desteği ekleme

[FabrikamFiber.CallCenter.sln][link-fabrikam-github] dosyasını Visual Studio’da açın.

**FabrikamFiber.Web** projesi > **Docker Desteği** > **Ekle**'ye sağ tıklayın.

### <a name="add-support-for-sql"></a>SQL için destek ekleme

Bu uygulama, veri sağlayıcısı olarak SQL kullanır; dolayısıyla uygulamayı çalıştırmak için bir SQL sunucusu gereklidir. Docker-compose.override.yml dosyamızdaki bir SQL Server kapsayıcı görüntüsüne başvurun.

Visual Studio’da **Çözüm Gezgini**’ni açın, **docker-compose** seçeneğini bulun ve **docker-compose.override.yml** dosyasını açın.

`services:` düğümüne gidip kapsayıcı için SQL Server girdisini tanımlayan `db:` adlı düğümü ekleyin.

```yml
  db:
    image: microsoft/mssql-server-windows-developer
    environment:
      sa_password: "Password1"
      ACCEPT_EULA: "Y"
    ports:
      - "1433"
    healthcheck:
      test: [ "CMD", "sqlcmd", "-U", "sa", "-P", "Password1", "-Q", "select 1" ]
      interval: 1s
      retries: 20
```

>[!NOTE]
>Ana bilgisayarınızdan erişilebilir olduğu sürece, yerel hata ayıklama için tercih ettiğiniz herhangi bir SQL Server’ı kullanabilirsiniz. Ancak **localdb**, `container -> host` iletişimini desteklemez.

>[!WARNING]
>Kapsayıcıda SQL Server çalıştırmak, kalıcı verileri desteklemez. Kapsayıcı durduğunda verileriniz silinir. Bu yapılandırmayı üretim için kullanmayın.

`fabrikamfiber.web:` düğümüne gidip `depends_on:` adlı bir alt düğüm ekleyin. Bu işlem, `db` hizmetinin (SQL Server kapsayıcısı) web uygulamamızdan (fabrikamfiber.web) önce başlatılmasını sağlar.

```yml
  fabrikamfiber.web:
    depends_on:
      - db
```

### <a name="update-the-web-config"></a>Web yapılandırmasını güncelleştirme

**FabrikamFiber.Web** projesine geri dönüp **web.config** dosyasındaki bağlantı dizesini kapsayıcıdaki SQL Server noktası ile güncelleştirin.

```xml
<add name="FabrikamFiber-Express" connectionString="Data Source=db,1433;Database=FabrikamFiber;User Id=sa;Password=Password1;MultipleActiveResultSets=True" providerName="System.Data.SqlClient" />

<add name="FabrikamFiber-DataWarehouse" connectionString="Data Source=db,1433;Database=FabrikamFiber;User Id=sa;Password=Password1;MultipleActiveResultSets=True" providerName="System.Data.SqlClient" />
```

>[!NOTE]
>Web uygulamanızın sürüm derlemesini oluştururken farklı bir SQL Server kullanmak isterseniz, web.release.config dosyasına başka bir bağlantı dizesi ekleyin.

### <a name="test-your-container"></a>Kapsayıcınızı test etme

Kapsayıcınızda uygulamayı çalıştırıp hata ayıklama işlemini gerçekleştirmek için **F5** tuşuna basın.

Edge, iç NAT ağındaki kapsayıcının IP adresini (genellikle 172.x.x.x) kullanarak uygulamanızda tanımlı başlangıç sayfasını açar. Visual Studio 2017 kullanarak kapsayıcılardaki uygulamalarda hata ayıklama hakkında daha fazla bilgi edinmek için [bu makaleye][link-debug-container] göz atın.

![kapsayıcıdaki fabrikam örneği][image-web-preview]

Artık kapsayıcı, Service Fabric uygulamasında oluşturulup paketlenmeye hazırdır. Makinenizde kapsayıcı görüntüsü oluşturulduğunda, bu görüntüyü herhangi bir kapsayıcı kayıt defterine gönderebilir ve çalıştırmak için herhangi bir ana bilgisayara çekebilirsiniz.

## <a name="get-the-application-ready-for-the-cloud"></a>Uygulamayı buluta hazırlama

Uygulamayı Azure’daki Service Fabric’te çalışmaya hazır hale getirmek için iki adımı tamamlamamız gerekir:

1. Service Fabric kümesinde web uygulamamıza ulaşmak istediğimiz bağlantı noktasını kullanıma sunma.
2. Uygulamamız için üretime hazır SQL veritabanı sağlama.

### <a name="expose-the-port-for-the-app"></a>Uygulama için bağlantı noktasını kullanıma sunma
Yapılandırdığımız Service Fabric kümesinin *80* numaralı bağlantı noktası, Azure Load Balancer’da varsayılan olarak açıktır. Bu, kümeye gelen trafiği dengeler. Kapsayıcımızı, docker-compose.yml dosyamızı kullanarak bu bağlantı noktasında kullanıma sunabiliriz.

Visual Studio’da **Çözüm Gezgini**’ni açın, **docker-compose** seçeneğini bulun ve **docker-compose.yml** dosyasını açın.

`fabrikamfiber.web:` düğümünü değiştirip `ports:` adlı bir alt düğüm ekleyin.

`- "80:80"` dize girdisi ekleyin. Docker-compose.yml dosyanız şöyle görünmelidir:

```yml
  version: '3'

  services:
    fabrikamfiber.web:
      image: fabrikamfiber.web
      build:
        context: .\FabrikamFiber.Web
        dockerfile: Dockerfile
      ports:
        - "80:80"
```

### <a name="use-a-production-sql-database"></a>Üretim SQL veritabanı kullanma
Üretimde çalışırken, veritabanımızdaki verilerimizin kalıcı olması gerekir. Şu anda kapsayıcıdaki verilerin kalıcı olmasını garanti altına alan bir yöntem olmadığından üretim verileriniz, kapsayıcıdaki bir SQL Server’da depolanamaz.

Azure SQL Veritabanı kullanmanızı öneririz. Azure’da yönetilen bir SQL Server ayarlayıp çalıştırmak için [Azure SQL Veritabanı Hızlı Başlangıçları][link-azure-sql] makalesini ziyaret edin.

>[!NOTE]
>**FabrikamFiber.Web** projesinde bulunan **web.release.config** dosyasındaki SQL Server bağlantı dizelerini değiştirmeyi unutmayın.
>
>Erişilebilir bir SQL veritabanı yoksa, bu uygulama düzgün biçimde başarısız olur. SQL Server olmadan devam edip uygulamayı dağıtmayı seçebilirsiniz.

## <a name="deploy-with-visual-studio-team-services"></a>Visual Studio Team Services ile dağıtma

Visual Studio Team Services kullanarak dağıtımı ayarlamak istiyorsanız, [Visual Studio 2017 için Sürekli Teslim Araçları uzantısını][link-visualstudio-cd-extension] yüklemeniz gerekir. Bu uzantı, Visual Studio Team Services’ı yapılandırıp uygulamanızın Service Fabric kümenize dağıtılmasını sağlayarak Azure’a dağıtım işlemini kolaylaştırır.

Kullanmaya başlamak için, kodunuzun kaynak denetiminde barındırılması gerekir. Bu bölümün kalan kısmında **git** kullanıldığı varsayılmıştır.

### <a name="set-up-a-vsts-repo"></a>VSTS deposu ayarlama
Visual Studio’nun sağ alt köşesindeki **Kaynak Denetimine Ekle** > **Git** seçeneğine (veya dilediğiniz başka bir seçeneğe) tıklayın.

![kaynak denetim düğmesine basma][image-source-control]

_Takım Gezgini_ bölmesinde **Git Deposunda Yayımla** seçeneğine basın.

VSTS depo adınızı seçip **Depo** seçeneğine basın.

![VSTS’ye depo yayımlama][image-publish-repo]

Kodunuz VSTS kaynak deposuyla eşitlendiğinden, sürekli tümleştirmeyi ve sürekli teslimi yapılandırabilirsiniz.

### <a name="setup-continuous-delivery"></a>Sürekli teslimi ayarlama

_Çözüm Gezgini_’nde **çözüm** > **Sürekli Teslimi Yapılandır** seçeneğine sağ tıklayın.

Azure Aboneliğini seçin.

**Ana Bilgisayar Türü**’nü **Service Fabric Kümesi** olarak ayarlayın.

**Hedef Ana Bilgisayar**’ı önceki bölümde oluşturduğunuz Service Fabric kümesi olarak ayarlayın.

Kapsayıcınızı yayımlayacağınız **Kapsayıcı Kayıt Defteri**’ni seçin.

>[!TIP]
>Bir kapsayıcı kayıt defteri oluşturmak için **Düzenle** düğmesini kullanın.

**Tamam**'a basın.

![service fabric sürekli tümleştirmeyi kurma][image-setup-ci]
   
   Yapılandırma tamamlandığında, kapsayıcınız Service Fabric’e dağıtılır. Depoya gönderdiğiniz her güncelleştirmede yeni bir derleme ve sürüm yürütülür.
   
   >[!NOTE]
   >Kapsayıcı görüntülerini oluşturmak yaklaşık 15 dakika süren bir işlemdir.
   >Service Fabric kümesine yapılan ilk dağıtım, temel Windows Server Core kapsayıcı görüntülerinin indirilmesine neden olur. İndirme işleminin tamamlanması yaklaşık 5-10 dakika sürer.

Kümenizin URL’sini kullanarak Fabrikam Call Center’a göz atın, örneğin, *http://mycluster.westeurope.cloudapp.azure.com*

Artık Fabrikam Call Center çözümünüzü kapsayıcılı hale getirip dağıttığınıza göre, [Azure Portal][link-azure-portal]'ı açıp Service Fabric’te çalışan uygulamanızı görebilirsiniz. Uygulamayı denemek için bir web tarayıcısı açın ve Service Fabric kümenizin URL’sine gidin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Visual Studio'da Docker projesi oluşturma
> * Mevcut uygulamayı kapsayıcılı hale getirme
> * Visual Studio ve VSTS ile sürekli tümleştirme kurulumu

Öğreticinin sonraki bölümünde [kapsayıcınızı izlemeyi](service-fabric-tutorial-monitoring-wincontainers.md) nasıl ayarlayacağınızı öğrenebilirsiniz.

<!--   NOTE SURE WHAT WE SHOULD DO YET HERE

Advance to the next tutorial to learn how to bind a custom SSL certificate to it.

> [!div class="nextstepaction"]
> [Bind an existing custom SSL certificate to Azure Web Apps](app-service-web-tutorial-custom-ssl.md)

## Next steps

- [Container Tooling in Visual Studio][link-visualstudio-container-tools]
- [Get started with containers in Service Fabric][link-servicefabric-containers]
- [Creating Service Fabric applications][link-servicefabric-createapp]
-->

[link-debug-container]: /dotnet/articles/core/docker/visual-studio-tools-for-docker
[link-fabrikam-github]: https://aka.ms/fabrikamcontainer
[link-container-quickstart]: /virtualization/windowscontainers/quick-start/quick-start-windows-10
[link-visualstudio-container-tools]: /dotnet/articles/core/docker/visual-studio-tools-for-docker
[link-azure-powershell-install]: /powershell/azure/install-azurerm-ps
[link-servicefabric-create-secure-clusters]: service-fabric-cluster-creation-via-arm.md
[link-visualstudio-cd-extension]: https://aka.ms/cd4vs
[link-servicefabric-containers]: service-fabric-get-started-containers.md
[link-servicefabric-createapp]: service-fabric-create-your-first-application-in-visual-studio.md
[link-azure-portal]: https://portal.azure.com
[link-sf-clustertemplate]: https://aka.ms/securepreviewonelineclustertemplate
[link-azure-pricing-calculator]: https://azure.microsoft.com/pricing/calculator/
[link-azure-subscription]: https://azure.microsoft.com/free/
[link-vsts-account]: https://www.visualstudio.com/team-services/pricing/
[link-azure-sql]: /azure/sql-database/

[image-web-preview]: media/service-fabric-host-app-in-a-container/fabrikam-web-sample.png
[image-source-control]: media/service-fabric-host-app-in-a-container/add-to-source-control.png
[image-publish-repo]: media/service-fabric-host-app-in-a-container/publish-repo.png
[image-setup-ci]: media/service-fabric-host-app-in-a-container/configure-continuous-integration.png
