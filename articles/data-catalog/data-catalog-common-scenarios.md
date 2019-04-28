---
title: Azure Veri Kataloğu genel senaryoları
description: Yaygın Azure veri kataloğu için senaryo kayıt ve yüksek değerli veri kaynağı bulma gibi Self Servis iş zekası etkinleştirme ve mevcut veri kaynakları ve işlemleri hakkında bilgi alın yakalama, genel bakış.
services: data-catalog
author: JasonWHowell
ms.author: jasonh
ms.assetid: 60930d78-d2d4-4d5d-9651-bdda50b0da0e
ms.service: data-catalog
ms.topic: conceptual
ms.date: 01/18/2018
ms.openlocfilehash: e95cc64b9086a6fb4c5e2d42521a5fd3f44244ba
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61003965"
---
# <a name="azure-data-catalog-common-scenarios"></a>Azure Veri Kataloğu genel senaryoları
Bu makalede Azure veri Kataloğu kuruluşunuz var olan veri kaynaklarından daha fazla değer elde burada yardımcı olabilecek yaygın senaryolar sunulmakta.

## <a name="scenario-1-registration-of-central-data-sources"></a>Senaryo 1: Merkezi bir veri kaynağı kaydı
Kuruluşlar genellikle birçok yüksek değerli veri kaynağına sahiptir. Bu veri kaynakları, iş kolu satır, çevrimiçi işlem (gerçekleştirme OLTP) sistemleri, veri ambarları ve iş zekası/analiz veritabanlarını işleme içerir. Sistemleri ve bunlar arasındaki çakışma sayısı, genellikle iş gereksinimleriniz ve örneğin, birleşmeler ve satın almalar, iş geliştikçe zamanla artar.

Kuruluş üyelerinin bu veri kaynaklarının içinde verileri bulmak nereden başlayacağınızı bilemiyor zor olabilir. Aşağıdaki gibi sorulara tüm çok ortaktır:

* Şirket içinde kullanılan üç ik sistemleri, hangi türde rapor oluşturmak için kullanmalıyım?
* Sertifikalı satış rakamlarının yalnızca sona erdi mali yıl için almak için nereye Git?
* Kimin miyim istemelisiniz veya veri ambarı erişmek için kullanmanız gerekir işlemi nedir?
* Bu sayı doğru olup olmadığını bilmiyorum. Kimin hakkındaki bilgiler için nasıl bu verileri takımımla birlikte oluşturduğum bu panoya paylaşabilir önce kullanılan geç üzerinde nüfusuna?

Bu ve diğer sorular için Azure veri Kataloğu yanıtlar sağlayabilir. Kuruluş genelinde kullanılan merkezi yüksek değerli, BT tarafından yönetilen veri kaynaklarını genellikle Kataloğu doldurmak için mantıksal başlangıç noktasıdır. Herhangi bir kullanıcı bir veri kaynağına kaydedebilirsiniz olsa da, en fazla sayıda kullanıcı için değer sağlayın olasılığı en yüksek veri kaynaklarıyla kick-started Kataloğu sahip benimsenmesini ve sistem kullanımını yardımcı olur. 

Azure veri Kataloğu ile başlıyorsanız tanımlayarak ve kaydederek veri tüketicileri, birçok farklı ekip tarafından kullanılan önemli veri kaynakları, ilk adımınız başarılı olabilir.

Bu senaryo ayrıca anlamak ve erişimi daha kolay hale getirmek için yüksek değerli veri kaynaklarına açıklama eklemek için bir fırsat sunar. Bu anahtar yönlerinden biri, kullanıcılar veri kaynağına erişimi nasıl isteyebilir bilgi eklemektir. Azure veri Kataloğu ile e-posta adresini kullanıcı veya mevcut araçları ya da belgeler için veri kaynağı erişimi denetleme, bağlantılar için sorumlu bir ekip veya erişim isteği işlemini açıklar serbest metin sağlayabilir. Bu bilgiler kim kayıtlı veri kaynaklarını bulma ancak kimin henüz kolayca tanımlanan ve veri kaynağı sahipleri tarafından denetlenen işlemleri kullanarak erişim istemek için veri erişim izni olmayan üyeleri yardımcı olur.

## <a name="scenario-2-self-service-business-intelligence"></a>Senaryo 2: Self Servis iş zekası
Birçok kuruluş veri ortamlarını her bir parçası olarak, geleneksel kurumsal iş zekası çözümleri devam etse de, adım iş kolu değiştirme Self Servis BI ve daha önemli yaptı. Self Servis BI'ı kullanarak, bilgi çalışanları ve analistleri kendi raporlar, çalışma kitapları ve panolar merkezi bir BT ekip veya BT takımın zamanlama ve kullanılabilirlik tarafından kısıtlanmasını bağlı kalmadan oluşturabilirsiniz.

Self Servis BI senaryolarda kullanıcıların çoğu daha önce BI ve analiz için kullanılmış değil, birden çok kaynaktan veri yaygın olarak birleştirin. Bu veri kaynaklarının bazıları zaten biliniyor olabilir ancak bunu bulmak ve değerlendirmek için belirli bir görevin olası veri kaynakları için yapmanız gerekenler bulmak zor olabilir.

Geleneksel olarak, bu bulma işlemi el ile bir paroladır: analistleri, diğer kişilerin Aranan verileri ile çalışma belirlemek için kendi eş ağ bağlantılarını kullanın. Bir veri kaynağı bulunabilir ve kullanılabilir sonra işlem tekrar her sonraki Self Servis BI çabayla, birden çok kullanıcı bulma yedekli manuel bir işlem gerçekleştiriliyor yinelenir.

Azure veri Kataloğu ile kuruluşunuz bu döngü çaba bozabilir. Geleneksel araçlarla veri kaynağı bulma sonra bir analist daha kolay bulunabilir diğer kullanıcılar tarafından gelecekte hale kaydedebilirsiniz. Analist, kayıtlı veri varlıklarına daha fazla değer ekleyebilirsiniz, ancak bu ek açıklama aynı zamanda kayıt gerçekleşmesi gerekmez. Kullanıcılar kendi zamanlamaları izin, zaman içinde aşamalı olarak değer kataloğa veri kaynaklarını ekleme katkıda bulunabilir.

Bu organik büyüme katalog içeriğinin ön kayıt merkezi veri kaynakları için doğal bir tamamlayıcı ' dir. Birçok kullanıcıların veri Kataloğu'na önceden doldurmak için ilk kullanım ve bulma bir motivator olabilir. Kullanıcılara kaydolmayı ve kaynaklarına ek açıklama etkinleştirme, bunları ve diğer kuruluş üyelerinin bağlı tutmak için bir yol olabilir.

Bu, aynı desenleri ve zorlukları bu senaryo, özellikle Self Servis BI odaklanıyor olsa da, büyük ölçekli Kurumsal BI projeleri için de uygulama hatalarının ayıklanabileceğini belirtmekte yarar. Kuruluşunuz, veri kataloğu kullanarak el ile bir veri kaynağı bulma işlemini içeren herhangi bir çaba artırabilir.

## <a name="scenario-3-capturing-tribal-knowledge"></a>Senaryo 3: Yakalama açılıyor
İşiniz ve bu verileri nerede bulacağını yapmanız gereken hangi verilerin nasıl bilebilirsiniz?

Bir süredir işinizi kılavuzluğa odaklanmamı varsa, büyük olasılıkla yalnızca bilirsiniz. Öğrenme sürecinde bir gradual incelediğinize ve zaman içinde anahtar günlük işlerinizde çalışmak için veri kaynakları hakkında öğrendiniz.

Nasıl yeni bir çalışan takımınız katıldığında, bu kişiye hangi verileri iş için gereklidir ve nerede bulacağını bilir?

Büyük olasılıkla olan yeni bir kişi size bu sorularla birlikte gelir.

Devam eden bu aktarım açılıyor, küçük ve büyük ölçekli kuruluşlarda veri kaynağı bulma işleminin bir parçasıdır. Daha üst düzey ve deneyimli takım üyelerinin Bilgi Bankası ' yıllar içinde yerleşik ve yeni takım üyeleri, bunlar sorularınız varsa bunların yanıtlarını öğrendiniz. En önemli bilgileri genellikle yalnızca birkaç anahtar kişinin heads içinde var ve bu kişilere tatilde olduğu veya ekipten ayrıldığında, kuruluş alternatife.

Veri uzmanlarının normalde e-posta yoluyla veya bir takım SharePoint sitesindeki Word belgelerinde paylaşımını bilgilerini, belge için çaba hale getirin. Bu yaklaşım değerli olsa da, bunu yeni bir bulma sorun oluşturmaktadır: nasıl kişilere bildirin hangi belge var ve nerede bulacağını?

Azure veri Kataloğu ile tek, merkezi konuma depolamak ve bu açılıyor paylaşımı ve kolayca bulunabilmesini yapmak için kuruluşunuzun sahiptir. Veri Kataloğu'nda, veri uzmanlarından doğrudan veri varlıklarına açıklama ve mevcut olan belgelere bağlantılar sağlar. Kuruluşun üyeleri, bir veri kaynağı için katalog kullandığınızda, bunlar yalnızca kaynak kendisi, aynı zamanda önceden var olan bir Bilgi Bankası yalnızca kuruluşunuzun uzmanlarına kulak vermek içinde bulabilirsiniz.
