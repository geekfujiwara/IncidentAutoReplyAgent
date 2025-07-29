# 📘 Copilot Studio Solution: PC不具合対応ワークフロー

このリポジトリは、https://learn.microsoft.com/power-platform/ の **Copilot Studio** を用いて構築した「PC不具合対応ワークフロー」のソリューションです。  
ユーザーからの不具合報告を受け付け、保証期間の判定、対策案の提示、対応履歴の記録までを自動化するエージェント群とツールを実装しています。

---

## 🧩 構成

本ソリューションは以下の **親エージェント** と **子エージェント**、および **ツール** で構成されています。

### 👑 親エージェント

- **PC不具合対応エージェント**  
  ユーザーからの不具合報告に基づき、全体のワークフローを統括し、適切な子エージェントやツールを呼び出して対策案を検討します。

### 🧒 子エージェント

- **PC不具合対策検討エージェント**  
  不具合の内容に応じて具体的な対策案を検討します。

- **保証判定エージェント**  
  本日の日付とシリアル番号をもとに、保証期間内かどうかを判定します。

### 🛠️ ツール

- **不具合対策案のメール返信**  
  保証期間内の場合、対策案をメールで返信します。

- **保証期間外のため対応ができない旨のメール**  
  保証期間外の場合、対応できない旨をメールで返信します。

- **保守サービスシステムへの対応履歴の記録**  
  対応内容を保守サービスシステムに記録します。

---

## 📊 シーケンス図

```mermaid
sequenceDiagram
    participant User as ユーザー
    participant Trigger as 不具合報告トリガー
    participant Parent as PC不具合対応エージェント
    participant Child1 as PC不具合対策検討エージェント
    participant Child2 as 保証判定エージェント
    participant ToolOK as 不具合対策案のメール返信
    participant ToolNG as 保証期間外メール
    participant ToolLog as 対応履歴記録ツール

    User->>Trigger: 不具合報告メール送信
    Trigger->>Parent: 起動通知
    Parent->>Child1: 対策案検討依頼
    Child1-->>Parent: 対策案
    Parent->>Child2: 保証判定依頼
    Child2-->>Parent: 判定結果
    alt 保証期間内
        Parent->>ToolOK: 対策案メール送信
    else 保証期間外
        Parent->>ToolNG: 対応不可メール送信
    end
    Parent->>ToolLog: 対応履歴を記録
```


## 🚀 導入方法

1. https://github.com/your-org/copilotstudio-pc-issue-workflow/releases から最新のソリューションファイル (.zip) をダウンロードします。

2. https://make.powerapps.com/ にアクセスし、Power Platform 環境にソリューションをインポートします。
   - 左側メニューの **「ソリューション」** を選択し、**「インポート」** をクリックします。
   - ダウンロードした .zip ファイルを選択してインポートします。

3. インポート後、**すべてのカスタマイズを公開** します。
<img width="712" height="157" alt="image" src="https://github.com/user-attachments/assets/1ddab924-356a-4ec0-847d-4a1caa4459dc" />


4. モデル駆動型アプリ`受注管理アプリ`を開きます。
<img width="1129" height="358" alt="image" src="https://github.com/user-attachments/assets/974503cd-492f-4561-b98e-1a2c93c94bd1" />

5. 1年以内の販売日を選択し、保存します。
<img width="606" height="391" alt="image" src="https://github.com/user-attachments/assets/ad052afe-27bc-4fa8-b05e-afdfa85bfb55" />

6. 保存すると出力されるシリアル番号をメモしておきます。
<img width="1430" height="728" alt="image" src="https://github.com/user-attachments/assets/ab817940-46e7-4d6c-9ee4-ba9f9f1cb882" />


7. エージェントにメールを送信します。件名には、`不具合報告`といれる必要があります。
>[!Note]
>デモ用に不具合報告を入れたときだけトリガーされるようにしていますが、トリガーに設定されているフローにて変更できます。

8. メールをインポートしたユーザーに対して送信します。以下の本文を参考に利用してください。シリアル番号はメモした内容を利用してください。
<img width="1235" height="783" alt="image" src="https://github.com/user-attachments/assets/43d42735-ccd7-4226-9984-99e0890d5467" />


```デモ用のメール文章
ご担当者様

お世話になっております。田中と申します。

購入したSurface Laptop にトラブルがあります。
正しく電源コードはつなげていますが電源ボタンを押しても起動しません。どのようにすればよいでしょうか。
ちなみに充電ランプは点灯しています。

シリアル番号: {ここにシリアル番号をいれる}

よろしくお願いいたします。

高橋太郎

```
9. 暫く待つと、自動的にメールがエージェントにより作成され返信されます。
<img width="1256" height="997" alt="image" src="https://github.com/user-attachments/assets/16c4a5c6-b6d2-42c5-bc5a-3f1f8c6f9ff6" />

エージェントにより、このように自動的な判断と処理がなされています。
<img width="1186" height="885" alt="image" src="https://github.com/user-attachments/assets/5d9e2cc1-6a53-4872-b792-32c3eebf80a1" />

---

## 🔗 参考情報（Microsoft公式ドキュメント）

以下の公式ドキュメントが参考になります。

- https://learn.microsoft.com/power-platform/
- [Power Apps ソリューションのインポート](https://learn.microsoft.com/power-apps/maker/data-platform/import-update-exporte の接続とコネクタ](https://learn.microsoft.com/power-automate/conarn.microsoft.com/power-virtual-agents/overview
- https://learn.microsoft.com/power-platform/admin/environments-overview
- https://learn.microsoft.com/power-automate/fix-flow-failures

---

他にも必要に応じて、具体的なエラーコードやスクリーンショットをもとにサポートしますので、お気軽にご相談ください！

