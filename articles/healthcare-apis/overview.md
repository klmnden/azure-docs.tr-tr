---
title: Azure API FHIR Önizleme - FHIR Önizleme için Azure API nedir
description: Bu makalede, Azure API için FHIR açıklanır.
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.topic: overview
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: 327c28b70c564f7e5099ada14a0d4653b7f0db6c
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55823557"
---
# <a name="what-is-azure-api-for-fhirreg-preview"></a>Azure API FHIR için nedir&reg; Önizleme?
Azure API FHIR için hızlı exchange bulutta sunan bir yönetilen Platform-bir hizmet olarak (PaaS) tarafından desteklenen FHIR API'leri aracılığıyla veri sağlar. Herkes alımı için Sistem Durumu verileriyle çalışma yönetmek ve korunan sağlık bilgileri kalıcı hale getirmek için kolaylaştırır [PHI](https://www.hhs.gov/answers/hipaa/what-is-phi/index.html) bulutta: 

- Dakikalar içinde bulutta sağlanan FHIR hizmeti yönetilen 
- Kurumsal düzeyde, uç nokta FHIR® tabanlı veri erişimi için azure'da ve FHIR® biçimde depolama
- Yüksek performanslı, düşük gecikme süresi
- Bir uyumlu bulut ortamında güvenli Yönetimi korumalı sistem durumu verileri (PHI)
- Akıllı FHIR üzerinde mobil ve web uygulamaları
- Rol tabanlı erişim denetimi ile (RBAC) uygun ölçekte kendi verilerinizi denetleme
- Denetim günlük izleme için erişim, oluşturulması, değiştirilmesi ve her veri deposunda okuma

Azure API FHIR için FHIR hizmet bulutun esnek ölçeklendirme yararlanmak için sadece birkaç dakika içinde oluşturup sağlar.  Yalnızca aktarım hızını ve ihtiyacınız olan depolama için ödeme yaparsınız. Azure API için FHIR güç Azure hizmetlerini, yönettiğiniz hangi boyutu veri kümeleri ne olursa olsun daha hızlı performans için tasarlanmıştır.  99. yüzdebirlik gecikme sürelerini garanti eder ve çoklu yönlendirme özelliğiyle yüksek kullanılabilirliği garanti eder Azure Cosmos DB, Azure API FHIR için veri saklama katmanını yararlanır. 

FHIR API ve uyumlu veri deposu, güvenli bir şekilde bağlanın ve FHIR API kullanan herhangi bir sistemi ile etkileşim sağlar.  Böylece kendi işlem ve geliştirme kaynakları boşaltabilir işlemleri, bakım, güncelleştirmeleri ve uyumluluk gereksinimlerini PaaS teklifi, Microsoft alır. 

## <a name="leveraging-the-power-of-your-data-with-fhir"></a>**FHIR verilerinizle gücünden yararlanan**
Sağlık sektörü ortaya çıkan standardı olan sistem durumu verileri hızlıca dönüştürüyor [FHIR&reg; ](https://hl7.org/fhir) (hızlı sağlık birlikte çalışabilirlik kaynaklar). FHIR FHIR birlikte kullanarak tüm sistemlerin sağlayan standart semantiği ve veri değişimi ile bir güçlü ve Genişletilebilir bir veri modeli sağlar.  FHIR verilerinizi dönüştürme, hızlı bir şekilde elektronik sağlık kaydı sistemleri gibi mevcut veri kaynaklarına bağlanmak veya veritabanları araştırma olanak tanır. FHIR ayrıca modern mobil uygulamaları ve web geliştirme verileri hızlı alışverişi sağlar. En önemlisi de FHIR veri alımı basitleştirin ve analiz ve makine öğrenme araçlarını geliştirme sürecinizi hızlandırın.  

### <a name="securely-manage-health-data-in-the-cloud"></a>**Sistem Durumu verileri bulutta güvenli bir şekilde yönetme**
Azure API FHIR için veri değişimi için sağlar aracılığıyla tutarlı, RESTful, FHIR API'leri HL7 FHIR belirtimini temel alır. Sunan Azure'da yönetilen bir PaaS ile desteklenen, ayrıca ölçeklenebilir ve güvenli bir ortam için yerel FHIR biçimde korumalı sağlık bilgilerini (PHI) veri depolama ve yönetimi sağlar.  

### <a name="free-up-your-resources-to-innovate"></a>**Kaynaklarınızın yenilik için ücretsiz**
Kaynakları oluşturma ve kendi FHIR hizmet çalıştırma yatırım, ancak FHIR Azure API'si ile Microsoft, ücretsiz kendi işletimsel yukarı ve geliştirme işlemlerini, bakım, güncelleştirmeleri ve uyumluluk gereksinimlerini, iş yüküne alır kaynaklar.

### <a name="enable-interoperability-with-fhir"></a>**FHIR ile birlikte çalışabilirlik etkinleştir** 
FHIR için Azure API'sini kullanarak sağlar, okuma, yazma, arama ve diğer işlevleri için FHIR API'lerden yararlanır; her sistemiyle bağlanın.  Bu güçlü bir araç olarak birleştirmek, normalleştirmeye çalışır ve makine öğrenimi, FHIR API'leri veritabanlarını sisteminizi dışında veya elektronik sağlık kayıtlarına, clinician ve hasta panolar, Uzaktan izleme programlar Klinik veriler ile uygulamak için kullanılabilir.

### <a name="control-data-access-at-scale"></a>**Uygun ölçekte veri erişimi denetleme**
Sizin denetiminizdedir. Rol tabanlı erişim denetimi (RBAC), verilerinizin nasıl depolandığını ve erişilen yönetmenize olanak sağlar.  Artırılmıştır güvenlik ve yönetim yükünü azaltır, ortamınız için oluşturduğunuz rol tanımları temel oluşturma veri kümelerine kimlerin erişebileceğini belirler.  

### <a name="audit-logs-and-tracking"></a>**Denetim günlükleri ve izleme**
Yerleşik denetim günlükleri ile verilerinizi nerede bulunacağını hızlı bir şekilde izleyin. Erişim, oluşturulması, değiştirilmesi ve her veri deposunda okuma izleyin.

### <a name="secure-your-data"></a>**Verilerinizi güvenli hale getirme** 
Eşsiz güvenlik zekası ile PHI'nin koruyun.  Verilerinizi API örnek başına benzersiz bir veritabanı için yalıtılmış ve çok bölgeli yük devretme ile korumalı. Azure API FHIR için bir katmanlı ve derinlemesine savunma ve verileriniz için Gelişmiş tehdit koruması uygular.  

## <a name="applications-for-a-fhir-service"></a>**FHIR hizmet uygulamaları**
Sistem Durumu verilerini birlikte çalışabilirliği için anahtar araçları FHIR sunucularıdır.  Bir API'yi ve oluşturabilirsiniz, hizmet dağıtma ve hızlı bir şekilde kullanmaya başlamak Azure API FHIR için tasarlanmıştır.  FHIR standart içinde sağlık genişledikçe kullanım örnekleri büyümeye devam edecek, ancak Azure API FHIR için yararlı olduğu bazı ilk müşteri uygulamalar aşağıda verilmiştir: 

- **Başlangıç/IOT ve uygulama geliştirme:**  Hasta veya sağlayıcı odaklı bir uygulama geliştiren müşterilerin (mobil veya web) Azure API için tam olarak yönetilen bir arka uç hizmeti olarak FHIR yararlanabilirsiniz. Müşterileri Yönetme veri olabilir, FHIR için Azure API değerli kaynak sağlar ve veri değişimi sistem durumu veriler için tasarlanmış bir güvenli bir bulut ortamında, akıllı FHIR uygulama yönergeleri yararlanın ve teknolojiye tüm tarafından kullanılabilmesi etkinleştirin Sağlayıcı sistemleri (örneğin, çoğu EHRs API'leri okuma FHIR etkinleştirdiyseniz).   
- **Sağlık Hizmeti Ekosistemlerini:**  'Gerçeklik kaynağı' birçok Klinik Ayarları'nda birincil olarak EHRs mevcut olsa da, seyrek sağlayıcıları, farklı biçimlerde verilerini birbirine bağlı olmayan veya birden çok veritabanına sahip değil.  Bu sistemlerin en üstünde yer alan hizmet olarak FHIR için Azure API'sini kullanarak verileri FHIR biçiminde standart hale getirmek sağlar.  Bu veri değişimi ile tutarlı veri biçimi birden çok sistemler arasında etkinleştirmek yardımcı olur. 

- **Araştırma:** Ortak bir FHIR veri modeli çerçevesinde veri normalleştirir ve makine öğrenimi ve veri paylaşımı için iş yükünü azaltır sağlık Araştırmacıları FHIR standart genel ve Azure API FHIR için yararlı bulabilirsiniz.
Azure API aracılığıyla veri değişimi FHIR için denetim günlüklerini sağlar ve bu Yardım denetim akışı verileri ve hangi veri türlerini kimin erişimi denetler. 

## <a name="fhir-from-microsoft"></a>Microsoft gelen FHIR

Microsoft gelen FHIR özellikleri, iki yapılandırmada kullanılabilir:

* FHIR – A Azure'da sunan PaaS için Azure API kolayca Azure portalında sağlanan ve Microsoft tarafından yönetilir.
* FHIR Server için Azure – Azure aboneliğinize github'da kullanılabilir dağıtılabilir bir açık kaynak projesi https://github.com/Microsoft/fhir-server.

Kullanım için genişletme veya sunucunun FHIR özelleştirme gerektirir veya durumları temel Hizmetleri erişim — veritabanı gibi — FHIR API'ler aracılığıyla olmadan, geliştiricilerin Azure için açık kaynaklı FHIR sunucu seçmeniz gerekir.   Kullanıma hazır, üretime hazır FHIR API ve burada kalıcı veri yalnızca FHIR API aracılığıyla erişilmelidir arka uç hizmeti uygulaması için geliştiriciler Azure API için FHIR seçmeniz gerekir

## <a name="get-started"></a>başlarken

FHIR hizmeti ile çalışmaya başlamak için 5 dakikalık hızlı başlangıcı izleyin:

* Açık kaynak FHIR sunucusu dağıtmanızı [PowerShell](fhir-oss-powershell-quickstart.md)

## <a name="next-steps"></a>Sonraki adımlar

FHIR barındırılan hizmeti kurmak ayarladıktan sonra yapılandırın ve hizmeti test etmek için önemli adımlar:

* [Erişim FHIR Postman kullanarak hizmeti](access-fhir-postman-tutorial.md)

FHIR HL7 tescilli ticari markasıdır ve HL7 izinle kullanılır.
