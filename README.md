# 📘 Copilot Studio Solution: PC不具合対応エージェント

**Copilot Studio** を用いて構築した「PC不具合対応エージェント」のソリューションです。  
ユーザーからの不具合報告をメールで受け付け、保証期間の判定、対策案の提示、対応履歴の記録までを自動化するエージェント群とツール

## PC不具合対応エージェント
PC不具合対応エージェントは、Surface の不具合対応を題材とした以下の作業を代行する、サポート担当者向けのAIエージェントです。

💬 PC製品の不具合報告メールに対して、自動的に起動

🔍 CRMから販売日を取得し、社内マニュアルに基づき保証判定

✉️ ナレッジに基づき対策方法、またはサポート契約のご提案メール返信の実行

📝 対応履歴の登録

---

## 動画

https://github.com/user-attachments/assets/b8d7bdc7-f29d-4815-a419-877efc1d83e5

---

## 🧩 構成

本ソリューションは以下の **親エージェント** と **子エージェント**、および **ツール** で構成されています。


<img width="1980" height="1112" alt="image" src="https://github.com/user-attachments/assets/acb60415-1e47-44de-bef6-53f2c44db063" />


### 👑 親エージェント
<img width="358" height="234" alt="image" src="https://github.com/user-attachments/assets/a6c475a0-0898-4045-9234-df7491aa9f8e" />

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

## 📊 処理の流れ

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

1. [このリポジトリのリリース](https://github.com/geekfujiwara/IncidentAutoReplyAgent/releases)から最新のソリューションファイル (.zip) をダウンロードします。

2. https://make.powerapps.com/ にアクセスし、Power Platform 環境にソリューションをインポートします。
   - 左側メニューの **「ソリューション」** を選択し、**「インポート」** をクリックします。
   - ダウンロードした .zip ファイルを選択してインポートします。

3. インポート後、**すべてのカスタマイズを公開** します。
<img width="712" height="157" alt="image" src="https://github.com/user-attachments/assets/1ddab924-356a-4ec0-847d-4a1caa4459dc" />

4. 親エージェントであるPC不具合対応エージェントを開きます。その接続情報を更新します。設定からすべて接続して状態を接続済みにします。
<img width="1806" height="485" alt="image" src="https://github.com/user-attachments/assets/4c21e55c-450a-4c39-bb86-71ac4447a742" />


ここまでで導入は完了です。次は実際にテストしてみます。

テストシナリオは以下の2種類あります。

1. 保証期間内の場合
2. 保証期間外の場合
---
### (1) 保証期間内の場合

1. モデル駆動型アプリ`受注管理アプリ`を開きます。
<img width="1129" height="358" alt="image" src="https://github.com/user-attachments/assets/974503cd-492f-4561-b98e-1a2c93c94bd1" />

2. 1年以内の販売日を選択し、保存します。
<img width="606" height="391" alt="image" src="https://github.com/user-attachments/assets/ad052afe-27bc-4fa8-b05e-afdfa85bfb55" />

3. 保存すると出力されるシリアル番号をメモしておきます。
<img width="1430" height="728" alt="image" src="https://github.com/user-attachments/assets/ab817940-46e7-4d6c-9ee4-ba9f9f1cb882" />

4. エージェントにメールを送信します。件名には、`不具合報告`といれる必要があります。
>[!Note]
>デモ用に不具合報告を入れたときだけトリガーされるようにしていますが、トリガーに設定されているフローにて変更できます。

5. メールをインポートしたユーザーに対して送信します。以下の本文を参考に利用してください。シリアル番号はメモした内容を利用してください。

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
6. 暫く待つと、自動的にメールがエージェントにより作成され返信されます。
<img width="1256" height="997" alt="image" src="https://github.com/user-attachments/assets/16c4a5c6-b6d2-42c5-bc5a-3f1f8c6f9ff6" />

エージェントにより、このように自動的な判断と処理がなされています。
<img width="1186" height="885" alt="image" src="https://github.com/user-attachments/assets/5d9e2cc1-6a53-4872-b792-32c3eebf80a1" />

7. エージェントは同時に対応履歴を残してくれています。モデル駆動型アプリから対応履歴を確認することができます。
<img width="1908" height="911" alt="image" src="https://github.com/user-attachments/assets/99dc0cdc-e3ac-43a1-bde3-7325ae7cae25" />

### (2) 保証期間外の場合
販売日が1年以上前の受注を作成してそのシリアル番号でメールを送信した場合、どのような処理がなされるかを確認します。
1. 販売日が1年以上前の受注を作成します。
<img width="1492" height="627" alt="image" src="https://github.com/user-attachments/assets/870293f2-941f-4cc7-a0c6-8a61023469c4" />

2. シリアル番号を本文に入れ、不具合報告メールを送信します。
エージェントは以下のように処理を行います。
<img width="1187" height="880" alt="image" src="https://github.com/user-attachments/assets/45fc3be1-71c1-4963-8cf0-8175d7801c15" />


3. しばらくすると、エージェントから対応できない&サポート契約の提案を行うメール返信が行われます。
<img width="1218" height="828" alt="image" src="https://github.com/user-attachments/assets/cdb26df7-6752-44f4-ab69-b5232643fb6f" />

4. モデル駆動型アプリに対応履歴が記録されていることを確認します。
<img width="1902" height="942" alt="image" src="https://github.com/user-attachments/assets/b5992e69-d972-4480-ba8a-1a164dd7fa6b" />

---

