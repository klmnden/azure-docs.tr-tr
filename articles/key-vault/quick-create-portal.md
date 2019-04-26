---
title: "Azure Hızlı Başlangıç: Azure portalı kullanarak Key Vault'tan gizli dizi ayarlama ve alma | Microsoft Docs"
description: Azure portalı kullanarak Azure Key Vault'tan gizli dizi ayarlamayı ve almayı gösteren hızlı başlangıç
services: key-vault
author: barclayn
manager: barbkess
tags: azure-resource-manager
ms.service: key-vault
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/14/2019
ms.author: barclayn
ms.openlocfilehash: 0902e3fb64a73a095b457306aa561c13519e4066
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60203643"
---
# <a name="quickstart-set-and-retrieve-a-secret-from-azure-key-vault-using-the-azure-portal"></a>Hızlı Başlangıç: Ayarlayın ve Azure portalını kullanarak Azure Key Vault gizli dizi alma

Azure Key Vault, gizli diziler için güvenli bir depolama sağlayan bulut hizmetidir. Anahtarları, parolaları, sertifikaları ve diğer gizli dizileri güvenli bir şekilde depolayabilirsiniz. Azure anahtar kasaları Azure portalı aracılığıyla oluşturulup yönetilebilir. Bu hızlı başlangıçta, bir anahtar kasası oluşturacak ve ardından bir gizli diziyi depolamak için kullanacaksınız. Key Vault hakkında daha fazla bilgi için [Genel Bakış](key-vault-overview.md) bölümünü inceleyin.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="create-a-vault"></a>Kasa oluşturma

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** seçeneğini belirleyin

    ![Key Vault oluşturma işlemi tamamlandıktan sonra alınan çıktı](./media/quick-create-portal/search-services.png)
2. Arama kutusuna **Key Vault** yazın.
3. Sonuç listesinden **Key Vault**’u seçin.
4. Key Vault bölümünde **Oluştur**’u seçin.
5. **Anahtar kasası oluşturma** bölümünde aşağıdaki bilgileri sağlayın:
    - **Ad**: Benzersiz bir ad gereklidir. Bu hızlı başlangıç için **Contoso-vault2** kullanılır. 
    - **Abonelik**: Bir abonelik seçin.
    - **Kaynak Grubu** altında **Yeni oluştur**’u seçin ve bir kaynak grubu adı girin.
    - **Konum** açılır menüsünden bir konum seçin.
    - Diğer seçenekleri varsayılan değerlerinde bırakın.
6. Yukarıdaki bilgileri girdikten sonra **Oluştur**’u seçin.

Aşağıda listelenen iki özelliği not edin:

* **Kasa adı**: Bu örnekte, olan **Contoso-Vault2**. Bu adı diğer adımlar için kullanacaksınız.
* **Kasa URI'si**: Bu örnekte, olan https://contoso-vault2.vault.azure.net/. REST API'si aracılığıyla kasanızı kullanan uygulamaların bu URI'yi kullanması gerekir.

Bu noktada Azure hesabınız, bu yeni anahtar kasasında işlemler gerçekleştirmeye yetkili olan tek hesaptır.

![Key Vault oluşturma işlemi tamamlandıktan sonra alınan çıktı](./media/quick-create-portal/vault-properties.png)

## <a name="add-a-secret-to-key-vault"></a>Key Vault’a gizli dizi ekleme

Kasaya bir gizli dizi eklemek için birkaç ek adım uygulamanız gerekir. Bu örnekte, bir uygulama tarafından kullanılabilecek bir gizli dizi ekleyeceğiz. Parola olarak adlandırılır **ExamplePassword** ve değerini depolarız **hVFkk965BuUv** da.

1. Key Vault özellikleri sayfalarında **Gizli Diziler**’i seçin.
2. **Oluştur/İçeri Aktar**’a tıklayın.
3. **Bir gizli dizi oluştur** ekranında aşağıdaki değerleri seçin:
    - **Karşıya yükleme seçenekleri**: El ile.
    - **Ad**: ExamplePassword.
    - **Değer**: hVFkk965BuUv
    - Diğer değerleri varsayılan değerlerinde bırakın. **Oluştur**’a tıklayın.

Gizli dizinin başarıyla oluşturulduğunu belirten iletiyi aldıktan sonra listede gizli diziye tıklayabilirsiniz. Ondan sonra bazı özellikleri görebilirsiniz. Geçerli sürüme tıklarsanız önceki adımda belirtilen değeri görebilirsiniz.

![Gizli dizi özellikleri](./media/quick-create-portal/current-version-hidden.png)

Sağ bölmede "Gizli dizi değerini göster" düğmesine tıklayarak, gizli değeri görebilirsiniz. 

![Gizli değer göründü](./media/quick-create-portal/current-version-shown.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Diğer Key Vault hızlı başlangıçları ve öğreticileri bu hızlı başlangıcı temel alır. Sonraki hızlı başlangıç ve öğreticilerle çalışmaya devam etmeyi planlıyorsanız, bu kaynakları yerinde bırakmanız yararlı olabilir.
Artık gerek kalmadığında kaynak grubunu silin; bunu yaptığınızda Key Vault ve ilgili kaynaklar silinir. Kaynak grubunu portal aracılığıyla silmek için:

1. Portalın üst kısmındaki Arama kutusuna kaynak grubunuzun adını girin. Bu hızlı başlangıçta kullanılan kaynak grubunu arama sonuçlarında gördüğünüzde seçin.
2. **Kaynak grubunu sil**'i seçin.
3. **KAYNAK GRUBU ADINI YAZIN:** kutusuna kaynak grubunun adını yazın ve **Sil**’i seçin.


## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta bir Key Vault oluşturdunuz ve bir gizli dizi depoladınız. Key Vault ve bunu uygulamalarınızla nasıl kullanabileceğiniz hakkında daha fazla bilgi almak için, Key Vault ile birlikte çalışan web uygulamalarına yönelik öğreticiye geçin.

> [!div class="nextstepaction"]
> Azure kaynakları için yönetilen kimlikler kullanan bir web uygulamasındaki Key Vault’tan bir gizli diziyi okuma hakkında bilgi almak için aşağıdaki [Bir Azure web uygulamasını Key Vault’tan gizli dizi okuyacak şekilde yapılandırma](quick-create-net.md) öğreticisine geçin.
