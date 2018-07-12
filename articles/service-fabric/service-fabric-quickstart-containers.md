---
title: Azure'da Service Fabric üzerinde Windows kapsayıcı uygulaması oluşturma | Microsoft Docs
description: Bu hızlı başlangıçta, Azure Service Fabric üzerinde ilk Windows kapsayıcı uygulamanızı oluşturursunuz.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: vturecek
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: quickstart
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/30/2018
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 081b2be82b15c36566e8eb9fe4af0037804d0e7e
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37951204"
---
# <a name="quickstart-deploy-windows-containers-to-service-fabric"></a>Hızlı başlangıç: Windows kapsayıcıları Service Fabric'e dağıtma

Azure Service Fabric; ölçeklenebilir ve güvenilir mikro hizmetleri ve kapsayıcıları dağıtmayı ve yönetmeyi sağlayan bir dağıtılmış sistemler platformudur.

Bir Service Fabric kümesindeki Windows kapsayıcısında mevcut olan bir uygulamayı çalıştırmak için uygulamanızda herhangi bir değişiklik yapılması gerekmez. Bu hızlı başlangıç, Service Fabric uygulamasında önceden oluşturulmuş bir Docker kapsayıcısı görüntüsünü dağıtmayı gösterir. İşlemi tamamladığınızda, çalışan bir Windows Server 2016 Nano Server ve IIS kapsayıcısına sahip olacaksınız. Bu hızlı başlangıç, Windows kapsayıcısı dağıtmayı açıklar. Linux kapsayıcısı dağıtmak için [bu Hızlı Başlangıca](service-fabric-quickstart-containers-linux.md) bakın.

![IIS varsayılan web sayfası][iis-default]

Bu hızlı başlangıçta şunları yapmayı öğrenirsiniz:

* Docker görüntü kapsayıcısını paketleme
* İletişimi yapılandırma
* Service Fabric uygulamasını oluşturma ve paketleme
* Kapsayıcı uygulamasını Azure’a dağıtma

## <a name="prerequisites"></a>Ön koşullar

* Bir Azure aboneliği ([ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturabilirsiniz).
* Şunları çalıştıran bir geliştirme bilgisayarı:
  * Visual Studio 2015 veya Visual Studio 2017.
  * [Service Fabric SDK’sı ve araçları](service-fabric-get-started.md).

## <a name="package-a-docker-image-container-with-visual-studio"></a>Visual Studio ile Docker görüntü kapsayıcısını paketleme

Service Fabric SDK’sı ve araçları, bir kapsayıcıyı Service Fabric kümesine dağıtmanıza yardımcı olan bir hizmet şablonu sağlar.

Visual Studio'yu “Yönetici” olarak başlatın.  **Dosya** > **Yeni** > **Proje**’yi seçin.

**Service Fabric uygulaması**’nı seçin, "MyFirstContainer" olarak adlandırın ve **Tamam**’a tıklayın.

**Barındırılan Kapsayıcılar ve Uygulamalar** şablonlarından **Kapsayıcı**’yı seçin.

**Görüntü Adı** olarak "microsoft/iis:nanoserver" değerini [Windows Server Nano Server ve IIS temel görüntüsü](https://hub.docker.com/r/microsoft/iis/)'nü seçin.

80 numaralı bağlantı noktasında hizmete gelen isteklerin, kapsayıcı üzerindeki 80 numaralı bağlantı noktasıyla eşlenmesi için kapsayıcının bağlantı noktasından konağa bağlantı noktası eşlenmesini yapılandırın.  **Kapsayıcı Bağlantı Noktası**’nı "80" olarak ve **Ana Bilgisayar Bağlantı Noktası**’nı "80" olarak ayarlayın.  

Hizmeti "MyContainerService" olarak adlandırın ve **Tamam**’a tıklayın.

![Yeni hizmet iletişim kutusu][new-service]

## <a name="specify-the-os-build-for-your-container-image"></a>Kapsayıcı görüntünüz için OS derlemesini belirtme
Windows Server'ın belirli bir sürümüyle derlenen kapsayıcılar, Windows Server'ın farklı sürümünü çalıştıran bir konakta çalışmayabilir. Örneğin, Windows Server sürüm 1709 kullanarak derlenen kapsayıcılar Windows Server 2016 çalıştıran konaklarda çalışmaz. Daha fazla bilgi için bkz. [Windows Server kapsayıcı işletim sistemi ve ana bilgisayar işletim sistemi uyumluluğu](service-fabric-get-started-containers.md#windows-server-container-os-and-host-os-compatibility). 

Service Fabric çalışma zamanının sürüm 6.1 veya daha yeni bir sürümüyle, kapsayıcı başına birden çok işletim sistemi görüntüsü belirtebilir ve her birini dağıtılacağı işletim sisteminin derleme sürümüyle etiketleyebilirsiniz. Bu, uygulamanızın Windows işletim sisteminin farklı sürümlerini çalıştıran konaklar arasında çalıştırılabilmesine yardımcı olur. Daha fazla bilgi edinmek için bkz. [İşletim sistemi derlemesine özgü kapsayıcı görüntüleri belirtme](service-fabric-get-started-containers.md#specify-os-build-specific-container-images). 

Microsoft, Windows Server'ın farklı sürümleri üzerinde oluşturulmuş IIS sürümleri için farklı görüntüler yayımlar. Service Fabric'in, uygulamanızın dağıtıldığı küme düğümlerinde çalıştırılan Windows Server sürümüyle uyumlu bir kapsayıcı dağıttığından emin olmak için, *ApplicationManifest.xml* dosyasına aşağıdaki satırları ekleyin. WIndows Server 2016 için derleme sürümü 14393, Windows Server 1709 sürümü için derleme sürümü 16299’dur. 

```xml
    <ContainerHostPolicies CodePackageRef="Code"> 
      <ImageOverrides> 
        ...
          <Image Name="microsoft/iis:nanoserverDefault" /> 
          <Image Name= "microsoft/iis:nanoserver" Os="14393" /> 
          <Image Name="microsoft/iis:windowsservercore-1709" Os="16299" /> 
      </ImageOverrides> 
    </ContainerHostPolicies> 
```

Hizmet bildirimi, `microsoft/iis:nanoserver` nano sunucusu için tek bir görüntü belirtmeye devam eder. 

## <a name="create-a-cluster"></a>Küme oluşturma

Uygulamayı Azure’daki bir kümeye dağıtmak için bir grup kümesine katılabilirsiniz. Grup kümeleri, Azure üzerinde barındırılan ve Service Fabric ekibi tarafından sunulan ücretsiz, sınırlı süreli Service Fabric kümeleridir. Bu kümelerde herkes uygulama dağıtabilir ve platform hakkında bilgi edinebilir.  Küme, düğümden düğüme ve istemciden düğüme güvenlik için tek bir otomatik olarak imzalanan sertifika kullanır. Grup kümeleri, kapsayıcıları destekler. Kendi kümenizi ayarlamaya ve kullanmaya karar verirseniz küme, kapsayıcıları destekleyen bir SKU’da (örn. Kapsayıcılar’ı içeren Windows Server 2016 Datacenter) çalışıyor olmalıdır.

Oturum açın ve [bir Windows kümesine katılın](http://aka.ms/tryservicefabric). **PFX** bağlantısına tıklayarak PFX sertifikasını bilgisayarınıza indirin. **Güvenli Grup kümesine bağlanma** bağlantısına tıklayın ve sertifika parolasını kopyalayın. Aşağıdaki adımlarda sertifika, sertifika parolası ve **Bağlantı uç noktası** değeri kullanılır.

![PFX ve bağlantı uç noktası](./media/service-fabric-quickstart-containers/party-cluster-cert.png)

> [!Note]
> Saat başına sınırlı sayıda Grup kümesi vardır. Bir Grup kümesine kaydolmaya çalıştığınızda hata alırsanız bir süre bekleyebilir ve tekrar deneyebilirsiniz veya [.NET uygulaması dağıtma](https://docs.microsoft.com/azure/service-fabric/service-fabric-tutorial-deploy-app-to-party-cluster#deploy-the-sample-application) öğreticisindeki adımları izleyerek Azure aboneliğinizde bir Service Fabric kümesi oluşturabilir ve bu kümede uygulamayı dağıtabilirsiniz. Visual Studio aracılığıyla oluşturulan küme, kapsayıcıları destekler. Kümenizde uygulamayı dağıtıp doğruladıktan sonra, bu hızlı başlangıçtaki [Örnek Service Fabric uygulama ve hizmet bildirimlerini tamamlama](#complete-example-service-fabric-application-and-service-manifests) kısmına atlayabilirsiniz.
>

Bir Windows bilgisayarda PFX’i *CurrentUser\My* sertifika deposuna yükleyin.

```powershell
PS C:\mycertificates> Import-PfxCertificate -FilePath .\party-cluster-873689604-client-cert.pfx -CertStoreLocation Cert:\CurrentUser\My -Password (ConvertTo-SecureString 873689604 -AsPlainText -Force)


  PSParentPath: Microsoft.PowerShell.Security\Certificate::CurrentUser\My

Thumbprint                                Subject
----------                                -------
3B138D84C077C292579BA35E4410634E164075CD  CN=zwin7fh14scd.westus.cloudapp.azure.com
```

Sonraki adım için parmak izini unutmayın.

## <a name="deploy-the-application-to-azure-using-visual-studio"></a>Visual Studio kullanarak uygulamayı Azure’a dağıtma

Uygulama hazır olduğuna göre, doğrudan Visual Studio'dan bir kümeye dağıtabilirsiniz.

Çözüm Gezgini'nde **MyFirstContainer**’a sağ tıklayın ve **Yayımla**’yı seçin. Yayımla iletişim kutusu görüntülenir.

Grup kümesi sayfasındaki **Bağlantı Uç Noktası**'nı **Bağlantı Uç Noktası** alanına kopyalayın. Örneğin, `zwin7fh14scd.westus.cloudapp.azure.com:19000`. **Gelişmiş Bağlantı Parametrelerine** tıklayıp bağlantı parametresi bilgilerini doğrulayın.  *FindValue* ve *ServerCertThumbprint* değerleri önceki adımda yüklenen sertifikanın parmak iziyle eşleşmelidir.

![Yayımla İletişim Kutusu](./media/service-fabric-quickstart-containers/publish-app.png)

**Yayımla**’ta tıklayın.

Kümedeki her uygulamanın benzersiz bir adı olmalıdır.  Bununla birlikte grup kümeleri ortak, paylaşılan bir ortamdır ve mevcut uygulamalardan biriyle çakışma olabilir.  Ad çakışması varsa, Visual Studio projesini yeniden adlandırın ve bir kez daha dağıtın.

Bir tarayıcıyı açıp Grup kümesi sayfasında belirtilen **Bağlantı uç noktasına** gidin. İsteğe bağlı olarak, URL’nin başına düzen tanımlayıcısını (`http://`) ve sonuna bağlantı noktasını (`:80`) ekleyebilirsiniz. Örneğin, http://zwin7fh14scd.westus.cloudapp.azure.com:80. IIS varsayılan web sayfasını görmeniz gerekir: ![IIS varsayılan web sayfası][iis-default]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta şunları öğrendiniz:

* Docker görüntü kapsayıcısını paketleme
* İletişimi yapılandırma
* Service Fabric uygulamasını oluşturma ve paketleme
* Kapsayıcı uygulamasını Azure’a dağıtma

Service Fabric’te Windows kapsayıcılarıyla çalışma hakkında daha fazla bilgi için Windows kapsayıcı uygulamaları öğreticisiyle devam edin.

> [!div class="nextstepaction"]
> [Windows kapsayıcı uygulaması oluşturma](./service-fabric-host-app-in-a-container.md)

[iis-default]: ./media/service-fabric-quickstart-containers/iis-default.png
[publish-dialog]: ./media/service-fabric-quickstart-containers/publish-dialog.png
[new-service]: ./media/service-fabric-quickstart-containers/NewService.png
