---
title: Azure - Azure FHIR sunucusu FHIR sunucusu nedir
description: Bu makalede, Azure için FHIR sunucu açıklanır.
services: healthcare-apis
author: matjazl
ms.service: healthcare-apis
ms.topic: overview
ms.date: 02/07/2019
ms.author: matjazl
ms.openlocfilehash: 20b29ba4c23da90b4941b37d02a1d3f42310dd40
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55823572"
---
# <a name="what-is-fhir-server-for-azure"></a>Azure için FHIR Server nedir

FHIR&reg; sunucusudur Azure için açık kaynak uygulaması, Gelişmekte olan [HL7 hızlı sağlık birlikte çalışabilirlik kaynakları (FHIR) belirtimi](https://www.hl7.org/fhir/) Microsoft bulut için tasarlandı. FHIR belirtimi, verileri sistemler arasında birlikte çalışabilen yapılabilir nasıl Klinik sağlık tanımlar. FHIR Server için Azure, bulutta, birlikte çalışabilirlik kolaylaştırmak yardımcı olur. Bu Microsoft Healthcare projenin FHIR hizmeti hızlı bir şekilde dağıtmak geliştiricilerin olmaktır.
 
Azure etkinleştirir geliştiricilerin hızla alın ve bulutta FHIR veri kümelerini Yönet FHIR sunucu FHIR biçimde verilerle izlemek ve veri erişimini yönetmek ve makine öğrenimi iş yükleri için verileri Normalleştir. Azure için FHIR sunucusu, Azure ekosistemi için optimize edilmiştir: 
* Betikler ve ARM şablonları Microsoft Cloud anında sağlama için kullanılabilir 
* Betikleri Azure AAD eşleyin ve rol tabanlı erişim denetimi (RBAC) etkinleştirmek kullanılabilir 

FHIR Server için Azure, geliştiricilerin nasıl uygulanır ve gerektiğinde yeteneklerini genişletmek değiştirme esnekliği etkinleştirme mantıksal ayrılığı ile oluşturulmuştur. Mantık katmanları FHIR sunucusunun şunlardır: 

* Özel yapılandırma denetimi tersine çevirme (IOC) kapsayıcı ile farklı ortamlarda barındırma destekler katmanı – barındırma. 
* RESTful API katmanı – HL7 FHIR belirtimi tarafından tanımlanan bir API uygulaması. 
* Mantığı katmanı – çekirdek FHIR mantık uygulaması çekirdek. 
* Kalıcılık katman – FHIR neredeyse tüm veri kalıcılığı yardımcı programı bağlanmak sunucunun etkinleştirme takılabilir kalıcı bir sağlayıcı. Azure FHIR Server kullanıma hazır veri kalıcı bir sağlayıcı için Azure Cosmos DB (veriler üzerinde zengin sorgulama sunan bir Global olarak çoğaltılmış bir veritabanı hizmeti) içerir. 

Azure FHIR sunucusu zaman FHIR sunucusu kendi uygulamalarına hızlı bir şekilde tümleştirmek için ihtiyaç duydukları zaman kaydetme veya bunları kendi FHIR hizmeti üzerinde özelleştirebilecekleri ile sağlamaya geliştiricilere güçlendirir. Açık kaynaklı proje, bu projeyi geliştirmek Katkıları ve geri bildirim FHIR Geliştirici topluluğundan devam edecektir. 

Gizliliği ve güvenliği en önemli önceliklerden olan ve Azure FHIR sunucusu korumalı sağlık bilgilerini (PHI) gereksinimlerini desteklemek üzere geliştirilmiştir. Tüm Azure Hizmetleri için Azure FHIR sunucusunda kullanılan [korunan sağlık bilgileri uyumluluk gereksinimlerini](https://www.microsoft.com/en-us/trustcenter/compliance/complianceofferings).

Bu açık kaynaklı proje, tam olarak Microsoft Healthcare ekibi tarafından desteklenir, ancak bu proje yalnızca geri bildirim ve katkılar ile daha iyi alırsınız olduğunu biliyoruz. Biz bu kod tabanının Geliştirme lideri ve derleme ve dağıtım günlük test edin. 