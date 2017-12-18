---
title: "Tek başına Azure Otomasyonu Hesabı oluşturma | Microsoft Docs"
description: "Eğitici, Azure Automation’da güvenlik temel elemanı kimlik doğrulaması oluşturulması, test edilmesi ve örneklerinde size yol göstermektedir."
services: automation
documentationcenter: 
author: georgewallace
manager: carmonm
editor: 
ms.assetid: 2f783441-15c7-4ea0-ba27-d7daa39b1dd3
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/18/2017
ms.author: magoedte
ms.openlocfilehash: dc4bfa4a94eaa2fb4e0e821c4931dcd1963f3109
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2017
---
# <a name="create-a-standalone-azure-automation-account"></a>Tek başına Azure Otomasyonu hesabı oluşturma
Bu konu başlığında, runbook işlerinin gelişmiş izlemesini sağlayan ek yönetim çözümlerini veya OMS Log Analytics tümleştirmesini dahil etmeden Azure Otomasyonu’nu değerlendirip öğrenmek istiyorsanız, Azure portalından Otomasyon hesabı oluşturma işlemini göstermektedir.  Daha sonra dilediğiniz zaman bu yönetim çözümlerini ekleyebilir veya Log Analytics ile tümleştirebilirsiniz.  Otomasyon hesabı ile, Azure Resource Manager veya Azure klasik dağıtımında kaynakları yöneten runbook’ların kimliğini doğrulayabilirsiniz.

Azure portalında bir Otomasyon hesabı oluşturduğunuzda otomatik olarak şunlar oluşturulur:

* Azure Active Directory’de yeni bir hizmet sorumlusu ve bir sertifika oluşturan ve Katkı Yapana runbook’lar kullanılarak Resource Manager kaynaklarını yönetmek için kullanılan rol tabanlı erişim denetimi (RBAC) atayan Farklı Çalıştır hesabı.   
* Runbook kullanan klasik kaynakları yönetmek için kullanılan bir yönetim sertifikasını karşıya yükleyen Klasik Farklı Çalıştır hesabı.  

İşlemi sizin için basitleştirir ve otomasyon gerekliliklerini desteklemek için runbook’ları oluşturmaya ve dağıtmaya hemen başlamanıza yardımcı olur.  

## <a name="permissions-required-to-create-automation-account"></a>Otomasyon hesabı oluşturmak için gereken izinler
Otomasyon hesabını oluşturmak veya güncelleştirmek isterseniz bu konuyu tamamlamak için gereken aşağıdaki özel ayrıcalıklara ve izinlere sahip olmanız gerekir.   
 
* Bir Otomasyon hesabı oluşturmak için AD kullanıcı hesabınızın, [Azure Otomasyonu’nda rol tabanlı erişim denetimi](automation-role-based-access-control.md) makalesinde açıklandığı gibi Microsoft.Automation kaynaklarındaki Sahip rolüne eşdeğer izinlere sahip bir role eklenmesi gerekir.  
* Azure AD kiracınızdaki yönetici olmayan kullanıcılar, Uygulama kayıtları ayarı [Evet](../azure-resource-manager/resource-group-create-service-principal-portal.md#check-azure-subscription-permissions) olarak ayarlıysa **AD uygulamalarını kaydedebilir**.  Uygulama kayıtları ayarı **Hayır** olarak ayarlanırsa bu işlemi gerçekleştiren kullanıcının, Azure AD’de genel yönetici olması gerekir. 

Aboneliğin genel yönetici/ortak yönetici rolüne eklenmeden önce aboneliğin Active Directory örneğine üye değilseniz Active Directory’ye konuk olarak eklenirsiniz. Bu durumda, “Oluşturma izniniz yok…” iletisini alırsınız. uyarısını **Otomasyon Hesabı Ekle** dikey penceresinde görürsünüz. İlk olarak genel yönetici/ortak yönetici rolüne eklenen kullanıcılar aboneliğin Active Directory örneğinden kaldırılabilir ve tekrar eklenerek Active Directory’de tam bir Kullanıcı haline getirilebilir. Bu durumu doğrulamak için Azure portalındaki **Azure Active Directory** bölmesinde **Kullanıcılar ve gruplar**’ı, **Tüm kullanıcılar**’ı seçin ve belirli bir kullanıcıyı seçtikten sonra **Profil**’i seçin. Kullanıcı profili altındaki **Kullanıcı türü** özniteliğinin **Konuk** olmaması gerekir.

## <a name="create-a-new-automation-account-from-the-azure-portal"></a>Azure portalından yeni Otomasyon Hesabı oluşturma
Bu bölümde, bir Azure Otomasyonu hesabını Azure portalından oluşturmak için aşağıdaki adımları gerçekleştirin.    

1. Azure portalında Abonelik Yöneticileri rolünün üyesi ve aboneliğin ortak yöneticisi olan bir hesapla oturum açın.
2. **Yeni**’ye tıklayın.<br><br> ![Azure portalında Yeni seçeneğini belirleyin](media/automation-offering-get-started/automation-portal-martketplacestart.png)<br>  
3. **Otomasyon** araması yapın ve sonra arama sonuçlarından **Otomasyon ve Denetim*** öğesini seçin.<br><br> ![Market’te Otomasyon araması yapıp seçin](media/automation-create-standalone-account/automation-marketplace-select-create-automationacct.png)<br> 
3. Automation Hesapları dikey penceresinde **Ekle**’ye tıklayın.<br><br>![Otomasyon Hesabı ekleme](media/automation-create-standalone-account/automation-create-automationacct-properties.png)


   > [!NOTE]
   > **Otomasyon Hesabı Ekle** dikey penceresinde aşağıdaki uyarıyı görürseniz, bunun nedeni hesabınızın Abonelik Yöneticileri rolünün üyesi ya da aboneliğin ortak yöneticisi olmamasıdır.<br><br>![Otomasyon Hesabı Ekleme Uyarısı](media/automation-create-standalone-account/create-account-without-perms.png)
   > 
   > 
4. **Automation Hesabı Ekle** dikey penceresinde, **Ad** kutusuna, yeni Automation hesabınız için bir ad yazın.
5. Birden fazla aboneliğiniz varsa, yeni hesap için mevcut veya yeni **Kaynak grubu** ve Azure veri merkezi **Konumu** ile birlikte bir tane belirtin.
6. **Azure Farklı Çalıştır hesabı oluştur** seçeneği için **Evet** değerinin seçili olduğunu doğrulayın ve **Oluştur** düğmesine tıklayın.  
   
   > [!NOTE]
   > **Hayır** seçeneğini belirleyerek Farklı Çalıştır hesabı oluşturmamayı seçerseniz, **Otomasyon Hesabı Ekle** dikey penceresinde bir uyarı iletisi görürsünüz.  Hesap Azure portalında oluşturulurken klasik veya Resource Manager abonelik dizininizde ona karşılık gelen bir kimlik doğrulama kimliği olmaz ve bu nedenle aboneliğinizdeki kaynaklara erişemez.  Bu durum, bu hesaba başvuran runbook’ların söz konusu dağıtım modellerindeki kaynaklara göre kimlik doğrulama yapmasını ve görevler gerçekleştirmesini engeller.
   > 
   > ![Otomasyon Hesabı Ekleme Uyarısı](media/automation-create-standalone-account/create-account-decline-create-runas-msg.png)<br>
   > Hizmet sorumlusu oluşturulmadığında Katkıda Bulunan rolü atanmaz.
   > 

7. Azure Automation hesabını oluşturduğu sırada menünün **Bildirimler** öğesi altında ilerleme durumunu izleyebilirsiniz.

### <a name="resources-included"></a>Kaynaklar dahil
Otomasyon hesabı başarıyla oluşturulduğunda bazı kaynaklar sizin için otomatik olarak oluşturulur.  Aşağıdaki tabloda Farklı Çalıştır hesabının kaynakları özetlenmektedir.<br>

| Kaynak | Açıklama |
| --- | --- |
| AzureAutomationTutorial Runbook |Farklı Çalıştır hesabını kullanarak kimlik doğrulaması yapmayı gösteren ve tüm Resource Manager kaynaklarını alan örnek bir Graf runbook. |
| AzureAutomationTutorialScript Runbook |Farklı Çalıştır hesabını kullanarak kimlik doğrulaması yapmayı gösteren ve tüm Resource Manager kaynaklarını alan örnek bir PowerShell runbook. |
| AzureAutomationTutorialPython2 Runbook |Farklı Çalıştır hesabını kullanarak kimlik doğrulaması gerçekleştiren ve belirtilen abonelik içindeki kaynak gruplarını listeleyen örnek Python runbook. |
| AzureRunAsCertificate |Otomasyon hesabı oluşturulurken otomatik olarak oluşturulan ya da var olan bir hesap için aşağıdaki PowerShell komut dosyası kullanılarak oluşturulan sertifika varlığı.  Azure Resource Manager kaynaklarını runbook’lardan yönetebilmeniz için Azure kimlik doğrulaması yapmanıza imkan tanır.  Bu sertifikanın bir yıllık kullanım ömrü vardır. |
| AzureRunAsConnection |Otomasyon hesabı oluşturulurken otomatik olarak oluşturulan ya da var olan bir hesap için aşağıdaki PowerShell komut dosyası kullanılarak oluşturulan bağlantı varlığı. |

Aşağıdaki tabloda Klasik Farklı Çalıştır hesabının kaynakları özetlenmektedir.<br>

| Kaynak | Açıklama |
| --- | --- |
| AzureClassicAutomationTutorial Runbook |Bir abonelikteki tüm Klasik VM'ler, Klasik Farklı Çalıştır Hesabı (sertifika) kullanarak alan ve sonra VM adını ve durumunu çıkaran örnek Graph runbook. |
| AzureClassicAutomationTutorial Script Runbook |Bir abonelikteki tüm Klasik VM'ler, Klasik Farklı Çalıştır Hesabı (sertifika) kullanarak alan ve sonra VM adını ve durumunu çıkaran örnek PowerShell runbook. |
| AzureClassicRunAsCertificate |Otomatik olarak oluşturulan ve Azure klasik kaynaklarını runbook’lardan yönetebilmeniz için Azure kimlik doğrulaması yapmak üzere kullanılan sertifika varlığı.  Bu sertifikanın bir yıllık kullanım ömrü vardır. |
| AzureClassicRunAsConnection |Otomatik olarak oluşturulan ve Azure klasik kaynaklarını runbook’lardan yönetebilmeniz için Azure kimlik doğrulaması yapmak üzere kullanılan bağlantı varlığı. |


## <a name="next-steps"></a>Sonraki adımlar
* Grafik Yazma hakkında daha fazla bilgi için bkz. [Azure Otomasyonu’nda grafik yazma](automation-graphical-authoring-intro.md).
* PowerShell runbook'ları kullanmaya başlamak için bkz. [İlk PowerShell runbook’um](automation-first-runbook-textual-powershell.md).
* PowerShell iş akışı runbook'larını kullanmaya başlamak için bkz. [İlk PowerShell iş akışı runbook uygulamam](automation-first-runbook-textual.md).
* Python2 runbook'larını kullanmaya başlamak için bkz. [İlk Python2 runbook'um](automation-first-runbook-textual-python2.md).
