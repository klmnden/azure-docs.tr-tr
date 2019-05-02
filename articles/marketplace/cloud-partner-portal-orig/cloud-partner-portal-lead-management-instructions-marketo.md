---
title: Marketo müşteri adayı yönetimini yapılandırma | Azure Market
description: Azure Market müşterileri için Marketo için sağlama yönetimi yapılandırın.
services: Azure, Marketplace, Cloud Partner Portal,
author: dan-wesley
ms.service: marketplace
ms.topic: conceptual
ms.date: 09/14/2018
ms.author: pabutler
ms.openlocfilehash: d749a92a1837bad053dc586477bfc27fc65299fa
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64935222"
---
# <a name="configure-lead-management-in-marketo"></a>Marketo müşteri adayı yönetimini yapılandırma

Bu makalede, Microsoft müşteri adaylarını işlemek için Marketo ayarlama açıklanır.

1. Marketo için oturum açın.
2. Seçin **tasarım Studio**.
    ![Marketo tasarım Studio](./media/cloud-partner-portal-lead-management-instructions-marketo/marketo1.png)

3.  Seçin **yeni formu**.
    ![Marketo yeni formu](./media/cloud-partner-portal-lead-management-instructions-marketo/marketo2.png)

4.  Yeni Form gerekli alanları doldurun ve ardından **Oluştur**.
    ![Marketo yeni form oluşturma](./media/cloud-partner-portal-lead-management-instructions-marketo/marketo3.png)

4.  Alan hakkında ayrıntılı bilgi seçin **son**.
    ![Marketo son formu](./media/cloud-partner-portal-lead-management-instructions-marketo/marketo4.png)

5.  Onayla ve kapatın.

6.  MarketplaceLeadBacked sekmesinde **ekleme kodu**.
    ![Marketo ekleme kodu seçeneği](./media/cloud-partner-portal-lead-management-instructions-marketo/marketo5.png)

7.  Marketo kod ekleme, aşağıdaki örneğe benzer bir kod görüntüler.

`<script src="//app-ys12.marketo.com/js/forms2/js/forms2.min.js"></script>`

    <form id="mktoForm_1179"></form>
    <script>MktoForms2.loadForm("("//app-ys12.marketo.com", "123-PQR-789", 1179);</script>

1. Ekleme kodu, yapılandırabilmek için gösterilen değerleri kopyalayın **sunucu kimliği**, **Munchkin kimliği**, ve **Form kimliği** bulut iş ortağı portalı Marketo alanları.

Sonraki örnek, Marketo katıştırma kod örneğindeki ihtiyacınız kimliklerini almak için bir kılavuz olarak kullanın.

- Server Id = **ys12**
- Munchkin Id = **123-PQR-789**
- Form kimliği = **1179**\
