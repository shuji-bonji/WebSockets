# WebSocket 学習カリキュラム（詳細版）

## 📚 カリキュラム概要
**総学習時間**: 約45-55時間  
**対象者**: TypeScript/JavaScript中級者、Svelte学習者、PWA・リアルタイム通信に興味がある開発者  
**最終目標**: WebSocketを使ったモダンなリアルタイムWebアプリケーション（PWA対応）の設計・実装ができる

**主要技術スタック**: 
- フロントエンド: Svelte/SvelteKit + TypeScript
- バックエンド: Node.js + TypeScript
- テスト: Vitest + Playwright
- PWA: Service Worker + WebSocket統合


## 1. WebSocket 入門（学習時間: 3-4時間）

### 1.1 WebSocketとは何か（1時間）
- **学習内容**
  - WebSocketの定義と基本概念
  - リアルタイム通信の必要性
  - WebSocketの歴史と標準化
- **演習**
  - ブラウザ開発者ツールでWebSocket通信を観察
  - 簡単なWebSocketテストサイトでの動作確認

### 1.2 HTTPの限界とWebSocketの優位性（1時間）
- **学習内容**
  - HTTPリクエスト/レスポンス型の制限
  - ポーリング手法の問題点（定期ポーリング、ロングポーリング）
  - WebSocketによる双方向通信の実現
- **演習**
  - ポーリングとWebSocketのパフォーマンス比較
  - ネットワークトラフィックの測定と分析

### 1.3 WebSocketの利用例と適用場面（1-2時間）
- **学習内容**
  - **適している場面**: チャットアプリ、株価表示、オンラインゲーム、コラボ編集
  - **適さない場面**: 静的コンテンツ配信、一方向データ取得
  - 実際のサービス事例研究
- **演習**
  - 既存WebSocketアプリの通信パターン分析
  - 自分のプロジェクトでの適用可能性検討


## 2. WebSocket を取り巻くネットワーク技術（学習時間: 5-6時間）

### 2.1 OSI参照モデルとWebSocketの位置（1時間）
- **学習内容**
  - OSI 7層モデルの復習
  - WebSocket（セッション層）とSocket（トランスポート層）の違い
  - HTTPからWebSocketへのプロトコル昇格
- **演習**
  - Wiresharkを使ったパケット解析
  - WebSocketハンドシェイクの詳細観察

### 2.2 HTTP/1.1からHTTP/3までのWebSocket対応（2時間）
- **学習内容**
  - **HTTP/1.1**: 従来のWebSocketハンドシェイク
  - **HTTP/2**: WebSocketとの関係、HTTP/2 over WebSocketの制限
  - **HTTP/3 (QUIC)**: WebSocketの将来性、WebTransport APIとの比較
  - 各バージョンでのパフォーマンス特性
- **演習**
  - 各HTTPバージョンでのWebSocket接続テスト
  - パフォーマンス比較測定

### 2.3 WebSocket接続確立プロセス（1-2時間）
- **学習内容**
  - HTTP/1.1 Upgradeハンドシェイクの詳細
  - WebSocketキーの生成と検証
  - プロトコルネゴシエーション
- **演習**
  - `curl`コマンドでWebSocketハンドシェイクを手動実行
  - ハンドシェイクヘッダーの検証実装

### 2.4 セキュリティとポート管理（1時間）
- **学習内容**
  - ws（ポート80）とwss（ポート443）の違い
  - Originチェックとセキュリティ
  - ファイアウォール・プロキシとの関係
  - CORSとの違い
- **演習**
  - wss接続の設定と証明書管理
  - Originチェックの実装


## 3. WebSocket の基本構造と仕組み（学習時間: 5-6時間）

### 3.1 WebSocket接続ライフサイクル（2時間）
- **学習内容**
  - 接続確立（ハンドシェイク）
  - データ通信フェーズ
  - 接続終了（クローズハンドシェイク）
  - 異常切断の検出と処理
- **演習**
  - TypeScriptでWebSocket接続状態の管理実装
  - 接続品質の監視システム作成

### 3.2 イベントベース通信モデル（2時間）
- **学習内容**
  - `onopen`, `onmessage`, `onclose`, `onerror`イベント
  - Svelteのリアクティブシステムとの統合
  - Svelteストア（Writable, Readable）でのWebSocket管理
  - Promise/async-awaitでのラッピング
- **演習**
  - SvelteストアでWebSocketをリアクティブに管理
  - TypeScriptでタイプセーフなWebSocketクライアント作成

### 3.3 クライアント・サーバー役割分担（1-2時間）
- **学習内容**
  - クライアント側の責務
  - サーバー側の責務
  - 状態管理とセッション管理
- **演習**
  - Node.js + TypeScriptでWebSocketサーバー実装
  - 複数クライアント接続管理


## 4. WebSocket 接続の開始と基本操作（学習時間: 4-5時間）

### 4.1 WebSocket URL と接続確立（1時間）
- **学習内容**
  - WebSocket URL形式（`ws://`, `wss://`）
  - サブプロトコルの指定
  - 接続オプションとヘッダー
- **演習**
  - 環境別（開発・本番）の接続設定
  - 接続パラメータの動的生成

### 4.2 Svelte/SvelteKit実装（2-3時間）
- **学習内容**
  - ブラウザ標準WebSocket APIの使用
  - SvelteKitでのクライアント側WebSocket処理
  - Svelteコンポーネントでのリアルタイムデータ表示
  - TypeScriptでの型定義とSvelteでの利用
- **演習**
  ```typescript
  // Svelteストア形式のWebSocketクライアント
  import { writable, type Writable } from 'svelte/store';
  
  interface WebSocketStore {
    connected: boolean;
    error: string | null;
    data: any[];
  }
  
  export function createWebSocketStore(url: string): {
    subscribe: Writable<WebSocketStore>['subscribe'];
    connect: () => void;
    send: <T>(data: T) => void;
    disconnect: () => void;
  } {
    // 実装演習
  }
  ```

### 4.3 接続失敗と再接続処理（1-2時間）
- **学習内容**
  - 接続失敗の種類と原因
  - 指数バックオフによる再接続
  - 接続品質の監視
- **演習**
  - 堅牢な再接続ロジックの実装
  - 接続状態インジケーターの作成


## 5. WebSocket API の使用（学習時間: 6-7時間）

### 5.1 WebSocketオブジェクトとSvelteストア（2-3時間）
- **学習内容**
  - `readyState`プロパティ（CONNECTING, OPEN, CLOSING, CLOSED）
  - `send()`メソッドの使い方
  - `close()`メソッドとコード/理由
  - SvelteストアでのWebSocket状態管理
  - リアクティブな接続状態表示
- **演習**
  - WebSocket状態管理Svelteストアの実装
  - カスタムWebSocketイベントシステムの構築
  ```svelte
  <!-- WebSocket接続状態表示コンポーネント -->
  <script lang="ts">
    import { webSocketStore } from '$lib/stores/websocket';
    
    $: status = $webSocketStore.connected ? '接続中' : '切断中';
  </script>
  
  <div class="status {$webSocketStore.connected ? 'connected' : 'disconnected'}">
    {status}
  </div>
  ```

### 5.2 データ送受信パターン（2時間）
- **学習内容**
  - 文字列データの送受信
  - JSONデータのシリアライゼーション
  - メッセージ形式の設計（type, payload パターン）
- **演習**
  ```typescript
  interface WebSocketMessage<T = any> {
    type: string;
    id?: string;
    timestamp: number;
    payload: T;
  }
  
  // メッセージハンドラーの実装
  class MessageHandler {
    handle<T>(message: WebSocketMessage<T>): void {
      // 実装演習
    }
  }
  ```

### 5.3 高度なエラーハンドリング（2-3時間）
- **学習内容**
  - ネットワークエラーの分類
  - メッセージ配送保証の実装
  - タイムアウト処理
- **演習**
  - メッセージACKシステムの実装
  - 配送失敗時の再送機能
  - ハートビート機能の実装


## 6. WebSocket データフレームの仕組み（学習時間: 3-4時間）

### 6.1 WebSocketフレーム構造（1-2時間）
- **学習内容**
  - フレームヘッダーの詳細（FIN, RSV, opcode）
  - ペイロード長の表現方法
  - マスキングキーの仕組み
- **演習**
  - フレーム解析ツールの作成
  - バイナリデータの手動パース

### 6.2 バイナリデータ送信（1-2時間）
- **学習内容**
  - `ArrayBuffer`, `Blob`, `Uint8Array`の使い分け
  - バイナリデータの効率的な送信
  - ファイルアップロードの実装
- **演習**
  - 画像データのリアルタイム送信
  - プロトコルバッファの実装


## 7. WebSocket 高度なトピック（学習時間: 10-12時間）

### 7.1 セキュリティ実装（3-4時間）
- **学習内容**
  - wss（WebSocket Secure）の設定
  - JWT認証トークンの統合
  - CSRF攻撃の対策
  - Rate limitingの実装
- **演習**
  ```typescript
  // 認証付きWebSocket接続
  class AuthenticatedWebSocket extends WebSocketClient {
    constructor(private authToken: string) {
      super();
    }
    
    protected createConnection(url: string): WebSocket {
      // Authorization実装演習
    }
  }
  ```

### 7.2 PWAとWebSocketの統合（3-4時間）
- **学習内容**
  - Service WorkerとWebSocketの関係
  - オフライン時のメッセージキューイング
  - Push通知とWebSocketの使い分け
  - Background Syncでの再接続処理
  - PWAライフサイクルとWebSocket管理
- **演習**
  ```typescript
  // Service Worker内でのWebSocketメッセージ管理
  // sw.js
  self.addEventListener('message', (event) => {
    if (event.data.type === 'WEBSOCKET_MESSAGE') {
      // オフライン時のメッセージ保存演習
    }
  });
  
  // SvelteKitでのPWA統合
  // app.html でのService Worker登録
  // WebSocketとPush通知の協調動作
  ```
  - SvelteKitでのPWA設定（@vite-pwa/sveltekit使用）
  - オフライン対応WebSocketアプリの構築

### 7.3 スケーラビリティと負荷対策（2-3時間）
- **学習内容**
  - 水平スケーリングの課題
  - Redis Pub/Subによるメッセージ配信
  - ロードバランサー設定
  - 接続数監視とリソース管理
- **演習**
  - Socket.IO + Redisクラスター構築
  - 接続負荷テストの実施

### 7.4 プロトコル拡張とパターン（2時間）
- **学習内容**
  - Pub/Subパターンの実装
  - RPC風APIの設計
  - Room/Namespaceの概念
- **演習**
  - カスタムプロトコルの設計・実装
  - メッセージルーティングシステム


## 8. テスト手法（学習時間: 5-6時間）

### 8.1 クライアント側テスト（2-3時間）
- **学習内容**
  - Vitest + @vitest/ui を使用したユニットテスト
  - WebSocketのモック化
  - Svelteコンポーネントのテスト（@testing-library/svelte）
  - 非同期処理のテスト
- **演習**
  ```typescript
  // WebSocketストアのテスト（Vitest）
  import { describe, it, expect, vi } from 'vitest';
  import { createWebSocketStore } from '$lib/stores/websocket';
  
  describe('WebSocketStore', () => {
    it('should connect and send message', async () => {
      // WebSocketモック作成
      const mockWS = vi.fn();
      global.WebSocket = mockWS;
      
      // テスト実装演習
    });
  });
  ```
  - Svelteコンポーネントでのリアルタイムデータ表示テスト
  - WebSocketストアの状態遷移テスト

### 8.2 サーバー側テスト（1-2時間）
- **学習内容**
  - VitestでのNode.js WebSocketサーバーテスト
  - 接続管理のテスト
  - メッセージハンドリングのテスト
- **演習**
  - Node.js WebSocketサーバーのテストスイート作成
  - クライアント・サーバー統合テスト

### 8.3 E2Eテスト（1-2時間）
- **学習内容**
  - PlaywrightでのSvelteKitアプリテスト
  - WebSocket通信のE2Eテスト
  - PWA機能のテスト
- **演習**
  - リアルタイムチャット機能のE2Eテスト実装
  - オフライン・オンライン切り替えテスト


## 9. 他技術との比較・使い分け（学習時間: 3-4時間）

### 9.1 代替技術の比較（2時間）
- **学習内容**
  - Server-Sent Events (SSE) との比較
  - Long Polling との違い
  - GraphQL Subscriptions との関係
- **演習**
  - 各技術のパフォーマンス比較実験
  - 使い分けガイドライン作成

### 9.2 適用判断基準（1-2時間）
- **学習内容**
  - WebSocketが適している場面・適さない場面
  - コスト・複雑さとのトレードオフ
  - 代替案の提示方法
- **演習**
  - 技術選定マトリックスの作成
  - 既存プロジェクトへの適用可能性評価


## 10. 実践演習プロジェクト（学習時間: 12-18時間）

### 10.1 基礎プロジェクト: PWA対応リアルタイムチャット（6-8時間）
- **機能要件**
  - ユーザー認証（JWT）
  - リアルタイムメッセージ送受信
  - オンラインユーザー表示
  - メッセージ履歴
  - **PWA機能**: オフライン対応、プッシュ通知、アプリインストール
- **技術スタック**
  - フロントエンド: SvelteKit + TypeScript + PWA
  - バックエンド: Node.js + TypeScript + Socket.IO
  - データベース: MongoDB/PostgreSQL
  - PWA: @vite-pwa/sveltekit + Service Worker
- **実装ポイント**
  ```typescript
  // SvelteKitでのSSR + WebSocket統合
  // src/routes/chat/+page.svelte
  <script lang="ts">
    import { onMount } from 'svelte';
    import { browser } from '$app/environment';
    import { chatStore } from '$lib/stores/chat';
    
    onMount(() => {
      if (browser) {
        chatStore.connect();
      }
    });
  </script>
  ```

### 10.2 応用プロジェクト: リアルタイム共同編集システム（6-10時間）
- **機能要件**
  - リアルタイム文書編集
  - カーソル位置共有
  - 変更履歴管理
  - 競合解決
  - **PWA機能**: オフライン編集、同期復旧
- **技術的チャレンジ**
  - Operational Transform/CRDTsの実装
  - 大量データの効率的同期
  - SvelteKitでのSSR + クライアント側リアルタイム処理
  - Service Workerでのオフライン対応


## 📋 学習リソース・参考資料

### 公式ドキュメント
- [RFC 6455 - WebSocket Protocol](https://tools.ietf.org/html/rfc6455)
- [MDN WebSocket API](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket)
- [SvelteKit Documentation](https://kit.svelte.dev/docs)
- [Vitest Documentation](https://vitest.dev/)

### 推奨ライブラリ
- **フロントエンド**: SvelteKit, Socket.IO-client, ws
- **バックエンド**: Socket.IO, ws (Node.js)
- **テスト**: Vitest, @testing-library/svelte, Playwright
- **PWA**: @vite-pwa/sveltekit, workbox

### PWA・WebSocket統合リソース
- [PWA WebSocket Background Sync](https://web.dev/background-sync/)
- [Service Worker WebSocket Integration](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [SvelteKit PWA Guide](https://vite-pwa-org.netlify.app/frameworks/sveltekit.html)

### 開発ツール
- **デバッグ**: ブラウザ開発者ツール、Wireshark
- **テスト**: Postman, WebSocket King, Vitest UI
- **監視**: Socket.IO Admin UI
- **PWA**: Lighthouse, PWA Builder


## 🎯 習得目標チェックリスト

- [ ] WebSocketの基本概念と適用場面を説明できる
- [ ] HTTP/1.1、HTTP/2、HTTP/3でのWebSocket対応の違いを理解している
- [ ] TypeScriptでWebSocketクライアントを実装できる
- [ ] SvelteKitでリアルタイムWebアプリケーションを構築できる
- [ ] Svelteストアを使ったWebSocket状態管理ができる
- [ ] Node.jsでWebSocketサーバーを構築できる
- [ ] PWAとWebSocketを統合したアプリケーションを開発できる
- [ ] Service WorkerでのオフラインWebSocket対応ができる
- [ ] 認証・セキュリティを考慮した実装ができる
- [ ] スケーラブルなWebSocketアプリケーションを設計できる
- [ ] Vitestを使った適切なテスト戦略を立てて実装できる
- [ ] 他技術との使い分けを判断できる
- [ ] プロダクションレベルのPWA + WebSocketアプリを構築できる