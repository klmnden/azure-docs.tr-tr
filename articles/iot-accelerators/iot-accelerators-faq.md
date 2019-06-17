---
title: IOT Çözüm Hızlandırıcıları SSS - Azure | Microsoft Docs
description: IOT Çözüm Hızlandırıcıları için sık sorulan sorular
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 02/15/2018
ms.author: dobett
ms.openlocfilehash: b2f08e811217572e09a254e9ab3306ab954b14b5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61447967"
---
# <a name="frequently-asked-questions-for-iot-solution-accelerators"></a>IOT Çözüm Hızlandırıcıları için sık sorulan sorular

Ayrıca bkz [bağlı Fabrika özgü SSS](iot-accelerators-faq-cf.md) ve [Uzaktan izleme özgü SSS](iot-accelerators-faq-rm-v2.md) .

### <a name="where-can-i-find-the-source-code-for-the-solution-accelerators"></a>Kaynak kodu için Çözüm Hızlandırıcıları nerede bulabilirim?

Kaynak kodu, aşağıdaki GitHub depolarında depolanır:

* [Uzaktan izleme çözüm Hızlandırıcısını (.NET)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet)
* [Uzaktan izleme çözüm Hızlandırıcısını (Java)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java)
* [Tahmine dayalı bakım Çözüm Hızlandırıcısı](https://github.com/Azure/azure-iot-predictive-maintenance)
* [Bağlı Fabrika Çözüm Hızlandırıcısı](https://github.com/Azure/azure-iot-connected-factory)

### <a name="what-sdks-can-i-use-to-develop-device-clients-for-the-solution-accelerators"></a>Cihaz istemcileri için Çözüm Hızlandırıcıları geliştirmek için hangi SDK'ları kullanabilir miyim?

Farklı dil (C, .NET, Java, Node.js, Python) IOT cihaz SDK'ları bağlantılarını bulabilirsiniz, [Microsoft Azure IOT SDK'ları](https://github.com/Azure/azure-iot-sdks) GitHub depoları.

DevKit cihaz kullanıyorsanız, kaynakları ve örnekleri bulabilirsiniz [IOT DevKit SDK'sı](https://github.com/Microsoft/devkit-sdk) GitHub deposu.

### <a name="is-the-new-microservices-architecture-available-for-all-the-three-solution-accelerators"></a>Yeni bir mikro hizmet mimarisi, tüm üç Çözüm Hızlandırıcıları için kullanılabilir mi?

Şu anda, en geniş kapsamlı senaryosuna değiniyor yalnızca Uzaktan izleme çözümü mikro hizmet mimarisi kullanır.

### <a name="what-advantages-does-the-new-open-sourced-microservices-based-architecture-provide-in-the-new-update"></a>Yeni güncelleştirme yeni açık kaynaklı mikro hizmet tabanlı mimariye sağladığı avantajları sağlar?

Son iki yıl içinde bulut mimarisi büyük ölçüde geliştirilmiştir. Mikro hizmetler geliştirme hızdan ödün vermeden ölçek ve esneklik elde etmek için harika bir düzen olarak ortaya çıkmıştır. Bu mimari deseni harika güvenilirlik ve ölçeklenebilirlik sonuçları ile çeşitli Microsoft hizmetleriyle dahili olarak kullanılır. Böylece müşteriler, bunları avantaj elde Microsoft Çözüm Hızlandırıcısı uygulama içinde bu dersleri koyuyor.

### <a name="im-a-service-administrator-and-id-like-to-change-the-directory-mapping-between-my-subscription-and-a-specific-azure-ad-tenant-how-do-i-complete-this-task"></a>Ben bir Hizmet Yöneticisi ve Aboneliğimi ve belirli bir arasında dizin eşlemesini değiştirmek istediğiniz Azure AD kiracısı. Bu görevi nasıl tamamlamak?

Bkz: [Azure AD dizininize mevcut bir aboneliğe eklemek için](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md#to-associate-an-existing-subscription-to-your-azure-ad-directory)

### <a name="i-want-to-change-a-service-administrator-or-co-administrator-when-logged-in-with-an-organizational-account"></a>Bir Hizmet Yöneticisi veya bir kuruluş hesabıyla oturum açıldığında ortak yöneticiyi değiştirmek istiyorum

Destek makalesine bakın [Hizmet Yöneticisi değiştiriliyor ve bir kuruluş hesabıyla oturum açıldığında ortak yönetici](https://azure.microsoft.com/support/changing-service-admin-and-co-admin).

### <a name="why-am-i-seeing-this-error-your-account-does-not-have-the-proper-permissions-to-create-a-solution-please-check-with-your-account-administrator-or-try-with-a-different-account"></a>Bu hatayı neden görüyorum? "Hesabınız bir çözüm oluşturmak için uygun izinlere sahip değil. Lütfen hesap yöneticinize başvurun veya farklı bir hesap ile deneyin."

Yönergeler için aşağıdaki çizime bakın:

![İzinleri akış çizelgesi](media/iot-accelerators-faq/flowchart.png)

> [!NOTE]
> Doğruladıktan sonra hatayı görmeye devam ederseniz Azure AD kiracısının genel Yöneticisi ve aboneliğin ortak yöneticisi olan, hesap yöneticinizden kullanıcıyı kaldırmasını ve gerekli izinleri şu sırayla yeniden atama vardır. İlk olarak, kullanıcının genel yönetici olarak ekleyin ve ardından kullanıcı Azure aboneliğinin ortak yönetici olarak ekleyin. Sorunlar devam ederse, başvurun [Yardım ve Destek](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

### <a name="why-am-i-seeing-this-error-when-i-have-an-azure-subscription-an-azure-subscription-is-required-to-create-pre-configured-solutions-you-can-create-a-free-trial-account-in-just-a-couple-of-minutes"></a>Bir Azure aboneliğim varken bu hatayı neden görüyorum? "Azure aboneliğinin önceden yapılandırılmış çözümler oluşturmak için gereklidir. Ücretsiz bir deneme hesabı yalnızca birkaç dakika içinde oluşturabilirsiniz."

Bir Azure aboneliğiniz varsa, Kiracı, aboneliğiniz için eşlemesini doğrulayın ve açılır menüden doğru kiracının seçili olup olmadığını denetleyin. Kiracının doğru olduğunu onayladıysanız yukarıdaki diyagramı izleyin ve aboneliğinizi ve bu Azure AD Kiracı eşlemesini doğrulayın.

### <a name="where-can-i-find-information-about-the-previous-version-of-the-remote-monitoring-solution"></a>Uzaktan izleme çözümünün önceki sürümü hakkında bilgileri nerede bulabilirim?

Uzaktan izleme çözüm Hızlandırıcısını önceki sürümünü IOT paketi uzaktan önceden yapılandırılmış izleme çözümü olarak bilinir. Arşivlenen belgeleri bulabilirsiniz [ https://docs.microsoft.com/previous-versions/azure/iot-suite/ ](https://docs.microsoft.com/previous-versions/azure/iot-suite/).

### <a name="is-the-new-solution-accelerator-available-in-the-same-geographic-region-as-the-existing-solution"></a>Yeni çözüm Hızlandırıcısını varolan bir çözümü ile aynı coğrafi bölgede kullanılabilir mi?

Evet, yeni Uzaktan izleme aynı coğrafi bölgelerde kullanılabilir.

### <a name="whats-the-difference-between-deleting-a-resource-group-in-the-azure-portal-and-clicking-delete-on-a-solution-accelerator-in-azureiotsolutionscom"></a>Azure portalında bir kaynak grubu silme ve bir çözüm Hızlandırıcı azureiotsolutions.com'silme ' ı tıklatarak arasındaki fark nedir?

* Çözüm Hızlandırıcısını içinde silerseniz [azureiotsolutions.com](https://www.azureiotsolutions.com/), çözüm Hızlandırıcısını oluşturduğunuzda dağıtılan tüm kaynakları silin. Bir kaynak grubuna ek kaynaklar eklediyseniz, bu kaynaklar da silinir.
* Kaynak grubunu silerseniz [Azure portalında](https://portal.azure.com), yalnızca bu kaynak grubundaki kaynakları silin. Ayrıca çözüm Hızlandırıcı ile ilişkili Azure Active Directory uygulamasını silmek gerekir.

### <a name="can-i-continue-to-leverage-my-existing-investments-in-azure-iot-solution-accelerators"></a>Azure IOT Çözüm Hızlandırıcıları olarak my mevcut yatırımlardan yararlanma devam edebilir?

Evet. Bugün var olan herhangi bir çözümü Azure aboneliğinizde çalışmaya devam eder ve kaynak kodu Github'da kullanılabilir kalır.

### <a name="how-many-iot-hub-instances-can-i-provision-in-a-subscription"></a>Bir abonelikte kaç IoT Hub örneği sağlayabilirim?

Varsayılan olarak, sağlayabilirsiniz [10 IOT hub'ı abonelik başına](../azure-subscription-service-limits.md#iot-hub-limits). Oluşturabileceğiniz bir [Azure destek bileti](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) Bu limiti yükseltmek için. Sonuç olarak, her çözüm Hızlandırıcı hükümlerinin bu yana yeni bir IOT Hub, yalnızca belirli bir abonelikte en fazla 10 Çözüm Hızlandırıcıları sağlayabilirsiniz.

### <a name="how-many-azure-cosmos-db-instances-can-i-provision-in-a-subscription"></a>Bir abonelikte kaç tane Azure Cosmos DB örneği sağlayabilirim?

Elli. Oluşturabileceğiniz bir [Azure destek bileti](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) Bu limiti yükseltmek için ancak varsayılan olarak, yalnızca abonelik başına 50 Cosmos DB örnekleri sağlayabilirsiniz.

### <a name="how-many-free-bing-maps-apis-can-i-provision-in-a-subscription"></a>Bir abonelikte kaç tane Ücretsiz Bing Haritaları API'si sağlayabilirim?

İki. Bir Azure aboneliğinde Enterprise planları için yalnızca iki adet İç İşlemler Düzey 1 Bing Haritası oluşturabilirsiniz. Uzaktan izleme çözümü, varsayılan iç işlemler düzey 1 planı ile hazırlanır. Sonuç olarak, yalnızca en fazla iki uzaktan izleme çözümü bir Abonelikteki herhangi bir değişiklik yapılmadıysa sağlayabilirsiniz.

### <a name="can-i-create-a-solution-accelerator-if-i-have-microsoft-azure-for-dreamspark"></a>Çözüm Hızlandırıcısı, Microsoft Azure için DreamSpark varsa oluşturabilir miyim?

> [!NOTE]
> DreamSpark için Microsoft Azure artık Öğrenciler için Microsoft Imagine da bilinir.

Şu anda bir çözüm Hızlandırıcı ile oluşturamazsınız. bir [DreamSpark için Microsoft Azure](https://azure.microsoft.com/pricing/member-offers/imagine/) hesabı. Ancak, oluşturabileceğiniz bir [ücretsiz deneme hesabı için Azure](https://azure.microsoft.com/free/) çözüm Hızlandırıcısını sağlayan yalnızca birkaç dakika içinde oluşturun.

### <a name="how-do-i-delete-an-azure-ad-tenant"></a>Azure AD kiracısını nasıl silerim?

Eric Golpe'un blog gönderisini inceleyin [bir Azure AD Kiracısını silme Kılavuzu](https://blogs.msdn.com/b/ericgolpe/archive/2015/04/30/walkthrough-of-deleting-an-azure-ad-tenant.aspx).

### <a name="next-steps"></a>Sonraki adımlar

IoT çözüm hızlandırıcılarının diğer özellik ve yeteneklerinden bazılarını da keşfedebilirsiniz:

* [Uzaktan izleme çözüm Hızlandırıcısını özelliklerini keşfedin](quickstart-remote-monitoring-deploy.md)
* [Tahmine Dayalı Bakım çözüm hızlandırıcısına genel bakış](iot-accelerators-predictive-overview.md)
* [Bağlı Fabrika çözüm Hızlandırıcısını dağıtma](quickstart-connected-factory-deploy.md)
* [Baştan sona IOT güvenliği](/azure/iot-fundamentals/iot-security-ground-up)
