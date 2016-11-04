---
title: Güvenlik duvarının arkasındaki Anahtar Kasasına Erişme | Microsoft Docs
description: Güvenlik duvarının arkasındaki bir uygulamadan Anahtar Kasasına nasıl erişebileceğinizi öğrenin
services: key-vault
documentationcenter: ''
author: amitbapat
manager: mbaldwin
tags: azure-resource-manager

ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 09/13/2016
ms.author: ambapat

---
# Güvenlik duvarının arkasındaki Anahtar Kasasına Erişme
### S: Anahtar kasası istemci uygulamamın bir güvenlik duvarının arkasında olması gerekiyor. Anahtar kasasına erişmek için hangi bağlantı noktalarını/ana bilgisayarları/IP adreslerini açmalıyım?
Bir anahtar kasasına erişmek için, anahtar kasası istemci uygulamanızın çeşitli işlevlere ilişkin birden çok uç noktaya erişebilmesi gerekir.

* Kimlik doğrulaması (Azure Active Directory aracılığıyla)
* Azure Resource Manager üzerinden Anahtar Kasası yönetimi (oluşturma/okuma/güncelleştirme/silme ve erişim ilkeleri ayarlarını içerir)
* Anahtar kasasında depolanan nesnelere (anahtarlar ve gizli diziler) erişim ve bu nesnelerin yönetimi, anahtar kasasına özgü uç nokta üzerinden gerçekleşir (örneğin, [https://yourvaultname.vault.azure.net](https://yourvaultname.vault.azure.net)).  

Yapılandırmanıza ve ortamınıza bağlı olarak, bazı farklılıklar mevcuttur.   

## Bağlantı Noktaları
3 işlevin tümü için (kimlik doğrulaması, yönetim ve veri düzlemine erişim) anahtar kasasına gelen tüm trafik HTTPS üzerinden geçer: Bağlantı noktası 443. Ancak CRL için zaman zaman HTTP (bağlantı noktası 80) trafiği de olacaktır. OCSP destekleyen istemciler CRL'ye erişmemelidir, ancak zaman zaman [http://cdp1.public-trust.com/CRL/Omniroot2025.crl](http://cdp1.public-trust.com/CRL/Omniroot2025.crl)'ye erişebilirler.  

## Kimlik Doğrulaması
Anahtar Kasası istemci uygulamasının, kimlik doğrulaması için Azure Active Directory uç noktalarına erişmesi gerekir. Kullanılan uç nokta, AAD kiracı yapılandırmasına ve sorumlu türüne (kullanıcı sorumlusu, hizmet sorumlusu ve hesap türü; örneğin, Microsoft Hesabı veya Kuruluş Kimliği) bağlıdır.  

| Sorumlu Türü | Uç nokta:bağlantı noktası |
| --- | --- |
| Microsoft Hesabı kullanan kullanıcı<br> (örneğin, user@hotmail.com) |**Genel:**<br> login.microsoftonline.com:443<br><br> **Azure Çin:**<br> login.chinacloudapi.cn:443<br><br>**Azure ABD:**<br> login-us.microsoftonline.com:443<br><br>**Azure Almanya:**<br> login.microsoftonline.de:443<br><br> ve <br>login.live.com:443 |
| AAD ile Kuruluş Kimliği kullanan Kullanıcı/Hizmet sorumlusu (örneğin, user@contoso.com) |**Genel:**<br> login.microsoftonline.com:443<br><br> **Azure Çin:**<br> login.chinacloudapi.cn:443<br><br>**Azure ABD:**<br> login-us.microsoftonline.com:443<br><br>**Azure Almanya:**<br> login.microsoftonline.de:443 |
| Kuruluş Kimliği ve ADFS veya farklı bir şirket dışı uç nokta kullanan Kullanıcı/Hizmet sorumlusu (örneğin, user@contoso.com) |Kuruluş Kimliği ve ADFS için yukarıdaki tüm uç noktalar veya diğer şirket dışı uç noktalar |

Farklı olası karmaşık senaryolar da mevcuttur. Daha fazla bilgi için bkz. [Azure Active Directory Kimlik Doğrulaması Akışı](/documentation/articles/active-directory-authentication-scenarios/), [Uygulamaları Azure Active Directory ile Tümleştirme](/documentation/articles/active-directory-integrating-applications/) ve [Active Directory Kimlik Doğrulaması Protokolleri](https://msdn.microsoft.com/library/azure/dn151124.aspx).  

## Anahtar Kasası Yönetimi
Anahtar Kasası Yönetimi için (CRUD ve erişimi ilkesi ayarı), anahtar kasası istemci uygulamasının Azure Resource Manager uç noktasına erişmesi gerekir.  

| İşlem türü | Uç nokta:bağlantı noktası |
| --- | --- |
| Anahtar Kasası Denetim Düzlemi işlemleri<br> Azure Resource Manager yoluyla |**Genel:**<br> management.azure.com:443<br><br> **Azure Çin:**<br> management.chinacloudapi.cn:443<br><br> **Azure ABD:**<br> management.usgovcloudapi.net:443<br><br> **Azure Almanya:**<br> management.microsoftazure.de:443 |
| Azure Active Directory Grafik API'si |**Genel:**<br> graph.windows.net:443<br><br> **Azure Çin:**<br> graph.chinacloudapi.cn:443<br><br> **Azure ABD:**<br> graph.windows.net:443<br><br> **Azure Almanya:**<br> graph.cloudapi.de:443 |

## Anahtar Kasası İşlemleri
Tüm anahtar kasası nesne (anahtarlar ve gizli diziler) yönetimi ve şifreleme işlemleri için, anahtar kasası istemcisinin anahtar kasası uç noktasına erişmesi gerekir. Anahtar Kasanızın konumuna bağlı olarak, uç nokta DNS soneki farklılık gösterir. Anahtar Kasası uç noktası şu biçimdedir: Aşağıdaki tabloda tanımlandığı gibi <kasa-adı>.<bölgeye-özgü-dns-soneki>.  

| İşlem türü | Uç nokta:bağlantı noktası |
| --- | --- |
| Anahtarlardaki şifreleme işlemleri gibi Anahtar Kasası işlemleri, anahtarları ve gizli dizileri oluşturma/okuma/güncelleştirme/silme, anahtar kasası nesnelerindeki (anahtarlar/gizli diziler) ayarlama/getirme etiketleri ve diğer öznitelikler |**Genel:**<br> &lt;vault-name&gt;.vault.azure.net:443<br><br> **Azure Çin:**<br> &lt;vault-name&gt;.vault.azure.cn:443<br><br> **Azure ABD:**<br> &lt;vault-name&gt;.vault.usgovcloudapi.net:443<br><br> **Azure Almanya:**<br> &lt;vault-name&gt;.vault.microsoftazure.de:443 |

## IP Adresi Aralıkları
Anahtar kasası hizmet uç noktalarının belirli bir zamanda sahip olduğu IP adresleri için belirli bir aralık sağlamak mümkün olmadığından Anahtar Kasası hizmeti, PaaS altyapısı gibi diğer Azure kaynaklarını sırasıyla kullanır. Ancak güvenlik duvarınız yalnızca IP adres aralıklarını destekliyorsa [Microsoft Azure Veri Merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653) belgesini inceleyiniz.   Kimlik doğrulaması ve kimlik için (Azure Active Directory), uygulamanızın [Kimlik Doğrulaması ve Kimlik Adresleri](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) bölümünde açıklanan uç noktalara bağlanabilmesi gerekir.

## Sonraki Adımlar
* Anahtar Kasası ile ilgili sorularınız varsa bkz. [Azure Anahtar Kasası Forumları](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault)

<!--HONumber=Sep16_HO3-->


