---
title: Azure Key Vault güvenlik dünyaları | Microsoft Docs
ms.service: key-vault
ms.topic: conceptual
author: msmbaldwin
ms.author: mbaldwin
manager: barbkess
ms.date: 07/03/2017
ms.openlocfilehash: d75ce953f73912428ee88fe2e19a138799f979fa
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64704704"
---
# <a name="azure-key-vault-security-worlds-and-geographic-boundaries"></a>Azure Key Vault güvenlik dünyaları ve coğrafi sınırlar

Azure Key Vault bir çok kiracılı bir hizmettir ve havuzu donanım güvenlik modülleri (HSM'ler) içinde her Azure konumu kullanır. 

Aynı coğrafi bölgede Azure konumlarında tüm HSM'ler aynı şifreleme sınırı (Thales güvenlik Dünyası) paylaşın. Örneğin, ABD coğrafi konuma ait oldukları için Doğu ABD ve Batı ABD aynı güvenlik Dünyası paylaşın. Benzer şekilde, tüm Azure konumlarında Japonya aynı güvenlik dünyası ve Avustralya, Hindistan ve benzeri tüm Azure konumlarında paylaşın. 

## <a name="backup-and-restore-behavior"></a>Yedekleme ve geri yükleme davranışı

Bu koşulların her ikisinin de doğru olduğu sürece başka bir Azure konumunda bir anahtar kasasına bir anahtarı bir anahtar kasasından tek bir Azure konumda gerçekleştirilen bir yedekleme geri yüklenebilir:

- Azure konumları her ikisi de aynı coğrafi konuma ait
- Anahtar kasaları her ikisi de aynı Azure aboneliğine ait

Örneğin, bir yedek anahtarı bir anahtar kasasındaki Batı Hindistan, belirli bir abonelik tarafından gerçekleştirilecek yalnızca geri yükleyebilirsiniz aynı abonelikte ve coğrafi konumdaki başka bir anahtar kasasına; Batı Hindistan, Orta Hindistan veya Güney Hindistan.

## <a name="regions-and-products"></a>Bölgeler ve ürünler

- [Azure bölgeleri](https://azure.microsoft.com/regions/)
- [Bölgelere göre Microsoft ürünleri](https://azure.microsoft.com/regions/services/)

Tablolar büyük başlıklar olarak gösterilen, güvenlik dünyaları bölgeleri eşlenir:

Bölge makale tarafından ürünleri **Americas** sekmesini içeren Doğu ABD, Orta ABD, Batı ABD Kuzey ve Güney Amerika bölgesine tüm eşleme. 

>[!NOTE]
>Bir özel durum, US DOD Doğu ve ABD DOD Orta kendi güvenlik dünyaları sahip olduğunu belirtir. 

Benzer şekilde, üzerinde **Avrupa** sekmesinde, Kuzey Avrupa ve Batı Avrupa için Avrupa bölgesinde her ikisi de eşlenir. Aynı de true olarak **Asya Pasifik** sekmesi.



