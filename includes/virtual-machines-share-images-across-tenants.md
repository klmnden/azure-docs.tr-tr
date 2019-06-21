---
title: include dosyası
description: include dosyası
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 04/25/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 499aeccdf00980eeb66ac6ee06e45267fd515143
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188335"
---
Paylaşılan resim galerileri, RBAC kullanarak paylaşım görüntüleri sağlar. Görüntüleri kiracınızdaki ve hatta kişiler, kiracınızın dışında paylaşmak için RBAC kullanabilirsiniz. Ancak, uygun ölçekte, Azure kiracınızın dışındaki görüntülerin paylaşmak istiyorsanız, paylaşımını kolaylaştırmak için bir uygulama kaydı oluşturmanız gerekir.  Bir uygulama kaydı kullanarak gibi daha karmaşık paylaşım senaryolarını etkinleştirebilirsiniz: 

* Görüntüleri bir şirket başka alır ve Azure altyapı ayrı kiracılar genelinde dağıldığında paylaşılan yönetme. 
* Azure iş ortakları, müşterilerine adına Azure altyapısını yönetme. Görüntü özelleştirme iş ortakları Kiracı içinde gerçekleştirilir, ancak altyapısı dağıtımlarını müşteri kiracısında gerçekleşir. 


## <a name="create-the-app-registration"></a>Uygulama kaydı oluşturma

Görüntü Galerisi kaynakları paylaşmak için hem de kiracılar tarafından kullanılacak bir uygulama kaydı oluşturun.
1. Açık [uygulama kayıtları (Önizleme) Azure portalında](https://ms.portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade/quickStartType//sourceType/).    
1. Seçin **yeni kayıt** sayfanın üst kısmındaki menüden.
1. İçinde **adı**, türü *myGalleryApp*.
1. İçinde **desteklenen hesap türleri**seçin **herhangi bir kuruluş dizinini ve kişisel Microsoft hesapları hesaplarında**.
1. İçinde **yeniden yönlendirme URI'si**, türü *https://www.microsoft.com* seçip **kaydetme**. Uygulama kaydı oluşturulduktan sonra genel bakış sayfası açılır.
1. Genel bakış sayfasında kopyalamak **uygulama (istemci) kimliği** ve daha sonra kullanmak üzere kaydetme.   
1. Seçin **sertifikaları ve parolaları**ve ardından **yeni gizli**.
1. İçinde **açıklama**, türü *paylaşılan görüntü Galerisi kiracılar arası uygulama gizli anahtarı*.
1. İçinde **Expires**, varsayılan değerini bırakın **1 yıl** seçip **Ekle**.
1. Gizli anahtar değerini kopyalayın ve güvenli bir yere kaydedin. Sayfadan çıktıktan sonra alamazsınız.


Paylaşılan görüntü Galerisi kullanmak için uygulama kaydı izin verin.
1. Azure portalında, başka bir kiracıyla paylaşmak istediğiniz paylaşılan görüntü Galerisi seçin.
1. Seçin **erişim denetimi (IAM) seçin**, altında **rol ataması Ekle** seçin *Ekle*. 
1. Altında **rol**seçin **okuyucu**.
1. Altında **erişim ata:** , bu olarak bırakın **Azure AD kullanıcı, Grup veya hizmet sorumlusu**.
1. Altında **seçin**, türü *myGalleryApp* ve listede görünür olduğunda bu seçeneği belirleyin. İşiniz bittiğinde **Kaydet**.


## <a name="give-tenant-2-access"></a>Kiracı 2 erişimi verin

Kiracı 2 erişmesini uygulamaya, bir tarayıcı kullanarak bir oturum açma isteyerek sağlayın. Değiştirin *<Tenant2 ID>* , görüntü Galerisi ile paylaşmak istiyorsunuz kiracısı için Kiracı kimliği. Değiştirin *< Uygulama (istemci) kimliği >* oluşturduğunuz uygulama kaydı uygulama kimliği. Değişiklik yapmadan işiniz bittiğinde, URL'yi tarayıcıya yapıştırın ve Kiracı 2 oturum açmak için oturum açma yönergeleri izleyin.

```
https://login.microsoftonline.com/<Tenant 2 ID>/oauth2/authorize?client_id=<Application (client) ID>&response_type=code&redirect_uri=https%3A%2F%2Fwww.microsoft.com%2F 
```

İçinde [Azure portalında](https://portal.azure.com) Kiracı 2 olarak oturum açın ve VM'yi oluşturmak istediğiniz kaynak grubunu uygulama kayıt erişim verin.

1. Kaynak grubunu seçin ve ardından **erişim denetimi (IAM)** . Altında **rol ataması Ekle** seçin **Ekle**. 
1. Altında **rol**, türü **katkıda bulunan**.
1. Altında **erişim ata:** , bu olarak bırakın **Azure AD kullanıcı, Grup veya hizmet sorumlusu**.
1. Altında **seçin** türü *myGalleryApp* listede görünür olduğunda seçin. İşiniz bittiğinde **Kaydet**.

> [!NOTE]
> Görüntü sürümü yerleşik ve başka bir görüntü sürümünü oluşturmak için aynı yönetilen görüntüsünü kullanabilmeniz için önce çoğaltılmış tamamen tamamlanmasını beklemeniz gerekir.

