# ハモリアプリ 競合・需要調査（2026-11-22）

サブエージェントによるWeb調査の結果です。各サイトを直接開けず、検索結果の要約をもとにしているため、価格や機能の細部は公式ページで要確認。【未確認】は一次情報で確認できなかったもの。

## 1. 既存のアプリ・サービス

### A. 自分の歌から、AIがハモリを作る（中心機能に近い）

| サービス | 内容 | 場所 | 価格 | 近さ | 1曲まるごと | 歌詞 | 別の声 | 即時/後から |
|---|---|---|---|---|---|---|---|---|
| Kits.ai Harmony Generator | 歌の録音から最大4声のハモリ。キーは自分で指定 | Web | 有料Starter（月約$10）以上。ダウンロード月15分まで | 非常に高い | 【未確認】 | ○ | ○（75〜150種以上のAIの声） | 後から |
| Wondera | 主旋律を分析してハモリ・デュエット用の歌声を作る | Web（iPhoneアプリは機能【未確認】） | 無料で月10回【公式の宣伝文】 | 高い | 【未確認】 | ○【未確認】 | ○ | 後から |
| Audimee | 1つの歌から最大5本のハモリを別々のAIの声で | Web | 月$9から【第三者情報】 | 高い | ×（45秒未満） | ○ | ○ | 後から |
| TopMediai | 2〜4声のハモリ | Web | 週$4.99からのクレジット制【未確認】 | 中〜高 | 【未確認】 | ○ | ○ | 後から |
| MIT「AI Harmonizer」（2025年の研究） | 歌→音符→AIが4声ハモリを作曲→音程変更→RVCで声変換。**仕組みはアイデアとほぼ同じ** | 研究（GitHub公開） | 無料 | 製品ではない | 不明 | ○ | ○ | 後から |

### B. スマホのハーモナイザー（その場で声にハモリ効果）
- Voloco（iOS、無料＋定額）、MeloVox（iOS、$14.99買い切り、最大5声）、Solo Harmony（iOS、€0.99程度）。声は「自分の声の重ね」で、曲を理解したハモリではない。
- ezMixRecorder／nana：自分で重ね録り（AIではない）。nana Premiumは月580円。

### C. 制作者向けプラグイン（パソコン用）
Antares Harmony Engine、Waves Harmony、iZotope Nectar 4、Sonarworks SoundID VoiceAI、Cubase、ABILITY Pro など。音楽知識が必要で、気軽に歌う人向けではない。

### D. カラオケアプリ
Smule（デュエット・合唱）。ポケカラ／DAM／JOYSOUND にハモリパートのガイド機能は確認できず【未確認】。YouTubeのハモリ練習チャンネルは曲ごとに人が作っている。

## 2. 空白（差別化の余地）

- 技術の中心部分（一度歌えば、歌詞付きのハモリを別の声で返す）は **すでにある**（Kits.ai、Wondera、Audimee など。海外のWebサービス）。
- 次の組み合わせを満たすものは見つからなかった:
  1. 気軽に歌う人向けのスマホアプリ
  2. アカペラ1曲分をそのまま扱える
  3. キーの自動判定
  4. 声で直しを頼める
  5. 日本語・J-POP前提
  6. ハモリと一緒に歌う→SNS投稿までが一つの流れ
- → 差別化は技術ではなく **「誰向けに、どんな体験を作るか」**。大手がスマホ版を出せば追いつかれるリスクあり。

### 今すぐ試せるもの
1. Kits.ai Harmony Generator（有料）
2. Wondera（無料枠）
3. Audimee（45秒未満のサビで）
4. 比較用に Voloco

## 3. 需要の手がかり

- カラオケ参加人口: 2024年 約4,070万人（レジャー白書2025の二次情報）。市場規模 2024年度 約3,200億円（帝国データバンク）。
- ハモリ動画は1本数千〜数万回再生程度。**根強いが小さめの需要**。
- 「自動でハモリを作るアプリはないか」という質問は存在。検索数は【未確認】。
- 「AIカバー」アプリは多く、別の声で歌わせることには需要がある。

## 4. SNS投稿と著作権（簡易）

- YouTube: JASRACと包括契約あり。NexToneも管理。
- TikTok／Instagram: JASRAC許諾ありとして紹介されている【公式一覧は未確認】。
- X: 許諾なし（JASRACが2026年4月に注意喚起）。YouTubeのURLを貼る方法が推奨されている。
- 包括契約の対象は「歌詞と曲」のみ。CD音源（原盤）は対象外 → アカペラなら有利。
- AIのハモリが「編曲」に当たるかは【未確認。専門家に確認】。
- 実在の歌手に似せた声は、パブリシティ権の問題の可能性。

## 主な出典
- Kits.ai: https://kits.ai/pricing / https://kits.ai/blog/kits-ai-harmony-generator / https://help.kits.ai/hc/en-us/articles/33741478546195-How-do-I-use-the-Harmony-Generator
- Wondera: https://wondera.ai/tools/en/AI-harmonies-duet
- Audimee: https://audimee.com/tools/harmony
- TopMediai: https://www.topmediai.com/create-music/harmony-generator/
- MIT AI Harmonizer: https://nime.org/proc/nime2025_84/ / https://arxiv.org/html/2506.18143v1
- Sonarworks: https://www.sonarworks.com/blog/product-news/soundid-voiceai-unison-mode
- Voloco: https://apps.apple.com/gb/app/voloco/id1052970183 / MeloVox: https://apps.apple.com/us/app/-/id6544800478
- カラオケ人口・市場: https://www.travelvoice.jp/20251030-158645 / https://kyodonewsprwire.jp/release/202510147079 / https://karaoke.or.jp/05hakusyo/2024/p1.php
- 著作権: https://blog.youtube/intl/ja-jp/news-and-events/2023_02_youtube-content-id-jasrac / https://www.famitsu.com/article/202605/74167 / https://nex-tone.co.jp/DM_FAQ/index_en.html
