---
title: CI/CD'den Jenkins VSTS ile Azure vm'lerinin | Microsoft Docs
description: "Sürekli Tümleştirme (CI) ve Visual Studio Team Services (VSTS) ya da Microsoft Team Foundation Server (TFS) yayın Yönetimi'nden Azure Vm'lerine Jenkins kullanarak bir Node.js uygulaması sürekli dağıtımını (CD) ayarlama"
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
ms.openlocfilehash: d5c15d6ab36a8454d1c69bac498be89b990c7e33
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="deploy-your-app-to-linux-vms-using-jenkins-and-vsts"></a>Jenkins ve VSTS kullanarak Linux VM'ler için uygulamanızı dağıtma

Sürekli Tümleştirme (CI) ve sürekli dağıtımı (CD) olarak derleme, sürüm ve kullanabilirsiniz, kodunuzu dağıtmak bir ardışık düzen olur. Visual Studio Team Services (VSTS) CI/CD Otomasyon Araçları'nın tam, tam özellikli bir dizi Azure'a dağıtılmasını sağlar. Jenkins CI/CD Otomasyon de sağlayan bir Popüler 3. taraf CI/CD sunucu tabanlı araçtır. Her ikisi de birlikte bulut uygulaması veya hizmeti teslim nasıl özelleştirmek için kullanabilirsiniz.

Bu öğreticide, Jenkins oluşturmak için kullandığınız bir **Node.js web uygulamasına**, VSTS veya Team Foundation Server (TFS) ve ona dağıtmak için bir [dağıtım grubu](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) Linux sanal makineleri içeren.

Yapacaklarınız:

> [!div class="checklist"]
> * Örnek uygulamayı Al
> * Jenkins eklentileri yapılandırın
> * Jenkins Freestyle projeyi Node.js için yapılandırın
> * Jenkins için VSTS tümleştirmesini yapılandırma
> * Jenkins hizmet uç noktası oluşturma
> * Azure sanal makineler için bir dağıtım grubu oluşturun
> * VSTS yayın tanımı oluşturun
> * El ile ve tetiklenen CI dağıtımları yürütün

## <a name="before-you-begin"></a>Başlamadan önce

* Jenkins sunucuya erişmeniz gerekir. Jenkins server henüz oluşturmadıysanız, bkz: [Jenkins asıl bir Azure sanal makinesinde oluşturmak](https://docs.microsoft.com/en-us/azure/jenkins/install-jenkins-solution-template). 

* VSTS hesabınızda oturum açın (`https://{youraccount}.visualstudio.com`). 
  Alabileceğiniz bir [ücretsiz VSTS hesap](https://go.microsoft.com/fwlink/?LinkId=307137&clcid=0x409&wt.mc_id=o~msft~vscom~home-vsts-hero~27308&campaign=o~msft~vscom~home-vsts-hero~27308).

  > [!NOTE]
  > Daha fazla bilgi için bkz: [VSTS Bağlan](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services).

*  Linux sanal makine için bir dağıtım hedef gerekir.  Daha fazla bilgi için bkz: [oluşturma ve yönetme Linux VM'ler Azure CLI ile](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/tutorial-manage-vm).

*  Gelen bağlantı noktası 80, sanal makine için açın.  Daha fazla bilgi için bkz: [Azure Portalı'nı kullanarak ağ güvenlik grupları oluşturma](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-create-nsg-arm-pportal).

## <a name="get-the-sample-app"></a>Örnek uygulamayı Al

Bir uygulamayı dağıtmak için bir Git deposu içinde depolanan gerekir.
Bu öğretici için kullandığınız olan öneririz [Github'dan Bu örnek uygulama](https://github.com/azooinmyluggage/fabrikam-node).  Bu öğretici, Node.js ve bir uygulama yüklemek için kullanılan bir örnek komut dosyası içerir.  Kendi deposuyla çalışmak isterseniz, benzer bir örnek yapılandırmanız gerekir.

1. Bu uygulamanın çatalı oluşturun ve bu öğreticinin daha sonraki adımlarda kullanılmak konumu (URL) not edin.  Daha fazla bilgi için bkz: [bir depoyu Çatallaştırma](https://help.github.com/articles/fork-a-repo/)    

> [!NOTE]
> Uygulama kullanılarak oluşturulan [Yeoman](http://yeoman.io/learning/index.html); kullandığı **Express**, **bower**, ve **grunt**; ve bazı sahip **npm** paketler bağımlılık olarak.
> Örnek, Nginx ayarlar ve uygulamayı dağıtır bir betik de içerir. Sanal makinelerde yürütülür. Özellikle, betik düğümünde, Nginx ve PM2 yükler; Nginx ve PM2 yapılandırır; ardından düğümü uygulaması başlatır.

## <a name="configure-jenkins-plugins"></a>Jenkins eklentileri yapılandırın

İlk olarak, iki Jenkins eklentileri için yapılandırmanız gerekir **NodeJS** ve **VS Team Services sürekli dağıtımı**.

1. Jenkins hesabınızı açın ve seçin **yönetmek Jenkins**.
2. İçinde **yönetmek Jenkins** sayfasında, **eklentileri yönetme**.
3. Listenin bulmak için filtre **NodeJS** eklentisi ve **yeniden yükleme** seçeneği.
    ![Jenkins için NodeJS eklenti ekleme](media/tutorial-build-deploy-jenkins/jenkins-nodejs-plugin.png)
4. Listenin bulmak için filtre **VS Team Services sürekli dağıtımı** eklentisi ve **yeniden yükleme** seçeneği.
5. Jenkins panosuna geri gidin ve seçin **yönetmek Jenkins**.
6. Seçin **genel aracı yapılandırma**.  Bul **NodeJS** tıklatıp **NodeJS yüklemeleri**.
7. Etkinleştirme **otomatik olarak yüklemek** seçeneğini ve ardından girin bir **adı** değeri.
8. **Kaydet** düğmesine tıklayın.

## <a name="configure-a-jenkins-freestyle-project-for-nodejs"></a>Jenkins Freestyle projeyi Node.js için yapılandırın

1. Tıklatın **yeni öğe**.  Girin bir **öğe adı**.
2. Seçin **Serbest stilde proje**.  **Tamam** düğmesine tıklayın.
3. İçinde **kaynak kodu Yönetimi** sekmesine **Git** ve depo ve uygulama kodu içeren dalı ayrıntılarını girin.    
    ![Bir depo, derleme ekleyin](media/tutorial-build-deploy-jenkins/jenkins-git.png)
4. İçinde **yapı tetikleyicileri** sekmesine **yoklama SCM** ve zamanlama girin `H/03 * * * *` değişiklikleri için Git deposu üç dakikada yoklamak için. 
5. İçinde **yapı ortamı** sekmesine **sağlayan düğüm &amp; npm bin / klasör yolu** seçip **NodeJS yükleme** değeri. Bırakın **npmrc dosya** "sistem varsayılan kullanır." olarak ayarlayın
6. İçinde **yapı** sekmesinde, seçin **Kabuk yürütme** ve komutu girin `npm install` tüm bağımlılıkları güncelleştirilir emin olmak için.


## <a name="configure-jenkins-for-vsts-integration"></a>Jenkins için VSTS tümleştirmesini yapılandırma

  > [!NOTE]
  İçin aşağıdaki adımları kullanın ve kişisel erişim belirteci (PAT) içerdiğinden olun **VSTS (okuma, yazma, yürütme ve yönetme) sürüm izin**.
 
1.  Zaten yoksa, bir PAT VSTS hesabınızı oluşturun. Jenkins VSTS hesabınıza erişmek için bu bilgileri gerektirir.  Sağlamanız **depolamak** yaklaşan bölümünde açıklanan adımları izleyerek bu belirteci bilgileri.
  Okuma [nasıl oluşturabilirim kişisel erişim belirteci VSTS ve TFS için](https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) biri oluşturma hakkında bilgi edinmek için.
2. İçinde **oluşturma sonrası eylemleri** sekmesini tıklatın, **oluşturma sonrası eylem eklemek**. Seçin **yapıları arşiv**.
3. İçin **arşivlemek için dosyaları**, girin `**/*` tüm dosyaları eklemek için.
4. Başka bir eylem oluşturmak için tıklatın **oluşturma sonrası eylem eklemek**.
5. Seçin **tetiklemek TFS/Team Services sürümde**, URI VSTS hesabınızın gibi girin: `https://{your-account-name}.visualstudio.com`.
6. Girin **takım projesi** adı.
7. İçin bir ad seçin **yayın tanımı** (Bu yayın tanımını daha sonra VSTS içinde oluşturduğunuz).
8. VSTS veya TFS ortamınıza bağlanmak için kimlik bilgilerini seçin.  Bırakın **kullanıcıadı** VSTS kullanıyorsanız, boş.
   Girin bir **kullanıcı adı ve parola** TFS şirket içi sürümünü kullanıyorsa.    
    ![Jenkins oluşturma sonrası eylemleri yapılandırma](media/tutorial-build-deploy-jenkins/trigger-release-from-jenkins.png)
5. **Kaydet** jenkins projesi.

## <a name="create-a-jenkins-service-endpoint"></a>Jenkins hizmet uç noktası oluşturma

Hizmet uç noktası için Jenkins bağlanmak VSTS sağlar.

1. Açık **Hizmetleri** VSTS, açık sayfasında **yeni hizmet uç noktası** listesinde ve seçin **Jenkins**.
    ![Jenkins uç nokta ekleyin](media/tutorial-build-deploy-jenkins/add-jenkins-endpoint.png)
2. Bağlantı için bir ad girin.
3. Jenkins sunucunuza ve onay URL'sini girin **kabul güvenilmeyen SSL sertifikalarını** seçeneği.  URL bir örnek verilmiştir:`http://{YourJenkinsURL}.westcentralus.cloudapp.azure.com`
4. Girin **kullanıcı adı ve parola** Jenkins hesabınız için.
5. Seçin **bağlantısını doğrulama** bilgilerin doğru olup olmadığını denetlemek için.
6. Seçin **Tamam** hizmet uç noktası oluşturulamadı.

## <a name="create-a-deployment-group-for-azure-virtual-machines"></a>Azure sanal makineler için bir dağıtım grubu oluşturun

Gereksinim duyduğunuz bir [dağıtım grubu](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) yayın tanımı sanal makinenize dağıtabilmek için VSTS Aracısı'nı kaydetmek için.  Dağıtım grupları dağıtımı için hedef makinelerin mantıksal gruplarını tanımlamak kolaylaştırır ve gerekli aracı her makineye yükleyin.

   > [!NOTE]
   > Aşağıdaki adımlarda Önkoşulları Yükleme emin olun ve **sudo ayrıcalıklarına sahip bir komut dosyası yürütme yok.**

1. Açık **sürümleri** sekmesinde **yapı &amp; sürüm** hub'ı açın **dağıtım grupları**ve seçin **+ yeni**.
2. Dağıtım grubu ve isteğe bağlı bir açıklama için bir ad girin.
   Ardından **oluşturma**.
3. Dağıtım hedef sanal makine için işletim sistemi seçin.  Örneğin, tercih **Ubuntu 16.04 +**.
4. Değer çizgilerinin **kişisel erişim belirteci kimlik doğrulaması için betik kullanmak**.
5. Seçin **sistem önkoşulları** bağlantı.  Bir işletim sistemi için önkoşullarını yükleyin.
6. Seçin **betik Panoya Kopyala** komut dosyasını kopyalamak için.
7. **Oturum açma** dağıtım hedef sanal makinenize ve **yürütme** komut dosyası.  **Verme** komut dosyası sudo ayrıcalıklarıyla çalıştırın.
8. Yükleme sonrasında dağıtım grubu etiketlerini istenir.  Varsayılanları kabul edin.
9. Yeni kaydettiğiniz sanal makinenizde VSTS içinde denetle **hedefleri** altında **dağıtım grupları**.

## <a name="create-a-vsts-release-definition"></a>VSTS yayın tanımı oluşturun

Bir yayın tanımı işlemi belirtir VSTS yürütür uygulama dağıtmak için.  Bu örnekte, bir kabuk betiği yürütün.

VSTS içinde sürüm tanımı oluşturmak için:

1. Açık **sürümleri** üzerinde **yapı &amp; yayın** hub'ı seçin **oluşturma yayın tanımı**. 
2. Seçin **boş** Başlarken seçerek şablonu bir **boş işlem**.
3. İçinde **yapıları** bölümünde, tıklayın **+ ekleyin Yapıt** ve **Jenkins** için **kaynağı türü**. Jenkins hizmet uç noktası bağlantınızı seçin. Ardından Jenkins kaynak iş seçip **Ekle**.
4.  Yanındaki üç nokta düğmesine **ortam 1**.  Tıklatın **Ekle dağıtım grubu aşaması**.
5.  Seçin, **dağıtım grubu**.
5. Tıklatın  **+**  bir görev eklemek için **dağıtım grubu aşaması**.
6. Seçin **Kabuk betiği** 'ı tıklatın ve görev **Ekle**.    
    **Kabuk betiği** görev Node.js yüklemek ve uygulamayı başlatmak için her sunucuda çalıştırmak bir komut dosyası için yapılandırma sağlamak için kullanılır. Aşağıdaki gibi yapılandırın:
8. **Komut dosyası yolu**:     
    `$(System.DefaultWorkingDirectory)/Fabrikam-Node/deployscript.sh`
9.  Tıklatın **Gelişmiş**ve ardından Etkinleştir **çalışma dizini belirtin**.
10.  **Çalışma dizini**:`$(System.DefaultWorkingDirectory)/Fabrikam-Node`
11. İçinde belirtilen adı yayın tanımına adını düzenleyin **oluşturma sonrası eylemleri** Jenkins derlemede sekmesinde. Jenkins kaynak yapıları güncelleştirildiğinde yeni sürüm tetikleme edebilmek için bu ad gerektirir.
12. Tıklatın **kaydetmek**, tıklatıp **Tamam** yayın tanımını kaydetmek için.

## <a name="execute-manual-and-ci-triggered-deployments"></a>El ile ve tetiklenen CI dağıtımları yürütün

1. Seçin **+ yayın** seçip **sürüm oluşturma**.
2. Tamamlanan yapı vurgulanan aşağı açılan listeden seçip **sıra**.
3. Yayın bağı açılan iletide'i seçin. Örneğin: "yayın **sürüm 1** oluşturuldu."
4. Açık **günlükleri** yayın konsol çıktısı izlemek için sekmesi.
5. Tarayıcınızda, dağıtım grubuna eklenen sunucuların birini URL'sini açın. Örneğin, girin`http://{your-server-ip-address}`
6. Kaynak Git deposuna gidin ve içeriğini değiştirmeye **h1** bazı değiştirilen metin dosyası app/views/index.jade başlığı.
7. **Commit** değişikliğinizin.
8. Birkaç dakika sonra oluşturulan yeni bir sürüm görürsünüz **sürümleri** VSTS veya TFS sayfası. Yayın gerçekleşmesini dağıtım görmek için açın. Tebrikler!

## <a name="next-steps"></a>Sonraki Adımlar

Bu öğreticide, Azure Jenkins yapı ve VSTS sürüm için kullanarak bir uygulama dağıtımını otomatik. Şunları öğrendiniz:

> [!div class="checklist"]
> * Jenkins uygulamanızda derleme
> * Jenkins için VSTS tümleştirmesini yapılandırma
> * Azure sanal makineler için bir dağıtım grubu oluşturun
> * Sanal makineleri yapılandırır ve uygulamayı dağıtır bir yayın tanımı oluşturun

Bir AMPUL dağıtma hakkında daha fazla bilgi için sonraki öğretici İlerlet (Linux, Apache, MySQL ve PHP) yığını.

> [!div class="nextstepaction"]
> [LAMP yığını dağıtma](tutorial-lamp-stack.md)