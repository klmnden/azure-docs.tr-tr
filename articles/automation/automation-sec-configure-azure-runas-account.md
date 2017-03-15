---
title: "Azure Farklı Çalıştır Hesabını Yapılandırma | Microsoft Belgeleri"
description: "Eğitici, Azure Automation’da güvenlik temel elemanı kimlik doğrulaması oluşturulması, test edilmesi ve örneklerinde size yol göstermektedir."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: 
keywords: "hizmet asıl adı, setspn, azure kimlik doğrulaması"
ms.assetid: 2f783441-15c7-4ea0-ba27-d7daa39b1dd3
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/06/2017
ms.author: magoedte
translationtype: Human Translation
ms.sourcegitcommit: 094729399070a64abc1aa05a9f585a0782142cbf
ms.openlocfilehash: 7230fb1a8d27708c40040950e3ec8950c6c04780
ms.lasthandoff: 03/07/2017


---
# <a name="authenticate-runbooks-with-azure-run-as-account"></a>Azure Farklı Çalıştır hesabıyla Kimlik Doğrulaması Runbook’ları
Bu konu başlığında, Azure Resource Manager veya Azure Service Management'ta kaynakları yöneten runbook'ların kimliğini doğrulamak üzere Farklı Çalıştır hesap özelliği kullanılarak Azure portalından bir Otomasyon hesabının nasıl yapılandırılacağı gösterilecektir.

Azure portalında yeni bir Otomasyon hesabı oluşturduğunuzda otomatik olarak şunlar oluşturulur:

* Azure Active Directory’de yeni bir hizmet sorumlusu ve bir sertifika oluşturan ve Katkı Yapana runbook’lar kullanılarak Resource Manager kaynaklarını yönetmek için kullanılacak rol tabanlı erişim denetimi (RBAC) atayan Farklı Çalıştır hesabı.   
* Azure Service Management’ı ya da runbook kullanan klasik kaynakları yönetmek için kullanılacak bir yönetim sertifikasını karşıya yükleyen Klasik Farklı Çalıştır hesabı.  

İşlemi sizin için basitleştirir ve otomasyon gerekliliklerini desteklemek için runbook’ları oluşturmaya ve dağıtmaya hemen başlamanıza yardımcı olur.      

Farklı Çalıştır ve Klasik Farklı Çalıştır hesabını kullanarak şunları yapabilirsiniz:

* Azure portalındaki runbook’lardan Azure Resource Manager veya Azure Service Management kaynakları yönetilirken Azure ile kimlik doğrulaması için standartlaştırılmış bir yol sağlama.  
* Azure Uyarıları’nda yapılandırılmış genel runbook'ların kullanımını otomatikleştirin.

> [!NOTE]
> Otomasyon Genel Runbook’larına sahip Azure [Uyarı tümleştirme özelliği](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) için Farklı Çalıştır ve Klasik Farklı Çalıştır hesabıyla yapılandırılmış bir Otomasyon hesabı gerekir. Farklı Çalıştır ve Klasik Farklı Çalıştır hesabı zaten tanımlanmış bir Otomasyon hesabı seçebilir ya da yeni bir tane de oluşturabilirsiniz.
>  

Otomasyon hesabının Azure portalından oluşturulması, bir Otomasyon hesabının PowerShell kullanılarak güncelleştirilmesi, hesap yapılandırmasının yönetilmesi ve runbook’larınızda kimlik doğrulamasının nasıl yapılacağı size gösterilecektir.

Bunu yapmadan önce anlamanız ve göz önünde bulundurmanız gereken birkaç nokta vardır.

1. Bu durum klasik veya Resource Manager dağıtım modelinde daha önce oluşturulmuş var olan Otomasyon hesaplarını etkilemez.  
2. Bu özellik yalnızca Azure portalından oluşturulan Otomasyon hesapları için çalışır.  Klasik portaldan bir hesap oluşturulmaya çalışıldığında Farklı Çalıştır hesabının yapılandırması çoğaltılmaz.
3. Şu anda klasik kaynakları yönetmek için daha önce oluşturulmuş runbook’lara ve varlıklara (zamanlamalar, değişkenler vb.) sahipseniz ve bu runbook’ların yeni Klasik Farklı Çalıştır hesabıyla kimlik doğrulamasını yapmak istiyorsanız Farklı Çalıştır Hesabı Yönetme adımlarını kullanarak Klasik Farklı Çalıştır Hesabı oluşturmanız veya var olan hesabınızı aşağıdaki PowerShell komut dosyasıyla güncelleştirmeniz gerekir.  
4. Yeni Farklı Çalıştır hesabını ve Klasik Farklı Çalıştır Otomasyon hesabını kullanarak kimlik doğrulamak için var olan runbook’larınızı aşağıdaki örnek kod ile değiştirmeniz gerekir.  Farklı Çalıştır hesabının sertifika tabanlı hizmet sorumlusu kullanan Resource Manager kaynaklarına göre kimlik doğrulamasına, Klasik Farklı Çalıştır hesabının ise yönetim sertifikasına sahip Service Management kaynaklarına göre kimlik doğrulamasına yönelik olduğunu **lütfen unutmayın**.     

## <a name="create-a-new-automation-account-from-the-azure-portal"></a>Azure portalından yeni Otomasyon Hesabı oluşturma
Bu bölümde, yeni bir Azure Otomasyonu hesabını Azure portalından oluşturmak için aşağıdaki adımları gerçekleştireceksiniz.  Bu işlem hem Farklı Çalıştır hem de klasik Farklı Çalıştır hesabı oluşturur.  

> [!NOTE]
> Bu adımları gerçekleştiren kullanıcı, Hizmet Yöneticileri rolünün üyesi veya kullanıcıya abonelik erişimi veren aboneliğin ortak yöneticisi olmalıdır. Kullanıcı ayrıca ilgili aboneliğin varsayılan Active Directory’sine Kullanıcı olarak eklenmelidir; hesabın ayrıcalıklı bir role atanması gerekmez. Aboneliğin Ortak Yönetici rolüne eklenmeden önce Aboneliğin Active Directory üyesi olmayan kullanıcılar, Active Directory’ye Konuk olarak eklenir ve **Otomasyon Hesabı Ekle** dikey penceresinde "Oluşturma izniniz yok..." uyarısı görüntülenir. İlk olarak ortak yönetici rolüne eklenen kullanıcılar Active Directory aboneliklerinden kaldırılabilir ve tekrar eklenerek Active Directory’ye tam bir Kullanıcı haline getirilebilir. Bu durumu Azure portalındaki **Azure Active Directory** bölmesinde **Kullanıcılar ve gruplar**’ı ardından **Tüm kullanıcılar**’ı seçtikten sonra gerekli kullanıcıyı seçip **Profil**’i seçerek doğrulayabilirsiniz.  Kullanıcı profili altındaki **Kullanıcı türü** özniteliğinin **Konuk** olmaması gerekir.  
> 

1. Azure portalında Abonelik Yöneticileri rolünün üyesi ve aboneliğin ortak yöneticisi olan bir hesapla oturum açın.
2. **Automation Hesapları**’nı seçin.
3. Automation Hesapları dikey penceresinde **Ekle**’ye tıklayın.<br>![Otomasyon Hesabı ekleme](media/automation-sec-configure-azure-runas-account/create-automation-account-properties-b.png)
   
   > [!NOTE]
   > **Otomasyon Hesabı Ekle** dikey penceresinde aşağıdaki uyarıyı görürseniz, bunun nedeni hesabınızın Abonelik Yöneticileri rolünün üyesi ya da aboneliğin ortak yöneticisi olmamasıdır.<br>![Otomasyon Hesabı Ekleme Uyarısı](media/automation-sec-configure-azure-runas-account/create-account-without-perms.png)
   > 
   > 
4. **Automation Hesabı Ekle** dikey penceresinde, **Ad** kutusuna, yeni Automation hesabınız için bir ad yazın.
5. Birden fazla aboneliğiniz varsa, yeni hesap için mevcut veya yeni **Kaynak grubu** ve Azure veri merkezi **konumu** ile birlikte bir tane belirtin.
6. **Azure Farklı Çalıştır hesabı oluştur** seçeneği için **Evet** değerinin seçili olduğunu doğrulayın ve **Oluştur** düğmesine tıklayın.  
   
   > [!NOTE]
   > **Hayır** seçeneğini belirleyerek Farklı Çalıştır hesabı oluşturmamayı seçerseniz, **Automation Hesabı Ekle** dikey penceresinde bir uyarı iletisi görürsünüz.  Hesap Azure portalında oluşturulurken klasik veya Resource Manager abonelik dizininizde ona karşılık gelen bir kimlik doğrulama kimliği olmaz ve bu nedenle aboneliğinizdeki kaynaklara erişemez.  Bu durum, bu hesaba başvuran runbook’ların söz konusu dağıtım modellerindeki kaynaklara göre kimlik doğrulama yapmasını ve görevler gerçekleştirmesini engeller.
   > 
   > ![Otomasyon Hesabı Ekleme Uyarısı](media/automation-sec-configure-azure-runas-account/create-account-decline-create-runas-msg.png)<br>
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
| AzureClassicAutomationTutorial Runbook |Bir abonelikteki tüm Klasik VM'ler, Klasik Farklı Çalıştır Hesabı (sertifika) kullanarak alan ve sonra VM adını ve durumunu çıkaran örnek Grafik runbook. |
| AzureClassicAutomationTutorial Script Runbook |Bir abonelikteki tüm Klasik VM'ler, Klasik Farklı Çalıştır Hesabı (sertifika) kullanarak alan ve sonra VM adını ve durumunu çıkaran örnek PowerShell runbook. |
| AzureClassicRunAsCertificate |Otomatik olarak oluşturulan ve Azure klasik kaynaklarını runbook’lardan yönetebilmeniz için Azure kimlik doğrulaması yapmak üzere kullanılan sertifika varlığı.  Bu sertifikanın bir yıllık kullanım ömrü vardır. |
| AzureClassicRunAsConnection |Otomatik olarak oluşturulan ve Azure klasik kaynaklarını runbook’lardan yönetebilmeniz için Azure kimlik doğrulaması yapmak üzere kullanılan bağlantı varlığı. |

## <a name="verify-run-as-authentication"></a>Farklı Çalıştır kimlik doğrulamasını onaylama
Yeni Farklı Çalıştır hesabını kullanarak kimlik doğrulamasını sorunsuz yaptığınızı doğrulamak için burada küçük bir testle devam ediyoruz.     

1. Azure portalında, daha önce oluşturduğunuz Otomasyon hesabını açın.  
2. Runbook'ların listesini açmak için **Runbook'lar** kutucuğuna tıklayın.
3. **AzureAutomationTutorialScript** runbook’unu seçin ve ardından **Başlat**’a tıklayarak runbook’u başlatın.  Runbook'u başlatmak istediğinizi doğrulayan bir ileti alacaksınız.
4. Bir [runbook işi](automation-runbook-execution.md) oluşturulur, İş dikey penceresi gösterilir ve iş durumu **İş Özeti** kutucuğunda gösterilir.  
5. İş durumu, bulutta bir runbook çalışanının kullanılabilir hale gelmesinin beklendiğini gösteren şekilde *Sırada* olarak başlar. Ardından, bir çalışan işi talep ettiğinde *Başlatılıyor* olarak ve runbook gerçekten çalışmaya başladığında *Çalışıyor* olarak değiştirilir.  
6. Runbook işi tamamlandığında **Tamamlandı** durumunu görmeniz gerekir.<br> ![Güvenlik Sorumlusu Runbook Testi](media/automation-sec-configure-azure-runas-account/job-summary-automationtutorialscript.png)<br>
7. Runbook’un ayrıntılı sonuçlarını görmek için **Çıktı** kutucuğuna tıklayın.
8. **Çıktı** dikey penceresinde kimlik doğrulamasının başarıyla yapıldığını ve kaynak grubunda mevcut olan tüm kaynakların bir listesini döndürdüğünü görmeniz gerekir.
9. **Çıktı** dikey penceresini kapatarak **İş Özeti** dikey penceresine geri dönün.
10. **İş Özeti**’ni ve karşılık gelen **AzureAutomationTutorialScript** runbook dikey penceresini kapatın.

## <a name="verify-classic-run-as-authentication"></a>Klasik Farklı Çalıştır kimlik doğrulamasını onaylama
Yeni Klasik Farklı Çalıştır hesabını kullanarak kimlik doğrulamasını sorunsuz yaptığınızı doğrulamak için burada küçük bir testle devam ediyoruz.     

1. Azure portalında, daha önce oluşturduğunuz Otomasyon hesabını açın.  
2. Runbook'ların listesini açmak için **Runbook'lar** kutucuğuna tıklayın.
3. **AzureClassicAutomationTutorialScript** runbook’unu seçin ve ardından **Başlat**’a tıklayarak runbook’u başlatın.  Runbook'u başlatmak istediğinizi doğrulayan bir ileti alacaksınız.
4. Bir [runbook işi](automation-runbook-execution.md) oluşturulur, İş dikey penceresi gösterilir ve iş durumu **İş Özeti** kutucuğunda gösterilir.  
5. İş durumu, bulutta bir runbook çalışanının kullanılabilir hale gelmesinin beklendiğini gösteren şekilde *Sırada* olarak başlar. Ardından, bir çalışan işi talep ettiğinde *Başlatılıyor* olarak ve runbook gerçekten çalışmaya başladığında *Çalışıyor* olarak değiştirilir.  
6. Runbook işi tamamlandığında **Tamamlandı** durumunu görmeniz gerekir.<br> ![Güvenlik Sorumlusu Runbook Testi](media/automation-sec-configure-azure-runas-account/job-summary-automationclassictutorialscript.png)<br>
7. Runbook’un ayrıntılı sonuçlarını görmek için **Çıktı** kutucuğuna tıklayın.
8. **Çıktı** dikey penceresinde kimlik doğrulamasının başarıyla yapıldığını ve abonelikteki tüm klasik sanal makinelerin bir listesini döndürdüğünü görmeniz gerekir.
9. **Çıktı** dikey penceresini kapatarak **İş Özeti** dikey penceresine geri dönün.
10. **İş Özeti**’ni ve karşılık gelen **AzureClassicAutomationTutorialScript** runbook dikey penceresini kapatın.

## <a name="managing-azure-run-as-account"></a>Azure Farklı Çalıştır hesabını yönetme
Otomasyon hesabınızın kullanım süresi boyunca sertifikayı kullanım süresi sona ermeden yenilemeniz gerekecektir. Hesabın ele geçirildiğine inanıyorsanız Farklı Çalıştır hesabını silip yeniden oluşturabilirsiniz.  Bu bölümde bu işlemleri gerçekleştirme adımları anlatılmaktadır.  

### <a name="self-signed-certificate-renewal"></a>Otomatik olarak imzalanan sertifika yenileme
Azure Farklı çalıştır hesabı için oluşturulan otomatik olarak imzalanan sertifika, oluşturma tarihinden itibaren bir yıl olan son kullanım tarihine kadar herhangi bir zamanda yenilenebilir.  Sertifikayı yenilediğinizde eski geçerli sertifika Farklı Çalıştır hesabıyla kimlik doğrulaması yapılan kuyruktaki veya etkin olarak çalışan runbook'ların etkilenmemesi için tutulur.  Sertifika, kullanım süresi dolana kadar tutulmaya devam eder.    

> [!NOTE]
> Otomasyon Farklı Çalıştır hesabınızı, kuruluş sertifika yetkiliniz tarafından yayınlanan bir sertifika kullanmak üzere yapılandırdıysanız ve bu seçeneği kullanırsanız, bu sertifika, otomatik olarak imzalanan bir sertifikayla değiştirilir.  

1. Azure portalında Otomasyon hesabınızı açın.  
2. Otomasyon hesabı dikey penceresinin hesap özellikleri bölmesindeki **Hesap Ayarları** bölümünden **Farklı Çalıştır Hesapları**'nı seçin.<br><br> ![Otomasyon hesabı özellikleri bölmesi](media/automation-sec-configure-azure-runas-account/automation-account-properties-pane.png)<br><br>
3. **Farklı Çalıştır Hesapları** özellikleri dikey penceresinde sertifikasını yenilemek istediğiniz Farklı Çalıştır Hesabını veya Klasik Farklı Çalıştır Hesabını seçin ve seçilen hesabın özellikler dikey penceresinde **Sertifikayı yenile**'ye tıklayın.<br><br> ![Farklı Çalıştır hesabının sertifikasını yenileme](media/automation-sec-configure-azure-runas-account/automation-account-renew-runas-certificate.png)<br><br> Devam etmek istediğinizi doğrulayan bir ileti alacaksınız.  
4. Sertifika yenilenirken menünün **Bildirimler** öğesi altında ilerleme durumunu izleyebilirsiniz.

### <a name="delete-run-as-account"></a>Farklı Çalıştır hesabını silme
Aşağıdaki adımlar, Azure Farklı Çalıştır veya Klasik Farklı Çalıştır hesabınızı silme ve yeniden oluşturmayı açıklamaktadır.  Bu eylemi gerçekleştirdiğinizde Otomasyon hesabı korunur.  Farklı Çalıştır veya Klasik Farklı Çalıştır hesabını sildikten sonra portalda yeniden oluşturabilirsiniz.  

1. Azure portalında Otomasyon hesabınızı açın.  
2. Otomasyon hesabı dikey penceresinin hesap özellikleri bölmesindeki **Hesap Ayarları** bölümünden **Farklı Çalıştır Hesapları**'nı seçin.
3. **Farklı Çalıştır Hesapları** özellikleri dikey penceresinde silmek istediğiniz Farklı Çalıştır Hesabını veya Klasik Farklı Çalıştır Hesabını seçin ve seçilen hesabın özellikler dikey penceresinde **Sil**'e tıklayın.<br><br> ![Farklı Çalıştır hesabını silme](media/automation-sec-configure-azure-runas-account/automation-account-delete-runas.png)<br><br>  Devam etmek istediğinizi doğrulayan bir ileti alacaksınız.
4. Hesap silinirken menünün **Bildirimler** öğesi altında ilerleme durumunu izleyebilirsiniz.  Silme işlemi tamamlandıktan sonra **Farklı Çalıştır Hesapları** özellikler dikey penceresinde **Azure Farklı Çalıştır Hesabı** seçeneğini belirleyerek yeniden oluşturabilirsiniz.<br><br> ![Otomasyon Farklı Çalıştır hesabını yeniden oluşturma](media/automation-sec-configure-azure-runas-account/automation-account-create-runas.png)<br> 

### <a name="misconfiguration"></a>Yanlış yapılandırma
Farklı Çalıştır veya Klasik Farklı Çalıştır hesabının düzgün çalışması için gerekli olan yapılandırma öğelerinden birinin silinmiş veya ilk kurulum sırasında düzgün oluşturulmamış olması, örnek:

* Sertifika varlığı 
* Bağlantı varlığı 
* Farklı Çalıştır hesabının katkıda bulunan rolünden kaldırılması
* Azure AD'de hizmet sorumlusu veya uygulama

Otomasyon bu değişiklikleri algılar ve hesabın **Farklı Çalıştır Hesapları** özellikleri dikey penceresinde **Tamamlanmamış** durumuyla sizi bilgilendirir.<br><br> ![Tamamlanmamış Farklı Çalıştır yapılandırma durumu iletisi](media/automation-sec-configure-azure-runas-account/automation-account-runas-incomplete-config.png)<br><br>Farklı Çalıştır hesabını seçtiğinizde hesabın özellikler bölmesinde aşağıdaki hata görüntülenir:<br><br> ![Tamamlanmamış Farklı Çalıştır yapılandırma uyarısı iletisi](media/automation-sec-configure-azure-runas-account/automation-account-runas-incomplete-config-msg.png).<br>  
Farklı Çalıştır hesabı hatalı yapılandırılmışsa bu sorunu Farklı Çalıştır hesabını silip yeniden oluşturarak hızlı bir şekilde çözebilirsiniz.   

## <a name="update-an-automation-account-using-powershell"></a>PowerShell kullanarak Automation Hesabını güncelleştirme
Burada aşağıdaki durumlarda var olan Otomasyon hesabınızı güncelleştirmek için PowerShell kullanma seçeneği sunulmaktadır:

1. Bir Otomasyon hesabı oluşturdunuz, ancak Farklı Çalıştır hesabı oluşturmayı reddettiniz 
2. Resource Manager kaynaklarını yönetmek için bir Otomasyon hesabınız zaten var ve runbook kimlik doğrulaması için Farklı Çalıştır hesabını içerecek şekilde güncelleştirmek istiyorsunuz
4. Klasik kaynakları yönetmek için bir Otomasyon hesabınız zaten var ve yeni bir hesap oluşturup runbook’larınızı ve varlıklarınızı ona geçirmek yerine Klasik Farklı Çalıştır’ı kullanacak şekilde güncelleştirmek istiyorsunuz   
5. Kuruluş sertifika yetkiliniz tarafından verilen bir sertifika kullanarak bir Azure Farklı Çalıştır ve Klasik Farklı Çalıştır hesabı oluşturmak istiyorsunuz

Bu betiğin aşağıdaki önkoşulları vardır:

1. Bu betik yalnızca Azure Resource Manager modülleri 2.01 veya üzeri yüklü Windows 10 ve Windows Server 2016 üzerinde çalışır.  Windows'un önceki sürümleri desteklenmez.  
2. Azure PowerShell 1.0 ve üzeri. Bu sürüm ve nasıl yükleneceği hakkında daha fazla bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azureps-cmdlets-docs).
3. Bir otomasyon hesabı oluşturduğunuzu.  Bu hesaba aşağıdaki betikte yer alan –AutomationAccountName ve -ApplicationDisplayName parametre değeri olarak başvurulacaktır.

Komut dosyaları için gerekli parametreler olan *SubscriptionID*, *ResourceGroup* ve *AutomationAccountName* değerlerini almak için Azure portalında **Otomasyon hesabı** dikey penceresinden Otomasyon hesabınızı seçin ve **Tüm ayarlar** seçeneğini belirleyin.  **Tüm ayarlar** dikey penceresindeki **Hesap Ayarları** altında **Özellikler**’i seçin.  **Özellikler** dikey penceresinde bu değerleri fark edebilirsiniz.<br><br> ![Otomasyon Hesabı özellikleri](media/automation-sec-configure-azure-runas-account/automation-account-properties.png)  

### <a name="create-run-as-account-powershell-script"></a>Farklı Çalıştır Hesabı PowerShell komut dosyası oluşturma
Bu PowerShell betiği aşağıdaki yapılandırmalar için destek içerir: 

* Otomatik olarak imzalanan sertifika kullanarak Azure Farklı Çalıştır hesabı oluşturma
* Otomatik olarak imzalanan sertifika kullanarak Azure Farklı Çalıştır hesabı ve Azure Klasik Çalıştır hesabı oluşturma
* Kuruluş sertifikası kullanarak Azure Farklı Çalıştır hesabı ve Azure Klasik Çalıştır hesabı oluşturma
* Azure Kamu bulutunda Otomatik olarak imzalanan sertifika kullanarak Azure Farklı Çalıştır hesabı ve Azure Klasik Çalıştır hesabı oluşturma

Belirlediğiniz yapılandırma seçeneğine bağlı olarak aşağıdakileri oluşturur:

* Otomatik olarak imzalanan sertifikayla veya kuruluş sertifikasıyla kimlik doğrulaması yapılacak Azure AD uygulaması, Azure AD'de bu uygulama için bir hizmet sorumlusu hesabı oluşturun ve geçerli aboneliğinizde bu hesap için Katılımcı rolü (bunu Sahip veya herhangi başka bir rolle değiştirebilirsiniz) atayın.  Daha fazla bilgi için lütfen[Azure Automation’da Rol Tabanlı Erişim Denetimi](automation-role-based-access-control.md) makalesini inceleyin.
* Hizmet sorumlusu tarafından kullanılan sertifikayı tutan **AzureRunAsCertificate** adlı, belirtilen Otomasyon hesabındaki bir Otomasyon sertifikası varlığı.
* applicationId, tenantId, subscriptionId ve certificate thumbprint öğelerini tutan **AzureRunAsConnection** adlı, belirtilen Automation hesabındaki bir Automation sertifikası varlığı.    

Klasik Farklı Çalıştır hesabı için:

* Runbook’larınızın kimliğini doğrulamak için kullanılan otomatik olarak imzalanan veya kuruluş sertifika yetkilisinin verdiği sertifikayı tutan **AzureClassicRunAsCertificate** adlı, belirtilen Otomasyon hesabındaki bir Otomasyon sertifikası varlığı.
* Subscription name, subscriptionId ve certificate asset name öğelerini tutan **AzureClassicRunAsConnection** adlı, belirtilen Otomasyon hesabındaki bir Otomasyon bağlantı varlığı.

Klasik Farklı Çalıştır için otomatik olarak imzalanan sertifikayı kullanma seçeneğini belirlerseniz, betik otomatik olarak imzalanan bir yönetim sertifikası oluşturur ve bilgisayarınızda PowerShell oturumunu yürütmek için kullanılan kullanıcı profili altındaki geçici dosyalar klasörüne kaydeder - *%USERPROFILE%\AppData\Local\Temp*.  Komut dosyası yürütme sonrasında Otomasyon hesabının oluşturulduğu abonelik için yönetim deposuna Azure yönetim sertifikasını yüklemeniz gerekir.  Aşağıdaki adımlar komut dosyası yürütme ve sertifikayı karşıya yükleme işleminde size kılavuzluk edecektir.  

1. Aşağıdaki betiği bilgisayarınıza kaydedin.  Bu örnekte **New-RunAsAccount.ps1** dosya adıyla kaydedin.  
   
        #Requires -RunAsAdministrator
        Param (
        [Parameter(Mandatory=$true)]
        [String] $ResourceGroup,

        [Parameter(Mandatory=$true)]
        [String] $AutomationAccountName,

        [Parameter(Mandatory=$true)]
        [String] $ApplicationDisplayName,

        [Parameter(Mandatory=$true)]
        [String] $SubscriptionId,

        [Parameter(Mandatory=$true)]
        [Boolean] $CreateClassicRunAsAccount,

        [Parameter(Mandatory=$true)]
        [String] $SelfSignedCertPlainPassword,
 
        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPathForRunAsAccount,

        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPlainPasswordForRunAsAccount,

        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPathForClassicRunAsAccount,

        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPlainPasswordForClassicRunAsAccount,

        [Parameter(Mandatory=$false)]
        [ValidateSet("AzureCloud","AzureUSGovernment")]
        [string]$EnvironmentName="AzureCloud",

        [Parameter(Mandatory=$false)]
        [int] $NoOfMonthsUntilExpired = 12
        )

        function CreateSelfSignedCertificate([string] $keyVaultName, [string] $certificateName, [string] $selfSignedCertPlainPassword,[string] $certPath, [string] $certPathCer, [string] $noOfMonthsUntilExpired ) {
        $Cert = New-SelfSignedCertificate -DnsName $certificateName -CertStoreLocation cert:\LocalMachine\My -KeyExportPolicy Exportable -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider"

        $CertPassword = ConvertTo-SecureString $selfSignedCertPlainPassword -AsPlainText -Force
        Export-PfxCertificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $certPath -Password $CertPassword -Force | Write-Verbose
        Export-Certificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $certPathCer -Type CERT | Write-Verbose 
        }

        function CreateServicePrincipal([System.Security.Cryptography.X509Certificates.X509Certificate2] $PfxCert, [string] $applicationDisplayName) {  
        $CurrentDate = Get-Date
        $keyValue = [System.Convert]::ToBase64String($PfxCert.GetRawCertData())
        $KeyId = (New-Guid).Guid 

        $KeyCredential = New-Object  Microsoft.Azure.Commands.Resources.Models.ActiveDirectory.PSADKeyCredential
        $KeyCredential.StartDate = $CurrentDate
        $KeyCredential.EndDate= [DateTime]$PfxCert.GetExpirationDateString()
        $KeyCredential.KeyId = $KeyId
        $KeyCredential.CertValue  = $keyValue

        # Use Key credentials and create AAD Application
        $Application = New-AzureRmADApplication -DisplayName $ApplicationDisplayName -HomePage ("http://" + $applicationDisplayName) -IdentifierUris ("http://" + $KeyId) -KeyCredentials $KeyCredential
        $ServicePrincipal = New-AzureRMADServicePrincipal -ApplicationId $Application.ApplicationId 
        $GetServicePrincipal = Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id
   
        # Sleep here for a few seconds to allow the service principal application to become active (should only take a couple of seconds normally)
        Sleep -s 15
        $NewRole = New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
        $Retries = 0;
        While ($NewRole -eq $null -and $Retries -le 6)
        {
           Sleep -s 10
           New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId | Write-Verbose -ErrorAction SilentlyContinue
           $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
           $Retries++;
        }
           return $Application.ApplicationId.ToString();
        }

        function CreateAutomationCertificateAsset ([string] $resourceGroup, [string] $automationAccountName, [string] $certifcateAssetName,[string] $certPath, [string] $certPlainPassword, [Boolean] $Exportable) {
        $CertPassword = ConvertTo-SecureString $certPlainPassword -AsPlainText -Force   
        Remove-AzureRmAutomationCertificate -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Name $certifcateAssetName -ErrorAction SilentlyContinue
        New-AzureRmAutomationCertificate -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Path $certPath -Name $certifcateAssetName -Password $CertPassword -Exportable:$Exportable  | write-verbose
        }

        function CreateAutomationConnectionAsset ([string] $resourceGroup, [string] $automationAccountName, [string] $connectionAssetName, [string] $connectionTypeName, [System.Collections.Hashtable] $connectionFieldValues ) {
        Remove-AzureRmAutomationConnection -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Name $connectionAssetName -Force -ErrorAction SilentlyContinue
        New-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $automationAccountName -Name $connectionAssetName -ConnectionTypeName $connectionTypeName -ConnectionFieldValues $connectionFieldValues 
        }

        Import-Module AzureRM.Profile
        Import-Module AzureRM.Resources

        $AzureRMProfileVersion= (Get-Module AzureRM.Profile).Version
        if (!(($AzureRMProfileVersion.Major -ge 2 -and $AzureRMProfileVersion.Minor -ge 1) -or ($AzureRMProfileVersion.Major -gt 2)))
        {
          Write-Error -Message "Please install the latest Azure PowerShell and retry. Relevant doc url : https://docs.microsoft.com/powershell/azureps-cmdlets-docs/ "
          return
        }
 
        Login-AzureRmAccount -EnvironmentName $EnvironmentName
        $Subscription = Select-AzureRmSubscription -SubscriptionId $SubscriptionId

        # Create Run As Account using Service Principal
        $CertifcateAssetName = "AzureRunAsCertificate"
        $ConnectionAssetName = "AzureRunAsConnection"
        $ConnectionTypeName = "AzureServicePrincipal"
 
        if ($EnterpriseCertPathForRunAsAccount -and $EnterpriseCertPlainPasswordForRunAsAccount) {
           $PfxCertPathForRunAsAccount = $EnterpriseCertPathForRunAsAccount
           $PfxCertPlainPasswordForRunAsAccount = $EnterpriseCertPlainPasswordForRunAsAccount
        } else {
           $CertificateName = $AutomationAccountName+$CertifcateAssetName
           $PfxCertPathForRunAsAccount = Join-Path $env:TEMP ($CertificateName + ".pfx")
           $PfxCertPlainPasswordForRunAsAccount = $SelfSignedCertPlainPassword
           $CerCertPathForRunAsAccount = Join-Path $env:TEMP ($CertificateName + ".cer")
           CreateSelfSignedCertificate $KeyVaultName $CertificateName $PfxCertPlainPasswordForRunAsAccount $PfxCertPathForRunAsAccount $CerCertPathForRunAsAccount $NoOfMonthsUntilExpired
        }

        # Create Service Principal
        $PfxCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($PfxCertPathForRunAsAccount, $PfxCertPlainPasswordForRunAsAccount)
        $ApplicationId=CreateServicePrincipal $PfxCert $ApplicationDisplayName

         # Create the automation certificate asset
         CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $CertifcateAssetName $PfxCertPathForRunAsAccount $PfxCertPlainPasswordForRunAsAccount $true

         # Populate the ConnectionFieldValues
         $SubscriptionInfo = Get-AzureRmSubscription -SubscriptionId $SubscriptionId
         $TenantID = $SubscriptionInfo | Select TenantId -First 1
         $Thumbprint = $PfxCert.Thumbprint
         $ConnectionFieldValues = @{"ApplicationId" = $ApplicationId; "TenantId" = $TenantID.TenantId; "CertificateThumbprint" = $Thumbprint; "SubscriptionId" = $SubscriptionId} 

         # Create a Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
         CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ConnectionAssetName $ConnectionTypeName $ConnectionFieldValues

        if ($CreateClassicRunAsAccount) {
           # Create Run As Account using Service Principal
           $ClassicRunAsAccountCertifcateAssetName = "AzureClassicRunAsCertificate"
           $ClassicRunAsAccountConnectionAssetName = "AzureClassicRunAsConnection"
           $ClassicRunAsAccountConnectionTypeName = "AzureClassicCertificate "
           $UploadMessage = "Please upload the .cer format of #CERT# to the Management store by following the steps below." + [Environment]::NewLine +
                    "Log in to the Microsoft Azure Management portal (https://manage.windowsazure.com) and select Settings -> Management Certificates." + [Environment]::NewLine +
                    "Then click Upload and upload the .cer format of #CERT#" 
 
            if ($EnterpriseCertPathForClassicRunAsAccount -and $EnterpriseCertPlainPasswordForClassicRunAsAccount ) {
            $PfxCertPathForClassicRunAsAccount = $EnterpriseCertPathForClassicRunAsAccount
            $PfxCertPlainPasswordForClassicRunAsAccount = $EnterpriseCertPlainPasswordForClassicRunAsAccount
            $UploadMessage = $UploadMessage.Replace("#CERT#", $PfxCertPathForClassicRunAsAccount)
         } else {
            $ClassicRunAsAccountCertificateName = $AutomationAccountName+$ClassicRunAsAccountCertifcateAssetName
            $PfxCertPathForClassicRunAsAccount = Join-Path $env:TEMP ($ClassicRunAsAccountCertificateName + ".pfx")
            $PfxCertPlainPasswordForClassicRunAsAccount = $SelfSignedCertPlainPassword
            $CerCertPathForClassicRunAsAccount = Join-Path $env:TEMP ($ClassicRunAsAccountCertificateName + ".cer")
            $UploadMessage = $UploadMessage.Replace("#CERT#", $CerCertPathForClassicRunAsAccount)
            CreateSelfSignedCertificate $KeyVaultName $ClassicRunAsAccountCertificateName $PfxCertPlainPasswordForClassicRunAsAccount $PfxCertPathForClassicRunAsAccount $CerCertPathForClassicRunAsAccount $NoOfMonthsUntilExpired
         }

         # Create the automation certificate asset
         CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountCertifcateAssetName $PfxCertPathForClassicRunAsAccount $PfxCertPlainPasswordForClassicRunAsAccount $false

         # Populate the ConnectionFieldValues
         $SubscriptionName = $subscription.Subscription.SubscriptionName
         $ClassicRunAsAccountConnectionFieldValues = @{"SubscriptionName" = $SubscriptionName; "SubscriptionId" = $SubscriptionId; "CertificateAssetName" = $ClassicRunAsAccountCertifcateAssetName}

         # Create a Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
         CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountConnectionAssetName $ClassicRunAsAccountConnectionTypeName $ClassicRunAsAccountConnectionFieldValues

         Write-Host -ForegroundColor red $UploadMessage
         }

2. Bilgisayarınızda **Windows PowerShell**’i yükseltilmiş kullanıcı haklarına sahip **Başlat** ekranından başlatın.
3. Yükseltilmiş PowerShell komut satırı kabuğundan, 1. adımda oluşturduğunuz betiği içeren klasöre gidin ve ihtiyacınız olan parametre yapılandırmasına göre gerekli parametre değerleri ayarlayarak betiği yürütün.  

    **Otomatik olarak imzalanan sertifika kullanarak Azure Farklı Çalıştır hesabı oluşturma**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword>` 

    **Otomatik olarak imzalanan sertifika kullanarak Azure Farklı Çalıştır hesabı ve Azure Klasik Farklı Çalıştır hesabı oluşturma**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true`

    **Kuruluş sertifikası kullanarak Azure Farklı Çalıştır hesabı ve Azure Klasik Farklı Çalıştır hesabı oluşturma**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication>  -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true -EnterpriseCertPathForRunAsAccount <EnterpriseCertPfxPathForRunAsAccount> -EnterpriseCertPlainPasswordForRunAsAccount <StrongPassword> -EnterpriseCertPathForClassicRunAsAccount <EnterpriseCertPfxPathForClassicRunAsAccount> -EnterpriseCertPlainPasswordForClassicRunAsAccount <StrongPassword>`

    **Azure Kamu bulutunda otomatik olarak imzalanan sertifika kullanarak Azure Farklı Çalıştır hesabı ve Azure Klasik Farklı Çalıştır hesabı oluşturma**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true  -EnvironmentName AzureUSGovernment`
 
    > [!NOTE]
    > Betiği yürüttükten sonra Azure’ün kimlik doğrulamasını yapmanız istenecektir. Abonelik Yöneticileri rolünün üyesi ve aboneliğin ortak yöneticisi olan bir hesapla oturum açmanız gerekir.
    > 
    > 

Betik başarıyla tamamlandıktan sonra, Klasik Farklı Çalıştır hesabı oluşturduysanız, kullanıcı profilinizin **Temp** klasöründe oluşturulan sertifikayı kopyalamanız gerekir.  Klasik Azure portalına [yönetim API sertifikası yükleme](../azure-api-management-certs.md) adımlarını izleyin ve ardından [örnek koduna](#sample-code-to-authenticate-with-service-management-resources) bakarak Service Management kaynaklarıyla kimlik doğrulama yapılandırmasını doğrulayın.  Klasik Farklı Çalıştır hesabı oluşturmadıysanız, Resource Manager kaynaklarının kimliğini doğrulamak ve kimlik bilgisi yapılandırmasını doğrulamak için aşağıdaki [örnek koduna](#sample-code-to-authenticate-with-resource-manager-resources) bakın veya Servis Yönetimi kaynaklarıyla kimlik bilgisi yapılandırmasını doğrulamak için [örnek koduna](#sample-code-to-authenticate-with-service-management-resources) bakın.

## <a name="sample-code-to-authenticate-with-resource-manager-resources"></a>Resource Manager kaynaklarıyla kimlik doğrulaması için örnek kod
Runbook’larınızla Resource Manager kaynaklarını yönetecek Farklı Çalıştır hesabını kullanarak kimlik doğrulaması yapmak için **AzureAutomationTutorialScript** örnek runbook’undan alınan aşağıdaki güncelleştirilmiş örnek kodu kullanabilirsiniz.   

    $connectionName = "AzureRunAsConnection"
    $SubId = Get-AutomationVariable -Name 'SubscriptionId'
    try
    {
       # Get the connection "AzureRunAsConnection "
       $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         

       "Signing in to Azure..."
       Add-AzureRmAccount `
         -ServicePrincipal `
         -TenantId $servicePrincipalConnection.TenantId `
         -ApplicationId $servicePrincipalConnection.ApplicationId `
         -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint
       "Setting context to a specific subscription"     
       Set-AzureRmContext -SubscriptionId $SubId              
    }
    catch {
        if (!$servicePrincipalConnection)
        {
           $ErrorMessage = "Connection $connectionName not found."
           throw $ErrorMessage
         } else{
            Write-Error -Message $_.Exception
            throw $_.Exception
         }
    }


Bu betikte, abonelik bağlamına başvuruyu desteklemek için iki ek kod satırı bulunmaktadır; böylece birden fazla abonelikte kolayca çalışabilirsiniz. SubscriptionId adlı değişken varlığında aboneliğin kimliği vardır; Add-AzureRmAccount cmdlet’i deyiminden sonra da [Set-AzureRmContext cmdlet’i](https://msdn.microsoft.com/library/mt619263.aspx) *-SubscriptionId* parametre kümesiyle belirtilir. Değişken adı çok genelse, amaçlarınız doğrultusunda tanımlanmasını kolaylaştırmak amacıyla bir önek veya başka diğer adlandırma kuralı eklemek için değişken adını gözden geçirebilirsiniz. Alternatif olarak, ilgili değişken varlığıyla -SubscriptionId yerine -SubscriptionName parametre kümesini kullanabilirsiniz.    

Runbook’ta kimlik doğrulaması için kullanılan cmdlet’in (**Add-AzureRmAccount**) *ServicePrincipalCertificate* parametre kümesini kullandığına dikkat edin.  Kimlik bilgilerini değil, hizmet asıl sertifikasını kullanarak kimlik doğrulamasını yapar.  

## <a name="sample-code-to-authenticate-with-service-management-resources"></a>Service Management kaynaklarıyla kimlik doğrulaması için örnek kod
Runbook’larınızla klasik kaynakları yönetecek Klasik Farklı Çalıştır hesabını kullanarak kimlik doğrulaması yapmak için **AzureClassicAutomationTutorialScript** örnek runbook’undan alınan aşağıdaki güncelleştirilmiş örnek kodu kullanabilirsiniz.

    $ConnectionAssetName = "AzureClassicRunAsConnection"
    # Get the connection
    $connection = Get-AutomationConnection -Name $connectionAssetName        

    # Authenticate to Azure with certificate
    Write-Verbose "Get connection asset: $ConnectionAssetName" -Verbose
    $Conn = Get-AutomationConnection -Name $ConnectionAssetName
    if ($Conn -eq $null)
    {
       throw "Could not retrieve connection asset: $ConnectionAssetName. Assure that this asset exists in the Automation account."
    }

    $CertificateAssetName = $Conn.CertificateAssetName
    Write-Verbose "Getting the certificate: $CertificateAssetName" -Verbose
    $AzureCert = Get-AutomationCertificate -Name $CertificateAssetName
    if ($AzureCert -eq $null)
    {
       throw "Could not retrieve certificate asset: $CertificateAssetName. Assure that this asset exists in the Automation account."
    }

    Write-Verbose "Authenticating to Azure with certificate." -Verbose
    Set-AzureSubscription -SubscriptionName $Conn.SubscriptionName -SubscriptionId $Conn.SubscriptionID -Certificate $AzureCert
    Select-AzureSubscription -SubscriptionId $Conn.SubscriptionID

## <a name="next-steps"></a>Sonraki adımlar
* Hizmet Sorumluları hakkında daha fazla bilgi için bkz. [Uygulama Nesneleri ve Hizmet Sorumlusu Nesneleri](../active-directory/active-directory-application-objects.md).
* Azure Automation’da Rol Tabanlı Erişim Denetimi hakkında daha fazla bilgi için bkz. [Azure Automation’da rol tabanlı erişim denetimi](automation-role-based-access-control.md).
* Sertifikalar ve Azure hizmetleri hakkında daha fazla bilgi için bkz. [Azure Cloud Services sertifikalarına genel bakış](../cloud-services/cloud-services-certs-create.md)


