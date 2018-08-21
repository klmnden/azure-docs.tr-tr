---
title: Bir hizmet hesabı olarak Azure Stack doğrulama ayarlama | Microsoft Docs
description: Bir hizmet hesabı olarak doğrulama ayarlama konusunda bilgi edinin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 07/24/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: f6c78a3e79ac88194e7e34ad8be7a941ee715d39
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2018
ms.locfileid: "40235492"
---
# <a name="set-up-your-validation-as-a-service-account"></a>Bir hizmet hesabı olarak doğrulama ayarlayın

[!INCLUDE[Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Doğrulama (VaaS) hizmet olarak tasarlamak, geliştirmek, doğrulama, satış, dağıtma ve Azure Stack çözümleri piyasadaki desteklemek için Microsoft ile ortak mühendislik anlaşmanız Microsoft Azure Stack iş ortakları için sunulan bir Azure hizmetidir.

Sisteminiz için doğrulama hizmeti olarak hazır hale getirmek öğrenin. Azure Active Directory örneği ve VaaS kullanıma hazır almak için diğer gerekli görevler gerçekleştirebilirsiniz. 

Görevler aşağıdakileri içerir:

- Günlükleri depolamak için bir Azure depolama blobu oluştur
- Yerel aracı dağıtma
- Test edilecek Azure Stack örneğinde test görüntüsü sanal makineleri indirme

## <a name="create-an-azure-active-directory-tenant-id"></a>Bir Azure Active Directory Kiracı kimliği oluşturma

1. Bir Azure Active Directory kiracısında oluşturma [Azure portalında](https://portal.azure.com) veya mevcut bir kiracıyı kullanın.

    Gibi açıklayıcı bir ad ile kullanılmak üzere özel olarak bir kiracı ile VaaS oluşturmanız önemle tavsiye edilir ContosoVaaS@onmicrosoft.com. Kiracı rol tabanlı erişim denetimi (RBAC) iş ortağı tarafından kimlerin ortağı kuruluşundaki VaaS kullanabileceğini yönetmek için kullanılır.  
    
    Daha fazla bilgi için bkz: [Azure AD dizininizi yönetme](https://docs.microsoft.com/azure/active-directory/active-directory-administer).

    > [!Note]  
    > Yeni Azure Active Directory kiracıları oluşturma hakkında daha fazla bilgi için bkz. [Azure AD'yi kullanmaya başlama](https://docs.microsoft.com/azure/active-directory/get-started-azure-ad).

2. Kuruluşunuzun kiracıya hizmet kullanımı için sorumlu olacak üyeleri ekleyin. Her kullanıcının kiracısında VaaS kendi erişim düzeyini denetlemek için aşağıdaki rollerden birine atayın:

    | Rol Adı | Açıklama |
    |---------------------|------------------------------------------|
    | Sahip | Tüm kaynaklar için tam erişimi vardır. |
    | Okuyucu | Tüm kaynakları görüntüleyebilir ancak düzenleyemez. |
    | Test katkıda bulunan | Test kaynakları yönetebilir. |
    | Katalog katkıda bulunan | Çözüm yayımlama kaynakları yönetebilir. |

## <a name="set-up-your-tenant"></a>Kiracınızı ayarladığınızda

Kiracınızda ayarlama **Azure Stack doğrulama hizmeti** uygulama. 

1. Microsoft'ta Kiracı hakkında aşağıdaki bilgileri gönderin vaashelp@microsoft.com.

    | Veriler | Açıklama |
    |--------------------------------|---------------------------------------------------------------------------------------------|
    | Kuruluş Adı | Resmi kuruluş adı. |
    | Azure AD Kiracısı dizin adı | Azure AD Kiracı dizin adı kaydediliyor. |
    | Azure AD Kiracı dizin kimliği | Azure AD Kiracısı Directory dizini ile ilişkili GUID.<br> Azure AD Kiracı dizin Kimliğinizi bulma konusunda bilgi için bulunabilir, bkz: "[Kiracı kimliği alma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal#get-tenant-id)." |

    

2. Azure yığını ekibinden kiracınızı VaaS portalı kullanabilirsiniz onay sağlar.

3. Oturum açmak için bir kiracı için genel yönetici kimlik bilgilerini kullanmak [VaaS portalı](https://azurestackvalidation.com/
). Seçin **Hesabımı**.

    ![VaaS portalda oturum](media/vaas_portalsignin.png)

3. Site için VaaS erişim vermek için istenir. Devam etmek için koşullarını kabul edin.

## <a name="assign-user-roles"></a>Kullanıcı Rolleri Ata

Bir kullanıcı rolüne atamak için:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Seçin **tüm hizmetleri** > **Azure Active Directory** içinde **kimlik** grubu.
3. Seçin **kurumsal uygulamalar** > **Azure Stack doğrulama hizmeti** uygulama.
4. **Kullanıcı ve gruplar**'ı seçin. **Azure Stack doğrulama hizmeti - kullanıcı ve grup** dikey penceresinde uygulamayı kullanmak için izne sahip kullanıcılar listelenir.
5. Seçin **+ Ekle kullanıcı** atama eklemek için.

## <a name="create-an-azure-storage-blob-to-store-logs"></a>Günlükleri depolamak için bir Azure depolama blobu oluştur

VaaS doğrulama testleri çalıştırılırken tanılama günlükleri oluşturur. Bir Azure blob hizmeti SAS URL'Sİ'ın günlüklerinizi depolamak için bir konum ihtiyacınız var. Depolama hesabı deposu OEM özelleştirme paketler için de kullanılabilir.

Bu adımları yol ayarlayın ve Azure depolama hesabınız için bir hizmet olarak depolama (SAS) URI oluşturmak nasıl ve nerede testleri portalda başlatırken VaaS Portalı'nda depolama hesabı belirtin.

### <a name="create-an-azure-storage-account"></a>Azure Storage hesabı oluşturma

1. Depolama hesabı oluşturma yönergeleri için [depolama hesabı oluşturma](https://docs.microsoft.com/azure/storage/storage-create-storage-account#create-a-storage-account).

2. Depolama hesabı türü seçerken Seç **Blob Depolama** hesap türü.

### <a name="generate-a-sas-url-for-the-storage-account"></a>Depolama hesabı için bir SAS URL'si oluşturun

1. Yukarıda oluşturduğunuz depolama hesabına gidin.

2. Altındaki dikey penceresinde **ayarları**seçin **paylaşılan erişim imzası**.

3. Yalnızca denetle **Blob** gelen **izin verilen hizmetler seçenekleri** (kalan seçeneğinin işaretini kaldırın).

4. Denetleme **hizmet**, ** kapsayıcı ve **nesne** gelen **izin verilen kaynak türleri**.

5. Denetleme **okuma**, **yazma**, **listesi**, **Ekle**, **Oluştur** gelen **iznine**  (kalan seçeneğinin işaretini kaldırın).

6. Ayarlama **başlangıç zamanı** geçerli saat ve **bitiş saati** geçerli saatten üç ay için.

7. Seçin **SAS oluşturma ve bağlantı dizesini** kaydedip **Blob hizmeti SAS URL'si** dize.

> [!Note]  
> SAS URL'sini URL oluşturulduğunda ayarlayın son anda süresi dolar. Hata ayıklama için ürün ekibiyle paylaşmadan önce URL'nin yeterince geçerli olduğunu veya URL'nin testleri zamanlarken 30 günden fazla bir süre için geçerli olduğundan emin olun.

## <a name="next-steps"></a>Sonraki adımlar

- Donanımınız VaaS yerel aracı kullanın. Yönergeler için bkz [yerel aracısı ve test sanal makineleri dağıtmak](azure-stack-vaas-test-vm.md).
- Hakkında daha fazla bilgi edinmek için [hizmet olarak Azure Stack doğrulama](https://docs.microsoft.com/azure/azure-stack/partner).