# お父さんポイント

家族で「お父さんポイント」を加点・減点・ごほうび交換できる、単一HTMLのスマホ向けアプリです。

## 使い方

`index.html` をブラウザで開くだけです。

## 機能

- リン / ミク / OKS を切り替えてポイント管理
- 家事、送迎、勉強を見る、ごはん作りなどをワンタップ加点
- 寝落ち、スマホ見すぎ、約束忘れなどの減点
- 自由な理由と点数で登録
- ごほうび交換
- 履歴表示
- 名前変更
- localStorage保存
- Supabase外部DB保存
- JSONバックアップ保存

## GitHub Pages

リポジトリの Settings → Pages で Branch を `main`、Folder を `/root` にすると公開できます。

公開URLの例：

`https://kouheim1979.github.io/ots-points/`

## Supabase設定

Supabaseで新規プロジェクトを作成し、SQL Editorで以下を実行してください。

```sql
create table if not exists dad_points_logs (
  id uuid primary key default gen_random_uuid(),
  member text not null,
  reason text not null,
  points integer not null,
  who text not null default '家族',
  created_at timestamptz not null default now()
);

create table if not exists dad_points_rewards (
  id uuid primary key default gen_random_uuid(),
  name text not null,
  cost integer not null,
  created_at timestamptz not null default now()
);

alter table dad_points_logs enable row level security;
alter table dad_points_rewards enable row level security;

create policy "read logs" on dad_points_logs for select using (true);
create policy "insert logs" on dad_points_logs for insert with check (true);
create policy "read rewards" on dad_points_rewards for select using (true);
create policy "insert rewards" on dad_points_rewards for insert with check (true);
```

アプリの「設定」タブに以下を入力します。

- Project URL
- anon public key

注意：`service_role key` は絶対に入れないでください。公開アプリでは `anon public key` だけを使います。
