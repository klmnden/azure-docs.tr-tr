---
title: Tek başına Azure Otomasyonu hesabı oluşturma
description: Bu makalede oluşturma, test etme ve Azure Otomasyonu'ndaki bir örnek güvenlik sorumlusu kimlik doğrulaması kullanma adımlarında size kılavuzluk eder.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 01/15/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: cdffc339bee1f5456e4eeb619e566b1f9c34b143
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61076823"
---
# <a name="create-a-standalone-azure-automation-account"></a>Tek başına Azure Otomasyonu hesabı oluşturma

Bu makalede, Azure portalında Azure Otomasyonu hesabı oluşturma işlemini gösterir. Değerlendirmek ve ek yönetim çözümlerini veya tümleştirme ile Azure İzleyici günlüklerine kullanmadan Otomasyon hakkında bilgi edinmek için portal Otomasyon hesabı kullanabilirsiniz. Bu yönetim çözümlerini ekleyebilir veya Gelişmiş runbook işlerinin herhangi bir noktada gelecekte izlemek için Azure İzleyici günlükleri ile tümleştirin.

Bir Otomasyon hesabı ile Azure Resource Manager veya Klasik dağıtım modeli kaynakları yöneterek runbook'ların kimliğini doğrulayabilirsiniz. Bir Otomasyon Hesabı belirli bir kiracı için kaynakları tüm bölgelerde ve aboneliklerde yönetebilir.

Bu hesaplar, Azure portalında bir Otomasyon hesabı oluşturduğunuzda otomatik olarak oluşturulur:

* **Farklı Çalıştır hesabı**. Bu hesap aşağıdaki görevleri gerçekleştirir:
  * Azure Active Directory (Azure AD) bir hizmet sorumlusu oluşturur.
  * Bir sertifika oluşturur.
  * Runbook'ları kullanarak Azure Resource Manager kaynaklarını yöneten Contributor Role-Based Access Control'nı (RBAC) atar.
* **Klasik farklı çalıştır hesabı**. Bu hesap, bir yönetim sertifikasını karşıya yükler. Sertifika, runbook kullanarak Klasik kaynakları yönetir.

Bu hesapları sizin için oluşturulan, oluşturma ve Otomasyon gerekliliklerini desteklemek için runbook'ları dağıtma hızlıca başlatabilirsiniz.

## <a name="permissions-required-to-create-an-automation-account"></a>Bir Otomasyon hesabı oluşturmak için gereken izinler

Oluşturulacak veya güncelleştirilecek bir Otomasyon hesabı ve bu makalede açıklanan görevleri tamamlamak için aşağıdaki ayrıcalıklara ve izinlere sahip olmalıdır:

* Bir Otomasyon hesabı oluşturmak için Azure AD kullanıcı hesabınızın sahip rolüne eşdeğer izinlere sahip bir role eklenmesi gerekir **Microsoft. Otomasyon** kaynakları. Daha fazla bilgi için [Azure automation'da rol tabanlı erişim denetimi](automation-role-based-access-control.md).
* Azure portalında altında **Azure Active Directory** > **Yönet** > **uygulama kayıtları**, **uygulama kayıtları**  ayarlanır **Evet**, Azure AD kiracınızdaki yönetici olmayan kullanıcılar [Active Directory uygulamaları kaydetme](../active-directory/develop/howto-create-service-principal-portal.md#check-azure-subscription-permissions). Varsa **uygulama kayıtları** ayarlanır **Hayır**, bu eylemi gerçekleştiren kullanıcının Azure AD'de genel yönetici olması gerekir.

Aboneliğin genel yönetici/Abonelikteki rolüne eklenmeden önce aboneliğin Active Directory örneğine üye değilseniz Active Directory'ye konuk olarak eklenir. Bu senaryoda, bu ileti gördüğünüz **Otomasyon hesabı Ekle** sayfası: "Oluşturmak için izniniz yok."

Bir kullanıcı, genel yönetici/Abonelikteki role eklendi, ilk olarak, aboneliğin Active Directory örneğinden kaldırın ve Active Directory'de tam bir kullanıcı rolüne rolleriniz.

Kullanıcı rolleri doğrulamak için:

1. Azure portalında Git **Azure Active Directory** bölmesi.
1. **Kullanıcı ve gruplar**'ı seçin.
1. Seçin **tüm kullanıcılar**.
1. Belirli bir kullanıcıyı seçtikten sonra seçin **profili**. Değerini **kullanıcı türü** özniteliği kullanıcı profili altındaki olmamalıdır **Konuk**.

## <a name="create-a-new-automation-account-in-the-azure-portal"></a>Azure portalında yeni bir Otomasyon hesabı oluşturma

Azure portalında bir Azure Otomasyonu hesabını oluşturmak için aşağıdaki adımları tamamlayın:

1. Azure portalında abonelik Yöneticileri rolünün üyesi ve bir Abonelikteki olan bir hesapla oturum açın.
1. Seçin **+ kaynak Oluştur**.
1. Arama **Otomasyon**. Arama sonuçlarında seçin **Otomasyon**.

   ![Arayın ve Azure Market'te Automation and Control seçin](media/automation-create-standalone-account/automation-marketplace-select-create-automationacct.png)

1. Sonraki ekranda seçin **Oluştur**.

   ![Otomasyon hesabı Ekle](media/automation-create-standalone-account/automation-create-automationacct-properties.png)

   > [!NOTE]
   > İçinde aşağıdaki iletiyi görürseniz **Otomasyon hesabı Ekle** bölmesinde, hesabınızın abonelik Yöneticileri rolünün üyesi ve bir Abonelikteki değil.
   >
   > ![Automation hesabı uyarısı ekleme](media/automation-create-standalone-account/create-account-without-perms.png)

1. İçinde **Otomasyon hesabı Ekle** bölmesinde, **adı** kutusuna, yeni Automation hesabınız için bir ad girin. Bu ad, seçildikten sonra değiştirilemez. *Bölge ve kaynak grubu başına Otomasyon hesabı adları benzersizdir. Silinen bir Otomasyon hesapları için adları hemen kullanılamayabilir.*
1. İçinde birden fazla aboneliğiniz varsa **abonelik** kutusunda, yeni hesap için kullanmak istediğiniz aboneliği belirtin.
1. İçin **kaynak grubu**yeni veya mevcut bir kaynak grubu seçin veya girin.
1. İçin **konumu**, bir Azure veri merkezi bölgesi seçin.
1. İçin **oluşturma Azure farklı çalıştır hesabı** seçeneğinde, emin **Evet** seçili ve ardından **Oluştur**.

   > [!NOTE]
   > Seçerek farklı çalıştır hesabı oluşturmamayı seçerseniz **Hayır** için **oluşturma Azure farklı çalıştır hesabı**, bir ileti görünür **Otomasyon hesabı Ekle** bölmesi. Hesap Azure portalında oluşturulsa da, hesap, Klasik dağıtım modeli aboneliğinizdeki veya Azure Resource Manager abonelik dizininizde karşılık gelen bir kimlik doğrulama kimliği yoktur. Bu nedenle, Otomasyon hesabı, aboneliğinizde kaynaklara erişimi yok. Bu kimlik doğrulaması ve söz konusu dağıtım modellerindeki kaynaklara göre görevleri gerçekleştirmesini bu hesaba başvuran runbook'ları engeller.
   >
   > ![Automation hesabı uyarısı ekleme](media/automation-create-standalone-account/create-account-decline-create-runas-msg.png)
   >
   > Hizmet sorumlusu oluşturulmaz, katkıda bulunan rolü atanmaz.
   >

1. Otomasyon hesabı oluşturma, menüde ilerlemesini izlemek için **bildirimleri**.

### <a name="resources-included"></a>Kaynaklar dahil

Otomasyon hesabı başarıyla oluşturulduğunda bazı kaynaklar sizin için otomatik olarak oluşturulur. Oluşturulduktan sonra bunları tutmak istemiyorsanız bu runbook'ları güvenli bir şekilde silinebilir. Farklı Çalıştır hesapları, hesabınızda bir runbook'ta kimlik doğrulaması için kullanılabilir ve başka bir oluşturmadığınız sürece bırakılmalıdır veya bunları gerektirmez. Aşağıdaki tabloda Farklı Çalıştır hesabının kaynakları özetlenmektedir.

| Resource | Açıklama |
| --- | --- |
| AzureAutomationTutorial Runbook |Farklı Çalıştır hesabını kullanarak kimlik doğrulaması yapmayı gösteren bir örnek grafik runbook. Runbook, tüm Resource Manager kaynaklarını alır. |
| AzureAutomationTutorialScript Runbook |Farklı Çalıştır hesabını kullanarak kimlik doğrulaması yapmayı gösteren bir örnek PowerShell runbook. Runbook, tüm Resource Manager kaynaklarını alır. |
| AzureAutomationTutorialPython2 Runbook |Farklı Çalıştır hesabını kullanarak kimlik doğrulaması yapmayı gösteren bir örnek Python runbook. Runbook, abonelikte mevcut olan tüm kaynak gruplarını listeler. |
| AzureRunAsCertificate |Otomasyon hesabı oluşturduğunuzda veya mevcut bir hesap için bir PowerShell Betiği kullanılarak otomatik olarak oluşturulan sertifika varlığı. Azure Resource Manager kaynaklarını runbook'lardan yönetebilmeniz ile Azure sertifika kimlik doğrulamasını yapar. Bu sertifikanın bir yıllık kullanım ömrü vardır. |
| AzureRunAsConnection |Otomasyon hesabı oluşturduğunuzda veya mevcut bir hesap için bir PowerShell Betiği kullanılarak otomatik olarak oluşturulan bağlantı varlığı. |

Aşağıdaki tabloda Klasik Farklı Çalıştır hesabının kaynakları özetlenmektedir.

| Resource | Açıklama |
| --- | --- |
| AzureClassicAutomationTutorial Runbook |Örnek grafik runbook. Runbook, Klasik farklı çalıştır hesabı (sertifika) kullanarak bir Abonelikteki tüm Klasik Vm'leri alır. Ardından, sanal makine adları ve durumunu görüntüler. |
| AzureClassicAutomationTutorial Script Runbook |Örnek PowerShell runbook. Runbook, Klasik farklı çalıştır hesabı (sertifika) kullanarak bir Abonelikteki tüm Klasik Vm'leri alır. Ardından, sanal makine adları ve durumunu görüntüler. |
| AzureClassicRunAsCertificate |Otomatik olarak oluşturulan sertifika varlığı. Azure Klasik kaynaklarını runbook'lardan yönetebilmeniz ile Azure sertifika kimlik doğrulamasını yapar. Bu sertifikanın bir yıllık kullanım ömrü vardır. |
| AzureClassicRunAsConnection |Otomatik olarak oluşturulan bağlantı varlığı. Azure Klasik kaynaklarını runbook'lardan yönetebilmeniz varlık Azure ile kimlik doğrulaması yapar. |

## <a name="next-steps"></a>Sonraki adımlar

* Grafik yazma hakkında daha fazla bilgi için bkz: [Azure Otomasyonu'nda grafik yazma](automation-graphical-authoring-intro.md).
* PowerShell runbook'ları kullanmaya başlamak için bkz. [İlk PowerShell runbook’um](automation-first-runbook-textual-powershell.md).
* PowerShell iş akışı runbook'larını kullanmaya başlamak için bkz. [İlk PowerShell iş akışı runbook uygulamam](automation-first-runbook-textual.md).
* Python2 runbook'larını kullanmaya başlamak için bkz. [İlk Python2 runbook'um](automation-first-runbook-textual-python2.md).

