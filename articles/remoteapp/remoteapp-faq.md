<properties 
    pageTitle="Azure RemoteApp hakkında SSS | Microsoft Azure" 
    description="Azure RemoteApp hakkında en sık sorulan soruların yanıtlarını öğrenin." 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" 
    editor=""/>

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="04/08/2016" 
    ms.author="elizapo"/>

# Azure RemoteApp hakkında SSS
Azure RemoteApp hakkında aşağıdaki soruları duymuştuk. Başka var mı? [RemoteApp forumlarını](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureRemoteApp) ziyaret edin ve bilmeniz gerekenleri öğrenin veya aşağıya bir yorum bırakın.

## Azure RemoteApp nedir? ##


- **Azure RemoteApp nedir?** Azure hizmetindeki RemoteApp birçok farklı kullanıcı cihazından uygulamalara güvenli uzaktan erişim sağlamanıza yardımcı olur. [Azure RemoteApp](remoteapp-whatis.md) hakkında daha fazla bilgi okuyun
- **Dağıtım seçenekleri nelerdir?** İki tür RemoteApp koleksiyonu vardır: bulut ve karma. Aynı, etki alanına katılımda olduğu gibi, burada da size hangisinin gerektiği bir dizi etkene bağlıdır. Bu kararların tümü hakkında [burada](remoteapp-collections.md) konuşacağız.

## Azure RemoteApp kullanma hakkında hızlı ipuçları ##
- **Bağlantım ne kadar kesik kalacak? Bana önyükleme vermenizden önce ne kadar boşta olabilirim?** 4 saat. Siz veya kullanıcılarınız 4 saat işlem yapmadıysanız, Azure RemoteApp oturumunuz otomatik olarak kapanır. [Azure Aboneliği ve Hizmet Sınırları, Kotaları ve Kısıtlamaları](../azure-subscription-service-limits.md) makalesinde diğer varsayılan ayarları inceleyin.
- **Bu hizmeti ücretsiz deneyebilir miyim?** Evet. 30 günlük ücretsiz bir deneme sürümü vardır. Deneme süresi bittikten sonra, ücretli bir hesaba geçebilir (üretimde kullanabildiğiniz) ya da hizmeti kullanmayı durdurabilirsiniz. Ücretsiz deneme sürümünüzü [portal.azure.com](http://portal.azure.com) adresine giderek başlatın - yeni bir RemoteApp örneğini oluşturun. Ücretsiz deneme sürümüyle her örnekte 10 kullanıcı olmak üzere 2 RemoteApp örneği oluşturabilirsiniz. Bu deneme sürümünün yalnızca 30 gün ömrü olduğunu unutmayın.
## Azure RemoteApp aboneliği ayrıntıları ##

- **Hizmet sınırları nelerdir?** Azure RemoteApp varsayılan ayarları ve hizmet sınırları hakkında bilgileri, [Azure Aboneliği ve Hizmet Sınırları, Kotaları ve Kısıtlamaları](../azure-subscription-service-limits.md) makalesinden edinebilirsiniz. Daha fazla sorunuz olup olmadığını biz de bilelim.
- **Kaç kullanıcım olmalıdır?** En az 20 kullanıcı vardır. Çok daha belirgin olması için bir kez daha - EN AZ 20. Size 20 kullanıcı için fatura edilecek. 
- **RemoteApp maliyeti ne kadar?** [Azure RemoteApp Fiyatlandırma Ayrıntıları](https://azure.microsoft.com/pricing/details/remoteapp/) makalesini inceleyin.
- **Bir tür koleksiyonun maliyeti bir başkasına göre daha mı yüksek?** Evet, koleksiyon gereksinimlerinize bağlı olarak daha yüksek olabilir. Karma koleksiyon için Azure RemoteApp’ten şirket içi ağınıza bağlantı gerekir. Mevcut bir VNET/Hızlı Rota kullanıyorsanız ek bir maliyet yoktur. Ancak, yeni Azure VNET ve bir ağ geçidi veya Hızlı Rota kullanıyorsanız, [VPN ağ geçidi](https://azure.microsoft.com/pricing/details/vpn-gateway) veya [Hızlı Rota](../../../pricing/details/expressroute/) ücretlendirilir. Bu maliyet (bağlantılarda ayrıntılı), aylık Azure RemoteApp maliyetinin en üstündedir.

## Koleksiyonlar - ne desteklenir, hangisini kullanmalısınız ve diğerleri
- **Özel iş kolu (LOB) uygulamaları destekleniyor mu?** Evet. Azure RemoteApp’te özel bir uygulama kullanmak için [özel şablon görüntüsü](remoteapp-create-custom-image.md) oluşturun ve bunu RemoteApp koleksiyonuna yükleyin.
- **Özel LOB Uygulamam Azure RemoteApp’te çalışacak mı?** Bunu anlamanın en iyi yolu test etmektir. [RD Uyumluluk Merkezi](http://www.rdcompatibility.com/compatibility/default.aspx) sayfasını inceleyin.
- **Kuruluşum için hangi dağıtım yöntemi (bulut veya karma) en iyisidir?** Karma koleksiyonlar, çoklu oturum açma (SSO) ve güvenli şirket içi ağ bağlantısıyla tam tümleştirme isterseniz en eksiksiz deneyimi sağlar. Bulut koleksiyonları, birden çok kimlik doğrulaması yöntemi kullanılarak dağıtımınızın yalıtması için çevik ve kolay bir yol sağlar. [Dağıtım seçenekleri](remoteapp-whatis.md) hakkında daha fazlasını okuyun.
- **İster şirket içi. ister Azure üzerinde olsun SQL veya başka bir veritabanımız vardır. Hangi tür dağıtımı kullanmalıyız?** SQL veya arka uç veritabanınızın nerede olduğuna bağlıdır. Veritabanı özel bir ağdaysa karma koleksiyonunu kullanın. Veritabanı İnternet'e açıksa ve istemci bağlantılarının buna bağlanmasına izin veriyorsa bulut koleksiyonunu kullanabilirsiniz.
- **Sürücü eşleme, USB ve seri bağlantı noktası, pano paylaşımı ve yazıcı yeniden yönlendirme hakkında neler diyorsunuz?** Tüm bu özellikler Azure RemoteApp’te desteklenir. Pano paylaşımı ve yazıcı yeniden yönlendirme varsayılan olarak etkindir. Yeniden yönlendirme hakkında bilgileri [buradan](remoteapp-redirection.md) edinebilirsiniz. 


## Şablon görüntüleri
- **RemoteApp koleksiyonum için bulut veya mevcut sanal makineyi şablon olarak kullanabilir miyim?** Evet! Azure VM temelinde görüntü oluşturabilirsiniz, aboneliğinizde yer alan görüntülerden birini kullanın veya özel bir görüntü oluşturun. [RemoteApp görüntü seçenekleri](remoteapp-imageoptions.md)’ni inceleyin.


## Ağ seçenekleri
- **Karma koleksiyona VNET gerekir. Mevcut VNET’imizi kullanabilir miyim?** Mevcut VNET Azure VNET'se kullanabilirsiniz. Daha fazla bilgi için [karma koleksiyon yönergelerinde](remoteapp-create-hybrid-deployment.md) "1. Adım: Sanal ağınızı kurun" konusuna bakın.
- **VNET’i bulut koleksiyonuyla kullanabilir miyim?** Aslında kullanabilirsiniz. Daha fazla bilgi için [Bulut koleksiyonu oluşturma](remoteapp-create-cloud-deployment.md) konusunu, özellikle de 1. Adım’ı inceleyin.

## Kimlik doğrulaması seçenekleri



- **Kimlik doğrulaması hakkında ne denebilir? Hangi yöntemler destekleniyor?** Bulut koleksiyonu, Microsoft hesaplarını ve Azure Active Directory hesaplarını destekler; bunlar aynı zamanda Office 365 hesaplarıdır. Karma koleksiyon, yalnızca Windows Server Active Directory dağıtımdan eşitlenmiş Azure Active Directory hesaplarını destekler ([Azure Active Directory Eşitleme](http://blogs.technet.com/b/ad/archive/2014/09/16/azure-active-directory-sync-is-now-ga.aspx) gibi bir araç kullanılarak); özellikle, Parola Eşitleme seçeneğiyle ya da yapılandırılan Active Directory Federasyon Hizmetleri (AD FS) federasyonuyla eşitlenir. [Multi-Factor Authentication (MFA)](https://azure.microsoft.com/services/multi-factor-authentication/) öğesini de yapılandırabilirsiniz.

>[AZURE.NOTE]Azure Active Directory kullanıcıları, aboneliğinizle ilişkili kiracıya ait olmalıdır. (Aboneliğinizi, portalın **Ayarlar** sekmesinde görüntüleyebilir ve değiştirebilirsiniz. Daha fazla bilgi için bkz. [RemoteApp tarafından kullanılan Azure Active Directory kiracısını değiştirme](remoteapp-changetenant.md).)

- **Azure Active Directory hesabımın erişimini neden veremiyorum?** Azure Active Directory kullanıcıları, aboneliğinizle ilişkili dizine ait olmalıdır. Portalın Ayarlar sekmesinde bu dizini görüntüleyebilir veya değiştirebilirsiniz. Daha fazla bilgi için bkz. [RemoteApp tarafından kullanılan Azure Active Directory kiracısını değiştirme](remoteapp-changetenant.md).

## İstemciler - Azure RemoteApp’e erişmek için hangi cihazı kullanabilirim?
[Azure RemoteApp’te uygulamalarınıza erişme](remoteapp-clients.md) konusunda farklı istemcilerin yüklenmesiyle ilgili adımların da yer aldığı yararlı istemci bilgilerini bulabilirsiniz.

- **İstemci uygulamaları hangi cihazları ve işletim sistemlerini destekler?**
Önce, bilgisayarlar ve tabletler: 
    - Windows 10 (istemci önizleme)
    - Windows 8.1 ve Windows 8
    - Windows 7 Service Pack 1
    - Mac OS X
    - Windows RT
    - Android tabletler
    - iPad’ler ve telefonlar:
    - iPhone
    - Android Phone
    - Windows Phone
 
    RemoteApp istemcisini hemen [indirin](https://www.remoteapp.windowsazure.com/ClientDownload/AllClients.aspx).
- **Azure RemoteApp İnce İstemcileri destekliyor mu?** Evet, aşağıdakiler desteklenen Windows Embedded ince istemcilerdir:
    - Windows Embedded Standard 7
    - Windows Embedded 8 Standard
    - Windows Embedded 8.1 Industry Pro
    - Windows 10 IoT Enterprise

- **Uzak Masaüstü Oturumu Ana Bilgisayarı (RDSH) için Windows Server'ın hangi sürümü destekleniyor?** Windows Server 2012 R2.

##Destek ve geri bildirim


- **RemoteApp için destek planı nedir?** Faturalama ve abonelik yönetimi desteği ücretsiz olarak sağlanır. Teknik desteğe [Azure hizmet planlarından](https://azure.microsoft.com/support/plans/) ulaşabilirsiniz. [Azure tartışma forumumuzdan](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=AzureRemoteApp) da ücretsiz topluluk desteği alabilirsiniz. 
- **Geri bildirimi nasıl gönderirim?** [Geri bildirim forumunu](https://feedback.azure.com/forums/247748-azure-remoteapp/) ziyaret edin.
- **Azure RemoteApp hakkında daha fazla bilgi için kiminle görüşeyim?** Soruların gönderildiği muhteşem bir yer olan [tartışma forumuna](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=AzureRemoteApp) ek olarak, RemoteApp ile ilgili her şeyden söz ettiğimiz haftalık [Uzmanlara sorun web seminerimize](https://azureinfo.microsoft.com/US-Azure-WBNR-FY15-11Nov-AzureRemoteAppAskTheExperts-Registration-Page.html) katılın.
- **RemoteApp belgeleri hakkında neler diyeceksiniz?** Bunu sorduğunuza çok memnun olduk. Portalın yardım bölümündeki (portalın herhangi bir sayfasında **?** işaretinin tıklanması yeterlidir) yardım içeriğine ek olarak, aşağıdaki makaleler de RemoteApp hakkında bilgi almanız için kullanılabilir:
    - **Başlarken:**
        - [RemoteApp nedir?](remoteapp-whatis.md)
        - [RemoteApp şablon görüntülerinde neler var?](remoteapp-images.md)
        - [Lisans nasıl çalışır?](remoteapp-licensing.md)
        - [RemoteApp ve Office birlikte nasıl çalışır?](remoteapp-o365.md)
        - [RemoteApp’te yeniden yönlendirme nasıl işler](remoteapp-redirection.md)?
    - **Dağıt:**
        - [Özel şablon görüntüsü oluşturma](remoteapp-create-custom-image.md)
        - [Karma koleksiyon oluşturma](remoteapp-create-hybrid-deployment.md)
        - [Bulut koleksiyonu oluşturma](remoteapp-create-cloud-deployment.md)
        - [RemoteApp için Azure Active Directory’yi yapılandırma](remoteapp-ad.md)
        - [RemoteApp’te uygulama yayımlama](remoteapp-publish.md)
    - **Yönet:**
        - [Kullanıcı ekle](remoteapp-user.md)
        - [RemoteApp’in yapılandırılması ve kullanılması için en iyi yöntemler](remoteapp-bestpractices.md)  

    Videolar! RemoteApp hakkında sayısız videomuz da vardır. Bunlardan bazıları giriş ([Azure RemoteApp’e giriş](https://azure.microsoft.com/documentation/videos/cloud-cover-ep-150-azure-remote-app-with-thomas-willingham-and-nihar-namjoshi/)) sağlarken, diğerleri de size dağıtımda ([Bulut dağıtımı](https://www.youtube.com/watch?v=3NAv2iwZtGc&feature=youtu.be) ve [Karma dağıtım](https://www.youtube.com/watch?v=GCIMxPUvg0c&feature=youtu.be)) yol göstermektedir. Bunları inceleyin!

 
### Yardımımıza katkıda bulunun 
Bu makaleyi derecelendirmenin ve aşağıda yorum yapmamanın yanı sıra makalede değişiklik de yapabileceğinizi biliyor muydunuz? Eksik bir şeyler mi var? Yanlış bir şeyler mi var? Kafa karıştırıcı bir şeyler mi yazdım? Değişiklik yapmak için yukarı doğru ilerleyin ve **GitHub üzerinde düzenle**’ye tıklayın; bu değişiklikler incelenmek üzere bize gönderilir ve kabul edildikten sonra değişiklikleriniz ve iyileştirmeleriniz burada görünür.



<!----HONumber=Jun16_HO2-->


