---
title: Bir Azure Veri Gezgini kümesi ve CLI kullanarak veritabanı oluşturma
description: Bu makalede bir Azure Veri Gezgini kümesi ve Azure CLI kullanarak veritabanı oluşturma
services: data-explorer
author: radennis
ms.author: radennis
ms.reviewer: orspod
ms.service: data-explorer
ms.topic: howto
ms.date: 1/31/2019
ms.openlocfilehash: a4c9156ef80f05e247b1cfef0acd56b601a2db65
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55812693"
---
# <a name="create-an-azure-data-explorer-cluster-and-a-database-using-cli"></a>Bir Azure Veri Gezgini kümesi ve CLI kullanarak veritabanı oluşturma

Bu makalede bir Azure Veri Gezgini kümesi ve Azure CLI kullanarak bir veritabanı oluşturmayı açıklar.

## <a name="whats-the-difference-between-the-management-plane-and-data-plane-apis"></a>Yönetim düzlemi ve veri düzlemi API'leri arasındaki fark nedir

İki farklı API kitaplık, yönetim ve veri düzlemi API'leri.
Yönetim API'leri kullanılır kaynaklarını yönetmek, örneği için bir küme oluşturmak, bir veritabanı oluşturmak, veri bağlantısını Sil örnek sayısı vb. sayısını değiştirin. Veri düzlemi API'leri sorguları verilerle etkileşim kurmak için kullanılan veri vb. alın.

## <a name="configure-the-cli-parameters"></a>CLI parametreleri Yapılandır

Hesabınızda oturum açın

```Bash
az login
```

Abonelik oluşturulacak kümelendirmek istediğiniz ayarlayın.

```Bash
az account set --subscription "your_subscription"
```

## <a name="create-the-azure-data-explorer-cluster"></a>Azure Veri Gezgini kümesi oluşturma

Aşağıdaki komutu kullanarak kümenizi oluşturun.

```Bash
az kusto cluster create --name azureclitest --sku D11_v2 --resource-group testrg
```

Aşağıdaki değerleri sağlayın

   |**Ayar** | **Önerilen değer** | **Alan açıklaması**|
   |---|---|---|
   | ad | *azureclitest* | İstenen kümenizin adıdır.|
   | sku | *D13_v2* | Kümeniz için kullanılan SKU. |
   | resource-group | *testrg* | Burada küme oluşturulan kaynak grubu adı. |

İsterseniz, vb. küme kapasitesi gibi kullanabileceğiniz daha fazla isteğe bağlı parametre yok.

Kümenizi başarıyla oluşturulup oluşturulmadığını kontrol etmek için çalıştırabilirsiniz

```Bash
az kusto cluster show --name azureclitest --resource-group testrg
```

Sonuç "Başarılı" değerine sahip "provisioningState" içeriyorsa, bu küme başarıyla oluşturuldu anlamına gelir.

## <a name="create-the-database-in-the-azure-data-explorer-cluster"></a>Azure Veri Gezgini kümede veritabanı oluşturma

Aşağıdaki komutu kullanarak veritabanınızı oluşturun.

```Bash
az kusto database create --cluster-name azureclitest --name clidatabase --resource-group testrg --soft-delete-period 3650:00:00:00 --hot-cache-period 3650:00:00:00
```

Aşağıdaki değerleri sağlayın

   |**Ayar** | **Önerilen değer** | **Alan açıklaması**|
   |---|---|---|
   | Küme adı | *azureclitest* | Kümenizin adını burada oluşturulmalıdır.|
   | ad | *clidatabase* | İstenen veritabanınızın adı.|
   | resource-group | *testrg* | Burada küme oluşturulan kaynak grubu adı. |
   | Geçici silme süresi | *3650:00:00:00* | Sorgu kullanılabilir, böylece veri tutulması gereken süre miktarı. |
   | Sık erişimli-cache-süresi | *3650:00:00:00* | Veriler önbellekte tutulması gereken süre miktarı. |

Çalıştırarak, oluşturduğunuz veritabanını görebilirsiniz.

```Bash
az kusto database show --name clidatabase --resource-group testrg --cluster-name azureclitest
```

İşte bu kadar artık bir küme ve bir veritabanı vardır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Diğer hızlı başlangıçlarımızı ve öğreticilerimizi izlemeyi planlıyorsanız, oluşturduğunuz kaynakları tutun.

Bir küme sildiğinizde, tüm veritabanları da siler. Kullanım aşağıdaki kümenizi sildiğinizden komutu.

```Bash
az kusto cluster delete --name azureclitest --resource-group testrg
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Hızlı Başlangıç: Azure veri Gezgini'ne olay Hub'ından veri alma](ingest-data-event-hub.md)
