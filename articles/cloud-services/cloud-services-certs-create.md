---
title: "Bulut Hizmetleri ve yönetim sertifikaları | Microsoft Docs"
description: "Oluşturma ve Microsoft Azure ile sertifikaları kullanma hakkında bilgi edinin"
services: cloud-services
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: fc70d00d-899b-4771-855f-44574dc4bfc6
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: adegeo
ms.openlocfilehash: 6a1e4f5316cc0321c1409f9e48daeae6ee483bf6
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2018
---
# <a name="certificates-overview-for-azure-cloud-services"></a>Azure Cloud Services sertifikalarına genel bakış
Sertifikalar ile Azure bulut Hizmetleri için kullanılır ([hizmet sertifikaları](#what-are-service-certificates)) ve yönetim API'si ile kimlik doğrulaması için ([yönetim sertifikaları](#what-are-management-certificates)). Bu konuda iki sertifika türleri için genel bir bakış nasıl sahip için [oluşturma](#create) ve [dağıtmak](#deploy) Azure onları.

Azure'da kullanılan sertifikalar x.509 v3 sertifikaları ve başka bir güvenilen sertifika tarafından imzalanan ya da kendinden imzalı olabilirler. Otomatik olarak imzalanan sertifika kendi oluşturucusu tarafından imzalanan, bu nedenle varsayılan olarak güvenilmiyor. Çoğu tarayıcılar bu sorunu yoksayabilirsiniz. Yalnızca geliştirme ve sınama bulut Hizmetleri zaman otomatik olarak imzalanan sertifikalar kullanmanız gerekir. 

Azure tarafından kullanılan sertifikalar, özel veya ortak anahtar içerebilir. Sertifikaların bir benzersiz şekilde tanımlamak için bir yol sağlayan bir parmak izi vardır. Bu parmak izi Azure kullanılan [yapılandırma dosyası](cloud-services-configure-ssl-certificate-portal.md) tanımlamak için bir bulut hizmeti sertifika kullanmanız gerekir. 

## <a name="what-are-service-certificates"></a>Hizmet sertifikaları nelerdir?
Bulut Hizmetleri ve güvenli iletişim için ve hizmetinden etkinleştirmek için hizmet sertifikaları bağlanmış. Örneğin, bir web rolü dağıttıysanız, kullanıma sunulan bir HTTPS uç noktası doğrulanabilir bir sertifika sağlamak istersiniz. Hizmet tanımında tanımlanan hizmet sertifikaları, rol örneği çalıştıran sanal makine otomatik olarak dağıtılır. 

Hizmet sertifikaları ya da Azure portal'ı kullanarak Azure veya Klasik dağıtım modeli kullanarak yükleyebilirsiniz. Hizmet sertifikaları özel bulut hizmeti ile ilişkilendirilmiş. Hizmet tanımı dosyası bir dağıtımda atandığı.

Hizmet sertifikaları, Hizmetleri'nden ayrı olarak yönetilebilir ve farklı kişiler tarafından yönetiliyor olabilir. Örneğin, bir geliştirici bir BT yöneticisi için Azure önceden yükledi bir sertifika başvurduğu bir hizmet paketi yükleme. Bir BT yöneticisi, yönetin ve yeni bir hizmet paketi yüklemeye gerek kalmadan (hizmetinin yapılandırmasını değiştirme), bu sertifikayı yenilemek. Yeni bir hizmet paketi güncelleştirme mantıksal adını, depolama ad ve sertifikanın konumunu hizmet tanımı dosyasında olduğundan ve sertifika parmak izini hizmet yapılandırma dosyasında belirtilen sırada mümkündür. Bir sertifikayı güncelleştirmek için yalnızca yeni bir sertifika karşıya yüklemek ve hizmet yapılandırma dosyası parmak izi değerini değiştirmek gereklidir.

>[!Note]
>[Bulut Hizmetleri ile ilgili SSS](cloud-services-faq.md) makale bazı sertifikalar hakkında yararlı bilgiler sahiptir.

## <a name="what-are-management-certificates"></a>Yönetim sertifikaları nelerdir?
Yönetim sertifikaları, Klasik dağıtım modeliyle kimlik doğrulaması sağlar. Birçok programlar ve araçlar (örneğin, Visual Studio ya da Azure SDK'sı) yapılandırma ve çeşitli Azure hizmetlerine dağıtımını otomatik hale getirmek için bu sertifikaları kullanacak. Bu gerçekten bulut hizmetlerine ilgili değildir. 

> [!WARNING]
> Dikkatli olun! Bu tür bir sertifika ile ilişkili aboneliği yönetmek için kimlik doğrulamasını herkes izin verin. 
> 
> 

### <a name="limitations"></a>Sınırlamalar
Abonelik başına 100 Yönetim sertifikaları sınırı yoktur. Ayrıca belirli hizmet yöneticisinin kullanıcı kimliğini altındaki tüm abonelikler için 100 Yönetim sertifikaları sınırı vardır Hesap Yöneticisi kullanıcı kimliği 100 Yönetim sertifikaları eklemek için zaten kullanılmış ve gerekirse daha fazla sertifikaları için ek sertifika eklemek için bir ortak yönetici ekleyebilirsiniz. 

<a name="create"></a>
## <a name="create-a-new-self-signed-certificate"></a>Yeni bir otomatik olarak imzalanan sertifika oluşturma
Bu ayarların uyması sürece otomatik olarak imzalanan sertifika oluşturmak için kullanılabilir herhangi bir aracı kullanabilirsiniz:

* Bir X.509 sertifikası.
* Bir özel anahtar içerir.
* Anahtar Değişimi (.pfx dosyası) oluşturulur.
* Konu adı, bulut hizmetine erişmek için kullanılan etki alanını eşleşmelidir.

    > Cloudapp.net için bir SSL sertifikası alınamıyor (veya herhangi bir için Azure ile ilgili) etki alanı; Sertifikanın konu adı, uygulamaya erişmek için kullanılan özel etki alanı adı eşleşmelidir. Örneğin, **contoso.net**değil **contoso.cloudapp.net**.

* En az 2048 bit şifreleme.
* **Hizmet sertifika yalnızca**: istemci-tarafı sertifika bulunmalıdır *kişisel* sertifika deposu.

Windows, bir sertifika oluşturmak için iki kolay yolla `makecert.exe` yardımcı programı veya IIS.

### <a name="makecertexe"></a>Makecert.exe
Bu yardımcı program kullanım dışı bırakıldı ve artık aşağıda anlatılmıştır. Daha fazla bilgi için bkz: [bu MSDN makalesine](https://msdn.microsoft.com/library/windows/desktop/aa386968).

### <a name="powershell"></a>PowerShell
```powershell
$cert = New-SelfSignedCertificate -DnsName yourdomain.cloudapp.net -CertStoreLocation "cert:\LocalMachine\My" -KeyLength 2048 -KeySpec "KeyExchange"
$password = ConvertTo-SecureString -String "your-password" -Force -AsPlainText
Export-PfxCertificate -Cert $cert -FilePath ".\my-cert-file.pfx" -Password $password
```

> [!NOTE]
> Bir etki alanı yerine IP adresiyle sertifika kullanmak istiyorsanız, IP adresi - DnsName parametreyi kullanın.


Bu kullanmak istiyorsanız, [Sertifika Yönetim Portalı ile](../azure-api-management-certs.md), kendisine verme bir **.cer** dosyası:

```powershell
Export-Certificate -Type CERT -Cert $cert -FilePath .\my-cert-file.cer
```

### <a name="internet-information-services-iis"></a>Internet Information Services (IIS)
İnternet'ın IIS ile bunun nasıl ele birçok sayfalarında vardır. [Burada](https://www.sslshopper.com/article-how-to-create-a-self-signed-certificate-in-iis-7.html) harika bir bulunan yeni çalışmaktan de açıklanmaktadır. 

### <a name="linux"></a>Linux
[Bu](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) makalede sertifikaların SSH ile nasıl oluşturulacağı açıklanmaktadır.

## <a name="next-steps"></a>Sonraki adımlar
[Azure portalına hizmet sertifikanızı karşıya](cloud-services-configure-ssl-certificate-portal.md).

Karşıya bir [yönetim API sertifikası](../azure-api-management-certs.md) Azure portalına.

