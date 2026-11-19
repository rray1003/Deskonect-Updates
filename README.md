# Deskonect — Windows / Android Alpha 下載

最新版：**Deskonect 0.2.0-alpha.91**（2026-11-20）。

- [Windows x64 網路安裝程式](https://github.com/rray1003/Deskonect-Updates/releases/download/v0.2.0-preview.91/Deskonect-0.2.0-alpha.91-x64.exe)
- [Android v2 APK](https://github.com/rray1003/Deskonect-Updates/releases/download/v0.2.0-preview.91/Deskonect-0.2.0-alpha.91.apk)
- [最新共同 Release](https://github.com/rray1003/Deskonect-Updates/releases/latest)
- [完整更新日誌（自 Build 82 起保留每一版）](CHANGELOG.md)

Windows Setup 需要網路下載同版本的程式封裝；Android v2 以相同發行簽章更新，**無法直接覆蓋原 Android v1 App**。此版仍為 Alpha，實機的登入、跨網連線、攝影機及未來版本的完整更新流程未全部驗收。詳情見[Build 91 驗證報告](https://github.com/rray1003/Deskonect/blob/main/docs/ALPHA91_RTDB_ACCOUNT_REPAIR.md)（私人原始碼儲存庫，需權限）。

**帳戶修復：電腦與手機都要升級到 Build 91，開啟並登入同一 Google 帳號。新版使用免費 RTDB，避免原 Firestore 429 阻擋裝置同步；不需要清除 App 資料。**

## 下載與版本保留原則

此公開庫只保留最新的 Windows／Android 共同 Release，及讓舊 Android v2 更新器繼續工作的必要相容入口。已下架的舊版安裝檔不再提供下載，但舊 tag 仍保留，發布資訊與雜湊已保存在私人來源庫的 [Release 歷史歸檔](https://github.com/rray1003/Deskonect/blob/main/docs/RELEASE_ARCHIVE_PRE_ALPHA82.json)。

相容更新器目前會排除 GitHub 的 prerelease；所以 Alpha 的共同 Release 暫時維持 GitHub `prerelease=false`，**不表示產品已為正式穩定版**。請勿單獨刪除 `v0.2.0-android.91` 相容入口。

原始碼：私人儲存庫 `rray1003/Deskonect`；這個公開儲存庫只託管已發布安裝檔與更新日誌。
