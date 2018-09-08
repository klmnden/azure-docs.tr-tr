---
title: Yeni bir Azure Stack çözüm doğrulama | Microsoft Docs
description: Hizmet olarak doğrulama ile yeni bir Azure Stack çözüm doğrulamak hakkında bilgi edinin.
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
ms.openlocfilehash: 1e908a8cf5576ce3bc3d58d1ef6f29d596ebc58b
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44158186"
---
# <a name="validate-a-new-azure-stack-solution"></a>Yeni bir Azure Stack çözüm doğrula

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

İçin çözümü doğrulama iş akışı yeni Azure Stack çözümleri için sertifika kullanma hakkında bilgi edinin.

Tüm dünyada Microsoft ile anlaşılan ve Windows Server logo sertifikası gereksinimleri geçti donanım ürün reçetesi (BoM) bir Azure Stack çözümüdür. Bir çözüm olarak sınıflandırılan neden BoM donanım bir değişiklik olduğunda da çözüm doğrulama iş akışı kullanabilirsiniz *yeni*. Ne tetikleyecek hakkında sorularınız varsa bir **yeni** veya **yeniden sertifikalandırılmasını** bir çözümün başvurun [ vaashelp@microsoft.com ](mailto:vaashelp@microsoft.com).

Çözümünüzü onaylamak için iş akışı iki kez çalıştırın. İçin bir kez çalıştır *en düşük düzeyde* yapılandırması desteklenir. İçinde ikinci bir kez çalıştır *maksimum* yapılandırma. Her iki yapılandırma tüm sınamaları geçmesi durumunda Microsoft çözümü onaylar.

Bu hızlı başlangıçta, testleri çalıştırma ve çözümünüzü ekleme işlemini kullanmaya başlamanızı sağlar.

## <a name="add-a-new-solution"></a>Yeni bir çözüme ekleyin

1. Oturum [doğrulama portalı](https://azurestackvalidation.com).
2. Seçin **yeni çözüm**.
3. Seçin ve çözüm için bir ad girin **Kaydet**.

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
4. Seçin **karşıya** ve ardından, dağıtım yapılandırma dosyası ekleyin. Bu isteğe bağlı bir adımdır. Sonraki bölümde yer alan adımları izleyerek, test parametreleri de ekleyebilirsiniz.

    > [!note]  
    > Yapılandırma dosyanızı ortam parametrelerinde parametreleri ekleyerek bir genel test parametreleri bölümleri arabiriminde oluşturabilirsiniz. Doğrulanmakta olan Azure Stack dağıtım hizmeti tarafından oluşturulan dosya alabilir. Yönergeler için [iş akışı ortak parametreleri için bir hizmet olarak Azure Stack doğrulama](azure-stack-vaas-parameters.md).

5. Çevre bir parametreler ekleyin. Daha fazla bilgi için [çevre bir parametreler ekleme](#add-environmental-parameters).
6. Genel test parametrelerinizi ekleyin. Daha fazla bilgi için [ortak test parametreleri ekleme](#add-common-test-parameters).

    Test test tanımına bağlı olarak, ortak parametreleri bağımsız olarak bir değer girmesini gerektirebilir veya ortak parametre değeri geçersiz kılmak izin verebilir.

7. Tıklayın **Gönder** test zamanlamak için.

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

- [Randevularını yeniden zamanlayabilir veya testi iptal et](azure-stack-vaas-monitor-test.md#reschedule-a-test)
- Hakkında daha fazla bilgi edinmek için [hizmet olarak Azure Stack doğrulama](https://docs.microsoft.com/azure/azure-stack/partner).