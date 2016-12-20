---
title: "Buluttaki şirket içi kimlik altyapınızı izleyin."
description: "Bu sayfa, Azure AD Connect Health&quot;ın ne olduğunu ve neden kullanacağınızı tanımlar."
services: active-directory
documentationcenter: 
author: karavar
manager: samueld
editor: curtand
ms.assetid: 82798ea6-5cd3-4f30-93ae-d56536f8d8e3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/18/2016
ms.author: vakarand
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: aaab182844b685469cff6a5b692d735482a33952


---
# <a name="monitor-your-on-premises-identity-infrastructure-and-synchronization-services-in-the-cloud"></a>Buluttaki şirket içi kimlik altyapınızı ve eşitleme hizmetlerini izleyin
Azure AD Connect Health, şirket içi kimlik altyapınızı ve eşitleme hizmetlerini izlemenize ve daha iyi kavramanıza yardımcı olur.  AD FS sunucuları, Azure AD Connect sunucuları (diğer adıyla eşitleme altyapısı), Active Directory etki alanı denetleyicileri vb. gibi anahtar kimlik bileşenlerinizi izleme işlevleri sağlayarak Office 365 ve Microsoft Online Services ile güvenilir bir bağlantı sürdürmenizi sağlar. Aynı zamanda, bu bileşenlere ait önemli veri noktalarını kolayca erişilebilir hale getirir. Böylece, bilgiye dayalı kararlar almak için kullanım bilgilerini ve diğer önemli öngörüleri almayı kolaylaştırır.

Bilgiler, [Azure AD Connect Health Portalı](https://aka.ms/aadconnecthealth)'nda size sunulur. Azure AD Connect Health portalını kullanarak uyarıları, performans izlemeyi, kullanım analizini ve daha fazlasını görüntüleyebilirsiniz. Azure AD Connect Health, tüm anahtar kimlik bileşenleriniz için aynı yerde tek sistem durumu odağı sağlar.

![Azure AD Connect Health nedir?](./media/active-directory-aadconnect-health/aadconnecthealth2.png)

Azure AD Connect Health'e ilişkin gelecekte yapılacak olan güncelleştirmeler, ek izleme ve ek kimlik bileşenleri için öngörü içerir. Bu nedenle, kimlik odağı aracılığıyla size tek pano sağlamak; kullanıcılarınızın işleri tamamlama becerilerini artırma avantajından faydalanacakları daha sağlam, sağlıklı ve tümleşik bir ortam sağlar.

## <a name="why-use-azure-ad-connect-health"></a>Azure AD Connect Health neden kullanılır?
Şirket içi dizinlerinizin Azure AD ile tümleştirilmesi, kullanıcılarınızın hem bulut kaynaklarına hem de şirket içi kaynaklara erişmesi için ortak bir kimlik oluşturarak daha üretken olmalarını sağlar. Ancak kullanıcıların herhangi bir cihazdan şirket içi ve buluttaki kaynaklara güvenilir bir şekilde erişmeleri için bu ortamın sağlıklı olup olmadığından emin olma testleri de bu tümleştirmeyle ortaya çıkar. Azure AD Connect Health, Office 365 veya diğer Azure AD uygulamalarına erişmek için kullanılan şirket içi kimlik altyapınızı izlemeniz ve altyapınıza ilişkin öngörüler elde etmeniz için bulut tabanlı kolay bir yaklaşım sağlar. Her bir şirket içi kimlik sunucunuza bir aracı yüklemek kadar basittir.

## <a name="azure-ad-connect-health-for-ad-fsactive-directory-aadconnect-health-adfsmd"></a>[AD FS için Azure AD Connect Health](active-directory-aadconnect-health-adfs.md)
AD FS'ye ilişkin Azure AD Connect Health, Windows Server 2008 R2 üzerinde AD FS 2.0'ı, Windows Server 2012 ve Windows Server 2012R2'de AD FS'yi destekler. Ayrıca, extranet erişimi için kimlik doğrulaması desteği sağlayan AD FS ve Web Uygulaması Ara Sunucularının izlenmesini de destekler. Kolay ve düşük maliyetli durum aracısı kurulumu ile AD FS için Azure AD Connect Health şu önemli özellikleri sağlar:

* AD FS ve AD FS Ara sunucularının sağlıklı olup olmadıklarını öğrenmek için uyarılarla izleme
* Kritik uyarılar için e-posta bildirimleri
* AD FS kapasite planlaması için yararlı olan performans verilerindeki eğilimleri görüntüleme
* Farklı özetlere sahip (uygulamalar, kullanıcılar, ağ konumu vb.) AD FS oturum açma işlemlerine yönelik kullanım analizi, AD FS'nin nasıl kullanıldığını anlama konusunda faydalıdır.
* AD FS raporları (örneğin, son IP adresi ile hatalı Kullanıcı Adı/Parola denemesi yapan ilk 50 kullanıcı)

Aşağıdaki video, AD FS için Azure AD Connect Health hakkında genel bakış sağlar

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-AD-Connect-Health--Monitor-you-identity-bridge/player]
>
>

## <a name="azure-ad-connect-health-for-syncactive-directory-aadconnect-health-syncmd"></a>[Eşitleme için Azure AD Connect Health](active-directory-aadconnect-health-sync.md)
Eşitleme için Azure AD Connect Health, şirket içi Active Directory'niz ve Azure Active Directory'niz arasında oluşan eşitlemeleri izler ve haklarında bilgi verir. Eşitlemeye ilişkin Azure AD Connect Health aşağıdaki temel işlevler kümesini sağlar:

* Azure AD Connect sunucularının (diğer adıyla Eşitleme Altyapısının) sağlıklı olup olmadığını öğrenmek için uyarılarla izleme
* Kritik uyarılar için e-posta bildirimleri
* Ekleme, güncelleştirme, silme gibi farklı işlemlerdeki eğilimlere ve Eşitleme İşlemlerine ilişkin gecikme süresi grafikleri de dahil olmak üzere, eşitleme işlemiyle ilgili öngörüler.
* Eşitleme özelliklerine hızlı bakış bilgileri, Azure AD'ye son başarılı aktarma
* Nesne düzeyinde eşitleme hatalarıyla ilgili raporlar için \(Azure AD Premium gerekli değildir\)

Aşağıdaki video, Eşitleme için Azure AD Connect Health ile ilgili genel bakış sağlar

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-Active-Directory-Connect-Health-Monitoring-the-sync-engine/player]
>
>

## <a name="azure-ad-connect-health-for-ad-ds-previewactive-directory-aadconnect-health-addsmd"></a>[AD DS için Azure AD Connect Health (önizleme)](active-directory-aadconnect-health-adds.md)
AD DS için Azure AD Connect Health, Windows Server 2008 R2, Windows Server 2012 ve Windows Server 2012 R2'de yüklü Etki Alanı Denetleyicileri için izleme olanağı sağlar. Kolay ve düşük maliyetli durum aracısı kurulumu, size şirket içi AD DS ortamınızı doğrudan buluttan izleme olanağı sağlar. AD DS için Azure AD Connect Health aşağıdaki temel işlevler kümesini sağlar:

* Etki alanı denetleyicilerinin iyi durum olmadığı zamanları algılayan, kritik uyarılara ilişkin e-posta bildirimleri ile birlikte izleme uyarıları.
* Etki alanı denetleyicilerinizin durumunu ve işlem durumunu hızlıca görüntülemenizi sağlayan Etki Alanı Denetleyicileri panosu.
* Hatalar algılandığında sorun giderme kılavuzlarının bağlantılarıyla birlikte en son çoğaltma bilgilerini içeren Çoğaltma Durumu panosu.
* Sorun giderme ve izleme için gereken popüler performans sayaçlarının performans veri grafiklerine her yerden hızlı erişim.

Aşağıdaki video, AD DS için Azure AD Connect Health ile ilgili genel bakış sağlar

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-AD-Connect-Health-monitors-on-premises-AD-Domain-Services/player]
>
>

## <a name="get-started-with-azure-ad-connect-health"></a>Azure AD Connect Health ile çalışmaya başlama
Azure AD Connect Health ile çalışmaya başlamak çok kolaydır. Aşağıdaki adımları izleyin:

1. [Azure AD Premium alın](active-directory-get-started-premium.md) veya [deneme sürümünü başlatın](https://azure.microsoft.com/trial/get-started-active-directory/)
2. Kimlik sunucularınıza [Azure AD Connect Health aracılarını İndirme ve Yükleme](#download-and-install-azure-ad-connect-health-agent) 
3. [https://aka.ms/aadconnecthealth](https://aka.ms/aadconnecthealth) adresinde Azure AD Connect Health Panosunu görüntüleme

> [!NOTE]
> Azure AD Connect Health Pano'nuzda herhangi bir veri görmeden önce hedeflenen sunucularınıza Azure Connect Health Aracıları yüklemeniz gerektiğini unutmayın.
>
>

## <a name="download-and-install-azure-ad-connect-health-agent"></a>Azure AD Connect Health Aracılarını İndirme ve Yükleme
* Azure AD Connect Health gereksinimlerini yerine getirdiğinizden emin olun
* AD FS için Azure AD Connect Health ile çalışmaya başlamak için aracının en son sürümünü buradan indirebilirsiniz: [AD FS için Azure AD Connect Health Aracısı İndirme.](http://go.microsoft.com/fwlink/?LinkID=518973)
  [](active-directory-aadconnect-health-agent-install.md#installing-the-azure-ad-connect-health-agent-for-ad-fs)
* Eşitleme için Azure AD Connect Health ile çalışmaya başlamak için [Azure AD Connect'in en son sürümünü](http://go.microsoft.com/fwlink/?linkid=615771) indirin ve yükleyin.  Durum aracısı, Azure AD Connect yüklemesinin bir parçası olarak yüklenir (sürüm 1.0.9125.0 veya daha yeni bir sürüm).  Azure AD Connect, önceki sürümlerinden yerinde yükseltmeyi destekler.
* AD DS için Azure AD Connect Health ile çalışmaya başlamak için aracının en son sürümünü buradan indirebilirsiniz: [AD DS için Azure AD Connect Health Aracısı İndirme.](http://go.microsoft.com/fwlink/?LinkID=820540)
  [](active-directory-aadconnect-health-agent-install.md#installing-the-azure-ad-connect-health-agent-for-ad-fs)

## <a name="azure-ad-connect-health-portal"></a>Azure AD Connect Health Portalı
Azure AD Connect Health portalı; uyarıları, performans izlemeyi ve kullanım analizini görüntülemenizi sağlar. https://aka.ms/aadconnecthealth adresi sizi Azure AD Connect Health'in ana dikey penceresine götürür.  Dikey pencereyi bir pencere olarak düşünebilirsiniz. Ana dikey penceresinde Hızlı Başlangıç'ı, Azure AD Connect Health'teki Hizmetleri ve ek yapılandırma seçeneklerini görebilirsiniz. Ekran görüntüsünün altında bunların kısa açıklamaları vardır.  Aracıları dağıttıktan sonra sistem durumu hizmeti Azure AD Connect Health tarafından hizmetleri otomatik olarak tanımlar.

![Azure AD Connect Health Portalı](./media/active-directory-aadconnect-health/portal4.png)

* **Hızlı Başlangıç** - Bunu seçerek Hızlı Başlangıç dikey penceresini açarsınız. Burada, Araçları Alma'yı seçerek Azure AD Connect Health Aracısı'nı indirebilir, belgelere erişebilir ve geri bildirim edinebilirsiniz.
* **Active Directory Federasyon Hizmetleri** - Azure AD Connect Health'in şu anda izlediği tüm AD FS hizmetlerini temsil eder. Örneklerden birini seçerseniz o hizmet örneği hakkında bilgi içeren dikey pencere açılır.  Bu bilgiler; genel bakışı, özellikleri, uyarıları, izlemeyi ve kullanım analizlerini içerir. İşlevler hakkında daha fazla bilgiyi [buradan](active-directory-aadconnect-health-adfs.md) edinebilirsiniz.
* **Azure Active Directory Connect (Eşitleme)** - Azure AD Connect Health'in şu anda izlediği Azure AD Connect sunucularınızı temsil eder. Girişi seçerseniz Azure AD Connect sunucularınız hakkında bilgi içeren dikey pencere açılır. İşlevler hakkında daha fazla bilgiyi [buradan](active-directory-aadconnect-health-sync.md) edinebilirsiniz.
* **Active Directory Etki Alanı Hizmetleri** - Azure AD Connect Health'in o anda izlediği tüm AD DS ormanlarını temsil eder. Ormanlardan birini seçerseniz o orman hakkında bilgi içeren bir dikey pencere açılır.  Bu bilgiler; temel bilgiler, Etki Alanı Denetleyicileri panosu, Çoğaltma Durumu panosu, uyarılar ve izlemeye genel bakış içerir. İşlevler hakkında daha fazla bilgiyi [buradan](active-directory-aadconnect-health-adds.md) edinebilirsiniz.
* **Yapılandırma** - şunları açmanıza veya kapatmanıza olanak tanır:

  1. Azure AD Connect Health aracısını en son sürüme otomatik olarak güncelleştirmek için otomatik güncelleştirme - Azure AD Connect Health Aracısı kullanılabilir olduğunda otomatik olarak en son sürümüne güncelleştirilir. Bu, varsayılan olarak etkindir.
  2. Microsoft'un Azure AD Directory durum verilerine yalnız sorun gidermek amacıyla erişmesine izin verilmesi- Bu etkinleştirildiğinde Microsoft sizin gördüğünüz verilerin aynılarını görür. Sorunları giderir ve sorunlara yardımcı olur. Bu, varsayılan olarak devre dışıdır.

## <a name="related-links"></a>İlgili bağlantılar
* [Azure AD Connect Health Aracısı Yüklemesi](active-directory-aadconnect-health-agent-install.md)
* [Azure AD Connect Health İşlemleri](active-directory-aadconnect-health-operations.md)
* [Azure AD Connect Health'i AD FS ile Kullanma](active-directory-aadconnect-health-adfs.md)
* [Eşitleme için Azure AD Connect Health'i Kullanma](active-directory-aadconnect-health-sync.md)
* [Azure AD Connect Health'i AD DS ile Kullanma](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect Health ile ilgili SSS](active-directory-aadconnect-health-faq.md)
* [Azure AD Connect Health Sürüm Geçmişi](active-directory-aadconnect-health-version-history.md)



<!--HONumber=Dec16_HO1-->


