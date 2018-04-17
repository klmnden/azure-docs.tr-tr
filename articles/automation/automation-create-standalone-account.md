---
title: Tek başına Azure Otomasyonu hesabı oluşturma
description: Bu makalede, oluşturma, sınama ve Azure Otomasyonu'nda bir örnek güvenlik temel elemanı kimlik doğrulaması kullanarak adım adım anlatılmaktadır.
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 03/15/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: 619b0a12122fcd6b51a4e34a3021bc99047f9251
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="create-a-standalone-azure-automation-account"></a>Tek başına Azure Otomasyonu hesabı oluşturma

Bu makalede Azure portalında bir Azure Otomasyonu hesabı oluşturulacağını gösterir. Portal Otomasyon hesabı değerlendirmek ve ek yönetim çözümleri veya tümleştirme Azure günlük analizi ile kullanmadan Otomasyon hakkında bilgi almak için kullanabilirsiniz. Bu yönetim çözümleri ekleyebilir veya Gelişmiş runbook işlerinin herhangi bir noktada gelecekte izleme için günlük analizi ile tümleştirin.

Bir Otomasyon hesabı ile runbook'ları Azure Resource Manager veya Klasik dağıtım modeli kaynakları yöneterek doğrulayabilir.

Azure portalında bir Otomasyon hesabı oluşturduğunuzda, bu hesapları otomatik olarak oluşturulur:

* **Farklı Çalıştır hesabı**. Bu hesap, aşağıdaki görevleri gerçekleştirir:
  * Azure Active Directory (Azure AD) bir hizmet sorumlusu oluşturur.
  * Bir sertifika oluşturur.
  * Runbook'ları kullanarak Azure Resource Manager kaynaklarını yönetir Contributor Role-Based erişim denetimi'nı (RBAC) atar.
* **Klasik farklı çalıştır hesabı**. Bu hesap bir yönetim sertifikasını karşıya yükler. Sertifika, runbook'ları kullanarak Klasik kaynakları yönetir.

Bu hesaplar için oluşturduğunuz, oluşturma ve Otomasyon gerekliliklerini desteklemek için runbook'ları dağıtma hızla başlatabilirsiniz.

## <a name="permissions-required-to-create-an-automation-account"></a>Bir Otomasyon hesabı oluşturmak için gereken izinler

Oluşturun veya bir Otomasyon hesabı güncelleştirin ve bu makalede açıklanan görevleri tamamlamak için aşağıdaki ayrıcalıkları ve izinleri olması gerekir:

* Bir Otomasyon hesabı oluşturmak için Azure AD kullanıcı hesabınız için sahibi rolüne eşdeğer izinlere sahip bir rol eklenmeli **Microsoft. Otomasyon** kaynakları. Daha fazla bilgi için bkz: [Azure automation'da rol tabanlı erişim denetimi](automation-role-based-access-control.md).
* Azure portalında altında **Azure Active Directory** > **Yönet** > **uygulama kayıtlar**, **uygulama kayıtlar**  ayarlanır **Evet**, yönetici olmayan kullanıcıların Azure AD kiracınızda [Active Directory uygulamalarını kaydetmek](../azure-resource-manager/resource-group-create-service-principal-portal.md#check-azure-subscription-permissions). Varsa **uygulama kayıtlar** ayarlanır **Hayır**, bu eylem gerçekleştiren kullanıcıyı Azure AD genel yönetici olması gerekir.

Aboneliğin genel yönetici/Abonelikteki rolüne eklenmeden önce aboneliğin Active Directory örneğine üyesi değilseniz, Active Directory'ye konuk olarak eklenir. Bu senaryoda, bu iletiyi gördüğünüz **Automation hesabı Ekle** sayfa: "Oluşturmak için izniniz yok."

Bir kullanıcı genel yönetici/Abonelikteki rolüne eklenen yaparsanız ilk olarak, aboneliğin Active Directory örnekten kaldırın ve ardından bunları Active Directory'de tam kullanıcı rolünü yeniden ekleyin.

Kullanıcı rolleri doğrulamak için:

1. Azure portalında Git **Azure Active Directory** bölmesi.
1. Seçin **kullanıcılar ve gruplar**.
1. Seçin **tüm kullanıcılar**.
1. Belirli bir kullanıcı seçtikten sonra Seç **profil**. Değeri **kullanıcı türü** özniteliğinin kullanıcının profilini altında olmamalıdır **Konuk**.

## <a name="create-a-new-automation-account-in-the-azure-portal"></a>Azure portalında yeni bir Otomasyon hesabı oluşturma

Azure portalında bir Azure Otomasyonu hesabı oluşturmak için aşağıdaki adımları tamamlayın:

1. Azure portalında abonelik Yöneticileri rolünün üyesi ve bir Abonelikteki olan bir hesapla oturum açın.
1. Seçin **+ kaynak oluşturma**.
1. Arama **Otomasyon**. Arama sonuçlarında seçin **Otomasyon**.

   ![Arama ve otomasyon ve denetim Azure Marketi'nde seçin](media/automation-create-standalone-account/automation-marketplace-select-create-automationacct.png)

1. Sonraki ekranda seçin **oluşturma**.
  ![Automation hesabı ekleme](media/automation-create-standalone-account/automation-create-automationacct-properties.png)

  > [!NOTE]
  > Aşağıdaki iletiyi görürseniz **Automation hesabı Ekle** bölmesinde, hesabınızın abonelik Yöneticileri rolünün üyesi ve bir Abonelikteki değil.
  >
  > ![Automation hesabı uyarısı ekleme](media/automation-create-standalone-account/create-account-without-perms.png)
  >
1. İçinde **Automation hesabı Ekle** bölmesi, **adı** kutusuna, yeni Automation hesabınız için bir ad girin.
1. İçinde birden fazla aboneliğiniz varsa **abonelik** kutusunda, yeni hesap için kullanmak istediğiniz aboneliği belirtin.
1. İçin **kaynak grubu**yeni veya var olan kaynak grubu seçin veya girin.
1. İçin **konumu**, bir Azure veri merkezi konum seçin.
1. İçin **oluşturma Azure farklı çalıştır hesabı** seçeneği, emin **Evet** seçili ve ardından **oluşturma**.

  > [!NOTE]
  > Belirleyerek farklı çalıştır hesabı oluşturmamayı seçerseniz **Hayır** için **oluşturma Azure farklı çalıştır hesabı**, bir ileti belirir **Automation hesabı Ekle** bölmesi. Hesap Azure portalında oluşturulur, ancak karşılık gelen bir kimlik doğrulama kimliği Klasik dağıtım modeli aboneliğinizi veya Azure Resource Manager Abonelik dizin hizmeti hesabı yok. Bu nedenle, Automation hesabı aboneliğinizde kaynaklara erişimi yok. Bu kimlik doğrulama ve konusu dağıtım modellerindeki kaynaklara karşı görevleri gerçekleştirme becerisinden bu hesaba başvuran runbook'ları önler.
  >
  > ![Automation hesabı uyarısı ekleme](media/automation-create-standalone-account/create-account-decline-create-runas-msg.png)
  >
  > Hizmet sorumlusu oluşturulmadığında, katkıda bulunan rolü atanmaz.
  >

1. Otomasyon hesabı oluşturma menüsünde ilerlemesini izlemek için **bildirimleri**.

### <a name="resources-included"></a>Kaynaklar dahil

Otomasyon hesabı başarıyla oluşturulduğunda bazı kaynaklar sizin için otomatik olarak oluşturulur. Oluşturulduktan sonra bu runbook'lar güvenle olabilir kalmasını istemiyorsanız silindi. Farklı Çalıştır hesapları, hesabınıza bir runbook'ta kimliğini doğrulamak için kullanılan ve başka bir tane oluşturmak sürece bırakılmalıdır veya bunları gerektirmez. Aşağıdaki tabloda Farklı Çalıştır hesabının kaynakları özetlenmektedir.

| Kaynak | Açıklama |
| --- | --- |
| AzureAutomationTutorial Runbook |Farklı Çalıştır hesabını kullanarak kimlik doğrulaması yapmayı gösteren bir örnek grafik runbook. Runbook tüm Resource Manager kaynakları alır. |
| AzureAutomationTutorialScript Runbook |Farklı Çalıştır hesabını kullanarak kimlik doğrulaması yapmayı gösteren bir örnek PowerShell runbook. Runbook tüm Resource Manager kaynakları alır. |
| AzureAutomationTutorialPython2 Runbook |Farklı Çalıştır hesabını kullanarak kimlik doğrulaması yapmayı gösteren bir örnek Python runbook. Runbook abonelikte mevcut tüm kaynak grupları listeler. |
| AzureRunAsCertificate |Otomasyon hesabı oluşturduğunuzda veya mevcut hesap için bir PowerShell komut dosyası kullanarak otomatik olarak oluşturulan sertifika varlığı. Azure Resource Manager kaynaklarını runbook'lardan yönetebilmeniz için şekilde Azure ile sertifika kimlik doğrulamasını yapar. Bu sertifikanın bir yıllık kullanım ömrü vardır. |
| AzureRunAsConnection |Otomasyon hesabı oluşturduğunuzda veya mevcut hesap için bir PowerShell komut dosyası kullanarak otomatik olarak oluşturulan bağlantı varlığı. |

Aşağıdaki tabloda Klasik Farklı Çalıştır hesabının kaynakları özetlenmektedir.

| Kaynak | Açıklama |
| --- | --- |
| AzureClassicAutomationTutorial Runbook |Bir örnek grafik runbook. Runbook, Klasik farklı çalıştır hesabı (sertifika) kullanarak, bir Abonelikteki tüm Klasik sanal makineleri alır. Ardından, VM adları ve durumunu görüntüler. |
| AzureClassicAutomationTutorial Script Runbook |Örnek bir PowerShell runbook. Runbook, Klasik farklı çalıştır hesabı (sertifika) kullanarak, bir Abonelikteki tüm Klasik sanal makineleri alır. Ardından, VM adları ve durumunu görüntüler. |
| AzureClassicRunAsCertificate |Otomatik olarak oluşturulan sertifika varlığı. Azure Klasik kaynaklarını runbook'lardan yönetebilmeniz için şekilde Azure ile sertifika kimlik doğrulamasını yapar. Bu sertifikanın bir yıllık kullanım ömrü vardır. |
| AzureClassicRunAsConnection |Otomatik olarak oluşturulan bağlantı varlığı. Azure Klasik kaynaklarını runbook'lardan yönetebilmeniz için şekilde varlık Azure ile kimliğini doğrular. |

## <a name="next-steps"></a>Sonraki adımlar

* Grafik yazma hakkında daha fazla bilgi için bkz: [Azure Automation'da grafik yazma](automation-graphical-authoring-intro.md).
* PowerShell runbook'ları kullanmaya başlamak için bkz. [İlk PowerShell runbook’um](automation-first-runbook-textual-powershell.md).
* PowerShell iş akışı runbook'larını kullanmaya başlamak için bkz. [İlk PowerShell iş akışı runbook uygulamam](automation-first-runbook-textual.md).
* Python2 runbook'larını kullanmaya başlamak için bkz. [İlk Python2 runbook'um](automation-first-runbook-textual-python2.md).
