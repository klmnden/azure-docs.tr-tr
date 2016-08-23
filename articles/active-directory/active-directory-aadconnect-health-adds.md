
<properties
    pageTitle="Azure AD Connect Health'i AD DS ile Kullanma| Microsoft Azure"
    description="Bu Azure AD Connect Health sayfasında AD DS’nin nasıl izleneceği ele alınmaktadır."
    services="active-directory"
    documentationCenter=""
    authors="arluca"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/14/2016"
    ms.author="arluca"/>

# Azure AD Connect Health'i AD DS ile Kullanma
Aşağıdaki belgeler Azure AD Connect Health ile Active Directory Etki Alanı Hizmetleri’nin izlenmesine özgüdür. Buna Windows Server 2008 R2, Windows Server 2012 ve Windows Server 2012 R2’ye yüklü AD DS dahildir.

Azure Connect Health ile AD FS'yi izleme hakkında bilgi almak için bkz. [Azure AD Connect Health'i AD FS ile kullanma](active-directory-aadconnect-health-adfs.md). Ek olarak, Azure AD Connect’i (Eşitleme) Azure AD Connect Health ile izleme hakkında bilgi için bkz. [Eşitleme için Azure AD Connect Health Kullanma](active-directory-aadconnect-health-sync.md).

![AD DS için Azure AD Connect Health](./media/active-directory-aadconnect-health/aadconnect-health-adds-entry.png)

## AD DS için Azure AD Connect Health uyarıları
AD DS için Azure AD Connect Health içindeki Uyarılar bölümünde, Etki alanı Denetleyicileriniz ile ilgili olarak etkin ve çözümlenmiş uyarıların bir listesi verilir. Etkin veya çözümlenmiş bir uyarının seçilmesi, çözüm adımlarıyla birlikte ek bilgileri ve destekleyici belgelerin bağlantılarını içeren yeni bir dikey pencere açar. Her uyarı türünün, ilgili uyarıdan etkilenen etki alanı denetleyicilerinin her birine karşılık gelen bir veya daha fazla örneği olabilir. Uyarı dikey penceresinin alt kısmında etkilenen bir etki alanı denetleyicisi seçtiğinizde ilgili uyarı örneğinin ayrıntılarını içeren yeni bir dikey pencere açılır.

Bu dikey pencerede uyarılar için e-posta bildirimlerinin etkinleştirilmesi ve görünümdeki saat aralığının değiştirilmesi mümkündür. Zaman aralığının genişletilmesi daha önce çözümlenen uyarıları görmenize imkan tanır.

![Azure AD Connect eşitleme hatası](./media/active-directory-aadconnect-health/aadconnect-health-adds-alerts.png)

## Etki Alanı Denetleyicileri
Bu pano teme çalışma ölçümleri ve izlenen etki alanlarınızın her birinin sistem durumu ile birlikte ortamınızın topolojik bir görünümünü sağlar. Sunulan ölçümler daha fazla araştırma gerektirebilecek herhangi bir etki alanı denetleyicisini hızlıca tanımlamanıza yardımcı olur. Varsayılan olarak yalnızca bir sütun alt kümesi görüntülenir; ancak sütunlar komutuna tıklayarak tüm kullanılabilir sütunları bulabilirsiniz. En fazla önemsediğiniz sütunların seçilmesi bu panoyu AD DS ortamınızın durumunu görüntülemek için tek ve kolay bir yer haline getirir. 

![Etki Alanı Denetleyicileri](./media/active-directory-aadconnect-health/aadconnect-health-adds-domainsandsites-dashboard.png)

Etki Alanı Denetleyicileri ilgili etki alanlarına ya da sitelerine göre gruplandırılabilir; bunun yapılması ortam topolojisinin anlaşılması için yararlıdır. Son olarak, dikey pencere üst bilgisine çift tıklarsanız pano mevcut ekrandan yararlanmak üzere genişletilir. Bunun yapılması özellikle birkaç sütun görüntülediğinizde yararlı olabilir. 

## Çoğaltma Durumu
Bu pano, izlenen etki alanı denetleyicilerinizin çoğaltma durumuna ve çoğaltma topolojisine ilişkin görünümü sağlar. En son çoğaltma girişiminin durumu, bulunan herhangi bir hataya ilişkin yararlı belgelerle birlikte listelenir. Hata içeren bir etki alanı denetleyicisinin seçilmesi çözüm adımlarıyla birlikte ek bilgileri ve sorun giderme belgelerinin bağlantılarını içeren yeni bir dikey pencere açar. 

![Çoğaltma Durumu](./media/active-directory-aadconnect-health/aadconnect-health-adds-replication.png)

## İzleme
Bu özellik, izlenen her bir etki alanından sürekli olarak toplanan farklı performans sayaçlarının grafik eğilimlerini verir. Bir etki alanı denetleyicisinin performansı, ormanınızdaki diğer tüm izlenen etki alanı denetleyicileri arasında kolayca karşılaştırılabilir. Ayrıca, ortamınızdaki sorunları gidermede yardımcı olmak üzere çeşitli performans sayaçlarını yan yana görebilirsiniz. 

![İzleme](./media/active-directory-aadconnect-health/aadconnect-health-adds-monitoring.png)

Varsayılan olarak, dört performans sayacı önceden seçilmiştir; ancak, filtre komutuna tıklayarak ve istediğiniz performans sayaçlarını seçerek ya da seçimini kaldırarak başkalarını ekleyebilirsiniz. Ayrıca, belirli bir performans sayacı grafiğine tıklarsanız izlenen etki alanı denetleyicilerinin her birine ait veri noktalarını içeren yeni bir dikey pencere açılır.

## İlgili bağlantılar

* [Azure AD Connect Health](active-directory-aadconnect-health.md)
* [Azure AD Connect Health Aracısı Yüklemesi](active-directory-aadconnect-health-agent-install.md)
* [Azure AD Connect Health İşlemleri](active-directory-aadconnect-health-operations.md)
* [Azure AD Connect Health'i AD FS ile Kullanma](active-directory-aadconnect-health-adfs.md)
* [Eşitleme için Azure AD Connect Health'i kullanma](active-directory-aadconnect-health-sync.md)
* [Azure AD Connect Health ile ilgili SSS](active-directory-aadconnect-health-faq.md)
* [Azure AD Connect Health Sürüm Geçmişi](active-directory-aadconnect-health-version-history.md)



<!--HONumber=Aug16_HO1-->


