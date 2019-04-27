---
title: Müşteri adayı yönetimi için bulut Marketi | Azure Market ve AppSource
description: Genel bir bakış sunar ve teknik yapıtlar için Azure Market ve AppSource yayımlamayla ilgili çeşitli konuları
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
documentationcenter: ''
author: yijenj
manager: nunoc
editor: ''
ms.assetid: e8d228c8-f9e8-4a80-9319-7b94d41c43a6
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 10/05/2018
ms.author: yijenj
ms.openlocfilehash: 810298fc45becf132da6f082df7ad33e7af828aa
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60769898"
---
# <a name="lead-management-for-cloud-marketplace"></a>Bulut Marketi Yönetimi sağlama


İyi bir iş merkezi müşterilerdir. Günümüzün ürün satın almalar dönüşümünde Pazarlamacılar müşterilerle doğrudan bağlanma ve bir ilişki oluşturmak gerekir. Yüksek kaliteli müşteri adayı oluşturma satış döngünüz için önemli bir araç olan nedeni budur. Teklife listeleme sonra [bulut iş ortağı portalı](https://cloudpartner.azure.com/), bir müşteri vade farkı ifade eder veya ürününüzü dağıtır hemen sonra program aracılığıyla müşteri iletişim bilgilerini alabilmeniz için etkinleştirilmiş araç vardır Market. 



## <a name="what-are-leads-in-the-marketplace"></a>Hangi Market'te geliyor?

Müşteri adayları ile ilgilenen müşterilerin olan ve ürünlerinizi marketten dağıtımı. Azure Market veya Appsource'ta ürününüzü listelenip listelenmediğini, müşterilerden müşteri adayları, düzgün bir şekilde, CRM için bulut iş ortağı Portalı'nda, listing(s) ayarlandıktan almak mümkün olmayacak 


## <a name="how-to-connect-your-crm-system-with-the-cloud-partner-portal"></a>Bulut iş ortağı portalı ile CRM sisteminize bağlanma

Müşteri adayları alma başlatmak için bulut iş ortağı portalında müşteri adayı Yönetimi Bağlayıcısı CRM bilgilerinizle kullanılabilir CRM sisteminin bir listesine kolayca takılabilen şekilde tasarlanmıştır. Artık bir dış sistemle tümleştirmek için önemli bir mühendislik çabası olmadan Market tarafından oluşturulan müşteri adayları kolayca yararlanabilirsiniz.

Her olası müşteri adayı hedeflere bağlanmak adım adım yönergeler şunlardır:

**Dynamics CRM Online** - [Buraya](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-dynamics) Dynamics CRM Online müşteri adayları almak için nasıl yapılandırılacağı hakkında yönergeler alın.

**Marketo** - [Buraya](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-marketo) adayları alma Marketo sağlama yapılandırmasını ayarlama yönergeleri almak için.

**Salesforce** - [Buraya](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-salesforce) adayları alma Salesforce örneğinizin ayarlamaya yönelik yönergeler alın.

**Azure tablo** – [Buraya](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-azure-table) bir Azure tablosu müşteri adayları alma için Azure depolama hesabınızı ayarlamaya yönelik yönergeler alın.

**HTTPS uç noktası** – [Buraya](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-https) müşteri adayları almak, Https uç noktası ayarlamaya yönelik yönergeler alın.

Müşteri adayı hedefiniz doğru şekilde yapılandırdığınızdan ve yayımlama teklifinizi sınırınıza sonra bağlantıyı doğrulama eder ve bir test müşteri adayı Gönder. Teklif kendiniz Önizleme ortamı elde etmeye çalışarak, kullanıma sunulmadan önce teklif görüntülerken, müşteri adayı bağlantınızı sınayabilirsiniz. Böylece tüm müşteri adaylarını kaybetmeyin güncel, müşteri adayı ayarları kalın bu nedenle bir şey sizin değiştirildiğinde bu bağlantıları güncelleştirme emin olun, emin olmak önemlidir.


### <a name="what-are-the-next-steps"></a>Sonraki adımlar nelerdir?

Ayarlanan teknik yerleştirildikten sonra bu müşteri adayları geçerli satış ve pazarlama stratejisi ve çalışma süreçleri eklemeniz gerekir. Biz, genel satış sürecini daha iyi anlamak isteyen ve yakından yüksek kaliteli müşteri adaylarını ve başarılı olmasını sağlamak için yeterli veri sağlamak sizinle birlikte çalışmak istediğiniz. Nasıl size en iyi duruma getirme ve bu müşterilerin başarılı olmasına yardımcı olmak için ek veri göndereceğiz müşteri adaylarını geliştirmek üzerinde bildirimleriniz bizim için değerli. İlgili geri bildirim ve öneriler Market müşteri adayları ile daha başarılı olması Satış ekibinize etkinleştirmek için sağlama konusunda bize bildirin.



## <a name="common-lead-configuration-errors-during-publishing-on-cloud-partner-portal"></a>Bulut iş ortağı portalı yayımlama sırasında sık karşılaşılan müşteri adayı yapılandırma hatalar 

**Dynamics CRM'e müşteri adayı kaydedilemedi. Dynamics CRM hesap ayarlarını denetleyin. LastCRMError: Dynamics CRM, LastCRMException oturum açılamıyor:** 

> O365 kimlik doğrulaması seçildiğinde, kullanıcı hesabı ve parolası olup olmadığını geçerli denetleyin. AAD seçildiyse, Kiracı kimliği, uygulama kimliği ve uygulama gizli anahtarı eşleşme ne AAD'de ayarlanmıştır olmadığını denetleyin. Yönergeleri izleyerek [burada](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-dynamics). Hesap kullanıcı adı/parola geçerliyse, Dynamics 365 erişiminin ve bir lisans atanmış (adımları kullanarak Office kullanıcı Azure Active Directory veya güvenlik ayarları kullanılarak, 11-15) emin olun. 

 
**Dynamics CRM'e müşteri adayı kaydedilemedi. Kullanıcı oluşturma izni yok leadsourcecode özniteliği için Müşteri Adayı varlığı** 

> Uygulama/kullanıcı Microsoft Marketplace sağlama yazıcı için güvenlik rolleri eksik. Azure Active Directory veya güvenlik ayarları Office kullanıcı kullanıyorsanız kullanıyorsanız 11-15 adımları izleyin [burada](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-dynamics).

**AAD kullanılarak Dynamics CRM'e müşteri adayı kaydedilemedi. Özel durum:: Kiracı bulunamadı. Bu örnek, Kiracı ilişkin hiçbir etkin aboneliğin varsa ortaya çıkabilir.**  

> Müşteri adayı Yönetim bölümünde sağlanan dizin kimliği geçerli bir dizin değil. Lütfen 2. adım yönergelerini temel dizin kimliği Al (Azure Active Directory altında gelen [burada](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-dynamics) 

**Dynamics CRM'e müşteri adayı kaydedilemedi. LastCRMError: Başarısız oldu - SecLib::RetrievePrivilegeForUser kullanıcıya rol atanır.**  

> Çözüm: Microsoft Marketplace sağlama yazıcısına güvenlik rolü atayın. Yönergeleri izleyerek [burada](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-dynamics) güvenlik ayarları 

**AAD kullanılarak Dynamics CRM'e müşteri adayı kaydedilemedi. Özel durum:: Uygulama tanımlayıcısı ile dizinde bulunamadı.** 

> Müşteri adayı Yönetim bölümünde sağlanan uygulama kimliği, geçerli bir dizin değil. Lütfen adım 8 yönergelerini temel dizin kimliği Al (Azure Active Directory altında gelen [burada](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-dynamics)). 

**AAD kullanılarak Dynamics CRM'e müşteri adayı kaydedilemedi. Özel durum:: İstenen Kiracı tanımlayıcısı, geçerli ve geçersiz dış etki alanı biçimi değil** 

> Müşteri adayı Yönetim bölümünde sağlanan dizin kimliği geçerli bir dizin değil. Lütfen 2. adım yönergelerini temel dizin kimliği Al (Azure Active Directory altında gelen [burada](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-dynamics)). 

**AAD kullanılarak Dynamics CRM'e müşteri adayı kaydedilemedi. Özel durum:: Kimlik bilgilerini doğrulama hatası.: Geçersiz istemci gizli anahtarı sağlanır.** 

> Çözüm: Azure portalında oturum açın, uygulama anahtarı nedir bulut iş ortağı Portalı'nda eşleşip eşleşmediğini kontrol edin. Lütfen dan 10. adımı (altında Azure Active Directory), yönerge göre parola oluştur [burada](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-dynamics)). 

**Dynamics CRM'e müşteri adayı kaydedilemedi. LastCRMError: İstek kanalı, 00:02:00 sonra bir yanıtı beklenirken zaman aşımına uğradı. Request çağrısına geçirilen zaman aşımı değerini artırın ya da Binding üstündeki SendTimeout değerini artırın. Bu işlem için ayrılan süre daha uzun bir zaman aşımı değerinin bir bölümü olabilir.**  

> Çözüm: Bulut iş ortağı portalında oturum açın, mağaza ayrıntılarını kontrol edin >> sağlama hedef >> URL, geçerli bir dinamik CRM örneği olup olmadığını denetleyin

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

**Müşteri adayları ve neden önemlidir marketi'ndeki yayımcı olarak nelerdir?** 

Müşteri adayları, ürünleri Market dağıtan müşterilerdir. Ürününüzü üzerinde listelenip listelenmediğini [Azure Marketi](https://azuremarketplace.microsoft.com/en-us) veya [AppSource](https://appsource.microsoft.com/), müşteri adayları, müşteri adayı hedef Kurulum teklifinizi varsa, ürününüzü ilgilenen müşterilerin almak mümkün olacaktır.  


**Benim müşteri adayı hedef ayarlama konusunda Yardım nereden alabilirim?** 

Burada sağlanan belgelerde bulabilirsiniz: [Müşteri adayları alma](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-get-customer-leads) aka.ms/marketplacepublishersupport select Teklif türü üzerinden bir destek bileti gönderin ve müşteri adayı yönetim. 



**Bir müşteri adayı hedef Marketi'nde teklif yayımlamak için yapılandırma gerekli?**

Evet, bir kişi bana SaaS uygulaması veya danışmanlık hizmetleri yayımlıyorsanız.  
 


**Müşteri adayı yapılandırmasının doğru olduğundan nasıl doğrulayabilirim?**

Teklif ve müşteri adayı hedef ayarladıktan sonra teklifinizi yayımlayın. Müşteri adayı doğrulama adımında, Market teklife yapılandırılmış sağlama hedef test müşteri adayı gönderir. 


**Test müşteri adayına nasıl bulabilirim?**


Arama "MSFT_TEST için" Müşteri adayı hedefiniz içinde bir örnek test müşteri adayı verilerini şu şekildedir: 

company = MSFT_TEST_636573304831318844 

Ülke = US 

Açıklama MSFT_TEST_636573304831318844 = 

e-posta = MSFT_TEST_636573304831318844@test.com

kodlama = UTF-8 

kodlama = UTF-8 

first_name MSFT_TEST_636573304831318844 = 

last_name = MSFT_TEST_636573304831318844 

lead_source = MSFT_TEST_636573304831318844-MSFT_TEST_636573304831318844|<Offer Name> 

oid = 00Do0000000ZHog 

Telefon 1234567890 = 

title = MSFT_TEST_636573304831318844 

 

**Canlı bir teklif sahibim, ancak tüm müşteri adaylarını görmüyorum?**

Her sağlama veri alanları, Seçilen müşteri adayı hedef iletilen gerekir, müşteri adaylarını şu biçimde gelir: **Kaynak eylem | Teklif** 

  *Kaynakları:*

    “AzureMarketplace”, 
    “AzurePortal”, 
    “TestDrive”,  
    “SPZA” (acronym for AppSource) 

  *Eylemler:*

    “INS” – Stands for Installation. This is on Azure Marketplace or AppSource whenever a customer hits the button to acquire your product. 
    “PLT” – Stands for Partner Led Trial. This is on AppSource whenever a customer hits the Contact me button. 

    “DNC” – Stands for Do Not Contact. This is on AppSource whenever a Partner who was cross listed on your app page gets requested to be contacted. We are sharing the heads up that this customer was cross listed on your app, but they do not need to be contacted. 

    “Create” – This is inside Azure Portal only and is whenever a customer purchases your offer to their account. 

    “StartTestDrive” – This is for Test Drives only and is whenever a customer starts their test drive. 


  *Sunar:*

    “checkpoint.check-point-r77-10sg-byol”, 
    “bitnami.openedxcypress”, 
    “docusign.3701c77e-1cfa-4c56-91e6-3ed0b622145a” 

 

  *Müşteri bilgileri, örnek veriler aşağıdadır.*

    { 

    "FirstName":"John", 

    "LastName":"Smith", 

    "Email":"jsmith@microsoft.com", 

    "Phone":"1234567890", 

    "Country":"US", 

    "Company":"Microsoft", 

    "Title":"CTO" 

    } 

Altında daha fazla bilgi edinin [müşteri adayı bilgileri](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-get-customer-leads). 


**Müşteri adayı neden göremiyorum benim müşteri adayı hedef Azure BLOB yapılandırmış olduğunuz?** 

Müşteri adayı hedef olarak Azure BLOB Depolama seçtiğinizde müşteri adayına yalnızca yazılan. Müşteri adayı gerçek zamanlı olarak almak için Azure tablo için geçiş 


**Müşteri adayı my CRM'de neden bulamıyorum Market'ten bir e-posta aldım?**  

Bu, son kullanıcının e-posta etki .edu olduğunu mümkündür. Gizlilik nedeniyle biz .edu etki alanından PII verileri geçirmeyin. Aka.ms/marketplacepublishersupport üzerinden bir destek bileti gönderin 


 **Benim müşteri adayı hedef olarak Azure tablo/Azure BLOB yapılandırdınız, müşteri adaylarını nasıl görüntüleyebilirim?** 

Azure Portalı'ndan blob veya tablo erişebilir veya indirip yükleyebilirsiniz [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/) ücretsiz, Azure depolama hesabınızın tablolar/BLOB'ları görüntülemek için. 


**Benim müşteri adayı hedef olarak Azure tablo yapılandırdınız, her yeni müşteri adayı, Market tarafından gönderilen bildirim alabilirim?** 

Evet, ayarlamak için yönergeleri izleyin. Azure tablo + işlev belgelerine yukarı [burada](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-azure-table). 



**Salesforce müşteri adaylarını neden bulamıyorum benim müşteri adayı hedefi olarak yapılandırılmış?** 

Form müşteri adayı için web tabanlı bir seçim listesi üzerinde zorunlu alan olup olmadığını denetleyin. Yanıt Evet ise alanın üzerine bir zorunlu olmayan metin alanına geçin.  
 

**Benim müşteri adayı hedef ile ilgili bir sorun oluştu ve bazı müşteri adayları ı eksik. Bunları bana bir e-postayla gönderilen olabilir mi?** 

PII (özel olarak tanımlanabilir bilgiler) ilkeleri nedeniyle, biz müşteri adayı bilgileri güvenli olmayan bir e-posta paylaşamazsınız. 



**Benim müşteri adayı hedef olarak Azure Storage (BLOB/tablo) nasıl maliyeti yapılandırmış olduğunuz?** 

Müşteri adayı genel verilerdir düşük (< 1 GB neredeyse tüm yayımcılar için). Maliyet alınan, müşteri adaylarını sayısına bağlıdır bir ayda 1.000 müşteri adayları alınırsa, yaklaşık 50 Sent tutarında maliyetlerini. 
