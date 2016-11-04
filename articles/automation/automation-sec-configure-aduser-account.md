---
title: Azure AD Kullanıcı Hesabı Yapılandırma | Microsoft Docs
description: Bu makale, ARM ve ASM’ye karşı kimlik doğrulamak için Azure Automation’da runbook’lar için Azure AD Kullanıcı kimlik bilgilerinin yapılandırılması açıklanmaktadır.
services: automation
documentationcenter: ''
author: MGoedtel
manager: jwhit
editor: tysonn
keywords: azure active directory kullanıcısı, azure hizmet yönetimi, azure ad kullanıcı hesabı

ms.service: automation
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/12/2016
ms.author: magoedte

---
# Azure Hizmet Yönetimi ve Resource Manager ile Runbook'ların kimliklerini doğrulama
Bu makalede, Azure Hizmet Yönetimi (ASM) veya Azure Resource Manager (ARM) kaynaklarına karşı çalışan Azure Automation runbook’ları için bir Azure AD Kullanıcı hesabını yapılandıracak gerçekleştirmeniz gereken adımlar açıklanmaktadır.  ARM tabanlı runbook'larınız için kimlik doğrulamasının desteklenmesi amacıyla bu devam ederken önerilen yöntem yeni Azure Farklı Çalıştır hesabının kullanılmasıdır.       

## Yeni bir Azure Active Directory kullanıcısı oluşturma
1. Klasik Azure Portalı’nda, yönetmek istediğiniz Azure aboneliği için hizmet yöneticisi olarak oturum açın.
2. **Active Directory**'yi seçin ve ardından kuruluşunuz dizininizin adını seçin.
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

## Klasik Azure Portalı'nda Automation hesabı oluşturma
Bu bölümde, ASM ve ARM modunda kaynakları yöneten runbook’larınızla kullanılacak Azure Portal’daki yeni Azure Automation hesabını oluşturmak için şu adımları gerçekleştireceksiniz.  

> [!NOTE]
> Klasik Azure Portalı’yla oluşturulan Automation hesapları hem Klasik Azure Portalı, hem de Azure Portal veya cmdlet’ler grubu tarafından yönetilebilir. Hesap oluşturulduktan sonra, nasıl oluşturduğunuz veya hesaptaki kaynakları nasıl yönelttiğiniz fark etmez. Klasik Azure Portalı’nı kullanmaya devam etmeyi planlıyorsanız, Automation hesaplarını oluşturmak için Azure Portal yerine bunu kullanmalısınız.
> 
> 

1. Klasik Azure Portalı’nda, yönetmek istediğiniz Azure aboneliği için hizmet yöneticisi olarak oturum açın.
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

## Azure Portal'da Automation hesabı oluşturma
Bu bölümde, ARM modunda kaynakları yöneten runbook’larınızla kullanılacak Azure Portal’daki yeni Azure Automation hesabını oluşturmak için şu adımları gerçekleştireceksiniz.  

1. Azure Portalı’nda, yönetmek istediğiniz Azure aboneliği için hizmet yöneticisi olarak oturum açın.
2. **Automation Hesapları**’nı seçin.
3. Automation Hesapları dikey penceresinde **Ekle**’ye tıklayın.<br>![Automation Hesabı ekleme](media/automation-sec-configure-azure-runas-account/add-automation-acct-properties.png)
4. **Automation Hesabı Ekle** dikey penceresinde, **Ad** kutusuna, yeni Automation hesabınız için bir ad yazın.
5. Birden fazla aboneliğiniz varsa, yeni hesap için, mevcut veya yeni **Kaynak grubu** ve Azure veri merkezi **konumu** ile birlikte bir tane belirtin.
6. **Azure Farklı Çalıştır hesabı oluştur** seçeneği için **Hayır** değerini seçin ve **Oluştur** düğmesine tıklayın.  
   
   > [!NOTE]
   > **Hayır** seçeneğini belirleyerek Farklı Çalıştır hesabı oluşturmamayı seçerseniz, **Automation Hesabı Ekle** dikey penceresinde bir uyarı iletisi görürsünüz.  Hesap oluşturulup abonelikteki **Katılımcı** rolüne atandığı sırada, abonelikler dizini hizmetinizde karşılık gelen bir kimlik doğrulaması kimliği olmaz, bu nedenle de aboneliğinizde kaynaklara erişim de olmaz.  ARM kaynaklarına karşı kimlik doğrulama ve görev gerçekleştirme becerisinden bu hesaba başvuran runbook’ları koruyacaktır.
   > 
   > 
   
    ![Automation Hesabı Uyarısı ekleme](media/automation-sec-configure-azure-runas-account/add-automation-acct-properties-error.png)
7. Azure Automation hesabını oluşturduğu sırada menünün **Bildirimler** öğesi altında ilerleme durumunu izleyebilirsiniz.

Kimlik bilgisinin oluşturulması tamamlandığında, Automation hesabını daha önce oluşturulan AD Kullanıcı hesabıyla ilişkilendirmek için bir Kimlik Bilgileri Varlığı oluşturmanız gerekir.  Unutmayın, biz yalnızca Automation hesabı oluşturduk ve bu da kimlik doğrulama kimliğiyle ilişkili değil.  [Azure Automation’da kimlik bilgisi varlıkları](automation-credentials.md#creating-a-new-credential) makalesinde özetlenen adımları gerçekleştirin ve **kullanıcı adı** değerini **etki alanı\kullanıcı** biçiminde girin.

## Runbook'ta kimlik bilgisi kullanma
[Get-AutomationPSCredential](http://msdn.microsoft.com/library/dn940015.aspx) etkinliğini kullanarak bir runbook’taki kimlik bilgisini getirebilir ve ardından Azure aboneliğinize bağlanmak için [Add-AzureAccount](http://msdn.microsoft.com/library/azure/dn722528.aspx) ile birlikte bu bilgiyi kullanabilirsiniz. Kimlik bilgisi birden çok Azure aboneliğinin yöneticisi ise, doğru olanı belirlemek için [Select-AzureSubscription](http://msdn.microsoft.com/library/dn495203.aspx)’ı kullanmalısınız. Bu, genellikle çoğu Azure Automation runbook’larının üst kısmında görünen örnek Windows PowerShell’in altında gösterilir.

    $cred = Get-AutomationPSCredential –Name "myuseraccount.onmicrosoft.com"
    Add-AzureAccount –Credential $cred
    Select-AzureSubscription –SubscriptionName "My Subscription"

Runbook uygulamanızdaki her [kontrol noktası](http://technet.microsoft.com/library/dn469257.aspx#bk_Checkpoints) sonrasında bu satırları yinelemelisiniz. Runbook askıya alınır ve ardından başka bir çalışan üzerinde işlemi sürdürürse, yeniden kimlik doğrulaması gerçekleştirmesi gerekir.

## Sonraki Adımlar
* Farklı runbook türleri ve kendi runbook'larınızı oluşturma adımları için [Azure Automation runbook türleri](automation-runbook-types.md) makalesini inceleyin.

<!--HONumber=Sep16_HO3-->


