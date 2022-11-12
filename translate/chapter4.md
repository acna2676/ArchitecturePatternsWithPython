## Service Layer
この章では、オーケストレーション ロジック、ビジネス ロジック、インターフェイス コードの違いについて説明し、ワークフローのオーケストレーションとシステムのユース ケースの定義を処理するサービス層パターンを紹介します。
テストについても説明します。サービス レイヤーをデータベース上のリポジトリ抽象化と組み合わせることで、ドメイン モデルだけでなく、ユース ケースのワークフロー全体の高速テストを作成できます。
図 4-2 は、私たちが目指しているものを示しています。ドメイン モデルへのエントリポイントとして機能するサービス層と通信する Flask API を追加します。 私たちのサービス層は AbstractRepository に依存しているため、FakeRepository を使用して単体テストを実行できますが、SqlAlchemyRepository を使用して本番コードを実行できます。
## Connecting Our Application to the Real World
Like any good agile team, we’re hustling to try to get an MVP out and in front of the users to start gathering feedback. We have the core of our domain model and the domain ser- vice we need to allocate orders, and we have the repository interface for permanent storage.
Let’s plug all the moving parts together as quickly as we can and then refactor toward a cleaner architecture. Here’s our plan:
1. Use Flask to put an API endpoint in front of our allocate domain service. Wire up the database session and our repository. Test it with an end-to-end test and some quick-and-dirty SQL to prepare test data.
2. Refactor out a service layer that can serve as an abstraction to capture the use case and that will sit between Flask and our domain model. Build some service-layer tests and show how they can use FakeRepository .
3. Experiment with different types of parameters for our service layer functions; show that using primitive data types allows the service layer’s clients (our tests and our Flask API) to be decoupled from the model layer.

## Why Is Everything Called a Service?
1 つ目は、アプリケーション サービス (サービス層) です。その仕事は、外界からの要求を処理し、操作を調整することです。つまり、サービス レイヤーは一連の簡単な手順に従ってアプリケーションを駆動するということです。
データベースからデータを取得する ドメイン モデルを更新する 変更を保持する これは、システム内のすべての操作で行わなければならない一種の退屈な作業であり、ビジネス ロジックから分離しておくと、整理整頓に役立ちます。
2 番目のタイプのサービスは、ドメイン サービスです。これは、ドメイン モデルに属しているが、ステートフル エンティティまたは値オブジェクト内に自然に収まらないロジックの一部の名前です。
