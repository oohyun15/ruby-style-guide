---
layout: base
title: 루비 스타일 가이드
permalink: '/'
---

# 루비 스타일 가이드

Ruby는 Shopify에서 사용하는 메인 언어입니다. 저희 소스코드는 주로 Ruby로 이뤄져 있으며 아마 Ruby로 짜여진
큰 회사(Ruby shop)들 중 하나일 것입니다. 또한 Ruby는 저희가 새로운 웹 프로젝트 및 스크립트 작성 시 즐겨 사용하는 언어입니다.

저희는 Shopify의 모든 개발자들이 최소한 Ruby에 대한 기본적인 이해는 있다고 기대합니다. 정말 대단한 언어에요. 이 언어는
여러분이 매일 어떤 일을 하든 더 나은 개발자로 만들어 줄겁니다. 아래 내용들은 Ruby로 개발하면서 따라야 할 느슨한 코딩 스타일입니다.

이 스타일 가이드는 10여 년간 Shopify의 Ruby 개발에 대한 결과물입니다. 가이드 내용의 대부분은 Bozhidar Batsov의
[루비 스타일 가이드](https://github.com/rubocop-hq/ruby-style-guide)에 기반을 두고 있고, Shopify의
[많은 컨트리뷰터들](https://github.com/Shopify/ruby-style-guide/graphs/contributors)에 의해 각색되어 있습니다.

### RuboCop에 적용하기

저희는 여러분의 Ruby 프로젝트들에서 이 스타일 가이드를 적용할 수 있도록 [RuboCop](https://github.com/rubocop-hq/rubocop)을
사용하는 걸 추천합니다. RuboCop 설치 및 사용 방법은 [RuboCop 공식 문서](https://docs.rubocop.org/rubocop/)를 참고해주세요.

저희는 이 스타일 가이드로 적용하고 싱크할 수 있는 기본 RuboCop 구성을 제공합니다.
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
* [Exceptions](#exceptions)
* [Collections](#collections)
* [Strings](#strings)
* [Regular Expressions](#regular-expressions)
* [Percent Literals](#percent-literals)
* [Testing](#testing)

## 일반(General)

* 메서드의 모든 줄이 같은 추상화 단계에서 동작하게 하세요.
  (Single Level of Abstraction 원리)

* 함수형 방식으로 코딩하세요. 가능한 한 뮤테이션(사이드 이펙트)을 피하세요.

* [방어적 프로그래밍을 피하세요.](https://web.archive.org/web/20211013164839/http://www.erlang.se/doc/programming_rules.shtml#HDR11)
  > 과한 방어적 프로그래밍은 전혀 발생하지 않을 에러를 보호해 불필요한 런타임 및 유지 보수 비용을 발생시킬 수 있습니다.

* 메서드 내에서 인자값의 변화를 피하세요.

* 몽키패치 방식을 피하세요.

* 긴 메서드가 되지 않게 피하세요.

* 많은 매개변수를 갖지 않게 피하세요.

* 필요없는 메타 프로그래밍을 피하세요.

* `private`/`protected`가 우회되지 않게 `send` 보다 `public_send`를 권장합니다.

* `ruby -w`를 통해 안전한 코드를 작성하세요.

* 3개보다 많은 블록 중첩이 생기지 않게 피하세요.

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

* 람다 표현식 내에서는 공백 사용을 피하세요.

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

* begin 블록 결과값으로 변수 할당 시, rescue/ensure/end를 시작 줄에 맞추세요.

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

* 메서드 파라미터에 기본값을 할당할 때 `=` 연산자 주변에 공백을 사용하세요.

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

* 메서드, 클래스, 모듈, 블록 간에 빈 줄을 피하세요.

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
  `::`를 사용하세요. 일반 메서드 호출에서는 `::`를 피하세요.

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

* `if/then/else/end` 구조보다 삼항 연산자(`?:`) 권장합니다.

  ~~~ ruby
  # 나쁜 예
  result = if some_condition then something else something_else end

  # 좋은 예
  result = some_condition ? something : something_else
  ~~~

* 삼항 연산자에서 분기당 하나의 표현식을 사용하세요. 즉, 삼항 연산자는 반드시 중첩되지 않아야 합니다.
  이런 경우에는 `if/else` 구조를 사용하세요.

* 여러 줄의 `?:`(삼항 연산자) 사용을 피하세요. 대신 `if/unless`를 사용하세요.

* 1줄 조건일 때 `when x then ...`을 사용하세요.

* `not` 대신 `!`을 사용하세요.

* `and`/`or`보단 `&&`/`||`을 권장합니다.

* 부정 조건문에서 `if`보단 `unless`를 권장합니다.

* `else`와 함께 `unless` 사용을 피하세요. 긍정 조건문이 먼저 앞에 오도록 다시 작성하세요.

* 메서드 호출 시 인자들 사이에 괄호를 사용하세요. 단, 인자를 제공하지 않는 메서드의 경우엔 괄호를 생략하세요.
  또는 메서드 호출 시 단일 줄이면서 아래 조건일 때는 괄호를 생략하세요.
  - 내부 수신자를 포함한 클래스 메서드 호출일 때
  - 문법 설탕(syntactic sugar)을 통한 호출일 때 (예시로 `1 + 1`은 `+` 메서드를 호출하고, `foo[bar]`
    는 `[]` 메서드를 호출한다.)

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

* 단일 줄 블록에서 `do...end`보단 `{...}`을 권장합니다.

* 여러 줄 블록에서 `{...}`보단 `do...end`를 권장합니다.

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
  # 나쁜 예 - 값이 false더라도 true로 설정된다. 
  @enabled ||= true

  # 좋은 예
  @enabled = true if @enabled.nil?

  # 또한 타당한 예 - defined?를 이용한 회피 방법
  @enabled = true unless defined?(@enabled)
  ~~~

* 메서드 이름과 여는 괄호 사이에 공백을 피하세요.

* `lambda` 키워드를 사용하기 보단 람다 표현식을 권장합니다.

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

* `Proc.new`보단 `proc`을 권장합니다.

* 사용하지 않는 블록 파라미터 이름 앞에 `_`를 붙이세요. 또한 `_`로만 사용해도 괜찮습니다.

* 유효하지 않은 데이터가 생길 수 있을 때 보호 구문(guard clause)를 사용하세요. 보호 구문은 가능한 한
  빨리 빠져나올 수 있는 함수 맨 위에 위치한 조건문입니다.

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

* 옵션 해시보단 키워드 인자를 권장합니다.

* `collect`보단 `map`, `detect`보단 `find`, `find_all`보단 `select`, `length`보단
  `size`를 권장합니다.

* `DateTime`보단 `Time`을 권장합니다.

* `"2018-03-20T11:16:39-04:00"`과 같이 ISO08601 포맷의 문자열을 받을 때 `Time.parse(foo)`
  대신 `Time.iso8601(f00)`를 권장합니다.

## 네이밍(Naming)

* 심볼, 메서드, 변수에 대해선 `snake_case`를 사용하세요.

* 클래스, 모듈에 대해선 `CamelCase`를 사용하세요. 단, HTTP, RFC, XML과 같은 두문자어에 대해선
  그대로 대문자로 유지하세요.

* 파일 및 디렉토리 네이밍에 대해선 `snake_case`를 사용하세요. (예시: `hello_world.rb`)

* 소스파일당 하나의 클래스나 모듈을 정의하세요. 파일 이름을 클래스나 모듈의 이름으로 지으세요. 단,
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

* bang 타입이 아닌 메서드가 없을 때 메서드 이름 끝에 `!`을 붙이는 걸 피하세요. bang은 기본 메서드보다
  더 위험한 버전의 메서드를 의미합니다. 예를 들어 `save`는 ActiveRecord에서 boolean 값을 반환하지만,
  `save!`는 실패 시 예외를 발생시킵니다.

* 매직 넘버를 피하세요. 상수를 사용하고 그 상수에 의미있는 이름을 지어주세요.

* 차별적 어원을 가진 (혹은 그렇게 해석될 수 있는) 명명법을 피하세요.

## 주석(Comments)

* 읽는 사람이 놓치지 않게 주석에 관련된 문맥을 포함하세요.

* 주석을 코드와 계속 동기화 시키세요.

* 적절한 대문자 쓰기 및 구두점을 사용해 주석을 작성하세요. (각주: 주석은 항상 영어로 작성합니다.)

* 불필요한 주석을 피하세요. 만약 코드 작동 방식히 명확하지 않다면 **어떻게** 코드가 돌아가는지가 아닌
   **왜** 코드 작성을 이렇게 했는지에 대해 집중하세요.

## 클래스 및 모듈(Classes and Modules)

* 클래스 메서드만 갖고 있을 경우 클래스보단 모듈을 권장합니다. 클래스는 해당 클래스로부터 인스턴스를
  생성하는게 적절할 때만 사용해야 합니다.

* `module_function`보단 `extend self`를 권장합니다.

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
  클래스 메서드를 모으세요.

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

* 사소한 접근자와 설정자를 정의할 때 `attr_accessor`, `attr_reader`, `attr_writer`를 사용하세요.

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

* `attr`보단 `attr_reader`와 `attr_accessor`를 권장합니다.

* 클래스(`@@`) 변수 사용을 피하세요.

* `public`, `protect`, `private`는 그 메서드 정의와 같은 깊이 만큼 들여쓰기를 하세요. 가시성을
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

* `alias`보다 `alias_method`를 권장합니다.

## Exceptions

* Signal exceptions using the `raise` method.

* Omit `RuntimeError` in the two argument version of `raise`.

  ~~~ ruby
  # bad
  raise RuntimeError, "message"

  # good - signals a RuntimeError by default
  raise "message"
  ~~~

* Prefer supplying an exception class and a message as two separate arguments to
  `raise` instead of an exception instance.

  ~~~ ruby
  # bad
  raise SomeException.new("message")
  # Note that there is no way to do `raise SomeException.new("message"), backtrace`.

  # good
  raise SomeException, "message"
  # Consistent with `raise SomeException, "message", backtrace`.
  ~~~

* Avoid returning from an `ensure` block. If you explicitly return from a method
  inside an `ensure` block, the return will take precedence over any exception
  being raised, and the method will return as if no exception had been raised at
  all. In effect, the exception will be silently thrown away.

  ~~~ ruby
  # bad
  def foo
    raise
  ensure
    return "very bad idea"
  end
  ~~~

* Use implicit begin blocks where possible.

  ~~~ ruby
  # bad
  def foo
    begin
      # main logic goes here
    rescue
      # failure handling goes here
    end
  end

  # good
  def foo
    # main logic goes here
  rescue
    # failure handling goes here
  end
  ~~~

* Avoid empty `rescue` statements.

  ~~~ ruby
  # bad
  begin
    # an exception occurs here
  rescue SomeError
    # the rescue clause does absolutely nothing
  end

  # bad - `rescue nil` swallows all errors, including syntax errors, and
  # makes them hard to track down.
  do_something rescue nil
  ~~~

* Avoid `rescue` in its modifier form.

  ~~~ ruby
  # bad - this catches exceptions of StandardError class and its descendant
  # classes.
  read_file rescue handle_error($!)

  # good - this catches only the exceptions of Errno::ENOENT class and its
  # descendant classes.
  def foo
    read_file
  rescue Errno::ENOENT => error
    handle_error(error)
  end
  ~~~

* Avoid rescuing the `Exception` class.

  ~~~ ruby
  # bad
  begin
    # calls to exit and kill signals will be caught (except kill -9)
    exit
  rescue Exception
    puts "you didn't really want to exit, right?"
    # exception handling
  end

  # good
  begin
    # a blind rescue rescues from StandardError, not Exception.
  rescue => error
    # exception handling
  end
  ~~~

* Prefer exceptions from the standard library over introducing new exception
  classes.

* Use meaningful names for exception variables.

  ~~~ ruby
  # bad
  begin
    # an exception occurs here
  rescue => e
    # exception handling
  end

  # good
  begin
    # an exception occurs here
  rescue => error
    # exception handling
  end
  ~~~

## Collections

* Use literal array and hash creation notation unless you need to pass
  parameters to their constructors.

  ~~~ ruby
  # bad
  arr = Array.new
  hash = Hash.new

  # good
  arr = []
  hash = {}
  ~~~

* Prefer the literal array syntax over `%w` or `%i`.

  ~~~ ruby
  # bad
  STATES = %w(draft open closed)

  # good
  STATES = ["draft", "open", "closed"]
  ~~~

* Append a trailing comma in multi-line collection literals.

  ~~~ ruby
  # bad
  {
    foo: :bar,
    baz: :toto
  }

  # good
  {
    foo: :bar,
    baz: :toto,
  }
  ~~~

* When accessing the first or last element from an array, prefer `first` or
  `last` over `[0]` or `[-1]`.

* Avoid mutable objects as hash keys.

* Use shorthand hash literal syntax when all keys are symbols.

  ~~~ ruby
  # bad
  { :a => 1, :b => 2 }

  # good
  { a: 1, b: 2 }
  ~~~

* Prefer hash rockets syntax over shorthand syntax when not all keys are
  symbols.

  ~~~ ruby
  # bad
  { a: 1, "b" => 2 }

  # good
  { :a => 1, "b" => 2 }
  ~~~

* Prefer `Hash#key?` over `Hash#has_key?`.

* Prefer `Hash#value?` over `Hash#has_value?`.

* Use `Hash#fetch` when dealing with hash keys that should be present.

  ~~~ ruby
  heroes = { batman: "Bruce Wayne", superman: "Clark Kent" }
  # bad - if we make a mistake we might not spot it right away
  heroes[:batman] # => "Bruce Wayne"
  heroes[:supermann] # => nil

  # good - fetch raises a KeyError making the problem obvious
  heroes.fetch(:supermann)
  ~~~

* Introduce default values for hash keys via `Hash#fetch` as opposed to using
  custom logic.

  ~~~ ruby
  batman = { name: "Bruce Wayne", is_evil: false }

  # bad - if we just use || operator with falsy value we won't get the expected result
  batman[:is_evil] || true # => true

  # good - fetch work correctly with falsy values
  batman.fetch(:is_evil, true) # => false
  ~~~

* Place `]` and `}` on the line after the last element when opening
  brace is on a separate line from the first element.

  ~~~ ruby
  # bad
  [
    1,
    2]

  {
    a: 1,
    b: 2}

  # good
  [
    1,
    2,
  ]

  {
    a: 1,
    b: 2,
  }
  ~~~

## Strings

* Prefer string interpolation and string formatting instead of string
  concatenation:

  ~~~ ruby
  # bad
  email_with_name = user.name + " <" + user.email + ">"

  # good
  email_with_name = "#{user.name} <#{user.email}>"

  # good
  email_with_name = format("%s <%s>", user.name, user.email)
  ~~~

* Avoid padded-spacing inside braces in interpolated expressions.

  ~~~ ruby
  # bad
  "From: #{ user.first_name }, #{ user.last_name }"

  # good
  "From: #{user.first_name}, #{user.last_name}"
  ~~~

* Use double-quoted strings.

  ~~~ ruby
  # bad
  'Just some text'
  'No special chars or interpolation'

  # good
  "Just some text"
  "No special chars or interpolation"
  "Every string in #{project} uses double_quotes"
  ~~~

* Avoid the character literal syntax `?x`.

* Use `{}` around instance and global variables being interpolated into a
  string.

  ~~~ ruby
  class Person
    attr_reader :first_name, :last_name

    def initialize(first_name, last_name)
      @first_name = first_name
      @last_name = last_name
    end

    # bad - valid, but awkward
    def to_s
      "#@first_name #@last_name"
    end

    # good
    def to_s
      "#{@first_name} #{@last_name}"
    end
  end

  $global = 0
  # bad
  puts "$global = #$global"

  # fine, but don't use globals
  puts "$global = #{$global}"
  ~~~

* Avoid `Object#to_s` on interpolated objects.

  ~~~ ruby
  # bad
  message = "This is the #{result.to_s}."

  # good - `result.to_s` is called implicitly.
  message = "This is the #{result}."
  ~~~

* Avoid `String#gsub` in scenarios in which you can use a faster more
  specialized alternative.

  ~~~ ruby
  url = "http://example.com"
  str = "lisp-case-rules"

  # bad
  url.gsub("http://", "https://")
  str.gsub("-", "_")
  str.gsub(/[aeiou]/, "")

  # good
  url.sub("http://", "https://")
  str.tr("-", "_")
  str.delete("aeiou")
  ~~~

* When using heredocs for multi-line strings keep in mind the fact that they
  preserve leading whitespace. It's a good practice to employ some margin based
  on which to trim the excessive whitespace.

  ~~~ ruby
  code = <<-END.gsub(/^\s+\|/, "")
    |def test
    |  some_method
    |  other_method
    |end
  END
  # => "def test\n  some_method\n  other_method\nend\n"

  # In Rails you can use `#strip_heredoc` to achieve the same result
  code = <<-END.strip_heredoc
    def test
      some_method
      other_method
    end
  END
  # => "def test\n  some_method\n  other_method\nend\n"
  ~~~

* In Ruby 2.3, prefer ["squiggly
  heredoc"](https://github.com/ruby/ruby/pull/878) syntax, which has the same
  semantics as `strip_heredoc` from Rails:

  ~~~ruby
  code = <<~END
    def test
      some_method
      other_method
    end
  END
  # => "def test\n  some_method\n  other_method\nend\n"
  ~~~

* Indent heredoc contents and closing according to its opening.

  ~~~ruby
  # bad
  class Foo
    def bar
      <<~SQL
        'Hi'
    SQL
    end
  end

  # good
  class Foo
    def bar
      <<~SQL
        'Hi'
      SQL
    end
  end

  # bad

  # heredoc contents is before closing heredoc.
  foo arg,
      <<~EOS
    Hi
      EOS

  # good
  foo arg,
      <<~EOS
    Hi
  EOS

  # good
  foo arg,
    <<~EOS
      Hi
    EOS
  ~~~

## Regular Expressions

* Prefer plain text search over regular expressions in strings.

  ~~~ ruby
  string["text"]
  ~~~

* Use non-capturing groups when you don't use the captured result.

  ~~~ ruby
  # bad
  /(first|second)/

  # good
  /(?:first|second)/
  ~~~

* Prefer `Regexp#match` over Perl-legacy variables to capture group matches.

  ~~~ ruby
  # bad
  /(regexp)/ =~ string
  process $1

  # good
  /(regexp)/.match(string)[1]
  ~~~

* Prefer named groups over numbered groups.

  ~~~ ruby
  # bad
  /(regexp)/ =~ string
  ...
  process Regexp.last_match(1)

  # good
  /(?<meaningful_var>regexp)/ =~ string
  ...
  process meaningful_var
  ~~~

* Prefer `\A` and `\z` over `^` and `$` when matching strings from start to end.

  ~~~ ruby
  string = "some injection\nusername"
  string[/^username$/] # `^` and `$` matches start and end of lines.
  string[/\Ausername\z/] # `\A` and `\z` matches start and end of strings.
  ~~~

## Percent Literals

* Use `%()` for single-line strings which require both interpolation and
  embedded double-quotes. For multi-line strings, prefer heredocs.

* Avoid `%q` unless you have a string with both `'` and `"` in it. Regular
  string literals are more readable and should be preferred unless a lot of
  characters would have to be escaped in them.

* Use `%r` only for regular expressions matching at least one `/` character.

  ~~~ ruby
  # bad
  %r{\s+}

  # good
  %r{^/(.*)$}
  %r{^/blog/2011/(.*)$}
  ~~~

* Avoid the use of `%s`. Use `:"some string"` to create a symbol with spaces in
  it.

* Prefer `()` as delimiters for all `%` literals, except, as often occurs in
  regular expressions, when parentheses appear inside the literal. Use the first
  of `()`, `{}`, `[]`, `<>` which does not appear inside the literal.

## Testing

* Treat test code like any other code you write. This means: keep readability,
  maintainability, complexity, etc. in mind.

* Prefer Minitest as the test framework.

* Limit each test case to cover a single aspect of your code.

* Organize the setup, action, and assertion sections of the test case into
  paragraphs separated by empty lines.

  ~~~ ruby
  test "sending a password reset email clears the password hash and set a reset token" do
    user = User.create!(email: "bob@example.com")
    user.mark_as_verified

    user.send_password_reset_email

    assert_nil user.password_hash
    refute_nil user.reset_token
  end
  ~~~

* Split complex test cases into multiple simpler tests that test functionality
  in isolation.

* Prefer using `test "foo"`-style syntax to define test cases over `def
  test_foo`.

* Prefer using assertion methods that will yield a more descriptive error
  message.

  ~~~ ruby
  # bad
  assert user.valid?
  assert user.name == "tobi"


  # good
  assert_predicate user, :valid?
  assert_equal "tobi", user.name
  ~~~

* Avoid using `assert_nothing_raised`. Use a positive assertion instead.

* Prefer using assertions over expectations. Expectations lead to more brittle
  tests, especially in combination with singleton objects.

  ~~~ ruby
  # bad
  StatsD.expects(:increment).with("metric")
  do_something

  # good
  assert_statsd_increment("metric") do
    do_something
  end
  ~~~
