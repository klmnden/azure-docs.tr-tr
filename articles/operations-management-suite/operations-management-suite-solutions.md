---
title: "Operations Management Suite (OMS) çözümleri | Microsoft Docs"
description: "Çözümleri, müşterilerin kendi OMS çalışma alanına ekleyebilirsiniz paketlenmiş yönetim senaryoları sağlayarak Operations Management Suite (OMS) işlevselliği genişletir.  Bu makalede, müşterileri ve ortakları tarafından oluşturulan nasıl özel çözümler hakkında ayrıntılar sağlar."
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
ms.openlocfilehash: 2443dd73fdf441721bd6f6f340da515d9f5a22a2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="working-with-management-solutions-in-operations-management-suite-oms-preview"></a>Yönetim çözümleri Operations Management Suite (OMS) (Önizleme) içinde ile çalışma
> [!NOTE]
> Bu, şu anda önizlemede OMS yönetim çözümleri için ön belgesidir.    
> 
> 

Yönetim çözümleri, müşterilerin ortamlarına ekleyebilirsiniz paketlenmiş yönetim senaryoları sağlayarak Operations Management Suite (OMS) işlevselliği genişletir.  Ek olarak [Microsoft tarafından sağlanan çözüm](../log-analytics/log-analytics-add-solutions.md), iş ortakları ve müşterileri kendi ortamında kullanılan veya topluluk aracılığıyla müşterilere sunulacağını için yönetim çözümleri oluşturabilirsiniz.

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
8. Gerekli bilgileri gibi istenir [OMS çalışma ve Automation hesabı](#oms-workspace-and-automation-account) ek olarak çözümdeki herhangi bir parametre için değerler.
9. Tıklatın **oluşturma** çözümü yüklemek için.

### <a name="oms-portal"></a>OMS portalı
Microsoft tarafından sağlanan yönetim çözümleri OMS portalında çözümleri galerisinden yüklenebilir.

1. OMS portalında oturum açın.
2. Tıklatın **Çözümleri Galerisi** döşeme.
3. OMS Çözümleri Galerisi sayfasında, her kullanılabilir bir çözüm hakkında bilgi edinin. OMS için eklemek istediğiniz çözüm adına tıklayın.
4. Seçtiğiniz çözüm için sayfada çözümü hakkında ayrıntılı bilgi görüntülenir. **Ekle**'ye tıklayın.
5. Sayfa OMS ve verilerinizi OMS hizmetine işledikten sonra kullanmaya başlayabilmeniz için genel bakış görünür eklenen çözüm için yeni bir kutucuk.

### <a name="azure-quickstart-templates"></a>Azure Hızlı Başlangıç Şablonları
Topluluk üyeleri Azure hızlı başlangıç şablonlarını yönetim çözümleri gönderebilirsiniz.  Sonraki yüklemesi için bu şablonları yükleyebileceğiniz veya bunları öğrenmek için incelemek için nasıl [kendi çözümleri oluşturma](#creating-a-solution).

1. Açıklanan işlemi izleyin [OMS çalışma ve Automation hesabı](#oms-workspace-and-automation-account) çalışma ve hesabı bağlamak için.
2. Git [Azure hızlı başlangıç şablonlarını](https://azure.microsoft.com/documentation/templates/).  
3. İlgilendiğiniz bir çözüm arayın.
4. Çözümü sonuçlarını ayrıntılarını görüntülemek için seçin.
5. Tıklatın **Azure'a Dağıt** düğmesi.
6. Çözümdeki herhangi bir parametre için bir kaynak grubu ve konum değerleri yanı sıra gibi bilgileri sağlamak için istenir.
7. Tıklatın **satın alma** çözümü yüklemek için.

### <a name="deploy-azure-resource-manager-template"></a>Azure Resource Manager şablonu dağıtma
Topluluk veya sizin alma çözümleri [kendiniz oluşturmanız](#creating-a-solution) Resource Manager şablonu uygulanır ve için standart yöntemlerden herhangi birini kullanabilirsiniz [bir şablonu dağıtmayı](../azure-resource-manager/resource-group-template-deploy-portal.md).  Çözüm yüklemeden önce oluşturma bağlamak ve gerekir unutmayın [OMS çalışma ve Automation hesabı](#oms-workspace-and-automation-account).

## <a name="oms-workspace-and-automation-account"></a>OMS çalışma ve Automation hesabı
Çoğu yönetim çözümleri gerektiren bir [OMS çalışma](../log-analytics/log-analytics-manage-access.md) görünümleri içerecek şekilde ve bir [Otomasyon hesabı](../automation/automation-security-overview.md#automation-account-overview) runbook'ları ve ilgili kaynakları içerecek şekilde. Hesap ve çalışma alanı aşağıdaki gereksinimleri karşılaması gerekir.

* Bir çözümü, yalnızca bir OMS çalışma ve bir Automation hesabı kullanabilirsiniz.  
* OMS çalışma ve bir çözüm tarafından kullanılan Otomasyon hesabı birbirine bağlı olması gerekir. Bir OMS çalışma yalnızca bir Automation hesabı için bağlı ve Automation hesabı yalnızca bir OMS çalışma alanına bağlanır.
* Bağlantılı olması için OMS çalışma ve Automation hesabı aynı kaynak grubu ve bölge olması gerekir.  Doğu ABD bölgesinde bir OMS çalışma istisnadır ve ve Automation hesabında Doğu ABD 2.

### <a name="creating-a-link-between-an-oms-workspace-and-automation-account"></a>Bir OMS çalışma ve Otomasyon hesabı arasında bir bağlantı oluşturuluyor
Nasıl OMS çalışma alanını belirtin ve yükleme yöntemi, çözümünüz için Otomasyon hesabı bağlıdır.

* Microsoft Çözüm OMS portalı üzerinden yüklediğinizde, geçerli OMS çalışma alanında yüklenir ve herhangi bir Otomasyon hesabı gereklidir.
* Azure Market üzerinden bir çözüm yüklediğinizde, bir OMS çalışma ve Otomasyon hesabı için istenir ve aralarındaki bağlantının sizin için oluşturulur.  
* Azure Market dışında çözümleri için çözüm yüklemeden önce OMS çalışma ve Automation hesabı bağlanmalıdır.  Azure Marketi'nde herhangi bir çözüm ve Automation hesabı ve OMS çalışma alanını seçerek bunu yapabilirsiniz.  OMS çalışma ve Automation hesabı seçili hemen sonra bağlantı oluşturulacağından çözümü gerçekten yüklemek zorunda değilsiniz.  Daha sonra bağlantısı oluşturulduktan sonra herhangi bir çözümü bu OMS çalışma ve Automation hesabı kullanabilirsiniz. 

### <a name="verifying-the-link-between-an-oms-workspace-and-automation-account"></a>Bir OMS çalışma ve Otomasyon hesabı arasındaki bağlantıyı doğrulama
Bir OMS çalışma ve aşağıdaki yordamı kullanarak bir Otomasyon hesabı arasındaki bağlantıyı doğrulayabilirsiniz.

1. Azure portalında Otomasyon hesabı seçin.
2. Listenin sonuna kaydırın **ayarları** bölmesi.
3. Adlı bir bölüm olup olmadığını **OMS Kaynakları** içinde **ayarları** bölmesi, ardından bu hesap bir OMS çalışma alanına eklenir.
4. Seçin **çalışma** OMS çalışma ayrıntılarını görüntülemek için bu Otomasyon hesabına bağlanır.

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

