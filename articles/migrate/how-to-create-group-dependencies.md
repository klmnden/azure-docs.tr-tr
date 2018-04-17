---
title: Grup bağımlılık eşlemesindeki Azure geçirmek değerlendirme grubuyla İyileştir | Microsoft Docs
description: Azure geçirmek hizmetinde Grup bağımlılık eşlemesi kullanarak bir değerlendirme iyileştirmek açıklar.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 12/22/2017
ms.author: raynew
ms.openlocfilehash: a7c1dcae5708164252fa04a0fd1471eb1ae9bf90
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="refine-a-group-using-group-dependency-mapping"></a>Grup bağımlılık eşlemesi kullanarak bir grup daraltın

Bu makalede, bir grup bağımlılıkları grubundaki tüm makinelerin görselleştirme tarafından iyileştirmek açıklar. Bir değerlendirme çalıştırmadan önce varolan bir grubun üyeliğini Çapraz denetimi Grup bağımlılıkları tarafından iyileştirmek istediğinizde genellikle bu yöntemi kullanın. Bağımlılık görselleştirme kullanarak bir grup daraltmayı geçişinizi Azure.You etkili bir şekilde planlamanıza yardımcı birlikte geçirmek için gereken tüm bağımlı sistemleri bulabilir. Hiçbir şey geride bıraktığı ve Azure'a geçirilirken beklenmedik biçimde kesintiler meydana gelmediğinden emin olmanıza yardımcı olur. 


> [!NOTE]
> Bağımlılıklar görselleştirmek istediğiniz grupları 10'dan fazla makineler içermemelidir. 10'dan fazla makineler grubunda varsa, bağımlılık görselleştirme işlevselliği yararlanmak için daha küçük gruplar halinde bölmek için öneririz.


# <a name="prepare-the-group-for-dependency-visualization"></a>Grup için bağımlılık görselleştirme hazırlama
Bir grubun bağımlılıkları görüntülemek için aracıları grubun parçası olan her şirket içi makinede yükleyip gerekir. İnternet bağlantısı makinelerle varsa, ayrıca, indirmek ve yüklemek ihtiyacınız [OMS ağ geçidi](../log-analytics/log-analytics-oms-gateway.md) bunlardaki.

### <a name="download-and-install-the-vm-agents"></a>VM aracıları yükleyip
1. İçinde **genel bakış**, tıklatın **Yönet** > **grupları**gerekli grup gidin.
2. Makineler, listesinde içinde **bağımlılık Aracısı** sütun tıklatın **yükleme gerektirir** nasıl karşıdan yüklenir ve aracıları yükleme ile ilgili yönergeleri görmek için.
3. Üzerinde **bağımlılıkları** sayfasında indirin ve grubun parçası olan her bir VM üzerinde Microsoft İzleme Aracısı'nı (MMA) ve bağımlılık Aracısı'nı yükleyin.
4. Çalışma alanı kimliği ve anahtarı kopyalayın. Şirket içi makinelerde MMA yüklediğinizde bu gerekir.

### <a name="install-the-mma"></a>MMA yükleyin

Bir Windows makinesinde aracı yüklemek için:

1. İndirilen Aracısı'nı çift tıklatın.
2. **Hoş Geldiniz** sayfasında **İleri**'ye tıklayın. Üzerinde **Lisans Koşulları'nı** sayfasında, **ediyorum** lisans kabul etmek için.
3. İçinde **hedef klasörü**, saklamak veya varsayılan yükleme klasörünü değiştirmek > **sonraki**. 
4. İçinde **aracı Kur Seçenekleri**seçin **Azure günlük analizi** > **sonraki**. 
5. Tıklatın **Ekle** yeni bir günlük analizi çalışma alanı eklemek için. Çalışma alanı kimliği ve portaldan kopyaladığınız anahtarını yapıştırın. **İleri**’ye tıklayın.


Bir Linux makinesinde aracı yüklemek için:

1. Uygun paket (x86 veya x64) scp/sftp kullanarak Linux bilgisayarınıza aktarın.
2. Kullanarak paket yükleme yükleme bağımsız değişkeni.

    ```sudo sh ./omsagent-<version>.universal.x64.sh --install -w <workspace id> -s <workspace key>```


### <a name="install-the-dependency-agent"></a>Bağımlılık Aracısı'nı yükleme
1. Bir Windows makinesinde bağımlılık Aracısı'nı yüklemek için kurulum dosyasını çift tıklatın ve sihirbazı izleyin.
2. Bir Linux makinesinde bağımlılık Aracısı'nı yüklemek için aşağıdaki komutu kullanarak kök olarak yükleyin:

    ```sh InstallDependencyAgent-Linux64.bin```

[Daha fazla bilgi edinin](../operations-management-suite/operations-management-suite-service-map-configure.md#supported-operating-systems) bağımlılık aracısı tarafından desteklenen işletim sistemleri hakkında. 

## <a name="refine-the-group-based-on-dependency-visualization"></a>Bağımlılık görselleştirme dayalı olarak grubu daraltın
Grubun tüm makinelerde aracılarını yükledikten sonra Grup bağımlılıkları görselleştirmek ve izleyerek İyileştir aşağıdaki adımları.

1. Azure projesi altında geçirmek **Yönet**, tıklatın **grupları**ve grubu seçin.
2. Grup sayfasında, tıklatın **bağımlılıklarını görüntüleme**Grup bağımlılık Haritası açmak için.
3. Grup için bağımlılık Haritası aşağıdaki ayrıntıları gösterir:
    - (İstemciler) gelen ve giden (Sunucuları) TCP bağlantıları / grubunun parçası olan tüm makinelerden
        - MMA ve bağımlılık aracısı yüklü olmayan bağımlı makineler bağlantı noktası numaralarını göre gruplandırılır
        - MMA ve bağımlılık aracısı yüklü olan dependenct makineler ayrı kutuları olarak gösterilir 
    - Makine içinde çalışan işlemler, işlemleri görüntülemek için her makine kutusunu genişletebilirsiniz.
    - Tam etki alanı adı, işletim sistemi, her makinenin MAC adresi vb. gibi özellikleri, bu ayrıntıları görüntülemek için her makine kutusunu tıklatabilirsiniz.

     ![Grup bağımlılıklarını görüntüleme](./media/how-to-create-group-dependencies/view-group-dependencies.png)

3. Daha ayrıntılı bağımlılıkları görüntülemek için zaman aralığını değiştirmek için tıklayın. Varsayılan olarak, aralığı bir saattir. Zaman aralığını değiştirmek veya başlangıç ve bitiş tarihleri ve süresini belirtin.
4. Her makinede çalışan işlem bağımlı makineleri doğrulamak ve gruptan kaldırılan veya eklenen gerekir makineler tanımlayın.
5. Makineler eklemek veya gruptan kaldırmak için harita üzerinde seçmek için CTRL + Click kullanın.
    - Yalnızca bulunmuş makineler ekleyebilirsiniz.
    - Ekleme ve makineler bir gruptan kaldırma değerlendirmeleri onun için geçmiş geçersiz kılar.
    - Grup değiştirdiğinizde, isteğe bağlı olarak yeni bir değerlendirme oluşturabilirsiniz.
5. Tıklatın **Tamam** grubunu kaydetmek için.

    ![Makineler Ekle Kaldır](./media/how-to-create-group-dependencies/add-remove.png)

Grup bağımlılık Haritası görünür belirli bir makine bağımlılıkları denetlemek istiyorsanız [makine bağımlılık eşlemesi ayarlamanız](how-to-create-group-machine-dependencies.md).


## <a name="next-steps"></a>Sonraki adımlar

Değerlendirmelerin nasıl hesaplandığı hakkında [daha fazla bilgi](concepts-assessment-calculation.md) edinin.
