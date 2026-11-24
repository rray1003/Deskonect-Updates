# Deskonect 版本更新日誌

此檔自 **0.2.0-alpha.82** 起維護每次實際成功發布的內容。依 SemVer 分系列，**同一 minor 系列最新 Build 永遠置頂**；新的 minor／major 另開系列，舊系列不得刪除，移到下方或封存到 `docs/changelog/`（私人來源庫）並保留連結。每個版本固定按「新增／優化／除錯」分類，未做的項目明列「無」。只有兩端安裝檔皆完成發布並核對，才能列為「已發布」；未驗收的功能不得寫成已通過。

## 0.3.x 系列

### 0.3.3-alpha.98 — 2026-11-24（已發布）

**狀態：** Windows／Android 已由本機正式流程共同建置並公開發布；實際建置來源 `9f593ee898570a7856cacfff678f6e5b9cd9dd2a`，發布器核對歷史 Tag／Release 後分配 Build 98。

**新增**
- Android 五款獨立配色（天空、愛心、奶油、薄荷、貴族），各有淺色與深色配色，並加入外觀設定與本機偏好保存；Windows 同步提供五色深淺介面。
- Android 將觀看頁移至獨立原生 `CameraViewerActivity`／FlutterEngine，使用 Android 系統 PiP 疊在仍可操作的裝置清單上，而非主視窗內嵌視窗。
- Windows 提供經本機授權的一鍵攝影機原始／處理後影格與曝光、FPS 診斷、結果預覽，資料留在本機。
- Android 已授權遠端控制加入連續拖曳封包、手勢延續與較低延遲的預覽更新，保留舊協定回退。

**優化**
- Windows 裝置清單區分電腦與手機裝置，精簡文字與控制介面，維持觀看頁為主視窗之外獨立視窗。
- Android WebRTC 預設音訊與 Windows 音效裝置處理改進一般媒體音量與 HFP 裝置排除，既有對講／收音權限與連線機制保留。
- Android 原生 PiP 關閉、旋轉與觀看會話清理採有界等待，避免把裝置清單 Activity 結束；五色主題新增完整獨立亮暗表面色。

**除錯**
- 修正 Android Flutter 3.35+ `ThemeData.appBarTheme` 參數型別，將 `AppBarTheme` 改成 `AppBarThemeData` 並新增來源回歸檢查；本機主題儲存不可用時不再讓使用者操作產生未處理的非同步錯誤。
- 修正 Android 原生觀看端部分 WebRTC 音訊路由與關閉時生命週期問題；保留授權要求與背景相機服務的獨立性。
- 修正 Windows 診斷模組的 logger、Android 連續拖曳封包解析、Flutter 主題棄用 API，以及 Android 8 以下的連續拖曳 API 呼叫邊界；更新 Windows 八頁導覽測試，保留原有 swipe 回退與授權流程。

**驗證與限制**
- `BUILD_ALL_LOCAL.bat --worker` 正式流程通過 Python 語法、Ruff、pip check、444 項 pytest（另 2 subtests，1 項依賴套件棄用警告）、Flutter analyze、72 項 Flutter 測試（另 2 項 live 測試未執行）、Kotlin／Manifest／lintRelease、Windows 四 EXE Smoke Test、線上及離線 Setup、Payload ZIP、原 v2 簽章 APK 與草稿資產 SHA-256 核對；失敗 0 項。完整日誌為 `local-release-20261124-195608.log`，本機清單為 `dist/build-98-verified.json`。
- APK 保留 Package ID `app.deskonect.deskonect_android.v2`、原憑證 SHA-256 `4dbfc50a4219aea08410c6cd0ac9b72b5257782e7b0147fe2687ebec528917e4`、versionCode 98、versionName `0.3.3-alpha.98`；主要與相容 APK 位元相同。共同 `v0.3.3-preview.98` 與舊版 `v0.2.0-android.98` 均已公開，主要版為 GitHub latest。
- 草稿上傳資產與本機清單逐一核對 SHA-256：Windows 線上 Setup `e2a5d3a869f39c195a2cf06a55a93f9756118fb216c10c502881a4419d907bba`；離線更新 Setup `4b1b7aacaaa484a3b7c8924d97acc13a93e452eba86e85e3e472747c01546ae5`；Payload ZIP `67459f1caae76f78180544952cb41a7b1632e1804a7336a0a84e42e128a53f38`；主要與相容 APK `11f44f08fc59784c86e70719bbc5a1e0b98f70d49939b0d5bdabbc551c2ad49a`。
- Android 系統 PiP、音訊媒體路由、持續拖曳、MSI Webcam 診斷及跨網品質仍需真實 Redmi／POCO／Windows 裝置驗證，不能以 mock 或編譯結果宣稱完整通過。
- 實體手機、另一台 Windows 的安裝／App 內升級、Google 真實登入、跨網媒體／控制、PiP、耗電與長時間運作尚未實機驗收；Windows 安裝檔尚無 Authenticode。不啟用任何有額外費用的服務，保留舊版更新相容入口與歷史紀錄。
- [Windows／Android Build 98](https://github.com/rray1003/Deskonect-Updates/releases/tag/v0.3.3-preview.98) · [舊 Android v2 相容入口](https://github.com/rray1003/Deskonect-Updates/releases/tag/v0.2.0-android.98)



### 0.3.2-alpha.97 — 2026-11-23（已發布）

**狀態：** Windows／Android 已共同建置並公開發布；來源提交 `959e33634523b00027059dc248939a2352ec3eb9`，發布器依全部歷史 Tag／Release 分配 Build 97。

**新增**
- Windows 攝影機新增可選的完全離線 OpenCV 微光全彩實驗引擎（balanced／bright）；預設仍為原版夜視處理。
- 新增本機可選的 IAT ONNX 推論介面，模型與 Runtime 不內建、也不自動下載；實際模型推論仍待獨立驗證。
- Windows 攝影機新增使用者本機明確啟用、每次連線最多兩組的原始／處理後無損 PNG A/B 診斷，永不自動上傳。

**優化**
- Windows Tk 觀看端移除 20 FPS 的輪詢繪圖上限，只繪製最新影格；新增相機讀取／主機有效 FPS／Android 解碼 FPS 的分段觀察。
- Android 攝影機遠端收音及按住對講時，嘗試回復一般媒體音量路由，保留 WebRTC、背景接聽、PiP 與原有權限。
- 微光全彩保留暗部實際紅藍色彩，採取較保守的色度／時域處理與新增回歸案例。

**除錯**
- 修正 Android／HyperOS 的受限制無障礙設定引導，不繞過 Android 系統授權。
- 修整新增 Python 模組匯入排序並同步 Python、Flutter、Windows 安裝器、預設 build-info、測試與文件至 0.3.2。
- 保留實驗演算法／診斷出錯時回退，不影響授權、原觀看與音訊會話。

**驗證與限制**
- `BUILD_ALL_LOCAL.bat --worker` 正式流程通過：Python 語法、Ruff、pip check、405 項 pytest（另 2 subtests，1 項依賴套件棄用警告）、Flutter analyze 與 67 項 Flutter 測試（另 2 項 live 測試未執行）、Kotlin／Manifest／lintRelease、Windows 四 EXE smoke、線上及離線 Setup、Payload ZIP、原 v2 簽章 APK。發布前指定的 3 個相機診斷測試檔另有 22 項通過。
- APK 保留 `app.deskonect.deskonect_android.v2`、原發行憑證 SHA-256 `4dbfc50a4219aea08410c6cd0ac9b72b5257782e7b0147fe2687ebec528917e4`、versionCode 97、versionName `0.3.2-alpha.97`；主要與舊 Android 相容 APK 位元相同。共同 `v0.3.2-preview.97` 與舊版 `v0.2.0-android.97` 入口均已公開，主要版為 GitHub latest。
- 五個公開資產均重新下載並核對 SHA-256，與本機建置清單一致：Windows 線上 Setup `f18f4095020774469f26c7613cb6ba82cc7076bd6db14561b04ada4b14c6006a`；離線更新包 `4d1e445cbf5151ff4296222541249451a9e02068f89211e8ce1c63054a62f91c`；Payload ZIP `d17ceea7a3628a198b6a9a966745ff47c3b266834ce9550814eef92fa2d875da`；主要與相容 APK `02556ac6c3911c7de4728018f409cb2e74ea6e6369539118bedaace6276cce31`。
- OpenCV 原型於 1152×1536 靜態照片先前曾量測約 0.44–0.51 秒／張，這不是即時串流 FPS；IAT 尚未以真實權重測試，Redmi／POCO 真實音訊路由與 MSI 相機畫質均未驗收。
- 實體手機、另一台 Windows 的安裝／App 內升級、Google 真實登入、跨網媒體／控制、PiP、耗電與長時間運作尚未實機驗收。Windows 安裝檔尚無 Authenticode。保持零額外費用、既有 Android v2 簽章／Package ID、舊版更新相容 Tag 和全部歷史版本紀錄。
- [Windows／Android Build 97](https://github.com/rray1003/Deskonect-Updates/releases/tag/v0.3.2-preview.97) · [舊 Android v2 相容入口](https://github.com/rray1003/Deskonect-Updates/releases/tag/v0.2.0-android.97)


### 0.3.1-alpha.96 — 2026-11-22（已發布）

**狀態：** Windows／Android 已共同建置並公開發布；來源提交 ae359879461f4b6da8d9a5036e32f5924d84ec5e，Build 由全部歷史 Release／Tag 分配為 96。

**新增**
- Windows 低光攝影機原生曝光協助：在確認硬體驅動支援時嘗試 MSMF／DirectShow 感光增亮，保留原有數位夜視處理及使用者授權流程。
- 新增唯讀曝光能力診斷工具 `tools/probe_camera_exposure.py`。

**優化**
- 攝影機讀取、曝光參數調整及停止釋放的序列化，避免連線終止競態。
- Alpha／Beta 分段版號規則：0 固定、第二段重大階段、第三段相容功能、Build 除錯修訂；全部採十進位，維持 Windows／Android 更新器相容格式。

**除錯**
- 盡可能在曝光調整無效、過曝或關閉時還原硬體原始曝光設定，防止更改使用者手動設定。
- 同步 Python、Flutter、Windows 安裝器的版本來源、版本驗證與回歸測試。

**驗證與限制**
- `BUILD_ALL_LOCAL.bat --worker` 正式流程通過：pip check、Python 語法、Ruff、373 項 pytest（另 2 subtests）、Flutter analyze 與 64 項 Flutter 測試（另 2 項 live 測試未執行）、Kotlin／Manifest／lintRelease、Windows 四 EXE smoke、兩種 Setup、Payload ZIP、原 v2 簽章 APK 及 SHA-256 核對。
- APK 保留 `app.deskonect.deskonect_android.v2`、原發行憑證、versionCode 96、versionName `0.3.1-alpha.96`；主要與舊 Android 相容 APK 位元相同。共同 `v0.3.1-preview.96` 與舊版 `v0.2.0-android.96` 入口均已公開。
- 實體手機、MSI Webcam、另一台 Windows 的安裝／App 內升級、Google 真實登入、跨網媒體／控制、PiP、耗電與長時間運作尚未實機驗收。Windows 安裝檔尚無 Authenticode。
- [Windows／Android Build 96](https://github.com/rray1003/Deskonect-Updates/releases/tag/v0.3.1-preview.96) · [舊 Android v2 相容入口](https://github.com/rray1003/Deskonect-Updates/releases/tag/v0.2.0-android.96)


### 0.3.0-alpha.95 — 2026-11-22（已發布）

**狀態：完整本機雙平台重新建置並正式公開；最新共同 Release 與舊 Android 相容入口均非草稿。來源 eeb6a4bfa2857ce4456748aa8e2974fc61254334，Build 由歷史 Release／tag 自動分配為 95。**

**新增**
- Android 手機畫面的逐次授權遠端操控：Windows／Android 觀看端可請求點擊、滑動與系統導航；來源手機必須另行啟用系統無障礙服務並在每次連線本機批准，授權最長 10 分鐘、一次一名操作者。
- Android 額外 opt-in 的低幀率自動螢幕預覽，沿用簽章邀請與 P2P DataChannel；觀看時必須有可見通知，無觀看者時不進行截圖循環，保留原本 MediaProjection 手動分享。

**優化**
- 微光全彩使用既有 OpenCV NLM、陰影亮度／亮部保護與色度穩定；維持原始 BGR uint8 尺寸與五種模式，加入分塊像素一致性及光線／彩色雜訊回歸。
- Android PiP 與雙畫面的影像尺寸／橫直向接線與原生比例相容處理。

**除錯**
- 修正最新提交的匯入排序與條件式 Ruff 問題；移除協定測試的動態 exec，直接測試實際驗證函式，新增未授權、請求、取消及攝影機會話的發送邊界測試，未放寬任何授權條件或測試斷言。
- 移除兩處 Flutter 重複匯入，修正新增自動螢幕預覽後未同步的攝影機釋放回歸檢查；同時檢查 MediaProjection 與預覽觀看者均不持有實體鏡頭。
- PiP 共用版本判斷及參數建構函式補齊可供 Android Lint 驗證的 API 契約，保留 API 24／25 相容與所有執行時保護，不關閉 NewApi 檢查、不提高最低 Android 版本。

**驗證與限制**
- 實際執行 `BUILD_ALL_LOCAL.bat --worker`：全 Python 語法／Ruff、pip check、Flutter pub get/analyze、351 Python 測試（另 2 subtests）、64 Flutter 測試、Kotlin／Manifest／完整 lintRelease、四 EXE Smoke Test、線上／離線 Setup、Payload ZIP 與正式 APK 全部通過。既有 2 項需外部設定的 Flutter live 測試未驗證，未新增跳過條件；Lint 為 0 errors、19 warnings。
- APK 為原 v2 package／憑證、versionCode 95、versionName 0.3.0-alpha.95，保留三種 ABI；模擬器 94→95 原地覆蓋成功，appId 10217 與首次安裝時間保留，啟動並顯示首頁。公開下載與本機 SHA-256 核對，主要與相容 APK 位元相同。完整證據見私人來源 `docs/ALPHA95_RELEASE_REPORT.md`。
- 自動預覽是低幀率 JPEG，不是 30/60 FPS 影像；受保護畫面、ROM 背景限制與 Accessibility API 政策仍適用，不隱藏或繞過 Android 系統保護。
- Redmi／POCO、MSI Webcam、Google 真實登入、跨網媒體／控制、PiP、耗電、長時間存活與真實系統更新流程仍需實機驗證；Windows 尚無 Authenticode，另一台電腦實際安裝／App 內更新尚未驗證。未新增付費雲端服務、未改 v2 package／簽章或全域 Build 規則。
[Windows／Android Build 95](https://github.com/rray1003/Deskonect-Updates/releases/tag/v0.3.0-preview.95) · [舊 Android v2 相容入口](https://github.com/rray1003/Deskonect-Updates/releases/tag/v0.2.0-android.95)

### 0.3.0-alpha.94 — 2026-11-21（已發布）

**狀態：Windows／Android 已共同公開發布。來源 e7890ec9591a7edfde956ebfdbe8d94fcb1feb88；本機正式發布日誌 local-release-20261121-022928.log 核對 288 Python／56 Flutter 通過，既有 2 項 live 測試未驗證。**

**新增**
- Android 手機螢幕的使用者授權擷取與跨網 WebRTC P2P 影像觀看（不含遠端操作）。
- Android 雙裝置攝影機觀看、App 內可拖曳小視窗與 Android 系統 PiP。
- Windows／Android 遠端手機畫面的數位夜視、微光全彩等觀看選項。

**優化**
- Android 背景服務存活機率、重開機恢復提醒及 HyperOS 設定引導；不承諾抵抗強制停止。
- WebRTC 觀看、音訊、RTDB 待命和安裝封裝資源使用的無退步優化；實測功耗與容量改善尚待驗收。
- 安卓更新器改為支援跨 Minor 共同 Release，保留 0.2.x 的舊版相容入口。

**除錯**
- Windows 遠端收音輸出與控制事件生命週期。
- Windows／Android 攝影機邀請排序、時間校正、過期資料清理及免費 STUN 備援。
- Android PiP／觀看關閉及遠端分享端服務互不干擾的生命週期。
- 修正 PiP 原生 API／計時器、相機前景類型的 API 相容、Manifest 硬體宣告及完整 lint 依賴轉換；建置改為所有快速門檻先通過才封裝，附回歸測試。
- 微光全彩改為先降噪再提升亮度，保留色彩、輪廓與既有模式；新增合成影格回歸，尚未實體 Webcam A/B 驗收。

**驗證與限制**
- 最新來源完整本機驗證：288 Python、56 Flutter 測試通過；2 項既有 live 測試未驗證。全 Ruff、Flutter analyze、原生 Kotlin／Manifest／完整 lint、四 EXE 媒體 smoke、線上／離線 Setup、Payload ZIP、原簽章 APK 與五項產物雜湊通過。
- Android 36 模擬器 91→94 覆蓋成功，UID 保留並能顯示首頁；高負載啟動曾逾時。真實手機／另一台 Windows、Google 實機登入、跨網媒體、PiP、耗電及系統更新流程仍待驗證。詳細根因、日誌與 SHA-256 見 `docs/BUILD_REPAIR_2026-11-21.md`。
- 無付費 TURN 預設啟用；受限 NAT 無法保證穿透，Android MediaProjection 每次擷取仍需系統授權。
[Windows／Android Build 94](https://github.com/rray1003/Deskonect-Updates/releases/tag/v0.3.0-preview.94)

## 0.2.x 系列

### 0.2.0-alpha.93 — 2026-11-20

**狀態：Windows／Android 已在公開 Release 共同發布；原 Release 未列出完整逐項變更及真機驗收證據。**

**新增**
- 公開 Release 未提供可獨立核實的逐項紀錄，暫不推測。

**優化**
- 公開 Release 未提供可獨立核實的逐項紀錄，暫不推測。

**除錯**
- 公開 Release 未提供可獨立核實的逐項紀錄，暫不推測。

**驗證與限制**
- 原始碼來源 `35af98c72f99c43a1ed412291ef7525712160742`；公開資產包含 Windows x64、Payload、離線 Setup 與 Android APK。
- 原 Release 註明硬體攝影機、Google 登入與真實設備更新尚未驗證。

[Windows／Android Build 93](https://github.com/rray1003/Deskonect-Updates/releases/tag/v0.2.0-preview.93)

### 0.2.0-alpha.92 — 2026-11-20（台灣時間）

**狀態：Windows／Android 已在公開 Release 共同發布；原 Release 未列出完整逐項變更及真機驗收證據。**

**新增**
- 公開 Release 未提供可獨立核實的逐項紀錄，暫不推測。

**優化**
- 公開 Release 未提供可獨立核實的逐項紀錄，暫不推測。

**除錯**
- 公開 Release 未提供可獨立核實的逐項紀錄，暫不推測。

**驗證與限制**
- 原始碼來源 `e86c0e5888c54350bf893bd28126421edfb3699f`；公開資產包含 Windows x64、Payload、離線 Setup 與 Android APK。
- 原 Release 註明硬體攝影機、Google 登入與真實設備更新尚未驗證。

[Windows／Android Build 92](https://github.com/rray1003/Deskonect-Updates/releases/tag/v0.2.0-preview.92)


### 0.2.0-alpha.91 — 2026-11-20

**狀態：Windows／Android 已共同發布；正式 RTDB 帳戶與雙向協商驗證通過。兩端都需要更新。**

**新增**
- Windows／Android 使用免費 Spark RTDB v2 裝置目錄與攝影機協商，保留 Google 身分驗證及既有裝置簽章。
- 加入正式 Python／Dart 雙向授權協商驗證器與 50 項 RTDB v2 規則測試。

**優化**
- 有效邀請索引查詢最多 8 筆，過期清理每五分鐘最多 4 筆；silent 寫入、回應大小限制、共用退避。

**除錯**
- 修正 Firestore HTTP 429 導致手機登入失敗、PC 裝置清單空白；新版資料流程不再呼叫 Firestore，故障時保留已驗證登入。
- 重導向必須保留完整憑證與查詢，裝置 ID／既有金鑰與 Android v2 簽章保持不變。

**驗證與限制**
- Python 211、Flutter 42、RTDB 規則 50 項通過；Ruff、Flutter analyze、Windows 四個 EXE smoke、兩種 Setup、Android release 編譯與資產雜湊核對通過。
- 正式 Google Auth／RTDB 的 Python／Dart 註冊／列出裝置、雙向簽章邀請／回覆／刪除成功，Firestore 資料呼叫 0。未啟用計費。
- 本機 Windows 90→91 覆蓋安裝成功，原登入自動恢復，畫面顯示同帳號裝置及背景接聽運作中；Android 16 模擬器 90→91 覆蓋安裝及冷啟動成功。實體手機選帳戶登入、跨網媒體、Doze、長時間耗電仍未實機驗收。
- 舊 Firestore 歷史紀錄保留，不自動轉成有效授權。兩端更新並登入後，以原本身分在新目錄重新登記；不需清除 App 資料。

[Windows／Android Build 91](https://github.com/rray1003/Deskonect-Updates/releases/tag/v0.2.0-preview.91)

### 0.2.0-alpha.90 — 2026-11-20

**狀態：Windows／Android 已共同發布。正式 Firestore 配額仍待恢復。**

**新增**
- 加入 429 帳號保留、共用冷卻及大量舊會話查詢的回歸測試。

**優化**
- Windows 僅查詢目標攝影機未過期邀請；兩端取消舊會話全目錄掃描，減少可見清單的自動刷新。
- Firestore 429 共用五分鐘退避，Auth／RTDB SSE 保持獨立。

**除錯**
- 手機裝置登錄配額錯誤不再當成 Google 登入失敗；保存登入並清楚提示暫時無法讀取裝置，稍後重試。
- Windows 配額錯誤保留既有帳號與清單，避免誤導使用者重新登入或清除資料。

**驗證與限制**
- Python、Ruff、Flutter analyze/test 與 Firestore 模擬器回歸已執行，詳見 `docs/ALPHA90_QUOTA_REPAIR.md`。
- 本機發布器 205 Python／41 Flutter 測試通過，四個 Windows 封裝程式 smoke、Setup、既有 v2 簽章 release APK 與 GitHub 資產雜湊核對通過；模擬器 89→90 覆蓋安裝成功。正式後端 429 的 Flutter 帳號恢復測試通過，但不是實體手機驗收。
- 沒有升級 Blaze 或啟用計費；本版無法補回已耗盡的服務端額度，正式同步恢復及 24 小時用量仍待驗證。

[Windows／Android Build 90](https://github.com/rray1003/Deskonect-Updates/releases/tag/v0.2.0-preview.90)

### 0.2.0-alpha.89 — 2026-11-20

**狀態：已發布 Windows x64 + Android，共同 Build 89。**

**新增**
- 免費 Spark RTDB 實例及安全規則真正部署；兩端內建公開 SSE 網址。加入 RTDB 規則／SSE 整合測試、Android 影格停滯偵測及分享停止通知。

**優化**
- 沿用簽章 Firestore 邀請、SSE 即時提示與安全輪詢回退；背景閒置釋放鏡頭，斷線漸進重試。
- Windows 攝影機等待邀請回覆涵蓋背景待命排程，短暫 ICE 中斷先給既有連線恢復機會。

**除錯**
- 修正 Android WebRTC applyConstraints 不會改變影像尺寸的問題；改為重建本機擷取及替換 RTP track，保留 SDP/ICE。
- 修正 SSE 無資料逾時、停止後擷取競態、Windows 舊鍵鼠事件重播及明確授權拒絕後仍重試。
- 修復交接程式 Ruff 診斷與生命週期測試。

**驗證與限制**
- Python 203、發布工具 17、Flutter 40 項測試；Ruff／Flutter analyze；RTDB 19、Firestore 33、Auth/同步 33 項模擬器檢查通過。既有 v2 憑證核對與 Kotlin/Gradle debug APK 編譯通過。
- 正式 RTDB 規則已回讀，匿名存取拒絕；管理者 SSE 寫入收送成功。未啟用計費，無付費 TURN。
- Windows 使用者 SSE 喚醒兩次各 0.157 秒，主動切斷 socket 後 2.156 秒自動恢復；實際 Dart SSE 喚醒 134～135 ms。這些是通道延遲，不是鏡頭首幀或跨裝置串流重連時間。
- 兩種 Windows Setup、四個封裝程式 smoke、既有 v2 簽章 release APK、GitHub 資產雜湊核對通過；Android 16 模擬器 Build 82→89 覆蓋安裝及啟動成功。另有 1 項 opt-in 正式 Dart SSE 測試通過。
- 無實體手機／第二台 Windows；Doze、耗電、跨網、鏡頭畫質切換與各方向端到端重連尚未實機驗收。完整步驟見 `docs/REALTIME_RELEASE_VALIDATION_2026-11-20.md`。

[Windows／Android 共同 Release](https://github.com/rray1003/Deskonect-Updates/releases/tag/v0.2.0-preview.89)

### 0.2.0-alpha.87 — 2026-11-19

**狀態：已發布（Windows）；沿用本輪 Windows 優先發布授權，Android 保持原版。**

**新增**
- App 內使用 Google 憑證 HTTPS 回應時間建立登入驗證基準，不修改 Windows 時鐘。

**優化**
- 登入有效期及續期改用單調時鐘；Google 金鑰輪替時重新抓取並重驗一次。

**除錯**
- 修正本機時間不準導致新 Google／Firebase 憑證被拒絕；仍以 Google 時間嚴格檢查有效期並驗證簽章、issuer、audience。

**驗證與限制**
- 175 Python tests、Ruff、四個執行檔 smoke 通過；模擬本機 ±30 天，新 token 通過、過期及竄改 token 仍拒絕。真實 Firebase 續期於程序內模擬本機 ±1 天均通過。
- 上傳三個資產 SHA-256 相符，公開更新器可由 Build 86 讀到 87。新 Build 在第三台錯時鐘環境下的實機驗收仍待確認；TLS 無法建立可信連線時不會繞過安全檢查。
- 來源：`24c97d981b88fc48f15ad37ee748cffea7a5166e`。

[Windows 登入時間校正修正版](https://github.com/rray1003/Deskonect-Updates/releases/tag/v0.2.0-preview.87)

### 0.2.0-alpha.86 — 2026-11-19

**狀態：已發布（依本次使用者指示僅 Windows）；Android 保留 Build 84。**

**新增**
- 無。

**優化**
- Windows 更新期間暫停背景代理 watchdog，等待代理生命週期 mutex 釋放後才執行安裝；逾時取消並恢復原程式。

**除錯**
- 修正舊版 headless 攝影機代理無法被安裝器正常關閉，造成靜默安裝中止、重新啟動後重複要求更新的問題；安裝器僅針對 Deskonect 執行檔關閉占用，成功後恢復代理。
- 包含 main 已合併的 Google 憑證傳輸備援與錯誤分類修正，避免把無效憑證一律誤報為網路問題；詳見 docs/GOOGLE_LOGIN_NETWORK_REPAIR.md。

**驗證與限制**
- 合併最新 main 後本機 170 項 Python 測試、Ruff、GUI／CLI／Agent／獨立更新助手 smoke、兩種 Windows Setup 建置通過；三個公開資產 SHA-256 均與本機相符。
- 公開 latest 已核對為 Build 86；舊更新器以 Build 83／85 檢查可取得 86，以 Build 86 檢查無更新。此為真實更新 API 驗證，不代表已完成安裝。
- Windows 實際安裝目前仍為 Build 83；修正後端到端升級、另一台電腦登入、真實手機與跨網相機未驗收。未聲稱已修復所有設備問題。
- Android v2 發行金鑰密碼目前不可用，依使用者明確選擇只發布 Windows；沒有換金鑰或把舊 APK 標為新 Build。
- 安裝檔來源：`e768e44d1236163fd10e0d190c345efa54a092cb`。

[Windows 更新修正版](https://github.com/rray1003/Deskonect-Updates/releases/tag/v0.2.0-preview.86)

### 0.2.0-alpha.85 — 2026-11-19

**狀態：已發布（本次使用者明確同意僅 Windows）；Android 保留 Build 84。**

**新增**
- Google／Firebase 公開驗證憑證下載的系統 HTTPS 備援與有限重試。

**優化**
- 尊重伺服器有效期快取公開憑證，不使用過期快取。

**除錯**
- 修正無效簽章被誤報為 G-NETWORK；回傳網頁等待 Windows Google 身分驗證結果，失敗不回報成功。

**驗證與限制**
- 本機 167 Python tests、Ruff、GUI／CLI／Agent／更新助手 smoke 通過；三個上傳資產 SHA-256 相符，公開更新器從 Build 84 可讀到 Build 85。
- 第三台 Windows 真實 Google 登入、完整更新安裝與受管理網路尚待驗收；斷網不會繞過驗證。Windows Setup 尚無 Authenticode 簽章。
- 原始碼：`ca472bc9c8f72eee8106c80615a6d659b63cbae0`。

[Windows 修正版](https://github.com/rray1003/Deskonect-Updates/releases/tag/v0.2.0-preview.85)

### 0.2.0-alpha.82 — 2026-11-19

**狀態：已發布（Alpha）；功能實機驗收未全數完成。**

**新增**
- Windows x64 與 Android v2 的 Build 82 安裝檔整合於同一個公開 Release，並保留舊 Android 更新器使用的相容入口（沿用 Build 81 起的發布設計）。

**優化**
- Windows 提供輕量網路安裝程式、完整的離線 App 內更新包與 SHA-256 驗證；公開舊版資產已先完成中繼資料歸檔後下架。
- Android v2 模擬器完成 Build 81 → 82 的 App 內下載、系統安裝及重新啟動驗證。

**除錯**
- 修復 Windows 線上安裝時只解壓最外層、導致舊版 `_internal/assets/build-info.json` 仍留在安裝目錄的問題；Build 82 本機安裝已核對新版本戳記。

**驗證與限制**
- Python 123 tests、Flutter 21 tests、Ruff、Flutter analyze 通過；Windows 本機安裝與 Android 36 模擬器測試通過。
- 真實手機、另一台 Windows、完整跨網控制與相機、Google 登入、Windows Build 82 → 後續 Build 的端到端自動更新仍未在本輪全部驗收。
- Android 舊 v1 package 的簽章與 v2 不同，不能原地覆蓋更新；Windows 安裝程式尚未有 Authenticode 簽章。

[共同下載與驗證報告](https://github.com/rray1003/Deskonect-Updates/releases/tag/v0.2.0-preview.82) · [Build 82 驗證資料（私人來源庫）](https://github.com/rray1003/Deskonect/blob/main/docs/ALPHA82_RELEASE_REPORT.md)

---

## 歷史發布（0.2.0-alpha.82 之前）

舊版本的 Release 已按要求從公開下載頁撤下，tag 仍保留。2026-11-19 清理前的 15 筆 Release 文字、資產名稱、雜湊、日期已記錄於私人來源庫的 [舊版歸檔](https://github.com/rray1003/Deskonect/blob/main/docs/RELEASE_ARCHIVE_PRE_ALPHA82.json)。這些歷史資料不是逐版人工整理的「新增／優化／除錯」日誌，因此不冒充已完整補記。

## 後續版本撰寫規則

每次成功發布後，在目前 minor 系列**最上方**加入 `### x.y.z-channel.Build — YYYY-MM-DD`，依序撰寫「新增／優化／除錯／驗證與限制」，附 Release 連結與來源提交。新的 minor／major 開新系列並保留過去系列與每筆 Build；不得因公開 Release 檔案下架而抹除文字紀錄。若發布失敗，保留為草稿而非「已發布」。
