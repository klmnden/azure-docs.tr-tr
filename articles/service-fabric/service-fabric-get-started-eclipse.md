---
title: "Azure Service Fabric için Eclipse Eklentisi ile Çalışmaya Başlama | Microsoft Belgeleri"
description: "Azure Service Fabric için Eclipse Eklentisi ile Çalışmaya Başlama."
services: service-fabric
documentationcenter: java
author: sayantancs
manager: timlt
editor: 
ms.assetid: bf84458f-4b87-4de1-9844-19909e368deb
ms.service: service-fabric
ms.devlang: java
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/27/2016
ms.author: saysa
translationtype: Human Translation
ms.sourcegitcommit: 24d86e17a063164c31c312685c0742ec4a5c2f1b
ms.openlocfilehash: 891df38aa685cac7bbfecb71b15c88d557ac493b
ms.lasthandoff: 03/11/2017


---

# <a name="getting-started-with-eclipse-plugin-for-service-fabric-java-application-development"></a>Service Fabric Java uygulama geliştirme için Eclipse Eklentisi ile çalışmaya başlama
Eclipse, Java Geliştiricileri için en fazla kullanılan IDE’lerden biridir. Bu makalede, Service Fabric ile çalışmak için Eclipse geliştirme ortamınızı ayarlama işlemi ele alınmaktadır. Bu makale, eklentiyi kurmanıza, Service fabric uygulamaları oluşturup Service Fabric uygulamanızı yerel veya uzak bir Service Fabric kümesine dağıtmanıza yardımcı olur.

## <a name="install-or-update-service-fabric-plugin-on-eclipse-neon"></a>Eclipse Neon’da Service Fabric Eklentisi yükleme veya güncelleştirme
Service Fabric, **Java Geliştiricileri için Eclipse IDE**’ye yönelik Java hizmetlerini derleyip dağıtmayı kolaylaştırabilen bir eklenti sağlar.

1. En yeni Eclipse **Neon** ve en yeni Buildship sürümü (1.0.17) veya sonraki bir sürümün yüklü olduğundan emin olun. ``Help => Installation Details`` öğesini seçerek yüklü bileşenlerin sürümlerini denetleyebilirsiniz. Buildship'i güncelleştirmek için [buradaki][buildship-update] yönergeleri kullanabilirsiniz. Eclipse Neon’un en son sürümde olup olmadığını denetlemek ve sürümü güncelleştirmek için ``Help => Check for Updates`` sayfasına gidebilirsiniz.

2. Service Fabric eklentisini yüklemek için ``Help => Install New Software...`` öğesini seçin.
  1. "Birlikte çalış" metin kutusuna şunu girin: ``http://dl.windowsazure.com/eclipse/servicefabric``.
  2. Ekle'ye tıklayın.

  ![Service Fabric için Eclipse Neon eklentisi][sf-eclipse-plugin-install]

  3. Service Fabric eklentisini seçin ve İleri’ye tıklayın.
  4. Yükleme işlemine devam edin ve son kullanıcı lisans sözleşmesini kabul edin.

Service Fabric Eclipse eklentisi zaten yüklüyse, en yeni sürümü kullandığınızdan emin olun. ``Help => Installation Details`` konumuna giderek daha fazla güncelleştirilebilir olup olmadığını kontrol edebilirsiniz. Daha sonra yüklenen eklentiler listesinde Service Fabric’i arayın ve Güncelleştir’e tıklayın. Bekleyen bir güncelleştirme varsa alınır ve yüklenir.

> [!NOTE]
> Eclipse programında Service Fabric Eclipse eklentisinin yüklenmesi veya güncelleştirilmesi uzun sürüyorsa, bunun nedeni Eclipse’in Eclipse örneğinize kaydedilmiş tüm güncelleştirme sitelerinde gerçekleşen tüm yeni değişikliklere ait meta verileri getirmeye çalışmasıdır. Bu nedenle, işlemi hızlandırmak için şu küçük ipucunu kullanabilirsiniz: `Available Software Sites` sayfasına gidip `http://dl.windowsazure.com/eclipse/servicefabric` Service Fabric eklenti konumunu işaret eden dışındaki tüm işaretleri kaldırın.
>

## <a name="create-service-fabric-application-using-eclipse"></a>Eclipse kullanarak Service Fabric uygulaması oluşturma

1. ``File => New => Other`` kısmına gidin. ``Service Fabric Project`` öğesini seçin. ``Next`` öğesine tıklayın.

    ![Service Fabric Yeni Proje Sayfa 1][create-application/p1]

2. Projenize bir ad verin. ``Next`` öğesine tıklayın.

    ![Service Fabric Yeni Proje Sayfa 2][create-application/p2]

3. Kullanılabilir şablon kümesinden Hizmet Şablonunu seçin (Aktör, Durum Bilgisiz, Kapsayıcı veya Konuk tarafından yürütülebilir). ``Next`` öğesine tıklayın.

    ![Service Fabric Yeni Proje Sayfa 3][create-application/p3]

4. Bu sayfaya Hizmet Adı ve/veya ilgili Hizmet ayrıntılarını girip ``Finish`` öğesine tıklayın.

    ![Service Fabric Yeni Proje Sayfa 4][create-application/p4]

5. İlk Service Fabric projenizi oluşturduğunuzda, Service Fabric perspektifini ayarlamak isteyip istemediğiniz sorulur; devam etmek için lütfen ``yes`` öğesini seçin.

    ![Service Fabric Yeni Proje Sayfa 5][create-application/p5]

6. Başarılı bir şekilde oluşturulduktan sonra proje şu şekilde görünür -

    ![Service Fabric Yeni Proje Sayfa 6][create-application/p6]

## <a name="build-and-deploy-the-service-fabric-application-using-eclipse"></a>Eclipse kullanarak Service Fabric uygulaması derleme ve dağıtma

* Yukarıda yeni oluşturduğunuz Service Fabric uygulamasına sağ tıklayın. Bağlam menüsünden ``Service Fabric`` seçeneğini belirleyin. Bunun yapılması, birden fazla seçenek içeren bir alt menü getirir. şunun gibi görünür -

    ![Service Fabric Sağ Tıklama Menüsü][publish/RightClick]

  Derleme, yeniden derleme ve temizleme seçeneklerine tıkladıktan sonra istediğiniz eylemleri gerçekleştirir.
  - ``Build Application`` uygulamayı temizlemeden derler
  - ``Rebuild Application`` uygulamanın temiz-derlemesini gerçekleştirir
  - ``Clean Application`` uygulamayı derlenen yapıtlardan temizler


* Bu menüden uygulamanızı dağıtmayı, dağıtımını kaldırmayı ve yayımlamayı da seçebilirsiniz.
  - ``Deploy Application`` yerel kümenize dağıtır
  - ``Publish Application...``, ``Local.json`` ile ``Cloud.json`` arasından seçmek istediğiniz yayımlama profilini sorar. Bu JSON dosyaları, yerel veya bulut (Azure) kümesine bağlanmak için gereken bilgileri (örneğin, bağlantı uç noktaları ve güvenlik bilgileri) depolamak için kullanılır.

  ![Service Fabric Sağ Tıklama Menüsü][publish/Publish]

* Service Fabric uygulamanızı Eclipse Çalıştırma Yapılandırmaları kullanarak dağıtabileceğiniz alternatif bir yöntem mevcuttur.

  1. ``Run => Run Configurations`` öğesini seçin. ``Grade Project`` altındaki ``ServiceFabricDeployer`` çalıştırma yapılandırmasını seçin.
  2. Sağ bölmedeki ``Arguments`` sekmesinin altında ``publishProfile`` olarak **yerel** veya **bulut** seçeneğini belirtin. Varsayılan olarak **yerel** seçilidir. Uzak/bulut kümeye dağıtmak için **bulut** seçeneğini belirleyin.
  3. Duruma göre `Local.json` veya `Cloud.json` öğesine düzenleyerek yayımlama profillerine, varsa uç nokta bilgileri ve güvenlik kimlik bilgileri ile birlikte doğru bilgilerin eklendiğinden emin olun.
  4. ``Grade Project`` altındaki sağ bölmede bulunan ``Working Directory`` öğesinin, dağıtmak istediğiniz uygulamayı işaret ettiğinden emin olun. Aksi durumda, yalnızca ``Workspace...`` düğmesine tıklayın ve istediğiniz uygulamayı seçin.
  5. **Uygula** ve **Çalıştır**’a tıklayın.

Uygulamanız birkaç dakika içinde derlenip dağıtılır. Durumunu Service Fabric Explorer’dan izleyebilirsiniz.  

## <a name="add-new-service-fabric-service-to-your-service-fabric-application"></a>Service Fabric uygulamanıza yeni Service Fabric hizmeti ekleme

Aşağıdaki adımlar kullanılarak, mevcut bir Service Fabric uygulamasına yeni bir Service Fabric hizmeti eklenebilir:

1. Hizmet eklemek istediğiniz projeye sağ tıklayarak bağlam menüsünü açın ve 'Service Fabric' seçeneğini belirleyin. Bunun yapılması, birden fazla seçenek içeren bir alt menü getirir.

    ![Service Fabric Hizmet Ekleme sayfa 1][add-service/p1]

2. `Add ServiceFabric Service` seçeneğini belirlediğinizde, sizi projeye hizmet eklemek için sonraki adımlara yönlendirir.
3. Projenize eklemek istediğiniz Hizmet şablonunu seçip 'İleri'’ye tıklayın.

    ![Service Fabric Hizmet Ekleme sayfa 2][add-service/p2]

4. Hizmet Adı (ve gerektiğinde diğer gerekli bilgileri) giriş aşağıdaki "Hizmet Ekle" düğmesine tıklayın.  

    ![Service Fabric Hizmet Ekleme sayfa 3][add-service/p3]

5. Hizmet başarıyla eklendikten sonra tüm proje yapısı aşağıdaki gibi görünür -

    ![Service Fabric Hizmet Ekleme sayfa 4][add-service/p4]

## <a name="upgrade-your-service-fabric-java-application"></a>Service Fabric Java uygulamanızı yükseltme

Service Fabric Eclipse eklentinizi kullanarak ``App1`` projesini oluşturduğunuzu ve uygulama türü ``App1AppicationType``, uygulama sürümü 1.0 olan ``fabric:/App1Application`` bir uygulama oluşturmak üzere eklentiyi kullanarak dağıttığınızı varsayalım. Şimdi de uygulamanızı kesinti olmadan yükseltmek istiyorsunuz.

Uygulamanızda değişikliği yapın ve değiştirilen hizmeti yeniden derleyin.  Değiştirilen hizmetin bildirim dosyasını (``ServiceManifest.xml``) hizmetin güncel sürümleriyle (ve uygun olan Kod veya Yapılandırma ya da Veriler ile) güncelleştirin. Uygulamanın bildirim dosyasını (``ApplicationManifest.xml``), uygulamanın ve değiştirilen hizmetin güncel sürüm numarasıyla da değiştirin.  

Eclipse kullanarak uygulamanızı yükseltmek için, yinelenen bir çalıştırma yapılandırması oluşturabilir ve aşağıdaki adımları uygulayarak gerektiğinde uygulamanızı yükseltmek için bu yapılandırmayı kullanabilirsiniz -
1. ``Run => Run Configurations`` öğesini seçin. Sol bölmedeki ``Grade Project`` öğesinin sol tarafında bulunan küçük oka tıklayın.
2. ``ServiceFabricDeployer`` öğesine sağ tıklayıp ``Duplicate`` öğesini seçin. Bu yapılandırmaya yeni bir ad verip ``ServiceFabricUpgrader`` deyin.
3. Sağ paneldeki ``Arguments`` sekmesi altında ``-Pconfig='deploy'`` değerini ``-Pconfig=upgrade`` ile değiştirip ``Apply`` öğesine tıklayın.
4. Bu durumda, istediğinizde ``Run`` yapabileceğiniz bir çalıştırma yapılandırması oluşturup uygulamanızı yükseltmek için kaydettiniz. Bu işlem, en son güncelleştirilmiş uygulama türü sürümünü uygulama bildirim dosyasından alır.

Bundan böyle uygulama yükseltmesini Service Fabric Explorer’dan izleyebilirsiniz. Birkaç dakika içinde uygulamanız güncelleştirilmiş olur.

<!-- Images -->

[sf-eclipse-plugin-install]: ./media/service-fabric-get-started-mac/sf-eclipse-plugin-install.png

[create-application/p1]:./media/service-fabric-get-started-eclipse/create-application/p1.png
[create-application/p2]:./media/service-fabric-get-started-eclipse/create-application/p2.png
[create-application/p3]:./media/service-fabric-get-started-eclipse/create-application/p3.png
[create-application/p4]:./media/service-fabric-get-started-eclipse/create-application/p4.png
[create-application/p5]:./media/service-fabric-get-started-eclipse/create-application/p5.png
[create-application/p6]:./media/service-fabric-get-started-eclipse/create-application/p6.png

[publish/Publish]: ./media/service-fabric-get-started-eclipse/publish/Publish.png
[publish/RightClick]: ./media/service-fabric-get-started-eclipse/publish/RightClick.png

[add-service/p1]: ./media/service-fabric-get-started-eclipse/add-service/p1.png
[add-service/p2]: ./media/service-fabric-get-started-eclipse/add-service/p2.png
[add-service/p3]: ./media/service-fabric-get-started-eclipse/add-service/p3.png
[add-service/p4]: ./media/service-fabric-get-started-eclipse/add-service/p4.png

<!-- Links -->
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship

