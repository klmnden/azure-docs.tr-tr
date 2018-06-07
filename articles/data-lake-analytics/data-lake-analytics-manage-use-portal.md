---
title: Azure portalı kullanarak Azure Data Lake Analytics'i yönetme
description: Bu makalede, Data Lake Analytics hesaplarını, veri kaynakları, kullanıcılar ve işleri yönetmek için Azure Portalı'nı kullanmayı açıklar.
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
manager: kfile
editor: jasonwhowell
ms.assetid: a0e045f1-73d6-427f-868d-7b55c10f811b
ms.topic: conceptual
ms.date: 12/05/2016
ms.openlocfilehash: 1ccd4dd6b8d4ee15b7d9f14e7436ccd87392121e
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34623715"
---
# <a name="manage-azure-data-lake-analytics-using-the-azure-portal"></a>Azure Data Lake Analytics'i Azure Portalı'nı kullanarak yönetme
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Bu makalede, Azure portalını kullanarak Azure Data Lake Analytics hesaplarını, veri kaynakları, kullanıcılar ve işleri Yönet açıklar.


<!-- ################################ -->
<!-- ################################ -->

## <a name="manage-data-lake-analytics-accounts"></a>Data Lake Analytics hesaplarını yönetme

### <a name="create-an-account"></a>Hesap oluşturma

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Tıklatın **kaynak oluşturma** > **Intelligence + analiz** > **Data Lake Analytics**.
3. Aşağıdaki öğeler için değerleri seçin: 
   1. **Ad**: Data Lake Analytics hesabının adı.
   2. **Abonelik**: hesap için kullanılan Azure aboneliği.
   3. **Kaynak grubu**: hesap oluşturmak üzere Azure kaynak grubu. 
   4. **Konum**: Data Lake Analytics hesabı için Azure veri merkezi. 
   5. **Data Lake Store**: Data Lake Analytics hesabı için kullanılacak varsayılan deposu. Azure Data Lake Store hesabı ve Data Lake Analytics hesabı aynı konumda olması gerekir.
4. **Oluştur**’a tıklayın. 

### <a name="delete-a-data-lake-analytics-account"></a>Bir Data Lake Analytics hesabı silme

Bir Data Lake Analytics hesabı silmeden önce varsayılan Data Lake Store hesabını silin.

1. Azure Portal'da Data Lake Analytics hesabınıza gidin.
2. **Sil**'e tıklayın.
3. Hesap adını yazın.
4. **Sil**'e tıklayın.

<!-- ################################ -->
<!-- ################################ -->

## <a name="manage-data-sources"></a>Veri kaynaklarını yönet

Data Lake Analytics aşağıdaki veri kaynaklarını destekler:

* Data Lake Store
* Azure Storage

Veri kaynağına Gözat ve temel dosya yönetimi işlemlerini gerçekleştirmek için Veri Gezgini'ni kullanın. 

### <a name="add-a-data-source"></a>Veri kaynağı ekleme

1. Azure Portal'da Data Lake Analytics hesabınıza gidin.
2. Tıklatın **veri kaynakları**.
3. Tıklatın **veri kaynağı ekleme**.
    
   * Bir Data Lake Store hesabı eklemek için hesap adı ve onu sorgulayabilmesi için hesabına erişim izni gerekir.
   * Azure Blob depolama alanına eklemek için depolama hesabı ve hesap anahtarı gerekir. Bunları bulmak için Portalı'nda depolama hesabına gidin.

## <a name="set-up-firewall-rules"></a>Güvenlik duvarı kuralları ayarlamak

Data Lake Analytics hesabınızla ağ düzeyinde daha ayrıntılı erişim kilitleme için Data Lake Analytics kullanabilirsiniz. Bir güvenlik duvarını etkinleştir, bir IP adresi belirtin veya güvenilen istemcileriniz için bir IP adresi aralığı tanımlayın. Bu ölçüleri etkinleştirdikten sonra IP adreslerini tanımlı aralığın içinde olan istemciler deposuna bağlanabilirsiniz.

Azure Data Factory veya VM gibi diğer Azure hizmetleriyle Data Lake Analytics hesabına bağlanırsanız, emin olun **izin Azure Hizmetleri** açık **üzerinde**. 

### <a name="set-up-a-firewall-rule"></a>Güvenlik duvarı kuralı ayarlama

1. Azure Portal'da Data Lake Analytics hesabınıza gidin.
2. Sol taraftaki menüsünde **Güvenlik Duvarı**.

## <a name="add-a-new-user"></a>Yeni kullanıcı ekleme

Kullanabileceğiniz **Kullanıcı Ekleme Sihirbazı** kolayca yeni Data Lake kullanıcılar sağlamak için.

1. Azure Portal'da Data Lake Analytics hesabınıza gidin.
2. Solda, altında **Başlarken**, tıklatın **Kullanıcı Ekleme Sihirbazı**.
3. Bir kullanıcı seçin ve ardından **seçin**.
4. Bir rol seçin ve ardından **seçin**. Azure Data Lake kullanmak üzere yeni bir geliştirici ayarlamak için seçin **Data Lake Analytics Geliştirici** rol.
5. U-SQL veritabanları için erişim denetim listelerini (ACL'ler) seçin. Seçimlerinizi yaptıktan sonra tıklatın **seçin**.
6. Dosyalar için ACL'leri seçin. Varsayılan deposu için kök klasör için ACL'ler değişmez "/" ve/sistem klasörü için. **Seç**'e tıklayın.
7. Tüm seçili değişikliklerinizi gözden geçirin ve ardından **çalıştırmak**.
8. Sihirbaz sona erdiğinde tıklatın **Bitti**.

## <a name="manage-role-based-access-control"></a>Rol tabanlı erişim denetimini yönetmek

Diğer Azure hizmetlerinde olduğu gibi kullanıcıların hizmetle nasıl etkileşim denetlemek için rol tabanlı erişim denetimi (RBAC) kullanabilirsiniz.

Standart RBAC rolleri aşağıdaki özelliklere sahiptir:
* **Sahibi**: iş göndermek, izleyebilir işleri, herhangi bir kullanıcı işlerini iptal edin ve hesabı yapılandırın.
* **Katkıda bulunan**: iş göndermek, izleyebilir işleri, herhangi bir kullanıcı işlerini iptal edin ve hesabı yapılandırın.
* **Okuyucu**: işleri izleyebilirsiniz.

U-SQL geliştiriciler Data Lake Analytics hizmetini kullanmak üzere etkinleştirmek için Data Lake Analytics Geliştirici rol kullanın. Data Lake Analytics Geliştirici rolüne kullanabilirsiniz:
* İşlerini gönderin.
* İş durumu ve herhangi bir kullanıcı tarafından gönderilen işlerin ilerleme durumunu izleyin.
* U-SQL betikleri herhangi bir kullanıcı tarafından gönderilen işlerine bakın.
* Yalnızca kendi işlerini iptal edin.

### <a name="add-users-or-security-groups-to-a-data-lake-analytics-account"></a>Kullanıcıların veya güvenlik gruplarının bir Data Lake Analytics hesabı Ekle

1. Azure Portal'da Data Lake Analytics hesabınıza gidin.
2. Tıklatın **erişim denetimi (IAM)** > **Ekle**.
3. Bir rol seçin.
4. Bir kullanıcı ekleyin.
5. **Tamam**’a tıklayın.

>[!NOTE]
>İşlerini göndermek bir kullanıcı veya güvenlik grubu gerekirse, bunlar ayrıca depolama hesabındaki izni gerekir. Daha fazla bilgi için bkz: [güvenli Data Lake Store içinde depolanan verileri](../data-lake-store/data-lake-store-secure-data.md).
>

<!-- ################################ -->
<!-- ################################ -->

## <a name="manage-jobs"></a>İşleri yönetme

### <a name="submit-a-job"></a>Bir işi gönderin

1. Azure Portal'da Data Lake Analytics hesabınıza gidin.

2. Tıklatın **yeni iş**. Her bir iş yapılandırın:

    1. **İş adı**: İş adı.
    2. **Öncelik**: alt numaraları yüksek önceliği vardır. İki işi sıraya alınmışsa, düşük öncelik değerine sahip ilk çalışır.
    3. **Paralellik**: Bu proje için ayrılan işlem işlemlerin sayısı.

3. **İşi Gönder**'e tıklayın.

### <a name="monitor-jobs"></a>İşleri izleme

1. Azure Portal'da Data Lake Analytics hesabınıza gidin.
2. Tıklatın **tüm işleri görüntüleyin**. Hesaptaki tüm etkin ve en son bitmiş işlerin bir listesini gösterilir.
3. İsteğe bağlı olarak, tıklayın **filtre** işleri tarafından bulmanıza yardımcı olmak için **zaman aralığı**, **iş adı**, ve **Yazar** değerleri. 

### <a name="monitoring-pipeline-jobs"></a>Ardışık Düzen işlerini izleme
Bir işlem hattı parçası olan birlikte, genellikle bir sırayla, belirli bir senaryoyu gerçekleştirmek için işler. Örneğin, temizler, ayıklar, dönüşümleri, müşteri Öngörüler için kullanım toplayan bir ardışık düzen olabilir. Ardışık Düzen işleri, iş gönderildiğinde "Ardışık düzen" özelliği kullanılarak tanımlanır. ADF V2 kullanılarak zamanlanan işleri otomatik olarak doldurulmuş bu özelliğe sahip. 

Ardışık Düzen parçası olan U-SQL işlerin bir listesini görüntülemek için: 

1. Azure Portal'da Data Lake Analytics hesaplarına gidin.
2. Tıklatın **iş Öngörüler**. "Tüm işleri" sekmesini olacak varsayılan, çalışan, listesini gösteren sıraya ve işleri sona erdi.
3. Tıklatın **ardışık düzen işleri** sekmesi. Ardışık Düzen işlerin bir listesini her ardışık düzeni için toplu istatistikler birlikte gösterilir.

### <a name="monitoring-recurring-jobs"></a>Yinelenen işlerini izleme
Yinelenen bir işi aynı iş mantığı vardır, ancak her çalıştığında, farklı giriş verilerini kullanan biridir. İdeal olarak, yinelenen işleri her zaman başarılı ve göreceli olarak tutarlı yürütme süresi vardır; Bu davranışların izleme iş sağlıklı olduğundan emin olun yardımcı olur. Yinelenen işleri "Recurrence" özelliği kullanılarak tanımlanır. ADF V2 kullanılarak zamanlanan işleri otomatik olarak doldurulmuş bu özelliğe sahip.

Yinelenen U-SQL işlerin bir listesini görüntülemek için: 

1. Azure Portal'da Data Lake Analytics hesaplarına gidin.
2. Tıklatın **iş Öngörüler**. "Tüm işleri" sekmesini olacak varsayılan, çalışan, listesini gösteren sıraya ve işleri sona erdi.
3. Tıklatın **yinelenen işleri** sekmesi. Yinelenen işlerin bir listesini, yinelenen her iş için toplu istatistikler birlikte gösterilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md)
* [Azure PowerShell kullanarak Azure Data Lake Analytics'i yönetme](data-lake-analytics-manage-use-powershell.md)
* [İlkeleri kullanarak Azure Data Lake Analytics yönetme](https://docs.microsoft.com/en-us/azure/data-lake-analytics/data-lake-analytics-policies)
