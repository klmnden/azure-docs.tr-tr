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
    ms.date="08/15/2016"
    ms.author="elizapo" />


# <a name="using-microsoft-outlook-in-azure-remoteapp"></a>Azure RemoteApp’te Microsoft Outlook kullanma

> [AZURE.IMPORTANT]
> Azure RemoteApp kullanımdan kaldırılıyor. Ayrıntılı bilgi için [duyuruyu](https://go.microsoft.com/fwlink/?linkid=821148) okuyun.

Azure RemoteApp Microsoft Outlook O365’i destekler. [Azure RemoteApp’te Office’in çalışması](remoteapp-officesubscription.md) hakkında daha fazla bilgi edinin. Azure RemoteApp’te kullanılırken Outlook için bazı önerilen ayarlar vardır.

## <a name="cached-mode"></a>Önbelleğe alınmış mod
Önbelleğe alınmış mod, Azure RemoteApp’te Outlook kullanırken önerilen bir yapılanmadır. Önbelleğe Alınmış Modu kullanmak üzere bir Outlook 2013 hesabı yapılandırırken, Outlook 2013 kullanıcının bilgisayarında, Çevrimdışı Adres Defteri (OAB) ile birlikte bir çevrimdışı veri dosyasında (OST dosyası) depolanan kullanıcının Microsoft Exchange posta kutusunun yerel bir kopyasından çalışır. Önbelleğe alınmış posta kutusu ve OAB düzenli olarak O365 hizmetinden güncelleştirilir. [Önbelleğe alınmış ve çevrimiçi mod arasındaki farklar](https://technet.microsoft.com/library/jj683103.aspx) hakkında daha fazla bilgi edinin.

Kullanıcı, hesap kurulumu sırasında veya hesap ayarlarını değiştirerek **Önbelleğe Alınmış Exchange Modu** ya da **Çevrimiçi Mod** arasında seçim yapabilir. Ayrıca, Office Özelleştirme Aracı’nı (OCT) ya da Grup İlkesi’ni kullanarak bir mod ya da diğerini dağıtabilirsiniz.  

[Adım adım önbelleğe alınmış modu etkinleştirme yönergeleri](https://technet.microsoft.com/library/c6f4cad9-c918-420e-bab3-8b49e1885034#proc)ni okuyun.

## <a name="search"></a>Arama
Azure RemoteApp’te Outlook arama özelliğini kullanmanın sınırlamaları vardır. Azure RemoteApp kullanıcı oturumlarını yerleştirmek için havuza alınmış sanal makineler kullanır. Arama dizini oluşturma, farklı sanal makineler için farklı olan makine kimliğine bağlıdır. Bir kullanıcının Azure RemoteApp’te oturum açtığı her sefer yeni bir sanal makineye yönlendirilmesi mümkündür. Bu, yerel aramayı etkinleştirdiğimizde, makine kimliği her değiştiğinde dizin oluşturucunun çalışacağı anlamına gelir (kullanıcı farklı sanal makinedeyken).  .OST dosyasının boyutuna bağlı olarak, dizin oluşturucunun diğer uygulamalar için gereken kaynakları tamamlaması ve kullanması uzun sürebilir. Arama yavaş olacağı gibi sonuç da vermeyebilir. Çevrimiçi Mod hesap profilinin kullanılması bunu çözebilir, ancak yerel bir önbelleğin olmaması nedeniyle genel performans etkilenebilir (önbelleğe alınan mod ile çevrimiçi mod arasındaki fark hakkında daha fazla bilgi için yukarıdaki bağlantıya bakın). Ne yazık ki, Outlook 2013’te dizinli/yerel arama devre dışı bırakılamaz ve çevrimiçi arama varsayılan olarak etkinleştirilmez.



<!--HONumber=Oct16_HO3-->


