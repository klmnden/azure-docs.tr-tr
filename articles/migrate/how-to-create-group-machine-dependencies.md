---
title: Makineleri Azure geçişi ile makine bağımlılıkları kullanan Grup | Microsoft Docs
description: Makine bağımlılıkları kullanan Azure geçişi hizmeti ile bir değerlendirme oluşturmayı açıklar.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 07/05/2018
ms.author: raynew
ms.openlocfilehash: 4b83380558c10bc4f96d56f89a5cc2b7b53edc2e
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39621088"
---
# <a name="group-machines-using-machine-dependency-mapping"></a>Makine bağımlılık eşlemesi kullanan Grup makineleri

Bu makalede bu makine için bir grup oluşturmak nasıl [Azure geçişi](migrate-overview.md) makinelerin bağımlılıklarını görselleştirme tarafından değerlendirme. Makine bağımlılıklarını arası denetimi, bir değerlendirme çalıştırmadan önce göre daha yüksek güven düzeylerine sahip VM grupları değerlendirmek istediğiniz zaman genellikle bu yöntemi kullanın. Bağımlılık görselleştirmesi etkili bir şekilde azure'a geçişinizi planlamanıza yardımcı olabilir. Hiçbir şey geride bıraktığı ve Azure'a geçirirken şaşkınlık kesintiler meydana gelmediğinden emin olmanıza yardımcı olur. Geçişini birlikte yaptığınız ve çalışan bir sistemi hala kullanıcıya hizmet veren veya geçiş yerine yetkisinin alınması için bir adaydır olup olmadığını belirlemek için gereken tüm bağımlı sistemlere bulabilir.


## <a name="prepare-machines-for-dependency-mapping"></a>Makine bağımlılık eşlemesi için hazırlanma
Makinelerin bağımlılıklarını görüntülemek için indirip değerlendirmek istediğiniz her bir şirket içi makineye aracılar yüklemeniz gerekir. İnternet bağlantısı olmayan makineleriniz varsa, ayrıca, indirmek ve yüklemek ihtiyacınız [OMS ağ geçidi](../log-analytics/log-analytics-oms-gateway.md) bunlar üzerinde.

### <a name="download-and-install-the-vm-agents"></a>Sanal makine aracılarını indirip yükleme
1. İçinde **genel bakış**, tıklayın **Yönet** > **makineler**, gerekli makineyi seçin.
2. İçinde **bağımlılıkları** sütun tıklayın **aracıları yüklemek**.
3. Üzerinde **bağımlılıkları** sayfasında indirin ve değerlendirmek istediğiniz her sanal makinede Microsoft Monitoring Agent (MMA) ve bağımlılık aracısını yükleyin.
4. Çalışma alanı kimliğini ve anahtarını kopyalayın. Şirket içi makinede MMA'yı yüklediğinizde bunlar gerekir.

> [!NOTE]
> Aracıların yüklenmesini otomatik hale getirmek için herhangi bir dağıtım aracı System Center Configuration Manager gibi kullanın veya iş ortağı aracımızı kullanın [Intigua](https://www.intigua.com/getting-started-intigua-for-azure-migration), Azure geçişi için bir aracı dağıtım çözümü vardır.

### <a name="install-the-mma"></a>MMA’yı yükleme

Bir Windows makinede aracı yüklemek için:

1. İndirilen aracıya çift tıklayın.
2. **Hoş Geldiniz** sayfasında **İleri**'ye tıklayın. **Lisans Koşulları** sayfasında **Kabul Ediyorum**’a tıklayarak lisansı kabul edin.
3. İçinde **hedef klasör**, saklamak veya varsayılan yükleme klasörünü değiştirin > **sonraki**.
4. İçinde **Aracı Kurulum Seçenekleri**seçin **Azure Log Analytics** > **sonraki**.
5. Tıklayın **Ekle** yeni bir Log Analytics çalışma alanı eklemek için. Çalışma alanı kimliği ve portaldan kopyaladığınız anahtarını yapıştırın. **İleri**’ye tıklayın.


Bir Linux makinesinde aracıyı yüklemek için:

1. Uygun olan paketi (x86 veya x64), scp/sftp kullanarak Linux bilgisayarınıza aktarın.
2. Paket kullanarak yükleme yükleme bağımsız değişken.

    ```sudo sh ./omsagent-<version>.universal.x64.sh --install -w <workspace id> -s <workspace key>```


### <a name="install-the-dependency-agent"></a>Bağımlılık aracısını yükleme
1. Bir Windows makinede bağımlılık Aracısı'nı yüklemek için kurulum dosyasına çift tıklayın ve sihirbazı izleyin.
2. Bağımlılık Aracısı'nı bir Linux makineye yüklemek için aşağıdaki komutu kullanarak kök olarak yükleyin:

    ```sh InstallDependencyAgent-Linux64.bin```

Bağımlılık Aracısı desteği hakkında daha fazla bilgi [Windows](../monitoring/monitoring-service-map-configure.md#supported-windows-operating-systems) ve [Linux](../monitoring/monitoring-service-map-configure.md#supported-linux-operating-systems) işletim sistemleri.

[Daha fazla bilgi edinin](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#installation-script-examples) bağımlılık Aracısı'nı yüklemek için komut dosyalarını nasıl kullanabileceğiniz hakkında.

## <a name="create-a-group"></a>Grup oluşturma

1. Aracıları yükledikten sonra portal ve tıklayın Git **Yönet** > **makineler**.
2. Aracıların yüklü olduğu makinenin arayın.
3. **Bağımlılıkları** makine için sütun olarak artık göstermelidir **bağımlılıklarını görüntüleme**. Sütun makinenin bağımlılıklarını görüntülemek için tıklayın.
4. Makine bağımlılık eşlemesi aşağıdaki ayrıntıları gösterir:
    - (İstemciler) gelen ve giden (sunucu) TCP bağlantıları/makineden
        - MMA ve bağımlılık aracısı yüklü olmayan bağımlı makineler bağlantı noktası numaralarını tarafından gruplandırılır.
        - MMA ve bağımlılık aracısının yüklü olduğu bağımlı makineler, ayrı kutular olarak gösterilir
    - İşlemler, makinenin içinde çalışan işlemleri görüntülemek için her makine kutusunu genişletebilirsiniz.
    - Tam etki alanı adı, işletim sistemi, her makinenin MAC adresi vb. gibi özellikler, bu ayrıntıları görüntülemek için her makine kutusuna tıklayın

 ![Makine bağımlılıklarını görüntüleme](./media/how-to-create-group-machine-dependencies/machine-dependencies.png)

4. Zaman aralığı etikette süre tıklayarak için farklı süreler sırasında bağımlılıkları bakabilirsiniz. Varsayılan olarak aralığı bir saattir. Zaman aralığı değiştirmek veya başlangıç ve bitiş tarihlerini ve süresini belirtin.
5. Gruplamak istediğiniz bağımlı makineler tanımladıktan sonra harita üzerinde birden çok makine seçin ve Ctrl + tıklama kullanın **Grup makineleri**.
6. Bir grup adı belirtin. Bağımlı makinelere Azure geçişi tarafından bulunduğundan emin olun.

    > [!NOTE]
    > Bağımlı bir makine Azure geçişi tarafından bulunamadı, gruba eklenemiyor. Tür makineler gruba eklemek için doğru kapsamda vCenter sunucusu ile yeniden bulma işlemini çalıştırın ve Azure geçişi tarafından makine bulunduğundan emin olun gerekir.  

7. Bu grup için bir değerlendirme oluşturmak istiyorsanız, grup için yeni bir değerlendirme oluşturmak için onay kutusunu seçin.
8. Tıklayın **Tamam** grubunu kaydetmek için.

Grup oluşturulduktan sonra grubun tüm makinelerde aracıları yüklemek ve tüm Grup bağımlılığı görselleştirerek grubu geliştirmek için önerilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Bilgi nasıl](how-to-create-group-dependencies.md) Grup bağımlılıklarını görselleştirerek grubu geliştirmek için
- Değerlendirmelerin nasıl hesaplandığı hakkında [daha fazla bilgi](concepts-assessment-calculation.md) edinin.
