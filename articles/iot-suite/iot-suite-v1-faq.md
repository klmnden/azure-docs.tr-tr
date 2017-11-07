---
title: Azure IOT paketi ile ilgili SSS | Microsoft Docs
description: "IoT Paketi için sık sorulan sorular"
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: cb537749-a8a1-4e53-b3bf-f1b64a38188a
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: corywink
ms.openlocfilehash: 5d795a8aaaa8db5b4b5705b0c6ffd303ea1985c0
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2017
---
# <a name="frequently-asked-questions-for-iot-suite"></a>IoT Paketi için sık sorulan sorular

Ayrıca bkz.: bağlı Fabrika özel [SSS](iot-suite-faq-cf.md).

### <a name="where-can-i-find-the-source-code-for-the-preconfigured-solutions"></a>Önceden yapılandırılmış çözümleri için kaynak kodunu nereden bulabilirim?

Kaynak kodu aşağıdaki GitHub depolarının depolanır:
* [Önceden yapılandırılmış Uzaktan izleme çözümü][lnk-remote-monitoring-github]
* [Önceden yapılandırılmış Tahmine dayalı bakım çözümü][lnk-predictive-maintenance-github]
* [Bağlı Fabrika önceden yapılandırılmış çözümü](https://github.com/Azure/azure-iot-connected-factory)

### <a name="how-do-i-update-to-the-latest-version-of-the-remote-monitoring-preconfigured-solution-that-uses-the-iot-hub-device-management-features"></a>IOT Hub cihaz yönetimi özellikleri kullanan Uzaktan izleme önceden yapılandırılmış çözümü en son sürümüne nasıl güncelleştiririm?

* Önceden yapılandırılmış bir çözüm https://www.azureiotsuite.com/ sitesinden dağıtırsanız, her zaman çözüm en son sürümünü yeni bir örneğini dağıtır.
* Komut satırını kullanarak önceden yapılandırılmış bir çözüm dağıtırsanız, yeni kodu ile var olan bir dağıtıma güncelleştirebilirsiniz. Bkz: [bulut dağıtımı] [ lnk-cloud-deployment] github'da [deposu][lnk-remote-monitoring-github].

### <a name="how-can-i-add-support-for-a-new-device-method-to-the-remote-monitoring-preconfigured-solution"></a>Uzaktan izleme önceden yapılandırılmış çözümü için yeni bir cihaz yöntemi desteğini nasıl ekleyebilirim?

Bölümüne bakın [simulator için yeni bir yöntem için destek eklemek] [ lnk-add-method] içinde [önceden yapılandırılmış çözümü özelleştirme] [ lnk-customize] makalesi.

### <a name="the-simulated-device-is-ignoring-my-desired-property-changes-why"></a>Sanal cihaz my istenen özellik değişiklikleri neden yoksayılıyor?
Uzaktan izleme önceden yapılandırılmış çözümde sanal cihaz kod yalnızca **Desired.Config.TemperatureMeanValue** ve **Desired.Config.TelemetryInterval** bildirilen özellikleri güncelleştirmek için özellikler istenen. Diğer tüm istenen özelliği değişiklik isteklerini göz ardı edilir.

### <a name="my-device-does-not-appear-in-the-list-of-devices-in-the-solution-dashboard-why"></a>Cihazımı çözüm panosunda aygıtların listesi görünmez neden?

Çözüm panosunda aygıtların listesi, cihaz listesini döndürmek için bir sorgu kullanır. Şu anda bir sorgu en fazla 10 K aygıtları döndüremez. Arama ölçütlerini sorgunuza daha kısıtlayıcı yapmayı deneyin.

### <a name="whats-the-difference-between-deleting-a-resource-group-in-the-azure-portal-and-clicking-delete-on-a-preconfigured-solution-in-azureiotsuitecom"></a>Azure portalında bir kaynak grubunu silmek ile azureiotsuite.com'da önceden yapılandırılmış bir çözüm için silmeye tıklama arasındaki fark nedir?

* Önceden yapılandırılmış çözümü silerseniz [azureiotsuite.com][lnk-azureiotsuite], önceden yapılandırılmış çözümü oluşturduğunuzda sağlanan tüm kaynakları silin. Kaynak grubuna ek kaynaklar eklediyseniz, bu kaynakları da silinir. 
* Kaynak grubunu silerseniz [Azure portal][lnk-azure-portal], yalnızca bu kaynak grubundaki kaynakları silersiniz. Ayrıca önceden yapılandırılmış Çözümle ilişkili Azure Active Directory Uygulama silmenize gerek [Klasik Azure portalı][lnk-classic-portal].

### <a name="how-many-iot-hub-instances-can-i-provision-in-a-subscription"></a>Bir abonelikte kaç IoT Hub örneği sağlayabilirim?

Varsayılan olarak, sağlayabilirsiniz [10 IOT hub'ları abonelik başına][link-azuresublimits]. Oluşturabileceğiniz bir [Azure destek bileti] [ link-azuresupportticket] bu sınırı artırmak için. Sonuç olarak, önceden yapılandırılmış her çözüm yeni bir IOT hub'ı hazırlar olduğundan, yalnızca belirli bir aboneliğe en fazla 10 önceden yapılandırılmış çözümü sağlayabilirsiniz. 

### <a name="how-many-azure-cosmos-db-instances-can-i-provision-in-a-subscription"></a>Bir abonelikte kaç Azure Cosmos DB örneği sağlayabilirim?

Elli. Oluşturabileceğiniz bir [Azure destek bileti] [ link-azuresupportticket] bu sınırı artırmak için ancak varsayılan olarak, abonelik başına 50 Cosmos DB örnekleri yalnızca sağlayabilirsiniz. 

### <a name="how-many-free-bing-maps-apis-can-i-provision-in-a-subscription"></a>Bir abonelikte kaç tane Ücretsiz Bing Haritaları API'si sağlayabilirim?

İki. Bir Azure aboneliğinde Enterprise planları için yalnızca iki adet İç İşlemler Düzey 1 Bing Haritası oluşturabilirsiniz. Uzaktan izleme çözümü, varsayılan olarak bir İç İşlemler Düzey 1 planı ile hazırlanır. Sonuç olarak, herhangi bir değişiklik yapılmadıysa bir abonelikte yalnızca en fazla iki tane uzaktan izleme çözümü sağlayabilirsiniz.

### <a name="i-have-a-remote-monitoring-solution-deployment-with-a-static-map-how-do-i-add-an-interactive-bing-map"></a>Statik haritaya sahip bir uzaktan izleme çözümü dağıtımım var; etkileşimli bir Bing haritasını nasıl eklerim?

1. Kurumsal QueryKey için Bing haritaları API'nizi almak [Azure portal][lnk-azure-portal]: 
   
   1. Kurumsal için Bing haritaları API'nizi olduğu kaynak grubuna gidin [Azure portal][lnk-azure-portal].
   2. Tıklatın **tüm ayarları**, ardından **anahtar yönetimi**. 
   3. İki anahtar görebilirsiniz: **MasterKey** ve **QueryKey**. Değeri kopyalama **QueryKey**.
      
      > [!NOTE]
      > Kurumsal için Bing Haritaları API'si hesabınız yok mu? Oluşturun [Azure portal] [ lnk-azure-portal] göre tıklayarak + yeni, Kurumsal için Bing haritaları API'si için arama ve oluşturmak için istemleri izleyin.
      > 
      > 
2. En son koddan aşağı çekmek [Azure-IoT-Remote-Monitoring][lnk-remote-monitoring-github].
3. Depodaki /docs/ klasöründe komut satırı dağıtım rehberini izleyerek dağıtım Bulut veya yerel çalıştırın. 
4. Yerel bir dağıtım veya bir bulut dağıtımını çalıştırdıktan sonra, kök klasörünüzde dağıtım sırasında oluşturulan *.user.config file dosyanızı arayın. Bu dosyayı bir metin düzenleyicisinde açın. 
5. Kopyaladığınız değeri eklemek için aşağıdaki satırı değiştirin, **QueryKey**: 
   
   `<setting name="MapApiQueryKey" value="" />`

### <a name="can-i-create-a-preconfigured-solution-if-i-have-microsoft-azure-for-dreamspark"></a>DreamSpark için Microsoft Azure'a sahipsem önceden yapılandırılmış bir çözüm oluşturabilir miyim?

Şu anda önceden yapılandırılmış bir çözüm oluşturamazsınız bir [DreamSpark için Microsoft Azure] [ lnk-dreamspark] hesabı. Ancak, oluşturabileceğiniz bir [ücretsiz deneme hesabı için Azure] [ lnk-30daytrial] sağlayan yalnızca birkaç dakika içinde önceden yapılandırılmış bir çözüm oluşturun.

### <a name="can-i-create-a-preconfigured-solution-if-i-have-cloud-solution-provider-csp-subscription"></a>Önceden yapılandırılmış bir çözüm, bulut çözümü sağlayıcısı (CSP) aboneliğiniz varsa oluşturabilir miyim?

Şu anda bir bulut çözümü sağlayıcısı (CSP) aboneliğiyle önceden yapılandırılmış bir çözüm oluşturamazsınız. Ancak, oluşturabileceğiniz bir [ücretsiz deneme hesabı için Azure] [ lnk-30daytrial] sağlayan yalnızca birkaç dakika içinde önceden yapılandırılmış bir çözüm oluşturun.

### <a name="how-do-i-delete-an-aad-tenant"></a>Bir AAD kiracısını nasıl silerim?

Bkz. Eric Golpe'un blog gönderisi [bir Azure AD Kiracısını silme Kılavuzu][lnk-delete-aad-tennant].

### <a name="next-steps"></a>Sonraki adımlar

Önceden yapılandırılmış IoT Suite çözümlerinin diğer özelliklerinden bazılarını da keşfedebilirsiniz:

* [Önceden yapılandırılmış Tahmine dayalı bakım çözümüne genel bakış][lnk-predictive-overview]
* [Önceden yapılandırılmış bağlı Fabrika çözümüne genel bakış](iot-suite-connected-factory-overview.md)
* [Baştan sona IoT güvenliği][lnk-security-groundup]

[lnk-predictive-overview]: iot-suite-predictive-overview.md
[lnk-security-groundup]: securing-iot-ground-up.md

[link-azuresupportticket]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade 
[link-azuresublimits]: https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/#iot-hub-limits
[lnk-azure-portal]: https://portal.azure.com
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-remote-monitoring-github]: https://github.com/Azure/azure-iot-remote-monitoring 
[lnk-dreamspark]: https://www.dreamspark.com/Product/Product.aspx?productid=99 
[lnk-30daytrial]: https://azure.microsoft.com/free/
[lnk-delete-aad-tennant]: http://blogs.msdn.com/b/ericgolpe/archive/2015/04/30/walkthrough-of-deleting-an-azure-ad-tenant.aspx
[lnk-cloud-deployment]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/cloud-deployment.md
[lnk-add-method]: iot-suite-v1-guidance-on-customizing-preconfigured-solutions.md#add-support-for-a-new-method-to-the-simulator
[lnk-customize]: iot-suite-v1-guidance-on-customizing-preconfigured-solutions.md
[lnk-remote-monitoring-github]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-predictive-maintenance-github]: https://github.com/Azure/azure-iot-predictive-maintenance
