<properties
   pageTitle="OneDrive İş İle Azure RemoteApp’i tümleştirme | Microsoft Azure"
   description="OneDrive İş’i Azure RemoteApp ile kullanmayı öğrenin."
   services="remoteapp"
   documentationCenter=""
   authors="pavithir"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="03/31/2016"
   ms.author="elizapo"/>

# OneDrive İş İle Azure RemoteApp’i tümleştirme

OneDrive İş’i Azure RemoteApp ile bir dosya deposu olarak kullanabilirsiniz. OneDrive İş dosyaları tüm cihazlarınız ve çalışma alanlarınızda eşitlenmiş olarak tutmanın harika bir yoludur. Kullanıcının [kullanıcı profili diski](remoteapp-upd.md) (UPD), kullanıcıların Azure RemoteApp uygulamaları için dosyalarını depolayabilecekleri bir yerdir, ancak bu dosyalara yalnızca Azure RemoteApp kullanarak erişilebilir. OneDrive İş ise kullanıcının Azure RemoteApp kullanması ön koşulu olmadan, istediği zaman istediği yerden dosyalarına erişmesini sağlar. Bu makale, desteklene OneDrive İş sürümlerini ve yöneticilerin Azure RemoteApp için OneDrive İş kurmalarının farklı yöntemlerini el alacaktır.

## OneDrive’ın tüm sürümleri destekleniyor mu?

OneDrive’ın iki sürümü vardır: OneDrive ve OneDrive İş. Azure RemoteApp’te yalnızca OneDrive İş desteklenir. Kişisel OneDrive çalışır ancak resmi olarak desteklenmez. Ayrıca, yalnızca OneDrive İş’in, diğer adıyla Yeni Nesil Eşitleme istemcisi, son sürümü Azure RemoteApp’te (ve RDSH/Citrix/Terminal sunucuları) desteklenir.

>[AZURE.NOTE]  OneDrive (kullanıcılar için/kişisel sürüm için) Azure RemoteApp’te desteklenmez. Windows Server’da çalışmak üzere yetkilendirilmediklerinden, OneDrive İş’in tüm sürümleri de desteklenmez. Her yeni istemci (Yeni Nesil Eşitleme İstemcisi) hem eski Groove sürümleri Azure RemoteApp’te sorunsuz çalışıyor görünmekle birlikte, [https://support.microsoft.com/en-us/kb/2965687](https://support.microsoft.com/kb/2965687) makalesinden açıklandığı gibi, eski eşitleme altyapıları Citrix’te / Terminal Sunucuları’nda (Windows Server) tam işlev göstermez.

## OneDrive İş için farklı kurulum seçenekleri nelerdir?

- **OneDrive İş eşitleme altyapısının geleneksel kurulumu:** Bu seçenek Azure RemoteApp, RDSH, Citrix dağıtımlarında şu anda desteklenmemektedir.
- **OneDrive İş’i “Görselleştirme”/yerel güçlü istemcisinden oturuma yönlendirme:** OneDrive’ı istemci cihazındaki sürücü altındaki klasöre eşitliyorsanız, bu sürücüyü Azure RemoteApp’e [yönlendirmeyi](remoteapp-redirection.md) seçebilirsiniz; bu sürücü kullanıcılarınızın tüm istemcilerinde aynı olmalı ve bunlar bu sürücü altında bir klasöre eşitlenmiş OneDrive sahibi olmalıdır. RemoteApp’e başka bir istemciden erişiyorlarsa, bu dosyaların kullanılamaması mümkündür (çözüm: dosyalarına OneDrive’ın çevrimiçi sürümünü kullanarak her zaman erişebilirler) 
- **Dosyaları önbelleğe almadan/eşitlemeden OneDrive’ı Azure RemoteApp ortamında bir sürücü olarak sunma:** (OneDrive İş http URL’sini sanal makinedeki bir sürücüye eşleme) OneDrive İş’i RDSH ortamındaki bir ağ sürücüsüne eşleme desteklenir. Bunun kullanılabileceği senaryolar şöyledir: 
    - Azure RemoteApp’e erişim için ince istemciler (yerel depolama olmadan) kullanıldığında: Uygulama dosyaların OneDrive İş’te depolanmasını gerektirir, ancak yerel “görünmesini” istiyor ve yönetici dosyaları bir sanal makineye eşitlemek istemiyor.
    - OneDrive İş’te eşitleme için seçilen birçok büyük dosya olduğunda. Eşitlemenin iş yüküne bağlı olarak, kullanıcı kullanmak istediğinde tüm dosyalar eşitlenmeyebilir. Ayrıca dosyaların toplam boyutu 50 GB’ı aşarsa, bunlar UPD’de depolanamaz.

Yukarıdaki senaryolarda, yöneticiler sanal makinedeki bir sürücüyü kullanıcının OneDrive İş http URL’sine eşlemeyi tercih edebilir. Bunu etkinleştirmenin bazı seçenekleri şunlardır:

- Görüntüde Office ikilileri sahibi olma ve OneDrive İş eşitleme altyapısını etkinleştirmeme.
- Görüntüde Office ikilileri sahibi OLMAMA: Bu örnek bir ön koşul gerektirir: Masaüstü Deneyimi Paketi. Özellikle, WebClient hizmetinin (diğer adıyla WebDAV) Masaüstü Deneyimi Paketi’nin bir parçası olarak yüklü olması gerekir. 

### Windows Server 2012 R2'de Masaüstü Deneyimi Paketi’ni Yükleme
Masaüstü Deneyimi Paketi’ni yüklemek için: 

1. Sunucu Yöneticisi'nde **Yönet -> Rol ve Özellik Ekle**’ye tıklayın.
2. **Özellikleri**’e ve ardından **Kullanıcı Arabirimleri ve Altyapısı ->Masaüstü Deneyimi**’ne tıklayın.

### Bir sürücüyü OneDrive İş URL’sine eşleme

[https://support.microsoft.com/kb/2616712](https://support.microsoft.com/kb/2616712) destek makalesindeki yönergeleri izleyin.
 
Kurulumdaki önemli bir adım**oturumumu açık bırak** seçeneği seçtiğinizden emin olmanızdır.

Bir üst düzeyde, yönergeler şöyledir:

1.  Sürücü için URL’yi bulun. Sürücü eşleme için kullanılan URL, OneDrive İş çevrimiçinde giriş dizinine gittiğinizde aldığınızdır. Örneğin:
 
    https://microsoft-my.sharepoint.com/personal/alias_microsoft_com/_layouts/15/onedrive.aspx?AjaxDelta=1&isStartPlt1=something
2.  Bir tarayıcı kullanarak RemoteApp oturumunda URL'yi açın ve hesabınız için URL’de oturum açmadan önce **oturumumu açık bırak** seçeneğini seçin.
3.  Windows Gezgini'ni açın ve bir sürücüyü için yukarıdaki URL’ye eşleyin. URL çözümlenmezse, kısa biçimi de kullanılabilir:
    
    https://microsoft-my.sharepoint.com/personal/alias_microsoft_com. 

    Bu eşlenen sürücüyü hemen oluşturur; şöyle görünür:
 
    ![Eşlenen ağ sürücüsü olarak OneDrive İş](./media/remoteapp-onedrive/ra-mappeddrive.png)


<!----HONumber=Jun16_HO2-->


