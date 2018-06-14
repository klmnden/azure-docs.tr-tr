---
title: İçeri ve dışarı aktarma Azure Redis önbelleği verilerde | Microsoft Docs
description: İçeri aktarma ve blob depolama, premium Azure Redis önbelleği örnekleri ile gelen ve giden verileri dışarı aktarma hakkında bilgi edinin
services: redis-cache
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
ms.assetid: 4a68ac38-87af-4075-adab-569d37d7cc9e
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: wesmc
ms.openlocfilehash: db488f759752880a47a78dfeec13b14f65bd503c
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
ms.locfileid: "27910096"
---
# <a name="import-and-export-data-in-azure-redis-cache"></a>İçeri ve dışarı aktarma veri Azure Redis önbelleği
İçeri/dışarı aktarma veri Azure Redis önbelleğine alma veya içeri aktarma ve Redis önbelleği veritabanı'nı (RDB) anlık görüntü premium önbellekten bir Azure blob verme Azure Redis Önbelleği'nden veri verme olanak tanıyan bir Azure Redis önbelleği veri yönetimi, bir işlemdir Depolama hesabı. 

- **Dışarı aktarma** -sayfa blobu için Azure Redis önbelleği RDB anlık görüntü dışarı aktarabilirsiniz.
- **Alma** -sayfa blobu veya bir blok blobu, Redis önbelleği RDB anlık görüntüleri içeri aktarabilirsiniz.

İçeri/dışarı aktarma, farklı Azure Redis önbelleği örnekleri arasında geçirmek veya önbellek kullanmadan önce verileri ile doldurmak sağlar.

Bu makalede, Azure Redis önbelleği ile veri alma ve verme için bir kılavuz sağlar ve sık sorulan soruların yanıtlarını içerir.

> [!IMPORTANT]
> İçeri/dışarı aktarma Önizleme aşamasındadır ve yalnızca için kullanılabilir [premium katmanı](cache-premium-tier-intro.md) önbelleğe alır.
>
>

## <a name="import"></a>İçeri Aktarma
İçeri aktarma, herhangi bir bulut veya ortamını Linux, Windows ya da herhangi bir bulut sağlayıcısına Amazon Web Hizmetleri ve diğerleri gibi çalışan Redis çalıştıran herhangi bir Redis sunucudan Redis uyumlu RDB dosyaları getirmek için kullanılabilir. Veri alma, önceden doldurulmuş haldedir verilerle önbellek oluşturmak için kolay bir yoludur. İçeri aktarma işlemi sırasında Azure Redis önbelleği RDB dosyaları Azure Storage'dan belleğe yükler ve ardından anahtarları önbelleğe ekler.

> [!NOTE]
> İçeri aktarma işlemi başlamadan önce Redis veritabanı (RDB) dosya veya dosyalar sayfası veya blok blobları aynı bölgede ve Azure Redis önbelleği örneğinizi olarak abonelik Azure depolama alanında içine karşıya emin olun. Daha fazla bilgi için bkz: [Azure Blob storage ile çalışmaya başlama](../storage/blobs/storage-dotnet-how-to-use-blobs.md). RDB dosyasını kullanarak veriliyorsa [Azure Redis önbelleği dışarı aktarma](#export) özelliği RDB dosyanızın bir sayfa blob'u zaten depolanır ve içeri aktarmak için hazırdır.
>
>

1. Bir veya daha fazla dışa aktarılan önbellek BLOB, içeri aktarmak için [Gözat önbelleğiniz için](cache-configure.md#configure-redis-cache-settings) tıklatın ve Azure Portalı'ndaki **veri içeri aktarma** gelen **kaynak menü**.

    ![Veri içeri aktarma][cache-import-data]
2. Tıklatın **seçin Blob(s)** ve içeri aktarmak için verileri içeren depolama hesabı seçin.

    ![Depolama hesabı seç][cache-import-choose-storage-account]
3. Alınacak verileri içeren kapsayıcı'ı tıklatın.

    ![Kapsayıcı seçin][cache-import-choose-container]
4. Blob adı solundaki alanda tıklayarak içeri aktarmak için bir veya daha fazla BLOB seçin ve ardından **seçin**.

    ![BLOB'ları seçin][cache-import-choose-blobs]
5. Tıklatın **alma** içeri aktarma işlemini başlatmak için.

   > [!IMPORTANT]
   > Önbelleğe alma işlemi sırasında önbelleği istemcileri tarafından erişilebilir değil ve önbelleğinde mevcut verileri silinir.
   >
   >

    ![İçeri Aktarma][cache-import-blobs]

    Azure portalından bildirimleri izleyerek veya olayları görüntüleyerek ilerleme durumunu alma işlemini izleyebilirsiniz [denetim günlüğü](../azure-resource-manager/resource-group-audit.md).

    ![İçeri aktarma ilerlemesi][cache-import-data-import-complete]

## <a name="export"></a>Dışarı Aktarma
Dışarı aktarma, Azure Redis uyumlu RDB dosyaları Redis için önbellekte depolanan veriler vermenize olanak sağlar. Verileri bir Azure Redis önbelleği örneğinden diğerine veya başka bir Redis sunucuya taşımak için bu özelliği kullanın. Dışa aktarma işlemi sırasında geçici bir dosya Azure Redis önbelleği sunucu örneğini barındıran VM oluşturulur ve dosya belirtilen depolama hesabına yüklenir. Ya da durumunu başarı veya hata ile dışarı aktarma işlemi tamamlandıktan sonra geçici dosya silindi.

1. Depolama birimine önbelleğinin geçerli içeriğini dışarı aktarmak için [Gözat önbelleğiniz için](cache-configure.md#configure-redis-cache-settings) tıklatın ve Azure Portalı'ndaki **dışarı veri** gelen **kaynak menü**.

    ![Depolama kapsayıcısı seçin][cache-export-data-choose-storage-container]
2. Tıklatın **depolama kapsayıcısı seçin** ve istenen depolama hesabını seçin. Depolama hesabı aynı abonelikte ve bölgede olarak önbelleğinizin olması gerekir.

   > [!IMPORTANT]
   > Klasik ve Resource Manager depolama hesapları tarafından desteklenir, ancak tarafından desteklenmeyen sayfa bloblarını çalışır verme [Blob storage hesapları](../storage/common/storage-account-options.md#blob-storage-accounts) şu anda.
   >
   >

    ![Depolama hesabı][cache-export-data-choose-account]
3. İstenen blob kapsayıcısı seçin ve tıklatın **seçin**. Yeni bir kapsayıcı kullanmak için tıklatın **kapsayıcı ekleme** önce ekleyin ve ardından listeden seçin.

    ![Depolama kapsayıcısı seçin][cache-export-data-container]
4. Tür a **Blob adı ön ekini** tıklatıp **verme** dışarı aktarma işlemini başlatmak için. Blob adı ön eki Bu dışarı aktarma işlemi tarafından oluşturulan dosyaların adlarını öneki için kullanılır.

    ![Dışarı Aktarma][cache-export-data]

    Azure portalından bildirimleri izleyerek veya olayları görüntüleyerek verme işleminin ilerleme durumunu izleyebilirsiniz [denetim günlüğü](../azure-resource-manager/resource-group-audit.md).

    ![Tam Veri dışarı aktarma][cache-export-data-export-complete]

    Önbellekleri dışa aktarma işlemi sırasında kullanılabilir kalır.

## <a name="importexport-faq"></a>İçeri/dışarı aktarma ile ilgili SSS
Bu bölüm içeri/dışarı aktarma özelliği hakkında sık sorulan sorular içerir.

* [Hangi fiyatlandırma katmanlarını içeri/dışarı aktarma kullanabilir miyim?](#what-pricing-tiers-can-use-importexport)
* [Herhangi bir Redis sunucudan verileri alabilir miyim?](#can-i-import-data-from-any-redis-server)
* [Hangi RDB sürümleri alabilirim?](#what-rdb-versions-can-i-import)
* [My önbellek içeri/dışarı aktarma işlemi sırasında var mı?](#is-my-cache-available-during-an-importexport-operation)
* [İçeri/dışarı aktarma ile Redis kümesi kullanabilir miyim?](#can-i-use-importexport-with-redis-cluster)
* [İçeri/dışarı aktarma ayarı için özel bir veritabanlarını nasıl çalışır?](#how-does-importexport-work-with-a-custom-databases-setting)
* [İçeri/dışarı aktarma Redis kalıcılığı farklı mı?](#how-is-importexport-different-from-redis-persistence)
* [İçeri/dışarı aktarma PowerShell'i, CLI veya diğer yönetim istemcileri kullanarak otomatik hale getirebilirsiniz?](#can-i-automate-importexport-using-powershell-cli-or-other-management-clients)
* [My içeri/dışarı aktarma işlemi sırasında zaman aşımı hatası aldım. Ne anlama geliyor?](#i-received-a-timeout-error-during-my-importexport-operation-what-does-it-mean)
* [Verilerimi Azure Blob depolama alanına dışarı aktarılırken bir hata ile aldım. Ne oldu?](#i-got-an-error-when-exporting-my-data-to-azure-blob-storage-what-happened)

### <a name="what-pricing-tiers-can-use-importexport"></a>Hangi fiyatlandırma katmanlarını içeri/dışarı aktarma kullanabilir miyim?
İçeri/dışarı aktarma premium fiyatlandırma katmanı yalnızca içinde kullanılabilir.

### <a name="can-i-import-data-from-any-redis-server"></a>Herhangi bir Redis sunucudan verileri alabilir miyim?
Evet, Azure Redis önbelleği örnekleri üzerinden aktarılan verilerin içe aktarılması yanı sıra Windows, herhangi bir bulut ya da Linux gibi ortam çalıştıran herhangi bir Redis sunucusundan RDB dosyalarını içeri aktarmak veya Amazon Web Hizmetleri gibi sağlayıcıları bulut. Bunu yapmak için bir Azure depolama hesabındaki sayfa veya blok blob içine istenen Redis sunucusundan RDB dosyayı karşıya yüklemeyi ve premium Azure Redis önbelleği örneğine içeri aktarın. Örneğin, üretim önbellekten veri verin ve test etme veya geçiş için hazırlama ortamının bir parçası kullanılan bir önbelleğe almak isteyebilirsiniz.

> [!IMPORTANT]
> Başarıyla bir sayfa blob'u kullanırken, Azure Redis önbelleği dışında Redis sunucularından dışarı veri almak için sayfa blob boyutu 512 baytlık sınırında hizalanmalıdır. Tüm gerekli bayt doldurma gerçekleştirmek örnek kod için bkz: [örnek sayfa blog yüklemesi](https://github.com/JimRoberts-MS/SamplePageBlobUpload).
> 
> 

### <a name="what-rdb-versions-can-i-import"></a>Hangi RDB sürümleri alabilirim?

Azure Redis önbelleği RDB alma sürüm 7 RDB yukarı destekler.

### <a name="is-my-cache-available-during-an-importexport-operation"></a>My önbellek içeri/dışarı aktarma işlemi sırasında var mı?
* **Dışarı aktarma** - önbellekleri kullanılabilir olmaya devam eder ve bir dışa aktarma işlemi sırasında önbelleğiniz kullanmaya devam edebilirsiniz.
* **Alma** - içe aktarma işlemi başladığında, kullanılamaz hale ve içeri aktarma işlemi tamamlandıktan sonra kullanılabilir hale önbellekleri.

### <a name="can-i-use-importexport-with-redis-cluster"></a>İçeri/dışarı aktarma ile Redis kümesi kullanabilir miyim?
Evet, ve, alma/kümelenmemiş bir önbellek ve kümelenmiş bir önbellek arasında verme. Redis kümesi itibaren [destekler 0 veritabanı yalnızca](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering), herhangi bir veri 0 dışında veritabanlarında içe değil. Kümelenmiş önbellek verilerini içe aktarıldığında, anahtarları küme parça arasında dağıtılır.

### <a name="how-does-importexport-work-with-a-custom-databases-setting"></a>İçeri/dışarı aktarma ayarı için özel bir veritabanlarını nasıl çalışır?
Bazı fiyatlandırma katmanları farklı sahip [veritabanları sınırları](cache-configure.md#databases), böylece bazı noktalar için özel bir değer yapılandırdıysanız alırken `databases` önbellek oluşturma sırasında ayarlama.

* Bir fiyatlandırma katmanı ile bir alt içeri aktarırken `databases` içinden dışa aktardığınız katmanı sınırından:
  * Varsayılan sayısını kullanıyorsanız `databases`, tüm fiyatlandırma katmanlarına 16 olduğu, veri kaybı olmamasına.
  * Özel bir dizi kullanıyorsanız `databases` sınırları içinde bu düştüğünde, içeri aktardığınız, katmanı için hiçbir veriler kaybolur.
  * Verilen verilerinizi yeni katmanı sınırlarını aşıyor bir veritabanında veri içeriyorsa, bu daha yüksek veritabanlarındaki verileri içe aktarılmaz.

### <a name="how-is-importexport-different-from-redis-persistence"></a>İçeri/dışarı aktarma Redis kalıcılığı farklı mı?
Azure Redis önbelleği Kalıcılık Azure depolama birimine Redis depolanan verilere kalıcı olanak tanır. Kalıcılık yapılandırıldığında, Azure Redis önbelleği Redis önbelleği Redis ikili biçimde yapılandırılabilir bir yedekleme sıklığına bağlı disk için anlık görüntü devam ettirir. Hem birincil hem de çoğaltma önbelleği devre dışı bırakan geri dönülemez bir olay meydana gelirse, önbellek verilerini otomatik olarak en son anlık görüntü kullanılarak geri yüklendi. Daha fazla bilgi için bkz: [Premium Azure Redis önbelleği için veri kalıcılığını yapılandırma](cache-how-to-premium-persistence.md).

Alma / verme verisine Getir veya Azure Redis Önbelleği'nden verme olanak tanır. Yedekleme yapılandırması yok ve Redis kalıcılığı kullanarak geri yükleyin.

### <a name="can-i-automate-importexport-using-powershell-cli-or-other-management-clients"></a>İçeri/dışarı aktarma PowerShell'i, CLI veya diğer yönetim istemcileri kullanarak otomatik hale getirebilirsiniz?
Evet, yönergeler için PowerShell bkz [Redis önbelleği almak için](cache-howto-manage-redis-cache-powershell.md#to-import-a-redis-cache) ve [Redis önbelleği dışarı aktarmak için](cache-howto-manage-redis-cache-powershell.md#to-export-a-redis-cache).

### <a name="i-received-a-timeout-error-during-my-importexport-operation-what-does-it-mean"></a>My içeri/dışarı aktarma işlemi sırasında zaman aşımı hatası aldım. Ne anlama geliyor?
Üzerinde kalırsa **veri içeri aktarma** veya **dışarı veri** dikey 15'ten daha uzun dakika işlem başlatmadan önce aşağıdaki örneğe benzer bir hata iletisi ile bir hata alıyorsunuz:

    The request to import data into cache 'contoso55' failed with status 'error' and error 'One of the SAS URIs provided could not be used for the following reason: The SAS token end time (se) must be at least 1 hour from now and the start time (st), if given, must be at least 15 minutes in the past.

Bunu çözmek için içe aktarmayı başlatmak veya dışarı aktarma işlemi 15 dakika önce geçti.

### <a name="i-got-an-error-when-exporting-my-data-to-azure-blob-storage-what-happened"></a>Verilerimi Azure Blob depolama alanına dışarı aktarılırken bir hata ile aldım. Ne oldu?
Sayfa blobları depolanan dosyalarla yalnızca RDB verme çalışır. Diğer blob türleri şu anda, blob storage hesapları sık erişimli ve seyrek katmanları dahil olmak üzere desteklenmez. Daha fazla bilgi için bkz: [Blob storage hesapları](../storage/common/storage-account-options.md#blob-storage-accounts).

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla premium önbellek özelliklerinin nasıl kullanılacağını öğrenin.

* [Azure Redis önbelleği Premium katmanına giriş](cache-premium-tier-intro.md)    

<!-- IMAGES -->
[cache-settings-import-export-menu]: ./media/cache-how-to-import-export-data/cache-settings-import-export-menu.png
[cache-export-data-choose-account]: ./media/cache-how-to-import-export-data/cache-export-data-choose-account.png
[cache-export-data-choose-storage-container]: ./media/cache-how-to-import-export-data/cache-export-data-choose-storage-container.png
[cache-export-data-container]: ./media/cache-how-to-import-export-data/cache-export-data-container.png
[cache-export-data-export-complete]: ./media/cache-how-to-import-export-data/cache-export-data-export-complete.png
[cache-export-data]: ./media/cache-how-to-import-export-data/cache-export-data.png
[cache-import-data]: ./media/cache-how-to-import-export-data/cache-import-data.png
[cache-import-choose-storage-account]: ./media/cache-how-to-import-export-data/cache-import-choose-storage-account.png
[cache-import-choose-container]: ./media/cache-how-to-import-export-data/cache-import-choose-container.png
[cache-import-choose-blobs]: ./media/cache-how-to-import-export-data/cache-import-choose-blobs.png
[cache-import-blobs]: ./media/cache-how-to-import-export-data/cache-import-blobs.png
[cache-import-data-import-complete]: ./media/cache-how-to-import-export-data/cache-import-data-import-complete.png
