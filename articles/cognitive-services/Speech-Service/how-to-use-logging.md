---
title: Konuşma SDK günlüğü - konuşma Hizmetleri
titleSuffix: Azure Cognitive Services
description: Konuşma SDK'da günlüğü etkinleştirme.
services: cognitive-services
author: amitkumarshukla
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/03/2019
ms.author: amishu
ms.openlocfilehash: 75eaea22c4809eda78e54514961d13113b4a5f3a
ms.sourcegitcommit: e43ea344c52b3a99235660960c1e747b9d6c990e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2019
ms.locfileid: "59012455"
---
# <a name="enable-logging-in-the-speech-sdk"></a>Konuşma SDK'da günlüğü etkinleştirme

Dosyasına günlük kaydetmeyi Speech SDK'sı için isteğe bağlı bir özelliktir. Geliştirme sırasında ek bilgi ve Speeck SDK'ın temel bileşenlerinden tanılama günlüğü sağlar. Özelliğini ayarlayarak etkinleştirilebilir `Speech_LogFilename` günlük dosyasının adını ve konumunu bir konuşma yapılandırma nesnesine üzerinde. Günlüğe kaydetme, yapılandırmasından bir tanıyıcı oluşturulduktan sonra genel olarak etkinleştirilir ve daha sonra devre dışı bırakılamaz. Günlüğe kaydetme oturumu sırasında çalışan bir günlük dosyasının adını değiştiremezsiniz.

> [!NOTE]
> Günlüğe kaydetme programlama dilleri, JavaScript hariç tüm desteklenen konuşma SDK kullanıma sunulmuştur.

## <a name="sample"></a>Örnek

Günlük dosyası adı, bir yapılandırma nesnesi üzerinde belirtilir. Alma `SpeechConfig` örneği ve örneğini oluşturduğunuz varsayılarak adlı `config`:

```csharp
config.SetProperty(PropertyId.Speech_LogFilename, "LogfilePathAndName");
```

```java
config.setProperty(PropertyId.Speech_LogFilename, "LogfilePathAndName");
```

```C++
config->SetProperty(PropertyId::Speech_LogFilename, "LogfilePathAndName");
```

```Python
config.set_property(speechsdk.PropertyId.Speech_LogFilename, "LogfilePathAndName")
```

```objc
[config setPropertyTo:@"LogfilePathAndName" byId:SPXSpeechLogFilename];
```

Config nesneden bir tanıyıcı oluşturabilirsiniz. Bu, tüm tanıyıcılar için günlük kaydını etkinleştirir.

> [!NOTE]
> Oluşturursanız, bir `SpeechSynthesizer` yapılandırma nesnesinden, günlüğe kaydetme etkinleştirmez. Günlük ancak etkinleştirildiğinde, Tanılama'ya da alırsınız `SpeechSynthesizer`.

## <a name="create-a-log-file-on-different-platforms"></a>Farklı platformlarda günlük dosyası oluşturur

Windows veya Linux günlük dosyası kullanıcının için yazma iznine sahip herhangi bir yolu olabilir. Yazma izinleri dosya sistemi konumları diğer işletim sistemleri için sınırlı veya kısıtlanmış varsayılan olarak.

### <a name="universal-windows-platform-uwp"></a>Evrensel Windows Platformu (UWP)

UWP uygulamaları Yerler günlük dosyaları (yerel, dolaşım veya geçici) uygulama veri konumlardan birinde olmanız gerekir. Bir günlük dosyası yerel uygulama klasörü oluşturulabilir:

```csharp
StorageFolder storageFolder = ApplicationData.Current.LocalFolder;
StorageFile logFile = await storageFolder.CreateFileAsync("logfile.txt", CreationCollisionOption.ReplaceExisting);
config.SetProperty(PropertyId.Speech_LogFilename, logFile.Path);
```

Daha fazla dosya erişimi hakkında izni UWP uygulamaları için kullanılabilir [burada](https://docs.microsoft.com/windows/uwp/files/file-access-permissions).

### <a name="android"></a>Android

İç Depolama alanı, dış depolama veya önbellek dizini için bir günlük dosyasına kaydedebilirsiniz. İç depolamadaki dosyaları oluşturan veya uygulamaya özel önbellek dizini. Dış depolama alanında bir günlük dosyası oluşturmak için daha iyidir.

```java
File dir = context.getExternalFilesDir(null);
File logFile = new File(dir, "logfile.txt");
config.setProperty(PropertyId.Speech_LogFilename, logFile.getAbsolutePath());
```

Yukarıdaki kod, bir günlük dosyası bir uygulamaya özgü dizin kökündeki dış depolama birimine kaydedecek. Bir kullanıcı dosyayı dosya Yöneticisi ile erişebilirsiniz (genellikle `Android/data/ApplicationName/logfile.txt`). Dosya, uygulama kaldırıldığında silinir.

Ayrıca istek gerekir `WRITE_EXTERNAL_STORAGE` izin bildirim dosyası:

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android" package="...">
  ...
  <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
  ...
</manifest>
```

Daha fazla veri ve dosya depolama Android uygulamaları için kullanılabilir [burada](https://developer.android.com/guide/topics/data/data-storage.html).

#### <a name="ios"></a>iOS

Yalnızca dizinleri uygulama korumalı alanı içinde erişilebilir. Dosyaları, belgeler, kitaplık ve geçici dizin oluşturulabilir. Belgelerim dizini dosyalarında bir kullanıcı için kullanılabilir. Aşağıdaki kod parçacığı uygulama belge dizininde bir günlük dosyası oluşturma gösterilmektedir:

```objc
NSString *filePath = [
  [NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES) firstObject]
    stringByAppendingPathComponent:@"logfile.txt"];
[speechConfig setPropertyTo:filePath byId:SPXSpeechLogFilename];
```

Oluşturulan bir dosyaya erişmek için ekleme özellikleri aşağıda `Info.plist` uygulama özellik listesi:

```xml
<key>UIFileSharingEnabled</key>
<true/>
<key>LSSupportsOpeningDocumentsInPlace</key>
<true/>
```

Daha fazla iOS dosya sistemi kullanılabilir [burada](https://developer.apple.com/library/archive/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Github'da örneklerimizi keşfedin](https://aka.ms/csspeech/samples)

