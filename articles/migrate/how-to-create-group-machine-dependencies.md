---
title: Grup Azure geçirme ile makine bağımlılıkları kullanarak makine | Microsoft Docs
description: Makine bağımlılıkları ile Azure geçiş hizmetini kullanarak bir değerlendirme oluşturmayı açıklar.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 05/15/2018
ms.author: raynew
ms.openlocfilehash: a9850044266ec05cee5e32c6825609bcf969351d
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="group-machines-using-machine-dependency-mapping"></a>Makine bağımlılık eşleme kullanarak grubu makineleri

Bu makine için bir grup oluşturmak makalede [Azure geçirmek](migrate-overview.md) makinelerin bağımlılıkları görselleştirme tarafından değerlendirmesi. Makine bağımlılıkları Çapraz denetimi, bir değerlendirme çalıştırmadan önce tarafından güvenirlik daha yüksek düzeyde VM'ler gruplarıyla değerlendirmek istediğiniz zaman genellikle bu yöntemi kullanın. Bağımlılık görselleştirme etkili bir şekilde Azure geçişinizi planlama yapmanıza yardımcı olabilir. Hiçbir şey geride bıraktığı ve Azure'a geçirilirken beklenmedik biçimde kesintiler meydana gelmediğinden emin olmanıza yardımcı olur. Birlikte geçirmek ve çalışan bir sistemi kullanıcılar hala sunma veya geçiş yerine yetkisini için bir adaydır olup olmadığını belirlemek için gereken tüm bağımlı sistemleri bulabilir. 


## <a name="prepare-machines-for-dependency-mapping"></a>Makineler bağımlılık eşlemesi için hazırlanma
Makinelerin bağımlılıkları görüntülemek için aracıları değerlendirmek istediğiniz her şirket içi makinede yükleyip gerekir. İnternet bağlantısı makinelerle varsa, ayrıca, indirmek ve yüklemek ihtiyacınız [OMS ağ geçidi](../log-analytics/log-analytics-oms-gateway.md) bunlardaki.

### <a name="download-and-install-the-vm-agents"></a>Sanal makine aracılarını indirip yükleme
1. İçinde **genel bakış**, tıklatın **Yönet** > **makineler**ve gerekli makineyi seçin.
2. İçinde **bağımlılıkları** sütun tıklatın **aracıları yüklemek**. 
3. Üzerinde **bağımlılıkları** sayfasında indirin ve değerlendirmek istediğiniz her bir VM üzerinde Microsoft İzleme Aracısı'nı (MMA) ve bağımlılık Aracısı'nı yükleyin.
4. Çalışma alanı kimliğini ve anahtarını kopyalayın. Şirket içi makinede MMA yüklediğinizde bu gerekir.

### <a name="install-the-mma"></a>MMA’yı yükleme

Bir Windows makinesinde aracı yüklemek için:

1. İndirilen aracıya çift tıklayın.
2. **Hoş Geldiniz** sayfasında **İleri**'ye tıklayın. **Lisans Koşulları** sayfasında **Kabul Ediyorum**’a tıklayarak lisansı kabul edin.
3. İçinde **hedef klasörü**, saklamak veya varsayılan yükleme klasörünü değiştirmek > **sonraki**. 
4. İçinde **aracı Kur Seçenekleri**seçin **Azure günlük analizi** > **sonraki**. 
5. Tıklatın **Ekle** yeni bir günlük analizi çalışma alanı eklemek için. Çalışma alanı kimliği ve portaldan kopyaladığınız anahtarını yapıştırın. **İleri**’ye tıklayın.


Bir Linux makinesinde aracı yüklemek için:

1. Uygun paket (x86 veya x64) scp/sftp kullanarak Linux bilgisayarınıza aktarın.
2. Kullanarak paket yükleme yükleme bağımsız değişkeni.

    ```sudo sh ./omsagent-<version>.universal.x64.sh --install -w <workspace id> -s <workspace key>```


### <a name="install-the-dependency-agent"></a>Bağımlılık aracısını yükleme
1. Bir Windows makinesinde bağımlılık Aracısı'nı yüklemek için kurulum dosyasını çift tıklatın ve sihirbazı izleyin.
2. Bir Linux makinesinde bağımlılık Aracısı'nı yüklemek için aşağıdaki komutu kullanarak kök olarak yükleyin:

    ```sh InstallDependencyAgent-Linux64.bin```

[Daha fazla bilgi edinin](../monitoring/monitoring-service-map-configure.md#supported-operating-systems) bağımlılık aracısı tarafından desteklenen işletim sistemleri hakkında. 

## <a name="create-a-group"></a>Grup oluştur

1. Aracılar yüklendikten sonra portal'ı tıklatın'a gidin ve **Yönet** > **makineler**.
2. Aracıların yüklü olduğu makinede arayın.
3. **Bağımlılıkları** sütun makine için şimdi olarak göster **bağımlılıklarını görüntüleme**. Sütunu makinenin bağımlılıkları görüntülemek için tıklatın.
4. Makine için bağımlılık Haritası aşağıdaki ayrıntıları gösterir:
    - (İstemciler) gelen ve giden (Sunucuları) TCP bağlantıları/makineden
        - MMA ve bağımlılık aracısı yüklü olmayan bağımlı makineler bağlantı noktası numaralarını göre gruplandırılır
        - MMA ve bağımlılık aracısı yüklü olan dependenct makineler ayrı kutuları olarak gösterilir 
    - Makine içinde çalışan işlemler, işlemleri görüntülemek için her makine kutusunu genişletebilirsiniz.
    - Tam etki alanı adı, işletim sistemi, her makinenin MAC adresi vb. gibi özellikleri, bu ayrıntıları görüntülemek için her makine kutusunu tıklatabilirsiniz.

 ![Makine bağımlılıklarını görüntüleme](./media/how-to-create-group-machine-dependencies/machine-dependencies.png)

4. Zaman aralığı etiketinde süre tıklayarak farklı süreler için bağımlılıkları bakabilirsiniz. Varsayılan olarak aralığı bir saattir. Zaman aralığını değiştirmek veya başlangıç ve bitiş tarihleri ve süresini belirtin.
5. Grup haline getirmek istediğiniz bağımlı makineleri tanımladıktan sonra Ctrl + Click harita üzerinde birden fazla makine seçin ve tıklatın kullanın **Grup makineler**.
6. Bir grup adı belirtin. Bağımlı makineler Azure geçirmek tarafından bulunduğundan emin olun. 

    > [!NOTE]
    > Bağımlı bir makine Azure geçirmek tarafından bulunamazsa, gruba ekleyemezsiniz. Bu tür makineler grubuna eklemek için sağ kapsamıyla vcenter Server'daki bulma işlemini yeniden çalıştırın ve makine Azure geçirmek tarafından bulunduğundan emin olun gerekir.  

7. Bu grup için bir değerlendirme oluşturmak istiyorsanız, grup için yeni bir değerlendirme oluşturmak için onay kutusunu seçin.
8. Tıklatın **Tamam** grubunu kaydetmek için.

Grup oluşturulduktan sonra grubun tüm makinelerde aracıları yüklemek ve grubun tüm Grup bağımlılık görselleştirme tarafından iyileştirmek için önerilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Bilgi nasıl](how-to-create-group-dependencies.md) Grup bağımlılıkları görselleştirme tarafından Grup iyileştirmek için
- Değerlendirmelerin nasıl hesaplandığı hakkında [daha fazla bilgi](concepts-assessment-calculation.md) edinin.
