## インストール

[Composer](https://getcomposer.org) を通じて _Qiq_ をインストールした後...

```
composer require qiq/qiq ^3.0
```

...は[ここ](/3.x/intro.html)から始められます。

Githubリポジトリは[qiqphp/qiq](https://github.com/qiqphp/qiq) にあります。

## なぜQiqを使うのか？

Qiqは、ネイティブPHPテンプレートを好むが、より簡潔さを求める開発者向けです。
以下を提供します：

- ネイティブの`<?php ?>`**と**`{{ qiq }}`構文
- 簡潔で明示的、かつコンテキスト固有のエスケープ
- ビュー、[レイアウト](./3.x/layouts.html)、[パーシャル](./3.x/partials.html)
- [ブロック](./3.x/blocks.html)と[継承](./3.x/inheritance.html)
- 豊富で拡張可能な[HTMLヘルパー](./3.x/helpers/overview.html)
- 実装が簡単な[静的解析](./3.x/static-analysis.html)
- 完全なドキュメントとユニットテスト

Qiqは、デザイナーやコンテンツ作成者に対してテンプレートを何らかの方法で「保護」する必要があるシステムには向いていません。そのような場合は、
[Handlebars](https://pecl.php.net/package/handlebars)、
[Mustache](https://pecl.php.net/package/mustache)、
または[Twig](https://twig.symfony.com/)のようなものを使用してください。

## Qiqテンプレートとは？

Qiqは通常のPHPで、必要に応じて軽いシンタックスシュガーを加えたものです。

例えば、通常のPHPでのエスケープは次のようになります：

```
<?php echo htmlspecialchars(
    $var,
    ENT_QUOTES|ENT_DISALLOWED,
    'utf-8'
) ?>
```

これは、HTMLエスケープ用のQiqヘルパーを使用した同じものです：

```
<?= $this->h($var) ?>
```

最後に、これはQiqのシンタックスシュガーを使用した同じものです：

```
{{h $var }}
```

同じテンプレート内で通常のPHPとQiqを常に混在させることができます。例えば：

```
<?php $var = random_int(1, 99) ?>
{{h $var }}
```

実際、認識されないQiqコードはPHPとして扱われます。例えば、次のQiqコードは...

```qiq
{{ $title = "Prefix: " . $title . " (Suffix)" }}
<title>{{h $title}}</title>
```

...このPHPコードとQiqヘルパーと同等です：

```html+php
<?php $title = "Prefix: " . $title . " (Suffix)" ?>
<title><?= $this->h($title) ?></title>
```
これにより、既存のPHPテンプレートでQiqを簡単に使用でき、必要に応じてPHP構文から[Qiq構文](/3.x/syntax.html)への円滑な移行が可能になります。

## Qiqヘルパーとは？

Qiqヘルパーは単に_Helper_オブジェクトのメソッドです。例えば、HTML フォーム要素の`<select>`を追加するには、Qiqでヘルパーを使用して生成できます...

```php
{{= select (
    id: 'country-select',
    name: 'Country',
    value: 'usa',
    placeholder: 'Please pick a country',
    default: 'usa',
    options: [
        'usa' => 'United States',
        'can' => 'Canada',
        'mex' => 'Mexico',
    ],
) }}
```

..または通常のPHPで

```
<?= $this->select (
    id: 'country-select',
    name: 'Country',
    value: 'usa',
    placeholder: 'Please pick a country',
    default: 'usa',
    options: [
        'usa' => 'United States',
        'can' => 'Canada',
        'mex' => 'Mexico',
    ],
) ?>
```

[一般的なHTMLヘルパー](./3.x/helpers/general.html)、[フォームヘルパー](./3.x/helpers/forms.html)についてさらに詳しく読むか、[カスタムヘルパー](./3.x/helpers/custom.html)の作成方法を学んでください。


## なぜ明示的なエスケープなのか？

Qiqは自動エスケープを提供しません。設計上、`{{ ... }}`タグは出力を**生成しません**。すべての出力は、特定のコンテキストに対して明示的にエスケープする必要があり、開始タグの後の最初の文字で示されます：

- `{{h ... }}` はHTML コンテンツ用にエスケープします
- `{{a ... }}` はHTML 属性用にエスケープします
- `{{u ... }}` はURL 用にエスケープします
- `{{c ... }}` はCSS 用にエスケープします
- `{{j ... }}` はJavaScript 用にエスケープします
- `{{= ... }}` は生の、エスケープされていない出力です

これはQiqの意図的な設計選択です。自動エスケープを使用すると、どのコンテキストでエスケープすべきかを忘れやすくなります。コンテキストを明示的にマークすることで、常に何をしているかを考える必要があります。セキュリティに関しては、これは良いことです。

もっと知りたいですか？[ここ](/3.x/intro.html)から始めましょう！