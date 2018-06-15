---
title: Azure Active Directory'yi (Önizleme) kullanarak Azure Storage erişimi kimlik doğrulaması | Microsoft Docs
description: Azure Active Directory'yi (Önizleme) kullanarak Azure Storage erişimi kimlik doğrulaması.
services: storage
author: tamram
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 06/01/2018
ms.author: tamram
ms.openlocfilehash: 9a0782b96b45d27c9b7e603959ecadf5b2632064
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34737655"
---
# <a name="authenticate-access-to-azure-storage-using-azure-active-directory-preview"></a>Azure Active Directory'yi (Önizleme) kullanarak Azure Storage erişimi kimlik doğrulaması

Azure Storage blobu ve kuyruk Hizmetleri kimlik doğrulaması ve yetkilendirme ile Azure Active Directory (AD) destekler. Azure AD ile erişim vermek için kullanıcıların, grupların veya uygulama hizmet asıl adı için rol tabanlı erişim denetimi (RBAC) kullanabilirsiniz. 

Azure Storage Azure AD kullanarak erişen uygulamalar yetkilendirme üst düzey güvenlik ve kullanım kolaylığı diğer yetkilendirme seçenekleri sağlar. Paylaşılan anahtar yetkilendirme uygulamalarınızla kullanmaya devam edebilirsiniz, ancak Azure AD kullanarak hesap erişim anahtarınızı kodunuzu ile depolamak için gereken bozar. Benzer şekilde, hassas depolama hesabınızdaki kaynaklara erişim vermek için paylaşılan erişim imzaları (SAS) kullanmaya devam edebilirsiniz, ancak Azure AD SAS belirteçlerini yönetin veya güvenliği aşılmış bir SAS iptal etme hakkında endişelenmeye gerek kalmadan benzer özellikleri sunar.

## <a name="about-the-preview"></a>Önizleme hakkında

' Nin önizlemesi hakkında aşağıdaki noktaları göz önünde bulundurun:

- Azure AD tümleştirme Blob ve kuyruk hizmetlerine sadece Önizleme için kullanılabilir.
- Azure AD tümleştirme, tüm ortak bölgelerde GPv1, GPv2 ve Blob Depolama hesapları için kullanılabilir. 
- Yalnızca Resource Manager dağıtım modeli kullanılarak oluşturulmuş depolama hesapları desteklenir. 
- Arayan kimlik bilgileri Azure depolama çözümlemeleri günlüğe desteği yakında geliyor.
- Azure AD yetkilendirme standart depolama hesapları kaynaklara erişim şu anda desteklenmiyor. Premium depolama hesapları sayfa bloblarını erişim yetkilendirme yakında desteklenecek.
- Azure depolama yerleşik ve özel RBAC rollerini destekler. Abonelik, kaynak grubu, depolama hesabı ya da bir bireysel kapsayıcı veya kuyruğa kapsamlı roller atayabilirsiniz.
- Şu anda Azure AD tümleştirmeyi Azure Storage istemcisi kitaplıklarını içerir:
    - [.NET](https://www.nuget.org/packages/WindowsAzure.Storage/9.2.0)
    - [Java](http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage) (7.1.x-Preview kullanın)
    - Python
        - [BLOB](https://github.com/Azure/azure-storage-python/releases/tag/v1.2.0rc1-blob)
        - [Sırası](https://github.com/Azure/azure-storage-python/releases/tag/v1.2.0rc1-queue)
    - [Node.js](https://www.npmjs.com/package/azure-storage)
    - [JavaScript](https://aka.ms/downloadazurestoragejs))

> [!IMPORTANT]
> Bu önizleme yalnızca üretim dışı kullanılması amaçlanmıştır. Azure AD tümleştirmesi için Azure Storage genel kullanıma bildirilmiş kadar üretim hizmet düzeyi sözleşmelerine (SLA) kullanılamaz. Azure AD tümleştirme senaryonuz için henüz desteklenmiyorsa paylaşılan anahtar yetkilendirme veya SAS belirteci uygulamalarınızı kullanmaya devam.
>
> Önizleme sırasında RBAC rolü atamalarını yayılması beş dakika kadar sürebilir.
>
> Azure Storage ile Azure AD tümleştirme Azure depolama işlemleri için HTTPS kullanmanızı gerektirir.

## <a name="get-started-with-azure-ad-for-storage"></a>Depolama için Azure AD ile çalışmaya başlama

Azure Storage ile Azure AD tümleştirme kullanarak ilk adımı RBAC rolleri depolama veriler için hizmet sorumlusu (kullanıcı, Grup veya uygulama hizmet sorumlusu) veya yönetilen hizmet kimliği (MSI) atamaktır. RBAC rolleri kapsayıcıları ve Kuyruklar izinlerini ortak kümesi kapsar. Azure Storage için RBAC rolleri hakkında daha fazla bilgi edinmek için [RBAC (Önizleme) depolama verilerle Yönet erişim hakları](storage-auth-aad-rbac.md).

Depolama kaynaklarını uygulamalarınızda erişim yetkisi vermek için Azure AD kullanmak için bir OAuth 2.0 erişim belirteci kodunuzdan istemeniz gerekir. Bir erişim belirteci istemek ve Azure depolama isteklerine yetkilendirmek için kullanmak üzere öğrenmek için bkz: [Azure Storage uygulamadan (Önizleme) Azure AD ile kimlik doğrulama](storage-auth-aad-app.md). Azure yönetilen hizmet kimliği (MSI) kullanıyorsanız, bkz: [bir Azure VM yönetilen hizmet kimliği (Önizleme) Azure AD ile kimlik doğrulama](storage-auth-aad-msi.md).

Azure CLI ve PowerShell şimdi bir Azure AD kimlik oturum açma destekler. Bir Azure AD kimlik bilgilerinizle oturum sonra oturumunuz bu kimliği altında çalışır. Daha fazla bilgi için bkz: [CLI veya PowerShell (Önizleme) ile Azure depolama alanına erişmek için bir Azure AD kimlik kullanmak](storage-auth-aad-script.md).

## <a name="next-steps"></a>Sonraki adımlar

Azure depolama ekibi blog gönderisi, Azure BLOB'ları ve kuyrukları için Azure AD tümleştirme hakkında ek bilgi için bkz: [Azure depolama için Azure Önizleme AD Authentication Duyurusu](https://azure.microsoft.com/blog/announcing-the-preview-of-aad-authentication-for-storage/).
