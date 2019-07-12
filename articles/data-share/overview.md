---
title: Azure veri paylaşımı önizlemesi nedir
description: Azure veri paylaşımı Önizlemesi'ne genel bakış
author: joannapea
ms.service: data-share
ms.topic: overview
ms.date: 07/10/2019
ms.author: joanpo
ms.openlocfilehash: 7d4e51ec9564bfb123cf73d9fe89d040f42fe650
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67807552"
---
# <a name="what-is-azure-data-share-preview"></a>Azure Veri Paylaşımı Önizleme nedir?

Günümüzün dünyasında, müşterileri ve iş ortakları ile basit ve güvenli bir şekilde paylaşmak için birçok kuruluş gereken anahtar stratejik varlıklar olarak verileri görüntülenir. Müşteriler bu Bugün, FTP aracılığıyla e-posta, dizileri API'leri dahil yapmak birçok yolu vardır. Kuruluşlar, kimin, tüm verilerini sahip oldukları paylaştığınız izleme kolayca kaybedebilir. Ayakta kendi API altyapı kurmak veya FTP aracılığıyla veri paylaşımı genellikle sağlamak ve yönetmek pahalı olur. Büyük ölçekte paylaşımı bu yöntemleri kullanmayla ilişkili yönetim yükünü yoktur. 

Birçok kuruluşun paylaşılan sahip verileri sorumlu olması gerekir. Sorumluluk yanı sıra pek çok kuruluş denetlemek, yönetmek ve tüm basit bir şekilde paylaşımı verilerini izlemek ister misiniz? Burada bir üstel yükselmeye devam etmek için veri bekleniyordu, bugün dünyasında, kuruluşların büyük veri paylaşımı için basit bir yol gerekir. Müşteriler, zamanında içgörülere sahip olun mümkün olmasını sağlamak için en güncel verileri isteğe bağlı.

Azure veri paylaşma Önizleme basit ve güvenli bir şekilde veri müşteriler ve iş ortakları ile paylaşmak, kuruluşların sağlar. Yalnızca birkaç tıklama ile yeni bir veri paylaşımı hesabı sağlayın, veri kümeleri ekleme ve müşteriler ve iş ortakları, veri paylaşımı için davet edin. Veri sağlayıcıları her zaman paylaşılan sahip veri denetimi sizdedir. Azure veri paylaşımını yönetmek ve ne zaman ve kim tarafından hangi verilerin paylaşıldı izlemek basit hale getirir. 

Bir veri sağlayıcısı, veri paylaşımı için kullanım koşullarını belirterek, verilerin nasıl işlendiğini denetim takip edebilirsiniz. Veri tüketicisi verileri almak için önce bu koşulları kabul etmeniz gerekir. Veri sağlayıcıları, veri tüketicilerinin güncelleştirmeleri aldıkları sıklığını belirleyebilirsiniz. Yeni güncelleştirmeler erişimi veri sağlayıcısı tarafından herhangi bir zamanda iptal edilebilir. 

Azure veri paylaşımına verileri analiz ve yapay ZEKA senaryoları zenginleştirmek için üçüncü tarafların birleştirmek kolay hale getirerek içgörüler geliştirmek yardımcı olur. Kolayca hazırlamak, işlemek ve Azure veri paylaşımı kullanılarak paylaşılan verileri çözümlemek için güç ot Azure analiz araçlarını kullanın. 

## <a name="scenarios-for-azure-data-share"></a>Azure veri paylaşımı için senaryolar

Azure veri paylaşımı farklı endüstriler bir süre içinde kullanılabilir. Örneğin, bir perakende satış veri son noktası ile bunların tedarikçileri paylaşmak isteyebilirsiniz. Azure veri paylaşımı kullanarak bir satıcıya, tüm bunların tedarikçileri için satış veri noktası içeren bir veri paylaşımı ayarlama ve satış günlük veya saatlik olarak paylaşın. 

Azure veri paylaşımı, belirli bir sektör için bir veri marketi kurmak için de kullanılabilir. Örneğin, bir kamu veya düzenli olarak üçüncü taraflarla nüfus artışı hakkında anonim verileri paylaşan bir araştırma kuruluşu. 

Azure veri paylaşımı için başka bir kullanım örneği veri consortium oluşturabilirsiniz. Örneğin, bir dizi farklı araştırma kuruluşları, tek bir güvenilen gövde ile veri paylaşabilir. Veriler analiz, toplu veya Azure analiz araçlarını kullanarak işlenir ve ardından ile ilgili tarafların paylaşılan. 

## <a name="how-it-works"></a>Nasıl çalışır?

Azure veri paylaşımı, verilerin veri sağlayıcısının Azure aboneliğinden taşır ve veri tüketici'nin Azure aboneliğinde gölünüzdeki bir anlık görüntü tabanlı paylaşım yaklaşımı kullanılmaktadır. Bir veri sağlayıcısı olarak veri paylaşımı sağlama ve veri paylaşımına alıcılara davet edin. Veri tüketicileri, e-posta yoluyla veri paylaşımı için bir davet alırsınız. Veri tüketicisi daveti kabul ettikten sonra tam bir anlık görüntü tetikleyebilirsiniz paylaşılan verilerin bunları paylaştığınız. Bu verileri veri tüketicileri depolama dikkate alınır. Veri tüketicileri, böylece her zaman en son sürümünü veri olduğu kendileriyle Paylaşılan verilere normal, artımlı güncelleştirmeleri alabilirsiniz. 

Veri sağlayıcıları verilerine sunabilir kendileriyle paylaşılan bir anlık görüntü zamanlamalarını veri tüketicileri artımlı güncelleştirmeleri. Anlık görüntü zamanlamaları bir saatlik veya günlük olarak sunulmaktadır. Veri tüketicisi kabul eder ve kendi veri paylaşımını yapılandırır, bir anlık görüntü zamanlama için abone olabilirsiniz. Bu, burada paylaşılan veriler düzenli olarak güncelleştirilir ve en güncel verileri veri tüketici gereken senaryolarda yararlıdır. 

![veri akışı paylaşma](media/data-share-flow.png)

Veri paylaşımına veri tüketici kabul ettiğinde, bunlar PKI'nizin bir depolama hesabına veri alma olanağına sahip olursunuz. Örneğin, veri sağlayıcısı, verileri Azure Blob depolamayı kullanarak paylaşırsa, bu verileri Azure Data Lake Store veri tüketici alabilir. 

## <a name="key-capabilities"></a>Temel işlevler

Azure veri paylaşımına veri sağlayıcıların aşağıdakileri yapmasını sağlar:

* Müşteriler ve iş ortakları, kuruluşunuzun dışındaki Azure Data Lake Store ve Azure depolama veri paylaşın

* Kimin verilerinizle paylaştığı izler

* Veri tüketicilerinin verilerinize güncelleştirmeleri ne sıklıkla alıyorsanız

* Müşterilerinizin en son sürümünü verilerinizi gerektiği şekilde çekme izin vermek veya bunları otomatik olarak artımlı değişiklikler için tanımladığınız bir aralıkta almaya izin ver

Azure veri paylaşımı, veri tüketicilerinin sağlar: 

* Paylaşılan veri türünü açıklamasını görüntüleyin

* Veriler için kullanım koşullarını görüntüle

* Kabul edin veya bir Azure veri paylaşımı davet Reddet

* Bir kuruluş, sizinle paylaştığı bir veri paylaşımı tam veya artımlı anlık görüntüsünü tetikleyin

* Artımlı anlık görüntü kopyalama aracılığıyla veriler en son kopyasını almak için bir veri paylaşımı için abone olun

* Bir Azure Blob Depolama veya Azure Data Lake Gen2 hesaba paylaşılan veri kabul edin

Yukarıda listelenen tüm anahtar özellikleri, Azure veya REST API'leri aracılığıyla desteklenir. Azure REST API'leri aracılığıyla veri paylaşımını kullanma hakkında daha fazla ayrıntı için başvuru belgelerimize bakın. 

## <a name="security"></a>Güvenlik

Azure veri paylaşımı, bekleyen ve Aktarımdaki verileri korumak için Azure'un sunduğu temel güvenlik yararlanır. Veriler bekleme durumundayken şifrelenir temel alınan depolama mekanizması tarafından desteklenen durumlarda. Veriler de Aktarım sırasında şifrelenir. Bir veri paylaşımı hakkında meta veriler de beklerken ve aktarım sırasında şifrelenir. 

Erişim denetimleri yetkisi olan kişiler erişilebilir emin olmak için Azure veri paylaşımı kaynak düzeyinde ayarlanabilir. 

Azure veri paylaşımı, Azure Active Directory'de otomatik kimlik yönetimi için (daha önce Msı'ler da bilinir) Azure kaynakları için yönetilen kimlikleri kullanır. Azure kaynakları için yönetilen kimlikleri, veri paylaşımı için kullanılmakta olan depolama hesaplarına erişim için kullanılabilir. Hiçbir exchange arasında bir veri sağlayıcısı ve bir veri tüketici kimlik bilgilerinin yoktur. Daha fazla bilgi için [sayfası Azure kaynakları için yönetilen kimlikleri](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/services-support-managed-identities). 

## <a name="supported-regions"></a>Desteklenen bölgeler

Azure veri paylaşımı kullanılabilir hale getirdiğiniz Azure bölgeleri listesi için bkz [bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/global-infrastructure/services/) sayfası ve Azure veri paylaşımı arayın. 

Azure veri paylaşımı, tüm verileri depolamaz. Veriler, paylaşılan temel alınan depolama hesaplarında depolanır. Örneğin, bir veri üreticisinin Batı ABD'de bulunan bir Azure Data Lake Store hesabındaki verileri içermiyorsa, verilerin depolandığı olmasıdır. Batı Avrupa'da bulunan bir Azure depolama hesabı ile verileri paylaşıyor, veriler Batı Avrupa'da bulunan Azure depolama hesabına doğrudan aktarılır. 

Azure veri paylaşımı service hizmetini kullanarak kendi Bölgenizde kullanılabilir olması yok. Örneğin, burada Azure veri paylaşımı henüz kullanılabilir olmayan bir bölgede yer alan bir Azure depolama hesabında depolanan verileriniz varsa, yine de verilerinizi paylaşmak için hizmetinden yararlanabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

Verileri paylaşmaya başlayın öğrenmek için devam [verilerinizi paylaşmak](share-your-data.md) öğretici.
