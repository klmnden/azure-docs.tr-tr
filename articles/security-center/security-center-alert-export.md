---
title: Azure Log Analytics çalışma alanına uyarı günlükleri dışarı aktarma | Microsoft Docs
description: Bu makalede, olan uyarılar dışarı aktarmak açıklanmaktadır Azure Güvenlik Merkezi tarafından oluşturulan bir Log Analytics çalışma alanı.
services: security-center
documentationcenter: na
author: monhaber
manager: mbaldwin
editor: ''
ms.assetid: 2bc67ce5-ec3a-49df-afc3-b4bad5d8ce21
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/13/2019
ms.author: monhaber
ms.openlocfilehash: 12b8b376a0ff149cbfafc7fe69dd8da636280de8
ms.sourcegitcommit: a4efc1d7fc4793bbff43b30ebb4275cd5c8fec77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2019
ms.locfileid: "56653727"
---
# <a name="export-azure-security-center-alerts"></a>Azure Güvenlik Merkezi uyarılarını dışarı aktarma
Bu makalede, diğer araçları tarafından kullanılabilecek bir konuma Azure Güvenlik Merkezi tarafından oluşturulan uyarıların dışarı aktarmak açıklanmaktadır.

Güvenlik Merkezi'nin müşteriler bazıları (örneğin, tehdit algılama uyarıları, güvenlik ilkesi önerileri, kapsamı, toplanmış ve işlenen verilerin tümünü içeren bir merkezi konumdan Güvenlik Merkezi tarafından oluşturulan verileri kullanmasına gerek belirtmiş Güvenli puanı, vb.). Bu, daha fazla işleme ve analiz edilmesi veriler sağlar. Veya, Güvenlik Merkezi'nin portalı yerine verileri programlı şekilde kullanma başka bir yolu olabilir.

Azure Log Analytics çalışma alanları depolamak ve sanal makinelerden toplanan işlenmemiş verileri işlemek için kullanıldığından **dışarı Log Analytics'e** özelliği sağlar çözümü bu gereksinim için bir kullanıcı tanımlı çalışma alanına veri akışı tarafından.

## <a name="prerequisites"></a>Önkoşullar
Log Analytics çalışma alanına ihtiyacınız vardır. Veri Koleksiyonu Ayarları'nda tanımlanan aynı Log Analytics çalışma alanı kullanmanızı öneririz. veri toplama için bir tane tanımlarken. [Log Analytics çalışma alanı verileri toplama](security-center-enable-data-collection.md)


## <a name="export-the-alerts"></a>Uyarı verme
1. Tıklayın **Güvenlik İlkesi**.
1. Kullanmak istediğiniz aboneliği satırı tıklatın **ayarlarını Düzenle**.
  ![Ayarları Düzenle](./media/security-center-alert-export/edit_settings.png "ayarlarını Düzenle")
1. Tıklayın **veri toplama**.
1. Ekranı aşağı kaydırarak **Store Güvenlik Merkezi veri zenginleştirilmiş** Aç/Kapat seçeneğini tıklatıp **üzerinde**.

   ![Store veri seçeneği](./media/security-center-alert-export/store_data_option.png "güvenlik ilkesi")
1. Uyarılar için dışarı aktarmak istediğiniz çalışma alanını seçin.

    > [!NOTE]
    > Günlük uyarıları Güvenlik Merkezi varsayılan çalışma alanına dışarı aktaramazsınız. Belirtildiği gibi [önkoşulları](#Prerequisites), Log Analytics çalışma alanı kullanmanız gerekir.

1. Tıklayın **Kaydet** (ekranın üstünde).