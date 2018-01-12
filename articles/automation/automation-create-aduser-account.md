---
title: "Azure AD Kullanıcı Hesabı oluşturma | Microsoft Docs"
description: "Bu makalede, Azure'da kimlik doğrulaması yapmak için Azure automation'da runbook'lar için bir Azure AD kullanıcı hesabı kimlik bilgisi oluşturmak açıklar."
services: automation
documentationcenter: 
author: georgewallace
manager: jwhit
editor: tysonn
keywords: "azure active directory kullanıcısı, azure hizmet yönetimi, azure ad kullanıcı hesabı"
ms.assetid: fcfe266d-b22e-4dfb-8272-adcab09fc0cf
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/13/2017
ms.author: magoedte
ms.openlocfilehash: f0a9664898cd27529daf73d130dd25fd296a9b48
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2018
---
# <a name="authenticate-runbooks-with-azure-classic-deployment-and-resource-manager"></a>Azure klasik dağıtımı ve Resource Manager ile Runbook'ların kimliklerini doğrulama
Bu makalede, Azure klasik dağıtım modeli veya Azure Resource Manager kaynaklarına karşı çalışan Azure Otomasyonu runbook’ları için bir Azure AD Kullanıcı hesabını yapılandıracak gerçekleştirmeniz gereken adımlar açıklanmaktadır.  Bu Azure Resource Manager tabanlı runbook'larınızın kimlik doğrulamasının desteklenmesi amacıyla bu devam ederken önerilen yöntem bir Azure farklı çalıştır hesabı kullanmaktır.       

## <a name="create-a-new-azure-active-directory-user"></a>Yeni bir Azure Active Directory kullanıcısı oluşturma
1. Azure Portalı’nda, yönetmek istediğiniz Azure aboneliği için hizmet yöneticisi olarak oturum açın.
2. Seçin **Azure Active Directory** > **kullanıcılar ve gruplar** > **tüm kullanıcılar** > **yeni kullanıcı**.
3. Kullanıcı için gibi ayrıntılarını girin **adı** ve **kullanıcı adı**.  
4. Kullanıcının tam adını ve geçici parolasını not edin.
5. Seçin **dizin rolünü**.
6. Genel veya sınırlı Yönetici rolü atayın.
7. Azure oturumunu kapatın ve yeni oluşturduğunuz hesapla oturum açın. Kullanıcı parolasını değiştirmeniz istenir.

## <a name="create-an-automation-account-in-the-azure-portal"></a>Azure portalında Otomasyon hesabı oluşturma
Bu bölümde, Azure Resource Manager modunda kaynakları yöneten runbook’larınızla kullanılacak Azure portalındaki Azure Otomasyonu hesabını oluşturmak için aşağıdaki adımları gerçekleştireceksiniz.  

1. Azure Portalı’nda, yönetmek istediğiniz Azure aboneliği için hizmet yöneticisi olarak oturum açın.
2. **Automation Hesapları**’nı seçin.
3. **Add (Ekle)** seçeneğini belirleyin.<br><br>![Otomasyon Hesabı ekleme](media/automation-create-aduser-account/add-automation-acct-properties.png)
4. **Automation Hesabı Ekle** dikey penceresinde, **Ad** kutusuna, yeni Automation hesabınız için bir ad yazın.
5. Birden fazla aboneliğiniz varsa, yeni hesap için, mevcut veya yeni **Kaynak grubu** ve Azure veri merkezi **konumu** ile birlikte bir tane belirtin.
6. **Azure Farklı Çalıştır hesabı oluştur** seçeneği için **Evet** değerini seçin ve **Oluştur** düğmesine tıklayın.  
   
    > [!NOTE]
    > **Hayır** seçeneğini belirleyerek Farklı Çalıştır hesabı oluşturmamayı seçerseniz, **Automation Hesabı Ekle** dikey penceresinde bir uyarı iletisi görürsünüz.  Hesap oluşturulup abonelikteki **Katılımcı** rolüne atandığı sırada, abonelikler dizini hizmetinizde karşılık gelen bir kimlik doğrulaması kimliği olmaz, bu nedenle de aboneliğinizde kaynaklara erişim de olmaz.  Azure Resource Manager kaynaklarına karşı kimlik doğrulama ve görev gerçekleştirme becerisinden bu hesaba başvuran runbook’ları koruyacaktır.
    > 
    >

    <br>![Automation Hesabı Uyarısı ekleme](media/automation-create-aduser-account/add-automation-acct-properties-error.png)<br>  
7. Azure Automation hesabını oluşturduğu sırada menünün **Bildirimler** öğesi altında ilerleme durumunu izleyebilirsiniz.

Kimlik bilgisinin oluşturulması tamamlandığında, Otomasyon hesabını daha önce oluşturulan AD Kullanıcı hesabıyla ilişkilendirmek için bir Kimlik Bilgileri Varlığı oluşturmanız gerekir.  Unutmayın, biz yalnızca Automation hesabı oluşturduk ve bu da kimlik doğrulama kimliğiyle ilişkili değil.  [Azure Automation’da kimlik bilgisi varlıkları](automation-credentials.md#creating-a-new-credential-asset) makalesinde özetlenen adımları gerçekleştirin ve **kullanıcı adı** değerini **etki alanı\kullanıcı** biçiminde girin.

## <a name="use-the-credential-in-a-runbook"></a>Runbook'ta kimlik bilgisi kullanma
[Get-AutomationPSCredential](http://msdn.microsoft.com/library/dn940015.aspx) etkinliğini kullanarak bir runbook’taki kimlik bilgisini getirebilir ve ardından Azure aboneliğinize bağlanmak için [Add-AzureAccount](http://msdn.microsoft.com/library/azure/dn722528.aspx) ile birlikte bu bilgiyi kullanabilirsiniz. Kimlik bilgisi birden çok Azure aboneliğinin yöneticisi ise, doğru olanı belirlemek için [Select-AzureSubscription](http://msdn.microsoft.com/library/dn495203.aspx)’ı kullanmalısınız. Bu, genellikle çoğu Azure Automation runbook’larının üst kısmında görünen örnek Windows PowerShell’in altında gösterilir.

    $cred = Get-AutomationPSCredential –Name "myuseraccount.onmicrosoft.com"
    Add-AzureAccount –Credential $cred
    Select-AzureSubscription –SubscriptionName "My Subscription"

Runbook uygulamanızdaki her [kontrol noktası](http://technet.microsoft.com/library/dn469257.aspx#bk_Checkpoints) sonrasında bu satırları yinelemelisiniz. Runbook askıya alınır ve ardından başka bir çalışan üzerinde işlemi sürdürürse, yeniden kimlik doğrulaması gerçekleştirmesi gerekir.

## <a name="next-steps"></a>Sonraki Adımlar
* Farklı runbook türleri ve kendi runbook'larınızı oluşturma adımları için [Azure Automation runbook türleri](automation-runbook-types.md) makalesini inceleyin.

