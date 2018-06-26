---
title: Azure yönetim çözümleri | Microsoft Docs
description: Azure yönetim çözümlerine kurallarının belirli sorunu etrafına özetlenebilir ölçümleri sağlayan mantığı, Görselleştirme ve veri alım koleksiyonudur.  Bu makalede, yükleme ve yönetim çözümleri kullanma hakkında bilgi sağlar.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: f029dd6d-58ae-42c5-ad27-e6cc92352b3b
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2018
ms.author: bwren
ms.openlocfilehash: 3377a0b4e6440d83962d103b3e1770ccf43bd785
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36752930"
---
# <a name="management-solutions-in-azure"></a>Azure yönetim çözümleri
Yönetim çözümleri belirli bir uygulama veya hizmet işlemi hakkındaki ek bilgiler sağlamak üzere Azure hizmetlerinde yararlanın. Bu makale, kullanma ve bunları yükleme Azure ve ayrıntıları yönetim çözümlerine kısa bir genel bakış sağlar.

Yönetim çözümleri genellikle günlük analizi bilgi toplamak ve günlük arar ve toplanan verileri çözümlemek için görünümleri sağlar. Bunlar uygulama veya hizmet ile ilgili eylemler gerçekleştirmek için Azure Otomasyonu gibi başka hizmetleri da.

Azure aboneliğiniz tüm uygulamaları ve kullandığınız Hizmetleri için yönetim çözümleri ekleyebilirsiniz. Bunlar genellikle, kullanım ücretleri çağıramadı hiçbir maliyet ancak toplama verileri kullanılabilir. Microsoft tarafından sağlanan çözümler ek olarak, iş ortakları ve müşterileri için [yönetim çözümleri oluşturma](../monitoring/monitoring-solutions-creating.md) kendi ortamında kullanılan veya topluluk aracılığıyla müşteriler için kullanılabilir.

## <a name="using-management-solutions"></a>Yönetim çözümleri kullanma
**Genel bakış** her günlük analizi çalışma alanı çalışma alanında yüklü her bir çözüm için bir kutucuğu görüntüler için sayfa. Daha ayrıntılı bir analiz içeren görünümünü açmak çözüm için döşeme toplanan verileri tıklayın.

![Genel Bakış](media/monitoring-solutions/overview.png)

Herhangi bir kaynağa gibi bir çözüm bulunan herhangi bir kaynağa görüntüleyebilir ve yönetim çözümleri Azure kaynaklarını birden çok türde içerebilir. Örneğin, Çözümdeki tüm günlük aramaları dahil edilen **kayıtlı aramaları** çalışma. Geçici analiz günlük analizi gerçekleştirirken aramaları kullanabilirsiniz.

## <a name="list-installed-management-solutions"></a>Yüklü yönetim çözümleri listesi 
Aboneliğinizde yüklü yönetim çözümleri listelemek için aşağıdaki yordamı kullanın.

1. Azure portalında oturum açın.
2. Sol bölmede seçin **tüm hizmetleri**.
3. Aşağı kaydırarak ya da **çözümleri** veya türü *çözümleri* içine **filtre** iletişim.
4. Tüm çalışma alanlarında yüklü çözümleri listelenmektedir. Çözüm adı yüklenir günlük analizi çalışma alanı adı tarafından izlenir.
1. Açılan kutuları ekranın üstünde abonelik veya kaynak grubu tarafından filtre uygulamak için kullanın.


![Tüm çözümleri listesi](media/monitoring-solutions/list-solutions-all.png)

Özet sayfasını açmak için bir çözüm adına tıklayın. Bu sayfayı Çözümdeki tüm günlük analizi görünümlerini görüntüler ve çözüm için farklı seçenekler kendisi ve çalışma alanı sağlar. Yukarıdaki yordamları listesi çözümlerine birini kullanarak bir çözüm için Özet sayfasını görüntülemek ve çözüm adına tıklayın.

![Çözüm özellikleri](media/monitoring-solutions/solution-properties.png)



## <a name="install-a-management-solution"></a>Bir yönetim çözümü
Microsoft ve ortaklarından yönetim çözümleri web'da [Azure Marketi](https://azuremarketplace.microsoft.com). Kullanılabilir çözümler aramak ve bunları aşağıdaki yordamı kullanarak yükleyin.

1. Gelen [aboneliğiniz için çözümlerin listesini](#list-installed-management-solutions), tıklatın **Ekle**. 
1. Sağındaki **yönetim çözümleri**, tıklatın **daha fazla**. 
1. İstediğiniz ve açıklamasını okuma yönetim çözümü bulun.
1. Tıklatın **oluşturma** yükleme işlemini başlatmak için.
1. Yükleme işlemi başladığında, hangi değişir gerekli yapılandırma için her bir çözüm sağlamak için istenir. Bunların tümünün günlük analizi çalışma alanı seçmek için çözüm yüklü olduğu ve verileri nerede toplanacağını gerektirir. 

![Bir çözümü](media/monitoring-solutions/install-solution.png)

### <a name="install-a-solution-from-the-community"></a>Topluluktan bir çözüm yükleyin
Topluluk üyeleri Azure hızlı başlangıç şablonlarını yönetim çözümleri gönderebilirsiniz. Bu çözümlerin doğrudan yükleyebilir veya daha yeni yükleme için şablonlar indirin.

1. Açıklanan işlemi izleyin [günlük analizi çalışma alanı ve Automation hesabı](#log-analytics-workspace-and-automation-account) çalışma ve hesabı bağlamak için.
2. Git [Azure hızlı başlangıç şablonlarını](https://azure.microsoft.com/documentation/templates/). 
3. İlgilendiğiniz bir çözüm arayın.
4. Çözümü sonuçlarını ayrıntılarını görüntülemek için seçin.
5. Tıklatın **Azure'a Dağıt** düğmesi.
6. Çözümdeki herhangi bir parametre için bir kaynak grubu ve konum değerleri yanı sıra gibi bilgileri sağlamak için istenir.
7. Tıklatın **satın alma** çözümü yüklemek için.


## <a name="log-analytics-workspace-and-automation-account"></a>Günlük analizi çalışma alanı ve Automation hesabı
Tüm yönetim çözümleri gerektiren bir [günlük analizi çalışma alanı](../log-analytics/log-analytics-manage-access.md) çözümü tarafından toplanan verileri depolamak ve kendi günlük aramalar ve görünümler barındırmak için. Bazı çözümleri de gerektiren bir [Otomasyon hesabı](../automation/automation-security-overview.md#automation-account-overview) runbook'ları ve ilgili kaynakları içerecek şekilde. Hesap ve çalışma alanı aşağıdaki gereksinimleri karşılaması gerekir.

* Her bir çözüm yüklemesini yalnızca bir günlük analizi çalışma alanı ve bir Automation hesabı kullanabilirsiniz. Çözüm birden çok çalışma alanları halinde ayrı olarak yükleyebilirsiniz.
* Bir çözümü bir Otomasyon hesabı gerektiriyorsa, ardından günlük analizi çalışma alanı ve Automation hesabı birbirine bağlı olması gerekir. Günlük analizi çalışma alanı yalnızca bir Automation hesabı için bağlı ve Automation hesabı yalnızca bir günlük analizi çalışma alanına bağlanır.
* Bağlanması için günlük analizi çalışma alanı ve Automation hesabı aynı kaynak grubunu ve bölge olması gerekir. Doğu ABD bölgesinde çalışma ve Doğu ABD 2 Automation hesabında istisnadır.

### <a name="creating-a-link-between-a-log-analytics-workspace-and-automation-account"></a>Günlük analizi çalışma alanı ve Automation hesabı arasında bir bağlantı oluşturuluyor
Otomasyon hesabı ve günlük analizi çalışma alanı nasıl belirttiğiniz yükleme yöntemi, çözümünüz için bağlıdır.

* Azure Market üzerinden bir çözüm yüklediğinizde, bir çalışma ve Automation hesabı girmeniz istenir. Zaten bağlı değilseniz, bunların arasındaki bağlantı oluşturulur.
* Azure Market dışında çözümleri için günlük analizi çalışma alanı ve Automation hesabı çözümü yüklemeden önce bağlanmalıdır. Azure Marketi'nde herhangi bir çözüm ve Automation hesabı ve günlük analizi çalışma alanını seçerek bunu yapabilirsiniz. Otomasyon hesabı ve günlük analizi çalışma alanı seçili hemen bağlantısını oluşturduğundan çözümü gerçekten yüklemek zorunda değilsiniz. Ardından bağlantısı oluşturulduktan sonra herhangi bir çözümü için günlük analizi çalışma alanı ve Automation hesabı kullanabilirsiniz.

### <a name="verifying-the-link-between-a-log-analytics-workspace-and-automation-account"></a>Günlük analizi çalışma alanı ve Automation hesabı arasındaki bağlantıyı doğrulama
Günlük analizi çalışma alanı aşağıdaki yordamı kullanarak bir Otomasyon hesabı arasındaki bağlantıyı doğrulayabilirsiniz.

1. Azure portalında Otomasyon hesabı seçin.
1. Kaydırma **ilgili kaynaklar** menüsünün bölümünde.
1. Varsa **çalışma** ayarı etkinleştirildiğinde, sonra bu hesabı günlük analizi çalışma alanına bağlanır. Tıklatabilirsiniz **çalışma** çalışma ayrıntılarını görüntülemek için.

## <a name="remove-a-management-solution"></a>Bir yönetim çözümünü Kaldır
Yüklü çözümünü kaldırmak için bulmak [yüklü çözümlerin listesini](#list-installed-management-solutions). Özet sayfasını açın ve ardından tıklatın için çözüm adına tıklayın **silmek**.




## <a name="next-steps"></a>Sonraki adımlar
* Alma bir [Microsoft'tan Yönetimi çözümlerinin listesi](monitoring-solutions-inventory.md).
* Bilgi nasıl [sorgular oluşturmak](../log-analytics/log-analytics-log-searches.md) yönetim çözümleri tarafından toplanan verileri çözümlemek için.

