---
title: "Altyapı bulut hizmeti sağlayıcıları için Azure yığını için raporlama kullanım | Microsoft Docs"
description: "Azure yığın oluşur ve Azure'a iletir kullanımını izlemek için gereken altyapıyı içerir."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/08/2018
ms.author: mabrigg
ms.reviewer: alfredo
ms.openlocfilehash: 4ac808e0e85b1daeb54a3f2fd7bec0a7c10aa13e
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
## <a name="usage-reporting-infrastructure-for-cloud-service-providers"></a>Bulut hizmeti sağlayıcılar için altyapı raporlama kullanım

Azure yığın oluşur ve Azure'a iletir kullanımını izlemek için gereken altyapıyı içerir. Azure, Azure ticaret kullanım verilerini işler ve genel Azure bulutta gerçekleşir kullanım aynı şekilde uygun Azure abonelikleri için kullanım ücretlerini.

Belirli kavramları Azure yığını ve genel Azure arasında tutarlı olduğunu bilmeniz gerekir. Azure yığını, Azure aboneliği için benzer bir rolü karşılamak yerel abonelik yok. Yerel abonelikler yalnızca yerel olarak geçerlidir. Kullanım için Azure iletildiğinde yerel abonelikleri Azure aboneliklerine eşlenir.

Yerel kullanım ölçümler Azure yığınına sahiptir. Yerel kullanım Azure ticaret kullanılan ölçümler eşleştirilir. Ancak, ölçüm kimlikleri farklıdır. Daha fazla ölçümler kullanılabilir yerel olarak olandan Microsoft faturalama için kullanır.

Azure yığını ve Azure hizmetleri nasıl fiyatlandırılır arasındaki bazı farklar vardır. Örneğin, Azure yığınında VM'ler için ücret yalnızca vcore/saat, Azure aksine tüm VM seri için aynı oranıyla temel alır. Genel Azure'da farklı donanım farklı fiyatlar yansıtacak nedenidir. Bu yüzden farklı VM sınıfları için farklı hızlarını kaydedilecek herhangi bir neden Azure yığınında donanım, müşteri sağlar.

Ticaret ve iş ortağı Merkezi'nde fiyatları için Azure services gibi aynı şekilde kullanılan Azure yığın ölçümler hakkında öğrenebilirsiniz:

1. İş ortağı Merkezi'nde Git **Pano menü** > **fiyatlandırma ve teklifleri**.
2. Altında **kullanım tabanlı hizmetler**seçin **geçerli**.
3. Açık **genel CSP fiyat listesindeki Azure** elektronik tablo.
4. Üzerindeki filtre **bölge Azure yığın =**.

## <a name="usage-and-billing-error-codes"></a>Kullanım ve fatura hata kodları

Kiracı için bir kayıt ekleme, aşağıdaki hata iletilerinden karşılaşılabilir.

| Hata                           | Ayrıntılar                                                                                                                                                                                                                                                                                                                           | Açıklamalar                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|---------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| RegistrationNotFound            | Sağlanan kayıt bulunamadı. Aşağıdaki bilgiler doğru sağlanan emin olun:<br>1. Abonelik tanımlayıcısı (sağlanan değer: _abonelik tanımlayıcısı_),<br>2. Kaynak grubu (sağlanan değer: _kaynak grubu_),<br>3. Kayıt adı (sağlanan değer: _kayıt adı_).                             | Bu hata genellikle ilk kaydı işaret eden bilgiler doğru değil oluşur. Kaynak grubu ve kaydınızı adını doğrulamanız gerekiyorsa, tüm kaynakları listeleyerek Azure portalında bulabilirsiniz. Birden fazla kayıt kaynağı bulursanız, özelliklerinde CloudDeploymentID bakın ve, CloudDeploymentID bulut sürümüyle eşleşen kayıt seçin. CloudDeploymentID bulmak için bu PowerShell Azure yığında kullanabilirsiniz:<br>`$azureStackStampInfo = Invoke-Command -Session $session -ScriptBlock { Get-AzureStackStampInformation }` |
| BadCustomerSubscriptionId       | Sağlanan _müşteri abonelik tanımlayıcısı_ ve _kayıt adı_ abonelik tanımlayıcısı değil aynı Microsoft bulut hizmeti sağlayıcısı tarafından sahip olunan. Müşteri abonelik tanımlayıcısı doğru olduğunu denetleyin. Sorun devam ederse, desteğe başvurun. | Müşteri aboneliği bir CSP abonelik olsa da, bir CSP ortağına ilk kayıt için kullanılan abonelik için toplanan olandan farklı toplanan bu hata oluşur. Bu denetim için Azure kullanılan yığınına sorumlu olmayan bir CSP ortağı faturalama neden olan bir durum önlemek için yapılır.                                                                                                                                                                                                                                                                          |
| InvalidCustomerSubscriptionId   | '_Müşteri abonelik tanımlayıcısı_' geçerli değil. Geçerli bir Azure aboneliğinizin sağlanan emin olun.                                                                                                                                                                         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| CustomerSubscriptionNotFound    | _Müşteri abonelik tanımlayıcısı_ _registration adı altında bulunamadı. Geçerli bir Azure aboneliği kullanılıyor ve abonelik tanımlayıcısı PUT işlemini kullanarak kayıt eklendiğinden emin olun.                                                   | Bu hata, bir kiracı aboneliği eklendi ve kayıt ile ilişkilendirilecek müşteri aboneliğe bulunamadı raporlar çalışırken oluşur. Müşteri kaydı için eklenmemiş veya abonelik kimliği yanlış yazılmış.                                                                                                                                                                                                                                                                                                                                |
| UnauthorizedCspRegistration     | Sağlanan _kayıt adı_ çoklu kiracı kullanacak şekilde onaylanmadı. E-posta Gönder azstCSP@microsoft.com ve kayıt adı, kaynak grubu ve kayıt için kullanılan abonelik tanımlayıcısı içerir.                                                                                    | Bir kayıt kiracılar eklemeyi başlayabilmeniz için önce çoklu kiracı için Microsoft tarafından onaylanması gerekir. Daha fazla açıklama için bu belgenin bölüm kiracılar kaydetme bakın.                                                                                                                                                                                                                                                                                                                                                                                                             |
| CustomerSubscriptionsNotAllowed | Müşteri abonelikleri işlemleri, bağlantısı kesilen müşteriler için desteklenmez. Bu özelliği kullanabilmek için ödeme olarak kullandığınız Lisans'ı yeniden kaydedin.                                                                                                                                                                    | Kaydın oluşturulduğu BillingModel kapasite kullanılan parametre kiracılar eklemeye çalıştığınız kayıt kapasite kayıt diğer bir deyişle, olur. Yalnızca kayıtlar Kullandıkça Öde kiracılar eklemek için izin verilir. BillingModel PayAsYouUse parametresini kullanarak yeniden kaydetmeniz gerekir.                                                                                                                                                                                                                                                                                          |
| InvalidCSPSubscription          | Sağlanan _müşteri abonelik tanımlayıcısı_ geçerli bir CSP abonelik değil. Geçerli bir Azure aboneliğinizin sağlanan emin olun.                                                                                                                                                        | Bu çoğunlukla yanlış yazmış Müşteri aboneliğini nedeniyle olması olasıdır.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| MetadataResolverBadGatewayError | Yukarı Akış sunucuları beklenmeyen bir hata verdi. Daha sonra tekrar deneyin. Sorun devam ederse, desteğe başvurun.                                                                                                                                                                                                |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |

## <a name="terms-used-for-billing-and-usage"></a>Faturalama ve kullanım için kullanılan terimler

Aşağıdaki terimleri ve kavramları kullanım için kullanılan ve Azure yığınında fatura şunlardır:

| Dönem | Tanım |
| --- | --- |
| Doğrudan CSP iş ortağı | Doğrudan bir bulut çözümü sağlayıcısı (CSP) ortak bir fatura doğrudan Microsoft Azure ve Azure yığın kullanım ve fatura müşteriler için doğrudan alır. |
| Dolaylı CSP | Dolaylı satıcılar dolaylı bir sağlayıcı (dağıtıcı olarak da bilinir) ile çalışır. Satıcılar son müşterilere işe almak; Dolaylı sağlayıcısı Microsoft faturalama ilişkisiyle tutan, müşteri faturalandırma yönetir ve ürün desteği gibi ek hizmetler sağlar. |
| Son müşteriye | Son işletmelerin ve uygulamalar ve Azure yığın üzerinde çalışan diğer iş yüklerini kendi devlet dairesi müşterilerdir. |

## <a name="next-steps"></a>Sonraki adımlar

 - CSP program hakkında daha fazla bilgi için bkz: [bulut çözümü sağlayıcısı programı](https://partnercenter.microsoft.com/en-us/partner/programs).
 - Azure yığınından kaynak kullanım bilgilerini alma hakkında daha fazla bilgi için bkz: [kullanım ve fatura Azure yığınında](/azure-stack-billing-and-chargeback.md).
