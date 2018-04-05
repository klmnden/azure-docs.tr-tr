---
title: Azure sistem durumu Analytics şeması
description: HIPAA/HITRUST sistem durumu Analytics şeması dağıtmak için yönergeler
services: security
documentationcenter: na
author: simorjay
manager: mbaldwin
editor: tomsh
ms.assetid: 26566e0a-0a54-49f4-a91d-48e20b7cef71
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/27/2018
ms.author: simorjay
ms.openlocfilehash: 700378d23f869427fb50b9dee5bcf8448ac73404
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="azure-security-and-compliance-blueprint---hipaahitrust-health-data-and-ai"></a>Azure güvenlik ve uyumluluk şeması - HIPAA/HITRUST sistem durumu verileri ve AI

## <a name="overview"></a>Genel Bakış

**Azure güvenlik ve uyumluluk şeması - HIPAA/HITRUST sistem durumu verileri ve AI Azure PaaS çözümünü nasıl güvenli bir şekilde alma, depolama, çözümlemek ve endüstri uyumluluk gereklerini karşılamak üzere kullanabilmeye devam ederken sistem durumu verileri ile etkileşim göstermek için anahtar teslim dağıtımını sağlar gereksinimleri. Şeması bulut benimseme ve düzenlenir veri sahip müşteriler için kullanımını artırmanıza yardımcı olur.**

Azure güvenlik ve uyumluluk şeması - HIPAA/HITRUST sistem durumu verileri ve AI şeması sağlayan araçlar ve güvenli dağıtmanıza yardımcı olan kılavuzlar, sağlık sigortası taşınabilirlik ve Sorumluluk Yasası (HIPAA) ve sistem durumu bilgileri güven Alliance (HITRUST) hazır alma, depolama, çözümleme ve kişisel ve kişisel olmayan tıbbi kayıtları bir uçtan uca çözümü dağıtılmış bir güvenli, çok katmanlı bulut ortamınızdaki ile etkileşim için hizmet olarak platform (PaaS) ortamı. Ortak bir başvuru mimarisi paylaşan ve Microsoft Azure benimsenmesi kolaylaştırmak için tasarlanmıştır. Sağlanan Bu mimarinin yükünü ve dağıtım maliyetini azaltmak için bulut tabanlı bir yaklaşım arayan kuruluşlar ihtiyaçlarını karşılamak için bir çözüm gösterilmektedir.

![](images/components.png)

Çözüm, hızlı sağlık birlikte çalışabilirlik kaynakları (FHIR), sağlık bilgilerini elektronik olarak değiştirmek için dünya çapında bir standart kullanılarak biçimlendirilmiş bir örnek veri kümesi kullanmak ve güvenli bir şekilde depolamak için tasarlanmıştır. Müşteriler, Azure Machine Learning güçlü iş zekası araçları ve örnek verilere yapılan tahminlerin gözden geçirmek için çözümlemeler yararlanmak için daha sonra kullanabilirsiniz. Azure Machine Learning kolaylaştırabilir deneme tür örnek olarak, ayrıntılı bir örnek veri kümesi, komut dosyaları ve bir Hastanenin tesiste hastanın kalma uzunluğu tahmin etmeye yönelik araçları içerir. 

Bu şeması, müşterilerin yeni Azure denemeler hem Klinik ve işletimsel kullanım örneği senaryosu çözmek için makine öğrenimi geliştirme kendi belirli gereksinimleri ayarlamak modüler bir temel olarak hizmet için tasarlanmıştır. Güvenli ve dağıtıldığında uyumlu olacak şekilde tasarlanmıştır; Ancak, müşteriler rolleri düzgün yapılandırmak ve değişiklikleri uygulamak için sorumlu değildir. Şunlara dikkat edin:

-   Bu ayrıntılı bir HITRUST, Microsoft Azure kullanmak müşterilere yardımcı olmak için bir temel sağlar ve HIPAA ortamı.

-   HIPAA ve (genel güvenlik çerçevesi aracılığıyla--CSF) HITRUST hizalanacak şeması tasarlanmış olsa da, HIPAA ve HITRUST sertifika gereksinimleri başına dış bir denetçi tarafından sertifikalı kadar uyumlu düşünülmemelidir.

-   Müşteriler, bu temel mimari kullanılarak oluşturulan herhangi bir çözümü uygun güvenlik ve uyumluluk değerlendirmelerini yürütmek için sorumludur.

## <a name="deploying-the-automation"></a>Otomasyon dağıtma

- Çözümü dağıtmak için dağıtım kılavuzunda sağlanan yönergeleri izleyin. 

[![](./images/deploy.png)](https://aka.ms/healthblueprintdeploy)

Bu çözümün nasıl çalıştığı ilişkin hızlı genel bakış için bu izleme [video](https://aka.ms/healthblueprintvideo) açıklayan ve onun dağıtım gösterilmesi.

- Sık sorulan sorular bulunabilir [SSS](https://aka.ms/healthblueprintfaq) Kılavuzu.

-   **Mimari diyagramı.** Şeması kullanılan referans mimarisi diyagramı gösterir ve örnek senaryosu kullanın.

-   **Dağıtım şablonları**. Bu dağıtımda [Azure Resource Manager şablonları](/azure/azure-resource-manager/resource-group-overview#template-deployment) Kurulum sırasında yapılandırma parametrelerini belirterek mimarisinin bileşenlerine Microsoft Azure otomatik olarak dağıtmak için kullanılan.

-   **[Dağıtım betikleri otomatik](https://aka.ms/healthblueprintdeploy)**. Bu komut dosyalarını çözümünü dağıtmanıza yardımcı olur. Komut dosyaları oluşur:


-   Bir modül yükleme ve [genel yönetici](/azure/active-directory/active-directory-assign-admin-roles-azure-portal) Kurulum komut dosyası yükleyin ve gerekli PowerShell modülleri ve genel yönetici rolleri doğru şekilde yapılandırıldığını doğrulamak için kullanılır. 
-   PowerShell komut dosyası yüklemesi, önceden oluşturulmuş demo işlevleri içeren bir .zip dosyası sağlanan bu çözümü dağıtmak için kullanılır.

## <a name="solution-components"></a>Çözüm bileşenleri


Temel mimari aşağıdaki bileşenlerden oluşur:

-   **[Tehdit modeli](https://aka.ms/healththreatmodel)**  kapsamlı tehdit modeli ile kullanılmak üzere tm7 biçimde sağlanmış [Microsoft tehdit modelleme aracı](https://www.microsoft.com/en-us/download/details.aspx?id=49168), çözümün bileşenlerinin gösteren, veriler bunları ve güven arasında akar sınırlar. Model müşteriler makine öğrenme bileşenleri veya başka değişiklikler geliştirirken sistem altyapısında potansiyel risk puanları anlamanıza yardımcı olabilir.

-   **[Müşteri uygulaması matris](https://aka.ms/healthcrmblueprint)**  bir Microsoft Excel çalışma kitabı ilgili HITRUST gereksinimleri listelenir ve Microsoft ve müşterinin nasıl her biri toplantı sorumlu açıklar.

-   **[Sistem durumu gözden geçirin.](https://aka.ms/healthreviewpaper)** Çözüm Coalfire systems, Inc. tarafından gözden Sistem durumu uyumluluk (HIPAA ve HITRUST) inceleyin ve uygulama için rehberlik sağlar bir denetçi\'çözüm ve ayrıntılı bir üretime hazır dağıtımına dönüştürme dikkat edilmesi gereken noktalar s gözden geçirin.

# <a name="architectural-diagram"></a>Mimari diyagramı


![](images/refarch.png)

## <a name="roles"></a>Roller


Şeması iki rolü yönetici kullanıcılara (Yöneticiler) ve kullanıcılar için üç rol Hastanenin yönetimi ve Hasta Bakım tanımlar. Altıncı rol, HIPAA ve diğer düzenlemelerle uyumluluğu değerlendirmek bir denetleyici için tanımlanır. Azure rol tabanlı erişim denetimi (RBAC) tam olarak bu çözüm yerleşik ve özel rolleri aracılığıyla her kullanıcı odaklı erişim yönetimi sağlar. Bkz: [Azure portalında rol tabanlı erişim denetimi ile çalışmaya başlama](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is) ve [Azure rol tabanlı erişim denetimi için yerleşik roller](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles) RBAC, rolleri ve izinleri hakkında ayrıntılı bilgi için.

### <a name="site-administrator"></a>Site Yöneticisi


Müşteri'nin Azure aboneliği için site yöneticisine sorumludur. Bütün olarak dağıtımını denetlemek, ancak hasta kayıtlara erişiminiz yok.

-   Varsayılan rol atamalarını: [sahibi](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles#owner)

-   Özel rol atamalarını: yok

-   Kapsam: Abonelik

### <a name="database-analyst"></a>Veritabanı Analisti

Veritabanı Analisti SQL Server örneği ve veritabanı yönetir.
Hiçbir hasta kayıtları erişimi.

-   Yerleşik rol atamalarını: [SQL DB Katılımcısı](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles#sql-db-contributor), [SQL Server katkıda bulunan](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles#sql-server-contributor)

-   Özel rol atamalarını: yok

-   Kapsam: kaynak grubu

 ### <a name="data-scientist"></a>Veri Bilimcisi


Veri Bilimcisi Azure Machine Learning hizmeti çalışır. Bunlar içeri aktarma, dışarı aktarabilir ve verileri yönetmek ve raporları çalıştırabilir. Veri Bilimcisi hasta veri erişimi olan ancak yönetici ayrıcalıklarına sahip değil.

-   Yerleşik rol atamalarını: [depolama hesabı katkıda bulunan](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles#storage-account-contributor)

-   Özel rol atamalarını: yok

-   Kapsam: kaynak grubu

### <a name="chief-medical-information-officer-cmio"></a>TIP bilgi Müdürü (CMIO)


CMIO informatics/teknoloji ve sağlık bir kuruluşta sağlık uzmanları arasında bölme yayılan. Genellikle, görevlerini kaynaklara kuruluş içinde uygun şekilde tahsis varsa belirlemek için analizi kullanarak içerir.

-   Yerleşik rol atamalarını: yok

### <a name="care-line-manager"></a>Satır Yöneticisi dikkat edin


Dikkatli satır doğrudan yöneticisidir hastalar dikkatli söz konusu.
Bu rol, hastaların durumlarının izlenmesini ve personelin hastalarının belirli bakım ihtiyaçlarını karşıladığından emin olmayı gerektirir. Dikkatli satır Yöneticisi ekleme ve hasta kayıtları güncelleştirme sorumludur.

-   Yerleşik rol atamalarını: yok

-   Özel rol atamalarını: her iki hasta giriş yapmak için HealthcareDemo.ps1 çalıştırmak için ayrıcalığı ve Boşalma.

-   Kapsam: kaynak grubu

### <a name="auditor"></a>Denetçi


Denetçi çözüm uyumluluk için değerlendirilir. Bunlar doğrudan ağa erişiminiz yok.

-   Yerleşik rol atamalarını: [okuyucusu](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles#reader)

-   Özel rol atamalarını: yok

-   Kapsam: Abonelik

## <a name="example-use-case"></a>Örnek Kullanım örneği


Bu şeması ile dahil örnek kullanım örneği, machine learning ve sistem durumu verileri bulutta analytics etkinleştirmek için Şeması'nın nasıl kullanılabileceğini gösterir. Contosoclinic Birleşik Devletler'de bulunan küçük hastaneler ' dir. Hastanenin ağ yöneticileri, Azure Machine Learning hastanın kalma uzunluğu işlem iş yükü verimliliği artırmak ve bunu sağlayabilir dikkatli kalitesini geliştirmek için admittance, aynı anda daha iyi tahmin etmek için kullanmak istediğiniz.

### <a name="predicting-length-of-stay"></a>Kalma tahmin


Örnek Kullanım senaryosu Azure Machine Learning hasta girişi sırasında toplanan geçmiş verileri önceki hastalar gerçekleştirilecek sağlık Ayrıntılar karşılaştırarak kalma yeni kabul edilen hastanın uzunluğu tahmin etmek için kullanır.
Şeması çok sayıda çözüm eğitim ve Tahmine dayalı özelliklerini göstermek için anonim tıbbi kayıtları içerir. Bir üretim dağıtımında, müşterilerin kendi kayıtlarını daha doğru tahminleri, ortamlarına, tesis ve hastalar benzersiz ayrıntılarını yansıtma için çözüm eğitmek için kullanırsınız.

### <a name="users-and-roles"></a>Kullanıcılar ve roller


**Site Yöneticisi--Alex**

*E-posta: Alex\_SiteAdmin*

Alex'ın iş yönetme bir şirket içi ağ yükünü azaltmak ve yönetim maliyetlerini azaltmak teknolojilerini değerlendirmek için kullanılır. Alex Azure süre için değerlendirme ancak kendisine bulutta Hasta verileri depolamak için HiTrust uyumluluk gereksinimlerini karşılamak için gereken hizmetleri yapılandırmak zorlandığınız. Alex HiTrust müşteri gereksinimlerini gereksinimlerini ele bir uyumluluk hazır durumu çözümü dağıtmak için Azure sistem durumu AI seçti.

**Data Scientist -- Debra**

*E-posta: Gamze\_DataScientist*

Gamze Hasta Bakım Öngörüler sağlamak için tıbbi kayıtları çözümlemek modelleri oluşturma ve kullanma sorumlu olur. Gamze, kendi modelleri oluşturmak için SQL ve R istatistiksel programlama dilini kullanır.

**Veritabanı Analisti--Danny**

*E-posta: Danny\_DBAnalyst*

Danny her şey için ana ilgili kişi için Contosoclinic hasta tüm verileri depolar Microsoft SQL Server ile ilgili ' dir. Danny en son Azure SQL veritabanı ile aşina deneyimli bir SQL Server Yönetici ' dir.

**TIP bilgi Müdürü--Caroline**

Caroline Chris verdiğiniz satır Yöneticisi ve Gamze veri Bilimcisi ile hangi faktörlerin kalma hasta uzunluğu etkisi belirlemek için çalışmaktadır.
Caroline Kal uzunluğu (LOS) çözümden tahminleri kaynaklara Hastanenin ağ uygun şekilde tahsis varsa belirlemek için kullanır. Örneğin, bu çözümde sağlanan Panoyu kullanarak.

**Satır Yöneticisi--Chris dikkat edin**

*E-posta: Chris\_CareLineManager*

Tek tek doğrudan hasta giriş ve discharges Contosoclinic, adresindeki yönetme sorumlu Chris LOS çözümü tarafından oluşturulan tahminleri yeterli personel dikkatli olarak kaldığını için hastalar sağlarken kullanılabilir olduğundan emin olmak için kullanır olanağı.

**Denetçi--Han**

*E-posta: Han\_denetçi*

Han deneyimi ISO, SOC ve HiTrust denetim sahip sertifikalı bir denetçi ' dir. Han Contosoclinc'ın ağ gözden geçirmek için işe. Han müşteri sorumluluk çözümüyle sağlanan, işlem, depolama ve hassas kişisel verileri görüntülemek için şeması ve LOS çözüm kullanılabilir emin olmak için matris gözden geçirebilirsiniz.


# <a name="design-configuration"></a>Tasarım yapılandırma


Bu bölümde varsayılan yapılandırmaları ve güvenlik önlemleri için ana hatlarıyla şeması yerleşik ayrıntıları:

- **Alma** FHIR veri kaynağı dahil olmak üzere veri ham kaynakları
- **Mağaza** hassas bilgileri
- **ÇÖZÜMLE** ve sonuçlarını tahmin etme
- **Etkileşim** Öngörüler ve sonuçları
- **KİMLİK** yönetim çözümü
- **Güvenlik** özelliklerinin etkin


## <a name="identity"></a>KİMLİK 

### <a name="azure-active-directory-and-role-based-access-control-rbac"></a>Azure Active Directory ve rol tabanlı erişim denetimi (RBAC)


**Kimlik doğrulaması:**

-   [Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/) Microsoft\'s çok kiracılı bulut tabanlı dizin ve Kimlik Yönetimi Hizmeti. Çözüm için tüm kullanıcılar Azure Active SQL veritabanına erişen kullanıcılar dahil olmak üzere Directory'de oluşturuldu.



-   Uygulama kimlik doğrulaması, Azure AD kullanılarak gerçekleştirilir. Daha fazla bilgi için bkz: [uygulamaları Azure Active Directory ile tümleştirme](/azure/active-directory/develop/active-directory-integrating-applications).

-   [Azure Active Directory kimlik koruması](/azure/active-directory/active-directory-identityprotection) kuruluşunuzdaki kimlikleri etkileyen olası güvenlik açıklarını algılarsa, otomatik yanıtları, kuruluşunuzun kimlikleri, ilgili algılanan kuşkulu eylemler için yapılandırır ve Şüpheli olaylar araştırır ve bunları gidermek için uygun tedbiri alır.

-   [Azure rol tabanlı erişim denetimi (RBAC)](/azure/active-directory/role-based-access-control-configure) tam olarak Azure için odaklı erişim yönetimi sağlar. Abonelik erişim Abonelik Yöneticisi sınırlıdır ve Azure anahtar kasası erişim için site yöneticisine sınırlıdır. Güçlü parolalar (en az bir büyük/küçük harf, sayı ve özel karakter ile en az 12 karakter) gereklidir.

-   Çok faktörlü kimlik doğrulaması - enableMFA anahtar dağıtımı sırasında etkin olduğunda desteklenir.

-   Parolalar - enableADDomainPasswordPolicy anahtar dağıtımı sırasında etkin olduğunda 60 gün sonra süresi dolacak.

**Roller:**

-   Çözüm kullanır [yerleşik roller](/azure/active-directory/role-based-access-built-in-roles) kaynaklarına erişimi yönetmek için.

-   Tüm kullanıcıların belirli yerleşik roller varsayılan olarak atanır.

### <a name="azure-key-vault"></a>Azure Key Vault

-   Anahtar kasasında depolanan verileri şunları içerir:

    -   Uygulama Insight anahtarı
    -   Hasta veri depolama erişim tuşu
    -   Hasta bağlantı dizesi
    -   Hasta veri tablosu adı
    -   Azure ML Web Hizmeti uç noktası
    -   Azure ML Service API Key

-   Gelişmiş erişim ilkeleri bir gereksinim temelinde yapılandırılır
-   Anahtar kasası erişim ilkelerini anahtarları ve gizli anahtarları için gerekli minimum izinleri ile tanımlanır
-   Tüm anahtarları ve gizli anahtar kasasında sona erme tarihleri sahip
-   Tüm anahtarları anahtar Kasası'HSM tarafından korunan \[anahtar türü HSM korumalı 2048 bit RSA anahtar =\]
-   Tüm kullanıcılar kimliklerini rol tabanlı erişim denetimi (RBAC) kullanarak en az gerekli izinlerin verildiğinden
-   Uygulamalar bir anahtar kasası birbirine güvenen ve çalışma zamanında aynı parolaları erişmesi gereken sürece paylaşmıyor
-   Tanılama günlüklerini anahtar kasası için en az 365 gün bekletme süresi ile etkinleştirilir.
-   İzin verilen şifreleme işlemleri anahtarları için gerekli olanlar sınırlıdır

## <a name="ingest"></a>ALMA 

### <a name="azure-functions"></a>Azure İşlevleri
Çözüm kullanmak üzere tasarlanmış [Azure işlevleri](/azure/azure-functions/) Kal veri analizi gösteride kullanılan örnek uzunluğu işleyemedi. Üç yetenekleri işlevlerinde oluşturdunuz.

**1. Müşteri verileri phi verileri toplu olarak alma işlemi**

Tanıtım betiği kullanırken. . \\İle HealthcareDemo.ps1 **BulkPatientAdmission** kısmında özetlendiği gibi anahtar **dağıtma ve tanıtım çalıştıran** aşağıdaki işleme ardışık çalıştırır:
1. **Azure Blob Storage** -Hasta verileri .csv dosyası örneği karşıya depolama
2. **Olay kılavuz** -olay yayımlar veri Azure işlevi (toplu içeri - blob olay)
3. **Azure işlevi** - işlemeyi gerçekleştirir ve güvenli işlevini kullanarak SQL depolama alanına veri depolayan - olayı (yazın; url blob)
4. **SQL DB** - Hasta verileri kullanarak için veritabanı depolama için sınıflandırma etiketlerini ve ML işlem eğitim denemenizi yapmak için başlayacağı zamana devre dışı.

![](images/dataflow.png)

Ayrıca azure işlevi okuyun ve örnek veri kümesi'nin aşağıdaki etiketleri kullanarak belirlenmiş hassas verileri korumak için tasarlanmıştır:
- dataProfile = > "ePHI"
- sahibi = > \<Site Yönetim UPN\>
- ortam = > "Pilot"
- Departman = > "Genel etiketleme burada hasta 'adları' tanımlanan düz metin olarak örnek veri kümesine uygulandığı ekosistemi".

**2. Yeni hastalar giriş**

Tanıtım betiği kullanırken. . \\İle HealthcareDemo.ps1 **BulkPatientadmission** kısmında özetlendiği gibi anahtar **dağıtma ve tanıtım çalıştıran** aşağıdaki işleme ardışık yürütür: ![](images/securetransact.png) 
 **1. Azure işlevi** tetiklenir ve işlev istekleri için bir [taşıyıcı belirteci](/rest/api/) Azure Active Directory'den.

**2. Anahtar kasası** istenen belirtece ilişkili bir gizlilik için istenen.

**3. Azure rolleri isteği doğrulama ve anahtar kasası erişim isteği yetkilendirin.

**4. Anahtar kasası** gizli bu durumda SQL DB bağlantı dizesini döndürür.

**5. Azure işlevi** güvenli bir şekilde SQL veritabanına bağlanmak için bağlantı dizesini kullanır ve ePHI verileri depolamak için başka bir işleme devam eder.

Veri depolama elde etmek için ortak bir API şema hızlı sağlık birlikte çalışabilirlik kaynakları (FHIR yangın denilir) aşağıdaki uygulanmıştır. İşlevi aşağıdaki FHIR exchange öğeleri sağlanmıştır:

-   [Hasta şema](https://www.hl7.org/fhir/patient.html) "olan" bir Hasta hakkında bilgi içerir.

-   [Gözlem şema](https://www.hl7.org/fhir/observation.html) sağlık merkezi öğeyi kapsayan, tanılama desteği, ilerleme durumunu izlemek, taban çizgileri ve desenlerini belirlemek için kullanılır ve bile demografik özellikleri yakalayın. 

-   [Şema karşılaştığınız](https://www.hl7.org/fhir/encounter.html) kapsar karşılaştığı ambulatory, Acil, gibi türlerini ev sistem durumu, inpatient ve sanal karşılaştığı.

-   [Koşul şema](https://www.hl7.org/fhir/condition.html) bir koşul, sorun, tanılama, veya diğer olay, durum, sorun veya bir sorunu düzeyine risen Klinik kavram hakkında ayrıntılı bilgiler içerir.  



### <a name="event-grid"></a>Event Grid


Çözüm Azure olay kılavuz, olayların tüm herhangi bir kaynaktan herhangi bir hedefe yönlendirme sağlanması yönetmek için tek bir hizmeti destekler:

-   [Güvenlik ve kimlik doğrulama](/azure/event-grid/security-authentication)

-   [Rol tabanlı erişim denetimi](/azure/event-grid/security-authentication#management-access-control) Olay Aboneliklerini listeleme, yenilerini oluşturmadan ve anahtarlar oluşturma gibi çeşitli yönetim işlemleri

-   Denetim

## <a name="store"></a>DEPOLAMA 

### <a name="sql-database-and-server"></a>SQL veritabanı ve sunucu 


-   [Saydam veri şifreleme (TDE)](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) gerçek zamanlı şifreleme ve şifre çözme Azure anahtar kasasında depolanan bir anahtar kullanarak Azure SQL veritabanında depolanan veri sağlar.

-   [SQL güvenlik açığı değerlendirmesi](https://docs.microsoft.com/azure/sql-database/sql-vulnerability-assessment) bir kolay bulmak, izlemek ve olası veritabanı açıkları düzeltme aracı yapılandırın.

-   [SQL veritabanı tehdit algılama](/azure/sql-database/sql-database-threat-detection) etkin.

-   [SQL veritabanı denetimi](/azure/sql-database/sql-database-auditing) etkin.

-   [SQL veritabanı ölçümleri ve tanılama günlük](/azure/sql-database/sql-database-metrics-diag-logging) etkin.

-   [Sunucu ve veritabanı düzeyinde güvenlik duvarı kuralları](/azure/sql-database/sql-database-firewall-configure) kısıtlanmıştır.

-   [Sütunları'her zaman şifreli](/azure/sql-database/sql-database-always-encrypted-azure-key-vault) hasta, Orta, adları ve soyadları korumak için kullanılır.
    Ayrıca, veritabanı sütun şifreleme Azure Active Directory (AD) uygulama Azure SQL veritabanı kimlik doğrulaması için de kullanır.

-   SQL Database ve SQL Server erişimi en az ayrıcalık prensibi göre yapılandırılır.

-   Yalnızca gerekli IP adreslerini SQL güvenlik duvarı üzerinden erişime izin verilir.

### <a name="storage-accounts"></a>Depolama hesapları


-   [Hareket halindeki verileriniz, yalnızca TLS/SSL kullanarak aktarılır](/azure/storage/common/storage-require-secure-transfer?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json).

-   Anonim erişim için kapsayıcı izin verilmiyor.

-   Uyarı kuralları anonim etkinliğini izlemek için yapılandırılır.

-   HTTPS, depolama hesabı kaynaklarına erişmek için gereklidir.

-   Kimlik doğrulama isteği verileri günlüğe ve izlenir.

-   Blob Depolama birimindeki veri bekleme sırasında şifrelenir.

## <a name="analyze"></a>ANALYZE

### <a name="machine-learning"></a>Machine Learning


-   [Günlüğe kaydetme etkinleştirildiğinde](/azure/machine-learning/studio/web-services-logging) için Machine Learning web hizmetleri.
- kullanarak [Machine Learning](/azure/machine-learning/preview/experimentation-service-configuration) çalışma ekranı bir çözüm kümesine tahmin etme olanağı sağlar denemeler, geliştirilmesi gerektirir. [Çalışma ekranı tümleştirme](/azure/machine-learning/preview/using-git-ml-project) denemeler yönetimini basitleştirmeye yardımcı olabilir.

## <a name="security"></a>GÜVENLİK

### <a name="azure-security-center"></a>Azure Güvenlik Merkezi
- [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/) tüm Azure kaynaklarınızın güvenlik durumunu merkezi bir görünümünü sağlar. Bir bakışta, uygun güvenlik denetimleri yerinde olduğundan ve doğru şekilde yapılandırıldığını ve dikkat gerektiren herhangi bir kaynağa hızla tanımlayabilen doğrulayabilirsiniz. 

- [Azure Danışmanı](/azure/advisor/advisor-overview) olan yardımcı olan kişiselleştirilmiş bulut Danışman Azure dağıtımlarınızı iyileştirmek için en iyi uygulamaları izleyin. Danışman, kaynak yapılandırmanızı ve kullanım telemetrinizi analiz ederek Azure kaynaklarınızın maliyet verimliliğini, performansını, yüksek kullanılabilirliğini ve güvenliğini geliştirmenize yardımcı olabilecek çözümler önerir.

### <a name="application-insights"></a>Application Insights
- [Application Insights](/azure/application-insights/app-insights-overview) birden çok platformdaki web geliştiricileri için genişletilebilir bir uygulama performansı Yönetimi (APM) hizmetidir. Canlı web uygulamanızı izlemek için kullanabilirsiniz. Performans anormalliklerini algılar. Sorunları tanılamanıza ve kullanıcıların uygulamanızla aslında neler yaptığını anlamanıza yardımcı olan güçlü analiz araçları içerir. Performansı ve kullanılabilirliği sürekli geliştirmenize yardımcı olmak amacıyla tasarlanmıştır.

### <a name="azure-alerts"></a>Azure uyarıları
- [Uyarılar Azure hizmetlerini izleme, bir yöntem sunar ve verileri üzerinde koşullar yapılandırmanıza izin verir. Bir uyarı durumu izleme verilerini eşleştiğinde uyarı bildirimleri de sağlar.

### <a name="operations-management-suite-oms"></a>Operations Management Suite (OMS)
[Operations Management Suite (OMS olarak da bilinir)](/azure/operations-management-suite/operations-management-suite-overview) Yönetim Hizmetleri koleksiyonudur.

-   Çalışma alanı için Güvenlik Merkezi etkinleştirilir

-   İş yükü izleme için çalışma alanı etkin

-   İş yükü izleme için etkinleştirilir:

    -   Kimlik ve Erişim

    -   Güvenlik ve Denetim

    -   Azure SQL DB analizi

    -   [Azure WebApp Analytics](/azure/log-analytics/log-analytics-azure-web-apps-analytics) çözümü

    -   Key Vault Analytics

    -   Değişiklik İzleme

-   [Uygulama Öngörüler Bağlayıcısı (Önizleme)](/azure/log-analytics/log-analytics-app-insights-connector) etkin

-   [Etkinlik günlük analizi](/azure/log-analytics/log-analytics-activity) etkin
