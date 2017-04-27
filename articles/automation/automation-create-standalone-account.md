---
title: "Tek başına Azure Otomasyonu Hesabı oluşturma | Microsoft Docs"
description: "Eğitici, Azure Automation’da güvenlik temel elemanı kimlik doğrulaması oluşturulması, test edilmesi ve örneklerinde size yol göstermektedir."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: 
ms.assetid: 2f783441-15c7-4ea0-ba27-d7daa39b1dd3
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/14/2017
ms.author: magoedte
translationtype: Human Translation
ms.sourcegitcommit: e851a3e1b0598345dc8bfdd4341eb1dfb9f6fb5d
ms.openlocfilehash: 97b863748dd726e19217e360645b8e6189c010b3
ms.lasthandoff: 04/15/2017

---

# <a name="create-a-standalone-azure-automation-account"></a>Tek başına Azure Otomasyonu hesabı oluşturma
Bu konu başlığında, runbook işlerinin gelişmiş izlemesini sağlayan ek yönetim çözümlerini veya OMS Log Analytics tümleştirmesini dahil etmeden Azure Otomasyonu’nu değerlendirip öğrenmek istiyorsanız, Azure portalından Otomasyon hesabı oluşturma işlemini göstermektedir.  Daha sonra dilediğiniz zaman bu yönetim çözümlerini ekleyebilir veya Log Analytics ile tümleştirebilirsiniz.  Otomasyon hesabı ile, Azure Resource Manager veya Azure klasik dağıtımında kaynakları yöneten runbook’ların kimliğini doğrulayabilirsiniz.

Azure portalında bir Otomasyon hesabı oluşturduğunuzda otomatik olarak şunlar oluşturulur:

* Azure Active Directory’de yeni bir hizmet sorumlusu ve bir sertifika oluşturan ve Katkı Yapana runbook’lar kullanılarak Resource Manager kaynaklarını yönetmek için kullanılan rol tabanlı erişim denetimi (RBAC) atayan Farklı Çalıştır hesabı.   
* Runbook kullanan klasik kaynakları yönetmek için kullanılan bir yönetim sertifikasını karşıya yükleyen Klasik Farklı Çalıştır hesabı.  

İşlemi sizin için basitleştirir ve otomasyon gerekliliklerini desteklemek için runbook’ları oluşturmaya ve dağıtmaya hemen başlamanıza yardımcı olur.  

## <a name="create-a-new-automation-account-from-the-azure-portal"></a>Azure portalından yeni Otomasyon Hesabı oluşturma
Bu bölümde, bir Azure Otomasyonu hesabını Azure portalından oluşturmak için aşağıdaki adımları gerçekleştirin.    

>[!NOTE]
>Bir Otomasyon hesabı oluşturmak için, Hizmet Yöneticileri rolünün bir üyesi veya aboneliğe erişim veren aboneliğin ortak yöneticisi olmanız gerekir. Ayrıca, bu aboneliğin varsayılan Active Directory örneğine kullanıcı olarak eklenmeniz gerekir. Hesabın ayrıcalıklı bir role atanması gerekmez.
>
>Aboneliğin ortak yönetici rolüne eklenmeden önce aboneliğin Active Directory örneğine üye değilseniz, Active Directory’ye konuk olarak eklenirsiniz. Bu örnekte, “Oluşturma izniniz yok…” iletisini alırsınız uyarısını **Otomasyon Hesabı Ekle** dikey penceresinde görürsünüz.
>
>İlk olarak ortak yönetici rolüne eklenen kullanıcılar aboneliğin Active Directory örneğinden kaldırılabilir ve tekrar eklenerek Active Directory’de tam bir Kullanıcı haline getirilebilir. Bu durumu Azure portalındaki **Azure Active Directory** bölmesinde **Kullanıcılar ve gruplar**’ı ve **Tüm kullanıcılar**’ı seçtikten sonra gerekli kullanıcıyı belirleyip **Profil**’i seçerek doğrulayabilirsiniz. Kullanıcı profili altındaki **Kullanıcı türü** özniteliğinin **Konuk** olmaması gerekir.

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
| AzureAutomationTutorial Runbook |Farklı Çalıştır hesabını kullanarak kimlik doğrulaması yapmayı gösteren ve tüm Resource Manager kaynaklarını alan örnek bir Grafik runbook. |
| AzureAutomationTutorialScript Runbook |Farklı Çalıştır hesabını kullanarak kimlik doğrulaması yapmayı gösteren ve tüm Resource Manager kaynaklarını alan örnek bir PowerShell runbook. |
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
* PowerShell iş akışı runbook'larını kullanmaya başlamak için bkz. İlk PowerShell iş akışı runbook’um (automation-first-runbook-textual.md).
