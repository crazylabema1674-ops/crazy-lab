# ハモリアプリ 技術調査（2026-11-22）

サブエージェントによるWeb調査の結果です。【未確認】は一次情報で確かめられなかった点、【推定】は見積もり。為替は1ドル＝150円。

## 0. 結論

- 全部作る場合: ★★★★☆（かなり難しい）。特に「2回目に歌うのに合わせてハモリを流す（テンポ追従）」は★5で研究レベル。
- 小さく作る場合（1回目の歌＋自動ハモリ（別の歌手の声）を重ねて聴く）: ★★★☆☆。部品はほぼオープンソースで揃っている。
- 月300円の採算: 必要な時だけ動くGPUサーバー＋RVCなら1曲約1〜15円【推定】で条件付きで成立。外部API（ElevenLabs）は1曲約54円で赤字。
- 最大の注意点は技術より **権利**（変換先の歌手の書面同意、モデルのライセンス、J-POPの著作権）。

## 1. 処理の流れ

録音 → 音程とタイミングを抽出 → 調（キー）を推定しハモリの音を決める → ハモリの音程＋別の歌手の声に変換 → 再生

工夫: 歌声変換モデル（RVC等）は「発音」と「音程の線」を別々に受け取るので、**ユーザーの音程の線を3度上にずらして直接渡せば、音程変換と声の変換が1回で済む**（音質劣化を避けやすい）。音ごとに半音3つ/4つを切り替える追加プログラムは必要だが小規模。

## 2. 部品の候補

| 工程 | おすすめ | 備考 |
|---|---|---|
| 音程抽出 | RMVPE（サーバー側）／YIN系（スマホの音程表示用） | RMVPEは伴奏なしの歌声で正解率97.3%（論文著者の評価）。CREPE、pYIN、Basic Pitch も候補 |
| 調の推定・ハモリ音 | ルール方式（音の分布から調を推定 → 音階で3度上） | music21、librosa が使える。フレーズごとに調を判定して転調に対応。高すぎる所は下ハモリに切替。「サビは高く」もルール切替で実現 |
| 音程変換（単体） | WORLD/pyworld、PSOLA | Rubber BandはGPLで、アプリ組み込みには有料ライセンス。歌声変換に音程を直接渡すなら省略可 |
| 歌声変換 | **RVC v2** | コードMIT。土台モデルは公開データで学習済み。変換先の歌手の声を10〜30分録音して追加学習【推定】。安価なGPU（T4）で数十秒【推定】 |
| 〃（比較候補） | Seed-VC、YingMusic-SVC、HQ-SVC、R2-SVC | Seed-VCはGPL-3.0・更新停止。新モデルは研究段階で利用条件【未確認】 |
| 外部API | ElevenLabs Voice Changer | 約0.12ドル/分。歌の品質は未検証。Kits.ai のアーティスト声は原則個人の非商用のみ |
| 歌声合成（別案） | 不採用 | 歌詞の文字起こしが必要で間違えやすく、歌い方の揺れも失われる |

## 3. 構成と1曲（3分）あたりのコスト【推定】

構成: スマホ単体は非現実的。**必要な時だけ動くGPUサーバー（Modal、RunPod）を推奨**。Modalは毎月30ドル分の無料枠あり。常時起動は月約8.6万円で個人には不可。

| ケース | 1回あたり |
|---|---|
| 楽観的（RVC＋T4、40秒） | 約1円 |
| 標準的（RVC＋L4、120秒） | 約4円 |
| 悲観的（Seed-VC系＋A10G、300秒） | 約14円 |
| 外部API（ElevenLabs） | 約54円 |

月300円（手取り約210〜255円）、1曲あたり平均2回生成と仮定:

| 1ユーザー・1か月 | 楽観的 | 標準的 | 悲観的 | 外部API |
|---|---|---|---|---|
| 10曲（20回） | 20円 ◎ | 80円 ○ | 280円 ✕ | 1,080円 ✕ |
| 30曲（60回） | 60円 ◎ | 240円 △ | 840円 ✕ | 3,240円 ✕ |

成り立たせる条件: 軽いモデル（RVC）、作り直しは音程の線だけ変えて再変換、有料プランにも回数上限（例: 月20回）、無料は1日1曲・30秒まで。

## 4. 実際の利用場面での課題

| 課題 | 対策【推定を含む】 |
|---|---|
| お風呂・車・台所の反響や雑音 | 雑音除去を前処理に。MVPは「静かな部屋で」と案内 |
| 歌いながら再生すると音が混ざる | イヤホン必須。Bluetoothは遅れ（0.1〜0.3秒程度）が大きいので有線推奨 |
| 2回目の歌とハモリを合わせる | **一番難しい（★5）**。MVPは「1回目の歌＋ハモリの二重唱を聴く」だけ、または1回目の自分の歌をガイドとして小さく流して合わせてもらう |
| 声の指示 | スマホ標準の音声認識＋決まった言い回し、または安価な言語AI。技術的には容易 |
| 著作権 | 個人で楽しむ範囲の録音・加工は問題になりにくいと考えられる【推定】。共有・公開機能、歌詞表示、原曲音源の使用は許諾が必要な可能性。公開前に専門家へ確認 |
| 声の権利 | 日本俳優連合のガイドライン（2025年10月）は「学習」「モデル作成」「生成物の利用」の3段階それぞれで書面同意を求める。歌手を雇い契約書に明記。有名歌手に似せない。Synthesizer Vの声は他アプリへの組み込み不可の可能性 |

## 5. おすすめの試作の進め方

1. **ステップ0（プログラムなし・半日〜1日・ほぼ0円）**: スマホで1曲録音 → Audacityで+4半音 → Replicate上のRVCで声を変換 → 元の歌と重ねて、友人5人に「聴きたいか」を聞く。
2. **ステップ1**: Google Colabで処理の流れを作る（Claude Codeに作ってもらう）。お風呂・車・台所で録った10曲で音の外れ率と処理秒数を測る。
3. **ステップ2**: Gradioで試用版をWeb公開（Modal / Hugging Face Spaces）。10人に使ってもらい、実際のGPU秒数を測る。
4. **ステップ3**: 歌手を雇って録音し、3段階の同意を含む契約を結んでRVCに学習させる（数万円程度【推定】）。
5. **ステップ4**: 品質に納得できたら、Expoでスマホアプリ化。最初は「録音 → アップロード → 二重唱を再生」だけ。

## 6. 主なリスク（重要度順）

1. 権利（声の同意、J-POP著作権、GPLライセンス、学習済みモデルの利用条件）
2. 品質（素人の音程のずれ・反響で、ハモリが外れる／ロボット声になる）
3. 2回目の歌との同期
4. コスト（重いモデル・作り直し過多で赤字）
5. 運用の手間（サーバー待ち時間、障害対応、アプリ審査）

## 主な出典
- RMVPE: https://arxiv.org/abs/2306.15412
- RVC: https://github.com/RVC-Project/Retrieval-based-Voice-Conversion-WebUI
- Seed-VC: https://github.com/Plachtaa/seed-vc
- YingMusic-SVC: https://arxiv.org/html/2512.04793v1 / HQ-SVC: https://arxiv.org/pdf/2511.08496 / R2-SVC: https://arxiv.org/html/2510.20677v1
- WORLD: https://github.com/mmorise/World / Rubber Band: https://breakfastquay.com/rubberband/license.html
- ElevenLabs: https://help.elevenlabs.io/hc/en-us/articles/24938328105873-How-much-does-Voice-Changer-cost
- Kits.ai: https://kits.ai/blog/voice-artist-library-faq
- Replicate RVC: https://replicate.com/zsxkib/realistic-voice-cloning
- Modal: https://modal.com/blog/nvidia-t4-price-article / https://modal.com/blog/nvidia-l4-price-article
- RunPod: https://docs.runpod.io/serverless/pricing
- Synthesizer V 規約: https://dreamtonics.com/terms
- 声の権利: https://www.moj.go.jp/content/001463284.pdf / https://www.dir.co.jp/report/research/economics/japan/20251217_025474.pdf
- Apple エコー除去: https://developer.apple.com/tutorials/data/documentation/avfaudio/avaudiosession/mode-swift.struct/voicechat.md
