# Build 90：帳號 HTTP 429 與免費配額修正（2026-11-20）

## 已確認的事故與邊界

使用者在 Build 89 更新後回報手機無法登入、Windows 登入卻沒有裝置。正式 Firestore 使用既有 Windows Firebase 使用者 token 讀取 1 筆裝置，得到 HTTP 429、status RESOURCE_EXHAUSTED、message Quota exceeded；背景代理亦反覆記錄 429。Auth 仍可續期及驗證，因此不是以此證據認定 Google 密碼、簽章或裝置資料被刪除。

Cloud Billing API 本輪再次確認 billingEnabled=false。未升級 Blaze、未綁定帳單、未啟用付費服務。Firestore 免費限制是每日文件讀取 50,000、寫入 20,000，官方說明約太平洋午夜重設；不是扣款後可繼續用。尚未取得本輪是哪項操作先達上限的完整使用量明細，不能把根因量化為某台裝置的確切讀取數。

官方：[Firestore 免費配額](https://firebase.google.com/docs/firestore/quotas)。RTDB SSE 只負責喚醒，Build 89 的裝置目錄及簽章 SDP 仍使用 Firestore；SSE 上線不代表 Firestore 已不再消耗額度。

## 新增／優化／除錯

- 手機 Google/Firebase 帳號驗證與裝置登錄分開。Firestore 429 不再回復未登入狀態，不刪除保存的 refresh token；保留帳號，清楚顯示雲端目錄暫時不可讀，五分鐘後重新嘗試登錄。
- 警示放在全頁可見位置；空清單不再誤稱「沒有裝置」。切換帳號時清除前一帳號的顯示資料；登出清除警示，不擅自恢復相機。
- Python 與 Dart 的 Firestore 429 使用程序共用五分鐘冷卻。Auth、SSE 不受此冷卻阻擋。沒有用取消驗證或把舊裝置資料另存成授權來源的方式掩蓋錯誤。
- Windows 接聽端改為 Firestore runQuery，服務端過濾 host_id 和未過期 expires_at，最多八筆。刪除反覆掃描最多五百筆舊會話的方式；Android 索引不可用時也不再退回全目錄掃描。
- 正式索引狀態已核對 READY。模擬器以實際 Python REST 查詢，放入兩百筆過期資料及一筆新邀請，仍只回傳新邀請。
- Windows UI 自動同步由 30 改為 120 秒；Android 可見清單最短間隔兩分鐘。Windows 攝影機訊號或背景代理遇到配額錯誤時延長等待，避免失敗後更頻繁請求。既有配對、TLS、簽章、Firestore 安全規則沒有放寬。

## 驗證

- 發布器完整 Python 測試 205 通過，包括 Windows 配額警示保留會話及既有清單、共用冷卻不影響 Auth 的測試。Ruff 通過。
- Flutter analyze 通過；41 項測試通過、1 項需正式 token 的 opt-in SSE 測試預設略過。其中新 widget 測試模擬有效 Google refresh/lookup + Firestore 429，確認仍顯示已登入帳號、保存新 refresh token、顯示 429、跨 FirebaseDirectory 實例不再連發 Firestore 請求，Auth lookup 仍正常。
- Firestore 模擬器 33 項授權檢查通過；實際 Python indexed query 通過 200 筆過期資料案例。
- 未執行實體手機新登入與免費額度重設後的跨裝置恢復；未完成 24 小時使用量量測，因此不能保證所有裝置數／使用頻率都不會達到免費上限。
- 另外在 Windows Flutter 測試程序使用既有正式 Firebase refresh token，執行實際帳號恢復／lookup，連到正式 Firestore 得到真實 429，確認帳號仍保持已登入、refresh token 保存、畫面顯示配額警示。此為正式後端 + 桌面 Flutter 測試，不是 Android 實體手機 Google 帳號選擇器驗收；憑證僅存在子程序環境及測試記憶體，未輸出／提交。
- Cloud Monitoring 在台灣 2026-11-19 01:49～2026-11-20 01:49 的成功文件讀取計數為 LOOKUP 1,759 + QUERY 30,929；寫入 CREATE 133 + UPDATE 4,954 + UPDATE_NOOP 80。這些監控計數不能直接當成完整免費配額帳本，也沒有證明某一台裝置單獨造成耗盡。[官方監控指標說明](https://firebase.google.com/docs/firestore/monitor-usage)。

## 仍然受阻

**修正客戶端不會補回本日已耗盡的服務端額度。正式裝置目錄與攝影機 Firestore 信令目前仍可能回 429；必須等待服務端額度恢復後，再確認兩端裝置重新登錄與互相可見。** 不把新的警示畫面當成服務已恢復，也不把等候重設當成設計問題已完全解決。

所有仍在使用的 Windows／Android 都需要升級，舊版仍可能繼續高頻讀取。長期驗收須量測多裝置待命至少 24 小時，記錄 Firestore 讀寫及 RTDB 流量；達上限仍只能暫停並回報，不升級付費。若使用量持續超過免費服務上限，需要另評估有完整授權／資料遷移測試的免費傳輸方案，不能臨時改用不同目錄而讓撤銷狀態分裂。

## 發布

**0.2.0-alpha.90 / Build 90 已共同本機建置並發布**；二進位來源 `405127e120915d5315b2022780dbc2ad92b5a9a7`。本機紀錄 `%LOCALAPPDATA%/Deskonect/Logs/local-release-20261120-014918.log`。Windows 四個封裝程式 smoke 及兩種 Setup 建置通過；Android assembleRelease、package／versionCode／既有 v2 簽章核對通過；GitHub 草稿資產 digest 全部與本機 SHA-256 相符後才公開。本次沒有把本機已安裝的 Windows App 升級當成已完成。

Android 16 模擬器 89→90 `adb install -r` 成功，versionCode=90／versionName=0.2.0-alpha.90。首次 `am start -W` 逾時，但系統稍後記錄首幀 25.509 秒；畫面已實際檢視為登入首頁，程序存活、前景 Activity 正常，查閱範圍未見 crash/ANR。強制停止後再冷啟動為 Status ok、12.016 秒。**啟動偏慢仍待量測與優化，不能標示為快速啟動通過，也不能等同真機登入驗收。**

共同入口：[Windows + Android Build 90](https://github.com/rray1003/Deskonect-Updates/releases/tag/v0.2.0-preview.90)。舊 Android 更新器相容入口保留 `v0.2.0-android.90`，APK 內容相同。

| 檔案 | SHA-256 |
|---|---|
| [Windows Setup](https://github.com/rray1003/Deskonect-Updates/releases/download/v0.2.0-preview.90/Deskonect-0.2.0-alpha.90-x64.exe) | `202ae3190e2636ab1ddf4ea57e9a6ba0e1f548c77d1756bed14414305933675b` |
| [Android APK](https://github.com/rray1003/Deskonect-Updates/releases/download/v0.2.0-preview.90/Deskonect-0.2.0-alpha.90.apk) | `ce5d6369b3598f8d5602822ea6406eccee83050be10bdc33ccc95fbdb1bbdcd1` |
| Windows payload | `75022dc99a295e22028f43cff030b0f9ead3552f7c8bec5a047d648386758109` |
| Windows 離線更新 Setup | `cc3a5bac22ac66a1aba04fdec07f44560f30ad1d4e00c3b2d0ce6054866875b4` |

**Build 90 發布成功不代表正式 Firestore 雲端服務已恢復。**
