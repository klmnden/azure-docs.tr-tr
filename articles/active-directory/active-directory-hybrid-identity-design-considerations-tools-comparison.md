<properties
    pageTitle="Karma Kimlik: Dizin tümleştirme araçları karşılaştırması | Microsoft Azure"
    description="Bu sayfada, dizin tümleştirme için kullanılabilen çeşitli dizin tümleştirme araçlarını karşılaştıran kapsamlı bir tablo sunulmaktadır."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/08/2016"
    ms.author="billmath"/>

# Karma Kimlik dizin tümleştirme araçları karşılaştırması

Yıllar içinde dizin tümleştirme araçları büyüme ve gelişim göstermiştir.  Bu belgenin amacı, bu araçlara yönelik birleştirilmiş bir görünüm ve her birinin içerdiği özelliklere dair bir karşılaştırma sağlamaya yardımcı olmaktır.

<!-- The hardcoded link is a workaround for campaign ids not working in acom links-->

>[AZURE.NOTE] Azure AD Connect, daha önce Dirsync ve AAD Eşitleme olarak yayınlanan bileşenleri ve işlevleri bir araya getirir. Bu araçlar artık tek tek yayınlanmamaktadır ve ilerideki tüm geliştirmeler Azure AD Connect güncelleştirmelerine dahil edilecektir, böylece her zaman en güncel işlevlere nereden ulaşabileceğinizi bileceksiniz.
>
>DirSync ve Azure AD Eşitleme kullanım dışı bırakılmıştır. [Buradan](active-directory-aadconnect-dirsync-deprecated.md) daha fazla bilgi edinebilirsiniz.


Tabloların her biri için aşağıdaki anahtarı kullanın.

●  = Şimdi Kullanılabilir  
GS = Gelecekteki Sürüm  
GÖ = Genel Önizleme  

## Şirket İçi - Bulut Eşitlemesi

| Özellik  | Azure Active Directory Connect  | Azure Active Directory Eşitleme Hizmetleri (AAD Eşitleme) | Azure Active Directory Eşitleme Aracı (DirSync)| Forefront Identity Manager 2010 R2 (FIM) |Microsoft Identity Manager 2016 (MIM)|
| :-------- |:--------:|:--------:|:--------:|:--------:|:--------:
| Tek şirket içi AD ormanına bağlanma | ● | ● | ● | ● |● |
| Birden çok şirket içi AD ormanına bağlanma |●  | ● |  | ● |● |
| Birden çok şirket içi Exchange Kuruluşu'na bağlanma | ● |  |  |  | |
| Tek şirket içi LDAP dizinine bağlanma | GS |  |  | ● |● |
| Birden çok şirket içi LDAP dizinine bağlanma |GS  |  |  | ● |● |
| Şirket içi AD ve şirket içi LDAP dizinlerine bağlanma |GS  |  |  | ● |● |
| Özel sistemlere (SQL, Oracle, MySQL vb.) bağlanma | GS |  |  | ● |● |
| Müşteri tanımlı öznitelikleri (dizin uzantıları) eşitleme | ● |  |  |  |  |
|Şirket içi İK olanağına (SAP, Oracle eBusiness,PeopleSoft) bağlanma| GS |  |  | ● |● |
|FIM eşitleme kurallarını ve şirket içi sistemlere hazırlamaya yönelik bağlayıcıları destekler.|  |  |  | ● |● |

## Bulut - Şirket İçi Eşitlemesi

| Özellik  | Azure Active Directory Connect  | Azure Active Directory Eşitleme Hizmetleri | Azure Active Directory Eşitleme Aracı (DirSync) | Forefront Identity Manager 2010 R2 (FIM) |Microsoft Identity Manager 2016 (MIM)|
| :-------- |:--------:|:--------:|:--------:|:--------:|:--------:
| Cihazlar için geri yazma | ● |  | ● |  ||
| Öznitelik geri yazma (Exchange karma dağıtımı için) | ● | ● | ● | ● |● |
| Kullanıcılar ve gruplar nesneleri için geri yazma |  ●|  | |  ||
| Parola geri yazma (self servis parola sıfırlama (SSPR) ve parola değiştirme işleminden) |  ● | ● |  |  ||



## Kimlik Doğrulama Özelliği Desteği

| Özellik  | Azure Active Directory Connect | Azure Active Directory Eşitleme Hizmetleri | Azure Active Directory Eşitleme Aracı (DirSync) | Forefront Identity Manager 2010 R2 (FIM) |Microsoft Identity Manager 2016 (MIM)|
| :-------- |:--------:|:--------:|:--------:|:--------:|:--------:
| Tek şirket içi AD ormanı için Parola Eşitleme | ● | ● | ● |  ||
| Birden çok şirket içi AD ormanı için Parola Eşitleme |  ●| ● |  |  ||
| Federasyon ile Çoklu Oturum Açma | ● | ● | ● | ● |● |
| Parola geri yazma (SSPR ve parola değiştirme işleminden) |●  | ● |  |  ||



## Kurulum ve Yükleme

| Özellik  | Azure Active Directory Connect  | Azure Active Directory Eşitleme Hizmetleri | Azure Active Directory Eşitleme Aracı (DirSync) | Microsoft Identity Manager 2016 (MIM) |
| :-------- |:--------:|:--------:|:--------:|:--------:
| Etki Alanı Denetleyicisi üzerinde yüklemeyi destekler | ● | ● | ● |  |
| SQL Express kullanarak yüklemeyi destekler | ● | ● | ● |  |
| DirSync'ten kolay yükseltme |● |  |  |  |
| Yönetici Kullanıcı Deneyiminin Windows Server dillerine yerelleştirmesi | ● | ● | ● |  |
|Son kullanıcının kullanıcı deneyiminin Windows Server dillerine yerelleştirmesi| |  |  |● |
| Windows Server 2008 ve Windows Server 2008 R2 desteği | ● Eşitleme içindir, Federasyon için hayır| ● | ●  | ● |
| Windows Server 2012 ve Windows Server 2012 R2 desteği | ● | ● | ● | ● |

## Filtreleme ve Yapılandırma

Özellik  | Azure Active Directory Connect | Azure Active Directory Eşitleme Hizmetleri | Azure Active Directory Eşitleme Aracı (DirSync) | Forefront Identity Manager 2010 R2 (FIM)| Microsoft Identity Manager 2016 (MIM)
:-------- |:--------:|:--------:|:--------:|:--------:|:--------:|
Etki Alanları ve Kuruluş Birimleri üzerinde filtreleme | ● | ● | ● | ●  | ●
Nesnelerin öznitelik değerleri üzerinde filtreleme | ● | ● | ● | ●| ●
Minimal öznitelik kümesinin eşitlenmesine izin ver (MinSync) | ● | ● |  ||
Öznitelik akışları için farklı hizmet şablonlarının uygulanmasına izin ver |●  | ● |  ||
Öznitelik kaldırma işleminin AD'den Azure AD'ye akışına izin ver | ● | ● |  |  |
Öznitelik akışları için gelişmiş özelleştirmeye izin ver | ● | ● |  | ●  | ●

## Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md) hakkında daha fazla bilgi edinin.



<!--HONumber=Aug16_HO4-->


