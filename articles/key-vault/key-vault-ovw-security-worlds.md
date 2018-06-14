---
ms.assetid: ''
title: Azure anahtar kasası güvenlik dünyaları | Microsoft Docs
ms.service: key-vault
author: lleonard-msft
ms.author: alleonar
manager: mbaldwin
ms.date: 07/03/2017
ms.openlocfilehash: aeab36153d3a4e004d12206bab92cea9b30400ad
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
ms.locfileid: "27928109"
---
# <a name="azure-key-vault-security-worlds-and-geographic-boundaries"></a>Azure anahtar kasası güvenlik dünyaları ve coğrafi sınırlar

Azure anahtar kasası çok kiracılı bir hizmet olduğundan ve her Azure konumu donanım güvenlik modülleri (HSM'ler) havuzu kullanır. 

Aynı coğrafi bölgede Azure konumlardaki tüm HSM'ler aynı şifreleme sınırı (Thales güvenlik Dünyası) paylaşır. Örneğin, ABD coğrafi konuma ait olduğundan Doğu ABD ve Batı ABD aynı güvenlik Dünyası paylaşır. Benzer şekilde, Japonya tüm Azure konumlarda aynı güvenlik dünyası ve Avustralya, Hindistan vb. tüm Azure konumlarda paylaşır. 

## <a name="backup-and-restore-behavior"></a>Yedekleme ve geri yükleme davranışı

Bu koşulların her ikisi de doğruysa sürece konumda başka bir Azure anahtar kasası için bir anahtarın tek bir Azure konumda anahtar Kasası'ndan alınan bir yedeklemeyi geri yüklenebilir:

- Azure konumları her ikisi de aynı coğrafi konuma ait
- Anahtar kasalarını her ikisi de aynı Azure aboneliğine ait

Örneğin, belirli bir aboneliğe Batı Hindistan, bir anahtar kasasına bir anahtar tarafından gerçekleştirilecek bir yedekleme yalnızca geri yükleyebilirsiniz aynı abonelikte ve coğrafi konumu başka bir anahtar kasasına; Batı Hindistan, Orta Hindistan veya Güney Hindistan.

## <a name="regions-and-products"></a>Bölgeler ve ürünler

- [Azure bölgeleri](https://azure.microsoft.com/regions/)
- [Bölgeye göre Microsoft ürünleri](https://azure.microsoft.com/regions/services/)

Bölgeler tablolarda ana başlıklar olarak gösterilen güvenlik dünyaları eşlenir:

Bölge makale tarafından ürünlerindeki **Americas** sekmesi içerir Doğu ABD, Orta ABD, Batı ABD Kuzey ve Güney Amerika bölgesine tüm eşlemesi. 

>[!NOTE]
>Bir özel durum BİZE savunma Bakanlığı Doğu ve BİZE savunma Bakanlığı merkezi kendi güvenlik dünyaları olmasıdır. 

Benzer şekilde, üzerinde **Avrupa** sekmesinde, Kuzey Avrupa ve Batı Avrupa Avrupa bölgesine her ikisi de eşlenir. Aynı de true olarak **Asya Pasifik** sekmesi.



