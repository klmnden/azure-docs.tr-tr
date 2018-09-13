---
title: Azure IOT paketi ile ilgili SSS | Microsoft Docs
description: IoT Paketi için sık sorulan sorular
services: ''
suite: iot-suite
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: cb537749-a8a1-4e53-b3bf-f1b64a38188a
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: corywink
ms.openlocfilehash: 7e7c4affee64a945900c02b6375ba4df5d085183
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "35757813"
---
# <a name="frequently-asked-questions-for-iot-suite"></a>IoT Paketi için sık sorulan sorular

Ayrıca, bağlı Fabrika belirli bkz [SSS](../iot-accelerators/iot-accelerators-faq-cf.md).

### <a name="where-can-i-find-the-source-code-for-the-preconfigured-solutions"></a>Önceden yapılandırılmış çözümler için kaynak kodu nerede bulabilirim?

Kaynak kodu, aşağıdaki GitHub depolarında depolanır:
* [Önceden yapılandırılmış Uzaktan izleme çözümü][lnk-remote-monitoring-github]
* [Önceden yapılandırılmış Tahmine dayalı bakım çözümü][lnk-predictive-maintenance-github]
* [Önceden yapılandırılmış bağlı Fabrika çözümü](https://github.com/Azure/azure-iot-connected-factory)

### <a name="how-do-i-update-to-the-latest-version-of-the-remote-monitoring-preconfigured-solution-that-uses-the-iot-hub-device-management-features"></a>IOT Hub cihaz yönetimi özellikleri kullanan Uzaktan izleme önceden yapılandırılmış çözümün en son sürüme nasıl güncelleştirebilirim?

* Önceden yapılandırılmış bir çözüm dağıtırsanız https://www.azureiotsuite.com/ site, her zaman en son sürümünü çözüm yeni bir örneğini dağıtır.
* Komut satırını kullanarak önceden yapılandırılmış çözümü dağıttığınızda, yeni kodu ile mevcut bir dağıtımı güncelleştirebilirsiniz. Bkz: [bulut dağıtımı] [ lnk-cloud-deployment] github'da [depo][lnk-remote-monitoring-github].

### <a name="how-can-i-add-support-for-a-new-device-method-to-the-remote-monitoring-preconfigured-solution"></a>Uzaktan izleme önceden yapılandırılmış çözüme yeni bir cihaz yöntemi desteği nasıl ekleyebilirim?

Bölümüne bakın [simülatöre yeni bir yöntem için destek ekleme] [ lnk-add-method] içinde [önceden yapılandırılmış çözümü özelleştirme] [ lnk-customize] makalesi.

### <a name="the-simulated-device-is-ignoring-my-desired-property-changes-why"></a>Sanal cihaz, istenen özellik Değişikliklerimi neden yoksayılıyor?
Uzaktan izleme önceden yapılandırılmış çözümde sanal cihaz kodu yalnızca kullandığı **Desired.Config.TemperatureMeanValue** ve **Desired.config.telemetryınterval** istenen özellikleri için bildirilen özellikleri güncelleştirin. Diğer tüm istenen özellik değişiklik istekleri göz ardı edilir.

### <a name="my-device-does-not-appear-in-the-list-of-devices-in-the-solution-dashboard-why"></a>Çözüm panosundaki cihaz listesinden Cihazınızı görünmüyor neden?

Çözüm panosundaki cihaz listesini, cihazların listesini döndürmek için bir sorgu kullanır. Şu anda, bir sorgu en fazla 10 bin cihazları döndüremez. Sorgunuz için arama ölçütlerini daha kısıtlayıcı içermemesini sağlamayı deneyin.

### <a name="whats-the-difference-between-deleting-a-resource-group-in-the-azure-portal-and-clicking-delete-on-a-preconfigured-solution-in-azureiotsuitecom"></a>Azure portalında bir kaynak grubunu silmek ile azureiotsuite.com'da önceden yapılandırılmış bir çözüm için silmeye tıklama arasındaki fark nedir?

* Önceden yapılandırılmış çözümü silerseniz [azureiotsuite.com][lnk-azureiotsuite], önceden yapılandırılmış çözümü oluşturduğunuzda sağlanan tüm kaynakları silin. Bir kaynak grubuna ek kaynaklar eklediyseniz, bu kaynaklar da silinir. 
* Kaynak grubunu silerseniz [Azure portalında][lnk-azure-portal], yalnızca bu kaynak grubundaki kaynakları silin. Ayrıca önceden yapılandırılmış Çözümle ilişkili Azure Active Directory uygulamasını silmek gerekir [Azure portalında][lnk-azure-portal].

### <a name="how-many-iot-hub-instances-can-i-provision-in-a-subscription"></a>Bir abonelikte kaç IoT Hub örneği sağlayabilirim?

Varsayılan olarak, sağlayabilirsiniz [10 IOT hub'ı abonelik başına][link-azuresublimits]. Oluşturabileceğiniz bir [Azure destek bileti] [ link-azuresupportticket] Bu limiti yükseltmek için. Sonuç olarak, önceden yapılandırılmış her çözüm yeni bir IOT Hub hazırladığından belirli bir abonelikte en fazla 10 önceden yapılandırılmış Çözümler yalnızca sağlayabilirsiniz. 

### <a name="how-many-azure-cosmos-db-instances-can-i-provision-in-a-subscription"></a>Bir abonelikte kaç tane Azure Cosmos DB örneği sağlayabilirim?

Elli. Oluşturabileceğiniz bir [Azure destek bileti] [ link-azuresupportticket] Bu limiti yükseltmek için ancak varsayılan olarak, yalnızca abonelik başına 50 Cosmos DB örnekleri sağlayabilirsiniz. 

### <a name="how-many-free-bing-maps-apis-can-i-provision-in-a-subscription"></a>Bir abonelikte kaç tane Ücretsiz Bing Haritaları API'si sağlayabilirim?

İki. Bir Azure aboneliğinde Enterprise planları için yalnızca iki adet İç İşlemler Düzey 1 Bing Haritası oluşturabilirsiniz. Uzaktan izleme çözümü, varsayılan olarak bir İç İşlemler Düzey 1 planı ile hazırlanır. Sonuç olarak, herhangi bir değişiklik yapılmadıysa bir abonelikte yalnızca en fazla iki tane uzaktan izleme çözümü sağlayabilirsiniz.

### <a name="i-have-a-remote-monitoring-solution-deployment-with-a-static-map-how-do-i-add-an-interactive-bing-map"></a>Statik haritaya sahip bir uzaktan izleme çözümü dağıtımım var; etkileşimli bir Bing haritasını nasıl eklerim?

1. Kurumsal QueryKey için Bing haritaları API'nizi edinin [Azure portalında][lnk-azure-portal]: 
   
   1. Kurumsal için Bing haritaları API'nizi olduğu kaynak grubuna gidin [Azure portalında][lnk-azure-portal].
   2. Tıklayın **tüm ayarlar**, ardından **anahtar yönetimi**. 
   3. İki anahtar görebilirsiniz: **MasterKey** ve **QueryKey**. Değeri kopyalamak **QueryKey**.
      
      > [!NOTE]
      > Kurumsal için Bing Haritaları API'si hesabınız yok mu? Oluşturacağınızı [Azure portalında] [ lnk-azure-portal] göre tıklayarak + yeni, Kurumsal için Bing Haritalar API'si için arama ve oluşturmak için istemleri takip edin.
      > 
      > 
2. Aşağı en son kodu çekin [Azure-IoT-Remote-Monitoring][lnk-remote-monitoring-github].
3. Yerel çalıştırma veya depodaki /docs/ klasöründe komut satırı dağıtım yönergeleri izleyerek dağıtım bulut. 
4. Yerel bir dağıtım veya bir bulut dağıtımını çalıştırdıktan sonra, kök klasörünüzde dağıtım sırasında oluşturulan *.user.config file dosyanızı arayın. Bu dosyayı bir metin düzenleyicisinde açın. 
5. Kopyaladığınız değeri eklemek için şu satırı değiştirin, **QueryKey**: 
   
   `<setting name="MapApiQueryKey" value="" />`

### <a name="can-i-create-a-preconfigured-solution-if-i-have-microsoft-azure-for-dreamspark"></a>DreamSpark için Microsoft Azure'a sahipsem önceden yapılandırılmış bir çözüm oluşturabilir miyim?

> [!NOTE]
> DreamSpark için Microsoft Azure artık Öğrenciler için Microsoft Imagine da bilinir.

Şu anda bir önceden yapılandırılmış çözüm oluşturamazsınız. bir [DreamSpark için Microsoft Azure](https://azure.microsoft.com/pricing/member-offers/imagine/) hesabı. Ancak, oluşturabileceğiniz bir [için Azure ücretsiz deneme hesabınızı](https://azure.microsoft.com/free/) önceden yapılandırılmış bir çözüm sağlayan yalnızca birkaç dakika içinde oluşturun.

### <a name="can-i-create-a-preconfigured-solution-if-i-have-cloud-solution-provider-csp-subscription"></a>Bulut çözümü sağlayıcısı (CSP) abonelik sahipsem önceden yapılandırılmış bir çözüm oluşturabilir miyim?

Şu anda bir bulut çözümü sağlayıcısı (CSP) aboneliği ile önceden yapılandırılmış bir çözüm oluşturamazsınız. Ancak, oluşturabileceğiniz bir [için Azure ücretsiz deneme hesabınızı] [ lnk-30daytrial] önceden yapılandırılmış bir çözüm sağlayan yalnızca birkaç dakika içinde oluşturun.

### <a name="how-do-i-delete-an-azure-ad-tenant"></a>Azure AD kiracısını nasıl silerim?

Eric Golpe'un blog gönderisini inceleyin [bir Azure AD Kiracısını silme Kılavuzu][lnk-delete-aad-tennant].

### <a name="next-steps"></a>Sonraki adımlar

Önceden yapılandırılmış IoT Suite çözümlerinin diğer özelliklerinden bazılarını da keşfedebilirsiniz:

* [Önceden yapılandırılmış Tahmine dayalı bakım çözümüne genel bakış][lnk-predictive-overview]
* [Önceden yapılandırılmış bağlı Fabrika çözümüne genel bakış](../iot-accelerators/iot-accelerators-connected-factory-overview.md)
* [Baştan sona IoT güvenliği][lnk-security-groundup]

[lnk-predictive-overview]:../iot-accelerators/iot-accelerators-predictive-overview.md
[lnk-security-groundup]:/azure/iot-fundamentals/iot-security-ground-up

[link-azuresupportticket]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade 
[link-azuresublimits]: https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/#iot-hub-limits
[lnk-azure-portal]: https://portal.azure.com
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-remote-monitoring-github]: https://github.com/Azure/azure-iot-remote-monitoring 
[lnk-dreamspark]: https://www.dreamspark.com/Product/Product.aspx?productid=99 
[lnk-30daytrial]: https://azure.microsoft.com/free/
[lnk-delete-aad-tennant]: http://blogs.msdn.com/b/ericgolpe/archive/2015/04/30/walkthrough-of-deleting-an-azure-ad-tenant.aspx
[lnk-cloud-deployment]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/cloud-deployment.md
[lnk-add-method]: iot-suite-v1-guidance-on-customizing-preconfigured-solutions.md#add-support-for-a-new-method-to-the-simulator
[lnk-customize]: iot-suite-v1-guidance-on-customizing-preconfigured-solutions.md
[lnk-remote-monitoring-github]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-predictive-maintenance-github]: https://github.com/Azure/azure-iot-predictive-maintenance
