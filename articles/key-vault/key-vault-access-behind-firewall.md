---
title: "Güvenlik duvarının arkasındaki Anahtar Kasasına erişme | Microsoft Belgeleri"
description: "Güvenlik duvarının arkasındaki bir uygulamadan Azure Anahtar Kasasına nasıl erişebileceğinizi öğrenin"
services: key-vault
documentationcenter: 
author: amitbapat
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 50d21774-2ee1-4212-8995-570c9de603c5
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 01/07/2017
ms.author: ambapat
translationtype: Human Translation
ms.sourcegitcommit: da4d156132fba9efc98b3af441b6d095a4bb60ea
ms.openlocfilehash: e0bc6e75fef1f3567940e30acf6f9f429258be12


---
# <a name="access-azure-key-vault-behind-a-firewall"></a>Güvenlik duvarının ardındayken Azure Anahtar Kasası’na erişme
### <a name="q-my-key-vault-client-application-needs-to-be-behind-a-firewall-what-ports-hosts-or-ip-addresses-should-i-open-to-enable-access-to-a-key-vault"></a>S: Anahtar kasası istemci uygulamamın güvenlik duvarının ardında olması gerekiyor. Anahtar kasasına erişebilmek için hangi bağlantı noktaları, konaklar veya IP adreslerini açmam gerekiyor?
Bir anahtar kasasına erişmek için, anahtar kasası istemci uygulamanızın çeşitli işlevlere ilişkin birden çok uç noktaya erişebilmesi gerekir:

* Azure Active Directory aracılığıyla kimlik doğrulama (Azure AD).
* Azure Anahtar Kasası'nın yönetimi. Buna, Azure Resource Manager aracılığıyla erişim ilkeleri oluşturma, okuma, güncelleştirme, silme ve ayarlama da dahildir.
* Anahtar Kasası’nda depolanan nesnelere (anahtarlar ve gizli anahtarlar) erişim ve bu nesnelerin yönetimi, Anahtar Kasası’na özgü uç nokta üzerinden gerçekleşir (örneğin, [https://yourvaultname.vault.azure.net](https://yourvaultname.vault.azure.net)).  

Yapılandırmanıza ve ortamınıza bağlı olarak, bazı farklılıklar mevcuttur.   

## <a name="ports"></a>Bağlantı Noktaları
Üç işlev (kimlik doğrulama, yönetim ve veri düzlemi erişimi) için de anahtar kasası trafiği HTTPS: bağlantı noktası 443 üzerinden gider. Ancak CRL için zaman zaman HTTP (bağlantı noktası 80) trafiği de olacaktır. OCSP destekleyen istemciler CRL'ye erişmemelidir, ancak zaman zaman [http://cdp1.public-trust.com/CRL/Omniroot2025.crl](http://cdp1.public-trust.com/CRL/Omniroot2025.crl)'ye erişebilirler.  

## <a name="authentication"></a>Kimlik Doğrulaması
Anahtar kasası istemci uygulamasının, kimlik doğrulaması için Azure Active Directory uç noktalarına erişmesi gerekir. Kullanılan uç nokta, Azure AD kiracı yapılandırmasına, sorumlu türüne (kullanıcı sorumlusu veya hizmet sorumlusu) ve hesap türüne (örneğin, Microsoft hesabı ya da iş veya okul hesabı) bağlıdır.  

| Sorumlu türü | Uç nokta:bağlantı noktası |
| --- | --- |
| Microsoft hesabı kullanan kullanıcı<br> (örneğin, user@hotmail.com) |**Genel:**<br> login.microsoftonline.com:443<br><br> **Azure Çin:**<br> login.chinacloudapi.cn:443<br><br>**Azure ABD:**<br> login-us.microsoftonline.com:443<br><br>**Azure Almanya:**<br> login.microsoftonline.de:443<br><br> ve <br>login.live.com:443 |
| Azure AD ile iş veya okul hesabı kullanan kullanıcı veya hizmet sorumlusu (örneğin, user@contoso.com) |**Genel:**<br> login.microsoftonline.com:443<br><br> **Azure Çin:**<br> login.chinacloudapi.cn:443<br><br>**Azure ABD:**<br> login-us.microsoftonline.com:443<br><br>**Azure Almanya:**<br> login.microsoftonline.de:443 |
| İş veya okul hesabı ve Active Directory Federasyon Hizmetleri (AD FS) veya başka bir federasyon uç noktası kullanan kullanıcı veya hizmet sorumlusu, user@contoso.com) |İş veya okul hesabı için tüm uç noktalar ve AD FS veya diğer federasyon uç noktaları |

Farklı olası karmaşık senaryolar da mevcuttur. Daha fazla bilgi için bkz. [Azure Active Directory Kimlik Doğrulaması Akışı](/documentation/articles/active-directory-authentication-scenarios/), [Uygulamaları Azure Active Directory ile Tümleştirme](/documentation/articles/active-directory-integrating-applications/) ve [Active Directory Kimlik Doğrulaması Protokolleri](https://msdn.microsoft.com/library/azure/dn151124.aspx).  

## <a name="key-vault-management"></a>Anahtar Kasası yönetimi
Anahtar Kasası yönetimi için (CRUD ve erişim ilkesi ayarı), anahtar kasası istemci uygulamasının Azure Resource Manager uç noktasına erişmesi gerekir.  

| İşlem türü | Uç nokta:bağlantı noktası |
| --- | --- |
| Anahtar Kasası denetim düzlemi işlemleri<br> Azure Resource Manager yoluyla |**Genel:**<br> management.azure.com:443<br><br> **Azure Çin:**<br> management.chinacloudapi.cn:443<br><br> **Azure ABD:**<br> management.usgovcloudapi.net:443<br><br> **Azure Almanya:**<br> management.microsoftazure.de:443 |
| Azure Active Directory Grafik API'si |**Genel:**<br> graph.windows.net:443<br><br> **Azure Çin:**<br> graph.chinacloudapi.cn:443<br><br> **Azure ABD:**<br> graph.windows.net:443<br><br> **Azure Almanya:**<br> graph.cloudapi.de:443 |

## <a name="key-vault-operations"></a>Anahtar Kasası işlemleri
Tüm anahtar kasası nesne (anahtarlar ve gizli anahtarlar) yönetimi ve şifreleme işlemleri için, anahtar kasası istemcisinin anahtar kasası uç noktasına erişmesi gerekir. Uç nokta DNS soneki, anahtar kasanızın konumuna bağlı olarak farklılık gösterir. Anahtar kasası uç noktası, aşağıdaki tabloda gösterildiği gibi *vault-name*.*region-specific-dns-suffix* biçimindedir.  

| İşlem türü | Uç nokta:bağlantı noktası |
| --- | --- |
| Anahtarlar üzerindeki şifreleme işlemlerini de içeren işlemler, anahtarları ve gizli anahtarları oluşturma, okuma, güncelleştirme ve silme, anahtar kasası nesnelerindeki etiketleri ve diğer öznitelikleri (anahtarlar ya da gizli anahtarlar) ayarlama veya alma |**Genel:**<br> &lt;vault-name&gt;.vault.azure.net:443<br><br> **Azure Çin:**<br> &lt;vault-name&gt;.vault.azure.cn:443<br><br> **Azure ABD:**<br> &lt;vault-name&gt;.vault.usgovcloudapi.net:443<br><br> **Azure Almanya:**<br> &lt;vault-name&gt;.vault.microsoftazure.de:443 |

## <a name="ip-address-ranges"></a>IP adresi aralıkları
Anahtar Kasası hizmeti, PaaS altyapısı gibi diğer Azure kaynaklarını kullanır. Bu nedenle, Anahtar Kasası hizmet uç noktalarının belirli bir zamanda sahip olacağı IP adresleri için özel bir aralık belirtmek mümkün değildir. Güvenlik duvarınız yalnızca IP adresi aralıklarını destekliyorsa [Microsoft Azure Veri Merkezi IP Aralıkları](https://www.microsoft.com/download/details.aspx?id=41653) belgesini inceleyin. Kimlik doğrulaması ve kimlik için (Azure Active Directory), uygulamanızın [Kimlik doğrulaması ve kimlik adresleri](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) bölümünde açıklanan uç noktalara bağlanabilmesi gerekir.

## <a name="next-steps"></a>Sonraki adımlar
Anahtar Kasası ile ilgili sorularınız varsa bkz. [Azure Anahtar Kasası Forumları](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault).




<!--HONumber=Dec16_HO3-->


