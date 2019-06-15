---
title: Erişimi - Azure App Service | Microsoft Docs
description: Azure App Service ile erişim kısıtlamaları kullanma
author: ccompy
manager: stefsch
editor: ''
services: app-service\web
documentationcenter: ''
ms.assetid: 3be1f4bd-8a81-4565-8a56-528c037b24bd
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 06/06/2019
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 2b0892fb107827cd9060a36855e9b8bf4416463c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67069427"
---
# <a name="azure-app-service-access-restrictions"></a>Azure App Service'e erişim kısıtlamaları #

Erişim kısıtlamaları uygulamanıza ağ erişimi denetleyen öncelik sıralı izin verme/reddetme listesini tanımlamanıza olanak sağlar. Liste, IP adresleri ya da Azure sanal ağ alt ağları ekleyebilirsiniz. Bir veya daha fazla olduğunda, yoktur ardından örtük "tümünü listenin en sonunda bulunan reddet".

Erişim kısıtlamalarını yeteneği de dahil olmak üzere barındırılan iş yükler tüm App Service ile çalışır; Web apps, API apps, Linux uygulamaları, Linux kapsayıcı uygulamaları ve işlevleri.

Uygulamanız için bir istek yapıldığında, Kimden adresinin, erişim kısıtlamaları listesindeki IP adresi kurallara göre değerlendirilir. Kimden adresi Microsoft.Web için hizmet uç noktaları ile yapılandırılmış bir alt ağda ise, kaynak alt ağ erişim kısıtlamaları listenizde sanal ağ kuralları karşı karşılaştırılır. Adres listesinde yer alan kurallara göre erişim izin verilmiyorsa, hizmet ile yanıtlar bir [HTTP 403](https://en.wikipedia.org/wiki/HTTP_403) durum kodu.

Erişim kısıtlamaları özelliği, kodunuzun çalıştığı çalışan ana Yukarı Akış olan App Service ön uç rollerinde gerçekleştirilir. Bu nedenle, erişim kısıtlamaları etkili bir şekilde ağ ACL'leri altındadır.

Bir Azure sanal ağdan (VNet), web uygulamanıza erişimi kısıtlama olanağı adlı [hizmet uç noktalarını][serviceendpoints]. Hizmet uç noktaları, çok kiracılı bir hizmet için Seçili alt ağdan erişimi sağlar. Hem ağ tarafı, hem de ile etkin bir hizmet üzerinde etkinleştirilmesi gerekir. Bir App Service Ortamı'nda barındırılan uygulamalar için trafiği kısıtlamak için çalışmaz.  Bir App Service Ortamı'nda varsa, IP adresi kuralları ile uygulamanıza erişimi denetleyebilirsiniz.

![erişim kısıtlamaları akışı](media/app-service-ip-restrictions/access-restrictions-flow.png)

## <a name="adding-and-editing-access-restriction-rules-in-the-portal"></a>Ekleme ve erişimi kısıtlama kuralları Portalı'nda düzenleme ##

Uygulamanız için bir erişim kısıtlama kuralı eklemek, açmak için menü kullanın **ağ**>**erişim kısıtlamalarını** tıklayın **erişim kısıtlamalarını yapılandırma**

![App Service ağ seçenekleri](media/app-service-ip-restrictions/access-restrictions.png)  

Erişim kısıtlamaları Arabiriminden uygulamanız için tanımlanan erişim kısıtlama kuralları listesini gözden geçirebilirsiniz.

![Liste erişim kısıtlamaları](media/app-service-ip-restrictions/access-restrictions-browse.png)

Listenin tüm uygulamanızı olan geçerli kısıtlamalar gösterilir. Uygulamanızı bir sanal ağ kısıtlaması varsa, tablonun hizmet uç noktaları için Microsoft.Web etkinleştirilip etkinleştirilmediğini gösterir. Uygulama tanımlı hiçbir kısıtlama olduğunda, uygulamanızı her yerden erişilebilir.  

## <a name="adding-ip-address-rules"></a>IP adresi kuralları ekleme

Tıklayabilirsiniz **[+] Ekle** yeni bir erişim kısıtlama kuralı eklemek için. Bir kural eklediğinizde, hemen geçerli olur. Kurallar öncelik sırasına göre yukarı ve en düşük sayıdan başlayan uygulanır. Örtük Reddet tek bir kural eklediğinizde, geçerli tüm yoktur.

Bir kural oluştururken izin verme/reddetme ve ayrıca kuralının türünü seçmeniz gerekir. Öncelik değeri ve kısıtlama erişim sağlamak için gereklidir.  İsteğe bağlı olarak kurala bir ad ve açıklama ekleyebilirsiniz.  

![bir IP erişim kısıtlama Kuralı Ekle](media/app-service-ip-restrictions/access-restrictions-ip-add.png)

Bir IP adresi ayarlamak için kural tabanlı, bir IPv4 veya IPv6 türünü seçin. IP adresi gösterimi, IPv4 ve IPv6 adresleri CIDR gösteriminde belirtilmelidir. Bir tam adresini belirtmek için burada IP adresiniz ilk dört sekizlik tabanda temsil eder ve özelliğini/32 maske 1.2.3.4/32 gibi kullanabilirsiniz. Tüm adresler için IPv4 CIDR gösteriminde 0.0.0.0/0 ' dir. CIDR gösterimi hakkında daha fazla bilgi edinebilirsiniz [sınıfsız etki alanları arası yönlendirme](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing). 

## <a name="service-endpoints"></a>Hizmet uç noktaları

Hizmet uç noktaları, seçili Azure sanal ağ alt ağlara erişimi kısıtlama olanak sağlar. Belirli bir alt ağa erişimini kısıtlamak için bir sanal ağ türü ile bir kısıtlama kuralı oluşturun. Abonelik, VNet ve alt ağı izin verdiğini veya reddettiğini erişim ile istediğiniz yerden devam edebilir. Hizmet uç noktaları zaten Microsoft.Web ile seçtiğiniz bir alt ağ için etkin değilse, bunu yapmak soran kutuyu sürece bu otomatik olarak sizin için etkinleştirilecek. Veya alt ağdaki hizmet uç noktalarını etkinleştirmek için izinleriniz varsa uygulama ancak alt etkinleştirmek istediğiniz durum için büyük ölçüde ilişkilidir. Alt ağdaki hizmet uç noktalarının etkinleştirilmesi başkası almanız gerekirse, onay kutusunu işaretleyin ve uygulamanızı, daha sonra alt ağda etkinleştiriliyor olasılığına hizmet uç noktaları için yapılandırılmış olması. 

![bir sanal ağ erişimini kısıtlama Kuralı Ekle](media/app-service-ip-restrictions/access-restrictions-vnet-add.png)

Hizmet uç noktaları, bir App Service ortamında çalışan uygulamalar için erişimi kısıtlamak için kullanılamaz. Uygulamanızı bir App Service Ortamı'nda olduğunda, IP erişim kuralları ile uygulamanıza erişimi denetleyebilirsiniz. 

Hizmet uç noktaları ile uygulama ağ geçitleri ya da diğer WAF cihazlarını uygulamanızla yapılandırabilirsiniz. Güvenli arka uçları ile çok katmanlı uygulamalar da yapılandırabilirsiniz. Bazı olasılık hakkında daha fazla bilgi almak için okuma [ağ özellikleri ve App Service](networking-features.md).

## <a name="managing-access-restriction-rules"></a>Erişimi kısıtlama kurallarını yönetme

Mevcut bir erişim kısıtlama kuralı düzenlemek için herhangi bir satıra tıklayabilirsiniz. Düzenlemeler hemen öncelik sıralamada değişiklikleri içeren son derece etkilidir.

![bir erişim kısıtlama kuralını Düzenle](media/app-service-ip-restrictions/access-restrictions-ip-edit.png)

Bir kural düzenlediğinizde, bir IP adresi kuralı ve bir sanal ağ kuralı arasında türünü değiştiremezsiniz. 

![bir erişim kısıtlama kuralını Düzenle](media/app-service-ip-restrictions/access-restrictions-vnet-edit.png)

Bir kuralı silmek için tıklayın **...**  kural ve ardından **Kaldır**.

![erişimi kısıtlama kuralını Sil](media/app-service-ip-restrictions/access-restrictions-delete.png)

## <a name="blocking-a-single-ip-address"></a>Tek bir IP adresi engelleme ##

İlk IP kısıtlaması kuralınızı eklerken, hizmet açık bir ekleme **tümünü Reddet** 2147483647 önceliğine sahip. Uygulamada, açık **tümünü Reddet** kuralı yürütülen son kural olacaktır ve kullanarak açıkça verilmeyen tüm IP adreslerini erişimini engeller bir **izin** kuralı.

Kullanıcılar tek bir IP adresi veya IP adresi bloğu açıkça engellemek istediğiniz senaryosu için ancak her şeyi izin başka erişim, açık bir eklemek için gereken **tümüne izin ver** kuralı.

![bloğu tek bir IP adresi](media/app-service-ip-restrictions/block-single-address.png)

## <a name="scm-site"></a>SCM sitesine 

Uygulama erişimi denetleme olanağına olmaya ek olarak, erişim, uygulamanız tarafından kullanılan scm sitesine da kısıtlayabilirsiniz. Web dağıtımı uç noktası ve ayrıca Kudu Konsolu scm sitedir. Ayrı ayrı erişim kısıtlamalarını uygulamadan scm sitesine atayın veya aynı hem uygulama hem de scm sitesine ayarlayın. Uygulamanız aynı kısıtlamalara sahip kutuyu işaretlediğinizde her şey kullanıma blanked. Kutunun işaretini kaldırırsanız, daha önce scm sitesine sahip hangi ayarları uygulanır. 

![Liste erişim kısıtlamaları](media/app-service-ip-restrictions/access-restrictions-scm-browse.png)

## <a name="programmatic-manipulation-of-access-restriction-rules"></a>Programsal olarak erişim kısıtlama kuralları ##

Şu anda herhangi bir CLI veya PowerShell yeni erişim kısıtlamaları özelliği için olmakla birlikte değerlerini el ile Kaynak Yöneticisi'nde uygulama yapılandırması üzerindeki PUT işlemi sırasında ayarlanabilir. Örneğin, resources.azure.com kullanın ve gerekli JSON eklemek için ipSecurityRestrictions bloğu düzenleyin.

Bu bilgiler Kaynak Yöneticisi'nde konumudur:

Management.Azure.com/subscriptions/**abonelik kimliği**/resourceGroups/**kaynak grupları**/providers/Microsoft.Web/sites/**web uygulaması adı**  /config/web? api sürümü 2018-02-01 =

Önceki örnek JSON sözdizimi aşağıdaki gibidir:

    "ipSecurityRestrictions": [
      {
        "ipAddress": "131.107.159.0/24",
        "action": "Allow",
        "tag": "Default",
        "priority": 100,
        "name": "allowed access"
      }
    ],

## <a name="function-app-ip-restrictions"></a>İşlev uygulaması IP kısıtlamaları

IP kısıtlamaları, App Service planları ile aynı işlevlere sahip her iki işlev uygulamaları için kullanılabilir. IP kısıtlamaları etkinleştirme, izin verilmeyen tüm IP'ler için portal Kod Düzenleyicisi'ni devre dışı bırakır.

[Buradan daha fazla bilgi edinin](../azure-functions/functions-networking-options.md#inbound-ip-restrictions)


<!--Links-->
[serviceendpoints]: https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview
