---
title: Azure portalında Power BI Embedded kapasite oluşturma | Microsoft Docs
description: Bu makalede, Microsoft Azure'da bir Power BI Embedded kapasitesi oluşturmak nasıl size yol göstermektedir.
services: power-bi-embedded
documentationcenter: ''
author: markingmyname
manager: kfile
editor: ''
tags: ''
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 09/28/2017
ms.author: maghan
ms.openlocfilehash: 2fadfde13aee3aaf965c6ba30188544a8a075b9d
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="create-power-bi-embedded-capacity-in-the-azure-portal"></a>Azure portalında Power BI Embedded kapasite oluşturma

Bu makalede, Microsoft Azure'da bir Power BI Embedded kapasitesi oluşturmak nasıl size yol göstermektedir. Power BI Embedded Power BI özellikleri, hızlı bir şekilde etkileyici görseller, raporlar ve panolar uygulamalarınızla eklemenize yardımcı olarak basitleştirir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

> [!VIDEO https://www.youtube.com/embed/aXrvFfg_iSk]

## <a name="before-you-begin"></a>Başlamadan önce

Bu hızlı başlangıcı tamamlamak için şunlar gerekir:

* **Azure aboneliği:** ziyaret [Azure ücretsiz deneme sürümü](https://azure.microsoft.com/free/) hesap oluşturmak için.
* **Azure Active Directory:** aboneliğinizi bir Azure Active Directory (AAD) Kiracı ile ilişkilendirilmiş olması gerekir. Ve ***Azure'a Kiracı bir hesapla oturum açmanız gerekir***. Microsoft hesapları desteklenmez. Daha fazla bilgi edinmek için bkz. [Kimlik doğrulaması ve kullanıcı izinleri](../analysis-services/analysis-services-manage-users.md).
* **Power BI Kiracı:** AAD kiracınızda en az bir hesabı gerekir kaydolup kaydolmadığını için Power BI.
* **Kaynak grubu:** zaten bir kaynak grubunu kullanın veya [yeni bir tane oluşturun](../azure-resource-manager/resource-group-overview.md).

## <a name="create-a-capacity"></a>Bir kapasite oluşturma

1. [Azure portal](https://portal.azure.com/) oturum açın.

2. Seçin **kaynak oluşturma** > **veri + analiz**.

3. Arama kutusuna arama *Power BI Embedded*.

4. Power BI Embedded içinde seçin **oluşturma**.

5. Gerekli bilgileri doldurun ve ardından **oluşturma**.

    ![Yeni kapasitesi oluşturmak için doldurmak için alanları](media/create-capacity/azure-portal-create-power-bi-embedded.png)

    |Ayar |Açıklama |
    |---------|---------|
    |**Kaynak adı**|Kapasite tanımlamak için bir ad. Kaynak adı, Power BI yönetim portalındaki Azure portalına ek olarak görüntülenir.|
    |**Abonelik**|Abonelik karşı kapasite oluşturmak istersiniz.|
    |**Kaynak grubu**|Bu yeni kapasite içeren kaynak grubunu. Varolan bir kaynak grubu seçin veya başka bir oluşturun. Daha fazla bilgi için bkz. [Azure Resource Manager’a genel bakış](../azure-resource-manager/resource-group-overview.md).|
    |**Power BI kapasite yönetici**|Power BI kapasite Yöneticiler, Power BI Yönetim Portalı'nda kapasitesini görüntüleyin ve diğer kullanıcılara atama izinleri verin. Varsayılan olarak, hesabınız kapasite yöneticisidir. Kapasite yönetici Power BI kiracınızın içinde olmalıdır.|
    |**Konum**|Power BI kiracınız için barındırıldığı konumu. Bu ayar, farklı bir konum seçilemez otomatik olarak çözümlenir.|
    |**Fiyatlandırma katmanı**|İhtiyaçlarınıza uygun SKU (v çekirdek sayısı ve bellek boyutu) seçin.  Ayrıntılar için bkz [Power BI Embedded fiyatlandırması](https://azure.microsoft.com/pricing/details/power-bi-embedded/)|

6. **Oluştur**’u seçin.

Oluşturulması genellikle bir dakika altında alır; genellikle yalnızca birkaç saniye sayısı. Seçtiyseniz **panoya Sabitle**, yeni kapasitenizi görmek için panoya gidin. Ya da gidin **tüm hizmetleri** > **Power BI Embedded** kapasitenizi hazır olup olmadığını görmek için.

![Power BI Embedded kapasiteye sahip Azure portalı panosunun](media/create-capacity/azure-portal-dashboard.png)

## <a name="next-steps"></a>Sonraki adımlar

Yeni Power BI Embedded kapasitenizi kullanmak için çalışma alanları atamak için Power BI yönetim portalına göz atın. Daha fazla bilgi için bkz. [Power BI Premium ve Power BI Embedded kapasitelerini yönetme](https://powerbi.microsoft.com/documentation/powerbi-admin-premium-manage/).

Bu kapasite kullanmanız gerekmez, faturalama durdurulmasına duraklatın. Daha fazla bilgi için bkz: [duraklatma ve Power BI Embedded kapasitenizi Azure portalında Başlat](pause-start.md).

Power BI içeriğini, uygulamanızda katıştırma başlamak için bkz: [, Power BI panoları, raporları ve döşeme katıştırma](https://powerbi.microsoft.com/documentation/powerbi-developer-embedding-content/).

Başka sorunuz mu var? [Power BI topluluk isteyen deneyin](http://community.powerbi.com/)