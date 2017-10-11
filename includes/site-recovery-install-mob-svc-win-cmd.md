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
  UnifiedAgentConfigurator.exe”  /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
  ```

#### <a name="mobility-service-installer-command-line-arguments"></a>Mobility Hizmeti Yükleyici komut satırı bağımsız değişkenleri

```
Usage :
UnifiedAgent.exe /Role <MS|MT> /InstallLocation <Install Location> /Platform “VmWare” /Silent
```

| Parametre|Tür|Açıklama|Olası değerler|
|-|-|-|-|
|/ Rol|Zorunlu|Mobility hizmetinin (MS) yüklü olmalıdır veya MasterTarget(MT) yüklenmesi gerektiğini belirtir|MS </br> MT|
|/InstallLocation|İsteğe bağlı|Mobility Hizmeti'nin yüklendiği konumu|Bilgisayardaki herhangi bir klasör|
|/ Platform|Zorunlu|Mobility hizmetinin yüklendiği platformu belirtir </br> </br>- **VMware** : mobility hizmeti üzerinde çalışan bir VM yüklüyorsanız bu değeri kullanın *VMware vSphere ESXi konakları*, *Hyper-V konakları* ve *Phsyical sunucuları* </br> - **Azure** : bir Azure Iaas VM aracısı yüklüyorsanız, bu değeri kullanın| VMware </br> Azure|
|/ Sessiz|İsteğe bağlı|Yükleyici sessiz modda çalıştırmak için belirtir| NA|

>[!TIP]
> Kurulum günlüklerini %ProgramData%\ASRSetupLogs\ASRUnifiedAgentInstaller.log altında bulunabilir.

#### <a name="mobility-service-registration-command-line-arguments"></a>Mobility hizmeti kayıt komut satırı bağımsız değişkenleri

```
Usage :
UnifiedAgentConfigurator.exe”  /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
```

  | Parametre|Tür|Açıklama|Olası değerler|
  |-|-|-|-|
  |/ CSEndPoint |Zorunlu|Yapılandırma sunucusu IP adresi| Geçerli bir IP adresi|
  |/PassphraseFilePath|Zorunlu|Parola konumu |Herhangi bir geçerli UNC veya yerel dosya yolu|


>[!TIP]
> AgentConfiguration günlükleri %ProgramData%\ASRSetupLogs\ASRUnifiedAgentConfigurator.log altında bulunabilir.
