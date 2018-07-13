---
title: Bulut hizmeti sağlayıcıları için Azure Stack için raporlama altyapınızın kullanım | Microsoft Docs
description: Azure Stack oluşur ve Azure'a iletir gibi bir bulut hizmeti sağlayıcısı (CSP) tarafından hizmet verilen kiracılar için kullanımını izlemek için gereken altyapıyı içerir.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2018
ms.author: mabrigg
ms.reviewer: alfredo
ms.openlocfilehash: 3dd17f9180bae5de2d59355326ad34528eae2713
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39005419"
---
## <a name="usage-reporting-infrastructure-for-cloud-service-providers"></a>Bulut hizmeti sağlayıcıları için raporlama altyapınızın kullanımı

Azure Stack gerçekleşir ve Azure'a iletir kullanımını izlemek için gereken altyapıyı içerir. Azure'da, Azure ticaret kullanım verileri işler ve uygun Azure abonelikleri için kullanım ücretlerini. Kullanımı izleme, Azure genel bulutunda izlenen gibi bu yolla gerçekleşir.

Belirli kavramları genel Azure ve Azure Stack arasında tutarlı olmasına dikkat etmelisiniz. Azure Stack benzer bir rol bir Azure aboneliğine karşılamak yerel abonelikler var. Yerel abonelikler yalnızca yerel olarak geçerlidir. Kullanım Azure'a iletildiğinde yerel abonelikler Azure aboneliklerine eşlenir.

Azure Stack yerel kullanım ölçümleri sahiptir. Yerel kullanım ölçümleri Azure ticaret kullanılan eşleştirilir. Ancak, ölçüm kimlikleri farklıdır. Daha fazla ölçümleri bir faturalandırma için Microsoft'un kullandığı daha yerel olarak kullanılabilir.

Azure Stack ve Azure hizmetleri nasıl fiyatlandırılır arasındaki bazı farklar vardır. Örneğin, Azure Stack'te Vm'leri için ücretlendirme yalnızca sanal çekirdek/saat ile aynı fiyat aksine Azure, tüm VM serisi için temel alır. Genel Azure'da farklı donanım farklı fiyatlara yansıtılmıştır nedenidir. Farklı VM sınıflar için farklı ücretler kaydedilecek neden olduğundan Azure Stack'te donanım, müşteri sağlar.

Ticaret ve iş ortağı Merkezi, kullanıcıların fiyatları Azure hizmetlerinde olduğu gibi aynı şekilde kullanılan Azure Stack ölçümleri hakkında bilgi edinebilirsiniz:

1. İş ortağı Merkezi'nde Git **Panosu menüsünden** > **fiyatlandırma ve teklifler**.
2. Altında **kullanım tabanlı Hizmetleri**seçin **geçerli**.
3. Açık **genel CSP fiyat Listesi'nde Azure'da** elektronik tablo.
4. Filtre **bölge Azure Stack =**.

## <a name="usage-and-billing-error-codes"></a>Kullanım ve faturalandırma hata kodları

Bir kaydı için Kiracı ekleme sırasında aşağıdaki hata iletilerinden karşılaşılabilir.

| Hata                           | Ayrıntılar                                                                                                                                                                                                                                                                                                                           | Yorumlar                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|---------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| RegistrationNotFound            | Sağlanan kayıt bulunamadı. Aşağıdaki bilgileri doğru bir şekilde sağlanan emin olun:<br>1. Abonelik tanımlayıcısı (sağlanan değer: _abonelik tanımlayıcısı_),<br>2. Kaynak grubu (sağlanan değer: _kaynak grubu_),<br>3. Kayıt adı (sağlanan değer: _kayıt adı_).                             | Bu hata genellikle ilk kayıt için işaret eden bilgiler doğru değil oluşur. Kaynak grubu ve kaydınızı adını doğrulamak gerekiyorsa, tüm kaynaklar listeleyerek Azure portalında bulabilirsiniz. Birden fazla kayıt kaynağı bulursanız, özelliklerinde CloudDeploymentID bakın ve ayarlanmış CloudDeploymentID bulut eşleştiği kaydı seçin. CloudDeploymentID bulmak için Azure Stack üzerinde bu PowerShell kullanabilirsiniz:<br>`$azureStackStampInfo = Invoke-Command -Session $session -ScriptBlock { Get-AzureStackStampInformation }` |
| BadCustomerSubscriptionId       | Sağlanan _müşteri abonelik tanımlayıcısı_ ve _kayıt adı_ abonelik tanımlayıcısı tarafından aynı Microsoft bulut hizmeti sağlayıcısına ait değil. Müşteri abonelik tanımlayıcısı doğru olduğundan emin olun. Sorun devam ederse desteğe başvurun. | Müşteri aboneliğinde bir CSP aboneliği olmakla birlikte, CSP iş ortağı için ilk kayıt için kullanılan abonelik için toplanan olandan farklı toplar, bu hata oluşur. Bu onay, kullanılan Azure Stack için sorumlu olmayan bir CSP iş ortağı faturalama neden olan bir durumu önlemek için yapılır.                                                                                                                                                                                                                                                                          |
| InvalidCustomerSubscriptionId   | '_Müşteri abonelik tanımlayıcısı_' geçerli değil. Geçerli bir Azure aboneliği sağlanan emin olun.                                                                                                                                                                         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| CustomerSubscriptionNotFound    | _Müşteri abonelik tanımlayıcısı_ _registration adı altında bulunamadı. Geçerli bir Azure aboneliği kullanılıyor ve PUT işlemi kullanarak kaydı için abonelik tanımlayıcısı eklendiğinden emin olun.                                                   | Bu hata, bir kiracı aboneliği eklendi ve kayıt ile ilişkilendirilecek bir müşteri aboneliğinde bulunamadı verity çalışılırken oluşur. Müşteri kaydı için eklenmemiş veya abonelik kimliği yanlış yazılmış.                                                                                                                                                                                                                                                                                                                                |
| UnauthorizedCspRegistration     | Sağlanan _kayıt adı_ çok kiracılı kullanmak için onaylanmadı. Bir e-posta Gönder azstCSP@microsoft.com ve kayıt adınız, kaynak grubu ve kayıt için kullanılan abonelik tanımlayıcısı içerir.                                                                                    | Bir kaydı için çok kiracılı, kiracılar eklemeden başlamadan önce Microsoft tarafından onaylanması gerekir. Daha fazla açıklama için bu belgenin bölüm kiracılar kaydetme bakın.                                                                                                                                                                                                                                                                                                                                                                                                             |
| CustomerSubscriptionsNotAllowed | Bağlantısı kesilmiş müşteriler için müşteri abonelikleri işlemleri desteklenmez. Bu özelliği kullanmak için ödeme olarak kullandığınız Lisans'ı yeniden kaydedin.                                                                                                                                                                    | Kaydın oluşturulduğu BillingModel kapasite kullanılan parametre kiracılar eklemeye çalıştığınız kayıt kapasite kayıt, diğer bir deyişle, olur. Yalnızca kayıtları Kullandıkça Öde için kiracılar eklemek için izin verilir. ' % S'parametresini BillingModel PayAsYouUse kullanarak yeniden kaydetmeniz gerekir.                                                                                                                                                                                                                                                                                          |
| InvalidCSPSubscription          | Sağlanan _müşteri abonelik tanımlayıcısı_ geçerli bir CSP aboneliği değil. Geçerli bir Azure aboneliği sağlanan emin olun.                                                                                                                                                        | Bu çoğunlukla yanlış yazılmış bir müşteri aboneliğinde nedeniyle olabilir.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| MetadataResolverBadGatewayError | Bir Yukarı Akış sunucuları, beklenmeyen bir hata döndürdü. Daha sonra tekrar deneyin. Sorun devam ederse Destek ekibiyle iletişime geçin.                                                                                                                                                                                                |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |

## <a name="terms-used-for-billing-and-usage"></a>Faturalama ve kullanım için kullanılan terimler

Aşağıdaki terimler ve kavramlar kullanımı için kullanılan ve Azure stack'teki faturalandırma şunlardır:

| Sözleşme Dönemi | Tanım |
| --- | --- |
| Doğrudan bir CSP iş ortağı | Doğrudan bir bulut çözümü sağlayıcısı (CSP) iş ortağı fatura doğrudan Azure ve Azure Stack kullanım ve fatura müşterilerin Microsoft'tan doğrudan alır. |
| Dolaylı CSP | Dolaylı satıcıları dolaylı sağlayıcısı (dağıtıcı olarak da bilinir) ile çalışır. Satıcılar, son müşteriler kazanmak; Dolaylı sağlayıcısı olan Microsoft faturalama ilişkiyi tutar, müşteri faturalandırma yönetir ve ürün desteği gibi ek hizmetler sağlar. |
| Son Müşteri | Son müşterilerin uygulamaları ve Azure Stack üzerinde çalıştırılan diğer iş yükleri kendi devlet kurumları ve işletmelerin önerilir. |

## <a name="next-steps"></a>Sonraki adımlar

 - CSP programı hakkında daha fazla bilgi için bkz: [bulut çözümü sağlayıcısı programı](https://partnercenter.microsoft.com/en-us/partner/programs).
 - Azure yığını kaynak kullanım bilgilerini alma hakkında daha fazla bilgi için bkz: [kullanım ve faturalandırma Azure Stack'te](azure-stack-billing-and-chargeback.md).
