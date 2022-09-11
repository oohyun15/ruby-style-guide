---
layout: base
title: 루비 스타일 가이드
permalink: '/'
---

# 루비 스타일 가이드

Ruby는 Shopify에서 사용하는 주요 언어입니다. 저희 소스코드는 주로 Ruby로 이뤄져 있으며 아마 Ruby로 짜여진
큰 회사(각주: Ruby shop)들 중 하나일 것입니다. 또한 Ruby는 저희가 새로운 웹 프로젝트 및 스크립트 작성 시 즐겨 사용하는 언어입니다.

저희는 Shopify의 모든 개발자들이 최소한 Ruby에 대한 기본적인 이해는 있다고 생각합니다. 정말 대단한 언어에요. 이 언어는
여러분이 매일 어떤 일을 하든 더 나은 개발자로 만들어 줄겁니다. 아래 내용들은 Ruby로 개발하면서 따라야 할 느슨한 코딩 스타일입니다.

이 스타일 가이드는 10여 년간 Shopify의 Ruby 개발에 대한 결과물입니다. 가이드 내용의 대부분은 Bozhidar Batsov의
[루비 스타일 가이드](https://github.com/rubocop-hq/ruby-style-guide)에 기반을 두고 있고, Shopify의
[많은 컨트리뷰터들](https://github.com/Shopify/ruby-style-guide/graphs/contributors)에 의해 각색되어 있습니다.

### RuboCop에 적용하기

저희는 여러분의 Ruby 프로젝트들에서 이 스타일 가이드를 적용할 수 있도록 [RuboCop](https://github.com/rubocop-hq/rubocop)을
사용하는 걸 추천합니다. RuboCop 설치 및 사용 방법은 [RuboCop 공식 문서](https://docs.rubocop.org/rubocop/)를 참고해주세요.

저희는 이 스타일 가이드로 적용 및 동기화할 수 있는 기본 RuboCop 구성을 제공합니다.
이를 사용하려면 `Gemfile`에 아래 코드를 추가해 주세요.

  ~~~ruby
  gem "rubocop-shopify", require: false
  ~~~

그리고 프로젝트의 RuboCop 설정 파일의 최상단에 아래 코드를 추가해 주세요.

  ~~~yml
  inherit_gem:
    rubocop-shopify: rubocop.yml
  ~~~

그럼 이 스타일 가이드의 `Include`, `Exclude` 설정값이 RuboCop의 기본 구성에 적용됩니다.

설정 적용에 대한 더 많은 정보는 [RuboCop 문서](https://docs.rubocop.org/rubocop/configuration.html#inheriting-configuration-from-a-dependency-gem)를 참고해 주세요.

## 목차

* [일반(General)](#일반general)
* [레이아웃(Layout)](#레이아웃layout)
* [문법(Syntax)](#문법syntax)
* [네이밍(Naming)](#네이밍naming)
* [주석(Comments)](#주석comments)
* [클래스 및 모듈(Classes and Modules)](#클래스-및-모듈classes-and-modules)
* [예외(Exceptions)](#예외exceptions)
* [컬렉션(Collections)](#컬렉션collections)
* [문자열(Strings)](#문자열strings)
* [정규 표현식(Regular Expressions)](#정규-표현식regular-expressions)
* [퍼센트 리터럴(Percent Literals)](#퍼센트-리터럴percent-literals)
* [테스트(Testing)](#테스트testing)

## 일반(General)

* 메서드의 모든 줄이 동일한 추상화 단계에서 동작하게 하세요.
  (Single Level of Abstraction 원리)

* 함수형 방식으로 코딩하세요. 가능한 한 뮤테이션(사이드 이펙트)을 피하세요.

* [방어적 프로그래밍을 피하세요.](https://web.archive.org/web/20211013164839/http://www.erlang.se/doc/programming_rules.shtml#HDR11)
  > 과한 방어적 프로그래밍은 전혀 발생하지 않을 에러를 보호해 불필요한 런타임 및 유지 보수 비용을 발생시킬 수 있습니다.

* 메서드 내에서 인자값의 변화를 피하세요.

* 몽키패치 방식을 피하세요.

* 긴 메서드가 되지 않게 피하세요.

* 많은 매개변수를 갖지 않게 피하세요.

* 필요없는 메타 프로그래밍을 피하세요.

* `private`/`protected`가 우회되지 않게 `send` 보다 `public_send` 사용을 권장합니다.

* `ruby -w`를 통해 안전한 코드를 작성하세요.

* 3개보다 많은 블록 중첩을 피하세요.

## 레이아웃(Layout)

* 소스 파일 인코딩은 `UTF-8`로 사용하세요.

* 들여쓰기 시 탭이 아닌 공백 2칸을 사용하세요.

* Unix 스타일의 줄바꿈을 사용하세요.

* 명령문 및 표현식을 구분하기 위해 `;`을 사용하는 걸 피하세요. 1줄마다 1개의 표현식을 쓰세요.

* 연산자 앞뒤, `,`, `:`, `;` 뒤, `{` 앞뒤, `}` 앞에 공백을 사용하세요.

* `(`, `[` 이후나 `]`, `)` 이전의 공백 사용을 피하세요.

* `!` 연산자 이후에 공백 사용을 피하세요.

* 범위식(예시: `1..5`)에서는 공백 사용을 피하세요.

* 메서드 콜 앞뒤에는 공백 사용을 피하세요.

  ~~~ruby
  # 나쁜 예
  foo . bar

  # 좋은 예
  foo.bar
  ~~~

* 람다 표현식 내에서 공백 사용을 피하세요.

  ~~~ruby
  # 나쁜 예
  a = -> (x, y) { x + y }

  # 좋은 예
  a = ->(x, y) { x + y }
  ~~~

* `when`은 `case`와 같은 깊이로 들여쓰기를 하세요.

* 조건문의 결과값으로 변수 할당 시, 분기(`if`, `else`, ...)를 변수에 맞추세요.

  ~~~ ruby
  # 나쁜 예
  result =
    if some_cond
      # ...
      # ...
      calc_something
    else
      calc_something_else
    end

  # 좋은 예
  result = if some_cond
    # ...
    # ...
    calc_something
  else
    calc_something_else
  end
  ~~~

* `begin` 블록 결과값으로 변수 할당 시, `rescue`/`ensure`/`end`를 시작 줄에 맞추세요.

  ~~~ ruby
  # 나쁜 예
  host = begin
           URI.parse(value).host
         rescue URI::Error
           nil
         end

  # 좋은 예
  host = begin
    URI.parse(value).host
  rescue URI::Error
    nil
  end
  ~~~

* 메서드 정의 사이마다, 그리고 메서드 내부적으로 논리적 단락마다 빈 줄을 사용하세요.

* 메서드 파라미터에 기본값을 할당할 때 `=` 연산자 주위에 공백을 사용하세요.

* 불필요한 `\` 줄 바꿈을 피하세요.

* 만약 메서드 호출 시 파라미터로 인해 1줄이 넘어갈 때, 파라미터를 메서드 호출 줄보다 1탭 더 들여쓰세요.

  ~~~ ruby
  # 시작 상태 (줄이 너무 김)
  def send_mail(source)
    Mailer.deliver(to: "bob@example.com", from: "us@example.com", subject: "Important message", body: source.text)
  end

  # 나쁜 예 (2번 들여쓰기)
  def send_mail(source)
    Mailer.deliver(
        to: "bob@example.com",
        from: "us@example.com",
        subject: "Important message",
        body: source.text)
  end

  # 좋은 예
  def send_mail(source)
    Mailer.deliver(
      to: "bob@example.com",
      from: "us@example.com",
      subject: "Important message",
      body: source.text,
    )
  end
  ~~~

* 여러 줄에 걸쳐 메서드 체인을 할 때, 1탭 더 들여써서 메서드를 호출하세요.

  ~~~ ruby
  # 나쁜 예 (이전 메서드 호출에 맞게 들여쓰기)
  User.pluck(:name)
      .sort(&:casecmp)
      .chunk { |n| n[0] }

  # 좋은 예
  User
    .pluck(:name)
    .sort(&:casecmp)
    .chunk { |n| n[0] }
  ~~~

* 여러 줄에 걸친 배열 원소를 정렬하세요.

* 한 줄에 최대 120자로 제한하세요.

* 줄 마지막에 공백을 피하세요.

* 정렬 목적을 제외한 여분의 공백을 피하세요.

* 모든 파일을 줄바꿈으로 끝내세요.

* 블록 주석을 피하세요.

  ~~~ ruby
  # 나쁜 예
  =begin
  comment line
  another comment line
  =end

  # 좋은 예
  # comment line
  # another comment line
  ~~~

* 메서드 호출 시 여는 괄호가 첫 번째 인자와 다른 줄에 있을 때, 닫는 괄호를 마지막 인자 뒷 줄에 사용하세요.

  ~~~ ruby
  # 나쁜 예
  method(
    arg_1,
    arg_2)

  # 좋은 예
  method(
    arg_1,
    arg_2,
  )
  ~~~

* 매직 코멘트와 코드 및 설명 주석을 빈 줄로 분리하세요.

  ~~~ruby
  # 좋은 예
  # frozen_string_literal: true

  # Some documentation for Person
  class Person
    # Some code
  end

  # 나쁜 예
  # frozen_string_literal: true
  # Some documentation for Person
  class Person
    # Some code
  end
  ~~~

* 접근 제어자(attribute accessor) 주변에 빈 줄을 사용하세요.

  ~~~ruby
  # 나쁜 예
  class Foo
    attr_reader :foo
    def foo
      # do something...
    end
  end

  # 좋은 예
  class Foo
    attr_reader :foo

    def foo
      # do something...
    end
  end
  ~~~

* 메서드, 클래스, 모듈, 블록 간에 빈 줄 사용을 피하세요.

  ~~~ruby
  # 나쁜 예
  class Foo

    def foo

      begin

        do_something do

          something

        end

      rescue

        something

      end

      true

    end

  end

  # 좋은 예
  class Foo
    def foo
      begin
        do_something do
          something
        end
      rescue
        something
      end
    end
  end
  ~~~

## 문법(Syntax)

* 오직 상수(클래스 및 모듈 포함)와 생성자(예시: `Array()`, `Nokogiri::HTML()`)를 참조할 때만
  `::`를 사용하세요. 일반 메서드 호출에서는 `::` 사용을 피하세요.

* 상수를 찾아갈 때 부모 클래스/모듈을 탐색하지 않으므로 클래스와 모듈을 정의, 상속할 때 `::` 사용을 피하세요.

  ~~~ ruby
  # 나쁜 예
  module A
    FOO = "test"
  end

  class A::B
    puts FOO  # NameError exception을 발생시킵니다.
  end

  # 좋은 예
  module A
    FOO = "test"

    class B
      puts FOO
    end
  end
  ~~~

* 메서드에 파라미터가 있을 때 괄호와 함께 `def`를 사용하세요. 메서드에 어느 파라미터도 들어가지 않을 때
  괄호를 생략하세요.

* `for` 사용을 피하세요.

* `then` 사용을 피하세요.

* `if`/`then`/`else`/`end` 구조보다 삼항 연산자(`?:`) 사용을 권장합니다.

  ~~~ ruby
  # 나쁜 예
  result = if some_condition then something else something_else end

  # 좋은 예
  result = some_condition ? something : something_else
  ~~~

* 삼항 연산자에서 분기당 하나의 표현식을 사용하세요. 즉, 삼항 연산자는 반드시 중첩되지 않아야 합니다.
  이런 경우에는 `if`/`else` 구조를 사용하세요.

* 여러 줄의 `?:`(삼항 연산자) 사용을 피하세요. 대신 `if`/`unless`를 사용하세요.

* 1줄 조건일 때 `when x then ...`을 사용하세요.

* `not` 대신 `!`을 사용하세요.

* `and`/`or`보단 `&&`/`||` 사용을 권장합니다.

* 부정 조건문에서 `if`보단 `unless` 사용을 권장합니다.

* `else`와 함께 `unless` 사용을 피하세요. 긍정 조건문이 먼저 앞에 오도록 다시 작성하세요.

* 메서드 호출 시 인자들 사이에 괄호를 사용하세요. 단, 인자를 제공하지 않는 메서드의 경우엔 괄호를 생략하세요.
  또는 메서드 호출 시 단일 줄이면서 아래 조건일 때는 괄호를 생략하세요.
  - 내부 수신자(implicit receiver)를 포함한 클래스 메서드 호출일 때
  - 문법 설탕(syntactic sugar)을 통한 호출일 때 (예시로 `1 + 1`은 `+` 메서드를 호출하고, `foo[bar]`
    는 `[]` 메서드를 호출합니다.)

  ~~~ ruby
  # 나쁜 예
  class User
    include(Bar)
    has_many(:posts)
  end

  # 좋은 예
  class User
    include Bar
    has_many :posts
    SomeClass.some_method(:foo)
  end
  ~~~

  - 아래 메서드들 중 하나일 때
    * `require`
    * `require_relative`
    * `require_dependency`
    * `yield`
    * `raise`
    * `puts`

* 메서드 호출 시 내부 옵션 해시의 중괄호를 생략하세요.

* 블록 내 메서드 호출이 유일한 연산일 때 proc 호출 단축 방법을 사용하세요.

  ~~~ ruby
  # 나쁜 예
  names.map { |name| name.upcase }

  # 좋은 예
  names.map(&:upcase)
  ~~~

* 단일 줄 블록에서 `do...end`보단 `{...}` 사용을 권장합니다.

* 여러 줄 블록에서 `{...}`보단 `do...end` 사용을 권장합니다.

* 가능하다면 `return`을 생략하세요.

* 가능하다면 `self`를 생략하세요.

  ~~~ ruby
  # 나쁜 예
  self.my_method

  # 좋은 예
  my_method

  # 또한 좋은 예
  attr_writer :name

  def my_method
    self.name = "Rafael" # `self`는 접근 제어자를 참조할 때 필요합니다.
  end
  ~~~

* 조건문에서 해당 결과값을 사용할 때 괄호로 그 식을 감싸주세요.

  ~~~ ruby
  if (value = /foo/.match(string))
  ~~~

* 아직 초기화되지 않은 변수를 초기화할 때만 `||=`를 사용하세요.

* Boolean 변수를 초기화할 때 `||=` 사용을 피하세요.

  ~~~ ruby
  # 나쁜 예 - 값이 false더라도 true로 설정됩니다.
  @enabled ||= true

  # 좋은 예
  @enabled = true if @enabled.nil?

  # 또한 유효한 예 - defined?를 이용한 회피 방법
  @enabled = true unless defined?(@enabled)
  ~~~

* 메서드 이름과 여는 괄호 사이에 공백을 피하세요.

* `lambda` 키워드보단 람다 표현식 사용을 권장합니다.

  ~~~ ruby
  # 나쁜 예
  l = lambda { |a, b| a + b }
  l.call(1, 2)

  l = lambda do |a, b|
    tmp = a * 7
    tmp * b / 50
  end

  # 좋은 예
  l = ->(a, b) { a + b }
  l.call(1, 2)

  l = ->(a, b) do
    tmp = a * 7
    tmp * b / 50
  end
  ~~~

* `Proc.new`보단 `proc` 사용을 권장합니다.

* 호출하지 않는 블록 파라미터 이름 앞에 `_`를 붙이세요. 또는 `_`로만 이름을 지어도 괜찮습니다.

* 유효하지 않은 데이터가 생길 수 있을 때 보호 구문(guard clause) 사용을 권장합니다. 보호 구문은
  함수 맨 위에 가능한 한 빨리 빠져나올 수 있는 조건문을 배치하는 걸 말합니다.

  ~~~ ruby
  # 나쁜 예
  def compute_thing(thing)
    if thing[:foo]
      update_with_bar(thing)
      if thing[:foo][:bar]
        partial_compute(thing)
      else
        re_compute(thing)
      end
    end
  end

  # 좋은 예
  def compute_thing(thing)
    return unless thing[:foo]
    update_with_bar(thing[:foo])
    return re_compute(thing) unless thing[:foo][:bar]
    partial_compute(thing)
  end
  ~~~

* 옵션 해시보단 키워드 인자 사용을 권장합니다.

* `collect`보단 `map`, `detect`보단 `find`, `find_all`보단 `select`, `length`보단
  `size` 사용을 권장합니다.

* `DateTime`보단 `Time` 사용을 권장합니다.

* `"2018-03-20T11:16:39-04:00"`과 같이 ISO08601 포맷의 문자열을 받을 때 `Time.parse(foo)`
  대신 `Time.iso8601(f00)` 사용을 권장합니다.

## 네이밍(Naming)

* 심볼, 메서드, 변수에 대해선 `snake_case`를 사용하세요.

* 클래스, 모듈에 대해선 `CamelCase`를 사용하세요. 단, HTTP, RFC, XML과 같은 두문자어에 대해선
  그대로 대문자로 유지하세요.

* 파일 및 디렉토리 네이밍에 대해선 `snake_case`를 사용하세요. (예시: `hello_world.rb`)

* 소스파일 당 1개의 클래스나 모듈을 정의하세요. 파일 이름을 클래스나 모듈의 이름으로 지으세요. 단,
  `CamelCase`를 `snake_case`로 바꾸세요.

* 상수에 대해선 `SCREAMING_SNAKE_CASE`를 사용하세요.

* 짧은 블록을 넣을 때, 실제 인자값으로 들어갈 것에 따라 해당 인자의 이름을 지으세요. (예시:
  `|hash, e|`는 hash, element로 유추 가능)

* 이항 연산자를 정의할 때, 파라미터 이름을 `other`로 지으세요. (`<<`와 `[]`는 의미가 달라지므로
  이 규칙에선 제외합니다.)

* 서술형 메서드(predicate method)는 `?`와 함께 이름을 지으세요. 서술형 메서드는 boolean 값을
  반환하는 메서드입니다.

* 만약 메서드가 boolean 값을 반환하지 않는다면 메서드 이름 끝에 `?`를 피하세요.

* 메서드 이름 앞에 `is_`를 붙이는 걸 피하세요.

  ~~~ruby
  # 나쁜 예
  def is_empty?
  end

  # 좋은 예
  def empty?
  end
  ~~~

* 메서드 이름 앞에 `get_`을 붙이는 걸 피하세요.

* bang 타입이 아닌 메서드가 없을 때 메서드 이름 끝에 `!`을 붙이는 걸 피하세요. bang은 기존 메서드보다
  더 위험한 버전의 메서드를 의미합니다. 예를 들어 `save`는 ActiveRecord에서 boolean 값을 반환하지만,
  `save!`는 실패 시 예외를 발생시킵니다.

* 매직 넘버 사용을 피하세요. 상수를 사용하고 그 상수에 의미 있는 이름을 지어주세요.

* 차별적 어원을 가진 (혹은 그렇게 해석될 수 있는) 명명법 사용을 피하세요.

## 주석(Comments)

* 읽는 사람이 놓치지 않게 주석에 관련된 문맥을 포함하세요.

* 주석을 코드와 계속 동기화 시키세요.

* 적절한 대문자 쓰기 및 구두점을 사용해 주석을 작성하세요. (각주: 주석은 항상 영어로 작성합니다.)

* 불필요한 주석 작성을 피하세요. 만약 코드 작동 방식히 명확하지 않다면 **어떻게** 코드가 돌아가는지가
  아닌 **왜** 코드 작성을 이렇게 했는지에 집중하세요.

## 클래스 및 모듈(Classes and Modules)

* 클래스 메서드로만 이뤄져 있을 경우 클래스보단 모듈 사용을 권장합니다. 클래스는 해당 클래스로부터 인스턴스를
  생성하는게 적절할 때만 사용해야 합니다.

* `module_function`보단 `extend self` 사용을 권장합니다.

  ~~~ ruby
  # 나쁜 예
  module SomeModule
    module_function

    def some_method
    end

    def some_other_method
    end
  end

  # 좋은 예
  module SomeModule
    extend self

    def some_method
    end

    def some_other_method
    end
  end
  ~~~

* 클래스 메서드를 선언할 때 `def self.`보단 `class << self` 블록을 사용하세요. 그리고 한 블록 안에
  클래스 메서드를 모두 모으세요.

  ~~~ ruby
  # 나쁜 예
  class SomeClass
    def self.method1
    end

    def method2
    end

    private

    def method3
    end

    def self.method4 # 이건 실제로 private 메서드로 동작하지 않습니다.
    end
  end

  # 좋은 예
  class SomeClass
    class << self
      def method1
      end

      private

      def method4
      end
    end

    def method2
    end

    private

    def method3
    end
  end
  ~~~

* 클래스 계층 구조를 설계할 때 [리스코프 치환 원칙](https://en.wikipedia.org/wiki/Liskov_substitution_principle)을
  준수하세요.

* 간단한 접근자와 설정자를 정의할 때 `attr_accessor`, `attr_reader`, `attr_writer`를 사용하세요.

  ~~~ ruby
  # 나쁜 예
  class Person
    def initialize(first_name, last_name)
      @first_name = first_name
      @last_name = last_name
    end

    def first_name
      @first_name
    end

    def last_name
      @last_name
    end
  end

  # 좋은 예
  class Person
    attr_reader :first_name, :last_name

    def initialize(first_name, last_name)
      @first_name = first_name
      @last_name = last_name
    end
  end
  ~~~

* `attr`보단 `attr_reader`와 `attr_accessor` 사용을 권장합니다.

* 클래스(`@@`) 변수 사용을 피하세요.

* `public`, `protect`, `private`는 메서드 정의와 같은 깊이만큼 들여쓰세요. 가시성을
  위해 위아래로 빈 줄을 띄어두세요.

  ~~~ ruby
  class SomeClass
    def public_method
      # ...
    end

    private

    def private_method
      # ...
    end

    def another_private_method
      # ...
    end
  end
  ~~~

* `alias`보다 `alias_method` 사용을 권장합니다.

## 예외(Exceptions)

* `raise`를 사용해서 예외를 발생시키세요.

* 2개 인자를 받는 `raise`에서 `RuntimeError`를 생략하세요.

  ~~~ ruby
  # 나쁜 예
  raise RuntimeError, "message"

  # 좋은 예 - 기본적으로 RuntimeError를 발생시킵니다.
  raise "message"
  ~~~

* 예외 인스턴스 생성해 `raise`를 하는 것보다 예외 클래스와 메시지를 `raise` 두 인자값에 넣어 사용하는걸 권장합니다.

  ~~~ ruby
  # 나쁜 예
  raise SomeException.new("message")
  # `raise SomeException.new("message"), backtrace`와 같이 사용할 수 없습니다.

  # 좋은 예
  raise SomeException, "message"
  # `raise SomeException, "message", backtrace`로 사용할 수 있습니다.
  ~~~

* `ensure` 블록으로부터 반환(return)하는 걸 피하세요. 만약 메서드 내 `ensure` 블록에서 명시적으로
  반환한다면, 어떤 예외들이 발생하더라도 반환값이 나오게 되고, 그 메서드는 전혀 예외 발생을 하지 않았더라도
  그 반환값을 도출합니다. 사실상 예외 발생 부분은 무시될 것입니다.

  ~~~ ruby
  # 나쁜 예
  def foo
    raise
  ensure
    return "very bad idea"
  end
  ~~~

* 가능하다면 명시적으로 `begin` 블록을 사용하지 마세요.

  ~~~ ruby
  # 나쁜 예
  def foo
    begin
      # main logic goes here
    rescue
      # failure handling goes here
    end
  end

  # 좋은 예
  def foo
    # main logic goes here
  rescue
    # failure handling goes here
  end
  ~~~

* 아무것도 하지 않는 `rescue` 구문 사용을 피하세요.

  ~~~ ruby
  # 나쁜 예
  begin
    # 에러 발생
  rescue SomeError
    # rescue 절에서 아무것도 하지 않음
  end

  # 나쁜 예 - `rescue nil`은 문법 에러를 포함한 모든 에러들을 내포해서 에러 추적을 어렵게 만듭니다.
  do_something rescue nil
  ~~~

* 예외 발생 시점에서 바로 `resuce` 사용을 피하세요.

  ~~~ ruby
  # 나쁜 예 - StandardError 클래스와 그 하위 클래스들의 예외들을 잡습니다.
  read_file rescue handle_error($!)

  # 좋은 예 - 오직 Errno::ENOENT 클래스와 그 하위 클래스들의 예외들을 잡습니다.
  def foo
    read_file
  rescue Errno::ENOENT => error
    handle_error(error)
  end
  ~~~

* `Exception` 클래스를 이용한 예외 처리를 피하세요.

  ~~~ ruby
  # 나쁜 예
  begin
    # exit를 호출하면 kill 시그널이 잡힙니다. (단, kill -9는 제외)
    exit
  rescue Exception
    puts "you didn't really want to exit, right?"
    # exception handling
  end

  # 좋은 예
  begin
    # 아무것도 없는 rescue는 `Exception`이 아닌 `StandardError`를 기준으로 예외 처리를 합니다.
  rescue => error
    # exception handling
  end
  ~~~

* 새 예외 클래스를 선언하기보단 표준 라이브러리에 있는 예외 사용을 권장합니다.

* 예외 변수로 의미 있는 이름을 사용하세요.

  ~~~ ruby
  # 나쁜 예
  begin
    # an exception occurs here
  rescue => e
    # exception handling
  end

  # 좋은 예
  begin
    # an exception occurs here
  rescue => error
    # exception handling
  end
  ~~~

## 컬렉션(Collections)

* 배열 및 해시를 생성할 때 파라미터를 넘겨주지 않는다면 배열/해시 표기법을 사용하세요.

  ~~~ ruby
  # 나쁜 예
  arr = Array.new
  hash = Hash.new

  # 좋은 예
  arr = []
  hash = {}
  ~~~

* `%w`나 `%i`보단 배열 문법 그대로 사용하는 걸 권장합니다.

  ~~~ ruby
  # 나쁜 예
  STATES = %w(draft open closed)

  # 좋은 예
  STATES = ["draft", "open", "closed"]
  ~~~

* 여러 줄의 컬렉션에서는 마지막에 콤마를 붙이세요.

  ~~~ ruby
  # 나쁜 예
  {
    foo: :bar,
    baz: :toto
  }

  # 좋은 예
  {
    foo: :bar,
    baz: :toto,
  }
  ~~~

* 배열에서 처음이나 마지막 원소를 접근할 때 `[0]`, `[-1]`보단 `first`, `last` 사용을 권장합니다.

* 해시 키에 값이 바뀔 수 있는(mutable) 객체를 사용하는 걸 피하세요.

* 해시의 모든 키가 심볼일 때 단축 문법을 사용하세요.

  ~~~ ruby
  # 나쁜 예
  { :a => 1, :b => 2 }

  # 좋은 예
  { a: 1, b: 2 }
  ~~~

* 해시의 모든 키가 심볼이 아닐 때 단축 문법보단 로켓(`=>`) 문법 사용을 권장합니다.

  ~~~ ruby
  # 나쁜 예
  { a: 1, "b" => 2 }

  # 좋은 예
  { :a => 1, "b" => 2 }
  ~~~

* `Hash#has_key?`보단 `Hash#key?` 사용을 권장합니다.

* `Hash#has_value?`보단 `Hash#value?` 사용을 권장합니다.

* 반드시 존재해야 하는 해시 키를 다룰 때 `Hash#fetch`를 사용하세요.

  ~~~ ruby
  heroes = { batman: "Bruce Wayne", superman: "Clark Kent" }
  # 나쁜 예 - 만약 실수하게 된다면 문제를 바로 알아차리지 못할 수 있습니다.
  heroes[:batman] # => "Bruce Wayne"
  heroes[:supermann] # => nil

  # 좋은 예 - fetch는 문제를 명확하게 하기 위해 KeyError를 발생시킵니다.
  heroes.fetch(:supermann)
  ~~~

* 해시 키에 기본값을 설정할 때 커스텀 로직보단 `Hash#fetch`를 사용하세요.

  ~~~ ruby
  batman = { name: "Bruce Wayne", is_evil: false }

  # 나쁜 예 - 만약 거짓 값(nil, false)과 함께 || 연산자를 사용한다면 예상치 못한 결과를 얻을 수 있습니다.
  batman[:is_evil] || true # => true

  # 좋은 예 - fetch는 거짓 값에서 올바르게 동작합니다.
  batman.fetch(:is_evil, true) # => false
  ~~~

* 여는 괄호가 첫번째 원소 줄과 다른 줄에 있을 때 `]`과 `}`를 마지막 원소 뒷 줄에 배치하세요.

  ~~~ ruby
  # 나쁜 예
  [
    1,
    2]

  {
    a: 1,
    b: 2}

  # 좋은 예
  [
    1,
    2,
  ]

  {
    a: 1,
    b: 2,
  }
  ~~~

## 문자열(Strings)

* 문자열 연결보단 문자열 보간과 문자열 형식화(formatting)를 권장합니다.

  ~~~ ruby
  # 나쁜 예
  email_with_name = user.name + " <" + user.email + ">"

  # 좋은 예
  email_with_name = "#{user.name} <#{user.email}>"

  # 좋은 예
  email_with_name = format("%s <%s>", user.name, user.email)
  ~~~

* 문자열 보간 표현식 괄호 안에 패딩 공백 사용을 피하세요.

  ~~~ ruby
  # 나쁜 예
  "From: #{ user.first_name }, #{ user.last_name }"

  # 좋은 예
  "From: #{user.first_name}, #{user.last_name}"
  ~~~

* 쌍따옴표 문자열을 사용하세요.

  ~~~ ruby
  # 나쁜 예
  'Just some text'
  'No special chars or interpolation'

  # 좋은 예
  "Just some text"
  "No special chars or interpolation"
  "Every string in #{project} uses double_quotes"
  ~~~

* `?x`와 같은 문자 문법 사용을 피하세요.

* 문자열에 보간된 인스턴스 및 전역 변수 주위에 `{}`를 사용하세요.

  ~~~ ruby
  class Person
    attr_reader :first_name, :last_name

    def initialize(first_name, last_name)
      @first_name = first_name
      @last_name = last_name
    end

    # 나쁜 예 - 유효하지만 어색합니다.
    def to_s
      "#@first_name #@last_name"
    end

    # 좋은 예
    def to_s
      "#{@first_name} #{@last_name}"
    end
  end

  $global = 0
  # 나쁜 예
  puts "$global = #$global"

  # 괜찮지만 전역 변수를 쓰지 마세요.
  puts "$global = #{$global}"
  ~~~

* 보간한 객체에서 `Object#to_s` 사용을 피하세요.

  ~~~ ruby
  # 나쁜 예
  message = "This is the #{result.to_s}."

  # 좋은 예 - 암시적으로 `result.to_s`가 호출됩니다.
  message = "This is the #{result}."
  ~~~

* 더 빠르고 전문화된 대안이 있는 상황에서는 `String#gsub` 사용을 피하세요.

  ~~~ ruby
  url = "http://example.com"
  str = "lisp-case-rules"

  # 나쁜 예
  url.gsub("http://", "https://")
  str.gsub("-", "_")
  str.gsub(/[aeiou]/, "")

  # 좋은 예
  url.sub("http://", "https://")
  str.tr("-", "_")
  str.delete("aeiou")
  ~~~

* 여러 줄에 걸친 히어독(heredoc)을 사용할 때 공백 문자가 유지되는 걸 기억하세요. 과도한 공백
  문자를 줄이기 위해 약간의 마진을 사용하는 건 좋은 방법입니다.

  ~~~ ruby
  code = <<-END.gsub(/^\s+\|/, "")
    |def test
    |  some_method
    |  other_method
    |end
  END
  # => "def test\n  some_method\n  other_method\nend\n"

  # Rails에서 `#strip_heredoc`을 통해 위와 같은 결과를 얻을 수 있습니다.
  code = <<-END.strip_heredoc
    def test
      some_method
      other_method
    end
  END
  # => "def test\n  some_method\n  other_method\nend\n"
  ~~~

* Ruby 2.3 이상에서는 ["물결표 히어독"](https://github.com/ruby/ruby/pull/878) 문법
  사용을 권장합니다. 이건 Rails에서의 `strip_heredoc`과 동일하게 동작합니다.

  ~~~ruby
  code = <<~END
    def test
      some_method
      other_method
    end
  END
  # => "def test\n  some_method\n  other_method\nend\n"
  ~~~

* 히어독 내용과 끝을 시작 지점과 맞추세요.

  ~~~ruby
  # 나쁜 예
  class Foo
    def bar
      <<~SQL
        'Hi'
    SQL
    end
  end

  # 좋은 예
  class Foo
    def bar
      <<~SQL
        'Hi'
      SQL
    end
  end

  # 나쁜 예

  # 히어독 내용이 히어독 끝맺음보다 전에 있습니다.
  foo arg,
      <<~EOS
    Hi
      EOS

  # 좋은 예
  foo arg,
      <<~EOS
    Hi
  EOS

  # 좋은 예
  foo arg,
    <<~EOS
      Hi
    EOS
  ~~~

## 정규 표현식(Regular Expressions)

* 문자열 내에서 단순 텍스트 찾기라면 정규 표현식을 쓰지 않는걸 권장합니다.

  ~~~ ruby
  string["text"]
  ~~~

* 캡쳐한 결과를 사용하지 않을 때 non-capturing 그룹을 사용하세요.

  ~~~ ruby
  # 나쁜 예
  /(first|second)/

  # 좋은 예
  /(?:first|second)/
  ~~~

* 그룹 내 값을 캡쳐할 때 Perl 기반의 레거시 변수보단 `Regexp#match` 사용을 권장합니다.

  ~~~ ruby
  # 나쁜 예
  /(regexp)/ =~ string
  process $1

  # 좋은 예
  /(regexp)/.match(string)[1]
  ~~~

* 숫자 형식 그룹보단 이름 기반의 그룹 사용을 권장합니다.

  ~~~ ruby
  # 나쁜 예
  /(regexp)/ =~ string
  ...
  process Regexp.last_match(1)

  # 좋은 예
  /(?<meaningful_var>regexp)/ =~ string
  ...
  process meaningful_var
  ~~~

* 문자열의 처음부터 끝까지 찾을 때는 `^`, `$`보단 `\A`, `\z` 사용을 권장합니다.

  ~~~ ruby
  string = "some injection\nusername"
  string[/^username$/] # `^`, `$`는 줄의 시작부터 끝까지 찾습니다.
  string[/\Ausername\z/] # `\A`, `\z`는 문자열의 시작부터 끝까지 찾습니다.
  ~~~

## 퍼센트 리터럴(Percent Literals)

* 문자열 보간과 큰따옴표가 필요한 단일 줄의 문자열에서만 `%()`를 사용하세요. 여러 줄의 경우,
  히어독 사용을 권장합니다.

* 문자열에 `'`와 `"`가 모두 있지 않다면 `%q` 사용을 피하세요. 일반 문자열 리터럴이 더 읽기 쉬우며
  문자열 내 이스케이프 할 단어가 많지 않다면 문자열 그대로 쓰는 것이 더 낫습니다.

* 정규 표현식에 적어도 1개 이상의 `/`가 있을 때만 `%r`을 사용하세요.

  ~~~ ruby
  # 나쁜 예
  %r{\s+}

  # 좋은 예
  %r{^/(.*)$}
  %r{^/blog/2011/(.*)$}
  ~~~

* `%s` 사용을 피하세요. 공백이 있는 심볼을 만들기 위해선 `:"some string"`과 같이 사용하세요.

* 괄호가 포함된 정규 표현식에서만 제외하고 모든 `%` 리터럴 구분 기호로 `()`사용을 권장합니다.
  `()`, `{}`, `[]`, `<>` 순으로 리터럴 안에서 사용하지 않는 기호를 사용하세요.

## 테스트(Testing)

* 여러분이 쓴 여타 다른 코드들처럼 테스트 코드를 취급하세요. 이 말은 즉슨 가독성, 유지관리성, 복잡성 등을
  계속 염두에 두어야 한다는 뜻입니다.

* 테스트 프레임워크로 [Minitest](https://github.com/minitest/minitest)를 권장합니다.

* 코드 각기의 스펙을 다룰 수 있게 테스트 케이스를 제한해서 구성하세요.

* 테스트 케이스의 시작(setup), 동작(action), 어서션(assertion) 구역을 빈 줄로 구분된 단략으로 구성하세요.

  ~~~ ruby
  test "sending a password reset email clears the password hash and set a reset token" do
    user = User.create!(email: "bob@example.com")
    user.mark_as_verified

    user.send_password_reset_email

    assert_nil user.password_hash
    refute_nil user.reset_token
  end
  ~~~

* 복합적인 테스트 케이스를 단독으로 기능을 테스트하는 여러 개의 간단한 테스트들로 분리하세요.

* 테스트 케이스를 정의할 때 `def test_foo`보단 `test "foo"` 스타일의 구문 사용을 권장합니다.

* 좀 더 설명적인 에러 메시지를 가져오는 어서션 메서드 사용을 권장합니다.

  ~~~ ruby
  # 나쁜 예
  assert user.valid?
  assert user.name == "tobi"


  # 좋은 예
  assert_predicate user, :valid?
  assert_equal "tobi", user.name
  ~~~

* `assert_nothing_raised` 사용을 피하세요. 대신 긍정적인 어서션을 사용하세요.

* 기대 구문(expectation)보단 어서션 사용을 권장합니다. 기대 구문은 특히 싱글톤 객체의 조합에서
  더 불확실한 테스트로 이끕니다.

  ~~~ ruby
  # 나쁜 예
  StatsD.expects(:increment).with("metric")
  do_something

  # 좋은 예
  assert_statsd_increment("metric") do
    do_something
  end
  ~~~
