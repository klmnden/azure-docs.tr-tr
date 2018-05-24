---
title: Öğretici - Team Services ile Azure üzerinde Jenkins sanal makinelerinden sürekli tümleştirme (CI)/sürekli dağıtım (CD) | Microsoft Docs
description: Bu öğreticide, Visual Studio Team Services veya Microsoft Team Foundation Server’da Release Management’tan Azure üzerinde Jenkins sanal makinelerini kullanarak bir Node.js uygulamasının sürekli tümleştirme (CI) ve sürekli dağıtımını (CD) nasıl ayarlanacağını öğreneceksiniz
author: ahomer
manager: douge
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 10/19/2017
ms.author: ahomer
ms.custom: mvc
ms.openlocfilehash: fc301edf13f8e6874f0b77440e2b0dc01b2a55fc
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="tutorial-deploy-your-app-to-linux-virtual-machines-in-azure-with-using-jenkins-and-visual-studio-team-services"></a>Öğretici: Jenkins ve Visual Studio Team Services kullanarak uygulamanızı Azure üzerinde Linux sanal makinelerine dağıtma

Sürekli tümleştirme (CI) ve sürekli dağıtım (CD), kodunuzu derleyebileceğiniz, yayınlayabileceğiniz ve dağıtabileceğiniz bir işlem hattı oluşturur. Visual Studio Team Services, Azure’a dağıtım için eksiksiz ve tam özellikli bir dizi CI/CD otomasyon aracı sağlar. Jenkins, CI/CD otomasyonu sağlayan, popüler bir üçüncü taraf CI/CD sunucu tabanlı aracıdır. Bulut uygulamanızı ve hizmetinizi nasıl sunacağınızı özelleştirmek için Team Services ve Jenkins’i birlikte kullanabilirsiniz.

Bu öğreticide, Node.js web uygulaması derlemek için Jenkins’i kullanacaksınız. Daha sonra Team Services veya Team Foundation Server kullanarak bunu Linux sanal makineleri içeren bir [dağıtım grubuna](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) dağıtacaksınız. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Örnek uygulamayı alma.
> * Jenkins eklentilerini yapılandırma.
> * Node.js için Jenkins Freestyle projesi yapılandırma.
> * Team Services tümleştirmesi için Jenkins’i yapılandırma.
> * Jenkins hizmet uç noktası oluşturma.
> * Azure sanal makineleri için dağıtım grubu oluşturma.
> * Team Services yayın tanımı oluşturma.
> * El ile ve CI ile tetiklenen dağıtımlar yürütme.

## <a name="before-you-begin"></a>Başlamadan önce

* Bir Jenkins sunucusuna erişmeniz gerekir. Henüz bir Jenkins sunucusu oluşturmadıysanız bkz. [Azure sanal makinesinde Jenkins yöneticisi oluşturma](https://docs.microsoft.com/azure/jenkins/install-jenkins-solution-template). 

* Team Services hesabınızda (**https://{youraccount}.visualstudio.com**) oturum açın. 
  [Ücretsiz bir Team Services hesabı](https://go.microsoft.com/fwlink/?LinkId=307137&clcid=0x409&wt.mc_id=o~msft~vscom~home-vsts-hero~27308&campaign=o~msft~vscom~home-vsts-hero~27308) alabilirsiniz.

  > [!NOTE]
  > Daha fazla bilgi için bkz. [Team Services’a bağlanma](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services).

*  Dağıtım hedefi için bir Linux sanal makinesi gerekir.  Daha fazla bilgi için bkz. [Azure CLI ile Linux sanal makineleri oluşturma ve yönetme](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-vm).

*  Sanal makineniz için 80 numaralı gelen bağlantı noktasını açın. Daha fazla bilgi için bkz. [Azure portalını kullanarak ağ güvenlik grupları oluşturma](https://docs.microsoft.com/azure/virtual-network/virtual-networks-create-nsg-arm-pportal).

## <a name="get-the-sample-app"></a>Örnek uygulamayı alma

Bir Git deposunda depolanan, dağıtılacak bir uygulamanız olması gerekir.
Bu öğretici için, [GitHub’dan erişebileceğiniz bu örnek uygulamayı](https://github.com/azooinmyluggage/fabrikam-node) kullanmanızı öneririz. Bu öğretici, Node.js ve bir uygulama yüklemek için kullanılan örnek bir betik içerir. Kendi deponuzla çalışmak istiyorsanız benzer bir örnek yapılandırmanız gerekir.

Bu uygulamanın çatalını oluşturun ve bu öğreticinin daha sonraki adımlarında kullanmak üzere konumu (URL) not edin. Daha fazla bilgi için bkz. [Depo çatalı oluşturma](https://help.github.com/articles/fork-a-repo/).    

> [!NOTE]
> Uygulama, [Yeoman](http://yeoman.io/learning/index.html) aracılığıyla oluşturulmuştur. Express, bower ve grunt kullanır. Ayrıca bağımlılıklar olarak bazı npm paketlerini içerir.
> Örnek, Nginx’i ayarlayan ve uygulamayı dağıtan bir betik de içerir. Sanal makinelerde yürütülür. Betik özellikle:
> 1. Düğüm, Nginx ve PM2 yükler.
> 2. Nginx ve PM2’yi yapılandırır.
> 3. Düğüm uygulamasını başlatır.

## <a name="configure-jenkins-plug-ins"></a>Jenkins eklentilerini yapılandırma

İlk olarak iki Jenkins eklentisini yapılandırmanız gerekir: **NodeJS** ve **VS Team Services Sürekli Dağıtımı**.

1. Jenkins hesabınızı açın ve **Jenkins’i yönet** seçeneğini belirleyin.
2. **Jenkins’i yönet** sayfasında **Eklentileri yönet** seçeneğini belirleyin.
3. Listeyi filtreleyerek **NodeJS** eklentisini bulun ve **Yeniden başlatmadan yükle** seçeneğini belirleyin.
    ![Jenkins’e NodeJS eklentisini ekleme](media/tutorial-build-deploy-jenkins/jenkins-nodejs-plugin.png)
4. Listeyi filtreleyerek **VS Team Services Sürekli Dağıtımı** eklentisini bulun ve **Yeniden başlatmadan yükle** seçeneğini belirleyin.
5. Jenkins panosuna geri dönüp **Jenkins’i yönet** seçeneğini belirleyin.
6. **Genel Araç Yapılandırması** seçeneğini belirleyin. **NodeJS** öğesini bulun ve **NodeJS yüklemeleri** seçeneğini belirleyin.
7. **Otomatik olarak yükle** seçeneğini belirleyin ve bir **Ad** değeri girin.
8. **Kaydet**’i seçin.

## <a name="configure-a-jenkins-freestyle-project-for-nodejs"></a>Node.js için Jenkins Freestyle projesi yapılandırma

1. **Yeni Öğe**’yi seçin. Bir öğe adı girin.
2. **Serbest stilde proje**’yi seçin. **Tamam**’ı seçin.
3. **Kaynak Kodu Yönetimi** sekmesinde **Git**’i seçin ve uygulama kodunuzu içeren deponun ve dalın ayrıntılarını girin.    
    ![Derlemenize bir depo ekleme](media/tutorial-build-deploy-jenkins/jenkins-git.png)
4. **Derleme Tetikleyicileri** sekmesinde **SCM’yi Yokla** seçeneğini belirleyin ve üç dakikada bir Git deposundaki değişiklikleri yoklamak için `H/03 * * * *` zamanlamasını girin. 
5. **Derleme Ortamı** sekmesinde **Düğüm &amp; npm bin/ klasör YOLUNU sağla** seçeneğini belirleyin ve **NodeJS Yüklemesi** değerini seçin. **npmrc dosyası** değerini **use system default** olarak ayarlanmış şekilde bırakın.
6. **Derleme** sekmesinde **Kabuk yürüt**’ü seçin ve `npm install` komutunu girerek tüm bağımlılıkların güncelleştirildiğinden emin olun.


## <a name="configure-jenkins-for-team-services-integration"></a>Team Services tümleştirmesi için Jenkins’i yapılandırma

> [!NOTE]
> Aşağıdaki adımlar için kullandığınız kişisel erişim belirtecinin, Team Services’te *Yayınlama* (okuma, yazma, yürütme ve yönetme) iznini içerdiğinden emin olun.
 
1.  Yoksa, Team Services hesabınızda bir kişisel erişim belirteci oluşturun. Jenkins, Team Services hesabınıza erişmek için bu bilgileri zorunlu tutar. Bu bölümün ilerleyen kısımlarındaki adımlar için belirteç bilgilerini saklayın.
  
    Belirteç oluşturma hakkında bilgi edinmek için [VSTS ve TFS için nasıl kişisel erişim belirteci oluştururum?](https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) bölümünü okuyun.
2. **Derleme Sonrası Eylemler** sekmesinde **Derleme sonrası eylem ekle** seçeneğini belirleyin. **Yapıtları arşivle**’yi seçin.
3. **Arşivlenecek dosyalar** için `**/*` değerini girerek tüm dosyaları dahil edin.
4. Başka bir eylem oluşturmak için **Derleme sonrası eylem ekle**’yi seçin.
5. **TFS/Team Services’te yayınlamayı tetikle** seçeneğini belirleyin. Team Services hesabınızın URI’sini girin; örn. **https://{your-account-name}.visualstudio.com**.
6. **Takım Projesi** adını girin.
7. Yayın tanımı için bir ad seçin. (Daha sonra Team Services’te bu yayın tanımını oluşturursunuz.)
8. Team Services veya Team Foundation Server ortamınıza bağlanmak için kimlik bilgilerini girin:
   - Team Services kullanıyorsanız, **Kullanıcı adı** alanını boş bırakın. 
   - Team Foundation Server’ın şirket içi bir sürümünü kullanıyorsanız kullanıcı adı ve parola girin.    
   ![Jenkins derleme sonrası eylemlerini yapılandırma](media/tutorial-build-deploy-jenkins/trigger-release-from-jenkins.png)
5. Jenkins projesini kaydedin.


## <a name="create-a-jenkins-service-endpoint"></a>Jenkins hizmet uç noktası oluşturma

Hizmet uç noktası, Team Services'in Jenkins’e bağlanmasına olanak sağlar.

1. Team Services’te **Hizmetler** sayfasını açın, **Yeni Hizmet Uç Noktası** listesini açın ve **Jenkins**’i seçin.
   ![Jenkins uç noktası ekleme](media/tutorial-build-deploy-jenkins/add-jenkins-endpoint.png)
2. Bağlantı için bir ad girin.
3. Jenkins sunucunuzun URL’sini girin ve **Güvenilmeyen SSL sertifikalarını kabul et** seçeneğini belirleyin. Örnek bir URL: **http://{YourJenkinsURL}.westcentralus.cloudapp.azure.com**.
4. Jenkins hesabınız için kullanıcı adı ve parolayı girin.
5. Bilgilerin doğru olup olmadığını denetlemek için **Bağlantıyı doğrula**’yı seçin.
6. Hizmet uç noktasını oluşturmak için **Tamam**’ı seçin.

## <a name="create-a-deployment-group-for-azure-virtual-machines"></a>Azure sanal makineleri için dağıtım grubu oluşturma

Yayın tanımının sanal makinenize dağıtılabilmesi için Team Services aracısını kaydetmek üzere bir [dağıtım grubunuz](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) olması gerekir. Dağıtım grupları, dağıtım için hedef makinelerin mantıksal gruplarının tanımlanmasını ve her bir makineye gerekli aracının yüklenmesini kolaylaştırır.

   > [!NOTE]
   > Aşağıdaki yordamda, önkoşulları yüklediğinizden ve *betiği sudo ayrıcalıklarıyla çalıştırmadığınızdan* emin olun.

1. **Derleme ve Yayın** hub’ının **Yayınlar** sekmesini açın, **Dağıtım grupları**’nı açın ve **+ Yeni**’yi seçin.
2. Dağıtım grubu için bir ad ve isteğe bağlı bir açıklama girin. Ardından **Oluştur**’u seçin.
3. Dağıtım hedefi sanal makineniz için işletim sistemini seçin. Örneğin, **Ubuntu 16.04+** seçeneğini belirleyin.
4. **Kimlik doğrulaması için betikte kişisel bir erişim belirteci kullan** seçeneğini belirleyin.
5. **Sistem önkoşulları** bağlantısını seçin. İşletim sisteminiz için önkoşullarını yükleyin.
6. **Betiği panoya kopyala**’yı seçerek betiği kopyalayın.
7. Dağıtım hedefi sanal makinenizde oturum açın ve betiği çalıştırın. Betiği sudo ayrıcalıklarıyla çalıştırmayın.
8. Yüklemeden sonra sizden dağıtım grubu etiketleri istenir. Varsayılanları kabul edin.
9. Team Services’te, **Dağıtım Grupları** bölümündeki **Hedefler** kısmında yeni kaydettiğiniz sanal makinenizin olup olmadığını denetleyin.

## <a name="create-a-team-services-release-definition"></a>Team Services yayın tanımı oluşturma

Yayın tanımı, Team Services’ın uygulamayı dağıtmak için kullandığı işlemi belirtir. Bu örnekte, bir kabuk betiği yürütürsünüz.

Team Services’te yayın tanımı oluşturmak için:

1. **Derleme ve Yayın** hub’ının **Yayınlar** sekmesini açın ve **Yayın tanımı oluştur**’u seçin. 
2. **Boş** şablonunu seçerek **Boş işlem** ile başlamayı seçin.
3. **Yapıtlar** bölümünde **+ Yapıt Ekle** seçeneğini belirleyin ve **Kaynak türü** için **Jenkins**’i seçin. Jenkins hizmet uç noktası bağlantınızı seçin. Ardından Jenkins kaynak işini seçin ve **Ekle** seçeneğini belirleyin.
4. **Ortam 1**’in yanındaki üç noktayı seçin. **Dağıtım grubu aşaması ekle**’yi seçin.
5. Dağıtım grubunuzu seçin.
5. **Dağıtım grubu aşaması**’na görev eklemek için **+** seçeneğini belirleyin.
6. **Kabuk Betiği** görevini seçin ve **Ekle** seçeneğini belirleyin. **Kabuk Betiği** görevi, Node.js’yi yükleyip uygulamayı başlatmak için her bir sunucuda çalıştırılacak bir betik için yapılandırmayı sağlar.
8. **Betik Yolu** için **$(System.DefaultWorkingDirectory)/Fabrikam-Node/deployscript.sh** girin.
9. **Gelişmiş**’i seçin ve **Çalışma Dizinini Belirtin** seçeneğini etkinleştirin.
10. **Çalışma Dizini** için **$(System.DefaultWorkingDirectory)/Fabrikam-Node** girin.
11. Yayın tanımının adını, Jenkins’te derlemenin **Derleme Sonrası Eylemler** sekmesinde belirttiğiniz ad olacak şekilde düzenleyin. Jenkins, kaynak yapıtlar güncelleştirildiğinde yeni yayını tetikleyebilmesi için bu adı zorunlu tutar.
12. **Kaydet**’i seçin ve **Tamam**’ı seçerek yayın tanımını kaydedin.

## <a name="execute-manual-and-ci-triggered-deployments"></a>El ile ve CI ile tetiklenen dağıtımlar yürütme

1. **+ Yayın**’ı seçin ve **Yayın Oluştur** seçeneğini belirleyin.
2. Vurgulanan açılır listede tamamladığınız derlemeyi seçin ve **Kuyruk** seçeneğini belirleyin.
3. Açılır iletide yayın bağlantısını seçin. Örneğin: "**Yayın-1** yayını oluşturulmuştur."
4. Yayın konsolu çıktısını izlemek için **Günlükler** sekmesini açın.
5. Tarayıcınızda, dağıtım grubunuza eklediğiniz sunuculardan birinin URL’sini açın. Örneğin, **http://{your-server-ip-address}** girin.
6. Kaynak Git deposuna gidin ve app/views/index.jade dosyasındaki **h1** başlığının içeriklerini, değiştirilen bazı metinlerle değiştirin.
7. Değişikliğinizi işleyin.
8. Birkaç dakika sonra, Team Services veya Team Foundation Server’ın **Yayınlar** sayfasında yeni bir yayının oluşturulduğunu görürsünüz. Gerçekleşen dağıtımı görmek için yayını açın. Tebrikler!

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, derleme için Jenkins’i ve yayın için Team Services’i kullanarak bir uygulamanın Azure’a dağıtımını otomatikleştirdiniz. Şunları öğrendiniz:

> [!div class="checklist"]
> * Jenkins’te uygulamanızı derleme.
> * Team Services tümleştirmesi için Jenkins’i yapılandırma.
> * Azure sanal makineleri için dağıtım grubu oluşturma.
> * Sanal makineleri yapılandıran ve uygulamayı dağıtan bir yayın tanımı oluşturma.

LAMP (Linux, Apache, MySQL ve PHP) yığınını dağıtma hakkında bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [LAMP yığını dağıtma](tutorial-lamp-stack.md)
