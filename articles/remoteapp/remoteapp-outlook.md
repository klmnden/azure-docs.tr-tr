<properties
    pageTitle="Azure RemoteApp’te Outlook kullanma | Microsoft Azure" 
    description="Azure RemoteApp’te Outlook’u yapılandırmayı ve kullanmayı öğrenin | Microsoft Azure"
    services="remoteapp"
    documentationCenter=""
    authors="pavithir"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="05/18/2016"
    ms.author="elizapo" />

# Azure RemoteApp’te Microsoft Outlook kullanma

Azure RemoteApp Microsoft Outlook O365’i destekler. [Azure RemoteApp’te Office’in çalışması](remoteapp-officesubscription.md) hakkında daha fazla bilgi edinin. Azure RemoteApp’te kullanılırken Outlook için bazı önerilen ayarlar vardır.

## Önbelleğe alınmış mod
Önbelleğe alınmış mod, Azure RemoteApp’te Outlook kullanırken önerilen bir yapılanmadır. Önbelleğe Alınmış Modu kullanmak üzere bir Outlook 2013 hesabı yapılandırırken, Outlook 2013 kullanıcının bilgisayarında, Çevrimdışı Adres Defteri (OAB) ile birlikte bir çevrimdışı veri dosyasında (OST dosyası) depolanan kullanıcının Microsoft Exchange posta kutusunun yerel bir kopyasından çalışır. Önbelleğe alınmış posta kutusu ve OAB düzenli olarak O365 hizmetinden güncelleştirilir. [Önbelleğe alınmış ve çevrimiçi mod arasındaki farklar](https://technet.microsoft.com/library/jj683103.aspx) hakkında daha fazla bilgi edinin.

Kullanıcı, hesap kurulumu sırasında veya hesap ayarlarını değiştirerek **Önbelleğe Alınmış Exchange Modu** ya da **Çevrimiçi Mod** arasında seçim yapabilir. Ayrıca, Office Özelleştirme Aracı’nı (OCT) ya da Grup İlkesi’ni kullanarak bir mod ya da diğerini dağıtabilirsiniz.  

[Adım adım önbelleğe alınmış modu etkinleştirme yönergeleri](https://technet.microsoft.com/library/c6f4cad9-c918-420e-bab3-8b49e1885034#proc)ni okuyun.

## Arama
Azure RemoteApp’te Outlook arama özelliğini kullanmanın sınırlamaları vardır. Azure RemoteApp kullanıcı oturumlarını yerleştirmek için havuza alınmış sanal makineler kullanır. Arama dizini oluşturma, farklı sanal makineler için farklı olan makine kimliğine bağlıdır. Bir kullanıcının Azure RemoteApp’te oturum açtığı her sefer yeni bir sanal makineye yönlendirilmesi mümkündür. Bu, yerel aramayı etkinleştirdiğimizde, makine kimliği her değiştiğinde dizin oluşturucunun çalışacağı anlamına gelir (kullanıcı farklı sanal makinedeyken).  .OST dosyasının boyutuna bağlı olarak, dizin oluşturucunun diğer uygulamalar için gereken kaynakları tamamlaması ve kullanması uzun sürebilir. Arama yavaş olacağı gibi sonuç da vermeyebilir. Bu sorunu çözmenin bir yolu varsayılan olarak çevrimiçi aramayı etkin hale getirmektir. Ne yazık ki, Outlook 2013’te dizinli/yerel arama devre dışı bırakılamaz ve çevrimiçi arama varsayılan olarak etkinleştirilmez.

Outlook 2016’nın, Exchange 2016’da barındırılan (veya Office 365’te barındırılan ) posta kutuları için yeni bir çevrimiçi arama deneyimi sağlayarak bu durumun üstesinden gelmek için bir çözümü vardır. Bu, sunucu arama sonuçlarını yerel önbelleğe karşı kullanır (OST). Outlook, bazı senaryolarda arama dizin oluşturucusu kullanmaya geri dönebilir, ancak çoğu arama çevrimiçi mod kullanır. Azure RemoteApp’in önerisi, posta araması çok önemli bir senaryo ise, Outlook 2016’nın kullanılması yönündedir.



<!--HONumber=Jun16_HO2-->


