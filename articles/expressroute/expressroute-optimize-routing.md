<properties
   pageTitle="ExpressRoute yönlendirmeyi en iyi duruma getirme | Microsoft Azure"
   description="Bu sayfa, bir müşteri Microsoft ve müşterinin şirket ağı arasında bağlanan birden fazla ExpressRoute bağlantı hattına sahip olduğunda yönlendirmenin nasıl iyileştirileceği hakkında ayrıntılı bilgi sağlar."
   documentationCenter="na"
   services="expressroute"
   authors="charwen"
   manager="carmonm"
   editor=""/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="06/03/2016"
   ms.author="charwen"/>

# ExpressRoute Yönlendirmeyi En iyi Duruma Getirme
Birden çok ExpressRoute bağlantı hattına sahip olduğunuzda, Microsoft'a bağlanmak için birden fazla yolunuz vardır. Sonuç olarak, yetersiz yönlendirme olabilir, diğer bir deyişle, trafiğinizin Microsoft’a ulaşması ve Microsoft’un ağınıza ulaşması daha uzun bir yol alabilir. Ne kadar uzun ağ yolu, o kadar yüksek gecikme. Gecikmenin uygulama performansı ve kullanıcı deneyimi üzerinde doğrudan etkisi vardır. Bu makale, bu sorunu gösterir ve standart yönlendirme teknolojilerini kullanarak yönlendirmenin nasıl iyileştirileceğini açıklar.

## Yetersiz yönlendirme örneği 1
Bir örnekle yönlendirme problemine daha yakından bir bakalım. Biri Los Angeles ve diğeri New York’ta olmak üzere ABD’de iki ofisiniz olduğunu düşünün. Ofisleriniz, kendi omurga ağınız veya hizmet sağlayıcınızın IP VPN’i olabilen bir Geniş Alan Ağı (WAN) üzerine bağlıdır. Ayrıca, WAN’da bağlı biri ABD Batı ve diğeri ABD Doğu’da olmak üzere iki ExpressRoute bağlantı hattınız vardır. Belli ki, Microsoft ağına bağlanmak için iki yolunuz vardır. Şimdi, hem ABD Batı hem de ABD Doğu’da Azure dağıtımına (örneğin, Azure App Service) sahip olduğunuzu düşünün. Hizmet yöneticiniz en iyi deneyimler için her ofisteki kullanıcıların yakınlardaki Azure hizmetlerine erişimini tanıttığından, amacınız Los Angeles’taki kullanıcılarınızı Azure ABD Batı’ya ve New York’taki kullanıcılarınızı Azure ABD Doğu’ya bağlamaktır. Ne yazık ki, doğu yakası kullanıcıları için plan yolunda giderken batı yakası kullanıcıları için yolunda değildir. Sorunun nedeni aşağıda verilmiştir. Her bir ExpressRoute bağlantı hattında, hem Azure ABD Doğu (23.100.0.0/16) hem de Azure ABD Batı’daki (13.100.0.0/16) önekini size tanıtırız. Hangi önekin hangi bölgeden olduğunu bilmiyorsanız, farklı şekilde ele almanız mümkün değildir. WAN ağınız her iki önekin de ABD Batı’dan daha çok ABD Doğu’ya yakın olduğunu düşünebilir ve bu nedenle, iki ofis kullanıcılarını da ABD Doğu’daki ExpressRoute bağlantı hattına yönlendirebilir. Sonunda, Los Angeles ofisinde birçok mutsuz kullanıcınız olacaktır.

![](./media/expressroute-optimize-routing/expressroute-case1-problem.png)

### Çözüm: BGP Toplulukları’nı kullanın
Her iki ofis kullanıcıları için yönlendirmeyi en iyi hale getirmek için, hangi önekin Azure Batı ABD ve Azure Doğu ABD olduğunu bilmeniz gerekir. Bu bilgileri [BGP Topluluğu değerlerini](expressroute-routing.md) kullanarak kodlarız. Her bir Azure bölgesine benzersiz bir BGP Topluluğu değeri atadık, örneğin Doğu ABD için "12076:51004", Batı ABD için "12076:51006". Artık hangi önekin hangi Azure bölgesinden olduğunu bildiğinize göre, tercih edilmesi gereken ExpressRoute bağlantı hattını yapılandırabilirsiniz. Yönlendirme bilgilerinin değişimi için BGP kullandığımızdan, yönlendirmeyi etkilemek için BGP'nin Yerel Tercihini kullanabilirsiniz. Bizim örneğimizde, ABD Batı’daki 13.100.0.0/16 için ABD Doğu’dan daha yüksek bir yerel tercih değeri atayabilir ve benzer şekilde, ABD Doğu’daki 23.100.0.0/16 için ABD Batı’dan daha yüksek bir yerel tercih değeri atayabilirsiniz. Bu yapılandırma her iki Microsoft yolunun da kullanılabilir olduğunda, New York’taki kullanıcılarınız ABD Doğu’dan Azure ABD Doğu’ya ExpressRoute bağlantı hattını kullanırken Los Angeles’taki kullanıcılarınızın Azure ABD Batı’ya bağlanması için ABD Batı’daki ExpressRoute bağlantı hattının kullanıldığından emin olacaktır. Her iki tarafta da yönlendirme en iyi duruma getirilmiştir. 

![](./media/expressroute-optimize-routing/expressroute-case1-solution.png)

## Yetersiz yönlendirme örneği 2
Burada Microsoft bağlantılarının ağınıza ulaşmasına için daha uzun bir yol aldığı başka bir örnek gösterilmektedir. Bu durumda, [karma bir ortamda](https://technet.microsoft.com/library/jj200581%28v=exchg.150%29.aspx) şirket içi Exchange sunucularını ve Exchange Online kullanın. Ofisleriniz bir WAN’a bağlıdır. İki ExpressRoute bağlantı hattı aracılığıyla her iki ofisinizdeki şirket için sunucuların öneklerini Microsoft’a tanıtırsınız. Exchange Online, posta kutusu geçişi gibi durumlarda şirket içi sunuculara bağlantılar başlatır. Ne yazık ki, Los Angeles ofisinizin bağlantısı batı yakasına dönerken tüm kıtadan geçmeden önce ABD Doğu’daki ExpressRoute bağlantı hattına yönlendirilir. Sorunun nedeni birinciye benzemektedir. Herhangi bir ipucu olmadan, Microsoft ağı hangi müşteri önekinin ABD Doğu'ya veya ABD Batı’ya yakın olduğunu bildiremez. Los Angeles’taki ofisiniz için yanlış yolu seçmiş olabilirsiniz.

![](./media/expressroute-optimize-routing/expressroute-case2-problem.png)

### Çözüm: AS YOLU eklenmesini kullanın
Bu sorunun iki çözümü vardır. Birinci yol Los Angeles ofisiniz için ABD Batı’daki ExpressRoute bağlantı hattındaki 177.2.0.0/31 şirket içi öneki ve New York ofisiniz için ABD Doğu’daki ExpressRoute bağlantı hattındaki 177.2.0.2/31 şirket içi önekini tanıtmanızdır. Sonuç olarak, Microsoft’un ofislerinizin her birine bağlanması için yalnızca bir yol vardır. Belirsizlik olmaz ve yönlendirme en iyi duruma getirilir. Bu tasarımla, yük devretme stratejinizi düşünmeniz gerekir. ExpressRoute aracılığıyla Microsoft yolunun bozuk olması durumunda, Exchange Online’ın yine de şirket içi sunucularına bağlandığından emin olmanız gerekir. 

İkinci çözüm, her iki ExpressRoute bağlantı hattında iki öneki de tanıtmaya devam etmeniz ve buna ek olarak, hangi önekin hangi ofisinize daha yakın olduğu ile ilgili bize ipucu vermenizdir. BGP AS Yolu eklenmesini desteklediğimizden, yönlendirmeyi etkilemek için önekiniz için AS Yolu’nu yapılandırabilirsiniz. Bu örnekte, bu önek (ağımız bu önek yolunun batıdan daha kısa olacağını düşüneceğinden) için hedeflenen trafikte ABD Batı’daki ExpressRoute bağlantı hattını tercih edeceğimizden, ABD Doğu’daki 172.2.0.0/31 AS YOLU’nu uzatabilirsiniz. Benzer şekilde, ABD Doğu ExpressRoute bağlantı hattını tercih edeceğimiz için, ABD Batı’daki 172.2.0.2/31 için AS YOLU’nu uzatabilirsiniz. Her iki ofis için de yönlendirme en iyi duruma getirilmiştir. Bu tasarımla, bir ExpressRoute bağlantı hattı bozuk ise, Exchange Online başka bir ExpressRoute bağlantı hattı ve WAN’ınız aracılığıyla yine de size ulaşabilir. 

>[AZURE.IMPORTANT] Microsoft Eşlemesi’nde alınan önekler için AS YOLU’ndaki özel AS numaralarını kaldırırız. Microsoft Eşlemesi için yönlendirmeyi etkilemek amacıyla AS YOLU’nda ortak AS numaraları eklemeniz gerekir.

![](./media/expressroute-optimize-routing/expressroute-case2-solution.png)



<!---HONumber=Jun16_HO2-->


