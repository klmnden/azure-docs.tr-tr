1. Yükleyici, korumak istediğiniz sunucuda yerel bir klasöre (örneğin, C:\Temp) kopyalayın. Bir yönetici komut isteminde aşağıdaki komutları çalıştırın:

  ```
  cd C:\Temp
  ren Microsoft-ASR_UA*Windows*release.exe MobilityServiceInstaller.exe
  MobilityServiceInstaller.exe /q /x:C:\Temp\Extracted
  cd C:\Temp\Extracted.
  ```
2. Mobilite hizmetinin yüklenmesi için aşağıdaki komutu çalıştırın:

  ```
  UnifiedAgent.exe /Role "MS" /InstallLocation "C:\Program Files (x86)\Microsoft Azure Site Recovery" /Platform "VmWare" /Silent
  ```
3. Şimdi aracı yapılandırma sunucusuna kayıtlı olması gerekir.

  ```
  cd C:\Program Files (x86)\Microsoft Azure Site Recovery\agent
  UnifiedAgentConfigurator.exe  /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
  ```

#### <a name="mobility-service-installer-command-line-arguments"></a>Mobility Hizmeti Yükleyici komut satırı bağımsız değişkenleri

```
Usage :
UnifiedAgent.exe /Role <MS|MT> /InstallLocation <Install Location> /Platform “VmWare” /Silent
```

| Parametre|Tür|Açıklama|Olası değerler|
|-|-|-|-|
|/ Rol|Zorunlu|Mobility hizmetinin (MS) yüklü olmalıdır veya MasterTarget (MT) yüklenmesi gerektiğini belirtir.|MS </br> MT|
|/InstallLocation|İsteğe bağlı|Mobility Hizmeti'nin yüklendiği konumu.|Bilgisayardaki herhangi bir klasör|
|/ Platform|Zorunlu|Mobility hizmetinin yüklü olduğu platform belirtir. </br> </br>- **VMware**: Mobility hizmeti üzerinde çalışan bir VM'de yüklerseniz, bu değeri kullanın *VMware vSphere ESXi konakları*, *Hyper-V konakları*, ve *fiziksel sunucuları*. </br> - **Azure**: bir Azure Iaas sanal aracıyı yüklerseniz, bu değeri kullanın. | VMware </br> Azure|
|/ Sessiz|İsteğe bağlı|Yükleyici sessiz modda çalıştırmak için belirtir.| Yok|

>[!TIP]
> Kurulum günlükleri ProgramData%\ASRSetupLogs\ASRUnifiedAgentInstaller.log % altında bulunabilir.

#### <a name="mobility-service-registration-command-line-arguments"></a>Mobility hizmeti kayıt komut satırı bağımsız değişkenleri

```
Usage :
UnifiedAgentConfigurator.exe  /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
```

  | Parametre|Tür|Açıklama|Olası değerler|
  |-|-|-|-|
  |/CSEndPoint |Zorunlu|Yapılandırma sunucusu IP adresi| Geçerli bir IP adresi|
  |/PassphraseFilePath|Zorunlu|Parola deyimi konumu |Herhangi bir geçerli UNC veya yerel dosya yolu|


>[!TIP]
> Aracı yapılandırması günlükleri ProgramData%\ASRSetupLogs\ASRUnifiedAgentConfigurator.log % altında bulunabilir.
