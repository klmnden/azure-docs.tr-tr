---
title: Azure AD uygulama proxy'si bağlayıcılar anlama | Microsoft Docs
description: Azure AD uygulama proxy'si bağlayıcılar hakkında temel bilgiler yer almaktadır.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/12/2017
ms.author: barbkess
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 14e2b82b5c32e1b36bf730b7b834c9b8ad124629
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="understand-azure-ad-application-proxy-connectors"></a>Azure AD uygulama proxy'si bağlayıcılar anlama

Bağlayıcılar, hangi Azure AD uygulama proxy'si mümkün kılar ' dir. Basit, dağıtım ve Bakım daha kolay ve Süper güçlü oldukları. Bu makalede nasıl çalıştığını, hangi bağlayıcılar olan ve dağıtımınız en iyi duruma getirme için bazı öneriler açıklanır. 

## <a name="what-is-an-application-proxy-connector"></a>Bir uygulama Proxy Bağlayıcısı nedir?

Bağlayıcılar, şirket içi yaslanın ve uygulama proxy'si Hizmeti giden bağlantısı kolaylaştırmak basit aracılardır. Bağlayıcılar arka uç uygulama erişimi olan bir Windows Server yüklenmelidir. Bağlayıcılar, belirli uygulamaları işlemekten her grubuyla bağlayıcı gruplar halinde düzenleyebilirsiniz. Bağlayıcılar Yük Dengelemesi otomatik olarak ve ağ yapınıza en iyi duruma getirme yardımcı olabilir. 

## <a name="requirements-and-deployment"></a>Gereksinimler ve dağıtım

Uygulama proxy'si başarıyla dağıtmak için en az bir bağlayıcı gerekir, ancak iki veya daha fazla bilgi için büyük esneklik öneririz. Windows Server 2012 R2 veya 2016 makinesinin Bağlayıcısı'nı yükleyin. Bağlayıcı yayımladığınız şirket içi uygulamaların yanı sıra uygulama proxy'si hizmeti ile iletişim kurabilmesi gerekir. 

Bağlayıcı sunucusu için ağ gereksinimleri hakkında daha fazla bilgi için bkz: [uygulama proxy'si ile çalışmaya başlama ve bağlayıcıyı yükleme](active-directory-application-proxy-enable.md).

## <a name="maintenance"></a>Bakım
Bağlayıcılar ve hizmet, tüm yüksek kullanılabilirlik görevleri dikkatli olun. Bunlar eklenebilir veya dinamik olarak kaldırılmış. Yeni bir istek geldiğinde her zaman, şu anda kullanılabilir bağlayıcılar birine yönlendirilir. Bağlayıcıyı geçici olarak kullanılamıyor, bu trafiği yanıt vermiyor.

Bağlayıcılar durum bilgisiz ve hiçbir yapılandırma verilerini makinede sahiptir. Depoladıkları veriler yalnızca, hizmet ve kimlik doğrulama sertifikasını bağlanmak için ayarlarıdır. Hizmete bağlandıklarında tüm gerekli yapılandırma verileri çekmek ve her birkaç dakika yenileyin.

Bağlayıcılar da tanımlanmadığını bulmak üzere sunucuyu sorgulamak Bağlayıcısı'nın daha yeni bir sürümü. Bulunması durumunda bağlayıcıları kendilerini güncelleştirin.

Üzerinde çalışan makineden, bağlayıcılar olay günlüğü ve performans sayaçlarını kullanarak izleyebilirsiniz. Veya Azure portalının uygulama proxy'si sayfasından durumlarını görüntüleyebilirsiniz:

 ![Azuread'i uygulama Proxy bağlayıcıları](./media/application-proxy-understand-connectors/app-proxy-connectors.png)

Kullanılmayan bağlayıcıları el ile silmeniz gerekmez. Bir bağlayıcı çalıştırırken, hizmete bağlandığında etkin kalır. Kullanılmayan bağlayıcılar olarak etiketlenmiş _etkin olmayan_ ve kaldıktan sonra 10 gün kaldırılır. Ancak, bir bağlayıcı kaldırmak istiyorsanız, hem bağlayıcı hizmetini hem de güncelleştirici hizmetini sunucudan kaldırın. Hizmeti tam olarak kaldırmak için bilgisayarınızı yeniden başlatın.

## <a name="automatic-updates"></a>Otomatik güncelleştirmeler

Azure AD dağıttığınız tüm bağlayıcıları için otomatik güncelleştirmeler sağlar. Bağlayıcılar Application Proxy Connector Updater çalıştığı sürece, otomatik olarak güncelleştirilir. Sunucunuzda bağlayıcı güncelleştirici hizmetini görmüyorsanız, gerek [Bağlayıcınızı yeniden](active-directory-application-proxy-enable.md) tüm güncelleştirmeleri almak için. 

Bir otomatik güncelleştirme için Bağlayıcınızı gelmesini beklemek istemiyorsanız, el ile yükseltme gerçekleştirebilirsiniz. Git [Bağlayıcısı indirme sayfası](https://download.msappproxy.net/subscription/d3c8b69d-6bf7-42be-a529-3fe9c2e70c90/connector/download) bulunduğu ve select Bağlayıcınızı olduğu sunucuda **karşıdan**. Bu işlem yerel bağlayıcı için bir yükseltme kapalı başlatır. 

Birden çok bağlayıcı ile kiracılar için kapalı kalma süresi, ortamınızdaki önlemek için her grubu zamanında bir bağlayıcı otomatik güncelleştirmeler hedefleyin. 

Bağlayıcınızı varsa güncelleştirdiğinde kapalı kalma süresi karşılaşabilirsiniz:  
- Yalnızca tek bir bağlayıcıyı gerekir. Bu kesinti süresini önler ve yüksek kullanılabilirliğini artırmak için ikinci bağlayıcı yüklemeniz önerilir ve [bağlayıcı grubu oluşturma](active-directory-application-proxy-connectors-azure-portal.md).  
- Güncelleştirme başladığındaki bağlayıcı ortasında bir işlem oluştu. İlk işlem kaybı olmamasına rağmen tarayıcınızı işlemi otomatik olarak yeniden denemeniz gerekir veya sayfanızı yenileyebilirsiniz. İstek yeniden gönderilir, trafik için yedek bir bağlayıcı yönlendirilir.

## <a name="creating-connector-groups"></a>Bağlayıcı grupları oluşturma

Bağlayıcı grupları, belirli uygulamaları sunmaya belirli bağlayıcılar atamak olanak tanır. Bağlayıcıları çeşitli gruplamak ve her uygulama grubuna atayın. 

Bağlayıcı grupları büyük dağıtımları yönetmek kolaylaştırır. Bunlar aynı zamanda gecikme farklı bölgelerde barındırılan uygulamalar yalnızca yerel uygulamalar sunmak için konum temelli bağlayıcı grupları oluşturabileceğinden olan kiracılar için iyileştirir. 

Bağlayıcı grupları hakkında daha fazla bilgi edinmek için [ayrı ağlar ve konumları bağlayıcı gruplarını kullanarak uygulamaları yayımlama](active-directory-application-proxy-connectors-azure-portal.md).

## <a name="capacity-planning"></a>Kapasite Planlaması 

Bağlayıcılar otomatik olarak yük dengelemesi bir bağlayıcı grubundaki, ancak aynı zamanda beklenen trafik hacmi işlemek için bağlayıcılar arasındaki yeterli kapasitesi planladığınızdan emin olmak önemlidir. Genel, sahip olduğunuz daha fazla kullanıcı büyük bir makine ihtiyacınız olacak. Birim ana hattı farklı makinelerde vermiş bir tablo işleyebilir aşağıdadır. Lütfen tüm beklenen işlemleri başına ikinci (TP'ler üzerinde) temel kullanım bu yana bir kullanıcı tarafından desenleri değişir ve yük tahmin etmek için kullanılamaz yerine unutmayın.  Ayrıca, yanıtları ve arka uç uygulama yanıt süresini boyutuna göre bazı farklılıklar olacaktır - büyük yanıt boyutu ve daha yavaş yanıt süresi içinde daha düşük bir maksimum TP'leri sonuçlanır unutmayın.

|Çekirdek|RAM|Gecikme süresi (MS) beklenen-P99|Max TP'leri|
| ----- | ----- | ----- | ----- |
|2|8|325|586|
|4|16|320|1150|
|8|32|270|1190|
|16|64|245|1200*|
\* Bu makineye 800 bağlantı sınırı var. Diğer tüm makineler için varsayılan 200 bağlantı sınırı kullandık.
 
>[!NOTE]
>En fazla TP'leri kadar fark 4, 8 ile 16 çekirdekli makine arasında değil. Bunlar arasındaki başlıca fark beklenen gecikme ' dir.  

## <a name="security-and-networking"></a>Güvenlik ve ağ özellikleri

Bağlayıcılar için uygulama proxy'si hizmet istekleri gönderecek şekilde sağlayan ağ üzerindeki her yerden yüklenebilir. Önemli olan Bağlayıcısı'nı çalıştıran bilgisayar da uygulamalarınıza sahip olur. Bağlayıcılar Kurumsal ağınızın içinde veya bulutta çalışan bir sanal makineye yükleyebilirsiniz. Bağlayıcılar sivil bölge (DMZ) içinde çalıştırabilirsiniz, ancak tüm trafik ağınıza güvenli kalması giden olduğu için ise gerekli değildir.

Bağlayıcılar, yalnızca giden istekleri göndermek. Giden trafik uygulama proxy'si hizmeti ve yayımlanan uygulamalar için gönderilir. Oturum kurulduktan sonra her iki yönde trafik akışlarına olduğundan gelen bağlantı noktalarını açmak zorunda değilsiniz. Yük Dengeleme arasında bağlayıcılar ayarlayın ya da, güvenlik duvarı üzerinden gelen erişimi yapılandırmak gerekmez. 

Giden güvenlik duvarı kuralları yapılandırma hakkında daha fazla bilgi için bkz: [varolan çalışma şirket içi proxy sunucuları](application-proxy-working-with-proxy-servers.md).

Kullanım [Azure AD uygulama Proxy Bağlayıcısı bağlantı noktaları Test aracı](https://aadap-portcheck.connectorporttest.msappproxy.net/) Bağlayıcınızı uygulama proxy'si hizmeti ulaşabilir doğrulanamadı. En azından, Orta ABD bölgesi ve size en yakın bölgeyi tüm yeşil onay işaretli olduğundan emin olun. Bunun ötesinde, daha fazla yeşil onay işaretleri büyük esneklik anlamına gelir. 

## <a name="performance-and-scalability"></a>Performans ve ölçeklenebilirlik

Uygulama proxy'si hizmeti için ölçek saydamdır, ancak ölçek bağlayıcıların bir unsurdur. Yoğun trafik işlemek için yeterli bağlayıcılar olması gerekir. Ancak, bir bağlayıcı grubundaki tüm bağlayıcılar otomatik olarak yük dengelemesi nedeniyle yük dengelemesini yapılandırmak gerekmez.

Bağlayıcılar durum bilgisiz olduğundan, kullanıcılar ya da oturumları sayısına göre etkilenmez. Bunun yerine, isteği sayısını ve bunların yükü boyutu için yanıt. Standart web trafiği ile ortalama bir makine saniyede birkaç bin istekleri işleyebilir. Belirli kapasite tam makine özelliklerine bağlıdır. 

Bağlayıcı performansı, CPU ve ağ ile ilişkilidir. Ağ uygulamaları ve çevrimiçi hizmet hızlı bağlantıyı Azure'da almak önemli olan sırasında CPU performans SSL şifreleme ve şifre çözme, için gereklidir.

Buna karşılık, küçük bir sorun bağlayıcıların bellektir. Çevrimiçi hizmet işleme çoğunu ve tüm kimliği doğrulanmamış trafiği mvc'deki. Bulutta yapılabilir her şeyi bulutta yapılır. 

Yük Dengeleme verilen bağlayıcı Grup bağlayıcılar arasında gerçekleşir. Biz, belirli bir istek hangi bağlayıcı grubunda hizmet belirlemek için bir kez deneme çeşitlemesi yapın. Seçme sonra bir bağlayıcı Biz bu kullanıcıyla oturum süresince uygulama arasında oturum benzeşim korumak. Bağlayıcı veya makine kullanılamaz hale herhangi bir nedenle varsa, trafik, gruptaki başka bir bağlayıcı gidip başlar. Bu esneklik, ayrıca birden çok bağlayıcı olması neden öneririz olur.

Performansı etkileyen başka bir ağ dahil olmak üzere bu bağlayıcıları arasında kalitesini faktördür: 

* **Çevrimiçi hizmet**: yavaş veya Yüksek gecikmeli bağlantılar uygulama proxy'si için hizmet Azure etkisi bağlayıcı performansını. En iyi performans için kuruluşunuzun Azure hızlı rota ile bağlanır. Aksi takdirde, Azure bağlantıları mümkün olduğunca verimli bir şekilde getirilmesine ağ ekibinizin sahip. 
* **Arka uç uygulamaları**: Bazı durumlarda, bağlayıcıyı ve yavaş veya bağlantıları engelle arka uç uygulamalar arasında ek proxy'leri vardır. Bu senaryoda sorun gidermek için bağlayıcı sunucusundan bir tarayıcı açın ve uygulamaya erişmeyi deneyin. Azure'da bağlayıcıları çalıştırın, ancak şirket içi uygulamalardır deneyimi ne kullanıcılarınızın beklediğiniz olmayabilir.
* **Etki alanı denetleyicileri**: bağlayıcıları Kerberos Kısıtlı temsilci kullanarak SSO gerçekleştirirseniz, bunlar etki alanı denetleyicileri arka ucuna isteği göndermeden önce başvurun. Bağlayıcılar Kerberos biletlerinin bir önbelleğe sahip, ancak meşgul bir ortamda etki alanı denetleyicilerinin yanıtlama performansını etkileyebilir. Bu sorunu daha yaygın Azure'da çalıştırın, ancak şirket içi etki alanı denetleyicileriyle iletişim bağlayıcıları için kullanılır. 

Ağınızın en iyi duruma getirme hakkında daha fazla bilgi için bkz: [Azure Active Directory Uygulama proxy'si kullanırken ağ topolojisi hakkında önemli noktalar](application-proxy-network-topology-considerations.md).

## <a name="domain-joining"></a>Etki alanına katılma

Bağlayıcılar olmayan etki alanına katılmış bir makinede çalıştırabilirsiniz. Ancak, tümleşik Windows kimlik doğrulaması (IWA) kullanan uygulamalar için çoklu oturum açma (SSO) isterseniz, etki alanına katılmış bir makine gerekir. Bu durumda, bağlayıcı makineler gerçekleştirebileceğiniz bir etki alanına katılması gerekir [Kerberos](https://web.mit.edu/kerberos) yayımlanan uygulamalar için kullanıcılar adına Kısıtlı temsilci.

Bağlayıcılar, etki alanları veya kısmi güven ormanları ya da salt okunur etki alanı denetleyicileri katılabilir.

## <a name="connector-deployments-on-hardened-environments"></a>Sağlamlaştırılmış ortamlarla bağlayıcı dağıtımları

Genellikle, bağlayıcı dağıtım basittir ve özel yapılandırma gerektirmez. Ancak, düşünülmesi gereken bazı benzersiz koşullar vardır:

* Giden trafiğini sınırlandırmak kuruluşlar gerekir [gerekli bağlantı noktalarını açın](active-directory-application-proxy-enable.md#open-your-ports).
* FIPS uyumlu makineler oluşturmak ve bir sertifika deposu bağlayıcı işlemleri izin vermek için kendi yapılandırmasını değiştirmek için gerekli olabilir.
* Her iki bağlayıcı Hizmetleri tüm gerekli bağlantı noktaları ve IP'leri erişmek için etkinleştirildiğinden emin olmak ağ istekleri işlemleri temel alan ortamlarına kilitleme kuruluşlar sahip.
* Bazı durumlarda, giden iletme proxy'leri iki yönlü sertifika kimlik doğrulaması bölme ve iletişimin başarısız olmasına neden.

## <a name="connector-authentication"></a>Bağlayıcı kimlik doğrulaması

Güvenli bir hizmet sağlamak için doğru bir hizmetin kimliğini bağlayıcılar gerekir ve hizmet bağlayıcı kimlik doğrulaması gerekir. Bu kimlik doğrulaması yapılır bağlayıcıları bağlantı başlattığınızda istemci ve sunucu sertifikaları kullanarak. Bu şekilde bağlayıcı makinede yönetici kullanıcı adı ve parola depolanmaz.

Uygulama proxy'si hizmeti için kullanılan sertifikaları özgüdür. İlk kaydı sırasında oluşturulan ve otomatik olarak bağlayıcılar tarafından her ay birkaç yenilendi. 

Bir bağlayıcı için birkaç ay hizmetine bağlı değilse sertifikalarını güncel olmayabilir. Bu durumda, kaldırın ve tetikleyici kaydını bağlayıcıya yükleyin. Aşağıdaki PowerShell komutları çalıştırabilirsiniz:

```
Import-module AppProxyPSModule
Register-AppProxyConnector
```

## <a name="under-the-hood"></a>Başlık altında

Windows olay günlükleri de dahil olmak üzere aynı Yönetim Araçları'nın çoğu sahip oldukları için bağlayıcıları Windows Server Web uygulama proxy'si üzerinde temel alır

 ![Olay günlükleri Olay Görüntüleyicisi'ni ile yönetme](./media/application-proxy-understand-connectors/event-view-window.png)

ve Windows performans sayaçları. 

 ![Performans İzleyicisi'ni bağlayıcısıyla sayaçları ekleyin](./media/application-proxy-understand-connectors/performance-monitor.png)

Bağlayıcıları yönetici ve oturum sahip günlükleri. Yönetici günlükleri anahtar olayları ve bunların hataları vardır. Oturum günlükleri, tüm işlemler ve işlem ayrıntılarını içerir. 

Günlükleri görmek için açık Olay Görüntüleyici'ye gidin **Görünüm** menü ve Etkinleştir **Göster Analitik ve hata ayıklama günlüklerini**. Daha sonra bunları olaylarını toplamaya başlamak etkinleştirin. Bağlayıcılar sunucudaki daha yeni bir sürüm tabanlı olarak bu günlükler Windows Server 2012 R2'deki Web uygulaması proxy'si görünmez.

Hizmetleri penceresinde hizmetinin durumunu inceleyebilirsiniz. İki Windows Hizmetleri Bağlayıcısı oluşur: Gerçek Bağlayıcısı ve güncelleştirici. Bunların her ikisi de, her zaman çalıştırmanız gerekir.

 ![Azuread'i Hizmetleri yerel](./media/application-proxy-understand-connectors/aad-connector-services.png)

## <a name="next-steps"></a>Sonraki adımlar


* [Ayrı ağlarda ve konumları bağlayıcı gruplarını kullanarak uygulama yayımlama](active-directory-application-proxy-connectors-azure-portal.md)
* [Mevcut şirket içi proxy sunucuları ile çalışma](application-proxy-working-with-proxy-servers.md)
* [Uygulama proxy'si ve bağlayıcı hatalarında sorun giderme](active-directory-application-proxy-troubleshoot.md)
* [Azure AD uygulama ara sunucusu Bağlayıcısı sessiz yükleme](active-directory-application-proxy-silent-installation.md)

