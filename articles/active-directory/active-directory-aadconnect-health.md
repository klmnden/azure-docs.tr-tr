<properties
    pageTitle="Buluttaki şirket içi kimlik altyapınızı izleyin."
    description="Bu sayfa, Azure AD Connect Health'ın ne olduğunu ve neden kullanacağınızı tanımlar."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="stevenpo"
    editor="karavar"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="03/21/2016"
    ms.author="vakarand"/>

# Buluttaki şirket içi kimlik altyapınızı ve eşitleme hizmetlerini izleyin

Azure AD Connect Health, şirket içi kimlik altyapınızı ve eşitleme hizmetlerini izlemenize ve daha iyi kavramanıza yardımcı olur.  AD FS sunucuları, Azure AD Connect sunucuları (diğer adıyla eşitleme altyapısı), Active Directory etki alanı denetleyicileri vb. gibi anahtar kimlik bileşenlerinizi izleme işlevleri sağlayarak Office 365 ve Microsoft Online Services ile güvenilir bir bağlantı sürdürmenizi sağlar. Aynı zamanda, kullanıma ve diğer önemli öngörülere ulaşmayı kolaylaştırarak bu bileşenler hakkındaki anahtar veri noktalarını kolayca erişilebilir hale getirir.

Bilgiler, [Azure AD Connect Health Portalı](https://aka.ms/aadconnecthealth)'nda size sunulur. Azure AD Connect Health portalını kullanarak uyarıları, performans izlemeyi, kullanım analizini ve daha fazlasını görüntüleyebilirsiniz. Azure AD Connect Health, tüm anahtar kimlik bileşenleriniz için aynı yerde tek sistem durumu odağı sağlar.

![Azure AD Connect Health nedir?](./media/active-directory-aadconnect-health/aadconnecthealth2.png)

Azure AD Connect Health'e ilişkin gelecekte yapılacak olan güncelleştirmeler, ek izleme ve ek kimlik bileşenleri için öngörü içerir. Bu nedenle, kimlik odağı aracılığıyla size tek pano sağlamak; kullanıcılarınızın işleri tamamlama becerilerini artırma avantajından faydalanacakları daha sağlam, sağlıklı ve tümleşik bir ortam sağlar.

<!-- <center>![What is Azure AD Connect Health](./media/active-directory-aadconnect-health/logo1.png)</center> -->

## Azure AD Connect Health neden kullanılır?

Şirket içi dizinlerinizin Azure AD ile tümleştirilmesi, kullanıcılarınızın hem bulut kaynaklarına hem de şirket içi kaynaklara erişmesi için ortak bir kimlik oluşturarak daha üretken olmalarını sağlar. Ancak kullanıcıların herhangi bir cihazdan şirket içi ve buluttaki kaynaklara güvenilir bir şekilde erişmeleri için bu ortamın sağlıklı olup olmadığından emin olma testleri de bu tümleştirmeyle ortaya çıkar. Azure AD Connect Health, izleme yaklaşımına dayalı basit bir bulut sağlar ve Office 365 veya diğer Azure AD uygulamalarına erişmek için kullanılan şirket içi kimlik altyapınıza öngörü kazandırır. Her bir şirket içi kimlik sunucunuza bir aracı yüklemek kadar basittir.

## [AD FS için Azure AD Connect Health](active-directory-aadconnect-health-adfs.md)

AD FS'ye ilişkin Azure AD Connect Health, Windows Server 2008 R2 üzerinde AD FS 2.0'ı, Windows Server 2012 ve Windows Server 2012R2'de AD FS'yi destekler. Bu, extranet erişimi için kimlik doğrulama desteği sağlayan AD FS Proxy veya Web Uygulama Ara sunucularını da içerir. Durum aracısının çok kolay ve düşük maliyetli olarak yüklenmesi ile AD FS'ye ilişkin Azure AD Connect Health aşağıdaki temel işlevler kümesini sağlar:

- AD FS ve AD FS Ara sunucularının sağlıklı olup olmadıklarını öğrenmek için uyarılarla izleme
- Kritik uyarılar için e-posta bildirimleri
- AD FS kapasite planlaması için yararlı olan performans verilerindeki eğilimleri görüntüleme
- Farklı özetler ile AD FS oturumları açmak için kullanım analizleri (uygulamalar, kullanıcılar, ağ konumu vb.), AD FS kullanımını anlamak için faydalıdır.
- AD FS için raporlar (örneğin, hatalı Kullanıcı Adı/Parola denemesi ile ilk 50 kullanıcı)

Aşağıdaki videoda, AD FS için Azure AD Connect Health'e ilişkin genel bakış sağlanmıştır

>[AZURE.VIDEO azure-ad-connect-health--monitor-you-identity-bridge]

## [Eşitleme için Azure AD Connect Health](active-directory-aadconnect-health-sync.md)
Eşitleme için Azure AD Connect Health, şirket içi Active Directory'niz ve Azure Active Directory'niz arasında oluşan eşitlemeleri izler ve haklarında bilgi verir. Eşitlemeye ilişkin Azure AD Connect Health aşağıdaki temel işlevler kümesini sağlar:

- Azure AD Connect sunucularının (diğer adıyla Eşitleme Altyapısının) sağlıklı olup olmadığını öğrenmek için uyarılarla izleme
- Kritik uyarılar için e-posta bildirimleri
- Eşitleme İşlemleri için gecikme grafikleri ve eşitleme işlemlerinde eğilimler (örneğin, eklemeler, güncelleştirmeler, silinenler) dahil işletimsel öngörüleri eşitleme
- Eşitleme özelliklerine hızlı bakış bilgileri, Azure AD'ye son başarılı aktarma

Aşağıdaki videoda, eşitleme için Azure AD Connect Health'e genel bakış sağlanmıştır

[Azure Active Directory Connect Health: Eşitleme altyapısı izleme](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-Active-Directory-Connect-Health-Monitoring-the-sync-engine)


## Azure AD Connect Health ile çalışmaya başlama
Azure AD Connect Health ile çalışmaya başlamak çok kolaydır. Aşağıdaki adımları izleyin:

1. [Azure AD Premium alın](active-directory-get-started-premium) veya [deneme sürümünü başlatın](https://azure.microsoft.com/trial/get-started-active-directory/)

2. Kimlik sunucularınıza [Azure AD Connect Health aracılarını İndirme ve Yükleme](#download-and-install-azure-ad-connect-health-agent) 

3. [https://aka.ms/aadconnecthealth](https://aka.ms/aadconnecthealth) adresinde Azure AD Connect Health Panosunu görüntüleme

>[AZURE.NOTE]Azure AD Connect Health Pano'nuzda herhangi bir veri görmeden önce hedeflenen sunucularınıza Azure Connect Health Aracıları yüklemeniz gerektiğini unutmayın.

## Azure AD Connect Health Aracılarını İndirme ve Yükleme

- Bkz. Azure AD Connect Health için [Gereksinimler](active-directory-aadconnect-health-agent-install.md#Requirements)

- AD FS için Azure AD Connect Health ile çalışmaya başlamak için aracının en son sürümünü buradan indirebilirsiniz: [AD FS için Azure AD Connect Health Aracısı İndirme.](http://go.microsoft.com/fwlink/?LinkID=518973)
[](active-directory-aadconnect-health-agent-install.md#installing-the-azure-ad-connect-health-agent-for-ad-fs)

- Eşitleme için Azure AD Connect Health ile çalışmaya başlamak için [Azure AD Connect'in en son sürümünü](http://go.microsoft.com/fwlink/?linkid=615771) indirin ve yükleyin.  Durum aracısı, Azure AD Connect yüklemesinin bir parçası olarak yüklenir (sürüm 1.0.9125.0 veya daha yeni bir sürüm).  Azure AD Connect, önceki sürümlerinden yerinde yükseltmeyi destekler.


## Azure AD Connect Health Portalı
Azure AD Connect Health portalı; uyarıları, performans izlemeyi ve kullanım analizini görüntülemenizi sağlar. https://aka.ms/aadconnecthealth adresi sizi Azure AD Connect Health'in ana dikey penceresine götürür.  Dikey pencereyi bir pencere olarak düşünebilirsiniz. Ana dikey penceresinde Hızlı Başlangıç'ı, Azure AD Connect Health'teki Hizmetleri ve ek yapılandırma seçeneklerini görebilirsiniz. Ekran görüntüsünün altında bunların kısa açıklamaları vardır.  Aracılar dağıtıldıktan sonra, Azure AD Connect Health hizmetleri için hizmet tanımlayıcıları izlenir.

![Azure AD Connect Health Portalı](./media/active-directory-aadconnect-health/portal2.png)

- **Hızlı Başlangıç** - Bunu seçerek Hızlı Başlangıç dikey penceresini açarsınız. Burada, Araçları Alma'yı seçerek Azure AD Connect Health Aracısı'nı indirebilir, belgelere erişebilir ve geri bildirim edinebilirsiniz.

- **Active Directory Federasyon Hizmetleri** - Azure AD Connect Health'in şu anda izlediği tüm AD FS hizmetlerini temsil eder. Örneklerden birini seçerseniz o hizmet örneği hakkında bilgi içeren dikey pencere açılır.  Bu bilgiler; genel bakışı, özellikleri, uyarıları, izlemeyi ve kullanım analizlerini içerir. İşlevler hakkında daha fazla bilgiyi [buradan](active-directory-aadconnect-health-adfs.md) edinebilirsiniz.

- **Azure Active Directory Connect (Eşitleme)** - Azure AD Connect Health'in şu anda izlediği Azure AD Connect sunucularınızı temsil eder. Girişi seçerseniz Azure AD Connect sunucularınız hakkında bilgi içeren dikey pencere açılır. İşlevler hakkında daha fazla bilgiyi [buradan](active-directory-aadconnect-health-sync.md) edinebilirsiniz.

- **Yapılandırma** - şunları açmanıza veya kapatmanıza olanak tanır:

    1. Azure AD Connect Health aracısını en son sürüme otomatik olarak güncelleştirmek için otomatik güncelleştirme - Azure AD Connect Health Aracısı kullanılabilir olduğunda otomatik olarak en son sürümüne güncelleştirilir. Bu, varsayılan olarak etkindir.

    2. Microsoft'un Azure AD Directory durum verilerine yalnız sorun gidermek amacıyla erişmesine izin verilmesi- Bu etkinleştirildiğinde Microsoft sizin gördüğünüz verilerin aynılarını görür. Sorunları giderir ve sorunlara yardımcı olur. Bu, varsayılan olarak devre dışıdır.


## İlgili bağlantılar

* [Azure AD Connect Health Aracısı Yüklemesi](active-directory-aadconnect-health-agent-install.md)
* [Azure AD Connect Health İşlemleri](active-directory-aadconnect-health-operations.md)
* [Azure AD Connect Health'i AD FS ile Kullanma](active-directory-aadconnect-health-adfs.md)
* [Eşitleme için Azure AD Connect Health'i kullanma](active-directory-aadconnect-health-sync.md)
* [Azure AD Connect Health ile ilgili SSS](active-directory-aadconnect-health-faq.md)
* [Azure AD Connect Health Sürüm Geçmişi](active-directory-aadconnect-health-version-history.md)



<!---HONumber=Jun16_HO2-->


