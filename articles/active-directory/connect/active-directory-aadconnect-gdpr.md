---
title: Azure AD Connect ve kullanıcı gizlilik | Microsoft Docs
description: Bu belge, Azure AD Connect ile GDPR uyumluluğunu elde açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2018
ms.author: billmath
ms.openlocfilehash: a1fa7f58040b420bf52d89a57b1234416c2fb939
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="user-privacy-and-azure-ad-connect"></a>Kullanıcı gizliliği ve Azure AD Connect 

[!INCLUDE [Privacy](../../../includes/gdpr-intro-sentence.md)]

>[!NOTE] 
>Bu makalede, Azure AD Connect ve kullanıcı gizlilik ile ilgilidir.  Azure AD Connect Health ve kullanıcı gizlilik hakkında bilgi için bkz: [burada](../../active-directory/connect-health/active-directory-aadconnect-health-gdpr.md).

Azure AD Connect yüklemeleri için kullanıcı gizlilik uyumluluk iki yolla erişilebilir:

1.  İstek, bir kişi için verileri ayıklamak ve yüklemeleri bu kişiden veri kaldırın
2.  Hiçbir veri 48 saat dışında tutulur emin olun.

Uygulama ve koruma çok daha kolay olduğundan Azure AD Connect takım ikinci seçeneği önerir.

Bir Azure AD Connect eşitleme sunucusu aşağıdaki kullanıcı gizlilik verilerini depolar:
1.  Bir kişi hakkındaki verileri **Azure AD Connect veritabanı**
2.  Verileri **Windows olay günlüğü** bir kişi bilgilerini içerebilir dosyaları
3.  Verileri **Azure AD Connect yükleme günlüğü dosyaları** hakkında bir kişi içerebilir

Azure AD Connect müşteriler, kullanıcı verilerini kaldırırken aşağıdaki yönergeleri kullanmanız gerekir:
1.  Azure AD Connect yükleme günlük dosyalarını düzenli olarak – en az 48 saatte içeren klasörün içeriğini silin
2.  Bu ürün, olay günlükleri de oluşturabilir.  Olay günlükleri günlükleri hakkında daha fazla bilgi için lütfen bkz [şuradaki belgelere](https://msdn.microsoft.com/library/windows/desktop/aa385780.aspx).

Bu kişinin veri onu kaynaklandığı kaynak sistemden kaldırıldığında bir kişi hakkındaki verileri otomatik olarak Azure AD Connect veritabanından kaldırılır. Yöneticiler belirli hiçbir eylemden GDPR uyumlu olması için gereklidir.  Ancak, Azure AD Connect veri en az iki günde veri kaynağınız ile eşitlenen gerektirir.

## <a name="delete-the-azure-ad-connect-installation-log-file-folder-contents"></a>Azure AD Connect yükleme günlük dosyası klasör içeriğini sil
Düzenli olarak kontrol edin ve içeriğini silin **c:\programdata\aadconnect** klasör – dışında **PersistedState.Xml** dosya. Bu dosya bir Azure Connect, önceki yükleme durumunu korur ve yükseltme yüklemesi gerçekleştirildiğinde kullanılır. Bu dosya, bir kişi hakkında herhangi bir veri içermiyor ve silinmesi gerekir.

>[!IMPORTANT]
>PersistedState.xml dosyasını silmeyin.  Bu dosya hiçbir kullanıcı bilgilerini içeren ve önceki yükleme durumunu korur.

Gözden geçirin ve Windows Gezgini'ni kullanarak bu dosyaları silin veya gerekli eylemleri gerçekleştirmek için aşağıdaki gibi bir komut dosyası kullanabilirsiniz:


```
$Files = ((Get-childitem -Path "$env:programdata\aadconnect" -Recurse).VersionInfo).FileName
Foreach ($file in $files) {
If ($File.ToUpper() -ne "$env:programdata\aadconnect\PERSISTEDSTATE.XML".toupper()) # Do not delete this file
    {Remove-Item -Path $File -Force}
    } 
```

### <a name="schedule-this-script-to-run-every-48-hours"></a>Bu komut dosyasının çalışmasını her 48 saat zamanlama
48 saatte bir çalıştırılacak komut dosyasını zamanlamak için aşağıdaki adımları kullanın.

1.  Komut dosyası uzantılı bir dosyaya Kaydet  **&#46;PS1**, ardından Denetim Masası'nı açın ve tıklayın **sistemleri ve güvenlik**.
    ![Sistem](media\active-directory-aadconnect-gdpr\gdpr2.png)

2.  Yönetimsel Araçlar başlığına tıklayın **zamanlama görevleri**.
    ![Görev](media\active-directory-aadconnect-gdpr\gdpr3.png)
3.  Görev Zamanlayıcısı'nda sağ tıklayın **görev zamanlama Kitaplığı** ve tıklayın **oluşturma temel görev...**
4.  Yeni görev için bir ad girin ve tıklayın **sonraki**.
5.  Seçin **günlük** 'ı tıklatın ve görev tetiklemesi için **sonraki**.
6.  Yineleme kümesine **2 gün** tıklatıp **sonraki**.
7.  Seçin **bir programı başlatamayan** 'ı tıklatın ve eylemi olarak **sonraki**.
8.  Tür **PowerShell** etiketli kutusundan ve Program/komut dosyası kutusunda **bağımsız değişkenler (isteğe bağlı) eklemek**, daha önce oluşturduğunuz komut dosyasının tam yolunu girin ve ardından **sonraki**.
9.  Sonraki ekranda oluşturmak üzere olduğunuz görev bir özetini gösterir. Değerleri doğrulayın ve tıklayın **son** görevi oluşturmak için.



## <a name="next-steps"></a>Sonraki adımlar
* [Güven Merkezi Microsoft Privacy ilkeyi gözden geçirin](https://www.microsoft.com/trustcenter)
- [Azure AD Connect sistem durumu ve kullanıcı gizliliği](../../active-directory/connect-health/active-directory-aadconnect-health-gdpr.md)
