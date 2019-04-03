---
title: Azure sistem durumu Analytics şema
description: HIPAA/HITRUST sağlık analizi Blueprint dağıtmak için yönergeler
services: security
documentationcenter: na
author: RajeevRangappa
ms.assetid: 26566e0a-0a54-49f4-a91d-48e20b7cef71
ms.service: security
ms.topic: article
ms.date: 07/23/2018
ms.author: rarangap
ms.openlocfilehash: 70721b8bfbecaf554a9502b9ec3417fc8e561b3f
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58885953"
---
# <a name="azure-security-and-compliance-blueprint---hipaahitrust-health-data-and-ai"></a>Azure güvenlik ve uyumluluk planı - HIPAA/HITRUST sağlık verileri ve yapay ZEKA

## <a name="overview"></a>Genel Bakış

**Azure güvenlik ve uyumluluk planı - HIPAA/HITRUST sağlık verileri ve yapay ZEKA sunan Azure PaaS ve Iaas çözümünü nasıl içe almak, depolamak, çözümlemek, etkileşim, kimlik ve sistem durumu veri çözümleriyle güvenli bir şekilde dağıtmak için bir anahtar teslim dağıtımı sektör uyumluluk gereksinimlerini karşılayabiliyor olması. Blueprint bulut benimseme ve düzenlendiğini veri sahip müşteriler için kullanım artırmanıza yardımcı olur.**

Azure güvenlik ve uyumluluk planı - HIPAA/HITRUST sağlık verileri ve yapay ZEKA şema sağlar, araçları ve güvenli dağıtmak için yönergeler, sağlık sigortası taşınabilirlik ve Sorumluluk Yasası (HIPAA) ve sistem durumu bilgileri güven Alliance (HITRUST) hazır almak, depolamak, çözümlemek ve bir uçtan uca çözüm olarak dağıtılan bir güvenli, çok katmanlı bulut ortamında tıbbi kayıtlarla kişisel ve kişisel olmayan etkileşim için hizmet olarak platform (PaaS) ortamı. 

Iaas çözümü, şirket içi SQL tabanlı çözümünü Azure'a geçirmek için ve bir ayrıcalıklı erişim iş istasyonu (bulut tabanlı hizmet ve çözümler güvenli bir şekilde yönetmek için PAW) uygulamak için nasıl sürdürebileceğiniz gösterilecek. Iaas SQL Server veritabanını bir SQL Iaas sanal makinede veriler içeri olası deneme ekler ve kimliği doğrulanmış erişim bir SQL Azure PaaS hizmeti etkileşim kurmak için VM MSI kullanır. Hem bu ortak bir başvuru mimarisi gösteren ve Microsoft Azure'nin yayılmasını kolaylaştırmak için tasarlanmıştır. Sağlanan bu mimari, yük ve dağıtım maliyetini azaltmak için bulut tabanlı bir yaklaşım isteyen kuruluşların gereksinimlerini karşılayacak bir çözüm gösterilmektedir.

![](images/components.png)

Çözüm hızlı sağlık birlikte çalışabilirlik kaynakları (FHIR), dünya çapında standart elektronik olarak sağlık bilgi alışverişi kullanılarak biçimlendirilmiş bir örnek veri kümesini kullanan ve güvenli bir şekilde depolamak için tasarlanmıştır. Müşteriler Azure Machine Learning Studio, güçlü iş zekası araçları ve örnek veriler üzerinde yapılan tahminlerin gözden geçirmek için analiz yararlanmak için sonra kullanabilirsiniz. Azure Machine Learning Studio rahatlatabilir deneme türüne bir örnek olarak, şema örnek veri kümesi, betikler ve bir hastane tesiste hastanın hastalar tahmin etmeye yönelik araçlar içerir. 

Bu plan, müşterilerin yeni Azure makine öğrenimi denemeleri hem Klinik ve operasyonel kullanım örneği senaryoları çözmek için geliştirme kendi belirli gereksinimlerine ayarlamak modüler bir temel olarak görev yapacak yöneliktir. Güvenli ve dağıtıldığında uyumlu olacak şekilde tasarlanmıştır; Bununla birlikte, müşteriler rollerini doğru yapılandırmayı ve herhangi bir değişiklik uygulama için sorumludur. Şunlara dikkat edin:

-   Bu plan müşterileri bir HITRUST içinde Microsoft Azure'u kullanmak için bir temel sağlar ve HIPAA ortamı.

-   Blueprint HIPAA ve HITRUST (genel güvenlik çerçevesi aracılığıyla--CSF) ile uyumlu için tasarlanmış olsa da, HIPAA ve HITRUST sertifika gereksinimleri başına dış bir denetçi olarak sertifikalı kadar uyumlu kabul edilmemelidir.

-   Müşteriler, bu temel mimari kullanılarak oluşturulan herhangi bir çözümü uygun güvenlik ve uyumluluk geçirmeler yürütmek için sorumludur.

## <a name="deploying-the-automation"></a>Otomasyon dağıtma

- Çözümü dağıtmak için bölümlerinde sağlanan yönergeleri izleyin. [Dağıtım Kılavuzu](https://github.com/Azure/Health-Data-and-AI-Blueprint/blob/master/deployment.md). 

- Bu çözümü nasıl çalıştığına ilişkin hızlı bir genel bakış için bu izleme [video](https://aka.ms/healthblueprintvideo) açıklayan ve gösteren dağıtımı.

- Sık sorulan sorular bulunabilir [SSS](https://aka.ms/healthblueprintfaq) Kılavuzu.

-   **Mimari diyagramı.** Şema için kullanılan başvuru mimarisi diyagramı gösterir ve örnek kullanım örneği senaryosu.

-   [Iaas uzantısı](https://github.com/Azure/Health-Data-and-AI-Blueprint/blob/master/README%20IaaS.md) Bu çözüm, şirket içi SQL tabanlı çözümünü Azure'a geçirme ve bulut tabanlı hizmet ve çözümler güvenli bir şekilde yönetmek için ayrıcalıklı bir erişim Workstation uygulamak için nasıl gösterilecektir. 

## <a name="solution-components"></a>Çözüm bileşenleri


Temel mimari aşağıdaki bileşenlerden oluşur:

-   **[Tehdit modeli](https://aka.ms/healththreatmodel)**  ile kullanılmak üzere sağlanan kapsamlı tehdit modeli tm7 biçiminde [Microsoft tehdit modelleme aracı](https://www.microsoft.com/en-us/download/details.aspx?id=49168), çözüm bileşenlerini görüntüleyen veri bunları ve güven arasında akış sınırlar. Model sistem altyapısında potansiyel risk puanları, Machine Learning Studio bileşenleri ve diğer değişikliklerin geliştirirken anlamasına yardımcı olabilir.

-   **[Müşteri uygulamasını matris](https://aka.ms/healthcrmblueprint)**  bir Microsoft Excel çalışma kitabını ilgili HITRUST gereksinimleri listeler ve Microsoft ve müşterinin nasıl her bir toplantı için sorumlu açıklar.

-   **[Sistem durumu gözden geçirin.](https://aka.ms/healthreviewpaper)** Çözüm Coalfire systems, Inc. tarafından gözden geçirildi Sistem durumu uyumluluk (HIPAA ve HITRUST) gözden geçirin ve uygulama yönergeleri sağlayan bir denetçi\'çözüm ve üretime hazır bir dağıtım için şema dönüştürme değerlendirmeleri s gözden geçirin.

## <a name="architectural-diagram"></a>Mimari diyagramı


![](images/ra2.png)

## <a name="roles"></a>Roller


Blueprint iki rolü yönetim kullanıcılarının (işleçler) ve kullanıcılar için üç rol hasta ve hastane yönetiminde de tanımlar. Altıncı bir rol, HIPAA ve diğer düzenlemelere uyumluluğu değerlendirmek bir denetleyici için tanımlanır. Azure rol tabanlı erişim denetimi (RBAC), tam olarak bu çözümün yerleşik ve özel rolleri aracılığıyla her kullanıcı odaklı erişim yönetimi sağlar. Bkz: [Azure portalında rol tabanlı erişim denetimi ile çalışmaya başlama](https://docs.microsoft.com/azure/role-based-access-control/overview) ve [Azure rol tabanlı erişim denetimi için yerleşik roller](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) RBAC rolleri ve izinleri hakkında ayrıntılı bilgi için.

### <a name="site-administrator"></a>Site Yöneticisi


Müşterinin Azure aboneliği için site yöneticisi sorumludur. Genel dağıtım denetlemek, ancak hasta kayıtlarına erişiminiz yok.

-   Varsayılan rol atamaları: [Sahip](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#owner)

-   Özel rol atamaları: Yok

-   Kapsam: Abonelik

### <a name="database-analyst"></a>Veritabanı Analisti

Veritabanı Analisti, veritabanı ve SQL Server örneği yönetir.
Hasta kayıtları erişim sahiptirler.

-   Yerleşik rol atamaları: [SQL DB Katılımcısı](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#sql-db-contributor), [SQL Server Katılımcısı](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#sql-server-contributor)

-   Özel rol atamaları: Yok

-   Kapsam: ResourceGroup

### <a name="data-scientist"></a>Veri uzmanı


Veri uzmanı, Azure Machine Learning Studio çalışır. Bunlar içeri aktarabilir, verme ve verileri yönetmek ve raporları çalıştırma. Veri uzmanı hasta veri erişimi olan ancak yönetici ayrıcalıklarına sahip değil.

-   Yerleşik rol atamaları: [Depolama Hesabı Katılımcısı](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-account-contributor)

-   Özel rol atamaları: Yok

-   Kapsam: ResourceGroup

### <a name="chief-medical-information-officer-cmio"></a>TIP bilişim daire Başkanı (CMIO)


CMIO bilişim/teknoloji ve sağlık hizmeti kuruluşunda sağlık uzmanları arasındaki ayrımı idare etmeye. Kaynakları uygun şekilde kuruluş içinde ayrılıp ayrılmadığını belirlemek için analiz kullanarak görevleri genellikle içerir.

-   Yerleşik rol atamaları: None

### <a name="care-line-manager"></a>Bakım satır Yöneticisi


Bakım satır Yöneticisi doğrudan olan hastalara dikkatli söz konusu.
Bu rol, hastaların durumlarının izlenmesini ve personelin hastalarının belirli bakım ihtiyaçlarını karşıladığından emin olmayı gerektirir. Ekleme ve hasta kayıtlarını güncelleştirmek için dikkatli satır Yöneticisi sorumludur.

-   Yerleşik rol atamaları: None

-   Özel rol atamaları: Her iki hasta giriş yapmak için HealthcareDemo.ps1 çalıştırılacak ayrıcalığına sahip ve Taburcu.

-   Kapsam: ResourceGroup

### <a name="auditor"></a>Denetçi


Denetçi çözüm uyumluluk için değerlendirilir. Hiçbir ağa doğrudan erişim sahiptirler.

-   Yerleşik rol atamaları: [Okuyucu](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#reader)

-   Özel rol atamaları: Yok

-   Kapsam: Abonelik

## <a name="example-use-case"></a>Örnek Kullanım örneği


Bu şema ile dahil örnek kullanım örneği, machine learning ve analytics sistem durumu verileri bulutta etkinleştirmek için şema'nın nasıl kullanılabileceğini gösterir. Amerika Birleşik Devletleri'nde bulunan küçük bir hastane Contosoclinic olur. Yöneticiler, Azure Machine Learning Studio'da daha iyi kullanmak istediğiniz hastane ağ hastanın hastalar admittance, işlemsel iş verimliliği artırın ve sağlamak bakım kalitesini artırmak için zamanında tahmin edin.

### <a name="predicting-length-of-stay"></a>Tahmin etme


Örnek Kullanım örneği senaryosu, hasta işleme sırasında toplanan geçmiş veri önceki hastalara geçen tıbbi ayrıntıları karşılaştırarak kalma yeni kabul edilen bir hastanın uzunluğu tahmin etmek için Azure Machine Learning Studio kullanır.
Blueprint büyük bir çözüm eğitim ve Tahmine dayalı özelliklerini göstermek için anonim tıbbi kayıt kümesi içerir. Bir üretim dağıtımında, müşterilerin kendi kayıtlarını benzersiz, ortam, tesis ve hastaların ayrıntılarını yansıtan daha doğru tahminler elde etmek için çözüm geliştirmek için kullanırsınız.

### <a name="users-and-roles"></a>Kullanıcılar ve roller


**Site Yöneticisi--Alex**

*E-posta: Alex\_SiteAdmin*

Alex'ın şirket içi ağ yönetme yükü azaltmak ve yönetim maliyetlerini azaltmak teknolojilerini değerlendirmek için işidir. Alex Azure süre değerlendirme ancak kendisinin hasta verilerini bulutta depolamak için Hıtrust uyumluluk gereksinimlerini karşılamak için gereken hizmetleri yapılandırmak işlerinin. Alex Hıtrust müşteri gereksinimlerini gereksinimlerini ele bir hazır uyumluluk sistem durumu çözümü dağıtmak için Azure durumu AI seçildi.

**Data Scientist -- Debra**

*E-posta: Gamze\_DataScientist*

Gamze, hasta Öngörüler sağlamak için tıbbi kayıtları analiz modelleri oluşturma ve kullanarak sorumlu olur. Gamze, kendi modeller oluşturmak için SQL ve R istatistik programlama dilini kullanır.

**Analist--Danny veritabanı**

*E-posta: Danny\_DBAnalyst*

Danny Contosoclinic için tüm hasta veri depolayan bir Microsoft SQL Server ile ilgili her şey için ana kişi olur. Danny en son Azure SQL veritabanı ile ilgili bilgi sahibi oldu deneyimli bir SQL Server Yönetici ' dir.

**TIP bilişim daire Başkanı--Caroline**

Caroline Chris Care satır Yöneticisi ile Gamze veri Bilimcisi kalma hasta uzunluğu hangi faktörlerin etkilediğini belirlemek için çalışmaktadır.
Caroline kalın uzunluğu (LOS) çözüme ait tahminlerin kaynakları uygun şekilde hastane ağında ayrılıp ayrılmadığını belirlemek için kullanır. Örneğin, bu çözümde sağlanan Panoyu kullanarak.

**Satır Yöneticisi--Chris dikkat edin.**

*E-posta: Chris\_CareLineManager*

Bireysel olarak doğrudan hasta kabul edildikten ve geri Contosoclinic, adresindeki yönetmek için sorumlu Chris LOS çözümü tarafından oluşturulan tahminlerin yeterli personel dikkat içinde kaldığını hastalara sağlarken kullanılabilir olmasını sağlamak için kullanır olanağı.

**Denetçi--Han**

*E-posta: Han\_denetçi*

Han deneyimi ISO, SOC ve Hıtrust denetim olan sertifikalı bir denetçi ' dir. Han Contosoclinc'ın ağ gözden geçirmek için işe alındım. Han müşteri sorumluluk çözümle birlikte sağlanan depolayan, işleyen ve hassas kişisel verileri görüntülemek için şema ve LOS çözüm kullanılabilir emin olmak için matris gözden geçirebilirsiniz.


## <a name="design-configuration"></a>Tasarım yapılandırma


Bu bölümde, için ana hatlarıyla belirtilen şema yerleşik güvenlik önlemleri ve varsayılan yapılandırmaları açıklanmaktadır:

- **Alma** FHIR veri kaynağı gibi veri ham kaynakları
- **Mağaza** hassas bilgileri
- **ANALİZ** ve sonuçlarını tahmin edin
- **INTERACT** Öngörüler ve sonuçları
- **KİMLİK** çözümünün Yönetimi
- **Güvenlik** özellikleri etkin


## <a name="identity"></a>KİMLİK 

### <a name="azure-active-directory-and-role-based-access-control-rbac"></a>Azure Active Directory ve rol tabanlı erişim denetimi (RBAC)


**Kimlik Doğrulama:**

-   [Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/) Microsoft\'s çok kiracılı bulut tabanlı dizin ve kimlik yönetimi hizmetidir. Çözüm için tüm kullanıcılar Azure Active SQL veritabanına erişen kullanıcılar dahil olmak üzere Directory'de oluşturuldu.



-   Azure AD kullanarak uygulamaya kimlik doğrulaması gerçekleştirilir. Daha fazla bilgi için [uygulamaları Azure Active Directory ile tümleştirme](/azure/active-directory/develop/active-directory-integrating-applications).

-   [Azure Active Directory kimlik koruması](/azure/active-directory/active-directory-identityprotection) kuruluşunuzun kimliklerini etkileyen olası güvenlik açıklarını algılar, kuruluşunuzun kimliklerini için ilgili algılanan kuşkulu eylemleri için otomatik yanıtlar yapılandırır ve Şüpheli olayları araştırdıktan ve bunları gidermek için uygun tedbiri alır.

-   [Azure rol tabanlı erişim denetimi (RBAC)](/azure/role-based-access-control/role-assignments-portal) tam olarak Azure için odaklanmış erişim yönetimi sağlar. Abonelik erişimi için abonelik yöneticisine sınırlıdır ve site yöneticisi için Azure anahtar kasası erişim sınırlıdır. Güçlü parolalar (en az bir büyük/küçük harf, sayı ve özel karakter ile en az 12 karakter) gerekli değildir.

-   Çok faktörlü kimlik doğrulaması - enableMFA anahtar dağıtımı sırasında etkin olduğunda desteklenir.

-   Parola - enableADDomainPasswordPolicy anahtar dağıtımı sırasında etkin olduğunda 60 gün sonra süresi dolar.

**Roller:**

-   Çözüm kullanır [yerleşik roller](/azure/role-based-access-control/built-in-roles) kaynaklara erişimi yönetmek için.

-   Tüm kullanıcılar varsayılan olarak yerleşik belirli rollere atanır.

### <a name="azure-key-vault"></a>Azure Key Vault

-   Key Vault'ta depolanan verileri içerir:

    -   Application Insight anahtarı
    -   Hasta veri depolama erişim anahtarı
    -   Hasta bağlantı dizesi
    -   Hasta veri tablosu adı
    -   Azure ML Web Hizmeti uç noktası
    -   Azure ML Service API anahtarı

-   Gelişmiş erişim ilkeleri gerek temelinde yapılandırılır
-   Key Vault erişim ilkeleri anahtarlara ve gizli anahtarları en düşük gerekli izinlerle tanımlanmış
-   Tüm anahtarları ve gizli anahtar Kasası'nda son kullanma tarihi vardır.
-   Tüm anahtarları Key vault'ta HSM ile korunan \[anahtar türü HSM korumalı anahtarlar 2048 bit RSA anahtarı =\]
-   Tüm kullanıcılar kimliklerini rol tabanlı erişim denetimi (RBAC) kullanarak en düşük gerekli izinleri verilir.
-   Uygulamaları bir Key Vault birbirine güvenen ve çalışma zamanında aynı gizli dizilere erişim ihtiyaç duydukları sürece paylaşmayın
-   Key Vault için tanılama günlükleri ile 365 gün en az bir saklama süresi etkinleştirilir.
-   Anahtarlar için izin verilen şifreleme işlemleri gerekli olanlarla sınırlıdır

## <a name="ingest"></a>ALMA 

### <a name="azure-functions"></a>Azure İşlevleri
Çözümü kullanmak için tasarlanmış [Azure işlevleri](/azure/azure-functions/) kalın veri analizi kullanılan örnek uzunluğunu işleyemedi. Üç özelliklerinde işlevleri oluşturdunuz.

**1. Müşteri verileri phı verileri toplu olarak içeri aktarma**

Tanıtım betiğini kullanırken. . \\İle HealthcareDemo.ps1 **BulkPatientAdmission** açıklandığı şekilde geçiş **dağıtma ve tanıtım çalıştıran** aşağıdaki işleme ardışık düzeninde yürütür:
1. **Azure Blob Depolama** -Hasta verileri .csv dosyası örneği depolama için karşıya yüklendi
2. **Event Grid** -(toplu içeri aktarma - blob olay) Azure işlevine olayı yayımlar veri
3. **Azure işlevi** - işlemeyi gerçekleştirir ve güvenli işlevi kullanarak SQL depolama alanına verileri depolayan - olay (yazın; url blob)
4. **SQL DB** - hasta veri kullanmak için veritabanı depolama için sınıflandırma etiketleri ve eğitim denemesini yapmak için ML işlemi başlatılır.

![](images/dataflow.png)

Ayrıca azure işlevi, okumak ve aşağıdaki etiketleri kullanarak örnek veri kümesini belirlenen hassas verileri korumak için tasarlanmıştır:
- dataProfile = > "ePHI"
- sahibi = > \<Site yönetici UPN\>
- ortam = > "Pilot"
- Departman = > "Genel ekosistemi etiketlemesi için örnek veri kümesini burada hasta 'adları' belirlenen düz metin uygulandığı".

**2. Yeni hastaların giriş**

Tanıtım betiğini kullanırken. . \\İle HealthcareDemo.ps1 **BulkPatientadmission** açıklandığı şekilde geçiş **dağıtma ve tanıtım çalıştıran** aşağıdaki işleme ardışık düzeninde yürütür: ![](images/securetransact.png)
**1. Azure işlevi** tetiklenir ve işlev istekleri için bir [taşıyıcı belirteç](/rest/api/) Azure Active Directory'den.

**2. Key Vault** istenen belirteci ilişkili gizli dizi için istenen.

**3. Azure rolleri** isteği doğrulama ve yetkilendirme Key vault'a erişim isteği.

**4. Key Vault** gizli dizi, bu durumda SQL DB bağlantı dizesini döndürür.

**5. Azure işlevi** güvenli bir şekilde SQL veritabanına bağlanmak için bağlantı dizesini kullanır ve ePHI verileri depolamak için daha fazla işleme devam eder.

Veri depolama alanı elde etmek için ortak bir API şema hızlı sağlık birlikte çalışabilirlik kaynakları (yangın telaffuz FHIR) aşağıdaki uygulanmıştır. İşlev aşağıdaki FHIR exchange öğeleri sağlanmıştır:

-   [Hasta şema](https://www.hl7.org/fhir/patient.html) "olan" bir Hasta hakkında bilgi içerir.

-   [Gözlem şema](https://www.hl7.org/fhir/observation.html) sağlık merkezi öğesinde kapsar ve demografik özelliklere bile yakalama tanılama desteği, ilerleme durumunu izlemek, temelleri ve desenleri belirlemek için kullanılır. 

-   [Şema karşılaştığınız](https://www.hl7.org/fhir/encounter.html) karşılaştığı ambulatory, Acil durum, gibi türlerini kapsar, sistem durumu, inpatient ve sanal karşılaştığı giriş.

-   [Koşul şema](https://www.hl7.org/fhir/condition.html) bir koşul, sorun, tanılama, veya diğer olay, durumu, sorunu veya sorunlu bir düzeye risen Klinik kavramı hakkında ayrıntılı bilgiler ele alınmaktadır.  



### <a name="event-grid"></a>Event Grid


Çözüm, Azure Event Grid, tek bir hizmet sağlanması, herhangi bir kaynaktan herhangi bir hedefe tüm olayları yönlendirmenizi sağlayan için destekler:

-   [Güvenlik ve kimlik doğrulaması](/azure/event-grid/security-authentication)

-   [Rol tabanlı erişim denetimi](/azure/event-grid/security-authentication#management-access-control) için anahtarlar oluşturma Olay Aboneliklerini listeleme ve yenilerini oluşturmak gibi çeşitli yönetim işlemlerini

-   Denetim

## <a name="store"></a>DEPOLAMA 

### <a name="sql-database-and-server"></a>SQL veritabanı ve sunucu 


-   [Saydam veri şifrelemesi (TDE)](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) gerçek zamanlı şifreleme ve şifre çözme işlemleri Azure Key Vault'ta depolanan bir anahtar kullanarak Azure SQL veritabanı'nda depolanan verileri sağlar.

-   [SQL güvenlik açığı değerlendirmesi](https://docs.microsoft.com/azure/sql-database/sql-vulnerability-assessment) bir kolayca bulmak, izlemek ve olası veritabanı güvenlik açıklarını düzeltme Aracı'nı yapılandırmak.

-   [SQL veritabanı tehdit algılama](/azure/sql-database/sql-database-threat-detection) etkin.

-   [SQL veritabanı denetimi](/azure/sql-database/sql-database-auditing) etkin.

-   [SQL veritabanı ölçümleri ve tanılama günlüğüne kaydetme](/azure/sql-database/sql-database-metrics-diag-logging) etkin.

-   [Sunucu ve veritabanı düzeyinde güvenlik duvarı kuralları](/azure/sql-database/sql-database-firewall-configure) kısıtlanmıştır.

-   [Sütunları'her zaman şifreli](/azure/sql-database/sql-database-always-encrypted-azure-key-vault) hasta birinci, ikinci ve son adları korumak için kullanılır.
    Ayrıca, veritabanı sütun şifreleme Azure Active Directory (AD) uygulama Azure SQL veritabanı kimlik doğrulaması için de kullanır.

-   SQL veritabanı ve SQL Server'a erişim, en az ayrıcalık ilkesini göre yapılandırılır.

-   Yalnızca gerekli IP adresleri SQL güvenlik duvarı aracılığıyla erişmesine izin verilir.

### <a name="storage-accounts"></a>Depolama hesapları


-   [Hareket halindeki veriler, yalnızca TLS/SSL kullanılarak aktarılır](/azure/storage/common/storage-require-secure-transfer?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json).

-   Kapsayıcılar için anonim erişime izin verilmez.

-   Uyarı kuralları, anonim bir etkinliği izlemek için yapılandırılır.

-   HTTPS, depolama hesabı kaynaklarına erişmek için gereklidir.

-   Kimlik doğrulama isteği verileri günlüğe ve izlenir.

-   Blob depolama alanındaki veriler bekleme durumundayken şifrelenir.

## <a name="analyze"></a>ANALİZ EDİN

### <a name="machine-learning"></a>Machine Learning


- [Günlüğe kaydetme etkinleştirildiğinde](/azure/machine-learning/studio/web-services-logging) için Machine Learning Studio web hizmetleri.
- Kullanarak [Machine Learning Studio](/azure/machine-learning/studio/what-is-ml-studio) geliştirmeye yönelik bir çözüm kümesine tahmin etme olanağı sağlayan denemeleri gerektirir.

## <a name="security"></a>GÜVENLİK

### <a name="azure-security-center"></a>Azure Güvenlik Merkezi
- [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/) tüm Azure kaynaklarınızın güvenlik durumunu merkezi bir görünümünü sağlar. Bir bakışta, uygun güvenlik denetimlerinin uygulandığını ve doğru bir şekilde yapılandırıldığını ve ilgilenmenizi gerektiren tüm kaynakları hızla tanımlayın doğrulayabilirsiniz. 

- [Azure Danışmanı](/azure/advisor/advisor-overview) olan yardımcı olan kişiselleştirilmiş bulut Danışmanı, Azure dağıtımlarınızın iyileştirilmesine yönelik en iyi uygulamaları izleyin. Danışman, kaynak yapılandırmanızı ve kullanım telemetrinizi analiz ederek Azure kaynaklarınızın maliyet verimliliğini, performansını, yüksek kullanılabilirliğini ve güvenliğini geliştirmenize yardımcı olabilecek çözümler önerir.

### <a name="application-insights"></a>Application Insights
- [Application Insights](/azure/application-insights/app-insights-overview) birden çok platformlardaki web geliştiricilerine yönelik genişletilebilir bir uygulama performans yönetimi (APM) hizmetidir. Canlı web uygulamanızı izlemek için kullanabilirsiniz. Bu performans anomalileri algılar. Sorunları tanılamanıza ve kullanıcıların uygulamanızla aslında neler yaptığını anlamanıza yardımcı olan güçlü analiz araçları içerir. Performansı ve kullanılabilirliği sürekli geliştirmenize yardımcı olmak amacıyla tasarlanmıştır.

### <a name="azure-alerts"></a>Azure Uyarıları
- [Uyarılar](/azure/azure-monitor/platform/alerts-metric) Azure hizmetlerini izleme, bir yöntem sunan ve veriler üzerinde koşullar yapılandırmanıza olanak sağlar. Bir uyarı durumu izleme verilerini eşleştiğinde uyarılar, bildirimler de sağlar.

### <a name="azure-monitor-logs"></a>Azure İzleyici günlükleri
[Azure İzleyici günlüklerine](/azure/operations-management-suite/operations-management-suite-overview) Yönetim Hizmetleri koleksiyonudur.

-   Güvenlik Merkezi için çalışma alanı etkindir

-   İş yükü izleme için çalışma alanı etkindir

-   İş yükü izleme için etkinleştirilir:

    -   Kimlik ve Erişim

    -   Güvenlik ve Denetim

    -   Azure SQL DB analizi

    -   [Azure WebApp Analytics](/azure/log-analytics/log-analytics-azure-web-apps-analytics) çözümü

    -   Key Vault Analytics

    -   Değişiklik İzleme

-   [Application Insights Bağlayıcısı (Önizleme)](/azure/log-analytics/log-analytics-app-insights-connector) etkin

-   [Etkinlik günlüğü analizi](/azure/log-analytics/log-analytics-activity) etkin
