---
title: ClearDB MySql veritabanları hakkında SSS Azure App Service | Microsoft Docs
description: ClearDB MySQL veritabanları Azure App Service ile kullanma hakkında sık sorulan soruların yanıtları.
documentationcenter: php
services: ''
author: sunbuild
manager: yochayk
editor: ''
tags: mysql
ms.assetid: c2ed5e78-6d7d-4d0c-b7ee-a52ae41ceab8
ms.service: multiple
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/27/2016
ms.author: sumuth
ms.openlocfilehash: 8186e86bd7a441fcefb0759d75ded6f063a4722f
ms.sourcegitcommit: e19742f674fcce0fd1b732e70679e444c7dfa729
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
ms.locfileid: "28948045"
---
# <a name="faq-for-cleardb-mysql-databases-with-azure-app-service"></a>Azure Uygulama Hizmeti ile ClearDB MySQL veritabanları hakkında SSS
Bu SSS ClearDB MySQL veritabanları Azure Web uygulamaları için satın alma ve kullanma hakkında sık sorulan soruları yanıtlar.

## <a name="what-options-do-i-have-for-mysql-on-azure"></a>Azure üzerinde MySQL için hangi seçenekleri var mı?
Birkaç seçeneğiniz vardır:

* [Paylaşılan ClearDB MySQL veritabanı](/marketplace/partners/cleardb/databases/)
* [ClearDB MySQL Premium kümeleri](/marketplace/partners/cleardb-clusters/cluster/)
* [Bir Azure VM çalıştıran MySQL kümesi](https://github.com/azure/azure-quickstart-templates/tree/master/mysql-replication)
* [Tek bir Azure VM'de çalışan MySQL örneği](virtual-machines/windows/classic/mysql-2008r2.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

ClearDB hizmeti barındırma MySQL olduğu ve MySQL altyapısı tarafından yönetilir. Bir Azure sanal makinesinde kendi MySQL kümesi veya veritabanı çalıştırdığınızda, MySQL sunucu ayarlama ve düzeltme ekleri ile güncel tutun gerekmez.

## <a name="do-i-need-a-credit-card-for-the-web-app--mysql-template-in-the-azure-marketplace"></a>Bir kredi kartı ihtiyacım Web uygulaması + Azure Marketi MySQL şablonunda var?
Bu, kullanmakta olduğunuz abonelik türüne göre değişir. Bazı yaygın olarak kullanılan abonelik türleri şunlardır:

* [Kullandıkça Öde](/offers/ms-azr-0003p/): bir kredi kartı ve kredi kartınızdan ücret Ücretli bir MySQL veritabanı satın aldığınızda gerektirir.
* [Ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/): Microsoft Azure ile kullanmak üzere Hizmetleri ancak üçüncü taraf kaynakları satın izin vermeyen krediler içerir. Üçüncü taraf hizmetleri veya kredi kartı kullanmanıza gerek Ücretli bir MySQL veritabanı satın almak için abonelik etkin. Web uygulamaları için ücretsiz ClearDB MySQL veritabanı oluşturabilirsiniz.
* [MSDN aboneliği](https://azure.microsoft.com/pricing/member-offers/msdn-benefits/) ve **MSDN Geliştirme Test Kullandıkça Öde**: benzer ücretsiz deneme için bir MSDN aboneliği Ücretli bir MySQL çözüm ClearDB satın almak için bir kredi kartı olmasını gerektirir.
* [Kurumsal Anlaşma (Kurumsal Sözleşme)](https://azure.microsoft.com/pricing/enterprise-agreement/): EA müşteriler kendi EA karşı her üç aylık tüm Azure Marketi (üçüncü taraf) aldıklarını ayrı, birleştirilmiş bir fatura için faturalandırılır. Tüm Market satın alımları için parasal taahhüt dışında faturalandırılır. Lütfen, şu anda unutmayın, Azure mağazası Azerbaycan, Hırvatistan, Norveç ve Porto Riko kayıtlı müşteriler için kullanılabilir değildir. 
* [DreamSpark](https://www.dreamspark.com/Product/Product.aspx?productid=99): Web uygulamaları için yalnızca ücretsiz ClearDB veritabanları oluşturabilirsiniz. Ücretsiz ClearDB MySQL veritabanları oluşturabilirsiniz sayısına bir sınır yoktur. Bu hizmet yalnızca deneme için tasarlandığı gibi ücretsiz veritabanlarından üretim web uygulamaları için kullanılmayacak olduğuna dikkat edin.

## <a name="why-was-i-charged-350-for-a-web-app--mysql-from-the-azure-marketplace"></a>3.50 bir Web uygulaması + Azure Marketi'nden MySQL için ücret neden?
Varsayılan veritabanı $3.50 olduğu Titan seçeneğidir. Maliyet veritabanı oluşturma sırasında gösterme ve yanlışlıkla düşündüğünüz kaydetmedi bir veritabanı satın alabilirsiniz. Numaralı deneyimini geliştirmek için bir yol bulmaya çalışıyoruz ancak o zamana kadar tıklatmadan önce tüm, seçilen fiyatlandırma katmanlarına web uygulaması ve veritabanı için denetlemelisiniz **oluşturma** ve kaynakların dağıtımı başlatılıyor.

## <a name="i-am-running-mysql-on-my-own-azure-virtual-machine-can-i-connect-my-azure-web-app-to-my-database"></a>MySQL kendi Azure sanal makinede çalışan. Azure web Uygulamam my veritabanına bağlanabilir miyim?
Evet. Azure VM'nizi uzaktan erişim, web uygulamanızın verdiği sürece web uygulamanızın veritabanınıza bağlanabilir. Daha fazla bilgi için bkz: [bir sanal makinede yüklemek MySQL](virtual-machines/windows/classic/mysql-2008r2.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="in-which-countries-are-cleardb-premium-mysql-clusters-supported"></a>Hangi ülkelerde desteklenen ClearDB Premium MySQL kümeleri misiniz?
[ClearDB Premium MySQL kümeleri](/marketplace/partners/cleardb-clusters/cluster/) tüm Azure bölgeleri Hindistan, Avustralya, Brezilya Güney ve Çin dışında tüm dünyada kullanılabilir.

## <a name="can-i-create-a-new-cluster-prior-to-creating-a-database-with-cleardb-premium-cluster-solution"></a>ClearDB premium küme çözümü ile bir veritabanı oluşturmadan önce yeni bir küme oluşturabilir miyim?
Hayır, boş ClearDB kümeleri oluşturma desteklenmiyor. Azure portalı, aynı anda yeni bir küme oluşturabilir bir kümede veritabanları oluşturmanıza olanak sağlar.

## <a name="will-i-be-warned-if-i-try-to-delete-a-cleardb-mysql-database-that-is-in-use-by-one-of-my-applications"></a>I ı kullanımda olan ClearDB MySQL veritabanını silmek uygulamalarım biri tarafından çalışırsanız uyarılırsınız?
Hayır, Azure, uygulamanızın bağımlı bir Market satın alma silerseniz uyarmaz.

## <a name="which-regions-can-i-create-cleardb-databases-in"></a>Hangi bölgeleri ClearDB veritabanlarında oluşturabilirim?
Azure Market Azerbaycan, Hırvatistan, Norveç veya Porto Riko kayıtlı müşteriler için kullanılabilir değil. ClearDB bu bölgede kullanılamıyor.

## <a name="what-pricing-tier-should-i-choose-for-a-production-web-app-and-database"></a>Bir üretim web uygulaması ve veritabanı için hangi fiyatlandırma katmanı seçmeliyim?
Temel veya daha yüksek bir fiyatlandırma katmanı Web uygulamaları için kullanın. ClearDB için Satürn veya Jüpiter planı öneririz. Özellikler ve her ikisi de her fiyatlandırma katmanının sınırlamalarını gözden [Web Apps](https://azure.microsoft.com/pricing/details/app-service/) ve [ClearDB MySQL veritabanları](/marketplace/partners/cleardb/databases/) gereksinimlerinize uygun olanı seçin.

## <a name="how-do-i-upgrade-my-cleardb-database-from-one-plan-to-another"></a>Nasıl ClearDB veritabanı bir plandan başka bir sürümüne yükseltme?
İçinde [Azure portal](https://portal.azure.com), ClearDB bir paylaşılan barındırma veritabanı ölçeklendirebilirsiniz. Bu okuma [makale](https://blogs.msdn.microsoft.com/appserviceteam/2016/10/06/upgrade-your-cleardb-mysql-database-in-azure-portal/) daha fazla bilgi için. Şu anda Azure portalında ClearDB Premium kümeleri için yükseltme desteklemiyoruz.

## <a name="i-cant-see-my-cleardb-database-in-azure-portal"></a>Azure portalında my ClearDB veritabanı göremiyorum?
Klasik bir ClearDB veritabanı oluşturduysanız, veritabanınızdaki görmeye olmaz [Azure Portal](https://portal.azure.com). Var olan hiçbir iş-geçici bu senaryo için.

## <a name="who-do-i-contact-for-support-when-my-database-is-down"></a>Veritabanı bağlantısı kesilse kimin destek için ile iletişim?
Kişi [ClearDB Destek](https://www.cleardb.com/developers/help/support) herhangi ilgili sorunlar veritabanı için. Azure aboneliği bilgilerinizle sağlamaya hazır olun.

## <a name="can-i-create-additional-users-for-my-cleardb-mysql-database-cluster-solution"></a>ClearDB MySQL veritabanı küme çözümüme için ek kullanıcılar oluşturabilir miyim?
Hayır. Ek kullanıcılar oluşturamaz ancak ClearDB veritabanı kümenizde ek veritabanları oluşturabilirsiniz.  

## <a name="can-basicpro-series-databases-be-upgraded-in-place-similar-to-planetary-plans-today-on-cleardb-portal"></a>Veritabanları Basic/Pro serisi yükseltme yerinde ClearDB portalında bugün Planetary planlarına benzer?
Evet, veritabanları olabilir temel serisi yerinde (temel 500 aracılığıyla temel 60) yükseltilmiştir. Pro serisi olabilir yerinde (Pro 1000 aracılığıyla Pro 125) Pro 60 dışında yükseltildi. Pro 60 veritabanı şu anda yükseltiliyor desteklemez. 

## <a name="when-i-migrate-my-resources-from-one-subscription-to-another-does-my-cleardb-mysql-database-get-migrated-as-well"></a>I Kaynaklarım bir abonelikten diğerine geçirdiğinizde, my ClearDB MySQL veritabanı de geçirilen mu?
Gerçekleştirdiğinizde kaynak geçişi abonelikler arasında bazı [sınırlamalar](azure-resource-manager/resource-group-move-resources.md#app-service-limitations) uygulayın. ClearDB MySQL veritabanı bir üçüncü taraf hizmeti ve bu nedenle Azure abonelik geçişi sırasında geçirilmedi. MySQL veritabanınız geçirme Azure kaynaklarını önce geçişini yönetin değil, ClearDB MySQL veritabanlarınızın devre dışı bırakılabilir. Web uygulamanız için Azure aboneliği geçiş işlemini gerçekleştirmek ve el ile veritabanlarınızı ilk geçirin. 

## <a name="i-hit-the-spending-limit-on-my-subscription-i-removed-the-limit-and-my-app-service-is-online-however-the-database-is-not-accessible-how-do-i-re-enable-the-cleardb-database"></a>I Aboneliğimi üzerinde harcama sınırına ulaştı. Sınır kaldırıldı ve veritabanı erişilebilir değil ancak uygulama Hizmetim çevrimiçidir. ClearDB veritabanı yeniden nasıl etkinleştirebilirim?
Kişi [ClearDB Destek](https://www.cleardb.com/developers/help/support) veritabanını yeniden etkinleştirmek için. Bunları Azure abonelik bilgilerini ve veritabanı adı ile sağlayın.

## <a name="can-i-transfer-a-cleardb-database-from-a-credit-card-subscription-to-an-ea-subscription"></a>EA abonelik için bir ClearDB veritabanı bir kredi kartı abonelikten aktarabilirim?
ClearDB veritabanlarında mevcut abonelikleri ile ilişkili kredi kartından kullanın. Bir EA aboneliği kullanmak için yeni bir veritabanı verilerinizi taşımanız gerekir:

* Yeni bir ClearDB veritabanı EA aboneliğinizle satın alın.
* Verilerinizi yeni veritabanınızı geçirin.
* Uygulamanızı yeni veritabanını kullanacak şekilde güncelleştirin.
* Eski ClearDB veritabanını silin.

MySQL (ClearDB) ile yeni bir web uygulaması oluşturduğunuzda veya bir MySQL veritabanı (ClearDB) oluşturmak, hizmet için nasıl ödeme seçtiğiniz abonelik belirler. Bir EA abonelikle biz ClearDB gibi üçüncü taraf hizmetleri Azure portalında tedarik engellemez. EA abonelikleri dışında parasal taahhüt faturalandırılır ve üç aylık ve arrears faturalandırılır. EA müşteri tüm üçüncü taraf Market hizmetler için ödeme için bir ödeme yöntemi bir kredi kartı gibi ayarlamanız gerekir.

## <a name="where-can-i-see-the-charges-for-cleardb-resources-in-an-ea-subscription"></a>EA abonelik ClearDB kaynaklarında ücretlerinin burada görüyor musunuz?
Doğrudan EA müşteriler için Azure Marketi ücretleri Enterprise Portal'da görülebilir. Tüm Market satın alımları ve tüketim dışında parasal taahhüt faturalandırılır ve üç aylık ve arrears faturalandırılır unutmayın. EA müşteriler doğrudan üçüncü taraf sağlayıcılar için ödeme yapmam gerekir ve bir ödeme yöntemi EA hesabıyla bir kredi kartı gibi etkinleştirerek bunu yapabilirsiniz.

Dolaylı EA müşteriler kendi Azure Marketi abonelikleri üzerinde bulabilir **Aboneliklerini Yönet** Enterprise Portal sayfasının ancak fiyatlandırma gizlenir. Müşteriler, Market ücretleri hakkında bilgi için kendi LSP başvurmalıdır.

Üçüncü taraf hizmetleri için Azure Marketi erişmeye EA Azure kayıt yöneticiler tarafından yönetilebilir. Devre dışı veya erişim 3 taraf Harcamaları hesaplarını yönetme ve abonelikleri deposundan Enterprise Portal hesapları bölümünde yeniden etkinleştirin.

## <a name="who-do-i-contact-for-questions-about-my-bill-for-cleardb-services-in-my-ea-subscription"></a>Kimin ı sorular için faturamı ClearDB hizmetleri hakkında EA Aboneliğimi iletişim?
Kişi [Kurumsal Müşteri Destek](http://aka.ms/AzureEntSupport) EA kayıt altında faturalama göre. EA Portal destek ekibi sorunuza yanıt veya sorununuzu Yardım.

## <a name="more-information"></a>Daha fazla bilgi
[Azure Market SSS](/marketplace/faq/)

