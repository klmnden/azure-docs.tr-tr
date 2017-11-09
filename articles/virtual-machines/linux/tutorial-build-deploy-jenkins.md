---
title: CI/CD'den Jenkins Team Services ile Azure vm'lerinin | Microsoft Docs
description: "Jenkins Azure VM'ler için yayın Yönetimi Visual Studio Team Services veya Microsoft Team Foundation Server kullanarak sürekli tümleştirme (CI) ve sürekli dağıtımı (CD) bir Node.js uygulaması ayarlayın"
author: ahomer
manager: douge
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 10/19/2017
ms.author: ahomer
ms.custom: mvc
ms.openlocfilehash: c96aafeb05293ccdc4c30c2b828cead1dfdb157c
ms.sourcegitcommit: adf6a4c89364394931c1d29e4057a50799c90fc0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/09/2017
---
# <a name="deploy-your-app-to-linux-vms-by-using-jenkins-and-team-services"></a>Jenkins ve Team Services kullanarak Linux VM'ler için uygulamanızı dağıtma

Sürekli Tümleştirme (CI) ve sürekli dağıtımı (CD) olarak derleme, sürüm ve kullanabilirsiniz, kodunuzu dağıtmak işlem hattı oluşturur. Visual Studio Team Services CI/CD Otomasyon Araçları'nın tam, tam özellikli bir dizi Azure'a dağıtılmasını sağlar. Jenkins CI/CD Otomasyon de sağlayan bir popüler üçüncü taraf CI/CD sunucu tabanlı araçtır. Team Services ve Jenkins birlikte bulut uygulaması veya hizmeti teslim nasıl özelleştirmek için kullanabilirsiniz.

Bu öğreticide, bir Node.js web uygulaması oluşturmak için Jenkins kullanın. Ardından Team Services veya Team Foundation Server için dağıtmak için kullandığınız bir [dağıtım grubu](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) Linux sanal makineleri (VM'ler) içerir.

Yapacaklarınız:

> [!div class="checklist"]
> * Örnek uygulamayı Al.
> * Jenkins eklentileri yapılandırın.
> * Jenkins Freestyle projeyi Node.js için yapılandırın.
> * Jenkins Team Services tümleştirme için yapılandırın.
> * Jenkins hizmet uç noktası oluşturun.
> * Azure sanal makineler için bir dağıtım grubu oluşturun.
> * Bir Team Services sürüm tanımı oluşturun.
> * El ile ve CI tetiklenen dağıtımları yürütün.

## <a name="before-you-begin"></a>Başlamadan önce

* Jenkins sunucuya erişmeniz gerekir. Jenkins server henüz oluşturmadıysanız, bkz: [Jenkins asıl bir Azure sanal makinede oluşturmak](https://docs.microsoft.com/en-us/azure/jenkins/install-jenkins-solution-template). 

* Team Services hesabınızda oturum açın (**https://{youraccount}.visualstudio.com**). 
  Alabileceğiniz bir [serbest Team Services hesabı](https://go.microsoft.com/fwlink/?LinkId=307137&clcid=0x409&wt.mc_id=o~msft~vscom~home-vsts-hero~27308&campaign=o~msft~vscom~home-vsts-hero~27308).

  > [!NOTE]
  > Daha fazla bilgi için bkz: [Team Services Bağlan](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services).

*  Linux sanal makine için bir dağıtım hedef gerekir.  Daha fazla bilgi için bkz: [oluşturma ve Azure CLI ile Linux sanal makineleri yönetme](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/tutorial-manage-vm).

*  Gelen bağlantı noktası 80, sanal makine için açın. Daha fazla bilgi için bkz: [Azure Portalı'nı kullanarak ağ güvenlik grupları oluşturma](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-create-nsg-arm-pportal).

## <a name="get-the-sample-app"></a>Örnek uygulamayı Al

Bir uygulamayı dağıtmak için bir Git deposu içinde depolanan gerekir.
Bu öğretici için kullanmanızı öneririz [Github'dan Bu örnek uygulama](https://github.com/azooinmyluggage/fabrikam-node). Bu öğretici, Node.js ve bir uygulama yüklemek için kullanılan bir örnek komut dosyası içerir. Kendi deposuyla çalışmak isterseniz, benzer bir örnek yapılandırmanız gerekir.

Bu uygulamanın çatalı oluşturun ve bu öğreticinin daha sonraki adımlarda kullanılmak konumu (URL) not edin. Daha fazla bilgi için bkz: [bir depoyu Çatallaştırma](https://help.github.com/articles/fork-a-repo/).    

> [!NOTE]
> Uygulama aracılığıyla oluşturulan [Yeoman](http://yeoman.io/learning/index.html). Express, bower ve grunt kullanır. Ve bazı npm paketler bağımlılık vardır.
> Örnek, Nginx ayarlar ve uygulamayı dağıtır bir betik de içerir. Sanal makinelerde yürütülür. Özellikle, komut dosyası:
> 1. Düğüm, Nginx ve PM2 yükler.
> 2. Nginx ve PM2 yapılandırır.
> 3. Düğüm uygulama başlatır.

## <a name="configure-jenkins-plug-ins"></a>Jenkins eklentilerini yapılandırma

İlk olarak, iki Jenkins eklentileri yapılandırmanız gerekir: **NodeJS** ve **VS Team Services sürekli dağıtımı**.

1. Jenkins hesabınızı açın ve seçin **yönetmek Jenkins**.
2. Üzerinde **yönetmek Jenkins** sayfasında, **eklentileri yönetme**.
3. Listenin bulmak için filtre **NodeJS** eklenti ve select **yeniden yükleme** seçeneği.
    ![Jenkins için NodeJS eklenti ekleme](media/tutorial-build-deploy-jenkins/jenkins-nodejs-plugin.png)
4. Listenin bulmak için filtre **VS Team Services sürekli dağıtımı** seçin ve eklenti **yeniden yükleme** seçeneği.
5. Geri dönerek seçin ve Jenkins Pano **yönetmek Jenkins**.
6. Seçin **genel aracı yapılandırma**. Bul **NodeJS** seçip **NodeJS yüklemeleri**.
7. Seçin **otomatik olarak yüklemek** seçeneğini ve ardından girin bir **adı** değeri.
8. **Kaydet**’i seçin.

## <a name="configure-a-jenkins-freestyle-project-for-nodejs"></a>Jenkins Freestyle projeyi Node.js için yapılandırın

1. Seçin **yeni öğe**. Bir öğe adı girin.
2. Seçin **Serbest stilde proje**. **Tamam**’ı seçin.
3. Üzerinde **kaynak kodu Yönetimi** sekmesine **Git** ve uygulama kodu içeren depo ve şube ayrıntılarını girin.    
    ![Bir depo, derleme ekleyin](media/tutorial-build-deploy-jenkins/jenkins-git.png)
4. Üzerinde **yapı tetikleyicileri** sekmesine **yoklama SCM** ve zamanlama girin `H/03 * * * *` değişiklikleri için Git deposu üç dakikada yoklamak için. 
5. Üzerinde **yapı ortamı** sekmesine **sağlayan düğüm &amp; npm bin / klasör yolu** seçip **NodeJS yükleme** değeri. Bırakın **npmrc dosya** kümesine **sistem varsayılanını kullanmak**.
6. Üzerinde **yapı** sekmesine **Kabuk yürütme** ve komutu girin `npm install` tüm bağımlılıkları güncelleştirildiğinden emin olmak için.


## <a name="configure-jenkins-for-team-services-integration"></a>Jenkins Team Services tümleştirme için yapılandırma

> [!NOTE]
> İçin aşağıdaki adımları kullanın ve kişisel erişim belirteci (PAT) içerdiğinden emin olun *sürüm* (okuma, yazma, yürütme ve yönetme) Team Services izni.
 
1.  Zaten yoksa, bir PAT, Team Services hesabı oluşturun. Jenkins Team Services hesabınıza erişmek için bu bilgileri gerektirir. Bu bölümde yaklaşan adımları için belirteç bilgilerini depolamak üzere emin olun.
  
    Bir belirteç oluşturmak nasıl öğrenmek için [nasıl oluşturabilirim kişisel erişim belirteci VSTS ve TFS için?](https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate).
2. İçinde **oluşturma sonrası eylemleri** sekmesine **oluşturma sonrası eylem eklemek**. Seçin **yapıları arşiv**.
3. İçin **arşivlemek için dosyaları**, girin `**/*` tüm dosyaları eklemek için.
4. Başka bir eylem oluşturmak için seçin **oluşturma sonrası eylem eklemek**.
5. Seçin **tetiklemek TFS/Team Services sürümde**. Team Services hesabınız için URI girin **https://{your-account-name}.visualstudio.com**.
6. Girin **takım projesi** adı.
7. Yayın tanımı için bir ad seçin. (Daha sonra Team Services içinde bu yayın tanımı oluşturun.)
8. Team Services veya Team Foundation Server ortamınıza bağlanmak için kimlik bilgilerini seçin:
   - Bırakın **kullanıcıadı** Team Services kullanıyorsanız, boş. 
   - Team Foundation Server şirket içi sürümünü kullanıyorsanız, bir kullanıcı adı ve parola girin.    
   ![Jenkins oluşturma sonrası eylemleri yapılandırma](media/tutorial-build-deploy-jenkins/trigger-release-from-jenkins.png)
5. Jenkins projeyi kaydedin.


## <a name="create-a-jenkins-service-endpoint"></a>Jenkins hizmet uç noktası oluşturma

Hizmet uç noktası Team Services'ı için Jenkins bağlanmasına izin verir.

1. Açık **Hizmetleri** Team Services, açık sayfasında **yeni hizmet uç noktası** listesinde ve seçin **Jenkins**.
   ![Jenkins uç nokta ekleyin](media/tutorial-build-deploy-jenkins/add-jenkins-endpoint.png)
2. Bağlantı için bir ad girin.
3. Jenkins sunucunuzun URL'sini girin ve seçin **kabul güvenilmeyen SSL sertifikalarını** seçeneği. URL bir örnek **http://{YourJenkinsURL}.westcentralus.cloudapp.azure.com**.
4. Jenkins hesabınız için kullanıcı adı ve parola girin.
5. Seçin **bağlantısını doğrulama** bilgilerin doğru olup olmadığını denetlemek için.
6. Seçin **Tamam** hizmet uç noktası oluşturulamadı.

## <a name="create-a-deployment-group-for-azure-virtual-machines"></a>Azure sanal makineler için bir dağıtım grubu oluşturun

Gereksinim duyduğunuz bir [dağıtım grubu](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) yayın tanımı sanal makinenize dağıtılabilmesi amacıyla Team Services Aracısı'nı kaydetmek için. Dağıtım grupları dağıtımı için hedef makinelere oluşan mantıksal grupları tanımlayın ve her makinede gerekli aracı yüklemek için kolay hale getirir.

   > [!NOTE]
   > Aşağıdaki yordamda, önkoşulları yüklediğinizden emin olun ve *komut dosyası sudo ayrıcalıklarıyla çalıştırmayın.*

1. Açmak **sürümleri** sekmesinde **yapı &amp; sürüm** hub'ı Aç **dağıtım grupları**seçip **+ yeni**.
2. Dağıtım grubu ve isteğe bağlı bir açıklama için bir ad girin. Ardından **oluşturma**.
3. Dağıtım hedef sanal makine için işletim sistemi seçin. Örneğin, seçin **Ubuntu 16.04 +**.
4. Seçin **kişisel erişim belirteci kimlik doğrulaması için betik kullanmak**.
5. Seçin **sistem önkoşulları** bağlantı. Bir işletim sistemi için önkoşullarını yükleyin.
6. Seçin **betik Panoya Kopyala** komut dosyasını kopyalamak için.
7. Dağıtım hedef sanal makineye oturum açın ve komut dosyasını çalıştırın. Komut dosyası sudo ayrıcalıklarıyla çalıştırmayın.
8. Yükleme sonrasında dağıtım grubu etiketlerini istenir. Varsayılanları kabul edin.
9. Yeni kaydettiğiniz sanal makinenizde Team Services içinde denetle **hedefleri** altında **dağıtım grupları**.

## <a name="create-a-team-services-release-definition"></a>Bir Team Services sürüm tanımı oluşturun

Bir yayın tanımı Team Services uygulamasını dağıtmak için kullandığı işlem belirtir. Bu örnekte, bir kabuk betiği yürütün.

Team Services içinde sürüm tanımı oluşturmak için:

1. Açık **sürümleri** sekmesinde **yapı &amp; yayın** hub ve Seç **oluşturma yayın tanımı**. 
2. Seçin **boş** başlamak seçerek şablonu bir **boş işlem**.
3. İçinde **yapıları** bölümünde, select **+ ekleyin Yapıt** ve **Jenkins** için **kaynak türünü**. Jenkins hizmet uç noktası bağlantınızı seçin. Ardından Jenkins kaynak işi seçin ve Seç **Ekle**.
4. Üç nokta seçin **ortam 1**. Seçin **Ekle dağıtım grubu aşaması**.
5. Dağıtım grubu seçin.
5. Seçin  **+**  bir görev eklemek için **dağıtım grubu aşaması**.
6. Seçin **Kabuk betiği** seçin ve görev **Ekle**. **Kabuk betiği** görevi Node.js yüklemek ve uygulamayı başlatmak için her bir sunucuda çalıştırmak bir komut dosyası için yapılandırmasını sağlar.
8. İçin **betik yolu**, girin **$(System.DefaultWorkingDirectory)/Fabrikam-Node/deployscript.sh**.
9. Seçin **Gelişmiş**ve ardından Etkinleştir **çalışma dizini belirtin**.
10. İçin **çalışma dizini**, girin **$(System.DefaultWorkingDirectory) / Fabrikam düğümlü**.
11. Belirttiğiniz ad yayın tanımına adını düzenleyin **oluşturma sonrası eylemleri** Jenkins derlemede sekmesinde. Jenkins kaynak yapıları güncelleştirildiğinde yeni sürüm tetikleme edebilmek için bu ad gerektirir.
12. Seçin **kaydetmek** seçip **Tamam** yayın tanımını kaydetmek için.

## <a name="execute-manual-and-ci-triggered-deployments"></a>El ile ve CI tetiklenen dağıtımları yürütme

1. Seçin **+ yayın** seçip **sürüm oluşturma**.
2. Vurgulanan aşağı açılan listesinde, Tamamlanan yapı seçin ve seçin **sıra**.
3. Yayın bağlantı açılır iletide seçin. Örneğin: "yayın **sürüm 1** oluşturuldu."
4. Açık **günlükleri** yayın konsol çıktısı izlemek için sekmesi.
5. Tarayıcınızda, dağıtım grubuna eklenen sunucuların birini URL'sini açın. Örneğin **http://{your-server-ip-address}**.
6. Kaynak Git deposuna gidin ve içeriğini değiştirmeye **h1** bazı değiştirilen metin dosyası app/views/index.jade başlığı.
7. Değişikliğiniz uygulayın.
8. Birkaç dakika sonra oluşturulan yeni bir sürüm görürsünüz **sürümleri** Team Services veya Team Foundation Server sayfası. Yayın gerçekleşmesini dağıtım görmek için açın. Tebrikler!

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, derleme ve sürüm için Team Services Jenkins kullanarak bir uygulamayı Azure dağıtımını otomatik. Şunları öğrendiniz:

> [!div class="checklist"]
> * Jenkins uygulamanızı oluşturun.
> * Jenkins Team Services tümleştirme için yapılandırın.
> * Azure sanal makineler için bir dağıtım grubu oluşturun.
> * Sanal makineleri yapılandırır ve uygulamayı dağıtır bir yayın tanımı oluşturun.

AMPUL (Linux, Apache, MySQL ve PHP) dağıtma hakkında bilgi edinmek için yığın, ilerletmek için sonraki öğretici.

> [!div class="nextstepaction"]
> [LAMP yığını dağıtma](tutorial-lamp-stack.md)
