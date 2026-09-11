# ぼうさいネコ＠平泉 ── 公式ページ（外部向け）

岩手県平泉町の消火栓を、猫の目で見回る防災ゲーム「ぼうさいネコ＠平泉」の紹介ページ。
ゲーム公開までは **Coming Soon**、公開後はこのページからそのまま遊べる導線になります。

- 公開 URL: https://tannaitomoya.github.io/bousai-neko-hiraizumi-lp/
- 構成: `index.html` 1枚（ビルド不要・GitHub Pages で直接配信）

## 関連リポジトリ

| リポジトリ | 役割 |
| --- | --- |
| kabumotohomare/bousai-neko-hiraizumi | ゲーム本体 |
| TannaiTomoya/bousai-neko-scenario | チーム内向け・シナリオ設計の共有 |
| **TannaiTomoya/bousai-neko-hiraizumi-lp** | 外部向け紹介・ゲームへの導線（このリポジトリ） |

## ゲーム公開時にやること

`index.html` 末尾の設定ブロックだけを書き換えます。

```html
<script>
  window.BN_CONFIG = {
    gameUrl: 'https://…',   // ゲーム本体の URL → CTA が「PLAY」に切り替わる
    reportUrl: 'https://…'  // 「ねこの もくげきほうこく」フォーム（空なら非表示）
  };
</script>
```

あわせて `assets/screens/` の画面画像を完成版に差し替え、`04 SCREENS` のキャプションを調整してください。

## 使用ライブラリ（すべて CDN）

- [GSAP 3](https://gsap.com/) + ScrollTrigger ── スクロール演出・夜→朝の背景遷移
- [Swiper 11](https://swiperjs.com/) ── 画面カルーセル
- [three.js 0.160](https://threejs.org/) + GLTFLoader ── ヒーロー背景（平泉の町 3D モデル）
- Google Fonts ── Zen Kaku Gothic New / Space Grotesk

## アセット

- `assets/characters/` ── タキザワ／シラヤマのキャラクターイラスト
- `assets/screens/` ── 開発中のゲーム画面（390×844 @2x）
- `assets/models/hiraizumi-town.glb` ── 平泉の町 3D モデル（ゲーム本体と共通）

## ローカル確認

```sh
python3 -m http.server 5500
# → http://localhost:5500/
```

`file://` では 3D モデルと importmap が読めないため、必ず HTTP サーバー経由で開いてください。
