---
title: Hazırlanmış veri analizi çözümü, Azure ve Azure Stack ile oluştur | Microsoft Docs
description: Azure ve Azure Stack ile hazırlanmış veri analizi çözümü oluşturmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/01/2018
ms.author: mabrigg
ms.reviewer: Anjay.Ajodha
ms.openlocfilehash: d63faf63012360d4448166ac5d69eba6ede9d0ed
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52969541"
---
# <a name="tutorial-create-a-staged-data-analytics-solution-with-azure-and-azure-stack"></a>Öğretici: Azure ve Azure Stack ile hazırlanmış veri analizi çözümü oluşturma 

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Çoklu tesis kuruluşların taleplerini karşılamak üzere hem şirket içi hem de genel bulut ortamları kullanmayı öğrenin. Azure Stack toplama, işleme, depolama ve özellikle güvenlik, gizlilik, şirket ilkesi ve Mevzuat gereklilikleri konumlar arasında farklılık gösterebilir, yerel ve uzak veri dağıtmak için hızlı, güvenli ve esnek bir çözüm sunar. ve kullanıcılar.

Bu düzende, müşterilerinizin noktasında koleksiyonu gerekir ve böylece hızlı kararlar hale getirilebilir veri topluyoruz. Genellikle bu veri toplama, Internet erişimi oluşur. Bağlantı kurulduğunda verilere ek Öngörüler elde etmek için bir kaynak kullanımı yoğun çözümlemesi gerekebilir. Genel bulut çok geç veya kullanılabilir olduğunda yine de verileri analiz edebilirsiniz.

Bu öğreticide, bir örnek ortamı için derleme:

> [!div class="checklist"]
> - Ham veri depolama blob'u oluşturun.
> - Verileri temizleme Azure yığını, Azure'a taşımak için yeni bir Azure Stack işlevi oluşturun.
> - Blob Depolama ile tetiklenen bir işlev oluşturun.
> - Azure Stack, blob ve kuyruk içeren bir depolama hesabı oluşturun.
> - Kuyruk ile tetiklenen bir işlev oluşturun.
> - Test kuyruk ile tetiklenen işlev.

> [!Tip]  
> ![karma pillars.png](./media/azure-stack-solution-cloud-burst/hybrid-pillars.png)  
> Microsoft Azure Stack, Azure'nın bir uzantısıdır. Azure Stack çevikliğini ve yenilik bulut bilgi işlem, şirket içi ortamınıza ve hibrit uygulamaları her yerde oluşturup dağıtmayı olanak tanıyan tek hibrit Bulutu sunar.  
> 
> Teknik incelemeyi [karma uygulamaları için tasarım konuları](https://aka.ms/hybrid-cloud-applications-pillars) (yerleştirme, ölçeklenebilirlik, kullanılabilirlik, dayanıklılık, yönetilebilirlik ve güvenlik) yazılım kalitesinin yapı taşları tasarlama, dağıtma ve çalıştırma için gözden geçirmeleri karma uygulamalar. Tasarım konuları, üretim ortamlarında sorunlarını en aza karma uygulama tasarımının en iyi duruma getirme yardımcı olur.

## <a name="prerequisites"></a>Önkoşullar

Bazı hazırlık, bu çözümü oluşturmak için gereklidir:

-   Yüklü ve çalışır durumda bir Azure Stack (daha fazla bilgi burada bulunabilir: [Azure Stack genel bakış](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-storage-overview))

-   Azure aboneliği. (Oluşturma bir [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F))

-   [Microsoft Azure Depolama Gezgini](http://storageexplorer.com/)'ni indirip yükleme.

-   İşlevler tarafından işlenmek üzere kendi veri sağlamanız gerekir. Verileri, oluşturulan ve Azure Stack depolama blob kapsayıcısını karşıya yüklemek kullanılabilir olmalıdır.

## <a name="issues-and-considerations"></a>Sorunlar ve Dikkat Edilmesi Gerekenler

### <a name="scalability-considerations"></a>Ölçeklenebilirlik konusunda dikkat edilmesi gerekenler

Azure işlevleri ve depolama çözümleri, veri hacmi ve işlem taleplerini karşılayacak şekilde ölçeklendirin. Azure ölçeklenebilirlik bilgileri ve hedefleri için bkz: [Azure ölçeklenebilirlik belgeleri](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets).

### <a name="availability-considerations"></a>Kullanılabilirlik konusunda dikkat edilmesi gerekenler

Bu düzen birincil kullanılabilirlik meselesi depolamadır. Hızlı bağlantılar üzerinden bağlantı, büyük veri toplu işleme ve dağıtım için gereklidir.

### <a name="manageability-considerations"></a>Yönetilebilirlik konusunda dikkat edilmesi gerekenler

Geliştirme araçları ve kaynak denetimine çözümünüzü yönetmenizi nasıl etkinleştirecektir göz önünde bulundurun.

## <a name="create-the-raw-data-storage-blob"></a>Ham verileri depolama blobu oluştur

Depolama hesabı ve blob kapsayıcısı, makine ve çalışan etkinlik, tesis veri, üretim ölçümleri ve diğer raporlama dahil olmak üzere şirket içi etkinlikleri tarafından oluşturulan tüm orijinal verileri tutar.

1.  Oturum [ *Azure Stack portalı*](https://portal.local.azurestack.external/).

2.  Azure Stack Portal'da hizmet menüsünü açın ve sol taraftaki menüyü genişleterek **tüm hizmetleri**. Ekranı aşağı kaydırarak **depolama** ve **depolama hesapları**. Depolama hesapları penceresinde **Ekle**.

3.  Hesap için aşağıdaki bilgileri kullanın:

    a.  Ad: **seçiminiz**

    b.  Dağıtım modeli: **Resource Manager**

    c.  Hesap türü: **depolama (genel amaçlı V1)**

    d.  Konum: **Batı ABD**

    e.  Çoğaltma: **yerel olarak yedekli depolama (LRS)**

    f.  Performans: **standart**

    g.  Güvenli aktarım gereklidir: **devre dışı**

    h.  Abonelik: seçin

    i.  Kaynak grubu: yeni bir kaynak grubu belirtin veya mevcut bir kaynak grubu seçin

    j.  Sanal ağları yapılandırma: **devre dışı**

4.  Seçin **depolama hesabı oluşturmak için Oluştur**.

    ![Alternatif metin](media/azure-stack-solution-staged-data-analytics/image1.png)

5.  Oluşturulduktan sonra depolama hesabı adını seçin.

6.  Hesabı dikey penceresinden, altında **BLOB hizmeti** başlığı seçin **kapsayıcıları**.

7.  Dikey pencerenin en üstünde seçin **+ kapsayıcı.** seçip **kapsayıcı**.

    ![Alternatif metin](media/azure-stack-solution-staged-data-analytics/image2.png)

8.  Ad: **seçiminiz**

9.  Genel erişim düzeyi: **kapsayıcı** (kapsayıcılar ve bloblar için anonim okuma erişimi)

10.  **Tamam**’ı seçin.

## <a name="create-an-azure-stack-function"></a>Azure Stack işlev oluşturma

Verileri temizleme Azure yığını, Azure'a taşımak için yeni bir Azure Stack işlevi oluşturun.

### <a name="create-the-azure-stack-function-app"></a>Azure Stack işlev uygulaması oluşturma

1. Oturum [Azure Stack portalı](https://portal.local.azurestack.external).
2. **Tüm Hizmetler**’i seçin.
3. Seçin **işlev uygulamaları** içinde **Web + mobil** grubu.

4.  Görüntünün altındaki tabloda belirtilen ayarları kullanarak bir işlev uygulaması oluşturun.

    | Ayar | Önerilen değer | Açıklama |
    | ---- | ---- | ---- |
    | Uygulama adı | Genel olarak benzersiz bir ad | Yeni işlev uygulamanızı tanımlayan ad. Geçerli karakterler `a` - `z`, `0``-9`, ve `-`. |
    | Abonelik | Aboneliğiniz | Bu yeni işlev uygulamasının oluşturulduğu abonelik. |
    | **Kaynak Grubu** |  |  |
    | myResourceGroup | İşlev uygulamanızın oluşturulacağı yeni kaynak grubunun adı. |  |
    | İşletim Sistemi | Windows | Sunucusuz barındırma şu anda yalnızca Windows’ta çalıştırıldığında kullanılabilir. |
    | **Barındırma planı** |  |  |
    | Tüketim planı | Kaynakların işlev uygulamanıza nasıl ayrılacağını tanımlayan barındırma planı. ' De varsayılan tüketim planı, kaynaklar olarak işlevlerin taleplerine göre dinamik olarak eklenir. Bu sunucusuz barındırmada, yalnızca işlevlerinizin çalıştığı süre için ödeme yaparsınız. |  |
    | Konum | Size en yakın bölge | İşlevleri erişiminizi erişeceği diğer hizmetlere ya da size yakın bir bölge seçin. |
    | **Depolama hesabı** |  |  |
    | \<Yukarıda oluşturulan depolama hesabı > | İşlev uygulamanız tarafından kullanılan yeni depolama hesabının adı. Depolama hesabı adları 3 ila 24 karakter uzunluğunda olmalıdır. Ad yalnızca sayılar ve küçük harfler kullanabilirsiniz. Var olan bir hesabı da kullanabilirsiniz. |  |

    **Örnek:**

    ![Yeni işlev uygulaması ayarlarını tanımlama](media/azure-stack-solution-staged-data-analytics/image6.png)

5.  İşlev uygulamasını sağlamak ve dağıtmak için **Oluştur**'u seçin.

6.  Portalın sağ üst köşesindeki Bildirim simgesini seçin ve **Dağıtım başarılı** iletisini bekleyin.

    ![Yeni işlev uygulaması ayarlarını tanımlama](media/azure-stack-solution-staged-data-analytics/image7.png)

7.  Seçin **kaynağa Git** yeni işlev uygulaması görüntülemek için.

![İşlev uygulaması başarıyla oluşturuldu.](media/azure-stack-solution-staged-data-analytics/image8.png)

### <a name="add-a-function-to-the-azure-stack-function-app"></a>Azure Stack işlev uygulamasına bir işlev Ekle

1.  Üzerine tıklayarak yeni bir işlev oluşturma **işlevleri**, ardından **+ yeni işlev** düğmesi.

    ![Alternatif metin](media/azure-stack-solution-staged-data-analytics/image3.png)

2.  Seçin **Zamanlayıcı tetikleyicisi**.

    ![Alternatif metin](media/azure-stack-solution-staged-data-analytics/image4.png)

3.  Seçin **C\#**  dil ve işlev adı: `upload-to-azure` zamanlamasını ayarlamak `0 0 * * * *`, hangi CRON içinde bir kez bir saat gösterimidir.

    ![Alternatif metin](media/azure-stack-solution-staged-data-analytics/image5.png)

## <a name="create-a-blob-storage-triggered-function"></a>Blob depolama ile tetiklenen bir işlev oluşturma

1.  İşlev uygulaması'nı genişletin ve seçin **+** düğmesinin yanındaki **işlevleri**.

2.  Arama alanına yazın `blob` için istediğiniz dili seçin **Blob tetikleyicisi** şablonu.

  ![Blob depolama tetikleyici şablonunu seçin.](media/azure-stack-solution-staged-data-analytics/image10.png)

3.  Aşağıdaki tabloda belirtilen ayarları kullanın:

    | Ayar | Önerilen değer | Açıklama |
    | ------- | ------- | ------- |
    | Ad | İşlev uygulamanızda benzersiz olmalıdır | Blob ile tetiklenen bu işlevin adı. |
    | Yol | \<depolama konumu yolu > | İzlenmekte olan Blob depolamanın konumu. Blob dosya adı bağlamaya name parametresi olarak geçirilir. |
    | Depolama hesabı bağlantısı | İşlevi uygulama bağlantısı | İşlev uygulamanız tarafından zaten kullanılan depolama hesabı bağlantısı kullanın veya yeni bir tane oluşturun. |

    **Örnek:**

    ![Blob depolama ile tetiklenen bir işlev oluşturun.](media/azure-stack-solution-staged-data-analytics/image11.png)

4.  Seçin **Oluştur** işlevi oluşturmak için.

### <a name="test-the-function"></a>İşlevi test etme

1.  Azure portalında işlevine göz atın. Genişletin **günlükleri** sayfanın alt kısmındaki ve günlük akışını değil duraklatıldı emin olun.

2.  Depolama Gezgini'ni açın ve bu bölümün başında oluşturduğunuz depolama hesabına bağlanın.

3.  Depolama hesabını genişletin **Blob kapsayıcıları**, ve daha önce oluşturduğunuz blob. Seçin **karşıya** ardından **dosyaları karşıya yükleme**.

    ![Dosyayı blob kapsayıcısına yükleyin.](media/azure-stack-solution-staged-data-analytics/image12.png)

4.  Dosyaları karşıya yükleme iletişim kutusunda dosyaları alanı seçin. Bir görüntü dosyası gibi yerel bir bilgisayardaki bir dosyaya göz atın, onu seçin ve seçin **açık** ardından **karşıya**.

5.  İşlev günlüklerini geri dönün ve blob'un okunduğunu doğrulayın.

    **Örnek:**

    ![Günlüklerde iletiyi görüntüleyin.](media/azure-stack-solution-staged-data-analytics/image13.png)

## <a name="create-an-azure-stack-storage-account"></a>Bir Azure Stack depolama hesabı oluşturma

Azure Stack, blob ve kuyruk içeren bir depolama hesabı oluşturun.

### <a name="storage-blob--data-archiving"></a>Depolama Blob verileri arşivleme

Bu depolama hesabında iki kapsayıcı kümelerinizi barındıracak. Bu, bir blob arşiv verileri tutmak için kullanılan ve ana ofis dağıtım için atanmış veri işleme için kullanılan bir sıra kapsayıcılardır.

Arşiv depolama olarak başka bir depolama hesabı ve blob kapsayıcısı oluşturmak için yukarıda özetlenen ayarları ve adımları kullanın.

### <a name="storage-queue--filtered-data-holding"></a>Depolama kuyruğu filtrelenen verilerini tutma

1.  Yeni depolama hesabına erişmek için yukarıda özetlenen ayarları ve adımları kullanın.

2.  Depolama hesabı genel bakış bölümünde **kuyruk.**

3.  Seçin **+ kuyruk** ve doldurma **adı** alan ve doldurma yeni kuyruğu için bir ad.

4.  Seçin **Tamam.**

    ![Alternatif metin](media/azure-stack-solution-staged-data-analytics/image14.png)

    ![Alternatif metin](media/azure-stack-solution-staged-data-analytics/image15.png)

## <a name="create-a-queue-triggered-function"></a>Kuyruk ile tetiklenen bir işlev oluşturma

1.  Adımlar yukarıdaki işlevi oluşturma bölümünde ek bir Azure Stack işlevi bir sıra INSTEAD OF tetikleyicisi bir blob tetikleyicisi oluşturmak için kullanın.

2.  Aşağıdaki tabloda belirtilen ayarları kullanın:

    | Ayar | Önerilen değer | Açıklama |
    | ------- | ------- | ------- |
    | Ad | İşlev uygulamanızda benzersiz olmalıdır | Kuyruk tarafından tetiklenen bu işlevin adı. |
    | Yol | \<depolama konumu yolu > | İzlenmekte olan depolama konumu. Kuyruğun dosya adı bağlamaya name parametresi olarak geçirilir. |
    | Depolama hesabı bağlantısı | İşlevi uygulama bağlantısı | İşlev uygulamanız tarafından zaten kullanılan depolama hesabı bağlantısı kullanın veya yeni bir tane oluşturun. |

3.  Seçin **Oluştur** işlevi oluşturmak için.

## <a name="test-the-queue-triggered-function"></a>Test kuyruk ile tetiklenen işlev

1.  Azure Stack portalında işlevine göz atın. Genişletin **günlükleri** sayfanın alt kısmındaki ve günlük akışını değil duraklatıldı emin olun.

2.  Depolama Gezgini'ni açın ve bu bölümün başında oluşturduğunuz depolama hesabına bağlanın.

3.  Depolama hesabını genişletin **Blob kapsayıcıları**, ve daha önce oluşturduğunuz blob. Seçin **karşıya** ardından **dosyaları karşıya yükleme.**

    ![Dosyayı blob kapsayıcısına yükleyin.](media/azure-stack-solution-staged-data-analytics/image12.png)

4.  Dosyaları karşıya yükleme iletişim kutusunda dosyaları alanı seçin. Bir görüntü dosyası gibi yerel bir bilgisayardaki bir dosyaya göz atın, onu seçin ve seçin **açık** ardından **karşıya**.

5.  İşlev günlüklerini geri dönün ve blob'un okunduğunu doğrulayın.

  **Örnek:**

    ![Günlüklerde iletiyi görüntüleyin.](media/azure-stack-solution-staged-data-analytics/image13.png)

## <a name="securely-stored-and-accessed-compliant-data"></a>Güvenli bir şekilde depolanır ve erişilen uyumlu veri

Genel Kurumsal verilere güvenli bir şekilde depolanan işlenen, dağıtılmış ve Azure ve Azure Stack aşamalı veri analizi ve özel uç nokta yönergeleri erişilebilir. Uzaktan ofis çalışanı ve makineler etkinlikleri, tesis verileri ve iş ölçümlerini sürekli olarak derlenmiş, depolanan, uyumluluk için test ve şirket ilkeleri ve bölgesel yönetmeliği göre dağıtılmış.

## <a name="next-steps"></a>Sonraki adımlar

- Azure bulut desenleri hakkında daha fazla bilgi için bkz: [bulut tasarımı desenleri](https://docs.microsoft.com/azure/architecture/patterns).