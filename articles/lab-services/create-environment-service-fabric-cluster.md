---
title: Azure DevTest labs'deki bir Service Fabric kümesi ile bir ortam oluşturun. | Microsoft Docs
description: Başlangıç kendi içinde bir Service Fabric kümesi ile bir ortam oluşturun ve zamanlamaları kullanarak kümeyi Durdur öğrenin.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: EMaher
manager: femila
editor: spelluru
ms.assetid: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/01/2019
ms.author: enewman
ms.openlocfilehash: 9848f197800c391285c4065685b910685f0ac64b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60312210"
---
# <a name="create-an-environment-with-self-contained-service-fabric-cluster-in-azure-devtest-labs"></a>Azure DevTest labs'deki müstakil Service Fabric kümesi ile bir ortam oluşturun
Bu makale, kendi içinde bir Service Fabric kümesinde Azure DevTest Labs ile bir ortam oluşturma hakkında bilgi sağlar. 

## <a name="overview"></a>Genel Bakış
DevTest Labs, Azure Resource Manager şablonlarını tarafından tanımlandığı şekilde müstakil ve test ortamları oluşturabilirsiniz. Bu ortamların, sanal makineler ve Service Fabric gibi PaaS kaynakları gibi hem Iaas kaynaklarını içerir. DevTest Labs sanal makineleri denetlemek için komutları sağlayarak bir ortamdaki sanal makinelerin yönetmenize olanak sağlar. Bu komutlar başlatmak veya bir zamanlamaya göre bir sanal makineyi durdurmak için kabiliyeti sağlar. Benzer şekilde, DevTest Labs Ayrıca, bir ortamda Service Fabric kümelerini yönetmenize yardımcı olabilir. Başlatabilir veya el ile veya bir zamanlamaya aracılığıyla bir Service Fabric kümesi bir ortamda durdurun.

## <a name="create-a-service-fabric-cluster"></a>Service Fabric kümesi oluşturma
Service Fabric kümeleri, DevTest Labs'de ortamları kullanarak oluşturulur. Her ortam bir Git deposunda bir Azure Resource Manager şablonu tarafından tanımlanır. [Genel bir Git deposu](https://github.com/Azure/azure-devtestlab/tree/master/Environments/) DevTest Labs içeren bir Service Fabric kümesi oluşturmak için Resource Manager şablonu için [ServiceFabric kümesi](https://github.com/Azure/azure-devtestlab/tree/master/Environments/ServiceFabric-LabCluster) klasör. 

1. İlk olarak aşağıdaki makalede yönergeleri kullanarak Azure DevTest Labs'de Laboratuvar oluşturma: [Bir laboratuvar oluşturma](devtest-lab-create-lab.md). Dikkat **ortak ortamlardaki** seçenektir **üzerinde** varsayılan olarak. 
2. Aşağıdaki adımları izleyerek Service Fabric sağlayıcısı, aboneliğiniz için kayıtlı olduğunu doğrulayın:
    1. Seçin **abonelikleri** sol gezinti menüsünde ve seçin, **abonelik**
    2. Üzerinde **abonelik** sayfasında **kaynak sağlayıcıları** içinde **ayarları** sol menüde bölümü. 
    3. Varsa **Microsoft.ServiecFabric** seçeneğini kayıtlı değil **kaydetme**. 
3. Üzerinde **DevTest Labs** seçin, Laboratuvar için sayfa **+ Ekle** araç. 
    
    ![Araç çubuğunda Ekle düğmesi](./media/create-environment-service-fabric-cluster/add-button.png)
3. Üzerinde **temel seçin** sayfasında **Service Fabric Laboratuvar kümesi** listesinde. 

    ![Service Fabric Laboratuvar kümesi listeden seçin.](./media/create-environment-service-fabric-cluster/select-service-fabric-cluster.png)
4. Üzerinde **ayarlarını yapılandırma** sayfasında, aşağıdaki adımları uygulayın: 
    1. Belirtin bir **adı** kümenizin **ortam**. Service Fabric kümesi içinde oluşturulması geçiyor azure'da kaynak grubu adıdır. 
    2. Seçin **işletim sistemi (OS)** küme sanal makineleri için. Varsayılan değerdir: **Windows**.
    3. İçin bir ad belirtin **yönetici** küme için. 
    4. Belirtin bir **parola** yönetici için. 
    5. İçin **sertifika**, Base64 olarak kodlanmış dize olarak sertifika bilgilerinizi girin. Bir sertifika oluşturmak için aşağıdaki adımları uygulayın:
        1. İndirme **Oluştur ClusterCertificate.ps1** dosya [Git deposu](https://github.com/Azure/azure-devtestlab/tree/master/Environments/ServiceFabric-LabCluster). Alternatif olarak, depoyu makinenize kopyalayın. 
        2. **PowerShell**’i başlatın. 
        3. Çalıştırma **ps1** komutunu kullanarak dosya `.\Create-ClusterCertificate.ps1`. Not Defteri, bu sayfadaki sertifikayla ilgili alanları doldurmak için ihtiyacınız olan bilgileri ile açılmış bir metin dosyasına bakın. . 
    6. Girin **sertifika için parola**.
    7. Belirtin **parmak izi** sertifikanız için.
    8. Seçin **Ekle** üzerinde **ayarlarını yapılandır** sayfası. 

        ![Küme ayarlarını yapılandırma](./media/create-environment-service-fabric-cluster/configure-settings.png)
5. Küme oluşturulduktan sonra önceki adımda sağladığınız ortamın adı ile bir kaynak grubu görürsünüz. Genişlettiğinizde, Service Fabric kümesi görürsünüz. Kaynak grubu durumunu, takılıyorsa **oluşturma**seçin **Yenile** araç. **Service Fabric kümesi** ortamı, Linux veya Windows üzerinde 5 düğümlü 1-nodetype küme oluşturur.

    Aşağıdaki örnekte, **mysfabricclusterrg** özellikle Service Fabric kümesi için oluşturduğunuz kaynak grubunun adıdır. Laboratuvar ortamları içinde oluşturuldukları kaynak grubu içinde kendi içinde olduğuna dikkat edin önemlidir. Yalnızca yeni oluşturulan kaynak grubu içindeki kaynaklara erişebilir ortamı tanımlayan şablonu anlamına veya [Laboratuvar tarafından kullanılmak üzere yapılandırılmış sanal ağları](devtest-lab-configure-vnet.md). Yukarıdaki Bu örnek, aynı kaynak grubunda tüm gerekli kaynakları oluşturur.

    ![Oluşturulan küme](./media/create-environment-service-fabric-cluster/cluster-created.png)

## <a name="start-or-stop-the-cluster"></a>Başlatma veya durdurma küme
Başlatabilir veya sayfasından ya da geliştirme ve test laboratuvarı kendisini veya Service Fabric kümesi sayfanın DevTest Labs tarafından sağlanan kümeyi durdurun. 

### <a name="from-the-devtest-lab-page"></a>DevTest Labs sayfasından
Başlatabilir veya küme laboratuvarınız için geliştirme ve test laboratuvarı sayfasında durdurun. 

1. Seçin **üç noktaya (...)**  aşağıdaki görüntüde gösterildiği gibi bir Service Fabric kümesi için: 

    ![Başlatma ve durdurma küme için komutlar](./media/create-environment-service-fabric-cluster/start-stop-on-devtest-lab-page.png)

2. Bağlam menüsüne iki komut gördüğünüz **Başlat** ve **Durdur** kümesi. Start komutunu bir kümede tüm düğümler başlar. Stop komutu, bir kümedeki tüm düğümlerin durdurur. Bir küme durdurulduktan sonra Service Fabric kümesi hazır durumda kalır ancak Laboratuvar kümesinde start komutunu yeniden kadar hiçbir düğümler mevcuttur.

    Service Fabric kümesi bir test ortamında kullanırken dikkat edilmesi gereken birkaç nokta vardır. Bu, düğümler yeniden başlattıktan sonra tam olarak yeniden doldurma Service Fabric kümesi için biraz zaman alabilir. Başlangıç Kapatma verileri korumak için veri üzerinde yönetilen disk sanal makineye yeniden kaydedilmesi gerekir. Yalnızca test ortamları için önerilir bu nedenle, bağlı bir yönetilen disk kullanırken performans etkileri vardır. Veri depolama için kullanılan disk bağlı olmadığı, küme üzerinde Durdur komutu verildiğinde ardından veriler kaybolur.

### <a name="from-the-service-fabric-cluster-page"></a>Service Fabric kümesi sayfasından 
Başlatma veya durdurma kümedeki başka bir yolu yoktur. 

1. DevTest Labs sayfasında ağaç görünümünde Service Fabric kümenizi seçin. 

    ![Kümenizi seçin](./media/create-environment-service-fabric-cluster/select-cluster.png)

2. Üzerinde **Service Fabric kümesi** sayfasında küme için komutlar başlatmak veya küme durdurmak için araç çubuğunda görürsünüz. 

    ![Başlatma veya durdurma Service Fabric kümesi sayfasında komutları](./media/create-environment-service-fabric-cluster/start-stop-on-cluster-page.png)

## <a name="configure-auto-startup-and-auto-shutdown-schedule"></a>Otomatik başlatma ve Otomatik kapatma zamanlamasını yapılandırma
Service Fabric kümeleri ayrıca kullanmaya veya bir zamanlamaya göre durduruldu. Bu deneyim deneyimi laboratuvarda sanal makineler için benzer. Tasarruf, varsayılan olarak sağlamak için bir laboratuar ortamında otomatik olarak oluşturulan her küme Laboratuvar tarafından tanımlanan zaman kapanırken [kapatma ilke](devtest-lab-get-started-with-lab-policies.md#set-auto-shutdown). Küme olup olmadığını kapatılmalıdır belirterek veya küme kapatılmış durumda belirtilmesinden geçersiz kılabilirsiniz. 

![Otomatik başlatma ve Otomatik kapatma için mevcut zamanlamalar](./media/create-environment-service-fabric-cluster/existing-schedules.png)

### <a name="opt-in-to-the-auto-start-schedule"></a>Otomatik başlatma zamanlamaya kabul et
Başlangıç zamanlama için katılım için aşağıdaki adımları uygulayın:

1. Seçin **otomatik başlatma** sol menüde
2. Seçin **üzerinde** için **otomatik başlangıç için zamanlanacak bu service fabric kümesi izin**. Yalnızca Laboratuvar sahibi autostart kullanıcıları kendi sanal makineleri veya Service Fabric kümeleri izin verdiyse bu sayfa etkin.
3. Araç çubuğunda **Kaydet**’i seçin. 

    ![Otomatik yıldız sayfası](./media/create-environment-service-fabric-cluster/set-auto-start-settings.png)

### <a name="configure-auto-shutdown-settings"></a>Otomatik kapanma ayarlarını yapılandırma 
Kapanma ayarlarını değiştirmek için aşağıdaki adımları uygulayın:

1. Seçin **otomatik kapatma** sol menüsünde. 
2. Bu sayfada seçerek otomatik kapatma dışında seçebilirsiniz **kapalı** için **etkin**. 
3. Seçtiyseniz **üzerinde** için **etkin**, şu adımları izleyin:
    1. Belirtin **zaman** kapatma için.
    2. Belirtin **saat dilimi** kez. 
    3. DevTest Labs'ın göndermek isteyip istememenize bağlı **bildirimleri** otomatik kapatma önce. 
    4. Seçtiyseniz **Evet** bildirim seçenek için belirlediğiniz **Web kancası URL'si** ve/veya **e-posta adresi** bildirimleri göndermek için. 
    5. Araç çubuğunda **Kaydet**’i seçin.

        ![Sayfa aşağı Kapat otomatik](./media/create-environment-service-fabric-cluster/auto-shutdown-settings.png)

## <a name="to-view-nodes-in-the-service-fabric-cluster"></a>Service Fabric kümesinde düğümlerini görüntülemek için
Service Fabric kümesi sayfasında, önceki adımlarda gördüğünüz DevTest Labs sayfasına özgüdür. Bu kümedeki düğümlerin göstermez. Küme hakkında daha fazla bilgi için bu adımları izleyin:

1. Üzerinde **DevTest Labs** seçin, Laboratuvar için sayfa **kaynak grubu** ağaç görünümünde **sanal makinelerim** bölümü.

    ![Kaynak grubu seçin](./media/create-environment-service-fabric-cluster/select-resource-group.png)
2. Üzerinde **kaynak grubu** sayfasında, listedeki kaynak grubundaki tüm kaynakları görürsünüz. Seçin, **Service Fabric kümesi** listeden. 

    ![Kümenizi listeden seçin.](./media/create-environment-service-fabric-cluster/select-cluster-resource-group-page.png)
3. Gördüğünüz **Service Fabric kümesi** kümenizin sayfası. Bu, Service Fabric sağlayan sayfasıdır. Küme düğümleri, düğüm türleri, vb. gibi hakkındaki tüm bilgileri görebilirsiniz.

    ![Service Fabric kümesi ana sayfası](./media/create-environment-service-fabric-cluster/service-fabric-cluster-page.png)

## <a name="next-steps"></a>Sonraki adımlar
Ortamlar hakkında ayrıntılı bilgi için aşağıdaki makalelere bakın: 

- [Azure Resource Manager şablonları ile çoklu VM ortamları ve PaaS kaynakları oluşturma](devtest-lab-create-environment-from-arm.md)
- [Yapılandırma ve Azure DevTest Labs'de ortak ortamlardaki kullanma](devtest-lab-configure-use-public-environments.md)
