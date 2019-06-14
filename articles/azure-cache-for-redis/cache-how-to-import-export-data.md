---
title: İçeri ve dışarı aktarma verileri Azure önbelleği için Redis | Microsoft Docs
description: Ve, ' % s'premium Azure önbelleği için Redis örneği ile blob depolama alanından verileri dışarı ve içeri aktarma hakkında bilgi edinin
services: cache
documentationcenter: ''
author: yegu-ms
manager: jhubbard
editor: ''
ms.assetid: 4a68ac38-87af-4075-adab-569d37d7cc9e
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: yegu
ms.openlocfilehash: dfa8b47ced70386efa1daa44af318f1da55f49e1
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60542341"
---
# <a name="import-and-export-data-in-azure-cache-for-redis"></a>İçeri ve dışarı aktarma verileri Azure önbelleği için Redis
İçeri/dışarı aktarma olan Azure önbelleği için Redis içine veri aktarmak veya Azure önbelleği için Redis içeri ve dışarı aktarma bir Azure önbelleği için Redis veritabanı (RDB) anlık görüntü için bir premium önbelleğinden tarafından verileri dışarı aktarma olanak tanıyan Redis veri yönetimi işlemi için bir Azure önbelleği bir bir Azure depolama hesabında blob. 

- **Dışarı aktarma** -sayfa blobu, Azure önbelleği için Redis RDB anlık görüntülerin dışarı aktarabilirsiniz.
- **İçeri aktarma** -sayfa blobu veya blok blobu, Azure önbelleği için Redis RDB anlık görüntülerini alabilirsiniz.

İçeri/dışarı aktarma, farklı Azure önbelleği için Redis örneği arasında geçirmek veya önbellek kullanılmadan önce veri ile doldurmak sağlar.

Bu makale, içeri ve verilerle Azure önbelleği için Redis dışarı aktarma için bir kılavuz sağlar ve sık sorulan soruların yanıtlarını sağlar.

> [!IMPORTANT]
> İçeri/dışarı aktarma Önizleme aşamasındadır ve yalnızca [premium katmanı](cache-premium-tier-intro.md) önbelleğe alır.
>
>

## <a name="import"></a>İçeri Aktarma
İçeri aktarma, herhangi bir bulut veya ortam, Linux, Windows ya da herhangi bir bulut sağlayıcısı Amazon Web Hizmetleri ve diğerleri gibi çalışan bir Redis dahil olmak üzere çalışan herhangi bir Redis sunucudan Redis uyumlu RDB dosyaları getirmek için kullanılabilir. Verileri içeri aktarma ile önceden doldurulmuş veri önbellek oluşturmak için kolay bir yoludur. İçeri aktarma işlemi sırasında Azure önbelleği için Redis RDB dosyaları Azure Storage'dan belleğine yükler ve ardından anahtarları önbelleğe ekler.

> [!NOTE]
> İçeri aktarma işlemi başlamadan önce Redis veritabanı (RDB) dosyanız veya dosyalarınız sayfa veya blok blobları Azure depolama, aynı bölge ve abonelik, Azure önbelleği için Redis örneği olarak içine yüklenir emin olun. Daha fazla bilgi için [Azure Blob depolamayı kullanmaya başlama](../storage/blobs/storage-dotnet-how-to-use-blobs.md). RDB dosya kullanarak dışarı aktardıysanız [Azure önbelleği için Redis dışarı aktarma](#export) özelliği RDB dosyanız zaten bir sayfa blobu depolanır ve içeri aktarma için hazır.
>
>

1. Dışarı aktarılan önbellek blobları, bir veya daha fazla içeri aktarmak için [Gözat önbelleğinize](cache-configure.md#configure-azure-cache-for-redis-settings) tıklayıp Azure portalını **verileri içeri aktarma** gelen **kaynak menüsünde**.

    ![Veri içeri aktarma][cache-import-data]
2. Tıklayın **seçin inputblobpath** ve verilerin içe aktarılacağı içeren depolama hesabını seçin.

    ![Depolama hesabı seçin][cache-import-choose-storage-account]
3. Alınacak verileri içeren kapsayıcı tıklayın.

    ![Kapsayıcı Seç][cache-import-choose-container]
4. Bir veya daha fazla BLOB'lar, blob adının sol tarafındaki alana tıklayarak içeri aktarmak için seçin ve ardından **seçin**.

    ![BLOB'ları seçin][cache-import-choose-blobs]
5. Tıklayın **alma** içeri aktarma işlemini başlatmak için.

   > [!IMPORTANT]
   > Önbelleğe alma işlemi sırasında önbellek istemcileri tarafından erişilebilir değil ve önbellek tüm mevcut veriler silinir.
   >
   >

    ![İçeri Aktarma][cache-import-blobs]

    Azure portalından bildirimleri izleyerek veya olayları görüntüleyerek içeri aktarma işleminin ilerleme durumunu izleyebilirsiniz [denetim günlüğü](../azure-resource-manager/resource-group-audit.md).

    ![İçeri aktarma ilerlemesi][cache-import-data-import-complete]

## <a name="export"></a>Dışarı Aktarma
Dışarı aktarma, Redis için Redis uyumlu RDB dosyaları Azure önbelleğinde depolanan verileri dışarı aktarma olanak tanır. Bu özellik, verileri bir Azure önbelleği için Redis örneğinden diğerine veya başka bir Redis sunucusuna taşıma için kullanabilirsiniz. Dışarı aktarma işlemi sırasında Azure önbelleği için Redis sunucusu örneğini barındıran sanal makine geçici bir dosya oluşturulur ve dosyanın belirtilen depolama hesabına yüklenir. Ya da bir durum başarı veya hata ile dışarı aktarma işlemi tamamlandığında, geçici dosya silinir.

1. Geçerli önbelleğinin içeriğini depolama için dışarı aktarmak için [Gözat önbelleğinize](cache-configure.md#configure-azure-cache-for-redis-settings) tıklayıp Azure portalını **verileri dışarı aktar** gelen **kaynak menüsünde**.

    ![Depolama kapsayıcısı seçin][cache-export-data-choose-storage-container]
2. Tıklayın **depolama kapsayıcısı seçin** ve istenen depolama hesabını seçin. Depolama hesabı olarak önbelleğinizin aynı abonelik ve aynı bölgede olması gerekir.

   > [!IMPORTANT]
   > Dışarı aktarma, hem Klasik hem de Resource Manager depolama hesapları tarafından desteklenir, ancak Blob Depolama hesapları tarafından şu anda desteklenmeyen sayfa bloblarını ile çalışır. Daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](../storage/common/storage-account-overview.md).
   >
   >

    ![Depolama hesabı][cache-export-data-choose-account]
3. İstenen blob kapsayıcısını seçin ve tıklayın **seçin**. Yeni bir kapsayıcı kullanmak için **Ekle kapsayıcı** ilk ekleyin ve ardından listeden seçin.

    ![Depolama kapsayıcısı seçin][cache-export-data-container]
4. Tür a **Blob adı ön eki** tıklatıp **dışarı** dışarı aktarma işlemini başlatmak için. Blob adı ön eki, bu dışarı aktarma işlemi tarafından oluşturulan dosya adını önek olarak eklemek için kullanılır.

    ![Dışarı Aktarma][cache-export-data]

    Azure portalından bildirimleri izleyerek veya olayları görüntüleyerek dışarı aktarma işleminin ilerleme durumunu izleyebilirsiniz [denetim günlüğü](../azure-resource-manager/resource-group-audit.md).

    ![Verileri dışarı aktarma tamamlandı][cache-export-data-export-complete]

    Önbellekler dışarı aktarma işlemi sırasında kullanılabilir kalır.

## <a name="importexport-faq"></a>İçeri/dışarı aktarma ile ilgili SSS
Bu bölüm, içeri/dışarı aktarma özelliği hakkında sık sorulan sorular içerir.

* [Fiyatlandırma katmanları içeri/dışarı aktarma kullanabilir miyim?](#what-pricing-tiers-can-use-importexport)
* [Herhangi bir Redis sunucusundan verileri alabilir miyim?](#can-i-import-data-from-any-redis-server)
* [Hangi RDB sürümleri alabilirim?](#what-rdb-versions-can-i-import)
* [Önbelleğimin bir içeri/dışarı aktarma işlemi sırasında kullanılabilir mi?](#is-my-cache-available-during-an-importexport-operation)
* [İçeri/dışarı aktarma Redis kümesi ile kullanabilir miyim?](#can-i-use-importexport-with-redis-cluster)
* [İçeri/dışarı aktarma ayarlamak için özel bir veritabanlarını nasıl çalışır?](#how-does-importexport-work-with-a-custom-databases-setting)
* [İçeri/dışarı aktarma Redis kalıcılığı farklı mı?](#how-is-importexport-different-from-redis-persistence)
* [İçeri/dışarı aktarma PowerShell, CLI veya diğer yönetim istemcilerini kullanarak otomatik hale getirebilirim?](#can-i-automate-importexport-using-powershell-cli-or-other-management-clients)
* [My içeri/dışarı aktarma işlemi sırasında zaman aşımı hatası aldım. Bu ne demektir?](#i-received-a-timeout-error-during-my-importexport-operation-what-does-it-mean)
* [Azure Blob depolama alanına verilerimi dışarı aktarılırken bir hata aldım. Ne oldu?](#i-got-an-error-when-exporting-my-data-to-azure-blob-storage-what-happened)

### <a name="what-pricing-tiers-can-use-importexport"></a>Fiyatlandırma katmanları içeri/dışarı aktarma kullanabilir miyim?
İçeri/dışarı aktarma fiyatlandırma katmanı premium katmanında kullanılabilir.

### <a name="can-i-import-data-from-any-redis-server"></a>Herhangi bir Redis sunucusundan verileri alabilir miyim?
Evet, Azure önbelleği için Redis örneği dışarı aktarılan verileri içeri aktarma yanı sıra Windows, herhangi bir bulut veya gibi Linux ortamında çalışan herhangi bir Redis sunucudan RDB dosyaları alabilir veya sağlayıcıları Amazon Web Hizmetleri gibi bulut. Bunu yapmak için istenen Redis sunucusunu RDB dosyasından bir sayfa veya blok blobu bir Azure depolama hesabına dosya yükleme ve, ' % s'premium Azure önbelleği için Redis örneği aktarın. Örneğin, üretim Önbelleği'ndeki verileri dışarı aktarma ve hazırlama ortamına bir parçası olarak test veya geçiş için kullanılan bir önbelleğe almak isteyebilirsiniz.

> [!IMPORTANT]
> Başarıyla dışında Azure Cache, Redis sunucularından Redis için bir sayfa blobu kullanırken, dışarı aktarılan verileri içeri aktarmak için sayfa blob boyutu 512 baytlık sınırında hizalanması gerekir. Tüm gerekli bayt doldurma gerçekleştirmek için örnek kod için bkz: [örnek sayfa blobu karşıya yükleme](https://github.com/JimRoberts-MS/SamplePageBlobUpload).
> 
> 

### <a name="what-rdb-versions-can-i-import"></a>Hangi RDB sürümleri alabilirim?

Azure önbelleği için Redis RDB içeri aktarma sürüm 7 RDB yukarı destekler.

### <a name="is-my-cache-available-during-an-importexport-operation"></a>Önbelleğimin bir içeri/dışarı aktarma işlemi sırasında kullanılabilir mi?
* **Dışarı aktarma** - önbellekler kullanılabilir olmaya devam eder ve bir dışa aktarma işlemi sırasında önbelleğinizi kullanmaya devam edebilirsiniz.
* **İçeri aktarma** - önbellekler içeri aktarma işlemi başladığında kullanılamaz hale ve içeri aktarma işlemi tamamlandığında kullanılabilir hale gelir.

### <a name="can-i-use-importexport-with-redis-cluster"></a>İçeri/dışarı aktarma Redis kümesi ile kullanabilir miyim?
Evet, ve, içeri aktarma/kümelenmiş bir önbellek ve kümelenmiş olmayan önbellek arasında dışarı aktarma. Redis kümesi beri [destekler 0 veritabanı yalnızca](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering), herhangi bir veri 0 dışında veritabanlarında aktarılmamış. Kümelenmiş önbellek veriler içeri aktarıldığında, anahtarları kümenin parçalar arasında dağıtılır.

### <a name="how-does-importexport-work-with-a-custom-databases-setting"></a>İçeri/dışarı aktarma ayarlamak için özel bir veritabanlarını nasıl çalışır?
Bazı fiyatlandırma katmanları farklı sahip [veritabanları sınırları](cache-configure.md#databases), böylece bazı önemli noktalar için özel bir değer yapılandırdıysanız içeri aktarılırken `databases` önbellek oluşturma işlemi sırasında ayarlama.

* Bir fiyatlandırma katmanına düşük ile içeri aktarılırken `databases` içinden dışarı aktardığınız katmanından sınırı:
  * Varsayılan sayısını kullanıyorsanız `databases`16 tüm fiyatlandırma katmanlarına yönelik olan, veri kaybedilmez.
  * Özel bir dizi kullanıyorsanız `databases` sınırları içinde bu düştüğünde, içeri aktardığınız, katman için veri kaybedilmez.
  * Dışarı aktarılan verilerinizi yeni katmanı sınırlarını aşan bir veritabanında veri içeriyorsa, daha yüksek veritabanlarınızdaki veriler içe aktarılmaz.

### <a name="how-is-importexport-different-from-redis-persistence"></a>İçeri/dışarı aktarma Redis kalıcılığı farklı mı?
Azure önbelleği için Redis kalıcılığı, Redis Azure Depolama'da depolanan verileri kalıcı hale getirmenize olanak tanır. Kalıcılık yapılandırıldığında, Azure önbelleği için Redis anlık görüntüsünü Azure önbelleği için Redis diske yapılandırılabilir bir yedekleme sıklığı temel alarak bir Redis ikili biçimde devam ettirir. Birincil ve çoğaltma önbellek devre dışı bırakan bir felaket ortaya çıkarsa, önbellek verilerini otomatik olarak en son anlık görüntü kullanılarak geri yüklenir. Daha fazla bilgi için [Premium Azure önbelleği için Redis veri kalıcılığı yapılandırma](cache-how-to-premium-persistence.md).

İçeri aktarma / dışarı aktarma verileri getirin ya da Azure önbelleği için Redis dışarı aktarma olanak tanır. Yedeklemeyi yapılandırma değildir ve Redis kalıcılığı kullanarak geri yükleyin.

### <a name="can-i-automate-importexport-using-powershell-cli-or-other-management-clients"></a>İçeri/dışarı aktarma PowerShell, CLI veya diğer yönetim istemcilerini kullanarak otomatik hale getirebilirim?
Evet, PowerShell için yönergeleri görmek [bir Azure önbelleği için Redis içeri aktarmak için](cache-howto-manage-redis-cache-powershell.md#to-import-an-azure-cache-for-redis) ve [bir Azure önbelleği için Redis dışarı aktarmak için](cache-howto-manage-redis-cache-powershell.md#to-export-an-azure-cache-for-redis).

### <a name="i-received-a-timeout-error-during-my-importexport-operation-what-does-it-mean"></a>My içeri/dışarı aktarma işlemi sırasında zaman aşımı hatası aldım. Bu ne demektir?
Üzerinde kalırsa **verileri içeri aktarma** veya **verileri dışarı aktar** dikey 15 daha uzun dakika işlemi başlatmadan önce aşağıdaki örneğe benzer bir hata iletisi ile bir hata alırsınız:

    The request to import data into cache 'contoso55' failed with status 'error' and error 'One of the SAS URIs provided could not be used for the following reason: The SAS token end time (se) must be at least 1 hour from now and the start time (st), if given, must be at least 15 minutes in the past.

Bu sorunu çözmek için alma işlemi başlatmak veya dışarı aktarma işlemi 15 dakika önce geçti.

### <a name="i-got-an-error-when-exporting-my-data-to-azure-blob-storage-what-happened"></a>Azure Blob depolama alanına verilerimi dışarı aktarılırken bir hata aldım. Ne oldu?
Dışarı aktarma, sayfa blobu olarak depolanan RDB dosyaları ile birlikte çalışır. Diğer blob türleri şu anda, sık ve seyrek erişimli katmanlardaki Blob Depolama hesapları dahil olmak üzere desteklenmez. Daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](../storage/common/storage-account-overview.md).

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla premium önbellek özelliklerini kullanmayı öğrenin.

* [Azure önbelleği için Redis Premium katmanına giriş](cache-premium-tier-intro.md)    

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
