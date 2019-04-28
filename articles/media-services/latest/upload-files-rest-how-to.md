---
title: REST kullanarak bir Azure Media Services hesabına dosya yükleme | Microsoft Docs
description: Medya içeriği oluşturma ve karşıya yükleme varlıklar Media Services'e almayı öğrenin.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2019
ms.author: juliako
ms.openlocfilehash: a241f66adecbab1d0b1462f379d3765d6c1de252
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61466808"
---
# <a name="upload-files-into-a-media-services-account-using-rest"></a>REST kullanarak bir Media Services hesabına dosya yükleme

Media Services'de dijital dosyalar bir varlık ile ilişkili blob kapsayıcısına yükleyin. [Varlık](https://docs.microsoft.com/rest/api/media/operations/asset) varlığı video, ses, görüntüler, küçük resim koleksiyonları, metin parçaları ve kapalı açıklamalı alt yazı dosyaları (ve bu dosyalar hakkındaki meta veriler) içerebilir. Varlık kapsayıcısının içine dosyalar yüklendiğinde, içeriğiniz sonraki işleme ve akışla için bulutta güvenli bir şekilde depolanır.

Bu makalede REST suing yerel bir dosya karşıya yükleme gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

Bu konu başlığı altında açıklanan adımları tamamlamak için için gerekenler:

- Gözden geçirme [varlık kavramı](assets-concept.md).
- [Azure Media Services REST API çağrıları için Postman yapılandırma](media-rest-apis-with-postman.md).
    
    Konunun son adımı takip edin [Azure AD belirteci Al](media-rest-apis-with-postman.md#get-azure-ad-token). 

## <a name="create-an-asset"></a>Bir varlık oluşturun

Bu bölümde, yeni bir varlık oluşturma işlemi gösterilmektedir.

1. Seçin **varlıklar** -> **oluşturma veya güncelleştirme bir varlık**.
2. **Gönder**’e basın.

    ![Bir varlık oluşturun](./media/upload-files/postman-create-asset.png)

Gördüğünüz **yanıt** yeni oluşturulan varlık hakkındaki bilgileri ile.

## <a name="get-a-sas-url-with-read-write-permissions"></a>Okuma / yazma izinlerine sahip bir SAS URL'sini alma 

Bu bölümde, oluşturulan bir varlık için oluşturulmuş bir SAS URL'si alma işlemi gösterilmektedir. SAS URL'sini okuma-yazma izinleri ile oluşturulmuş ve dijital dosyalar varlık kapsayıcıya yüklemek için kullanılabilir.

1. Seçin **varlıklar** -> **varlık URL'lerin listesi**.
2. **Gönder**’e basın.

    ![Dosyayı karşıya yükleme](./media/upload-files/postman-create-sas-locator.png)

Gördüğünüz **yanıt** varlığın URL'leri hakkındaki bilgileri ile. İlk URL'sini kopyalayın ve dosyanızı karşıya yüklemek için kullanın.

## <a name="upload-a-file-to-blob-storage-using-the-upload-url"></a>Bir dosya karşıya yükleme URL'yi kullanarak blob depolamaya yükleme

Azure depolama API veya SDK'larını (örneğin, [depolama REST API'si](../../storage/common/storage-rest-api-auth.md), [JAVA SDK'sı](../../storage/blobs/storage-quickstart-blobs-java-v10.md), veya [.NET SDK'sı](../../storage/blobs/storage-quickstart-blobs-dotnet.md).

## <a name="next-steps"></a>Sonraki adımlar

[Öğretici: Uzak dosya tabanlı URL kodlama ve video akışı yapma - REST](stream-files-tutorial-with-rest.md)
