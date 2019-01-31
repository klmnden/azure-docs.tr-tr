---
title: Azure İzleyici'de yönetim çözümleri | Microsoft Docs
description: Azure İzleyici'de yönetim çözümleri belirli sorun alanı çerçevesinde özetlenmiş ölçümler sağlayan mantık, Görselleştirme ve veri alımı kurallarından oluşan bir koleksiyondur.  Bu makalede, yükleme ve yönetim çözümleri kullanma hakkında bilgi sağlar.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: f029dd6d-58ae-42c5-ad27-e6cc92352b3b
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 12/07/2018
ms.author: bwren
ms.openlocfilehash: 0103225803ca98cb9d8e27eb57f150e0660cd7ab
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55299147"
---
# <a name="management-solutions-in-azure-monitor"></a>Azure İzleyici'de yönetim çözümleri
Yönetim çözümleri belirli bir uygulama veya hizmet işlemi ek Öngörüler sağlar, Azure hizmetlerinde yararlanın. Bu makalede, kullanarak ve bunları yükleme yönetim çözümlerini Azure'da ve ayrıntıları kısa bir genel bakış sağlar.

Yönetim çözümleri genellikle günlük verilerini toplamak ve sorguları ve toplanan verileri çözümlemek için görünümler sağlar. Ayrıca uygulama veya hizmetin ilgili eylemleri gerçekleştirmek için Azure Otomasyonu gibi diğer hizmetlerin yararlanarak.

Azure İzleyici, tüm uygulamaları ve Hizmetleri için yönetim çözümleri ekleyebilirsiniz. Bunlar genellikle, kullanım ücretleri çağırabilirsiniz maliyet ancak toplama veri yok kullanılabilir. Microsoft tarafından sağlanan çözümleri yanı sıra, iş ortaklarımız ve müşterilerimiz için [yönetim çözümleri oluşturma](solutions-creating.md) kendi ortamında kullanılabilir veya topluluk aracılığıyla kullanıma sunulan için.

## <a name="use-management-solutions"></a>Yönetim çözümleri kullanın
Açık **genel bakış** Log Analytics çalışma alanınız için sayfayı görüntülemek çalışma alanında yüklü her çözüm için bir kutucuk. 

1. Azure portalında oturum açın.
1. Açık **tüm hizmetleri** bulun **İzleyici**.
1. Altında **Insights** menüsünde **daha fazla**.
1. Açılır liste kutusu ekranın en üstünde çalışma alanı ya da kutucuklar için kullanılan zaman aralığını değiştirmek için kullanın.
1. Toplanan verileri bir çözümün daha ayrıntılı analiz içeren onun görünümünü açmak kutucuğa tıklayın.

![Genel Bakış](media/solutions/overview.png)

Yönetim çözümleri birden çok Azure kaynakları içerebilir ve bir çözüme gibi herhangi bir kaynağa dahil tüm kaynakları görüntüleyebilir. Örneğin, Çözümdeki tüm günlük sorguları altında listelenen **çözüm sorguları** içinde [sorgu Gezgini](../log-query/get-started-portal.md#load-queries) Log Analytics ile geçici analiz gerçekleştirilirken bu sorguları kullanabilirsiniz.

## <a name="list-installed-management-solutions"></a>Yüklü yönetim çözümleri listesi 
Aboneliğinizde yüklü yönetim çözümleri listelemek için aşağıdaki yordamı kullanın.

1. Azure portalında oturum açın.
1. Açık **tüm hizmetleri** bulun **çözümleri**.
4. Tüm çalışma alanlarında yüklü çözümleri listelenmiştir. Çözüm adını yüklenir Log Analytics çalışma alanının adını takip eder.
1. Açılır liste kutusu ekranın en üstünde, abonelik veya kaynak grubuna göre filtrele için kullanın.


![Tüm çözümler listesi](media/solutions/list-solutions-all.png)

Kendi Özet sayfasını açmak için bir çözümün adına tıklayın. Bu sayfa, Çözümdeki tüm görünümleri görüntüler ve kendisi ve çalışma alanı çözümü farklı seçenekler sunar. Listesi çözümlere yordamlardan birini kullanarak bir çözüm için Özet sayfasında görüntüleyin ve ardından çözümün adına tıklayın.

![Çözüm özellikleri](media/solutions/solution-properties.png)



## <a name="install-a-management-solution"></a>Bir yönetim çözümü yükleme
Microsoft ve ortaklarından yönetim çözümleri web'da [Azure Marketi](https://azuremarketplace.microsoft.com). Kullanılabilir çözümler arayabilir ve bunları aşağıdaki yordamı kullanarak yükleyin. Bir çözüm yüklediğinizde seçmelisiniz bir [Log Analytics çalışma alanı](../platform/manage-access.md) çözüm yükleneceği ve verilerinin nerede toplanacağını.

1. Gelen [aboneliğiniz için çözümlerin listesini](#list-installed-management-solutions), tıklayın **Ekle**. 
1. Sağındaki **yönetim çözümleri**, tıklayın **daha fazla**. 
1. Açıklamasını okuma ve istediğiniz yönetim çözümü bulun.
1. Tıklayın **Oluştur** yükleme işlemini başlatmak için.
1. Yükleme işlemi başladığında, göre farklılık gösteren gerekli yapılandırma her çözüm için sağlamanız istenir.

![Çözüm yükleme](media/solutions/install-solution.png)

### <a name="install-a-solution-from-the-community"></a>Topluluktan bir çözüm yükleyin
Topluluk üyeleri, yönetim çözümleri Azure hızlı başlangıç şablonları gönderebilirsiniz. Bu çözümler doğrudan yükleyebilir veya bunları daha sonra yükleme için şablonları indirin.

1. Açıklanan işlemi izleyin [Log Analytics çalışma alanını ve Otomasyon hesabı](#log-analytics-workspace-and-automation-account) bir çalışma alanı ve hesabı bağlamak için.
2. Git [Azure hızlı başlangıç şablonları](https://azure.microsoft.com/documentation/templates/). 
3. İlginizi çeken bir çözüm arayın.
4. Çözüm sonuçları ayrıntılarını görüntülemek için seçin.
5. Tıklayın **azure'a Dağıt** düğmesi.
6. Çözümdeki herhangi bir parametre için bir kaynak grubu ve konum değerlere ek olarak gibi bilgiler sağlamanız istenir.
7. Tıklayın **satın alma** çözümü yüklemek için.


## <a name="log-analytics-workspace-and-automation-account"></a>Log Analytics çalışma alanı ve Otomasyon hesabı
Tüm yönetim çözümleri gerektiren bir [Log Analytics çalışma alanı](../platform/manage-access.md) çözüm tarafından toplanan verileri depolamak ve kendi günlük aramaları ve görünümleri barındırmak için. Ayrıca bazı çözümler gerektiren bir [Otomasyon hesabı](../../automation/automation-security-overview.md#automation-account-overview) runbook'ları ve ilgili kaynakları içerecek şekilde. Çalışma alanı ve hesabı aşağıdaki gereksinimleri karşılaması gerekir.

* Her bir çözümün yüklenmesi yalnızca bir Log Analytics çalışma alanı ve bir Otomasyon hesabı kullanabilirsiniz. Çözüm birden çok çalışma alanı içinde ayrı olarak yükleyebilirsiniz.
* Bir çözüm bir Otomasyon hesabı gerektiriyorsa, ardından Log Analytics çalışma alanını ve Otomasyon hesabı birbirine bağlı olmalıdır. Bir Log Analytics çalışma alanı yalnızca bir Otomasyon hesabına bağlı ve bir Otomasyon hesabı yalnızca bir Log Analytics çalışma alanına bağlı.
* Bağlanacak, Log Analytics çalışma alanını ve Otomasyon hesabı aynı kaynak grubunda ve bölgede olması gerekir. Özel durum, Doğu ABD bölgesinde bir çalışma alanı ve Otomasyon hesabı Doğu ABD 2 ' dir.

### <a name="create-a-link-between-a-log-analytics-workspace-and-automation-account"></a>Log Analytics çalışma alanını ve Otomasyon hesabı arasında bir bağlantı oluşturun
Log Analytics çalışma alanını ve Otomasyon hesabının nasıl belirttiğiniz çözümünüz için yükleme yöntemine bağlıdır.

* Azure Marketi aracılığıyla bir çözüm yükleme sırasında bir çalışma alanı ve Otomasyon hesabı için istenir. Zaten bağlı değilseniz, bunlar arasında bir bağlantı oluşturulur.
* Azure Marketi dışında çözümler için çözüm yüklenmeden önce Log Analytics çalışma alanını ve Otomasyon hesabı bağlamalısınız. Azure Market'te herhangi bir çözüm ve Log Analytics çalışma alanını ve Otomasyon hesabı seçerek bunu yapabilirsiniz. Log Analytics çalışma alanını ve Otomasyon hesabı seçili hemen sonra bağlantıyı oluşturulduğundan çözüm gerçekten yüklemeniz gerekmez. Ardından bağlantıyı oluşturulduktan sonra herhangi bir çözümü, Log Analytics çalışma alanını ve Otomasyon hesabı kullanabilirsiniz.

### <a name="verify-the-link-between-a-log-analytics-workspace-and-automation-account"></a>Log Analytics çalışma alanını ve Otomasyon hesabı arasındaki bağlantıyı doğrulayın
Aşağıdaki yordamı kullanarak bir Otomasyon hesabı ile Log Analytics çalışma alanı arasındaki bağlantı doğrulayabilirsiniz.

1. Azure portalında Otomasyon hesabı seçin.
1. Kaydırma **ilgili kaynakları** menüsünün bölümünde.
1. Varsa **çalışma** ayarı etkinse, sonra bu hesabı bir Log Analytics çalışma alanına bağlı. Tıklayabilirsiniz **çalışma** çalışma alanının ayrıntılarını görüntülemek için.

## <a name="remove-a-management-solution"></a>Bir yönetim çözümünü Kaldır
Yüklü çözümünü kaldırmak için bulmak [yüklü çözümlerinin listesini](#list-installed-monitoring-solutions). Kendi Özet sayfasını açın ve ardından çözümün adına tıklayarak **Sil**.




## <a name="next-steps"></a>Sonraki adımlar
* Alma bir [Microsoft gelen yönetim çözümlerinin listesini](solutions-inventory.md).
* Bilgi nasıl [sorguları oluşturma](../../azure-monitor/log-query/log-query-overview.md) yönetim çözümleri tarafından toplanan verileri analiz etmek için.

