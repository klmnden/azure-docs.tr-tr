---
title: Dışarı aktarma ve Machine Learning Studio - Azure verilerinizi silmek | Microsoft Docs
description: Azure Machine Learning Studio tarafından depolanan ürün verileri dışarı aktarma ve Azure Portalı aracılığıyla ve ayrıca kimliği doğrulanmış REST API'leri aracılığıyla silme işlemi için kullanılabilir. Telemetri verileri Azure gizlilik Portalı aracılığıyla erişilebilir. Bu makale size nasıl gösterir.
services: machine-learning
author: heatherbshapiro
ms.author: hshapiro
manager: cgronlun
ms.reviewer: jmartens, mldocs
ms.service: machine-learning
ms.component: studio
ms.topic: conceptual
ms.date: 05/25/2018
ms.openlocfilehash: 6317d4baba5775c1e5a4fda66de80dd8299d8fed
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655777"
---
# <a name="export-and-delete-in-product-user-data-from-machine-learning-studio"></a>Dışarı aktarma ve Machine Learning Studio'dan ürün kullanıcı verilerini sil

Silebilmek için veya ürün verileri dışa Studio arabirimi, PowerShell Azure portalını kullanarak Azure Machine Learning Studio tarafından depolanır ve REST API'leri kimlik doğrulaması. Bu makalede bunun nasıl yapılacağı açıklanmaktadır. 

Telemetri verileri Azure gizlilik Portalı aracılığıyla erişilebilir. 

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="what-kinds-of-user-data-does-studio-collect"></a>Kullanıcı verilerini ne tür Studio topluyor?

Bu hizmet için kullanıcı verilerini çalışma alanları ve hizmeti ile kullanıcı etkileşimleri telemetri kayıtları erişimi için yetkilendirilmiş kullanıcılar hakkında bilgi içerir.

İki tür kullanıcı veri Machine Learning Studio'da vardır:
- **Kişisel hesap verilerini:** hesap kimlikleri ve hesapla ilişkili e-posta adresi.
- **Müşteri verileri:** çözümlemek için yüklenen veriler.

## <a name="studio-account-types-and-how-data-is-stored"></a>Studio hesap türleri ve verilerin depolanma şeklini

Üç türde Machine Learning Studio'da hesapları vardır. Sahip olduğunuz hesap türü, verilerin depolanma şeklini ve nasıl silin veya dışa belirler.

- A **Konuk çalışma** ücretsiz, anonim bir hesaptır. Size bir e-posta adresi veya parola gibi kimlik bilgilerini sağlamadan kaydolun.
    -  Konuk çalışma süresi dolduktan sonra veri temizlenir.
    - Konuk kullanıcılar, kullanıcı Arabirimi, REST API'leri veya PowerShell paket müşteri verilerine dışarı aktarabilirsiniz.
- A **ücretsiz çalışma** oturum açtığınızda Microsoft hesap kimlik - bir e-posta adresi ve parola ücretsiz bir hesap.
    - Dışarı aktarma ve veri konu hakları (DSR) istekleri tabi kişisel ve müşteri verilerini silin.
    - Kullanıcı Arabirimi, REST API'leri veya PowerShell paket müşteri verilerine dışarı aktarabilirsiniz.
    - Ücretsiz Azure AD hesapları kullanmayan çalışma alanları, telemetri gizlilik Portalı'nı kullanarak dışa aktarılabilir.
    - Çalışma alanı sildiğinizde, tüm kişisel müşteri verilerini silin.
- A **standart çalışma** oturum açma kimlik bilgileriyle erişim bir Ücretli hesabıdır.
    - Dışarı aktarma ve tabi DSR istekleri, kişisel ve müşteri verilerini silin.
    - Azure gizlilik Portalı aracılığıyla verilere erişebilir
    - Kullanıcı Arabirimi, REST API'leri veya PowerShell paket kişisel ve müşteri verilerine verebilirsiniz
    - Verilerinizi Azure portalında silebilirsiniz.

## <a name="delete-workspace-data-in-studio"></a>Studio çalışma alanı verilerini sil 

### <a name="delete-individual-assets"></a>Tek tek varlığını silme

Kullanıcılar çalışma alanındaki varlıklar bunları seçerek ve ardından Sil düğmesini seçerek silebilirsiniz.

![Machine Learning Studio'da varlığını silme](./media/export-delete-personal-data-dsr/delete-studio-asset.png)

### <a name="delete-an-entire-workspace"></a>Tüm çalışma alanını silme

Kullanıcılar kendi tüm çalışma alanını silebilirsiniz:
- Çalışma alanı Ücretli: Azure portalı üzerinden silin.
- Ücretsiz çalışma: Sil düğmesini kullanarak **ayarları** bölmesi.

![Machine Learning Studio'da boş bir çalışma alanı silme](./media/export-delete-personal-data-dsr/delete-studio-data-workspace.png)
 
## <a name="export-studio-data-with-powershell"></a>PowerShell ile Studio verileri dışarı aktarma
Tüm bilgilerinizi taşınabilir bir biçim Azure Machine Learning komutları kullanarak Studio'dan vermek için PowerShell kullanın. Bilgi için bkz: [Azure Machine Learning için PowerShell Modülü](powershell-module.md) makalesi.

## <a name="next-steps"></a>Sonraki adımlar

Web Hizmetleri ve faturalama taahhüt plan kapsayan belgeler için bkz: [Azure Machine Learning REST API Başvurusu](https://docs.microsoft.com/en-us/rest/api/machinelearning/). 
