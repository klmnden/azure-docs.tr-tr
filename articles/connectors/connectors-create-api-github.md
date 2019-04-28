---
title: GitHub - Azure Logic Apps'i bağlama | Microsoft Docs
description: GitHub REST API'leri ve Azure Logic Apps ile GitHub olay izleme
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.date: 03/02/2018
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: 0d2ff9368bc244a5afd6fafc40cf476b90a80a52
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61462599"
---
# <a name="connect-to-github"></a>Github'a bağlanma

GitHub tüm dağıtılmış düzeltme denetimi ve kaynak kodu Yönetimi (SCM) işlevlerini git'te yanı sıra diğer özellikleri sunan bir web tabanlı Git deposu barındırma hizmetidir.

GitHub Bağlayıcısı ile kullanmaya başlamak için [ilk mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-a-connection-to-github"></a>GitHub bağlantı oluşturun

Bir mantıksal uygulama çalıştırmasında GitHub Bağlayıcısı'nı kullanmak için öncelikle oluşturmalısınız bir *bağlantı* ve ardından bu özellikler için ayrıntıları sağlayın: 

| Özellik | Gerekli | Açıklama | 
| -------- | -------- | ----------- | 
| Belirteç | Evet | GitHub kimlik bilgilerinizi sağlayın. |

Bağlantıyı oluşturduktan sonra Eylemler yürütmek ve bu makalede açıklanan Tetikleyiciler dinler.

> [!INCLUDE [Steps to create a connection to GitHub](../../includes/connectors-create-api-github.md)]
> 

## <a name="connector-specific-details"></a>Özel bağlayıcı ayrıntıları

Tetikleyiciler ve Eylemler Swagger ve herhangi bir sınırlama tanımlanmış için gözden [bağlayıcı ayrıntıları](/connectors/github/).

## <a name="find-more-connectors"></a>Daha fazla bağlayıcı bulun

* Gözden geçirme [bağlayıcılar listesi](apis-list.md).