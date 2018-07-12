---
title: Dışarı aktarma ve Machine Learning Studio - Azure verilerinizi silme | Microsoft Docs
description: Azure Machine Learning Studio'da tarafından depolanan ürün içi verileri dışarı aktarma ve silme işlemi Azure portalından ve kimliği doğrulanmış REST API aracılığıyla da kullanılabilir. Telemetri verilerini Azure gizlilik Portal üzerinden erişilebilir. Bu makalede, nasıl gösterir.
services: machine-learning
author: heatherbshapiro
ms.author: hshapiro
manager: cgronlun
ms.reviewer: jmartens, mldocs
ms.service: machine-learning
ms.component: studio
ms.topic: conceptual
ms.date: 05/25/2018
ms.openlocfilehash: 2ebd777a9723732de6ebbdf07020802190cb4b61
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38969291"
---
# <a name="export-and-delete-in-product-user-data-from-machine-learning-studio"></a>Dışarı aktarma ve Machine Learning Studio'dan ürün içi kullanıcı verilerini sil

Silmeniz veya ürün içi verileri dışa Studio arabirimini, PowerShell, Azure portalı kullanarak Azure Machine Learning Studio'da tarafından depolanan ve kimliği doğrulanmış REST API'leri. Bu makalede nasıl yapılacağı açıklanmaktadır. 

Telemetri verilerini Azure gizlilik portal üzerinden erişilebilir. 

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="what-kinds-of-user-data-does-studio-collect"></a>Studio hangi tür kullanıcı verileri toplar?

Bu hizmet için kullanıcı verileri çalışma alanları ve telemetri kayıt hizmeti ile kullanıcı etkileşimi sonucu oluşan erişimi için yetkilendirilmiş kullanıcılar hakkında bilgi içerir.

Machine Learning Studio'da kullanıcı verilerini iki tür vardır:
- **Kişisel hesap verileri:** hesabı kimlikleri ve hesapla ilişkili e-posta adresleri.
- **Müşteri verileri:** çözümlemek için yüklediğiniz verileri.

## <a name="studio-account-types-and-how-data-is-stored"></a>Studio hesap türleri ve veriler nasıl depolanır

Machine Learning Studio'da hesapları üç tür vardır. Verilerinizin nasıl depolandığını ve nasıl silebilir veya dışarı aktarmak, sahip olduğunuz hesap türünü belirler.

- A **Konuk çalışma** ücretsiz, anonim bir hesaptır. Size bir e-posta adresi veya parola gibi kimlik bilgilerini girmeden kaydolun.
    -  Veri Konuk çalışma süresi dolduktan sonra temizlenir.
    - Konuk kullanıcılar, kullanıcı Arabirimi, REST API veya PowerShell paketi aracılığıyla müşteri verilerini dışarı aktarabilirsiniz.
- A **ücretsiz çalışma alanı** oturum açtığınızda kimlik bilgileri - bir e-posta adresi ve parola ile Microsoft hesabı, ücretsiz bir hesap.
    - Dışarı aktarma ve veri sahibi hakları (DSR) isteklerini tabi olan kişisel ve müşteri verilerini sil.
    - Kullanıcı Arabirimi, REST API veya PowerShell paketi aracılığıyla müşteri verilerini dışarı aktarabilirsiniz.
    - Ücretsiz çalışma alanları, Azure AD hesapları kullanmayan telemetri gizlilik portalını kullanarak dışarı aktarılabilir.
    - Çalışma alanını sildiğinizde, tüm kişisel müşteri verilerini silin.
- A **standart çalışma** bir Ücretli hesap oturum açma kimlik bilgileriyle erişim.
    - Dışarı aktarma ve DSR isteklerini tabi olan kişisel ve müşteri verilerini sil.
    - Azure gizlilik portal aracılığıyla verilere erişebilir
    - Kullanıcı Arabirimi, REST API veya PowerShell paketi aracılığıyla kişisel ve müşteri verilerini dışarı aktarma
    - Azure portalındaki verilerinize silebilirsiniz.

## <a name="delete-workspace-data-in-studio"></a>Studio çalışma alanı verilerini sil 

### <a name="delete-individual-assets"></a>Tek tek varlığını silme

Kullanıcılar, bunları seçerek ve ardından Sil düğmesini seçerek bir çalışma alanında varlıklar silebilir.

![Machine Learning Studio'da varlığını silme](./media/export-delete-personal-data-dsr/delete-studio-asset.png)

### <a name="delete-an-entire-workspace"></a>Tüm bir çalışma alanını silme

Kullanıcılar, kendi tüm çalışma alanını silebilirsiniz:
- Çalışma alanını Ücretli: Azure portalından silin.
- Ücretsiz çalışma alanı: Sil düğmesini kullanın **ayarları** bölmesi.

![Machine Learning Studio'da ücretsiz çalışma alanını silme](./media/export-delete-personal-data-dsr/delete-studio-data-workspace.png)
 
## <a name="export-studio-data-with-powershell"></a>PowerShell ile Studio verileri dışarı aktarma
Tüm bilgiler, Azure Machine Learning komutları kullanarak Studio'dan taşınabilir bir biçime dışarı aktarmak için PowerShell kullanın. Bilgi için [Azure Machine Learning için PowerShell Modülü](powershell-module.md) makalesi.

## <a name="next-steps"></a>Sonraki adımlar

Web Hizmetleri ve faturalama Taahhütlü bir plana kapsayan belgeler için bkz: [Azure Machine Learning REST API Başvurusu](https://docs.microsoft.com/rest/api/machinelearning/). 
