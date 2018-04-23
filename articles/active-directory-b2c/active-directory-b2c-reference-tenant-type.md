---
title: 'Azure Active Directory B2C: Bölge kullanılabilirliği & veri residency | Microsoft Docs'
description: Bir konu Azure Active Directory B2C kiracılar türleri hakkında
services: active-directory-b2c
documentationcenter: ''
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 04/10/2017
ms.author: davidmu
ms.openlocfilehash: b982a6b6e0068370ef7b5cb30ea689829e90d494
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="azure-active-directory-b2c-region-availability--data-residency"></a>Azure Active Directory B2C: Bölge kullanılabilirliği & veri residency
Bölge kullanılabilirliği ve veri residency Azure geri kalanından farklı Azure AD B2C'ye uygulamak iki çok farklı kavram olmasıdır. Bu makalede, bu iki kavram arasındaki farkları açıklamaktadır ve nasıl Azure Azure AD B2C ile uygulandıkları karşılaştırın.

## <a name="summary"></a>Özet
Azure AD B2C olan **genel olarak kullanılabilir dünya çapında** için seçeneğiyle **Amerika Birleşik Devletleri ya da Avrupa veri residency**.

## <a name="concepts"></a>Kavramlar
* **Bölge kullanılabilirliği** hizmet kullanılabilir olduğu için başvuruyor.
* **Veri residency** kullanıcı verilerinin depolandığı anlamına gelir.

## <a name="region-availability"></a>Bölge kullanılabilirliği
Azure AD B2C, dünya çapında Azure genel bulut kullanılabilir. 

Bu kullanılabilirlik veri residency ile eşleştiği en diğer Azure Hizmetleri izleyin modelden farklıdır. Her iki Azure'nın bu örneklerde görebilirsiniz [tarafından ürünleri kullanılabilir bölge](https://azure.microsoft.com/regions/services/) sayfa ve [Active Directory B2C fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/details/active-directory-b2c/).

## <a name="data-residency"></a>Veri yerleşikliği
Azure AD B2C, Amerika Birleşik Devletleri veya Avrupa kullanıcı verilerini depolar.

Veri residency hangi ülke/bölge seçili göre belirlenir zaman [Azure AD B2C kiracısı oluşturma](active-directory-b2c-get-started.md).

![Önizleme Kiracı ekran görüntüsü](./media/active-directory-b2c-reference-tenant-type/data-residency-b2c-tenant.png)

Amerika Birleşik Devletleri aşağıdaki ülkelerde/bölgelerde için veri bulunur:

> Amerika Birleşik Devletleri, Kanada, Kosta Rika, Dominik Cumhuriyeti, El Salvador, Guatemala, Meksika, Panama, Porto Riko ve Trinidad ve Tobago

Veri Avrupa'da aşağıdaki ülkelerde/bölgelerde bulunur:

> Cezayir, Avusturya, Azerbaycan, Bahreyn, Belarus, Belçika, Bulgaristan, Hırvatistan, Kıbrıs, Çek Cumhuriyeti, Danimarka, Mısır, Estonya, Finlandiya, Fransa, Almanya, Yunanistan, Macaristan, İzlanda, İrlanda, İsrail, İtalya, Ürdün, Kazakistan, Kenya, Kuveyt, Lativa, Lübnan, Liechtenstein, Lituania, Lüksemburg, Makedonya (EYC), Malta, Karadağ, Fas, Hollanda, Nijerya, Norveç, Umman, Pakistan, Polonya, Portekiz, Katar, Romanya, Rusya, Suudi Arabistan, Sırbistan, Slovakya, Slovenya, Güney Afrika, İspanya, İsveç, İsviçre, Tunus, Türkiye, Ukrayna, Birleşik Arap Emirlikleri ve İngiltere.

Kalan ülkelerde/bölgelerde listeye eklenen sürecinde ' dir.  Şimdilik, Azure AD B2C ülkelerde/bölgelerde yukarıdaki birini seçerek kullanmaya devam edebilirsiniz.

> Afganistan, Arjantin, Avustralya, Brezilya, Şili, Kolombiya, Ekvator, Hong Kong ÖİB, Hindistan, Endonezya, Irak, Japonya, Kore, Malezya, Yeni Zelanda, Paraguay, Peru, Filipin, Singapur, Sri Lanka, Tayvan, Tayland, Uruguay ve Venezuela.

## <a name="preview-tenant"></a>Önizleme kiracısı
Azure AD B2C'ın Önizleme dönemi boyunca B2C Kiracı oluşturduysanız olasıdır, **Kiracı türü** diyor **Önizleme Kiracı**. Bu durumda, yalnızca geliştirme ve test amaçlıdır ve üretim uygulamaları için değil, Kiracı kullanmanız gerekir.

> [!IMPORTANT]
> Üretim ölçeği B2C Kiracı Önizleme B2C Kiracı hiçbir geçiş yolu yoktur. Önizleme B2C Kiracı sildiğinizde ve bir üretim ölçeği B2C Kiracı aynı etki alanı adı ile yeniden oluşturmanız bilinen vardır Not verir. Farklı bir etki alanı adı ile bir üretim ölçeği B2C Kiracı oluşturmanız gerekir.


![Önizleme Kiracı ekran görüntüsü](./media/active-directory-b2c-reference-tenant-type/preview-b2c-tenant.png)
