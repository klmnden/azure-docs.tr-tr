---
title: "Azure web uygulamaları için yapılandırma ile ilgili SSS | Microsoft Docs"
description: "Azure App Service Web Apps özelliği için yapılandırması ve yönetimi sorunları hakkında sık sorulan soruların yanıtlarını alın."
services: app-service\web
documentationcenter: 
author: genlin
manager: cshepard
editor: 
tags: top-support-issue
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: genli
ms.openlocfilehash: cc17196603a5bdcd7f880c3650512846fa0facef
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="configuration-and-management-faqs-for-web-apps-in-azure"></a>Azure Web uygulamalarının yapılandırması ve Yönetimi SSS

Bu makale için yapılandırması ve yönetimi sorunları hakkında sık sorulan sorular (SSS) yanıtlarını sahip [Azure App Service Web Apps özelliğini](https://azure.microsoft.com/services/app-service/web/).

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="are-there-limitations-i-should-be-aware-of-if-i-want-to-move-app-service-resources"></a>Uygulama hizmeti kaynakları taşımak istiyorsanız farkında olmalıdır sınırlamalar var mı?

Uygulama hizmeti kaynakları yeni kaynak grubu ya da abonelik taşımayı planlıyorsanız, dikkat edilmesi gereken bazı sınırlamalar vardır. Daha fazla bilgi için bkz: [App Service sınırlamalar](../azure-resource-manager/resource-group-move-resources.md#app-service-limitations).

## <a name="how-do-i-use-a-custom-domain-name-for-my-web-app"></a>Web Uygulamam için özel etki alanı adı nasıl kullanabilirim?

Özel etki alanı adı, Azure web uygulaması ile kullanma hakkında sık sorulan soruların yanıtlarını görmek için bizim yedi dakikalık video [özel etki alanı ekleme](https://channel9.msdn.com/blogs/Azure-App-Service-Self-Help/Add-a-Custom-Domain-Name). Video nasıl özel etki alanı eklemek bir kılavuz sağlar. Kendi URL yerine kullanmayı açıklar *. azurewebsites.net URL App Service web uygulaması ile. Ayrıntılı bilgileri de görebilirsiniz [özel etki alanı adını eşleştirmek nasıl](app-service-web-tutorial-custom-domain.md).


## <a name="how-do-i-purchase-a-new-custom-domain-for-my-web-app"></a>Web Uygulamam için nasıl yeni bir özel etki alanı satın?

Satın alma ve App Service web uygulamanız için özel bir etki alanı ayarlama hakkında bilgi almak için bkz: [satın alın ve App Service'te özel etki alanı adı yapılandırma](custom-dns-web-site-buydomains-web-app.md).


## <a name="how-do-i-upload-and-configure-an-existing-ssl-certificate-for-my-web-app"></a>Nasıl karşıya yükleme ve web Uygulamam için mevcut bir SSL sertifikası yapılandırma?

Karşıya yüklemek ve var olan bir özel SSL sertifika ayarlama hakkında bilgi almak için bkz: [Azure web uygulaması için var olan özel bir SSL sertifikası bağlama](app-service-web-tutorial-custom-ssl.md#upload).


## <a name="how-do-i-purchase-and-configure-a-new-ssl-certificate-in-azure-for-my-web-app"></a>Nasıl satın alın ve web Uygulamam için Azure'da yeni bir SSL sertifikası yapılandırma?

Satın alma ve App Service web uygulamanız için bir SSL sertifikası ayarlama hakkında bilgi almak için bkz: [uygulama hizmeti uygulamanızı bir SSL sertifikası ekleme](web-sites-purchase-ssl-web-site.md).


## <a name="how-do-i-move-application-insights-resources"></a>Application Insights kaynakları nasıl taşıyabilirim?

Şu anda Azure Application Insights taşıma işlemini desteklemiyor. Özgün kaynak grubunuz Application Insights kaynağı içeriyorsa, bu kaynak taşınamıyor. Bir App Service uygulaması taşımaya çalıştığınızda Application Insights kaynağı eklerseniz, tüm işlem başarısız taşıyın. Ancak, Application Insights ve uygulama hizmeti planı düzgün çalışabilmesi için aynı kaynak grubunda için uygulaması olarak olması gerekmez.

Daha fazla bilgi için bkz: [App Service sınırlamalar](../azure-resource-manager/resource-group-move-resources.md#app-service-limitations).

## <a name="where-can-i-find-a-guidance-checklist-and-learn-more-about-resource-move-operations"></a>Burada ı kılavuzu denetim bulabilir ve bilgi kaynağı hakkında daha fazla taşıma işlemlerini?

[App Service sınırlamalar](../azure-resource-manager/resource-group-move-resources.md#app-service-limitations) yeni bir abonelik veya yeni bir kaynak grubu aynı Abonelikteki kaynakların taşınması gösterilmiştir. Kaynak taşıma denetim listesi hakkında bilgi almak, hangi hizmetlerin taşıma işlemini desteklemek ve App Service sınırlamalar ve diğer konular hakkında daha fazla bilgi öğrenin.

## <a name="how-do-i-set-the-server-time-zone-for-my-web-app"></a>Web Uygulamam için nasıl sunucu saat dilimini ayarlar?

Web uygulamanız için sunucu saat dilimini ayarlamak için:

1. Azure portalında uygulama hizmeti aboneliğinizi Git **uygulama ayarları** menüsü.
2. Altında **uygulama ayarları**, bu ayarı ekleyin:
    * Anahtar WEBSITE_TIME_ZONE =
    * Değer = *istediğiniz saat dilimi*
3. **Kaydet**’i seçin.

## <a name="why-do-my-continuous-webjobs-sometimes-fail"></a>Neden my sürekli Webjob'lar bazen başarısız?

Varsayılan olarak ayarlanmış bir süre için boşta olmaları durumunda web uygulamaları kaldırılır. Bu kaynakların tasarrufu sistem sağlar. Temel ve standart planlarda, açabilirsiniz **her zaman açık** yüklenen her zaman web uygulaması tutmak için ayarlama. Web uygulamanızı sürekli Webjob'lar çalıştırıyorsa, açmanız **her zaman açık**, veya WebJobs güvenilir bir şekilde çalışmayabilir. Daha fazla bilgi için bkz: [sürekli olarak çalışan bir WebJob oluşturma](web-sites-create-web-jobs.md#CreateContinuous).

## <a name="how-do-i-get-the-outbound-ip-address-for-my-web-app"></a>Web Uygulamam için giden IP adresi nasıl sağlarım?

Web uygulamanız için giden IP adreslerinin listesini almak için:

1. Azure portalında, web uygulaması dikey Git **özellikleri** menüsü.
2. Arama **giden IP adreslerini**.

Giden IP adresleri listesi görüntülenir.

Web sitenizi PowerApps için uygulama hizmeti ortamı'nda barındırılıyorsa, giden IP adresiniz alma hakkında bilgi için bkz: [giden ağ adresleri](environment/app-service-app-service-environment-network-architecture-overview.md#outbound-network-addresses).

## <a name="how-do-i-get-a-reserved-or-dedicated-inbound-ip-address-for-my-web-app"></a>Web Uygulamam için nasıl bir ayrılmış ya da ayrılmış gelen IP adresi elde?

Azure uygulaması Web sitenize yapılan gelen çağrıları için adanmış veya ayrılmış bir IP adresi ayarlamak için yükleme ve bir IP tabanlı SSL sertifikası yapılandırın.

Gelen çağrıları için adanmış veya ayrılmış bir IP adresi kullanmak için uygulama hizmeti planınızın bir temel veya daha yüksek hizmet planı olması gerektiğini unutmayın.

## <a name="can-i-export-my-app-service-certificate-to-use-outside-azure-such-as-for-a-website-hosted-elsewhere"></a>Azure dışında gibi başka bir yerde barındırılan bir Web sitesi için kullanılacak my uygulama hizmeti sertifika dışa aktarabilirsiniz? 

Uygulama Hizmeti sertifikaları Azure kaynaklarını olarak kabul edilir. Azure hizmetlerinizi dışında kullanmak için amaçlanmamıştır. Azure dışında kullanacak biçimde dışarı aktaramazsınız. Daha fazla bilgi için bkz: [uygulama hizmeti sertifikaları ve özel etki alanları için SSS](https://social.msdn.microsoft.com/Forums/azure/f3e6faeb-5ed4-435a-adaa-987d5db43b80/faq-on-app-service-certificates-and-custom-domains?forum=windowsazurewebsitespreview).

## <a name="can-i-export-my-app-service-certificate-to-use-with-other-azure-cloud-services"></a>Diğer Azure bulut Hizmetleri ile kullanmak üzere my uygulama hizmeti sertifika dışa aktarabilirsiniz?

Portal, Azure anahtar kasası ile bir uygulama hizmeti sertifikası için uygulama hizmeti uygulamalarını dağıtmak için birinci sınıf bir deneyim sağlar. Ancak, biz istekleri Azure sanal makinelerle Örneğin, App Service platformu dışında bu sertifikaları kullanacak müşterilerden alıyor olabilir. Sertifikanın diğer Azure kaynakları ile kullanabilmek için bir yerel PFX sertifikanın bir kopyasını uygulama hizmeti oluşturmayı öğrenmek için bkz: [yerel bir uygulama hizmeti Sertifika PFX kopyasını oluşturmak](https://blogs.msdn.microsoft.com/appserviceteam/2017/02/24/creating-a-local-pfx-copy-of-app-service-certificate/).

Daha fazla bilgi için bkz: [uygulama hizmeti sertifikaları ve özel etki alanları için SSS](https://social.msdn.microsoft.com/Forums/azure/f3e6faeb-5ed4-435a-adaa-987d5db43b80/faq-on-app-service-certificates-and-custom-domains?forum=windowsazurewebsitespreview).


## <a name="why-do-i-see-the-message-partially-succeeded-when-i-try-to-back-up-my-web-app"></a>Web Uygulamam geri yüklemeye çalıştığınızda "Kısmen başarılı oldu" iletisini neden görüyorum?

Bir ortak yedekleme hatası bazı dosyalar uygulama tarafından kullanılmakta olan nedenidir. Yedekleme gerçekleştirirken kullanılmakta olan dosyaları kilitlenir. Bu, bu dosyaları yedeklenen engeller ve bir "Kısmen başarılı oldu" durumu sağlayabilir. Büyük olasılıkla bu dosya yedekleme işleminden hariç tutarak oluşmasını engelleyebilir. Yalnızca gerekli olanla yukarı geri seçebilirsiniz. Daha fazla bilgi için bkz: [Azure web uygulamaları ile sitenizin yalnızca önemli bölümleri yedekleme](http://www.zainrizvi.io/2015/06/05/creating-partial-backups-of-your-site-with-azure-web-apps/).

## <a name="how-do-i-remove-a-header-from-the-http-response"></a>Gelen HTTP yanıtı nasıl üstbilgi kaldırılsın mı?

Gelen HTTP yanıtı üstbilgileri kaldırmak için sitenizin web.config dosyasını güncelleştirin. Daha fazla bilgi için bkz: [kaldırmak, Azure Web siteleri standart sunucu üstbilgilerinde](https://azure.microsoft.com/blog/removing-standard-server-headers-on-windows-azure-web-sites/).

## <a name="is-app-service-compliant-with-pci-standard-30-and-31"></a>Uygulama hizmeti PCI standart 3.0 ve 3.1 ile uyumlu mu?

Şu anda, Azure App Service Web Apps özelliğidir uyumlu PCI veri güvenliği standardı (DSS) sürüm 3.0 düzey 1 ' dir. PCI DSS sürüm 3.1 bizim yol haritası üzerinde ' dir. Planlama zaten nasıl son standart benimsenmesi devam edecek için işleniyor.

PCI DSS sürüm 3.1 sertifika Aktarım Katmanı Güvenliği (TLS) 1.0 devre dışı bırakılması gerekir. Şu anda, TLS 1.0 devre dışı bırakma çoğu uygulama hizmeti planları için bir seçenek değil. Ancak, uygulama hizmeti ortamı veya uygulama hizmeti ortamı için İş yükünüzün geçirmek istiyorum kullanıyorsanız, ortamınızın daha fazla denetim elde edebilirsiniz. Bu, TLS 1.0 Azure desteği ile iletişim kurarak devre dışı bırakma içerir. Yakın gelecekte bu ayarlar kullanıcılar tarafından erişilebilir planlıyoruz.

Daha fazla bilgi için bkz: [PCI standart 3.0 ve 3.1 ile Microsoft Azure App Service web uygulama uyumluluğu](https://support.microsoft.com/help/3124528).

## <a name="how-do-i-use-the-staging-environment-and-deployment-slots"></a>Hazırlama ortamı ve dağıtım yuvası nasıl kullanabilirim?

Web uygulamanızı App Service'e dağıtma, standart ve Premium uygulama hizmeti planları, varsayılan üretim yuvasıyla yerine ayrı bir dağıtım yuvası dağıtabilirsiniz. Dağıtım yuvaları, kendi ana bilgisayar adları olan canlı web uygulamalardır. Web uygulaması içerik ve yapılandırma öğeleri üretim yuvası da dahil olmak üzere iki dağıtım yuvası arasında değişiklik yapılabilir.

Dağıtım yuvaları kullanma hakkında daha fazla bilgi için bkz: [hazırlama bir uygulama hizmeti ortamında ayarlama](web-sites-staged-publishing.md).

## <a name="how-do-i-access-and-review-webjob-logs"></a>Nasıl erişmek ve Web işi günlüklerini gözden geçirin?

Web işi günlüklerini gözden geçirmek için:

1. Oturum açın, [Kudu Web sitesi](https://*yourwebsitename*.scm.azurewebsites.net).
2. Web işi seçin.
3. Seçin **geçiş çıktı** düğmesi.
4. Çıkış dosyasını yüklemek üzere seçin **karşıdan** bağlantı.
5. Tek tek çalıştırmalarında seçin **tek tek çağırma**.
6. Seçin **geçiş çıktı** düğmesi.
7. Karşıdan yükleme bağlantısını seçin.

## <a name="im-trying-to-use-hybrid-connections-with-sql-server-why-do-i-see-the-message-systemoverflowexception-arithmetic-operation-resulted-in-an-overflow"></a>SQL Server ile karma bağlantılar kullanabilmesi çalışıyorum. Neden iletisini görüyorum "System.OverflowException: Aritmetik işlem taşması ile sonuçlandı"?

SQL Server'a erişmek için karma bağlantılar kullanırsanız, Microsoft .NET güncelleştirmesi 10 Mayıs 2016 bağlantıların başarısız olmasına neden olabilir. Bu iletiyi görebilirsiniz:

```
Exception: System.Data.Entity.Core.EntityException: The underlying provider failed on Open. —> System.OverflowException: Arithmetic operation resulted in an overflow. or (64 bit Web app) System.OverflowException: Array dimensions exceeded supported range, at System.Data.SqlClient.TdsParser.ConsumePreLoginHandshake
```

### <a name="resolution"></a>Çözüm

Bu sorunu gidermek için karma Bağlantı Yöneticisi'ni güncelleştirmek için çalışıyoruz. Geçici çözümler için bkz: [SQL Server ile karma bağlantılar hatası: System.OverflowException: Aritmetik işlem taşması ile sonuçlandı](https://blogs.msdn.microsoft.com/waws/2016/05/17/hybrid-connection-error-with-sql-server-system-overflowexception-arithmetic-operation-resulted-in-an-overflow/).

## <a name="how-do-i-add-or-edit-a-url-rewrite-rule"></a>Nasıl eklemek veya bir URL yeniden yazma kuralı düzenleme?

URL yeniden yazma Kuralı Ekle veya Düzenle için:

1. Internet Information Services (IIS) Manager'ı, App Service web uygulamanızın bağlayan şekilde ayarlayın. IIS Yöneticisi'ni Uygulama hizmetine bağlanmak öğrenmek için bkz: [uzaktan yönetimi, IIS Yöneticisi'ni kullanarak Azure Web siteleri](https://azure.microsoft.com/blog/remote-administration-of-windows-azure-websites-using-iis-manager/).
2. IIS Yöneticisi ' nde ekleyin veya bir URL yeniden yazma kuralı düzenleyin. URL yeniden yazma Kuralı Ekle veya Düzenle öğrenmek için bkz: [Oluştur yeniden yazma kuralları için URL yeniden yazma Modülü](https://www.iis.net/learn/extensions/url-rewrite-module/creating-rewrite-rules-for-the-url-rewrite-module).

## <a name="how-do-i-control-inbound-traffic-to-app-service"></a>Uygulama hizmeti gelen trafiği nasıl kontrol?

Site düzeyinde App Service'e gelen trafiği denetlemek için iki seçeneğiniz vardır:

* Dinamik IP kısıtlamaları etkinleştirin. Dinamik IP kısıtlamaları devre dışı bırakma bilgi edinmek için [Azure Web siteleri için IP ve etki alanı kısıtlamaları](https://azure.microsoft.com/blog/ip-and-domain-restrictions-for-windows-azure-web-sites/).
* Modül güvenliği kapatın. Modül güvenliği öğrenmek için bkz: [ModSecurity web uygulaması Güvenlik Duvarı'nı Azure Web siteleri](https://azure.microsoft.com/blog/modsecurity-for-azure-websites/).

Uygulama hizmeti ortamı kullanırsanız, kullanabileceğiniz [Barracuda Güvenlik Duvarı'nı](https://azure.microsoft.com/blog/configuring-barracuda-web-application-firewall-for-azure-app-service-environment/).

## <a name="how-do-i-block-ports-in-an-app-service-web-app"></a>Bir App Service web uygulaması'nda bağlantı noktaları nasıl engelleyebilir?

Paylaşılan Kiracı uygulama hizmeti ortamı'nda altyapı yapısı nedeniyle belirli bağlantı noktalarını engellemek üzere mümkün değil. 4016, 4018 ve 4020 TCP bağlantı noktaları da Visual Studio uzaktan hata ayıklama için açık olabilir.

Uygulama hizmeti ortamı'nda, gelen ve giden trafik üzerinde tam denetime sahiptir. Erişimi kısıtlamak veya belirli bağlantı noktalarını engellemek için ağ güvenlik gruplarını kullanabilirsiniz. Uygulama hizmeti ortamı hakkında daha fazla bilgi için bkz: [Tanıtımı uygulama hizmeti ortamı](https://azure.microsoft.com/blog/introducing-app-service-environment/).

## <a name="how-do-i-capture-an-f12-trace"></a>Bir F12 izleme nasıl yakalamak?

F12 izleme yakalamak için iki seçeneğiniz vardır:

* F12 HTTP izleme
* F12 konsol çıkışı

### <a name="f12-http-trace"></a>F12 HTTP izleme

1. Internet Explorer'da Web sitenize gidin. Sonraki adımları uygulamadan önce oturum açmak önemlidir. Aksi takdirde, F12 izleme oturum açma hassas verileri yakalar.
2. F12 tuşuna basın.
3. Doğrulayın **ağ** sekmesi seçili ve yeşil seçip **Yürüt** düğmesi.
4. Sorunu yeniden oluşturma adımları uygulayın.
5. Kırmızı seçin **durdurmak** düğmesi.
6. Seçin **kaydetmek** düğmesini (disk simgesi) ve (Internet Explorer ve kenar) HAR dosyasını kaydedin *veya* HAR dosyasını sağ tıklatın ve ardından **içerikle HAR Kaydet** (içinde Chrome).

### <a name="f12-console-output"></a>F12 konsol çıkışı

1. Seçin **konsol** sekmesi.
2. Sıfırdan fazla öğelerini içeren her sekme için sekmeyi seçin (**hata**, **uyarı**, veya **bilgi**). Sekmesi seçili değilse, imleci uzağa taşıdığınızda sekmesini gri veya siyah simgedir.
3. Bölmesinde ileti alanına sağ tıklayın ve ardından **tüm kopyalayın**.
4. Kopyalanan metni bir dosyaya yapıştırın ve ardından dosyayı kaydedin.

HAR dosyayı görüntülemek için kullanabileceğiniz [HAR Görüntüleyicisi](http://www.softwareishard.com/har/viewer/).

## <a name="why-do-i-get-an-error-when-i-try-to-connect-an-app-service-web-app-to-a-virtual-network-that-is-connected-to-expressroute"></a>Bir uygulama hizmeti bağlanmaya çalıştığınızda neden bir hata Expressroute'a bağlı bir sanal ağ web uygulamasına sağlarım?

Azure web uygulaması için Azure ExpressRoute bağlı olduğu sanal bir ağa bağlanmaya çalışırsanız, başarısız olur. Aşağıdaki ileti görünür: "Ağ geçidi değil bir VPN ağ geçidi."

Şu anda Expressroute'a bağlı bir sanal ağa noktadan siteye VPN bağlantıları sahip olamaz. Bir noktadan siteye VPN ve ExpressRoute aynı sanal ağda birlikte çalışamaz. Daha fazla bilgi için bkz: [ExpressRoute ve siteden siteye VPN bağlantıları sınırlar ve sınırlamalar](../expressroute/expressroute-howto-coexist-classic.md#limits-and-limitations).

## <a name="how-do-i-connect-an-app-service-web-app-to-a-virtual-network-that-has-a-static-routing-policy-based-gateway"></a>Statik yönlendirme (ilke tabanlı) ağ geçidi sanal bir ağa nasıl bir App Service web uygulaması bağlanacağını?

Şu anda bir App Service web uygulaması bir statik yönlendirme (ilke tabanlı) ağ geçidi sanal bir ağa bağlanması desteklenmiyor. Hedef sanal ağınız zaten varsa, noktadan siteye VPN, dinamik yönlendirme ağ geçidi ile bir uygulama için bağlı önce etkinleştirilmiş olması gerekir. Statik yönlendirme ağ geçidi ayarlanırsa, noktadan siteye VPN etkinleştiremezsiniz. 

Daha fazla bilgi için bkz: [bir Azure sanal ağı ile bir uygulamayı tümleştirin](web-sites-integrate-with-vnet.md#getting-started).

## <a name="in-my-app-service-environment-why-can-i-create-only-one-app-service-plan-even-though-i-have-two-workers-available"></a>İki çalışanları kullanılabilir olmasına rağmen my uygulama hizmeti ortamı'nda neden yalnızca bir uygulama hizmeti planı oluşturabilirim?

Hataya dayanıklılık sağlamak için her bir çalışan havuzu en az bir ek hesaplama kaynak gerektiğini uygulama hizmeti ortamı gerektirir. Bir iş yükü ek hesaplama kaynak atanamaz.

Daha fazla bilgi için bkz: [bir uygulama hizmeti ortamı oluşturmak nasıl](environment/app-service-web-how-to-create-an-app-service-environment.md).

## <a name="why-do-i-see-timeouts-when-i-try-to-create-an-app-service-environment"></a>Bir uygulama hizmeti ortamı oluşturmak çalıştığınızda zaman aşımları neden görüyorum?

Bazı durumlarda, bir uygulama hizmeti ortamı oluşturma başarısız olur. Bu durumda, etkinlik günlükleri aşağıdaki hatayı görürsünüz:
```
ResourceID: /subscriptions/{SubscriptionID}/resourceGroups/Default-Networking/providers/Microsoft.Web/hostingEnvironments/{ASEname}
Error:{"error":{"code":"ResourceDeploymentFailure","message":"The resource provision operation did not complete within the allowed timeout period.”}}
```

Bu sorunu çözmek için aşağıdaki koşulların hiçbiri doğru olduğundan emin olun:
* Alt ağ çok küçük.
* Alt ağ boş değil.
* ExpressRoute ağ bağlantı gereksinimleri bir uygulama hizmeti ortamı engeller.
* Bozuk bir ağ güvenlik grubu, bir uygulama hizmeti ortamı ağ bağlantı gereksinimleri engeller.
* Zorlamalı tünel açıktır.

Daha fazla bilgi için bkz: [(oluşturma) dağıtırken sorunları sık yeni bir Azure uygulama hizmeti ortamı](https://blogs.msdn.microsoft.com/waws/2016/05/13/most-frequent-issues-when-deploying-creating-a-new-azure-app-service-environment-ase/).

## <a name="why-cant-i-delete-my-app-service-plan"></a>Neden my uygulama hizmeti planı silemezsiniz?

Uygulama hizmeti uygulamalardan uygulama hizmeti plan ile ilişkili ise, bir uygulama hizmeti planı silemezsiniz. Bir uygulama hizmeti planı silmeden önce ilişkili tüm App Service uygulamalarının uygulama hizmeti planında kaldırın.

## <a name="how-do-i-schedule-a-webjob"></a>Bir Web işi nasıl zamanlama?

Zamanlanmış Web işi Cron ifadeler kullanarak oluşturabilirsiniz:

1. Bir settings.job dosyası oluşturun.
2. Bu JSON dosyasında Cron ifade kullanarak bir zamanlama özellik içerir: 
    ```
    { "schedule": "{second}
    {minute} {hour} {day}
    {month} {day of the week}" }
    ```

Zamanlanmış Web işleri hakkında daha fazla bilgi için bkz: [Cron ifade kullanarak bir zamanlanmış WebJob oluşturma](web-sites-create-web-jobs.md#CreateScheduledCRON).

## <a name="how-do-i-perform-penetration-testing-for-my-app-service-app"></a>Uygulama hizmeti Uygulamam için test sızma nasıl yaparım?

Sızma test gerçekleştirmek için [bir istek göndermek](https://security-forms.azure.com/penetration-testing/terms).

## <a name="how-do-i-configure-a-custom-domain-name-for-an-app-service-web-app-that-uses-traffic-manager"></a>Trafik Yöneticisi kullanan bir App Service web uygulaması için bir özel etki alanı adını nasıl yapılandırırım?

Yük Dengeleme için Azure Traffic Manager kullanan bir uygulama hizmeti uygulaması ile özel etki alanı kullanmayı öğrenmek için bkz: [trafik Yöneticisi ile bir Azure web uygulaması için bir özel etki alanı adı yapılandırma](web-sites-traffic-manager-custom-domain-name.md).

## <a name="my-app-service-certificate-is-flagged-for-fraud-how-do-i-resolve-this"></a>My uygulama hizmet sertifikası sahtekarlık için işaretlenir. Bu nasıl giderebilirim?

Bir uygulama hizmeti sertifika satın alma etki alanı doğrulama sırasında şu iletiyi görebilirsiniz:

"Sertifikanızı olası sahtekarlık için işaretlendi. İstek şu anda incelenmektedir. Sertifika 24 saat içinde kullanılabilir olmaz, lütfen Azure desteğine başvurun."

İleti da anlaşılacağı gibi bu sahtekarlık doğrulama işlemi tamamlanması 24 saate kadar sürebilir. Bu süre boyunca, iletiyi görmeye devam.

Uygulama hizmeti sertifikanızı 24 saat sonra bu iletiyi gösterme olmaya devam ederse, lütfen aşağıdaki PowerShell betiğini çalıştırın. Komut dosyası kişiler [sertifika sağlayıcısı](https://www.godaddy.com/) doğrudan bu sorunu gidermek için.

```
Login-AzureRmAccount
Set-AzureRmContext -SubscriptionId <subId>
$actionProperties = @{
    "Name"= "<Customer Email Address>"
    };
Invoke-AzureRmResourceAction -ResourceGroupName "<App Service Certificate Resource Group Name>" -ResourceType Microsoft.CertificateRegistration/certificateOrders -ResourceName "<App Service Certificate Resource Name>" -Action resendRequestEmails -Parameters $actionProperties -ApiVersion 2015-08-01 -Force   
```

## <a name="how-do-authentication-and-authorization-work-in-app-service"></a>Nasıl kimlik doğrulaması ve yetkilendirme App Service içinde çalışır?

Kimlik doğrulaması ve yetkilendirme App Service içinde ayrıntılı belgeler için bkz: belgelerine yönelik çeşitli sağlayıcı oturum açma işlemleri tanımlayın:
* [Azure Active Directory](app-service-mobile-how-to-configure-active-directory-authentication.md)
* [Facebook](app-service-mobile-how-to-configure-facebook-authentication.md)
* [Google](app-service-mobile-how-to-configure-google-authentication.md)
* [Microsoft Hesabı](app-service-mobile-how-to-configure-microsoft-authentication.md)
* [Twitter](app-service-mobile-how-to-configure-twitter-authentication.md)

## <a name="how-do-i-redirect-the-default-azurewebsitesnet-domain-to-my-azure-web-apps-custom-domain"></a>Ne t yeniden yönlendirme varsayılan *. azurewebsites.net etki alanım Azure web uygulamanızın özel etki alanı için?

Azure, varsayılan Web Apps kullanarak yeni bir Web sitesi oluşturduğunuzda *sitename*. azurewebsites.net etki alanı, sitenize atanır. Bir özel ana bilgisayar adı sitenize ekleyin ve varsayılan erişebilmeleri için kullanıcılara istemiyorsanız *. azurewebsites.net etki alanı, varsayılan URL yönlendirebilirsiniz. Özel etki alanınızı, Web sitenizin varsayılan etki alanından tüm trafik yönlendirme hakkında bilgi edinmek için [Azure web uygulamalarında özel etki alanınız için varsayılan etki alanını yeniden yönlendirme](http://www.zainrizvi.io/2016/04/07/block-default-azure-websites-domain/).

## <a name="how-do-i-determine-which-version-of-net-version-is-installed-in-app-service"></a>Hangi sürümünün nasıl belirleyebilirim .NET App Service'te sürümü yüklenir?

App Service içinde yüklü Microsoft .NET sürümünü bulmak için en hızlı yolu, Kudu konsol kullanmaktır. Portalı veya uygulama hizmeti uygulamanızı URL'sini kullanarak Kudu Konsolu erişebilir. Ayrıntılı yönergeler için bkz: [yüklü olan .NET sürümünü App Service'te](https://blogs.msdn.microsoft.com/waws/2016/11/02/how-to-determine-the-installed-net-version-in-azure-app-services/).

## <a name="why-isnt-autoscale-working-as-expected"></a>Neden otomatik ölçeklendirme beklendiği gibi çalışmıyor?

Azure otomatik ölçeklendirme ölçeklendirilmiş değiştirilmediğini veya, beklendiği gibi web uygulama örneğini ölçeği, size, özellikle "dalgalanma." nedeniyle sonsuz bir döngüde önlemek için ölçeklendirme değil seçeneğini belirledik bir senaryo içine çalışmıyor olabilir Bu, genellikle genişleme ve ölçek bileşenini eşiklerin arasında yeterli bir kenar boşluğu olmadığında gerçekleşir. "Dalgalanma" önlemek için ve diğer otomatik ölçeklendirme en iyi uygulamalar hakkında okumak için öğrenmek için bkz: [otomatik ölçeklendirme en iyi yöntemler](../monitoring-and-diagnostics/insights-autoscale-best-practices.md#autoscale-best-practices).

## <a name="why-does-autoscale-sometimes-scale-only-partially"></a>Neden otomatik bazen yalnızca kısmen ölçeklendirme?

Otomatik ölçeklendirme ölçüm önceden yapılandırılmış sınırları aştığında tetiklenir. Bazı durumlarda, kapasite yalnızca kısmen beklediğiniz karşılaştırıldığında doldurulduğunu fark edebilirsiniz. İstediğiniz örneklerinin sayısını olmadığında bu durum oluşabilir. Bu senaryoda, otomatik ölçeklendirme kısmen kullanılabilir örneklerin sayısı ile doldurur. Otomatik ölçeklendirme, daha sonra daha fazla kapasite almak için yeniden dengeleyin mantığı çalıştırır. Kalan örnekleri ayırır. Bu işlem birkaç dakika sürebileceğini unutmayın.

Birkaç dakika sonra örnekleri beklenen sayıda görmüyorsanız, kısmi Dolum sınırları içinde ölçümleri getirmek için yeterli olduğundan olabilir. Ya da daha düşük ölçümleri sınırına ulaştığından otomatik ölçeklendirme ölçeklendirilmiş.

Bu koşulların hiçbiri geçerli ve sorun devam ederse, destek isteği gönderin.

## <a name="how-do-i-turn-on-http-compression-for-my-content"></a>İçeriğim için nasıl HTTP sıkıştırmasını etkinleştirmek?

Sıkıştırma hem statik ve dinamik içerik türleri için etkinleştirmek için uygulama düzeyinde web.config dosyasına aşağıdaki kodu ekleyin:

```
<system.webServer>
<urlCompression doStaticCompression="true" doDynamicCompression="true" />
< /system.webServer>
```

Sıkıştırılacak istediğiniz belirli dinamik ve statik MIME türlerini de belirtebilirsiniz. Bizim Forumu sorusuna yanıt daha fazla bilgi için bkz: [httpCompression ayarları basit bir Azure Web sitesinde](https://social.msdn.microsoft.com/Forums/azure/890b6d25-f7dd-4272-8970-da7798bcf25d/httpcompression-settings-on-a-simple-azure-website?forum=windowsazurewebsitespreview).

## <a name="how-do-i-migrate-from-an-on-premises-environment-to-app-service"></a>Uygulama hizmeti için nasıl bir şirket içi ortamından geçişini?

Siteleri Windows ve Linux web sunucularından App Service'e geçirme için Azure App Service geçiş Yardımcısı'nı kullanabilirsiniz. Geçiş Aracı gerektiğinde Azure web uygulamaları ve veritabanları oluşturur ve içeriği yayımlar. Daha fazla bilgi için bkz: [Azure App Service geçiş Yardımcısı](https://www.movemetothecloud.net/).
