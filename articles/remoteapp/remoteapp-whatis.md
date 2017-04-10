---
title: Azure RemoteApp nedir? | Microsoft Belgeleri
description: "Azure RemoteApp aracılığıyla uygulama ve kaynakları nasıl herhangi bir cihazla paylaşacağınızı öğrenin."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
editor: 
ms.assetid: d7a8a311-e70a-4463-ac85-c7f62c500921
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/23/2016
ms.author: mbaldwin
translationtype: Human Translation
ms.sourcegitcommit: 5cce99eff6ed75636399153a846654f56fb64a68
ms.openlocfilehash: bc5de9daeb09339c9488de8699174cd280197525
ms.lasthandoff: 03/31/2017


---
# <a name="what-is-azure-remoteapp"></a>Azure RemoteApp nedir?
> [!IMPORTANT]
> Azure RemoteApp 31 Ağustos 2017’de kullanımdan kaldırılacaktır. Ayrıntılı bilgi için [duyuruyu](https://go.microsoft.com/fwlink/?linkid=821148) okuyun.
> 
> 

Azure RemoteApp, Uzak Masaüstü Hizmetleri altyapısının kullanıldığı şirket içi Microsoft RemoteApp işlevlerini Azure’a getirir. Azure RemoteApp birçok farklı kullanıcı cihazından uygulamalara güvenli uzaktan erişim sağlamanıza yardımcı olur. Azure RemoteApp, bulutta kalıcı olmayan Terminal Sunucusu oturumları barındırır ve bunları kullanabilir ve kullanıcılarınızla paylaşabilirsiniz.

Azure RemoteApp ile neredeyse tüm cihazlardan kullanıcılarla uygulamaları ve kaynakları paylaşabilirsiniz. Uygulamalarınızın bulutta barındırılması, donanım ve ölçeklendirmenin kullanıcı taleplerine uygun olmasına dikkat ettiğimiz anlamına gelir. Yapmanız gereken tek şey paylaşmak istediğiniz uygulamaları karşıya yüklemek ve ardından kullanıcıların bu uygulamaları kullanmasını sağlamaktır. [Kullanıcılar kendi cihazlarını kullanırken](remoteapp-clients.md), siz de her şeyi Azure portalından yönetirsiniz. Uygulamalar ve verilerin güvenliğini sağlamak için şirket kimlik bilgilerinizi kullanma seçeneğiniz de vardır.

Azure RemoteApp hakkında daha fazla bilgi için okumaya devam edin, veya zaten ikna olduysanız [şimdi deneyin](https://azure.microsoft.com/services/remoteapp/).

Azure RemoteApp hakkında sorularınız mı var? [SSS](remoteapp-faq.md)’lerimize göz atın.

Azure RemoteApp [Microsoft Sanal Masaüstü Altyapısı](http://www.microsoft.com/server-cloud/products/virtual-desktop-infrastructure/explore.aspx)’nın bir parçasıdır.

**Yeni!** Azure RemoteApp hakkında daha fazla bilgi almak ister misiniz? Veya Azure RemoteApp’i ölçekte doğrulamak için hazır mısınız? Haftalık [uzmanlara sorun web seminerimize](https://azureinfo.microsoft.com/AzureRemoteAppAskTheExperts-Registration-Page.html?ls=Website) katılın.

## <a name="azure-remoteapp-collections"></a>Azure RemoteApp koleksiyonları
İki tür [Azure RemoteApp koleksiyonu](remoteapp-collections.md) vardır:

* **Bulut koleksiyonu**, bulutta barındırılır ve buluttaki programlar için verileri depolar. Kullanıcılar, kendi Microsoft hesapları veya Azure Active Directory ile eşitlenen ya da federasyonla yönetilen kurumsal kimlik bilgilerini kullanarak uygulamalara erişebilir.
  
    Paylaşmak istediğiniz uygulama şirketinizin özel ağındaki herhangi bir kaynağa bağlanmayı gerektirmediğinde (örneğin bir VPN aracı üzerinden) bulut koleksiyonunu seçin. Uygulama İnternet, OneDrive veya Azure üzerindeki kaynakları kullanıyorsa, bulut bağlantısı kullanabilirsiniz. Ayrıca bu oluşturulması en hızlı seçenektir.
* **Karma koleksiyon**, Azure bulutunda barındırılır ve burada veri toplar, ancak kullanıcıların yerel ağınızda depolanan veriler ve kaynaklara erişmesine de olanak sağlar. Kullanıcılar, Azure Active Directory ile eşitlenen ya da federasyonla yönetilen kurumsal kimlik bilgilerini kullanarak uygulamalara erişebilir.
  
    Şirketinizin özel ağındaki kaynaklara bağlantı gerekiyorsa, karma koleksiyonu seçin. Örneğin, uygulamanın aşağıdakilerden birine erişmesi gerekiyorsa:
  
  * İntranet üzerinde bulunan dosya sunucuları
  * Quicken
  * Bir güvenlik duvarının arkasındaki veritabanları
    
    Bu genellikle, özel ağlarında çok sayıda buluta taşınamaz kaynak bulunan büyük şirketler için yararlıdır.

Farklı koleksiyonlarda ağlar da dahil farklı seçenekler vardır, bu nedenle sizin için [hangi koleksiyonun](remoteapp-collections.md) en iyi çalıştığını belirlemeniz gerekir. 

### <a name="updating-your-collection"></a>Koleksiyonunuzu güncelleştirme
Karma ve bulut koleksiyonları arasındaki temel farklardan biri yazılım güncelleştirmelerinin işlenme şeklidir. Önceden yüklenmiş Office 365 ProPlus veya Office 2013 görüntüsünü kullanan bulut bağlantısında güncelleştirmeler hakkında endişelenmeniz gerekmez. Hizmet, hem uygulamalar hem de işletim sistemi için kendi bakımını yapar ve düzenli olarak güncelleştirmeleri uygular.

Karma koleksiyonlarda ve özel bir şablon görüntüsü kullanan bulut koleksiyonlarında görüntü ve uygulamaların güncelliğini sağlamaktan siz sorumlu olursunuz. Etki alanına katılmış görüntüler için Windows Update, Grup İlkesi veya System Center gibi araçları kullanarak güncelleştirmeleri denetleyebilirsiniz.

Özel şablon görüntünüzü güncelleştirdikten sonra, yeni görüntüyü Azure bulutuna yükler ve ardından yeni görüntüyü kullanarak koleksiyonu güncelleştirirsiniz. (Bunu Azure RemoteApp **Hızlı Başlangıç** sayfası veya Pano’dan yapabilirsiniz.)

Daha fazla bilgi için bkz. [Koleksiyonunuzu güncelleştirme](remoteapp-update.md).

## <a name="supported-remoteapp-clients"></a>Desteklenen RemoteApp istemcileri
Azure RemoteApp, Windows ve Windows RT RemoteApp istemci uygulamaları ile Mac, iOS ve Android için Microsoft Uzak Masaüstü uygulamalarında desteklenir. Kullanıcılarınız yeni Azure RemoteApp programlarına erişmek için bu uygulamaları kendi mobil cihazlarında veya bilgisayarlarında kullanabilir.

İstemciler hakkında daha fazla bilgi için bkz. [Azure RemoteApp’te uygulamalarınıza erişme](remoteapp-clients.md).

## <a name="next-steps"></a>Sonraki adımlar
Başlayın! Deneyin! Bu makaleler Azure RemoteApp kullanmaya başlamanıza yardımcı olur:

* [Azure RemoteApp için nasıl bir koleksiyona ihtiyacınız var?](remoteapp-collections.md)
* [Azure RemoteApp görüntüsü oluşturma](remoteapp-imageoptions.md)
* [Azure RemoteApp bulut koleksiyonu oluşturma](remoteapp-create-cloud-deployment.md)
* [Azure RemoteApp karma koleksiyonu oluşturma](remoteapp-create-hybrid-deployment.md)
* [Azure RemoteApp’te lisanslama nasıl çalışır?](remoteapp-licensing.md)
* [Azure RemoteApp’i kullanmak için en iyi uygulamalar](remoteapp-bestpractices.md)
* [Azure RemoteApp hakkında SSS](remoteapp-faq.md)

### <a name="help-us-help-you"></a>Yardımımıza katkıda bulunun
Bu makaleyi derecelendirmenin ve aşağıda yorum yapmamanın yanı sıra makalede değişiklik de yapabileceğinizi biliyor muydunuz? Eksik bir şeyler mi var? Yanlış bir şeyler mi var? Kafa karıştırıcı bir şeyler mi yazdım? Değişiklik yapmak için yukarı doğru ilerleyin ve **GitHub üzerinde düzenle** veya **Düzenle**’ye tıklayın. Bu değişiklikler incelenmek üzere bize gönderilir ve kabul edildikten sonra değişiklikleriniz ve iyileştirmeleriniz burada görünür.


