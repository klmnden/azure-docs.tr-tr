---
title: "Azure yönetim çözümleri | Microsoft Docs"
description: "Yönetim çözümleri paketlenmiş yönetim senaryoları müşteriler kendi günlük analizi çalışma alanına ekleyebilirsiniz Azure dahil edin.  Bu makalede, müşterileri ve ortakları tarafından oluşturulan nasıl özel çözümler hakkında ayrıntılar sağlar."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 1f054a4e-6243-4a66-a62a-0031adb750d8
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/01/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2b9ad6da3963fefc5441581d113f6f690bd72be0
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="working-with-management-solutions-in-azure-preview"></a>Yönetim çözümlerine Azure (Önizleme) ile çalışma
> [!NOTE]
> Bu, şu anda önizlemede olan Azure yönetim çözümleri için ön belgesidir.    
> 
> 

Yönetim çözümleri müşteriler kendi Azure ortamına ekleyebileceğiniz paketlenmiş yönetim senaryolar içerir.  Ek olarak [Microsoft tarafından sağlanan çözüm](../log-analytics/log-analytics-add-solutions.md), iş ortakları ve müşterileri kendi ortamında kullanılan veya topluluk aracılığıyla müşterilere sunulacağını için yönetim çözümleri oluşturabilirsiniz.

## <a name="finding-and-installing-management-solutions"></a>Bulma ve yönetim çözümleri yükleme
Bulma ve yönetim çözümleri aşağıdaki bölümlerde açıklandığı gibi yükleme için birden çok yöntem bulunmaktadır.

### <a name="azure-marketplace"></a>Azure Market
Yönetim çözümleri Microsoft tarafından sağlanan ve Azure portalında Azure Marketi'nden güvenilen ortaklar yüklü olabilir.

1. Azure portalında oturum açın.
2. Sol bölmede seçin **daha fazla hizmet**.
3. Aşağı kaydırarak ya da **çözümleri** veya türü *çözümleri* içine **filtre** iletişim.
4. Tıklatın **+ Ekle** düğmesi.
5. Arama tıklatarak da giderek, ilgilendiğiniz çözümleri için **filtre** düğmesini veya yazarak **arama Everthing** kutusu.
6. Ayrıntılı bilgilerini görüntülemek için bir Market öğesini tıklatın.
7. Tıklatın **oluşturma** açmak için **Çözüm Ekle** bölmesi.
8. Gerekli bilgileri gibi istenir [günlük analizi çalışma alanı ve Automation hesabı](#log-analytics-workspace-and-automation-account) ek olarak çözümdeki herhangi bir parametre için değerler.
9. Tıklatın **oluşturma** çözümü yüklemek için.

### <a name="oms-portal"></a>OMS portalı
Microsoft tarafından sağlanan yönetim çözümleri OMS portalında çözümleri galerisinden yüklenebilir.

1. OMS portalında oturum açın.
2. Tıklatın **Çözümleri Galerisi** döşeme.
3. OMS Çözümleri Galerisi sayfasında, her kullanılabilir bir çözüm hakkında bilgi edinin. Eklemek istediğiniz çözüm adına tıklayın.
4. Seçtiğiniz çözüm için sayfada çözümü hakkında ayrıntılı bilgi görüntülenir. **Ekle**'ye tıklayın.
5. Portal ve sayfa günlük analizi verilerinizi işledikten sonra kullanmaya başlayabilmeniz için genel bakış görünür eklenen çözüm için yeni bir kutucuk.

### <a name="azure-quickstart-templates"></a>Azure Hızlı Başlangıç Şablonları
Topluluk üyeleri Azure hızlı başlangıç şablonlarını yönetim çözümleri gönderebilirsiniz.  Sonraki yüklemesi için bu şablonları yükleyebileceğiniz veya bunları öğrenmek için incelemek için nasıl [kendi çözümleri oluşturma](#creating-a-solution).

1. Açıklanan işlemi izleyin [günlük analizi çalışma alanı ve Automation hesabı](#log-analytics-workspace-and-automation-account) çalışma ve hesabı bağlamak için.
2. Git [Azure hızlı başlangıç şablonlarını](https://azure.microsoft.com/documentation/templates/).  
3. İlgilendiğiniz bir çözüm arayın.
4. Çözümü sonuçlarını ayrıntılarını görüntülemek için seçin.
5. Tıklatın **Azure'a Dağıt** düğmesi.
6. Çözümdeki herhangi bir parametre için bir kaynak grubu ve konum değerleri yanı sıra gibi bilgileri sağlamak için istenir.
7. Tıklatın **satın alma** çözümü yüklemek için.

### <a name="deploy-azure-resource-manager-template"></a>Azure Resource Manager şablonu dağıtma
Topluluk veya sizin alma çözümleri [kendiniz oluşturmanız](#creating-a-solution) Resource Manager şablonu uygulanır ve için standart yöntemlerden herhangi birini kullanabilirsiniz [bir şablonu dağıtmayı](../azure-resource-manager/resource-group-template-deploy-portal.md).  Çözüm yüklemeden önce oluşturma bağlamak ve gerekir unutmayın [günlük analizi çalışma alanı ve Automation hesabı](#log-analytics-workspace-and-automation-account).

## <a name="log-analytics-workspace-and-automation-account"></a>Günlük analizi çalışma alanı ve Automation hesabı
Çoğu yönetim çözümleri gerektiren bir [günlük analizi çalışma alanı](../log-analytics/log-analytics-manage-access.md) görünümleri içerecek şekilde ve bir [Otomasyon hesabı](../automation/automation-security-overview.md#automation-account-overview) runbook'ları ve ilgili kaynakları içerecek şekilde. Hesap ve çalışma alanı aşağıdaki gereksinimleri karşılaması gerekir.

* Bir çözümü, yalnızca bir günlük analizi çalışma alanı ve bir Automation hesabı kullanabilirsiniz.  
* Günlük analizi çalışma alanı ve bir çözüm tarafından kullanılan Otomasyon hesabı birbirine bağlı olması gerekir. Günlük analizi çalışma alanı yalnızca bir Automation hesabı için bağlı ve Automation hesabı yalnızca bir günlük analizi çalışma alanına bağlanır.
* Bağlanması için günlük analizi çalışma alanı ve Automation hesabı aynı kaynak grubunu ve bölge olması gerekir.  Doğu ABD bölgesinde çalışma istisnadır ve ve Automation hesabında Doğu ABD 2.

### <a name="creating-a-link-between-a-log-analytics-workspace-and-automation-account"></a>Günlük analizi çalışma alanı ve Automation hesabı arasında bir bağlantı oluşturuluyor
Otomasyon hesabı ve günlük analizi çalışma alanı nasıl belirttiğiniz yükleme yöntemi, çözümünüz için bağlıdır.

* Microsoft Çözüm OMS portalı üzerinden yüklediğinizde, geçerli çalışma alanında yüklenir ve herhangi bir Otomasyon hesabı gereklidir.
* Azure Market üzerinden bir çözüm yüklediğinizde, bir çalışma ve Automation hesabı için istenir ve aralarındaki bağlantının sizin için oluşturulur.  
* Azure Market dışında çözümleri için günlük analizi çalışma alanı ve Automation hesabı çözümü yüklemeden önce bağlanmalıdır.  Azure Marketi'nde herhangi bir çözüm ve Automation hesabı ve günlük analizi çalışma alanını seçerek bunu yapabilirsiniz.  Otomasyon hesabı ve günlük analizi çalışma alanı seçili hemen sonra bağlantı oluşturulacağından çözümü gerçekten yüklemek zorunda değilsiniz.  Ardından bağlantısı oluşturulduktan sonra herhangi bir çözümü için günlük analizi çalışma alanı ve Automation hesabı kullanabilirsiniz. 

### <a name="verifying-the-link-between-a-log-analytics-workspace-and-automation-account"></a>Günlük analizi çalışma alanı ve Automation hesabı arasındaki bağlantıyı doğrulama
Günlük analizi çalışma alanı aşağıdaki yordamı kullanarak bir Otomasyon hesabı arasındaki bağlantıyı doğrulayabilirsiniz.

1. Azure portalında Otomasyon hesabı seçin.
2. Varsa **çalışma** ayarı **ilgili kaynaklar** menüsünün bölümünde etkin sonra bu hesabı günlük analizi çalışma alanına eklenir.  Tıklatabilirsiniz **çalışma** çalışma ayrıntılarını görüntülemek için.

## <a name="listing-management-solutions"></a>Yönetim çözümleri listeleme
Aşağıdaki yordam için yönetim çözümleri Azure aboneliğinize bağlı çalışma alanları görüntülemek için kullanın.

1. Azure portalında oturum açın.
2. Sol bölmede seçin **daha fazla hizmet**.
3. Aşağı kaydırarak ya da **çözümleri** veya türü *çözümleri* içine **filtre** iletişim.
4. Tüm çalışma alanlarında yüklü çözümleri listelenir.

OMS Portalı'nı kullanarak geçerli çalışma alanında yüklü Microsoft çözümleri görüntüleyebilirsiniz unutmayın.

## <a name="removing-a-management-solution"></a>Bir yönetim çözümü kaldırma
Bir yönetim çözümü kaldırıldığında, Çözümdeki tüm kaynaklar da kaldırılır.  

1. Konusundaki yordamı kullanarak Azure portalında çözümü bulmak [çözümleri listeleme](#listing-solutions).
2. Kaldırmak istediğiniz çözümü seçin.
3. Tıklatın **silmek** düğmesi.

## <a name="creating-a-management-solution"></a>Bir yönetim çözümü oluşturma
Yönetim çözümleri oluşturma konusunda tam yönergeler şurada bulunabilir [Operations Management Suite (OMS) çözümleri oluşturma](operations-management-suite-solutions-creating.md). 

## <a name="next-steps"></a>Sonraki adımlar
* Arama [Azure hızlı başlangıç şablonlarını](https://azure.microsoft.com/documentation/templates) farklı Resource Manager şablonları örneklerini için.
* Kendinizinkini oluşturun [yönetim çözümleri](operations-management-suite-solutions-creating.md).

