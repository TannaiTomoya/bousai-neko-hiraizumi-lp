# ぼうさいネコ＠平泉 ── 公式ページ（外部向け）

岩手県平泉町の消火栓を、猫の目の高さで見回る防災ゲーム「ぼうさいネコ＠平泉」の紹介ページ。

- このページ: https://tannaitomoya.github.io/bousai-neko-hiraizumi-lp/
- ゲーム本体: https://bousai-neko-hiraizumi.vercel.app/
- 構成: `index.html` 1枚（ビルド不要・GitHub Pages で直接配信）

## 関連リポジトリ

| リポジトリ | 役割 |
| --- | --- |
| kabumotohomare/bousai-neko-hiraizumi | ゲーム本体 |
| TannaiTomoya/bousai-neko-scenario | チーム内向け・シナリオ設計の共有 |
| **TannaiTomoya/bousai-neko-hiraizumi-lp** | 外部向け紹介・ゲームへの導線（このリポジトリ） |

## URL が変わったときにやること

`index.html` 末尾の設定ブロックだけを書き換えます。

```html
<script>
  window.BN_CONFIG = {
    gameUrl: 'https://bousai-neko-hiraizumi.vercel.app/', // 空にすると CTA が「準備中」表示に戻る
    reportUrl: ''                                         // 「ねこの もくげきほうこく」フォーム（空なら非表示）
  };
</script>
```

## 画面の作り

スクロール量が、そのままカメラの目の高さになります。ページを下へ読むと視点が
**人の目 150cm から猫の目 45cm まで降りていき**、人の高さでは見えなかった消火栓が
目の前に現れます。左端のゲージはその高さの実測値です。

着地点はゲームと同じ場所です。タキザワのスポーン地点から 47.7m 先にある
**泉屋75番地の消火栓**で、ゲームを始めたときに最初に向かうことになる消火栓です。

読み終えると視点は上空 820m へ上がり、東（橙）と西（藍）の縄張りが見えます。

## ゲーム本体と揃えている値

3D の見た目がゲームとずれないように、下の値はゲーム本体
`src/components/three/PatrolScene.tsx` と同じものを使っています。
**本体を変更したときは、こちらも確認してください。**

| 項目 | 値 | 本体の該当箇所 |
| --- | --- | --- |
| 座標系の原点 | 38.9899314, 141.1152492 | `game-config.json` の `defaultMapCenter` |
| 町モデルの配置 | scale (0.77726, **1**, 0.77726) / position (-64.378, 0.02, -64.754) | `townModelPlacement.ts`。**Y は拡大しない** |
| 猫の目の高さ | 0.45m | `CAMERA_HEIGHT_M` |
| 地面 / 道路 / 縄張りの輪の高さ | -0.08 / 0.025 / 0.07 | `GROUND_Y` / `ROAD_Y` / ring |
| 歩道の幅 | 片側 1.8m | `SIDEWALK_M` |
| 空の色 | `#a9d0f5` | `SKY_COLOR` |
| ライト | hemi `#e7f3ff`/`#6e7d5c` 1.2、sun `#fff6e4` 0.7 | 同ファイル |
| 消火栓 | 円柱 r0.2/0.24 h1.1、色 `#dc2626` | `createHydrantMarker` |
| 縄張りの色 | 東 `#ea580c` / 西 `#4f46e5` | `cats.json` の `territoryColor` |
| 地面と道路のテクスチャ | 同じ手順で canvas に描画 | `createGrassTexture` / `createRoadTexture` |
| 道路の帯 | 同じ miter 処理 | `toRoadRibbon.ts` |

消火栓の座標は `hydrants.json` の実データで、志羅山3番地には本体と同じ
建物めり込み補正（dx +0.5）をかけています。

## 変更しやすい定数（`index.html` の module スクリプト冒頭）

| 定数 | 意味 |
| --- | --- |
| `SPAWN` / `FIRST` | カメラの出発点と、最初に向かう消火栓 |
| `LAT` | 街路の中央へ寄せる横方向のずれ。スポーンのままだと右の壁まで2.9mしかない |
| `HYDRANTS` | タキザワの縄張りに入る消火栓4件の実座標 |
| `DESC` / `MAP` / `START` | カメラの道筋。`py` が目の高さ(m)、`f` が街路に沿った距離(m) |

## アセット

- `assets/models/hiraizumi-town.glb` ── 平泉の町（ゲーム本体の現行モデルを軽量化したもの。10.4MB → 1.8MB、テクスチャを1024px WebP 化。形状と座標は同一）
- `assets/data/roads.json` ── 道路データ（ゲーム本体と同じもの）
- `assets/screens/` ── 本番環境から撮影したゲーム画面
- `assets/characters/` ── タキザワ／シラヤマのイラスト

モデルを本体の最新版から作り直す場合:

```sh
npx @gltf-transform/cli resize in.glb t1.glb --width 1024 --height 1024
npx @gltf-transform/cli webp t1.glb t2.glb --quality 80
npx @gltf-transform/cli prune t2.glb assets/models/hiraizumi-town.glb
```

## 使用ライブラリ（すべて CDN）

- [GSAP 3](https://gsap.com/) + ScrollTrigger ── カメラの降下と地図への上昇（scrub でスクロールに追従）
- [three.js 0.160](https://threejs.org/) + GLTFLoader + BufferGeometryUtils ── 平泉の町
- Google Fonts ── Zen Old Mincho（地の文・見出し）/ Zen Maru Gothic（猫の台詞・UI）

## ローカル確認

```sh
python3 -m http.server 5500
# → http://localhost:5500/
```

`file://` では 3D モデルと importmap が読めないため、必ず HTTP サーバー経由で開いてください。

3D の読み込みに失敗した環境では、背景が静的なグラデーションになるだけで
本文とリンクはそのまま動きます。`prefers-reduced-motion` が有効な環境では、
カメラは連続した降下ではなくセクションごとの切り替えになります。
