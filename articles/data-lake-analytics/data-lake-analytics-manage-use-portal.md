---
title: "Azure portalı kullanarak Azure Data Lake Analytics'i yönetme | Microsoft Docs"
description: "Data Lake Analytics sayılarının, veri kaynakları, kullanıcılar ve işleri yönetmeyi öğrenin."
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: kfile
editor: cgronlun
ms.assetid: a0e045f1-73d6-427f-868d-7b55c10f811b
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2016
ms.author: saveenr
ms.openlocfilehash: 93815904e7e21e1ba8283d7a522297c7e3466702
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="manage-azure-data-lake-analytics-by-using-the-azure-portal"></a>Azure portalı kullanarak Azure Data Lake Analytics'i yönetme
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Azure portalı kullanarak Azure Data Lake Analytics hesapları, hesap veri kaynakları, kullanıcılar ve işleri yönetmeyi öğrenin. Yönetimi konuları hakkında diğer araçları kullanarak görmek için sayfanın üst kısmındaki sekme Ek Yardım düğmesini tıklatın.

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

## <a name="manage-policies"></a>İlkeleri yönetme

### <a name="account-level-policies"></a>Hesap düzeyinde ilkeleri

Bu ilkeler, bir Data Lake Analytics hesabı tüm işler için geçerlidir.

#### <a name="maximum-number-of-aus-in-a-data-lake-analytics-account"></a>En fazla Avustralya numarasında Data Lake Analytics hesabı
Bir ilke Analytics Data Lake Analytics hesabınızı kullanabilirsiniz birimlerin (Avustralya) toplam sayısını denetler. Varsayılan değer 250'ye ayarlanır. Örneğin, bu değer 250'ye ayarlanmışsa, Avustralya, 250 ile çalışan işiniz olabilir veya 25 ile çalışan 10 işleri atanan Avustralya Avustralya her. Çalışan işler tamamlanana kadar gönderilen ek işler kuyruğa alınır. Çalışan işleri tamamlandığında, Avustralya çalıştırmak için sıraya alınan işleri kaydınızı kurtulurlar.

Data Lake Analytics hesabınızın Avustralya sayısını değiştirmek için:

1. Azure Portal'da Data Lake Analytics hesabınıza gidin.
2. **Özellikler**'e tıklayın.
3. Altında **maksimum Avustralya**, bir değer seçmek için kaydırıcıyı hareket ettirin veya metin kutusuna bir değer girin. 
4. **Kaydet**’e tıklayın.

> [!NOTE]
> Birden çok varsayılan (250) gerekiyorsa Avustralya, Portalı'nda tıklatın **Yardım + Destek** bir destek isteği gönderilemedi. Avustralya Data Lake Analytics hesabınızı kullanılabilir sayısı artırılabilir.
>

#### <a name="maximum-number-of-jobs-that-can-run-simultaneously"></a>Aynı anda çalışabilir işi sayısı üst sınırı
Bir ilke, aynı anda kaç işleri çalıştırabilirsiniz denetler. Varsayılan olarak, bu değeri 20'ye ayarlanır. Data Lake Analytics Avustralya kullanılabilir varsa, yeni işleri hemen işlerin toplam sayısı bu ilkenin değere ulaşana kadar çalışmak üzere zamanlanmış. Maksimum sayıda aynı anda çalışabilir işi ulaştığında, sonraki işleri (AU kullanılabilirliğine bağlı olarak) bir veya daha fazla çalışan iş tamamlanana kadar öncelik sırasına göre sıralanır.

Aynı anda çalışabilir işlerinin sayısını değiştirmek için:

1. Azure Portal'da Data Lake Analytics hesabınıza gidin.
2. **Özellikler**'e tıklayın.
3. Altında **en büyük sayı, çalışan işleri**, bir değer seçmek için kaydırıcıyı hareket ettirin veya metin kutusuna bir değer girin. 
4. **Kaydet**’e tıklayın.

> [!NOTE]
> İşlerini, varsayılan (20) sayısından daha portalda çalıştırmanız gerekiyorsa, tıklatın **Yardım + Destek** bir destek isteği gönderilemedi. Data Lake Analytics hesabınızı aynı anda çalışabilir işlerin sayısı artırılabilir.
>

#### <a name="how-long-to-keep-job-metadata-and-resources"></a>Ne kadar süreyle kalmasını iş meta veri ve kaynakları 
Kullanıcılarınızın U-SQL işleri çalıştırdığınızda, Data Lake Analytics hizmeti tüm ilişkili dosyaları korur. İlgili dosyalar U-SQL betiği, U-SQL betiği, derlenmiş kaynaklar ve İstatistikler başvurulan DLL dosyaları içerir. Varsayılan Azure Data Lake Store hesabına /system/ klasöründe dosyalarıdır. Bu ilke, bu kaynakları otomatik olarak silinmeden önce ne kadar süreyle depolanır denetler (varsayılan 30 gündür). Hata ayıklama ve gelecekte yeniden işleri için performans ayarlama, bu dosyaları kullanabilirsiniz.

Ne kadar süreyle iş meta veri ve kaynakların korunmasına değiştirmek için:

1. Azure Portal'da Data Lake Analytics hesabınıza gidin.
2. **Özellikler**'e tıklayın.
3. Altında **iş sorguları korumak için gün**, bir değer seçmek için kaydırıcıyı hareket ettirin veya metin kutusuna bir değer girin.  
4. **Kaydet**’e tıklayın.

### <a name="job-level-policies"></a>Proje düzeyi ilkeleri
İş düzeyinde ilkeleriyle maksimum Avustralya ve bireysel kullanıcılar (veya belirli güvenlik gruplarının üyeleri) bunlar gönderme işlere ayarlayabilirsiniz en yüksek öncelik kontrol edebilirsiniz. Bu, kullanıcılar tarafından maliyetleri denetlemenize olanak tanır. Ayrıca sağlar denetimi zamanlanmış bir iş etkisi aynı Data Lake Analytics hesabı çalıştıran yüksek öncelikli üretim işlerini olabilir.

Data Lake Analytics işi düzeyinde ayarlayabilirsiniz iki ilke vardır:

* **İş başına AU sınırı**: kullanıcılar yalnızca bu Avustralya sayıya sahip işleri gönderme. Varsayılan olarak, bu sınır AU sınırını hesabı ile aynıdır.
* **Öncelik**: kullanıcılar yalnızca bu değere eşit veya değerinden daha düşük bir öncelik sahip işleri gönderme. Daha yüksek bir sayı daha düşük bir öncelik anlamına gelir. Varsayılan olarak, bu olası en yüksek öncelikli olduğu 1 olarak ayarlanır.

Varsayılan ilke her hesabında kümesi yok. Varsayılan ilke, hesap tüm kullanıcılar için geçerlidir. Ek ilkeler belirli kullanıcılar ve gruplar için ayarlayabilirsiniz. 

> [!NOTE]
> Hesap düzeyinde ilkeler ve iş düzeyinde ilkeleri aynı anda uygulayın.
>

#### <a name="add-a-policy-for-a-specific-user-or-group"></a>Belirli bir kullanıcı veya grup için bir ilke ekleme

1. Azure Portal'da Data Lake Analytics hesabınıza gidin.
2. **Özellikler**'e tıklayın.
3. Altında **iş gönderim sınırları**, tıklatın **ilke Ekle** düğmesi. Ardından, seçin veya aşağıdaki ayarları girin:
    1. **İlke adı işlem**: ilke amacı anımsatmak için bir ilke adı girin.
    2. **Kullanıcı veya Grup Seç**: kullanıcı veya grubun bu ilkenin uygulandığı seçin.
    3. **İş AU sınırı**: Seçili kullanıcı veya grup için geçerlidir AU sınırı ayarlar.
    4. **Öncelik sınırı**: Seçili kullanıcı veya grup için geçerli bir öncelik sınırı.

4. **Tamam**’a tıklayın.

5. Yeni ilke listelenen **varsayılan** altında İlkesi tablo **iş gönderim sınırları**. 

#### <a name="delete-or-edit-an-existing-policy"></a>Silin veya mevcut bir ilkeyi Düzenle

1. Azure Portal'da Data Lake Analytics hesabınıza gidin.
2. **Özellikler**'e tıklayın.
3. Altında **iş gönderim sınırları**, düzenlemek istediğiniz ilkeyi bulun.
4.  Görmek için **silmek** ve **Düzenle** en sağdaki sütun tablonun seçenekleri **...** .

### <a name="additional-resources-for-job-policies"></a>İş ilkeleri için ek kaynaklar
* [İlke genel bakış blog gönderisi](https://blogs.msdn.microsoft.com/azuredatalake/2017/06/08/managing-your-azure-data-lake-analytics-compute-resources-overview/)
* [Hesap düzeyinde ilkeler blog gönderisi](https://blogs.msdn.microsoft.com/azuredatalake/2017/06/08/managing-your-azure-data-lake-analytics-compute-resources-account-level-policy/)
* [Proje düzeyi ilkeleri blog gönderisi](https://blogs.msdn.microsoft.com/azuredatalake/2017/06/08/managing-your-azure-data-lake-analytics-compute-resources-job-level-policy/)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md)
* [Azure portalını kullanarak Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-portal.md)
* [Azure PowerShell kullanarak Azure Data Lake Analytics'i yönetme](data-lake-analytics-manage-use-powershell.md)

