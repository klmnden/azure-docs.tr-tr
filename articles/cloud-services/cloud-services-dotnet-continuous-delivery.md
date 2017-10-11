---
title: "Bulut için kesintisiz teslim hizmetleri Azure içinde TFS ile | Microsoft Docs"
description: "Azure bulut uygulamaları için sürekli teslimini ayarlayın öğrenin. MSBuild komut satırı deyimleri ve PowerShell komut dosyaları için kod örnekleri."
services: cloud-services
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 4f3c93c6-5c82-4779-9d19-7404a01e82ca
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/12/2017
ms.author: kraigb
ms.openlocfilehash: 0979722b9ec715e91825c7aba74657451df6e83f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="continuous-delivery-for-cloud-services-in-azure"></a>Azure bulut Hizmetleri için devamlı teslim
Bu makalede açıklanan işlemi Azure bulut uygulamaları için sürekli teslimini ayarlayın gösterilmiştir. Bu işlem, her kod iadesinden sonra paketleri otomatik olarak oluşturmanıza ve paketi Azure'da dağıtmanıza olanak tanır. Bu makalede açıklanan paket oluşturma işlemi eşdeğerdir **paket** Visual Studio komut ve yayımlama adımlarını eşdeğer **Yayımla** Visual Studio'da komutu.
Makale bir yapı sunucu MSBuild komut satırı deyimleri ve Windows PowerShell komut dosyaları oluşturmak için kullandığınız yöntemleri kapsar ve ayrıca isteğe bağlı olarak Visual Studio Team Foundation Server - ekip tanımları kullanacak şekilde yapılandırmak nasıl gösterir MSBuild komutlar ve PowerShell komut dosyaları. Yapı ortamı ve Azure hedef ortamları için özelleştirilebilir işlemidir.

Visual Studio Team Services, bunu daha kolay yapmak için Azure üzerinde barındırılan TFS sürümünü de kullanabilirsiniz. 

Başlamadan önce uygulamanızı Visual Studio'dan yayımlamanız gerekir.
Bu tüm kaynakların kullanılabilir ve başlatılmış olduğundan emin olmak için yayın işlemini otomatikleştirmek çalıştığınızda.

## <a name="1-configure-the-build-server"></a>1: derleme sunucusunu yapılandırın
MSBuild kullanarak bir Azure paketi oluşturabilmeniz için önce yapı sunucuda gerekli yazılım ve araçları yüklemeniz gerekir.

Visual Studio derleme sunucuya yüklenmesi gerekli değildir. Team Foundation Yapı Hizmeti yapı sunucunuzu yönetmek için kullanmak istiyorsanız,'ndaki yönergeleri izleyin [Team Foundation Yapı Hizmeti] [ Team Foundation Build Service] belgeleri.

1. Yapı sunucuya yüklemek [.NET Framework 4.5.2][.NET Framework 4.5.2], MSBuild içerir.
2. En son yükleme [.NET için Azure yazma araçları](https://azure.microsoft.com/develop/net/).
3. Yükleme [.NET için Azure kitaplıkları](http://go.microsoft.com/fwlink/?LinkId=623519).
4. Microsoft.WebApplication.targets dosyasını Visual Studio yüklemesinden yapı sunucuya kopyalayın.

   Visual Studio'nun yüklü olan bir bilgisayarda bu dosyayı C: dizininde bulunan\\Program Files(x86)\\MSBuild\\Microsoft\\Visual Studio\\v14.0\\Web. Derleme sunucusundaki aynı dizine kopyalamalısınız.
5. Yükleme [Visual Studio için Azure Araçları](https://www.visualstudio.com/features/azure-tools-vs.aspx).

## <a name="2-build-a-package-using-msbuild-commands"></a>2: MSBuild komutları kullanarak bir paket oluşturun
Bu bölümde, bir Azure paketi derlemeler MSBuild komut oluşturmak açıklar. Bu adımı her şeyin doğru şekilde yapılandırıldığını ve MSBuild komut ne yapmak istiyorsunuz yaptığını doğrulamak için yapı sunucusunda çalıştırın. Ya da bu komut satırı derleme sunucusundaki mevcut derleme betiklerini ekleyebileceğiniz veya komut satırı derleme TFS tanımında, sonraki bölümde açıklandığı gibi kullanabilirsiniz. Komut satırı parametreleri ve MSBuild hakkında daha fazla bilgi için bkz: [MSBuild komut satırı başvurusu](https://msdn.microsoft.com/library/ms164311%28v=vs.140%29.aspx).

1. Visual Studio derleme sunucuda yüklüyse, bulun ve seçin **Visual Studio komut istemi** içinde **Visual Studio Araçları** Windows klasörü.

   Visual Studio derleme sunucuda yüklü değilse, bir komut istemi açın ve MSBuild.exe yola erişilebilir olduğundan emin olun. MSBuild yolu % WINDIR % .NET Framework yüklenmiş\\Microsoft.NET\\Framework\\*sürüm*. Örneğin, .NET Framework 4 yüklü olduğunda MSBuild.exe PATH ortam değişkenine eklemek için komut istemine aşağıdaki komutu yazın:

       set PATH=%PATH%;"C:\Windows\Microsoft.NET\Framework\v4.0.30319"
2. Komut isteminde, oluşturmak istediğiniz Azure proje dosyasını içeren klasöre gidin.
3. / Target ile MSBuild çalıştırın: Aşağıdaki örnekte olduğu gibi seçeneği yayımlama:

       MSBuild /target:Publish

   Bu seçenek /t kısaltılır: yayımlayın. Azure SDK yüklü olduğunda MSBuild /t:Publish seçeneğinde Visual Studio'da kullanılabilen Yayımla komutları ile karıştırılmamalıdır. /T: seçeneği yalnızca derlemeleri Azure paketleri yayımlama. Visual Studio'da yayımlama komutları gibi paketleri dağıtmaz.

   İsteğe bağlı olarak, bir MSBuild parametresi olarak proje adı belirtebilirsiniz. Belirtilmezse, geçerli dizin kullanılır. MSBuild komut satırı seçenekleri hakkında daha fazla bilgi için bkz: [MSBuild komut satırı başvurusu](https://msdn.microsoft.com/library/ms164311%28v=vs.140%29.aspx).
4. Çıktı bulun. Varsayılan olarak, bu komut bir dizin projenin kök klasörüne göre gibi oluşturur *ProjectDir*\\bin\\*yapılandırma*\\app.publish \\. Bir Azure projesi derlerken, iki dosya, paket dosyası ve eşlik eden yapılandırma dosyası oluşturun:

   * Project.cspkg
   * ServiceConfiguration. *TargetProfile*.cscfg

   Varsayılan olarak, her Azure projesi yerel (hata ayıklama) yapılar ve bulut (hazırlık veya üretim) yapılar için başka bir hizmet yapılandırma dosyası (.cscfg dosyası) içerir, ancak ekleyip gerektiği gibi hizmet yapılandırma dosyalarını kaldırabilirsiniz. Visual Studio içinde bir paket yapılandırdığınızda, paket dahil etmek için hangi hizmet yapılandırma dosyası istenir.
5. Hizmet yapılandırma dosyası belirtin. MSBuild kullanarak bir paket oluşturduğunuzda, yerel hizmet yapılandırma dosyası varsayılan olarak dahil edilir. Farklı hizmet yapılandırma dosyası eklemek için aşağıdaki örnekte olduğu gibi MSBuild komut TargetProfile özelliğini ayarlayın:

       MSBuild /t:Publish /p:TargetProfile=Cloud
6. Çıktı konumunu belirtin. /P:PublishDir kullanarak yolunu ayarlama =*Directory* \\ seçeneği, aşağıdaki örnekte olduğu gibi sondaki eğik çizgi ayırıcı dahil olmak üzere:

       MSBuild /target:Publish /p:PublishDir=\\myserver\drops\

   Oluşturulan ve projelerinizi oluşturma ve bunları Azure bir pakete birleştirmek için uygun bir MSBuild komut satırı test sonra bu komut satırı derleme komutlarınızı ekleyebilirsiniz. Yapı sunucunuz özel komut dosyaları kullanıyorsa, bu işlem, özel derleme işlem ayrıntılarını bağlıdır. Ardından bir yapı ortamı olarak TFS kullanıyorsanız, yapı işleminizin Azure paketi yapı eklemek için yönergeleri sonraki adımda izleyebilirsiniz.

## <a name="3-build-a-package-using-tfs-team-build"></a>3: TFS ekip kullanarak bir paket oluşturun
Varsa Team Foundation Server (TFS) bir yapı denetleyicisi ve yapı server ayarlanmış bir TFS yapı makine olarak ayarlayın, sonra Azure paketiniz için otomatikleştirilmiş bir yapıyı isteğe bağlı olarak ayarlayabilirsiniz. Ayarlama ve Team Foundation server yapı sistem olarak kullanma konusunda daha fazla bilgi için bkz: [derleme sisteminiz genişletme][Scale out your build system]. Özellikle, aşağıdaki yordamda açıklandığı gibi yapı sunucunuz yapılandırdığınız varsayılmaktadır [dağıtma ve bir yapı sunucusunu yapılandırmak][Deploy and configure a build server], ve bir takım projesi oluşturduğunuz Bulutu oluşturulmuş takım projesinde hizmet projesi.

Azure paketleri oluşturmak için TFS yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Visual Studio geliştirme bilgisayarınızda, Görünüm menüsünde seçin **Takım Gezgini**, veya Ctrl + seçin\\, Ctrl + M. Takım Gezgini penceresinde **derlemeler** düğümü veya seçin **derlemeler** sayfasında ve seçin **yeni yapı tanımı**.

   ![Yeni bir yapı tanımı seçeneği][0]
2. Seçin **tetikleyici** sekmesini tıklatın ve oluşturulacak paket istediğinizde için istenen koşulları belirtin. Örneğin, **sürekli tümleştirme** bir kaynak denetim her giriş paketi oluşturmak için oluşur.
3. Seçin **kaynak ayarları** sekmesinde ve proje klasörünüzdeki listelendiğinden emin olun **kaynak denetimi klasörü** sütun ve durum **etkin**.
4. Seçin **Yapı Varsayılanları** sekmesini tıklatın ve yapı denetleyicisi altında yapı sunucu adını doğrulayın.  Ayrıca, seçeneği **kopyalama yapı çıktısını aşağıdaki bırakma klasörüne** ve istenilen bırakma konumu belirtin.
5. Seçin **işlem** sekmesi. İşlem sekmesinde altında varsayılan şablonu seçin **yapı**, zaten seçili değilse ve genişletin, proje seçme **Gelişmiş** bölümüne **yapı** bölümü kılavuzunun.
6. Seçin **MSBuild bağımsız değişkenleri**, yukarıda 2. adımda açıklandığı gibi uygun MSBuild komut satırı bağımsız değişkenleri ayarlayın. Örneğin **/t: /p:PublishDir yayımlama =\\\\myserver\\bırakır\\**  bir paket oluşturmak ve paket dosyaları konumuna kopyalamak için \\ \\ myserver\\bırakır\\:

   ![MSBuild bağımsız değişkenleri][2]

   > [!NOTE]
   > Bir genel paylaşıma dosyaları kopyalanıyor geliştirme bilgisayarınıza paketlerinden el ile dağıtmak kolaylaştırır.
7. Projeniz için bir değişiklik denetleyerek test, derleme adımının başarılı ya da yeni bir yapıyı sıraya. Takım Gezgini'nde, yeni bir derleme kuyruğuna sağ **tüm yapı tanımlarını** ve ardından **yeni yapıyı sıraya al**.

## <a name="4-publish-a-package-using-a-powershell-script"></a>4: bir PowerShell komut dosyası kullanarak bir paket yayımlama
Bu bölüm, bulut uygulama paketi çıktı isteğe bağlı parametreleri kullanarak Azure'a yayımlayacak bir Windows PowerShell betiği oluşturmak açıklar. Bu komut dosyası derleme adım sonra özel derleme otomasyonunuz çağrılabilir. Ayrıca, Visual Studio TFS takım yapısı içinde işlem şablonu iş akışı etkinliklerden de çağrılabilir.

1. Yükleme [Azure PowerShell cmdlet'lerini] [ Azure PowerShell cmdlets] (v0.6.1 ya da daha yüksek).
   Bir ek yüklemek cmdlet kurulumu aşamasında seçin. Önceki sürümler 2.x.x numaralı ancak bu resmi olarak desteklenen sürüm CodePlex sunulan eski sürümün yerine geçer unutmayın.
2. Başlat menüsünü kullanarak Azure PowerShell'i başlatın veya başlangıç sayfası. Bu yolla başlatırsanız, Azure PowerShell cmdlet'leri yüklenir.
3. PowerShell komut isteminde, PowerShell cmdlet'leri kısmi komutunu girerek yüklendiğini doğrulamak `Get-Azure` ve deyim tamamlama için SEKME tuşuna basarak.

   Art arda SEKME tuşuna basın, çeşitli Azure PowerShell komutlarını görmeniz gerekir.
4. Azure aboneliğinize .publishsettings dosyasından abonelik bilgilerinizi içe aktararak bağlanabildiğini doğrulayın.

   `Import-AzurePublishSettingsFile c:\scripts\WindowsAzure\default.publishsettings`

   Komutunu girin

   `Get-AzureSubscription`

   Bu aboneliğiniz hakkında bilgi gösterir. Her şeyin doğru olduğundan emin olun.
5. Komut dosyaları klasörünüze c: olarak bu makalenin sonunda sağlanan komut dosyası şablonu kaydetmek\\betikleri\\WindowsAzure\\**PublishCloudService.ps1**.
6. Komut parametreleri bölümünü gözden geçirin. Ekleyin veya herhangi bir varsayılan değeri değiştirin. Bu değerler, her zaman açık parametreleri geçirerek kılınabilir.
7. Geçerli bulut hizmeti ve depolama hesapları Yayımla komut dosyası tarafından hedeflenebilir aboneliğinizde oluşturulur sağlamak. Depolama hesabı (blob depolama), karşıya yükleme ve dağıtım oluşturulurken dağıtım paketini ve yapılandırma dosyası geçici olarak depolamak için kullanılır.

   * Yeni bir bulut hizmeti oluşturmak için bu komut dosyasını çağırın veya kullanmak [Azure portal](https://portal.azure.com). Bulut hizmeti adı tam etki alanı adının öneki olarak kullanılacak ve bu nedenle benzersiz olmalıdır.

         New-AzureService -ServiceName "mytestcloudservice" -Location "North Central US" -Label "mytestcloudservice"
   * Yeni bir depolama hesabı oluşturmak için bu komut dosyasını çağırın veya kullanmak [Azure portal](https://portal.azure.com). Depolama hesabı adı tam etki alanı adının öneki olarak kullanılacak ve bu nedenle benzersiz olmalıdır. Bulut hizmeti olarak aynı adı kullanarak deneyebilirsiniz.

         New-AzureStorageAccount -ServiceName "mytestcloudservice" -Location "North Central US" -Label "mytestcloudservice"
8. Bu komut dosyasını, paket yapıdan sonra gerçekleşecek şekilde, ana bilgisayar derleme Otomasyon kablo bağlantılarını veya doğrudan Azure Powershell'den komut dosyasını çağırın.

   > [!IMPORTANT]
   > Komut dosyası her zaman silin veya bunlar algılanırsa, var olan dağıtımlar varsayılan olarak değiştirin. Bu hiçbir kullanıcıdan mümkün olduğu Otomasyon sürekli teslimat etkinleştirmek gereklidir.
   >
   >

   **Örnek Senaryo 1:** hazırlama ortamına bir hizmetin sürekli dağıtımı:

       PowerShell c:\scripts\windowsazure\PublishCloudService.ps1 -environment Staging -serviceName mycloudservice -storageAccountName mystoragesaccount -packageLocation c:\drops\app.publish\ContactManager.Azure.cspkg -cloudConfigLocation c:\drops\app.publish\ServiceConfiguration.Cloud.cscfg -subscriptionDataFile c:\scripts\default.publishsettings

   Bu genellikle doğrulama ve bir VIP takası testi tarafından izlendiğinden. VIP takas aracılığıyla yapılabilir [Azure portal](https://portal.azure.com) veya Move-Deployment cmdlet'ini kullanarak.

   **Örnek Senaryo 2:** üretim ortamına ayrılmış sınama hizmetin sürekli dağıtım

       PowerShell c:\scripts\windowsazure\PublishCloudService.ps1 -environment Production -enableDeploymentUpgrade 1 -serviceName mycloudservice -storageAccountName mystorageaccount -packageLocation c:\drops\app.publish\ContactManager.Azure.cspkg -cloudConfigLocation c:\drops\app.publish\ServiceConfiguration.Cloud.cscfg -subscriptionDataFile c:\scripts\default.publishsettings

   **Uzak Masaüstü:**

   Bu komut dosyası tarafından hedeflenen tüm bulut hizmetlerine doğru bulut hizmet sertifikası karşıya sağlamak için tek seferlik ek adımlar gerçekleştirmesi gerekir, Azure projenizdeki Uzak Masaüstü etkinleştirilirse.

   Rolleri tarafından beklenen sertifika parmak izi değerlerini bulun. Parmak izi değerleri bulut yapılandırma dosyasının (yani ServiceConfiguration.Cloud.cscfg) sertifikaları bölümünde görünür. Görüntülediğinizde, aynı zamanda Visual Studio'da uzak masaüstü yapılandırması iletişim kutusu görünür seçenekleri ve Görünüm Seçili sertifika.

       <Certificates>
             <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption" thumbprint="C33B6C432C25581601B84C80F86EC2809DC224E8" thumbprintAlgorithm="sha1" />
       </Certificates>

   Uzak Masaüstü sertifikaları, aşağıdaki cmdlet komut dosyası kullanarak bir kerelik Kurulum adım olarak karşıya yükle:

       Add-AzureCertificate -serviceName <CLOUDSERVICENAME> -certToDeploy (get-item cert:\CurrentUser\MY\<THUMBPRINT>)

   Örneğin:

       Add-AzureCertificate -serviceName 'mytestcloudservice' -certToDeploy (get-item cert:\CurrentUser\MY\C33B6C432C25581601B84C80F86EC2809DC224E8

   Alternatif olarak özel anahtara sahip sertifika dosyasını PFX dışa aktarmak ve her hedef bulut kullanan hizmet sertifikalarını karşıya yükleme [Azure portal](https://portal.azure.com).

   <!---
   Fixing broken links for Azure content migration from ACOM to DOCS. I'm unable to find a replacement links, so I'm commenting out this reference for now. The author can investigate in the future. "Read the following article to learn more: http://msdn.microsoft.com/library/windowsazure/gg443832.aspx.
   -->
   **Dağıtım vs yükseltin. Dağıtım - Sil\> yeni dağıtım**

   Komut dosyasını bir yükseltme dağıtımı varsayılan olarak gerçekleştireceği ($enableDeploymentUpgrade = 1) ne zaman hiçbir parametre geçirilen veya 1 değerini açıkça geçirilir. Tek örnekleri için tam dağıtımını daha az zaman ayırdığınız avantajı vardır. Bu ayrıca diğerlerinde çalıştıran bazı örnekleri bırakarak avantajı vardır, yüksek kullanılabilirlik gerektiren örnekleri (, güncelleştirme etki alanının yürütülmesi) yükseltilir, artı, VIP silinmeyecek için.

   Yükseltme dağıtımı devre dışı bırakılabilir komut dosyasında ($enableDeploymentUpgrade = 0) veya geçirerek *- enableDeploymentUpgrade 0* bir parametre olarak, hangi değiştirir önce tüm mevcut dağıtımı silin ve yenisini oluşturmak için komut dosyası davranışı dağıtımı.

   > [!IMPORTANT]
   > Komut dosyası her zaman silin veya bunlar algılanırsa, var olan dağıtımlar varsayılan olarak değiştirin. Hiçbir kullanıcı/işleci isteyen mümkün olduğu Otomasyon sürekli teslimat etkinleştirmek için gereken budur.
   >
   >

## <a name="5-publish-a-package-using-tfs-team-build"></a>5: TFS ekip kullanarak paket yayımlama
Bu isteğe bağlı adım, TFS yayımlama Azure paketi yapı işleyen ekip 4. adımda oluşturduğunuz betiğin bağlanır. Bu, iş akışının sonunda bir yayımlama etkinliği çalışır, yapı tanımı tarafından kullanılan işlem şablonunu değiştirme kapsar. Yayımla etkinlik parametrelerinde derlemeden geçirme, PowerShell komutunu çalıştırır. MSBuild çıktısını hedefler ve komut dosyası yayımlama standart yapı çıktı yöneltilen.

1. Yapı tanımı sorumlu Düzenle sürekli dağıtın.
2. Seçin **işlem** sekmesi.
3. İzleyin [bu yönergeleri](http://msdn.microsoft.com/library/dd647551.aspx) derleme işlem şablonu için bir etkinlik projesi eklemek için varsayılan şablonunu indirebilir, projeye ekleyin ve iade etme. Derleme işlem şablonu AzureBuildProcessTemplate gibi yeni bir ad verin.
4. Geri dönüp **işlem** sekmesini tıklatın ve kullanmak **ayrıntıları göster** kullanılabilir yapı işlem şablonları listesini gösterme. Seçin **yeni...**  düğmesine tıklayın ve yalnızca eklenir ve iade projesine gidin. Yeni oluşturduğunuz şablonu bulun ve seçin **Tamam**.
5. Düzenleme için seçilen işlem şablonunu açın. İş Akışı Tasarımcısı'nda doğrudan veya XML Düzenleyicisi'ni XAML ile çalışmak için açabilirsiniz.
6. İş Akışı Tasarımcısı'nın bağımsız değişkenleri sekmesinden ayrı satırı öğeleri olarak yeni bağımsız değişkenleri aşağıdaki listeye ekleyin. Tüm bağımsız değişkenler yönü olması gerekir, = ve türü = String. Bu akış parametrelerinin iş akışına yapı tanımından sonra Yayımla betik çağırmak için kullanılan kullanılır.

       SubscriptionName
       StorageAccountName
       CloudConfigLocation
       PackageLocation
       Environment
       SubscriptionDataFileLocation
       PublishScriptLocation
       ServiceName

   ![Bağımsız değişkenler listesi][3]

   Karşılık gelen XAML şöyle görünür:

       <Activity  _ />
         <x:Members>
           <x:Property Name="BuildSettings" Type="InArgument(mtbwa:BuildSettings)" />
           <x:Property Name="TestSpecs" Type="InArgument(mtbwa:TestSpecList)" />
           <x:Property Name="BuildNumberFormat" Type="InArgument(x:String)" />
           <x:Property Name="CleanWorkspace" Type="InArgument(mtbwa:CleanWorkspaceOption)" />
           <x:Property Name="RunCodeAnalysis" Type="InArgument(mtbwa:CodeAnalysisOption)" />
           <x:Property Name="SourceAndSymbolServerSettings" Type="InArgument(mtbwa:SourceAndSymbolServerSettings)" />
           <x:Property Name="AgentSettings" Type="InArgument(mtbwa:AgentSettings)" />
           <x:Property Name="AssociateChangesetsAndWorkItems" Type="InArgument(x:Boolean)" />
           <x:Property Name="CreateWorkItem" Type="InArgument(x:Boolean)" />
           <x:Property Name="DropBuild" Type="InArgument(x:Boolean)" />
           <x:Property Name="MSBuildArguments" Type="InArgument(x:String)" />
           <x:Property Name="MSBuildPlatform" Type="InArgument(mtbwa:ToolPlatform)" />
           <x:Property Name="PerformTestImpactAnalysis" Type="InArgument(x:Boolean)" />
           <x:Property Name="CreateLabel" Type="InArgument(x:Boolean)" />
           <x:Property Name="DisableTests" Type="InArgument(x:Boolean)" />
           <x:Property Name="GetVersion" Type="InArgument(x:String)" />
           <x:Property Name="PrivateDropLocation" Type="InArgument(x:String)" />
           <x:Property Name="Verbosity" Type="InArgument(mtbw:BuildVerbosity)" />
           <x:Property Name="Metadata" Type="mtbw:ProcessParameterMetadataCollection" />
           <x:Property Name="SupportedReasons" Type="mtbc:BuildReason" />
           <x:Property Name="SubscriptionName" Type="InArgument(x:String)" />
           <x:Property Name="StorageAccountName" Type="InArgument(x:String)" />
           <x:Property Name="CloudConfigLocation" Type="InArgument(x:String)" />
           <x:Property Name="PackageLocation" Type="InArgument(x:String)" />
           <x:Property Name="Environment" Type="InArgument(x:String)" />
           <x:Property Name="SubscriptionDataFileLocation" Type="InArgument(x:String)" />
           <x:Property Name="PublishScriptLocation" Type="InArgument(x:String)" />
           <x:Property Name="ServiceName" Type="InArgument(x:String)" />
         </x:Members>

         <this:Process.MSBuildArguments>
7. Yeni bir sıra Aracısını Çalıştır sonuna ekleyin:

   1. Geçerli komut dosyası için denetlemek için bir If ifadesinden etkinlik ekleyerek başlayın. Koşul için bu değeri ayarlayın:

          Not String.IsNullOrEmpty(PublishScriptLocation)
   2. Ardından bu durumda yeni bir dizi etkinlik varsa deyiminin ekleyin. 'Başlangıç Yayımla' görünen adını ayarlayın
   3. Halen seçili dizisi ile başlangıç yayımlama, ayrı satırı öğeleri değişkenleri sekmesi iş akışı Tasarımcısı'nın olarak aşağıdaki yeni değişkenleri listesi ekleyin. Tüm değişkenleri değişken türü olmalıdır = dize ve kapsam başlangıç = yayımlayın. Bu akış parametrelerinin iş akışına yapı tanımından sonra Yayımla betik çağırmak için kullanılan kullanılır.

      * Dize türünde SubscriptionDataFilePath
      * Dize türünde PublishScriptFilePath

        ![Yeni değişkenleri][4]
   4. TFS 2012 veya önceki bir sürümünü kullanıyorsanız, yeni sıra başında ConvertWorkspaceItem etkinliği ekleyin. TFS 2013 veya üzeri kullanıyorsanız, yeni sıra başında GetLocalPath etkinliği ekleyin. Bir ConvertWorkspaceItem için özellikleri aşağıdaki gibi ayarlayın: yönü ServerToLocal, DisplayName = 'Dönüştürme Yayımla betik filename' = giriş 'PublishScriptLocation', sonuç = 'PublishScriptFilePath', çalışma alanı = = 'Çalışma alanı'. GetLocalPath etkinliği için 'PublishScriptLocation' IncomingPath ve sonucu 'PublishScriptFilePath' özelliğini ayarlayın. Bu etkinlik konumlardan TFS sunucusu (varsa) standart yerel disk yoluna Yayımla betik yolu dönüştürür.
   5. TFS 2012 veya önceki bir sürümünü kullanıyorsanız, başka bir ConvertWorkspaceItem etkinlik yeni sırası sonuna ekleyin. Yön ServerToLocal, DisplayName = 'abonelik filename Dönüştür' = giriş 'SubscriptionDataFileLocation', sonuç = 'SubscriptionDataFilePath', çalışma alanı = = 'Çalışma alanı'. TFS 2013 veya üzeri kullanıyorsanız, başka bir GetLocalPath ekleyin. IncomingPath 'SubscriptionDataFileLocation' = ve sonuç 'SubscriptionDataFilePath.' =
   6. InvokeProcess aktivite yeni sırası sonuna ekleyin.
      Bu etkinlik, derleme tanımına göre geçirilen bağımsız değişkenlerle PowerShell.exe çağırır.

      + Bağımsız değişkenler String.Format = ("-""{0}" "- serviceName {1} - storageAccountName {2} - packageLocation""{3}" "- cloudConfigLocation""{4}" "- subscriptionDataFile""{5}" "- selectedSubscription {6} dosya-ortam""{7}" "",  PublishScriptFilePath, ServiceName, StorageAccountName, PackageLocation, CloudConfigLocation, SubscriptionDataFilePath, varlığıyla SubscriptionName, ortamı)
      + DisplayName Execute = betik yayımlama
      + FileName = "PowerShell" (tırnak işaretleri dahil)
      + OutputEncoding System.Text.Encoding.GetEncoding(System.Globalization.CultureInfo.InstalledUICulture.TextInfo.OEMCodePage) =
   7. İçinde **işlemek standart çıktı** bölümünde InvokeProcess textbox, 'data' textbox değerine ayarlayın. Bu standart çıktı verilerini depolamak üzere bir değişkendir.
   8. WriteBuildMessage etkinlik eklemek yalnızca aşağıda **işlemek standart çıktı** bölümü. Önem düzeyini belirlemek = 'Microsoft.TeamFoundation.Build.Client.BuildMessageImportance.High' ve ileti = 'data'. Bu komut dosyasının standart çıktı yapı çıktı yazılmış sağlar.
   9. İçinde **hata çıkış işleme** bölümünde InvokeProcess textbox, 'data' textbox değerine ayarlayın. Bu standart hata verileri depolamak için bir değişkendir.
   10. WriteBuildError etkinlik eklemek yalnızca aşağıda **hata çıkış işleme** bölümü. İleti Ayarla = 'data'. Bu komut dosyasının standart hatalar yapı hata çıktısı yazılan sağlar.
   11. Mavi ünlem işaretleri tarafından belirtilen tüm hataları düzeltin. Hata hakkında bir ipucu almak için ünlem işaretleri üzerine gelerek. Hataları temizlemek için iş akışını kaydedin.

   Yayımla iş akışı etkinlikleri sonucunu Tasarımcısı'nda şöyle görünür:

   ![İş akışı etkinlikleri][5]

   Yayımla iş akışı etkinlikleri sonucunu XAML'de şuna benzeyecektir:

       <If Condition="[Not String.IsNullOrEmpty(PublishScriptLocation)]" sap2010:WorkflowViewState.IdRef="If_1">
           <If.Then>
             <Sequence DisplayName="Start Publish" sap2010:WorkflowViewState.IdRef="Sequence_4">
               <Sequence.Variables>
                 <Variable x:TypeArguments="x:String" Name="SubscriptionDataFilePath" />
                 <Variable x:TypeArguments="x:String" Name="PublishScriptFilePath" />
               </Sequence.Variables>
               <mtbwa:ConvertWorkspaceItem DisplayName="Convert publish script filename" sap2010:WorkflowViewState.IdRef="ConvertWorkspaceItem_1" Input="[PublishScriptLocation]" Result="[PublishScriptFilePath]" Workspace="[Workspace]" />
               <mtbwa:ConvertWorkspaceItem DisplayName="Convert subscription filename" sap2010:WorkflowViewState.IdRef="ConvertWorkspaceItem_2" Input="[SubscriptionDataFileLocation]" Result="[SubscriptionDataFilePath]" Workspace="[Workspace]" />
               <mtbwa:InvokeProcess Arguments="[String.Format(&quot; -File &quot;&quot;{0}&quot;&quot; -serviceName {1}&#xD;&#xA;            -storageAccountName {2} -packageLocation &quot;&quot;{3}&quot;&quot;&#xD;&#xA;            -cloudConfigLocation &quot;&quot;{4}&quot;&quot; -subscriptionDataFile &quot;&quot;{5}&quot;&quot;&#xD;&#xA;            -selectedSubscription {6} -environment &quot;&quot;{7}&quot;&quot;&quot;,&#xD;&#xA;            PublishScriptFilePath, ServiceName, StorageAccountName,&#xD;&#xA;            PackageLocation, CloudConfigLocation,&#xD;&#xA;            SubscriptionDataFilePath, SubscriptionName, Environment)]" DisplayName="'Execute Publish Script'" FileName="[PowerShell]" sap2010:WorkflowViewState.IdRef="InvokeProcess_1">
                 <mtbwa:InvokeProcess.ErrorDataReceived>
                   <ActivityAction x:TypeArguments="x:String">
                     <ActivityAction.Argument>
                       <DelegateInArgument x:TypeArguments="x:String" Name="data" />
                     </ActivityAction.Argument>
                     <mtbwa:WriteBuildError Message="{x:Null}" sap2010:WorkflowViewState.IdRef="WriteBuildError_1" />
                   </ActivityAction>
                 </mtbwa:InvokeProcess.ErrorDataReceived>
                 <mtbwa:InvokeProcess.OutputDataReceived>
                   <ActivityAction x:TypeArguments="x:String">
                     <ActivityAction.Argument>
                       <DelegateInArgument x:TypeArguments="x:String" Name="data" />
                     </ActivityAction.Argument>
                     <mtbwa:WriteBuildMessage sap2010:WorkflowViewState.IdRef="WriteBuildMessage_2" Importance="[Microsoft.TeamFoundation.Build.Client.BuildMessageImportance.High]" Message="[data]" mva:VisualBasic.Settings="Assembly references and imported namespaces serialized as XML namespaces" />
                   </ActivityAction>
                 </mtbwa:InvokeProcess.OutputDataReceived>
               </mtbwa:InvokeProcess>
             </Sequence>
           </If.Then>
         </If>
       </Sequence>
8. Yapı işlem şablonu iş akışı ve iade etme Bu dosyayı kaydedin.
9. Derleme tanımını Düzenle (kapatın, zaten açıksa) ve seçin **yeni** işlem şablonları yeni şablona henüz görmüyorsanız düğmesine tıklayın.
10. Parametre özellik değerleri diğer bölümünde aşağıdaki gibi ayarlayın:

    1. CloudConfigLocation ='c:\\bırakır\\app.publish\\ServiceConfiguration.Cloud.cscfg' *bu değer türetilir: ($PublishDir)ServiceConfiguration.Cloud.cscfg*
    2. PackageLocation = ' c:\\bırakır\\app.publish\\ContactManager.Azure.cspkg' *bu değer türetilir: ($PublishDir)($ProjectName) .cspkg*
    3. PublishScriptLocation = ' c:\\betikleri\\WindowsAzure\\PublishCloudService.ps1'
    4. ServiceName = 'mycloudservicename' *uygun bulut hizmeti adını buraya kullanın*
    5. Ortam 'Hazırlama' =
    6. StorageAccountName 'mystorageaccountname' = *uygun depolama hesabı adı burada kullanın*
    7. SubscriptionDataFileLocation = ' c:\\betikleri\\WindowsAzure\\Subscription.xml'
    8. Varlığıyla SubscriptionName = 'varsayılan'

    ![Parametre özellik değerleri][6]
11. Yapı tanımı için değişiklikleri kaydedin.
12. Her iki paketi yapı yürütün ve yayımlamak için bir yapıyı sıraya al. Sürekli tümleştirme için ayarlanmış bir tetikleyici varsa, her iade Bu davranış yürütülür.

### <a name="publishcloudserviceps1-script-template"></a>PublishCloudService.ps1 komut dosyası şablonu
```
Param(  $serviceName = "",
        $storageAccountName = "",
        $packageLocation = "",
        $cloudConfigLocation = "",
        $environment = "Staging",
        $deploymentLabel = "ContinuousDeploy to $servicename",
        $timeStampFormat = "g",
        $alwaysDeleteExistingDeployments = 1,
        $enableDeploymentUpgrade = 1,
        $selectedsubscription = "default",
        $subscriptionDataFile = ""
     )


function Publish()
{
    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot -ErrorVariable a -ErrorAction silentlycontinue
    if ($a[0] -ne $null)
    {
        Write-Output "$(Get-Date -f $timeStampFormat) - No deployment is detected. Creating a new deployment. "
    }
    #check for existing deployment and then either upgrade, delete + deploy, or cancel according to $alwaysDeleteExistingDeployments and $enableDeploymentUpgrade boolean variables
    if ($deployment.Name -ne $null)
    {
        switch ($alwaysDeleteExistingDeployments)
        {
            1
            {
                switch ($enableDeploymentUpgrade)
                {
                    1  #Update deployment inplace (usually faster, cheaper, won't destroy VIP)
                    {
                        Write-Output "$(Get-Date -f $timeStampFormat) - Deployment exists in $servicename.  Upgrading deployment."
                        UpgradeDeployment
                    }
                    0  #Delete then create new deployment
                    {
                        Write-Output "$(Get-Date -f $timeStampFormat) - Deployment exists in $servicename.  Deleting deployment."
                        DeleteDeployment
                        CreateNewDeployment

                    }
                } # switch ($enableDeploymentUpgrade)
            }
            0
            {
                Write-Output "$(Get-Date -f $timeStampFormat) - ERROR: Deployment exists in $servicename.  Script execution cancelled."
                exit
            }
        } #switch ($alwaysDeleteExistingDeployments)
    } else {
            CreateNewDeployment
    }
}

function CreateNewDeployment()
{
    write-progress -id 3 -activity "Creating New Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Creating New Deployment: In progress"

    $opstat = New-AzureDeployment -Slot $slot -Package $packageLocation -Configuration $cloudConfigLocation -label $deploymentLabel -ServiceName $serviceName

    $completeDeployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $completeDeploymentID = $completeDeployment.deploymentid

    write-progress -id 3 -activity "Creating New Deployment" -completed -Status "Complete"
    Write-Output "$(Get-Date -f $timeStampFormat) - Creating New Deployment: Complete, Deployment ID: $completeDeploymentID"

    StartInstances
}

function UpgradeDeployment()
{
    write-progress -id 3 -activity "Upgrading Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Upgrading Deployment: In progress"

    # perform Update-Deployment
    $setdeployment = Set-AzureDeployment -Upgrade -Slot $slot -Package $packageLocation -Configuration $cloudConfigLocation -label $deploymentLabel -ServiceName $serviceName -Force

    $completeDeployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $completeDeploymentID = $completeDeployment.deploymentid

    write-progress -id 3 -activity "Upgrading Deployment" -completed -Status "Complete"
    Write-Output "$(Get-Date -f $timeStampFormat) - Upgrading Deployment: Complete, Deployment ID: $completeDeploymentID"
}

function DeleteDeployment()
{

    write-progress -id 2 -activity "Deleting Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Deleting Deployment: In progress"

    #WARNING - always deletes with force
    $removeDeployment = Remove-AzureDeployment -Slot $slot -ServiceName $serviceName -Force

    write-progress -id 2 -activity "Deleting Deployment: Complete" -completed -Status $removeDeployment
    Write-Output "$(Get-Date -f $timeStampFormat) - Deleting Deployment: Complete"

}

function StartInstances()
{
    write-progress -id 4 -activity "Starting Instances" -status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instances: In progress"

    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $runstatus = $deployment.Status

    if ($runstatus -ne 'Running')
    {
        $run = Set-AzureDeployment -Slot $slot -ServiceName $serviceName -Status Running
    }
    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $oldStatusStr = @("") * $deployment.RoleInstanceList.Count

    while (-not(AllInstancesRunning($deployment.RoleInstanceList)))
    {
        $i = 1
        foreach ($roleInstance in $deployment.RoleInstanceList)
        {
            $instanceName = $roleInstance.InstanceName
            $instanceStatus = $roleInstance.InstanceStatus

            if ($oldStatusStr[$i - 1] -ne $roleInstance.InstanceStatus)
            {
                $oldStatusStr[$i - 1] = $roleInstance.InstanceStatus
                Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instance '$instanceName': $instanceStatus"
            }

            write-progress -id (4 + $i) -activity "Starting Instance '$instanceName'" -status "$instanceStatus"
            $i = $i + 1
        }

        sleep -Seconds 1

        $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    }

    $i = 1
    foreach ($roleInstance in $deployment.RoleInstanceList)
    {
        $instanceName = $roleInstance.InstanceName
        $instanceStatus = $roleInstance.InstanceStatus

        if ($oldStatusStr[$i - 1] -ne $roleInstance.InstanceStatus)
        {
            $oldStatusStr[$i - 1] = $roleInstance.InstanceStatus
            Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instance '$instanceName': $instanceStatus"
        }

        $i = $i + 1
    }

    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $opstat = $deployment.Status

    write-progress -id 4 -activity "Starting Instances" -completed -status $opstat
    Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instances: $opstat"
}

function AllInstancesRunning($roleInstanceList)
{
    foreach ($roleInstance in $roleInstanceList)
    {
        if ($roleInstance.InstanceStatus -ne "ReadyRole")
        {
            return $false
        }
    }

    return $true
}

#configure powershell with Azure 1.7 modules
Import-Module Azure

#configure powershell with publishsettings for your subscription
$pubsettings = $subscriptionDataFile
Import-AzurePublishSettingsFile $pubsettings
Set-AzureSubscription -CurrentStorageAccountName $storageAccountName -SubscriptionName $selectedsubscription
Select-AzureSubscription $selectedsubscription

#set remaining environment variables for Azure cmdlets
$subscription = Get-AzureSubscription $selectedsubscription
$subscriptionname = $subscription.subscriptionname
$subscriptionid = $subscription.subscriptionid
$slot = $environment

#main driver - publish & write progress to activity log
Write-Output "$(Get-Date -f $timeStampFormat) - Azure Cloud Service deploy script started."
Write-Output "$(Get-Date -f $timeStampFormat) - Preparing deployment of $deploymentLabel for $subscriptionname with Subscription ID $subscriptionid."

Publish

$deployment = Get-AzureDeployment -slot $slot -serviceName $servicename
$deploymentUrl = $deployment.Url

Write-Output "$(Get-Date -f $timeStampFormat) - Created Cloud Service with URL $deploymentUrl."
Write-Output "$(Get-Date -f $timeStampFormat) - Azure Cloud Service deploy script finished."
```

## <a name="next-steps"></a>Sonraki adımlar
Kesintisiz teslim kullanırken uzaktan hata ayıklamayı etkinleştirmek için bkz: [kesintisiz teslim için Azure yayımlama için kullanırken uzaktan hata ayıklamayı etkinleştirme](cloud-services-virtual-machines-dotnet-continuous-delivery-remote-debugging.md).

[Team Foundation Build Service]: https://msdn.microsoft.com/library/ee259687.aspx
[.NET Framework 4]: https://www.microsoft.com/download/details.aspx?id=17851
[.NET Framework 4.5]: https://www.microsoft.com/download/details.aspx?id=30653
[.NET Framework 4.5.2]: https://www.microsoft.com/download/details.aspx?id=42643
[Scale out your build system]: https://msdn.microsoft.com/library/dd793166.aspx
[Deploy and configure a build server]: https://msdn.microsoft.com/library/ms181712.aspx
[Azure PowerShell cmdlets]: /powershell/azureps-cmdlets-docs
[the .publishsettings file]: https://manage.windowsazure.com/download/publishprofile.aspx?wa=wsignin1.0
[0]: ./media/cloud-services-dotnet-continuous-delivery/tfs-01bc.png
[2]: ./media/cloud-services-dotnet-continuous-delivery/tfs-02.png
[3]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-03.png
[4]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-04.png
[5]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-05.png
[6]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-06.png
