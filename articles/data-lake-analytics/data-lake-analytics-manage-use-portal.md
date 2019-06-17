---
title: Azure portalını kullanarak Azure Data Lake Analytics'i yönetme
description: Bu makalede, Data Lake Analytics hesaplarını, veri kaynakları, kullanıcılar ve işlerini yönetmek için Azure portalını kullanmayı açıklar.
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: a0e045f1-73d6-427f-868d-7b55c10f811b
ms.topic: conceptual
ms.date: 12/05/2016
ms.openlocfilehash: 8b2f16f45be1d095e9be8042611de328af36f064
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60813443"
---
# <a name="manage-azure-data-lake-analytics-using-the-azure-portal"></a>Azure portalını kullanarak Azure Data Lake Analytics'i yönetme
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Bu makalede, Azure portalını kullanarak Azure Data Lake Analytics hesaplarını, veri kaynakları, kullanıcılar ve işlerini yönetmek açıklar.


<!-- ################################ -->
<!-- ################################ -->

## <a name="manage-data-lake-analytics-accounts"></a>Data Lake Analytics hesaplarını yönetme

### <a name="create-an-account"></a>Hesap oluşturma

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Tıklayın **kaynak Oluştur** > **zeka + analiz** > **Data Lake Analytics**.
3. Aşağıdaki öğeler için değerleri seçin: 
   1. **Ad**: Data Lake Analytics hesap adı.
   2. **Abonelik**: Hesabı için kullanılan Azure aboneliği.
   3. **Kaynak grubu**: Hesabın oluşturulacağı Azure kaynak grubu. 
   4. **Konum**: Data Lake Analytics hesabı için Azure veri merkezi. 
   5. **Data Lake Store**: Data Lake Analytics hesabı için kullanılacak varsayılan depolama. Azure Data Lake Store hesabı ve Data Lake Analytics hesabı aynı konumda olmalıdır.
4. **Oluştur**’a tıklayın. 

### <a name="delete-a-data-lake-analytics-account"></a>Bir Data Lake Analytics hesabını Sil

Data Lake Analytics hesabı silmeden önce varsayılan Data Lake Store hesabı silin.

1. Azure portalında Data Lake Analytics hesabınıza gidin.
2. Tıklayın **Sil**.
3. Hesap adını yazın.
4. Tıklayın **Sil**.

<!-- ################################ -->
<!-- ################################ -->

## <a name="manage-data-sources"></a>Veri kaynaklarını yönetme

Data Lake Analytics, aşağıdaki veri kaynaklarını destekler:

* Data Lake Store
* Azure Storage

Veri Gezgini, bir veri kaynağına Gözat ve temel dosyanın yönetim işlemleri gerçekleştirmek için kullanabilirsiniz. 

### <a name="add-a-data-source"></a>Veri Kaynağı Ekle

1. Azure portalında Data Lake Analytics hesabınıza gidin.
2. Tıklayın **veri kaynakları**.
3. Tıklayın **veri kaynağı ekleme**.
    
   * Bir Data Lake Store hesabı eklemek için hesap adını ve bu sorgu için hesabına erişimi gerekir.
   * Azure Blob Depolama alanı eklemek için depolama hesabı ve hesap anahtarı gerekir. Bunları bulmak için Portalı'nda depolama hesabına gidin.

## <a name="set-up-firewall-rules"></a>Güvenlik duvarı kurallarını ayarlayın

Data Lake Analytics ağ düzeyinde Data Lake Analytics hesabınıza erişim başka kilitleme için kullanabilirsiniz. Bir Güvenlik Duvarı'nı etkinleştirme, bir IP adresi belirtin veya güvenilen istemcileriniz için bir IP adresi aralığı tanımlayın. Bu ölçüleri etkinleştirdikten sonra yalnızca IP adresleri tanımlı aralığın içinde olan istemciler Store'a bağlanabilirsiniz.

Azure Data Factory veya VM gibi diğer Azure Hizmetleri Data Lake Analytics hesabına bağlanırsanız, emin **izin Azure Hizmetleri** açıktır **üzerinde**. 

### <a name="set-up-a-firewall-rule"></a>Güvenlik duvarı kuralı ayarlama

1. Azure portalında Data Lake Analytics hesabınıza gidin.
2. Soldaki menüsünde **Güvenlik Duvarı**.

## <a name="add-a-new-user"></a>Yeni kullanıcı ekleme

Kullanabileceğiniz **kullanıcı ekleme sihirbazını** kolayca yeni Data Lake kullanıcıları sağlama.

1. Azure portalında Data Lake Analytics hesabınıza gidin.
2. Sol taraftaki altında **Başlarken**, tıklayın **kullanıcı ekleme sihirbazını**.
3. Bir kullanıcı seçin ve ardından **seçin**.
4. Bir rol seçin ve ardından **seçin**. Azure Data Lake kullanmak için yeni bir geliştiricinin ayarlamak için seçin **Data Lake Analytics geliştiricisi** rol.
5. U-SQL veritabanları için erişim denetim listelerini (ACL'ler) seçin. Seçimlerinizi ile memnun kaldığınızda, tıklayın **seçin**.
6. Dosyaları için ACL'leri seçin. Varsayılan depo için kök klasörün ACL'lerini değiştirme "/" ve/sistem klasörü için. **Seç**'e tıklayın.
7. Seçili tüm değişikliklerinizi gözden geçirin ve ardından **çalıştırma**.
8. Sihirbaz tamamlandığında, tıklayın **Bitti**.

## <a name="manage-role-based-access-control"></a>Rol tabanlı erişim denetimini yönetme

Diğer Azure Hizmetleri gibi kullanıcıların hizmetle nasıl etkileşim denetlemek için rol tabanlı erişim denetimi (RBAC) kullanabilirsiniz.

Standart RBAC rolleri aşağıdaki özelliklere sahiptir:
* **Sahibi**: İşleri göndermek, işleri izlemek, herhangi bir kullanıcının işlerini iptal ve hesabı yapılandırın.
* **Katkıda bulunan**: İşleri göndermek, işleri izlemek, herhangi bir kullanıcının işlerini iptal ve hesabı yapılandırın.
* **Okuyucu**: İşleri izleyebilirsiniz.

U-SQL geliştiricilerin Data Lake Analytics hizmeti kullanmak için Data Lake Analytics Geliştirici rolünü kullanın. Data Lake Analytics geliştiricisi rolüne kullanabilirsiniz:
* İşleri göndermek.
* İş durumunu ve herhangi bir kullanıcı tarafından gönderilen bir iş ilerlemesini izleyin.
* U-SQL betikleri herhangi bir kullanıcı tarafından gönderilen bir iş öğesinden bakın.
* Yalnızca kendi işlerini iptal edin.

### <a name="add-users-or-security-groups-to-a-data-lake-analytics-account"></a>Bir Data Lake Analytics hesabı için kullanıcıların veya güvenlik grupları Ekle

1. Azure portalında Data Lake Analytics hesabınıza gidin.
2. Tıklayın **erişim denetimi (IAM)**  > **rol ataması Ekle**.
3. Bir rol seçin.
4. Bir kullanıcı ekleyin.
5. **Tamam**'ı tıklatın.

>[!NOTE]
>İşleri göndermek bir kullanıcı veya güvenlik grubu gerekiyorsa, bunlar ayrıca depolama hesabındaki izni gerekir. Daha fazla bilgi için [Secure Data Lake Store içinde depolanan verileri](../data-lake-store/data-lake-store-secure-data.md).
>

<!-- ################################ -->
<!-- ################################ -->

## <a name="manage-jobs"></a>İşleri yönetme

### <a name="submit-a-job"></a>Bir iş gönderdiniz

1. Azure portalında Data Lake Analytics hesabınıza gidin.

2. Tıklayın **yeni iş**. Her bir iş için yapılandırın:

    1. **İş adı**: İş adı.
    2. **Öncelik**: Daha düşük bir sayı daha yüksek önceliğe sahiptir. Düşük öncelik değerine sahip iki işi kuyruğa alınır, ilk olarak çalışır.
    3. **Paralellik**: Bu proje için ayrılacak işlem işleme sayısı.

3. **İşi Gönder**'e tıklayın.

### <a name="monitor-jobs"></a>İşleri izleme

1. Azure portalında Data Lake Analytics hesabınıza gidin.
2. Tıklayın **işlerin tümünü görüntülemek**. Bir hesaptaki tüm etkin ve yakın zamanda tamamlanan işleri listesi gösterilir.
3. İsteğe bağlı olarak, tıklayın **filtre** işleri tarafından bulmanıza yardımcı olmak için **zaman aralığı**, **iş adı**, ve **Yazar** değerleri. 

### <a name="monitoring-pipeline-jobs"></a>İşlem hattı işlerini izleme
Bir işlem hattı bir parçası olan işleri belirli bir senaryoyu gerçekleştirmek için birlikte, genellikle bir sırayla çalışır. Örneğin, temizler, ayıklar, dönüştüren, customer ınsights için kullanım toplayan bir işlem hattı olabilir. İşlem hattı işleri, iş gönderildiğinde "İşlem hattı" özelliği kullanılarak tanımlanır. ADF V2 kullanarak zamanlanmış işleri otomatik olarak doldurulmuş bu özellik gerekir. 

İşlem hatları parçası olan U-SQL işleri listesini görüntülemek için: 

1. Azure portalında Data Lake Analytics hesaplarınızın gidin.
2. Tıklayın **iş öngörüleri**. "Tüm işler" sekmesinde olacak varsayılan olarak, çalışan, listesini gösteren sıraya alındı ve işleri sona erdi.
3. Tıklayın **işlem hattı işlerini** sekmesi. İşlem hattı işlerin bir listesini, her işlem hattı için toplu istatistikleri birlikte gösterilir.

### <a name="monitoring-recurring-jobs"></a>Yinelenen işleri izleme
Yinelenen bir iş, aynı iş mantığına sahip, ancak her çalıştırıldığında, farklı giriş verilerini kullanan biridir. İdeal olarak, yinelenen işleri her zaman başarılı ve göreceli olarak kararlı yürütme zamanlarının; Bu davranışların izleme, iş iyi durumda olmanıza yardımcı olur. Yinelenen işleri "Yinelenme" özelliği kullanılarak tanımlanır. ADF V2 kullanarak zamanlanmış işleri otomatik olarak doldurulmuş bu özellik gerekir.

Yinelenen U-SQL işleri listesini görüntülemek için: 

1. Azure portalında Data Lake Analytics hesaplarınızın gidin.
2. Tıklayın **iş öngörüleri**. "Tüm işler" sekmesinde olacak varsayılan olarak, çalışan, listesini gösteren sıraya alındı ve işleri sona erdi.
3. Tıklayın **yinelenen işleri** sekmesi. Yinelenen işler listesi yinelenen her iş için toplu istatistikleri birlikte gösterilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md)
* [Azure PowerShell kullanarak Azure Data Lake Analytics'i yönetme](data-lake-analytics-manage-use-powershell.md)
* [İlkeleri kullanarak Azure Data Lake Analytics'i yönetme](https://docs.microsoft.com/azure/data-lake-analytics/data-lake-analytics-policies)
