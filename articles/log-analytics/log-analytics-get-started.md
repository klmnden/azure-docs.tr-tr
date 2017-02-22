---
title: "Azure Log Analytics çalışma alanını kullanmaya başlama | Microsoft Docs"
description: "Log Analytics içindeki bir çalışma alanını birkaç dakika içinde kullanmaya başlayabilirsiniz."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 508716de-72d3-4c06-9218-1ede631f23a6
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/08/2017
ms.author: banders
translationtype: Human Translation
ms.sourcegitcommit: f75386f970aeb5694d226cfcd569b8c04a253191
ms.openlocfilehash: 0f418af5728b6a156ebc72fb99a3d16d559654ed


---
# <a name="get-started-with-a-log-analytics-workspace"></a>Log Analytics çalışma alanını kullanmaya başlama
IT altyapınızdan toplanan çalışma bilgilerini değerlendirmenize yardımcı olan Azure Log Analytics’i birkaç dakika içinde kullanmaya başlayabilirsiniz. Bu makaleyi kullanarak, *ücretsiz olarak* topladığınız verileri keşfetmeye, analiz etmeye ve üzerinde işlem yapmaya kolayca başlayabilirsiniz.

Bu makale, hizmeti kullanmaya başlayabilmeniz için Azure’da küçük bir dağıtım yapmayı gösteren kısa bir öğretici kullanarak Log Analytics’e giriş bilgileri sağlar. Azure’daki yönetim verilerinizin depolandığı mantıksal kapsayıcı, çalışma alanı olarak adlandırılır. Bu bilgileri gözden geçirip kendi değerlendirmenizi tamamladıktan sonra, değerlendirme çalışma alanını kaldırabilirsiniz. Bu makale bir öğretici olduğu için, iş gereksinimlerini, planlamayı veya mimari yönergelerini ele almaz.

>[!NOTE]
>Microsoft Azure Kamu Bulutu kullanıyorsanız, bunun yerine [Azure Kamu İzleme + Yönetim belgelerine](https://review.docs.microsoft.com/azure/azure-government/documentation-government-services-monitoringandmanagement#log-analytics) bakın.

Başlamak için kullanılan işleme hızlı bir bakış aşağıda verilmiştir:

![işlem diyagramı](./media/log-analytics-get-started/onboard-oms.png)

## <a name="1-create-an-azure-account-and-sign-in"></a>1 Bir Azure hesabı oluşturup oturum açın

Henüz bir Azure hesabınız yoksa, Log Analytics kullanmak için bir tane oluşturmanız gerekir. Herhangi bir Azure hizmetine 30 gün boyunca erişmenizi sağlayan [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturabilirsiniz.

### <a name="to-create-a-free-account-and-sign-in"></a>Ücretsiz hesap oluşturup oturum açmak için
1. [Ücretsiz Azure hesabınızı oluşturun](https://azure.microsoft.com/free/) bölümündeki yönergeleri izleyin.
2. [Azure portalına](https://portal.azure.com) gidin ve oturum açın.

## <a name="2-create-a-workspace"></a>2 Çalışma alanı oluşturun

Sonraki adım bir çalışma alanı oluşturmayı içerir.

1. Azure portalında, Market hizmet listesinde *Log Analytics* araması yapın ve ardından **Log Analytics**’i seçin.  
    ![Azure portal](./media/log-analytics-get-started/log-analytics-portal.png)
2. **Oluştur**'a tıklayın, ardından şu öğeler için seçim yapın:
   * **OMS Çalışma Alanı** - Çalışma alanınız için bir ad yazın.
   * **Abonelik** - Birden çok aboneliğiniz varsa yeni çalışma alanıyla ilişkilendirmek istediğiniz aboneliği seçin.
   * **Kaynak grubu**
   * **Konum**
   * **Fiyatlandırma katmanı**  
       ![hızlı oluşturma](./media/log-analytics-get-started/oms-onboard-quick-create.png)
3. Çalışma alanlarınızın listesini görmek için **Tamam**'a tıklayın.
4. Azure portalında ayrıntılarını görmek için bir çalışma alanı seçin.       
    ![çalışma alanı ayrıntıları](./media/log-analytics-get-started/oms-onboard-workspace-details.png)         

## <a name="3-add-solutions-and-solution-offerings"></a>3 Çözüm ve çözüm teklifleri ekleme

Ardından, yönetim çözümleri ve çözüm teklifleri ekleyin. Yönetim çözümleri belirli bir sorun alanına odaklanan ölçümler sağlayan mantık, görselleştirme ve veri alımı kural koleksiyonudur. Çözüm teklifi ise bir yönetim çözümleri paketidir.

Çalışma alanınıza çözüm eklenmesi, Log Analytics’in aracıları kullanarak çalışma alanınıza bağlı bilgisayarlardan çeşitli türlerde verileri toplamasına olanak tanır. Aracı ekleme işlemi daha sonra ele alınacaktır.

### <a name="to-add-solutions-and-solution-offerings"></a>Çözüm ve çözüm teklifleri eklemek için

1. Azure portalında **Yeni**’ye tıklayın ve ardından **Market içinde ara** kutusuna **Activity Log Analytics** yazıp ENTER tuşuna basın.
2. “Her Şey” dikey penceresinde **Activity Log Analytics**’i seçip **Oluştur**’a tıklayın.  
    ![Activity Log Analytics](./media/log-analytics-get-started/activity-log-analytics.png)  
3. *Yönetim çözümü adı* dikey penceresinde, yönetim çözümüyle ilişkilendirmek istediğiniz bir çalışma alanı seçin.
4. **Oluştur**’a tıklayın.  
    ![çözüm çalışma alanı](./media/log-analytics-get-started/solution-workspace.png)  
5. Eklemek için 1-4 arası adımları yineleyin:
    - Kötü Amaçlı Yazılımdan Koruma Değerlendirmesi ile Güvenlik ve Denetim çözümlerini içeren **Güvenlik ve Uyumluluk** hizmet teklifi.
    - Otomasyon Karma Çalışanı, Değişiklik İzleme ve Sistem Güncelleştirme Değerlendirmesi (aynı zamanda Update Management olarak adlandırılır) çözümlerini içeren **Otomasyon ve Denetim** hizmet teklifi. Çözüm teklifini eklediğinizde bir Otomasyon hesabı oluşturmanız gerekir.  
        ![Otomasyon hesabı](./media/log-analytics-get-started/automation-account.png)  
6. **Log Analytics** > **Abonelikler** > ***çalışma alanı adı*** > **Genel Bakış** seçeneğine giderek, çalışma alanınıza eklediğiniz yönetim çözümlerini görüntüleyebilirsiniz. Eklediğiniz yönetim çözümlerinin kutucukları gösterilir.  
    >[!NOTE]
    >Çalışma alanına herhangi bir aracı bağlanmadığı için, eklediğiniz çözümlere ilişkin herhangi bir veri görmezsiniz.  

    ![veri olmadan çözüm kutucukları](./media/log-analytics-get-started/solutions-no-data.png)

## <a name="4-create-a-vm-and-onboard-an-agent"></a>4 VM oluşturma ve aracı ekleme

Ardından, Azure’da basit bir sanal makine oluşturun. Bir VM oluşturduktan sonra etkinleştirmek için OMS aracısını ekleyin. Aracının etkinleştirilmesi VM’den veri toplamayı başlatır ve Log Analytics’e veri gönderir.

### <a name="to-create-a-virtual-machine"></a>Sanal makine oluşturmak için

- [Azure portalında ilk Windows sanal makinenizi oluşturma](../virtual-machines/virtual-machines-windows-hero-tutorial.md) bölümündeki yönergeleri izleyin ve yeni sanal makineyi başlatın.

### <a name="connect-the-virtual-machine-to-log-analytics"></a>Sanal makineyi Log Analytics’e bağlama

- Azure portalını kullanarak VM’yi Log Analytics’e bağlamak için [Azure sanal makinelerini Log Analytics’e bağlama](log-analytics-azure-vm-extension.md) bölümündeki yönergeleri izleyin.

## <a name="5-view-and-act-on-data"></a>5 Verileri görüntüleme ve üzerinde işlem yapma

Daha önce Activity Log Analytics çözümünü ve Güvenlik ve Uyumluluk ile Otomasyon ve Denetim tekliflerini etkinleştirdiniz. Bundan sonra, çözümler tarafından toplanan verilere ve günlük aramalarındaki sonuçlara bakmaya başlayacağız.

Başlamak için çözümlerin içinde gösterilen verilere bakın. Ardından, günlük aramalarından erişilen bazı günlük aramalarına bakın. Günlük aramaları, ortamınızdaki birden fazla kaynaktan makine verilerini birleştirmenize ve ilişkilendirmenize olanak tanır. Daha fazla bilgi için bkz. [Log Analytics’te günlük aramaları](log-analytics-log-searches.md). Son olarak, Azure portalının dışında yer alan OMS portalını kullanarak bulduğumuz veriler üzerinde işlem yapın.

### <a name="to-view-antimalware-data"></a>Kötü Amaçlı Yazılımdan Koruma verilerini görüntülemek için

1. Azure portalında **Log Analytics** > ***çalışma alanınız*** seçeneğine gidin.
2. Çalışma alanınızın dikey penceresindeki **Genel** altında **Genel bakış**’a tıklayın.  
    ![Genel Bakış](./media/log-analytics-get-started/overview.png)
3. **Kötü Amaçlı Yazılımdan Koruma Değerlendirmesi** kutucuğuna tıklayın. Bu örnekte bir bilgisayara Windows Defender’ın yüklü olduğunu, ancak imzasının eski olduğunu görebilirsiniz.  
    ![Kötü Amaçlı Yazılımdan Koruma](./media/log-analytics-get-started/solution-antimalware.png)
4. Bu örnek için **Koruma durumu** altında **İmza eski**’ye tıklayarak Günlük Araması’nı açın ve imzaları eski olan bilgisayara ilişkin ayrıntıları görüntüleyin. Bu örnekte bilgisayar adı *getstarted* şeklindedir. İmzası eski olan birden fazla bilgisayar varsa, bunların tümü Günlük Araması sonuçlarında görünür.  
    ![Kötü amaçlı yazılımdan koruma eski](./media/log-analytics-get-started/antimalware-search.png)

### <a name="to-view-security-and-audit-data"></a>Güvenlik ve Denetim verilerini görüntülemek için

1. Çalışma alanınızın dikey penceresindeki **Genel** altında **Genel bakış**’a tıklayın.  
2. **Güvenlik ve Denetim** kutucuğuna tıklayın. Bu örnekte, öne çıkan iki sorun olduğunu görebilirsiniz: kritik güncelleştirmeleri eksik olan bir bilgisayar ve koruması yetersiz olan bir bilgisayar vardır.  
    ![Güvenlik ve Denetim](./media/log-analytics-get-started/security-audit.png)
3. Bu örnek için **Önemli Sorunlar** altında **Kiritik güncelleştirmeleri eksik olan bilgisayarlar**’a tıklayarak Günlük Araması’nı açın ve kritik güncelleştirmeleri eksik olan bilgisayarlara ilişkin ayrıntıları görüntüleyin. Bu örnekte, bir kritik güncelleştirme ve 63 diğer güncelleştirme eksiktir.  
    ![Güvenlik ve Denetim Günlük Araması](./media/log-analytics-get-started/security-audit-log-search.png)

### <a name="to-view-and-act-on-system-update-data"></a>Sistem Güncelleştirme verilerini görüntülemek ve üzerinde işlem yapmak için

1. Çalışma alanınızın dikey penceresindeki **Genel** altında **Genel bakış**’a tıklayın.  
2. **Sistem Güncelleştirme Değerlendirmesi** kutucuğuna tıklayın. Bu örnekte, kritik güncelleştirme gerektiren *getstarted* adlı bir Windows bilgisayarın ve tanım güncelleştirmeleri gerektiren bir bilgisayarın olduğunu görebilirsiniz.  
    ![Sistem Güncelleştirmeleri](./media/log-analytics-get-started/system-updates.png)
3. Bu örnek için **Eksik Güncelleştirmeler** altında **Kiritik Güncelleştirmeler**’e tıklayarak Günlük Araması’nı açın ve kritik güncelleştirmeleri eksik olan bilgisayarlara ilişkin ayrıntıları görüntüleyin. Bu örnekte, bir eksik güncelleştirme ve bir gerekli güncelleştirme vardır.  
    ![Sistem Güncelleştirmeleri Günlük Araması](./media/log-analytics-get-started/system-updates-log-search.png)
4. [Operations Management Suite](http://microsoft.com/oms) web sitesine gidin ve Azure hesabınızla oturum açın. Oturum açtığınızda, çözüm bilgilerinin Azure portalında gördüğünüz bilgilere benzer olduğuna dikkat edin.  
    ![OMS portalı](./media/log-analytics-get-started/oms-portal.png)
5. **Update Management** kutucuğuna tıklayın.
6. Update Management panosunda sistem güncelleştirme bilgilerinin, Azure portalında gördüğünüz Sistem Güncelleştirme bilgilerine benzer olduğuna dikkat edin. Ancak, **Güncelleştirme Dağıtımlarını Yönet** kutusu yenidir. **Güncelleştirme Dağıtımlarını Yönet** kutusuna tıklayın.  
    ![Güncelleştirme Yönetimi kutucuğu](./media/log-analytics-get-started/update-management.png)
7. **Güncelleştirme Dağıtımları** sayfasında **Ekle**’ye tıklayarak bir *güncelleştirme çalışması* oluşturun.  
    ![Güncelleştirme Dağıtımları](./media/log-analytics-get-started/update-management-update-deployments.png)
8.  **Yeni Güncelleştirme Dağıtımı** sayfasında güncelleştirme dağıtımı için bir ad yazın, güncelleştirilecek bilgisayarları seçin (bu örnekte, *getstarted*), bir zamanlama seçin ve ardından **Kaydet**’e tıklayın.  
    ![Yeni Dağıtım](./media/log-analytics-get-started/new-deployment.png)  
    Güncelleştirme dağıtımını kaydettikten sonra zamanlanan güncelleştirmeyi görürsünüz.  
    ![zamanlanmış güncelleştirme](./media/log-analytics-get-started/scheduled-update.png)  
    Güncelleştirme çalışması tamamlandıktan sonra durum **Tamamlandı** olarak değişir.
    ![biten güncelleştirme](./media/log-analytics-get-started/completed-update.png)
9. Güncelleştirme çalışması bittikten sonra çalışmanın başarılı olup olmadığını ve hangi güncelleştirmelerin uygulandığını görüntüleyebilirsiniz.

## <a name="after-evaluation"></a>Değerlendirme sonrasında

Bu öğreticide bir sanal makineye aracı yükleyip hızlıca çalışmaya başladınız. İzlediğiniz adımlar hızlı ve kolaydı. Ancak, çoğu büyük kuruluş ve işletme, karmaşık şirket içi BT altyapılarına sahiptir. Bu nedenle, bu karmaşık ortamlardan veri toplanması, öğreticide gösterilenden daha fazla planlama ve çaba gerektirir. Yararlı makalelerin bağlantıları için aşağıdaki Sonraki adımlar bölümünde verilen bilgileri gözden geçirin.

İsteğe bağlı olarak, bu öğreticiyle oluşturduğunuz çalışma alanını kaldırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* Log Analytics’e [Windows aracıları](log-analytics-windows-agents.md) bağlama hakkında bilgi edinin.
* Log Analytics’e [Operations Manager aracıları](log-analytics-om-agents.md) bağlama hakkında bilgi edinin.
* İşlev eklemek ve veri toplamak için bkz. [Çözüm Galerisinden Log Analytics çözümleri ekleme](log-analytics-add-solutions.md).
* Çözümler tarafından toplanan ayrıntılı bilgileri görüntülemek için [günlük aramaları](log-analytics-log-searches.md) hakkında bilgi edinin.



<!--HONumber=Feb17_HO2-->


