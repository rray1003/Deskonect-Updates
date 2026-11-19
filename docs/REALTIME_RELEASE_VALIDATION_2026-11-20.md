# 免費即時接聽與重連驗證（2026-11-20）

## 範圍與狀態

直接在 main 從遠端 `66fc647161845eaa3ea37ef15b9ed2081a293839` 接手；根目錄是無提交的舊快照，實際 Git 工作目錄為 `work/main`，未覆寫根目錄或其他工作目錄。此版仍為 Alpha。真實手機、第二台 Windows 及跨網驗收未完成。

## 新增

- 實際建立 `deskonect-rray1003-default-rtdb`，區域 `asia-southeast1`，部署 `database.rules.json`。兩端內建已部署的公開網址，不需每部 Windows 手動設定環境變數；明確設空網址仍可停用 SSE。
- RTDB 模擬器加入 19 項跨帳號、未登入、欄位、時間戳、SSE 事件與斷線重新訂閱測試。
- Android 觀看端透過 WebRTC inbound stats 偵測 15 秒影格計數不前進；不支援原生統計時不誤判為影像凍結。
- DataChannel `camera:stopped:v1` 告知新版觀看端主機停止或撤銷授權，停止自動重連。

## 優化

- 沿用簽章 Firestore offer，RTDB 僅為 nonce 喚醒提示，不提供攝影機授權。SSE 正常時 45 秒保底查詢，失效時安全回退；不是所有網路都能免費 P2P。
- Android SSE 75 秒無資料逾時重建，短命連線採漸進退避；重新訂閱重新取得 Firebase token。
- Windows 攝影機等待回覆由 40 延長至 90 秒，涵蓋背景回退排程；斷線給既有 ICE 四秒自復原機會。
- Android 背景服務沿用已啟動的 FlutterEngine 與離屏元件，無觀眾時釋放擷取。程序被回收、強制停止、重啟後須使用者開啟 App／點恢復通知，不從背景秘密啟動相機服務。

## 除錯

- 修正 Ruff 七項診斷及舊生命週期測試未等待取消完成的預期。
- Windows 桌面明確拒絕授權後停止重試；無連線時不接收操作，進入新會話前丟棄佇列，避免重播舊輸入。
- Android LAN 明確拒絕授權／TLS 指紋不符／格式無效時停止重試。
- Android 影像 `applyConstraints()` 在 flutter_webrtc 1.6.3 為無作用操作（只支援 volume）；改為重新擷取、`replaceTrack()` 保留既有 SDP/ICE/DataChannel，失敗嘗試恢復舊設定。驅動可能選擇鄰近尺寸，畫質名稱不是實測解析度。
- 攝影機停止時等待 track.stop，並防止較早的非同步擷取在停止後重新啟用。活動 Android 觀看者每 30 秒檢查登錄金鑰仍有效。
- 關閉 Android 觀看頁後，延遲完成的 peer 建立會立即關閉；延遲寫入的邀請會立即刪除，不再繼續喚醒目標。

## 實際驗證

| 檢查 | 結果與證據範圍 |
|---|---|
| Python | 修復前 198 通過／1 失敗；修復及新增行為測試後 203 項通過（最終發布器再次完整執行） |
| 發布工具 | `python -m pytest tools -q`：17 通過 |
| Ruff | src、tests、prepare-android、collect-build-licenses 通過 |
| Flutter | analyze 無問題；40 項 test 通過 |
| Kotlin／Gradle | debug APK assembleDebug 成功，既有 v2 憑證前置檢查成功；最終 release 建置另記於發布結果 |
| Firebase | JDK 25 + Firebase CLI 15.19.1，database/auth/firestore 模擬器：19 RTDB、33 Firestore、33 Auth/同步通過 |
| 正式 RTDB | ACTIVE，規則部署成功並回讀相符；未登入讀取 HTTP 401；管理者 OAuth SSE HTTP 200 並收到實際 PUT 事件，探測資料已刪除。管理者測試不等於 App 使用者端端到端測試 |
| 費用 | Cloud Billing API 在建立前及規則部署後均回報 billingEnabled=false；未綁定帳單、未升 Blaze、未啟用付費 TURN |
| Firestore index | `firebase deploy --only firestore:indexes` 成功 |
| 重連行為 | Python 實際執行的模擬會話測試：暫時斷線後重試、明確拒絕後不重試、未連線不排入輸入、SSE 不可用退避與重新取得 token。不是實體 Wi-Fi／行動網路測試 |

正式 Windows 使用者通道另外使用既有 DPAPI 登入經 FirebaseRegistry 驗證，直接執行 CameraWakeListener + send_camera_wake：兩次訂閱／重新訂閱的 PUT 至回呼耗時各 0.157 秒，停止後執行緒正常結束。主動從接聽執行緒切斷 SSE socket，再由原 run 迴圈自動恢復訂閱，耗時 2.156 秒，恢復後新喚醒成功。未輸出 ID token，隨機測試節點均已刪除。這不等於遠端鏡頭首幀或跨裝置串流恢復時間。另直接執行 CameraWebRTCHost._serve，以不可用 RTDB、模擬空 Firestore 訊號驗證回退排程：兩次查詢間隔 5.000 秒，沒有開鏡頭。

Windows 獨立 Python SSE 接聽程序，在正式使用者 SSE 已建立後量測 30.000 秒：process_time 差為 0.000 秒（低於本次計時解析度，不能宣稱 CPU 絕對為零），Windows GetProcessMemoryInfo 工作集 46.59→46.64 MiB；閒置事件回呼 0、測試程式 Firestore 查詢 0、未開啟相機。此測試不含 GUI、完整 CameraWebRTCHost 的 45 秒保底查詢、裝置心跳或完整網路下載 bytes；不能當成 Android 耗電或完整 App 資源數據。

可重現指令：使用 `%LOCALAPPDATA%/Deskonect/BuildTools/venv311/Scripts/python.exe -m pytest -q`；PATH 的 MSYS Python 缺少 encodings，不能使用。`ruff check src tests scripts/prepare-android.py scripts/collect-build-licenses.py`；`flutter analyze`、`flutter test`。規則測試以 JDK >=21 執行 `firebase emulators:exec --only database,firestore,auth --project demo-deskonect "node scripts/test-database-rules.mjs && node scripts/test-firestore-rules.mjs && node scripts/test-account-sync.mjs"`。Android 建置繼續使用 JDK 17。

## 未驗證與實機步驟

ADB 僅有 emulator-5554，沒有實體手機或第二台 Windows。**完整 App／Android 背景耗電百分比、CPU、記憶體、每小時實際 Firebase 請求量及各方向影像重連時間未量測**。上方僅有 Windows SSE 獨立接聽程序的範圍受限數據。45 秒保底查詢與 60 秒心跳是程式排程，不是耗電測量。

1. 兩台 Android 與兩台 Windows 安裝同版，登入同帳號並確認各自裝置金鑰；在目標手機可見畫面啟用分享及授權，關閉分享頁、按 Home、鎖屏後各待命 30 分鐘。記錄服務通知、Firestore 讀寫、RTDB 下載與 Android batterystats／Power Profiler、CPU、PSS。
2. Android A→B、B→A、Windows→Android、Android→Windows、Windows→Windows 分別測攝影機；兩種觀看端切前後鏡頭與畫質，確認 session ID 不變並記錄實際影像尺寸。
3. 各方向斷網 3 秒／15 秒／2 分鐘、切 Wi-Fi／行動網路；記錄最後影格到第一個恢復影格的時間。桌面同時確認輸入不會重播。
4. 開啟 Doze、省電模式、鎖屏、移除 Activity，檢查待命可用性；程序被殺後確認恢復通知／重新開啟，不應背景自啟相機。
5. 分享端停止、觀看端關閉、登出、刪除或撤銷裝置，各驗證鏡頭關閉且不未授權重連。DataChannel 停止通知屬 best effort，網路同時中斷可能遺失；重新要求仍必須重新驗證授權。
6. 阻斷 RTDB 而保留 Firestore，確認提示回退仍能接聽；恢復 RTDB 後確認 SSE 重訂閱。Spark 免費額度可用性不能保證，超量不升級付費。

官方依據：[RTDB REST/SSE](https://firebase.google.com/docs/database/rest/retrieve-data)、[Android camera FGS 限制](https://developer.android.com/develop/background-work/services/fgs/service-types)。

## 發布結果

已本機建置及發布 **0.2.0-alpha.89 / Build 89**，未使用 GitHub Actions 建置。二進位來源提交：`232b74670c9dcbd80f6282b75271114b6d8c5f12`。本機紀錄：`%LOCALAPPDATA%/Deskonect/Logs/local-release-20261120-004622.log`。後續報告／測試檔提交不改動二進位程式碼。

- Windows PyInstaller GUI、CLI、Agent、Update helper smoke 全通過；Inno Setup 6.7.3 產生網路 Setup、離線更新 Setup 及 payload。
- Android release Gradle assembleRelease 成功；apksigner 驗證既有 v2 SHA-256 憑證 `4dbfc50a4219aea08410c6cd0ac9b72b5257782e7b0147fe2687ebec528917e4`；package=`app.deskonect.deskonect_android.v2`，versionCode=89，versionName=0.2.0-alpha.89。未產生新金鑰。
- Android 16 `sdk_gphone64_x86_64` 模擬器由原 Build 82 `adb install -r` 升級成功；am start -W 回報 Status ok，啟動後 PID 存在，所查 300 行 app 日誌未見 FATAL EXCEPTION／Unhandled Exception／Fatal signal。不是實體手機攝影機或登入驗收。
- 發布器先上傳草稿，逐一核對 GitHub digest 與本機 SHA-256 後公開共同 Release 和舊 Android 相容入口。先前取消的建置未發布。
- 真實公開 Windows 更新 API：`check_update(88)` 取得 89，`check_update(89)` 回傳無更新。僅驗證更新資訊，沒有把這台已安裝 Windows App 自動升級當成已通過。
- 實際 Dart CameraWake 程式透過 Windows Flutter test 程序連正式 RTDB，以既有 Firebase 使用者 token 執行喚醒及手動停止，134～135 ms，1 項通過；可重跑 `android_client/test/camera_wake_live_test.dart`。僅從程序環境提供 DESKONECT_TEST_UID／DESKONECT_TEST_TOKEN／隨機 DESKONECT_TEST_HOST，缺少時預設略過，不能寫入 Git／命令列定義／日誌。測試會刪除自己的隨機節點。

共同 Release：[v0.2.0-preview.89](https://github.com/rray1003/Deskonect-Updates/releases/tag/v0.2.0-preview.89)；舊 Android 更新器：[v0.2.0-android.89](https://github.com/rray1003/Deskonect-Updates/releases/tag/v0.2.0-android.89)。

| 資產 | SHA-256 |
|---|---|
| [Windows Setup](https://github.com/rray1003/Deskonect-Updates/releases/download/v0.2.0-preview.89/Deskonect-0.2.0-alpha.89-x64.exe) | `f91454e4e9c8c9ed523654656468464a00681daed2638af76398d63713a15dd5` |
| [Android APK](https://github.com/rray1003/Deskonect-Updates/releases/download/v0.2.0-preview.89/Deskonect-0.2.0-alpha.89.apk) | `0287af4d3001d006fd1cf0a8f8bd75af431c4773b0f5e753b73efa41028454ea` |
| Windows payload | `9f4c0b551fa9aa9bc243196836e7ded1a63eeab989e15eb849c92b777e54fc98` |
| Windows 離線更新 Setup | `aebc671e3ad9ffb68474daca95f6655fa59fa371da111328967631d93c4f66f4` |

完整 App 實機接受度仍依未驗證清單；共同發布成功不代表主要目標全部完成。
