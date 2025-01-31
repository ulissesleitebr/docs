コード
#######

パッチと Pull Request はコードを CakePHP に送って貢献するための素晴らしい方法です。
Pull Request は GitHub で作成することができるもので、パッチファイルをチケットのコメントで
伝えるよりも推奨される方法です。

最初のセットアップ
===================

CakePHP の修正に取りかかる前に自分の環境を整えることをお勧めします。
以下のソフトウェアが必要になります。

* Git
* PHP |minphpversion| 以上
* PHPUnit 5.7.0 以上

ユーザー情報（自分の名前/ハンドル名とメールアドレス）をセットしてください。 ::

    git config --global user.name 'Bob Barker'
    git config --global user.email 'bob.barker@example.com'

.. note::

    Git を使うのが初めてなら、無料で素敵なドキュメント
    `ProGit <https://git-scm.com/book/ja/>`_ をぜひお読みください。

CakePHP のソースコードの clone を GitHub から取得してください。

* `GitHub <http://github.com>`_ アカウントを持っていないなら、作成してください。
* `CakePHP リポジトリー <http://github.com/cakephp/cakephp>`_ の **Fork**
  ボタンをクリックして Fork してください。

Fork できたら、Fork したものを自分のローカルマシンへと clone してください。 ::

    git clone git@github.com:YOURNAME/cakephp.git

オリジナルの CakePHP リポジトリーを remote リポジトリーとして追加してください。
これは後で CakePHP リポジトリーから変更分を fetch するのに使うことになり、
こうすることで CakePHP を最新の状態にしておくのです。 ::

    cd cakephp
    git remote add upstream git://github.com/cakephp/cakephp.git

いまや CakePHP はセットアップされましたので
:ref:`データベース接続 <database-configuration>` で ``$test`` を定義すれば、
:ref:`すべてのテストを実行する <running-tests>` ことができるでしょう。

修正に取りかかる
==================

バグや新機能、改善に取り組むたびに毎回、 トピック・ブランチを作成してください。

ブランチは 修正/改善対象のバージョンをベースにして作成してください。たとえば、 ``3.x``
のバグを修正するなら、ブランチのベースとして ``master`` ブランチを使うことになります。
もし、 2.x 系のバグ修正なら、 ``2.x`` ブランチを使用してください。これで、GitHub は、
あなたに対象のブランチを編集させないので、後で変更をマージする際にとても簡単になります。 ::

    # 3.x のバグを修正
    git fetch upstream
    git checkout -b ticket-1234 upstream/master

    # 2.x のバグを修正
    git fetch upstream
    git checkout -b ticket-1234 upstream/2.x

.. tip::

    ブランチの名前は説明的につけてください。チケット名や機能名を含めるのは良い慣習です。
    例) ticket-1234, feature-awesome

上記は upstream (CakePHP) 2.x ブランチをベースにローカル・ブランチを作成します。
修正に取り組み、必要なだけ commit してください。ただし、注意点があります:

* :doc:`/contributing/cakephp-coding-conventions` に従ってください。
* バグが修正されたこと、もしくは新機能が機能することが判るテストケースを追加してください。
* 理にかなったコミットを心がけ、コミット文は解りやすく簡潔に書いてください。

Pull Request を送信する
=========================

変更し終え、 CakePHP へとマージされる準備が整ったら、あなたのブランチを
更新したくなることでしょう。 ::

    # master のトップに修正をリベース
    git checkout master
    git fetch upstream
    git merge upstream/master
    git checkout <branch_name>
    git rebase master

これは作業開始以降に CakePHP で行われたすべての変更を fetch + merge します。
その後に rebase 、つまり現在のコードの先端にあなたの変更を適用します。
``rebase`` 中、コンフリクトに出会うかもしれません。もし rebase が早期終了したら、
``git status`` で、どのファイルがコンフリクト/マージ失敗したのかを見ることができます。
各コンフリクトを解決させてください。その後、 rebase を continue してください。 ::

    git add <filename> # コンフリクトしたファイルごとにこれを行ってください。
    git rebase --continue

あなたのテストがすべて通過し続けているか確認してください。
その後、あなたのブランチをあなたの Fork へと push します。 ::

    git push origin <branch-name>

ブランチを push した後 rebase した場合、force push を使用する必要があります。 ::

    git push --force origin <branch-name>

あなたのブランチが GitHub に上がったら、GitHub で Pull Request を送ってください。

変更対象のマージ先を選ぶ
-------------------------

Pull Request を作る際には、ベースとなるブランチが正しく選ばれているか良く確認してください。
ひとたび Pull Request を作った後ではもう変更することはできません。

* あなたの変更が **バグ修正** であり、新機能を追加しておらず、
  現在のリリースに存在している既存の振る舞いを正すだけなら、
  マージ先として **master** を選んでください。
* あなたの変更が **新機能** もしくはフレームワークへの追加なら、
  次のバージョン番号のブランチを選んでください。
  たとえば、現在の安定版リリースが ``3.2.10`` なら、
  新機能を受け入れるブランチは ``3.next`` になります。
* あなたの変更が既存の機能性を壊すものであったり、API の仕様を変えるものであるなら、
  次のメジャーリリースを選ばなければなりません。たとえば、現在のリリースが ``3.2.2`` なら、
  次に既存の振る舞いを変更できるのは ``4.x`` となりますので、そのブランチを選んでください。

.. note::

    あなたが貢献したすべてのコードは MIT License に基づき CakePHP にライセンスされることを
    覚えておいてください。 `Cake Software Foundation
    <https://cakefoundation.org/old>`_ がすべての貢献されたコードの所有者になります。
    貢献する人は `CakePHP Community Guidelines
    <https://cakephp.org/get-involved>`_ に従うようお願いします。

メンテナンス・ブランチへとマージされたすべてのバグ修正は、
コアチームにより定期的に次期リリースにもマージされます。

.. meta::
    :title lang=ja: コード
    :keywords lang=ja: cakephp source code,code patches,test ref,descriptive name,bob barker,initial setup,global user,database connection,clone,repository,user information,enhancement,back patches,checkout
