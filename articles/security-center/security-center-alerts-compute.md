---
title: Tehdit algılama için Azure Güvenlik Merkezi'nde bulut yerel işlem | Microsoft Docs
description: Bu konuda, Azure Güvenlik Merkezi'nde kullanılabilir yerel işlem uyarılarını bulut sunulmaktadır. Azure Güvenlik Merkezi'nde.
services: security-center
documentationcenter: na
author: monhaber
manager: rkarlin
editor: ''
ms.assetid: 5aa5efcf-9f6f-4aa1-9f72-d651c6a7c9cd
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/02/2019
ms.author: monhaber
ms.openlocfilehash: bc3cb66d43e71777e06c6bd63dcff35e2ff19df8
ms.sourcegitcommit: 1e347ed89854dca2a6180106228bfafadc07c6e5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67571692"
---
# <a name="threat-detection-for-cloud-native-compute-in-azure-security-center"></a>Tehdit algılama için Azure Güvenlik Merkezi'nde yerel işlem bulut

Bir bulut sağlayıcısı olarak, Güvenlik Merkezi, iç günlüklerini analiz etme ve birden çok hedef üzerinde saldırı yöntemleri tanımlamak için olan benzersiz görünürlük yararlanır. Bu konu, aşağıdaki Azure Hizmetleri için kullanılabilir uyarıları sunar:

* [Azure uygulama hizmeti](#app-services)
* [Kapsayıcılar](#azure-containers) 

## Azure uygulama hizmeti <a name="app-services"></a>

Güvenlik Merkezi, Azure App Service üzerinde çalışan müşterilerin uygulamalarını hedefleyen saldırılara tanımlamak için bulutun ölçek yararlanır. Neredeyse tüm modern ağında olan web uygulamalarıyla saldırganlar zayıf yararlanma ve bunları bulmak için araştırma. Belirli ortamlara yönlendirilmeden önce Azure'da çalışan uygulamalar için istekleri inceledi günlüğe kaydedilir ve burada çeşitli ağ geçitleri üzerinden gider. Bu veriler, ardından açıklarını, saldırganlar, tanımlamak ve daha sonra kullanılacak yeni desenler bilgi almak için kullanılır.

Bir bulut sağlayıcısı olarak Azure olan görünürlüğü yararlanarak, Güvenlik Merkezi, birden çok hedef üzerinde saldırı yöntemleri tanımlamak için App Service iç günlüklerini analiz eder. Örneğin, yaygın tarama ve dağıtılmış saldırıları. Bu tür bir saldırı, genelde küçük bir kısmı IP'ler gelir ve arama savunmasız sayfası veya eklenti için birden çok ana bilgisayarda benzer uç noktalar ile gezinme, desenleri sergiler. Bu bulut kullanılarak algılanabilir ancak tek bir ana açısından tanımlanamıyor.

Güvenlik Merkezi temel alınan sanal ve sanal makineleri için ayrıca erişebilir. Bellek adli birlikte altyapı hikayeyi, müşteri makinelerde güvenlik ihlalleri için yazılımların döngüye yeni bir saldırı söyleyebilirsiniz. Bu nedenle, Güvenlik Merkezi yararlanılmasını sonra web uygulamaları uzun saldırıları algılayabilir.

> [!div class="mx-tableFixed"]

|Uyarı|Açıklama|
|---|---|
|**Algılanan kuşkulu WordPress tema çağırma**|Azure App Service etkinlik günlüğü, App Service kaynak olası kod ekleme faaliyete gösterir.<br/> Bu şüpheli etkinlik, doğrudan web isteği tarafından yönetilebilen bir tema dosyası çağırmak ve ardından kod, sunucu tarafı yürütülmesini desteklemek için bir WordPress tema işleyen etkinliği benzer. Bu tür bir etkinlik geçmişte bir saldırı kampanyasını bir parçası olarak WordPress görüldü.|
|**Web sayfası bağlantısı anormal IP adresi algılandı**|Azure App Service etkinliği günlük daha önce hiç bağlı bir kaynak adresten hassas bir web sayfasına bir bağlantı gösterir. Bu, biri Web uygulaması yönetim sayfalarınıza doğrudan bir deneme yanılma saldırısı çalışıyor olduğunu gösteriyor olabilir. Geçerli bir kullanıcı tarafından kullanılan yeni bir IP adresi sonucunu de olabilir.|
|**Tehdit zekası, Azure App Service FTP arabirime bağlı bir IP bulundu**|Azure App Service FTP günlükleri analiz, tehdit zekası akışı bulunan bir kaynak adresinden bağlantı algıladı. Bu bağlantı sırasında bir kullanıcı, aşağıda listelenen sayfaları erişilir.|
|**Algılanan Web izinden**|Azure App Service etkinlik günlüğü, App Service kaynak etkinliği izinden olası bir web gösterir. <br/>Bu şüpheli etkinlik algılandı görme fil adlı bir aracı ile ilişkilidir. Aracı web sunucusu parmak izi ve yüklü uygulamalar ve bunların sürümlerini algılamaya çalışır. Saldırganlar genellikle bu araç web uygulamalarının yoklaması için güvenlik açıkları bulmak için kullanın.|
|**Büyük olasılıkla saldırılara açık web sayfasına algılanan şüpheli erişim**|Azure uygulama hizmeti Etkinlik günlüğünü hassas görünüyor bir web sayfası erişildi gösterir. <br/>Bir web tarayıcısı olan erişim desenine benzer bir kaynak adresinden kaynaklanan bu şüpheli etkinlik. Genellikle bu tür bir etkinlik gerçekleştirip deneyin ve hassas veya savunmasız web sayfalarına erişim kazanmak için bir saldırgan tarafından denemesi ile ilişkili.|
|**PHP dosyasını karşıya yükleme klasörü**|Azure uygulama hizmeti Etkinlik günlüğünü bir şey karşıya yükleme klasöründe yer alan şüpheli bir PHP sayfasına eriştiğini belirtir. <br/>Bu tür bir klasör, genellikle PHP dosya içermiyor. Bu dosya türü varlığını rastgele dosya karşıya yükleme güvenlik açıklarından faydalanarak bir istismar gösterebilir.|
|**Bir Windows App Service'te Linux komutlarını çalıştırma denemesi**|App Service işlemlerin analiz Windows App Service üzerinde bir Linux komutu çalıştırma denemesi algıladı. Bu eylem, web uygulaması tarafından çalışıyordu. Bu davranışı çok yaygın bir web uygulamasının bir güvenlik açığından yararlandıktan kampanyaları sırasında görülür.|
|**Algılanan şüpheli PHP yürütme**|Makine günlükleri belirten bir şüpheli bir PHP işlemi çalışıyor. İşletim sistemi komutları veya PHP kodunu PHP işlemi kullanarak komut satırından çalıştırma denemesi eylemi dahil. Bu davranış yasal olabilir, ancak web uygulamalarında bu davranışı Ayrıca Web siteleri ile web Kabukları bulaşmak girişimleri gibi kötü amaçlı etkinlikleri gözlemlenen.|
|**Geçici klasörden işlem yürütme**|App Service işlemler analiz, bir uygulamanın geçici klasöründen bir işlemin yürütülmesi algıladı. Web uygulamalarında bu davranış yasal olabilir, ancak bu davranış kötü amaçlı etkinlikleri de gözlenir.|
|**Algılanan yüksek ayrıcalık komut çalıştırma denemesi**|App Service işlemlerin analiz yüksek ayrıcalıkları gerektiren bir komut çalıştırma denemesi algıladı. Web uygulaması bağlamında bir komut çalıştı. Web uygulamalarında bu davranış yasal olabilir, ancak bu davranış kötü amaçlı etkinlikleri de gözlenir.|

> [!NOTE]
> App Service için Güvenlik Merkezi tehdit algılama şu anda Azure devlet kurumları ve bağımsız bulut bölgelerinde kullanılabilir değil.

Uyarılar, App Service tehdit algılama hakkında daha fazla bilgi için Azure Güvenlik Merkezi ile koruma App Service'ı ziyaret edin ve izleme ve App Service iş yüklerinizi korumasını etkinleştirmek nasıl gözden geçirin.

## Kapsayıcıları <a name="azure-containers"></a>

Güvenlik Merkezi, gerçek zamanlı algılama için kapsayıcılarınızı auditd framework tabanlı Linux makinelerinde sağlar. Uyarıları göstergesidir güvenli Kabuk (SSH) sunucusunun bir Docker kapsayıcısı ya da şifreleme madencilerinin kullanımını içinde çalışan bir ana bilgisayarda ayrıcalıklı bir kapsayıcı oluşturma gibi birkaç şüpheli Docker etkinliği tanımlayın. Bu bilgileri kullanarak güvenlik sorunlarını hızlı bir şekilde çözebilir ve kapsayıcılarınızın güvenlik düzeyini artırabilirsiniz. Linux algılamalar yanı sıra Güvenlik Merkezi ayrıca kapsayıcıları dağıtımları için ayrıntılı analizler sunar.
