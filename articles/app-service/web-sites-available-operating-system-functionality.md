---
title: "Azure App Service'te işletim sistemi işlevi"
description: "Web uygulamaları, mobil uygulaması arka uçlarını ve Azure App Service'te API uygulamaları için kullanılabilir OS işlevselliği hakkında bilgi edinin"
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: mollybos
ms.assetid: 39d5514f-0139-453a-b52e-4a1c06d8d914
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/01/2016
ms.author: cephalin
ms.openlocfilehash: 18ff5c81d0aa5e8a28ed8a11dad19811d2fa1d2c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="operating-system-functionality-on-azure-app-service"></a>Azure App Service'te işletim sistemi işlevi
Bu makalede çalışan tüm uygulamaları için kullanılabilir ortak temel işletim sistemi işlevselliğini [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714). Bu işlevsellik, dosya, ağ ve kayıt defteri erişimi ve tanılama günlüklerini ve olayları içerir. 

<a id="tiers"></a>

## <a name="app-service-plan-tiers"></a>Uygulama hizmeti planı katmanları
Uygulama hizmeti çok kiracılı bir barındırma ortamında müşteri uygulamalar çalışır. Uygulamaları dağıtılan **serbest** ve **paylaşılan** katmanları uygulamaları dağıtılmış sırada bu çalışan işlemleri içinde paylaşılan sanal makinelerde çalıştırma **standart** ve **Premium** katmanları özellikle tek bir müşteri ile ilişkili uygulamalar için ayrılmış sanal makineleri üzerinde çalışır.

Uygulama hizmeti farklı katmanlar arasında sorunsuz ölçeklendirme bir deneyim desteklediğinden, uygulama hizmeti uygulamalarınız için zorunlu güvenlik yapılandırması aynı kalır. Bu uygulamalar aniden farklı şekilde, uygulama hizmeti planı bir katmandan diğerine geçtiğinde beklenmeyen şekilde başarısız davrandığını yok sağlar.

<a id="developmentframeworks"></a>

## <a name="development-frameworks"></a>Geliştirme çerçeveleri
Uygulama hizmeti fiyatlandırma katmanlarına işlem kaynakları (CPU, disk depolama, bellek ve ağ çıkış) uygulamaları için kullanılabilir miktarını denetler. Ancak, framework işlevselliği uygulamaları için kullanılabilir derecesini ölçeklendirme katmanları bakılmaksızın aynı kalır.

App Service ASP.NET, klasik ASP, node.js dahil olmak üzere geliştirme çerçeveleri, PHP ve Python tümü uzantıları IIS içinde farklı çalıştır - çeşitli destekler. Basitleştirmek ve güvenlik yapılandırması normalleştirmek için uygulama hizmeti uygulamalar genellikle varsayılan ayarlarına çeşitli geliştirme çerçeveleri çalıştırın. Her bireysel geliştirme çerçevesi için işlevselliği ve API yüzey alanını özelleştirmek için uygulamaları yapılandırma bir yaklaşım silinmiş. Uygulama hizmeti bir uygulamanın geliştirme framework bakılmaksızın işletim sistemi işlevselliğini ortak bir taban çizgisi sağlayarak daha genel bir yaklaşım yerine alır.

Aşağıdaki bölümlerde, işletim sistemi işlevselliğini uygulama hizmeti uygulamaları için kullanılabilir genel türlerini özetler.

<a id="FileAccess"></a>

## <a name="file-access"></a>Dosya erişimi
Yerel sürücüler ve ağ sürücülerini içeren uygulama hizmeti içinde çeşitli sürücüsü yok.

<a id="LocalDrives"></a>

### <a name="local-drives"></a>Yerel sürücüler
Özünde, App Service Azure PaaS (hizmet olarak platform) altyapısı üzerinde çalışan bir hizmettir. Sonuç olarak, "bir sanal makineye bağlı olan" yerel sürücüleri aynı sürücü Azure'da çalışan herhangi bir çalışan rolü için kullanılabilir türleridir. Bu, bir işletim sistemi sürücüsü (D:\ sürücüsüne), Azure paket cspkg dosyalarını özel olarak App Service tarafından kullanılan (ve müşteriler için erişilemez) içeren bir uygulama sürücü ve büyüklüğü VM boyutuna bağlı olarak değişir "kullanıcı" sürücüsü (C:\ sürücüsü) içerir.

<a id="NetworkDrives"></a>

### <a name="network-drives-aka-unc-shares"></a>Ağ sürücüleri (diğer adıyla UNC paylaşımları)
Uygulama dağıtımını ve bakımını basitleştirir uygulama hizmeti benzersiz yönleri tüm kullanıcı içeriğin UNC paylaşımlarına kümesine göre depolandığı biridir. Bu model çok sorunsuz bir şekilde birden çok Yük Dengeleme sunucuları sahip ortamlarda şirket içi web sitesi barındırma tarafından kullanılan içerik depolama ortak desenini eşler. 

App Service içinde vardır her veri merkezinde oluşturulan UNC paylaşımlarına sayısı. Her veri merkezi tüm müşteriler için kullanıcı içeriği yüzdesi ayrılan her UNC paylaşımı. Ayrıca, tüm tek bir müşterinin aboneliğini her zaman aynı UNC yerleştirilir için içerik dosyasının paylaşır. 

Bulut iş hizmetleri nasıl nedeniyle unutmayın, belirli bir sanal makine bir UNC paylaşımı barındırmak için sorumlu zaman içinde değişir. Bulut işlemleri normal sürecinde duruma yukarı ve aşağı gibi UNC paylaşımlarına farklı sanal makineler tarafından bağlanacaktır sağlanır. Bu nedenle, uygulamaları hiçbir zaman bir UNC dosya yolu makine bilgileri zamanla kararlı kalacak sabit kodlanmış varsayımlar olmanız gerekir. Bunun yerine, bunlar uygun kullanmalısınız *sahte* mutlak bir yol **D:\home\site** , uygulama hizmeti sağlar. Bu sahte mutlak yolu birinin kendi uygulama başvuran taşınabilir, uygulama ve kullanıcı belirsiz bir yöntem sağlar. Kullanarak **D:\home\site**, bir aktarma paylaşılan dosyaları uygulama uygulaması her aktarımı için yeni bir mutlak yol yapılandırmak zorunda kalmadan.

<a id="TypesOfFileAccess"></a>

### <a name="types-of-file-access-granted-to-an-app"></a>Bir uygulamaya verilen dosya erişim türleri
Her müşterinin aboneliğini, bir veri merkezi içinde belirli bir UNC paylaşımında ayrılmış dizin yapısının. Bir müşteri paylaşmak tek bir müşteri aboneliğine ait dizinleri tümünün aynı UNC oluşmasını belirli veri merkezi içinde oluşturulan birden fazla uygulama olabilir. Paylaşım içerik, hata ve tanılama günlüklerini ve kaynak denetimi tarafından oluşturulan uygulama önceki sürümleri için olanlar gibi dizinleri içerebilir. Beklendiği gibi müşteri'nin uygulama dizinler için okuma ve yazma erişimi çalışma zamanında uygulamanın uygulama kodu tarafından kullanılabilir.

Bir uygulamanın çalıştığı sanal makineye bağlı yerel sürücüler, uygulama hizmeti uygulamaya özgü geçici yerel depolama C:\ sürücüsünde yığınını ayırır. Bir uygulama geçici kendi yerel depolama tam okuma/yazma erişimi olsa da, bu depolama gerçekten doğrudan uygulama kodu tarafından kullanılmaya yönelik değildir. Bunun yerine, hedefi geçici dosya depolaması için IIS ve web uygulama çerçeveleri sağlamaktır. Uygulama hizmeti de tek tek uygulamalar yerel dosya depolama aşırı miktarda kullanmasını önlemek için her uygulama için kullanılabilir geçici yerel depolama miktarını sınırlar.

Dizin geçici ASP.NET dosyaları için nasıl uygulama hizmeti geçici yerel depolama alanı kullanır, iki örnek verilmiştir ve IIS için dizin sıkıştırılmış dosyaları dışarıda bırak. ASP.NET derleme sistem "ASP.NET dosyaları" dizinini geçici derleme önbellek konumu olarak kullanır. IIS "IIS geçici sıkıştırılmış dosyaların" dizin sıkıştırılmış yanıt çıktısı depolamak için kullanır. Uygulama başına geçici yerel depolama için iki dosya kullanım (yanı sıra diğerleri) bu tür App Service'te eşleştirilir. Bu yeniden eşleme işlevselliği beklendiği gibi devam etmesini sağlar.

Uygulama hizmeti her uygulamada "daha fazla burada açıklanan uygulama havuzu kimliği" olarak adlandırılan bir rastgele benzersiz düşük ayrıcalıklı çalışan işlem kimliği çalışan: [http://www.iis.net/learn/manage/configuring-security/application-pool-identities](http://www.iis.net/learn/manage/configuring-security/application-pool-identities). Uygulama kodu, işletim sistemi sürücüsü (D:\ sürücüsüne) temel salt okunur erişim için bu kimliği kullanır. Bu uygulama kodu ortak dizin yapıları listesinde ve işletim sistemi sürücüsünde ortak dosyaları okumasına anlamına gelir. Bu, sağladığınızda biraz geniş düzeyde erişim, aynı dizinleri ve dosyaları erişilebilir olmasını görünse de Azure çalışan rolünde barındırılan hizmetin ve sürücü içeriğini okuyun. 

<a name="multipleinstances"></a>

### <a name="file-access-across-multiple-instances"></a>Birden çok örneği arasında dosya erişimi
Uygulama kodu yazabildiğinizi ve bir uygulamanın içeriği giriş dizini içerir. Tüm örnekleri aynı dizinde görebilmesi için giriş dizini, bir uygulama birden çok örneği üzerinde çalışıyorsa, tüm örnekler arasında paylaşılır. Bir uygulama için giriş dizini karşıya yüklenen dosyaların kaydederse, bu nedenle, örneğin, bu dosyaları tüm örnekleri için hemen kullanılabilir. 

<a id="NetworkAccess"></a>

## <a name="network-access"></a>Ağ erişimi
Ağ bağlantıları dış hizmetler kullanıma Internet erişilebilir uç noktalarına giden yapmak için temel UDP protokollerini ve TCP/IP'yi uygulama kodu kullanabilirsiniz. Uygulamaları, Hizmetleri içinde Azure &#151; Örneğin, SQL veritabanı için HTTPS bağlantıları oluşturarak bağlanmak için bu aynı protokollerini kullanabilirsiniz.

Bir yerel bir geri döngü bağlantısını kurmak ve bu yerel bir geri döngü yuvadaki dinleme bir uygulamaya sahip uygulamalar için sınırlı bir özellik yok. Öncelikle işlevleri bir parçası olarak yerel bir geri döngü yuvalarda dinleme uygulamalarını etkinleştirmek için bu özelliği mevcut. Her uygulama bir "özel" geri döngü bağlantı görür unutmayın; uygulama "A", "B" uygulama tarafından oluşturulan yerel bir geri döngü yuva için dinleyemiyor.

Adlandırılmış Kanallar topluca bir uygulama çalıştırmasına farklı işlemler arasında işlemler arası iletişim (IPC) mekanizması olarak da desteklenir. Örneğin, IIS Fastcgı modülü PHP sayfaları çalıştırmak tek tek işlemleri koordine etmek için adlandırılmış kanallar kullanır.

<a id="Code"></a>

## <a name="code-execution-processes-and-memory"></a>Kod yürütmeyi, işlemleri ve bellek
Daha önce belirtildiği gibi uygulamalar düşük ayrıcalıklı çalışan işlemleri içinde rastgele uygulama havuzu kimliği kullanarak çalıştırın. Uygulama kodu kökenli tüm alt işlemleri CGI işlemi veya diğer uygulamalar tarafından yanı sıra, çalışan işlem ile ilişkili bellek alanına erişebilir. Ancak, aynı sanal makineye olsa bile bir uygulama bellek veya başka bir uygulama verilerinin erişemez.

Uygulamalar, komut dosyaları veya desteklenen web geliştirme çerçeveleri ile yazılan sayfa çalıştırabilirsiniz. Uygulama hizmeti daha kısıtlı modları için herhangi bir web framework ayarı yapılandırmaz. Örneğin, uygulama hizmeti üzerinde çalışan ASP.NET uygulamaları daha kısıtlı bir güven modu aksine "tam" güven çalıştırın. Web çerçeveleri hem klasik ASP ve ASP.NET dahil olmak üzere, Windows işletim sistemindeki varsayılan kayıtlı ADO (ActiveX Data Objects) gibi işlem içi COM bileşenleri (ancak işlem COM bileşenleri dışı) çağırabilirsiniz.

Uygulamaları oluşturma ve rasgele kod çalıştırabilir. Bir komut kabuğu spawn gibi şeyler veya bir PowerShell betiğini çalıştırmak bir uygulama için izin verilebilir. Rastgele kod ve işlemleri bir uygulamadan kökenli olsa bile, ancak yürütülebilir programları ve betikleri hala üst uygulama havuzuna ayrıcalıklarının için kısıtlanır. Örneğin, yürütülebilir aynı IP adresini kendi NIC'i bir sanal makineden bağlantısını girişimi olamaz, ancak giden bir HTTP çağrısı yapan yürütülebilir bir uygulama oluşturabilir Giden ağ araması yapmadan düşük ayrıcalıklı kodu için izin verilir, ancak bir sanal makinede ağ ayarlarını yapılandırmak çalışırken yönetici ayrıcalıkları gerektirir.

<a id="Diagnostics"></a>

## <a name="diagnostics-logs-and-events"></a>Tanılama günlüklerini ve olayları
Günlük bilgilerini bazı uygulamalar erişme girişimi verileri başka bir kümesidir. App Service içinde çalışan kod için kullanılabilir günlük bilgi türlerini tanılama içerir ve uygulamaya kolayca erişilebilen bir uygulama tarafından oluşturulan bilgileri günlüğe kaydeder. 

Örneğin, etkin bir uygulama tarafından oluşturulan W3C HTTP günlükleri ya da bir günlük dizini W3C günlük kaydı depolama için bir müşteri ayarladıysa uygulama veya kullanılabilir blob depolamada oluşturulan ağ paylaşımı konumu üzerinde kullanılabilir. İkinci seçeneği büyük miktarda bir ağ paylaşımı ile ilişkilendirilmiş dosya depolama sınırları aşan riski olmadan toplanması günlükleri sağlar.

Benzer bir damarlı içinde .NET uygulamalarını gerçek zamanlı tanılama bilgilerini de izleme bilgileri uygulamanın ağ paylaşımına yazma seçeneklerle .NET izleme ve tanılama altyapısını kullanarak kaydedilebilir veya alternatif bir blob depolama konumuna.

Uygulamaları için kullanılabilir olmayan günlüğe kaydetme ve izleme tanılama Windows ETW olayları ve ortak Windows olay günlüklerini (örneğin sistem, uygulama ve güvenlik olay günlüklerini) alanlarıdır. ETW İzleme bilgilerini potansiyel olarak görüntülenebilir makine genelinde (ile hakkı ACL'leri) olabileceği için okuma ve yazma erişimi ETW olayları engellenir. Geliştiriciler, başarılı olması için görünecekleri API okuyup ETW olayları ve ortak Windows olay günlüklerini çalışmak için görünür, ancak uygulama hizmeti "çağrıları faking çünkü" yazmak için çağırdığı fark edebilirsiniz. Gerçekte, uygulama kodu bu olay verileri için erişim yok.

<a id="RegistryAccess"></a>

## <a name="registry-access"></a>Kayıt defteri erişimi
Uygulamaları salt okunur erişime sahiptir çok (tüm rağmen) kayıt defterinin bunlar üzerinde çalıştığını sanal makinenin. Uygulamada, bu yerel kullanıcı grubu salt okunur erişime izin kayıt defteri anahtarlarını uygulamalar tarafından erişilebilen anlamına gelir. Bir okuma veya yazma erişimi için şu anda desteklenmiyor kayıt defteri alanıdır HKEY\_geçerli\_kullanıcı yığını.

Tüm kullanıcı başına kayıt defteri anahtarlarına erişimi de dahil olmak üzere kayıt defteri yazma erişimi engellendi. Uygulamaları (yapıp bu yana) uygulamanın açısından bakıldığında, kayıt defteri yazma erişimi hiçbir zaman bağlı Azure ortamında dayanıyordu farklı sanal makineler arasında geçişi. Bir uygulama tarafından bağımlı yalnızca kalıcı yazılabilir depolama, App Service UNC paylaşımlarına depolanmış uygulama başına içerik dizin yapısıdır. 

## <a name="more-information"></a>Daha fazla bilgi

[Azure Web uygulaması sandbox](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox) -App Service'in yürütme ortamı hakkında en güncel bilgileri. Bu sayfayı doğrudan App Service geliştirme ekibi tarafından korunur.

> [!NOTE]
> Azure hesabı için kaydolmadan önce Azure App Service’i kullanmaya başlamak isterseniz, App Service’te hemen kısa süreli bir başlangıç web uygulaması oluşturabileceğiniz [App Service’i Deneyin](https://azure.microsoft.com/try/app-service/) sayfasına gidin. Kredi kartı ve taahhüt gerekmez.
> 
> 


