---
title: Azure MFA sürümleri ve tüketim planları | Microsoft Docs
description: Çok faktörlü kimlik doğrulaması istemci farklı yöntemler ve kullanılabilir sürümler hakkında bilgi.
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 08/25/2017
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: richagi
ms.openlocfilehash: ec1a3e7e3e8969e161854a9f386d4b2e767ee48a
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38970425"
---
# <a name="how-to-get-azure-multi-factor-authentication"></a>Azure multi-Factor Authentication'ı edinme

İki aşamalı doğrulama hesaplarınızı korumak için söz konusu olduğunda, kuruluşunuz genelinde standart olması gerekir. Bu özellik, kaynaklara erişimi ayrıcalıklı yönetim hesapları için özellikle önemlidir. Bu nedenle, Azure yöneticileri için herhangi bir ek maliyet ve Microsoft Office 365'e temel iki aşamalı doğrulama özellikleri sunar. Yöneticilerinizin özellikleri yükseltme veya geri kalanı kullanıcılarınız için iki aşamalı doğrulamayı genişletmek istiyorsanız, Azure multi-Factor Authentication satın alabilirsiniz. 

Bu makalede, Yöneticiler ve tam Azure mfa'yı sürüm için sunulan sürümler arasındaki farkı açıklar. Teklifi tüm Azure MFA dağıtmak hazırsanız, sonraki bölümde uygulama seçenekleri ve Microsoft tüketim nasıl hesaplar ele alınmaktadır.


>[!IMPORTANT]
>Bu makalede, Azure multi-Factor Authentication'ı satın almak için farklı yollarını anlamanıza yardımcı olması için bir kılavuz olarak tasarlanmıştır. Fiyatlandırma ve faturalandırma hakkında belirli Ayrıntılar için her zaman başvurmanız gerekir [multi-Factor Authentication fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/multi-factor-authentication/).

## <a name="available-versions-of-azure-multi-factor-authentication"></a>Azure multi-Factor Authentication'ın kullanılabilir sürümleri

Aşağıdaki tabloda, çok faktörlü kimlik doğrulaması için üç sürümü arasındaki farklar açıklanmaktadır:

| Sürüm | Açıklama |
| --- | --- |
| Office 365 için Multi-Factor Authentication |Bu sürümü, özel olarak Office 365 uygulamalarıyla çalışır ve Office 365 portalından yönetilir. Yöneticiler, [Office 365 kaynakları iki aşamalı doğrulama ile güvenli hale getirme](https://support.office.com/article/Set-up-multi-factor-authentication-for-Office-365-users-8f0454b2-f51a-4d9c-bcde-2c48e41621c6). Bu sürümü, bir Office 365 aboneliğinizin bir parçasıdır. |
| Azure AD yöneticileri için multi-Factor Authentication | Azure AD kiracıları içinde genel Yönetici rolüne atanan kullanıcılar hiçbir ek ücret ödemeden Azure AD genel yönetici hesapları için iki aşamalı doğrulamayı etkinleştirebilirsiniz.|
| Azure Multi-Factor Authentication | Genellikle için "tam" sürüm olarak adlandırılan, Azure multi-Factor Authentication en zengin özellik kümesi sunar. Aracılığıyla ek yapılandırma seçenekleri sağlar [Azure portalında](https://portal.azure.com), Gelişmiş raporlama ve şirket içi bir dizi için destek ve bulut uygulamalarına. Azure multi-Factor Authentication dahil [Azure Active Directory Premium plan](https://www.microsoft.com/cloud-platform/azure-active-directory-features) ve [Enterprise Mobility + Security planları](https://www.microsoft.com/cloud-platform/enterprise-mobility-security-pricing)ve bulutta veya şirket içinde dağıtılabilir. |

## <a name="feature-comparison-of-versions"></a>Sürümleri özellik karşılaştırması
Aşağıdaki tabloda, çeşitli Azure multi-Factor Authentication sürümlerinde kullanılabilir olan özelliklerin bir listesini sağlar.

> [!NOTE]
> Bu karşılaştırma tablosu, her bir multi-Factor Authentication sürümü parçası olan özellikler açıklanmaktadır. Tam Azure multi-Factor Authentication hizmeti varsa, bazı özellikler kullanmak isteyip istemediğinize bağlı olarak mevcut olmayabilir [bulutta MFA veya MFA şirket içi](concept-mfa-whichversion.md).


| Özellik | Office 365 için Multi-Factor Authentication | Azure AD yöneticileri için multi-Factor Authentication | Azure Multi-Factor Authentication |
| --- |:---:|:---:|:---:|
| Azure AD yönetici hesapları MFA ile koruma |● |● (yalnızca Azure AD genel yönetici hesapları için) |● |
| İkinci öğe olarak mobil uygulama |● |● |● |
| İkinci öğe olarak telefon araması |● |● |● |
| İkinci öğe olarak SMS |● |● |● |
| Mfa'yı desteklemeyen istemciler için uygulama parolaları |● |● |● |
| Doğrulama yöntemleri üzerinde yönetici denetimi |● |● |● |
| Yönetici olmayan hesaplar MFA ile koruma |● (yalnızca Office 365 uygulamaları için) | |● |
| PIN modu | | |● |
| Sahtekarlık uyarısı | | |● |
| MFA Raporları | | |● |
| Bir Kerelik Atlama | | |● |
| Telefon aramaları için özel karşılama | | |● |
| Telefon aramaları için özel arayan kimliği | | |● |
| Güvenilen IP'ler | | |● |
| Güvenilen cihazlar için MFA hatırlama |● |● |● |
| MFA SDK | | |● (kullanım dışı) | 
| Şirket içi uygulamalar için MFA | | |● |

## <a name="how-to-turn-on-azure-multi-factor-authentication-for-azure-ad-administrators"></a>Azure AD yöneticileri için Azure multi-Factor Authentication devre dışı bırakma
Azure AD kiracıları içinde genel Yönetici rolüne atanan kullanıcılar hiçbir ek ücret ödemeden Azure AD genel yönetici hesapları için iki aşamalı doğrulamayı etkinleştirebilirsiniz. Bir Microsoft Account kullanıyorsanız, multi-Factor authentication için kaydedebilirsiniz [burada](https://support.microsoft.com/en-us/help/12408/microsoft-account-about-two-step-verification). Genel Yöneticiler için multi-Factor authentication Microsoft Account kullanmıyorsanız açma [burada](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-user-states).

## <a name="how-to-get-azure-multi-factor-authentication"></a>Azure multi-Factor Authentication'ı edinme
Azure multi-Factor Authentication tarafından sunulan tam işlevselliği istiyorsanız, birkaç seçenek vardır:

### <a name="option-1---mfa-licenses"></a>Seçenek 1 - MFA lisansları

Azure multi-Factor Authentication lisansları satın almanız ve bunları kullanıcılarınızın Azure Active Directory'de atayabilirsiniz. 

Yalnızca bu seçeneği kullanırsanız, lisansınız yoksa kullanıcılarınız için iki aşamalı doğrulamayı sağlamak istiyorsanız Azure multi-Factor Authentication sağlayıcısı oluşturun. Aksi takdirde, iki kez fatura.

### <a name="option-2---bundled-licenses-that-include-mfa"></a>Mfa'yı içeren paket seçeneği 2 - lisansları

Azure multi-Factor Authentication içeren lisansları satın alma ister Azure Active Directory Premium veya Enterprise Mobility + Security ve bunları Azure Active Directory kullanıcılarınıza atayabilirsiniz. 

Bu seçeneği kullanırsanız, yalnızca, ayrıca lisansınız yoksa kullanıcılarınız için iki aşamalı doğrulamayı sağlamanız gerekiyorsa, Azure multi-Factor Authentication sağlayıcısı oluşturmanız gerekir. Aksi takdirde, iki kez fatura. 

### <a name="option-3---mfa-consumption-based-model"></a>Seçenek 3 - kullanım tabanlı model MFA

Bir Azure aboneliğinde Azure multi-Factor Authentication sağlayıcısı oluşturun. Azure MFA sağlayıcıları, Kurumsal Anlaşma, Azure mali taahhüt veya kredi kartı gibi diğer tüm Azure kaynaklarına karşı faturalandırılır Azure kaynaklarıdır. Bu sağlayıcıları yalnızca tam Azure Abonelikleri, bir $0 olan harcama sınırlarını sınırlı Azure aboneliklerini oluşturulabilir. Lisans gibi seçenekleri 1 ve 2 etkinleştirdiğinizde, sınırlı bir abonelik oluşturulur. 

Azure multi-Factor Authentication sağlayıcısı kullanırken, Azure aboneliğiniz faturalandırılırsınız kullanılabilir iki kullanım modeli vardır:  

1. **Etkin kullanıcı başına** - sabit sayıda düzenli olarak kimlik doğrulaması gereken çalışanlar için iki aşamalı doğrulamayı etkinleştirmek istediğiniz kuruluşlara yöneliktir. Kullanıcı başına faturalandırma, Azure AD kiracınıza ve Azure MFA sunucunuzu MFA için etkinleştirilen kullanıcı sayısına bağlıdır. Kullanıcılar için mfa'yı hem de Azure AD'de etkin olup olmadığını ve Azure MFA sunucusu ve etki alanı eşitleme (Azure AD Connect) etkin sonra size daha büyük bir kullanıcı kümesine sayısı. Etki alanı eşitleme etkin değil sonra size Azure AD MFA için etkinleştirilmiş tüm kullanıcıların toplam sayısı ve Azure MFA sunucusu. Faturalandırma günlere eşit olarak dağıtılır ve ticaret sisteme günlük bildirdi. 

  > [!NOTE]
  > Faturalandırma örnek 1: 5000 kullanıcılar için mfa'yı bugün etkin olması. MFA sistem bu sayıyı o gün için 31 ve raporları 161.29 kullanıcı tarafından böler. Yarın MFA sistem 161.77 kullanıcı o gün için raporlar için 15 daha fazla kullanıcı sağlar. Fatura dönemi sonunda Azure aboneliğinize faturalandırılır kullanıcıların toplam sayısı 5.000 kadar ekler. 
  >
  > Faturalandırma örnek 2: bir kullanıcı başına Azure MFA Sağlayıcısı'nın farkını yapmak zorunda lisansların ve kullanıcıların olmadan, bir karışımı vardır. 4.500 vardır Enterprise Mobility + Security Lisanslarımı kiracınıza ancak MFA için etkinleştirilmiş 5.000 kullanıcılar. Azure aboneliğiniz 500 kullanıcı için fatura, günlere eşit olarak dağıtılır ve günlük 16.13 kullanıcılar olarak bildirilen. 

2. **Kimlik doğrulaması başına** - çok sayıda seyrek kimlik doğrulaması gereken kullanıcılar için iki aşamalı doğrulamayı etkinleştirmek istediğiniz kuruluşlara yöneliktir. Bu doğrulamaları başarısız veya reddedilen bağımsız olarak iki aşamalı doğrulama isteklerinin sayısı göre faturalandırılır. Bu faturalandırma Azure kullanım ekstreniz 10 kimlik doğrulama paketlerine görünür ve günlük olarak bildirilir. 

  > [!NOTE]
  > Faturalandırma örnek 3: Bugün Azure MFA hizmeti 3,105 iki aşamalı doğrulama isteklerini aldı. Azure aboneliğiniz için 310.5 kimlik doğrulama paketlerini faturalandırılır. 

Azure mfa'yı lisansları olabilir, ancak tüketim tabanlı yapılandırma için yine de faturalandırılır mıyım dikkat edin önemlidir. Bir kimlik doğrulaması başına Azure MFA sağlayıcısını ayarlama, olanlar lisansına sahip kullanıcılar tarafından gerçekleştirilen her iki aşamalı doğrulama isteği için faturalandırılırsınız. Azure AD kiracınıza bağlı olmayan bir etki alanındaki bir kullanıcı başına Azure MFA sağlayıcısını ayarlama, kullanıcılarınızın Azure AD lisansına sahip olsa bile etkinleştirilen kullanıcı başına faturalandırılır. 

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla fiyatlandırma ayrıntıları için bkz. [Azure MFA fiyatlandırması](https://azure.microsoft.com/pricing/details/multi-factor-authentication/).

- Azure mfa'yı dağıtmak isteyip istemediğinizi seçin [bulutta veya şirket içi](concept-mfa-whichversion.md)
