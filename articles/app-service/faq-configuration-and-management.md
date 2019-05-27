---
title: Yapılandırma SSS - Azure uygulama hizmeti | Microsoft Docs
description: Azure App Service'in Web Apps özelliği için yapılandırma ve yönetim sorunları hakkında sık sorulan soruların yanıtlarını alın.
services: app-service\web
documentationcenter: ''
author: genlin
manager: cshepard
editor: ''
tags: top-support-issue
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/30/2018
ms.author: genli
ms.openlocfilehash: 88051c45f21bdf11807ffcc63d8248cba81ae70b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66137065"
---
# <a name="configuration-and-management-faqs-for-web-apps-in-azure"></a>Azure'daki Web uygulamaları için yapılandırma ve yönetim hakkında SSS

Bu makale için yapılandırma ve yönetim sorunları hakkında sık sorulan sorular (SSS) yanıtlarını sahip [Azure App Service'in Web Apps özelliği](https://azure.microsoft.com/services/app-service/web/).

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="are-there-limitations-i-should-be-aware-of-if-i-want-to-move-app-service-resources"></a>App Service kaynaklarını taşımak isterseniz dikkat etmem sınırlaması var mı?

App Service kaynakları yeni kaynak grubuna veya aboneliğe taşımayı planlıyorsanız, dikkat edilmesi gereken bazı sınırlamalar vardır. Daha fazla bilgi için [App Service sınırlamalarını](../azure-resource-manager/resource-group-move-resources.md#app-service-limitations).

## <a name="how-do-i-use-a-custom-domain-name-for-my-web-app"></a>Özel etki alanı için web uygulamamı nasıl kullanabilirim?

Özel etki alanı adını Azure web uygulamanız ile kullanma hakkında sık sorulan soruların yanıtlarını görmek için yedi dakikalık videomuza [bir özel etki alanı adı ekleme](https://channel9.msdn.com/blogs/Azure-App-Service-Self-Help/Add-a-Custom-Domain-Name). Video, bir özel etki alanı adı ekleme hakkında kılavuz sunar. Bunun yerine kendi URL kullanmayı açıklar *. App Service web uygulamanızı azurewebsites.net URL'SİYLE. Ayrıntılı bilgileri de görebilirsiniz [özel etki alanı adı eşlemeyle ilgili bilgi](app-service-web-tutorial-custom-domain.md).


## <a name="how-do-i-purchase-a-new-custom-domain-for-my-web-app"></a>Web uygulamamı nasıl yeni bir özel etki alanı satın?

Satın alma ve App Service web uygulamanıza özel bir etki alanı ayarlama hakkında bilgi edinmek için [satın alma ve App Service'te özel etki alanı adı yapılandırma](manage-custom-dns-buy-domain.md).


## <a name="how-do-i-upload-and-configure-an-existing-ssl-certificate-for-my-web-app"></a>Nasıl karşıya yükleme ve web Uygulamam için mevcut bir SSL sertifikasını yapılandırma?

Karşıya yükleyin ve var olan bir özel SSL sertifika ayarlama hakkında bilgi edinmek için [mevcut bir özel SSL sertifikasını bir Azure web uygulamasına bağlama](app-service-web-tutorial-custom-ssl.md#upload).


## <a name="how-do-i-purchase-and-configure-a-new-ssl-certificate-in-azure-for-my-web-app"></a>Nasıl satın alın ve yeni bir SSL sertifikası Azure'da web uygulamamı yapılandırma?

Satın alma ve App Service web uygulamanız için SSL sertifikası ayarlama hakkında bilgi edinmek için [App Service uygulamanız için bir SSL sertifikası ekleme](web-sites-purchase-ssl-web-site.md).


## <a name="how-do-i-move-application-insights-resources"></a>Application Insights kaynaklarını nasıl taşırım?

Azure Application Insights, taşıma işlemi şu anda desteklemiyor. Özgün kaynak grubunuz bir Application Insights kaynağı içeriyorsa, bu kaynak taşınamıyor. Bir App Service uygulaması taşımaya çalıştığınızda Application Insights kaynak içeriyorsa, tüm işlemi başarısız taşıyın. Ancak, Application Insights ve App Service planı düzgün çalışması için aynı kaynak grubunda uygulama için uygulama olması gerekmez.

Daha fazla bilgi için [App Service sınırlamalarını](../azure-resource-manager/resource-group-move-resources.md#app-service-limitations).

## <a name="where-can-i-find-a-guidance-checklist-and-learn-more-about-resource-move-operations"></a>Burada kılavuz denetim bulmak ve miyim öğrenin kaynak hakkında daha fazla taşıma işlemlerini?

[App Service sınırlamalarını](../azure-resource-manager/resource-group-move-resources.md#app-service-limitations) kaynakları aynı abonelikte yeni bir kaynak grubu veya yeni bir aboneliğe taşıma işlemi gösterilmektedir. Kaynak taşıma denetim listesi hakkında bilgi edinin, hangi hizmetlerin taşıma işlemi destekler ve App Service kısıtlamaları ve diğer konular hakkında daha fazla bilgi edinin.

## <a name="how-do-i-set-the-server-time-zone-for-my-web-app"></a>Sunucu saat dilimi web uygulamamı nasıl ayarlayabilirim?

Web uygulamanız için sunucusunun saat dilimini ayarlamak için:

1. Azure portalında, App Service aboneliğinizdeki Git **uygulama ayarları** menüsü.
2. Altında **uygulama ayarları**, bu ayarı ekleyin:
    * Anahtar WEBSITE_TIME_ZONE =
    * Değer = *istediğiniz saat dilimi*
3. **Kaydet**’i seçin.

Bkz **saat dilimi** sütununda [varsayılan saat dilimlerini](https://docs.microsoft.com/windows-hardware/manufacture/desktop/default-time-zones) makale için kabul edilen değerler.

## <a name="why-do-my-continuous-webjobs-sometimes-fail"></a>Neden benim sürekli WebJobs bazen hata veriyor?

Belirli bir süre için boşta olmaları durumunda varsayılan olarak, web uygulamaları kaldırılır. Bu kaynak tasarrufu yapmak sistemi sağlar. Temel ve standart planlarında etkinleştirebilirsiniz **Always On** her zaman yüklü web uygulamasını tutmak ayarlama. Web uygulamanız sürekli WebJobs çalışıyorsa açmanız **Always On**, veya WebJobs güvenilir bir şekilde çalışmayabilir. Daha fazla bilgi için [sürekli olarak çalışan bir WebJob oluşturmak](webjobs-create.md#CreateContinuous).

## <a name="how-do-i-get-the-outbound-ip-address-for-my-web-app"></a>Giden IP adresi için web uygulamamı nasıl alabilirim?

Web uygulamanız için giden IP adreslerinin listesini almak için:

1. Azure portalında, web uygulaması dikey penceresine gidin **özellikleri** menüsü.
2. Arama **giden IP adresleri**.

Giden IP adresleri listesi görüntülenir.

Bir App Service ortamında Web sitenizi barındırılıyorsa giden IP adresini alma hakkında bilgi için bkz: [giden ağ adresleri](environment/app-service-app-service-environment-network-architecture-overview.md#outbound-network-addresses).

## <a name="how-do-i-get-a-reserved-or-dedicated-inbound-ip-address-for-my-web-app"></a>Web Uygulamam için nasıl bir ayrılmış veya özel gelen IP adresi alabilirim?

Azure uygulaması Web sitesine yapılan gelen çağrılar için ayrılmış veya ayrılmış bir IP adresi ayarlamak için yükleme ve IP tabanlı SSL sertifikası yapılandırma.

Gelen çağrılar için ayrılmış veya ayrılmış bir IP adresi kullanmak için App Service planınız temel veya daha yüksek hizmet planında olması gerektiğini unutmayın.

## <a name="can-i-export-my-app-service-certificate-to-use-outside-azure-such-as-for-a-website-hosted-elsewhere"></a>Azure gibi başka bir yerde barındırılan bir Web sitesi için kullanılacak my App Service sertifikası dışarı aktarabilir miyim? 

App Service sertifikaları, Azure kaynaklarını olarak kabul edilir. Azure hizmetlerinizi dışında kullanmak için amaçlanmamıştır. Azure'un dışında kullanmak için bunları dışarı aktaramazsınız. Daha fazla bilgi için [App Service sertifikaları ve özel etki alanları hakkında SSS](https://social.msdn.microsoft.com/Forums/azure/f3e6faeb-5ed4-435a-adaa-987d5db43b80/faq-on-app-service-certificates-and-custom-domains?forum=windowsazurewebsitespreview).

## <a name="can-i-export-my-app-service-certificate-to-use-with-other-azure-cloud-services"></a>Diğer Azure bulut Hizmetleri ile kullanmak için my App Service sertifikası dışarı aktarabilir miyim?

Portal, bir App Service sertifikasını Azure Key Vault aracılığıyla uygulama hizmeti uygulamalarına dağıtmak için birinci sınıf bir deneyim sunar. Ancak biz istekleri bu sertifikalar, App Service platformu dışında Azure sanal makineler ile kullanmak için müşterilerin alıyor olabilir. Diğer Azure kaynakları ile bir sertifika kullanabilmeniz için App Service sertifikasının yerel PFX kopyasını oluşturmayı öğrenmek için bkz: [bir App Service sertifikasının yerel PFX kopyasını oluşturma](https://blogs.msdn.microsoft.com/appserviceteam/2017/02/24/creating-a-local-pfx-copy-of-app-service-certificate/).

Daha fazla bilgi için [App Service sertifikaları ve özel etki alanları hakkında SSS](https://social.msdn.microsoft.com/Forums/azure/f3e6faeb-5ed4-435a-adaa-987d5db43b80/faq-on-app-service-certificates-and-custom-domains?forum=windowsazurewebsitespreview).


## <a name="why-do-i-see-the-message-partially-succeeded-when-i-try-to-back-up-my-web-app"></a>Web Uygulamam geri yüklemeye çalıştığımda neden "Kısmen başarılı oldu" iletisini görüyorum?

Yaygın bir nedeni, yedekleme hatası bazı dosyalar uygulama tarafından olmasıdır. Yedekleme gerçekleştirirken kullanımda olan dosyalar kilitli olmadığı. Bu, bu dosyalar yedeklenen engeller ve bir "Kısmen başarılı" durumu neden olabilir. Büyük olasılıkla bu yedekleme işleminden gelen dosyaları hariç tutarak oluşmasını engelleyebilir. Yalnızca gerekli öğeleri oluşturan geri seçebilirsiniz. Daha fazla bilgi için [sitenizi Azure web apps ile yalnızca önemli bölümleri yedekleme](https://zainrizvi.io/blog/creating-partial-backups-of-your-site-with-azure-web-apps/).

## <a name="how-do-i-remove-a-header-from-the-http-response"></a>Gelen HTTP yanıt üstbilgi nasıl kaldırabilirim?

Gelen HTTP yanıt üst bilgilerini kaldırmak için sitenizin web.config dosyasını güncelleştirin. Daha fazla bilgi için [kaldırın, Azure Web siteleri standart sunucu üstbilgilerinde](https://azure.microsoft.com/blog/removing-standard-server-headers-on-windows-azure-web-sites/).

## <a name="is-app-service-compliant-with-pci-standard-30-and-31"></a>App Service PCI standart 3.0 ve 3.1 ile uyumlu mu?

Şu anda, Azure App Service'in Web Apps özelliği uyduğunuzu PCI veri güvenliği standardı (DSS) sürüm 3.0 düzey 1 ' dir. PCI DSS 3.1 sürümünü yol haritamızda ' dir. Planlama zaten devam ettiği en son standart benimseme nasıl devam için var.

Aktarım Katmanı Güvenliği (TLS) 1.0 devre dışı bırakma, PCI DSS 3.1 sürümünü gerektirir. Şu anda, TLS 1.0 devre dışı bırakma çoğu App Service planları için bir seçenek değildir. Ancak, App Service Ortamı'nı kullanın ya da iş yükünüz için App Service ortamı geçirmek istiyorsanız, ortamınızın daha fazla denetim elde edebilirsiniz. Bu, Azure Destek birimine başvurarak TLS 1.0 devre dışı bırakma içerir. Yakın gelecekte, bu ayarlar kullanıcılar tarafından erişilebilir kılın planlıyoruz.

Daha fazla bilgi için [PCI standart 3.0 ve 3.1 Microsoft Azure App Service web app uyumluluğa](https://support.microsoft.com/help/3124528).

## <a name="how-do-i-use-the-staging-environment-and-deployment-slots"></a>Hazırlama ortamını ve dağıtım yuvaları nasıl kullanabilirim?

Web uygulamanızı App Service'e dağıttığınızda standart ve Premium App Service planlarında yerine ayrı bir dağıtım yuvası varsayılan üretim yuvasına dağıtım yapabilirsiniz. Dağıtım yuvaları kendi ana bilgisayar adları olan canlı web uygulamalardır. Web app içerik ve yapılandırma öğeleri, üretim yuvası dahil iki dağıtım yuvası arasında değişiklik yapılabilir.

Dağıtım yuvaları kullanma hakkında daha fazla bilgi için bkz. [App Service'te hazırlık ortamı ayarlama](deploy-staging-slots.md).

## <a name="how-do-i-access-and-review-webjob-logs"></a>Nasıl erişim ve WebJob günlükleri gözden?

WebJob günlükleri gözden geçirmek için:

1. Oturum açın, [Kudu Web sitesi](https://*yourwebsitename*.scm.azurewebsites.net).
2. Webjob'ı seçin.
3. Seçin **çıkışı Aç/Kapat** düğmesi.
4. Çıktı dosyasını indirmek için seçin **indirme** bağlantı.
5. Tek tek çalıştırmaları için seçin **tek çağırma**.
6. Seçin **çıkışı Aç/Kapat** düğmesi.
7. İndirme bağlantısı seçin.

## <a name="im-trying-to-use-hybrid-connections-with-sql-server-why-do-i-see-the-message-systemoverflowexception-arithmetic-operation-resulted-in-an-overflow"></a>SQL Server ile karma bağlantılar kullanabilmesi hale getirmeye çalışıyorum. İletiyi neden görüyorum "System.OverflowException: Aritmetik işlem taşma ile sonuçlandı"?

SQL Server'a erişmek için karma bağlantılar'ı kullanırsanız, bir Microsoft .NET güncelleştirme 10 Mayıs 2016, bağlantıların başarısız olmasına neden. Bu iletiyi görebilirsiniz:

```
Exception: System.Data.Entity.Core.EntityException: The underlying provider failed on Open. —> System.OverflowException: Arithmetic operation resulted in an overflow. or (64 bit Web app) System.OverflowException: Array dimensions exceeded supported range, at System.Data.SqlClient.TdsParser.ConsumePreLoginHandshake
```

### <a name="resolution"></a>Çözüm

Özel durum, karma bağlantı beri düzeltildiğini Yöneticisi ile bir sorun nedeniyle oluştu. Mutlaka [karma bağlantı yöneticinizi güncelleştirin](https://go.microsoft.com/fwlink/?LinkID=841308) bu sorunu çözmek için.

## <a name="how-do-i-add-or-edit-a-url-rewrite-rule"></a>Nasıl eklerim veya bir URL yeniden yazma kuralı Düzenle?

URL yeniden yazma kuralı ekleme veya düzenleme için:

1. App Service web uygulamanıza bağlanır, böylece Internet Information Services (IIS) Yöneticisi'ni ayarlayın. App Service için IIS Yöneticisi'ni bağlanma hakkında bilgi almak için bkz: [IIS Yöneticisi'ni kullanarak Azure Web Siteleri'nde uzaktan yönetimi](https://azure.microsoft.com/blog/remote-administration-of-windows-azure-websites-using-iis-manager/).
2. IIS Yöneticisi'nde, eklemek veya bir URL yeniden yazma kuralı düzenleyin. URL yeniden yazma kuralı ekleme veya düzenleme konusunda bilgi almak için bkz: [URL yeniden yazma kuralları oluşturma, yeniden yazma Modülü](https://www.iis.net/learn/extensions/url-rewrite-module/creating-rewrite-rules-for-the-url-rewrite-module).

## <a name="how-do-i-control-inbound-traffic-to-app-service"></a>App Service için gelen trafiği nasıl kontrol edebilirim?

Site düzeyinde App Service için gelen trafiği denetlemek için iki seçeneğiniz vardır:

* Dinamik IP kısıtlamaları etkinleştirin. Dinamik IP kısıtlamaları devre dışı bırakma bilgi edinmek için [Azure Web siteleri için IP ve etki alanı kısıtlamaları](https://azure.microsoft.com/blog/ip-and-domain-restrictions-for-windows-azure-web-sites/).
* Modül güvenliği'ni açın. Modül güvenliği öğrenmek için bkz. [ModSecurity web uygulaması güvenlik duvarı Azure Web Siteleri'nde](https://azure.microsoft.com/blog/modsecurity-for-azure-websites/).

App Service ortamı kullanıyorsanız, kullanabileceğiniz [Barracuda Güvenlik Duvarı](https://azure.microsoft.com/blog/configuring-barracuda-web-application-firewall-for-azure-app-service-environment/).

## <a name="how-do-i-block-ports-in-an-app-service-web-app"></a>Bir App Service web uygulamasında bağlantı noktalarını nasıl engelleyebilir?

Paylaşılan çok kiracılı App Service Ortamı'nda altyapısının yapısı nedeniyle belirli bağlantı noktalarını engellemek mümkün değildir. 4016 4018 ve 4020 TCP bağlantı noktaları ayrıca Visual Studio uzaktan hata ayıklama için açık olabilir.

App Service Ortamı'nda, gelen ve giden trafiği üzerinde tam denetime sahiptir. Erişimi kısıtlamak veya belirli bağlantı noktalarını engellemek için ağ güvenlik grupları'nı kullanabilirsiniz. App Service ortamı hakkında daha fazla bilgi için bkz: [App Service ortamı ile tanışın](https://azure.microsoft.com/blog/introducing-app-service-environment/).

## <a name="how-do-i-capture-an-f12-trace"></a>F12 izleme nasıl yakalama?

F12 izleme dosyası yakalama işleminde için iki seçeneğiniz vardır:

* F12 HTTP izleme
* F12 konsol çıktısı

### <a name="f12-http-trace"></a>F12 HTTP izleme

1. Internet Explorer'da, Web sitenize gidin. Sonraki adımları uygulamadan önce oturum açmanız önemlidir. Aksi takdirde, F12 izleme oturum hassas verileri yakalar.
2. F12 tuşuna basın.
3. Doğrulayın **ağ** sekmesi seçili olduğundan ve yeşil seçip **Play** düğmesi.
4. Sorunu yeniden oluşturma adımları uygulayın.
5. Kırmızı seçin **Durdur** düğmesi.
6. Seçin **Kaydet** düğmesine (disk simgesi) ve (Internet Explorer ve Microsoft Edge) HAR dosyasını kaydedin *veya* HAR dosyasını sağ tıklayın ve ardından **içerikleHARolarakKaydet**(chrome'da).

### <a name="f12-console-output"></a>F12 konsol çıktısı

1. Seçin **konsol** sekmesi.
2. Sıfırdan fazla öğeleri içeren her sekme için sekmeyi seçin (**hata**, **uyarı**, veya **bilgi**). Sekmesi seçili değilse, imleç aldığı tab simgesi gri siyah olduğunda.
3. Bölmenin ileti alanına sağ tıklayın ve ardından **Tümünü Kopyala**.
4. Bir dosyada kopyalanan metni yapıştırın ve dosyayı kaydedin.

Bir HAR dosyasını görüntülemek için kullanabileceğiniz [HAR Görüntüleyicisi](https://www.softwareishard.com/har/viewer/).

## <a name="why-do-i-get-an-error-when-i-try-to-connect-an-app-service-web-app-to-a-virtual-network-that-is-connected-to-expressroute"></a>Bir App Service bağlanmaya çalışırken neden bir hata Expressroute'a bağlı bir sanal ağ web uygulamasına alırım?

Bir Azure web uygulaması için Azure ExpressRoute bağlı olduğu bir sanal ağa bağlanmaya çalışırsanız, başarısız olur. Aşağıdaki ileti görünür: "Ağ geçidi bir VPN ağ geçidi değil."

Expressroute'a bağlanan bir sanal ağa noktadan siteye VPN bağlantıları şu anda sahip olamaz. Bir noktadan siteye VPN ve ExpressRoute aynı sanal ağ için bir arada bulunamaz. Daha fazla bilgi için [ExpressRoute ve siteden siteye VPN bağlantıları sınırlar ve sınırlamalar](../expressroute/expressroute-howto-coexist-classic.md#limits-and-limitations).

## <a name="how-do-i-connect-an-app-service-web-app-to-a-virtual-network-that-has-a-static-routing-policy-based-gateway"></a>Bir App Service web uygulaması statik yönlendirme (ilke tabanlı) ağ geçidi olan bir sanal ağa nasıl bağlanabilirim?

Şu anda, App Service web uygulaması statik yönlendirme (ilke tabanlı) ağ geçidi olan bir sanal ağa bağlanması desteklenmiyor. Hedef sanal ağınız zaten varsa, noktadan siteye VPN, dinamik yönlendirme ağ geçidi ile bir uygulamaya bağlı önce etkinleştirilmiş olması gerekir. Statik yönlendirme ağ geçidi olarak ayarlanırsa, noktadan siteye VPN etkinleştiremezsiniz. 

Daha fazla bilgi için [bir Azure sanal ağı ile bir uygulamayı tümleştirin](web-sites-integrate-with-vnet.md#getting-started).

## <a name="in-my-app-service-environment-why-can-i-create-only-one-app-service-plan-even-though-i-have-two-workers-available"></a>Ben iki çalışan kullanılabilir olsa bile my App Service Ortamı'nda neden yalnızca bir App Service planı oluşturabilir miyim?

Hataya dayanıklılık sağlamak için App Service ortamı, her çalışan havuzu en az bir ek işlem kaynağı gereken gerektirir. Ek bilgi işlem iş yükü atanamaz.

Daha fazla bilgi için [bir App Service ortamı oluşturma](environment/app-service-web-how-to-create-an-app-service-environment.md).

## <a name="why-do-i-see-timeouts-when-i-try-to-create-an-app-service-environment"></a>Bir App Service ortamı oluşturma çalıştığınızda zaman aşımları neden görüyorum?

Bazı durumlarda bir App Service ortamı oluşturma başarısız olur. Bu durumda, etkinlik günlükleri aşağıdaki hatayı görürsünüz:
```
ResourceID: /subscriptions/{SubscriptionID}/resourceGroups/Default-Networking/providers/Microsoft.Web/hostingEnvironments/{ASEname}
Error:{"error":{"code":"ResourceDeploymentFailure","message":"The resource provision operation did not complete within the allowed timeout period.”}}
```

Bu sorunu çözmek için aşağıdaki koşulların hiçbiri doğru olduğundan emin olun:
* Alt ağ çok küçükse.
* Alt ağ boş değil.
* ExpressRoute, App Service ortamı ağ bağlantısı gereksinimlerine engeller.
* Hatalı ağ güvenlik grubu, App Service ortamı ağ bağlantısı gereksinimlerine engeller.
* Zorlamalı tünel açıktır.

Daha fazla bilgi için [sık karşılaşılan sorunlar (oluşturma) dağıtırken yeni bir Azure App Service ortamı](https://blogs.msdn.microsoft.com/waws/2016/05/13/most-frequent-issues-when-deploying-creating-a-new-azure-app-service-environment-ase/).

## <a name="why-cant-i-delete-my-app-service-plan"></a>App Service planımı neden silemiyorum?

Herhangi bir App Service uygulamaları App Service planı ile ilişkili olan bir App Service planı silemezsiniz. Bir App Service planı silmeden önce ilişkili tüm App Service uygulamaları App Service planından kaldırın.

## <a name="how-do-i-schedule-a-webjob"></a>Nasıl bir Web işi zamanlama?

Zamanlanan Webjob'lar Cron ifadeleri kullanarak oluşturabilirsiniz:

1. Bir settings.job dosyası oluşturun.
2. Bu JSON dosyasında bir Cron ifadesi kullanarak bir zamanlama özellik içerir: 
    ```json
    { "schedule": "{second}
    {minute} {hour} {day}
    {month} {day of the week}" }
    ```

Zamanlanan Webjob'lar hakkında daha fazla bilgi için bkz: [bir Cron ifadesi kullanarak bir zamanlanmış Web işi oluşturma](webjobs-create.md#CreateScheduledCRON).

## <a name="how-do-i-perform-penetration-testing-for-my-app-service-app"></a>App Service Uygulamam için sızma nasıl yaparım?

Sızma testi, gerçekleştirilecek [talebinizi](https://portal.msrc.microsoft.com/en-us/engage/pentest).

## <a name="how-do-i-configure-a-custom-domain-name-for-an-app-service-web-app-that-uses-traffic-manager"></a>Özel etki alanı adını Traffic Manager'ı kullanan bir App Service web uygulaması için nasıl yapılandırabilirim?

Azure Traffic Manager Yük Dengeleme için kullandığı bir App Service uygulaması ile bir özel etki alanı adını kullanmayı öğrenmek için bkz: [Traffic Manager ile bir Azure web uygulaması için bir özel etki alanı adı yapılandırma](web-sites-traffic-manager-custom-domain-name.md).

## <a name="my-app-service-certificate-is-flagged-for-fraud-how-do-i-resolve-this"></a>My App Service sertifikası dolandırıcılık için işaretlenir. Bu nasıl giderebilirim?

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bir App Service sertifikası satın alma sırasında etki alanı doğrulaması, şu iletiyi görebilirsiniz:

"Sertifikanız olası dolandırıcılık için işaretlendi. İstek şu anda incelenmektedir. Sertifika 24 saat içinde kullanılabilir hale gelmezse, lütfen Azure desteğine başvurun."

Mesajın gösterdiği gibi bu sahtekarlık doğrulama işleminin tamamlanması 24 saat sürebilir. Bu süre boyunca, iletiyi görmeye devam.

App Service sertifikanızı 24 saat sonra bu mesajını göstermeye devam ederse, lütfen aşağıdaki PowerShell betiğini çalıştırın. Betik kişiler [sertifika sağlayıcısı](https://www.godaddy.com/) doğrudan bu sorunu çözmek için.

```powershell
Connect-AzAccount
Set-AzContext -SubscriptionId <subId>
$actionProperties = @{
    "Name"= "<Customer Email Address>"
    };
Invoke-AzResourceAction -ResourceGroupName "<App Service Certificate Resource Group Name>" -ResourceType Microsoft.CertificateRegistration/certificateOrders -ResourceName "<App Service Certificate Resource Name>" -Action resendRequestEmails -Parameters $actionProperties -ApiVersion 2015-08-01 -Force   
```

## <a name="how-do-authentication-and-authorization-work-in-app-service"></a>Nasıl kimlik doğrulaması ve yetkilendirme App Service'te çalışır?

Docs sağlayıcısı oturum açma işlemleri tanımlamak için çeşitli kimlik doğrulama ve yetkilendirme App Service için ayrıntılı belgelere bakın:
* [Azure Active Directory](configure-authentication-provider-aad.md)
* [Facebook](configure-authentication-provider-facebook.md)
* [Google](configure-authentication-provider-google.md)
* [Microsoft Hesabı](configure-authentication-provider-microsoft.md)
* [Twitter](configure-authentication-provider-twitter.md)

## <a name="how-do-i-redirect-the-default-azurewebsitesnet-domain-to-my-azure-web-apps-custom-domain"></a>Ne miyim yeniden yönlendirme varsayılan *. azurewebsites.net etki alanında my Azure web uygulamanızın özel etki alanı için mi?

Azure, varsayılan Web Apps kullanarak yeni bir Web sitesi oluşturduğunuzda *sitename*. azurewebsites.net etki alanında, sitenize atanır. Bir özel konak adı sitenize ekleyin ve varsayılan erişebilmesi için kullanıcıların istemediğiniz *. azurewebsites.net etki alanında, varsayılan URL yeniden yönlendirebilirsiniz. Özel etki alanınızda Web sitesinin varsayılan etki alanından tüm trafik yönlendirme hakkında bilgi edinmek için [varsayılan etki alanı, Azure web apps'te özel etki alanınızı yeniden yönlendirme](https://zainrizvi.io/blog/block-default-azure-websites-domain/).

## <a name="how-do-i-determine-which-version-of-net-version-is-installed-in-app-service"></a>Hangi sürümün nasıl belirleyebilirim .NET App Service'te sürümü yüklenir?

Microsoft .NET, App Service'te yüklü sürümü bulmak için en hızlı yolu, Kudu konsolunu kullanmaktır. Kudu konsolunu portalından veya App Service uygulamanızın URL'sini kullanarak erişebilirsiniz. Ayrıntılı yönergeler için bkz. [yüklü olan .NET sürümünü App Service'te](https://blogs.msdn.microsoft.com/waws/2016/11/02/how-to-determine-the-installed-net-version-in-azure-app-services/).

## <a name="why-isnt-autoscale-working-as-expected"></a>Neden otomatik ölçeklendirme beklendiği gibi çalışmıyor?

Azure otomatik ölçeklendirme edilmemiş veya ölçeği, beklendiği gibi web app örneğini ölçeği, size bir senaryo, kasıtlı olarak "dalgalanma." nedeniyle sonsuz bir döngüye önlemek için ölçeklendirme değil Seçtiğimiz içine çalışıyor olabilir Bu durum, genellikle genişletmek ve ölçeğini eşiklerin arasında yeterli bir kenar boşluğu olmadığında gerçekleşir. "Dalgalanma" önlemek için ve diğer otomatik ölçeklendirme en iyi yöntemler hakkında okunacak öğrenmek için bkz: [otomatik ölçeklendirme en iyi](../azure-monitor/platform/autoscale-best-practices.md#autoscale-best-practices).

## <a name="why-does-autoscale-sometimes-scale-only-partially"></a>Neden otomatik bazen yalnızca kısmen ölçeklendirme?

Otomatik ölçeklendirme, ölçüm önceden yapılandırılmış sınırları aşan tetiklenir. Bazı durumlarda, kapasite yalnızca kısmen beklediğiniz karşılaştırıldığında doldurulduğunu fark edebilirsiniz. İstediğiniz örnek sayısını olmadığında bu durum oluşabilir. Bu senaryoda, otomatik ölçeklendirme, kısmen kullanılabilir örneklerin sayısı ile doldurur. Otomatik ölçeklendirme, daha sonra daha fazla kapasite almak için yeniden Dengeleme mantığı çalıştırır. Bu, kalan örnekleri ayırır. Bu işlem birkaç dakika sürebileceğini unutmayın.

Birkaç dakika sonra örnekleri beklenen sayıda görmüyorsanız, kısmi Dolum sınırları içinde ölçümleri getirmek için yeterli olduğundan olabilir. Ya da daha düşük ölçümleri sınırına ulaştığınız için otomatik ölçeklendirme ölçeği.

Bu koşulların hiçbiri geçerli ve sorun devam ederse, bir destek isteği gönderin.

## <a name="how-do-i-turn-on-http-compression-for-my-content"></a>HTTP sıkıştırmayı İçeriğim için nasıl kapatırım?

Sıkıştırma hem statik ve dinamik içerik türleri için açmak için uygulama düzeyi web.config dosyasına aşağıdaki kodu ekleyin:

```xml
<system.webServer>
    <urlCompression doStaticCompression="true" doDynamicCompression="true" />
</system.webServer>
```

Sıkıştırılacak istediğiniz belirli dinamik ve statik MIME türleri de belirtebilirsiniz. Forum soru için yanıt sürelerimiz daha fazla bilgi için bkz. [httpCompression ayarları basit bir Azure Web sitesinde](https://social.msdn.microsoft.com/Forums/azure/890b6d25-f7dd-4272-8970-da7798bcf25d/httpcompression-settings-on-a-simple-azure-website?forum=windowsazurewebsitespreview).

## <a name="how-do-i-migrate-from-an-on-premises-environment-to-app-service"></a>Bir şirket içi ortamdan App Service'e nasıl geçirebilirim?

Siteler, Windows ve Linux web sunucularından App Service'e geçirmek için Azure App Service Migration Yardımcısı'nı kullanabilirsiniz. Geçiş Aracı gerektiği gibi Azure'da web uygulamalarını ve veritabanlarını oluşturur ve ardından içeriği yayımlar. Daha fazla bilgi için [Azure App Service Migration Yardımcısı](https://www.migratetoazure.net/).
