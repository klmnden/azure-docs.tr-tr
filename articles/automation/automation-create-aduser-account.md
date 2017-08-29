---
title: "Azure AD Kullanıcı Hesabı oluşturma | Microsoft Docs"
description: "Bu makale, Azure ve klasik Azure’da kimlik doğrulamak için Azure Otomasyonu’nda runbook’lara yönelik Azure AD Kullanıcı hesabı kimlik bilgilerini oluşturma işlemini açıklamaktadır."
services: automation
documentationcenter: 
author: MGoedtel
manager: jwhit
editor: tysonn
keywords: "azure active directory kullanıcısı, azure hizmet yönetimi, azure ad kullanıcı hesabı"
ms.assetid: fcfe266d-b22e-4dfb-8272-adcab09fc0cf
ms.service: automation
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/13/2017
ms.author: magoedte
ms.translationtype: HT
ms.sourcegitcommit: b309108b4edaf5d1b198393aa44f55fc6aca231e
ms.openlocfilehash: 4eaa3e36ededddeb5268ec4f49b9daee2f824cee
ms.contentlocale: tr-tr
ms.lasthandoff: 08/15/2017

---
# <a name="authenticate-runbooks-with-azure-classic-deployment-and-resource-manager"></a>Azure klasik dağıtımı ve Resource Manager ile Runbook'ların kimliklerini doğrulama
Bu makalede, Azure klasik dağıtım modeli veya Azure Resource Manager kaynaklarına karşı çalışan Azure Otomasyonu runbook’ları için bir Azure AD Kullanıcı hesabını yapılandıracak gerçekleştirmeniz gereken adımlar açıklanmaktadır.  Azure Resource Manager tabanlı runbook'larınız için kimlik doğrulamasının desteklenmesi amacıyla bu devam ederken önerilen yöntem bir Azure Farklı Çalıştır hesabının kullanılmasıdır.       

## <a name="create-a-new-azure-active-directory-user"></a>Yeni bir Azure Active Directory kullanıcısı oluşturma
1. Klasik Azure portalında, yönetmek istediğiniz Azure aboneliği için hizmet yöneticisi olarak oturum açın.
2. **Active Directory**'yi seçin ve ardından kuruluş dizininizin adını seçin.
3. **Kullanıcılar** sekmesini seçin ve ardından komut alanında **Kullanıcı Ekle**'yi seçin.
4. **Bu kullanıcı hakkındaki görüşlerinizi bize bildirin** sayfasında, **Kullanıcı türü** altında **Kuruluşunuzda yeni kullanıcı**’yı seçin.
5. Bir kullanıcı adı girin.  
6. Active Directory sayfasında Azure aboneliğinizle ilişkili dizin adını seçin.
7. **Kullanıcı profili** sayfasında bir ad ve soyadı, bir kolay ad, **Roller** listesinden de bir kullanıcı rolü sağlayın.  **Multi-Factor Authentication’ı Etkinleştir** seçeneğini kullanmayın.
8. Kullanıcının tam adını ve geçici parolasını not edin.
9. **Ayarlar > Yöneticiler > Ekle**’yi seçin.
10. Oluşturduğunuz kullanıcının tam kullanıcı adını yazın.
11. Kullanıcının yönetmesini istediğiniz aboneliği seçin.
12. Azure oturumunu kapatın ve yeni oluşturduğunuz hesapla oturum açın. Kullanıcı parolasını değiştirmeniz istenecektir.

## <a name="create-an-automation-account-in-azure-classic-portal"></a>Klasik Azure portalında Otomasyon hesabı oluşturma
Bu bölümde, Azure klasik dağıtımında kaynakları yöneten runbook’larınızla kullanılacak Azure portalındaki yeni Azure Otomasyonu hesabını oluşturmak için aşağıdaki adımları gerçekleştireceksiniz.  

> [!NOTE]
> Klasik Azure portalıyla oluşturulan Otomasyon hesapları hem Klasik Azure portalı, hem de Azure Portal veya cmdlet’ler grubu tarafından yönetilebilir. Hesap oluşturulduktan sonra, nasıl oluşturduğunuz veya hesaptaki kaynakları nasıl yönelttiğiniz fark etmez. Klasik Azure portalını kullanmaya devam etmeyi planlıyorsanız, Otomasyon hesaplarını oluşturmak için Azure portalı yerine bunu kullanmalısınız.
> 
> 

1. Klasik Azure portalında, yönetmek istediğiniz Azure aboneliği için hizmet yöneticisi olarak oturum açın.
2. **Automation**’ı seçin.
3. **Automation** sayfasında **Automation Hesabı Oluştur**’u seçin.
4. **Automation Hesabı Oluştur** kutusuna yeni Automation hesabınız için bir ad yazın ve açılan listeden bir **Bölge** seçin.  
5. Ayarlarınızı kabul etmek ve hesabı oluşturmak için **Tamam**’a tıklayın.
6. Oluşturulduktan sonra **Automation** sayfasında listelenecektir.
7. Hesaba tıklayın; bu sizi Pano sayfasına taşır.  
8. Automation Panosu sayfasında **Varlıklar**’ı seçin.
9. **Varlıklar** sayfasının alt tarafında yer alan **Ayarları Ekle**’yi seçin.
10. **Ayarları Ekle** sayfasında **Kimlik Bilgileri Ekle**’yi seçin.
11. **Kimlik Bilgisi Tanımla** sayfasında, **Kimlik Bilgisi Türü** açılan listesinden **Windows PowerShell Kimlik Bilgileri**’ni seçip kimlik bilgisi için bir ad verin.
12. Aşağıdaki **Kimlik Bilgisi Tanımla** sayfasında, daha önce oluşturulan AD kullanıcı hesabının kullanıcı adını **Kullanıcı Adı** alanına, parolayı **Parola** ve **Parolayı Onayla** alanlarına yazın. Değişikliklerinizi kaydetmek için **Tamam**’a tıklayın.

## <a name="create-an-automation-account-in-the-azure-portal"></a>Azure portalında Otomasyon hesabı oluşturma
Bu bölümde, Azure Resource Manager modunda kaynakları yöneten runbook’larınızla kullanılacak Azure portalındaki Azure Otomasyonu hesabını oluşturmak için aşağıdaki adımları gerçekleştireceksiniz.  

1. Azure Portalı’nda, yönetmek istediğiniz Azure aboneliği için hizmet yöneticisi olarak oturum açın.
2. **Automation Hesapları**’nı seçin.
3. Automation Hesapları dikey penceresinde **Ekle**’ye tıklayın.<br><br>![Otomasyon Hesabı ekleme](media/automation-create-aduser-account/add-automation-acct-properties.png)
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


