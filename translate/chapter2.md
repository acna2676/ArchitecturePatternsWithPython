## Persisting Our Domain Model
第 1 章では、注文を在庫のバッチに割り当てることができる単純なドメイン モデルを構築しました。セットアップする依存関係やインフラストラクチャがないため、このコードに対するテストを書くのは簡単です。データベースや API を実行してテスト データを作成する必要がある場合、テストの記述と保守が難しくなります。
悲しいことに、ある時点で、完璧な小さなモデルをユーザーの手に渡して、スプレッドシートや Web ブラウザー、競合状態の現実の世界と戦う必要があります。次の数章では、理想化されたドメイン モデルを外部状態に接続する方法を見ていきます。
私たちは機敏な方法で作業することを期待しているため、最小限の実行可能な製品にできるだけ早く到達することが優先されます。私たちの場合、それは Web API になります。実際のプロジェクトでは、いくつかのエンド ツー エンドのテストに直接飛び込んで、Web フレームワークのプラグインを開始し、アウトサイド インでテストを実行することがあります。
しかし、何があっても永続的なストレージが必要になることはわかっています。これは教科書なので、ボトムアップの開発をもう少し許可して、ストレージとデータベースについて考え始めることができます。 .
## Some Pseudocode: What Are We Going to Need?
データベースからバッチ情報を取得して、そこからドメインモデルオブジェクトをインスタンス化する方法が必要だし、それをデータベースに保存する方法も必要だ。
え？gubbins "は英国語で "物 "という意味です。それは無視してください。pseu-docodeです、OK?

## What Is a Port and What Is an Adapter, in Python?
ここで主に焦点を当てたいのは依存関係の逆転であり、使用する手法の詳細はあまり重要ではないため、ここでは用語についてあまり詳しく説明したくありません。また、人によって若干異なる定義が使用されていることも承知しています。
  
ポートとアダプターは OO の世界から生まれました。私たちが保持している定義は、ポートはアプリケーションと抽象化したいものとの間のインターフェースであり、アダプターはそのインターフェースまたは抽象化の背後にある実装です。
現在、Python にはインターフェイス自体がないため、通常はアダプターを識別するのは簡単ですが、ポートを定義するのは難しい場合があります。抽象基本クラスを使用している場合、それがポートです。そうでない場合、ポートは、アダプタが準拠し、コア アプリケーションが期待するダック型 (使用中の関数名とメソッド名、およびそれらの引数名と型) にすぎません。
具体的には、この章では、  AbstractRepository がポートで、SqlAlchemyRepository と FakeRepository がアダプタです。