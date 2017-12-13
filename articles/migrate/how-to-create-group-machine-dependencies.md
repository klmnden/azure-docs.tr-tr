---
title: "Grup Azure geçirme ile makine bağımlılıkları kullanarak makine | Microsoft Docs"
description: "Makine bağımlılıkları ile Azure geçiş hizmetini kullanarak bir değerlendirme oluşturmayı açıklar."
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 12/12/2017
ms.author: raynew
ms.openlocfilehash: 769c05916de4e7ad5b14812c2c8dbcf69e91320c
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2017
---
# <a name="group-machines-using-machine-dependency-mapping"></a>Makine bağımlılık eşleme kullanarak grubu makineleri

Bu makalede makineler için bir grup oluşturmak nasıl [Azure geçirmek](migrate-overview.md) makine bağımlılık eşleme kullanarak değerlendirmesi. Makine bağımlılıkları Çapraz denetimi, bir değerlendirme çalıştırmadan önce tarafından güvenirlik daha yüksek düzeyde VM'ler gruplarıyla değerlendirmek istediğiniz zaman genellikle bu yöntemi kullanın.



## <a name="prepare-machines-for-dependency-mapping"></a>Makineler bağımlılık eşlemesi için hazırlanma
Bağımlılık eşlemesindeki makineler eklemek için aracıları değerlendirmek istediğiniz her şirket içi makinede yükleyip gerekir. İnternet bağlantısı makinelerle varsa, ayrıca, indirmek ve yüklemek ihtiyacınız [OMS ağ geçidi](../log-analytics/log-analytics-oms-gateway.md) bunlardaki.

### <a name="download-and-install-the-vm-agents"></a>VM aracıları yükleyip
1. İçinde **genel bakış**, tıklatın **Yönet** > **makineler**ve gerekli makineyi seçin.
2. İçinde **bağımlılıkları** sütun tıklatın **aracıları yüklemek**. 
3. Üzerinde **bağımlılıkları** sayfasında indirin ve değerlendirmek istediğiniz her bir VM üzerinde Microsoft İzleme Aracısı'nı (MMA) ve bağımlılık Aracısı'nı yükleyin.
4. Çalışma alanı kimliği ve anahtarı kopyalayın. Şirket içi makinede MMA yüklediğinizde bu gerekir.

### <a name="install-the-mma"></a>MMA yükleyin

Bir Windows makinesinde aracı yüklemek için:

1. İndirilen Aracısı'nı çift tıklatın.
2. Üzerinde **Hoş Geldiniz** sayfasında, **sonraki**. Üzerinde **Lisans Koşulları'nı** sayfasında, **ediyorum** lisans kabul etmek için.
3. İçinde **hedef klasörü**, saklamak veya varsayılan yükleme klasörünü değiştirmek > **sonraki**. 
4. İçinde **aracı Kur Seçenekleri**seçin **Azure günlük analizi (OMS)** > **sonraki**. 
5. Tıklatın **Ekle** yeni bir OMS çalışma alanı eklemek için. Çalışma alanı kimliği ve portaldan kopyaladığınız anahtarını yapıştırın. **İleri**’ye tıklayın.


Bir Linux makinesinde aracı yüklemek için:

1. Uygun paket (x86 veya x64) scp/sftp kullanarak Linux bilgisayarınıza aktarın.
2. Kullanarak paket yükleme yükleme bağımsız değişkeni.

    ```sudo sh ./omsagent-<version>.universal.x64.sh --install -w <workspace id> -s <workspace key>```


### <a name="install-the-dependency-agent"></a>Bağımlılık Aracısı'nı yükleme
1. Bir Windows makinesinde bağımlılık Aracısı'nı yüklemek için kurulum dosyasını çift tıklatın ve sihirbazı izleyin.
2. Bir Linux makinesinde bağımlılık Aracısı'nı yüklemek için aşağıdaki komutu kullanarak kök olarak yükleyin:

    ```sh InstallDependencyAgent-Linux64.bin```

[Daha fazla bilgi edinin](../operations-management-suite/operations-management-suite-service-map-configure.md#supported-operating-systems) bağımlılık aracısı tarafından desteklenen işletim sistemleri hakkında. 

## <a name="create-a-group"></a>Bir grup oluşturun

1. Aracılar yüklendikten sonra portal'ı tıklatın'a gidin ve **Yönet** > **makineler**.
2. **Bağımlılıkları** sütun şimdi olarak göster **bağımlılıklarını görüntüleme**. Sütunu bağımlılıkları görüntülemek için tıklatın.
3. Her makine için doğrulayabilirsiniz:
    - Olup MMA ve bağımlılık Aracısı yüklenir ve makine olup olmadığını buldu.
    - Makinede çalışan konuk işletim sistemi.
    - Gelen ve giden IP bağlantıları ve bağlantı noktaları.
    - Makinelerde çalışan işlemler.
    - Makineler arasındaki bağımlılıkları.

4. Daha ayrıntılı bağımlılıklar için zaman aralığını değiştirmek için tıklayın. Varsayılan olarak aralığı bir saattir. Zaman aralığını değiştirmek veya başlangıç ve bitiş tarihleri ve süresini belirtin.
5. Grup haline getirmek istediğiniz bağımlı makineleri tanımladıktan sonra harita üzerinde makineleri seçin ve'ı tıklatın **Grup makineler**.
6. Bir grup adı belirtin. Makine Azure geçirmek tarafından bulunduğundan emin olun. Bulma işlemi içi tekrar çalıştırırsanız değil. İsterseniz, bir değerlendirme hemen çalıştırabilirsiniz.
7. Tıklatın **Tamam** grubunu kaydetmek için.

    ![Makine bağımlılıkları olan bir grup oluşturun](./media/how-to-create-group-machine-dependencies/create-group.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Bilgi nasıl](how-to-create-group-dependencies.md) Grup bağımlılıkları denetleyerek Grup iyileştirmek için
- [Daha fazla bilgi edinin](concepts-assessment-calculation.md) değerlendirmelerinin nasıl hesaplandığını hakkında.
