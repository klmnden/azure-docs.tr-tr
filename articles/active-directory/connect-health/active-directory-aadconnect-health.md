---
title: "Buluttaki şirket içi kimlik altyapınızı izleyin."
description: "Bu sayfa, Azure AD Connect Health'ın ne olduğunu ve neden kullanacağınızı tanımlar."
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
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 881ce13b6e4b10064294e590431434b29da3fb33
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="monitor-your-on-premises-identity-infrastructure-and-synchronization-services-in-the-cloud"></a>Buluttaki şirket içi kimlik altyapınızı ve eşitleme hizmetlerini izleyin
Azure Active Directory (Azure AD) Connect Health, şirket içi kimlik altyapınızı ve eşitleme hizmetlerini izlemenize ve daha iyi kavramanıza yardımcı olur. Active Directory Federasyon Hizmetleri (AD FS) sunucuları, Azure AD Connect sunucuları (diğer adıyla eşitleme altyapısı), Active Directory etki alanı denetleyicileri gibi anahtar kimlik bileşenlerinizi izleme işlevleri sağlayarak Office 365 ve Microsoft Online Services ile güvenilir bir bağlantı sürdürmenizi sağlar. Aynı zamanda, bu bileşenlere ait önemli veri noktalarını kolayca erişilebilir hale getirir. Böylece, bilgiye dayalı kararlar almak için kullanım bilgilerini ve diğer önemli öngörüleri almayı kolaylaştırır.

Bilgiler, [Azure AD Connect Health Portalı](https://aka.ms/aadconnecthealth)'nda sunulur. Azure AD Connect Health portalında uyarıları, performans izlemeyi, kullanım analizini ve daha fazla bilgiyi görüntüleyebilirsiniz. Azure AD Connect Health, anahtar kimlik bileşenleriniz için aynı yerde tek sistem durumu odağı sağlar.

![Azure AD Connect Health nedir?](./media/active-directory-aadconnect-health/aadconnecthealth2.png)

Azure AD Connect Health özellikleri arttıkça portal, kimlik odaklı tek bir pano sunar. Kullanıcılarınızın iş yapma olanaklarını artırmanızı sağlayacak daha sağlam, iyi durumda ve tümleşik bir ortam sağlar.

## <a name="why-use-azure-ad-connect-health"></a>Azure AD Connect Health neden kullanılır?
Şirket içi dizinlerinizi Azure AD ile tümleştirdiğinizde, kullanıcılarınızın hem bulut kaynaklarına hem de şirket içi kaynaklara erişmesi için ortak bir kimlik oluşturulur ve kullanıcıların daha üretken olmaları sağlanır. Ancak kullanıcıların herhangi bir cihazdan şirket içi ve buluttaki kaynaklara güvenilir bir şekilde erişmeleri için bu ortamın sağlıklı olup olmadığından emin olma testleri de bu tümleştirmeyle oluşturulur. Azure AD Connect Health, Office 365 veya diğer Azure AD uygulamalarına erişmek için kullanılan şirket içi kimlik altyapınızı izlemeniz ve altyapınıza ilişkin öngörüler elde etmenize yardımcı olur. Her bir şirket içi kimlik sunucunuza bir aracı yüklemek kadar basittir.

## <a name="azure-ad-connect-health-for-ad-fsactive-directory-aadconnect-health-adfsmd"></a>[AD FS için Azure AD Connect Health](active-directory-aadconnect-health-adfs.md)
AD FS'ye ilişkin Azure AD Connect Health Windows Server 2008 R2, Windows Server 2012 ve Windows Server 2012 R2 üzerinde AD FS 2.0'ı destekler. Ayrıca, extranet erişimi için kimlik doğrulaması desteği sağlayan AD FS ve web uygulaması ara sunucularının izlenmesini de destekler. Kolay ve düşük maliyetli Durum Aracısı kurulumu ile AD FS için Azure AD Connect Health şu önemli özellikleri sağlar:

* AD FS ve AD FS ara sunucularının sağlıklı olup olmadıklarını öğrenmek için uyarılarla izleme
* Kritik uyarılar için e-posta bildirimleri
* AD FS kapasite planlaması için yararlı olan performans verilerindeki eğilimler
* AD FS'nin nasıl kullanıldığını anlama konusunda yararlı olan özetlere sahip (uygulamalar, kullanıcılar, ağ konumu vb.) AD FS oturum açma işlemlerine yönelik kullanım analizi
* Son IP adresi ile hatalı kullanıcı adı/parola denemesi yapan ilk 50 kullanıcı gibi AD FS raporları

Aşağıdaki video, AD FS için Azure AD Connect Health hakkında genel bakış sağlar.

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-AD-Connect-Health--Monitor-you-identity-bridge/player]
>
>

## <a name="azure-ad-connect-health-for-syncactive-directory-aadconnect-health-syncmd"></a>[Eşitleme için Azure AD Connect Health](active-directory-aadconnect-health-sync.md)
Eşitleme için Azure AD Connect Health, şirket içi Active Directory'niz ve Azure AD'niz arasında oluşan eşitlemeleri izler ve haklarında bilgi verir. Eşitleme için Azure AD Connect Health aşağıdaki temel işlevler kümesini sağlar:

* Azure AD Connect sunucularının (diğer adıyla Eşitleme Altyapısının) sağlıklı olup olmadığını öğrenmek için uyarılarla izleme
* Kritik uyarılar için e-posta bildirimleri
* Ekleme, güncelleştirme, silme gibi farklı işlemlerdeki eğilimlere ve eşitleme işlemlerine ilişkin gecikme süresi grafikleri de dahil olmak üzere, eşitlemeyle ilgili operasyonel öngörüler
* Eşitleme özelliklerine hızlı bakış bilgileri ve Azure AD'ye son başarılı aktarma
* Nesne düzeyinde eşitleme hatalarıyla ilgili raporlar için \(Azure AD Premium gerekli değildir\)

Aşağıdaki video, Eşitleme için Azure AD Connect Health ile ilgili genel bakış sağlar.

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-Active-Directory-Connect-Health-Monitoring-the-sync-engine/player]
>
>

## <a name="azure-ad-connect-health-for-ad-dsactive-directory-aadconnect-health-addsmd"></a>[AD DS için Azure AD Connect Health](active-directory-aadconnect-health-adds.md)
Active Directory Domain Services (AD DS) için Azure AD Connect Health, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 ve Windows Server 2016'da yüklü etki alanı denetleyicileri için izleme olanağı sağlar. Durum Aracısı kurulumu, size şirket içi AD DS ortamınızı buluttan izleme olanağı sağlar. AD DS için Azure AD Connect Health aşağıdaki temel işlevler kümesini sağlar:

* Etki alanı denetleyicilerinin iyi durum olmadığı zamanları algılayan, kritik uyarılara ilişkin e-posta bildirimleri ve izleme uyarıları
* Etki alanı denetleyicilerinizin durumunu ve işlem durumunu hızlıca görüntülemenizi sağlayan Etki Alanı Denetleyicileri panosu
* Hatalar algılandığında sorun giderme kılavuzlarının bağlantılarını ve en son çoğaltma bilgilerini içeren Çoğaltma Durumu panosu
* Sorun giderme ve izleme için gerekli olan popüler performans sayaçlarının performans veri grafiklerine her yerden hızlı erişim

Aşağıdaki video, AD DS için Azure AD Connect Health ile ilgili genel bakış sağlar.

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-AD-Connect-Health-monitors-on-premises-AD-Domain-Services/player]
>
>

## <a name="get-started-with-azure-ad-connect-health"></a>Azure AD Connect Health ile çalışmaya başlama
Azure AD Connect Health ile çalışmaya başlamak için aşağıdaki adımları kullanın:

1. [Azure AD Premium alın](../active-directory-get-started-premium.md) veya [deneme sürümünü başlatın](https://azure.microsoft.com/trial/get-started-active-directory/).
2. Kimlik sunucularınıza [Azure AD Connect Health Aracılarını indirin ve yükleyin](#download-and-install-azure-ad-connect-health-agent).
3. [https://aka.ms/aadconnecthealth](https://aka.ms/aadconnecthealth) adresinde Azure AD Connect Health Panosunu görüntüleyin.

> [!NOTE]
> Azure AD Connect Health panonuzda veri görmeden önce hedeflenen sunucularınıza Azure Connect Health Aracıları yüklemeniz gerektiğini unutmayın.
>
>

## <a name="download-and-install-azure-ad-connect-health-agent"></a>Azure AD Connect Health Aracısını indirme ve yükleme
* Azure AD Connect Health [gereksinimlerini yerine getirdiğinizden](active-directory-aadconnect-health-agent-install.md#requirements) emin olun.
* AD FS için Azure AD Connect Health kullanmaya başlama
    * [AD FS için Azure AD Connect Health Aracısını indirin.](http://go.microsoft.com/fwlink/?LinkID=518973)
    * [Yükleme talimatlarına bakın](active-directory-aadconnect-health-agent-install.md#installing-the-azure-ad-connect-health-agent-for-ad-fs).
* Eşitleme için Azure AD Connect Health kullanmaya başlama
    * [Azure AD Connect'in en son sürümünü indirip yükleyin](http://go.microsoft.com/fwlink/?linkid=615771). Eşitleme için Durum Aracısı, Azure AD Connect yüklemesinin bir parçası olarak yüklenir (sürüm 1.0.9125.0 veya daha yeni bir sürüm).
* AD DS için Azure AD Connect Health kullanmaya başlama
    * [AD DS için Azure AD Connect Health Aracısını indirin](http://go.microsoft.com/fwlink/?LinkID=820540).
    * [Yükleme talimatlarına bakın](active-directory-aadconnect-health-agent-install.md#installing-the-azure-ad-connect-health-agent-for-ad-ds).

## <a name="azure-ad-connect-health-portal"></a>Azure AD Connect Health portalı
Azure AD Connect Health portalı; uyarıları, performans izlemeyi ve kullanım analizini görüntüler. https://aka.ms/aadconnecthealth URL'si sizi Azure AD Connect Health'in ana dikey penceresine götürür. Dikey pencereyi bir pencere olarak düşünebilirsiniz. Ana dikey pencerede **Hızlı Başlangıç**'ı, Azure AD Connect Health'teki hizmetleri ve ek yapılandırma seçeneklerini görebilirsiniz. Aşağıdaki ekran görüntüsünü ve altındaki kısa açıklamaları inceleyin. Aracıları dağıttıktan sonra sistem durumu hizmeti Azure AD Connect Health tarafından izlenen hizmetleri otomatik olarak tanımlar.

> [!NOTE]
> Lisans bilgileri için bkz. [Azure AD Connect SSS](active-directory-aadconnect-health-faq.md) veya [Azure AD Fiyatlandırma sayfası](https://aka.ms/aadpricing).
    
![Azure AD Connect Health Portalı](./media/active-directory-aadconnect-health/portal4.png)

* **Hızlı Başlangıç**: Bu seçeneği belirlediğinizde **Hızlı Başlangıç** dikey penceresi açılır. **Araçları Edinme**'yi seçerek Azure AD Connect Health Aracısı'nı indirebilirsiniz. Ayrıca, belgelere erişebilir ve geri bildirim gönderebilirsiniz.
* **Active Directory Federasyon Hizmetleri**: Bu seçenek, Azure AD Connect Health'in o anda izlediği tüm AD FS hizmetlerini gösterir. Örneklerden birini seçtiğinizde açılan dikey pencerede seçtiğiniz hizmet örneğiyle ilgili bilgiler gösterilir. Bu bilgiler; genel bakışı, özellikleri, uyarıları, izlemeyi ve kullanım analizlerini içerir. Özellikler hakkında daha fazla bilgi için bkz. [Azure AD Connect Health'i AD FS ile kullanma](active-directory-aadconnect-health-adfs.md).
* **Azure Active Directory Connect (eşitleme)**: Bu seçenek, Azure AD Connect Health'in o anda izlediği Azure AD Connect sunucularınızı gösterir. Girişi seçtiğinizde açılan dikey pencerede Azure AD Connect sunucularınız hakkında bilgiler gösterilir. Özellikler hakkında daha fazla bilgi için bkz. [Eşitleme için Azure AD Connect Health'i kullanma](active-directory-aadconnect-health-sync.md).
* **Active Directory Domain Services**: Bu seçenek, Azure AD Connect Health'in o anda izlediği tüm AD DS ormanlarını gösterir. Ormanlardan birini seçtiğinizde açılan dikey pencerede seçtiğiniz ormanla ilgili bilgiler gösterilir. Bu bilgiler; temel bilgiler, Etki Alanı Denetleyicileri panosu, Çoğaltma Durumu panosu, uyarılar ve izlemeye genel bakış içerir. Özellikler hakkında daha fazla bilgi için bkz. [Azure AD Connect Health'i AD DS ile kullanma](active-directory-aadconnect-health-adds.md).
* **Yapılandırma**: Bu bölümde aşağıdakileri açma veya kapatma seçenekleri bulunur:

  - Azure AD Connect Health aracısını en son sürüme otomatik olarak güncelleştirmek için otomatik güncelleştirme: Azure AD Connect Health Aracısı kullanılabilir olduğunda otomatik olarak en son sürümüne güncelleştirilir. Bu, varsayılan olarak etkindir.
  - Microsoft'un Azure AD Directory durum verilerine yalnız sorun gidermek amacıyla erişmesine izin verilmesi: Bu etkinleştirildiğinde Microsoft sizin gördüğünüz verilerin aynılarını görür. Bu bilgiler sorun giderme konusunda yardımcı olabilir. Bu, varsayılan olarak devre dışıdır.

## <a name="related-links"></a>İlgili bağlantılar
* [Azure AD Connect Health Aracısı yüklemesi](active-directory-aadconnect-health-agent-install.md)
* [Azure AD Connect Health işlemleri](active-directory-aadconnect-health-operations.md)
* [Azure AD Connect Health'i AD FS ile Kullanma](active-directory-aadconnect-health-adfs.md)
* [Eşitleme için Azure AD Connect Health'i kullanma](active-directory-aadconnect-health-sync.md)
* [Azure AD Connect Health'i AD DS ile Kullanma](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect Health ile ilgili SSS](active-directory-aadconnect-health-faq.md)
* [Azure AD Connect Health sürüm geçmişi](active-directory-aadconnect-health-version-history.md)
