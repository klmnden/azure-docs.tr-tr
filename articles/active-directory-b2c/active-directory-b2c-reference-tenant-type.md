---
title: Bölge kullanılabilirliği ve veri yerleşikliğinin yanı sıra Azure Active Directory B2C | Microsoft Docs
description: Azure Active Directory B2C kiracıları türlerinde bir konu.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/10/2017
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 30f13a3b85e68babcaef62b9200a296105b3ce83
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66508998"
---
# <a name="azure-active-directory-b2c-region-availability--data-residency"></a>Azure Active Directory B2C: Bölge kullanılabilirliği ve veri yerleşikliği
Bölge kullanılabilirliği ve veri yerleşikliği Azure AD B2C'ye Azure geri kalanından farklı uygulanan çok farklı kavramları vardır. Bu makalede, bu iki konsepti arasındaki farkları açıklar ve nasıl Azure ve Azure AD B2C uygulandıkları karşılaştırın.

## <a name="summary"></a>Özet
Azure AD B2C **sunuldu dünya çapında** için seçeneğiyle **Amerika Birleşik Devletleri veya Avrupa veri yerleşikliği**.

## <a name="concepts"></a>Kavramlar
* **Bölge kullanılabilirliği** hizmet kullanılabilir olduğu için ifade eder.
* **Veri yerleşikliği** kullanıcı verilerinin depolandığı anlamına gelir.

## <a name="region-availability"></a>Bölge kullanılabilirliği
Azure AD B2C, tüm dünyada Azure genel bulut kullanılabilir. 

Bu kullanılabilirlik veri yerleşimi ile eşleştiği çoğu diğer Azure hizmetlerini izleme modelden farklıdır. Hem Azure'nın bu örneklerde gördüğünüz [tarafından ürünleri kullanılabilir bölge](https://azure.microsoft.com/regions/services/) sayfası ve [Active Directory B2C fiyatlandırma hesaplayıcısını](https://azure.microsoft.com/pricing/details/active-directory-b2c/).

## <a name="data-residency"></a>Veri yerleşikliği
Azure AD B2C kullanıcı verilerini Amerika Birleşik Devletleri veya Avrupa içinde depolar.

Veri yerleşikliği hangi ülke/bölge seçili göre belirlenir, [Azure AD B2C kiracısı oluşturma](active-directory-b2c-get-started.md).

![Önizleme kiracısı ekran görüntüsü](./media/active-directory-b2c-reference-tenant-type/data-residency-b2c-tenant.png)

Şu ülkeler/bölgeler için ABD verileri yer:

> Amerika Birleşik Devletleri, Kanada, Kosta Rika, Dominik Cumhuriyeti, El Salvador, Guatemala, Meksika, Panama, Porto Riko ve Trinidad ve Tobago

Veriler aşağıdaki ülkelerde/bölgelerde Avrupa'da bulunur:

> Cezayir, Avusturya, Azerbaycan, Bahreyn, Belarus, Belçika, Bulgaristan, Hırvatistan, Kıbrıs, Çek Cumhuriyeti, Danimarka, Mısır, Estonya, Finlandiya, Fransa, Almanya, Yunanistan, Macaristan, İzlanda, İrlanda, İsrail, İtalya, Ürdün, Kazakistan, Kenya, Kuveyt, Lativa, Lübnan, Liechtenstein, Litvanya, Lüksemburg, Kuzey Makedonya, Malta, Karadağ, Fas, Hollanda, Nijerya, Norveç, Umman, Pakistan, Polonya, Portekiz, Katar, Romanya, Rusya, Suudi Arabistan, Sırbistan, Slovakya, Slovenya, Güney Afrika, İspanya, İsveç, İsviçre, Tunus, Türkiye, Ukrayna, Birleşik Arap Emirlikleri ve Birleşik Krallık.

Diğer ülkeler/bölgeler listeye eklenen sürecinde olan.  Şu an için Azure AD B2C ülkeler/bölgeler yukarıdaki seçerek kullanmaya devam edebilirsiniz.

> Afganistan, Arjantin, Avustralya, Brezilya, Şili, Kolombiya, Ekvador, Hong Kong ÖİB, Hindistan, Endonezya, Irak, Japonca, Kore, Malezya, Yeni Zelanda, Paraguay, Peru, Filipinler, Singapur, Sri Lanka, Tayvan, Tayland, Uruguay ve Venezuela.

## <a name="preview-tenant"></a>Önizleme kiracısı
Azure AD B2C'in Önizleme dönemi boyunca bir B2C kiracısı oluşturduysanız, büyük olasılıkla, uygulamanızın **Kiracı türü** diyor **Önizleme kiracısı**. Bu durumda, kiracınıza yalnızca geliştirme ve test amaçlıdır ve üretim uygulamaları için kullanmanız gerekir.

> [!IMPORTANT]
> Üretim ölçeği-B2C kiracısına bir B2C kiracısı preview sürümünden geçiş yolu yoktur. Bir önizleme B2C kiracısı silmek ve bir üretim ölçeği-B2C kiracısı aynı etki alanı adıyla yeniden oluştur olduğu bilinen Not verir. Farklı bir etki alanı adıyla bir üretim ölçeği-B2C kiracısı oluşturma gerekir.


![Önizleme kiracısı ekran görüntüsü](./media/active-directory-b2c-reference-tenant-type/preview-b2c-tenant.png)
