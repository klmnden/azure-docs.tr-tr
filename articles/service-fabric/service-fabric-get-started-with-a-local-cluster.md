<properties
   pageTitle="Yerel kümenizdeki uygulamaları dağıtma ve yükseltme işlemlerine giriş | Microsoft Azure"
   description="Yerel bir Service Fabric kümesi ayarlayın, var olan bir uygulamayı bu kümeye dağıtın ve ardından uygulamayı yükseltin."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/10/2016"
   ms.author="ryanwi"/>

# Yerel kümenizdeki uygulamaları dağıtma ve yükseltme işlemlerine giriş
Azure Service Fabric SDK, yerel bir kümede hızlıca dağıtma ve uygulamaları yönetme işlemlerine başlamak için kullanabileceğiniz eksiksiz bir yerel geliştirme ortamı içerir. Bu makalede, yerel bir küme oluşturup var olan bir uygulamayı kümeye dağıtır, ardından uygulamayı yeni bir sürüme yükseltirsiniz ve tüm bu işlemleri Windows PowerShell'den gerçekleştirirsiniz.

> [AZURE.NOTE] Bu makale [geliştirme ortamınızı daha önceden ayarladığınızı](service-fabric-get-started.md) varsayar.

## Yerel bir küme oluşturma
Service Fabric kümesi, uygulamaları dağıtabileceğiniz bir dizi donanım kaynağını ifade eder. Genel olarak, bir küme herhangi bir yerden oluşturulabilir ve en az beş olmak üzere yüzlerce makine içerebilir. Ancak Service Fabric SDK yalnızca tek bir makinede çalışabilen küme yapılandırması içerir.

Service Fabric yerel kümesinin bir öykünücü veya benzetici olmadığını anlamak oldukça önemlidir. Çok makineli kümelerde bulunan aynı platform kodunu çalıştırır. Tek fark, normalde beş makineye yayılan platform işlemlerini tek bir makinede çalıştırmasıdır.

SDK, yerel küme ayarlamak için iki yöntem sunar: Windows PowerShell betiği ve Yerel Küme Yöneticisi sistemi tepsisi uygulaması. Bu öğreticide PowerShell betiğini kullanıyoruz.

> [AZURE.NOTE] Visual Studio'dan bir uygulama dağıtarak zaten bir yerel küme oluşturduysanız bu bölümü atlayabilirsiniz.


1. Yönetici olarak yeni bir PowerShell penceresi başlatın.

2. SDK klasöründeki küme kurulumu betiğini çalıştırın:

    ```powershell
    & "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1"
    ```

    Küme kurulumu biraz zaman alır. Kurulum tamamlandıktan sonra şunun gibi görünen bir çıktı görmelisiniz:

    ![Küme kurulumu çıktısı][cluster-setup-success]

    Artık kümenize bir uygulama dağıtmaya hazırsınız.

## Uygulama dağıtma
Service Fabric SDK, uygulama oluşturmaya yönelik geliştirici araçları ve zengin bir altyapı dizisi içerir. Visual Studio'da uygulama oluşturmayı öğrenmek isterseniz [Visual Studio'da ilk Service Fabric uygulamanızı oluşturun](service-fabric-create-your-first-application-in-visual-studio.md) konu başlığına bakın.

Bu öğreticide, var olan örnek bir uygulama kullanacağımızdan (WordCount olarak adlandırılır) platformun dağıtım, izleme ve yükseltme gibi yönetim özelliklerine odaklanabiliriz.


1. Yönetici olarak yeni bir PowerShell penceresi başlatın.

2. Service Fabric SDK PowerShell modülünü içeri aktarın.

    ```powershell
    Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
    ```

3. İndirip dağıttığınız uygulamayı depolamak için C:\ServiceFabric gibi bir dizin oluşturun.

    ```powershell
    mkdir c:\ServiceFabric\
    cd c:\ServiceFabric\
    ```

4. Oluşturduğunuz konuma [WordCount uygulamasını indirin](http://aka.ms/servicefabric-wordcountapp).  Not: Microsoft Edge tarayıcısı dosyayı bir *.zip* uzantısı ile kaydeder.  Dosya uzantısını *.sfpkg* olarak değiştirin.

5. Yerel kümeye bağlanın:

    ```powershell
    Connect-ServiceFabricCluster localhost:19000
    ```

6. SDK’nın dağıtım komutunu bir uygulama paketi adı ve yoluyla kullanarak yeni bir uygulama oluşturun.

    ```powershell  
  Publish-NewServiceFabricApplication -ApplicationPackagePath c:\ServiceFabric\WordCountV1.sfpkg -ApplicationName "fabric:/WordCount"
    ```

    Her şey doğru şekilde yapıldığında şuna benzer bir çıktı göreceksiniz:

    ![Uygulamayı yerel kümeye dağıtma][deploy-app-to-local-cluster]

7. Uygulamayı eylem halinde görmek için tarayıcıyı başlatın ve [http://localhost:8081/wordcount/index.html](http://localhost:8081/wordcount/index.html) adresine gidin. Şunu görmeniz gerekir:

    ![Dağıtılan uygulama kullanıcı arabirimi][deployed-app-ui]

    WordCount uygulaması oldukça basit bir şekilde işler. Daha sonra ASP.NET Web API'si aracılığıyla uygulamaya geçirilecek olan beş karakterli rastgele "sözcükler" oluşturmak için istemci tarafı JavaScript kodu içerir. Durum bilgisi olan bir hizmet, sayılan sözcüklerin sayısını izler. Sözcükler, ilk karakterlerine göre bölümlenir. WordCount uygulamasının kaynak kodunu [başlangıç örneklerinde](https://azure.microsoft.com/documentation/samples/service-fabric-dotnet-getting-started/) bulabilirsiniz.

    Dağıttığımız uygulama ise dört bölümden oluşur. İlk karakteri A ile G arasında olan kelimeler ilk bölümdür. İlk karakteri H ile N arasında yer alan kelimeler de ikinci bölümü oluşturur ve bölümleme bu şekilde devam eder.

## Uygulama bilgilerini ve durumu görüntüleme
Artık uygulamayı dağıttığımıza göre PowerShell'deki bazı uygulama bilgilerine bakalım.

1. Kümedeki tüm dağıtılan uygulamaları sorgulayın:

    ```powershell
    Get-ServiceFabricApplication
    ```

    Yalnızca WordCount uygulamasını dağıttığınızı varsayarsak şuna benzeyen bir ekran görürsünüz:

    ![PowerShell'deki dağıtılan uygulamaların tümünü sorgulama][ps-getsfapp]

2. WordCount uygulamasında bulunan hizmet dizisini sorgulayarak bir sonraki seviyeye geçin.

    ```powershell
    Get-ServiceFabricService -ApplicationName 'fabric:/WordCount'
    ```

    ![PowerShell'deki uygulamaya ait hizmetleri listeleme][ps-getsfsvc]

    Uygulama, web ön ucu ve sözcükleri yöneten durum bilgisi olan hizmet olmak üzere iki hizmetten oluşur.

3. Son olarak, WordCountService'e ait bölümlerin listesine gözatın:

    ```powershell
    Get-ServiceFabricPartition 'fabric:/WordCount/WordCountService'
    ```

    ![PowerShell'de hizmet bölümlerini görüntüleme][ps-getsfpartitions]

    Tüm Service Fabric PowerShell komutları gibi kullandığınız komutlar kümesi, yerel veya uzaktan olması farketmeksizin bağlanmak isteyebileceğiniz herhangi bir küme tarafından kullanılabilir.

    Kümeyle etkileşime geçmek için daha görsel bir yöntem için tarayıcıda [http://localhost:19080/Explorer](http://localhost:19080/Explorer) adresine giderek web tabanlı Service Fabric Explorer aracını kullanabilirsiniz.

    ![Service Fabric Explorer'da uygulama bilgilerini görüntüleme][sfx-service-overview]

    > [AZURE.NOTE] Service Fabric Explorer hakkında daha fazla bilgi edinmek için bkz. [Service Fabric Explorer ile kümenizi görselleştirme](service-fabric-visualizing-your-cluster.md).

## Uygulama yükseltme
Service Fabric, kümeye gönderilirken uygulamanın durumunu izleyerek kesinti süresi olmadan gerçekleştirilen yükseltmeler sunar. WordCount uygulamasının basit bir yükseltmesini gerçekleştirelim.

Uygulamanın yeni sürümü artık yalnızca sesli bir harfle başlayan sözcükleri sayar. Yükseltme dağıtılırken uygulamanın davranışında iki değişiklik görürüz. İlk olarak, daha az sözcük sayıldığından sayaç büyümesinin hızı azalacaktır. İkinci ise ilk bölüm iki sesli harf (A ve E) ve diğer tüm bölümler bunlardan yalnızca birini içerebileceğinden ilk bölümün sayacı sonunda diğerlerini geride bırakır.

1. v1 paketini indirdiğiniz konuma [WordCount v2 paketini indirin](http://aka.ms/servicefabric-wordcountappv2).

2. PowerShell pencerenize geri dönün ve yeni sürümü kümeye kaydetmek için SDK'nın yükseltme komutunu kullanın. Ardından fabric:/WordCount uygulamasını yükseltmeye başlayın.

    ```powershell
    Publish-UpgradedServiceFabricApplication -ApplicationPackagePath C:\ServiceFabric\WordCountV2.sfpkg -ApplicationName "fabric:/WordCount" -UpgradeParameters @{"FailureAction"="Rollback"; "UpgradeReplicaSetCheckTimeout"=1; "Monitored"=$true; "Force"=$true}
    ```

    Yükseltme başladıktan sonra PowerShell'de aşağıdaki gibi bir çıktı görmelisiniz.

    ![PowerShell'de yükseltme süreci][ps-appupgradeprogress]

3. Yükseltme devam ederken yükseltme durumunu Service Fabric Explorer'dan daha rahat bir şekilde izleyebilirsiniz. Bir tarayıcı penceresi başlatın ve [http://localhost:19080/Explorer](http://localhost:19080/Explorer) adresine gidin. Sol taraftaki ağaçta bulunan **Uygulamalar** kısmını genişletin, **WordCount** uygulamasını ve son olarak da **fabric:/WordCount** uygulamasını seçin. Temel bilgiler sekmesinde,yükseltme kümenin yükseltme etki alanlarında ilerlerken siz de bu işlemin durumunu görürsünüz.

    ![Service Fabric Explorer'da yükseltme süreci][sfx-upgradeprogress]

    Yükseltme işlemi etki alanlarında ilerlerken, uygulamanın uygun şekilde davrandığından emin olmak için durum denetimleri yapılır.

4. fabric:/WordCount uygulamasındaki hizmet kümesi için daha önce gerçekleştirdiğiniz sorguyu tekrar çalıştırırsanız WordCountService sürümünün değiştiğini, ancak WordCountWebService sürümünün değişmediğini fark edebilirsiniz:

    ```powershell
    Get-ServiceFabricService -ApplicationName 'fabric:/WordCount'
    ```

    ![Yükseltme işleminden sonra uygulama hizmetlerini sorgulama][ps-getsfsvc-postupgrade]

    Bu kısımda Service Fabric hizmetinin uygulama yükseltmelerini yönetme şekli vurgulanır. Yalnızca yükseltmeyi sürecini daha hızlı ve güvenilir hale getiren, değiştirilmiş hizmetler dizisi (veya bu hizmetlerin içindeki kod/yapılandırma paketleri) ele alınır.

5. Son olarak, yeni uygulama sürümünün davranışını izlemek için tarayıcıya dönün. Tahmin edildiği üzere sayaç daha yavaş ilerler ve ilk bölüm kısmen daha fazla miktarla sonlanır.

    ![Uygulamanın yeni sürümünü tarayıcıda görüntüleme][deployed-app-ui-v2]

## Temizleme

Sonlandırmadan önce yerel kümenin gerçek olduğunu unutmamanız önemlidir. Uygulamalar, sizin tarafınızdan kaldırılıncaya kadar arka planda çalışmaya devam eder.  Uygulamalarınızın niteliğine bağlı olarak, çalışan bir uygulama makinenizde önemli miktarda kaynağı kullanabilir. Uygulamaları ve kümeyi yönetmek için birkaç seçeneğiniz vardır:

1. Tek bir uygulamayı ve onunla ilişkili tüm verileri kaldırmak için aşağıdakini çalıştırın:

    ```powershell
    Unpublish-ServiceFabricApplication -ApplicationName "fabric:/WordCount"
    ```

    Veya Service Fabric Explorer’ın **EYLEMLER** menüsünden ya da sol bölmedeki uygulama listesi görünümündeki bağlam menüsünden uygulamayı silin.

    ![Service Fabric Explorer'da uygulama silme][sfe-delete-application]

2. Uygulamayı kümeden sildikten sonra WordCount uygulama türünün 1.0.0 ve 2.0.0 sürümlerinin kaydını kaldırabilirsiniz. Silme işlemi, kod ve yapılandırma dahil olmak üzere uygulama paketlerini kümenin görüntü deposundan kaldırır.

    ```powershell
    Remove-ServiceFabricApplicationType -ApplicationTypeName WordCount -ApplicationTypeVersion 2.0.0
    Remove-ServiceFabricApplicationType -ApplicationTypeName WordCount -ApplicationTypeVersion 1.0.0
    ```

    Veya Service Fabric Explorer’da uygulamanın **Sağlamayı Kaldırma Türü**’nü seçin.

3. Kümeyi kapatıp uygulama verilerini ve izlemelerini tutmak için sistem tepsisi uygulamasında **Yerel Kümeyi Durdur**'a tıklayın.

4. Kümeyi tamamen silmek için sistem tepsisi uygulamasında **Yerel Kümeyi Kaldır**'a tıklayın. Visual Studio'da F5'e bir sonraki basışınızda bu seçeneğin başka bir yavaş dağıtımla sonuçlanacağını unutmayın. Yerel kümeyi yalnızca bir süre kullanmayı planlamıyorsanız veya kaynaklarınızı geri kazanmanız gerekiyorsa kaldırın.

## Sonraki adımlar
- Önceden derlenen bazı uygulamaları dağıtıp geliştirdiğinize göre [Visual Studio'da kendi uygulamanızı derlemeyi deneyebilirsiniz](service-fabric-create-your-first-application-in-visual-studio.md).
- Bu makalede yer alan yerel kümede gerçekleştirilen eylemlerin tümü [Azure kümesinde](service-fabric-cluster-creation-via-portal.md) de gerçekleştirilebilir.
- Bu makalede gerçekleştirdiğimiz yükseltme işlemi temel bir işlemdi. Service Fabric yükseltmelerinin gücü ve esnekliği hakkında daha fazla bilgi edinmek için [yükseltme belgelerine](service-fabric-application-upgrade.md) bakın.

<!-- Images -->

[cluster-setup-success]: ./media/service-fabric-get-started-with-a-local-cluster/LocalClusterSetup.png
[extracted-app-package]: ./media/service-fabric-get-started-with-a-local-cluster/ExtractedAppPackage.png
[deploy-app-to-local-cluster]: ./media/service-fabric-get-started-with-a-local-cluster/DeployAppToLocalCluster.png
[deployed-app-ui]: ./media/service-fabric-get-started-with-a-local-cluster/DeployedAppUI-v1.png
[deployed-app-ui-v2]: ./media/service-fabric-get-started-with-a-local-cluster/DeployedAppUI-PostUpgrade.png
[sfx-app-instance]: ./media/service-fabric-get-started-with-a-local-cluster/SfxAppInstance.png
[sfx-two-app-instances-different-partitions]: ./media/service-fabric-get-started-with-a-local-cluster/SfxTwoAppInstances-DifferentPartitionCount.png
[ps-getsfapp]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFApp.png
[ps-getsfsvc]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFSvc.png
[ps-getsfpartitions]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFPartitions.png
[ps-appupgradeprogress]: ./media/service-fabric-get-started-with-a-local-cluster/PS-AppUpgradeProgress.png
[ps-getsfsvc-postupgrade]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFSvc-PostUpgrade.png
[sfx-upgradeprogress]: ./media/service-fabric-get-started-with-a-local-cluster/SfxUpgradeOverview.png
[sfx-service-overview]: ./media/service-fabric-get-started-with-a-local-cluster/sfx-service-overview.png
[sfe-delete-application]: ./media/service-fabric-get-started-with-a-local-cluster/sfe-delete-application.png



<!--HONumber=Aug16_HO4-->


