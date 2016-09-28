<properties
    pageTitle="Azure RemoteApp lisanslama | Microsoft Azure"
    description="Azure RemoteApp’te lisanslamanın nasıl çalıştığını öğrenin."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# Azure RemoteApp’te lisanslama nasıl çalışır?

> [AZURE.IMPORTANT]
> Azure RemoteApp kullanımdan kaldırılıyor. Ayrıntılı bilgi için [duyuruyu](https://go.microsoft.com/fwlink/?linkid=821148) okuyun.

Azure RemoteApp hizmetinizi ayarladınız, şablonlarınızı oluşturdunuz ve kullanıcılarınıza uygulama yayımlamaya hazırsınız. Ancak hala çözülmesi gereken son bir konu var: lisanslama. RemoteApp ve RemoteApp aracılığıyla paylaştığınız uygulamalar için lisanslama nasıl çalışır?

RemoteApp herhangi bir Windows lisansı veya Uzak Masaüstü CAL’si gerektirmez. Aboneliğiniz RemoteApp tarafındaki lisans sorunlarını çözer. (Ayrıntıları [fiyatlandırma planlarında](https://azure.microsoft.com/pricing/details/remoteapp) kontrol edebilirsiniz.)

Aboneliğinize dahil edilen görüntülerden birini kullanırsanız, bu görüntüdeki herhangi bir uygulamayı ayrı bir lisans olmadan paylaşabilirsiniz. Örneğin, koleksiyonunuzu oluşturmak için Windows Server 2012 R2 şablon görüntüsünü kullanıyorsanız, System Center Endpoint Protection’ı kullanıcılarınızla paylaşabilirsiniz. Bu kuralın tek özel durumu, ayrı bir abonelik gerektiren Office 365 ProPlus ve bir üretim koleksiyonunda paylaşılamayan Office 2013'tür.

RemoteApp ile birlikte gelen Office 365 şablon görüntüsünü kullanmak istiyorsanız, *mevcut* bir Office 365 ProPlus planınızın olması gerekir. Özel bir şablon kullanarak yayımladığınız herhangi bir Office 365 uygulaması için de aynı durum geçerlidir. Uygulamaları kendi aboneliğinizle etkinleştirmeniz gerekir. Bu deneme abonelikleri ve ücretli abonelikler için geçerlidir. Deneme süresi boyunca Office 365 şablon görüntüsünü kullanmak istiyorsanız, *ve zaten bir aboneliğiniz yoksa*, Office 365 sayfasına giderek bir deneme aboneliği için [kaydolun](https://go.microsoft.com/fwlink/p/?LinkID=403802). Daha fazla bilgi için bkz. [RemoteApp ve Office nasıl birlikte çalışır?](remoteapp-o365.md)

Deneme süresi boyunca, bir Office 365 deneme aboneliği istemiyorsanız, RemoteApp ile birlikte gelen Office 2013 Professional Plus şablon görüntüsünü kullanın. Bu şablon görüntüsü yalnızca 30 gün boyunca kullanılabilir ve ücretli bir koleksiyona dönüştürülemez.

Diğer uygulamalar için, uygulamayı paylaşmak için lisansa sahip olduğunuzdan emin olmanız gerekir.

Kulağa mantıklı geliyor, değil mi? Yasal olarak paylaşma hakkınız olan herhangi bir uygulamayı yayımlayabilirsiniz. Programlarınızı paylaşmak için gerçekten yetkili olduğunuzdan emin olmanız gerekir.

Lütfen bir bulut koleksiyonunda CAL veya Toplu Lisans sözleşmesi kullanamayacağınızı unutmayın. Karma koleksiyonunuzda uygulamaları etkinleştirmek için bir Toplu Lisans sözleşmesi *kullanabilirsiniz* (Office hariç). Bunları Toplu Lisans medyasından şablon görüntünüze yüklemeniz yeterlidir. Lisansları bir Uzak Masaüstü ortamında yüklemek için uygulama satıcısından aldığınız bilgiler doğrultusunda hareket edin.



<!--HONumber=Sep16_HO3-->


