---
title: Azure AD Connect ve kullanıcı gizlilik | Microsoft Docs
description: Bu belge, Azure AD Connect ile GDPR uyumluluk elde açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 05/21/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6f5d3125b7b77e8ce7a943f640c44615049ab160
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60455797"
---
# <a name="user-privacy-and-azure-ad-connect"></a>Kullanıcı gizliliği ve Azure AD Connect 

[!INCLUDE [Privacy](../../../includes/gdpr-intro-sentence.md)]

>[!NOTE] 
>Bu makalede, Azure AD Connect ve kullanıcı gizlilik ile ilgilidir.  Azure AD Connect Health ve kullanıcı gizliliği hakkında bilgi için bkz [burada](reference-connect-health-user-privacy.md).

Azure AD Connect yükleme iki yolla için kullanıcı gizliliğini geliştirme:

1.  İstek, bir kişi için verileri ayıklayın ve bu kişiden yüklemeleri veri kaldırma
2.  Hiçbir veri 48 saat dışında tutulur emin olun.

Azure AD Connect takım, uygulamak ve sürdürmek daha kolay olduğundan ikinci seçenek önerir.

Bir Azure AD Connect eşitleme sunucusu aşağıdaki kullanıcı gizlilik verilerini depolar:
1.  Bir kişi hakkında veri **Azure AD Connect'in veritabanı**
2.  Verileri **Windows olay günlüğü** kişi hakkında bilgi içeren dosyaları
3.  Verileri **Azure AD Connect yükleme günlük dosyalarını** hakkında bir kişi içerebilir

Azure AD Connect müşterileri, kullanıcı verilerini kaldırırken aşağıdaki yönergeleri kullanmanız gerekir:
1.  Azure AD Connect yükleme günlük dosyalarını düzenli olarak – en az 48 saatte içeren klasörün içeriğini silin
2.  Bu ürün, olay günlükleri de oluşturabilirsiniz.  Olay günlükleri günlükleri hakkında daha fazla bilgi edinmek için lütfen bkz [burada sağlanan belgelerde](https://msdn.microsoft.com/library/windows/desktop/aa385780.aspx).

Bu kişinin veri öğesinden geldiği kaynak sistemden kaldırıldığında kişi hakkında verileri otomatik olarak Azure AD Connect veritabanından kaldırılır. Yöneticilerin belirli bir eylem yok GDPR ile uyumlu olması gerekir.  Ancak, Azure AD Connect veri en az iki günde, veri kaynağı ile eşitlenmiş gerektirir.

## <a name="delete-the-azure-ad-connect-installation-log-file-folder-contents"></a>Azure AD Connect yükleme günlük dosyası klasörü içeriğini sil
Düzenli olarak denetleyin ve içeriğini silin **c:\programdata\aadconnect** klasör – dışında **PersistedState.Xml** dosya. Bu dosya, bir Azure Connect, önceki yükleme durumunu korur ve yükseltme bir yükleme gerçekleştirildiğinde kullanılır. Bu dosya, kişi hakkında hiç veri içermediğini ve silinmesi gerekir.

>[!IMPORTANT]
>PersistedState.xml dosyayı silmeyin.  Bu dosya, hiçbir kullanıcı bilgisi içermez ve önceki yükleme durumunu korur.

Gözden geçirin ve Windows Gezgini'ni kullanarak bu dosyaları silin veya gerekli eylemleri gerçekleştirmek için aşağıdaki gibi bir betik kullanın:


```
$Files = ((Get-childitem -Path "$env:programdata\aadconnect" -Recurse).VersionInfo).FileName
Foreach ($file in $files) {
If ($File.ToUpper() -ne "$env:programdata\aadconnect\PERSISTEDSTATE.XML".toupper()) # Do not delete this file
    {Remove-Item -Path $File -Force}
    } 
```

### <a name="schedule-this-script-to-run-every-48-hours"></a>Bu betik, 48 saatte çalıştırılacak zamanlama
48 saatte çalıştırılacak betik zamanlamak için aşağıdaki adımları kullanın.

1.  Betik uzantılı bir dosyaya Kaydet  **&#46;PS1**, ardından Denetim Masası'nı açın ve tıklayarak **sistemleri ve güvenlik**.
    ![Sistem](./media/reference-connect-user-privacy/gdpr2.png)

2.  Yönetimsel Araçlar başlığına tıklayarak **görevleri zamanlayın**.
    ![Görev](./media/reference-connect-user-privacy/gdpr3.png)
3.  Görev Zamanlayıcı'da, sağ tıklayın **görev zamanlama Kitaplığı** tıklayın **oluşturma temel görevi...**
4.  Yeni görev için bir ad girin ve tıklayın **sonraki**.
5.  Seçin **günlük** tıklatın ve görev tetikleyici için **sonraki**.
6.  Yinelenme kümesine **2 gün** tıklatıp **sonraki**.
7.  Seçin **programı Başlat** tıklatın ve eylemi olarak **sonraki**.
8.  Türü **PowerShell** etiketli kutusundan ve Program/komut kutusundaki **bağımsız değişkenler (isteğe bağlı) ekleme**, daha önce oluşturduğunuz bir komut dosyası için tam yolu girin ve ardından tıklayın **sonraki**.
9.  Sonraki ekranda, görev oluşturmak üzere olduğunuz bir özetini gösterir. Değerleri doğrulayın ve tıklayın **son** görevi oluşturur.



## <a name="next-steps"></a>Sonraki adımlar
* [Güven Merkezi Microsoft Privacy ilkeyi gözden geçirin](https://www.microsoft.com/trustcenter)
* [Azure AD Connect sistem durumu ve kullanıcı gizliliği](reference-connect-health-user-privacy.md)
