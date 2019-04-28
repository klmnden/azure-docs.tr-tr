---
title: Eclipse için Azure Service Fabric eklentisi | Microsoft Docs
description: Eclipse için Service Fabric eklentisini kullanmaya başlayın.
services: service-fabric
documentationcenter: java
author: rapatchi
manager: chackdan
editor: ''
ms.assetid: bf84458f-4b87-4de1-9844-19909e368deb
ms.service: service-fabric
ms.devlang: java
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/06/2018
ms.author: rapatchi
ms.openlocfilehash: c33ecce5610dbef0dce13aa95f04ae4f0620603b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60950372"
---
# <a name="service-fabric-plug-in-for-eclipse-java-application-development"></a>Eclipse Java uygulama geliştirmesi için Service Fabric eklentisi
Eclipse, Java geliştiricileri için en yaygın kullanılan tümleşik geliştirme ortamlarından (IDE’ler) biridir. Bu makalede, Azure Service Fabric ile çalışmak için Eclipse geliştirme ortamınızı ayarlama işlemi ele alınmaktadır. Service Fabric eklentisini yükleme, Service fabric uygulaması oluşturma ve Service Fabric uygulamanızı Eclipse’teki yerel veya uzak bir Service Fabric kümesine dağıtma hakkında bilgi edinin. 

> [!NOTE]
> Eclipse eklentisi Windows üzerinde şu anda desteklenmemektedir. 

## <a name="install-or-update-the-service-fabric-plug-in-in-eclipse"></a>Eclipse’te Service Fabric eklentisi yükleme veya güncelleştirme
Eclipse'te Service Fabric eklentisi yükleyebilirsiniz. Eklenti, Java hizmetleri oluşturup dağıtma işlemini kolaylaştırmaya yardımcı olur.

> [!IMPORTANT]
> Service Fabric eklentisi için Eclipse Neon veya üzeri bir sürüm gerekir. Eclipse sürümünüzün nasıl denetleneceği hakkında bu nottan sonraki yönergelere bakın. Eclipse’in önceki bir sürümünü yüklediyseniz, [Eclipse sitesinden](https://www.eclipse.org) daha yeni sürümleri indirebilirsiniz. Mevcut bir Eclipse yüklemesinin üzerine yüklemeniz (üzerine yazmanız) önerilmez. Yükleyiciyi çalıştırmadan önce kaldırabilir veya yeni sürümü farklı bir dizine yükleyebilirsiniz. 
> 
> Ubuntu üzerinde, paket yükleyici (`apt` veya `apt-get`) kullanmak yerine doğrudan Eclipse sitesinden yükleme yapılmasını öneririz. Böylece, Eclipse’in en güncel sürümünü elde etmeniz sağlanır. 

[Eclipse sitesinden](https://www.eclipse.org) Eclipse Neon veya sonraki bir sürümü yükleyin.  Ayrıca Buildship 2.2.1 veya sonraki bir sürümü de yükleyin (Service Fabric eklentisi, eski Buildship sürümleriyle uyumlu değildir):
-   Yüklü bileşenlerin sürümlerini denetlemek için Eclipse’te **Yardım** > **Eclipse Hakkında** > **Yükleme Ayrıntıları** seçeneğine gidin.
-   Buildship'i güncelleştirmek için bkz: [Eclipse Buildship: Gradle için Eclipse eklentileri][buildship-update].
-   Eclipse güncelleştirmelerini denetleyip yüklemek için **Yardım** > **Güncelleştirmeleri Denetle** seçeneğine gidin.

Service Fabric eklentisini yüklemek için **Yardım** > **Yeni Yazılım Yükle** seçeneğine gidin.
1. İçinde **çalışmak** kutusuna, https girin:\//dl.microsoft.com/eclipse.
2. **Ekle**'ye tıklayın.

   ![Eclipse için Service Fabric eklentisi][sf-eclipse-plugin-install]
3. Service Fabric eklentisini seçip **İleri**’ye tıklayın.
4. Yükleme adımlarını tamamlayın ve ardından Microsoft Yazılım Lisans Koşulları’nı kabul edin.
  
Service Fabric eklentisi zaten yüklüyse, en yeni sürümü yükleyin. 
1. Kullanılabilir güncelleştirmeleri denetlemek için **Yardım** > **Eclipse Hakkında** > **Yükleme Ayrıntıları** seçeneğine gidin. 
2. Yüklü eklentiler listesinde Service Fabric’i seçip **Güncelleştir**’e tıklayın. Kullanılabilir güncelleştirmeler yüklenir.
3. Service Fabric eklentisini güncelleştirdikten sonra Gradle projesini de yenileyin.  **build.gradle** öğesine sağ tıklayın ve **Yenile**’yi seçin.

> [!NOTE]
> Service Fabric eklentisi yavaş yükleniyor veya güncelleştiriliyorsa, bunun nedeni bir Eclipse ayarı olabilir. Eclipse, Eclipse örneğinize kaydedilmiş siteleri güncelleştirmek üzere tüm değişikliklere ait meta verileri toplar. Bir Service Fabric eklenti güncelleştirmesini denetleme ve yükleme işlemini hızlandırmak için **Kullanılabilir Yazılım Siteleri** bölümüne gidin. Service Fabric eklenti konumunu işaret eden site dışındaki tüm sitelerin onay kutularının işaretini kaldırın (https:\//dl.microsoft.com/eclipse/azure/servicefabric).

> [!NOTE]
>Eclipse Mac bilgisayarınızda beklendiği gibi çalışmıyorsa (veya süper kullanıcı olarak çalışmanızı gerektiriyorsa), **ECLIPSE_INSTALLATION_PATH** klasörüne ve ardından **Eclipse.app/Contents/MacOS** alt klasörüne gidin. `./eclipse` öğesini çalıştırarak Eclipse’i başlatın.


## <a name="create-a-service-fabric-application-in-eclipse"></a>Eclipse’te Service Fabric uygulaması oluşturma

1.  Eclipse’te **Dosya** > **Yeni** > **Diğer** seçeneğine gidin. **Service Fabric Projesi**’ni seçip **İleri**’ye tıklayın.

    ![Service Fabric Yeni Proje sayfa 1][create-application/p1]

2.  Projeniz için bir ad girip **İleri**’ye tıklayın.

    ![Service Fabric Yeni Proje sayfa 2][create-application/p2]

3.  Şablonlar listesinde **Hizmet Şablonu**’nu seçin. Hizmet şablonu türünüzü (Aktör, Durum Bilgisi Olmayan, Kapsayıcı veya Konuk İkili) seçip **İleri**’ye tıklayın.

    ![Service Fabric Yeni Proje sayfa 3][create-application/p3]

4.  Hizmet adını ve hizmet ayrıntılarını girip **Son**’a tıklayın.

    ![Service Fabric Yeni Proje sayfa 4][create-application/p4]

5. İlk Service Fabric projenizi oluşturduktan sonra, **İlişkili Perspektifi Aç** iletişim kutusunda **Evet**’e tıklayın.

    ![Service Fabric Yeni Proje sayfa 5][create-application/p5]

6.  Yeni projeniz şöyle görünür:

    ![Service Fabric Yeni Proje sayfa 6][create-application/p6]

## <a name="build-a-service-fabric-application-in-eclipse"></a>Eclipse'te Service Fabric uygulaması derleme

1.  Yeni Service Fabric uygulamanıza sağ tıklayın ve ardından **Service Fabric**’i seçin.

    ![Service Fabric sağ tıklama menüsü][publish/RightClick]

2. Bağlam menüsünde aşağıdaki seçeneklerden birini seçin:
    -   Uygulamayı temizlemeden derlemek için **Uygulamayı Derle**’ye tıklayın.
    -   Uygulamanın temiz bir derlemesini gerçekleştirmek için **Uygulamayı Yeniden Derle**’ye tıklayın.
    -   Derlenen yapıtları uygulamadan temizlemek için **Uygulamayı Temizle**’ye tıklayın.
     
## <a name="deploy-a-service-fabric-application-to-the-local-cluster-with-eclipse"></a>Yerel küme ile Eclipse için Service Fabric uygulaması dağıtma

Service Fabric uygulamanızı oluşturduktan sonra yerel kümeye dağıtmak için aşağıdaki adımları izleyin.

1. Yerel küme başlamamış, yönergeleri izleyin. [yerel küme oluşturma](./service-fabric-get-started-linux.md#set-up-a-local-cluster) yerel kümenizi başlatın ve çalışıyor olduğundan emin olun.
2. Service Fabric uygulamanıza sağ tıklayın ve ardından **Service Fabric**.

    ![Service Fabric sağ tıklama menüsü][publish/RightClick]

3.  Bağlam menüsünden tıklayın **uygulamayı dağıtma**.
4.  Konsol penceresinde dağıtma işleminin ilerleme durumunu izleyebilirsiniz.
5.  Uygulamanızın çalıştığını doğrulamak için bir tarayıcı penceresinde yerel kümenizde Service Fabric Explorer açın [ http://localhost:19080/Explorer ](http://localhost:19080/Explorer). Genişletin **uygulamaları** düğüm ve emin olun, uygulamanız çalışıyor. 

Eclipse kullanarak yerel kümeye uygulamanızda hata ayıklama öğrenmek için bkz. [eclipse'te Java hizmeti hata ayıklaması](./service-fabric-debugging-your-application-java.md).

Yerel küme ile birlikte uygulamanızı da dağıtabilirsiniz **uygulama Yayımla** komutu:

1. Service Fabric uygulamanıza sağ tıklayın ve ardından **Service Fabric**.
2. Bağlam menüsünden tıklayın **uygulama Yayımla...** .
3. İçinde **uygulama Yayımla** penceresinde seçin **PublishProfiles/Local.json** tıklayın ve hedef profil olarak **Yayımla**.

    ![Yayımla İletişim Kutusu Konumu](./media/service-fabric-get-started-eclipse/localjson.png)

    Varsayılan olarak, yayımlama profilini Local.json yerel kümeye yayımlamak için ayarlanır. Yayımlama profillerini mevcut bağlantı ve uç nokta parametreleri hakkında daha fazla bilgi için sonraki bölüme bakın.

## <a name="publish-your-service-fabric-application-to-azure-with-eclipse"></a>Service Fabric uygulamanızı Eclipse ile azure'a yayımlama

Uygulamanızı buluta yayımlamak için aşağıdaki adımları izleyin:

1. Bulutta güvenli bir kümeye uygulamanızı yayımlamak için kümenizin ile iletişim kurmak için kullanılacak bir X.509 sertifikası gerekir. Test ve geliştirme ortamlarında kullanılan sertifika genellikle küme sertifikası olmalıdır. Üretim ortamlarında sertifika küme sertifikasından farklı bir istemci sertifikası olmalıdır. Hem sertifika hem de özel anahtar gerekir. Sertifika (ve anahtar) dosyası, PEM biçimli olması gerekir. Sertifikayı ve özel anahtarı aşağıdaki openssl komutu ile bir PFX dosyasından içeren bir PEM dosyası oluşturabilirsiniz:

    ```bash
    openssl pkcs12 -in your-cert-file.pfx -out your-cert-file.pem -nodes -passin pass:your-pfx-password
    ```

   PFX dosyasının parola korumalı değilse, `--passin pass:` son parametresi için.

2. Açık **Cloud.json** altında dosya **PublishProfiles** dizin. Küme uç noktası ve güvenlik kimlik bilgileri kümeniz için uygun şekilde yapılandırmanız gerekir.

   - `ConnectionIPOrURL` Alan IP adresini veya kümenizin URL'sini içerir. URL düzeni değeri içermiyor Not (`https://`).
   - Varsayılan olarak `ConnectionPort` alana olmalıdır `19080`, kümeniz için bu bağlantı noktası açıkça değiştirmediğiniz sürece.
   - `ClientKey` Alan bir PEM biçimli .pem veya .key dosyasının, istemci veya küme sertifikası özel anahtarı içeren yerel makinenizde işaret etmelidir.
   - `ClientCert` Alan bir dosyaya PEM biçimli .pem veya .crt istemci ya da küme için sertifika verileri içeren yerel makinenize işaret etmelidir. Sertifika. 

     ```bash
     {
         "ClusterConnectionParameters":
         {
            "ConnectionIPOrURL": "lnxxug0tlqm5.westus.cloudapp.azure.com",
            "ConnectionPort": "19080",
            "ClientKey": "[path_to_your_pem_file_on_local_machine]",
            "ClientCert": "[path_to_your_pem_file_on_local_machine]"
         }
     }
     ```

2. Service Fabric uygulamanıza sağ tıklayın ve ardından **Service Fabric**.
3. Bağlam menüsünden tıklayın **uygulama Yayımla...** .
3. İçinde **uygulama Yayımla** penceresinde seçin **PublishProfiles/Cloud.json** tıklayın ve hedef profil olarak **Yayımla**.

    ![İletişim Kutusunu Yayımlama Bulutu](./media/service-fabric-get-started-eclipse/cloudjson.png)

4. Konsol penceresinde yayımlama işleminin ilerleme durumunu izleyebilirsiniz.
5. Service Fabric Explorer, uygulamanızın çalıştığını doğrulamak için Azure kümenizdeki bir tarayıcı penceresinde açın. Yukarıdaki örnekte, bu olacaktır: `https://lnxxug0tlqm5.westus.cloudapp.azure.com:19080/Explorer`. Genişletin **uygulamaları** düğüm ve emin olun, uygulamanız çalışıyor. 


Uygulamanız Reliable Services hizmetlerini içeriyorsa güvenli Linux kümelerinde, ayrıca hizmetlerinizi Service Fabric çalışma zamanı API'leri çağırmak için kullanabileceğiniz bir sertifika yapılandırmanız gerekir. Daha fazla bilgi için bkz. [Linux kümelerinde çalıştırmak için bir Reliable Services uygulaması yapılandırırsınız](./service-fabric-configure-certificates-linux.md#configure-a-reliable-services-app-to-run-on-linux-clusters).

Bir hızlı bir kılavuz nasıl güvenli bir Linux kümesi için Java dilinde yazılmış bir Service Fabric güvenilir hizmetler uygulaması dağıtmak için bkz: [hızlı başlangıç: Bir Java Reliable Services uygulaması dağıtma](./service-fabric-quickstart-java-reliable-services.md).

## <a name="deploy-a-service-fabric-application-by-using-eclipse-run-configurations"></a>Eclipse çalıştırma yapılandırmalarının kullanarak bir Service Fabric uygulaması dağıtma

Service Fabric uygulamanızı dağıtmanın alternatif bir yolu, Eclipse çalıştırma yapılandırmalarının kullanılmasıdır.

1. Eclipse'te, Git **çalıştırma** > **çalıştırma yapılandırmaları**.
2. **Grade Projesi** altındaki **ServiceFabricDeployer** çalıştırma yapılandırmasını seçin.
3. Sağ bölmede üzerinde **bağımsız değişkenleri** sekmesinde, emin **IP**, **bağlantı noktası**, **clientCert**, ve **clientKey**parametreleri dağıtımınız için uygun şekilde ayarlanır. Varsayılan olarak, aşağıdaki ekran görüntüsünde gösterildiği gibi yerel kümeye dağıtmak için parametreleri ayarlanır. Uygulamanızı Azure'da yayımlamak için uç nokta bilgilerini ve Azure kümeniz için güvenlik kimlik bilgileri içerecek şekilde parametreleri değiştirebilirsiniz. Daha fazla bilgi için önceki bölüme bakın [Service Fabric uygulamanızı Eclipse ile azure'a yayımlama](#publish-your-service-fabric-application-to-azure-with-eclipse).

    ![Yapılandırma iletişim yerel çalıştırma](./media/service-fabric-get-started-eclipse/run-config-local.png)

5. Emin olun **çalışma dizini** dağıtmak istediğiniz uygulamayı işaret eder. Uygulamayı değiştirmek için, **Çalışma Alanı** düğmesine tıklayın ve istediğiniz uygulamayı seçin.
6. **Uygula** ve ardından **Çalıştır** seçeneğine tıklayın.

Uygulamanız birkaç dakika içinde derlenip dağıtılır. Dağıtım durumunu Service Fabric Explorer’dan izleyebilirsiniz.  

## <a name="add-a-service-fabric-service-to-your-service-fabric-application"></a>Service Fabric uygulamanıza Service Fabric hizmeti ekleme

Var olan bir Service Fabric uygulamasına Service Fabric hizmeti eklemek için aşağıdaki adımları uygulayın:

1.  Hizmet eklemek istediğiniz projeye sağ tıklayın ve ardından **Service Fabric**’e tıklayın.

    ![Service Fabric Hizmet Ekleme sayfa 1][add-service/p1]

2.  **Service Fabric Hizmeti Ekle**’ye tıklayın ve projeye bir hizmet eklemek için adımları tamamlayın.
3.  Projenize eklemek istediğiniz hizmet şablonunu seçip **İleri**’ye tıklayın.

    ![Service Fabric Hizmet Ekleme sayfa 2][add-service/p2]

4.  Hizmet adını (ve gereken diğer ayrıntıları) girip **Hizmet Ekle** düğmesine tıklayın.  

    ![Service Fabric Hizmet Ekleme sayfa 3][add-service/p3]

5.  Hizmet eklendikten sonra projenizin genel yapısı aşağıdaki projeye benzer:

    ![Service Fabric Hizmet Ekleme sayfa 4][add-service/p4]

## <a name="edit-manifest-versions-of-your-service-fabric-java-application"></a>Service Fabric Java uygulamanızın Bildirim sürümlerini düzenleme

Bildirim sürümlerini düzenlemek için projeye sağ tıklayın, **Service Fabric**'e gidin ve açılan menüden **Bildirim Sürümlerini Düzenle...** seçeneğini belirleyin. Sihirbazda uygulama bildiriminin ve hizmet bildiriminin yanı sıra **Kod**, **Yapılandırma** ve **Veriler** adlı paketlere ilişkin sürümlere yönelik bildirim sürümlerini güncelleştirebilirsiniz.

**Uygulama ve hizmet sürümlerini otomatik olarak güncelleştir** seçeneğini işaretler ve ardından bir sürümü güncelleştirirseniz bildirim sürümleri de otomatik olarak güncelleştirilir. Örneğin, onay kutusunu işaretleyip **Kod** sürümünü 0.0.0'dan 0.0.1'e güncelleştirir ve ardından **Son**'a tıklarsanız hizmet bildirim sürümü ve uygulama bildirim sürümü otomatik olarak 0.0.1'e güncelleştirilir.

## <a name="upgrade-your-service-fabric-java-application"></a>Service Fabric Java uygulamanızı yükseltme

Bir yükseltme senaryosu için, Eclips’te Service Fabric eklentisini kullanarak **App1** projesini oluşturduğunuzu varsayalım. **fabric:/App1Application** adlı bir uygulama oluşturmak için eklentiyi kullanarak projeyi dağıttınız. Uygulama türü **App1ApplicationType**, uygulama sürümü 1.0 ise. Şimdi de uygulamanızı kesinti olmadan yükseltmek istiyorsunuz.

İlk olarak, uygulamanızda değişiklikleri yapın ve değiştirilen hizmeti yeniden derleyin. Değiştirilen hizmetin bildirim dosyasını (ServiceManifest.xml) hizmetin güncel sürümleriyle (ve uygun olan Kod, Yapılandırma ya da Veriler ile) güncelleştirin. Uygulamanın bildirim dosyasını (ApplicationManifest.xml), uygulamanın ve değiştirilen hizmetin güncel sürüm numarasıyla da değiştirin.  

Eclipse kullanarak uygulamanızı yükseltmek için, yinelenen bir çalıştırma yapılandırma profili oluşturabilirsiniz. Ardından, uygulamanızı gereken şekilde yükseltmek için bu profili kullanın.

1.  **Çalıştır** > **Çalıştırma Yapılandırmaları** öğesine gidin. Sol bölmede, **Grade Projesi** öğesinin sol tarafında bulunan küçük oka tıklayın.
2.  **ServiceFabricDeployer**’a sağ tıklayıp **Yinelenen**’i seçin. Bu yapılandırma için **ServiceFabricUpgrader** gibi yeni bir ad girin.
3.  Sağ paneldeki **Bağımsız Değişkenler** sekmesinde **-Pconfig='deploy'** değerini **-Pconfig='upgrade'** olarak değiştirip **Uygula**’ya tıklayın.

Bu işlem, uygulamanızı yükseltmek için dilediğiniz zaman kullanabileceğiniz bir çalıştırma yapılandırma profili oluşturup kaydeder. Ayrıca, en son güncelleştirilmiş uygulama türü sürümünü uygulama bildirim dosyasından alır.

Uygulama yükseltmesi birkaç dakika sürer. Uygulama yükseltme işlemini Service Fabric Explorer’dan izleyebilirsiniz.

## <a name="migrating-old-service-fabric-java-applications-to-be-used-with-maven"></a>Eski Service Fabric Java uygulamalarını Maven ile kullanılmak üzere geçirme
Service Fabric Java kitaplıklarını yakın zamanda Service Fabric Java SDK’sından Maven deposuna taşıdık. Eclipse kullanarak oluşturduğunuz yeni uygulamalar en son güncelleştirilen projeleri oluşturur (Maven ile çalışırlar), ancak daha önce Service Fabric Java SDK’sı kullanan mevcut Service Fabric durum bilgisi olmayan ya da aktör Java uygulamalarını Maven’ın Service Fabric Java bağımlılıklarını kullanacak şekilde güncelleştirebilirsiniz. Eski uygulamanızın Maven ile çalıştığından emin olmak için lütfen [burada](service-fabric-migrate-old-javaapp-to-use-maven.md) belirtilen adımları izleyin.

## <a name="next-steps"></a>Sonraki adımlar

- Hizmet uygulaması için hızlı adımlar Java Reliable oluşturma ve bunu yerel olarak ve azure'a dağıtma, bakın [hızlı başlangıç: Bir Java Reliable Services uygulaması dağıtma](./service-fabric-quickstart-java-reliable-services.md).
- Yerel kümenizdeki bir Java uygulamasının hatasını ayıklama öğrenmek için bkz. [eclipse'te Java hizmeti hata ayıklaması](./service-fabric-debugging-your-application-java.md).
- Service Fabric uygulamaları izleme ve tanılama konusunda bilgi almak için bkz: [İzleyici ve yerel makine dağıtım kurulumunda Hizmetleri tanılama](./service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md).

<!-- Images -->

[sf-eclipse-plugin-install]: ./media/service-fabric-get-started-eclipse/service-fabric-eclipse-plugin.png

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
