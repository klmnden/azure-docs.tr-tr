<properties
   pageTitle="Azure Automation Güvenliği"
   description="Bu makalede, Azure Automation’da Automation Hesapları için uygun Automation güvenliği ve farklı kumluk doğrulasa yöntemlerine genel bakış verilmektedir."
   services="automation"
   documentationCenter=""
   authors="MGoedtel"
   manager="jwhit"
   editor="tysonn"
   keywords="automation security, secure automation" />
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/10/2016"
   ms.author="magoedte" />

# Azure Automation Güvenliği
Azure Automation, Azure’deki, şirket içindeki kaynaklara karşı ve Amazon Web Hizmetleri (AWS) gibi diğer bulut sağlayıcılarıyla görevleri otomatikleştirmenizi sağlar.  Runbook'un gerekli işlemlerini gerçekleştirebilmesi için, abonelikte gereken en düşük haklara sahip kaynaklara güvenli erişim izinlerinin olması gerekir.  
Bu makale, Azure Automation’ın desteklediği çeşitli kimlik doğrulaması senaryolarını kapsamakta ve yönetmeniz gereken ortam veya ortamlar temelinde nasıl başlayacağınızı göstermektedir.  

## Automation Hesabına Genel Bakış
Azure Automation’ı ilk kez başlattığınızda, en az bir Automation hesabı oluşturmanız gerekir. Automation hesapları Automation kaynaklarınızı (runbook'lar, varlıklar, yapılandırmalar) diğer Automation hesaplarında yer alan kaynaklarından yalıtmanızı sağlar. Kaynaklarını ayrı mantıksal ortamlara ayırmak için Automation hesaplarını kullanabilirsiniz. Örneğin, geliştirme için bir hesap, üretim için başka bir hesap ve şirket içi ortamınız için de başka bir hesap kullanabilirsiniz.  Azure Automation hesabı, Azure aboneliğinizde oluşturduğunuz Microsoft hesabı veya hesaplarından farklıdır.

Her Automation hesabı için Automation kaynakları tek bir Azure bölgesiyle ilişkilendirilir, ancak Automation hesapları tüm bölgelerdeki kaynakları yönetebilir. Farklı bölgelerde Automation hesapları oluşturmanın temel nedeni, veri ve kaynakların belirli bir bölgede yalıtılmasını gerektiren ilkelere sahip olmanız olabilir.

>[AZURE.NOTE]Azure portalında oluşturulan Automation hesapları ve içerdikleri kaynaklara Klasik Azure portalında erişilemez. Bu hesapları veya kaynaklarını Windows PowerShell’le yönetmek istiyorsanız, Azure Resource Manager modüllerini kullanmanız gerekir.

Azure Resource Manager (ARM) ve Azure Automation’daki Azure cmdlet'lerini kullanan kaynaklara karşı gerçekleştirdiğiniz görevlerin tümü, Azure Active Directory kuruluş kimliğini kullanarak Azure’de kimlik bilgileri tabanlı kimlik doğrulamasını doğrulamalıdır.  Sertifika tabanlı kimlik doğrulaması Azure Hizmet Yönetimi (ASM) moduyla asıl kimlik doğrulaması yöntemi olsa da, bunun kurulması karmaşıktır.  Azure AD kullanıcısının bulunduğu Azure’de kimlik doğrulaması, 2014’te yalnızca Kimlik Doğrulaması hesabını sadeleştirmek amacıyla değil, hem ASM, hem de ARM modunda çalışan tek kullanıcı hesabıyla Azure’de etkileşimsiz kimlik doğrulamasını becerisini de destekler.   

Yakın zamanda başka bir güncelleştirme de yayımladık; burada, Automation hesabı oluşturulduğunda artık otomatik olarak Azure AD hizmet sorumlusu nesnesi de oluşturuyoruz. Bu, Azure Farklı Çalıştır hesabı olarak da bilinir ve Azure Resource Manager ile runbook otomasyonu için varsayılan kimlik doğrulaması yöntemidir.     

Rol tabanlı erişim denetimi, Azure AD kullanıcı hesabı ve hizmet sorumlusuna izin verilen eylemleri vermek, ve bu hizmet sorumlusunun kimliğini doğrulamak için ARM modunda kullanılabilir.  Automation izinlerinin yönetilmesi için modelinizin geliştirilmesine yardımcı olma hakkında daha fazla bilgi için lütfen [Azure Automation’da rol tabanlı erişim denetimi](../automation/automation-role-based-access-control.md) makalesini okuyun.  

Veri merkezindeki Karma Runbook Çalışanı’nda veya AWS’deki bilgi işlem hizmetlerine karşı çalışan runbook'lar, genel olarak Azure kaynaklarında kimlik doğrulayan runbook’lar için kullanılan yöntemin aynısını kullanamaz.  Bunun nedeni, bu kaynakların Azure dışında çalışmasıdır; sonuç olarak da, yerel olarak erişecekleri kaynakların kimliğini doğrulamak için Automation’da tanımlanan kendi güvenlik kimlik bilgileri gerekecektir.  

## Kimlik Doğrulaması Yöntemleri

Aşağıdaki tabloda, Azure Automation tarafından desteklenen her ortamla ilgili farklı kimlik doğrulaması yöntemleri ve runbook’larınızın nasıl ayarlanacağını anlatan makaledeki bilgiler özetlenmiştir.

Yöntem  |  Ortam  | Makale
----------|----------|----------
Azure AD Kullanıcı Hesabı | Azure Resource Manager ve Azure Hizmet Yönetimi | [Azure AD Kullanıcı hesabıyla Kimlik Doğrulaması Runbook’ları](../automation/automation-sec-configure-aduser-account.md)
Azure AD Hizmet Sorumlusu nesnesi | Azure Resource Manager | [Azure Farklı Çalıştır hesabıyla Kimlik Doğrulaması Runbook’ları](../automation/automation-sec-configure-azure-runas-account.md)
Windows Kimlik Doğrulaması | Şirket İçi Veri Merkezi | [Karma Runbook Çalışanları için Kimlik Doğrulaması Runbook’ları](../automation/automation-hybrid-runbook-worker.md)
AWS Kimlik Bilgileri | Amazon Web Hizmetleri | [Amazon Web Hizmetleri (AWS) ile Kimlik Doğrulaması Runbook'ları](../automation/automation-sec-configure-aws-account.md)



<!--HONumber=Jun16_HO2-->


