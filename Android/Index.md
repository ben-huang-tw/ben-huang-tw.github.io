# Android 開發

## 如何下載安裝 ADT Plugin for Eclipse ？

ADT Plugin for Eclipse的主要目的是做為Andorid SDK整合至Eclipse當中的一個重要外掛套件。

1. 開啟Eclipse IDE
2. 點選"Help"下拉式選單當中的"Install New Software"選項
3. 當出現"Install"對話視窗時，請在"Work with"文字框中，鍵入 `https://dl-ssl.google.com/android/eclipse/` 網址，並按下"Add..."按鈕。
4. 當出現"Add Repository"對話視窗時，請您在"Name:"對話視窗中，輸入一個英文代號名稱。因為，這個名稱只是說明這個網址為何？所以，您可以自由輸入一個英文代號名稱(例如，`ADT Plugin`)。
5. 您會看到"ADT Plugin for Eclipse相關工具列表"，請您按下"Select All" 按鈕，以便選取所有ADT Plugin for Eclipse的相關工具。
6. 接著，請您按下"Next >"按鈕。
7. 然後會出現"Install Details"視窗，列出要安裝的工具列表。接著，請再按下"Next >"按鈕。
8. 當"Install"對話視窗中，出現"Review Licenses"內容時，請您點選"I accept the terms of the license agreements"選鈕。然後，按下"Finish"按鈕。
9. ADT Plugin for Eclipse相關工具下載安裝中...
10. 安裝過程中可能出現安全性警告，按下OK即完成安裝ADT外掛套件。
11. 當下載安裝完畢後，需要按下"Restart Now"按鈕且重新啟動 Eclipse，即可完成ADT Plugin for Eclipse的安裝程序。
12. 重新啟動後，會出現"Welcome to Android Development"視窗，並開啟"Android SDK"訊息視窗，顯示"Location of the Android SDK has not been setup in the preferences."，你可以按下"Open Preferences"按鈕，以開啟"Preferences"視窗，或按下"Close"按鈕，關閉該訊息視窗。
13. "Welcome to Android Development"視窗可以協助您下載"Android SDK"。

## 下載 Android SDK 基本工具

1. 到[Download Android Studio and SDK Tools](https://developer.android.com/studio/index.html)頁面。
2. 點選"Download Options"
3. 下載基本的命令列工具，例如，`android-sdk_r24.4.1-windows.zip`。
4. 下載之後，將其解壓縮，此路徑就是你的 Android SDK 路徑。

> For information about how to keep your SDK tools up to date, refer to the [SDK Manager](https://developer.android.com/studio/intro/update.html) guide.

> TIP: Android 2.1 support 97% phones and tablets.

解壓縮後的大小約為 315 MB

## 在 Eclipse 設定 Android SDK 路徑

1. 點選"Window"下拉式選單當中的"Preferences"選項
2. 當出現"Preferences"視窗時，點選左邊的"Android"，然後在右邊的"SDK Location"欄位輸入您的Android SDK 路徑(此路徑會有`SDK Manager.exe`執行檔)
3. 點選"Apply"按鈕即可。

## 使用 SDK Manager 下載 Android SDK 平台工具

事實上，Android SDK 基本工具只包含"Android SDK Tools"，要開發 Android APP，需要再下載額外的 Android SDK 平台工具。

至少需要以下的工具：

* Android SDK Tools(基本工具中已包含，但並非最新版，最好透過SDK Manager進行更新)
* Android SDK Build Tools
* Android SDK Platform-tools
* 至少一個平台的工具：
    * Android SDK Platform
    * Intel or ARM System Images

此外，建議也安裝以下工具：

* Android Support Repository
* Google Repository

1. 進入 Eclipse。
2. 點選"Window"下拉式選單當中的"Android SDK Manager"選項
3. 當出現"SDK Manager"視窗後，預設會勾選`Android SDK Tools`、`Android SDK Build-Tools`、`Android SDK Platform-tools`、`Google USB Driver` 以及最新平台的所有檔案(例如，Android 7.0)。
4. 請先取消勾選最新平台的所有檔案。
5. 勾選左下角的 `Obsolete` 核取方塊。
6. 勾選 `Android 4.4.2 (API 19)` 以下檔案：
    * SDK Platform
    * ARM EABI v7a System Image
7. 勾選 `Extra` 的以下檔案：
    * Android Support Resposity
    * Google Repository
    * Android Support Library(Obsolete)
8. 點選 "Install 9 packages" 按鈕
9. 當出現時"Choose Packages to Install"視窗時，請點選"Accept License"(左邊的每個類別，都要個別按一次"Accept License")，然後按下"Install"按鈕。
10. 此時會出現"Android SDK Manager Log"視窗，並顯示下載的Log，下方也會有進度條。狀態列則會顯示目前正在進行的下載作業，當顯示"Done loading packages"，通常表示已下載完成。


註：下載完成後，應該會新您目前正在使用的 Android SDK 和 AVD Manager。建議您現在關閉 SDK Manager視窗，然後重新打開它。如果您使用 Eclipse，請執行 `Help > Check for Updates`，看看是否 Android plug-in 需要更新。

## 下載及安裝 Intel x86 Emulator Accelerator

Intel x86 Emulator Accelerator(簡稱 HAX), 為模擬器的硬體加速器, 安裝後可加快模擬器的執行效能, 不過電腦必須使用 Intel 的 CPU 且有支援 HAX (目前幾乎都支援), 而模擬器則必須裝載 x86 的 System Image 才行。如果是使用 Intel 的 CPU, 建議要安裝以加快模擬器的效能。

下載：可在 "SDK Manager" 中的 "Extra" 類別勾選 "Intel x86 Emulator Accelerator(HAXM Installer)"進行下載。

安裝：安裝檔在 `<SDK Path>\extras\intel\Hardware_Accelerated_Execution_Manager\intelhaxm-android.exe`，請執行安裝檔進行安裝。

## FAQ

### Unable to execute dex: Multiple dex files define Lcom/google/gson/JsonSerializer

若在執行時，遇到下列錯誤訊息時：

```
[2016-09-25 16:12:47 - Dex Loader] Unable to execute dex: Multiple dex files define Lcom/google/gson/JsonSerializer;
[2016-09-25 16:12:47 - UIMClientSample] Conversion to Dalvik format failed: Unable to execute dex: Multiple dex files define Lcom/google/gson/JsonSerializer;
```

解決方式：

1. 在專案節點上按滑鼠右鍵 > "Properties"
2. 當出現專案的屬性視窗時，點選左方的 "Java Build Path"，然後點選"Order and Export"頁籤，將 "Android private Libraries" 取消勾選。

參考來源: [stackoverflow](http://stackoverflow.com/questions/7870265/unable-to-execute-dex-multiple-dex-files-define-lcom-myapp-rarray)


















