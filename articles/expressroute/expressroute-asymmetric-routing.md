<properties
   pageTitle="Asimetrik Yönlendirme | Microsoft Azure"
   description="Bu makalede, bir müşterinin sahip olduğu ve bir hedefin birden çok bağlantısını içeren bir ağda asimetrik yönlendirme konusunda karşılaşabileceği sorunlarla ilgili yol gösterilmektedir."
   documentationCenter="na"
   services="expressroute"
   authors="osamazia"
   manager="carmonm"
   editor=""/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/23/2016"
   ms.author="osamazia"/>

# Birden çok ağ yoluyla Asimetrik Yönlendirme

Bu makale, kaynak ile hedef arasında birden çok yol varsa iletme ve döndürme trafiğinin nasıl farklı rotalar izleyebileceği açıklanmıştır.

Asimetrik yönlendirmeyi anlayabilmemiz için iki konsepti anlamamız gerekir. Bunlardan biri, birden çok ağ yolunun etkisidir. Diğeriyse güvenlik duvarları gibi durum bilgisi tutan cihazların davranışıdır. Bu cihazlara durum bilgisi olan cihazlar denir. Bu iki etmenin birleşimi, durum bilgisi olan bir cihaz tarafından trafiğin kendisi üzerinden başladığını görmediği için trafiğin bırakıldığı senaryolar oluşturur.

## Birden Çok Ağ Yolu

Bir kurumsal ağın İnternet hizmet sağlayıcısı aracılığıyla sunulan tek bir İnternet bağlantısı varsa, İnternet’e yönelik ve İnternet’ten gelen tüm trafik aynı yolu izler. Şirketler çoğunlukla ağ çalışma süresini artırmak için yedek yollar olarak birden çok devre satın alır. Böyle durumlarda, ağ dışına, yani İnternet’e giden trafik bir bağlantı üzerinden geçerken dönen trafik farklı bir bağlantı üzerinden geçer. Ters trafiğin ilk akıştan farklı bir yol izlediği bu olgu Asimetrik Yönlendirme olarak bilinir.

![Yönlendirme3](./media/expressroute-asymmetric-routing/AsymmetricRouting3.png)

Bir önceki açıklama İnternet için olsa da diğer çoklu yol birleşimleri için de geçerlidir. Örnek olarak bir İnternet yolu ve aynı hedefe yönelik özel bir yol, aynı hedefe yönelik birden çok özel yol, vs. verilebilir. 

Kaynakla hedef arasındaki her yol, hedefe ulaşmak için en iyi yol hesaplamasına bağlı olarak bir hedefe ulaşmanın en iyi yolunu hesaplar. Olası en iyi yolun belirlenmesi iki ana etmene bağlıdır.

1.  Dış ağlar arası yönlendirme, BGP olarak bilinen Sınır Ağ Geçidi Protokolü adlı yönlendirme protokolüne bağlıdır. BGP komşularından tanıtımları toplar ve hedefe yönelik en iyi yolu belirlemek için bunları birkaç adımdan geçirip en iyi yolu yönlendirme tablosuna yükler.
2.  Bir yolla ilişkili alt ağ maskesinin uzunluğu. Aynı IP adresi için birden çok tanıtım olmasına rağmen farklı alt ağ maskeleri alındıysa, alt ağ maskesi daha uzun olan tanıtım daha belirli bir yol olarak görüldüğü için bu tanıtım seçilir.

## Durum Bilgisi Olan Cihazlar

Yönlendiriciler yönlendirme amacıyla paketin IP üst bilgisine bakar. Bununla birlikte, pakette daha da derinlere bakan cihazlar vardır. Bu cihazlar genellikle Layer4 (TCP/UDP), hatta Layer7 (Uygulama Katmanı) üst bilgileri kullanır. Bu cihazlar ya güvenlik cihazları ya da bant genişliği iyileştirme cihazlarıdır. Durum bilgisi olan cihazlar için sık kullanılan bir örnek güvenlik duvarıdır. Güvenlik duvarı; protokol, TCP/UDP bağlantı noktası, URL üst bilgileri gibi çeşitli alanlara bağlı olarak bir paketin arabirimleri üzerinden geçmesine izin verir veya bunu reddeder. Bu paket denetimi nedeniyle cihaz üzerinde büyük bir işlem yükü oluşur. Güvenlik duvarı, performansı artırmak için bir akıştaki ilk paketi denetler. Pakete izin verilirse akış bilgileri durum tablosunda tutulur. İlk karar temel alınarak bu akışla ilgili sonraki tüm paketlere izin verilir. Dolayısıyla, var olan bir akışın bir parçası olan bir paket güvenlik duvarına ulaştığında ve güvenlik duvarının paketle ilgili önceki durum bilgisi olmadığında güvenlik duvarı paketi bırakır.

## ExpressRoute ile asimetrik yönlendirme

ExpressRoute üzerinden Microsoft’a bağlandığınızda, ağınızda aşağıdaki değişiklikler gerçekleşir.

1.  Birden fazla Microsoft bağlantınız var. Var olan İnternet bağlantınız bir bağlantı, ExpressRoute üzerinden kurulan bağlantı ise başka bir bağlantıdır. Microsoft’a yönelik bazı trafikler İnternet üzerinden gidip ExpressRoute üzerinden dönebilir ya da tam tersi gerçekleşebilir.
2.  ExpressRoute üzerinden daha belirli IP adresleri alırsınız. Bu nedenle, ExpressRoute aracılığıyla sunulan hizmetler için ağınızdan Microsoft’a giden trafik her zaman ExpressRoute seçeneğini tercih eder. 

Yukarıdaki iki durumun etkisini anlamak için bazı senaryolara bakalım. İnternet’e yönelik tek bir devreniz olduğunu ve tüm Microsoft hizmetlerini İnternet üzerinden tükettiğinizi varsayalım. Ağınızdan Microsoft’a giden ve geri gelen trafik aynı İnternet bağlantısını kullanır ve güvenlik duvarından geçer. Güvenlik duvarı ilk paketi gördüğünde akışı kaydeder ve akış durum tablosunda var olduğundan dönüş paketlerine izin verilir.

![Yönlendirme1](./media/expressroute-asymmetric-routing/AsymmetricRouting1.png)


Şimdi ExpressRoute hizmetini açıyorsunuz ve Microsoft tarafından sunulan tüm hizmetleri ExpressRoute üzerinden tüketiyorsunuz ve Microsoft tarafından sunulan diğer tüm hizmetler İnternet üzerinden tüketiliyor diyelim. ExpressRoute’a bağlanan ucunuzda ayrı bir güvenlik duvarı dağıtıyorsunuz. Microsoft, belirli hizmetler için ExpressRoute üzerinden ağınıza daha belirli ön ekler tanıtır. Yönlendirme altyapınız bu ön ekler için tercih edilen yol olarak ExpressRoute’u seçer. Genel IP adreslerinizi ExpressRoute üzerinden Microsoft’a tanıtmıyorsanız Microsoft, Genel IP adreslerinizle İnternet üzerinden iletişim kurar. Bu nedenle, ağınızdan Microsoft’a gönderilen ileri yönlü trafik ExpressRoute’u kullanırken, Microsoft’tan dönen trafik İnternet’i kullanır. Uçtaki güvenlik duvarı, durum tablosunda bulunmayan bir akış için bir yanıt paketi gördüğünde dönüş trafiğini bırakır. 

ExpressRoute ve İnternet için aynı NAT havuzunu kullanmayı tercih ederseniz, ağınızdaki özel IP adreslerindeki istemcilerle de benzer sorunlar yaşarsınız. Windows Update gibi hizmetlerin IP adresleri ExpressRoute üzerinden tanıtılmadığından, bu hizmetlere yönelik istekler İnternet üzerinden gider. Ancak, dönüş trafiği ExpressRoute üzerinden geri gelir. Microsoft İnternet ve ExpressRoute’tan aynı alt ağ maskesine sahip bir IP adresi alırsa ExpressRoute’u İnternet’e tercih eder. ExpressRoute’a yönelik ağ ucunuzdaki bir güvenlik duvarının ya da durum bilgisi olan başka bir cihazın akışla ilgili daha önceden bilgisi yoksa, o akışa ait olan paketler bırakılır. 

## Asimetrik Yönlendirme çözümleri

Asimetrik Yönlendirme sorununu çözmenin iki ana yolu vardır. Biri yönlendirme, diğeriyse kaynak tabanlı NAT (SNAT) aracılığıyladır. 

1. Yönlendirme 

    - Genel IP adreslerinizin uygun WAN bağlantılarına tanıtıldığından emin olmanız gerekir. Örneğin, kimlik doğrulama trafiği için İnternet’i, posta trafiğiniz içinse ExpressRoute’u kullanmak istiyorsanız, ExpressRoute’ta ADFS genel IP adreslerinizi tanıtmamanız gerekir. Benzer şekilde, şirket içi ADFS sunucusu ExpressRoute üzerinden alınan IP adreslerine açılmamalıdır. ExpressRoute üzerinden alınan rotalar daha belirli olduğundan, bunlar Microsoft’a yönelik kimlik doğrulama trafiği için ExpressRoute’u tercih edilen yol haline getirir ve asimetrik yönlendirmeye yol açar.

    - Kimlik doğrulaması için ExpressRoute’u kullanmak istiyorsanız ADFS genel IP adreslerini ExpressRoute üzerinden NAT olmadan tanıttığınızdan emin olun. Bu sayede, Microsoft’tan çıkıp şirket içi ADFS sunucusuna giden trafik ExpressRoute üzerinden giderken, müşteriden gelip Microsoft’a giden dönüş trafiği tercih edilen rota olarak İnternet’i değil ExpressRoute’u kullanır. 

2. Kaynak tabanlı NAT

    Asimetrik Yönlendirme sorunlarını çözmenin bir başka yolu da Kaynak NAT (SNAT) üzerindendir. Örneğin, bir iletişimde İnternet’i kullanmak istediğiniz için şirket içi SMTP sunucusunun genel IP adresini ExpressRoute üzerinden tanıtmadığınızı varsayalım. Şirket içi SMTP sunucunuza yönelik olarak Microsoft’tan çıkan bir istek İnternet’ten gönderilir. Gelen isteğe kaynak NAT uygulayarak bir dahili IP adresine yönlendirirsiniz. SMTP sunucusundan çıkan ters yönlü trafik, ExpressRoute yerine uç güvenlik duvarına (NAT uygulamak için kullanılan) gider. Bu sayede, dönüş trafiği İnternet üzerinden gönderilir. 


![Yönlendirme2](./media/expressroute-asymmetric-routing/AsymmetricRouting2.png)

## Asimetrik Yönlendirmenin Algılanması

Traceroute, trafiğin beklenen yoldan gittiğinden emin olmanın en iyi yoludur. Şirket içi SMTP sunucunuzdan Microsoft’a giden trafiğin İnternet yolunu tercih etmesini bekliyorsanız, SMTP sunucusu ile Office 365 arasındaki yolu izleyin. Sonuç, ağınızdan çıkan trafiğin gerçekten de ExpressRoute’a değil İnternet’e gittiğini doğrular. 





<!--HONumber=Aug16_HO4-->


