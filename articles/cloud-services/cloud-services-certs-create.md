---
title: Bulut Hizmetleri ve yönetim sertifikaları | Microsoft Docs
description: Oluşturma ve Microsoft Azure ile sertifikaları kullanma hakkında bilgi edinin
services: cloud-services
documentationcenter: .net
author: jpconnock
manager: timlt
editor: ''
ms.assetid: fc70d00d-899b-4771-855f-44574dc4bfc6
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: jeconnoc
ms.openlocfilehash: 4ca26c7b8fbfebbce8cfcb9915a7db12e5ad2352
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59793857"
---
# <a name="certificates-overview-for-azure-cloud-services"></a>Azure Cloud Services sertifikalarına genel bakış
Sertifikalar Azure'da bulut Hizmetleri için kullanılır ([hizmet sertifikaları](#what-are-service-certificates)) ve yönetim API'si ile kimlik doğrulaması ([yönetim sertifikaları](#what-are-management-certificates)). Bu konu nasıl hem sertifika türleri için genel bir bakış sağlar için [oluşturma](#create) ve Azure'a dağıtın.

Azure'da kullanılan sertifikalar x.509 v3 sertifikaları ve başka bir güvenilir sertifika tarafından imzalanmış ya da kendinden imzalı olabilirler. Otomatik olarak imzalanan bir sertifika kendi oluşturucusu tarafından imzalanmış, bu nedenle varsayılan olarak güvenilmiyor. Çoğu tarayıcısı bu sorunu yoksayabilirsiniz. Yalnızca geliştirme ve test cloud services, otomatik olarak imzalanan sertifikalar kullanmanız gerekir. 

Azure tarafından kullanılan sertifikalar, özel veya ortak anahtar içerebilir. Sertifikaları belirsiz bir şekilde tanımak için bir yol sağlayan bir parmak izi var. Bu parmak izine Azure'da kullanılan [yapılandırma dosyası](cloud-services-configure-ssl-certificate-portal.md) tanımlamak için sertifika bir bulut hizmeti kullanmanız gerekir. 

>[!Note]
>Azure bulut Hizmetleri AES256 SHA256 şifrelenmiş sertifikayı kabul etmiyor.

## <a name="what-are-service-certificates"></a>Hizmet sertifikaları nelerdir?
Hizmet sertifikaları, bulut Hizmetleri ve hizmetinden güvenli iletişimini etkinleştirmek için eklenir. Örneğin, bir web rolü dağıttıysanız, kullanıma sunulan bir HTTPS uç noktasının kimlik doğrulaması bir sertifika sağlamak istersiniz. Hizmet sertifikalarını, hizmet tanımında, rol örneği çalıştıran sanal makineye otomatik olarak dağıtılır. 

Hizmet sertifikası, ya da Azure portal'ı kullanarak azure'a veya Klasik dağıtım modelini kullanarak yükleyebilirsiniz. Hizmet sertifikaları, bir özel bulut hizmeti ile ilişkilidir. Hizmet tanım dosyası içinde bir dağıtım için atanır.

Hizmet sertifikaları hizmetlerinizden ayrı olarak yönetilebilir ve farklı kişiler tarafından yönetiliyor olabilir. Örneğin, bir geliştirici, bir BT yöneticisi Azure'a daha önce yüklenmiş bir sertifikaya başvurur bir hizmet paketi karşıya yükleyebilirsiniz. Bir BT yöneticisi, yönetin ve yeni bir hizmet paketini karşıya yüklemek gerek kalmadan (hizmetinin yapılandırmasını değiştirme) Bu sertifika yenileme. Yeni bir hizmet paketi güncelleştirme mantıksal adı, depo adı ve sertifika konumu hizmet tanımı dosyasında olduğundan ve sertifika parmak izini, hizmet yapılandırma dosyasında belirtilen sırada mümkündür. Bir sertifikayı güncelleştirmek için yalnızca yeni sertifikayı karşıya yüklemek ve hizmet yapılandırma dosyasında parmak izi değerini değiştirmek gereklidir.

>[!Note]
>[Cloud Services hakkında SSS'yi](cloud-services-faq.md) makale sertifikalar hakkında bazı yararlı bilgiler vardır.

## <a name="what-are-management-certificates"></a>Yönetim sertifikaları nelerdir?
Yönetim sertifikaları Klasik dağıtım modeliyle kimlik doğrulamasından geçmesini sağlar. Birçok programları ve Araçları (örneğin, Visual Studio ya da Azure SDK'sı) bu sertifikaları yapılandırma ve çeşitli Azure Hizmetleri dağıtımını otomatikleştirmek için kullanın. Bunlar gerçekten bulut Hizmetleri için ilgili değildir. 

> [!WARNING]
> Dikkat et! Bu tür bir sertifika ile ilişkili aboneliği yönetmek için onlarla kimliğini doğrulayan herkes izin verin. 
> 
> 

### <a name="limitations"></a>Sınırlamalar
Abonelik başına 100 Yönetim sertifikaları bir sınırlama yoktur. Ayrıca belirli bir hizmet yöneticisinin kullanıcı kimliğini altındaki tüm abonelikler için 100 Yönetim sertifikaları bir sınır yoktur Hesap Yöneticisi kullanıcı kimliği 100 Yönetim sertifikaları eklemek için zaten kullanılmış ve daha fazla sertifika gereksinimi varsa, ek bir sertifika eklemek için bir ortak yönetici ekleyebilirsiniz. 

<a name="create"></a>
## <a name="create-a-new-self-signed-certificate"></a>Yeni bir otomatik olarak imzalanan sertifika oluştur
Bunlar bu ayarlara bağlı olarak, kendinden imzalı bir sertifika oluşturmak için kullanılabilir olan herhangi bir aracı kullanabilirsiniz:

* X.509 sertifikası.
* Özel anahtarı içerir.
* Anahtar Değişimi (.pfx dosyası) oluşturulur.
* Konu adı, bulut hizmetine erişmek için kullanılan etki alanı eşleşmesi gerekir.

    > Cloudapp.net için SSL sertifikası alınamıyor. (veya herhangi bir için Azure ile ilgili) etki alanı; Sertifikanın konu adı, uygulamanıza erişmek için kullanılan özel etki alanı adı eşleşmelidir. Örneğin, **contoso.net**değil **contoso.cloudapp.net**.

* En az 2048 bit şifreleme.
* **Hizmet sertifikası yalnızca**: İstemci tarafı sertifika bulunmalıdır *kişisel* sertifika deposu.

Windows, sertifikayı oluşturmak için iki kolay yolu vardır `makecert.exe` yardımcı programı veya IIS.

### <a name="makecertexe"></a>Makecert.exe
Bu yardımcı programı, kullanım dışı bırakıldı ve artık burada belirtilmiştir. Daha fazla bilgi için [bu MSDN makalesinde](/windows/desktop/SecCrypto/makecert).

### <a name="powershell"></a>PowerShell
```powershell
$cert = New-SelfSignedCertificate -DnsName yourdomain.cloudapp.net -CertStoreLocation "cert:\LocalMachine\My" -KeyLength 2048 -KeySpec "KeyExchange"
$password = ConvertTo-SecureString -String "your-password" -Force -AsPlainText
Export-PfxCertificate -Cert $cert -FilePath ".\my-cert-file.pfx" -Password $password
```

> [!NOTE]
> Bir etki alanı yerine bir IP adresine sahip bir sertifika kullanmak istiyorsanız, IP adresi - DnsName parametreyi kullanın.


Bunu kullanmak isterseniz [Sertifika Yönetim Portalı'yla](../azure-api-management-certs.md), kendisine dışarı bir **.cer** dosyası:

```powershell
Export-Certificate -Type CERT -Cert $cert -FilePath .\my-cert-file.cer
```

### <a name="internet-information-services-iis"></a>Internet Information Services (IIS)
IIS ile bunun nasıl yapılacağını kapsayan çok sayıda sayfalarında internet vardır. [Burada](https://www.sslshopper.com/article-how-to-create-a-self-signed-certificate-in-iis-7.html) harika bir bulunan iyi açıklayan düşünüyorum. 

### <a name="linux"></a>Linux
[Bu](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) makale SSH ile sertifikaları oluşturma işlemini açıklar.

## <a name="next-steps"></a>Sonraki adımlar
[Azure portalında service sertifikanızı karşıya](cloud-services-configure-ssl-certificate-portal.md).

Karşıya bir [yönetim API sertifikasını](../azure-api-management-certs.md) Azure portalında.

