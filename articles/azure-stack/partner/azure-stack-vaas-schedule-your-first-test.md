---
title: İlk test zamanlamak için Azure Stack portalı için bir hizmet olarak doğrulama kullanın | Microsoft Docs
description: İlk test zamanlamak için Azure Stack portalı için bir hizmet olarak doğrulama kullanın.
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
ms.openlocfilehash: 0f681070df4b4b3384171c05edb3851abec2ab5c
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2018
ms.locfileid: "40235516"
---
# <a name="quickstart-use-the-validation-as-a-service-portal-to-schedule-your-first-test"></a>Hızlı Başlangıç: doğrulama ilk test zamanlamak için bir servis portalı kullanma

[!INCLUDE[Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Doğrulama donanımınız denetlemek için hizmet (VaaS) portalı ile ilk test zamanlaması hakkında bilgi edinin. Yerel aracı doğrulama testlerini çalıştırmadan önce doğrulanması gereken Azure Stack çözüm dağıtılması gerekir.

Bu hızlı başlangıçta, çözümünüze ekleyin ve testleri çalıştırın.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı takip önce olması gerekir:
 - Bir hizmet hesabı olarak bir doğrulama. Yönergeler için [doğrulama bir hizmet hesabı olarak ayarlama](azure-stack-vaas-set-up-account.md).  
- Sisteminizde yüklü yerel aracı. Yönergeler için [yerel aracısı ve test sanal makineleri dağıtmak](azure-stack-vaas-test-vm.md).

## <a name="add-a-new-solution"></a>Yeni bir çözüme ekleyin

1. Oturum [doğrulama portalı](https://azurestackvalidation.com).

    ![Doğrulama portalında oturum](media/vaas_portalsignin.png)  

2. Seçin **yeni çözüm**.
3. Çözüm için bir ad girin ve seçin **Kaydet**.

## <a name="create-a-solution-validation-workflow"></a>Bir çözümü doğrulama iş akışı oluşturma

1. Çözüm adı seçin.
2. Seçin **Yönet** üzerinde **çözüm doğrulamaları** Döşe.

    ![Çözüm doğrulamaları](media/image2.png)

## <a name="create-a-solution-workflow"></a>Bir çözümü iş akışı oluşturma

1. Seçin **yeni çözüm doğrulama**.
2. Doğrulama adını yazın.
3. Seçin **Minimum** veya **maksimum**.  
    - **En az**  
    Çözüm, desteklenen düğüm sayısı alt sınırı ile yapılandırılır.  
    - **En fazla**  
    Çözüm, en fazla desteklenen düğüm sayısını ile yapılandırılır.
4. Çevre bir parametreler ekleyin. Daha fazla bilgi için [çevre bir parametreler ekleme](#add-environmental-parameters).
5. Genel test parametrelerinizi ekleyin. Daha fazla bilgi için [ortak test parametreleri ekleme](#add-common-test-parameters).

    Test test tanımına bağlı olarak, ortak parametreleri bağımsız olarak bir değer girmesini gerektirebilir veya ortak parametre değeri geçersiz kılmak izin verebilir.
6. Tıklayın **Gönder** test zamanlamak için.

## <a name="add-environmental-parameters"></a>Çevre bir parametreler Ekle

Ortam şu parametreleri ekleyin:

| Test geçiş bilgileri | Gerekli | Açıklama |
| --- | --- | --- | --- |
| Azure Stack derleme | Gerekli | Azure Stack derleme (20170501.1 örneğin) bir sayı değeri geçerli bir Azure Stack yapı numarası, sürüm veya örneğin 1.0.170330.9 olmalıdır |
| Kiracı Kimliği | Gerekli | Active Directory Kiracı kimliği Bu bir GUID (örneğin ECA23256-6BA0-4F27-8E4D-AFB02F088363) olmalıdır |
| Bölge | Gerekli | Azure Stack Dağıtım bölgesi |
| Kiracı Resource Manager uç noktası | Gerekli | (Örneğin, Kiracı Azure Resource Manager işlemleri için uç nokta https://management.loc-ext.domain.com) |
| Yönetici Resource Manager uç noktası | Gerekli değil | (Örneğin, Kiracı Azure Resource Manager işlemleri için uç nokta https://management.loc-ext.domain.com) |
| Dış FQDN | Gerekli değil | Dış uç noktaları için sonek olarak kullanılan tam etki alanı adı. (örneğin local.azurestack.external veya redmond.contoso.com) |
| Düğüm sayısı | Gerekli | Çözümünüzdeki düğüm sayısı. |

## <a name="add-common-test-parameters"></a>Ortak test parametreleri ekleme

Aşağıdaki ortak test parametreleri ekleyin:

| Test geçiş bilgileri | Gerekli | Açıklama |
| --- | --- | --- |
| Kiracı Kullanıcı adı | Gerekli | Kiracı Kullanıcı adı (örneğin tenant@contoso.onmicrosoft.com) |
| Kiracı parolası | Gerekli | Kiracı parolası. |
| Hizmet Yöneticisi kullanıcı adı | Gerekli değil | Kiracı Kullanıcı adı (örneğin tenant@contoso.onmicrosoft.com) |
| Hizmet Yöneticisi parolası | Gerekli değil | Hizmet Yöneticisi kullanıcı adı (örneğin serviceadmin@contoso.onmicrosoft.com) |
| Bulut yönetici kullanıcı adı | Gerekli değil | Azure Stack etki alanı yöneticisi hesabı (örneğin, contoso\cloudadmin) |
| Bulut yönetici parolası | Gerekli değil | |
|  Tanılama bağlantı dizesi | Gerekli değil | Bir Azure depolama hesabı için hangi tanılama günlükleri test yürütme sırasında kopyalanacak SAS URI'si. Bkz: [günlüklerini depolamak için bir Azure depolama blob oluşturma](azure-stack-vaas-set-up-account.md#create-an-azure-storage-blob-to-store-logs). <br><br>Değerini **tanılama bağlantı dizesi** hizmeti tarafından depolanan ve bu parametreyi kullanın iş akışında tüm testleri zamanlama zaman sağlanan ortak parametresi. SAS URL'sini 30 gün içinde sona erme olduğunda, ortak parametreler sayfasında yeni bir SAS URL'si istenir. |
| Etiket - ad | Gerekli değil |  İş akışı etiketlemek için açıklayıcı bir etiket girilebilir. Bu etiket adıdır. |
| Etiket - değeri | Gerekli değil | İş akışı etiketlemek için açıklayıcı bir etiket girilebilir. Bu etiketin değerdir. |

## <a name="next-steps"></a>Sonraki adımlar

- [Yeni bir Azure Stack çözüm doğrula](azure-stack-vaas-validate-solution-new.md)  
- Hakkında daha fazla bilgi edinmek için [hizmet olarak Azure Stack doğrulama](https://docs.microsoft.com/azure/azure-stack/partner).
