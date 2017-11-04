---
title: "Azure Service Fabric bir kapsayıcıda .NET uygulaması dağıtma | Microsoft Docs"
description: "Docker kapsayıcısı içinde Visual Studio'da .NET uygulaması paketlemek öğretir. Bu yeni \"kapsayıcı\" uygulaması, ardından bir Service Fabric kümesi dağıtılır."
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/19/2017
ms.author: mikhegn
ms.openlocfilehash: 021c695a91ff46274b2a5174918711d04bcff239
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-a-net-application-in-a-windows-container-to-azure-service-fabric"></a>Azure Service Fabric Windows kapsayıcısında .NET uygulaması dağıtma

Bu öğretici, Azure üzerinde bir Windows kapsayıcıda mevcut bir ASP.NET uygulamasını dağıtma gösterir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Visual Studio'da bir Docker projesi oluşturma
> * Varolan bir uygulama containerize
> * Visual Studio ve VSTS sürekli tümleştirme Kurulumu

## <a name="prerequisites"></a>Ön koşullar

1. Yükleme [Docker Windows CE](https://store.docker.com/editions/community/docker-ce-desktop-windows?tab=description) kapsayıcıları üzerinde Windows 10 çalıştırabilmeniz için.
2. İle öğrenmeniz [Windows 10 kapsayıcıları quickstart][link-container-quickstart].
3. Karşıdan [Fabrikam Fiber CallCenter] [ link-fabrikam-github] örnek uygulama.
4. Yükleme [Azure PowerShell][link-azure-powershell-install]
5. Yükleme [Visual Studio 2017 için sürekli teslim araçları uzantısı][link-visualstudio-cd-extension]
6. Oluşturma bir [Azure aboneliği] [ link-azure-subscription] ve [Visual Studio Team Services hesabı][link-vsts-account]. 
7. [Azure üzerinde bir küme oluşturun](service-fabric-tutorial-create-cluster-azure-ps.md)

## <a name="containerize-the-application"></a>Uygulama containerize

Sahip olduğunuza göre bir [Service Fabric kümesi Azure'da çalışan](service-fabric-tutorial-create-cluster-azure-ps.md) oluşturup kapsayıcılı uygulama dağıtmak hazır olursunuz. Uygulamamız bir kapsayıcıda çalışan başlatmak için eklemek ihtiyacımız **Docker Destek** Visual Studio'da projeye. Eklediğinizde **Docker Destek** uygulamaya iki şey olur. İlk olarak, bir _Dockerfile_ projeye eklenir. Bu yeni dosya nasıl kapsayıcı görüntünün oluşturulması açıklanmaktadır. Ardından ikinci, yeni bir _docker compose'u_ proje çözüme eklenir. Yeni Proje birkaç içeren dosyaları docker compose'u. Docker compose'u dosyaları kapsayıcı nasıl yürütüleceğini tanımlamak için kullanılabilir.

İle çalışma hakkında daha fazla bilgi [Visual Studio kapsayıcı Araçları][link-visualstudio-container-tools].

>[!NOTE]
>Windows kapsayıcı görüntülerini bilgisayarınızda çalıştırdığınız ilk kez ise, Docker CE kapsayıcılarınızı temel görüntülerde aşağı çekmek gerekir. Bu öğreticide kullanılan görüntüleri 14 GB bağımsızdır. Bir tane temel görüntüleri çıkarmak için terminal aşağıdaki komutu çalıştırın:
>```cmd
>docker pull microsoft/mssql-server-windows-developer
>docker pull microsoft/aspnet:4.6.2
>```

### <a name="add-docker-support"></a>Docker desteği ekleme

Açık [FabrikamFiber.CallCenter.sln] [ link-fabrikam-github] dosyasını Visual Studio'da.

Sağ **FabrikamFiber.Web** Proje > **Ekle** > **Docker Destek**.

### <a name="add-support-for-sql"></a>SQL desteği ekleme

Bu uygulama, bir SQL server uygulamayı çalıştırmak için gerekli olacak şekilde SQL veri sağlayıcısı olarak kullanır. Bizim docker compose.override.yml dosyasındaki bir SQL Server kapsayıcı yansıma başvuru.

Visual Studio'da açın **Çözüm Gezgini**, bulma **docker compose'u**ve dosyayı açmak **docker compose.override.yml**.

Gidin `services:` düğümü, adlandırılmış bir düğüm eklemek `db:` kapsayıcısı için SQL Server giriş tanımlar.

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
>Ana bilgisayardan erişilebilir olduğu müddetçe, yerel hata ayıklama için tercih ettiğiniz herhangi bir SQL sunucusu kullanabilirsiniz. Ancak, **localdb** desteklemediği `container -> host` iletişim.

>[!WARNING]
>SQL Server çalıştıran bir kapsayıcıda kalıcı veri desteklemiyor. Kapsayıcı sona erdiğinde, verileriniz silinir. Bu yapılandırma, üretim için kullanmayın.

Gidin `fabrikamfiber.web:` düğümü ve adlı bir alt düğüm eklemek `depends_on:`. Bu sağlar `db` hizmetini (SQL Server kapsayıcısı) web uygulamamız önce (fabrikamfiber.web) başlatır.

```yml
  fabrikamfiber.web:
    depends_on:
      - db
```

### <a name="update-the-web-config"></a>Güncelleştirme web yapılandırma

Geri **FabrikamFiber.Web** proje, bağlantı dizesinde güncelleştirme **web.config** kapsayıcısında SQL sunucusunu işaret edecek şekilde dosya.

```xml
<add name="FabrikamFiber-Express" connectionString="Data Source=db,1433;Database=FabrikamFiber;User Id=sa;Password=Password1;MultipleActiveResultSets=True" providerName="System.Data.SqlClient" />

<add name="FabrikamFiber-DataWarehouse" connectionString="Data Source=db,1433;Database=FabrikamFiber;User Id=sa;Password=Password1;MultipleActiveResultSets=True" providerName="System.Data.SqlClient" />
```

>[!NOTE]
>Farklı bir kullanmak istiyorsanız, bir yayın oluştururken, SQL Server web uygulamanızın veya derleme, başka bir bağlantı dizesi web.release.config dosyanıza ekleyin.

### <a name="test-your-container"></a>Test, kapsayıcısı

Tuşuna **F5** çalıştırıp, kapsayıcı uygulamada hata ayıklama için.

Edge iç NAT ağdaki (genellikle 172.x.x.x) kapsayıcı IP adresini kullanarak uygulamanızın tanımlanmış başlatma sayfasını açar. Visual Studio 2017 kullanarak kapsayıcıları uygulamalarında hata ayıklama hakkında daha fazla bilgi için bkz: [bu makalede][link-debug-container].

![bir kapsayıcıda fabrikam örneği][image-web-preview]

Kapsayıcı şimdi oluşturulur ve bir Service Fabric uygulaması paketlenmiş hazırdır. Makinenizde yerleşik kapsayıcı görüntü olduktan sonra kapsayıcı kayıt defterine itme ve çalıştırmak için herhangi bir ana aşağıya doğru çekme.

## <a name="get-the-application-ready-for-the-cloud"></a>Uygulamayı bulut için hazırlanma

Uygulama Azure Service Fabric içinde çalıştırmak için hazır hale getirmek için iki adımı tamamlamak ihtiyacımız var:

1. Burada web uygulamamız Service Fabric kümesindeki ulaşabilmesi istiyoruz bağlantı noktasını kullanıma sunar.
2. Bir üretim hazır SQL veritabanı uygulamamız için sağlayın.

### <a name="expose-the-port-for-the-app"></a>Uygulama için bağlantı noktası kullanıma sunma
Yapılandırılmış, Service Fabric kümesi olan bağlantı noktası *80* açık Azure yük dengeleyici varsayılan olarak, kümeye gelen trafiği dengeler. Biz bu bağlantı noktasında bizim kapsayıcı bizim docker-compose.yml dosyası getirebilir.

Visual Studio'da açın **Çözüm Gezgini**, bulma **docker compose'u**ve dosyayı açmak **docker-compose.yml**.

Değiştirme `fabrikamfiber.web:` düğümü adlı bir alt düğüm eklemek `ports:`.

Bir dize girişi ekleme `- "80:80"`. Bu, docker-compose.yml dosyası aşağıdaki gibi görünmelidir.

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

### <a name="use-a-production-sql-database"></a>Bir üretim SQL veritabanını kullan
Üretimde çalıştırırken, verilerimizi ihtiyacımız bizim veritabanında kalıcı. Şu anda bir kapsayıcıda kalıcı veri güvence altına almak için bir yolu yoktur, bu nedenle bir kapsayıcıda SQL Server'daki üretim verileri depolanamıyor.

Bir Azure SQL veritabanı kullanan öneririz. Ayarlama ve yönetilen bir SQL Server Azure'da çalışması için ziyaret edin [Azure SQL veritabanı hızlı başlangıç ipuçları] [ link-azure-sql] makalesi.

>[!NOTE]
>SQL server için bağlantı dizelerini değiştirmeyi unutmayın **web.release.config** dosyasını **FabrikamFiber.Web** projesi.
>
>SQL veritabanı erişilebilir olduğundan, bu uygulama düzgün biçimde başarısız olur. Devam edin ve SQL server ile uygulama dağıtmak seçebilirsiniz.

## <a name="deploy-with-visual-studio-team-services"></a>Visual Studio Team Services ile dağıtma

Visual Studio Team Services kullanarak dağıtımı ayarlamak için yüklemeniz gerekir [sürekli teslim araçları uzantısı için Visual Studio 2017][link-visualstudio-cd-extension]. Bu uzantıyı Visual Studio Team Services yapılandırarak Azure'a dağıtma ve, Service Fabric kümesi dağıtılmış uygulamanızı alma daha kolay hale getirir.

Başlamak için kaynak denetiminde kodunuzun barındırılması gerekir. Bu bölümde rest varsayar **git** kullanılıyor.

### <a name="set-up-a-vsts-repo"></a>VSTS depodaki ayarlayın
Visual Studio sağ alt köşesinde tıklatın **eklemek için kaynak denetimi** > **Git** (veya hangi seçeneği tercih).

![Kaynak denetimi düğmesine basın][image-source-control]

İçinde _Takım Gezgini_ bölmesi, basın **yayımlama Git deposuna**.

VSTS havuz adı ve tuşuna seçin **depo**.

![Depodaki VSTS yayımlama][image-publish-repo]

Kodunuzu VSTS kaynak deposu ile eşitlenir, sürekli tümleştirme ve kesintisiz teslim yapılandırabilirsiniz.

### <a name="setup-continuous-delivery"></a>Kurulum kesintisiz teslim

İçinde _Çözüm Gezgini_, sağ **çözüm** > **yapılandırma kesintisiz teslim**.

Azure aboneliğini seçin.

Ayarlama **ana bilgisayar türü** için **Service Fabric kümesi**.

Ayarlama **hedef ana bilgisayarın** önceki bölümde oluşturduğunuz service fabric kümesi için.

Seçin bir **kapsayıcı kayıt defteri** , kapsayıcıya yayımlamak için.

>[!TIP]
>Kullanım **Düzenle** kapsayıcı kayıt oluşturmak üzere düğmesi.

**Tamam**'a basın.

![Kurulum service fabric sürekli tümleştirme][image-setup-ci]
   
   Yapılandırma tamamlandıktan sonra kapsayıcı için Service Fabric dağıtılır. Her değiştiğinde, anında iletme depoya yeni bir yapı güncelleştirir ve yayın yürütülür.
   
   >[!NOTE]
   >Kapsayıcı görüntüleri alma yaklaşık 15 dakika oluşturma.
   >Service Fabric kümesi ilk dağıtımda yüklenmek üzere temel Windows Server Core kapsayıcı görüntüleri neden olur. Yüklemeyi tamamlamak için ek 5-10 dakika sürer.

Kümenizin URL'yi kullanarak Fabrikam çağrı merkezi uygulaması'na göz: Örneğin, *http://mycluster.westeurope.cloudapp.azure.com*

Kapsayıcılı ve Fabrikam çağrı merkezi çözümü dağıtılmış göre açabilirsiniz [Azure portal] [ link-azure-portal] ve Service Fabric içinde çalışan uygulama bakın. Uygulama denemek için bir web tarayıcısı açın ve Service Fabric kümesi URL'sine gidin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Visual Studio'da bir Docker projesi oluşturma
> * Varolan bir uygulama containerize
> * Visual Studio ve VSTS sürekli tümleştirme Kurulumu

Öğreticinin sonraki bölümünde nasıl ayarlanacağını öğrenin [kapsayıcısı için izleme](service-fabric-tutorial-monitoring-wincontainers.md).

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
[link-azure-pricing-calculator]: https://azure.microsoft.com/en-us/pricing/calculator/
[link-azure-subscription]: https://azure.microsoft.com/en-us/free/
[link-vsts-account]: https://www.visualstudio.com/team-services/pricing/
[link-azure-sql]: /azure/sql-database/

[image-web-preview]: media/service-fabric-host-app-in-a-container/fabrikam-web-sample.png
[image-source-control]: media/service-fabric-host-app-in-a-container/add-to-source-control.png
[image-publish-repo]: media/service-fabric-host-app-in-a-container/publish-repo.png
[image-setup-ci]: media/service-fabric-host-app-in-a-container/configure-continuous-integration.png
